---
name: syncfusion-blazor-smart-rich-text-editor
description: "Implements Syncfusion SfSmartRichTextEditor, an AI-enhanced WYSIWYG editor extending SfRichTextEditor in Blazor. Use this when configuring AI backends (OpenAI, Azure OpenAI, Ollama, custom IChatClient), Smart Action toolbar, AI query dialog, AssistViewSettings, AI popup events and methods, or any inherited Rich Text Editor features in Blazor Server and Web App."
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Syncfusion Blazor Smart Rich Text Editor

A comprehensive skill for implementing and configuring the **Syncfusion Blazor Smart Rich Text Editor** (`SfSmartRichTextEditor`) — an AI-powered WYSIWYG editor that extends the full-featured `SfRichTextEditor` with intelligent content assistance. Supports OpenAI, Azure OpenAI, Ollama, and custom AI backends. Provides a Smart Action dropdown toolbar, an AI query dialog (`Alt+Enter`), and a fully customizable AI Assistant popup via `AssistViewSettings`.

## When to Use This Skill

Use this skill when the user needs to:
- Install and set up `SfSmartRichTextEditor` in a Blazor Server or Web App project
- Configure an AI backend: OpenAI, Azure OpenAI, Ollama, or a custom `IChatClient`
- Register `IChatInferenceService` / `SyncfusionAIService` in `Program.cs`
- Use the Smart Action dropdown toolbar for AI commands (summarize, expand, adjust tone)
- Add `Name = "AI Commands"` and `Name = "AI Query"` to `ToolbarItemModel` list to render AI toolbar buttons
- Open the AI Query dialog with `Alt+Enter` for free-form AI prompts
- Configure `AssistViewSettings`: `Commands`, `Suggestions`, `Prompts`, `Placeholder`, `MaxPromptHistory`, popup sizing
- Customize the AI Assistant popup (`BannerTemplate`, `HeaderToolbarSettings`, `PromptToolbarSettings`, `ResponseToolbarSettings`)
- Handle AI events: `AIPromptRequested`, `AIResponseStopped`, `AIToolbarItemClicked`, `AIPopupOpening`, `AIPopupClosing`
- Use AI Assistant methods: `ShowAIPopupAsync`, `HideAIPopupAsync`, `ExecuteAIPromptAsync`, `UpdateAIResponseAsync`, `GetAIPromptHistoryAsync`, `ClearAIPromptHistoryAsync`
- Style or animate the AI Assistant popup (`.e-rte-aiquery-dialog`)
- Use all inherited `SfRichTextEditor` features: toolbar, images, tables, events, methods, data binding, paste cleanup, import/export, accessibility

## Quick Start

**1. Install NuGet packages:**
```bash
dotnet add package Syncfusion.Blazor.SmartRichTextEditor
dotnet add package Syncfusion.Blazor.Themes
dotnet add package Microsoft.Extensions.AI
dotnet add package Microsoft.Extensions.AI.OpenAI
```

**2. Register services in `Program.cs`:**
```csharp
using Syncfusion.Blazor;
using Syncfusion.Blazor.AI;
using Microsoft.Extensions.AI;
using OpenAI;

builder.Services.AddSyncfusionBlazor();

string openAIApiKey = "YOUR_API_KEY";
string openAIModel = "gpt-4";
OpenAIClient openAIClient = new OpenAIClient(openAIApiKey);
IChatClient chatClient = openAIClient.GetChatClient(openAIModel).AsIChatClient();
builder.Services.AddChatClient(chatClient);
builder.Services.AddSingleton<IChatInferenceService, SyncfusionAIService>();
```

**3. Add to `_Imports.razor`:**
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.SmartRichTextEditor
```

**4. Add CSS/JS in `App.razor` (Server) or `index.html` (WASM):**
```html
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

**5. Add the component:**
```razor
@rendermode InteractiveServer

<SfSmartRichTextEditor>
    <h2>Welcome to Smart Rich Text Editor</h2>
    <p>Select text and use the Smart Action toolbar, or press Alt+Enter to open the AI Query dialog.</p>
    <AssistViewSettings Placeholder="Ask AI to rewrite or generate content." />
</SfSmartRichTextEditor>
```

## Navigation Guide

### Getting Started & AI Service Setup
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet installation: `Syncfusion.Blazor.SmartRichTextEditor` + `Syncfusion.Blazor.Themes`
- Full setup for Blazor Server App (Visual Studio and VS Code) and Blazor Web App (.NET 8+)
- OpenAI, Azure OpenAI, Ollama, and custom `IChatClient` configuration in `Program.cs`
- `AddInteractiveServerComponents()` registration pattern for Web App
- CSS theme and JS script references (.NET 6 through .NET 10)
- Adding the component with `AssistViewSettings`
- Content retrieval methods (`GetTextAsync`, `GetCharCountAsync`)
- Common setup issues and fixes

