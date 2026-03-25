# Customization & Icons

## Table of Contents
- [Prompt Icon Customization](#prompt-icon-customization)
- [Response Icon Customization](#response-icon-customization)
- [Using Built-in Icon Libraries](#using-built-in-icon-libraries)
- [CSS Styling](#css-styling)
- [Theme Integration](#theme-integration)

## Prompt Icon Customization

The `PromptIconCss` property customizes the appearance of the user avatar that displays next to prompt messages:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div class="aiassist-container" style="height: 350px; width: 650px;">
    <SfAIAssistView 
        Prompts="@prompts" 
        PromptRequested="@OnPromptRequested" 
        PromptIconCss="e-icons e-user">
    </SfAIAssistView>
</div>

@code {
    private List<AssistViewPrompt> prompts = new()
    {
        new AssistViewPrompt() 
        { 
            Prompt = "What is AI?", 
            Response = "<div>AI stands for Artificial Intelligence...</div>" 
        }
    };

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response content</div>";
    }
}
```

**Available Prompt Icon Classes:**
- `e-icons e-user` - Generic user icon
- `e-icons e-person` - Person silhouette
- `e-icons e-profile` - Profile icon
- `e-icons e-user-circle` - Circular user icon
- Custom CSS classes for brand-specific icons

## Response Icon Customization

The `ResponseIconCss` property customizes the AI avatar that appears next to response messages. The default is `e-assistview-icon`:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div class="aiassist-container" style="height: 350px; width: 650px;">
    <SfAIAssistView 
        Prompts="@prompts" 
        PromptRequested="@OnPromptRequested" 
        ResponseIconCss="e-icons e-bullet-2">
    </SfAIAssistView>
</div>

@code {
    private List<AssistViewPrompt> prompts = new()
    {
        new AssistViewPrompt() 
        { 
            Prompt = "What is AI?", 
            Response = "<div>AI stands for Artificial Intelligence...</div>" 
        }
    };

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response content</div>";
    }
}
```

**Available Response Icon Classes:**
- `e-icons e-bullet-2` - Filled bullet point
- `e-icons e-robots` - Robot/AI icon
- `e-icons e-bot` - Bot icon (if available)
- `e-icons e-settings` - Settings/gear icon
- `e-assistview-icon` - Default Syncfusion AI icon (recommended)

## Using Built-in Icon Libraries

### Syncfusion Built-in Icons

```csharp
<!-- Show user with default user icon -->
<SfAIAssistView 
    PromptIconCss="e-icons e-user"
    ResponseIconCss="e-assistview-icon"
    PromptRequested="@OnPromptRequested">
</SfAIAssistView>

<!-- Show user and AI with robot/bot icons -->
<SfAIAssistView 
    PromptIconCss="e-icons e-person"
    ResponseIconCss="e-icons e-robots"
    PromptRequested="@OnPromptRequested">
</SfAIAssistView>
```

### Font Awesome Icons

If using Font Awesome in your project:

```html
<!-- Include Font Awesome in App.razor -->
<link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet" />
```

```csharp
<!-- Use Font Awesome classes -->
<SfAIAssistView 
    PromptIconCss="fas fa-user"
    ResponseIconCss="fas fa-robot"
    PromptRequested="@OnPromptRequested">
</SfAIAssistView>
```

### Bootstrap Icons

If using Bootstrap Icons in your project:

```html
<!-- Include Bootstrap Icons in App.razor -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.10.0/font/bootstrap-icons.css" rel="stylesheet" />
```

```csharp
<!-- Use Bootstrap Icons classes -->
<SfAIAssistView 
    PromptIconCss="bi bi-person-circle"
    ResponseIconCss="bi bi-robot"
    PromptRequested="@OnPromptRequested">
</SfAIAssistView>
```

## CSS Styling

### Component Container Styling

```csharp
@page "/assistant"

<style>
    .aiassist-container {
        height: 400px;
        width: 100%;
        border: 1px solid #ddd;
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    }

    .custom-chat {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        border-radius: 12px;
        padding: 12px;
    }
</style>

<div class="custom-chat">
    <div class="aiassist-container">
        <SfAIAssistView PromptRequested="@OnPromptRequested"></SfAIAssistView>
    </div>
</div>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response content</div>";
    }
}
```

### Message Styling

Customize how prompts and responses are displayed:

```csharp
@page "/assistant"

<style>
    /* Style user prompts */
    .e-assistview-prompt {
        background-color: #e3f2fd;
        border-left: 4px solid #2196f3;
        padding: 12px;
        margin: 8px 0;
        border-radius: 4px;
    }

    /* Style AI responses */
    .e-assistview-response {
        background-color: #f5f5f5;
        border-left: 4px solid #4caf50;
        padding: 12px;
        margin: 8px 0;
        border-radius: 4px;
    }

    /* Style icons */
    .e-assistview-icon {
        width: 32px;
        height: 32px;
        display: flex;
        align-items: center;
        justify-content: center;
    }
</style>

<div style="height: 400px;">
    <SfAIAssistView PromptRequested="@OnPromptRequested"></SfAIAssistView>
</div>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response content</div>";
    }
}
```

## Theme Integration

### Syncfusion Theme Consistency

Ensure the component uses the same theme as your application:

```csharp
@page "/assistant"

<!-- Import theme stylesheet -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<div style="height: 400px;">
    <SfAIAssistView PromptRequested="@OnPromptRequested"></SfAIAssistView>
</div>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response content</div>";
    }
}
```

**Available Themes:**
- `bootstrap5.css` - Bootstrap 5 theme
- `material.css` - Material Design
- `fluent.css` - Fluent UI (Microsoft)
- `tailwind.css` - Tailwind CSS
- `fabric.css` - Office Fabric
- `material-dark.css` - Dark mode

### Custom Theme Styling

Create a custom theme by overriding Syncfusion classes:

```csharp
@page "/assistant"

<style>
    /* Override Syncfusion theme colors */
    .e-assistview {
        --primary-color: #6366f1;
        --response-bg: #f8f9fa;
        --prompt-bg: #e0e7ff;
    }

    /* Custom dark theme */
    .dark-theme .e-assistview {
        --response-bg: #2a2a2a;
        --prompt-bg: #404040;
        color: #e0e0e0;
    }
</style>

<div style="height: 400px;" class="dark-theme">
    <SfAIAssistView PromptRequested="@OnPromptRequested"></SfAIAssistView>
</div>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response content</div>";
    }
}
```

## Complete Customization Example

```csharp
@page "/custom-assistant"
@using Syncfusion.Blazor.InteractiveChat

<style>
    .custom-container {
        height: 500px;
        width: 100%;
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        border-radius: 12px;
        padding: 12px;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
    }

    .chat-wrapper {
        height: 100%;
        background: white;
        border-radius: 8px;
        display: flex;
        flex-direction: column;
    }

    .chat-header {
        padding: 16px;
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        border-radius: 8px 8px 0 0;
        font-weight: bold;
    }

    .chat-content {
        flex: 1;
        overflow-y: auto;
    }
</style>

<div class="custom-container">
    <div class="chat-wrapper">
        <div class="chat-header">
            <span class="bi bi-robot"></span>
            AI Assistant
        </div>
        <div class="chat-content">
            <SfAIAssistView 
                PromptIconCss="bi bi-person-circle"
                ResponseIconCss="bi bi-robot"
                PromptSuggestions="@suggestions"
                PromptSuggestionsHeader="Quick Questions"
                PromptRequested="@OnPromptRequested">
            </SfAIAssistView>
        </div>
    </div>
</div>

@code {
    private List<string> suggestions = new()
    {
        "What can you help me with?",
        "How do I get started?",
        "Show me an example"
    };

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = $"<div><strong>Response:</strong> {args.Prompt}</div>";
    }
}
```

## Best Practices

1. **Choose theme-consistent icons** - match your application's design language
2. **Use semantic icons** - select icons that clearly represent their purpose
3. **Consider accessibility** - provide appropriate ARIA labels and descriptions
4. **Test icon visibility** - ensure icons are visible on all themes
5. **Maintain visual consistency** - use the same icon style throughout the app
6. **Responsive design** - adjust container size for different screen sizes
