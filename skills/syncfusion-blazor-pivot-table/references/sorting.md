# Sorting — Syncfusion Blazor Pivot Table

The Pivot Table supports two sorting modes: **Member Sorting** (sorts row/column header members alphabetically) and **Value Sorting** (sorts rows/columns by their aggregated values).

## Table of Contents
- [Member Sorting](#member-sorting)
- [Value Sorting](#value-sorting)

---

## Member Sorting

Arranges field members in the rows and columns alphabetically in ascending or descending order.

**Default behavior:** `EnableSorting="true"` (ascending) — members sort A→Z automatically.

### Enable/disable sorting

```razor
<!-- Sorting enabled (default) -->
<PivotViewDataSourceSettings DataSource="@data" EnableSorting="true">

<!-- No sorting — members display in data source order -->
<PivotViewDataSourceSettings DataSource="@data" EnableSorting="false">
```

### Configure sorting programmatically

Use `PivotViewSortSettings` to set initial sort direction per field:

```razor
@using Syncfusion.Blazor.PivotView

<SfPivotView TValue="ProductDetails" ShowGroupingBar="true">
    <PivotViewDataSourceSettings DataSource="@data" EnableSorting="true">
        <PivotViewColumns>
            <PivotViewColumn Name="Year"></PivotViewColumn>
            <PivotViewColumn Name="Quarter"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="Country"></PivotViewRow>
            <PivotViewRow Name="Products"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Sold" Caption="Units Sold"></PivotViewValue>
            <PivotViewValue Name="Amount" Caption="Sold Amount"></PivotViewValue>
        </PivotViewValues>
        <PivotViewFormatSettings>
            <PivotViewFormatSetting Name="Amount" Format="C"></PivotViewFormatSetting>
        </PivotViewFormatSettings>
        <PivotViewSortSettings>
            <!-- Sort Country descending (Z→A) -->
            <PivotViewSortSetting Name="Country" Order="Sorting.Descending">
            </PivotViewSortSetting>
            <!-- Remove sort icon and use data source order for Products -->
            <PivotViewSortSetting Name="Products" Order="Sorting.None">
            </PivotViewSortSetting>
        </PivotViewSortSettings>
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    public List<ProductDetails> data { get; set; }
    protected override void OnInitialized() => data = ProductDetails.GetProductData().ToList();
}
```

**`Sorting` enum values:**
- `Sorting.Ascending` — A→Z (default)
- `Sorting.Descending` — Z→A
- `Sorting.None` — preserve data source order; removes sort icon from UI

### Sort via UI

When `EnableSorting="true"` and the grouping bar or field list is visible, click the sort icon on any row/column field chip to toggle between ascending and descending.

---

## Value Sorting

Value sorting reorders row/column members based on their aggregated values. For example, sort countries by their total "Amount" in descending order to see the highest-revenue countries first.

```razor
@using Syncfusion.Blazor.PivotView

<SfPivotView TValue="ProductDetails" EnableValueSorting="true">
    <PivotViewDataSourceSettings DataSource="@data" EnableSorting="true">
        <PivotViewColumns>
            <PivotViewColumn Name="Year"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="Country"></PivotViewRow>
            <PivotViewRow Name="Products"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Sold" Caption="Units Sold"></PivotViewValue>
            <PivotViewValue Name="Amount" Caption="Sold Amount"></PivotViewValue>
        </PivotViewValues>
    </PivotViewDataSourceSettings>
</SfPivotView>
```

When `EnableValueSorting="true"`, clicking any value column header arrow sorts all rows by that column's aggregated values.

> **Tip:** Value sorting works well with virtual scrolling for large datasets — combine `EnableVirtualization="true"` and `EnableValueSorting="true"` for performant large-data scenarios.
