# Axis Customization

## Table of Contents
- [Overview](#overview)
- [Value Types](#value-types)
- [Axis Line Customization](#axis-line-customization)
- [Min and Max Values](#min-and-max-values)
- [Axis Value Display](#axis-value-display)

## Overview

While sparklines typically don't display full axes like traditional charts, axis configuration controls data scaling, value types, and reference lines.

## Value Types

The `ValueType` property specifies how X-axis values are interpreted. Use this property to define whether your data uses numeric values (`SparklineValueType.Numeric`), category strings (`SparklineValueType.Category`), or date/time values (`SparklineValueType.DateTime`).

### Numeric Value Type (Default)

For numeric X-axis values, set `ValueType="SparklineValueType.Numeric"` and configure `XName` and `YName` properties to map to numeric data fields:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@ExpenditureData" 
              TValue="ExpenditureInfo"
              XName="Year" 
              YName="Expense"
              ValueType="SparklineValueType.Numeric"
              Type="SparklineType.Column"
              Height="80px" 
              Width="300px">
</SfSparkline>

@code {
    public class ExpenditureInfo
    {
        public int Year { get; set; }
        public int Expense { get; set; }
    }

    public List<ExpenditureInfo> ExpenditureData = new List<ExpenditureInfo>
    {
        new ExpenditureInfo { Year = 2018, Expense = 190 },
        new ExpenditureInfo { Year = 2019, Expense = 165 },
        new ExpenditureInfo { Year = 2020, Expense = 158 },
        new ExpenditureInfo { Year = 2021, Expense = 175 },
        new ExpenditureInfo { Year = 2022, Expense = 200 },
        new ExpenditureInfo { Year = 2023, Expense = 180 }
    };
}
```

### Category Value Type

For string-based X-axis values, set `ValueType="SparklineValueType.Category"` to interpret X-axis values as categories with properties `XName` and `YName`:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@EmployeeData" 
              TValue="WorkDetails"
              XName="EmployeeName" 
              YName="WorkHours"
              ValueType="SparklineValueType.Category"
              Type="SparklineType.Column"
              Height="80px" 
              Width="350px">
</SfSparkline>

@code {
    public class WorkDetails
    {
        public string EmployeeName { get; set; }
        public double WorkHours { get; set; }
    }

    public List<WorkDetails> EmployeeData = new List<WorkDetails>
    {
        new WorkDetails { EmployeeName = "Robert", WorkHours = 60 },
        new WorkDetails { EmployeeName = "Andrew", WorkHours = 65 },
        new WorkDetails { EmployeeName = "Suyama", WorkHours = 70 },
        new WorkDetails { EmployeeName = "Michael", WorkHours = 80 },
        new WorkDetails { EmployeeName = "Janet", WorkHours = 55 }
    };
}
```

### DateTime Value Type

For date/time X-axis values, set `ValueType="SparklineValueType.DateTime"` to handle temporal data with `XName` and `YName` properties mapping to DateTime and numeric fields respectively:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@TimeSeriesData" 
              TValue="TimeSeriesPoint"
              XName="Date" 
              YName="Value"
              ValueType="SparklineValueType.DateTime"
              Type="SparklineType.Line"
              Height="80px" 
              Width="350px">
</SfSparkline>

@code {
    public class TimeSeriesPoint
    {
        public DateTime Date { get; set; }
        public double Value { get; set; }
    }

    public List<TimeSeriesPoint> TimeSeriesData = new List<TimeSeriesPoint>
    {
        new TimeSeriesPoint { Date = new DateTime(2026, 1, 1), Value = 150 },
        new TimeSeriesPoint { Date = new DateTime(2026, 1, 8), Value = 165 },
        new TimeSeriesPoint { Date = new DateTime(2026, 1, 15), Value = 158 },
        new TimeSeriesPoint { Date = new DateTime(2026, 1, 22), Value = 172 },
        new TimeSeriesPoint { Date = new DateTime(2026, 1, 29), Value = 180 }
    };
}
```

## Axis Line Customization

Display and customize the axis line (typically at Y=0) using the `SparklineAxisSettings` container and `LineSettings` property with options like `Visible`, `Color`, `Width`, and `DashArray`:

### Basic Axis Line

Set `Value` property to specify the axis line position and use `LineSettings` with `Visible="true"` to display it:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@ProfitLossData" 
              Type="SparklineType.Column"
              Height="80px" 
              Width="300px">
    <SparklineAxisSettings Value="0"
                           LineSettings="new SparklineAxisLineSettings { Visible = true }">
    </SparklineAxisSettings>
</SfSparkline>

@code {
    public int[] ProfitLossData = new int[] { 12, -5, 8, -3, 15, -8, 10, 6 };
}
```

### Styled Axis Line

Customize axis line appearance using `SparklineAxisLineSettings` with properties `Color`, `Width`, and `DashArray`:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@TemperatureData" 
              Type="SparklineType.Line"
              Height="80px" 
              Width="300px">
    <SparklineAxisSettings Value="0">
        <SparklineAxisLineSettings Visible="true"
                                   Color="#ff0000"
                                   Width="2"
                                   DashArray = "5,3">
        </SparklineAxisLineSettings>
    </SparklineAxisSettings>
</SfSparkline>

@code {
    public double[] TemperatureData = new double[] { -2.5, 1.2, -0.5, 3.1, 2.8, -1.5, 0.8 };
}
```

**Axis Line Properties:**
- `Visible` - Show/hide axis line
- `Color` - Line color
- `Width` - Line thickness
- `DashArray` - Dash pattern (e.g., "5,3" for dashed line)

## Min and Max Values

Control the data range displayed on both X and Y axes using the `SparklineAxisSettings` properties `MinY`, `MaxY`, `MinX`, and `MaxX`:

### Y-Axis Min/Max

Set `MinY` and `MaxY` properties in `SparklineAxisSettings` to define the Y-axis value range:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@ScoreData" 
              Type="SparklineType.Line"
              Height="80px" 
              Width="300px"
              LineWidth="2">
    <SparklineAxisSettings MinY="0" MaxY="100">
    </SparklineAxisSettings>
</SfSparkline>

@code {
    public int[] ScoreData = new int[] { 65, 72, 68, 85, 78, 92, 88 };
}
```

**Use case:** Display percentage data with fixed 0-100 range for consistency.

### X-Axis Min/Max

Set `MinX`, `MaxX`, `MinY`, and `MaxY` properties to control both X and Y axis ranges simultaneously:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@SimpleData" 
              Type="SparklineType.Line"
              Height="80px" 
              Width="300px">
    <SparklineAxisSettings MinX="-1" MaxX="8" MinY="-1" MaxY="8">
    </SparklineAxisSettings>
</SfSparkline>

@code {
    public int[] SimpleData = new int[] { 0, 6, 4, 1, 3, 2, 5 };
}
```

**Use case:** Add padding around data by extending min/max beyond actual data range.

### Dynamic Range Calculation

Calculate `MinY` and `MaxY` properties dynamically in the component lifecycle using `OnInitialized()` method:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@DynamicData" 
              Type="SparklineType.Area"
              Height="80px" 
              Width="300px">
    <SparklineAxisSettings MinY="@minValue" MaxY="@maxValue">
    </SparklineAxisSettings>
</SfSparkline>

@code {
    public double[] DynamicData = new double[] { 12.5, 18.2, 15.7, 22.3, 19.8 };
    
    private double minValue;
    private double maxValue;

    protected override void OnInitialized()
    {
        // Calculate min/max with 10% padding
        var dataMin = DynamicData.Min();
        var dataMax = DynamicData.Max();
        var range = dataMax - dataMin;
        
        minValue = dataMin - (range * 0.1);
        maxValue = dataMax + (range * 0.1);
    }
}
```

## Axis Value Display

### Zero-Based Axis

Display axis at Y=0 to show positive/negative values by setting the `Value` property to `"0"` in `SparklineAxisSettings`:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@ChangeData" 
              Type="SparklineType.Column"
              Height="80px" 
              Width="300px"
              Fill="#4CAF50" NegativePointColor ="#F44336 >
    <SparklineAxisSettings Value="0">

    <SparklineAxisLineSettings Visible="true"
                                   Color="#333"
                                   Width="1">
        </SparklineAxisLineSettings>

    </SparklineAxisSettings>
</SfSparkline>

@code {
    public int[] ChangeData = new int[] { 5, -3, 8, -6, 4, -2, 7, 3, -4, 6 };
}
```

### Custom Axis Value

Set the `Value` property in `SparklineAxisSettings` to a specific Y-value (e.g., target threshold) to display a reference line:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@PerformanceData" 
              Type="SparklineType.Line"
              Height="80px" 
              Width="300px">
    <SparklineAxisSettings Value="75">

        <SparklineAxisLineSettings Visible="true"
                                   Color="#ff9800"
                                   Width="2"
                                   DashArray="5,5">
        </SparklineAxisLineSettings>

    </SparklineAxisSettings>
</SfSparkline>

@code {
    public int[] PerformanceData = new int[] { 65, 72, 78, 85, 80, 88, 92 };
}
```

**Use case:** Show target line at 75% performance threshold.

## Complete Example: Multiple Axis Features

```razor
@using Syncfusion.Blazor.Charts

<div class="sparkline-container">
    <h4>Sales Performance vs Target</h4>
    
    <SfSparkline DataSource="@SalesPerformance" 
                  TValue="SalesData"
                  XName="Month" 
                  YName="Amount"
                  ValueType="SparklineValueType.Category"
                  Type="SparklineType.Column"
                  Height="100px" 
                  Width="400px"
                  Fill="#2196F3">
        
        <!-- Set Y-axis range to 0-120K -->
        <SparklineAxisSettings MinY="0" 
                               MaxY="120000"
                               Value="80000" >
        <SparklineAxisLineSettings Visible="true"
                                   Color="#4CAF50"
                                   Width="2"
                                   DashArray="5,3">
        </SparklineAxisLineSettings>
        </SparklineAxisSettings>
        
        <!-- Highlight high/low points -->
        <SparklineHighPointColor>#FFD700</SparklineHighPointColor>
        <SparklineLowPointColor>#FF6B6B</SparklineLowPointColor>
        
        <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.High, VisibleType.Low }" 
                                 Size="6">
        </SparklineMarkerSettings>
        
        <SparklineTooltipSettings TValue="SalesData" 
                                  Visible="true" 
                                  Format="${Month}: ${Amount:C}">
        </SparklineTooltipSettings>
    </SfSparkline>
    
    <p><em>Green dashed line shows $80K target</em></p>
</div>

@code {
    public class SalesData
    {
        public string Month { get; set; }
        public double Amount { get; set; }
    }

    public List<SalesData> SalesPerformance = new List<SalesData>
    {
        new SalesData { Month = "Jan", Amount = 65000 },
        new SalesData { Month = "Feb", Amount = 78000 },
        new SalesData { Month = "Mar", Amount = 85000 },
        new SalesData { Month = "Apr", Amount = 92000 },
        new SalesData { Month = "May", Amount = 88000 },
        new SalesData { Month = "Jun", Amount = 95000 }
    };
}
```

## Common Patterns

### Pattern: Percentage Charts

Always use 0-100 range for percentages by setting `MinY="0"` and `MaxY="100"` properties:

```razor
<SparklineAxisSettings MinY="0" MaxY="100">
</SparklineAxisSettings>
```

### Pattern: Symmetric Range

For data centered around zero, use `MinY="-@maxAbsValue"` and `MaxY="@maxAbsValue"` properties for balanced display:

```razor
@using Syncfusion.Blazor.Charts

<SparklineAxisSettings MinY="-@maxAbsValue" MaxY="@maxAbsValue">
</SparklineAxisSettings>

@code {
    private double maxAbsValue = 50; // Calculated from data
}
```

### Pattern: Fixed Baseline

Show consistent baseline across multiple sparklines by using `Value`, `MinY`, and `MaxY` properties consistently:

```razor
<SparklineAxisSettings Value="0" MinY="0" MaxY="1000">
</SparklineAxisSettings>
```

**Axis customization ensures sparklines display data in the most meaningful context.**
