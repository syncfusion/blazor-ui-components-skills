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

## Multi-Language Support

### Configurable UI Text

```csharp
@page "/multi-language"
@using Syncfusion.Blazor.Maps

<div style="margin-bottom: 20px;">
    <label>
        Language:
        <select @bind="CurrentLanguage" @onchange="ChangeLanguage">
            <option value="en">English</option>
            <option value="es">Spanish</option>
            <option value="fr">French</option>
            <option value="de">German</option>
            <option value="ja">Japanese</option>
        </select>
    </label>
</div>

<SfMaps @ref="mapInstance" >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer TValue="StateData" DataSource="@StateData">
            <MapsDataLabelSettings Visible="true" LabelPath="Name">
            </MapsDataLabelSettings>
        </MapsLayer>
    </MapsLayers>
    <MapsLegendSettings Visible="true">
    </MapsLegendSettings>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition { Latitude = 39.0, Longitude = -98.0 };
    private string CurrentLanguage = "en";
    private List<StateData> StateData = new();

    private async Task ChangeLanguage(ChangeEventArgs e)
    {
        CurrentLanguage = e.Value.ToString();
        // Refresh UI with new language strings
        await mapInstance.RefreshAsync();
        StateHasChanged();
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

<SfMaps >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer TValue="LocalizedData" DataSource="@LocalizedData">
            <MapsDataLabelSettings Visible="true" LabelPath="LocalizedName">
            </MapsDataLabelSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition { Latitude = 39.0, Longitude = -98.0 };

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

## Right-to-Left (RTL) Support

### Enable RTL Mode

```csharp
@page "/rtl-map"
@using Syncfusion.Blazor.Maps

<div dir="rtl" style="font-family: Arial, sans-serif;">
    <h2>خريطة العالم</h2>
    
    <SfMaps EnableRtl="true" >
    <MapsZoomSettings ZoomFactor="3"></MapsZoomSettings>
        <MapsLayers>
            <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            </MapsLayer>
        </MapsLayers>
        <MapsLegendSettings Visible="true">
        </MapsLegendSettings>
    </SfMaps>
</div>

@code {
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition { Latitude = 20.0, Longitude = 55.0 };
}
```

**Key points for RTL:**
- Set `EnableRtl="true"` on SfMaps
- Wrap in `<div dir="rtl">` for HTML direction
- Use RTL-appropriate fonts
- Test legend and label positioning

### RTL with Arabic Text

```csharp
@page "/arabic-map"
@using Syncfusion.Blazor.Maps

<style>
    .rtl-map {
        direction: rtl;
        font-family: 'Arial Unicode MS', 'Segoe UI', sans-serif;
    }

    .rtl-map .e-maps {
        background: white;
    }
</style>

<div class="rtl-map">
    <h2>خريطة السكان حسب المنطقة</h2>

    <SfMaps EnableRtl="true" >
    <MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
        <MapsLayers>
            <MapsLayer TValue="ArabicRegionData" DataSource="@ArabicRegionData">
                <MapsShapeSettings ColorValuePath="Population">
                    <MapsShapeColorMappings>
                        <MapsShapeColorMapping StartRange="0" EndRange="1000000" Color='new string[] { "#B3E5FC" }'>
                        </MapsShapeColorMapping>
                        <MapsShapeColorMapping StartRange="1000000" EndRange="5000000" Color='new string[] { "#0288D1" }'>
                        </MapsShapeColorMapping>
                    </MapsShapeColorMappings>
                </MapsShapeSettings>
                <MapsDataLabelSettings Visible="true" LabelPath="ArabicName">
                </MapsDataLabelSettings>
            </MapsLayer>
        </MapsLayers>
        <MapsLegendSettings Visible="true">
        </MapsLegendSettings>
    </SfMaps>
</div>

