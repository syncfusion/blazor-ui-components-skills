# Actual and Target Bars

## Table of Contents
- [Actual Bar (Feature Measure)](#actual-bar-feature-measure)
- [Target Bar (Comparative Measure)](#target-bar-comparative-measure)
- [Styling and Customization](#styling-and-customization)
- [Best Practices](#best-practices)

The Bullet Chart has two primary visual elements that represent data: the **Actual Bar** (feature measure) showing the current value, and the **Target Bar** (comparative measure) showing the goal or benchmark.

## Actual Bar (Feature Measure)

The Actual Bar displays the primary data value being measured. It's the main visual element that represents current performance, sales, revenue, or any metric you're tracking.

### Basic Configuration

Map your data field to the actual bar using the `ValueField` property:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="CurrentSales" 
               Minimum="0" 
               Maximum="500" 
               Interval="100">
    <BulletChartRangeCollection>
        <BulletChartRange End="200"></BulletChartRange>
        <BulletChartRange End="350"></BulletChartRange>
        <BulletChartRange End="500"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class SalesMetric
    {
        public double CurrentSales { get; set; }
    }
    
    public List<SalesMetric> SalesData = new List<SalesMetric>
    {
        new SalesMetric { CurrentSales = 380 }
    };
}
```

### Actual Bar Types

The shape of the actual bar can be customized using the `Type` property. Two types are available:

#### 1. Rect Type (Default)

A rectangular bar that extends horizontally (or vertically) from the minimum to the actual value:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@RevenueData" 
               Type="FeatureType.Rect"
               ValueField="Revenue" 
               Minimum="0" 
               Maximum="300" 
               Interval="50"
               Title="Revenue - Rect Type">
    <BulletChartRangeCollection>
        <BulletChartRange End="150" Color="#ff5252"></BulletChartRange>
        <BulletChartRange End="250" Color="#ffeb3b"></BulletChartRange>
        <BulletChartRange End="300" Color="#4caf50"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class Metric
    {
        public double Revenue { get; set; }
    }
    
    public List<Metric> RevenueData = new List<Metric>
    {
        new Metric { Revenue = 270 }
    };
}
```

**When to use:** Best for showing magnitude and comparing against ranges clearly. Ideal for most KPI dashboards.

#### 2. Dot Type

A circular marker positioned at the actual value:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@RevenueData" 
               Type="FeatureType.Dot"
               ValueField="Revenue" 
               Minimum="0" 
               Maximum="300" 
               Interval="50"
               Title="Revenue - Dot Type">
    <BulletChartRangeCollection>
        <BulletChartRange End="150" Color="#ff5252"></BulletChartRange>
        <BulletChartRange End="250" Color="#ffeb3b"></BulletChartRange>
        <BulletChartRange End="300" Color="#4caf50"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class Metric
    {
        public double Revenue { get; set; }
    }
    
    public List<Metric> RevenueData = new List<Metric>
    {
        new Metric { Revenue = 270 }
    };
}
```

**When to use:** Best for emphasizing the exact value without dominating the visual space. Useful when you want ranges to be more prominent.

## Target Bar (Comparative Measure)

The Target Bar is a perpendicular line or shape that marks the goal, quota, or benchmark value. It provides a quick visual comparison point against the actual value.

### Basic Configuration

Map your target field using the `TargetField` property:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               ValueField="Actual" 
               TargetField="Target" 
               Minimum="0" 
               Maximum="100" 
               Interval="20"
               Title="Performance vs Target">
    <BulletChartRangeCollection>
        <BulletChartRange End="40" Color="#d32f2f"></BulletChartRange>
        <BulletChartRange End="70" Color="#ffa726"></BulletChartRange>
        <BulletChartRange End="100" Color="#66bb6a"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class PerformanceMetric
    {
        public double Actual { get; set; }
        public double Target { get; set; }
    }
    
    public List<PerformanceMetric> PerformanceData = new List<PerformanceMetric>
    {
        new PerformanceMetric { Actual = 85, Target = 75 }
    };
}
```

### Target Bar Types

Customize the target marker shape using the `TargetTypes` property:

#### 1. Rect Type (Default)

A rectangular line perpendicular to the chart orientation:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               TargetTypes="new List<TargetType> { TargetType.Rect }"
               Minimum="0" 
               Maximum="500">
    <BulletChartRangeCollection>
        <BulletChartRange End="200"></BulletChartRange>
        <BulletChartRange End="350"></BulletChartRange>
        <BulletChartRange End="500"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>
```

#### 2. Circle Type

A circular marker at the target value:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               TargetTypes="new List<TargetType> { TargetType.Circle }"
               Minimum="0" 
               Maximum="500">
    <BulletChartRangeCollection>
        <BulletChartRange End="200"></BulletChartRange>
        <BulletChartRange End="350"></BulletChartRange>
        <BulletChartRange End="500"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>
```

#### 3. Cross Type

An "X" marker at the target position:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               TargetTypes="new List<TargetType> { TargetType.Cross }"
               Minimum="0" 
               Maximum="500">
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

### Multiple Target Values

Display multiple targets for different comparison points:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@MultiTargetData" 
               ValueField="Actual" 
               TargetField="Target1,Target2,Target3" 
               TargetTypes="new List<Syncfusion.Blazor.Charts.TargetType> { Syncfusion.Blazor.Charts.TargetType.Rect, Syncfusion.Blazor.Charts.TargetType.Circle, Syncfusion.Blazor.Charts.TargetType.Cross }"
               Minimum="0" 
               Maximum="100" 
               Interval="20"
               Title="Performance with Multiple Targets">
    <BulletChartRangeCollection>
        <BulletChartRange End="40" Color="#d32f2f"></BulletChartRange>
        <BulletChartRange End="70" Color="#ffa726"></BulletChartRange>
        <BulletChartRange End="100" Color="#66bb6a"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class MultiTargetMetric
    {
        public double Actual { get; set; }
        public double Target1 { get; set; }  // Minimum acceptable
        public double Target2 { get; set; }  // Expected target
        public double Target3 { get; set; }  // Stretch goal
    }
    
    public List<MultiTargetMetric> MultiTargetData = new List<MultiTargetMetric>
    {
        new MultiTargetMetric 
        { 
            Actual = 78, 
            Target1 = 60,   // Minimum
            Target2 = 75,   // Expected
            Target3 = 90    // Stretch
        }
    };
}
```

## Styling and Customization

### Actual Bar Customization

Customize the actual bar appearance using `BulletChartValueBorder`:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               Minimum="0" 
               Maximum="500">
    <BulletChartValueBorder Color="#1976d2" Width="3"></BulletChartValueBorder>
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
    }
    
    public List<SalesMetric> SalesData = new List<SalesMetric>
    {
        new SalesMetric { Sales = 380 }
    };
}
```

### Target Bar Customization

Customize the target bar using `BulletChartTargetBorder`:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               ValueField="Actual" 
               TargetField="Target" 
               TargetTypes="new List<Syncfusion.Blazor.Charts.TargetType> { Syncfusion.Blazor.Charts.TargetType.Circle }"
               Minimum="0" 
               Maximum="100"
               TargetColor="#d32f2f"
               TargetWidth="4">
    <BulletChartRangeCollection>
        <BulletChartRange End="40"></BulletChartRange>
        <BulletChartRange End="70"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class PerformanceMetric
    {
        public double Actual { get; set; }
        public double Target { get; set; }
    }
    
    public List<PerformanceMetric> PerformanceData = new List<PerformanceMetric>
    {
        new PerformanceMetric { Actual = 65, Target = 75 }
    };
}
```

## Best Practices

### Visual Hierarchy

1. **Actual Bar Dominance**: The actual bar should be the primary visual element
2. **Target Clarity**: Target marker should be visible but not overwhelming
3. **Contrast**: Ensure actual and target colors contrast with ranges

### Type Selection

| Scenario | Recommended Actual Type | Recommended Target Type |
|----------|------------------------|------------------------|
| Standard KPI Dashboard | Rect | Rect or Circle |
| Space-constrained layout | Dot | Circle or Cross |
| Multiple targets | Rect | Different shapes (Rect, Circle, Cross) |
| Emphasis on exact values | Dot | Circle |

### Performance Indicators

```razor
@using Syncfusion.Blazor.Charts

<!-- Good: Clear visual hierarchy -->
<SfBulletChart DataSource="@Data" 
               Type="FeatureType.Rect"
               ValueField="Value" 
               TargetField="Target" 
               TargetTypes="new List<Syncfusion.Blazor.Charts.TargetType> { Syncfusion.Blazor.Charts.TargetType.Circle }"
               TargetColor="#d32f2f"
               TargetWidth="3">
    <BulletChartValueBorder Color="#1976d2" Width="2"></BulletChartValueBorder>
    <BulletChartRangeCollection>
        <BulletChartRange End="40" Color="#ffebee"></BulletChartRange>
        <BulletChartRange End="70" Color="#fff3e0"></BulletChartRange>
        <BulletChartRange End="100" Color="#e8f5e9"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>
```

### Common Patterns

**Exceeding Target (Positive):**
- Actual bar extends beyond target marker
- Green/positive range color
- Clear visual success indicator

**Below Target (Needs Attention):**
- Actual bar stops before target marker
- Yellow/orange warning color
- Gap between actual and target is visible

**At Target (Meeting Goal):**
- Actual bar aligns with target marker
- Neutral or positive color
- Alignment shows goal achievement

