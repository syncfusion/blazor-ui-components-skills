---
name: syncfusion-blazor-progress-bar
description: Guide for implementing Syncfusion Blazor ProgressBar components in Blazor applications. Use this when displaying progress indicators, loading states, task completion status, or file upload progress. This skill covers linear and circular progress bars, indeterminate loaders, buffer states, and progress tracking. Ideal for visual feedback during operations, showing completion percentages, and any scenario requiring visual progress indicators.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
  category: "Data Visualization"
---

# Implementing Syncfusion Blazor ProgressBar

## When to Use This Skill

Use this skill when the user needs to:
- Display progress for file uploads, downloads, or data processing
- Show loading states with determinate or indeterminate progress
- Visualize task completion percentages
- Implement buffer states for media players or streaming content
- Create circular or linear progress indicators
- Add progress tracking with custom styling and animations
- Show multi-stage progress with segments
- Implement accessible progress indicators with WCAG compliance
- Display progress with annotations, labels, or custom formatting

## Component Overview

The Syncfusion Blazor ProgressBar component is a visual indicator that displays the progress of an operation. It supports both linear and circular types, multiple states (determinate, indeterminate, buffer, active, striped), extensive customization options, animations, and full accessibility support.

## API Reference

📄 **Read:** [references/api-reference.md](references/api-reference.md)

Use this reference when you need the exact ProgressBar API surface, including:
- `SfProgressBar` properties such as `Value`, `Type`, `Role`, `Theme`, `Visible`, `ID`, `StartAngle`, `EndAngle`, and `RefreshAsync()`
- Child components such as `ProgressBarAnimation`, `ProgressBarEvents`, `ProgressBarAnnotations`, `ProgressBarAnnotation`, `ProgressBarLabelStyle`, `ProgressBarMargin`, `ProgressBarRangeColor`, and `ProgressBarRangeColors`
- Event callbacks such as `ValueChanged`, `ProgressCompleted`, `AnimationComplete`, `AnnotationRender`, `TextRender`, and `Loaded`
- Enums such as `ProgressType`, `CornerType`, `ModeType`, and `TextAlignmentType`

**Key Capabilities:**
- **Types:** Linear (horizontal bar) and Circular (donut/pie chart style)
- **States:** Determinate (known progress), Indeterminate (unknown progress), Buffer (dual progress), Active (animated), Striped (visual pattern)
- **Customization:** Colors, thickness, segments, radius, corners, margins, RTL support
- **Features:** Annotations, labels, range colors, gradients, secondary progress
- **Animation:** Configurable speed and delay
- **Events:** Value changes, completion, animation lifecycle
- **Accessibility:** WCAG 2.2 compliant with keyboard navigation

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When the user needs to set up the ProgressBar for the first time, guide them to this reference for:
- Prerequisites and system requirements
- Installation via Visual Studio, VS Code, or .NET CLI
- NuGet package installation (Syncfusion.Blazor.ProgressBar)
- Namespace imports and service registration
- Adding stylesheet and script references
- Basic linear and circular ProgressBar implementation
- First working example

### Types and Modes
📄 **Read:** [references/types-and-modes.md](references/types-and-modes.md)

When the user asks about or needs:
- Linear vs Circular ProgressBar types
- Choosing the right type for their use case
- Implementing basic linear progress bars
- Implementing circular/donut progress bars
- Pie progress mode for circular bars
- Segments in linear or circular types
- Secondary progress indicators
- Complete type-specific examples

### States and Behavior
📄 **Read:** [references/states-and-behavior.md](references/states-and-behavior.md)

When the user asks about or needs:
- Determinate progress (known completion percentage)
- Indeterminate progress (loading without known duration)
- Buffer state (dual progress for media/streaming)
- Active state (animated progress indicator)
- Striped visual pattern (linear only)
- Range configuration (Minimum/Maximum values)
- Combining multiple states
- Choosing the right state for their scenario

### Customization
📄 **Read:** [references/customization.md](references/customization.md)

When the user asks about or needs:
- Dividing progress into segments with custom colors
- Adjusting track, progress, or secondary progress thickness
- Customizing radius and inner radius (circular)
- Rounded corners (CornerRadius)
- Custom colors for progress, track, and secondary progress
- Range colors with gradient effects
- RTL (right-to-left) support
- Visibility control (showing/hiding progress bar)
- Margin and spacing adjustments
- Complete visual customization examples

