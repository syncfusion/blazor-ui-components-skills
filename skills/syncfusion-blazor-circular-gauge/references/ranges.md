# Circular Gauge Ranges Reference

## Table of Contents
- [Overview](#overview)
- [Creating Ranges](#creating-ranges)
- [Range Customization](#range-customization)
- [Multiple Ranges](#multiple-ranges)
- [Rounded Corners](#rounded-corners)
- [Range Position](#range-position)
- [Radius Configuration](#radius-configuration)
- [Dragging Ranges](#dragging-ranges)
- [Gradient Colors](#gradient-colors)
- [Use Cases](#use-cases)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Ranges in Circular Gauge allow you to categorize and visually distinguish specific intervals on the axis. They provide color-coded zones that help users quickly identify data thresholds, states, or categories. Ranges are particularly useful for creating visual indicators like danger zones, safe zones, or performance levels.

Ranges are defined using the `CircularGaugeRanges` collection and `CircularGaugeRange` component within an axis. Each range can be extensively customized using properties like `Start`, `End`, `Color`, `Radius`, `Position`, and more for appearance and behavior control.

## Creating Ranges

### Basic Range

A range is defined by its `Start` and `End` property values, which correspond to positions on the axis scale.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="40" End="80">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Range Properties

**Essential Properties:**
- `Start` - Beginning value of the range on the axis
- `End` - Ending value of the range on the axis
- `StartWidth` - Width at the range start point
- `EndWidth` - Width at the range end point
- `Color` - Fill color of the range
- `Opacity` - Transparency level (0 to 1)
- `Radius` - Distance from the gauge center
- `Position` - Placement relative to axis (Inside, Outside, Cross)
- `RoundedCornerRadius` - Rounds the range ends

## Range Customization

### Width Customization

Control the visual weight and tapering of ranges using the `StartWidth` and `EndWidth` properties.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="40"
                                    End="80"
                                    StartWidth="2"
                                    EndWidth="20">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Width Tips:**
- Equal `StartWidth` and `EndWidth` create uniform ranges
- Different values create tapered/gradient effects
- Larger widths make ranges more prominent

### Color and Opacity

Apply colors and transparency using the `Color` and `Opacity` properties to create visual hierarchy.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="40"
                                    End="80"
                                    StartWidth="2"
                                    EndWidth="20"
                                    Color="blue"
                                    Opacity="0.2">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Color Best Practices:**
- Use contrasting colors for different ranges
- Apply opacity for overlapping ranges or subtle indicators
- Follow color conventions (red=danger, green=safe, yellow=warning)

## Multiple Ranges

Create comprehensive visual guides by adding multiple `CircularGaugeRange` components with distinct `Start`, `End`, and `Radius` property values.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="25" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="25" End="50" Radius="70%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="75" Radius="70%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="75" End="100" Radius="108%">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Applying Range Colors to Axis Elements

Use `UseRangeColor` property to apply range colors to axis labels, major ticks, and minor ticks.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="25" Radius="108%" Color="#FF6B6B">
                </CircularGaugeRange>
                <CircularGaugeRange Start="25" End="50" Radius="70%" Color="#FFD93D">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="75" Radius="70%" Color="#6BCB77">
                </CircularGaugeRange>
                <CircularGaugeRange Start="75" End="100" Radius="108%" Color="#4D96FF">
                </CircularGaugeRange>
            </CircularGaugeRanges>
            <CircularGaugeAxisLabelStyle UseRangeColor="true">
            </CircularGaugeAxisLabelStyle>
            <CircularGaugeAxisMinorTicks UseRangeColor="true">
            </CircularGaugeAxisMinorTicks>
            <CircularGaugeAxisMajorTicks UseRangeColor="true">
            </CircularGaugeAxisMajorTicks>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Rounded Corners

Create smooth, arc-shaped range ends using the `RoundedCornerRadius` property on the `CircularGaugeRange` component.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="40" 
                                    End="80" 
                                    RoundedCornerRadius="5">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Rounded Corner Tips:**
- Larger radius values create more pronounced curves
- Works best with wider ranges (`StartWidth` and `EndWidth`)
- Provides a modern, polished appearance

## Range Position

Control range placement relative to the axis using the `Position` property on the `CircularGaugeRange` component with `PointerRangePosition` enum values.

### Position Values
- `PointerRangePosition.Inside` - Range positioned inside the axis
- `PointerRangePosition.Outside` - Range positioned outside the axis
- `PointerRangePosition.Cross` - Range crosses the axis line

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="40" 
                                    End="80" 
                                    StartWidth="15" 
                                    EndWidth="15" 
                                    Color="#ff5985" 
                                    Position="PointerRangePosition.Cross">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Radius Configuration

The `Radius` property determines how far the range is positioned from the gauge center.

### Radius in Percentage

By default, ranges use percentage-based `Radius` property values relative to the axis radius.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="40" 
                                    End="80" 
                                    Radius="50%">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Radius in Pixels

Set absolute positioning for the `Radius` property using pixel values.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="40" 
                                    End="80" 
                                    Radius="100px">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Radius Tips:**
- Values greater than 100% place ranges outside the axis
- Values less than 100% place ranges inside the axis
- Different radii for multiple ranges create layered effects

## Dragging Ranges

Enable interactive range adjustment by setting the `EnableRangeDrag` property to true on the `SfCircularGauge` component, allowing users to drag ranges along the axis.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge EnableRangeDrag="true" Height="250px" Width="250px">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="50">
                </CircularGaugePointer>
            </CircularGaugePointers>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" 
                                    End="100" 
                                    Radius="108%" 
                                    Color="#30B32D" 
                                    StartWidth="8" 
                                    EndWidth="8">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Drag Functionality:**
- Set `EnableRangeDrag="true"` on the main gauge component
- Users can click and drag ranges to adjust start/end values
- Useful for interactive threshold setting or parameter adjustment

## Gradient Colors

Apply linear or radial gradients to ranges using `LinearGradient` or `RadialGradient` components with `ColorStop` elements for sophisticated visual effects.

### Linear Gradient

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge CenterY="57%">
    <CircularGaugeAxes>
        <CircularGaugeAxis StartAngle="200" EndAngle="130" Minimum="0" Maximum="14" Radius="80%">
            <CircularGaugeAxisLineStyle Width="0.001" />
            <CircularGaugeAxisMajorTicks Width="0.01" />
            <CircularGaugeAxisMinorTicks Width="0.01" />
            <CircularGaugeAxisLabelStyle>
                <CircularGaugeAxisLabelFont Size="0px" />
            </CircularGaugeAxisLabelStyle>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="12" Radius="105%" StartWidth="25" EndWidth="25">
                    <LinearGradient StartValue="1%" EndValue="99%">
                        <ColorStops>
                            <ColorStop Opacity="0.9" Offset="0%" Color="#fef3f9">
                            </ColorStop>
                            <ColorStop Opacity="0.9" Offset="100%" Color="#f54ea2">
                            </ColorStop>
                        </ColorStops>
                    </LinearGradient>
                </CircularGaugeRange>
                <CircularGaugeRange Start="0" End="11" Radius="75%" StartWidth="25" EndWidth="25">
                    <LinearGradient StartValue="1%" EndValue="99%">
                        <ColorStops>
                            <ColorStop Opacity="0.9" Offset="0%" Color="#fef3f9">
                            </ColorStop>
                            <ColorStop Opacity="0.9" Offset="100%" Color="#f54ea2">
                            </ColorStop>
                        </ColorStops>
                    </LinearGradient>
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Radial Gradient

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge CenterY="57%">
    <CircularGaugeAxes>
        <CircularGaugeAxis StartAngle="200" EndAngle="130" Minimum="0" Maximum="14" Radius="80%">
            <CircularGaugeAxisLineStyle Width="0.001" />
            <CircularGaugeAxisMajorTicks Width="0.01" />
            <CircularGaugeAxisMinorTicks Width="0.01" />
            <CircularGaugeAxisLabelStyle>
                <CircularGaugeAxisLabelFont Size="0px" />
            </CircularGaugeAxisLabelStyle>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="12" Radius="105%" StartWidth="25" EndWidth="25">
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
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Gradient Properties:**

**Linear Gradient:**
- `StartValue` - Gradient start position (percentage)
- `EndValue` - Gradient end position (percentage)
- `ColorStop` - Color transitions with offset and opacity

**Radial Gradient:**
- `InnerPosition` - Center point coordinates (X, Y percentages)
- `OuterPosition` - Outer circle coordinates (X, Y percentages)
- `Radius` - Gradient spread radius
- `ColorStop` - Color transitions with offset and opacity

## Use Cases

### Performance Indicator
Create color-coded zones (red/yellow/green) to indicate poor, average, and excellent performance ranges. Use with pointers to show current status.

### Temperature Zones
Define comfort, warning, and danger zones for temperature gauges in HVAC systems or industrial monitoring.

### Speed Limits
Display speed ranges with different colors representing safe, caution, and dangerous speeds on vehicle dashboards.

### Battery Level Indicator
Show battery charge levels with multiple ranges: critical (red), low (yellow), medium (orange), and full (green).

### Pressure Monitoring
Industrial applications can use ranges to indicate safe operating pressures, warning zones, and critical thresholds.

### Progress Tracking
Combine range bars with pointers to show progress through different project phases or completion stages.

## Best Practices

1. **Range Definition**
   - Ensure ranges don't overlap unless intentional
   - Use continuous ranges to cover the entire axis scale
   - Set logical start and end values that align with axis minimum and maximum

2. **Color Selection**
   - Use intuitive colors (red=bad, green=good, yellow=caution)
   - Maintain sufficient contrast between adjacent ranges
   - Consider color-blind friendly palettes for accessibility
   - Test colors on different backgrounds and themes

3. **Visual Hierarchy**
   - Use width variations to emphasize important ranges
   - Apply different radii to create layered visualizations
   - Use opacity to de-emphasize less critical ranges

4. **Multiple Ranges**
   - Limit the number of ranges to avoid visual clutter (3-5 recommended)
   - Use consistent width for ranges representing the same data type
   - Consider alternating positions (inside/outside) for better visibility

5. **Interactive Ranges**
   - Provide visual feedback when drag is enabled
   - Document drag behavior for users
   - Set constraints to prevent invalid range configurations

6. **Performance**
   - Minimize gradient complexity for better rendering performance
   - Use solid colors when gradients aren't necessary
   - Optimize the number of ranges for mobile devices

## Troubleshooting

### Range Not Visible

**Problem:** Range is defined but not appearing on the gauge.

**Solutions:**
- Verify `Start` and `End` values are within axis minimum and maximum
- Check that range `Color` contrasts with the background
- Ensure `StartWidth` and `EndWidth` are greater than 0
- Verify `Radius` places the range in a visible location

### Ranges Overlapping Incorrectly

**Problem:** Multiple ranges are overlapping in unexpected ways.

**Solutions:**
- Set different `Radius` values for each range to create layers
- Use `Position` property to control placement (Inside/Outside/Cross)
- Adjust `StartWidth` and `EndWidth` to reduce overlap
- Apply `Opacity` to semi-transparent ranges

### Range Colors Not Applying to Labels

**Problem:** `UseRangeColor` not working on axis elements.

**Solutions:**
- Ensure `UseRangeColor="true"` is set on the specific axis element (labels, ticks)
- Verify ranges fully cover the areas where labels appear
- Check that range colors are properly defined
- Ensure axis labels are within range boundaries

### Gradient Not Rendering

**Problem:** Gradient colors not displaying on ranges.

**Solutions:**
- Verify `ColorStop` elements are properly nested within gradient tags
- Check that offset percentages progress from 0% to 100%
- Ensure opacity values are between 0 and 1
- For radial gradients, verify `InnerPosition` and `OuterPosition` are valid

### Dragging Not Working

**Problem:** Range drag functionality not responding.

**Solutions:**
- Ensure `EnableRangeDrag="true"` is set on the `SfCircularGauge` component
- Verify JavaScript interop is properly configured
- Check that ranges have sufficient width for interaction
- Ensure no CSS is blocking pointer events

### Rounded Corners Not Smooth

**Problem:** Rounded corners appear jagged or incorrect.

**Solutions:**
- Increase `RoundedCornerRadius` value for more pronounced effect
- Ensure range width is sufficient for the corner radius
- Check browser rendering capabilities
- Verify SVG rendering is enabled

### Performance Issues with Multiple Ranges

**Problem:** Gauge renders slowly with many ranges.

**Solutions:**
- Reduce the number of ranges (combine similar ones)
- Simplify or remove gradients
- Decrease animation duration or disable animations
- Optimize range widths and complexity
- Consider server-side rendering for initial load
