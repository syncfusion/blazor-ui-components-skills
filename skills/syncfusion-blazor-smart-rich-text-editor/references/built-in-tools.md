# Built-in Toolbar Tools Reference

## Table of Contents
- [Default Toolbar](#default-toolbar)
- [AI-Specific Toolbar Items](#ai-specific-toolbar-items)
- [Text Formatting Tools](#text-formatting-tools)
- [Font and Styling Tools](#font-and-styling-tools)
- [Alignment Tools](#alignment-tools)
- [Lists and Indentation](#lists-and-indentation)
- [Hyperlink Tools](#hyperlink-tools)
- [Image Quick Toolbar Commands](#image-quick-toolbar-commands)
- [Table Quick Toolbar Commands](#table-quick-toolbar-commands)
- [Utility Tools](#utility-tools)
- [Removing Default Tools](#removing-default-tools)

---

## Default Toolbar

Out of the box, the Smart Rich Text Editor renders these toolbar items when no `Items` list is specified:

`Bold | Italic | Underline | | Formats | Alignments | Blockquote | OrderedList | UnorderedList | | CreateLink | Image | | SourceCode | Undo | Redo`

---

## AI-Specific Toolbar Items

`SfSmartRichTextEditor` provides two built-in AI toolbar buttons that are **not** part of the `ToolbarCommand` enum. They must be added using the `Name` property on `ToolbarItemModel`.

| `Name` value | Renders | Behaviour |
|---|---|---|
| `"AI Commands"` | Smart Action dropdown button | Shows the AI command menu (Summarize, Expand, Fix Grammar, Change Tone, etc.) for the selected text. Configured via `AssistViewSettings.Commands`. |
| `"AI Query"` | Ask AI button | Opens the AI Query dialog popup (same as pressing `Alt`+`Enter`). |

> **Critical:** Using `ToolbarCommand` enum values will **not** render these buttons. You must use `Name = "AI Commands"` and `Name = "AI Query"` exactly (case-sensitive).

### Example — adding AI buttons to a custom toolbar

```razor
<SfSmartRichTextEditor>
    <RichTextEditorToolbarSettings Items="@Tools" />
    <AssistViewSettings Commands="@AiCommands"
                        Placeholder="How can I help?" />
</SfSmartRichTextEditor>

@code {
    private List<ToolbarItemModel> Tools = new()
    {
        // ⬇ AI-specific items — MUST use Name, not Command; place first for visibility
        new() { Name = "AI Commands" },
        new() { Name = "AI Query" },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Bold },
        new() { Command = ToolbarCommand.Italic },
        new() { Command = ToolbarCommand.Underline },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Formats },
        new() { Command = ToolbarCommand.Alignments },
        new() { Command = ToolbarCommand.OrderedList },
        new() { Command = ToolbarCommand.UnorderedList },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Undo },
        new() { Command = ToolbarCommand.Redo }
    };

    private List<AICommands> AiCommands = new()
    {
        new() { Text = "Improve Writing",  Prompt = "Improve the clarity and quality of this text." },
        new() { Text = "Fix Grammar",      Prompt = "Fix all grammar and spelling errors." },
        new() { Text = "Summarize",        Prompt = "Summarize this content concisely." },
        new() { Text = "Expand",           Prompt = "Expand this with more detail and examples." }
    };
}
```

> If you omit `Name = "AI Commands"` and `Name = "AI Query"` from `Items`, the Smart Action dropdown and Ask AI button will **not appear** in the toolbar even though `AssistViewSettings` is configured.

---

## Text Formatting Tools
| `ToolbarCommand` | Effect |
|---|---|
| `Bold` | Makes selected text bold (`<strong>`) |
| `Italic` | Italicises selected text (`<em>`) |
| `Underline` | Underlines selected text |
| `StrikeThrough` | Applies strikethrough |
| `InlineCode` | Wraps selection in `<code>` |
| `SubScript` | Subscript — positions text lower |
| `SuperScript` | Superscript — positions text higher |
| `LowerCase` | Converts selection to lowercase |
| `UpperCase` | Converts selection to uppercase |
| `ClearFormat` | Strips all inline styles from selection |
---

## Font and Styling Tools
| `ToolbarCommand` | Effect |
|---|---|
| `FontName` | Font family dropdown |
| `FontSize` | Font size dropdown |
| `FontColor` | Font colour picker |
| `BackgroundColor` | Highlight / background colour picker |
| `Formats` | Paragraph/heading format dropdown (P, H1–H6, Pre, BlockQuote) |
| `LineHeight` | Line-height dropdown; applies to parent block |
---

## Alignment Tools
| `ToolbarCommand` | Effect |
|---|---|
| `Alignments` | Combined alignment dropdown |
| `JustifyLeft` | Left-align |
| `JustifyCenter` | Centre-align |
| `JustifyRight` | Right-align |
| `JustifyFull` | Justify |
> Alignment is applied to the **entire block** that contains the cursor, not just selected inline text.
---

## Lists and Indentation
| `ToolbarCommand` | Effect |
|---|---|
| `OrderedList` | Toggle numbered list |
| `UnorderedList` | Toggle bulleted list |
| `NumberFormatList` | Numbered list with style picker (decimal, roman, alpha, …) |
| `BulletFormatList` | Bulleted list with style picker (disc, square, circle, …) |
| `Indent` | Increase indentation / nest list item |
| `Outdent` | Decrease indentation / un-nest list item |
---

## Hyperlink Tools
**Main toolbar:**
| `ToolbarCommand` | Effect |
|---|---|
| `CreateLink` | Open insert-link dialog |
**Link quick toolbar** — configure via `RichTextEditorQuickToolbarSettings.Link`:
| Command | Effect |
|---|---|
| `LinkToolbarCommand.Open` | Open the linked URL |
| `LinkToolbarCommand.Edit` | Edit the link href/text |
| `LinkToolbarCommand.UnLink` | Remove hyperlink |
```razor
<SfSmartRichTextEditor>
    <RichTextEditorQuickToolbarSettings Link="@LinkTools" />
</SfSmartRichTextEditor>
@code {
    private List<LinkToolbarItemModel> LinkTools = new()
    {
        new() { Command = LinkToolbarCommand.Open },
        new() { Command = LinkToolbarCommand.Edit },
        new() { Command = LinkToolbarCommand.UnLink }
    };
}
```
---

## Image Quick Toolbar Commands
Configure via `RichTextEditorQuickToolbarSettings.Image`:
| Command | Effect |
|---|---|
| `ImageToolbarCommand.Replace` | Swap image with another |
| `ImageToolbarCommand.Align` | Align image left / centre / right |
| `ImageToolbarCommand.Caption` | Wrap image in figure/caption |
| `ImageToolbarCommand.Remove` | Delete image from content |
| `ImageToolbarCommand.OpenImageLink` | Follow attached hyperlink |
| `ImageToolbarCommand.EditImageLink` | Edit attached hyperlink |
| `ImageToolbarCommand.RemoveImageLink` | Remove attached hyperlink |
| `ImageToolbarCommand.Display` | Toggle inline / block display |
| `ImageToolbarCommand.AltText` | Edit alternative text |
| `ImageToolbarCommand.Dimension` | Set width/height in px |
```razor
<SfSmartRichTextEditor>
    <RichTextEditorQuickToolbarSettings Image="@ImageTools" />
</SfSmartRichTextEditor>
@code {
    private List<ImageToolbarItemModel> ImageTools = new()
    {
        new() { Command = ImageToolbarCommand.Replace },
        new() { Command = ImageToolbarCommand.Align },
        new() { Command = ImageToolbarCommand.Caption },
        new() { Command = ImageToolbarCommand.Remove },
        new() { Command = ImageToolbarCommand.HorizontalSeparator },
        new() { Command = ImageToolbarCommand.AltText },
        new() { Command = ImageToolbarCommand.Dimension }
    };
}
```
---

## Table Quick Toolbar Commands
Configure via `RichTextEditorQuickToolbarSettings.Table`:
| Command | Effect |
|---|---|
| `TableToolbarCommand.TableHeader` | Toggle table header row |
| `TableToolbarCommand.TableColumns` | Insert/delete column dropdown |
| `TableToolbarCommand.TableRows` | Insert/delete row dropdown |
| `TableToolbarCommand.TableCell` | Merge / split cells |
| `TableToolbarCommand.TableCellHorizontalAlign` | Horizontal cell alignment |
| `TableToolbarCommand.TableCellVerticalAlign` | Vertical cell alignment |
| `TableToolbarCommand.TableEditProperties` | Edit width, padding, spacing |
| `TableToolbarCommand.RemoveTable` | Delete entire table |
---

## Utility Tools
| `ToolbarCommand` | Effect |
|---|---|
| `SourceCode` | Toggle HTML source view |
| `Preview` | Show render preview |
| `FullScreen` | Maximise editor to viewport |
| `Print` | Print editor content |
| `FormatPainter` | Copy formatting; double-click for sticky mode |
| `Undo` | Undo last action |
| `Redo` | Redo last undone action |
| `Separator` | Visual divider between toolbar groups |
---

## Removing Default Tools
Pass only the tools you want — the default set is replaced entirely:
```razor
<SfSmartRichTextEditor>
    <RichTextEditorToolbarSettings Items="@MinimalTools" />
</SfSmartRichTextEditor>
@code {
    private List<ToolbarItemModel> MinimalTools = new()
    {
        new() { Command = ToolbarCommand.Bold },
        new() { Command = ToolbarCommand.Italic },
        new() { Command = ToolbarCommand.Underline },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Undo },
        new() { Command = ToolbarCommand.Redo }
    };
}
```
> Omitting a tool from `Items` removes it; you don't need any separate "remove" call.
```