# Layout & Display — Syncfusion Blazor Pivot Table

## Table of Contents
- [Width and Height](#width-and-height)
- [Classic (Tabular) Layout](#classic-tabular-layout)
- [Row & Column Customization](#row--column-customization)
- [Show or Hide Totals](#show-or-hide-totals)
- [Hyperlinks in Cells](#hyperlinks-in-cells)
- [Tooltip](#tooltip)

---

## Width and Height

Set dimensions using pixel values, percentages, or auto:

```razor
<!-- Fixed pixel size -->
<SfPivotView TValue="ProductDetails" Width="800" Height="400">

<!-- Responsive: fill container width -->
<SfPivotView TValue="ProductDetails" Width="100%" Height="500px">

<!-- Auto height: expands with content, parent scrolls -->
<SfPivotView TValue="ProductDetails" Width="100%" Height="auto">
```

> Minimum width is **400px** — values below this are ignored.

---

## Classic (Tabular) Layout

The default layout is **compact** (hierarchical, indented). The **classic/tabular layout** places each row field in its own separate column, resembling a flat table — useful for exporting or when users prefer Excel-style pivot views.

> **Limitation:** Classic layout works only with **relational** data sources and the **client-side engine**.

```razor
@using Syncfusion.Blazor.PivotView

<SfPivotView TValue="ProductDetails" Height="450" Width="100%">
    <PivotViewDataSourceSettings DataSource="@data">
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
    </PivotViewDataSourceSettings>
    <!-- Enable classic/tabular layout -->
    <PivotViewGridSettings Layout="PivotLayout.Tabular"></PivotViewGridSettings>
</SfPivotView>

@code {
    public List<ProductDetails> data { get; set; }
    protected override void OnInitialized() => data = ProductDetails.GetProductData().ToList();
}
```

`PivotLayout` options:
- `PivotLayout.Compact` — default hierarchical view
- `PivotLayout.Tabular` — classic/tabular (each row field in its own column)

---

## Row & Column Customization

### Column width

```razor
<PivotViewGridSettings ColumnWidth="120"></PivotViewGridSettings>
```

### Freeze rows and columns (sticky headers)

```razor
<PivotViewGridSettings FrozenRows="2" FrozenColumns="1"></PivotViewGridSettings>
```

### Auto-fit columns

```razor
<PivotViewGridSettings AllowAutoResizing="true"></PivotViewGridSettings>
```

### Row height

```razor
<PivotViewGridSettings RowHeight="36"></PivotViewGridSettings>
```

---

## Show or Hide Totals

Control grand totals and sub-totals in `PivotViewDataSourceSettings`:

```razor
<!-- Hide all grand totals -->
<PivotViewDataSourceSettings DataSource="@data" ShowGrandTotals="false">

<!-- Hide only row grand totals -->
<PivotViewDataSourceSettings DataSource="@data" ShowRowGrandTotals="false">

<!-- Hide only column grand totals -->
<PivotViewDataSourceSettings DataSource="@data" ShowColumnGrandTotals="false">

<!-- Hide all sub-totals -->
<PivotViewDataSourceSettings DataSource="@data" ShowSubTotals="false">

<!-- Hide row sub-totals only -->
<PivotViewDataSourceSettings DataSource="@data" ShowRowSubTotals="false">
```

> All totals are shown by default (`true`). Hiding them reduces visual clutter in reports with many nested levels.

---

## Hyperlinks in Cells

Render cells as clickable hyperlinks and handle them via the `HyperlinkCellClick` event:

```razor
<SfPivotView TValue="ProductDetails">
    <PivotViewDataSourceSettings DataSource="@data">
        ...
    </PivotViewDataSourceSettings>
    <!-- Enable hyperlinks on value cells only -->
    <PivotViewHyperlinkSettings ShowValueCellHyperlink="true"
                                CssClass="e-custom-hyperlink">
    </PivotViewHyperlinkSettings>
    <PivotViewEvents TValue="ProductDetails"
                     HyperlinkCellClick="OnHyperlinkClick">
    </PivotViewEvents>
</SfPivotView>

@code {
    public List<ProductDetails> data { get; set; }
    protected override void OnInitialized() => data = ProductDetails.GetProductData().ToList();

    void OnHyperlinkClick(HyperCellClickEventArgs args)
    {
        // args.CurrentCell contains the clicked cell info
        Console.WriteLine($"Clicked: {args.CurrentCell.Value}");
    }
}
```

**`PivotViewHyperlinkSettings` properties:**

| Property | Purpose |
|---|---|
| `ShowHyperlink` | Enable links on all cells |
| `ShowRowHeaderHyperlink` | Links only on row headers |
| `ShowColumnHeaderHyperlink` | Links only on column headers |
| `ShowValueCellHyperlink` | Links only on value cells |
| `ShowSummaryCellHyperlink` | Links only on summary/total cells |
| `HeaderText` | Enable link on a specific header text value |
| `CssClass` | Custom CSS class for styling the links |

---

## Tooltip

Tooltips show contextual information (row/column headers + value) when hovering over value cells. Enabled by default.

```razor
<!-- Show tooltip (default: true) -->
<SfPivotView TValue="ProductDetails" ShowTooltip="true">

<!-- Disable tooltip for better performance on large datasets -->
<SfPivotView TValue="ProductDetails" ShowTooltip="false">
```

> **Performance tip:** Disable tooltips (`ShowTooltip="false"`) when using `EnableVirtualization="true"` — toggling the tooltip on large virtualized datasets can cause minor re-render overhead.
