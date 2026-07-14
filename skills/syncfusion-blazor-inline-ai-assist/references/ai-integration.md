# AI Provider Integration Guide

## Table of Contents
1. [Overview](#overview)
2. [Integration Patterns](#integration-patterns)
3. [Azure OpenAI Integration](#azure-openai-integration)
4. [OpenAI Integration](#openai-integration)
5. [Ollama Integration](#ollama-integration)
6. [Gemini Integration](#gemini-integration)
7. [LiteLLM Proxy Integration](#litellm-proxy-integration)
8. [Microsoft.Extensions.AI Framework](#microsoftextensionsai-framework)
9. [Provider Comparison](#provider-comparison)
10. [Error Handling](#error-handling)

## Overview

The Inline AI Assist component works by sending user prompts to external AI providers and displaying responses. Multiple AI providers are supported:

- **Azure OpenAI** - Enterprise Azure-hosted OpenAI models
- **OpenAI** - Direct OpenAI API integration
- **Ollama** - Local LLM models running on your machine
- **Gemini** - Google's Gemini API
- **LiteLLM** - Unified proxy for multiple providers
- **Microsoft.Extensions.AI** - .NET unified AI framework

## Integration Patterns

### Pattern 1: Direct HttpClient Integration

Most direct and flexible approach. You manage the HTTP requests and AI provider communication.

```csharp
private async Task OnPromptRequested(PromptRequestedEventArgs args)
{
    try
    {
        using var client = new HttpClient();
        client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", apiKey);
        
        var requestBody = new { prompt = args.Prompt };
        var response = await client.PostAsJsonAsync(providerUrl, requestBody);
        response.EnsureSuccessStatusCode();
        
        var json = await response.Content.ReadFromJsonAsync<JsonElement>();
        var responseText = ExtractResponseText(json);
        
        await inlineAssist.UpdateResponseAsync(responseText, true);
    }
    catch (Exception ex)
    {
        await inlineAssist.UpdateResponseAsync($"Error: {ex.Message}", true);
    }
}
```

**Pros:** Full control, no dependencies, works with any provider
**Cons:** More boilerplate code, need to manage authentication

### Pattern 2: Azure OpenAI SDK

Use the official Azure OpenAI C# SDK for type-safe interactions.

```csharp
var client = new AzureOpenAIClient(endpoint, new AzureKeyCredential(apiKey));
var chatClient = client.GetChatClient(deploymentName);
var response = await chatClient.CompleteChatAsync(messages);
```

**Pros:** Type-safe, SDK features, official support
**Cons:** Additional dependency, Azure-specific

### Pattern 3: Microsoft.Extensions.AI

Unified .NET framework for AI integration. Supports multiple providers with same code.

```csharp
builder.Services.AddChatClient(
    new AzureOpenAIClient(endpoint, credential)
        .GetChatClient(deploymentName)
        .AsIChatClient()
);
```

**Pros:** Provider-agnostic, .NET integrated, consistent API
**Cons:** Newer framework, limited provider support

## Azure OpenAI Integration

Azure OpenAI provides enterprise-grade OpenAI models hosted on Azure infrastructure.

### Prerequisites

1. Azure account with OpenAI access
2. Azure OpenAI resource created in Azure Portal
3. API key and endpoint URL from resource
4. Deployment name for your model

### Step 1: Configure in Program.cs

```csharp
using Azure.AI.OpenAI;
using Azure;
using Microsoft.Extensions.AI;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents();

// Azure OpenAI configuration
var endpoint = "https://your-resource.openai.azure.com";
var apiKey = builder.Configuration["AzureOpenAI:ApiKey"];
var deploymentName = builder.Configuration["AzureOpenAI:DeploymentName"];

var credential = new AzureKeyCredential(apiKey);

// Register chat client (Optional: using Microsoft.Extensions.AI)
builder.Services.AddChatClient(
    new AzureOpenAIClient(new Uri(endpoint), credential)
        .GetChatClient(deploymentName)
        .AsIChatClient()
);

builder.Services.AddSyncfusionBlazor();

var app = builder.Build();

if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Error");
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();
app.UseAntiforgery();

app.MapRazorComponents<App>()
    .AddInteractiveServerRenderMode();

app.Run();
```

### Step 2: Implement in Razor Component

```razor
@page "/azure-openai"
@using Syncfusion.Blazor.InteractiveChat
@using System.Text.Json
@inject HttpClient Http

<SfInlineAIAssist @ref="inlineAssist"
                 PromptRequested="OnPromptRequested"
                 EnableStreaming="true">
    <ResponseActions ItemSelect="OnResponseAction"></ResponseActions>
</SfInlineAIAssist>

@code {
    private SfInlineAIAssist inlineAssist = new();
    
    private const string AzureOpenAIEndpoint = "https://your-resource.openai.azure.com";
    private const string AzureOpenAIDeployment = "gpt-4o-mini";
    private const string AzureOpenAIApiVersion = "2024-02-15-preview";

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        try
        {
            var url = $"{AzureOpenAIEndpoint.TrimEnd('/')}/openai/deployments/{AzureOpenAIDeployment}/chat/completions" +
                     $"?api-version={AzureOpenAIApiVersion}";

            var requestBody = new
            {
                messages = new[]
                {
                    new { role = "user", content = args.Prompt }
                },
                max_tokens = 150,
                temperature = 0.7
            };

            using var request = new HttpRequestMessage(HttpMethod.Post, url);
            request.Content = JsonContent.Create(requestBody);
            request.Headers.Add("api-key", GetAzureOpenAIKey()); // Implement secure key retrieval

            var response = await Http.SendAsync(request);
            response.EnsureSuccessStatusCode();

            var json = await response.Content.ReadFromJsonAsync<JsonElement>();
            var responseText = json
                .GetProperty("choices")[0]
                .GetProperty("message")
                .GetProperty("content")
                .GetString() ?? "No response received";

            await inlineAssist.UpdateResponseAsync(responseText, true);
        }
        catch (Exception ex)
        {
            await inlineAssist.UpdateResponseAsync($"Error: {ex.Message}", true);
        }
    }

    private async Task OnResponseAction(ResponseItemSelectEventArgs args)
    {
        if (args.Item.Label == "Accept")
        {
            await inlineAssist.HidePopupAsync();
        }
    }

    private string GetAzureOpenAIKey()
    {
        // Retrieve from secure configuration, not hardcoded
        return "your-api-key";
    }
}
```

## OpenAI Integration

Direct integration with OpenAI's API.

### Prerequisites

1. OpenAI account at https://openai.com
2. API key generated from account dashboard
3. Sufficient API credits

### Implementation

```razor
@using System.Net.Http.Json
@using System.Text.Json

<SfInlineAIAssist @ref="inlineAssist" PromptRequested="OnPromptRequested">
</SfInlineAIAssist>

@code {
    private SfInlineAIAssist inlineAssist = new();
    private const string OpenAIApiKey = "sk-..."; // Don't hardcode in production
    private const string OpenAIModel = "gpt-4o-mini";
    private const string OpenAIUrl = "https://api.openai.com/v1/chat/completions";

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        try
        {
            var requestBody = new
            {
                model = OpenAIModel,
                messages = new[]
                {
                    new { role = "user", content = args.Prompt }
                },
                max_tokens = 150,
                temperature = 0.7
            };

            using var client = new HttpClient();
            client.DefaultRequestHeaders.Add("Authorization", $"Bearer {OpenAIApiKey}");

            var response = await client.PostAsJsonAsync(OpenAIUrl, requestBody);
            response.EnsureSuccessStatusCode();

            var json = await response.Content.ReadFromJsonAsync<JsonElement>();
            var responseText = json
                .GetProperty("choices")[0]
                .GetProperty("message")
                .GetProperty("content")
                .GetString() ?? "No response";

            await inlineAssist.UpdateResponseAsync(responseText, true);
        }
        catch (Exception ex)
        {
            await inlineAssist.UpdateResponseAsync($"Error: {ex.Message}", true);
        }
    }
}
```

## Ollama Integration

Run LLMs locally using Ollama. Great for development and privacy-sensitive applications.

### Prerequisites

1. Ollama installed on your system (https://ollama.com)
2. Model downloaded: `ollama run llama2` or similar
3. Ollama running: `ollama serve`

### Configuration

```csharp
using OllamaSharp;
using Microsoft.Extensions.AI;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents();

// Configure Ollama
builder.Services.AddChatClient(
    new OllamaApiClient(new Uri("http://localhost:11434/"), "llama2")
        .UseDistributedCache()
        .UseLogging()
);

builder.Services.AddSyncfusionBlazor();
```

### Implementation

```razor
<SfInlineAIAssist @ref="inlineAssist" PromptRequested="OnPromptRequested">
</SfInlineAIAssist>

@code {
    private SfInlineAIAssist inlineAssist = new();
    private const string OllamaUrl = "http://localhost:11434/api/generate";
    private const string OllamaModel = "llama2";

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        try
        {
            var requestBody = new
            {
                model = OllamaModel,
                prompt = args.Prompt,
                stream = false
            };

            using var client = new HttpClient();
            var response = await client.PostAsJsonAsync(OllamaUrl, requestBody);
            response.EnsureSuccessStatusCode();

            var json = await response.Content.ReadFromJsonAsync<JsonElement>();
            var responseText = json.GetProperty("response").GetString() ?? "No response";

            await inlineAssist.UpdateResponseAsync(responseText, true);
        }
        catch (Exception ex)
        {
            await inlineAssist.UpdateResponseAsync($"Error: {ex.Message}", true);
        }
    }
}
```

## Gemini Integration

Google's Gemini API for modern AI capabilities.

### Prerequisites

1. Google account
2. API key from Google AI Studio (https://aistudio.google.com/app/apikey)
3. Gemini API enabled

### Implementation

```razor
@using System.Text.Json

<SfInlineAIAssist @ref="inlineAssist" PromptRequested="OnPromptRequested">
</SfInlineAIAssist>

@code {
    private SfInlineAIAssist inlineAssist = new();
    private const string GeminiApiKey = "your-api-key";
    private const string GeminiModel = "gemini-2.5-flash";

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        try
        {
            var url = $"https://generativelanguage.googleapis.com/v1beta/models/{GeminiModel}:generateContent?key={GeminiApiKey}";

            var requestBody = new
            {
                contents = new[]
                {
                    new { parts = new[] { new { text = args.Prompt } } }
                }
            };

            using var client = new HttpClient();
            var response = await client.PostAsJsonAsync(url, requestBody);
            response.EnsureSuccessStatusCode();

            var json = await response.Content.ReadFromJsonAsync<JsonElement>();
            var responseText = json
                .GetProperty("candidates")[0]
                .GetProperty("content")
                .GetProperty("parts")[0]
                .GetProperty("text")
                .GetString() ?? "No response";

            await inlineAssist.UpdateResponseAsync(responseText, true);
        }
        catch (Exception ex)
        {
            await inlineAssist.UpdateResponseAsync($"Error: {ex.Message}", true);
        }
    }
}
```

## LiteLLM Proxy Integration

Use LiteLLM to support multiple providers with a single API.

### Setup LiteLLM

1. Install Python and LiteLLM:
```bash
pip install litellm
```

2. Create `config.yaml`:
```yaml
model_list:
  - model_name: gpt-4o
    litellm_params:
      model: gpt-4o-mini
      api_key: $OPENAI_API_KEY
```

3. Run proxy:
```bash
litellm --config ./config.yaml --port 4000
```

### Implementation

```razor
<SfInlineAIAssist @ref="inlineAssist" PromptRequested="OnPromptRequested">
</SfInlineAIAssist>

@code {
    private SfInlineAIAssist inlineAssist = new();
    private const string LiteLLMUrl = "http://localhost:4000/v1/chat/completions";
    private const string LiteLLMModel = "gpt-4o";

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        try
        {
            var requestBody = new
            {
                model = LiteLLMModel,
                messages = new[] { new { role = "user", content = args.Prompt } }
            };

            using var client = new HttpClient();
            var response = await client.PostAsJsonAsync(LiteLLMUrl, requestBody);
            response.EnsureSuccessStatusCode();

            var json = await response.Content.ReadFromJsonAsync<JsonElement>();
            var responseText = json
                .GetProperty("choices")[0]
                .GetProperty("message")
                .GetProperty("content")
                .GetString() ?? "No response";

            await inlineAssist.UpdateResponseAsync(responseText, true);
        }
        catch (Exception ex)
        {
            await inlineAssist.UpdateResponseAsync($"Error: {ex.Message}", true);
        }
    }
}
```

## Microsoft.Extensions.AI Framework

Unified .NET framework for AI integration.

### Setup

```csharp
using Microsoft.Extensions.AI;
using Azure.AI.OpenAI;
using Azure;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents();

// Register chat client using Microsoft.Extensions.AI
var endpoint = builder.Configuration["AzureOpenAI:Endpoint"];
var apiKey = builder.Configuration["AzureOpenAI:ApiKey"];
var deploymentName = builder.Configuration["AzureOpenAI:DeploymentName"];

builder.Services.AddChatClient(
    new AzureOpenAIClient(new Uri(endpoint), new AzureKeyCredential(apiKey))
        .GetChatClient(deploymentName)
        .AsIChatClient()
);

builder.Services.AddSyncfusionBlazor();
```

### Usage in Component

```razor
@inject IChatClient ChatClient

<SfInlineAIAssist @ref="inlineAssist" PromptRequested="OnPromptRequested">
</SfInlineAIAssist>

@code {
    private SfInlineAIAssist inlineAssist = new();
    private IChatClient? chatClient;

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        try
        {
            if (chatClient == null)
                throw new InvalidOperationException("Chat client not configured");

            var messages = new[] 
            { 
                new ChatMessage(ChatRole.User, args.Prompt) 
            };

            var response = await chatClient.GetChatCompletionAsync(messages);
            var responseText = response.Message.Text ?? "No response";

            await inlineAssist.UpdateResponseAsync(responseText, true);
        }
        catch (Exception ex)
        {
            await inlineAssist.UpdateResponseAsync($"Error: {ex.Message}", true);
        }
    }
}
```

## Provider Comparison

| Feature | Azure OpenAI | OpenAI | Ollama | Gemini | LiteLLM |
|---------|--------------|--------|--------|--------|---------|
| Cost | Paid | Paid | Free | Free/Paid | Proxy cost |
| Setup Complexity | Medium | Easy | Medium | Easy | High |
| Latency | Low | Low | Variable | Low | Variable |
| Privacy | Enterprise | Cloud | Local | Cloud | Flexible |
| Model Options | Limited | Multiple | Many | Multiple | All |
| Best For | Enterprise | General use | Development | Advanced | Multi-provider |

## Error Handling

### Common Errors and Solutions

#### 401 Unauthorized
**Cause:** Invalid or missing API key
```csharp
catch (HttpRequestException ex) when (ex.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
    await inlineAssist.UpdateResponseAsync("Authentication failed. Check API key.", true);
}
```

#### 429 Rate Limited
**Cause:** Too many requests
```csharp
if (response.StatusCode == System.Net.HttpStatusCode.TooManyRequests)
{
    await inlineAssist.UpdateResponseAsync("Rate limited. Please wait before trying again.", true);
    // Implement exponential backoff
}
```

#### Connection Timeout
**Cause:** Network issue or provider unavailable
```csharp
catch (HttpRequestException ex) when (ex.InnerException is TimeoutException)
{
    await inlineAssist.UpdateResponseAsync("Connection timeout. Check your network and provider.", true);
}
```

#### Invalid Response Format
**Cause:** Provider response doesn't match expected format
```csharp
try
{
    var responseText = json.GetProperty("choices")[0].GetProperty("message").GetProperty("content").GetString();
}
catch (KeyNotFoundException)
{
    await inlineAssist.UpdateResponseAsync("Invalid response from AI provider.", true);
}
```

## Best Practices

1. **Never hardcode API keys** - Use configuration, environment variables, or secure vaults
2. **Implement retry logic** - Handle transient failures gracefully
3. **Set timeouts** - Prevent indefinite hangs
4. **Monitor API usage** - Track costs and quota
5. **Log errors** - Implement comprehensive logging
6. **Stream large responses** - Better UX for long responses
7. **Validate input** - Check prompt validity before sending
8. **Cache responses** - Reduce API calls where appropriate
9. **Handle rate limits** - Implement exponential backoff
10. **Test error scenarios** - Network failures, invalid responses, etc.

## Security Considerations

- Store API keys in Azure Key Vault or similar
- Use environment variables for sensitive config
- Don't expose keys in client-side code
- Implement server-side proxy for key management
- Use HTTPS for all API calls
- Validate and sanitize user input
- Monitor for unusual activity
- Implement access controls
