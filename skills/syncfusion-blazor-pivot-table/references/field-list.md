# Field List — Syncfusion Blazor Pivot Table

The Field List is a UI panel (similar to Excel's PivotTable Fields pane) that lets users drag fields between row, column, value, and filter axes at runtime, apply sorting and filtering, and configure the Pivot Table layout interactively.

## Table of Contents
- [In-Built Field List (Popup)](#in-built-field-list-popup)
- [Stand-Alone Field List (Fixed)](#stand-alone-field-list-fixed)
- [Defer Layout Update](#defer-layout-update)
- [Customizing the Field List](#customizing-the-field-list)
- [Field List with Grouping Bar](#field-list-with-grouping-bar)

---

## In-Built Field List (Popup)

Enable the popup field list by setting `ShowFieldList="true"` on `SfPivotView`. A small icon appears in the top-left (or top-right when the grouping bar is enabled) of the Pivot Table — clicking it opens the field list dialog.

```razor
@using Syncfusion.Blazor.PivotView

<SfPivotView TValue="ProductDetails" ShowFieldList="true">
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

## Stand-Alone Field List (Fixed)

Render the field list as a separate `SfPivotFieldList` component alongside the Pivot Table. This gives a permanent panel layout similar to Excel's default view.

```razor
@using Syncfusion.Blazor.PivotView

<div style="display: flex; gap: 16px;">
    <SfPivotView TValue="ProductDetails" @ref="pivotRef" Height="530">
        <PivotViewDataSourceSettings DataSource="@data" EnableSorting="true">
            <PivotViewColumns>
                <PivotViewColumn Name="Year"></PivotViewColumn>
            </PivotViewColumns>
            <PivotViewRows>
                <PivotViewRow Name="Country"></PivotViewRow>
                <PivotViewRow Name="Products"></PivotViewRow>
            </PivotViewRows>
            <PivotViewValues>
                <PivotViewValue Name="Amount" Caption="Sold Amount"></PivotViewValue>
            </PivotViewValues>
        </PivotViewDataSourceSettings>
        <PivotViewEvents TValue="ProductDetails"
                         EnginePopulated="@UpdateFieldList">
        </PivotViewEvents>
    </SfPivotView>

    <SfPivotFieldList TValue="ProductDetails" @ref="fieldListRef"
                      RenderMode="Mode.Fixed" AllowCalculatedField="true">
        <PivotFieldListDataSourceSettings DataSource="@data" EnableSorting="true">
            <PivotFieldListColumns>
                <PivotFieldListColumn Name="Year"></PivotFieldListColumn>
            </PivotFieldListColumns>
            <PivotFieldListRows>
                <PivotFieldListRow Name="Country"></PivotFieldListRow>
            </PivotFieldListRows>
            <PivotFieldListValues>
                <PivotFieldListValue Name="Amount" Caption="Sold Amount"></PivotFieldListValue>
            </PivotFieldListValues>
        </PivotFieldListDataSourceSettings>
        <PivotFieldListEvents TValue="ProductDetails"
                              EnginePopulated="@UpdatePivotView">
        </PivotFieldListEvents>
    </SfPivotFieldList>
</div>

@code {
    SfPivotView<ProductDetails> pivotRef;
    SfPivotFieldList<ProductDetails> fieldListRef;
    List<ProductDetails> data = ProductDetails.GetProductData().ToList();

    async Task UpdateFieldList(EnginePopulatedEventArgs args)
    {
        await fieldListRef.UpdateAsync(pivotRef);
    }

    async Task UpdatePivotView(EnginePopulatedEventArgs args)
    {
        await fieldListRef.UpdateViewAsync(pivotRef);
    }
}
```

> **Why use stand-alone?** It keeps the pivot grid area uncluttered and gives users a persistent panel to configure the report — ideal for analytics dashboards.

---

## Defer Layout Update

When users make multiple field changes (drag fields, change aggregations) the Pivot Table re-renders after every action, which can be slow. Enable `AllowDeferLayoutUpdate` to batch all changes and apply them when the user explicitly clicks the **Update** button.

```razor
<SfPivotView TValue="ProductDetails" AllowDeferLayoutUpdate="true" ShowFieldList="true">
    ...
</SfPivotView>
```

Or on the stand-alone field list:
```razor
<SfPivotFieldList TValue="ProductDetails" AllowDeferLayoutUpdate="true" RenderMode="Mode.Fixed">
```

> **When to use:** Essential for large datasets where each re-render is expensive. Reduces server calls in Blazor Server apps.

---

## Customizing the Field List

### Hide specific fields from the field list

```razor
<PivotViewDataSourceSettings DataSource="@data">
    <!-- Field captions rename display name in field list -->
    <PivotViewColumn Name="Year" Caption="Fiscal Year"></PivotViewColumn>
</PivotViewDataSourceSettings>
```

### Open field list dialog programmatically

```razor
<SfPivotView TValue="ProductDetails" @ref="pivot" ShowFieldList="false">
    ...
</SfPivotView>
<button @onclick="OpenFieldList">Open Field List</button>

@code {
    SfPivotView<ProductDetails> pivot;
    void OpenFieldList() => pivot.ShowFieldListAsync();
}
```

---

## Field List with Grouping Bar

Both can be enabled simultaneously. The field list icon moves to the top-right corner when the grouping bar is visible:

```razor
<SfPivotView TValue="ProductDetails"
             ShowFieldList="true"
             ShowGroupingBar="true">
    ...
</SfPivotView>
```
