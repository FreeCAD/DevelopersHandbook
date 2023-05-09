---
layout: default
---

# Automated Testing

FreeCAD uses two different automated testing mechanisms, depending on the language being tested. The oldest, and most well-used test framework is the Python `unittest` system, which hooks directly into the FreeCAD Test Workbench. The second is the standalone Google Test C++ testing framework, which generates individual executables fo each part of the test suite.

## References

Some good references about automated testing:
* [Back to Basics: C++ Testing - Amir Kirsh - CppCon 2022 - YouTube.](https://www.youtube.com/watch?v=SAM4rWaIvUQ)
* [Practical Advice for Maintaining and Migrating Working Code - Brian Ruth - CppCon 2021 - YouTube.](https://www.youtube.com/watch?v=CktRuMALe2A)
* [The Science of Unit Tests - Dave Steffen - CppCon 2020 - YouTube.](https://www.youtube.com/watch?v=FjwayiHNI1w)
* [Michael Feathers. Working Effectively with Legacy Code. ISBN 0131177052.](https://www.amazon.com/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052)
* [Jeff Langr. Modern C++ Programming with Test-Driven Development: Code Better, Sleep Better. ISBN 1937785483.](https://www.amazon.com/Modern-Programming-Test-Driven-Development-Better/dp/1937785483/)

## Python Testing

Most Python workbenches in FreeCAD already have at least a rudimentary test suite, so no additional cMake setup should be required beyond simply adding your test file(s) to the cMakeLists.txt file. In addition, in Python it is very easy
to create Mock functions and objects to reduce the dependency on external code, and/or to ensure you are testing only the isolated bit of code that you mean to. A typical Python unit test file might look like this:
```python
# SPDX-License-Identifier: LGPL-2.1-or-later

import unittest
import unittest.mock

# Optional, allows your IDE to locate the appropriate test files to run outside FreeCAD if the code doesn't
# depend on a FreeCAD import
sys.path.append("../../") 

# Here "Version" is the name of the class being tested
class TestVersion(unittest.TestCase):

    MODULE = "test_metadata"  # file name without extension

    def setUp(self) -> None:
        pass # Or do any setup you want to run before every test, creating objects, etc.
        
    def tearDown(self) -> None:
        pass # Or to any cleanup work you need
 
    def test_from_file(self) -> None:
        """When loading from a file, the from_bytes function is called with the expected data"""
        from addonmanager_metadata import MetadataReader

        MetadataReader.from_bytes = Mock()
        with tempfile.NamedTemporaryFile(delete=False) as temp:
            temp.write(b"Some data")
            temp.close()
            MetadataReader.from_file(temp.name)
            self.assertTrue(MetadataReader.from_bytes.called)
            MetadataReader.from_bytes.assert_called_once_with(b"Some data")
            os.unlink(temp.name)
```

## C++ Testing

In an ideal world, a C++ unit test would be perfectly isolated from any external dependencies, which would be replaced with minimal, instrumented "mock" versions of themselves. However,
this almost always requires that the code under test has been *designed* for testing, which is usually not the case for our existing code. In many cases you must add tests for the existing
functionality and implementation, with all its deficiencies, before you can begin to refactor the code to make the tests better. There are many strategies for doing those "dependency injections", 
and over time we aspire to refactor FreeCAD such that it is possible, but developers are also encouraged to remember that:
* "A journey of a thousand miles begins with a single step"
* "How do you eat an elephant? One bite at a time."
* "Perfect is the enemy of good"

A single not-perfect test is better than no test at all (in nearly 100% of cases). As a general rule, a single test should verify a single piece of functionality
of the code (though sometimes that "functionality" is encompassed by multiple functions. For example, you will typically test getters and setters in pairs). Because your test functions will not
themselves be "under test" it is critical that they be as short, simple, and self-explanatory as possible. A common idiom to use is "Arrange-Act-Assert", which in our test
framework looks like this:
```c++
TEST(MappedName, toConstString)
{
    // Arrange
    Data::MappedName mappedName(Data::MappedName("TEST"), "POSTFIXTEST");
    int size {0};

    // Act
    const char* temp = mappedName.toConstString(0, size);

    // Assert
    EXPECT_EQ(QByteArray(temp, size), QByteArray("TEST"));
    EXPECT_EQ(size, 4);
}
```

While you can write a series of standalone tests, it is often more convenient to group them together into a "test fixture." This is a class that your test is derived from, which can
be used both to do setup and teardown, as well as to easily run all tests in the fixture without running the entire suite. Most IDEs recognize Google Test code and will offer the ability
to run both individual tests as well as entire fixtures very easily from the IDE's interface. An example test fixture and associated tests:
```c++
// SPDX-License-Identifier: LGPL-2.1-or-later

#include "gtest/gtest.h"

#include "App/IndexedName.h"
#include "App/MappedElement.h" // This is the class under test

// This class is the "Test Fixture" -- each test below is subclassed from this class
class MappedElementTest: public ::testing::Test
{
protected:
    // void SetUp() override {}

    // void TearDown() override {}

    static Data::MappedElement givenMappedElement(const char* index, const char* name)
    {
        Data::IndexedName indexedName {index};
        Data::MappedName mappedName {name};
        return {indexedName, mappedName};
    }
};

// Use the TEST_F macro to set up your test's subclass, derived from MappedElementTest
TEST_F(MappedElementTest, constructFromNameAndIndex)
{
    // Arrange
    Data::IndexedName indexedName {"EDGE1"};
    Data::MappedName mappedName {"OTHER_NAME"};

    // Act
    Data::MappedElement mappedElement {indexedName, mappedName};

    // Assert
    EXPECT_EQ(mappedElement.index, indexedName);
    EXPECT_EQ(mappedElement.name, mappedName);
}

TEST_F(MappedElementTest, moveConstructor)
{
    // Arrange
    auto originalMappedElement = givenMappedElement("EDGE1", "OTHER_NAME");
    auto originalName = originalMappedElement.name;
    auto originalIndex = originalMappedElement.index;

    // Act
    Data::MappedElement newMappedElement {std::move(originalMappedElement)};

    // Assert
    EXPECT_EQ(originalName, newMappedElement.name);
    EXPECT_EQ(originalIndex, newMappedElement.index);
}
```
To run the tests, either directly run the executables that are generated (they are placed in the `$BUILD_DIR/test` subdirectory), or use your IDE's test discovery functionality to run just the tests for
the code you are working on. FreeCAD's Continuous Integration (CI) suite will always run the full test suite, but it is advisable that before submitting a PR you run all tests on your local
machine first.

The test directory structure exactly matches that of FreeCAD as a whole. To prevent ever having to link the entirety of FreeCAD and all of its tests into a single executable (at some point in the future
when we have better test coverage!), the breakdown of the test executables mimics that of FreeCAD itself, with individual workbenches being compiled into their own test runners. To add a test runner to
a workbench that does not have one, a developer should add a new target for their WB to the end of the cMakeLists.txt file at the top of the `tests` directory structure, e.g.
```cmake
add_executable(Sketcher_tests_run)
add_subdirectory(src/Mod/Sketcher)
target_include_directories(Sketcher_tests_run PUBLIC ${EIGEN3_INCLUDE_DIR})
target_link_libraries(Sketcher_tests_run gtest_main ${Google_Tests_LIBS} Sketcher)
```
