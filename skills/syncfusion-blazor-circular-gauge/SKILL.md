---
name: syncfusion-blazor-circular-gauge
description: Implement Syncfusion Blazor Circular Gauge (SfCircularGauge) for radial data visualization. Use this when creating speedometers, tachometers, KPI dashboards, or circular progress indicators. This skill covers needle pointers, gauge ranges, annotations, and circular metric visualization in Blazor applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Data Visualization"
---

# Implementing Syncfusion Blazor CircularGauge

**NuGet:** `Syncfusion.Blazor.CircularGauge` + `Syncfusion.Blazor.Themes`  
**Namespace:** `Syncfusion.Blazor.CircularGauge`

A comprehensive guide for implementing circular gauge components in Blazor applications. CircularGauge provides radial data visualization with customizable axes, pointers, ranges, annotations, and interactive features.

## When to Use This Skill

Use this skill when you need to:

- **Dashboard Gauges**: Create KPI dashboards with circular metric displays
- **Speedometers/Tachometers**: Build vehicle instrument panels or speed indicators
- **Temperature/Pressure Indicators**: Display environmental or industrial sensor data
- **Progress Visualization**: Show circular progress indicators or completion status
- **Status Displays**: Create color-coded status gauges with ranges (red/yellow/green zones)
- **Multi-Metric Displays**: Show multiple values with multiple pointers on a single gauge
- **Real-Time Monitoring**: Display live data updates in a circular format
- **Interactive Gauges**: Allow users to adjust values by dragging pointers
- **Radial KPIs**: Visualize performance metrics in a radial/circular layout

## Component Overview

The Syncfusion Blazor CircularGauge (`SfCircularGauge`) is a data visualization component that displays quantitative information in a circular arc. It supports multiple pointer types, color-coded ranges, custom annotations, legends, and rich interactivity.

**Key Capabilities:**
- Multiple pointer types (needle, range bar, marker)
- Configurable axes with custom ranges and angles
- Color-coded ranges for visual zones
- Annotations with text, images, or HTML content
- Tooltips and user interactions
- Pointer dragging for value adjustment
- Legends for ranges
- Animations and transitions
- Globalization and RTL support
- Accessibility features (WCAG compliant)
- Print and export functionality

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

Read this reference when you need to:
- Install NuGet packages and set up the component
- Configure services and CSS/theme imports
- Create your first basic circular gauge
- Understand project structure and prerequisites
- Run the application with CircularGauge

### Axes Configuration
📄 **Read:** [references/axes-configuration.md](references/axes-configuration.md)

Read this reference when you need to:
- Customize axis appearance (line width, color, background)
- Set minimum and maximum values for the scale
- Configure axis radius and positioning
- Set start and end angles (semi-circle, quarter-circle, custom arcs)
- Configure axis direction (clockwise/anticlockwise)
- Customize labels (format, font, position, rotation, custom text)
- Configure major and minor ticks (intervals, size, styling)
- Implement multiple axes on a single gauge

### Pointers
📄 **Read:** [references/pointers.md](references/pointers.md)

Read this reference when you need to:
- Add needle pointers with customizable appearance
- Configure needle cap/knob and tail styling
- Implement range bar pointers with gradients
- Add marker pointers (circle, rectangle, triangle, diamond, image)
- Set pointer values and animate pointer movement
- Use multiple pointers of different types on one axis
- Enable pointer dragging for interactive value adjustment
- Position and layer pointers correctly

### Ranges
📄 **Read:** [references/ranges.md](references/ranges.md)

Read this reference when you need to:
- Create color-coded zones on the gauge (e.g., red/yellow/green)
- Define range start and end values
- Customize range colors, gradients, and styling
- Configure range positioning (radius and thickness)
- Implement multiple overlapping ranges
- Add rounded corners to ranges
- Use ranges for status indicators or zone marking

### Annotations
📄 **Read:** [references/annotations.md](references/annotations.md)

Read this reference when you need to:
- Add text annotations to display values or labels
- Insert HTML content or images as annotations
- Position annotations using angle, radius, or X/Y coordinates
- Style annotations (font, color, background, borders)
- Create multiple annotations on a single gauge
- Display dynamic content (e.g., current pointer value)
- Add conditional annotations based on gauge state
- Center labels showing gauge readings

### Legend
📄 **Read:** [references/legend.md](references/legend.md)

Read this reference when you need to:
- Enable legends for gauge ranges
- Position legends (top, bottom, left, right, custom)
- Customize legend appearance (font, colors, shapes, borders)
- Implement interactive legends with toggle functionality
- Create custom legend templates
- Handle legend click events

### Appearance and Styling
📄 **Read:** [references/appearance.md](references/appearance.md)

Read this reference when you need to:
- Apply built-in themes (Material, Bootstrap, Fluent, etc.)
- Customize colors, backgrounds, and borders
- Configure gauge dimensions (width, height, responsive sizing)
- Add and style gauge titles
- Configure center X/Y positioning
- Set up animations (duration, easing)
- Implement print and export functionality (image/PDF)
- Apply custom styling and CSS classes

