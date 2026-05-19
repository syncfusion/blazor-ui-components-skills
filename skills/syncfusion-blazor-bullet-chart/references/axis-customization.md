# Axis Customization

The axis in a Bullet Chart defines the scale and provides context for interpreting values. This guide covers comprehensive axis customization including scale, orientation, tick marks, labels, and formatting.

## Table of Contents
- [Axis Scale Properties](#axis-scale-properties)
- [Orientation](#orientation)
- [Tick Lines Customization](#tick-lines-customization)
- [Label Formatting](#label-formatting)
- [Label Positioning](#label-positioning)
- [Opposed Position and RTL](#opposed-position-and-rtl)
- [Best Practices](#best-practices)

## Axis Scale Properties

### Minimum, Maximum, and Interval

The fundamental axis properties define the scale range and graduation marks:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@RevenueData" 
               ValueField="Revenue" 
               TargetField="Target" 
               Minimum="0" 
               Maximum="500" 
               Interval="100"
               Title="Revenue (in thousands $)">
    <BulletChartRangeCollection>
        <BulletChartRange End="200"></BulletChartRange>
        <BulletChartRange End="350"></BulletChartRange>
        <BulletChartRange End="500"></BulletChartRange>
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
        new RevenueMetric { Revenue = 380, Target = 350 }
    };
}
```

**Properties explained:**
- `Minimum="0"` - Axis starts at 0
- `Maximum="500"` - Axis ends at 500
- `Interval="100"` - Major tick marks at 0, 100, 200, 300, 400, 500

### Dynamic Scale Calculation

Calculate axis scale based on data values:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               Minimum="@MinValue" 
               Maximum="@MaxValue" 
               Interval="@IntervalValue"
               Title="Dynamic Sales Scale">
    <BulletChartRangeCollection>
        <BulletChartRange End="@(MaxValue * 0.4)"></BulletChartRange>
        <BulletChartRange End="@(MaxValue * 0.7)"></BulletChartRange>
        <BulletChartRange End="@MaxValue"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class SalesMetric
    {
        public double Sales { get; set; }
        public double Quota { get; set; }
    }
    
    public List<SalesMetric> SalesData { get; set; } = new();
    public double MinValue { get; set; }
    public double MaxValue { get; set; }
    public double IntervalValue { get; set; }
    
    protected override void OnInitialized()
    {
        SalesData = new List<SalesMetric>
        {
            new SalesMetric { Sales = 4500, Quota = 5000 }
        };
        
        // Calculate scale dynamically
        MinValue = 0;
        MaxValue = Math.Ceiling((SalesData.First().Quota * 1.2) / 1000) * 1000; // Round up to nearest 1000
        IntervalValue = MaxValue / 5;
    }
}
```

## Orientation

### Horizontal Orientation (Default)

The standard layout with axis running left to right:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               Orientation="OrientationType.Horizontal"
               ValueField="Score" 
               TargetField="Goal" 
               Minimum="0" 
               Maximum="100" 
               Interval="25"
               Title="Horizontal Performance Chart">
    <BulletChartRangeCollection>
        <BulletChartRange End="40" Color="#d32f2f"></BulletChartRange>
        <BulletChartRange End="70" Color="#ffa726"></BulletChartRange>
        <BulletChartRange End="100" Color="#66bb6a"></BulletChartRange>
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

**When to use:** Standard dashboards, single KPIs, wide layouts.

### Vertical Orientation

Axis runs from bottom to top:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               Orientation="OrientationType.Vertical"
               Width="20%"
               Height="400"
               ValueField="Score" 
               TargetField="Goal" 
               Minimum="0" 
               Maximum="100" 
               Interval="25"
               Title="Vertical Performance"
               Subtitle="Q1 2026">
    <BulletChartRangeCollection>
        <BulletChartRange End="40" Color="#d32f2f"></BulletChartRange>
        <BulletChartRange End="70" Color="#ffa726"></BulletChartRange>
        <BulletChartRange End="100" Color="#66bb6a"></BulletChartRange>
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

**When to use:** Space-constrained layouts, tall panels, thermometer-style visualizations.

### Multiple Vertical Charts

```razor
@using Syncfusion.Blazor.Charts

<div class="d-flex justify-content-around">
    @foreach (var metric in KPIMetrics)
    {
        <SfBulletChart DataSource="@(new List<KPIData> { metric })" 
                       Orientation="OrientationType.Vertical"
                       Width="15%"
                       Height="350"
                       ValueField="Value" 
                       TargetField="Target" 
                       Minimum="0" 
                       Maximum="100"
                       Interval="25"
                       Title="@metric.Name">
            <BulletChartRangeCollection>
                <BulletChartRange End="40"></BulletChartRange>
                <BulletChartRange End="70"></BulletChartRange>
                <BulletChartRange End="100"></BulletChartRange>
            </BulletChartRangeCollection>
        </SfBulletChart>
    }
</div>

@code {
    public class KPIData
    {
        public string Name { get; set; }
        public double Value { get; set; }
        public double Target { get; set; }
    }
    
    public List<KPIData> KPIMetrics = new List<KPIData>
    {
        new KPIData { Name = "Sales", Value = 88, Target = 85 },
        new KPIData { Name = "Quality", Value = 92, Target = 90 },
        new KPIData { Name = "Efficiency", Value = 76, Target = 80 },
        new KPIData { Name = "Satisfaction", Value = 85, Target = 85 }
    };
}
```

## Tick Lines Customization

### Major Tick Lines

Customize the primary axis graduation marks:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               Minimum="0" 
               Maximum="600" 
               Interval="100"
               Title="Sales with Custom Major Ticks">
    <BulletChartMajorTickLines Width="3" 
                               Height="20" 
                               Color="#1976d2"></BulletChartMajorTickLines>
    <BulletChartRangeCollection>
        <BulletChartRange End="200"></BulletChartRange>
        <BulletChartRange End="400"></BulletChartRange>
        <BulletChartRange End="600"></BulletChartRange>
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
        new SalesMetric { Sales = 450, Quota = 400 }
    };
}
```

**Properties:**
- `Width` - Thickness of tick line (default: 1)
- `Height` - Length of tick line (default: 10)
- `Color` - Tick line color

### Minor Tick Lines

Add intermediate tick marks between major ticks:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               Minimum="0" 
               Maximum="500" 
               Interval="100"
               Title="Sales with Minor Ticks">
    <BulletChartMajorTickLines Width="2" Height="15" Color="#333"></BulletChartMajorTickLines>
    <BulletChartMinorTickLines Width="1" Height="8" Color="#999"></BulletChartMinorTickLines>
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

### Range-Colored Tick Lines

Make tick lines adopt colors from their corresponding ranges:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@RevenueData" 
               ValueField="Revenue" 
               TargetField="Target" 
               Minimum="0" 
               Maximum="300" 
               Interval="50"
               Title="Revenue with Range-Colored Ticks">
    <BulletChartMajorTickLines Width="3" Height="15" EnableRangeColor="true"></BulletChartMajorTickLines>
    <BulletChartMinorTickLines Width="2" Height="10" EnableRangeColor="true"></BulletChartMinorTickLines>
    <BulletChartRangeCollection>
        <BulletChartRange End="100" Color="#c62828"></BulletChartRange>
        <BulletChartRange End="200" Color="#f9a825"></BulletChartRange>
        <BulletChartRange End="300" Color="#2e7d32"></BulletChartRange>
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
        new RevenueMetric { Revenue = 240, Target = 200 }
    };
}
```

### Tick Placement

Control whether ticks appear inside or outside the chart area:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               ValueField="Score" 
               TargetField="Goal" 
               TickPosition="TickPosition.Inside"
               Minimum="0" 
               Maximum="100"
               Title="Inside Tick Placement">
    <BulletChartMajorTickLines Width="2" Height="12"></BulletChartMajorTickLines>
    <BulletChartRangeCollection>
        <BulletChartRange End="40"></BulletChartRange>
        <BulletChartRange End="70"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
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

**Options:**
- `TickPosition.Outside` (default) - Ticks extend outward
- `TickPosition.Inside` - Ticks extend into chart area

## Label Formatting

### Basic Number Formats

Apply standard numeric formats using the `Format` property:

```razor
@using Syncfusion.Blazor.Charts

<!-- Currency Format -->
<SfBulletChart DataSource="@RevenueData" 
               ValueField="Revenue" 
               TargetField="Target" 
               Format="C"
               Minimum="0" 
               Maximum="100000"
               Interval="20000"
               Title="Revenue in Currency">
    <BulletChartRangeCollection>
        <BulletChartRange End="40000"></BulletChartRange>
        <BulletChartRange End="70000"></BulletChartRange>
        <BulletChartRange End="100000"></BulletChartRange>
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
        new RevenueMetric { Revenue = 75000, Target = 70000 }
    };
}
```

### Percentage Format

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@CompletionData" 
               ValueField="Completion" 
               TargetField="Goal" 
               Format="P0"
               Minimum="0" 
               Maximum="1"
               Interval="0.25"
               Title="Project Completion">
    <BulletChartRangeCollection>
        <BulletChartRange End="0.5"></BulletChartRange>
        <BulletChartRange End="0.8"></BulletChartRange>
        <BulletChartRange End="1"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class CompletionMetric
    {
        public double Completion { get; set; }
        public double Goal { get; set; }
    }
    
    public List<CompletionMetric> CompletionData = new List<CompletionMetric>
    {
        new CompletionMetric { Completion = 0.85, Goal = 0.80 }
    };
}
```

### Common Format Strings

| Format | Example Value | Result | Description |
|--------|--------------|--------|-------------|
| `N0` | 1000 | 1,000 | Number with no decimals |
| `N1` | 1000 | 1,000.0 | Number with 1 decimal |
| `N2` | 1000 | 1,000.00 | Number with 2 decimals |
| `C` | 1000 | $1,000.00 | Currency (default culture) |
| `C0` | 1000 | $1,000 | Currency with no decimals |
| `P0` | 0.75 | 75% | Percentage with no decimals |
| `P1` | 0.75 | 75.0% | Percentage with 1 decimal |

### Custom Label Format

Use `LabelFormat` for custom label templates:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               LabelFormat="${value}K"
               Minimum="0" 
               Maximum="500"
               Interval="100"
               Title="Sales (in thousands)">
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

**Result:** Labels display as "0K", "100K", "200K", etc.

### Grouping Separator

Add thousands separators to large numbers:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@RevenueData" 
               ValueField="Revenue" 
               TargetField="Target" 
               EnableGroupSeparator="true"
               Format="N0"
               Minimum="0" 
               Maximum="5000000"
               Interval="1000000"
               Title="Annual Revenue">
    <BulletChartRangeCollection>
        <BulletChartRange End="2000000"></BulletChartRange>
        <BulletChartRange End="3500000"></BulletChartRange>
        <BulletChartRange End="5000000"></BulletChartRange>
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
        new RevenueMetric { Revenue = 3800000, Target = 3500000 }
    };
}
```

**Result:** Labels show "1,000,000", "2,000,000", etc.

## Label Positioning

### Label Placement

Control label position relative to the chart:

```razor
@using Syncfusion.Blazor.Charts

<!-- Outside Placement (Default) -->
<SfBulletChart DataSource="@PerformanceData" 
               ValueField="Score" 
               TargetField="Goal" 
               LabelPosition="LabelsPlacement.Outside"
               Minimum="0" 
               Maximum="100"
               Title="Labels Outside">
    <BulletChartRangeCollection>
        <BulletChartRange End="40"></BulletChartRange>
        <BulletChartRange End="70"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

<!-- Inside Placement -->
<SfBulletChart DataSource="@PerformanceData" 
               ValueField="Score" 
               TargetField="Goal" 
               LabelPosition="LabelsPlacement.Inside"
               Minimum="0" 
               Maximum="100"
               Title="Labels Inside">
    <BulletChartRangeCollection>
        <BulletChartRange End="40"></BulletChartRange>
        <BulletChartRange End="70"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
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

### Label Style Customization

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               Minimum="0" 
               Maximum="500">
    <BulletChartLabelStyle Color="#1976d2" 
                           Size="14px" 
                           FontWeight="600"
                           FontStyle="italic"
                           Opacity="1"></BulletChartLabelStyle>
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

### Range-Colored Labels

Make axis labels adopt range colors:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               ValueField="Score" 
               TargetField="Goal" 
               Minimum="0" 
               Maximum="100"
               Interval="25"
               Title="Range-Colored Labels">
    <BulletChartLabelStyle EnableRangeColor="true" Size="13px" FontWeight="500"></BulletChartLabelStyle>
    <BulletChartRangeCollection>
        <BulletChartRange End="40" Color="#d32f2f"></BulletChartRange>
        <BulletChartRange End="70" Color="#ffa726"></BulletChartRange>
        <BulletChartRange End="100" Color="#66bb6a"></BulletChartRange>
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

### Category Label Customization

When using `CategoryField`, customize category labels separately:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@DepartmentData" 
               ValueField="Performance" 
               TargetField="Target" 
               CategoryField="Department"
               Height="400"
               Minimum="0" 
               Maximum="100">
    <BulletChartCategoryLabelStyle Color="#d32f2f" 
                                   Size="15px" 
                                   FontWeight="bold"
                                   FontStyle="normal"
                                   Opacity="1"></BulletChartCategoryLabelStyle>
    <BulletChartLabelStyle Color="#1976d2" Size="12px"></BulletChartLabelStyle>
    <BulletChartRangeCollection>
        <BulletChartRange End="40"></BulletChartRange>
        <BulletChartRange End="70"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class DepartmentMetric
    {
        public string Department { get; set; }
        public double Performance { get; set; }
        public double Target { get; set; }
    }
    
    public List<DepartmentMetric> DepartmentData = new List<DepartmentMetric>
    {
        new DepartmentMetric { Department = "Sales", Performance = 88, Target = 85 },
        new DepartmentMetric { Department = "Marketing", Performance = 76, Target = 80 },
        new DepartmentMetric { Department = "Support", Performance = 92, Target = 90 }
    };
}
```

## Opposed Position and RTL

### Opposed Position

Flip the axis to the opposite side:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               ValueField="Score" 
               TargetField="Goal" 
               OpposedPosition="true"
               Minimum="0" 
               Maximum="100"
               Title="Opposed Axis Position">
    <BulletChartRangeCollection>
        <BulletChartRange End="40"></BulletChartRange>
        <BulletChartRange End="70"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
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

**Effect:** Axis labels and ticks appear above the chart (horizontal) or to the right (vertical).

### Right-to-Left (RTL) Support

Enable RTL layout for Arabic, Hebrew, and similar languages:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               EnableRtl="true"
               Minimum="0" 
               Maximum="500"
               Title="RTL Bullet Chart">
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

**Effect:** Chart renders from right to left, with 0 on the right side.

## Best Practices

### 1. Scale Selection

**Rule of thumb:** Set `Maximum` to 10-20% above the highest expected value.

```razor
<!-- Good: Room for growth -->
<SfBulletChart Minimum="0" Maximum="120" Interval="20">
    <!-- Data values: Actual=95, Target=100 -->
</SfBulletChart>

<!-- Avoid: Too tight, values appear cramped -->
<SfBulletChart Minimum="0" Maximum="100" Interval="20">
    <!-- Data value: Actual=98 appears at edge -->
</SfBulletChart>
```

### 2. Interval Guidelines

| Data Range | Recommended Intervals | Example |
|------------|----------------------|---------|
| 0-100 | 10, 20, or 25 | 0, 20, 40, 60, 80, 100 |
| 0-500 | 50 or 100 | 0, 100, 200, 300, 400, 500 |
| 0-1000 | 100, 200, or 250 | 0, 200, 400, 600, 800, 1000 |
| Large values | Use powers of 10 | 0, 10000, 20000, 30000 |

### 3. Label Formatting

- Use `Format="C"` for monetary values
- Use `Format="P0"` for percentages
- Use `EnableGroupSeparator="true"` for values > 10,000
- Use `LabelFormat` for custom units (K, M, B)

### 4. Orientation Choice

| Scenario | Recommended | Reason |
|----------|------------|--------|
| Single KPI | Horizontal | Standard, familiar |
| Multiple KPIs | Horizontal with CategoryField | Easy comparison |
| Space-constrained | Vertical | Fits narrow panels |
| Dashboard cards | Vertical | Compact |

## Troubleshooting

### Issue: Axis labels overlap

**Solutions:**
1. Increase `Interval` value
2. Reduce font size in `BulletChartLabelStyle`
3. Use `LabelFormat` to shorten labels (e.g., "100K" instead of "100000")
4. Enable `EnableGroupSeparator` for readability

### Issue: Data not visible on chart

**Solutions:**
1. Verify `Minimum` and `Maximum` encompass your data values
2. Check that `ValueField` and `TargetField` map to numeric properties
3. Ensure data values are not outside the axis range
4. Use dynamic scale calculation for variable data

### Issue: Tick lines not displaying

**Solutions:**
1. Set `Width` and `Height` properties on tick line components
2. Ensure `Color` contrasts with background
3. Check `TickPosition` setting
4. Verify interval settings are appropriate

### Issue: Format not applying

**Solutions:**
1. Use `Format` for standard formats (C, N, P)
2. Use `LabelFormat` for custom templates with `${value}` placeholder
3. Ensure `EnableGroupSeparator="true"` when using N format
4. Check that culture/localization settings are correct