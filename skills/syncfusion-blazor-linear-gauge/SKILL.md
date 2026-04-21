---
name: syncfusion-blazor-linear-gauge
description: Implement Syncfusion Blazor Linear Gauge (SfLinearGauge) for data visualization with linear scales. Use this when creating thermometer displays, progress indicators, or measurement visualizations. This skill covers axis configuration, pointers, ranges, and gauge appearance customization in Blazor applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Data Visualization"
---

# Implementing Syncfusion Blazor Linear Gauges

## When to Use This Skill

Use this skill when the user needs to:
- **Visualize numeric values** on a linear scale (horizontal or vertical)
- **Display measurement data** like temperature, pressure, speed, or progress
- **Create dashboard components** with gauge visualizations
- **Implement KPI displays** with ranges and indicators
- **Show real-time data** with animated pointers
- **Build interactive gauges** where users can drag pointers to change values
- **Create multi-scale visualizations** with multiple axes and pointers

The Syncfusion Blazor Linear Gauge is ideal for applications requiring precise numeric visualization with customizable scales, pointers, and ranges.

---

## Component Overview

The **Blazor Linear Gauge (SfLinearGauge)** is a data visualization component that displays numeric values on a linear scale with features including:

- **Multiple axes** with customizable ranges and styling
- **Various pointer types** (marker shapes, bar pointers, text, images)
- **Color-coded ranges** for value categorization
- **Annotations** for labels and custom content
- **Interactive features** including drag-to-change pointer values
- **Events and methods** for dynamic updates
- **Accessibility support** with WCAG compliance
- **Internationalization** for global applications

---

## Documentation and Navigation Guide

This skill provides comprehensive, self-contained reference files for each aspect of the Linear Gauge component. Read the appropriate reference based on your current need.

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** First-time setup, new project initialization, installation questions

**What's covered:**
- Installation via Visual Studio, VS Code, and .NET CLI
- NuGet package installation (Syncfusion.Blazor.LinearGauge, Syncfusion.Blazor.Themes)
- Namespace imports and service registration
- Stylesheet and script references
- Basic LinearGauge component implementation
- Adding pointers and setting values
- Adding titles and ranges
- Running your first gauge application

### Core Components

#### Axes Configuration

📄 **Read:** [references/axes-configuration.md](references/axes-configuration.md)

**When to read:** Setting up gauge scales, customizing axis appearance, working with multiple axes

**What's covered:**
- Setting Minimum and Maximum values for the scale
- Line customization (height, width, color, offset)
- Major ticks configuration (interval, height, color, width, position, offset)
- Minor ticks configuration (interval, height, color, width, position, offset)
- Label customization (format, font, position, offset)
- Multiple axes on single gauge
- Inverse axis direction
- Opposed position for axes
- Complete code examples for all configurations

#### Pointers

📄 **Read:** [references/pointers.md](references/pointers.md)

**When to read:** Adding value indicators, customizing pointer appearance, implementing multiple pointers

**What's covered:**
- Setting pointer values
- Marker pointers with various shapes (Circle, Rectangle, Triangle, Diamond, InvertedTriangle)
- Bar pointers with customization options
- Image pointers with custom images
- Text pointers with custom styling
- Pointer positioning and placement
- Animation configuration for smooth transitions
- Multiple pointers on single axis
- Drag and drop interaction for value changes
- Complete examples for each pointer type

#### Ranges

📄 **Read:** [references/ranges.md](references/ranges.md)

**When to read:** Adding color-coded value regions, implementing performance zones, creating visual thresholds

**What's covered:**
- Setting range start and end values
- Range color customization
- Range positioning (inside, outside, cross)
- Range offset configuration
- Start and end width for gradient effects
- Multiple ranges on single axis
- Practical use cases (temperature zones, performance indicators, status ranges)
- Complete code examples with visual results

### Appearance and Styling

#### Annotations

📄 **Read:** [references/annotations.md](references/annotations.md)

**When to read:** Adding labels, custom content, units, or dynamic text to the gauge

**What's covered:**
- Adding text annotations
- HTML content in annotations
- Positioning annotations with x, y coordinates
- Z-index for layering multiple annotations
- Font styling for text annotations
- Horizontal and vertical alignment
- Multiple annotations on gauge
- Dynamic content updates
- Practical examples (unit labels, value displays, warnings)

