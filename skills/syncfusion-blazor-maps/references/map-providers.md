# Map Providers and Configuration

## Table of Contents

- [Overview](#overview)
- [OpenStreetMap](#openstreetmap)
   - [Basic Setup (No API Key Required)](#basic-setup-no-api-key-required)
   - [Alternative OpenStreetMap Tiles](#alternative-openstreetmap-tiles)
   - [Attribution Requirements](#attribution-requirements)
   - [Usage Restrictions](#usage-restrictions)
- [Google Maps](#google-maps)
   - [Prerequisites](#prerequisites)
   - [Implementation](#implementation)
   - [Google Map Types](#google-map-types)
   - [Example with Satellite View](#example-with-satellite-view)
   - [Pricing](#pricing)
   - [Restrictions](#restrictions)
- [Bing Maps](#bing-maps)
   - [Prerequisites](#prerequisites)
   - [Implementation](#implementation)
   - [Bing Map Types](#bing-map-types)
   - [Example with Aerial View](#example-with-aerial-view)
   - [Licensing](#licensing)
- [Azure Maps](#azure-maps)
   - [Prerequisites](#prerequisites)
   - [Implementation](#implementation)
   - [Azure Map Styles](#azure-map-styles)
   - [Example with Dark Grayscale](#example-with-dark-grayscale)
   - [Pricing](#pricing)
- [Provider Comparison](#provider-comparison)
- [Switching Providers](#switching-providers)
   - [Switch at Runtime](#switch-at-runtime)
   - [Managing API Keys Securely](#managing-api-keys-securely)
- [Troubleshooting Provider Issues](#troubleshooting-provider-issues)
   - [Tiles not loading or blank map](#tiles-not-loading-or-blank-map)
   - ["Access denied" or authentication errors](#access-denied-or-authentication-errors)
   - [Map loads but appears distorted or misaligned](#map-loads-but-appears-distorted-or-misaligned)


## Overview

Map providers supply the base tile layer (streets, satellite, terrain) that forms the foundation of your visualization. Syncfusion Maps supports multiple providers, each with different coverage, styling, and configuration requirements.

**Common UrlTemplate format:**
```
https://tile-provider.com/{level}/{tileX}/{tileY}.{extension}
```

Where:
- `{level}` = zoom level (0-20)
- `{tileX}` = horizontal tile coordinate
- `{tileY}` = vertical tile coordinate

## OpenStreetMap

**Best for:** Free, open-source projects with global coverage

### Basic Setup (No API Key Required)

```csharp
<SfMaps>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

### Alternative OpenStreetMap Tiles

Different tile providers with OpenStreetMap data:

**Stamen Terrain (landscape focus):**
```csharp
<MapsLayer UrlTemplate="https://stamen-tiles-{s}.a.ssl.fastly.net/terrain/level/tileX/tileY.png" TValue="string">
</MapsLayer>
```

**CartoDB Positron (clean, minimal):**
```csharp
<MapsLayer UrlTemplate="https://cartodb-basemaps-{s}.global.ssl.fastly.net/light_all/level/tileX/tileY.png" TValue="string">
</MapsLayer>
```

**USGS Topo (US topographic map):**
```csharp
<MapsLayer UrlTemplate="https://basemap.nationalmap.gov/arcrest/services/USGSTopo/MapServer/tile/level/tileY/tileX" TValue="string">
</MapsLayer>
```

### Attribution Requirements

Always display attribution for OpenStreetMap:

```csharp
<SfMaps>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
    <MapsLayouts>
        <MapsLayout Type="Type.BottomRight">
            <MapsLayoutAnnotations>
                <MapsLayoutAnnotation Content="© OpenStreetMap contributors">
                </MapsLayoutAnnotation>
            </MapsLayoutAnnotations>
        </MapsLayout>
    </MapsLayouts>
</SfMaps>
```

### Usage Restrictions

- No API key required
- Tile usage limits apply (check current policy)
- Attribution required in all views
- Suitable for production applications

## Google Maps

**Best for:** Premium quality, comprehensive coverage, satellite imagery

### Prerequisites

1. **Create Google Cloud Project:**
   - Go to [Google Cloud Console](https://console.cloud.google.com)
   - Create a new project
   - Enable "Maps Static API" and "Maps JavaScript API"
   - Create an API key in Credentials

2. **Restrict API Key:**
   - Restrict to HTTP referrers matching your domain
   - Restrict to Maps Static API and JavaScript API

### Implementation

```csharp
<SfMaps>
    <MapsLayers>
        <MapsLayer Type="LayerType.GoogleStaticMap" Key="YOUR_API_KEY" 
                   GoogleStaticMapType="GoogleMapType.Roadmap">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

### Google Map Types

| Type | Description | Use Case |
|------|-------------|----------|
| `Roadmap` | Standard street map | Default choice |
| `Satellite` | Satellite imagery | Aerial views |
| `Terrain` | Terrain with elevation | Topography |
| `Hybrid` | Satellite + street labels | Detailed imagery |

### Example with Satellite View

```csharp
<SfMaps>
    <MapsLayers>
        <MapsLayer Type="LayerType.GoogleStaticMap" 
                   Key="AIzaSyDOkaKwIy4-B-5lW3j7H7xUV8k1JnPXpCU"
                   GoogleStaticMapType="GoogleMapType.Satellite">
        </MapsLayer>
    </MapsLayers>
    <MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
</SfMaps>

```

### Pricing

- First 28,000 map loads/month: Free
- Additional loads: $0.007 per load
- Requires valid payment method

### Restrictions

- API key must be kept private
- Restrict to HTTP referrers for security
- Monitor usage in Cloud Console

## Bing Maps

**Best for:** Enterprise applications, strong coverage in certain regions

### Prerequisites

1. **Get Bing Maps Key:**
   - Register at [Bing Maps Dev Center](https://www.bingmapsportal.com/)
   - Create a new key
   - Choose "Basic" or "Enterprise" license

2. **Configure Key:**
   - Copy the key from the portal
   - Restrict to your application domain

### Implementation

```csharp
<SfMaps>
    <MapsLayers>
        <MapsLayer Type="LayerType.Bing" Key="YOUR_BING_MAPS_KEY" 
                   BingMapType="BingMapType.Road">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

### Bing Map Types

| Type | Description |
|------|-------------|
| `Road` | Street map (default) |
| `Aerial` | Satellite imagery |
| `AerialWithLabel` | Satellite with labels |
| `CanvasDark` | Dark themed street map |
| `CanvasLight` | Light themed street map |

### Example with Aerial View

```csharp
<SfMaps >
    <MapsLayers>
        <MapsLayer Type="LayerType.Bing" 
                   Key="YOUR_BING_MAPS_KEY"
                   BingMapType="BingMapType.Aerial">
        </MapsLayer>
    </MapsLayers>
    <MapsZoomSettings ZoomFactor="12"></MapsZoomSettings>
</SfMaps>

```

### Licensing

- **Basic:** Free for non-enterprise apps
- **Enterprise:** Contact Bing Maps for pricing
- Register at Microsoft account

## Azure Maps

**Best for:** Enterprise integration with Microsoft Azure ecosystem

### Prerequisites

1. **Create Azure Account:**
   - Go to [Azure Portal](https://portal.azure.com)
   - Create Azure Maps resource
   - Get primary key from Authentication

2. **Enable Services:**
   - Ensure "Render Service" is enabled
   - Configure authentication options

### Implementation

```csharp
<SfMaps>
    <MapsLayers>
        <MapsLayer Type="LayerType.AzureMap" Key="YOUR_AZURE_MAPS_KEY">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

### Azure Map Styles

| Style | Description |
|-------|-------------|
| `Road` | Standard street map |
| `Satellite` | Satellite imagery |
| `GrayscaleDark` | Dark grayscale |
| `GrayscaleLight` | Light grayscale |
| `Night` | Nighttime styling |

### Example with Dark Grayscale

```csharp
<SfMaps >
    <MapsLayers>
        <MapsLayer Type="LayerType.AzureMap" 
                   Key="YOUR_AZURE_MAPS_KEY"
                   AzureMapStyleName="GrayscaleDark">
        </MapsLayer>
    </MapsLayers>
    <MapsZoomSettings ZoomFactor="8"></MapsZoomSettings>
</SfMaps>

```

### Pricing

Pay-as-you-go model:
- First 2,000 transactions: Free per month
- Additional transactions: $0.50 per 1,000 requests
- Integration with Azure services available

## Provider Comparison

| Feature | OpenStreetMap | Google Maps | Bing Maps | Azure Maps |
|---------|---------------|------------|-----------|------------|
| **Cost** | Free | Free tier + pay | Free/Enterprise | Free tier + pay |
| **API Key Required** | No | Yes | Yes | Yes |
| **Coverage** | Global | Excellent | Good | Good |
| **Satellite** | Limited | Excellent | Good | Good |
| **Speed** | Fast | Fast | Fast | Fast |
| **Commercial Use** | Yes (attribution) | Yes (licensed) | Yes (licensed) | Yes (licensed) |
| **Setup Difficulty** | Easy | Medium | Medium | Medium |
| **Best For** | Open source | Premium quality | Enterprise | Azure integration |

## Switching Providers

### Switch at Runtime

Store provider config and update dynamically:

```csharp
@page "/maps-provider"
@using Syncfusion.Blazor.Maps

<div>
    <button @onclick="() => ChangeProvider('osm')">OpenStreetMap</button>
    <button @onclick="() => ChangeProvider('google')">Google</button>
    <button @onclick="() => ChangeProvider('bing')">Bing</button>
</div>

<div style="width: 100%; height: 500px;">
    <SfMaps >
    <MapsZoomSettings ZoomFactor="4"></MapsZoomSettings>
        <MapsLayers>
            @if (CurrentProvider == "osm")
            {
                <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
                </MapsLayer>
            }
            else if (CurrentProvider == "google")
            {
                <MapsLayer Type="LayerType.GoogleStaticMap" 
                           Key="YOUR_GOOGLE_KEY"
                           GoogleStaticMapType="GoogleMapType.Roadmap">
                </MapsLayer>
            }
            else if (CurrentProvider == "bing")
            {
                <MapsLayer Type="LayerType.Bing" 
                           Key="YOUR_BING_KEY"
                           BingMapType="BingMapType.Road">
                </MapsLayer>
            }
        </MapsLayers>
    </SfMaps>
</div>

@code {
    private SfMaps mapInstance;
    private string CurrentProvider = "osm";
    private MapsCenterPosition CenterLatLng = new MapsCenterPosition 
    { 
        Latitude = 39.0, 
        Longitude = -98.0 
    };

    private async Task ChangeProvider(string provider)
    {
        CurrentProvider = provider;
        await mapInstance.RefreshAsync();
    }
}
```

### Managing API Keys Securely

Store API keys in configuration, not code:

**appsettings.json:**
```json
{
  "MapProviders": {
    "GoogleKey": "AIzaSyD...",
    "BingKey": "Ao8...",
    "AzureKey": "..."
  }
}
```

**Razor component:**
```csharp
@inject IConfiguration Configuration

@code {
    private string GoogleKey;
    private string BingKey;

    protected override void OnInitialized()
    {
        GoogleKey = Configuration["MapProviders:GoogleKey"];
        BingKey = Configuration["MapProviders:BingKey"];
    }
}
```

## Troubleshooting Provider Issues

### Tiles not loading or blank map
- Verify API key is correct and active
- Check domain restrictions in provider settings
- Ensure provider service is enabled (Google/Azure)
- Check browser DevTools console for CORS errors

### "Access denied" or authentication errors
- Verify key is for correct provider
- Check HTTP referrer restrictions match your domain
- Ensure key has correct API permissions enabled
- Try key in different browser/incognito to rule out caching

### Map loads but appears distorted or misaligned
- Verify UrlTemplate format matches provider specification
- Check tile coordinates are using correct format
- Some providers use {s} for subdomain; ensure it's included if needed

