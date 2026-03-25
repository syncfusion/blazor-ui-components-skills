# Getting Started with Syncfusion Blazor Rich Text Editor

## Table of Contents
- [NuGet Installation](#nuget-installation)
- [Service Registration](#service-registration)
- [Namespace Imports](#namespace-imports)
- [CSS and JS References](#css-and-js-references)
- [Adding the Component](#adding-the-component)
- [Retrieving Content](#retrieving-content)

---

## NuGet Installation

Install two packages — the RTE component and a theme:

**Visual Studio (Package Manager Console):**
```powershell
Install-Package Syncfusion.Blazor.RichTextEditor
Install-Package Syncfusion.Blazor.Themes
```

**.NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.RichTextEditor
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

> Use the same version for both packages to avoid compatibility issues.

---

## Service Registration

Register the Syncfusion Blazor service in `Program.cs` **before** `builder.Build()`:

```csharp
using Syncfusion.Blazor;

// Blazor WebAssembly
var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.Services.AddSyncfusionBlazor();
await builder.Build().RunAsync();

// Blazor Server
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddSyncfusionBlazor();
```

---

## Namespace Imports

Add to `_Imports.razor`:
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.RichTextEditor
```

---

## CSS and JS References

### Blazor WebAssembly (`wwwroot/index.html`)
```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
<body>
    ...
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
    <!-- Optional: standalone RTE script for better isolation -->
    <!-- <script src="_content/Syncfusion.Blazor.RichTextEditor/scripts/sf-richtexteditor.min.js"></script> -->
</body>
```

### Blazor Server / Web App (`App.razor` or `_Host.cshtml`)
```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

**Available themes:** `bootstrap5.css` | `material.css` | `fluent.css` | `tailwind.css` | `fabric.css` | `material-dark.css`

### Blazor Web App (.NET 8+) — Render Mode

For interactive components in a Blazor Web App, add a render mode directive:
```razor
@rendermode InteractiveServer
@* or InteractiveWebAssembly / InteractiveAuto *@
```

---

## Adding the Component

### Minimal usage
```razor
<SfRichTextEditor />
```

### With initial content and two-way binding
```razor
<SfRichTextEditor @bind-Value="@Content" />

@code {
    private string Content { get; set; } = "<p>Hello, <b>World!</b></p>";
}
```

### With toolbar configured
```razor
<SfRichTextEditor @bind-Value="@Content">
    <RichTextEditorToolbarSettings Items="@Tools" />
</SfRichTextEditor>

@code {
    private string Content { get; set; } = string.Empty;

    private List<ToolbarItemModel> Tools = new()
    {
        new() { Command = ToolbarCommand.Bold },
        new() { Command = ToolbarCommand.Italic },
        new() { Command = ToolbarCommand.Underline },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.OrderedList },
        new() { Command = ToolbarCommand.UnorderedList },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Undo },
        new() { Command = ToolbarCommand.Redo }
    };
}
```

---

## Retrieving Content

### Get HTML value (property)
The `Value` property always contains the current HTML:
```razor
<SfRichTextEditor @bind-Value="@HtmlContent" />
<p>Current HTML: @HtmlContent</p>

@code {
    private string HtmlContent { get; set; } = string.Empty;
}
```

### Get plain text (method)
```razor
<SfRichTextEditor @ref="RteObj" />
<button @onclick="GetPlainText">Get Text</button>

@code {
    private SfRichTextEditor RteObj;

    private async Task GetPlainText()
    {
        string plainText = await RteObj.GetTextAsync();
        Console.WriteLine(plainText);
    }
}
```

### Get character count (method)
```razor
<SfRichTextEditor @ref="RteObj" />
<button @onclick="GetCount">Get Count</button>

@code {
    private SfRichTextEditor RteObj;

    private async Task GetCount()
    {
        double count = await RteObj.GetCharCountAsync();
        Console.WriteLine($"Characters: {count}");
    }
}
```

### Show character count in UI
```razor
<SfRichTextEditor @bind-Value="@Content" ShowCharCount="true" MaxLength="1000" />

@code {
    private string Content { get; set; } = string.Empty;
}
```

---

## Common Setup Issues

| Problem | Fix |
|---|---|
| Styles not applied | Verify theme CSS is in `<head>`, not `<body>` |
| Component not rendering | Confirm `AddSyncfusionBlazor()` is called in `Program.cs` |
| Scripts not loading | Check `syncfusion-blazor.min.js` is at end of `<body>` |
| License warning in console | Register license key before `builder.Build()` |
| Not interactive in .NET 8 Web App | Add `@rendermode InteractiveServer` (or WASM/Auto) |
