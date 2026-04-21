# Tooltip for Blazor 3D Charts

This reference provides comprehensive guidance on configuring tooltips in Syncfusion Blazor 3D Charts, including enabling, formatting, templates, and customization options.

## Table of Contents

- [Overview](#overview)
- [Enable Tooltip](#enable-tooltip)
- [Fixed Tooltip](#fixed-tooltip)
- [Format Tooltip](#format-tooltip)
- [Tooltip Template](#tooltip-template)
- [Customize Appearance](#customize-appearance)
- [Best Practices](#best-practices)

## Overview

Tooltips display information about data points when users hover over them. They provide context and detailed information without cluttering the chart. The `Chart3DTooltipSettings` component controls tooltip behavior using properties like `Enable`, `Format`, `Header`, and templates to customize tooltip appearance and content.

## Enable Tooltip

Enable tooltips by setting the `Enable` property to `true` in the `Chart3DTooltipSettings` component. This activates the tooltip functionality to display data information on hover.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Monthly Sales" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DTooltipSettings Enable="true"></Chart3DTooltipSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData" XName="Month" YName="Sales" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class MonthlySales
    {
        public string Month { get; set; }
        public double Sales { get; set; }
    }

    public List<MonthlySales> SalesData = new List<MonthlySales>
    {
        new MonthlySales { Month = "Jan", Sales = 35 },
        new MonthlySales { Month = "Feb", Sales = 28 },
        new MonthlySales { Month = "Mar", Sales = 34 },
        new MonthlySales { Month = "Apr", Sales = 32 },
        new MonthlySales { Month = "May", Sales = 40 },
        new MonthlySales { Month = "Jun", Sales = 32 }
    };
}
```

## Fixed Tooltip

Set tooltip in fixed position using the `Chart3DTooltipLocation` component with `X` and `Y` properties to define static tooltip coordinates on the chart canvas.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Product Performance" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DTooltipSettings Enable="true">
        <Chart3DTooltipLocation X="150" Y="50"></Chart3DTooltipLocation>
    </Chart3DTooltipSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ProductData" XName="Product" YName="Units" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ProductInfo
    {
        public string Product { get; set; }
        public double Units { get; set; }
    }

    public List<ProductInfo> ProductData = new List<ProductInfo>
    {
        new ProductInfo { Product = "Laptop", Units = 450 },
        new ProductInfo { Product = "Phone", Units = 620 },
        new ProductInfo { Product = "Tablet", Units = 380 },
        new ProductInfo { Product = "Monitor", Units = 290 }
    };
}
```

## Format Tooltip

Format tooltip content using the `Format` property in `Chart3DTooltipSettings` with dynamic placeholders like `${point.x}`, `${point.y}`, and `${series.name}`. The `Header` property can also be used to add a title to the tooltip.

### Basic Format

Use the `Format` property with HTML and placeholder syntax to control tooltip content display.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Revenue Analysis" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DTooltipSettings Enable="true" 
                            Format="<b>${series.name}</b><br>Value: ${point.y}">
    </Chart3DTooltipSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@RevenueData" Name="Revenue" XName="Quarter" 
                       YName="Amount" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class QuarterRevenue
    {
        public string Quarter { get; set; }
        public double Amount { get; set; }
    }

    public List<QuarterRevenue> RevenueData = new List<QuarterRevenue>
    {
        new QuarterRevenue { Quarter = "Q1", Amount = 125000 },
        new QuarterRevenue { Quarter = "Q2", Amount = 145000 },
        new QuarterRevenue { Quarter = "Q3", Amount = 165000 },
        new QuarterRevenue { Quarter = "Q4", Amount = 185000 }
    };
}
```

### Format with Header

Add a title to the tooltip using the `Header` property along with the `Format` property for complete tooltip customization.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Report" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DTooltipSettings Enable="true" 
                            Header="Sales Data" 
                            Format="<b>${point.x}</b>: ${point.y}">
    </Chart3DTooltipSettings>
    
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

### Multi-Series Format

Display multiple series data in a single tooltip using the `Format` property with `${series.name}` and `${point.y}` placeholders to show series-specific values.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Comparative Analysis" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DTooltipSettings Enable="true" 
                            Format="<b>${point.x}</b><br>${series.name}: ${point.y}">
    </Chart3DTooltipSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ComparisonList" Name="Product A" XName="Month" 
                       YName="ValueA" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@ComparisonList" Name="Product B" XName="Month" 
                       YName="ValueB" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ComparisonData
    {
        public string Month { get; set; }
        public double ValueA { get; set; }
        public double ValueB { get; set; }
    }

    public List<ComparisonData> ComparisonList = new List<ComparisonData>
    {
        new ComparisonData { Month = "Jan", ValueA = 120, ValueB = 95 },
        new ComparisonData { Month = "Feb", ValueA = 145, ValueB = 110 },
        new ComparisonData { Month = "Mar", ValueA = 165, ValueB = 130 }
    };
}
```

## Tooltip Template

Create custom tooltip templates using the `Chart3DTooltipTemplate` component child element within `Chart3DTooltipSettings`. The template context provides `Chart3DTooltipInfo` with `X`, `Y` properties for rendering custom HTML content.

### Basic Template

Create a simple custom template using `Chart3DTooltipTemplate` with basic HTML markup and `Chart3DTooltipInfo` properties like `X` and `Y` for data display.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Custom Tooltip" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DTooltipSettings Enable="true">
        <Chart3DTooltipTemplate>
            @{
                var data = context as Chart3DTooltipInfo;
            }
            <div style="background-color: #0066CC; color: white; padding: 10px; border-radius: 5px;">
                <b>@data.X</b><br/>
                Value: @data.Y
            </div>
        </Chart3DTooltipTemplate>
    </Chart3DTooltipSettings>
    
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
        new DataPoint { Category = "A", Value = 45 },
        new DataPoint { Category = "B", Value = 62 },
        new DataPoint { Category = "C", Value = 58 },
        new DataPoint { Category = "D", Value = 70 }
    };
}
```

### Advanced Template with Table

Build complex tooltip layouts using `Chart3DTooltipTemplate` with table structures and styled rows to display formatted data from `Chart3DTooltipInfo`.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Detailed Information" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DTooltipSettings Enable="true">
        <Chart3DTooltipTemplate>
            @{
                var data = context as Chart3DTooltipInfo;
            }
            <table style="border: 2px solid #0066CC; background-color: #f0f0f0;">
                <tr style="background-color: #0066CC;">
                    <th colspan="2" style="color: white; padding: 5px;">
                        Performance Data
                    </th>
                </tr>
                <tr>
                    <td style="padding: 5px; font-weight: bold;">Category:</td>
                    <td style="padding: 5px;">@data.X</td>
                </tr>
                <tr>
                    <td style="padding: 5px; font-weight: bold;">Score:</td>
                    <td style="padding: 5px; color: #0066CC;">@data.Y</td>
                </tr>
            </table>
        </Chart3DTooltipTemplate>
    </Chart3DTooltipSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@PerformanceData" XName="Team" YName="Score" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

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
        new TeamPerformance { Team = "Team C", Score = 95 }
    };
}
```

### Template with Conditional Formatting

Apply dynamic styling in `Chart3DTooltipTemplate` by using conditional C# logic within the template context to change colors and status based on `Chart3DTooltipInfo` values.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Status Indicator" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DTooltipSettings Enable="true">
        <Chart3DTooltipTemplate>
            @{
                var data = context as Chart3DTooltipInfo;
                double value = Convert.ToDouble(data.Y);
                var status = value >= 80 ? "Excellent" : value >= 60 ? "Good" : "Needs Improvement";
                var color = value >= 80 ? "#28a745" : value >= 60 ? "#ffc107" : "#dc3545";
            }
            <div style="background-color: @color; color: white; padding: 10px; border-radius: 5px;">
                <b>@data.X</b><br/>
                Score: @data.Y<br/>
                Status: <b>@status</b>
            </div>
        </Chart3DTooltipTemplate>
    </Chart3DTooltipSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@StatusData" XName="Department" YName="Rating" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class DepartmentStatus
    {
        public string Department { get; set; }
        public double Rating { get; set; }
    }

    public List<DepartmentStatus> StatusData = new List<DepartmentStatus>
    {
        new DepartmentStatus { Department = "Sales", Rating = 85 },
        new DepartmentStatus { Department = "Marketing", Rating = 72 },
        new DepartmentStatus { Department = "Support", Rating = 55 }
    };
}
```

## Customize Appearance

Customize tooltip appearance using properties like `Fill` (background color) and child components `Chart3DTooltipBorder` (with `Color` and `Width` properties) and `Chart3DTooltipTextStyle` (with `Color`, `FontSize`, `FontWeight` properties) in `Chart3DTooltipSettings`.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Styled Tooltip" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DTooltipSettings Enable="true" 
                            Fill="#FFE5B4" 
                            Format="<b>${point.x}</b>: ${point.y}">
        <Chart3DTooltipBorder Color="#FF6347" Width="2"></Chart3DTooltipBorder>
        <Chart3DTooltipTextStyle Color="#8B4513" FontSize="14px" FontWeight="bold">
        </Chart3DTooltipTextStyle>
    </Chart3DTooltipSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@StyledData" XName="Item" YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class StyledDataPoint
    {
        public string Item { get; set; }
        public double Value { get; set; }
    }

    public List<StyledDataPoint> StyledData = new List<StyledDataPoint>
    {
        new StyledDataPoint { Item = "Item 1", Value = 45 },
        new StyledDataPoint { Item = "Item 2", Value = 62 },
        new StyledDataPoint { Item = "Item 3", Value = 58 }
    };
}
```

### Opacity and Shadow

Enhance tooltip visual effects using the `Opacity` property in `Chart3DTooltipSettings` to control transparency levels along with `Fill`, `Chart3DTooltipBorder`, and `Chart3DTooltipTextStyle` for complete styling control.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Enhanced Tooltip" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DTooltipSettings Enable="true" 
                            Fill="#0066CC" 
                            Opacity="0.9"
                            Format="<b>${point.x}</b><br/>Value: ${point.y}">
        <Chart3DTooltipBorder Color="#FFFFFF" Width="1"></Chart3DTooltipBorder>
        <Chart3DTooltipTextStyle Color="white" FontSize="12px">
        </Chart3DTooltipTextStyle>
    </Chart3DTooltipSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@EnhancedData" XName="Category" YName="Amount" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class EnhancedDataPoint
    {
        public string Category { get; set; }
        public double Amount { get; set; }
    }

    public List<EnhancedDataPoint> EnhancedData = new List<EnhancedDataPoint>
    {
        new EnhancedDataPoint { Category = "Product A", Amount = 125 },
        new EnhancedDataPoint { Category = "Product B", Amount = 145 },
        new EnhancedDataPoint { Category = "Product C", Amount = 165 }
    };
}
```

## Best Practices

1. **Enable Appropriately**:
   - Always enable tooltips for interactive charts
   - Provide additional context beyond what's visible
   - Essential for charts with many data points

2. **Format Strings**:
   - Use clear, concise formats
   - Include series name for multi-series charts
   - Format numbers appropriately (currency, percentage)
   - Use HTML tags for formatting (bold, line breaks)

3. **Fixed Position**:
   - Use sparingly, mainly for demos or specific layouts
   - Default tracking behavior is usually better for UX
   - Ensure fixed position doesn't overlap important data

4. **Templates**:
   - Use templates for complex information
   - Keep template HTML simple and lightweight
   - Test with various data values
   - Ensure templates are responsive

5. **Appearance Customization**:
   - Match tooltip style with chart theme
   - Use high contrast colors for readability
   - Keep font sizes legible (minimum 12px)
   - Use borders to define tooltip boundaries

6. **Content Guidelines**:
   - Show relevant data (X value, Y value, series name)
   - Include units of measurement
   - Use consistent formatting across tooltips
   - Avoid displaying redundant information

7. **Performance**:
   - Keep templates simple for better performance
   - Avoid complex calculations in templates
   - Use format strings instead of templates when possible
   - Test with large datasets

8. **Accessibility**:
   - Ensure tooltip text is readable
   - Use sufficient color contrast
   - Don't rely solely on tooltips for critical information
   - Consider keyboard users (tooltips may not be accessible)

9. **Multi-Series Charts**:
   - Always include series name in tooltip
   - Use consistent format across all series
   - Consider showing comparative information

10. **Testing**:
    - Test tooltips with various data ranges
    - Verify tooltips don't overflow chart boundaries
    - Check tooltip positioning at chart edges
    - Test on different screen sizes
