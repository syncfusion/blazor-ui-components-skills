# Syncfusion Blazor Smart TextArea - Custom AI Integration

## Overview

The Syncfusion Blazor Smart TextArea component leverages AI to provide context-aware autocompletion. While it comes with built-in support for OpenAI and Azure OpenAI, you can also integrate custom AI services using the `IChatInferenceService` interface. This allows you to use alternative AI providers like Claude, DeepSeek, Groq, Gemini, or your own custom AI backend.

## IChatInferenceService Interface

The `IChatInferenceService` interface defines a contract for integrating custom AI services with Syncfusion Smart Components. It standardizes communication between the Smart TextArea and any AI provider.

### Interface Definition

```csharp
using Syncfusion.Blazor.AI;

public interface IChatInferenceService
{
    Task<string> GenerateResponseAsync(ChatParameters options);
}
```

### Interface Details

| Member | Type | Description |
|--------|------|-------------|
| `GenerateResponseAsync` | Method | Asynchronously generates AI responses based on chat parameters |
| `ChatParameters` | Parameter | Contains properties like user input and context information |

### Benefits

- **Provider Agnostic**: Enables seamless switching between AI providers without modifying component code
- **Standardized Contract**: Consistent interface for all AI service implementations
- **Flexible Integration**: Support for any AI service that can process text and return responses
- **Easy Testing**: Mock implementations for unit testing without external API calls

## Simple Mock Implementation Example

Below is a sample implementation of a mock AI service named `MockAIService`. This demonstrates how to implement the `IChatInferenceService` interface by returning context-aware responses.

```csharp
using Syncfusion.Blazor.AI;
using System.Threading.Tasks;

public class MockAIService : IChatInferenceService
{
    public Task<string> GenerateResponseAsync(ChatParameters options)
    {
        // Example: Return predefined responses based on input
        var input = options.Input?.ToLower() ?? string.Empty;
        
        string response = input switch
        {
            var s when s.Contains("thank") => "Thank you for reaching out to us.",
            var s when s.Contains("investigate") => "To investigate this issue, we'll need a reproducible example as a public Git repository.",
            var s when s.Contains("screenshot") => "Could you please post a screenshot of the issue?",
            var s when s.Contains("error") => "An error occurred. Please provide the error message and stack trace.",
            _ => "How can we help you today?"
        };
        
        return Task.FromResult(response);
    }
}
```

## Custom AI Service Implementation Examples

### Claude Integration

Implement Claude as your custom AI service:

```csharp
using Anthropic;
using Syncfusion.Blazor.AI;

public class ClaudeAIService : IChatInferenceService
{
    private readonly string _apiKey;
    private readonly Anthropic.Messages.MessageCreateRequest _client;
    
    public ClaudeAIService(string apiKey)
    {
        _apiKey = apiKey;
    }
    
    public async Task<string> GenerateResponseAsync(ChatParameters options)
    {
        // Implement Claude API integration
        // Return AI-generated response
        var response = "Generated response from Claude";
        return await Task.FromResult(response);
    }
}
```

### DeepSeek Integration

Implement DeepSeek as your custom AI service:

```csharp
using Syncfusion.Blazor.AI;

public class DeepSeekAIService : IChatInferenceService
{
    private readonly string _apiKey;
    
    public DeepSeekAIService(string apiKey)
    {
        _apiKey = apiKey;
    }
    
    public async Task<string> GenerateResponseAsync(ChatParameters options)
    {
        // Implement DeepSeek API integration
        // Return AI-generated response
        var response = "Generated response from DeepSeek";
        return await Task.FromResult(response);
    }
}
```

### Groq Integration

Implement Groq as your custom AI service:

```csharp
using Syncfusion.Blazor.AI;

public class GroqAIService : IChatInferenceService
{
    private readonly string _apiKey;
    
    public GroqAIService(string apiKey)
    {
        _apiKey = apiKey;
    }
    
    public async Task<string> GenerateResponseAsync(ChatParameters options)
    {
        // Implement Groq API integration
        // Return AI-generated response
        var response = "Generated response from Groq";
        return await Task.FromResult(response);
    }
}
```

### Gemini Integration

Implement Google Gemini as your custom AI service:

```csharp
using Google.Generative.AI;
using Syncfusion.Blazor.AI;

public class GeminiAIService : IChatInferenceService
{
    private readonly string _apiKey;
    
    public GeminiAIService(string apiKey)
    {
        _apiKey = apiKey;
    }
    
    public async Task<string> GenerateResponseAsync(ChatParameters options)
    {
        // Implement Gemini API integration
        // Return AI-generated response
        var response = "Generated response from Gemini";
        return await Task.FromResult(response);
    }
}
```

## Registering Custom AI Service

Register your custom AI service in the `~/Program.cs` file of your Blazor Web App:

```csharp
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Web;
using Syncfusion.Blazor;
using Syncfusion.Blazor.AI;
using Syncfusion.Blazor.SmartComponents;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();
builder.Services.AddSyncfusionBlazor();
builder.Services.AddSyncfusionSmartComponents();

// Register the custom AI service
builder.Services.AddSingleton<IChatInferenceService, MockAIService>();

// Or with API key from configuration
var customAIApiKey = builder.Configuration["CustomAI:ApiKey"];
builder.Services.AddSingleton<IChatInferenceService>(sp => 
    new CustomAIService(customAIApiKey));

var app = builder.Build();
// ... rest of configuration
```

