# Text Formatting

## Table of Contents
- [Basic Inline Styles](#basic-inline-styles)
- [Text Alignment](#text-alignment)
- [Heading and Paragraph Formats](#heading-and-paragraph-formats)
- [Number and Bullet Lists](#number-and-bullet-lists)
- [Indentation](#indentation)
- [Blockquotes](#blockquotes)
- [Line Height](#line-height)
- [Horizontal Line](#horizontal-line)
- [Format Painter](#format-painter)
- [Clear Format](#clear-format)
- [Markdown Auto-Format](#markdown-auto-format)

---

## Basic Inline Styles

Add these `ToolbarCommand` values to expose inline text style buttons:

```razor
<SfRichTextEditor>
    <RichTextEditorToolbarSettings Items="@Items" />
</SfRichTextEditor>

@code {
    private List<ToolbarItemModel> Items = new()
    {
        new() { Command = ToolbarCommand.Bold },
        new() { Command = ToolbarCommand.Italic },
        new() { Command = ToolbarCommand.Underline },
        new() { Command = ToolbarCommand.StrikeThrough },
        new() { Command = ToolbarCommand.InlineCode },
        new() { Command = ToolbarCommand.SubScript },
        new() { Command = ToolbarCommand.SuperScript },
        new() { Command = ToolbarCommand.LowerCase },
        new() { Command = ToolbarCommand.UpperCase }
    };
}
```

| Command | Output tag |
|---|---|
| `Bold` | `<strong>` |
| `Italic` | `<em>` |
| `Underline` | `<u>` |
| `StrikeThrough` | `<s>` |
| `InlineCode` | `<code>` |
| `SubScript` | `<sub>` |
| `SuperScript` | `<sup>` |

---

## Text Alignment

Alignment is applied to the **full parent block element** — not just the selected text.

```razor
<SfRichTextEditor>
    <RichTextEditorToolbarSettings Items="@Items" />
</SfRichTextEditor>

@code {
    private List<ToolbarItemModel> Items = new()
    {
        new() { Command = ToolbarCommand.Alignments }
    };
}
```

The `Alignments` command exposes a dropdown with JustifyLeft / JustifyCenter / JustifyRight / JustifyFull. You can also add individual commands like `ToolbarCommand.JustifyLeft` directly.

---

## Heading and Paragraph Formats

The `Formats` dropdown applies block-level formatting (paragraph, headings, pre, blockquote).

**Built-in items:** `Paragraph`, `Code` (`<pre>`), `Quotation` (`<blockquote>`), `Heading 1`–`Heading 4`.

**Extend with custom formats:**

```razor
<SfRichTextEditor>
    <RichTextEditorToolbarSettings Items="@Tools" />
    <RichTextEditorFormat Items="@FormatItems" />
</SfRichTextEditor>

@code {
    private List<ToolbarItemModel> Tools = new()
    {
        new() { Command = ToolbarCommand.Formats }
    };

    private List<DropDownItemModel> FormatItems = new()
    {
        new() { Text = "Paragraph", Value = "P" },
        new() { Text = "Code", Value = "Pre" },
        new() { Text = "Quotation", Value = "BlockQuote" },
        new() { Text = "Heading 1", Value = "H1" },
        new() { Text = "Heading 2", Value = "H2" },
        new() { Text = "Heading 3", Value = "H3" },
        new() { Text = "Heading 4", Value = "H4" },
        new() { Text = "Heading 5", Value = "H5" },
        new() { Text = "Heading 6", Value = "H6" }
    };
}
```

---

## Number and Bullet Lists

### Quick toggle
```razor
new() { Command = ToolbarCommand.OrderedList }   // numbered
new() { Command = ToolbarCommand.UnorderedList } // bulleted
```

### With style picker

```razor
<SfRichTextEditor>
    <RichTextEditorToolbarSettings Items="@Tools" />
    <RichTextEditorNumberFormatList Items="@NumberItems" />
    <RichTextEditorBulletFormatList Items="@BulletItems" />
</SfRichTextEditor>

@code {
    private List<ToolbarItemModel> Tools = new()
    {
        new() { Command = ToolbarCommand.NumberFormatList },
        new() { Command = ToolbarCommand.BulletFormatList }
    };

    private List<DropDownItemModel> NumberItems = new()
    {
        new() { Text = "Number",      Value = "decimal" },
        new() { Text = "Lower Alpha", Value = "lower-alpha" },
        new() { Text = "Upper Alpha", Value = "upper-alpha" },
        new() { Text = "Lower Roman", Value = "lower-roman" },
        new() { Text = "Upper Roman", Value = "upper-roman" }
    };

    private List<DropDownItemModel> BulletItems = new()
    {
        new() { Text = "Disc",   Value = "disc" },
        new() { Text = "Circle", Value = "circle" },
        new() { Text = "Square", Value = "square" }
    };
}
```

---

## Indentation

Use `Indent` (increase) and `Outdent` (decrease) toolbar buttons, or `Tab` / `Shift+Tab` inside a list item to create nested lists.

```razor
new() { Command = ToolbarCommand.Indent },
new() { Command = ToolbarCommand.Outdent }
```

---

## Blockquotes

The `Blockquote` toolbar command wraps selected text in `<blockquote>`. Nested blockquotes can be inserted via Source Code view or by pasting pre-formatted content.

```razor
new() { Command = ToolbarCommand.Blockquote }
```

---

## Line Height

`LineHeight` applies `line-height` as an inline style to the parent block. Supports undo/redo.

```razor
<SfRichTextEditor>
    <RichTextEditorToolbarSettings Items="@Tools" />
    <RichTextEditorLineHeight Items="@LineHeightItems" Default="Default" />
</SfRichTextEditor>

@code {
    private List<ToolbarItemModel> Tools = new()
    {
        new() { Command = ToolbarCommand.LineHeight }
    };

    private List<DropDownItemModel> LineHeightItems = new()
    {
        new() { Text = "Default", Value = "" },
        new() { Text = "1",       Value = "1" },
        new() { Text = "1.15",    Value = "1.15" },
        new() { Text = "1.5",     Value = "1.5" },
        new() { Text = "2",       Value = "2" }
    };
}
```

---

## Horizontal Line

Inserts an `<hr>` element at the cursor position:

```razor
new() { Command = ToolbarCommand.HorizontalLine }
```

---

## Format Painter

Copies formatting from one selection and applies it to another.

- **Single click** → one-shot apply.
- **Double click** → sticky mode; stays active until `Esc` is pressed.
- **Keyboard:** `Alt+Shift+C` to copy format, `Alt+Shift+V` to paste.

```razor
new() { Command = ToolbarCommand.FormatPainter }
```

Configure which CSS properties are captured with `RichTextEditorFormatPainterSettings`.

---

## Clear Format

Removes all inline styles (bold, italic, colour, font, etc.) from the selected text:

```razor
new() { Command = ToolbarCommand.ClearFormat }
```

---

## Markdown Auto-Format

When `EnableMarkdownAutoFormat="true"`, typing markdown shortcuts converts them to HTML in real time — **no need to switch to Markdown editor mode**.

**Inline shortcuts:**
| Type | Result |
|---|---|
| `**text**` or `__text__` | Bold |
| `*text*` or `_text_` | Italic |
| `` `text` `` | Inline code |
| `~~text~~` | Strikethrough |

**Block shortcuts (typed at the start of a new line):**
| Type | Result |
|---|---|
| `# ` | Heading 1 |
| `## ` | Heading 2 |
| `### ` | Heading 3 |
| `* ` or `- ` | Unordered list |
| `1. ` | Ordered list |
| `> ` | Blockquote |
| ` ``` ` + space | Code block |
| `---` + space | Horizontal line |

```razor
<SfRichTextEditor EnableMarkdownAutoFormat="true">
    <p>Type ** around text for <b>bold</b>, or start a line with # for a heading.</p>
</SfRichTextEditor>
```
