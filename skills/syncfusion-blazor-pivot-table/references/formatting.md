# Formatting — Syncfusion Blazor Pivot Table

## Table of Contents
- [Number Formatting](#number-formatting)
- [Number Formatting via Toolbar at Runtime](#number-formatting-via-toolbar-at-runtime)
- [Conditional Formatting](#conditional-formatting)
- [Conditional Formatting via Toolbar at Runtime](#conditional-formatting-via-toolbar-at-runtime)

---

## Number Formatting

Format how numeric values display in cells using `PivotViewFormatSettings` inside `PivotViewDataSourceSettings`. Apply one format setting per value field.

### Format codes

| Code | Example Output | Usage |
|---|---|---|
| `C` / `C2` | $1,234.56 | Currency (2 decimal places) |
| `C0` | $1,235 | Currency, no decimals |
| `N` / `N2` | 1,234.56 | Number with grouping |
| `P` / `P2` | 12.34% | Percentage |
| `#,##0.00` | 1,234.56 | Custom pattern |

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
            <PivotViewValue Name="Sold" Caption="Units Sold"></PivotViewValue>
            <PivotViewValue Name="Amount" Caption="Sold Amount"></PivotViewValue>
        </PivotViewValues>
        <PivotViewFormatSettings>
            <!-- Currency with thousand separators -->
            <PivotViewFormatSetting Name="Amount" Format="C0" UseGrouping="true">
            </PivotViewFormatSetting>
            <!-- Plain number, 2 decimal places -->
            <PivotViewFormatSetting Name="Sold" Format="N2">
            </PivotViewFormatSetting>
        </PivotViewFormatSettings>
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    public List<ProductDetails> data { get; set; }
    protected override void OnInitialized() => data = ProductDetails.GetProductData().ToList();
}
```

### Key `PivotViewFormatSetting` properties

| Property | Purpose |
|---|---|
| `Name` | Field name to format (must match `PivotViewValue Name`) |
| `Format` | Format string (C, N, P, or custom pattern) |
| `UseGrouping` | Show thousand separators (default: `true`) |
| `Currency` | Currency code (e.g., "USD", "EUR", "GBP") |
| `MinimumFractionDigits` | Minimum decimal places |
| `MaximumFractionDigits` | Maximum decimal places |

### Custom format example

```razor
<!-- Show as "1,234.56 USD" -->
<PivotViewFormatSetting Name="Amount" Format="#,##0.00" Currency="USD">
</PivotViewFormatSetting>
```

---

## Number Formatting via Toolbar at Runtime

Enable number formatting through the toolbar to let users change formats interactively:

```razor
<SfPivotView TValue="ProductDetails" ShowToolbar="true"
             Toolbar="@toolbar" AllowNumberFormatting="true">

@code {
    public List<ToolbarItems> toolbar = new List<ToolbarItems>
    {
        ToolbarItems.NumberFormatting
    };
}
```

---

## Conditional Formatting

Apply CSS-style formatting (background color, font color, font size, font family) to value cells that meet specified conditions. This helps highlight key data points visually.

### Programmatic configuration

Use `PivotViewConditionalFormatSettings`:

```razor
<SfPivotView TValue="ProductDetails" AllowConditionalFormatting="true">
    <PivotViewDataSourceSettings DataSource="@data">
        ...
    </PivotViewDataSourceSettings>
    <PivotViewConditionalFormatSettings>
        <!-- Highlight Amount cells > 100000 in green -->
        <PivotViewConditionalFormatSetting
            Measure="Amount"
            Conditions="Condition.GreaterThan"
            Value1="100000"
            Style="@highStyle">
        </PivotViewConditionalFormatSetting>
        <!-- Highlight Amount cells < 10000 in red -->
        <PivotViewConditionalFormatSetting
            Measure="Amount"
            Conditions="Condition.LessThan"
            Value1="10000"
            Style="@lowStyle">
        </PivotViewConditionalFormatSetting>
    </PivotViewConditionalFormatSettings>
</SfPivotView>

@code {
    public List<ProductDetails> data { get; set; }
    protected override void OnInitialized() => data = ProductDetails.GetProductData().ToList();

    PivotViewStyle highStyle = new PivotViewStyle
    {
        BackgroundColor = "#9ef59e",    // light green
        Color = "#004d00",              // dark green text
        FontSize = "14px",
        FontFamily = "Segoe UI"
    };

    PivotViewStyle lowStyle = new PivotViewStyle
    {
        BackgroundColor = "#f5a59e",    // light red
        Color = "#4d0000",              // dark red text
        FontSize = "14px",
        FontFamily = "Segoe UI"
    };
}
```

**`PivotViewConditionalFormatSetting` properties:**

| Property | Purpose |
|---|---|
| `Measure` | Value field name the condition applies to |
| `Conditions` | Comparison (`GreaterThan`, `LessThan`, `Equals`, `Between`, etc.) |
| `Value1` | First threshold value |
| `Value2` | Second value (for `Between` condition) |
| `Style` | `PivotViewStyle` object with CSS properties |
| `ApplyGrandTotals` | Apply formatting to grand total cells too |

---

## Conditional Formatting via Toolbar at Runtime

```razor
<SfPivotView TValue="ProductDetails" ShowToolbar="true"
             Toolbar="@toolbar" AllowConditionalFormatting="true">

@code {
    public List<ToolbarItems> toolbar = new List<ToolbarItems>
    {
        ToolbarItems.ConditionalFormatting
    };
}
```

Users click the **Conditional Formatting** toolbar icon to open an interactive dialog where they can create, edit, and delete formatting rules without code.
