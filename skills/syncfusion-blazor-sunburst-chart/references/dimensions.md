# Dimensions

The size of the `Blazor Sunburst Chart` determines how much space the chart occupies on the page and how the rings of the Sunburst fit within the available area. Size the chart to fit its container, specify a fixed size in pixels, or use percentage values relative to its parent.

Dimensions are configured using the `Width` and `Height` properties of `SfSunburstChart`.

> **Default values:** When `Width` and `Height` are not specified, the chart sizes to its parent container (`100%` width and `100%` height).

Ring geometry inside that area is controlled separately by `Radius` and `InnerRadius` — see [Appearance](appearance.md#radius-and-inner-radius).

## Table of Contents
- [Size for container](#size-for-container)
- [Size in pixels](#size-in-pixels)
- [Size in percentage](#size-in-percentage)
- [See also](#see-also)

## Size for container

Scale the chart to fit its container. Set the size using CSS on a parent element and the chart fills the available space using percentage dimensions.

```cshtml
@using Syncfusion.Blazor.Charts

<div style="width:600px; height:450px; background-color:#F5F7FB;">
    <SfSunburstChart TItem="RegionData"
                     Title="Population by Region"
                     DataSource="@Regions"
                     IdMemberPath="@nameof(RegionData.Id)"
                     ParentIdMemberPath="@nameof(RegionData.ParentId)"
                     LabelMemberPath="@nameof(RegionData.Label)"
                     ValueMemberPath="@nameof(RegionData.Population)"
                     Width="100%"
                     Height="100%">
    </SfSunburstChart>
</div>
```

## Size in pixels

Set `Width` and `Height` in pixels to define a fixed size for the chart.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 IdMemberPath="@nameof(RegionData.Id)"
                 ParentIdMemberPath="@nameof(RegionData.ParentId)"
                 LabelMemberPath="@nameof(RegionData.Label)"
                 ValueMemberPath="@nameof(RegionData.Population)"
                 Width="650px"
                 Height="450px">
</SfSunburstChart>
```

## Size in percentage

By setting `Width` and `Height` to percentage values, the chart dimensions are calculated relative to its parent container. Setting both to `100%` makes the chart fill the available width and height.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 IdMemberPath="@nameof(RegionData.Id)"
                 ParentIdMemberPath="@nameof(RegionData.ParentId)"
                 LabelMemberPath="@nameof(RegionData.Label)"
                 ValueMemberPath="@nameof(RegionData.Population)"
                 Width="100%"
                 Height="100%">
</SfSunburstChart>
```

## See also

- [Appearance](appearance.md) for ring geometry and themes.
