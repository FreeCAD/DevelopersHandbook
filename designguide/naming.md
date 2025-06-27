## Naming Convention and Writing Style

Because FreeCAD involves both CAD and programming, it's easy to use overly technical or redundant language. Names that seem obvious to developers may confuse general users. Additionally, with limited UI space, concise and clear naming is critical for usability. Follow the guidelines below when naming or labeling functions, features, and add-ons.

> **Note:** While translations exist, English is the reference standard. Translators should follow these guidelines as closely as possible for the target language.

Avoid verbose or inconsistent language in the UI, as it may cause layout issues across workbenches and dialogs. Always review naming before merging UI-related pull requests.

### Language

- Use short, descriptive terms. Prefer single nouns when possible: `Additive Cube`, not “Create new Additive Cube”.
- Favor unique noun-first naming: `Horizontal Constraint`, not “Constrain Horizontal”.
- Avoid unnecessary verbs in command names (`New File`, not “Create new file”). Use verbs only if clarity requires them (e.g. `Insert Image`).
- For items with optional plurality use plural language: e.g. `Creates holes based on the selected profiles` not "Creates hole(s) based on the selected profile(s)".
- Do not add additional information which is obvious or list multiple steps like "Select one or more line(s) click this button, add dimension". Better: `Insert dimension for the selected lines`.
- Acronyms like `CAD` or `ISO` are allowed but should be reviewed for clarity.
- Use standard numerals (`0, 1, 2,...`) not spelled out ("one, two,...")
- Keep terminology consistent with existing FreeCAD concepts.
- Avoid programmer jargon in favor of simpler terminology.
- For all user-facing strings add comments to help adding context for translators `//: Sketcher: Command name for new line`. [More info]([Writing Source Code for Translation | Qt 6.9](https://doc.qt.io/qt-6/i18n-source-translation.html#add-comments-for-translators)).
- Use **third-person present tense** in tooltips: `Creates`, `Adds`, `Inserts`. Write complete sentences where appropriate.
- Tooltips should describe the action and required context. E.g., `Adds a new sketch to the active body`, not just “Add sketch”.
- Avoid step-by-step instructions in tooltips. Focus on outcome: `Inserts a dimension for the selected lines`, not “Select one or more lines...”.
- Avoid stating the obvious: "If checked by the user, the view is updated automatically", use `Updates view atomatically`.
- Don’t address the user directly: say what the command does, not “Click this button...”.

### UI Text Style

Use **title case** only for command names, buttons, menu items, and group titles. All other text should use normal sentence case.

**Do not use periods** on short UI elements like buttons or labels. Use them only in full-sentence tooltips or descriptive text.

**Capitalize command and object names** using title case, even if not technical terms.

**Use ellipsis (...)** at the end of menu items that require user input.

### How to implement title case

In title case, capitalize the following words:

- the first word of the title or heading, even if it is a minor word such as “The” or “A”
- major words, including the second part of hyphenated major words (e.g., “Self-Report,” not “Self-report”)
- words of four letters or more (e.g., “With,” “Between,” “From”)
- capitalize the last word in the label, regardless of the part of speech.

Lowercase only minor words that are three letters or fewer:

- short conjunctions (e.g., “and,” “as,” “but,” “for,” “if,” “nor,” “or,” “so,” “yet”)
- articles (“a,” “an,” “the”)
- short prepositions (e.g., “as,” “at,” “by,” “for,” “in,” “of,” “off,” “on,” “per,” “to,” “up,” “via”)

**Examples:**

- Tooltip: `Constrains new geometry automatically. It is recommended to leave it enabled for most workflows.` - Complete sentences end with a period.
- Hint: `Toggles the visibility of the active sketch`
- Command: `New File` - This is just a short label. A tooltip for that command button would be "Creates a new file".
- Command: `Save As ...` - The function requires user input to function.
- Command: `Addon Manager` - This is not a simple action, so no ellipsis.

[Return to Design Guide Main Page](index.md)
