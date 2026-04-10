# Button Component - Features

## Table of Contents
- [Getting Started](#getting-started)
- [Installation](#installation)
- [Basic Button Implementation](#basic-button-implementation)
- [Button Types](#button-types)
- [Button Styles](#button-styles)
- [Icons and Icon Positioning](#icons-and-icon-positioning)
- [Button Sizes](#button-sizes)
- [Repeat Button](#repeat-button)
- [Tooltips](#tooltips)
- [HTML Attribute Support](#html-attribute-support)
- [Native Event Handling](#native-event-handling)
- [Common Scenarios](#common-scenarios)
- [Troubleshooting](#troubleshooting)

---

## Getting Started

The Blazor Button component provides a standard clickable button with extensive customization options including types, styles, icons, sizes, and states.

### Prerequisites
- .NET SDK 6.0 or later
- Blazor WebAssembly or Blazor Server application
- Visual Studio 2022 or VS Code

---

## Installation

### Step 1: Install NuGet Packages

```bash
dotnet add package Syncfusion.Blazor.Buttons
dotnet add package Syncfusion.Blazor.Themes
```

Or via Package Manager Console:
```powershell
Install-Package Syncfusion.Blazor.Buttons
Install-Package Syncfusion.Blazor.Themes
```

### Step 2: Add Namespaces

Open `~/_Imports.razor` and add:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Buttons
```

### Step 3: Register Syncfusion Service

In `~/Program.cs`:

```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { 
    BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) 
});

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

### Step 4: Add Theme and Script References

In `~/index.html` (WebAssembly) or `~/Pages/_Host.cshtml` (Server):

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" 
            type="text/javascript"></script>
</head>
```

---

## API Reference

### SfButton Properties

**Content Properties:**
- `Content` (string) - Button text content
- `ChildContent` (RenderFragment) - Child content for custom markup

**Icon Properties:**
- `IconCss` (string) - CSS class for button icon
- `IconPosition` (IconPosition enum) - Icon position: Left (default), Right, Top, Bottom

**Appearance Properties:**
- `CssClass` (string) - Custom CSS classes for styling
- `IsPrimary` (bool) - Apply primary button styling
- `IsToggle` (bool) - Enable toggle button behavior

**State Properties:**
- `Disabled` (bool) - Disable the button
- `EnableRtl` (bool) - Enable right-to-left rendering

**Additional Properties:**
- `HtmlAttributes` (Dictionary<string, object>) - Custom HTML attributes

**Events:**
- `OnClick` (EventCallback<MouseEventArgs>) - Click event handler
- `Created` (EventCallback<Object>) - Component created event

### IconPosition Enum Values
- `Left` - Icon on the left (default)
- `Right` - Icon on the right
- `Top` - Icon on top
- `Bottom` - Icon on bottom

### Common CSS Classes
- `e-primary` - Primary button style
- `e-success` - Success/green style
- `e-info` - Info/blue style
- `e-warning` - Warning/orange style
- `e-danger` - Danger/red style
- `e-link` - Link style
- `e-flat` - Flat style (no background)
- `e-outline` - Outline style
- `e-round` - Round/circular style
- `e-small` - Small size
- `e-block` - Full width block button

## Basic Button Implementation

### Simple Button

```razor
<SfButton>Click Me</SfButton>
```

### Button with Click Event

```razor
<SfButton OnClick="HandleClick">Submit</SfButton>

@code {
    private void HandleClick()
    {
        Console.WriteLine("Button clicked!");
    }
}
```

### Button with Two-Way Binding

```razor
<SfButton @onclick="IncrementCount">
    Count: @currentCount
</SfButton>

@code {
    private int currentCount = 0;

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

---

## Button Types

The Button component supports multiple visual types through the `CssClass` property.

### Flat Button
Button with no background color:

```razor
<SfButton CssClass="e-flat">Flat Button</SfButton>
```

### Outline Button
Button with border and transparent background:

```razor
<SfButton CssClass="e-outline">Outline Button</SfButton>
```

### Round Button
Circular button (usually contains only an icon):

```razor
<SfButton CssClass="e-round" IconCss="e-icons e-plus-icon"></SfButton>

<style>
    .e-plus-icon::before {
        content: '\e823';
    }
</style>
```

### Primary Button
Button with prominent background (using `IsPrimary` property):

```razor
<SfButton IsPrimary="true">Primary Button</SfButton>
```

### Toggle Button
Button that maintains an active/inactive state:

```razor
<SfButton CssClass="e-flat" 
          IsPrimary="true" 
          IconCss="@IconCss" 
          Content="@Content" 
          IsToggle="true" 
          @onclick="OnToggleClick" 
          @ref="ToggleBtnObj">
</SfButton>

@code {
    SfButton ToggleBtnObj;
    public string IconCss = "e-icons e-play";
    public string Content = "Play";
    
    public void OnToggleClick()
    {
        if(ToggleBtnObj.Content == "Play")
        {
            this.Content = "Pause";
            this.IconCss = "e-icons e-pause";
        }
        else
        {
            this.Content = "Play";
            this.IconCss = "e-icons e-play";
        }
    }
}

<style>
    .e-play::before { content: '\e324'; }
    .e-pause::before { content: '\e326'; }
</style>
```

### Multiple Types Example

```razor
<div class="button-group">
    <SfButton>Normal Button</SfButton>
    <SfButton IsPrimary="true">Primary</SfButton>
    <SfButton CssClass="e-flat">Flat</SfButton>
    <SfButton CssClass="e-outline">Outline</SfButton>
    <SfButton CssClass="e-round" IconCss="e-icons e-add-icon"></SfButton>
</div>

<style>
    .button-group { display: flex; gap: 10px; }
    .e-add-icon::before { content: '\e823'; }
</style>
```

---

## Button Styles

Predefined styles provide semantic meaning through the `CssClass` property.

### Available Style Classes

| Class | Purpose | Visual |
|-------|---------|--------|
| `e-primary` | Primary action | Blue background |
| `e-success` | Positive action | Green background |
| `e-info` | Informational action | Light blue background |
| `e-warning` | Caution action | Orange background |
| `e-danger` | Negative action | Red background |
| `e-link` | Hyperlink style | No background, colored text |

### Style Examples

```razor
<SfButton CssClass="e-primary">Primary</SfButton>
<SfButton CssClass="e-success">Success</SfButton>
<SfButton CssClass="e-info">Info</SfButton>
<SfButton CssClass="e-warning">Warning</SfButton>
<SfButton CssClass="e-danger">Danger</SfButton>
<SfButton CssClass="e-link">Link</SfButton>
```

### Combining Styles with Types

```razor
<!-- Outline Primary Button -->
<SfButton CssClass="e-primary e-outline">Primary Outline</SfButton>

<!-- Flat Success Button -->
<SfButton CssClass="e-success e-flat">Success Flat</SfButton>

<!-- Round Danger Button -->
<SfButton CssClass="e-danger e-round" IconCss="e-icons e-delete-icon"></SfButton>
```

### Complete Style Grid Example

```razor
<div class="style-grid">
    <div class="row">
        <SfButton CssClass="e-primary">Primary</SfButton>
        <SfButton CssClass="e-primary e-outline">Primary Outline</SfButton>
        <SfButton CssClass="e-primary e-flat">Primary Flat</SfButton>
    </div>
    <div class="row">
        <SfButton CssClass="e-success">Success</SfButton>
        <SfButton CssClass="e-success e-outline">Success Outline</SfButton>
        <SfButton CssClass="e-success e-flat">Success Flat</SfButton>
    </div>
    <div class="row">
        <SfButton CssClass="e-danger">Danger</SfButton>
        <SfButton CssClass="e-danger e-outline">Danger Outline</SfButton>
        <SfButton CssClass="e-danger e-flat">Danger Flat</SfButton>
    </div>
</div>

<style>
    .style-grid { display: flex; flex-direction: column; gap: 15px; }
    .row { display: flex; gap: 10px; }
</style>
```

**Important:** Style classes provide only visual indication. Always define meaningful content for assistive technologies.

---

## Icons and Icon Positioning

### Button with Icon

```razor
<SfButton IconCss="e-icons e-save-icon">Save</SfButton>

<style>
    .e-save-icon::before {
        content: '\e74e';  /* Syncfusion icon unicode */
    }
</style>
```

### Icon Position

Use the `IconPosition` property to control icon placement:

```razor
<!-- Icon on Left (default) -->
<SfButton IconCss="e-icons e-play-icon">Play</SfButton>

<!-- Icon on Right -->
<SfButton IconCss="e-icons e-play-icon" IconPosition="IconPosition.Right">
    Play
</SfButton>

<!-- Icon on Top -->
<SfButton IconCss="e-icons e-upload-icon" IconPosition="IconPosition.Top">
    Upload
</SfButton>

<!-- Icon on Bottom -->
<SfButton IconCss="e-icons e-download-icon" IconPosition="IconPosition.Bottom">
    Download
</SfButton>

<style>
    .e-play-icon::before { content: '\e324'; }
    .e-upload-icon::before { content: '\e725'; }
    .e-download-icon::before { content: '\e736'; }
</style>
```

### Icon-Only Button

```razor
<!-- Round icon button -->
<SfButton CssClass="e-round e-primary" 
          IconCss="e-icons e-add-icon">
</SfButton>

<!-- Small icon button -->
<SfButton CssClass="e-small" 
          IconCss="e-icons e-edit-icon">
</SfButton>
```

### Using External Icon Libraries

#### Font Awesome
```razor
<SfButton IconCss="fas fa-save">Save</SfButton>
```

#### Material Icons
```razor
<SfButton IconCss="material-icons">save</SfButton>
```

### Custom Icon Styling

```razor
<SfButton IconCss="custom-icon" CssClass="e-primary">
    Custom Icon
</SfButton>

<style>
    .custom-icon {
        background: url('images/custom-icon.png') no-repeat center;
        width: 16px;
        height: 16px;
        display: inline-block;
    }
</style>
```

---

## Button Sizes

### Available Sizes

```razor
<!-- Small Button -->
<SfButton CssClass="e-small">Small Button</SfButton>

<!-- Normal Button (default) -->
<SfButton>Normal Button</SfButton>

<!-- Block Button (full width) -->
<SfButton CssClass="e-block">Block Button</SfButton>
```

### Size Comparison

```razor
<div class="size-demo">
    <SfButton CssClass="e-small e-primary">Small</SfButton>
    <SfButton CssClass="e-primary">Normal</SfButton>
</div>

<div style="margin-top: 20px;">
    <SfButton CssClass="e-block e-primary">Block Button</SfButton>
</div>

<style>
    .size-demo { display: flex; gap: 10px; align-items: center; }
</style>
```

### Custom Size with CSS

```razor
<SfButton CssClass="custom-large">Large Custom Button</SfButton>

<style>
    .custom-large.e-btn {
        padding: 12px 24px;
        font-size: 18px;
        min-height: 48px;
    }
</style>
```

---

## Custom Repeat Button Implementation

**Note:** SfButton does not have built-in repeat button functionality. Here's how to implement it with custom logic:

### Repeat Button with Interval Control

```razor
<SfButton @ref="RepeatBtn" 
          @onmousedown="StartRepeat" 
          @onmouseup="StopRepeat" 
          @onmouseleave="StopRepeat">
    Count: @repeatCount
</SfButton>

@code {
    SfButton RepeatBtn;
    private int repeatCount = 0;
    private System.Threading.Timer timer;

    private void StartRepeat()
    {
        repeatCount = 0;
        if (timer == null)
        {
            timer = new System.Threading.Timer(_ => 
            {
                repeatCount++;
                InvokeAsync(StateHasChanged);
            }, null, 0, 100); // Trigger every 100ms
        }
    }

    private void StopRepeat()
    {
        timer?.Dispose();
        timer = null;
    }

    public void Dispose()
    {
        timer?.Dispose();
    }
}
```

**Implementation Notes:**
- Use `@onmousedown` to start the timer
- Use `@onmouseup` and `@onmouseleave` to stop the timer
- This is a custom implementation, not a built-in feature
- Remember to dispose the timer to prevent memory leaks

---

## Tooltips

### Using HTML Title Attribute

```razor
<SfButton title="Click to submit the form">Submit</SfButton>
```

### Using HtmlAttributes Property

```razor
<SfButton HtmlAttributes="@buttonAttributes">
    Delete
</SfButton>

@code {
    private Dictionary<string, object> buttonAttributes = new Dictionary<string, object>
    {
        { "title", "Permanently delete this item" }
    };
}
```

### Integration with Syncfusion Tooltip Component

```razor
<div id="tooltip-target">
    <SfButton CssClass="e-danger">Delete</SfButton>
</div>

<SfTooltip Target="#tooltip-target" Content="This action cannot be undone">
</SfTooltip>

@code {
    @using Syncfusion.Blazor.Popups
}
```

---

## HTML Attribute Support

The Button component supports HTML attributes through the `HtmlAttributes` property.

### Adding Custom Attributes

```razor
<SfButton HtmlAttributes="@attributes">
    Custom Attributes
</SfButton>

@code {
    private Dictionary<string, object> attributes = new Dictionary<string, object>
    {
        { "data-id", "btn-123" },
        { "aria-label", "Submit form" },
        { "title", "Click to submit" },
        { "data-action", "submit" }
    };
}
```

### Form-Related Attributes

```razor
<EditForm Model="@model" OnValidSubmit="HandleSubmit">
    <SfButton HtmlAttributes="@submitAttributes" CssClass="e-primary">
        Submit
    </SfButton>
</EditForm>

@code {
    private FormModel model = new FormModel();

    private Dictionary<string, object> submitAttributes = new Dictionary<string, object>
    {
        { "type", "submit" },
        { "form", "myForm" },
        { "name", "submitBtn" }
    };

    private void HandleSubmit()
    {
        // Handle form submission
    }

    public class FormModel { }
}
```

### Accessibility Attributes

```razor
<SfButton HtmlAttributes="@accessibilityAttributes">
    Accessible Button
</SfButton>

@code {
    private Dictionary<string, object> accessibilityAttributes = new Dictionary<string, object>
    {
        { "aria-label", "Submit form" },
        { "aria-describedby", "btn-description" },
        { "role", "button" },
        { "tabindex", "0" }
    };
}
```

---

## Native Event Handling

### Mouse Events

```razor
<SfButton @onclick="OnClick"
          @onmouseover="OnMouseOver"
          @onmouseout="OnMouseOut"
          @onmousedown="OnMouseDown"
          @onmouseup="OnMouseUp">
    Interactive Button
</SfButton>

@code {
    private void OnClick() => Console.WriteLine("Clicked");
    private void OnMouseOver() => Console.WriteLine("Mouse over");
    private void OnMouseOut() => Console.WriteLine("Mouse out");
    private void OnMouseDown() => Console.WriteLine("Mouse down");
    private void OnMouseUp() => Console.WriteLine("Mouse up");
}
```

### Keyboard Events

```razor
<SfButton @onkeydown="OnKeyDown"
          @onkeyup="OnKeyUp"
          @onkeypress="OnKeyPress">
    Keyboard Events
</SfButton>

@code {
    private void OnKeyDown(KeyboardEventArgs e)
    {
        Console.WriteLine($"Key down: {e.Key}");
    }

    private void OnKeyUp(KeyboardEventArgs e)
    {
        Console.WriteLine($"Key up: {e.Key}");
    }

    private void OnKeyPress(KeyboardEventArgs e)
    {
        Console.WriteLine($"Key press: {e.Key}");
    }
}
```

### Focus Events

```razor
<SfButton @onfocus="OnFocus"
          @onblur="OnBlur">
    Focus Events
</SfButton>

@code {
    private void OnFocus() => Console.WriteLine("Button focused");
    private void OnBlur() => Console.WriteLine("Button blurred");
}
```

### Event Arguments

```razor
<SfButton OnClick="HandleClickWithArgs">Click Me</SfButton>

@code {
    private void HandleClickWithArgs(MouseEventArgs e)
    {
        Console.WriteLine($"Button: {e.Button}");
        Console.WriteLine($"X: {e.ClientX}, Y: {e.ClientY}");
        Console.WriteLine($"Ctrl: {e.CtrlKey}, Shift: {e.ShiftKey}");
    }
}
```

---

## Common Scenarios

### Submit Button in Form

```razor
<EditForm Model="@formData" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <div class="form-group">
        <label>Name:</label>
        <InputText @bind-Value="formData.Name" class="form-control" />
    </div>

    <SfButton CssClass="e-primary" HtmlAttributes="@submitAttrs">
        Submit
    </SfButton>
</EditForm>

@code {
    private FormData formData = new FormData();
    private Dictionary<string, object> submitAttrs = new() { { "type", "submit" } };

    private void HandleValidSubmit()
    {
        Console.WriteLine($"Form submitted: {formData.Name}");
    }

    public class FormData
    {
        [Required]
        public string Name { get; set; }
    }
}
```

### Async Button Operation

```razor
<SfButton OnClick="HandleAsyncOperation" Disabled="@isLoading">
    @(isLoading ? "Loading..." : "Load Data")
</SfButton>

@if (data != null)
{
    <p>Data: @data</p>
}

@code {
    private bool isLoading = false;
    private string data = null;

    private async Task HandleAsyncOperation()
    {
        isLoading = true;
        try
        {
            data = await FetchDataAsync();
        }
        finally
        {
            isLoading = false;
        }
    }

    private async Task<string> FetchDataAsync()
    {
        await Task.Delay(2000); // Simulate API call
        return "Fetched data";
    }
}
```

### Conditional Button Rendering

```razor
@if (showPrimaryAction)
{
    <SfButton CssClass="e-primary" OnClick="HandlePrimaryAction">
        Primary Action
    </SfButton>
}
else
{
    <SfButton CssClass="e-secondary" OnClick="HandleSecondaryAction">
        Secondary Action
    </SfButton>
}

@code {
    private bool showPrimaryAction = true;

    private void HandlePrimaryAction() => showPrimaryAction = false;
    private void HandleSecondaryAction() => showPrimaryAction = true;
}
```

---

## Troubleshooting

### Button Not Responding to Clicks

**Problem:** Button click event not firing.

**Solutions:**
1. Ensure `OnClick` is properly bound
2. Check if button is disabled
3. Verify no CSS z-index issues
4. Check JavaScript console for errors

```razor
<!-- Correct -->
<SfButton OnClick="HandleClick">Click Me</SfButton>

<!-- Incorrect - missing @ symbol -->
<SfButton OnClick="HandleClick">Click Me</SfButton>
```

### Icons Not Displaying

**Problem:** Icons show as empty boxes or don't appear.

**Solutions:**
1. Verify icon CSS is defined
2. Check Syncfusion script reference
3. Ensure icon unicode is correct
4. Verify CSS class names

```razor
<!-- Correct icon definition -->
<SfButton IconCss="e-icons e-custom-icon">Button</SfButton>

<style>
    .e-custom-icon::before {
        content: '\e823';  /* Valid unicode */
    }
</style>
```

### Styling Not Applied

**Problem:** Custom CSS classes not working.

**Solutions:**
1. Use `::deep` or `:global` for style penetration
2. Increase CSS specificity
3. Verify CssClass property syntax

```css
/* Correct - using ::deep */
::deep .custom-button.e-btn {
    background-color: purple;
}

/* Correct - global scope */
:global(.custom-button.e-btn) {
    background-color: purple;
}
```

### Button Not Showing in Form

**Problem:** Button submits form unintentionally.

**Solution:** Add `type="button"` attribute:

```razor
<SfButton HtmlAttributes="@buttonAttrs" OnClick="HandleClick">
    Don't Submit
</SfButton>

@code {
    private Dictionary<string, object> buttonAttrs = new() 
    { 
        { "type", "button" } 
    };
}
```

### Performance Issues with Many Buttons

**Problem:** Page slow with numerous buttons.

**Solutions:**
1. Use virtualization for lists
2. Implement lazy loading
3. Optimize event handlers
4. Consider ButtonGroup for related actions

---

## Best Practices

1. **Use semantic styles** - Choose button styles that match action intent (e-primary for main actions, e-danger for destructive actions)

2. **Provide clear labels** - Use descriptive text instead of generic labels like "Click Here"

3. **Add icons for clarity** - Icons help users quickly identify button purpose

4. **Handle loading states** - Disable buttons during async operations and show loading indicators

5. **Implement keyboard support** - Ensure buttons are keyboard accessible (they are by default)

6. **Use appropriate sizes** - Small buttons for dense UIs, block buttons for mobile

7. **Avoid overusing primary style** - Only one primary action per section

8. **Test accessibility** - Verify with screen readers and keyboard navigation

9. **Provide visual feedback** - Use `:hover`, `:active`, and `:focus` states effectively

10. **Consider mobile touch targets** - Ensure buttons are at least 44x44px on mobile

---

## Next Steps

- Explore [button-styling.md](button-styling.md) for advanced appearance customization
- Learn about [button-advanced-features.md](button-advanced-features.md) for accessibility and optimization
