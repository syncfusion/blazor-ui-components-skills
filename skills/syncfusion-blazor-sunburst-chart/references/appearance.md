# Appearance

## Table of Contents
- [Overview](#overview)
- [Built-in Themes](#built-in-themes)
- [Custom Color Palette](#custom-color-palette)
- [Background Color](#background-color)
- [Chart Border](#chart-border)
- [Chart Margin](#chart-margin)
- [Radius and Inner Radius](#radius-and-inner-radius)
- [Start Angle and End Angle](#start-angle-and-end-angle)
- [Animation](#animation)
- [Title and Subtitle](#title-and-subtitle)
- [Customize Title and Subtitle](#customize-title-and-subtitle)

## Overview

The appearance of the `Blazor Sunburst Chart` controls how segments, rings, and the surrounding chart area are rendered. Configure it through properties on `SfSunburstChart` and the `SunburstChartBorder` and `SunburstChartMargin` child components.

> **Default values:** `Theme` is `Bootstrap5`, `Background` is `null` (uses the theme background), `SunburstChartBorder.Color` is `transparent`, `SunburstChartBorder.Width` is `0`, the default margins are `10` on each side, `Radius` is `1`, `InnerRadius` is `0.2`, `StartAngle` is `0`, and `EndAngle` is `360`.

## Built-in Themes

Set `Theme` on `SfSunburstChart` to one of the Syncfusion themes to match your application's visual style. The theme controls segment colors, fonts, backgrounds, and supporting elements.

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Charts

<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 Theme="Theme.Bootstrap5"
                 DataSource="@Regions"
                 IdMemberPath="@nameof(RegionData.Id)"
                 ParentIdMemberPath="@nameof(RegionData.ParentId)"
                 LabelMemberPath="@nameof(RegionData.Label)"
                 ValueMemberPath="@nameof(RegionData.Population)"
                 Width="100%" Height="600px">
</SfSunburstChart>
```

## Custom Color Palette

Pass an array of color values to `Palette` to define a custom palette. Colors are applied sequentially to the root-level segments. Child segments inherit the color of their root-level segment.

```cshtml
@using Syncfusion.Blazor.Charts

<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 Palette="@CustomPalette"
                 DataSource="@Regions"
                 IdMemberPath="@nameof(RegionData.Id)"
                 ParentIdMemberPath="@nameof(RegionData.ParentId)"
                 LabelMemberPath="@nameof(RegionData.Label)"
                 ValueMemberPath="@nameof(RegionData.Population)"
                 Width="100%" Height="600px">
</SfSunburstChart>

@code {
    public string[] CustomPalette = new string[]
    {
        "#4472C4", "#ED7D31", "#A5A5A5", "#FFC000", "#5B9BD5", "#70AD47"
    };

    // The RegionData model and Regions data source are defined in
    // Working with Data (working-with-data.md). Reuse that model and list,
    // or replace Regions with your own hierarchy data source.
}
```

## Background Color

Use `Background` to set the chart area's background color. Any valid CSS color value is accepted (named, hex, RGB, RGBA). The default is `null`, which uses the theme background. Set `Background="transparent"` to display the parent container's background.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 Background="#DDEBFF"
                 DataSource="@Regions"
                 ...>
</SfSunburstChart>
```

## Chart Border

Add a visible border around the chart area using the `SunburstChartBorder` child component. The border renders only when `Width` is greater than `0`.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 Background="#F5F7FB"
                 DataSource="@Regions"
                 ...>
    <SunburstChartBorder Color="#9E9E9E" Width="2" />
</SfSunburstChart>
```

## Chart Margin

Use `SunburstChartMargin` to control spacing between the chart and its container edges. Margins are specified independently for left, right, top, and bottom, in pixels. The default is `10` on each side.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 Width="100%" Height="600px">
    <SunburstChartBorder Color="#9E9E9E" Width="1" />
    <SunburstChartMargin Left="40" Right="40" Top="40" Bottom="40" />
</SfSunburstChart>
```

## Radius and Inner Radius

The `Radius` and `InnerRadius` properties control the geometry of the Sunburst rings:

- `Radius` — ratio of the outer ring to the available rendering area. Acceptable values are between `0` and `1`. Default is `1`. Smaller values leave more empty space around the chart.
- `InnerRadius` — ratio of the inner ring to the overall `Radius`. Default is `0.2`. A value of `0` renders the chart without a center hole; values greater than `0` create a donut-like appearance.

```cshtml
<SfSunburstChart TItem="NodeDetails"
                 Title="Global Workforce Distribution"
                 DataSource="@Data"
                 Palette="@Palette"
                 IdMemberPath="@nameof(NodeDetails.Id)"
                 ParentIdMemberPath="@nameof(NodeDetails.ParentId)"
                 LabelMemberPath="@nameof(NodeDetails.Name)"
                 ValueMemberPath="@nameof(NodeDetails.Value)"
                 Radius="0.9"
                 InnerRadius="0.35"
                 Width="90%"
                 Height="600px">
</SfSunburstChart>
```

## Start Angle and End Angle

The `StartAngle` and `EndAngle` properties control the angular span of the Sunburst Chart:

- `StartAngle` — angle in degrees at which the first segment begins. Default is `0` (12 o'clock position).
- `EndAngle` — angle in degrees at which rendering ends. Default is `360` (full circle).

Setting `EndAngle` to less than `360` renders a partial circle.

```cshtml
<SfSunburstChart TItem="NodeDetails"
                 Title="Global Workforce Distribution"
                 DataSource="@Data"
                 StartAngle="45"
                 EndAngle="315"
                 Radius="0.9"
                 InnerRadius="0.2"
                 Width="90%"
                 Height="600px">
</SfSunburstChart>
```

## Animation

Use `EnableAnimation` to enable or disable the initial rendering animation. Default is `true`. Use `AnimationType` to select the animation effect. Supported values are `SunburstAnimationType.Rotation` (default) and `SunburstAnimationType.FadeIn`. See [Animation](animation.md) for details.

## Title and Subtitle

The chart exposes a heading through `Title` and an optional secondary line through `Subtitle` on `SfSunburstChart`. Both default to empty, so the chart renders without a heading unless one is supplied.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 Subtitle="Fiscal Year 2026"
                 DataSource="@Regions"
                 ...>
</SfSunburstChart>
```

## Customize Title and Subtitle

Customize the title and subtitle appearance using the `SunburstTitleSettings` and `SunburstSubtitleSettings` child components. These provide font family, size, weight, style, color, opacity, and alignment options that override the active theme defaults.

### `SunburstTitleSettings` properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `FontFamily` | `string` | Theme default | Font family for the title text (e.g., `"Segoe UI"`, `"Arial"`). |
| `Size` | `string` | Theme default | Font size for the title (e.g., `"16px"`, `"1.2em"`). |
| `FontWeight` | `string` | Theme default | Font weight for the title (`"Normal"`, `"Bold"`, `"Bolder"`, `"Lighter"`, or a numeric value like `"600"`). |
| `FontStyle` | `string` | Theme default | Font style for the title (`"Normal"`, `"Italic"`, `"Oblique"`). |
| `Color` | `string` | Theme default | Text color for the title (any valid CSS color — hex, named, RGB, RGBA). |
| `Opacity` | `double` | `1` | Opacity of the title text, from `0` (fully transparent) to `1` (fully opaque). |
| `TextAlignment` | `Alignment` | `Alignment.Center` | Horizontal alignment of the title within the chart area. Accepted values: `Alignment.Near` (left), `Alignment.Center`, `Alignment.Far` (right). |
| `TextOverflow` | `TextOverflow` | `TextOverflow.None` | Controls how the title text behaves when it exceeds the available width. Accepted values: `TextOverflow.None`, `TextOverflow.Wrap`, `TextOverflow.Trim`. |

### `SunburstSubtitleSettings` properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `FontFamily` | `string` | Theme default | Font family for the subtitle text. |
| `Size` | `string` | Theme default | Font size for the subtitle (typically smaller than the title). |
| `FontWeight` | `string` | Theme default | Font weight for the subtitle (`"Normal"`, `"Bold"`, `"Bolder"`, `"Lighter"`, or numeric). |
| `FontStyle` | `string` | Theme default | Font style for the subtitle (`"Normal"`, `"Italic"`, `"Oblique"`). |
| `Color` | `string` | Theme default | Text color for the subtitle. |
| `Opacity` | `double` | `1` | Opacity of the subtitle text, from `0` to `1`. |
| `TextAlignment` | `Alignment` | `Alignment.Center` | Horizontal alignment of the subtitle. Accepted values: `Alignment.Near`, `Alignment.Center`, `Alignment.Far`. |
| `TextOverflow` | `TextOverflow` | `TextOverflow.None` | Overflow behavior for the subtitle text: `TextOverflow.None`, `TextOverflow.Wrap`, `TextOverflow.Trim`. |

### Example

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 Subtitle="Fiscal Year 2026"
                 DataSource="@Regions"
                 IdMemberPath="@nameof(RegionData.Id)"
                 ParentIdMemberPath="@nameof(RegionData.ParentId)"
                 LabelMemberPath="@nameof(RegionData.Label)"
                 ValueMemberPath="@nameof(RegionData.Population)"
                 Width="100%"
                 Height="600px">
    <SunburstTitleSettings FontFamily="Segoe UI"
                           Size="20px"
                           FontWeight="Bold"
                           Color="#1A1A1A"
                           TextAlignment="Alignment.Center">
    </SunburstTitleSettings>
    <SunburstSubtitleSettings FontFamily="Segoe UI"
                              Size="14px"
                              FontWeight="Normal"
                              FontStyle="Italic"
                              Color="#666666"
                              TextAlignment="Alignment.Center">
    </SunburstSubtitleSettings>
</SfSunburstChart>
```
