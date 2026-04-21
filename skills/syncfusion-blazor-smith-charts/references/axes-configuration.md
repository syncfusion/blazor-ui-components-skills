# Axes Configuration

## Table of Contents
- [Overview](#overview)
- [Axis Types](#axis-types)
- [Label Customization](#label-customization)
- [Font Styling](#font-styling)
- [Label Positioning](#label-positioning)
- [Label Intersection Handling](#label-intersection-handling)
- [Complete Examples](#complete-examples)

## Overview

Smith Charts use two distinct axis types to represent impedance coordinates:

- **Horizontal Axis** - Straight line drawn horizontally across the chart
- **Radial Axis** - Circular paths radiating from the center

Both axes support extensive customization for labels, fonts, positioning, and visibility. The `SmithChartHorizontalAxis` and `SmithChartRadialAxis` components provide properties for controlling axis appearance and behavior.

## Axis Types

### Horizontal Axis

The horizontal axis represents constant resistance circles. The `SmithChartHorizontalAxis` component with its `Visible` property controls the axis display:

```razor
@using Syncfusion.Blazor.Charts

<SfSmithChart>
    <SmithChartHorizontalAxis Visible="true">
    </SmithChartHorizontalAxis>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@Data" Reactance="Reactance" Resistance="Resistance">
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }

    public List<SmithChartData> Data = new()
    {
        new SmithChartData { Resistance = 10,  Reactance = 15 },
        new SmithChartData { Resistance = 7,   Reactance = 8 },
        new SmithChartData { Resistance = 5,   Reactance = 4 },
        new SmithChartData { Resistance = 3,   Reactance = 2 },
        new SmithChartData { Resistance = 1.5, Reactance = 1 },
        new SmithChartData { Resistance = 1,   Reactance = 0.2 }
    };
}
```

**Properties:**
- `Visible` - Show/hide the axis (default: `true`)
- `LabelPosition` - Place labels inside or outside
- `LabelIntersectAction` - Handle overlapping labels

### Radial Axis

The radial axis represents constant reactance arcs. The `SmithChartRadialAxis` component with its `Visible` property controls the axis display:

```razor
@using Syncfusion.Blazor.Charts

<SfSmithChart>
    <SmithChartRadialAxis Visible="true">
    </SmithChartRadialAxis>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@Data" Reactance="Reactance" Resistance="Resistance">
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }

    public List<SmithChartData> Data = new()
    {
        new SmithChartData { Resistance = 10,  Reactance = 15 },
        new SmithChartData { Resistance = 7,   Reactance = 8 },
        new SmithChartData { Resistance = 5,   Reactance = 4 },
        new SmithChartData { Resistance = 3,   Reactance = 2 },
        new SmithChartData { Resistance = 1.5, Reactance = 1 },
        new SmithChartData { Resistance = 1,   Reactance = 0.2 }
    };
}
```

## Label Customization

### Horizontal Axis Labels

Customize label appearance with font styling using the `SmithChartHorizontalAxisLabelStyle` component with properties like `FontFamily`, `FontWeight`, `FontStyle`, `Size`, `Color`, and `Opacity`:

```razor
@using Syncfusion.Blazor.Charts

<SfSmithChart>
    <SmithChartHorizontalAxis Visible="true" LabelPosition="@AxisLabelPosition.Outside">
        <SmithChartHorizontalAxisLabelStyle 
            FontFamily="Arial"
            FontWeight="bold"
            FontStyle="normal"
            Size="14px"
            Color="#333333"
            Opacity="1.0">
        </SmithChartHorizontalAxisLabelStyle>
    </SmithChartHorizontalAxis>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@TransmissionData" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };
}
```

### Radial Axis Labels

Apply custom styling to radial axis labels using the `SmithChartRadialAxisLabelStyle` component with properties like `FontFamily`, `FontWeight`, `FontStyle`, `Size`, `Color`, and `Opacity`:

```razor
@using Syncfusion.Blazor.Charts

<SfSmithChart>
    <SmithChartRadialAxis Visible="true" LabelPosition="@AxisLabelPosition.Inside">
        <SmithChartRadialAxisLabelStyle 
            FontFamily="Segoe UI"
            FontWeight="600"
            FontStyle="italic"
            Size="12px"
            Color="#666666"
            Opacity="0.9">
        </SmithChartRadialAxisLabelStyle>
    </SmithChartRadialAxis>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@TransmissionData" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };
}
```

## Font Styling

### Font Properties

Both `SmithChartHorizontalAxisLabelStyle` and `SmithChartRadialAxisLabelStyle` components support these font properties:

| Property | Type | Description | Default | Options |
|----------|------|-------------|---------|---------|
| `FontFamily` | string | Font typeface | "Segoe UI" | Any valid CSS font family |
| `FontWeight` | string | Font thickness | "normal" | "normal", "bold", "lighter", "100"-"900" |
| `FontStyle` | string | Font style | "normal" | "normal", "italic", "oblique" |
| `Size` | string | Font size | "12px" | Any valid CSS size (px, em, rem) |
| `Color` | string | Label color | "#000000" | Hex, RGB, RGBA, named colors |
| `Opacity` | double | Label transparency | 1.0 | 0.0 (transparent) to 1.0 (opaque) |

### Font Family Options

Configure the `FontFamily` property of label style components to set the typeface:

```razor
<!-- Professional/Technical -->
<SmithChartHorizontalAxisLabelStyle FontFamily="Segoe UI">
</SmithChartHorizontalAxisLabelStyle>

<!-- Monospace for precise values -->
<SmithChartHorizontalAxisLabelStyle FontFamily="Consolas">
</SmithChartHorizontalAxisLabelStyle>

<!-- Classic serif -->
<SmithChartHorizontalAxisLabelStyle FontFamily="Times New Roman">
</SmithChartHorizontalAxisLabelStyle>

<!-- Sans-serif modern -->
<SmithChartHorizontalAxisLabelStyle FontFamily="Arial">
</SmithChartHorizontalAxisLabelStyle>
```

### Font Weight Examples

Control text thickness using the `FontWeight` property of label style components:

```razor
<!-- Light labels for background axes -->
<SmithChartRadialAxisLabelStyle FontWeight="300">
</SmithChartRadialAxisLabelStyle>

<!-- Normal weight (default) -->
<SmithChartRadialAxisLabelStyle FontWeight="normal">
</SmithChartRadialAxisLabelStyle>

<!-- Bold for emphasis -->
<SmithChartRadialAxisLabelStyle FontWeight="bold">
</SmithChartRadialAxisLabelStyle>

<!-- Extra bold -->
<SmithChartRadialAxisLabelStyle FontWeight="900">
</SmithChartRadialAxisLabelStyle>
```

## Label Positioning

### Position Options

Control where labels appear relative to axis lines using the `LabelPosition` property of `SmithChartHorizontalAxis` and `SmithChartRadialAxis` components:

```razor
<!-- Labels outside the chart area -->
<SmithChartHorizontalAxis LabelPosition="@AxisLabelPosition.Outside">
</SmithChartHorizontalAxis>

<!-- Labels inside the chart area -->
<SmithChartRadialAxis LabelPosition="@AxisLabelPosition.Inside">
</SmithChartRadialAxis>
```

**`AxisLabelPosition` Enum:**
- `Outside` - Labels placed outside axis lines (more readable, uses more space)
- `Inside` - Labels placed inside axis lines (compact, may overlap with data)

### Positioning Strategy

**Use `Outside` for:**
- Primary horizontal axis with prominent values
- Charts with dense data near the perimeter
- Print/export where clarity is critical
- Large charts with ample margin space

**Use `Inside` for:**
- Radial axis to avoid clutter
- Compact charts with limited space
- Secondary or reference axes
- Dashboards with multiple charts

### Combined Positioning Example

```razor
@using Syncfusion.Blazor.Charts

<SfSmithChart Height="600px" Width="100%">
    <SmithChartTitle Text="Optimized Label Positioning"></SmithChartTitle>
    
    <!-- Horizontal axis: Outside for readability -->
    <SmithChartHorizontalAxis LabelPosition="@AxisLabelPosition.Outside">
        <SmithChartHorizontalAxisLabelStyle 
            FontFamily="Arial"
            FontWeight="600"
            Size="13px"
            Color="#2C3E50">
        </SmithChartHorizontalAxisLabelStyle>
    </SmithChartHorizontalAxis>
    
    <!-- Radial axis: Inside to save space -->
    <SmithChartRadialAxis LabelPosition="@AxisLabelPosition.Inside">
        <SmithChartRadialAxisLabelStyle 
            FontFamily="Arial"
            FontWeight="normal"
            Size="11px"
            Color="#7F8C8D"
            Opacity="0.85">
        </SmithChartRadialAxisLabelStyle>
    </SmithChartRadialAxis>
    
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@TransmissionData" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };
}
```

## Label Intersection Handling

When labels overlap, use the `LabelIntersectAction` property of `SmithChartHorizontalAxis` and `SmithChartRadialAxis` components to control behavior:

```razor
@using Syncfusion.Blazor.Charts

<SfSmithChart>
    <SmithChartRadialAxis 
        LabelPosition="@AxisLabelPosition.Inside"
        LabelIntersectAction="@SmithChartLabelIntersectAction.Hide">
        <SmithChartRadialAxisLabelStyle Size="12px">
        </SmithChartRadialAxisLabelStyle>
    </SmithChartRadialAxis>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@TransmissionData" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };
}
```

**`SmithChartLabelIntersectAction` Enum:**
- `None` - Show all labels (may overlap)
- `Hide` - Hide overlapping labels automatically

**When to Use:**

**`None` (Show all labels):**
- Charts with sufficient space between axis values
- High-resolution displays or print outputs
- Logarithmic or sparse axis spacing

**`Hide` (Auto-hide overlapping):**
- Compact charts with dense axis labels
- Responsive designs where chart size varies
- Radial axes with many closely-spaced values
- Mobile/tablet viewports

### Intersection Handling Example

```razor
@using Syncfusion.Blazor.Charts

<SfSmithChart Height="500px" Width="100%">
    <SmithChartRadialAxis 
        LabelPosition="@AxisLabelPosition.Inside"
        LabelIntersectAction="@SmithChartLabelIntersectAction.Hide">
        <SmithChartRadialAxisLabelStyle 
            Size="11px"
            Color="#555555">
        </SmithChartRadialAxisLabelStyle>
    </SmithChartRadialAxis>
    
    <SmithChartHorizontalAxis 
        LabelPosition="@AxisLabelPosition.Outside"
        LabelIntersectAction="@SmithChartLabelIntersectAction.None">
        <SmithChartHorizontalAxisLabelStyle 
            Size="12px"
            FontWeight="bold"
            Color="#333333">
        </SmithChartHorizontalAxisLabelStyle>
    </SmithChartHorizontalAxis>
    
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@TransmissionData" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };
}
```

## Complete Examples

### Professional Technical Documentation Style

```razor
@page "/smith-chart-professional"
@using Syncfusion.Blazor.Charts

<h3>RF Circuit Analysis - Professional Style</h3>

<SfSmithChart Height="700px" Width="100%">
    <SmithChartTitle Text="S11 Parameter Measurement"></SmithChartTitle>
    
    <!-- Horizontal Axis: Bold, outside placement -->
    <SmithChartHorizontalAxis 
        Visible="true"
        LabelPosition="@AxisLabelPosition.Outside"
        LabelIntersectAction="@SmithChartLabelIntersectAction.None">
        <SmithChartHorizontalAxisLabelStyle 
            FontFamily="Segoe UI"
            FontWeight="600"
            FontStyle="normal"
            Size="14px"
            Color="#1A1A1A"
            Opacity="1.0">
        </SmithChartHorizontalAxisLabelStyle>
    </SmithChartHorizontalAxis>
    
    <!-- Radial Axis: Subtle, inside placement -->
    <SmithChartRadialAxis 
        Visible="true"
        LabelPosition="@AxisLabelPosition.Inside"
        LabelIntersectAction="@SmithChartLabelIntersectAction.Hide">
        <SmithChartRadialAxisLabelStyle 
            FontFamily="Segoe UI"
            FontWeight="normal"
            FontStyle="normal"
            Size="12px"
            Color="#666666"
            Opacity="0.85">
        </SmithChartRadialAxisLabelStyle>
    </SmithChartRadialAxis>
    
    <SmithChartSeriesCollection>
        <SmithChartSeries Name="Antenna Impedance" 
                          DataSource="@TransmissionData" 
                          Reactance="Reactance" 
                          Resistance="Resistance"
                          Fill="#2E86AB"
                          Width="2.5">
            <SmithChartSeriesMarker Visible="true" 
                                    Shape="@Syncfusion.Blazor.Charts.Shape.Circle"
                                    Height="8"
                                    Width="8">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 8, Reactance = 10 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 4.5, Reactance = 2 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };
}
```

### High Contrast Accessibility Style

```razor
@page "/smith-chart-accessible"
@using Syncfusion.Blazor.Charts

<h3>High Contrast Accessible Smith Chart</h3>

<SfSmithChart Height="600px" Width="100%">
    <SmithChartTitle Text="Accessible Impedance Analysis"></SmithChartTitle>
    
    <!-- High contrast horizontal axis -->
    <SmithChartHorizontalAxis 
        Visible="true"
        LabelPosition="@AxisLabelPosition.Outside">
        <SmithChartHorizontalAxisLabelStyle 
            FontFamily="Arial"
            FontWeight="bold"
            Size="16px"
            Color="#000000"
            Opacity="1.0">
        </SmithChartHorizontalAxisLabelStyle>
    </SmithChartHorizontalAxis>
    
    <!-- High contrast radial axis -->
    <SmithChartRadialAxis 
        Visible="true"
        LabelPosition="@AxisLabelPosition.Inside"
        LabelIntersectAction="@SmithChartLabelIntersectAction.Hide">
        <SmithChartRadialAxisLabelStyle 
            FontFamily="Arial"
            FontWeight="600"
            Size="14px"
            Color="#000000"
            Opacity="1.0">
        </SmithChartRadialAxisLabelStyle>
    </SmithChartRadialAxis>
    
    <SmithChartSeriesCollection>
        <SmithChartSeries Name="Measurement Data" 
                          DataSource="@TransmissionData" 
                          Reactance="Reactance" 
                          Resistance="Resistance"
                          Fill="#000000"
                          Width="3">
            <SmithChartSeriesMarker Visible="true" 
                                    Shape="@Syncfusion.Blazor.Charts.Shape.Circle"
                                    Height="10"
                                    Width="10"
                                    Fill="#FFFF00">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 8, Reactance = 10 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 4.5, Reactance = 2 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };
}
```

### Compact Dashboard Style

```razor
@page "/smith-chart-compact"
@using Syncfusion.Blazor.Charts

<h3>Compact Dashboard Style</h3>

<SfSmithChart Height="400px" Width="100%">
    <!-- Minimal horizontal axis -->
    <SmithChartHorizontalAxis 
        Visible="true"
        LabelPosition="@AxisLabelPosition.Outside">
        <SmithChartHorizontalAxisLabelStyle 
            FontFamily="Arial"
            FontWeight="normal"
            Size="10px"
            Color="#4A4A4A"
            Opacity="0.9">
        </SmithChartHorizontalAxisLabelStyle>
    </SmithChartHorizontalAxis>
    
    <!-- Compact radial axis -->
    <SmithChartRadialAxis 
        Visible="true"
        LabelPosition="@AxisLabelPosition.Inside"
        LabelIntersectAction="@SmithChartLabelIntersectAction.Hide">
        <SmithChartRadialAxisLabelStyle 
            FontFamily="Arial"
            FontWeight="normal"
            Size="9px"
            Color="#6A6A6A"
            Opacity="0.8">
        </SmithChartRadialAxisLabelStyle>
    </SmithChartRadialAxis>
    
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@TransmissionData" 
                          Reactance="Reactance" 
                          Resistance="Resistance"
                          Width="1.5">
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 8, Reactance = 10 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 4.5, Reactance = 2 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };
}
```

## Best Practices

### Label Visibility

**Do:**
- Use larger fonts (14-16px) for horizontal axis (primary)
- Use smaller fonts (11-13px) for radial axis (secondary)
- Apply `Hide` intersection action for inside-positioned labels
- Use high contrast colors (#000000, #333333) for readability

**Don't:**
- Use fonts smaller than 9px (hard to read)
- Set opacity below 0.7 (labels become invisible)
- Place both axes outside on small charts (wastes space)
- Use decorative fonts for technical data

### Performance

- Axis rendering is optimized; customization doesn't impact performance
- Font loading: Use system fonts for faster rendering
- Label count: Smith Chart automatically manages label density

