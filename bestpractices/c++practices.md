---
layout: default
---

# C++ practices

Most rules presented here cover the same things as the very good [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines).
Whenever something is not covered or you are in doubt - don't hesitate to consult it - it is a very good source of information.

## Avoid anonymous namespaces

Anonymous namespaces are very convenient, but makes code unreachable by tests
and other code.
Some code might only make sense in a given context, but if the functionality
is generic, it could be put in a library or suchlike.

Use a *named namespace* to house free functions rather than relying on a class or struct filled with static functions (or functions that should be static).
This approach ensures better organization and clarity.

In addition, private static functions do not need to be declared in the header file when moved out of a class.
This reduces unnecessary exposure of implementation details.

## Algorithms and data structures

> Algorithms + Data Structures = Programs
--- Niklaus Wirth, 1976

> STL algorithms say what they do, as opposed to hand-made for loops that just show how they are implemented. By doing this, STL algorithms are a way to rise the level of abstraction of the code, to match the one of your calling site.
--- Jonathan Boccara, 2016

> Debugging code is twice as hard as writing the code in the first place. Therefore, if you write code as cleverly as possible, you are, by definition not smart enough to debug it.
--- Brian W. Kernighan

Data is information, facts etc. An algorithm is code that operates on data.

Programming languages, or their libraries, include thoroughly tested algorithms to handle common data structures.

By properly considering algorithms and data structure, and keeping data separate from code, both code and data become simpler, more reliable, more flexible, and easier to maintain for the next person.

Raw loops are those starting with `for`, `while` etc. While there are many options on how to write a loop, readabillity and maintainabillity should be the priority.

```cpp
// Pre-C++11:
for(std::map<KeyType, ValueType>::iterator it = itemsMap.begin(); it != itemsMap.end(); ++it) {
	//...
    doSomething(*it);
	//...
}

// C++11
for(auto item : itemsMap) {
	//...
	doSomething(item);
	//...
}

// C++17
for(auto [name, value] : itemsMap) {
	//...
	doSomething(name, value);
	//...
}
```

Another way, which can be even better, is to use STL `<algorithm>` library, which offers a wealth of proven, declarative solutions, e.g.:
```cpp
// C++17
std::for_each(stuff.begin(), stuff.end(), do_something);
auto result = std::find_if(stuff.begin(), stuff.end(), do_something);

// C++20
std::ranges::for_each(stuff, do_something);
auto result = std::ranges::find_if(stuff, do_something);
```

Note, STL `<algorithm>` library can also use lambdas.


**Example**: For a given input, find the appropriate prefix.
```cpp
constexpr std::array<std::string_view, 5> prefixes {"", "k", "M", "G", "T"};
size_t base = 0;
double inUnits = bytes;
constexpr double siFactor {1000.0};

while (inUnits > siFactor && base < prefixes.size() - 1) {
	++base;
	inUnits /= siFactor;
}

auto prefix = prefixes[base];
```

Let’s make the data more expressive, more self-contained, and use STL
algorithm `find_if`:

```cpp
using PrefixSpec = struct {
	char prefix;
	unsigned long long factor;
};

static constexpr std::array<PrefixSpec, 7> sortedPrefixes {
	{{'E', 1ULL << 60}, // 1 << 60 = 2^60
	{'P', 1ULL << 50},
	{'T', 1ULL << 40},
	{'G', 1ULL << 30},
	{'M', 1ULL << 20},  // 1 << 20 = 2^20 = 1024
	{'k', 1ULL << 10},  // 1 << 10 = 2^10 = 1024
	{'\0', 0}}};

const auto res = std::find_if(prefixes.begin(), prefixes.end(), [&](const auto& spec) {
	return spec.factor <= size;
});

// Add one digit after the decimal place for all prefixed sizes
return res->factor
	? fmt::format("{:.1f} {}B", static_cast<double>(size) / res->factor, res->prefix)
	: fmt::format("{} B", size);
```

Simpler, cleaner, more reliable. No raw loops, magic numbers or calculations.
Note the reverse iterator.

## Code comments

> Don’t comment bad code—rewrite it.
--- Brian W. Kernighan and P. J. Plaugher, 1974

Comments are a piece of the program that the computer doesn't execute. While the intension is to aid comprehension, they have a tendency to get out-of-sync with the actual code it is commenting.

It is preferred that code is self documenting when possible. This can be achieved using good naming and structure.

In some cases, comments can be necessary, to convey information that cannot be described in code. This can be links to bugs that a workaround describe or why edge cases are needed.

## Conditionals

Every branch in code doubles the number of paths and makes the code difficult to debug and maintain.

Simple `if else` may be better expressed as a ternary `_ : _ ? _`. There are often ways to avoid `else`, when possible the resulting code will be better without. Ternary is ok though.

