# Legend

## Table of Contents
- [Overview](#overview)
- [Enable the Legend](#enable-the-legend)
- [SunburstLegendSettings Properties](#sunburstlegendsettings-properties)
- [SunburstLegendTextStyle Properties](#sunburstlegendtextstyle-properties)
- [SunburstLegendBorder Properties](#sunburstlegendborder-properties)
- [Full customization example](#full-customization-example)
- [Toggle Segment Visibility](#toggle-segment-visibility)
- [See also](#see-also)

## Overview

The legend provides a visual key for the top-level (root) categories of a `Blazor Sunburst Chart`. It helps users identify what each color in the chart represents, and clicking a legend item can interactively show or hide the related segment group. Legends are recommended whenever the chart contains more than one root category.

The legend is enabled and customized using the `SunburstLegendSettings` child component.

> **Supported positions:** `Position` supports `SunburstLegendPosition.Top`, `Bottom`, `Left`, and `Right`. The default position is `Bottom`.

## Enable the Legend

The legend is hidden by default. Set `Visible` to `true` to render it.

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
    <SunburstLegendSettings Visible="true" Position="SunburstLegendPosition.Bottom" ToggleVisibility="true" />
</SfSunburstChart>
```

## SunburstLegendSettings Properties

- `Visible`: Enables or disables the display of the legend. Set to `true` to render the legend.
- `Position`: Position of the legend — `SunburstLegendPosition.Top`, `Bottom`, `Left`, or `Right`. Default is `Bottom`.
- `Background`: Background color of the legend container. Any valid CSS color value.
- `Opacity`: Transparency of the legend container, from `0` to `1`.
- `ShapeWidth`: Width of the marker drawn beside each legend item, in pixels.
- `ShapeHeight`: Height of the marker drawn beside each legend item, in pixels.
- `ItemPadding`: Spacing between adjacent legend items, in pixels.
- `ToggleVisibility`: When `true`, clicking a legend item toggles the visibility of its corresponding root-level segment group. Default is `true`. Set to `false` for a non-interactive legend.
- `Focusable`: Whether the legend can receive keyboard focus. Default is `true`.
- `AccessibilityDescription`: Additional descriptive text for the legend. Default is `null`.
- `AccessibilityRole`: Semantic accessibility role applied to the legend. Default is `null`.

## SunburstLegendTextStyle Properties

- `FontSize`: Font size in pixels (e.g., `"14px"`). Falls back to the active theme if unset.
- `FontFamily`: Font family. Multiple families can be specified as a comma-separated list.
- `FontWeight`: Font weight — `Normal`, `Bold`, `Bolder`, `Lighter`, or numeric values. Falls back to the active theme if unset.
- `FontStyle`: Font style — `Normal`, `Italic`, or `Oblique`. Falls back to the active theme if unset.
- `Color`: Text color using any valid CSS color value. Falls back to the active theme if unset.
- `Opacity`: Text transparency, from `0` to `1`.

## SunburstLegendBorder Properties

- `Color`: Color of the border drawn around the legend container. Any valid CSS color value.
- `Width`: Width of the border, in pixels. The border is only rendered when `Width` is greater than `0`.

## Full customization example

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 ...>
    <SunburstLegendSettings Visible="true"
                            Position="SunburstLegendPosition.Top"
                            Background="#F5F5F5"
                            Opacity="0.95"
                            ShapeWidth="14"
                            ShapeHeight="14"
                            ItemPadding="12"
                            ToggleVisibility="true">
        <SunburstLegendBorder Color="#BDBDBD" Width="1" />
        <SunburstLegendTextStyle FontSize="13px"
                                 FontFamily="Segoe UI"
                                 FontWeight="Bold"
                                 FontStyle="Normal"
                                 Color="#424242"
                                 Opacity="1" />
    </SunburstLegendSettings>
</SfSunburstChart>
```

## Toggle Segment Visibility

When `ToggleVisibility` is `true`, selecting a legend item hides or restores its corresponding hierarchy branch and descendants. The chart recalculates the visible layout based on the remaining visible branches.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 ...>
    <SunburstLegendSettings Visible="true"
                            Position="SunburstLegendPosition.Right"
                            ToggleVisibility="true" />
</SfSunburstChart>
```

## See also

- [Data Label](data-label.md)
- [Tooltip](tooltip.md)
- [Selection](selection.md)
