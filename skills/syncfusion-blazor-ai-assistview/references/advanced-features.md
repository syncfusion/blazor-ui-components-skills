# Advanced Features

## Table of Contents
- [Markdown Rendering](#markdown-rendering)
- [Scroll to Bottom](#scroll-to-bottom)
- [Conversation History Management](#conversation-history-management)
- [Streaming Responses](#streaming-responses)
- [Common Patterns](#common-patterns)

## Markdown Rendering

The AI AssistView supports rendering responses as **Markdown** content, which is automatically converted to HTML. This enables rich text formatting like bold, italic, headings, lists, code blocks, and links.

### Basic Markdown Responses

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 650px;">
    <SfAIAssistView PromptRequested="@OnPromptRequested"></SfAIAssistView>
</div>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        
        // Response with markdown formatting
        var markdownResponse = @"
# Heading 1

## Heading 2

This is **bold** text and this is *italic* text.

### Code Example
```csharp
var message = ""Hello Blazor"";
Console.WriteLine(message);
```

### List
- Item 1
- Item 2
- Item 3

### Link
[Visit Syncfusion Docs](https://docs.syncfusion.com)
";
        
        args.Response = markdownResponse;
    }
}
```

### Markdown Features Supported

```csharp
private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
{
    var response = @"
# Main Title

## Subheading

**Bold Text** | *Italic Text* | ***Bold and Italic***

### Ordered List
1. First step
2. Second step
3. Third step

### Unordered List
- Point A
- Point B
- Point C

### Code Block
```csharp
public class Example
{
    public string Message { get; set; }
}
```

### Inline Code
Use `var x = 10;` to declare a variable.

### Blockquote
> This is an important quote

### Table
| Column 1 | Column 2 |
|----------|----------|
| Value 1  | Value 2  |

### Link
[Documentation](https://example.com)

### Horizontal Rule
---
";
    
    args.Response = response;
}
```

## Scroll to Bottom

The `EnableScrollToBottom` property shows or hides the scroll-to-bottom indicator. By default, this is `true`. When enabled, a floating icon appears when the user scrolls away from the bottom, allowing them to quickly jump to the latest message.

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 650px;">
    <!-- Enable scroll-to-bottom indicator (default: true) -->
    <SfAIAssistView 
        EnableScrollToBottom="true"
        PromptRequested="@OnPromptRequested">
    </SfAIAssistView>
</div>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response content</div>";
    }
}
```

### Disable Scroll to Bottom

```csharp
<!-- Disable the floating scroll indicator -->
<SfAIAssistView 
    EnableScrollToBottom="false"
    PromptRequested="@OnPromptRequested">
</SfAIAssistView>
```

### Scroll Behavior

- **User scrolls up:** Scroll-to-bottom button appears (if EnableScrollToBottom=true)
- **User clicks button:** View smoothly scrolls to the latest message
- **New message arrives:** Auto-scrolls to bottom if user is already there
- **User manually scrolls down:** Button disappears

## Conversation History Management

Manage multiple conversations by storing and retrieving prompt-response pairs:

### Store Conversation in Memory

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 650px;">
    <SfAIAssistView 
        Prompts="@conversationHistory"
        PromptRequested="@OnPromptRequested">
    </SfAIAssistView>
</div>

<button @onclick="ClearHistory">Clear History</button>
<button @onclick="ExportHistory">Export History</button>

@code {
    private List<AssistViewPrompt> conversationHistory = new();

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        
        var response = await GenerateResponse(args.Prompt);
        
        // Add to conversation history
        conversationHistory.Add(new AssistViewPrompt
        {
            Prompt = args.Prompt,
            Response = response
        });
        
        args.Response = response;
        StateHasChanged();
    }

    private async Task<string> GenerateResponse(string prompt)
    {
        // Call AI service
        return $"<div>Response to: {prompt}</div>";
    }

    private void ClearHistory()
    {
        conversationHistory.Clear();
        StateHasChanged();
    }

    private void ExportHistory()
    {
        var json = System.Text.Json.JsonSerializer.Serialize(conversationHistory);
        // Save or download JSON
    }
}
```

### Load Previous Conversation

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 650px;">
    <SfAIAssistView 
        Prompts="@conversationHistory"
        PromptRequested="@OnPromptRequested">
    </SfAIAssistView>
</div>

<button @onclick="LoadPreviousConversation">Load Previous Chat</button>

@code {
    private List<AssistViewPrompt> conversationHistory = new();

    protected override async Task OnInitializedAsync()
    {
        // Load previous conversation from storage
        await LoadConversationFromStorage();
    }

    private async Task LoadConversationFromStorage()
    {
        // Example: Load from localStorage or database
        var savedConversation = await GetSavedConversation();
        conversationHistory = savedConversation ?? new();
    }

    private async Task<List<AssistViewPrompt>?> GetSavedConversation()
    {
        // Fetch from your persistence layer
        return null;
    }

    private async Task LoadPreviousConversation()
    {
        await LoadConversationFromStorage();
        StateHasChanged();
    }

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        var response = $"<div>Response to: {args.Prompt}</div>";
        
        conversationHistory.Add(new AssistViewPrompt
        {
            Prompt = args.Prompt,
            Response = response
        });
        
        args.Response = response;
        StateHasChanged();
    }
}
```

## Streaming Responses

Implement streaming responses where the AI response is progressively displayed as it arrives:

```csharp
@using Syncfusion.Blazor.InteractiveChat
@inject HttpClient Http

<div style="height: 400px; width: 650px;">
    <SfAIAssistView 
        Prompts="@conversationHistory"
        PromptRequested="@OnPromptRequested">
    </SfAIAssistView>
</div>

@code {
    private List<AssistViewPrompt> conversationHistory = new();

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        // For streaming, start with placeholder
        var streamedResponse = await StreamAIResponse(args.Prompt);
        
        conversationHistory.Add(new AssistViewPrompt
        {
            Prompt = args.Prompt,
            Response = streamedResponse
        });
        
        args.Response = streamedResponse;
        StateHasChanged();
    }

    private async Task<string> StreamAIResponse(string prompt)
    {
        var response = new System.Text.StringBuilder();
        
        try
        {
            // Call streaming API endpoint
            using var httpResponse = await Http.GetAsync($"/api/stream?prompt={Uri.EscapeDataString(prompt)}");
            using var stream = await httpResponse.Content.ReadAsStreamAsync();
            using var reader = new System.IO.StreamReader(stream);

            string? line;
            while ((line = await reader.ReadLineAsync()) != null)
            {
                response.Append(line);
                // Simulate progressive rendering
                await Task.Delay(50);
            }

            return response.ToString();
        }
        catch (Exception ex)
        {
            return $"<div class='error'>Error streaming response: {ex.Message}</div>";
        }
    }
}
```

## Common Patterns

### Pattern 1: Multi-Turn Conversation with Context

Build context from conversation history for better AI responses:

```csharp
private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
{
    // Build context from conversation history
    var context = BuildContextFromHistory(conversationHistory);
    
    // Send prompt with context to AI service
    var response = await CallAIWithContext(args.Prompt, context);
    
    conversationHistory.Add(new AssistViewPrompt
    {
        Prompt = args.Prompt,
        Response = response
    });
    
    args.Response = response;
    StateHasChanged();
}

private string BuildContextFromHistory(List<AssistViewPrompt> history)
{
    var context = new System.Text.StringBuilder();
    context.AppendLine("Previous conversation:");
    
    foreach (var item in history.TakeLast(5)) // Use last 5 exchanges
    {
        context.AppendLine($"User: {item.Prompt}");
        context.AppendLine($"Assistant: {item.Response}");
    }
    
    return context.ToString();
}

private async Task<string> CallAIWithContext(string prompt, string context)
{
    // Call your AI service with both prompt and context
    return $"<div>Response considering context</div>";
}
```

### Pattern 2: Conversation Persistence

Save conversations to a database for retrieval:

```csharp
private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
{
    var response = await GenerateResponse(args.Prompt);
    
    var promptEntry = new AssistViewPrompt
    {
        Prompt = args.Prompt,
        Response = response
    };
    
    conversationHistory.Add(promptEntry);
    
    // Persist to database
    await SaveToDatabase(promptEntry);
    
    args.Response = response;
    StateHasChanged();
}

private async Task SaveToDatabase(AssistViewPrompt entry)
{
    var response = await Http.PostAsJsonAsync("/api/conversations", entry);
    if (!response.IsSuccessStatusCode)
    {
        // Handle error
    }
}
```

### Pattern 3: Rate Limiting and Error Handling

```csharp
private DateTime lastRequestTime = DateTime.MinValue;
private const int MinIntervalMs = 500;

private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
{
    // Rate limiting
    var timeSinceLastRequest = DateTime.Now - lastRequestTime;
    if (timeSinceLastRequest.TotalMilliseconds < MinIntervalMs)
    {
        args.Response = "<div class='warning'>Please wait before sending another message</div>";
        return;
    }

    lastRequestTime = DateTime.Now;

    try
    {
        var response = await GenerateResponse(args.Prompt);
        args.Response = response;
        conversationHistory.Add(new AssistViewPrompt
        {
            Prompt = args.Prompt,
            Response = response
        });
    }
    catch (HttpRequestException)
    {
        args.Response = "<div class='error'>Connection error. Please try again.</div>";
    }
    catch (OperationCanceledException)
    {
        args.Response = "<div class='error'>Request timeout. Please try again.</div>";
    }
    catch (Exception ex)
    {
        args.Response = $"<div class='error'>Error: {ex.Message}</div>";
    }
    
    StateHasChanged();
}

private async Task<string> GenerateResponse(string prompt)
{
    await Task.Delay(1000);
    return $"<div>Response to: {prompt}</div>";
}
```

## Best Practices

1. **Use markdown for rich formatting** - improves readability and user experience
2. **Enable scroll-to-bottom** - helps users see latest messages in long conversations
3. **Persist conversation history** - allow users to retrieve previous chats
4. **Implement streaming** - provide real-time feedback for long-running operations
5. **Add context to prompts** - improve AI response quality by including conversation history
6. **Handle errors gracefully** - show user-friendly error messages
7. **Rate limit requests** - prevent abuse and excessive API calls
8. **Test with long conversations** - ensure performance remains acceptable