Even modestly complex code in an `if` or `else` branch should be
extracted to a function or lambda.

A sequence of conditionals stepping through related variables may indicate
code and data conflation.

Can also apply to `switch` statements.

Complicated `if else` code might benefit from converting to a state machine.

### Ternary operator
You can reduce six lines:
```cpp
int r; // can’t be const

if (x == 2) {
	r = 2;
} else {
	r = 3;
}
```
to one, with a single assignment, no curly braces, no repetition, and
const option:
```cpp
const int r = x == 2 ? 2 : 3;
```
This works especially great for simplifying return statements.

If you have more than one ternary it might be worth to extract function that encapsulates the conditional logic:
```
auto someConditionalLogic = []() {
	if (condition1) {
		return 1;
	} else if (condition2) {
		return 2;
	} else {
		return 3;
	}
}

const int r = someConditionalLogic();
```

## Const correctness

> const all of the things.
--- Jason Turner, 2021

`const` is a statement of intent that something is to be immutable
(not able to change). Immutability aids reliability and is done using `constexpr`

This provides compile-time evaluation. Can increase compile time, but speedup runtime. More errors are caught at compile-time.

`constexpr` is preferable for everything that can be so represented.

## Reducing dependencies

> Any source code dependency, no matter where it is, can be inverted
--- Robert C. (Uncle Bob) Martin

Hard dependencies makes the codebase more entangled, makes changes more difficult, and makes unit testing really difficult.

Three examples of dependencies creeping in:
```cpp
Application::getSomething(); // or any other singleton
SomeDistantClass thing;
void method(AnotherDistantClass values){}
```

This does not stand in contrast to code reuse, but it does require care when designing how code is accessing data.

A function which has hard dependencies cannot function, be understood, edited or tested, without the context of its dependencies. Avoiding these types of dependencies without code duplication is worth striving for.

Code and its dependencies are said to be _coupled._ When different pieces of code _have the same_ dependency, they in turn are coupled to each other.

Required information can be injected via constructor or method parameters.

If it is necessary to introduce external code (e.g. a service object), do so by passing an interface, helper function or similar to avoid coupling.

Even in complex cases where singletons are used we can avoid hard coupling and make unit test a breeze.
Example:
```c++
// For this example the implementation is included in the class definition
// to simplify the example.
class Example {
public:
    using DependencyHelper std::function<std::unique_ptr<Dependency>()>;
    Example(
        //...
        DependencyHelper provide_helper_ = {}
    ) : provide_helper_(std::move(provide_helper))
    {
        if (!provide_helper_) {
            provide_helper_ = my_namespace::defaultDependencyHelper();
        }
    };
private:
    DependencyHelper provide_helper_;
}
```

Ideally, all dependencies can be avoided, what dependencies to keep depends on the situation.

If your code needs external data that was not available when calling this
code, then there's likely a better overall design that can be used.

**Code that has no hard dependencies is single-purpose, reusable, changeable, and testable. Everything is simpler!**

## Code design

> Any fool can write code that a computer can understand. Good programmers write code that humans can understand
--- Martin Fowler

Something well-designed is _instantly_ understandable, and a pleasure to work
with. Programming principles developed over the last 50 years ensure
well-designed code not only runs well, but is also understandable by humans.

Well-designed code is adaptable, flexible, easily changed to suite new
circumstances.

Well-designed code is completely free from hard-coded data.

Understandable code can be more easily evaluated for correctness.

For a novice programmer many of these concepts are probably quite foreign, but with a little study and help from the community and code reviews, better code will ensue.

---

## Enums

Used correctly, enums are invaluable.

Using enums:
* ...specifically `enum class` allows strongly typed and scoped enumeration to be created.
* ...instead of booleans for function arguments makes it easy to understand what the argument means without consulting the documentation or at least the method signature.
* ...instead of integers when expressing anything other than numbers.

Using enums to codify data values is strongly discouraged, as enums are best suited for representing fixed, intrinsic states rather than variable data. Instead, use `std::unordered_map<>` or other data structures that better represent dynamic or data-driven values, offering flexibility and improving maintainability.

## Minimize getters and setters

In object-oriented design, a class should encapsulate behavior, not just data. **Frequent use of getters and setters can limit a class’s ability to fully encapsulate its responsibilities** and may suggest that the data could be handled differently.

Consider:

* Using a struct for simple data containers with public fields.
* Focusing on methods that represent meaningful actions rather than exposing raw data.

A well-designed class manages its own state and provides behavior, not just access.

## Appropriate typing

Using strings for everything can make code harder to understand and maintain. Use appropriate types to add clarity and structure.

## Main code path and indentation

