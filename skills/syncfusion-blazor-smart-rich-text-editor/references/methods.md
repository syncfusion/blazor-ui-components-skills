# SfSmartRichTextEditor – Public Methods
## Table of Contents
- [Focus & Selection](#focus--selection)
- [Content Retrieval](#content-retrieval)
- [Command Execution](#command-execution)
- [Toolbar Control](#toolbar-control)
- [Dialog & UI Control](#dialog--ui-control)
- [Undo / Redo](#undo--redo)
- [CommandName Enum Reference](#commandname-enum-reference)
- [ToolbarCommand Enum Reference](#toolbarcommand-enum-reference)
- [DialogType Enum Reference](#dialogtype-enum-reference)
---
## Focus & Selection
**`FocusAsync()`**
Sets the focus on the editor, enabling user interaction.
**Returns:** `Task`
**`FocusOutAsync()`**
Removes the focus from the editor.
**Returns:** `Task`
**`SaveSelectionAsync()`**
Saves the current selection range. Call before any async operation that might move focus, then restore with `RestoreSelectionAsync()`.
**Returns:** `Task`
**`RestoreSelectionAsync()`**
Restores the previously saved selection range.
**Returns:** `Task`
**`SelectAllAsync()`**
Selects all content in the editor.
**Returns:** `Task`
**`GetSelectionAsync()`**
Retrieves the HTML markup of the currently selected content.
**Returns:** `Task<string>`
---
## Content Retrieval
**`GetTextAsync()`**
Gets the plain-text content of the editor (HTML tags stripped).
**Returns:** `Task<string>`
**`GetSelectedHtmlAsync()`**
Gets the HTML value of the selected content as a string.
**Returns:** `Task<string>`
**`GetCharCountAsync()`**
Retrieves the number of characters in the editor.
**Returns:** `Task<double>`
**`GetXhtmlAsync()`**
Retrieves XHTML-validated HTML content. Requires `EnableXhtml="true"`.
**Returns:** `Task<string>`
---
## Command Execution
All `ExecuteCommandAsync` overloads take a `CommandName` value (see [CommandName Enum Reference](#commandname-enum-reference)).
### Simple command (no arguments)
```csharp
await SmartRteObj.ExecuteCommandAsync(CommandName.Bold);
await SmartRteObj.ExecuteCommandAsync(CommandName.Undo);
await SmartRteObj.ExecuteCommandAsync(CommandName.InsertParagraph);
```
### Command with a string value
```csharp
// Insert raw HTML at the cursor
await SmartRteObj.ExecuteCommandAsync(CommandName.InsertHTML, "<strong>Hello</strong>");
// Insert plain text
await SmartRteObj.ExecuteCommandAsync(CommandName.InsertText, "typed text");
```
### Insert / edit an image
```csharp
await SmartRteObj.ExecuteCommandAsync(
    CommandName.InsertImage,
    new ImageCommandsArgs
    {
        Url      = "https://example.com/photo.jpg",
        AltText  = "A photo",
        Width    = new CommandsWidth { Width = "300px", MaxWidth = "100%" },
        Height   = new CommandsHeight { Height = "200px" },
        CssClass = "rounded"
    }
);
```
### Insert / edit a link
```csharp
await SmartRteObj.ExecuteCommandAsync(
    CommandName.CreateLink,
    new LinkCommandsArgs
    {
        Url    = "https://syncfusion.com",
        Text   = "Syncfusion",
        Title  = "Visit Syncfusion",
        Target = "_blank"
    }
);
```
### Insert a table
```csharp
await SmartRteObj.ExecuteCommandAsync(
    CommandName.InsertTable,
    new TableCommandsArgs { Rows = 3, Columns = 4 }
);
```
### Insert a video
```csharp
await SmartRteObj.ExecuteCommandAsync(
    CommandName.Video,
    new VideoCommandsArgs
    {
        Url    = "https://example.com/video.mp4",
        Width  = new CommandsWidth { Width = "560px" },
        Height = new CommandsHeight { Height = "315px" }
    }
);
```
### Insert audio
```csharp
await SmartRteObj.ExecuteCommandAsync(
    CommandName.Audio,
    new AudioCommandsArgs { Url = "https://example.com/sound.mp3" }
);
```
### Insert a code block
```csharp
await SmartRteObj.ExecuteCommandAsync(
    CommandName.InsertCodeBlock,
    new CodeBlockCommandArgs { Language = "typescript", Label = "TypeScript" },
    new ExecuteCommandOption { Undo = true }
);
```
### Format Painter
```csharp
// Copy formatting from current selection
await SmartRteObj.ExecuteCommandAsync(CommandName.CopyFormatPainter, new FormatPainterParams
{
    Action = FormatPainterAction.CopyFormat
});
// Apply copied formatting to new selection
await SmartRteObj.ExecuteCommandAsync(CommandName.ApplyFormatPainter, new FormatPainterParams
{
    Action = FormatPainterAction.PasteFormat,
    Options = new FormatPainterOptions
    {
        AllowedFormats = "b;strong;i;em;u",
        DeniedFormats  = "span[style]"
    }
});
```
**`ExecuteCommandOption`**
| Property | Type | Description |
|---|---|---|
| `Undo` | `bool` | When `true`, the command is added to the undo stack so it can be reversed |
---
## Toolbar Control
**`EnableToolbarItem(List<ToolbarCommand>? items)`**
Enables the specified toolbar items, making them interactive.
```csharp
SmartRteObj.EnableToolbarItem(new List<ToolbarCommand>
{
    ToolbarCommand.Bold,
    ToolbarCommand.Italic
});
```
**`DisableToolbarItem(List<ToolbarCommand>? items)`**
Disables the specified toolbar items, preventing their use.
```csharp
SmartRteObj.DisableToolbarItem(new List<ToolbarCommand>
{
    ToolbarCommand.Image,
    ToolbarCommand.CreateTable
});
```
**`RemoveToolbarItem(List<ToolbarCommand> items)`**
Removes the specified toolbar items from the toolbar entirely.
```csharp
SmartRteObj.RemoveToolbarItem(new List<ToolbarCommand>
{
    ToolbarCommand.FullScreen,
    ToolbarCommand.Print
});
```
---
## Dialog & UI Control
**`ShowDialogAsync(DialogType type)`**
Opens a specific insert/import dialog programmatically.
```csharp
// Open the insert-image dialog
await SmartRteObj.ShowDialogAsync(DialogType.InsertImage);
```
**`CloseDialogAsync(DialogType type)`**
Closes the specified dialog.
```csharp
await SmartRteObj.CloseDialogAsync(DialogType.InsertLink);
```
**`ShowInlineToolbarAsync()`** / **`HideInlineToolbarAsync()`**
Shows or hides the inline quick toolbar.
**`ShowFullScreenAsync()`**
Expands the editor to fill the viewport (full-screen mode).
**`ShowSourceCodeAsync()`**
Toggles the HTML/Markdown source code view.
**`PrintAsync()`**
Opens the browser print dialog for the editor content.
**`RefreshUIAsync()`**
Forces a UI refresh — useful after programmatic content changes.
---
## Undo / Redo
**`ClearUndoRedoAsync()`**
Clears both the undo and redo stacks and disables the undo/redo toolbar buttons. Use after loading new content programmatically to give the user a clean history.
```csharp
// Load fresh content then reset history
SmartRteObj.Value = "<p>New document</p>";
await SmartRteObj.ClearUndoRedoAsync();
```
---
## CommandName Enum Reference
| Value | Description |
|---|---|
| `ApplyFormatPainter` | Apply previously copied formatting |
| `Audio` | Insert/manage audio |
| `BackgroundColor` | Set text background color |
| `Blockquote` | Toggle blockquote |
| `Bold` | Toggle bold |
| `BulletFormatList` | Insert bullet list |
| `Checklist` | Insert checklist |
| `CopyFormatPainter` | Copy formatting from selection |
| `CreateLink` | Insert or edit a hyperlink |
| `EditImage` | Edit an existing image |
| `EditLink` | Edit an existing link |
| `ExportPdf` | Export content to PDF |
| `ExportWord` | Export content to Word |
| `FontColor` | Set text color |
| `FontName` | Set font family |
| `FontSize` | Set font size |
| `FormatBlock` | Apply block format |
| `Heading` | Apply heading |
| `ImportWord` | Import from Word |
| `Indent` | Indent content |
| `InsertBrOnReturn` | Insert `<br>` on Enter |
| `InsertCode` | Insert inline code |
| `InsertCodeBlock` | Insert code block with language |
| `InsertHTML` | Insert raw HTML at cursor |
| `InsertHorizontalRule` | Insert `<hr>` |
| `InsertImage` | Insert an image |
| `InsertOrderedList` | Insert numbered list |
| `InsertParagraph` | Insert paragraph break |
| `InsertTable` | Insert a table |
| `InsertText` | Insert plain text at cursor |
| `InsertUnorderedList` | Insert bullet list |
| `Italic` | Toggle italic |
| `JustifyCenter` | Center align |
| `JustifyFull` | Justify align |
| `JustifyLeft` | Left align |
| `JustifyRight` | Right align |
| `Lowercase` | Convert to lowercase |
| `NumberFormatList` | Insert number format list |
| `Outdent` | Outdent content |
| `Redo` | Redo last undone action |
| `RemoveFormat` | Clear all formatting |
| `StrikeThrough` | Toggle strikethrough |
| `Subscript` | Toggle subscript |
| `Superscript` | Toggle superscript |
| `Underline` | Toggle underline |
| `Undo` | Undo last action |
| `Uppercase` | Convert to uppercase |
| `Video` | Insert/manage video |
---
## ToolbarCommand Enum Reference
Used by `EnableToolbarItem`, `DisableToolbarItem`, `RemoveToolbarItem`.
| Value | Description |
|---|---|
| `Alignments` | Alignment dropdown |
| `Audio` | Audio insertion tool |
| `BackgroundColor` | Background colour picker |
| `Blockquote` | Blockquote toggle |
| `Bold` | Bold toggle |
| `BulletFormatList` | Bullet format list |
| `Checklist` | Checklist tool |
| `ClearFormat` | Clear formatting |
| `CodeBlock` | Code block insertion |
| `CreateLink` | Insert/edit link |
| `CreateTable` | Insert table |
| `ExportPdf` | Export to PDF |
| `ExportWord` | Export to Word |
| `FontColor` | Font colour picker |
| `FontName` | Font family selector |
| `FontSize` | Font size selector |
| `FormatPainter` | Format Painter tool |
| `Formats` | Format block dropdown |
| `FullScreen` | Full-screen toggle |
| `HorizontalLine` | Insert horizontal rule |
| `HorizontalSeparator` | Toolbar separator |
| `Image` | Image insertion tool |
| `ImportWord` | Import Word document |
| `Indent` | Indent |
| `InlineCode` | Inline code |
| `InsertCode` | Insert code |
| `Italic` | Italic toggle |
| `LineHeight` | Line height selector |
| `LowerCase` | Convert to lowercase |
| `Maximize` | Maximize |
| `Minimize` | Minimize |
| `NumberFormatList` | Number format list |
| `OrderedList` | Ordered list |
| `Outdent` | Outdent |
| `Preview` | Preview mode |
| `Print` | Print |
| `Redo` | Redo |
| `RemoveLink` | Remove hyperlink |
| `Separator` | Toolbar separator |
| `SourceCode` | HTML source view |
| `StrikeThrough` | Strikethrough |
| `SubScript` | Subscript |
| `SuperScript` | Superscript |
| `Underline` | Underline |
| `Undo` | Undo |
| `UnorderedList` | Unordered list |
| `UpperCase` | Convert to uppercase |
| `Video` | Video insertion tool |
---
## DialogType Enum Reference
Used by `ShowDialogAsync` and `CloseDialogAsync`.
| Value | Description |
|---|---|
| `ImportWord` | Word document import dialog |
| `InsertAudio` | Audio insertion dialog |
| `InsertImage` | Image insertion dialog |
| `InsertLink` | Hyperlink insertion dialog |
| `InsertTable` | Table insertion dialog |
| `InsertVideo` | Video insertion dialog |
```