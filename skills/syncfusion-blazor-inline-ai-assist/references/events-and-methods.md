# Events and Methods in Blazor Inline AI Assist

## Table of Contents
1. [Events Overview](#events-overview)
2. [Created Event](#created-event)
3. [PromptRequested Event](#promptrequested-event)
4. [Opened Event](#opened-event)
5. [Closed Event](#closed-event)
6. [Prompts Collection Property](#prompts-collection-property)
7. [Methods Overview](#methods-overview)
8. [ShowPopupAsync Method](#showpopupasync-method)
9. [HidePopupAsync Method](#hidepopupasync-method)
10. [ShowCommandPopupAsync Method](#showcommandpopupasync-method)
11. [HideCommandPopupAsync Method](#hidecommandpopupasync-method)
12. [UpdateResponseAsync Method](#updateresponseasync-method)
13. [AddResponse Method](#addresponse-method)
14. [ExecutePromptAsync Method](#executeprompatasync-method)
15. [Real-World Examples](#real-world-examples)

## Events Overview

The Inline AI Assist component triggers events at key points in its lifecycle. Events allow you to hook into component behavior, handle user actions, and implement custom business logic.

**Event Flow:**
1. **Created** - Component finishes rendering
2. **PromptRequested** - User submits a prompt
3. **Opened** - Popup opens
4. **Closed** - Popup closes

## Created Event

The `Created` event fires when the component finishes rendering and is ready for interaction.

### Use Cases
- Initialize component state
- Load user preferences
- Prefill commands or responses
- Set up AI provider connection
- Analytics tracking

### Example

```razor
<SfInlineAIAssist Created="OnComponentCreated">
</SfInlineAIAssist>

@code {
    private void OnComponentCreated(object args)
    {
        Console.WriteLine("Inline AI Assist component created and ready");
        // Initialize your logic here
    }
}
```

### Advanced Example: Load User Preferences

```razor
<SfInlineAIAssist @ref="inlineAssist" Created="OnCreatedAsync">
</SfInlineAIAssist>

@code {
    private SfInlineAIAssist inlineAssist = new();

    private async Task OnCreatedAsync(object args)
    {
        try
        {
            // Load user preferences from database or local storage
            var userPrefs = await LoadUserPreferencesAsync();
            
            Console.WriteLine($"Loaded preferences: {userPrefs.Theme}, {userPrefs.Language}");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error loading preferences: {ex.Message}");
        }
    }

    private async Task<UserPreferences> LoadUserPreferencesAsync()
    {
        // Simulate loading from database
        return new UserPreferences 
        { 
            Theme = "dark", 
            Language = "en" 
        };
    }
}

public class UserPreferences
{
    public string Theme { get; set; }
    public string Language { get; set; }
}
```

## PromptRequested Event

The `PromptRequested` event fires when the user submits a prompt. This is where you integrate with your AI provider.

### Event Arguments
- `args.Prompt` - The user's prompt text
- `args.Cancel` - Set to true to cancel prompt processing

### Use Cases
- Send prompt to AI provider (Azure OpenAI, OpenAI, etc.)
- Validate prompt before processing
- Log analytics
- Apply rate limiting
- Stream responses in real-time

### Basic Example

```razor
<SfInlineAIAssist PromptRequested="OnPromptRequested">
</SfInlineAIAssist>

@code {
    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        try
        {
            // Call your AI provider
            var response = await GetAIResponse(args.Prompt);
            
            // Update component with response
            await inlineAssist.UpdateResponseAsync(response, true);
        }
        catch (Exception ex)
        {
            await inlineAssist.UpdateResponseAsync($"Error: {ex.Message}", true);
        }
    }

    private async Task<string> GetAIResponse(string prompt)
    {
        // This is where you integrate with Azure OpenAI, OpenAI, etc.
        // See ai-integration.md for detailed examples
        return "AI response here";
    }
}
```

### Streaming Response Example

```razor
<SfInlineAIAssist PromptRequested="OnPromptRequested" EnableStreaming="true">
</SfInlineAIAssist>

@code {
    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        try
        {
            var response = await GetAIResponseAsync(args.Prompt);
            
            // Stream response in chunks
            var words = response.Split(' ');
            var buffer = new StringBuilder();
            
            foreach (var word in words)
            {
                buffer.Append(word + " ");
                
                // Update every 5 words or at end
                if (buffer.Length > 50 || word == words.Last())
                {
                    bool isFinal = (word == words.Last());
                    await inlineAssist.UpdateResponseAsync(buffer.ToString(), isFinal);
                }
                
                // Add delay for visual streaming effect
                await Task.Delay(50);
            }
        }
        catch (Exception ex)
        {
            await inlineAssist.UpdateResponseAsync($"Error: {ex.Message}", true);
        }
    }
}
```

### Validation and Rate Limiting

```razor
<SfInlineAIAssist PromptRequested="OnPromptRequested">
</SfInlineAIAssist>

@code {
    private DateTime lastPromptTime = DateTime.MinValue;
    private const int RateLimitSeconds = 2; // Min 2 seconds between requests

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        // Validate prompt length
        if (string.IsNullOrWhiteSpace(args.Prompt))
        {
            await inlineAssist.UpdateResponseAsync("Prompt cannot be empty", true);
            return;
        }

        if (args.Prompt.Length > 5000)
        {
            await inlineAssist.UpdateResponseAsync("Prompt is too long (max 5000 characters)", true);
            return;
        }

        // Rate limiting
        var timeSinceLastPrompt = DateTime.Now - lastPromptTime;
        if (timeSinceLastPrompt.TotalSeconds < RateLimitSeconds)
        {
            var waitSeconds = RateLimitSeconds - (int)timeSinceLastPrompt.TotalSeconds;
            await inlineAssist.UpdateResponseAsync(
                $"Please wait {waitSeconds} seconds before making another request", 
                true
            );
            return;
        }

        lastPromptTime = DateTime.Now;
        
        try
        {
            var response = await GetAIResponse(args.Prompt);
            await inlineAssist.UpdateResponseAsync(response, true);
        }
        catch (Exception ex)
        {
            await inlineAssist.UpdateResponseAsync($"Error: {ex.Message}", true);
        }
    }
}
```

## Opened Event

The `Opened` event fires when the Inline AI Assist popup opens.

### Use Cases
- Track analytics (component opened)
- Focus input field
- Load dynamic content
- Reset component state

### Example

```razor
<SfInlineAIAssist Opened="OnOpened">
</SfInlineAIAssist>

@code {
    private void OnOpened(OpenEventArgs args)
    {
        Console.WriteLine("Inline AI Assist popup opened");
        // Add tracking, focus input, etc.
    }
}
```

## Closed Event

The `Closed` event fires when the Inline AI Assist popup closes.

### Use Cases
- Clear sensitive data
- Save state
- Analytics tracking
- Trigger next workflow

### Example

```razor
<SfInlineAIAssist Closed="OnClosed">
</SfInlineAIAssist>

@code {
    private void OnClosed(object args)
    {
        Console.WriteLine("Inline AI Assist popup closed");
        // Cleanup, save state, etc.
    }
}
```

## Prompts Collection Property

The `Prompts` collection provides access to all prompts and their responses submitted by the user. Each entry contains the submitted `Prompt` text and the generated `Response` text.

### Use Cases
- Retrieve the last response for insertion into the editor
- Build a history of all prompts and responses
- Implement "Accept" workflow by applying the response to your content
- Export conversation history

### Accessing the Last Response

```razor
@code {
    private async Task OnItemSelectAsync(ResponseItemSelectEventArgs args)
    {
        if (args.Item.Label == "Accept")
        {
            // Retrieve the last prompt and its response
            var lastPrompt = inlineAssist?.Prompts.LastOrDefault();
            if (lastPrompt != null && !string.IsNullOrEmpty(lastPrompt.Response))
            {
                editableContent = $"<p>{lastPrompt.Response}</p>";
            }
            await inlineAssist!.HidePopupAsync();
        }
        else if (args.Item.Label == "Discard")
        {
            await inlineAssist!.HidePopupAsync();
        }
    }
}
```

### Iterating Over All Prompts

```razor
@code {
    private void ExportConversation()
    {
        foreach (var prompt in inlineAssist.Prompts)
        {
            Console.WriteLine($"Prompt: {prompt.Prompt}");
            Console.WriteLine($"Response: {prompt.Response}");
        }
    }
}
```

## Methods Overview

Methods allow you to control the component programmatically.

| Method | Purpose |
|--------|---------|
| `ShowPopupAsync()` | Open the popup |
| `HidePopupAsync()` | Close the popup |
| `UpdateResponseAsync(response, isFinal)` | Update the response text |
| `ExecutePromptAsync(prompt)` | Programmatically execute a prompt |
| `AddResponse(response)` | Add a response to history |

## ShowPopupAsync Method

Opens the Inline AI Assist popup programmatically.

### Use Cases
- Open popup from button click
- Open on page load
- Trigger from keyboard shortcut
- Show contextual help

### Signature
```csharp
public async Task ShowPopupAsync()
```

### Example

```razor
<SfButton @onclick="OnShowAI">Show AI Assistant</SfButton>

<SfInlineAIAssist @ref="inlineAssist">
</SfInlineAIAssist>

@code {
    private SfInlineAIAssist inlineAssist = new();

    private async Task OnShowAI()
    {
        await inlineAssist.ShowPopupAsync();
    }
}
```

## HidePopupAsync Method

Closes the Inline AI Assist popup programmatically.

### Use Cases
- Close after accepting response
- Close on error
- Auto-close after delay
- Close on escape key

### Signature
```csharp
public async Task HidePopupAsync()
```

### Example

```razor
@code {
    private async Task OnResponseAccepted()
    {
        // Process response...
        await inlineAssist.HidePopupAsync();
    }
}
```

## UpdateResponseAsync Method

Updates the response displayed in the component. Typically called from `PromptRequested` event.

### Signature
```csharp
public async Task UpdateResponseAsync(string response, bool isFinal = false)
```

### Parameters
- `response` - The response text to display (supports HTML)
- `isFinal` - Set to true when response is complete (default: false)

### Use Cases
- Stream responses chunk by chunk
- Display static responses
- Show error messages
- Update response after user action

### Example: Streaming Response

```razor
private async Task OnPromptRequested(PromptRequestedEventArgs args)
{
    var fullResponse = await FetchFromAI(args.Prompt);
    
    // Stream response in chunks for visual effect
    var chunkSize = 10;
    for (int i = 0; i < fullResponse.Length; i += chunkSize)
    {
        var chunk = fullResponse.Substring(i, Math.Min(chunkSize, fullResponse.Length - i));
        bool isFinal = (i + chunkSize >= fullResponse.Length);
        
        await inlineAssist.UpdateResponseAsync(chunk, isFinal);
        await Task.Delay(50); // Delay for streaming effect
    }
}
```

## AddResponse Method

Adds a response to the component's history without triggering the prompt flow.

### Signature
```csharp
public void AddResponse(string response)
```

### Use Cases
- Add canned responses
- Add previous responses from database
- Populate component on load

### Example

```razor
@code {
    private void PopulateWithPreviousResponses()
    {
        inlineAssist.AddResponse("Response 1");
        inlineAssist.AddResponse("Response 2");
    }
}
```

## ExecutePromptAsync Method

Programmatically execute a prompt without user input.

### Signature
```csharp
public async Task ExecutePromptAsync(string prompt)
```

### Use Cases
- Execute predefined prompts
- Implement command palette
- Batch processing
- Automation

### Example

```razor
<SfButton @onclick="OnExecuteCommand">Execute Command</SfButton>

<SfInlineAIAssist @ref="inlineAssist" PromptRequested="OnPromptRequested">
</SfInlineAIAssist>

@code {
    private async Task OnExecuteCommand()
    {
        await inlineAssist.ExecutePromptAsync("Summarize the above text in 3 sentences");
    }
}
```

## ShowCommandPopupAsync Method

Opens the command action popup programmatically. The main popup must already be open.

### Signature
```csharp
public async Task ShowCommandPopupAsync()
```

### Requirements
- Main popup must be open (call `ShowPopupAsync()` first)
- CommandMenu must be configured in component

### Use Cases
- Show command menu on demand
- Auto-open for quick actions
- Show suggested commands
- Display context-specific commands

### Example

```razor
<SfButton @onclick="OnShowCommands">Show AI Commands</SfButton>

<SfInlineAIAssist @ref="inlineAssist" RelateTo="#button" PromptRequested="OnPromptRequested">
    <CommandMenu Commands="commandItems"></CommandMenu>
    <ResponseActions ItemSelect="OnItemSelectAsync"></ResponseActions>
</SfInlineAIAssist>

@code {
    private SfInlineAIAssist inlineAssist = new();
    private List<CommandItem> commandItems = new()
    {
        new CommandItem { Label = "Summarize", Prompt = "Summarize this text" },
        new CommandItem { Label = "Translate", Prompt = "Translate to Spanish" },
        new CommandItem { Label = "Rewrite", Prompt = "Rewrite in a professional tone" }
    };

    private async Task OnShowCommands()
    {
        await inlineAssist.ShowPopupAsync();
        await inlineAssist.ShowCommandPopupAsync();
    }
}
```

## HideCommandPopupAsync Method

Closes the command action popup programmatically.

### Signature
```csharp
public async Task HideCommandPopupAsync()
```

### Use Cases
- Close command menu after selection
- Hide on error
- Auto-close after delay
- Close on escape key

### Example

```razor
<SfInlineAIAssist @ref="inlineAssist" PromptRequested="OnPromptRequested">
    <CommandMenu Commands="commandItems" ItemSelect="OnCommandSelectAsync"></CommandMenu>
</SfInlineAIAssist>

@code {
    private async Task OnCommandSelectAsync(CommandItemSelectEventArgs args)
    {
        Console.WriteLine($"Selected command: {args.Item.Label}");
        
        // Auto-hide command popup after selection
        await inlineAssist.HideCommandPopupAsync();
    }
}
```

## Real-World Examples

### Example 1: Content Editor with AI Suggestions

```razor
@page "/editor"
@using Syncfusion.Blazor.InteractiveChat
@using Syncfusion.Blazor.Buttons

<div style="display: grid; grid-template-columns: 1fr 300px; gap: 20px;">
    <div>
        <h3>Content Editor</h3>
        <textarea @ref="editorRef" 
                  style="width: 100%; min-height: 400px; padding: 10px; border: 1px solid #ddd;">
        </textarea>
    </div>
    
    <div>
        <h3>AI Tools</h3>
        <SfButton @onclick="OnSummarize" style="width: 100%; margin-bottom: 10px;">
            Summarize
        </SfButton>
        <SfButton @onclick="OnGrammarCheck" style="width: 100%; margin-bottom: 10px;">
            Grammar Check
        </SfButton>
        <SfButton @onclick="OnRewrite" style="width: 100%;">
            Rewrite
        </SfButton>

        <SfInlineAIAssist @ref="inlineAssist"
                         RelateTo="#editorTools"
                         PromptRequested="OnPromptRequested">
            <ResponseActions ItemSelect="OnResponseAction"></ResponseActions>
        </SfInlineAIAssist>
    </div>
</div>

@code {
    private SfInlineAIAssist inlineAssist = new();
    private ElementReference editorRef;

    private async Task OnSummarize()
    {
        await inlineAssist.ExecutePromptAsync(
            $"Summarize this text:\n{await GetEditorContent()}"
        );
    }

    private async Task OnGrammarCheck()
    {
        await inlineAssist.ExecutePromptAsync(
            $"Check grammar and provide corrections:\n{await GetEditorContent()}"
        );
    }

    private async Task OnRewrite()
    {
        await inlineAssist.ExecutePromptAsync(
            $"Rewrite this text in a more professional tone:\n{await GetEditorContent()}"
        );
    }

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        // Call your AI provider
        var response = await GetAIResponse(args.Prompt);
        await inlineAssist.UpdateResponseAsync(response, true);
    }

    private async Task OnResponseAction(ResponseItemSelectEventArgs args)
    {
        if (args.Item.Label == "Accept")
        {
            // Insert response into editor
            await inlineAssist.HidePopupAsync();
        }
    }

    private async Task<string> GetEditorContent()
    {
        // Return editor content
        return "Editor content here";
    }

    private async Task<string> GetAIResponse(string prompt)
    {
        // Call AI provider
        return "AI response";
    }
}
```

### Example 2: Event Tracking and Analytics

```razor
<SfInlineAIAssist @ref="inlineAssist"
                 Created="OnCreated"
                 Opened="OnOpened"
                 Closed="OnClosed"
                 PromptRequested="OnPromptRequested">
</SfInlineAIAssist>

@code {
    private void OnCreated(object args)
    {
        LogEvent("InlineAIAssist.Created");
    }

    private void OnOpened(OpenEventArgs args)
    {
        LogEvent("InlineAIAssist.Opened");
    }

    private void OnClosed(object args)
    {
        LogEvent("InlineAIAssist.Closed");
    }

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        LogEvent("InlineAIAssist.PromptRequested", new { PromptLength = args.Prompt.Length });
        
        try
        {
            var response = await GetAIResponse(args.Prompt);
            await inlineAssist.UpdateResponseAsync(response, true);
            LogEvent("InlineAIAssist.ResponseReceived", new { ResponseLength = response.Length });
        }
        catch (Exception ex)
        {
            LogEvent("InlineAIAssist.Error", new { Error = ex.Message });
            await inlineAssist.UpdateResponseAsync($"Error: {ex.Message}", true);
        }
    }

    private void LogEvent(string eventName, object data = null)
    {
        Console.WriteLine($"[{DateTime.Now:HH:mm:ss}] {eventName}: {System.Text.Json.JsonSerializer.Serialize(data)}");
        // Send to analytics service
    }

    private async Task<string> GetAIResponse(string prompt)
    {
        return "AI response";
    }
}
```

## Best Practices

1. **Always handle exceptions** in `PromptRequested` - Show error messages to users
2. **Use streaming** for better UX - Updates feel more responsive
3. **Implement rate limiting** - Prevent abuse and API quota issues
4. **Validate input** - Check prompt length and content
5. **Track analytics** - Use Created, Opened, Closed events for insights
6. **Test error scenarios** - Network failures, timeout, invalid responses
7. **Provide user feedback** - Show loading state, success, or error messages
