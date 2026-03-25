# Syncfusion Blazor Smart TextArea - Getting Started

## Overview
This guide explains how to create and configure a Blazor Web App with the Syncfusion Blazor Smart TextArea component, including AI service setup.

## Prerequisites
- [System requirements for Blazor components](https://blazor.syncfusion.com/documentation/system-requirements)
- OpenAI or Azure OpenAI Account
  - OpenAI API Key
  - Azure OpenAI Account Setup

> **Note:** Syncfusion Blazor Smart Components are compatible with both OpenAI and Azure OpenAI, and fully support Server Interactivity mode apps.

## Create a New Blazor Web App

You can create a Blazor Web App using:
- **Visual Studio 2022** via Microsoft Templates
- **Syncfusion Blazor Extension** - Template Studio

## Install NuGet Packages

### Using NuGet Package Manager
In Visual Studio: **Tools → NuGet Package Manager → Manage NuGet Packages for Solution**

Search and install:
- `Syncfusion.Blazor.SmartComponents`
- `Syncfusion.Blazor.Themes`

### Using Package Manager Console
```
Install-Package Syncfusion.Blazor.SmartComponents -Version 33.1.44
Install-Package Syncfusion.Blazor.Themes -Version 33.1.44
```

> For available NuGet packages list, refer to Syncfusion NuGet packages

## Register Syncfusion Blazor Service

### For Server-Side Blazor

Open `~/_Imports.razor` (located in the Components folder) and add:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.SmartComponents
```

In `~/Program.cs`:

```csharp
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Web;
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();
// ... rest of configuration
```

## Configure AI Service

### OpenAI Configuration

#### Install Required NuGet Packages
```
Install-Package Microsoft.Extensions.AI
Install-Package Microsoft.Extensions.AI.OpenAI
```

#### Update Program.cs

```csharp
using Syncfusion.Blazor.SmartComponents;
using Syncfusion.Blazor.AI;
using Microsoft.Extensions.AI;
using OpenAI;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();
builder.Services.AddSyncfusionBlazor();

string openAIApiKey = "API-KEY";
string openAIModel = "OPENAI_MODEL";  // e.g., gpt-3.5-turbo, gpt-4

OpenAIClient openAIClient = new OpenAIClient(openAIApiKey);
IChatClient openAIChatClient = openAIClient.GetChatClient(openAIModel).AsIChatClient();
builder.Services.AddChatClient(openAIChatClient);

builder.Services.AddSyncfusionSmartComponents()
    .InjectOpenAIInference();

var app = builder.Build();
// ... rest of configuration
```

### Azure OpenAI Configuration

#### Prerequisites
First, [deploy an Azure OpenAI Service resource and model](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/create-resource)

#### Install Required NuGet Packages
```
Install-Package Microsoft.Extensions.AI
Install-Package Microsoft.Extensions.AI.OpenAI
Install-Package Azure.AI.OpenAI
```

#### Update Program.cs

```csharp
using Syncfusion.Blazor.SmartComponents;
using Syncfusion.Blazor.AI;
using Azure.AI.OpenAI;
using Microsoft.Extensions.AI;
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

> **Note:** From version 28.2.33 to 30.2.6, the Azure.AI.OpenAI package was removed from SmartComponents dependency. Install [Azure.AI.OpenAI](https://www.nuget.org/packages/Azure.AI.OpenAI) package separately if needed.

### Ollama Configuration (Self-Hosted Models)

#### Setup Steps
1. **Install Ollama** - Download from Ollama's official website
2. **Install a Model** - Browse and install from Ollama Library (e.g., `llama2:13b`, `mistral:7b`)
3. **Configure Application** - Provide the endpoint URL and model name

#### Install Required NuGet Packages
```
Install-Package Microsoft.Extensions.AI
Install-Package OllamaSharp
```

#### Update Program.cs

```csharp
using Syncfusion.Blazor.SmartComponents;
using Syncfusion.Blazor.AI;
using Microsoft.Extensions.AI;
using OllamaSharp;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();
builder.Services.AddSyncfusionBlazor();

string ModelName = "MODEL_NAME";  // e.g., llama2:13b
IChatClient chatClient = new OllamaApiClient("http://localhost:11434", ModelName);
builder.Services.AddChatClient(chatClient);

builder.Services.AddSyncfusionSmartComponents()
    .InjectOpenAIInference();

var app = builder.Build();
// ... rest of configuration
```

## Add Stylesheet and Script Resources

Include the theme stylesheet and script in the `~/Components/App.razor` file:

```razor
<head>
    <!-- ... other head content ... -->
    <link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />
</head>

<body>
    <!-- ... body content ... -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</body>
```

> For more theme options and methods of referencing themes, see [Blazor Themes](../../syncfusion-blazor-themes/references/theme-appearance.md) documentation.
> 
> For different approaches to add script references, see [Adding Script Reference](../../syncfusion-blazor-common/references/add-script-reference-and-cdn.md) documentation.

## Add Smart TextArea Component

Create or update the `~/Pages/Index.razor` file:

```razor
<SfSmartTextArea UserRole="@userRole" 
                UserPhrases="@userPhrases" 
                Placeholder="Enter your queries here" 
                @bind-Value="prompt" 
                Width="75%" 
                RowCount="5">
</SfSmartTextArea>

@code {
    private string? prompt;
    
    public string userRole = "Maintainer of an open-source project replying to GitHub issues";
    
    public string[] userPhrases = 
    [
        "Thank you for contacting us.",
        "To investigate, We will need a reproducible example as a public Git repository.",
        "Could you please post a screenshot of NEED_INFO?",
        "This sounds like a usage question. This issue tracker is intended for bugs and feature proposals.",
        "We do not accept ZIP files as reproducible examples.",
        "Bug report: File not found error occurred in NEED_INFO"
    ];
}
```

### Component Attributes

- **UserRole** (Required): Defines the context of autocompletion based on the role of the person typing
- **UserPhrases** (Optional): Provides predefined expressions that align with the user/application's tone and frequently used content
- **Placeholder**: Text displayed when the input is empty
- **@bind-Value**: Two-way binding for the textarea value
- **Width & RowCount**: Styling properties


