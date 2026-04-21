# Markers and Data Labels

## Table of Contents
- [Overview](#overview)
- [Markers](#markers)
  - [Adding Markers](#adding-markers)
  - [Special Point Markers](#special-point-markers)
  - [Marker Customization](#marker-customization)
  - [Marker Visibility Options](#marker-visibility-options)
  - [Marker Shapes and Sizes](#marker-shapes-and-sizes)
- [Data Labels](#data-labels)
  - [Enabling Data Labels](#enabling-data-labels)
  - [Data Label Customization](#data-label-customization)
  - [Label Formatting](#label-formatting)
  - [Edge Label Modes](#edge-label-modes)
  - [Label Templates](#label-templates)
  - [Label Positioning](#label-positioning)
- [Combined Usage](#combined-usage)
- [Best Practices](#best-practices)

## Overview

Markers and data labels enhance sparkline readability by highlighting important data points and displaying their values. Markers are visual indicators (dots, shapes) at data points, while data labels show the actual numeric values.

**Key Features:**
- Selective visibility for specific points (All, Start, End, High, Low, Negative)
- Extensive customization options for appearance
- Flexible formatting and positioning
- Support for custom templates

## Markers

Markers provide visual emphasis on data points in the Sparkline series, making it easier to identify specific values and trends. Use the `SparklineMarkerSettings` component with the `Visible` property to control marker display on specific point types.

### Adding Markers

Enable markers using the `Visible` property in `SparklineMarkerSettings` by specifying which points should display markers. The `Visible` property accepts a list of `VisibleType` values to control marker display on specific data point types.

#### All Points Marker

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="new int[]{ 0, 6, 4, 1, 3, 2, 5 }" 
              Type="SparklineType.Line" 
              Height="200px" 
              Width="350px">
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }">
    </SparklineMarkerSettings>
    <SparklineAxisSettings MinX="-1" MaxX="7" MaxY="7" MinY="-1">
    </SparklineAxisSettings>
</SfSparkline>
```

### Special Point Markers

Display markers only for specific points based on their significance in the dataset. Use the `Visible` property with `VisibleType` enumeration (High, Low, Start, End, Negative) to target specific data point types. Additional color properties like `HighPointColor`, `LowPointColor`, `StartPointColor`, `EndPointColor`, and `NegativePointColor` customize colors for each point type.

#### High and Low Points

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="new int[]{ 0, 6, 4, 1, 3, 2, 5 }" 
              Type="SparklineType.Line" 
              Height="200px" 
              Width="350px" 
              HighPointColor="Blue" 
              LowPointColor="Red">
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.High, VisibleType.Low }">
    </SparklineMarkerSettings>
    <SparklineAxisSettings MinX="-1" MaxX="7" MaxY="7" MinY="-1">
    </SparklineAxisSettings>
</SfSparkline>
```

#### Start and End Points

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@SalesData" 
              Type="SparklineType.Line" 
              Height="150px" 
              Width="300px"
              StartPointColor="#4CAF50"
              EndPointColor="#F44336">
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.Start, VisibleType.End }"
                             Size="6">
    </SparklineMarkerSettings>
</SfSparkline>

@code {
    public double[] SalesData = new double[] { 120, 145, 132, 168, 155, 178, 165 };
}
```

#### Negative Points

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@ProfitData" 
              Type="SparklineType.Column" 
              Height="150px" 
              Width="300px"
              NegativePointColor="#DC3545">
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.Negative }"
                             Size="8">
    </SparklineMarkerSettings>
</SfSparkline>

@code {
    public int[] ProfitData = new int[] { 12, -5, 8, -3, 15, -8, 10, 6, -4, 11 };
}
```

### Marker Customization

Customize marker appearance using various properties for a polished, branded look. Configure `Fill`, `Opacity`, and `Size` properties within `SparklineMarkerSettings`. Use `SparklineMarkerBorder` component with `Color` and `Width` properties to add marker borders.

#### Fill, Opacity, and Size

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="new int[]{ 0, 6, 4, 1, 3, 2, 5 }" 
              Type="SparklineType.Line" 
              Height="200px" 
              Width="450px">
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }" 
                             Fill="yellow" 
                             Opacity="0.4" 
                             Size="8">
        <SparklineMarkerBorder Color="green" Width="1">
        </SparklineMarkerBorder>
    </SparklineMarkerSettings>
    <SparklineAxisSettings MinX="-1" MaxX="7" MaxY="7" MinY="-1">
    </SparklineAxisSettings>
</SfSparkline>
```

#### Complete Customization Example

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@TemperatureData" 
              TValue="TempReading"
              XName="Hour"
              YName="Celsius"
              Type="SparklineType.Line" 
              Height="180px" 
              Width="500px"
              LineWidth="2"
              Fill="#2196F3">
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }" 
                             Fill="#FFFFFF" 
                             Opacity="1" 
                             Size="6">
        <SparklineMarkerBorder Color="#2196F3" Width="2">
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
        new TempReading { Hour = 12, Celsius = 26.5 },
        new TempReading { Hour = 16, Celsius = 28.3 },
        new TempReading { Hour = 20, Celsius = 23.7 }
    };
}
```

### Marker Visibility Options

Available visibility types for controlling marker display via the `Visible` property accepting `VisibleType` enum values:

| Type | Description | Use Case |
|------|-------------|----------|
| `All` | Shows markers on all data points | Dense datasets, general emphasis |
| `Start` | Marker on first point only | Highlight starting value |
| `End` | Marker on last point only | Emphasize final value |
| `High` | Marker on highest value point | Highlight peak performance |
| `Low` | Marker on lowest value point | Identify minimum values |
| `Negative` | Markers on negative values | Financial data, profit/loss |
| `None` | No markers displayed | Clean, minimal design |

#### Multiple Marker Types

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@StockPrices" 
              Type="SparklineType.Area" 
              Height="120px" 
              Width="400px"
              Fill="#E3F2FD"
              LineWidth="2"
              HighPointColor="#4CAF50"
              LowPointColor="#F44336"
              StartPointColor="#2196F3"
              EndPointColor="#FF9800">
    <SparklineMarkerSettings Visible="new List<VisibleType> 
                                      { 
                                          VisibleType.Start, 
                                          VisibleType.End, 
                                          VisibleType.High, 
                                          VisibleType.Low 
                                      }"
                             Size="7">
        <SparklineMarkerBorder Color="#FFFFFF" Width="2">
        </SparklineMarkerBorder>
    </SparklineMarkerSettings>
</SfSparkline>

@code {
    public double[] StockPrices = new double[] 
    { 
        95.5, 98.2, 97.8, 102.3, 100.5, 104.7, 103.2, 101.8, 99.5, 97.2, 98.8, 100.1 
    };
}
```

### Marker Shapes and Sizes

While Syncfusion Blazor Sparkline uses circular markers, you can customize their visual impact through the `Size` property and styling options in `SparklineMarkerSettings`.

#### Size Variations

```razor
@using Syncfusion.Blazor.Charts

<div class="sparkline-container">
    <div class="sparkline-item">
        <h5>Small Markers (Size: 4)</h5>
        <SfSparkline DataSource="@Data" Type="SparklineType.Line" Height="80px" Width="200px">
            <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }" Size="4">
            </SparklineMarkerSettings>
        </SfSparkline>
    </div>
    
    <div class="sparkline-item">
        <h5>Medium Markers (Size: 8)</h5>
        <SfSparkline DataSource="@Data" Type="SparklineType.Line" Height="80px" Width="200px">
            <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }" Size="8">
            </SparklineMarkerSettings>
        </SfSparkline>
    </div>
    
    <div class="sparkline-item">
        <h5>Large Markers (Size: 12)</h5>
        <SfSparkline DataSource="@Data" Type="SparklineType.Line" Height="80px" Width="200px">
            <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }" Size="12">
            </SparklineMarkerSettings>
        </SfSparkline>
    </div>
</div>

@code {
    public int[] Data = new int[] { 3, 6, 4, 8, 5, 9, 7 };
}
```

## Data Labels

Data labels display the actual values of data points, improving readability and providing precise information. Use the `SparklineDataLabelSettings` component with the `Visible` property to enable and control label display on specific point types.

### Enabling Data Labels

Enable data labels using the `Visible` property in `SparklineDataLabelSettings`, accepting `VisibleType` enum values to display labels on all or specific point types.

#### All Points Labels

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="new int[]{ 0, 6, 4, 1, 3, 2, 5 }" 
              Type="SparklineType.Line" 
              Height="200px" 
              Width="350px">
    <SparklineAxisSettings MinX="-1" MaxX="7" MaxY="7" MinY="-1">
    </SparklineAxisSettings>
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }">
    </SparklineDataLabelSettings>
</SfSparkline>
```

#### Selective Labels

Display labels only on specific data point types using the `Visible` property with `VisibleType` enumeration values (High, Low, Start, End, Negative) within `SparklineDataLabelSettings`.

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@QuarterlyRevenue" 
              TValue="RevenueData"
              XName="Quarter"
              YName="Amount"
              ValueType="SparklineValueType.Category"
              Type="SparklineType.Column" 
              Height="180px" 
              Width="400px"
              Fill="#4CAF50">
    <SparklineDataLabelSettings Visible="new List<VisibleType> 
                                         { 
                                             VisibleType.High, 
                                             VisibleType.Low 
                                         }">
    </SparklineDataLabelSettings>
</SfSparkline>

@code {
    public class RevenueData
    {
        public string Quarter { get; set; }
        public double Amount { get; set; }
    }

    public List<RevenueData> QuarterlyRevenue = new List<RevenueData>
    {
        new RevenueData { Quarter = "Q1", Amount = 250000 },
        new RevenueData { Quarter = "Q2", Amount = 320000 },
        new RevenueData { Quarter = "Q3", Amount = 290000 },
        new RevenueData { Quarter = "Q4", Amount = 380000 }
    };
}
```

### Data Label Customization

Customize data labels for improved visual hierarchy and branding. Use properties like `Fill`, `Opacity`, and `EdgeLabelMode` in `SparklineDataLabelSettings`. Style text with `SparklineFont` component (Color, FontStyle, FontWeight, Size, Opacity) and add borders using `SparklineDataLabelBorder` (Color, Width).

#### Style Customization

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="new int[]{ 0, 6, 4, 1, 3, 2, 5 }" 
              Type="SparklineType.Line" 
              Height="200px" 
              Width="350px">
    <SparklineAxisSettings MinX="-1" MaxX="7" MaxY="7" MinY="-1">
    </SparklineAxisSettings>
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }" 
                                Fill="yellow" 
                                Opacity="0.4" 
                                EdgeLabelMode="EdgeLabelMode.Shift">
        <SparklineFont Color="blue" 
                       FontStyle="italic" 
                       FontWeight="bold" 
                       Size="15" 
                       Opacity="0.8">
        </SparklineFont>
        <SparklineDataLabelBorder Color="green" Width="1">
        </SparklineDataLabelBorder>
    </SparklineDataLabelSettings>
</SfSparkline>
```

#### Professional Label Styling

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@PerformanceScores" 
              Type="SparklineType.Column" 
              Height="180px" 
              Width="450px"
              Fill="#2196F3">
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }" 
                                Fill="#FFFFFF" 
                                Opacity="0.9">
        <SparklineFont Color="#333333" 
                       FontWeight="600" 
                       Size="12" 
                       FontFamily="Arial">
        </SparklineFont>
        <SparklineDataLabelBorder Color="#2196F3" Width="1">
        </SparklineDataLabelBorder>
    </SparklineDataLabelSettings>
    <SparklinePadding Top="30">
    </SparklinePadding>
</SfSparkline>

@code {
    public int[] PerformanceScores = new int[] { 85, 92, 78, 95, 88, 91, 87 };
}
```

### Label Formatting

Format data labels to display custom text patterns using the `Format` property in `SparklineDataLabelSettings`. Reference data source properties with placeholder syntax (e.g., `${PropertyName}`) to create dynamic label templates.

#### Custom Format String

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@ClimateData" 
              TValue="WeatherReport" 
              XName="Month" 
              YName="Celsius" 
              ValueType="SparklineValueType.Category" 
              Height="200px" 
              Width="500px">
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }" 
                                Format="${Month} - ${Celsius}°C" 
                                EdgeLabelMode="EdgeLabelMode.Shift">
    </SparklineDataLabelSettings>
    <SparklinePadding Top="25">
    </SparklinePadding>
</SfSparkline>

@code {
    public class WeatherReport
    {
        public string Month { get; set; }
        public double Celsius { get; set; }
    }

    public List<WeatherReport> ClimateData = new List<WeatherReport> 
    {
        new WeatherReport { Month = "Jan", Celsius = 34 },
        new WeatherReport { Month = "Feb", Celsius = 36 },
        new WeatherReport { Month = "Mar", Celsius = 32 },
        new WeatherReport { Month = "Apr", Celsius = 35 },
        new WeatherReport { Month = "May", Celsius = 40 },
        new WeatherReport { Month = "Jun", Celsius = 38 }
    };
}
```

#### Currency and Number Formatting

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@SalesRevenue" 
              TValue="SalesData"
              XName="Region"
              YName="Revenue"
              ValueType="SparklineValueType.Category"
              Type="SparklineType.Column" 
              Height="200px" 
              Width="500px"
              Fill="#FF9800"
              Format="c0">
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }" 
                                Format="${Region}: ${Revenue}">
        <SparklineFont Size="11" FontWeight="600">
        </SparklineFont>
    </SparklineDataLabelSettings>
    <SparklinePadding Top="35">
    </SparklinePadding>
</SfSparkline>

@code {
    public class SalesData
    {
        public string Region { get; set; }
        public double Revenue { get; set; }
    }

    public List<SalesData> SalesRevenue = new List<SalesData>
    {
        new SalesData { Region = "North", Revenue = 45000 },
        new SalesData { Region = "South", Revenue = 52000 },
        new SalesData { Region = "East", Revenue = 38000 },
        new SalesData { Region = "West", Revenue = 48000 }
    };
}
```

### Edge Label Modes

Control how labels behave when they reach the edge of the sparkline container using the `EdgeLabelMode` property in `SparklineDataLabelSettings`. Choose from `None`, `Shift`, or `Hide` modes to manage edge label visibility and positioning.

#### Available Modes

| Mode | Behavior | Use Case |
|------|----------|----------|
| `None` | Labels may overlap or get cut off | Default, minimal processing |
| `Shift` | Labels shift inside to stay visible | Recommended for most cases |
| `Hide` | Edge labels are hidden | Clean look, accept data loss |

#### Shift Mode Example

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@Data" 
              Type="SparklineType.Line" 
              TValue="DataPoint"
              XName="ID" 
              YName="Value"
              Height="150px" 
              Width="400px"
              LineWidth="2">
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }" 
                                EdgeLabelMode="EdgeLabelMode.Shift">
        <SparklineFont Size="12" Color="#333">
        </SparklineFont>
    </SparklineDataLabelSettings>
    <SparklinePadding Top="25" Bottom="10" Left="10" Right="10">
    </SparklinePadding>
</SfSparkline>

@code {
    public class DataPoint 
    {
        public int ID { get; set; }
        public double Value { get; set; }
    }

    private List<DataPoint> Data = new List<DataPoint> 
    { 
        new DataPoint { ID = 0, Value = 95.5 },
        new DataPoint { ID = 1, Value = 98.2 },
        new DataPoint { ID = 2, Value = 97.8 },
        new DataPoint { ID = 3, Value = 102.3 },
        new DataPoint { ID = 4, Value = 100.5 },
        new DataPoint { ID = 5, Value = 104.7 },
        new DataPoint { ID = 6, Value = 103.2 },
        new DataPoint { ID = 7, Value = 101.8 },
        new DataPoint { ID = 8, Value = 99.5 }
    };
}
```

#### Hide Mode Example

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@CompactData" 
              Type="SparklineType.Column" 
              Height="120px" 
              Width="300px">
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }" 
                                EdgeLabelMode="EdgeLabelMode.Hide">
    </SparklineDataLabelSettings>
</SfSparkline>

@code {
    public int[] CompactData = new int[] { 12, 15, 18, 14, 17, 16, 19, 13 };
}
```

### Label Templates

Create custom label content using the `Template` property within `SparklineDataLabelSettings` (Note: Template support varies; verify with current API documentation).

### Label Positioning

Control label position using the `SparklineDataLabelOffset` component with `X` and `Y` properties to adjust label placement relative to data points. Use `SparklineFont` for text styling properties.

#### Offset Configuration

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@MetricsData" 
              Type="SparklineType.Line" 
              Height="180px" 
              Width="450px">
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }">
        <SparklineDataLabelOffset X="0" Y="-15">
        </SparklineDataLabelOffset>
        <SparklineFont Size="11" Color="#666">
        </SparklineFont>
    </SparklineDataLabelSettings>
    <SparklinePadding Top="30">
    </SparklinePadding>
</SfSparkline>

@code {
    public int[] MetricsData = new int[] { 42, 55, 48, 62, 58, 67, 61 };
}
```

## Combined Usage

Use markers and data labels together for maximum information density. Configure both `SparklineMarkerSettings` with `Visible` property and `SparklineDataLabelSettings` with `Visible` property to display complementary visual indicators on the same sparkline.

### Markers with Labels

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@MonthlyMetrics" 
              TValue="MetricData"
              XName="Month"
              YName="Value"
              ValueType="SparklineValueType.Category"
              Type="SparklineType.Line" 
              Height="200px" 
              Width="500px"
              LineWidth="2"
              Fill="#673AB7"
              HighPointColor="#4CAF50"
              LowPointColor="#F44336">
    <SparklineMarkerSettings Visible="new List<VisibleType> 
                                      { 
                                          VisibleType.High, 
                                          VisibleType.Low 
                                      }" 
                             Size="8">
        <SparklineMarkerBorder Color="#FFFFFF" Width="2">
        </SparklineMarkerBorder>
    </SparklineMarkerSettings>
    <SparklineDataLabelSettings Visible="new List<VisibleType> 
                                         { 
                                             VisibleType.High, 
                                             VisibleType.Low 
                                         }" 
                                Format="${Month}: ${Value}"
                                EdgeLabelMode="EdgeLabelMode.Shift">
        <SparklineFont Size="12" FontWeight="600" Color="#333">
        </SparklineFont>
        <SparklineDataLabelOffset Y="-20">
        </SparklineDataLabelOffset>
    </SparklineDataLabelSettings>
    <SparklinePadding Top="35" Bottom="10">
    </SparklinePadding>
</SfSparkline>

@code {
    public class MetricData
    {
        public string Month { get; set; }
        public int Value { get; set; }
    }

    public List<MetricData> MonthlyMetrics = new List<MetricData>
    {
        new MetricData { Month = "Jan", Value = 125 },
        new MetricData { Month = "Feb", Value = 148 },
        new MetricData { Month = "Mar", Value = 112 },
        new MetricData { Month = "Apr", Value = 165 },
        new MetricData { Month = "May", Value = 152 },
        new MetricData { Month = "Jun", Value = 178 }
    };
}
```

### Dashboard Example

```razor
@using Syncfusion.Blazor.Charts

<div class="dashboard-grid">
    <div class="metric-card">
        <h4>Sales Performance</h4>
        <div class="metric-value">$52,450</div>
        <SfSparkline DataSource="@SalesData" 
                      Type="SparklineType.Area" 
                      Height="60px" 
                      Width="250px"
                      Fill="#E8F5E9"
                      LineWidth="2"
                      EndPointColor="#4CAF50">
            <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.End }" 
                                     Size="6">
            </SparklineMarkerSettings>
            <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.End }">
                <SparklineFont Size="11" FontWeight="600" Color="#2E7D32">
                </SparklineFont>
            </SparklineDataLabelSettings>
            <SparklineBorder Color="#4CAF50" Width="2">
            </SparklineBorder>
        </SfSparkline>
    </div>
</div>

@code {
    public double[] SalesData = new double[] 
    { 
        45200, 48300, 46800, 51200, 49500, 52450 
    };
}
```

## Best Practices

### Visibility Selection

**Use All markers when:**
- Dataset is small (< 10 points)
- Each point is significant
- Emphasizing overall trend

**Use selective markers when:**
- Dataset is large
- Only key points matter (high/low/start/end)
- Avoiding visual clutter

**Example: Small Dataset**
```razor
<SfSparkline DataSource="new int[]{ 12, 15, 14, 18 }" Height="80px" Width="200px">
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }">
    </SparklineMarkerSettings>
</SfSparkline>
```

**Example: Large Dataset**
```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@LargeDataset" Height="80px" Width="400px">
    <SparklineMarkerSettings Visible="new List<VisibleType> 
                                      { 
                                          VisibleType.High, 
                                          VisibleType.Low 
                                      }">
    </SparklineMarkerSettings>
</SfSparkline>

@code {
    public double[] LargeDataset = new double[50]; // 50 data points
}
```

### Performance Optimization

**Minimize Label Count:**
- Too many labels reduce readability
- Use selective visibility for large datasets
- Consider showing only High/Low for trends

**Adjust Padding:**
- Add top/bottom padding when using data labels
- Prevents label clipping at chart edges
- Recommended: 25-35px top padding with labels

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@Data" Height="150px" Width="400px" TValue="MyDataPoint"
             XName="ID"
             YName="Value">
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }">
    </SparklineDataLabelSettings>
    <SparklinePadding Top="30" Bottom="10">
    </SparklinePadding>
</SfSparkline>
@code {
    public class MyDataPoint
    {
        public int ID { get; set; }
        public double Value { get; set; }
    }

    public List<MyDataPoint> Data = new()
    {
        new() { ID = 1, Value = 95.5 },
        new() { ID = 2, Value = 98.2 },
        new() { ID = 3, Value = 97.8 },
        new() { ID = 4, Value = 102.3 },
        new() { ID = 5, Value = 100.5 },
        new() { ID = 6, Value = 104.7 },
        new() { ID = 7, Value = 103.2 },
        new() { ID = 8, Value = 101.8 },
        new() { ID = 9, Value = 99.5 }
    };
}
```

### Accessibility

**Color Considerations:**
- Don't rely solely on color for information
- Use markers + labels together for clarity
- Ensure sufficient contrast ratios

**Font Sizing:**
- Minimum 11px for readability
- 12-14px recommended for data labels
- Bold weights improve legibility

### Design Guidelines

**Consistency:**
- Use same marker/label style across related sparklines
- Maintain consistent sizing within dashboards
- Follow brand color guidelines

**Visual Hierarchy:**
- Use larger markers for more important points
- Bold fonts for critical labels
- Color coding for positive/negative values

**Responsive Design:**
- Test marker sizes at different screen sizes
- Adjust label font size for mobile views
- Consider hiding labels on very small sparklines

### Common Patterns

#### Trend Visualization
```razor
<!-- Show start and end to emphasize change -->
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@TrendData" Height="60px" Width="200px">
    <SparklineMarkerSettings Visible="new List<VisibleType> 
                                      { 
                                          VisibleType.Start, 
                                          VisibleType.End 
                                      }">
    </SparklineMarkerSettings>
    <SparklineDataLabelSettings Visible="new List<VisibleType> 
                                         { 
                                             VisibleType.Start, 
                                             VisibleType.End 
                                         }">
    </SparklineDataLabelSettings>
</SfSparkline>
@code {
    public double[] TrendData = { 42, 48, 45, 55, 53 };
}

```

#### Performance Monitoring
```razor
<!-- Highlight peaks and valleys -->
<SfSparkline DataSource="@PerformanceData" Height="80px" Width="300px">
    <SparklineMarkerSettings Visible="new List<VisibleType> 
                                      { 
                                          VisibleType.High, 
                                          VisibleType.Low 
                                      }">
    </SparklineMarkerSettings>
</SfSparkline>
@code {
    public double[] PerformanceData = { 42, 48, 45, 55, 53 };
}

```

#### Financial Data
```razor
<!-- Emphasize negative values -->
<SfSparkline DataSource="@FinancialData" Type="SparklineType.Column" Height="100px" Width="350px">
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.Negative }">
    </SparklineMarkerSettings>
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.Negative }">
        <SparklineFont Color="#DC3545" FontWeight="600">
        </SparklineFont>
    </SparklineDataLabelSettings>
</SfSparkline>
```

### Troubleshooting

**Labels Cut Off:**
- Increase container padding (Top/Bottom)
- Use EdgeLabelMode.Shift
- Reduce font size

**Markers Overlapping:**
- Reduce marker size
- Use selective visibility
- Increase sparkline width

**Poor Contrast:**
- Add marker borders
- Use label background (Fill property)
- Adjust opacity values

**Example: Fix Cut-off Labels**
```razor
<!-- Before: Labels may be cut off -->
<SfSparkline DataSource="@Data" Height="100px" Width="300px">
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }">
    </SparklineDataLabelSettings>
</SfSparkline>

<!-- After: Labels fully visible -->
<SfSparkline DataSource="@Data" Height="100px" Width="300px">
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }"
                                EdgeLabelMode="EdgeLabelMode.Shift">
    </SparklineDataLabelSettings>
    <SparklinePadding Top="30" Bottom="15">
    </SparklinePadding>
</SfSparkline>
```
