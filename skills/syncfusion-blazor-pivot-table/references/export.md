# Export (Excel & PDF) — Syncfusion Blazor Pivot Table

## Table of Contents
- [Excel Export](#excel-export)
- [CSV Export](#csv-export)
- [PDF Export](#pdf-export)
- [Export via Toolbar](#export-via-toolbar)
- [Custom File Name and Settings](#custom-file-name-and-settings)
- [Export as Memory Stream](#export-as-memory-stream)
- [Export Chart with Table](#export-chart-with-table)

---

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

## PDF Export

Enable with `AllowPdfExport="true"` and call `ExportToPdfAsync()`:

```razor
@using Syncfusion.Blazor.PivotView
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="OnPdfExport" Content="Export to PDF"></SfButton>

<SfPivotView TValue="ProductDetails" @ref="pivot" AllowPdfExport="true">
    <PivotViewDataSourceSettings DataSource="@data">
        ...
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    SfPivotView<ProductDetails> pivot;
    public List<ProductDetails> data { get; set; }
    protected override void OnInitialized() => data = ProductDetails.GetProductData().ToList();

    public void OnPdfExport() => pivot.ExportToPdfAsync();
}
```

---

## Export via Toolbar

Add `ToolbarItems.Export` to the toolbar — users can choose Excel, PDF, or CSV from a dropdown:

```razor
<SfPivotView TValue="ProductDetails" ShowToolbar="true"
             Toolbar="@toolbar"
             AllowExcelExport="true"
             AllowPdfExport="true">
    ...
</SfPivotView>

@code {
    public List<ToolbarItems> toolbar = new List<ToolbarItems>
    {
        ToolbarItems.Export
    };
}
```

---

## Custom File Name and Settings

> **⚠️ Custom File Name API — Common Mistake:**
>
> To set a custom file name, create an `ExcelExportProperties` (or `PdfExportProperties`) object and pass it to the export method. Do NOT try to set `FileName` directly on an event args object — there is no such pattern.
>
> ```csharp
> // ✅ CORRECT — pass ExcelExportProperties with FileName
> var exportProperties = new ExcelExportProperties { FileName = "MyReport.xlsx" };
> await pivot.ExportToExcelAsync(exportProperties);
>
> // ❌ WRONG — args.FileName does not exist in this context
> // args.FileName = "MyReport.xlsx";
> ```

### Excel export with custom file name and header

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

### PDF export with page orientation and theme

```razor
@code {
    public async Task OnPdfExport()
    {
        var exportProperties = new PdfExportProperties
        {
            FileName = "SalesReport.pdf",
            PageOrientation = PageOrientation.Landscape,
            Theme = new PdfTheme
            {
                Header = new PdfThemeStyle { Bold = true, FontSize = 14 },
                Record = new PdfThemeStyle { FontSize = 10 }
            }
        };
        await pivot.ExportToPdfAsync(exportProperties);
    }
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

## Export Chart with Table

When both the pivot grid and pivot chart are visible, export them together:

```razor
<SfPivotView TValue="ProductDetails" @ref="pivot"
             AllowPdfExport="true" AllowExcelExport="true">
    <PivotViewDisplayOption View="View.Both" Primary="Primary.Table">
    </PivotViewDisplayOption>
    ...
</SfPivotView>

@code {
    // Export both grid and chart to PDF on the same page
    public void ExportAll() => pivot.ExportToPdfAsync();
}
```
