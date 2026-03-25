````markdown
# Streaming Responses

## Table of Contents
- [Overview](#overview)
- [Enabling Streaming](#enabling-streaming)
- [UpdateResponseAsync Method](#updateresponseasync-method)
- [Streaming Patterns](#streaming-patterns)
- [Stop Responding](#stop-responding)
- [Real-World Examples](#real-world-examples)
- [Best Practices](#best-practices)

## Overview

Streaming responses allow AI responses to appear progressively as they're generated, providing a better user experience for long-running AI operations. Instead of waiting for the entire response, users see content appearing in real-time, similar to ChatGPT and other modern AI interfaces.

**Benefits:**
- Improved perceived performance
- Better user engagement
- Ability to stop long-running responses
- More natural conversation flow

## Enabling Streaming

Enable streaming mode using the `EnableStreaming` property:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px;">
    <SfAIAssistView 
        EnableStreaming="true"
        PromptRequested="@OnPromptRequested">
    </SfAIAssistView>
</div>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        // Enable streaming mode
        await Task.Delay(1000);
        args.Response = "Initial response";
    }
}
```

## UpdateResponseAsync Method

Use `UpdateResponseAsync` to progressively update the response content:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px;">
    <SfAIAssistView 
        @ref="assistView"
        EnableStreaming="true"
        PromptRequested="@OnPromptRequested">
    </SfAIAssistView>
</div>

@code {
    private SfAIAssistView? assistView;

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        // Start with initial response
        args.Response = "<div>Thinking";
        
        // Stream additional content
        await StreamResponse("Thinking...");
        await Task.Delay(500);
        
        await StreamResponse("Analyzing your request...");
        await Task.Delay(500);
        
        await StreamResponse("Here's your answer: Lorem ipsum dolor sit amet, consectetur adipiscing elit.");
    }

    private async Task StreamResponse(string content)
    {
        if (assistView != null)
        {
            await assistView.UpdateResponseAsync(content);
        }
    }
}
```

## Streaming Patterns

### Pattern 1: Character-by-Character Streaming

Simulate typing effect:

```csharp
@code {
    private SfAIAssistView? assistView;

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        var fullResponse = "This is a complete response that will be streamed character by character.";
        
        // Initialize with empty response
        args.Response = "<div></div>";
        
        // Stream each character
        foreach (char c in fullResponse)
        {
            await assistView!.UpdateResponseAsync(c.ToString());
            await Task.Delay(50); // Delay between characters
        }
    }
}
```

### Pattern 2: Word-by-Word Streaming

Stream content word by word:

```csharp
@code {
    private SfAIAssistView? assistView;

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        var fullResponse = "This response will appear one word at a time for better readability";
        var words = fullResponse.Split(' ');
        
        args.Response = "<div>";
        
        foreach (var word in words)
        {
            await assistView!.UpdateResponseAsync($" {word}");
            await Task.Delay(100); // Delay between words
        }
        
        await assistView!.UpdateResponseAsync("</div>");
    }
}
```

### Pattern 3: Chunk-Based Streaming

Stream content in larger chunks:

```csharp
@code {
    private SfAIAssistView? assistView;

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        args.Response = "<div>";
        
        var chunks = new[]
        {
            "<h3>Introduction</h3><p>AI streaming provides real-time feedback.</p>",
            "<h3>Benefits</h3><ul><li>Better UX</li><li>Faster perceived response</li></ul>",
            "<h3>Conclusion</h3><p>Streaming improves user experience significantly.</p>"
        };
        
        foreach (var chunk in chunks)
        {
            await assistView!.UpdateResponseAsync(chunk);
            await Task.Delay(800); // Delay between chunks
        }
        
        await assistView!.UpdateResponseAsync("</div>");
    }
}
```

### Pattern 4: Server-Sent Events (SSE)

Stream responses from a server endpoint:

```csharp
@using System.Net.Http
@inject HttpClient Http

@code {
    private SfAIAssistView? assistView;

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        args.Response = "<div>";
        
        try
        {
            // Call streaming endpoint
            var response = await Http.GetAsync($"/api/ai/stream?prompt={Uri.EscapeDataString(args.PromptText)}");
            
            using var stream = await response.Content.ReadAsStreamAsync();
            using var reader = new StreamReader(stream);
            
            string? line;
            while ((line = await reader.ReadLineAsync()) != null)
            {
                if (!string.IsNullOrWhiteSpace(line))
                {
                    await assistView!.UpdateResponseAsync(line);
                    await Task.Delay(50);
                }
            }
        }
        catch (Exception ex)
        {
            await assistView!.UpdateResponseAsync($"<div class='error'>Error: {ex.Message}</div>");
        }
        
        await assistView!.UpdateResponseAsync("</div>");
    }
}
```

## Stop Responding

Handle the stop button click using the `ResponseStopped` event:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px;">
    <SfAIAssistView 
        @ref="assistView"
        EnableStreaming="true"
        PromptRequested="@OnPromptRequested"
        ResponseStopped="@OnResponseStopped">
    </SfAIAssistView>
</div>

@if (!string.IsNullOrEmpty(statusMessage))
{
    <div class="alert alert-info mt-2">@statusMessage</div>
}

@code {
    private SfAIAssistView? assistView;
    private CancellationTokenSource? streamingCts;
    private string statusMessage = "";

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        // Create cancellation token for this streaming operation
        streamingCts = new CancellationTokenSource();
        
        args.Response = "<div>";
        statusMessage = "Streaming response...";
        
        try
        {
            await StreamResponseWithCancellation(streamingCts.Token);
        }
        catch (OperationCanceledException)
        {
            await assistView!.UpdateResponseAsync("<div class='text-warning'>Response stopped by user</div>");
            statusMessage = "Streaming stopped";
        }
        
        await assistView!.UpdateResponseAsync("</div>");
    }

    private async Task StreamResponseWithCancellation(CancellationToken cancellationToken)
    {
        var sentences = new[]
        {
            "First sentence of the response.",
            "Second sentence with more information.",
            "Third sentence providing additional context.",
            "Fourth sentence with more details.",
            "Final sentence concluding the response."
        };
        
        foreach (var sentence in sentences)
        {
            cancellationToken.ThrowIfCancellationRequested();
            
            await assistView!.UpdateResponseAsync($" {sentence}");
            await Task.Delay(1000, cancellationToken);
        }
    }

    private void OnResponseStopped(ResponseStoppedEventArgs args)
    {
        // Cancel the streaming operation
        streamingCts?.Cancel();
        
        statusMessage = $"Stopped response for prompt: {args.Prompt}";
        Console.WriteLine($"Streaming stopped at data index: {args.DataIndex}");
    }
}
```

## Real-World Examples

### Example 1: OpenAI Streaming Integration

```csharp
@using System.Net.Http
@using System.Text
@using System.Text.Json
@inject HttpClient Http

@code {
    private SfAIAssistView? assistView;
    private string openAIApiKey = "YOUR_API_KEY";

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        args.Response = "<div>";
        
        var requestBody = new
        {
            model = "gpt-3.5-turbo",
            messages = new[] 
            {
                new { role = "user", content = args.PromptText }
            },
            stream = true
        };
        
        var request = new HttpRequestMessage(HttpMethod.Post, "https://api.openai.com/v1/chat/completions")
        {
            Headers = 
            {
                { "Authorization", $"Bearer {openAIApiKey}" }
            },
            Content = new StringContent(
                JsonSerializer.Serialize(requestBody), 
                Encoding.UTF8, 
                "application/json")
        };
        
        using var response = await Http.SendAsync(request, HttpCompletionOption.ResponseHeadersRead);
        using var stream = await response.Content.ReadAsStreamAsync();
        using var reader = new StreamReader(stream);
        
        string? line;
        while ((line = await reader.ReadLineAsync()) != null)
        {
            if (line.StartsWith("data: ") && !line.Contains("[DONE]"))
            {
                var jsonData = line.Substring(6);
                // Parse and extract content from JSON
                var content = ExtractContentFromJSON(jsonData);
                
                if (!string.IsNullOrEmpty(content))
                {
                    await assistView!.UpdateResponseAsync(content);
                }
            }
        }
        
        await assistView!.UpdateResponseAsync("</div>");
    }

    private string ExtractContentFromJSON(string json)
    {
        try
        {
            using var doc = JsonDocument.Parse(json);
            return doc.RootElement
                .GetProperty("choices")[0]
                .GetProperty("delta")
                .GetProperty("content")
                .GetString() ?? "";
        }
        catch
        {
            return "";
        }
    }
}
```

### Example 2: Azure OpenAI Streaming

```csharp
@code {
    private SfAIAssistView? assistView;
    private string azureEndpoint = "https://YOUR_RESOURCE.openai.azure.com";
    private string azureApiKey = "YOUR_API_KEY";
    private string deploymentName = "gpt-35-turbo";

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        args.Response = "<div>";
        
        var url = $"{azureEndpoint}/openai/deployments/{deploymentName}/chat/completions?api-version=2024-02-15-preview";
        
        var requestBody = new
        {
            messages = new[] 
            {
                new { role = "user", content = args.PromptText }
            },
            stream = true
        };
        
        var request = new HttpRequestMessage(HttpMethod.Post, url)
        {
            Headers = 
            {
                { "api-key", azureApiKey }
            },
            Content = new StringContent(
                JsonSerializer.Serialize(requestBody), 
                Encoding.UTF8, 
                "application/json")
        };
        
        using var response = await Http.SendAsync(request, HttpCompletionOption.ResponseHeadersRead);
        using var stream = await response.Content.ReadAsStreamAsync();
        using var reader = new StreamReader(stream);
        
        string? line;
        while ((line = await reader.ReadLineAsync()) != null)
        {
            if (line.StartsWith("data: ") && !line.Contains("[DONE]"))
            {
                var content = ExtractContentFromJSON(line.Substring(6));
                if (!string.IsNullOrEmpty(content))
                {
                    await assistView!.UpdateResponseAsync(content);
                }
            }
        }
        
        await assistView!.UpdateResponseAsync("</div>");
    }
}
```

### Example 3: Local Ollama Streaming

```csharp
@using System.Net.Http
@inject HttpClient Http

@code {
    private SfAIAssistView? assistView;
    private string ollamaEndpoint = "http://localhost:11434";

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        args.Response = "<div>";
        
        var requestBody = new
        {
            model = "llama2",
            prompt = args.PromptText,
            stream = true
        };
        
        var request = new HttpRequestMessage(HttpMethod.Post, $"{ollamaEndpoint}/api/generate")
        {
            Content = new StringContent(
                JsonSerializer.Serialize(requestBody), 
                Encoding.UTF8, 
                "application/json")
        };
        
        using var response = await Http.SendAsync(request, HttpCompletionOption.ResponseHeadersRead);
        using var stream = await response.Content.ReadAsStreamAsync();
        using var reader = new StreamReader(stream);
        
        string? line;
        while ((line = await reader.ReadLineAsync()) != null)
        {
            if (!string.IsNullOrWhiteSpace(line))
            {
                try
                {
                    using var doc = JsonDocument.Parse(line);
                    var response_text = doc.RootElement.GetProperty("response").GetString();
                    
                    if (!string.IsNullOrEmpty(response_text))
                    {
                        await assistView!.UpdateResponseAsync(response_text);
                    }
                    
                    // Check if done
                    if (doc.RootElement.GetProperty("done").GetBoolean())
                    {
                        break;
                    }
                }
                catch (JsonException)
                {
                    // Skip invalid JSON
                }
            }
        }
        
        await assistView!.UpdateResponseAsync("</div>");
    }
}
```

## Best Practices

### 1. Handle Cancellation Properly

Always support cancellation for streaming operations:

```csharp
@code {
    private CancellationTokenSource? streamingCts;

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        streamingCts?.Cancel();
        streamingCts = new CancellationTokenSource();
        
        try
        {
            await StreamWithCancellation(streamingCts.Token);
        }
        catch (OperationCanceledException)
        {
            // Handle cancellation
        }
    }

    private void OnResponseStopped(ResponseStoppedEventArgs args)
    {
        streamingCts?.Cancel();
    }
}
```

### 2. Provide Visual Feedback

Show streaming status to users:

```csharp
@code {
    private bool isStreaming = false;

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        isStreaming = true;
        StateHasChanged();
        
        try
        {
            // Stream response
            await StreamResponse();
        }
        finally
        {
            isStreaming = false;
            StateHasChanged();
        }
    }
}

