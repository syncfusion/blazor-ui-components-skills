# Smith Chart Markers and Data Labels

## Table of Contents
- [Overview](#overview)
- [Marker Configuration](#marker-configuration)
  - [Enabling Markers](#enabling-markers)
  - [Marker Shapes](#marker-shapes)
  - [Marker Customization](#marker-customization)
  - [Marker Borders](#marker-borders)
- [Data Labels](#data-labels)
  - [Enabling Data Labels](#enabling-data-labels)
  - [Data Label Customization](#data-label-customization)
  - [Smart Labels](#smart-labels)
  - [Connector Lines](#connector-lines)
  - [Data Label Templates](#data-label-templates)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

## Overview

Markers and data labels are essential visualization features in Smith Charts that help identify data points and display their values. Markers are visual indicators placed at each data point, while data labels show the actual resistance and reactance values. This guide covers comprehensive configuration options for both features.

## Marker Configuration
Configure marker appearance and behavior using SmithChartSeriesMarker properties.

### Enabling Markers
Use the `Visible` property within `SmithChartSeriesMarker` to enable markers:

```razor
@page "/smithchart-markers-basic"
@using Syncfusion.Blazor.Charts

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@TransmissionData" 
                          Name="Transmission Line" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.15, Reactance = 0 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.15 },
        new SmithChartData { Resistance = 0.35, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.45, Reactance = 0.35 },
        new SmithChartData { Resistance = 0.5, Reactance = 0.4 }
    };
}
```

### Marker Shapes
Use the `Shape` property to set marker shapes - supported values include Circle, Rectangle, Triangle, Diamond, Pentagon, and InvertedTriangle:

```razor
@page "/smithchart-marker-shapes"
@using Syncfusion.Blazor.Charts

<h3>Marker Shapes Comparison</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@Series1Data" 
                          Name="Circle Markers" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Circle"
                                   Height="10"
                                   Width="10">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@Series2Data" 
                          Name="Triangle Markers" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Triangle"
                                   Height="10"
                                   Width="10">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@Series3Data" 
                          Name="Diamond Markers" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Diamond"
                                   Height="10"
                                   Width="10">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@Series4Data" 
                          Name="Rectangle Markers" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Rectangle"
                                   Height="10"
                                   Width="10">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true"></SmithChartLegendSettings>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> Series1Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.1, Reactance = 0.1 },
        new SmithChartData { Resistance = 0.2, Reactance = 0.15 },
        new SmithChartData { Resistance = 0.3, Reactance = 0.2 }
    };

    public List<SmithChartData> Series2Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.15, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.35, Reactance = 0.35 }
    };

    public List<SmithChartData> Series3Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.4, Reactance = 0.1 },
        new SmithChartData { Resistance = 0.5, Reactance = 0.15 },
        new SmithChartData { Resistance = 0.6, Reactance = 0.2 }
    };

    public List<SmithChartData> Series4Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.45, Reactance = 0.4 },
        new SmithChartData { Resistance = 0.55, Reactance = 0.45 },
        new SmithChartData { Resistance = 0.65, Reactance = 0.5 }
    };
}
```

**Available Marker Shapes:**
- `Circle` - Default circular marker
- `Rectangle` - Square or rectangular marker
- `Triangle` - Triangular marker pointing upward
- `Diamond` - Diamond-shaped marker
- `Pentagon` - Five-sided marker
- `InvertedTriangle` - Triangle pointing downward

### Marker Customization
Use `Height`, `Width`, `Fill`, and `Opacity` properties to customize marker appearance:

```razor
@page "/smithchart-marker-customization"
@using Syncfusion.Blazor.Charts

<h3>Customized Markers</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@ImpedanceData" 
                          Name="Impedance Match" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Diamond"
                                   Height="15"
                                   Width="15"
                                   Fill="#FF6B6B"
                                   Opacity="0.9">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@LoadData" 
                          Name="Load Line" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Triangle"
                                   Height="12"
                                   Width="12"
                                   Fill="#4ECDC4"
                                   Opacity="0.85">
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true" Position="Syncfusion.Blazor.Charts.LegendPosition.Bottom"></SmithChartLegendSettings>
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
        new SmithChartData { Resistance = 0.3, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.4, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.5, Reactance = 0.35 },
        new SmithChartData { Resistance = 0.6, Reactance = 0.4 }
    };

    public List<SmithChartData> LoadData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.1, Reactance = 0.4 },
        new SmithChartData { Resistance = 0.2, Reactance = 0.45 },
        new SmithChartData { Resistance = 0.3, Reactance = 0.5 },
        new SmithChartData { Resistance = 0.4, Reactance = 0.55 },
        new SmithChartData { Resistance = 0.5, Reactance = 0.6 }
    };
}
```

**Marker Customization Properties:**
| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Height` | double | Height of the marker in pixels | 8 |
| `Width` | double | Width of the marker in pixels | 8 |
| `Fill` | string | Fill color of the marker | Series color |
| `Opacity` | double | Transparency of the marker (0-1) | 1 |

### Marker Borders
Use `SmithChartSeriesMarkerBorder` component with `Width` and `Color` properties to add borders:

```razor
@page "/smithchart-marker-borders"
@using Syncfusion.Blazor.Charts

<h3>Markers with Borders</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@AntennaData" 
                          Name="Antenna Impedance" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Circle"
                                   Height="14"
                                   Width="14"
                                   Fill="#FFD93D"
                                   Opacity="0.8">
                <SmithChartSeriesMarkerBorder Width="3" Color="#6C5B7B">
                </SmithChartSeriesMarkerBorder>
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> AntennaData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.15, Reactance = 0.15 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.35, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.45, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.55, Reactance = 0.35 },
        new SmithChartData { Resistance = 0.65, Reactance = 0.4 }
    };
}
```

## Data Labels
Display data point values using SmithChartSeriesDatalabel properties.

### Enabling Data Labels
Use the `Visible` property within `SmithChartSeriesDatalabel` to enable data labels:

```razor
@page "/smithchart-datalabels-basic"
@using Syncfusion.Blazor.Charts

<h3>Smith Chart with Data Labels</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@TransmissionData" 
                          Name="Transmission" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true">
                <SmithChartSeriesDatalabel Visible="true">
                </SmithChartSeriesDatalabel>
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.2, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.4, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.6, Reactance = 0.4 },
        new SmithChartData { Resistance = 0.8, Reactance = 0.5 }
    };
}
```

### Data Label Customization
Use `Fill` and `Opacity` properties on `SmithChartSeriesDatalabel` and `SmithChartSeriesDataLabelBorder` for styling:

```razor
@page "/smithchart-datalabel-customization"
@using Syncfusion.Blazor.Charts

<h3>Customized Data Labels</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@FilterData" 
                          Name="Filter Response" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Circle"
                                   Height="10"
                                   Width="10">
                <SmithChartSeriesDatalabel Visible="true"
                                          Fill="#E8F8F5"
                                          Opacity="0.95">
                    <SmithChartSeriesDataLabelBorder Width="2" Color="#1ABC9C">
                    </SmithChartSeriesDataLabelBorder>
                </SmithChartSeriesDatalabel>
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> FilterData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.15, Reactance = 0.1 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.35, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.45, Reactance = 0.4 },
        new SmithChartData { Resistance = 0.55, Reactance = 0.5 }
    };
}
```

### Smart Labels
Set `EnableSmartLabels="true"` on SmithChartSeries to automatically prevent label overlapping:

```razor
@page "/smithchart-smart-labels"
@using Syncfusion.Blazor.Charts

<h3>Smart Labels - Automatic Label Positioning</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@DenseData" 
                          Name="Dense Points" 
                          Reactance="Reactance" 
                          Resistance="Resistance"
                          EnableSmartLabels="true">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Circle">
                <SmithChartSeriesDatalabel Visible="true"
                                          Fill="#FFF3E0"
                                          Opacity="0.9">
                    <SmithChartSeriesDataLabelBorder Width="1" Color="#FF9800">
                    </SmithChartSeriesDataLabelBorder>
                </SmithChartSeriesDatalabel>
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    // Dense data points that would normally cause label overlap
    public List<SmithChartData> DenseData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.2, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.22, Reactance = 0.21 },
        new SmithChartData { Resistance = 0.24, Reactance = 0.22 },
        new SmithChartData { Resistance = 0.26, Reactance = 0.23 },
        new SmithChartData { Resistance = 0.28, Reactance = 0.24 },
        new SmithChartData { Resistance = 0.3, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.32, Reactance = 0.26 },
        new SmithChartData { Resistance = 0.34, Reactance = 0.27 },
        new SmithChartData { Resistance = 0.36, Reactance = 0.28 },
        new SmithChartData { Resistance = 0.38, Reactance = 0.29 }
    };
}
```

### Connector Lines
Use `SmithChartDataLabelConnectorLine` component with `Color` and `Width` properties to display connector lines:

```razor
@page "/smithchart-connector-lines"
@using Syncfusion.Blazor.Charts

