# Drill-Down

## Table of Contents
- [Overview](#overview)
- [Enable Drill-Down](#enable-drill-down)
- [Show Breadcrumbs](#show-breadcrumbs)
- [SunburstDrillSettings Properties](#sunburstdrillsettings-properties)
- [SunburstBreadcrumbSettings Properties](#sunburstbreadcrumbsettings-properties)
- [Customization Example](#customization-example)

## Overview

The drill feature allows users to explore hierarchical data by focusing on a selected Sunburst segment and displaying its child segments. Users can drill down into a segment that contains child nodes and return to a previous hierarchy level using the focused segment or breadcrumb navigation.

Drill functionality is enabled and customized using the `SunburstDrillSettings` child component.

> **Default behavior:** Drill-down is disabled by default (`Enable` is `false`). `ShowBreadcrumbs` is enabled by default, but breadcrumbs are displayed only when drilling is enabled and the chart is currently drilled into a hierarchy level.

## Enable Drill-Down

Set `Enable` of `SunburstDrillSettings` to `true` to enable drill-down navigation.

Users drill down by **double-clicking** a segment that contains child segments. Double-clicking the currently focused root segment drills up to its parent level. A single click never triggers drill — it only fires `OnMouseClick`. From a touchscreen, a double tap within 400 ms on the same segment drills. From the keyboard, `Enter` on a focused segment drills.

```cshtml
@using Syncfusion.Blazor.Charts

<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 IdMemberPath="@nameof(RegionData.Id)"
                 ParentIdMemberPath="@nameof(RegionData.ParentId)"
                 LabelMemberPath="@nameof(RegionData.Label)"
                 ValueMemberPath="@nameof(RegionData.Population)"
                 Width="100%"
                 Height="600px">
    <SunburstDrillSettings Enable="true" />
</SfSunburstChart>
```

> Drill-down is available only for segments that contain child segments. Double-clicking a leaf segment does not change the current drill level.

## Show Breadcrumbs

Breadcrumbs display the current hierarchy path when the chart is drilled into a segment. Each breadcrumb item represents a level in the current hierarchy and can be selected to navigate back to that level.

Set `ShowBreadcrumbs` to `true` to display breadcrumbs. The default value is `true`.

```cshtml
<SfSunburstChart TItem="RegionData"
                 DataSource="@Regions"
                 IdMemberPath="@nameof(RegionData.Id)"
                 ParentIdMemberPath="@nameof(RegionData.ParentId)"
                 LabelMemberPath="@nameof(RegionData.Label)"
                 ValueMemberPath="@nameof(RegionData.Population)"
                 Width="100%"
                 Height="600px">
    <SunburstDrillSettings Enable="true"
                           ShowBreadcrumbs="true" />
</SfSunburstChart>
```

> Breadcrumbs are displayed only after the chart is drilled into a hierarchy level.

> **Common request:** When the user asks for breadcrumbs at the bottom of the chart (below the rings), set `BreadcrumbVerticalAlignment="BreadcrumbVerticalAlignment.Bottom"`. The default is `Top` — override it explicitly for bottom placement. Pair with `BreadcrumbHorizontalAlignment="BreadcrumbHorizontalAlignment.Center"` for a centered bottom breadcrumb bar.

## SunburstDrillSettings Properties

- `Enable`: Enables or disables drill-down navigation. Default is `false`.
- `ShowBreadcrumbs`: Whether breadcrumb navigation is displayed while drilled into a hierarchy level. Default is `true`.
- `BreadcrumbHorizontalAlignment`: Horizontal position of the breadcrumbs — `Left`, `Center`, or `Right`. Default is `Left`.
- `BreadcrumbVerticalAlignment`: Vertical position of the breadcrumbs — `Top` or `Bottom`. Default is `Top`.

## SunburstBreadcrumbSettings Properties

Customize the appearance and accessibility of breadcrumbs using `SunburstBreadcrumbSettings`:

> **Position vs. appearance:** Breadcrumb *position* (`BreadcrumbHorizontalAlignment` and `BreadcrumbVerticalAlignment`) is set on the parent `SunburstDrillSettings` (see above), not on `SunburstBreadcrumbSettings`. `SunburstBreadcrumbSettings` controls only font, text format, separator, color, and accessibility attributes.

- `AccessibilityDescription`: Accessible description announced for each breadcrumb item. Default is `string.Empty`.
- `AccessibilityRole`: Accessibility role applied to each breadcrumb item. Default is `button`.
- `Focusable`: Whether breadcrumb items can receive keyboard focus. Default is `true`.
- `Size`: Font size of the breadcrumb text. Falls back to the active theme if unset.
- `FontFamily`: Font family of the breadcrumb text. Falls back to the active theme if unset.
- `FontWeight`: Font weight of the breadcrumb text. Falls back to the active theme if unset.
- `FontStyle`: Font style of the breadcrumb text. Falls back to the active theme if unset.
- `Format`: Format used to compose the breadcrumb text. Use `${value}` to insert the breadcrumb label.
- `Separator`: Separator displayed between breadcrumb items. Default is `/`.
- `Color`: Color of the breadcrumb text. Falls back to the active theme if unset.
- `SeparatorColor`: Color of the breadcrumb separator. Falls back to the active theme if unset.
- `SeparatorPadding`: Spacing between a breadcrumb item and its adjacent separator. Default is `5px`.

## Customization Example

Positions the breadcrumbs at the bottom center of the chart and customizes their text and separator appearance.

```cshtml
@using Syncfusion.Blazor.Charts

<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 IdMemberPath="@nameof(RegionData.Id)"
                 ParentIdMemberPath="@nameof(RegionData.ParentId)"
                 LabelMemberPath="@nameof(RegionData.Label)"
                 ValueMemberPath="@nameof(RegionData.Population)"
                 Width="100%"
                 Height="600px">
    <SunburstDrillSettings Enable="true"
                           ShowBreadcrumbs="true"
                           BreadcrumbHorizontalAlignment="BreadcrumbHorizontalAlignment.Center"
                           BreadcrumbVerticalAlignment="BreadcrumbVerticalAlignment.Bottom">
        <SunburstBreadcrumbSettings Size="13px"
                                     FontWeight="600"
                                     Color="#424242"
                                     Format="${value}"
                                     Separator=">"
                                     SeparatorColor="#9e9e9e"
                                     SeparatorPadding="8px"
                                     AccessibilityDescription="Navigate to"
                                     AccessibilityRole="button"
                                     Focusable="true" />
    </SunburstDrillSettings>
</SfSunburstChart>
```

> Use the `DrillDownStarting` and `DrillUpStarting` events to execute custom logic or cancel a drill operation before navigation. Use the `DrillDownCompleted` and `DrillUpCompleted` events to respond after the drill operation is completed. See [Events](events.md).

## See also

- [Events](events.md)
- [Selection](selection.md)
- [Highlight](highlight.md)
- [Tooltip](tooltip.md)
- [Accessibility](accessibility.md)