### Registration Patterns

**Pattern 1: Simple Registration (for mock or self-hosted services)**
```csharp
builder.Services.AddSingleton<IChatInferenceService, MockAIService>();
```

**Pattern 2: With Configuration (for API-based services)**
```csharp
builder.Services.AddSingleton<IChatInferenceService>(sp => 
{
    var apiKey = builder.Configuration["AI:ApiKey"];
    return new CustomAIService(apiKey);
});
```

**Pattern 3: With Dependency Injection**
```csharp
builder.Services.AddScoped<IChatInferenceService, CustomAIService>();
```

## Testing Custom AI Integration

Follow these steps to test your custom AI service implementation:

### Step 1: Implement and Register
Implement the `IChatInferenceService` interface as shown earlier and register it in `Program.cs`.

### Step 2: Add Smart TextArea Component
Add the Smart TextArea component to your application (e.g., in `Pages/Index.razor`):

```razor
<SfSmartTextArea UserRole="@userRole" 
                Placeholder="Enter your queries here" 
                @bind-Value="prompt" 
                Width="75%" 
                RowCount="5">
</SfSmartTextArea>

@code {
    private string? prompt;
    public string userRole = "Maintainer of an open-source project replying to GitHub issues";
}
```

### Step 3: Run the Application
Press **Ctrl+F5** (Windows) or **⌘+F5** (macOS) to launch the application.

### Step 4: Test Autocompletion
Type phrases in the Smart TextArea to verify the custom AI service is working:
- Type "thank" to trigger responses related to gratitude
- Type "investigate" to get investigation-related suggestions
- Type "error" to get error-related suggestions

### Step 5: Verify Suggestions Display
- Ensure suggestions appear as configured (popup or inline based on `ShowSuggestionOnPopup` setting)
- Verify that responses are contextually appropriate
- Check timing and performance of suggestion generation

## Troubleshooting

### No Suggestions Displayed

**Possible Causes:**
- `IChatInferenceService` implementation is not registered in `Program.cs`
- `GenerateResponseAsync` method is not returning valid responses
- Component configuration may not be correct

**Solutions:**
1. Verify registration in `Program.cs`:
   ```csharp
   builder.Services.AddSingleton<IChatInferenceService, YourService>();
   ```

2. Check error logs in browser console (F12)

3. Add logging to `GenerateResponseAsync`:
   ```csharp
   public async Task<string> GenerateResponseAsync(ChatParameters options)
   {
       Console.WriteLine($"Generating response for: {options.Input}");
       // Your logic here
   }
   ```

4. Ensure the Smart TextArea component is properly configured with `UserRole`

### Dependency Issues

**Solutions:**
1. Verify all required NuGet packages are installed:
   ```
   Install-Package Syncfusion.Blazor.SmartComponents
   ```

2. Run `dotnet restore` to resolve dependencies:
   ```bash
   dotnet restore
   ```

3. Clean and rebuild the solution:
   ```bash
   dotnet clean
   dotnet build
   ```

### Incorrect Responses

**Possible Causes:**
- `ChatParameters` not being processed correctly
- Custom AI API returning unexpected format
- Input/output encoding issues

**Solutions:**
1. Debug `ChatParameters` content:
   ```csharp
   public async Task<string> GenerateResponseAsync(ChatParameters options)
   {
       var input = options.Input;
       Console.WriteLine($"Input: {input}");
       // Verify input is what you expect
   }
   ```

2. Verify API response format matches expected output type (string)

3. Test API integration independently of the Smart TextArea component

4. Add exception handling:
   ```csharp
   public async Task<string> GenerateResponseAsync(ChatParameters options)
   {
       try
       {
           // Your implementation
       }
       catch (Exception ex)
       {
           Console.WriteLine($"Error: {ex.Message}");
           return "An error occurred while generating suggestions.";
       }
   }
   ```

## Advanced: Custom AI Service with Full Integration

```csharp
public class AdvancedCustomAIService : IChatInferenceService
{
    private readonly HttpClient _httpClient;
    private readonly string _apiEndpoint;
    private readonly ILogger<AdvancedCustomAIService> _logger;
    
    public AdvancedCustomAIService(
        HttpClient httpClient, 
        IConfiguration configuration,
        ILogger<AdvancedCustomAIService> logger)
    {
        _httpClient = httpClient;
        _apiEndpoint = configuration["CustomAI:Endpoint"];
        _logger = logger;
    }
    
    public async Task<string> GenerateResponseAsync(ChatParameters options)
    {
        try
        {
            _logger.LogInformation($"Generating response for input: {options.Input}");
            
            var request = new { input = options.Input };
            var content = new StringContent(
                System.Text.Json.JsonSerializer.Serialize(request),
                System.Text.Encoding.UTF8,
                "application/json");
            
            var response = await _httpClient.PostAsync(_apiEndpoint, content);
            response.EnsureSuccessStatusCode();
            
            var result = await response.Content.ReadAsStringAsync();
            _logger.LogInformation($"Response generated successfully");
            
            return result;
        }
        catch (Exception ex)
        {
            _logger.LogError($"Error generating response: {ex.Message}");
            return "Unable to generate response at this time.";
        }
    }
}
```

