# Syncfusion Blazor Smith Chart - Complete API Reference

## Table of Contents

- [Overview](#overview)
- [SfSmithChart Component](#sfsmithchart-component)
  - [Properties](#properties)
  - [Public Methods](#public-methods)
- [Events and Event Args](#events-and-event-args)
- [Nested Components and Classes](#nested-components-and-classes)
- [Enumerations](#enumerations)
- [Important Notes](#important-notes)
- [Basic Example](#basic-example)

## Overview

This document provides a Smith Chart focused API reference for Syncfusion Blazor Charts. It summarizes the public `SfSmithChart` surface, common nested components, event arguments, and enums used when working with Smith Chart-based RF and transmission line visualizations.

Namespace: `Syncfusion.Blazor.Charts`

---

## SfSmithChart Component

`SfSmithChart` is the main Blazor Smith Chart component used to display impedance and admittance data in RF and microwave applications.

### Properties

| Property | Type | Description |
| --- | --- | --- |
| `Background` | `string` | Sets the background color of the Smith Chart. |
| `ElementSpacing` | `double` | Sets the space between chart elements. Default: `10`. |
| `Height` | `string` | Sets the chart height. Default: `100%`. |
| `ID` | `string` | Sets the chart element ID. |
| `Radius` | `double` | Sets the chart radius. Default: `1`. |
| `RenderType` | `RenderType` | Sets whether the chart renders in impedance or admittance mode. Default: `Impedance`. |
| `Theme` | `Theme` | Sets the chart theme. Default: `Bootstrap4`. |
| `Width` | `string` | Sets the chart width. Default: `100%`. |

### Public Methods

#### `RefreshAsync(bool isUpdateData = true)`
Re-renders the Smith Chart.

```csharp
public Task RefreshAsync(bool isUpdateData = true)
```

#### `ExportAsync(ExportType type, string fileName, PdfPageOrientation? orientation = null)`
Exports the Smith Chart to the selected output format.

```csharp
public Task ExportAsync(ExportType type, string fileName, PdfPageOrientation? orientation = null)
```

#### `PrintAsync(ElementReference elementRef = default)`
Prints the Smith Chart.

```csharp
public Task PrintAsync(ElementReference elementRef = default)
```

---

## Events and Event Args

The Smith Chart exposes events through `SmithChartEvents`.

### Common events

- `Loaded` → `SmithChartLoadedEventArgs`
- `SeriesRender` → `SmithChartSeriesRenderEventArgs`
- `AxisLabelRendering` → `SmithChartAxisLabelRenderEventArgs`
- `LegendRendering` → `SmithChartLegendRenderEventArgs`
- `TitleRendering` → `TitleRenderEventArgs`
- `SubtitleRendering` → `SubTitleRenderEventArgs`
- `TextRendering` → `SmithChartTextRenderEventArgs`
- `TooltipRender` → `SmithChartTooltipEventArgs`
- `OnExportComplete` → `SmithChartExportEventArgs`
- `OnPrintComplete` → `SmithChartExportEventArgs`
- `SizeChanged` → `SmithChartResizeEventArgs`

### Additional Smith Chart event args

- `SmithChartAnimationCompleteEventArgs`
- `SmithChartResizeEventArgs`
- `SmithChartExportEventArgs`
- `SmithChartPoint`

---

## Nested Components and Classes

### Chart setup

- `SmithChartEvents`
- `SmithChartSeriesCollection`
- `SmithChartSeries`

### Axes

- `SmithChartHorizontalAxis`
- `SmithChartRadialAxis`
- `SmithChartAxisLine`
- `SmithChartHorizontalAxisLine`
- `SmithChartHorizontalAxisLabelStyle`
- `SmithChartRadialAxisLine`
- `SmithChartRadialAxisLabelStyle`
- `SmithChartMajorGridLines`
- `SmithChartMinorGridLines`
- `SmithChartHorizontalMajorGridLines`
- `SmithChartHorizontalMinorGridLines`
- `SmithChartRadialMajorGridLines`
- `SmithChartRadialMinorGridLines`

### Series visuals

- `SmithChartSeriesMarker`
- `SmithChartSeriesMarkerBorder`
- `SmithChartSeriesDatalabel`
- `SmithChartSeriesDataLabelBorder`
- `SmithChartDataLabelConnectorLine`
- `SmithChartDataLabelTextStyle`
- `SmithChartSeriesTooltip`
- `SmithChartSeriesTooltipBorder`

### Titles, legends, and layout

- `SmithChartTitle`
- `SmithChartSubtitle`
- `SmithChartLegendSettings`
- `SmithChartLegendBorder`
- `SmithChartLegendItemStyle`
- `SmithChartLegendTitle`
- `SmithChartLegendTitleTextStyle`
- `SmithChartLegendTextStyle`
- `SmithChartMargin`
- `SmithChartBorder`

### Core data model

- `SmithChartPoint`

---

## Enumerations

- `RenderType` - `Impedance` or `Admittance`
- `SmithChartAlignment` - `Near`, `Center`, `Far`
- `SmithChartLabelIntersectAction`
- `AxisLabelPosition`
- `LegendPosition`
- `Shape`
- `ExportType`

---

## Important Notes

- The Smith Chart component is part of the `Syncfusion.Blazor.Charts` namespace.
- `SmithChartSeriesDatalabel` is the official Smith Chart data label component name.
- `SmithChartSeriesDataLabelBorder` is the official data label border helper component name.
- Smith Chart series are typically bound to resistance/reactance fields.
- Use `RenderType` to switch between impedance and admittance rendering.
- `ExportAsync` and `PrintAsync` are available on the component instance for programmatic output.

---

## Basic Example

```razor
@page "/smithchart-api-reference"
@using Syncfusion.Blazor.Charts

<SfSmithChart RenderType="@RenderType.Impedance" Width="600px" Height="500px">
    <SmithChartTitle Text="Smith Chart API Reference Sample" Visible="true" />
    <SmithChartLegendSettings Visible="true" Position="@Syncfusion.Blazor.Charts.LegendPosition.Bottom" />

    <SmithChartEvents Loaded="OnLoaded"
                      SeriesRender="OnSeriesRender"
                      AxisLabelRendering="OnAxisLabelRendering"
                      LegendRendering="OnLegendRendering"
                      TextRendering="OnTextRendering"
                      TooltipRender="OnTooltipRender" />

    <SmithChartSeriesCollection>
        <SmithChartSeries Name="Impedance Path"
                          DataSource="@Data"
                          Resistance="Resistance"
                          Reactance="Reactance">
            <SmithChartSeriesMarker Visible="true" Shape="@Syncfusion.Blazor.Charts.Shape.Circle" >
                <SmithChartSeriesDatalabel Visible="true">
                    <SmithChartSeriesDataLabelBorder Width="1" Color="#4F46E5" />
                </SmithChartSeriesDatalabel>
            </SmithChartSeriesMarker>
            <SmithChartSeriesTooltip Visible="true" />
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithPoint
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    private List<SmithPoint> Data = new()
    {
        new SmithPoint { Resistance = 1.0, Reactance = 0.0 },
        new SmithPoint { Resistance = 0.8, Reactance = 0.2 },
        new SmithPoint { Resistance = 0.6, Reactance = 0.4 }
    };

    private void OnLoaded(SmithChartLoadedEventArgs args) { }
    private void OnSeriesRender(SmithChartSeriesRenderEventArgs args) { }
    private void OnAxisLabelRendering(SmithChartAxisLabelRenderEventArgs args) { }
    private void OnLegendRendering(SmithChartLegendRenderEventArgs args) { }
    private void OnTextRendering(SmithChartTextRenderEventArgs args) { }
    private void OnTooltipRender(SmithChartTooltipEventArgs args) { }
}
```