> If you are past three indents you are basically screwed.
> Time to rewrite.
--- Linus Torvalds, 1995

Indented code can be diﬃcult to reason about, and fragile.

Main execution path should be the least indented one, i.e. conditions should cover specific cases. Early-Exit should be preferred to prune unwanted execution branches fast.

Example:
```cpp
if (something) {
	doSomething();
	if (somethingElse) {
		doSomethingElse()
		if (somethingElseAgain) {
			doThing();
		} else {
			doDifferent();
		}
	} else {
		doTheOther();
	}
} else {
	doNothing();
}
```

Can be changed into:
```cpp
if (!something) {
	doNothing();
	return;
}
doSomething();
if (!somethingElse) {
	doTheOther();
	return;
}
doSomethingElse();
if (!somethingElseAgain) {
	doDifferent();
	return;
}
doThing();
```

## Initialization

**Initialize all objects, and, if possible, use const or better still, constexpr.**

Avoid default constructors. If there is not yet a value for an object, then there's no need to create it. Declare variables close to where they are used (there's no need to start the declare variables at the start of the block like in ansi-c). Joining declaration and initialization allows const:

```cpp
AType thing; // mutable. Does it have a default constructor?
const AType thing3 { calcVal() }; // immutable
```

Using uninitialized POD type variables is undefined behavior.

It _is_ OK to declare variables inside a loop.

Prefer uniform initialization { } (also called brace initialization)(since C++11). Prevents narrowing.

Clarifies not assignment (=) or function call (()).

Initialize class member variables at declaration, not in constructors (since C++11):
- Simplifies constructors
- Avoids repetition
- Establishes default state

Static members can be defined in header file, no need to split to source file:
```cpp
struct Something {
	static const int _value_ = 2; // since C++98 for int
	static inline std::string _something_ {"str"}; // since C++17
}
```
Lambdas can create and initialize variables in the capture, removing the
need to have the parameter in the surrounding scope. But don’t forget
mutable if you want to update its value. The variable stays for the lifetime
of the lambda (think static, but better).

## Lambdas

> One of the most popular features of Modern C++.
--- Jonathan Boccara, 2021

A lambda (since C++11) is like a function that can be named, passed into and
out of functions. They accept parameters, can be generic (since C++ 14), do a
really good job of type deduction (auto), and can be constexpr (since C++17).

Lambdas can capture data from enclosing scopes and enforce encapsulation, simplifying surrounding scopes.

Lambdas are indispensable when breaking up complex code into individual
responsibilities, perhaps as a precursor to moving to functions. Ditto
when removing repetition.

Whilst lambdas are quite happy with simple auto parameters, best not
to omit const or & as appropriate. Types may be required to help the IDE.

Consider the following code:
```c++
doSomething()
if (somethingWentWrong()) {
    // Clean up this
    // ...
    // Clean up that
    return;
}
doSomethingElse();
if (somethingElseWentWrong()) {
    // Clean up this
    // ...
    // Clean up that
    return;
}
doSomeOtherThing();
// Clean up this
// ...
// Clean up that
```
Using lambdas we can remove code duplication to create the following:
```c++
auto cleanup = [](){
    // Clean up this
    // ...
    // Clean up that
};

doSomething()
if (somethingWentWrong()) {
    cleanup();
    return;
}
doSomethingElse();
if (somethingElseWentWrong()) {
    cleanup();
    return;
}
doSomeOtherThing();
cleanup();
```


## Avoid macros

---

While macros was needed back in the days. Today, with modern c++, that's usually not the case anymore.

Macros are global, can be lead to unpredicted side effects and are difficult to debug. Consider replacing with a function. For conditional compilation in
templates consider constexpr if.

## Avoid magic literals

“Magic” literals placed directly in code offer no clue as to what exactly
is being specified or its origin, and no clue if the same data is used
elsewhere. Comprehension and maintenance burden.

To document what the magic literal is, use a suitable named constant.

Instead of this:
```cpp
displayLines(80);
```

Do the following instead:
```c++
constexpr auto standardScreenLength {80};
displayLines(standardScreenLength);
```

## Good naming

**Clear, concise, naming makes code understandable**.

For an object whose purpose is to _do_ something (service object), prefer a verb. E.g. `renderer`.

For an object that _is_ something (value object), prefer a noun. E.g. `drawing`.

Something diﬃcult to name concisely likely does not have a single purpose and needs refactoring.

Use names that are specific. E.g. `SaveLogToDisk`, not `ProcessLog`. `Process` could be anything.

A variable named after its data value defeats the whole point of a variable:
```cpp
struct Dog {
	std::string color {};
};
Dog redDog {"blue"}; // BAD
// 200 lines later, -_obviously_\- redDog is red! He’s blue? WTF?
Dog dog {"pink"}; // OK
constexpr auto redDogColor {"red"}; // OK
```
See also variable sets.

