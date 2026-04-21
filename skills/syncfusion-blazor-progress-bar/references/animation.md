# Animation

## Table of Contents
- [Overview](#overview)
- [Enabling Animation](#enabling-animation)
  - [ProgressBarAnimation Configuration](#progressbaranimation-configuration)
  - [Enable Property](#enable-property)
- [Animation Properties](#animation-properties)
  - [Duration (Speed Control)](#duration-speed-control)
  - [Delay (Start Delay)](#delay-start-delay)
- [Animation with Different States](#animation-with-different-states)
  - [Determinate Animation](#determinate-animation)
  - [Indeterminate Animation](#indeterminate-animation)
  - [Active State Animation](#active-state-animation)
- [Animation Events](#animation-events)
  - [AnimationComplete Event](#animationcomplete-event)
  - [Coordinating with Other Events](#coordinating-with-other-events)
- [Performance Considerations](#performance-considerations)
- [Complete Examples](#complete-examples)

## Overview

Animation enhances the ProgressBar by providing smooth visual transitions when progress values change or during indeterminate loading states. The `ProgressBarAnimation` component with `Enable`, `Duration`, and `Delay` properties controls all animation behavior. Properly configured animations improve user experience by:

- Providing visual feedback that something is happening
- Making progress changes appear smooth and natural
- Drawing attention to progress updates
- Indicating active processing during indeterminate states

**Animation Capabilities:**
- **Smooth progress transitions** - Animated fill from current to new value
- **Indeterminate motion** - Continuous movement for unknown duration operations
- **Configurable speed** - Control animation duration
- **Delayed starts** - Add initial delay before animation begins
- **Completion events** - Execute code when animation finishes

## Enabling Animation

### ProgressBarAnimation Configuration

Enable animation using the `<ProgressBarAnimation>` child component within `<SfProgressBar>`. The `Enable` property controls whether animation is active:

**Basic Animation Setup:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="90" 
               Height="60" 
               Width="90%" 
               TrackColor="#FFFFFF" 
               ShowProgressValue="true" 
               ProgressColor="#2BB20E" 
               TrackThickness="24" 
               CornerRadius="CornerType.Round" 
               ProgressThickness="24" 
               Minimum="0" 
               Maximum="100">
    <ProgressBarAnimation Enable="true" Duration="2000" Delay="0">
    </ProgressBarAnimation>
</SfProgressBar>
```

**Component Structure:**
```razor
<SfProgressBar ...>
    <ProgressBarAnimation Enable="true" Duration="2000" Delay="0">
    </ProgressBarAnimation>
</SfProgressBar>
```

### Enable Property

The `Enable` property activates animation on the `<ProgressBarAnimation>` component:

**Animation Enabled:**
```razor
<SfProgressBar Value="75" Height="60">
    <ProgressBarAnimation Enable="true"></ProgressBarAnimation>
</SfProgressBar>
```
**Result:** Progress animates smoothly when value changes.

**Animation Disabled (Default):**
```razor
<SfProgressBar Value="75" Height="60">
    <!-- No animation component, or Enable="false" -->
</SfProgressBar>
```
**Result:** Progress jumps instantly to new value without animation.

**Toggling Animation:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="@progress" Height="60">
    <ProgressBarAnimation Enable="@animationEnabled"></ProgressBarAnimation>
</SfProgressBar>

<button @onclick="ToggleAnimation">
    @(animationEnabled ? "Disable" : "Enable") Animation
</button>

@code {
    private double progress = 50;
    private bool animationEnabled = true;
    
    private void ToggleAnimation()
    {
        animationEnabled = !animationEnabled;
    }
}
```

## Animation Properties

### Duration (Speed Control)

The `Duration` property on `<ProgressBarAnimation>` controls how long the animation takes to complete, specified in milliseconds:

**Fast Animation (1 second):**
```razor
<SfProgressBar Value="80" Height="60">
    <ProgressBarAnimation Enable="true" Duration="1000"></ProgressBarAnimation>
</SfProgressBar>
```

**Normal Animation (2 seconds - Default):**
```razor
<SfProgressBar Value="80" Height="60">
    <ProgressBarAnimation Enable="true" Duration="2000"></ProgressBarAnimation>
</SfProgressBar>
```

**Slow Animation (4 seconds):**
```razor
<SfProgressBar Value="80" Height="60">
    <ProgressBarAnimation Enable="true" Duration="4000"></ProgressBarAnimation>
</SfProgressBar>
```

**Duration Guidelines:**

| Duration | Use Case | Example |
|----------|----------|---------|
| 300-500ms | Quick updates, real-time data | Live counters, fast operations |
| 1000-1500ms | Standard updates | File progress updates |
| 2000-2500ms | Smooth, noticeable transitions | Initial loading, page loads |
| 3000-5000ms | Slow, deliberate animations | Large file operations, emphasis |

**Dynamic Duration Based on Change:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="@currentValue" Height="60">
    <ProgressBarAnimation Enable="true" Duration="@animationDuration">
    </ProgressBarAnimation>
</SfProgressBar>

<button @onclick="SmallIncrement">+10 (Fast)</button>
<button @onclick="LargeIncrement">+50 (Slow)</button>

@code {
    private double currentValue = 0;
    private int animationDuration = 1000;
    
    private void SmallIncrement()
    {
        currentValue += 10;
        animationDuration = 500; // Fast for small changes
    }
    
    private void LargeIncrement()
    {
        currentValue += 50;
        animationDuration = 2000; // Slower for large changes
    }
}
```

### Delay (Start Delay)

The `Delay` property on `<ProgressBarAnimation>` adds a pause before the animation starts, specified in milliseconds:

**No Delay (Immediate - Default):**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="70" Height="60">
    <ProgressBarAnimation Enable="true" Duration="2000" Delay="0">
    </ProgressBarAnimation>
</SfProgressBar>
```

**500ms Delay:**
```razor
<SfProgressBar Value="70" Height="60">
    <ProgressBarAnimation Enable="true" Duration="2000" Delay="500">
    </ProgressBarAnimation>
</SfProgressBar>
```

**2 Second Delay:**
```razor
<SfProgressBar Value="70" Height="60">
    <ProgressBarAnimation Enable="true" Duration="2000" Delay="1900">
    </ProgressBarAnimation>
</SfProgressBar>
```

**Use Cases for Delay:**
- Staggered animations (multiple progress bars start at different times)
- Wait for user to read information before animating
- Coordinated sequences with other UI elements
- Dramatic effect or emphasis

**Staggered Progress Bars:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="80" Height="60">
    <ProgressBarAnimation Enable="true" Duration="2000" Delay="0">
    </ProgressBarAnimation>
</SfProgressBar>

<SfProgressBar Value="60" Height="60">
    <ProgressBarAnimation Enable="true" Duration="2000" Delay="500">
    </ProgressBarAnimation>
</SfProgressBar>

<SfProgressBar Value="90" Height="60">
    <ProgressBarAnimation Enable="true" Duration="2000" Delay="1000">
    </ProgressBarAnimation>
</SfProgressBar>
```
**Result:** Three progress bars animate in sequence with 500ms between each.

## Animation with Different States

### Determinate Animation

For determinate progress (known percentage), animation smoothly transitions from the current value to the new value using the `Value` property on `<SfProgressBar>` with `<ProgressBarAnimation>` enabled:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="@progressValue" 
               Height="60"
               ShowProgressValue="true"
               TrackThickness="20"
               ProgressThickness="20"
               ProgressColor="#0d6efd"
               TrackColor="#e9ecef">
    <ProgressBarAnimation Enable="true" Duration="1000"></ProgressBarAnimation>
</SfProgressBar>

<button class="btn btn-primary" @onclick="UpdateProgress">Update Progress</button>

@code {
    private double progressValue = 25;
    
    private void UpdateProgress()
    {
        progressValue += 15;
        if (progressValue > 100) progressValue = 0;
    }
}
```

**Behavior:**
- Progress animates from current value (25%) to new value (40%)
- Animation takes 1000ms (1 second)
- Smooth, linear transition

### Indeterminate Animation

For indeterminate progress (unknown duration), animation provides continuous movement using the `IsIndeterminate` property set to `true` with `<ProgressBarAnimation>` enabled:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="20" 
               Height="60" 
               IsIndeterminate="true"
               TrackThickness="16"
               ProgressThickness="16"
               ProgressColor="#6f42c1"
               TrackColor="#e9ecef">
    <ProgressBarAnimation Enable="true" Duration="2000"></ProgressBarAnimation>
</SfProgressBar>
```

**Behavior:**
- Continuous, repeating animation
- Progress indicator moves across the bar repeatedly
- Duration controls the speed of each animation cycle
- **Must** have `Enable="true"` for indeterminate state to animate

**Circular Indeterminate (Loading Spinner):**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="20" 
               Height="100px"
               Width="100px"
               IsIndeterminate="true"
               TrackThickness="8"
               ProgressThickness="8"
               ProgressColor="#0d6efd"
               TrackColor="#e9ecef">
    <ProgressBarAnimation Enable="true" Duration="1500"></ProgressBarAnimation>
</SfProgressBar>
```

**Behavior:**
- Rotating arc/spinner
- Duration controls rotation speed
- Faster duration (1000-1500ms) for active feel
- Slower duration (2500-3000ms) for subtle loading

### Active State Animation

The `IsActive` property adds flowing animation to determinate progress, and can be combined with the `IsStriped` property for striped animation effects:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               IsActive="true" 
               Value="60" 
               Height="60"
               TrackThickness="20"
               ProgressThickness="20"
               ProgressColor="#28a745"
               TrackColor="#e9ecef">
    <ProgressBarAnimation Enable="true" Duration="1500"></ProgressBarAnimation>
</SfProgressBar>
```

**Behavior:**
- Progress fills to the value (60%)
- Flowing/shimmering effect over the filled portion
- Indicates active, ongoing operation
- Combine with `IsStriped="true"` for striped + active animation (Linear only)

**Active + Striped:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               IsActive="true" 
               IsStriped="true"
               Value="70" 
               Height="60"
               TrackThickness="20"
               ProgressThickness="20"
               ProgressColor="#ffc107"
               TrackColor="#e9ecef">
    <ProgressBarAnimation Enable="true" Duration="1000"></ProgressBarAnimation>
</SfProgressBar>
```

**Result:** Animated striped pattern that moves/flows, creating strong visual activity.

## Animation Events

### AnimationComplete Event

The `AnimationComplete` event on `<ProgressBarEvents>` fires when the animation finishes, providing access to the animation state through event arguments:

**Basic Usage:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="100" 
               Height="60">
    <ProgressBarAnimation Enable="true" Duration="2000"></ProgressBarAnimation>
    <ProgressBarEvents AnimationComplete="@OnAnimationComplete"></ProgressBarEvents>
</SfProgressBar>

<p>@statusMessage</p>

@code {
    private string statusMessage = "";
    
    private void OnAnimationComplete(ProgressValueEventArgs args)
    {
        statusMessage = $"Animation completed at value: {args.Value}";
    }
}
```

**Event Arguments:**
- `args.Value` - The value when animation completed

**Use Cases:**
- Show completion message
- Hide progress bar after animation
- Trigger next step in a workflow
- Play sound or show notification
- Update UI state

**Hide Progress Bar After Completion:**
```razor
<SfProgressBar Type="ProgressType.Linear" 
               Value="@progressValue" 
               Height="60" 
               Visible="@showProgressBar"
               TrackThickness="16"
               ProgressThickness="16">
    <ProgressBarAnimation Enable="true" Duration="2000"></ProgressBarAnimation>
    <ProgressBarEvents AnimationComplete="@OnAnimationComplete"></ProgressBarEvents>
</SfProgressBar>

<p style="color: green; font-weight: bold;">@completionMessage</p>

<button class="btn btn-primary" @onclick="StartProcess">Start Process</button>

@code {
    private double progressValue = 0;
    private bool showProgressBar = false;
    private string completionMessage = "";
    
    private void StartProcess()
    {
        showProgressBar = true;
        progressValue = 100;
        completionMessage = "";
    }
    
    private void OnAnimationComplete(ProgressValueEventArgs args)
    {
        if (args.Value == 100)
        {
            Task.Delay(500).ContinueWith(_ =>
            {
                InvokeAsync(() =>
                {
                    showProgressBar = false;
                    completionMessage = "✓ Process completed successfully!";
                    StateHasChanged();
                });
            });
        }
    }
}
```

### Coordinating with Other Events

Combine `AnimationComplete` with other `<ProgressBarEvents>` like `ProgressCompleted` for complex workflows:

**With ProgressCompleted:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="@progress" Height="60">
    <ProgressBarAnimation Enable="true" Duration="2000"></ProgressBarAnimation>
    <ProgressBarEvents 
        ProgressCompleted="@OnProgressCompleted"
        AnimationComplete="@OnAnimationComplete">
    </ProgressBarEvents>
</SfProgressBar>

@code {
    private double progress = 100;
    
    private void OnProgressCompleted(ProgressValueEventArgs args)
    {
        // Fires when progress reaches maximum (100)
        Console.WriteLine("Progress reached 100%");
    }
    
    private void OnAnimationComplete(ProgressValueEventArgs args)
    {
        // Fires when animation finishes
        Console.WriteLine("Animation finished");
        // If progress is 100, both events fire
    }
}
```

**Multi-Stage Process:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="@currentProgress" Height="60" ShowProgressValue="true">
    <ProgressBarAnimation Enable="true" Duration="1500"></ProgressBarAnimation>
    <ProgressBarEvents AnimationComplete="@OnStageComplete"></ProgressBarEvents>
</SfProgressBar>

<p>@statusMessage</p>

@code {
    private double currentProgress = 0;
    private string statusMessage = "Ready to start";
    private int stage = 0;
    
    protected override void OnInitialized()
    {
        StartProcess();
    }
    
    private void StartProcess()
    {
        stage = 1;
        currentProgress = 33;
        statusMessage = "Stage 1: Initializing...";
    }
    
    private void OnStageComplete(ProgressValueEventArgs args)
    {
        stage++;
        
        switch (stage)
        {
            case 2:
                currentProgress = 66;
                statusMessage = "Stage 2: Processing...";
                break;
            case 3:
                currentProgress = 100;
                statusMessage = "Stage 3: Finalizing...";
                break;
            case 4:
                statusMessage = "Complete!";
                break;
        }
    }
}
```

## Performance Considerations

### Animation Performance Tips

The `Duration` and `Delay` properties on `<ProgressBarAnimation>` significantly impact performance and should be selected carefully.

**1. Duration Selection:**
- **Short animations (300-500ms):** Better for frequent updates but may feel jarring
- **Medium animations (1000-2000ms):** Good balance for most scenarios
- **Long animations (3000ms+):** Use sparingly, can feel sluggish

**2. Update Frequency:**
```razor
@using Syncfusion.Blazor.ProgressBar

<!-- Good: Moderate update frequency -->
<SfProgressBar Value="@progress" Height="60">
    <ProgressBarAnimation Enable="true" Duration="500"></ProgressBarAnimation>
</SfProgressBar>

@code {
    private System.Threading.Timer? timer;
    private double progress = 0;
    
    protected override void OnInitialized()
    {
        // Update every 500ms matches animation duration
        timer = new System.Threading.Timer(_ =>
        {
            if (progress < 100)
            {
                progress += 10;
                InvokeAsync(StateHasChanged);
            }
        }, null, 0, 500);
    }
}
```

**3. Disable Animation for Rapid Updates:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="@progress" Height="60">
    <ProgressBarAnimation Enable="@enableAnimation"></ProgressBarAnimation>
</SfProgressBar>

@code {
    private double progress = 80;
    private bool enableAnimation = true;
    
    private void RapidUpdate()
    {
        // Disable animation for rapid, small changes
        enableAnimation = false;
        progress = 75;
        StateHasChanged();
        
        // Re-enable after stable
        Task.Delay(1000).ContinueWith(_ =>
        {
            InvokeAsync(() =>
            {
                enableAnimation = true;
                StateHasChanged();
            });
        });
    }
}
```

**4. Indeterminate Animation:**
- Uses continuous animation, minimal performance impact
- Avoid multiple indeterminate progress bars on same page
- Use shorter duration (1000-2000ms) to reduce animation overhead

**5. Mobile Devices:**
- Test animations on slower devices
- Consider slightly longer durations (add 500ms) for smoother appearance
- Avoid very fast animations (<500ms) that may stutter

### When to Disable Animation

Set the `Enable` property to `false` on `<ProgressBarAnimation>` when:

**Disable animation when:**
- User has motion sensitivity preferences
- Very frequent updates (multiple times per second)
- Low-end devices or poor performance detected
- User explicitly disables animations in settings

**Respect Motion Preferences:**
```razor
@using Syncfusion.Blazor.ProgressBar
@using Microsoft.JSInterop

<SfProgressBar Value="@progress" Height="60">
    <ProgressBarAnimation Enable="@animationEnabled"></ProgressBarAnimation>
</SfProgressBar>

@code {
    [Inject] private IJSRuntime JS { get; set; }
    private double progress = 50;
    private bool animationEnabled = true;
    
    protected override async Task OnInitializedAsync()
    {
        // Check for reduced motion preference
        var prefersReducedMotion = await JS.InvokeAsync<bool>(
            "matchMedia", "(prefers-reduced-motion: reduce)");
        
        if (prefersReducedMotion)
        {
            animationEnabled = false;
        }
    }
}
```

## Complete Examples

**Example 1: Smooth File Upload**
```razor
@using Syncfusion.Blazor.ProgressBar

<div style="padding: 20px;">
    <h4>Uploading: report.pdf</h4>
    
    <SfProgressBar Type="ProgressType.Linear" 
                   Value="@uploadProgress" 
                   Height="60"
                   ShowProgressValue="true"
                   TrackThickness="20"
                   ProgressThickness="20"
                   CornerRadius="CornerType.Round"
                   ProgressColor="#28a745"
                   TrackColor="#e9ecef">
        <ProgressBarAnimation Enable="true" Duration="800"></ProgressBarAnimation>
        <ProgressBarEvents AnimationComplete="@CheckCompletion"></ProgressBarEvents>
    </SfProgressBar>
    
    <p style="margin-top: 10px;">@statusMessage</p>
    <button class="btn btn-primary" @onclick="SimulateUpload" disabled="@isUploading">
        Start Upload
    </button>
</div>

@code {
    private double uploadProgress = 0;
    private bool isUploading = false;
    private string statusMessage = "Ready to upload";
    private System.Threading.Timer? timer;
    
    private void SimulateUpload()
    {
        isUploading = true;
        uploadProgress = 0;
        statusMessage = "Uploading...";
        
        timer = new System.Threading.Timer(_ =>
        {
            if (uploadProgress < 100)
            {
                uploadProgress += 10;
                InvokeAsync(StateHasChanged);
            }
            else
            {
                timer?.Dispose();
            }
        }, null, 0, 800);
    }
    
    private void CheckCompletion(ProgressValueEventArgs args)
    {
        if (args.Value >= 100)
        {
            statusMessage = "✓ Upload complete!";
            isUploading = false;
            StateHasChanged();
        }
    }
    
    public void Dispose()
    {
        timer?.Dispose();
    }
}
```

**Example 2: Loading Spinner**
```razor
@using Syncfusion.Blazor.ProgressBar

<div style="display: flex; justify-content: center; align-items: center; height: 300px;">
    <div style="text-align: center;">
        <SfProgressBar Type="ProgressType.Circular" 
                       Value="20" 
                       Height="120px"
                       Width="120px"
                       IsIndeterminate="true"
                       TrackThickness="8"
                       ProgressThickness="8"
                       ProgressColor="#0d6efd"
                       TrackColor="#e9ecef">
            <ProgressBarAnimation Enable="true" Duration="1500"></ProgressBarAnimation>
        </SfProgressBar>
        
        <p style="margin-top: 20px; font-size: 18px; color: #666;">
            Loading, please wait...
        </p>
    </div>
</div>
```

**Example 3: Multi-Stage with Different Durations**
```razor
@using Syncfusion.Blazor.ProgressBar

<div style="padding: 20px;">
    <h4>@stageName</h4>
    
    <SfProgressBar Value="@stageProgress" 
                   Height="60"
                   ShowProgressValue="true"
                   TrackThickness="18"
                   ProgressThickness="18"
                   ProgressColor="@stageColor">
        <ProgressBarAnimation Enable="true" Duration="@animationDuration">
        </ProgressBarAnimation>
        <ProgressBarEvents AnimationComplete="@OnStageAnimationComplete">
        </ProgressBarEvents>
    </SfProgressBar>
</div>

@code {
    private double stageProgress = 0;
    private int animationDuration = 2000;
    private string stageName = "Stage 1: Quick initialization";
    private string stageColor = "#0d6efd";
    
    protected override void OnInitialized()
    {
        StartStage1();
    }
    
    private void StartStage1()
    {
        stageName = "Stage 1: Quick initialization";
        stageProgress = 100;
        animationDuration = 1000; // Fast
        stageColor = "#0d6efd";
    }
    
    private void OnStageAnimationComplete(ProgressValueEventArgs args)
    {
        if (stageName.Contains("Stage 1"))
        {
            Task.Delay(500).ContinueWith(_ =>
            {
                InvokeAsync(() =>
                {
                    stageName = "Stage 2: Processing data";
                    stageProgress = 100;
                    animationDuration = 3000; // Slow for emphasis
                    stageColor = "#ffc107";
                    StateHasChanged();
                });
            });
        }
        else if (stageName.Contains("Stage 2"))
        {
            Task.Delay(500).ContinueWith(_ =>
            {
                InvokeAsync(() =>
                {
                    stageName = "Stage 3: Finalizing";
                    stageProgress = 100;
                    animationDuration = 1500; // Medium
                    stageColor = "#28a745";
                    StateHasChanged();
                });
            });
        }
    }
}
```

**Example 4: Smooth Value Updates**
```razor
@using Syncfusion.Blazor.ProgressBar

<div style="padding: 20px;">
    <SfProgressBar Value="@targetProgress" 
                   Height="60"
                   ShowProgressValue="true"
                   TrackThickness="20"
                   ProgressThickness="20"
                   CornerRadius="CornerType.Round">
        <ProgressBarAnimation Enable="true" Duration="1200"></ProgressBarAnimation>
    </SfProgressBar>
    
    <div style="margin-top: 20px;">
        <button class="btn btn-secondary" @onclick="() => SetProgress(25)">25%</button>
        <button class="btn btn-secondary" @onclick="() => SetProgress(50)">50%</button>
        <button class="btn btn-secondary" @onclick="() => SetProgress(75)">75%</button>
        <button class="btn btn-secondary" @onclick="() => SetProgress(100)">100%</button>
    </div>
</div>

@code {
    private double targetProgress = 0;
    
    private void SetProgress(double value)
    {
        targetProgress = value;
    }
}
```

Animation is a powerful feature that enhances user experience when properly configured. Match animation duration to the frequency and magnitude of progress updates for optimal results.
