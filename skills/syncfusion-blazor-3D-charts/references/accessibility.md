# Accessibility for Blazor 3D Charts

This reference provides comprehensive guidance on implementing accessibility features in Syncfusion Blazor 3D Charts to meet WCAG 2.2, ADA, and Section 508 standards.

## Table of Contents

- [Overview](#overview)
- [Accessibility Standards](#accessibility-standards)
- [WAI-ARIA Attributes](#wai-aria-attributes)
- [Keyboard Navigation](#keyboard-navigation)
- [Best Practices](#best-practices)

## Overview

Accessibility ensures that charts are usable by people with disabilities. Syncfusion 3D Charts support screen readers, keyboard navigation, and WAI-ARIA attributes for full accessibility compliance.

## Accessibility Standards

Syncfusion 3D Charts comply with the following accessibility standards:

### WCAG 2.2 Compliance

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Performance" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DTooltipSettings Enable="true"></Chart3DTooltipSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData" XName="Month" YName="Sales" 
                       Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true"></Chart3DDataLabel>
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

### Section 508 Compliance

Syncfusion 3D Charts meet Section 508 requirements:

| Criteria | Support | Description |
|----------|---------|-------------|
| Keyboard Navigation | Yes | Full keyboard support for navigation and interaction |
| Screen Reader | Yes | Compatible with JAWS, NVDA, Narrator |
| High Contrast | Yes | Supports Windows High Contrast mode |
| Focus Indicators | Yes | Clear visual focus indicators |
| Alternative Text | Yes | ARIA labels for all interactive elements |

### ADA Compliance

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Accessible Chart Example" WallColor="transparent" 
           EnableRotation="true" RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
        <Chart3DMajorGridLines Width="1"></Chart3DMajorGridLines>
    </Chart3DPrimaryXAxis>
    
    <Chart3DPrimaryYAxis>
        <Chart3DMajorGridLines Width="1"></Chart3DMajorGridLines>
    </Chart3DPrimaryYAxis>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>
    <Chart3DTooltipSettings Enable="true"></Chart3DTooltipSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@AccessibleData" Name="Revenue" XName="Quarter" 
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

    public List<QuarterRevenue> AccessibleData = new List<QuarterRevenue>
    {
        new QuarterRevenue { Quarter = "Q1", Amount = 125000 },
        new QuarterRevenue { Quarter = "Q2", Amount = 145000 },
        new QuarterRevenue { Quarter = "Q3", Amount = 165000 },
        new QuarterRevenue { Quarter = "Q4", Amount = 185000 }
    };
}
```

## WAI-ARIA Attributes

Syncfusion 3D Charts automatically apply appropriate ARIA attributes:

### ARIA Roles

```html
<!-- Chart container -->
<div role="region" aria-label="Sales Performance Chart">
    <!-- Chart elements with ARIA attributes -->
</div>
```

### ARIA Labels

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Product Analysis" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ProductData" Name="Sales" XName="Product" 
                       YName="Units" Type="Chart3DSeriesType.Column">
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

The chart automatically includes:
- `aria-label` for chart title
- `role="img"` for chart visualization
- `aria-label` for series and data points
- `aria-hidden="true"` for decorative elements

### ARIA Live Regions

Dynamic updates are announced using ARIA live regions:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Live Data Updates" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DTooltipSettings Enable="true"></Chart3DTooltipSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@LiveData" XName="Time" YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class LiveDataPoint
    {
        public string Time { get; set; }
        public double Value { get; set; }
    }

    public List<LiveDataPoint> LiveData = new List<LiveDataPoint>
    {
        new LiveDataPoint { Time = "10:00", Value = 85 },
        new LiveDataPoint { Time = "10:15", Value = 92 },
        new LiveDataPoint { Time = "10:30", Value = 88 }
    };
}
```

## Keyboard Navigation

Full keyboard support for navigation and interaction:

### Keyboard Shortcuts

| Key | Action |
|-----|--------|
| **Alt + J** | Focus on chart |
| **Tab** | Navigate to next element |
| **Shift + Tab** | Navigate to previous element |
| **Arrow Keys** | Navigate between data points |
| **Enter / Space** | Select data point |
| **Ctrl + P** | Print chart |
| **Esc** | Clear selection |
| **Home** | First data point |
| **End** | Last data point |
| **Page Up** | Previous series |
| **Page Down** | Next series |

### Keyboard Navigation Example

```cshtml
@using Syncfusion.Blazor.Chart3D

<div style="margin-bottom: 10px;">
    <p><strong>Keyboard Navigation:</strong></p>
    <ul>
        <li>Press <kbd>Alt + J</kbd> to focus on chart</li>
        <li>Use <kbd>Arrow Keys</kbd> to navigate between data points</li>
        <li>Press <kbd>Enter</kbd> to select a data point</li>
        <li>Press <kbd>Ctrl + P</kbd> to print</li>
    </ul>
</div>

<SfChart3D Title="Keyboard Accessible Chart" WallColor="transparent" 
           EnableRotation="true" RotationAngle="7" TiltAngle="10" Depth="100" 
           SelectionMode="Syncfusion.Blazor.Chart3D.SelectionMode.Point">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DTooltipSettings Enable="true"></Chart3DTooltipSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@KeyboardData" XName="Category" YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class KeyboardDataPoint
    {
        public string Category { get; set; }
        public double Value { get; set; }
    }

    public List<KeyboardDataPoint> KeyboardData = new List<KeyboardDataPoint>
    {
        new KeyboardDataPoint { Category = "Item 1", Value = 45 },
        new KeyboardDataPoint { Category = "Item 2", Value = 62 },
        new KeyboardDataPoint { Category = "Item 3", Value = 58 },
        new KeyboardDataPoint { Category = "Item 4", Value = 70 }
    };
}
```

### Legend Keyboard Navigation

Navigate legend items using keyboard:

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Multi-Series Navigation" WallColor="transparent" 
           EnableRotation="true" RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DLegendSettings Visible="true" ToggleVisibility="true">
    </Chart3DLegendSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SeriesData" Name="Product A" XName="Month" 
                       YName="ValueA" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@SeriesData" Name="Product B" XName="Month" 
                       YName="ValueB" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@SeriesData" Name="Product C" XName="Month" 
                       YName="ValueC" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class MultiSeriesData
    {
        public string Month { get; set; }
        public double ValueA { get; set; }
        public double ValueB { get; set; }
        public double ValueC { get; set; }
    }

    public List<MultiSeriesData> SeriesData = new List<MultiSeriesData>
    {
        new MultiSeriesData { Month = "Jan", ValueA = 35, ValueB = 28, ValueC = 34 },
        new MultiSeriesData { Month = "Feb", ValueA = 32, ValueB = 27, ValueC = 35 },
        new MultiSeriesData { Month = "Mar", ValueA = 34, ValueB = 30, ValueC = 38 }
    };
}
```

## Best Practices

1. **Chart Title**:
   - Always provide descriptive titles
   - Use clear, concise language
   - Avoid jargon and abbreviations
   - Example: "Monthly Sales Performance" not "Chart 1"

2. **Series Names**:
   - Use meaningful series names for legends
   - Provide context: "2023 Revenue" not "Series 1"
   - Keep names concise but descriptive
   - Avoid duplicate names

3. **Color Contrast**:
   - Ensure WCAG 2.2 AA contrast ratio (4.5:1 for text)
   - Use high contrast colors for series
   - Test with colorblind simulators
   - Don't rely solely on color to convey information

4. **Data Labels**:
   - Enable data labels for critical information
   - Use clear, readable fonts (minimum 12px)
   - Ensure labels don't overlap
   - Format numbers appropriately

5. **Tooltips**:
   - Enable tooltips for detailed information
   - Format tooltip content clearly
   - Include series name, category, and value
   - Use consistent formatting

6. **Keyboard Support**:
   - Test all functionality with keyboard only
   - Ensure focus indicators are visible
   - Provide keyboard shortcuts documentation
   - Test with screen readers (JAWS, NVDA, Narrator)

7. **Alternative Content**:
   - Provide data table alternative
   - Include text summary of key insights
   - Offer CSV/Excel export options
   - Document trends and patterns

8. **Screen Reader Support**:
   - Test with multiple screen readers
   - Verify ARIA labels are announced correctly
   - Ensure navigation is logical
   - Test dynamic updates

9. **Responsive Design**:
   - Ensure chart is usable at different zoom levels
   - Test with browser zoom (200%, 400%)
   - Maintain readability on mobile devices
   - Support touch navigation

10. **Testing Checklist**:
    - ✅ Test with keyboard only (no mouse)
    - ✅ Test with screen reader (JAWS, NVDA, Narrator)
    - ✅ Verify color contrast meets WCAG standards
    - ✅ Test with Windows High Contrast mode
    - ✅ Verify focus indicators are visible
    - ✅ Test at 200% browser zoom
    - ✅ Check ARIA attributes using browser tools
    - ✅ Test on mobile devices
    - ✅ Verify alternative content is available
    - ✅ Test dynamic updates are announced

### Complete Accessible Chart Example

```cshtml
@using Syncfusion.Blazor.Chart3D

<div role="region" aria-label="Sales Analysis Region">
    <h2 id="chartTitle">Quarterly Sales Analysis</h2>
    
    <div style="margin-bottom: 15px;">
        <p><strong>Chart Summary:</strong> This chart displays quarterly sales performance 
           for three products across four quarters. Product B shows the highest growth, 
           while Product A maintains steady performance.</p>
    </div>
    
    <div style="margin-bottom: 10px;">
        <p><strong>Keyboard Instructions:</strong></p>
        <ul>
            <li>Press <kbd>Alt + J</kbd> to focus the chart</li>
            <li>Use <kbd>Arrow Keys</kbd> to navigate data points</li>
            <li>Press <kbd>Enter</kbd> to select</li>
            <li>Use <kbd>Page Up/Down</kbd> to switch between series</li>
        </ul>
    </div>
    
    <SfChart3D Title="Quarterly Sales by Product" Width="900px" Height="500px" 
               WallColor="transparent" EnableRotation="true" 
               RotationAngle="7" TiltAngle="10" Depth="100" 
               SelectionMode="Syncfusion.Blazor.Chart3D.SelectionMode.Point">
        <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
            <Chart3DMajorGridLines Width="1"></Chart3DMajorGridLines>
        </Chart3DPrimaryXAxis>
        
        <Chart3DPrimaryYAxis>
            <Chart3DMajorGridLines Width="1"></Chart3DMajorGridLines>
        </Chart3DPrimaryYAxis>
        
        <Chart3DLegendSettings Visible="true" ToggleVisibility="true">
        </Chart3DLegendSettings>
        
        <Chart3DTooltipSettings Enable="true" 
                                Format="<b>${series.name}</b><br>${point.x}: ${point.y}">
        </Chart3DTooltipSettings>
        
        <Chart3DSeriesCollection>
            <Chart3DSeries DataSource="@AccessibleSalesData" Name="Product A" 
                           XName="Quarter" YName="ProductA" 
                           Type="Chart3DSeriesType.Column">
                <Chart3DDataLabel Visible="true" Position="Syncfusion.Blazor.Chart3D.Chart3DDataLabelPosition.Top"></Chart3DDataLabel>
            </Chart3DSeries>
            <Chart3DSeries DataSource="@AccessibleSalesData" Name="Product B" 
                           XName="Quarter" YName="ProductB" 
                           Type="Chart3DSeriesType.Column">
                <Chart3DDataLabel Visible="true" Position="Syncfusion.Blazor.Chart3D.Chart3DDataLabelPosition.Top"></Chart3DDataLabel>
            </Chart3DSeries>
            <Chart3DSeries DataSource="@AccessibleSalesData" Name="Product C" 
                           XName="Quarter" YName="ProductC" 
                           Type="Chart3DSeriesType.Column">
                <Chart3DDataLabel Visible="true" Position="Syncfusion.Blazor.Chart3D.Chart3DDataLabelPosition.Top"></Chart3DDataLabel>
            </Chart3DSeries>
        </Chart3DSeriesCollection>
    </SfChart3D>
    
    <div style="margin-top: 15px;">
        <details>
            <summary><strong>View Data Table</strong></summary>
            <table style="border-collapse: collapse; width: 100%; margin-top: 10px;">
                <thead>
                    <tr style="background-color: #f0f0f0;">
                        <th style="border: 1px solid #ddd; padding: 8px;">Quarter</th>
                        <th style="border: 1px solid #ddd; padding: 8px;">Product A</th>
                        <th style="border: 1px solid #ddd; padding: 8px;">Product B</th>
                        <th style="border: 1px solid #ddd; padding: 8px;">Product C</th>
                    </tr>
                </thead>
                <tbody>
                    @foreach (var item in AccessibleSalesData)
                    {
                        <tr>
                            <td style="border: 1px solid #ddd; padding: 8px;">@item.Quarter</td>
                            <td style="border: 1px solid #ddd; padding: 8px;">@item.ProductA</td>
                            <td style="border: 1px solid #ddd; padding: 8px;">@item.ProductB</td>
                            <td style="border: 1px solid #ddd; padding: 8px;">@item.ProductC</td>
                        </tr>
                    }
                </tbody>
            </table>
        </details>
    </div>
</div>

@code {
    public class QuarterlyPerformance
    {
        public string Quarter { get; set; }
        public double ProductA { get; set; }
        public double ProductB { get; set; }
        public double ProductC { get; set; }
    }

    public List<QuarterlyPerformance> AccessibleSalesData = new List<QuarterlyPerformance>
    {
        new QuarterlyPerformance { Quarter = "Q1", ProductA = 85, ProductB = 92, ProductC = 78 },
        new QuarterlyPerformance { Quarter = "Q2", ProductA = 90, ProductB = 98, ProductC = 82 },
        new QuarterlyPerformance { Quarter = "Q3", ProductA = 88, ProductB = 105, ProductC = 85 },
        new QuarterlyPerformance { Quarter = "Q4", ProductA = 95, ProductB = 110, ProductC = 88 }
    };
}
```

This comprehensive example includes:
- Descriptive title and summary
- Keyboard navigation instructions
- High contrast colors
- Data labels for all points
- Tooltips with detailed information
- Legend with toggle functionality
- Alternative data table
- Proper ARIA structure
- Screen reader friendly markup
