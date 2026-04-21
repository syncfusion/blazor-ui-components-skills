# Data Labels for Blazor 3D Charts

This reference provides comprehensive guidance on configuring data labels in Syncfusion Blazor 3D Charts, including positioning, formatting, templates, and customization options.

## Table of Contents

- [Overview](#overview)
- [Enable Data Labels](#enable-data-labels)
- [Label Position](#label-position)
- [Label Template](#label-template)
- [Text Mapping](#text-mapping)
- [Label Formatting](#label-formatting)
- [Label Margin](#label-margin)
- [Label Customization](#label-customization)
- [Specific Label Customization](#specific-label-customization)
- [Best Practices](#best-practices)

## Overview

Data labels display information about data points directly on the chart. They are added to series by enabling the `Visible` property in the `Chart3DDataLabel` component. The `Chart3DDataLabel` component manages all label configurations, and labels arrange smartly to avoid overlapping by default.

## Enable Data Labels

Enable data labels by setting the `Visible` property to `"true"` in the `Chart3DDataLabel` component:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Performance" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData" XName="Quarter" YName="Sales" 
                       Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true"></Chart3DDataLabel>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class QuarterSales
    {
        public string Quarter { get; set; }
        public double Sales { get; set; }
    }

    public List<QuarterSales> SalesData = new List<QuarterSales>
    {
        new QuarterSales { Quarter = "Q1", Sales = 120 },
        new QuarterSales { Quarter = "Q2", Sales = 145 },
        new QuarterSales { Quarter = "Q3", Sales = 165 },
        new QuarterSales { Quarter = "Q4", Sales = 190 }
    };
}
```

## Label Position

Control label placement using the `Position` property in `Chart3DDataLabel` with values (Top, Middle, Bottom):

### Top Position (Default)

The `Position` property set to `Chart3DDataLabelPosition.Top` places labels above the data points:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Revenue Analysis" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@RevenueData" XName="Month" YName="Revenue" 
                       Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true" 
                              Position="Syncfusion.Blazor.Chart3D.Chart3DDataLabelPosition.Top">
            </Chart3DDataLabel>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class MonthlyRevenue
    {
        public string Month { get; set; }
        public double Revenue { get; set; }
    }

    public List<MonthlyRevenue> RevenueData = new List<MonthlyRevenue>
    {
        new MonthlyRevenue { Month = "Jan", Revenue = 85000 },
        new MonthlyRevenue { Month = "Feb", Revenue = 92000 },
        new MonthlyRevenue { Month = "Mar", Revenue = 88000 },
        new MonthlyRevenue { Month = "Apr", Revenue = 95000 }
    };
}
```

### Middle Position

The `Position` property set to `Chart3DDataLabelPosition.Middle` places labels at the center of data points:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Product Performance" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ProductData" XName="Product" YName="Units" 
                       Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true" 
                              Position="Syncfusion.Blazor.Chart3D.Chart3DDataLabelPosition.Middle">
            </Chart3DDataLabel>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ProductSales
    {
        public string Product { get; set; }
        public double Units { get; set; }
    }

    public List<ProductSales> ProductData = new List<ProductSales>
    {
        new ProductSales { Product = "Laptop", Units = 450 },
        new ProductSales { Product = "Phone", Units = 620 },
        new ProductSales { Product = "Tablet", Units = 380 },
        new ProductSales { Product = "Monitor", Units = 290 }
    };
}
```

### Bottom Position

The `Position` property set to `Chart3DDataLabelPosition.Bottom` places labels below the data points:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Regional Sales" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@RegionalData" XName="Region" YName="Amount" 
                       Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true" 
                              Position="Syncfusion.Blazor.Chart3D.Chart3DDataLabelPosition.Bottom">
            </Chart3DDataLabel>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class RegionSales
    {
        public string Region { get; set; }
        public double Amount { get; set; }
    }

    public List<RegionSales> RegionalData = new List<RegionSales>
    {
        new RegionSales { Region = "North", Amount = 125000 },
        new RegionSales { Region = "South", Amount = 145000 },
        new RegionSales { Region = "East", Amount = 165000 },
        new RegionSales { Region = "West", Amount = 135000 }
    };
}
```

## Label Template

Create custom label templates using the `Chart3DDataLabelTemplate` component with placeholders `${point.x}` and `${point.y}` to display custom-formatted data:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Dashboard" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesReport" XName="Period" YName="Value" 
                       Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true" NameField="Label">
                <Chart3DDataLabelTemplate>
                    @{
                        var data = context as Chart3DDataPointInfo;
                    }
                    <table style="background-color: #0066CC; border: 2px solid white;">
                        <tr>
                            <td style="color: white; font-weight: bold; padding: 5px;">
                                @data.PointText: @data.Y
                            </td>
                        </tr>
                    </table>
                </Chart3DDataLabelTemplate>
            </Chart3DDataLabel>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class SalesInfo
    {
        public string Period { get; set; }
        public double Value { get; set; }
        public string Label { get; set; }
    }

    public List<SalesInfo> SalesReport = new List<SalesInfo>
    {
        new SalesInfo { Period = "Q1", Value = 120, Label = "First Quarter" },
        new SalesInfo { Period = "Q2", Value = 145, Label = "Second Quarter" },
        new SalesInfo { Period = "Q3", Value = 165, Label = "Third Quarter" },
        new SalesInfo { Period = "Q4", Value = 190, Label = "Fourth Quarter" }
    };
}
```

## Text Mapping

Map text from the data source using the `NameField` property in `Chart3DDataLabel` to display custom text values from the data object:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Product Categories" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@CategoryData" XName="Code" YName="Sales" 
                       Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true" NameField="CategoryName"></Chart3DDataLabel>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class CategoryInfo
    {
        public string Code { get; set; }
        public double Sales { get; set; }
        public string CategoryName { get; set; }
    }

    public List<CategoryInfo> CategoryData = new List<CategoryInfo>
    {
        new CategoryInfo { Code = "A1", Sales = 120, CategoryName = "Electronics" },
        new CategoryInfo { Code = "B2", Sales = 145, CategoryName = "Clothing" },
        new CategoryInfo { Code = "C3", Sales = 165, CategoryName = "Food Items" },
        new CategoryInfo { Code = "D4", Sales = 135, CategoryName = "Home Goods" }
    };
}
```

## Label Formatting

Format data labels using the `Format` property in `Chart3DDataLabel` with standard .NET format codes (N1 for numeric, P1 for percentage, C1 for currency):

### Numeric Format (N1)

The `Format` property set to `"N1"` displays values with one decimal place:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Temperature Data" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@TempData" XName="Month" YName="Temp" 
                       Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true" Format="N1"></Chart3DDataLabel>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class TemperatureRecord
    {
        public string Month { get; set; }
        public double Temp { get; set; }
    }

    public List<TemperatureRecord> TempData = new List<TemperatureRecord>
    {
        new TemperatureRecord { Month = "Jan", Temp = 23.45 },
        new TemperatureRecord { Month = "Feb", Temp = 25.87 },
        new TemperatureRecord { Month = "Mar", Temp = 27.32 },
        new TemperatureRecord { Month = "Apr", Temp = 29.61 }
    };
}
```

### Currency Format (C1)

The `Format` property set to `"C0"` displays values as currency with no decimal places:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Revenue Report" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@RevenueReport" XName="Quarter" YName="Revenue" 
                       Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true" Format="C0"></Chart3DDataLabel>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class QuarterRevenue
    {
        public string Quarter { get; set; }
        public double Revenue { get; set; }
    }

    public List<QuarterRevenue> RevenueReport = new List<QuarterRevenue>
    {
        new QuarterRevenue { Quarter = "Q1", Revenue = 125000 },
        new QuarterRevenue { Quarter = "Q2", Revenue = 145000 },
        new QuarterRevenue { Quarter = "Q3", Revenue = 165000 },
        new QuarterRevenue { Quarter = "Q4", Revenue = 185000 }
    };
}
```

### Percentage Format (P1)

The `Format` property set to `"P1"` displays values as percentages with one decimal place:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Market Share" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MarketData" XName="Company" YName="Share" 
                       Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true" Format="P1"></Chart3DDataLabel>
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
        new MarketShare { Company = "Company B", Share = 0.28 },
        new MarketShare { Company = "Company C", Share = 0.22 },
        new MarketShare { Company = "Company D", Share = 0.15 }
    };
}
```

## Label Margin

Add spacing around labels using the `Chart3DDataLabelMargin` component with properties `Bottom`, `Left`, `Right`, and `Top` to define margin values in pixels:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Analysis" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData" XName="Period" YName="Sales" 
                       Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true">
                <Chart3DDataLabelBorder Color="#0066CC" Width="2"></Chart3DDataLabelBorder>
                <Chart3DDataLabelMargin Bottom="8" Left="8" Right="8" Top="8">
                </Chart3DDataLabelMargin>
            </Chart3DDataLabel>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class SalesPeriod
    {
        public string Period { get; set; }
        public double Sales { get; set; }
    }

    public List<SalesPeriod> SalesData = new List<SalesPeriod>
    {
        new SalesPeriod { Period = "Week 1", Sales = 28 },
        new SalesPeriod { Period = "Week 2", Sales = 34 },
        new SalesPeriod { Period = "Week 3", Sales = 42 },
        new SalesPeriod { Period = "Week 4", Sales = 38 }
    };
}
```

## Label Customization

Customize label appearance using the `Fill` property for background color, `Chart3DDataLabelBorder` component for border styling, and `Chart3DDataLabelFont` component for text formatting:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Performance Metrics" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MetricsData" XName="Metric" YName="Score" 
                       Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true" Fill="#FF6347">
                <Chart3DDataLabelBorder Width="2" Color="#FFD700"></Chart3DDataLabelBorder>
                <Chart3DDataLabelFont Color="white" FontWeight="bold" FontSize="12px">
                </Chart3DDataLabelFont>
            </Chart3DDataLabel>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class MetricScore
    {
        public string Metric { get; set; }
        public double Score { get; set; }
    }

    public List<MetricScore> MetricsData = new List<MetricScore>
    {
        new MetricScore { Metric = "Quality", Score = 92 },
        new MetricScore { Metric = "Speed", Score = 88 },
        new MetricScore { Metric = "Reliability", Score = 95 },
        new MetricScore { Metric = "Cost", Score = 78 }
    };
}
```

## Specific Label Customization

Customize specific labels using the `DataLabelRendering` event with `Chart3DTextRenderEventArgs` to apply conditional formatting, modify text, or hide labels:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Olympic Medals" DataLabelRendering="CustomizeLabel" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MedalData" XName="Country" YName="Gold" 
                       Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true"></Chart3DDataLabel>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class MedalCount
    {
        public string Country { get; set; }
        public double Gold { get; set; }
    }

    public List<MedalCount> MedalData = new List<MedalCount>
    {
        new MedalCount { Country = "USA", Gold = 50 },
        new MedalCount { Country = "China", Gold = 40 },
        new MedalCount { Country = "Japan", Gold = 70 },
        new MedalCount { Country = "Australia", Gold = 60 }
    };

    public void CustomizeLabel(Chart3DTextRenderEventArgs args)
    {
        // Highlight the maximum value
        if (args.Point.Index == 2)  // Japan has highest medals
        {
            args.Text = "★ " + args.Text + " ★";
        }
        
        // Hide labels for values below 50
        if ((double)args.Point.Y < 50)
        {
            args.Cancel = true;
        }
    }
}
```

### Multiple Series Label Customization

The `DataLabelRendering` event with `args.Series.Name` allows different label formatting for multiple series:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Comparison" DataLabelRendering="CustomizeSeriesLabels" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
        <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
        </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData" Name="Product A" XName="Quarter" 
                       YName="ProductA" Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true"></Chart3DDataLabel>
        </Chart3DSeries>
        <Chart3DSeries DataSource="@SalesData" Name="Product B" XName="Quarter" 
                       YName="ProductB" Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true"></Chart3DDataLabel>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class QuarterSales
    {
        public string Quarter { get; set; }
        public double ProductA { get; set; }
        public double ProductB { get; set; }
    }

    public List<QuarterSales> SalesData = new List<QuarterSales>
    {
        new QuarterSales { Quarter = "Q1", ProductA = 120, ProductB = 95 },
        new QuarterSales { Quarter = "Q2", ProductA = 145, ProductB = 110 },
        new QuarterSales { Quarter = "Q3", ProductA = 165, ProductB = 130 },
        new QuarterSales { Quarter = "Q4", ProductA = 185, ProductB = 150 }
    };

    public void CustomizeSeriesLabels(Chart3DTextRenderEventArgs args)
    {
        // Add prefix based on series
        if (args.Series.Name == "Product A")
        {
            args.Text = "A: " + args.Text;
        }
        else if (args.Series.Name == "Product B")
        {
            args.Text = "B: " + args.Text;
        }
    }
}
```

## Best Practices

1. **Enable Appropriately**:
   - Only enable data labels when they add value
   - Avoid cluttering charts with too many labels
   - Consider chart size and number of data points

2. **Position Selection**:
   - Use Top for column charts (default, most readable)
   - Use Middle for bars or when space is limited
   - Use Bottom sparingly, mainly for special cases

3. **Formatting**:
   - Match format to data type (C for currency, P for percentage, N for numbers)
   - Keep decimal places consistent across series
   - Use appropriate precision (C0 for whole dollars, N2 for precise numbers)

4. **Templates**:
   - Use templates for complex label content
   - Keep template HTML simple and lightweight
   - Test templates with various data values

5. **Text Mapping**:
   - Map descriptive names for better readability
   - Ensure mapped text is concise
   - Use abbreviations when full text is too long

6. **Margins and Spacing**:
   - Add margin when labels have borders
   - Balance margin size with chart dimensions
   - Ensure margins don't cause overlap

7. **Customization**:
   - Use high contrast colors for readability
   - Match label style with overall chart theme
   - Keep font sizes legible (minimum 10-12px)

8. **Event-Based Customization**:
   - Use TextRender for dynamic label modifications
   - Highlight important data points
   - Hide redundant or insignificant labels

9. **Performance**:
   - Limit number of visible data labels
   - Avoid complex templates for large datasets
   - Use text mapping instead of templates when possible

10. **Accessibility**:
    - Ensure sufficient color contrast
    - Use readable font sizes
    - Don't rely solely on color to convey information
