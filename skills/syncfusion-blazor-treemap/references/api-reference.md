# API Reference

## Table of Contents
- [Overview](#overview)
- [Component Model](#component-model)
- [Core Properties](#core-properties)
- [Child Components](#child-components)
- [Events](#events)
- [Methods](#methods)
- [Enums and Supporting Types](#enums-and-supporting-types)
- [Related Guides](#related-guides)

## Overview
This page summarizes the main Syncfusion Blazor TreeMap API surface used throughout the skill. It is intended as a quick reference for the component, its child configuration objects, supported events, public methods, and the enums that appear across the topic guides.

## Component Model
- Namespace: `Syncfusion.Blazor.TreeMap`
- Main component: `SfTreeMap<TValue>`
- Interface: `ITreeMap`
- Primary purpose: render hierarchical, area-proportional data as nested rectangles.

## Core Properties
These are the most commonly used `SfTreeMap<TValue>` members across the skill:

- `DataSource` - source collection for TreeMap items.
- `TValue` - the data model type bound to the component.
- `WeightValuePath` - numeric value used to determine rectangle size.
- `ColorValuePath` - value path used for color-based rendering.
- `RangeColorValuePath` - value path used with range color mapping.
- `EqualColorValuePath` - value path used with equal color mapping.
- `Palette` - explicit color palette.
- `LayoutType` - layout algorithm such as `LayoutMode.Squarified`.
- `RenderDirection` - rectangle rendering direction.
- `Height` and `Width` - component dimensions.
- `EnableDrillDown` - enables hierarchical drill-down.
- `EnableRtl` - enables right-to-left rendering.

Other commonly used configuration containers include `TreeMapLegendSettings`, `TreeMapTooltipSettings`, `TreeMapSelectionSettings`, `TreeMapHighlightSettings`, `TreeMapTitleSettings`, `TreeMapSubtitleSettings`, and `TreeMapMargin`.

## Child Components
TreeMap configuration is composed through nested child settings. The most important ones are:

- `TreeMapLevels` - container for one or more `TreeMapLevel` items.
- `TreeMapLevel` - defines a hierarchy level and its header styling.
- `TreeMapLevelBorder` - level border settings.
- `TreeMapLeafItemSettings` - leaf-node styling and label configuration.
- `TreeMapLeafLabelStyle` - leaf label text styling.
- `TreeMapLeafBorder` - leaf border styling.
- `TreeMapLeafColorMapping` - single leaf color mapping entry.
- `TreeMapLeafColorMappings` - collection of leaf color mappings.
- `TreeMapLegendSettings` - legend configuration.
- `TreeMapLegendBorder` - legend border styling.
- `TreeMapLegendLocation` - legend placement and offset configuration.
- `TreeMapLegendTitle` - legend title configuration.
- `TreeMapLegendTextStyle` - legend text styling.
- `TreeMapLegendTitleStyle` - legend title text styling.
- `TreeMapLegendShapeBorder` - border styling for legend shapes.
- `TreeMapTooltipSettings` - tooltip configuration.
- `TreeMapTooltipBorder` - tooltip border styling.
- `TreeMapTooltipTextStyle` - tooltip text styling.
- `TreeMapSelectionSettings` - selection behavior and appearance.
- `TreeMapHighlightSettings` - highlight behavior and appearance.
- `TreeMapTitleSettings` - title configuration.
- `TreeMapSubtitleSettings` - subtitle configuration.
- `TreeMapBorderSettings` - component border settings.
- `TreeMapMargin` - spacing around the TreeMap.
- `TreeMapInitialDrillSettings` - initial drill-down state.
- `TreeMapEvents` - event callbacks.

## Events
`TreeMapEvents` exposes the callbacks used across the topic guides:

- `Load`
- `Loaded`
- `ItemRendering`
- `ItemSelected`
- `LegendRendering`
- `LegendItemRendering`
- `OnClick`
- `OnDoubleClick`
- `OnDrillStart`
- `OnItemClick`
- `OnItemMove`
- `OnPrint`
- `OnRightClick`
- `Resizing`
- `TooltipRendering`
- `DrillCompleted`
- `ItemHighlighted`

Common event argument types include:

- `LoadEventArgs`
- `LoadedEventArgs`
- `ItemRenderingEventArgs`
- `ItemClickEventArgs`
- `ItemSelectedEventArgs`
- `ItemMoveEventArgs`
- `ItemHighlightEventArgs`
- `LegendRenderingEventArgs`
- `LegendItemRenderingEventArgs`
- `TreeMapTooltipArgs`
- `PrintEventArgs`
- `RightClickEventArgs`
- `ClickEventArgs`
- `DoubleClickEventArgs`
- `DrillStartEventArgs`
- `DrillEndEventArgs`
- `ResizeEventArgs`

## Methods
Common `SfTreeMap<TValue>` methods used in the skill:

- `ExportAsync()` - exports the TreeMap output.
- `PrintAsync()` - prints the TreeMap.
- `RefreshAsync()` - refreshes the component.
- `SelectItemAsync()` - selects a TreeMap item programmatically.

## Enums and Supporting Types
The following enums and supporting types appear throughout the references:

### Layout and Rendering
- `LayoutMode`
- `RenderingMode`
- `RenderDirection`

### Legends
- `LegendMode`
- `LegendPosition`
- `LegendOrientation`
- `LegendShape`

### Labels
- `LabelPosition`
- `LabelPlacement`
- `LabelIntersectAction`

### Interaction
- `SelectionMode`
- `HighLightMode`

### General UI
- `Alignment`
- `ExportType`

## Related Guides
- [Getting Started](getting-started.md)
- [Data Binding](data-binding.md)
- [Layout and Levels](layout-and-levels.md)
- [Leaf Items](leaf-items.md)
- [Color Mapping](color-mapping.md)
- [Labels](labels.md)
- [Legend](legend.md)
- [Tooltips](tooltip.md)
- [Drill-Down](drill-down.md)
- [Selection and Highlight](selection-and-highlight.md)
- [Events and Methods](events-and-methods.md)
- [Advanced Features](advanced-features.md)
