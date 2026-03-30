# PDF Export — Syncfusion Blazor Pivot Table

PDF export allows you to generate high-quality PDF documents from the pivot table with customizable page orientation, themes, and formatting.

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

## PDF Export with Customization

### Page Orientation and Theme

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

## Export as Memory Stream

Pass `true` to get a `MemoryStream` instead of downloading — useful for server-side file generation or email attachments:

```razor
@code {
    public async Task ExportToStream()
    {
        // Returns MemoryStream — save to disk, email, or upload to storage
        MemoryStream stream = await pivot.ExportToPdfAsync(true) as MemoryStream;
        // e.g., save to file:
        await File.WriteAllBytesAsync("output.pdf", stream.ToArray());
    }
}
```
