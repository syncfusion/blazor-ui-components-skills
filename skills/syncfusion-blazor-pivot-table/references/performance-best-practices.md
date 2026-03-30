# Pivot Table Performance & Scalability

## Virtual Scrolling

Renders only rows/columns visible in the current viewport — the most impactful performance feature for large datasets.

```razor
<SfPivotView TValue="PivotVirtualData"
             Width="100%" Height="500"
             EnableVirtualization="true"
             ShowTooltip="false">
    <PivotViewDataSourceSettings DataSource="@data" EnableSorting="false">
        <PivotViewColumns>
            <PivotViewColumn Name="Year"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="ProductID"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Price" Caption="Unit Price"></PivotViewValue>
            <PivotViewValue Name="Sold" Caption="Units Sold"></PivotViewValue>
        </PivotViewValues>
    </PivotViewDataSourceSettings>
    <PivotViewGridSettings ColumnWidth="120"></PivotViewGridSettings>
</SfPivotView>
```

> **Important:** Do not enable `EnableVirtualization` and `EnablePaging` at the same time — they conflict.

---

## Paging

Alternative to virtual scrolling for browsers with pixel-height limitations. Divides row/column data into pages.

```razor
<SfPivotView TValue="PivotProductDetails"
             Height="500" Width="100%"
             EnablePaging="true">
    <PivotViewPageSettings
        CurrentRowPage="1"
        CurrentColumnPage="1"
        RowPageSize="10"
        ColumnPageSize="5">
    </PivotViewPageSettings>
    <PivotViewPagerSettings
        Position="PagerPosition.Bottom"
        ShowRowPager="true"
        ShowColumnPager="true"
        EnableCompactView="false"
        RowPageSizes="@(new List<int>{10, 50, 100})"
        ColumnPageSizes="@(new List<int>{5, 10, 20})">
    </PivotViewPagerSettings>
    <PivotViewDataSourceSettings TValue="PivotProductDetails">
        <!-- data source config here -->
    </PivotViewDataSourceSettings>
</SfPivotView>
```

### `PivotViewPageSettings` Properties

| Property | Description |
|---|---|
| `CurrentRowPage` | Initial row page number (default: 1) |
| `CurrentColumnPage` | Initial column page number (default: 1) |
| `RowPageSize` | Records per row page |
| `ColumnPageSize` | Records per column page |

### `PivotViewPagerSettings` Properties

| Property | Description |
|---|---|
| `Position` | `PagerPosition.Top` / `PagerPosition.Bottom` |
| `ShowRowPager` | Show/hide row pager |
| `ShowColumnPager` | Show/hide column pager |
| `EnableCompactView` | Compact pager UI |
| `IsInversed` | Swap row/column pager positions |
| `RowPageSizes` / `ColumnPageSizes` | Dropdown size options |

---

## Data Compression

Compresses repeated raw data records before pivot calculations. Useful when input data has many duplicate rows.

```razor
<SfPivotView TValue="ProductDetails"
             Width="800" Height="300"
             EnableVirtualization="true"
             AllowDataCompression="true">
    <!-- data source settings -->
</SfPivotView>
```

> **Requirement:** `AllowDataCompression` only works when `EnableVirtualization="true"` is also set.

**Limitation — unsupported aggregation types (auto-convert to Sum):**
- Average, PopulationStDev, SampleStDev, PopulationVar, SampleVar
- DistinctCount → behaves as Count

---

## Defer Layout Update

Prevents the pivot table from re-rendering after every field change in the Field List. Apply all changes at once with the "Apply" button.

```razor
<SfPivotView TValue="ProductDetails"
             ShowFieldList="true"
             AllowDeferLayoutUpdate="true">
    <!-- data source settings -->
</SfPivotView>
```

- Works with both in-built popup Field List and stand-alone `SfPivotFieldList`
- For stand-alone: set `AllowDeferLayoutUpdate="true"` on `SfPivotFieldList` as well
- Reduces multiple unnecessary renders when rearranging many fields

---

## Server-Side Pivot Engine

Offloads all aggregation/filtering/sorting to a server Web API. Recommended for WASM apps with large datasets.