#### Appearance and Styling

📄 **Read:** [references/appearance-and-styling.md](references/appearance-and-styling.md)

**When to read:** Customizing gauge look and feel, theming, responsive design, print styling

**What's covered:**
- Container customization (width, height, background, border)
- Orientation (horizontal vs vertical)
- Margin and container adjustments
- Title customization (text, font, position)
- Theme selection (Bootstrap, Material, Fabric, Fluent, Tailwind, etc.)
- Custom CSS styling
- Responsive design considerations
- Color schemes and palettes
- Print-friendly styling
- Complete styling examples

### Advanced Features

#### Methods and Events

📄 **Read:** [references/methods-and-events.md](references/methods-and-events.md)

**When to read:** Implementing dynamic updates, handling user interactions, programmatic control

**What's covered:**
- setPointerValue() method for dynamic pointer updates
- setAnnotationValue() method for annotation updates
- print() method for printing gauges
- refresh() method for re-rendering
- Load and Loaded events
- AnimationComplete event
- AxisLabelRendering event for label customization
- AnnotationRendering event
- ValueChange event for user interactions
- TooltipRendering event
- ResizeCompleted event
- Complete event handler examples with practical scenarios

#### User Interaction

📄 **Read:** [references/user-interaction.md](references/user-interaction.md)

**When to read:** Enabling tooltips, drag interactions, mouse/touch events, value changes

**What's covered:**
- Tooltip configuration and customization
- Tooltip templates with custom HTML
- Pointer drag to change values
- Enabling/disabling pointer interaction
- Mouse and touch event handling
- Value change tracking and callbacks
- Animation on user interaction
- Complete interactive examples

### Accessibility and Internationalization

📄 **Read:** [references/accessibility-and-internationalization.md](references/accessibility-and-internationalization.md)

**When to read:** Implementing WCAG compliance, keyboard navigation, screen readers, global applications

**What's covered:**
- Accessibility overview (WCAG 2.1 Level AA compliance)
- Keyboard navigation support
- Screen reader compatibility
- ARIA attributes and labels
- High contrast mode support
- Focus indicators
- Internationalization (i18n) setup
- Number formatting for different locales
- RTL (Right-to-Left) support
- Complete accessibility implementation examples

---

## Quick Start Example

Here's a minimal example to get started with Blazor Linear Gauge:

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Title="Temperature Monitor">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="120">
            <LinearGaugeAxisLabelStyle Format="{value}°C"></LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="65" Color="#007bff">
                </LinearGaugePointer>
            </LinearGaugePointers>
            <LinearGaugeRanges>
                <LinearGaugeRange Start="0" End="40" Color="#4CAF50"></LinearGaugeRange>
                <LinearGaugeRange Start="40" End="80" Color="#FFC107"></LinearGaugeRange>
                <LinearGaugeRange Start="80" End="120" Color="#F44336"></LinearGaugeRange>
            </LinearGaugeRanges>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** A vertical linear gauge showing temperature from 0°C to 120°C with three color-coded ranges (green, yellow, red) and a pointer at 65°C.

---

## Common Patterns

### Pattern 1: Simple Progress Indicator

**Use case:** Show completion percentage or progress status

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Orientation="Syncfusion.Blazor.LinearGauge.Orientation.Horizontal" Width="400px" Height="80px">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="75" Type="Syncfusion.Blazor.LinearGauge.Point.Bar" 
                                    Color="#28a745" Width="20">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Pattern 2: Multi-Scale Dashboard Gauge

**Use case:** Display multiple metrics on one gauge

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="60" Color="#007bff"></LinearGaugePointer>
                <LinearGaugePointer PointerValue="80" Color="#dc3545"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Pattern 3: Interactive Value Adjuster

