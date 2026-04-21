# Smith Chart Dimensions and Titles

## Table of Contents
- [Overview](#overview)
- [Chart Dimensions](#chart-dimensions)
    - [Fixed Dimensions](#fixed-dimensions)
    - [Percentage-Based Dimensions](#percentage-based-dimensions)
    - [Responsive Sizing Strategies](#responsive-sizing-strategies)
- [Title Configuration](#title-configuration)
    - [Basic Title](#basic-title)
    - [Title Alignment](#title-alignment)
    - [Title Styling](#title-styling)
- [Subtitle Configuration](#subtitle-configuration)
    - [Basic Subtitle](#basic-subtitle)
    - [Comprehensive Title and Subtitle Styling](#comprehensive-title-and-subtitle-styling)
- [Margin and Padding](#margin-and-padding)
    - [Chart Margins](#chart-margins)
- [Background Customization](#background-customization)
    - [Chart Background Color](#chart-background-color)
- [Complete Responsive Example](#complete-responsive-example)
    - [Professional RF Measurement Dashboard](#professional-rf-measurement-dashboard)
- [Best Practices](#best-practices)
    - [Dimension Guidelines](#dimension-guidelines)
    - [Title Best Practices](#title-best-practices)
    - [Margin and Spacing](#margin-and-spacing)
- [Troubleshooting](#troubleshooting)
    - [Chart Too Small](#chart-too-small)
    - [Title Cut Off](#title-cut-off)
    - [Responsive Layout Issues](#responsive-layout-issues)
    - [Background Not Applied](#background-not-applied)

## Overview

Proper configuration of Smith Chart dimensions and titles is essential for creating professional and responsive visualizations. This guide covers chart sizing using the `Width` and `Height` properties, title and subtitle configuration using `SmithChartTitle` and `SmithChartSubtitle` components, responsive design strategies, and styling options.

## Chart Dimensions

Configure chart dimensions using the `Width` and `Height` properties of the `SfSmithChart` component.

### Fixed Dimensions

Set explicit width and height for the chart using fixed pixel values with the `Width` and `Height` properties:

```razor
@page "/smithchart-fixed-dimensions"
@using Syncfusion.Blazor.Charts

<h3>Fixed Dimensions Smith Chart</h3>

<SfSmithChart Width="600px" Height="600px">
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@ImpedanceData" 
                          Name="Impedance" 
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

    public List<SmithChartData> ImpedanceData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.15, Reactance = 0.15 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.35, Reactance = 0.35 },
        new SmithChartData { Resistance = 0.45, Reactance = 0.45 }
    };
}
```

### Percentage-Based Dimensions

Use percentage values in the `Width` and `Height` properties for responsive layouts:

```razor
@page "/smithchart-percentage-dimensions"
@using Syncfusion.Blazor.Charts

<h3>Responsive Percentage-Based Dimensions</h3>

<div style="width: 100%; max-width: 1200px; margin: 0 auto;">
    <SfSmithChart Width="100%" Height="600px">
        <SmithChartTitle Text="Responsive Smith Chart" Visible="true">
        </SmithChartTitle>
        
        <SmithChartSeriesCollection>
            <SmithChartSeries DataSource="@AntennaData" 
                              Name="Antenna S11" 
                              Reactance="Reactance" 
                              Resistance="Resistance">
                <SmithChartSeriesMarker Visible="true">
                </SmithChartSeriesMarker>
            </SmithChartSeries>
        </SmithChartSeriesCollection>
        
        <SmithChartLegendSettings Visible="true" Position="Syncfusion.Blazor.Charts.LegendPosition.Bottom">
        </SmithChartLegendSettings>
    </SfSmithChart>
</div>

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
        new SmithChartData { Resistance = 0.4, Reactance = 0.4 },
        new SmithChartData { Resistance = 0.5, Reactance = 0.5 }
    };
}
```

### Responsive Sizing Strategies

Create fully responsive charts that adapt to different screen sizes using percentage values for the `Width` property and binding the `Height` property to dynamic values in the `@code` block:

```razor
@page "/smithchart-responsive"
@using Syncfusion.Blazor.Charts

<h3>Fully Responsive Smith Chart</h3>

<div class="chart-container">
    <SfSmithChart Width="100%" Height="@chartHeight">
        <SmithChartTitle Text="Multi-Device Responsive Chart" 
                        Visible="true"
                        TextAlignment="@SmithChartAlignment.Center">
        </SmithChartTitle>
        
        <SmithChartSeriesCollection>
            <SmithChartSeries DataSource="@FilterResponse" 
                              Name="Filter S-Parameters" 
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
        </SmithChartSeriesCollection>
        
        <SmithChartLegendSettings Visible="true" Position="@legendPosition">
        </SmithChartLegendSettings>
    </SfSmithChart>
</div>

<style>
    .chart-container {
        width: 100%;
        max-width: 1000px;
        margin: 0 auto;
        padding: 20px;
    }

    @@media (max-width: 768px) {
        .chart-container {
            padding: 10px;
        }
    }
</style>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    private string chartHeight = "600px";
    private Syncfusion.Blazor.Charts.LegendPosition legendPosition = Syncfusion.Blazor.Charts.LegendPosition.Bottom;

    protected override void OnInitialized()
    {
        // Adjust based on viewport (simplified example)
        chartHeight = "600px";
        legendPosition = Syncfusion.Blazor.Charts.LegendPosition.Bottom;
    }

    public List<SmithChartData> FilterResponse = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.15, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.35, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.45, Reactance = 0.35 },
        new SmithChartData { Resistance = 0.55, Reactance = 0.4 }
    };
}
```

## Title Configuration

Configure chart titles using the `SmithChartTitle` component with properties like `Text`, `Visible`, and `TextAlignment`.

### Basic Title

Add a title to the Smith Chart using the `SmithChartTitle` component with the `Text` property:

```razor
@page "/smithchart-basic-title"
@using Syncfusion.Blazor.Charts

<SfSmithChart Width="700px" Height="700px">
    <SmithChartTitle Text="Impedance Matching Network Analysis" 
                    Visible="true">
    </SmithChartTitle>
    
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@MatchingData" 
                          Name="Matching Network" 
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

    public List<SmithChartData> MatchingData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.2, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.3, Reactance = 0.35 },
        new SmithChartData { Resistance = 0.4, Reactance = 0.4 },
        new SmithChartData { Resistance = 0.5, Reactance = 0.45 }
    };
}
```

### Title Alignment

Control title text alignment using the `TextAlignment` property with `SmithChartAlignment` values (Near, Center, Far):

```razor
@page "/smithchart-title-alignment"
@using Syncfusion.Blazor.Charts

<h3>Title Alignment Options</h3>

<div style="margin-bottom: 20px;">
    <button @onclick="@(() => SetAlignment("Near"))">Left Align</button>
    <button @onclick="@(() => SetAlignment("Center"))">Center Align</button>
    <button @onclick="@(() => SetAlignment("Far"))">Right Align</button>
</div>

<SfSmithChart Width="800px" Height="600px">
    <SmithChartTitle Text="VNA S-Parameter Measurement" 
                    Visible="true"
                    TextAlignment="@currentAlignment">
    </SmithChartTitle>
    
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@SParamData" 
                          Name="S11" 
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

    private SmithChartAlignment currentAlignment = SmithChartAlignment.Center;

    private void SetAlignment(string alignment)
    {
        currentAlignment = alignment switch
        {
            "Near" => SmithChartAlignment.Near,
            "Center" => SmithChartAlignment.Center,
            "Far" => SmithChartAlignment.Far,
            _ => SmithChartAlignment.Center
        };
    }

    public List<SmithChartData> SParamData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.25, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.35, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.45, Reactance = 0.35 }
    };
}
```

### Title Styling

Customize title appearance using the `SmithChartTitleTextStyle` component with font properties like `FontFamily`, `Size`, `FontWeight`, `Color`, and `FontStyle`:

```razor
@page "/smithchart-title-styling"
@using Syncfusion.Blazor.Charts

<h3>Styled Chart Title</h3>

<SfSmithChart Width="800px" Height="700px">
    <SmithChartTitle Text="RF Power Amplifier Input Impedance" 
                    Visible="true"
                    TextAlignment="@SmithChartAlignment.Center">
        <SmithChartTitleTextStyle FontFamily="'Segoe UI', Arial, sans-serif" 
                                 Size="20px" 
                                 FontWeight="700"
                                 Color="#1976D2"
                                 FontStyle="Normal">
        </SmithChartTitleTextStyle>
    </SmithChartTitle>
    
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@AmplifierData" 
                          Name="Input Port" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Circle"
                                   Height="10"
                                   Width="10">
            </SmithChartSeriesMarker>
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

    public List<SmithChartData> AmplifierData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.3, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.4, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.5, Reactance = 0.35 },
        new SmithChartData { Resistance = 0.6, Reactance = 0.4 }
    };
}
```

## Subtitle Configuration

Configure chart subtitles using the `SmithChartSubtitle` component with properties like `Text`, `Visible`, and `TextAlignment`.

### Basic Subtitle

Add a subtitle below the main title using the `SmithChartSubtitle` component with the `Text` property:

```razor
@page "/smithchart-subtitle"
@using Syncfusion.Blazor.Charts

<SfSmithChart Width="800px" Height="700px">
    <SmithChartTitle Text="Antenna Impedance Analysis" 
                    Visible="true"
                    TextAlignment="@SmithChartAlignment.Center">
        <SmithChartTitleTextStyle Size="18px" 
                                 FontWeight="700"
                                 Color="#2C3E50">
        </SmithChartTitleTextStyle>
        <SmithChartSubtitle Text="2.4 GHz ISM Band | Measured at 25°C" 
                       Visible="true"
                       TextAlignment="@SmithChartAlignment.Center">
            <SmithChartSubtitleTextStyle Size="13px" 
                                        FontWeight="400"
                                        Color="#7F8C8D">
            </SmithChartSubtitleTextStyle>
        </SmithChartSubtitle>
    </SmithChartTitle>
    
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@AntennaImpedance" 
                          Name="Antenna" 
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

    public List<SmithChartData> AntennaImpedance = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.2, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.3, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.4, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.5, Reactance = 0.35 }
    };
}
```

### Comprehensive Title and Subtitle Styling

Complete example with full styling using `SmithChartTitleTextStyle` and `SmithChartSubtitleTextStyle` components to customize font and color properties:

```razor
@page "/smithchart-complete-titles"
@using Syncfusion.Blazor.Charts

<SfSmithChart Width="900px" Height="750px">
    <SmithChartTitle Text="Bandpass Filter Design Verification" 
                    Visible="true"
                    TextAlignment="@SmithChartAlignment.Center">
        <SmithChartTitleTextStyle FontFamily="'Roboto', 'Segoe UI', sans-serif" 
                                 Size="22px" 
                                 FontWeight="700"
                                 Color="#1A237E"
                                 FontStyle="Normal">
        </SmithChartTitleTextStyle>
        <SmithChartSubtitle Text="Three-Pole Chebyshev Response | 900 MHz Center Frequency | 0.1 dB Ripple" 
                       Visible="true"
                       TextAlignment="@SmithChartAlignment.Center">
            <SmithChartSubtitleTextStyle FontFamily="'Roboto', 'Segoe UI', sans-serif" 
                                        Size="14px" 
                                        FontWeight="400"
                                        Color="#5C6BC0"
                                        FontStyle="Italic">
            </SmithChartSubtitleTextStyle>
        </SmithChartSubtitle>
    </SmithChartTitle>
    
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@InputPort" 
                          Name="Input Port (S11)" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Circle"
                                   Height="10"
                                   Width="10"
                                   Fill="#E91E63">
            </SmithChartSeriesMarker>
            <SmithChartSeriesTooltip Visible="true">
            </SmithChartSeriesTooltip>
        </SmithChartSeries>
        
        <SmithChartSeries DataSource="@OutputPort" 
                          Name="Output Port (S22)" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Diamond"
                                   Height="10"
                                   Width="10"
                                   Fill="#2196F3">
            </SmithChartSeriesMarker>
            <SmithChartSeriesTooltip Visible="true">
            </SmithChartSeriesTooltip>
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

    public List<SmithChartData> InputPort = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.15, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.2, Reactance = 0.22 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.24 },
        new SmithChartData { Resistance = 0.3, Reactance = 0.26 },
        new SmithChartData { Resistance = 0.35, Reactance = 0.28 }
    };

    public List<SmithChartData> OutputPort = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.2, Reactance = 0.15 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.18 },
        new SmithChartData { Resistance = 0.3, Reactance = 0.21 },
        new SmithChartData { Resistance = 0.35, Reactance = 0.24 },
        new SmithChartData { Resistance = 0.4, Reactance = 0.27 }
    };
}
```

## Margin and Padding

### Chart Margins

Control spacing around the chart using the `SmithChartMargin` component with properties `Left`, `Right`, `Top`, and `Bottom`:

```razor
@page "/smithchart-margins"
@using Syncfusion.Blazor.Charts

<div style="border: 2px solid #E0E0E0; padding: 20px;">
    <SfSmithChart Width="700px" Height="700px">
        <SmithChartMargin Left="50" Right="50" Top="40" Bottom="40">
        </SmithChartMargin>
        
        <SmithChartTitle Text="Chart with Custom Margins" 
                        Visible="true"
                        TextAlignment="@SmithChartAlignment.Center">
        </SmithChartTitle>
        
        <SmithChartSeriesCollection>
            <SmithChartSeries DataSource="@TestData" 
                              Name="Test Series" 
                              Reactance="Reactance" 
                              Resistance="Resistance">
                <SmithChartSeriesMarker Visible="true">
                </SmithChartSeriesMarker>
            </SmithChartSeries>
        </SmithChartSeriesCollection>
    </SfSmithChart>
</div>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    public List<SmithChartData> TestData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.2, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.4, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.6, Reactance = 0.4 }
    };
}
```

## Background Customization

### Chart Background Color

Customize the chart background using the `Background` property of the `SfSmithChart` component:

```razor
@page "/smithchart-background"
@using Syncfusion.Blazor.Charts

<SfSmithChart Width="800px" Height="700px" Background="#F5F5F5">
    <SmithChartTitle Text="Custom Background Color" 
                    Visible="true"
                    TextAlignment="@SmithChartAlignment.Center">
        <SmithChartTitleTextStyle Size="18px" 
                                 FontWeight="700"
                                 Color="#333333">
        </SmithChartTitleTextStyle>
    </SmithChartTitle>
    
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@SignalData" 
                          Name="RF Signal" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true" 
                                   Shape="@Syncfusion.Blazor.Charts.Shape.Circle"
                                   Height="10"
                                   Width="10"
                                   Fill="#FF6B6B">
            </SmithChartSeriesMarker>
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

    public List<SmithChartData> SignalData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.25, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.35, Reactance = 0.3 },
        new SmithChartData { Resistance = 0.45, Reactance = 0.35 },
        new SmithChartData { Resistance = 0.55, Reactance = 0.4 }
    };
}
```

## Complete Responsive Example

### Professional RF Measurement Dashboard

Complete example combining `Width`, `Height`, `SmithChartMargin`, `SmithChartTitle`, `SmithChartSubtitle`, and `Background` properties:

```razor
@page "/smithchart-professional-dashboard"
@using Syncfusion.Blazor.Charts

<div class="dashboard-container">
    <SfSmithChart Width="100%" Height="@chartHeight" Background="#FAFAFA">
        <SmithChartMargin Left="30" Right="30" Top="20" Bottom="20">
        </SmithChartMargin>
        
        <SmithChartTitle Text="RF Component Characterization Laboratory" 
                        Visible="true"
                        TextAlignment="@SmithChartAlignment.Center">
            <SmithChartTitleTextStyle FontFamily="'Inter', 'Segoe UI', sans-serif" 
                                     Size="20px" 
                                     FontWeight="700"
                                     Color="#1E293B">
            </SmithChartTitleTextStyle>
            <SmithChartSubtitle Text="Vector Network Analyzer Sweep: 100 MHz - 6 GHz | Reference Impedance: 50Ω" 
                           Visible="true"
                           TextAlignment="@SmithChartAlignment.Center">
                <SmithChartSubtitleTextStyle FontFamily="'Inter', 'Segoe UI', sans-serif" 
                                            Size="13px" 
                                            FontWeight="400"
                                            Color="#64748B">
                </SmithChartSubtitleTextStyle>
            </SmithChartSubtitle>
        </SmithChartTitle>
        
        <SmithChartSeriesCollection>
            <SmithChartSeries DataSource="@Measured_S11" 
                              Name="Measured S11" 
                              Reactance="Reactance" 
                              Resistance="Resistance">
                <SmithChartSeriesMarker Visible="true" 
                                       Shape="@Syncfusion.Blazor.Charts.Shape.Circle"
                                       Height="9"
                                       Width="9"
                                       Fill="#10B981">
                    <SmithChartSeriesMarkerBorder Width="2" Color="#059669">
                    </SmithChartSeriesMarkerBorder>
                </SmithChartSeriesMarker>
                <SmithChartSeriesTooltip Visible="true"
                                        Fill="#333333"
                                        Opacity="0.98">
                    <SmithChartSeriesTooltipBorder Width="2" Color="#10B981">
                    </SmithChartSeriesTooltipBorder>
                </SmithChartSeriesTooltip>
            </SmithChartSeries>
            
            <SmithChartSeries DataSource="@Simulated_S11" 
                              Name="Simulated S11" 
                              Reactance="Reactance" 
                              Resistance="Resistance">
                <SmithChartSeriesMarker Visible="true" 
                                       Shape="@Syncfusion.Blazor.Charts.Shape.Diamond"
                                       Height="9"
                                       Width="9"
                                       Fill="#3B82F6">
                    <SmithChartSeriesMarkerBorder Width="2" Color="#2563EB">
                    </SmithChartSeriesMarkerBorder>
                </SmithChartSeriesMarker>
                <SmithChartSeriesTooltip Visible="true"
                                        Fill="#333333"
                                        Opacity="0.98">
                    <SmithChartSeriesTooltipBorder Width="2" Color="#3B82F6">
                    </SmithChartSeriesTooltipBorder>
                </SmithChartSeriesTooltip>
            </SmithChartSeries>
        </SmithChartSeriesCollection>
        
        <SmithChartLegendSettings Visible="true" 
                                 Position="Syncfusion.Blazor.Charts.LegendPosition.Bottom"
                                 Alignment="@SmithChartAlignment.Center"
                                 ItemPadding="20">
            <SmithChartLegendTextStyle FontFamily="'Inter', 'Segoe UI', sans-serif" 
                                      Size="13px" 
                                      FontWeight="500"
                                      Color="#475569">
            </SmithChartLegendTextStyle>
        </SmithChartLegendSettings>
    </SfSmithChart>
</div>

<style>
    .dashboard-container {
        width: 100%;
        max-width: 1200px;
        margin: 0 auto;
        padding: 20px;
        background: white;
        border-radius: 12px;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }

    @@media (max-width: 768px) {
        .dashboard-container {
            padding: 15px;
        }
    }
</style>

@code {
    public class SmithChartData
    {
        public double Resistance { get; set; }
        public double Reactance { get; set; }
    }

    private string chartHeight = "700px";

    public List<SmithChartData> Measured_S11 = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.18, Reactance = 0.22 },
        new SmithChartData { Resistance = 0.23, Reactance = 0.25 },
        new SmithChartData { Resistance = 0.28, Reactance = 0.28 },
        new SmithChartData { Resistance = 0.33, Reactance = 0.31 },
        new SmithChartData { Resistance = 0.38, Reactance = 0.34 },
        new SmithChartData { Resistance = 0.43, Reactance = 0.37 }
    };

    public List<SmithChartData> Simulated_S11 = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 0.2, Reactance = 0.2 },
        new SmithChartData { Resistance = 0.25, Reactance = 0.24 },
        new SmithChartData { Resistance = 0.3, Reactance = 0.28 },
        new SmithChartData { Resistance = 0.35, Reactance = 0.32 },
        new SmithChartData { Resistance = 0.4, Reactance = 0.36 },
        new SmithChartData { Resistance = 0.45, Reactance = 0.4 }
    };
}
```

## Best Practices

### Dimension Guidelines
1. **Square aspect ratio**: Smith Charts work best with equal width and height
2. **Minimum size**: Use at least 400x400px for readability
3. **Responsive design**: Use percentage widths with fixed heights
4. **Container constraints**: Wrap charts in containers with max-width

### Title Best Practices
1. **Clear and concise**: Keep titles descriptive but brief
2. **Hierarchy**: Use larger, bolder fonts for titles
3. **Subtitles**: Provide context with measurement conditions
4. **Alignment**: Center alignment is standard for technical charts
5. **Color**: Use dark colors for good contrast

### Margin and Spacing
1. **Adequate margins**: Provide 30-50px margins for professional appearance
2. **Legend space**: Account for legend when setting dimensions
3. **Title space**: Ensure sufficient top margin for title and subtitle
4. **Consistent spacing**: Maintain uniform margins

## Troubleshooting

### Chart Too Small
**Problem**: Chart appears cramped or unreadable.
**Solution**: Increase dimensions:
```razor
<SfSmithChart Width="800px" Height="800px">
```

### Title Cut Off
**Problem**: Title text is truncated.
**Solution**: Increase top margin:
```razor
<SmithChartMargin Top="60">
</SmithChartMargin>
```

### Responsive Layout Issues
**Problem**: Chart doesn't resize properly.
**Solution**: Use container with percentage width:
```razor
<div style="width: 100%; max-width: 1000px;">
    <SfSmithChart Width="100%" Height="700px">
    </SfSmithChart>
</div>
```

### Background Not Applied
**Problem**: Background color not showing.
**Solution**: Ensure Background property is set:
```razor
<SfSmithChart Background="#F5F5F5">
```

