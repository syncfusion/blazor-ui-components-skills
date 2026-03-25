# Getting Started with Blazor FileManager

## Table of Contents

- [Installation and Setup](#installation-and-setup)
- [NuGet Packages](#nuget-packages)
- [Service Registration](#service-registration)
- [Stylesheet and Scripts](#stylesheet-and-scripts)
- [Basic Component Usage](#basic-component-usage)
- [AJAX Configuration](#ajax-configuration)
- [File Provider Setup](#file-provider-setup)
- [Complete Example](#complete-example)

## Installation and Setup

The Syncfusion Blazor FileManager component supports multiple hosting models:

### For Blazor Web App (Interactive)

When using `WebAssembly` or `Auto` render mode, install NuGet packages in the **client project**.

When using `Server` render mode, install packages in the **server project**.

### For Blazor Server App

Install packages in the server project.

### For Blazor WebAssembly App

Install packages in the WebAssembly project.

### For MAUI Blazor App

Install packages in the MAUI project.

## NuGet Packages

**Required packages:**

```
Syncfusion.Blazor.FileManager
Syncfusion.Blazor.Themes
```

### Installation Methods

**Visual Studio Package Manager:**
```
Install-Package Syncfusion.Blazor.FileManager -Version {{ site.releaseversion }}
Install-Package Syncfusion.Blazor.Themes -Version {{ site.releaseversion }}
```

**.NET CLI:**
```
dotnet add package Syncfusion.Blazor.FileManager --version {{ site.releaseversion }}
dotnet add package Syncfusion.Blazor.Themes --version {{ site.releaseversion }}
dotnet restore
```

## Service Registration

### In Program.cs (Blazor Web App / Server)

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// Add services
builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents()
    .AddInteractiveWebAssemblyComponents();

// Add Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();

// Configure routing
app.UseRouting();
app.MapControllers(); // Required for file operations
app.MapRazorComponents<App>();
```

### In Program.cs (Blazor WebAssembly)

```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");

// Add Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

## Stylesheet and Scripts

### Add to App.razor (or _Layout.cshtml for Server)

In the `<head>` section, add the theme stylesheet:

```html
<head>
    <!-- Other head content -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
```

At the end of `<body>`, add the Syncfusion Blazor script:

```html
<body>
    <!-- Other body content -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</body>
```

### Available Themes

- bootstrap5.css
- bootstrap4.css
- fabric.css
- fluent.css
- fluent2.css
- highcontrast.css
- material.css
- material3.css
- tailwind.css

## Basic Component Usage

### Minimal Component with AJAX

From `getting-started-with-web-app.md` (lines 150-165):

```razor
@using Syncfusion.Blazor.FileManager

<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="https://ej2-aspcore-service.azurewebsites.net/api/FileManager/FileOperations"
                             UploadUrl="https://ej2-aspcore-service.azurewebsites.net/api/FileManager/Upload"
                             DownloadUrl="https://ej2-aspcore-service.azurewebsites.net/api/FileManager/Download"
                             GetImageUrl="https://ej2-aspcore-service.azurewebsites.net/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### With Render Mode (Blazor Web App)

```razor
@rendermode InteractiveAuto

@using Syncfusion.Blazor.FileManager

<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### Render Mode Options

| Interactivity Location | Render Mode | Syntax |
|--|--|--|
| Per page/component | Auto | `@rendermode InteractiveAuto` |
| Per page/component | WebAssembly | `@rendermode InteractiveWebAssembly` |
| Per page/component | None | (no render mode needed) |
| Global | Auto/WebAssembly | Configured in App.razor |

## AJAX Configuration

### FileManagerAjaxSettings Properties

**From `data-binding.md` (lines 45-80):**

| Property | Type | Required | Purpose |
|--|--|--|--|
| `Url` | string | Yes | Endpoint for file operations (Read, Create, Delete, Rename, Search, Details, Copy, Move) |
| `UploadUrl` | string | Optional | Endpoint for file upload |
| `DownloadUrl` | string | Optional | Endpoint for file download |
| `GetImageUrl` | string | Optional | Endpoint for image preview |

### AJAX Configuration Example

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### Using External Service URLs

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="https://your-api-domain/api/FileManager/FileOperations"
                             UploadUrl="https://your-api-domain/api/FileManager/Upload"
                             DownloadUrl="https://your-api-domain/api/FileManager/Download"
                             GetImageUrl="https://your-api-domain/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

## File Provider Setup

### Physical File Provider

From `getting-started-with-web-app.md` (lines 200-250):

1. **Create Models folder** in server project
2. **Create Controllers folder** and `FileManagerController.cs`
3. **Add wwwroot/Files folder** with your files and folders

### Create FileManagerController

From `getting-started-with-web-app.md` (lines 280-330):

```csharp
using Microsoft.AspNetCore.Mvc;
using Syncfusion.EJ2.FileManager.Base;
using Syncfusion.EJ2.FileManager.PhysicalFileProvider;

namespace YourProject.Server.Controllers
{
    [Route("api/[controller]")]
    public class FileManagerController : Controller
    {
        public PhysicalFileProvider operation;
        public string basePath;
        string root = "wwwroot\\Files";

        public FileManagerController(IWebHostEnvironment hostingEnvironment)
        {
            this.basePath = hostingEnvironment.ContentRootPath;
            this.operation = new PhysicalFileProvider();
            this.operation.RootFolder(this.basePath + "\\" + this.root);
        }

        [Route("FileOperations")]
        public object FileOperations([FromBody] FileManagerDirectoryContent args)
        {
            switch (args.Action)
            {
                case "read":
                    return this.operation.ToCamelCase(this.operation.GetFiles(args.Path, args.ShowHiddenItems));
                case "delete":
                    return this.operation.ToCamelCase(this.operation.Delete(args.Path, args.Names));
                case "copy":
                    return this.operation.ToCamelCase(this.operation.Copy(args.Path, args.TargetPath, args.Names, args.RenameFiles, args.TargetData));
                case "move":
                    return this.operation.ToCamelCase(this.operation.Move(args.Path, args.TargetPath, args.Names, args.RenameFiles, args.TargetData));
                case "details":
                    return this.operation.ToCamelCase(this.operation.Details(args.Path, args.Names));
                case "create":
                    return this.operation.ToCamelCase(this.operation.Create(args.Path, args.Name));
                case "search":
                    return this.operation.ToCamelCase(this.operation.Search(args.Path, args.SearchString, args.ShowHiddenItems, args.CaseSensitive));
                case "rename":
                    return this.operation.ToCamelCase(this.operation.Rename(args.Path, args.Name, args.NewName));
            }
            return null;
        }
    }
}
```

### FileManagerDirectoryContent Data Model

**From `file-operations.md` (lines 35-60):**

The component uses `FileManagerDirectoryContent` to represent files and folders:

```csharp
public class FileManagerDirectoryContent
{
    public string Name { get; set; }
    public string DateCreated { get; set; }
    public string DateModified { get; set; }
    public string FilterPath { get; set; }
    public bool HasChild { get; set; }
    public bool IsFile { get; set; }
    public long Size { get; set; }
    public string Type { get; set; }
}
```

### Add wwwroot Files

1. Create folder: `wwwroot/Files/`
2. Add your files and folders to this directory
3. FileManager will display them when you initialize the component

## Complete Example

**Full Blazor Web App Example:**

From `getting-started-with-web-app.md` (complete setup):

```razor
@rendermode InteractiveAuto

@using Syncfusion.Blazor.FileManager

<h2>File Manager</h2>

<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

**Program.cs setup:**

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents()
    .AddInteractiveWebAssemblyComponents();

builder.Services.AddSyncfusionBlazor();
builder.Services.AddControllers();

var app = builder.Build();

if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Error");
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();
app.UseAntiforgery();

app.UseRouting();
app.MapControllers();

app.MapRazorComponents<App>();

app.Run();
```

## Next Steps

1. Choose your file provider from [File Providers and Storage](file-providers.md)
2. Configure file operations in [File Operations](file-operations.md)
3. Set up events in [Events and Callbacks](events-and-callbacks.md)
4. Customize UI in [Customization and UI](customization.md)

## Troubleshooting

**Component not rendering?**
- Verify Syncfusion Blazor service is registered in Program.cs
- Check that stylesheets and scripts are added to App.razor
- Ensure NuGet packages are installed

**File operations not working?**
- Verify FileManagerController is created and mapped
- Check that AJAX URLs are correct
- Ensure wwwroot/Files directory exists

**Uploads not working?**
- Implement UploadUrl endpoint in controller
- Verify UploadUrl is configured in FileManagerAjaxSettings
- Check file permissions on server