<h3>Data Labels with Connector Lines</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@MeasurementData" 
                          Name="S11 Measurements" 
                          Reactance="Reactance" 
                          Resistance="Resistance"
                          EnableSmartLabels="true">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Diamond"
                                   Height="8"
                                   Width="8">
                <SmithChartSeriesDatalabel Visible="true"
                                          Fill="#E8EAF6"
                                          Opacity="0.95">
                    <SmithChartSeriesDataLabelBorder Width="1" Color="#3F51B5">
                    </SmithChartSeriesDataLabelBorder>
                    <SmithChartDataLabelConnectorLine Color="#3F51B5" Width="1.5">
                    </SmithChartDataLabelConnectorLine>
                </SmithChartSeriesDatalabel>
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> MeasurementData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.15, Reactance = 0.15 },
        new SmithChartData { Resistance = 0.17, Reactance = 0.16 },
        new SmithChartData { Resistance = 0.19, Reactance = 0.17 },
        new SmithChartData { Resistance = 0.21, Reactance = 0.18 },
        new SmithChartData { Resistance = 0.23, Reactance = 0.19 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.27, Reactance = 0.21 },
        new SmithChartData { Resistance = 0.29, Reactance = 0.22 }
    };
}
```

## Complete Examples

### RF Matching Network Analysis
Demonstrates usage of all marker and data label properties together for matching network visualization:

```razor
@page "/smithchart-matching-network"
@using Syncfusion.Blazor.Charts

