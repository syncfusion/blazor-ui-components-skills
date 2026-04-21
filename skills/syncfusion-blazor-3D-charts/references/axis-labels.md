# Axis Labels for Blazor 3D Charts

This reference provides comprehensive guidance on configuring axis labels in Syncfusion Blazor 3D Charts, including formatting, rotation, placement, and customization options.

## Table of Contents

- [Overview](#overview)
- [Smart Axis Labels](#smart-axis-labels)
- [Edge Label Placement](#edge-label-placement)
- [Maximum Labels](#maximum-labels)
- [Label Formatting](#label-formatting)
- [Label Rotation](#label-rotation)
- [Label Customization](#label-customization)
- [Best Practices](#best-practices)

## Overview

Axis labels display descriptive information about axis values. They appear adjacent to the Y-axis and beneath the X-axis. Proper label configuration ensures readability and prevents overlap in 3D charts. The axis label behavior and appearance can be controlled using properties like `LabelIntersectAction`, `EdgeLabelPlacement`, `MaximumLabels`, `LabelFormat`, `LabelRotationAngle`, and `Chart3DAxisLabelStyle`.

## Smart Axis Labels

When axis labels overlap, use the `LabelIntersectAction` property to handle the overlap intelligently. This property accepts values like `Hide`, `Rotate45`, `Rotate90`, `Trim`, and `MultipleRows` to automatically manage overlapping labels on the X-axis.

### Hide Overlapping Labels

Use the `LabelIntersectAction` property set to `Hide` to hide overlapping labels:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Country Sales Analysis" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category" 
                         LabelIntersectAction="Syncfusion.Blazor.Chart3D.LabelIntersectAction.Hide">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@CountrySales" XName="Country" YName="Sales" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class SalesData
    {
        public string Country { get; set; }
        public double Sales { get; set; }
    }

    public List<SalesData> CountrySales = new List<SalesData>
    {
        new SalesData { Country = "United States", Sales = 120 },
        new SalesData { Country = "United Kingdom", Sales = 90 },
        new SalesData { Country = "Russian Federation", Sales = 75 },
        new SalesData { Country = "Saudi Arabia", Sales = 60 },
        new SalesData { Country = "South Africa", Sales = 85 },
        new SalesData { Country = "New Zealand", Sales = 50 },
        new SalesData { Country = "Netherlands", Sales = 95 },
        new SalesData { Country = "Switzerland", Sales = 70 }
    };
}
```

### Rotate Labels 45 Degrees

Use the `LabelIntersectAction` property set to `Rotate45` to rotate overlapping labels by 45 degrees:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Product Performance" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category" 
                         LabelIntersectAction="Syncfusion.Blazor.Chart3D.LabelIntersectAction.Rotate45">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ProductData" XName="Name" YName="Revenue" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class Product
    {
        public string Name { get; set; }
        public double Revenue { get; set; }
    }

    public List<Product> ProductData = new List<Product>
    {
        new Product { Name = "Gaming Laptop Pro", Revenue = 145 },
        new Product { Name = "Business Tablet Ultra", Revenue = 120 },
        new Product { Name = "Smart Phone Premium", Revenue = 180 },
        new Product { Name = "Wireless Headphones", Revenue = 95 },
        new Product { Name = "4K Monitor Display", Revenue = 130 },
        new Product { Name = "Mechanical Keyboard", Revenue = 75 }
    };
}
```

### Rotate Labels 90 Degrees

Use the `LabelIntersectAction` property set to `Rotate90` to rotate overlapping labels vertically:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Regional Revenue" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category" 
                         LabelIntersectAction="Syncfusion.Blazor.Chart3D.LabelIntersectAction.Rotate90">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@RegionalData" XName="Region" YName="Revenue" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class RegionData
    {
        public string Region { get; set; }
        public double Revenue { get; set; }
    }

    public List<RegionData> RegionalData = new List<RegionData>
    {
        new RegionData { Region = "North America East Coast", Revenue = 180 },
        new RegionData { Region = "Europe Western Region", Revenue = 145 },
        new RegionData { Region = "Asia Pacific North", Revenue = 200 },
        new RegionData { Region = "Middle East and Africa", Revenue = 120 },
        new RegionData { Region = "Latin America South", Revenue = 95 }
    };
}
```

### Trim Overlapping Labels

Use the `LabelIntersectAction` property set to `Trim` to truncate long labels:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Department Statistics" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category" 
                         LabelIntersectAction="Syncfusion.Blazor.Chart3D.LabelIntersectAction.Trim">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@DepartmentData" XName="Department" YName="Count" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class DeptData
    {
        public string Department { get; set; }
        public double Count { get; set; }
    }

    public List<DeptData> DepartmentData = new List<DeptData>
    {
        new DeptData { Department = "Human Resources", Count = 45 },
        new DeptData { Department = "Information Technology", Count = 120 },
        new DeptData { Department = "Sales and Marketing", Count = 85 },
        new DeptData { Department = "Customer Service", Count = 60 }
    };
}
```

## Edge Label Placement

Edge labels at the beginning or end of an axis may appear partially. Use the `EdgeLabelPlacement` property to adjust their positioning. This property accepts values like `Shift` (move labels inside the chart) and `Hide` (hide edge labels) to control how edge labels are displayed.

### Shift Edge Labels

Use the `EdgeLabelPlacement` property set to `Shift` to move edge labels inside the chart area:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Annual Performance" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis EdgeLabelPlacement="Syncfusion.Blazor.Chart3D.EdgeLabelPlacement.Shift" 
                         ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@AnnualData" XName="Quarter" YName="Performance" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class QuarterData
    {
        public string Quarter { get; set; }
        public double Performance { get; set; }
    }

    public List<QuarterData> AnnualData = new List<QuarterData>
    {
        new QuarterData { Quarter = "Q1 2023", Performance = 85 },
        new QuarterData { Quarter = "Q2 2023", Performance = 92 },
        new QuarterData { Quarter = "Q3 2023", Performance = 88 },
        new QuarterData { Quarter = "Q4 2023", Performance = 95 }
    };
}
```

### Hide Edge Labels

Use the `EdgeLabelPlacement` property set to `Hide` to hide labels that appear at the edges:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Monthly Trends" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis EdgeLabelPlacement="Syncfusion.Blazor.Chart3D.EdgeLabelPlacement.Hide" 
                         ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MonthlyData" XName="Month" YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class MonthlyRecord
    {
        public string Month { get; set; }
        public double Value { get; set; }
    }

    public List<MonthlyRecord> MonthlyData = new List<MonthlyRecord>
    {
        new MonthlyRecord { Month = "January", Value = 65 },
        new MonthlyRecord { Month = "February", Value = 70 },
        new MonthlyRecord { Month = "March", Value = 75 },
        new MonthlyRecord { Month = "April", Value = 80 },
        new MonthlyRecord { Month = "May", Value = 85 },
        new MonthlyRecord { Month = "June", Value = 90 }
    };
}
```

## Maximum Labels

Control the number of axis labels using the `MaximumLabels` property to specify the maximum number of labels displayed per 100 pixels. This property helps prevent label overcrowding on the axis.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Stock Performance" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis MaximumLabels="5" 
                         ValueType="Syncfusion.Blazor.Chart3D.ValueType.Double">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@StockData" XName="Day" YName="Price" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class StockPrice
    {
        public double Day { get; set; }
        public double Price { get; set; }
    }

    public List<StockPrice> StockData { get; set; }

    protected override void OnInitialized()
    {
        StockData = new List<StockPrice>();
        double price = 100;
        var random = new Random();

        for (int i = 1; i <= 30; i++)
        {
            price += (random.NextDouble() - 0.5) * 5;
            StockData.Add(new StockPrice { Day = i, Price = Math.Round(price, 2) });
        }
    }
}
```

## Label Formatting

Use the `LabelFormat` property to format axis labels according to different data types. This property accepts standard .NET format strings to customize how labels are displayed.

### Numeric Label Format

Format numeric labels using the `LabelFormat` property with standard .NET format strings:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Revenue Analysis" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DPrimaryYAxis LabelFormat="C0">
    </Chart3DPrimaryYAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@RevenueData" XName="Product" YName="Revenue" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class RevenueRecord
    {
        public string Product { get; set; }
        public double Revenue { get; set; }
    }

    public List<RevenueRecord> RevenueData = new List<RevenueRecord>
    {
        new RevenueRecord { Product = "Laptop", Revenue = 125000 },
        new RevenueRecord { Product = "Phone", Revenue = 185000 },
        new RevenueRecord { Product = "Tablet", Revenue = 145000 },
        new RevenueRecord { Product = "Monitor", Revenue = 95000 }
    };
}
```

### DateTime Label Format

Format date/time labels using the `LabelFormat` property with date/time format strings:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Timeline" WallColor="transparent" EnableRotation="true" 
        RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.DateTime" 
                        LabelFormat="MMM yyyy">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@TimelineData" XName="Date" YName="Sales" 
                    Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class TimelineRecord
    {
        public DateTime Date { get; set; }
        public double Sales { get; set; }
    }

    public List<TimelineRecord> TimelineData = new List<TimelineRecord>
    {
        new TimelineRecord { Date = new DateTime(2023, 1, 1), Sales = 120 },
        new TimelineRecord { Date = new DateTime(2023, 4, 1), Sales = 145 },
        new TimelineRecord { Date = new DateTime(2023, 7, 1), Sales = 170 },
        new TimelineRecord { Date = new DateTime(2023, 10, 1), Sales = 195 }
    };
}
```

### Percentage Label Format

Display values as percentages using the `LabelFormat` property with percentage format strings:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Market Share" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DPrimaryYAxis LabelFormat="P0">
    </Chart3DPrimaryYAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MarketData" XName="Company" YName="Share" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class MarketShare
    {
        public string Company { get; set; }
        public double Share { get; set; }
    }

    public List<MarketShare> MarketData = new List<MarketShare>
    {
        new MarketShare { Company = "Company A", Share = 0.35 },
        new MarketShare { Company = "Company B", Share = 0.25 },
        new MarketShare { Company = "Company C", Share = 0.20 },
        new MarketShare { Company = "Company D", Share = 0.15 },
        new MarketShare { Company = "Others", Share = 0.05 }
    };
}
```

## Label Rotation

Rotate axis labels to improve readability using the `LabelRotationAngle` property to specify a custom rotation angle in degrees.

### Custom Label Rotation

Rotate labels to a specific angle using the `LabelRotationAngle` property:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Technology Stack" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category" 
                         LabelRotationAngle ="45">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@TechData" XName="Technology" YName="Usage" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class TechStack
    {
        public string Technology { get; set; }
        public double Usage { get; set; }
    }

    public List<TechStack> TechData = new List<TechStack>
    {
        new TechStack { Technology = "Blazor WebAssembly", Usage = 85 },
        new TechStack { Technology = "ASP.NET Core", Usage = 92 },
        new TechStack { Technology = "Entity Framework", Usage = 78 },
        new TechStack { Technology = "SignalR", Usage = 65 }
    };
}
```

## Label Customization

Customize label appearance using the `Chart3DAxisLabelStyle` class to control font properties, size, color, and other visual aspects of axis labels.

### Custom Label Style

Customize label appearance with font, size, and color using the `Chart3DAxisLabelStyle` class with properties like `FontFamily`, `FontSize`, `FontWeight`, and `Color`:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Dashboard" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
        <Chart3DAxisLabelStyle FontFamily="Arial" FontSize="14px" 
                               FontWeight="600" Color="#0066CC" />
    </Chart3DPrimaryXAxis>
    
    <Chart3DPrimaryYAxis LabelFormat="C0">
        <Chart3DAxisLabelStyle FontFamily="Arial" FontSize="12px" 
                               FontWeight="500" Color="#333333" />
    </Chart3DPrimaryYAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData" XName="Region" YName="Amount" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class SalesRecord
    {
        public string Region { get; set; }
        public double Amount { get; set; }
    }

    public List<SalesRecord> SalesData = new List<SalesRecord>
    {
        new SalesRecord { Region = "North", Amount = 120000 },
        new SalesRecord { Region = "South", Amount = 145000 },
        new SalesRecord { Region = "East", Amount = 165000 },
        new SalesRecord { Region = "West", Amount = 135000 }
    };
}
```

### Label Template

Use custom templates for axis labels with the `Chart3DAxisLabelStyle` class to apply consistent styling across your chart labels:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Performance Metrics" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DPrimaryYAxis>
        <Chart3DAxisLabelStyle FontFamily="Arial" FontSize="12px" />
    </Chart3DPrimaryYAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MetricsData" XName="Metric" YName="Score" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class PerformanceMetric
    {
        public string Metric { get; set; }
        public double Score { get; set; }
    }

    public List<PerformanceMetric> MetricsData = new List<PerformanceMetric>
    {
        new PerformanceMetric { Metric = "Quality ⭐", Score = 92 },
        new PerformanceMetric { Metric = "Speed ⚡", Score = 88 },
        new PerformanceMetric { Metric = "Reliability 🔒", Score = 95 },
        new PerformanceMetric { Metric = "Cost 💰", Score = 78 }
    };
}
```

## Best Practices

1. **Handle Overlapping Labels**:
   - Use `LabelIntersectAction` when labels overlap
   - Choose appropriate action: Hide, Rotate45, Rotate90, or Trim
   - Test different options to find best fit for your data

2. **Edge Label Management**:
   - Use `EdgeLabelPlacement.Shift` to keep edge labels visible
   - Use `EdgeLabelPlacement.Hide` when edge labels are not critical
   - Ensure important data points at edges remain readable

3. **Label Count Optimization**:
   - Use `MaximumLabels` to control label density
   - Balance between readability and information density
   - Consider chart width when setting maximum labels

4. **Label Formatting**:
   - Use appropriate format strings (C for currency, P for percentage)
   - Keep formats consistent across similar charts
   - Consider locale-specific formatting for international apps

5. **Custom Rotation**:
   - Use `LabelRotation` for precise angle control
   - Common angles: 0° (horizontal), 45° (diagonal), 90° (vertical)
   - Ensure rotated labels don't exceed chart boundaries

6. **Label Styling**:
   - Use readable font sizes (12-14px recommended)
   - Ensure sufficient color contrast
   - Keep font styles consistent with overall app design

7. **Performance Considerations**:
   - Limit number of labels for large datasets
   - Use `MaximumLabels` to prevent label overcrowding
   - Consider aggregating data for charts with many points

8. **Accessibility**:
   - Ensure labels are readable at various screen sizes
   - Use high contrast colors
   - Provide alternative text descriptions for screen readers

9. **Date/Time Labels**:
   - Choose appropriate `IntervalType` (Days, Months, Years)
   - Use clear date formats (e.g., "MMM yyyy" for monthly data)
   - Align interval with data granularity

10. **Label Templates**:
    - Use templates for special formatting needs
    - Keep templates simple and performant
    - Test templates with various data ranges
