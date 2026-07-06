---
name: syncfusion-blazor-sparkline-charts
description: Implement Syncfusion Blazor Sparkline Charts for compact, inline trend visualizations in Blazor applications. Use this when creating mini charts, KPI indicators, dashboard sparklines, or word-sized data visualizations. This skill covers line, area, column, WinLoss, and pie sparklines, including data labels, markers, special points (high/low/negative), range bands, tooltips, and appearance customization. Ideal for dashboards, data tables with inline trends, email reports, and mobile data visualization scenarios requiring compact trend indicators.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
  category: "Data Visualization"
---

# Implementing Sparkline Charts

**NuGet:** `Syncfusion.Blazor.Charts` + `Syncfusion.Blazor.Themes` (or `Syncfusion.Blazor.Sparkline` for individual package)
**Namespace:** `Syncfusion.Blazor.Charts`

## When to Use This Skill

Use this skill when you need to guide the user to implement **Syncfusion Blazor Sparkline Charts** in their application. Sparklines are small, word-sized charts designed to show trends in data at a glance - perfect for inline visualization within text, tables, or dashboards.

**Use this when the user:**
- Wants to add inline trend charts to dashboards or tables
- Needs compact KPI indicators with visual trends
- Asks about small charts, mini charts, or micro charts
- Wants to visualize trends without full chart axes/labels
- Needs data-dense visualizations for limited space
- Asks about sparkline, line trends, or inline data visualization
- Wants to add visual indicators to email reports or mobile views

## Component Overview

**Sparkline Charts** are specialized data visualizations optimized for small spaces. Unlike full-featured charts, sparklines:
- Display trends without axes, legends, or extensive labels
- Fit inline within text or table cells
- Show data patterns at a glance
- Support multiple chart types (line, area, column, WinLoss, pie)
- Highlight special data points (high, low, negative, start, end)
- Provide tooltips for detailed information on hover

**Perfect for:** KPI dashboards, data tables, financial reports, analytics summaries, mobile interfaces, email reports, and any scenario requiring compact trend visualization.

## Documentation and Navigation Guide

This skill uses progressive disclosure. The main **SKILL.md** (this file) provides overview and navigation. Read the appropriate reference files based on what the user needs:

### API Reference

📄 **Read:** [references/api-reference.md](references/api-reference.md)
- Complete `SfSparkline` property, method, and event surface
- Supported child components and nested tags
- Enum and value-type reference
- Compact example for quick implementation

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup
- Basic sparkline implementation
- CSS theme imports
- First working example
- WebAssembly vs Server setup differences

### Chart Types and Visualization

📄 **Read:** [references/chart-types.md](references/chart-types.md)
- Line sparkline (default, shows continuous trends)
- Area sparkline (filled line chart)
- Column sparkline (vertical bars)
- WinLoss sparkline (binary win/loss indicators)
- Pie sparkline (proportional segments)
- When to use each type
- Type-specific properties and examples

📄 **Read:** [references/data-binding.md](references/data-binding.md)
- DataSource property configuration
- Array and list data binding
- XName and YName field mapping
- Value types and formatting
- Dynamic data updates
- Data transformation patterns

### Data Presentation

📄 **Read:** [references/markers-and-data-labels.md](references/markers-and-data-labels.md)
- Marker configuration (shapes, sizes, colors)
- Data label visibility and formatting
- Edge label handling
- Label templates
- Marker visibility for specific points

📄 **Read:** [references/special-points-customization.md](references/special-points-customization.md)
- Highlighting start and end points
- High point customization
- Low point customization
- Negative point styling
- Color and size configuration for special points

📄 **Read:** [references/range-bands.md](references/range-bands.md)
- Range band configuration (horizontal threshold zones)
- Start and end range values
- Color and opacity customization
- Multiple range bands
- Use cases: targets, thresholds, acceptable ranges

### Customization and Styling

📄 **Read:** [references/axis-customization.md](references/axis-customization.md)
- Axis line visibility and styling
- Min and max value configuration
- Value display modes
- Axis label formatting
- Grid line customization

📄 **Read:** [references/dimensions-and-appearance.md](references/dimensions-and-appearance.md)
- Width and height configuration
- Padding and margins
- Border styling
- Background and fill colors
- Line width customization
- RTL (right-to-left) support
- Container area styling

### Interactivity and Events

📄 **Read:** [references/user-interaction-and-events.md](references/user-interaction-and-events.md)
- Tooltip configuration and templates
- Tooltip formatting
- Tracking line for hover
- Component events (`OnLoaded`, `OnPointRendering`, `OnSeriesRendering`, `OnMarkerRendering`, `OnDataLabelRendering`, `OnPointRegionMouseClick`, `OnResizing`, `OnAxisRendering`)
- Method: `RefreshAsync()`
- User interaction patterns

### Globalization and Accessibility

