# Circular Gauge Pointers Reference

## Table of Contents
- [Overview](#overview)
- [Pointer Types](#pointer-types)
  - [Needle Pointer](#needle-pointer)
  - [Range Bar Pointer](#range-bar-pointer)
  - [Marker Pointer](#marker-pointer)
- [Pointer Customization](#pointer-customization)
- [Multiple Pointers](#multiple-pointers)
- [Pointer Animation](#pointer-animation)
- [Dragging Pointers](#dragging-pointers)
- [Gradient Colors](#gradient-colors)
- [Use Cases](#use-cases)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Pointers are essential visual elements in the Circular Gauge that indicate values on an axis. They provide dynamic visual feedback to users by marking specific points on the gauge. The Syncfusion Blazor Circular Gauge supports three distinct pointer types: **Needle**, **RangeBar**, and **Marker**, each serving different visualization purposes.

The pointer value is controlled using the `Value` property, which determines where the pointer appears on the axis scale.

## Pointer Types

### Needle Pointer

The needle pointer is the default pointer type in Circular Gauge. It resembles a traditional gauge needle and consists of three key parts:

1. **Needle** - The main pointing element
2. **Cap/Knob** - The center anchor point
3. **Tail** - The rear extension (optional)

#### Basic Needle Pointer

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="90">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

#### Needle Customization Properties

**Main Needle Properties:**
- `Radius` - Sets the length of the needle (percentage or pixels)
- `PointerWidth` - Controls the width of the needle
- `Color` - Specifies the needle color
- `NeedleStartWidth` - Width at the needle's starting point
- `NeedleEndWidth` - Width at the needle's ending point
- `Position` - Placement relative to axis (Inside, Outside, Cross)

**Cap Customization:**
- `Radius` - Size of the cap/knob
- `Color` - Cap color
- `Border` - Cap border styling (color and width)

**Tail Customization:**
- `Length` - Tail length (percentage of needle length)
- `Color` - Tail color
- `Border` - Tail border styling

#### Advanced Needle Example

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge Height="250px" Width="250px">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="90"
                                      Radius="40%"
                                      PointerWidth="30"
                                      Color="#007DD1"
                                      Position="PointerRangePosition.Inside">
                    <CircularGaugeCap Radius="15"
                                      Color="white">
                        <CircularGaugeCapBorder Width="4"
                                                Color="#007DD1">
                        </CircularGaugeCapBorder>
                    </CircularGaugeCap>
                    <CircularGaugeNeedleTail Length="35%"
                                             Color="#007DD1">
                    </CircularGaugeNeedleTail>
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

#### Custom Needle Width Example

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis StartAngle="270" EndAngle="90" Radius="90%" Minimum="0" Maximum="100">
            <CircularGaugeAxisLineStyle Width="3" Color="#1E7145">
            </CircularGaugeAxisLineStyle>
            <CircularGaugeAxisLabelStyle>
                <CircularGaugeAxisLabelFont Color="#1E7145" Size="0px">
                </CircularGaugeAxisLabelFont>
            </CircularGaugeAxisLabelStyle>
            <CircularGaugeAxisMajorTicks Interval="100" Height="0" Width="1">
            </CircularGaugeAxisMajorTicks>
            <CircularGaugeAxisMinorTicks Width="0" Height="0">
            </CircularGaugeAxisMinorTicks>
            <CircularGaugePointers>
                <CircularGaugePointer Value="70"
                                      Radius="80%" 
                                      Color="green" 
                                      PointerWidth="2" 
                                      NeedleStartWidth="4" 
                                      NeedleEndWidth="4">
                    <CircularGaugeCap Radius="8" Color="green">
                    </CircularGaugeCap>
                    <CircularGaugeNeedleTail Length="0%">
                    </CircularGaugeNeedleTail>
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Range Bar Pointer

The range bar pointer visualizes data as a continuous bar extending from the axis start point to the pointer value. It's ideal for showing progress or indicating filled capacity.

#### Basic Range Bar

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="50"
                                      Type="PointerType.RangeBar">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

#### Range Bar Customization Properties

- `PointerWidth` - Width of the range bar
- `Color` - Range bar color
- `Radius` - Distance from the center
- `RoundedCornerRadius` - Creates arc-shaped ends
- `Border` - Border styling (color and width)

#### Customized Range Bar

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="46"
                                      Type="PointerType.RangeBar"
                                      PointerWidth="8"
                                      Radius="95%"
                                      Color="lightgray">
                    <CircularGaugePointerBorder Color="darkgray"
                                                Width="2">
                    </CircularGaugePointerBorder>
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

#### Rounded Corners

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="46"
                                      RoundedCornerRadius="20"
                                      Type="PointerType.RangeBar">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Marker Pointer

Marker pointers use geometric shapes or images to mark specific values on the axis. They're useful for discrete value indication or when you need multiple visible values simultaneously.

#### Available Marker Shapes

- Circle
- Rectangle
- Triangle
- InvertedTriangle
- Diamond
- Image (custom images)

#### Basic Marker Pointer

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="90"
                                      Type="PointerType.Marker"
                                      MarkerShape="GaugeShape.Diamond"
                                      MarkerHeight="15"
                                      MarkerWidth="15">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

#### Marker Customization Properties

- `Color` - Marker color
- `MarkerWidth` - Width of the marker
- `MarkerHeight` - Height of the marker
- `Radius` - Distance from axis center
- `MarkerShape` - Shape type
- `Border` - Border styling

#### Customized Marker Example

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="90"
                                      Type="PointerType.Marker"
                                      MarkerShape="GaugeShape.InvertedTriangle"
                                      MarkerHeight="15"
                                      MarkerWidth="15"
                                      Color="white"
                                      Radius="110%">
                    <CircularGaugePointerBorder Width="2" Color="#007DD1">
                    </CircularGaugePointerBorder>
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

#### Image Marker Pointer

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="90"
                                      Type="PointerType.Marker"
                                      MarkerShape="GaugeShape.Image"
                                      ImageUrl="football.png"
                                      MarkerHeight="20"
                                      MarkerWidth="20"
                                      Radius="100%">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Pointer Customization

### Position Property

Control where pointers are positioned relative to the axis:

- `PointerRangePosition.Inside` - Pointer inside the axis
- `PointerRangePosition.Outside` - Pointer outside the axis
- `PointerRangePosition.Cross` - Pointer crosses the axis

```cshtml
<CircularGaugePointer Value="90"
                      Position="PointerRangePosition.Cross">
</CircularGaugePointer>
```

## Multiple Pointers

Add multiple pointers to display different values or create complex visualizations.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <!-- Marker Pointer -->
                <CircularGaugePointer Value="90"
                                      Type="PointerType.Marker"
                                      MarkerShape="GaugeShape.InvertedTriangle"
                                      Radius="100%"
                                      MarkerHeight="15"
                                      MarkerWidth="15">
                </CircularGaugePointer>
                
                <!-- Range Bar Pointer -->
                <CircularGaugePointer Value="90"
                                      Type="PointerType.RangeBar"
                                      Radius="60%"
                                      PointerWidth="10">
                </CircularGaugePointer>
                
                <!-- Needle Pointer -->
                <CircularGaugePointer Value="90"
                                      Radius="50%"
                                      PointerWidth="25"
                                      Color="#007DD1">
                    <CircularGaugeCap Radius="15">
                        <CircularGaugeCapBorder Width="5">
                        </CircularGaugeCapBorder>
                    </CircularGaugeCap>
                    <CircularGaugeNeedleTail Length="25%">
                    </CircularGaugeNeedleTail>
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Pointer Animation

Animate pointers on gauge load for enhanced user experience.

### Animation Properties

- `Enable` - Turn animation on/off
- `Duration` - Animation duration in milliseconds

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge Height="250px" Width="250px">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="70">
                    <CircularGaugePointerAnimation Enable="true" Duration="1500">
                    </CircularGaugePointerAnimation>
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Dragging Pointers

Enable interactive pointer dragging to allow users to change values.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge EnablePointerDrag="true" Height="250px" Width="250px">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="50">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Handling Drag Events

```cshtml
@using Syncfusion.Blazor.CircularGauge

<div>Current Value: @pointerValue</div>
<SfCircularGauge EnablePointerDrag="true">
    <CircularGaugeEvents OnDragMove="@UpdatePointerValue">
    </CircularGaugeEvents>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="@pointerValue">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

@code {
    private double pointerValue = 50;
    
    void UpdatePointerValue(Syncfusion.Blazor.CircularGauge.PointerDragEventArgs args)
    {
        pointerValue = args.CurrentValue;
    }
}
```

## Gradient Colors

Apply linear or radial gradients to pointers for enhanced visual appeal.

### Linear Gradient

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="80" Radius="80%" PointerWidth="10">
                    <LinearGradient StartValue="1%" EndValue="99%">
                        <ColorStops>
                            <ColorStop Opacity="0.9" Offset="0%" Color="#fef3f9">
                            </ColorStop>
                            <ColorStop Opacity="0.9" Offset="100%" Color="#f54ea2">
                            </ColorStop>
                        </ColorStops>
                    </LinearGradient>
                    <CircularGaugeCap Radius="8" Color="White">
                        <CircularGaugeCapBorder Color="#E63B86" Width="1" />
                    </CircularGaugeCap>
                    <CircularGaugeNeedleTail Length="20%">
                        <LinearGradient StartValue="1%" EndValue="99%">
                            <ColorStops>
                                <ColorStop Opacity="0.9" Offset="0%" Color="#fef3f9">
                                </ColorStop>
                                <ColorStop Opacity="0.9" Offset="100%" Color="#f54ea2">
                                </ColorStop>
                            </ColorStops>
                        </LinearGradient>
                    </CircularGaugeNeedleTail>
                    <CircularGaugePointerAnimation Enable="true" Duration="1000" />
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Radial Gradient

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="80" Radius="80%" PointerWidth="10">
                    <RadialGradient Radius="65%">
                        <InnerPosition X="60%" Y="60%"></InnerPosition>
                        <OuterPosition X="50%" Y="70%"></OuterPosition>
                        <ColorStops>
                            <ColorStop Opacity="0.9" Offset="5%" Color="#fff5f5">
                            </ColorStop>
                            <ColorStop Opacity="0.9" Offset="99%" Color="#f54ea2">
                            </ColorStop>
                        </ColorStops>
                    </RadialGradient>
                    <CircularGaugeCap Radius="8" Color="White">
                        <CircularGaugeCapBorder Color="#E63B86" Width="1" />
                    </CircularGaugeCap>
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Use Cases

### Speedometer Dashboard
Use a needle pointer with cap and tail customization to create a realistic speedometer effect. Combine with ranges for speed zones.

### Progress Indicator
Use a range bar pointer with rounded corners to show completion status or progress percentage. Ideal for dashboards.

### Temperature Gauge
Use a marker pointer (triangle) with multiple markers at different radii to show current, high, and low temperatures.

### Multiple Data Points
Use multiple marker pointers with different shapes and colors to display several related metrics on a single gauge.

### Interactive Controls
Enable pointer dragging for user input scenarios like volume control, brightness adjustment, or parameter tuning.

## Best Practices

1. **Pointer Type Selection**
   - Use needle pointers for single value indication (traditional gauge look)
   - Use range bar pointers for progress or filled capacity visualization
   - Use marker pointers for discrete values or when showing multiple simultaneous values

2. **Visual Clarity**
   - Ensure pointer colors contrast well with the gauge background
   - Use appropriate pointer sizes relative to the gauge dimensions
   - Avoid overlapping multiple pointers of the same type

3. **Animation**
   - Use moderate animation durations (500-1500ms) for optimal effect
   - Disable animation if performance is critical or for rapid updates
   - Apply consistent animation duration across similar components

4. **Interactive Pointers**
   - Provide visual feedback when dragging is enabled
   - Set appropriate min/max values to constrain drag behavior
   - Update related UI elements when pointer values change

5. **Gradient Usage**
   - Use gradients sparingly to avoid visual clutter
   - Ensure gradient colors provide adequate contrast
   - Test gradients on different screen sizes and resolutions

6. **Multiple Pointers**
   - Use different pointer types for different data series
   - Position pointers at different radii to prevent overlap
   - Use distinct colors for easy differentiation

## Troubleshooting

### Pointer Not Visible

**Problem:** Pointer is not showing on the gauge.

**Solutions:**
- Verify the `Value` property is within the axis minimum and maximum range
- Check that pointer `Color` contrasts with the background
- Ensure `Radius` is set to a visible position (default 100%)
- Verify pointer dimensions (`MarkerWidth`, `MarkerHeight`, `PointerWidth`) are appropriate

### Dragging Not Working

**Problem:** Pointer drag is not responding.

**Solutions:**
- Ensure `EnablePointerDrag` is set to `true` on the `SfCircularGauge` component
- Check that JavaScript interop is properly configured
- Verify no CSS styles are preventing pointer events
- Ensure the gauge has sufficient size for interaction

### Animation Issues

**Problem:** Pointer animation not smooth or not working.

**Solutions:**
- Set reasonable `Duration` values (500-2000ms recommended)
- Ensure `Enable` is set to `true` in `CircularGaugePointerAnimation`
- Check for JavaScript errors in browser console
- Verify sufficient browser performance

### Image Marker Not Displaying

**Problem:** Image marker pointer shows broken image or doesn't appear.

**Solutions:**
- Verify image path is correct and accessible
- Check image file format is supported (PNG, JPG, SVG)
- Ensure `MarkerShape` is set to `GaugeShape.Image`
- Verify `ImageUrl` property is properly set
- Check image dimensions are appropriate for the marker size

### Gradient Not Rendering

**Problem:** Gradient colors not appearing on pointer.

**Solutions:**
- Verify `ColorStop` elements are properly nested within gradient tags
- Check opacity values are between 0 and 1
- Ensure offset percentages are in correct order (0% to 100%)
- Verify gradient type (Linear or Radial) is appropriate for your use case

### Multiple Pointers Overlapping

**Problem:** Multiple pointers are stacked on each other.

**Solutions:**
- Set different `Radius` values for each pointer
- Use different pointer types for better distinction
- Adjust `Position` property (Inside, Outside, Cross)
- Use appropriate colors and sizes for visual separation
