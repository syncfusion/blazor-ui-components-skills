# Getting Started with AI AssistView

## Installation

### NuGet Package Setup

Install the Syncfusion.Blazor.InteractiveChat NuGet package:

```bash
dotnet add package Syncfusion.Blazor.InteractiveChat
```

### Import Namespaces

Add the required using statements to your `_Imports.razor` file:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.InteractiveChat
```

### Register Service

In your `Program.cs`, add the Syncfusion Blazor service registration:

```csharp
// Program.cs
builder.Services.AddSyncfusionBlazor();
```

### Add Theme

Include the Syncfusion Blazor theme stylesheet in your `App.razor` or layout file:

```html
<!-- Bootstrap 5 theme (or choose another: material, fluent, tailwind) -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Syncfusion Blazor script -->
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

Available themes: `bootstrap5.css`, `material.css`, `fluent.css`, `tailwind.css`, `fabric.css`, `material-dark.css`

## Basic Component Structure

### Minimal Example

Create a simple AI AssistView component with prompt handling:

```csharp
@page "/ai-assistant"
@using Syncfusion.Blazor.InteractiveChat

<h2>AI Assistant</h2>

<div class="aiassist-container" style="height: 400px; width: 650px;">
    <SfAIAssistView 
        Prompt="How can I help you today?"
        PromptRequested="@OnPromptRequested">
    </SfAIAssistView>
</div>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        // Simulate AI response delay
        await Task.Delay(1000);
        
        // Set response text
        args.Response = "<div>This is a response from the AI.</div>";
    }
}
```

### Component Properties

- **Prompt:** Sets the initial prompt text displayed in the conversation
- **PromptRequested:** Event triggered when the user submits a prompt; use this to call your AI service

## PromptRequested Event Handler

The `PromptRequested` event provides access to the user's input through `AssistViewPromptRequestedEventArgs`:

```csharp
private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
{
    // args.Prompt contains the user's input text
    string userPrompt = args.Prompt;
    
    // Call your AI service (OpenAI, Azure, Ollama, etc.)
    var aiResponse = await CallAIService(userPrompt);
    
    // Set the response - can be plain text or HTML
    args.Response = aiResponse;
}

private async Task<string> CallAIService(string prompt)
{
    // Example: Call OpenAI API
    // var client = new OpenAIClient(apiKey);
    // var response = await client.Chat.Completions.CreateAsync(...);
    // return response.Choices[0].Message.Content;
    
    return $"<div><strong>AI Response:</strong> Processing '{prompt}'...</div>";
}
```

## Async Response Processing

The component supports asynchronous prompt processing, allowing you to make API calls before returning responses:

```csharp
private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
{
    try
    {
        // Add processing delay to simulate real API call
        await Task.Delay(2000);
        
        // Example: Call external AI service
        var response = await FetchAIResponse(args.Prompt);
        args.Response = response;
    }
    catch (Exception ex)
    {
        args.Response = $"<div class='error'>Error: {ex.Message}</div>";
    }
}

private async Task<string> FetchAIResponse(string prompt)
{
    // Simulated API call
    // In production, integrate with OpenAI, Azure OpenAI, Ollama, etc.
    return $"<div>Response to: {prompt}</div>";
}
```

## Component Sizing

Control component dimensions using `Width` and `Height` properties:

```csharp
<!-- Fixed dimensions -->
<SfAIAssistView 
    Width="600px" 
    Height="400px"
    PromptRequested="@OnPromptRequested">
</SfAIAssistView>

<!-- Percentage-based sizing -->
<SfAIAssistView 
    Width="100%" 
    Height="500px"
    PromptRequested="@OnPromptRequested">
</SfAIAssistView>

<!-- Using container styling -->
<div style="height: 400px; width: 100%;">
    <SfAIAssistView PromptRequested="@OnPromptRequested"></SfAIAssistView>
</div>
```

## Additional Properties

### Show/Hide Header

Control header visibility with `ShowHeader`:

```csharp
<!-- Hide header for embedded scenarios -->
<SfAIAssistView 
    ShowHeader="false"
    PromptRequested="@OnPromptRequested">
</SfAIAssistView>
```

### Custom CSS Classes

Apply custom styling with `CssClass`:

```csharp
<style>
    .my-custom-assistant {
        border: 2px solid #0066cc;
        border-radius: 8px;
        box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    }
</style>

<SfAIAssistView 
    CssClass="my-custom-assistant"
    PromptRequested="@OnPromptRequested">
</SfAIAssistView>
```

### Right-to-Left Support

Enable RTL layout for right-to-left languages:

```csharp
<SfAIAssistView 
    EnableRtl="true"
    PromptPlaceholder="اكتب سؤالك هنا..."
    PromptRequested="@OnPromptRequested">
</SfAIAssistView>
```

### Component ID

Set a specific ID for the component:

```csharp
<SfAIAssistView 
    ID="aiAssistant1"
    PromptRequested="@OnPromptRequested">
</SfAIAssistView>
```

## Common Configuration

Basic setup with common properties:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<SfAIAssistView 
    Prompt="Welcome to AI Assistant"
    PromptPlaceholder="Type your question..."
    PromptRequested="@OnPromptRequested"
    EnableScrollToBottom="true">
</SfAIAssistView>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = $"<div>You asked: {args.Prompt}</div>";
    }
}
```

## Component Lifecycle Events

Handle component creation with the `Created` event:

```csharp
<SfAIAssistView 
    Created="@OnCreated"
    PromptRequested="@OnPromptRequested">
</SfAIAssistView>

@code {
    private void OnCreated(object args)
    {
        Console.WriteLine("AI AssistView component created");
        // Initialize component state
    }

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response content</div>";
    }
}
```

## Next Steps

- **Prompt Suggestions:** Add guided suggestions to help users refine queries
- **Customization:** Change avatar icons and styling
- **Advanced Features:** Enable markdown rendering and manage conversation history
- **AI Integration:** Connect to OpenAI, Azure, or your custom API
- **Attachments:** Enable file uploads with attachment settings
- **Multi-View:** Create specialized views for different purposes
- **Streaming:** Implement real-time streaming responses
- **Toolbars:** Add custom actions with toolbars
- **Templates:** Fully customize the UI with templates
