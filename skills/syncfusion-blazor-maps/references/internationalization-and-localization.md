> **MANDATORY SECURITY NOTICE:** Do NOT load GeoJSON, ShapeData, tile, or image resources directly from untrusted third‑party URLs at runtime. Host assets locally or return server-validated, signed URLs; validate GeoJSON/ShapeData against a strict schema, sanitize/HTML-encode properties, enforce size/complexity limits, and require human review before automated processing or forwarding to agents.

## Table of Contents

- [Multi-Language Support](#multi-language-support)
   - [Configurable UI Text](#configurable-ui-text)
   - [Localized Map Labels](#localized-map-labels)
- [Right-to-Left (RTL) Support](#right-to-left-rtl-support)
   - [Enable RTL Mode](#enable-rtl-mode)
   - [RTL with Arabic Text](#rtl-with-arabic-text)
- [Regional Number and Date Formatting](#regional-number-and-date-formatting)
   - [Localized Number Formatting](#localized-number-formatting)
   - [Currency Formatting](#currency-formatting)
- [Regional Map Variations](#regional-map-variations)
   - [Geopolitical Boundaries](#geopolitical-boundaries)
- [Language-Specific Customization](#language-specific-customization)
   - [Dynamic String Resources](#dynamic-string-resources)
- [Accessibility with Localization](#accessibility-with-localization)

# Internationalization and Localization

> **W011 Security Note:** Localization examples still rely on external map data. If translated labels or region names originate outside the trusted boundary, validate and encode them before display.

## Multi-Language Support

### Configurable UI Text

```csharp
@page "/multi-language"
@using Syncfusion.Blazor.Maps

<div style="margin-bottom: 20px;">
    <label>
        Language:
        <select @bind="CurrentLanguage" @onChange="async _ => await ChangeLanguage()">
            <option value="en">English</option>
            <option value="es">Spanish</option>
            <option value="fr">French</option>
            <option value="de">German</option>
            <option value="ja">Japanese</option>
        </select>
    </label>
</div>

<SfMaps @ref="mapInstance">
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "/data/world-map.json"}' TValue="StateData" DataSource="@StateDataLabel">
            <MapsDataLabelSettings Visible="true" LabelPath="Name"></MapsDataLabelSettings>
        </MapsLayer>
    </MapsLayers>

    <MapsLegendSettings Visible="true"></MapsLegendSettings>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private string CurrentLanguage = "en";
    private List<StateData> StateDataLabel = new();

    protected override async Task OnInitializedAsync()
    {
        // Load initial state data here if needed
        await Task.CompletedTask;
    }

    private void ChangeLanguage()
    {
        // Apply language‑specific changes here
        mapInstance.Refresh();
    }

    public class StateData
    {
        public string Name { get; set; }
        public int Value { get; set; }
    }
}
```

### Localized Map Labels

Translate geographic labels and place names:

```csharp
@page "/localized-labels"
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "/data/usa-map.json"}' TValue="LocalizedRegion" DataSource="@LocalizedData">
            <MapsDataLabelSettings Visible="true" LabelPath="LocalizedName">
            </MapsDataLabelSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    // Stores data with localized names
    private List<LocalizedRegion> LocalizedData = new()
    {
        new LocalizedRegion 
        { 
            Name = "California",
            LocalizedName = "Californie",  // French
            Value = 100 
        },
        new LocalizedRegion 
        { 
            Name = "Texas",
            LocalizedName = "Tejas",  // Spanish
            Value = 85 
        }
    };

    public class LocalizedRegion
    {
        public string Name { get; set; }
        public string LocalizedName { get; set; }
        public int Value { get; set; }
    }
}
```
## Regional Number and Date Formatting

### Localized Number Formatting

```csharp
@page "/localized-numbers"
@using Syncfusion.Blazor.Maps
@using System.Globalization

<label>
    Select Region:
    <select @bind="SelectedCulture" @onChange="ChangeCulture">
        <option value="en-US">English (USA)</option>
        <option value="de-DE">German (Germany)</option>
        <option value="fr-FR">French (France)</option>
        <option value="ja-JP">Japanese</option>
        <option value="ar-SA">Arabic (Saudi Arabia)</option>
    </select>
</label>

<SfMaps >
    <MapsLayers>
        <MapsLayer ShapeDataPath="@ShapeDataPath" ShapePropertyPath="@ShapePropertyPath" ShapeData='new {dataOptions= "/data/usa-map.json"}' TValue="RegionData" DataSource="@FormattedData">
            <MapsDataLabelSettings Visible="true" 
                LabelPath="FormattedValue">
            </MapsDataLabelSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {    
    public string[] ShapePropertyPath = { "name" };
    public string ShapeDataPath = "Name";
    private string SelectedCulture = "en-US";
    private List<RegionData> FormattedData = new();

    protected override void OnInitialized()
    {
        LoadFormattedData();
    }

    private void ChangeCulture(Microsoft.AspNetCore.Components.ChangeEventArgs e)
    {
        SelectedCulture = e.Value.ToString();
    }

    private void LoadFormattedData()
    {
        var culture = new CultureInfo(SelectedCulture);
        
        FormattedData = new()
        {
            new RegionData 
            { 
                Name = "Texas", 
                Value = 1000000,
                FormattedValue = (1000000).ToString("N0", culture)
            }
            //....
        };
    }

    public class RegionData
    {
        public string Name { get; set; }
        public int Value { get; set; }
        public string FormattedValue { get; set; }
    }
}
```

### Currency Formatting

```csharp
private string FormatCurrency(decimal amount, string cultureName)
{
    var culture = new CultureInfo(cultureName);
    return amount.ToString("C", culture);
}

// Usage
<p>@FormatCurrency(1500, "en-US")</p>    <!-- $1,500.00 -->
<p>@FormatCurrency(1500, "de-DE")</p>    <!-- 1.500,00 € -->
<p>@FormatCurrency(1500, "fr-FR")</p>    <!-- 1 500,00 € -->
<p>@FormatCurrency(1500, "ja-JP")</p>    <!-- ¥1,500 -->
```

## Regional Map Variations

### Geopolitical Boundaries

Display different map boundaries based on region/country:

```csharp
@page "/regional-variants"
@using Syncfusion.Blazor.Maps
<div style="margin-bottom:10px;">
    <button @onclick="MoveToUSA">Move to USA</button>
    <button @onclick="MoveToIndia">Move to India</button>
    <button @onclick="MoveToEurope">Move to Europe</button>
</div>
<SfMaps @ref="mapInstance">
    <MapsCenterPosition 
        Latitude="@MapCenter.Latitude" 
        Longitude="@MapCenter.Longitude">
    </MapsCenterPosition>
    <MapsZoomSettings Enable="false" ZoomFactor="5"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer ShapeData='new { dataOptions = "/data/world-map.json" }'
                   TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
@code {
    private SfMaps mapInstance;
    private MapsCenterPosition MapCenter = new MapsCenterPosition
    {
        Latitude = 25.54244147012483,
        Longitude = -89.62646484375
    };
    private void MoveToUSA()
    {
        MapCenter.Latitude = 37.0902;
        MapCenter.Longitude = -95.7129;
        mapInstance.Refresh();
    }
    private void MoveToIndia()
    {
        MapCenter.Latitude = 20.5937;
        MapCenter.Longitude = 78.9629;
        mapInstance.Refresh();
    }
    private void MoveToEurope()
    {
        MapCenter.Latitude = 54.5260;
        MapCenter.Longitude = 15.2551;
        mapInstance.Refresh();
    }
}
```

## Language-Specific Customization

### Dynamic String Resources

```csharp
@page "/string-resources"
@using Syncfusion.Blazor.Maps

<label>
    Language:
    <select @bind="CurrentLanguage" @onChange="ChangeLanguage">
        <option value="en">English</option>
        <option value="es">Espaol</option>
        <option value="fr">Franais</option>
    </select>
</label>

<h2>@GetString("Title")</h2>
<p>@GetString("Description")</p>

<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions= "https://cdn.syncfusion.com/maps/map-data/world-map.json"}' TValue="string">
            <MapsDataLabelSettings Visible="true">
            </MapsDataLabelSettings>
        </MapsLayer>
    </MapsLayers>
    <MapsLegendSettings Visible="true">
    </MapsLegendSettings>
</SfMaps>

@code {
    private string CurrentLanguage = "en";

    private Dictionary<string, Dictionary<string, string>> StringResources = new()
    {
        { "en", new Dictionary<string, string>
        {
            { "Title", "World Population Map" },
            { "Description", "Population by country" },
            { "Legend", "Population Density" }
        }},
        { "es", new Dictionary<string, string>
        {
            { "Title", "Mapa de Poblacin Mundial" },
            { "Description", "Poblacin por pas" },
            { "Legend", "Densidad de Poblacin" }
        }},
        { "fr", new Dictionary<string, string>
        {
            { "Title", "Carte de la Population Mondiale" },
            { "Description", "Population par pays" },
            { "Legend", "Densit de Population" }
        }}
    };

    private string GetString(string key)
    {
        if (StringResources.TryGetValue(CurrentLanguage, out var lang))
        {
            if (lang.TryGetValue(key, out var value))
            {
                return value;
            }
        }
        return key; // Fallback to key if not found
    }

    private void ChangeLanguage(Microsoft.AspNetCore.Components.ChangeEventArgs e)
    {
        CurrentLanguage = e.Value.ToString();
        StateHasChanged();
    }
}
```

## Accessibility with Localization

Ensure accessibility features work across languages:

```csharp
@page "/accessible-localized-map"
@using Syncfusion.Blazor.Maps

<div role="region" aria-label='@GetString("AriaLabel")'>
    <h2>@GetString("MapTitle")</h2>
    
    <SfMaps>
        <MapsLayers>
            <MapsLayer ShapeData='new {dataOptions= "https://cdn.syncfusion.com/maps/map-data/world-map.json"}' TValue="string">
                <MapsDataLabelSettings Visible="true">
                </MapsDataLabelSettings>
            </MapsLayer>
        </MapsLayers>
        <MapsLegendSettings Visible="true">
        </MapsLegendSettings>
    </SfMaps>
</div>

@code {

    private Dictionary<string, string> AriaStrings = new()
    {
        { "AriaLabel", "Interactive world population map" },
        { "MapTitle", "Population Distribution" },
        { "MapDescription", "Color-coded map showing population by region" },
        { "LegendLabel", "Population density legend" }
    };

    private string GetString(string key)
    {
        return AriaStrings.ContainsKey(key) ? AriaStrings[key] : key;
    }
}
```