@if (isStreaming)
{
    <div class="streaming-indicator">
        <span class="spinner-border spinner-border-sm me-2"></span>
        AI is thinking...
    </div>
}
```

### 3. Handle Errors Gracefully

Catch and display streaming errors:

```csharp
@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        args.Response = "<div>";
        
        try
        {
            await StreamResponse();
        }
        catch (HttpRequestException ex)
        {
            await assistView!.UpdateResponseAsync($"<div class='alert alert-danger'>Network error: {ex.Message}</div>");
        }
        catch (TaskCanceledException)
        {
            await assistView!.UpdateResponseAsync("<div class='alert alert-warning'>Request timed out</div>");
        }
        catch (Exception ex)
        {
            await assistView!.UpdateResponseAsync($"<div class='alert alert-danger'>Error: {ex.Message}</div>");
        }
        
        await assistView!.UpdateResponseAsync("</div>");
    }
}
```

### 4. Optimize Update Frequency

Don't update too frequently to avoid performance issues:

```csharp
@code {
    private async Task StreamWithThrottling()
    {
        var buffer = new StringBuilder();
        var lastUpdate = DateTime.Now;
        var updateInterval = TimeSpan.FromMilliseconds(100);
        
        foreach (var chunk in GetStreamChunks())
        {
            buffer.Append(chunk);
            
            // Only update every 100ms
            if (DateTime.Now - lastUpdate > updateInterval)
            {
                await assistView!.UpdateResponseAsync(buffer.ToString());
                buffer.Clear();
                lastUpdate = DateTime.Now;
            }
        }
        
        // Send remaining buffer
        if (buffer.Length > 0)
        {
            await assistView!.UpdateResponseAsync(buffer.ToString());
        }
    }
}
```

### 5. Clean Up Resources

Dispose of resources properly:

```csharp
@implements IDisposable

@code {
    private CancellationTokenSource? streamingCts;
    private HttpClient? streamingHttpClient;

    public void Dispose()
    {
        streamingCts?.Cancel();
        streamingCts?.Dispose();
        streamingHttpClient?.Dispose();
    }
}
```

### 6. Test Different Network Conditions

Simulate slow connections during development:

```csharp
@code {
    private async Task SimulateSlowStreaming()
    {
        var chunks = new[] { "First", "Second", "Third", "Fourth" };
        
        foreach (var chunk in chunks)
        {
            // Simulate network delay
            await Task.Delay(Random.Shared.Next(200, 800));
            await assistView!.UpdateResponseAsync($" {chunk}");
        }
    }
}
```

## Performance Tips

1. **Buffer small chunks** - Combine multiple small updates into larger ones
2. **Use string builders** - Avoid string concatenation in loops
3. **Throttle updates** - Don't update more than 10-20 times per second
4. **Monitor memory** - Clean up large responses to prevent memory leaks
5. **Handle back-pressure** - Slow down if UI can't keep up
6. **Test with long responses** - Ensure performance with large content
7. **Use cancellation tokens** - Support stopping long-running streams

````
