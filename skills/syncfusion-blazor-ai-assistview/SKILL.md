---
name: syncfusion-blazor-ai-assistview
description: Implement the Syncfusion Blazor AI AssistView component for AI-powered chat interfaces in Blazor applications. Use this skill when implementing conversational AI, chatbots, AI assistants, or prompt-response interfaces. Covers AssistViewPrompt setup, PromptRequested events, markdown responses, and prompt suggestions with avatar customization.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Syncfusion Blazor AI AssistView

Build conversational AI interfaces with the Syncfusion Blazor AI AssistView component. This skill provides complete guidance for creating interactive chat applications, AI assistant UIs, and prompt-response systems in Blazor Server, WebAssembly, and Web App projects.

## Component Overview

The **SfAIAssistView** component is a specialized UI control for building AI-powered chat applications. It manages the presentation and interaction model for prompt-response conversations with built-in support for:
- **Prompt Management:** Input text, suggestions, placeholder text
- **Response Handling:** Async processing, markdown rendering, streaming support
- **Customization:** Avatar icons, styling, theme integration
- **UX Features:** Scroll-to-bottom indicator, conversation history
- **Event Integration:** PromptRequested events for AI service callbacks

## Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup
- Basic component structure and initialization
- PromptRequested event handler implementation
- Minimal working example with async responses

### Prompt Configuration
📄 **Read:** [references/prompt-configuration.md](references/prompt-configuration.md)
- Setting initial prompt text
- Configuring prompt placeholder text
- Managing prompt-response collections
- Handling prompt events asynchronously

### Prompt Suggestions
📄 **Read:** [references/prompt-suggestions.md](references/prompt-suggestions.md)
- Adding suggestion lists for user guidance
- Customizing suggestion headers
- Building suggestion-response mappings
- User interaction patterns with suggestions

### Customization & Icons
📄 **Read:** [references/customization-styling.md](references/customization-styling.md)
- Customizing user avatar with PromptIconCss
- Customizing AI avatar with ResponseIconCss
- Applying theme-specific icons
- CSS class and icon integration

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Markdown rendering in AI responses
- Scroll-to-bottom navigation (EnableScrollToBottom)
- Managing conversation history with Prompts collection
- Common patterns and use cases

### AI Service Integration
📄 **Read:** [references/ai-service-integration.md](references/ai-service-integration.md)
- OpenAI API integration (non-streaming and streaming)
- Azure OpenAI deployment
- Local Ollama LLM hosting
- Custom backend patterns
- Error handling and rate limiting strategies

### Attachments
📄 **Read:** [references/attachments.md](references/attachments.md)
- Enabling file attachments
- Configuring attachment settings (file types, size limits)
- Handling attachment events (upload, click, remove)
- Server-side upload implementation
- Security and validation best practices

### Multi-View Support
📄 **Read:** [references/multi-view-support.md](references/multi-view-support.md)
- Creating multiple specialized AI views
- Active view management and switching
- View-specific configuration
- Separate conversation histories per view
- Role-based and workspace-based views

### Toolbars
📄 **Read:** [references/toolbars.md](references/toolbars.md)
- Prompt toolbar customization
- Response toolbar actions (copy, like, regenerate)
- Footer toolbar configuration
- Custom toolbar items and templates
- Handling toolbar events

### Templates
📄 **Read:** [references/templates.md](references/templates.md)
- BannerTemplate for initial/empty state
- PromptItemTemplate for custom prompt rendering
- ResponseItemTemplate for custom response rendering
- PromptSuggestionItemTemplate for custom suggestions
- FooterTemplate and ViewTemplate customization
- Template contexts and available properties

### Streaming Responses
📄 **Read:** [references/streaming.md](references/streaming.md)
- Enabling streaming mode
- UpdateResponseAsync for progressive updates
- Real-time response rendering
- Integration with OpenAI, Azure, and Ollama streaming APIs
- Stop responding functionality
- Performance optimization

