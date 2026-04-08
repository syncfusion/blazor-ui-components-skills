## Table of Contents

- [Security Warning](#security-warning)
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

> **MANDATORY SECURITY NOTICE:** Export and print examples may reference third-party tiles or GeoJSON for illustration only. In production you MUST ensure exported content does not leak sensitive data. Recommended controls:
>
> - Host tiles and GeoJSON locally or serve via a validated server-side proxy that enforces allow-lists, HTTPS, content-type checks, schema validation, size/complexity limits, and property sanitization.
> - Require user authorization for export features and log all export actions for audit.
> - Apply watermarks or redaction for sensitive layers before export.
> - Sanitize labels, tooltips, and annotations (HTML-encode or strip markup) prior to including them in exported files.
> - NEVER export or forward raw `ShapeData`, tooltips, or annotation content to automated agents or LLMs without explicit schema validation and human sign-off.
>
## Prerequisites for Export and Print (NEW - Important)

Ensure the following properties are enabled on your `SfMaps` component:

```csharp
<SfMaps AllowImageExport="true"   //  Required for PNG/SVG export
        AllowPdfExport="true"     //  Required for PDF export
        AllowPrint="true">        //  Required for printing
    <!-- Map configuration -->
</SfMaps>
```

Without these properties set to `true`, export and print methods will not function.

**Security Recommendation:** Only enable these properties for authorized users:

```csharp
<SfMaps AllowImageExport="@IsUserAuthorized"
        AllowPdfExport="@IsUserAuthorized"
        AllowPrint="@IsUserAuthorized">
    <!-- Map configuration -->
</SfMaps>

@code {
    private bool IsUserAuthorized { get; set; }
    
    protected override async Task OnInitializedAsync()
    {
        // Check user permissions
        IsUserAuthorized = await AuthService.CanExportMaps();
    }
}
```

---

## Exporting Maps

### Export as PNG

Export the current map view as PNG image:

```csharp
@page "/export-png"
@using Syncfusion.Blazor.Maps
<button @onclick="ExportAsPng">Export as PNG</button>
<SfMaps @ref="mapInstance" >
<MapsZoomSettings Enable="true" ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer UrlTemplate="/tiles/{level}/{tileX}/{tileY}.png" TValue="string">
            <MapsMarkerSettings>
                <MapsMarker Visible="true" Datasource="@MarkerDataSource" TValue="MarkerData"
                     Width="15" Height="15">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>
@code {
    private SfMaps mapInstance;
    public class MarkerData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
    };
    public List<MarkerData> MarkerDataSource = new List<MarkerData>
    {
        new MarkerData { Latitude = 47.60621, Longitude = -122.332071 }
    };

    private async Task ExportAsPng()
    {
        await mapInstance.ExportAsync(Syncfusion.Blazor.Maps.ExportType.PNG, "map-export");
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
        await mapInstance.ExportAsync(ExportType.PDF, "map-report", 0);
    }

    private async Task ExportAsPdfLandscape()
    {
        // Export with Landscape orientation
        await mapInstance.ExportAsync(ExportType.PDF, "map-report", 1);
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

<button @onclick="export">Export</button>
<SfMaps @ref="Maps" AllowPdfExport="true">
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    SfMaps Maps;
    string exportString;
    public async Task export()
    {
        exportString = await this.Maps.ExportAsync(Syncfusion.Blazor.Maps.ExportType.PDF, "Maps", 0, false);
        Console.WriteLine(exportString);
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

<SfMaps @ref="mapInstance" AllowPrint="true">
    <MapsLayers>
        <MapsLayer UrlTemplate="/tiles/{level}/{tileX}/{tileY}.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private async Task PrintMap()
    {
        await mapInstance.PrintAsync();
    }
}
```

This opens the browser's print dialog. Users can select printer, page layout, and margins.

## Export with Legend and Labels

### Including Map Elements

```csharp
@page "/export-complete"
@using Syncfusion.Blazor.Maps
<button @onclick="ExportWithLegend">Export Map with Legend</button>
<SfMaps @ref="mapInstance" AllowImageExport="true">
    <MapsLegendSettings Visible="true" Shape="Syncfusion.Blazor.Maps.LegendShape.Star" ShapeHeight="30" ShapeWidth="30" ShapePadding="10">
        <MapsLegendShapeBorder Color="blue" Width="2">
        </MapsLegendShapeBorder>
    </MapsLegendSettings>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions ="/data/world-map.json"}' ShapeDataPath="Country"
                   DataSource="MemberShipDetails" ShapePropertyPath='new string[] {"name"}' TValue="UNCouncil">
            <MapsDataLabelSettings Visible="true" LabelPath="name"></MapsDataLabelSettings>
            <MapsShapeSettings ColorValuePath="Membership">
                <MapsShapeColorMappings>
                    <MapsShapeColorMapping Value="Permanent" Color='new string[] {"#D84444"}' />
                    <MapsShapeColorMapping Value="Non-Permanent" Color='new string[] {"#316DB5"}' />
                </MapsShapeColorMappings>
            </MapsShapeSettings>
            <MapsMarkerSettings>
                <MapsMarker Visible="true" DataSource="MarkerData" Height="25" Width="15" TValue="City"
                            Shape="Syncfusion.Blazor.Maps.MarkerType.Image" ImageUrl="/images/ballon.png">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    public class City
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
    }

    public List<City> MarkerData = new List<City> {
        new City { Latitude  = 35.145083, Longitude = -117.960260 },
        new City { Latitude = 40.724546, Longitude = -73.850344 },
        new City { Latitude = 41.657782, Longitude = -91.533857 }
    };
    public class UNCouncil
    {
        public string Country { get; set; }
        public string Membership { get; set; }
    }

    private List<UNCouncil> MemberShipDetails = new List<UNCouncil>
    {
        new UNCouncil { Country = "China", Membership = "Permanent" },
        new UNCouncil { Country = "France", Membership = "Permanent" },
        new UNCouncil { Country = "Russia", Membership = "Permanent" },
        new UNCouncil { Country = "Kazakhstan", Membership = "Non-Permanent" },
        new UNCouncil { Country = "Poland", Membership = "Non-Permanent" },
        new UNCouncil { Country = "Sweden", Membership = "Non-Permanent" }
    };
    private async Task ExportWithLegend()
    {
        // Map exports automatically include visible legend and labels
        await mapInstance.ExportAsync(Syncfusion.Blazor.Maps.ExportType.PNG, "map-with-legend");
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
<button @onclick="ExportWithPNG">PNG</button>
<button @onclick="ExportWithJPEG">JPEG</button>
<button @onclick="ExportWithSVG">SVG</button>
<SfMaps @ref="mapInstance" AllowImageExport="true" AllowPdfExport="true" AllowPrint="true">
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions ="/data/world-map.json"}' ShapeDataPath="Country"
                   DataSource="MemberShipDetails" ShapePropertyPath='new string[] {"name"}' TValue="UNCouncil">
            <MapsShapeSettings ColorValuePath="Membership">
                <MapsShapeColorMappings>
                    <MapsShapeColorMapping Value="Permanent" Color='new string[] {"#D84444"}' />
                    <MapsShapeColorMapping Value="Non-Permanent" Color='new string[] {"#316DB5"}' />
                </MapsShapeColorMappings>
            </MapsShapeSettings>
            <MapsMarkerSettings>
                <MapsMarker Visible="true" DataSource="MarkerData" Height="25" Width="15" TValue="City" >
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    public class City
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
    }

    public List<City> MarkerData = new List<City> {
        new City { Latitude  = 35.145083, Longitude = -117.960260 },
        new City { Latitude = 40.724546, Longitude = -73.850344 },
        new City { Latitude = 41.657782, Longitude = -91.533857 }
    };
    public class UNCouncil
    {
        public string Country { get; set; }
        public string Membership { get; set; }
    }

    private List<UNCouncil> MemberShipDetails = new List<UNCouncil>
    {
        new UNCouncil { Country = "China", Membership = "Permanent" },
        new UNCouncil { Country = "France", Membership = "Permanent" },
        new UNCouncil { Country = "Russia", Membership = "Permanent" },
        new UNCouncil { Country = "Kazakhstan", Membership = "Non-Permanent" },
        new UNCouncil { Country = "Poland", Membership = "Non-Permanent" },
        new UNCouncil { Country = "Sweden", Membership = "Non-Permanent" }
    };
    private async Task ExportWithPNG()
    {
        await mapInstance.ExportAsync(Syncfusion.Blazor.Maps.ExportType.PNG, "PNG");
    }
    private async Task ExportWithJPEG()
    {
        await mapInstance.ExportAsync(Syncfusion.Blazor.Maps.ExportType.JPEG, "JPEG");
    }
    private async Task ExportWithSVG()
    {
        await mapInstance.ExportAsync(Syncfusion.Blazor.Maps.ExportType.SVG, "SVG");
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

