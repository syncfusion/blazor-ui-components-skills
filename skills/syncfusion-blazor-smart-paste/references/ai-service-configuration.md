# Syncfusion Blazor Smart Paste Button - Configuration Guide

## Overview

This guide covers AI service configuration and resource setup for the Smart Paste Button. Ensure you've completed the setup steps in **getting-started.md** before proceeding.

## AI Service Configuration

The Smart Paste Button requires an AI service to analyze and map copied text to form fields. Choose one of the following configuration options:

### Prerequisites

Install AI Service NuGet packages based on your chosen AI provider:

**Common AI Service Packages:**
```powershell
Install-Package Microsoft.Extensions.AI
Install-Package Microsoft.Extensions.AI.OpenAI
```

**For Azure OpenAI:**
```powershell
Install-Package Azure.AI.OpenAI
```

**For Ollama (Self-hosted):**
```powershell
Install-Package OllamaSharp
```

## Configuration Options

### OpenAI Configuration

**Prerequisites:**
- Obtain an API key from [OpenAI](https://help.openai.com/en/articles/4936850-where-do-i-find-my-openai-api-key)
- Choose a model (e.g., `gpt-3.5-turbo`, `gpt-4`)
- Store the API key securely

**Update Program.cs:**

```csharp
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Web;
using Syncfusion.Blazor;
using Syncfusion.Blazor.SmartComponents;
using Syncfusion.Blazor.AI;
using Microsoft.Extensions.AI;
using OpenAI;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();
builder.Services.AddSyncfusionBlazor();

string openAIApiKey = "API-KEY";
string openAIModel = "OPENAI_MODEL";

OpenAIClient openAIClient = new OpenAIClient(openAIApiKey);
IChatClient openAIChatClient = openAIClient.GetChatClient(openAIModel).AsIChatClient();
builder.Services.AddChatClient(openAIChatClient);

builder.Services.AddSyncfusionSmartComponents()
    .InjectOpenAIInference();

var app = builder.Build();
// ... rest of configuration
```

### Azure OpenAI Configuration

**Prerequisites:**
- Deploy a resource and model as described in [Azure OpenAI documentation](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/create-resource)
- Obtain the API key, endpoint, and model name from your Azure portal

**Update Program.cs:**

```csharp
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Web;
using Syncfusion.Blazor;
using Syncfusion.Blazor.SmartComponents;
using Syncfusion.Blazor.AI;
using Microsoft.Extensions.AI;
using Azure.AI.OpenAI;
using System.ClientModel;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();
builder.Services.AddSyncfusionBlazor();

string azureOpenAIKey = "AZURE_OPENAI_KEY";
string azureOpenAIEndpoint = "AZURE_OPENAI_ENDPOINT";
string azureOpenAIModel = "AZURE_OPENAI_MODEL";

AzureOpenAIClient azureOpenAIClient = new AzureOpenAIClient(
    new Uri(azureOpenAIEndpoint),
    new ApiKeyCredential(azureOpenAIKey)
);
IChatClient azureOpenAIChatClient = azureOpenAIClient.GetChatClient(azureOpenAIModel).AsIChatClient();
builder.Services.AddChatClient(azureOpenAIChatClient);

builder.Services.AddSyncfusionSmartComponents()
    .InjectOpenAIInference();

var app = builder.Build();
// ... rest of configuration
```

> **NOTE:** For versions 28.2.33 to 30.2.6, the Azure.AI.OpenAI package is not included in SmartComponents dependency. Install the [Azure.AI.OpenAI](https://www.nuget.org/packages/Azure.AI.OpenAI) package separately. Refer to [Syncfusion version history](https://blazor.syncfusion.com/documentation/release-notes) for the latest dependency information.

### Ollama Configuration (Self-hosted)

**Prerequisites:**
- Download and install [Ollama](https://ollama.com/)
- Install a model from [Ollama Library](https://ollama.com/library) (e.g., `llama2:13b`, `mistral:7b`)
- Configure the endpoint URL (e.g., `http://localhost:11434`) and model name

**Update Program.cs:**

```csharp
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Web;
using Syncfusion.Blazor;
using Syncfusion.Blazor.SmartComponents;
using Syncfusion.Blazor.AI;
using Microsoft.Extensions.AI;
using OllamaSharp;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();
builder.Services.AddSyncfusionBlazor();

string ModelName = "MODEL_NAME";

IChatClient chatClient = new OllamaApiClient("http://localhost:11434", ModelName);
builder.Services.AddChatClient(chatClient);

builder.Services.AddSyncfusionSmartComponents()
    .InjectOpenAIInference();

var app = builder.Build();
// ... rest of configuration
```

## Add Stylesheet and Script Resources

Include the theme stylesheet and script from NuGet via Static Web Assets. Add the stylesheet reference in the `<head>` section and the script reference at the end of the `<body>` in `~/Components/App.razor`:

```html
<head>
    ....
    <link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />
</head>

<body>
    ....
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</body>
```

### Theme Options

You can choose from different themes:
- `tailwind.css`
- `bootstrap5.css`
- `fluent.css`
- `material.css`
- And others

### Alternative Methods for Adding References

Refer to [Blazor Themes](https://blazor.syncfusion.com/documentation/appearance/themes) documentation for:
- **Static Web Assets** (recommended)
- **CDN** - Content Delivery Network
- **CRG** - Custom Resource Generator

## Next Steps

Proceed to **implementation-and-customization.md** to implement and customize the Smart Paste Button component.