**Step 1 — Download the PivotController app:**
```
https://github.com/SyncfusionExamples/server-side-pivot-engine-for-blazor-pivot-table.git
```
Run the app (hosts at e.g. `http://localhost:61379/api/pivot/post`).

**Step 2 — Connect the Pivot Table:**
```razor
<SfPivotView TValue="PivotViewData" Height="300">
    <PivotViewDataSourceSettings TValue="PivotViewData"
        Url="http://localhost:61379/api/pivot/post"
        EnableServerSideAggregation="true">
        <!-- field axis config -->
    </PivotViewDataSourceSettings>
</SfPivotView>
```

**Key NuGet for the server app:**
```
Syncfusion.Pivot.Engine
```

**Benefits:**
- Only aggregated viewport data transmitted (not raw data)
- Supports virtual scrolling + paging on the server side
- Cache keeps engine instance per-user GUID for fast subsequent requests
- Especially useful for Blazor WASM (avoids large data transfer to browser)

---

## Member Editor Performance Optimization

When working with fields that have thousands of unique values, the member editor (filter dialog) can become slow. Use these properties to optimize performance.

### MaxNodeLimitInMemberEditor

Limits the maximum number of members displayed in the filter dialog. Essential for fields with thousands of unique values.

```razor
<SfPivotView TValue="ProductDetails"
             ShowFieldList="true"
             MaxNodeLimitInMemberEditor="1000">
    <PivotViewDataSourceSettings DataSource="@data">
        <PivotViewColumns>
            <PivotViewColumn Name="Year"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="CustomerID"></PivotViewRow>
            <PivotViewRow Name="ProductID"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Amount"></PivotViewValue>
        </PivotViewValues>
    </PivotViewDataSourceSettings>
</SfPivotView>
```

**Default Value:** 1000  
**Recommended Range:** 500 - 5000 (depending on browser capability)

**Performance Impact:**
| Member Count | Without Limit | With Limit (1000) |
|--------------|---------------|-------------------|
| 5,000 members | ~3-5 seconds | ~1-2 seconds |
| 10,000 members | ~8-12 seconds | ~1-2 seconds |
| 50,000+ members | >30 seconds | ~1-2 seconds |

### LoadOnDemandInMemberEditor

Enables lazy loading for filter dialog members. Members load incrementally as users scroll, dramatically improving initial load time.

```razor
<SfPivotView TValue="ProductDetails"
             ShowFieldList="true"
             LoadOnDemandInMemberEditor="true"
             MaxNodeLimitInMemberEditor="2000">
    <PivotViewDataSourceSettings DataSource="@data">
        <PivotViewColumns>
            <PivotViewColumn Name="Year"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="CustomerName"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Sales"></PivotViewValue>
        </PivotViewValues>
    </PivotViewDataSourceSettings>
</SfPivotView>
```

**Default Value:** false  
**Best Used With:** MaxNodeLimitInMemberEditor

**Benefits:**
- Filter dialog opens immediately (no wait for all members to load)
- Reduced memory consumption
- Smooth scrolling experience
- Works with hundreds of thousands of members

**Recommended Configuration for Large Datasets:**
```razor
<SfPivotView TValue="SalesData"
             EnableVirtualization="true"
             LoadOnDemandInMemberEditor="true"
             MaxNodeLimitInMemberEditor="2000">
    <!-- Configuration for optimal performance -->
</SfPivotView>
```

---

## Restricting UI Options

### Customize Available Aggregations

Limit which aggregation types users can select in the UI. Useful for simplifying choices or enforcing business rules.

```razor
<SfPivotView TValue="ProductDetails"
             ShowGroupingBar="true"
             AggregateTypes="@allowedAggregations">
    <PivotViewDataSourceSettings DataSource="@data">
        <PivotViewColumns>
            <PivotViewColumn Name="Year"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="Country"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Amount"></PivotViewValue>
        </PivotViewValues>
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    List<SummaryTypes> allowedAggregations = new List<SummaryTypes> 
    {
        SummaryTypes.Sum,
        SummaryTypes.Count,
        SummaryTypes.Avg
    };
}
```