**Use case:** Allow users to change values by dragging the pointer

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeEvents ValueChange="@OnValueChange"></LinearGaugeEvents>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="@currentValue" 
                                    EnableDrag="true">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private double currentValue = 50;
    
    private void OnValueChange(ValueChangeEventArgs args)
    {
        currentValue = args.Value;
    }
}
```

### Pattern 4: Styled Thermometer

**Use case:** Temperature display with visual zones

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Width="150px" Height="400px">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="-20" Maximum="120">
            <LinearGaugeAxisLabelStyle Format="{value}°">
            </LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="@temperature" 
                                    Type="Syncfusion.Blazor.LinearGauge.Point.Bar"
                                    Color="#ff6b6b"
                                    Width="20">
                </LinearGaugePointer>
            </LinearGaugePointers>
            <LinearGaugeRanges>
                <LinearGaugeRange Start="-20" End="0" Color="#0099ff"></LinearGaugeRange>
                <LinearGaugeRange Start="0" End="25" Color="#00e676"></LinearGaugeRange>
                <LinearGaugeRange Start="25" End="120" Color="#ff5252"></LinearGaugeRange>
            </LinearGaugeRanges>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private double temperature = 35;
}
```

---

## API Reference

The official Linear Gauge API is centered on the `SfLinearGauge` component and comprehensive child components for configuring axes, pointers, ranges, annotations, gradients, tooltips, and event handlers. This reference is based on the official [Syncfusion Blazor Linear Gauge API documentation](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.LinearGauge.html).

### SfLinearGauge

Primary container component for the gauge. This is the root component that hosts all gauge elements.

**Component Properties:**

**Sizing & Layout:**
- `Width` (string): Sets the width of the linear gauge container
- `Height` (string): Sets the height of the linear gauge container
- `Orientation` (Orientation): Sets the gauge orientation (`Horizontal` or `Vertical`)
- `Description` (string): Sets the description for accessibility purposes

**Appearance:**
- `Title` (string): Sets the title text displayed on the gauge
- `TitleStyle` (LinearGaugeTitleStyle): Configures title font, position, and styling
- `Background` (string): Sets the background color of the gauge container
- `Theme` (Theme): Applies predefined themes (Bootstrap, Material, Fabric, Fluent, Tailwind, etc.)
- `Container` (LinearGaugeContainer): Configures the container appearance and border
- `Margin` (LinearGaugeMargin): Sets left, right, top, and bottom margins

**Behavior:**
- `AnimationDuration` (double): Sets the animation duration (in milliseconds) for initial rendering and pointer updates
- `Format` (string): Sets the number format for axis labels
- `EnableGroupingSeparator` (bool): Enables thousand separators in numbers
- `ID` (string): Sets the unique identifier for the gauge component
- `TabIndex` (int): Sets the tab order for keyboard navigation

**Export & Print:**
- `AllowPrint` (bool): Enables print functionality
- `AllowPdfExport` (bool): Enables PDF export functionality
- `AllowImageExport` (bool): Enables image export (PNG, SVG) functionality
- `AllowMargin` (bool): Includes margins when printing or exporting

**Styling:**
- `RangePalettes` (string[]): Defines automatic color palette for ranges

**Methods:**

```csharp
// Update pointer value programmatically (async version)
public async Task SetPointerValueAsync(int axisIndex, int pointerIndex, double value)

// Update annotation content dynamically (async version)
public async Task SetAnnotationValueAsync(int annotationIndex, string content)

// Refresh the entire gauge (async version)
public async Task RefreshAsync()

// Print the gauge (requires AllowPrint="true")
public async Task PrintAsync()

// Export gauge to file (requires corresponding AllowXyzExport="true")
public async Task ExportAsync(ExportType type, string fileName, 
    PdfPageOrientation? orientation = null, bool allowDownload = true)
```

**Export Types:**
- `ExportType.PNG`
- `ExportType.SVG`
- `ExportType.PDF`

### LinearGaugeAxes & LinearGaugeAxis

Container (`LinearGaugeAxes`) for one or more axes. Each `LinearGaugeAxis` defines a scale with range, ticks, labels, pointers, and ranges.

**LinearGaugeAxis Properties:**

**Scale Configuration:**
- `Minimum` (double): Sets the minimum value of the axis scale
- `Maximum` (double): Sets the maximum value of the axis scale
- `IsInversed` (bool): Reverses the direction of the axis (high to low instead of low to high)
- `OpposedPosition` (bool): Places the axis on the opposite side (left/right for vertical, top/bottom for horizontal)
- `ShowLastLabel` (bool): Shows or hides the last label on the axis

