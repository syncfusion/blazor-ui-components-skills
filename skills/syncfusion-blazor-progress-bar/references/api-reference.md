# ProgressBar API Reference

## Table of Contents
- [Overview](#overview)
- [Core Component](#core-component)
	- [SfProgressBar](#sfprogressbar)
- [Child Components](#child-components)
	- [ProgressBarAnimation](#progressbaranimation)
	- [ProgressBarEvents](#progressbarevents)
	- [ProgressBarAnnotations](#progressbarannotations)
	- [ProgressBarAnnotation](#progressbarannotation)
	- [ProgressBarLabelStyle](#progressbarlabelstyle)
	- [ProgressBarMargin](#progressbarmargin)
	- [ProgressBarRangeColor](#progressbarrangecolor)
	- [ProgressBarRangeColors](#progressbarrangecolors)
- [Enums](#enums)
	- [ProgressType](#progresstype)
	- [CornerType](#cornertype)
	- [ModeType](#modetype)
	- [TextAlignmentType](#textalignmenttype)
- [Notes](#notes)

## Overview

This reference summarizes the official `Syncfusion.Blazor.ProgressBar` API surface for `SfProgressBar` and its related child components, events, and enums.

## Core Component

### `SfProgressBar`

Main rendering component for the ProgressBar control.

#### Properties

| Property | Type | Description |
| --- | --- | --- |
| `ChildContent` | `RenderFragment` | Defines custom content inside the component. |
| `CornerRadius` | `CornerType` | Sets the corner style of the progress bar. |
| `EnablePieProgress` | `bool` | Renders circular progress as a pie visualization. |
| `EnableProgressSegments` | `bool` | Enables segmented progress rendering. |
| `EnableRtl` | `bool` | Renders the component from right to left. |
| `EndAngle` | `int` | Sets the end angle for circular progress. |
| `GapWidth` | `double` | Sets the gap width between segments. |
| `Height` | `string` | Sets the component height. |
| `ID` | `string` | Sets the component id. |
| `InnerRadius` | `string` | Sets the inner radius for circular progress. |
| `IsActive` | `bool` | Enables the active state animation style. |
| `IsGradient` | `bool` | Enables gradient rendering. |
| `IsIndeterminate` | `bool` | Enables indeterminate progress mode. |
| `IsStriped` | `bool` | Enables striped rendering for linear progress. |
| `Maximum` | `double` | Sets the maximum progress value. |
| `Minimum` | `double` | Sets the minimum progress value. |
| `ProgressColor` | `string` | Sets the primary progress color. |
| `ProgressThickness` | `double` | Sets the progress thickness. |
| `Radius` | `string` | Sets the circular radius. |
| `Role` | `ModeType` | Sets the progress indication mode. |
| `SecondaryProgress` | `double` | Sets the secondary progress value. |
| `SecondaryProgressColor` | `string` | Sets the secondary progress color. |
| `SecondaryProgressThickness` | `double` | Sets the secondary progress thickness. |
| `SegmentColor` | `string[]` | Sets segment colors. |
| `SegmentCount` | `int` | Sets the number of segments. |
| `ShowProgressValue` | `bool` | Shows the progress label value. |
| `StartAngle` | `int` | Sets the start angle for circular progress. |
| `Theme` | `Theme` | Sets the component theme. |
| `TrackColor` | `string` | Sets the track color. |
| `TrackThickness` | `double` | Sets the track thickness. |
| `Type` | `ProgressType` | Sets linear or circular rendering. |
| `Value` | `double` | Sets the current progress value. |
| `Visible` | `bool` | Shows or hides the component. |
| `Width` | `string` | Sets the component width. |

#### Methods

| Method | Return Type | Description |
| --- | --- | --- |
| `RefreshAsync()` | `Task` | Re-renders the ProgressBar component. |

## Child Components

### `ProgressBarAnimation`

Controls progress animation behavior.

| Property | Type | Description |
| --- | --- | --- |
| `Enable` | `bool` | Enables animation. |
| `Duration` | `int` | Sets the animation duration in milliseconds. |
| `Delay` | `int` | Sets the animation delay in milliseconds. |

### `ProgressBarEvents`

Defines event callbacks for the ProgressBar.

| Event | Type | Description |
| --- | --- | --- |
| `AnimationComplete` | `EventCallback<ProgressValueEventArgs>` | Fires after animation completes. |
| `AnnotationRender` | `EventCallback<AnnotationRenderEventArgs>` | Fires before annotations render. |
| `Loaded` | `EventCallback<EventArgs>` | Fires after the component loads. |
| `ProgressCompleted` | `EventCallback<ProgressValueEventArgs>` | Fires when progress reaches maximum. |
| `TextRender` | `EventCallback<TextRenderEventArgs>` | Fires before the label text renders. |
| `ValueChanged` | `EventCallback<ProgressValueEventArgs>` | Fires when the progress value changes. |

### `ProgressBarAnnotations`

Container for one or more `ProgressBarAnnotation` items.

### `ProgressBarAnnotation`

Defines an annotation overlay for the progress bar.

| Property | Type | Description |
| --- | --- | --- |
| `ContentTemplate` | `RenderFragment` | Custom content for the annotation. |
| `AnnotationAngle` | `int` | Sets the annotation angle. |
| `AnnotationRadius` | `string` | Sets the annotation radius. |

### `ProgressBarLabelStyle`

Controls label styling.

| Property | Type | Description |
| --- | --- | --- |
| `Color` | `string` | Sets label color. |
| `FontFamily` | `string` | Sets label font family. |
| `FontStyle` | `string` | Sets label font style. |
| `FontWeight` | `string` | Sets label font weight. |
| `Size` | `string` | Sets label font size. |
| `Text` | `string` | Sets custom label text. |
| `TextAlignment` | `TextAlignmentType` | Sets label alignment. |
| `Opacity` | `double` | Sets label opacity. |

### `ProgressBarMargin`

Controls outer spacing.

| Property | Type | Description |
| --- | --- | --- |
| `Top` | `double` | Sets the top margin. |
| `Bottom` | `double` | Sets the bottom margin. |
| `Left` | `double` | Sets the left margin. |
| `Right` | `double` | Sets the right margin. |

### `ProgressBarRangeColor`

Defines a single range color.

| Property | Type | Description |
| --- | --- | --- |
| `Color` | `string` | Sets the range color. |
| `Start` | `double` | Sets the range start value. |
| `End` | `double` | Sets the range end value. |

### `ProgressBarRangeColors`

Container for multiple `ProgressBarRangeColor` items.

## Enums

### `ProgressType`

| Value | Description |
| --- | --- |
| `Linear` | Renders a horizontal progress bar. |
| `Circular` | Renders a circular progress bar. |

### `CornerType`

| Value | Description |
| --- | --- |
| `Auto` | Uses automatic corner selection. |
| `Square` | Uses square corners. |
| `Round` | Uses fully rounded corners. |
| `Round4px` | Uses 4px rounded corners. |

### `ModeType`

| Value | Description |
| --- | --- |
| `Auto` | Uses automatic mode selection. |
| `Success` | Indicates success mode. |
| `Info` | Indicates informational mode. |
| `Danger` | Indicates danger mode. |
| `Warning` | Indicates warning mode. |

### `TextAlignmentType`

| Value | Description |
| --- | --- |
| `Near` | Aligns text near the start. |
| `Center` | Centers the text. |
| `Far` | Aligns text toward the end. |

## Notes

- `IsStriped` applies to linear progress bars.
- `Radius`, `InnerRadius`, `StartAngle`, and `EndAngle` apply to circular progress bars.
- `RefreshAsync()` is the public method available on `SfProgressBar` for re-rendering.
