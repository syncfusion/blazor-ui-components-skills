# Toast - Getting Started

This guide covers the basics of implementing the Syncfusion Blazor Toast component for displaying temporary notification messages.

## Overview

The SfToast component is a lightweight, non-blocking notification popup that appears temporarily to provide user feedback. It's ideal for success messages, errors, warnings, and informational notifications that don't require persistent display.

**When to use Toast:**
- Show temporary feedback after user actions (save, delete, update)
- Display non-critical notifications that auto-dismiss
- Provide status updates without interrupting workflow
- Show multiple notifications in a stack
- Need positioned notifications (corners, center)

## Installation

### Step 1: Install NuGet Package

```bash
dotnet add package Syncfusion.Blazor.Notifications
dotnet add package Syncfusion.Blazor.Themes
```

### Step 2: Register Syncfusion Service

Add the service registration in **Program.cs**:

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// Add Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();
```

### Step 3: Add Theme and Script References

In **App.razor** (or your main layout file):

```html
<head>
    <!-- Syncfusion Blazor theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>

<body>
    <!-- App content -->
    
    <!-- Syncfusion Blazor scripts -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
</body>
```

**Available themes:**
- `bootstrap5.css` - Bootstrap 5
- `material.css` - Material Design
- `fluent.css` - Fluent UI
- `tailwind.css` - Tailwind CSS
- `fabric.css` - Office Fabric

### Step 4: Import Namespace

Add to **_Imports.razor**:

```razor
@using Syncfusion.Blazor.Notifications
@using Syncfusion.Blazor.Buttons
```

## Basic Toast Implementation

### Simple Toast with Button

```razor
@page "/toast-basic"

<div class="col-lg-12 control-section toast-default-section">
    <SfButton @onclick="ShowToast">Show Toast</SfButton>
</div>

<SfToast @ref="ToastObj" Title="Notification" Content="Your message has been sent successfully!" />

@code {
    SfToast ToastObj;

    private async Task ShowToast()
    {
        await ToastObj.ShowAsync();
    }
}
```

**How it works:**
1. `@ref="ToastObj"` - Creates a reference to the toast instance
2. `ShowAsync()` - Displays the toast notification
3. Toast auto-hides after default timeout (5 seconds)

### Toast with Custom Content

```razor
@page "/toast-content"

<SfButton @onclick="@(() => ShowToastWithContent("Data saved successfully!"))">Save</SfButton>
<SfButton @onclick="@(() => ShowToastWithContent("Changes discarded"))">Cancel</SfButton>

<SfToast @ref="ToastObj" Title="Status Update" />

@code {
    SfToast ToastObj;

    private async Task ShowToastWithContent(string message)
    {
        ToastObj.Content = message;
        await ToastObj.ShowAsync();
    }
}
```

### Toast with Icon

```razor
@page "/toast-icon"

<SfButton @onclick="ShowSuccessToast">Success</SfButton>
<SfButton @onclick="ShowErrorToast">Error</SfButton>

<SfToast @ref="ToastObj" />

@code {
    SfToast ToastObj;

    private async Task ShowSuccessToast()
    {
        ToastObj.Title = "Success";
        ToastObj.Content = "Your profile has been updated";
        ToastObj.Icon = "e-success toast-icons";
        ToastObj.CssClass = "e-toast-success";
        await ToastObj.ShowAsync();
    }

    private async Task ShowErrorToast()
    {
        ToastObj.Title = "Error";
        ToastObj.Content = "Failed to connect to server";
        ToastObj.Icon = "e-error toast-icons";
        ToastObj.CssClass = "e-toast-danger";
        await ToastObj.ShowAsync();
    }
}

<style>
    .toast-icons {
        font-size: 20px;
    }
</style>
```

## Toast Position

The toast can be positioned in 9 predefined locations using the `Position` property.

### Position Examples

```razor
@page "/toast-position"

<div class="button-grid">
    <SfButton @onclick="@(() => ShowToastAt("Left", "Top"))">Top Left</SfButton>
    <SfButton @onclick="@(() => ShowToastAt("Center", "Top"))">Top Center</SfButton>
    <SfButton @onclick="@(() => ShowToastAt("Right", "Top"))">Top Right</SfButton>

    <SfButton @onclick="@(() => ShowToastAt("Left", "Center"))">Center Left</SfButton>
    <SfButton @onclick="@(() => ShowToastAt("Center", "Center"))">Center</SfButton>
    <SfButton @onclick="@(() => ShowToastAt("Right", "Center"))">Center Right</SfButton>

    <SfButton @onclick="@(() => ShowToastAt("Left", "Bottom"))">Bottom Left</SfButton>
    <SfButton @onclick="@(() => ShowToastAt("Center", "Bottom"))">Bottom Center</SfButton>
    <SfButton @onclick="@(() => ShowToastAt("Right", "Bottom"))">Bottom Right</SfButton>
