# Data Labels, Tooltips, and Legend

This comprehensive guide covers data labels for value display, tooltips for interactive information, and legends for range identification in Bullet Charts.

## Table of Contents
- [Data Labels](#data-labels)
- [Tooltips](#tooltips)
- [Legend](#legend)
- [Combined Features](#combined-features)
- [Best Practices](#best-practices)

## Data Labels

Data labels display the actual value directly on the bullet chart, making it easy to read exact figures at a glance.

### Enable Data Labels

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@RevenueData" 
               ValueField="Revenue" 
               TargetField="Target" 
               CategoryField="Quarter"
               Height="400"
               Minimum="0" 
               Maximum="500" 
               Interval="100"
               Title="Quarterly Revenue (in thousands $)">
    <BulletChartDataLabel></BulletChartDataLabel>
    <BulletChartRangeCollection>
        <BulletChartRange End="200" Color="#d32f2f"></BulletChartRange>
        <BulletChartRange End="350" Color="#ffa726"></BulletChartRange>
        <BulletChartRange End="500" Color="#66bb6a"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class RevenueMetric
    {
        public double Revenue { get; set; }
        public double Target { get; set; }
        public string Quarter { get; set; }
    }
    
    public List<RevenueMetric> RevenueData = new List<RevenueMetric>
    {
        new RevenueMetric { Revenue = 380, Target = 350, Quarter = "Q1" },
        new RevenueMetric { Revenue = 420, Target = 400, Quarter = "Q2" },
        new RevenueMetric { Revenue = 310, Target = 350, Quarter = "Q3" }
    };
}
```

### Data Label Styling

Customize appearance using `BulletChartDataLabelStyle`:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               CategoryField="Product"
               Height="400"
               Minimum="0" 
               Maximum="100">
    <BulletChartDataLabel>
        <BulletChartDataLabelStyle Color="#ffffff" 
                                   Size="14px" 
                                   FontWeight="bold"
                                   FontStyle="normal"
                                   FontFamily="Arial, sans-serif"
                                   Opacity="1"></BulletChartDataLabelStyle>
    </BulletChartDataLabel>
    <BulletChartRangeCollection>
        <BulletChartRange End="40"></BulletChartRange>
        <BulletChartRange End="70"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class SalesMetric
    {
        public string Product { get; set; }
        public double Sales { get; set; }
        public double Quota { get; set; }
    }
    
    public List<SalesMetric> SalesData = new List<SalesMetric>
    {
        new SalesMetric { Product = "Product A", Sales = 85, Quota = 80 },
        new SalesMetric { Product = "Product B", Sales = 72, Quota = 75 },
        new SalesMetric { Product = "Product C", Sales = 92, Quota = 85 }
    };
}
```

### Data Label Format

Apply formatting to data label values:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               ValueField="Score" 
               TargetField="Goal" 
               CategoryField="Metric"
               Height="350"
               LabelFormat="{value}%"
               Minimum="0" 
               Maximum="100">
    <BulletChartDataLabel></BulletChartDataLabel>
    <BulletChartRangeCollection>
        <BulletChartRange End="40"></BulletChartRange>
        <BulletChartRange End="70"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class PerformanceMetric
    {
        public string Metric { get; set; }
        public double Score { get; set; }
        public double Goal { get; set; }
    }
    
    public List<PerformanceMetric> PerformanceData = new List<PerformanceMetric>
    {
        new PerformanceMetric { Metric = "Quality", Score = 88, Goal = 85 },
        new PerformanceMetric { Metric = "Efficiency", Score = 76, Goal = 80 },
        new PerformanceMetric { Metric = "Satisfaction", Score = 92, Goal = 90 }
    };
}
```

### Data Label Positioning

Control where data labels appear:

```razor
@using Syncfusion.Blazor.Charts

<!-- Data labels on actual bar -->
<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               Title="Sales with Data Labels">
    <BulletChartDataLabel>
        <BulletChartDataLabelStyle Color="#000000" Size="12px"></BulletChartDataLabelStyle>
    </BulletChartDataLabel>
    <BulletChartRangeCollection>
        <BulletChartRange End="200"></BulletChartRange>
        <BulletChartRange End="350"></BulletChartRange>
        <BulletChartRange End="500"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class SalesMetric
    {
        public double Sales { get; set; }
        public double Quota { get; set; }
    }
    
    public List<SalesMetric> SalesData = new List<SalesMetric>
    {
        new SalesMetric { Sales = 380, Quota = 350 }
    };
}
```

## Tooltips

Tooltips provide detailed information when users hover over chart elements.

### Enable Default Tooltip

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@RevenueData" 
               ValueField="Revenue" 
               TargetField="Target" 
               CategoryField="Month"
               Height="400"
               Minimum="0" 
               Maximum="500"
               Title="Monthly Revenue Performance">
    <BulletChartTooltip TValue="RevenueMetric" Enable="true"></BulletChartTooltip>
    <BulletChartRangeCollection>
        <BulletChartRange End="200"></BulletChartRange>
        <BulletChartRange End="350"></BulletChartRange>
        <BulletChartRange End="500"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class RevenueMetric
    {
        public string Month { get; set; }
        public double Revenue { get; set; }
        public double Target { get; set; }
    }
    
    public List<RevenueMetric> RevenueData = new List<RevenueMetric>
    {
        new RevenueMetric { Month = "Jan", Revenue = 380, Target = 350 },
        new RevenueMetric { Month = "Feb", Revenue = 420, Target = 400 },
        new RevenueMetric { Month = "Mar", Revenue = 310, Target = 350 }
    };
}
```

### Tooltip Styling

Customize tooltip appearance:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               ValueField="Score" 
               TargetField="Goal" 
               CategoryField="Department"
               Height="400"
               Minimum="0" 
               Maximum="100">
    <BulletChartTooltip TValue="PerformanceMetric" 
                        Enable="true" 
                        Fill="#1976d2">
        <BulletChartTooltipTextStyle Color="#ffffff" 
                                     Size="13px" 
                                     FontWeight="500"
                                     Opacity="1"></BulletChartTooltipTextStyle>
        <BulletChartTooltipBorder Color="#0d47a1" Width="2"></BulletChartTooltipBorder>
    </BulletChartTooltip>
    <BulletChartRangeCollection>
        <BulletChartRange End="40"></BulletChartRange>
        <BulletChartRange End="70"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class PerformanceMetric
    {
        public string Department { get; set; }
        public double Score { get; set; }
        public double Goal { get; set; }
    }
    
    public List<PerformanceMetric> PerformanceData = new List<PerformanceMetric>
    {
        new PerformanceMetric { Department = "Sales", Score = 88, Goal = 85 },
        new PerformanceMetric { Department = "Marketing", Score = 76, Goal = 80 },
        new PerformanceMetric { Department = "Support", Score = 92, Goal = 90 }
    };
}
```

### Custom Tooltip Template

Create rich, formatted tooltips with custom HTML:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               CategoryField="Region"
               Height="450"
               Minimum="0" 
               Maximum="500">
    <BulletChartTooltip TValue="SalesMetric" Enable="true">
        <Template>
            @{
                var data = context as SalesMetric;
                <div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); 
                            padding: 15px; 
                            border-radius: 8px; 
                            color: white; 
                            font-family: Arial, sans-serif;
                            box-shadow: 0 4px 6px rgba(0,0,0,0.2);">
                    <div style="font-weight: bold; font-size: 16px; margin-bottom: 10px; border-bottom: 2px solid rgba(255,255,255,0.3); padding-bottom: 5px;">
                        @data.Region Sales Report
                    </div>
                    <table style="width: 100%; border-spacing: 0;">
                        <tr>
                            <td style="padding: 5px 10px 5px 0; font-weight: 500;">Actual Sales:</td>
                            <td style="padding: 5px 0; text-align: right; font-weight: bold;">$@data.Sales.ToString("N0")K</td>
                        </tr>
                        <tr>
                            <td style="padding: 5px 10px 5px 0; font-weight: 500;">Target Quota:</td>
                            <td style="padding: 5px 0; text-align: right; font-weight: bold;">$@data.Quota.ToString("N0")K</td>
                        </tr>
                        <tr style="border-top: 1px solid rgba(255,255,255,0.2);">
                            <td style="padding: 8px 10px 5px 0; font-weight: 500;">Achievement:</td>
                            <td style="padding: 8px 0 5px 0; text-align: right; font-weight: bold; color: @(data.Sales >= data.Quota ? "#4caf50" : "#ffeb3b");">
                                @((data.Sales / data.Quota * 100).ToString("F1"))%
                            </td>
                        </tr>
                    </table>
                </div>
            }
        </Template>
    </BulletChartTooltip>
    <BulletChartRangeCollection>
        <BulletChartRange End="200"></BulletChartRange>
        <BulletChartRange End="350"></BulletChartRange>
        <BulletChartRange End="500"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class SalesMetric
    {
        public string Region { get; set; }
        public double Sales { get; set; }
        public double Quota { get; set; }
    }
    
    public List<SalesMetric> SalesData = new List<SalesMetric>
    {
        new SalesMetric { Region = "North America", Sales = 380, Quota = 350 },
        new SalesMetric { Region = "Europe", Sales = 420, Quota = 400 },
        new SalesMetric { Region = "Asia Pacific", Sales = 310, Quota = 350 }
    };
}
```

### Tooltip with Calculated Metrics

```razor
@using Syncfusion.Blazor.Charts

<BulletChartTooltip TValue="PerformanceMetric" Enable="true">
    <Template>
        @{
            var data = context as PerformanceMetric;
            var variance = data.Score - data.Goal;
            var variancePercent = (variance / data.Goal) * 100;
            var status = variance >= 0 ? "✓ Above Target" : "⚠ Below Target";
            var statusColor = variance >= 0 ? "#4caf50" : "#f44336";
            
            <div style="background-color: #ffffff; 
                        padding: 12px; 
                        border: 2px solid #e0e0e0; 
                        border-radius: 6px;
                        box-shadow: 0 2px 10px rgba(0,0,0,0.15);">
                <div style="font-weight: bold; color: #333; margin-bottom: 8px;">
                    @data.Metric Performance
                </div>
                <div style="margin-bottom: 5px; color: #666;">
                    Score: <strong>@data.Score.ToString("F1")</strong>
                </div>
                <div style="margin-bottom: 5px; color: #666;">
                    Goal: <strong>@data.Goal.ToString("F1")</strong>
                </div>
                <div style="margin-top: 8px; padding-top: 8px; border-top: 1px solid #e0e0e0;">
                    <span style="color: @statusColor; font-weight: bold;">@status</span>
                    <br />
                    <span style="color: #999; font-size: 12px;">
                        Variance: @variance.ToString("F1") (@variancePercent.ToString("F1")%)
                    </span>
                </div>
            </div>
        }
    </Template>
</BulletChartTooltip>
```

### Tooltip Format Options

```razor
@using Syncfusion.Blazor.Charts

<!-- Format with label format -->
<SfBulletChart DataSource="@RevenueData" 
               LabelFormat="${value}K">
    <BulletChartTooltip TValue="RevenueMetric" Enable="true"></BulletChartTooltip>
    <!-- Tooltip will show values formatted as $380K, etc. -->
</SfBulletChart>
```

## Legend

Legends help users identify what each range represents, especially useful when ranges have specific business meanings.

### Enable Legend

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               ValueField="Score" 
               TargetField="Goal" 
               Height="350"
               Title="Performance Metrics"
               Minimum="0" 
               Maximum="100">
    <BulletChartLegendSettings Visible="true"></BulletChartLegendSettings>
    <BulletChartRangeCollection>
        <BulletChartRange End="40" Name="Poor" Color="#d32f2f"></BulletChartRange>
        <BulletChartRange End="70" Name="Satisfactory" Color="#ffa726"></BulletChartRange>
        <BulletChartRange End="100" Name="Excellent" Color="#66bb6a"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class PerformanceMetric
    {
        public double Score { get; set; }
        public double Goal { get; set; }
    }
    
    public List<PerformanceMetric> PerformanceData = new List<PerformanceMetric>
    {
        new PerformanceMetric { Score = 85, Goal = 75 }
    };
}
```

### Legend Positioning

Control legend placement:

```razor
@using Syncfusion.Blazor.Charts