**Available Types:** Sum, Count, Avg, Max, Min, Product, DistinctCount, PercentageOfGrandTotal, PercentageOfRowTotal, PercentageOfColumnTotal, Index, etc.

**When to Use:**
- Simplify UI for non-technical users
- Hide aggregations that don't make sense for your data
- Enforce specific analysis methodologies
- Reduce cognitive load during training

**Example: Percentage-Only Analysis**
```razor
@code {
    List<SummaryTypes> percentageAggregations = new List<SummaryTypes> 
    {
        SummaryTypes.Sum,
        SummaryTypes.PercentageOfGrandTotal,
        SummaryTypes.PercentageOfRowTotal,
        SummaryTypes.PercentageOfColumnTotal
    };
}
```

### Customize Available Chart Types

Restrict which chart types users can select when displaying pivot data in chart view.

```razor
<SfPivotView TValue="ProductDetails"
             ShowToolbar="true"
             Toolbar="@toolbar"
             ChartTypes="@allowedCharts">
    <PivotViewDisplayOption View="View.Both"></PivotViewDisplayOption>
    <PivotViewDataSourceSettings DataSource="@data">
        <PivotViewColumns>
            <PivotViewColumn Name="Year"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="Products"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Sold"></PivotViewValue>
        </PivotViewValues>
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    public List<ToolbarItems> toolbar = new List<ToolbarItems> {
        ToolbarItems.Chart
    };
    
    List<ChartSeriesType> allowedCharts = new List<ChartSeriesType>
    {
        ChartSeriesType.Column,
        ChartSeriesType.Line,
        ChartSeriesType.Area
    };
}
```

**Available Types:** Column, Bar, Line, Area, Pie, Doughnut, Scatter, Bubble, StackingColumn, StackingBar, StackingArea, etc.

**When to Use:**
- Enforce company charting guidelines
- Hide charts unsuitable for your data type
- Simplify choices for non-expert users
- Maintain uniform dashboard aesthetics

**Example: Comparison-Focused Charts**
```razor
@code {
    List<ChartSeriesType> comparisonCharts = new List<ChartSeriesType>
    {
        ChartSeriesType.Column,
        ChartSeriesType.Bar,
        ChartSeriesType.StackingColumn,
        ChartSeriesType.StackingBar
    };
}
```

---

## WebAssembly Performance Tips

### Avoid Unnecessary Re-renders

Use `PreventRender()` to block pivot re-renders during unrelated state changes:

```razor
<SfPivotView @ref="pivot" TValue="ProductDetails">
    <!-- ... -->
</SfPivotView>

@code {
    private SfPivotView<ProductDetails> pivot;

    private void SomeOtherAction()
    {
        pivot.PreventRender();  // Pivot won't re-render
        // update unrelated state here
    }
}
```

### General WASM Recommendations

| Tip | Detail |
|---|---|
| Use individual NuGet | `Syncfusion.Blazor.PivotTable` instead of `Syncfusion.Blazor` bundle |
| Use individual script/CSS | Load only pivot-specific resources |
| Disable tooltip | `ShowTooltip="false"` when not needed |
| Disable sorting at initial load | `EnableSorting="false"` for large datasets |
| Use server-side engine | For datasets > 100k rows in WASM |
| Limit filter dialog members | `MaxNodeLimitInMemberEditor="1000"` + `LoadOnDemandInMemberEditor="true"` |
| Avoid grouping on large data | Built-in grouping re-processes all raw data |
| Restrict UI options | Use `AggregateTypes` and `ChartTypes` to simplify user interface |

---

## Performance Strategy Decision Tree

```
Large dataset (>50k rows)?
├── WASM app?
│   ├── Yes → Server-side pivot engine (EnableServerSideAggregation)
│   └── No  → Virtual scrolling (EnableVirtualization)
├── Many duplicate rows?
│   └── Yes → Add AllowDataCompression + EnableVirtualization
├── Browser pixel-height limitation?
│   └── Yes → Use Paging (EnablePaging) instead of virtual scrolling
└── Users rearranging many fields?
    └── Yes → AllowDeferLayoutUpdate for batch apply
```
