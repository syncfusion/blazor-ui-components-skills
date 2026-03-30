# Getting Started with Syncfusion Blazor Smart Rich Text Editor

## Table of Contents
- [Create a New Blazor App](#create-a-new-blazor-app)
- [NuGet Installation](#nuget-installation)
- [Configure AI Service](#configure-ai-service)
- [Service Registration](#service-registration)
- [Namespace Imports](#namespace-imports)
- [CSS and JS References](#css-and-js-references)
- [Adding the Component](#adding-the-component)
- [Retrieving Content](#retrieving-content)
- [Blazor Web App Setup](#blazor-web-app-setup)
- [Common Setup Issues](#common-setup-issues)

---

## Create a New Blazor App

### Visual Studio
Use [Microsoft Templates](https://learn.microsoft.com/en-us/aspnet/core/blazor/tooling) or the [Syncfusion Blazor Extension](https://blazor.syncfusion.com/documentation/visual-studio-integration/template-studio) to create a **Blazor Server App**.

### Visual Studio Code
Create a **Blazor Server App** via terminal (<kbd>Ctrl</kbd>+<kbd>`</kbd>):
```bash
dotnet new blazorserver -o BlazorApp
cd BlazorApp
```

> For **Blazor Web App** (.NET 8+), ensure you select **Server Interactivity** when creating the project.

---

## NuGet Installation

Install the Smart Rich Text Editor package and a theme:

**Visual Studio (Package Manager Console):**
```powershell
Install-Package Syncfusion.Blazor.SmartRichTextEditor
Install-Package Syncfusion.Blazor.Themes
```

**.NET CLI (Visual Studio Code):**
```bash
dotnet add package Syncfusion.Blazor.SmartRichTextEditor
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

> Use the same version for both packages to avoid compatibility issues.

---

## Configure AI Service

`SfSmartRichTextEditor` requires an AI backend registered in `Program.cs`. Three backends are supported out of the box: **OpenAI**, **Azure OpenAI**, and **Ollama**. A **custom** `IChatClient` is also supported.

### OpenAI

Install additional NuGet packages:
```powershell
Install-Package Microsoft.Extensions.AI
Install-Package Microsoft.Extensions.AI.OpenAI
```

Configure in `Program.cs`:
```csharp
using Syncfusion.Blazor;
using Syncfusion.Blazor.AI;
using Microsoft.Extensions.AI;
using OpenAI;

builder.Services.AddSyncfusionBlazor();

string openAIApiKey = "YOUR_OPENAI_API_KEY";
string openAIModel = "gpt-4"; // or gpt-3.5-turbo, gpt-4-turbo

OpenAIClient openAIClient = new OpenAIClient(openAIApiKey);
IChatClient chatClient = openAIClient.GetChatClient(openAIModel).AsIChatClient();
builder.Services.AddChatClient(chatClient);

builder.Services.AddSingleton<IChatInferenceService, SyncfusionAIService>();
```

> **Security:** Store your API key in environment variables or User Secrets — never hardcode it in source files.

---

### Azure OpenAI

Install additional NuGet packages:
```powershell
Install-Package Microsoft.Extensions.AI
Install-Package Microsoft.Extensions.AI.OpenAI
Install-Package Azure.AI.OpenAI
```

Configure in `Program.cs`:
```csharp
using Syncfusion.Blazor;
using Syncfusion.Blazor.AI;
using Azure.AI.OpenAI;
using Microsoft.Extensions.AI;
using System.ClientModel;

builder.Services.AddSyncfusionBlazor();

string azureOpenAIKey = "AZURE_OPENAI_KEY";
string azureOpenAIEndpoint = "https://your-resource.openai.azure.com/";
string azureOpenAIModel = "your-deployment-name";

AzureOpenAIClient azureOpenAIClient = new AzureOpenAIClient(
    new Uri(azureOpenAIEndpoint),
    new ApiKeyCredential(azureOpenAIKey)
);
IChatClient chatClient = azureOpenAIClient.GetChatClient(azureOpenAIModel).AsIChatClient();
builder.Services.AddChatClient(chatClient);

builder.Services.AddSingleton<IChatInferenceService, SyncfusionAIService>();
```

Reading credentials from `appsettings.json` (recommended):
```json
{
  "AzureOpenAI": {
    "Key": "your-azure-key",
    "Endpoint": "https://your-resource.openai.azure.com/",
    "DeploymentName": "your-deployment-name"
  }
}
```
```csharp
string azureOpenAIKey      = builder.Configuration["AzureOpenAI:Key"];
string azureOpenAIEndpoint = builder.Configuration["AzureOpenAI:Endpoint"];
string azureOpenAIModel    = builder.Configuration["AzureOpenAI:DeploymentName"];
```

---

### Ollama (Local / Self-hosted)

Install additional NuGet packages:
```powershell
Install-Package Microsoft.Extensions.AI
Install-Package OllamaSharp
```

Configure in `Program.cs`:
```csharp
using Syncfusion.Blazor;
using Syncfusion.Blazor.AI;
using Microsoft.Extensions.AI;
using OllamaSharp;

builder.Services.AddSyncfusionBlazor();

string modelName = "mistral"; // any model pulled via `ollama pull <model>`
IChatClient chatClient = new OllamaApiClient("http://localhost:11434", modelName);
builder.Services.AddChatClient(chatClient);

builder.Services.AddSingleton<IChatInferenceService, SyncfusionAIService>();
```

> Ollama must be running locally (`ollama serve`) and the model pulled before starting the app.

---

## Service Registration

Register the Syncfusion Blazor service in `Program.cs` **before** `builder.Build()`:

```csharp
using Syncfusion.Blazor;

// Blazor Server App
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();
builder.Services.AddSyncfusionBlazor();
// ... AI service registration (see above) ...
var app = builder.Build();
```

---

## Namespace Imports

Add to `_Imports.razor`:
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.SmartRichTextEditor
```

> Do **not** import `Syncfusion.Blazor.RichTextEditor` unless you also need `SfRichTextEditor` directly. `SfSmartRichTextEditor` is in the `SmartRichTextEditor` namespace.

---

## CSS and JS References

### Blazor Server App (`App.razor` or `_Host.cshtml`)

For **.NET 8, .NET 9, .NET 10** — `~/Components/App.razor`:
```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />
</head>
<body>
    ...
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"
            type="text/javascript"></script>
</body>
```

For **.NET 6** — `~/Pages/_Layout.cshtml`:
```html
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

**Available themes:** `tailwind.css` | `bootstrap5.css` | `material.css` | `fluent.css` | `fabric.css` | `material-dark.css`

---

## Adding the Component

### Minimal usage
```razor
@using Syncfusion.Blazor.SmartRichTextEditor

<SfSmartRichTextEditor />
```

### With initial content, two-way binding, and AI assistant
```razor
@rendermode InteractiveServer
@using Syncfusion.Blazor.SmartRichTextEditor

<SfSmartRichTextEditor @bind-Value="@Content">
    <h2>Welcome to Smart Rich Text Editor</h2>
    <p>Select text and use the Smart Action toolbar, or press
       <kbd>Alt</kbd>+<kbd>Enter</kbd> to open the AI Query dialog.</p>
    <AssistViewSettings Placeholder="Ask AI to rewrite or generate content." />
</SfSmartRichTextEditor>

@code {
    private string Content { get; set; } = string.Empty;
}
```

### With toolbar configured and AI assistant
```razor
@rendermode InteractiveServer
@using Syncfusion.Blazor.SmartRichTextEditor
@using Syncfusion.Blazor.RichTextEditor

<SfSmartRichTextEditor @bind-Value="@Content">
    <RichTextEditorToolbarSettings Items="@Tools" />
    <AssistViewSettings Placeholder="Ask AI for help." />
</SfSmartRichTextEditor>

@code {
    private string Content { get; set; } = string.Empty;

    private List<ToolbarItemModel> Tools = new()
    {
        new() { Command = ToolbarCommand.Bold },
        new() { Command = ToolbarCommand.Italic },
        new() { Command = ToolbarCommand.Underline },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Formats },
        new() { Command = ToolbarCommand.Alignments },
        new() { Command = ToolbarCommand.OrderedList },
        new() { Command = ToolbarCommand.UnorderedList },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.CreateLink },
        new() { Command = ToolbarCommand.Image },
        new() { Command = ToolbarCommand.CreateTable },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Undo },
        new() { Command = ToolbarCommand.Redo }
    };
}
```

### With custom AI commands and suggestions
```razor
@rendermode InteractiveServer
@using Syncfusion.Blazor.SmartRichTextEditor

<SfSmartRichTextEditor @bind-Value="@Content">
    <AssistViewSettings Commands="@MyCommands"
                        Suggestions="@QuickSuggestions"
                        Placeholder="How can I help you improve this content?" />
</SfSmartRichTextEditor>

@code {
    private string Content { get; set; } = "<p>Start writing here...</p>";

    private List<AICommands> MyCommands = new()
    {
        new AICommands { Text = "Summarize",   Prompt = "Summarize this content concisely" },
        new AICommands { Text = "Expand",      Prompt = "Add more details and examples" },
        new AICommands { Text = "Fix Grammar", Prompt = "Fix grammar and spelling errors" },
        new AICommands
        {
            Text = "Change Tone",
            Items = new List<AICommands>
            {
                new AICommands { Text = "Professional", Prompt = "Rewrite in a professional tone" },
                new AICommands { Text = "Casual",       Prompt = "Rewrite in a casual, friendly tone" },
                new AICommands { Text = "Formal",       Prompt = "Rewrite in a formal tone" }
            }
        }
    };

    private List<string> QuickSuggestions = new()
    {
        "Make it shorter", "Improve clarity", "Fix grammar", "More formal", "Simplify"
    };
}
```

---

## Retrieving Content

### Get HTML value (property)
The `Value` property always contains the current HTML:
```razor
<SfSmartRichTextEditor @bind-Value="@HtmlContent" />
<p>Current HTML: @HtmlContent</p>

@code {
    private string HtmlContent { get; set; } = string.Empty;
}
```

### Get plain text (method)
```razor
<SfSmartRichTextEditor @ref="SmartRteObj" />
<button @onclick="GetPlainText">Get Text</button>

@code {
    private SfSmartRichTextEditor SmartRteObj;

    private async Task GetPlainText()
    {
        string plainText = await SmartRteObj.GetTextAsync();
        Console.WriteLine(plainText);
    }
}
```

### Get character count (method)
```razor
<SfSmartRichTextEditor @ref="SmartRteObj" />
<button @onclick="GetCount">Get Count</button>

@code {
    private SfSmartRichTextEditor SmartRteObj;

    private async Task GetCount()
    {
        double count = await SmartRteObj.GetCharCountAsync();
        Console.WriteLine($"Characters: {count}");
    }
}
```

### Show character count in UI
```razor
<SfSmartRichTextEditor @bind-Value="@Content" ShowCharCount="true" MaxLength="1000">
    <AssistViewSettings Placeholder="Ask AI for help." />
</SfSmartRichTextEditor>

@code {
    private string Content { get; set; } = string.Empty;
}
```

---

## Blazor Web App Setup

For **.NET 8+ Blazor Web App** projects with `InteractiveServer` render mode:

**`Program.cs`:**
```csharp
using Syncfusion.Blazor;
using Syncfusion.Blazor.AI;
using Microsoft.Extensions.AI;
using OpenAI;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents();

builder.Services.AddSyncfusionBlazor();

string openAIApiKey = "YOUR_OPENAI_API_KEY";
string openAIModel  = "gpt-4";
OpenAIClient openAIClient = new OpenAIClient(openAIApiKey);
IChatClient chatClient = openAIClient.GetChatClient(openAIModel).AsIChatClient();
builder.Services.AddChatClient(chatClient);
builder.Services.AddSingleton<IChatInferenceService, SyncfusionAIService>();

var app = builder.Build();
```

**`~/Components/_Imports.razor`:**
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.SmartRichTextEditor
```

**`~/Components/App.razor`:**
```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />
</head>
<body>
    ...
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"
            type="text/javascript"></script>
</body>
```

**`~/Components/Pages/Home.razor`:**
```razor
@rendermode InteractiveServer
@using Syncfusion.Blazor.SmartRichTextEditor

<SfSmartRichTextEditor>
    <h2>Welcome to Smart Rich Text Editor</h2>
    <p>Use the Smart Action toolbar or press <kbd>Alt</kbd>+<kbd>Enter</kbd>
       to open the AI Query dialog.</p>
    <AssistViewSettings Placeholder="Start typing or use AI assistance..." />
</SfSmartRichTextEditor>
```

> The `@rendermode InteractiveServer` directive is **required** on each page (or globally in `App.razor`) for the component to be interactive in a Blazor Web App.

---

## Common Setup Issues

| Problem | Fix |
|---|---|
| Styles not applied | Verify theme CSS `<link>` is inside `<head>`, not `<body>` |
| Component not rendering | Confirm `AddSyncfusionBlazor()` is called in `Program.cs` |
| Scripts not loading | Check `syncfusion-blazor.min.js` is placed at end of `<body>` |
| AI not responding | Verify `AddChatClient(...)` and `AddSingleton<IChatInferenceService, SyncfusionAIService>()` are both registered |
| AI key errors (401) | Check API key is correct and not expired; use environment variables, not hardcoded strings |
| Not interactive in .NET 8+ Web App | Add `@rendermode InteractiveServer` to the page or globally in `App.razor` |
| `SfSmartRichTextEditor` not found | Add `@using Syncfusion.Blazor.SmartRichTextEditor` to `_Imports.razor` |
| Ollama not responding | Ensure Ollama is running (`ollama serve`) and model is pulled (`ollama pull mistral`) |
