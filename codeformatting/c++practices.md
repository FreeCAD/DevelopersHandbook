---
layout: default
---

# C++ coding practices

Although this chapter is not FreeCAD specific, it is provided here in hope
it will help to keep clean and easily maintainable code.

## anonymous namespace 👎

Anonymous namespaces are very convenient, but how are you going to unit test?
Maybe the enclosed code is reusable if in a library or suchlike?

Use a named namespace to house free functions rather than a class or struct
full of static (or what should be static) functions — idiomatic C++.

## algorithms + data structures = programs 👍

> Algorithms + Data Structures = Programs
--- Niklaus Wirth, 1976

> STL algorithms say what they do, as opposed to hand-made for loops that
> just show how they are implemented. By doing this, STL algorithms are a way
> to rise the level of abstraction of the code, to match the one of your
> calling site.
--- Jonathan Boccara, 2016

> No raw loops.
--- Sean Parent, 2013

Data is information, facts etc. An algorithm is code that operates on data.

Programming languages, or their libraries, include thoroughly tested algorithms
to handle common data structures.

**By properly considering data structure, and keeping data separate from code,
both code and data become simpler, more reliable, more flexible, and easier
to maintain.**

Raw loops are those starting with for, while etc. Pre-C++11 they were messy
and error-prone. C++11 added the range-based for loop which was a vast improvement:

```cpp
for(auto item : itemsMap) {
	doSomething(item);
}
```

C++17 added structured binding:
```cpp
for(auto [name, value] : itemsMap) {
	doSomething(name, value);
}
```

Which is great, up to a point. STL `<algorithm>` library offers a wealth
of proven, declarative solutions, e.g.:
```cpp
std::for_each(stuff.begin(), stuff.end(), functionThatDoesTheWork);
```
loops through stuff, sending each item in turn to the named function.

Benefits include:
- Can use just parts of a container
- Can do reverse iteration (`rbegin, rend`)
- Use a lambda, function or method name, or a lambda definition
- Someone else has done the debugging
- Can have execution policy for parallelism
- Can be one-liner
- Work with all types that have `for_each` defined, plus you can create your own
- C++20 adds ranges, and heaps more algorithms

Example: For a given input, find the appropriate prefix.

One possible solution:
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
{% raw %}
```cpp
using Pair = std::pair<std::string_view, double>;
constexpr std::array<Pair, 5> prefs {{
	{ "", 0 },
	{ "k", 1e3 },
	{ "M", 1e6 },
	{ "G", 1e9 },
	{ "T", 1e12 },
}};

auto comp = [&](auto pair) { return pair.second <= input; };

const auto spec = std::find_if(prefs.rbegin(), prefs.rend(), comp);

auto prefix = spec->first;
```
{% endraw %}

Simpler, cleaner, more reliable. No raw loops, magic numbers or calculations.
Note the reverse iterator.

## comment 👎

> The only truly good comment is the comment you found a way not to write.
--- Robert C. (Uncle Bob) Martin, 2016

> Don’t comment bad code—rewrite it.
--- Brian W. Kernighan and P. J. Plaugher, 1974

Comments are supposed to aid comprehension, but are often a nuisance. They have
a tendency to become stale, out-of-date. Well named functions and variables
obviate the need for comments. Worrying about the cost of function calls is more
than likely premature optimization. Sometimes it is helpful if a class etc is
prefaced by an explanatory comment.

**Above all, write code that explains itself.**

Remove:
- Comments that attempt to workaround bad naming. Fix the naming
- Comments that are frivolous, or repeat what code says
- Large comments from inside class or function. Place ahead of the class or function

## conditional 👎

**Every branch in code doubles the number of paths. Increases complexity,
maintenance burden**.

Conditionals create indentation, and often violate the “Tell-Don’t-Ask”
principle. Conditionals may indicate laziness, naive structure, not using
polymorphism, design patterns etc.

Simple `if else` may be better expressed as a ternary. `else` is rarely
necessary. Code will be better without it. Ternary is ok though.

Even modestly complex code in an `if` or `else` branch should be
extracted to a function or lambda.

A sequence of conditionals stepping through related variables may indicate
code and data conflation.

Can also apply to `switch` statements. See also: repetition, variable sets, raw loops.

Complicated `if else` code might benefit from converting to state machine.

One school of thought says a function should have no more than one conditional,
which should be the first statement!

## const 👍

> const all of the things.
--- Jason Turner, 2021

`const` is a statement of intent that something is to be immutable
(not able to change). Immutability aids reliability. In Rust immutability
is the default.

**In C++ immutability must be added manually using**

## constexpr 👍

Provides compile-time evaluation. Can increase compile time, but speedup runtime.
More errors are caught at compile-time.

`constexpr` is preferable for everything that can be so represented.

In class scope, qualify `constexpr` with `static` . This does not apply
in namespace scope. `const` qualification of `constexpr` is redundant.
It is `const` already.

