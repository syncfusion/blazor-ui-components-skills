---
name: syncfusion-blazor-inline-ai-assist
description: Implement Blazor Inline AI Assist component for seamless AI-powered text editing and content generation. Covers setup in WebAssembly and Web App projects, event handling, AI provider integration (Azure OpenAI, OpenAI, Ollama, Gemini), customization, accessibility, and enterprise deployment patterns.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Blazor Inline AI Assist Component Skill

The Inline AI Assist component enables developers to build AI-powered text editing experiences in Blazor applications. It provides a flexible UI for prompt entry, configurable AI provider integration, and comprehensive customization options for enterprise applications.

## Component Overview

The **SfInlineAIAssist** component enables users to prompt AI services and receive intelligent responses directly within their interface, with flexible display modes and extensive customization options such as:
- **Dual Response Modes:** Display AI responses inline at the cursor position or in a popup overlay for flexible UI integration.
- **Command Actions:** Display predefined commands and quick-action suggestions using mention-style popups with customizable grouping and icons via CommandMenu.
- **Response Actions:** Configurable actions such as Accept, Discard, and custom items using ResponseActions to manage AI-generated content.
- **Action Toolbar:** Includes a toolbar in the prompt input area with a send button and custom actions.
- **Markdown:** Automatic conversion of Markdown responses to HTML for rich content rendering.
- **Streaming Responses:** Supports word-by-word or chunk-based streaming with real-time visual feedback and stop controls.
- **Indicators:** Shows processing states using inline indicators and skeleton loading in popup mode.
- **Customization:** Supports customization of templates, appearance, and response rendering.


## Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- WebAssembly App setup (Visual Studio, VS Code, .NET CLI)
- Web App setup with interactive render modes
- NuGet package installation (Syncfusion.Blazor.InteractiveChat)
- Namespace imports and initial component rendering
- Minimal working example to verify setup

### Events and Methods
📄 **Read:** [references/events-and-methods.md](references/events-and-methods.md)
- Events: Created, PromptRequested, Opened, Closed
- Methods: AddResponse, RefreshContent, ShowPopupAsync, HidePopupAsync
- Event handling patterns and error management
- Response lifecycle and management
- Real-world event scenarios

### Commands and Toolbar
📄 **Read:** [references/commands-and-toolbar.md](references/commands-and-toolbar.md)
- Command configuration with labels, prompts, icons, and grouping
- CommandMenu tag helper and command popup sizing
- Inline toolbar with custom action buttons
- ItemSelect event for commands and ItemClick event for toolbar
- Real-world command examples (Summarize, Rewrite, Translate)

### AI Provider Integration
📄 **Read:** [references/ai-integration.md](references/ai-integration.md)
- Microsoft.Extensions.AI framework setup
- Azure OpenAI integration step-by-step
- OpenAI direct integration
- Ollama local LLM integration
- Gemini API integration
- LiteLLM proxy setup and configuration
- Provider comparison and selection guidance

### Customization and Response Settings
📄 **Read:** [references/customization-and-response.md](references/customization-and-response.md)
- ResponseMode property (Inline vs Popup display modes, dynamic switching)
- PopupWidth, PopupHeight, ZIndex configuration
- Placeholder, RelateTo, Target, EnableStreaming properties
- Response settings (streaming, formatting, templates)
- Custom response items configuration
- Appearance customization with CSSClass
- EditorTemplate and ResponseTemplate usage
- Responsive design patterns

### Accessibility and Globalization
📄 **Read:** [references/accessibility-and-globalization.md](references/accessibility-and-globalization.md)
- WCAG 2.2 AA compliance features
- Keyboard navigation support (Tab, Arrow Keys, Enter)
- ARIA attributes and screen reader support
- Globalization and RTL (Right-to-Left) support
- Localization configuration
- Mobile device accessibility

## Quick Start Example

Here's a minimal working example to verify your setup:

```razor
@using Syncfusion.Blazor.InteractiveChat
@using Syncfusion.Blazor.Buttons

<div style="height: 350px; width: 650px;">
    <SfButton id="aiButton" IsPrimary="true" @onclick="OnButtonClick">
        Ask AI
    </SfButton>
    <div id="editableText" contenteditable="true" style="border: 1px solid #ddd; padding: 10px; margin-top: 10px;">
        <p>Enter your text here for AI processing...</p>
    </div>
    <SfInlineAIAssist @ref="inlineAssist" 
                      RelateTo="#aiButton" 
                      PromptRequested="OnPromptRequested">
        <ResponseActions ItemSelect="OnResponseItemSelect"></ResponseActions>
    </SfInlineAIAssist>
</div>

@code {
    private SfInlineAIAssist inlineAssist = new();

    private async Task OnButtonClick()
    {
        await inlineAssist.ShowPopupAsync();
    }

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        // Connect to your AI provider here (Azure OpenAI, OpenAI, etc.)
        // For this example, return a simple response
        string response = $"Processing your request: {args.Prompt}";
        await inlineAssist.UpdateResponseAsync(response, true);
    }

    private async Task OnResponseItemSelect(ResponseItemSelectEventArgs args)
    {
        if (args.Item.Label == "Accept")
        {
            await inlineAssist.HidePopupAsync();
        }
        else if (args.Item.Label == "Discard")
        {
            await inlineAssist.HidePopupAsync();
        }
    }
}
```

## Common Patterns

