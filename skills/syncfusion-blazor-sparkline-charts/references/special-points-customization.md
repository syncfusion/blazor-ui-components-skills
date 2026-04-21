# Special Points Customization

## Table of Contents
- [Overview](#overview)
- [Understanding Special Points](#understanding-special-points)
- [Start and End Points](#start-and-end-points)
- [High and Low Points](#high-and-low-points)
- [Negative Points](#negative-points)
- [Tie Points (WinLoss)](#tie-points-winloss)
- [Color Configuration](#color-configuration)
- [Size Configuration](#size-configuration)
- [Use Cases and Patterns](#use-cases-and-patterns)
- [Best Practices](#best-practices)

## Overview

Special points customization allows you to highlight significant data points in your Sparkline charts with distinct colors and visual treatments. This feature helps users quickly identify key metrics like peaks, valleys, starting and ending values, and negative trends.

**Available Special Points:**
- **Start Point** - First data point in the series
- **End Point** - Last data point in the series
- **High Point** - Maximum value in the series
- **Low Point** - Minimum value in the series
- **Negative Point** - Values below zero
- **Tie Point** - Zero values (WinLoss type only)

## Understanding Special Points

Special points are automatically identified by the Sparkline component based on your data. You control their appearance through the `StartPointColor`, `EndPointColor`, `HighPointColor`, `LowPointColor`, `NegativePointColor`, and `TiePointColor` properties on the main `SfSparkline` component.

### Basic Example

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="new int[]{ 0, 6, 4, 1, 3, 2, 5 }" 
              Type="SparklineType.Line" 
              Height="150px" 
              Width="350px"
              StartPointColor="#4CAF50"
              EndPointColor="#2196F3"
              HighPointColor="#FFD700"
              LowPointColor="#F44336"
              NegativePointColor="#DC3545">
</SfSparkline>
```

### Special Points Across Chart Types

Special point customization works with Line, Column, and Area types:

```razor
@using Syncfusion.Blazor.Charts

<div class="sparkline-examples">
    <!-- Line Type -->
    <SfSparkline DataSource="@Data" 
                  Type="SparklineType.Line" 
                  Height="80px" 
                  Width="200px"
                  HighPointColor="green"
                  LowPointColor="red">
    </SfSparkline>
    
    <!-- Column Type -->
    <SfSparkline DataSource="@Data" 
                  Type="SparklineType.Column" 
                  Height="80px" 
                  Width="200px"
                  HighPointColor="green"
                  LowPointColor="red">
    </SfSparkline>
    
    <!-- Area Type -->
    <SfSparkline DataSource="@Data" 
                  Type="SparklineType.Area" 
                  Height="80px" 
                  Width="200px"
                  HighPointColor="green"
                  LowPointColor="red">
    </SfSparkline>
</div>

@code {
    public int[] Data = new int[] { 3, 8, 2, 9, 4, 7, 5 };
}
```

## Start and End Points

Highlight the beginning and ending values to show change over time using the `StartPointColor` and `EndPointColor` properties along with `SparklineMarkerSettings` to control visibility and size.

### Basic Start/End Configuration

Configure the `StartPointColor` and `EndPointColor` properties on `SfSparkline` and use `SparklineMarkerSettings` with the `Visible` property set to `VisibleType.Start` and `VisibleType.End` to display these points.

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@QuarterlySales" 
              TValue="SalesData"
              XName="Quarter"
              YName="Revenue"
              ValueType="SparklineValueType.Category"
              Type="SparklineType.Line" 
              Height="120px" 
              Width="400px"
              LineWidth="2"
              StartPointColor="#4CAF50"
              EndPointColor="#2196F3">
    <SparklineMarkerSettings Visible="new List<VisibleType> 
                                      { 
                                          VisibleType.Start, 
                                          VisibleType.End 
                                      }"
                             Size="8">
    </SparklineMarkerSettings>
</SfSparkline>

@code {
    public class SalesData
    {
        public string Quarter { get; set; }
        public double Revenue { get; set; }
    }

    public List<SalesData> QuarterlySales = new List<SalesData>
    {
        new SalesData { Quarter = "Q1", Revenue = 250000 },
        new SalesData { Quarter = "Q2", Revenue = 320000 },
        new SalesData { Quarter = "Q3", Revenue = 290000 },
        new SalesData { Quarter = "Q4", Revenue = 380000 }
    };
}
```

### Emphasizing Growth

Use contrasting `StartPointColor` and `EndPointColor` properties to highlight positive or negative change, combined with `SparklineDataLabelSettings` and `SparklineMarkerBorder` for visual emphasis:

```razor
@using Syncfusion.Blazor.Charts

<!-- Positive Growth (Green Start, Blue End) -->
<SfSparkline DataSource="@GrowthData" 
              Type="SparklineType.Area" 
              Height="100px" 
              Width="300px"
              Fill="#E3F2FD"
              LineWidth="2"
              StartPointColor="#66BB6A"
              EndPointColor="#1976D2">
    <SparklineMarkerSettings Visible="new List<VisibleType> 
                                      { 
                                          VisibleType.Start, 
                                          VisibleType.End 
                                      }"
                             Size="7">
        <SparklineMarkerBorder Color="#FFFFFF" Width="2">
        </SparklineMarkerBorder>
    </SparklineMarkerSettings>
    <SparklineDataLabelSettings Visible="new List<VisibleType> 
                                         { 
                                             VisibleType.Start, 
                                             VisibleType.End 
                                         }">
        <SparklineFont Size="12" FontWeight="600">
        </SparklineFont>
    </SparklineDataLabelSettings>
</SfSparkline>

@code {
    public int[] GrowthData = new int[] { 120, 145, 138, 162, 155, 178, 185 };
}
```

### Decline Visualization

Visualize declining trends using `StartPointColor` set to a positive color (green) and `EndPointColor` set to a negative color (red) to show the change direction:

```razor
@using Syncfusion.Blazor.Charts

<!-- Negative Trend (Green Start, Red End) -->
<SfSparkline DataSource="@DeclineData" 
              Type="SparklineType.Line" 
              Height="100px" 
              Width="300px"
              LineWidth="2"
              StartPointColor="#4CAF50"
              EndPointColor="#F44336">
    <SparklineMarkerSettings Visible="new List<VisibleType> 
                                      { 
                                          VisibleType.Start, 
                                          VisibleType.End 
                                      }"
                             Size="8">
    </SparklineMarkerSettings>
</SfSparkline>

@code {
    public int[] DeclineData = new int[] { 185, 178, 155, 162, 138, 145, 120 };
}
```

## High and Low Points

Identify peak and valley values in your data series using the `HighPointColor` and `LowPointColor` properties with `SparklineMarkerSettings` to highlight extreme values.

### Basic High/Low Highlighting

Use the `HighPointColor` and `LowPointColor` properties with `SparklineMarkerSettings` to highlight peak and valley points, and `SparklineTooltipSettings` for additional context:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@TemperatureData" 
              TValue="TempReading"
              XName="Hour"
              YName="Celsius"
              Type="SparklineType.Line" 
              Height="150px" 
              Width="450px"
              LineWidth="2"
              Fill="#FF9800"
              HighPointColor="#F44336"
              LowPointColor="#2196F3">
    <SparklineMarkerSettings Visible="new List<VisibleType> 
                                      { 
                                          VisibleType.High, 
                                          VisibleType.Low 
                                      }"
                             Size="9">
        <SparklineMarkerBorder Color="#FFFFFF" Width="2">
        </SparklineMarkerBorder>
    </SparklineMarkerSettings>
    <SparklineTooltipSettings TValue="TempReading" 
                              Visible="true" 
                              Format="${Hour}:00 - ${Celsius}°C">
    </SparklineTooltipSettings>
</SfSparkline>

@code {
    public class TempReading
    {
        public int Hour { get; set; }
        public double Celsius { get; set; }
    }

    public List<TempReading> TemperatureData = new List<TempReading>
    {
        new TempReading { Hour = 0, Celsius = 18.5 },
        new TempReading { Hour = 4, Celsius = 16.2 },
        new TempReading { Hour = 8, Celsius = 20.8 },
        new TempReading { Hour = 12, Celsius = 28.5 },
        new TempReading { Hour = 16, Celsius = 31.3 },
        new TempReading { Hour = 20, Celsius = 23.7 }
    };
}
```

### Stock Price Analysis

Apply `HighPointColor` and `LowPointColor` properties to identify trading peaks and valleys using `SparklineMarkerSettings` and `SparklineDataLabelSettings` for precise value display:

```razor
@using Syncfusion.Blazor.Charts

<div class="stock-card">
    <h4>AAPL - Daily Trading</h4>
    <div class="current-price">$178.45 <span class="change">+2.35%</span></div>
    <SfSparkline DataSource="@StockPrices" 
                  Type="SparklineType.Area" 
                  Height="120px" 
                  Width="400px"
                  Fill="#E8F5E9"
                  LineWidth="2"
                  HighPointColor="#4CAF50"
                  LowPointColor="#F44336">
        <SparklineMarkerSettings Visible="new List<VisibleType> 
                                          { 
                                              VisibleType.High, 
                                              VisibleType.Low 
                                          }"
                                 Size="8">
        </SparklineMarkerSettings>
        <SparklineDataLabelSettings Visible="new List<VisibleType> 
                                             { 
                                                 VisibleType.High, 
                                                 VisibleType.Low 
                                             }"
                                    Format="${y}">
            <SparklineFont Size="11" FontWeight="600">
            </SparklineFont>
        </SparklineDataLabelSettings>
        <SparklineBorder Color="#4CAF50" Width="2">
        </SparklineBorder>
    </SfSparkline>
</div>

@code {
    public double[] StockPrices = new double[] 
    { 
        172.50, 174.20, 173.80, 175.90, 174.50, 176.30, 
        177.10, 175.80, 178.45, 177.20, 178.10 
    };
}
```

### Performance Monitoring

Monitor system performance by applying `HighPointColor` (for problematic peaks) and `LowPointColor` (for optimal values) with `SparklineMarkerSettings` and `SparklineDataLabelSettings`:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@ServerResponseTimes" 
              Type="SparklineType.Column" 
              Height="130px" 
              Width="400px"
              Fill="#90CAF9"
              HighPointColor="#F44336"
              LowPointColor="#4CAF50">
    <SparklineMarkerSettings Visible="new List<VisibleType> 
                                      { 
                                          VisibleType.High, 
                                          VisibleType.Low 
                                      }">
    </SparklineMarkerSettings>
    <SparklineDataLabelSettings Visible="new List<VisibleType> 
                                         { 
                                             VisibleType.High, 
                                             VisibleType.Low 
                                         }">
        <SparklineFont Size="11" FontWeight="600">
        </SparklineFont>
    </SparklineDataLabelSettings>
</SfSparkline>

@code {
    public int[] ServerResponseTimes = new int[] 
    { 
        45, 52, 48, 89, 55, 62, 58, 43, 67, 51 
    };
}
```

## Negative Points

Highlight values below zero to emphasize losses, deficits, or negative trends using the `NegativePointColor` property along with `SparklineAxisSettings` to define the axis value reference point.

### Basic Negative Point Highlighting

Use the `NegativePointColor` property to highlight values below zero with `SparklineAxisSettings` to define the axis value reference point:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@ProfitLossData" 
              Type="SparklineType.Column" 
              Height="150px" 
              Width="400px"
              Fill="#4CAF50"
              NegativePointColor="#F44336">
    <SparklineAxisSettings Value="0" 
                           LineSettings="new SparklineAxisLineSettings 
                                        { 
                                            Visible = true, 
                                            Color = \"#000\", 
                                            Width = 1 
                                        }">
    </SparklineAxisSettings>
</SfSparkline>

@code {
    public int[] ProfitLossData = new int[] 
    { 
        12, -5, 8, -3, 15, -8, 10, 6, -4, 11, -2, 9 
    };
}
```

### Financial Dashboard

Display cash flow with `NegativePointColor` for losses using `ValueType="SparklineValueType.Category"` along with `SparklineMarkerSettings` and `SparklineDataLabelSettings`:

```razor
@using Syncfusion.Blazor.Charts

<div class="financial-metric">
    <h4>Monthly Cash Flow</h4>
    <div class="total">Net: $285,000</div>
    <SfSparkline DataSource="@CashFlowData" 
                  TValue="CashFlow"
                  XName="Month"
                  YName="Amount"
                  ValueType="SparklineValueType.Category"
                  Type="SparklineType.Column" 
                  Height="180px" 
                  Width="500px"
                  Fill="#66BB6A"
                  NegativePointColor="#EF5350">
        <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.Negative }"
                                 Size="8">
        </SparklineMarkerSettings>
        <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }">
            <SparklineFont Size="11" FontWeight="600">
            </SparklineFont>
        </SparklineDataLabelSettings>
        <SparklineAxisSettings Value="0" 
                               LineSettings="new SparklineAxisLineSettings 
                                            { 
                                                Visible = true, 
                                                Color = \"#666\", 
                                                Width = 2 
                                            }">
        </SparklineAxisSettings>
    </SfSparkline>
</div>

@code {
    public class CashFlow
    {
        public string Month { get; set; }
        public int Amount { get; set; }
    }

    public List<CashFlow> CashFlowData = new List<CashFlow>
    {
        new CashFlow { Month = "Jan", Amount = 45 },
        new CashFlow { Month = "Feb", Amount = -12 },
        new CashFlow { Month = "Mar", Amount = 58 },
        new CashFlow { Month = "Apr", Amount = 62 },
        new CashFlow { Month = "May", Amount = -8 },
        new CashFlow { Month = "Jun", Amount = 75 },
        new CashFlow { Month = "Jul", Amount = -15 },
        new CashFlow { Month = "Aug", Amount = 82 },
        new CashFlow { Month = "Sep", Amount = 68 },
        new CashFlow { Month = "Oct", Amount = -5 },
        new CashFlow { Month = "Nov", Amount = 88 },
        new CashFlow { Month = "Dec", Amount = 92 }
    };
}
```

### Temperature Variations

Highlight negative temperature values using `NegativePointColor` with `SparklineAxisSettings` configured with `Value="0"` to display the reference line:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@TemperatureVariations" 
              Type="SparklineType.Line" 
              Height="130px" 
              Width="450px"
              LineWidth="2"
              Fill="#2196F3"
              NegativePointColor="#0D47A1">
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }"
                             Size="5">
    </SparklineMarkerSettings>
    <SparklineAxisSettings Value="0" 
                           LineSettings="new SparklineAxisLineSettings 
                                        { 
                                            Visible = true, 
                                            Color = \"#999\", 
                                            Width = 1 
                                        }">
    </SparklineAxisSettings>
    <SparklineTooltipSettings TValue="int" 
                              Visible="true" 
                              Format="${y}°C">
    </SparklineTooltipSettings>
</SfSparkline>

@code {
    public int[] TemperatureVariations = new int[] 
    { 
        5, 3, -2, 1, 4, 8, 6, -1, 2, 7, 5, -3 
    };
}
```

## Tie Points (WinLoss)

Tie points are specific to the WinLoss chart type and represent neutral outcomes (value = 0) using the `Type="SparklineType.WinLoss"` property and the `TiePointColor` property to customize their appearance.

### Basic WinLoss with Tie Points

Use `Type="SparklineType.WinLoss"` with `Fill` (for wins), `NegativePointColor` (for losses), and `TiePointColor` (for neutral outcomes) properties:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@GameResults" 
              Type="SparklineType.WinLoss"
              Height="80px" 
              Width="400px"
              Fill="#4CAF50"
              NegativePointColor="#F44336"
              TiePointColor="#FFC107">
</SfSparkline>

@code {
    // 1 = Win, -1 = Loss, 0 = Tie
    public int[] GameResults = new int[] 
    { 
        1, 1, -1, 1, 0, 1, -1, -1, 1, 1, 0, 1, -1, 0 
    };
}
```

### Sports Season Tracker

Track sports performance using `Type="SparklineType.WinLoss"` with `Fill`, `NegativePointColor`, and `TiePointColor` properties along with `SparklineAxisSettings` to configure the axis range:

```razor
@using Syncfusion.Blazor.Charts

<div class="season-tracker">
    <h3>Season Performance</h3>
    <div class="record">Record: 8 Wins - 4 Losses - 3 Ties</div>
    <SfSparkline DataSource="@SeasonResults" 
                  Type="SparklineType.WinLoss"
                  Height="100px" 
                  Width="500px"
                  Fill="#0066CC"
                  NegativePointColor="#CC0000"
                  TiePointColor="#999999">
        <SparklineAxisSettings MinY="-1" 
                               MaxY="1" 
                               Value="0">
         <SparklineAxisLineSettings Visible="true"
                                   Color="#333"
                                   Width="1">
        </SparklineAxisLineSettings>

        </SparklineAxisSettings>
        <SparklineTooltipSettings TValue="int" 
                                  Visible="true">
        </SparklineTooltipSettings>
    </SfSparkline>
</div>

@code {
    public int[] SeasonResults = new int[] 
    { 
        1, 1, -1, 1, 0, -1, 1, 1, 1, 0, -1, -1, 1, 1, 0 
    };
}
```

### Trading Outcomes

Visualize trading results using `Type="SparklineType.WinLoss"` with `Fill` (profitable days), `NegativePointColor` (loss days), and `TiePointColor` (break-even days) properties with `SparklineTooltipSettings` for details:

```razor
@using Syncfusion.Blazor.Charts

<div class="trading-summary">
    <h4>Daily Trading Results - Last 20 Days</h4>
    <SfSparkline DataSource="@TradingOutcomes" 
                  Type="SparklineType.WinLoss"
                  Height="90px" 
                  Width="450px"
                  Fill="#28A745"
                  NegativePointColor="#DC3545"
                  TiePointColor="#6C757D">
        <SparklineTooltipSettings TValue="int" 
                                  Visible="true" 
                                  Format="Day ${x}: ${y}">
        </SparklineTooltipSettings>
    </SfSparkline>
    <div class="summary">Profitable Days: 12 | Loss Days: 6 | Break-even: 2</div>
</div>

@code {
    public int[] TradingOutcomes = new int[] 
    { 
        1, 1, -1, 0, 1, 1, 1, -1, 0, -1, 1, 1, -1, 1, 
        1, -1, 1, 1, -1, 1 
    };
}
```

## Color Configuration

Configure special points visual appearance using the dedicated color properties. The `StartPointColor`, `EndPointColor`, `HighPointColor`, `LowPointColor`, `NegativePointColor`, and `TiePointColor` properties control the color rendering for each special point type.

### Color Property Reference

| Property | Applied To | Default | Use Case |
|----------|-----------|---------|----------|
| `StartPointColor` | First point | Component Fill | Highlight starting value |
| `EndPointColor` | Last point | Component Fill | Emphasize final value |
| `HighPointColor` | Maximum value | Component Fill | Show peak performance |
| `LowPointColor` | Minimum value | Component Fill | Identify lowest point |
| `NegativePointColor` | Values < 0 | Component Fill | Emphasize losses/deficits |
| `TiePointColor` | Values = 0 (WinLoss) | Component Fill | Show neutral outcomes |

### Brand Color Scheme

Apply brand-specific colors using `Fill`, `HighPointColor`, and `LowPointColor` properties consistently across sparklines for unified visual design:

```razor
@using Syncfusion.Blazor.Charts

<div class="branded-sparklines">
    <!-- Primary Brand Colors -->
    <SfSparkline DataSource="@Data1" 
                  Type="SparklineType.Line" 
                  Height="80px" 
                  Width="250px"
                  Fill="#1976D2"
                  HighPointColor="#0D47A1"
                  LowPointColor="#42A5F5">
        <SparklineMarkerSettings Visible="new List<VisibleType> 
                                          { 
                                              VisibleType.High, 
                                              VisibleType.Low 
                                          }">
        </SparklineMarkerSettings>
    </SfSparkline>
    
    <!-- Secondary Brand Colors -->
    <SfSparkline DataSource="@Data2" 
                  Type="SparklineType.Column" 
                  Height="80px" 
                  Width="250px"
                  Fill="#388E3C"
                  HighPointColor="#1B5E20"
                  LowPointColor="#66BB6A">
    </SfSparkline>
</div>

@code {
    public int[] Data1 = new int[] { 45, 58, 52, 68, 55, 72, 65 };
    public int[] Data2 = new int[] { 120, 145, 132, 168, 155, 178, 165 };
}
```

### Semantic Color Usage

Use semantic color palettes with `Fill`, `HighPointColor`, `LowPointColor`, and `SparklineBorder` to communicate status (success, warning, critical) effectively:

```razor
@using Syncfusion.Blazor.Charts

<!-- Success/Warning/Danger Palette -->
<div class="status-indicators">
    <!-- Performance: Good -->
    <SfSparkline DataSource="@GoodPerformance" 
                  Type="SparklineType.Area" 
                  Height="70px" 
                  Width="200px"
                  Fill="#D4EDDA"
                  LineWidth="2"
                  HighPointColor="#28A745"
                  LowPointColor="#6C757D">
        <SparklineBorder Color="#28A745" Width="2">
        </SparklineBorder>
    </SfSparkline>
    
    <!-- Performance: Warning -->
    <SfSparkline DataSource="@WarningPerformance" 
                  Type="SparklineType.Area" 
                  Height="70px" 
                  Width="200px"
                  Fill="#FFF3CD"
                  LineWidth="2"
                  HighPointColor="#FFC107"
                  LowPointColor="#6C757D">
        <SparklineBorder Color="#FFC107" Width="2">
        </SparklineBorder>
    </SfSparkline>
    
    <!-- Performance: Critical -->
    <SfSparkline DataSource="@CriticalPerformance" 
                  Type="SparklineType.Area" 
                  Height="70px" 
                  Width="200px"
                  Fill="#F8D7DA"
                  LineWidth="2"
                  HighPointColor="#DC3545"
                  LowPointColor="#6C757D">
        <SparklineBorder Color="#DC3545" Width="2">
        </SparklineBorder>
    </SfSparkline>
</div>

@code {
    public int[] GoodPerformance = new int[] { 85, 88, 92, 90, 94, 96 };
    public int[] WarningPerformance = new int[] { 65, 68, 72, 70, 74, 76 };
    public int[] CriticalPerformance = new int[] { 45, 48, 42, 40, 44, 46 };
}
```

## Size Configuration

While special points themselves don't have individual size properties, you control their visual prominence through the `SparklineMarkerSettings` component with the `Size` property and `SparklineMarkerBorder` for additional styling control.

### Marker Size for Special Points

Control marker prominence using `SparklineMarkerSettings` with the `Size` property and `SparklineMarkerBorder` for enhanced visibility of special points:

```razor
@using Syncfusion.Blazor.Charts

<!-- Large Markers for Special Points -->
<SfSparkline DataSource="@ImportantMetrics" 
              Type="SparklineType.Line" 
              Height="150px" 
              Width="450px"
              LineWidth="2"
              HighPointColor="#FFD700"
              LowPointColor="#F44336">
    <SparklineMarkerSettings Visible="new List<VisibleType> 
                                      { 
                                          VisibleType.High, 
                                          VisibleType.Low 
                                      }"
                             Size="12">
        <SparklineMarkerBorder Color="#FFFFFF" Width="3">
        </SparklineMarkerBorder>
    </SparklineMarkerSettings>
</SfSparkline>

@code {
    public double[] ImportantMetrics = new double[] 
    { 
        125.5, 148.2, 132.8, 165.3, 155.5, 178.7, 168.2 
    };
}
```

### Visual Weight Progression

Adjust visual hierarchy by varying `SparklineMarkerSettings` `Size` property values (small, medium, large) to create emphasis levels:

```razor
@using Syncfusion.Blazor.Charts

<div class="visual-hierarchy">
    <!-- Small: Subtle -->
    <SfSparkline DataSource="@Data" Height="80px" Width="200px"
                  HighPointColor="blue" LowPointColor="red">
        <SparklineMarkerSettings Visible="new List<VisibleType> 
                                          { 
                                              VisibleType.High, 
                                              VisibleType.Low 
                                          }"
                                 Size="5">
        </SparklineMarkerSettings>
    </SfSparkline>
    
    <!-- Medium: Balanced -->
    <SfSparkline DataSource="@Data" Height="80px" Width="200px"
                  HighPointColor="blue" LowPointColor="red">
        <SparklineMarkerSettings Visible="new List<VisibleType> 
                                          { 
                                              VisibleType.High, 
                                              VisibleType.Low 
                                          }"
                                 Size="8">
        </SparklineMarkerSettings>
    </SfSparkline>
    
    <!-- Large: Prominent -->
    <SfSparkline DataSource="@Data" Height="80px" Width="200px"
                  HighPointColor="blue" LowPointColor="red">
        <SparklineMarkerSettings Visible="new List<VisibleType> 
                                          { 
                                              VisibleType.High, 
                                              VisibleType.Low 
                                          }"
                                 Size="12">
        </SparklineMarkerSettings>
    </SfSparkline>
</div>

@code {
    public int[] Data = new int[] { 3, 8, 2, 9, 4, 7, 5 };
}
```

## Use Cases and Patterns

Apply special points customization properties (`StartPointColor`, `EndPointColor`, `HighPointColor`, `LowPointColor`, `NegativePointColor`, `TiePointColor`) in real-world scenarios to create meaningful dashboards and visual indicators. Combine with `SparklineMarkerSettings`, `SparklineDataLabelSettings`, and `SparklineTooltipSettings` for complete context.

### Financial Performance Dashboard

Apply `StartPointColor`, `EndPointColor`, `HighPointColor` with `SparklineMarkerSettings` and `SparklineDataLabelSettings` to create comprehensive financial visualizations:

```razor
@using Syncfusion.Blazor.Charts

<div class="financial-dashboard">
    <div class="metric-card">
        <h5>Revenue Trend</h5>
        <div class="value">$458,230</div>
        <SfSparkline DataSource="@RevenueData" 
                      Type="SparklineType.Area" 
                      Height="70px" 
                      Width="280px"
                      Fill="#E8F5E9"
                      LineWidth="2"
                      StartPointColor="#66BB6A"
                      EndPointColor="#1B5E20"
                      HighPointColor="#4CAF50">
            <SparklineMarkerSettings Visible="new List<VisibleType> 
                                              { 
                                                  VisibleType.Start, 
                                                  VisibleType.End, 
                                                  VisibleType.High 
                                              }"
                                     Size="7">
            </SparklineMarkerSettings>
            <SparklineBorder Color="#4CAF50" Width="2">
            </SparklineBorder>
        </SfSparkline>
    </div>
</div>

@code {
    public double[] RevenueData = new double[] 
    { 
        385000, 412000, 398000, 445000, 428000, 458230 
    };
}
```

### Quality Control Monitoring

Monitor quality metrics using `HighPointColor` (problems) and `LowPointColor` (ideal values) properties with `SparklineMarkerSettings` and `SparklineDataLabelSettings` for precise tracking:

```razor
@using Syncfusion.Blazor.Charts

<div class="quality-monitor">
    <h4>Defect Rate Tracking</h4>
    <SfSparkline DataSource="@DefectRates" 
                  Type="SparklineType.Line" 
                  Height="120px" 
                  Width="450px"
                  LineWidth="2"
                  Fill="#2196F3"
                  HighPointColor="#F44336"
                  LowPointColor="#4CAF50">
        <SparklineMarkerSettings Visible="new List<VisibleType> 
                                          { 
                                              VisibleType.High, 
                                              VisibleType.Low 
                                          }"
                                 Size="8">
        </SparklineMarkerSettings>
        <SparklineDataLabelSettings Visible="new List<VisibleType> 
                                             { 
                                                 VisibleType.High, 
                                                 VisibleType.Low 
                                             }"
                                    Format="${y}%">
        </SparklineDataLabelSettings>
    </SfSparkline>
</div>

@code {
    public double[] DefectRates = new double[] 
    { 
        2.5, 1.8, 2.2, 3.5, 1.9, 1.6, 2.0, 1.4, 2.8, 1.5 
    };
}
```

### Customer Satisfaction Scores

Display satisfaction trends using `HighPointColor` (peaks) and `LowPointColor` (valleys) properties with `SparklineMarkerSettings` and `SparklineDataLabelSettings` for visual clarity:

```razor
@using Syncfusion.Blazor.Charts

<div class="satisfaction-tracker">
    <h4>Weekly Customer Satisfaction</h4>
    <SfSparkline DataSource="@SatisfactionScores" 
                  Type="SparklineType.Column" 
                  Height="140px" 
                  Width="500px"
                  Fill="#42A5F5"
                  HighPointColor="#4CAF50"
                  LowPointColor="#FF9800">
        <SparklineMarkerSettings Visible="new List<VisibleType> 
                                          { 
                                              VisibleType.High, 
                                              VisibleType.Low 
                                          }"
                                 Size="8">
        </SparklineMarkerSettings>
        <SparklineDataLabelSettings Visible="new List<VisibleType> 
                                             { 
                                                 VisibleType.All 
                                             }">
        </SparklineDataLabelSettings>
    </SfSparkline>
</div>

@code {
    public int[] SatisfactionScores = new int[] 
    { 
        88, 92, 85, 94, 87, 91, 89, 95, 86, 93, 90, 96 
    };
}
```

## Best Practices

Follow guidelines for implementing special points customization using properties like `StartPointColor`, `EndPointColor`, `HighPointColor`, `LowPointColor`, `NegativePointColor`, and `TiePointColor` effectively with supporting components like `SparklineMarkerSettings` and `SparklineDataLabelSettings`.

### Color Selection Guidelines

**Do:**
- Use red/negative colors for losses, declines, or problems
- Use green/positive colors for gains, growth, or success
- Use neutral colors (blue, gray) for tie points
- Ensure sufficient contrast with the background
- Follow brand color guidelines when applicable

**Don't:**
- Rely solely on color (use markers too)
- Use similar colors for different point types
- Use overly bright or harsh colors
- Ignore accessibility standards (WCAG)

### Accessibility Considerations

Implement accessible special points using high-contrast `HighPointColor` and `LowPointColor` with `SparklineMarkerSettings` and `SparklineDataLabelSettings` for multiple visual cues:

```razor
@using Syncfusion.Blazor.Charts

<!-- Good: High contrast, multiple visual cues -->
<SfSparkline DataSource="@Data" 
              Type="SparklineType.Line" 
              Height="120px" 
              Width="400px"
              LineWidth="2"
              HighPointColor="#1B5E20"
              LowPointColor="#B71C1C">
    <SparklineMarkerSettings Visible="new List<VisibleType> 
                                      { 
                                          VisibleType.High, 
                                          VisibleType.Low 
                                      }"
                             Size="9">
        <SparklineMarkerBorder Color="#FFFFFF" Width="2">
        </SparklineMarkerBorder>
    </SparklineMarkerSettings>
    <SparklineDataLabelSettings Visible="new List<VisibleType> 
                                         { 
                                             VisibleType.High, 
                                             VisibleType.Low 
                                         }">
    </SparklineDataLabelSettings>
</SfSparkline>
@code { }
```

### Performance Tips

1. **Selective Highlighting**: Don't highlight all special points unless necessary
2. **Appropriate Chart Type**: Column charts work best for negative values
3. **Data Labels**: Combine with data labels for precise values
4. **Tooltips**: Always enable tooltips for detailed information

### Common Patterns

#### Before/After Comparison
Use `HighPointColor` and `LowPointColor` properties to visualize improvements through before/after scenarios:

```razor
<div class="comparison">
    <div>
        <h5>Before Optimization</h5>
        <SfSparkline DataSource="@BeforeData" Height="70px" Width="200px"
                      HighPointColor="red" LowPointColor="orange">
            <SparklineMarkerSettings Visible="new List<VisibleType> 
                                              { 
                                                  VisibleType.High 
                                              }">
            </SparklineMarkerSettings>
        </SfSparkline>
    </div>
    <div>
        <h5>After Optimization</h5>
        <SfSparkline DataSource="@AfterData" Height="70px" Width="200px"
                      HighPointColor="green" LowPointColor="blue">
            <SparklineMarkerSettings Visible="new List<VisibleType> 
                                              { 
                                                  VisibleType.High 
                                              }">
            </SparklineMarkerSettings>
        </SfSparkline>
    </div>
</div>

@code {
    public int[] BeforeData = new int[] { 125, 158, 142, 178, 165, 192, 185 };
    public int[] AfterData = new int[] { 95, 108, 92, 118, 105, 122, 115 };
}
```

#### Trend with Alerts
Highlight critical points using `HighPointColor` property with `SparklineMarkerSettings` and `SparklineDataLabelSettings` to display alert indicators:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@AlertData" 
              Type="SparklineType.Line" 
              Height="100px" 
              Width="350px"
              LineWidth="2"
              HighPointColor="#DC3545"
              Fill="#28A745">
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.High }"
                             Size="10">
    </SparklineMarkerSettings>
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.High }"
                                Format="Alert: ${y}">
        <SparklineFont Color="#DC3545" FontWeight="bold">
        </SparklineFont>
    </SparklineDataLabelSettings>
</SfSparkline>
@code {
    public double[] AlertData =
    {
        62,   // normal
        68,   // normal
        71,   // rising
        85,   // alert (high point)
        73,   // decreasing
        69,   // normal
        65    // stabilized
    };
}
```

**Special points customization is essential for creating informative, actionable Sparkline visualizations that guide user attention to the most important data insights.**