<!-- Bottom position (default) -->
<SfBulletChart DataSource="@SalesData">
    <BulletChartLegendSettings Visible="true" Position="LegendPosition.Bottom"></BulletChartLegendSettings>
    <!-- Ranges with names -->
</SfBulletChart>

<!-- Top position -->
<SfBulletChart DataSource="@SalesData">
    <BulletChartLegendSettings Visible="true" Position="LegendPosition.Top"></BulletChartLegendSettings>
    <!-- Ranges with names -->
</SfBulletChart>

<!-- Left position -->
<SfBulletChart DataSource="@SalesData">
    <BulletChartLegendSettings Visible="true" Position="LegendPosition.Left"></BulletChartLegendSettings>
    <!-- Ranges with names -->
</SfBulletChart>

<!-- Right position -->
<SfBulletChart DataSource="@SalesData">
    <BulletChartLegendSettings Visible="true" Position="LegendPosition.Right"></BulletChartLegendSettings>
    <!-- Ranges with names -->
</SfBulletChart>
```

### Legend Alignment

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@RevenueData" 
               ValueField="Revenue" 
               TargetField="Target" 
               Height="350">
    <BulletChartLegendSettings Visible="true" 
                               Position="LegendPosition.Top" 
                               Alignment="Alignment.Center"></BulletChartLegendSettings>
    <BulletChartRangeCollection>
        <BulletChartRange End="150" Name="Below Expectations"></BulletChartRange>
        <BulletChartRange End="250" Name="Meeting Expectations"></BulletChartRange>
        <BulletChartRange End="300" Name="Exceeding Expectations"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class RevenueMetric
    {
        public double Revenue { get; set; }
        public double Target { get; set; }
    }
    
    public List<RevenueMetric> RevenueData = new List<RevenueMetric>
    {
        new RevenueMetric { Revenue = 270, Target = 250 }
    };
}
```

