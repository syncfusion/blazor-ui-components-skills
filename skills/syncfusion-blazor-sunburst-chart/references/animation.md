# Animation

The animation feature adds an entrance effect to the `Blazor Sunburst Chart`, so the rings and segments are revealed smoothly when the chart first renders. It is most useful when you want the chart to feel responsive and visually engaging on the page rather than appearing suddenly.

Animation is enabled and customized using the `EnableAnimation` and `AnimationType` properties on `SfSunburstChart`.

> **Default behavior:** `EnableAnimation` is `true`, so the chart renders with the default entrance animation. When disabled, the chart renders without animation. When enabled, the animation runs during the initial rendering only.

## Table of Contents
- [Enable animation](#enable-animation)
- [Animation type](#animation-type)
- [See also](#see-also)

## Enable animation

Animation is enabled by default. Set `EnableAnimation` to `false` to render the chart without animation.

```cshtml
@using Syncfusion.Blazor.Charts

<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 EnableAnimation="true"
                 DataSource="@Regions"
                 IdMemberPath="@nameof(RegionData.Id)"
                 ParentIdMemberPath="@nameof(RegionData.ParentId)"
                 LabelMemberPath="@nameof(RegionData.Label)"
                 ValueMemberPath="@nameof(RegionData.Population)"
                 Width="100%" Height="600px">
</SfSunburstChart>
```

> Disabling animation can improve rendering performance when working with large data sets or when immediate chart updates are preferred.

## Animation type

Use `AnimationType` to choose how the entrance animation reveals the chart. The animation style is a `SunburstAnimationType` enum value. `AnimationType` is applied only when `EnableAnimation` is `true`.

- `Rotation` — Displays segments using a rotational animation effect. This is the **default** value.
- `FadeIn` — Gradually displays segments using a fade-in effect.

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 EnableAnimation="true"
                 AnimationType="SunburstAnimationType.FadeIn"
                 DataSource="@Regions"
                 IdMemberPath="@nameof(RegionData.Id)"
                 ParentIdMemberPath="@nameof(RegionData.ParentId)"
                 LabelMemberPath="@nameof(RegionData.Label)"
                 ValueMemberPath="@nameof(RegionData.Population)"
                 Width="100%" Height="600px">
</SfSunburstChart>
```

## See also

- [Appearance](appearance.md) for themes, palette, and ring geometry.
