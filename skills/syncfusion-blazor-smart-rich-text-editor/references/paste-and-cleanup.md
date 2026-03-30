# Paste and Cleanup
## Table of Contents
- [Paste Cleanup Settings](#paste-cleanup-settings)
- [Paste Modes](#paste-modes)
- [Denied Tags and Attributes](#denied-tags-and-attributes)
- [Allowed Style Properties](#allowed-style-properties)
- [Accessing Pasted Content](#accessing-pasted-content)
- [Pasting Large Content (SignalR Buffer)](#pasting-large-content-signalr-buffer)
- [Enter Key Behaviour](#enter-key-behaviour)
- [Undo / Redo Manager](#undo--redo-manager)
---
## Paste Cleanup Settings
The `RichTextEditorPasteCleanupSettings` child component controls how content pasted from Word, Outlook, Excel, or other websites is sanitised before insertion.
| Property | Type | Default | Description |
|---|---|---|---|
| `Prompt` | `bool` | `false` | Show a dialog offering Keep / Clean / Plain Text options |
| `PlainText` | `bool` | `false` | Strip all tags and paste as plain text |
| `KeepFormat` | `bool` | `true` | Retain the source formatting (filtered by `AllowedStyleProperties`) |
| `DeniedTags` | `string[]` | `null` | HTML tags to remove from pasted content |
| `DeniedAttributes` | `string[]` | `null` | HTML attributes to strip from pasted content |
| `AllowedStyleProperties` | `string[]` | (extensive list) | CSS properties to preserve when `KeepFormat` is `true` |
> **Mutual exclusivity rules:**
> - `Prompt = true` overrides `PlainText` and `KeepFormat`
> - `PlainText = true` requires `Prompt = false`; ignores `KeepFormat` and `AllowedStyleProperties`
> - `AllowedStyleProperties` is only applied when `KeepFormat = true`
---
## Paste Modes
### Prompt Dialog
Lets the user choose how to paste each time:
```razor
<SfSmartRichTextEditor>
    <RichTextEditorPasteCleanupSettings Prompt="true" />
</SfSmartRichTextEditor>
```
Prompt options:
1. **Keep** — preserve source formatting (respects `AllowedStyleProperties` / `DeniedTags`)
2. **Clean** — strip inline styles, keep structural tags
3. **Plain Text** — discard all markup
### Plain Text
Always paste without any markup:
```razor
<SfSmartRichTextEditor>
    <RichTextEditorPasteCleanupSettings PlainText="true" Prompt="false" />
</SfSmartRichTextEditor>
```
### Keep Format (Default)
Retain source formatting filtered through the allowed-style list:
```razor
<SfSmartRichTextEditor>
    <RichTextEditorPasteCleanupSettings KeepFormat="true" PlainText="false" Prompt="false" />
</SfSmartRichTextEditor>
```
### Clean Format
Strip all inline styles but keep structural HTML tags (`<p>`, `<ul>`, `<table>`, etc.):
```razor
<!-- All three flags false = clean format -->
<SfSmartRichTextEditor>
    <RichTextEditorPasteCleanupSettings Prompt="false" PlainText="false" KeepFormat="false" />
</SfSmartRichTextEditor>
```
---
## Denied Tags and Attributes
Remove specific tags or attributes from pasted content:
```razor
<SfSmartRichTextEditor>
    <RichTextEditorPasteCleanupSettings
        DeniedTags="@DeniedTags"
        DeniedAttributes="@DeniedAttrs" />
</SfSmartRichTextEditor>
@code {
    // "a"           → remove all anchor tags
    // "a[!href]"    → remove anchors that have no href
    // "a[href,target]" → remove anchors that have href AND target
    private string[] DeniedTags  = new[] { "a[!href]", "script", "style" };
    private string[] DeniedAttrs = new[] { "class", "id", "title" };
}
```
> `DeniedTags` and `DeniedAttributes` apply in both KeepFormat and Clean Format modes, but **not** when `PlainText = true` (plain text already has no tags).
---
## Allowed Style Properties
Restrict which CSS properties survive when `KeepFormat = true`:
```razor
<SfSmartRichTextEditor>
    <RichTextEditorPasteCleanupSettings
        KeepFormat="true"
        AllowedStyleProperties="@AllowedStyles" />
</SfSmartRichTextEditor>
@code {
    // Only colour and font-size will be preserved; all other inline styles are removed
    private string[] AllowedStyles = new[] { "color", "font-size", "font-weight" };
}
```
---
## Accessing Pasted Content
Use the `AfterPasteCleanup` event to inspect or modify the cleaned HTML after it has been sanitised but before it is committed:
```razor
<SfSmartRichTextEditor>
    <RichTextEditorEvents AfterPasteCleanup="@OnPasted" />
</SfSmartRichTextEditor>
@code {
    private void OnPasted(PasteCleanupArgs args)
    {
        // args.Value holds the sanitised HTML string
        Console.WriteLine("Pasted HTML: " + args.Value);
    }
}
```
---
## Pasting Large Content (SignalR Buffer)
Pasting large blocks of text over SignalR can trigger a reconnect warning. Increase the maximum receive message size in `Program.cs`:
```csharp
// Blazor Server (Program.cs)
builder.Services.AddSignalR(options =>
{
    options.MaximumReceiveMessageSize = 1_024_000_000; // ~1 GB
});
```
```csharp
// Blazor WebAssembly (Program.cs)
builder.Services.AddSignalR(options =>
{
    options.MaximumReceiveMessageSize = 1_024_000_000;
});
await builder.Build().RunAsync();
```
---
## Enter Key Behaviour
Customise the HTML tag inserted when `Enter` or `Shift+Enter` is pressed:
| Property | Default | Options | Description |
|---|---|---|---|
| `EnterKey` | `EnterKeyTag.P` | `P`, `DIV`, `BR` | Tag created on `Enter` |
| `ShiftEnterKey` | `ShiftEnterKeyTag.BR` | `BR`, `P`, `DIV` | Tag created on `Shift+Enter` |
```razor
<SfSmartRichTextEditor EnterKey="EnterKeyTag.DIV" ShiftEnterKey="ShiftEnterKeyTag.P">
    <div>Pressing Enter inserts a DIV; Shift+Enter inserts a P.</div>
</SfSmartRichTextEditor>
```
> Inside a `<pre>` (code block) tag, Enter always inserts `<br>` regardless of `EnterKey`. Press Enter twice to exit the `<pre>` block.
---
## Undo / Redo Manager
### Configure Steps and Timer
```razor
<SfSmartRichTextEditor UndoRedoSteps="50" UndoRedoTimer="400">
    <!-- Keeps 50 undo steps, records every 400 ms -->
</SfSmartRichTextEditor>
```
| Property | Default | Description |
|---|---|---|
| `UndoRedoSteps` | `30` | Number of undo/redo history entries to retain; set to `0` to disable |
| `UndoRedoTimer` | `300` | Interval (ms) between undo history snapshots |
### Clear the Stack Programmatically
```razor
<SfSmartRichTextEditor @ref="SmartRteObj">
    <p>Editor content here.</p>
</SfSmartRichTextEditor>
<button @onclick="ClearHistory">Clear History</button>
@code {
    private SfSmartRichTextEditor SmartRteObj = default!;
    private async Task ClearHistory()
    {
        await SmartRteObj.ClearUndoRedoAsync();
    }
}
```
### Track Custom Tool Actions in Undo Stack
Pass `ExecuteCommandOption { Undo = true }` to include programmatic insertions in the undo history:
```razor
@code {
    private async Task InsertTemplate()
    {
        await SmartRteObj.ExecuteCommandAsync(
            CommandName.InsertHTML,
            "<p><strong>Template text</strong></p>",
            new ExecuteCommandOption { Undo = true });
    }
}

```
```