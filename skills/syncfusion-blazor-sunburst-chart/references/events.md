# Events

## Table of Contents
- [Overview](#overview)
- [Action vs EventCallback Semantics](#action-vs-eventcallback-semantics)
- [DrillDownStarting / DrillUpStarting](#drilldownstarting--drillupstarting)
- [DrillDownCompleted / DrillUpCompleted](#drilldowncompleted--drillupcompleted)
- [PointClick](#pointclick)
- [SelectionChanged](#selectionchanged)
- [LegendClick](#legendclick)
- [LegendItemRendering](#legenditemrendering)
- [DataLabelRendering](#datalabelrendering)
- [SegmentRendering](#segmentrendering)
- [TooltipRendering](#tooltiprendering)
- [Loaded](#loaded)
- [PrintCompleted and Exporting Hooks](#printcompleted-and-exporting-hooks)

## Overview

Events let you observe and customize the `Blazor Sunburst Chart` at well-defined points during interaction and rendering — from clicks and legend toggling to data-label and segment painting. Use them when you want to intercept a default behavior, perform custom validation, or surface chart interactions in your own UI.

Events are configured directly on the `SfSunburstChart` component by assigning the relevant callback parameters.

> **Default behavior:** No event callbacks are subscribed by default. The chart renders and behaves normally until at least one handler is attached. Cancelable events (`Cancel = true`) prevent the default action; the rest are observational.

## Action vs EventCallback Semantics

The events fall into two categories based on the Blazor delegate type:

- **`Action<>` (synchronous):** `LegendItemRendering`, `TooltipRendering`, `DataLabelRendering`, `SegmentRendering`, `Exporting`, `ExportCompleted`, and `PrintCompleted`. These run synchronously on the Blazor render thread. Keep handlers short and avoid `async`/`await` inside them, because long-running work delays the next render frame.
- **`EventCallback<>` (asynchronous):** `Loaded`, `LegendClick`, `PointClick`, `SelectionChanged`, `DrillDownStarting`, `DrillUpStarting`, `DrillDownCompleted`, and `DrillUpCompleted`. These follow standard Blazor asynchronous semantics.

## DrillDownStarting / DrillUpStarting

The `DrillDownStarting` event fires when a user double-clicks a segment with one or more children. `DrillUpStarting` fires when a user double-clicks the focused root segment to drill up. Both fire **after** the child check and **before** the visual drill is applied. Both are cancelable.

> The drill trigger gestures (double-click, double-tap within 400 ms, `Enter` on a focused segment, breadcrumb click) and the full `SunburstDrillSettings` configuration are documented in [Drill-Down](drill-down.md).

Event arguments: `SunburstDrillStartingEventArgs<TItem>` (with `EventName = "DrillDownStarting"` or `EventName = "DrillUpStarting"`).

> `DrillDownStarting` and `DrillUpStarting` are `EventCallback<>` delegates. The handler can be `void` or return `Task`. Returning `Task` lets you await async work before the drill proceeds. Set `args.Cancel = true` inside the handler to block the drill.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 IdMemberPath="@nameof(RegionData.Id)"
                 ParentIdMemberPath="@nameof(RegionData.ParentId)"
                 LabelMemberPath="@nameof(RegionData.Label)"
                 ValueMemberPath="@nameof(RegionData.Population)"
                 DrillDownStarting="@OnDrillDown"
                 Width="100%" Height="600px">
    <SunburstDrillSettings Enable="true" />
</SfSunburstChart>

@code {
    private void OnDrillDown(SunburstDrillStartingEventArgs<RegionData> args)
    {
        // args.Point.Label, args.Point.ParentLabel, args.Point.RootLabel, args.Point.Value
        // Set args.Cancel = true to block the drill-down navigation.
    }
}
```

### SunburstDrillStartingEventArgs properties

| Property | Type | Description |
|---|---|---|
| `EventName` | `string` | Returns `"DrillDownStarting"` or `"DrillUpStarting"`. |
| `Cancel` | `bool` | Set to `true` to prevent the navigation. Default is `false`. |
| `Point` | `SunburstDrillPointInfo<TItem>` | Information about the segment the user double-clicked. |

`SunburstDrillPointInfo<TItem>` fields: `Label`, `Value`, `ParentLabel`, `RootLabel`, `PreviousRootLabel`, `Source`.

## DrillDownCompleted / DrillUpCompleted

`DrillDownCompleted` and `DrillUpCompleted` fire after the drill-down or drill-up operation is completed. Both are observational and do not support cancellation. The arguments are the same `SunburstDrillStartingEventArgs<TItem>` shape, with `EventName` set to `"DrillDownCompleted"` or `"DrillUpCompleted"` and `Cancel` ignored.

## PointClick

`PointClick` fires when a user clicks a Sunburst segment. Observational — no cancellation. Use it to read the clicked segment or trigger custom actions.

Event arguments: `SunburstPointClickEventArgs<TItem>`.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 IdMemberPath="@nameof(RegionData.Id)"
                 ParentIdMemberPath="@nameof(RegionData.ParentId)"
                 LabelMemberPath="@nameof(RegionData.Label)"
                 ValueMemberPath="@nameof(RegionData.Population)"
                 PointClick="@PointClickHandler"
                 Width="100%" Height="600px">
</SfSunburstChart>

@code {
    private void PointClickHandler(SunburstPointClickEventArgs<RegionData> args)
    {
        // args.Point.Label, args.Point.Value, args.Fill, args.Source
    }
}
```

### SunburstPointClickEventArgs properties

| Property | Type | Description |
|---|---|---|
| `EventName` | `string` | Returns `"PointClick"`. |
| `Fill` | `string` | Fill color of the clicked segment. |
| `Point` | `SunburstPointInfo` | Clicked segment info — exposes `Label` and `Value`. |
| `Font` | `SunburstFontModel` | Font style associated with the clicked segment. |
| `Source` | `TItem` | Original data item associated with the clicked segment. |

`SunburstFontModel` (returned by the `Font` property above) exposes the following mutable fields:

| Property | Type | Description |
|---|---|---|
| `Color` | `string` | Text color (any valid CSS color). |
| `FontSize` | `string` | Text size (e.g., `"14px"`). |
| `FontFamily` | `string` | Font family (fallback to theme default when unset). |
| `FontWeight` | `string` | Font weight (`"Normal"`, `"Bold"`, `"Bolder"`, `"Lighter"`, or numeric). |
| `FontStyle` | `string` | Font style (`"Normal"`, `"Italic"`, `"Oblique"`). |
| `Opacity` | `double` | Opacity from `0` (transparent) to `1` (opaque). Default `1`. |

## SelectionChanged

`SelectionChanged` fires after the current selection state is committed. Observational — no cancellation. Useful for reading the active selection or updating external UI.

Event arguments: `SunburstSelectionChangedEventArgs<TItem>`.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 SelectionChanged="@SelectionChangedHandler"
                 Width="100%" Height="600px">
    <SunburstSelectionSettings Enable="true" />
</SfSunburstChart>

@code {
    private void SelectionChangedHandler(SunburstSelectionChangedEventArgs<RegionData> args)
    {
        if (args.HasSelection && args.SelectedPoint != null)
        {
            // args.SelectedPoint.Label, args.SelectedPoint.Value
        }
    }
}
```

`SunburstSelectionChangedEventArgs<TItem>` exposes: `EventName`, `SelectedPoint`, `PreviousPoint`, `Source`, `PreviousSource`, `HasSelection`.

> Clicking the currently selected segment again clears the selection, and the callback fires with the cleared state so external UI can synchronize.

## LegendClick

`LegendClick` fires when a user clicks a legend item. Cancelable — set `Cancel = true` to prevent the default visibility toggle.

Event arguments: `SunburstLegendClickEventArgs<TItem>`.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 LegendClick="@LegendClickHandler"
                 Width="100%" Height="600px">
    <SunburstLegendSettings Visible="true" Position="SunburstLegendPosition.Bottom" ToggleVisibility="true" />
</SfSunburstChart>

@code {
    private Task LegendClickHandler(SunburstLegendClickEventArgs<RegionData> args)
    {
        // Example: prevent the Germany branch from being hidden.
        // if (args.Text == "Germany") args.Cancel = true;
        return Task.CompletedTask;
    }
}
```

### SunburstLegendClickEventArgs properties

| Property | Type | Description |
|---|---|---|
| `EventName` | `string` | Returns `"LegendClick"`. |
| `Cancel` | `bool` | `true` prevents the visibility toggle. Default is `false`. |
| `LegendIndex` | `int` | Zero-based index of the clicked legend item. |
| `Text` | `string` | Label text of the clicked legend item. |
| `ShapeColor` | `string` | Marker color of the clicked legend item. |
| `Source` | `TItem` | Original data item for the clicked legend item. |

## LegendItemRendering

`LegendItemRendering` fires before each legend item is rendered. Customize the text, text color, or marker color, or cancel the item entirely (`Cancel = true`).

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 LegendItemRendering="@OnLegendRendering"
                 Width="100%" Height="600px">
    <SunburstLegendSettings Visible="true" Position="SunburstLegendPosition.Bottom" ToggleVisibility="false" />
</SfSunburstChart>

@code {
    private void OnLegendRendering(SunburstLegendItemRenderingEventArgs<RegionData> args)
    {
        if (args.Text == "USA")
        {
            args.Text = "United States";
            args.TextColor = "#1E88E5";
            args.ShapeColor = "#1E88E5";
        }
    }
}
```

`SunburstLegendItemRenderingEventArgs<TItem>` properties: `EventName`, `Cancel`, `LegendIndex`, `Text` (mutable), `TextColor` (mutable), `ShapeColor` (mutable), `Source`.

## DataLabelRendering

`DataLabelRendering` fires before each data label is rendered. Override the text or font, or cancel the label (`Cancel = true`).

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 DataLabelRendering="@OnDataLabel"
                 Width="100%" Height="600px">
    <SunburstDataLabelSettings Visible="true" />
</SfSunburstChart>

@code {
    private void OnDataLabel(SunburstDataLabelRenderingEventArgs<RegionData> args)
    {
        if (args.Text == "USA")
        {
            args.Text = "United States";
            args.Font.Color = "#1E88E5";
            args.Font.FontWeight = "Bold";
        }
    }
}
```

`SunburstDataLabelRenderingEventArgs<TItem>` properties: `EventName`, `Cancel`, `Text` (mutable), `Font` (mutable — `Color`, `FontSize`, `FontFamily`, `FontWeight`, `FontStyle`, `Opacity`), `Source`.

## SegmentRendering

`SegmentRendering` fires before each Sunburst segment is rendered. Override the segment color based on level or root label, or skip individual segments (`Cancel = true`).

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 SegmentRendering="@OnSegmentRender"
                 Width="100%" Height="600px">
</SfSunburstChart>

@code {
    private void OnSegmentRender(SunburstSegmentRenderingEventArgs<RegionData> args)
    {
        if (args.LevelIndex == 0)
        {
            args.Color = "#FFA500";
        }
    }
}
```

`SunburstSegmentRenderingEventArgs<TItem>` properties: `EventName`, `Cancel`, `Color` (mutable), `LevelIndex`, `RootLabel`, `Source`.

## TooltipRendering

`TooltipRendering` fires before each tooltip is rendered. Override the tooltip text and appearance, or cancel the tooltip (`Cancel = true`).

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 TooltipRendering="@OnTooltipRender"
                 Width="100%" Height="600px">
    <SunburstTooltipSettings Enable="true" />
</SfSunburstChart>

@code {
    private void OnTooltipRender(SunburstTooltipRenderingEventArgs<RegionData> args)
    {
        args.HeaderText = "Region";
        args.Text = $"{args.Point.Label}: {args.Point.Value:N0}";
        args.Fill = "#424242";
        args.Opacity = 1;
    }
}
```

### SunburstTooltipRenderingEventArgs properties

| Property | Type | Description |
|---|---|---|
| `EventName` | `string` | Returns `"TooltipRendering"`. |
| `Cancel` | `bool` | `true` prevents the tooltip. Default is `false`. |
| `Text` | `string` | Body text of the tooltip (mutable). |
| `HeaderText` | `string` | Header text of the tooltip (mutable). |
| `Fill` | `string` | Background fill color (mutable). Empty uses the theme default. |
| `Opacity` | `double` | Opacity between `0` and `1`. Default is `0.75`. |
| `Font` | `SunburstFontModel` | Body text font style (mutable). |
| `HeaderFont` | `SunburstFontModel` | Header text font style (mutable). |
| `HeaderLineColor` | `string` | Separator line color when `ShowHeaderLine` is true. |
| `Point` | `SunburstPointInfo` | Data point under the cursor — `Label` and `Value`. |
| `Source` | `TItem` | Original data item associated with the tooltip. |

## Loaded

`Loaded` fires once after the chart has been initialized and rendered. Use it to run post-load configuration. Observational.

Event arguments: `SunburstLoadedEventArgs` — exposes only `EventName` (returns `"Loaded"`).

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 Loaded="@OnLoaded"
                 Width="100%" Height="600px">
</SfSunburstChart>

@code {
    private void OnLoaded(SunburstLoadedEventArgs args)
    {
        // Post-load logic.
    }
}
```

## PrintCompleted and Exporting Hooks

The chart exposes synchronous `Action` callbacks for the print and export workflows initiated by `PrintAsync` and `ExportAsync`. They are not `EventCallback` instances because they are invoked by internal orchestration rather than by a user gesture.

- `PrintCompleted` (`Action`) — Fires after the chart's print workflow has been submitted to the browser.
- `Exporting` (`Action<ChartExportEventArgs>`) — Fires before an export is performed. Set `Cancel = true` to skip the export, or override `Width`, `Height`, or the in-progress `Workbook` for visual/data exports.
- `ExportCompleted` (`Action<ExportEventArgs>`) — Fires after the export completes. Receives the resulting `DataUrl` when `ExportAsync` is called with `AllowDownload = false`.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 Exporting="@OnExporting"
                 ExportCompleted="@OnExportCompleted"
                 @ref="Sunburst"
                 Width="100%" Height="600px">
</SfSunburstChart>

<button @onclick="ExportSunburst">Export PNG</button>

@code {
    private SfSunburstChart<RegionData>? Sunburst;

    private void OnExporting(ChartExportEventArgs args)
    {
        // args.Cancel = true to skip; args.Width / args.Height to override.
    }

    private void OnExportCompleted(ExportEventArgs args)
    {
        // args.DataUrl contains the result when AllowDownload = false.
    }

    private async Task ExportSunburst()
    {
        await Sunburst!.ExportAsync(ExportType.PNG, "SunburstChart");
    }
}
```

## See also

- [Drill-Down](drill-down.md)
- [Selection](selection.md)
- [Print and Export](print-export.md)
