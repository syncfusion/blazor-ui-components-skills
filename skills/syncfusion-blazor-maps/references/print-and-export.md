## Table of Contents

- [Prerequisites for Export and Print (NEW - Important)](#prerequisites-for-export-and-print-new---important)
- [Exporting Maps](#exporting-maps)
   - [Export as PNG](#export-as-png)
   - [Export as SVG](#export-as-svg)
   - [Export as PDF](#export-as-pdf)
   - [Export Options](#export-options)
- [Print Functionality](#print-functionality)
   - [Basic Print](#basic-print)
   - [Custom Print Styling](#custom-print-styling)
   - [Page Setup for Printing](#page-setup-for-printing)
- [Export with Legend and Labels](#export-with-legend-and-labels)
   - [Including Map Elements](#including-map-elements)
- [Format Options Comparison](#format-options-comparison)
- [Export Workflow](#export-workflow)
   - [Complete Export Example](#complete-export-example)
- [Troubleshooting Export Issues](#troubleshooting-export-issues)
   - [Export dialog doesn't appear](#export-dialog-doesnt-appear)
   - [Quality issues in exported image](#quality-issues-in-exported-image)
   - [Export very slow](#export-very-slow)

# Print and Export

## Prerequisites for Export and Print (NEW - Important)

Ensure the following properties are enabled on your `SfMaps` component:

```csharp
<SfMaps AllowImageExport="true"   // ← Required for PNG/SVG export
        AllowPdfExport="true"     // ← Required for PDF export
        AllowPrint="true">        // ← Required for printing
    <!-- Map configuration -->
</SfMaps>
```

Without these properties set to `true`, export and print methods will not function.

---

## Exporting Maps

### Export as PNG

Export the current map view as PNG image:

```csharp
@page "/export-png"
@using Syncfusion.Blazor.Maps

<button @onclick="ExportAsPng">Export as PNG</button>

<SfMaps @ref="mapInstance" >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Latitude="37.368" Longitude="-122.095"
                    Shape="MarkerType.Circle" Width="15" Height="15">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition { Latitude = 39.0, Longitude = -98.0 };

    private async Task ExportAsPng()
    {
        await mapInstance.ExportAsync(ExportType.PNG, "map-export");
    }
}
```

The exported file is named `map-export.png` and downloads automatically.

### Export as SVG

SVG format preserves vector quality and is ideal for scaling:

```csharp
<button @onclick="ExportAsSvg">Export as SVG</button>

@code {
    private async Task ExportAsSvg()
    {
        await mapInstance.ExportAsync(ExportType.SVG, "map-export");
    }
}
```

SVG exports are crisp at any size and can be edited in vector design tools.

### Export as PDF

Export map as a PDF document with optional page orientation:

```csharp
<button @onclick="ExportAsPdfPortrait">Export as PDF (Portrait)</button>
<button @onclick="ExportAsPdfLandscape">Export as PDF (Landscape)</button>

@code {
    private async Task ExportAsPdfPortrait()
    {
        // Export with Portrait orientation (default)
        await mapInstance.ExportAsync(ExportType.PDF, "map-report", PdfPageOrientation.Portrait);
    }

    private async Task ExportAsPdfLandscape()
    {
        // Export with Landscape orientation
        await mapInstance.ExportAsync(ExportType.PDF, "map-report", PdfPageOrientation.Landscape);
    }
}
```

**PDF Orientation Options (NEW - Previously Missing):**

| Option | Description |
|--------|-------------|
| `PdfPageOrientation.Portrait` | Standard vertical page (8.5" x 11") |
| `PdfPageOrientation.Landscape` | Wide horizontal page (11" x 8.5") |

The PDF includes the map view at the time of export. Legends and labels are automatically included.

### Export Options

Control export parameters:

```csharp
@page "/export-advanced"
@using Syncfusion.Blazor.Maps

<div style="margin: 20px 0;">
    <label>
        <input type="checkbox" @bind="IncludeLegend" /> Include Legend
    </label>
    <label>
        <input type="checkbox" @bind="IncludeLabels" /> Include Labels
    </label>
    
    <label>
        Width: <input type="number" @bind="ExportWidth" min="100" max="2000" />
    </label>
    <label>
        Height: <input type="number" @bind="ExportHeight" min="100" max="2000" />
    </label>
</div>

<button @onclick="ExportWithOptions">Export</button>

<SfMaps @ref="mapInstance" >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
    <MapsLegendSettings Visible="@IncludeLegend">
    </MapsLegendSettings>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition { Latitude = 39.0, Longitude = -98.0 };
    
    private bool IncludeLegend = true;
    private bool IncludeLabels = true;
    private int ExportWidth = 800;
    private int ExportHeight = 600;

    private async Task ExportWithOptions()
    {
        // Export with current settings
        await mapInstance.ExportAsync(ExportType.PNG, "map", ExportWidth, ExportHeight);
    }
}
```

## Print Functionality

### Basic Print

Print the map with browser print dialog:

```csharp
@page "/print-map"
@using Syncfusion.Blazor.Maps
@inject IJSRuntime JS

<button @onclick="PrintMap">Print Map</button>

<SfMaps @ref="mapInstance" >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private int ZoomLevel = 4;

    private async Task PrintMap()
    {
        await mapInstance.PrintAsync();
    }
}
```

This opens the browser's print dialog. Users can select printer, page layout, and margins.

### Custom Print Styling

Optimize appearance when printed:

```html
<style media="print">
    /* Hide controls that aren't needed in print -->
    .no-print {
        display: none !important;
    }

    /* Optimize map for paper -->
    .e-maps {
        width: 100%;
        height: auto;
        page-break-inside: avoid;
    }

    /* Larger legend for readability -->
    .e-legend {
        font-size: 12px;
        margin-top: 20px;
    }

    /* Ensure layers print -->
    .e-layer {
        display: block !important;
    }
</style>

<button class="no-print" @onclick="PrintMap">Print</button>

<SfMaps @ref="mapInstance">
    <!-- Map content -->
</SfMaps>
```

### Page Setup for Printing

```csharp
@page "/print-setup"
@using Syncfusion.Blazor.Maps

<div style="margin: 20px 0;">
    <label>
        Paper Size:
        <select @bind="PaperSize">
            <option value="A4">A4</option>
            <option value="A3">A3</option>
            <option value="Letter">Letter</option>
            <option value="Legal">Legal</option>
        </select>
    </label>

    <label>
        <input type="checkbox" @bind="PrintInColor" /> Print in Color
    </label>

    <label>
        Margin: <input type="number" @bind="PrintMargin" min="0" max="50" /> mm
    </label>
</div>

<button @onclick="PrintMap">Print</button>

@code {
    private SfMaps mapInstance;
    private string PaperSize = "A4";
    private bool PrintInColor = true;
    private int PrintMargin = 10;

    private async Task PrintMap()
    {
        // Apply print styles
        // Note: Actual paper size/margin control depends on browser capabilities
        await mapInstance.PrintAsync();
    }
}
```

## Export with Legend and Labels

### Including Map Elements

```csharp
@page "/export-complete"
@using Syncfusion.Blazor.Maps

<button @onclick="ExportWithLegend">Export Map with Legend</button>

<SfMaps @ref="mapInstance" CenterPosition="@CenterLatLng" >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer TValue="StateData" DataSource="@StateData">
            <MapsShapeSettings ColorValuePath="Population">
                <MapsShapeColorMappings>
                    <MapsShapeColorMapping StartRange="0" EndRange="5000000" Color='new string[] { "#B3E5FC" }'>
                    </MapsShapeColorMapping>
                    <MapsShapeColorMapping StartRange="5000000" EndRange="30000000" Color='new string[] { "#0288D1" }'>
                    </MapsShapeColorMapping>
                </MapsShapeColorMappings>
            </MapsShapeSettings>
            <MapsDataLabelSettings Visible="true" LabelPath="Name">
            </MapsDataLabelSettings>
        </MapsLayer>
    </MapsLayers>
    <MapsLegendSettings Visible="true" Position="LegendPosition.BottomRight">
    </MapsLegendSettings>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition { Latitude = 39.0, Longitude = -98.0 };
    private List<StateData> StateData = new();

    private async Task ExportWithLegend()
    {
        // Map exports automatically include visible legend and labels
        await mapInstance.ExportAsync(ExportType.PNG, "map-with-legend");
    }

    public class StateData
    {
        public string Name { get; set; }
        public int Population { get; set; }
    }
}
```

## Format Options Comparison

| Format | Use Case | Pros | Cons |
|--------|----------|------|------|
| **PNG** | Web sharing, reports | Universal support, smaller file | Raster, loses quality when scaled |
| **SVG** | Printing, editing | Vector quality, editablein design tools | Larger file, limited browser support |
| **PDF** | Official documents, archiving | Portable, print-ready, preserves layout | Larger file size, fixed layout |

## Export Workflow

### Complete Export Example

```csharp
@page "/export-workflow"
@using Syncfusion.Blazor.Maps

<div style="border: 1px solid #ddd; padding: 20px; margin-bottom: 20px; border-radius: 4px;">
    <h3>Export Map</h3>
    
    <div>
        <label>
            Format:
            <select @bind="ExportFormat">
                <option value="PNG">PNG Image</option>
                <option value="SVG">SVG Vector</option>
                <option value="PDF">PDF Document</option>
            </select>
        </label>
    </div>

    <div style="margin: 10px 0;">
        <label>
            <input type="checkbox" @bind="ExportLegend" /> Include Legend
        </label>
        <label>
            <input type="checkbox" @bind="ExportWithMarkers" /> Include Markers
        </label>
    </div>

    <button @onclick="PerformExport" style="background: #007bff; color: white; padding: 10px 20px; border: none; border-radius: 4px; cursor: pointer;">
        @(IsExporting ? "Exporting..." : "Export")
    </button>
</div>

<SfMaps @ref="mapInstance" CenterPosition="@CenterLatLng" >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer TValue="StateData" DataSource="@StateData">
            <MapsShapeSettings ColorValuePath="Population">
            </MapsShapeSettings>
        </MapsLayer>
    </MapsLayers>
    @if (ExportLegend)
    {
        <MapsLegendSettings Visible="true" Position="LegendPosition.BottomRight">
        </MapsLegendSettings>
    }
</SfMaps>

@code {
    private SfMaps mapInstance;
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition { Latitude = 39.0, Longitude = -98.0 };
    
    private string ExportFormat = "PNG";
    private bool ExportLegend = true;
    private bool ExportWithMarkers = true;
    private bool IsExporting = false;
    private List<StateData> StateData = new();

    private async Task PerformExport()
    {
        IsExporting = true;

        try
        {
            var exportType = ExportFormat switch
            {
                "SVG" => ExportType.SVG,
                "PDF" => ExportType.PDF,
                _ => ExportType.PNG
            };

            var fileName = $"map-export-{DateTime.Now:yyyyMMdd-HHmmss}";
            await mapInstance.ExportAsync(exportType, fileName);
        }
        finally
        {
            IsExporting = false;
        }
    }

    public class StateData
    {
        public string Name { get; set; }
        public int Population { get; set; }
    }
}
```

## Troubleshooting Export Issues

### Export dialog doesn't appear

- Verify map is fully rendered before export
- Check browser popup blockers aren't preventing download
- Ensure export method is awaited

### Quality issues in exported image

- PNG: Use higher zoom level before export for better quality
- SVG: Best quality output, consider for high-precision needs
- PDF: Use with legend included for complete document

### Export very slow

- Reduce map complexity (fewer markers, simpler shapes)
- Export smaller area rather than full globe
- Use PNG for faster export compared to SVG/PDF

