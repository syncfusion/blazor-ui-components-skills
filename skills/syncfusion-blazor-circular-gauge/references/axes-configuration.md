# Axes Configuration

## Table of Contents
- [Overview](#overview)
- [Axis Customization](#axis-customization)
- [Minimum and Maximum](#minimum-and-maximum)
- [Start and End Angles](#start-and-end-angles)
- [Axis Radius](#axis-radius)
- [Ticks Configuration](#ticks-configuration)
- [Labels Configuration](#labels-configuration)
- [Axis Direction](#axis-direction)
- [Multiple Axes](#multiple-axes)
- [Common Use Cases](#common-use-cases)

## Overview

By default, the Circular Gauge displays with an axis. Each axis can contain its own ranges, pointers, and annotations. Understanding axis configuration is essential for creating effective circular gauge visualizations.

**Key Capabilities:**
- Customize axis appearance (line width, color, background)
- Set minimum and maximum scale values
- Configure start/end angles for custom arc shapes
- Control axis radius in pixels or percentage
- Configure major and minor ticks
- Customize axis labels (format, position, styling)
- Set axis direction (clockwise/anticlockwise)
- Implement multiple axes on a single gauge

## Axis Customization

### Basic Axis Styling

Customize the axis line width and color using `CircularGaugeAxisLineStyle`, and set the axis background using the `Background` property:

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Background="rgba(0, 128, 128, 0.3)">
            <CircularGaugeAxisLineStyle Width="2" Color="red">
            </CircularGaugeAxisLineStyle>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Properties:**
- `Width`: Axis line width (default: 2)
- `Color`: Axis line color (e.g., "red", "#FF0000", "rgb(255,0,0)")
- `Background`: Axis background color (supports rgba for transparency)

**Use Case:** Highlight the axis with colored backgrounds for visual emphasis or brand consistency.

## Minimum and Maximum

Define the scale range using `Minimum` and `Maximum` properties:

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="50" Maximum="250">
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Default Values:**
- `Minimum`: 0
- `Maximum`: 100

**Examples:**
- **Temperature gauge**: Minimum="-20", Maximum="50"
- **Speedometer**: Minimum="0", Maximum="200"
- **Percentage gauge**: Minimum="0", Maximum="100"
- **Pressure gauge**: Minimum="0", Maximum="3000"

**Important:** Pointer values and range values must fall within the minimum-maximum range.

## Start and End Angles

Control the sweep angle of the gauge using `StartAngle` and `EndAngle` properties. The gauge can sweep from 0 to 360 degrees.

### Default Angles

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge>
    <CircularGaugeAxes>
        <!-- Default: StartAngle=200, EndAngle=160 -->
        <CircularGaugeAxis>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Custom Angles

#### Semi-Circle Gauge (Bottom Half)

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis StartAngle="270" EndAngle="90">
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

#### Semi-Circle Gauge (Top Half)

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis StartAngle="90" EndAngle="270">
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

#### Quarter-Circle Gauge

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis StartAngle="180" EndAngle="270">
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

#### Full-Circle Gauge

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis StartAngle="0" EndAngle="360">
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

#### Speedometer-Style Gauge

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis StartAngle="220" EndAngle="140">
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Angle Reference:**
- 0° = 3 o'clock position (right)
- 90° = 6 o'clock position (bottom)
- 180° = 9 o'clock position (left)
- 270° = 12 o'clock position (top)

**Use Cases:**
- **Speedometer**: 220° to 140° (classic car dashboard look)
- **Temperature indicator**: 270° to 90° (bottom semi-circle)
- **Progress indicator**: 0° to 360° (full circle)
- **Battery level**: 180° to 0° (right quarter-circle)

## Axis Radius

Control the size of the axis using the `Radius` property. The radius can be specified in **pixels** or **percentage**.

### Radius in Pixels

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Radius="150px">
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**When to use pixels:**
- Fixed-size gauges that don't need to scale
- Precise control over gauge dimensions
- Non-responsive layouts

### Radius in Percentage

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Radius="50%">
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Percentage Behavior:**
- Percentage is calculated relative to the gauge's available size
- "50%" renders the gauge at half the available space
- "80%" is a common value for good spacing
- "100%" fills the entire available area

**When to use percentage:**
- Responsive designs that adapt to container size
- Dashboards with dynamic layouts
- Mobile-friendly applications

**Default:** Auto-calculated based on available size

**Example: Multiple Axes with Different Radii**

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Radius="80%">
            <!-- Outer axis -->
        </CircularGaugeAxis>
        <CircularGaugeAxis Radius="50%">
            <!-- Inner axis -->
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Ticks Configuration

Ticks are the small lines on the axis that mark scale divisions. There are two types: **major ticks** and **minor ticks**.

### Major Ticks

Major ticks mark significant scale divisions:

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeAxisMajorTicks 
                Interval="10" 
                Color="red" 
                Height="10" 
                Width="3">
            </CircularGaugeAxisMajorTicks>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Properties:**
- `Interval`: Value interval between ticks (default: auto-calculated)
- `Height`: Tick length in pixels
- `Width`: Tick thickness in pixels
- `Color`: Tick color
- `Position`: Inside, Outside, or Cross (default: Inside)
- `Offset`: Distance from axis line

### Minor Ticks

Minor ticks provide finer scale divisions between major ticks:

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeAxisMinorTicks 
                Interval="5" 
                Color="green" 
                Height="5" 
                Width="2">
            </CircularGaugeAxisMinorTicks>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Complete Tick Configuration Example

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeAxisMajorTicks 
                Interval="10" 
                Color="#333" 
                Height="12" 
                Width="2"
                Position="Position.Inside">
            </CircularGaugeAxisMajorTicks>
            <CircularGaugeAxisMinorTicks 
                Interval="2" 
                Color="#999" 
                Height="6" 
                Width="1"
                Position="Position.Inside">
            </CircularGaugeAxisMinorTicks>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Tick Positioning

Control tick placement using `Offset` and `Position` properties:

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeAxisMajorTicks 
                Position="Syncfusion.Blazor.CircularGauge.Position.Outside" 
                Offset="5"
                Height="10">
            </CircularGaugeAxisMajorTicks>
            <CircularGaugeAxisMinorTicks 
                Position="Syncfusion.Blazor.CircularGauge.Position.Outside" 
                Offset="5"
                Height="5">
            </CircularGaugeAxisMinorTicks>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Position Options:**
- `Position.Inside`: Ticks point inward (default)
- `Position.Outside`: Ticks point outward
- `Position.Cross`: Ticks cross the axis line

**Offset:**
- Positive values move ticks away from the axis
- Negative values move ticks toward the axis center
- Default: 0

### Hiding Ticks

To hide ticks, set `Height="0"`:

```cshtml
<CircularGaugeAxisMajorTicks Height="0"></CircularGaugeAxisMajorTicks>
<CircularGaugeAxisMinorTicks Height="0"></CircularGaugeAxisMinorTicks>
```

## Labels Configuration

Axis labels display the scale values. Customize them using `CircularGaugeAxisLabelStyle`.

### Basic Label Styling

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeAxisLabelStyle>
                <CircularGaugeAxisLabelFont 
                    Color="red" 
                    Size="20px" 
                    FontWeight="Bold"
                    FontFamily="Arial">
                </CircularGaugeAxisLabelFont>
            </CircularGaugeAxisLabelStyle>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Font Properties:**
- `Color`: Label text color
- `Size`: Font size (e.g., "12px", "16px")
- `FontWeight`: Normal, Bold, Lighter, Bolder
- `FontFamily`: Font name (e.g., "Arial", "Segoe UI")
- `FontStyle`: Normal, Italic, Oblique

### Label Positioning

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeAxisLabelStyle 
                Position="Syncfusion.Blazor.CircularGauge.Position.Outside" 
                Offset="10">
            </CircularGaugeAxisLabelStyle>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Position Options:**
- `Position.Inside`: Labels inside the axis (default)
- `Position.Outside`: Labels outside the axis
- `Position.Cross`: Labels on the axis line

**Offset:** Distance from ticks (default: 0)

### Auto-Angle Labels

Make labels follow the axis curve:

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeAxisLabelStyle AutoAngle="true">
            </CircularGaugeAxisLabelStyle>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**When to use:** Improves readability on gauges with non-standard angles or full circles.

### Label Formatting

#### Standard Format

Format labels using globalize patterns:

```cshtml
<CircularGaugeAxisLabelStyle Format="n2"></CircularGaugeAxisLabelStyle>
```

**Common Formats:**

| Format | Example Value | Result | Description |
|--------|--------------|--------|-------------|
| `n1` | 1000 | 1000.0 | 1 decimal place |
| `n2` | 1000 | 1000.00 | 2 decimal places |
| `n3` | 1000 | 1000.000 | 3 decimal places |
| `p1` | 0.01 | 1.0% | Percentage with 1 decimal |
| `p2` | 0.01 | 1.00% | Percentage with 2 decimals |
| `c1` | 1000 | $1,000.0 | Currency with 1 decimal |
| `c2` | 1000 | $1,000.00 | Currency with 2 decimals |

#### Custom Format with Units

```cshtml
<CircularGaugeAxisLabelStyle Format="{value}°C"></CircularGaugeAxisLabelStyle>
```

**Examples:**
- `"{value}°C"` → Temperature (20°C)
- `"{value} km/h"` → Speed (65 km/h)
- `"{value} PSI"` → Pressure (30 PSI)
- `"{value}%"` → Percentage (75%)
- `"${value}"` → Currency ($1000)

### Smart Labels (Hiding Overlapping Labels)

When the axis forms a complete circle (0°-360°), the first and last labels overlap. Hide one using `HiddenLabel`:

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis 
            Minimum="0" 
            Maximum="12" 
            StartAngle="0" 
            EndAngle="360">
            <CircularGaugeAxisLabelStyle HiddenLabel="HiddenLabel.First">
            </CircularGaugeAxisLabelStyle>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Options:**
- `HiddenLabel.First`: Hide the first label
- `HiddenLabel.Last`: Hide the last label
- `HiddenLabel.None`: Show all labels (default)

### Show Last Label

If the maximum value doesn't align with the tick interval, the last label is hidden by default. Force it to display:

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Maximum="100" ShowLastLabel="true">
            <CircularGaugeAxisMajorTicks Interval="30"></CircularGaugeAxisMajorTicks>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Example:** With Maximum="100" and Interval="30", labels appear at 0, 30, 60, 90. Setting `ShowLastLabel="true"` also shows 100.

### Hide Intersecting Labels

Automatically hide labels that overlap each other:

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis 
            Maximum="200" 
            StartAngle="270" 
            EndAngle="90" 
            HideIntersectingLabel="true">
            <CircularGaugeAxisMajorTicks Interval="4"></CircularGaugeAxisMajorTicks>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Use Case:** Prevents label clutter on gauges with many ticks or custom angles.

### Hiding Labels Completely

```cshtml
<CircularGaugeAxisLabelStyle>
    <CircularGaugeAxisLabelFont Size="0px"></CircularGaugeAxisLabelFont>
</CircularGaugeAxisLabelStyle>
```

## Axis Direction

Control whether the gauge progresses clockwise or counter-clockwise:

### Clockwise (Default)

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Direction="GaugeDirection.ClockWise">
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### AntiClockwise

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Direction="GaugeDirection.AntiClockWise">
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Use Cases:**
- **Countdown timers**: Use AntiClockWise for visual countdown effect
- **Reverse indicators**: Show decreasing values
- **Cultural preferences**: Some regions prefer counter-clockwise progression

## Multiple Axes

Add multiple axes to display different scales or measurement systems on a single gauge:

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <!-- First Axis: Speed in km/h -->
        <CircularGaugeAxis Minimum="0" Maximum="140" Radius="80%">
            <CircularGaugeAxisLabelStyle>
                <CircularGaugeAxisLabelFont Size="12px"></CircularGaugeAxisLabelFont>
            </CircularGaugeAxisLabelStyle>
            <CircularGaugePointers>
                <CircularGaugePointer Value="80" Type="PointerType.Needle">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
        
        <!-- Second Axis: Temperature in °C -->
        <CircularGaugeAxis Minimum="-20" Maximum="60" Radius="50%">
            <CircularGaugeAxisLabelStyle Position="Syncfusion.Blazor.CircularGauge.Position.Outside">
            </CircularGaugeAxisLabelStyle>
            <CircularGaugeAxisMajorTicks Position="Syncfusion.Blazor.CircularGauge.Position.Outside">
            </CircularGaugeAxisMajorTicks>
            <CircularGaugeAxisMinorTicks Position="Syncfusion.Blazor.CircularGauge.Position.Outside">
            </CircularGaugeAxisMinorTicks>
            <CircularGaugePointers>
                <CircularGaugePointer 
                    Value="22" 
                    Type="PointerType.Marker" 
                    MarkerShape="GaugeShape.InvertedTriangle"
                    MarkerHeight="20"
                    MarkerWidth="20">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Design Tips:**
- Use different radii to nest axes (outer and inner)
- Position labels and ticks differently (inside vs outside) to avoid overlap
- Use different pointer types for visual distinction
- Assign different colors to each axis's elements

**Use Cases:**
- **Dual measurement**: Speed (km/h) and fuel level on the same gauge
- **Temperature and humidity**: Two environmental metrics
- **Currency conversion**: Same value in two currencies
- **RPM and speed**: Engine RPM and vehicle speed

## Common Use Cases

### Clock-Style Gauge (12-Hour)

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis 
            Minimum="0" 
            Maximum="12" 
            StartAngle="0" 
            EndAngle="360">
            <CircularGaugeAxisLabelStyle HiddenLabel="HiddenLabel.First">
            </CircularGaugeAxisLabelStyle>
            <CircularGaugeAxisMajorTicks Interval="1" Height="10">
            </CircularGaugeAxisMajorTicks>
            <CircularGaugeAxisMinorTicks Interval="0.2" Height="5">
            </CircularGaugeAxisMinorTicks>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Temperature Gauge with Custom Labels

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="-30" Maximum="50" StartAngle="270" EndAngle="90">
            <CircularGaugeAxisLabelStyle Format="{value}°C" Position="Position.Outside">
            </CircularGaugeAxisLabelStyle>
            <CircularGaugeAxisMajorTicks Interval="10" Position="Position.Outside">
            </CircularGaugeAxisMajorTicks>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Percentage Gauge with Clean Design

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeAxisLabelStyle Format="{value}%">
                <CircularGaugeAxisLabelFont Size="14px" FontWeight="Bold">
                </CircularGaugeAxisLabelFont>
            </CircularGaugeAxisLabelStyle>
            <CircularGaugeAxisLineStyle Width="0"></CircularGaugeAxisLineStyle>
            <CircularGaugeAxisMajorTicks Height="0"></CircularGaugeAxisMajorTicks>
            <CircularGaugeAxisMinorTicks Height="0"></CircularGaugeAxisMinorTicks>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Speedometer with Outside Labels

```cshtml
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis 
            Minimum="0" 
            Maximum="200" 
            StartAngle="220" 
            EndAngle="140">
            <CircularGaugeAxisLabelStyle 
                Position="Syncfusion.Blazor.CircularGauge.Position.Outside" 
                AutoAngle="true">
                <CircularGaugeAxisLabelFont Size="12px" Color="#333">
                </CircularGaugeAxisLabelFont>
            </CircularGaugeAxisLabelStyle>
            <CircularGaugeAxisLineStyle Width="8" Color="#E0E0E0">
            </CircularGaugeAxisLineStyle>
            <CircularGaugeAxisMajorTicks 
                Interval="20" 
                Height="12" 
                Width="2" 
                Color="#666">
            </CircularGaugeAxisMajorTicks>
            <CircularGaugeAxisMinorTicks 
                Interval="5" 
                Height="6" 
                Width="1" 
                Color="#999">
            </CircularGaugeAxisMinorTicks>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Best Practices

1. **Choose appropriate min/max values**: Match the scale to your data range
2. **Use readable label formats**: Add units for clarity (°C, km/h, %)
3. **Balance tick density**: Too many ticks clutter; too few lack precision
4. **Position labels strategically**: Use Outside for prominence, Inside for compactness
5. **Handle overlapping labels**: Use HiddenLabel, HideIntersectingLabel, or ShowLastLabel
6. **Match angles to use case**: Semi-circle for speedometers, full circle for clocks
7. **Use percentage radius for responsiveness**: Makes gauges adapt to container size
8. **Multiple axes need visual separation**: Use different radii and positions
9. **Consider axis direction**: Clockwise feels natural for most Western audiences
10. **Test readability**: Ensure labels and ticks are visible at various sizes

## Troubleshooting

**Issue: Labels are cut off**
- **Solution**: Increase gauge dimensions or reduce font size, or use Position.Inside

**Issue: First and last labels overlap**
- **Solution**: Use `HiddenLabel="HiddenLabel.First"` or `HiddenLabel.Last"`

**Issue: Last label doesn't appear**
- **Solution**: Set `ShowLastLabel="true"` on the axis

**Issue: Ticks are too dense**
- **Solution**: Increase `Interval` values for major/minor ticks

**Issue: Axis doesn't fill the gauge**
- **Solution**: Increase `Radius` percentage (try "90%" or "95%")

**Issue: Multiple axes overlap**
- **Solution**: Use different Radius values (e.g., 80% for outer, 50% for inner)

**Issue: Custom format not applying**
- **Solution**: Ensure Format string uses `{value}` placeholder correctly
