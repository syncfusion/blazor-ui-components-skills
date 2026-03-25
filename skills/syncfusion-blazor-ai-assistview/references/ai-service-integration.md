# AI Service Integration

## Table of Contents
- [Overview](#overview)
- [Service Integration Patterns](#service-integration-patterns)
- [OpenAI Integration](#openai-integration)
- [Azure OpenAI Integration](#azure-openai-integration)
- [Local Ollama Integration](#local-ollama-integration)
- [Custom Backend Integration](#custom-backend-integration)
- [Error Handling Strategies](#error-handling-strategies)
- [Rate Limiting & Throttling](#rate-limiting--throttling)
- [Best Practices](#best-practices)

## Overview

The AI AssistView component is designed to integrate seamlessly with various AI service backends. This guide covers integration patterns for popular AI services including OpenAI, Azure OpenAI, and local Ollama deployments, as well as custom backends.

**Supported Integration Patterns:**
- **OpenAI API** - Cloud-hosted GPT models with streaming support
- **Azure OpenAI** - Enterprise-grade OpenAI deployment on Azure
- **Ollama** - Local LLM hosting for privacy and offline capability
- **Custom Backends** - Your own AI service or inference engine

## Service Integration Patterns

### Architecture Overview

```
┌─────────────────────────────────────┐
│   SfAIAssistView Component          │
│  (User Interaction & UI)            │
└────────────────┬────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────┐
│   PromptRequested Event Handler      │
│  (Orchestration Logic)              │
└────────────────┬────────────────────┘
                 │
         ┌───────┴───────┬──────────┐
         ▼               ▼          ▼
    ┌────────┐      ┌────────┐  ┌────────┐
    │ OpenAI │      │ Azure  │  │ Ollama │
    │  API   │      │ OpenAI │  │ Local  │
    └────────┘      └────────┘  └────────┘
```

### Common Integration Workflow

```csharp
private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
{
    try
    {
        // 1. Validate input
        if (string.IsNullOrWhiteSpace(args.Prompt))
        {
            args.Response = "<div class='error'>Please enter a prompt</div>";
            return;
        }

        // 2. Show loading state (optional)
        args.Response = "<div class='loading'>Processing your request...</div>";

        // 3. Call AI service
        var response = await CallAIService(args.Prompt);

        // 4. Update response
        args.Response = response;
    }
    catch (Exception ex)
    {
        args.Response = $"<div class='error'>Error: {ex.Message}</div>";
        LogError(ex);
    }
}

private async Task<string> CallAIService(string prompt)
{
    // Implementation varies by service
    return "AI response";
}

private void LogError(Exception ex)
{
    Console.WriteLine($"AI Service Error: {ex}");
    // Log to your monitoring system
}
```

---

## OpenAI Integration

### Setup

1. **Obtain API Key:**
   - Visit [platform.openai.com](https://platform.openai.com/account/api-keys)
   - Create or retrieve your API key
   - Store securely in `appsettings.json` or environment variables

2. **Configure in Program.cs:**
```csharp
// Program.cs
var openAIApiKey = builder.Configuration["OpenAI:ApiKey"];
builder.Services.AddScoped<IOpenAIService>(sp => 
    new OpenAIService(openAIApiKey));
```

3. **Add HttpClient:**
```csharp
builder.Services.AddScoped<HttpClient>(sp =>
{
    var client = new HttpClient();
    client.DefaultRequestHeaders.Add("Authorization", $"Bearer {openAIApiKey}");
    return client;
});
```

### Non-Streaming Implementation

```csharp
@using Syncfusion.Blazor.InteractiveChat
@using System.Text.Json
@inject HttpClient Http

<div style="height: 400px;">
    <SfAIAssistView PromptRequested="@OnPromptRequested"></SfAIAssistView>
</div>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        try
        {
            var response = await CallOpenAI(args.Prompt);
            args.Response = response;
        }
        catch (Exception ex)
        {
            args.Response = $"<div class='error'>Error: {ex.Message}</div>";
        }
    }

    private async Task<string> CallOpenAI(string prompt)
    {
        var requestBody = new
        {
            model = "gpt-3.5-turbo",
            messages = new[] 
            {
                new { role = "system", content = "You are a helpful assistant." },
                new { role = "user", content = prompt }
            },
            temperature = 0.7,
            max_tokens = 1000
        };

        try
        {
            var response = await Http.PostAsJsonAsync(
                "https://api.openai.com/v1/chat/completions",
                requestBody);

            response.EnsureSuccessStatusCode();

            var content = await response.Content.ReadAsStringAsync();
            using var doc = JsonDocument.Parse(content);
            
            var messageContent = doc.RootElement
                .GetProperty("choices")[0]
                .GetProperty("message")
                .GetProperty("content")
                .GetString();

            return $"<div>{messageContent}</div>";
        }
        catch (HttpRequestException ex)
        {
            return $"<div class='error'>Failed to call OpenAI API: {ex.Message}</div>";
        }
    }
}
```

### Streaming Implementation

```csharp
@using Syncfusion.Blazor.InteractiveChat
@inject HttpClient Http

<div style="height: 400px;">
    <SfAIAssistView 
        @ref="assistView"
        EnableStreaming="true"
        PromptRequested="@OnPromptRequested">
    </SfAIAssistView>
</div>

@code {
    private SfAIAssistView? assistView;
    private string openAIApiKey = "YOUR_API_KEY"; // Store securely!

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        args.Response = "<div>";
        
        try
        {
            await StreamOpenAI(args.Prompt);
        }
        catch (Exception ex)
        {
            await assistView!.UpdateResponseAsync(
                $"<div class='error'>Error: {ex.Message}</div>");
        }
        
        await assistView!.UpdateResponseAsync("</div>");
    }

    private async Task StreamOpenAI(string prompt)
    {
        var requestBody = new
        {
            model = "gpt-3.5-turbo",
            messages = new[] 
            {
                new { role = "user", content = prompt }
            },
            stream = true,
            temperature = 0.7
        };

        var request = new HttpRequestMessage(HttpMethod.Post, 
            "https://api.openai.com/v1/chat/completions")
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

        using var response = await Http.SendAsync(request, 
            HttpCompletionOption.ResponseHeadersRead);
        using var stream = await response.Content.ReadAsStreamAsync();
        using var reader = new StreamReader(stream);

        string? line;
        while ((line = await reader.ReadLineAsync()) != null)
        {
            if (line.StartsWith("data: ") && !line.Contains("[DONE]"))
            {
                var jsonData = line.Substring(6);
                var content = ExtractOpenAIContent(jsonData);
                
                if (!string.IsNullOrEmpty(content))
                {
                    await assistView!.UpdateResponseAsync(content);
                }
            }
        }
    }

    private string ExtractOpenAIContent(string json)
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

### Configuration Options

```csharp
private class OpenAIConfig
{
    public string Model { get; set; } = "gpt-3.5-turbo"; // or "gpt-4"
    public float Temperature { get; set; } = 0.7f; // 0-2, higher = more creative
    public int MaxTokens { get; set; } = 2000;
    public float TopP { get; set; } = 1f; // nucleus sampling
    public int FrequencyPenalty { get; set; } = 0;
    public int PresencePenalty { get; set; } = 0;
}

// Usage:
var config = new OpenAIConfig { Temperature = 0.5f, Model = "gpt-4" };
await StreamOpenAI(prompt, config);
```

---

## Azure OpenAI Integration

### Setup

1. **Create Azure OpenAI Resource:**
   - Deploy in Azure Portal
   - Note: Deployment name, Endpoint, and API Key

2. **Configure:**
```csharp
// appsettings.json
{
  "AzureOpenAI": {
    "Endpoint": "https://YOUR_RESOURCE.openai.azure.com/",
    "ApiKey": "YOUR_API_KEY",
    "DeploymentName": "gpt-35-turbo"
  }
}
```

### Implementation

```csharp
@using Syncfusion.Blazor.InteractiveChat
@using System.Text.Json
@inject HttpClient Http
@inject IConfiguration Config

<div style="height: 400px;">
    <SfAIAssistView 
        @ref="assistView"
        EnableStreaming="true"
        PromptRequested="@OnPromptRequested">
    </SfAIAssistView>
</div>

@code {
    private SfAIAssistView? assistView;
    private string azureEndpoint = "";
    private string azureApiKey = "";
    private string deploymentName = "";

    protected override void OnInitialized()
    {
        azureEndpoint = Config["AzureOpenAI:Endpoint"];
        azureApiKey = Config["AzureOpenAI:ApiKey"];
        deploymentName = Config["AzureOpenAI:DeploymentName"];
    }

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        args.Response = "<div>";
        
        try
        {
            await StreamAzureOpenAI(args.Prompt);
        }
        catch (Exception ex)
        {
            await assistView!.UpdateResponseAsync(
                $"<div class='error'>Error: {ex.Message}</div>");
        }
        
        await assistView!.UpdateResponseAsync("</div>");
    }

    private async Task StreamAzureOpenAI(string prompt)
    {
        var url = $"{azureEndpoint}openai/deployments/{deploymentName}/chat/completions?api-version=2024-02-15-preview";
        
        var requestBody = new
        {
            messages = new[] 
            {
                new { role = "system", content = "You are a helpful AI assistant." },
                new { role = "user", content = prompt }
            },
            stream = true,
            temperature = 0.7
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

        using var response = await Http.SendAsync(request, 
            HttpCompletionOption.ResponseHeadersRead);
        using var stream = await response.Content.ReadAsStreamAsync();
        using var reader = new StreamReader(stream);

        string? line;
        while ((line = await reader.ReadLineAsync()) != null)
        {
            if (line.StartsWith("data: ") && !line.Contains("[DONE]"))
            {
                var content = ExtractAzureContent(line.Substring(6));
                if (!string.IsNullOrEmpty(content))
                {
                    await assistView!.UpdateResponseAsync(content);
                }
            }
        }
    }

    private string ExtractAzureContent(string json)
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

### API Version Management

```csharp
private string GetApiVersionUrl()
{
    // Azure OpenAI API versions: 2023-05-15, 2023-06-01-preview, 2024-02-15-preview
    return $"api-version=2024-02-15-preview";
}

// Deployment naming conventions
private string GetDeploymentUrl(string resourceName, string deployment)
{
    return $"https://{resourceName}.openai.azure.com/openai/deployments/{deployment}/chat/completions";
}
```

---

## Local Ollama Integration

### Setup

1. **Install Ollama:**
   - Download from [ollama.ai](https://ollama.ai)
   - Run: `ollama serve`
   - Default endpoint: `http://localhost:11434`

2. **Pull a Model:**
```bash
ollama pull llama2
ollama pull mistral
ollama pull neural-chat
```

3. **Verify Connection:**
```csharp
// Test endpoint availability
var client = new HttpClient();
var response = await client.GetAsync("http://localhost:11434/api/tags");
// If successful, Ollama is running
```

### Implementation

```csharp
@using Syncfusion.Blazor.InteractiveChat
@using System.Text.Json
@inject HttpClient Http

<div style="height: 400px;">
    <SfAIAssistView 
        @ref="assistView"
        EnableStreaming="true"
        PromptRequested="@OnPromptRequested">
    </SfAIAssistView>
</div>

<div class="mt-3">
    <select @bind="selectedModel">
        <option value="llama2">Llama 2</option>
        <option value="mistral">Mistral</option>
        <option value="neural-chat">Neural Chat</option>
    </select>
</div>

@code {
    private SfAIAssistView? assistView;
    private string ollamaEndpoint = "http://localhost:11434";
    private string selectedModel = "llama2";

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        args.Response = "<div>";
        
        try
        {
            await StreamOllama(args.Prompt);
        }
        catch (Exception ex)
        {
            await assistView!.UpdateResponseAsync(
                $"<div class='error'>Error: {ex.Message}</div>");
        }
        
        await assistView!.UpdateResponseAsync("</div>");
    }

    private async Task StreamOllama(string prompt)
    {
        var requestBody = new
        {
            model = selectedModel,
            prompt = prompt,
            stream = true,
            temperature = 0.7
        };

        var request = new HttpRequestMessage(HttpMethod.Post, 
            $"{ollamaEndpoint}/api/generate")
        {
            Content = new StringContent(
                JsonSerializer.Serialize(requestBody), 
                Encoding.UTF8, 
                "application/json")
        };

        using var response = await Http.SendAsync(request, 
            HttpCompletionOption.ResponseHeadersRead);
        
        if (!response.IsSuccessStatusCode)
        {
            throw new Exception($"Ollama API error: {response.StatusCode}");
        }

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
                    var responseText = doc.RootElement
                        .GetProperty("response")
                        .GetString();
                    
                    if (!string.IsNullOrEmpty(responseText))
                    {
                        await assistView!.UpdateResponseAsync(responseText);
                    }

                    // Check if generation is complete
                    if (doc.RootElement.GetProperty("done").GetBoolean())
                    {
                        break;
                    }
                }
                catch (JsonException)
                {
                    // Skip malformed lines
                }
            }
        }
    }

    private async Task<List<string>> GetAvailableModels()
    {
        try
        {
            var response = await Http.GetAsync($"{ollamaEndpoint}/api/tags");
            var content = await response.Content.ReadAsStringAsync();
            using var doc = JsonDocument.Parse(content);
            
            var models = new List<string>();
            foreach (var model in doc.RootElement.GetProperty("models").EnumerateArray())
            {
                models.Add(model.GetProperty("name").GetString()!);
            }
            return models;
        }
        catch
        {
            return new List<string>();
        }
    }
}
```

### Available Models

| Model | Size | Speed | Quality | Notes |
|-------|------|-------|---------|-------|
| neural-chat | 4GB | Very Fast | Good | Best for chat |
| mistral | 4GB | Fast | Very Good | Excellent reasoning |
| llama2 | 7GB | Medium | Excellent | Balanced performance |
| llama2:13b | 25GB | Slow | Excellent | High quality responses |

---

## Custom Backend Integration

### Abstract Service Pattern

```csharp
// Define interface for any AI service
public interface IAIService
{
    Task<string> GetResponseAsync(string prompt);
    Task StreamResponseAsync(string prompt, Func<string, Task> onChunkReceived);
}

// Implement for your backend
public class CustomAIService : IAIService
{
    private readonly HttpClient _httpClient;
    private readonly string _apiEndpoint;

    public CustomAIService(HttpClient httpClient, string apiEndpoint)
    {
        _httpClient = httpClient;
        _apiEndpoint = apiEndpoint;
    }

    public async Task<string> GetResponseAsync(string prompt)
    {
        var request = new { prompt = prompt };
        var response = await _httpClient.PostAsJsonAsync(
            $"{_apiEndpoint}/chat", request);
        
        response.EnsureSuccessStatusCode();
        
        var result = await response.Content.ReadAsAsync<dynamic>();
        return result.response;
    }

    public async Task StreamResponseAsync(string prompt, 
        Func<string, Task> onChunkReceived)
    {
        var request = new { prompt = prompt };
        var httpRequest = new HttpRequestMessage(HttpMethod.Post, 
            $"{_apiEndpoint}/chat/stream")
        {
            Content = new StringContent(
                JsonSerializer.Serialize(request), 
                Encoding.UTF8, 
                "application/json")
        };

        using var response = await _httpClient.SendAsync(httpRequest, 
            HttpCompletionOption.ResponseHeadersRead);
        using var stream = await response.Content.ReadAsStreamAsync();
        using var reader = new StreamReader(stream);

        string? line;
        while ((line = await reader.ReadLineAsync()) != null)
        {
            if (!string.IsNullOrWhiteSpace(line))
            {
                await onChunkReceived(line);
            }
        }
    }
}
```

### Usage in Component

```csharp
@using Syncfusion.Blazor.InteractiveChat
@inject IAIService aiService

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
        args.Response = "<div>";
        
        try
        {
            await aiService.StreamResponseAsync(args.Prompt, async chunk =>
            {
                await assistView!.UpdateResponseAsync(chunk);
            });
        }
        catch (Exception ex)
        {
            await assistView!.UpdateResponseAsync(
                $"<div class='error'>Error: {ex.Message}</div>");
        }
        
        await assistView!.UpdateResponseAsync("</div>");
    }
}
```

---

## Error Handling Strategies

### Network Error Handling

```csharp
private async Task<string> CallWithRetry(string prompt, int maxRetries = 3)
{
    for (int attempt = 0; attempt < maxRetries; attempt++)
    {
        try
        {
            return await CallAIService(prompt);
        }
        catch (HttpRequestException) when (attempt < maxRetries - 1)
        {
            // Exponential backoff: 1s, 2s, 4s
            await Task.Delay(1000 * (int)Math.Pow(2, attempt));
        }
        catch (Exception ex)
        {
            return $"<div class='error'>Failed after {maxRetries} attempts: {ex.Message}</div>";
        }
    }
    
    return "<div class='error'>Service unavailable</div>";
}
```

### Timeout Handling

```csharp
private async Task<string> CallWithTimeout(string prompt, TimeSpan timeout)
{
    using var cts = new CancellationTokenSource(timeout);
    try
    {
        return await CallAIServiceAsync(prompt, cts.Token);
    }
    catch (OperationCanceledException)
    {
        return "<div class='error'>Request timed out (> " + 
               timeout.TotalSeconds + "s)</div>";
    }
}
```

### Fallback Service Pattern

```csharp
private async Task<string> CallWithFallback(string prompt)
{
    try
    {
        return await CallPrimaryService(prompt);
    }
    catch (Exception ex1)
    {
        Console.WriteLine($"Primary service failed: {ex1.Message}");
        try
        {
            return await CallSecondaryService(prompt);
        }
        catch (Exception ex2)
        {
            Console.WriteLine($"Secondary service failed: {ex2.Message}");
            return "<div class='error'>All services unavailable</div>";
        }
    }
}
```

---

## Rate Limiting & Throttling

### Token Bucket Rate Limiter

```csharp
public class RateLimiter
{
    private DateTime _lastRefillTime = DateTime.Now;
    private double _tokensAvailable = 0;
    private readonly double _tokensPerSecond;
    private readonly double _maxTokens;

    public RateLimiter(double tokensPerSecond, double maxTokens)
    {
        _tokensPerSecond = tokensPerSecond;
        _maxTokens = maxTokens;
        _tokensAvailable = maxTokens;
    }

    public async Task WaitIfNeeded(double tokensRequired = 1)
    {
        RefillTokens();

        while (_tokensAvailable < tokensRequired)
        {
            await Task.Delay(100);
            RefillTokens();
        }

        _tokensAvailable -= tokensRequired;
    }

    private void RefillTokens()
    {
        var now = DateTime.Now;
        var timePassed = (now - _lastRefillTime).TotalSeconds;
        _tokensAvailable = Math.Min(_maxTokens, 
            _tokensAvailable + timePassed * _tokensPerSecond);
        _lastRefillTime = now;
    }
}

// Usage:
private RateLimiter limiter = new RateLimiter(tokensPerSecond: 5, maxTokens: 10);

private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
{
    await limiter.WaitIfNeeded();
    var response = await CallAIService(args.Prompt);
    args.Response = response;
}
```

### Request Queue

```csharp
public class AIServiceQueue
{
    private readonly Queue<Func<Task<string>>> _requestQueue = new();
    private bool _isProcessing = false;

    public async Task<string> EnqueueRequest(Func<Task<string>> request)
    {
        var taskCompletionSource = new TaskCompletionSource<string>();
        
        _requestQueue.Enqueue(async () =>
        {
            try
            {
                var result = await request();
                taskCompletionSource.SetResult(result);
            }
            catch (Exception ex)
            {
                taskCompletionSource.SetException(ex);
            }
        });

        if (!_isProcessing)
        {
            _ = ProcessQueue();
        }

        return await taskCompletionSource.Task;
    }

    private async Task ProcessQueue()
    {
        _isProcessing = true;
        while (_requestQueue.Count > 0)
        {
            var request = _requestQueue.Dequeue();
            await request();
            await Task.Delay(500); // Rate limit between requests
        }
        _isProcessing = false;
    }
}
```

---

## Best Practices

### 1. Secure API Key Management

```csharp
// ✓ DO: Use configuration and secrets
var apiKey = builder.Configuration["OpenAI:ApiKey"];

// ✗ DON'T: Hardcode API keys
// var apiKey = "sk-abc123...";

// ✓ DO: Use User Secrets in development
// dotnet user-secrets set "OpenAI:ApiKey" "sk-abc123..."
```

### 2. Implement Logging

```csharp
@inject ILogger<AIComponent> Logger

private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
{
    Logger.LogInformation("Processing prompt: {Prompt}", args.Prompt);
    
    try
    {
        var response = await CallAIService(args.Prompt);
        args.Response = response;
        
        Logger.LogInformation("Response generated successfully");
    }
    catch (Exception ex)
    {
        Logger.LogError(ex, "AI service call failed");
        args.Response = "<div class='error'>An error occurred</div>";
    }
}
```

### 3. Handle Streaming Cancellation

```csharp
private CancellationTokenSource? _streamingCts;

private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
{
    _streamingCts = new CancellationTokenSource();
    
    try
    {
        await StreamResponse(args.Prompt, _streamingCts.Token);
    }
    catch (OperationCanceledException)
    {
        // User stopped the response
    }
}

private void OnResponseStopped(ResponseStoppedEventArgs args)
{
    _streamingCts?.Cancel();
}
```

### 4. Validate Input

```csharp
private string ValidatePrompt(string prompt)
{
    if (string.IsNullOrWhiteSpace(prompt))
        return "Prompt cannot be empty";
    
    if (prompt.Length > 10000)
        return "Prompt exceeds maximum length (10000 characters)";
    
    return ""; // Valid
}

private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
{
    var validationError = ValidatePrompt(args.Prompt);
    if (!string.IsNullOrEmpty(validationError))
    {
        args.Response = $"<div class='error'>{validationError}</div>";
        return;
    }
    
    // Process prompt
}
```

### 5. Monitor Resource Usage

```csharp
private async Task<string> CallWithResourceMonitoring(string prompt)
{
    var startMemory = GC.GetTotalMemory(false);
    var startTime = DateTime.Now;

    try
    {
        var response = await CallAIService(prompt);
        
        var endMemory = GC.GetTotalMemory(false);
        var duration = DateTime.Now - startTime;
        
        Logger.LogInformation(
            "AI call - Duration: {Duration}ms, Memory delta: {Memory}KB",
            duration.TotalMilliseconds,
            (endMemory - startMemory) / 1024);
        
        return response;
    }
    catch (Exception ex)
    {
        Logger.LogError(ex, "AI service failed");
        return "<div class='error'>Service error</div>";
    }
}
```

### 6. Test Different Services

```csharp
#if DEBUG
private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
{
    // Test mode: cycle through services
    var service = args.Prompt.StartsWith("azure:") ? "azure" :
                  args.Prompt.StartsWith("ollama:") ? "ollama" : "openai";
    
    var prompt = args.Prompt.Replace("azure:", "").Replace("ollama:", "");
    
    var response = service switch
    {
        "azure" => await CallAzureOpenAI(prompt),
        "ollama" => await CallOllama(prompt),
        _ => await CallOpenAI(prompt)
    };
    
    args.Response = response;
}
#endif
```

---

## Summary

This guide provides production-ready integration patterns for connecting the AI AssistView component to various AI service backends:

- **OpenAI** - Cloud-hosted with excellent quality
- **Azure OpenAI** - Enterprise deployment option
- **Ollama** - Local hosting for privacy
- **Custom** - Your own backend service

All patterns include error handling, rate limiting, and best practices for production use. Choose the service that best fits your requirements and customize the implementations as needed.