### AI Backend Configuration
📄 **Read:** [references/ai-backends.md](references/ai-backends.md)
- **OpenAI**: API key, supported models (`gpt-4-turbo`, `gpt-4`, `gpt-3.5-turbo`), `OpenAIClient` setup, environment variables and User Secrets
- **Azure OpenAI**: resource deployment, `AzureOpenAIClient` + `ApiKeyCredential`, `appsettings.json` config, Managed Identity (production recommended), HIPAA/SOC 2 compliance, monitoring, cost optimization
- **Ollama**: local model installation (`mistral`, `llama2`), `OllamaApiClient` + `OllamaSharp` NuGet, Docker Compose, GPU acceleration, model recommendations
- **Custom `IChatClient`**: implementing `IChatClient` for internal/proprietary AI, registering in `Program.cs`, streaming responses, retry logic, error handling, corporate AI API example

### AssistViewSettings — Properties
📄 **Read:** [references/assist-view-settings.md](references/assist-view-settings.md)
- `Commands` (`List<AICommands>`): Smart Action dropdown with nested sub-commands and icon CSS
- `PopupMaxHeight` / `PopupWidth`: control popup dimensions (CSS values or pixel numbers)
- `Placeholder`: placeholder text in the AI prompt textarea
- `Prompts` (`List<AssistViewPrompt>`): predefined prompt/response templates loaded into the popup
- `Suggestions` (`List<string>`): quick suggestion chips shown in the AI popup
- `MaxPromptHistory`: maximum conversation entries retained (default: 20)
- `BannerTemplate`: custom branding/header `RenderFragment` for the AI popup
- `HeaderToolbarSettings` / `PromptToolbarSettings` / `ResponseToolbarSettings`: toolbar customization with `AssistViewToolbarItem`

### AssistViewSettings — Events
📄 **Read:** [references/ai-events.md](references/ai-events.md)
- `AIPromptRequested` (`AssistViewPromptRequestedEventArgs`): intercept/modify prompt before AI call; set `Cancel=true` to prevent; modify `Response`, `PromptSuggestions`, `ResponseToolbarItems`
- `AIResponseStopped` (`ResponseStoppedEventArgs`): user clicked "Stop Responding"; provides `DataIndex` and `Prompt`
- `AIToolbarItemClicked` (`AssistViewToolbarItemClickedEventArgs`): handle custom toolbar button clicks; access `Item`, `DataIndex`, `Event`; set `Cancel=true`
- `AIPopupOpening` (`BeforeOpenEventArgs`): cancel popup opening with `args.Cancel = true`; access `Element` reference
- `AIPopupClosing` (`BeforeCloseEventArgs`): cancel popup closing; access `ClosedBy`, `IsInteracted`, `PreventFocus`

### AssistViewSettings — Methods
📄 **Read:** [references/ai-methods.md](references/ai-methods.md)
- `ShowAIPopupAsync()`: open the AI Assistant popup programmatically
- `HideAIPopupAsync()`: close the AI Assistant popup
- `ExecuteAIPromptAsync(string prompt)`: send a prompt programmatically (as if user typed it)
- `UpdateAIResponseAsync(string response, bool isFinalUpdate)`: stream or inject AI response; set `isFinalUpdate=true` to hide Stop button
- `GetAIPromptHistoryAsync()`: retrieve all saved prompts/responses as `AssistViewPrompt[]` (chronological, limited to `MaxPromptHistory`)
- `ClearAIPromptHistoryAsync()`: reset all conversation history
- Complete example combining all six methods with `@ref`

### AI Popup Appearance & CSS
📄 **Read:** [references/ai-appearance.md](references/ai-appearance.md)
- CSS selectors for the AI popup: `.e-rte-aiquery-dialog`, `.e-aiassistview`
- Customizing popup background, border, box-shadow
- Targeting `.e-view-header`, `.e-view-content .e-toolbar`, `.e-footer` sub-sections for fine-grained control
- Full custom popup styling example

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
- Default toolbar items for `SfSmartRichTextEditor`
- **AI-specific toolbar items** — `Name = "AI Commands"` (Smart Action dropdown) and `Name = "AI Query"` (Ask AI button); must use `Name` property, NOT `ToolbarCommand` enum
- Complete list of all `ToolbarCommand` enum values
- Text formatting, font & styling, alignment, lists, hyperlinks
- Image/table/link quick toolbar item commands using `SfSmartRichTextEditor`
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

### Events (RTE)
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

### Methods (RTE)
📄 **Read:** [references/methods.md](references/methods.md)
- Focus & selection: `FocusAsync`, `FocusOutAsync`, `SaveSelectionAsync`, `RestoreSelectionAsync`, `SelectAllAsync`, `GetSelectionAsync`
- Content retrieval: `GetTextAsync`, `GetSelectedHtmlAsync`, `GetCharCountAsync`, `GetXhtmlAsync`
- Command execution: all `ExecuteCommandAsync` overloads with typed args (image, link, table, video, audio, code block, format painter)
- Toolbar control: `EnableToolbarItem`, `DisableToolbarItem`, `RemoveToolbarItem`
- Dialog & UI control: `ShowDialogAsync`, `CloseDialogAsync`, `ShowFullScreenAsync`, `PrintAsync`, `RefreshUIAsync`, `ShowSourceCodeAsync`
- Undo/redo: `ClearUndoRedoAsync`
- Full `CommandName`, `ToolbarCommand`, and `DialogType` enum references

