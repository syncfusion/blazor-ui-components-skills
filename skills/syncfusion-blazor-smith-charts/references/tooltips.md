# Smith Chart Tooltips

## Table of Contents
- [Overview](#overview)
- [Basic Tooltip Configuration](#basic-tooltip-configuration)
  - [Enabling Tooltips](#enabling-tooltips)
  - [Default Tooltip Behavior](#default-tooltip-behavior)
- [Tooltip Customization](#tooltip-customization)
  - [Color and Opacity](#color-and-opacity)
  - [Tooltip Borders](#tooltip-borders)
- [Tooltip Templates](#tooltip-templates)
  - [Basic Templates](#basic-templates)
  - [Advanced Templates with Formatting](#advanced-templates-with-formatting)
  - [Multi-Series Templates](#multi-series-templates)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)

## Overview

Tooltips provide interactive data visualization by displaying detailed information when users hover over data points in a Smith Chart. In tooltips, the `Visible` property enables/disables tooltip display, the `Fill` property sets the background color, and the `Opacity` property controls transparency. They can show resistance and reactance values, series names, and custom formatted data. This guide covers all aspects of tooltip configuration and customization using `SmithChartSeriesTooltip` component.

## Basic Tooltip Configuration

In the Basic Tooltip Configuration, the `SmithChartSeriesTooltip` component with `Visible` property will be used to enable and configure tooltip display behavior.

### Enabling Tooltips

In the Enabling Tooltips section, the `Visible` property of `SmithChartSeriesTooltip` will be used to enable tooltip display on data points:

```razor
@page "/smithchart-tooltip-basic"
@using Syncfusion.Blazor.Charts

<h3>Smith Chart with Basic Tooltips</h3>
<p>Hover over data points to see values</p>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@TransmissionData" 
                          Name="Transmission Line" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true">
            </SmithChartSeriesMarker>
            <SmithChartSeriesTooltip Visible="true">
            </SmithChartSeriesTooltip>
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
        new SmithChartData { Resistance = 0.15, Reactance = 0.15 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.35, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.45, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.55, Reactance = 0.35 }
    };
}
```

### Default Tooltip Behavior

In the Default Tooltip Behavior, the `Name` property of `SmithChartSeries` along with `Resistance` and `Reactance` properties will be used to automatically display default tooltip content. Default tooltips automatically display:
- **Series Name**: Controlled by `Name` property
- **Resistance**: Accessed from `Resistance` data property
- **Reactance**: Accessed from `Reactance` data property

```razor
@page "/smithchart-tooltip-default"
@using Syncfusion.Blazor.Charts

<h3>Default Tooltip Display</h3>

<SfSmithChart Width="600px" Height="600px">
    <SmithChartTitle Text="Antenna Impedance Measurements" Visible="true">
    </SmithChartTitle>
    
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@Antenna1Data" 
                          Name="Antenna 1 (2.4 GHz)" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Circle"
                                   Height="10"
                                   Width="10">
            </SmithChartSeriesMarker>
            <SmithChartSeriesTooltip Visible="true">
            </SmithChartSeriesTooltip>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@Antenna2Data" 
                          Name="Antenna 2 (5.8 GHz)" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Diamond"
                                   Height="10"
                                   Width="10">
            </SmithChartSeriesMarker>
            <SmithChartSeriesTooltip Visible="true">
            </SmithChartSeriesTooltip>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true" Position="Syncfusion.Blazor.Charts.LegendPosition.Bottom">
    </SmithChartLegendSettings>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> Antenna1Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.2, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.3, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.4, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.5, Reactance = 0.35 }
    };

    public List<SmithChartData> Antenna2Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.45, Reactance = 0.15 },
        new SmithChartData { Resistance = 0.55, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.65, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.75, Reactance = 0.3 }
    };
}
```

## Tooltip Customization

In the Tooltip Customization section, the `Fill` property will be used for background color customization and the `Opacity` property will be used for transparency control.

### Color and Opacity

In the Color and Opacity topic, the `Fill` property sets the tooltip background color and the `Opacity` property controls transparency (0-1 range) to customize tooltip appearance:

```razor
@page "/smithchart-tooltip-color"
@using Syncfusion.Blazor.Charts

<h3>Customized Tooltip Colors</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@InputImpedance" 
                          Name="Input Impedance" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Circle"
                                   Fill="#E91E63">
            </SmithChartSeriesMarker>
            <SmithChartSeriesTooltip Visible="true"
                                    Fill="#FCE4EC"
                                    Opacity="0.95">
            </SmithChartSeriesTooltip>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@OutputImpedance" 
                          Name="Output Impedance" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Diamond"
                                   Fill="#2196F3">
            </SmithChartSeriesMarker>
            <SmithChartSeriesTooltip Visible="true"
                                    Fill="#E3F2FD"
                                    Opacity="0.95">
            </SmithChartSeriesTooltip>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true" Position="Syncfusion.Blazor.Charts.LegendPosition.Bottom">
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
        new SmithChartData { Resistance = 0.5, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.6, Reactance = 0.35 },
        new SmithChartData { Resistance = 0.7, Reactance = 0.45 }
    };
}
```

**Customization Properties:**
| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Fill` | string | Background color of tooltip | White |
| `Opacity` | double | Transparency (0-1) | 0.95 |

### Tooltip Borders

In the Tooltip Borders section, the `SmithChartSeriesTooltipBorder` component with `Width` and `Color` properties will be used to add borders to tooltips for better visual definition:

```razor
@page "/smithchart-tooltip-borders"
@using Syncfusion.Blazor.Charts

<h3>Tooltips with Custom Borders</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@MatchingNetwork" 
                          Name="Matching Network Response" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Circle"
                                   Height="12"
                                   Width="12"
                                   Fill="#FF9800">
            </SmithChartSeriesMarker>
            <SmithChartSeriesTooltip Visible="true"
                                    Fill="#FFF3E0"
                                    Opacity="0.98">
                <SmithChartSeriesTooltipBorder Width="2" Color="#FF9800">
                </SmithChartSeriesTooltipBorder>
            </SmithChartSeriesTooltip>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> MatchingNetwork = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.15, Reactance = 0.15 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.35, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.45, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.55, Reactance = 0.35 }
    };
}
```

## Tooltip Templates

In the Tooltip Templates section, the `Template` property of `SmithChartSeriesTooltip` will be used to create custom tooltip content with specialized formatting.

### Basic Templates

In the Basic Templates topic, the `Template` property with context casting to `SmithChartPoint` will be used to create custom tooltip templates with specialized formatting:

```razor
@page "/smithchart-tooltip-template-basic"
@using Syncfusion.Blazor.Charts

<h3>Custom Tooltip Template</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@FilterData" 
                          Name="Filter Response" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Diamond"
                                   Height="10"
                                   Width="10">
            </SmithChartSeriesMarker>
            <SmithChartSeriesTooltip Visible="true">
                <Template>
                    @{
                        var point = context as SmithChartPoint;
                        if (point != null)
                        {
                            <div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); 
                                        color: white; 
                                        padding: 10px 15px; 
                                        border-radius: 6px;
                                        font-family: 'Segoe UI', sans-serif;
                                        box-shadow: 0 4px 6px rgba(0,0,0,0.2);">
                                <div style="font-weight: bold; font-size: 13px; margin-bottom: 8px; border-bottom: 1px solid rgba(255,255,255,0.3); padding-bottom: 5px;">
                                    Filter Response
                                </div>
                                <div style="font-size: 12px;">
                                    <div style="margin-bottom: 4px;">
                                        <strong>Resistance:</strong> @point.Resistance.ToString("0.000")
                                    </div>
                                    <div>
                                        <strong>Reactance:</strong> @point.Reactance.ToString("0.000")
                                    </div>
                                </div>
                            </div>
                        }
                    }
                </Template>
            </SmithChartSeriesTooltip>
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
        new SmithChartData { Resistance = 0.234, Reactance = 0.189 },
        new SmithChartData { Resistance = 0.345, Reactance = 0.267 },
        new SmithChartData { Resistance = 0.456, Reactance = 0.345 },
        new SmithChartData { Resistance = 0.567, Reactance = 0.423 }
    };
}
```

### Advanced Templates with Formatting

In the Advanced Templates with Formatting topic, the `Template` property will be used with `SmithChartPoint` properties (`Resistance` and `Reactance`) to create detailed templates with calculated impedance values, magnitude, and phase angle:

```razor
@page "/smithchart-tooltip-template-advanced"
@using Syncfusion.Blazor.Charts

<h3>Advanced Tooltip with Calculated Values</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@AmplifierData" 
                          Name="Amplifier S11" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Circle"
                                   Height="12"
                                   Width="12">
            </SmithChartSeriesMarker>
            <SmithChartSeriesTooltip Visible="true">
                <Template>
                    @{
                        var point = context as SmithChartPoint;
                        if (point != null)
                        {
                            // Calculate impedance magnitude
                            double z0 = 50.0; // Reference impedance
                            double realZ = point.Resistance * z0;
                            double imagZ = point.Reactance * z0;
                            double magnitude = Math.Sqrt(realZ * realZ + imagZ * imagZ);
                            double phase = Math.Atan2(imagZ, realZ) * (180.0 / Math.PI);

                            <div style="background: #263238; 
                                        color: #ECEFF1; 
                                        padding: 12px 16px; 
                                        border-radius: 8px;
                                        font-family: 'Courier New', monospace;
                                        min-width: 220px;
                                        box-shadow: 0 6px 12px rgba(0,0,0,0.3);">
                                <div style="font-weight: bold; font-size: 14px; margin-bottom: 10px; color: #80CBC4; border-bottom: 2px solid #80CBC4; padding-bottom: 6px;">
                                    📡 Amplifier S11
                                </div>
                                <table style="width: 100%; font-size: 11px; line-height: 1.6;">
                                    <tr>
                                        <td style="color: #B0BEC5;">Normalized R:</td>
                                        <td style="text-align: right; color: #FFD54F; font-weight: bold;">@point.Resistance.ToString("0.000")</td>
                                    </tr>
                                    <tr>
                                        <td style="color: #B0BEC5;">Normalized X:</td>
                                        <td style="text-align: right; color: #FFD54F; font-weight: bold;">@point.Reactance.ToString("0.000")</td>
                                    </tr>
                                    <tr style="border-top: 1px solid #37474F;">
                                        <td style="color: #B0BEC5; padding-top: 6px;">Impedance:</td>
                                        <td style="text-align: right; color: #4FC3F7; font-weight: bold; padding-top: 6px;">@realZ.ToString("0.0") + j @imagZ.ToString("0.0") Ω</td>
                                    </tr>
                                    <tr>
                                        <td style="color: #B0BEC5;">Magnitude:</td>
                                        <td style="text-align: right; color: #81C784; font-weight: bold;">@magnitude.ToString("0.0") Ω</td>
                                    </tr>
                                    <tr>
                                        <td style="color: #B0BEC5;">Phase:</td>
                                        <td style="text-align: right; color: #F06292; font-weight: bold;">@phase.ToString("0.0")°</td>
                                    </tr>
                                </table>
                            </div>
                        }
                    }
                </Template>
            </SmithChartSeriesTooltip>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> AmplifierData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.45, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.55, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.65, Reactance = 0.35 },
        new SmithChartData { Resistance = 0.75, Reactance = 0.4 }
    };
}
```

### Multi-Series Templates

In the Multi-Series Templates section, the `Template` property of `SmithChartSeriesTooltip` will be used to create templates that adapt to different series with series-specific styling and information:

```razor
@page "/smithchart-tooltip-multi-series"
@using Syncfusion.Blazor.Charts

<h3>Multi-Series Tooltip Templates</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@BeforeMatchingData" 
                          Name="Before Matching" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Circle"
                                   Fill="#FF6B6B">
            </SmithChartSeriesMarker>
            <SmithChartSeriesTooltip Visible="true">
                <Template>
                    @{
                        var point = context as SmithChartPoint;
                        if (point != null)
                        {
                            <div style="background: #FFF5F5; 
                                        border: 2px solid #FF6B6B;
                                        color: #C92A2A; 
                                        padding: 10px 14px; 
                                        border-radius: 6px;
                                        font-family: Arial, sans-serif;">
                                <div style="font-weight: bold; margin-bottom: 6px;">⚠️ Before Matching</div>
                                <div style="font-size: 11px;">
                                    <div>R: @point.Resistance.ToString("0.00")</div>
                                    <div>X: @point.Reactance.ToString("0.00")</div>
                                    <div style="color: #E03131; font-style: italic; margin-top: 4px;">Poor Match</div>
                                </div>
                            </div>
                        }
                    }
                </Template>
            </SmithChartSeriesTooltip>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@AfterMatchingData" 
                          Name="After Matching" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Diamond"
                                   Fill="#51CF66">
            </SmithChartSeriesMarker>
            <SmithChartSeriesTooltip Visible="true">
                <Template>
                    @{
                        var point = context as SmithChartPoint;
                        if (point != null)
                        {
                            <div style="background: #F4FCE3; 
                                        border: 2px solid #51CF66;
                                        color: #2B8A3E; 
                                        padding: 10px 14px; 
                                        border-radius: 6px;
                                        font-family: Arial, sans-serif;">
                                <div style="font-weight: bold; margin-bottom: 6px;">✓ After Matching</div>
                                <div style="font-size: 11px;">
                                    <div>R: @point.Resistance.ToString("0.00")</div>
                                    <div>X: @point.Reactance.ToString("0.00")</div>
                                    <div style="color: #2F9E44; font-style: italic; margin-top: 4px;">Good Match</div>
                                </div>
                            </div>
                        }
                    }
                </Template>
            </SmithChartSeriesTooltip>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true" Position="Syncfusion.Blazor.Charts.LegendPosition.Bottom">
    </SmithChartLegendSettings>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> BeforeMatchingData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.7, Reactance = 0.5 },
        new SmithChartData { Resistance = 0.65, Reactance = 0.45 },
        new SmithChartData { Resistance = 0.6, Reactance = 0.4 }
    };

    public List<SmithChartData> AfterMatchingData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.25, Reactance = 0.15 },
        new SmithChartData { Resistance = 0.15, Reactance = 0.1 },
        new SmithChartData { Resistance = 0.1, Reactance = 0.05 }
    };
}
```

## Complete Examples

### RF Network Analyzer Tooltips

In the RF Network Analyzer Tooltips example, the `SmithChartSeriesTooltip` component with `Template` property and `SmithChartPoint` context will be used to display comprehensive VNA measurements including S-parameters and VSWR calculations:

```razor
@page "/smithchart-vna-tooltips"
@using Syncfusion.Blazor.Charts