### Annotations and Labels
📄 **Read:** [references/annotations-and-labels.md](references/annotations-and-labels.md)

When the user asks about or needs:
- Adding text annotations to the progress bar
- Custom HTML or component overlays
- Showing progress percentage labels
- Custom label formatting (TextRender event)
- Positioning annotations
- Multiple annotations
- Styling annotations and labels
- Combining annotations with labels

### Animation
📄 **Read:** [references/animation.md](references/animation.md)

When the user asks about or needs:
- Enabling progress animations
- Controlling animation speed (Duration)
- Adding animation delays
- Animation with different states
- AnimationComplete event handling
- Performance considerations
- Smooth progress transitions

### Events and Accessibility
📄 **Read:** [references/events-and-accessibility.md](references/events-and-accessibility.md)

When the user asks about or needs:
- ValueChanged event (tracking progress changes)
- ProgressCompleted event (when progress reaches maximum)
- AnimationComplete event (animation lifecycle)
- AnnotationRender event (customizing annotations)
- TextRender event (custom label formatting)
- Loaded event (component initialization)
- WCAG 2.2 and Section 508 compliance
- Keyboard navigation support
- Screen reader compatibility
- Color contrast and accessibility standards
- Mobile device support

## Quick Start Example

### Linear ProgressBar (Basic)

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="70" 
               Minimum="0" 
               Maximum="100"
               Height="60"
               TrackThickness="12" 
               ProgressThickness="12">
</SfProgressBar>
```

### Circular ProgressBar (Basic)

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="75" 
               Minimum="0" 
               Maximum="100"
               Height="160px"
               Width="160px"
               TrackThickness="8" 
               ProgressThickness="8">
</SfProgressBar>
```

### Indeterminate Loading State

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="20" 
               IsIndeterminate="true"
               Height="60"
               Minimum="0" 
               Maximum="100">
    <ProgressBarAnimation Enable="true" Duration="2000"></ProgressBarAnimation>
</SfProgressBar>
```

## Common Patterns

### Pattern 1: File Upload Progress
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="@uploadProgress" 
               ShowProgressValue="true"
               Height="60"
               ProgressColor="#28a745"
               TrackColor="#e9ecef"
               CornerRadius="CornerType.Round">
    <ProgressBarEvents ProgressCompleted="OnUploadComplete"></ProgressBarEvents>
</SfProgressBar>

@code {
    private double uploadProgress = 0;
    
    private void OnUploadComplete(ProgressValueEventArgs args)
    {
        // Handle upload completion
    }
}
```

### Pattern 2: Multi-Stage Progress with Segments
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="100" 
               Height="60"
               SegmentCount="4"
               SegmentColor='new string[] { "#00bdaf", "#2f7ecc", "#e9648e", "#fbb78a" }'>
</SfProgressBar>
```

### Pattern 3: Buffer State for Media Player
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="@currentProgress" 
               SecondaryProgress="@bufferProgress"
               Height="60"
               ProgressColor="#ff4081"
               SecondaryProgressColor="#ff80ab">
</SfProgressBar>

@code {
    private double currentProgress = 0;
    private double bufferProgress = 0;

    protected override void OnInitialized()
    {
        // Sample values for testing
        currentProgress = 40;   // main progress
        bufferProgress = 70;    // secondary buffer progress
    }
}

```

