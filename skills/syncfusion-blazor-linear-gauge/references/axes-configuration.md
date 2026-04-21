# Axes Configuration in Blazor Linear Gauge

## Table of Contents
- [Overview](#overview)
- [Setting Axis Range](#setting-axis-range)
- [Line Customization](#line-customization)
- [Ticks Customization](#ticks-customization)
  - [Major Ticks](#major-ticks)
  - [Minor Ticks](#minor-ticks)
  - [Positioning Ticks](#positioning-ticks)
- [Labels Customization](#labels-customization)
  - [Label Font Styling](#label-font-styling)
  - [Positioning Labels](#positioning-labels)
  - [Label Format](#label-format)
  - [Numeric Formats](#numeric-formats)
  - [Show Last Label](#show-last-label)
- [Axis Orientation](#axis-orientation)
- [Inverted Axis](#inverted-axis)
- [Opposed Position](#opposed-position)
- [Multiple Axes](#multiple-axes)
- [Complete Examples](#complete-examples)

## Overview

The axis is the foundation of the Linear Gauge, defining the numeric scale and visual appearance. Each Linear Gauge can have multiple axes, and each axis contains several customizable sub-elements:

- **Line:** The main axis line
- **Ticks:** Major and minor interval markers
- **Labels:** Numeric values displayed along the axis
- **Ranges:** Color-coded value regions (covered in ranges.md)
- **Pointers:** Value indicators (covered in pointers.md)

This guide covers all axis configuration options in a self-contained manner.

## Setting Axis Range

The start and end values of the axis scale are defined using `Minimum` and `Maximum` properties.

### Basic Range Configuration

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="50"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Default values:**
- `Minimum`: 0
- `Maximum`: 100

### Custom Range Example

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="20" Maximum="200">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="120"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Axis displays values from 20 to 200, and the pointer shows at value 120.

### Negative Range

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="-50" Maximum="50">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="-20"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Use case:** Temperature gauges, profit/loss indicators, elevation charts.

## Line Customization

The axis line can be customized using the `LinearGaugeLine` component with the following properties:

- **Height:** Length of the axis line (in pixels or percentage)
- **Width:** Thickness of the axis line (in pixels)
- **Color:** Color of the axis line
- **Offset:** Distance from the gauge container (in pixels)

### Basic Line Customization

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugeLine Height="150" Width="2" Color="#4286f4" Offset="2">
            </LinearGaugeLine>
            <LinearGaugePointers>
                <LinearGaugePointer></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Blue axis line, 2px thick, 150px long, with 2px offset.

### Thick Axis Line

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugeLine Height="200" Width="5" Color="#212529">
            </LinearGaugeLine>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Hiding the Axis Line

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugeLine Width="0"></LinearGaugeLine>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Use case:** Create clean, minimalist gauges without visible axis lines.

## Ticks Customization

Ticks mark intervals on the axis. There are two types: **major ticks** (primary intervals) and **minor ticks** (subdivisions).

### Major Ticks

Major ticks are the primary interval markers on the axis.

**Available properties:**
- **Interval** : Gap between major ticks (in axis units)
- **Height** : Length of tick mark (in pixels)
- **Width** : Thickness of tick mark (in pixels)
- **Color** : Color of tick marks
- **Position** : Inside, Outside, Cross, or Auto
- **Offset** : Distance from axis (in pixels)
- **DashArray** : Allows creating dashed lines

#### Basic Major Ticks

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeMajorTicks Interval="10" Height="15" Width="2" Color="#007bff">
            </LinearGaugeMajorTicks>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Major ticks every 10 units, 15px tall, blue color.

#### Styled Major Ticks

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="20" Maximum="140">
            <LinearGaugeMajorTicks Color="red" Interval="20" Height="20" Width="3">
            </LinearGaugeMajorTicks>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Minor Ticks

Minor ticks subdivide the intervals between major ticks for finer measurement.

**Available properties:** Same as major ticks (Interval, Height, Width, Color, Position, Offset, DashArray)

#### Basic Minor Ticks

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeMajorTicks Interval="20" Height="15">
            </LinearGaugeMajorTicks>
            <LinearGaugeMinorTicks Interval="5" Height="8" Color="#6c757d">
            </LinearGaugeMinorTicks>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Major ticks every 20 units, minor ticks every 5 units.

### Complete Ticks Example

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="20" Maximum="140">
            <LinearGaugeMajorTicks Color="red" Interval="20" Height="20" Width="3">
            </LinearGaugeMajorTicks>
            <LinearGaugeMinorTicks Color="orange" Interval="5" Height="10" Width="1">
            </LinearGaugeMinorTicks>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="80"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Positioning Ticks

Ticks can be positioned using the `Position` and `Offset` properties.

**Position values:**
- `Position.Inside` - Ticks point inward from the axis (default)
- `Position.Outside` - Ticks point outward from the axis
- `Position.Cross` - Ticks cross the axis line
- `Position.Auto` - Automatic positioning

#### Outside Position

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="160">
            <LinearGaugeMajorTicks Interval="10" Color="red" Height="10" Width="3"
                                   Position="Syncfusion.Blazor.LinearGauge.Position.Outside">
            </LinearGaugeMajorTicks>
            <LinearGaugeMinorTicks Interval="5" Color="green" Height="5" Width="2"
                                   Position="Syncfusion.Blazor.LinearGauge.Position.Cross">
            </LinearGaugeMinorTicks>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Major ticks outside the axis, minor ticks crossing the axis line.

#### With Offset

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugeMajorTicks Interval="10" Offset="5" Position="Syncfusion.Blazor.LinearGauge.Position.Outside">
            </LinearGaugeMajorTicks>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Major ticks positioned 5px away from the axis line.

### Hiding Ticks

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugeMajorTicks Height="0"></LinearGaugeMajorTicks>
            <LinearGaugeMinorTicks Height="0"></LinearGaugeMinorTicks>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Use case:** Minimalist gauges without tick marks.

## Labels Customization

Axis labels display the numeric values along the axis scale.

### Label Font Styling

Customize label appearance using `LinearGaugeAxisLabelFont` within `LinearGaugeAxisLabelStyle`.

**Available properties:**
- **Color:** Text color
- **FontFamily:** Font family (e.g., Arial, Verdana)
- **FontStyle:** Normal, Italic, Oblique
- **FontWeight:** Normal, Bold, Bolder, Lighter, or numeric (100-900)
- **Size:** Font size (e.g., "14px")
- **Opacity:** Text opacity (0 to 1)

#### Basic Label Styling

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugeAxisLabelStyle>
                <LinearGaugeAxisLabelFont Color="red" Size="14px" FontWeight="bold">
                </LinearGaugeAxisLabelFont>
            </LinearGaugeAxisLabelStyle>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Red, bold labels at 14px size.

#### Advanced Label Styling

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugeAxisLabelStyle>
                <LinearGaugeAxisLabelFont 
                    Color="#2c3e50" 
                    Size="12px" 
                    FontFamily="Segoe UI"
                    FontWeight="600"
                    FontStyle="normal"
                    Opacity="0.9">
                </LinearGaugeAxisLabelFont>
            </LinearGaugeAxisLabelStyle>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Positioning Labels

Labels can be positioned using `Position` and `Offset` properties in `LinearGaugeAxisLabelStyle`.

**Position values:**
- `Position.Inside` - Labels inside the axis (default)
- `Position.Outside` - Labels outside the axis
- `Position.Cross` - Labels crossing the axis
- `Position.Auto` - Automatic positioning

#### Outside Position

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugeAxisLabelStyle Position="Syncfusion.Blazor.LinearGauge.Position.Outside">
            </LinearGaugeAxisLabelStyle>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

#### Cross Position with Offset

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugeAxisLabelStyle Offset="55" Position="Syncfusion.Blazor.LinearGauge.Position.Cross">
            </LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Labels positioned 55px from the axis, crossing the axis line.

### Label Format

Add custom formatting to labels using the `Format` property. Use `{value}` as a placeholder for the numeric value.

#### Temperature Format

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="20" Maximum="140">
            <LinearGaugeAxisLabelStyle Format="{value}°C">
            </LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="80"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Labels display as "20°C", "40°C", "60°C", etc.

#### Custom Unit Format

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeAxisLabelStyle Format="{value} km/h">
            </LinearGaugeAxisLabelStyle>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Labels display as "0 km/h", "20 km/h", "40 km/h", etc.

#### Percentage Format

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeAxisLabelStyle Format="{value}%">
            </LinearGaugeAxisLabelStyle>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Numeric Formats

Use the `Format` property on `SfLinearGauge` for standard numeric formatting (currency, percentage, decimal places).

**Available formats:**
- **n1, n2, n3:** Number with 1, 2, or 3 decimal places
- **p1, p2, p3:** Percentage with 1, 2, or 3 decimal places
- **c1, c2, c3:** Currency with 1, 2, or 3 decimal places

| Value | Format | Result | Description |
|-------|--------|--------|-------------|
| 1000 | n1 | 1000.0 | 1 decimal place |
| 1000 | n2 | 1000.00 | 2 decimal places |
| 0.01 | p1 | 1.0% | Percentage, 1 decimal |
| 0.01 | p2 | 1.00% | Percentage, 2 decimals |
| 1000 | c1 | $1,000.0 | Currency, 1 decimal |
| 1000 | c2 | $1,000.00 | Currency, 2 decimals |

#### Currency Format Example

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge EnableGroupingSeparator="true" Format="c">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="10000">
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Labels display as "$0", "$2,000", "$4,000", etc.

#### Percentage Format Example

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Format="p1">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="1">
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Labels display as "0.0%", "20.0%", "40.0%", etc.

### Show Last Label

By default, if the last label falls outside the visible range, it's hidden. Enable it using `ShowLastLabel`.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="170" ShowLastLabel="true">
            <LinearGaugePointers>
                <LinearGaugePointer></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** The label "170" is displayed even if it's at the edge of the visible area.

## Axis Orientation

Change the gauge from vertical (default) to horizontal using the `Orientation` property on `SfLinearGauge`.

### Horizontal Orientation

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Orientation="Syncfusion.Blazor.LinearGauge.Orientation.Horizontal" Width="500px" Height="150px">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="20" Maximum="140">
            <LinearGaugeMajorTicks Interval="10"></LinearGaugeMajorTicks>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** A horizontal linear gauge spanning 500px wide.

### Vertical Orientation (Default)

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Orientation="Syncfusion.Blazor.LinearGauge.Orientation.Vertical" Width="200px" Height="400px">
    <LinearGaugeAxes>
        <LinearGaugeAxis>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Use cases:**
- **Horizontal:** Dashboard panels, progress bars, sliders
- **Vertical:** Thermometers, tank level indicators, vertical meters

## Inverted Axis

Reverse the axis direction so values increase in the opposite direction.

### Basic Inverted Axis

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis IsInversed="true">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="40"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Axis values increase from top to bottom (or right to left for horizontal).

### Inverted with Custom Range

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100" IsInversed="true">
            <LinearGaugeAxisLabelStyle Format="{value}m">
            </LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="70"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Use case:** Depth gauges, countdown timers, descending scales.

## Opposed Position

Place the axis on the opposite side of the gauge using `OpposedPosition`.

### Basic Opposed Position

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis OpposedPosition="true">
            <LinearGaugePointers>
                <LinearGaugePointer></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Axis appears on the right side (vertical) or bottom (horizontal).

**Use case:** Comparing two metrics side-by-side with multiple axes.

## Multiple Axes

Display multiple scales on a single gauge by adding multiple `LinearGaugeAxis` components.

### Dual Axis Example

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <!-- First axis: Celsius -->
        <LinearGaugeAxis Minimum="20" Maximum="100">
            <LinearGaugeAxisLabelStyle Format="{value}°C">
            </LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="60" Color="#007bff">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
        
        <!-- Second axis: Fahrenheit (opposed) -->
        <LinearGaugeAxis Minimum="68" Maximum="212" OpposedPosition="true">
            <LinearGaugeMajorTicks Interval="20" Height="20">
            </LinearGaugeMajorTicks>
            <LinearGaugeMinorTicks Interval="5" Height="10">
            </LinearGaugeMinorTicks>
            <LinearGaugeAxisLabelStyle Format="{value}°F">
            </LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="140" Color="#dc3545">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Two axes showing Celsius on the left and Fahrenheit on the right.

### Triple Axis Example

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Width="500px" Height="200px" Orientation="Syncfusion.Blazor.LinearGauge.Orientation.Horizontal">
    <LinearGaugeAxes>
        <!-- Axis 1: Kilometers -->
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeAxisLabelStyle Format="{value} km">
            </LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="60" Color="#28a745">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
        
        <!-- Axis 2: Miles (center) -->
        <LinearGaugeAxis Minimum="0" Maximum="62">
            <LinearGaugeAxisLabelStyle Format="{value} mi" Position="Syncfusion.Blazor.LinearGauge.Position.Cross">
            </LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="37" Color="#ffc107">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
        
        <!-- Axis 3: Meters (opposed) -->
        <LinearGaugeAxis Minimum="0" Maximum="100000" OpposedPosition="true">
            <LinearGaugeAxisLabelStyle Format="{value} m">
            </LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="60000" Color="#17a2b8">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Use case:** Unit conversion displays, multi-metric dashboards.

## Complete Examples

### Styled Temperature Gauge

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Title="Temperature Monitor" Width="200px" Height="450px">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="-20" Maximum="120">
            <!-- Styled axis line -->
            <LinearGaugeLine Width="3" Color="#212529">
            </LinearGaugeLine>
            
            <!-- Custom ticks -->
            <LinearGaugeMajorTicks Interval="20" Height="15" Width="2" 
                                   Color="#495057" Position="Syncfusion.Blazor.LinearGauge.Position.Outside">
            </LinearGaugeMajorTicks>
            <LinearGaugeMinorTicks Interval="5" Height="8" Width="1" 
                                   Color="#6c757d" Position="Syncfusion.Blazor.LinearGauge.Position.Outside">
            </LinearGaugeMinorTicks>
            
            <!-- Formatted labels -->
            <LinearGaugeAxisLabelStyle Format="{value}°C" Position="Syncfusion.Blazor.LinearGauge.Position.Outside" Offset="10">
                <LinearGaugeAxisLabelFont Color="#212529" Size="12px" FontWeight="500">
                </LinearGaugeAxisLabelFont>
            </LinearGaugeAxisLabelStyle>
            
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="35" Color="#dc3545">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Horizontal Progress Indicator

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Orientation="Syncfusion.Blazor.LinearGauge.Orientation.Horizontal" Width="600px" Height="100px">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeLine Height="500" Width="20" Color="#e9ecef">
            </LinearGaugeLine>
            
            <LinearGaugeMajorTicks Interval="25" Height="0">
            </LinearGaugeMajorTicks>
            
            <LinearGaugeAxisLabelStyle Format="{value}%" Position="Syncfusion.Blazor.LinearGauge.Position.Outside" Offset="20">
                <LinearGaugeAxisLabelFont Size="14px" FontWeight="bold">
                </LinearGaugeAxisLabelFont>
            </LinearGaugeAxisLabelStyle>
            
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="75" Type="Syncfusion.Blazor.LinearGauge.Point.Bar" 
                                    Color="#28a745" Width="20" Offset="20">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Multi-Axis Dashboard Gauge

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Width="300px" Height="400px">
    <LinearGaugeAxes>
        <!-- Primary axis -->
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeAxisLabelStyle Format="{value}%">
                <LinearGaugeAxisLabelFont Color="#007bff">
                </LinearGaugeAxisLabelFont>
            </LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="65" Color="#007bff">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
        
        <!-- Secondary axis (opposed) -->
        <LinearGaugeAxis Minimum="0" Maximum="200" OpposedPosition="true">
            <LinearGaugeAxisLabelStyle Format="{value} MB">
                <LinearGaugeAxisLabelFont Color="#28a745">
                </LinearGaugeAxisLabelFont>
            </LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="130" Color="#28a745" MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Circle">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

## Key Takeaways

1. **Range:** Set `Minimum` and `Maximum` to define the scale
2. **Line:** Customize with `Height`, `Width`, `Color`, and `Offset`
3. **Ticks:** Configure interval, appearance, and position separately for major and minor ticks
4. **Labels:** Format with `{value}` placeholder or numeric formats (n, p, c)
5. **Orientation:** Choose `Vertical` or `Horizontal` based on layout needs
6. **Inverted:** Use `IsInversed="true"` to reverse value direction
7. **Opposed:** Use `OpposedPosition="true"` to move axis to opposite side
8. **Multiple Axes:** Add multiple `LinearGaugeAxis` components for multi-scale displays

All axis configurations are independent and can be combined to create highly customized linear gauges.
