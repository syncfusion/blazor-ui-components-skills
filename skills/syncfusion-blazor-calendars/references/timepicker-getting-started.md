# Getting Started with Blazor TimePicker

## Table of Contents
- [NuGet Installation](#nuget-installation)
- [Namespace Imports](#namespace-imports)
- [Service Registration](#service-registration)
- [Theme Setup](#theme-setup)
- [Basic Implementation](#basic-implementation)
- [Step Property and Time Intervals](#step-property-and-time-intervals)
- [Common Gotchas](#common-gotchas)

## NuGet Installation

The TimePicker component is part of the `Syncfusion.Blazor.Calendars` package. Install it using the Package Manager Console or dotnet CLI.

### Using Package Manager Console

```powershell
Install-Package Syncfusion.Blazor.Calendars
Install-Package Syncfusion.Blazor.Themes
```

### Using .NET CLI

```bash
dotnet add package Syncfusion.Blazor.Calendars
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

### Verify Installation

Check your `.csproj` file:

```xml
<ItemGroup>
    <PackageReference Include="Syncfusion.Blazor.Calendars" Version="*" />
    <PackageReference Include="Syncfusion.Blazor.Themes" Version="*" />
</ItemGroup>
```

## Namespace Imports

Add the Syncfusion namespaces to your `_Imports.razor` file:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Calendars
```

If you're importing in individual components, add at the top:

```razor
@using Syncfusion.Blazor.Calendars
```

## Service Registration

Register the Syncfusion Blazor service in `Program.cs`:

```csharp
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

### For Blazor Server

```csharp
// In Program.cs for Blazor Server
builder.Services.AddSyncfusionBlazor();
```

## Theme Setup

Add the theme stylesheet and script to your `index.html` (WebAssembly) or `App.razor` (Server):

### In `index.html` (WebAssembly)

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Blazor App</title>
    <base href="/" />
    
    <!-- Syncfusion theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    
    <!-- Other stylesheets -->
    <link href="app.css" rel="stylesheet" />
    <link href="YourApp.styles.css" rel="stylesheet" />
</head>
<body>
    <div id="app"></div>

    <!-- Syncfusion scripts -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
    
    <!-- Other scripts -->
    <script src="_framework/blazor.webassembly.js"></script>
</body>
</html>
```

### In `App.razor` (Server)

```razor
@page "/"

<!-- Syncfusion theme -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<Routes />

<!-- Syncfusion scripts -->
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

### Available Themes

- `bootstrap5.css` - Bootstrap 5 (recommended default)
- `material.css` - Material Design
- `fluent.css` - Fluent UI (Microsoft)
- `tailwind.css` - Tailwind CSS
- `fabric.css` - Office Fabric

## Basic Implementation

### Minimal TimePicker

```razor
@using Syncfusion.Blazor.Calendars

<SfTimePicker TValue="DateTime?" Placeholder="Select a time"></SfTimePicker>
```

### TimePicker with Initial Value

```razor
@using Syncfusion.Blazor.Calendars

<SfTimePicker TValue="DateTime?" Value="@CurrentTime" Placeholder="Select a time"></SfTimePicker>

@code {
    public DateTime CurrentTime { get; set; } = new DateTime(2026, 3, 19, 14, 30, 00);
}
```

### TimePicker with Data Binding

```razor
@using Syncfusion.Blazor.Calendars

<p>Selected Time: @SelectedTime?.ToString("HH:mm")</p>

<SfTimePicker TValue="DateTime?" @bind-Value="@SelectedTime" Placeholder="Select a time"></SfTimePicker>

@code {
    public DateTime? SelectedTime { get; set; } = DateTime.Now;
}
```

### TimePicker Disabled State

```razor
<SfTimePicker TValue="DateTime?" Enabled="false" Placeholder="Disabled"></SfTimePicker>
```

### Read-Only TimePicker

```razor
<SfTimePicker TValue="DateTime?" Value="@CurrentTime" Readonly="true"></SfTimePicker>

@code {
    public DateTime CurrentTime { get; set; } = DateTime.Now;
}
```

## Step Property and Time Intervals

The `Step` property controls the time interval in the dropdown list (in minutes).

### Common Step Values

```razor
@using Syncfusion.Blazor.Calendars

<!-- 15-minute intervals -->
<SfTimePicker TValue="DateTime?" Step=15 Placeholder="15 min intervals"></SfTimePicker>

<!-- 30-minute intervals (default) -->
<SfTimePicker TValue="DateTime?" Step=30 Placeholder="30 min intervals"></SfTimePicker>

<!-- 60-minute intervals (hourly) -->
<SfTimePicker TValue="DateTime?" Step=60 Placeholder="Hourly"></SfTimePicker>

<!-- 5-minute intervals (fine-grained) -->
<SfTimePicker TValue="DateTime?" Step=5 Placeholder="5 min intervals"></SfTimePicker>
```

### Step Use Cases

| Step Value | Use Case |
|-----------|----------|
| 5 | Medical appointments, precision scheduling |
| 15 | Standard appointments, meetings |
| 30 | Time tracking, shift scheduling |
| 60 | Hourly slots (flights, broadcasts) |

### When to Use Different Steps

```razor
@* For appointment booking: 15 or 30 minute slots *@
<SfTimePicker TValue="DateTime?" Step=15 Placeholder="Select appointment time"></SfTimePicker>

@* For shift work: 30 or 60 minute increments *@
<SfTimePicker TValue="DateTime?" Step=30 Placeholder="Select shift time"></SfTimePicker>

@* For hourly reservations: 60 minute intervals *@
<SfTimePicker TValue="DateTime?" Step=60 Placeholder="Select hour"></SfTimePicker>
```

## Common Gotchas

### Gotcha 1: Missing TimePickerEvents TValue

When using events, ensure the `TValue` is specified in `TimePickerEvents`:

```razor
<!-- ❌ WRONG -->
<SfTimePicker TValue="DateTime?">
    <TimePickerEvents ValueChange="OnChange"></TimePickerEvents>
</SfTimePicker>

<!-- ✅ CORRECT -->
<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" ValueChange="OnChange"></TimePickerEvents>
</SfTimePicker>
```

### Gotcha 2: DateTime vs DateTime?

Use `DateTime?` for nullable values (recommended for optional selection):

```razor
<!-- ✅ For optional time selection -->
<SfTimePicker TValue="DateTime?" Value="@SelectedTime"></SfTimePicker>

<!-- Use DateTime? in @code -->
@code {
    public DateTime? SelectedTime { get; set; }
}
```

### Gotcha 3: Format String Confusion

Standard format strings vs custom format strings:

```razor
<!-- Standard formats -->
<SfTimePicker TValue="DateTime?" Format="t"></SfTimePicker>  @* Short time *@
<SfTimePicker TValue="DateTime?" Format="T"></SfTimePicker>  @* Long time *@

<!-- Custom formats -->
<SfTimePicker TValue="DateTime?" Format="HH:mm"></SfTimePicker>     @* 24-hour format *@
<SfTimePicker TValue="DateTime?" Format="hh:mm tt"></SfTimePicker>  @* 12-hour with AM/PM *@
```

### Gotcha 4: Theme Not Loading

If TimePicker styling is missing:
- Verify theme link in `index.html` or `App.razor`
- Check URL path: `_content/Syncfusion.Blazor.Themes/...`
- Clear browser cache (F12 → Clear cache and reload)
- Ensure script reference: `syncfusion-blazor.min.js`

## Next Steps

- [Configure time formats →](../time-formats.md)
- [Set up data binding →](../data-binding.md)
- [Handle events →](../events-and-handlers.md)
