# Data Label

## Table of Contents
- [Overview](#overview)
- [Enable the Data Label](#enable-the-data-label)
- [Label Overflow Mode](#label-overflow-mode)
- [Label Rotation Mode](#label-rotation-mode)
- [Customization](#customization)

## Overview

Data labels display textual information about each segment directly on the chart, helping users identify categories and values at a glance without relying solely on tooltips or legends. They are most useful on charts with medium-to-large segments where the label can be clearly read.

Data labels are enabled and customized using the `SunburstDataLabelSettings` child component.

> **Default behavior:** `Visible` is `false`, so no data labels are rendered until explicitly enabled.

## Enable the Data Label

Set `Visible` of `SunburstDataLabelSettings` to `true` so each segment's label is rendered directly on the chart.

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
    <SunburstDataLabelSettings Visible="true" />
</SfSunburstChart>
```

## Label Overflow Mode

When a segment is too small, its label may not fit within the available arc space. Use `OverflowMode` to decide how the chart handles that label:

- `Trim` — shorten the label with an ellipsis so it fits in the arc (**default**). For example, `Los Angeles ...` instead of the full name.
- `Hide` — hide the label entirely when it does not fit. Use for a clean look.
- `None` — render the label as-is, even if it overlaps the segment edge. Use when full text matters more than a tidy appearance.

> `Trim` and `Hide` keep labels visible only when their text fits in the arc. `None` always shows the full label, which can overflow on small segments.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 ...>
    <SunburstDataLabelSettings Visible="true" OverflowMode="SunburstLabelOverflowMode.Hide" />
</SfSunburstChart>
```

## Label Rotation Mode

Use `RotationMode` to choose how each label aligns with the chart:

- `Angle` — rotate the label to follow the angle of its arc. Use when the chart is the focus and you want labels to feel symmetric with the radial layout.
- `Normal` — keep the label upright and horizontal, ignoring the arc angle. Use when users need to read labels quickly (dashboards, reports). This is the **default**.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 ...>
    <SunburstDataLabelSettings Visible="true" RotationMode="SunburstLabelRotationMode.Angle" />
</SfSunburstChart>
```

## Customization

Customize the data label text using the `SunburstDataLabelTextStyle` child component inside `SunburstDataLabelSettings`:

- `FontSize`: Label text size in pixels (e.g., `"14px"`). Falls back to the active theme if unset.
- `FontFamily`: Label text font family. Multiple families can be specified as a comma-separated list.
- `FontWeight`: Font weight — `Normal`, `Bold`, `Bolder`, `Lighter`, or numeric values like `400`, `500`, `700`. Falls back to the active theme if unset.
- `FontStyle`: Font style — `Normal`, `Italic`, or `Oblique`. Falls back to the active theme if unset.
- `Color`: Text color using any valid CSS color value. Falls back to the active theme if unset.
- `Opacity`: Text transparency, from `0` (fully transparent) to `1` (fully opaque).

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 ...>
    <SunburstDataLabelSettings Visible="true">
        <SunburstDataLabelTextStyle FontSize="14px"
                                    FontFamily="Segoe UI"
                                    FontWeight="Bold"
                                    FontStyle="Normal"
                                    Color="#424242"
                                    Opacity="1" />
    </SunburstDataLabelSettings>
</SfSunburstChart>
```

## See also

- [Tooltip](tooltip.md)
- [Legend](legend.md)
- [Selection](selection.md)
