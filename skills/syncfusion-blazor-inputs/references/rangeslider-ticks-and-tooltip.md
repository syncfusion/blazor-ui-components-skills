# RangeSlider - Ticks and Tooltip

## Table of Contents
- [Overview](#overview)
- [SliderTicks Configuration](#sliderticks-configuration)
- [Tick Intervals and Steps](#tick-intervals-and-steps)
- [Tick Placement Options](#tick-placement-options)
- [Tick Label Formatting](#tick-label-formatting)
- [SliderTooltip Configuration](#slidertooltip-configuration)
- [Tooltip Visibility Modes](#tooltip-visibility-modes)
- [Tooltip Formatting](#tooltip-formatting)
- [Custom Tooltip Templates](#custom-tooltip-templates)
- [Combined Patterns](#combined-patterns)

## Overview

Ticks and tooltips enhance the RangeSlider user experience by providing visual markers for values and displaying current selections. Ticks help users identify specific value positions, while tooltips show the exact values of each handle during interaction.

## SliderTicks Configuration

### Basic Ticks Setup

Add the `SliderTicks` component inside `SfSlider` to display value markers:

```razor
<SfSlider @bind-Value="@rangeValues"
          Type="SliderType.Range"
          Min="0"
          Max="100">
    <SliderTicks Placement="Placement.After" LargeStep="20"></SliderTicks>
</SfSlider>

@code {
    private int[] rangeValues = new int[] { 30, 70 };
}
```

**Key Points:**
- `SliderTicks` is a child component of `SfSlider`
- `LargeStep` defines the interval between major tick marks
- `Placement` controls where ticks appear relative to the slider bar
- Ticks improve value identification and selection precision

### Ticks with Both Large and Small Steps

```razor
<SfSlider @bind-Value="@priceRange"
          Type="SliderType.Range"
          Min="0"
          Max="1000"
          Step="10">
    <SliderTicks Placement="Placement.After" 
                 LargeStep="200" 
                 SmallStep="50"
                 ShowSmallTicks="true">
    </SliderTicks>
</SfSlider>

@code {
    private int[] priceRange = new int[] { 200, 800 };
}
```

**Tick Types:**
- **Large Ticks**: Primary markers with labels (LargeStep)
- **Small Ticks**: Secondary markers without labels (SmallStep)
- `ShowSmallTicks="true"` enables small tick visibility

## Tick Intervals and Steps

### LargeStep Property

Defines the interval for major tick marks with labels:

```razor
<!-- Every 25 units -->
<SfSlider @bind-Value="@range1"
          Type="SliderType.Range"
          Min="0"
          Max="100">
    <SliderTicks Placement="Placement.After" LargeStep="25"></SliderTicks>
</SfSlider>

<!-- Every 100 units for larger ranges -->
<SfSlider @bind-Value="@range2"
          Type="SliderType.Range"
          Min="0"
          Max="1000">
    <SliderTicks Placement="Placement.After" LargeStep="100"></SliderTicks>
</SfSlider>

@code {
    private int[] range1 = new int[] { 25, 75 };
    private int[] range2 = new int[] { 200, 800 };
}
```

**LargeStep Guidelines:**
- Use 4-10 major ticks for optimal readability
- Calculate: LargeStep ≈ (Max - Min) / 5
- For 0-100: Use 10, 20, or 25
- For 0-1000: Use 100, 200, or 250

### SmallStep Property

Adds intermediate tick marks between large ticks:

```razor
<SfSlider @bind-Value="@temperatureRange"
          Type="SliderType.Range"
          Min="0"
          Max="100"
          Step="1">
    <SliderTicks Placement="Placement.After" 
                 LargeStep="20"
                 SmallStep="5"
                 ShowSmallTicks="true">
    </SliderTicks>
</SfSlider>

@code {
    private int[] temperatureRange = new int[] { 20, 80 };
}
```

**SmallStep Behavior:**
- Only visible when `ShowSmallTicks="true"`
- Appears between large tick marks
- Does not display labels
- Helps users identify intermediate values
- SmallStep should divide evenly into LargeStep

### ShowSmallTicks Property

Controls visibility of small tick marks:

```razor
<!-- With small ticks -->
<SfSlider @bind-Value="@range1"
          Type="SliderType.Range"
          Min="0"
          Max="100">
    <SliderTicks Placement="Placement.After" 
                 LargeStep="20"
                 SmallStep="5"
                 ShowSmallTicks="true">
    </SliderTicks>
</SfSlider>

<!-- Without small ticks (cleaner appearance) -->
<SfSlider @bind-Value="@range2"
          Type="SliderType.Range"
          Min="0"
          Max="100">
    <SliderTicks Placement="Placement.After" 
                 LargeStep="20"
                 ShowSmallTicks="false">
    </SliderTicks>
</SfSlider>

@code {
    private int[] range1 = new int[] { 30, 70 };
    private int[] range2 = new int[] { 30, 70 };
}
```

## Tick Placement Options

### Placement Property Values

Control where tick marks appear:

```razor
<!-- Ticks after (below/right of) the slider -->
<SfSlider @bind-Value="@range1"
          Type="SliderType.Range"
          Min="0"
          Max="100">
    <SliderTicks Placement="Placement.After" LargeStep="20"></SliderTicks>
</SfSlider>

<!-- Ticks before (above/left of) the slider -->
<SfSlider @bind-Value="@range2"
          Type="SliderType.Range"
          Min="0"
          Max="100">
    <SliderTicks Placement="Placement.Before" LargeStep="20"></SliderTicks>
</SfSlider>

<!-- Ticks on both sides -->
<SfSlider @bind-Value="@range3"
          Type="SliderType.Range"
          Min="0"
          Max="100">
    <SliderTicks Placement="Placement.Both" LargeStep="20"></SliderTicks>
</SfSlider>

@code {
    private int[] range1 = new int[] { 30, 70 };
    private int[] range2 = new int[] { 30, 70 };
    private int[] range3 = new int[] { 30, 70 };
}
```

**Placement Guidelines:**
- `Placement.After`: Default, ticks below horizontal slider or right of vertical
- `Placement.Before`: Ticks above horizontal slider or left of vertical
- `Placement.Both`: Ticks on both sides, use for emphasis or symmetry
- Choose based on layout space and design requirements

## Tick Label Formatting

### Format Property

Format tick labels using standard .NET format strings:

```razor
<!-- Currency format -->
<SfSlider @bind-Value="@priceRange"
          Type="SliderType.Range"
          Min="0"
          Max="1000">
    <SliderTicks Placement="Placement.After" LargeStep="200" Format="C0"></SliderTicks>
</SfSlider>

<!-- Percentage format -->
<SfSlider @bind-Value="@percentRange"
          Type="SliderType.Range"
          Min="0"
          Max="100">
    <SliderTicks Placement="Placement.After" LargeStep="20" Format="P0"></SliderTicks>
</SfSlider>

<!-- Decimal format with 1 decimal place -->
<SfSlider @bind-Value="@tempRange"
          Type="SliderType.Range"
          Min="0.0"
          Max="50.0">
    <SliderTicks Placement="Placement.After" LargeStep="10" Format="N1"></SliderTicks>
</SfSlider>

@code {
    private int[] priceRange = new int[] { 200, 800 };
    private int[] percentRange = new int[] { 20, 80 };
    private double[] tempRange = new double[] { 10.5, 35.5 };
}
```

**Common Format Strings:**
- `C0`: Currency without decimals ($1,000)
- `C2`: Currency with 2 decimals ($1,000.00)
- `N0`: Number without decimals (1,000)
- `N1`: Number with 1 decimal (1,000.0)
- `P0`: Percentage without decimals (50%)
- `F1`: Fixed-point with 1 decimal (50.5)

### Custom Format Patterns

```razor
<!-- Temperature with unit -->
<SfSlider @bind-Value="@temperatureRange"
          Type="SliderType.Range"
          Min="0"
          Max="100">
    <SliderTicks Placement="Placement.After" LargeStep="20" Format="##°C"></SliderTicks>
</SfSlider>

<!-- Distance with unit -->
<SfSlider @bind-Value="@distanceRange"
          Type="SliderType.Range"
          Min="0"
          Max="500">
    <SliderTicks Placement="Placement.After" LargeStep="100" Format="## km"></SliderTicks>
</SfSlider>

@code {
    private int[] temperatureRange = new int[] { 20, 80 };
    private int[] distanceRange = new int[] { 50, 300 };
}
```

## SliderTooltip Configuration

### Basic Tooltip Setup

Add `SliderTooltip` to display current handle values:

```razor
<SfSlider @bind-Value="@rangeValues"
          Type="SliderType.Range"
          Min="0"
          Max="100">
    <SliderTooltip IsVisible="true"></SliderTooltip>
</SfSlider>

@code {
    private int[] rangeValues = new int[] { 30, 70 };
}
```

**Key Points:**
- `SliderTooltip` is a child component of `SfSlider`
- `IsVisible="true"` enables tooltip display
- Shows value for each handle independently
- Appears during user interaction

### Tooltip with Formatting

```razor
<SfSlider @bind-Value="@priceRange"
          Type="SliderType.Range"
          Min="0"
          Max="5000"
          Step="50">
    <SliderTooltip IsVisible="true" Format="C0" ShowOn="TooltipShowOn.Always"></SliderTooltip>
</SfSlider>

@code {
    private int[] priceRange = new int[] { 500, 3000 };
}
```

## Tooltip Visibility Modes

### ShowOn Property

Control when tooltips appear:

```razor
<!-- Always visible -->
<SfSlider @bind-Value="@range1"
          Type="SliderType.Range"
          Min="0"
          Max="100">
    <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
</SfSlider>

<!-- Show on hover -->
<SfSlider @bind-Value="@range2"
          Type="SliderType.Range"
          Min="0"
          Max="100">
    <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Hover"></SliderTooltip>
</SfSlider>

<!-- Show on focus (keyboard navigation) -->
<SfSlider @bind-Value="@range3"
          Type="SliderType.Range"
          Min="0"
          Max="100">
    <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Focus"></SliderTooltip>
</SfSlider>

<!-- Auto (show during drag) -->
<SfSlider @bind-Value="@range4"
          Type="SliderType.Range"
          Min="0"
          Max="100">
    <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Auto"></SliderTooltip>
</SfSlider>

@code {
    private int[] range1 = new int[] { 30, 70 };
    private int[] range2 = new int[] { 30, 70 };
    private int[] range3 = new int[] { 30, 70 };
    private int[] range4 = new int[] { 30, 70 };
}
```

**ShowOn Options:**
- `TooltipShowOn.Always`: Tooltip always visible (best for clarity)
- `TooltipShowOn.Hover`: Shows on mouse hover (clean UI)
- `TooltipShowOn.Focus`: Shows when handle has focus (accessibility)
- `TooltipShowOn.Auto`: Shows during dragging (default behavior)

### Tooltip Placement

```razor
<SfSlider @bind-Value="@rangeValues"
          Type="SliderType.Range"
          Min="0"
          Max="100">
    <SliderTooltip IsVisible="true" 
                   Placement="TooltipPlacement.Before"
                   ShowOn="TooltipShowOn.Always">
    </SliderTooltip>
</SfSlider>

@code {
    private int[] rangeValues = new int[] { 30, 70 };
}
```

**Placement Options:**
- `TooltipPlacement.Before`: Above horizontal slider, left of vertical
- `TooltipPlacement.After`: Below horizontal slider, right of vertical

## Tooltip Formatting

### Standard Format Strings

```razor
<!-- Currency tooltip -->
<SfSlider @bind-Value="@priceRange"
          Type="SliderType.Range"
          Min="0"
          Max="10000">
    <SliderTooltip IsVisible="true" Format="C0" ShowOn="TooltipShowOn.Always"></SliderTooltip>
</SfSlider>

<!-- Percentage tooltip -->
<SfSlider @bind-Value="@completionRange"
          Type="SliderType.Range"
          Min="0"
          Max="100">
    <SliderTooltip IsVisible="true" Format="N0" ShowOn="TooltipShowOn.Always"></SliderTooltip>
</SfSlider>

<!-- Temperature with decimals -->
<SfSlider @bind-Value="@tempRange"
          Type="SliderType.Range"
          Min="0.0"
          Max="50.0">
    <SliderTooltip IsVisible="true" Format="N1" ShowOn="TooltipShowOn.Always"></SliderTooltip>
</SfSlider>

@code {
    private int[] priceRange = new int[] { 2000, 8000 };
    private int[] completionRange = new int[] { 25, 75 };
    private double[] tempRange = new double[] { 15.5, 28.5 };
}
```

## Custom Tooltip Templates

### Template-Based Tooltips

While SliderTooltip provides built-in formatting, you can use `ChildContent` for advanced scenarios:

```razor
<SfSlider @bind-Value="@dateRange"
          Type="SliderType.Range"
          Min="1"
          Max="31">
    <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always">
        <ChildContent>
            <div class="custom-tooltip">
                Day @context
            </div>
        </ChildContent>
    </SliderTooltip>
</SfSlider>

@code {
    private int[] dateRange = new int[] { 5, 25 };
}
```

## Combined Patterns

### Pattern 1: Price Range with Ticks and Tooltip

```razor
<div class="price-filter">
    <h4>Price Range</h4>
    <SfSlider @bind-Value="@priceRange"
              Type="SliderType.Range"
              Min="0"
              Max="5000"
              Step="50">
        <SliderTicks Placement="Placement.After" 
                     LargeStep="1000"
                     SmallStep="250"
                     ShowSmallTicks="true"
                     Format="C0">
        </SliderTicks>
        <SliderTooltip IsVisible="true" 
                       Format="C0" 
                       ShowOn="TooltipShowOn.Always"
                       Placement="TooltipPlacement.Before">
        </SliderTooltip>
    </SfSlider>
    <p>Selected: @priceRange[0].ToString("C0") - @priceRange[1].ToString("C0")</p>
</div>

@code {
    private int[] priceRange = new int[] { 500, 3000 };
}
```

### Pattern 2: Temperature Range with Visual Feedback

```razor
<div class="temperature-control">
    <h4>Temperature Range (°C)</h4>
    <SfSlider @bind-Value="@temperatureRange"
              Type="SliderType.Range"
              Min="0.0"
              Max="50.0"
              Step="0.5">
        <SliderTicks Placement="Placement.Both" 
                     LargeStep="10"
                     SmallStep="2.5"
                     ShowSmallTicks="true"
                     Format="N1">
        </SliderTicks>
        <SliderTooltip IsVisible="true" 
                       Format="N1" 
                       ShowOn="TooltipShowOn.Hover">
        </SliderTooltip>
    </SfSlider>
</div>

@code {
    private double[] temperatureRange = new double[] { 18.0, 26.5 };
}
```

### Pattern 3: Age Range Demographic Filter

```razor
<div class="age-filter">
    <h4>Target Age Range</h4>
    <SfSlider @bind-Value="@ageRange"
              Type="SliderType.Range"
              Min="0"
              Max="100"
              Step="1">
        <SliderTicks Placement="Placement.After" 
                     LargeStep="10"
                     ShowSmallTicks="false">
        </SliderTicks>
        <SliderTooltip IsVisible="true" 
                       ShowOn="TooltipShowOn.Always">
        </SliderTooltip>
    </SfSlider>
    <p>Ages: @ageRange[0] - @ageRange[1] years</p>
</div>

@code {
    private int[] ageRange = new int[] { 18, 65 };
}
```

### Pattern 4: Percentage Range with Clean UI

```razor
<div class="completion-range">
    <h4>Completion Range</h4>
    <SfSlider @bind-Value="@completionRange"
              Type="SliderType.Range"
              Min="0"
              Max="100"
              Step="5">
        <SliderTicks Placement="Placement.After" 
                     LargeStep="25"
                     SmallStep="5"
                     ShowSmallTicks="false"
                     Format="N0">
        </SliderTicks>
        <SliderTooltip IsVisible="true" 
                       Format="N0" 
                       ShowOn="TooltipShowOn.Auto">
        </SliderTooltip>
    </SfSlider>
</div>

@code {
    private int[] completionRange = new int[] { 20, 80 };
}
```

## Best Practices

### Tick Configuration Guidelines

1. **Optimal Tick Count**: Use 5-10 large ticks for readability
2. **Small Tick Ratio**: SmallStep should be 1/4 to 1/2 of LargeStep
3. **Format Consistency**: Match tick and tooltip formats
4. **Placement Choice**: Use `After` for most layouts, `Both` for emphasis
5. **Clean UI**: Disable small ticks for simpler appearance

### Tooltip Best Practices

1. **Always Visible for Filters**: Use `ShowOn.Always` for filter scenarios
2. **Auto for General Use**: Use `ShowOn.Auto` to reduce visual clutter
3. **Format with Context**: Add units (°C, km, $) for clarity
4. **Consistent Formatting**: Match tooltip format with displayed range text

## Troubleshooting

### Ticks Not Visible
**Issue:** SliderTicks component added but no ticks appear.

**Solution:** Ensure CSS theme is loaded and LargeStep is set:
```razor
<SliderTicks Placement="Placement.After" LargeStep="20"></SliderTicks>
```

### Tooltip Not Showing
**Issue:** SliderTooltip component added but tooltip doesn't appear.

**Solution:** Set `IsVisible="true"`:
```razor
<SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
```

### Format String Not Applied
**Issue:** Format property ignored or displays incorrectly.

**Solution:** Use valid .NET format strings and ensure value type matches:
```razor
<!-- For integers -->
<SliderTicks Format="N0"></SliderTicks>  <!-- No decimals -->

<!-- For doubles -->
<SliderTicks Format="N2"></SliderTicks>  <!-- 2 decimals -->
```

## Next Steps

- **Color Ranges** - Add visual feedback with colored segments
- **Limits** - Restrict handle movement within specific bounds
- **Events** - Handle tick render events for advanced customization
- **Customization** - Apply custom CSS for unique styling
