# Selection

## Table of Contents
- [Overview](#overview)
- [Enable Selection](#enable-selection)
- [Selection Mode](#selection-mode)
- [SunburstSelectionSettings Properties](#sunburstselectionsettings-properties)

## Overview

The selection feature provides clear visual emphasis on a Sunburst segment and its related hierarchy branch when a user selects it. The selected state stays visible after the pointer leaves the segment, and is cleared only when the selection changes or the user drills down/up a level.

Selection is enabled and customized using the `SunburstSelectionSettings` child component.

> **Default behavior:** `Enable` is `false`, so selection is disabled until explicitly turned on. When `Enable` is `true`, non-selected segments are dimmed while the selected segment stays full-color.

## Enable Selection

Set `Enable` of `SunburstSelectionSettings` to `true` to let users select any Sunburst segment by clicking on it.

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
    <SunburstSelectionSettings Enable="true" />
</SfSunburstChart>
```

## Selection Mode

Use `Mode` to decide which related segments are selected together with the segment clicked by the user. The mode is a `SunburstSelectionMode` enum value.

- `Single` — Selects only the clicked segment. This is the **default**.
- `Parent` — Selects the clicked segment along with its parent segment.
- `Child` — Selects the clicked segment along with its child segment.
- `All` — Selects the clicked segment along with its parent and child segments (the entire branch).

> The default selection mode (`Single`) only selects the clicked segment, which differs from the default highlight mode (`All`). If you want selection to affect the same hierarchy branch as a hover-highlight, set `Mode="SunburstSelectionMode.All"` explicitly.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 ...>
    <SunburstSelectionSettings Enable="true"
                               Mode="SunburstSelectionMode.Parent" />
</SfSunburstChart>
```

## SunburstSelectionSettings Properties

- `Enable`: Enables or disables segment selection. Default is `false`.
- `Mode`: Which segments are affected by the selection — `SunburstSelectionMode.All`, `Parent`, `Child`, or `Single`. Default is `Single`.
- `Color`: Color applied to the selected segment. Any valid CSS color value. Default is empty; when empty, the selected segment uses its parent segment color (or its own color for the root segment).
- `Opacity`: Transparency of the selected segment, from `0` to `1`. Default is `1`.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 ...>
    <SunburstSelectionSettings Enable="true"
                               Mode="SunburstSelectionMode.All"
                               Color="#FFA500"
                               Opacity="0.4" />
</SfSunburstChart>
```

> Clicking the currently selected segment again clears the selection. Drilling into a branch, or back to the parent, also clears the selected and highlighted segments so the user can focus on the new hierarchy level.

## See also

- [Data Label](data-label.md)
- [Tooltip](tooltip.md)
- [Highlight](highlight.md)
