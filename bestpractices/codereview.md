---
layout: default
---

# FreeCAD - Code Review Guide

This document aims to provide set of good practices that should be helpful for both developers and code reviewers. They should be treated like food recipes - you can play with them, alter them - but every change should be thoughtful and intentional.

Just like the C++ practices - this document is __very__ much inspired by the [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines).
You can always consult it to learn more useful stuff.

While points of this guide might not be consistently followed throughout all of the existing codebase, checking if new code follows them is possible and in long term should help us immensely.

> [!NOTE]
> Remember that code review is a collaborative discussion. Don’t hesitate to ask for clarification or help when needed. Reviewers can also make mistakes, the goal is to work together to refine the code to a point where everyone is satisfied.

In this document the **bolded** text will indicate how important each suggestion is.
 - **must** will be used for fundamental things that should be non-controversial and which you really should follow
 - **should** will be used for important details that will apply for vast majority of cases, there could however be valid reasons to ignore them depending on context
 - **could** will be used for best practices, things that you should try to follow but not following them is not an error per se

## Common Rules
1. Consistency **must** be preferred over strict rule following. If, for example, in a given context code uses different naming scheme - it should be followed instead of one described in that document.
2. The aim of Code Review **is not** to find errors in code but to ensure code quality.
3. There are no dedicated reviewers, everyone **can** help with the effort!
4. Reviewers **should** comment mostly on added code and discuss existing one only if making change to existing code would help the new fragment.
5. Comments **could** be made to existing comments just to note that other (potentially better) soluyions are available and should be used instead when writing new code.
6. Reviewer **can** hint to make more changes to the existing code in order to improve the proposed solution (for example, refactor another class so it can be used).
7. Code **must** follow the Scout Rule - https://biratkirat.medium.com/step-8-the-boy-scout-rule-robert-c-martin-uncle-bob-9ac839778385 - i.e. leave code in better shape than you found it.
8. If possible, code cleanup done while working on the feature should be split into separate commits or even better PR.
9. PRs **must not** contain any remains of development code, like debug statements other than actual logs.
10. New code **must not** introduce any new linter warnings
11. Developer **should** have `pre-commit` installed and working. `pre-commit-ci` commits should be avoided.

## Basic Code Rules
1. (C++ only) New code **must** be formatted with clang-format tool or in a way that is compatible with clang-format result if file is excluded from auto formatting.
1. (Python only) Code **must** follow the PEP 8 standard and formatted with Black.
3. Main execution path **should** be the least indented one, i.e. conditions should cover specific cases.
4. Early-Exit **should** be preferred to prune unwanted execution branches fast.
7. Global state (global variables, static fields, singletons) **should be** avoided.
8. (C++ only) `enum class` **should** be preferred over normal enum whenever possible.
9. (C++ only) C++ libraries/structures **should** be preferred over C ones. i.e. `std::array/vector` should be used instead of C array.
10. (C++ only) C++ standard library **should** be utilized if possible, especially the `<algorithms>` one.
    <details>
        <summary>Example #1</summary>

        Consider following code:
        ```c++
            std::vector<int> vertices;

            std::set<int> vertexSet;
            for (auto &s : face.getSubShapes(TopAbs_VERTEX)) {
                int idx = shape.findShape(s) - 1;
                if (idx >= 0 && vertexSet.insert(idx).second) {
                    vertices.push_back(idx);
                }
            }
        ```

        You can rewrite it in a following way:
        ```c++
            std::vector<int> vertices;

            std::set<int> vertexSet;
            for (auto &vertex : face.getSubShapes(TopAbs_VERTEX)) {
                int vertexId = shape.findShape(vertex);

                if (vertexId > 0) {
                    vertexSet.insert(vertexId);
                }
            }


            std::copy(vertexSet.begin(), vertexSet.end(), std::back_inserter(vertices));
        ```

        This way you split the responsibility of computing unique set with result preparation.
    </details>
11. (C++ only) Return values **should** be preferred over out arguments.
    a. For methods that can fail `std::optional` should be used
    b. For methods that return multiple values it may be better to either provide dedicated struct for result or use `std::tuple` with expression binding.
12. If expression is not obvious - it **should** be given a name by using variable.
    <details>
        <summary>Example #1</summary>
        TODO: Find some good example
    </details>
13. (C++ only) Code **should not** be put in anonymous namespace.
14. All members **must** be initialized.
    <details>
        <summary>Rationale</summary>
        Not initialized members can easily cause undefined behaviors that are really hard to find.
    </details>

