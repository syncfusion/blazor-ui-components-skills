---
name: syncfusion-blazor-maps
description: Implement interactive spatial data visualization with Syncfusion Maps component for Blazor. Use this skill when user needs to display geographic data, add markers/polygons to maps, integrate map providers (Google Maps, Bing Maps, Azure Maps, OpenStreetMap), create choropleth visualizations, handle user interactions with maps, export/print maps, support internationalization, implement accessibility features, or customize map styling and appearance.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Data Visualization"
---

# Implementing Syncfusion Maps for Blazor

A comprehensive guide to implementing Syncfusion Maps component in Blazor applications. Syncfusion Maps provides powerful spatial visualization capabilities including marker management, polygon overlays, layer support, event handling, and integration with multiple map providers.

> **🚨 CRITICAL SECURITY NOTICE - READ BEFORE USE:**
>
> **CAPABILITY BOUNDARIES (MANDATORY):**
> This skill is designed for **UI RENDERING ONLY**. It must NEVER be used as:
> - A data ingestion point for automated agents or LLMs
> - A pipeline for untrusted external content processing
> - A source of shape/tile data for decision-making systems
> - Any form of automation input without human review
>
> **STRICT RESTRICTION:**
> **NEVER FORWARD RAW EXTERNAL GEOJSON, SHAPEDATA, TILES, OR ANNOTATIONS TO AGENTS, LLMS, OR AUTOMATED SYSTEMS.** All external map content must be treated as untrusted user input. Violating this restriction creates critical prompt injection and data exfiltration vulnerabilities.
>
> **REQUIRED PRODUCTION SAFEGUARDS:**
>
> 1. **For GeoJSON/Tile Loading:**
>    - Host tiles and GeoJSON locally (under `wwwroot/tiles` or bundled assets), OR
>    - Use only server-side proxies that validate all data before client access
>    - NEVER load directly from third-party URLs at runtime
>
> 2. **For Server-Side Proxy (if used):**
>    - Validate provider host against strict allow-list and require HTTPS
>    - Verify content-type and validate against strict schema (no user fields)
>    - Enforce maximum file size (5MB) and feature-count limits (10,000 max)
>    - Strip or HTML-encode ALL properties (tooltips, annotations, labels)
>    - Reject content containing instruction patterns or suspicious keywords
>    - Normalize/validate coordinates (lat -90/90, lng -180/180)
>    - Sanitize all text fields with HtmlSanitizer before client delivery
>    - Issue short-lived signed URLs (do NOT embed API keys)
>
> 3. **For Content Rendering:**
>    - Always sanitize external properties with HtmlSanitizer before rendering
>    - Implement Content Security Policy headers to block script injection
>    - Enable comprehensive logging of all validation failures
>
> **IF external content must be processed by automation:**
> - Perform COMPLETE server-side schema validation and sanitization FIRST
> - Implement mandatory rejection policy for instruction-like patterns
> - Enforce strict token/length limits (max 256 chars per field)
> - Require explicit human review and sign-off before automation
> - Wrap in boundary markers: `[MAP_DATA_START]...[MAP_DATA_END]`
> - Log all processing with full audit trail
> - See [readme-security](references/readme-security.md) for required validation templates
>
> **DO NOT USE THIS SKILL FOR:**
> - ❌ Analyzing untrusted external geographic data
> - ❌ Processing user-uploaded GeoJSON without validation
> - ❌ Forwarding map content to LLM/agent analysis systems
> - ❌ Making automated decisions based on external map metadata
> - ❌ Storing or caching external third-party content
>
> **SEVERE SECURITY CONSEQUENCES:**
> Ignoring these boundaries may result in:
> - Prompt injection attacks via malicious geographic data
> - LLM/agent behavioral manipulation
> - Unauthorized data exfiltration
> - System compromise through script injection

## When to Use This Skill

