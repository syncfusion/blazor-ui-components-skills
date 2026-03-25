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

### Basic Color Mapping

```csharp
@page "/choropleth-map"
@using Syncfusion.Blazor.Maps

<SfMaps >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
        <MapsLayer TValue="StateData"
            ShapeData='new { dataOptions = "https://cdn.syncfusion.com/maps/map-data/world-map.json" }'
            DataSource="@StateData"
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
    private List<StateData> StateData = new()
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

    protected override async Task OnInitializedAsync()
    {
        StateGeoJson = await LoadStateGeoJson();
    }

    private async Task<object> LoadStateGeoJson()
    {
        // Load GeoJSON with state boundaries
        return new { };
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
<MapsLayer TValue="ShapeData">
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
    <MapsLegendSettings Visible="true" Position="LegendPosition.BottomRight">
    </MapsLegendSettings>
</SfMaps>
```

### Legend with Custom Title and Labels

```csharp
<MapsLegendSettings Visible="true" Position="LegendPosition.BottomLeft"
    Title="Population by State"
    TitleStyle="@TitleStyle"
    LabelDisplayMode="LabelIntersectAction.Hide">
</MapsLegendSettings>

@code {
    private TitleStyle TitleStyle = new TitleStyle
    {
        FontWeight = "bold",
        Size = "16px",
        Color = "black"
    };
}
```

### Legend Positioning Options

```csharp
<!-- Top-Left -->
<MapsLegendSettings Visible="true" Position="LegendPosition.TopLeft">
</MapsLegendSettings>

<!-- Top-Center -->
<MapsLegendSettings Visible="true" Position="LegendPosition.TopCenter">
</MapsLegendSettings>

<!-- Top-Right -->
<MapsLegendSettings Visible="true" Position="LegendPosition.TopRight">
</MapsLegendSettings>

<!-- Bottom-Left, Bottom-Center, Bottom-Right, etc. -->
```

### Toggle Legend Visibility

```csharp
@page "/legend-toggle"
@using Syncfusion.Blazor.Maps

<button @onclick="() => ShowLegend = !ShowLegend">
    @(ShowLegend ? "Hide" : "Show") Legend
</button>

<SfMaps @ref="mapInstance">
    <MapsLayers>
        <MapsLayer TValue="string">
            <!-- Layer configuration -->
        </MapsLayer>
    </MapsLayers>
    @if (ShowLegend)
    {
        <MapsLegendSettings Visible="true" Position="LegendPosition.BottomRight">
        </MapsLegendSettings>
    }
</SfMaps>

@code {
    private SfMaps mapInstance;
    private bool ShowLegend = true;
}
```

## Data Labels

Data labels display text directly on map elements (regions, bubbles, markers) to show values, names, or statistics.

### Labels on Shapes (Regions)

```csharp
<MapsLayer TValue="StateData" DataSource="@StateData">
    <MapsShapeSettings ColorValuePath="Population">
    </MapsShapeSettings>
    <MapsDataLabelSettings Visible="true" LabelPath="Name">
    </MapsDataLabelSettings>
</MapsLayer>
```

This displays the state name on each state polygon.

### Custom Label Formatting

```csharp
<MapsDataLabelSettings Visible="true" LabelPath="Name"
    TextStyle="@LabelStyle"
    Border="@LabelBorder">
</MapsDataLabelSettings>

@code {
    private TextStyle LabelStyle = new TextStyle
    {
        FontSize = "12px",
        FontWeight = "bold",
        Color = "white"
    };

    private Border LabelBorder = new Border
    {
        Color = "black",
        Width = "1px"
    };
}
```

### Labels on Bubbles

```csharp
<MapsBubbleSettings Visible="true" ValuePath="Population">
    <MapsBubbleDataLabels Visible="true" LabelPath="CityName">
    </MapsBubbleDataLabels>
</MapsBubbleSettings>
```

Displays city names inside or near data bubbles.

### Conditional Labels

Show labels only for selected features:

```csharp
@page "/conditional-labels"
@using Syncfusion.Blazor.Maps

<SfMaps @ref="mapInstance" OnShapeSelected="ShapeSelected">
    <MapsLayers>
        <MapsLayer TValue="StateData" DataSource="@StateData">
            <MapsDataLabelSettings Visible="@ShowAllLabels" LabelPath="Name">
            </MapsDataLabelSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

<button @onclick="() => ShowAllLabels = !ShowAllLabels">
    @(ShowAllLabels ? "Hide" : "Show") Labels
</button>

@code {
    private SfMaps mapInstance;
    private bool ShowAllLabels = false;

    private void ShapeSelected(ShapeSelectedEventArgs args)
    {
        // Handle selection
    }
}
```

## Populating Maps with Data

### Binding Data to Shapes

Match geographic boundaries with your data:

