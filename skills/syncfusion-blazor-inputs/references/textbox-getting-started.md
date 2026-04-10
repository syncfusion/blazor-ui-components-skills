# Getting Started with Syncfusion Blazor TextBox

## Table of Contents
- [Installation & Setup](#installation--setup)
- [Creating a Blazor WASM App](#creating-a-blazor-wasm-app)
- [Registering Syncfusion Services](#registering-syncfusion-services)
- [Adding Resources](#adding-resources)
- [Basic TextBox Implementation](#basic-textbox-implementation)
- [Adding Icons](#adding-icons)
- [Floating Labels](#floating-labels)
- [First Run](#first-run)

---

## Installation & Setup

### Install NuGet Packages

Add the required Syncfusion NuGet packages to your project:

```bash
dotnet add package Syncfusion.Blazor.Inputs -Version 25.1.35
dotnet add package Syncfusion.Blazor.Themes -Version 25.1.35
```

Or use the Package Manager Console:

```powershell
Install-Package Syncfusion.Blazor.Inputs -Version 25.1.35
Install-Package Syncfusion.Blazor.Themes -Version 25.1.35
```

---

## Creating a Blazor WASM App

### Option 1: Visual Studio

1. Open Visual Studio and create a new project
2. Select **Blazor WebAssembly App**
3. Configure project name, location, and framework (.NET 8 or later recommended)
4. Complete the template wizard
5. The project structure is ready for TextBox integration

### Option 2: Visual Studio Code

Create a Blazor WASM app using the terminal:

```bash
dotnet new blazorwasm -o MyBlazorApp
cd MyBlazorApp
```

Then add the Syncfusion NuGet packages as shown above.

### Option 3: .NET CLI

```bash
dotnet new blazorwasm -o MyBlazorApp
cd MyBlazorApp
dotnet add package Syncfusion.Blazor.Inputs -Version 25.1.35
dotnet add package Syncfusion.Blazor.Themes -Version 25.1.35
dotnet restore
```

---

## Registering Syncfusion Services

Open `Program.cs` and register the Syncfusion Blazor service:

```csharp
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;
using YourProjectName;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

---

## Adding Resources

### Add Namespaces

Open `~/_Imports.razor` and add the Syncfusion namespaces:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Inputs
```

### Add Stylesheet & Script References

Open `~/wwwroot/index.html` and add the theme stylesheet and Syncfusion script in the `<head>` section:

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My Blazor App</title>
    
    <!-- Syncfusion Bootstrap5 Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    
    <!-- Syncfusion Core Script -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
    
    <!-- Optional: TextBox component script (usually auto-loaded) -->
    <!-- <script src="_content/Syncfusion.Blazor.Inputs/scripts/sf-textbox.min.js" type="text/javascript"></script> -->
    
    <link rel="stylesheet" href="app.css" />
</head>
```

**Available Themes:**
- `bootstrap5.css`
- `bootstrap5-dark.css`
- `fluent2.css`
- `fluent2-dark.css`
- `material3.css`
- `material3-dark.css`
- `tailwind.css`

---

## Basic TextBox Implementation

Add a simple TextBox to your Razor page (`Pages/Index.razor`):

```razor
@page "/"
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 300px;">
    <h2>My First TextBox</h2>
    <SfTextBox Placeholder="Enter your name"></SfTextBox>
</div>
```

### Component Anatomy

```razor
<SfTextBox 
    Placeholder="Enter text"           <!-- Hint text -->
    Value="@inputValue"                 <!-- Bound value -->
    Enabled="true"                      <!-- Enable/disable -->
    CssClass="custom-textbox"          <!-- CSS classes -->
    HtmlAttributes="@htmlAttrs">        <!-- HTML attributes -->
</SfTextBox>
```

---

## Adding Icons

Use the `AddIconAsync()` method to programmatically add icons to TextBox inputs:

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 300px;">
    <SfTextBox @ref="@TextBoxRef"
               Placeholder="Enter date"
               Created="@AddIcon">
    </SfTextBox>
</div>

@code {
    private SfTextBox TextBoxRef { get; set; }

    private async void AddIcon()
    {
        if (TextBoxRef != null)
        {
            // Add icon at the end (append position)
            await TextBoxRef.AddIconAsync("append", "e-icons e-calendar");
        }
    }
}
```

### Icon Positions

- **`"append"`**: Icon appears at the end (right side)
- **`"prepend"`**: Icon appears at the beginning (left side)

### Common Icon Classes

| Icon Class | Usage |
|------------|-------|
| `e-icons e-calendar` | Date/date picker |
| `e-icons e-search` | Search input |
| `e-icons e-close` | Clear/delete action |
| `e-icons e-eye` | Show password toggle |
| `e-icons e-user` | User profile |
| `e-icons e-mail` | Email |
| `e-icons e-phone` | Phone number |

**Example: Search TextBox with Icon**

```razor
<SfTextBox @ref="searchBox"
           Placeholder="Search..."
           Input="@OnSearch"
           Created="@AddSearchIcon">
</SfTextBox>

@code {
    private SfTextBox searchBox;
    
    private async void AddSearchIcon()
    {
        if (searchBox != null)
        {
            await searchBox.AddIconAsync("append", "e-icons e-search");
        }
    }
    
    private void OnSearch(InputEventArgs args)
    {
        // Real-time search logic
        Console.WriteLine($"Searching for: {args.Value}");
    }
}
```

---

## Floating Labels

Floating labels automatically lift above the TextBox when focused or when the field contains a value. Configure using the `FloatLabelType` property:

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 300px;">
    <!-- Auto: Float only when focused or filled -->
    <SfTextBox Placeholder="First Name" 
               FloatLabelType="@FloatLabelType.Auto">
    </SfTextBox>

    <!-- Always: Label always floats above -->
    <SfTextBox Placeholder="Last Name" 
               FloatLabelType="@FloatLabelType.Always">
    </SfTextBox>

    <!-- Never: No floating label (default) -->
    <SfTextBox Placeholder="Email" 
               FloatLabelType="@FloatLabelType.Never">
    </SfTextBox>
</div>
```

### FloatLabelType Options

| Value | Behavior |
|-------|----------|
| `Never` (default) | No floating label animation |
| `Auto` | Float on focus or when field has value |
| `Always` | Label always floated above the input |

**Best Practice**: Use `FloatLabelType.Auto` for modern form designs with space-saving layouts.

---

## First Run

### Build and Run

In your terminal:

```bash
dotnet run
```

Or press **Ctrl+F5** in Visual Studio (Windows) / **⌘+F5** (macOS).

### Expected Output

The app will launch in your default browser at `https://localhost:5001`. You should see:
- A simple TextBox with placeholder text
- Clean styling from the theme
- Responsive layout
- Smooth interactions when clicking/typing

### Verify Installation

To verify everything is working:

1. ✅ TextBox renders with placeholder
2. ✅ Placeholder text disappears when typing
3. ✅ Floating label animates (if enabled)
4. ✅ Icons display correctly (if added)
5. ✅ Theme styling is applied

---

## Common Setup Issues

| Issue | Solution |
|-------|----------|
| **Theme not loading** | Verify `index.html` has correct theme link path |
| **Component not rendering** | Check `_Imports.razor` has `@using Syncfusion.Blazor.Inputs` |
| **Script errors** | Ensure `AddSyncfusionBlazor()` is called in `Program.cs` |
| **NuGet conflicts** | Run `dotnet restore` to resolve package dependencies |
| **Icons not showing** | Check CSS includes `e-icons` class library (bundled with theme) |

---

## Next Steps

- ✅ [Types & Sizing](../types-and-sizing.md) - Explore TextBox variants
- ✅ [Input Configuration](../input-configuration.md) - Configure placeholder, masking, groups
- ✅ [Data Binding & Validation](../data-binding-validation.md) - Bind to form data, add validation
- ✅ [Events & Interactions](../events-handling.md) - Handle user interactions
- ✅ [Styling & Appearance](../styling-appearance.md) - Customize colors and theme
