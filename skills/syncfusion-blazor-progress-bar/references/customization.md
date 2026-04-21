# ProgressBar Customization

## Table of Contents
- [Overview](#overview)
- [Segments](#segments)
  - [SegmentCount Property](#segmentcount-property)
  - [EnableProgressSegments](#enableprogresssegments)
  - [SegmentColor (Multi-Color)](#segmentcolor-multi-color)
  - [Use Cases for Segments](#use-cases-for-segments)
- [Thickness Customization](#thickness-customization)
  - [TrackThickness](#trackthickness)
  - [ProgressThickness](#progressthickness)
  - [SecondaryProgressThickness](#secondaryprogressthickness)
  - [Examples for Both Types](#examples-for-both-types)
- [Radius and Shape](#radius-and-shape)
  - [Radius Property (Circular)](#radius-property-circular)
  - [InnerRadius Property](#innerradius-property)
  - [CornerRadius Property](#cornerradius-property)
  - [Creating Donut Charts](#creating-donut-charts)
- [Colors](#colors)
  - [ProgressColor](#progresscolor)
  - [TrackColor](#trackcolor)
  - [SecondaryProgressColor](#secondaryprogresscolor)
  - [Range Colors with Gradients](#range-colors-with-gradients)
  - [Color Selection Guide](#color-selection-guide)
- [Margin and Spacing](#margin-and-spacing)
  - [ProgressBarMargin Configuration](#progressbarmargin-configuration)
  - [Layout Considerations](#layout-considerations)
- [RTL Support](#rtl-support)
  - [EnableRtl Property](#enablertl-property)
  - [Localization Considerations](#localization-considerations)
- [Visibility Control](#visibility-control)
  - [Visible Property](#visible-property)
  - [Dynamic Show/Hide](#dynamic-showhide)
  - [Complete Visibility Example](#complete-visibility-example)
- [Complete Customization Examples](#complete-customization-examples)

## Overview

The Blazor ProgressBar component offers extensive customization options to match your application's design system and requirements. This guide covers all visual and behavioral customization capabilities.

**Customization Categories:**
- **Segments:** Divide progress into discrete blocks
- **Thickness:** Control width of track and progress indicators
- **Radius:** Adjust circular progress dimensions and corner rounding
- **Colors:** Customize progress, track, and secondary progress colors
- **Margin:** Control spacing around the component
- **RTL:** Right-to-left language support
- **Visibility:** Dynamic show/hide control

## Segments

Segments divide the progress bar into distinct visual blocks, useful for representing discrete steps or stages. The `SegmentCount`, `EnableProgressSegments`, and `SegmentColor` properties will be used for segment configuration.

### SegmentCount Property

The `SegmentCount` property divides progress into equal segments:

**Linear Segments:**
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

**Result:** Progress bar divided into 8 equal blocks.

**Circular Segments:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="100" 
               Height="180" 
               SegmentCount="8" 
               TrackColor="#696969"
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Result:** Circular progress divided into 8 arc segments.

**Segment Behavior:**
- Progress fills segments sequentially
- Each segment represents an equal portion of the range
- Partially filled segments show fractional progress

### EnableProgressSegments

The `EnableProgressSegments` property creates segments without track background, showing only filled segments:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               EnableProgressSegments="true" 
               Value="100" 
               Height="180" 
               SegmentCount="8" 
               TrackColor="#696969"
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Difference:**
- **SegmentCount only:** Shows track with segment dividers, progress fills segments
- **EnableProgressSegments="true":** Shows only filled segments, no track background

**Comparison:**
```razor
<!-- With track background -->
<SfProgressBar SegmentCount="6" Value="50" Height="60">
</SfProgressBar>

<!-- Without track background (segments only) -->
<SfProgressBar EnableProgressSegments="true" SegmentCount="6" Value="50" Height="60">
</SfProgressBar>
```

### SegmentColor (Multi-Color)

Apply different colors to segments using the `SegmentColor` array:

**Multi-Color Circular Segments:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="100" 
               Height="180" 
               SegmentCount="8" 
               SegmentColor='new string[] { "#00bdaf", "#2f7ecc", "#e9648e", "#fbb78a" }' 
               TrackColor="#696969"
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Color Array Behavior:**
- Colors are applied in sequence to segments
- If fewer colors than segments, colors repeat cyclically
- 4 colors for 8 segments: Colors repeat twice

**Multi-Color with EnableProgressSegments:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               EnableProgressSegments="true" 
               Value="100" 
               Height="180" 
               SegmentCount="8" 
               SegmentColor='new string[] { "#00bdaf", "#2f7ecc", "#e9648e", "#fbb78a" }' 
               TrackColor="#696969"
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Linear Multi-Color Segments:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="100" 
               Height="60" 
               SegmentCount="4" 
               SegmentColor='new string[] { "#28a745", "#17a2b8", "#ffc107", "#dc3545" }' 
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

### Use Cases for Segments

**Multi-Step Form (5 steps):**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="@((currentStep / 5.0) * 100)" 
               Height="60" 
               SegmentCount="5"
               TrackThickness="16"
               ProgressThickness="16"
               Minimum="0" 
               Maximum="100">
</SfProgressBar>

@code {
    private int currentStep = 3; // On step 3 of 5
}
```

**Skill Level Indicator:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="62.5" 
               Height="160px"
               Width="160px"
               SegmentCount="8" 
               SegmentColor='new string[] { "#4caf50", "#8bc34a", "#cddc39", "#ffeb3b", "#ffc107", "#ff9800", "#ff5722", "#f44336" }' 
               TrackThickness="12"
               ProgressThickness="12">
</SfProgressBar>
<!-- 5 out of 8 levels achieved -->
```

**Color-Coded Priority Levels:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="100" 
               Height="60" 
               SegmentCount="3" 
               SegmentColor='new string[] { "#28a745", "#ffc107", "#dc3545" }'>
</SfProgressBar>
<!-- Green (low), Yellow (medium), Red (high) priority -->
```

## Thickness Customization

Control the visual weight of track, progress, and secondary progress indicators. The `TrackThickness`, `ProgressThickness`, and `SecondaryProgressThickness` properties will be used for thickness configuration.

### TrackThickness

The `TrackThickness` property sets the background track width:

**Linear Track Thickness:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="100" 
               Height="60" 
               Width="90%" 
               TrackThickness="24" 
               ProgressThickness="24" 
               ShowProgressValue="true" 
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Circular Track Thickness:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="80" 
               Height="160px"
               Width="160px"
               TrackThickness="20" 
               ProgressThickness="20">
</SfProgressBar>
```

**Recommendations:**
- **Linear:** 8-30px (16-24px ideal)
- **Circular:** 6-20px (10-15px ideal)
- Match TrackThickness and ProgressThickness for uniform appearance
- Thicker tracks for emphasis, thinner for subtlety

### ProgressThickness

The `ProgressThickness` property sets the progress indicator width:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="70" 
               TrackThickness="20" 
               ProgressThickness="20" 
               Height="60">
</SfProgressBar>
```

**Different Thickness Example:**
```razor
@using Syncfusion.Blazor.ProgressBar

<!-- Thinner progress on thicker track -->
<SfProgressBar Type="ProgressType.Linear" 
               Value="50" 
               TrackThickness="24" 
               ProgressThickness="12" 
               Height="60">
</SfProgressBar>
```

**Use Cases:**
- **Same thickness:** Uniform, clean appearance
- **Thinner progress:** Subtle, minimal design
- **Thicker progress:** Emphasis, bold design

### SecondaryProgressThickness

For buffer states, control secondary progress thickness separately:

**Linear Secondary Progress:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="80" 
               SecondaryProgress="50" 
               SecondaryProgressThickness="30" 
               TrackThickness="20"
               ProgressThickness="20"
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Circular Secondary Progress:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="80" 
               SecondaryProgress="40" 
               SecondaryProgressThickness="20" 
               TrackThickness="12"
               ProgressThickness="12"
               Height="160px"
               Width="160px"
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Layering:**
- Secondary progress appears behind primary progress
- Can be thicker or thinner than primary
- Use different thickness for visual hierarchy

### Examples for Both Types

**Thin Minimal Style:**
```razor
<!-- Linear -->
<SfProgressBar Type="ProgressType.Linear" 
               Value="65" 
               Height="40"
               TrackThickness="4" 
               ProgressThickness="4">
</SfProgressBar>

<!-- Circular -->
<SfProgressBar Type="ProgressType.Circular" 
               Value="65" 
               Height="120px"
               Width="120px"
               TrackThickness="4" 
               ProgressThickness="4">
</SfProgressBar>
```

**Bold Prominent Style:**
```razor
<!-- Linear -->
<SfProgressBar Type="ProgressType.Linear" 
               Value="65" 
               Height="80"
               TrackThickness="30" 
               ProgressThickness="30">
</SfProgressBar>

<!-- Circular -->
<SfProgressBar Type="ProgressType.Circular" 
               Value="65" 
               Height="200px"
               Width="200px"
               TrackThickness="18" 
               ProgressThickness="18">
</SfProgressBar>
```

## Radius and Shape

Control circular progress dimensions and corner rounding. The `Radius`, `InnerRadius`, and `CornerRadius` properties will be used for radius and shape configuration.

### Radius Property (Circular)

The `Radius` property adjusts the outer radius as a percentage:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="60" 
               Height="160px" 
               Width="160px" 
               Radius="100%" 
               TrackThickness="10" 
               ProgressThickness="10"
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Radius Values:**
- `"100%"` - Full radius (default, touches container edges)
- `"80%"` - 80% of container, leaves margin
- `"90%"` - Slight inset from edges

**Use Cases:**
- Adjust to prevent clipping
- Create visual spacing
- Align with other circular elements

### InnerRadius Property

The `InnerRadius` property creates the donut hole:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="60" 
               Height="160px" 
               Width="160px" 
               Radius="100%" 
               InnerRadius="80%" 
               TrackColor="#FFD939" 
               ProgressColor="white" 
               TrackThickness="80" 
               ProgressThickness="10" 
               CornerRadius="CornerType.Round" 
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**InnerRadius Values:**
- `"90%"` - Thin ring (small donut hole)
- `"70%"` - Medium ring
- `"50%"` - Thick ring (large donut hole)
- `"190%"` - Can exceed 100% for special effects

**Relationship with TrackThickness:**
- Large InnerRadius + thick TrackThickness = thin ring appearance
- Small InnerRadius + thin TrackThickness = standard donut

### CornerRadius Property

The `CornerRadius` property rounds the edges of progress indicators:

**Round Corners:**
```razor
<SfProgressBar Type="ProgressType.Linear" 
               Value="60" 
               Height="60"
               TrackThickness="24" 
               ProgressThickness="24" 
               CornerRadius="CornerType.Round">
</SfProgressBar>
```

**Square Corners:**
```razor
<SfProgressBar Type="ProgressType.Linear" 
               Value="60" 
               Height="60"
               TrackThickness="24" 
               ProgressThickness="24" 
               CornerRadius="CornerType.Square">
</SfProgressBar>
```

**Options:**
- `CornerType.Round` - Rounded/pill-shaped ends
- `CornerType.Square` - Sharp 90-degree corners (default)

**Works with Both Types:**
- **Linear:** Rounds the left and right ends
- **Circular:** Rounds the start and end of the arc

### Creating Donut Charts

Combine properties for custom donut appearances:

**Wide Donut:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="75" 
               Height="180px" 
               Width="180px" 
               Radius="100%" 
               InnerRadius="60%" 
               TrackThickness="15" 
               ProgressThickness="15" 
               TrackColor="#e0e0e0"
               ProgressColor="#2196f3">
</SfProgressBar>
```

**Thin Ring:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="75" 
               Height="180px" 
               Width="180px" 
               Radius="100%" 
               InnerRadius="92%" 
               TrackThickness="8" 
               ProgressThickness="8" 
               TrackColor="#e0e0e0"
               ProgressColor="#2196f3">
</SfProgressBar>
```

**Thick Track with Thin Progress:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="60" 
               Height="160px" 
               Width="160px" 
               Radius="80%" 
               InnerRadius="190%" 
               TrackColor="#FFD939" 
               ProgressColor="white" 
               TrackThickness="80" 
               ProgressThickness="10" 
               CornerRadius="CornerType.Round">
</SfProgressBar>
```

## Colors

Customize progress, track, and secondary progress colors to match your design system. The `ProgressColor`, `TrackColor`, and `SecondaryProgressColor` properties will be used for color customization.

### ProgressColor

The `ProgressColor` property sets the main progress indicator color:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="70" 
               ProgressColor="#28a745" 
               Height="60">
</SfProgressBar>
```

**Accepts:**
- Hex colors: `"#28a745"`
- RGB: `"rgb(40, 167, 69)"`
- Named colors: `"green"` (use with caution)

### TrackColor

The `TrackColor` property sets the background track color:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="50" 
               ProgressColor="#E3165B" 
               TrackColor="#F8C7D8" 
               Height="60">
</SfProgressBar>
```

**Best Practices:**
- Use lighter shade of ProgressColor for subtle contrast
- Use neutral gray (#e0e0e0, #f0f0f0) for universal themes
- Ensure sufficient contrast with page background

### SecondaryProgressColor

The `SecondaryProgressColor` property colors the secondary progress:

**Linear with Secondary Color:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               ProgressColor="#cc0202" 
               Value="50" 
               SecondaryProgress="60" 
               SecondaryProgressColor="#faa7a7" 
               TrackThickness="10"
               ProgressThickness="10"
               SecondaryProgressThickness="10"
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Circular with Secondary Color:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               ProgressColor="#cc0202" 
               Value="50" 
               SecondaryProgress="60" 
               SecondaryProgressColor="#faa7a7" 
               TrackThickness="10"
               ProgressThickness="10"
               SecondaryProgressThickness="10"
               Height="160px"
               Width="160px"
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Color Relationships:**
- Secondary typically lighter than primary
- Both should contrast with track
- Consider colorblind-friendly palettes

### Range Colors with Gradients

Apply different colors to specific progress ranges with optional gradients:

**Basic Range Colors:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="100" IsGradient="false">
    <ProgressBarRangeColors>
        <ProgressBarRangeColor Start="0" End="25" Color="#00bdaf"></ProgressBarRangeColor>
        <ProgressBarRangeColor Start="25" End="50" Color="#2f7ecc"></ProgressBarRangeColor>
        <ProgressBarRangeColor Start="50" End="75" Color="#e9648e"></ProgressBarRangeColor>
        <ProgressBarRangeColor Start="75" End="100" Color="#fbb78a"></ProgressBarRangeColor>
    </ProgressBarRangeColors>
</SfProgressBar>
```

**With Gradient Effect:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Value="100" IsGradient="true">
    <ProgressBarRangeColors>
        <ProgressBarRangeColor Start="0" End="25" Color="#00bdaf"></ProgressBarRangeColor>
        <ProgressBarRangeColor Start="25" End="50" Color="#2f7ecc"></ProgressBarRangeColor>
        <ProgressBarRangeColor Start="50" End="75" Color="#e9648e"></ProgressBarRangeColor>
        <ProgressBarRangeColor Start="75" End="100" Color="#fbb78a"></ProgressBarRangeColor>
    </ProgressBarRangeColors>
</SfProgressBar>
```

**Properties:**
- `Start` - Range start value
- `End` - Range end value
- `Color` - Color for this range
- `IsGradient` - Smooth transitions between colors (true) vs hard stops (false)

**Use Cases:**
- **Temperature:** Blue (cold) → Yellow (warm) → Red (hot)
- **Performance:** Green (good) → Yellow (fair) → Red (poor)
- **Progress Stages:** Different color per milestone
- **Risk Levels:** Low (green) → Medium (yellow) → High (orange) → Critical (red)

### Color Selection Guide

**Semantic Colors (Bootstrap-inspired):**
```razor
@using Syncfusion.Blazor.ProgressBar

<!-- Success/Complete -->
<SfProgressBar ProgressColor="#28a745" TrackColor="#d4edda" Value="100" Height="60">
</SfProgressBar>

<!-- Info/Processing -->
<SfProgressBar ProgressColor="#17a2b8" TrackColor="#d1ecf1" Value="60" Height="60">
</SfProgressBar>

<!-- Warning -->
<SfProgressBar ProgressColor="#ffc107" TrackColor="#fff3cd" Value="75" Height="60">
</SfProgressBar>

<!-- Danger/Error -->
<SfProgressBar ProgressColor="#dc3545" TrackColor="#f8d7da" Value="90" Height="60">
</SfProgressBar>

<!-- Primary -->
<SfProgressBar ProgressColor="#007bff" TrackColor="#cfe2ff" Value="50" Height="60">
</SfProgressBar>
```

**Contrast Guidelines:**
- Ensure 3:1 contrast ratio minimum between progress and track
- Ensure 4.5:1 contrast ratio between progress and background for accessibility
- Test with colorblind simulators
- Provide non-color indicators (percentage text) for critical information

## Margin and Spacing

Control spacing around the progress bar. The `ProgressBarMargin` component will be used for margin and spacing configuration.

### ProgressBarMargin Configuration

Control spacing around the progress bar using the `ProgressBarMargin` component:

**Default Margins (10px all sides):**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="80" 
               Height="150px" 
               Width="150px" 
               TrackThickness="20" 
               ProgressThickness="20">
    <!-- Default: Left="10" Right="10" Top="10" Bottom="10" -->
</SfProgressBar>
```

**Zero Margins:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Height="150px" 
               Width="150px" 
               Value="80" 
               TrackThickness="20" 
               ProgressThickness="20">
    <ProgressBarMargin Left="0" Right="0" Bottom="0" Top="0">
    </ProgressBarMargin>
</SfProgressBar>
```

**Custom Margins:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="70" 
               Height="60">
    <ProgressBarMargin Left="20" Right="20" Bottom="5" Top="5">
    </ProgressBarMargin>
</SfProgressBar>
```

### Layout Considerations

**Tight Layouts (zero/minimal margins):**
- Use when progress bar is in a bordered container
- Cards, dialogs, or confined spaces
- Maximizes available space

**Spaced Layouts (standard/large margins):**
- Use in open layouts for breathing room
- Prevents visual crowding
- Improves touch target size

**Asymmetric Margins:**
```razor
<!-- More space at bottom for labels -->
<SfProgressBar Value="80" Height="60">
    <ProgressBarMargin Top="5" Bottom="20" Left="10" Right="10">
    </ProgressBarMargin>
</SfProgressBar>
<p style="margin-top: -15px;">Loading complete</p>
```

## RTL Support

The `EnableRtl` property will be used for right-to-left language support.

### EnableRtl Property

Enable right-to-left rendering for RTL languages (Arabic, Hebrew, etc.):

**Linear RTL:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar EnableRtl="true" 
               Value="50" 
               Type="ProgressType.Linear"
               Height="60">
</SfProgressBar>
```

**Effect:** Progress fills from right to left instead of left to right.

**Circular RTL:**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar EnableRtl="true" 
               Value="80" 
               Type="ProgressType.Circular"
               Height="160px"
               Width="160px">
</SfProgressBar>
```

**Effect:** Progress arc travels counterclockwise instead of clockwise.

### Localization Considerations

**Conditional RTL Based on Culture:**
```razor
@using Syncfusion.Blazor.ProgressBar
@using System.Globalization

<SfProgressBar EnableRtl="@isRtl" 
               Value="70" 
               Type="ProgressType.Linear"
               Height="60"
               ShowProgressValue="true">
</SfProgressBar>

@code {
    private bool isRtl = CultureInfo.CurrentUICulture.TextInfo.IsRightToLeft;
}
```

**Full RTL Example:**
```razor
@using Syncfusion.Blazor.ProgressBar

<div dir="rtl" style="padding: 20px;">
    <h4>تحميل الملف</h4>
    
    <SfProgressBar EnableRtl="true" 
                   Value="60" 
                   Type="ProgressType.Linear"
                   Height="60"
                   ShowProgressValue="true"
                   TrackThickness="16"
                   ProgressThickness="16">
    </SfProgressBar>
    
    <p>٪60 مكتمل</p>
</div>
```

## Visibility Control

The `Visible` property will be used for visibility control.

### Visible Property

Control whether the progress bar is rendered:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="100" 
               Height="60" 
               Visible="@isVisible"
               Minimum="0" 
               Maximum="100">
</SfProgressBar>

@code {
    private bool isVisible = true;
}
```

**Use Cases:**
- Show progress bar only during operations
- Hide when operation completes
- Conditional display based on state

### Dynamic Show/Hide

Toggle visibility based on operation state:

```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="@progressValue" 
               Height="60" 
               Visible="@showProgressBar"
               ShowProgressValue="true">
</SfProgressBar>

<button class="btn btn-primary" @onclick="StartOperation">Start</button>

@code {
    private double progressValue = 0;
    private bool showProgressBar = false;
    private System.Threading.Timer? timer;

    private void StartOperation()
    {
        progressValue = 0;
        showProgressBar = true;

        timer?.Dispose();
        timer = new System.Threading.Timer(_ =>
        {
            if (progressValue >= 100)
            {
                showProgressBar = false;
                timer?.Dispose();
            }
            else
            {
                progressValue += 10;
            }

            InvokeAsync(StateHasChanged); 

        }, null, 0, 500);
    }

    public void Dispose()
    {
        timer?.Dispose();
    }
}
```

### Complete Visibility Example

Show progress during upload, hide with success message:

```razor
@using Syncfusion.Blazor.ProgressBar

<div style="padding: 20px;">
    <SfProgressBar Type="ProgressType.Linear" 
                   Value="@uploadProgress" 
                   Height="60" 
                   Visible="@showProgressBar"
                   TrackThickness="16"
                   ProgressThickness="16"
                   ProgressColor="#28a745"
                   TrackColor="#e0e0e0"
                   Minimum="0" 
                   Maximum="100">
        <ProgressBarAnimation Enable="true" Duration="300"></ProgressBarAnimation>
        <ProgressBarEvents AnimationComplete="@OnUploadComplete"></ProgressBarEvents>
    </SfProgressBar>
    
    <div style="text-align: center; margin-top: 10px;">
        <p style="color: @messageColor; font-size: larger; font-weight: bold;">
            @statusMessage
        </p>
    </div>
    
    <button class="btn btn-primary" @onclick="StartUpload" disabled="@isUploading">
        Upload File
    </button>
</div>

@code {
    private double uploadProgress = 0;
    private bool showProgressBar = false;
    private bool isUploading = false;
    private string statusMessage = "";
    private string messageColor = "#000";
    private System.Threading.Timer? timer;
    
    private void StartUpload()
    {
        isUploading = true;
        showProgressBar = true;
        uploadProgress = 0;
        statusMessage = "";
        
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
        }, null, 0, 300);
    }
    
    private void OnUploadComplete(ProgressValueEventArgs args)
    {
        if (args.Value == 100)
        {
            showProgressBar = false;
            statusMessage = "UPLOAD SUCCESS!";
            messageColor = "#28a745";
            isUploading = false;
            StateHasChanged();
            
            // Clear message after 3 seconds
            Task.Delay(3000).ContinueWith(_ =>
            {
                InvokeAsync(() =>
                {
                    statusMessage = "";
                    StateHasChanged();
                });
            });
        }
    }
    
    public void Dispose()
    {
        timer?.Dispose();
    }
}
```

## Complete Customization Examples

**Example 1: Fully Customized Linear Progress**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="75" 
               Height="70"
               ShowProgressValue="true"
               TrackThickness="26"
               ProgressThickness="26"
               CornerRadius="CornerType.Round"
               ProgressColor="#6f42c1"
               TrackColor="#e9ecef"
               IsGradient="false"
               Minimum="0" 
               Maximum="100">
    <ProgressBarMargin Left="15" Right="15" Top="10" Bottom="10">
    </ProgressBarMargin>
    <ProgressBarAnimation Enable="true" Duration="500" Delay="0">
    </ProgressBarAnimation>
</SfProgressBar>
```

**Example 2: Fully Customized Circular Progress**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Circular" 
               Value="82" 
               Height="200px"
               Width="200px"
               Radius="95%"
               InnerRadius="75%"
               TrackThickness="15"
               ProgressThickness="15"
               CornerRadius="CornerType.Round"
               ProgressColor="#ff6b6b"
               TrackColor="#f0f0f0"
               Minimum="0" 
               Maximum="100">
    <ProgressBarMargin Left="10" Right="10" Top="10" Bottom="10">
    </ProgressBarMargin>
    <ProgressBarAnnotations>
        <ProgressBarAnnotation>
            <ContentTemplate>
                <div style="text-align: center;">
                    <div style="font-size: 14px; font-weight: bold; color: #ff6b6b;">
                        82%
                    </div>
                    <div style="font-size: 14px; color: #ff6b6b;">
                        Complete
                    </div>
                </div>
            </ContentTemplate>
        </ProgressBarAnnotation>
    </ProgressBarAnnotations>
</SfProgressBar>
```

**Example 3: Multi-Color Segmented Progress**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="100" 
               Height="60"
               SegmentCount="5"
               SegmentColor='new string[] { "#4caf50", "#8bc34a", "#cddc39", "#ffc107", "#ff9800" }'
               TrackThickness="20"
               ProgressThickness="20"
               TrackColor="#e0e0e0"
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

**Example 4: Buffer State with Custom Colors**
```razor
@using Syncfusion.Blazor.ProgressBar

<SfProgressBar Type="ProgressType.Linear" 
               Value="45" 
               SecondaryProgress="75"
               Height="60"
               ProgressColor="#1976d2"
               SecondaryProgressColor="#90caf9"
               TrackColor="#263238"
               TrackThickness="14"
               ProgressThickness="14"
               SecondaryProgressThickness="14"
               CornerRadius="CornerType.Round"
               Minimum="0" 
               Maximum="100">
</SfProgressBar>
```

All customization options can be combined to create progress bars that perfectly match your application's design requirements and user experience goals.
