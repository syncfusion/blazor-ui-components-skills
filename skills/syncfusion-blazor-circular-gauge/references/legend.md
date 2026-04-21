# Circular Gauge Legend Reference

## Table of Contents
- [Overview](#overview)
- [Enabling Legend](#enabling-legend)
- [Legend Customization](#legend-customization)
- [Position and Alignment](#position-and-alignment)
- [Legend Size](#legend-size)
- [Legend Shape](#legend-shape)
- [Legend Text](#legend-text)
- [Toggle Functionality](#toggle-functionality)
- [Paging Support](#paging-support)
- [Use Cases](#use-cases)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview
The legend in Circular Gauge provides a visual guide that helps users interpret the meaning of ranges displayed on the gauge; it is configured via `CircularGaugeLegendSettings` (for example `Visible`, `Position`, `Alignment`). Legends show color-coded symbols corresponding to each range, along with descriptive text, making it easier to understand what different axis range zones represent.

Legends are particularly valuable when you have multiple ranges with different colors representing various states, categories, or thresholds.

## Enabling Legend

Enable the legend by setting the `Visible` property to `true` in `CircularGaugeLegendSettings`.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeLegendSettings Visible="true">
    </CircularGaugeLegendSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="25" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="25" End="50" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="75" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="75" End="100" Radius="108%">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Legend Customization
Configure legend appearance and behavior through `CircularGaugeLegendSettings` properties such as `ShapeWidth`, `Padding`, `Opacity`, `Position`, and `Alignment`.

### Basic Legend Properties

**Core Properties:**
- `Visible` - Enable or disable the legend
- `Position` - Legend placement (Top, Bottom, Left, Right, Custom, Auto)
- `Alignment` - Legend alignment within the position (Near, Center, Far)
- `Height` - Legend container height
- `Width` - Legend container width
- `ShapeHeight` - Height of legend symbols
- `ShapeWidth` - Width of legend symbols
- `Padding` - Space between legend items
- `Opacity` - Transparency of legend symbols

### Complete Customization Example

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeLegendSettings Visible="true" 
                                 ShapeWidth="30" 
                                 ShapeHeight="30" 
                                 Padding="15">
        <CircularGaugeLegendBorder Color="green" Width="3">
        </CircularGaugeLegendBorder>
    </CircularGaugeLegendSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeAxisMajorTicks UseRangeColor="true">
            </CircularGaugeAxisMajorTicks>
            <CircularGaugeAxisMinorTicks UseRangeColor="true">
            </CircularGaugeAxisMinorTicks>
            <CircularGaugeAxisLabelStyle UseRangeColor="true">
            </CircularGaugeAxisLabelStyle>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="25" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="25" End="50" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="75" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="75" End="100" Radius="108%">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Position and Alignment
Use the `Position` and `Alignment` properties on `CircularGaugeLegendSettings` to control placement; use `CircularGaugeLegendLocation` for custom coordinates.

### Position Options

Control where the legend appears relative to the gauge by setting the `Position` property on `CircularGaugeLegendSettings`.

**Available Positions:**
- `Top` - Above the gauge
- `Bottom` - Below the gauge
- `Left` - Left side of the gauge
- `Right` - Right side of the gauge
- `Custom` - Custom location using X and Y coordinates
- `Auto` - Automatically determined based on available space

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeLegendSettings Visible="true" Position="Syncfusion.Blazor.CircularGauge.LegendPosition.Bottom">
    </CircularGaugeLegendSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="50" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="100" Radius="108%">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Alignment Options

Control how legend items are aligned within their container using the `Alignment` property on `CircularGaugeLegendSettings`.

**Alignment Values:**
- `Near` - Align to the start (left/top)
- `Center` - Center alignment
- `Far` - Align to the end (right/bottom)

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeLegendSettings Visible="true" 
                                 Position="Syncfusion.Blazor.CircularGauge.LegendPosition.Bottom"
                                 Alignment="Syncfusion.Blazor.CircularGauge.Alignment.Center">
    </CircularGaugeLegendSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="50" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="100" Radius="108%">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Custom Positioning

Set `Position` to `Syncfusion.Blazor.CircularGauge.LegendPosition.Custom` and provide `CircularGaugeLegendLocation` (`X`, `Y`) for absolute positioning.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeLegendSettings Visible="true" 
                                 Position="Syncfusion.Blazor.CircularGauge.LegendPosition.Custom">
        <CircularGaugeLegendLocation X="100" Y="50">
        </CircularGaugeLegendLocation>
    </CircularGaugeLegendSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="50" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="100" Radius="108%">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Legend Size

Control the dimensions of the legend container using the `Height` and `Width` properties on `CircularGaugeLegendSettings`.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeLegendSettings Visible="true" 
                                 Height="100" 
                                 Width="300">
    </CircularGaugeLegendSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="25" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="25" End="50" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="75" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="75" End="100" Radius="108%">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Legend Shape

Customize the appearance of legend symbols using the `Shape`, `ShapeHeight`, `ShapeWidth`, and `Opacity` properties on `CircularGaugeLegendSettings`.

### Shape Types
-Specify the symbol shape via the `Shape` property on `CircularGaugeLegendSettings`.

**Available Shapes:**
- `Circle` - Circular symbol
- `Rectangle` - Rectangular symbol
- `Diamond` - Diamond-shaped symbol
- `Triangle` - Triangular symbol
- `InvertedTriangle` - Inverted triangle symbol
- `Image` - Custom image symbol

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeLegendSettings Visible="true" 
                                 Shape="GaugeShape.Diamond"
                                 ShapeHeight="20"
                                 ShapeWidth="20">
    </CircularGaugeLegendSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="50" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="100" Radius="108%">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Shape Dimensions

Control symbol size with `ShapeHeight` and `ShapeWidth`.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeLegendSettings Visible="true" 
                                 Shape="GaugeShape.Rectangle"
                                 ShapeHeight="15"
                                 ShapeWidth="30">
    </CircularGaugeLegendSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="50" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="100" Radius="108%">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Legend Opacity

Adjust transparency of legend symbols.

Set the transparency via the `Opacity` property on `CircularGaugeLegendSettings`.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeLegendSettings Visible="true" Opacity="0.7">
    </CircularGaugeLegendSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="50" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="100" Radius="108%">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Legend Text

### Custom Legend Text

Set custom text for each range using the `LegendText` property on `CircularGaugeRange` (e.g., `CircularGaugeRange.LegendText`).

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeLegendSettings Visible="true" Height="50">
        <CircularGaugeLegendBorder Color="green" Width="3">
        </CircularGaugeLegendBorder>
    </CircularGaugeLegendSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeAxisMajorTicks UseRangeColor="true">
            </CircularGaugeAxisMajorTicks>
            <CircularGaugeAxisMinorTicks UseRangeColor="true">
            </CircularGaugeAxisMinorTicks>
            <CircularGaugeAxisLabelStyle UseRangeColor="true">
            </CircularGaugeAxisLabelStyle>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="25" Radius="108%" LegendText="Light Air">
                </CircularGaugeRange>
                <CircularGaugeRange Start="25" End="50" Radius="108%" LegendText="Calm">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="75" Radius="108%" LegendText="Light Breeze">
                </CircularGaugeRange>
                <CircularGaugeRange Start="75" End="100" Radius="108%" LegendText="Gentle Breeze">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Text Style Customization

Customize legend text appearance using `CircularGaugeLegendTextStyle` inside `CircularGaugeLegendSettings` (properties include `Size`, `Color`, `FontWeight`, `FontFamily`, `FontStyle`, `Opacity`).

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeLegendSettings Visible="true">
        <CircularGaugeLegendTextStyle Size="14px"
                                      Color="#333"
                                      FontWeight="600"
                                      FontFamily="Arial"
                                      FontStyle="normal"
                                      Opacity="0.9">
        </CircularGaugeLegendTextStyle>
    </CircularGaugeLegendSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="50" Radius="108%" LegendText="Normal">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="100" Radius="108%" LegendText="Warning">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Font Properties:**
- `Size` - Font size
- `Color` - Text color
- `FontWeight` - Bold, normal, or numeric weight
- `FontFamily` - Font family name
- `FontStyle` - Normal, italic, or oblique
- `Opacity` - Text transparency (0 to 1)

## Toggle Functionality

Enable interactive legend toggling to show/hide corresponding ranges by setting `ToggleVisibility` on `CircularGaugeLegendSettings`.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeLegendSettings Visible="true" ToggleVisibility="true">
        <CircularGaugeLegendBorder Color="green" Width="3">
        </CircularGaugeLegendBorder>
    </CircularGaugeLegendSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeAxisMajorTicks UseRangeColor="true">
            </CircularGaugeAxisMajorTicks>
            <CircularGaugeAxisMinorTicks UseRangeColor="true">
            </CircularGaugeAxisMinorTicks>
            <CircularGaugeAxisLabelStyle UseRangeColor="true">
            </CircularGaugeAxisLabelStyle>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="25" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="25" End="50" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="75" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="75" End="100" Radius="108%">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Toggle Features:**
- Click legend items to show/hide ranges
- Clicked items appear dimmed
- Corresponding ranges are hidden on the gauge
- Useful for focusing on specific data ranges

## Paging Support

When legend items exceed the container size, paging is automatically enabled; paging behavior is influenced by the legend `Height` and available container space (no separate paging property).

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeLegendSettings Visible="true" Height="50">
        <CircularGaugeLegendBorder Color="green" Width="3">
        </CircularGaugeLegendBorder>
    </CircularGaugeLegendSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeAxisMajorTicks UseRangeColor="true">
            </CircularGaugeAxisMajorTicks>
            <CircularGaugeAxisMinorTicks UseRangeColor="true">
            </CircularGaugeAxisMinorTicks>
            <CircularGaugeAxisLabelStyle UseRangeColor="true">
            </CircularGaugeAxisLabelStyle>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="25" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="25" End="50" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="75" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="75" End="100" Radius="108%">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Paging Features:**
- Automatic paging when items overflow
- Navigation buttons to move between pages
- Configurable legend height determines item capacity
- Seamless navigation between legend pages

## Legend Border and Padding
Configure legend border using `CircularGaugeLegendBorder` (`Color`, `Width`) and item spacing using the `Padding` property on `CircularGaugeLegendSettings`.

### Border Styling

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeLegendSettings Visible="true">
        <CircularGaugeLegendBorder Color="#007bff" Width="2">
        </CircularGaugeLegendBorder>
    </CircularGaugeLegendSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="50" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="100" Radius="108%">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Item Padding

Control spacing between legend items.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeLegendSettings Visible="true" Padding="20">
    </CircularGaugeLegendSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="50" Radius="108%">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="100" Radius="108%">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Use Cases

These examples typically use `CircularGaugeRange.LegendText`, legend `Shape`, `Position`, and color settings to communicate range meaning.

### Status Indicators
Display legends for status ranges (Critical, Warning, Normal, Optimal) with appropriate colors.

### Performance Zones
Show performance categories (Poor, Average, Good, Excellent) for KPI dashboards.

### Temperature Ranges
Indicate temperature zones (Cold, Cool, Moderate, Warm, Hot) for climate monitoring.

### Speed Categories
Display speed classifications (Slow, Moderate, Fast, Very Fast) for vehicle dashboards.

### Threshold Documentation
Provide clear documentation of threshold ranges for operational parameters.

### Multi-Level Classification
Show categorical breakdowns for complex data classifications.

## Best Practices
Apply properties like `LegendText`, `Padding`, `ShapeWidth`, `ShapeHeight`, `Opacity`, and `Position` when following these recommendations.

1. **Descriptive Text**
   - Use clear, concise legend text
   - Avoid abbreviations unless space-constrained
   - Ensure text accurately describes the range

2. **Position Selection**
   - Place legends where they don't obscure the gauge
   - Bottom position works well for most layouts
   - Use auto position for responsive designs

3. **Visual Clarity**
   - Choose shape types that render clearly at small sizes
   - Ensure adequate spacing with padding
   - Use appropriate symbol dimensions

4. **Color Coordination**
   - Ensure legend colors match range colors exactly
   - Test color combinations for accessibility
   - Consider color-blind friendly palettes

5. **Interactive Features**
   - Enable toggle for interactive data exploration
   - Provide visual feedback for toggled items
   - Document toggle functionality for users

6. **Responsive Design**
   - Set appropriate height for different screen sizes
   - Test paging behavior with various legend item counts
   - Consider legend position for mobile layouts

## Troubleshooting
Use the legend-related properties such as `Visible`, `LegendText`, `ToggleVisibility`, `Position`, and `Padding` when diagnosing issues.

### Legend Not Visible

**Problem:** Legend doesn't appear even when `Visible="true"`.

**Solutions:**
- Verify ranges are defined in the gauge axes
- Check that ranges have valid start and end values
- Ensure the gauge container has sufficient space
- Verify no CSS is hiding the legend element

### Legend Text Not Showing

**Problem:** Legend symbols appear but no text labels.

**Solutions:**
- Set `LegendText` property on ranges if not using default text
- Check text color isn't matching the background
- Verify font size is appropriate
- Ensure legend width provides sufficient space for text

### Legend Items Overlapping

**Problem:** Legend items overlap or appear cramped.

**Solutions:**
- Increase `Padding` value for more spacing
- Adjust legend `Width` and `Height`
- Reduce `ShapeWidth` and `ShapeHeight` if too large
- Consider smaller font size for text

### Toggle Not Working

**Problem:** Clicking legend items doesn't toggle ranges.

**Solutions:**
- Ensure `ToggleVisibility="true"` is set
- Verify JavaScript interop is properly configured
- Check browser console for errors
- Test with a simple gauge configuration first

### Paging Navigation Missing

**Problem:** Paging buttons don't appear with many items.

**Solutions:**
- Reduce legend `Height` to trigger paging
- Verify sufficient legend items exist to require paging
- Check legend container is not too large
- Ensure CSS isn't hiding navigation buttons

### Custom Position Not Working

**Problem:** Legend doesn't move to custom coordinates.

**Solutions:**
- Verify `Position="Syncfusion.Blazor.CircularGauge.LegendPosition.Custom"` is set
- Check `Location` X and Y values are within gauge bounds
- Ensure values are appropriate for the gauge size
- Consider using percentage-based positioning

### Border Not Visible

**Problem:** Legend border doesn't appear.

**Solutions:**
- Ensure `Width` in `CircularGaugeLegendBorder` is greater than 0
- Check `Color` contrasts with the background
- Verify border isn't outside the visible area
- Increase border width for better visibility

### Shape Not Rendering Correctly

**Problem:** Legend shapes appear distorted or incorrect.

**Solutions:**
- Verify `Shape` property is set to a valid GaugeShape value
- Check `ShapeHeight` and `ShapeWidth` are proportional
- Ensure sufficient space in legend container
- Test with default shape (Circle) first
