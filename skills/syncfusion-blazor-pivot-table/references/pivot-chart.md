# Pivot Chart

The Pivot Chart visualizes aggregated pivot data graphically. It supports 19+ chart types, drill-down, multiple axes, and can be shown alongside or instead of the pivot grid.

> **⚠️ Critical Tag Name — Frequently Confused:**
>
> The chart configuration tag is **`PivotChartSettings`** — NOT `PivotViewChartSettings`. Using the wrong name will cause a compile error or the chart settings to be silently ignored.
>
> ```razor
> <!-- ✅ CORRECT -->
> <PivotChartSettings Title="Sales Analysis">
>     <PivotChartSeries Type="ChartSeriesType.Column"></PivotChartSeries>
> </PivotChartSettings>
>
> <!-- ❌ WRONG — this tag does not exist -->
> <PivotViewChartSettings>...</PivotViewChartSettings>
> ```

---

## Display Options

Use `PivotViewDisplayOption` to control whether to show the grid, chart, or both.

| `View` Value | Result |
|---|---|
| `View.Table` | Only pivot grid (default) |
| `View.Chart` | Only pivot chart |
| `View.Both` | Both grid and chart |

```razor
<SfPivotView TValue="ProductDetails" Width="800" Height="500">
    <PivotViewDataSourceSettings DataSource="@data">
        <PivotViewColumns>
            <PivotViewColumn Name="Country"></PivotViewColumn>
            <PivotViewColumn Name="Products"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="Year"></PivotViewRow>
            <PivotViewRow Name="Quarter"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Sold" Caption="Unit Sold"></PivotViewValue>
            <PivotViewValue Name="Amount" Caption="Sold Amount"></PivotViewValue>
        </PivotViewValues>
        <PivotViewFormatSettings>
            <PivotViewFormatSetting Name="Amount" Format="C"></PivotViewFormatSetting>
        </PivotViewFormatSettings>
    </PivotViewDataSourceSettings>
    <PivotViewDisplayOption View="View.Chart"></PivotViewDisplayOption>
    <PivotChartSettings Title="Sales Analysis">
        <PivotChartSeries Type="ChartSeriesType.Column"></PivotChartSeries>
    </PivotChartSettings>
</SfPivotView>
```

**Show both grid and chart (chart as primary view):**
```razor
<PivotViewDisplayOption View="View.Both" Primary="Primary.Chart"></PivotViewDisplayOption>
```

---

## Chart Types

### Standard Chart Types (19)

Set via `PivotChartSeries.Type` (`ChartSeriesType` enum):

| Type | Type | Type |
|---|---|---|
| `Line` (default) | `Column` | `Area` |
| `Bar` | `StepArea` | `StackingColumn` |
| `StackingArea` | `StackingBar` | `StepLine` |
| `Pareto` | `Bubble` | `Scatter` |
| `Spline` | `SplineArea` | `StackingColumn100` |
| `StackingBar100` | `StackingArea100` | `Polar` |
| `Radar` | | |

```razor
<PivotChartSettings>
    <PivotChartSeries Type="ChartSeriesType.Bar"></PivotChartSeries>
</PivotChartSettings>
```

### Accumulation Charts (4)

| Type | Description |
|---|---|
| `ChartSeriesType.Pie` | Proportional slices |
| `ChartSeriesType.Doughnut` | Pie with center hole |
| `ChartSeriesType.Funnel` | Funnel/sales pipeline |
| `ChartSeriesType.Pyramid` | Pyramid visualization |

> **Note:** Accumulation charts only support values from a **single column**. By default the first column is used.

```razor
<PivotChartSettings Title="Sales Analysis">
    <PivotChartSeries Type="ChartSeriesType.Pie"></PivotChartSeries>
</PivotChartSettings>
```

**Select a specific column for accumulation charts:**
```razor
<PivotChartSettings
    Title="Sales Analysis"
    ColumnHeader="Germany-Road Bikes"
    ColumnDelimiter="-">
    <PivotChartSeries Type="ChartSeriesType.Doughnut"></PivotChartSeries>
</PivotChartSettings>
```

---

## Drill Down in Chart

For standard charts (non-accumulation), clicking a data point drills down into child members. The chart updates to show the next level of detail.

For accumulation charts, right-clicking shows a context menu with:
- **Expand** — drill down to child members
- **Collapse** — drill up to parent level
- **Exit** — close menu

> Accumulation chart drill operations work on **row headers only**.

---

## Multiple Value Axes

When multiple value fields exist, each gets its own Y-axis by default. Control this with `PivotChartSettings.ShowMultiLevelLabels`.

```razor
<PivotChartSettings>
    <PivotChartSeries Type="ChartSeriesType.Column"></PivotChartSeries>
    <PivotChartPrimaryYAxis>
        <PivotChartPrimaryYAxisBorder Width="0"></PivotChartPrimaryYAxisBorder>
    </PivotChartPrimaryYAxis>
</PivotChartSettings>
```

---

## `PivotChartSettings` Key Properties

| Property | Type | Description |
|---|---|---|
| `Title` | `string` | Chart title text |
| `ColumnHeader` | `string` | Column to use for accumulation charts |
| `ColumnDelimiter` | `string` | Delimiter for multi-level column headers |
| `ShowMultiLevelLabels` | `bool` | Show multi-level axis labels |
| `ShowPointColorByMembers` | `bool` | Color points by member |

---

## Chart with Field List and Grouping Bar

The chart works seamlessly with field list and grouping bar — users can rearrange fields and the chart updates automatically.

```razor
<SfPivotView TValue="ProductDetails"
             ShowFieldList="true"
             ShowGroupingBar="true">
    <PivotViewDataSourceSettings DataSource="@data">
        <!-- axis config -->
    </PivotViewDataSourceSettings>
    <PivotViewDisplayOption View="View.Chart"></PivotViewDisplayOption>
    <PivotChartSettings Title="Sales Analysis">
        <PivotChartSeries Type="ChartSeriesType.Column"></PivotChartSeries>
    </PivotChartSettings>
</SfPivotView>
```

---

## Chart Export

Export the pivot chart via toolbar or programmatically.

**Via toolbar (includes `ToolbarItems.Export`):**
```razor
<SfPivotView TValue="ProductDetails" ShowToolbar="true"
    Toolbar="@(new List<ToolbarItems> { ToolbarItems.Export })">
    <PivotViewDisplayOption View="View.Chart"></PivotViewDisplayOption>
    <!-- ... -->
</SfPivotView>
```

**Export both chart and table:**
When `View.Both` is enabled and export is triggered, both the grid and chart are exported.

---

## Toolbar Toggle (Chart / Table)

When `View.Both` is configured with a toolbar, users can switch between chart and table views using toolbar buttons:
- `ToolbarItems.Grid` — show table view
- `ToolbarItems.Chart` — show chart view

```razor
<SfPivotView TValue="ProductDetails"
             ShowToolbar="true"
             Toolbar="@toolbarItems">
    <PivotViewDisplayOption View="View.Both" Primary="Primary.Table"></PivotViewDisplayOption>
    <PivotChartSettings>
        <PivotChartSeries Type="ChartSeriesType.Column"></PivotChartSeries>
    </PivotChartSettings>
</SfPivotView>

@code {
    private List<ToolbarItems> toolbarItems = new List<ToolbarItems>
    {
        ToolbarItems.Grid,
        ToolbarItems.Chart,
        ToolbarItems.Export
    };
}
```
