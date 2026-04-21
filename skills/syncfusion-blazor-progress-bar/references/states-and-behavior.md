# ProgressBar States and Behavior

## Table of Contents
- [Overview](#overview)
- [Determinate State](#determinate-state)
  - [When to Use Determinate](#when-to-use-determinate)
  - [Implementation](#implementation)
  - [Range Configuration](#range-configuration)
  - [Updating Progress Dynamically](#updating-progress-dynamically)
- [Indeterminate State](#indeterminate-state)
  - [When to Use Indeterminate](#when-to-use-indeterminate)
  - [Implementation](#implementation-1)
  - [Animation Configuration](#animation-configuration)
  - [Combining with Determinate](#combining-with-determinate)
- [Buffer State](#buffer-state)
  - [When to Use Buffer State](#when-to-use-buffer-state)
  - [Implementation](#implementation-2)
  - [Primary vs Secondary Progress](#primary-vs-secondary-progress)
  - [Use Cases](#use-cases)
- [Active State](#active-state)
  - [When to Use Active State](#when-to-use-active-state)
  - [Implementation](#implementation-3)
  - [Visual Indicators](#visual-indicators)
- [Striped State](#striped-state)
  - [When to Use Striped](#when-to-use-striped)
  - [Implementation](#implementation-4)
  - [Limitations](#limitations)
- [Combining Multiple States](#combining-multiple-states)
- [State Selection Guide](#state-selection-guide)

## Overview

The Blazor ProgressBar component supports five distinct states that control how progress is displayed and animated. Understanding when and how to use each state ensures the best user experience for your specific scenario.

**Available States:**
1. **Determinate** (default) - Known progress percentage
2. **Indeterminate** - Unknown duration, continuous animation
3. **Buffer** - Dual progress indicators (primary + secondary)
4. **Active** - Animated progress with flowing pattern
5. **Striped** - Visual diagonal stripe pattern (Linear only)

**Key Concepts:**
- States can be combined (e.g., Buffer + Active + Striped)
- Choose state based on whether progress is measurable
- Animation enhances visual feedback but impacts performance
- Different states serve different user expectations

## Determinate State

The **determinate state** is the default behavior where the progress is known and can be measured as a percentage. This is the most common state for operations with predictable duration or trackable progress. The `Value` property is used to set the current progress, while `Minimum` and `Maximum` properties define the progress range.

### When to Use Determinate

The `Value`, `Minimum`, and `Maximum` properties are used in determinate scenarios. Use determinate progress when:
- ✅ You know the total amount of work (file size, item count, steps)
- ✅ Progress can be calculated as a percentage
- ✅ User needs to estimate time remaining
- ✅ Operation has clear start and end points

**Examples:**
- File upload/download with byte tracking
- Multi-step wizard with defined steps
- Data processing with item count
- Form completion percentage
- Installation progress

### Implementation

The determinate state requires the `Value`, `Minimum`, `Maximum`, and `Height` properties:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="100" 
               Height="60" 
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Properties:**
- `Value="100"` - Current progress (100 means complete)
- `Minimum="0"` - Starting point of range
- `Maximum="100"` - Ending point of range

**Visual Result:** A static progress bar filled to 100%.

### Range Configuration

The `Minimum` and `Maximum` properties are used to define the progress range. The `Value` property is then calculated relative to this range. While `0` to `100` is standard, you can use any range:

**Standard 0-100 Range:**
```razor
<SfProgressBar Value="50" Minimum="0" Maximum="100" Height="60">
</SfProgressBar>
```
Result: 50% progress

**Custom Range (0-500):**
```razor
<SfProgressBar Value="250" Minimum="0" Maximum="500" Height="60">
</SfProgressBar>
```
Result: 50% progress (250 out of 500)

**Custom Range (10-90):**
```razor
<SfProgressBar Value="50" Minimum="10" Maximum="90" Height="60">
</SfProgressBar>
```
Result: 50% progress relative to 10-90 range

**Use Cases for Custom Ranges:**
- File sizes (0 to fileSize in bytes)
- Temperature scales (32°F to 212°F)
- Price ranges ($100 to $1000)
- Custom metrics (10 to 1000 units)

### Updating Progress Dynamically

The `Value`, `ShowProgressValue`, `TrackThickness`, and `ProgressThickness` properties are used for dynamic updates. To update progress in real-time, bind the `Value` property to a C# field and use the `ProgressBarAnimation` element to control animation:

```razor
@using Syncfusion.Blazor.ProgressBar

<div style="padding: 20px;">
    <h4>File Upload Progress</h4>
    
    <SfProgressBar Type="ProgressType.Linear" 
                   Value="@currentProgress" 
                   Height="60"
                   ShowProgressValue="true"
                   TrackThickness="16"
                   ProgressThickness="16"
                   Minimum="0" 
                   Maximum="100">
        <ProgressBarAnimation Enable="true" Duration="300"></ProgressBarAnimation>
    </SfProgressBar>
    
    <p>@currentProgress% Complete</p>
    <button class="btn btn-primary" @onclick="SimulateUpload">Start Upload</button>
</div>

@code {
    private double currentProgress = 0;
    private System.Threading.Timer? timer;
    
    private void SimulateUpload()
    {
        currentProgress = 0;
        timer = new System.Threading.Timer(_ =>
        {
            if (currentProgress < 100)
            {
                currentProgress += 10;
                InvokeAsync(StateHasChanged);
            }
            else
            {
                timer?.Dispose();
            }
        }, null, 0, 500); // Update every 500ms
    }
    
    public void Dispose()
    {
        timer?.Dispose();
    }
}
```

## Indeterminate State

The **indeterminate state** displays continuous animation when progress cannot be measured or calculated. This provides visual feedback that work is happening without showing specific progress. The `IsIndeterminate` property is used to enable indeterminate mode, while the `ProgressBarAnimation` element (with `Enable` and `Duration` properties) controls the animation behavior.

### When to Use Indeterminate

The `IsIndeterminate` property is used to enable indeterminate mode. Use indeterminate progress when:
- ✅ Operation duration is unknown
- ✅ Progress cannot be measured
- ✅ Waiting for server response
- ✅ Background processing
- ✅ Initial loading states

**Examples:**
- API calls without progress tracking
- Search operations
- Authentication processes
- Database queries
- Page loading
- Establishing connections

### Implementation

Enable indeterminate state with the `IsIndeterminate` property set to `true`. The `ProgressBarAnimation` element with `Enable` and `Duration` properties controls the continuous animation:

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

**Properties:**
- `IsIndeterminate="true"` - Enables continuous animation
- `Value="20"` - Initial position (any value, typically 20-40)
- `<ProgressBarAnimation Enable="true">` - Required for animation

**Visual Result:** Progress bar with moving animation, indicating ongoing work.

**Circular Indeterminate:**
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

Creates a rotating spinner, commonly used as a loading indicator.

### Animation Configuration

The `ProgressBarAnimation` element with `Enable`, `Duration`, and `Delay` properties are used to control animation speed and delay for indeterminate progress:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="20" 
               IsIndeterminate="true" 
               Height="60">
    <ProgressBarAnimation Enable="true" 
                         Duration="2000" 
                         Delay="0">
    </ProgressBarAnimation>
</SfProgressBar>
```

**Animation Properties:**
- `Enable="true"` - Activates animation (required)
- `Duration="2000"` - Animation cycle duration in milliseconds (default: 2000ms)
- `Delay="0"` - Start delay in milliseconds (default: 0ms)

**Recommendations:**
- **Fast:** 1000-1500ms for quick operations
- **Normal:** 2000-2500ms for standard loading
- **Slow:** 3000-4000ms for subtle animation

### Combining with Determinate

The `IsIndeterminate`, `Value`, and `ProgressBarAnimation` properties are toggled to transition from indeterminate to determinate mode when progress becomes known:

```razor
@using Syncfusion.Blazor.ProgressBar

<div style="padding: 20px;">
    <h4>@statusMessage</h4>
    
    <SfProgressBar Type="ProgressType.Linear" 
                   Value="@progressValue" 
                   Height="60"
                   IsIndeterminate="@isIndeterminate"
                   ShowProgressValue="@(!isIndeterminate)"
                   TrackThickness="16"
                   ProgressThickness="16"
                   Minimum="0" 
                   Maximum="100">
        <ProgressBarAnimation Enable="true" Duration="2000"></ProgressBarAnimation>
    </SfProgressBar>
    
    <button class="btn btn-primary" @onclick="StartProcess">Start Process</button>
</div>

@code {
    private double progressValue = 20;
    private bool isIndeterminate = true;
    private string statusMessage = "Initializing...";
    private System.Threading.Timer? timer;
    
    private void StartProcess()
    {
        isIndeterminate = true;
        statusMessage = "Connecting to server...";
        progressValue = 20;
        
        // Simulate connection phase (indeterminate)
        Task.Delay(2000).ContinueWith(_ =>
        {
            InvokeAsync(() =>
            {
                isIndeterminate = false;
                statusMessage = "Processing...";
                progressValue = 0;
                StateHasChanged();
                
                // Start determinate progress
                timer = new System.Threading.Timer(callback =>
                {
                    if (progressValue < 100)
                    {
                        progressValue += 10;
                        InvokeAsync(StateHasChanged);
                    }
                    else
                    {
                        statusMessage = "Complete!";
                        timer?.Dispose();
                        InvokeAsync(StateHasChanged);
                    }
                }, null, 0, 500);
            });
        });
    }
    
    public void Dispose()
    {
        timer?.Dispose();
    }
}
```

**Pattern:**
1. Start with `IsIndeterminate="true"` during initial phase
2. Switch to `IsIndeterminate="false"` when progress becomes trackable
3. Update `Value` property as work progresses

## Buffer State

The **buffer state** displays two progress indicators simultaneously - primary progress and secondary progress. This is particularly useful for media players and multi-stage operations. The `Value` property sets the primary progress, while the `SecondaryProgress` property sets the secondary progress. Additional properties like `ProgressColor`, `SecondaryProgressColor`, `TrackColor`, `ProgressThickness`, and `SecondaryProgressThickness` control the visual appearance.

### When to Use Buffer State

The `Value` and `SecondaryProgress` properties are used together to display dual progress indicators. Use buffer state when:
- ✅ Two related progress values need simultaneous display
- ✅ Showing buffered content ahead of current position
- ✅ Tracking download vs processing progress
- ✅ Displaying loaded vs rendered content

**Examples:**
- Video player (playback position vs buffered)
- Audio streaming (played vs buffered)
- Download + extraction (downloaded vs extracted)
- Load + process (loaded items vs processed items)

### Implementation

Use the `SecondaryProgress` property alongside the `Value` property to display dual progress. The `Minimum` and `Maximum` properties define the range for both indicators:

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

**Properties:**
- `Value="40"` - Primary progress (e.g., playback position at 40%)
- `SecondaryProgress="60"` - Secondary progress (e.g., buffered up to 60%)

**Visual Result:** Two layers - lighter secondary progress (60%) with darker primary (40%) overlaid.

### Primary vs Secondary Progress

The `Value` property controls primary progress, while `SecondaryProgress` controls secondary progress. Color and thickness are customized using `ProgressColor`, `SecondaryProgressColor`, `TrackColor`, `ProgressThickness`, and `SecondaryProgressThickness` properties.

**Visual Hierarchy:**
- **Secondary Progress (background):** Lighter color, shows ahead progress
- **Primary Progress (foreground):** Darker color, shows current position

**Color Customization:**
```razor
<SfProgressBar Type="ProgressType.Linear" 
               Value="35" 
               SecondaryProgress="65"
               Height="60"
               ProgressColor="#ff0000"
               SecondaryProgressColor="#ffcccc"
               TrackColor="#000000"
               TrackThickness="16"
               ProgressThickness="16"
               SecondaryProgressThickness="16">
</SfProgressBar>
```

### Use Cases

The `Value` and `SecondaryProgress` properties are updated dynamically to reflect real-time progress for media and multi-stage operations. Color properties like `ProgressColor`, `SecondaryProgressColor`, and `TrackColor` enhance visual differentiation.

**Video Playback:**
```razor
@using Syncfusion.Blazor.ProgressBar

<div style="padding: 20px;">
    <h4>Video Player</h4>
    
    <SfProgressBar Type="ProgressType.Linear" 
                   Value="@playbackPosition" 
                   SecondaryProgress="@bufferedAmount"
                   Height="60"
                   ProgressColor="#e50914"
                   SecondaryProgressColor="#999999"
                   TrackColor="#333333"
                   TrackThickness="8"
                   ProgressThickness="8"
                   SecondaryProgressThickness="8"
                   Minimum="0" 
                   Maximum="100">
    </SfProgressBar>
    
    <p>Playback: @playbackPosition% | Buffered: @bufferedAmount%</p>
</div>

@code {
    private double playbackPosition = 35;
    private double bufferedAmount = 68;
}
```

**Circular Buffer State:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="40" 
               Height="160px"
               Width="160px"
               SecondaryProgress="60" 
               TrackThickness="12"
               ProgressThickness="12"
               SecondaryProgressThickness="12"
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

## Active State

The **active state** adds animated visual effects to the progress bar, creating a flowing or shimmering appearance that emphasizes ongoing activity. The `IsActive` property is used to enable the active animated state, and the `ProgressBarAnimation` element (with `Enable` and `Duration` properties) controls the animation effects.

### When to Use Active State

The `IsActive` property is used to enable active state animation. Use active state when:
- ✅ Emphasizing that work is actively happening
- ✅ Drawing attention to progress
- ✅ Indicating live updates
- ✅ Showing real-time processing

**Examples:**
- Active downloads with updates
- Real-time data processing
- Live streaming buffers
- Active synchronization

### Implementation

Enable active state with the `IsActive` property set to `true`. The `ProgressBarAnimation` element with `Enable` and `Duration` properties controls the animation behavior:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               IsActive="true" 
               Value="40" 
               Height="60" 
               Minimum="0" 
               Maximum="100">
    <ProgressBarAnimation Enable="true"></ProgressBarAnimation>
</SfProgressBar>
```

**Properties:**
- `IsActive="true"` - Enables active animated state
- `<ProgressBarAnimation Enable="true">` - Required for animation

### Visual Indicators

The active state creates a flowing animation effect over the progress bar, giving visual feedback that the operation is actively running (not stalled or paused). The `Value`, `IsActive`, `ProgressColor`, and `TrackColor` properties work together with `ProgressBarAnimation` to create the visual effect.

**Active + Custom Colors:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               IsActive="true" 
               Value="60" 
               Height="60"
               ProgressColor="#28a745"
               TrackColor="#e9ecef"
               TrackThickness="20"
               ProgressThickness="20">
    <ProgressBarAnimation Enable="true" Duration="1500"></ProgressBarAnimation>
</SfProgressBar>
```

## Striped State

The **striped state** adds diagonal stripe patterns to the progress bar for visual emphasis. This state is **only available for Linear type**. The `IsStriped` property is used to enable the striped pattern on linear progress bars.

### When to Use Striped

The `IsStriped` property is used to enable diagonal stripe patterns on linear progress bars. Use striped pattern when:
- ✅ Need visual differentiation from other progress bars
- ✅ Emphasizing active or important operations
- ✅ Following design guidelines requiring striped patterns
- ✅ Matching specific UI themes (Bootstrap-style)

**Examples:**
- Important uploads requiring attention
- Warning or critical progress indicators
- Visually distinct loading states

### Implementation

Enable striped pattern with the `IsStriped` property set to `true`. The `Type="ProgressType.Linear"` property is required for striped patterns to work:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               IsStriped="true" 
               Value="40" 
               Height="60" 
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Properties:**
- `IsStriped="true"` - Adds diagonal stripe pattern

**Visual Result:** Progress bar with diagonal stripes.

### Limitations

⚠️ **Linear Type Only:** The `IsStriped` property only works with `Type="ProgressType.Linear"`. It has no effect on `Type="ProgressType.Circular"`.

**Incorrect (no effect):**
```razor
<SfProgressBar Type="ProgressType.Circular" 
               IsStriped="true"  <!-- Ignored for Circular -->
               Value="50">
</SfProgressBar>
```

## Combining Multiple States

The `IsActive`, `IsStriped`, `IsIndeterminate`, `Value`, `SecondaryProgress`, and `ProgressBarAnimation` properties can be combined together for rich visual feedback:

**Example 1: Active + Striped + Buffer**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="35" 
               SecondaryProgress="60"
               IsActive="true"
               IsStriped="true"
               Height="60"
               TrackThickness="20"
               ProgressThickness="20"
               SecondaryProgressThickness="20"
               Minimum="0" 
               Maximum="100">
    <ProgressBarAnimation Enable="true" Duration="1500"></ProgressBarAnimation>
</SfProgressBar>
```

**Result:** Striped progress bar with active animation and secondary buffer progress.

**Example 2: Indeterminate + Active**
```razor
<SfProgressBar Type="ProgressType.Linear" 
               Value="20" 
               IsIndeterminate="true"
               IsActive="true"
               Height="60"
               Minimum="0" 
               Maximum="100">
    <ProgressBarAnimation Enable="true" Duration="2000"></ProgressBarAnimation>
</SfProgressBar>
```

**Result:** Continuously moving indeterminate progress with active animation effect.

**Valid Combinations:**

| State 1 | State 2 | State 3 | Valid | Notes |
|---------|---------|---------|-------|-------|
| Determinate | Active | Striped | ✅ | Common combination |
| Indeterminate | Active | - | ✅ | Enhanced loading state |
| Buffer | Active | Striped | ✅ | All features combined |
| Buffer | Striped | - | ✅ | Static dual progress |
| Determinate | Striped | - | ✅ | Classic striped progress |
| Indeterminate | Striped | - | ✅ | Moving striped loader |

## State Selection Guide

Use this decision tree to choose the appropriate state:

```
Do you know the completion percentage?
├─ YES → Determinate State
│         └─ Need dual progress? → Buffer State
│         └─ Emphasize activity? → Active State
│         └─ Need visual pattern? → Striped State
│
└─ NO → Indeterminate State
          └─ Emphasize activity? → Active State
```

**Quick Reference Table:**

| Scenario | Recommended State | Example |
|----------|------------------|---------|
| File upload with bytes | Determinate | Progress from 0-100% |
| API call (no tracking) | Indeterminate | Loading spinner |
| Video buffering | Buffer (Determinate) | Played: 30%, Buffered: 60% |
| Important download | Active + Striped | Animated striped bar |
| Initial page load | Indeterminate | Unknown duration |
| Multi-step form | Determinate | Step 3 of 5 |
| Database query | Indeterminate | Unknown duration |
| Batch processing | Determinate | Item 450 of 1000 |
| Real-time sync | Active + Buffer | Live updates with buffer |

**Best Practices:**

1. **Default to Determinate** when progress is trackable
2. **Use Indeterminate** for unknown durations (avoid showing static 0%)
3. **Add Active state** for emphasis on live operations
4. **Use Striped** sparingly (can be visually overwhelming)
5. **Buffer state** for dual metrics or buffering scenarios
6. **Combine states** judiciously (too many effects can distract)
7. **Test animations** on slower devices for performance

**Accessibility Note:** Ensure progress updates are announced to screen readers by using appropriate ARIA attributes, especially for indeterminate states where visual changes aren't as obvious.
