# Getting Started with DateRangePicker

## Installation & Setup

### Step 1: Install NuGet Packages

Install the required Syncfusion.Blazor.Calendars and theme packages using NuGet Package Manager or .NET CLI:

```bash
dotnet add package Syncfusion.Blazor.Calendars
dotnet add package Syncfusion.Blazor.Themes
```

### Step 2: Import Namespaces

Open the `~/_Imports.razor` file and add the following namespace imports:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Calendars
```

### Step 3: Register Syncfusion Service

Register the Syncfusion Blazor service in the `~/Program.cs` file:

```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });
builder.Services.AddSyncfusionBlazor();  // Add this line

await builder.Build().RunAsync();
```

**For Blazor Server Apps:**
```csharp
builder.Services.AddSyncfusionBlazor();  // In Program.cs for Blazor Server
```

### Step 4: Add Theme Stylesheet & Script

Include the Syncfusion stylesheet and script resources in the `~/index.html` file (for WebAssembly) or `~/Pages/_Host.cshtml` (for Server):

```html
<head>
    <!-- Add this stylesheet link in the <head> section -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>

<body>
    <!-- Add this script at the end of <body> section -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
</body>
```

**Available Theme Options:**
- `bootstrap5.css` - Bootstrap 5 (default)
- `material.css` - Material Design
- `fluent.css` - Fluent UI (Microsoft)
- `tailwind.css` - Tailwind CSS
- `fabric.css` - Office Fabric

## Basic Component Usage

### Minimal Example

Add the DateRangePicker component to any `.razor` page:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" Placeholder="Choose a Range"></SfDateRangePicker>
```

This renders a date range picker with:
- Placeholder text "Choose a Range"
- DateTime? type for nullable date values
- Default styling from the selected theme
- Standard calendar interface

### With Placeholder

```razor
<SfDateRangePicker TValue="DateTime?" Placeholder="Select travel dates"></SfDateRangePicker>
```

## Setting Date Constraints

Use the `Min` and `Max` properties to restrict the selectable date range:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    Placeholder="Choose a Range" 
    Min="@MinDate" 
    Max="@MaxDate">
</SfDateRangePicker>

@code {
    public DateTime MinDate { get; set; } = new DateTime(2024, 1, 1);
    public DateTime MaxDate { get; set; } = new DateTime(2024, 12, 31);
}
```

### Dynamic Constraints

Update constraints based on business logic:

```razor
<SfDateRangePicker TValue="DateTime?" 
    Min="@CalculateMinDate()" 
    Max="@CalculateMaxDate()">
</SfDateRangePicker>

@code {
    private DateTime CalculateMinDate()
    {
        return DateTime.Today;  // No past dates
    }
    
    private DateTime CalculateMaxDate()
    {
        return DateTime.Today.AddYears(1);  // Up to 1 year in future
    }
}
```

## Project Type Specific Setup

### Blazor WebAssembly App

1. Create or open existing Blazor WebAssembly project
2. Follow installation steps above
3. Add stylesheet/script to `~/index.html`
4. Register service in `Program.cs`
5. Add component to `.razor` pages

### Blazor Server App

1. Create or open existing Blazor Server project
2. Install packages via NuGet
3. Add stylesheet/script to `~/Pages/_Host.cshtml`
4. Register service in `Program.cs`
5. Add component to `.razor` pages

### Blazor Web App (.NET 8+)

```razor
@page "/daterange"
@using Syncfusion.Blazor.Calendars
@rendermode InteractiveServer

<h3>Date Range Selection</h3>
<SfDateRangePicker TValue="DateTime?" Placeholder="Select range"></SfDateRangePicker>
```

## Verify Installation

Test that everything is installed correctly:

```razor
@page "/test-daterangepicker"
@using Syncfusion.Blazor.Calendars

<h3>DateRangePicker Test</h3>
<SfDateRangePicker TValue="DateTime?" 
    Placeholder="If you see a calendar, setup is correct!"
    Min="@minRangeDate"
    Max="@maxRangeDate">
</SfDateRangePicker>

@code {
    private DateTime minRangeDate = DateTime.Today;
    private DateTime maxRangeDate = DateTime.Today.AddDays(90);
}
```

If you see a working date range picker:
- ✅ NuGet packages installed
- ✅ Namespaces imported
- ✅ Service registered
- ✅ Stylesheet loaded
- ✅ Script loaded

## Troubleshooting Installation

### Component Not Visible

**Problem:** DateRangePicker renders but appears blank or unstyled

**Solutions:**
1. Check theme CSS path in index.html matches package version
2. Verify CSS file exists in `_content/Syncfusion.Blazor.Themes/`
3. Try clearing browser cache (Ctrl+Shift+Delete)
4. Check browser console for 404 errors

### "SfDateRangePicker is not recognized"

**Problem:** Compile error about unknown component

**Solutions:**
1. Verify `@using Syncfusion.Blazor.Calendars` in _Imports.razor
2. Check that NuGet package is installed
3. Rebuild project (Ctrl+Shift+B)

### Script Not Loading

**Problem:** JavaScript errors or interactive features don't work

**Solutions:**
1. Verify script src in index.html: `_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js`
2. Ensure script is placed before closing `</body>` tag
3. Check Network tab in browser DevTools for failed requests

### License Warning

**Problem:** Console shows license expiry message

**Solution:** Register valid Syncfusion license key in Program.cs:
```csharp
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR-LICENSE-KEY");
```

Get a free community license at https://www.syncfusion.com/sales/products

## Next Steps

- Read [range-selection.md](range-selection.md) to bind data to the component
- Read [customization-and-styling.md](customization-and-styling.md) to customize appearance
- Read [events-and-interactions.md](events-and-interactions.md) to handle user interactions
