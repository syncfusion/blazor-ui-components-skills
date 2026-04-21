# ProgressBar Types and Modes

## Table of Contents
- [Overview](#overview)
- [Linear ProgressBar](#linear-progressbar)
  - [Basic Linear Implementation](#basic-linear-implementation)
  - [Linear with Secondary Progress](#linear-with-secondary-progress)
  - [Linear with Indeterminate State](#linear-with-indeterminate-state)
  - [Linear with Segments](#linear-with-segments)
  - [Linear Type Best Practices](#linear-type-best-practices)
- [Circular ProgressBar](#circular-progressbar)
  - [Basic Circular Implementation](#basic-circular-implementation)
  - [Circular with Secondary Progress](#circular-with-secondary-progress)
  - [Circular with Indeterminate State](#circular-with-indeterminate-state)
  - [Circular with Segments](#circular-with-segments)
  - [Pie Progress Mode](#pie-progress-mode)
  - [Circular Type Best Practices](#circular-type-best-practices)
- [Choosing Between Linear and Circular](#choosing-between-linear-and-circular)
- [Type-Specific Properties](#type-specific-properties)
- [Complete Examples](#complete-examples)

## Overview

The Syncfusion Blazor ProgressBar supports two fundamental types that determine the visual appearance and layout:

1. **Linear (ProgressType.Linear)** - Horizontal bar representation
2. **Circular (ProgressType.Circular)** - Circular/donut chart representation

Both types support the same core features (secondary progress, indeterminate state, segments, animation) but differ in visual presentation and some type-specific properties.

**Key Decision Factors:**
- **Layout space:** Linear for horizontal layouts, Circular for compact squares
- **Use case:** Linear for file operations, Circular for dashboards/goals
- **Information density:** Linear shows more clearly in tight spaces
- **Visual impact:** Circular draws more attention, better for KPIs

## Linear ProgressBar

The Linear type renders progress as a horizontal bar, ideal for file uploads, downloads, loading states, and sequential progress tracking.

### Basic Linear Implementation

Create a simple linear progress bar with the `Type` property:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="100" 
               Height="60" 
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Properties Explained:**
- `Type="ProgressType.Linear"` - Sets horizontal bar type
- `Value="100"` - Current progress (100 = complete)
- `Height="60"` - Bar height in pixels (recommended: 40-80px)
- `Minimum="0"` and `Maximum="100"` - Progress range

**Visual Result:** A filled horizontal bar from left to right.

### Linear with Secondary Progress

Display dual progress indicators using the `SecondaryProgress` property, useful for buffering scenarios (e.g., video player showing playback position and buffered amount):

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="40" 
               Height="60" 
               SecondaryProgress="60" 
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Properties Explained:**
- `Value="40"` - Primary progress (e.g., current playback position)
- `SecondaryProgress="60"` - Secondary progress (e.g., buffered amount)

**Visual Result:** Two layers - lighter secondary progress (60%) with darker primary progress (40%) on top.

**Use Cases:**
- Video/audio buffering (playback position vs buffered content)
- Multi-stage downloads (downloaded vs extracted)
- Resource loading (loaded vs processed)

### Linear with Indeterminate State

Show loading without known duration using the `IsIndeterminate` property:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="20" 
               Height="60" 
               IsIndeterminate="true" 
               Minimum="0" 
               Maximum="100">
    <ProgressBarAnimation Enable="true"></ProgressBarAnimation>
</SfProgressBar>
```

**Properties Explained:**
- `IsIndeterminate="true"` - Enables continuous animation
- `<ProgressBarAnimation Enable="true">` - Activates the animation

**Visual Result:** Continuously moving progress indicator, signaling ongoing work.

**Use Cases:**
- API calls with unknown response time
- Search operations
- Initial loading states
- Background processing

### Linear with Segments

Divide progress into distinct segments using the `SegmentCount` property:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="100" 
               Height="60" 
               SegmentCount="8" 
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Properties Explained:**
- `SegmentCount="8"` - Divides progress into 8 equal segments

**Visual Result:** Progress bar divided into 8 distinct blocks.

**Use Cases:**
- Multi-step wizards (5-step form = 5 segments)
- Task checklists
- Progress through sequential stages
- Level indicators

### Linear Type Best Practices

**When to Use Linear:**
- ✅ File uploads/downloads with percentage
- ✅ Horizontal layouts and forms
- ✅ Long-running operations with clear start/end
- ✅ Sequential multi-step processes
- ✅ When space height is limited
- ✅ Loading states in headers/footers

**Sizing Recommendations:**
- **Height:** 40-80px (60px ideal)
- **Width:** Auto-fits container (100% by default)
- **Thickness:** 8-24px for track and progress

**Color Recommendations:**
- Use distinct colors for primary and secondary progress
- Ensure track color contrasts with background
- Match progress color to action severity (green for success, blue for info)

## Circular ProgressBar

The Circular type renders progress as a donut or pie chart, ideal for dashboards, compact displays, and goal indicators.

### Basic Circular Implementation

Create a circular progress indicator:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="100" 
               Height="60" 
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Properties Explained:**
- `Type="ProgressType.Circular"` - Sets circular/donut type
- `Height="60"` - Outer diameter (note: different from Linear)
- Automatically centers in container

**Note:** For Circular type, Height controls the overall diameter. For precise sizing, use both Height and Width with "px" units:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="75" 
               Height="160px" 
               Width="160px"
               TrackThickness="8" 
               ProgressThickness="8"
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

### Circular with Secondary Progress

Display dual circular progress using the `SecondaryProgress` property (e.g., goal progress vs stretch goal):

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="40" 
               Height="60" 
               SecondaryProgress="60" 
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Visual Result:** Two concentric arcs - secondary (60%) as background, primary (40%) on top.

**Use Cases:**
- Goal tracking (achieved vs target)
- Dual metrics (sales vs quota)
- Nested progress indicators
- Comparison displays

### Circular with Indeterminate State

Show circular loading spinner using the `IsIndeterminate` property:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="20" 
               Height="60" 
               IsIndeterminate="true" 
               Minimum="0" 
               Maximum="100">
    <ProgressBarAnimation Enable="true"></ProgressBarAnimation>
</SfProgressBar>
```

**Visual Result:** Rotating arc, commonly used as a loading spinner.

**Use Cases:**
- Page loading overlays
- Modal dialog loading states
- Async operation indicators
- "Please wait" messages

### Circular with Segments

Create segmented circular progress using the `SegmentCount` property:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="100" 
               Height="60" 
               SegmentCount="8" 
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Visual Result:** Circular progress divided into 8 arc segments.

**Use Cases:**
- Skill level indicators (8 levels)
- Rating systems
- Achievement progress
- Competency tracking

### Pie Progress Mode

Fill the circle as a pie chart instead of a donut using the `EnablePieProgress` property:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="80" 
               Height="60" 
               EnablePieProgress="true" 
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Properties Explained:**
- `EnablePieProgress="true"` - Fills circle from center (pie chart style)

**Visual Result:** Solid filled circle (80% filled like a pie slice) instead of a donut arc.

**Use Cases:**
- Percentage visualizations (80% usage)
- Capacity indicators (disk space used)
- Allocation displays
- Proportion representations

**Comparison:**
- **Donut (default):** Circular arc around the perimeter
- **Pie (EnablePieProgress="true"):** Filled circle from center

### Circular Type Best Practices

**When to Use Circular:**
- ✅ Dashboards and KPI cards
- ✅ Compact square layouts
- ✅ Goal completion indicators
- ✅ Profile completion meters
- ✅ Loading spinners and overlays
- ✅ Scorecard displays

**Sizing Recommendations:**
- **Diameter:** 120-200px (160px ideal)
- **Thickness:** 6-15px for track and progress
- **InnerRadius:** 70-90% for donut appearance

**Color Recommendations:**
- Use contrasting colors for track and progress
- Consider semantic colors (green for complete, red for warnings)
- Ensure visibility against varied backgrounds

## Choosing Between Linear and Circular

### Use Linear When:

| Scenario | Reason |
|----------|--------|
| File uploads/downloads | Clear start-to-end progression |
| Horizontal layouts | Fits naturally in wide spaces |
| Multi-step forms | Sequential flow is obvious |
| Header/footer loading | Fits in thin horizontal strips |
| Progress with text labels | More space for inline text |
| Very small heights (<30px) | Circular becomes illegible |

### Use Circular When:

| Scenario | Reason |
|----------|--------|
| Dashboard widgets | Compact and eye-catching |
| Square card layouts | Fits in square containers |
| Goal indicators | Visually represents completion |
| Profile completeness | Common UX pattern |
| Loading spinners | Recognized loading pattern |
| KPI displays | Emphasizes the metric |
| Center-focused designs | Draws attention to center |

### Mixed Approach:

Use both in the same application:
- **Linear** for in-context progress (forms, file operations)
- **Circular** for overview metrics (dashboard, summary cards)

## Type-Specific Properties

### Properties for Linear Type ONLY:

| Property | Type | Description |
|----------|------|-------------|
| `IsStriped` | bool | Adds diagonal stripe pattern (visual style) |

**Example:**
```razor
<SfProgressBar Type="ProgressType.Linear" 
               IsStriped="true" 
               Value="60" 
               Height="60">
</SfProgressBar>
```

### Properties for Circular Type ONLY:

| Property | Type | Description |
|----------|------|-------------|
| `Radius` | string | Outer radius as percentage (e.g., "100%", "80%") |
| `InnerRadius` | string | Inner radius as percentage (e.g., "80%", "70%") |
| `EnablePieProgress` | bool | Fill as pie chart instead of arc |

**Example:**
```razor
<SfProgressBar Type="ProgressType.Circular" 
               Radius="90%" 
               InnerRadius="70%" 
               Value="75" 
               Height="160px" 
               Width="160px">
</SfProgressBar>
```

### Properties Available for BOTH Types:

- `Value` - Current progress
- `Minimum` / `Maximum` - Progress range
- `SecondaryProgress` - Secondary progress value
- `IsIndeterminate` - Indeterminate loading state
- `SegmentCount` - Number of segments
- `SegmentColor` - Array of segment colors
- `ProgressColor` / `TrackColor` - Colors
- `ProgressThickness` / `TrackThickness` - Thickness
- `CornerRadius` - Rounded corners
- All animation and event properties

## Complete Examples

### Example 1: File Upload (Linear)

```razor
@using Syncfusion.Blazor.ProgressBar

<div style="padding: 20px;">
    <h4>Uploading: document.pdf</h4>
    
    <SfProgressBar Type="ProgressType.Linear" 
                   Value="@uploadProgress" 
                   Height="60"
                   ShowProgressValue="true"
                   TrackThickness="20"
                   ProgressThickness="20"
                   CornerRadius="CornerType.Round"
                   TrackColor="#e9ecef"
                   ProgressColor="#0d6efd"
                   Minimum="0" 
                   Maximum="100">
        <ProgressBarAnimation Enable="true" Duration="300"></ProgressBarAnimation>
    </SfProgressBar>
    
    <p>@uploadProgress% Complete</p>
</div>

@code {
    private double uploadProgress = 65;
}
```

### Example 2: Dashboard KPI (Circular)

```razor
@using Syncfusion.Blazor.ProgressBar

<div style="text-align: center; padding: 20px;">
    <h4>Goal Achievement</h4>
    
    <SfProgressBar Type="ProgressType.Circular" 
                   Value="82" 
                   Height="180px"
                   Width="180px"
                   TrackThickness="12"
                   ProgressThickness="12"
                   TrackColor="#f0f0f0"
                   ProgressColor="#28a745"
                   Minimum="0" 
                   Maximum="100">
        <ProgressBarAnnotations>
            <ProgressBarAnnotation>
                <ContentTemplate>
                    <div style="font-size: 32px; font-weight: bold; color: #28a745;">
                        82%
                    </div>
                </ContentTemplate>
            </ProgressBarAnnotation>
        </ProgressBarAnnotations>
    </SfProgressBar>
    
    <p style="margin-top: 10px;">$82K of $100K Target</p>
</div>
```

### Example 3: Multi-Step Wizard (Linear Segments)

```razor
@using Syncfusion.Blazor.ProgressBar

<div style="padding: 20px;">
    <h4>Step @currentStep of 5: @GetStepName()</h4>
    
    <SfProgressBar Type="ProgressType.Linear" 
                   Value="@GetProgress()" 
                   Height="60"
                   SegmentCount="5"
                   TrackThickness="16"
                   ProgressThickness="16"
                   TrackColor="#dee2e6"
                   ProgressColor="#6f42c1"
                   Minimum="0" 
                   Maximum="100">
    </SfProgressBar>
</div>

@code {
    private int currentStep = 3;
    
    private double GetProgress()
    {
        return (currentStep / 5.0) * 100;
    }
    
    private string GetStepName()
    {
        return currentStep switch
        {
            1 => "Personal Info",
            2 => "Contact Details",
            3 => "Address",
            4 => "Preferences",
            5 => "Review",
            _ => "Unknown"
        };
    }
}
```

### Example 4: Video Buffer (Linear Secondary Progress)

```razor
@using Syncfusion.Blazor.ProgressBar

<div style="padding: 20px;">
    <h4>Video Playback</h4>
    
    <SfProgressBar Type="ProgressType.Linear" 
                   Value="@playbackPosition" 
                   SecondaryProgress="@bufferedAmount"
                   Height="60"
                   TrackThickness="12"
                   ProgressThickness="12"
                   SecondaryProgressThickness="12"
                   TrackColor="#000000"
                   ProgressColor="#ff0000"
                   SecondaryProgressColor="#666666"
                   Minimum="0" 
                   Maximum="100">
    </SfProgressBar>
    
    <p>Playing: @playbackPosition% | Buffered: @bufferedAmount%</p>
</div>

@code {
    private double playbackPosition = 35;
    private double bufferedAmount = 65;
}
```

### Example 5: Loading Spinner (Circular Indeterminate)

```razor
@using Syncfusion.Blazor.ProgressBar

<div style="display: flex; justify-content: center; align-items: center; height: 200px;">
    <div style="text-align: center;">
        <SfProgressBar Type="ProgressType.Circular" 
                       Value="20" 
                       Height="100px"
                       Width="100px"
                       IsIndeterminate="true"
                       TrackThickness="8"
                       ProgressThickness="8"
                       TrackColor="#e9ecef"
                       ProgressColor="#0d6efd"
                       Minimum="0" 
                       Maximum="100">
            <ProgressBarAnimation Enable="true" Duration="1000"></ProgressBarAnimation>
        </SfProgressBar>
        
        <p style="margin-top: 15px;">Loading, please wait...</p>
    </div>
</div>
```

### Example 6: Pie Progress for Disk Usage (Circular Pie)

```razor
@using Syncfusion.Blazor.ProgressBar

<div style="text-align: center; padding: 20px;">
    <h4>Disk Usage</h4>
    
    <SfProgressBar Type="ProgressType.Circular" 
                   Value="73" 
                   EnablePieProgress="true"
                   Height="160px"
                   Width="160px"
                   ProgressColor="#dc3545"
                   TrackColor="#f8f9fa"
                   Minimum="0" 
                   Maximum="100">
        <ProgressBarAnnotations>
            <ProgressBarAnnotation>
                <ContentTemplate>
                    <div style="font-size: 24px; font-weight: bold;">
                        <div style="color: #dc3545;">73%</div>
                        <div style="font-size: 14px; color: #6c757d;">Used</div>
                    </div>
                </ContentTemplate>
            </ProgressBarAnnotation>
        </ProgressBarAnnotations>
    </SfProgressBar>
    
    <p>730 GB of 1 TB used</p>
</div>
```

## Summary

**Linear ProgressBar:**
- Horizontal bar representation
- Best for sequential operations and horizontal layouts
- Supports striped pattern
- Ideal for file operations and forms

**Circular ProgressBar:**
- Circular/donut representation
- Best for compact displays and dashboards
- Supports pie progress mode
- Ideal for KPIs and goal tracking

Both types support secondary progress, indeterminate states, segments, and full customization. Choose based on layout requirements and use case context.
