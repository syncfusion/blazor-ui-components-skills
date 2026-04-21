# Annotations and Labels

## Table of Contents
- [Overview](#overview)
- [Annotations](#annotations)
  - [What Are Annotations](#what-are-annotations)
  - [ProgressBarAnnotations Setup](#progressbarannotations-setup)
  - [ContentTemplate Usage](#contenttemplate-usage)
  - [Adding Text Annotations](#adding-text-annotations)
  - [Adding Custom HTML/Components](#adding-custom-htmlcomponents)
  - [Positioning Annotations](#positioning-annotations)
  - [Multiple Annotations](#multiple-annotations)
  - [Styling Annotations](#styling-annotations)
- [Labels (ShowProgressValue)](#labels-showprogressvalue)
  - [Enabling Progress Labels](#enabling-progress-labels)
  - [Default Percentage Format](#default-percentage-format)
  - [Custom Text Formatting](#custom-text-formatting)
  - [TextRender Event](#textrender-event)
  - [Label Positioning](#label-positioning)
- [Combining Annotations and Labels](#combining-annotations-and-labels)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Overview

Annotations and labels enhance progress bars by displaying contextual information directly on or near the progress indicator. This improves usability by providing immediate visual feedback without requiring users to look elsewhere.

**Key Features:**
- **Annotations:** Custom content overlaid on the progress bar (text, HTML, components)
- **Labels:** Built-in progress percentage display with customizable formatting
- Both work with Linear and Circular types
- Fully customizable styling and positioning

**When to Use:**
- **Annotations:** Display custom information (time remaining, status messages, icons, complex layouts)
- **Labels:** Show simple progress percentage or custom numeric values

## Annotations

### What Are Annotations

Annotations are custom content elements created using the `ProgressBarAnnotation` and `ContentTemplate` properties that overlay the progress bar, typically centered within circular progress or positioned within linear progress. They can contain text, HTML, images, or even Blazor components.

**Common Use Cases:**
- Progress percentage with custom styling
- Status text ("Uploading...", "Processing...")
- Time remaining estimates
- Icons or images
- Multi-line information (percentage + label)
- Dynamic content based on progress

### ProgressBarAnnotations Setup

Annotations are added using the `ProgressBarAnnotations` collection, which contains one or more `ProgressBarAnnotation` elements:

**Basic Structure:**
```razor
<SfProgressBar ...>
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <!-- Your content here -->
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>
```

### ContentTemplate Usage

The `ContentTemplate` property defines the annotation content using standard Razor syntax, allowing you to embed custom HTML and Razor code within annotations.

**Simple Text Annotation:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="60" 
               Height="160px" 
               Width="160px" 
               TrackThickness="10" 
               ProgressThickness="10">
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="font-size:20px; font-weight:bold; color:#000000;">
                    <span>60%</span>
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>
```

### Adding Text Annotations

Use the `Value` property along with annotations to dynamically display text content based on the current progress value.

**Simple Percentage Display:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="60" 
               Height="160px" 
               Width="160px" 
               TrackColor="#FFD939" 
               Radius="100%" 
               InnerRadius="190%" 
               ProgressColor="white" 
               TrackThickness="80" 
               ProgressThickness="10" 
               CornerRadius="CornerType.Round">
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="font-size:20px; font-weight:bold; color:#ffffff;">
                    <span>60%</span>
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>
```

**Multi-Line Text:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="75" 
               Height="180px" 
               Width="180px" 
               ProgressColor="#28a745"
               TrackThickness="12"
               ProgressThickness="12">
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="text-align: center;">
                    <div style="font-size: 32px; font-weight: bold; color: #28a745;">
                        75%
                    </div>
                    <div style="font-size: 14px; color: #666;">
                        Complete
                    </div>
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>
```

**Dynamic Text Based on Progress:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="@currentProgress" 
               Height="180px" 
               Width="180px" 
               ProgressColor="#0d6efd"
               TrackThickness="15"
               ProgressThickness="15">
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="text-align: center;">
                    <div style="font-size: 28px; font-weight: bold; color: #0d6efd;">
                        @currentProgress%
                    </div>
                    <div style="font-size: 12px; color: #666;">
                        @GetStatusMessage()
                    </div>
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>

@code {
    private double currentProgress = 45;
    
    private string GetStatusMessage()
    {
        return currentProgress switch
        {
            (< 25) => "Getting started...",
            (< 50) => "In progress...",
            (< 75) => "Almost there...",
            (< 100) => "Finishing up...",
            _ => "Complete!"
        };
    }
}
```

### Adding Custom HTML/Components

Use the `ContentTemplate` property to include icons, images, and custom components within annotations for enhanced visual representation.

**With Icons:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="85" 
               Height="160px" 
               Width="160px" 
               ProgressColor="#ff9800"
               TrackThickness="12"
               ProgressThickness="12">
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="text-align: center;">
                    <i class="fas fa-download" style="font-size: 24px; color: #ff9800;"></i>
                    <div style="font-size: 20px; font-weight: bold; color: #ff9800; margin-top: 5px;">
                        85%
                    </div>
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>
```

**With Images:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="92" 
               Height="200px" 
               Width="200px" 
               ProgressColor="#4caf50"
               TrackThickness="10"
               ProgressThickness="10">
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="text-align: center;">
                    <img src="success-icon.png" alt="Success" style="width: 48px; height: 48px;" />
                    <div style="font-size: 24px; font-weight: bold; color: #4caf50; margin-top: 10px;">
                        92%
                    </div>
                    <div style="font-size: 12px; color: #666;">
                        Nearly done!
                    </div>
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>
```

**Complex Layout:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="@uploadProgress" 
               Height="220px" 
               Width="220px" 
               ProgressColor="#6f42c1"
               TrackThickness="15"
               ProgressThickness="15">
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="text-align: center; padding: 10px;">
                    <div style="font-size: 36px; font-weight: bold; color: #6f42c1;">
                        @uploadProgress%
                    </div>
                    <div style="font-size: 14px; color: #666; margin-top: 5px;">
                        Uploading document.pdf
                    </div>
                    <div style="font-size: 12px; color: #999; margin-top: 5px;">
                        @GetFileSize() of 50 MB
                    </div>
                    <div style="font-size: 11px; color: #aaa; margin-top: 5px;">
                        ~@GetTimeRemaining() remaining
                    </div>
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>

@code {
    private double uploadProgress = 67;
    
    private string GetFileSize()
    {
        return $"{uploadProgress * 0.5:F1} MB";
    }
    
    private string GetTimeRemaining()
    {
        var remainingPercent = 100 - uploadProgress;
        var seconds = remainingPercent * 0.5; // Estimate
        return $"{seconds:F0} seconds";
    }
}
```

### Positioning Annotations

The annotations are automatically positioned by the component's layout engine. Use CSS styling within the `ContentTemplate` to further customize positioning.

**Centered (Default):**
Annotations are automatically centered within the progress bar, which works perfectly for circular types:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="70" 
               Height="160px" 
               Width="160px">
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="font-size: 24px; font-weight: bold;">70%</div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>
```

**For Linear Progress Bars:**
Annotations appear centered within the linear bar. Use CSS for specific positioning:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="60" 
               Height="80">
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style=" display: flex; justify-content: center; align-items: center; height: 100%; font-size: 18px;
                    font-weight: bold; color: white; white-space: nowrap;">
                    60% Complete
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>
```

### Multiple Annotations

Add multiple `ProgressBarAnnotation` elements within the `ProgressBarAnnotations` collection for layered information:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="80" 
               Height="200px" 
               Width="200px" 
               ProgressColor="#e91e63"
               TrackThickness="15"
               ProgressThickness="15">
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="text-align: center;">
                    <div style="font-size: 40px; font-weight: bold; color: #e91e63;">
                        80
                    </div>
                    <div style="font-size: 16px; color: #666; margin-top: 5px;">
                        out of 100
                    </div>
                    <div style="font-size: 14px; color: #999; margin-top: 10px;">
                        Points
                    </div>
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>
```

**Note:** While you can add multiple `ProgressBarAnnotation` elements, they will overlay each other at the center. Use CSS positioning to arrange them if needed.

### Styling Annotations

Customize the appearance of annotations using inline styles within the `ContentTemplate` property to control fonts, colors, spacing, and backgrounds.

**Custom Fonts and Colors:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="55" 
               Height="180px" 
               Width="180px" 
               ProgressColor="#3f51b5"
               TrackThickness="12"
               ProgressThickness="12">
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="text-align: center; font-family: 'Arial', sans-serif;">
                    <div style="font-size: 48px; font-weight: 700; color: #3f51b5; line-height: 1;">
                        55
                    </div>
                    <div style="font-size: 16px; font-weight: 400; color: #757575; margin-top: 5px; letter-spacing: 1px;">
                        PERCENT
                    </div>
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>
```

**With Background:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="88" 
               Height="200px" 
               Width="200px" 
               ProgressColor="#00bcd4"
               TrackThickness="10"
               ProgressThickness="10">
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="background: white; border-radius: 50%; padding: 20px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); text-align: center;">
                    <div style="font-size: 32px; font-weight: bold; color: #00bcd4;">
                        88%
                    </div>
                    <div style="font-size: 12px; color: #666;">
                        Success Rate
                    </div>
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>
```

## Labels (ShowProgressValue)

### Enabling Progress Labels

The `ShowProgressValue` property enables automatic display of the progress percentage on the progress bar component:

**Linear with Label:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="50" 
               Height="60" 
               Width="90%" 
               ShowProgressValue="true" 
               TrackThickness="24" 
               ProgressThickness="24" 
               TrackColor="#F8C7D8" 
               ProgressColor="#E3165B" 
               CornerRadius="CornerType.Round">
</SfProgressBar>
```

**Circular with Label:**
```razor
<SfProgressBar Type="ProgressType.Circular" 
               Value="75" 
               Height="160px" 
               Width="160px" 
               ShowProgressValue="true" 
               TrackThickness="12" 
               ProgressThickness="12">
</SfProgressBar>
```

### Default Percentage Format

When the `ShowProgressValue` property is set to `true`, the progress is displayed as a percentage (e.g., "50%") by default:

```razor
<SfProgressBar Type="ProgressType.Linear" 
               Value="65" 
               ShowProgressValue="true" 
               Height="60">
</SfProgressBar>
```

**Result:** Displays "65%" within or near the progress bar.

### Custom Text Formatting

Use the `TextRender` event in the `ProgressBarEvents` collection to customize the displayed text before rendering:

**Custom Label Format:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="50" 
               Height="60" 
               Width="90%" 
               ShowProgressValue="true" 
               TrackColor="#F8C7D8" 
               ProgressColor="#E3165B" 
               TrackThickness="24" 
               CornerRadius="CornerType.Round" 
               ProgressThickness="24">
    <ProgressBarEvents TextRender="@TextHandler"></ProgressBarEvents>
</SfProgressBar>

@code{
    public void TextHandler(Syncfusion.Blazor.ProgressBar.TextRenderEventArgs args)
    {
        args.Text = $"Progress: {args.Text}";
    }
}
```

**Result:** Displays "Progress: 50%" instead of "50%".

### TextRender Event

The `TextRender` event in `ProgressBarEvents` fires before the text is rendered, allowing full customization of text through `TextRenderEventArgs`:

**Event Arguments:**
- `args.Value` - Current progress value
- `args.Text` - Current text (can be modified)

**Examples:**

**Fraction Format:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="40" 
               ShowProgressValue="true" 
               Maximum="100"
               Height="60">
    <ProgressBarEvents TextRender="@FractionFormat"></ProgressBarEvents>
</SfProgressBar>

@code{
    public void FractionFormat(Syncfusion.Blazor.ProgressBar.TextRenderEventArgs args)
    {
        args.Text = $"{args.Text}/100";
    }
}
```
**Result:** "40/100"

**Custom Units:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="@filesProcessed" 
               Maximum="@totalFiles"
               ShowProgressValue="true" 
               Height="60">
    <ProgressBarEvents TextRender="@FilesFormat"></ProgressBarEvents>
</SfProgressBar>

@code{
    private double filesProcessed = 37;
    private double totalFiles = 100;
    
    public void FilesFormat(Syncfusion.Blazor.ProgressBar.TextRenderEventArgs args)
    {
        args.Text = $"{filesProcessed} of {totalFiles} files";
    }
}
```
**Result:** "37 of 100 files"

**Conditional Formatting:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="@progress" 
               ShowProgressValue="true" 
               Height="60">
    <ProgressBarEvents TextRender="@ConditionalFormat"></ProgressBarEvents>
</SfProgressBar>

@code{
    private double progress = 100;
    
    public void ConditionalFormat(Syncfusion.Blazor.ProgressBar.TextRenderEventArgs args)
    {
        // Remove % if it exists and trim spaces
        string raw = args.Text.Replace("%", "").Trim();
        int.TryParse(raw, out int value)
        if (value >= 100)
        {
            args.Text = "✓ Complete!";
        }
        else if (value >= 75)
        {
            args.Text = $"{value}% - Almost done";
        }
        else
        {
            args.Text = $"{value}%";
        }
    }
}
```

### Label Positioning

Labels are automatically positioned by the component's layout engine:
- **Linear:** Inside or outside the progress bar depending on space
- **Circular:** Centered within the circle

The positioning is controlled internally and cannot be manually repositioned. For custom positioning, use annotations with the `ContentTemplate` property instead.

## Combining Annotations and Labels

You can use both the `ShowProgressValue` property and annotations simultaneously with `ProgressBarAnnotations`, though typically you choose one:

**Both Enabled (overlapping):**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="70" 
               Height="160px" 
               Width="160px" 
               ShowProgressValue="true">
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="font-size: 24px; font-weight: bold; color: #ff0000;">
                    Custom: 70%
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>
```

**⚠️ Warning:** This may cause overlapping text. Generally, choose either labels OR annotations, not both.

**Recommended Pattern:**
```razor
@using Syncfusion.Blazor.ProgressBar

<!-- Use ShowProgressValue for simple percentage display -->
<SfProgressBar Value="60" ShowProgressValue="true" Height="60">
</SfProgressBar>

<!-- Use Annotations for custom, styled, or complex content -->
<SfProgressBar Type="ProgressType.Circular" Value="60" Height="160px" Width="160px">
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="text-align: center;">
                    <div style="font-size: 28px; font-weight: bold;">60%</div>
                    <div style="font-size: 12px; color: #666;">Complete</div>
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>
```

## Complete Examples

**Example 1: Goal Tracker**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="@goalProgress" 
               Height="200px" 
               Width="200px" 
               ProgressColor="#ff6b6b"
               TrackColor="#ffe0e0"
               TrackThickness="15"
               ProgressThickness="15">
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="text-align: center;">
                    <div style="font-size: 14px; color: #999; text-transform: uppercase; letter-spacing: 1px;">
                        Goal
                    </div>
                    <div style="font-size: 42px; font-weight: bold; color: #ff6b6b; margin: 5px 0;">
                        @goalProgress%
                    </div>
                    <div style="font-size: 16px; color: #666;">
                        $@((goalProgress * 1000).ToString("N0"))
                    </div>
                    <div style="font-size: 12px; color: #999;">
                        of $100,000
                    </div>
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>

@code {
    private double goalProgress = 82;
}
```

**Example 2: File Upload with Time**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="@uploadProgress" 
               Height="70"
               TrackThickness="24"
               ProgressThickness="24"
               ProgressColor="#28a745"
               TrackColor="#e9ecef"
               CornerRadius="CornerType.Round">
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="display: flex; justify-content: center; align-items: center; height: 100%; font-size: 18px;
                    font-weight: bold; color: white; white-space: nowrap;">
                    <div style="font-size: 16px;">@uploadProgress%</div>
                    <div style="font-size: 11px; opacity: 0.9;">~@GetTimeRemaining() remaining</div>
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>

@code {
    private double uploadProgress = 45;
    
    private string GetTimeRemaining()
    {
        var remainingSeconds = (100 - uploadProgress) * 0.8;
        return $"{remainingSeconds:F0}s";
    }
}
```

**Example 3: Battery Level Indicator**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="@batteryLevel" 
               Height="140px" 
               Width="140px" 
               ProgressColor="@GetBatteryColor()"
               TrackColor="#f0f0f0"
               TrackThickness="10"
               ProgressThickness="10">
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="text-align: center;">
                    <i class="fas fa-battery-@GetBatteryIcon()" style="font-size: 32px; color: @GetBatteryColor();"></i>
                    <div style="font-size: 20px; font-weight: bold; color: @GetBatteryColor(); margin-top: 5px;">
                        @batteryLevel%
                    </div>
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>

@code {
    private double batteryLevel = 35;
    
    private string GetBatteryColor()
    {
        return batteryLevel switch
        {
            > 60 => "#28a745",
            > 20 => "#ffc107",
            _ => "#dc3545"
        };
    }
    
    private string GetBatteryIcon()
    {
        return batteryLevel switch
        {
            > 75 => "full",
            > 50 => "three-quarters",
            > 25 => "half",
            > 10 => "quarter",
            _ => "empty"
        };
    }
}
```

## Best Practices

1. **Choose Between Annotations and Labels:**
   - Use `ShowProgressValue` for simple percentage display
   - Use annotations for custom styling, multi-line content, or complex layouts

2. **Keep Annotations Readable:**
   - Ensure sufficient contrast between text and background
   - Use appropriate font sizes (minimum 14px for body text)
   - Test readability at different screen sizes

3. **Circular Progress Annotations:**
   - Center content vertically and horizontally
   - Account for available space (inner radius)
   - Use relative font sizes for responsiveness

4. **Linear Progress Annotations:**
   - Ensure text doesn't overflow the progress bar height
   - Use contrasting colors (white text on dark progress)
   - Consider positioning for different progress values

5. **Dynamic Content:**
   - Update annotations when progress changes
   - Use `StateHasChanged()` to refresh annotations
   - Avoid heavy computations in ContentTemplate

6. **Accessibility:**
   - Annotations are visual only - ensure screen readers can access progress
   - Don't rely solely on annotations for critical information
   - Provide text alternatives for icon-only annotations

7. **Performance:**
   - Minimize complex HTML in annotations
   - Avoid frequent annotation updates (throttle if needed)
   - Use simple CSS for styling rather than nested components

8. **Styling Consistency:**
   - Match annotation styling to your design system
   - Coordinate annotation colors with progress bar colors
   - Maintain consistent typography across annotations
