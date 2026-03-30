---
name: syncfusion-blazor-rich-text-editor
description: "Implements the Syncfusion Blazor Rich Text Editor (SfRichTextEditor) from Syncfusion.Blazor.RichTextEditor. Supports HTML, Markdown, and IFrame editor modes. Use this skills for toolbar configuration, image/video/audio insertion, table management, paste cleanup, import/export Word/PDF, accessibility, globalization, RTL, and inline or custom toolbar tool scenarios in Blazor applications."
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "File Viewers & Editors"
---

# Syncfusion Blazor Rich Text Editor

A comprehensive skill for implementing and configuring the **Syncfusion Blazor Rich Text Editor** (`SfRichTextEditor`) — a full-featured WYSIWYG editor supporting HTML and Markdown editing, rich toolbar customization, media insertion, tables, data binding, import/export, and accessibility.

## When to Use This Skill

Use this skill when the user needs to:
- Install and set up `SfRichTextEditor` in a Blazor Server, WebAssembly, or Web App project
- Configure toolbar items, toolbar types (Expand, MultiRow, Scrollable, Popup), or toolbar position
- Format text: bold/italic/underline, headings, lists, alignment, colors, fonts, line height
- Insert and manage images, videos, or audio with upload support
- Work with tables: insert, resize, merge cells, customize quick toolbar
- Switch between HTML, Markdown, or IFrame editor modes
- Enable inline editing (inline toolbar)
- Add custom toolbar tools or use exec commands
- Handle RTE events (`ValueChange`, `OnActionBegin`, image upload events, etc.)
- Bind content two-way with `@bind-Value` or retrieve via `GetTextAsync`
- Configure paste cleanup behavior
- Import from Word / export to Word or PDF
- Implement accessibility, keyboard shortcuts, RTL, or globalization

## Quick Start

**1. Install NuGet packages:**
```bash
dotnet add package Syncfusion.Blazor.RichTextEditor
dotnet add package Syncfusion.Blazor.Themes
```

**2. Register in `Program.cs`:**
```csharp
using Syncfusion.Blazor;
builder.Services.AddSyncfusionBlazor();
```

**3. Add to `_Imports.razor`:**
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.RichTextEditor
```

**4. Add CSS/JS in `index.html` (WASM) or `App.razor` (Server):**
```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

**5. Add the component:**
```razor
<SfRichTextEditor @bind-Value="@Content">
    <RichTextEditorToolbarSettings Items="@Tools" />
</SfRichTextEditor>

@code {
    private string Content { get; set; } = "<p>Start editing...</p>";

    private List<ToolbarItemModel> Tools = new()
    {
        new() { Command = ToolbarCommand.Bold },
        new() { Command = ToolbarCommand.Italic },
        new() { Command = ToolbarCommand.Underline },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Formats },
        new() { Command = ToolbarCommand.Alignments },
        new() { Command = ToolbarCommand.OrderedList },
        new() { Command = ToolbarCommand.UnorderedList },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.CreateLink },
        new() { Command = ToolbarCommand.Image },
        new() { Command = ToolbarCommand.CreateTable },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Undo },
        new() { Command = ToolbarCommand.Redo }
    };
}
```

## Navigation Guide

### Getting Started & Setup
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet installation for all project types (Visual Studio, VS Code, .NET CLI)
- Service registration and namespace imports
- CSS theme and JS script references
- Adding the first component to a page
- Retrieving content as HTML, plain text, or character count

### Toolbar Configuration
📄 **Read:** [references/toolbar.md](references/toolbar.md)
- Enabling or disabling the toolbar with `RichTextEditorToolbarSettings.Enable`
- Toolbar types: Expand, MultiRow, Scrollable, Popup
- Floating toolbar and offset configuration
- Toolbar position (top or bottom)
- Configuring toolbar items with `ToolbarItemModel`
- Custom toolbar items (template-based)

