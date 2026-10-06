# Tooltip

## Table of Contents
- [Overview](#overview)
- [Enable the Tooltip](#enable-the-tooltip)
- [SunburstTooltipSettings Properties](#sunbursttooltipsettings-properties)
- [SunburstTooltipTextStyle Properties](#sunbursttooltiptextstyle-properties)
- [SunburstTooltipBorder Properties](#sunbursttooltipborder-properties)
- [Customization Example](#customization-example)

## Overview

The tooltip displays additional information about a segment when users hover over or tap on it. It is most useful when the chart contains many small segments where data labels cannot fit, or when users need to inspect specific values such as the category name and the segment value.

The tooltip is enabled and customized using the `SunburstTooltipSettings` child component.

> By default, the tooltip shows the segment label and its value in the format `${point.label} : ${point.value}`.

## Enable the Tooltip

Set `Enable` of `SunburstTooltipSettings` to `true` to display a tooltip when users interact with a Sunburst segment.

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
    <SunburstTooltipSettings Enable="true" />
</SfSunburstChart>
```

## SunburstTooltipSettings Properties

- `Enable`: Enables or disables the tooltip. Default is `false`.
- `Format`: Defines the tooltip text using placeholders such as `${point.label}` and `${point.value}`. Default is `null`. When unset, the tooltip uses `${point.label} : ${point.value}`. The supported placeholders are `${point.label}` (the segment's `LabelMemberPath` value) and `${point.value}` (the segment's `ValueMemberPath` value). These map to the `Label` and `Value` fields of the `SunburstPointInfo` exposed in tooltip event arguments.
- `HeaderText`: Custom header that appears above the tooltip content. When set, the header is shown for every tooltip. Default is `null`.
- `EnableHighlight`: When `true`, the segment associated with the active tooltip is visually emphasized. Default is `false`. See [Highlight](highlight.md#tooltip-highlighting).
- `ShowHeaderLine`: When `true`, a separator line is rendered between the tooltip header and content. Default is `true`.
- `Opacity`: Tooltip transparency, from `0` (fully transparent) to `1` (fully opaque). Default is `1`.
- `Fill`: Tooltip background color using any valid CSS color value. Falls back to the active theme if unset.

## SunburstTooltipTextStyle Properties

- `FontSize`: Tooltip text size in pixels (e.g., `"14px"`). Falls back to the active theme if unset.
- `FontFamily`: Tooltip text font family. Multiple families can be specified as a comma-separated list.
- `FontWeight`: Font weight — `Normal`, `Bold`, `Bolder`, `Lighter`, or numeric values like `400`, `500`, `700`. Falls back to the active theme if unset.
- `FontStyle`: Font style — `Normal`, `Italic`, or `Oblique`. Falls back to the active theme if unset.
- `Color`: Text color using any valid CSS color value. Falls back to the active theme if unset.

## SunburstTooltipBorder Properties

- `Color`: Color of the border drawn around the tooltip. Any valid CSS color value. The border is only rendered when `Width` is greater than `0`. Default is `transparent`.
- `Width`: Border width in pixels. Set to `0` to hide the tooltip border. Default is `0`.

## Customization Example

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 IdMemberPath="@nameof(RegionData.Id)"
                 ParentIdMemberPath="@nameof(RegionData.ParentId)"
                 LabelMemberPath="@nameof(RegionData.Label)"
                 ValueMemberPath="@nameof(RegionData.Population)"
                 Width="100%" Height="600px">
    <SunburstTooltipSettings Enable="true"
                             Format="${point.label} : ${point.value}"
                             HeaderText="Region Detail"
                             EnableHighlight="true"
                             ShowHeaderLine="true"
                             Opacity="1"
                             Fill="#FFFFFF">
        <SunburstTooltipTextStyle FontSize="14px"
                                  FontFamily="Segoe UI"
                                  FontWeight="Bold"
                                  FontStyle="Normal"
                                  Color="#424242" />
        <SunburstTooltipBorder Color="#BDBDBD" Width="1" />
    </SunburstTooltipSettings>
</SfSunburstChart>
```

> The default `${point.label} : ${point.value}` format matches the tooltip content shown in the documentation images. Use the `Format` property to change the order, include prefixes or units, or add additional placeholders.

## See also

- [Data Label](data-label.md)
- [Legend](legend.md)
- [Selection](selection.md)
