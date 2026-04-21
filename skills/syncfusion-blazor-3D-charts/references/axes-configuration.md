# Axes Configuration for Blazor 3D Charts

This reference provides comprehensive guidance on configuring axes in Syncfusion Blazor 3D Charts, including Category, Numeric, DateTime, DateTimeCategory, and Logarithmic axes.

## Table of Contents

- [Axis Types Overview](#axis-types-overview)
- [Category Axis](#category-axis)
- [Numeric Axis](#numeric-axis)
- [DateTime Axis](#datetime-axis)
- [DateTimeCategory Axis](#datetimecategory-axis)
- [Logarithmic Axis](#logarithmic-axis)
- [Common Axis Properties](#common-axis-properties)
- [Best Practices](#best-practices)

## Axis Types Overview

Syncfusion Blazor 3D Charts support multiple axis types for different data scenarios:

- **Category**: For text-based categorical data (e.g., country names, product categories)
- **Numeric**: For numeric values (default axis type)
- **DateTime**: For date/time data with continuous time scale
- **DateTimeCategory**: For date/time data with non-linear intervals (e.g., business days only)
- **Logarithmic**: For data with wide range of magnitudes (e.g., 10⁻⁶ to 10⁶)

Use `ValueType` property in `Chart3DPrimaryXAxis` or `Chart3DPrimaryYAxis` to specify the axis type.

## Category Axis

Category axes display text labels for discrete categories. Ideal for comparing values across different categories. Use the `ValueType` property set to `Syncfusion.Blazor.Chart3D.ValueType.Category` to enable this axis type.

### Basic Category Axis

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Olympic Medals" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MedalDetails" XName="Country" YName="Medals" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class MedalData
    {
        public string Country { get; set; }
        public double Medals { get; set; }
    }

    public List<MedalData> MedalDetails = new List<MedalData>
    {
        new MedalData { Country = "USA", Medals = 46 },
        new MedalData { Country = "GBR", Medals = 27 },
        new MedalData { Country = "CHN", Medals = 26 },
        new MedalData { Country = "RUS", Medals = 23 },
        new MedalData { Country = "AUS", Medals = 16 },
        new MedalData { Country = "IND", Medals = 36 }
    };
}
```

### Category Axis with Label Placement

Control label placement using the `LabelPlacement` property with options BetweenTicks or OnTicks. This property determines where labels are positioned relative to axis ticks.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Quarterly Sales" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis LabelPlacement="Syncfusion.Blazor.Chart3D.LabelPlacement.OnTicks" 
                         ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData" XName="Quarter" YName="Sales" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class QuarterlySales
    {
        public string Quarter { get; set; }
        public double Sales { get; set; }
    }

    public List<QuarterlySales> SalesData = new List<QuarterlySales>
    {
        new QuarterlySales { Quarter = "Q1", Sales = 120 },
        new QuarterlySales { Quarter = "Q2", Sales = 150 },
        new QuarterlySales { Quarter = "Q3", Sales = 180 },
        new QuarterlySales { Quarter = "Q4", Sales = 200 }
    };
}
```

### Category Axis with Custom Range

Set custom range using the `Minimum`, `Maximum`, and `Interval` properties to control the axis bounds and label spacing.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Product Sales" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis Minimum="1" Maximum="5" Interval="2" 
                         ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ProductData" XName="Product" YName="Sales" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ProductSales
    {
        public string Product { get; set; }
        public double Sales { get; set; }
    }

    public List<ProductSales> ProductData = new List<ProductSales>
    {
        new ProductSales { Product = "Product A", Sales = 120 },
        new ProductSales { Product = "Product B", Sales = 150 },
        new ProductSales { Product = "Product C", Sales = 180 },
        new ProductSales { Product = "Product D", Sales = 200 },
        new ProductSales { Product = "Product E", Sales = 165 },
        new ProductSales { Product = "Product F", Sales = 190 }
    };
}
```

### Indexed Category Axis

For multiple series with different categories, use the `IsIndexed` property set to `true`. This ensures each category is positioned at a specific index regardless of the data source.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Weather Comparison" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis IsIndexed="true" 
                         ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@Location1" XName="Month" YName="Temperature" 
                       Type="Chart3DSeriesType.Column" Name="Location 1">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@Location2" XName="Month" YName="Temperature" 
                       Type="Chart3DSeriesType.Column" Name="Location 2">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class WeatherData
    {
        public string Month { get; set; }
        public double Temperature { get; set; }
    }

    public List<WeatherData> Location1 = new List<WeatherData>
    {
        new WeatherData { Month = "Jan", Temperature = 15 },
        new WeatherData { Month = "Feb", Temperature = 17 },
        new WeatherData { Month = "Mar", Temperature = 20 }
    };

    public List<WeatherData> Location2 = new List<WeatherData>
    {
        new WeatherData { Month = "Jan", Temperature = 10 },
        new WeatherData { Month = "Apr", Temperature = 22 },
        new WeatherData { Month = "May", Temperature = 25 }
    };
}
```

## Numeric Axis

Numeric axis is the default axis type for displaying numeric values. Set the `ValueType` property to `Syncfusion.Blazor.Chart3D.ValueType.Double` or omit it to use the default numeric axis.

### Basic Numeric Axis

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Revenue Analysis" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Double">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@RevenueData" XName="Year" YName="Revenue" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class YearlyRevenue
    {
        public double Year { get; set; }
        public double Revenue { get; set; }
    }

    public List<YearlyRevenue> RevenueData = new List<YearlyRevenue>
    {
        new YearlyRevenue { Year = 2015, Revenue = 120 },
        new YearlyRevenue { Year = 2016, Revenue = 145 },
        new YearlyRevenue { Year = 2017, Revenue = 170 },
        new YearlyRevenue { Year = 2018, Revenue = 195 },
        new YearlyRevenue { Year = 2019, Revenue = 220 }
    };
}
```

### Numeric Axis with Custom Range

Customize range with the `Minimum`, `Maximum`, and `Interval` properties to define the axis scale and spacing between tick marks.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Performance" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis Minimum="5" Maximum="75" Interval="10" 
                         ValueType="Syncfusion.Blazor.Chart3D.ValueType.Double">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@PerformanceData" XName="Value" YName="Score" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class PerformanceMetric
    {
        public double Value { get; set; }
        public double Score { get; set; }
    }

    public List<PerformanceMetric> PerformanceData = new List<PerformanceMetric>
    {
        new PerformanceMetric { Value = 10, Score = 45 },
        new PerformanceMetric { Value = 20, Score = 62 },
        new PerformanceMetric { Value = 30, Score = 78 },
        new PerformanceMetric { Value = 40, Score = 85 },
        new PerformanceMetric { Value = 50, Score = 91 },
        new PerformanceMetric { Value = 60, Score = 94 },
        new PerformanceMetric { Value = 70, Score = 97 }
    };
}
```

### Numeric Axis with Range Padding

Control padding using the `RangePadding` property with options None, Round, Additional, Normal, or Auto. This property adds space around data values for better visualization.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Temperature Data" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryYAxis RangePadding="Syncfusion.Blazor.Chart3D.ChartRangePadding.Round">
    </Chart3DPrimaryYAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@TempData" XName="Day" YName="Temp" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class TemperatureData
    {
        public double Day { get; set; }
        public double Temp { get; set; }
    }

    public List<TemperatureData> TempData = new List<TemperatureData>
    {
        new TemperatureData { Day = 1, Temp = 23.5 },
        new TemperatureData { Day = 2, Temp = 25.8 },
        new TemperatureData { Day = 3, Temp = 27.3 },
        new TemperatureData { Day = 4, Temp = 26.2 },
        new TemperatureData { Day = 5, Temp = 24.9 }
    };
}
```

## DateTime Axis

DateTime axis displays date/time values on a continuous time scale. Set the `ValueType` property to `Syncfusion.Blazor.Chart3D.ValueType.DateTime` to enable this axis type.

### Basic DateTime Axis

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Trend" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.DateTime">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesTrend" XName="Date" YName="Sales" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class SalesRecord
    {
        public DateTime Date { get; set; }
        public double Sales { get; set; }
    }

    public List<SalesRecord> SalesTrend = new List<SalesRecord>
    {
        new SalesRecord { Date = new DateTime(2023, 1, 1), Sales = 120 },
        new SalesRecord { Date = new DateTime(2023, 3, 1), Sales = 145 },
        new SalesRecord { Date = new DateTime(2023, 5, 1), Sales = 170 },
        new SalesRecord { Date = new DateTime(2023, 7, 1), Sales = 195 },
        new SalesRecord { Date = new DateTime(2023, 9, 1), Sales = 220 },
        new SalesRecord { Date = new DateTime(2023, 11, 1), Sales = 240 }
    };
}
```

### DateTime Axis with Custom Range

Set minimum and maximum dates using the `Minimum` and `Maximum` properties to limit the axis range to a specific time period. The `Interval` property controls the spacing between axis labels.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Monthly Revenue" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis Minimum="@MinDate" Maximum="@MaxDate" Interval="1" 
                         ValueType="Syncfusion.Blazor.Chart3D.ValueType.DateTime">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MonthlyData" XName="Date" YName="Revenue" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public DateTime MinDate = new DateTime(2023, 1, 1);
    public DateTime MaxDate = new DateTime(2023, 12, 31);

    public class MonthlyRecord
    {
        public DateTime Date { get; set; }
        public double Revenue { get; set; }
    }

    public List<MonthlyRecord> MonthlyData = new List<MonthlyRecord>
    {
        new MonthlyRecord { Date = new DateTime(2023, 2, 1), Revenue = 150 },
        new MonthlyRecord { Date = new DateTime(2023, 4, 1), Revenue = 180 },
        new MonthlyRecord { Date = new DateTime(2023, 6, 1), Revenue = 210 },
        new MonthlyRecord { Date = new DateTime(2023, 8, 1), Revenue = 240 },
        new MonthlyRecord { Date = new DateTime(2023, 10, 1), Revenue = 270 }
    };
}
```

### DateTime Axis with Interval Customization

Customize intervals using the `Interval` and `IntervalType` properties to control the frequency of axis labels based on time units like Years, Months, Days, Hours, or Minutes.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Yearly Growth" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis Interval="2" 
                         IntervalType="Syncfusion.Blazor.Chart3D.IntervalType.Years" 
                         ValueType="Syncfusion.Blazor.Chart3D.ValueType.DateTime">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@YearlyGrowth" XName="Year" YName="Growth" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class GrowthData
    {
        public DateTime Year { get; set; }
        public double Growth { get; set; }
    }

    public List<GrowthData> YearlyGrowth = new List<GrowthData>
    {
        new GrowthData { Year = new DateTime(2015, 1, 1), Growth = 100 },
        new GrowthData { Year = new DateTime(2017, 1, 1), Growth = 150 },
        new GrowthData { Year = new DateTime(2019, 1, 1), Growth = 200 },
        new GrowthData { Year = new DateTime(2021, 1, 1), Growth = 280 },
        new GrowthData { Year = new DateTime(2023, 1, 1), Growth = 350 }
    };
}
```

## DateTimeCategory Axis

DateTimeCategory axis displays date/time values with non-linear intervals (e.g., business days only). Set the `ValueType` property to `Syncfusion.Blazor.Chart3D.ValueType.DateTimeCategory` to enable this axis type.

### Basic DateTimeCategory Axis

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Business Days Sales" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.DateTimeCategory">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@BusinessData" XName="Day" YName="Sales" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class DailySales
    {
        public DateTime Day { get; set; }
        public double Sales { get; set; }
    }

    public List<DailySales> BusinessData = new List<DailySales>
    {
        new DailySales { Day = new DateTime(2023, 10, 2), Sales = 120 },  // Monday
        new DailySales { Day = new DateTime(2023, 10, 3), Sales = 135 },  // Tuesday
        new DailySales { Day = new DateTime(2023, 10, 4), Sales = 150 },  // Wednesday
        new DailySales { Day = new DateTime(2023, 10, 5), Sales = 145 },  // Thursday
        new DailySales { Day = new DateTime(2023, 10, 6), Sales = 160 }   // Friday
        // Weekend days (Oct 7-8) are omitted - chart shows only business days
    };
}
```

## Logarithmic Axis

Logarithmic axis uses logarithmic scale for data spanning multiple orders of magnitude. Set the `ValueType` property to `Syncfusion.Blazor.Chart3D.ValueType.Logarithmic` to enable this axis type.

### Basic Logarithmic Axis

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Exponential Growth" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.DateTime">
    </Chart3DPrimaryXAxis>
    
    <Chart3DPrimaryYAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Logarithmic">
    </Chart3DPrimaryYAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ExponentialData" XName="Date" YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ExponentialRecord
    {
        public DateTime Date { get; set; }
        public double Value { get; set; }
    }

    public List<ExponentialRecord> ExponentialData = new List<ExponentialRecord>
    {
        new ExponentialRecord { Date = new DateTime(2015, 1, 1), Value = 100 },
        new ExponentialRecord { Date = new DateTime(2016, 1, 1), Value = 500 },
        new ExponentialRecord { Date = new DateTime(2017, 1, 1), Value = 1000 },
        new ExponentialRecord { Date = new DateTime(2018, 1, 1), Value = 5000 },
        new ExponentialRecord { Date = new DateTime(2019, 1, 1), Value = 10000 },
        new ExponentialRecord { Date = new DateTime(2020, 1, 1), Value = 50000 }
    };
}
```

### Logarithmic Axis with Custom Range

Set custom range using the `Minimum` and `Maximum` properties to define the logarithmic scale bounds for better control over data visualization.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Population Growth" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.DateTime">
    </Chart3DPrimaryXAxis>
    
    <Chart3DPrimaryYAxis Minimum="100" Maximum="100000" 
                         ValueType="Syncfusion.Blazor.Chart3D.ValueType.Logarithmic">
    </Chart3DPrimaryYAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@PopulationData" XName="Year" YName="Population" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class PopulationRecord
    {
        public DateTime Year { get; set; }
        public double Population { get; set; }
    }

    public List<PopulationRecord> PopulationData = new List<PopulationRecord>
    {
        new PopulationRecord { Year = new DateTime(1950, 1, 1), Population = 200 },
        new PopulationRecord { Year = new DateTime(1960, 1, 1), Population = 800 },
        new PopulationRecord { Year = new DateTime(1970, 1, 1), Population = 2000 },
        new PopulationRecord { Year = new DateTime(1980, 1, 1), Population = 8000 },
        new PopulationRecord { Year = new DateTime(1990, 1, 1), Population = 20000 },
        new PopulationRecord { Year = new DateTime(2000, 1, 1), Population = 80000 }
    };
}
```

### Logarithmic Axis with Custom Base

Change logarithmic base using the `LogBase` property to specify the base of logarithm (default is 10). This allows you to customize how data is scaled on the logarithmic axis.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Scientific Data" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.DateTime">
    </Chart3DPrimaryXAxis>
    
    <Chart3DPrimaryYAxis LogBase="2" 
                         ValueType="Syncfusion.Blazor.Chart3D.ValueType.Logarithmic">
    </Chart3DPrimaryYAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ScientificData" XName="Time" YName="Measurement" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ScientificMeasurement
    {
        public DateTime Time { get; set; }
        public double Measurement { get; set; }
    }

    public List<ScientificMeasurement> ScientificData = new List<ScientificMeasurement>
    {
        new ScientificMeasurement { Time = new DateTime(2023, 1, 1), Measurement = 2 },
        new ScientificMeasurement { Time = new DateTime(2023, 2, 1), Measurement = 4 },
        new ScientificMeasurement { Time = new DateTime(2023, 3, 1), Measurement = 16 },
        new ScientificMeasurement { Time = new DateTime(2023, 4, 1), Measurement = 64 },
        new ScientificMeasurement { Time = new DateTime(2023, 5, 1), Measurement = 256 }
    };
}
```

## Common Axis Properties

Properties applicable to all axis types:

### Axis Title

Display axis labels using the `Title` property to provide descriptive text for the axis. This property accepts string values and helps users understand the data being displayed.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Analysis" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category" Title="Products">
    </Chart3DPrimaryXAxis>
    
    <Chart3DPrimaryYAxis Title="Sales (in thousands)" >
    </Chart3DPrimaryYAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ProductSales" XName="Product" YName="Sales" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ProductData
    {
        public string Product { get; set; }
        public double Sales { get; set; }
    }

    public List<ProductData> ProductSales = new List<ProductData>
    {
        new ProductData { Product = "Laptop", Sales = 120 },
        new ProductData { Product = "Phone", Sales = 180 },
        new ProductData { Product = "Tablet", Sales = 140 }
    };
}
```

### Axis Line Customization

Customize axis lines and grid lines using the `Chart3DMajorGridLines` and `Chart3DMinorGridLines` components with properties like `Width` and `Color` to control appearance.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Custom Axis Lines" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
        <Chart3DMajorGridLines Width="2" Color="blue" />
        <Chart3DMinorGridLines Width="1" Color="lightblue" />
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ChartData" XName="Category" YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class DataPoint
    {
        public string Category { get; set; }
        public double Value { get; set; }
    }

    public List<DataPoint> ChartData = new List<DataPoint>
    {
        new DataPoint { Category = "A", Value = 50 },
        new DataPoint { Category = "B", Value = 70 },
        new DataPoint { Category = "C", Value = 90 }
    };
}
```