Use Syncfusion Maps when you need to:
- Display geographic data on interactive maps
- Visualize data across regions using color mapping (choropleth)
- Add markers, polygons, and annotations to maps
- Handle user interactions (click, hover, zoom, pan)
- Support multiple map providers (Google, Bing, Azure, OpenStreetMap)
- Export or print maps with custom legends and labels
- Build location-aware applications
- Create multi-layer spatial visualizations

## Component Overview

Syncfusion Maps is a powerful geospatial visualization component that enables you to:
- Render interactive maps with multiple provider options
- Bind and visualize geographic datasets
- Support rich spatial features (markers, polygons, layers, annotations)
- Handle complex user interactions and events
- Customize appearance through themes and styling
- Enable localization and accessibility features
- Export maps for reporting and sharing

---

## ⚠️ CRITICAL: Approved and Restricted Use Cases

### ✅ APPROVED USES (UI Rendering with Validation)
- Display geographic data on interactive web dashboards
- Visualize regional statistics through choropleth maps
- Show business location markers with human review
- Render localized maps for user interaction
- Provide spatial search and filtering interfaces
- Create read-only geographic visualizations
- Support decision-making with validated, curated data

### ❌ STRICTLY PROHIBITED USES (No Agent/LLM Integration)
- **NEVER:** Pass raw map data to LLM analysis systems
- **NEVER:** Use as ingestion point for automated agents
- **NEVER:** Forward external GeoJSON to decision-making systems
- **NEVER:** Process untrusted third-party geographic data through automation
- **NEVER:** Use annotations/tooltips as LLM input without human review
- **NEVER:** Create automated geographic data pipelines
- **NEVER:** Integrate with AI systems that analyze map metadata

### 🚫 THIRD-PARTY CONTENT INGESTION BOUNDARY

**External Sources Referenced in This Skill:**
- `tile.openstreetmap.org` - OpenStreetMap provider
- `cdn.syncfusion.com` - Syncfusion component resources
- `maps.googleapis.com` - Google Maps provider
- `dev.virtualearth.net` - Bing Maps provider
- `atlas.microsoft.com` - Azure Maps provider

**IMPORTANT:** These third-party providers are ingested for **UI rendering only**. None of this content should ever reach automated systems. If you need to process geographic data through automation:

1. ❌ DO NOT use this skill's external bindings
2. ✅ DO create a separate server-side validation pipeline
3. ✅ DO sanitize and validate all external content first
4. ✅ DO require explicit human approval before automation
5. ✅ DO implement comprehensive audit logging

---

## Security Considerations

> **CRITICAL:** This skill involves several security-sensitive operations. Review and implement these safeguards:

### 1. API Key Management
- **Risk:** Hardcoded API keys in source code can be exposed in version control
- **Mitigation:** Store keys in configuration files, user secrets, or secret management services
- **Reference:** [Map Providers - Security Best Practices](references/map-providers.md)

### 2. HTML Content Injection
- **Risk:** Tooltips, annotations, and popups can render arbitrary HTML from external data sources
- **Attack Vectors:** XSS attacks, prompt injection through malicious geographic data
- **Mitigation:** Sanitize all external content using HtmlSanitizer library, HTML-encode user input
- **Reference:** [User Interactions - Sanitizing External Content](references/user-interactions.md)

### 3. External Data Sources
- **Risk:** Map data (GeoJSON, tiles, markers) fetched from untrusted sources, including CDNs
- **Attack Vectors:** Man-in-the-middle attacks, CDN compromise, malformed data injection, property injection
- **Mitigation:** Validate source URLs against allow-lists, enforce HTTPS, validate GeoJSON structure, implement CSP, cache locally in production
- **Reference:** [Data Visualization - Validating External GeoJSON](references/data-visualization.md) | [Map Providers - Data Exfiltration Prevention](references/map-providers.md)

