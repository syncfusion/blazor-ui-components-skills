---
name: syncfusion-blazor-bullet-chart
description: Implement Syncfusion Blazor Bullet Chart (SfBulletChart) for KPI and performance visualization. Use this when displaying target vs actual metrics, goal tracking, or performance dashboards. This skill covers actual/target bars, qualitative ranges, and comparative analysis for KPI visualization in Blazor applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Data Visualization"
---

# Implementing Bullet Charts

**NuGet:** `Syncfusion.Blazor.Charts` + `Syncfusion.Blazor.Themes` (or `Syncfusion.Blazor.BulletChart` for individual package)
**Namespace:** `Syncfusion.Blazor.Charts`

A comprehensive skill for implementing Syncfusion Blazor Bullet Chart components for KPI and performance visualization. The Bullet Chart is a variation of bar charts designed specifically for displaying key performance indicators with actual values, target markers, and qualitative ranges in a compact space.

## When to Use This Skill

Use this skill immediately when you need to:
- Display KPI (Key Performance Indicator) dashboards
- Show actual performance vs target/goal comparisons
- Visualize metrics with qualitative ranges (Good, Average, Poor)
- Create compact performance indicators for dashboards
- Track sales targets, revenue goals, or quota achievements
- Display feature measures against comparative measures
- Build executive dashboards with multiple KPI metrics
- Show progress toward goals with context zones
- Compare current performance against benchmarks
- Visualize performance metrics in limited dashboard space
- Create data-dense visualizations for business intelligence
- Display multiple performance metrics in vertical/horizontal layouts
- Build responsive KPI cards or widgets for Blazor Server, WebAssembly, or Web App

## Component Overview

The **Syncfusion Blazor Bullet Chart** (`SfBulletChart`) is a specialized charting component optimized for KPI visualization. It presents:

- **Actual Bar (Feature Measure)**: Primary data value displayed as a horizontal/vertical bar
- **Target Bar (Comparative Measure)**: Goal marker displayed as a perpendicular line or shape
- **Qualitative Ranges**: Background zones representing performance levels (Poor, Satisfactory, Good)
- **Compact Design**: Space-efficient layout ideal for dashboards
- **Multiple Layouts**: Single or multiple bullet charts with category grouping
- **Customization**: Extensive styling, theming, and formatting options

**Key Characteristics:**
- **Bar Types**: Rect (default) or Dot for actual values
- **Target Shapes**: Rect, Circle, or Cross markers
- **Orientation**: Horizontal (default) or Vertical layouts
- **Data Binding**: Single point, multiple series, or category-based data
- **Interactive**: Tooltips, data labels, legends, and events
- **Accessible**: WCAG compliant with keyboard navigation

## Documentation and Navigation Guide

### API Reference

A concise summary of the key API surface for `SfBulletChart<TValue>` — see `references/api-reference.md` for the full reference and examples.

**SfBulletChart<TValue> — Key Parameters**
- **DataSource:** object — Data collection (List, IEnumerable, or remote `DataManager`).
- **ValueField:** string — Property name for the actual (feature) value.
- **TargetField:** string — Property name for the target/comparative value.
- **CategoryField:** string — Property name for category grouping (when multiple bullets are shown).
- **Minimum / Maximum / Interval:** double — Axis scale configuration.
- **Orientation:** `OrientationType` — `Horizontal` or `Vertical`.
- **Type:** `FeatureType` — Actual bar rendering (`Rect` or `Dot`).
- **TargetTypes:** `List<TargetType>` — Marker shapes for targets (e.g., `Rect`, `Circle`, `Cross`).
- **Width / Height / Title / Theme:** visual settings to control layout and appearance.

**Core Child Components**
- **BulletChartRange:** defines qualitative background ranges (`End`, `Color`, `Opacity`).
- **BulletChartRangeCollection:** container for multiple `BulletChartRange` entries.
- **BulletChartDataLabel:** data label options (`Enable`, `Template`, `TextStyle`).
- **BulletChartTooltip:** tooltip configuration (`Enable`, `Template`, `BulletChartTooltipTextStyle`).

**Appearance & Layout**
- **Border / Margin / MajorTickLines / MinorTickLines / CategoryLabelStyle:** axis, tick and spacing controls.
- **LegendSettings / LegendLocation / LegendTextStyle:** legend appearance when using categories or multiple bullets.

**Events**
- **Loaded / Resized:** lifecycle events to detect initialization and size changes.
- **TooltipRender / LabelRender / LegendRender / PointClick:** customization hooks for runtime interaction.

**Important Enums**
- **OrientationType:** `Horizontal` | `Vertical`.
- **TargetType:** `Rect` | `Circle` | `Cross`.
- **FeatureType:** `Rect` | `Dot`.