### Built-in Toolbar Tools Reference
📄 **Read:** [references/built-in-tools.md](references/built-in-tools.md)
- Complete list of all `ToolbarCommand` enum values
- Text formatting, font & styling, alignment, lists, hyperlinks
- Image/table/link quick toolbar item commands
- Undo/redo, fullscreen, print, source code, preview tools
- Removing or reordering default toolbar items

### Text Formatting
📄 **Read:** [references/text-formatting.md](references/text-formatting.md)
- Bold, italic, underline, strikethrough, subscript, superscript, inline code
- Text alignment (left, center, right, justify)
- Ordered/unordered lists with custom number and bullet styles
- Heading formats (H1–H6, paragraph, blockquote, code)
- Line height configuration
- Horizontal line, format painter, clear format
- Markdown auto-format (inline and block shortcuts)

### Images, Video & Audio
📄 **Read:** [references/images-and-media.md](references/images-and-media.md)
- Image insertion from local machine or URL
- `RichTextEditorImageSettings`: SaveUrl, Path, AllowedTypes, EnableResize
- Base64 vs server-side upload
- Image quick toolbar configuration
- Video and audio insertion and settings

### Tables
📄 **Read:** [references/tables.md](references/tables.md)
- Creating tables with `CreateTable` toolbar item
- `RichTextEditorTableSettings`: width, resize, custom styles
- Table quick toolbar: add/remove rows & columns, merge cells, properties
- Advanced table manipulation (cell selection, split, custom borders)

### Editor Modes
📄 **Read:** [references/editor-modes.md](references/editor-modes.md)
- HTML editor mode (default WYSIWYG)
- Markdown editor mode (`EditorMode.Markdown`)
- IFrame vs DIV rendering mode
- Source code view toggling

### Inline Mode
📄 **Read:** [references/inline-mode.md](references/inline-mode.md)
- Enabling inline toolbar (`InlineMode`)
- Inline toolbar display on text selection
- Use cases: comment editors, in-place content editing

### Custom Tools & Exec Commands
📄 **Read:** [references/custom-tools.md](references/custom-tools.md)
- Adding custom toolbar buttons with `ToolbarItemModel` and templates
- Executing commands programmatically with `ExecuteCommandAsync`
- Slash commands (`/` trigger menu) configuration
- Format painter advanced settings (`RichTextEditorFormatPainterSettings`)

### Events
📄 **Read:** [references/events.md](references/events.md)
- `RichTextEditorEvents` child component usage and wiring pattern
- Lifecycle events: `Created`, `Destroyed`
- Focus/interaction: `Focus`, `Blur`, `OnToolbarClick`, `SelectionChanged`
- Content events: `ValueChange`, `BeforePasteCleanup`, `AfterPasteCleanup`
- Toolbar lifecycle: `OnActionBegin`, `OnActionComplete`
- Dialog events: `OnDialogOpen`, `DialogOpened`, `OnDialogClose`, `DialogClosed`
- Quick toolbar: `OnQuickToolbarOpen`, `QuickToolbarOpened`, `QuickToolbarClosed`
- Image events: `OnImageSelected`, `BeforeUploadImage`, `OnImageUploadSuccess`, `OnImageUploadFailed`, `ImageUploadChange`, `OnImageDrop`, `ImageDelete`
- Media events: `FileSelected`, `FileUploading`, `FileUploadSuccess`, `FileUploadFailed`, `FileUploadChange`, `OnMediaDrop`, `MediaDeleted`
- Resize events: `OnResizeStart`, `OnResizeStop`
- Toolbar status: `UpdatedToolbarStatus`
- Export events: `OnExport`, `OnExportFailure`
- Slash menu: `SlashMenuItemSelecting`
- Complete EventArgs class reference with all properties

