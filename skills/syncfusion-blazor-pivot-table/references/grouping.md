# Grouping — Syncfusion Blazor Pivot Table

Grouping automatically organizes date, number, and string fields into meaningful categories (e.g., dates into Year/Quarter/Month, numbers into ranges). Grouped fields act as independent fields and can be moved between axes. Applies to **relational data sources only**.

## Table of Contents
- [Enable Grouping](#enable-grouping)
- [Number Grouping](#number-grouping)
- [Date Grouping](#date-grouping)
- [Custom Grouping](#custom-grouping)
- [Programmatic Grouping Configuration](#programmatic-grouping-configuration)

---

## Enable Grouping

Set `AllowGrouping="true"` on `SfPivotView`. Users can then right-click any row/column header in the Pivot Table and select **Group** or **Ungroup**.

```razor
@using Syncfusion.Blazor.PivotView

<SfPivotView TValue="ProductDetails" ShowGroupingBar="true" AllowGrouping="true">
    <PivotViewDataSourceSettings DataSource="@data">
        <PivotViewColumns>
            <PivotViewColumn Name="Product_ID" Caption="Product ID"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="Date" Caption="Date"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Sold" Caption="Units Sold"></PivotViewValue>
            <PivotViewValue Name="Amount" Caption="Sold Amount"></PivotViewValue>
        </PivotViewValues>
        <PivotViewFormatSettings>
            <PivotViewFormatSetting Name="Date" Type="FormatType.DateTime"
                Format="dd/MM/yyyy-hh:mm">
            </PivotViewFormatSetting>
            <PivotViewFormatSetting Name="Amount" Format="C" UseGrouping="true">
            </PivotViewFormatSetting>
        </PivotViewFormatSettings>
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    public List<ProductDetails> data { get; set; }
    protected override void OnInitialized() => data = ProductDetails.GetProductData().ToList();
}
```

> Only **one type of grouping** can be applied to a single field at a time (similar to Excel behavior).

---

## Number Grouping

Groups numeric field members into ranges (e.g., Product IDs 1–5, 6–10).

**Via UI:** Right-click a numeric header → select **Group** → set start, end, and interval values.

**Programmatically:**
```razor
<PivotViewGroupSettings>
    <PivotViewGroupSetting Name="Product_ID" GroupInterval="5"
        StartingAt="1" EndingAt="200"
        Type="GroupType.Number">
    </PivotViewGroupSetting>
</PivotViewGroupSettings>
```

---

## Date Grouping

Groups a `DateTime` field into hierarchical levels such as Year, Quarter, Month, Day, Hours, Minutes, Seconds.

**Via UI:** Right-click a date/time header → **Group** → select desired intervals.

**Programmatically:**
```razor
<PivotViewGroupSettings>
    <PivotViewGroupSetting Name="Date"
        Type="GroupType.Date"
        GroupByDate="@dateGroupSettings">
    </PivotViewGroupSetting>
</PivotViewGroupSettings>

@code {
    // Group by Year and Month
    public DateGroup dateGroupSettings = new DateGroup
    {
        Years = true,
        Months = true,
        Quarters = false,
        Days = false
    };
}
```

**Available date group levels:** `Years`, `Quarters`, `Months`, `Days`, `Hours`, `Minutes`, `Seconds`

> **Tip:** Date grouping is the most common use case — group a date-of-order field by Year and Quarter to create time-series pivot reports without modifying the underlying data.

---

## Custom Grouping

Groups selected members of a field into a custom named category. For example, group "Mountain Bikes" and "Road Bikes" into "Bikes".

**Via UI:** Select specific row/column members using Ctrl+click → right-click → **Group** → enter a group name.

**Programmatically:**
```razor
<PivotViewGroupSettings>
    <PivotViewGroupSetting Name="Products"
        Type="GroupType.Custom"
        CustomGroups="@customGroups">
    </PivotViewGroupSetting>
</PivotViewGroupSettings>

@code {
    List<CustomGroups> customGroups = new List<CustomGroups>
    {
        new CustomGroups
        {
            GroupName = "Bikes",
            Items = new string[] { "Mountain Bikes", "Road Bikes" }
        }
    };
}
```

---

## Programmatic Grouping Configuration

Full example combining multiple settings:

```razor
<SfPivotView TValue="ProductDetails" AllowGrouping="true">
    <PivotViewDataSourceSettings DataSource="@data">
        <PivotViewColumns>
            <PivotViewColumn Name="Date"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="Product_ID"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Amount"></PivotViewValue>
        </PivotViewValues>
        <PivotViewFormatSettings>
            <PivotViewFormatSetting Name="Date" Type="FormatType.DateTime"
                Format="dd/MM/yyyy">
            </PivotViewFormatSetting>
        </PivotViewFormatSettings>
        <PivotViewGroupSettings>
            <!-- Group date by year and quarter -->
            <PivotViewGroupSetting Name="Date" Type="GroupType.Date"
                GroupByDate="@dateGroup">
            </PivotViewGroupSetting>
            <!-- Group Product IDs into ranges of 10 -->
            <PivotViewGroupSetting Name="Product_ID" GroupInterval="10"
                StartingAt="1" EndingAt="100" Type="GroupType.Number">
            </PivotViewGroupSetting>
        </PivotViewGroupSettings>
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    public List<ProductDetails> data { get; set; }
    protected override void OnInitialized() => data = ProductDetails.GetProductData().ToList();
    DateGroup dateGroup = new DateGroup { Years = true, Quarters = true };
}
```