For full API details, method signatures, event argument classes, and quick examples, consult `references/api-reference.md`.

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

Start here for installation, setup, and your first bullet chart. Covers:
- Installing Syncfusion.Blazor.BulletChart NuGet package
- Blazor Server, WebAssembly, and Web App setup
- Service registration and theme configuration
- Basic SfBulletChart implementation with sample data
- Project structure and script references
- Troubleshooting common setup issues

### Core Chart Elements

#### Actual and Target Bars

📄 **Read:** [references/actual-target-bars.md](references/actual-target-bars.md)

Learn about the two primary chart elements:
- **Actual Bar (Feature Measure)**: ValueField mapping, Rect vs Dot types, styling
- **Target Bar (Comparative Measure)**: TargetField mapping, Circle/Cross/Rect shapes, multiple targets
- Color customization for both bars
- Width and height configuration
- Best practices for visual hierarchy

#### Qualitative Ranges

📄 **Read:** [references/ranges.md](references/ranges.md)

Configure background ranges that define performance zones:
- Range boundaries using End property
- Color and opacity customization
- Good/Satisfactory/Poor zone patterns
- Multiple range configurations
- Visual design best practices

### Data Configuration

#### Data Binding

📄 **Read:** [references/data-binding.md](references/data-binding.md)

Configure data sources for bullet charts:
- Single data point for simple KPIs
- Multiple data points with categories (vertical layout)
- CategoryField, ValueField, TargetField mapping
- Local data sources (List, Array, ExpandoObject)
- Remote data binding with SfDataManager
- Dynamic data updates and refresh patterns

### Customization and Styling

#### Axis Customization

📄 **Read:** [references/axis-customization.md](references/axis-customization.md)

Customize the quantitative scale:
- Minimum, Maximum, Interval properties
- Horizontal vs Vertical orientation
- Major and minor tick configuration
- Axis label formatting and rotation
- Opposite position for labels
- RTL (Right-to-Left) support

#### Visual Customization

📄 **Read:** [references/visual-customization.md](references/visual-customization.md)

Control appearance, themes, and layout:
- Chart dimensions (Width, Height, Container)
- Theme selection (Material, Bootstrap, Fluent, Tailwind, Fabric, Highcontrast)
- Color palette customization
- Title and subtitle configuration
- Border, margin, and padding settings
- Responsive design patterns

### Additional Features

#### Data Labels, Tooltips, and Legend

📄 **Read:** [references/data-labels-tooltips-legend.md](references/data-labels-tooltips-legend.md)

Enhance data visibility and interaction:
- **Data Labels**: Enable, format, position, and template options
- **Tooltips**: Default and custom templates, format strings
- **Legend**: Position, alignment, custom text, toggle visibility

#### Events and Accessibility

📄 **Read:** [references/events-accessibility.md](references/events-accessibility.md)

Handle user interactions and ensure accessibility:
- **Events**: OnLoad, OnChartMouseClick, OnTooltipRender, OnLegendRender
- **Accessibility**: WCAG compliance, keyboard navigation, screen readers, high contrast themes
- ARIA attributes and testing checklist

## Quick Start Example

Here's a minimal bullet chart showing revenue performance:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@RevenueData" 
               ValueField="ActualRevenue" 
               TargetField="TargetRevenue" 
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
        public double ActualRevenue { get; set; }
        public double TargetRevenue { get; set; }
    }
    
    public List<RevenueMetric> RevenueData = new List<RevenueMetric>
    {
        new RevenueMetric { ActualRevenue = 270, TargetRevenue = 250 }
    };
}
```

**What this creates:**
- Actual bar showing $270K revenue
- Target marker at $250K goal
- Three qualitative ranges: Poor (0-150), Satisfactory (150-250), Good (250-300)
- Horizontal layout with title

## Common Use Cases

### 1. KPI Dashboard with Multiple Metrics

Display multiple performance indicators in a vertical layout:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               CategoryField="Metric" 
               ValueField="Actual" 
               TargetField="Target" 
               Orientation="OrientationType.Vertical"
               Height="400"
               Minimum="0" 
               Maximum="100">
    <BulletChartRangeCollection>
        <BulletChartRange End="35" Color="#d32f2f"></BulletChartRange>
        <BulletChartRange End="70" Color="#ffa726"></BulletChartRange>
        <BulletChartRange End="100" Color="#66bb6a"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class KPIMetric
    {
        public string Metric { get; set; }
        public double Actual { get; set; }
        public double Target { get; set; }
    }
    
    public List<KPIMetric> PerformanceData = new List<KPIMetric>
    {
        new KPIMetric { Metric = "Revenue", Actual = 85, Target = 80 },
        new KPIMetric { Metric = "Profit", Actual = 65, Target = 75 },
        new KPIMetric { Metric = "Market Share", Actual = 45, Target = 60 },
        new KPIMetric { Metric = "Customer Satisfaction", Actual = 90, Target = 85 }
    };
}
```

