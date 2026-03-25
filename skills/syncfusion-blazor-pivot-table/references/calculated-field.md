# Calculated Fields — Syncfusion Blazor Pivot Table

Calculated fields let users define custom value fields using mathematical formulas based on existing fields. They appear alongside regular value fields in the Pivot Table. Only applicable to relational data sources.

## Table of Contents
- [Enable Calculated Fields](#enable-calculated-fields)
- [Define a Calculated Field Programmatically](#define-a-calculated-field-programmatically)
- [Open the Calculated Field Dialog via UI](#open-the-calculated-field-dialog-via-ui)
- [Open Dialog Programmatically](#open-dialog-programmatically)
- [Formula Syntax](#formula-syntax)
- [Gotchas](#gotchas)

---

## Enable Calculated Fields

Set `AllowCalculatedField="true"` on `SfPivotView`. This also surfaces the **CALCULATED FIELD** button in the Field List UI.

```razor
<SfPivotView TValue="ProductDetails" AllowCalculatedField="true" ShowFieldList="true">
```

---

## Define a Calculated Field Programmatically

Use `PivotViewCalculatedFieldSettings` inside `PivotViewDataSourceSettings`. The calculated field must also be added to `PivotViewValues` with `Type=SummaryTypes.CalculatedField` to appear in the grid.

```razor
@using Syncfusion.Blazor.PivotView

<SfPivotView TValue="ProductDetails" ShowFieldList="true" AllowCalculatedField="true">
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
            <!-- Calculated field: must be declared here AND in CalculatedFieldSettings -->
            <PivotViewValue Name="TotalRevenue" Caption="Total Revenue"
                            Type="SummaryTypes.CalculatedField">
            </PivotViewValue>
        </PivotViewValues>
        <PivotViewFormatSettings>
            <PivotViewFormatSetting Name="Amount" Format="C"></PivotViewFormatSetting>
            <PivotViewFormatSetting Name="TotalRevenue" Format="C"></PivotViewFormatSetting>
        </PivotViewFormatSettings>
        <PivotViewCalculatedFieldSettings>
            <PivotViewCalculatedFieldSetting Name="TotalRevenue"
                Formula="@formula">
            </PivotViewCalculatedFieldSetting>
        </PivotViewCalculatedFieldSettings>
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    // Formula: Sum(Amount) + Sum(Sold) — field names wrapped in quotes
    public string formula = "\"Sum(Amount)\"" + "+" + "\"Sum(Sold)\"";
    public List<ProductDetails> data { get; set; }
    protected override void OnInitialized() => data = ProductDetails.GetProductData().ToList();
}
```

---

## Open the Calculated Field Dialog via UI

When `AllowCalculatedField="true"` and `ShowFieldList="true"`, a **CALCULATED FIELD** button appears at the bottom of the Field List dialog. Clicking it opens an interactive editor where users can:
- Name the new field
- Build formulas by clicking existing fields
- Preview the formula

---

## Open Dialog Programmatically

Trigger the dialog from any external button using `CreateCalculatedFieldDialogAsync`:

```razor
@using Syncfusion.Blazor.PivotView
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="OpenDialog">Add Calculated Field</SfButton>

<SfPivotView TValue="ProductDetails" @ref="pivot" AllowCalculatedField="true">
    <PivotViewDataSourceSettings DataSource="@data">
        ...
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    SfPivotView<ProductDetails> pivot;
    List<ProductDetails> data = ProductDetails.GetProductData().ToList();

    async Task OpenDialog() => await pivot.CreateCalculatedFieldDialogAsync();
}
```

---

## Formula Syntax

Formulas use aggregation functions with field names wrapped in double quotes:

| Pattern | Example |
|---|---|
| Simple aggregation | `"Sum(Amount)"` |
| Multiple fields | `"Sum(Amount)" + "Sum(Sold)"` |
| Arithmetic | `"Sum(Amount)" / "Count(Sold)"` |
| Percentage | `("Sum(Sold)" / "Sum(Amount)") * 100` |

In C# string literals, inner quotes must be escaped or use `@"..."` with doubled quotes:
```csharp
// Option 1: escape
string formula = "\"Sum(Amount)\" / \"Count(Sold)\"";

// Option 2: verbatim with doubled quotes
string formula = @"""Sum(Amount)"" / ""Count(Sold)""";
```

---

## Gotchas

> **⚠️ Most Common Mistake — Will Silently Break Your Calculated Field:**
>
> Adding a `PivotViewCalculatedFieldSetting` is NOT enough on its own. You **must also** declare the field in `PivotViewValues` with `Type="SummaryTypes.CalculatedField"` explicitly set. Without `Type=SummaryTypes.CalculatedField`, the field will not render at all — no error is thrown.
>
> ```razor
> <!-- ✅ CORRECT: Both declarations required -->
> <PivotViewValues>
>     <PivotViewValue Name="TotalRevenue" Caption="Total Revenue"
>                     Type="SummaryTypes.CalculatedField">   <!-- ← REQUIRED -->
>     </PivotViewValue>
> </PivotViewValues>
> <PivotViewCalculatedFieldSettings>
>     <PivotViewCalculatedFieldSetting Name="TotalRevenue" Formula="@formula">
>     </PivotViewCalculatedFieldSetting>
> </PivotViewCalculatedFieldSettings>
>
> <!-- ❌ WRONG: Missing Type — field won't appear in the grid -->
> <PivotViewValue Name="TotalRevenue" Caption="Total Revenue">
> </PivotViewValue>
> ```

- Calculated fields only work with **relational** data sources, not OLAP
- Field names in formulas are case-sensitive and must exactly match the property names in your data model
- Format settings for the calculated field must be declared separately in `PivotViewFormatSettings`