**Child Components:**
- `LinearGaugeLine` - Configures the axis line appearance
- `LinearGaugeMajorTicks` - Major tick marks on the axis
- `LinearGaugeMinorTicks` - Minor tick marks on the axis
- `LinearGaugeAxisLabelStyle` - Label styling and positioning
- `LinearGaugeAxisLabelFont` - Font properties for axis labels
- `LinearGaugeContainer` - Container for the axis
- `LinearGaugePointers` - Pointer collection for this axis
- `LinearGaugeRanges` - Range collection for this axis
- `LinearGaugeLinearGradient` - Gradient fill for axis elements
- `LinearGaugeRadialGradient` - Radial gradient for axis elements

### Axis Line & Ticks

**LinearGaugeLine** - Defines the axis line appearance
- `Height` (double): Height of the axis line
- `Width` (double): Width of the axis line
- `Color` (string): Color of the axis line
- `Offset` (double): Offset from axis

**LinearGaugeMajorTicks** - Major tick configuration
- `Interval` (double): Interval between major ticks
- `Height` (double): Height of major tick marks
- `Width` (double): Width of major tick marks
- `Color` (string): Color of major ticks
- `Position` (Position): Position relative to axis (`Inside`, `Outside`, `Cross`)
- `Offset` (double): Offset from axis line

**LinearGaugeMinorTicks** - Minor tick configuration
- `Interval` (double): Interval between minor ticks
- `Height` (double): Height of minor tick marks
- `Width` (double): Width of minor tick marks
- `Color` (string): Color of minor ticks
- `Position` (Position): Position relative to axis
- `Offset` (double): Offset from axis line

### Axis Labels

**LinearGaugeAxisLabelStyle** - Label styling and positioning
- `Format` (string): Format string for label values (e.g., "{value}°C")
- `Position` (Position): Label position (`Inside`, `Outside`, `Cross`)
- `Offset` (double): Distance from axis line
- `FontFamily` (string): Font family for labels
- `Color` (string): Color of label text
- `Opacity` (double): Opacity of labels (0 to 1)

**LinearGaugeAxisLabelFont** - Font properties for axis labels
- `FontFamily` (string): Font family name
- `FontSize` (string): Font size in pixels or relative units
- `FontStyle` (string): Font style (normal, italic, oblique)
- `FontWeight` (string): Font weight (normal, bold, lighter, bolder, 100-900)

### LinearGaugePointers & LinearGaugePointer

Container (`LinearGaugePointers`) for one or more value indicators. Each `LinearGaugePointer` represents a value on the axis.

**LinearGaugePointer Properties:**

**Value & Type:**
- `PointerValue` (double): Sets the pointer value
- `Type` (PointerType): Pointer type (`Marker` or `Bar`)
- `MarkerType` (MarkerType): Shape of marker pointer:
  - `Circle`
  - `Rectangle`
  - `Triangle`
  - `Diamond`
  - `InvertedTriangle`

**Appearance:**
- `Color` (string): Pointer color
- `Width` (double): Width of pointer
- `Height` (double): Height of pointer
- `Opacity` (double): Opacity of pointer (0 to 1)
- `Offset` (double): Offset from axis line
- `Placement` (Placement): Placement relative to axis (`Center`, `Near`, `Far`)
- `RoundedCornerRadius` (double): Radius for rounded corners (bar pointers)

**Content:**
- `ImageUrl` (string): URL for image pointer
- `Text` (string): Text to display with text pointer

**Interaction:**
- `EnableDrag` (bool): Allows user to drag the pointer to change value
- `AnimationDuration` (double): Duration of value change animation (milliseconds)

**Child Components:**
- `LinearGaugePointerBorder` - Border styling for pointer
- `LinearGaugeMarkerTextStyle` - Text styling for text pointers

### Pointer Styling

**LinearGaugePointerBorder** - Pointer border properties
- `Color` (string): Border color
- `Width` (double): Border width

**LinearGaugeMarkerTextStyle** - Text styling for pointer text
- `FontFamily` (string): Font family
- `FontSize` (string): Font size
- `Color` (string): Text color
- `FontWeight` (string): Font weight
- `Opacity` (double): Text opacity

### LinearGaugeRanges & LinearGaugeRange

Container (`LinearGaugeRanges`) for color-coded value zones. Each `LinearGaugeRange` defines a colored region.

**LinearGaugeRange Properties:**

**Range Definition:**
- `Start` (double): Start value of the range
- `End` (double): End value of the range