### 4. Data Export Operations
- **Risk:** Exporting maps to PNG/SVG/PDF may include sensitive geographic data
- **Mitigation:** Implement access controls, audit export operations, sanitize export content
- **Reference:** [Print and Export](references/print-and-export.md)

### 5. Prompt Injection Prevention & Agent/LLM Boundary
- **CRITICAL RISK:** Shape data, annotations, tooltips, and other external map metadata can include text resembling instructions. This skill ingests third-party content (tile providers, GeoJSON sources) that can be weaponized for prompt injection if forwarded to automated agents or LLMs without explicit human review.

- **Attack Surface:** External content enters via:
  - MapsLayer `UrlTemplate` (tile providers)
  - `ShapeData` and `DataSource` properties (GeoJSON)
  - Tooltip and annotation properties
  - Any data binding from external sources

- **MANDATORY BOUNDARY:** This skill must NEVER be the source of data for:
  - LLM analysis or summarization
  - Automated agent decision-making
  - AI-powered geographic analysis
  - Machine learning training pipelines
  - Any system that processes map data through AI/ML without human intervention

- **If external content must reach automation:**
    - Apply server-side validation BEFORE any client rendering: schema checks (strict GeoJSON validation), domain allow‑list verification
    - Sanitize ALL properties with HtmlSanitizer and remove HTML/script tags
    - Implement mandatory rejection policy for instruction-like keywords ("system:", "ignore", "bypass", "execute", "[SYSTEM]", etc.)
    - Enforce strict length limits (max 256 chars per property, max 5000 chars total)
    - Wrap data in boundary markers: `[MAP_DATA_START]...[MAP_DATA_END]`
    - Require explicit human review and sign-off before forwarding to any AI system
    - Maintain comprehensive audit log of all AI system accesses

- **Mitigation Architecture:**
    - **Layer 1:** UI-rendering-only designation (no agent/LLM use)
    - **Layer 2:** Pattern detection (suspicious keyword detection)
    - **Layer 3:** Immediate rejection (no alternative parsing)
    - **Layer 4:** HTML sanitization (whitelist approach)
    - **Layer 5:** Strict validation pipeline (schema, size, coordinates)
    - **Layer 6:** Boundary markers & audit logging (for unavoidable automation)

- **Reference:** [User Interactions - Preventing Prompt Injection](references/user-interactions.md) | [readme-security.md](references/readme-security.md)

---

## Security Issue Resolutions

This skill has been hardened against the following identified security warnings:

### [CREDENTIALS_UNSAFE] Hardcoded API Keys
**Status:** ✅ RESOLVED

**Finding:** Google Maps API key was shown hardcoded in example code.

**Fix Implemented:**
- Removed all hardcoded API keys from examples
- Updated all provider configuration examples to load keys from `IConfiguration`
- Added comprehensive API key management guide showing environment variables, user secrets, and Azure Key Vault integration
- All examples now use `Configuration["MapProviders:GoogleKey"]` pattern

**Action Required:** Never hardcode API keys. Load from configuration, environment variables, or secrets manager.