### Methods & Programmatic API
📄 **Read:** [references/methods-api.md](references/methods-api.md)
- ExecutePromptAsync for automated prompts
- UpdateResponseAsync for streaming updates
- RefreshUIAsync for manual UI refresh
- ScrollToBottomAsync for scroll control
- Automated workflows and batch processing

## Quick Start

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 600px;">
    <SfAIAssistView 
        Prompt="What is Blazor?" 
        PromptRequested="@OnPromptRequested">
    </SfAIAssistView>
</div>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        // Add delay to simulate AI processing
        await Task.Delay(1000);
        
        // Connect to your AI service here (OpenAI, Azure, etc.)
        var response = await GetAIResponse(args.Prompt);
        args.Response = response;
    }
    
    private async Task<string> GetAIResponse(string prompt)
    {
        // Call your AI service API
        // For testing: return a simple response
        return $"<div>Processing your prompt: {prompt}</div>";
    }
}
```

## Common Patterns

### Pattern 1: Pre-Initialized Conversation
Load the component with existing conversation history:

```csharp
<SfAIAssistView 
    Prompts="@conversationHistory" 
    PromptRequested="@OnPromptRequested">
</SfAIAssistView>

@code {
    private List<AssistViewPrompt> conversationHistory = new()
    {
        new AssistViewPrompt { 
            Prompt = "What is C#?", 
            Response = "<div>C# is a modern object-oriented programming language...</div>" 
        }
    };
}
```

### Pattern 2: Guided Suggestions
Use suggestions to help users refine their prompts:

```csharp
<SfAIAssistView 
    PromptSuggestions="@suggestions"
    PromptSuggestionsHeader="Try asking about:"
    PromptRequested="@OnPromptRequested">
</SfAIAssistView>

@code {
    private List<string> suggestions = new()
    {
        "How does dependency injection work?",
        "Explain async/await patterns",
        "What are Blazor components?"
    };
}
```

### Pattern 3: Custom Avatars
Personalize user and AI identities with custom icons:

```csharp
<SfAIAssistView 
    PromptIconCss="e-icons e-user"
    ResponseIconCss="e-icons e-bullet-2"
    PromptRequested="@OnPromptRequested">
</SfAIAssistView>
```

## Key Props

| Property | Type | Purpose |
|----------|------|---------|
| `Prompt` | string | Initial prompt text to display |
| `PromptPlaceholder` | string | Placeholder text in input field (default: "Type prompt for assistance...") |
| `PromptSuggestions` | List\<string\> | List of suggested prompts for user guidance |
| `PromptSuggestionsHeader` | string | Header text above suggestions |
| `Prompts` | List\<AssistViewPrompt\> | Collection of prompt-response pairs (conversation history) |
| `PromptIconCss` | string | CSS classes for user avatar icon |
| `ResponseIconCss` | string | CSS classes for AI avatar icon (default: "e-assistview-icon") |
| `EnableScrollToBottom` | bool | Show scroll-to-bottom indicator (default: true) |
| `EnableStreaming` | bool | Enable streaming mode for progressive responses |
| `AttachmentSettings` | AssistViewAttachmentSettings | File attachment configuration |
| `ActiveView` | int | Index of currently active view (for multi-view) |
| `ShowHeader` | bool | Show/hide component header (default: true) |
| `Width` | string | Component width (default: "100%") |
| `Height` | string | Component height (default: "100%") |
| `CssClass` | string | Custom CSS classes for styling |
| `EnableRtl` | bool | Enable right-to-left layout (default: false) |
| `ID` | string | Sets the id attribute for the component |

## Common Use Cases

1. **Customer Support Chatbot** - Combine with knowledge base to answer FAQs
2. **Coding Assistant** - Real-time code suggestions and explanations
3. **Content Generator** - Interactive writing and editing assistant
4. **Learning Interface** - Q&A system for educational content
5. **API Documentation** - Interactive API helper with examples
