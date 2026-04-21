# Range Bands

## Table of Contents
- [Overview](#overview)
- [Basic Range Band Configuration](#basic-range-band-configuration)
    - [Single Range Band](#single-range-band)
    - [Multiple Range Bands](#multiple-range-bands)
- [Range Band Properties](#range-band-properties)
    - [Color](#color)
    - [Opacity](#opacity)
    - [Start and End Range](#start-and-end-range)
- [Use Cases](#use-cases)
    - [Quality Control Monitoring](#quality-control-monitoring)
    - [Performance Target Zones](#performance-target-zones)
    - [Server Response Time Thresholds](#server-response-time-thresholds)
    - [Budget Tracking](#budget-tracking)
    - [Temperature Monitoring](#temperature-monitoring)
- [Best Practices](#best-practices)
    - [Color Selection](#color-selection)
    - [Opacity Guidelines](#opacity-guidelines)
    - [Range Definition](#range-definition)
    - [Accessibility](#accessibility)
    - [Performance Considerations](#performance-considerations)
    - [Documentation](#documentation)
- [Common Patterns](#common-patterns)
    - [Two-Zone Pattern (Target Line)](#two-zone-pattern-target-line)
    - [Three-Zone Pattern (Traffic Light)](#three-zone-pattern-traffic-light)
    - [Single Highlight Band](#single-highlight-band)

## Overview

Range bands are visual indicators that highlight specific value ranges on a Sparkline's Y-axis using the `SparklineRangeBand` component. They improve readability by showing target zones, thresholds, acceptable ranges, or quality standards. Range bands appear as colored, semi-transparent horizontal bands spanning the width of the chart.

**Key Features:**
- Define multiple range bands on a single sparkline using `SparklineRangeBandSettings`
- Customize `Color` and `Opacity` properties for each band
- Specify start and end values using `StartRange` and `EndRange` properties
- Useful for threshold monitoring and quality control

## Basic Range Band Configuration

Create range bands using the `SparklineRangeBandSettings` component with `SparklineRangeBand` child elements. Use properties like `Height`, `Width`, `LineWidth`, and `Fill` on `SfSparkline` to configure the chart container, and `StartRange`, `EndRange`, `Color`, and `Opacity` on `SparklineRangeBand` to define each band.

### Single Range Band

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="new int[]{ 0, 6, 4, 1, 3, 2, 5 }" 
              Height="150px" 
              Width="300px" 
              LineWidth="2" 
              Fill="#0D3C9B">
    <SparklineAxisSettings MinX="-1" MaxX="7" MaxY="7" MinY="-1">
    </SparklineAxisSettings>
    <SparklineRangeBandSettings>
        <SparklineRangeBand StartRange="1" 
                            EndRange="3" 
                            Color="#BFD4FC" 
                            Opacity="0.4">
        </SparklineRangeBand>
    </SparklineRangeBandSettings>
</SfSparkline>
```

### Multiple Range Bands

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="new int[]{ 0, 6, 4, 1, 3, 2, 5 }" 
              Height="150px" 
              Width="350px" 
              LineWidth="2" 
              Fill="#0D3C9B">
    <SparklineAxisSettings MinX="-1" MaxX="7" MaxY="7" MinY="-1">
    </SparklineAxisSettings>
    <SparklineRangeBandSettings>
        <SparklineRangeBand StartRange="1" 
                            EndRange="2" 
                            Color="#BFD4FC" 
                            Opacity="0.4">
        </SparklineRangeBand>
        <SparklineRangeBand StartRange="4" 
                            EndRange="5" 
                            Color="red" 
                            Opacity="0.4">
        </SparklineRangeBand>
    </SparklineRangeBandSettings>
</SfSparkline>
```

## Range Band Properties

### Color

The `Color` property specifies the fill color of the range band. It accepts standard CSS color values and is applied within the `SparklineRangeBand` component.

```razor
@using Syncfusion.Blazor.Charts

<SparklineRangeBand StartRange="50" 
                    EndRange="70" 
                    Color="#4CAF50" 
                    Opacity="0.3">
</SparklineRangeBand>
```

**Color Property Options:**
- Hex colors: `Color="#4CAF50"`, `Color="#FF5722"`
- RGB: `Color="rgb(76, 175, 80)"`
- RGBA: `Color="rgba(76, 175, 80, 0.5)"`
- Named colors: `Color="red"`, `Color="blue"`, `Color="green"`

### Opacity

The `Opacity` property controls the transparency of the range band. Values range from 0 (fully transparent) to 1 (fully opaque), applied to the `SparklineRangeBand` component.

```razor
<!-- Low opacity - subtle -->
<SparklineRangeBand StartRange="0" 
                    EndRange="25" 
                    Color="#F44336" 
                    Opacity="0.2">
</SparklineRangeBand>

<!-- Medium opacity - balanced -->
<SparklineRangeBand StartRange="25" 
                    EndRange="75" 
                    Color="#FFC107" 
                    Opacity="0.4">
</SparklineRangeBand>

<!-- High opacity - prominent -->
<SparklineRangeBand StartRange="75" 
                    EndRange="100" 
                    Color="#4CAF50" 
                    Opacity="0.6">
</SparklineRangeBand>
```

**Recommended Opacity Property Values:**
- `Opacity="0.2"` to `Opacity="0.3"`: Subtle background zones
- `Opacity="0.4"` to `Opacity="0.5"`: Standard visibility
- `Opacity="0.6"` to `Opacity="0.7"`: Prominent emphasis

### Start and End Range

The `StartRange` and `EndRange` properties define the Y-axis values where the range band begins and ends on the `SparklineRangeBand` component. Use `MinY` and `MaxY` properties in `SparklineAxisSettings` to define the overall chart Y-axis range.

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@TemperatureData" 
              TValue="TempReading"
              XName="Hour"
              YName="Celsius"
              Height="180px" 
              Width="450px">
    <SparklineAxisSettings MinY="0" MaxY="40">
    </SparklineAxisSettings>
    <SparklineRangeBandSettings>
        <!-- Cold zone -->
        <SparklineRangeBand StartRange="0" 
                            EndRange="15" 
                            Color="#2196F3" 
                            Opacity="0.3">
        </SparklineRangeBand>
        <!-- Comfortable zone -->
        <SparklineRangeBand StartRange="15" 
                            EndRange="28" 
                            Color="#4CAF50" 
                            Opacity="0.3">
        </SparklineRangeBand>
        <!-- Hot zone -->
        <SparklineRangeBand StartRange="28" 
                            EndRange="40" 
                            Color="#F44336" 
                            Opacity="0.3">
        </SparklineRangeBand>
    </SparklineRangeBandSettings>
</SfSparkline>

@code {
    public class TempReading
    {
        public int Hour { get; set; }
        public double Celsius { get; set; }
    }

    public List<TempReading> TemperatureData = new List<TempReading>
    {
        new TempReading { Hour = 0, Celsius = 18 },
        new TempReading { Hour = 4, Celsius = 16 },
        new TempReading { Hour = 8, Celsius = 22 },
        new TempReading { Hour = 12, Celsius = 30 },
        new TempReading { Hour = 16, Celsius = 32 },
        new TempReading { Hour = 20, Celsius = 25 }
    };
}
```

## Use Cases

### Quality Control Monitoring

Visualize acceptable, warning, and critical ranges for quality metrics using multiple `SparklineRangeBand` elements within `SparklineRangeBandSettings`. Configure `Type` property to `SparklineType.Line`, set `Fill` color, and define `StartRange` and `EndRange` for each quality level.

```razor
@using Syncfusion.Blazor.Charts

<div class="quality-dashboard">
    <h4>Product Quality Score</h4>
    <div class="current-value">Score: 87/100</div>
    <SfSparkline DataSource="@QualityScores" 
                  Type="SparklineType.Line" 
                  Height="150px" 
                  Width="450px"
                  LineWidth="2"
                  Fill="#2196F3">
        <SparklineAxisSettings MinY="0" MaxY="100">
        </SparklineAxisSettings>
        <SparklineRangeBandSettings>
            <!-- Critical: Below 60 -->
            <SparklineRangeBand StartRange="0" 
                                EndRange="60" 
                                Color="#F44336" 
                                Opacity="0.3">
            </SparklineRangeBand>
            <!-- Warning: 60-80 -->
            <SparklineRangeBand StartRange="60" 
                                EndRange="80" 
                                Color="#FFC107" 
                                Opacity="0.3">
            </SparklineRangeBand>
            <!-- Good: 80-100 -->
            <SparklineRangeBand StartRange="80" 
                                EndRange="100" 
                                Color="#4CAF50" 
                                Opacity="0.3">
            </SparklineRangeBand>
        </SparklineRangeBandSettings>
        <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }"
                                 Size="5">
        </SparklineMarkerSettings>
        <SparklineTooltipSettings TValue="int" 
                                  Visible="true" 
                                  Format="Score: ${y}">
        </SparklineTooltipSettings>
    </SfSparkline>
</div>

@code {
    public int[] QualityScores = new int[] 
    { 
        72, 78, 85, 82, 88, 91, 87, 89, 86, 90 
    };
}
```

### Performance Target Zones

Show target performance ranges with color-coded bands using `ValueType="SparklineValueType.Category"` and `Type="SparklineType.Column"` properties on `SfSparkline`. Use `XName`, `YName`, and `TValue` properties to bind data, and configure multiple `SparklineRangeBand` elements with `StartRange` and `EndRange` for above/below target visualization.

```razor
@using Syncfusion.Blazor.Charts

<div class="performance-monitor">
    <h4>Sales Performance vs Target</h4>
    <SfSparkline DataSource="@SalesPerformance" 
                  TValue="SalesData"
                  XName="Month"
                  YName="Percentage"
                  ValueType="SparklineValueType.Category"
                  Type="SparklineType.Column" 
                  Height="180px" 
                  Width="500px"
                  Fill="#42A5F5">
        <SparklineAxisSettings MinY="0" MaxY="150">
        </SparklineAxisSettings>
        <SparklineRangeBandSettings>
            <!-- Below target -->
            <SparklineRangeBand StartRange="0" 
                                EndRange="100" 
                                Color="#FFCDD2" 
                                Opacity="0.4">
            </SparklineRangeBand>
            <!-- Above target -->
            <SparklineRangeBand StartRange="100" 
                                EndRange="150" 
                                Color="#C8E6C9" 
                                Opacity="0.4">
            </SparklineRangeBand>
        </SparklineRangeBandSettings>
        <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }">
            <SparklineFont Size="11" FontWeight="600">
            </SparklineFont>
        </SparklineDataLabelSettings>
    </SfSparkline>
</div>

@code {
    public class SalesData
    {
        public string Month { get; set; }
        public int Percentage { get; set; }
    }

    public List<SalesData> SalesPerformance = new List<SalesData>
    {
        new SalesData { Month = "Jan", Percentage = 95 },
        new SalesData { Month = "Feb", Percentage = 108 },
        new SalesData { Month = "Mar", Percentage = 102 },
        new SalesData { Month = "Apr", Percentage = 115 },
        new SalesData { Month = "May", Percentage = 98 },
        new SalesData { Month = "Jun", Percentage = 122 }
    };
}
```

### Server Response Time Thresholds

Monitor response times against SLA thresholds using `Type="SparklineType.Area"` on `SfSparkline` and multiple `SparklineRangeBand` components with different `Color` and `Opacity` values. Set `LineWidth` property to define line thickness and use `SparklineTooltipSettings` with `Format` property for displaying metrics.

```razor
@using Syncfusion.Blazor.Charts

<div class="server-monitor">
    <h4>API Response Time (ms)</h4>
    <div class="status">Current: 145ms - Within SLA</div>
    <SfSparkline DataSource="@ResponseTimes" 
                  Type="SparklineType.Area" 
                  Height="160px" 
                  Width="500px"
                  Fill="#E3F2FD"
                  LineWidth="2">
        <SparklineAxisSettings MinY="0" MaxY="300">
        </SparklineAxisSettings>
        <SparklineRangeBandSettings>
            <!-- Excellent: 0-100ms -->
            <SparklineRangeBand StartRange="0" 
                                EndRange="100" 
                                Color="#4CAF50" 
                                Opacity="0.2">
            </SparklineRangeBand>
            <!-- Good: 100-200ms -->
            <SparklineRangeBand StartRange="100" 
                                EndRange="200" 
                                Color="#8BC34A" 
                                Opacity="0.2">
            </SparklineRangeBand>
            <!-- Acceptable: 200-250ms -->
            <SparklineRangeBand StartRange="200" 
                                EndRange="250" 
                                Color="#FFC107" 
                                Opacity="0.3">
            </SparklineRangeBand>
            <!-- Critical: 250-300ms -->
            <SparklineRangeBand StartRange="250" 
                                EndRange="300" 
                                Color="#F44336" 
                                Opacity="0.3">
            </SparklineRangeBand>
        </SparklineRangeBandSettings>
        <SparklineBorder Color="#2196F3" Width="2">
        </SparklineBorder>
        <SparklineTooltipSettings TValue="int" 
                                  Visible="true" 
                                  Format="${y}ms">
        </SparklineTooltipSettings>
    </SfSparkline>
</div>

@code {
    public int[] ResponseTimes = new int[] 
    { 
        85, 92, 145, 132, 118, 165, 142, 128, 155, 138, 145, 152 
    };
}
```

### Budget Tracking

Visualize spending against budget limits using `Type="SparklineType.Column"` and `ValueType="SparklineValueType.Category"` properties on `SfSparkline`. Configure `XName` and `YName` for data mapping, and define budget thresholds using `SparklineRangeBand` elements with `StartRange`, `EndRange`, `Color`, and `Opacity` properties.

```razor
@using Syncfusion.Blazor.Charts

<div class="budget-tracker">
    <h4>Monthly Expenses vs Budget</h4>
    <SfSparkline DataSource="@ExpenseData" 
                  TValue="ExpenseInfo"
                  XName="Category"
                  YName="Amount"
                  ValueType="SparklineValueType.Category"
                  Type="SparklineType.Column" 
                  Height="170px" 
                  Width="500px"
                  Fill="#7E57C2">
        <SparklineAxisSettings MinY="0" MaxY="60000">
        </SparklineAxisSettings>
        <SparklineRangeBandSettings>
            <!-- Under budget -->
            <SparklineRangeBand StartRange="0" 
                                EndRange="40000" 
                                Color="#4CAF50" 
                                Opacity="0.2">
            </SparklineRangeBand>
            <!-- Near budget limit -->
            <SparklineRangeBand StartRange="40000" 
                                EndRange="50000" 
                                Color="#FFC107" 
                                Opacity="0.3">
            </SparklineRangeBand>
            <!-- Over budget -->
            <SparklineRangeBand StartRange="50000" 
                                EndRange="60000" 
                                Color="#F44336" 
                                Opacity="0.3">
            </SparklineRangeBand>
        </SparklineRangeBandSettings>
        <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }"
                                    Format="${Amount}">
        </SparklineDataLabelSettings>
    </SfSparkline>
</div>

@code {
    public class ExpenseInfo
    {
        public string Category { get; set; }
        public int Amount { get; set; }
    }

    public List<ExpenseInfo> ExpenseData = new List<ExpenseInfo>
    {
        new ExpenseInfo { Category = "Marketing", Amount = 38000 },
        new ExpenseInfo { Category = "R&D", Amount = 52000 },
        new ExpenseInfo { Category = "Sales", Amount = 42000 },
        new ExpenseInfo { Category = "Operations", Amount = 35000 }
    };
}
```

### Temperature Monitoring

Track temperature ranges for environmental control using `Type="SparklineType.Line"` with `LineWidth` and `Fill` properties on `SfSparkline`. Define optimal temperature ranges using `SparklineRangeBand` with `StartRange`, `EndRange`, `Color`, and `Opacity` properties, and add data point visibility using `SparklineMarkerSettings` with `Visible` and `Size` properties.

```razor
@using Syncfusion.Blazor.Charts

<div class="temp-monitor">
    <h4>Server Room Temperature (°C)</h4>
    <SfSparkline DataSource="@TemperatureReadings" 
                  Type="SparklineType.Line" 
                  Height="150px" 
                  Width="450px"
                  LineWidth="2"
                  Fill="#FF5722">
        <SparklineAxisSettings MinY="15" MaxY="30">
        </SparklineAxisSettings>
        <SparklineRangeBandSettings>
            <!-- Optimal range -->
            <SparklineRangeBand StartRange="18" 
                                EndRange="24" 
                                Color="#4CAF50" 
                                Opacity="0.25">
            </SparklineRangeBand>
        </SparklineRangeBandSettings>
        <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }"
                                 Size="4">
        </SparklineMarkerSettings>
        <SparklineTooltipSettings TValue="double" 
                                  Visible="true" 
                                  Format="${y}°C">
        </SparklineTooltipSettings>
    </SfSparkline>
    <div class="legend">
        <span class="legend-item">🟢 Optimal: 18-24°C</span>
    </div>
</div>

@code {
    public double[] TemperatureReadings = new double[] 
    { 
        21.5, 22.3, 20.8, 23.2, 21.9, 22.7, 23.5, 22.1, 21.6, 22.9 
    };
}
```

## Best Practices

### Color Selection

**Semantic Colors (Color Property):**
- Use `Color="#4CAF50"` (green) for good/acceptable ranges
- Use `Color="#FFC107"` (yellow/orange) for warning ranges
- Use `Color="#F44336"` (red) for critical/dangerous ranges
- Use `Color="#2196F3"` (blue) for informational ranges

**Color Consistency:**
```razor
@using Syncfusion.Blazor.Charts

<!-- Consistent semantic coloring across metrics -->
<div class="dashboard">
    <SfSparkline DataSource="@Metric1" Height="100px" Width="300px">
        <SparklineRangeBandSettings>
            <SparklineRangeBand StartRange="0" EndRange="50" 
                                Color="#F44336" Opacity="0.3">
            </SparklineRangeBand>
            <SparklineRangeBand StartRange="50" EndRange="75" 
                                Color="#FFC107" Opacity="0.3">
            </SparklineRangeBand>
            <SparklineRangeBand StartRange="75" EndRange="100" 
                                Color="#4CAF50" Opacity="0.3">
            </SparklineRangeBand>
        </SparklineRangeBandSettings>
    </SfSparkline>
    
    <SfSparkline DataSource="@Metric2" Height="100px" Width="300px">
        <SparklineRangeBandSettings>
            <!-- Same color scheme for consistency -->
            <SparklineRangeBand StartRange="0" EndRange="50" 
                                Color="#F44336" Opacity="0.3">
            </SparklineRangeBand>
            <SparklineRangeBand StartRange="50" EndRange="75" 
                                Color="#FFC107" Opacity="0.3">
            </SparklineRangeBand>
            <SparklineRangeBand StartRange="75" EndRange="100" 
                                Color="#4CAF50" Opacity="0.3">
            </SparklineRangeBand>
        </SparklineRangeBandSettings>
    </SfSparkline>
</div>
```

### Opacity Guidelines

**Low Opacity (`Opacity="0.2"` to `Opacity="0.3"`):**
- Multiple overlapping bands
- Background context zones
- Subtle visual cues

**Medium Opacity (`Opacity="0.4"` to `Opacity="0.5"`):**
- Standard range bands using `SparklineRangeBand`
- Balanced visibility
- Most common use case

**High Opacity (`Opacity="0.6"` to `Opacity="0.8"`):**
- Critical zones requiring emphasis
- Emphasis on specific ranges
- Limited use (can obscure underlying data)

### Range Definition

**Clear Boundaries (Using StartRange and EndRange):**
```razor
<!-- Good: Non-overlapping, clear ranges -->
<SparklineRangeBandSettings>
    <SparklineRangeBand StartRange="0" EndRange="40" Color="red" Opacity="0.3">
    </SparklineRangeBand>
    <SparklineRangeBand StartRange="40" EndRange="70" Color="yellow" Opacity="0.3">
    </SparklineRangeBand>
    <SparklineRangeBand StartRange="70" EndRange="100" Color="green" Opacity="0.3">
    </SparklineRangeBand>
</SparklineRangeBandSettings>
```

**Avoid Overlapping (StartRange and EndRange Conflicts):**
```razor
<!-- Avoid: Overlapping StartRange/EndRange values create confusion -->
<SparklineRangeBandSettings>
    <SparklineRangeBand StartRange="0" EndRange="60" Color="red" Opacity="0.3">
    </SparklineRangeBand>
    <SparklineRangeBand StartRange="50" EndRange="100" Color="green" Opacity="0.3">
    </SparklineRangeBand>
</SparklineRangeBandSettings>
```

### Accessibility

**Color Independence (Color and Opacity Properties):**
- Don't rely solely on `Color` property to convey meaning
- Add data labels using `SparklineDataLabelSettings` or tooltips using `SparklineTooltipSettings`
- Use different `Color` values with varying `Opacity` for pattern distinction
- Provide text descriptions alongside visualizations

**Contrast (Fill, LineWidth, Color, and Opacity):**
```razor
<!-- Good contrast with line color -->
<SfSparkline DataSource="@Data" 
              Fill="#0D47A1"
              LineWidth="2">
    <SparklineRangeBandSettings>
        <!-- Light band for dark line -->
        <SparklineRangeBand StartRange="50" 
                            EndRange="75" 
                            Color="#FFF9C4" 
                            Opacity="0.5">
        </SparklineRangeBand>
    </SparklineRangeBandSettings>
</SfSparkline>
```

### Performance Considerations

**Limit Band Count (SparklineRangeBand Elements):**
- Maximum 3-5 `SparklineRangeBand` elements per `SfSparkline`
- Too many bands reduce clarity
- Consider combining similar ranges with strategic `StartRange` and `EndRange` values

**Appropriate Usage (SparklineRangeBandSettings):**
- Use `SparklineRangeBandSettings` with `SparklineRangeBand` for threshold monitoring
- Not necessary for simple trend visualization
- Best for operational dashboards requiring visual thresholds

### Documentation

**Add Labels or Legends (Using Color, StartRange, EndRange Properties):**
```razor
@using Syncfusion.Blazor.Charts

<div class="metric-with-legend">
    <SfSparkline DataSource="@Data" Height="120px" Width="400px">
        <SparklineRangeBandSettings>
            <SparklineRangeBand StartRange="0" EndRange="60" 
                                Color="#F44336" Opacity="0.3">
            </SparklineRangeBand>
            <SparklineRangeBand StartRange="60" EndRange="80" 
                                Color="#FFC107" Opacity="0.3">
            </SparklineRangeBand>
            <SparklineRangeBand StartRange="80" EndRange="100" 
                                Color="#4CAF50" Opacity="0.3">
            </SparklineRangeBand>
        </SparklineRangeBandSettings>
    </SfSparkline>
    <div class="legend">
        <span class="critical">🔴 Critical: 0-60</span>
        <span class="warning">🟡 Warning: 60-80</span>
        <span class="good">🟢 Good: 80-100</span>
    </div>
</div>
@code {
    public double[] Data =
    {
        40, 55, 60, 70, 80, 65, 50
    };
}
```

## Common Patterns

### Two-Zone Pattern (Target Line)

Define two `SparklineRangeBand` elements with `StartRange` and `EndRange` values split at the target boundary, using distinct `Color` and `Opacity` properties to differentiate below-target and above-target zones.

```razor
@using Syncfusion.Blazor.Charts

<!-- Below/Above target -->
<SfSparkline DataSource="@Data" Height="120px" Width="350px">
    <SparklineRangeBandSettings>
        <SparklineRangeBand StartRange="0" EndRange="100" 
                            Color="#FFCDD2" Opacity="0.4">
        </SparklineRangeBand>
        <SparklineRangeBand StartRange="100" EndRange="200" 
                            Color="#C8E6C9" Opacity="0.4">
        </SparklineRangeBand>
    </SparklineRangeBandSettings>
</SfSparkline>
@code {
    public double[] Data =
    {
        40, 55, 60, 70, 80, 65, 50
    };
}


```

### Three-Zone Pattern (Traffic Light)

Implement three `SparklineRangeBand` elements with sequential `StartRange` and `EndRange` values representing critical, warning, and good zones. Apply semantic `Color` values (red, yellow, green) with consistent `Opacity` for traffic light visualization.

```razor
@using Syncfusion.Blazor.Charts

<!-- Critical/Warning/Good -->
<SfSparkline DataSource="@Data" Height="120px" Width="350px">
    <SparklineRangeBandSettings>
        <SparklineRangeBand StartRange="0" EndRange="60" 
                            Color="#F44336" Opacity="0.3">
        </SparklineRangeBand>
        <SparklineRangeBand StartRange="60" EndRange="80" 
                            Color="#FFC107" Opacity="0.3">
        </SparklineRangeBand>
        <SparklineRangeBand StartRange="80" EndRange="100" 
                            Color="#4CAF50" Opacity="0.3">
        </SparklineRangeBand>
    </SparklineRangeBandSettings>
</SfSparkline>

@code {
    public double[] Data =
    {
        40, 55, 60, 70, 80, 65, 50
    };
}
```

### Single Highlight Band

Create a single `SparklineRangeBand` with defined `StartRange` and `EndRange` values representing the optimal range. Use a single `Color` and subtle `Opacity` value to highlight only the target zone without overwhelming the visualization.

```razor
@using Syncfusion.Blazor.Charts

<!-- Emphasize optimal range only -->
<SfSparkline DataSource="@Data" Height="120px" Width="350px">
    <SparklineRangeBandSettings>
        <SparklineRangeBand StartRange="18" EndRange="24" 
                            Color="#4CAF50" Opacity="0.25">
        </SparklineRangeBand>
    </SparklineRangeBandSettings>
</SfSparkline>
```

Range bands transform sparklines from simple trend indicators into powerful monitoring tools that help users quickly assess performance against standards, targets, and thresholds.
