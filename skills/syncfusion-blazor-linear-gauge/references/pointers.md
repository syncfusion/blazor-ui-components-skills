# Pointers in Blazor Linear Gauge

## Table of Contents
- [Overview](#overview)
- [Setting Pointer Value](#setting-pointer-value)
- [Pointer Types](#pointer-types)
  - [Marker Pointers](#marker-pointers)
  - [Bar Pointers](#bar-pointers)
- [Marker Pointer Customization](#marker-pointer-customization)
  - [Marker Shapes](#marker-shapes)
  - [Image Pointers](#image-pointers)
  - [Text Pointers](#text-pointers)
  - [Marker Properties](#marker-properties)
- [Bar Pointer Customization](#bar-pointer-customization)
- [Multiple Pointers](#multiple-pointers)
- [Pointer Animation](#pointer-animation)
- [Gradient Colors](#gradient-colors)
  - [Linear Gradient](#linear-gradient)
  - [Radial Gradient](#radial-gradient)
- [Pointer Positioning](#pointer-positioning)
- [Complete Examples](#complete-examples)

## Overview

Visual indicators that show specific values on the axis of a Linear Gauge, essential for displaying current readings, measurements, or status values through pointer types and customization.

Pointers are visual indicators that show specific values on the axis of a Linear Gauge. They are essential for displaying current readings, measurements, or status values. The Linear Gauge supports two main types of pointers:

1. **Marker Pointers:** Shape-based indicators (triangles, circles, diamonds, etc.)
2. **Bar Pointers:** Filled bars that extend from the axis minimum to the pointer value

Each pointer is highly customizable with properties for appearance, animation, and interaction.

## Setting Pointer Value

The `PointerValue` property determines where the pointer appears on the axis scale for individual pointer positioning.

The pointer value determines where the pointer appears on the axis scale. Use the `PointerValue` property to set this value.

### Basic Pointer

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="80"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** A pointer appears at value 80 on the axis (default range 0-100).

### Dynamic Pointer Value

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="200">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="@currentValue"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private double currentValue = 150;
}
```

**Use case:** Real-time data updates, sensor readings, live dashboards.

## Pointer Types

The `Type` property defines whether a pointer is a Marker or Bar type, controlling the visual representation of pointer values.

### Marker Pointers

Marker pointers use shapes to indicate values. This is the default pointer type.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="60" Type="Syncfusion.Blazor.LinearGauge.Point.Marker">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Default marker:** InvertedTriangle

### Bar Pointers

The `Type` property set to `Point.Bar` creates filled rectangle pointers extending from axis minimum to the specified `PointerValue`.

Bar pointers display as filled bars extending from the axis minimum to the pointer value.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="70" Type="Syncfusion.Blazor.LinearGauge.Point.Bar">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Use case:** Progress bars, fill level indicators, percentage displays.

## Marker Pointer Customization

Configuration of marker pointers using `MarkerType`, `Height`, `Width`, `Color`, `Opacity`, and other visual properties to customize appearance and styling.

### Marker Shapes

The `MarkerType` property defines the shape of marker pointers. Available shapes:

- **Circle**
- **Rectangle**
- **Triangle**
- **InvertedTriangle** (default)
- **Diamond**
- **Image** (requires ImageUrl)
- **Text** (requires Text property)

#### Circle Marker

The `MarkerType` property set to `MarkerType.Circle` creates circular-shaped markers with `Height` and `Width` properties defining dimensions.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="60" MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Circle"
                                    Height="20" Width="20" Color="#007bff">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

#### Diamond Marker

The `MarkerType` property set to `MarkerType.Diamond` creates diamond-shaped markers with customizable `Height`, `Width`, and `Color` properties.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="75" MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Diamond"
                                    Height="15" Width="15" Color="#28a745">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

#### Triangle Marker

The `MarkerType` property set to `MarkerType.Triangle` creates triangular-shaped markers controlled by `Height`, `Width`, and `Color` properties.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="50" MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Triangle"
                                    Height="18" Width="18" Color="#ffc107">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

#### Rectangle Marker

The `MarkerType` property set to `MarkerType.Rectangle` creates rectangular-shaped markers with `Height`, `Width`, and `Color` for customization.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="85" MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Rectangle"
                                    Height="25" Width="10" Color="#dc3545">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Image Pointers

The `MarkerType` property set to `MarkerType.Image` with `ImageUrl`, `Height`, `Width`, and `Offset` properties creates custom image-based pointers.

Use custom images as pointers by setting `MarkerType="MarkerType.Image"` and providing an `ImageUrl`.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Orientation="Syncfusion.Blazor.LinearGauge.Orientation.Horizontal">
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugeMajorTicks Position="Syncfusion.Blazor.LinearGauge.Position.Outside">
            </LinearGaugeMajorTicks>
            <LinearGaugeMinorTicks Position="Syncfusion.Blazor.LinearGauge.Position.Outside">
            </LinearGaugeMinorTicks>
            <LinearGaugeAxisLabelStyle Position="Syncfusion.Blazor.LinearGauge.Position.Outside">
            </LinearGaugeAxisLabelStyle>
            
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="60" 
                                    Offset="-27" 
                                    Height="40" 
                                    Width="40" 
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Image" 
                                    ImageUrl="https://example.com/pointer-icon.png">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Properties:**
- `ImageUrl`: Path to the image file
- `Height`: Image height in pixels
- `Width`: Image width in pixels
- `Offset`: Position adjustment

**Use case:** Custom branding, themed pointers, icon-based indicators.

### Text Pointers

The `MarkerType` property set to `MarkerType.Text` with `Text` property and `LinearGaugeMarkerTextStyle` including `Size`, `FontWeight`, `Color` for text-based pointers.

Display text as a pointer by setting `MarkerType="MarkerType.Text"` and providing the `Text` property.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="70" 
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Text" 
                                    Text="70°">
                    <LinearGaugeMarkerTextStyle Size="14px" 
                                                FontWeight="bold">
                    </LinearGaugeMarkerTextStyle>
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Text Styling Properties:**
- **Color:** Text color
- **Size:** Font size
- **FontFamily:** Font family name
- **FontWeight:** bold, normal, lighter, bolder
- **FontStyle:** normal, italic, oblique
- **Opacity:** 0 to 1

#### Styled Text Pointer

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="85" 
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Text" 
                                    Text="ALERT"
                                    Color="#dc3545">
                    <LinearGaugeMarkerTextStyle 
                        Size="16px" 
                        FontWeight="bold"
                        FontFamily="Arial">
                    </LinearGaugeMarkerTextStyle>
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Use case:** Warning labels, status text, custom annotations.

### Marker Properties

Common marker pointer properties including `PointerValue`, `Height`, `Width`, `Color`, `Opacity`, `Offset`, `Placement`, `AnimationDuration`, and `EnableDrag` for full customization.

Common properties for all marker pointers:

| Property | Description | Example Value |
|----------|-------------|---------------|
| `PointerValue` | Value position on axis | 80 |
| `Height` | Marker height (pixels) | 20 |
| `Width` | Marker width (pixels) | 20 |
| `Color` | Marker color | "#007bff" |
| `Opacity` | Transparency (0-1) | 0.8 |
| `Offset` | Distance from axis | -10 |
| `Placement` | Near, Far, Center, None | Placement.Center |
| `AnimationDuration` | Animation time (ms) | 1000 |
| `EnableDrag` | Allow drag interaction | true/false |

#### Complete Marker Example

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer 
                    PointerValue="65"
                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Diamond"
                    Height="25"
                    Width="25"
                    Color="#17a2b8"
                    Opacity="0.9"
                    Offset="-5"
                    Placement="Syncfusion.Blazor.LinearGauge.Placement.Center"
                    AnimationDuration="1500">
                    <LinearGaugePointerBorder Color="#0c5460" Width="2">
                    </LinearGaugePointerBorder>
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

## Bar Pointer Customization

Configuration of bar pointers using `Width`, `Color`, `Opacity`, `Offset`, `RoundedCornerRadius`, and `AnimationDuration` properties for styling and visual effects.

Bar pointers are filled rectangles that extend from the axis minimum to the pointer value.

### Basic Bar Pointer

The `Width`, `Color`, and `Opacity` properties control the appearance of bar pointers extending from minimum to specified `PointerValue`.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="70" 
                                    Type="Syncfusion.Blazor.LinearGauge.Point.Bar" 
                                    Color="#28a745"
                                    Width="20">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Bar Pointer Properties

| Property | Description | Example Value |
|----------|-------------|---------------|
| `PointerValue` | End value of bar | 80 |
| `Width` | Bar thickness (pixels) | 20 |
| `Color` | Bar fill color | "#f44141" |
| `Opacity` | Transparency (0-1) | 0.7 |
| `Offset` | Distance from axis | 10 |
| `RoundedCornerRadius` | Corner rounding (pixels) | 5 |
| `AnimationDuration` | Animation time (ms) | 1000 |

### Styled Bar Pointer

The `Color`, `Opacity`, `RoundedCornerRadius` properties with `LinearGaugePointerBorder` create styled bar pointers with borders.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="80" 
                                    Type="Syncfusion.Blazor.LinearGauge.Point.Bar" 
                                    Width="20"
                                    Color="#f44141"
                                    Opacity="0.8"
                                    RoundedCornerRadius="5">
                    <LinearGaugePointerBorder Color="#a32828" Width="2">
                    </LinearGaugePointerBorder>
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Rounded Bar Pointer

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="65" 
                                    Type="Syncfusion.Blazor.LinearGauge.Point.Bar" 
                                    Width="15"
                                    Color="#007bff"
                                    RoundedCornerRadius="10">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Use case:** Modern progress bars, smooth fill indicators.

### Multiple Bar Pointers

Multiple `LinearGaugePointer` instances with `Type="Point.Bar"`, different `PointerValue`, `Width`, `Color`, and `Offset` properties create layered bar displays.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="50" 
                                    Type="Syncfusion.Blazor.LinearGauge.Point.Bar" 
                                    Width="25"
                                    Color="#28a745"
                                    Offset="-30">
                </LinearGaugePointer>
                <LinearGaugePointer PointerValue="75" 
                                    Type="Syncfusion.Blazor.LinearGauge.Point.Bar" 
                                    Width="25"
                                    Color="#ffc107"
                                    Offset="10">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Use case:** Comparing multiple metrics, layered progress indicators.

## Multiple Pointers

Display multiple pointer instances on the same axis with different `PointerValue`, `MarkerType`, `Color`, and positioning to show multiple values simultaneously.

Add multiple pointers to display several values on the same axis.

### Basic Multiple Pointers

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="30" 
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Circle"
                                    Color="#28a745">
                </LinearGaugePointer>
                <LinearGaugePointer PointerValue="60" 
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Diamond"
                                    Color="#ffc107">
                </LinearGaugePointer>
                <LinearGaugePointer PointerValue="80" 
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.InvertedTriangle"
                                    Color="#dc3545">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Three pointers at different values with different shapes and colors.

### Mixed Pointer Types

Combining `Type="Point.Bar"` with `Type="Point.Marker"` and different `MarkerType` values with distinct `Color` and `Opacity` for layered visualizations.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <!-- Bar pointer as background -->
                <LinearGaugePointer PointerValue="70" 
                                    Type="Syncfusion.Blazor.LinearGauge.Point.Bar" 
                                    Width="20"
                                    Color="#e9ecef"
                                    Opacity="0.5">
                </LinearGaugePointer>
                
                <!-- Marker pointer as indicator -->
                <LinearGaugePointer PointerValue="70" 
                                    Type="Syncfusion.Blazor.LinearGauge.Point.Marker"
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Triangle"
                                    Color="#007bff"
                                    Height="20"
                                    Width="20">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Use case:** Combining fill bars with position markers for enhanced visualization.

### Color-Coded Multiple Pointers

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Width="200px" Height="400px">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="25" Color="#28a745" 
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Circle" Height="15" Width="15">
                </LinearGaugePointer>
                <LinearGaugePointer PointerValue="50" Color="#ffc107" 
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Circle" Height="15" Width="15">
                </LinearGaugePointer>
                <LinearGaugePointer PointerValue="75" Color="#dc3545" 
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Circle" Height="15" Width="15">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Use case:** Multi-sensor displays, comparative metrics, threshold indicators.

## Pointer Animation

The `AnimationDuration` property (in milliseconds) controls the animation speed of pointers during gauge load or value changes.

Pointers animate when the gauge loads or when their value changes dynamically.

### Animation Duration

The `AnimationDuration` property specifies the time in milliseconds for pointer animation during gauge initialization.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="80" AnimationDuration="2000">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**AnimationDuration:** Time in milliseconds (1000 = 1 second)

### Multiple Pointers with Different Animations

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="30" 
                                    AnimationDuration="1000"
                                    Color="#28a745">
                </LinearGaugePointer>
                <LinearGaugePointer PointerValue="60" 
                                    AnimationDuration="1500"
                                    Color="#ffc107">
                </LinearGaugePointer>
                <LinearGaugePointer PointerValue="90" 
                                    AnimationDuration="2000"
                                    Color="#dc3545">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Pointers animate sequentially with different speeds.

### Multiple Pointers with Different Animations

Multiple pointers with varying `AnimationDuration` values create sequential animation effects for visual hierarchy.

**Note:** Animation does not occur during pointer dragging.

## Gradient Colors

Apply gradient colors to pointers using `LinearGaugeLinearGradient` with `StartValue`, `EndValue`, `ColorStops` or `RadialGradient` with `Radius`, `InnerPosition`, `OuterPosition` properties.

Apply gradient colors to pointers for enhanced visual appeal.

### Linear Gradient

The `LinearGaugeLinearGradient` component with `StartValue`, `EndValue`, and `ColorStops` containing `Color`, `Offset`, and `Opacity` creates linear color transitions.

Colors transition in a linear progression from start to end.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Orientation="Syncfusion.Blazor.LinearGauge.Orientation.Horizontal">
    <LinearGaugeContainer Width="30" Offset="30">
        <LinearGaugeContainerBorder Width="0" />
        <LinearGaugeAxes>
            <LinearGaugeAxis>
                <LinearGaugeAxisLabelStyle Offset="55">
                </LinearGaugeAxisLabelStyle>
                <LinearGaugeLine Width="0" />
                <LinearGaugeMajorTicks Height="0" Interval="25" />
                <LinearGaugeMinorTicks Height="0" />
                
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="80" 
                                        Height="25" 
                                        Width="35"
                                        Offset="-40" 
                                        MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Triangle"
                                        Placement="Syncfusion.Blazor.LinearGauge.Placement.Near">
                        <LinearGradient StartValue="1%" EndValue="99%">
                            <Syncfusion.Blazor.LinearGauge.ColorStops>
                                <Syncfusion.Blazor.LinearGauge.ColorStop Opacity="1" Color="#fef3f9" Offset="1%">
                                </Syncfusion.Blazor.LinearGauge.ColorStop>
                                <Syncfusion.Blazor.LinearGauge.ColorStop Opacity="1" Color="#f54ea2" Offset="100%">
                                </Syncfusion.Blazor.LinearGauge.ColorStop>
                            </Syncfusion.Blazor.LinearGauge.ColorStops>
                        </LinearGradient>
                    </LinearGaugePointer>
                </LinearGaugePointers>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </LinearGaugeContainer>
</SfLinearGauge>
```

**Properties:**
- `StartValue`: Gradient start position (percentage)
- `EndValue`: Gradient end position (percentage)
- `ColorStops`: Array of color transitions with offset, opacity, and color

### Radial Gradient

The `LinearGaugeRadialGradient` component with `Radius`, `InnerPosition`, `OuterPosition`, and `ColorStops` creates circular color transitions from center outward.

Colors transition in a circular progression from inner to outer position.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Orientation="Syncfusion.Blazor.LinearGauge.Orientation.Horizontal">
    <LinearGaugeContainer Width="30" Offset="30">
        <LinearGaugeContainerBorder Width="0" />
        <LinearGaugeAxes>
            <LinearGaugeAxis>
                <LinearGaugeAxisLabelStyle Offset="55">
                </LinearGaugeAxisLabelStyle>
                <LinearGaugeLine Width="0" />
                <LinearGaugeMajorTicks Height="0" Interval="25" />
                <LinearGaugeMinorTicks Height="0" />
                
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="80" 
                                        Height="25" 
                                        Width="35"
                                        Offset="-40" 
                                        MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Triangle"
                                        Placement="Syncfusion.Blazor.LinearGauge.Placement.Near">
                        <LinearGaugeRadialGradient Radius="60%">
                            <Syncfusion.Blazor.LinearGauge.InnerPosition X="50%" Y="50%"></Syncfusion.Blazor.LinearGauge.InnerPosition>
                            <Syncfusion.Blazor.LinearGauge.OuterPosition X="50%" Y="50%"></Syncfusion.Blazor.LinearGauge.OuterPosition>
                            <Syncfusion.Blazor.LinearGauge.ColorStops>
                                <Syncfusion.Blazor.LinearGauge.ColorStop Opacity="0.9" Color="#fff5f5" Offset="1%">
                                </Syncfusion.Blazor.LinearGauge.ColorStop>
                                <Syncfusion.Blazor.LinearGauge.ColorStop Opacity="1" Color="#f54ea2" Offset="99%">
                                </Syncfusion.Blazor.LinearGauge.ColorStop>
                            </Syncfusion.Blazor.LinearGauge.ColorStops>
                        </LinearGaugeRadialGradient>
                    </LinearGaugePointer>
                </LinearGaugePointers>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </LinearGaugeContainer>
</SfLinearGauge>
```

**Properties:**
- `Radius`: Gradient radius (percentage)
- `InnerPosition`: Center point (X, Y percentages)
- `OuterPosition`: Outer circle position (X, Y percentages)
- `ColorStops`: Array of color transitions

**Note:** If both gradients are set, linear gradient takes precedence. Set `StartValue` and `EndValue` to empty strings to render radial gradient.

## Pointer Positioning

Control pointer placement using `Placement` property (Center, Near, Far, None) and `Offset` property to adjust distance from the axis.

Control pointer placement using `Placement` and `Offset` properties.

### Placement Options

- **Placement.Center:** Center of the axis (default)
- **Placement.Near:** Near the axis line
- **Placement.Far:** Far from the axis line
- **Placement.None:** No specific placement

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="50" 
                                    Placement="Syncfusion.Blazor.LinearGauge.Placement.Far"
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Circle">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Offset Positioning

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="70" 
                                    Offset="-20"
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Diamond"
                                    Height="20"
                                    Width="20">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Offset:** Positive values move away from axis, negative values move toward axis.

## Complete Examples

Practical implementations demonstrating combinations of pointer types, customization, animation, and positioning properties in real-world scenarios.

### Temperature Indicator with Multiple Markers

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Title="Temperature Zones" Width="200px" Height="450px">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="-20" Maximum="120">
            <LinearGaugeAxisLabelStyle Format="{value}°C">
            </LinearGaugeAxisLabelStyle>
            
            <LinearGaugePointers>
                <!-- Current temperature -->
                <LinearGaugePointer PointerValue="35" 
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Circle"
                                    Height="20"
                                    Width="20"
                                    Color="#dc3545"
                                    AnimationDuration="1000">
                    <LinearGaugePointerBorder Color="#a71d2a" Width="2">
                    </LinearGaugePointerBorder>
                </LinearGaugePointer>
                
                <!-- Minimum recorded -->
                <LinearGaugePointer PointerValue="-5" 
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Triangle"
                                    Height="15"
                                    Width="15"
                                    Color="#007bff">
                </LinearGaugePointer>
                
                <!-- Maximum recorded -->
                <LinearGaugePointer PointerValue="45" 
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.InvertedTriangle"
                                    Height="15"
                                    Width="15"
                                    Color="#ff6b6b">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Horizontal Progress Bar with Marker

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Orientation="Syncfusion.Blazor.LinearGauge.Orientation.Horizontal" Width="600px" Height="120px">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeAxisLabelStyle Format="{value}%">
            </LinearGaugeAxisLabelStyle>
            
            <LinearGaugePointers>
                <!-- Background bar -->
                <LinearGaugePointer PointerValue="100" 
                                    Type="Syncfusion.Blazor.LinearGauge.Point.Bar" 
                                    Width="30"
                                    Color="#e9ecef"
                                    Opacity="0.3">
                </LinearGaugePointer>
                
                <!-- Progress bar -->
                <LinearGaugePointer PointerValue="68" 
                                    Type="Syncfusion.Blazor.LinearGauge.Point.Bar" 
                                    Width="30"
                                    Color="#28a745"
                                    RoundedCornerRadius="5"
                                    AnimationDuration="1500">
                </LinearGaugePointer>
                
                <!-- Position marker -->
                <LinearGaugePointer PointerValue="68" 
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Diamond"
                                    Height="25"
                                    Width="25"
                                    Color="#155724"
                                    Placement="Syncfusion.Blazor.LinearGauge.Placement.Far">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Multi-Metric Display

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Width="250px" Height="400px">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <!-- CPU Usage -->
                <LinearGaugePointer PointerValue="65" 
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Circle"
                                    Height="18"
                                    Width="18"
                                    Color="#007bff"
                                    Offset="-25">
                </LinearGaugePointer>
                
                <!-- Memory Usage -->
                <LinearGaugePointer PointerValue="80" 
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Rectangle"
                                    Height="20"
                                    Width="12"
                                    Color="#28a745"
                                    Offset="0">
                </LinearGaugePointer>
                
                <!-- Disk Usage -->
                <LinearGaugePointer PointerValue="45" 
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Diamond"
                                    Height="18"
                                    Width="18"
                                    Color="#ffc107"
                                    Offset="25">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

## Key Takeaways

1. **Pointer Types:** Marker (shapes) and Bar (filled rectangles)
2. **Marker Shapes:** Circle, Rectangle, Triangle, Diamond, Image, Text, InvertedTriangle
3. **Customization:** Control size, color, opacity, borders, and positioning
4. **Multiple Pointers:** Display several values on one axis with different styles
5. **Animation:** Set duration for smooth transitions on load or value change
6. **Gradients:** Apply linear or radial gradients for enhanced visuals
7. **Positioning:** Use Placement and Offset for precise pointer positioning

Pointers are the primary way to display values in a Linear Gauge, and their extensive customization options allow for highly tailored visualizations.
