# ColorPicker - Modes and Configuration

## Table of Contents
- [Overview](#overview)
- [Picker Mode](#picker-mode)
- [Palette Mode](#palette-mode)
- [Inline vs Popup Display](#inline-vs-popup-display)
- [Mode Switcher](#mode-switcher)
- [Columns Configuration](#columns-configuration)
- [Show Buttons](#show-buttons)
- [Mode Selection Guide](#mode-selection-guide)

## Overview

The Syncfusion Blazor ColorPicker offers two distinct selection modes, each optimized for different use cases:

1. **Picker Mode** (default): Full gradient-based color selector with unlimited color choices
2. **Palette Mode**: Grid-based selection from predefined color sets

You can configure the display behavior (popup vs inline) and allow users to switch between modes dynamically.

## Picker Mode

Picker mode provides a gradient-based color selector that allows users to choose any color from the full spectrum.

### Basic Picker Mode

```razor
<SfColorPicker @bind-Value="@color" 
               Mode="ColorPickerMode.Picker">
</SfColorPicker>

@code {
    private string color = "#008000";
}
```

### Features of Picker Mode

- **Gradient Area**: Large color spectrum for precise selection
- **Hue Bar**: Vertical or horizontal bar for hue selection
- **Preview**: Shows current and previous color
- **Hex Input**: Manual hex value entry
- **Opacity Control**: Optional alpha slider (with `EnableOpacity="true"`)

### Picker Mode with Opacity

```razor
<SfColorPicker @bind-Value="@colorWithOpacity" 
               Mode="ColorPickerMode.Picker"
               EnableOpacity="true">
</SfColorPicker>

@code {
    private string colorWithOpacity = "rgba(231, 76, 60, 0.7)";
}
```

### When to Use Picker Mode

- **Design tools**: Graphic editors, drawing applications, image annotation
- **Unrestricted selection**: When users need any color from the spectrum
- **Professional applications**: CAD, design software, development tools
- **Custom styling**: User-defined themes, personalization features
- **Precise color matching**: When exact color values are critical

## Palette Mode

Palette mode displays a grid of predefined colors for quick selection.

### Basic Palette Mode

```razor
<SfColorPicker @bind-Value="@color" 
               Mode="ColorPickerMode.Palette">
</SfColorPicker>

@code {
    private string color = "#e74c3c";
}
```

### Default Palette

The default palette includes:
- Material design colors
- Standard web colors
- Recent colors (if enabled)

### Palette with Custom Columns

```razor
<SfColorPicker @bind-Value="@color" 
               Mode="ColorPickerMode.Palette"
               Columns="8">
</SfColorPicker>

@code {
    private string color = "#3498db";
}
```

### When to Use Palette Mode

- **Brand colors**: Limit selection to company/brand color palette
- **Themed applications**: Predefined theme color choices
- **Simplified UX**: Reduce decision fatigue with curated colors
- **Consistency**: Ensure color consistency across application
- **Quick selection**: Fast color picking without exploration

## Inline vs Popup Display

Control whether the ColorPicker appears inline (always visible) or as a popup (triggered by button click).

### Popup Display (Default)

```razor
<SfColorPicker @bind-Value="@color" 
               Inline="false">
</SfColorPicker>

@code {
    private string color = "#008000";
}
```

**Behavior:**
- Shows a color preview button
- Clicking button opens color picker popup
- User selects color in popup
- Can show Apply/Cancel buttons with `ShowButtons="true"`

### Inline Display

```razor
<SfColorPicker @bind-Value="@color" 
               Inline="true">
</SfColorPicker>

@code {
    private string color = "#008000";
}
```

**Behavior:**
- ColorPicker always visible
- No popup or button trigger
- Immediate selection feedback
- Takes up more screen space

### When to Use Inline Display

```razor
@page "/theme-builder"
@using Syncfusion.Blazor.Inputs

<div class="theme-panel">
    <h3>Theme Customization</h3>
    
    <div class="color-option">
        <label>Primary Color:</label>
        <SfColorPicker @bind-Value="@primaryColor" 
                       Inline="true"
                       Mode="ColorPickerMode.Palette">
        </SfColorPicker>
    </div>
    
    <div class="color-option">
        <label>Accent Color:</label>
        <SfColorPicker @bind-Value="@accentColor" 
                       Inline="true"
                       Mode="ColorPickerMode.Palette">
        </SfColorPicker>
    </div>
</div>

@code {
    private string primaryColor = "#3498db";
    private string accentColor = "#e74c3c";
}

<style>
    .theme-panel {
        padding: 20px;
        border: 1px solid #ddd;
    }
    
    .color-option {
        margin-bottom: 20px;
    }
</style>
```

**Best for:**
- Settings panels
- Theme builders
- Design tools
- Color customization screens
- When space is not a constraint

## Mode Switcher

Allow users to toggle between Picker and Palette modes dynamically.

### Enable Mode Switcher

```razor
<SfColorPicker @bind-Value="@color" 
               Mode="ColorPickerMode.Picker"
               ModeSwitcher="true">
</SfColorPicker>

@code {
    private string color = "#9b59b6";
}
```

**Features:**
- Shows mode toggle icon in UI
- Users can switch between Picker and Palette
- Current selection preserved when switching
- Fires `ModeSwitched` event on mode change

### Mode Switcher with Event Handling

```razor
<SfColorPicker @bind-Value="@color" 
               Mode="@currentMode"
               ModeSwitcher="true"
               ModeSwitched="@OnModeChanged">
</SfColorPicker>

<p>Current Mode: @currentMode</p>

@code {
    private string color = "#2ecc71";
    private ColorPickerMode currentMode = ColorPickerMode.Picker;
    
    private void OnModeChanged(ModeSwitchEventArgs args)
    {
        currentMode = args.Mode;
        Console.WriteLine($"Mode changed to: {currentMode}");
    }
}
```

### When to Enable Mode Switcher

- **Flexible workflows**: Users may prefer different modes for different tasks
- **Learning applications**: Help users understand both selection methods
- **Power users**: Give advanced users choice of tool
- **Hybrid use cases**: Some colors need precise picking, others quick selection

## Columns Configuration

Control the number of columns in Palette mode for optimal layout.

### Default Columns (10)

```razor
<SfColorPicker @bind-Value="@color" 
               Mode="ColorPickerMode.Palette">
</SfColorPicker>
```

### Custom Column Count

```razor
<SfColorPicker @bind-Value="@color" 
               Mode="ColorPickerMode.Palette"
               Columns="5">
</SfColorPicker>

@code {
    private string color = "#f39c12";
}
```

### Responsive Column Layout

```razor
<div class="desktop-view">
    <SfColorPicker @bind-Value="@colorDesktop" 
                   Mode="ColorPickerMode.Palette"
                   Columns="10">
    </SfColorPicker>
</div>

<div class="mobile-view">
    <SfColorPicker @bind-Value="@colorMobile" 
                   Mode="ColorPickerMode.Palette"
                   Columns="5">
    </SfColorPicker>
</div>

@code {
    private string colorDesktop = "#1abc9c";
    private string colorMobile = "#1abc9c";
}
```

### Column Count Recommendations

| Screen Size | Recommended Columns | Use Case |
|-------------|-------------------|----------|
| Desktop | 10-12 | Full palette visibility |
| Tablet | 6-8 | Balanced layout |
| Mobile | 4-5 | Touch-friendly spacing |
| Narrow Sidebars | 3-4 | Space-constrained panels |

## Show Buttons

Display Apply and Cancel buttons for popup mode (requires user confirmation).

### Without Buttons (Default)

```razor
<SfColorPicker @bind-Value="@color" 
               ShowButtons="false">
</SfColorPicker>

@code {
    private string color = "#e67e22";
}
```

**Behavior:**
- Color changes immediately on selection
- Popup closes automatically
- No confirmation required

### With Apply/Cancel Buttons

```razor
<SfColorPicker @bind-Value="@color" 
               ShowButtons="true">
</SfColorPicker>

@code {
    private string color = "#16a085";
}
```

**Behavior:**
- Color changes only when Apply clicked
- Cancel reverts to previous color
- User must confirm selection
- Popup remains open until Apply/Cancel

### Buttons with Event Handling

```razor
<SfColorPicker @bind-Value="@color" 
               ShowButtons="true"
               ValueChange="@OnColorApplied">
</SfColorPicker>

<p>Applied Color: @color</p>

@code {
    private string color = "#c0392b";
    
    private void OnColorApplied(ColorPickerEventArgs args)
    {
        Console.WriteLine($"Color applied: {args.CurrentValue.Hex}");
        // Save to database, update UI, etc.
    }
}
```

### When to Use Buttons

**Enable buttons when:**
- Changes have significant impact (theme changes, expensive operations)
- User needs preview before committing
- Undo/cancel is important
- Batch operations are involved
- Professional/enterprise applications

**Disable buttons when:**
- Immediate feedback is desired
- Lightweight, exploratory color selection
- Real-time preview is the primary use case
- Mobile/touch-first interfaces
- Simplified user experience preferred

## Mode Selection Guide

### Decision Matrix

| Requirement | Recommended Mode | Configuration |
|------------|------------------|---------------|
| Brand colors only | Palette | Custom `PresetColors` |
| Any color needed | Picker | Default or with opacity |
| Quick selection | Palette | Default presets |
| Precise control | Picker | With opacity enabled |
| Limited options | Palette | Custom palette, fewer columns |
| Design tool | Picker | ModeSwitcher enabled |
| Mobile-first | Palette | 4-5 columns |
| Theme builder | Picker + Palette | ModeSwitcher enabled |
| Transparency needed | Picker | EnableOpacity="true" |
| Consistent colors | Palette | Custom brand palette |

### Example: Complete Configuration

```razor
@page "/colorpicker-advanced"
@using Syncfusion.Blazor.Inputs

<h3>Advanced ColorPicker Configuration</h3>

<div class="example">
    <h4>Design Tool Mode (Flexible)</h4>
    <SfColorPicker @bind-Value="@designColor" 
                   Mode="ColorPickerMode.Picker"
                   ModeSwitcher="true"
                   EnableOpacity="true"
                   ShowButtons="true">
    </SfColorPicker>
    <p>Selected: @designColor</p>
</div>

<div class="example">
    <h4>Brand Palette (Restricted)</h4>
    <SfColorPicker @bind-Value="@brandColor" 
                   Mode="ColorPickerMode.Palette"
                   Columns="6"
                   ShowButtons="false"
                   Inline="true">
    </SfColorPicker>
    <p>Selected: @brandColor</p>
</div>

<div class="example">
    <h4>Quick Popup Selector</h4>
    <SfColorPicker @bind-Value="@quickColor" 
                   Mode="ColorPickerMode.Palette"
                   ShowButtons="false">
    </SfColorPicker>
    <p>Selected: @quickColor</p>
</div>

@code {
    private string designColor = "rgba(52, 152, 219, 0.8)";
    private string brandColor = "#e74c3c";
    private string quickColor = "#2ecc71";
}
```

## Best Practices

1. **Choose the right mode**: Use Picker for unrestricted selection, Palette for curated choices
2. **Enable ModeSwitcher**: For power users who want flexibility
3. **Configure columns wisely**: Match screen size and available space
4. **Use buttons for significant changes**: When color change has major impact
5. **Inline for settings**: Use inline display in dedicated customization panels
6. **Popup for forms**: Use popup mode for general form inputs
7. **Test on devices**: Ensure column count works on target devices
8. **Provide feedback**: Show color preview to confirm user selection

## Related Topics

- **Preset Colors**: Custom color palettes → [colorpicker-presets-colors.md](colorpicker-presets-colors.md)
- **Opacity**: Alpha channel configuration → [colorpicker-opacity-values.md](colorpicker-opacity-values.md)
- **Events**: Handle mode switching and selection → [colorpicker-events-binding.md](colorpicker-events-binding.md)