```csharp
<MapsLayer TValue="YourDataType"
    ShapeDataSource="@GeoJsonShapes"
    DataSource="@YourDataList"
    ShapePropertyPath="@new string[] { "properties.state_id" }">
    <MapsShapeSettings ColorValuePath="Value">
        <MapsShapeColorMappings>
            <!-- Color mappings -->
        </MapsShapeColorMappings>
    </MapsShapeSettings>
</MapsLayer>

@code {
    private List<YourDataType> YourDataList = new()
    {
        new YourDataType { StateId = "CA", Value = 100 },
        new YourDataType { StateId = "TX", Value = 85 },
        // Match property names to GeoJSON
    };
}
```

**Key:** `ShapePropertyPath` must match property names in your GeoJSON data.

### Real-Time Data Updates

```csharp
@page "/live-data-map"
@using Syncfusion.Blazor.Maps

<SfMaps @ref="mapInstance" >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer TValue="RegionData"
            DataSource="@LiveData">
            <MapsShapeSettings ColorValuePath="CurrentValue">
                <MapsShapeColorMappings>
                    <MapsShapeColorMapping StartRange="0" EndRange="50" Color='new string[] { "green" }'>
                    </MapsShapeColorMapping>
                    <MapsShapeColorMapping StartRange="50" EndRange="100" Color='new string[] { "red" }'>
                    </MapsShapeColorMapping>
                </MapsShapeColorMappings>
            </MapsShapeSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private List<RegionData> LiveData = new();
    private Timer updateTimer;

    protected override async Task OnInitializedAsync()
    {
        LiveData = await LoadInitialData();
        StartLiveUpdates();
    }

    private void StartLiveUpdates()
    {
        updateTimer = new Timer(async _ =>
        {
            // Update data from API
            LiveData = await FetchLatestData();
            await mapInstance.RefreshAsync();
        }, null, TimeSpan.Zero, TimeSpan.FromSeconds(5));
    }

    private async Task<List<RegionData>> LoadInitialData()
    {
        return new List<RegionData>();
    }

    private async Task<List<RegionData>> FetchLatestData()
    {
        return new List<RegionData>();
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
                       Color="#B3E5FC"
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
                       SmartLabelMode="SmartLabelMode.Hide"
                       IntersectAction="IntersectAction.Wrap">
    <MapsLayerDataLabelTextStyle FontSize="12"
                                  FontFamily="Arial"
                                  FontColor="black">
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
                    Position="LegendPosition.BottomRight"
                    Mode="LegendMode.Default"
                    Type="LegendType.Shapes"
                    Arrangement="LegendArrangement.Vertical"
                    Width="200"
                    Height="100">
    <MapsLegendTitle Text="Population">
        <MapsLegendTitleStyle FontSize="14" FontWeight="bold">
        </MapsLegendTitleStyle>
    </MapsLegendTitle>
    <MapsLegendTextStyle FontSize="12">
    </MapsLegendTextStyle>
</MapsLegendSettings>
```

| Property | Type | Description |
|----------|------|-------------|
| `Visible` | bool | Show/hide legend |
| `Position` | `LegendPosition` | Placement: Top, Bottom, Left, Right, etc. |
| `Mode` | `LegendMode` | Default (static) or Interactive (clickable) |
| `Type` | `LegendType` | Items shown: Layers, Markers, Bubbles, Shapes |
| `Arrangement` | `LegendArrangement` | Layout: Vertical or Horizontal |
| `Width` | double | Legend width |
| `Height` | double | Legend height |
| `ToggleLegendVisibility` | bool | Allow clicking items to toggle visibility |

### MapsBubbleSettings

Configure bubble visualization.

```csharp
<MapsBubbleSettings MinRadius="5"
                    MaxRadius="25"
                    ColorValuePath="revenue"
                    ValuePath="sales"
                    Opacity="0.8"
                    Fill="blue">
    <MapsBubbleColorMappings>
        <MapsBubbleColorMapping StartRange="0" EndRange="100000" Color='new string[] { "#B3E5FC" }'>
        </MapsBubbleColorMapping>
        <MapsBubbleColorMapping StartRange="100000" EndRange="500000" Color='new string[] { "#4FC3F7" }'>
        </MapsBubbleColorMapping>
    </MapsBubbleColorMappings>
</MapsBubbleSettings>
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
    public object From { get; set; }
    public object To { get; set; }
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
            @foreach (var city in CityMarkers)
            {
                <MapsMarker Latitude="@city.Latitude" Longitude="@city.Longitude">
                </MapsMarker>
            }
        </MapsMarkerSettings>
    </MapsLayer>
</MapsLayers>
```

### Aggregated Data Visualization

Combine multiple data points into regional summaries:

```csharp
@page "/aggregated-data-map"
@using Syncfusion.Blazor.Maps

<SfMaps >
<MapsZoomSettings ZoomFactor="3"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer TValue="RegionalSales"
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
    <MapsLegendSettings Visible="true" Position="LegendPosition.BottomRight">
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
<button @onclick="() => FilterData('high')">High Values</button>
<button @onclick="() => FilterData('all')">All Values</button>

<SfMaps @ref="mapInstance" >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer TValue="DataPoint"
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

        await mapInstance.RefreshAsync();
    }

    public class DataPoint
    {
        public string RegionId { get; set; }
        public double Value { get; set; }
    }
}
```

