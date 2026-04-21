# Smith Chart Legend Configuration

## Table of Contents
- [Overview](#overview)
- [Basic Legend Setup](#basic-legend-setup)
  - [Enabling Legend](#enabling-legend)
  - [Legend Visibility Toggle](#legend-visibility-toggle)
- [Legend Positioning](#legend-positioning)
  - [Predefined Positions](#predefined-positions)
  - [Custom Positioning](#custom-positioning)
  - [Legend Alignment](#legend-alignment)
- [Legend Appearance](#legend-appearance)
  - [Legend Shapes](#legend-shapes)
  - [Legend Sizing](#legend-sizing)
  - [Padding Configuration](#padding-configuration)
  - [Legend Borders](#legend-borders)
- [Legend Item Styling](#legend-item-styling)
  - [Item Size](#item-size)
  - [Text Styling](#text-styling)
- [Advanced Features](#advanced-features)
  - [Row and Column Layout](#row-and-column-layout)
  - [Legend Title](#legend-title)
  - [Toggle Visibility](#toggle-visibility)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

## Overview

The legend in a Smith Chart provides a visual reference for identifying different data series. It displays the series names along with corresponding shapes and colors. This guide covers all aspects of legend configuration, from basic setup to advanced customization.

## Basic Legend Setup

### Enabling Legend

Enable the legend using `SmithChartLegendSettings` component with the `Visible` property set to `true` to display the legend on the Smith Chart:

```razor
@page "/smithchart-legend-basic"
@using Syncfusion.Blazor.Charts

<h3>Smith Chart with Legend</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@Series1Data" 
                          Name="Transmission Line 1" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@Series2Data" 
                          Name="Transmission Line 2" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true">
    </SmithChartLegendSettings>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> Series1Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.15, Reactance = 0.15 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.35, Reactance = 0.35 }
    };

    public List<SmithChartData> Series2Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.4, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.5, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.6, Reactance = 0.4 }
    };
}
```

### Legend Visibility Toggle

Allow users to show/hide series by clicking legend items using the `ToggleVisibility` property set to `true` in `SmithChartLegendSettings`:

```razor
@page "/smithchart-legend-toggle"
@using Syncfusion.Blazor.Charts

<h3>Interactive Legend - Click to Toggle Series</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@InputImpedance" 
                          Name="Input Impedance" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" Shape="@Syncfusion.Blazor.Charts.Shape.Circle">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@OutputImpedance" 
                          Name="Output Impedance" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" Shape="@Syncfusion.Blazor.Charts.Shape.Diamond">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@LoadImpedance" 
                          Name="Load Impedance" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" Shape="@Syncfusion.Blazor.Charts.Shape.Triangle">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true" 
                             ToggleVisibility="true">
    </SmithChartLegendSettings>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> InputImpedance = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.2, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.3, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.4, Reactance = 0.4 }
    };

    public List<SmithChartData> OutputImpedance = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.5, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.6, Reactance = 0.4 },
        new SmithChartData { Resistance = 0.7, Reactance = 0.5 }
    };

    public List<SmithChartData> LoadImpedance = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.15, Reactance = 0.4 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.5 },
        new SmithChartData { Resistance = 0.35, Reactance = 0.6 }
    };
}
```

## Legend Positioning

### Predefined Positions

Position the legend at various locations on the chart using the `Position` property with `LegendPosition` enum values (Top, Bottom, Left, Right):

```razor
@page "/smithchart-legend-positions"
@using Syncfusion.Blazor.Charts

<h3>Legend Positioning Options</h3>

<div style="margin-bottom: 20px;">
    <label>
        <input type="radio" name="position" value="Top" @onchange="@(() => ChangePosition("Top"))" checked />
        Top
    </label>
    <label style="margin-left: 15px;">
        <input type="radio" name="position" value="Bottom" @onchange="@(() => ChangePosition("Bottom"))" />
        Bottom
    </label>
    <label style="margin-left: 15px;">
        <input type="radio" name="position" value="Left" @onchange="@(() => ChangePosition("Left"))" />
        Left
    </label>
    <label style="margin-left: 15px;">
        <input type="radio" name="position" value="Right" @onchange="@(() => ChangePosition("Right"))" />
        Right
    </label>
</div>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@Series1Data" 
                          Name="Series 1" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@Series2Data" 
                          Name="Series 2" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true" 
                             Position="@CurrentPosition">
    </SmithChartLegendSettings>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    private Syncfusion.Blazor.Charts.LegendPosition CurrentPosition = Syncfusion.Blazor.Charts.LegendPosition.Top;

    public List<SmithChartData> Series1Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.15, Reactance = 0.15 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.35, Reactance = 0.35 }
    };

    public List<SmithChartData> Series2Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.4, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.5, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.6, Reactance = 0.4 }
    };

    private void ChangePosition(string position)
    {
        CurrentPosition = position switch
        {
            "Top" => Syncfusion.Blazor.Charts.LegendPosition.Top,
            "Bottom" => Syncfusion.Blazor.Charts.LegendPosition.Bottom,
            "Left" => Syncfusion.Blazor.Charts.LegendPosition.Left,
            "Right" => Syncfusion.Blazor.Charts.LegendPosition.Right,
            _ => Syncfusion.Blazor.Charts.LegendPosition.Top
        };
    }
}
```

**Available Legend Positions:**
- `Top` - Above the chart
- `Bottom` - Below the chart
- `Left` - Left side of the chart
- `Right` - Right side of the chart
- `Custom` - Custom position using X and Y coordinates

### Custom Positioning

Place the legend at custom coordinates using the `Position` property set to `LegendPosition.Custom` along with `SmithChartLegendLocation` child component containing `X` and `Y` properties:

```razor
@page "/smithchart-legend-custom-position"
@using Syncfusion.Blazor.Charts

<h3>Custom Legend Position</h3>

<SfSmithChart Width="700px" Height="700px">
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@AntennaData" 
                          Name="Antenna S11" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@FilterData" 
                          Name="Filter Response" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true" 
                             Position="@Syncfusion.Blazor.Charts.LegendPosition.Custom">
        <SmithChartLegendLocation X="550" Y="50">
        </SmithChartLegendLocation>
    </SmithChartLegendSettings>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> AntennaData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.2, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.3, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.4, Reactance = 0.4 }
    };

    public List<SmithChartData> FilterData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.5, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.6, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.7, Reactance = 0.4 }
    };
}
```

### Legend Alignment

Align the legend within its position using the `Alignment` property with enum values (Near, Center, Far):

```razor
@page "/smithchart-legend-alignment"
@using Syncfusion.Blazor.Charts

<h3>Legend Alignment</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@Data1" Name="Near Alignment" 
                          Reactance="Reactance" Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
        <SmithChartSeries DataSource="@Data2" Name="Center Alignment" 
                          Reactance="Reactance" Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
        <SmithChartSeries DataSource="@Data3" Name="Far Alignment" 
                          Reactance="Reactance" Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true" 
                             Position="Syncfusion.Blazor.Charts.LegendPosition.Bottom"
                             Alignment="@SmithChartAlignment.Center">
    </SmithChartLegendSettings>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> Data1 = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.15, Reactance = 0.15 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.25 }
    };

    public List<SmithChartData> Data2 = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.35, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.45, Reactance = 0.3 }
    };

    public List<SmithChartData> Data3 = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.55, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.65, Reactance = 0.4 }
    };
}
```

**Alignment Options:**
- `Near` - Align to the start of the legend area
- `Center` - Center alignment (default)
- `Far` - Align to the end of the legend area

## Legend Appearance

### Legend Shapes

Customize legend item shapes using the `Shape` property with enum values (Circle, Rectangle, Triangle):

```razor
@page "/smithchart-legend-shapes"
@using Syncfusion.Blazor.Charts

<h3>Legend Shape Options</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@CircleData" 
                          Name="Circle Shape" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" Shape="@Syncfusion.Blazor.Charts.Shape.Circle">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@RectangleData" 
                          Name="Rectangle Shape" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" Shape="@Syncfusion.Blazor.Charts.Shape.Rectangle">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@TriangleData" 
                          Name="Triangle Shape" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" Shape="@Syncfusion.Blazor.Charts.Shape.Triangle">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true" 
                             Position="Syncfusion.Blazor.Charts.LegendPosition.Bottom"
                             Shape="@Syncfusion.Blazor.Charts.Shape.Circle">
    </SmithChartLegendSettings>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> CircleData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.15, Reactance = 0.15 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.25 }
    };

    public List<SmithChartData> RectangleData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.35, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.45, Reactance = 0.3 }
    };

    public List<SmithChartData> TriangleData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.55, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.65, Reactance = 0.4 }
    };
}
```

**Legend Shape Options:**
- `Circle` - Circular legend icons
- `Rectangle` - Rectangular legend icons
- `Triangle` - Triangular legend icons

### Legend Sizing

Control legend dimensions using the `Width` and `Height` properties in `SmithChartLegendSettings`:

```razor
@page "/smithchart-legend-sizing"
@using Syncfusion.Blazor.Charts

<h3>Legend Size Configuration</h3>

<SfSmithChart Width="800px" Height="600px">
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@MeasurementData1" 
                          Name="Frequency Band 1 (2.4 GHz)" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@MeasurementData2" 
                          Name="Frequency Band 2 (5.8 GHz)" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true" 
                             Position="Syncfusion.Blazor.Charts.LegendPosition.Right"
                             Width="200px"
                             Height="100px">
    </SmithChartLegendSettings>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> MeasurementData1 = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.2, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.3, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.4, Reactance = 0.4 }
    };

    public List<SmithChartData> MeasurementData2 = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.5, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.6, Reactance = 0.35 },
        new SmithChartData { Resistance = 0.7, Reactance = 0.45 }
    };
}
```

### Padding Configuration

Configure spacing within the legend using `ItemPadding` property (spacing between legend items) and `ShapePadding` property (spacing between shape and text):

```razor
@page "/smithchart-legend-padding"
@using Syncfusion.Blazor.Charts

<h3>Legend Padding Configuration</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@SeriesA" Name="Series A" 
                          Reactance="Reactance" Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
        <SmithChartSeries DataSource="@SeriesB" Name="Series B" 
                          Reactance="Reactance" Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
        <SmithChartSeries DataSource="@SeriesC" Name="Series C" 
                          Reactance="Reactance" Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true" 
                             Position="Syncfusion.Blazor.Charts.LegendPosition.Bottom"
                             ItemPadding="15"
                             ShapePadding="10">
    </SmithChartLegendSettings>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> SeriesA = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.15, Reactance = 0.15 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.25 }
    };

    public List<SmithChartData> SeriesB = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.35, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.45, Reactance = 0.3 }
    };

    public List<SmithChartData> SeriesC = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.55, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.65, Reactance = 0.4 }
    };
}
```

**Padding Properties:**
- `ItemPadding` - Space between legend items
- `ShapePadding` - Space between shape and text

### Legend Borders

Add borders to the legend container using `SmithChartLegendBorder` child component with `Width` property (border thickness) and `Color` property (border color):

```razor
@page "/smithchart-legend-borders"
@using Syncfusion.Blazor.Charts

<h3>Legend with Custom Border</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@ImpedanceData" 
                          Name="Impedance Trace" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@AdmittanceData" 
                          Name="Admittance Trace" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true" 
                             Position="Syncfusion.Blazor.Charts.LegendPosition.Bottom">
        <SmithChartLegendBorder Width="2" Color="#2196F3">
        </SmithChartLegendBorder>
    </SmithChartLegendSettings>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> ImpedanceData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.2, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.3, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.4, Reactance = 0.4 }
    };

    public List<SmithChartData> AdmittanceData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.5, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.6, Reactance = 0.35 },
        new SmithChartData { Resistance = 0.7, Reactance = 0.45 }
    };
}
```

## Legend Item Styling

### Item Size

Configure the size of legend items using `SmithChartLegendItemStyle` child component with `Width` and `Height` properties:

```razor
@page "/smithchart-legend-item-style"
@using Syncfusion.Blazor.Charts

<h3>Legend Item Size Configuration</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@NetworkData" 
                          Name="Matching Network" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true" Position="Syncfusion.Blazor.Charts.LegendPosition.Bottom">
        <SmithChartLegendItemStyle Width="20" Height="20">
        </SmithChartLegendItemStyle>
    </SmithChartLegendSettings>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> NetworkData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.2, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.3, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.4, Reactance = 0.4 }
    };
}
```

### Text Styling

Customize legend text appearance using `SmithChartLegendTextStyle` child component with properties: `FontFamily` (font type), `Size` (font size), `FontWeight` (font weight), and `Color` (text color):

```razor
@page "/smithchart-legend-text-style"
@using Syncfusion.Blazor.Charts

<h3>Legend Text Styling</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@AmplifierInput" 
                          Name="Amplifier Input Port" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@AmplifierOutput" 
                          Name="Amplifier Output Port" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true" Position="Syncfusion.Blazor.Charts.LegendPosition.Bottom">
        <SmithChartLegendTextStyle FontFamily="Segoe UI" 
                                  Size="14px" 
                                  FontWeight="600"
                                  Color="#333333">
        </SmithChartLegendTextStyle>
    </SmithChartLegendSettings>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> AmplifierInput = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.2, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.3, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.4, Reactance = 0.4 }
    };

    public List<SmithChartData> AmplifierOutput = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.5, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.6, Reactance = 0.35 },
        new SmithChartData { Resistance = 0.7, Reactance = 0.45 }
    };
}
```

## Advanced Features

### Row and Column Layout

Organize legend items in rows and columns using `RowCount` property (number of rows) and `ColumnCount` property (number of columns):

```razor
@page "/smithchart-legend-layout"
@using Syncfusion.Blazor.Charts

<h3>Legend Row and Column Layout</h3>

<SfSmithChart Width="900px" Height="600px">
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@Data1" Name="50 MHz" 
                          Reactance="Reactance" Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
        <SmithChartSeries DataSource="@Data2" Name="100 MHz" 
                          Reactance="Reactance" Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
        <SmithChartSeries DataSource="@Data3" Name="200 MHz" 
                          Reactance="Reactance" Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
        <SmithChartSeries DataSource="@Data4" Name="400 MHz" 
                          Reactance="Reactance" Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
        <SmithChartSeries DataSource="@Data5" Name="800 MHz" 
                          Reactance="Reactance" Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
        <SmithChartSeries DataSource="@Data6" Name="1.6 GHz" 
                          Reactance="Reactance" Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true" 
                             Position="Syncfusion.Blazor.Charts.LegendPosition.Bottom"
                             RowCount="2"
                             ColumnCount="3">
    </SmithChartLegendSettings>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> Data1 = new List<SmithChartData>
    { new SmithChartData { Resistance = 0.15, Reactance = 0.15 } };
    
    public List<SmithChartData> Data2 = new List<SmithChartData>
    { new SmithChartData { Resistance = 0.25, Reactance = 0.25 } };
    
    public List<SmithChartData> Data3 = new List<SmithChartData>
    { new SmithChartData { Resistance = 0.35, Reactance = 0.35 } };
    
    public List<SmithChartData> Data4 = new List<SmithChartData>
    { new SmithChartData { Resistance = 0.45, Reactance = 0.45 } };
    
    public List<SmithChartData> Data5 = new List<SmithChartData>
    { new SmithChartData { Resistance = 0.55, Reactance = 0.55 } };
    
    public List<SmithChartData> Data6 = new List<SmithChartData>
    { new SmithChartData { Resistance = 0.65, Reactance = 0.65 } };
}
```

### Legend Title

Add a title to the legend using `SmithChartLegendTitle` child component with `Text` property (title text), `Visible` property (show/hide title), and `TextAlignment` property (title alignment):

```razor
@page "/smithchart-legend-title"
@using Syncfusion.Blazor.Charts

<h3>Legend with Title</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@VNA_Data1" Name="Port 1" 
                          Reactance="Reactance" Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
        <SmithChartSeries DataSource="@VNA_Data2" Name="Port 2" 
                          Reactance="Reactance" Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true" Position="Syncfusion.Blazor.Charts.LegendPosition.Bottom">
        <SmithChartLegendTitle Text="VNA Measurement Ports" 
                              Visible="true"
                              TextAlignment="@SmithChartAlignment.Center">
            <SmithChartLegendTitleTextStyle FontFamily="Segoe UI" 
                                           Size="16px" 
                                           FontWeight="700"
                                           Color="#2C3E50">
            </SmithChartLegendTitleTextStyle>
        </SmithChartLegendTitle>
    </SmithChartLegendSettings>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> VNA_Data1 = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.2, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.3, Reactance = 0.3 }
    };

    public List<SmithChartData> VNA_Data2 = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.5, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.6, Reactance = 0.4 }
    };
}
```

### Toggle Visibility

Enable series visibility toggling through legend clicks using the `ToggleVisibility` property set to `true` in `SmithChartLegendSettings`:

```razor
@page "/smithchart-toggle-visibility"
@using Syncfusion.Blazor.Charts

<h3>Toggle Series Visibility</h3>
<p>Click on legend items to show/hide series</p>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@BeforeMatch" 
                          Name="Before Matching" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" Shape="@Syncfusion.Blazor.Charts.Shape.Circle">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@AfterMatch" 
                          Name="After Matching" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" Shape="@Syncfusion.Blazor.Charts.Shape.Diamond">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true" 
                             Position="Syncfusion.Blazor.Charts.LegendPosition.Bottom"
                             ToggleVisibility="true">
        <SmithChartLegendBorder Width="1" Color="#E0E0E0">
        </SmithChartLegendBorder>
    </SmithChartLegendSettings>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> BeforeMatch = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.8, Reactance = 0.6 },
        new SmithChartData { Resistance = 0.7, Reactance = 0.5 },
        new SmithChartData { Resistance = 0.6, Reactance = 0.4 }
    };

    public List<SmithChartData> AfterMatch = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.3, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.2, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.1, Reactance = 0.1 }
    };
}
```

## Complete Examples

### RF Filter Design Analysis

Comprehensive legend configuration for filter analysis:

```razor
@page "/smithchart-filter-analysis"
@using Syncfusion.Blazor.Charts

<h3>RF Filter Design - Multi-Band Analysis</h3>

<SfSmithChart Width="900px" Height="700px">
    <SmithChartTitle Text="Bandpass Filter S-Parameters" Visible="true">
    </SmithChartTitle>
    
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@S11_Data" 
                          Name="S11 (Input Reflection)" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Circle"
                                   Height="10"
                                   Width="10">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@S22_Data" 
                          Name="S22 (Output Reflection)" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Diamond"
                                   Height="10"
                                   Width="10">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@Matched_Data" 
                          Name="Matched Response" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Triangle"
                                   Height="10"
                                   Width="10">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true" 
                             Position="Syncfusion.Blazor.Charts.LegendPosition.Bottom"
                             Alignment="@SmithChartAlignment.Center"
                             ToggleVisibility="true"
                             ItemPadding="20"
                             ShapePadding="8">
        <SmithChartLegendTitle Text="S-Parameter Measurements" 
                              Visible="true"
                              TextAlignment="@SmithChartAlignment.Center">
            <SmithChartLegendTitleTextStyle FontFamily="Arial" 
                                           Size="15px" 
                                           FontWeight="700"
                                           Color="#1976D2">
            </SmithChartLegendTitleTextStyle>
        </SmithChartLegendTitle>
        <SmithChartLegendBorder Width="2" Color="#1976D2">
        </SmithChartLegendBorder>
        <SmithChartLegendTextStyle FontFamily="Arial" 
                                  Size="13px" 
                                  FontWeight="500"
                                  Color="#424242">
        </SmithChartLegendTextStyle>
        <SmithChartLegendItemStyle Width="18" Height="18">
        </SmithChartLegendItemStyle>
    </SmithChartLegendSettings>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> S11_Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.15, Reactance = 0.15 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.35, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.45, Reactance = 0.3 }
    };

    public List<SmithChartData> S22_Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.2, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.3, Reactance = 0.35 },
        new SmithChartData { Resistance = 0.4, Reactance = 0.4 },
        new SmithChartData { Resistance = 0.5, Reactance = 0.45 }
    };

    public List<SmithChartData> Matched_Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.05, Reactance = 0.05 },
        new SmithChartData { Resistance = 0.08, Reactance = 0.06 },
        new SmithChartData { Resistance = 0.1, Reactance = 0.08 },
        new SmithChartData { Resistance = 0.12, Reactance = 0.1 }
    };
}
```

## Best Practices

### Legend Positioning
1. **Bottom position**: Best for horizontal space, multiple series
2. **Right position**: Good for vertical space, fewer series
3. **Custom position**: Use when standard positions overlap with data
4. **Center alignment**: Default choice for balanced appearance

### Legend Styling
1. **Clear contrast**: Ensure legend text is readable against background
2. **Appropriate sizing**: Keep legend items proportional to chart size
3. **Consistent padding**: Use uniform padding for professional appearance
4. **Border usage**: Add borders when legend background differs from chart

### Interaction
1. **Toggle visibility**: Enable for charts with many series
2. **Legend title**: Add descriptive titles for complex measurements
3. **Row/column layout**: Organize multiple series systematically

### Performance
1. **Limit series count**: Too many legend items can clutter the view
2. **Appropriate sizing**: Don't make legend unnecessarily large
3. **Simple styling**: Complex borders/shadows can impact rendering

## Troubleshooting

### Legend Not Visible
**Problem**: Legend is not appearing on the chart.
**Solution**:
```razor
<SmithChartLegendSettings Visible="true">
</SmithChartLegendSettings>
```

### Legend Overlapping Chart
**Problem**: Legend is covering important chart data.
**Solution**: Change position or use custom positioning:
```razor
<SmithChartLegendSettings Visible="true" 
                         Position="@Syncfusion.Blazor.Charts.LegendPosition.Custom">
    <SmithChartLegendLocation X="600" Y="50">
    </SmithChartLegendLocation>
</SmithChartLegendSettings>
```

### Legend Text Truncated
**Problem**: Legend text is being cut off.
**Solution**: Increase legend width:
```razor
<SmithChartLegendSettings Visible="true" 
                         Position="Syncfusion.Blazor.Charts.LegendPosition.Right"
                         Width="250px">
</SmithChartLegendSettings>
```

### Toggle Not Working
**Problem**: Clicking legend items doesn't toggle series.
**Solution**: Enable toggle visibility:
```razor
<SmithChartLegendSettings Visible="true" 
                         ToggleVisibility="true">
</SmithChartLegendSettings>
```