### 2. Sales Target Tracker with Custom Styling

Track sales performance with custom colors and target marker:

```razor
@using Syncfusion.Blazor.Charts
<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               TargetTypes="new List<Syncfusion.Blazor.Charts.TargetType> { Syncfusion.Blazor.Charts.TargetType.Circle }"
               Type="FeatureType.Rect"
               Minimum="0" 
               Maximum="500" 
               Interval="100"
               Title="Q1 Sales Performance">
    <BulletChartRangeCollection>
        <BulletChartRange End="200" Color="#ff5252" Opacity="0.5"></BulletChartRange>
        <BulletChartRange End="350" Color="#ffeb3b" Opacity="0.5"></BulletChartRange>
        <BulletChartRange End="500" Color="#4caf50" Opacity="0.5"></BulletChartRange>
    </BulletChartRangeCollection>
    <BulletChartTooltip TValue="SalesMetric" Enable="true">
        <BulletChartTooltipTextStyle Color="white" FontWeight="600"></BulletChartTooltipTextStyle>
    </BulletChartTooltip>
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

### 3. Executive Dashboard Card with Data Labels

Compact KPI card for executive dashboards:

```razor
@using Syncfusion.Blazor.Charts

<div style="width: 300px; padding: 16px; border: 1px solid #e0e0e0; border-radius: 8px;">
    <SfBulletChart DataSource="@MetricData" 
                   ValueField="Value" 
                   TargetField="Goal" 
                   Minimum="0" 
                   Maximum="100" 
                   Interval="25"
                   Width="100%"
                   Height="60"
                   Title="Customer Retention Rate">
        <BulletChartRangeCollection>
            <BulletChartRange End="60" Color="#f44336"></BulletChartRange>
            <BulletChartRange End="80" Color="#ff9800"></BulletChartRange>
            <BulletChartRange End="100" Color="#4caf50"></BulletChartRange>
        </BulletChartRangeCollection>
        <BulletChartDataLabel>
            <BulletChartDataLabelStyle Color="#FFFFFF" Opacity="1" Size="15px" FontStyle="italic"></BulletChartDataLabelStyle>
        </BulletChartDataLabel>
    </SfBulletChart>
</div>

@code {
    public class Metric
    {
        public double Value { get; set; }
        public double Goal { get; set; }
    }
    
    public List<Metric> MetricData = new List<Metric>
    {
        new Metric { Value = 92, Goal = 85 }
    };
}
```

## Key Properties Reference

### Essential Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `DataSource` | object | Data collection for the chart | null |
| `ValueField` | string | Property name for actual value | null |
| `TargetField` | string | Property name for target value | null |
| `CategoryField` | string | Property for category grouping | null |
| `Minimum` | double | Minimum scale value | 0 |
| `Maximum` | double | Maximum scale value | 100 |
| `Interval` | double | Scale interval | 10 |

### Visual Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Type` | FeatureType | Actual bar type (Rect, Dot) | Rect |
| `TargetTypes` | List<TargetType> | Target marker shape | Rect |
| `Orientation` | OrientationType | Layout direction | Horizontal |
| `Width` | string | Chart width | "100%" |
| `Height` | string | Chart height | "126px" |
| `Title` | string | Chart title | "" |
| `Theme` | ChartTheme | Visual theme | Material |

### Data Display

| Property | Type | Description |
|----------|------|-------------|
| `BulletChartRangeCollection` | Component | Qualitative ranges |
| `BulletChartDataLabel` | Component | Data label configuration |
| `BulletChartTooltip` | Component | Tooltip settings |
| `BulletChartLegend` | Component | Legend configuration |

## Implementation Workflow

When implementing a bullet chart, follow this sequence:

1. **Start with [Getting Started](references/getting-started.md)** for installation and basic setup
2. **Configure data binding** from [Data Binding](references/data-binding.md) for your data structure
3. **Set up ranges** using [Ranges](references/ranges.md) to define performance zones
4. **Add actual and target bars** from [Actual and Target Bars](references/actual-target-bars.md)
5. **Customize the axis** with [Axis Customization](references/axis-customization.md) for proper scaling
6. **Apply visual styling** from [Visual Customization](references/visual-customization.md)
7. **Enhance with data labels/tooltips** from [Data Labels, Tooltips, and Legend](references/data-labels-tooltips-legend.md)
8. **Add interactivity and accessibility** using [Events and Accessibility](references/events-accessibility.md)

For questions or issues, refer to the troubleshooting sections in each reference file or consult [Events and Accessibility](references/events-accessibility.md) for compliance requirements.
