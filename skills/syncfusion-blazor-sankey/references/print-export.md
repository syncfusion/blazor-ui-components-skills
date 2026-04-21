# Print and Export for Blazor Sankey Diagram

This guide explains how to print and export the Syncfusion Blazor Sankey Diagram to generate high-quality outputs for presentations, reports, or further analysis.

## Table of Contents

- [Overview](#overview)
- [Printing the Sankey Diagram](#printing-the-sankey-diagram)
- [Exporting the Sankey Diagram](#exporting-the-sankey-diagram)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Key Considerations](#key-considerations)
- [Summary](#summary)

## Overview

The Blazor Sankey Diagram provides built-in methods for printing and exporting diagrams to various formats. These features enable you to create physical copies, save diagrams as images, or generate files for documentation.

**Supported operations:**
- **Print**: Send diagram to printer via browser print dialog
- **Export**: Save diagram as PNG, JPEG, SVG, or PDF file

**Required namespace:**
```csharp
@using Syncfusion.Blazor.Sankey
```

## Printing the Sankey Diagram

The `PrintAsync` method invokes the browser's print dialog, allowing users to select print settings and send the diagram to a printer.

### Basic Printing

**Steps to implement printing:**
1. Create a reference to the `SfSankey` component using `@ref`
2. Call the `PrintAsync()` method on the reference
3. Optionally handle the `PrintCompleted` event

**Basic printing example:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey @ref="SankeyRef" 
          Width="100%" 
          Height="600px"
          Nodes="@Nodes" 
          Links="@Links">
</SfSankey>

<button class="btn btn-primary" @onclick="PrintDiagram">Print Diagram</button>

@code {
    private SfSankey SankeyRef;
    
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    private async Task PrintDiagram()
    {
        await SankeyRef.PrintAsync();
    }

    protected override void OnInitialized()
    {
        Nodes = new List<SankeyDataNode>()
        {
            new SankeyDataNode() { Id = "Solar", Label = new SankeyDataLabel() { Text = "Solar" } },
            new SankeyDataNode() { Id = "Wind", Label = new SankeyDataLabel() { Text = "Wind" } },
            new SankeyDataNode() { Id = "Electricity", Label = new SankeyDataLabel() { Text = "Electricity" } },
            new SankeyDataNode() { Id = "Residential", Label = new SankeyDataLabel() { Text = "Residential" } }
        };

        Links = new List<SankeyDataLink>()
        {
            new SankeyDataLink() { SourceId = "Solar", TargetId = "Electricity", Value = 100 },
            new SankeyDataLink() { SourceId = "Wind", TargetId = "Electricity", Value = 120 },
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Residential", Value = 220 }
        };
    }
}
```

### Handling Print Completion

Use the `PrintCompleted` event to respond when printing finishes:

```razor
<SfSankey @ref="SankeyRef" 
          Nodes="@Nodes" 
          Links="@Links"
          PrintCompleted="OnPrintCompleted">
</SfSankey>

<button @onclick="PrintDiagram">Print</button>

<div class="alert alert-info" style="display: @(ShowPrintMessage ? "block" : "none")">
    Print operation completed!
</div>

@code {
    private SfSankey SankeyRef;
    private bool ShowPrintMessage = false;

    private async Task PrintDiagram()
    {
        ShowPrintMessage = false;
        await SankeyRef.PrintAsync();
    }

    private void OnPrintCompleted()
    {
        ShowPrintMessage = true;
        StateHasChanged();
        
        // Hide message after 3 seconds
        Task.Delay(3000).ContinueWith(_ =>
        {
            ShowPrintMessage = false;
            InvokeAsync(StateHasChanged);
        });
    }

    // Nodes and links initialization
}
```

### Print Considerations

**Important points about printing:**
- Print appearance depends on browser and operating system
- User must have printer drivers correctly configured
- Print dialog is controlled by the browser, not the component
- Background colors may not print by default in some browsers
- Page breaks and margins are controlled by print settings

**Tips for better print results:**
- Use high-contrast colors that print well
- Test print preview before sending to printer
- Consider page size when designing diagram dimensions
- Avoid light backgrounds that waste ink
- Provide clear labels visible at print resolution

## Exporting the Sankey Diagram

The `ExportAsync` method exports the diagram to an image or document format. Supported formats include PNG, JPEG, SVG, and PDF.

### Export Method Signature

```csharp
Task ExportAsync(
    SankeyExportType type,      // Export format
    string fileName,             // Output filename
    Orientation? orientation,    // PDF orientation (optional)
    bool allowDownload,          // Auto-download file
    bool isBase64               // Return base64 string
)
```

**Parameters:**
- **type**: Export format (`SankeyExportType.PNG`, `JPEG`, `SVG`, `PDF`)
- **fileName**: Name for the exported file (without extension)
- **orientation**: PDF page orientation (optional, only for PDF)
- **allowDownload**: If true, automatically downloads the file
- **isBase64**: If true, returns base64-encoded data in `ExportCompleted` event

### Export Formats

**Available formats:**
- **PNG**: Raster format, good for web use and documents
- **JPEG**: Compressed raster format, smaller file size
- **SVG**: Vector format, scalable without quality loss
- **PDF**: Document format, ideal for reports and printing

**Format selection guide:**
- Use **PNG** for presentations and web (best quality, transparency support)
- Use **JPEG** when file size is critical (no transparency)
- Use **SVG** for scalable graphics or further editing
- Use **PDF** for professional documents and reports

### Basic Export Examples

**Export as PNG:**
```razor
<SfSankey @ref="SankeyRef" Nodes="@Nodes" Links="@Links">
</SfSankey>

<button @onclick="ExportAsPNG">Export as PNG</button>

@code {
    private SfSankey SankeyRef;

    private async Task ExportAsPNG()
    {
        await SankeyRef.ExportAsync(
            SankeyExportType.PNG,
            $"sankey-diagram-{DateTime.Now:yyyyMMdd}",
            null,
            true,   // Auto-download
            false   // Don't need base64
        );
    }

    // Nodes and links
}
```

**Export as JPEG:**
```razor
<button @onclick="ExportAsJPEG">Export as JPEG</button>

@code {
    private async Task ExportAsJPEG()
    {
        await SankeyRef.ExportAsync(
            SankeyExportType.JPEG,
            "sankey-energy-flow",
            null,
            true,
            false
        );
    }
}
```

**Export as SVG:**
```razor
<button @onclick="ExportAsSVG">Export as SVG</button>

@code {
    private async Task ExportAsSVG()
    {
        await SankeyRef.ExportAsync(
            SankeyExportType.SVG,
            "sankey-vector",
            null,
            true,
            false
        );
    }
}
```

**Export as PDF:**
```razor
<button @onclick="ExportAsPDF">Export as PDF</button>

@code {
    private async Task ExportAsPDF()
    {
        await SankeyRef.ExportAsync(
            SankeyExportType.PDF,
            "sankey-report",
            Orientation.Landscape,  // PDF orientation
            true,
            false
        );
    }
}
```

### Export with Event Handling

Use the `ExportCompleted` event to respond when export finishes:

**Example with export event:**
```razor
<SfSankey @ref="SankeyRef" 
          Nodes="@Nodes" 
          Links="@Links"
          ExportCompleted="OnExportCompleted">
</SfSankey>

<button @onclick="ExportDiagram">Export PNG</button>

<div class="status-message">@StatusMessage</div>

@code {
    private SfSankey SankeyRef;
    private string StatusMessage = "";

    private async Task ExportDiagram()
    {
        StatusMessage = "Exporting...";
        StateHasChanged();
        
        await SankeyRef.ExportAsync(
            SankeyExportType.PNG,
            $"sankey-{DateTime.Now.Ticks}",
            null,
            true,
            false
        );
    }

    private void OnExportCompleted(SankeyExportedEventArgs args)
    {
        StatusMessage = "Export completed successfully!";
        StateHasChanged();
        
        Console.WriteLine($"Export finished. Base64 length: {args.Base64?.Length ?? 0}");
    }

    // Nodes and links initialization
}
```

### Getting Base64 Data

Retrieve base64-encoded image data for custom processing:

```razor
<SfSankey @ref="SankeyRef" 
          Nodes="@Nodes" 
          Links="@Links"
          ExportCompleted="OnExportWithBase64">
</SfSankey>

<button @onclick="GetBase64Export">Get Base64</button>

<div>
    <h4>Exported Data:</h4>
    <textarea rows="5" style="width: 100%; font-size: 10px;">@Base64Data</textarea>
</div>

@code {
    private SfSankey SankeyRef;
    private string Base64Data = "";

    private async Task GetBase64Export()
    {
        await SankeyRef.ExportAsync(
            SankeyExportType.PNG,
            "sankey-base64",
            null,
            false,  // Don't auto-download
            true    // Return base64 data
        );
    }

    private void OnExportWithBase64(SankeyExportedEventArgs args)
    {
        Base64Data = args.Base64;
        StateHasChanged();
        
        // Could upload to server, embed in document, etc.
    }

    // Nodes and links initialization
}
```

### Export to Server

Upload exported data to server for processing or storage:

```razor
<SfSankey @ref="SankeyRef" 
          Nodes="@Nodes" 
          Links="@Links"
          ExportCompleted="OnExportToServer">
</SfSankey>

<button @onclick="ExportAndUpload">Export & Upload</button>

@code {
    private SfSankey SankeyRef;
    
    [Inject]
    private HttpClient Http { get; set; }

    private async Task ExportAndUpload()
    {
        await SankeyRef.ExportAsync(
            SankeyExportType.PNG,
            "sankey-upload",
            null,
            false,  // Don't download
            true    // Get base64
        );
    }

    private async void OnExportToServer(SankeyExportedEventArgs args)
    {
        try
        {
            var payload = new
            {
                FileName = $"sankey-{DateTime.Now:yyyyMMddHHmmss}.png",
                ImageData = args.Base64
            };
            
            var response = await Http.PostAsJsonAsync("/api/diagrams/upload", payload);
            
            if (response.IsSuccessStatusCode)
            {
                Console.WriteLine("Upload successful");
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Upload failed: {ex.Message}");
        }
    }

    // Nodes and links initialization
}
```

## Complete Examples

### Example 1: Multi-Format Export with User Choice

```razor
@using Syncfusion.Blazor.Sankey

<SfSankey @ref="SankeyRef" 
          Width="100%" 
          Height="600px"
          BackgroundColor="#f8f9fa"
          Nodes="@Nodes" 
          Links="@Links"
          ExportCompleted="OnExportCompleted">
    <SankeyNodeSettings Color="#0d6efd"></SankeyNodeSettings>
    <SankeyLinkSettings Color="#6c757d" Opacity="0.4"></SankeyLinkSettings>
    <SankeyLabelSettings Visible="true" Color="#212529"></SankeyLabelSettings>
</SfSankey>

<div class="export-controls">
    <h4>Export Options</h4>
    <div class="btn-group">
        <button class="btn btn-secondary" @onclick="() => Export(SankeyExportType.PNG)">
            PNG
        </button>
        <button class="btn btn-secondary" @onclick="() => Export(SankeyExportType.JPEG)">
            JPEG
        </button>
        <button class="btn btn-secondary" @onclick="() => Export(SankeyExportType.SVG)">
            SVG
        </button>
        <button class="btn btn-secondary" @onclick="() => Export(SankeyExportType.PDF)">
            PDF
        </button>
    </div>
    <button class="btn btn-primary" @onclick="PrintDiagram">
        Print
    </button>
</div>

<div class="status-bar">
    @StatusMessage
</div>

@code {
    private SfSankey SankeyRef;
    private string StatusMessage = "Ready";
    
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    private async Task Export(SankeyExportType format)
    {
        StatusMessage = $"Exporting as {format}...";
        StateHasChanged();
        
        string fileName = $"sankey-energy-flow-{DateTime.Now:yyyyMMdd-HHmmss}";
        
        await SankeyRef.ExportAsync(
            format,
            fileName,
            format == SankeyExportType.PDF ? Orientation.Landscape : null,
            true,
            false
        );
    }

    private async Task PrintDiagram()
    {
        StatusMessage = "Printing...";
        StateHasChanged();
        await SankeyRef.PrintAsync();
    }

    private void OnExportCompleted(SankeyExportedEventArgs args)
    {
        StatusMessage = "Export completed successfully!";
        StateHasChanged();
    }

    protected override void OnInitialized()
    {
        Nodes = new List<SankeyDataNode>()
        {
            new SankeyDataNode() { Id = "Solar", Label = new SankeyDataLabel() { Text = "Solar" } },
            new SankeyDataNode() { Id = "Wind", Label = new SankeyDataLabel() { Text = "Wind" } },
            new SankeyDataNode() { Id = "Hydro", Label = new SankeyDataLabel() { Text = "Hydro" } },
            new SankeyDataNode() { Id = "Electricity", Label = new SankeyDataLabel() { Text = "Electricity" } },
            new SankeyDataNode() { Id = "Residential", Label = new SankeyDataLabel() { Text = "Residential" } },
            new SankeyDataNode() { Id = "Commercial", Label = new SankeyDataLabel() { Text = "Commercial" } }
        };

        Links = new List<SankeyDataLink>()
        {
            new SankeyDataLink() { SourceId = "Solar", TargetId = "Electricity", Value = 100 },
            new SankeyDataLink() { SourceId = "Wind", TargetId = "Electricity", Value = 120 },
            new SankeyDataLink() { SourceId = "Hydro", TargetId = "Electricity", Value = 80 },
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Residential", Value = 150 },
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Commercial", Value = 150 }
        };
    }
}
```

### Example 2: Batch Export with Dark Theme

```razor
@using Syncfusion.Blazor.Sankey

<SfSankey @ref="SankeyRef" 
          Width="1200px" 
          Height="800px"
          BackgroundColor="#0b1320"
          Nodes="@Nodes" 
          Links="@Links"
          ExportCompleted="OnBatchExportCompleted">
    <SankeyNodeSettings Color="#1c3f60" Width="30"></SankeyNodeSettings>
    <SankeyLinkSettings Color="#afc1d0" Opacity="0.3"></SankeyLinkSettings>
    <SankeyLabelSettings Visible="true" Color="#ffffff" FontWeight="400"></SankeyLabelSettings>
    <SankeyLegendSettings Visible="false"></SankeyLegendSettings>
</SfSankey>

<button class="btn btn-success" @onclick="ExportAll">Export All Formats</button>
<button class="btn btn-primary" @onclick="PrintDiagram">Print</button>

<div class="progress-info">
    <p>Progress: @ExportProgress</p>
</div>

@code {
    private SfSankey SankeyRef;
    private string ExportProgress = "Ready";
    private int CurrentExport = 0;
    private SankeyExportType[] ExportFormats = 
    {
        SankeyExportType.PNG,
        SankeyExportType.JPEG,
        SankeyExportType.SVG,
        SankeyExportType.PDF
    };

    private async Task ExportAll()
    {
        CurrentExport = 0;
        
        foreach (var format in ExportFormats)
        {
            ExportProgress = $"Exporting {format}... ({CurrentExport + 1}/{ExportFormats.Length})";
            StateHasChanged();
            
            await SankeyRef.ExportAsync(
                format,
                $"sankey-{format.ToString().ToLower()}-{DateTime.Now:yyyyMMdd}",
                format == SankeyExportType.PDF ? Orientation.Landscape : null,
                true,
                false
            );
            
            // Wait for export to complete
            await Task.Delay(500);
            CurrentExport++;
        }
    }

    private async Task PrintDiagram()
    {
        ExportProgress = "Printing...";
        StateHasChanged();
        await SankeyRef.PrintAsync();
        ExportProgress = "Print dialog opened";
        StateHasChanged();
    }

    private void OnBatchExportCompleted(SankeyExportedEventArgs args)
    {
        if (CurrentExport >= ExportFormats.Length)
        {
            ExportProgress = "All exports completed!";
        }
        StateHasChanged();
    }

    // Nodes and links initialization
}
```

## Best Practices

1. **File Naming**: Use descriptive, timestamped filenames for organization
2. **Format Selection**: Choose appropriate format for intended use
3. **User Feedback**: Provide status messages during export/print operations
4. **Error Handling**: Wrap operations in try-catch blocks
5. **Testing**: Test across browsers and operating systems
6. **Performance**: Avoid exporting very large diagrams repeatedly
7. **Server Upload**: Use base64 for server upload scenarios
8. **Auto-Download**: Enable auto-download for better user experience
9. **Event Handling**: Use completion events for follow-up actions
10. **Browser Compatibility**: Test print/export in target browsers

## Troubleshooting

### Print Not Working

**Problem**: Print dialog doesn't appear or print fails

**Solutions**:
- Check that `@ref` attribute is correctly set
- Verify `PrintAsync()` is being called (check for null reference)
- Check browser console for JavaScript errors
- Ensure browser allows pop-ups/dialogs
- Test in different browsers
- Verify printer drivers are installed

### Export File Not Downloaded

**Problem**: Export completes but file doesn't download

**Solutions**:
- Verify `allowDownload` parameter is `true`
- Check browser download settings (blocked downloads)
- Look in browser's downloads folder
- Check for browser pop-up blockers
- Verify filename doesn't have invalid characters
- Test with simpler filename

### Export Quality Issues

**Problem**: Exported image looks pixelated or blurry

**Solutions**:
- Use PNG or SVG for best quality
- Increase diagram dimensions before export
- Avoid JPEG for diagrams with text and sharp edges
- Use SVG for scalable output
- Check display resolution settings

### Base64 Data Not Returned

**Problem**: `ExportCompleted` event doesn't contain base64 data

**Solutions**:
- Verify `isBase64` parameter is `true`
- Check that `ExportCompleted` event is wired up
- Ensure event handler has correct signature
- Check for null in `args.Base64`
- Verify export completed successfully

### PDF Orientation Not Applied

**Problem**: PDF exports in wrong orientation

**Solutions**:
- Verify `Orientation` parameter is set correctly
- Only applies to PDF format, not PNG/JPEG/SVG
- Check diagram dimensions match desired orientation
- Use `Orientation.Landscape` or `Orientation.Portrait`

### Memory Issues with Large Exports

**Problem**: Browser crashes or slows during export

**Solutions**:
- Reduce diagram size or complexity
- Export smaller sections separately
- Use SVG instead of high-resolution PNG
- Close other browser tabs
- Increase browser memory allocation
- Consider server-side rendering for very large diagrams

## Key Considerations

- **Printing**: Depends on browser and OS capabilities
- **Export Formats**: PNG (quality), JPEG (size), SVG (scalable), PDF (documents)
- **File Naming**: Use descriptive, unique filenames
- **Event Handling**: Use completion events for notifications and follow-up
- **Browser Compatibility**: Test across target browsers
- **Performance**: Be mindful of large diagram exports
- **User Experience**: Provide clear feedback during operations

## Summary

The Blazor Sankey Diagram provides robust print and export capabilities through `PrintAsync` and `ExportAsync` methods. Use printing for physical copies via browser print dialog. Export to PNG, JPEG, SVG, or PDF formats depending on requirements. Handle `PrintCompleted` and `ExportCompleted` events for notifications and follow-up actions. Retrieve base64 data for server uploads or custom processing. Configure exports with appropriate filenames, formats, and options for optimal results. Test across browsers and scenarios to ensure reliable operation in production environments.