**Appearance:**
- `Color` (string): Background color of the range
- `StartWidth` (double): Width at start of range (for gradient width effect)
- `EndWidth` (double): Width at end of range
- `Position` (Position): Position relative to axis (`Inside`, `Outside`, `Cross`)
- `Offset` (double): Offset from axis line

**Child Components:**
- `LinearGaugeRangeBorder` - Border styling for range
- `LinearGaugeRangeTooltipSettings` - Tooltip for range (hover)

**LinearGaugeRangeBorder** - Range border properties
- `Color` (string): Border color
- `Width` (double): Border width

### LinearGaugeAnnotations & LinearGaugeAnnotation

Container (`LinearGaugeAnnotations`) for text or HTML overlays. Each `LinearGaugeAnnotation` adds a label or custom content.

**LinearGaugeAnnotation Properties:**

**Content & Positioning:**
- `AxisValue` (double): Position on the axis where annotation appears
- `AxisIndex` (int): Index of the axis (if multiple axes)
- `X` (double): Horizontal pixel offset from axis value
- `Y` (double): Vertical pixel offset from axis value
- `ZIndex` (int): Layer order (higher appears on top)
- `HorizontalAlignment` (Alignment): Horizontal alignment (`Near`, `Center`, `Far`)
- `VerticalAlignment` (Alignment): Vertical alignment

**Content:**
- `Content` (string): Text or HTML content for annotation
- `ContentTemplate` (RenderFragment): Template for rendering annotation content

**Child Components:**
- `LinearGaugeAnnotationFont` - Font styling for text annotations

**LinearGaugeAnnotationFont** - Font properties for annotations
- `FontFamily` (string): Font family
- `FontSize` (string): Font size
- `Color` (string): Text color
- `FontWeight` (string): Font weight
- `Opacity` (double): Text opacity

### Gradient Configuration

**LinearGaugeLinearGradient** - Linear gradient for gauge elements
- `StartValue` (double): Start position of gradient
- `EndValue` (double): End position of gradient
- `ColorStops` (ColorStops): Collection of color stops

**LinearGaugeRadialGradient** - Radial gradient for gauge elements
- `Radius` (string): Radius of radial gradient
- `InnerPosition` (InnerPosition): Inner circle position
- `OuterPosition` (OuterPosition): Outer circle position
- `ColorStops` (ColorStops): Collection of color stops

**ColorStop / ColorStops** - Gradient color definitions
- `Offset` (string): Position in gradient (e.g., "0%", "100%")
- `Color` (string): Color at this position
- `Opacity` (double): Opacity at this position

### Container & Border Styling

**LinearGaugeContainer** - Outer container for the gauge
- `Type` (ContainerType): Container type (`Normal`, `RoundedRectangle`, `Thermometer`)
- `Offset` (double): Offset from the axis
- `Width` (double): Width of container
- `BackgroundColor` (string): Background color
- `Border` (LinearGaugeContainerBorder): Border styling

**LinearGaugeContainerBorder** - Container border properties
- `Color` (string): Border color
- `Width` (double): Border width

**LinearGaugeBorder / LinearGaugeBorderSettings** - Gauge border properties
- `Color` (string): Border color
- `Width` (double): Border width

**LinearGaugeMargin / LinearGaugeMarginSettings** - Margin configuration
- `Left` (double): Left margin
- `Right` (double): Right margin
- `Top` (double): Top margin
- `Bottom` (double): Bottom margin

### Tooltip Configuration

**LinearGaugeTooltipSettings** - Pointer tooltip configuration
- `Enable` (bool): Enable or disable tooltips
- `Format` (string): Tooltip text format
- `Position` (TooltipPosition): Tooltip position (`Top`, `Bottom`, `Left`, `Right`)
- `Fill` (string): Tooltip background color
- `Opacity` (double): Tooltip opacity
- `ShowAtMousePosition` (bool): Show at cursor instead of pointer
- `TextStyle` (LinearGaugeTooltipTextStyle): Text styling
- `Border` (LinearGaugeTooltipBorder): Border styling

**LinearGaugeRangeTooltipSettings** - Range hover tooltip configuration
- Similar properties to tooltip settings for range hovers

**LinearGaugeTooltipTextStyle** - Tooltip text styling
- `FontFamily` (string), `FontSize` (string), `Color` (string), `FontWeight` (string), `Opacity` (double)

