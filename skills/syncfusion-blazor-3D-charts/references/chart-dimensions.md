# Chart Dimensions for Blazor 3D Charts

This reference provides comprehensive guidance on configuring dimensions and 3D properties of Syncfusion Blazor 3D Charts, including size, rotation angle, tilt angle, and depth.

## Table of Contents

- [Overview](#overview)
- [Chart Size](#chart-size)
- [Rotation Angle](#rotation-angle)
- [Tilt Angle](#tilt-angle)
- [Depth](#depth)
- [Combined Configuration](#combined-configuration)
- [Best Practices](#best-practices)

## Overview

Chart dimensions control the physical size and 3D perspective of charts using properties like `Width`, `Height`, `RotationAngle`, `TiltAngle`, and `Depth`. Proper configuration ensures optimal visualization and user experience across different screen sizes and devices.

## Chart Size

Configure chart dimensions using the `Width` and `Height` properties to set fixed or responsive dimensions.

### Set Size Using Width and Height

Configure chart size using `Width` and `Height` properties with pixel or percentage values:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Fixed Size Chart" Width="800px" Height="450px" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData" XName="Product" YName="Sales" 
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

    public List<ProductSales> SalesData = new List<ProductSales>
    {
        new ProductSales { Product = "Laptop", Sales = 120 },
        new ProductSales { Product = "Phone", Sales = 145 },
        new ProductSales { Product = "Tablet", Sales = 95 },
        new ProductSales { Product = "Monitor", Sales = 130 }
    };
}
```

### Percentage-Based Size

Use percentage values in `Width` and `Height` properties for responsive sizing:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Responsive Chart" Width="100%" Height="500px" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@RevenueData" XName="Quarter" YName="Revenue" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class QuarterRevenue
    {
        public string Quarter { get; set; }
        public double Revenue { get; set; }
    }

    public List<QuarterRevenue> RevenueData = new List<QuarterRevenue>
    {
        new QuarterRevenue { Quarter = "Q1", Revenue = 125000 },
        new QuarterRevenue { Quarter = "Q2", Revenue = 145000 },
        new QuarterRevenue { Quarter = "Q3", Revenue = 165000 },
        new QuarterRevenue { Quarter = "Q4", Revenue = 185000 }
    };
}
```

### Container-Based Sizing

Set `Width` and `Height` properties to 100% to allow the chart fill its container:

```cshtml
@using Syncfusion.Blazor.Chart3D

<div style="width: 900px; height: 600px; border: 1px solid #ccc;">
    <SfChart3D Title="Container-Sized Chart" Width="100%" Height="100%" 
               WallColor="transparent" EnableRotation="true" 
               RotationAngle="7" TiltAngle="10" Depth="100">
        <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
        </Chart3DPrimaryXAxis>
        
        <Chart3DSeriesCollection>
            <Chart3DSeries DataSource="@PerformanceData" XName="Team" YName="Score" 
                           Type="Chart3DSeriesType.Column">
            </Chart3DSeries>
        </Chart3DSeriesCollection>
    </SfChart3D>
</div>

@code {
    public class TeamPerformance
    {
        public string Team { get; set; }
        public double Score { get; set; }
    }

    public List<TeamPerformance> PerformanceData = new List<TeamPerformance>
    {
        new TeamPerformance { Team = "Team A", Score = 92 },
        new TeamPerformance { Team = "Team B", Score = 88 },
        new TeamPerformance { Team = "Team C", Score = 95 },
        new TeamPerformance { Team = "Team D", Score = 85 }
    };
}
```

## Rotation Angle

Control horizontal rotation of the 3D chart using the `RotationAngle` property (0-360 degrees). Enable or disable user rotation interaction with `EnableRotation` property.

### Basic Rotation

Set the `RotationAngle` property to apply horizontal rotation:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="15 Degree Rotation" Width="700px" Height="450px" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="15" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MonthlyDetails" XName="Month" YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class MonthlyData
    {
        public string Month { get; set; }
        public double Value { get; set; }
    }

    public List<MonthlyData> MonthlyDetails = new List<MonthlyData>
    {
        new MonthlyData { Month = "Jan", Value = 85 },
        new MonthlyData { Month = "Feb", Value = 92 },
        new MonthlyData { Month = "Mar", Value = 78 },
        new MonthlyData { Month = "Apr", Value = 88 },
        new MonthlyData { Month = "May", Value = 95 }
    };
}
```

### Maximum Rotation

Increase the `RotationAngle` property for a more dramatic horizontal rotation effect:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="45 Degree Rotation" Width="700px" Height="450px" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="45" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesReport" XName="Region" YName="Sales" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class RegionalSales
    {
        public string Region { get; set; }
        public double Sales { get; set; }
    }

    public List<RegionalSales> SalesReport = new List<RegionalSales>
    {
        new RegionalSales { Region = "North", Sales = 120000 },
        new RegionalSales { Region = "South", Sales = 145000 },
        new RegionalSales { Region = "East", Sales = 165000 },
        new RegionalSales { Region = "West", Sales = 135000 }
    };
}
```

### Comparison of Rotation Angles

Compare different `RotationAngle` values and `EnableRotation` settings:

```cshtml
@using Syncfusion.Blazor.Chart3D

<div style="display: flex; gap: 20px;">
    <div>
        <h4>Rotation: 0°</h4>
        <SfChart3D Title="No Rotation" Width="350px" Height="300px" 
                   WallColor="transparent" EnableRotation="false" 
                   RotationAngle="0" TiltAngle="10" Depth="100">
            <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
            </Chart3DPrimaryXAxis>
            <Chart3DSeriesCollection>
                <Chart3DSeries DataSource="@CompareValues" XName="X" YName="Y" 
                               Type="Chart3DSeriesType.Column">
                </Chart3DSeries>
            </Chart3DSeriesCollection>
        </SfChart3D>
    </div>
    
    <div>
        <h4>Rotation: 30°</h4>
        <SfChart3D Title="Medium Rotation" Width="350px" Height="300px" 
                   WallColor="transparent" EnableRotation="true" 
                   RotationAngle="30" TiltAngle="10" Depth="100">
            <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
            </Chart3DPrimaryXAxis>
            <Chart3DSeriesCollection>
                <Chart3DSeries DataSource="@CompareValues" XName="X" YName="Y" 
                               Type="Chart3DSeriesType.Column">
                </Chart3DSeries>
            </Chart3DSeriesCollection>
        </SfChart3D>
    </div>
</div>

@code {
    public class CompareData
    {
        public string X { get; set; }
        public double Y { get; set; }
    }

    public List<CompareData> CompareValues = new List<CompareData>
    {
        new CompareData { X = "A", Y = 50 },
        new CompareData { X = "B", Y = 70 },
        new CompareData { X = "C", Y = 60 }
    };
}
```

## Tilt Angle

Control vertical tilt of the 3D chart using the `TiltAngle` property (0-360 degrees) to adjust the viewing perspective.

### Minimal Tilt

Set the `TiltAngle` property to a lower value for subtle vertical tilt:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="5 Degree Tilt" Width="700px" Height="450px" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="5" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@TiltData" XName="Category" YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class TiltDataPoint
    {
        public string Category { get; set; }
        public double Value { get; set; }
    }

    public List<TiltDataPoint> TiltData = new List<TiltDataPoint>
    {
        new TiltDataPoint { Category = "Item 1", Value = 45 },
        new TiltDataPoint { Category = "Item 2", Value = 62 },
        new TiltDataPoint { Category = "Item 3", Value = 58 }
    };
}
```

### Maximum Tilt

Increase the `TiltAngle` property for a more dramatic vertical perspective:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="30 Degree Tilt" Width="700px" Height="450px" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="30" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MaxTiltValues" XName="Label" YName="Count" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class MaxTiltData
    {
        public string Label { get; set; }
        public double Count { get; set; }
    }

    public List<MaxTiltData> MaxTiltValues = new List<MaxTiltData>
    {
        new MaxTiltData { Label = "Product A", Count = 125 },
        new MaxTiltData { Label = "Product B", Count = 145 },
        new MaxTiltData { Label = "Product C", Count = 165 }
    };
}
```

## Depth

Control the 3D depth effect using the `Depth` property to adjust the perceived thickness and perspective intensity of the chart.

### Shallow Depth

Set the `Depth` property to a lower value for a subtle 3D effect:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Depth 50" Width="700px" Height="450px" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="50">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@DepthData" XName="Item" YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class DepthDataPoint
    {
        public string Item { get; set; }
        public double Value { get; set; }
    }

    public List<DepthDataPoint> DepthData = new List<DepthDataPoint>
    {
        new DepthDataPoint { Item = "Q1", Value = 85 },
        new DepthDataPoint { Item = "Q2", Value = 92 },
        new DepthDataPoint { Item = "Q3", Value = 78 }
    };
}
```

### Deep Perspective

Increase the `Depth` property for a pronounced 3D effect:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Depth 200" Width="700px" Height="450px" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="200">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@DeepData" XName="Month" YName="Sales" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class DeepDataPoint
    {
        public string Month { get; set; }
        public double Sales { get; set; }
    }

    public List<DeepDataPoint> DeepData = new List<DeepDataPoint>
    {
        new DeepDataPoint { Month = "Jan", Sales = 120 },
        new DeepDataPoint { Month = "Feb", Sales = 145 },
        new DeepDataPoint { Month = "Mar", Sales = 165 }
    };
}
```

## Combined Configuration

Combine multiple properties (`Width`, `Height`, `RotationAngle`, `TiltAngle`, `Depth`, and `EnableRotation`) to achieve optimal visualization.

### Optimal 3D Presentation

Balance `Width`, `Height`, `RotationAngle`, `TiltAngle`, and `Depth` properties for best viewing experience:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Optimized 3D View" Width="900px" Height="500px" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="15" TiltAngle="15" Depth="120">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DTooltipSettings Enable="true"></Chart3DTooltipSettings>
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@OptimizedData" Name="Sales" XName="Product" 
                       YName="Sales" Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true"></Chart3DDataLabel>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class OptimizedDataPoint
    {
        public string Product { get; set; }
        public double Sales { get; set; }
    }

    public List<OptimizedDataPoint> OptimizedData = new List<OptimizedDataPoint>
    {
        new OptimizedDataPoint { Product = "Laptop", Sales = 450 },
        new OptimizedDataPoint { Product = "Phone", Sales = 620 },
        new OptimizedDataPoint { Product = "Tablet", Sales = 380 },
        new OptimizedDataPoint { Product = "Monitor", Sales = 290 }
    };
}
```

### Dramatic 3D Effect

Set higher values for `RotationAngle`, `TiltAngle`, and `Depth` properties to create dramatic perspective:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Dramatic Perspective" Width="1000px" Height="600px" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="35" TiltAngle="25" Depth="180">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@DramaticData" Name="Revenue" XName="Year" 
                       YName="Amount" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class DramaticDataPoint
    {
        public string Year { get; set; }
        public double Amount { get; set; }
    }

    public List<DramaticDataPoint> DramaticData = new List<DramaticDataPoint>
    {
        new DramaticDataPoint { Year = "2020", Amount = 125000 },
        new DramaticDataPoint { Year = "2021", Amount = 145000 },
        new DramaticDataPoint { Year = "2022", Amount = 165000 },
        new DramaticDataPoint { Year = "2023", Amount = 185000 }
    };
}
```

### Subtle 3D Effect

Set lower values for `RotationAngle`, `TiltAngle`, and `Depth` properties for a subtle 3D appearance:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Subtle 3D View" Width="800px" Height="450px" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="5" Depth="80">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SubtleData" Name="Performance" XName="Team" 
                       YName="Score" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class SubtleDataPoint
    {
        public string Team { get; set; }
        public double Score { get; set; }
    }

    public List<SubtleDataPoint> SubtleData = new List<SubtleDataPoint>
    {
        new SubtleDataPoint { Team = "Team A", Score = 85 },
        new SubtleDataPoint { Team = "Team B", Score = 92 },
        new SubtleDataPoint { Team = "Team C", Score = 78 }
    };
}
```

## Best Practices

1. **Chart Size** (`Width` and `Height`):
   - Use fixed sizes for consistent layout
   - Use percentages for responsive designs
   - Minimum recommended: 600px width, 400px height
   - Consider mobile breakpoints for responsive applications

2. **Rotation Angle** (`RotationAngle`):
   - Recommended range: 7-15° for subtle effect
   - Use 20-35° for dramatic presentations
   - Avoid extreme angles (>45°) that distort data
   - Enable `EnableRotation` property for user interaction

3. **Tilt Angle** (`TiltAngle`):
   - Recommended range: 10-20° for optimal viewing
   - Lower values (5-10°) for subtle tilt
   - Higher values (20-30°) for top-down views
   - Balance with `RotationAngle` for best effect

4. **Depth** (`Depth`):
   - Standard depth: 100 (balanced 3D effect)
   - Shallow depth: 50-80 (subtle 3D)
   - Deep depth: 120-200 (pronounced 3D)
   - Higher `Depth` values require more rendering resources

5. **Combined Settings**:
   - Start with defaults: `RotationAngle="7"`, `TiltAngle="10"`, `Depth="100"`
   - Adjust `RotationAngle`, `TiltAngle`, and `Depth` incrementally to find optimal view
   - Test with actual data distribution
   - Consider data density when setting `Depth` property

6. **Responsive Design** (`Width` and `Height`):
   - Use percentage `Width` for fluid layouts
   - Set minimum dimensions with CSS
   - Test on multiple screen sizes
   - Consider touch interactions for mobile

7. **Performance** (`Depth` and size properties):
   - Larger `Width` and `Height` require more rendering time
   - Higher `Depth` values increase complexity
   - Test performance with target data volume
   - Consider using lower `Depth` for large datasets

8. **User Experience** (`EnableRotation`, `RotationAngle`, `TiltAngle`):
   - Enable `EnableRotation` property for exploration
   - Provide clear view of all data points with balanced angles
   - Avoid extreme `RotationAngle` and `TiltAngle` that hide important data
   - Maintain readability of labels

9. **Accessibility** (`Width`, `Height`, and 3D properties):
   - Ensure chart is readable at configured `Width` and `Height`
   - Don't rely on 3D effects (`RotationAngle`, `TiltAngle`, `Depth`) to convey critical information
   - Provide alternative data representations
   - Test with screen readers

10. **Testing** (all dimension properties):
    - Test with minimum and maximum data values at configured sizes
    - Verify labels don't overlap with current `Width` and `Height`
    - Check tooltip visibility with `RotationAngle` and `TiltAngle` settings
    - Validate configured properties on target devices and browsers