**Reference:** [map-providers.md#managing-api-keys-securely](references/map-providers.md)

### [EXTERNAL_DOWNLOADS] External Tile and Data Downloads
**Status:** ✅ DOCUMENTED AS EXPECTED BEHAVIOR

**Finding:** Skill downloads map tiles from tile.openstreetmap.org, cdn.syncfusion.com, and maps.googleapis.com.

**Explanation:** These downloads are **expected and necessary** for map visualization. Map tiles MUST come from a tile provider. This is normal behavior, not a vulnerability.

**Best Practices:**
- Use local tile caching in production environments
- Implement a server-side proxy to validate tile URLs
- Enforce HTTPS for all tile requests
- Configure domain allow-lists

**Reference:** [map-providers.md#external-asset-downloads-expected-behavior](references/map-providers.md)

### [COMMAND_EXECUTION] IJSRuntime Browser API Usage
**Status:** ✅ RESOLVED WITH SAFEGUARDS

**Finding:** Skill uses IJSRuntime for localStorage access and dynamic CSS/script injection.

**Safeguards Implemented:**
- Never allow user-controlled or external data in JavaScript interop
- All CSS theme URLs must come from allow-listed set of known themes
- No dynamic script loading from untrusted sources
- Input validation required before any JS invocation
- Content Security Policy headers block unauthorized script execution

**Secure Pattern:**
```csharp
// SAFE: Only pre-defined themes allowed
private readonly Dictionary<string, string> AllowedThemes = new()
{
    { "bootstrap5", "_content/Syncfusion.Blazor/styles/bootstrap5.css" },
    { "material", "_content/Syncfusion.Blazor/styles/material.css" }
};

private async Task SwitchThemeSafely(string themeName)
{
    if (!AllowedThemes.ContainsKey(themeName))
        throw new SecurityException($"Theme not allowed");
    await JS.InvokeVoidAsync("loadThemeSafely", AllowedThemes[themeName]);
}
```

**Reference:** [state-persistence.md](references/state-persistence.md) | [customization-and-styling.md](references/customization-and-styling.md)

### [PROMPT_INJECTION] Untrusted Data to Automated Agents
**Status:** ✅ RESOLVED WITH COMPREHENSIVE CONTROLS

**Finding:** GeoJSON and map metadata can include text resembling instructions, risking prompt injection if passed to LLMs/agents without sanitization.

**Multi-Layer Mitigation Implemented:**

1. **Capability Boundaries:** Skill designated for UI rendering ONLY
   - Explicit prohibition on passing raw data to LLMs/agents
   - All external content treated as untrusted

2. **Pattern Detection:** Implemented instruction pattern detection
   - Detects: "system:", "ignore", "bypass", "execute", "[SYSTEM]", etc.
   - Immediate rejection on suspicious content

3. **Sanitization:** All HTML content sanitized before rendering
   - HtmlSanitizer library removes scripts and unsafe tags
   - HTML encoding applied to external properties
   - No raw HTML from external sources

4. **Validation Pipeline:** Comprehensive GeoJSON validation
   - Schema validation (FeatureCollection structure)
   - Size limits (5MB max, 10,000 features max)
   - Property validation (50 max, 1000 chars each)
   - Coordinate range validation (-90/90 lat, -180/180 lng)

5. **Boundary Markers:** If automation required
   - Use `[MAP_DATA_START] ... [MAP_DATA_END]` delimiters
   - Strict length limits (256 chars per property, 5000 total)
   - Rejection policy on validation failure

6. **Logging & Audit:** Comprehensive security logging
   - All validation failures logged
   - Suspicious patterns logged with context
   - Human review required before any automation

**Production Checklist:**
- [ ] No hardcoded API keys
- [ ] GeoJSON from local files or validated provider
- [ ] HTTPS enforced for external requests
- [ ] HtmlSanitizer installed and configured
- [ ] Content Security Policy headers set
- [ ] Pattern rejection policy implemented
- [ ] Comprehensive logging enabled
- [ ] API keys in secrets manager
- [ ] Security review completed

**Reference:** [readme-security.md](references/readme-security.md) | [user-interactions.md#preventing-prompt-injection](references/user-interactions.md) | [data-visualization.md#validating-external-geojson-data](references/data-visualization.md)

---

## Documentation and Navigation Guide

Choose the reference guide that matches your current task:

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup
- Basic map initialization and rendering
- CSS imports and theme configuration
- Creating your first interactive map
- Step-by-step setup walkthrough

### Map Providers and Configuration
📄 **Read:** [references/map-providers.md](references/map-providers.md)
- Google Maps setup and API key configuration
- Bing Maps setup and authentication
- Azure Maps provider configuration
- OpenStreetMap setup
- Provider comparison and selection guide

### Markers and Layers
📄 **Read:** [references/markers-and-layers.md](references/markers-and-layers.md)
- Adding and managing markers
- Marker clustering and grouping
- Working with layers and layer collections
- Toggling layer visibility
- Dynamic marker updates and data binding

### Spatial Features and Overlays
📄 **Read:** [references/spatial-features.md](references/spatial-features.md)
- Drawing polygons and geographic shapes
- Creating navigation lines and polylines
- Adding annotations with text, icons, and circles
- Creating data bubbles and interactive overlays
- Advanced spatial geometry features

### Data Visualization and Mapping
📄 **Read:** [references/data-visualization.md](references/data-visualization.md)
- Color mapping and choropleth visualization
- Configuring legends and legend placement
- Data labels on map elements
- Populating maps with geographic datasets
- Advanced data binding and visualization patterns

### User Interactions
📄 **Read:** [references/user-interactions.md](references/user-interactions.md)
- Handling mouse clicks and double-clicks
- Zoom and pan control configuration
- Tooltip and popup behavior
- Keyboard navigation support
- Custom interaction patterns

### Events and Methods
📄 **Read:** [references/events-and-methods.md](references/events-and-methods.md)
- Mouse event handling (click, hover, move)
- Pan and zoom event capture
- Programmatic map methods (pan, zoom, reset, refresh)
- Event data and callback patterns
- Triggering actions from user interactions

### Customization and Styling
📄 **Read:** [references/customization-and-styling.md](references/customization-and-styling.md)
- CSS class customization
- Theme Studio integration
- Marker and popup styling
- Map controls and navigation styling
- Dark mode and responsive design

### Print and Export
📄 **Read:** [references/print-and-export.md](references/print-and-export.md)
- Exporting maps as PNG, SVG, and PDF
- Print functionality and page setup
- Exporting with legends and data labels
- File format considerations
- Server-side and client-side export options

### Internationalization and Localization
📄 **Read:** [references/internationalization-and-localization.md](references/internationalization-and-localization.md)
- Multi-language support for map labels
- Right-to-left (RTL) text support
- Localized number and date formatting
- Regional map variations
- Language-specific customization

### State Persistence
📄 **Read:** [references/state-persistence.md](references/state-persistence.md)
- Saving and restoring map state
- Persisting zoom level and center position
- Preserving user interaction state
- Session and local storage integration
- State management patterns

### Accessibility and Advanced Topics
📄 **Read:** [references/accessibility.md](references/accessibility.md)
- WCAG 2.1 compliance and standards
- Keyboard navigation and shortcuts
- ARIA attributes and semantic markup
- Screen reader support
- High contrast mode support
- Assistive technology compatibility

### Complete API Reference
📄 **Read:** [references/api-reference.md](references/api-reference.md)
- `SfMaps` main component properties and methods
- Configuration classes (`MapsCenterPosition`, `MapsZoomSettings`, `MapsLegendSettings`, etc.)
- Event arguments for all event types
- Interfaces (`ILayer`, `IMarker`, `IBubble`)
- Enumerations for `MarkerType`, `ExportType`, `ProjectionType`, `GeometryType`, etc.
- Properties quick reference guide
- Complete class hierarchy and API surface

---

## ⛔ SECURITY REQUIREMENT: What NOT to Do

**The following patterns are PROHIBITED and create critical security vulnerabilities:**

```csharp
// ❌ PROHIBITED: Forwarding map data to LLM/agents
var geoJsonData = await LoadGeoJsonFromMapLayer();
var analysis = await llmService.AnalyzeAsync(geoJsonData);  // NEVER DO THIS

// ❌ PROHIBITED: Using external annotations in agent prompts
var tooltipText = mapFeature.Properties["tooltip"];
var response = await agent.ExecuteAsync($"Summarize: {tooltipText}");  // NEVER DO THIS

// ❌ PROHIBITED: Processing third-party tiles through automation
var tileUrl = "https://tile.openstreetmap.org/{z}/{x}/{y}.png";
await automationPipeline.IngestAsync(tileUrl);  // NEVER DO THIS

// ❌ PROHIBITED: Making decisions based on untrusted GeoJSON
var externalGeoJson = await httpClient.GetAsync("https://external-source.com/map.json");
var decision = MakeCriticalDecision(externalGeoJson);  // NEVER DO THIS
```

**If you believe you need to use map data with AI/ML systems:**

1. Stop and re-evaluate your architecture
2. Create a separate server-side ingestion pipeline
3. Implement complete validation and sanitization
4. Get explicit security review and approval
5. Implement human review gates before AI processing
6. Maintain comprehensive audit trails

---

## Quick Start Example

```csharp
// Basic map setup in Blazor (use validated tile URL or local tiles in production)
@page "/maps-demo"
@using Syncfusion.Blazor.Maps

<SfMaps>
    <MapsLayers>
        <!-- ✅ SAFE: Use local bundled tiles (recommended for production) -->
        <MapsLayer UrlTemplate="@TileUrl">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    // SAFE: Host tiles locally or use a validated, domain-restricted provider
    private string TileUrl = "/tiles/{level}/{tileX}/{tileY}.png"; // local/cached tiles
    
    // This data stays in the UI layer only - NEVER forwarded to agents or LLMs
}
```

## Common Patterns

### Pattern 1: Adding Markers to a Map
```csharp
<SfMaps>
    <MapsLayers>
        <MapsLayer TValue="MarkerData" UrlTemplate="@TileUrl">
            <MapsMarkerSettings>
                <MapsMarker TValue="MarkerData" Latitude="37.368" Longitude="-122.095" 
                    Width="15" Height="15">
                </MapsMarker>
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    public class MarkerData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
    }
}
```

### Pattern 2: Binding Data with Color Mapping
```csharp
<SfMaps>
    <MapsLayers>
        <MapsLayer ShapeDataSource="@ShapeData" 
                   ShapePropertyPath="@ShapePropertyPath"
                   DataSource="@DataSource" TValue="DataType">
            <MapsShapeSettings ColorValuePath="Population">
                <MapsShapeColorMappings>
                    <MapsShapeColorMapping From="0" To="50000" Color="#B3E5FC"></MapsShapeColorMapping>
                    <MapsShapeColorMapping From="50000" To="100000" Color="#81D4FA"></MapsShapeColorMapping>
                </MapsShapeColorMappings>
            </MapsShapeSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>
```

### Pattern 3: Handling Map Click Events
```csharp
<SfMaps @ref="mapInstance" OnShapeSelected="ShapeSelected" 
        OnMarkerClick="MarkerClicked">
    <MapsLayers>
        <MapsLayer UrlTemplate="@TileUrl">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;

    private void MarkerClicked(MarkerClickEventArgs args)
    {
        Console.WriteLine($"Marker clicked at: {args.Latitude}, {args.Longitude}");
    }

    private void ShapeSelected(ShapeSelectedEventArgs args)
    {
        Console.WriteLine($"Shape selected: {args.Data}");
    }
}
```

## Key Features Summary

- **Multiple Map Providers:** Support for Google Maps, Bing Maps, Azure Maps, and OpenStreetMap
- **Rich Geospatial Features:** Markers, polygons, polylines, annotations, and bubbles
- **Data Visualization:** Color mapping, choropleth support, legends, and data labels
- **Interactivity:** Click handling, hover tooltips, zoom/pan controls, keyboard navigation
- **Layer Management:** Multi-layer support with visibility toggling and dynamic updates
- **Export Capabilities:** PNG, SVG, and PDF export with legends and labels
- **Internationalization:** Multi-language support and RTL text rendering
- **Accessibility:** WCAG compliance with keyboard navigation and screen reader support
- **Customization:** Theme Studio integration, CSS customization, marker styling
- **Performance:** Optimized rendering for large datasets and marker clustering

