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
- Broadly accepted and understood acronyms can be used in place of full words (e.g. `CAD` or `ISO`), however, these should be scrutinized closely by the design and development teams to avoid obscure references.
- Write numbers in arabic numerals (`0, 1, 2,...`) not spelled out ("one, two,...")
- For concepts already used within FreeCAD, use the same wording so those concepts remain consistent wherever possible.
- Avoid using common programming terms, preference should be given towards more plain language words.
- For all user-facing strings add comments to help adding context for translators `//: Sketcher: Command name for new line`. More info in the [Qt docs](https://doc.qt.io/qt-6/i18n-source-translation.html#add-comments-for-translators).


### UI Text Style

Use **title case** for buttons, menu items, group titles. All other text should use normal sentence case.

**Do not use periods** at the end of **short** UI strings like menu items, button labels, and checkbox texts. Use a period on tooltips and longer text elements if they contain a full sentence. Omit the period if the tooltip is a short phrase or label.

**Capitalize command and object names** using title case, even if not technical terms. This ensures UI labels remain visually consistent and scannable.

**Use ellipsis (...)** at the end of menu items when specific input from the user is required for the action.

In title case, major words are capitalized, and most minor words are lowercase.


### How to implement title case

In title case, capitalize the following words in a title or heading:

- the first word of the title or heading, even if it is a minor word such as “The” or “A”
- major words, including the second part of hyphenated major words (e.g., “Self-Report,” not “Self-report”)
- words of four letters or more (e.g., “With,” “Between,” “From”)

Lowercase only minor words that are three letters or fewer in a title or heading (except the first word in a title or subtitle or the first word after a colon, em dash, or end punctuation in a heading):

- short conjunctions (e.g., “and,” “as,” “but,” “for,” “if,” “nor,” “or,” “so,” “yet”)
- articles (“a,” “an,” “the”)
- short prepositions (e.g., “as,” “at,” “by,” “for,” “in,” “of,” “off,” “on,” “per,” “to,” “up,” “via”)
  
**Examples:**
- `Constrains new geometry automatically. It is recommended to leave it enabled for most workflows.` - Complete sentences end with a period.
- `New File` - This is just a short label. A tooltip for that command button would be "Creates a new file".
- `Save As ...` - The function requires user input to function.
- `Addon Manager` - This is not a simple action, so no ellipsis.

[Return to Design Guide Main Page](index.md)
