# Sparkline API Reference

## Table of Contents
- [Overview](#overview)
- [Component](#component)
    - [SfSparkline](#sfsparkline)
- [Child Components](#child-components)
- [Enumerations](#enumerations)
- [Notes](#notes)
- [Basic Example](#basic-example)

## Overview

This page summarizes the public API surface for Syncfusion Blazor Sparkline Charts. Use it as the quick reference for component properties, methods, events, child tags, and enumerations.

## Component

### `SfSparkline`

Primary component for rendering compact trend visualizations.

#### Core Properties
- `DataSource` - Data collection used to render the sparkline
- `EnableGroupingSeparator` - Formats large numeric values with separators
- `EnableRtl` - Enables right-to-left rendering
- `EndPointColor` - Color for the last point
- `Fill` - Primary fill or line color
- `Format` - Value format string
- `Height` - Component height
- `HighPointColor` - Color for the highest point
- `ID` - DOM identifier
- `LineWidth` - Thickness of the line stroke
- `LowPointColor` - Color for the lowest point
- `NegativePointColor` - Color for negative values
- `Opacity` - Opacity for the fill color
- `Palette` - Color palette for chart elements
- `Query` - Data query for filtered data binding
- `RangePadding` - Padding behavior for the value range
- `StartPointColor` - Color for the first point
- `Theme` - Theme applied to the component
- `TiePointColor` - Color for tie points in WinLoss sparklines
- `Type` - Sparkline type
- `ValueType` - Value interpretation mode
- `Width` - Component width
- `XName` - Field mapped to the X axis value
- `YName` - Field mapped to the Y axis value

#### Methods
- `RefreshAsync()` - Re-renders the sparkline programmatically

#### Events
- `OnAxisRendering` - Customizes the axis before rendering
- `OnDataLabelRendering` - Customizes data labels before rendering
- `OnLoaded` - Occurs after the sparkline is loaded and rendered
- `OnMarkerRendering` - Customizes markers before rendering
- `OnPointRegionMouseClick` - Occurs when a point region is clicked
- `OnPointRendering` - Customizes each point before rendering
- `OnResizing` - Occurs when the component is resized
- `OnSeriesRendering` - Customizes the sparkline series before rendering

## Child Components

Use these nested tags to configure the sparkline:

- `SparklineAxisLineSettings`
- `SparklineAxisSettings`
- `SparklineBorder`
- `SparklineContainerArea`
- `SparklineContainerAreaBorder`
- `SparklineDataLabelBorder`
- `SparklineDataLabelOffset`
- `SparklineDataLabelSettings`
- `SparklineEvents`
- `SparklineFont`
- `SparklineMarkerBorder`
- `SparklineMarkerSettings`
- `SparklinePadding`
- `SparklineRangeBand`
- `SparklineRangeBandSettings`
- `SparklineTooltipBorder`
- `SparklineTooltipSettings`
- `SparklineTooltipTextStyle`
- `SparklineTrackLineSettings`

## Enumerations

- `SparklineRangePadding`
- `SparklineType`
- `SparklineValueType`
- `VisibleType`

## Notes

- Sparklines do not use the full chart API surface; keep implementations compact and focused.
- Prefer `OnLoaded` and `RefreshAsync()` for lifecycle-aware updates.
- Use `VisibleType` values such as `All`, `High`, `Low`, `Start`, `End`, and `Negative` to target special points.

## Basic Example

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@SalesData"
              XName="Month"
              YName="Value"
              Type="SparklineType.Line"
              ValueType="SparklineValueType.Category"
              Height="60px"
              Width="180px"
              Fill="#3f51b5"
              LineWidth="2">
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }" Size="4"></SparklineMarkerSettings>
    <SparklineTooltipSettings TValue="SalesInfo" Visible="true"></SparklineTooltipSettings>
    <SparklineEvents OnLoaded="@OnSparklineLoaded"></SparklineEvents>
</SfSparkline>

@code {
    public class SalesInfo
    {
        public string Month { get; set; }
        public double Value { get; set; }
    }

    private List<SalesInfo> SalesData = new()
    {
        new SalesInfo { Month = "Jan", Value = 12 },
        new SalesInfo { Month = "Feb", Value = 18 },
        new SalesInfo { Month = "Mar", Value = 15 }
    };

    private void OnSparklineLoaded(System.EventArgs args)
    {
    }
}
```
