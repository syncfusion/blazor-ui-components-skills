# Filtering — Syncfusion Blazor Pivot Table

Filtering lets users focus on specific members of their data by including or excluding items from the Pivot Table display. Three filter types are supported: **Member**, **Label**, and **Value** filtering.

## Table of Contents
- [Member Filtering](#member-filtering)
- [Label Filtering](#label-filtering)
- [Value Filtering](#value-filtering)
- [Date Filtering](#date-filtering)
- [Disable Filtering](#disable-filtering)

---

## Member Filtering

Member filtering shows or hides specific members of a field. Enabled by default via `AllowMemberFilter` in `PivotViewDataSourceSettings`.

### Via UI
Click the filter icon on a field in the Field List or Grouping Bar. A dialog shows all members with checkboxes to include/exclude.

### Programmatically

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
        <PivotViewFormatSettings>
            <PivotViewFormatSetting Name="Amount" Format="C0" UseGrouping="true">
            </PivotViewFormatSetting>
        </PivotViewFormatSettings>
        <PivotViewFilterSettings>
            <!-- Exclude FY 2017 from Year field -->
            <PivotViewFilterSetting Name="Year"
                Type="FilterType.Exclude"
                Items="@(new string[] { "FY 2017" })">
            </PivotViewFilterSetting>
            <!-- Include only France and Germany -->
            <PivotViewFilterSetting Name="Country"
                Type="FilterType.Include"
                Items="@(new string[] { "France", "Germany" })">
            </PivotViewFilterSetting>
        </PivotViewFilterSettings>
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    public List<ProductDetails> data { get; set; }
    protected override void OnInitialized() => data = ProductDetails.GetProductData().ToList();
}
```

**Key properties on `PivotViewFilterSetting`:**
- `Name` — the field name to filter
- `Type` — `FilterType.Include` or `FilterType.Exclude`
- `Items` — array of member values to include/exclude

---

## Label Filtering

Label filtering applies conditions to row/column header text. For example, show only countries whose name begins with "F".

```razor
<PivotViewFilterSettings>
    <PivotViewFilterSetting Name="Country"
        Type="FilterType.Label"
        Condition="Operators.BeginWith"
        Value1="F">
    </PivotViewFilterSetting>
</PivotViewFilterSettings>
```

**Available `Operators` for label filtering:**

| Operator | Description |
|---|---|
| `Equals` | Exact match |
| `DoesNotEquals` | Not equal |
| `BeginWith` | Starts with |
| `DoesNotBeginWith` | Does not start with |
| `EndsWith` | Ends with |
| `Contains` | Contains substring |
| `GreaterThan` | Alphabetically greater |
| `LessThan` | Alphabetically less |
| `Between` | Between two values (`Value1` and `Value2`) |

---

## Value Filtering

Value filtering shows or hides members based on aggregated value conditions. For example, show only countries where total Amount > 100,000.

```razor
<PivotViewFilterSettings>
    <PivotViewFilterSetting Name="Country"
        Type="FilterType.Value"
        Measure="Amount"
        Condition="Operators.GreaterThan"
        Value1="100000">
    </PivotViewFilterSetting>
</PivotViewFilterSettings>
```

- `Measure` — the value field to evaluate (matches a `PivotViewValue Name`)
- `Condition` — comparison operator (`Equals`, `GreaterThan`, `LessThan`, `Between`, etc.)
- `Value1` (and `Value2` for `Between`) — threshold values

---

## Date Filtering

For date fields, use `FilterType.Date` combined with date-specific operators:

```razor
<PivotViewFilterSettings>
    <PivotViewFilterSetting Name="OrderDate"
        Type="FilterType.Date"
        Condition="Operators.Between"
        Value1="2023-01-01"
        Value2="2023-12-31">
    </PivotViewFilterSetting>
</PivotViewFilterSettings>
```

---

## Disable Filtering

### Disable member filtering globally
```razor
<PivotViewDataSourceSettings DataSource="@data" AllowMemberFilter="false">
```

### Disable label filtering globally
```razor
<PivotViewDataSourceSettings DataSource="@data" AllowLabelFilter="false">
```

### Disable value filtering globally
```razor
<PivotViewDataSourceSettings DataSource="@data" AllowValueFilter="false">
```

> When all filtering options are disabled, the filter icon will not appear in the Field List or Grouping Bar.
