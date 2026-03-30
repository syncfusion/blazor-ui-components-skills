# Toolbar Configuration in Blazor Rich Text Editor

## Table of Contents
- [Enabling / Disabling the Toolbar](#enabling--disabling-the-toolbar)
- [Toolbar Types](#toolbar-types)
- [Floating Toolbar](#floating-toolbar)
- [Toolbar Position](#toolbar-position)
- [Configuring Toolbar Items](#configuring-toolbar-items)

---

## Enabling / Disabling the Toolbar

### `Enable` — `bool` — default: `true`
Specifies whether to render the toolbar in the Rich Text Editor. When set to `false`, the toolbar is hidden entirely and no editing controls are displayed.

**Hide the toolbar (render editor without any toolbar):**
```razor
<SfRichTextEditor>
    <RichTextEditorToolbarSettings Enable="false" />
</SfRichTextEditor>
```

**Show the toolbar (default behaviour — no need to set explicitly):**
```razor
<SfRichTextEditor>
    <RichTextEditorToolbarSettings Enable="true" />
</SfRichTextEditor>
```

> **Tip:** Use `Enable="false"` for read-only display scenarios. For a fully non-interactive editor, combine with `Readonly="true"` on `SfRichTextEditor`.

```razor
<!-- Read-only editor with no toolbar -->
<SfRichTextEditor Value="@HtmlContent" Readonly="true">
    <RichTextEditorToolbarSettings Enable="false" />
</SfRichTextEditor>
```
---

## Toolbar Types

Set the toolbar layout using `RichTextEditorToolbarSettings.Type`. Four types are available:

### Expand (default)
Overflowing items are hidden and revealed via an expand arrow:
```razor
<SfRichTextEditor>
    <RichTextEditorToolbarSettings Type="ToolbarType.Expand" />
</SfRichTextEditor>
```

### MultiRow
All items always visible across multiple rows:
```razor
<SfRichTextEditor>
    <RichTextEditorToolbarSettings Type="ToolbarType.MultiRow" />
</SfRichTextEditor>
```

### Scrollable
Single row with horizontal scrolling for overflow items:
```razor
<SfRichTextEditor>
    <RichTextEditorToolbarSettings Type="ToolbarType.Scrollable" />
</SfRichTextEditor>
```

### Popup
Items overflow into a popup container — good for limited screen space:
```razor
<SfRichTextEditor>
    <RichTextEditorToolbarSettings Type="ToolbarType.Popup" />
</SfRichTextEditor>
```

> **Choosing a type:** Use `Expand` for most apps. Use `MultiRow` when you want all tools always visible. Use `Scrollable` on mobile-friendly layouts. Use `Popup` for very compact UIs.

---

## Floating Toolbar

By default, the toolbar floats (stays visible) when scrolling through a tall editor. Control this with `EnableFloating` and `FloatingToolbarOffset`.

**Disable floating (toolbar scrolls away with content):**
```razor
<SfRichTextEditor Height="800px">
    <RichTextEditorToolbarSettings EnableFloating="false" />
</SfRichTextEditor>
```

**Float with offset from top of viewport (e.g., to clear a fixed header):**
```razor
<SfRichTextEditor Height="800px">
    <RichTextEditorToolbarSettings EnableFloating="true" />
    <!-- FloatingToolbarOffset is a property on SfRichTextEditor, not toolbar settings -->
</SfRichTextEditor>
```

```razor
<SfRichTextEditor Height="800px" FloatingToolbarOffset="60">
    <RichTextEditorToolbarSettings EnableFloating="true" />
</SfRichTextEditor>
```

> Set `FloatingToolbarOffset` to match your fixed header height (in pixels) to prevent toolbar overlap.

---

## Toolbar Position

Place the toolbar above or below the content area:

**Bottom toolbar:**
```razor
<SfRichTextEditor>
    <RichTextEditorToolbarSettings Position="ToolbarPosition.Bottom" />
</SfRichTextEditor>
```

**Top toolbar (default):**
```razor
<SfRichTextEditor>
    <RichTextEditorToolbarSettings Position="ToolbarPosition.Top" />
</SfRichTextEditor>
```

> Bottom toolbar is useful for chat-style or mobile editors where the content should lead.

---

## Configuring Toolbar Items

Use `RichTextEditorToolbarSettings.Items` to define which tools appear and in what order. Each item is a `ToolbarItemModel`.

**Standard item:**
```csharp
new ToolbarItemModel() { Command = ToolbarCommand.Bold }
```

**Separator (visual divider between groups):**
```csharp
new ToolbarItemModel() { Command = ToolbarCommand.Separator }
```

**Full example with grouped items:**
```razor
<SfRichTextEditor>
    <RichTextEditorToolbarSettings Items="@Tools" />
</SfRichTextEditor>

@code {
    private List<ToolbarItemModel> Tools = new()
    {
        // Text formatting group
        new() { Command = ToolbarCommand.Bold },
        new() { Command = ToolbarCommand.Italic },
        new() { Command = ToolbarCommand.Underline },
        new() { Command = ToolbarCommand.Separator },
        // Font group
        new() { Command = ToolbarCommand.FontName },
        new() { Command = ToolbarCommand.FontSize },
        new() { Command = ToolbarCommand.FontColor },
        new() { Command = ToolbarCommand.BackgroundColor },
        new() { Command = ToolbarCommand.Separator },
        // Block group
        new() { Command = ToolbarCommand.Formats },
        new() { Command = ToolbarCommand.Alignments },
        new() { Command = ToolbarCommand.OrderedList },
        new() { Command = ToolbarCommand.UnorderedList },
        new() { Command = ToolbarCommand.Separator },
        // Insert group
        new() { Command = ToolbarCommand.CreateLink },
        new() { Command = ToolbarCommand.Image },
        new() { Command = ToolbarCommand.CreateTable },
        new() { Command = ToolbarCommand.Separator },
        // Utility group
        new() { Command = ToolbarCommand.SourceCode },
        new() { Command = ToolbarCommand.FullScreen },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Undo },
        new() { Command = ToolbarCommand.Redo }
    };
}
```

### Adding a custom toolbar item

Custom items use the `Name` property and require a matching `RichTextEditorCustomToolbarItem`:
```razor
<SfRichTextEditor @ref="RteObj">
    <RichTextEditorToolbarSettings Items="@Tools">
        <RichTextEditorCustomToolbarItems>
            <RichTextEditorCustomToolbarItem Name="MyTool">
                <Template>
                    <button @onclick="OnMyToolClick">★ Star</button>
                </Template>
            </RichTextEditorCustomToolbarItem>
        </RichTextEditorCustomToolbarItems>
    </RichTextEditorToolbarSettings>
</SfRichTextEditor>

@code {
    private SfRichTextEditor RteObj;
    private List<ToolbarItemModel> Tools = new()
    {
        new() { Command = ToolbarCommand.Bold },
        new() { Name = "MyTool", TooltipText = "Insert Star" }
    };
    private async Task OnMyToolClick()
    {
        await RteObj.ExecuteCommandAsync(CommandName.InsertText, "★",
            new ExecuteCommandOption { Undo = true });
    }
}
```

### Enable/disable toolbar items programmatically

```csharp
// Enable an item
await RteObj.EnableToolbarItemAsync(new List<ToolbarItemModel>
{
    new() { Command = ToolbarCommand.Bold }
});

// Disable an item
await RteObj.DisableToolbarItemAsync(new List<ToolbarItemModel>
{
    new() { Command = ToolbarCommand.Bold }
});
```

> Add `Command = ToolbarCommand.Custom` to include custom toolbar items in enable/disable operations.
