# Appearance Customization for Blazor 3D Charts

This reference provides comprehensive guidance on customizing the appearance of Syncfusion Blazor 3D Charts, including colors, backgrounds, animations, and styling options.

## Table of Contents

- [Overview](#overview)
- [Custom Color Palette](#custom-color-palette)
- [Data Point Customization](#data-point-customization)
- [Chart Area Customization](#chart-area-customization)
- [Animation](#animation)
- [Chart Rotation](#chart-rotation)
- [Title Customization](#title-customization)
- [Best Practices](#best-practices)

## Overview

Appearance customization allows you to match charts with your application's theme, improve readability, and enhance user experience through visual design.

## Custom Color Palette

Define custom colors for series using the `Palettes` property. The `Palettes` property accepts an array of color strings to customize the series colors.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Olympic Medals" Palettes="@CustomColors" WallColor="transparent" 
           EnableRotation="true" RotationAngle="7" TiltAngle="10" Depth="100">
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
</SfChart3D>

@code {
    public string[] CustomColors = new string[] { "#FFD700", "#C0C0C0", "#CD7F32" };

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

## Data Point Customization

### Point Color Mapping

Map colors from data source using the `PointColor` property. The `PointColor` property maps a data field containing color values to apply individual colors to each data point.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Temperature Variation" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@TempData" XName="Month" YName="Temp" 
                       PointColor="Color" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class TemperatureData
    {
        public string Month { get; set; }
        public double Temp { get; set; }
        public string Color { get; set; }
    }

    public List<TemperatureData> TempData = new List<TemperatureData>
    {
        new TemperatureData { Month = "Jan", Temp = 6.96, Color = "#4169E1" },
        new TemperatureData { Month = "Feb", Temp = 8.9, Color = "#1E90FF" },
        new TemperatureData { Month = "Mar", Temp = 12, Color = "#00BFFF" },
        new TemperatureData { Month = "Apr", Temp = 17.5, Color = "#87CEEB" },
        new TemperatureData { Month = "May", Temp = 22.1, Color = "#FFD700" }
    };
}
```

### Point Level Customization with Events

Use the `PointRendering` event to customize individual data points dynamically. The `PointRendering` event provides `Chart3DPointRenderEventArgs` with properties like `Fill` and `Point` for color and data manipulation.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Performance Analysis" PointRendering="OnPointRender" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@PerformanceData" XName="Department" YName="Score" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public string[] GradientColors = new string[] 
    { 
        "#FF6B6B", "#4ECDC4", "#45B7D1", "#FFA07A", "#98D8C8", 
        "#F7DC6F", "#BB8FCE", "#85C1E2", "#F8B88B", "#52B788" 
    };

    public class DepartmentPerformance
    {
        public string Department { get; set; }
        public double Score { get; set; }
    }

    public List<DepartmentPerformance> PerformanceData = new List<DepartmentPerformance>
    {
        new DepartmentPerformance { Department = "Sales", Score = 85 },
        new DepartmentPerformance { Department = "Marketing", Score = 72 },
        new DepartmentPerformance { Department = "IT", Score = 95 },
        new DepartmentPerformance { Department = "HR", Score = 68 }
    };

    public void OnPointRender(Chart3DPointRenderEventArgs args)
    {
        args.Fill = GradientColors[args.Point.Index % GradientColors.Length];
    }
}
```

### Combined Point and Label Customization

Combine point and label customization using the `PointRendering` and `DataLabelRendering` events. The `PointRendering` event (with `Chart3DPointRenderEventArgs`) customizes point appearance, while the `DataLabelRendering` event (with `Chart3DTextRenderEventArgs`) customizes label text and styling.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Highlighted Data" PointRendering="CustomizePoint" 
           DataLabelRendering="CustomizeLabel"
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData" XName="Product" YName="Sales" 
                       Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true"></Chart3DDataLabel>
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
        new ProductSales { Product = "Phone", Sales = 180 },
        new ProductSales { Product = "Tablet", Sales = 95 },
        new ProductSales { Product = "Monitor", Sales = 140 }
    };

    public void CustomizePoint(Chart3DPointRenderEventArgs args)
    {
        // Highlight the maximum value
        if (args.Point.Y is double y && y == 180)
        {
            args.Fill = "#28a745";
        }
    }

    public void CustomizeLabel(Chart3DTextRenderEventArgs args)
    {
        // Add special marker for highest value
        if (args.Point.Y is double y && y == 180)
        {
            args.Text = "★ " + args.Text;
        }
    }
}
```

## Chart Area Customization

### Background Color and Border

Customize the chart background and border using the `BackgroundColor`, `WallColor` properties, and the `Chart3DBorder` component. The `Chart3DBorder` component has `Color` and `Width` properties to define border appearance.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Dashboard" BackgroundColor="#F0F8FF" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DBorder Color="#4169E1" Width="2"></Chart3DBorder>
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@DashboardData" XName="Category" YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class Dashboard
    {
        public string Category { get; set; }
        public double Value { get; set; }
    }

    public List<Dashboard> DashboardData = new List<Dashboard>
    {
        new Dashboard { Category = "Q1", Value = 120 },
        new Dashboard { Category = "Q2", Value = 145 },
        new Dashboard { Category = "Q3", Value = 165 }
    };
}
```

### Chart Margin

Set chart margins using the `Chart3DMargin` component with `Left`, `Right`, `Top`, and `Bottom` properties to define spacing around the chart area.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Spaced Chart" BackgroundColor="#FFFACD" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DBorder Color="#FF6347" Width="3"></Chart3DBorder>
    <Chart3DMargin Left="80" Right="80" Top="80" Bottom="80"></Chart3DMargin>
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MarginData" XName="Item" YName="Count" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class MarginDataPoint
    {
        public string Item { get; set; }
        public double Count { get; set; }
    }

    public List<MarginDataPoint> MarginData = new List<MarginDataPoint>
    {
        new MarginDataPoint { Item = "A", Count = 50 },
        new MarginDataPoint { Item = "B", Count = 70 },
        new MarginDataPoint { Item = "C", Count = 60 }
    };
}
```

## Animation

Customize series animation using the `Chart3DAnimation` component with properties like `Enable` (boolean to enable/disable animation), `Duration` (animation duration in milliseconds), and `Delay` (delay before animation starts in milliseconds):

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Animated Chart" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@AnimatedData" Name="Sales" XName="Month" 
                       YName="Amount" Type="Chart3DSeriesType.Column">
            <Chart3DAnimation Enable="true" Duration="2000" Delay="300"></Chart3DAnimation>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class MonthlyData
    {
        public string Month { get; set; }
        public double Amount { get; set; }
    }

    public List<MonthlyData> AnimatedData = new List<MonthlyData>
    {
        new MonthlyData { Month = "Jan", Amount = 120 },
        new MonthlyData { Month = "Feb", Amount = 145 },
        new MonthlyData { Month = "Mar", Amount = 165 },
        new MonthlyData { Month = "Apr", Amount = 185 }
    };
}
```

### Multiple Series with Staggered Animation

Apply staggered animations to multiple series by setting different `Delay` values in the `Chart3DAnimation` component for each series. Use the `Duration` property to control animation speed and `Enable` property to toggle animation on/off.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Staggered Animation" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SeriesData" Name="Series 1" XName="X" 
                       YName="Y1" Type="Chart3DSeriesType.Column">
            <Chart3DAnimation Enable="true" Duration="1500" Delay="0"></Chart3DAnimation>
        </Chart3DSeries>
        <Chart3DSeries DataSource="@SeriesData" Name="Series 2" XName="X" 
                       YName="Y2" Type="Chart3DSeriesType.Column">
            <Chart3DAnimation Enable="true" Duration="1500" Delay="500"></Chart3DAnimation>
        </Chart3DSeries>
        <Chart3DSeries DataSource="@SeriesData" Name="Series 3" XName="X" 
                       YName="Y3" Type="Chart3DSeriesType.Column">
            <Chart3DAnimation Enable="true" Duration="1500" Delay="1000"></Chart3DAnimation>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class MultiSeriesData
    {
        public string X { get; set; }
        public double Y1 { get; set; }
        public double Y2 { get; set; }
        public double Y3 { get; set; }
    }

    public List<MultiSeriesData> SeriesData = new List<MultiSeriesData>
    {
        new MultiSeriesData { X = "A", Y1 = 50, Y2 = 60, Y3 = 70 },
        new MultiSeriesData { X = "B", Y1 = 40, Y2 = 55, Y3 = 65 }
    };
}
```

## Chart Rotation

Enable interactive 3D rotation using the `EnableRotation` property (boolean). Control the rotation perspective using `RotationAngle` (horizontal rotation in degrees) and `TiltAngle` (vertical tilt in degrees). The `Depth` property sets the depth perspective of the 3D view:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Rotatable 3D Chart" WallColor="transparent" 
           EnableRotation="true" RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@RotationData" XName="Category" YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class RotationDataPoint
    {
        public string Category { get; set; }
        public double Value { get; set; }
    }

    public List<RotationDataPoint> RotationData = new List<RotationDataPoint>
    {
        new RotationDataPoint { Category = "Item 1", Value = 85 },
        new RotationDataPoint { Category = "Item 2", Value = 92 },
        new RotationDataPoint { Category = "Item 3", Value = 78 }
    };
}
```

## Title Customization

### Title Style

Customize title appearance using the `Chart3DTitleStyle` component with properties like `FontSize`, `Color`, `FontFamily`, `FontWeight`, and `FontStyle` to control typography and styling.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Styled Title Chart" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DTitleStyle FontSize="24px" Color="#FF6347" FontFamily="Arial" 
                       FontWeight="bold" FontStyle="italic">
    </Chart3DTitleStyle>
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@TitleData" XName="Label" YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class TitleDataPoint
    {
        public string Label { get; set; }
        public double Value { get; set; }
    }

    public List<TitleDataPoint> TitleData = new List<TitleDataPoint>
    {
        new TitleDataPoint { Label = "A", Value = 50 },
        new TitleDataPoint { Label = "B", Value = 70 }
    };
}
```

### Title Position

Control title placement using the `Position` property in `Chart3DTitleStyle` component. Use predefined positions like `Chart3DTitlePosition.Top`, `Bottom`, `Left`, or `Right` to reposition the title.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Bottom Title" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DTitleStyle Position="Chart3DTitlePosition.Bottom">
    </Chart3DTitleStyle>
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@PositionData" XName="X" YName="Y" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class PositionDataPoint
    {
        public string X { get; set; }
        public double Y { get; set; }
    }

    public List<PositionDataPoint> PositionData = new List<PositionDataPoint>
    {
        new PositionDataPoint { X = "Item", Y = 65 }
    };
}
```

### Custom Title Position

Position the title at a specific location using the `Position` property set to `Chart3DTitlePosition.Custom` and use `X` and `Y` properties in `Chart3DTitleStyle` component to define pixel coordinates.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Custom Position Title" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DTitleStyle Position="Chart3DTitlePosition.Custom" X="300" Y="60">
    </Chart3DTitleStyle>
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@CustomData" XName="Label" YName="Count" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class CustomDataPoint
    {
        public string Label { get; set; }
        public double Count { get; set; }
    }

    public List<CustomDataPoint> CustomData = new List<CustomDataPoint>
    {
        new CustomDataPoint { Label = "Data", Count = 80 }
    };
}
```

### Title Alignment

Align the title horizontally using the `TextAlignment` property in `Chart3DTitleStyle` component. Use alignment values like `Alignment.Near` (left), `Center`, or `Far` (right) to position the text.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Aligned Title" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DTitleStyle TextAlignment="Syncfusion.Blazor.Chart3D.Alignment.Far">
    </Chart3DTitleStyle>
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@AlignData" XName="X" YName="Y" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class AlignDataPoint
    {
        public string X { get; set; }
        public double Y { get; set; }
    }

    public List<AlignDataPoint> AlignData = new List<AlignDataPoint>
    {
        new AlignDataPoint { X = "Value", Y = 75 }
    };
}
```

## Best Practices

1. **Color Palette Selection**:
   - Choose colors that provide good contrast
   - Use consistent color schemes across charts
   - Consider colorblind-friendly palettes
   - Limit palette to 5-7 colors for clarity

2. **Point Customization**:
   - Use point colors to highlight important data
   - Maintain consistency in color meanings
   - Test color combinations for visibility

3. **Background and Borders**:
   - Use subtle backgrounds that don't distract
   - Ensure borders enhance, not overwhelm
   - Match chart style with application theme

4. **Animation**:
   - Keep animations smooth (1000-2000ms duration)
   - Use delays for staggered effects
   - Don't overuse animations
   - Test performance with large datasets

5. **Rotation**:
   - Enable for better 3D perspective
   - Set appropriate initial angles (7-15° rotation, 10-20° tilt)
   - Test rotation on touch devices

6. **Title Styling**:
   - Use readable font sizes (18-24px)
   - Maintain sufficient contrast
   - Keep titles concise
   - Align appropriately for layout

7. **Margin and Spacing**:
   - Provide adequate breathing room
   - Balance margins with chart size
   - Consider responsive layouts

8. **Performance Considerations**:
   - Limit complex animations for large datasets
   - Optimize event handlers
   - Test rendering performance

9. **Accessibility**:
   - Ensure color contrast meets WCAG standards
   - Don't rely solely on color to convey information
   - Provide alternative text for charts

10. **Consistency**:
    - Maintain consistent styling across application
    - Use design tokens/variables when possible
    - Document custom color palettes
