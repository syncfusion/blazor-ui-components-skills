# Map Providers and Configuration

> **MANDATORY SECURITY NOTICE:** Provider examples may show tile and metadata templates that reference third-party services for illustration only. In production you MUST either host tiles/GeoJSON locally or serve them through a server-side proxy that:
>
> - validates provider hosts against an allow‑list and requires HTTPS,
> - verifies content-type and schema (strict GeoJSON validation),
> - enforces maximum file size and feature-count limits,
> - strips or HTML-encodes properties (tooltips/annotations) to remove markup and scripts,
> - issues short-lived signed URLs/tokens for client consumption (do NOT embed API keys in client code), and
> - logs ingestion events and requires documented human review before automated processing.
>
> NEVER forward raw ShapeData, tooltips, or annotations to automated agents or LLM prompts without schema validation, sanitization, and human sign-off. See [readme-security](readme-security.md) for required validation templates and deployment checklist.

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
- [Security Best Practices](#security-best-practices)
   - [Protecting API Keys](#protecting-api-keys)
   - [API Key Restrictions](#api-key-restrictions)
   - [Preventing Data Exfiltration](#preventing-data-exfiltration)
   - [Content Security Policy](#content-security-policy)
- [Troubleshooting Provider Issues](#troubleshooting-provider-issues)
   - [Tiles not loading or blank map](#tiles-not-loading-or-blank-map)
   - ["Access denied" or authentication errors](#access-denied-or-authentication-errors)
   - [Map loads but appears distorted or misaligned](#map-loads-but-appears-distorted-or-misaligned)


## Overview

Map providers supply the base tile layer (streets, satellite, terrain) that forms the foundation of your visualization. Syncfusion Maps supports multiple providers, each with different coverage, styling, and configuration requirements.

### External Asset Downloads (Expected Behavior)

This skill references external map tiles and data from **known, trusted vendor services**:
- **tile.openstreetmap.org** - OpenStreetMap tile provider (free, community-maintained)
- **cdn.syncfusion.com** - Syncfusion CDN for component resources
- **maps.googleapis.com** - Google Maps tile and metadata provider (requires API key)
- **dev.virtualearth.net** - Bing Maps provider (requires API key)
- **atlas.microsoft.com** - Azure Maps provider (requires API key)

These downloads are **expected and necessary behaviors** for map visualization. In production, you should either:
1. **Host tiles locally** (recommended for security-sensitive applications)
2. **Cache tiles on your server** with periodic refresh
3. **Use a validated proxy** that fetches tiles with domain/HTTPS verification

**Common UrlTemplate format:**
```
/tiles/{level}/{tileX}/{tileY}.{extension}
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
        <!-- Use a validated TileUrl variable or local tiles in production -->
        <MapsLayer UrlTemplate="@TileUrl" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

### Alternative OpenStreetMap Tiles

Different tile providers with OpenStreetMap data:

**Stamen Terrain (landscape focus):**
```csharp
// Example provider template; validate provider terms and domain before use
// <MapsLayer UrlTemplate="/tiles/terrain/{level}/{tileX}/{tileY}.png" TValue="string">
// </MapsLayer>
```

**CartoDB Positron (clean, minimal):**
```csharp
// Example provider template; validate provider terms and domain before use
// <MapsLayer UrlTemplate="/tiles/light_all/{level}/{tileX}/{tileY}.png" TValue="string">
// </MapsLayer>
```

**USGS Topo (US topographic map):**
```csharp
// Example provider template; validate provider terms and domain before use
// <MapsLayer UrlTemplate="/tiles/usgs/topo/{level}/{tileY}/{tileX}" TValue="string">
// </MapsLayer>
```

### Attribution Requirements

Always display attribution for OpenStreetMap:

```csharp
<SfMaps>
    <MapsLayers>
        <!-- Use a validated TileUrl variable or local tiles in production -->
        <MapsLayer UrlTemplate="@TileUrl" TValue="string">
        </MapsLayer>
    </MapsLayers>
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

**⚠️ SECURITY WARNING:** Never hardcode API keys in source code. Always load from secure configuration.

```csharp
@inject IConfiguration Configuration

<SfMaps>
    <MapsLayers>
        <!-- Load API key from secure configuration, NOT hardcoded -->
        <MapsLayer Type="LayerType.GoogleStaticMap" Key="@GoogleApiKey" 
                   GoogleStaticMapType="GoogleMapType.Roadmap">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private string GoogleApiKey;

    protected override void OnInitialized()
    {
        // Load from appsettings.json or environment variables (never hardcode)
        GoogleApiKey = Configuration["MapProviders:GoogleKey"];
        if (string.IsNullOrEmpty(GoogleApiKey))
        {
            throw new InvalidOperationException("Google Maps API key not configured. See 'Managing API Keys Securely' section.");
        }
    }
}
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
        <MapsLayer UrlTemplate="/tiles/google/{level}/{tileX}/{tileY}.png">
        </MapsLayer>
    </MapsLayers>
    <MapsZoomSettings Enable="true"></MapsZoomSettings>
</SfMaps>

```

> **Security Warning:** Never hardcode API keys in source code. Use configuration files (appsettings.json) or environment variables. See [Managing API Keys Securely](#managing-api-keys-securely) section below.

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
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <MapsLayer UrlTemplate="@UrlTemplate" TValue="string"></MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    public string UrlTemplate;

    protected override async Task OnInitializedAsync()
    {
        UrlTemplate = await SfMaps.GetBingUrlTemplate("SERVER_VALIDATED_BING_METADATA_URL");
    }
}
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
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <MapsLayer UrlTemplate="@UrlTemplate" TValue="string"></MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    public string UrlTemplate;

    protected override async Task OnInitializedAsync()
    {
        UrlTemplate = await SfMaps.GetBingUrlTemplate("SERVER_VALIDATED_BING_METADATA_URL");
    }
}

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
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <MapsLayer UrlTemplate="SERVER_VALIDATED_AZURE_TILE_URL" TValue="string">
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
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <MapsLayer UrlTemplate="https://atlas.microsoft.com/map/imagery/png?subscription-key=Your-Key &api-version=1.0&style=GrayscaleDark&zoom=level&x=tileX&y=tileY" TValue="string">
        </MapsLayer>
    </MapsLayers>
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
    <MapsZoomSettings Enable="true"></MapsZoomSettings>
        <MapsLayers>
            @if (CurrentProvider == "osm")
            {
                <MapsLayer UrlTemplate="@UrlTemplate" TValue="string">
                </MapsLayer>
            }
            else if (CurrentProvider == "google")
            {
                <MapsLayer UrlTemplate="@UrlTemplate">
                </MapsLayer>
            }
            else if (CurrentProvider == "bing")
            {
                <MapsLayer UrlTemplate="@UrlTemplate">
                </MapsLayer>
            }
        </MapsLayers>
    </SfMaps>
</div>

@code {
    private SfMaps mapInstance;
    // Default to a local/cached tile path; production: validate or restrict provider domains
    private string TileUrl = "/tiles/{level}/{tileX}/{tileY}.png";
    private string UrlTemplate = TileUrl;
    private string CurrentProvider = "osm";
    private async Task ChangeProvider(string provider)
    {
        if (provider == "osm") {
            UrlTemplate = TileUrl; // prefer local/cached tiles or validated provider
        } else if(provider == "google") {
            // Set via configuration and validate API usage; do NOT hardcode keys
            UrlTemplate="/tiles/google/{level}/{tileX}/{tileY}.png";
        } else{
            UrlTemplate = await SfMaps.GetBingUrlTemplate("https://dev.virtualearth.net/REST/V1/Imagery/Metadata/Aerial?output=json&uriScheme=https&key=");
        }
    }
}
```
## Security Best Practices

### Protecting API Keys

**Never hardcode API keys in source code.** Store them securely using one of these methods:

#### 1. Configuration Files (Development)
```json
// appsettings.json
{
  "MapProviders": {
    "GoogleKey": "YOUR_GOOGLE_API_KEY",
    "BingKey": "YOUR_BING_API_KEY"
  }
}
```

#### 2. User Secrets (Development)
```powershell
# Initialize user secrets
dotnet user-secrets init

# Set API keys
dotnet user-secrets set "MapProviders:GoogleKey" "YOUR_GOOGLE_API_KEY"
dotnet user-secrets set "MapProviders:BingKey" "YOUR_BING_API_KEY"
```

#### 3. Environment Variables (Production)
```csharp
@inject IConfiguration Configuration

@code {
    private string GoogleKey;

    protected override void OnInitialized()
    {
        // Reads from environment variable or appsettings.json
        GoogleKey = Configuration["MapProviders:GoogleKey"];
    }
}
```

#### 4. Azure Key Vault / AWS Secrets Manager (Production)
```csharp
// Program.cs
builder.Configuration.AddAzureKeyVault(
    new Uri($"https://{keyVaultName}.vault.azure.net/"),
    new DefaultAzureCredential());
```

### API Key Restrictions

Configure provider-side restrictions:

**Google Maps:**
- Restrict to HTTP referrers: `https://yourdomain.com/*`
- Restrict to specific APIs: Maps JavaScript API, Static Maps API
- Set usage quotas to prevent abuse

**Bing Maps:**
- Restrict to application URL
- Enable usage tracking
- Set rate limits

**Azure Maps:**
- Use Managed Identity when possible
- Configure CORS origins
- Enable Azure AD authentication

### Preventing Data Exfiltration

When handling external data sources:

```csharp
@code {
    private async Task<object> LoadExternalGeoJson(string url)
    {
        // Validate URL is from trusted domain
        // Replace with your validated allow-list entries (do not include untrusted domains)
        var trustedDomains = new[] { "{trusted_geojson_domain}", "yourdomain.com" };
        var uri = new Uri(url);
        
        if (!trustedDomains.Any(d => uri.Host.EndsWith(d)))
        {
            throw new SecurityException("Untrusted data source");
        }
        
        // Load data securely
        return await HttpClient.GetFromJsonAsync<object>(url);
    }
}
```

### Content Security Policy

Add CSP headers to prevent malicious content:

```csharp
// Program.cs or Startup.cs
app.Use(async (context, next) =>
{
    context.Response.Headers.Add("Content-Security-Policy", 
        "default-src 'self'; " +
        "img-src 'self' https://{trusted_tile_provider} https://{trusted_satellite_provider}; " +
        "script-src 'self' 'unsafe-inline' 'unsafe-eval'; " +
        "style-src 'self' 'unsafe-inline';");
    await next();
});
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

