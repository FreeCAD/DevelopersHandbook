## Naming Convention and Writing Style

Due to the technical nature associated with CAD and advanced programming, it is easy to fall into the trap of using terminology that is either overly descriptive or seems obvious as a developer but is obscure to the layman. Additionally, with so many functions at a user's disposal within FreeCAD, screen space comes at a high premium, and text strings can quickly change a UI/UX from being concise and efficient to awkwark and unpleasant. The following guidelines should be considered when naming/labeling functions, features and other various addons.

*NOTE: This area is highly subjective as FreeCAD is translated into many languages, however English is considered the standard by which naming will be assessed. It will be up to various translators to best adhere to these guidelines as best as possible.*

Efforts should be made to conserve space within the UI in order to prevent elements from needing to be resized from workbench-to-workbench or dialog-to-dialog. Word choices should be reviewed before merging a pull-request with text elements in the GUI.

### Language

- Use the shortest descriptive words possible for a function. Single words are preferred, however this will not always be possible. For example use `Additive cube` instead of "Add additive Cube" or "Create new additive cube" for naming the command. Tooltips can be more descriptive, but concepts and functionality should be explained in the documentation.
- Do not use verbs in the command name itself where not needed and try to avoid "Create", "Add", "Insert" in the command name. Example: `New File` is sufficient, the command should not say "Create new file". It can be relevant if the action is not obvious, e.g. `Insert image` (just "Image" would not be enough). Instead of verbs use nouns in the command name, where the most important word should be in the beginning: `Horizontal constraint` instead of "Constrain horizontal" (otherwise all similar commands start with constraint).
- Use third-person present tense in tooltips or hints. `Creates`, `adds`, `sends`, `opens`, etc. and write tooltips and hints as complete sentences, starting with the verb where possible.
- Describe the action or result in tooltips: say what the command does and what is needed (e.g. active body or selection), do not only repeat its name. Be specific if possible: instead of "Add sketch" write `Adds a new sketch to the active body`.
- If a text is valid for one or multiple actions use always plural, e.g. `Creates holes based on the selected profiles` not "Creates hole(s) based on the selected profile(s)".
- Do not add additional information which is obvious or list multiple steps like "Select one or more line(s) click this button, add dimension". Better: `Inserts a dimension for the selected lines`.
- Avoid redundant descriptions of states in tooltips and notes. Example: instead of "If checked by the user, the view is updated automatically", "If active,..." or "This option controls..." use `Updates view atomatically`.
- Avoid addressing the user directly or in third person. Do not write "Click this button" in the tooltip or "Select something then do this". Instead describe in a short sentence what the command does with which inputs without asking the user directly.
- Broadly accepted and understood acronyms can be used in place of full words, however, these should be scrutinized closely by the design and development teams to avoid obscure references.
- Write numbers in arabic numerals (0, 1, 2,...) not spelled out (one, two)
- For concepts already used within FreeCAD, use the same wording so those concepts remain consistent wherever possible.
- Avoid using common programming terms, preference should be given towards more plain language words.
- For all user-facing stings add comments to help adding context for translators `//: Sketcher: Command name for new line`. Mor info in the [Qt docs](https://doc.qt.io/qt-6/i18n-source-translation.html#add-comments-for-translators).


### UI Text Style

Use **sentence case** for all user-facing UI text, including buttons, menu items, tooltips, labels, and settings. Sentence case means only the first word and proper nouns are capitalized, and complete sentences end with a period. This makes the user interface visually less distracting and easier to read. For the same reason, avoid contractions like for example "don't" or "can't" - write `do not` and `cannot` instead.

**Do not use periods** at the end of **short** UI strings like menu items, button labels, and checkbox texts. Use a period on tooltips and longer text elements, if they contain a full sentence. Omit the period if the tooltip is a short phrase or label.

**Do not capitalize commands or object names** unless it is a technical term that is capitalized in general.

**Use ellipsis (...)** at the end of menu items when specific input from the user is required for the action.

**Example:**
- `Constrains new geometry automatically. It is recommended to leave it enabled for most workflows.` - Complete sentences end with a period.
- `New file` - This is just a short label. A tooltip for that command button would be "Creates a new file".
- `Save as...` - The function requires user input to function.
- `Addon manager` - This is not a simple action, so no ellipsis.

[Return to Design Guide Main Page](index.md)