**Alignment Options:**
- `Alignment.Near` - Left/Top aligned
- `Alignment.Center` - Center aligned
- `Alignment.Far` - Right/Bottom aligned

### Legend Customization

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               ValueField="Score" 
               TargetField="Goal" 
               Height="400">
    <BulletChartLegendSettings Visible="true" 
                               Position="LegendPosition.Bottom"
                               ShapeHeight="15" 
                               ShapeWidth="15" 
                               ShapePadding="10"
                               Padding="15"
                               Background="transparent">
        <BulletChartLegendTextStyle Size="14px" 
                                    Color="#333" 
                                    FontWeight="500"></BulletChartLegendTextStyle>
        <BulletChartLegendBorder Color="#e0e0e0" Width="1"></BulletChartLegendBorder>
    </BulletChartLegendSettings>
    <BulletChartRangeCollection>
        <BulletChartRange End="40" Name="Critical" Color="#c62828"></BulletChartRange>
        <BulletChartRange End="70" Name="Warning" Color="#f9a825"></BulletChartRange>
        <BulletChartRange End="100" Name="Success" Color="#2e7d32"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class PerformanceMetric
    {
        public double Score { get; set; }
        public double Goal { get; set; }
    }
    
    public List<PerformanceMetric> PerformanceData = new List<PerformanceMetric>
    {
        new PerformanceMetric { Score = 75, Goal = 70 }
    };
}
```

### Custom Legend Shapes

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               Height="350">
    <BulletChartLegendSettings Visible="true"></BulletChartLegendSettings>
    <BulletChartRangeCollection>
        <BulletChartRange End="200" Name="Low" Shape="LegendShape.Circle"></BulletChartRange>
        <BulletChartRange End="350" Name="Medium" Shape="LegendShape.Diamond"></BulletChartRange>
        <BulletChartRange End="500" Name="High" Shape="LegendShape.Triangle"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class SalesMetric
    {
        public double Sales { get; set; }
        public double Quota { get; set; }
    }
    
    public List<SalesMetric> SalesData = new List<SalesMetric>
    {
        new SalesMetric { Sales = 380, Quota = 350 }
    };
}
```

