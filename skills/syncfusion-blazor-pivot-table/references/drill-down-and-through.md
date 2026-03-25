# Drill Down & Drill Through — Syncfusion Blazor Pivot Table

## Table of Contents
- [Drill Down and Drill Up](#drill-down-and-drill-up)
- [Expand All on Load](#expand-all-on-load)
- [Expand Specific Members on Load](#expand-specific-members-on-load)
- [Drill Position (Independent Drill)](#drill-position-independent-drill)
- [Drill Through (Raw Data Popup)](#drill-through-raw-data-popup)
- [Programmatic Drill Operations](#programmatic-drill-operations)

---

## Drill Down and Drill Up

When a field has hierarchical members (e.g., Year → Quarter, Country → City), expand/collapse icons automatically appear on row and column headers. Click the **+** icon to drill down (expand children) and the **-** icon to drill up (collapse).

This feature is **built-in** and requires no additional configuration when multiple fields are placed on the same axis.

```razor
@using Syncfusion.Blazor.PivotView

<SfPivotView TValue="ProductDetails">
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
</SfPivotView>

@code {
    public List<ProductDetails> data { get; set; }
    protected override void OnInitialized() => data = ProductDetails.GetProductData().ToList();
}
```

---

## Expand All on Load

By default, only the top-level members are shown. Set `ExpandAll="true"` to show all levels expanded from the start:

```razor
<PivotViewDataSourceSettings DataSource="@data" ExpandAll="true">
```

> **Applies to:** Relational data sources only.

---

## Expand Specific Members on Load

Use `PivotViewDrilledMember` inside `PivotViewDataSourceSettings` to expand only specific members programmatically:

> **⚠️ `Name` Must Be the Field Name, Not a Member Value:**
>
> `PivotViewDrilledMember.Name` refers to the **field name** (the property/column name), not a member caption or a modified value like `"Year_FY2023"`. The members to expand go in `Items`.
>
> ```razor
> <!-- ✅ CORRECT: Name="Year" is the field; "FY 2023" is the member to expand -->
> <PivotViewDrilledMember Name="Year" Items="@(new string[] { "FY 2023" })">
> </PivotViewDrilledMember>
>
> <!-- For a date field named "OrderDate", use: -->
> <PivotViewDrilledMember Name="OrderDate" Items="@(new string[] { "2023" })">
> </PivotViewDrilledMember>
>
> <!-- ❌ WRONG: "OrderDate_Years" is not a valid field name -->
> <PivotViewDrilledMember Name="OrderDate_Years" Items="@(new string[] { "2023" })">
> </PivotViewDrilledMember>
> ```

```razor
<PivotViewDataSourceSettings DataSource="@data">
    ...
    <PivotViewDrilledMembers>
        <!-- Pre-expand "FY 2023" under Year column -->
        <PivotViewDrilledMember Name="Year" Items="@(new string[] { "FY 2023" })">
        </PivotViewDrilledMember>
        <!-- Pre-expand "France" under Country row -->
        <PivotViewDrilledMember Name="Country" Items="@(new string[] { "France" })">
        </PivotViewDrilledMember>
    </PivotViewDrilledMembers>
</PivotViewDataSourceSettings>
```

---

## Drill Position (Independent Drill)

Drilling down on a member in one position does not affect the same member name appearing elsewhere. For example, if both FY 2022 and FY 2023 have "Q1" as a child:
- Expanding Q1 under FY 2022 only expands that specific Q1
- Q1 under FY 2023 remains collapsed

This behavior is automatic and cannot be disabled — it ensures precise data focus.

---

## Drill Through (Raw Data Popup)

Drill-through shows the raw underlying records behind a value cell in a popup data grid. Double-clicking any value cell opens the drill-through dialog listing all rows that contribute to that aggregated value.

**Enable drill-through:**
```razor
<SfPivotView TValue="ProductDetails" AllowDrillThrough="true">
    ...
</SfPivotView>
```

**Configure the detail grid columns** (optional):
```razor
<SfPivotView TValue="ProductDetails" AllowDrillThrough="true">
    <PivotViewDataSourceSettings DataSource="@data">
        ...
    </PivotViewDataSourceSettings>
    <PivotViewDrillThroughSettings
        EnableColumnVirtualization="false">
    </PivotViewDrillThroughSettings>
</SfPivotView>
```

> **Use case:** Drill-through is ideal for financial reports where analysts need to see the transactions behind a summary figure. It surfaces data without navigating away from the pivot report.

---

## Programmatic Drill Operations

Expand or collapse members via method calls:

```razor
<SfPivotView TValue="ProductDetails" @ref="pivot">
    ...
</SfPivotView>
<button @onclick="DrillDown">Drill Down France</button>

@code {
    SfPivotView<ProductDetails> pivot;

    async Task DrillDown()
    {
        // Drill down the "France" member in the "Country" row field
        await pivot.DrillDownAsync(new DrillArgs
        {
            FieldName = "Country",
            FieldItem = "France",
            Axis = "row",
            Action = "down"   // "down" = expand, "up" = collapse
        });
    }
}
```