### Events & Event Arguments

**LinearGaugeEvents** - Event handler component

```razor
@using Syncfusion.Blazor.LinearGauge

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

**Event Types and Arguments:**

| Event | Argument Type | Description |
|-------|---------------|-------------|
| `Load` | `LoadEventArgs` | Fires before gauge renders; can cancel with `args.Cancel = true` |
| `Loaded` | `LoadedEventArgs` | Fires after gauge is successfully rendered |
| `AxisLabelRendering` | `AxisLabelRenderEventArgs` | Fires before each axis label is rendered; modify `args.Text` to customize |
| `AnnotationRendering` | `AnnotationRenderEventArgs` | Fires before annotation is rendered; modify `args.Content` to customize |
| `ValueChange` | `ValueChangeEventArgs` | Fires when pointer value changes (user drag or programmatic); provides `Value`, `PointerIndex`, `AxisIndex` |
| `TooltipRendering` | `TooltipRenderEventArgs` | Fires before tooltip displays; customize `args.Content` |
| `DragStart` | `PointerDragEventArgs` | Fires when user starts dragging pointer; provides `CurrentValue`, `PointerIndex`, `AxisIndex` |
| `DragMove` | `PointerDragEventArgs` | Fires continuously during pointer drag; provides current value |
| `DragEnd` | `PointerDragEventArgs` | Fires when user releases pointer after drag |
| `AnimationComplete` | `AnimationCompleteEventArgs` | Fires when pointer animation completes |
| `ResizeCompleted` | `ResizeEventArgs` | Fires after gauge is resized; provides `CurrentSize`, `PreviousSize` |
| `OnGaugeMouseDown` | `MouseEventArgs` | Fires on mouse down on gauge |
| `OnGaugeMouseLeave` | `MouseEventArgs` | Fires when mouse leaves gauge |
| `OnGaugeMouseUp` | `MouseEventArgs` | Fires on mouse up on gauge |
| `Print` | `PrintEventArgs` | Fires before printing |

### Enums

**Orientation** - Gauge layout direction
```csharp
Orientation.Horizontal  // Horizontal scale
Orientation.Vertical    // Vertical scale
```

**Position** - Element positioning relative to axis
```csharp
Position.Inside      // Inside the axis
Position.Outside     // Outside the axis
Position.Cross       // Crossing the axis line
```

**PointerType** - Type of value indicator
```csharp
PointerType.Marker   // Shaped marker (circle, square, etc.)
PointerType.Bar      // Bar/rectangle pointer
```

**MarkerType** - Shape of marker pointer
```csharp
MarkerType.Circle
MarkerType.Rectangle
MarkerType.Triangle
MarkerType.Diamond
MarkerType.InvertedTriangle
```

**Placement** - Pointer position relative to axis
```csharp
Placement.Center     // Centered on axis
Placement.Near       // Near side
Placement.Far        // Far side
```

**ContainerType** - Container shape
```csharp
ContainerType.Normal              // Rectangular container
ContainerType.RoundedRectangle    // Rounded rectangle
ContainerType.Thermometer         // Thermometer shape
```

**ExportType** - Export format
```csharp
ExportType.PNG   // Export as PNG image
ExportType.SVG   // Export as SVG image
ExportType.PDF   // Export as PDF document
```

**TooltipPosition** - Tooltip placement
```csharp
TooltipPosition.Top      // Above the pointer
TooltipPosition.Bottom   // Below the pointer
TooltipPosition.Left     // Left of pointer
TooltipPosition.Right    // Right of pointer
```

**Theme** - Predefined color themes
```csharp
Theme.Bootstrap
Theme.Material
Theme.Fabric
Theme.Fluent
Theme.Tailwind
Theme.Bootstrap5
Theme.FluentDark
Theme.MaterialDark
Theme.BootstrapDark
Theme.TailwindDark
// ... and others
```

### Primary Component Hierarchy

```
SfLinearGauge (root)
├── LinearGaugeTitleStyle
├── LinearGaugeMargin
├── LinearGaugeBorder
├── LinearGaugeContainer
│   └── LinearGaugeContainerBorder
├── LinearGaugeAxes
│   └── LinearGaugeAxis (repeatable)
│       ├── LinearGaugeLine
│       ├── LinearGaugeMajorTicks
│       ├── LinearGaugeMinorTicks
│       ├── LinearGaugeAxisLabelStyle
│       ├── LinearGaugeAxisLabelFont
│       ├── LinearGaugeContainer
│       ├── LinearGaugeLinearGradient
│       ├── LinearGaugeRadialGradient
│       ├── LinearGaugePointers
│       │   └── LinearGaugePointer (repeatable)
│       │       ├── LinearGaugePointerBorder
│       │       └── LinearGaugeMarkerTextStyle
│       └── LinearGaugeRanges
│           └── LinearGaugeRange (repeatable)
│               ├── LinearGaugeRangeBorder
│               └── LinearGaugeRangeTooltipSettings
├── LinearGaugeAnnotations
│   └── LinearGaugeAnnotation (repeatable)
│       └── LinearGaugeAnnotationFont
├── LinearGaugeTooltipSettings
│   ├── LinearGaugeTooltipTextStyle
│   └── LinearGaugeTooltipBorder
└── LinearGaugeEvents
    ├── Load handler
    ├── Loaded handler
    ├── AxisLabelRendering handler
    ├── AnnotationRendering handler
    ├── ValueChange handler
    ├── TooltipRendering handler
    ├── DragStart/Move/End handlers
    ├── AnimationComplete handler
    └── ... more event handlers
