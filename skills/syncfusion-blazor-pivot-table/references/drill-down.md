# Drill Down — Syncfusion Blazor Pivot Table

Drill down enables expanding and collapsing hierarchical members (e.g., Year → Quarter, Country → City) in row and column headers. Users can interactively explore data at different levels, and you can programmatically control expansion.

## Drill Down and Drill Up

When a field has hierarchical members, expand/collapse icons automatically appear on row and column headers. Click the **+** icon to drill down (expand children) and the **-** icon to drill up (collapse).

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

## Expand All on Load

By default, only the top-level members are shown. Set `ExpandAll="true"` to show all levels expanded from the start:

```razor
<PivotViewDataSourceSettings DataSource="@data" ExpandAll="true">
```

> **Applies to:** Relational data sources only.

## Expand Specific Members on Load

Use `PivotViewDrilledMember` inside `PivotViewDataSourceSettings` to expand only specific members programmatically:

> **⚠️ `Name` Must Be the Field Name, Not a Member Value:**
>
> `PivotViewDrilledMember.Name` refers to the **field name** (the property/column name), not a member caption or a modified value like `"Year_FY2023"`. The members to expand go in `Items`.

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

## Drill Position (Independent Drill)

Drilling down on a member in one position does not affect the same member name appearing elsewhere. For example, if both FY 2022 and FY 2023 have "Q1" as a child:
- Expanding Q1 under FY 2022 only expands that specific Q1
- Q1 under FY 2023 remains collapsed

This behavior is automatic and cannot be disabled — it ensures precise data focus.

## Responding to Drill Operations with Events

Use the `OnActionBegin` event to detect drill-down and drill-up actions before they execute, and optionally block them:

```razor
@using Syncfusion.Blazor.PivotView

<SfPivotView TValue="ProductDetails" ShowGroupingBar="true">
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
    <PivotViewEvents TValue="ProductDetails" OnActionBegin="ActionBegin">
    </PivotViewEvents>
</SfPivotView>

@code {
    public List<ProductDetails> data { get; set; }
    protected override void OnInitialized() => data = ProductDetails.GetProductData().ToList();

    public void ActionBegin(PivotActionBeginEventArgs args)
    {
        if (args.ActionName == "Drill down")
        {
            // Handle drill down action
            Console.WriteLine("User is drilling down");
            // Optionally block the action
            // args.Cancel = true;
        }
        else if (args.ActionName == "Drill up")
        {
            // Handle drill up action
            Console.WriteLine("User is drilling up");
        }
    }
}
```

The `ActionName` parameter indicates the drill action:
- `"Drill down"` - User expanded a member
- `"Drill up"` - User collapsed a member
