# Setup Modes and Deployment for the Image Editor

## Table of Contents
- [Blazor WebAssembly Setup](#blazor-webassembly-setup)
- [Blazor Web App Setup](#blazor-web-app-setup)
- [Interactive Render Modes](#interactive-render-modes)
- [Theme Configuration](#theme-configuration)
- [Resource References](#resource-references)
- [Troubleshooting](#troubleshooting)

## Blazor WebAssembly Setup

### Visual Studio Setup

**Prerequisites:**
- Visual Studio 2022 with Blazor workload
- .NET 6.0 or later

**Steps:**

1. Create new project: `File → New → Project`
2. Select "Blazor WebAssembly App" template
3. Configure project name and location
4. Choose authentication (optional)
5. Click Create

6. Install NuGet packages:
   - Right-click project → `Manage NuGet Packages`
   - Search "Syncfusion.Blazor.ImageEditor"
   - Install latest version
   - Search "Syncfusion.Blazor.Themes"
   - Install latest version

7. Add imports to `_Imports.razor`:
   ```razor
   @using Syncfusion.Blazor
   @using Syncfusion.Blazor.ImageEditor
   ```

8. Register service in `Program.cs`:
   ```csharp
   using Syncfusion.Blazor;
   
   var builder = WebAssemblyHostBuilder.CreateDefault(args);
   builder.RootComponents.Add<App>("#app");
   
   builder.Services.AddSyncfusionBlazor();
   
   await builder.Build().RunAsync();
   ```

9. Add stylesheet to `index.html`:
   ```html
   <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
   ```

10. Add script to `index.html`:
    ```html
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
    ```

### Visual Studio Code Setup

**Prerequisites:**
- VS Code with C# extension
- .NET 6.0 or later
- Syncfusion Blazor Extension (optional)

**Steps:**

```bash
# Create WebAssembly app
dotnet new blazorwasm -o ImageEditorApp
cd ImageEditorApp

# Install NuGet packages
dotnet add package Syncfusion.Blazor.ImageEditor
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

Then follow steps 7-10 from Visual Studio setup above.

### .NET CLI Setup

```bash
# Create project
dotnet new blazorwasm -o BlazorImageEditor
cd BlazorImageEditor

# Add packages
dotnet add package Syncfusion.Blazor.ImageEditor -v [version]
dotnet add package Syncfusion.Blazor.Themes -v [version]
dotnet restore

# Run application
dotnet watch run
```

## Blazor Web App Setup

### Visual Studio Setup

**Prerequisites:**
- Visual Studio 2022 (.NET 8.0 or later)
- Blazor Web App template support

**Steps:**

1. Create new project: `File → New → Project`
2. Select "Blazor Web App" template
3. Configure interactive render mode:
   - **Auto:** Automatically choose best mode
   - **WebAssembly:** Client-side only
   - **Server:** Server-side rendering
4. Select interactivity location (Global or Per-page)
5. Click Create

6. Install NuGet packages:
   - If using WebAssembly/Auto: Install in **Client** project
   - If using Server: Install in **Server** project
   - Search and install `Syncfusion.Blazor.ImageEditor`
   - Search and install `Syncfusion.Blazor.Themes`

7. Add imports to `_Imports.razor`:
   ```razor
   @using Syncfusion.Blazor
   @using Syncfusion.Blazor.ImageEditor
   ```

8. Register service:
   - **Server Program.cs:**
     ```csharp
     builder.Services.AddSyncfusionBlazor();
     ```
   - **Client Program.cs (if WebAssembly/Auto):**
     ```csharp
     builder.Services.AddSyncfusionBlazor();
     ```

9. Add stylesheet to `App.razor`:
   ```html
   <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
   ```

10. Add script to `App.razor`:
    ```html
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
    ```

### Visual Studio Code Setup

```bash
# Create Web App with Auto render mode
dotnet new blazor -o ImageEditorWebApp -int Auto
cd ImageEditorWebApp
cd ImageEditorWebApp.Client

# Install packages
dotnet add package Syncfusion.Blazor.ImageEditor
dotnet add package Syncfusion.Blazor.Themes
dotnet restore

# Run application
cd ..
dotnet watch run
```

### .NET CLI Setup

```bash
# Create Web App with Auto render mode
dotnet new blazor -o BlazorWebImageEditor -int Auto
cd BlazorWebImageEditor
cd BlazorWebImageEditor.Client

# Add packages
dotnet add package Syncfusion.Blazor.ImageEditor
dotnet add package Syncfusion.Blazor.Themes
dotnet restore

# Run from parent directory
cd ..
dotnet watch run
```

## Interactive Render Modes

### Auto Mode

Automatically selects optimal rendering:

```csharp
@rendermode InteractiveAuto
<SfImageEditor Height="500px"></SfImageEditor>
```

- **Server initially:** Fast first render
- **WebAssembly afterward:** Rich interactivity
- **Fallback:** Graceful degradation

### WebAssembly Mode

Client-side rendering only:

```csharp
@rendermode InteractiveWebAssembly
<SfImageEditor Height="500px"></SfImageEditor>
```

**Advantages:**
- No server required after initial load
- Works offline (after initial download)
- Reduced server load

**Considerations:**
- Larger initial download
- First-load performance impact
- Requires .NET runtime in browser

### Server Mode

Server-side rendering and interactivity:

```csharp
@rendermode InteractiveServer
<SfImageEditor Height="500px"></SfImageEditor>
```

**Advantages:**
- Minimal initial download
- Fast first render
- Better for slow networks

**Considerations:**
- Constant server connection required
- Server-side processing
- Potential latency issues

### Static (None)

Static HTML only (no interactivity):

```html
<!-- No render mode specified -->
<SfImageEditor Height="500px" DisableInteraction="true"></SfImageEditor>
```

**Use for:**
- Image preview/display only
- Server-side rendering static content

## Theme Configuration

### Built-in Themes

Select from four professionally designed themes:

#### Bootstrap5 (Default)

```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

Modern, clean design based on Bootstrap framework.

#### Fluent2

```html
<link href="_content/Syncfusion.Blazor.Themes/fluent2.css" rel="stylesheet" />
```

Microsoft Fluent Design System aesthetic.

#### Material3

```html
<link href="_content/Syncfusion.Blazor.Themes/material3.css" rel="stylesheet" />
```

Google Material Design 3 standard.

#### Tailwind3

```html
<link href="_content/Syncfusion.Blazor.Themes/tailwind3.css" rel="stylesheet" />
```

Tailwind CSS-inspired minimalist design.

### Light and Dark Variants

Each theme includes light and dark variants:

```html
<!-- Light theme (default) -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Dark theme -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5-dark.css" rel="stylesheet" />
```

### Switch Themes Dynamically

```csharp
@inject IJSRuntime JS

<SfDropDownList TValue="string" TItem="ThemeItem" 
    DataSource="@Themes" 
    Fields="@(new DropDownListFieldSettings { Text = "Name", Value = "Value" })"
    @bind-Value="SelectedTheme"
    Change="OnThemeChange">
</SfDropDownList>

@code {
    private string SelectedTheme = "bootstrap5";
    
    private List<ThemeItem> Themes = new()
    {
        new ThemeItem { Name = "Bootstrap 5", Value = "bootstrap5" },
        new ThemeItem { Name = "Fluent 2", Value = "fluent2" },
        new ThemeItem { Name = "Material 3", Value = "material3" },
        new ThemeItem { Name = "Tailwind 3", Value = "tailwind3" }
    };
    
    private async Task OnThemeChange(ChangeEventArgs<string> args)
    {
        string href = $"_content/Syncfusion.Blazor.Themes/{args.Value}.css";
        await JS.InvokeVoidAsync("changeTheme", href);
    }
    
    public class ThemeItem
    {
        public string Name { get; set; }
        public string Value { get; set; }
    }
}
```

## Resource References

### Static Web Assets (Recommended)

Reference resources from NuGet via Static Web Assets:

```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

**Advantages:**
- Automatic versioning
- Works offline
- No external dependencies
- Faster delivery

### CDN References

Alternative using CDN (internet required):

```html
<link href="https://cdn.syncfusion.com/blazor/[version]/styles/bootstrap5.css" rel="stylesheet" />
<script src="https://cdn.syncfusion.com/blazor/[version]/scripts/syncfusion-blazor.min.js"></script>
```

**Replace [version] with actual release version.**

### Custom Resource Generator (CRG)

Use CRG to create custom theme bundles:

1. Visit Syncfusion CRG website
2. Select components (Image Editor)
3. Choose theme variant
4. Generate custom CSS bundle
5. Reference in application

```html
<link href="_content/CustomTheme/custom-bundle.css" rel="stylesheet" />
```

## Troubleshooting

### Component Not Rendering

**Issue:** SfImageEditor element appears empty

**Solutions:**
- Verify stylesheet is loaded (DevTools → Network)
- Check script reference is present
- Confirm service registered in Program.cs
- Clear browser cache and rebuild

### Styling Issues

**Issue:** Component looks unstyled or broken

**Solutions:**
- Switch theme stylesheet
- Check theme CSS path is correct
- Verify no CSS conflicts in application
- Use browser DevTools to inspect styles

### Interactivity Not Working

**Issue:** Component loads but interactions fail

**Solutions:**
- Verify JavaScript initialized (check console)
- Confirm InteractiveRender mode set correctly
- Check circuit connection (Server mode)
- Review browser console for errors

### Package Version Conflicts

**Issue:** Assembly version mismatch errors

**Solutions:**
- Update all Syncfusion packages to same version
- Clean NuGet cache: `dotnet nuget locals all --clear`
- Remove bin/obj folders and rebuild
- Check project target framework compatibility

### Performance Issues

**Issue:** Component slow to load or respond

**Solutions:**
- Use WebAssembly render mode for rich interactivity
- Minimize initial bundle size
- Defer non-critical resources
- Profile with DevTools Performance tab
- Consider lazy loading

### CORS Issues

**Issue:** Cannot load resources from CDN

**Solutions:**
- Use Static Web Assets instead of CDN
- Configure CORS in server application
- Check firewall/network restrictions
- Verify CDN URL is accessible

Proper setup and configuration ensures optimal performance and user experience.