### User Interaction
📄 **Read:** [references/user-interaction.md](references/user-interaction.md)

Read this reference when you need to:
- Enable and customize tooltips
- Create custom tooltip templates
- Enable pointer dragging for user input
- Handle drag events (DragStart, DragMove, DragEnd)
- Implement gauge events (Load, Loaded, AnimationComplete, etc.)
- Handle AxisLabelRendering and TooltipRendering events
- Restrict pointer drag ranges
- Create interactive gauges (adjustable temperature, speed controls)

### Globalization
📄 **Read:** [references/globalization.md](references/globalization.md)

Read this reference when you need to:
- Format numbers based on locale
- Implement custom number patterns
- Localize labels and text
- Enable RTL (right-to-left) support
- Format currency or date/time values
- Integrate CLDR data for internationalization

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

Read this reference when you need to:
- Use methods (SetPointerValue, SetAnnotationValue, Refresh, Print, Export)
- Update pointer values dynamically at runtime
- Optimize performance for complex gauges
- Place gauges inside other components (grids, dashboards, tabs)
- Implement real-time data updates with timers or SignalR
- Configure accessibility features (WCAG, keyboard navigation, screen readers)
- Implement responsive design patterns
- Handle server-side vs WebAssembly considerations
- Test and troubleshoot complex gauge scenarios

## Quick Start Example

Here's a basic circular gauge with a needle pointer:

