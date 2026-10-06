# Print and Export

## Table of Contents
- [Overview](#overview)
- [Print](#print)
- [Export](#export)
- [ExportAsync Parameters](#exportasync-parameters)
- [Export to PDF with Page Orientation](#export-to-pdf-with-page-orientation)
- [Export the Hierarchy Data as XLSX or CSV](#export-the-hierarchy-data-as-xlsx-or-csv)

## Overview

The print and export features capture the rendered `Blazor Sunburst Chart` so it can be shared, archived, or embedded in reports and presentations. Printing sends the current chart to the browser print dialog, while exporting generates a file in a chosen format without altering the live chart state.

The chart can be printed and exported using the `PrintAsync` and `ExportAsync` methods of `SfSunburstChart`.

## Print

The `PrintAsync` method opens the browser print dialog and lets users print the chart with the current drill state, title, data labels, legend, breadcrumbs, and theme applied.

```cshtml
@using Syncfusion.Blazor.Charts

<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 IdMemberPath="@nameof(RegionData.Id)"
                 ParentIdMemberPath="@nameof(RegionData.ParentId)"
                 LabelMemberPath="@nameof(RegionData.Label)"
                 ValueMemberPath="@nameof(RegionData.Population)"
                 @ref="Sunburst"
                 Width="100%" Height="600px">
</SfSunburstChart>

<button class="btn btn-secondary" @onclick="PrintSunburst">Print Sunburst Chart</button>

@code {
    private SfSunburstChart<RegionData>? Sunburst;

    private async Task PrintSunburst()
    {
        await Sunburst!.PrintAsync();
    }

    // The RegionData model and Regions data source are defined in
    // Working with Data (working-with-data.md). Bind your own hierarchy
    // data when reusing this snippet.
}
```

## Export

The `ExportAsync` method exports the currently rendered chart to the specified file format.

This example exports the chart as a `PNG` image:

```cshtml
<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 @ref="Sunburst"
                 ...>
</SfSunburstChart>

<button class="btn btn-secondary" @onclick="ExportSunburst">Export as PNG</button>

@code {
    private SfSunburstChart<RegionData>? Sunburst;

    private async Task ExportSunburst()
    {
        await Sunburst!.ExportAsync(ExportType.PNG, "SunburstChart");
    }
}
```

## ExportAsync Parameters

- `Type` — The export format as an `ExportType` enum value: `PNG`, `JPEG`, `SVG`, `PDF`, `XLSX`, or `CSV`.
- `FileName` — The name of the exported file. The file extension is appended automatically based on the selected format.
- `Orientation` — The page orientation for `PDF` export: `PdfPageOrientation.Portrait` or `PdfPageOrientation.Landscape`. This parameter is ignored for other formats.
- `AllowDownload` — When `true`, the browser downloads the exported file. When `false`, the generated content is returned as a Data URL string instead. Default is `true`.
- `IsBase64` — When `true` and `AllowDownload` is `false`, the generated content is returned as a Base64 string instead of a Data URL. Default is `false`.

## Export to PDF with Page Orientation

The `PDF` export supports page orientation through the `Orientation` parameter. This requires the `Syncfusion.PdfExport` namespace.

> **Required NuGet package:** PDF export depends on the `Syncfusion.PdfExport.Net` package. Install it in the project that performs the export before adding the `@using Syncfusion.PdfExport` directive:
>
> ```bash
> dotnet add package Syncfusion.PdfExport.Net -v 35.x.x
> ```
>
> The other export formats (`PNG`, `JPEG`, `SVG`, `XLSX`, `CSV`) do not require this package.

```cshtml
@using Syncfusion.Blazor.Charts
@using Syncfusion.PdfExport

<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 @ref="Sunburst"
                 Width="100%" Height="600px">
</SfSunburstChart>

<button class="btn btn-secondary" @onclick="ExportPdf">Export as PDF</button>

@code {
    private SfSunburstChart<RegionData>? Sunburst;

    private async Task ExportPdf()
    {
        await Sunburst!.ExportAsync(ExportType.PDF, "SunburstChart", PdfPageOrientation.Landscape);
    }
}
```

> **Supported export formats:** `ExportAsync` renders the visual chart to `PNG`, `JPEG`, `SVG`, and `PDF`, and writes the generated hierarchy data to `XLSX` and `CSV`.

## Export the Hierarchy Data as XLSX or CSV

Passing `ExportType.XLSX` or `ExportType.CSV` to `ExportAsync` writes a parent-first flattened representation of the hierarchy with `Id`, `ParentId`, `Label`, and `Value` columns. This is useful when the underlying data needs to be shared for further analysis in a spreadsheet.

> **What `Value` contains:** XLSX/CSV exports write the raw source `ValueMemberPath` values from each bound row — not the rendered aggregate values the chart computes for non-leaf segments. If a non-leaf row leaves `ValueMemberPath` unset or `0` in the source data, the exported cell is `0`. Consumers that need the aggregated totals should recompute them from the leaf rows or rely on PNG/JPEG/SVG/PDF exports, which capture the rendered visual.

```cshtml
@using Syncfusion.Blazor.Charts

<SfSunburstChart TItem="RegionData"
                 Title="Population by Region"
                 DataSource="@Regions"
                 @ref="Sunburst"
                 Width="100%" Height="600px">
</SfSunburstChart>

<button class="btn btn-secondary" @onclick="ExportHierarchy">Export hierarchy as XLSX</button>

@code {
    private SfSunburstChart<RegionData>? Sunburst;

    private async Task ExportHierarchy()
    {
        await Sunburst!.ExportAsync(ExportType.XLSX, "SunburstHierarchy");
    }
}
```

> You can customize or cancel an export through the `Exporting` event and get notified when the print or export workflow finishes through the `PrintCompleted` and `ExportCompleted` events. See [Events](events.md).

## See also

- [Events](events.md)
- [Legend](legend.md)
- [Data Label](data-label.md)
