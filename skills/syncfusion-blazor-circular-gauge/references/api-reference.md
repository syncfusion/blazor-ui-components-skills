# CircularGauge API Reference (summary)

This file summarizes the primary API surface for `SfCircularGauge` and related child tags. Use this as a quick checklist when validating reference docs.

## Table of Contents
- [Namespace / Import](#namespace--import)
- [Component: SfCircularGauge](#component-sfcirculargauge)
    - [Core Properties](#core-properties)
    - [Important Methods](#important-methods)
    - [Events (common)](#events-common)
- [Child Tags and Key Properties](#child-tags-and-key-properties)
- [Styling CSS classes](#styling-css-classes)
- [Typical Usage Snippet](#typical-usage-snippet)
- [Notes for reference authors](#notes-for-reference-authors)

## Namespace / Import

- `@using Syncfusion.Blazor.CircularGauge`

## Component: `SfCircularGauge`

### Core Properties
- `AllowImageExport` (bool)
- `AllowMargin` (bool)
- `AllowPdfExport` (bool)
- `AllowPrint` (bool)
- `AnimationDuration` (double)
- `Background` (string)
- `CenterX` (string)
- `CenterY` (string)
- `Description` (string)
- `EnableGroupingSeparator` (bool)
- `EnablePointerDrag` (bool)
- `EnableRangeDrag` (bool)
- `Height` (string)
- `ID` (string)
- `MoveToCenter` (bool)
- `TabIndex` (int)
- `Theme` (enum)
- `Title` (string)
- `Width` (string)

### Important Methods
- `Task<string> ExportAsync(ExportType type, string fileName, PdfPageOrientation? orientation = null, bool allowDownload = true)`
- `Task PrintAsync()`
- `Task RefreshAsync()`
- `Task SetAnnotationValueAsync(int axisIndex, int annotationIndex, string content)`
- `Task SetPointerValueAsync(int axisIndex, int pointerIndex, double pointerValue)`
- `void SetRangeValue(int axisIndex, int rangeIndex, double start, double end)`
- `void UpdateChildProperties(string key, object keyValue)`

### Events (common)
- `AnimationCompleted` (AnimationCompleteEventArgs)
- `AnnotationRendering` (AnnotationRenderEventArgs)
- `AxisLabelRendering` (AxisLabelRenderEventArgs)
- `Loaded` / `OnLoad` (LoadedEventArgs)
- `OnGaugeMouseDown` / `OnGaugeMouseMove` / `OnGaugeMouseUp` / `OnGaugeMouseLeave` (MouseEventArgs)
- `OnDrag`, `OnDragStart`, `OnDragEnd` (PointerDragEventArgs)
- `OnPrint` (PrintEventArgs)
- `OnRadiusCalculate` (RadiusCalculateEventArgs)
- `Resizing` (ResizeEventArgs)
- `TooltipRendering` (TooltipRenderEventArgs)

## Child Tags and Key Properties
- `CircularGaugeAnnotation` — `Content`, `Angle`, `Radius`, `AutoAngle`
- `CircularGaugeAxes` — collection of `CircularGaugeAxis`
- `CircularGaugeAxis` — `Background`, `Direction`, `EndAngle`, `StartAngle`, `Maximum`, `Minimum`, `Radius`
- `CircularGaugeBorder` — `Color`, `Width`
- `CircularGaugeLegendSettings` — `Visible`, `Position`, `Shape`, `ToggleVisibility`
- `CircularGaugeMargin` — `Top`, `Bottom`, `Left`, `Right`
- `CircularGaugePointer` — `Type`, `Value`, `Color`, `MarkerShape`, `Position`, `PointerWidth`, `Radius`
- `CircularGaugeRange` — `Start`, `End`, `Color`, `StartWidth`, `EndWidth`, `Radius`
- `CircularGaugeTooltipSettings` — `Enable`, `Fill`, `Format`, `ShowAtMousePosition`

## Styling CSS classes
- `e-circulargauge`
- `e-gauge-axis`
- `e-gauge-pointer`
- `e-gauge-range`
- `e-gauge-annotation`

## Typical Usage Snippet

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge Title="Basic Circular Gauge" Height="400px" Width="400px">
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100" StartAngle="200" EndAngle="160">
            <CircularGaugePointers>
                <CircularGaugePointer Value="65" Color="#007DD1" PointerWidth="8">
                </CircularGaugePointer>
            </CircularGaugePointers>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="50" Color="#3A5998" StartWidth="10" EndWidth="10" />
                <CircularGaugeRange Start="50" End="100" Color="#33BCDA" StartWidth="10" EndWidth="10" />
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Notes for reference authors
- Ensure each feature reference (pointers, ranges, annotations, axes, legend, appearance, user-interaction) includes related API property names and relevant events/methods.
- Link examples to this `api-reference.md` for consistent API terminology.
- Verify method signatures (async return types) against the official Syncfusion docs when adding code samples.