## Design / Architecture
1. Each class / method / function **should** be doing only one thing and should do it well.
2. Classes that do provide business logic **should** be stateless. This helps with reusability.
3. Functions / Methods **should** be pure i.e. their result should only depend on arguments (and object state in case of method). This helps with reusability, predictability and testing.
4. Immutable data **should** be used whenever possible, (For C++ use `const`)
    <details>
        <summary>Rationale</summary>
        It is much easier to reason about code that deals with data that does not change.

        Using const modifier ensures that the object will stay unmodified and compiler will make sure that it is the case.
    </details>
5. (C++ only) If possible `constexpr` **should** be used for storing constant data.
5. Long methods **should** be split into smaller, better described ones.
6. (C++ only) Defining new macros **must** be avoided, unless absolutely necessary.
7. Integers **must not** be used to express anything other than numbers. For enumerations enums **must** be used.
8. Code **should** be written in a way that it expresses intent, i.e. what should be done, rather than just how it is done.
    <details>
        <summary>Example #1</summary>
        Consider this code:
        ```c++
            void setOverlayMode(OverlayMode mode)
            {
                // ... some code ...

                QDockWidget *dock = nullptr;

                for (auto w = qApp->widgetAt(QCursor::pos()); w; w = w->parentWidget()) {
                    dock = qobject_cast<QDockWidget*>(w);
                    if (dock) {
                        break;
                    }
                    auto tabWidget = qobject_cast<OverlayTabWidget*>(w);
                    if (tabWidget) {
                        dock = tabWidget->currentDockWidget();
                        if (dock) {
                            break;
                        }
                    }
                }

                if (!dock) {
                    for (auto w = qApp->focusWidget(); w; w = w->parentWidget()) {
                        dock = qobject_cast<QDockWidget*>(w);
                        if (dock) {
                            break;
                        }
                    }
                }

                // some more code ...

                toggleOverlay(dock, m);
            }
        ```

        It is hard to understand what is the job of the for loop inside `if (!dock)` statement.
        We can refactor it to a new `QWidget* findClosestDockWidget()` method for it to look like this:

        ```c++
            void setOverlayMode(OverlayMode mode)
            {
                // ... some code ...

                QDockWidget *dock = findClosestDockWidget();

                // ... some more code ...

                toggleOverlay(dock, m);
            }
        ```
        The findClosestDockWidget() could either be implemented as private method or an inner function using lambdas.

        ```c++
        auto findClosestDockWidget = []() { ... }
        ```

        That way reading through code of `setOverlayMode` we don't need to care about the details of finding the closest dock widget.
    </details>
10. Boolean arguments **must** be avoided. Use enumerations instead - enum with 2 values is absolutely fine.
    For python boolean arguments are ok, but they **must** forced to be keyword ones.
    <details>
        <summary>example #1 (c++)</summary>
        consider following example:

        ```c++
        mapper.populate(false, it.key(), it.value());
        ```

        it is impossible to understand what false means without consulting the documentation or at least the method signature.
        instead the enum should be used:

        ```c++
        mapper.populate(mappingstatus::modified, it.key(), it.value());
        ```

        now the intent is clear
    </details>
11. (C++ only) Code **should** prefer uniform initialization `{ }` (also called brace initialization). Prevents narrowing.
12. (C++ only) Class member variables **should** be initialized at declaration, not in constructors
13. Magic numbers or other literals **must** be avoided.


## Naming Things
1. Code symbols (classes, structs, methods, functions, variables...) **must** have names that are meaningful and grammatically correct.
2. Variables **should not** be named using abbreviations and/or 1 letter names. Iterator variables or math related ones like `i` or `u` are obviously not covered by this rule.
3. Names **must not** use the hungarian notation.
4. (C++ only) Classes/Structs **must** be written in `PascalCase`, underscores are allowed but should be avoided.
5. (C++ only) Class members **should** be written in `camelCase`, underscores are allowed but should be avoided.
6. (C++ only) Global functions **should** be written in `camelCase`, underscores are allowed but should be avoided.
7. (C++ only) Enum cases **should** use `PascalCase`

## Commenting the code
1. Good naming **must** be preferred over commenting the code.
2. Comments that describe what code does **should** be avoided, instead comments **should** explain intended result.
3. All edge-cases in code **must** be described with comment describing when such edge-case can occur and why it is handled in certain way.
4. All "Hacks" **must** be described with how the hack works, why it is applied and when it no longer will be needed.
5. Commented code **must** be contain additional information on why it was commented out and when it is safe to remove it.
