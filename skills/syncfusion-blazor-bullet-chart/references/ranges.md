# Ranges

Ranges represent qualitative zones in the bullet chart that indicate performance levels such as **Poor**, **Satisfactory**, and **Good**. They provide visual context for interpreting the actual value against expected performance levels.

## Table of Contents
- [Understanding Ranges](#understanding-ranges)
- [Basic Range Configuration](#basic-range-configuration)
- [Color Customization](#color-customization)
- [Opacity Control](#opacity-control)
- [Multiple Range Configurations](#multiple-range-configurations)
- [Best Practices for Range Design](#best-practices-for-range-design)
- [Common Patterns](#common-patterns)

## Understanding Ranges

Ranges are background segments that divide the chart into qualitative zones. Each range typically represents a performance category:

- **Poor/Bad Range**: Values below acceptable levels (often red or dark colors)
- **Satisfactory/Average Range**: Acceptable but not exceptional values (yellow/orange)
- **Good/Excellent Range**: Target or above-target performance (green or light colors)

## Basic Range Configuration

Define ranges using the `BulletChartRangeCollection` component with the `End` property specifying where each range terminates:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               ValueField="Score" 
               TargetField="Goal" 
               Minimum="0" 
               Maximum="100" 
               Interval="20"
               Title="Performance Score">
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
        new PerformanceMetric { Score = 65, Goal = 75 }
    };
}
```

**How it works:**
- First range: Minimum (0) to End (40) = Poor zone
- Second range: 40 to 70 = Satisfactory zone
- Third range: 70 to 100 = Good zone

## Color Customization

Assign custom colors to ranges using the `Color` property:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@RevenueData" 
               ValueField="Revenue" 
               TargetField="Target" 
               Minimum="0" 
               Maximum="300" 
               Interval="50"
               Title="Revenue (in thousands)">
    <BulletChartRangeCollection>
        <BulletChartRange End="150" Color="#d32f2f"></BulletChartRange>
        <BulletChartRange End="250" Color="#ffa726"></BulletChartRange>
        <BulletChartRange End="300" Color="#66bb6a"></BulletChartRange>
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

**Common Color Patterns:**

| Range Type | Hex Color | RGB | Description |
|-----------|-----------|-----|-------------|
| Poor/Bad | `#d32f2f` | rgb(211, 47, 47) | Material Red 700 |
| Satisfactory | `#ffa726` | rgb(255, 167, 38) | Material Orange 400 |
| Good/Excellent | `#66bb6a` | rgb(102, 187, 106) | Material Green 400 |

## Opacity Control

Adjust range transparency using the `Opacity` property (0.0 to 1.0):

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               Minimum="0" 
               Maximum="500" 
               Interval="100"
               Title="Sales Performance">
    <BulletChartRangeCollection>
        <BulletChartRange End="200" Color="#ff5252" Opacity="0.5"></BulletChartRange>
        <BulletChartRange End="350" Color="#ffeb3b" Opacity="0.6"></BulletChartRange>
        <BulletChartRange End="500" Color="#4caf50" Opacity="0.7"></BulletChartRange>
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

**When to use opacity:**
- Lower opacity (0.3-0.5): When ranges should be subtle background elements
- Medium opacity (0.5-0.7): Balanced visibility for most dashboards
- Higher opacity (0.7-1.0): When ranges are primary visual indicators

## Multiple Range Configurations

### Two-Range Pattern (Binary Performance)

For simple pass/fail or below/above scenarios:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@ComplianceData" 
               ValueField="Score" 
               TargetField="Threshold" 
               Minimum="0" 
               Maximum="100" 
               Title="Compliance Score">
    <BulletChartRangeCollection>
        <BulletChartRange End="70" Color="#e57373"></BulletChartRange>
        <BulletChartRange End="100" Color="#81c784"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class ComplianceMetric
    {
        public double Score { get; set; }
        public double Threshold { get; set; }
    }
    
    public List<ComplianceMetric> ComplianceData = new List<ComplianceMetric>
    {
        new ComplianceMetric { Score = 85, Threshold = 70 }
    };
}
```

### Four-Range Pattern (Detailed Gradation)

For more nuanced performance levels:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               ValueField="Rating" 
               TargetField="Expected" 
               Minimum="0" 
               Maximum="100" 
               Interval="25"
               Title="Employee Performance Rating">
    <BulletChartRangeCollection>
        <BulletChartRange End="25" Color="#c62828"></BulletChartRange>
        <BulletChartRange End="50" Color="#f57c00"></BulletChartRange>
        <BulletChartRange End="75" Color="#fdd835"></BulletChartRange>
        <BulletChartRange End="100" Color="#43a047"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class PerformanceMetric
    {
        public double Rating { get; set; }
        public double Expected { get; set; }
    }
    
    public List<PerformanceMetric> PerformanceData = new List<PerformanceMetric>
    {
        new PerformanceMetric { Rating = 82, Expected = 75 }
    };
}
```

### Five-Range Pattern (Fine-Grained Assessment)

For very detailed performance categories:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@QualityData" 
               ValueField="QualityScore" 
               TargetField="Benchmark" 
               Minimum="0" 
               Maximum="100" 
               Interval="20"
               Title="Product Quality Score">
    <BulletChartRangeCollection>
        <BulletChartRange End="20" Color="#b71c1c"></BulletChartRange>
        <BulletChartRange End="40" Color="#e65100"></BulletChartRange>
        <BulletChartRange End="60" Color="#f9a825"></BulletChartRange>
        <BulletChartRange End="80" Color="#7cb342"></BulletChartRange>
        <BulletChartRange End="100" Color="#2e7d32"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class QualityMetric
    {
        public double QualityScore { get; set; }
        public double Benchmark { get; set; }
    }
    
    public List<QualityMetric> QualityData = new List<QualityMetric>
    {
        new QualityMetric { QualityScore = 88, Benchmark = 80 }
    };
}
```

## Best Practices for Range Design

### 1. Range Count Guidelines

| Number of Ranges | Use Case | Example |
|------------------|----------|---------|
| 2 ranges | Binary outcomes | Pass/Fail, Above/Below target |
| 3 ranges | Standard KPIs | Poor, Satisfactory, Good |
| 4 ranges | Detailed assessment | Critical, Warning, Acceptable, Excellent |
| 5+ ranges | Fine-grained metrics | Rating scales, quality grades |

### 2. Color Selection

**Traffic Light Pattern (Most Common):**
```razor
<BulletChartRangeCollection>
    <BulletChartRange End="40" Color="#d32f2f"></BulletChartRange>   <!-- Red -->
    <BulletChartRange End="70" Color="#ffa726"></BulletChartRange>   <!-- Orange -->
    <BulletChartRange End="100" Color="#66bb6a"></BulletChartRange>  <!-- Green -->
</BulletChartRangeCollection>
```

**Monochromatic Gradient (Subtle):**
```razor
<BulletChartRangeCollection>
    <BulletChartRange End="33" Color="#ffcdd2"></BulletChartRange>   <!-- Light red -->
    <BulletChartRange End="66" Color="#e57373"></BulletChartRange>   <!-- Medium red -->
    <BulletChartRange End="100" Color="#d32f2f"></BulletChartRange>  <!-- Dark red -->
</BulletChartRangeCollection>
```

### 3. Range Boundaries

Align range boundaries with meaningful thresholds:

```razor
<!-- Good: Boundaries match business logic -->
<BulletChartRangeCollection>
    <BulletChartRange End="60"></BulletChartRange>   <!-- Below minimum acceptable -->
    <BulletChartRange End="80"></BulletChartRange>   <!-- Meeting expectations -->
    <BulletChartRange End="100"></BulletChartRange>  <!-- Exceeding expectations -->
</BulletChartRangeCollection>

<!-- Avoid: Arbitrary or uneven boundaries -->
<BulletChartRangeCollection>
    <BulletChartRange End="37"></BulletChartRange>
    <BulletChartRange End="63"></BulletChartRange>
    <BulletChartRange End="100"></BulletChartRange>
</BulletChartRangeCollection>
```

### 4. Accessibility Considerations

Ensure color choices work for colorblind users:

```razor
@using Syncfusion.Blazor.Charts

<!-- Good: Uses distinct hues and opacity -->
<SfBulletChart DataSource="@Data" ValueField="Value" TargetField="Target">
    <BulletChartRangeCollection>
        <BulletChartRange End="40" Color="#d32f2f" Opacity="0.6"></BulletChartRange>
        <BulletChartRange End="70" Color="#ffa726" Opacity="0.7"></BulletChartRange>
        <BulletChartRange End="100" Color="#1976d2" Opacity="0.8"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>
```

## Common Patterns

### Revenue/Sales Tracking

```razor
<BulletChartRangeCollection>
    <BulletChartRange End="150" Color="#d32f2f"></BulletChartRange>  <!-- Below break-even -->
    <BulletChartRange End="250" Color="#ffa726"></BulletChartRange>  <!-- Profitable -->
    <BulletChartRange End="300" Color="#66bb6a"></BulletChartRange>  <!-- Exceeding goals -->
</BulletChartRangeCollection>
```

### Customer Satisfaction

```razor
<BulletChartRangeCollection>
    <BulletChartRange End="60" Color="#e57373"></BulletChartRange>   <!-- Poor -->
    <BulletChartRange End="80" Color="#fff176"></BulletChartRange>   <!-- Average -->
    <BulletChartRange End="100" Color="#81c784"></BulletChartRange>  <!-- Excellent -->
</BulletChartRangeCollection>
```

### Project Progress

```razor
<BulletChartRangeCollection>
    <BulletChartRange End="25" Color="#c62828"></BulletChartRange>   <!-- Critical delay -->
    <BulletChartRange End="50" Color="#f57c00"></BulletChartRange>   <!-- Behind schedule -->
    <BulletChartRange End="75" Color="#fdd835"></BulletChartRange>   <!-- On track -->
    <BulletChartRange End="100" Color="#43a047"></BulletChartRange>  <!-- Ahead -->
</BulletChartRangeCollection>
```