</div>

<SfToast @ref="ToastObj"
         Title="Position Demo"
         Content="Toast notification">
    <ToastPosition X="@PositionX" Y="@PositionY"></ToastPosition>
</SfToast>

@code {
    private SfToast ToastObj;

    private string PositionX = "Center";
    private string PositionY = "Top";

    private async Task ShowToastAt(string x, string y)
    {
        PositionX = x;
        PositionY = y;

        await ToastObj.HideAsync();
        await ToastObj.ShowAsync();
    }
}

<style>
    .button-grid {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 10px;
        max-width: 600px;
    }
</style>
```

**Position values:**
- **X-axis:** `"Left"`, `"Center"`, `"Right"`, or custom pixel/percentage values
- **Y-axis:** `"Top"`, `"Center"`, `"Bottom"`, or custom pixel/percentage values

### Custom Position with Pixels

```razor
<SfToast @ref="ToastObj" 
         Title="Custom Position" 
         Content="Positioned at 100px from left, 50px from top">
    <ToastPosition X="100" Y="50"></ToastPosition>
</SfToast>
```

## Showing and Hiding Toasts

### Show Method

```razor
@code {
    // Show toast immediately
    await ToastObj.ShowAsync();
}
```

### Hide Method

```razor
@code {
    // Hide toast manually
    await ToastObj.HideAsync();
    
    // Hide all toasts
    await ToastObj.HideAsync("All");
}
```

### Hide Specific Toast

```razor
@code {
    // Each toast has a unique element
    await ToastObj.HideAsync(toastElement);
}
```

## Auto-Dismiss Timeout

Control how long the toast remains visible before auto-hiding.

```razor
<SfToast @ref="ToastObj" 
         Title="Timeout Demo" 
         Content="This message will disappear in 3 seconds"
         Timeout="3000" />

@code {
    // Timeout in milliseconds (default: 5000)
    // Set to 0 to disable auto-hide
}
```

**Common timeout values:**
- Quick messages: 2000-3000ms (2-3 seconds)
- Standard messages: 5000ms (5 seconds, default)
- Important messages: 7000-10000ms (7-10 seconds)
- Manual dismiss only: 0 (no auto-hide)

## Close Button

Enable users to manually dismiss the toast.

```razor
<SfToast @ref="ToastObj" 
         Title="Dismissible Toast" 
         Content="Click the X to close"
         ShowCloseButton="true" />
```

## Multiple Toasts

Display multiple toasts simultaneously.

```razor
@page "/multiple-toasts"

<SfButton @onclick="ShowMultipleToasts">Show 3 Toasts</SfButton>

<SfToast @ref="ToastObj" 
         NewestOnTop="true">
    <ToastPosition X="Right" Y="Bottom"></ToastPosition>
</SfToast>

@code {
    SfToast ToastObj;

    private async Task ShowMultipleToasts()
    {
        ToastObj.Content = "First notification";
        await ToastObj.ShowAsync();
        
        await Task.Delay(500);
        
        ToastObj.Content = "Second notification";
        await ToastObj.ShowAsync();
        
        await Task.Delay(500);
        
        ToastObj.Content = "Third notification";
        await ToastObj.ShowAsync();
    }
}
```

**NewestOnTop property:**
- `true` - Latest toast appears on top of stack (default)
- `false` - Latest toast appears at bottom of stack

## Complete Working Example

```razor
@page "/toast-complete"
@using Syncfusion.Blazor.Notifications
@using Syncfusion.Blazor.Buttons

<div class="toast-demo">
    <h3>Toast Notification Demo</h3>
    
    <div class="button-group">
        <SfButton CssClass="e-success" @onclick="ShowSuccess">Success</SfButton>
        <SfButton CssClass="e-info" @onclick="ShowInfo">Info</SfButton>
        <SfButton CssClass="e-warning" @onclick="ShowWarning">Warning</SfButton>
        <SfButton CssClass="e-danger" @onclick="ShowError">Error</SfButton>
    </div>
</div>

<SfToast @ref="ToastObj" 
         ShowCloseButton="true"
         Timeout="5000">
    <ToastPosition X="Right" Y="Top"></ToastPosition>
