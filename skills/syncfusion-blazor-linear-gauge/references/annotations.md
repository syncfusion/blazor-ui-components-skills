# Annotations in Blazor Linear Gauge

## Table of Contents
- [Overview](#overview)
- [Adding Basic Annotations](#adding-basic-annotations)
- [Annotation Content Types](#annotation-content-types)
  - [Text Annotations](#text-annotations)
  - [HTML Content Annotations](#html-content-annotations)
  - [Template Annotations](#template-annotations)
- [Annotation Positioning](#annotation-positioning)
  - [Using X and Y Coordinates](#using-x-and-y-coordinates)
  - [Using AxisValue](#using-axisvalue)
  - [Alignment Options](#alignment-options)
- [Customization](#customization)
- [Multiple Annotations](#multiple-annotations)
- [Practical Examples](#practical-examples)

## Overview

Annotations allow you to overlay custom content (text, HTML elements, or images) on specific areas of the Linear Gauge. They're useful for:

- **Adding labels or descriptions** to specific gauge regions
- **Displaying units** or measurement information
- **Showing dynamic values** alongside the gauge
- **Creating custom indicators** or warnings
- **Enhancing visual context** with images or icons

Any number of annotations can be added to a Linear Gauge component.

## Adding Basic Annotations

Create annotations using the `LinearGaugeAnnotation` component within `LinearGaugeAnnotations`.

### Simple Text Annotation

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAnnotations>
        <LinearGaugeAnnotation AxisValue="50" ZIndex="1" Content="50">
        </LinearGaugeAnnotation>
    </LinearGaugeAnnotations>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="50"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** The text "50" appears at axis value 50.

## Annotation Content Types

### Text Annotations

Simple text content displayed at a specific position.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAnnotations>
        <LinearGaugeAnnotation AxisValue="0" ZIndex="1" Content="40">
        </LinearGaugeAnnotation>
    </LinearGaugeAnnotations>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="40"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### HTML Content Annotations

Add styled HTML elements using `ContentTemplate`.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAnnotations>
        <LinearGaugeAnnotation AxisValue="0" ZIndex="1">
            <ContentTemplate>
                <div class="custom-annotation">40</div>
            </ContentTemplate>
        </LinearGaugeAnnotation>
    </LinearGaugeAnnotations>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="40"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

<style type="text/css">
    .custom-annotation {
        color: white;
        background-color: blue;
        height: 30px;
        width: 30px;
        border-radius: 15px;
        padding: 4px 0 0 6px;
        font-weight: bold;
    }
</style>
```

**Result:** A blue circular badge with white text showing "40".

### Template Annotations

Create complex annotations with dynamic content, images, and formatting.

#### Unit Label Annotation

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAnnotations>
        <LinearGaugeAnnotation ZIndex="1" AxisValue="100" X="-80" Y="-30">
            <ContentTemplate>
                <div style="background: #f0f0f0; padding: 5px 10px; border-radius: 4px;">
                    <strong>Temperature (°C)</strong>
                </div>
            </ContentTemplate>
        </LinearGaugeAnnotation>
    </LinearGaugeAnnotations>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="65"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

#### Image Annotation

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAnnotations>
        <LinearGaugeAnnotation ZIndex="1" AxisValue="50" X="-40" Y="0">
            <ContentTemplate>
                <img src="warning-icon.png" width="30" height="30" alt="Warning" />
            </ContentTemplate>
        </LinearGaugeAnnotation>
    </LinearGaugeAnnotations>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="50"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Use case:** Warning icons, status indicators, branding elements.

## Annotation Positioning

Annotations can be positioned using multiple methods for precise placement.

### Using X and Y Coordinates

Position annotations using pixel coordinates relative to the gauge container.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAnnotations>
        <LinearGaugeAnnotation AxisValue="0" ZIndex="1" X="50" Y="-100">
            <ContentTemplate>
                <div class="custom-annotation">40</div>
            </ContentTemplate>
        </LinearGaugeAnnotation>
    </LinearGaugeAnnotations>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="40"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

<style>
    .custom-annotation {
        color: white;
        background-color: blue;
        height: 30px;
        width: 30px;
        border-radius: 15px;
        padding: 4px 0 0 6px;
        font-weight: bold;
    }
</style>
```

**Properties:**
- **X:** Horizontal position in pixels (positive = right, negative = left)
- **Y:** Vertical position in pixels (positive = down, negative = up)

### Using AxisValue

Position annotations at specific axis values for value-aligned placement.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAnnotations>
        <LinearGaugeAnnotation AxisValue="75" ZIndex="1">
            <ContentTemplate>
                <div style="color: red; font-weight: bold;">WARNING</div>
            </ContentTemplate>
        </LinearGaugeAnnotation>
    </LinearGaugeAnnotations>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="80"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Annotation positioned at axis value 75.

#### With Multiple Axes

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAnnotations>
        <!-- Annotation on first axis -->
        <LinearGaugeAnnotation ZIndex="1" AxisValue="50" AxisIndex="0">
            <ContentTemplate>
                <span style="color: blue;">Axis 1: 50</span>
            </ContentTemplate>
        </LinearGaugeAnnotation>
        
        <!-- Annotation on second axis -->
        <LinearGaugeAnnotation ZIndex="1" AxisValue="30" AxisIndex="1">
            <ContentTemplate>
                <span style="color: green;">Axis 2: 30</span>
            </ContentTemplate>
        </LinearGaugeAnnotation>
    </LinearGaugeAnnotations>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="50"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
        <LinearGaugeAxis Minimum="0" Maximum="50" OpposedPosition="true">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="30"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Properties:**
- **AxisValue:** Value on the axis where annotation appears
- **AxisIndex:** Index of the axis (0-based) for multi-axis gauges

### Alignment Options

Control horizontal and vertical alignment when X and Y are not specified.

**Alignment values:**
- `Placement.Center` - Center alignment (default)
- `Placement.Far` - Far edge (right/bottom)
- `Placement.Near` - Near edge (left/top)
- `Placement.None` - No specific alignment

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAnnotations>
        <LinearGaugeAnnotation ZIndex="1"
                               HorizontalAlignment="Syncfusion.Blazor.LinearGauge.Placement.Center"
                               VerticalAlignment="Syncfusion.Blazor.LinearGauge.Placement.Center">
            <ContentTemplate>
                <div class="custom-annotation">40</div>
            </ContentTemplate>
        </LinearGaugeAnnotation>
    </LinearGaugeAnnotations>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="40"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

<style>
    .custom-annotation {
        color: white;
        background-color: blue;
        height: 30px;
        width: 30px;
        border-radius: 15px;
        padding: 4px 0 0 6px;
        font-weight: bold;
    }
</style>
```

**Result:** Annotation centered both horizontally and vertically on the gauge.

**Note:** HorizontalAlignment and VerticalAlignment are not effective when X and Y properties are set.

## Customization

### Z-Index Layering

Control the stacking order of overlapping annotations using `ZIndex`.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAnnotations>
        <!-- Background annotation (lower z-index) -->
        <LinearGaugeAnnotation AxisValue="50" ZIndex="0" X="-20" Y="0">
            <ContentTemplate>
                <div style="background: lightgray; padding: 25px; border-radius: 8px;">
                    Background
                </div>
            </ContentTemplate>
        </LinearGaugeAnnotation>
        
        <!-- Foreground annotation (higher z-index) -->
        <LinearGaugeAnnotation AxisValue="50" ZIndex="2" X="0" Y="0">
            <ContentTemplate>
                <div style="background: blue; color: white; padding: 5px; border-radius: 4px;">
                    Foreground
                </div>
            </ContentTemplate>
        </LinearGaugeAnnotation>
    </LinearGaugeAnnotations>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="50"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Higher ZIndex = Appears on top**

### Styled Annotations

Apply CSS styles for enhanced visual appearance.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAnnotations>
        <LinearGaugeAnnotation ZIndex="1" AxisValue="60" X="-50" Y="10">
            <ContentTemplate>
                <div class="styled-annotation">
                    <div class="annotation-title">Current Value</div>
                    <div class="annotation-value">60</div>
                    <div class="annotation-unit">°C</div>
                </div>
            </ContentTemplate>
        </LinearGaugeAnnotation>
    </LinearGaugeAnnotations>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="60"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

<style>
    .styled-annotation {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        padding: 15px;
        border-radius: 8px;
        box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        text-align: center;
        min-width: 100px;
    }
    .annotation-title {
        font-size: 12px;
        opacity: 0.9;
        margin-bottom: 5px;
    }
    .annotation-value {
        font-size: 28px;
        font-weight: bold;
        margin-bottom: 5px;
    }
    .annotation-unit {
        font-size: 14px;
        opacity: 0.9;
    }
</style>
```

## Multiple Annotations

Add multiple annotations to display various pieces of information simultaneously.

### Basic Multiple Annotations

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAnnotations>
        <!-- Title annotation -->
        <LinearGaugeAnnotation ZIndex="1" AxisValue="100" X="-110" Y="-35">
            <ContentTemplate>
                <div class="custom-annotation">Speed to get higher mileage</div>
            </ContentTemplate>
        </LinearGaugeAnnotation>
        
        <!-- Value annotation -->
        <LinearGaugeAnnotation AxisValue="0" ZIndex="1">
            <ContentTemplate>
                <div class="speed">40</div>
            </ContentTemplate>
        </LinearGaugeAnnotation>
    </LinearGaugeAnnotations>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="40"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

<style>
    .speed {
        color: white;
        background-color: blue;
        height: 30px;
        width: 30px;
        border-radius: 15px;
        padding: 4px 0 0 6px;
        font-weight: bold;
    }
    .custom-annotation {
        background-color: lightgray;
        width: 210px;
        padding: 2px 5px;
    }
</style>
```

### Annotated Zones

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Width="200px" Height="450px">
    <LinearGaugeAnnotations>
        <!-- Low zone label -->
        <LinearGaugeAnnotation ZIndex="1" AxisValue="15" X="-60" Y="0">
            <ContentTemplate>
                <span style="color: #28a745; font-weight: bold;">SAFE</span>
            </ContentTemplate>
        </LinearGaugeAnnotation>
        
        <!-- Medium zone label -->
        <LinearGaugeAnnotation ZIndex="1" AxisValue="50" X="-95" Y="0">
            <ContentTemplate>
                <span style="color: #ffc107; font-weight: bold;">WARNING</span>
            </ContentTemplate>
        </LinearGaugeAnnotation>
        
        <!-- High zone label -->
        <LinearGaugeAnnotation ZIndex="1" AxisValue="85" X="-85" Y="0">
            <ContentTemplate>
                <span style="color: #dc3545; font-weight: bold;">DANGER</span>
            </ContentTemplate>
        </LinearGaugeAnnotation>
    </LinearGaugeAnnotations>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeRanges>
                <LinearGaugeRange Start="0" End="30" Color="#4CAF50"
                                  StartWidth="20" EndWidth="20">
                </LinearGaugeRange>
                <LinearGaugeRange Start="30" End="70" Color="#FFC107"
                                  StartWidth="20" EndWidth="20">
                </LinearGaugeRange>
                <LinearGaugeRange Start="70" End="100" Color="#F44336"
                                  StartWidth="20" EndWidth="20">
                </LinearGaugeRange>
            </LinearGaugeRanges>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="65"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Use case:** Labeling ranges, providing zone descriptions.

## Practical Examples

### Dynamic Value Display

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Width="200px" Height="400px">
    <LinearGaugeAnnotations>
        <LinearGaugeAnnotation ZIndex="1" AxisValue="@currentValue" X="-80" Y="0">
            <ContentTemplate>
                <div class="value-display">
                    <div class="value">@currentValue.ToString("F1")</div>
                    <div class="unit">km/h</div>
                </div>
            </ContentTemplate>
        </LinearGaugeAnnotation>
    </LinearGaugeAnnotations>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="200">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="@currentValue"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private double currentValue = 85.5;
}

<style>
    .value-display {
        background: #007bff;
        color: white;
        padding: 10px 15px;
        border-radius: 6px;
        text-align: center;
    }
    .value {
        font-size: 24px;
        font-weight: bold;
    }
    .unit {
        font-size: 12px;
        opacity: 0.9;
    }
</style>
```

### Threshold Indicator

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAnnotations>
        <!-- Threshold marker -->
        <LinearGaugeAnnotation ZIndex="1" AxisValue="75" X="20" Y="0">
            <ContentTemplate>
                <div style="display: flex; align-items: center;">
                    <div style="width: 0; height: 0; 
                                border-top: 8px solid transparent;
                                border-bottom: 8px solid transparent;
                                border-right: 12px solid red;">
                    </div>
                    <span style="color: red; font-weight: bold; margin-left: 5px;">
                        MAX
                    </span>
                </div>
            </ContentTemplate>
        </LinearGaugeAnnotation>
    </LinearGaugeAnnotations>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="85" Color="#dc3545">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Information Panel

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Width="250px" Height="450px">
    <LinearGaugeAnnotations>
        <LinearGaugeAnnotation ZIndex="1" HorizontalAlignment="Syncfusion.Blazor.LinearGauge.Placement.Center" 
                               VerticalAlignment="Syncfusion.Blazor.LinearGauge.Placement.Far"
                               Y="-20">
            <ContentTemplate>
                <div class="info-panel">
                    <div class="info-row">
                        <span class="label">Current:</span>
                        <span class="value">65°C</span>
                    </div>
                    <div class="info-row">
                        <span class="label">Max:</span>
                        <span class="value">85°C</span>
                    </div>
                    <div class="info-row">
                        <span class="label">Status:</span>
                        <span class="value status-ok">NORMAL</span>
                    </div>
                </div>
            </ContentTemplate>
        </LinearGaugeAnnotation>
    </LinearGaugeAnnotations>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="65"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

<style>
    .info-panel {
        background: white;
        border: 1px solid #dee2e6;
        border-radius: 6px;
        padding: 15px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    .info-row {
        display: flex;
        justify-content: space-between;
        margin-bottom: 8px;
    }
    .info-row:last-child {
        margin-bottom: 0;
    }
    .label {
        font-weight: 600;
        color: #6c757d;
    }
    .value {
        color: #212529;
    }
    .status-ok {
        color: #28a745;
        font-weight: bold;
    }
</style>
```

## Key Takeaways

1. **Purpose:** Annotations add custom content to specific gauge areas
2. **Content Types:** Text (`Content`), HTML (`ContentTemplate`), or dynamic templates
3. **Positioning:** Use `AxisValue` for value-aligned or `X`/`Y` for pixel positioning
4. **Alignment:** `HorizontalAlignment` and `VerticalAlignment` when X/Y not set
5. **Layering:** Control overlap with `ZIndex` property
6. **Multiple Axes:** Use `AxisIndex` for multi-axis gauge annotations
7. **Styling:** Apply custom CSS for enhanced visual design

Annotations provide flexible ways to add context, labels, values, and custom visuals to enhance gauge readability and user understanding.