<h3>Vector Network Analyzer - S-Parameter Display</h3>

<SfSmithChart Width="800px" Height="700px">
    <SmithChartTitle Text="VNA Measurement: 1-6 GHz Sweep" Visible="true">
    </SmithChartTitle>
    
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@S11_Measurements" 
                          Name="S11 Input Reflection" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Circle"
                                   Height="10"
                                   Width="10"
                                   Fill="#3F51B5">
            </SmithChartSeriesMarker>
            <SmithChartSeriesTooltip Visible="true"
                                    Fill="#E8EAF6"
                                    Opacity="0.98">
                <Template>
                    @{
                        var point = context as SmithChartPoint;
                        if (point != null)
                        {
                            double vswr = (1 + Math.Sqrt(point.Resistance * point.Resistance + point.Reactance * point.Reactance)) /
                                          (1 - Math.Sqrt(point.Resistance * point.Resistance + point.Reactance * point.Reactance));
                            
                            <div style="background: #E8EAF6; 
                                        border: 2px solid #3F51B5;
                                        color: #1A237E; 
                                        padding: 12px 16px; 
                                        border-radius: 8px;
                                        font-family: 'Segoe UI', sans-serif;
                                        min-width: 200px;">
                                <div style="font-weight: bold; font-size: 13px; margin-bottom: 8px; color: #3F51B5;">
                                    S11 Measurement
                                </div>
                                <div style="font-size: 11px; line-height: 1.8;">
                                    <div><strong>R:</strong> @point.Resistance.ToString("0.000")</div>
                                    <div><strong>X:</strong> @point.Reactance.ToString("0.000")</div>
                                    <div style="margin-top: 6px; padding-top: 6px; border-top: 1px solid #C5CAE9;">
                                        <strong>VSWR:</strong> @vswr.ToString("0.00"):1
                                    </div>
                                </div>
                            </div>
                        }
                    }
                </Template>
            </SmithChartSeriesTooltip>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
    
    <SmithChartLegendSettings Visible="true" Position="Syncfusion.Blazor.Charts.LegendPosition.Bottom">
    </SmithChartLegendSettings>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> S11_Measurements = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.15, Reactance = 0.15 },
        new SmithChartData { Resistance = 0.2, Reactance = 0.18 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.3, Reactance = 0.22 },
        new SmithChartData { Resistance = 0.35, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.4, Reactance = 0.28 }
    };
}
```

## Best Practices

### Tooltip Design
In the Tooltip Design section, the `Fill` property for background color and `Opacity` property for transparency should be used effectively. Best practices include:
1. **Keep it concise**: Display only essential information using `Template`
2. **Use clear formatting**: Make values easy to read at a glance with formatted strings
3. **Consistent styling**: Maintain similar design across series using `Fill` color consistently
4. **Adequate contrast**: Ensure text is readable against `Fill` background
5. **Appropriate sizing**: Tooltips should be readable but not overwhelming

### Template Usage
In the Template Usage section, the `Template` property with `SmithChartPoint` context will be used following these practices:
1. **Performance**: Keep `Template` implementations simple for better performance
2. **Calculations**: Perform lightweight calculations only within templates
3. **Formatting**: Use consistent number formatting (e.g., "0.000" for precision) with ToString()
4. **Context handling**: Always check if `SmithChartPoint` context is not null
5. **Styling**: Use inline styles for tooltip `Template` customization

### User Experience
In the User Experience section, the `Visible` property and tooltip properties should enhance interaction:
1. **Quick display**: Tooltips should appear promptly on hover with `Visible` enabled
2. **Clear information**: Show relevant data clearly using `Template` formatting
3. **Visual hierarchy**: Use bold text and spacing for important values in templates
4. **Color coding**: Match tooltip `Fill` colors to series marker colors for clarity

## Troubleshooting

### Tooltips Not Appearing
In the Tooltips Not Appearing troubleshooting, the `Visible` property of `SmithChartSeriesTooltip` must be set to true, and ensure `SmithChartSeriesMarker` with its `Visible` property is also enabled.
**Problem**: Tooltips don't show when hovering over data points.
**Solution**:
```razor
<SmithChartSeriesTooltip Visible="true">
</SmithChartSeriesTooltip>
```
Ensure markers are visible as well:
```razor
<SmithChartSeriesMarker Visible="true">
</SmithChartSeriesMarker>
```

### Template Not Rendering
In the Template Not Rendering troubleshooting, the `Template` property context casting to `SmithChartPoint` must be verified for proper null checks.
**Problem**: Custom template is not displaying.
**Solution**: Verify context casting:
```razor
<Template>
    @{
        var point = context as SmithChartPoint;
        if (point != null)
        {
            // Template content
        }
    }
</Template>
```

### Styling Not Applied
In the Styling Not Applied troubleshooting, inline styles must be used within the `Template` property as external CSS may not be applied.
**Problem**: Custom styles in template are not working.
**Solution**: Use inline styles:
```razor
<div style="background: #FFF; color: #000; padding: 10px;">
    <!-- Content -->
</div>
```

### Values Not Formatted
In the Values Not Formatted troubleshooting, the `Resistance` and `Reactance` properties must be formatted using ToString() with appropriate format specifiers.
**Problem**: Numbers show too many decimal places.
**Solution**: Use ToString with format specifier:
```razor
@point.Resistance.ToString("0.000")
@point.Reactance.ToString("0.00")
```
