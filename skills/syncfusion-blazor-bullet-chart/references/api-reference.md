<!-- Auto-generated API reference summary for Syncfusion.Blazor.Charts (Bullet Chart) -->
# API Reference — Syncfusion.Blazor.Charts (Bullet Chart)

## Table of Contents
- [Overview](#overview)
- [SfBulletChart<TValue>](#sfbulletcharttvalue)
- [Core Components](#core-components)
  - [BulletChartRange](#bulletchartrange)
  - [BulletChartDataLabel](#bulletchartdatalabel)
  - [BulletChartTooltip](#bulletcharttooltip)
  - [BulletChartRangeCollection](#bulletchartrangecollection)
- [Appearance & Layout](#appearance--layout)
- [Events](#events)
- [Important Enums](#important-enums)
- [Quick Examples](#quick-examples)

## Overview
This file summarizes the primary API surface in the `Syncfusion.Blazor.Charts` namespace that is relevant for `SfBulletChart` usage. For full API docs consult the official Syncfusion reference.

## SfBulletChart<TValue>
Main component for rendering the Bullet Chart. Key parameters:
- `DataSource` — object: data collection (List, IEnumerable, remote DataManager)
- `ValueField` — string: property name used for the actual (feature) value
- `TargetField` — string: property name used for target/comparative value
- `CategoryField` — string: property name for category (when showing multiple bullets)
- `Minimum`, `Maximum`, `Interval` — double: axis scale configuration
- `Orientation` — `OrientationType`: Horizontal or Vertical
- `Type` — `FeatureType`: Actual bar shape (Rect, Dot)
- `TargetTypes` — List<TargetType>: Marker shapes for targets
- `Width`, `Height`, `Title`, `Theme` — visual settings

Methods & lifecycle events (component-level):
- `OnInit` / `OnParametersSet` / `Dispose` (standard Blazor lifecycle)
- `Loaded` / `Resized` / tooltip/legend-related callbacks exposed via `BulletChartEvents`

## Core Components

### BulletChartRange
Represents a qualitative background range. Important properties:
- `End` — double: range end value
- `Color` — string: fill color
- `Opacity` — double: transparency

### BulletChartDataLabel
Options to render data labels on the actual bar. Key props:
- `Enable` — bool
- `Template` — RenderFragment or string
- `TextStyle` / `BulletChartDataLabelStyle` — font/color

### BulletChartTooltip
Tooltip configuration for bullet chart points. Key props:
- `Enable` — bool
- `Template` — custom markup
- `BulletChartTooltipTextStyle` — font/color

### BulletChartRangeCollection
Container for multiple `BulletChartRange` entries. Use inside `SfBulletChart` to declare ranges.

## Appearance & Layout
- `BulletChartBorder`, `BulletChartMargin`, `BulletChartMajorTickLines`, `BulletChartMinorTickLines`, `BulletChartCategoryLabelStyle` — controls for ticks, labels, margins and borders.
- `BulletChartLegendSettings`, `BulletChartLegendLocation`, `BulletChartLegendTextStyle` — legend control when multiple series/categories are present.

## Events
The bullet chart exposes events through `BulletChartEvents` and related event-arg classes. Common events:
- `Loaded` / `OnLoaded` — chart initialization complete
- `TooltipRender` / `BulletChartTooltipEventArgs` — customize tooltip content
- `LabelRender` / `BulletChartLabelRenderEventArgs` — modify label text or styling
- `LegendRender` / `BulletChartLegendRenderEventArgs`
- `PointClick` / `BulletChartPointEventArgs`

## Important Enums
- `OrientationType` — Horizontal | Vertical
- `TargetType` — Rect | Circle | Cross (target marker shapes)
- `FeatureType` — Rect | Dot (actual bar rendering)
- `LabelsPlacement` — placement options for labels
- `TickPosition` — tick placement options
- `LegendPosition` / `LegendLocation` — legend alignment

## Quick Examples

### Minimal bullet chart
````razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@Data" ValueField="Actual" TargetField="Target">
  <BulletChartRangeCollection>
    <BulletChartRange End="50" Color="#d32f2f" />
    <BulletChartRange End="80" Color="#ffa726" />
    <BulletChartRange End="100" Color="#66bb6a" />
  </BulletChartRangeCollection>
</SfBulletChart>
````

### Data labels and tooltip
````razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@Data" ValueField="Actual" TargetField="Target">
  <BulletChartDataLabel Enable="true"></BulletChartDataLabel>
  <BulletChartTooltip Enable="true"></BulletChartTooltip>
</SfBulletChart>
````