DO NOT make `constexpr` variable names UPPERCASE. Leave that for macros.
Unlike a macro, `constexpr` is not global. A `constexpr` definition can
be hoisted to a higher scope to expand applicability.

## dependencies 👎

> Any source code dependency, no matter where it is, can be inverted
--- Robert C. (Uncle Bob) Martin

A car needs wheels. But should a wheel know anything about a car? _Of course not!_

Examples of dependencies creeping in:
```cpp
Application::getSomething(); // possibly dreaded singleton
SomeDistantClass fing;
void method(AnotherDistantClass values){}
```

Code cannot function, be understood, be edited or tested, without
its dependencies. So code free from dependencies is worth striving for.
Code and its dependencies are said to be _coupled._

When different pieces of code _have the same_ dependency, they in turn
are coupled to each other!

Everything affects everything else. **Spaghetti Code!**

Ideally, eliminate dependencies (stdlib etc don’t count).

Required information can be injected via constructor or method parameters. “_Tell-Don’t-Ask_”.

If it is necessary to introduce external code (e.g. a service object), do so via an interface.

If your code needs external data that was not available when calling this
code, then likely there is a serious flaw in the overall design.

**Code that has no hard dependencies is single-purpose, reusable, changeable,
and testable. Everything is simpler!**

## design 👍

> Any fool can write code that a computer can understand. Good programmers
write code that humans can understand
--- Martin Fowler

Something well-designed is _instantly_ understandable, and a pleasure to work
with. Programming principles developed over the last 50 years ensure
well-designed code not only runs well, but is also understandable by humans.

Well-designed code is adaptable, flexible, easily changed to suite new
circumstances.

Well-designed code is completely free from hard-coded data.

Understandable code can be more easily evaluated for correctness.

For a novice programmer many of these concepts are probably incomprehensible,
but with a little study and a determination to understand established
principles, better code will ensue.

## enum 👍 👎

Used correctly, enums are invaluable.

**BUT! Enums that codify _data values_ are a significant code smell!**

See also algorithms + data structures = programs.

## getter, setter 👎

Why encapsulate stuff in a class in true OO style, and then expose it?
Maybe those variables belong in a struct (def public)?

**Getters and setters that act directly on class member variables are a significant code smell**.

A class with lots of getters and setters is likely fundamentally flawed OO design.

## inappropriate type 👎

When everything is a string it is diffcult to understand what’s what.
Apply suitable abstraction.

## indentation 👎

> If you are past three indents you are basically screwed.
> Time to rewrite.
--- Linus Torvalds, 1995

