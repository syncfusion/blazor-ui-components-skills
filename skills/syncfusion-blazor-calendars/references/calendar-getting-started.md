# Getting Started with Blazor Calendar

## Table of Contents
- [Installation & NuGet Packages](#installation--nuget-packages)
- [Web App Setup](#web-app-setup)
- [Server App Setup](#server-app-setup)
- [Register Syncfusion Service](#register-syncfusion-service)
- [Add Styles & Scripts](#add-styles--scripts)
- [Add Calendar Component](#add-calendar-component)
- [Basic Examples](#basic-examples)
- [Min/Max Date Constraints](#minmax-date-constraints)
- [Quick Start Checklist](#quick-start-checklist)

---

## Installation & NuGet Packages

### Required Packages

The Blazor Calendar requires two NuGet packages:

1. **Syncfusion.Blazor.Calendars** - Calendar component
2. **Syncfusion.Blazor.Themes** - Theme styles

### Installation Methods

**Via Visual Studio Package Manager:**
```bash
Tools → NuGet Package Manager → Manage NuGet Packages for Solution
Search: "Syncfusion.Blazor.Calendars"
Install latest version
```

**Via NuGet Package Manager Console:**
```powershell
Install-Package Syncfusion.Blazor.Calendars -Version 28.1.33
Install-Package Syncfusion.Blazor.Themes -Version 28.1.33
```

**Via .NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.Calendars
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

> **Note:** Version numbers shown are examples. Use the latest stable version available on [NuGet.org](https://www.nuget.org/packages?q=syncfusion.blazor).

---

## Web App Setup

### Create Blazor WebAssembly App

**Visual Studio:**
1. File → New Project
2. Select "Blazor WebAssembly App"
3. Configure project name & location

**Visual Studio Code:**
```bash
dotnet new blazorwasm -o BlazorApp
cd BlazorApp
```

**NuGet install (as shown above)**

### Add Namespaces

Edit `~/_Imports.razor` to include Calendar namespaces:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Calendars
```

### Register Service

Edit `~/Program.cs`:

```csharp
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(serviceProvider => 
    new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

### Add Styles & Scripts

Edit `~/index.html` in the `<head>` section:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <title>Blazor Calendar</title>
    <base href="/" />
    
    <!-- Syncfusion Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    
    <!-- Other styles -->
    <link rel="stylesheet" href="app.css" />
</head>
<body>
    <div id="app"></div>
    
    <!-- Syncfusion Scripts -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
    
    <!-- Other scripts -->
    <script src="_framework/blazor.webassembly.js"></script>
</body>
</html>
```

---

## Server App Setup

### Create Blazor Server App

**Visual Studio:**
1. File → New Project
2. Select "Blazor Server App"
3. Configure project name & location

**Visual Studio Code/CLI:**
```bash
dotnet new blazorserver -o BlazorServerApp
cd BlazorServerApp
```

### Add Namespaces

Edit `~/Pages/_Layout.cshtml` or `~/App.razor`:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Calendars
```

### Register Service

Edit `~/Program.cs`:

```csharp
var builder = WebApplicationBuilder.CreateBuilder(args);

// Add services
builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();

app.UseHttpsRedirection();
app.UseStaticFiles();
app.UseRouting();

app.MapBlazorHub();
app.MapFallbackToPage("/_Host");

app.Run();
```

### Add Styles & Scripts

Edit `~/Pages/_Layout.cshtml`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <title>Blazor Calendar</title>
    <base href="~/" />
    
    <!-- Syncfusion Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    
    <!-- Other styles -->
    <link rel="stylesheet" href="~/css/site.css" />
</head>
<body>
    <component type="typeof(App)" render-mode="ServerPrerendered" />
    
    <!-- Syncfusion Scripts -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
    
    <!-- Blazor Script -->
    <script src="_framework/blazor.server.js"></script>
</body>
</html>
```

---

## Register Syncfusion Service

### What Gets Registered?

`builder.Services.AddSyncfusionBlazor()` registers:
- Interop services for JavaScript communication
- Localization support
- Theme utilities
- Component lifecycles

### Service Configuration

Basic registration (default):
```csharp
builder.Services.AddSyncfusionBlazor();
```

With options (advanced):
```csharp
builder.Services.AddSyncfusionBlazor(options => 
{
    // Additional configuration if needed
});
```

> The Calendar component requires this service to function properly.

---

## Add Styles & Scripts

### Available Themes

Syncfusion provides multiple themes via NuGet Static Web Assets:

| Theme | Path |
|-------|------|
| Bootstrap 5 | `_content/Syncfusion.Blazor.Themes/bootstrap5.css` |
| Bootstrap 4 | `_content/Syncfusion.Blazor.Themes/bootstrap4.css` |
| Tailwind | `_content/Syncfusion.Blazor.Themes/tailwind.css` |
| Fluent | `_content/Syncfusion.Blazor.Themes/fluent.css` |
| Material | `_content/Syncfusion.Blazor.Themes/material.css` |

**Dark variants:** Append `-dark` to filename (e.g., `bootstrap5-dark.css`)

### Script Reference

Always include the Syncfusion script:
```html
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

This enables JavaScript interop for Calendar interactions.

### Theme Selection

Choose ONE theme for your app:
```html
<!-- Light Theme (Bootstrap 5) -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Dark Theme -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5-dark.css" rel="stylesheet" />
```

---

## Add Calendar Component

### Minimal Example

Add to any Blazor component file (`.razor`):

```razor
@using Syncfusion.Blazor.Calendars

<SfCalendar TValue="DateTime?"></SfCalendar>

@code {
    // Component renders with today's date highlighted
}
```

### With Value Binding

```razor
@using Syncfusion.Blazor.Calendars

<p>Selected Date: @SelectedDate</p>
<SfCalendar TValue="DateTime?" Value="@SelectedDate"></SfCalendar>

@code {
    public DateTime? SelectedDate { get; set; } = DateTime.Now;
}
```

### With Two-Way Binding

```razor
@using Syncfusion.Blazor.Calendars

<p>Selected Date: @SelectedDate</p>
<SfCalendar TValue="DateTime?" @bind-Value="@SelectedDate"></SfCalendar>

@code {
    public DateTime? SelectedDate { get; set; } = DateTime.Now;
}
```

---

## Basic Examples

### Example 1: Basic Calendar

```razor
@using Syncfusion.Blazor.Calendars

<h3>Basic Calendar</h3>
<SfCalendar TValue="DateTime?"></SfCalendar>
```

**Output:** Calendar showing current month with today highlighted.

### Example 2: Calendar with Initial Value

```razor
@using Syncfusion.Blazor.Calendars

<h3>Calendar with Value</h3>
<SfCalendar TValue="DateTime?" Value="@(new DateTime(2024, 3, 15))"></SfCalendar>
```

**Output:** Calendar displaying March 2024, with the 15th selected.

### Example 3: Reactive Calendar

```razor
@using Syncfusion.Blazor.Calendars

<p>Selected: @SelectedDate?.ToString("dd/MM/yyyy")</p>
<SfCalendar TValue="DateTime?" @bind-Value="@SelectedDate"></SfCalendar>

<button @onclick="ResetDate">Reset to Today</button>

@code {
    public DateTime? SelectedDate { get; set; } = DateTime.Now;
    
    private void ResetDate()
    {
        SelectedDate = DateTime.Now;
    }
}
```

**Output:** Calendar that updates a display when date is changed; button resets to today.

---

## Min/Max Date Constraints

### Purpose

Restrict date selection to a specific range. Out-of-range dates appear disabled.

### Properties

- **Min** (DateTime) - Earliest selectable date (inclusive)
- **Max** (DateTime) - Latest selectable date (inclusive)

### Basic Example

```razor
@using Syncfusion.Blazor.Calendars

<h3>Date Range: 5th to 27th of Current Month</h3>

<SfCalendar TValue="DateTime?" 
    Min="@MinDate" 
    Value="@SelectedDate" 
    Max="@MaxDate">
</SfCalendar>

@code {
    public DateTime MinDate { get; set; } = 
        new DateTime(DateTime.Now.Year, DateTime.Now.Month, 5);
    
    public DateTime MaxDate { get; set; } = 
        new DateTime(DateTime.Now.Year, DateTime.Now.Month, 27);
    
    public DateTime? SelectedDate { get; set; } = 
        new DateTime(DateTime.Now.Year, DateTime.Now.Month, 15);
}
```

**Output:** Calendar shows only days 5-27 as selectable; others are grayed out.

### Advanced: Dynamic Range Update

```razor
@using Syncfusion.Blazor.Calendars

<p>Min Date: @MinDate.ToString("dd/MM/yyyy")</p>
<p>Max Date: @MaxDate.ToString("dd/MM/yyyy")</p>

<SfCalendar TValue="DateTime?" 
    Min="@MinDate" 
    Value="@SelectedDate" 
    Max="@MaxDate">
</SfCalendar>

<button @onclick="ExtendRange">Extend Range by 1 Week</button>

@code {
    public DateTime MinDate { get; set; } = DateTime.Now;
    public DateTime MaxDate { get; set; } = DateTime.Now.AddDays(7);
    public DateTime? SelectedDate { get; set; } = DateTime.Now;
    
    private void ExtendRange()
    {
        MaxDate = MaxDate.AddDays(7);
    }
}
```

**Output:** Calendar range expands dynamically when button is clicked.

### Important Behavior

- Out-of-range dates are **disabled** (cannot be selected)
- If `Value` is outside range, it's automatically corrected to closest boundary
- Min/Max apply to date only; time components are ignored
- Both Min and Max are **inclusive** (both can be selected)

---

## Quick Start Checklist

- [ ] Install `Syncfusion.Blazor.Calendars` NuGet package
- [ ] Install `Syncfusion.Blazor.Themes` NuGet package
- [ ] Add `@using Syncfusion.Blazor` to `_Imports.razor`
- [ ] Add `@using Syncfusion.Blazor.Calendars` to `_Imports.razor`
- [ ] Call `builder.Services.AddSyncfusionBlazor()` in `Program.cs`
- [ ] Include theme stylesheet in `index.html` or `_Layout.cshtml`
- [ ] Include `syncfusion-blazor.min.js` script
- [ ] Add `<SfCalendar TValue="DateTime?"></SfCalendar>` to component
- [ ] Run application and verify calendar renders

---

## Troubleshooting

### Calendar Not Rendering

- **Check:** Script reference is present (`syncfusion-blazor.min.js`)
- **Check:** Service registered (`AddSyncfusionBlazor()`)
- **Check:** Theme stylesheet loaded
- **Check:** Namespaces imported in component

### Styling Looks Wrong

- **Check:** Theme stylesheet loaded (not cached)
- **Try:** Clear browser cache (Ctrl+Shift+Delete)
- **Try:** Hard refresh (Ctrl+F5)

### Date Value Not Updating

- **Check:** Using `@bind-Value` for two-way binding
- **Check:** `TValue` type matches your property (`DateTime?` vs `DateTime`)
- **Check:** Property is public and settable

### Min/Max Not Working

- **Check:** Min < Max
- **Check:** Dates are set before rendering
- **Check:** Min and Max use date-only (time is ignored)