📄 **Read:** [references/globalization-and-accessibility.md](references/globalization-and-accessibility.md)
- Internationalization and localization
- Number format customization
- Currency and date formatting
- RTL support
- WCAG compliance
- Keyboard navigation
- Screen reader support

## Quick Start Example

Here's a minimal sparkline showing sales trends:

```razor
@page "/sparkline-demo"
@using Syncfusion.Blazor.Charts

<h3>Monthly Sales Trend</h3>

<SfSparkline DataSource="@SalesData" 
              XName="Month" 
              YName="Sales"
              Type="SparklineType.Line"
              Height="50px" 
              Width="200px"
              Fill="#3366cc"
              LineWidth="2"  TValue="SalesInfo">
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }" 
                             Size="4" 
                             Fill="#ffffff" 
                             Border="new SparklineMarkerBorder { Color = "#3366cc", Width = 1 }">
    </SparklineMarkerSettings>
    <SparklineTooltipSettings  TValue="SalesInfo" Visible="true" Format="${Month}: ${Sales}"></SparklineTooltipSettings>
</SfSparkline>

@code {
    public class SalesInfo
    {
        public string Month { get; set; }
        public double Sales { get; set; }
    }

    public List<SalesInfo> SalesData = new List<SalesInfo>
    {
        new SalesInfo { Month = "Jan", Sales = 35000 },
        new SalesInfo { Month = "Feb", Sales = 28000 },
        new SalesInfo { Month = "Mar", Sales = 34000 },
        new SalesInfo { Month = "Apr", Sales = 32000 },
        new SalesInfo { Month = "May", Sales = 40000 },
        new SalesInfo { Month = "Jun", Sales = 32000 },
        new SalesInfo { Month = "Jul", Sales = 35000 }
    };
}
```

**Result:** A compact line chart showing monthly sales trends with markers and tooltips.

## Common Patterns

### Pattern 1: KPI Dashboard with Sparklines

When user needs a dashboard showing multiple KPIs with trend indicators:

```razor
@page "/sparkline-demo"
@using Syncfusion.Blazor.Charts
<div class="kpi-grid">
    <div class="kpi-card">
        <h4>Revenue</h4>
        <p class="value">$1.2M <span class="change positive">+15%</span></p>
        <SfSparkline DataSource="@RevenueData" Type="SparklineType.Area" Height="40px" Width="150px" Fill="#28a745">
        </SfSparkline>
    </div>
    
    <div class="kpi-card">
        <h4>Orders</h4>
        <p class="value">5,432 <span class="change negative">-3%</span></p>
        <SfSparkline DataSource="@OrdersData" Type="SparklineType.Column" Height="40px" Width="150px" Fill="#dc3545">
        </SfSparkline>
    </div>
</div>

@code {
    public List<double> RevenueData { get; set; } = new()
    {
        820000, 900000, 870000, 960000, 1100000, 1200000
    };

    public List<double> OrdersData { get; set; } = new()
    {
        5200, 5400, 5600, 5300, 5500, 5432
    };
}
```

**Guide user to:** Use Area type for cumulative metrics, Column for discrete counts, and configure special points to highlight highs/lows.

### Pattern 2: Data Table with Inline Trends

When user wants to add sparklines to table cells:

```razor
@page "/sparkline-demo"
@using Syncfusion.Blazor.Charts
<table class="data-table">
    <thead>
        <tr>
            <th>Product</th>
            <th>Current</th>
            <th>Trend (7 days)</th>
        </tr>
    </thead>
    <tbody>
        @foreach (var product in Products)
        {
            <tr>
                <td>@product.Name</td>
                <td>@product.CurrentValue.ToString("C")</td>
                <td>
                    <SfSparkline DataSource="@product.TrendData" 
                                  Type="SparklineType.Line" 
                                  Height="30px" 
                                  Width="100px"
                                  LineWidth="1">
                    </SfSparkline>
                </td>
            </tr>
        }
    </tbody>
</table>
@code {
    public class ProductInfo
    {
        public string Name { get; set; }
        public decimal CurrentValue { get; set; }
        public List<double> TrendData { get; set; } = new();
    }

    public List<ProductInfo> Products { get; set; } = new()
    {
        new ProductInfo
        {
            Name = "Product A",
            CurrentValue = 1250.75m,
            TrendData = new List<double> { 1200, 1220, 1235, 1240, 1245, 1250, 1251 }
        },
        new ProductInfo
        {
            Name = "Product B",
            CurrentValue = 980.30m,
            TrendData = new List<double> { 950, 955, 960, 970, 975, 980, 980 }
        }
    };
}
```

**Guide user to:** Keep sparklines minimal (no markers, no labels), use consistent dimensions across rows, and enable tooltips for details.

### Pattern 3: Win/Loss Indicator

When user needs binary success/failure visualization:

```razor
@page "/sparkline-demo"
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@WinLossData" 
              Type="SparklineType.WinLoss"
              Height="50px" 
              Width="300px"
              TiePointColor="#ffc107">
    <SparklineAxisSettings MinY="-1" MaxY="1"></SparklineAxisSettings>
</SfSparkline>
```

**Guide user to:** Use WinLoss type with values of 1 (win), -1 (loss), and 0 (tie). Configure TiePointColor for neutral outcomes.

### Pattern 4: Highlighting Special Points

When user wants to emphasize specific data points:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@TemperatureData" 
              Type="SparklineType.Line"
              Height="60px" 
              Width="250px">
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.High, VisibleType.Low }">
    </SparklineMarkerSettings>
    <SparklineHighPointColor>#28a745</SparklineHighPointColor>
    <SparklineLowPointColor>#dc3545</SparklineLowPointColor>
</SfSparkline>

@code {
    public List<int> WinLossData { get; set; } = new()
    {
        1, 1, -1, 0, 1, -1, 1, 1, 0, -1
    };
}
```

**Guide user to:** Use VisibleType enumeration to show markers only for High/Low/Start/End/Negative points. Configure colors for visual emphasis.

## API Surface Reference

Use the API reference file for the authoritative `SfSparkline` surface. The most important members are:

**Component properties:**
- `DataSource`, `EnableGroupingSeparator`, `EnableRtl`, `EndPointColor`, `Fill`, `Format`, `Height`, `HighPointColor`, `ID`, `LineWidth`, `LowPointColor`, `NegativePointColor`, `Opacity`, `Palette`, `Query`, `RangePadding`, `StartPointColor`, `Theme`, `TiePointColor`, `Type`, `ValueType`, `Width`, `XName`, `YName`

**Methods:**
- `RefreshAsync()`

**Events:**
- `OnAxisRendering`, `OnDataLabelRendering`, `OnLoaded`, `OnMarkerRendering`, `OnPointRegionMouseClick`, `OnPointRendering`, `OnResizing`, `OnSeriesRendering`

**Child components:**
- `SparklineAxisLineSettings`, `SparklineAxisSettings`, `SparklineBorder`, `SparklineContainerArea`, `SparklineContainerAreaBorder`, `SparklineDataLabelBorder`, `SparklineDataLabelOffset`, `SparklineDataLabelSettings`, `SparklineEvents`, `SparklineFont`, `SparklineMarkerBorder`, `SparklineMarkerSettings`, `SparklinePadding`, `SparklineRangeBand`, `SparklineRangeBandSettings`, `SparklineTooltipBorder`, `SparklineTooltipSettings`, `SparklineTooltipTextStyle`, `SparklineTrackLineSettings`

**Enums:**
- `SparklineRangePadding`, `SparklineType`, `SparklineValueType`, `VisibleType`

## Common Use Cases

### Use Case 1: Financial Dashboard
User wants to show stock prices, portfolio performance, or financial KPIs with inline trends. **Guide to:** Line or Area sparklines with special point highlighting for highs/lows.

### Use Case 2: Analytics Dashboard
User needs compact visualizations for page views, user engagement, or conversion rates. **Guide to:** Column sparklines for discrete metrics, Area for cumulative data.

### Use Case 3: Reporting Tables
User wants to add visual trends to data tables without cluttering the layout. **Guide to:** Minimal Line sparklines (30-40px height) with tooltips enabled.

### Use Case 4: Email Reports
User needs charts that render well in email clients with size constraints. **Guide to:** Simple sparklines without complex styling, fixed dimensions.

### Use Case 5: Mobile Dashboards
User requires data visualization optimized for small screens. **Guide to:** Compact sparklines with touch-friendly tooltips, responsive dimensions.

### Use Case 6: Performance Monitoring
User wants to visualize server metrics, response times, or system health. **Guide to:** Line sparklines with range bands for threshold zones (green/yellow/red).

### Use Case 7: E-commerce Analytics
User needs product performance trends, sales velocity, or inventory indicators. **Guide to:** Column sparklines for sales counts, WinLoss for stock status.

## Implementation Tips

**When guiding users:**

1. **Start Simple:** Begin with basic DataSource and Type, add features incrementally
2. **Choose Type Based on Data:** Line for continuous trends, Column for discrete counts, WinLoss for binary outcomes
3. **Keep It Minimal:** Sparklines work best with minimal styling - avoid clutter
4. **Enable Tooltips:** Since sparklines lack axes, tooltips provide essential details
5. **Consider Context:** Match sparkline size to surrounding content (inline vs. standalone)
6. **Use Special Points:** Highlight meaningful data points (high/low) for quick insights
7. **Consistent Sizing:** Use uniform dimensions when displaying multiple sparklines
8. **Test Responsiveness:** Ensure sparklines scale appropriately on different screen sizes

**Read the appropriate reference files above based on the specific features the user needs to implement.**