### Pattern 4: Circular Progress with Annotation
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="80" 
               Height="160px" 
               Width="160px"
               TrackThickness="10" 
               ProgressThickness="10"
               ProgressColor="#e91e63">
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="font-size:24px; font-weight:bold; color:#e91e63;">
                    80%
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>
```

## Key Props Reference

### Core Properties
- **Type**: `ProgressType.Linear` or `ProgressType.Circular` - Progress bar type
- **Value**: Current progress value within the configured range
- **Minimum**: Minimum range value
- **Maximum**: Maximum range value
- **Height**: Component height
- **Width**: Component width
- **ID**: Component identifier for programmatic targeting
- **ChildContent**: Custom child content inside the component

### State Properties
- **IsIndeterminate**: Enable indeterminate loading state (unknown duration)
- **SecondaryProgress**: Secondary progress value for buffer state
- **IsActive**: Enable active animated state
- **IsStriped**: Enable striped pattern (linear only)
- **Role**: Progress indication mode (`ModeType.Auto`, `Success`, `Info`, `Danger`, `Warning`)
- **Theme**: Visual theme for the ProgressBar
- **EnableRtl**: Right-to-left rendering support
- **Visible**: Show or hide the component

### Customization Properties
- **ProgressColor**: Progress indicator color
- **TrackColor**: Background track color
- **SecondaryProgressColor**: Secondary progress color
- **ProgressThickness**: Progress bar thickness
- **TrackThickness**: Track thickness
- **SecondaryProgressThickness**: Secondary progress thickness
- **CornerRadius**: Edge rounding (`CornerType.Auto`, `Square`, `Round`, `Round4px`)
- **SegmentCount**: Divide progress into segments
- **SegmentColor**: Array of colors for segments
- **EnableProgressSegments**: Segments without track background
- **EnablePieProgress**: Circular pie-style rendering
- **Radius**: Circular progress radius
- **InnerRadius**: Circular inner radius
- **GapWidth**: Gap width between segments
- **StartAngle**: Circular start angle
- **EndAngle**: Circular end angle
- **IsGradient**: Enable gradient rendering

### Display Properties
- **ShowProgressValue**: Show percentage label
- **EnablePieProgress**: Fill circular progress as pie chart

### Animation Properties
Use `<ProgressBarAnimation>` child component:
- **Enable**: Enable animation
- **Duration**: Animation duration in milliseconds
- **Delay**: Animation start delay in milliseconds

### Methods
- **RefreshAsync()**: Re-render the ProgressBar when the state changes outside the normal render flow

## Common Use Cases

### Use Case 1: Simple Loading Indicator
**Scenario:** Show a loading bar while fetching data
**Implementation:** Use indeterminate linear progress bar with animation

### Use Case 2: File Upload with Percentage
**Scenario:** Display file upload progress with percentage
**Implementation:** Determinate progress with `ShowProgressValue="true"`

### Use Case 3: Multi-Step Wizard
**Scenario:** Show progress through a multi-step form
**Implementation:** Segmented progress bar with step count as segments

### Use Case 4: Video Buffer State
**Scenario:** Show playback position and buffered content
**Implementation:** Buffer state with primary (playback) and secondary (buffered) progress

### Use Case 5: Dashboard KPI Indicator
**Scenario:** Circular progress showing goal completion
**Implementation:** Circular progress with annotation showing percentage and target

### Use Case 6: Download Progress with Time Estimate
**Scenario:** Show download progress with custom time remaining label
**Implementation:** Linear progress with TextRender event for custom formatting

### Use Case 7: Determinate Task Progress
**Scenario:** Show progress of a batch operation with known steps
**Implementation:** Determinate progress bar updating Value as each step completes

## Best Practices

1. **Choose the Right Type:**
   - Use **Linear** for horizontal layouts, file operations, loading bars
   - Use **Circular** for compact spaces, dashboards, goal indicators

2. **Select Appropriate State:**
   - **Determinate** when you know the completion percentage
   - **Indeterminate** for unknown duration operations
   - **Buffer** for streaming or multi-stage loading
   - **Active/Striped** for visual emphasis on ongoing operations

3. **Provide User Feedback:**
   - Show percentage labels for long operations
   - Use annotations for contextual information
   - Handle ProgressCompleted event to notify users

4. **Accessibility:**
   - Ensure adequate color contrast
   - Don't rely solely on color to convey information
   - Test with screen readers
   - Support keyboard navigation

5. **Performance:**
   - Avoid excessive Value updates (throttle updates for smooth animation)
   - Use appropriate animation durations (1000-2000ms typically)
   - Consider disabling animation for very frequent updates

6. **Styling:**
   - Match your application's design system
   - Use range colors for meaningful thresholds
   - Ensure progress bar is visible against background
   - Use appropriate thickness for the context

## Troubleshooting Quick Reference

- **Progress bar not visible:** Check Height, Width, and color contrast with background
- **Animation not working:** Ensure `<ProgressBarAnimation Enable="true" />`
- **Themes not applied:** Verify stylesheet reference in index.html
- **Events not firing:** Check ProgressBarEvents component and method signatures
- **Striped not showing:** Striped only works with Linear type
- **Percentage not displaying:** Set `ShowProgressValue="true"`

For detailed setup, configuration, and advanced scenarios, navigate to the appropriate reference file above.