### Properties (RTE)
📄 **Read:** [references/properties.md](references/properties.md)
- Content & value: `Value`, `Placeholder`, `Readonly`, `Enabled`, `MaxLength`, `ShowCharCount`
- Editor behaviour: `EditorMode`, `EnterKey`, `ShiftEnterKey`, `EnableTabKey`, `EnableAutoUrl`, `EnableMarkdownAutoFormat`, `EnableClipboardCleanup`, `EnableXhtml`, `EnableHtmlEncode`, `EnableResize`
- Appearance & layout: `Height`, `Width`, `CssClass`, `ShowTooltip`, `FloatingToolbarOffset`
- IFrame mode: `RichTextEditorIFrameSettings` child component with `Enable`, `Attributes`, `Resources`
- Security & sanitization: `EnableHtmlSanitizer`, `AdditionalSanitizeAttributes`, `AdditionalSanitizeTags`, `DeniedSanitizeSelectors`
- Keyboard & shortcuts: `KeyConfigure` with full `ShortcutKeys` default bindings table
- Persistence & auto-save: `EnablePersistence`, `SaveInterval`, `AutoSaveOnIdle`
- Undo/redo config: `UndoRedoSteps`, `UndoRedoTimer`
- HTTP integration: `HttpClientInstance`
- Full 38-property summary table

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

### Basic Smart Rich Text Editor with AI
```razor
@rendermode InteractiveServer
@using Syncfusion.Blazor.SmartRichTextEditor

<SfSmartRichTextEditor @bind-Value="@Content">
    <AssistViewSettings Placeholder="Ask AI to rewrite or generate content." />
</SfSmartRichTextEditor>

@code {
    private string Content { get; set; } = "<p>Start editing...</p>";
}
```

### AI with custom Smart Action commands
```razor
<SfSmartRichTextEditor>
    <AssistViewSettings Commands="@MyCommands" Suggestions="@QuickSuggestions" />
</SfSmartRichTextEditor>

@code {
    private List<AICommands> MyCommands = new()
    {
        new AICommands { Text = "Summarize", Prompt = "Summarize this content concisely" },
        new AICommands { Text = "Expand", Prompt = "Add more details and examples" },
        new AICommands
        {
            Text = "Translate",
            Items = new List<AICommands>
            {
                new AICommands { Text = "To French", Prompt = "Translate to French" },
                new AICommands { Text = "To Spanish", Prompt = "Translate to Spanish" }
            }
        }
    };

    private List<string> QuickSuggestions = new()
    {
        "Fix grammar", "Make shorter", "More formal", "Simplify"
    };
}
```

### Programmatic AI popup control
```razor
<SfSmartRichTextEditor>
    <AssistViewSettings @ref="AssistViewRef" MaxPromptHistory="10" />
</SfSmartRichTextEditor>
<button @onclick="OpenAI">Open AI</button>
<button @onclick="RunPrompt">Run Prompt</button>

@code {
    private AssistViewSettings AssistViewRef;

    private async Task OpenAI() =>
        await AssistViewRef.ShowAIPopupAsync();

    private async Task RunPrompt() =>
        await AssistViewRef.ExecuteAIPromptAsync("Improve the clarity of this text");
}
```

### Two-way bound editor with character count
```razor
<SfSmartRichTextEditor @bind-Value="@HtmlContent" ShowCharCount="true" MaxLength="2000">
    <AssistViewSettings Placeholder="Ask AI for writing help." />
</SfSmartRichTextEditor>

@code {
    private string HtmlContent { get; set; } = string.Empty;
}
```

### Read-only display
```razor
<SfSmartRichTextEditor Value="@HtmlContent" Readonly="true">
    <RichTextEditorToolbarSettings Enable="false" />
</SfSmartRichTextEditor>
```

### Markdown editor mode
```razor
<SfSmartRichTextEditor EditorMode="EditorMode.Markdown" @bind-Value="@MarkdownContent">
    <AssistViewSettings Placeholder="Ask AI to help with your Markdown content." />
</SfSmartRichTextEditor>

@code {
    private string MarkdownContent { get; set; } = "**Hello** world!";
}
```

### Handle AI prompt event to intercept requests
```razor
<SfSmartRichTextEditor>
    <AssistViewSettings AIPromptRequested="OnAIPrompt" />
</SfSmartRichTextEditor>

@code {
    private async Task OnAIPrompt(AssistViewPromptRequestedEventArgs args)
    {
        // Log, validate, or modify args.Prompt before it is sent to AI
        Console.WriteLine($"Prompt: {args.Prompt}");
        // To cancel: args.Cancel = true;
    }
}
```
