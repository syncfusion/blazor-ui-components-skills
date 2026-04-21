# API Reference

## Table of Contents
- [Overview](#overview)
- [SfStockChart](#sfstockchart)
- [Key Properties](#key-properties)
- [Methods](#methods)
- [Child Components](#child-components)
- [Events](#events)
- [Common Enums](#common-enums)

## Overview

This reference summarizes the validated public API for Syncfusion Blazor Stock Chart based on the official `Syncfusion.Blazor.Charts` documentation.

## SfStockChart

`SfStockChart` is the root component used to render financial charts for stock and market data.

### Main capabilities

- Financial series visualization with `ChartSeriesType`
- Period selector and range selector support
- Technical indicators and trend lines
- Stock events and annotations
- Zooming, panning, crosshair, tooltip, and export features

## Key Properties

| Property | Type | Purpose |
|----------|------|---------|
| `Background` | string | Sets the chart background color |
| `DataSource` | IEnumerable<object> | Binds the chart data |
| `EnableCustomRange` | bool | Enables custom date range selection |
| `EnablePeriodSelector` | bool | Shows or hides the period selector |
| `EnableRtl` | bool | Enables right-to-left rendering |
| `EnableSelector` | bool | Shows or hides the range selector |
| `ExportType` | List<ExportType> | Configures available export formats |
| `Height` | string | Sets the chart height |
| `ID` | string | Sets the component ID |
| `IndicatorType` | List<TechnicalIndicators> | Selects technical indicators |
| `IsMultiSelect` | bool | Enables multi-selection |
| `IsSelect` | bool | Enables selection |
| `IsTransposed` | bool | Renders the chart in transposed mode |
| `NoDataTemplate` | RenderFragment | Displays content when no data is available |
| `SelectionMode` | SelectionMode | Sets the chart selection mode |
| `SeriesType` | List<ChartSeriesType> | Selects the available stock chart series types |
| `Theme` | Theme | Sets the chart theme |
| `Title` | string | Sets the chart title |
| `TrendlineType` | List<TrendlineTypes> | Selects the available trend line types |
| `Width` | string | Sets the chart width |

## Methods

| Method | Signature | Purpose |
|--------|-----------|---------|
| `ExportAsync()` | `Task ExportAsync(ExportType type, string fileName, PdfPageOrientation? orientation = null, bool allowDownload = true, bool isBase64 = false)` | Exports the rendered stock chart |
| `PrintAsync()` | `Task PrintAsync(ElementReference elementRef = default)` | Prints the rendered stock chart |
| `Refresh()` | `void Refresh()` | Re-renders the chart |
| `UpdateStockChart()` | `void UpdateStockChart()` | Refreshes the stock chart UI element |

## Child Components

| Component | Purpose |
|-----------|---------|
| `StockChartAnnotations` | Adds annotations to the chart |
| `StockChartAxes` | Configures custom axes |
| `StockChartEvents` | Handles chart events |
| `StockChartIndicators` | Adds technical indicators |
| `StockChartPeriods` | Configures period selector buttons |
| `StockChartPrimaryXAxis` | Configures the primary X axis |
| `StockChartPrimaryYAxis` | Configures the primary Y axis |
| `StockChartRows` | Defines chart rows for multi-panel layouts |
| `StockChartSeriesCollection` | Contains one or more stock series |
| `StockChartStockEvents` | Displays corporate or market events |
| `StockChartTooltipSettings` | Configures tooltip appearance and behavior |
| `StockChartTrendlines` | Adds trend lines to the chart |
| `StockChartZoomSettings` | Configures zoom and pan behavior |

## Events

The `StockChartEvents` component exposes these validated event callbacks:

| Event | Argument Type | Purpose |
|------|---------------|---------|
| `AxisLabelRendering` | `StockChartAxisLabelRenderEventArgs` | Customizes axis labels before rendering |
| `Exporting` | `ChartExportEventArgs` | Fires when export starts |
| `ExportCompleted` | `ExportEventArgs` | Fires when export completes |
| `OnLoaded` | `StockChartEventArgs` | Fires after the chart finishes rendering |
| `OnPointClick` | `StockChartPointEventArgs` | Fires when a point is clicked |
| `OnPrintComplete` | `PrintEventArgs` | Fires when printing completes |
| `OnZooming` | `StockChartZoomingEventArgs` | Fires after a zoom selection is made |
| `PeriodChanged` | `StockChartPeriodChangedEventArgs` | Fires when a period is selected |
| `RangeChange` | `StockChartRangeChangeEventArgs` | Fires when the visible range changes |
| `SharedTooltipRendering` | `SharedTooltipRenderEventArgs` | Fires before shared tooltip rendering |
| `TooltipRendering` | `TooltipRenderEventArgs` | Fires before tooltip rendering |

## Common Enums

| Enum | Purpose |
|------|---------|
| `ChartSeriesType` | Defines series types such as `Line`, `Spline`, `Candle`, `HiloOpenClose`, and `Hilo` |
| `ExportType` | Defines export formats such as `PNG`, `JPEG`, `SVG`, and `PDF` |
| `RangeIntervalType` | Defines period selector intervals |
| `SelectionMode` | Defines chart selection behavior |
| `TechnicalIndicators` | Defines technical indicator types |
| `Theme` | Defines available chart themes |
| `TrendlineTypes` | Defines supported trend line types |
| `ValueType` | Defines axis value types |
| `VisibleType` | Defines visible type options used by related stock chart components |
