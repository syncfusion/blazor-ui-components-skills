# Prompt Configuration

## Table of Contents
- [Setting Prompt Text](#setting-prompt-text)
- [Prompt Placeholder](#prompt-placeholder)
- [Prompt-Response Collection](#prompt-response-collection)
- [Event Handling](#event-handling)
- [Response Management](#response-management)

## Setting Prompt Text

Use the `Prompt` property to define the initial prompt text that appears in the component:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div class="aiassist-container" style="height: 350px; width: 650px;">
    <SfAIAssistView 
        Prompt="What tools can help me prioritize tasks?" 
        PromptRequested="@OnPromptRequested">
    </SfAIAssistView>
</div>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        var response = "For real-time prompt processing, connect the AI AssistView component " +
                      "to your preferred AI service, such as OpenAI or Azure Cognitive Services. " +
                      "Ensure you obtain the necessary API credentials to authenticate and enable " +
                      "seamless integration.";
        args.Response = response;
    }
}
```

**Use Cases:**
- Display a greeting or instruction as the first prompt
- Initialize the component with a pre-loaded question
- Show example usage patterns to users

## Prompt Placeholder

The `PromptPlaceholder` property sets the placeholder text shown in the prompt input textarea. The default value is `"Type prompt for assistance..."`.

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div class="aiassist-container" style="height: 350px; width: 650px;">
    <SfAIAssistView 
        PromptPlaceholder="Type a message..." 
        PromptRequested="@OnPromptRequested">
    </SfAIAssistView>
</div>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "Response to your query";
    }
}
```

**Common Placeholder Examples:**
- `"Ask me anything..."`
- `"Describe your issue..."`
- `"Enter your question..."`
- `"Type your request here..."`
- `"How can I assist you?"`

## Prompt-Response Collection

Use the `Prompts` property to initialize the component with a collection of existing prompt-response pairs. This represents the conversation history:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div class="aiassist-container" style="height: 350px; width: 650px;">
    <SfAIAssistView 
        Prompts="@conversationHistory" 
        PromptRequested="@OnPromptRequested">
    </SfAIAssistView>
</div>

@code {
    private List<AssistViewPrompt> conversationHistory = new()
    {
        new AssistViewPrompt() 
        { 
            Prompt = "What is AI?", 
            Response = "<div>AI stands for Artificial Intelligence, enabling machines to mimic human " +
                      "intelligence for tasks such as learning, problem-solving, and decision-making.</div>" 
        },
        new AssistViewPrompt() 
        { 
            Prompt = "How is AI used?", 
            Response = "<div>AI is used in recommendation systems, natural language processing, " +
                      "computer vision, and autonomous systems.</div>" 
        }
    };

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        
        // Look up response in history or generate new one
        var existingPrompt = conversationHistory
            .FirstOrDefault(p => p.Prompt == args.Prompt);
            
        if (existingPrompt != null)
        {
            args.Response = existingPrompt.Response;
        }
        else
        {
            args.Response = "For real-time prompt processing, connect to your preferred AI service.";
        }
    }
}
```

**Conversation History Management:**

```csharp
private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
{
    // Generate or fetch response from AI service
    var response = await CallAIService(args.Prompt);
    
    // Add new prompt-response pair to history
    conversationHistory.Add(new AssistViewPrompt 
    { 
        Prompt = args.Prompt, 
        Response = response 
    });
    
    args.Response = response;
    
    // Trigger UI refresh if using StateHasChanged
    StateHasChanged();
}
```

## Event Handling

### PromptRequested Event

The `PromptRequested` event fires when a user submits a prompt. Use this event to integrate with AI services:

```csharp
private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
{
    // args.Prompt: User's input text
    // Set args.Response to provide the AI's response
    
    var userInput = args.Prompt;
    
    // Call AI service with error handling
    try
    {
        var aiResponse = await CallAIService(userInput);
        args.Response = aiResponse;
    }
    catch (Exception ex)
    {
        args.Response = $"<div class='error'>An error occurred: {ex.Message}</div>";
    }
}
```

### Async Processing Pattern

Handle long-running operations gracefully:

```csharp
private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
{
    try
    {
        // Optional: Show loading indicator
        args.Response = "<div class='loading'>Processing your request...</div>";
        
        // Simulate API delay
        await Task.Delay(2000);
        
        // Call your AI service
        var response = await GetAIResponse(args.Prompt);
        
        // Update response with actual content
        args.Response = response;
    }
    catch (HttpRequestException)
    {
        args.Response = "<div class='error'>Failed to connect to AI service. Please try again.</div>";
    }
    catch (Exception ex)
    {
        args.Response = $"<div class='error'>Error: {ex.Message}</div>";
    }
}

private async Task<string> GetAIResponse(string prompt)
{
    // Replace with actual AI service integration
    return $"<div>Processing: {prompt}</div>";
}
```

## Response Management

### Setting Response Content

Responses can be plain text or HTML:

```csharp
// Plain text response
args.Response = "This is a simple text response";

// HTML response with formatting
args.Response = "<div>" +
                "<strong>Key Point:</strong> This is important<br/>" +
                "<em>Additional info:</em> More details here" +
                "</div>";

// Response with styling
args.Response = "<div style='color: blue; font-size: 14px;'>" +
                "Formatted response content" +
                "</div>";
```

### Response with Timeout Handling

Implement timeout logic for API calls:

```csharp
private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
{
    using (var cts = new CancellationTokenSource(TimeSpan.FromSeconds(30)))
    {
        try
        {
            var response = await GetAIResponseWithTimeout(args.Prompt, cts.Token);
            args.Response = response;
        }
        catch (OperationCanceledException)
        {
            args.Response = "<div class='error'>Request timed out. Please try again.</div>";
        }
    }
}

private async Task<string> GetAIResponseWithTimeout(string prompt, CancellationToken cancellationToken)
{
    // Your AI service call here
    await Task.Delay(1000, cancellationToken);
    return $"Response to: {prompt}";
}
```

### Response with Fallback

Provide fallback responses when AI service is unavailable:

```csharp
private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
{
    var response = await CallAIServiceWithFallback(args.Prompt);
    args.Response = response;
}

private async Task<string> CallAIServiceWithFallback(string prompt)
{
    try
    {
        // Try primary AI service
        return await CallOpenAI(prompt);
    }
    catch
    {
        try
        {
            // Fallback to secondary service
            return await CallAzureOpenAI(prompt);
        }
        catch
        {
            // Ultimate fallback
            return "<div>Unable to process request. Please try again later.</div>";
        }
    }
}
```

## Best Practices

1. **Always set `args.Response`** in the PromptRequested event handler - never leave it null
2. **Handle exceptions gracefully** - users should see error messages, not crashes
3. **Manage conversation history** - save prompts and responses for context
4. **Use HTML for rich formatting** - responses support HTML content
5. **Add delays in testing** - simulate real API response times (500ms-2000ms)
6. **Cache responses** - avoid repeated API calls for identical prompts
