# Appearance and Styling in Blazor Linear Gauge

## Table of Contents
- [Overview](#overview)
- [Gauge Container Customization](#gauge-container-customization)
  - [Width and Height](#width-and-height)
  - [Background and Border](#background-and-border)
  - [Margins](#margins)
- [Title Customization](#title-customization)
- [Container Types](#container-types)
  - [Normal Container](#normal-container)
  - [Rounded Rectangle](#rounded-rectangle)
  - [Thermometer](#thermometer)
- [Orientation](#orientation)
- [Themes](#themes)
- [Custom CSS Styling](#custom-css-styling)
- [Responsive Design](#responsive-design)
- [Print Styling](#print-styling)
- [Complete Examples](#complete-examples)

## Overview

The Blazor Linear Gauge offers extensive customization options for appearance and styling. You can control:

- **Container dimensions** (width, height)
- **Colors and backgrounds**
- **Borders and margins**
- **Title styling**
- **Container shapes** (normal, rounded, thermometer)
- **Orientation** (vertical, horizontal)
- **Themes** (Bootstrap, Material, Fabric, etc.)
- **Custom CSS styling**
- **Responsive behavior**

This guide covers all styling options in a self-contained manner.

## Gauge Container Customization

### Width and Height

Control the overall gauge size using `Width` and `Height` properties on `SfLinearGauge`.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Width="200px" Height="400px">
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="60"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Supported units:**
- **Pixels:** "400px"
- **Percentage:** "100%"
- **Auto:** Let the gauge determine size

#### Horizontal Gauge Dimensions

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Orientation="Syncfusion.Blazor.LinearGauge.Orientation.Horizontal" Width="600px" Height="150px">
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="70"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Default sizes:**
- **Vertical:** Width: auto, Height: 450px
- **Horizontal:** Width: auto, Height: 150px

### Background and Border

Customize the gauge area background and border.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Width="200px" Height="400px" Background="skyblue">
    <LinearGaugeBorder Color="#FF0000" Width="2"></LinearGaugeBorder>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Properties:**
- `Background`: Background color (hex, RGB, or color name)
- `LinearGaugeBorder`:
    - `Color` - Border color
    - `Width` - Border thickness in pixels

#### Styled Container

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Width="250px" Height="450px" Background="#f8f9fa">
    <LinearGaugeBorder Color="#dee2e6" Width="1"></LinearGaugeBorder>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="75" Color="#007bff">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

#### Transparent Background

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Background="transparent">
    <LinearGaugeBorder Width="0"></LinearGaugeBorder>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Use case:** Embedding gauges into existing layouts without visible container.

### Margins

Adjust spacing around the gauge using `LinearGaugeMargin`.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Width="200px" Height="400px" Background="skyblue">
    <LinearGaugeBorder Color="#FF0000" Width="2"></LinearGaugeBorder>
    <LinearGaugeMargin Left="20" Right="20" Top="20" Bottom="20"></LinearGaugeMargin>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Properties:**
- `Left`: Left margin in pixels
- `Right`: Right margin in pixels
- `Top`: Top margin in pixels
- `Bottom`: Bottom margin in pixels

#### Remove All Margins

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge AllowMargin="false" Width="100%" Height="100%">
    <LinearGaugeMargin Left="0" Right="0" Top="0" Bottom="0"></LinearGaugeMargin>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Gauge fills the entire container with no margin.

**Use case:** Dashboard panels, embedded widgets requiring precise fit.

## Title Customization

Add and style titles to provide context for your gauge using the `Title` property in the `SfLinearGauge`.

### Basic Title

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Title="Temperature Monitor">
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="45"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Styled Title

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Title="Server Performance">
    <LinearGaugeTitleStyle 
        FontFamily="Arial" 
        FontWeight="regular" 
        FontStyle="italic" 
        Color="#E27F2D" 
        Size="23px">
    </LinearGaugeTitleStyle>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Title Style Properties:**
- `Color`: Text color
- `FontFamily`: Font name
- `FontWeight`: normal, bold, lighter, bolder, or numeric (100-900)
- `FontStyle`: normal, italic, oblique
- `Size`: Font size (e.g., "16px")
- `Opacity`: Text opacity (0-1)

### Complete Title Example

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Title="CPU Usage Monitor" Width="250px" Height="450px">
    <LinearGaugeTitleStyle 
        Color="#212529"
        FontFamily="Segoe UI"
        FontWeight="bold"
        Size="18px"
        Opacity="1">
    </LinearGaugeTitleStyle>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeAxisLabelStyle Format="{value}%">
            </LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="68"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

## Container Types

The inner container (where ranges and pointers render) can have different shapes. The type can be customized using the `Type` property of the `LinearGaugeContainer`.

### Normal Container

The default rectangular container.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeContainer Width="30" Type="ContainerType.Normal">
        <LinearGaugeAxes>
            <LinearGaugeAxis>
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="50" 
                                        Width="15" 
                                        Type="Syncfusion.Blazor.LinearGauge.Point.Bar"
                                        Color="#a6a6a6">
                    </LinearGaugePointer>
                </LinearGaugePointers>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </LinearGaugeContainer>
</SfLinearGauge>
```

**Result:** Standard rectangular container with sharp corners.

### Rounded Rectangle

Container with rounded corners for a modern look.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeContainer Width="30" 
                          Type="ContainerType.RoundedRectangle"
                          RoundedCornerRadius="10">
        <LinearGaugeAxes>
            <LinearGaugeAxis>
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="50" 
                                        Width="15" 
                                        Type="Syncfusion.Blazor.LinearGauge.Point.Bar"
                                        Color="#a6a6a6">
                    </LinearGaugePointer>
                </LinearGaugePointers>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </LinearGaugeContainer>
</SfLinearGauge>
```

**Properties:**
- `RoundedCornerRadius`: Corner radius in pixels (default: 10)

**Use case:** Modern UI designs, mobile-friendly interfaces.

### Thermometer

Special container that resembles a thermometer with a bulb at the bottom.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeContainer Width="30" Type="ContainerType.Thermometer">
        <LinearGaugeAxes>
            <LinearGaugeAxis>
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="80" 
                                        Width="15" 
                                        Type="Syncfusion.Blazor.LinearGauge.Point.Bar"
                                        Color="#ff6b6b">
                    </LinearGaugePointer>
                </LinearGaugePointers>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </LinearGaugeContainer>
</SfLinearGauge>
```

**Result:** Thermometer-style gauge with characteristic bulb shape.

**Use case:** Temperature displays, medical instruments, weather applications.

### Container Customization Properties

All container types support these properties:

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeContainer 
        Width="40"
        Height="300"
        Offset="10"
        BackgroundColor="#e9ecef"
        Type="ContainerType.RoundedRectangle"
        RoundedCornerRadius="8">
        <LinearGaugeContainerBorder Color="#dee2e6" Width="2">
        </LinearGaugeContainerBorder>
        <LinearGaugeAxes>
            <LinearGaugeAxis>
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="65" 
                                        Type="Syncfusion.Blazor.LinearGauge.Point.Bar"
                                        Width="40"
                                        Color="#007bff">
                    </LinearGaugePointer>
                </LinearGaugePointers>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </LinearGaugeContainer>
</SfLinearGauge>
```

**Properties:**
- `Width`: Container width in pixels
- `Height`: Container height in pixels or percentage
- `Offset`: Distance from axis
- `BackgroundColor`: Container fill color
- `LinearGaugeContainerBorder.Color`: Border color
- `LinearGaugeContainerBorder.Width`: Border thickness

## Orientation

Change the gauge direction between vertical and horizontal.

### Vertical Orientation (Default)

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Orientation="Syncfusion.Blazor.LinearGauge.Orientation.Vertical" Width="200px" Height="400px">
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="60"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Best for:** Thermometers, tower displays, vertical meters.

### Horizontal Orientation

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Orientation="Syncfusion.Blazor.LinearGauge.Orientation.Horizontal" Width="600px" Height="150px">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeMajorTicks Interval="10"></LinearGaugeMajorTicks>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="75"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Best for:** Dashboard widgets, progress bars, horizontal sliders.

## Themes

Syncfusion Blazor Linear Gauge supports multiple built-in themes for consistent styling.

### Available Themes

Apply themes by changing the stylesheet reference in your HTML head:

```html
<head>
    <!-- Bootstrap 5 Theme (default) -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    
    <!-- Or choose another theme: -->
    <!-- <link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" /> -->
    <!-- <link href="_content/Syncfusion.Blazor.Themes/fabric.css" rel="stylesheet" /> -->
    <!-- <link href="_content/Syncfusion.Blazor.Themes/bootstrap.css" rel="stylesheet" /> -->
    <!-- <link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" /> -->
    <!-- <link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" /> -->
    <!-- <link href="_content/Syncfusion.Blazor.Themes/material3.css" rel="stylesheet" /> -->
</head>
```

**Available Themes:**
- **bootstrap5.css** - Bootstrap 5 styling
- **material.css** - Material Design
- **fabric.css** - Microsoft Fabric
- **bootstrap.css** - Bootstrap 4
- **tailwind.css** - Tailwind CSS
- **fluent.css** - Fluent Design
- **material3.css** - Material Design 3
- **bootstrap5-dark.css** - Dark mode variants
- **material-dark.css**
- **fabric-dark.css**

### Theme Application

No code changes needed in components - themes are applied automatically based on the stylesheet:

```razor
@using Syncfusion.Blazor.LinearGauge

<!-- This gauge will use the theme from your stylesheet -->
<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="60"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

## Custom CSS Styling

Apply custom styles to gauge elements using CSS classes.

### Container Styling

```razor
@using Syncfusion.Blazor.LinearGauge

<div class="gauge-wrapper">
    <SfLinearGauge Width="200px" Height="400px">
        <LinearGaugeAxes>
            <LinearGaugeAxis>
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="65"></LinearGaugePointer>
                </LinearGaugePointers>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </SfLinearGauge>
</div>

<style>
    .gauge-wrapper {
        padding: 20px;
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        border-radius: 12px;
        box-shadow: 0 10px 25px rgba(0,0,0,0.2);
    }
</style>
```

### Card-Style Gauge

```razor
@using Syncfusion.Blazor.LinearGauge

<div class="gauge-card">
    <div class="gauge-header">
        <h3>Temperature</h3>
        <span class="gauge-status">NORMAL</span>
    </div>
    <SfLinearGauge Width="100%" Height="300px" Background="transparent">
        <LinearGaugeBorder Width="0"></LinearGaugeBorder>
        <LinearGaugeAxes>
            <LinearGaugeAxis Minimum="0" Maximum="100">
                <LinearGaugeAxisLabelStyle Format="{value}°C">
                </LinearGaugeAxisLabelStyle>
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="68" Color="#28a745">
                    </LinearGaugePointer>
                </LinearGaugePointers>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </SfLinearGauge>
</div>

<style>
    .gauge-card {
        background: white;
        border-radius: 8px;
        padding: 20px;
        box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }
    .gauge-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 15px;
    }
    .gauge-header h3 {
        margin: 0;
        font-size: 18px;
        color: #212529;
    }
    .gauge-status {
        background: #d4edda;
        color: #155724;
        padding: 4px 12px;
        border-radius: 12px;
        font-size: 12px;
        font-weight: 600;
    }
</style>
```

### Dashboard Panel Style

```razor
@using Syncfusion.Blazor.LinearGauge

<div class="dashboard-panel">
    <div class="panel-title">CPU Usage</div>
    <div class="panel-content">
        <SfLinearGauge Orientation="Syncfusion.Blazor.LinearGauge.Orientation.Horizontal" 
                       Width="100%" 
                       Height="80px"
                       Background="transparent">
            <LinearGaugeBorder Width="0"></LinearGaugeBorder>
            <LinearGaugeAxes>
                <LinearGaugeAxis Minimum="0" Maximum="100">
                    <LinearGaugeAxisLabelStyle Format="{value}%">
                    </LinearGaugeAxisLabelStyle>
                    <LinearGaugePointers>
                        <LinearGaugePointer PointerValue="72" 
                                            Type="Syncfusion.Blazor.LinearGauge.Point.Bar"
                                            Width="30"
                                            Color="#007bff">
                        </LinearGaugePointer>
                    </LinearGaugePointers>
                </LinearGaugeAxis>
            </LinearGaugeAxes>
        </SfLinearGauge>
    </div>
    <div class="panel-footer">
        <span>Current: 72%</span>
        <span>Max: 95%</span>
    </div>
</div>

<style>
    .dashboard-panel {
        background: #f8f9fa;
        border: 1px solid #dee2e6;
        border-radius: 6px;
        overflow: hidden;
    }
    .panel-title {
        background: #343a40;
        color: white;
        padding: 12px 16px;
        font-weight: 600;
    }
    .panel-content {
        padding: 20px;
    }
    .panel-footer {
        background: #e9ecef;
        padding: 10px 16px;
        display: flex;
        justify-content: space-between;
        font-size: 14px;
        color: #6c757d;
    }
</style>
```

## Responsive Design

Create gauges that adapt to different screen sizes.

### Percentage-Based Sizing

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Width="100%" Height="400px">
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="65"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Media Query Responsive

```razor
@using Syncfusion.Blazor.LinearGauge

<div class="responsive-gauge">
    <SfLinearGauge Width="100%" Height="100%">
        <LinearGaugeAxes>
            <LinearGaugeAxis>
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="75"></LinearGaugePointer>
                </LinearGaugePointers>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </SfLinearGauge>
</div>

<style>
    .responsive-gauge {
        width: 200px;
        height: 400px;
    }
    
    /* Tablet */
    @@media (max-width: 768px) {
        .responsive-gauge {
            width: 150px;
            height: 300px;
        }
    }
    
    /* Mobile */
    @@media (max-width: 480px) {
        .responsive-gauge {
            width: 100px;
            height: 250px;
        }
    }
</style>
```

### Container Query Approach

```razor
@using Syncfusion.Blazor.LinearGauge

<div class="gauge-container">
    <SfLinearGauge Width="100%" Height="100%" AllowMargin="false">
        <LinearGaugeMargin Left="0" Right="0" Top="0" Bottom="0"></LinearGaugeMargin>
        <LinearGaugeAxes>
            <LinearGaugeAxis>
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="68"></LinearGaugePointer>
                </LinearGaugePointers>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </SfLinearGauge>
</div>

<style>
    .gauge-container {
        width: 100%;
        max-width: 300px;
        height: 450px;
        margin: 0 auto;
    }
</style>
```

## Print Styling

The Linear Gauge supports printing with the `AllowPrint` property and `PrintAsync()` method.

### Basic Print Setup

```razor
@using Syncfusion.Blazor.LinearGauge

<button @onclick="PrintGauge">Print Gauge</button>

<SfLinearGauge @ref="gauge" AllowPrint="true">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeMajorTicks Interval="20"></LinearGaugeMajorTicks>
            <LinearGaugeMinorTicks Interval="10"></LinearGaugeMinorTicks>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="65"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    SfLinearGauge gauge;
    
    public async Task PrintGauge()
    {
        await gauge.PrintAsync();
    }
}
```

### Print-Friendly Styling

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge @ref="gauge" AllowPrint="true" Background="white">
    <LinearGaugeBorder Color="#000000" Width="1"></LinearGaugeBorder>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugeAxisLabelStyle>
                <LinearGaugeAxisLabelFont Color="#000000">
                </LinearGaugeAxisLabelFont>
            </LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="65" Color="#000000">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    SfLinearGauge gauge;
}
```

**Best practices for printing:**
- Use white or light backgrounds
- Use dark colors for text and pointers
- Avoid transparency effects
- Ensure sufficient contrast
- Test print preview before deployment

## Complete Examples

### Modern Dashboard Gauge

```razor
@using Syncfusion.Blazor.LinearGauge

<div class="modern-gauge">
    <SfLinearGauge Title="System Performance" 
                   Width="100%" 
                   Height="450px"
                   Background="linear-gradient(180deg, #f8f9fa 0%, #e9ecef 100%)">
        <LinearGaugeTitleStyle Color="#212529" 
                               FontWeight="bold" 
                               Size="20px">
        </LinearGaugeTitleStyle>
        <LinearGaugeBorder Color="#dee2e6" Width="1"></LinearGaugeBorder>
        <LinearGaugeMargin Left="15" Right="15" Top="15" Bottom="15">
        </LinearGaugeMargin>
        
        <LinearGaugeContainer Width="40" 
                              Type="ContainerType.RoundedRectangle"
                              RoundedCornerRadius="10"
                              BackgroundColor="#ffffff">
            <LinearGaugeContainerBorder Color="#ced4da" Width="1">
            </LinearGaugeContainerBorder>
            <LinearGaugeAxes>
                <LinearGaugeAxis Minimum="0" Maximum="100">
                    <LinearGaugeAxisLabelStyle Format="{value}%">
                        <LinearGaugeAxisLabelFont Color="#495057" Size="12px">
                        </LinearGaugeAxisLabelFont>
                    </LinearGaugeAxisLabelStyle>
                    
                    <LinearGaugeMajorTicks Height="10" Color="#6c757d">
                    </LinearGaugeMajorTicks>
                    <LinearGaugeMinorTicks Height="5" Color="#adb5bd">
                    </LinearGaugeMinorTicks>
                    
                    <LinearGaugePointers>
                        <LinearGaugePointer PointerValue="72" 
                                            Type="PointerType.Bar"
                                            Width="40"
                                            Color="#007bff"
                                            RoundedCornerRadius="10"
                                            AnimationDuration="1500">
                        </LinearGaugePointer>
                    </LinearGaugePointers>
                    
                    <LinearGaugeRanges>
                        <LinearGaugeRange Start="0" End="50" Color="#28a745" 
                                          StartWidth="8" EndWidth="8">
                        </LinearGaugeRange>
                        <LinearGaugeRange Start="50" End="75" Color="#ffc107" 
                                          StartWidth="8" EndWidth="8">
                        </LinearGaugeRange>
                        <LinearGaugeRange Start="75" End="100" Color="#dc3545" 
                                          StartWidth="8" EndWidth="8">
                        </LinearGaugeRange>
                    </LinearGaugeRanges>
                </LinearGaugeAxis>
            </LinearGaugeAxes>
        </LinearGaugeContainer>
    </SfLinearGauge>
</div>

<style>
    .modern-gauge {
        max-width: 300px;
        box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        border-radius: 12px;
        overflow: hidden;
    }
</style>
```

### Minimalist Horizontal Gauge

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Orientation="Syncfusion.Blazor.LinearGauge.Orientation.Horizontal" 
               Width="600px" 
               Height="100px"
               Background="transparent">
    <LinearGaugeBorder Width="0"></LinearGaugeBorder>
    <LinearGaugeMargin Left="0" Right="0" Top="0" Bottom="0">
    </LinearGaugeMargin>
    
    <LinearGaugeContainer Width="20" BackgroundColor="#e9ecef">
        <LinearGaugeContainerBorder Width="0"></LinearGaugeContainerBorder>
        <LinearGaugeAxes>
            <LinearGaugeAxis>
                <LinearGaugeLine Width="0"></LinearGaugeLine>
                <LinearGaugeMajorTicks Height="0"></LinearGaugeMajorTicks>
                <LinearGaugeMinorTicks Height="0"></LinearGaugeMinorTicks>
                <LinearGaugeAxisLabelStyle>
                    <LinearGaugeAxisLabelFont Size="0px">
                    </LinearGaugeAxisLabelFont>
                </LinearGaugeAxisLabelStyle>
                
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="65" 
                                        Type="Syncfusion.Blazor.LinearGauge.Point.Bar"
                                        Width="20"
                                        Color="#28a745"
                                        RoundedCornerRadius="10">
                    </LinearGaugePointer>
                </LinearGaugePointers>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </LinearGaugeContainer>
</SfLinearGauge>
```

### Thermometer Style Temperature Gauge

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Title="Room Temperature" Width="150px" Height="500px">
    <LinearGaugeTitleStyle Color="#212529" FontWeight="600" Size="16px">
    </LinearGaugeTitleStyle>
    
    <LinearGaugeContainer Width="30" 
                          Type="ContainerType.Thermometer"
                          BackgroundColor="#f0f0f0">
        <LinearGaugeContainerBorder Color="#cccccc" Width="2">
        </LinearGaugeContainerBorder>
        <LinearGaugeAxes>
            <LinearGaugeAxis Minimum="0" Maximum="50">
                <LinearGaugeAxisLabelStyle Format="{value}°C">
                    <LinearGaugeAxisLabelFont Color="#495057" Size="11px">
                    </LinearGaugeAxisLabelFont>
                </LinearGaugeAxisLabelStyle>
                
                <LinearGaugeMajorTicks Interval="10" Height="8" Color="#6c757d">
                </LinearGaugeMajorTicks>
                <LinearGaugeMinorTicks Interval="2" Height="4" Color="#adb5bd">
                </LinearGaugeMinorTicks>
                
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="22" 
                                        Type="Syncfusion.Blazor.LinearGauge.Point.Bar"
                                        Width="30"
                                        Color="#dc3545"
                                        AnimationDuration="2000">
                    </LinearGaugePointer>
                </LinearGaugePointers>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </LinearGaugeContainer>
</SfLinearGauge>
```

## Key Takeaways

1. **Dimensions:** Set width and height with pixels or percentages
2. **Background:** Customize with colors, gradients, or transparency
3. **Borders:** Add definition with color and width
4. **Margins:** Control spacing with Left, Right, Top, Bottom
5. **Title:** Add context with styled titles
6. **Container Types:** Normal, RoundedRectangle, or Thermometer
7. **Orientation:** Vertical (default) or Horizontal
8. **Themes:** Multiple built-in themes via stylesheet
9. **Custom CSS:** Wrap in styled containers for enhanced design
10. **Responsive:** Use percentage widths and media queries
11. **Print:** Enable with AllowPrint and PrintAsync() method

Comprehensive styling options allow you to create gauges that perfectly match your application's design language and requirements.