# Out parameters

Out parameters are _non-const, by-reference, or by-pointer_, function parameters. These are known to cause hard to find bugs.

Whether values are updated by the function is not obvious.

**Where possible make function parameters `const` or `const&` and return a value, or return a container to return multiple values**.

Return value optimizations (RVO) simplifies return and usually elides copies.
```cpp
auto func = [](const auto& str, const auto num) {
	return { str, num };
};
```
Structured binding (since C++17) simplifies reading back the result at the calling side:
```cpp
auto [name, value] = func("qty", 2);
```

For methods that return multiple values, depending on context, it may be better to provide dedicated struct for result.

## Code repetition

> One of the things I've been trying to do is look for simpler or rules underpinning good or bad design. I think one of the most valuable rules is to avoid duplication
--- Martin Fowler

> Code duplication is by far one of the worst anti-patterns in software
> engineering, eventually leading to buggy and unmaintainable systems.
--- Magnus Stuhr, 2020

Reducing code duplication and similar code also reduces the different paths a program might take during runtime. This in turn makes it easier to debug and test the code base.

Change requires finding every usage (difficult) and replicating the change (error-prone). Failure to catch just one instance creates a nasty bug that might remain undiscovered for a long time. Comprehension requires studying every item. Small differences are notoriously difficult to spot.

**Repetition is entirely avoidable.**

The variant part (the bit that is different between usages) of repeating code is often just one or two items in a long chain of statement.
The variant parts can be extracted and passed as parameters to a function or lambda executing the common body. A sequence of repeated code likely indicates the underlying data is actually a set and hence should be defined in a container and dealt with accordingly.

## Static

Often best avoided. For `const` variables consider `constexpr`, or
initialisation in lambda capture.

`static` functions _may_ be better moved out of class into a named namespace or some utility library/file.


## Automated testing

> Nothing makes a system more flexible than a suite of tests. Nothing. Good architecture and design are important, but the effect of a robust suite of tests is an order of magnitude greater. It’s so much greater because those tests enable you to improve the design.
--- Uncle Bob

There are multiple types of automated testing. Regression tests, integration tests. unit tests etc. While there are similarities the idea behind them differs. These are different tools in the toolbox we can use to ensure good quality code.

* **Regression tests** when bugs are found, tests are created to ensure that the found bug is 1. fixed, and 2. does not happen again.
* **Integration tests** ensures that multiple components can work together to complete a task.
* **Unit tests** targets a single behavior of a single unit of code, with a single assertion.

Writing tests on a coupled design is difficult, therefore removing hard dependencies is not just an academic exercise, but it is a great tool to make code testable.

Test driven development, where tests are written before, and during code creation, is a good practice to follow to ensure good quality code. Writing tests afterwards can be difficult as it won't help you with code design as much when code is produced.

Gui code is often classified as untestable because there are good tools to verify behavior when buttons and sliders are manipulated. And while gui code can't easily be tested, the logic it is connected to, can. Splitting ui from from "business logic" is a good choice and highly recommended.

Just like good naming is important for variables and functions, naming tests are too. A good test name describes what has been tested. This way, when it fails for someone else, they can easily see what failed.

If code is difficult to unit test consider if it can be extracted from its surrounds — maybe into some library-like class — where it can be tested in isolation. Is it possible to refactor to remove hard-wired dependencies? Is it trying to do too much? Is it too tightly coupled? See also dependencies.

---

## Avoid variable sets

Representing data with code where related variables have names closely coupled to their initial value should be avoided to reduce repetition and make the code easier to understand and maintain.

For example, instead of this:
```cpp
Item fred { "Fred", 20 };
Item martha { "Martha", 30 };
Item george { "George", 40 };
```

Move the data into a container e.g. `constexpr std::array`, not a vector or map.

Container elements are typically value, array, pair, tuple, or a defined struct.
Pair and tuple should only be used for very short lived data, such as this case:

```cpp
using Pair = std::pair<std::string_view, size_t>;

constexpr auto numItems {3};

constexpr std::array<Pair, numItems> items {{
	{ "Fred", 20 },
	{ "Martha", 30 },
	{ "George", 40 }
}};
```

A struct can also be used, which has the advantage of named elements, but is slightly more overhead.

```cpp
struct Button {
	std::string_view name;
	size_t height;
	size_t width;
};

constexpr auto numButtons {3};

constexpr std::array<Button, numButtons> buttonDefs {{
	{ "Go", 25, 25 },
	{ "Get set", 20, 20 },
	{ "On your marks", 15, 15 }
}};
```

When in doubt, use a struct - it is better to have good names than not.
