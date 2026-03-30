# Getting Started with Blazor DatePicker

This guide covers installation, setup, and basic usage of the Syncfusion Blazor DatePicker component.

## Installation

### Step 1: Install NuGet Packages

Open your project terminal and install the required packages:

```bash
dotnet add package Syncfusion.Blazor.Calendars
dotnet add package Syncfusion.Blazor.Themes
```

### Step 2: Import Namespaces

Add to `_Imports.razor`:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Calendars
```

### Step 3: Register Syncfusion Services

Update `Program.cs`:

```csharp
// For Blazor Server
builder.Services.AddSyncfusionBlazor();

// For Blazor WebAssembly (in Program.cs of Web project)
builder.Services.AddSyncfusionBlazor();
```

### Step 4: Add Theme

In your `App.razor` or main layout file:

```html
<!DOCTYPE html>
<html>
<head>
    <!-- Add Syncfusion theme CSS -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
<body>
    <div id="app"></div>

    <!-- Add Syncfusion Blazor script -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
</body>
</html>
```

### Available Themes

- `bootstrap5.css` - Bootstrap 5 theme
- `material.css` - Material Design
- `fluent.css` - Fluent UI
- `tailwind.css` - Tailwind CSS
- `fabric.css` - Office Fabric

## Basic Implementation

### Simple DatePicker

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
}
```

**Result:** A calendar input field with today's date selected.

### With Placeholder

```razor
<SfDatePicker TValue="DateTime?" Placeholder="Select your date"></SfDatePicker>
```

### Programmatic Value Setting

```razor
<button @onclick="SetTodayDate">Set Today</button>
<SfDatePicker @ref="datePicker" TValue="DateTime?" Value="@selectedDate"></SfDatePicker>

@code {
    SfDatePicker<DateTime?> datePicker;
    DateTime? selectedDate;

    void SetTodayDate()
    {
        selectedDate = DateTime.Now.Date;
    }
}
```

## Blazor Project Types

### Blazor Server App

```razor
<!-- Default setup works fine for Server -->
<SfDatePicker TValue="DateTime?" Value="@selectedDate" ValueChange="@OnDateChange"></SfDatePicker>

@code {
    DateTime? selectedDate;

    void OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        // Server-side logic here
    }
}
```

### Blazor WebAssembly App

Same implementation, but ensure:
1. NuGet packages are in **client project** (`.Client`)
2. `Program.cs` in **client project** has service registration
3. Theme CSS loaded in `index.html` (not `App.razor`)

Example `Program.cs` for WebAssembly:
```csharp
var builder = WebAssemblyHostBuilder.CreateDefault(args);

builder.RootComponents.Add<App>("#app");
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

### Blazor Web App (.NET 8+)

For interactive components:

```razor
@* In Components/Pages/DatePickerPage.razor *@
@page "/datepicker"
@rendermode InteractiveServer

<SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>

@code {
    DateTime? selectedDate;
}
```

Use `@rendermode InteractiveServer` or `@rendermode InteractiveWebAssembly`.

## Configuration Examples

### Date Input with Format

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" Format="dd/MM/yyyy" Placeholder="DD/MM/YYYY"></SfDatePicker>
```

### Disabled DatePicker

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" Enabled="false"></SfDatePicker>
```

### ReadOnly Mode

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" Readonly="true"></SfDatePicker>
```

User can view but not modify the date.

### With Clear Button

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" ShowClearButton="true"></SfDatePicker>
```

Allows user to clear the selected date with a button click.

## Troubleshooting Installation

### Issue: Component not recognized
**Solution:** Verify `_Imports.razor` includes `@using Syncfusion.Blazor.Calendars`

### Issue: Styling looks broken
**Solution:** Confirm theme CSS is loaded in `App.razor` or `index.html`

### Issue: NuGet package not found
**Solution:** Run `dotnet restore` and ensure NuGet source includes `nuget.org`

### Issue: "AddSyncfusionBlazor not found"
**Solution:** Add `using Syncfusion.Blazor` namespace to `Program.cs`

## Next Steps

- **Data Binding:** Learn [two-way binding and value updates](data-binding.md)
- **Formatting:** Configure [custom date formats](date-formats-and-input.md)
- **Constraints:** Set [min/max dates and special dates](date-range-and-constraints.md)
- **Events:** Handle [value changes and user interactions](events.md)
