# Aggregation — Syncfusion Blazor Pivot Table

Aggregation determines how numeric values are summarized in Pivot Table cells. By default, values are summed. You can configure per-field aggregation types both programmatically and interactively via the Field List or Grouping Bar.

> **Note:** Aggregation applies to relational data sources only. String, boolean, and date fields support only `Count` and `DistinctCount`.

## Table of Contents
- [Available Aggregation Types](#available-aggregation-types)
- [Setting Aggregation Programmatically](#setting-aggregation-programmatically)
- [DifferenceFrom and PercentageOfDifferenceFrom](#differencefrom-and-percentageofdifferencefrom)
- [PercentageOfParentTotal](#percentageofparenttotal)
- [Changing Aggregation at Runtime via UI](#changing-aggregation-at-runtime-via-ui)

---

## Available Aggregation Types

| `SummaryTypes` Value | Description |
|---|---|
| `Sum` | Total sum (default for numeric fields) |
| `Count` | Number of records |
| `DistinctCount` | Number of unique values |
| `Avg` | Arithmetic mean |
| `Median` | Middle value |
| `Min` | Minimum value |
| `Max` | Maximum value |
| `Product` | Product of all values |
| `Index` | Cell index relative to grand total |
| `RunningTotals` | Cumulative running total |
| `PopulationStDev` | Population standard deviation |
| `SampleStDev` | Sample standard deviation |
| `PopulationVar` | Population variance |
| `SampleVar` | Sample variance |
| `DifferenceFrom` | Difference from a base item in a base field |
| `PercentageOfDifferenceFrom` | % difference from a base item |
| `PercentageOfGrandTotal` | % of grand total |
| `PercentageOfColumnTotal` | % of column total |
| `PercentageOfRowTotal` | % of row total |
| `PercentageOfParentTotal` | % of parent total based on a base field |
| `PercentageOfParentColumnTotal` | % of parent column total |
| `PercentageOfParentRowTotal` | % of parent row total |
| `CalculatedField` | Marks the field as a custom calculated field |

---

## Setting Aggregation Programmatically

Set the `Type` property on `PivotViewValue` using `SummaryTypes`:

```razor
@using Syncfusion.Blazor.PivotView

<SfPivotView TValue="ProductDetails">
    <PivotViewDataSourceSettings DataSource="@data">
        <PivotViewColumns>
            <PivotViewColumn Name="Year"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="Country"></PivotViewRow>
            <PivotViewRow Name="Products"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <!-- Default: Sum -->
            <PivotViewValue Name="Sold" Caption="Units Sold"
                            Type="SummaryTypes.Sum"></PivotViewValue>
            <!-- Average amount -->
            <PivotViewValue Name="Amount" Caption="Avg Amount"
                            Type="SummaryTypes.Avg"></PivotViewValue>
            <!-- % of grand total -->
            <PivotViewValue Name="Sold" Caption="% of Total"
                            Type="SummaryTypes.PercentageOfGrandTotal"></PivotViewValue>
        </PivotViewValues>
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    public List<ProductDetails> data { get; set; }
    protected override void OnInitialized() => data = ProductDetails.GetProductData().ToList();
}
```

---

## DifferenceFrom and PercentageOfDifferenceFrom

These types compare each cell value to a specific member in a base field. Configure `BaseField` and `BaseItem`:

```razor
<PivotViewValues>
    <PivotViewValue Name="Amount" Caption="Diff from FY 2022"
                    Type="SummaryTypes.DifferenceFrom"
                    BaseField="Year"
                    BaseItem="FY 2022">
    </PivotViewValue>
    <PivotViewValue Name="Amount" Caption="% Diff from FY 2022"
                    Type="SummaryTypes.PercentageOfDifferenceFrom"
                    BaseField="Year"
                    BaseItem="FY 2022">
    </PivotViewValue>
</PivotViewValues>
```

> **Use case:** Show year-over-year growth by comparing each year's revenue against the baseline year.

---

## PercentageOfParentTotal

Shows each cell as a percentage of its parent total. Use `BaseField` to specify which parent field:

```razor
<PivotViewValues>
    <PivotViewValue Name="Sold" Caption="% of Country Total"
                    Type="SummaryTypes.PercentageOfParentTotal"
                    BaseField="Country">
    </PivotViewValue>
</PivotViewValues>
```

---

## Changing Aggregation at Runtime via UI

When `ShowGroupingBar="true"` or `ShowFieldList="true"`, users can change the aggregation type by clicking the value type icon (Σ) on a value field chip. A dropdown appears with all available `SummaryTypes`.

To allow this, no extra configuration is needed — it's enabled by default when the grouping bar or field list is visible.

To hide the aggregation type icon:
```razor
<PivotViewGroupingBarSettings ShowValueTypeIcon="false">
</PivotViewGroupingBarSettings>
```
