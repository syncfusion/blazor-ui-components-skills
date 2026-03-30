# Excel Export — Syncfusion Blazor Pivot Table

Excel export allows you to export the pivot table data to Microsoft Excel format with formatting, formulas, and styling preserved.

## Excel Export

Enable with `AllowExcelExport="true"` and call `ExportToExcelAsync()`:

```razor
@using Syncfusion.Blazor.PivotView
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="OnExcelExport" Content="Export to Excel"></SfButton>

<SfPivotView TValue="ProductDetails" @ref="pivot" AllowExcelExport="true">
    <PivotViewDataSourceSettings DataSource="@data" EnableSorting="true">
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
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    SfPivotView<ProductDetails> pivot;
    public List<ProductDetails> data { get; set; }
    protected override void OnInitialized() => data = ProductDetails.GetProductData().ToList();

    // false = direct download; true = returns MemoryStream
    public void OnExcelExport() => pivot.ExportToExcelAsync(false);
}
```

---

## CSV Export

Export as a comma-separated CSV file — useful for further processing in Excel or data pipelines:

```razor
<SfButton OnClick="OnCsvExport" Content="Export to CSV"></SfButton>

@code {
    // AllowExcelExport must be true for CSV export as well
    public void OnCsvExport() => pivot.ExportToCsvAsync(false);
}
```

---

## Export as Memory Stream

Pass `true` to get a `MemoryStream` instead of downloading — useful for server-side file generation or email attachments:

```razor
@code {
    public async Task ExportToStream()
    {
        // Returns MemoryStream — save to disk, email, or upload to storage
        MemoryStream stream = await pivot.ExportToExcelAsync(true) as MemoryStream;
        // e.g., save to file:
        await File.WriteAllBytesAsync("output.xlsx", stream.ToArray());
    }
}
```

---

## Custom File Name and Settings

### Excel export with custom file name

To set a custom file name, create an `ExcelExportProperties` object and pass it to the export method:

```csharp
// ✅ CORRECT — pass ExcelExportProperties with FileName
var exportProperties = new ExcelExportProperties { FileName = "MyReport.xlsx" };
await pivot.ExportToExcelAsync(exportProperties);
```

Full example:

```razor
@code {
    public async Task OnExcelExport()
    {
        var exportProperties = new ExcelExportProperties
        {
            FileName = "SalesReport_Q4_2024.xlsx"
        };
        await pivot.ExportToExcelAsync(exportProperties);
    }
}
```
