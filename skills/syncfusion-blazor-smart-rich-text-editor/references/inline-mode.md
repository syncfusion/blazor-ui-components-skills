# Inline Mode
## Table of Contents
- [Overview](#overview)
- [Enabling Inline Mode](#enabling-inline-mode)
- [Show Toolbar on Selection Only](#show-toolbar-on-selection-only)
- [Customising the Inline Toolbar Items](#customising-the-inline-toolbar-items)
- [Use Cases](#use-cases)
---
## Overview
Inline mode hides the toolbar by default and shows a **floating toolbar** when the user focuses or selects text inside the editor. It is ideal for in-place editing scenarios where the toolbar should not take up permanent screen real estate.
---
## Enabling Inline Mode
Add `<RichTextEditorInlineMode Enable="true" />` inside the editor. The toolbar appears as soon as the user clicks into the editable area:
```razor
@using Syncfusion.Blazor.SmartRichTextEditor
@using Syncfusion.Blazor.RichTextEditor
<SfSmartRichTextEditor>
    <RichTextEditorInlineMode Enable="true" ShowOnSelection="false" />
    <p>
        The editor is in inline mode. Click anywhere in this text to reveal the
        formatting toolbar.
    </p>
</SfSmartRichTextEditor>
```
| Property | Type | Default | Description |
|---|---|---|---|
| `Enable` | `bool` | `false` | Activates inline (floating) toolbar mode |
| `ShowOnSelection` | `bool` | `false` | When `true`, toolbar only appears when text is selected (not on focus) |
---
## Show Toolbar on Selection Only
Set `ShowOnSelection="true"` to keep the toolbar hidden until the user actually selects some text. This is the most unobtrusive option:
```razor
<SfSmartRichTextEditor>
    <RichTextEditorToolbarSettings Items="@Tools" />
    <RichTextEditorInlineMode Enable="true" ShowOnSelection="true" />
    <p>
        Select any text in this paragraph to see the inline formatting toolbar appear
        at the cursor.
    </p>
</SfSmartRichTextEditor>
@code {
    private List<ToolbarItemModel> Tools = new()
    {
        new() { Command = ToolbarCommand.Bold },
        new() { Command = ToolbarCommand.Italic },
        new() { Command = ToolbarCommand.Underline },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.CreateLink },
        new() { Command = ToolbarCommand.Image }
    };
}
```
> The floating toolbar anchors near the selection and repositions automatically when the user scrolls or the selection changes.
---
## Customising the Inline Toolbar Items
Inline mode uses the same `RichTextEditorToolbarSettings.Items` list as regular mode. Limit the list to frequently used commands to keep the floating bar compact:
```razor
<SfSmartRichTextEditor>
    <RichTextEditorToolbarSettings Items="@InlineTools" />
    <RichTextEditorInlineMode Enable="true" ShowOnSelection="true" />
    <p>Rich in-place editing with a minimal floating toolbar.</p>
</SfSmartRichTextEditor>
@code {
    private List<ToolbarItemModel> InlineTools = new()
    {
        new() { Command = ToolbarCommand.Bold },
        new() { Command = ToolbarCommand.Italic },
        new() { Command = ToolbarCommand.Underline },
        new() { Command = ToolbarCommand.StrikeThrough },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.FontColor },
        new() { Command = ToolbarCommand.BackgroundColor },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Alignments },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.CreateLink },
        new() { Command = ToolbarCommand.ClearFormat }
    };
}
```
---
## Use Cases
| Scenario | Recommended Configuration |
|---|---|
| Blog post editor with minimal UI | `Enable="true"`, `ShowOnSelection="false"` |
| Document viewer with in-place annotation | `Enable="true"`, `ShowOnSelection="true"` |
| Comment / note editing inside a card | `Enable="true"`, `ShowOnSelection="true"` + condensed `Items` list |
| Full-page content editor | Standard mode (no `RichTextEditorInlineMode`) |
> Inline mode can be combined with a **read-only** initial state. Use `Readonly="true"` by default and programmatically toggle it on a double-click via `@ref` to create a click-to-edit pattern.
```razor
<SfSmartRichTextEditor @ref="SmartRteObj" Readonly="@IsReadonly" @ondblclick="EnableEdit">
    <RichTextEditorInlineMode Enable="true" ShowOnSelection="true" />
    <p>Double-click to start editing.</p>
</SfSmartRichTextEditor>
@code {
    private SfSmartRichTextEditor SmartRteObj = default!;
    private bool IsReadonly = true;
    private void EnableEdit() => IsReadonly = false;
}

```
```