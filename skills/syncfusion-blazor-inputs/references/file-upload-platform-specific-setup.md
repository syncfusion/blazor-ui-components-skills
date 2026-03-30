# Platform-Specific Setup

## Table of Contents
- [Blazor WebAssembly App](#wasm)
- [Blazor Server App](#server)
- [Blazor Web App](#webapp)
- [MAUI Integration](#maui)
- [Different Render Modes](#render)
- [Platform Differences](#differences)

---

## Blazor WebAssembly App

### Prerequisites
- Visual Studio 2022 or VS Code
- .NET 6.0 or later SDK
- Basic Blazor knowledge

### Step-by-Step Setup

#### 1. Create Blazor WebAssembly App

**Using Visual Studio:**
1. File → New → Project
2. Select "Blazor WebAssembly App"
3. Configure project and hosting options
4. Check "ASP.NET Core hosted" if API needed

**Using CLI:**
```bash
dotnet new blazorwasm -o BlazorUploadApp
cd BlazorUploadApp
```

#### 2. Install NuGet Packages

```bash
dotnet add package Syncfusion.Blazor.Inputs
dotnet add package Syncfusion.Blazor.Themes
```

#### 3. Update _Imports.razor

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Inputs
```

#### 4. Configure Program.cs

```csharp
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient 
{ 
    BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) 
});

builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

#### 5. Add Styles and Scripts

In `wwwroot/index.html`:

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Blazor Upload</title>
    <base href="/" />
    
    <!-- Syncfusion Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    
    <!-- Syncfusion Scripts -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
</head>
```

#### 6. Create Upload Component

Create `Pages/Upload.razor`:

```razor
@page "/upload"
@using Syncfusion.Blazor.Inputs

<h3>File Upload</h3>

<SfUploader AutoUpload="true">
    <UploaderAsyncSettings 
        SaveUrl="api/upload/save"
        RemoveUrl="api/upload/remove">
    </UploaderAsyncSettings>
</SfUploader>
```

#### 7. Create Backend API (If Hosted)

In `Server` project, create `Controllers/UploadController.cs`:

```csharp
[ApiController]
[Route("api/[controller]")]
public class UploadController : ControllerBase
{
    private readonly IHostingEnvironment _env;

    public UploadController(IHostingEnvironment env)
    {
        _env = env;
    }

    [HttpPost("[action]")]
    public async Task Save(IList<IFormFile> UploadFiles)
    {
        var uploadsPath = Path.Combine(_env.ContentRootPath, "uploads");
        if (!Directory.Exists(uploadsPath))
            Directory.CreateDirectory(uploadsPath);

        foreach (var file in UploadFiles)
        {
            var filePath = Path.Combine(uploadsPath, file.FileName);
            using (var fileStream = new FileStream(filePath, FileMode.Create))
            {
                await file.CopyToAsync(fileStream);
            }
        }
    }

    [HttpPost("[action]")]
    public void Remove(IList<IFormFile> UploadFiles)
    {
        var uploadsPath = Path.Combine(_env.ContentRootPath, "uploads");
        foreach (var file in UploadFiles)
        {
            var filePath = Path.Combine(uploadsPath, file.FileName);
            if (System.IO.File.Exists(filePath))
                System.IO.File.Delete(filePath);
        }
    }
}
```

### Key Characteristics
- Client-side rendering by default
- API calls to backend server
- Better offline support
- Smaller initial load (with caching)

---

## Blazor Server App

### Prerequisites
- Visual Studio 2022 or VS Code
- .NET 6.0 or later SDK
- Blazor Server project template

### Step-by-Step Setup

#### 1. Create Blazor Server App

**Using Visual Studio:**
1. File → New → Project
2. Select "Blazor Server App"
3. Configure project settings

**Using CLI:**
```bash
dotnet new blazorserver -o BlazorServerUploadApp
cd BlazorServerUploadApp
```

#### 2. Install NuGet Packages

```bash
dotnet add package Syncfusion.Blazor.Inputs
dotnet add package Syncfusion.Blazor.Themes
```

#### 3. Update _Imports.razor

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Inputs
```

#### 4. Configure Program.cs

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();

// Add Syncfusion services
builder.Services.AddSyncfusionBlazor();

// Configure file upload size limits
builder.Services.Configure<FormOptions>(options =>
{
    options.MultipartBodyLengthLimit = 104857600; // 100 MB
    options.ValueLengthLimit = 104857600;
    options.MemoryBufferThreshold = 104857600;
});

var app = builder.Build();

if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Error");
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();
app.UseRouting();

app.MapBlazorHub();
app.MapFallbackToPage("/_Host");

app.Run();
```

#### 5. Add Styles and Scripts

In `Pages/_Host.cshtml` (or `_Layout.cshtml`):

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Blazor Server Upload</title>
    <base href="~/" />
    
    <!-- Syncfusion Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    
    <!-- Syncfusion Scripts -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
    
    <link href="_framework/scoped.styles.css" rel="stylesheet" />
</head>
```

#### 6. Create Upload Component

Create `Pages/Upload.razor`:

```razor
@page "/upload"
@using Syncfusion.Blazor.Inputs
@using System.IO

<h3>File Upload (Server-Side)</h3>

<SfUploader AutoUpload="true">
    <UploaderEvents ValueChange="@OnChange"></UploaderEvents>
</SfUploader>

@if (!string.IsNullOrEmpty(message))
{
    <p>@message</p>
}

@code {
    private string message = "";

    private async Task OnChange(UploadChangeEventArgs args)
    {
        try
        {
            foreach (var fileEntry in args.Files)
            {
                var uploadsFolder = Path.Combine(
                    Directory.GetCurrentDirectory(), 
                    "wwwroot", 
                    "uploads"
                );
                
                if (!Directory.Exists(uploadsFolder))
                    Directory.CreateDirectory(uploadsFolder);

                var filePath = Path.Combine(uploadsFolder, fileEntry.FileInfo.Name);

                await using (var fileStream = new FileStream(
                    filePath, 
                    FileMode.Create, 
                    FileAccess.Write))
                {
                    await fileEntry.File.OpenReadStream(long.MaxValue)
                        .CopyToAsync(fileStream);
                }
                
                message = $"✓ {fileEntry.FileInfo.Name} uploaded successfully";
            }
        }
        catch (Exception ex)
        {
            message = $"✗ Error: {ex.Message}";
        }
    }
}
```

### Key Characteristics
- Server-side rendering
- Files saved directly on server
- Full access to server resources
- No need for API endpoints
- WebSocket connection required

---

## Blazor Web App

### Prerequisites
- Visual Studio 2022 or VS Code
- .NET 8.0 or later SDK

### Setup for .NET 8+

#### 1. Create Blazor Web App

```bash
dotnet new blazor -o BlazorWebUploadApp
cd BlazorWebUploadApp
```

#### 2. Install Packages

```bash
dotnet add package Syncfusion.Blazor.Inputs
dotnet add package Syncfusion.Blazor.Themes
```

#### 3. Configure Program.cs

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents()
    .AddInteractiveWebAssemblyComponents();

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
    .AddInteractiveServerRenderMode()
    .AddInteractiveWebAssemblyRenderMode()
    .AddAdditionalAssemblies(typeof(Program).Assembly);

app.Run();
```

#### 4. Update Layout

In `App.razor`:

```razor
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Blazor Web App</title>
    <base href="/" />
    <link rel="stylesheet" href="bootstrap.css" />
    
    <!-- Syncfusion Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    
    <HeadOutlet />
</head>
<body>
    <Routes />
    
    <!-- Syncfusion Scripts -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
    <script src="_framework/blazor.web.js"></script>
</body>
</html>
```

#### 5. Create Component with Render Mode

Create `Pages/Upload.razor`:

```razor
@page "/upload"
@rendermode InteractiveServer
@using Syncfusion.Blazor.Inputs

<h3>File Upload</h3>

<SfUploader AutoUpload="true">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
</SfUploader>
```

### Available Render Modes
- `InteractiveServer` - Server-side interactivity
- `InteractiveWebAssembly` - WebAssembly
- `InteractiveAuto` - Auto-detect best mode

---

## MAUI Integration

### Prerequisites
- Visual Studio 2022 with MAUI workload
- .NET 8.0 or later

### Setup Steps

#### 1. Create MAUI App

```bash
dotnet new maui -o MauiUploadApp
cd MauiUploadApp
```

#### 2. Add Syncfusion References

```bash
dotnet add package Syncfusion.Blazor.Inputs
dotnet add package Syncfusion.Maui.Core
```

#### 3. Configure MauiProgram.cs

```csharp
using Microsoft.Maui;
using Syncfusion.Blazor;
using Syncfusion.Maui.Core;

public static class MauiProgram
{
    public static MauiApp CreateMauiApp()
    {
        var builder = MauiApp.CreateBuilder();
        
        builder
            .UseMauiApp<App>()
            .ConfigureFonts(fonts =>
            {
                fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
            });

        builder.Services.AddSingleton<MainPage>();
        
        // Add Syncfusion services
        builder.ConfigureSyncfusionCore();
        builder.Services.AddSyncfusionBlazor();

        return builder.Build();
    }
}
```

#### 4. Create MAUI Blazor Component

In `Components/Upload.razor`:

```razor
@using Syncfusion.Blazor.Inputs
@using System.IO

<SfUploader AutoUpload="true" MaxFileSize="104857600">
    <UploaderEvents ValueChange="@OnFileChange"></UploaderEvents>
</SfUploader>

<p>@statusMessage</p>

@code {
    private string statusMessage = "";

    private async Task OnFileChange(UploadChangeEventArgs args)
    {
        try
        {
            foreach (var file in args.Files)
            {
                statusMessage = $"Processing: {file.FileInfo.Name}";
                await Task.Delay(100);
            }
        }
        catch (Exception ex)
        {
            statusMessage = $"Error: {ex.Message}";
        }
    }
}
```

---

## Different Render Modes

### Server Render Mode

```razor
@rendermode RenderMode.InteractiveServer

<SfUploader AutoUpload="true">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
</SfUploader>
```

**Characteristics:**
- All logic on server
- WebSocket communication
- Good for real-time updates
- Higher bandwidth usage

### WebAssembly Render Mode

```razor
@rendermode RenderMode.InteractiveWebAssembly

<SfUploader AutoUpload="true">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
</SfUploader>
```

**Characteristics:**
- Client-side execution
- Lower server load
- Better offline capability
- Larger initial download

### Auto Render Mode

```razor
@rendermode RenderMode.InteractiveAuto

<SfUploader AutoUpload="true">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
</SfUploader>
```

**Characteristics:**
- Server initially, then WebAssembly
- Best of both worlds
- Better UX and performance

---

## Platform Differences

| Feature | WebAssembly | Server | Web App | MAUI |
|---------|-------------|--------|---------|------|
| **Execution** | Client | Server | Both | Device |
| **Save Location** | API → Server | Direct | Flexible | Local/Server |
| **Offline Support** | Good | None | Limited | Excellent |
| **Performance** | Fast UI | Server dependent | Good | Excellent |
| **File Size Limit** | Browser limit | Server limit | Server limit | Device storage |
| **API Required** | Yes | Optional | Optional | Optional |

### Choosing Platform

**Use WebAssembly when:**
- High client-side performance needed
- Offline capability required
- Distributed load preferred

**Use Server when:**
- Direct server file access needed
- Simpler development desired
- Real-time updates important

**Use Web App when:**
- Need both server and client capabilities
- Want automatic mode selection
- Building modern .NET 8+ apps

**Use MAUI when:**
- Building native desktop/mobile apps
- Local file system access needed
- Cross-platform support required

---

## Complete Platform-Specific Examples

### WebAssembly with API

```razor
@* Client-side component *@
@page "/upload"
@using Syncfusion.Blazor.Inputs

<SfUploader AutoUpload="true" AllowMultiple="true">
    <UploaderAsyncSettings 
        SaveUrl="YOUR_SAVE_API_ENDPOINT"
        RemoveUrl="YOUR_REMOVE_API_ENDPOINT">
    </UploaderAsyncSettings>
</SfUploader>

```

### Server-Side Direct

```razor
@* Blazor Server component *@
@page "/upload"
@using Syncfusion.Blazor.Inputs
@using System.IO

<SfUploader AutoUpload="true">
    <UploaderEvents ValueChange="@SaveToServer"></UploaderEvents>
</SfUploader>

@code {
    private async Task SaveToServer(UploadChangeEventArgs args)
    {
        var uploadsPath = Path.Combine(
            Directory.GetCurrentDirectory(), 
            "wwwroot", 
            "uploads"
        );
        
        foreach (var file in args.Files)
        {
            var path = Path.Combine(uploadsPath, file.FileInfo.Name);
            await using var stream = new FileStream(path, FileMode.Create);
            await file.File.OpenReadStream(long.MaxValue).CopyToAsync(stream);
        }
    }
}
```

### CORS Configuration (WebAssembly with Remote API)

In Server `Program.cs`:

```csharp
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowUpload", builder =>
    {
        builder.WithOrigins("https://localhost:5001")
               .AllowAnyMethod()
               .AllowAnyHeader();
    });
});

app.UseCors("AllowUpload");
```