Indented code can be diﬃcult to reason about, and fragile:
```cpp
if (sumfing) {
	doSumfing();
	if (sumfingElse) {
		doSumfingElse()
		if (sumfingElseAgain) {
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
Early returns help (the single return principle died long ago):
```cpp
if (!sumfing) {
	doNothing();
	return;
}
doSumfing();
if (!sumfingElse) {
	doTheOther();
	return;
}
doSumfingElse();
if (!sumfingElseAgain) {
	doDifferent();
	return;
}
doThing();
```
Much better! Each block could be a function. Functions names mean
comments not required:
```cpp
if (checkFing() && checkElse() && checkAgain()) {
	doThing()
}
```
Or all rolled into a single function:
```cpp
if (fingsValid()) {
	doThing();
}
```
Intent is clear, easy to read, simple.

## initialization 👍

**Initialize all objects, and, if possible, make const (or better still, constexpr).**

Avoid default constructors. If there is not yet a value for an object,
then don’t create it! Declare variables where they are used (not at
the start of the block like last century C). Joining declaration and
definition allows const:
```cpp
AnType thing; // mutable. Does it even have a default constructor?
const AnType thing3 { calcVal() }; // immutable
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
struct sumFing {
	static const int _value_ = 2; // since C++98 for int
	static inline std::string _sumFing_ {"str"}; // since C++17
}
```
Lambdas can create and initialize variables in the capture, removing the
need to have the parameter in the surrounding scope. But don’t forget
mutable if you want to update its value. The variable stays for the lifetime
of the lambda (think static, but better).

## lambda 👍

> One of the most popular features of Modern C++.
--- Jonathan Boccara, 2021

A lambda (since C++11) is like a function that can be named, passed into and
out of functions. They accept parameters, can be generic (since C++ 14), do a
really good job of type deduction (auto), can be constexpr (since C++17).

Lambdas can capture data from enclosing scopes.

Lambdas enforce encapsulation, simplifying surrounding scopes.

Lambdas are indispensable when breaking up complex code into individual
responsibilities, perhaps as a precursor to moving to functions. Ditto
when removing repetition.

Whilst lambdas are quite happy with simple auto parameters, best not
to omit const or & as appropriate. Types may be required to help the IDE.

## macro 👎

Macros are global, horrible to write, horrible to read, can hide all
manner of sins, are diﬃcult to debug, and don’t play well with tooling.
Consider replacing with a function. For conditional compilation in
templates consider constexpr if.

## magic literal 👎

“Magic” literals placed directly in code offer no clue as to what exactly
is being specified or its origin, and no clue if the same data is used
elsewhere. Comprehension and maintenance burden.

Magic literals are data. **Replace with suitably named constants**. See also constexpr.
```cpp
displayLines(12); // bad
constexpr auto stdScreenLength {25};
constexpr auto bigScreenLength {43};
displayLines(bigScreenLength); // good
```

## naming 👍

**Clear, concise, naming makes code understandable**.

For an object whose purpose is to _do_ something (service object), prefer a verb.
E.g. “renderer”.

For an object that _is_ something (value object), prefer a noun. E.g. “drawing”.

Something diﬃcult to name concisely likely does not have a single purpose, and needs refactoring.

Use names that are specific. E.g. “SaveLogToDisc”, not “ProcessLog”. “Process” could be anything.

A variable named after its data value defeats the whole point of “variable”:
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

## out parameter 👎

Out parameters are _non-const, by-reference_, function parameters.
Known to cause hard to find bugs.

Whether values are updated by the function is not obvious.

**Where possible make function parameters `const` or `const&` and return
a value, or return a container to return multiple values**.

RVO simplifies return and usually elides copies.
```cpp
auto func = [](const auto& str, const auto num) {
	return { str, num };
};
```
Structured binding (since C++17) simplifies reading back the result at the calling side:
```cpp
auto [name, value] = func("qty", 2);
```

## repetition 👎

> One of the things I've been trying to do is look for simpler or rules
> underpinning good or bad design. I think one of the most valuable rules
> is to avoid duplication
--- Martin Fowler

> Code duplication is by far one of the worst anti-patterns in software
> engineering, eventually leading to buggy and unmaintainable systems.
--- Magnus Stuhr, 2020

> Don’t Repeat Yourself.
--- Andy Hunt and Dave Thomas, 1999

> Duplicate code is the root of all evil in software design
--- Robert C. (Uncle Bob) Martin

Alright already! Repetition should be _ruthlessly_ eliminated! And not just
identical code, but similar code too!

**DRY** = “_Don’t Repeat Yourself_”

**WET** = “_Waste Everyone’s Time_”, “_Write Everything Twice_”

Change requires finding every usage (difficult) and replicating the change
(error-prone). Failure to catch just one instance creates a nasty bug that
might remain undiscovered for a long time. Comprehension requires studying
every item. Small differences are notoriously difficult to spot.

**Repetition is entirely avoidable!**

The variant part (the bit that is different between usages) of repeating
code is often just one or two simple items in a long gobbledygook statement.
The variant parts can be extracted and passed as parameters to a function
or lambda executing the common body. A sequence of repeated code likely
indicates the underlying data is actually a set and hence should be defined
in a container and dealt with accordingly. clang-format off is often a sign
of repetition or data represented by code.

See also: ternary operator, variable sets, naming.

## static 👎

Often best avoided. For `const` variables consider `constexpr`, or
initialisation in lambda capture.

`static` functions _may_ be better moved out of class into namespace or some
utility library/file.

See also initialization.

## ternary operator 👍

Reduce six lines:
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
Also great for simplifying return statements. What’s not to like?

## unit test 👍

New code should be delivered with unit tests. Whilst e.g. GUI code is
not so testable, it should contain only the GUI part, no “business logic”.
Ideally, unit tests are created _before_, or _during_, code creation, and
run after every small change, ensuring all behaviours are tested and that
code conforms to the tests (rather than the other way round).

**A unit test targets a single behavior of a single unit of code, with a single assertion**.

If code is difficult to unit test consider if it can be extracted from
its surrounds — maybe into some library-like class — where it can be
tested in isolation. Is it possible to refactor to remove hard-wired
dependencies? Is it trying to do too much? Is it too tightly coupled?
See also dependencies.

Prematurely falling back to integration testing might be a sign of
failure to properly structure new code. Integration tests are to ensure
multiple units play well together.

## variable sets 👎

Related variables having names closely coupled to their initial value. E.g.:
```cpp
Item fred { "Fred", 20 };
Item martha { "Martha", 30 };
Item george { "George", 40 };
```
Issues:
- Data represented by code, variables no longer really variable. See also naming
- Every declaration, definition and usage has to be repeated = exploding code size
- Comprehension and maintenance nightmare

Solution:
- Move data into a container e.g. constexpr

std::array

(not vector or map).

Container elements are typically value, array, pair or tuple:
{% raw %}
```cpp
using Pair = std::pair<std::string_view, size_t>;

constexpr auto numItems {3};

constexpr std::array<Pair, numItems> items {{
	{ "Fred", 20 },
	{ "Martha", 30 },
	{ "George", 40 }
}};
```
{% endraw %}
Or struct, which has the advantage of named elements, but is slightly more overhead:
{% raw %}
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
{% endraw %}