<h3>RF Matching Network Analysis</h3>
<p>Impedance transformation from 50Ω to load impedance</p>

<SfSmithChart Width="600px" Height="600px">
    <SmithChartTitle Text="L-Section Matching Network" Visible="true">
    </SmithChartTitle>
    
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@LoadPath" 
                          Name="Load Path" 
                          Reactance="Reactance" 
                          Resistance="Resistance"
                          EnableSmartLabels="true">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Circle"
                                   Height="12"
                                   Width="12"
                                   Fill="#FF6B6B">
                <SmithChartSeriesMarkerBorder Width="2" Color="#C92A2A">
                </SmithChartSeriesMarkerBorder>
                <SmithChartSeriesDatalabel Visible="true"
                                          Fill="#FFF5F5"
                                          Opacity="0.95">
                    <SmithChartSeriesDataLabelBorder Width="1" Color="#FF6B6B">
                    </SmithChartSeriesDataLabelBorder>
                    <SmithChartDataLabelConnectorLine Color="#C92A2A" Width="1.5">
                    </SmithChartDataLabelConnectorLine>
                </SmithChartSeriesDatalabel>
            </SmithChartSeriesMarker>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@MatchedPath" 
                          Name="Matched Path" 
                          Reactance="Reactance" 
                          Resistance="Resistance"
                          EnableSmartLabels="true">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Diamond"
                                   Height="12"
                                   Width="12"
                                   Fill="#51CF66">
                <SmithChartSeriesMarkerBorder Width="2" Color="#2B8A3E">
                </SmithChartSeriesMarkerBorder>
                <SmithChartSeriesDatalabel Visible="true"
                                          Fill="#F4FCE3"
                                          Opacity="0.95">
                    <SmithChartSeriesDataLabelBorder Width="1" Color="#51CF66">
                    </SmithChartSeriesDataLabelBorder>
                    <SmithChartDataLabelConnectorLine Color="#2B8A3E" Width="1.5">
                    </SmithChartDataLabelConnectorLine>
                </SmithChartSeriesDatalabel>
            </SmithChartSeriesMarker>
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

    // Initial load impedance path
    public List<SmithChartData> LoadPath = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 1.0, Reactance = 0.0 },
        new SmithChartData { Resistance = 0.9, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.7, Reactance = 0.5 },
        new SmithChartData { Resistance = 0.5, Reactance = 0.6 }
    };

    // Path after matching network
    public List<SmithChartData> MatchedPath = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.5, Reactance = 0.6 },
        new SmithChartData { Resistance = 0.35, Reactance = 0.4 },
        new SmithChartData { Resistance = 0.2, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.1, Reactance = 0.05 }
    };
}
```

## Best Practices
Guidelines for effective marker and data label implementation.

### Marker Usage
1. **Choose appropriate shapes**: Use different shapes to distinguish series
2. **Size appropriately**: Markers should be visible but not overwhelming (8-14px typically)
3. **Use borders**: Add borders for better visibility on complex backgrounds
4. **Consistent styling**: Maintain consistent marker sizes within similar data types

### Data Label Best Practices
1. **Enable smart labels**: Always use `EnableSmartLabels="true"` for dense data
2. **Contrast**: Ensure labels have good contrast with the background
3. **Connector lines**: Use visible connector lines when labels are repositioned
4. **Template wisely**: Use templates for complex information, but keep them concise
5. **Opacity**: Use slight transparency (0.9-0.95) for better integration

### Performance Considerations
1. **Limit data points**: Too many markers and labels can impact performance
2. **Smart labels**: Enable only when needed (dense data scenarios)
3. **Template complexity**: Keep templates simple to maintain rendering performance

## Troubleshooting
Common issues and solutions related to markers and data labels.

### Markers Not Visible
**Problem**: Markers are not appearing on the chart.
**Solution**: 
- Ensure `Visible="true"` is set in `SmithChartSeriesMarker`
- Check that data is properly bound
- Verify marker size is appropriate for chart dimensions

### Overlapping Labels
**Problem**: Data labels are overlapping and unreadable.
**Solution**:
```razor
<SfSmithChart EnableSmartLabels="true">
    <!-- Series configuration -->
</SfSmithChart>
```

### Connector Lines Not Showing
**Problem**: Smart label connector lines are not visible.
**Solution**:
- Ensure `EnableSmartLabels="true"` on the SmithChart
- Configure connector line properties:
```razor
<SmithChartDataLabelConnectorLine Color="#000000" Width="1.5">
</SmithChartDataLabelConnectorLine>
```

### Template Context Not Rendering
**Problem**: Custom template is not displaying data.
**Solution**:
```razor
<Template>
    @{
        var point = context as SmithChartPoint;
        if (point != null)
        {
            // Access point.Resistance and point.Reactance
        }
    }
</Template>
```
