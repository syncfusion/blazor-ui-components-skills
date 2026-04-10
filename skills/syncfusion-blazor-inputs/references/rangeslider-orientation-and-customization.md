# RangeSlider - Orientation and Customization

## Table of Contents
- [Overview](#overview)
- [Orientation Property](#orientation-property)
- [ShowButtons for Increment/Decrement](#showbuttons-for-incrementdecrement)
- [Width and Size Configuration](#width-and-size-configuration)
- [Animation Settings](#animation-settings)
- [CssClass Customization](#cssclass-customization)
- [RTL Support](#rtl-support)
- [ReadOnly and Enabled States](#readonly-and-enabled-states)
- [Theme Customization](#theme-customization)

## Overview

The RangeSlider component offers extensive customization options to match your application's design and user experience requirements. From orientation (horizontal vs vertical) to animation behavior, button controls, styling, and internationalization, these settings allow you to tailor the slider to your specific use case.

## Orientation Property

### Horizontal Orientation (Default)

The default orientation displays the slider horizontally:

```razor
@using Syncfusion.Blazor.Inputs

<div class="horizontal-slider">
    <h4>Price Range (Horizontal)</h4>
    <SfSlider @bind-Value="@priceRange"
              Type="SliderType.Range"
              Orientation="SliderOrientation.Horizontal"
              Min="0"
              Max="1000">
        <SliderTicks Placement="Placement.After" LargeStep="200"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Selected: $@priceRange[0] - $@priceRange[1]</p>
</div>

@code {
    private int[] priceRange = new int[] { 200, 800 };
}
```

### Vertical Orientation

Display the slider vertically for specific layouts:

```razor
<div class="vertical-slider-container">
    <h4>Volume Control</h4>
    <div style="height: 400px; display: flex; justify-content: center;">
        <SfSlider @bind-Value="@volumeRange"
                  Type="SliderType.Range"
                  Orientation="SliderOrientation.Vertical"
                  Min="0"
                  Max="100">
            <SliderTicks Placement="Placement.Before" LargeStep="20"></SliderTicks>
            <SliderTooltip IsVisible="true" 
                           Placement="TooltipPlacement.Before"
                           ShowOn="TooltipShowOn.Always">
            </SliderTooltip>
        </SfSlider>
    </div>
    <p>Range: @volumeRange[0]% - @volumeRange[1]%</p>
</div>

@code {
    private int[] volumeRange = new int[] { 30, 80 };
}
```

**Key Points for Vertical Sliders:**
- Container must have explicit height (e.g., `height: 400px`)
- Consider using `Placement.Before` for ticks (left side)
- Adjust tooltip placement to avoid overlap
- Ideal for audio controls, thermostats, or sidebar filters

### Side-by-Side Comparison

```razor
<div class="orientation-comparison">
    <div class="slider-example">
        <h4>Horizontal</h4>
        <SfSlider @bind-Value="@range1"
                  Type="SliderType.Range"
                  Orientation="SliderOrientation.Horizontal"
                  Min="0"
                  Max="100">
            <SliderTicks Placement="Placement.After" LargeStep="25"></SliderTicks>
        </SfSlider>
    </div>
    
    <div class="slider-example">
        <h4>Vertical</h4>
        <div style="height: 300px;">
            <SfSlider @bind-Value="@range2"
                      Type="SliderType.Range"
                      Orientation="SliderOrientation.Vertical"
                      Min="0"
                      Max="100">
                <SliderTicks Placement="Placement.Before" LargeStep="25"></SliderTicks>
            </SfSlider>
        </div>
    </div>
</div>

@code {
    private int[] range1 = new int[] { 25, 75 };
    private int[] range2 = new int[] { 25, 75 };
}
```

## ShowButtons for Increment/Decrement

### Enable Button Controls

Add increment/decrement buttons for precise value adjustment:

```razor
<div class="slider-with-buttons">
    <h4>Temperature Range with Buttons</h4>
    <SfSlider @bind-Value="@temperatureRange"
              Type="SliderType.Range"
              Min="0"
              Max="50"
              Step="1"
              ShowButtons="true">
        <SliderTicks Placement="Placement.After" LargeStep="10"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Selected: @temperatureRange[0]°C - @temperatureRange[1]°C</p>
</div>

@code {
    private int[] temperatureRange = new int[] { 18, 26 };
}
```

**Button Features:**
- Appear on both ends of the slider
- Click to increment/decrement by Step value
- Useful for precise adjustments
- Improves accessibility (keyboard navigation alternative)
- Works with both horizontal and vertical orientations

### Buttons with Custom Step

```razor
<div class="fine-control-slider">
    <h4>Budget Range (Fine Control)</h4>
    <SfSlider @bind-Value="@budgetRange"
              Type="SliderType.Range"
              Min="0"
              Max="10000"
              Step="100"
              ShowButtons="true">
        <SliderTicks Placement="Placement.After" LargeStep="2000" Format="C0"></SliderTicks>
        <SliderTooltip IsVisible="true" Format="C0" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Budget: @budgetRange[0].ToString("C0") - @budgetRange[1].ToString("C0")</p>
</div>

@code {
    private int[] budgetRange = new int[] { 2000, 7000 };
    // Buttons adjust by $100 increments
}
```

## Width and Size Configuration

### Custom Width for Horizontal Sliders

```razor
<div class="custom-width-sliders">
    <!-- Default width (100% of container) -->
    <div class="slider-wrapper">
        <h4>Full Width Slider</h4>
        <SfSlider @bind-Value="@range1"
                  Type="SliderType.Range"
                  Min="0"
                  Max="100">
            <SliderTicks Placement="Placement.After" LargeStep="25"></SliderTicks>
        </SfSlider>
    </div>
    
    <!-- Fixed width in pixels -->
    <div class="slider-wrapper">
        <h4>Fixed Width (500px)</h4>
        <SfSlider @bind-Value="@range2"
                  Type="SliderType.Range"
                  Min="0"
                  Max="100"
                  Width="500px">
            <SliderTicks Placement="Placement.After" LargeStep="25"></SliderTicks>
        </SfSlider>
    </div>
    
    <!-- Percentage width -->
    <div class="slider-wrapper">
        <h4>Responsive Width (75%)</h4>
        <SfSlider @bind-Value="@range3"
                  Type="SliderType.Range"
                  Min="0"
                  Max="100"
                  Width="75%">
            <SliderTicks Placement="Placement.After" LargeStep="25"></SliderTicks>
        </SfSlider>
    </div>
</div>

@code {
    private int[] range1 = new int[] { 25, 75 };
    private int[] range2 = new int[] { 25, 75 };
    private int[] range3 = new int[] { 25, 75 };
}
```

### Height for Vertical Sliders

```razor
<div class="vertical-heights">
    <div style="display: flex; gap: 50px;">
        <!-- Short vertical slider -->
        <div>
            <h4>Compact (200px)</h4>
            <div style="height: 200px;">
                <SfSlider @bind-Value="@range1"
                          Type="SliderType.Range"
                          Orientation="SliderOrientation.Vertical"
                          Min="0"
                          Max="100">
                </SfSlider>
            </div>
        </div>
        
        <!-- Tall vertical slider -->
        <div>
            <h4>Extended (400px)</h4>
            <div style="height: 400px;">
                <SfSlider @bind-Value="@range2"
                          Type="SliderType.Range"
                          Orientation="SliderOrientation.Vertical"
                          Min="0"
                          Max="100">
                    <SliderTicks Placement="Placement.Before" LargeStep="20"></SliderTicks>
                </SfSlider>
            </div>
        </div>
    </div>
</div>

@code {
    private int[] range1 = new int[] { 30, 70 };
    private int[] range2 = new int[] { 30, 70 };
}
```

## Animation Settings

### EnableAnimation Property

Control smooth transitions when handles move:

```razor
<div class="animation-demo">
    <!-- With animation (default) -->
    <div class="slider-example">
        <h4>Animated Slider</h4>
        <SfSlider @bind-Value="@range1"
                  Type="SliderType.Range"
                  EnableAnimation="true"
                  Min="0"
                  Max="100"
                  ShowButtons="true">
            <SliderTicks Placement="Placement.After" LargeStep="20"></SliderTicks>
        </SfSlider>
        <p>Smooth transitions enabled</p>
    </div>
    
    <!-- Without animation -->
    <div class="slider-example">
        <h4>Non-Animated Slider</h4>
        <SfSlider @bind-Value="@range2"
                  Type="SliderType.Range"
                  EnableAnimation="false"
                  Min="0"
                  Max="100"
                  ShowButtons="true">
            <SliderTicks Placement="Placement.After" LargeStep="20"></SliderTicks>
        </SfSlider>
        <p>Instant updates</p>
    </div>
</div>

@code {
    private int[] range1 = new int[] { 30, 70 };
    private int[] range2 = new int[] { 30, 70 };
}
```

**Animation Guidelines:**
- **EnableAnimation="true"** (default): Smooth, professional appearance
- **EnableAnimation="false"**: Better performance for frequent updates
- Disable for real-time data visualization or gaming scenarios
- Keep enabled for form inputs and filters

## CssClass Customization

### Applying Custom CSS Classes

```razor
<div class="custom-styled-sliders">
    <h4>Custom Styled Range Slider</h4>
    <SfSlider @bind-Value="@rangeValues"
              Type="SliderType.Range"
              Min="0"
              Max="100"
              CssClass="custom-range-slider">
        <SliderTicks Placement="Placement.After" LargeStep="25"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
</div>

@code {
    private int[] rangeValues = new int[] { 30, 70 };
}
```

**Custom CSS:**
```css
/* Custom styling for slider track */
.custom-range-slider .e-slider-track {
    background: linear-gradient(to right, #e3f2fd, #1976d2);
    height: 8px;
}

/* Custom handle styling */
.custom-range-slider .e-handle {
    background-color: #1976d2;
    border: 3px solid #ffffff;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
    width: 24px;
    height: 24px;
}

/* Custom handle hover effect */
.custom-range-slider .e-handle:hover {
    background-color: #1565c0;
    transform: scale(1.1);
}

/* Custom tooltip styling */
.custom-range-slider .e-slider-tooltip {
    background-color: #1976d2;
    color: #ffffff;
    font-weight: 600;
    border-radius: 8px;
}
```

### Dark Mode Slider

```razor
<div class="dark-mode-slider">
    <h4>Dark Mode Range Slider</h4>
    <SfSlider @bind-Value="@rangeValues"
              Type="SliderType.Range"
              Min="0"
              Max="100"
              CssClass="dark-slider">
        <SliderTicks Placement="Placement.After" LargeStep="25"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
</div>

@code {
    private int[] rangeValues = new int[] { 30, 70 };
}
```

**Dark Mode CSS:**
```css
.dark-mode-slider {
    background-color: #1e1e1e;
    padding: 20px;
    border-radius: 8px;
}

.dark-slider .e-slider-track {
    background-color: #333333;
}

.dark-slider .e-range {
    background-color: #4a9eff;
}

.dark-slider .e-handle {
    background-color: #ffffff;
    border: 2px solid #4a9eff;
}

.dark-slider .e-tick {
    color: #cccccc;
}

.dark-slider .e-slider-tooltip {
    background-color: #333333;
    color: #ffffff;
    border: 1px solid #4a9eff;
}
```

## RTL Support

### EnableRtl for Right-to-Left Languages

```razor
<div class="rtl-demo">
    <h4>Right-to-Left Slider (Arabic, Hebrew)</h4>
    <SfSlider @bind-Value="@rangeValues"
              Type="SliderType.Range"
              EnableRtl="true"
              Min="0"
              Max="100">
        <SliderTicks Placement="Placement.After" LargeStep="20"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p style="direction: rtl;">النطاق المحدد: @rangeValues[0] - @rangeValues[1]</p>
</div>

@code {
    private int[] rangeValues = new int[] { 30, 70 };
}
```

**RTL Behavior:**
- Slider direction reverses (right to left)
- Min value on the right, Max on the left
- Handles move in opposite direction
- Ticks and tooltips adjust automatically
- Essential for Arabic, Hebrew, Persian, and other RTL languages

## ReadOnly and Enabled States

### ReadOnly Mode

Display values without allowing user interaction:

```razor
<div class="readonly-display">
    <h4>Approved Budget Range (Read-Only)</h4>
    <SfSlider @bind-Value="@approvedRange"
              Type="SliderType.Range"
              Min="0"
              Max="100000"
              ReadOnly="true">
        <SliderTicks Placement="Placement.After" LargeStep="20000" Format="C0"></SliderTicks>
        <SliderTooltip IsVisible="true" Format="C0" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>✓ Approved: @approvedRange[0].ToString("C0") - @approvedRange[1].ToString("C0")</p>
</div>

@code {
    private int[] approvedRange = new int[] { 25000, 75000 };
}
```

### Disabled State

Completely disable the slider:

```razor
<div class="disabled-slider">
    <h4>Unavailable Range Slider</h4>
    <SfSlider @bind-Value="@disabledRange"
              Type="SliderType.Range"
              Min="0"
              Max="100"
              Enabled="false">
        <SliderTicks Placement="Placement.After" LargeStep="25"></SliderTicks>
    </SfSlider>
    <p>⚠️ This feature is currently unavailable</p>
</div>

@code {
    private int[] disabledRange = new int[] { 30, 70 };
}
```

### Conditional Enable/Disable

```razor
<div class="conditional-enable">
    <label>
        <input type="checkbox" @bind="isEnabled" />
        Enable Range Selection
    </label>
    
    <h4>Conditional Range Slider</h4>
    <SfSlider @bind-Value="@rangeValues"
              Type="SliderType.Range"
              Min="0"
              Max="100"
              Enabled="@isEnabled">
        <SliderTicks Placement="Placement.After" LargeStep="20"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
</div>

@code {
    private int[] rangeValues = new int[] { 30, 70 };
    private bool isEnabled = true;
}
```

**State Differences:**
- **ReadOnly**: Values visible, no interaction, styling may indicate view-only
- **Enabled="false"**: Grayed out appearance, completely disabled
- **Use ReadOnly** for confirmed/approved values
- **Use Disabled** for unavailable or locked features

## Theme Customization

### Using Built-in Themes

Reference different Syncfusion themes in your HTML:

```html
<!-- Bootstrap 5 Theme -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Material Theme -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Fluent Theme -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />

<!-- Tailwind Theme -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />

<!-- Dark Themes -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap-dark.css" rel="stylesheet" />
<link href="_content/Syncfusion.Blazor.Themes/material-dark.css" rel="stylesheet" />
```

### Theme Studio Customization

Visit [Syncfusion Theme Studio](https://blazor.syncfusion.com/themestudio/) to create custom themes with:
- Brand colors
- Custom fonts
- Border radius
- Component-specific styling

### CSS Variable Overrides

```css
:root {
    --slider-track-color: #e0e0e0;
    --slider-range-color: #1976d2;
    --slider-handle-color: #1976d2;
    --slider-handle-hover-color: #1565c0;
    --slider-tooltip-bg: #1976d2;
    --slider-tooltip-text: #ffffff;
}

.custom-theme-slider .e-slider-track {
    background-color: var(--slider-track-color);
}

.custom-theme-slider .e-range {
    background-color: var(--slider-range-color);
}

.custom-theme-slider .e-handle {
    background-color: var(--slider-handle-color);
}

.custom-theme-slider .e-handle:hover {
    background-color: var(--slider-handle-hover-color);
}

.custom-theme-slider .e-slider-tooltip {
    background-color: var(--slider-tooltip-bg);
    color: var(--slider-tooltip-text);
}
```

## Complete Customization Example

```razor
<div class="fully-customized-slider">
    <h4>Premium Price Range Filter</h4>
    
    <div class="controls">
        <label>
            <input type="checkbox" @bind="showButtons" />
            Show Buttons
        </label>
        <label>
            <input type="checkbox" @bind="enableAnimation" />
            Enable Animation
        </label>
        <label>
            <input type="checkbox" @bind="isRtl" />
            RTL Mode
        </label>
    </div>
    
    <SfSlider @bind-Value="@priceRange"
              Type="SliderType.Range"
              Min="0"
              Max="10000"
              Step="100"
              Width="90%"
              ShowButtons="@showButtons"
              EnableAnimation="@enableAnimation"
              EnableRtl="@isRtl"
              CssClass="premium-slider">
        <SliderColorRanges>
            <ColorRange Start="0" End="3000" Color="#4CAF50"></ColorRange>
            <ColorRange Start="3000" End="7000" Color="#2196F3"></ColorRange>
            <ColorRange Start="7000" End="10000" Color="#9C27B0"></ColorRange>
        </SliderColorRanges>
        <SliderTicks Placement="Placement.After" 
                     LargeStep="2000" 
                     SmallStep="500"
                     ShowSmallTicks="true"
                     Format="C0">
        </SliderTicks>
        <SliderTooltip IsVisible="true" 
                       Format="C0" 
                       ShowOn="TooltipShowOn.Always">
        </SliderTooltip>
    </SfSlider>
    
    <div class="range-display">
        <strong>Selected:</strong> @priceRange[0].ToString("C0") - @priceRange[1].ToString("C0")
    </div>
</div>

@code {
    private int[] priceRange = new int[] { 2000, 7000 };
    private bool showButtons = true;
    private bool enableAnimation = true;
    private bool isRtl = false;
}
```

**CSS for Premium Slider:**
```css
.fully-customized-slider {
    padding: 30px;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    border-radius: 12px;
    color: white;
}

.premium-slider .e-slider-track {
    height: 10px;
    border-radius: 5px;
}

.premium-slider .e-handle {
    width: 28px;
    height: 28px;
    border: 4px solid white;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
}

.premium-slider .e-slider-tooltip {
    font-size: 16px;
    font-weight: 700;
    padding: 8px 16px;
    border-radius: 20px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
}

.range-display {
    margin-top: 20px;
    text-align: center;
    font-size: 20px;
    font-weight: 600;
}
```

## Best Practices

1. **Choose Appropriate Orientation**: Horizontal for most cases, vertical for specific layouts
2. **Use ShowButtons**: Add for precise control and accessibility
3. **Set Explicit Width/Height**: Especially for vertical sliders
4. **Consider Animation**: Disable for performance-critical scenarios
5. **Apply CssClass**: For consistent branding across application
6. **Test RTL**: If supporting international audiences
7. **Use ReadOnly vs Disabled**: Based on user intent

## Next Steps

- **Events** - Handle user interactions and value changes
- **Form Integration** - Combine with validation and data binding
- **Responsive Design** - Adapt slider to different screen sizes
- **Accessibility** - Ensure keyboard navigation and screen reader support
