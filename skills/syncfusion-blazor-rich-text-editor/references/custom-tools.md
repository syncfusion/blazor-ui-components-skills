# Custom Tools

## Table of Contents
- [Custom Toolbar Items](#custom-toolbar-items)
- [ExecuteCommandAsync](#executecommandasync)
- [Slash Commands](#slash-commands)
- [Enabling and Disabling Toolbar Items](#enabling-and-disabling-toolbar-items)

---

## Custom Toolbar Items

Use `RichTextEditorCustomToolbarItems` inside `RichTextEditorToolbarSettings` to add buttons with arbitrary Blazor content (text labels, icons, or full templates). Reference the custom item in `Items` by its `Name`:

```razor
@using Syncfusion.Blazor.Buttons
@using Syncfusion.Blazor.RichTextEditor

<SfRichTextEditor @ref="RteObj">
    <RichTextEditorToolbarSettings Items="@Tools">
        <RichTextEditorCustomToolbarItems>
            <RichTextEditorCustomToolbarItem Name="Symbol">
                <Template>
                    <SfButton @onclick="InsertSymbol">Ω</SfButton>
                </Template>
            </RichTextEditorCustomToolbarItem>
        </RichTextEditorCustomToolbarItems>
    </RichTextEditorToolbarSettings>
    <p>Click <b>Ω</b> to insert a special character at the cursor.</p>
</SfRichTextEditor>

@code {
    private SfRichTextEditor RteObj = default!;

    private List<ToolbarItemModel> Tools = new()
    {
        new() { Command = ToolbarCommand.Bold },
        new() { Command = ToolbarCommand.Italic },
        new() { Command = ToolbarCommand.Separator },
        new() { Name = "Symbol", TooltipText = "Insert Symbol" },   // custom item
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.SourceCode },
        new() { Command = ToolbarCommand.FullScreen }
    };

    private async Task InsertSymbol()
    {
        await RteObj.ExecuteCommandAsync(
            CommandName.InsertText,
            "₹",
            new ExecuteCommandOption { Undo = true });  // add to undo stack
    }
}
```

**Key points:**
- `Name` in `ToolbarItemModel` must match `Name` in `RichTextEditorCustomToolbarItem`
- `TooltipText` sets the accessible tooltip
- Blazor components (buttons, dropdowns, etc.) are valid template content

---

## ExecuteCommandAsync

`SfRichTextEditor.ExecuteCommandAsync` applies formatting or inserts content at the current cursor position programmatically.

### HTML Editor Commands

| Command | Signature | Description |
|---|---|---|
| `Bold` | `ExecuteCommandAsync(CommandName.Bold)` | Toggle bold |
| `Italic` | `ExecuteCommandAsync(CommandName.Italic)` | Toggle italic |
| `Underline` | `ExecuteCommandAsync(CommandName.Underline)` | Toggle underline |
| `StrikeThrough` | `ExecuteCommandAsync(CommandName.StrikeThrough)` | Toggle strikethrough |
| `Superscript` | `ExecuteCommandAsync(CommandName.Superscript)` | Toggle superscript |
| `Subscript` | `ExecuteCommandAsync(CommandName.Subscript)` | Toggle subscript |
| `Uppercase` | `ExecuteCommandAsync(CommandName.Uppercase)` | Convert to uppercase |
| `Lowercase` | `ExecuteCommandAsync(CommandName.Lowercase)` | Convert to lowercase |
| `FontColor` | `ExecuteCommandAsync(CommandName.FontColor, "Red")` | Set font colour |
| `FontName` | `ExecuteCommandAsync(CommandName.FontName, "Impact")` | Set font family |
| `FontSize` | `ExecuteCommandAsync(CommandName.FontSize, "10pt")` | Set font size |
| `BackgroundColor` | `ExecuteCommandAsync(CommandName.BackgroundColor, "yellow")` | Set highlight colour |
| `JustifyCenter` | `ExecuteCommandAsync(CommandName.JustifyCenter)` | Centre-align |
| `JustifyLeft` | `ExecuteCommandAsync(CommandName.JustifyLeft)` | Left-align |
| `JustifyRight` | `ExecuteCommandAsync(CommandName.JustifyRight)` | Right-align |
| `JustifyFull` | `ExecuteCommandAsync(CommandName.JustifyFull)` | Justify |
| `Indent` | `ExecuteCommandAsync(CommandName.Indent)` | Increase indent |
| `Outdent` | `ExecuteCommandAsync(CommandName.Outdent)` | Decrease indent |
| `InsertText` | `ExecuteCommandAsync(CommandName.InsertText, "text")` | Insert plain text |
| `InsertHTML` | `ExecuteCommandAsync(CommandName.InsertHTML, "<b>hi</b>")` | Insert HTML at cursor |
| `InsertOrderedList` | `ExecuteCommandAsync(CommandName.InsertOrderedList)` | Start numbered list |
| `InsertUnorderedList` | `ExecuteCommandAsync(CommandName.InsertUnorderedList)` | Start bullet list |
| `NumberFormatList` | `ExecuteCommandAsync(CommandName.NumberFormatList, "Decimal")` | Styled numbered list |
| `BulletFormatList` | `ExecuteCommandAsync(CommandName.BulletFormatList, "Disc")` | Styled bullet list |
| `RemoveFormat` | `ExecuteCommandAsync(CommandName.RemoveFormat)` | Strip all formatting |
| `CreateLink` | `ExecuteCommandAsync(CommandName.CreateLink, new LinkCommandsArgs { Text="Link", Url="https://..." })` | Insert hyperlink |
| `InsertImage` | `ExecuteCommandAsync(CommandName.InsertImage, new ImageCommandsArgs { Url="...", CssClass="rte-img" })` | Insert image |
| `Undo` | `ExecuteCommandAsync(CommandName.Undo)` | Undo last action |
| `Redo` | `ExecuteCommandAsync(CommandName.Redo)` | Redo last undone action |

**`ExecuteCommandOption`:** Pass `new ExecuteCommandOption { Undo = true }` to ensure programmatic changes are tracked in the undo stack.

```razor
@code {
    private async Task ApplyYellow()
    {
        await RteObj.ExecuteCommandAsync(
            CommandName.BackgroundColor,
            "yellow",
            new ExecuteCommandOption { Undo = true });
    }
}
```

---

## Slash Commands

Slash commands show a floating suggestion menu when the user types `/` at the start of a line.

### Enabling with Built-in Items

```razor
<SfRichTextEditor Placeholder="Type / for commands">
    <RichTextEditorSlashMenuSettings Enable="true" />
</SfRichTextEditor>
```

### Custom Slash Menu Items

Supply an `Items` list. Built-in items use `SlashMenuCommand`; custom items use freeform properties:

```razor
<SfRichTextEditor @ref="RteObj" Placeholder="Type '/' and choose format">
    <RichTextEditorToolbarSettings Items="@Tools" />
    <RichTextEditorEvents SlashMenuItemSelecting="OnSlashSelect" />
    <RichTextEditorSlashMenuSettings Enable="true" Items="@SlashItems" />
</SfRichTextEditor>

@code {
    private SfRichTextEditor RteObj = default!;

    private List<SlashMenuItemModel> SlashItems = new()
    {
        // Built-in items
        new() { Command = SlashMenuCommand.Heading1 },
        new() { Command = SlashMenuCommand.Heading2 },
        new() { Command = SlashMenuCommand.Paragraph },
        new() { Command = SlashMenuCommand.OrderedList },
        new() { Command = SlashMenuCommand.UnorderedList },
        new() { Command = SlashMenuCommand.Blockquote },
        new() { Command = SlashMenuCommand.CodeBlock },
        new() { Command = SlashMenuCommand.Table },
        new() { Command = SlashMenuCommand.Image },
        // Custom items
        new() {
            Text        = "Meeting Notes",
            GroupBy     = "Custom",
            IconCss     = "e-icons e-description",
            Description = "Insert a meeting note template."
        },
        new() {
            Text        = "Signature",
            GroupBy     = "Custom",
            IconCss     = "e-icons e-signature",
            Description = "Insert a signature block."
        }
    };

    private async Task OnSlashSelect(SlashMenuSelectEventArgs args)
    {
        string html = args.ItemData.Text switch
        {
            "Meeting Notes" => "<p><strong>Meeting Notes</strong></p><table>...</table>",
            "Signature"     => "<p>Warm regards,<br/>John Doe</p>",
            _               => string.Empty
        };

        if (!string.IsNullOrEmpty(html))
            await RteObj.ExecuteCommandAsync(CommandName.InsertHTML, html);
    }
}
```

**`SlashMenuItemModel` properties:**

| Property | Description |
|---|---|
| `Command` | Built-in `SlashMenuCommand` value (built-in items only) |
| `Text` | Display label |
| `GroupBy` | Group header in the menu |
| `IconCss` | CSS class for the icon |
| `Description` | Short description shown below the label |

---

## Enabling and Disabling Toolbar Items

Use `EnableToolbarItemAsync` / `DisableToolbarItemAsync` to toggle built-in or custom items at runtime:

```razor
<SfRichTextEditor @ref="RteObj">
    <RichTextEditorToolbarSettings Items="@Tools">
        <RichTextEditorCustomToolbarItems>
            <RichTextEditorCustomToolbarItem Name="MyTool">
                <Template><SfButton>Custom</SfButton></Template>
            </RichTextEditorCustomToolbarItem>
        </RichTextEditorCustomToolbarItems>
    </RichTextEditorToolbarSettings>
</SfRichTextEditor>

@code {
    private SfRichTextEditor RteObj = default!;

    // Disable Bold and the custom "MyTool" item
    private async Task DisableItems()
    {
        await RteObj.DisableToolbarItemAsync(new List<ToolbarItemModel>
        {
            new() { Command = ToolbarCommand.Bold },
            new() { Name = "MyTool" }          // use "Custom" command name for custom items
        });
    }

    private async Task EnableItems()
    {
        await RteObj.EnableToolbarItemAsync(new List<ToolbarItemModel>
        {
            new() { Command = ToolbarCommand.Bold },
            new() { Name = "MyTool" }
        });
    }
}
```

> For custom toolbar items, set `Command = ToolbarCommand.Custom` in the model passed to these methods to correctly target them during source-code-view and quick-toolbar operations.
