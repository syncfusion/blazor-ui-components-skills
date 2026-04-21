# Legend for Blazor 3D Charts

This reference provides comprehensive guidance on configuring legends in Syncfusion Blazor 3D Charts, including positioning, customization, and interaction options.

## Table of Contents

- [Overview](#overview)
- [Enable Legend](#enable-legend)
- [Position and Alignment](#position-and-alignment)
- [Legend Reverse](#legend-reverse)
- [Legend Customization](#legend-customization)
- [Legend Size](#legend-size)
- [Legend Item Size](#legend-item-size)
- [Legend Paging](#legend-paging)
- [Legend Text Wrap](#legend-text-wrap)
- [Series Selection](#series-selection)
- [Collapsing Legend Items](#collapsing-legend-items)
- [Legend Title](#legend-title)
- [Arrow Page Navigation](#arrow-page-navigation)
- [Legend Item Padding](#legend-item-padding)
- [Best Practices](#best-practices)

## Overview

Legends provide information about series rendered in the 3D chart. They help users identify different data series through color-coded icons and labels.

## Enable Legend

Enable legend by setting the `Visible` property to `"true"` in the `Chart3DLegendSettings` component:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Analysis" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData" Name="Q1 Sales" XName="Product" 
                       YName="Q1" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@SalesData" Name="Q2 Sales" XName="Product" 
                       YName="Q2" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>
</SfChart3D>

@code {
    public class QuarterlySales
    {
        public string Product { get; set; }
        public double Q1 { get; set; }
        public double Q2 { get; set; }
    }

    public List<QuarterlySales> SalesData = new List<QuarterlySales>
    {
        new QuarterlySales { Product = "Laptop", Q1 = 120, Q2 = 145 },
        new QuarterlySales { Product = "Phone", Q1 = 180, Q2 = 200 },
        new QuarterlySales { Product = "Tablet", Q1 = 140, Q2 = 155 }
    };
}
```

## Position and Alignment

### Legend Position

Position legend at Top, Bottom, Left, or Right using the `Position` property in `Chart3DLegendSettings`:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Olympic Medals" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MedalData" Name="Gold" XName="Country" 
                       YName="Gold" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@MedalData" Name="Silver" XName="Country" 
                       YName="Silver" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@MedalData" Name="Bronze" XName="Country" 
                       YName="Bronze" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
    
    <Chart3DLegendSettings Visible="true" 
                           Position="Syncfusion.Blazor.Chart3D.LegendPosition.Top">
    </Chart3DLegendSettings>
</SfChart3D>

@code {
    public class MedalCount
    {
        public string Country { get; set; }
        public double Gold { get; set; }
        public double Silver { get; set; }
        public double Bronze { get; set; }
    }

    public List<MedalCount> MedalData = new List<MedalCount>
    {
        new MedalCount { Country = "USA", Gold = 50, Silver = 70, Bronze = 45 },
        new MedalCount { Country = "China", Gold = 40, Silver = 60, Bronze = 55 },
        new MedalCount { Country = "Japan", Gold = 70, Silver = 60, Bronze = 50 }
    };
}
```

### Custom Position

Position legend using the `Chart3DLocation` component with `X` and `Y` coordinate properties:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Market Share" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MarketData" Name="Product A" XName="Quarter" 
                       YName="Share" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
    
    <Chart3DLegendSettings Visible="true" 
                           Position="Syncfusion.Blazor.Chart3D.LegendPosition.Custom">
        <Chart3DLocation X="250" Y="30"></Chart3DLocation>
    </Chart3DLegendSettings>
</SfChart3D>

@code {
    public class MarketInfo
    {
        public string Quarter { get; set; }
        public double Share { get; set; }
    }

    public List<MarketInfo> MarketData = new List<MarketInfo>
    {
        new MarketInfo { Quarter = "Q1", Share = 25 },
        new MarketInfo { Quarter = "Q2", Share = 30 },
        new MarketInfo { Quarter = "Q3", Share = 28 },
        new MarketInfo { Quarter = "Q4", Share = 32 }
    };
}
```

### Legend Alignment

Align legend using the `Alignment` property in `Chart3DLegendSettings` with values (Near, Center, Far):

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Revenue Report" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@RevenueData" Name="Revenue" XName="Month" 
                       YName="Amount" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
    
    <Chart3DLegendSettings Visible="true" 
                           Position="Syncfusion.Blazor.Chart3D.LegendPosition.Bottom"
                           Alignment="Syncfusion.Blazor.Chart3D.Alignment.Far">
    </Chart3DLegendSettings>
</SfChart3D>

@code {
    public class MonthlyRevenue
    {
        public string Month { get; set; }
        public double Amount { get; set; }
    }

    public List<MonthlyRevenue> RevenueData = new List<MonthlyRevenue>
    {
        new MonthlyRevenue { Month = "Jan", Amount = 12000 },
        new MonthlyRevenue { Month = "Feb", Amount = 14500 },
        new MonthlyRevenue { Month = "Mar", Amount = 16500 }
    };
}
```

## Legend Reverse

Reverse legend item order using the `Reverse` property in `Chart3DLegendSettings`:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Performance Metrics" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MetricsData" Name="First Quarter" XName="Category" 
                       YName="Value1" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@MetricsData" Name="Second Quarter" XName="Category" 
                       YName="Value2" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@MetricsData" Name="Third Quarter" XName="Category" 
                       YName="Value3" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
    
    <Chart3DLegendSettings Visible="true" Reverse="true"></Chart3DLegendSettings>
</SfChart3D>

@code {
    public class MetricData
    {
        public string Category { get; set; }
        public double Value1 { get; set; }
        public double Value2 { get; set; }
        public double Value3 { get; set; }
    }

    public List<MetricData> MetricsData = new List<MetricData>
    {
        new MetricData { Category = "A", Value1 = 50, Value2 = 60, Value3 = 70 },
        new MetricData { Category = "B", Value1 = 40, Value2 = 55, Value3 = 65 }
    };
}
```

## Legend Customization

### Legend Shape

Customize legend icon shape using the `LegendShape` property on individual `Chart3DSeries` components:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Dashboard" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesInfo" Name="Product A" XName="Region" 
                       YName="Sales1" Type="Chart3DSeriesType.Column"
                       LegendShape="Syncfusion.Blazor.Chart3D.LegendShape.Circle">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@SalesInfo" Name="Product B" XName="Region" 
                       YName="Sales2" Type="Chart3DSeriesType.Column"
                       LegendShape="Syncfusion.Blazor.Chart3D.LegendShape.Diamond">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@SalesInfo" Name="Product C" XName="Region" 
                       YName="Sales3" Type="Chart3DSeriesType.Column"
                       LegendShape="Syncfusion.Blazor.Chart3D.LegendShape.Triangle">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>
</SfChart3D>

@code {
    public class RegionalSales
    {
        public string Region { get; set; }
        public double Sales1 { get; set; }
        public double Sales2 { get; set; }
        public double Sales3 { get; set; }
    }

    public List<RegionalSales> SalesInfo = new List<RegionalSales>
    {
        new RegionalSales { Region = "North", Sales1 = 120, Sales2 = 95, Sales3 = 85 },
        new RegionalSales { Region = "South", Sales1 = 145, Sales2 = 110, Sales3 = 100 }
    };
}
```

## Legend Size

Set legend dimensions using the `Height` and `Width` properties in `Chart3DLegendSettings`:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Annual Report" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@AnnualData" Name="Revenue" XName="Year" 
                       YName="Amount" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
    
    <Chart3DLegendSettings Visible="true" Height="60" Width="350">
        <Chart3DLegendBorder Color="#0066CC" Width="2"></Chart3DLegendBorder>
    </Chart3DLegendSettings>
</SfChart3D>

@code {
    public class AnnualReport
    {
        public string Year { get; set; }
        public double Amount { get; set; }
    }

    public List<AnnualReport> AnnualData = new List<AnnualReport>
    {
        new AnnualReport { Year = "2021", Amount = 120000 },
        new AnnualReport { Year = "2022", Amount = 145000 },
        new AnnualReport { Year = "2023", Amount = 170000 }
    };
}
```

## Legend Item Size

Customize legend icon size using the `ShapeHeight` and `ShapeWidth` properties in `Chart3DLegendSettings`:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Product Comparison" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ProductData" Name="Product A" XName="Category" 
                       YName="Value" Type="Chart3DSeriesType.Column"
                       LegendShape="Syncfusion.Blazor.Chart3D.LegendShape.Circle">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
    
    <Chart3DLegendSettings Visible="true" ShapeHeight="20" ShapeWidth="20">
    </Chart3DLegendSettings>
</SfChart3D>

@code {
    public class ProductInfo
    {
        public string Category { get; set; }
        public double Value { get; set; }
    }

    public List<ProductInfo> ProductData = new List<ProductInfo>
    {
        new ProductInfo { Category = "Electronics", Value = 85 },
        new ProductInfo { Category = "Clothing", Value = 92 },
        new ProductInfo { Category = "Food", Value = 78 }
    };
}
```

## Legend Paging

Legend paging is enabled automatically when items exceed bounds by configuring `Width` and `Padding` properties in `Chart3DLegendSettings`:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Multi-Series Chart" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ChartData" Name="Series 1" XName="X" YName="Y1">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@ChartData" Name="Series 2" XName="X" YName="Y2">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@ChartData" Name="Series 3" XName="X" YName="Y3">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@ChartData" Name="Series 4" XName="X" YName="Y4">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
    
    <Chart3DLegendSettings Visible="true" Width="200" Padding="10" ShapePadding="10">
    </Chart3DLegendSettings>
</SfChart3D>

@code {
    public class MultiSeriesData
    {
        public string X { get; set; }
        public double Y1 { get; set; }
        public double Y2 { get; set; }
        public double Y3 { get; set; }
        public double Y4 { get; set; }
    }

    public List<MultiSeriesData> ChartData = new List<MultiSeriesData>
    {
        new MultiSeriesData { X = "A", Y1 = 12, Y2 = 22, Y3 = 32, Y4 = 42 },
        new MultiSeriesData { X = "B", Y1 = 15, Y2 = 25, Y3 = 35, Y4 = 45 }
    };
}
```

## Legend Text Wrap

Wrap legend text using the `TextWrap` and `MaximumLabelWidth` properties in `Chart3DLegendSettings`:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Technology Analysis" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@TechData" Name="Cloud Computing Services" 
                       XName="Category" YName="Value" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@TechData" Name="Artificial Intelligence Solutions" 
                       XName="Category" YName="Value2" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
    
    <Chart3DLegendSettings Visible="true" 
                           Position="Syncfusion.Blazor.Chart3D.LegendPosition.Right"
                           TextWrap="@Syncfusion.Blazor.TextWrap.Wrap" 
                           MaximumLabelWidth="60">
    </Chart3DLegendSettings>
</SfChart3D>

@code {
    public class TechnologyData
    {
        public string Category { get; set; }
        public double Value { get; set; }
        public double Value2 { get; set; }
    }

    public List<TechnologyData> TechData = new List<TechnologyData>
    {
        new TechnologyData { Category = "Q1", Value = 85, Value2 = 92 },
        new TechnologyData { Category = "Q2", Value = 90, Value2 = 95 }
    };
}
```

## Series Selection

Enable series selection through legend using the `ToggleVisibility` property in `Chart3DLegendSettings`:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Comparison" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100"
           SelectionMode="Syncfusion.Blazor.Chart3D.SelectionMode.Series">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ComparisonData" Name="Product A" XName="Month" 
                       YName="Sales1" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@ComparisonData" Name="Product B" XName="Month" 
                       YName="Sales2" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
    
    <Chart3DLegendSettings Visible="true" ToggleVisibility="false">
    </Chart3DLegendSettings>
</SfChart3D>

@code {
    public class SalesComparison
    {
        public string Month { get; set; }
        public double Sales1 { get; set; }
        public double Sales2 { get; set; }
    }

    public List<SalesComparison> ComparisonData = new List<SalesComparison>
    {
        new SalesComparison { Month = "Jan", Sales1 = 120, Sales2 = 95 },
        new SalesComparison { Month = "Feb", Sales1 = 145, Sales2 = 110 }
    };
}
```

## Collapsing Legend Items

Hide legend for a series by setting the `Name` property to an empty string (`Name=""`) on `Chart3DSeries`:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Selective Legend" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ChartData" Name="Visible Series" XName="X" 
                       YName="Y1" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@ChartData" Name="" XName="X" 
                       YName="Y2" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>
</SfChart3D>

@code {
    public class ChartPoint
    {
        public string X { get; set; }
        public double Y1 { get; set; }
        public double Y2 { get; set; }
    }

    public List<ChartPoint> ChartData = new List<ChartPoint>
    {
        new ChartPoint { X = "A", Y1 = 50, Y2 = 40 },
        new ChartPoint { X = "B", Y1 = 60, Y2 = 55 }
    };
}
```

## Legend Title

Add title to legend using the `Title` property in `Chart3DLegendSettings`:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Regional Performance" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@RegionData" Name="North" XName="Quarter" 
                       YName="North" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@RegionData" Name="South" XName="Quarter" 
                       YName="South" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
    
    <Chart3DLegendSettings Visible="true" Title="Regions">
        <Chart3DLegendTitleStyle FontSize="14px" Color="#0066CC" FontWeight="bold">
        </Chart3DLegendTitleStyle>
    </Chart3DLegendSettings>
</SfChart3D>

@code {
    public class RegionalData
    {
        public string Quarter { get; set; }
        public double North { get; set; }
        public double South { get; set; }
    }

    public List<RegionalData> RegionData = new List<RegionalData>
    {
        new RegionalData { Quarter = "Q1", North = 120, South = 95 },
        new RegionalData { Quarter = "Q2", North = 145, South = 110 }
    };
}
```

## Arrow Page Navigation

Enable arrow navigation by setting the `AllowPaging` property to `"true"` in `Chart3DLegendSettings`:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Data Analysis" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@AnalysisData" Name="Series 1" XName="X" YName="Y1">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@AnalysisData" Name="Series 2" XName="X" YName="Y2">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@AnalysisData" Name="Series 3" XName="X" YName="Y3">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
    
    <Chart3DLegendSettings Visible="true" Width="180" Height="25" AllowPaging="true">
    </Chart3DLegendSettings>
</SfChart3D>

@code {
    public class AnalysisPoint
    {
        public string X { get; set; }
        public double Y1 { get; set; }
        public double Y2 { get; set; }
        public double Y3 { get; set; }
    }

    public List<AnalysisPoint> AnalysisData = new List<AnalysisPoint>
    {
        new AnalysisPoint { X = "A", Y1 = 12, Y2 = 22, Y3 = 32 }
    };
}
```

## Legend Item Padding

Adjust space between legend items using the `ItemPadding` property in `Chart3DLegendSettings`:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Spaced Legend" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SpacedData" Name="Series 1" XName="X" YName="Y">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
    
    <Chart3DLegendSettings Visible="true" ItemPadding="25">
    </Chart3DLegendSettings>
</SfChart3D>

@code {
    public class SpacedDataPoint
    {
        public string X { get; set; }
        public double Y { get; set; }
    }

    public List<SpacedDataPoint> SpacedData = new List<SpacedDataPoint>
    {
        new SpacedDataPoint { X = "A", Y = 50 }
    };
}
```

## Best Practices

1. **Enable Appropriately**:
   - Always enable legend for multi-series charts
   - Consider disabling for single-series charts
   - Use legend to help users identify series

2. **Position Selection**:
   - Use Bottom (default) for most charts
   - Use Right for charts with many categories
   - Use Top when bottom space is limited
   - Use Custom position sparingly

3. **Legend Size**:
   - Let chart auto-calculate size when possible
   - Set explicit size only when needed
   - Ensure legend doesn't consume too much space

4. **Legend Shapes**:
   - Use different shapes for better distinction
   - Keep shapes consistent with chart type
   - Use SeriesType for automatic matching

5. **Text Wrapping**:
   - Enable wrap for long series names
   - Set appropriate MaximumLabelWidth
   - Consider abbreviating long names

6. **Paging**:
   - Automatic paging handles overflow gracefully
   - Use arrow navigation for cleaner look
   - Adjust legend size before relying on paging

7. **Series Selection**:
   - Use ToggleVisibility for interactive filtering
   - Disable when using programmatic selection
   - Enable highlighting for better UX

8. **Legend Title**:
   - Add title for clarity (e.g., "Products", "Regions")
   - Style title to match chart theme
   - Keep title concise

9. **Performance**:
   - Limit number of series in legend
   - Use paging for many series
   - Consider hiding legend for 20+ series

10. **Accessibility**:
    - Ensure legend text is readable
    - Use high contrast colors
    - Provide keyboard navigation support
