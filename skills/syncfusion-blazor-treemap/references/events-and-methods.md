# Events and Methods

## Table of Contents
- [Overview](#overview)
- [Common Events](#common-events)
- [Render Events](#render-events)
- [Methods: Print & Export](#methods-print--export)
- [Utility Methods](#utility-methods)

## Overview
Lists the `TreeMapEvents` callbacks and the main `SfTreeMap<TValue>` methods.

## Common Events
- `OnClick`, `OnDoubleClick`, `OnRightClick`, `OnItemClick`, `OnItemMove`, `ItemSelected`, and `ItemHighlighted` are exposed through `TreeMapEvents`.

## Render Events
- `Load`, `Loaded`, `ItemRendering`, `LegendRendering`, `LegendItemRendering`, `TooltipRendering`, and `Resizing` are available for render-time customization.

## Methods: Print & Export
- Use `PrintAsync()`, `ExportAsync()`, and the export-related `AllowPrint`, `AllowPdfExport`, and `AllowImageExport` properties.

## Utility Methods
- Use `RefreshAsync()` and `SelectItemAsync()` for programmatic updates and selection.