### Pattern 1: Content Summarization
```razor
<SfButton id="summarizeButton" @onclick="OnSummarizeClick">
    Summarize
</SfButton>

<SfInlineAIAssist @ref="inlineAssist" 
                  RelateTo="#summarizeButton"
                  PromptRequested="OnSummarizeRequest">
</SfInlineAIAssist>

@code {
    private async Task OnSummarizeClick()
    {
        await inlineAssist.ShowPopupAsync();
    }

    private async Task OnSummarizeRequest(PromptRequestedEventArgs args)
    {
        // Send to AI provider with system prompt:
        // "Summarize the following text in 2-3 sentences: " + selectedText
        await inlineAssist.UpdateResponseAsync(summaryResult, true);
    }
}
```

### Pattern 2: Custom Commands
```razor
<SfInlineAIAssist @ref="inlineAssist" PromptRequested="OnPromptRequested">
    <CommandMenu Commands="commandItems"></CommandMenu>
</SfInlineAIAssist>

@code {
    private List<CommandItem> commandItems = new List<CommandItem>
    {
        new CommandItem { Label = "Summarize", Prompt = "Summarize this text", IconCss = "e-icons e-collapse-2" },
        new CommandItem { Label = "Translate", Prompt = "Translate to Spanish", IconCss = "e-icons e-translate" },
        new CommandItem { Label = "Rewrite", Prompt = "Rewrite in professional tone", IconCss = "e-icons e-edit" }
    };
}
```

### Pattern 3: Response Management
```razor
<SfInlineAIAssist @ref="inlineAssist" PromptRequested="OnPromptRequested">
    <ResponseActions Items="customResponseItems" ItemSelect="OnResponseAction">
    </ResponseActions>
</SfInlineAIAssist>

@code {
    private List<ResponseItem> customResponseItems = new()
    {
        new ResponseItem { Label = "Regenerate", IconCss = "e-icons e-refresh", Tooltip = "Generate new response" },
        new ResponseItem { Label = "Copy", IconCss = "e-icons e-copy", Tooltip = "Copy to clipboard" },
        new ResponseItem { Label = "Insert", IconCss = "e-icons e-insert", Tooltip = "Insert into document" }
    };
}
```

## Key Props

| Property | Type | Purpose |
|----------|------|---------|
| `RelateTo` | string | Anchor element selector for popup positioning (e.g., "#aiButton") |
| `Target` | string | DOM selector where the component container is appended (e.g., `"body"`, `"#app"`) |
| `Placeholder` | string | Input field placeholder text (default: "Enter prompt") |
| `ResponseMode` | ResponseDisplayMode | Display mode: `Inline` (at cursor) or `Popup` (overlay) |
| `PopupWidth` | string | Popup width when ResponseMode is `Popup` (e.g., "400px") |
| `PopupHeight` | string | Popup height for response display area (e.g., "300px" or "auto") |
| `ZIndex` | int | CSS z-index for popup layering |
| `EnableStreaming` | bool | Enable word-by-word streaming responses (default: `false`) |
| `CssClass` | string | Custom CSS class for styling |
| `PromptRequested` | EventCallback\<PromptRequestedEventArgs\> | Triggered when user submits a prompt |
| `Created` | EventCallback | Fires when component is created and ready |
| `Opened` | EventCallback\<OpenEventArgs\> | Triggered when popup opens |
| `Closed` | EventCallback | Triggered when popup closes |
| `Prompts` | IEnumerable\<PromptRecord\> | Read-only collection of submitted prompts and their responses |

## Programmatic Methods

The Inline AI Assist component provides async methods for programmatic control:

### ShowPopupAsync()
Opens the AI Assist popup at the RelatedTo element position:

```csharp
@ref="inlineAssist"

private SfInlineAIAssist inlineAssist;

private async Task OpenAIAssist()
{
    await inlineAssist.ShowPopupAsync();
}
```

### HidePopupAsync()
Closes the AI Assist popup:

```csharp
private async Task CloseAIAssist()
{
    await inlineAssist.HidePopupAsync();
}
```

### UpdateResponseAsync(string response, bool isFinal)
Sets or streams the AI response content:

```csharp
private async Task OnPromptRequested(PromptRequestedEventArgs args)
{
    // Non-streaming response
    await inlineAssist.UpdateResponseAsync("Generated response", true);
    
    // Streaming response (call multiple times)
    foreach (var chunk in streamedResponse)
    {
        await inlineAssist.UpdateResponseAsync(chunk, false);
    }
    // Final call with isFinal=true
    await inlineAssist.UpdateResponseAsync(lastChunk, true);
}
```

### ExecutePromptAsync(string prompt)
Programmatically execute a prompt without user input:

```csharp
private async Task ExecuteSamplePrompt()
{
    await inlineAssist.ExecutePromptAsync("Summarize the selected text in 2 sentences");
}
```

### ShowCommandPopupAsync()
Opens the command menu popup programmatically. The main popup should be open first.

```csharp
private async Task OpenCommands()
{
    await inlineAssist.ShowPopupAsync();
    await inlineAssist.ShowCommandPopupAsync();
}
```

### HideCommandPopupAsync()
Closes the command menu popup programmatically.

```csharp
private async Task CloseCommands()
{
    await inlineAssist.HideCommandPopupAsync();
}
```

## Common Use Cases

1. **Content Summarization** - Summarize selected text with AI
2. **Grammar & Tone Refinement** - Rewrite content in professional tone
3. **Translation** - Translate selected text to different languages
4. **Code Generation** - Generate code snippets from descriptions
5. **Email Composition** - Draft emails with AI assistance
6. **Form Completion** - Auto-fill form fields with AI suggestions
