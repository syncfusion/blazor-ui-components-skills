# Blazor Linear Gauge API Reference

Complete API documentation for the Syncfusion Blazor Linear Gauge component. This reference covers all properties, methods, events, and component classes available in the `Syncfusion.Blazor.LinearGauge` namespace.

**Official Documentation:** [Syncfusion Blazor Linear Gauge API](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.LinearGauge.html)

## Table of Contents

- [SfLinearGauge Component](#sflineargauge-component)
- [Methods](#methods)
- [Properties](#properties)
- [Axes API](#axes-api)
- [Pointers API](#pointers-api)
- [Ranges API](#ranges-api)
- [Annotations API](#annotations-api)
- [Tooltips API](#tooltips-api)
- [Gradients API](#gradients-api)
- [Events API](#events-api)
- [Enums](#enums)
- [Event Arguments](#event-arguments)
- [Complete Component List](#complete-component-list)

---

## SfLinearGauge Component

The root component that hosts all gauge elements. This is the main container for the linear gauge visualization.

### Methods

All methods are asynchronous and must be awaited.

#### SetPointerValueAsync

Updates a pointer's value programmatically.

```csharp
public async Task SetPointerValueAsync(int axisIndex, int pointerIndex, double value)
```

**Parameters:**
- `axisIndex` (int): Zero-based index of the axis
- `pointerIndex` (int): Zero-based index of the pointer on that axis
- `value` (double): New pointer value

**Example:**
```razor
await gauge.SetPointerValueAsync(0, 0, 85);  // Set first pointer of first axis to 85
```

**Use Cases:**
- Real-time sensor data updates
- Progress tracking
- Live metrics display
- Data synchronization

---

#### SetAnnotationValueAsync

Updates an annotation's content dynamically.

```csharp
public async Task SetAnnotationValueAsync(int annotationIndex, string content)
```

**Parameters:**
- `annotationIndex` (int): Zero-based index of the annotation
- `content` (string): New HTML content for the annotation

**Example:**
```razor
await gauge.SetAnnotationValueAsync(0, "<div style='color: red;'>Warning!</div>");
```

**Use Cases:**
- Dynamic status labels
- Live value displays
- Contextual messages
- Conditional alerts

---

#### RefreshAsync

Refreshes the entire gauge to apply configuration changes.

```csharp
public async Task RefreshAsync()
```

**Use Cases:**
- After programmatic property changes
- Theme switching
- Configuration updates
- Responsive layout adjustments

**Example:**
```razor
backgroundColor = "#2c3e50";
await gauge.RefreshAsync();
```

---

#### PrintAsync

Prints the gauge to paper or PDF.

```csharp
public async Task PrintAsync()
```

**Prerequisites:**
- Must set `AllowPrint="true"` on SfLinearGauge component

**Example:**
```razor
@using Syncfusion.Blazor.LinearGauge

<button @onclick="PrintGauge">Print Gauge</button>

<SfLinearGauge @ref="gauge" AllowPrint="true">
    <!-- gauge content -->
</SfLinearGauge>

@code {
    SfLinearGauge gauge;
    
    private async Task PrintGauge()
    {
        await gauge.PrintAsync();
    }
}
```

---

#### ExportAsync

Exports the gauge to PNG, SVG, or PDF format.

```csharp
public async Task ExportAsync(ExportType type, string fileName, 
    PdfPageOrientation? orientation = null, bool allowDownload = true)
```

**Parameters:**
- `type` (ExportType): `PNG`, `SVG`, or `PDF`
- `fileName` (string): Name for exported file (without extension)
- `orientation` (PdfPageOrientation?): `Portrait` or `Landscape` (PDF only)
- `allowDownload` (bool): Download file to browser (default: true)

**Prerequisites:**
- `AllowPdfExport="true"` for PDF export
- `AllowImageExport="true"` for PNG/SVG export

**Examples:**
```razor
// Export as PNG
await gauge.ExportAsync(ExportType.PNG, "gauge-export");

// Export as PDF (landscape)
await gauge.ExportAsync(ExportType.PDF, "gauge-report", PdfPageOrientation.Landscape);

// Export as SVG (save without automatic download)
await gauge.ExportAsync(ExportType.SVG, "gauge-vector", null, false);
```

---

### Properties

#### Sizing & Layout

| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `Width` | string | Width of the gauge container | `"400px"`, `"100%"` |
| `Height` | string | Height of the gauge container | `"500px"`, `"80vh"` |
| `Orientation` | Orientation | `Horizontal` or `Vertical` | `Orientation.Vertical` |
| `Description` | string | Accessibility description | `"Temperature gauge"` |

#### Appearance

| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `Title` | string | Title text displayed on gauge | `"System Temperature"` |
| `TitleStyle` | LinearGaugeTitleStyle | Title font and positioning | See TitleStyle |
| `Background` | string | Background color | `"#ffffff"`, `"transparent"` |
| `Theme` | Theme | Predefined color theme | `Theme.Material`, `Theme.Bootstrap` |
| `Container` | LinearGaugeContainer | Container appearance and border | See Container API |
| `Margin` | LinearGaugeMargin | Left, right, top, bottom margins | `new LinearGaugeMargin { Left = 10 }` |

#### Behavior

| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `AnimationDuration` | double | Animation duration in milliseconds | `1000` (1 second), `0` (instant) |
| `Format` | string | Number format for axis labels | `"N2"` (2 decimal places) |
| `EnableGroupingSeparator` | bool | Show thousand separators | `true`, `false` |
| `ID` | string | Unique component identifier | `"gauge1"`, `"temp-gauge"` |
| `TabIndex` | int | Tab order for keyboard navigation | `1`, `2`, `3` |

#### Export & Print

| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `AllowPrint` | bool | Enable print functionality | `true`, `false` |
| `AllowPdfExport` | bool | Enable PDF export | `true`, `false` |
| `AllowImageExport` | bool | Enable PNG/SVG export | `true`, `false` |
| `AllowMargin` | bool | Include margins when printing/exporting | `true`, `false` |

#### Styling

| Property | Type | Description |
|----------|------|-------------|
| `RangePalettes` | string[] | Automatic color palette for ranges |

---

## Properties

### Complete Property Reference

#### SfLinearGauge Properties

```csharp
// Sizing
public string Width { get; set; }
public string Height { get; set; }

// Layout & Appearance
public Orientation Orientation { get; set; }
public string Title { get; set; }
public LinearGaugeTitleStyle TitleStyle { get; set; }
public string Background { get; set; }
public Theme Theme { get; set; }
public LinearGaugeContainer Container { get; set; }
public LinearGaugeMargin Margin { get; set; }
public LinearGaugeBorder Border { get; set; }

// Behavior
public double AnimationDuration { get; set; }
public string Format { get; set; }
public bool EnableGroupingSeparator { get; set; }

// Identification & Accessibility
public string ID { get; set; }
public int TabIndex { get; set; }
public string Description { get; set; }

// Export & Print
public bool AllowPrint { get; set; }
public bool AllowPdfExport { get; set; }
public bool AllowImageExport { get; set; }
public bool AllowMargin { get; set; }

// Collections
public LinearGaugeAxes Axes { get; set; }
public LinearGaugeAnnotations Annotations { get; set; }
public LinearGaugeTooltipSettings TooltipSettings { get; set; }
public LinearGaugeEvents Events { get; set; }
```

---

## Axes API

Configure gauge scales with multiple axes, each with its own range, ticks, labels, pointers, and ranges.

### LinearGaugeAxis

Defines a single scale on the gauge.

#### Properties

**Scale Configuration:**

```csharp
public class LinearGaugeAxis
{
    // Scale definition
    public double Minimum { get; set; }                    // Minimum value
    public double Maximum { get; set; }                    // Maximum value
    public bool IsInversed { get; set; }                   // Reverse direction
    public bool OpposedPosition { get; set; }              // Opposite side
    public bool ShowLastLabel { get; set; }                // Show last label
    
    // Child components
    public LinearGaugeLine Line { get; set; }
    public LinearGaugeMajorTicks MajorTicks { get; set; }
    public LinearGaugeMinorTicks MinorTicks { get; set; }
    public LinearGaugeAxisLabelStyle LabelStyle { get; set; }
    public LinearGaugeContainer Container { get; set; }
    public LinearGaugePointers Pointers { get; set; }
    public LinearGaugeRanges Ranges { get; set; }
    public LinearGradient LinearGradient { get; set; }
    public LinearGaugeRadialGradient RadialGradient { get; set; }
}
```

### LinearGaugeLine

Defines the axis line appearance.

```csharp
public class LinearGaugeLine
{
    public double Height { get; set; }              // Height of line
    public double Width { get; set; }               // Width of line
    public string Color { get; set; }               // Color
    public double Offset { get; set; }              // Offset from axis
}
```

### LinearGaugeMajorTicks

Major tick marks on the axis.

```csharp
public class LinearGaugeMajorTicks
{
    public double Interval { get; set; }            // Interval between ticks
    public double Height { get; set; }              // Height of tick marks
    public double Width { get; set; }               // Width of tick marks
    public string Color { get; set; }               // Color
    public Position Position { get; set; }          // Inside/Outside/Cross
    public double Offset { get; set; }              // Offset from axis
}
```

### LinearGaugeMinorTicks

Minor tick marks on the axis.

```csharp
public class LinearGaugeMinorTicks
{
    public double Interval { get; set; }            // Interval between ticks
    public double Height { get; set; }              // Height of tick marks
    public double Width { get; set; }               // Width of tick marks
    public string Color { get; set; }               // Color
    public Position Position { get; set; }          // Position relative to axis
    public double Offset { get; set; }              // Offset from axis
}
```

### LinearGaugeAxisLabelStyle

Axis label styling and positioning.

```csharp
public class LinearGaugeAxisLabelStyle
{
    public string Format { get; set; }              // Format: "{value}°C"
    public Position Position { get; set; }          // Label position
    public double Offset { get; set; }              // Distance from axis
    public string FontFamily { get; set; }          // Font family
    public string Color { get; set; }               // Label color
    public double Opacity { get; set; }             // Opacity (0-1)
}
```

### LinearGaugeAxisLabelFont

Font properties for axis labels.

```csharp
public class LinearGaugeAxisLabelFont
{
    public string FontFamily { get; set; }          // Font family name
    public string FontSize { get; set; }            // Font size (e.g., "14px")
    public string FontStyle { get; set; }           // normal, italic, oblique
    public string FontWeight { get; set; }          // normal, bold, 100-900
}
```

---

## Pointers API

Configure value indicators (pointers) on the gauge.

### LinearGaugePointer

Represents a single pointer/indicator on an axis.

```csharp
public class LinearGaugePointer
{
    // Value & Type
    public double PointerValue { get; set; }                    // Pointer value
    public PointerType Type { get; set; }                       // Marker or Bar
    public MarkerType MarkerType { get; set; }                  // Circle, Rectangle, etc.
    
    // Appearance
    public string Color { get; set; }                           // Pointer color
    public double Width { get; set; }                           // Pointer width
    public double Height { get; set; }                          // Pointer height
    public double Opacity { get; set; }                         // Opacity (0-1)
    public double Offset { get; set; }                          // Offset from axis
    public Placement Placement { get; set; }                    // Center, Near, Far
    public double RoundedCornerRadius { get; set; }             // Corner radius
    
    // Content
    public string ImageUrl { get; set; }                        // Image pointer URL
    public string Text { get; set; }                            // Text pointer content
    
    // Interaction
    public bool EnableDrag { get; set; }                        // Allow dragging
    public double AnimationDuration { get; set; }               // Animation duration
    
    // Styling
    public LinearGaugePointerBorder Border { get; set; }
    public LinearGaugeMarkerTextStyle TextStyle { get; set; }
}
```

### LinearGaugePointerBorder

Pointer border styling.

```csharp
public class LinearGaugePointerBorder
{
    public string Color { get; set; }               // Border color
    public double Width { get; set; }               // Border width
}
```

### LinearGaugeMarkerTextStyle

Text styling for text pointers.

```csharp
public class LinearGaugeMarkerTextStyle
{
    public string FontFamily { get; set; }          // Font family
    public string FontSize { get; set; }            // Font size
    public string Color { get; set; }               // Text color
    public string FontWeight { get; set; }          // Font weight
    public double Opacity { get; set; }             // Text opacity
}
```

---

## Ranges API

Configure color-coded value zones on the gauge.

### LinearGaugeRange

Represents a single colored range.

```csharp
public class LinearGaugeRange
{
    // Range Definition
    public double Start { get; set; }               // Start value
    public double End { get; set; }                 // End value
    
    // Appearance
    public string Color { get; set; }               // Background color
    public double StartWidth { get; set; }          // Width at start
    public double EndWidth { get; set; }            // Width at end
    public Position Position { get; set; }          // Inside/Outside/Cross
    public double Offset { get; set; }              // Offset from axis
    
    // Styling
    public LinearGaugeRangeBorder Border { get; set; }
    public LinearGaugeRangeTooltipSettings TooltipSettings { get; set; }
}
```

### LinearGaugeRangeBorder

Range border styling.

```csharp
public class LinearGaugeRangeBorder
{
    public string Color { get; set; }               // Border color
    public double Width { get; set; }               // Border width
}
```

### LinearGaugeRangeTooltipSettings

Tooltip shown when hovering over a range.

```csharp
public class LinearGaugeRangeTooltipSettings
{
    public bool Enable { get; set; }                // Enable tooltip
    public string Format { get; set; }              // Format: "{value}%"
    public TooltipPosition Position { get; set; }   // Tooltip position
    public string Fill { get; set; }                // Background color
    public double Opacity { get; set; }             // Opacity
    public LinearGaugeRangeTooltipTextStyle TextStyle { get; set; }
    public LinearGaugeRangeTooltipBorder Border { get; set; }
}
```

---

## Annotations API

Overlay text, HTML, or templated content on the gauge.

### LinearGaugeAnnotation

Represents a single annotation overlay.

```csharp
public class LinearGaugeAnnotation
{
    // Content & Positioning
    public double AxisValue { get; set; }           // Position on axis
    public int AxisIndex { get; set; }              // Axis index
    public double X { get; set; }                   // X pixel offset
    public double Y { get; set; }                   // Y pixel offset
    public int ZIndex { get; set; }                 // Layer order
    public Alignment HorizontalAlignment { get; set; }  // Horizontal align
    public Alignment VerticalAlignment { get; set; }    // Vertical align
    
    // Content
    public string Content { get; set; }             // Text/HTML content
    public RenderFragment ContentTemplate { get; set; }  // Template content
    
    // Styling
    public LinearGaugeAnnotationFont Font { get; set; }
}
```

### LinearGaugeAnnotationFont

Font properties for annotations.

```csharp
public class LinearGaugeAnnotationFont
{
    public string FontFamily { get; set; }          // Font family
    public string FontSize { get; set; }            // Font size
    public string Color { get; set; }               // Text color
    public string FontWeight { get; set; }          // Font weight
    public double Opacity { get; set; }             // Opacity
}
```

---

## Tooltips API

Configure tooltips for pointers and ranges.

### LinearGaugeTooltipSettings

Tooltip configuration for pointers.

```csharp
public class LinearGaugeTooltipSettings
{
    public bool Enable { get; set; }                        // Enable tooltips
    public string Format { get; set; }                      // Format: "{value}°C"
    public TooltipPosition Position { get; set; }           // Top/Bottom/Left/Right
    public string Fill { get; set; }                        // Background color
    public double Opacity { get; set; }                     // Opacity (0-1)
    public bool ShowAtMousePosition { get; set; }           // Show at cursor
    public LinearGaugeTooltipTextStyle TextStyle { get; set; }  // Text styling
    public LinearGaugeTooltipBorder Border { get; set; }    // Border styling
}
```

### LinearGaugeTooltipTextStyle

Tooltip text styling.

```csharp
public class LinearGaugeTooltipTextStyle
{
    public string FontFamily { get; set; }          // Font family
    public string FontSize { get; set; }            // Font size
    public string Color { get; set; }               // Text color
    public string FontWeight { get; set; }          // Font weight
    public double Opacity { get; set; }             // Opacity
}
```

### LinearGaugeTooltipBorder

Tooltip border styling.

```csharp
public class LinearGaugeTooltipBorder
{
    public string Color { get; set; }               // Border color
    public double Width { get; set; }               // Border width
}
```

---

## Gradients API

Apply gradient fills to gauge elements.

### LinearGaugeLinearGradient

Linear gradient configuration.

```csharp
public class LinearGaugeLinearGradient
{
    public double StartValue { get; set; }          // Start position
    public double EndValue { get; set; }            // End position
    public ColorStops ColorStops { get; set; }      // Gradient stops
}
```

### LinearGaugeRadialGradient

Radial gradient configuration.

```csharp
public class LinearGaugeRadialGradient
{
    public string Radius { get; set; }              // Gradient radius
    public InnerPosition InnerPosition { get; set; }        // Inner circle
    public OuterPosition OuterPosition { get; set; }        // Outer circle
    public ColorStops ColorStops { get; set; }      // Gradient stops
}
```

### ColorStop

Individual gradient color stop.

```csharp
public class ColorStop
{
    public string Offset { get; set; }              // Position: "0%", "50%", "100%"
    public string Color { get; set; }               // Color at position
    public double Opacity { get; set; }             // Opacity at position
}
```

---

## Events API

Handle gauge interactions and lifecycle events.

### LinearGaugeEvents

Event handler component.

```razor
<LinearGaugeEvents 
    Load="OnLoad"
    Loaded="OnLoaded"
    AxisLabelRendering="OnAxisLabelRendering"
    AnnotationRendering="OnAnnotationRendering"
    ValueChange="OnValueChange"
    TooltipRendering="OnTooltipRendering"
    DragStart="OnDragStart"
    DragMove="OnDragMove"
    DragEnd="OnDragEnd"
    AnimationComplete="OnAnimationComplete"
    ResizeCompleted="OnResizeCompleted"
    OnGaugeMouseDown="OnMouseDown"
    OnGaugeMouseLeave="OnMouseLeave"
    OnGaugeMouseUp="OnMouseUp"
    Print="OnPrint">
</LinearGaugeEvents>
```

### Event Reference Table

| Event | Argument Type | Fires When | Properties |
|-------|---------------|-----------|-----------|
| `Load` | `LoadEventArgs` | Before gauge renders | `Cancel` (bool) |
| `Loaded` | `LoadedEventArgs` | After gauge renders | None specific |
| `AxisLabelRendering` | `AxisLabelRenderEventArgs` | Before each label renders | `Text`, `Value`, `Axis` |
| `AnnotationRendering` | `AnnotationRenderEventArgs` | Before annotation renders | `Content`, `Annotation` |
| `ValueChange` | `ValueChangeEventArgs` | Pointer value changes | `Value`, `PointerIndex`, `AxisIndex` |
| `TooltipRendering` | `TooltipRenderEventArgs` | Before tooltip displays | `Content`, `Pointer` |
| `DragStart` | `PointerDragEventArgs` | User starts dragging pointer | `CurrentValue`, `PointerIndex`, `AxisIndex` |
| `DragMove` | `PointerDragEventArgs` | During pointer drag | `CurrentValue`, `PointerIndex`, `AxisIndex` |
| `DragEnd` | `PointerDragEventArgs` | User releases pointer | `CurrentValue`, `PointerIndex`, `AxisIndex` |
| `AnimationComplete` | `AnimationCompleteEventArgs` | Animation finishes | None specific |
| `ResizeCompleted` | `ResizeEventArgs` | Gauge resized | `CurrentSize`, `PreviousSize` |
| `OnGaugeMouseDown` | `MouseEventArgs` | Mouse down on gauge | Standard mouse properties |
| `OnGaugeMouseLeave` | `MouseEventArgs` | Mouse leaves gauge | Standard mouse properties |
| `OnGaugeMouseUp` | `MouseEventArgs` | Mouse up on gauge | Standard mouse properties |
| `Print` | `PrintEventArgs` | Before printing | None specific |

### Event Handler Examples

```csharp
// Load event - can cancel rendering
private void OnLoad(LoadEventArgs args)
{
    if (someCondition)
    {
        args.Cancel = true;  // Prevent rendering
    }
}

// Value change - track pointer updates
private void OnValueChange(ValueChangeEventArgs args)
{
    double newValue = args.Value;
    int pointerIndex = args.PointerIndex;
    int axisIndex = args.AxisIndex;
}

// Axis label rendering - customize labels
private void OnAxisLabelRendering(AxisLabelRenderEventArgs args)
{
    args.Text = args.Text + "°C";  // Add unit
}

// Drag events - track user interaction
private void OnDragStart(PointerDragEventArgs args)
{
    Console.WriteLine($"Started dragging from {args.CurrentValue}");
}

private void OnDragMove(PointerDragEventArgs args)
{
    double currentValue = args.CurrentValue;
    // Live update feedback
}

private void OnDragEnd(PointerDragEventArgs args)
{
    double finalValue = args.CurrentValue;
    // Save or process final value
}

// Tooltip rendering - customize tooltip content
private void OnTooltipRendering(TooltipRenderEventArgs args)
{
    args.Content = $"Temperature: {args.Content}°C";
}
```

---

## Enums

### Orientation

Gauge layout direction.

```csharp
public enum Orientation
{
    Horizontal,      // Horizontal scale (default)
    Vertical         // Vertical scale
}
```

### Position

Element positioning relative to axis line.

```csharp
public enum Position
{
    Inside,          // Inside the axis
    Outside,         // Outside the axis
    Cross            // Crossing the axis line
}
```

### PointerType

Type of value indicator.

```csharp
public enum PointerType
{
    Marker,          // Shaped marker (circle, square, etc.)
    Bar              // Bar/rectangle pointer
}
```

### MarkerType

Shape of marker pointer.

```csharp
public enum MarkerType
{
    Circle,                // Circle marker
    Rectangle,             // Rectangle marker
    Triangle,              // Triangle marker
    Diamond,               // Diamond marker
    InvertedTriangle       // Inverted triangle marker
}
```

### Placement

Pointer position relative to axis.

```csharp
public enum Placement
{
    Center,          // Centered on axis (default)
    Near,            // Near side
    Far              // Far side
}
```

### ContainerType

Container shape type.

```csharp
public enum ContainerType
{
    Normal,                 // Rectangular container (default)
    RoundedRectangle,       // Rounded rectangle
    Thermometer             // Thermometer shape
}
```

### ExportType

Export file format.

```csharp
public enum ExportType
{
    PNG,             // PNG image format
    SVG,             // SVG vector format
    PDF              // PDF document format
}
```

### TooltipPosition

Tooltip placement relative to pointer.

```csharp
public enum TooltipPosition
{
    Top,             // Above pointer
    Bottom,          // Below pointer
    Left,            // Left of pointer
    Right            // Right of pointer
}
```

### Theme

Predefined color themes.

```csharp
public enum Theme
{
    Bootstrap,                  // Bootstrap theme
    Material,                   // Material Design
    Fabric,                     // Fabric theme
    Fluent,                     // Fluent Design
    Tailwind,                   // Tailwind theme
    Bootstrap5,                 // Bootstrap 5
    FluentDark,                 // Fluent Dark theme
    MaterialDark,               // Material Dark
    BootstrapDark,              // Bootstrap Dark
    TailwindDark,               // Tailwind Dark
    // ... additional themes
}
```

### Alignment

Horizontal/Vertical alignment.

```csharp
public enum Alignment
{
    Near,            // Start/left alignment
    Center,          // Center alignment
    Far              // End/right alignment
}
```

### PdfPageOrientation

PDF page orientation.

```csharp
public enum PdfPageOrientation
{
    Portrait,        // Vertical orientation
    Landscape        // Horizontal orientation
}
```

---

## Event Arguments

Complete details of all event argument classes.

### LoadEventArgs

Fired before gauge rendering.

```csharp
public class LoadEventArgs
{
    public bool Cancel { get; set; }        // Set to true to cancel rendering
}
```

### LoadedEventArgs

Fired after gauge successfully renders.

```csharp
public class LoadedEventArgs
{
    // No specific properties - just indicates gauge is ready
}
```

### ValueChangeEventArgs

Fired when pointer value changes.

```csharp
public class ValueChangeEventArgs
{
    public double Value { get; set; }               // New pointer value
    public int PointerIndex { get; set; }           // Pointer index
    public int AxisIndex { get; set; }              // Axis index
}
```

### AxisLabelRenderEventArgs

Fired before axis label rendering.

```csharp
public class AxisLabelRenderEventArgs
{
    public string Text { get; set; }                // Label text (modifiable)
    public double Value { get; set; }               // Numeric value
    public LinearGaugeAxis Axis { get; set; }       // Axis configuration
}
```

### AnnotationRenderEventArgs

Fired before annotation rendering.

```csharp
public class AnnotationRenderEventArgs
{
    public string Content { get; set; }             // Annotation content (modifiable)
    public LinearGaugeAnnotation Annotation { get; set; }  // Annotation config
}
```

### TooltipRenderEventArgs

Fired before tooltip display.

```csharp
public class TooltipRenderEventArgs
{
    public string Content { get; set; }             // Tooltip content (modifiable)
    public LinearGaugePointer Pointer { get; set; } // Pointer configuration
}
```

### PointerDragEventArgs

Fired during pointer drag (DragStart, DragMove, DragEnd).

```csharp
public class PointerDragEventArgs
{
    public double CurrentValue { get; set; }        // Current pointer value
    public int PointerIndex { get; set; }           // Pointer index
    public int AxisIndex { get; set; }              // Axis index
}
```

### ResizeEventArgs

Fired after gauge resize.

```csharp
public class ResizeEventArgs
{
    public Size CurrentSize { get; set; }           // New gauge size
    public Size PreviousSize { get; set; }          // Previous gauge size
}
```

### MouseEventArgs

Fired for mouse events.

```csharp
public class MouseEventArgs
{
    // Standard mouse event properties
    public int ClientX { get; set; }
    public int ClientY { get; set; }
    public int OffsetX { get; set; }
    public int OffsetY { get; set; }
    // ... and other standard properties
}
```

### PrintEventArgs

Fired before printing.

```csharp
public class PrintEventArgs
{
    // Event context for print operation
}
```

### AnimationCompleteEventArgs

Fired when animation completes.

```csharp
public class AnimationCompleteEventArgs
{
    // Event context for animation completion
}
```

---

## Complete Component List

All classes in the `Syncfusion.Blazor.LinearGauge` namespace.

### Core Components (44 classes)

1. **SfLinearGauge** - Main gauge component
2. **LinearGaugeAnnotation** - Individual annotation
3. **LinearGaugeAnnotationFont** - Annotation font styling
4. **LinearGaugeAnnotations** - Annotation collection
5. **LinearGaugeAxes** - Axis collection
6. **LinearGaugeAxis** - Individual axis
7. **LinearGaugeAxisLabelFont** - Axis label font
8. **LinearGaugeAxisLabelStyle** - Axis label styling
9. **LinearGaugeBorder** - Main gauge border
10. **LinearGaugeBorderSettings** - Border settings
11. **LinearGaugeContainer** - Container configuration
12. **LinearGaugeContainerBorder** - Container border
13. **LinearGaugeEvents** - Event handlers
14. **LinearGaugeFontSettings** - Font configuration
15. **LinearGaugeLine** - Axis line
16. **LinearGaugeLinearGradient** - Linear gradient
17. **LinearGaugeMajorTicks** - Major tick marks
18. **LinearGaugeMargin** - Margin configuration
19. **LinearGaugeMarginSettings** - Margin settings
20. **LinearGaugeMarkerTextStyle** - Marker text styling
21. **LinearGaugeMinorTicks** - Minor tick marks
22. **LinearGaugePointer** - Individual pointer
23. **LinearGaugePointerBorder** - Pointer border
24. **LinearGaugePointers** - Pointer collection
25. **LinearGaugeRadialGradient** - Radial gradient
26. **LinearGaugeRange** - Individual range
27. **LinearGaugeRangeBorder** - Range border
28. **LinearGaugeRangeTooltipBorder** - Range tooltip border
29. **LinearGaugeRangeTooltipSettings** - Range tooltip
30. **LinearGaugeRangeTooltipTextStyle** - Range tooltip text
31. **LinearGaugeRanges** - Range collection
32. **LinearGaugeTickSettings** - Tick configuration
33. **LinearGaugeTitleStyle** - Title styling
34. **LinearGaugeTooltipBorder** - Tooltip border
35. **LinearGaugeTooltipSettings** - Tooltip configuration
36. **LinearGaugeTooltipTextStyle** - Tooltip text styling
37. **LinearGaugeLinearGradient** - Linear gradient definition
38. **LinearGaugeRadialGradient** - Radial gradient definition
39. **ColorStop** - Gradient color stop
40. **ColorStops** - Gradient color stops collection
41. **Point** - Pointer type enum
42. **GradientPosition** - Gradient position
43. **InnerPosition** - Inner gradient position
44. **OuterPosition** - Outer gradient position

### Event Argument Classes (10 classes)

1. **LoadEventArgs** - Load event arguments
2. **LoadedEventArgs** - Loaded event arguments
3. **AxisLabelRenderEventArgs** - Axis label rendering arguments
4. **AnnotationRenderEventArgs** - Annotation rendering arguments
5. **ValueChangeEventArgs** - Value change arguments
6. **TooltipRenderEventArgs** - Tooltip rendering arguments
7. **PointerDragEventArgs** - Pointer drag arguments
8. **ResizeEventArgs** - Resize event arguments
9. **MouseEventArgs** - Mouse event arguments
10. **PrintEventArgs** - Print event arguments
11. **AnimationCompleteEventArgs** - Animation complete arguments

### Enum Types (12 enums)

1. **Orientation** - Horizontal/Vertical
2. **Position** - Inside/Outside/Cross
3. **PointerType** - Marker/Bar
4. **MarkerType** - Circle/Rectangle/Triangle/Diamond/InvertedTriangle
5. **Placement** - Center/Near/Far
6. **ContainerType** - Normal/RoundedRectangle/Thermometer
7. **ExportType** - PNG/SVG/PDF
8. **TooltipPosition** - Top/Bottom/Left/Right
9. **Theme** - Various theme options
10. **Alignment** - Near/Center/Far
11. **PdfPageOrientation** - Portrait/Landscape
12. **GradientPosition** - Gradient positioning

---

## Best Practices

### 1. Async Method Usage

Always await async methods and wrap in `InvokeAsync` when calling from non-UI contexts:

```csharp
private async Task UpdatePointer()
{
    await gauge.SetPointerValueAsync(0, 0, 85);
}

// From event handler
timer.Elapsed += async (sender, e) => 
{
    await InvokeAsync(async () =>
    {
        await gauge.SetPointerValueAsync(0, 0, newValue);
    });
};
```

### 2. Animation Performance

For frequent updates, disable animation:

```razor
@using Syncfusion.Blazor.LinearGauge

<LinearGaugePointer PointerValue="@value" 
                    AnimationDuration="0">
</LinearGaugePointer>
```

### 3. Event Argument Modification

Only modify event arguments that are marked as modifiable:

```csharp
private void OnAxisLabelRendering(AxisLabelRenderEventArgs args)
{
    args.Text = args.Text + "%";  // ✓ Modifiable
}
```

### 4. Index-Based Referencing

Always use zero-based indices:

```csharp
// First axis, first pointer, first annotation
await gauge.SetPointerValueAsync(0, 0, 50);
await gauge.SetAnnotationValueAsync(0, "<div>50</div>");
```

### 5. Accessibility

Provide descriptions and labels for accessibility:

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Description="Temperature monitoring gauge"
               ID="tempGauge"
               Title="Temperature">
</SfLinearGauge>
```

### 6. Export Prerequisites

Enable appropriate flags before exporting:

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge AllowPdfExport="true" 
               AllowImageExport="true"
               AllowPrint="true">
</SfLinearGauge>
```

### 7. Theme Consistency

Use theme property for consistent styling across gauge:

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Theme="Theme.Material">
    <!-- All child components inherit theme -->
</SfLinearGauge>
```

---

## Common Implementation Patterns

### Pattern 1: Real-Time Data Update

```csharp
private async Task UpdateGaugeValue(double newValue)
{
    if (gauge != null)
    {
        await gauge.SetPointerValueAsync(0, 0, newValue);
    }
}
```

### Pattern 2: Range Validation

```csharp
private void OnValueChange(ValueChangeEventArgs args)
{
    if (args.Value < 0 || args.Value > 100)
    {
        // Invalid range
        return;
    }
    // Process valid value
}
```

### Pattern 3: Custom Tooltips

```csharp
private void OnTooltipRendering(TooltipRenderEventArgs args)
{
    string formattedValue = ((double)Convert.ToDouble(args.Content))
        .ToString("F2");
    args.Content = $"Value: {formattedValue}°C";
}
```

### Pattern 4: Export with Error Handling

```csharp
private async Task ExportAsImage()
{
    try
    {
        await gauge.ExportAsync(ExportType.PNG, "gauge-report");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Export failed: {ex.Message}");
    }
}
```

---

## Reference Links

- [Official API Documentation](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.LinearGauge.html)
- [Getting Started Guide](./getting-started.md)
- [Axes Configuration](./axes-configuration.md)
- [Pointers Guide](./pointers.md)
- [Ranges Guide](./ranges.md)
- [Methods and Events](./methods-and-events.md)
