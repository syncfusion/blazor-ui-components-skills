# ColorPicker - Customization and Accessibility

## Table of Contents
- [Overview](#overview)
- [ShowButtons Configuration](#showbuttons-configuration)
- [Custom Styling with CssClass](#custom-styling-with-cssclass)
- [Component States](#component-states)
- [HtmlAttributes Customization](#htmlattributes-customization)
- [Keyboard Navigation](#keyboard-navigation)
- [ARIA Attributes](#aria-attributes)
- [WCAG Compliance](#wcag-compliance)
- [Theme Customization](#theme-customization)
- [Responsive Design](#responsive-design)
- [Best Practices](#best-practices)

## Overview

The ColorPicker component provides extensive customization options and built-in accessibility features including:
- Custom CSS styling
- Disabled and RTL states
- Keyboard navigation support
- ARIA attributes for screen readers
- WCAG 2.1 compliance
- Theme Studio integration
- Responsive design patterns

## ShowButtons Configuration

Control Apply and Cancel buttons visibility in popup mode.

### With Buttons (Confirmation Required)

```razor
<SfColorPicker @bind-Value="@color" 
               ShowButtons="true">
</SfColorPicker>

@code {
    private string color = "#3498db";
}
```

**Features:**
- Apply button confirms selection
- Cancel button reverts to previous value
- User must explicitly confirm
- Better for significant changes

### Without Buttons (Immediate Selection)

```razor
<SfColorPicker @bind-Value="@color" 
               ShowButtons="false">
</SfColorPicker>

@code {
    private string color = "#e74c3c";
}
```

**Features:**
- Immediate color selection
- No confirmation needed
- Faster interaction
- Better for exploratory use

## Custom Styling with CssClass

Apply custom CSS classes for tailored appearance.

### Basic Custom Styling

```razor
<SfColorPicker @bind-Value="@color" 
               CssClass="custom-picker">
</SfColorPicker>

@code {
    private string color = "#2ecc71";
}

<style>
    .custom-picker .e-split-btn-wrapper {
        border: 2px solid #3498db;
        border-radius: 8px;
    }
    
    .custom-picker .e-selected-color {
        border-radius: 6px;
    }
</style>
```

### Themed Color Picker

```razor
<SfColorPicker @bind-Value="@color" 
               CssClass="dark-picker">
</SfColorPicker>

@code {
    private string color = "#9b59b6";
}

<style>
    .dark-picker {
        background-color: #2c3e50;
    }
    
    .dark-picker .e-split-btn-wrapper {
        background-color: #34495e;
        border: 1px solid #7f8c8d;
    }
    
    .dark-picker .e-color-picker-tooltip {
        background-color: #34495e;
        color: #ecf0f1;
    }
</style>
```

### Size Variations

```razor
<div class="size-examples">
    <h4>Small</h4>
    <SfColorPicker @bind-Value="@color1" CssClass="small-picker"></SfColorPicker>
    
    <h4>Medium (Default)</h4>
    <SfColorPicker @bind-Value="@color2"></SfColorPicker>
    
    <h4>Large</h4>
    <SfColorPicker @bind-Value="@color3" CssClass="large-picker"></SfColorPicker>
</div>

@code {
    private string color1 = "#3498db";
    private string color2 = "#e74c3c";
    private string color3 = "#2ecc71";
}

<style>
    .small-picker .e-split-btn-wrapper {
        width: 60px;
        height: 30px;
    }
    
    .large-picker .e-split-btn-wrapper {
        width: 100px;
        height: 50px;
    }
</style>
```

### Custom Button Styling

```razor
<SfColorPicker @bind-Value="@color" 
               CssClass="rounded-picker"
               ShowButtons="true">
</SfColorPicker>

@code {
    private string color = "#f39c12";
}

<style>
    .rounded-picker .e-split-btn-wrapper {
        border-radius: 25px;
    }
    
    .rounded-picker .e-btn {
        border-radius: 20px;
        padding: 8px 20px;
    }
    
    .rounded-picker .e-apply {
        background-color: #27ae60;
        border-color: #27ae60;
    }
    
    .rounded-picker .e-cancel {
        background-color: #e74c3c;
        border-color: #e74c3c;
    }
</style>
```

## Component States

### Disabled State

```razor
<SfColorPicker @bind-Value="@color" 
               Disabled="@isDisabled">
</SfColorPicker>

<button @onclick="() => isDisabled = !isDisabled">
    @(isDisabled ? "Enable" : "Disable")
</button>

@code {
    private string color = "#1abc9c";
    private bool isDisabled = false;
}
```

### Conditional Disabling

```razor
<SfColorPicker @bind-Value="@backgroundColor" 
               Disabled="@(!allowCustomization)">
</SfColorPicker>

<label>
    <input type="checkbox" @bind="allowCustomization" />
    Allow Color Customization
</label>

@code {
    private string backgroundColor = "#ecf0f1";
    private bool allowCustomization = true;
}
```

### EnableRtl (Right-to-Left)

```razor
<SfColorPicker @bind-Value="@color" 
               EnableRtl="true">
</SfColorPicker>

@code {
    private string color = "#e67e22";
}
```

**Use for:**
- Arabic language interfaces
- Hebrew language interfaces
- Other RTL languages
- Mirrored layouts

### EnablePersistence

```razor
<SfColorPicker @bind-Value="@color" 
               EnablePersistence="true"
               ID="persistentPicker">
</SfColorPicker>

@code {
    private string color = "#16a085";
}
```

**Features:**
- Maintains state across page reloads
- Stores color selection in browser
- Requires unique ID property
- Useful for user preferences

## HtmlAttributes Customization

Add custom HTML attributes to the wrapper element.

### Basic HTML Attributes

```razor
<SfColorPicker @bind-Value="@color" 
               HtmlAttributes="@customAttributes">
</SfColorPicker>

@code {
    private string color = "#c0392b";
    
    private Dictionary<string, object> customAttributes = new Dictionary<string, object>
    {
        { "data-color-type", "theme-primary" },
        { "aria-label", "Primary theme color selector" },
        { "title", "Select a color for the primary theme" }
    };
}
```

### Tooltip with Title Attribute

```razor
<SfColorPicker @bind-Value="@color" 
               HtmlAttributes="@tooltipAttributes">
</SfColorPicker>

@code {
    private string color = "#8e44ad";
    
    private Dictionary<string, object> tooltipAttributes = new Dictionary<string, object>
    {
        { "title", "Click to choose a color" },
        { "data-toggle", "tooltip" }
    };
}
```

### Data Attributes for Testing

```razor
<SfColorPicker @bind-Value="@color" 
               HtmlAttributes="@testAttributes">
</SfColorPicker>

@code {
    private string color = "#27ae60";
    
    private Dictionary<string, object> testAttributes = new Dictionary<string, object>
    {
        { "data-testid", "brand-color-picker" },
        { "data-component", "colorpicker" },
        { "data-required", "true" }
    };
}
```

## Keyboard Navigation

The ColorPicker supports comprehensive keyboard navigation.

### Keyboard Shortcuts

| Key | Action |
|-----|--------|
| **Space** / **Enter** | Open color picker popup |
| **Escape** | Close popup without applying |
| **Tab** | Move between interactive elements |
| **Shift+Tab** | Move backwards through elements |
| **Arrow Keys** | Navigate in picker/palette |
| **Enter** | Select color and close (without buttons) |
| **Enter** | Apply button (with buttons) |

### Keyboard Navigation Example

```razor
@page "/keyboard-accessible-picker"
@using Syncfusion.Blazor.Inputs

<h3>Keyboard Accessible Color Selection</h3>

<p>Try navigating with keyboard:</p>
<ul>
    <li>Tab to reach the color picker</li>
    <li>Press Space or Enter to open</li>
    <li>Use arrow keys to navigate colors</li>
    <li>Press Enter to select</li>
    <li>Press Escape to cancel</li>
</ul>

<div class="form-group">
    <label for="picker1">Primary Color:</label>
    <SfColorPicker @bind-Value="@primaryColor" 
                   ID="picker1"
                   Mode="ColorPickerMode.Palette">
    </SfColorPicker>
</div>

<div class="form-group">
    <label for="picker2">Secondary Color:</label>
    <SfColorPicker @bind-Value="@secondaryColor" 
                   ID="picker2"
                   Mode="ColorPickerMode.Palette">
    </SfColorPicker>
</div>

@code {
    private string primaryColor = "#3498db";
    private string secondaryColor = "#e74c3c";
}
```

### Focus Management

```razor
<SfColorPicker @bind-Value="@color" 
               CssClass="focus-visible">
</SfColorPicker>

@code {
    private string color = "#2ecc71";
}

<style>
    .focus-visible .e-split-btn-wrapper:focus {
        outline: 3px solid #3498db;
        outline-offset: 2px;
    }
    
    .focus-visible .e-tile:focus {
        box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.5);
    }
</style>
```

## ARIA Attributes

Built-in ARIA attributes for screen reader support.

### Accessible Label

```razor
<div class="form-field">
    <label id="colorLabel">Background Color:</label>
    <SfColorPicker @bind-Value="@color" 
                   HtmlAttributes="@ariaAttributes">
    </SfColorPicker>
</div>

@code {
    private string color = "#f39c12";
    
    private Dictionary<string, object> ariaAttributes = new Dictionary<string, object>
    {
        { "aria-labelledby", "colorLabel" },
        { "aria-describedby", "colorHelp" }
    };
}
```

### ARIA Live Region for Changes

```razor
<SfColorPicker @bind-Value="@color" 
               ValueChange="@AnnounceColorChange">
</SfColorPicker>

<div aria-live="polite" aria-atomic="true" class="sr-only">
    @screenReaderAnnouncement
</div>

@code {
    private string color = "#9b59b6";
    private string screenReaderAnnouncement = "";
    
    private void AnnounceColorChange(ColorPickerEventArgs args)
    {
        screenReaderAnnouncement = $"Color changed to {args.CurrentValue.Hex}";
        StateHasChanged();
    }
}

<style>
    .sr-only {
        position: absolute;
        width: 1px;
        height: 1px;
        padding: 0;
        margin: -1px;
        overflow: hidden;
        clip: rect(0,0,0,0);
        white-space: nowrap;
        border-width: 0;
    }
</style>
```

## WCAG Compliance

Ensure ColorPicker meets WCAG 2.1 accessibility standards.

### Sufficient Color Contrast

```razor
<div class="accessible-color-selector">
    <label for="textColor">Text Color:</label>
    <SfColorPicker @bind-Value="@textColor" 
                   ID="textColor"
                   ValueChange="@ValidateContrast">
    </SfColorPicker>
    
    <label for="bgColor">Background Color:</label>
    <SfColorPicker @bind-Value="@bgColor" 
                   ID="bgColor"
                   ValueChange="@ValidateContrast">
    </SfColorPicker>
    
    <div class="contrast-info" role="status" aria-live="polite">
        @if (!string.IsNullOrEmpty(contrastMessage))
        {
            <p class="@contrastClass">@contrastMessage</p>
        }
    </div>
    
    <div class="preview" style="color: @textColor; background-color: @bgColor; padding: 20px;">
        Sample Text - Check Contrast
    </div>
</div>

@code {
    private string textColor = "#000000";
    private string bgColor = "#ffffff";
    private string contrastMessage = "";
    private string contrastClass = "";
    
    private void ValidateContrast(ColorPickerEventArgs args)
    {
        // Simplified contrast check (real implementation would calculate luminance ratio)
        double ratio = CalculateContrastRatio(textColor, bgColor);
        
        if (ratio >= 7.0)
        {
            contrastMessage = $"Excellent contrast ratio: {ratio:F1}:1 (AAA)";
            contrastClass = "success";
        }
        else if (ratio >= 4.5)
        {
            contrastMessage = $"Good contrast ratio: {ratio:F1}:1 (AA)";
            contrastClass = "success";
        }
        else
        {
            contrastMessage = $"Poor contrast ratio: {ratio:F1}:1 - May not meet WCAG standards";
            contrastClass = "warning";
        }
    }
    
    private double CalculateContrastRatio(string color1, string color2)
    {
        // Simplified mock calculation
        return 4.5; // Real implementation would calculate actual luminance ratio
    }
}

<style>
    .contrast-info .success { color: #27ae60; }
    .contrast-info .warning { color: #e67e22; }
</style>
```

### Required Field Indication

```razor
<div class="form-group">
    <label for="requiredColor">
        Brand Color <span class="required" aria-label="required">*</span>
    </label>
    <SfColorPicker @bind-Value="@brandColor" 
                   ID="requiredColor"
                   HtmlAttributes="@requiredAttributes">
    </SfColorPicker>
</div>

@code {
    private string brandColor = "#3498db";
    
    private Dictionary<string, object> requiredAttributes = new Dictionary<string, object>
    {
        { "aria-required", "true" }
    };
}

<style>
    .required {
        color: #e74c3c;
        font-weight: bold;
    }
</style>
```

## Theme Customization

### Theme Studio Integration

The ColorPicker integrates with Syncfusion Theme Studio for custom themes.

**Steps:**
1. Visit [Theme Studio](https://ej2.syncfusion.com/themestudio/)
2. Customize ColorPicker appearance
3. Download generated CSS
4. Include in your application

### Custom Theme Example

```razor
<link href="custom-theme.css" rel="stylesheet" />

<SfColorPicker @bind-Value="@color" 
               CssClass="custom-theme">
</SfColorPicker>

@code {
    private string color = "#e74c3c";
}
```

### Dark Mode Support

```razor
<SfColorPicker @bind-Value="@color" 
               CssClass="@(isDarkMode ? "dark-mode-picker" : "")">
</SfColorPicker>

<button @onclick="ToggleDarkMode">Toggle Dark Mode</button>

@code {
    private string color = "#2ecc71";
    private bool isDarkMode = false;
    
    private void ToggleDarkMode()
    {
        isDarkMode = !isDarkMode;
    }
}

<style>
    .dark-mode-picker {
        filter: invert(1) hue-rotate(180deg);
    }
    
    /* Or with custom dark theme */
    .dark-mode-picker .e-color-picker-container {
        background-color: #2c3e50;
        color: #ecf0f1;
    }
</style>
```

## Responsive Design

### Mobile-Friendly Configuration

```razor
<SfColorPicker @bind-Value="@color" 
               Mode="ColorPickerMode.Palette"
               Columns="@GetResponsiveColumns()"
               CssClass="responsive-picker">
</SfColorPicker>

@code {
    private string color = "#f39c12";
    
    [Inject]
    private NavigationManager NavigationManager { get; set; }
    
    private int GetResponsiveColumns()
    {
        // Simplified - real implementation would check window size
        return 5; // Use 5 columns for mobile
    }
}
```

### Adaptive Layout

```razor
@page "/responsive-colorpicker"
@using Syncfusion.Blazor.Inputs

<div class="color-selector-container">
    <h3>Responsive Color Picker</h3>
    
    <!-- Desktop: Inline display -->
    <div class="desktop-only">
        <SfColorPicker @bind-Value="@color" 
                       Inline="true"
                       Mode="ColorPickerMode.Palette"
                       Columns="10">
        </SfColorPicker>
    </div>
    
    <!-- Mobile: Popup display with fewer columns -->
    <div class="mobile-only">
        <SfColorPicker @bind-Value="@color" 
                       Inline="false"
                       Mode="ColorPickerMode.Palette"
                       Columns="5">
        </SfColorPicker>
    </div>
</div>

@code {
    private string color = "#3498db";
}

<style>
    .mobile-only {
        display: none;
    }
    
    @@media (max-width: 768px) {
        .desktop-only {
            display: none;
        }
        
        .mobile-only {
            display: block;
        }
    }
</style>
```

## Best Practices

### Accessibility Checklist

- ✅ Provide clear labels for all color pickers
- ✅ Ensure keyboard navigation works completely
- ✅ Include ARIA attributes for screen readers
- ✅ Announce color changes to assistive technologies
- ✅ Validate color contrast for readability
- ✅ Support focus indicators
- ✅ Test with screen readers (NVDA, JAWS, VoiceOver)
- ✅ Ensure touch targets are 44x44px minimum (mobile)
- ✅ Don't rely solely on color to convey information

### Customization Best Practices

1. **Consistent styling**: Match your app's design system
2. **Performance**: Minimize custom CSS complexity
3. **Responsive**: Test on various screen sizes
4. **Dark mode**: Support system dark mode preference
5. **Testing**: Validate on multiple browsers
6. **Documentation**: Document custom CSS classes
7. **Maintenance**: Keep custom styles separate and organized
8. **Accessibility**: Never compromise accessibility for aesthetics

### Common Customization Patterns

```razor
/* Minimal modern look */
.modern-picker .e-split-btn-wrapper {
    border: none;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

/* Bordered with shadow */
.bordered-picker .e-split-btn-wrapper {
    border: 2px solid #3498db;
    box-shadow: 0 4px 6px rgba(52, 152, 219, 0.2);
}

/* Compact size */
.compact-picker .e-split-btn-wrapper {
    width: 50px;
    height: 28px;
}

/* Full-width on mobile */
@@media (max-width: 768px) {
    .responsive-picker .e-split-btn-wrapper {
        width: 100%;
    }
}
```

## Related Topics

- **Getting Started**: Initial setup → [colorpicker-getting-started.md](colorpicker-getting-started.md)
- **Events**: Event handling → [colorpicker-events-binding.md](colorpicker-events-binding.md)
- **Presets**: Custom palettes → [colorpicker-presets-colors.md](colorpicker-presets-colors.md)
