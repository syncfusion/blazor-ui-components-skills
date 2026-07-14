# Getting Started with Blazor Inline AI Assist

## Table of Contents
1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Create Blazor WebAssembly App](#create-blazor-webassembly-app)
4. [Create Blazor Web App](#create-blazor-web-app)
5. [Install Syncfusion Packages](#install-syncfusion-packages)
6. [Add Import Namespaces](#add-import-namespaces)
7. [Minimal Working Example](#minimal-working-example)
8. [Verify Your Setup](#verify-your-setup)

## Overview

The Inline AI Assist component works in both Blazor WebAssembly (WASM) and Blazor Web App projects. This guide covers setup for both project types using Visual Studio, Visual Studio Code, and .NET CLI.

## Prerequisites

- .NET SDK 8.0 or later installed
- Syncfusion Blazor packages available on NuGet
- Modern browser with JavaScript support (Chrome, Firefox, Safari, Edge)

## Create Blazor WebAssembly App

### Visual Studio

1. Open Visual Studio 2022 or later
2. Select **Create a new project**
3. Search for **Blazor WebAssembly App** template
4. Configure the project:
   - **Project name**: Your desired project name
   - **.NET version**: .NET 8.0 or later
   - **Authentication type**: Select based on your needs (None for simple projects)
5. Click **Create**
6. Verify project structure includes:
   - `Program.cs` - Application entry point
   - `wwwroot/` - Static assets folder
   - `Pages/` - Blazor components

### Visual Studio Code

1. Open terminal and run:
```bash
dotnet new blazorwasm -o BlazorApp
cd BlazorApp
```

2. Optional: Use Syncfusion Blazor Extension
   - Install Syncfusion Blazor Extension from VS Code marketplace
   - Use extension to create projects with pre-configured Syncfusion packages

3. Open project in VS Code:
```bash
code .
```

### .NET CLI

Run these commands:
```bash
dotnet new blazorwasm -o BlazorWasmApp
cd BlazorWasmApp
dotnet build
dotnet run
```

The application starts on `http://localhost:5000` by default.

## Create Blazor Web App

### Visual Studio

1. Open Visual Studio 2022 or later
2. Select **Create a new project**
3. Search for **Blazor Web App** template
4. Configure the project:
   - **Project name**: Your desired project name
   - **.NET version**: .NET 8.0 or later
   - **Authentication type**: Select based on your needs
   - **Interactive render mode**: Choose `Auto` for Server and WebAssembly
5. Click **Create**
6. Project structure includes both server and client components

### Visual Studio Code

1. Create Blazor Web App with `Auto` interactive render mode:
```bash
dotnet new blazor -o BlazorWebApp -int Auto
cd BlazorWebApp
cd BlazorWebApp.Client
```

2. Navigate to client project:
```bash
cd BlazorWebApp.Client
```

### .NET CLI

Create and run Blazor Web App:
```bash
dotnet new blazor -o BlazorWebApp -int Auto
cd BlazorWebApp
cd BlazorWebApp.Client
dotnet build
cd ..
dotnet run
```

The application starts and displays both server and client components.

## Install Syncfusion Packages

### Using NuGet Package Manager (Visual Studio)

1. Open **Tools → NuGet Package Manager → Manage NuGet Packages for Solution**
2. Search for **Syncfusion.Blazor.InteractiveChat**
3. Select latest version and click **Install**
4. Repeat for **Syncfusion.Blazor.Themes** package
5. Click **OK** to confirm installation

### Using Package Manager Console

Run these commands in Package Manager Console:
```powershell
Install-Package Syncfusion.Blazor.InteractiveChat
Install-Package Syncfusion.Blazor.Themes
```

### Using .NET CLI

```bash
dotnet add package Syncfusion.Blazor.InteractiveChat
dotnet add package Syncfusion.Blazor.Themes
```

### Verify Installation

Check `csproj` file for:
```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Blazor.InteractiveChat" Version="x.x.x" />
  <PackageReference Include="Syncfusion.Blazor.Themes" Version="x.x.x" />
</ItemGroup>
```

## Add Import Namespaces

### Modify App.razor

Add theme import at the top of `App.razor`:
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Theme
```

Add stylesheet in `<head>` section or create a dedicated theme file.

### Modify Program.cs

Register Syncfusion Blazor services:

**For WebAssembly App:**
```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient 
{ 
    BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) 
});

// Register Syncfusion Blazor services
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

**For Web App (Server Component):**
```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container
builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents();

// Register Syncfusion Blazor services
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();

if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Error", createScopeForErrors: true);
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();
app.UseAntiforgery();

app.MapRazorComponents<App>()
    .AddInteractiveServerRenderMode();

app.Run();
```

## Add Stylesheet

### For WebAssembly App

Add to `wwwroot/index.html` in the `<head>` section:
```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

Choose appropriate theme:
- `bootstrap5.css` - Default Bootstrap theme
- `material3.css` - Material Design 3
- `tailwind.css` - Tailwind CSS theme
- Other available themes in Syncfusion packages

### For Web App

Add to `Components/App.razor` in the `<head>` section:
```razor
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

## Minimal Working Example

Create or modify a Blazor component (e.g., `Pages/Index.razor`):

```razor
@page "/"
@using Syncfusion.Blazor.InteractiveChat
@using Syncfusion.Blazor.Buttons

<PageTitle>Inline AI Assist Demo</PageTitle>

<div class="container" style="padding: 20px;">
    <h2>Blazor Inline AI Assist - Getting Started</h2>
    
    <div style="height: 400px; width: 700px; border: 1px solid #ddd; padding: 10px; margin-bottom: 20px;">
        <SfButton id="aiButton" 
                  IsPrimary="true" 
                  Style="margin-bottom: 10px;"
                  @onclick="OnButtonClick">
            Ask AI Assistant
        </SfButton>
        
        <div id="editableText" 
             contenteditable="true"
             style="border: 1px solid #ccc; padding: 15px; min-height: 300px; border-radius: 4px;">
            <p>Enter your text here for AI processing. You can edit this content directly.</p>
            <p>The Inline AI Assist will help you generate, summarize, or modify content using AI.</p>
        </div>

        <SfInlineAIAssist @ref="inlineAssist"
                         RelateTo="#aiButton"
                         PopupWidth="500px"
                         PopupHeight="auto"
                         PromptRequested="OnPromptRequested">
            <ResponseActions ItemSelect="OnResponseItemSelect"></ResponseActions>
        </SfInlineAIAssist>
    </div>

    <p style="color: #666; font-size: 12px;">
        <strong>Note:</strong> To see AI responses, configure an AI provider (Azure OpenAI, OpenAI, etc.) 
        in the PromptRequested event. See the AI Integration guide for details.
    </p>
</div>

@code {
    private SfInlineAIAssist inlineAssist = new();

    private async Task OnButtonClick()
    {
        await inlineAssist.ShowPopupAsync();
    }

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        try
        {
            // For now, return a placeholder response
            // In production, integrate with your AI provider
            string response = $"Processing prompt: \"{args.Prompt}\"...\n\n" +
                            "To enable AI responses, configure an AI provider:\n" +
                            "1. Choose provider (Azure OpenAI, OpenAI, Ollama, Gemini)\n" +
                            "2. Authenticate with API key\n" +
                            "3. Call provider API in this event\n" +
                            "4. Update response using UpdateResponseAsync()";
            
            await inlineAssist.UpdateResponseAsync(response, true);
        }
        catch (Exception ex)
        {
            await inlineAssist.UpdateResponseAsync($"Error: {ex.Message}", true);
        }
    }

    private async Task OnResponseItemSelect(ResponseItemSelectEventArgs args)
    {
        if (args.Item.Label == "Accept")
        {
            await inlineAssist.HidePopupAsync();
        }
        else if (args.Item.Label == "Discard")
        {
            await inlineAssist.HidePopupAsync();
        }
    }
}
```

## Verify Your Setup

### Compile Project

```bash
dotnet build
```

Verify no compilation errors related to Syncfusion packages.

### Run Application

```bash
dotnet run
```

Navigate to `http://localhost:5000` (or port shown in terminal).

### Test Basic Functionality

1. **Verify Component Renders**: Should see "Ask AI Assistant" button
2. **Click Button**: Button click should show popup with text input area
3. **Enter Text**: You can type prompts in the popup
4. **Check Styling**: Button should have primary styling from theme

### Troubleshooting

**Issue: "SfInlineAIAssist is not recognized"**
- Solution: Ensure `@using Syncfusion.Blazor.InteractiveChat` is added to component

**Issue: Styling not applied**
- Solution: Verify CSS stylesheet link in `index.html` or `App.razor`
- Verify `AddSyncfusionBlazor()` called in `Program.cs`

**Issue: "Package Syncfusion.Blazor.InteractiveChat not found"**
- Solution: Check NuGet.org has the package
- Run `dotnet nuget locals all --clear` to clear cache
- Retry package installation

**Issue: Project won't run**
- Solution: Verify .NET SDK version is 8.0 or later: `dotnet --version`
- Check all dependencies installed: `dotnet restore`
- Clean and rebuild: `dotnet clean && dotnet build`

## Next Steps

- **Configure AI Provider**: Read `ai-integration.md` to connect Azure OpenAI, OpenAI, Ollama, or Gemini
- **Add Commands**: See `commands-and-toolbar.md` for quick-action prompts
- **Handle Events**: Review `events-and-methods.md` for event handling patterns
- **Customize UI**: Check `customization-and-response.md` for appearance customization
- **Ensure Accessibility**: Read `accessibility-and-globalization.md` for WCAG compliance

## Resources

- [Syncfusion Blazor Documentation](https://blazor.syncfusion.com/documentation/getting-started)
- [Blazor Inline AI Assist API](https://blazor.syncfusion.com/documentation/inline-ai-assist/)
- [Syncfusion NuGet Packages](https://www.nuget.org/packages?q=syncfusion.blazor)