### Methods
📄 **Read:** [references/methods.md](references/methods.md)
- Focus & selection: `FocusAsync`, `FocusOutAsync`, `SaveSelectionAsync`, `RestoreSelectionAsync`, `SelectAllAsync`, `GetSelectionAsync`
- Content retrieval: `GetTextAsync`, `GetSelectedHtmlAsync`, `GetCharCountAsync`, `GetXhtmlAsync`
- Command execution: all `ExecuteCommandAsync` overloads with typed args (image, link, table, video, audio, code block, format painter)
- Toolbar control: `EnableToolbarItem`, `DisableToolbarItem`, `RemoveToolbarItem`
- Dialog & UI control: `ShowDialogAsync`, `CloseDialogAsync`, `ShowFullScreenAsync`, `PrintAsync`, `RefreshUIAsync`, `ShowSourceCodeAsync`
- Undo/redo: `ClearUndoRedoAsync`
- Full `CommandName`, `ToolbarCommand`, and `DialogType` enum references

### Properties
📄 **Read:** [references/properties.md](references/properties.md)
- Content & value: `Value`, `Placeholder`, `Readonly`, `Enabled`, `MaxLength`, `ShowCharCount`
- Editor behaviour: `EditorMode`, `EnterKey`, `ShiftEnterKey`, `EnableTabKey`, `EnableAutoUrl`, `EnableMarkdownAutoFormat`, `EnableClipboardCleanup`, `EnableXhtml`, `EnableHtmlEncode`, `EnableResize`
- Appearance & layout: `Height`, `Width`, `CssClass`, `ShowTooltip`, `FloatingToolbarOffset`
- Security & sanitization: `EnableHtmlSanitizer`, `AdditionalSanitizeAttributes`, `AdditionalSanitizeTags`, `DeniedSanitizeSelectors`
- Keyboard & shortcuts: `KeyConfigure` with full `ShortcutKeys` default bindings table
- Persistence & auto-save: `EnablePersistence`, `SaveInterval`, `AutoSaveOnIdle`
- Undo/redo config: `UndoRedoSteps`, `UndoRedoTimer`
- HTTP integration: `HttpClientInstance`
- Full 37-property summary table

### Data Binding & Value Management
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Two-way binding with `@bind-Value`
- One-way binding and programmatic value updates
- `ValueChange` event for detecting edits
- Read-only mode (`Readonly` property)
- Max length enforcement and character count display

### Paste Cleanup
📄 **Read:** [references/paste-and-cleanup.md](references/paste-and-cleanup.md)
- `RichTextEditorPasteCleanupSettings` configuration
- Stripping/keeping specific HTML attributes and styles on paste
- Forcing plain text paste
- Enter key behavior (`EnterKey`, `ShiftEnterKey`)
- Undo/redo manager configuration

### Import & Export
📄 **Read:** [references/import-export.md](references/import-export.md)
- Importing Word documents into the editor
- Exporting content to Word (.docx) and PDF
- HTTP client configuration for import/export services
- Mail merge with dynamic data
- WebAssembly performance optimizations for large documents

### Accessibility & Globalization
📄 **Read:** [references/accessibility-globalization.md](references/accessibility-globalization.md)
- WCAG 2.1 compliance details
- Keyboard shortcuts and navigation reference
- ARIA roles and screen reader support
- RTL layout (`EnableRtl`)
- Localization and culture configuration
- XHTML validation

## Common Patterns

### Two-way bound editor with character count
```razor
<SfRichTextEditor @bind-Value="@HtmlContent" ShowCharCount="true" MaxLength="2000">
</SfRichTextEditor>

@code {
    private string HtmlContent { get; set; } = string.Empty;
}
```

### Read-only display
```razor
<SfRichTextEditor Value="@HtmlContent" Readonly="true">
    <RichTextEditorToolbarSettings Enable="false" />
</SfRichTextEditor>
```

### Markdown editor mode
```razor
<SfRichTextEditor EditorMode="EditorMode.Markdown" @bind-Value="@MarkdownContent">
</SfRichTextEditor>

@code {
    private string MarkdownContent { get; set; } = "**Hello** world!";
}
```
