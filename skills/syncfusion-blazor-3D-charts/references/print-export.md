# Print and Export for Blazor 3D Charts

This reference provides comprehensive guidance on printing and exporting Syncfusion Blazor 3D Charts to various formats including JPEG, PNG, SVG, and PDF.

## Table of Contents

- [Overview](#overview)
- [Print Chart](#print-chart)
- [Export to Image](#export-to-image)
- [Export to PDF](#export-to-pdf)
- [Export to SVG](#export-to-svg)
- [Button Integration](#button-integration)
- [Best Practices](#best-practices)

## Overview

Print and export features allow users to save chart visualizations for reports, presentations, and offline analysis. Syncfusion 3D Charts support printing and exporting to multiple formats.

## Print Chart

Print charts using the `PrintAsync()` method of the `SfChart3D` component. The `PrintAsync()` method prints the entire chart with its current visual state including all configured properties like `Title`, `WallColor`, `EnableRotation`, `RotationAngle`, `TiltAngle`, and `Depth`.

### Basic Print

The `PrintAsync()` method prints the chart synchronously by calling `await Chart.PrintAsync()` where `Chart` is the reference variable (using `@ref="Chart"`) to the `SfChart3D` component instance.

```cshtml
@using Syncfusion.Blazor.Chart3D
@using Syncfusion.Blazor.Buttons

<SfButton Content="Print Chart" OnClick="PrintChart"></SfButton>

<SfChart3D @ref="Chart" Title="Sales Report" WallColor="transparent" 
           EnableRotation="true" RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData" XName="Month" YName="Sales" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    SfChart3D Chart;

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

    public async Task PrintChart()
    {
        await Chart.PrintAsync();
    }
}
```

### Print with Multiple Series

Print charts with multiple series using the `PrintAsync()` method. Multiple series are defined using `Chart3DSeriesCollection` containing multiple `Chart3DSeries` components, each with properties like `DataSource`, `Name`, `XName`, `YName`, and `Type`. All series are included in the print output along with the `Chart3DLegendSettings` configuration.

```cshtml
@using Syncfusion.Blazor.Chart3D
@using Syncfusion.Blazor.Buttons

<SfButton Content="Print" IsPrimary="true" OnClick="Print"></SfButton>

<SfChart3D @ref="ChartObj" Title="Quarterly Performance" WallColor="transparent" 
           EnableRotation="true" RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@PerformanceData" Name="Product A" XName="Quarter" 
                       YName="ValueA" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@PerformanceData" Name="Product B" XName="Quarter" 
                       YName="ValueB" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@PerformanceData" Name="Product C" XName="Quarter" 
                       YName="ValueC" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    SfChart3D ChartObj;

    public class QuarterlyData
    {
        public string Quarter { get; set; }
        public double ValueA { get; set; }
        public double ValueB { get; set; }
        public double ValueC { get; set; }
    }

    public List<QuarterlyData> PerformanceData = new List<QuarterlyData>
    {
        new QuarterlyData { Quarter = "Q1", ValueA = 85, ValueB = 92, ValueC = 78 },
        new QuarterlyData { Quarter = "Q2", ValueA = 90, ValueB = 88, ValueC = 82 },
        new QuarterlyData { Quarter = "Q3", ValueA = 88, ValueB = 95, ValueC = 85 },
        new QuarterlyData { Quarter = "Q4", ValueA = 95, ValueB = 90, ValueC = 88 }
    };

    public async Task Print()
    {
        await ChartObj.PrintAsync();
    }
}
```

## Export to Image

Export charts to image formats (JPEG, PNG) using the `ExportAsync(ExportType, fileName)` method. The `ExportType` parameter accepts enum values `ExportType.PNG` or `ExportType.JPEG`, and the `fileName` parameter specifies the name of the exported file without the file extension.

### Export to PNG

Export the chart to PNG format using `ExportAsync(Syncfusion.Blazor.Chart3D.ExportType.PNG, "fileName")`. The PNG format supports transparency and is suitable for charts requiring transparent backgrounds. The method accepts the `ExportType.PNG` parameter and a custom file name.

```cshtml
@using Syncfusion.Blazor.Chart3D
@using Syncfusion.Blazor.Buttons

<SfButton Content="Export as PNG" OnClick="ExportPNG"></SfButton>

<SfChart3D @ref="ChartInstance" Title="Revenue Analysis" WallColor="transparent" 
           EnableRotation="true" RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@RevenueData" XName="Category" YName="Revenue" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    SfChart3D ChartInstance;

    public class RevenueInfo
    {
        public string Category { get; set; }
        public double Revenue { get; set; }
    }

    public List<RevenueInfo> RevenueData = new List<RevenueInfo>
    {
        new RevenueInfo { Category = "Product 1", Revenue = 120000 },
        new RevenueInfo { Category = "Product 2", Revenue = 145000 },
        new RevenueInfo { Category = "Product 3", Revenue = 165000 },
        new RevenueInfo { Category = "Product 4", Revenue = 185000 }
    };

    public async Task ExportPNG()
    {
        await ChartInstance.ExportAsync(Syncfusion.Blazor.Chart3D.ExportType.PNG, "chart");
    }
}
```

### Export to JPEG

Export the chart to JPEG format using `ExportAsync(Syncfusion.Blazor.Chart3D.ExportType.JPEG, "fileName")`. The JPEG format compresses images without transparency, resulting in smaller file sizes. The method uses `ExportType.JPEG` parameter with a specified file name.

```cshtml
@using Syncfusion.Blazor.Chart3D
@using Syncfusion.Blazor.Buttons

<SfButton Content="Export as JPEG" OnClick="ExportJPEG"></SfButton>

<SfChart3D @ref="Chart3D" Title="Sales Dashboard" WallColor="transparent" 
           EnableRotation="true" RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@DashboardData" XName="Region" YName="Sales" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    SfChart3D Chart3D;

    public class RegionalSales
    {
        public string Region { get; set; }
        public double Sales { get; set; }
    }

    public List<RegionalSales> DashboardData = new List<RegionalSales>
    {
        new RegionalSales { Region = "North", Sales = 120000 },
        new RegionalSales { Region = "South", Sales = 145000 },
        new RegionalSales { Region = "East", Sales = 165000 },
        new RegionalSales { Region = "West", Sales = 135000 }
    };

    public async Task ExportJPEG()
    {
        await Chart3D.ExportAsync(Syncfusion.Blazor.Chart3D.ExportType.JPEG, "sales-dashboard");
    }
}
```

### Custom File Name

Custom file names can be generated dynamically using C# string formatting and date properties. The `fileName` parameter in `ExportAsync(ExportType, fileName)` can include date/time values using `DateTime.Now` formatting patterns like `DateTime.Now:yyyyMMdd`. File extensions are automatically appended based on the `ExportType`.

```cshtml
@using Syncfusion.Blazor.Chart3D
@using Syncfusion.Blazor.Buttons

<SfButton Content="Export Report" OnClick="ExportWithCustomName"></SfButton>

<SfChart3D @ref="ReportChart" Title="Monthly Report" WallColor="transparent" 
           EnableRotation="true" RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MonthlyData" XName="Month" YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    SfChart3D ReportChart;

    public class MonthlyReport
    {
        public string Month { get; set; }
        public double Value { get; set; }
    }

    public List<MonthlyReport> MonthlyData = new List<MonthlyReport>
    {
        new MonthlyReport { Month = "Jan", Value = 85 },
        new MonthlyReport { Month = "Feb", Value = 92 },
        new MonthlyReport { Month = "Mar", Value = 78 }
    };

    public async Task ExportWithCustomName()
    {
        string fileName = $"Monthly_Report_{DateTime.Now:yyyyMMdd}";
        await ReportChart.ExportAsync(Syncfusion.Blazor.Chart3D.ExportType.PNG, fileName);
    }
}
```

## Export to PDF

Export charts to PDF format using `ExportAsync(Syncfusion.Blazor.Chart3D.ExportType.PDF, "fileName")`. The PDF export is suitable for reports and document sharing. The `ExportType.PDF` parameter is used along with a file name to generate portable PDF documents with preserved formatting and properties like `Chart3DLegendSettings`.

```cshtml
@using Syncfusion.Blazor.Chart3D
@using Syncfusion.Blazor.Buttons

<SfButton Content="Export to PDF" OnClick="ExportPDF"></SfButton>

<SfChart3D @ref="PDFChart" Title="Annual Report" WallColor="transparent" 
           EnableRotation="true" RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@AnnualList" Name="Revenue" XName="Year" 
                       YName="Amount" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    SfChart3D PDFChart;

    public class AnnualData
    {
        public string Year { get; set; }
        public double Amount { get; set; }
    }

    public List<AnnualData> AnnualList = new List<AnnualData>
    {
        new AnnualData { Year = "2020", Amount = 125000 },
        new AnnualData { Year = "2021", Amount = 145000 },
        new AnnualData { Year = "2022", Amount = 165000 },
        new AnnualData { Year = "2023", Amount = 185000 }
    };

    public async Task ExportPDF()
    {
        await PDFChart.ExportAsync(Syncfusion.Blazor.Chart3D.ExportType.PDF, "annual-report");
    }
}
```

## Export to SVG

Export charts to SVG format for scalable vector graphics using `ExportAsync(Syncfusion.Blazor.Chart3D.ExportType.SVG, "fileName")`. SVG exports are ideal for presentations and web usage as they scale without quality loss. The `ExportType.SVG` parameter produces vector-based graphics that preserve all chart elements including legends, tooltips, and data labels.

```cshtml
@using Syncfusion.Blazor.Chart3D
@using Syncfusion.Blazor.Buttons

<SfButton Content="Export as SVG" OnClick="ExportSVG"></SfButton>

<SfChart3D @ref="SVGChart" Title="Vector Export Example" WallColor="transparent" 
           EnableRotation="true" RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@VectorData" XName="Item" YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    SfChart3D SVGChart;

    public class VectorDataPoint
    {
        public string Item { get; set; }
        public double Value { get; set; }
    }

    public List<VectorDataPoint> VectorData = new List<VectorDataPoint>
    {
        new VectorDataPoint { Item = "A", Value = 45 },
        new VectorDataPoint { Item = "B", Value = 62 },
        new VectorDataPoint { Item = "C", Value = 58 },
        new VectorDataPoint { Item = "D", Value = 70 }
    };

    public async Task ExportSVG()
    {
        await SVGChart.ExportAsync(Syncfusion.Blazor.Chart3D.ExportType.SVG, "vector-chart");
    }
}
```

## Button Integration

Create export toolbar with multiple format options using `SfButton` components with `OnClick` event handlers. Each button can call `ExportAsync()` or `PrintAsync()` methods with different parameters. The `SfButton` component properties like `Content`, `IconCss`, and `CssClass` are used to customize button appearance.

### Export Toolbar

The export toolbar uses multiple `SfButton` components with event handlers. Each button calls the `ExportChart(string type)` method which switches between export types: `Print`, `PNG`, `JPEG`, `SVG`, and `PDF`. The `Content` property displays button text, and `IconCss` adds visual icons using Syncfusion icon classes.

```cshtml
@using Syncfusion.Blazor.Chart3D
@using Syncfusion.Blazor.Buttons

<div style="margin-bottom: 10px;">
    <SfButton Content="Print" IconCss="e-icons e-print" OnClick="@(() => ExportChart("Print"))"></SfButton>
    <SfButton Content="PNG" IconCss="e-icons e-image" OnClick="@(() => ExportChart("PNG"))"></SfButton>
    <SfButton Content="JPEG" IconCss="e-icons e-image" OnClick="@(() => ExportChart("JPEG"))"></SfButton>
    <SfButton Content="SVG" IconCss="e-icons e-chart" OnClick="@(() => ExportChart("SVG"))"></SfButton>
    <SfButton Content="PDF" IconCss="e-icons e-pdf" OnClick="@(() => ExportChart("PDF"))"></SfButton>
</div>

<SfChart3D @ref="ChartRef" Title="Product Performance" WallColor="transparent" 
           EnableRotation="true" RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>
    <Chart3DTooltipSettings Enable="true"></Chart3DTooltipSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ProductData" Name="Sales" XName="Product" 
                       YName="Sales" Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true"></Chart3DDataLabel>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    SfChart3D ChartRef;

    public class ProductPerformance
    {
        public string Product { get; set; }
        public double Sales { get; set; }
    }

    public List<ProductPerformance> ProductData = new List<ProductPerformance>
    {
        new ProductPerformance { Product = "Laptop", Sales = 450 },
        new ProductPerformance { Product = "Phone", Sales = 620 },
        new ProductPerformance { Product = "Tablet", Sales = 380 },
        new ProductPerformance { Product = "Monitor", Sales = 290 }
    };

    public async Task ExportChart(string type)
    {
        string fileName = $"product-performance-{DateTime.Now:yyyyMMdd}";
        
        switch (type)
        {
            case "Print":
                await ChartRef.PrintAsync();
                break;
            case "PNG":
                await ChartRef.ExportAsync(Syncfusion.Blazor.Chart3D.ExportType.PNG, fileName);
                break;
            case "JPEG":
                await ChartRef.ExportAsync(Syncfusion.Blazor.Chart3D.ExportType.JPEG, fileName);
                break;
            case "SVG":
                await ChartRef.ExportAsync(Syncfusion.Blazor.Chart3D.ExportType.SVG, fileName);
                break;
            case "PDF":
                await ChartRef.ExportAsync(Syncfusion.Blazor.Chart3D.ExportType.PDF, fileName);
                break;
        }
    }
}
```

### Styled Export Menu

A styled export menu uses `SfButton` components with the `CssClass` property set to color variants like `e-success`, `e-info`, and `e-warning`. The menu includes `Chart3DLegendSettings` and multiple `Chart3DSeries` components. Methods like `PrintChartAsync()`, `ExportPNGAsync()`, and `ExportPDFAsync()` are called via the `OnClick` event handler.

```cshtml
@using Syncfusion.Blazor.Chart3D
@using Syncfusion.Blazor.Buttons

<div style="display: flex; gap: 10px; margin-bottom: 15px; padding: 10px; background: #f5f5f5; border-radius: 5px;">
    <SfButton Content="Print Chart" CssClass="e-success" OnClick="PrintChartAsync"></SfButton>
    <SfButton Content="Export PNG" CssClass="e-info" OnClick="ExportPNGAsync"></SfButton>
    <SfButton Content="Export PDF" CssClass="e-warning" OnClick="ExportPDFAsync"></SfButton>
</div>

<SfChart3D @ref="StyledChart" Title="Comprehensive Report" Width="900px" Height="500px" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ComprehensiveData" Name="Q1" XName="Category" 
                       YName="Q1" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@ComprehensiveData" Name="Q2" XName="Category" 
                       YName="Q2" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@ComprehensiveData" Name="Q3" XName="Category" 
                       YName="Q3" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    SfChart3D StyledChart;

    public class ComprehensiveReport
    {
        public string Category { get; set; }
        public double Q1 { get; set; }
        public double Q2 { get; set; }
        public double Q3 { get; set; }
    }

    public List<ComprehensiveReport> ComprehensiveData = new List<ComprehensiveReport>
    {
        new ComprehensiveReport { Category = "Sales", Q1 = 85, Q2 = 92, Q3 = 88 },
        new ComprehensiveReport { Category = "Marketing", Q1 = 72, Q2 = 78, Q3 = 82 },
        new ComprehensiveReport { Category = "Support", Q1 = 68, Q2 = 75, Q3 = 80 }
    };

    public async Task PrintChartAsync()
    {
        await StyledChart.PrintAsync();
    }

    public async Task ExportPNGAsync()
    {
        await StyledChart.ExportAsync(Syncfusion.Blazor.Chart3D.ExportType.PNG, "comprehensive-report");
    }

    public async Task ExportPDFAsync()
    {
        await StyledChart.ExportAsync(Syncfusion.Blazor.Chart3D.ExportType.PDF, "comprehensive-report");
    }
}
```

## Best Practices

1. **Format Selection**:
   - Use **PNG** for high-quality images with transparency
   - Use **JPEG** for smaller file sizes (no transparency)
   - Use **SVG** for scalable vector graphics (presentations, web)
   - Use **PDF** for reports and document sharing
   - Use **Print** for direct printing

2. **File Naming**:
   - Use descriptive, meaningful file names
   - Include dates for reports: `report_20240323`
   - Avoid special characters and spaces
   - Use lowercase with hyphens: `sales-report-q1`

3. **Chart Reference**:
   - Always use `@ref` to get chart instance
   - Store reference in field: `SfChart3D ChartObj;`
   - Initialize before calling methods
   - Check for null before async calls

4. **User Experience**:
   - Provide clear button labels
   - Use icons for visual clarity
   - Group related export options
   - Show feedback during export (loading indicator)
   - Handle export errors gracefully

5. **Print Considerations**:
   - Ensure chart fits on standard paper sizes
   - Test print layout before production
   - Consider landscape orientation for wide charts
   - Remove unnecessary UI elements before printing

6. **Export Quality**:
   - Ensure adequate chart size for clarity
   - Test exported files at target resolution
   - Verify colors render correctly
   - Check text readability in exports

7. **Performance**:
   - Export operations are async (use await)
   - Large charts may take time to export
   - Consider showing progress for large exports
   - Test export with maximum data volumes

8. **Accessibility**:
   - Provide keyboard shortcuts for common actions
   - Use ARIA labels on export buttons
   - Ensure exported content is accessible
   - Provide alternative data formats (CSV, Excel)

9. **Security**:
   - Validate file names to prevent path traversal
   - Sanitize user-provided file names
   - Consider server-side export for sensitive data
   - Control access to export functionality

10. **Testing**:
    - Test all export formats
    - Verify exported file quality
    - Test on different browsers
    - Validate print output
    - Test with complex charts (multiple series, legends)
    - Check file downloads work correctly