**Available Shapes:**
- `LegendShape.Circle`
- `LegendShape.Rectangle` (default)
- `LegendShape.Triangle`
- `LegendShape.Diamond`
- `LegendShape.Pentagon`
- `LegendShape.Cross`

### Legend with Paging

For charts with many ranges:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@QualityData" 
               ValueField="Quality" 
               TargetField="Standard" 
               Height="400">
    <BulletChartLegendSettings Visible="true" 
                               Width="20%" 
                               Height="15%"
                               Position="LegendPosition.Right"></BulletChartLegendSettings>
    <BulletChartRangeCollection>
        <BulletChartRange End="20" Name="Very Poor"></BulletChartRange>
        <BulletChartRange End="40" Name="Poor"></BulletChartRange>
        <BulletChartRange End="60" Name="Average"></BulletChartRange>
        <BulletChartRange End="80" Name="Good"></BulletChartRange>
        <BulletChartRange End="100" Name="Excellent"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class QualityMetric
    {
        public double Quality { get; set; }
        public double Standard { get; set; }
    }
    
    public List<QualityMetric> QualityData = new List<QualityMetric>
    {
        new QualityMetric { Quality = 85, Standard = 80 }
    };
}
```

## Combined Features

### All Features Together

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@ComprehensiveData" 
               ValueField="Current" 
               TargetField="Target" 
               CategoryField="Metric"
               Height="500"
               Minimum="0" 
               Maximum="100"
               LabelFormat="{value}%"
               Title="Comprehensive Performance Dashboard">
    <!-- Data Labels -->
    <BulletChartDataLabel>
        <BulletChartDataLabelStyle Color="#ffffff" Size="13px" FontWeight="bold"></BulletChartDataLabelStyle>
    </BulletChartDataLabel>
    
    <!-- Tooltip -->
    <BulletChartTooltip TValue="MetricData" Enable="true" Fill="#1976d2">
        <Template>
            @{
                var data = context as MetricData;
                <div style="padding: 10px; background: #ffffff; border: 2px solid #1976d2; border-radius: 4px;">
                    <strong>@data.Metric</strong><br />
                    Current: @data.Current%<br />
                    Target: @data.Target%
                </div>
            }
        </Template>
    </BulletChartTooltip>
    
    <!-- Legend -->
    <BulletChartLegendSettings Visible="true" Position="LegendPosition.Bottom">
        <BulletChartLegendTextStyle Size="12px"></BulletChartLegendTextStyle>
    </BulletChartLegendSettings>
    
    <!-- Ranges -->
    <BulletChartRangeCollection>
        <BulletChartRange End="40" Name="Needs Improvement" Color="#d32f2f"></BulletChartRange>
        <BulletChartRange End="70" Name="Good" Color="#ffa726"></BulletChartRange>
        <BulletChartRange End="100" Name="Excellent" Color="#66bb6a"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class MetricData
    {
        public string Metric { get; set; }
        public double Current { get; set; }
        public double Target { get; set; }
    }
    
    public List<MetricData> ComprehensiveData = new List<MetricData>
    {
        new MetricData { Metric = "Sales", Current = 88, Target = 85 },
        new MetricData { Metric = "Quality", Current = 76, Target = 80 },
        new MetricData { Metric = "Customer Satisfaction", Current = 92, Target = 90 }
    };
}
```

