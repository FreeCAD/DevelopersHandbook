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
* Use capital letters for Workbench names, and lowercase elsewhere, to help disambiguate. For example "Create a Path object" refers to the Path workbench, but "Create a part" refers to a conceptual "part".
* Never rely on the displayed string as a conditional value in your code: you must always map from your internal representation to a translatable string. For example, if a `QComboBox` is constructed with a set of user options, you should only ever examine the *index* of the current item, and never its actual displayed text.

Important technical details:
* The NOOP-versions of the Qt translation functions extract the string for translation purposes, but *do not* replace the string in place with its translated equivalent. A later call to `tr()` or `translate()` is always required in these cases. See more detail below.
* In a non-QObject-derived class, use `Q_DECLARE_TR_FUNCTIONS(MyClass)` to give your class access to the `tr()` function. See also [the Qt documentation](https://doc.qt.io/qt-5/i18n-source-translation.html).

## Basic Usage

For approximately 99% of text strings, the usage of the translation functions is completely straightforward. This section shows the most basic usage patterns. More [advanced use is covered later](#advanced-translation).

### UI Files

When you add a string to a *.ui file, by default it is set to be translated. For most UI strings this is the appropriate choice. If you need to indicate that a string should *not* be translated, you can either disable translation in Designer via the checkbox that controls the "translatable" property, or you can add `notr="true"` as an XML attribute on the string element in the raw text of the UI file, e.g.
```xml
<string notr="true">FreeCAD</string> <!-- No need to translate the word "FreeCAD" -->
```

If the string is ambiguous when displayed by itself to translators, please add "disambiguation" to the element. In Qt Designer:
* Select a widget with the text property.
* If the "Disambiguation" field is not visible, click the "Option" arrow next to the "Text" property in the Property Editor.
* Enter your explanatory remarks in the "disambiguation" field. You can also add an extra comment in the "comment" field, but these display very similarly to translators so there's little value in separating the two things.

In the resulting .ui XML, it appears like this:
```
    <string comment="Disambiguation string here" extracomment="This is a comment">Do thing</string>```
```

### C++

All classes derived from `QObject` have access to the `tr()` method from that class, which is the simplest way of ensuring a string gets translated:
```cpp
QPushButton myButton(tr("Do great things"));
```
Note that the argument to `tr()` is a *string literal* -- it cannot be a variable, or a formatted string, etc. If you would like to provide translators with additional information about the string, you can add a comment on the immediately-preceeding line beginning with a `//:`, e.g.
```
//: Runs the function
QPushButton myButton(tr("Execute"));
```
You can also add a "disambiguation string", which is displayed to translators to help them understand what the word means in this context (remember that translators don't see the source code itself, only the isolated string and the name of the file it's in. Many times that is not enough information for a good translation):
```
QLabel myLabel(tr("Draft angle", "For injection molding, not the Draft workbench"));
```
If you need to do string replacements, provide translators with the whole string, and and use the `QString::arg` method to fill in the placeholders:
```
label.setText(tr("%1 of %2 files copied. Copying: %3")
                  .arg(done)
                  .arg(total)
                  .arg(currentFile));
```
Most C++ translation should use these mechanisms as written here, but in various places in FreeCAD more complex mechanisms are needed to handle alternate use cases. Those are described in the [Advanced Translation section below](#advanced-translation).

### Python

In your Python code, import FreeCAD, then create a function called `translate` with the following code:

```python
translate = FreeCAD.Qt.translate
```

It then gets used similarly to `tr()` in C++, but with a manually-specified "context":

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

The "context" is generally the name of the class that the string appears in, but in some cases it may make sense to use an alternate context. See the Advanced Translation section below.

In many cases it is desirable to provide additional information to translators about what a word means in the context in which it appears. Translators see the string in isolation, and are only given the name of the file it appears in. To give extra information to aid in translation, provide a "disambiguation string" to your `translate()` call:
```
translate ("FEM", "Node Set", "Refers to a collection of finite-element nodes to be used in the extraction operation")
```
The translation team greatly appreciates such additional information to assist with strings that might otherwise not make sense when displayed in isolation in their CrowdIn interface.

### Working with Translators

In most cases the developers and translators are not the same groups of people, and translators typically work on [the CrowdIn platform](https://crowdin.com/project/freecad). Some coordination and extra courtesy is required to ensure that we present the best possible experience for everyone in the project.

1. *Any* change to a string requires it to be retranslated. For small changes that is an easy thing to do, but **it requires human intervention for each and every language that FreeCAD supports**. This means that, e.g. changing the capitalization of a single letter in a string will require additional input on CrowdIn from several dozen individual translators. Until they do that, the string will be presented in English. The consequence of this is that minor string changes *must not* be made late in a release cycle: it is better for a small inconsistency to appear in English than for every single language to display the "correct" English string instead of the translated one (in many cases the translators will have corrected the inconsistency or error in their own language).
2. Two strings with different context require separate translation. In cases where the string is used throughout FreeCAD, explicitly use `QObject::tr()` to place the string in the `QObject` context so translators only need translate the string a single time.
3. Re-use strings (within the same context) whenever possible. An entirely new UI element that re-uses an existing string in the same context requires no intervention from translators, and the string translation will simply work.
4. Most translators don't frequent GitHub, so translation bugs reported in the main FreeCAD issue tracker will often go unnoticed. Problems with translations (that is, where the English text is deemed correct, but in some language or other the translated text is incorrect) should be reported on CrowdIn, not GitHub.
5. Conversely, most developers don't frequent CrowdIn, so when the original English string is incorrect, when a UI element is not available for translation at all, or when a string appears for translators but does not appear translated in the UI, the issue should ultimately be migrated to GitHub. The FreeCAD project maintains a separate repository purely for translation-related bugs: https://github.com/FreeCAD/FreeCAD-translations/

## Advanced Translation

To understand how and why the more advanced translation mechanisms are needed, it's helpful to understand how Qt actually does the translation. Qt's main page for developers can be found at [Writing Source Code for Translation](https://doc.qt.io/qt-6/i18n-source-translation.html)

### Qt Translation Process

Qt translation takes place in four steps (only two of which developers need concern themselves with, steps 1 and 4 below):

1. A source-code pre-processor called `lupdate` examines the source code, looking for specific strings that mark string literals to be translated. This is **not** a compiler or interpreter, and does not understand all of the details of C++ or Python code. Its only purpose is to generate a `*.ts` file that can be uploaded to our translation platform, CrowdIn. That upload is automated via a GitHub Action and is done once per week.
2. Translators on CrowdIn are presented with these strings (plus some metadata about where they are in the code, and optional comments that developers can leave for them). For each language, translators work to develop appropriate translated strings.
3. A GitHub Action then exports those strings once per week, processes them to `*.qm` files, and creates a PR to commit them to the FreeCAD repository.
4. When FreeCAD is compiled, those files become part of FreeCAD's Qt resources and are available to the translation functions at runtime. The translation functions `tr()` and `translate()` do the actual work of looking up the string in the translation table and extracting the correct user-visible translation.

#### lupdate extraction

The critical first step above is the use of `lupdate` to extract strings marked for translation. From a developer point of view, the most critical thing to
note is that *only string literals are extracted*. A variable containing a string is **not**. There are many different functions that can be used to mark strings
for extraction: the four most important are:
* `tr(string)` All QObject-derived classes have access to this function. It serves *both* to mark a string for extraction (if a string literal is provided) *and* to do the actual translation lookup at runtime. In classes *not* derived from `QObject`, the `Q_DECLARE_TR_FUNCTIONS` macro can be used to define this function in your class. It sets the context of the string to the name of the class.
* `translate(context, string)` It's sometimes necessary to manually specify the "context" of a string. Otherwise this does the same thing that `tr()` does.
* `QT_TR_NOOP(string)` This marks a string literal for extraction, *but does not translate it in place*. In compiled/interpreted code, this is a literal no-op, it does nothing and is completely ignored. A later call to one of the above translation functions must do the actual user-visible translation.
* `QT_TRANSLATE_NOOP(context, string)` The same as `QT_TR_NOOP` but with manual specification of context.

### Context

All translatable text strings have a "context" that is used to group them together for translators, and to prevent identical strings from being translated identically when they occur in different scenarios. For example, the word "Draft" might refer to the Draft workbench, to the action of creating a "rough draft", or to a "draft angle" in injection molding, etc. By giving this string a different context in each scenario, the translation system won't mistakenly choose to translate the word in the wrong sense of its use. Identical strings with different contexts must be individually translated (though the translation system on CrowdIn makes it easy to copy a translation from one context to another, so this is not a difficult process, simply one that must be done). By default, using the bare `tr()` function sets the context to the name of the class the string appears in.

### More Details and Advanced Use-Cases

The most basic usage is:
```cpp
QPushButton myButton(tr("Do great things"));
```
In this case, `tr()` functions both as a marker for the `lupdate` tool to extract the string literal "Do great things", as well as an actual function call at
runtime to look up the appropriate translated string. The string's context is the name of the class that this code is executed from, provided automatically
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

#### C++

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

When using `tr()` it can also take an additional parameter: a "disambiguation string" that is displayed to translators, but is not itself translated. Translators appreciate it when developers provide the additional information, so this usage is encouraged, but not yet widespread in the FreeCAD codebase.
```
tr("Back", "as opposed to Front")
```
And a comment:
```
//: A label that appears next to the gizmo for aligning the view, so it's best to keep it as short as possible
tr("Back", "as opposed to Front")
```

To do string replacement use `QString::arg()`, for example:

```cpp
label.setText(tr("Item %1 of %2 confabulated. Please wait...").arg(i).arg(total));
```

The string "Item %1 of %2 confabulated. Please wait..." will be presented to translators, but CrowdIn will prevent them from removing the placeholders accidentally.

Note that in many places in the FreeCAD source code you will see uses of `QT_TR_NOOP` - this is *only* used in cases where the string is later passed through a separate call to `tr()`. If you aren't sure, check with a Maintainer, and when in doubt, `tr()` is probably what you need. You will also see explicit calls to `QObject::tr()` -- this ensures that the string will be placed in the `QObject` context, which is useful for strings that are repeated in many classes throughout the codebase.

### Special Cases

There are several places in FreeCAD where specific procedures must be followed to ensure translations are correct:

#### Preference Pages

Whether using `addPreferencePage` in Python, or `new Gui::PrefPageProducer` in C++, the "Group" of the page must use the NOOP-functions and the `QObject` context.

In Python this is:
```python
FreeCADGui.addPreferencePage("somePage.ui", QT_TRANSLATE_NOOP("QObject", "SomeGroupName"))
```
and in C++,
```cpp
new Gui::PrefPageProducer<SomeClass> (QT_TRANSLATE_NOOP("QObject","SomeGroupName"));
```

#### Exceptions

C++ exceptions that may end up displayed to the user should be put inside a QT_TRANSLATE_NOOP with the `Exception` context, e.g.
```cpp
throw MyException(QT_TRANSLATE_NOOP("Exception", "Something bad happened"));
```

#### Commands

When creating a new command, use the following template:
```cpp
CmdDoAThing::CmdDoAThing()
  :Command("MyWorkbench_DoAThing")
{
    sAppModule    = "MyWorkbench";
    sGroup        = "MyWorkbench";
    sMenuText     = QT_TR_NOOP("Do a thing");
    sToolTipText  = QT_TR_NOOP("A longer explanation of doing a thing");
    sWhatsThis    = "MyWorkbench_DoAThing";
    sStatusTip    = sToolTipText;
    sPixmap       = "MyWorkbench_DoAThing";
}
```
Note that `sGroup` should not be translated here (you will still find many counterexamples in older code, however).

If for any reason you need to manually set the name of the command's menu item or tooltip, you will need to explicitly (and manually) set the context to the name of the class, e.g.:
```
action->setText(QCoreApplication::translate("MyCommandClass", "Some text to be translated"));
```

## Translating plurals

Many languages have more complex pluralization rules than English. To support this, you can write strings that are designed for pluralization, including in English, by specifying a parameter that indicates how many of a thing there are. The translation system will dynamically select the correct translation (assuming translators wrote one), even for English. For example:

```cpp
int nErrors = getNumberOfErrors(); // Might be zero, might be one, might be some other number.
tr("There were %n errors.", "", nErrors); // Note %n, and the third parameter (which was not present in earlier examples)
```

Translators will be given the opportunity to translate this string in several variants (depending on the language they are translating for). For example, in English we would have:

```txt
There were 0 errors.
There was 1 error.
There were 2 errors.
```

In other languages there might be more than two variants, the sentence structure might change completely, etc. Qt and the CrowdIn translation platform support this structure. Note that as of this writing the English strings are unused: English translations are not actually used. Future work on the FreeCAD project includes enabling these English "translations" so that we can easily provide the plural forms.

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

## Manually Synchronizing Translations with CrowdIn

In most cases the GitHub-Actions based workflow for synchronizing translations is used, and no manual intervention is required. That process runs weekly at midnight on Monday morning (in GitHub Actions' timezone).

To run a manual translation sequence:
1. Ensure you have at least Qt 6.4 available on your system, and use the `updatets.py` script in src/Tools to extract the translatable strings from the FreeCAD source code. You do not need to be compiling FreeCAD with this version of Qt, but you must use it to extract the strings, it addresses many longstanding bugs with earlier extractors. Alternatively, you can also launch `updatecrowdin.py gather` which will under the hood run update.ts.
2. Examine the output from that script to ensure there were no errors that prevented some of the workbenches from being extracted.
3. If all sections of the code completed appropriately, use the `updatecrowdin.py` in src/Tools with the `update` subcommand to send the new TS files to CrowdIn. You will need a CrowdIn API key in your home directory for this to work: see the comments at the top of that script for details.
4. Next, using `updatecrowdin.py build` (or manually on the CrowdIn website) trigger a build of the current translation files. This process will take a few minutes. You can check the status with `updatecrowdin.py build-status`, or by visiting the CrowdIn website.
5. Use `updatecrowdin.py download` to get a new copy of the translations.
6. Create a new PR branch using git.
7. Use `updatecrowdin.py apply` to move the TS files into place, and to generate the QM files for the parts of the code where that is not yet part of the build process (the Python workbenches).
8. Create a PR with the changed files.
