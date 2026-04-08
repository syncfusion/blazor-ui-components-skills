## Table of Contents

- [Automatic State Persistence (NEW - Previously Missing)](#automatic-state-persistence-new---previously-missing)
   - [EnablePersistence Property](#enablepersistence-property)
- [Manual State Persistence (Manual Approach)](#manual-state-persistence-manual-approach)
   - [Saving and Restoring Map State](#saving-and-restoring-map-state)
   - [Auto-Save State on Navigation](#auto-save-state-on-navigation)
- [Persisting Zoom and Center Position](#persisting-zoom-and-center-position)
   - [Manual Zoom/Center Bookmarking](#manual-zoomcenter-bookmarking)
- [Preserving User Interaction State](#preserving-user-interaction-state)
   - [Selection State Persistence](#selection-state-persistence)
- [Session Management](#session-management)
   - [Session-Based State](#session-based-state)

# State Persistence

> **MANDATORY SECURITY NOTICE:** State persistence examples may reference external GeoJSON or tile sources. Do NOT persist untrusted GeoJSON or annotation payloads without server-side validation, sanitization, and human review. When storing or restoring state (localStorage, session), ensure persisted artifacts do not include raw ShapeData, HTML, or other untrusted content. Prefer storing only minimal state (center, zoom, visible layer IDs) and rehydrate content from validated, local sources.

> **COMMAND_EXECUTION WARNING (IJSRuntime):** This skill uses IJSRuntime for browser-side interactions (localStorage access, dynamic CSS/script injection). Never allow user-controlled or external data to be injected as script or style content. Always validate and sanitize any values passed to JavaScript interop. Do NOT dynamically load scripts or stylesheets from untrusted sources.

## Automatic State Persistence (NEW - Previously Missing)

### EnablePersistence Property

Enable automatic persistence of map state across page refreshes:

```csharp
@page "/auto-persistence"
@using Syncfusion.Blazor.Maps

<SfMaps EnablePersistence="true"  //  NEW: Automatically saves state
        ID="maps-instance">       //  ID is required when EnablePersistence is true
    <MapsLayers>
        <MapsLayer UrlTemplate="/tiles/{level}/{tileX}/{tileY}.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    // No additional code needed! State is automatically persisted.
}
```

**What Gets Persisted:**
- ✅ Map center position (Latitude/Longitude)
- ✅ Zoom level
- ✅ Selected shapes and markers
- ✅ Visible layers
- ✅ Layer properties

**Requirements:**
- Set `EnablePersistence="true"` on the SfMaps component
- Provide a unique `ID` attribute to identify the instance

**How It Works:**
1. Map state is automatically saved to browser local storage
2. When page reloads, previous state is restored
3. State is stored under key: `maps-instance` (your ID)

**Advanced Configuration:**
```csharp
<SfMaps EnablePersistence="true"
        ID="regional-maps">
    <!-- Map configuration -->
</SfMaps>
```

---

## Manual State Persistence (Manual Approach)

### Saving and Restoring Map State

If you prefer manual control over state management, save the map view programmatically:

Save the current map view and restore it later:

```csharp
@page "/state-persistence"
@using Syncfusion.Blazor.Maps

<button @onclick="SaveMapState">Enable Persistence</button>
<button @onclick="ClearMapState">Disable Persistence</button>
<SfMaps ID="Maps" EnablePersistence="@EnablePersistence">
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions ="/data/world-map.json"}'
                   DataSource="PopulationDetails" ShapeDataPath="Name" ShapePropertyPath='new string[] {"name"}' TValue="Country">
            <MapsBubbleSettings>
                <MapsBubble MinRadius="5" MaxRadius="25" Visible="true" ValuePath="Population" ColorValuePath="Color" DataSource="PopulationDetails" TValue="Country">
                </MapsBubble>
            </MapsBubbleSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    public bool EnablePersistence = false;
    public class Country
    {
        public string Name { get; set; }
        public double Population { get; set; }
        public string Color { get; set; }
    };

    public List<Country> PopulationDetails = new List<Country> {
        new Country { Name = "United States", Population = 325020000, Color = "#b5e485" },
        new Country { Name = "Russia", Population = 142905208, Color = "#7bc1e8" },
       new Country { Name ="India", Population=1198003000, Color = "#df819c" }
    };
     private void SaveMapState()
    {
        EnablePersistence = true;
    }
    private void ClearMapState()
    {
        EnablePersistence = false;
    }
}
```

### Auto-Save State on Navigation

```csharp
@page "/auto-persist"
@using Syncfusion.Blazor.Maps

<button @onclick="SaveMapState">Enable Persistence</button>
<button @onclick="ClearMapState">Disable Persistence</button>
<SfMaps ID="Maps" EnablePersistence="@EnablePersistence">
    <MapsLayers>
        <MapsLayer ShapeData='new {dataOptions ="/data/world-map.json"}' TValue="string">
        <MapsNavigationLines>
                <MapsNavigationLine Visible="true" Color="blue" Angle="90" Width="2" DashArray="4"
                                    Latitude="new double[]{ 40.7128, 36.7783 }" Longitude="new double[]{ -74.0060, -119.4179 }">
                </MapsNavigationLine>
            </MapsNavigationLines>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    public bool EnablePersistence = false;    
     private void SaveMapState()
    {
        EnablePersistence = true;
    }
    private void ClearMapState()
    {
        EnablePersistence = false;
    }
}
```