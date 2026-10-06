# Multiple Doughnut Charts

## Table of Contents

- [Overview](#overview)
- [When to Use Multiple Doughnut Charts](#when-to-use-multiple-doughnut-charts)
- [How Nested Rings Work](#how-nested-rings-work)
- [Required Properties](#required-properties)
- [Basic Multiple Doughnut Example](#basic-multiple-doughnut-example)
- [Legend Grouping with MappingKey](#legend-grouping-with-mappingkey)
- [Customizing Each Series](#customizing-each-series)
- [Three or More Rings](#three-or-more-rings)
- [Tooltips for Nested Rings](#tooltips-for-nested-rings)
- [Data Labels for Nested Rings](#data-labels-for-nested-rings)
- [Disabling Animation](#disabling-animation)
- [Common Pitfalls and Troubleshooting](#common-pitfalls-and-troubleshooting)
- [Complete Production Example](#complete-production-example)

---

## Overview

The Multiple Doughnut feature renders two or more concentric doughnut rings within a single `SfAccumulationChart`. Each ring is a separate `AccumulationChartSeries` driven by its own data source, colors, and labels. This lets you compare multiple datasets across the same categories side-by-side in one compact visual — for example, plotting total sales on the outer ring and total profit on the inner ring.

---

## When to Use Multiple Doughnut Charts

- **Comparing related measures by category** — e.g., total sales vs. profit per product line
- **Showing actual vs. target values** for the same set of categories
- **Visualizing two metrics per category** without a second chart
- **Comparing current period vs. previous period** across shared category names

Avoid when:
- The categories differ between the datasets (legend grouping loses meaning)
- Either dataset has more than ~7 categories (rings become unreadable)
- You need precise value comparison (a bar chart is clearer)

---

## How Nested Rings Work

Each `AccumulationChartSeries` is drawn at a `Radius` (outer edge, relative to chart size) and `InnerRadius` (hole size). To create a clean nested effect:

- **Outer ring:** larger `Radius` with a non-zero `InnerRadius` (a true ring)
- **Inner ring:** smaller `Radius` with `InnerRadius` equal to `Radius` (a solid disc, no hole)

The chart draws series in document order, so declare the outermost series first and the innermost last.

> **Why `AccumulationType.Pie` and not `Doughnut`?** Each ring is a pie series whose `InnerRadius` carves out the hole, turning it into a ring. `InnerRadius` on `AccumulationType.Pie` gives precise control over both the outer edge (`Radius`) and the hole size per series — the two knobs needed to stack rings without overlap. `AccumulationType.Doughnut` applies a single global hole to one series and does not support the per-series `Radius`/`InnerRadius` pairing required for nested rings, so `Pie` is the correct type for every series in a multiple-doughnut layout.

```
            ┌─────────────┐   ← Outer series: Radius=90%, InnerRadius=60%
            │   ┌─────┐   │
            │   │  ●  │   │   ← Inner series: Radius=50%, InnerRadius=50%
            │   └─────┘   │
            └─────────────┘
```

---

## Required Properties

| Property | Component | Purpose |
|----------|-----------|---------|
| `Type` | `AccumulationChartSeries` | Set to `AccumulationType.Pie` for every ring |
| `Radius` | `AccumulationChartSeries` | Outer edge size, e.g. "90%" or "50%" |
| `InnerRadius` | `AccumulationChartSeries` | Hole size; non-zero for ring, equal to `Radius` for solid disc |
| `DataSource` | `AccumulationChartSeries` | Per-series data source |
| `XName` | `AccumulationChartSeries` | Category field, shared across series for legend grouping |
| `YName` | `AccumulationChartSeries` | Value field |
| `Name` | `AccumulationChartSeries` | Display name for the series |
| `MappingKey` | `AccumulationChartLegendSettings` | Legend grouping field; should match `XName` |

---

## Basic Multiple Doughnut Example

```razor
@using Syncfusion.Blazor.Charts

<SfAccumulationChart Title="Product Sales vs Profit Analysis">
    <AccumulationChartSeriesCollection>
        <AccumulationChartSeries DataSource="@TotalSalesData"
                                 XName="@nameof(ProductData.X)"
                                 YName="@nameof(ProductData.Y)"
                                 Name="Total Sales"
                                 Type="@AccumulationType.Pie"
                                 Radius="90%"
                                 InnerRadius="60%">

            <AccumulationDataLabelSettings Visible="true"
                                            Name="@nameof(ProductData.Text)"
                                            Position="@AccumulationLabelPosition.Outside">
                <AccumulationChartConnector Type="@ConnectorType.Curve" Color="black" Width="2" DashArray="2,1" Length="5" />
            </AccumulationDataLabelSettings>

            <AccumulationChartAnimation Enable="false" />
        </AccumulationChartSeries>

        <AccumulationChartSeries DataSource="@TotalProfitData"
                                 XName="@nameof(ProductData.X)"
                                 YName="@nameof(ProductData.Y)"
                                 Name="Total Profit"
                                 Type="@AccumulationType.Pie"
                                 Radius="50%"
                                 InnerRadius="50%">

            <AccumulationDataLabelSettings Visible="true"
                                            Name="@nameof(ProductData.Text)"
                                            Position="@AccumulationLabelPosition.Inside">
                <!-- Connectors only draw leader lines for Outside labels; omit AccumulationChartConnector for Inside labels. -->
            </AccumulationDataLabelSettings>
            <AccumulationChartAnimation Enable="false" />
        </AccumulationChartSeries>
    </AccumulationChartSeriesCollection>

    <AccumulationChartLegendSettings Visible="true" MappingKey="@nameof(ProductData.X)" />
</SfAccumulationChart>

@code {
    private List<ProductData> TotalSalesData { get; set; } = new()
    {
        new() { X = "Electronics",  Y = 45000, Text = "45K" },
        new() { X = "Fashion",      Y = 32000, Text = "32K" },
        new() { X = "Home & Garden", Y = 18000, Text = "18K" },
        new() { X = "Sports",       Y = 15000, Text = "15K" },
        new() { X = "Books",        Y = 8000,  Text = "8K" }
    };

    private List<ProductData> TotalProfitData { get; set; } = new()
    {
        new() { X = "Electronics",  Y = 18000, Text = "18K" },
        new() { X = "Fashion",      Y = 12800, Text = "12.8K" },
        new() { X = "Home & Garden", Y = 6300, Text = "6.3K" },
        new() { X = "Sports",       Y = 4500, Text = "4.5K" },
        new() { X = "Books",        Y = 2400, Text = "2.4K" }
    };

    public class ProductData
    {
        public string X { get; set; } = string.Empty;
        public double Y { get; set; }
        public string Text { get; set; } = string.Empty;
    }
}
```

---

## Legend Grouping with MappingKey

When multiple series share the same category names, the default legend shows one entry per point per series, which duplicates categories and clutters the legend. `AccumulationChartLegendSettings.MappingKey` fixes this by grouping legend items by a shared field from the data source. Points with matching `MappingKey` values collapse into a single legend entry.

```razor
<AccumulationChartLegendSettings Visible="true" MappingKey="@nameof(ProductData.X)" />
```

**Rules:**
- `MappingKey` must reference the same data field used as `XName` on every series (typically the category name)
- All series should share the same set of category values in their data sources
- Toggling a grouped legend item hides that category in every series at once

Without `MappingKey`, each ring contributes its own legend entry for every category, producing redundant rows.

---

## Customizing Each Series

Each `AccumulationChartSeries` supports independent color, label, border, and tooltip configuration:

```razor
<SfAccumulationChart Title="Actual vs Target">
    <AccumulationChartSeriesCollection>
        <!-- Outer ring: actual values -->
        <AccumulationChartSeries DataSource="@Actual"
                                 XName="Category"
                                 YName="Value"
                                 Name="Actual"
                                 Type="@AccumulationType.Pie"
                                 Radius="90%"
                                 InnerRadius="55%"
                                 PointColorMapping="Color">
            <AccumulationChartSeriesBorder Width="2" Color="#FFFFFF"></AccumulationChartSeriesBorder>
        </AccumulationChartSeries>

        <!-- Inner ring: target values -->
        <AccumulationChartSeries DataSource="@Target"
                                 XName="Category"
                                 YName="Value"
                                 Name="Target"
                                 Type="@AccumulationType.Pie"
                                 Radius="50%"
                                 InnerRadius="50%"
                                 PointColorMapping="Color">
        </AccumulationChartSeries>
    </AccumulationChartSeriesCollection>

    <AccumulationChartLegendSettings Visible="true" MappingKey="Category" />
</SfAccumulationChart>
```

Per-series options commonly customized independently:
- `PointColorMapping` — map slice colors from a data field
- `AccumulationChartSeriesBorder` — child component controlling the width/color of the divider between slices
- `TooltipMappingName` — field shown in tooltip header
- `StartAngle` / `EndAngle` — apply the same arc range to all rings for consistency

---

## Three or More Rings

You can stack more than two rings by adding additional series. Keep decreasing `Radius` for each inner ring and maintain consistent gaps:

```razor
<AccumulationChartSeriesCollection>
    <!-- Outermost ring -->
    <AccumulationChartSeries DataSource="@Q4" XName="Category" YName="Value" Name="Q4"
                              Type="@AccumulationType.Pie" Radius="95%" InnerRadius="75%" />
    <!-- Middle ring -->
    <AccumulationChartSeries DataSource="@Q3" XName="Category" YName="Value" Name="Q3"
                              Type="@AccumulationType.Pie" Radius="70%" InnerRadius="50%" />
    <!-- Innermost disc -->
    <AccumulationChartSeries DataSource="@Q2" XName="Category" YName="Value" Name="Q2"
                              Type="@AccumulationType.Pie" Radius="45%" InnerRadius="45%" />
</AccumulationChartSeriesCollection>

<AccumulationChartLegendSettings Visible="true" MappingKey="Category" />
```

**Guidance:**
- Avoid more than three rings; readability drops quickly
- Keep the innermost series as a solid disc (`InnerRadius` = `Radius`)
- Maintain a visible gap (~5–15% depending on ring count) between adjacent ring edges by tuning `Radius`/`InnerRadius` pairs — narrower gaps (~5%) suit three-ring layouts where space is tight; wider gaps (~10–15%) work well for two rings

---

## Tooltips for Nested Rings

Tooltips show per-point values. Use `TooltipMappingName` on each series to identify the point, and a shared `AccumulationChartTooltipSettings.Format` for consistent display:

```razor
<SfAccumulationChart>
    <AccumulationChartSeriesCollection>
        <AccumulationChartSeries DataSource="@Sales"
                                 XName="@nameof(ProductData.X)"
                                 YName="@nameof(ProductData.Y)"
                                 TooltipMappingName="@nameof(ProductData.X)"
                                 Radius="90%" InnerRadius="60%"
                                 Type="@AccumulationType.Pie" />
        <AccumulationChartSeries DataSource="@Profit"
                                 XName="@nameof(ProductData.X)"
                                 YName="@nameof(ProductData.Y)"
                                 TooltipMappingName="@nameof(ProductData.X)"
                                 Radius="50%" InnerRadius="50%"
                                 Type="@AccumulationType.Pie" />
    </AccumulationChartSeriesCollection>

    <AccumulationChartTooltipSettings Enable="true"
        Format="<b>${point.x}</b><br/>Value: <b>${point.y}</b><br/>Percentage: <b>${point.percentage}%</b>" />
</SfAccumulationChart>
```

The `Format` string uses `point.x`, `point.y`, and `point.percentage`. Each series renders its own tooltip for the hovered point.

---

## Data Labels for Nested Rings

Place outer-ring labels outside and inner-ring labels inside to avoid overlap:

```razor
<AccumulationChartSeries DataSource="@Outer" XName="X" YName="Y"
                         Radius="90%" InnerRadius="60%" Type="@AccumulationType.Pie">
    <AccumulationDataLabelSettings Visible="true" Position="@AccumulationLabelPosition.Outside">
        <AccumulationChartConnector Length="5" />
    </AccumulationDataLabelSettings>
</AccumulationChartSeries>

<AccumulationChartSeries DataSource="@Inner" XName="X" YName="Y"
                         Radius="50%" InnerRadius="50%" Type="@AccumulationType.Pie">
    <AccumulationDataLabelSettings Visible="true" Position="@AccumulationLabelPosition.Inside">
    </AccumulationDataLabelSettings>
</AccumulationChartSeries>
```

Use `Name` to map custom label text from a data field (e.g., abbreviated values like "45K").

> **`AccumulationChartConnector` only applies to `Outside` labels.** Leader lines are drawn from the slice edge to the label, which only happens when `Position` is `AccumulationLabelPosition.Outside`. Including `AccumulationChartConnector` on an `Inside`-positioned label has no visible effect and should be omitted to avoid confusing readers who copy-paste the snippet. Both the basic and production examples in this file follow this rule. `DashArray="2,1"` is used on the connector across all examples for a consistent dashed leader-line appearance.

---

## Disabling Animation

Individual ring animations can look jittery when multiple series animate at once. Disable animation per series for instant rendering:

```razor
<AccumulationChartSeries …>
    <AccumulationChartAnimation Enable="false" />
</AccumulationChartSeries>
```

Apply to every series in a multi-ring chart for a clean static load.

---

## Common Pitfalls and Troubleshooting

| Symptom | Cause | Fix |
|---------|-------|-----|
| Rings overlap / appear as a single shape | `InnerRadius` of outer ring is too small | Ensure outer `InnerRadius` ≥ inner `Radius` so rings don't share space |
| Inner ring renders as a ring instead of a solid disc | `InnerRadius` < `Radius` on innermost series | Set innermost `InnerRadius` equal to its `Radius` |
| Duplicate legend entries per category | `MappingKey` not set on `AccumulationChartLegendSettings` | Add `MappingKey` matching the `XName` field |
| Legend toggle hides only one ring's slice | Series use different `XName` field or value casing | Use the same category field and values across all series |
| Labels overlap between rings | Both rings use `Position="Outside"` | Use `Inside` for the inner ring |
| Labels distorted or cut off | Inner disc too small | Increase the inner `Radius` (e.g., from "40%" to "50%") |
| Animation looks broken with multiple series | Per-series animations stagger | Set `<AccumulationChartAnimation Enable="false" />` on each series |

---

## Complete Production Example

A two-ring nested doughnut comparing total sales and profit with grouped legend, tooltips, formatted labels, and disabled animation:

```razor
@page "/multiple-doughnut"
@using Syncfusion.Blazor.Charts

<SfAccumulationChart Title="Product Sales vs Profit Analysis">
    <AccumulationChartSeriesCollection>
        <AccumulationChartSeries DataSource="@TotalSalesData"
                                 XName="@nameof(ProductData.X)"
                                 YName="@nameof(ProductData.Y)"
                                 Name="Total Sales"
                                 Type="@AccumulationType.Pie"
                                 Radius="90%"
                                 InnerRadius="60%"
                                 TooltipMappingName="@nameof(ProductData.X)">

            <AccumulationDataLabelSettings Visible="true"
                                            Name="@nameof(ProductData.Text)"
                                            Position="@AccumulationLabelPosition.Outside">
                <AccumulationChartConnector Type="@ConnectorType.Curve" Color="black" Width="2" DashArray="2,1" Length="5" />
            </AccumulationDataLabelSettings>
            <AccumulationChartAnimation Enable="false" />
        </AccumulationChartSeries>

        <AccumulationChartSeries DataSource="@TotalProfitData"
                                 XName="@nameof(ProductData.X)"
                                 YName="@nameof(ProductData.Y)"
                                 Name="Total Profit"
                                 Type="@AccumulationType.Pie"
                                 Radius="50%"
                                 InnerRadius="50%"
                                 TooltipMappingName="@nameof(ProductData.X)">
            <AccumulationDataLabelSettings Visible="true"
                                            Name="@nameof(ProductData.Text)"
                                            Position="@AccumulationLabelPosition.Inside">
                <!-- Connectors only draw leader lines for Outside labels; omit AccumulationChartConnector for Inside labels. -->
            </AccumulationDataLabelSettings>
            <AccumulationChartAnimation Enable="false" />
        </AccumulationChartSeries>
    </AccumulationChartSeriesCollection>

    <AccumulationChartTooltipSettings Enable="true"
        Format="<b>${point.x}</b><br/>Value: <b>${point.y}</b><br/>Percentage: <b>${point.percentage}%</b>" />

    <AccumulationChartLegendSettings Visible="true" MappingKey="@nameof(ProductData.X)" />

    <AccumulationChartBorder Color="#333" Width="2" />
</SfAccumulationChart>

@code {
    private List<ProductData> TotalSalesData { get; set; } = new()
    {
        new() { X = "Electronics",  Y = 45000, Text = "45K" },
        new() { X = "Fashion",      Y = 32000, Text = "32K" },
        new() { X = "Home & Garden", Y = 18000, Text = "18K" },
        new() { X = "Sports",       Y = 15000, Text = "15K" },
        new() { X = "Books",        Y = 8000,  Text = "8K" }
    };

    private List<ProductData> TotalProfitData { get; set; } = new()
    {
        new() { X = "Electronics",  Y = 18000, Text = "18K" },
        new() { X = "Fashion",      Y = 12800, Text = "12.8K" },
        new() { X = "Home & Garden", Y = 6300, Text = "6.3K" },
        new() { X = "Sports",       Y = 4500, Text = "4.5K" },
        new() { X = "Books",        Y = 2400, Text = "2.4K" }
    };

    public class ProductData
    {
        public string X { get; set; } = string.Empty;
        public double Y { get; set; }
        public string Text { get; set; } = string.Empty;
    }
}
```
