# Grouping Bar — Syncfusion Blazor Pivot Table

The Grouping Bar is a drag-and-drop UI element rendered above the Pivot Table that displays the currently applied row, column, value, and filter fields. Users can rearrange, remove, filter, and sort fields interactively at runtime without needing to write code.

## Table of Contents
- [Enable Grouping Bar](#enable-grouping-bar)
- [What Users Can Do in the Grouping Bar](#what-users-can-do-in-the-grouping-bar)
- [Customizing Grouping Bar Behavior](#customizing-grouping-bar-behavior)
- [Show/Hide Specific Buttons](#showhide-specific-buttons)
- [Grouping Bar with Field List](#grouping-bar-with-field-list)

---

## Enable Grouping Bar

Set `ShowGroupingBar="true"` on `SfPivotView`:

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
            <PivotViewFormatSetting Name="Amount" Format="C"></PivotViewFormatSetting>
        </PivotViewFormatSettings>
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    public List<ProductDetails> data { get; set; }
    protected override void OnInitialized() => data = ProductDetails.GetProductData().ToList();
}
```

---

## What Users Can Do in the Grouping Bar

| Interaction | How |
|---|---|
| Rearrange fields | Drag chips between row/column/value/filter drop zones |
| Remove a field | Click the **×** (remove) icon on a field chip |
| Add a field | Click **Fields** icon to open field panel, then drag to a zone |
| Filter field members | Click the **filter** icon on a field chip |
| Sort field members | Click the **sort** icon on a field chip (ascending/descending) |
| Change aggregation type | Click the **aggregation** icon on a value field chip |

---

## Customizing Grouping Bar Behavior

Use `PivotViewGroupingBarSettings` inside `SfPivotView` to control which UI features are available:

```razor
<SfPivotView TValue="ProductDetails" ShowGroupingBar="true">
    <PivotViewDataSourceSettings DataSource="@data">
        ...
    </PivotViewDataSourceSettings>
    <PivotViewGroupingBarSettings
        ShowFilterIcon="true"
        ShowSortIcon="true"
        ShowRemoveIcon="true"
        ShowValueTypeIcon="true"
        AllowDragAndDrop="true">
    </PivotViewGroupingBarSettings>
</SfPivotView>
```

| Property | Default | Purpose |
|---|---|---|
| `ShowFilterIcon` | `true` | Show filter icon on each field chip |
| `ShowSortIcon` | `true` | Show sort icon on each field chip |
| `ShowRemoveIcon` | `true` | Show remove (×) icon on each field chip |
| `ShowValueTypeIcon` | `true` | Show aggregation type icon on value field chips |
| `AllowDragAndDrop` | `true` | Allow fields to be dragged between axes |

---

## Show/Hide Specific Buttons

### Disable all sorting icons in the grouping bar

```razor
<PivotViewGroupingBarSettings ShowSortIcon="false">
</PivotViewGroupingBarSettings>
```

### Read-only grouping bar (no drag, no remove)

```razor
<PivotViewGroupingBarSettings
    AllowDragAndDrop="false"
    ShowRemoveIcon="false">
</PivotViewGroupingBarSettings>
```

---

## Grouping Bar with Field List

The grouping bar and field list work together seamlessly. When both are enabled, the Field List icon moves to the top-right corner of the grouping bar area:

```razor
<SfPivotView TValue="ProductDetails"
             ShowGroupingBar="true"
             ShowFieldList="true">
    ...
</SfPivotView>
```

> **Pattern:** Enable the grouping bar for users who primarily rearrange existing fields. Add the field list for users who need to add new fields from the data source. Both together give the full Excel-like pivot experience.
