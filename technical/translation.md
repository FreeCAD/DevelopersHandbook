---
layout: default
---

#  Writing Code for Translation

How to write code with strings that should be translated.

In most cases, any user-visible text in FreeCAD should be made translatable. Exceptions to this general rule are:
1. Text that only appears as log-level messages in the Report View
2. Names of Python commands, etc.

Some general guidelines when constructing strings for translation:
* Not all languages use the same word order, so developers should write complete phrases/sentences (and sometimes complete paragraphs), using the `QString::args()` function, or the Python `format()` function to do replacements where necessary. For example, use `tr("Edit the %1 using the %2").args(varOne).args(varTwo)` instead of `tr("Edit the ") + varOne + tr(" using the ") + varTwo`.
* Avoid using Rich Text or HTML for labels unless their formatting capabilities are absolutely necessary.
* Do not abbreviate unless absolutely necessary for space considerations. For example "Boolean operation", not "Bool Op".
* Use capital letters for Workbench names, and lowercase elsewhere, to help disambiguate. For example "Create a Path object" refers to the Path workbench, but "Create a part" referes to a conceptual "part".
* Never rely on the displayed string as a conditional value in your code: you must always map from your internal representation to a translatable string. For example, if a `QComboBox` is constructed with a set of user options, you should only ever examine the *index* of the current item, and never its actual displayed text.

