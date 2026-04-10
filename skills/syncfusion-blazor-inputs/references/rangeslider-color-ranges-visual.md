# RangeSlider - Color Ranges and Visual Indication

## Table of Contents
- [Overview](#overview)
- [SliderColorRanges Configuration](#slidercolorranges-configuration)
- [ColorRange Component](#colorrange-component)
- [Multiple Color Segments](#multiple-color-segments)
- [Common Use Cases](#common-use-cases)
- [Color Customization](#color-customization)
- [Accessibility Considerations](#accessibility-considerations)

## Overview

Color ranges provide visual feedback by applying different colors to segments of the slider track. This feature helps users quickly understand value zones, such as temperature ranges (cold/warm/hot), risk levels (low/medium/high), or price tiers (budget/standard/premium). Color ranges enhance usability by adding intuitive visual cues to numeric values.

## SliderColorRanges Configuration

### Basic Color Range Setup

Add `SliderColorRanges` component with one or more `ColorRange` child components:

```razor
@using Syncfusion.Blazor.Inputs

<SfSlider @bind-Value="@temperatureRange"
          Type="SliderType.Range"
          Min="0"
          Max="100">
    <SliderColorRanges>
        <ColorRange Start="0" End="40" Color="#0078D4"></ColorRange>
        <ColorRange Start="40" End="70" Color="#FFB900"></ColorRange>
        <ColorRange Start="70" End="100" Color="#D13438"></ColorRange>
    </SliderColorRanges>
</SfSlider>

@code {
    private int[] temperatureRange = new int[] { 30, 80 };
}
```

**Key Points:**
- `SliderColorRanges` is a child component of `SfSlider`
- Contains one or more `ColorRange` components
- Each `ColorRange` defines a value segment and its color
- Colors are applied to the slider track for visual indication

## ColorRange Component

### ColorRange Properties

Each `ColorRange` component requires three properties:

```razor
<ColorRange Start="0" End="50" Color="#4CAF50"></ColorRange>
```

**Properties:**
- `Start`: Beginning value of the color segment (inclusive)
- `End`: Ending value of the color segment (inclusive)
- `Color`: Hex color code or CSS color name for the segment

### Single Color Range Example

```razor
<div class="battery-level">
    <h4>Battery Level</h4>
    <SfSlider @bind-Value="@batteryRange"
              Type="SliderType.Range"
              Min="0"
              Max="100">
        <SliderColorRanges>
            <ColorRange Start="0" End="20" Color="#D32F2F"></ColorRange>
            <ColorRange Start="20" End="100" Color="#388E3C"></ColorRange>
        </SliderColorRanges>
        <SliderTicks Placement="Placement.After" LargeStep="20"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Range: @batteryRange[0]% - @batteryRange[1]%</p>
</div>

@code {
    private int[] batteryRange = new int[] { 30, 90 };
}
```

## Multiple Color Segments

### Three-Zone Temperature Range

```razor
<div class="temperature-zones">
    <h4>Temperature Control (°C)</h4>
    <SfSlider @bind-Value="@temperatureRange"
              Type="SliderType.Range"
              Min="-20"
              Max="50">
        <SliderColorRanges>
            <ColorRange Start="-20" End="10" Color="#2196F3"></ColorRange>
            <ColorRange Start="10" End="25" Color="#4CAF50"></ColorRange>
            <ColorRange Start="25" End="50" Color="#FF5722"></ColorRange>
        </SliderColorRanges>
        <SliderTicks Placement="Placement.After" LargeStep="10" Format="N0"></SliderTicks>
        <SliderTooltip IsVisible="true" Format="N0" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <div class="legend">
        <span><span class="color-box" style="background: #2196F3;"></span> Cold (-20 to 10°C)</span>
        <span><span class="color-box" style="background: #4CAF50;"></span> Comfortable (10 to 25°C)</span>
        <span><span class="color-box" style="background: #FF5722;"></span> Hot (25 to 50°C)</span>
    </div>
</div>

@code {
    private int[] temperatureRange = new int[] { 18, 26 };
}
```

### Five-Zone Risk Indicator

```razor
<div class="risk-assessment">
    <h4>Risk Level Assessment</h4>
    <SfSlider @bind-Value="@riskRange"
              Type="SliderType.Range"
              Min="0"
              Max="100">
        <SliderColorRanges>
            <ColorRange Start="0" End="20" Color="#4CAF50"></ColorRange>
            <ColorRange Start="20" End="40" Color="#8BC34A"></ColorRange>
            <ColorRange Start="40" End="60" Color="#FFB900"></ColorRange>
            <ColorRange Start="60" End="80" Color="#FF9800"></ColorRange>
            <ColorRange Start="80" End="100" Color="#D32F2F"></ColorRange>
        </SliderColorRanges>
        <SliderTicks Placement="Placement.After" LargeStep="20"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Risk Range: @riskRange[0] - @riskRange[1]</p>
</div>

@code {
    private int[] riskRange = new int[] { 25, 75 };
}
```

**CSS for Legend:**
```css
.legend {
    display: flex;
    gap: 15px;
    margin-top: 10px;
    font-size: 14px;
}

.color-box {
    display: inline-block;
    width: 16px;
    height: 16px;
    margin-right: 5px;
    border-radius: 2px;
    vertical-align: middle;
}
```

## Common Use Cases

### Use Case 1: Price Tier Filtering

```razor
<div class="price-tiers">
    <h4>Product Price Range</h4>
    <SfSlider @bind-Value="@priceRange"
              Type="SliderType.Range"
              Min="0"
              Max="5000"
              Step="50">
        <SliderColorRanges>
            <ColorRange Start="0" End="1000" Color="#4CAF50"></ColorRange>
            <ColorRange Start="1000" End="3000" Color="#2196F3"></ColorRange>
            <ColorRange Start="3000" End="5000" Color="#9C27B0"></ColorRange>
        </SliderColorRanges>
        <SliderTicks Placement="Placement.After" LargeStep="1000" Format="C0"></SliderTicks>
        <SliderTooltip IsVisible="true" Format="C0" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <div class="tier-info">
        <p><strong>Budget:</strong> $0 - $1,000</p>
        <p><strong>Standard:</strong> $1,000 - $3,000</p>
        <p><strong>Premium:</strong> $3,000 - $5,000</p>
    </div>
</div>

@code {
    private int[] priceRange = new int[] { 500, 2500 };
}
```

### Use Case 2: Air Quality Index (AQI)

```razor
<div class="aqi-monitor">
    <h4>Air Quality Index Range</h4>
    <SfSlider @bind-Value="@aqiRange"
              Type="SliderType.Range"
              Min="0"
              Max="500">
        <SliderColorRanges>
            <ColorRange Start="0" End="50" Color="#00E400"></ColorRange>
            <ColorRange Start="50" End="100" Color="#FFFF00"></ColorRange>
            <ColorRange Start="100" End="150" Color="#FF7E00"></ColorRange>
            <ColorRange Start="150" End="200" Color="#FF0000"></ColorRange>
            <ColorRange Start="200" End="300" Color="#8F3F97"></ColorRange>
            <ColorRange Start="300" End="500" Color="#7E0023"></ColorRange>
        </SliderColorRanges>
        <SliderTicks Placement="Placement.After" LargeStep="100"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <div class="aqi-legend">
        <span style="color: #00E400;">⬤ Good (0-50)</span>
        <span style="color: #FFFF00;">⬤ Moderate (51-100)</span>
        <span style="color: #FF7E00;">⬤ Unhealthy for Sensitive (101-150)</span>
        <span style="color: #FF0000;">⬤ Unhealthy (151-200)</span>
        <span style="color: #8F3F97;">⬤ Very Unhealthy (201-300)</span>
        <span style="color: #7E0023;">⬤ Hazardous (301-500)</span>
    </div>
</div>

@code {
    private int[] aqiRange = new int[] { 50, 150 };
}
```

### Use Case 3: Performance Rating

```razor
<div class="performance-rating">
    <h4>Performance Score Range</h4>
    <SfSlider @bind-Value="@performanceRange"
              Type="SliderType.Range"
              Min="0"
              Max="100">
        <SliderColorRanges>
            <ColorRange Start="0" End="40" Color="#D32F2F"></ColorRange>
            <ColorRange Start="40" End="60" Color="#FF9800"></ColorRange>
            <ColorRange Start="60" End="80" Color="#2196F3"></ColorRange>
            <ColorRange Start="80" End="100" Color="#4CAF50"></ColorRange>
        </SliderColorRanges>
        <SliderTicks Placement="Placement.After" LargeStep="20"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <div class="rating-labels">
        <span>Poor</span>
        <span>Fair</span>
        <span>Good</span>
        <span>Excellent</span>
    </div>
</div>

@code {
    private int[] performanceRange = new int[] { 50, 90 };
}
```

**CSS for Rating Labels:**
```css
.rating-labels {
    display: flex;
    justify-content: space-between;
    margin-top: 10px;
    font-size: 12px;
    color: #666;
}
```

### Use Case 4: Heart Rate Zones

```razor
<div class="heart-rate-zones">
    <h4>Target Heart Rate Zone (BPM)</h4>
    <SfSlider @bind-Value="@heartRateRange"
              Type="SliderType.Range"
              Min="60"
              Max="200">
        <SliderColorRanges>
            <ColorRange Start="60" End="100" Color="#90CAF9"></ColorRange>
            <ColorRange Start="100" End="130" Color="#66BB6A"></ColorRange>
            <ColorRange Start="130" End="160" Color="#FFB74D"></ColorRange>
            <ColorRange Start="160" End="180" Color="#FF8A65"></ColorRange>
            <ColorRange Start="180" End="200" Color="#E57373"></ColorRange>
        </SliderColorRanges>
        <SliderTicks Placement="Placement.After" LargeStep="20"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <div class="zone-info">
        <p>Resting: 60-100 | Fat Burn: 100-130 | Cardio: 130-160 | Peak: 160-200</p>
    </div>
</div>

@code {
    private int[] heartRateRange = new int[] { 120, 155 };
}
```

## Color Customization

### Using CSS Color Names

```razor
<SliderColorRanges>
    <ColorRange Start="0" End="33" Color="green"></ColorRange>
    <ColorRange Start="33" End="66" Color="yellow"></ColorRange>
    <ColorRange Start="66" End="100" Color="red"></ColorRange>
</SliderColorRanges>
```

### Using Hex Color Codes

```razor
<SliderColorRanges>
    <ColorRange Start="0" End="33" Color="#4CAF50"></ColorRange>
    <ColorRange Start="33" End="66" Color="#FFB900"></ColorRange>
    <ColorRange Start="66" End="100" Color="#D32F2F"></ColorRange>
</SliderColorRanges>
```

### Using RGB/RGBA Colors

```razor
<SliderColorRanges>
    <ColorRange Start="0" End="50" Color="rgb(76, 175, 80)"></ColorRange>
    <ColorRange Start="50" End="100" Color="rgba(211, 47, 47, 0.8)"></ColorRange>
</SliderColorRanges>
```

### Brand Color Integration

```razor
@code {
    private string LowRiskColor = "#00C851";    // Success green
    private string MediumRiskColor = "#FFB900";  // Warning yellow
    private string HighRiskColor = "#FF4444";    // Danger red
}

<SliderColorRanges>
    <ColorRange Start="0" End="40" Color="@LowRiskColor"></ColorRange>
    <ColorRange Start="40" End="70" Color="@MediumRiskColor"></ColorRange>
    <ColorRange Start="70" End="100" Color="@HighRiskColor"></ColorRange>
</SliderColorRanges>
```

## Accessibility Considerations

### Color and Text Combination

Always combine color ranges with text labels or legends:

```razor
<div class="accessible-slider">
    <label id="temp-label">Temperature Range (°C)</label>
    <SfSlider @bind-Value="@temperatureRange"
              Type="SliderType.Range"
              Min="0"
              Max="50"
              aria-labelledby="temp-label">
        <SliderColorRanges>
            <ColorRange Start="0" End="18" Color="#2196F3"></ColorRange>
            <ColorRange Start="18" End="26" Color="#4CAF50"></ColorRange>
            <ColorRange Start="26" End="50" Color="#FF5722"></ColorRange>
        </SliderColorRanges>
        <SliderTicks Placement="Placement.After" LargeStep="10"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <div class="accessible-legend">
        <div><span class="sr-only">Cold zone:</span> ❄️ Below 18°C</div>
        <div><span class="sr-only">Comfortable zone:</span> ✓ 18-26°C</div>
        <div><span class="sr-only">Hot zone:</span> 🔥 Above 26°C</div>
    </div>
</div>

@code {
    private int[] temperatureRange = new int[] { 20, 24 };
}
```

### Color Contrast Requirements

Ensure sufficient contrast between:
- Color ranges and slider background
- Tooltip text and tooltip background
- Tick labels and page background

```css
/* High contrast for better visibility */
.e-slider-container .e-slider {
    border: 1px solid #ccc;  /* Adds definition to slider track */
}

.e-slider-tooltip {
    font-weight: 600;  /* Bold tooltip text for readability */
    border: 2px solid #333;  /* Strong border for tooltip */
}
```

### Don't Rely Solely on Color

Combine color with other visual cues:

```razor
<div class="status-slider">
    <SfSlider @bind-Value="@statusRange"
              Type="SliderType.Range"
              Min="0"
              Max="100">
        <SliderColorRanges>
            <ColorRange Start="0" End="33" Color="#4CAF50"></ColorRange>
            <ColorRange Start="33" End="66" Color="#FFB900"></ColorRange>
            <ColorRange Start="66" End="100" Color="#D32F2F"></ColorRange>
        </SliderColorRanges>
        <SliderTicks Placement="Placement.After" LargeStep="33"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <div class="status-icons">
        <span>✓ Good (0-33)</span>
        <span>⚠ Warning (34-66)</span>
        <span>✗ Critical (67-100)</span>
    </div>
</div>

@code {
    private int[] statusRange = new int[] { 20, 80 };
}
```

## Best Practices

### Color Range Guidelines

1. **Use 3-5 Segments**: Too many colors become confusing
2. **Logical Progression**: Order colors by intensity or severity
3. **Standard Conventions**: 
   - Red = Danger/Hot/High
   - Green = Safe/Good/Low
   - Yellow/Orange = Warning/Medium
4. **Consistent Meanings**: Use same color scheme across your application
5. **Test Accessibility**: Verify colors work for colorblind users

### Combining with Other Features

```razor
<!-- Complete example with all features -->
<div class="complete-range-slider">
    <h4>Budget Allocation</h4>
    <SfSlider @bind-Value="@budgetRange"
              Type="SliderType.Range"
              Min="0"
              Max="100000"
              Step="1000">
        <SliderColorRanges>
            <ColorRange Start="0" End="30000" Color="#4CAF50"></ColorRange>
            <ColorRange Start="30000" End="70000" Color="#2196F3"></ColorRange>
            <ColorRange Start="70000" End="100000" Color="#9C27B0"></ColorRange>
        </SliderColorRanges>
        <SliderTicks Placement="Placement.After" 
                     LargeStep="20000" 
                     SmallStep="10000"
                     ShowSmallTicks="true"
                     Format="C0">
        </SliderTicks>
        <SliderTooltip IsVisible="true" Format="C0" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <div class="budget-categories">
        <span>💚 Low Budget</span>
        <span>💙 Mid Budget</span>
        <span>💜 High Budget</span>
    </div>
</div>

@code {
    private int[] budgetRange = new int[] { 20000, 75000 };
}
```

## Troubleshooting

### Colors Not Displaying
**Issue:** ColorRange components added but colors don't appear on slider.

**Solution:** Ensure:
1. CSS theme is loaded
2. Start/End values are within slider Min/Max bounds
3. `SliderColorRanges` is a direct child of `SfSlider`

### Overlapping Color Ranges
**Issue:** Multiple colors appear in same segment.

**Solution:** Define non-overlapping ranges:
```razor
<!-- ✅ Correct - No overlap -->
<ColorRange Start="0" End="50" Color="#4CAF50"></ColorRange>
<ColorRange Start="50" End="100" Color="#D32F2F"></ColorRange>

<!-- ❌ Incorrect - Overlapping at 50 -->
<ColorRange Start="0" End="50" Color="#4CAF50"></ColorRange>
<ColorRange Start="49" End="100" Color="#D32F2F"></ColorRange>
```

### Gap Between Color Ranges
**Issue:** White/gray gaps appear between color segments.

**Solution:** Ensure End of one range matches Start of next:
```razor
<!-- ✅ Correct - Continuous -->
<ColorRange Start="0" End="33" Color="green"></ColorRange>
<ColorRange Start="33" End="66" Color="yellow"></ColorRange>
<ColorRange Start="66" End="100" Color="red"></ColorRange>
```

## Next Steps

- **Limits** - Restrict handle movement within specific bounds
- **Events** - React to range changes based on color zones
- **Customization** - Apply custom styling to enhance visual appearance
- **Orientation** - Use color ranges with vertical sliders