```cshtml
@page "/circular-gauge"
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="120">
            <CircularGaugePointers>
                <CircularGaugePointer Value="65" Color="#007DD1">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Prerequisites:**
1. Install `Syncfusion.Blazor.CircularGauge` and `Syncfusion.Blazor.Themes` NuGet packages
2. Register services: `builder.Services.AddSyncfusionBlazor();`
3. Import theme CSS in `App.razor` or `index.html`

## Common Patterns

### Pattern 1: Speedometer Gauge

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="120" StartAngle="220" EndAngle="140">
            <CircularGaugeAxisLabelStyle>
                <CircularGaugeAxisLabelFont Size="12px"></CircularGaugeAxisLabelFont>
            </CircularGaugeAxisLabelStyle>
            <CircularGaugeAxisLineStyle Width="10" Color="#E0E0E0">
            </CircularGaugeAxisLineStyle>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="40" Color="#30B32D"></CircularGaugeRange>
                <CircularGaugeRange Start="40" End="80" Color="#FFDD00"></CircularGaugeRange>
                <CircularGaugeRange Start="80" End="120" Color="#F03E3E"></CircularGaugeRange>
            </CircularGaugeRanges>
            <CircularGaugePointers>
                <CircularGaugePointer Value="65" Radius="60%" Color="#757575">
                    <CircularGaugePointerAnimation Enable="true" Duration="1500">
                    </CircularGaugePointerAnimation>
                    <CircularGaugeCap Radius="7">
                        <CircularGaugeCapBorder Width="3" Color="#757575">
                        </CircularGaugeCapBorder>
                    </CircularGaugeCap>
                    <CircularGaugeNeedleTail Length="18%" Color="#757575">
                    </CircularGaugeNeedleTail>
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Pattern 2: Temperature Indicator

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="-20" Maximum="50">
            <CircularGaugeAxisLineStyle Width="0">
            </CircularGaugeAxisLineStyle>
            <CircularGaugeAxisMajorTicks Height="0"></CircularGaugeAxisMajorTicks>
            <CircularGaugeAxisMinorTicks Height="0"></CircularGaugeAxisMinorTicks>
            <CircularGaugeAxisLabelStyle>
                <CircularGaugeAxisLabelFont Size="0px"></CircularGaugeAxisLabelFont>
            </CircularGaugeAxisLabelStyle>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="-20" End="0" Color="#6495ED" Radius="90%" StartWidth="30" EndWidth="30">
                </CircularGaugeRange>
                <CircularGaugeRange Start="0" End="20" Color="#FFA500" Radius="90%" StartWidth="30" EndWidth="30">
                </CircularGaugeRange>
                <CircularGaugeRange Start="20" End="50" Color="#FF4500" Radius="90%" StartWidth="30" EndWidth="30">
                </CircularGaugeRange>
            </CircularGaugeRanges>
            <CircularGaugePointers>
                <CircularGaugePointer Value="22" Radius="0%" Type="PointerType.Marker" 
                                      MarkerShape="GaugeShape.Triangle" MarkerHeight="20" MarkerWidth="20" 
                                      Color="#333">
                </CircularGaugePointer>
            </CircularGaugePointers>
            <CircularGaugeAnnotations>
                <CircularGaugeAnnotation Angle="0" Radius="0%" ZIndex="1">
                    <ContentTemplate>
                        <div style="font-size:24px;font-weight:bold;color:#333;">22°C</div>
                    </ContentTemplate>
                </CircularGaugeAnnotation>
            </CircularGaugeAnnotations>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Pattern 3: Multi-Pointer Gauge

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugePointers>
                <CircularGaugePointer Value="30" Color="#007DD1" PointerWidth="8" Radius="70%">
                </CircularGaugePointer>
                <CircularGaugePointer Value="60" Color="#E5CE20" PointerWidth="8" Radius="80%">
                </CircularGaugePointer>
                <CircularGaugePointer Value="90" Color="#F44336" PointerWidth="8" Radius="90%">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Pattern 4: Range-Based Status Gauge with Legend

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeLegendSettings Visible="true" Position="Syncfusion.Blazor.CircularGauge.LegendPosition.Bottom">
    </CircularGaugeLegendSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="33" Color="#30B32D" LegendText="Good">
                </CircularGaugeRange>
                <CircularGaugeRange Start="33" End="66" Color="#FFDD00" LegendText="Warning">
                </CircularGaugeRange>
                <CircularGaugeRange Start="66" End="100" Color="#F03E3E" LegendText="Critical">
                </CircularGaugeRange>
            </CircularGaugeRanges>
            <CircularGaugePointers>
                <CircularGaugePointer Value="75" Type="PointerType.RangeBar" Radius="60%" 
                                      PointerWidth="15" Color="#F03E3E">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Key Props and Configuration

### API Reference (summary)

#### Namespace / Import

- `@using Syncfusion.Blazor.CircularGauge`

#### Component: `SfCircularGauge`

##### Core Properties
- `AllowImageExport` (bool)
- `AllowMargin` (bool)
- `AllowPdfExport` (bool)
- `AllowPrint` (bool)
- `AnimationDuration` (double) — animation duration in milliseconds
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

##### Important Methods
- `Task<string> ExportAsync(ExportType type, string fileName, PdfPageOrientation? orientation = null, bool allowDownload = true)`
- `Task PrintAsync()`
- `Task RefreshAsync()`
- `Task SetAnnotationValueAsync(int axisIndex, int annotationIndex, string content)`
- `Task SetPointerValueAsync(int axisIndex, int pointerIndex, double pointerValue)`
- `void SetRangeValue(int axisIndex, int rangeIndex, double start, double end)`
- `void UpdateChildProperties(string key, object keyValue)`

##### Events (common)
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

#### Child Tags and Key Properties
- `CircularGaugeAnnotation` — `Content`, `Angle`, `Radius`, `AutoAngle`
- `CircularGaugeAxes` — collection of `CircularGaugeAxis`
- `CircularGaugeAxis` — `Background`, `Direction`, `EndAngle`, `StartAngle`, `Maximum`, `Minimum`, `Radius`
- `CircularGaugeBorder` — `Color`, `Width`
- `CircularGaugeLegendSettings` — `Visible`, `Position`, `Shape`, `ToggleVisibility`
- `CircularGaugeMargin` — `Top`, `Bottom`, `Left`, `Right`
- `CircularGaugePointer` — `Type`, `Value`, `Color`, `MarkerShape`, `Position`, `PointerWidth`, `Radius`
- `CircularGaugeRange` — `Start`, `End`, `Color`, `StartWidth`, `EndWidth`, `Radius`
- `CircularGaugeTooltipSettings` — `Enable`, `Fill`, `Format`, `ShowAtMousePosition`

#### Styling CSS classes
- `e-circulargauge`
- `e-gauge-axis`
- `e-gauge-pointer`
- `e-gauge-range`
- `e-gauge-annotation`

#### Typical Usage Snippet

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
                <CircularGaugeRange Start="0" End="50" Color="#3A5998" StartWidth="10" EndWidth="10">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="100" Color="#33BCDA" StartWidth="10" EndWidth="10">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Common Use Cases

1. **Dashboard KPI Displays**: Show business metrics like sales performance, customer satisfaction scores, or completion percentages
2. **Vehicle Instruments**: Create speedometers, tachometers, fuel gauges, or battery indicators
3. **Environmental Monitoring**: Display temperature, humidity, pressure, or air quality readings
4. **Industrial Controls**: Show machine status, pressure levels, RPM, or load percentages
5. **Health & Fitness**: Visualize heart rate, step count, calorie burn, or workout intensity
6. **Resource Utilization**: Display CPU usage, memory consumption, disk space, or network bandwidth
7. **Progress Tracking**: Show project completion, task progress, or milestone achievement
8. **Quality Metrics**: Display defect rates, test pass rates, or compliance scores
9. **Financial Indicators**: Show portfolio performance, risk levels, or budget utilization
10. **Interactive Settings**: Allow users to adjust thermostat settings, volume levels, or brightness controls

## Related Skills

- [Implementing Linear Gauges](../syncfusion-blazor-linear-gauge/) - For horizontal/vertical linear gauge displays

---

**Next Steps:** Read the appropriate reference file based on your implementation needs. Start with `getting-started.md` for new implementations, or navigate directly to specific feature documentation.
