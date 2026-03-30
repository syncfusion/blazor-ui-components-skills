# AI Backend Configuration

## Table of Contents
- [OpenAI](#openai)
- [Azure OpenAI](#azure-openai)
- [Ollama (Local / Self-hosted)](#ollama-local--self-hosted)
- [Custom IChatClient](#custom-ichatclient)

All backends follow the same pattern: install NuGet packages → create `IChatClient` → register with `AddChatClient()` → register `SyncfusionAIService`.

---

## OpenAI

### Prerequisites
- Active OpenAI account and API key from [platform.openai.com](https://platform.openai.com/)

### Supported Models
| Model | Notes |
|---|---|
| `gpt-4-turbo` | Most capable, latest |
| `gpt-4` | Advanced reasoning |
| `gpt-3.5-turbo` | Fast, cost-effective |
| `gpt-3.5-turbo-16k` | Extended context |

### NuGet Packages
```powershell
Install-Package Microsoft.Extensions.AI
Install-Package Microsoft.Extensions.AI.OpenAI
```

### Program.cs
```csharp
using Syncfusion.Blazor;
using Syncfusion.Blazor.AI;
using Microsoft.Extensions.AI;
using OpenAI;

builder.Services.AddSyncfusionBlazor();

string openAIApiKey = "YOUR_API_KEY"; // Use env vars or User Secrets in production
string openAIModel  = "gpt-4";

OpenAIClient openAIClient = new OpenAIClient(openAIApiKey);
IChatClient chatClient = openAIClient.GetChatClient(openAIModel).AsIChatClient();
builder.Services.AddChatClient(chatClient);

builder.Services.AddSingleton<IChatInferenceService, SyncfusionAIService>();
```

### Secure Key Storage

**Environment variable (Windows):**
```powershell
$env:OPENAI_API_KEY = "your-api-key"
```

**Read from environment:**
```csharp
string openAIApiKey = Environment.GetEnvironmentVariable("OPENAI_API_KEY")
    ?? throw new InvalidOperationException("OpenAI API key not found");
```

**User Secrets (development):**
```bash
dotnet user-secrets set "OpenAI:ApiKey" "your-api-key"
```
```csharp
string openAIApiKey = builder.Configuration["OpenAI:ApiKey"];
```

### Troubleshooting
| Error | Fix |
|---|---|
| 401 Unauthorized | Verify API key is correct and not expired |
| 429 Too Many Requests | Reduce request frequency; check usage limits |
| Model not found | Verify model name at [openai.com/docs/models](https://platform.openai.com/docs/models) |

---

## Azure OpenAI

### Prerequisites
- Active Azure subscription with Azure OpenAI resource deployed
- Deployed model (e.g., `gpt-4`, `gpt-35-turbo`)
- Endpoint URL, API key, and deployment name from Azure Portal

### Deploy a Model
1. Sign in to [Azure Portal](https://portal.azure.com/) → Create **Azure OpenAI** resource
2. Go to **Azure AI Studio** → **Deployments** → **Create new deployment**
3. Configure: deployment name, model (e.g., `gpt-35-turbo`), version
4. Copy **Endpoint** and **Key** from **Keys and Endpoint** in your resource

### Supported Models
| Model | Deployment Name | Use Case |
|---|---|---|
| GPT-4 | `gpt-4` | Complex reasoning, high quality |
| GPT-4 Turbo | `gpt-4-turbo` | Latest capabilities |
| GPT-3.5 Turbo | `gpt-35-turbo` | Fast, cost-effective |

### NuGet Packages
```powershell
Install-Package Microsoft.Extensions.AI
Install-Package Microsoft.Extensions.AI.OpenAI
Install-Package Azure.AI.OpenAI
```

### Program.cs
```csharp
using Syncfusion.Blazor;
using Syncfusion.Blazor.AI;
using Azure.AI.OpenAI;
using Microsoft.Extensions.AI;
using System.ClientModel;

builder.Services.AddSyncfusionBlazor();

string azureOpenAIKey        = "AZURE_OPENAI_KEY";
string azureOpenAIEndpoint   = "https://your-resource.openai.azure.com/";
string azureOpenAIDeployment = "your-deployment-name";

AzureOpenAIClient azureClient = new AzureOpenAIClient(
    new Uri(azureOpenAIEndpoint),
    new ApiKeyCredential(azureOpenAIKey)
);
IChatClient chatClient = azureClient.GetChatClient(azureOpenAIDeployment).AsIChatClient();
builder.Services.AddChatClient(chatClient);

builder.Services.AddSingleton<IChatInferenceService, SyncfusionAIService>();
```

### Reading from appsettings.json (recommended)
```json
{
  "AzureOpenAI": {
    "Key":            "your-azure-key",
    "Endpoint":       "https://your-resource.openai.azure.com/",
    "DeploymentName": "your-deployment-name"
  }
}
```
```csharp
string azureOpenAIKey        = builder.Configuration["AzureOpenAI:Key"];
string azureOpenAIEndpoint   = builder.Configuration["AzureOpenAI:Endpoint"];
string azureOpenAIDeployment = builder.Configuration["AzureOpenAI:DeploymentName"];
```

### Managed Identity (Production Recommended)
```csharp
using Azure.Identity;

var credential = new DefaultAzureCredential();
AzureOpenAIClient azureClient = new AzureOpenAIClient(
    new Uri(azureOpenAIEndpoint),
    credential
);
```

### Security & Compliance Features
- Virtual Network support and private endpoints
- Azure AD / RBAC integration
- HIPAA and SOC 2 compliance
- Data residency options
- Azure Monitor integration (requests/minute, token usage, latency, error rates)

### Troubleshooting
| Error | Fix |
|---|---|
| ResourceNotFound (404) | Verify endpoint URL and resource name |
| InvalidAuthenticationTokenTenant (401) | Verify API key and region |
| Model not found (404) | Check deployment name and active status |
| Timeout | Check Azure OpenAI resource capacity |

---

## Ollama (Local / Self-hosted)

Runs open-source LLMs locally — no API costs, no cloud connectivity required.

### Install Ollama

**Windows:** Download installer from [ollama.com](https://ollama.com)  
**macOS:** Download `.dmg`, drag to Applications  
**Linux:** `curl https://ollama.ai/install.sh | sh`

Verify installation: `ollama --version`

### Pull a Model
```bash
ollama pull mistral       # Fast, good quality
ollama pull llama2        # General purpose
ollama pull orca-mini     # Lightweight, very fast
ollama pull neural-chat   # Conversational
```

### Start Ollama Service
```bash
ollama serve              # macOS/Linux (Windows starts automatically)
```
Access at `http://localhost:11434`

### NuGet Packages
```powershell
Install-Package Microsoft.Extensions.AI
Install-Package OllamaSharp
```

### Program.cs
```csharp
using Syncfusion.Blazor;
using Syncfusion.Blazor.AI;
using Microsoft.Extensions.AI;
using OllamaSharp;

builder.Services.AddSyncfusionBlazor();

string modelName    = "mistral"; // any model you pulled
string ollamaUrl    = "http://localhost:11434";

IChatClient chatClient = new OllamaApiClient(ollamaUrl, modelName);
builder.Services.AddChatClient(chatClient);

builder.Services.AddSingleton<IChatInferenceService, SyncfusionAIService>();
```

### Docker Compose Deployment
```yaml
version: '3.8'
services:
  ollama:
    image: ollama/ollama:latest
    ports:
      - "11434:11434"
    volumes:
      - ollama_data:/root/.ollama
    environment:
      - OLLAMA_HOST=0.0.0.0:11434
volumes:
  ollama_data:
```

Pull models inside container:
```bash
docker exec -it <container-id> ollama pull mistral
```

### Model Recommendations
| Use Case | Model | Notes |
|---|---|---|
| General editing | `mistral` | Fast, good quality |
| Content creation | `llama2` | Balanced performance |
| Lightweight / fast | `orca-mini` | Very fast, limited capability |
| Best quality | `dolphin-mixtral` | Excellent, resource-heavy |

### GPU Acceleration
- **NVIDIA:** Install CUDA toolkit
- **AMD:** Install ROCm
- **Intel:** Install Intel oneAPI

### Troubleshooting
| Problem | Fix |
|---|---|
| Unable to connect | Run `curl http://localhost:11434/api/tags` to verify; check `ollama serve` is running |
| Model not found | Run `ollama list`; pull with `ollama pull <model>` |
| Out of memory | Use smaller models (`orca-mini`, `mistral`); restart Ollama |
| Slow responses | Enable GPU acceleration; use faster models |

---

## Custom IChatClient

Integrate any proprietary or internal AI service by implementing the `IChatClient` interface from `Microsoft.Extensions.AI`.

### Step 1: Implement IChatClient

```csharp
using Microsoft.Extensions.AI;

namespace YourApp.AI
{
    public class CustomAIBackend : IChatClient
    {
        private readonly string _endpoint;
        private readonly string _apiKey;
        private readonly HttpClient _httpClient;

        public CustomAIBackend(string endpoint, string apiKey)
        {
            _endpoint   = endpoint;
            _apiKey     = apiKey;
            _httpClient = new HttpClient();
        }

        public async ValueTask<ChatCompletion> CompleteAsync(
            IList<ChatMessage> chatMessages,
            ChatOptions? options = null,
            CancellationToken cancellationToken = default)
        {
            var requestBody = new
            {
                messages    = chatMessages.Select(m => new {
                    role    = m.Role.ToString().ToLower(),
                    content = m.Content?[0]?.Text ?? string.Empty
                }),
                max_tokens  = options?.MaxCompletionTokens ?? 500,
                temperature = options?.Temperature ?? 0.7f
            };

            var request = new HttpRequestMessage(HttpMethod.Post, _endpoint)
            {
                Content = new StringContent(
                    System.Text.Json.JsonSerializer.Serialize(requestBody),
                    System.Text.Encoding.UTF8,
                    "application/json"
                )
            };

            if (!string.IsNullOrEmpty(_apiKey))
                request.Headers.Add("Authorization", $"Bearer {_apiKey}");

            var response = await _httpClient.SendAsync(request, cancellationToken);
            response.EnsureSuccessStatusCode();

            var content = await response.Content.ReadAsStringAsync();
            var result  = System.Text.Json.JsonSerializer
                              .Deserialize<CustomResponse>(content);

            return new ChatCompletion(new ChatMessage(
                ChatRole.Assistant,
                result?.output ?? "No response"
            ));
        }

        public void Dispose() => _httpClient?.Dispose();

        private class CustomResponse { public string? output { get; set; } }
    }
}
```

### Step 2: Register in Program.cs

```csharp
using Syncfusion.Blazor;
using Syncfusion.Blazor.AI;
using YourApp.AI;

builder.Services.AddSyncfusionBlazor();

var customBackend = new CustomAIBackend(
    "https://your-ai-service.com/api/inference",
    "your-api-key"
);
builder.Services.AddChatClient(customBackend);

builder.Services.AddSingleton<IChatInferenceService, SyncfusionAIService>();
```

### With IConfiguration Support

```csharp
// appsettings.json
{
  "CustomAI": {
    "Endpoint": "https://your-ai-service.com/api/inference",
    "ApiKey":   "your-api-key"
  }
}

// In constructor
public CustomAIBackend(IConfiguration configuration)
{
    _endpoint = configuration["CustomAI:Endpoint"]
        ?? throw new InvalidOperationException("CustomAI:Endpoint not configured");
    _apiKey   = configuration["CustomAI:ApiKey"]
        ?? throw new InvalidOperationException("CustomAI:ApiKey not configured");
    _httpClient = new HttpClient();
}
```

### Streaming Responses

For streaming UX, read SSE (Server-Sent Events) chunks and call `UpdateAIResponseAsync` on `AssistViewSettings`:

```csharp
public async ValueTask<ChatCompletion> CompleteAsync(
    IList<ChatMessage> chatMessages,
    ChatOptions? options = null,
    CancellationToken cancellationToken = default)
{
    var request = new HttpRequestMessage(HttpMethod.Post, _endpoint);
    request.Content = new StringContent(
        System.Text.Json.JsonSerializer.Serialize(new { messages = chatMessages }),
        System.Text.Encoding.UTF8, "application/json"
    );
    request.Headers.Add("Accept", "text/event-stream");

    var response = await _httpClient.SendAsync(
        request, HttpCompletionOption.ResponseHeadersRead, cancellationToken);

    using var stream = await response.Content.ReadAsStreamAsync(cancellationToken);
    using var reader = new System.IO.StreamReader(stream);

    var fullContent = new System.Text.StringBuilder();
    while (!reader.EndOfStream)
    {
        var line = await reader.ReadLineAsync();
        if (line?.StartsWith("data: ") == true)
        {
            var chunk = System.Text.Json.JsonSerializer
                .Deserialize<StreamChunk>(line.Substring(6));
            if (chunk?.content != null) fullContent.Append(chunk.content);
        }
    }

    return new ChatCompletion(new ChatMessage(
        ChatRole.Assistant, fullContent.ToString()
    ));
}

private class StreamChunk { public string? content { get; set; } }
```

### Retry Logic

```csharp
public async ValueTask<ChatCompletion> CompleteAsync(
    IList<ChatMessage> chatMessages,
    ChatOptions? options = null,
    CancellationToken cancellationToken = default)
{
    int maxRetries = 3;
    for (int attempt = 1; attempt <= maxRetries; attempt++)
    {
        try
        {
            return await AttemptCompletionAsync(chatMessages, cancellationToken);
        }
        catch (HttpRequestException) when (attempt < maxRetries)
        {
            await Task.Delay(TimeSpan.FromSeconds(attempt * 2), cancellationToken);
        }
    }
    throw new InvalidOperationException("Failed after max retries");
}
```