## Best Practices

### 1. Data Labels

**When to use:**
- Single KPI displays where exact value is critical
- Executive dashboards requiring at-a-glance numbers
- Print reports where interactivity isn't available

**When to avoid:**
- Multiple categories (can clutter)
- Small chart dimensions
- When tooltips provide sufficient detail

### 2. Tooltips

**Best practices:**
- Always enable for interactive dashboards
- Include context (labels, units)
- Keep content concise but informative
- Use consistent styling across charts
- Add calculated metrics (variance, percentages)

**Template guidelines:**
- Max width: 300-400px
- Use clear typography hierarchy
- Include visual indicators (colors, icons)
- Format numbers appropriately

### 3. Legend

**Naming conventions:**
- Use clear, business-friendly names
- Keep names short (2-3 words max)
- Use action-oriented language when appropriate
  - Good: "Needs Improvement", "Exceeding Expectations"
  - Avoid: "Range 1", "Zone A"

**Positioning:**
- Bottom: Best for wide charts
- Top: When title space allows
- Right: For tall/vertical charts
- Left: Rarely used, consider alternatives

### 4. Combined Usage Matrix

| Scenario | Data Labels | Tooltips | Legend |
|----------|-------------|----------|--------|
| Executive Dashboard | ✓ | ✓ | ✓ |
| Mobile View | ✗ | ✓ | ✓ |
| Print Report | ✓ | ✗ | ✓ |
| Embedded Widget | ✗ | ✓ | ✗ |
| Single KPI Card | ✓ | Optional | ✗ |
| Comparison View | ✗ | ✓ | ✓ |

## Troubleshooting

### Issue: Data labels overlapping

**Solutions:**
1. Reduce font size
2. Disable data labels, rely on tooltips
3. Increase chart height (for vertical)
4. Use fewer categories

### Issue: Tooltip not appearing

**Solutions:**
1. Verify `Enable="true"` is set
2. Ensure `TValue` generic type matches data model
3. Check browser console for errors
4. Test with default tooltip before custom template

### Issue: Legend items cut off

**Solutions:**
1. Increase legend `Width` and `Height`
2. Shorten range names
3. Reduce `ShapeWidth` and `ShapePadding`
4. Position legend at bottom instead of side

### Issue: Custom tooltip template not rendering

**Solutions:**
1. Verify data model properties match template references
2. Check for null values in data
3. Escape special characters in HTML
4. Use `@((MarkupString)"<html>")` for complex HTML