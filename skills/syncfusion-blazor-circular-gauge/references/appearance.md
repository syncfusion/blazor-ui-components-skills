# Appearance and Styling

## Table of Contents
- [Overview](#overview)
- [Themes](#themes)
- [Dimensions](#dimensions)
- [Title](#title)
- [Center Positioning](#center-positioning)
- [Background and Border](#background-and-border)
- [Animation](#animation)
- [Print and Export](#print-and-export)

## Overview

The CircularGauge component provides extensive styling options including built-in themes, custom dimensions, titles, positioning, animations, and export capabilities.

## Themes

Syncfusion Blazor components come with built-in themes that can be applied by referencing the appropriate CSS file. The theme styling is controlled by CSS file references in the HTML head.

### Available Themes

Reference themes in **~/index.html** or **App.razor**:

```html
<!-- Bootstrap 5 (default) -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Material -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Fabric (Office 365) -->
<link href="_content/Syncfusion.Blazor.Themes/fabric.css" rel="stylesheet" />

<!-- Fluent -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />

<!-- Tailwind -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />

<!-- Bootstrap 4 -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap4.css" rel="stylesheet" />

<!-- High Contrast -->
<link href="_content/Syncfusion.Blazor.Themes/highcontrast.css" rel="stylesheet" />
```

### Dark Themes

```html
<!-- Material Dark -->
<link href="_content/Syncfusion.Blazor.Themes/material-dark.css" rel="stylesheet" />

<!-- Bootstrap 5 Dark -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5-dark.css" rel="stylesheet" />

<!-- Tailwind Dark -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind-dark.css" rel="stylesheet" />

<!-- Fluent Dark -->
<link href="_content/Syncfusion.Blazor.Themes/fluent-dark.css" rel="stylesheet" />

<!-- Fabric Dark -->
<link href="_content/Syncfusion.Blazor.Themes/fabric-dark.css" rel="stylesheet" />
```

### Custom Colors

Override default colors using custom CSS:

```html
<style>
    .e-circulargauge .e-axis-line {
        stroke: #007DD1;
    }
    
    .e-circulargauge .e-pointer {
        fill: #E74C3C;
    }
</style>
```

## Dimensions

Control the gauge size using the `Width` and `Height` properties to define custom dimensions for the circular gauge.

### Fixed Dimensions (Pixels)

```cshtml
<SfCircularGauge Width="400px" Height="400px">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="65">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Use Cases:**
- Fixed-size dashboard widgets
- Consistent sizing across pages
- Non-responsive layouts

### Responsive Dimensions (Percentage)

```cshtml
<div style="width:800px; height:600px">
    <SfCircularGauge Width="100%" Height="100%">
        <CircularGaugeAxes>
            <CircularGaugeAxis>
                <CircularGaugePointers>
                    <CircularGaugePointer Value="65">
                    </CircularGaugePointer>
                </CircularGaugePointers>
            </CircularGaugeAxis>
        </CircularGaugeAxes>
    </SfCircularGauge>
</div>
```

**Percentage Behavior:**
- `100%`: Fills the entire container
- `50%`: Half of the container size
- Adapts to container resizing

**Use Cases:**
- Responsive dashboards
- Mobile-friendly applications
- Flexible layouts

### Default Size

If no size is specified:
- **Height**: `450px`
- **Width**: `100%` (fills container width)

### Aspect Ratio Consideration

For circular gauges, keep width and height equal or similar:

```cshtml
<!-- Square (ideal for full circles) -->
<SfCircularGauge Width="400px" Height="400px">
</SfCircularGauge>

<!-- Wide (good for semi-circles) -->
<SfCircularGauge Width="600px" Height="300px">
</SfCircularGauge>
```

## Title

Add a descriptive title using the `Title` property and customize it with the `CircularGaugeTitleStyle` component to provide context for the gauge.

### Basic Title

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge Title="Speedometer">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Title Styling

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge Title="Temperature Monitor">
    <CircularGaugeTitleStyle 
        Color="#007DD1" 
        FontWeight="bold" 
        Size="24px"
        FontFamily="Arial"
        FontStyle="normal">
    </CircularGaugeTitleStyle>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Title Style Properties:**
- `Color`: Text color
- `Size`: Font size (e.g., "20px", "24px")
- `FontWeight`: normal, bold, lighter, bolder
- `FontFamily`: Font name
- `FontStyle`: normal, italic, oblique

### Title with Subtitle

Use annotations for subtitles:

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge Title="CPU Usage">
    <CircularGaugeTitleStyle Color="#333" FontWeight="bold" Size="22px">
    </CircularGaugeTitleStyle>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeAnnotations>
                <CircularGaugeAnnotation Angle="180" Radius="15%" ZIndex="1">
                    <ContentTemplate>
                        <div style="color:#666; font-size:14px;">Real-time Monitoring</div>
                    </ContentTemplate>
                </CircularGaugeAnnotation>
            </CircularGaugeAnnotations>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Center Positioning

Position the gauge anywhere in the container using the `CenterX` and `CenterY` properties to control the horizontal and vertical positioning of the gauge center.

### Default Position (Center)

```cshtml
<!-- Default: CenterX="50%", CenterY="50%" -->
<SfCircularGauge>
</SfCircularGauge>
```

### Position in Pixels

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge CenterX="100px" CenterY="100px">
    <CircularGaugeAxes>
        <CircularGaugeAxis StartAngle="90" EndAngle="180">
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Use Case:** Absolute positioning for specific layouts.

### Position in Percentage

```cshtml
@using Syncfusion.Blazor.CircularGauge
<!-- Top-left corner -->
<SfCircularGauge CenterX="25%" CenterY="25%">
    <CircularGaugeAxes>
        <CircularGaugeAxis StartAngle="0" EndAngle="180">
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

<!-- Bottom-right area -->
<SfCircularGauge CenterX="75%" CenterY="75%">
    <CircularGaugeAxes>
        <CircularGaugeAxis StartAngle="180" EndAngle="360">
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Percentage Reference:**
- `0%, 0%`: Top-left
- `50%, 50%`: Center (default)
- `100%, 100%`: Bottom-right

**Use Cases:**
- Multi-gauge dashboards
- Semi-circle gauges positioned strategically
- Asymmetric layouts

## Background and Border

Customize the gauge's background using the `Background` property and border using the `CircularGaugeBorder` component with `Color` and `Width` properties.

### Background Color

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge Background="#F5F5F5">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Background Options:**
- Solid colors: `"#FFFFFF"`, `"lightblue"`
- Transparent: `"transparent"`
- RGB/RGBA: `"rgba(0, 123, 255, 0.1)"`

### Border Styling

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge Background="white">
    <CircularGaugeBorder Color="#007DD1" Width="3">
    </CircularGaugeBorder>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Border Properties:**
- `Color`: Border color
- `Width`: Border thickness in pixels

### Card-Style Gauge

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge Background="white">
    <CircularGaugeBorder Color="#E0E0E0" Width="1">
    </CircularGaugeBorder>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="75">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

<style>
    .e-circulargauge {
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        border-radius: 8px;
    }
</style>
```

## Animation

Animate all gauge elements on load using the `AnimationDuration` property and control pointer-specific animation with the `CircularGaugePointerAnimation` component using `Enable` and `Duration` properties.

### Enabling Animation

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge AnimationDuration="2000">
    <CircularGaugeAxes>
        <CircularGaugeAxis Radius="80%" StartAngle="230" EndAngle="130">
            <CircularGaugePointers>
                <CircularGaugePointer Value="60" Radius="60%">
                    <CircularGaugePointerAnimation Enable="true" Duration="1500">
                    </CircularGaugePointerAnimation>
                </CircularGaugePointer>
            </CircularGaugePointers>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="30" Color="#30B32D">
                </CircularGaugeRange>
                <CircularGaugeRange Start="30" End="60" Color="#E0E0E0">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**AnimationDuration Property:**
- Value in milliseconds
- `0`: No animation (default)
- `1000-3000`: Typical duration
- Animates: Axis, ticks, labels, ranges, pointers, annotations

**Animation Sequence:**
1. Axis line draws
2. Ticks and labels appear
3. Ranges fill in
4. Pointers move to value
5. Annotations fade in

### Pointer-Only Animation

For animated pointers without animating other elements:

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="70">
                    <CircularGaugePointerAnimation Enable="true" Duration="1500">
                    </CircularGaugePointerAnimation>
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Disabling Animation

```cshtml
<!-- No gauge animation -->
<SfCircularGauge AnimationDuration="0">
</SfCircularGauge>

<!-- No pointer animation -->
<CircularGaugePointerAnimation Enable="false">
</CircularGaugePointerAnimation>
```

**When to Disable:**
- Real-time data updates
- Performance concerns
- User interactions (dragging)

## Print and Export

Export gauges as images or PDF, or print directly using the `AllowPrint`, `AllowImageExport`, and `AllowPdfExport` properties to enable export functionality and call corresponding export methods.

### Print

```cshtml
@using Syncfusion.Blazor.CircularGauge

<button @onclick="PrintGauge">Print Gauge</button>

<SfCircularGauge @ref="Gauge" AllowPrint="true">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="65">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

@code {
    SfCircularGauge Gauge;
    
    void PrintGauge()
    {
        this.Gauge.PrintAsync();
    }
}
```

**Requirements:**
- Set `AllowPrint="true"`
- Call `Print()` method
- Opens browser print dialog

### Export as Image

```cshtml
@using Syncfusion.Blazor.CircularGauge

<button @onclick="ExportPNG">Export PNG</button>
<button @onclick="ExportJPEG">Export JPEG</button>
<button @onclick="ExportSVG">Export SVG</button>

<SfCircularGauge @ref="Gauge" AllowImageExport="true">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="75">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

@code {
    SfCircularGauge Gauge;
    
    void ExportPNG()
    {
        Gauge.ExportAsync(Syncfusion.Blazor.CircularGauge.ExportType.PNG, "CircularGauge");
    }
    
    void ExportJPEG()
    {
        Gauge.ExportAsync(Syncfusion.Blazor.CircularGauge.ExportType.JPEG, "CircularGauge");
    }
    
    void ExportSVG()
    {
        Gauge.ExportAsync(Syncfusion.Blazor.CircularGauge.ExportType.SVG, "CircularGauge");
    }
}
```

**Image Export Formats:**
- `ExportType.PNG`: Best for web, supports transparency
- `ExportType.JPEG`: Smaller file size, no transparency
- `ExportType.SVG`: Vector format, scalable

**Requirements:**
- Set `AllowImageExport="true"`
- Call `Export(type, filename)`

### Export as PDF

```cshtml
@using Syncfusion.Blazor.CircularGauge

<button @onclick="ExportPDFPortrait">Export PDF (Portrait)</button>
<button @onclick="ExportPDFLandscape">Export PDF (Landscape)</button>

<SfCircularGauge @ref="Gauge" AllowPdfExport="true">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="80">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

@code {
    SfCircularGauge Gauge;
    
    void ExportPDFPortrait()
    {
        Gauge.ExportAsync(Syncfusion.Blazor.CircularGauge.ExportType.PDF, "CircularGauge");
    }
    
    void ExportPDFLandscape()
    {
        Gauge.ExportAsync(Syncfusion.Blazor.CircularGauge.ExportType.PDF, "CircularGauge");
    }
}
```

**PDF Orientation:**
- `0`: Portrait
- `1`: Landscape

**Requirements:**
- Set `AllowPdfExport="true"`
- Call `Export(ExportType.PDF, filename, orientation)`

### Export Use Cases

1. **Reports**: Export gauges for PDF reports
2. **Documentation**: Save gauge screenshots
3. **Sharing**: Share gauge images via email/messaging
4. **Archiving**: Store historical gauge states
5. **Presentations**: Include gauge images in slides

## Best Practices

### Themes
- Use consistent themes across your application
- Test with both light and dark themes
- Consider accessibility with high-contrast themes

### Dimensions
- Use percentage for responsive layouts
- Use pixels for fixed dashboards
- Maintain aspect ratio for circular gauges

### Titles
- Keep titles concise (2-4 words)
- Use descriptive, meaningful titles
- Style consistently with your brand

### Positioning
- Center by default for best visual balance
- Adjust only for specific layout needs
- Test positioning across screen sizes

### Background
- Use subtle backgrounds (light grays, whites)
- Avoid busy patterns that distract
- Ensure contrast with gauge elements

### Animation
- Enable on initial load for polish
- Disable for real-time updates
- Use moderate durations (1-2 seconds)

### Export
- Name exports descriptively
- Choose format based on use case
- Test export quality before production

## Troubleshooting

**Gauge appears cut off:**
- Increase container size
- Reduce axis radius percentage
- Adjust CenterX/CenterY positioning

**Theme not applying:**
- Verify CSS file reference in index.html
- Check file path is correct
- Clear browser cache

**Animation stuttering:**
- Reduce AnimationDuration
- Disable for performance-sensitive scenarios
- Check for other performance bottlenecks

**Export not working:**
- Verify `AllowImageExport` or `AllowPdfExport` is true
- Check browser console for errors
- Test with a simple gauge first

**Title overlaps gauge:**
- Reduce title font size
- Increase gauge height
- Adjust title positioning with CSS

**Responsive sizing issues:**
- Ensure parent container has explicit dimensions
- Use percentage for Width/Height
- Test across different screen sizes
