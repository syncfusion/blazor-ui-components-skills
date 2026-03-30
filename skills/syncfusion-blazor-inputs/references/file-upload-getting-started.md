# Getting Started with File Upload

## Table of Contents
- [Installation Requirements](#installation)
- [Visual Studio Setup](#visual-studio)
- [Visual Studio Code Setup](#vscode)
- [.NET CLI Setup](#cli)
- [Import Namespaces](#namespaces)
- [Register Service](#service)
- [Add Stylesheets](#stylesheets)
- [Basic Component](#basic)
- [Running Your App](#run)

---

## Installation Requirements

### Prerequisites
- .NET 6.0 or later SDK installed
- Visual Studio 2022 or Visual Studio Code
- Basic knowledge of Blazor application structure

### NuGet Packages
Install the required Syncfusion Blazor packages:
- `Syncfusion.Blazor.Inputs` - Contains File Upload component
- `Syncfusion.Blazor.Themes` - For styling

---

## Visual Studio Setup

### Step 1: Create Blazor WebAssembly App
1. Open Visual Studio 2022
2. Create a new project → Select "Blazor WebAssembly App"
3. Configure project name and location
4. Ensure you select "ASP.NET Core hosted" if backend API needed

### Step 2: Install NuGet Packages
In the **Package Manager Console**:

```powershell
Install-Package Syncfusion.Blazor.Inputs -Version 25.1.35
Install-Package Syncfusion.Blazor.Themes -Version 25.1.35
```

Or use the **NuGet Package Manager UI**:
1. Right-click Project → Manage NuGet Packages
2. Search "Syncfusion.Blazor.Inputs"
3. Install latest version

---

## Visual Studio Code Setup

### Step 1: Create Blazor App via CLI
```bash
dotnet new blazorwasm -o BlazorUploadApp
cd BlazorUploadApp
```

### Step 2: Install Packages via Terminal
```bash
dotnet add package Syncfusion.Blazor.Inputs
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

### Step 3: Open in VS Code
```bash
code .
```

---

## .NET CLI Setup

### Create and Setup
```bash
# Create Blazor WebAssembly app
dotnet new blazorwasm -o BlazorUploadApp
cd BlazorUploadApp

# Install Syncfusion packages
dotnet add package Syncfusion.Blazor.Inputs
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

---

## Import Namespaces

Open `_Imports.razor` file in your project root and add:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Inputs
```

This makes File Upload component available throughout your Blazor app.

---

## Register Service

Open `Program.cs` file and register Syncfusion Blazor service:

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

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

---

## Add Stylesheets

Open `index.html` (in `wwwroot` folder) and add stylesheet and script references in the `<head>` section:

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Blazor File Upload</title>
    <base href="/" />
    
    <!-- Add Syncfusion Blazor theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    
    <!-- Add Syncfusion scripts -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
</head>
```

### Available Themes
- `bootstrap5.css` - Bootstrap 5 theme (recommended for modern apps)
- `fluent2.css` - Microsoft Fluent 2 design
- `material3.css` - Material Design 3
- `tailwind.css` - Tailwind CSS theme

---

## Basic Component

Create a new Razor component `FileUpload.razor`:

```razor
@page "/file-upload"
@using Syncfusion.Blazor.Inputs

<div class="container">
    <h3>File Upload Example</h3>
    
    <SfUploader AutoUpload="true">
        <UploaderAsyncSettings 
            SaveUrl="api/upload/save" 
            RemoveUrl="api/upload/remove">
        </UploaderAsyncSettings>
    </SfUploader>
</div>

@code {
    // Component logic here
}
```

### Component Breakdown

- **`@page "/file-upload"`** - Sets route for component
- **`<SfUploader>`** - Main file upload component
- **`AutoUpload="true"`** - Files upload automatically after selection
- **`UploaderAsyncSettings`** - Configures backend API endpoints
  - `SaveUrl` - API endpoint for file upload
  - `RemoveUrl` - API endpoint for file deletion

---

## Running Your App

### Via Visual Studio
1. Press **F5** or click "Start Debugging"
2. App opens in default browser
3. Navigate to your file upload page

### Via CLI
```bash
dotnet run
```
Then open `https://localhost:5001` in browser.

### Test the Upload
1. Click the upload area
2. Select a file from your computer
3. File uploads to the server
4. Check your configured upload directory

---

## Next Steps

After basic setup:
1. Configure file type restrictions with `AllowedExtensions`
2. Set file size limits with `MaxFileSize` and `MinFileSize`
3. Handle file selection with `ValueChange` event
4. Customize appearance with CSS or templates
5. Implement server-side file handling
