# Print and Export in Blazor Chart Wizard Component

## Table of Contents

1. [Overview](#overview)
2. [Supported Export Formats](#supported-export-formats)
   - [PNG](#png)
   - [JPEG](#jpeg)
   - [SVG](#svg)
   - [PDF](#pdf)
   - [CSV](#csv)
   - [XLSX](#xlsx)
   - [PRINT](#print)
3. [Configuring Export Options](#configuring-export-options)
   - [ChartExportSettings Properties](#chartexportsettings-properties)
   - [Basic Configuration Example](#basic-configuration-example)
4. [Export Configuration Properties](#export-configuration-properties)
5. [Customizing Exports with the Exporting Event](#customizing-exports-with-the-exporting-event)
   - [ChartExportingEventArgs Properties](#chartexportingeventargs-properties)
   - [Event Handler Example](#event-handler-example)
6. [Complete Implementation Examples](#complete-implementation-examples)
   - [Basic Export Configuration](#basic-export-configuration)
   - [Advanced Export with Event Handler](#advanced-export-with-event-handler)
7. [Export Type Specifications](#export-type-specifications)
   - [Image Exports (PNG, JPEG, SVG)](#image-exports-png-jpeg-svg)
   - [Document Exports (PDF)](#document-exports-pdf)
   - [Data Exports (CSV, XLSX)](#data-exports-csv-xlsx)
   - [Print Export](#print-export)
8. [Best Practices](#best-practices)
9. [Troubleshooting](#troubleshooting)
   - [Common Issues](#common-issues)
   - [Solutions](#solutions)

---

## Overview

The Chart Wizard support the export of current chart to a variety of popular file formats. This feature enables users to:
- Save charts as images for presentations and documents
- Export raw data for further analysis
- Print charts for physical distribution
- Share visualizations across different platforms

---

## Supported Export Formats

### PNG
- **Type:** Raster Image
- **Use Case:** Web graphics, presentations, documentation
- **Features:** Lossless compression, transparency support
- **File Extension:** `.png`

### JPEG
- **Type:** Raster Image
- **Use Case:** Photos, web images requiring smaller file sizes
- **Features:** Lossy compression, smaller file sizes
- **File Extension:** `.jpg` or `.jpeg`

### SVG
- **Type:** Vector Image
- **Use Case:** Scalable graphics, logos, high-quality prints
- **Features:** Resolution independent, editable in vector software
- **File Extension:** `.svg`

### PDF
- **Type:** Document
- **Use Case:** Reports, presentations, archival
- **Features:** Page orientation support, print-ready format
- **File Extension:** `.pdf`

### CSV
- **Type:** Data File
- **Use Case:** Data analysis, spreadsheet import
- **Features:** Exports underlying chart data, widely compatible
- **File Extension:** `.csv`

### XLSX
- **Type:** Spreadsheet
- **Use Case:** Excel analysis, formatted data export
- **Features:** Exports data with formatting, formula support
- **File Extension:** `.xlsx`

### PRINT
- **Type:** Browser Print
- **Use Case:** Physical copies, print-to-PDF
- **Features:** Opens browser print dialog, configurable orientation
- **File Extension:** None (prints directly)

**Important Note:**
> `PRINT` opens the browser's print dialog and prints the rendered chart. This does not generate a downloadable file, but is ideal for creating physical copies or printing to PDF using your browser's print-to-PDF feature.

---

## Configuring Export Options

### ChartExportSettings Properties

Configure export behavior declaratively using the `ChartExportSettings` within `ChartSettings`. The main properties include:

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `FileName` | string | "Chart" | Sets the file name of the export (without extension) |
| `Width` | int | Chart width | Sets the requested output width in pixels for image/PDF exports |
| `Height` | int | Chart height | Sets the requested output height in pixels for image/PDF exports |
| `Orientation` | PageOrientation | Portrait | Sets the page orientation for PDF/print (`Portrait` or `Landscape`) |

### Basic Configuration Example

```cshtml
@using Syncfusion.Blazor.ChartWizard

<div class="control-section">
    <SfChartWizard>
        <ChartSettings DataSource="@OlympicsDataSource"
                       CategoryFields="@categories"
                       SeriesFields="@chartSeries"
                       SeriesType="ChartWizardSeriesType.Bar">
            <ChartExportSettings FileName="OlympicsReport" 
                                Width="800" 
                                Height="500" 
                                Orientation="PageOrientation.Landscape" />
        </ChartSettings>
    </SfChartWizard>
</div>

@code {
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country", "CountryCode" };

    private readonly List<OlympicsData> OlympicsDataSource = new()
    {
        new OlympicsData { Country = "USA", CountryCode = "USA", Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsData { Country = "China", CountryCode = "CHN", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsData { Country = "Great Britain", CountryCode = "GBR", Gold = 14, Silver = 22, Bronze = 29 },
        new OlympicsData { Country = "France", CountryCode = "FRA", Gold = 16, Silver = 26, Bronze = 22 },
        new OlympicsData { Country = "Australia", CountryCode = "AUS", Gold = 18, Silver = 19, Bronze = 16 },
        new OlympicsData { Country = "Japan", CountryCode = "JPN", Gold = 20, Silver = 12, Bronze = 13 },
        new OlympicsData { Country = "Italy", CountryCode = "ITA", Gold = 12, Silver = 13, Bronze = 15 },
        new OlympicsData { Country = "Netherlands", CountryCode = "NLD", Gold = 15, Silver = 7,  Bronze = 12 },
        new OlympicsData { Country = "Germany", CountryCode = "DEU", Gold = 12, Silver = 13, Bronze = 8  },
        new OlympicsData { Country = "South Korea", CountryCode = "KOR", Gold = 13, Silver = 9,  Bronze = 10 }
    };

    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }
}
```

---

## Export Configuration Properties

**FileName:**
```csharp
<ChartExportSettings FileName="QuarterlyReport" />
// Exports: QuarterlyReport.png, QuarterlyReport.pdf, etc.
```

**Width and Height:**
```csharp
<ChartExportSettings Width="1920" Height="1080" />
// Full HD resolution export
```

**Orientation:**
```csharp
<ChartExportSettings Orientation="PageOrientation.Portrait" />
// Portrait: 8.5" x 11"
<ChartExportSettings Orientation="PageOrientation.Landscape" />
// Landscape: 11" x 8.5"
```

---

## Customizing Exports with the Exporting Event

### ChartExportingEventArgs Properties

When an export is triggered, the component fires an `Exporting` event and supplies a `ChartExportingEventArgs` instance. You can use this to customize the export process. Available fields include:

| Property | Type | Description |
|----------|------|-------------|
| `FileName` | string | Sets the filename to use for the exported file (without extension). The component will append the appropriate extension for the selected export type. |
| `Cancel` | bool | Set to `true` to cancel the export operation. |
| `Width` | int | Sets width in pixels to use for the exported output (when supported by the export type); useful to control output resolution. |
| `Height` | int | Sets height in pixels to use for the exported output (when supported by the export type). |
| `Orientation` | PageOrientation | Sets page orientation for printable exports (`Portrait` or `Landscape`). Applies primarily to PDF/Print workflows. |

### Event Handler Example

Here's an example of handling the `Exporting` event:

```cshtml
@using Syncfusion.Blazor.ChartWizard

<div class="control-section">
    <SfChartWizard Exporting="OnExporting">
        <ChartSettings DataSource="@OlympicsDataSource"
                       CategoryFields="@categories"
                       SeriesFields="@chartSeries"
                       SeriesType="ChartWizardSeriesType.Bar">
            <ChartExportSettings FileName="Medals" />
        </ChartSettings>
    </SfChartWizard>
</div>

@code {
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country", "CountryCode" };

    private void OnExporting(ChartExportingEventArgs args)
    {
        // Set a custom file name
        args.FileName = "OlympicsMedalDetails";

        // Set explicit output dimensions when generating image/pdf outputs
        args.Width = 950; // pixels
        args.Height = 650; // pixels

        // Set page orientation for PDF/print exports
        args.Orientation = PageOrientation.Landscape; // or "Portrait"

        if (OlympicsDataSource == null || OlympicsDataSource.Count == 0)
        {
            // prevent export
            args.Cancel = true;
        }
    }

    private readonly List<OlympicsData> OlympicsDataSource = new()
    {
        new OlympicsData { Country = "USA", CountryCode = "USA", Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsData { Country = "China", CountryCode = "CHN", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsData { Country = "Great Britain", CountryCode = "GBR", Gold = 14, Silver = 22, Bronze = 29 },
        new OlympicsData { Country = "France", CountryCode = "FRA", Gold = 16, Silver = 26, Bronze = 22 },
        new OlympicsData { Country = "Australia", CountryCode = "AUS", Gold = 18, Silver = 19, Bronze = 16 },
        new OlympicsData { Country = "Japan", CountryCode = "JPN", Gold = 20, Silver = 12, Bronze = 13 },
        new OlympicsData { Country = "Italy", CountryCode = "ITA", Gold = 12, Silver = 13, Bronze = 15 },
        new OlympicsData { Country = "Netherlands", CountryCode = "NLD", Gold = 15, Silver = 7,  Bronze = 12 },
        new OlympicsData { Country = "Germany", CountryCode = "DEU", Gold = 12, Silver = 13, Bronze = 8  },
        new OlympicsData { Country = "South Korea", CountryCode = "KOR", Gold = 13, Silver = 9,  Bronze = 10 }
    };

    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }
}
```

**Important Notes:**
> - Use `args.Cancel = true` to prevent exporting when needed.
> - The `FileName` property should not include a file extension; the component will add the correct extension based on the selected `ExportType`.
> - Data export formats like `CSV` and `XLSX` export the underlying data table and do not use `Width`, `Height`, or `Orientation`.

---

## Complete Implementation Examples

### Basic Export Configuration

```cshtml
@using Syncfusion.Blazor.ChartWizard

<div class="control-section">
    <SfChartWizard Width="100%" Height="600px">
        <ChartSettings DataSource="@SalesData"
                       CategoryFields="@categories"
                       SeriesFields="@series"
                       SeriesType="ChartWizardSeriesType.Column">
            <ChartExportSettings 
                FileName="SalesReport" 
                Width="1200" 
                Height="800" 
                Orientation="PageOrientation.Landscape" />
        </ChartSettings>
    </SfChartWizard>
</div>

@code {
    private List<string> series = new() { "Sales", "Revenue" };
    private List<string> categories = new() { "Month" };
    
    private List<SalesData> SalesData = new()
    {
        new SalesData { Month = "Jan", Sales = 35, Revenue = 28 },
        new SalesData { Month = "Feb", Sales = 28, Revenue = 35 },
        new SalesData { Month = "Mar", Sales = 34, Revenue = 32 },
        new SalesData { Month = "Apr", Sales = 32, Revenue = 34 },
        new SalesData { Month = "May", Sales = 40, Revenue = 39 },
        new SalesData { Month = "Jun", Sales = 32, Revenue = 35 }
    };
    
    public class SalesData
    {
        public string? Month { get; set; }
        public int Sales { get; set; }
        public int Revenue { get; set; }
    }
}
```

### Advanced Export with Event Handler

```cshtml
@using Syncfusion.Blazor.ChartWizard
@using System.Globalization

<div class="control-section">
    <div class="toolbar-section">
        <button class="btn btn-primary" @onclick="ExportToPNG">Export PNG</button>
        <button class="btn btn-primary" @onclick="ExportToPDF">Export PDF</button>
        <button class="btn btn-primary" @onclick="ExportToExcel">Export Excel</button>
    </div>
    
    <SfChartWizard @ref="ChartWizardRef" 
                   Width="100%" 
                   Height="600px"
                   Exporting="OnExporting">
        <ChartSettings DataSource="@OlympicsDataSource"
                       CategoryFields="@categories"
                       SeriesFields="@chartSeries"
                       SeriesType="ChartWizardSeriesType.Bar">
            <ChartExportSettings FileName="Olympics" Width="1000" Height="700" />
        </ChartSettings>
    </SfChartWizard>
</div>

@code {
    private SfChartWizard? ChartWizardRef;
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country", "CountryCode" };
    private string exportFormat = "";

    private void OnExporting(ChartExportingEventArgs args)
    {
        // Add timestamp to filename
        string timestamp = DateTime.Now.ToString("yyyyMMdd_HHmmss", CultureInfo.InvariantCulture);
        args.FileName = $"{args.FileName}_{timestamp}";
        
        // Adjust dimensions based on export format
        if (exportFormat == "PNG" || exportFormat == "JPEG")
        {
            args.Width = 1920;  // Full HD width
            args.Height = 1080; // Full HD height
        }
        else if (exportFormat == "PDF")
        {
            args.Width = 1000;
            args.Height = 700;
            args.Orientation = PageOrientation.Landscape;
        }
        
        // Log export operation
        Console.WriteLine($"Exporting chart as {exportFormat}: {args.FileName}");
    }
    
    private void ExportToPNG()
    {
        exportFormat = "PNG";
        // Trigger export through UI
    }
    
    private void ExportToPDF()
    {
        exportFormat = "PDF";
        // Trigger export through UI
    }
    
    private void ExportToExcel()
    {
        exportFormat = "XLSX";
        // Trigger export through UI
    }

    private readonly List<OlympicsData> OlympicsDataSource = new()
    {
        new OlympicsData { Country = "USA", CountryCode = "USA", Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsData { Country = "China", CountryCode = "CHN", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsData { Country = "Great Britain", CountryCode = "GBR", Gold = 14, Silver = 22, Bronze = 29 },
        new OlympicsData { Country = "France", CountryCode = "FRA", Gold = 16, Silver = 26, Bronze = 22 },
        new OlympicsData { Country = "Australia", CountryCode = "AUS", Gold = 18, Silver = 19, Bronze = 16 },
        new OlympicsData { Country = "Japan", CountryCode = "JPN", Gold = 20, Silver = 12, Bronze = 13 },
        new OlympicsData { Country = "Italy", CountryCode = "ITA", Gold = 12, Silver = 13, Bronze = 15 },
        new OlympicsData { Country = "Netherlands", CountryCode = "NLD", Gold = 15, Silver = 7,  Bronze = 12 },
        new OlympicsData { Country = "Germany", CountryCode = "DEU", Gold = 12, Silver = 13, Bronze = 8  },
        new OlympicsData { Country = "South Korea", CountryCode = "KOR", Gold = 13, Silver = 9,  Bronze = 10 }
    };

    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }
}
```

---

## Export Type Specifications

### Image Exports (PNG, JPEG, SVG)

**Characteristics:**
- Support custom width and height
- Resolution determines quality
- Transparency support (PNG, SVG only)
- File size varies with complexity

**Best Practices:**
```csharp
// High-quality export
args.Width = 1920;
args.Height = 1080;

// Standard quality
args.Width = 1024;
args.Height = 768;

// Thumbnail
args.Width = 400;
args.Height = 300;
```

### Document Exports (PDF)

**Characteristics:**
- Supports page orientation
- Print-ready format
- Vector-based (sharp at any zoom)
- Supports custom dimensions

**Best Practices:**
```csharp
// Landscape A4
args.Width = 1122;  // 297mm at 96 DPI
args.Height = 793;  // 210mm at 96 DPI
args.Orientation = PageOrientation.Landscape;

// Portrait Letter
args.Width = 816;   // 8.5" at 96 DPI
args.Height = 1056; // 11" at 96 DPI
args.Orientation = PageOrientation.Portrait;
```

### Data Exports (CSV, XLSX)

**Characteristics:**
- Export underlying data only
- No visual formatting preserved
- Width, Height, Orientation ignored
- Ideal for data analysis

**Example Output (CSV):**
```
Country,CountryCode,Gold,Silver,Bronze
USA,USA,40,44,42
China,CHN,40,27,24
Great Britain,GBR,14,22,29
```

### Print Export

**Characteristics:**
- Opens browser print dialog
- No file generated
- Supports orientation setting
- Uses browser's print settings

**Best Practices:**
```csharp
private void OnExporting(ChartExportingEventArgs args)
{
    // Optimize for printing
    args.Orientation = PageOrientation.Landscape;
    args.Width = 1122;
    args.Height = 793;
}
```

---

## Best Practices

1. **Always provide meaningful filenames:**
   ```csharp
   args.FileName = $"SalesReport_{DateTime.Now:yyyy-MM}";
   ```

2. **Set appropriate dimensions for export type:**
   ```csharp
   private void OnExporting(ChartExportingEventArgs args)
   {
       if (args.ExportType == ExportType.PDF)
       {
           args.Width = 1000;
           args.Height = 700;
       }
       else if (args.ExportType == ExportType.PNG)
       {
           args.Width = 1920;
           args.Height = 1080;
       }
   }
   ```

3. **Validate data before export:**
   ```csharp
   private void OnExporting(ChartExportingEventArgs args)
   {
       if (DataSource == null || !DataSource.Any())
       {
           args.Cancel = true;
           ShowNotification("No data available for export");
       }
   }
   ```

4. **Use appropriate export formats for different scenarios:**
   - **PNG:** Presentations, web sharing
   - **PDF:** Reports, archival
   - **XLSX:** Data analysis
   - **SVG:** High-quality prints, scalable graphics

5. **Consider file size:**
   ```csharp
   // Balance quality and file size
   args.Width = 1280;  // Reasonable quality
   args.Height = 720;  // Manageable file size
   ```

6. **Handle export errors gracefully:**
   ```csharp
   private void OnExporting(ChartExportingEventArgs args)
   {
       try
       {
           // Validation logic
       }
       catch (Exception ex)
       {
           args.Cancel = true;
           LogError($"Export failed: {ex.Message}");
       }
   }
   ```

---

## Troubleshooting

### Common Issues

#### Issue 1: Export Button Not Working

**Symptoms:**
- Clicking export button does nothing
- No file download initiated

**Causes:**
- Chart not fully rendered
- Data not loaded
- Browser popup blocker

**Solutions:**
```csharp
// Ensure chart is ready
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await Task.Delay(500); // Allow chart to fully render
        StateHasChanged();
    }
}

// Check data availability
private void OnExporting(ChartExportingEventArgs args)
{
    if (DataSource == null || !DataSource.Any())
    {
        args.Cancel = true;
        Console.WriteLine("Cannot export: No data available");
        return;
    }
}
```

#### Issue 2: Exported Image Quality Poor

**Symptoms:**
- Blurry or pixelated exports
- Text not readable

**Causes:**
- Insufficient width/height settings
- Using JPEG for complex graphics

**Solutions:**
```csharp
// Increase resolution
private void OnExporting(ChartExportingEventArgs args)
{
    // High DPI export (2x)
    args.Width = ChartWidth * 2;
    args.Height = ChartHeight * 2;
}

// Use PNG instead of JPEG for better quality
<ChartExportSettings FileName="HighQuality" />
// Then select PNG format in the export menu
```

#### Issue 3: PDF Export Orientation Wrong

**Symptoms:**
- PDF exports in wrong orientation
- Content clipped or not fitting page

**Causes:**
- Orientation not set correctly
- Dimensions don't match orientation

**Solutions:**
```csharp
private void OnExporting(ChartExportingEventArgs args)
{
    if (args.ExportType == ExportType.PDF)
    {
        // Landscape orientation
        args.Orientation = PageOrientation.Landscape;
        args.Width = 1122;  // A4 width in landscape
        args.Height = 793;  // A4 height in landscape
    }
}
```

#### Issue 4: CSV/XLSX Export Empty or Incomplete

**Symptoms:**
- Exported file has no data
- Missing columns or rows

**Causes:**
- Data not properly bound
- Complex data structures not supported

**Solutions:**
```csharp
// Ensure data is properly structured
public class ExportableData
{
    // Simple properties only
    public string Category { get; set; }
    public int Value1 { get; set; }
    public int Value2 { get; set; }
    
    // Avoid complex nested objects for data export
}

// Verify data before export
private void OnExporting(ChartExportingEventArgs args)
{
    if (args.ExportType == ExportType.CSV || args.ExportType == ExportType.XLSX)
    {
        Console.WriteLine($"Exporting {DataSource.Count} rows");
        
        if (DataSource.Count == 0)
        {
            args.Cancel = true;
        }
    }
}
```

#### Issue 5: Filename Has Invalid Characters

**Symptoms:**
- Export fails silently
- File downloads with garbled name

**Causes:**
- Special characters in filename
- Path separators in filename

**Solutions:**
```csharp
private void OnExporting(ChartExportingEventArgs args)
{
    // Sanitize filename
    args.FileName = SanitizeFilename(args.FileName);
}

private string SanitizeFilename(string filename)
{
    // Remove invalid characters
    var invalidChars = Path.GetInvalidFileNameChars();
    var sanitized = string.Join("_", filename.Split(invalidChars));
    
    // Limit length
    if (sanitized.Length > 200)
    {
        sanitized = sanitized.Substring(0, 200);
    }
    
    return sanitized;
}
```

#### Issue 6: Print Dialog Shows Blank Page

**Symptoms:**
- Print preview shows empty page
- Nothing prints

**Causes:**
- Chart not fully rendered before print
- Browser compatibility issues

**Solutions:**
```csharp
private async Task PrintChart()
{
    // Wait for chart to render
    await Task.Delay(1000);
    
    // Then trigger print
    await JSRuntime.InvokeVoidAsync("window.print");
}

private void OnExporting(ChartExportingEventArgs args)
{
    if (args.ExportType == ExportType.Print)
    {
        // Ensure proper orientation
        args.Orientation = PageOrientation.Landscape;
        
        // Set reasonable dimensions
        args.Width = 1000;
        args.Height = 700;
    }
}
```

#### Issue 7: Large Data Export Fails or Times Out

**Symptoms:**
- Export hangs or fails with large datasets
- Browser becomes unresponsive

**Causes:**
- Too much data to process
- Memory limitations

**Solutions:**
```csharp
private void OnExporting(ChartExportingEventArgs args)
{
    const int MAX_ROWS = 10000;
    
    if (DataSource.Count > MAX_ROWS)
    {
        args.Cancel = true;
        ShowWarning($"Dataset too large ({DataSource.Count} rows). Maximum is {MAX_ROWS} rows.");
        return;
    }
}

// Alternative: Implement pagination or filtering
private List<DataItem> GetExportableData()
{
    // Return top N items
    return DataSource.Take(10000).ToList();
}
```
