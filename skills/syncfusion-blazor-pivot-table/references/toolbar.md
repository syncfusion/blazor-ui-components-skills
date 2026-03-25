# Toolbar — Syncfusion Blazor Pivot Table

The Toolbar provides quick access to frequently used features: switching between grid/chart views, exporting, conditional formatting, number formatting, report management, and more.

## Table of Contents
- [Enable Toolbar](#enable-toolbar)
- [Built-In Toolbar Items](#built-in-toolbar-items)
- [Report Management (Save, Load, Rename)](#report-management-save-load-rename)
- [Custom Toolbar Items](#custom-toolbar-items)
- [Toolbar Events](#toolbar-events)

---

## Enable Toolbar

Set `ShowToolbar="true"` and specify items via the `Toolbar` property:

```razor
@using Syncfusion.Blazor.PivotView

<SfPivotView TValue="ProductDetails" @ref="pivot"
             ShowFieldList="true"
             ShowToolbar="true"
             Toolbar="@toolbar"
             AllowConditionalFormatting="true"
             AllowNumberFormatting="true"
             AllowPdfExport="true"
             AllowExcelExport="true">
    <PivotViewDisplayOption Primary="Primary.Table" View="View.Both">
    </PivotViewDisplayOption>
    <PivotViewDataSourceSettings DataSource="@data"
                                  ShowGrandTotals="true" ShowSubTotals="true">
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
            <PivotViewFormatSetting Name="Amount" Format="C0"></PivotViewFormatSetting>
        </PivotViewFormatSettings>
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    SfPivotView<ProductDetails> pivot;
    public List<ProductDetails> data { get; set; }
    protected override void OnInitialized() => data = ProductDetails.GetProductData().ToList();

    public List<ToolbarItems> toolbar = new List<ToolbarItems>
    {
        ToolbarItems.New,
        ToolbarItems.Save,
        ToolbarItems.Grid,
        ToolbarItems.Chart,
        ToolbarItems.Export,
        ToolbarItems.SubTotal,
        ToolbarItems.GrandTotal,
        ToolbarItems.ConditionalFormatting,
        ToolbarItems.NumberFormatting,
        ToolbarItems.FieldList
    };
}
```

---

## Built-In Toolbar Items

| `ToolbarItems` | Action |
|---|---|
| `New` | Create a new pivot report |
| `Save` | Save the current report |
| `SaveAs` | Save current report with a new name |
| `Rename` | Rename the current report |
| `Remove` | Delete the current report |
| `Load` | Load a previously saved report |
| `Grid` | Switch to table/grid view |
| `Chart` | Switch to chart view (with chart type selector) |
| `Export` | Export as PDF, Excel, or CSV |
| `SubTotal` | Toggle sub-totals visibility |
| `GrandTotal` | Toggle grand totals visibility |
| `ConditionalFormatting` | Open conditional formatting dialog |
| `NumberFormatting` | Open number formatting dialog |
| `FieldList` | Toggle field list popup |
| `MDX` | Show MDX query (OLAP only) |

> The order of items in the `toolbar` list determines display order. Remove any item to hide it.

---

## Report Management (Save, Load, Rename)

The Save/Load/Rename toolbar items let users manage multiple named pivot reports. Handle the events to persist reports (e.g., to localStorage or a database):

```razor
<SfPivotView TValue="ProductDetails" ShowToolbar="true" Toolbar="@toolbar">
    ...
    <PivotViewEvents TValue="ProductDetails"
                     SaveReport="SaveReport"
                     LoadReport="LoadReport"
                     RenameReport="RenameReport"
                     RemoveReport="RemoveReport"
                     FetchReport="FetchReport">
    </PivotViewEvents>
</SfPivotView>

@code {
    List<string> reportNames = new List<string>();
    Dictionary<string, string> reports = new Dictionary<string, string>();
    SfPivotView<ProductDetails> pivot;

    public List<ToolbarItems> toolbar = new List<ToolbarItems>
    {
        ToolbarItems.New, ToolbarItems.Save, ToolbarItems.Load,
        ToolbarItems.Rename, ToolbarItems.Remove
    };

    void SaveReport(SaveReportArgs args)
    {
        if (!reports.ContainsKey(args.ReportName))
            reportNames.Add(args.ReportName);
        reports[args.ReportName] = args.Report;
    }

    void LoadReport(LoadReportArgs args)
    {
        if (reports.TryGetValue(args.ReportName, out var report))
            pivot.LoadPersistDataAsync(report);
    }

    void RenameReport(RenameReportArgs args)
    {
        reports[args.Rename] = reports[args.ReportName];
        reports.Remove(args.ReportName);
        int idx = reportNames.IndexOf(args.ReportName);
        if (idx >= 0) reportNames[idx] = args.Rename;
    }

    void RemoveReport(RemoveReportArgs args)
    {
        reports.Remove(args.ReportName);
        reportNames.Remove(args.ReportName);
    }

    void FetchReport(FetchReportArgs args) => args.ReportName = reportNames.ToArray();
}
```

---

## Custom Toolbar Items

Add custom buttons alongside built-in items:

```razor
<SfPivotView TValue="ProductDetails" ShowToolbar="true" Toolbar="@toolbar">
    ...
    <PivotViewEvents TValue="ProductDetails"
                     ToolbarRendered="ToolbarRender">
    </PivotViewEvents>
</SfPivotView>

@code {
    public List<ToolbarItems> toolbar = new List<ToolbarItems>
    {
        ToolbarItems.Grid, ToolbarItems.Export
    };

    void ToolbarRender(ToolbarArgs args)
    {
        args.CustomToolbar.Add(new ItemModel
        {
            Text = "Custom Export",
            TooltipText = "Export with custom settings",
            PrefixIcon = "e-icons e-export",
            Click = EventCallback.Factory.Create<ClickEventArgs>(this, CustomExport)
        });
    }

    void CustomExport(ClickEventArgs args) { /* custom logic */ }
}
```

---

## Toolbar Events

| Event | Fired When |
|---|---|
| `ToolbarRendered` | After toolbar is rendered — use to inject custom items |
| `BeforeExport` | Before export begins — modify export settings |
| `SaveReport` | User clicks Save |
| `LoadReport` | User clicks Load |
| `RenameReport` | User renames a report |
| `RemoveReport` | User deletes a report |
| `FetchReport` | Toolbar needs the list of saved report names |
