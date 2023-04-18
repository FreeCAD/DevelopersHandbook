---
layout: default
---
#  Writing Code for Translation

How to write code with strings that should be translated

In most cases, any user-visible text in FreeCAD should be made translatable. Exceptions to this general rule are:
1. Text that only appears as log-level messages in the Report View
2. Names of Workbenches (e.g. "Start", "Part", "Path")
3. Names of Python commands, etc.

Some general guidelines when constructing strings for translation:
* Not all languages use the same word order, so it's best to write complete sentences (and sometimes complete paragraphs), using the `QString::args()` function, or the Python `format()` function to do replacements where necessary.
* The script that extracts translatable strings from Python does not currently (as of Qt 6.4) support Python string concatenation, so to follow the above guideline you *must* write your translatable string on a single line of code.
* The NOOP-versions of the Qt translation functions extract the string for translation purposes, but *do not* replace the string in place with its translated equivalent. They should generally only be used in classes derived from Gui::Command for things like the menu entry and tooltip. The Command class handles actually loading the translated string.
* In a non-QObject-derived class, use `Q_DECLARE_TR_FUNCTIONS(MyClass)` to give your class access to the `tr()` function. See also [the Qt documentation](https://doc.qt.io/qt-5/i18n-source-translation.html).

# Qt Translation Functions

## C++

The main translation function for C++ code is `tr()`, automatically defined in any class derived from `QObject`. It automatically sets the "context" of the translation to the name of the class, providing disambiguation between the same string in different parts of FreeCAD, where the meaning might be different. It generates a `QString` out of a string literal:
```
void SpecialWidget::DoThings()
{
    this->labelThing.setText(tr("This will get translated")); // Context is "SpecialWidget"
}
```

If your class is not derived from `QObject`, include the Q_DECLARE_TR_FUNCTIONS macro when defining your class to gain the `tr()` function:
```
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
```
label.setText(tr("Item %1 of %2 confabulated. Please wait...").arg(i).arg(total));
```
The string "Item %1 of %2 confabulated. Please wait..." will be presented to translators, but CrowdIn will prevent them from removing the placeholders accidentally.

Note that in many places in the FreeCAD source code you will see uses of `QT_TR_NOOP` - this is *only* used in cases where the string is later passed through a separate call to `tr()`. If you aren't sure, check with a Maintainer, and when in doubt, `tr()` is probably what you need.

## Python

In your Python code, import FreeCAD, then create a function called `translate` with the following code:
```
translate = FreeCAD.Qt.translate
```
It then gets used similarly to `tr()` in C++, but with a manually-specified context:
```
print(translate("MyMod", "This will get translated"))
```
Note that "translate" is not a true function: it is really a tag that the lupdate utility uses to identify strings for translation. You *cannot* rename it, or use it with non-literal strings, etc. You also cannot use f-strings, or Python string concatenation:
```
print(translate(modNameInAVariable, someOtherVariable)) # NO!
print(translate("MyMod", f"This won't {work}")) # NO!
print(translate("MyMod", "This" " also " "won't work")) # NO!
```
To do argument replacement use the `format` function of Python's string class:
```
print(translate("MyMod", "There are {} widgets present, out of {} total").format(present, total))
```
(Note that the string sent to translators is "There are {} widgets present, out of {} total" -- the format function is called on the string that is *returned* from the translate function).

## UI Files

For the most part, all strings in UI files are automatically subject to translation. In some circumstances you may want to *disable* that translation. Using Qt Designer, uncheck the "translatable" checkbox on the widget that you want to disable translation for. 

## Translating plurals

Many languages have complex pluralization rules. To support this, you can write strings that are designed for pluralization, including in English, by specifying a parameter that indicates how many of a thing there are. The translation system will dynamically select the correct translation (assuming translators wrote one), even for English. For example
```
int nErrors = getNumberOfErrors(); // Might be zero, might be one, might be some other number.
tr("There were %n errors.", "", nErrors);
```
Translators will be given the opportunity to translate this string in several variants (depending on the language they are translating for). For example, in English we would have:
```
There were 0 errors.
There was 1 error.
There were 2 errors.
```
In other languages there might be more than two variants, the sentence structure might change completely, etc. Qt and the CrowdIn translation platform support this structure.

## Testing your strings

The translation process has several steps, each of which you can examine during the development process.

### String extraction

To determine whether your strings are being correctly extracted, you can directly run the `lupdate` command on the file you are working on, using a dummy TS output file. `lupdate` is included in your Qt installation (note that internally we use lupdate from Qt 6.4 -- this is particularly important if you are testing Python code, as several major problems were address by a patch we submitted to Qt 6.4).
```
lupdate MySourceCodeFile.cpp --ts test.ts
```
will generate the file test.ts -- you can examine it to ensure your string appears with the expected context in that file. In particular, make sure that each string makes sense alone: otherwise, without context, translators will not know what they should replace it with.

### String replacement

To determine whether the string is being replaced correctly, you can create a dummy language file from the above test.ts file, and inject it into your FreeCAD instance. To do this you must know the name of the file that the finished translations will be put into. In general this is the name of the module you are working on, an underscore, the language code, and ".ts". For example, "PartDesign_es-ES.ts" is the Spanish (Spain) translation file for the Part Design workbench. An exception is the code in `src/Gui`, which is FreeCAD_xx.ts (where xx is the language code). 

For testing purposes, choose a language you are familiar with (or at least, can navigate well enough to deactivate when you are done testing!), and copy the file generated by lupdate into the appropriate filename. Locate the string you want to test:
```
    <message>
        <location filename="../CommandDoc.cpp" line="1649"/>
        <source>Activates or Deactivates the selected object&apos;s edit mode</source>
        <translation type="unfinished"></translation>
    </message>
```
Edit this to eliminate the "unfinished" and to add a translation, e.g.
```
    <message>
      <location filename="../CommandDoc.cpp" line="1649"/>
      <source>Activates or Deactivates the selected object's edit mode</source>
      <translation>Activa o desactiva el modo de edición del objeto seleccionado</translation>
    </message>
```
Next, this file must be converted to a `*.qm` file using the `lrelease` tool (distributed with Qt). Finally, place the generated QM file in the same location as your user.cfg file, in a subdirectory called "translations", then switch FreeCAD to the language you chose to test with. Your translated string should appear.