### Multiple Y-Axes

Create multiple axes using the `Chart3DAxes` collection and configure secondary axes with properties like `Name` to identify each axis, `OpposedPosition` to position them on opposite sides, and `Title` for labeling. Associate series to specific axes using the `YAxisName` property.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Multiple Axes" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DPrimaryYAxis Title="Temperature (°C)">
    </Chart3DPrimaryYAxis>
    
    <Chart3DAxes>
        <Chart3DAxis Name="yAxis1" OpposedPosition="true" Title="Rainfall (mm)">
        </Chart3DAxis>
    </Chart3DAxes>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@WeatherData" XName="Month" YName="Temperature" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@WeatherData" XName="Month" YName="Rainfall" 
                       Type="Chart3DSeriesType.Column" YAxisName="yAxis1">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class WeatherInfo
    {
        public string Month { get; set; }
        public double Temperature { get; set; }
        public double Rainfall { get; set; }
    }

    public List<WeatherInfo> WeatherData = new List<WeatherInfo>
    {
        new WeatherInfo { Month = "Jan", Temperature = 15, Rainfall = 50 },
        new WeatherInfo { Month = "Feb", Temperature = 18, Rainfall = 40 },
        new WeatherInfo { Month = "Mar", Temperature = 22, Rainfall = 30 }
    };
}
```

## Best Practices

1. **Choose Appropriate Axis Type**:
   - Use Category for discrete text-based data
   - Use Numeric for continuous numeric data
   - Use DateTime for time-series data with regular intervals
   - Use DateTimeCategory for non-linear time data
   - Use Logarithmic for wide-ranging data

2. **Set Proper Ranges**:
   - Let the chart auto-calculate ranges when possible
   - Use custom ranges for focused data views
   - Ensure minimum < maximum
   - Choose intervals that make sense for your data

3. **Label Placement**:
   - Use BetweenTicks (default) for most scenarios
   - Use OnTicks when precise alignment is needed

4. **Performance Considerations**:
   - Avoid too many axis labels (use appropriate intervals)
   - Use indexed category axis for multiple series with different categories

5. **Accessibility**:
   - Always provide meaningful axis titles
   - Use appropriate label formats
   - Ensure sufficient contrast for axis lines

6. **Range Padding**:
   - Use None for exact data boundaries
   - Use Round for cleaner axis values
   - Use Additional for extra padding
   - Use Normal for balanced padding
   - Use Auto to let the chart decide

7. **Logarithmic Axes**:
   - Only use when data spans multiple orders of magnitude
   - Choose appropriate LogBase (default is 10)
   - Ensure all data values are positive

8. **DateTime Intervals**:
   - Choose IntervalType based on data granularity
   - Use Years for long-term trends
   - Use Months/Days for medium-term data
   - Use Hours/Minutes for short-term data
