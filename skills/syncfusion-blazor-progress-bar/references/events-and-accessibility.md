# Events and Accessibility

## Table of Contents
- [Events Overview](#events-overview)
- [ValueChanged Event](#valuechanged-event)
  - [When It Triggers](#when-it-triggers)
  - [Implementation](#implementation)
  - [Use Cases](#use-cases)
- [ProgressCompleted Event](#progresscompleted-event)
  - [When It Triggers](#when-it-triggers-1)
  - [Implementation](#implementation-1)
  - [Use Cases](#use-cases-1)
- [AnimationComplete Event](#animationcomplete-event)
  - [When It Triggers](#when-it-triggers-2)
  - [Implementation](#implementation-2)
  - [Coordinating Animations](#coordinating-animations)
- [AnnotationRender Event](#annotationrender-event)
  - [Customizing Annotations](#customizing-annotations)
  - [Implementation](#implementation-3)
- [TextRender Event](#textrender-event)
  - [Custom Label Formatting](#custom-label-formatting)
  - [Implementation](#implementation-4)
- [Loaded Event](#loaded-event)
  - [Component Initialization](#component-initialization)
  - [Implementation](#implementation-5)
- [Event Coordination Examples](#event-coordination-examples)
- [Accessibility Overview](#accessibility-overview)
  - [WCAG 2.2 Compliance](#wcag-22-compliance)
  - [Section 508 Support](#section-508-support)
  - [Accessibility Standards Table](#accessibility-standards-table)
- [Keyboard Navigation](#keyboard-navigation)
  - [Tab Navigation](#tab-navigation)
  - [Print Support](#print-support)
  - [Keyboard Shortcuts](#keyboard-shortcuts)
- [Screen Reader Support](#screen-reader-support)
- [RTL Support for Accessibility](#rtl-support-for-accessibility)
- [Color Contrast](#color-contrast)
- [Mobile Device Support](#mobile-device-support)
- [Ensuring Accessibility](#ensuring-accessibility)
  - [Axe-core Testing](#axe-core-testing)
  - [Accessibility Demo Link](#accessibility-demo-link)
- [Best Practices](#best-practices)

## Events Overview

The Blazor ProgressBar component provides several events to handle various lifecycle stages and user interactions. Events are configured using the `<ProgressBarEvents>` child component.

**Available Events:**
- **ValueChanged** - Fires when the `Value` property changes, triggered during any progress update
- **ProgressCompleted** - Fires when the `Value` property reaches the `Maximum` value
- **AnimationComplete** - Fires when the `ProgressBarAnimation` finishes, after animation duration expires
- **AnnotationRender** - Fires before annotations are rendered, allows modification through the annotation content template
- **TextRender** - Fires before text labels are rendered, allows custom text formatting through `TextRenderEventArgs`
- **Loaded** - Fires when component initialization is complete, component reference is ready for interaction

**Event Configuration Pattern:**
```razor
<SfProgressBar ...>
    <ProgressBarEvents 
        ValueChanged="@OnValueChanged"
        ProgressCompleted="@OnCompleted"
        AnimationComplete="@OnAnimationComplete"
        TextRender="@OnTextRender"
        AnnotationRender="@OnAnnotationRender"
        Loaded="@OnLoaded">
    </ProgressBarEvents>
</SfProgressBar>
```

## ValueChanged Event

### When It Triggers

The `ValueChanged` event fires whenever the `Value` property of the ProgressBar changes, whether programmatically or through binding updates. This event is handled by the `ValueChanged` handler in the `<ProgressBarEvents>` component.

**Trigger Conditions:**
- `Value` property is updated to a new value
- Fires for every value change, not just on completion, enabling real-time progress tracking
- Fires before rendering the new value on the UI

### Implementation

**Basic Usage:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="@currentValue" 
               Height="60">
    <ProgressBarEvents ValueChanged="@OnValueChanged"></ProgressBarEvents>
</SfProgressBar>

<p>Current value: @currentValue</p>
<p>Last change: @lastChangeMessage</p>

<button class="btn btn-primary" @onclick="IncrementValue">Increment</button>

@code {
    private double currentValue = 0;
    private string lastChangeMessage = "";
    
    private void OnValueChanged(ProgressValueEventArgs args)
    {
        lastChangeMessage = $"Value changed to: {args.Value} at {DateTime.Now:HH:mm:ss}";
    }
    
    private void IncrementValue()
    {
        currentValue += 10;
        if (currentValue > 100) currentValue = 0;
    }
}
```

**Event Arguments (ProgressValueEventArgs):**
- `args.Value` - The new progress value being set, accessible in the ValueChanged handler

### Use Cases

**1. Logging Progress Changes:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="@progress" Height="60">
    <ProgressBarEvents ValueChanged="@LogProgress"></ProgressBarEvents>
</SfProgressBar>

@code {
    private double progress = 0;
    private List<string> progressLog = new();
    
    private void LogProgress(ProgressValueEventArgs args)
    {
        progressLog.Add($"Progress: {args.Value}% at {DateTime.Now:HH:mm:ss}");
        Console.WriteLine($"Progress updated to {args.Value}%");
    }
}
```

**2. Triggering Side Effects:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="@downloadProgress" Height="60">
    <ProgressBarEvents ValueChanged="@OnDownloadProgress"></ProgressBarEvents>
</SfProgressBar>

<p>@statusMessage</p>

@code {
    private double downloadProgress = 55;
    private string statusMessage = "";
    
    private void OnDownloadProgress(ProgressValueEventArgs args)
    {
        if (args.Value >= 50 && args.Value < 60)
        {
            statusMessage = "Download is halfway complete!";
        }
        else if (args.Value >= 90)
        {
            statusMessage = "Almost done...";
        }
    }
}
```

**3. Updating Related UI:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="@taskProgress" Height="60">
    <ProgressBarEvents ValueChanged="@UpdateTaskStatus"></ProgressBarEvents>
</SfProgressBar>

<p style="color: @statusColor">@taskStatus</p>

@code {
    private double taskProgress = 50;
    private string taskStatus = "Not started";
    private string statusColor = "gray";
    
    private void UpdateTaskStatus(ProgressValueEventArgs args)
    {
        (taskStatus, statusColor) = args.Value switch
        {
            < 25 => ("Starting...", "#6c757d"),
            < 50 => ("In progress...", "#0d6efd"),
            < 75 => ("Making progress...", "#0dcaf0"),
            < 100 => ("Nearly complete...", "#ffc107"),
            _ => ("Complete!", "#28a745")
        };
    }
}
```

## ProgressCompleted Event

### When It Triggers

The `ProgressCompleted` event fires when the progress value reaches the `Maximum` value (typically 100). This event is handled by the `ProgressCompleted` handler in the `<ProgressBarEvents>` component.

**Trigger Conditions:**
- `Value` property equals or exceeds the `Maximum` property value
- Fires only once when reaching maximum, making it ideal for completion-based logic
- Does not fire for intermediate values, providing definitive completion notification

### Implementation

**Basic Usage:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="@progressValue" 
               Height="60">
    <ProgressBarEvents ProgressCompleted="@OnProgressCompleted"></ProgressBarEvents>
</SfProgressBar>

<p>@completionMessage</p>

<button class="btn btn-primary" @onclick="CompleteProgress">Complete</button>

@code {
    private double progressValue = 80;
    private string completionMessage = "";
    
    private void OnProgressCompleted(ProgressValueEventArgs args)
    {
        completionMessage = $"Progress completed at {DateTime.Now:HH:mm:ss}!";
    }
    
    private void CompleteProgress()
    {
        progressValue = 100;
    }
}
```

**Event Arguments (ProgressValueEventArgs):**
- `args.Value` - The value when completion occurs (equals Maximum property, typically 100)

### Use Cases

**1. Show Success Message:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="@uploadProgress" Height="60" Visible="@showProgress">
    <ProgressBarEvents ProgressCompleted="@OnUploadComplete"></ProgressBarEvents>
</SfProgressBar>

<div style="color: green; font-weight: bold;">@successMessage</div>

@code {
    private double uploadProgress = 100;
    private bool showProgress = true;
    private string successMessage = "";
    
    private void OnUploadComplete(ProgressValueEventArgs args)
    {
        showProgress = false;
        successMessage = "✓ Upload completed successfully!";
    }
}
```

**2. Trigger Next Step:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="@currentStepProgress" Height="60">
    <ProgressBarEvents ProgressCompleted="@OnStepComplete"></ProgressBarEvents>
</SfProgressBar>

<p>Step @currentStep of 3: @stepName</p>

@code {
    private double currentStepProgress = 0;
    private int currentStep = 1;
    private string stepName = "Initializing";
    
    private void OnStepComplete(ProgressValueEventArgs args)
    {
        currentStep++;
        currentStepProgress = 0;
        
        stepName = currentStep switch
        {
            2 => "Processing",
            3 => "Finalizing",
            _ => "Complete"
        };
        
        if (currentStep <= 3)
        {
            // Start next step
            SimulateNextStep();
        }
    }
    
    private void SimulateNextStep()
    {
        // Simulate progress for next step
    }
}
```

**3. Play Sound or Notification:**
```razor
@using Syncfusion.Blazor.ProgressBar
@inject IJSRuntime JS

<SfProgressBar Value="@progress" Height="60">
    <ProgressBarEvents ProgressCompleted="@OnComplete"></ProgressBarEvents>
</SfProgressBar>

@code {
    private double progress = 0;
    
    private async Task OnComplete(ProgressValueEventArgs args)
    {
        // Play completion sound
        await JS.InvokeVoidAsync("playCompletionSound");
        
        // Show browser notification
        await JS.InvokeVoidAsync("showNotification", "Task Complete", "Your operation has finished successfully.");
    }
}
```

## AnimationComplete Event

### When It Triggers

The `AnimationComplete` event fires when an animation finishes, only when animation is enabled. This event is handled by the `AnimationComplete` handler in the `<ProgressBarEvents>` component.

**Trigger Conditions:**
- `Enable` property in `<ProgressBarAnimation>` is set to `true`
- Progress animation sequence completes based on the `Duration` property
- Fires after value reaches target and animation duration expires, enabling sequential animations

### Implementation

**Basic Usage:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="@progressValue" 
               Height="60">
    <ProgressBarAnimation Enable="true" Duration="2000"></ProgressBarAnimation>
    <ProgressBarEvents AnimationComplete="@OnAnimationComplete"></ProgressBarEvents>
</SfProgressBar>

<p>@animationStatus</p>

@code {
    private double progressValue = 50;
    private string animationStatus = "Ready";
    
    private void OnAnimationComplete(ProgressValueEventArgs args)
    {
        animationStatus = $"Animation completed at value {args.Value}";
    }
}
```

**Event Arguments (ProgressValueEventArgs):**
- `args.Value` - The value when animation completed, reflects final progress position

### Coordinating Animations

**Hide Progress After Animation:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="@progress" 
               Height="60" 
               Visible="@showProgressBar"
               ShowProgressValue="true">
    <ProgressBarAnimation Enable="true" Duration="2000"></ProgressBarAnimation>
    <ProgressBarEvents AnimationComplete="@HideProgress"></ProgressBarEvents>
</SfProgressBar>

<p style="color: green; font-weight: bold;">@message</p>

<button class="btn btn-primary" @onclick="StartProcess">Start</button>

@code {
    private double progress = 0;
    private bool showProgressBar = false;
    private string message = "";
    
    private void StartProcess()
    {
        showProgressBar = true;
        progress = 100;
        message = "";
    }
    
    private void HideProgress(ProgressValueEventArgs args)
    {
        // Wait a moment, then hide
        Task.Delay(1000).ContinueWith(_ =>
        {
            InvokeAsync(() =>
            {
                showProgressBar = false;
                message = "Process complete!";
                StateHasChanged();
            });
        });
    }
}
```

**Sequential Operations:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="@stage1Progress" Height="60">
    <ProgressBarAnimation Enable="true" Duration="1500"></ProgressBarAnimation>
    <ProgressBarEvents AnimationComplete="@OnStage1Complete"></ProgressBarEvents>
</SfProgressBar>

<SfProgressBar Value="@stage2Progress" Height="60">
    <ProgressBarAnimation Enable="true" Duration="1500"></ProgressBarAnimation>
    <ProgressBarEvents AnimationComplete="@OnStage2Complete"></ProgressBarEvents>
</SfProgressBar>

@code {
    private double stage1Progress = 0;
    private double stage2Progress = 0;
    
    protected override void OnInitialized()
    {
        stage1Progress = 100; // Start first stage
    }
    
    private void OnStage1Complete(ProgressValueEventArgs args)
    {
        // Start second stage after first completes
        Task.Delay(500).ContinueWith(_ =>
        {
            InvokeAsync(() =>
            {
                stage2Progress = 100;
                StateHasChanged();
            });
        });
    }
    
    private void OnStage2Complete(ProgressValueEventArgs args)
    {
        // All stages complete
        Console.WriteLine("All operations complete!");
    }
}
```

## AnnotationRender Event

### Customizing Annotations

The `AnnotationRender` event fires before each annotation is rendered, allowing dynamic customization through the `AnnotationRender` handler. This event enables modification of annotation content before it appears on the component.

### Implementation

**Basic Usage:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="80" 
               Height="160" 
               Width="160">
    <ProgressBarEvents AnnotationRender="@OnAnnotationRender"></ProgressBarEvents>
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="font-size:20px; font-weight:bold; color:#000000;">
                    <span>80%</span>
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>

@code {
    private void OnAnnotationRender(AnnotationRenderEventArgs args)
    {
        // Perform actions before annotation renders
        Console.WriteLine("Annotation is rendering");
    }
}
```

**Event Arguments (AnnotationRenderEventArgs):**
- `args.Content` - Annotation content from `<ProgressBarAnnotation>` child element (can be modified)
- Allows customization of content template before rendering on the component

**Use Case - Dynamic Styling:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="@progressValue" 
               Height="160" 
               Width="160">
    <ProgressBarEvents AnnotationRender="@CustomizeAnnotation"></ProgressBarEvents>
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="font-size:20px; font-weight:bold; color:@annotationColor;">
                    <span>@progressValue%</span>
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>

@code {
    private double progressValue = 75;
    private string annotationColor = "#000000";
    
    private void CustomizeAnnotation(AnnotationRenderEventArgs args)
    {
        // Change color based on progress
        annotationColor = progressValue switch
        {
            (>= 75) => "#28a745",
            (>= 50) => "#ffc107",
            (>= 25) => "#0d6efd",
            _ => "#dc3545"
        };
    }
}
```

## TextRender Event

### Custom Label Formatting

The `TextRender` event fires before text labels are rendered, allowing custom formatting of the progress percentage. This event is handled by the `TextRender` handler and enables modification of displayed text through the `Text` property.

### Implementation

**Basic Usage:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               ShowProgressValue="true" 
               Value="50" 
               Height="60">
    <ProgressBarEvents TextRender="@OnTextRender"></ProgressBarEvents>
</SfProgressBar>

@code {
    private void OnTextRender(TextRenderEventArgs args)
    {
        // Customize the text displayed
        args.Text = $"Progress: {args.Text}";
    }
}
```

**Event Arguments (TextRenderEventArgs):**
- `args.Text` - Current text being rendered (can be modified), typically the percentage when `ShowProgressValue="true"`

**Custom Formats:**

**Fraction Format:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar ShowProgressValue="true" 
               Value="@current" 
               Maximum="@total"
               Height="60">
    <ProgressBarEvents TextRender="@FractionFormat"></ProgressBarEvents>
</SfProgressBar>

@code {
    private double current = 45;
    private double total = 100;
    
    private void FractionFormat(TextRenderEventArgs args)
    {
        args.Text = $"{current}/{total}";
    }
}
```

**Time Remaining:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar ShowProgressValue="true" 
               Value="@progress" 
               Height="60">
    <ProgressBarEvents TextRender="@TimeRemainingFormat"></ProgressBarEvents>
</SfProgressBar>

@code {
    private double progress = 60;
    
    private void TimeRemainingFormat(TextRenderEventArgs args)
    {
        var text = args.Text.Replace("%", ""); // remove percent symbol if exists
        int value = int.TryParse(text, out var parsed) ? parsed : 0;
        var remainingPercent = 100 - value;
        var estimatedSeconds = remainingPercent * 0.5; // Example calculation
        args.Text = $"{value}% (~{estimatedSeconds:F0}s remaining)";
    }
}
```

**Conditional Messages:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar ShowProgressValue="true" 
               Value="@progress" 
               Height="60">
    <ProgressBarEvents TextRender="@ConditionalMessage"></ProgressBarEvents>
</SfProgressBar>

@code {
    private double progress = 95;
    
    private void ConditionalMessage(TextRenderEventArgs args)
    {
        var text = args.Text.Replace("%", ""); // remove percent symbol 
        int.TryParse(text, out var value);
        args.Text = value switch
        {
            (100) => "✓ Complete!",
            (>= 90) => "Almost there...",
            (>= 50) => $"{value}% - Halfway done",
            _ => $"{value}%"
        };
    }
}
```

## Loaded Event

### Component Initialization

The `Loaded` event fires after the component is fully rendered and loaded. This event is handled by the `Loaded` handler in the `<ProgressBarEvents>` component and indicates the component is ready for programmatic interaction.

### Implementation

**Basic Usage:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="100" 
               Height="60">
    <ProgressBarEvents Loaded="@OnLoaded"></ProgressBarEvents>
</SfProgressBar>

@code {
    private void OnLoaded(System.EventArgs args)
    {
        Console.WriteLine("ProgressBar component has loaded");
        // Perform initialization tasks
    }
}
```

**Use Cases:**

**Start Progress After Load:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="@progress" Height="60">
    <ProgressBarAnimation Enable="true" Duration="300"></ProgressBarAnimation>
    <ProgressBarEvents Loaded="@StartProgress"></ProgressBarEvents>
</SfProgressBar>

@code {
    private double progress = 0;
    private System.Threading.Timer? timer;
    
    private void StartProgress(System.EventArgs args)
    {
        // Start progress simulation once component is loaded
        timer = new System.Threading.Timer(_ =>
        {
            if (progress < 100)
            {
                progress += 5;
                InvokeAsync(StateHasChanged);
            }
            else
            {
                timer?.Dispose();
            }
        }, null, 1000, 500);
    }
    
    public void Dispose()
    {
        timer?.Dispose();
    }
}
```

## Event Coordination Examples

**Complete Workflow with Multiple Events:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="@progress" 
               Height="60"
               ShowProgressValue="true"
               Visible="@showProgress">
    <ProgressBarAnimation Enable="true" Duration="1000"></ProgressBarAnimation>
    <ProgressBarEvents 
        Loaded="@OnLoaded"
        ValueChanged="@OnValueChanged"
        ProgressCompleted="@OnProgressCompleted"
        AnimationComplete="@OnAnimationComplete"
        TextRender="@OnTextRender">
    </ProgressBarEvents>
</SfProgressBar>

<p>Status: @status</p>
<button class="btn btn-primary" @onclick="StartOperation">Start</button>

@code {
    private double progress = 0;
    private bool showProgress = false;
    private string status = "Ready";
    private System.Threading.Timer? timer;
    
    private void OnLoaded(System.EventArgs args)
    {
        status = "Component loaded";
    }
    
    private void OnValueChanged(ProgressValueEventArgs args)
    {
        status = $"Processing: {args.Value}%";
    }
    
    private void OnProgressCompleted(ProgressValueEventArgs args)
    {
        status = "Progress reached 100%";
    }
    
    private void OnAnimationComplete(ProgressValueEventArgs args)
    {
        if (args.Value >= 100)
        {
            Task.Delay(1000).ContinueWith(_ =>
            {
                InvokeAsync(() =>
                {
                    showProgress = false;
                    status = "Operation complete!";
                    StateHasChanged();
                });
            });
        }
    }
    
    private void OnTextRender('Syncfusion.Blazor.ProgressBar.TextRenderEventArgs args)
    {
        var text = args.Text.Replace("%", ""); // remove percent symbol 
        int.TryParse(text, out var value);
        if (value >= 100)
        {
            args.Text = "Done!";
        }
    }
    
    private void StartOperation()
    {
        showProgress = true;
        progress = 0;
        status = "Starting...";
        
        timer = new System.Threading.Timer(_ =>
        {
            if (progress < 100)
            {
                progress += 10;
                InvokeAsync(StateHasChanged);
            }
            else
            {
                timer?.Dispose();
            }
        }, null, 500, 500);
    }
    
    public void Dispose()
    {
        timer?.Dispose();
    }
}
```

## Accessibility Overview

The Syncfusion Blazor ProgressBar component follows accessibility guidelines and standards to ensure usability for all users, including those using assistive technologies.

### WCAG 2.2 Compliance

The component follows **Web Content Accessibility Guidelines (WCAG) 2.2** at Level AA, ensuring:
- Perceivable: Information is presentable to users in ways they can perceive
- Operable: Interface components are operable
- Understandable: Information and operation are understandable
- Robust: Content works with current and future assistive technologies

### Section 508 Support

The ProgressBar component complies with **Section 508** of the Rehabilitation Act, making it accessible for federal agencies and organizations.

### Accessibility Standards Table

| Accessibility Criteria | Compatibility |
| --- | --- |
| WCAG 2.2 Support | AA |
| Section 508 Support | ✓ Yes |
| Screen Reader Support | ⚠ Intermediate |
| Right-To-Left Support | ✓ Yes |
| Color Contrast | ✓ Yes |
| Mobile Device Support | ✓ Yes |
| Keyboard Navigation Support | ⚠ Intermediate |
| Axe-core Accessibility Validation | ✓ Yes |

**Legend:**
- ✓ **Yes** - All features meet the requirement
- ⚠ **Intermediate** - Some features meet the requirement
- ✗ **No** - Does not meet the requirement

## Keyboard Navigation

### Tab Navigation

The ProgressBar supports keyboard focus and navigation:

**Tab Key:**
- Press <kbd>Tab</kbd> to move focus to the ProgressBar element
- Focus indicator appears on the component
- Screen readers announce the progress value

**Implementation:**
```razor
<SfProgressBar Value="75" 
               Height="60"
               ShowProgressValue="true"
               TrackThickness="16"
               ProgressThickness="16">
</SfProgressBar>
```

The component is automatically keyboard accessible with standard tab navigation.

### Print Support

**Ctrl+P (Windows) / ⌘+P (Mac):**
- Prints the ProgressBar component
- Useful for generating reports with progress indicators

### Keyboard Shortcuts

| Windows | Mac | Description |
| --- | --- | --- |
| <kbd>Tab</kbd> | <kbd>Tab</kbd> | Moves focus to the ProgressBar element |
| <kbd>Ctrl</kbd> + <kbd>P</kbd> | <kbd>⌘</kbd> + <kbd>P</kbd> | Prints the ProgressBar |

**Note:** The ProgressBar is primarily a visual indicator component and does not require extensive keyboard interaction beyond focus management.

## Screen Reader Support

The ProgressBar provides **intermediate screen reader support**:

**What's Announced:**
- Progress percentage when ShowProgressValue is enabled
- Progress state (indeterminate vs determinate)
- Completion status

**Best Practices for Screen Readers:**

**Add ARIA Labels:**
```razor
@using Syncfusion.Blazor.ProgressBar

<div role="progressbar" 
     aria-valuenow="@progress" 
     aria-valuemin="0" 
     aria-valuemax="100"
     aria-label="File upload progress">
    
    <SfProgressBar Value="@progress" 
                   Height="60"
                   ShowProgressValue="true">
    </SfProgressBar>
</div>

@code {
    private double progress = 65;
}
```

**Announce State Changes:**
```razor
@using Syncfusion.Blazor.ProgressBar

<div aria-live="polite" aria-atomic="true">
    @if (progress < 100)
    {
        <span class="sr-only">Uploading: @progress% complete</span>
    }
    else
    {
        <span class="sr-only">Upload complete</span>
    }
</div>

<SfProgressBar Value="@progress" Height="60">
</SfProgressBar>

@code {
    private double progress = 80;
}
```

**Screen Reader Only Text:**
```css
.sr-only {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0,0,0,0);
    white-space: nowrap;
    border-width: 0;
}
```

## RTL Support for Accessibility

The component supports right-to-left (RTL) languages for accessibility through the `EnableRtl` property:

**Enable RTL:**
```razor
<SfProgressBar EnableRtl="true" 
               Value="50" 
               Type="ProgressType.Linear"
               Height="60">
</SfProgressBar>
```

**Benefits:**
- Proper visual flow for RTL languages (Arabic, Hebrew) via `EnableRtl="true"` property
- Maintains logical reading order based on the `Direction` property setting
- Improves accessibility for RTL language users automatically

## Color Contrast

The ProgressBar ensures adequate color contrast for accessibility through the `ProgressColor` and `TrackColor` properties:

**WCAG AA Requirements:**
- Minimum 3:1 contrast ratio for large elements
- 4.5:1 contrast ratio for text

**Accessible Color Examples:**
```razor
<!-- High contrast: Dark progress on light background -->
<SfProgressBar Value="70" 
               ProgressColor="#1a1a1a" 
               TrackColor="#f0f0f0"
               Height="60">
</SfProgressBar>

<!-- Accessible semantic colors -->
<SfProgressBar Value="70" 
               ProgressColor="#28a745" 
               TrackColor="#d4edda"
               Height="60">
</SfProgressBar>
```

**Test Color Contrast:**
Use tools like:
- WebAIM Contrast Checker
- Browser DevTools accessibility features
- Axe DevTools extension

**Avoid:**
- Low contrast combinations (light blue on white)
- Relying solely on color to convey information
- Colors that are difficult for colorblind users

## Mobile Device Support

The ProgressBar is fully responsive and supports mobile devices:

**Touch-Friendly:**
- No interactive elements requiring precise touch
- Visual feedback works on all screen sizes
- Responsive sizing

**Mobile Considerations:**
```razor
<!-- Responsive circular progress -->
<SfProgressBar Type="ProgressType.Circular" 
               Value="80" 
               Height="120px"
               Width="120px"
               TrackThickness="10"
               ProgressThickness="10">
</SfProgressBar>

<!-- Responsive linear progress -->
<SfProgressBar Type="ProgressType.Linear" 
               Value="80" 
               Height="50"
               Width="100%"
               TrackThickness="12"
               ProgressThickness="12">
</SfProgressBar>
```

## Ensuring Accessibility

### Axe-core Testing

The component's accessibility is validated using **axe-core** with Playwright tests:

**What's Tested:**
- Color contrast ratios
- ARIA attributes
- Keyboard navigation
- Screen reader compatibility
- Focus management

### Accessibility Demo Link

View the component's accessibility features in action:
- [Blazor ProgressBar Accessibility Demo](https://blazor.syncfusion.com/accessibility/progress-bar)

**Demo Features:**
- Interactive accessibility testing
- Screen reader simulation
- Keyboard navigation demonstration
- Color contrast validation

## Best Practices

**1. Provide Context:**
```razor
<label id="upload-progress-label">File Upload Progress</label>
<div aria-labelledby="upload-progress-label">
    <SfProgressBar Value="@progress" Height="60" ShowProgressValue="true">
    </SfProgressBar>
</div>
```

**2. Announce Changes:**
```razor
@using Syncfusion.Blazor.ProgressBar

<div aria-live="polite" aria-atomic="true">
    <SfProgressBar Value="@progress" Height="60">
    </SfProgressBar>
    <span class="sr-only">@GetProgressMessage()</span>
</div>

@code {
    private double progress = 50;
    
    private string GetProgressMessage()
    {
        return progress < 100 
            ? $"{progress}% complete" 
            : "Operation complete";
    }
}
```

**3. Use Semantic Colors:**
- Green for success/completion
- Blue for normal progress
- Yellow/orange for warnings
- Red for errors or critical states

**4. Don't Rely Solely on Color:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="@progress" 
               ShowProgressValue="true"
               Height="60">
</SfProgressBar>
<p>Status: @GetStatusText()</p>

@code {
    private double progress = 90;
    
    private string GetStatusText()
    {
        return progress switch
        {
            (< 25) => "Just started",
            (< 50) => "In progress",
            (< 75) => "More than halfway",
            (< 100) => "Nearly complete",
            _ => "Complete"
        };
    }
}
```

**5. Test with Assistive Technologies:**
- NVDA (Windows screen reader)
- JAWS (Windows screen reader)
- VoiceOver (macOS/iOS screen reader)
- TalkBack (Android screen reader)

**6. Ensure Focus Visibility:**
```css
.e-progressbar:focus {
    outline: 2px solid #0d6efd;
    outline-offset: 2px;
}
```

**7. Support Reduced Motion:**
```razor
@using Syncfusion.Blazor.ProgressBar
@inject IJSRuntime JS

<SfProgressBar Value="@progress" Height="60">
    <ProgressBarAnimation Enable="@animationEnabled"></ProgressBarAnimation>
</SfProgressBar>

@code {
    private double progress = 50;
    private bool animationEnabled = true;
    
    protected override async Task OnInitializedAsync()
    {
        // Respect user's motion preferences
        var prefersReducedMotion = await JS.InvokeAsync<bool>(
            "window.matchMedia('(prefers-reduced-motion: reduce)').matches");
        
        if (prefersReducedMotion)
        {
            animationEnabled = false;
        }
    }
}
```

**8. Provide Text Alternatives:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="85" 
               Height="160px"
               Width="160px">
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div>
                    <i class="fas fa-check"></i>
                    <div>85%</div>
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>

<span class="sr-only">85% complete - Success</span>
```

By following these accessibility guidelines and leveraging the component's built-in accessibility features, you ensure that all users can effectively perceive and understand progress information in your application.