Important technical details:
* The NOOP-versions of the Qt translation functions extract the string for translation purposes, but *do not* replace the string in place with its translated equivalent. A later call to `tr()` or `translate()` is always required in these cases. See more detail below.
* In a non-QObject-derived class, use `Q_DECLARE_TR_FUNCTIONS(MyClass)` to give your class access to the `tr()` function. See also [the Qt documentation](https://doc.qt.io/qt-5/i18n-source-translation.html).

## Qt Translation Process

Qt translation takes place in four steps (only two of which developers need concern themselves with, steps 1 and 4 below):

1. A source-code pre-processor called `lupdate` examines the source code, looking for specific strings that mark string literals to be translated. This is **not** a compiler or interpreter, and does not understand all of the details of C++ or Python code. Its only purpose is to generate a `*.ts` file that can be uploaded to our translation platform, CrowdIn. That upload is not currently automated, and must be done manually by a CrowdIn Manager. See [https://crowdin.com/project/freecad](https://crowdin.com/project/freecad).
2. Translators on CrowdIn are presented with these strings (plus some metadata about where they are in the code, and optional comments that developers can leave for them). For each language, translators work to develop appropriate translated strings.
3. A CrowdIn Manager exports those strings, processes them to `*.qm` files, and commits them to the FreeCAD repository.
4. When FreeCAD is compiled, those files become part of FreeCAD's Qt resources and are available to the translation functions at runtime. The translation functions `tr()` and `translate()` do the actual work of looking up the string in the translation table and extracting the correct user-visible translation.

### lupdate extraction

The critical first step above is the use of `lupdate` to extract strings marked for translation. From a developer point of view, the most critical thing to
note is that *only string literals are extracted*. A variable containing a string is **not**. There are many different functions that can be used to mark strings
for extraction: the four most important are:
* `tr(string)` All QObject-derived classes have access to this function. It serves *both* to mark a string for extraction (if a string literal is provided) *and* to do the actual translation lookup at runtime. In classes *not* derived from `QObject`, the `Q_DECLARE_TR_FUNCTIONS` macro can be used to define this function in your class.
* `translate(context, string)` It's sometimes necessary to manually specify the "context" of a string. Otherwise this does the same thing that `tr()` does.
* `QT_TR_NOOP(string)` This marks a string literal for extraction, *but does not translate it in place*. In compiled/interpreted code, this is a literal no-op, it does nothing and is completely ignored. A later call to one of the above translation functions must do the actual user-visible translation.
* `QT_TRANSLATE_NOOP(context, string)` The same as `QT_TR_NOOP` but with manual specification of context.

The most basic usage is:
```cpp
QPushButton myButton(tr("Do great things"));
```
In this case, `tr()` functions both as a marker for the `lupdate` tool to extract the string literal "Do great things", as well as an actual function call at
runtime to look up the appropriate translated string. The string's context is the name of the class that this code is exectuded from, provided automatically
by the `tr()` function.

In some cases it's necessary or useful to split up the string extraction and the translation calls, for example:
```cpp
// The following vector ALWAYS contains the English language strings, regardless of the current
// language setting. The macro just serves as a marker for lupdate.
std::vector<std::string> optionsToDisplay {
    QT_TRANSLATE_NOOP("WidgetOptionDisplay", "Long bar"),
    QT_TRANSLATE_NOOP("WidgetOptionDisplay", "Short bar"),
    QT_TRANSLATE_NOOP("WidgetOptionDisplay", "Dots"),
    QT_TRANSLATE_NOOP("WidgetOptionDisplay", "Dashes")
};

// Then later in your code:
QComboBox *optionCombo = getOptionCombo();
for (const auto &option : optionsToDisplay) {
    // option is in English: ask the translate function to look it up in the database
    optionCombo->addItem(translate("WidgetOptionDisplay", option.c_str()));
}
```
In that case, the `translate()` function call cannot provide any information to `lupdate` because it is not being called on a string literal. Therefore
a separate mechanism must be used to provide the string to translators, and `translate()` only serves to do the final database lookup at runtime.

### C++

The main translation function for C++ code is `tr()`, automatically defined in any class derived from `QObject`. It automatically sets the "context" of the translation to the name of the class, providing disambiguation between the same string in different parts of FreeCAD, where the meaning might be different. It generates a `QString` out of a string literal:

```cpp
void SpecialWidget::DoThings()
{
    this->labelThing.setText(tr("This will get translated")); // Context is "SpecialWidget"
}
```

If your class is not derived from `QObject`, include the Q_DECLARE_TR_FUNCTIONS macro when defining your class to gain the `tr()` function:

```cpp
#include <QCoreApplication>

class NotAWidget
{
    Q_DECLARE_TR_FUNCTIONS(NotAWidget);
    
public:
    NotAWidget();
};
```

Technically `tr()` can take an additional string: a "comment" that is displayed to translators, but is not itself translated. This is rarely used.

To do string replacement use `QString::arg()`, for example:

```cpp
label.setText(tr("Item %1 of %2 confabulated. Please wait...").arg(i).arg(total));
```

The string "Item %1 of %2 confabulated. Please wait..." will be presented to translators, but CrowdIn will prevent them from removing the placeholders accidentally.

Note that in many places in the FreeCAD source code you will see uses of `QT_TR_NOOP` - this is *only* used in cases where the string is later passed through a separate call to `tr()`. If you aren't sure, check with a Maintainer, and when in doubt, `tr()` is probably what you need.

### Python

In your Python code, import FreeCAD, then create a function called `translate` with the following code:

```python
translate = FreeCAD.Qt.translate
```

It then gets used similarly to `tr()` in C++, but with a manually-specified context:

```python
print(translate("MyMod", "This will get translated"))
```

Note that "translate" is not a true function: it is really a tag that the lupdate utility uses to identify strings for translation. You *cannot* rename it, or use it with non-literal strings, etc. You also cannot use f-strings:

```python
print(translate(modNameInAVariable, someOtherVariable)) # NO!
print(translate("MyMod", f"This won't {work}")) # NO!
```
To do argument replacement use the `format` function of Python's string class:

```python
print(translate("MyMod", "There are {} widgets present, out of {} total").format(present, total))
```

(Note that the string sent to translators is "There are {} widgets present, out of {} total" -- the format function is called on the string that is *returned* from the translate function).

## UI Files

For the most part, all strings in UI files are automatically subject to translation. In some circumstances you may want to *disable* that translation. Using Qt Designer, uncheck the "translatable" checkbox on the widget that you want to disable translation for. 

## Translating plurals

Many languages have complex pluralization rules. To support this, you can write strings that are designed for pluralization, including in English, by specifying a parameter that indicates how many of a thing there are. The translation system will dynamically select the correct translation (assuming translators wrote one), even for English. For example:

```cpp
int nErrors = getNumberOfErrors(); // Might be zero, might be one, might be some other number.
tr("There were %n errors.", "", nErrors);
```

Translators will be given the opportunity to translate this string in several variants (depending on the language they are translating for). For example, in English we would have:

```txt
There were 0 errors.
There was 1 error.
There were 2 errors.
```

In other languages there might be more than two variants, the sentence structure might change completely, etc. Qt and the CrowdIn translation platform support this structure.

## Testing your strings

The translation process has several steps, each of which you can examine during the development process.

### String extraction

To determine whether your strings are being correctly extracted, you can directly run the `lupdate` command on the file you are working on, using a dummy TS output file. `lupdate` is included in your Qt installation (note that internally we use lupdate from Qt 6.4 -- this is particularly important if you are testing Python code, as several major problems were address by a patch we submitted to Qt 6.4).

```bash
lupdate MySourceCodeFile.cpp --ts test.ts
```

will generate the file test.ts -- you can examine it to ensure your string appears with the expected context in that file. In particular, make sure that each string makes sense alone: otherwise, without context, translators will not know what they should replace it with.

### String replacement

To determine whether the string is being replaced correctly, you can create a dummy language file from the above test.ts file, and inject it into your FreeCAD instance. To do this you must know the name of the file that the finished translations will be put into. In general this is the name of the module you are working on, an underscore, the language code, and ".ts". For example, "PartDesign_es-ES.ts" is the Spanish (Spain) translation file for the Part Design workbench. An exception is the code in `src/Gui`, which is FreeCAD_xx.ts (where xx is the language code). 

For testing purposes, choose a language you are familiar with (or at least, can navigate well enough to deactivate when you are done testing!), and copy the file generated by lupdate into the appropriate filename. Locate the string you want to test:

```xml
    <message>
        <location filename="../CommandDoc.cpp" line="1649"/>
        <source>Activates or Deactivates the selected object&apos;s edit mode</source>
        <translation type="unfinished"></translation>
    </message>
```

Edit this to eliminate the "unfinished" and to add a translation, e.g.

```xml
    <message>
      <location filename="../CommandDoc.cpp" line="1649"/>
      <source>Activates or Deactivates the selected object's edit mode</source>
      <translation>Activa o desactiva el modo de edición del objeto seleccionado</translation>
    </message>
```

Next, this file must be converted to a `*.qm` file using the `lrelease` tool (distributed with Qt). Finally, place the generated QM file in the same location as your user.cfg file, in a subdirectory called "translations", then switch FreeCAD to the language you chose to test with. Your translated string should appear.

## Synchronizing Translations with CrowdIn

This section is for CrowdIn administrators and FreeCAD Maintainers. Approximately once per week, FreeCAD's source tree should be synchronized with CrowdIn. This is a multi-step process that may be partially automated in the future. For the time being:
1. Ensure you have at least Qt 6.4 available on your system, and use the `updatets.py` script in src/Tools to extract the translatable strings from the FreeCAD source code. You do not need to be compiling FreeCAD with this version of Qt, but you must use it to extract the strings, it addresses many longstanding bugs with earlier extractors.
2. Examine the output from that script to ensure there were no errors that prevented some of the workbenches from being extracted.
3. If all sections of the code completed appropriately, use the `updatecrowdin.py` in src/Tools with the `update` subcommand to send the new TS files to CrowdIn. You will need a CrowdIn API key in your home directory for this to work: see the comments at the top of that script for details.
4. Next, using `updatecrowdin.py build` (or manually on the CrowdIn website) trigger a build of the current translation files. This process will take a few minutes. You can check the status with `updatecrowdin.py build-status`, or by visiting the CrowdIn website.
5. Use `updatecrowdin.py download` to get a new copy of the translations.
6. Create a new PR branch using git.
7. Use `updatecrowdin.py apply` to move the TS files into place, and to generate the QM files for the parts of the code where that is not yet part of the build process (the Python workbenches).
8. Create a PR with the changed files.
