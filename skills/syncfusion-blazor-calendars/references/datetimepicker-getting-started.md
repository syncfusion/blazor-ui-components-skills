# Getting Started with DateTimePicker

Learn how to install, configure, and implement the Syncfusion Blazor DateTimePicker component in your application.

## Installation

### Step 1: Install NuGet Package

Install the Syncfusion.Blazor.Calendars NuGet package to your Blazor project:

```bash
dotnet add package Syncfusion.Blazor.Calendars
```

### Step 2: Register Syncfusion Service

In your `Program.cs` file, add the Syncfusion Blazor service:

```csharp
builder.Services.AddSyncfusionBlazor();
```

### Step 3: Import Namespace

Add the required imports in your `_Imports.razor` file:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Calendars
```

### Step 4: Add Theme

Add the Syncfusion theme stylesheet to your `App.razor` or layout file:

```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

Available themes:
- `bootstrap5.css` - Bootstrap 5 theme
- `material.css` - Material Design theme
- `fluent.css` - Fluent UI theme
- `tailwind.css` - Tailwind CSS theme
- `fabric.css` - Office Fabric theme

## Basic Implementation

### Simple DateTimePicker

```blazor
@page "/datetimepicker"
@using Syncfusion.Blazor.Calendars

<SfDateTimePicker TValue="DateTime?"></SfDateTimePicker>
```

### DateTimePicker with Default Value

```blazor
<SfDateTimePicker TValue="DateTime?" Value="@selectedDate"></SfDateTimePicker>

@code {
    private DateTime? selectedDate = new DateTime(2024, 3, 19, 10, 30, 0);
}
```

### DateTimePicker with Value Binding

```blazor
<SfDateTimePicker TValue="DateTime?" @bind-Value="selectedDate"></SfDateTimePicker>

@code {
    private DateTime? selectedDate;
}
```

## Blazor Server App vs Web App

### Blazor Server App

For Blazor Server applications, the DateTimePicker works with server-side rendering and SignalR communication:

```blazor
@page "/datetimepicker"
@rendermode InteractiveServer

<SfDateTimePicker TValue="DateTime?" @bind-Value="selectedDate"></SfDateTimePicker>

@code {
    private DateTime? selectedDate;
}
```

### Blazor Web App (Interactivity)

For Blazor Web App with interactivity, specify the render mode:

```blazor
@page "/datetimepicker"
@rendermode InteractiveAuto

<SfDateTimePicker TValue="DateTime?" @bind-Value="selectedDate"></SfDateTimePicker>

@code {
    private DateTime? selectedDate;
}
```

**Render Mode Options:**
- `InteractiveServer` - Server-side interactivity with SignalR
- `InteractiveWebAssembly` - Client-side interactivity
- `InteractiveAuto` - Automatic selection (Server first, falls back to WASM)

## Complete Working Example

Here's a complete example combining all setup steps:

```blazor
@page "/datetimepicker-example"
@using Syncfusion.Blazor.Calendars

<div style="padding: 20px;">
    <h3>DateTimePicker Example</h3>
    
    <div style="margin-bottom: 15px;">
        <label>Select Date and Time:</label>
        <SfDateTimePicker TValue="DateTime?" 
                          @bind-Value="selectedDateTime"
                          Placeholder="Choose date and time">
        </SfDateTimePicker>
    </div>
    
    @if (selectedDateTime.HasValue)
    {
        <div style="margin-top: 20px; padding: 10px; background-color: #f0f0f0; border-radius: 4px;">
            <p><strong>Selected Value:</strong> @selectedDateTime.Value.ToString("g")</p>
            <p><strong>Year:</strong> @selectedDateTime.Value.Year</p>
            <p><strong>Month:</strong> @selectedDateTime.Value.ToString("MMMM")</p>
            <p><strong>Day:</strong> @selectedDateTime.Value.Day</p>
            <p><strong>Time:</strong> @selectedDateTime.Value.ToString("HH:mm:ss")</p>
        </div>
    }
</div>

@code {
    private DateTime? selectedDateTime;
}
```

## Project Structure

```
YourBlazorApp/
├── Pages/
│   ├── DateTimePicker.razor
│   └── ...
├── wwwroot/
├── _Imports.razor          # Syncfusion imports added here
├── App.razor               # Theme link added here
├── Program.cs              # Service registration
└── ...
```

## Verification

After setup, verify the component is working:

1. Open the DateTimePicker component in your browser
2. Click on the input field - the calendar popup should appear
3. Select a date and time
4. The value should display in the input field

If the component doesn't appear:
- Check that the theme CSS is loaded (check browser console for 404s)
- Verify NuGet package is installed: `dotnet list package`
- Confirm namespace imports in `_Imports.razor`
- Check that service is registered in `Program.cs`

## Common Issues

### Issue: Component not rendering
**Solution:** Ensure Syncfusion.Blazor.Calendars package is installed and service is registered.

### Issue: Styling looks broken
**Solution:** Verify the theme CSS link is correctly added to `App.razor` and the path is correct.

### Issue: Events not firing
**Solution:** For Blazor Web App, ensure render mode is set to `InteractiveServer` or `InteractiveAuto`.

---

**Next Step:** Once basic setup is complete, explore the [date-time-formatting.md](date-time-formatting.md) guide to customize date/time display formats.