```

### Practical API Usage Notes

1. **Async Methods**: All component methods (`SetPointerValueAsync`, `SetAnnotationValueAsync`, `RefreshAsync`, `PrintAsync`, `ExportAsync`) are asynchronous and must be awaited.

2. **Export Prerequisites**: Ensure `AllowPdfExport="true"`, `AllowImageExport="true"`, or `AllowPrint="true"` on the gauge before calling corresponding export or print methods.

3. **Animation**: Set `AnimationDuration="0"` for instant updates, or higher values for smooth animated transitions.

4. **Event Arguments**: Event handler arguments are provided by the framework and contain context-specific information (e.g., `ValueChangeEventArgs` contains the new value).

5. **Index-Based Access**: When using `SetPointerValueAsync` or `SetAnnotationValueAsync`, use zero-based indices for axes, pointers, and annotations.

6. **Performance**: For frequent updates (e.g., every 100ms), consider batching updates or using `AnimationDuration="0"` to avoid animation overhead.

7. **Accessibility**: Use `Description`, `Title`, and proper ARIA labels when the gauge needs to be accessible to screen readers.

### Complete Component List

**Component Classes (from Syncfusion.Blazor.LinearGauge namespace):**

1. `SfLinearGauge` - Main component
2. `LinearGaugeAnnotation` - Annotation element
3. `LinearGaugeAnnotationFont` - Annotation font styling
4. `LinearGaugeAnnotations` - Annotation collection
5. `LinearGaugeAxes` - Axis collection
6. `LinearGaugeAxis` - Axis configuration
7. `LinearGaugeAxisLabelFont` - Axis label font
8. `LinearGaugeAxisLabelStyle` - Axis label styling
9. `LinearGaugeBorder` - Gauge border
10. `LinearGaugeBorderSettings` - Border settings
11. `LinearGaugeContainer` - Container configuration
12. `LinearGaugeContainerBorder` - Container border
13. `LinearGaugeEvents` - Event handlers
14. `LinearGaugeFontSettings` - Font configuration
15. `LinearGaugeLine` - Axis line
16. `LinearGaugeLinearGradient` - Linear gradient
17. `LinearGaugeMajorTicks` - Major tick marks
18. `LinearGaugeMargin` - Margin settings
19. `LinearGaugeMarginSettings` - Margin configuration
20. `LinearGaugeMarkerTextStyle` - Marker text styling
21. `LinearGaugeMinorTicks` - Minor tick marks
22. `LinearGaugePointer` - Pointer element
23. `LinearGaugePointerBorder` - Pointer border
24. `LinearGaugePointers` - Pointer collection
25. `LinearGaugeRadialGradient` - Radial gradient
26. `LinearGaugeRange` - Range element
27. `LinearGaugeRangeBorder` - Range border
28. `LinearGaugeRangeTooltipBorder` - Range tooltip border
29. `LinearGaugeRangeTooltipSettings` - Range tooltip
30. `LinearGaugeRangeTooltipTextStyle` - Range tooltip text
31. `LinearGaugeRanges` - Range collection
32. `LinearGaugeTickSettings` - Tick configuration
33. `LinearGaugeTitleStyle` - Title styling
34. `LinearGaugeTooltipBorder` - Tooltip border
35. `LinearGaugeTooltipSettings` - Tooltip configuration
36. `LinearGaugeTooltipTextStyle` - Tooltip text styling
37. `LinearGradient` - Linear gradient definition
38. `RadialGradient` - Radial gradient definition
39. `ColorStop` - Gradient color stop
40. `ColorStops` - Gradient color stops collection
41. `Point` - Pointer type enum
42. `GradientPosition` - Gradient position
43. `InnerPosition` - Inner gradient position
44. `OuterPosition` - Outer gradient position

**Event Argument Classes:**
- `LoadEventArgs`
- `LoadedEventArgs`
- `AxisLabelRenderEventArgs`
- `AnnotationRenderEventArgs`
- `ValueChangeEventArgs`
- `TooltipRenderEventArgs`
- `PointerDragEventArgs`
- `ResizeEventArgs`
- `MouseEventArgs`
- `PrintEventArgs`
- `AnimationCompleteEventArgs`

**Enum Types:**
- `Orientation` - Horizontal or Vertical
- `Position` - Inside, Outside, Cross
- `PointerType` - Marker or Bar
- `MarkerType` - Circle, Rectangle, Triangle, Diamond, InvertedTriangle
- `Placement` - Center, Near, Far
- `ContainerType` - Normal, RoundedRectangle, Thermometer
- `ExportType` - PNG, SVG, PDF
- `TooltipPosition` - Top, Bottom, Left, Right
- `Theme` - Various theme options
- `Alignment` - Near, Center, Far
- `PdfPageOrientation` - Portrait, Landscape

---

## Common Use Cases

### 1. Dashboard KPI Display
Display key performance indicators with color-coded ranges for quick status assessment. Use multiple gauges for different metrics.

### 2. Temperature Monitoring
Visualize temperature readings with appropriate ranges for different zones (freezing, normal, warning, critical).

### 3. Progress Tracking
Show task completion, project progress, or goal achievement using bar pointers on horizontal gauges.

### 4. Speedometer/Tachometer
Create vehicle dashboard components with styled pointers and ranges for speed or RPM display.

### 5. Sensor Data Visualization
Display real-time sensor readings (pressure, humidity, voltage) with dynamic pointer updates and threshold indicators.

### 6. Survey/Rating Display
Visualize survey results or ratings on a scale with markers showing average values and ranges for distribution.

### 7. Budget vs. Actual
Show financial metrics comparing budgeted vs. actual values using multiple pointers on the same axis.

### 8. Stock/Inventory Levels
Display inventory levels with color-coded ranges (critical, low, adequate, full) for quick status checks.

---

## Tips for Implementation

### When Starting
1. Begin with [getting-started.md](references/getting-started.md) for setup
2. Add basic axis and pointer to verify functionality
3. Gradually add ranges, styling, and interactivity

### For Customization
1. Use [axes-configuration.md](references/axes-configuration.md) for scale customization
2. Use [pointers.md](references/pointers.md) for pointer styling
3. Use [appearance-and-styling.md](references/appearance-and-styling.md) for overall look

### For Interactivity
1. Enable pointer dragging for user input
2. Add tooltips for value display
3. Use events for responding to user actions
4. See [user-interaction.md](references/user-interaction.md) and [methods-and-events.md](references/methods-and-events.md)

### For Production
1. Implement accessibility features from [accessibility-and-internationalization.md](references/accessibility-and-internationalization.md)
2. Test with keyboard navigation
3. Verify screen reader compatibility
4. Configure proper ARIA labels

---

## Next Steps

1. **New to Linear Gauge?** Start with [getting-started.md](references/getting-started.md)
2. **Need specific feature?** Navigate to the relevant reference file based on your requirement
3. **Building dashboard?** Combine multiple patterns and refer to [appearance-and-styling.md](references/appearance-and-styling.md)
4. **Need interactivity?** Check [user-interaction.md](references/user-interaction.md) and [methods-and-events.md](references/methods-and-events.md)

All reference files are comprehensive and self-contained - you won't need to jump between multiple files for a single topic.
