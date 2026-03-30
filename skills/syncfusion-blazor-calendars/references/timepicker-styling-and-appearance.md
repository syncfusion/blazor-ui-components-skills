# Styling and Appearance in Blazor TimePicker

## Table of Contents
- [CSS Customization Overview](#css-customization-overview)
- [Container Element Styling](#container-element-styling)
- [Icon Element Styling](#icon-element-styling)
- [Popup and List Styling](#popup-and-list-styling)
- [Full-Screen Mode on Mobile](#full-screen-mode-on-mobile)
- [Theme Integration](#theme-integration)
- [Custom Styling Examples](#custom-styling-examples)
- [Common Styling Issues](#common-styling-issues)

## CSS Customization Overview

The TimePicker component uses standard CSS classes that can be overridden. All styling is applied through CSS class selectors targeting internal HTML structure.

### CSS Class Structure

```
.e-time-wrapper                 - Main wrapper
  └── .e-input-group            - Input field container
      ├── input.e-input         - Text input field
      └── .e-time-icon          - Calendar icon
  └── .e-timepicker.e-popup     - Dropdown popup
      └── .e-timepicker-list    - Time list
          └── li.e-list-item    - Individual time items
```

## Container Element Styling

Customize the input container appearance (height, font size, padding, etc.).

### Basic Container Styling

```css
/* Increase input field height and font size */
.e-input-group input.e-input,
.e-input-group.e-control-wrapper input.e-input {
    height: 45px;
    font-size: 16px;
    padding: 10px;
}
```

### Container in Razor Component

```razor
@using Syncfusion.Blazor.Calendars

<style>
    .custom-timepicker .e-input-group input.e-input {
        height: 50px;
        font-size: 18px;
        border-radius: 8px;
        border: 2px solid #007bff;
        padding: 12px;
    }
    
    .custom-timepicker .e-input-group input.e-input:focus {
        border-color: #0056b3;
        box-shadow: 0 0 5px rgba(0, 86, 179, 0.3);
    }
</style>

<SfTimePicker TValue="DateTime?" CssClass="custom-timepicker"></SfTimePicker>
```

### Container Variations

```razor
@* Large input *@
<style>
    .large-timepicker .e-input-group input.e-input {
        height: 60px;
        font-size: 20px;
    }
</style>
<SfTimePicker TValue="DateTime?" CssClass="large-timepicker"></SfTimePicker>

@* Compact input *@
<style>
    .compact-timepicker .e-input-group input.e-input {
        height: 32px;
        font-size: 12px;
    }
</style>
<SfTimePicker TValue="DateTime?" CssClass="compact-timepicker"></SfTimePicker>

@* Rounded corners *@
<style>
    .rounded-timepicker .e-input-group input.e-input {
        border-radius: 20px;
        height: 44px;
    }
</style>
<SfTimePicker TValue="DateTime?" CssClass="rounded-timepicker"></SfTimePicker>
```

## Icon Element Styling

Customize the calendar icon appearance (color, size, background).

### Basic Icon Styling

```css
/* Customize the time icon */
.e-time-wrapper .e-time-icon.e-icons,
.e-time-wrapper .e-control-wrapper .e-time-icon.e-icons {
    font-size: 20px;
    color: #007bff;
    background-color: transparent;
}
```

### Icon in Razor Component

```razor
@using Syncfusion.Blazor.Calendars

<style>
    .custom-icon .e-time-icon.e-icons {
        font-size: 24px;
        color: #ff6b6b;
        background-color: #f0f0f0;
        padding: 5px;
        border-radius: 4px;
    }
</style>

<SfTimePicker TValue="DateTime?" CssClass="custom-icon"></SfTimePicker>
```

### Icon Color Variations

```razor
@* Blue icon *@
<style>
    .blue-icon .e-time-icon { color: #007bff; }
</style>
<SfTimePicker TValue="DateTime?" CssClass="blue-icon"></SfTimePicker>

@* Green icon (success) *@
<style>
    .green-icon .e-time-icon { color: #28a745; }
</style>
<SfTimePicker TValue="DateTime?" CssClass="green-icon"></SfTimePicker>

@* Red icon (error/required) *@
<style>
    .red-icon .e-time-icon { color: #dc3545; }
</style>
<SfTimePicker TValue="DateTime?" CssClass="red-icon"></SfTimePicker>
```

## Popup and List Styling

Customize the dropdown popup appearance (colors, sizes, hover effects).

### Popup Styling

```css
/* Set popup height and background */
.e-timepicker.e-popup {
    height: 200px;
    background-color: #ffffff;
    border-radius: 8px;
}
```

### List Item Styling

```css
/* Style individual time items in the list */
.e-timepicker.e-popup .e-list-parent.e-ul li.e-list-item {
    background-color: #ffffff;
    color: #333333;
    font-size: 14px;
    padding: 10px;
}

/* Hover effect on list items */
.e-timepicker.e-popup .e-list-parent.e-ul li.e-list-item:hover {
    background-color: #e7f3ff;
    color: #007bff;
}

/* Selected/focused item */
.e-timepicker.e-popup .e-list-parent.e-ul li.e-list-item.e-active {
    background-color: #007bff;
    color: #ffffff;
}
```

### Complete Popup Customization Example

```razor
@using Syncfusion.Blazor.Calendars

<style>
    .styled-popup .e-timepicker.e-popup {
        height: 250px;
        background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
        border: 2px solid #007bff;
        border-radius: 12px;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }

    .styled-popup .e-list-parent.e-ul li.e-list-item {
        padding: 12px;
        border-bottom: 1px solid #e0e0e0;
        transition: all 0.2s ease;
    }

    .styled-popup .e-list-parent.e-ul li.e-list-item:hover {
        background-color: rgba(0, 123, 255, 0.1);
        padding-left: 16px;
        cursor: pointer;
    }

    .styled-popup .e-list-parent.e-ul li.e-list-item.e-active {
        background-color: #007bff;
        color: white;
        border-radius: 4px;
        margin: 4px 2px;
    }
</style>

<SfTimePicker TValue="DateTime?" CssClass="styled-popup"></SfTimePicker>
```

## Full-Screen Mode on Mobile

The `FullScreen` property enables full-screen popup mode on mobile devices for better usability.

### Enable Full-Screen Mode

```razor
@using Syncfusion.Blazor.Calendars

<SfTimePicker TValue="DateTime?" FullScreen=true></SfTimePicker>
```

### Conditional Full-Screen Based on Device

```razor
@using Syncfusion.Blazor.Calendars
@inject IJSRuntime JsRuntime

<SfTimePicker TValue="DateTime?" FullScreen="@IsMobileDevice"></SfTimePicker>

@code {
    private bool IsMobileDevice { get; set; }

    protected override async Task OnInitializedAsync()
    {
        // Check if viewport is mobile-sized
        IsMobileDevice = await JsRuntime.InvokeAsync<bool>("getIsMobile");
    }
}

@* JavaScript in app.js *@
<script>
    window.getIsMobile = function() {
        return window.innerWidth <= 768;
    };
</script>
```

### Full-Screen Styling

```css
/* Full-screen popup styling */
.e-timepicker.e-popup.e-fullscreen {
    height: 100vh;
    width: 100vw;
    max-width: 100%;
}

.e-timepicker.e-popup.e-fullscreen .e-list-parent.e-ul {
    height: 100%;
}

.e-timepicker.e-popup.e-fullscreen li.e-list-item {
    height: 60px;
    line-height: 60px;
    font-size: 16px;
}
```

## Theme Integration

Apply Syncfusion themes or create custom themes.

### Built-In Themes

Syncfusion TimePicker works with any Syncfusion theme:

```html
<!-- Bootstrap 5 -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Material Design -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Fluent UI -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />

<!-- Tailwind CSS -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />

<!-- Fabric/Office -->
<link href="_content/Syncfusion.Blazor.Themes/fabric.css" rel="stylesheet" />
```

### Dark Mode

Apply dark theme classes:

```razor
@using Syncfusion.Blazor.Calendars

<style>
    .dark-theme.e-time-wrapper {
        background-color: #1e1e1e;
        color: #ffffff;
    }

    .dark-theme .e-input-group input.e-input {
        background-color: #2d2d2d;
        color: #ffffff;
        border-color: #444444;
    }

    .dark-theme .e-timepicker.e-popup {
        background-color: #2d2d2d;
        color: #ffffff;
    }

    .dark-theme li.e-list-item:hover {
        background-color: #3d3d3d;
    }

    .dark-theme li.e-list-item.e-active {
        background-color: #0d7377;
    }
</style>

<SfTimePicker TValue="DateTime?" CssClass="dark-theme"></SfTimePicker>
```

## Custom Styling Examples

### Example 1: Professional Business Style

```razor
@using Syncfusion.Blazor.Calendars

<style>
    .professional {
        font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    }

    .professional .e-input-group input.e-input {
        height: 44px;
        border: 1px solid #cccccc;
        border-radius: 6px;
        font-size: 14px;
        background-color: #f9f9f9;
    }

    .professional .e-time-icon {
        color: #666666;
    }

    .professional .e-timepicker.e-popup {
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.12);
    }

    .professional li.e-list-item:hover {
        background-color: #f0f0f0;
    }

    .professional li.e-list-item.e-active {
        background-color: #0066cc;
        color: white;
    }
</style>

<SfTimePicker TValue="DateTime?" CssClass="professional"></SfTimePicker>
```

### Example 2: Modern Minimal Style

```razor
@using Syncfusion.Blazor.Calendars

<style>
    .minimal {
        --primary: #3b82f6;
        --hover: #eff6ff;
        --active: #1e40af;
    }

    .minimal .e-input-group {
        border: none;
        border-bottom: 2px solid #e5e7eb;
        padding-bottom: 8px;
    }

    .minimal .e-input-group input.e-input {
        background: none;
        border: none;
        font-size: 16px;
    }

    .minimal .e-input-group:focus-within {
        border-bottom-color: var(--primary);
    }

    .minimal .e-timepicker.e-popup {
        border: 1px solid #e5e7eb;
        box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08);
    }

    .minimal li.e-list-item {
        padding: 8px 12px;
    }

    .minimal li.e-list-item:hover {
        background-color: var(--hover);
    }

    .minimal li.e-list-item.e-active {
        background-color: var(--primary);
        color: white;
    }
</style>

<SfTimePicker TValue="DateTime?" CssClass="minimal"></SfTimePicker>
```

### Example 3: Vibrant/Colorful Style

```razor
@using Syncfusion.Blazor.Calendars

<style>
    .vibrant {
        --primary: #6366f1;
        --accent: #ec4899;
    }

    .vibrant .e-input-group input.e-input {
        height: 48px;
        background: linear-gradient(135deg, #ffffff 0%, #f3f4f6 100%);
        border: 2px solid var(--primary);
        border-radius: 12px;
        font-size: 16px;
        font-weight: 500;
    }

    .vibrant .e-time-icon {
        color: var(--accent);
        font-size: 22px;
    }

    .vibrant .e-timepicker.e-popup {
        border-radius: 12px;
        background: white;
        box-shadow: 0 10px 25px rgba(99, 102, 241, 0.15);
    }

    .vibrant li.e-list-item {
        padding: 14px 12px;
        border-left: 4px solid transparent;
        transition: all 0.3s ease;
    }

    .vibrant li.e-list-item:hover {
        background-color: #f8f9ff;
        border-left-color: var(--accent);
        padding-left: 16px;
    }

    .vibrant li.e-list-item.e-active {
        background: linear-gradient(135deg, var(--primary), var(--accent));
        color: white;
        border-left-color: white;
    }
</style>

<SfTimePicker TValue="DateTime?" CssClass="vibrant"></SfTimePicker>
```

## Common Styling Issues

### Issue 1: Styling Not Applied

**Problem**: CSS changes don't affect the TimePicker.

**Solution**: Ensure CSS specificity is correct:

```css
/* May not work - too low specificity */
.e-input-group input {
    font-size: 20px;
}

/* Better - higher specificity */
.e-time-wrapper .e-input-group input.e-input {
    font-size: 20px;
}

/* Best - use CssClass parameter */
.custom-picker .e-input-group input.e-input {
    font-size: 20px;
}
```

### Issue 2: Theme Conflicts

**Problem**: Multiple themes loading, causing conflicts.

**Solution**: Use only one theme in your app:

```html
<!-- ❌ WRONG - Multiple themes conflict -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- ✅ CORRECT - Single theme -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

### Issue 3: Popup Hidden Behind Other Elements

**Problem**: Dropdown popup appears behind modals or other elements.

**Solution**: Adjust z-index:

```css
.e-timepicker.e-popup {
    z-index: 9999 !important;
}
```

## See Also

- [Accessibility and Globalization →](../accessibility-and-globalization.md)
- [Getting Started →](../getting-started.md)
- [Events and Handlers →](../events-and-handlers.md)
