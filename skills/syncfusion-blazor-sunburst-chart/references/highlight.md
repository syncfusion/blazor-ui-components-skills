# Highlight

## Table of Contents
- [Overview](#overview)
- [Enable Highlighting](#enable-highlighting)
- [Supported Highlight Modes](#supported-highlight-modes)
- [SunburstHighlightSettings Properties](#sunbursthighlightsettings-properties)
- [Tooltip Highlighting](#tooltip-highlighting)

## Overview

Highlighting visually emphasizes the segment under the pointer so users can immediately tell which part of the hierarchy they are inspecting. It is most useful when the chart contains many small segments or several nested levels.

Highlighting is driven by the `SunburstHighlightSettings` child component.

> **Default behavior:** `Enable` is `false`, so highlighting is disabled until explicitly turned on. When `Enable` is `true`, non-highlighted segments are dimmed while the active segment stays full-color.

## Enable Highlighting

Set `Enable` of `SunburstHighlightSettings` to `true` to highlight the segment under the pointer.

```cshtml
@using Syncfusion.Blazor.Charts

<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 IdMemberPath="@nameof(RegionData.Id)"
                 ParentIdMemberPath="@nameof(RegionData.ParentId)"
                 LabelMemberPath="@nameof(RegionData.Label)"
                 ValueMemberPath="@nameof(RegionData.Population)"
                 Width="100%" Height="600px">
    <SunburstHighlightSettings Enable="true" />
</SfSunburstChart>
```

## Supported Highlight Modes

Use `Mode` to decide which related segments are highlighted together with the segment under the pointer. The mode is a `SunburstHighlightMode` enum value.

- `Single` — Highlights only the segment under the pointer. Use for a strict one-segment focus.
- `Parent` — Highlights the segment and its parent segments. Use to keep the hierarchy path visible.
- `Child` — Highlights the segment and its child segments. Use to focus on a segment and its descendants.
- `All` — Highlights the segment together with its parent and child segments (the complete branch). This is the **default**.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 ...>
    <SunburstHighlightSettings Enable="true" Mode="SunburstHighlightMode.Parent" />
</SfSunburstChart>
```

## SunburstHighlightSettings Properties

- `Enable`: Enables or disables highlighting on pointer hover. Default is `false`.
- `Mode`: Which segments are highlighted together with the segment under the pointer — `SunburstHighlightMode.Single`, `Parent`, `Child`, or `All`. Default is `All`.
- `Color`: Color applied to highlighted segments. Any valid CSS color value. Default is empty; when empty, the highlighted segment uses its parent segment color (or its own color for the root segment).
- `Opacity`: Transparency of highlighted segments, from `0` to `1`. Default is `1`.

> When `Enable` is `true`, the non-highlighted segments are always dimmed. That dim is a fixed visual effect not controlled by `Color` or `Opacity`.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 ...>
    <SunburstHighlightSettings Enable="true"
                               Mode="SunburstHighlightMode.All"
                               Color="#FFD200"
                               Opacity="0.8" />
</SfSunburstChart>
```

## Tooltip Highlighting

The chart highlights the segment associated with the active tooltip when `EnableHighlight` of `SunburstTooltipSettings` is set to `true`. Default is `false`. `EnableHighlight` is independent of `SunburstHighlightSettings.Enable`, and both can be active at the same time.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 ...>
    <SunburstTooltipSettings Enable="true" EnableHighlight="true" />
</SfSunburstChart>
```

## See also

- [Tooltip](tooltip.md)
- [Selection](selection.md)
- [Data Label](data-label.md)
