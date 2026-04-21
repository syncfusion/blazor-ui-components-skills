# Ranges in Blazor Linear Gauge

## Table of Contents
- [Overview](#overview)
- [Basic Range Configuration](#basic-range-configuration)
- [Range Customization](#range-customization)
  - [Start and End Width](#start-and-end-width)
  - [Color](#color)
  - [Position](#position)
  - [Offset](#offset)
  - [Border](#border)
- [Multiple Ranges](#multiple-ranges)
- [Range Color for Labels](#range-color-for-labels)
- [Gradient Colors in Ranges](#gradient-colors-in-ranges)
  - [Linear Gradient](#linear-gradient)
  - [Radial Gradient](#radial-gradient)
- [Practical Examples](#practical-examples)

## Overview

Ranges in the Linear Gauge are colored segments that visually categorize different value regions on the axis. They're essential for creating intuitive visualizations where users can quickly identify zones like:

- **Safe/Warning/Danger zones** (e.g., green/yellow/red)
- **Performance levels** (e.g., poor/average/excellent)
- **Temperature zones** (e.g., freezing/cold/normal/hot)
- **Pressure levels** (e.g., low/optimal/high)

Ranges are defined using start and end values and can be extensively customized with colors, widths, positioning, and gradients.

## Basic Range Configuration

Create a range by specifying `Start` and `End` values within a `LinearGaugeRange` component.

### Single Range

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeRanges>
                <LinearGaugeRange Start="50" End="80" StartWidth="20" EndWidth="20">
                </LinearGaugeRange>
            </LinearGaugeRanges>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="65"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** A colored range from value 50 to 80 with uniform width of 20 pixels.

### Colored Range

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeRanges>
                <LinearGaugeRange Start="30" End="70" Color="#FFA500" 
                                  StartWidth="15" EndWidth="15">
                </LinearGaugeRange>
            </LinearGaugeRanges>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Orange range from 30 to 70.

## Range Customization

### Start and End Width

Control the thickness of the range at its start and end points to create uniform or tapered effects.

#### Uniform Width Range

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeRanges>
                <LinearGaugeRange Start="20" End="80" 
                                  StartWidth="10" 
                                  EndWidth="10"
                                  Color="#28a745">
                </LinearGaugeRange>
            </LinearGaugeRanges>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Consistent 10px width throughout the range.

#### Tapered Range (Gradient Width)

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeRanges>
                <LinearGaugeRange Start="0" End="100" 
                                  StartWidth="5" 
                                  EndWidth="30"
                                  Color="#007bff">
                </LinearGaugeRange>
            </LinearGaugeRanges>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Range starts at 5px wide and gradually expands to 30px at the end, creating a tapered visual effect.

**Use case:** Creating visual flow, emphasizing the end of a scale.

### Color

Set the range color using the `Color` property with hex codes, RGB, or color names.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeRanges>
                <LinearGaugeRange Start="0" End="33" Color="#4CAF50" 
                                  StartWidth="15" EndWidth="15">
                </LinearGaugeRange>
                <LinearGaugeRange Start="33" End="66" Color="#FFC107" 
                                  StartWidth="15" EndWidth="15">
                </LinearGaugeRange>
                <LinearGaugeRange Start="66" End="100" Color="#F44336" 
                                  StartWidth="15" EndWidth="15">
                </LinearGaugeRange>
            </LinearGaugeRanges>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Three ranges with green, yellow, and red colors representing low, medium, and high zones.

### Position

Control where the range appears relative to the axis line using the `Position` property.

**Available positions:**
- `Position.Inside` - Range inside the axis (default)
- `Position.Outside` - Range outside the axis
- `Position.Cross` - Range crosses the axis line
- `Position.Auto` - Automatic positioning

#### Outside Position

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeRanges>
                <LinearGaugeRange Start="40" End="80" 
                                  Color="#17a2b8"
                                  Position="Syncfusion.Blazor.LinearGauge.Position.Outside"
                                  StartWidth="20" 
                                  EndWidth="20">
                </LinearGaugeRange>
            </LinearGaugeRanges>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Range appears outside the axis line.

#### Cross Position

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugeRanges>
                <LinearGaugeRange Start="30" End="70" 
                                  Color="#6f42c1"
                                  Position="Syncfusion.Blazor.LinearGauge.Position.Cross"
                                  StartWidth="25" 
                                  EndWidth="25">
                </LinearGaugeRange>
            </LinearGaugeRanges>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Range straddles the axis line, half inside and half outside.

### Offset

Adjust the range's distance from the axis using the `Offset` property (in pixels).

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeRanges>
                <LinearGaugeRange Start="50" End="80" 
                                  Color="orange"
                                  Position="Syncfusion.Blazor.LinearGauge.Position.Inside"
                                  Offset="10"
                                  StartWidth="15"
                                  EndWidth="15">
                </LinearGaugeRange>
            </LinearGaugeRanges>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Range positioned 10 pixels away from the axis.

**Use case:** Creating layered ranges, avoiding overlap with pointers or labels.

### Border

Add a border around the range using `LinearGaugeRangeBorder`.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeRanges>
                <LinearGaugeRange Start="50" End="80" 
                                  StartWidth="15" 
                                  EndWidth="15"
                                  Color="orange" 
                                  Position="Syncfusion.Blazor.LinearGauge.Position.Inside"
                                  Offset="4">
                    <LinearGaugeRangeBorder Color="red" Width="2">
                    </LinearGaugeRangeBorder>
                </LinearGaugeRange>
            </LinearGaugeRanges>
            <LinearGaugePointers>
                <LinearGaugePointer></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Orange range with a 2px red border.

**Use case:** Highlighting critical ranges, adding definition to ranges.

## Multiple Ranges

Display multiple ranges on a single axis to create color-coded zones.

### Three-Zone Range System

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeRanges>
                <!-- Safe zone (green) -->
                <LinearGaugeRange Start="0" End="50" 
                                  Color="#4CAF50"
                                  StartWidth="10" 
                                  EndWidth="10">
                </LinearGaugeRange>
                
                <!-- Warning zone (yellow) -->
                <LinearGaugeRange Start="50" End="75" 
                                  Color="#FFC107"
                                  StartWidth="10" 
                                  EndWidth="10">
                </LinearGaugeRange>
                
                <!-- Danger zone (red) -->
                <LinearGaugeRange Start="75" End="100" 
                                  Color="#F44336"
                                  StartWidth="10" 
                                  EndWidth="10">
                </LinearGaugeRange>
            </LinearGaugeRanges>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="65"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Three consecutive ranges creating a traffic-light style indicator.

### Multiple Styled Ranges

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeRanges>
                <LinearGaugeRange Start="0" End="30" 
                                  StartWidth="10" 
                                  EndWidth="10"
                                  Color="#41f47f">
                </LinearGaugeRange>
                <LinearGaugeRange Start="30" End="50" 
                                  StartWidth="10" 
                                  EndWidth="10"
                                  Color="#f49441">
                </LinearGaugeRange>
                <LinearGaugeRange Start="50" End="80" 
                                  StartWidth="10" 
                                  EndWidth="10"
                                  Color="#cd41f4">
                </LinearGaugeRange>
            </LinearGaugeRanges>
            <LinearGaugePointers>
                <LinearGaugePointer></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Temperature Zones

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Title="Temperature Monitor" Width="200px" Height="400px">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="-20" Maximum="120">
            <LinearGaugeAxisLabelStyle Format="{value}°C">
            </LinearGaugeAxisLabelStyle>
            
            <LinearGaugeRanges>
                <!-- Freezing -->
                <LinearGaugeRange Start="-20" End="0" 
                                  Color="#0099ff"
                                  StartWidth="15" 
                                  EndWidth="15">
                </LinearGaugeRange>
                
                <!-- Cold -->
                <LinearGaugeRange Start="0" End="15" 
                                  Color="#4da6ff"
                                  StartWidth="15" 
                                  EndWidth="15">
                </LinearGaugeRange>
                
                <!-- Comfortable -->
                <LinearGaugeRange Start="15" End="25" 
                                  Color="#00e676"
                                  StartWidth="15" 
                                  EndWidth="15">
                </LinearGaugeRange>
                
                <!-- Warm -->
                <LinearGaugeRange Start="25" End="35" 
                                  Color="#ffc107"
                                  StartWidth="15" 
                                  EndWidth="15">
                </LinearGaugeRange>
                
                <!-- Hot -->
                <LinearGaugeRange Start="35" End="120" 
                                  Color="#ff5252"
                                  StartWidth="15" 
                                  EndWidth="15">
                </LinearGaugeRange>
            </LinearGaugeRanges>
            
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="22" Color="#212529">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Five color-coded temperature zones from freezing to hot.

## Range Color for Labels

Apply range colors to axis labels using the `UseRangeColor` property.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeAxisLabelStyle UseRangeColor="true">
            </LinearGaugeAxisLabelStyle>
            
            <LinearGaugeRanges>
                <LinearGaugeRange Start="0" End="33" Color="#4CAF50"
                                  StartWidth="10" EndWidth="10">
                </LinearGaugeRange>
                <LinearGaugeRange Start="33" End="66" Color="#FFC107"
                                  StartWidth="10" EndWidth="10">
                </LinearGaugeRange>
                <LinearGaugeRange Start="66" End="100" Color="#F44336"
                                  StartWidth="10" EndWidth="10">
                </LinearGaugeRange>
            </LinearGaugeRanges>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Labels within each range inherit the range's color (green for 0-33, yellow for 33-66, red for 66-100).

**Use case:** Enhanced color coordination, improved visual association between labels and ranges.

## Gradient Colors in Ranges

Apply gradient fills to ranges for sophisticated visual effects.

### Linear Gradient

Create smooth color transitions along the range using linear gradients.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Orientation="Syncfusion.Blazor.LinearGauge.Orientation.Horizontal">
    <LinearGaugeContainer Width="30" Offset="30">
        <LinearGaugeContainerBorder Width="0" />
        <LinearGaugeAxes>
            <LinearGaugeAxis>
                <LinearGaugeAxisLabelStyle Offset="55">
                    <LinearGaugeAxisLabelFont Color="#424242" />
                </LinearGaugeAxisLabelStyle>
                <LinearGaugeLine Width="0" />
                <LinearGaugeMajorTicks Height="0" Interval="25" />
                <LinearGaugeMinorTicks Height="0" />
                
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="80" 
                                        Height="25" 
                                        Width="35"
                                        Color="#f54ea2" 
                                        Offset="-40" 
                                        MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Triangle"
                                        Placement="Syncfusion.Blazor.LinearGauge.Placement.Near">
                    </LinearGaugePointer>
                </LinearGaugePointers>
                
                <LinearGaugeRanges>
                    <LinearGaugeRange Color="#f54ea2" 
                                      Start="0" 
                                      End="80"
                                      StartWidth="30" 
                                      EndWidth="30" 
                                      Offset="30">
                        <Syncfusion.Blazor.LinearGauge.LinearGradient StartValue="1%" EndValue="99%">
                            <Syncfusion.Blazor.LinearGauge.ColorStops>
                                <Syncfusion.Blazor.LinearGauge.ColorStop Opacity="1" Offset="0%" Color="#fef3f9">
                                </Syncfusion.Blazor.LinearGauge.ColorStop>
                                <Syncfusion.Blazor.LinearGauge.ColorStop Opacity="1" Offset="100%" Color="#f54ea2">
                                </Syncfusion.Blazor.LinearGauge.ColorStop>
                            </Syncfusion.Blazor.LinearGauge.ColorStops>
                        </Syncfusion.Blazor.LinearGauge.LinearGradient>
                    </LinearGaugeRange>
                </LinearGaugeRanges>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </LinearGaugeContainer>
</SfLinearGauge>
```

**Properties:**
- `StartValue`: Where the gradient starts (percentage string)
- `EndValue`: Where the gradient ends (percentage string)
- `ColorStops`: Collection of color stop
    - `ColorStop`: Color range properties for the gradient.
        - `Offset`: Position of color stop (percentage)
        - `Color`: Color at this position
        - `Opacity`: Transparency (0-1)
        - `Style`: Custom styles for the color stop

### Radial Gradient

Create circular color progressions within the range.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Orientation="Syncfusion.Blazor.LinearGauge.Orientation.Horizontal">
    <LinearGaugeContainer Width="30" Offset="30">
        <LinearGaugeContainerBorder Width="0" />
        <LinearGaugeAxes>
            <LinearGaugeAxis>
                <LinearGaugeAxisLabelStyle Offset="55">
                    <LinearGaugeAxisLabelFont Color="#424242" />
                </LinearGaugeAxisLabelStyle>
                <LinearGaugeLine Width="0" />
                <LinearGaugeMajorTicks Height="0" Interval="25" />
                <LinearGaugeMinorTicks Height="0" />
                
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="80" 
                                        Height="25" 
                                        Width="35"
                                        Color="#f54ea2" 
                                        Offset="-40" 
                                        MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Triangle"
                                        Placement="Syncfusion.Blazor.LinearGauge.Placement.Near">
                    </LinearGaugePointer>
                </LinearGaugePointers>
                
                <LinearGaugeRanges>
                    <LinearGaugeRange Color="#f54ea2" 
                                      Start="0" 
                                      End="80"
                                      StartWidth="30" 
                                      EndWidth="30" 
                                      Offset="30">
                        <Syncfusion.Blazor.LinearGauge.RadialGradient Radius="65%">
                            <Syncfusion.Blazor.LinearGauge.InnerPosition X="50%" Y="70%"></Syncfusion.Blazor.LinearGauge.InnerPosition>
                            <Syncfusion.Blazor.LinearGauge.OuterPosition X="60%" Y="60%"></Syncfusion.Blazor.LinearGauge.OuterPosition>
                            <Syncfusion.Blazor.LinearGauge.ColorStops>
                                <Syncfusion.Blazor.LinearGauge.ColorStop Opacity="0.9" Color="#fff5f5" Offset="5%">
                                </Syncfusion.Blazor.LinearGauge.ColorStop>
                                <Syncfusion.Blazor.LinearGauge.ColorStop Opacity="1" Color="#f54ea2" Offset="99%">
                                </Syncfusion.Blazor.LinearGauge.ColorStop>
                            </Syncfusion.Blazor.LinearGauge.ColorStops>
                        </Syncfusion.Blazor.LinearGauge.RadialGradient>
                    </LinearGaugeRange>
                </LinearGaugeRanges>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </LinearGaugeContainer>
</SfLinearGauge>
```

**Properties:**
- `Radius`: Circle size (percentage string)
- `InnerPosition`: Center point (X, Y percentages)
- `OuterPosition`: Outer edge position (X, Y percentages)
- `ColorStops`: Same as linear gradient

**Note:** If both gradients are defined, linear gradient takes precedence. To render radial gradient when both are present, set linear gradient's `StartValue` and `EndValue` to empty strings.

## Practical Examples

### Performance Indicator

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Title="System Performance" Orientation="Syncfusion.Blazor.LinearGauge.Orientation.Horizontal" 
               Width="600px" Height="150px">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeAxisLabelStyle Format="{value}%">
            </LinearGaugeAxisLabelStyle>
            
            <LinearGaugeRanges>
                <!-- Critical (0-20%) -->
                <LinearGaugeRange Start="0" End="20" 
                                  Color="#dc3545"
                                  StartWidth="20" 
                                  EndWidth="20">
                    <LinearGaugeRangeBorder Color="#a71d2a" Width="1">
                    </LinearGaugeRangeBorder>
                </LinearGaugeRange>
                
                <!-- Poor (20-40%) -->
                <LinearGaugeRange Start="20" End="40" 
                                  Color="#fd7e14"
                                  StartWidth="20" 
                                  EndWidth="20">
                </LinearGaugeRange>
                
                <!-- Average (40-60%) -->
                <LinearGaugeRange Start="40" End="60" 
                                  Color="#ffc107"
                                  StartWidth="20" 
                                  EndWidth="20">
                </LinearGaugeRange>
                
                <!-- Good (60-80%) -->
                <LinearGaugeRange Start="60" End="80" 
                                  Color="#20c997"
                                  StartWidth="20" 
                                  EndWidth="20">
                </LinearGaugeRange>
                
                <!-- Excellent (80-100%) -->
                <LinearGaugeRange Start="80" End="100" 
                                  Color="#28a745"
                                  StartWidth="20" 
                                  EndWidth="20">
                    <LinearGaugeRangeBorder Color="#1e7e34" Width="1">
                    </LinearGaugeRangeBorder>
                </LinearGaugeRange>
            </LinearGaugeRanges>
            
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="72" 
                                    Type="Syncfusion.Blazor.LinearGauge.Point.Bar"
                                    Width="20"
                                    Color="#212529"
                                    Opacity="0.3">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Fuel Level Indicator

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Title="Fuel Level" Width="150px" Height="400px">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeAxisLabelStyle Format="{value}L">
            </LinearGaugeAxisLabelStyle>
            
            <LinearGaugeRanges>
                <!-- Empty (red) -->
                <LinearGaugeRange Start="0" End="15" 
                                  Color="#ff0000"
                                  StartWidth="30" 
                                  EndWidth="30"
                                  Position="Syncfusion.Blazor.LinearGauge.Position.Inside">
                </LinearGaugeRange>
                
                <!-- Low (orange) -->
                <LinearGaugeRange Start="15" End="40" 
                                  Color="#ff8800"
                                  StartWidth="30" 
                                  EndWidth="30"
                                  Position="Syncfusion.Blazor.LinearGauge.Position.Inside">
                </LinearGaugeRange>
                
                <!-- Normal (green) -->
                <LinearGaugeRange Start="40" End="100" 
                                  Color="#00cc00"
                                  StartWidth="30" 
                                  EndWidth="30"
                                  Position="Syncfusion.Blazor.LinearGauge.Position.Inside">
                </LinearGaugeRange>
            </LinearGaugeRanges>
            
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="32" 
                                    Type="Syncfusion.Blazor.LinearGauge.Point.Bar"
                                    Width="30"
                                    Color="#2196F3"
                                    RoundedCornerRadius="5">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Pressure Gauge with Offsets

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="150">
            <LinearGaugeAxisLabelStyle Format="{value} PSI">
            </LinearGaugeAxisLabelStyle>
            
            <LinearGaugeRanges>
                <!-- Low pressure (inside) -->
                <LinearGaugeRange Start="0" End="40" 
                                  Color="#ffc107"
                                  StartWidth="12" 
                                  EndWidth="12"
                                  Position="Syncfusion.Blazor.LinearGauge.Position.Inside"
                                  Offset="0">
                </LinearGaugeRange>
                
                <!-- Normal pressure (inside, offset) -->
                <LinearGaugeRange Start="40" End="120" 
                                  Color="#28a745"
                                  StartWidth="12" 
                                  EndWidth="12"
                                  Position="Syncfusion.Blazor.LinearGauge.Position.Inside"
                                  Offset="15">
                </LinearGaugeRange>
                
                <!-- High pressure (outside) -->
                <LinearGaugeRange Start="120" End="150" 
                                  Color="#dc3545"
                                  StartWidth="12" 
                                  EndWidth="12"
                                  Position="Syncfusion.Blazor.LinearGauge.Position.Outside"
                                  Offset="0">
                </LinearGaugeRange>
            </LinearGaugeRanges>
            
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="85"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

## Key Takeaways

1. **Purpose:** Ranges visually segment the axis into color-coded zones
2. **Basic Properties:** Start, End, StartWidth, EndWidth, Color
3. **Positioning:** Use Position (Inside/Outside/Cross) and Offset for placement
4. **Multiple Ranges:** Stack ranges to create comprehensive zone systems
5. **Borders:** Add definition with LinearGaugeRangeBorder
6. **Label Colors:** Use UseRangeColor to match label colors to ranges
7. **Gradients:** Apply linear or radial gradients for enhanced visuals
8. **Tapered Ranges:** Different StartWidth and EndWidth create gradient width effects

Ranges are powerful tools for creating intuitive, color-coded visualizations that help users quickly understand value categories and thresholds.