@code {
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition { Latitude = 25.0, Longitude = 55.0 };

    private List<ArabicRegion> ArabicRegionData = new()
    {
        new ArabicRegion { Name = "Dubai", ArabicName = "دبي", Population = 3137000 },
        new ArabicRegion { Name = "Abu Dhabi", ArabicName = "أبو ظبي", Population = 1450000 },
        new ArabicRegion { Name = "Sharjah", ArabicName = "الشارقة", Population = 1400000 }
    };

    public class ArabicRegion
    {
        public string Name { get; set; }
        public string ArabicName { get; set; }
        public int Population { get; set; }
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
    <select @bind="SelectedCulture" @onchange="ChangeCulture">
        <option value="en-US">English (USA)</option>
        <option value="de-DE">German (Germany)</option>
        <option value="fr-FR">French (France)</option>
        <option value="ja-JP">Japanese</option>
        <option value="ar-SA">Arabic (Saudi Arabia)</option>
    </select>
</label>

<SfMaps >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer TValue="FormattedData" DataSource="@FormattedData">
            <MapsDataLabelSettings Visible="true" 
                LabelPath="FormattedValue">
            </MapsDataLabelSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition { Latitude = 39.0, Longitude = -98.0 };
    private string SelectedCulture = "en-US";
    private List<RegionData> FormattedData = new();

    protected override void OnInitialized()
    {
        LoadFormattedData();
    }

    private void ChangeCulture(ChangeEventArgs e)
    {
        SelectedCulture = e.Value.ToString();
        LoadFormattedData();
    }

    private void LoadFormattedData()
    {
        var culture = new CultureInfo(SelectedCulture);
        
        FormattedData = new()
        {
            new RegionData 
            { 
                Name = "Region A", 
                Value = 1000000,
                FormattedValue = (1000000).ToString("N0", culture)
            },
            new RegionData 
            { 
                Name = "Region B", 
                Value = 2500000,
                FormattedValue = (2500000).ToString("N0", culture)
            }
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

<div>
    <label>
        View:
        <select @bind="SelectedView" @onchange="ChangeMapView">
            <option value="global">Global</option>
            <option value="india">India</option>
            <option value="china">China</option>
            <option value="middleeast">Middle East</option>
        </select>
    </label>
</div>

<SfMaps @ref="mapInstance" >
<MapsZoomSettings ZoomFactor="@CurrentZoomLevel"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer TValue="string" 
            ShapeDataSource="@GetMapDataForRegion(SelectedView)">
            <MapsShapeSettings Fill="lightblue" Stroke="blue">
            </MapsShapeSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private string SelectedView = "global";
    private MapsCenterPosition CurrentCenter = new MapsCenterPosition { Latitude = 20.0, Longitude = 0.0 };
    private int CurrentZoomLevel = 2;

    private async Task ChangeMapView(ChangeEventArgs e)
    {
        SelectedView = e.Value.ToString();

        (CurrentCenter, CurrentZoomLevel) = SelectedView switch
        {
            "india" => (new MapsCenterPosition { Latitude = 20.5937, Longitude = 78.9629 }, 5),
            "china" => (new MapsCenterPosition { Latitude = 35.8617, Longitude = 104.1954 }, 4),
            "middleeast" => (new MapsCenterPosition { Latitude = 27.0, Longitude = 53.0 }, 4),
            _ => (new MapsCenterPosition { Latitude = 20.0, Longitude = 0.0 }, 2)
        };

        await mapInstance.RefreshAsync();
    }

    private object GetMapDataForRegion(string region)
    {
        // Return appropriate GeoJSON/shape data for region
        return new { };
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
    <select @bind="CurrentLanguage" @onchange="ChangeLanguage">
        <option value="en">English</option>
        <option value="es">Español</option>
        <option value="fr">Français</option>
    </select>
</label>

<h2>@GetString("Title")</h2>
<p>@GetString("Description")</p>

<SfMaps >
<MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
    <MapsLayers>
        <MapsLayer TValue="string">
            <MapsDataLabelSettings Visible="true">
            </MapsDataLabelSettings>
        </MapsLayer>
    </MapsLayers>
    <MapsLegendSettings Visible="true">
    </MapsLegendSettings>
</SfMaps>

@code {
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition { Latitude = 39.0, Longitude = -98.0 };
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
            { "Title", "Mapa de Población Mundial" },
            { "Description", "Población por país" },
            { "Legend", "Densidad de Población" }
        }},
        { "fr", new Dictionary<string, string>
        {
            { "Title", "Carte de la Population Mondiale" },
            { "Description", "Population par pays" },
            { "Legend", "Densité de Population" }
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

    private void ChangeLanguage(ChangeEventArgs e)
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

<div role="region" aria-label="@GetString('AriaLabel')">
    <h2>@GetString("MapTitle")</h2>
    
    <SfMaps>
        <MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
        <MapsLayers>
            <MapsLayer TValue="string">
                <MapsDataLabelSettings Visible="true">
                </MapsDataLabelSettings>
            </MapsLayer>
        </MapsLayers>
        <MapsLegendSettings Visible="true"
            AriaLabel="@GetString('LegendLabel')">
        </MapsLegendSettings>
    </SfMaps>
</div>

@code {
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition { Latitude = 39.0, Longitude = -98.0 };

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