</SfToast>

@code {
    SfToast ToastObj;

    private async Task ShowSuccess()
    {
        ToastObj.Title = "Success";
        ToastObj.Content = "Your changes have been saved successfully!";
        ToastObj.Icon = "e-success toast-icon";
        ToastObj.CssClass = "e-toast-success";
        await ToastObj.ShowAsync();
    }

    private async Task ShowInfo()
    {
        ToastObj.Title = "Information";
        ToastObj.Content = "You have 3 new messages in your inbox.";
        ToastObj.Icon = "e-info toast-icon";
        ToastObj.CssClass = "e-toast-info";
        await ToastObj.ShowAsync();
    }

    private async Task ShowWarning()
    {
        ToastObj.Title = "Warning";
        ToastObj.Content = "Your session will expire in 5 minutes.";
        ToastObj.Icon = "e-warning toast-icon";
        ToastObj.CssClass = "e-toast-warning";
        await ToastObj.ShowAsync();
    }

    private async Task ShowError()
    {
        ToastObj.Title = "Error";
        ToastObj.Content = "Failed to connect to the server. Please try again.";
        ToastObj.Icon = "e-error toast-icon";
        ToastObj.CssClass = "e-toast-danger";
        await ToastObj.ShowAsync();
    }
}

<style>
    .toast-demo {
        padding: 20px;
    }

    .button-group {
        display: flex;
        gap: 10px;
        margin-top: 15px;
    }

    .toast-icon {
        font-size: 18px;
    }
</style>
```

## Common Patterns

### Form Submission Feedback

```razor
<EditForm Model="@model" OnValidSubmit="HandleValidSubmit">
    <!-- Form fields -->
    <SfButton Type="ButtonType.Submit">Submit</SfButton>
</EditForm>

<SfToast @ref="ToastObj">
    <ToastPosition X="Right" Y="Top"></ToastPosition>
</SfToast>

@code {
    SfToast ToastObj;
    private FormModel model = new();

    private async Task HandleValidSubmit()
    {
        try
        {
            await SaveData();
            ToastObj.Title = "Success";
            ToastObj.Content = "Form submitted successfully!";
            ToastObj.CssClass = "e-toast-success";
            await ToastObj.ShowAsync();
        }
        catch (Exception ex)
        {
            ToastObj.Title = "Error";
            ToastObj.Content = $"Submission failed: {ex.Message}";
            ToastObj.CssClass = "e-toast-danger";
            await ToastObj.ShowAsync();
        }
    }
}
```

### API Call Feedback

```razor
@code {
    private async Task LoadData()
    {
        try
        {
            var data = await Http.GetFromJsonAsync<Data[]>("api/data");
            
            ToastObj.Title = "Data Loaded";
            ToastObj.Content = $"Successfully loaded {data.Length} items";
            ToastObj.CssClass = "e-toast-info";
            ToastObj.Timeout = 3000;
            await ToastObj.ShowAsync();
        }
        catch
        {
            ToastObj.Title = "Load Failed";
            ToastObj.Content = "Unable to load data. Please refresh the page.";
            ToastObj.CssClass = "e-toast-danger";
            ToastObj.ShowCloseButton = true;
            ToastObj.Timeout = 0; // Manual dismiss only
            await ToastObj.ShowAsync();
        }
    }
}
```

## Troubleshooting

### Toast Not Appearing

**Problem:** Called `ShowAsync()` but toast doesn't appear.

**Solutions:**
- Ensure `@ref="ToastObj"` is set on the component
- Check that the component is initialized (not null)
- Verify Title or Content has a value
- Check if toast is positioned outside viewport
- Ensure theme CSS is loaded

### Toast Appears Behind Other Elements

**Problem:** Toast is hidden behind modal dialogs or other components.

**Solution:** Increase z-index with custom CSS:
```css
.custom-toast {
    z-index: 10000 !important;
}
```

```razor
<SfToast @ref="ToastObj" CssClass="custom-toast" />
```

### Multiple Toasts Not Stacking

**Problem:** New toasts replace old ones instead of stacking.

**Solution:** Ensure each toast call has different content or use slight delays:
```csharp
await Task.Delay(100); // Small delay between toast calls
```

## Next Steps

- Learn about [Advanced Toast Features](toast-features.md) including animations, templates, and action buttons
- Explore [Styling and Themes](styling-and-themes.md) for customization options
- See [Use Cases and Examples](use-cases-and-examples.md) for real-world scenarios
