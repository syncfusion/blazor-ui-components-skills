# Data Visualization and Mapping

## Table of Contents

- [Color Mapping and Choropleth](#color-mapping-and-choropleth)
   - [Basic Color Mapping](#basic-color-mapping)
   - [Equal Range Intervals](#equal-range-intervals)
   - [Quantile-Based Classification](#quantile-based-classification)
   - [Multiple Data Fields](#multiple-data-fields)
- [Legends](#legends)
   - [Basic Legend](#basic-legend)
   - [Legend with Custom Title and Labels](#legend-with-custom-title-and-labels)
   - [Legend Positioning Options](#legend-positioning-options)
   - [Toggle Legend Visibility](#toggle-legend-visibility)
- [Data Labels](#data-labels)
   - [Labels on Shapes (Regions)](#labels-on-shapes-regions)
   - [Custom Label Formatting](#custom-label-formatting)
   - [Labels on Bubbles](#labels-on-bubbles)
   - [Conditional Labels](#conditional-labels)
- [Populating Maps with Data](#populating-maps-with-data)
   - [Binding Data to Shapes](#binding-data-to-shapes)
   - [Real-Time Data Updates](#real-time-data-updates)
- [API Reference for Data Visualization](#api-reference-for-data-visualization)
   - [MapsShapeSettings](#mapsshapesettings)
   - [MapsShapeColorMapping](#mapsshapecolormapping)
   - [MapsDataLabelSettings](#mapsdatalabelsettings)
   - [MapsLegendSettings](#mapslegendsettings)
   - [MapsBubbleSettings](#mapsbubblesettings)
   - [MapsColorMapping](#mapscolormapping)
- [Advanced Data Binding](#advanced-data-binding)
   - [Multiple Data Sources](#multiple-data-sources)
   - [Aggregated Data Visualization](#aggregated-data-visualization)
   - [Data Filtering by Attribute](#data-filtering-by-attribute)


## Color Mapping and Choropleth

Color mapping visualizes data values across geographic regions by applying colors based on data ranges. This technique (called choropleth) is perfect for displaying statistics like population density, income levels, or disease prevalence by region.

> **Security Note:** GeoJSON and map tile data are typically fetched from external sources (CDNs, tile servers). This skill uses legitimate services:
> - **Syncfusion CDN** (`cdn.syncfusion.com`) - Official vendor resource for geographic datasets
> - **OpenStreetMap** (`tile.openstreetmap.org`) - Well-known public tile service
> 
> When loading data from other sources, validate URLs against trusted domain allow-lists. See [Map Providers - Security Best Practices](map-providers.md#security-best-practices).

### Basic Color Mapping

```csharp
@page "/choropleth-map"
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <MapsLayer TValue="StateData"
            ShapeData='new { dataOptions = "https://cdn.syncfusion.com/maps/map-data/usa.json" }'
            DataSource="@StateDataSource"
            ShapeDataPath="Name"
            ShapePropertyPath='new string[] { "name" }'>
            <MapsShapeSettings Fill="#E5E5E5" ColorValuePath="Population">
                <MapsShapeColorMappings>
                    <MapsShapeColorMapping StartRange="0" EndRange="1000000" Color='new string[] { "#B3E5FC" }'>
                    </MapsShapeColorMapping>
                    <MapsShapeColorMapping StartRange="1000000" EndRange="5000000" Color='new string[] { "#81D4FA" }'>
                    </MapsShapeColorMapping>
                    <MapsShapeColorMapping StartRange="5000000" EndRange="10000000" Color='new string[] { "#4FC3F7" }'>
                    </MapsShapeColorMapping>
                    <MapsShapeColorMapping StartRange="10000000" EndRange="30000000" Color='new string[] { "#29B6F6" }'>
                    </MapsShapeColorMapping>
                </MapsShapeColorMappings>
            </MapsShapeSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {

    private object StateGeoJson;
    private List<StateData> StateDataSource = new()
    {
        new StateData { Name = "California", Population = 39000000 },
        new StateData { Name = "Texas", Population = 29000000 },
        new StateData { Name = "Florida", Population = 21500000 },
        // ... more states ...
    };

    public class StateData
    {
        public string Name { get; set; }
        public int Population { get; set; }
    }
}
```

### Equal Range Intervals

Create equal-width ranges automatically:

```csharp
<MapsShapeColorMappings>
    <MapsShapeColorMapping StartRange="0" EndRange="25000" Color='new string[] { "rgb(173, 216, 230)" }'>
    </MapsShapeColorMapping>
    <MapsShapeColorMapping StartRange="25000" EndRange="50000" Color='new string[] { "rgb(135, 206, 250)" }'>
    </MapsShapeColorMapping>
    <MapsShapeColorMapping StartRange="50000" EndRange="75000" Color='new string[] { "rgb(0, 150, 255)" }'>
    </MapsShapeColorMapping>
    <MapsShapeColorMapping StartRange="75000" EndRange="100000" Color='new string[] { "rgb(0, 100, 200)" }'>
    </MapsShapeColorMapping>
</MapsShapeColorMappings>
```

### Quantile-Based Classification

Use data quartiles for better distribution:

```csharp
<MapsShapeColorMappings>
    <!-- 0-25th percentile -->
    <MapsShapeColorMapping StartRange="0" EndRange="500000" Color='new string[] { "#FEE5D9" }'>
    </MapsShapeColorMapping>
    <!-- 25-50th percentile -->
    <MapsShapeColorMapping StartRange="500000" EndRange="2000000" Color='new string[] { "#FCAE91" }'>
    </MapsShapeColorMapping>
    <!-- 50-75th percentile -->
    <MapsShapeColorMapping StartRange="2000000" EndRange="8000000" Color='new string[] { "#FB6A4A" }'>
    </MapsShapeColorMapping>
    <!-- 75-100th percentile -->
    <MapsShapeColorMapping StartRange="8000000" EndRange="40000000" Color='new string[] { "#CB181D" }'>
    </MapsShapeColorMapping>
</MapsShapeColorMappings>
```

### Multiple Data Fields

Map different fields to different color ranges:

```csharp
<MapsLayer TValue="string">
    <!-- First visualization: Population density -->
    <MapsShapeSettings ColorValuePath="PopulationDensity">
        <MapsShapeColorMappings>
            <MapsShapeColorMapping StartRange="0" EndRange="100" Color='new string[] { "lightgreen" }'>
            </MapsShapeColorMapping>
            <MapsShapeColorMapping StartRange="100" EndRange="1000" Color='new string[] { "orange" }'>
            </MapsShapeColorMapping>
        </MapsShapeColorMappings>
    </MapsShapeSettings>

    <!-- Alternative: Unemployment rate (toggle with button) -->
    @if (ShowUnemployment)
    {
        <MapsShapeSettings ColorValuePath="UnemploymentRate">
            <MapsShapeColorMappings>
                <MapsShapeColorMapping StartRange="0" EndRange="5" Color='new string[] { "lightblue" }'>
                </MapsShapeColorMapping>
                <MapsShapeColorMapping StartRange="5" EndRange="15" Color='new string[] { "red" }'>
                </MapsShapeColorMapping>
            </MapsShapeColorMappings>
        </MapsShapeSettings>
    }
</MapsLayer>

@code {
    private bool ShowUnemployment = false;
}
```

## Legends

Legends explain the color mapping and help users interpret the visualization. They should appear clearly and update dynamically when data changes.

### Basic Legend

```csharp
<SfMaps>
    <MapsLayers>
        <MapsLayer TValue="string">
            <MapsShapeSettings ColorValuePath="Population">
                <MapsShapeColorMappings>
                    <MapsShapeColorMapping StartRange="0" EndRange="1000000" Color='new string[] { "#B3E5FC" }'>
                    </MapsShapeColorMapping>
                    <MapsShapeColorMapping StartRange="1000000" EndRange="10000000" Color='new string[] { "#0288D1" }'>
                    </MapsShapeColorMapping>
                </MapsShapeColorMappings>
            </MapsShapeSettings>
        </MapsLayer>
    </MapsLayers>
    <MapsLegendSettings Visible="true" Position="LegendPosition.Bottom">
    </MapsLegendSettings>
</SfMaps>
```

### Legend with Custom Title and Labels

```csharp
<MapsLegendSettings Visible="true" Position="Syncfusion.Blazor.Maps.LegendPosition.Bottom"              LabelDisplayMode="Syncfusion.Blazor.Maps.LabelIntersectAction.Hide">
        <MapsLegendTitle Text="Population by State">
            <MapsLegendTitleStyle Size = "16px" Color = "black" FontWeight = "bold" />
        </MapsLegendTitle>
</MapsLegendSettings>
```

### Legend Positioning Options

```csharp
<!-- Left -->
<MapsLegendSettings Visible="true" Position="LegendPosition.Left">
</MapsLegendSettings>

<!-- Top -->
<MapsLegendSettings Visible="true" Position="LegendPosition.Top">
</MapsLegendSettings>

<!-- Right -->
<MapsLegendSettings Visible="true" Position="LegendPosition.Right">
</MapsLegendSettings>

<!-- Left, Bottom, Right, Top, Float -->
```

### Toggle Legend Visibility

```csharp
@page "/legend-toggle"
@using Syncfusion.Blazor.Maps

<button @onclick="() => ShowLegend = !ShowLegend"> Legend </button>

<SfMaps>
    <MapsLayers>
        <MapsLayer TValue="string">
            <!-- Layer configuration -->
        </MapsLayer>
    </MapsLayers>
    <MapsLegendSettings Visible="@ShowLegend" Position="Syncfusion.Blazor.Maps.LegendPosition.Bottom"               LabelDisplayMode="Syncfusion.Blazor.Maps.LabelIntersectAction.Hide">
    </MapsLegendSettings>
</SfMaps>

@code {
    private bool ShowLegend = true;
}
```

## Data Labels

Data labels display text directly on map elements (regions, bubbles, markers) to show values, names, or statistics.

### Labels on Shapes (Regions)

```csharp
<MapsLayer ShapeData='new {dataOptions= "https://cdn.syncfusion.com/maps/map-data/usa.json"}' TValue="string">
    <MapsDataLabelSettings Visible="true" LabelPath="name" />
</MapsLayer>
```

This displays the state name on each state polygon.

### Custom Label Formatting

```csharp
<MapsLayer ShapeData='new {dataOptions= "https://cdn.syncfusion.com/maps/map-data/usa.json"}' TValue="string">
    <MapsDataLabelSettings Visible="true" LabelPath="name">
        <MapsLayerDataLabelBorder Color="green" Width="2"></MapsLayerDataLabelBorder>
        <MapsLayerDataLabelTextStyle Color="blue" Size="12px" FontStyle="Sans-serif" FontWeight="normal">
        </MapsLayerDataLabelTextStyle>
    </MapsDataLabelSettings>
</MapsLayer>
```

### Conditional Labels

Show labels only for selected features:

```csharp
@page "/conditional-labels"
@using Syncfusion.Blazor.Maps
<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "https://cdn.syncfusion.com/maps/map-data/usa.json"}' TValue="string">
            <MapsDataLabelSettings Visible="@ShowAllLabels" LabelPath="name">
            </MapsDataLabelSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>
<button @onclick="() => ShowAllLabels = !ShowAllLabels"> Labels </button>

@code {
    private bool ShowAllLabels = false;
}
```

## Populating Maps with Data

### Binding Data to Shapes

Match geographic boundaries with your data:

```csharp
@using Syncfusion.Blazor.Maps
<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions ="https://cdn.syncfusion.com/maps/map-data/world-map.json"}' DataSource="PopulationDetails"
		    ShapeDataPath="Name" ShapePropertyPath='new string[] {"name"}' TValue="PopulationDetail">
            <MapsShapeSettings Fill="#E5E5E5" ColorValuePath="Density">
                <MapsShapeColorMappings>
                    <MapsShapeColorMapping StartRange="0.00001" EndRange="100" Color='new string[] {"yellow"}' />
                    <MapsShapeColorMapping StartRange="100" EndRange="400" Color='new string[] {"green"}' />
                </MapsShapeColorMappings>
            </MapsShapeSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    public class PopulationDetail
    {
        public string Code { get; set; }
        public double Value { get; set; }
        public string Name { get; set; }
        public double Population { get; set; }
        public double Density { get; set; }
    };
    public List<PopulationDetail> PopulationDetails = new List<PopulationDetail> {
       new PopulationDetail { Code = "US", Value = 34, Name ="United States", Population = 325020000, Density = 33 },
       new PopulationDetail { Code ="RU", Value = 9, Name = "Russia", Population = 142905208, Density = 8.3 },
       new PopulationDetail { Code = "In", Value = 384, Name = "India", Population = 1198003000, Density = 364 },
       new PopulationDetail { Code = "CN", Value = 143, Name = "China", Population = 1389750000,Density = 144 }
    };
}
```

**Key:** `ShapePropertyPath` must match property names in your GeoJSON data.

### Real-Time Data Updates

```csharp
@page "/live-data-map"
@using Syncfusion.Blazor.Maps
@using System.Threading

<SfMaps @ref="mapInstance">
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "https://cdn.syncfusion.com/maps/map-data/world-map.json"}' TValue="RegionData" DataSource="@LiveData">
            <MapsShapeSettings ColorValuePath="CurrentValue">
                <MapsShapeColorMappings>
                    <MapsShapeColorMapping StartRange="0" EndRange="50" Color='new string[] { "green" }' />
                    <MapsShapeColorMapping StartRange="50" EndRange="100" Color='new string[] { "red" }' />
                </MapsShapeColorMappings>
            </MapsShapeSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private List<RegionData> LiveData = new();
    private PeriodicTimer updateTimer;
    private CancellationTokenSource cts = new();

    protected override async Task OnInitializedAsync()
    {
        LiveData = await LoadInitialData();
        StartLiveUpdates();
    }

    private async void StartLiveUpdates()
    {
        updateTimer = new PeriodicTimer(TimeSpan.FromSeconds(5));

        try
        {
            while (await updateTimer.WaitForNextTickAsync(cts.Token))
            {
                await InvokeAsync(async () =>
                {
                    LiveData = await FetchLatestData();
                    mapInstance.Refresh();
                });
            }
        }
        catch (OperationCanceledException)
        {
            // Expected on dispose
        }
    }

    public void Dispose()
    {
        cts.Cancel();
        updateTimer?.Dispose();
    }

    private Task<List<RegionData>> LoadInitialData()
    {
        return Task.FromResult(new List<RegionData>());
    }

    private Task<List<RegionData>> FetchLatestData()
    {
        return Task.FromResult(new List<RegionData>());
    }

    public class RegionData
    {
        public string RegionId { get; set; }
        public double CurrentValue { get; set; }
    }
}
```

## API Reference for Data Visualization

### MapsShapeSettings

Configure shape appearance and data mapping.

```csharp
<MapsShapeSettings Fill="lightblue"
                   Opacity="0.7"
                   ColorValuePath="population"
                   DashArray="5,5">
    <MapsShapeColorMappings>
        <MapsShapeColorMapping StartRange="0" EndRange="50000" Color="#B3E5FC"></MapsShapeColorMapping>
        <MapsShapeColorMapping StartRange="50000" EndRange="100000" Color="#81D4FA"></MapsShapeColorMapping>
    </MapsShapeColorMappings>
</MapsShapeSettings>
```

| Property | Type | Description |
|----------|------|-------------|
| `Fill` | string | Default shape color |
| `DashArray` | string | SVG dash pattern for outline (e.g., "5,5") |
| `Opacity` | double | Transparency (0-1) |
| `ColorValuePath` | string | Data field for color mapping |
| `ValuePath` | string | Data field for values |
| `BorderColorValuePath` | string | Data field for border color |
| `BorderWidthValuePath` | string | Data field for border width |

### MapsShapeColorMapping

Maps data ranges to colors.

```csharp
<MapsShapeColorMapping StartRange="0" 
                       EndRange="50000" 
                       Color='new string[] {"#B3E5FC"}'
                       Label="Low">
</MapsShapeColorMapping>
```

| Property | Type | Description |
|----------|------|-------------|
| `StartRange` | double | Range minimum |
| `EndRange` | double | Range maximum |
| `Color` | string | Color for range (hex or named) |
| `Label` | string | Legend label |

### MapsDataLabelSettings

Configure data labels on shapes.

```csharp
<MapsDataLabelSettings Visible="true"
            LabelPath="name"
            SmartLabelMode="Syncfusion.Blazor.Maps.SmartLabelMode.Hide"
            IntersectionAction="Syncfusion.Blazor.Maps.IntersectAction.Trim">
    <MapsLayerDataLabelBorder Color="green" Width="2"></MapsLayerDataLabelBorder>
    <MapsLayerDataLabelTextStyle Color="blue" Size="12px" FontFamily="Arial">
    </MapsLayerDataLabelTextStyle>
</MapsDataLabelSettings>
```

| Property | Type | Description |
|----------|------|-------------|
| `Visible` | bool | Show/hide labels |
| `LabelPath` | string | Data field for label text |
| `SmartLabelMode` | `SmartLabelMode` | Overflow handling: None, Trim, Hide, Wrap |
| `IntersectAction` | `IntersectAction` | Intersection handling |

### MapsLegendSettings

Configure legend appearance.

```csharp
<MapsLegendSettings Visible="true"
                    Position="Syncfusion.Blazor.Maps.LegendPosition.Bottom"
                    Mode="Syncfusion.Blazor.Maps.LegendMode.Default"
                    Type="Syncfusion.Blazor.Maps.LegendType.Layers"
                    Width="200"
                    Height="100">
    <MapsLegendTitle Text="Population">
        <MapsLegendTitleStyle Size="14px" FontWeight="bold">
        </MapsLegendTitleStyle>
    </MapsLegendTitle>
    <MapsLegendTextStyle Size="12px">
    </MapsLegendTextStyle>
</MapsLegendSettings>
```

| Property | Type | Description |
|----------|------|-------------|
| `Visible` | bool | Show/hide legend |
| `Position` | `LegendPosition` | Placement: Top, Bottom, Left, Right, etc. |
| `Mode` | `LegendMode` | Default (static) or Interactive (clickable) |
| `Type` | `LegendType` | Items shown: Layers, Markers, Bubbles, Shapes |
| `Orientation` | `LegendArrangement` | Layout: Vertical or Horizontal |
| `Width` | double | Legend width |
| `Height` | double | Legend height |
| `ToggleVisibility` | bool | Allow clicking items to toggle visibility |

### MapsBubbleSettings

Configure bubble visualization.

```csharp
@using Syncfusion.Blazor.Maps
<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions ="https://cdn.syncfusion.com/maps/map-data/world-map.json"}'
                   ShapeDataPath="Name" ShapePropertyPath='new string[] {"name"}' TValue="Country">
            @* To add bubbles based on population count *@
            <MapsBubbleSettings>
                <MapsBubble Visible="true" ValuePath="Population" ColorValuePath="Color" MinRadius=20 MaxRadius=40 
                            DataSource="PopulationDetails" TValue="Country">
                </MapsBubble>
            </MapsBubbleSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    public class Country
    {
        public string Name { get; set; }
        public double Population { get; set; }
        public string Color { get; set; }
    };
    public List<Country> PopulationDetails = new List<Country> {
       new Country { Name = "Australia", Population = 325020000, Color = "#0000FF" },
       new Country { Name = "Russia", Population = 142905208, Color = "#09156D" },
       new Country { Name = "India", Population = 1198003000, Color = "#C2D2D6" }
    };
}
```

| Property | Type | Description |
|----------|------|-------------|
| `MinRadius` | double | Smallest bubble radius |
| `MaxRadius` | double | Largest bubble radius |
| `ValuePath` | string | Data field for bubble size |
| `ColorValuePath` | string | Data field for bubble color |
| `Fill` | string | Default bubble color |
| `Opacity` | double | Transparency |

### MapsColorMapping

Maps values to colors.

```csharp
public class MapsColorMapping
{
    public double From { get; set; }
    public double To { get; set; }
    public string Color { get; set; }
    public string Label { get; set; }
}
```

## Advanced Data Binding

### Multiple Data Sources

Layer multiple datasets on the same map:

```csharp
<MapsLayers>
    <!-- Population data -->
    <MapsLayer TValue="PopulationData"
        DataSource="@PopulationData">
        <MapsShapeSettings ColorValuePath="Population">
        </MapsShapeSettings>
    </MapsLayer>

    <!-- Economic data overlay -->
    <MapsLayer TValue="EconomicData"
        DataSource="@EconomicData">
        <MapsShapeSettings ColorValuePath="GdpPerCapita" Opacity="0.5">
        </MapsShapeSettings>
    </MapsLayer>

    <!-- Markers for cities -->
    <MapsLayer>
        <MapsMarkerSettings DataSource="@CityMarkers">
            <MapsMarker Visible="true" DataSource="MarkerDataSourceOne" />
            </MapsMarker>
            <MapsMarker Visible="true" DataSource="MarkerDataSourceTwo" />
            </MapsMarker>
        </MapsMarkerSettings>
    </MapsLayer>
</MapsLayers>
```

### Aggregated Data Visualization

Combine multiple data points into regional summaries:

```csharp
@page "/aggregated-data-map"
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "https://cdn.syncfusion.com/maps/map-data/world-map.json"}' TValue="RegionalSales"
            DataSource="@AggregatedData">
            <MapsShapeSettings ColorValuePath="TotalSales">
                <MapsShapeColorMappings>
                    <MapsShapeColorMapping StartRange="0" EndRange="100000" Color='new string[] { "lightgreen" }'>
                    </MapsShapeColorMapping>
                    <MapsShapeColorMapping StartRange="100000" EndRange="500000" Color='new string[] { "orange" }'>
                    </MapsShapeColorMapping>
                    <MapsShapeColorMapping StartRange="500000" EndRange="1000000" Color='new string[] { "red" }'>
                    </MapsShapeColorMapping>
                </MapsShapeColorMappings>
            </MapsShapeSettings>
            <MapsDataLabelSettings Visible="true" LabelPath="RegionName">
            </MapsDataLabelSettings>
        </MapsLayer>
    </MapsLayers>
    <MapsLegendSettings Visible="true" Position="Syncfusion.Blazor.Maps.LegendPosition.Bottom">
    </MapsLegendSettings>
</SfMaps>

@code {
    private List<RegionalSales> AggregatedData = new()
    {
        new RegionalSales { RegionName = "West", TotalSales = 250000 },
        new RegionalSales { RegionName = "Central", TotalSales = 180000 },
        new RegionalSales { RegionName = "East", TotalSales = 420000 }
    };
    public class RegionalSales
    {
        public string RegionName { get; set; }
        public decimal TotalSales { get; set; }
    }
}
```

### Data Filtering by Attribute

Filter and visualize specific data subsets:

```csharp
<button @onclick='() => FilterData("high")'>High Values</button>
<button @onclick='() => FilterData("all")'>All Values</button>

<SfMaps @ref="mapInstance" >
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "https://cdn.syncfusion.com/maps/map-data/world-map.json"}' TValue="DataPoint"
            DataSource="@FilteredData">
            <MapsShapeSettings ColorValuePath="Value">
            </MapsShapeSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private List<DataPoint> FullData = new();
    private List<DataPoint> FilteredData = new();

    private async Task FilterData(string filter)
    {
        FilteredData = filter switch
        {
            "high" => FullData.Where(d => d.Value > 75).ToList(),
            "low" => FullData.Where(d => d.Value <= 25).ToList(),
            _ => FullData
        };

        mapInstance.Refresh();
    }

    public class DataPoint
    {
        public string RegionId { get; set; }
        public double Value { get; set; }
    }
}
```

