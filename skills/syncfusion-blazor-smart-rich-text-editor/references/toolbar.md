# Toolbar Configuration in Blazor Smart Rich Text Editor
## Table of Contents
- [Enabling / Disabling the Toolbar](#enabling--disabling-the-toolbar)
- [Toolbar Types](#toolbar-types)
- [Floating Toolbar](#floating-toolbar)
- [Toolbar Position](#toolbar-position)
- [Configuring Toolbar Items](#configuring-toolbar-items)
---
## Enabling / Disabling the Toolbar
### `Enable` — `bool` — default: `true`
Specifies whether to render the toolbar in the Smart Rich Text Editor. When set to `false`, the toolbar is hidden entirely and no editing controls are displayed.
**Hide the toolbar (render editor without any toolbar):**
```razor
<SfSmartRichTextEditor>
    <RichTextEditorToolbarSettings Enable="false" />
</SfSmartRichTextEditor>
```
**Show the toolbar (default behaviour — no need to set explicitly):**
```razor
<SfSmartRichTextEditor>
    <RichTextEditorToolbarSettings Enable="true" />
</SfSmartRichTextEditor>
```
> **Tip:** Use `Enable="false"` for read-only display scenarios. For a fully non-interactive editor, combine with `Readonly="true"` on `SfSmartRichTextEditor`.
```razor
<!-- Read-only editor with no toolbar -->
<SfSmartRichTextEditor Value="@HtmlContent" Readonly="true">
    <RichTextEditorToolbarSettings Enable="false" />
</SfSmartRichTextEditor>
```
---
## Toolbar Types
Set the toolbar layout using `RichTextEditorToolbarSettings.Type`. Four types are available:
### Expand (default)
Overflowing items are hidden and revealed via an expand arrow:
```razor
<SfSmartRichTextEditor>
    <RichTextEditorToolbarSettings Type="ToolbarType.Expand" />
</SfSmartRichTextEditor>
```
### MultiRow
All items always visible across multiple rows:
```razor
<SfSmartRichTextEditor>
    <RichTextEditorToolbarSettings Type="ToolbarType.MultiRow" />
</SfSmartRichTextEditor>
```
### Scrollable
Single row with horizontal scrolling for overflow items:
```razor
<SfSmartRichTextEditor>
    <RichTextEditorToolbarSettings Type="ToolbarType.Scrollable" />
</SfSmartRichTextEditor>
```
### Popup
Items overflow into a popup container — good for limited screen space:
```razor
<SfSmartRichTextEditor>
    <RichTextEditorToolbarSettings Type="ToolbarType.Popup" />
</SfSmartRichTextEditor>
```
> **Choosing a type:** Use `Expand` for most apps. Use `MultiRow` when you want all tools always visible. Use `Scrollable` on mobile-friendly layouts. Use `Popup` for very compact UIs.
---
## Floating Toolbar
By default, the toolbar floats (stays visible) when scrolling through a tall editor. Control this with `EnableFloating` and `FloatingToolbarOffset`.
**Disable floating (toolbar scrolls away with content):**
```razor
<SfSmartRichTextEditor Height="800px">
    <RichTextEditorToolbarSettings EnableFloating="false" />
</SfSmartRichTextEditor>
```
**Float with offset from top of viewport (e.g., to clear a fixed header):**
```razor
<SfSmartRichTextEditor Height="800px">
    <RichTextEditorToolbarSettings EnableFloating="true" />
    <!-- FloatingToolbarOffset is a property on SfSmartRichTextEditor, not toolbar settings -->
</SfSmartRichTextEditor>
```
```razor
<SfSmartRichTextEditor Height="800px" FloatingToolbarOffset="60">
    <RichTextEditorToolbarSettings EnableFloating="true" />
</SfSmartRichTextEditor>
```
> Set `FloatingToolbarOffset` to match your fixed header height (in pixels) to prevent toolbar overlap.
---
## Toolbar Position
Place the toolbar above or below the content area:
**Bottom toolbar:**
```razor
<SfSmartRichTextEditor>
    <RichTextEditorToolbarSettings Position="ToolbarPosition.Bottom" />
</SfSmartRichTextEditor>
```
**Top toolbar (default):**
```razor
<SfSmartRichTextEditor>
    <RichTextEditorToolbarSettings Position="ToolbarPosition.Top" />
</SfSmartRichTextEditor>
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
<SfSmartRichTextEditor>
    <RichTextEditorToolbarSettings Items="@Tools" />
</SfSmartRichTextEditor>
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
### Adding AI toolbar items (Smart Action & Ask AI)

`SfSmartRichTextEditor` has two built-in AI toolbar buttons that are registered by **name**, not by `ToolbarCommand` enum. You **must** add them via the `Name` property — otherwise the Smart Action dropdown and Ask AI button will not appear even when `AssistViewSettings` is configured.

| `Name` | Button rendered | Notes |
|---|---|---|
| `"AI Commands"` | Smart Action dropdown | Displays the AI command menu for selected text. Driven by `AssistViewSettings.Commands`. |
| `"AI Query"` | Ask AI button | Opens the AI query popup (equivalent to `Alt`+`Enter`). |

```csharp
// ✅ Correct — AI buttons rendered
new ToolbarItemModel() { Name = "AI Commands" },
new ToolbarItemModel() { Name = "AI Query" }

// ❌ Wrong — no ToolbarCommand enum value exists for these
// new ToolbarItemModel() { Command = ToolbarCommand.AICommands }  // does not exist
```

**Full toolbar example including AI buttons:**
```razor
<SfSmartRichTextEditor>
    <RichTextEditorToolbarSettings Items="@Tools" />
    <AssistViewSettings Commands="@AiCommands" Placeholder="How can I help?" />
</SfSmartRichTextEditor>

@code {
    private List<ToolbarItemModel> Tools = new()
    {
        new() { Name = "AI Commands" },   // Smart Action dropdown — place first for visibility
        new() { Name = "AI Query" },      // Ask AI popup button
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Bold },
        new() { Command = ToolbarCommand.Italic },
        new() { Command = ToolbarCommand.Underline },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Formats },
        new() { Command = ToolbarCommand.Alignments },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Undo },
        new() { Command = ToolbarCommand.Redo }
    };

    private List<AICommands> AiCommands = new()
    {
        new() { Text = "Improve Writing", Prompt = "Improve the clarity and quality of this text." },
        new() { Text = "Fix Grammar",     Prompt = "Fix all grammar and spelling errors." },
        new() { Text = "Summarize",       Prompt = "Summarize this content concisely." }
    };
}
```

---

### Adding a custom toolbar item
Custom items use the `Name` property and require a matching `RichTextEditorCustomToolbarItem`:
```razor
<SfSmartRichTextEditor @ref="SmartRteObj">
    <RichTextEditorToolbarSettings Items="@Tools">
        <RichTextEditorCustomToolbarItems>
            <RichTextEditorCustomToolbarItem Name="MyTool">
                <Template>
                    <button @onclick="OnMyToolClick">★ Star</button>
                </Template>
            </RichTextEditorCustomToolbarItem>
        </RichTextEditorCustomToolbarItems>
    </RichTextEditorToolbarSettings>
</SfSmartRichTextEditor>
@code {
    private SfSmartRichTextEditor SmartRteObj;
    private List<ToolbarItemModel> Tools = new()
    {
        new() { Command = ToolbarCommand.Bold },
        new() { Name = "MyTool", TooltipText = "Insert Star" }
    };
    private async Task OnMyToolClick()
    {
        await SmartRteObj.ExecuteCommandAsync(CommandName.InsertText, "★",
            new ExecuteCommandOption { Undo = true });
    }
}
```
### Enable/disable toolbar items programmatically
```csharp
// Enable an item
await SmartRteObj.EnableToolbarItemAsync(new List<ToolbarItemModel>
{
    new() { Command = ToolbarCommand.Bold }
});
// Disable an item
await SmartRteObj.DisableToolbarItemAsync(new List<ToolbarItemModel>
{
    new() { Command = ToolbarCommand.Bold }
});
```
> Add `Command = ToolbarCommand.Custom` to include custom toolbar items in enable/disable operations.
```