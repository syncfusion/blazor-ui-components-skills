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

## Automatic State Persistence (NEW - Previously Missing)

### EnablePersistence Property

Enable automatic persistence of map state across page refreshes:

```csharp
@page "/auto-persistence"
@using Syncfusion.Blazor.Maps

<SfMaps EnablePersistence="true"  // ← NEW: Automatically saves state
        ID="maps-instance">       // ← ID is required when EnablePersistence is true
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
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
@inject IJSRuntime JS

<button @onclick="SaveMapState">Save Map State</button>
<button @onclick="RestoreMapState">Restore State</button>
<button @onclick="ClearSavedState">Clear Saved State</button>

<SfMaps @ref="mapInstance">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
    <MapsZoomSettings ZoomFactor="@CurrentZoomLevel"></MapsZoomSettings>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private MapsCenterPosition CurrentCenter = new MapsCenterPosition { Latitude = 39.0, Longitude = -98.0 };
    private int CurrentZoomLevel = 4;
    private const string StateKey = "mapState";

    private async Task SaveMapState()
    {
        var state = new MapStateData
        {
            Latitude = mapInstance.CenterPosition.Latitude,
            Longitude = mapInstance.CenterPosition.Longitude,
            ZoomLevel = mapInstance.ZoomLevel,
            SavedAt = DateTime.Now
        };

        // Save to browser local storage
        string json = System.Text.Json.JsonSerializer.Serialize(state);
        await JS.InvokeVoidAsync("localStorage.setItem", StateKey, json);
        
        Console.WriteLine("Map state saved");
    }

    private async Task RestoreMapState()
    {
        string json = await JS.InvokeAsync<string>("localStorage.getItem", StateKey);
        
        if (!string.IsNullOrEmpty(json))
        {
            var state = System.Text.Json.JsonSerializer.Deserialize<MapStateData>(json);
            
            CurrentCenter = new MapsCenterPosition 
            { 
                Latitude = state.Latitude, 
                Longitude = state.Longitude 
            };
            CurrentZoomLevel = state.ZoomLevel;
            
            await mapInstance.RefreshAsync();
            Console.WriteLine("Map state restored");
        }
    }

    private async Task ClearSavedState()
    {
        await JS.InvokeVoidAsync("localStorage.removeItem", StateKey);
        Console.WriteLine("Saved state cleared");
    }

    public class MapStateData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public int ZoomLevel { get; set; }
        public DateTime SavedAt { get; set; }
    }
}
```

### Auto-Save State on Navigation

```csharp
@page "/auto-persist"
@using Syncfusion.Blazor.Maps
@inject IJSRuntime JS
@implements IAsyncDisposable

<SfMaps @ref="mapInstance" OnZoomChange="OnMapChanged" OnPan="OnMapChanged">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private Timer autoSaveTimer;
    private bool isDisposed = false;

    protected override void OnInitialized()
    {
        // Auto-save every 30 seconds
        autoSaveTimer = new Timer(async _ => await SaveMapState(), null, 
            TimeSpan.FromSeconds(30), TimeSpan.FromSeconds(30));
    }

    private void OnMapChanged(dynamic args)
    {
        // Save immediately when user changes map
        _ = SaveMapState();
    }

    private async Task SaveMapState()
    {
        if (mapInstance == null || isDisposed) return;

        var state = new MapStateData
        {
            Latitude = mapInstance.CenterPosition.Latitude,
            Longitude = mapInstance.CenterPosition.Longitude,
            ZoomLevel = mapInstance.ZoomLevel,
            SavedAt = DateTime.Now
        };

        string json = System.Text.Json.JsonSerializer.Serialize(state);
        await JS.InvokeVoidAsync("localStorage.setItem", "mapState", json);
    }

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            // Restore state on page load
            string json = await JS.InvokeAsync<string>("localStorage.getItem", "mapState");
            if (!string.IsNullOrEmpty(json))
            {
                var state = System.Text.Json.JsonSerializer.Deserialize<MapStateData>(json);
                mapInstance.CenterPosition = new MapsCenterPosition 
                { 
                    Latitude = state.Latitude, 
                    Longitude = state.Longitude 
                };
                // Note: ZoomLevel is set via property in component
            }
        }
    }

    async ValueTask IAsyncDisposable.DisposeAsync()
    {
        isDisposed = true;
        autoSaveTimer?.Dispose();
        await SaveMapState();
    }

    public class MapStateData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public int ZoomLevel { get; set; }
        public DateTime SavedAt { get; set; }
    }
}
```

## Persisting Zoom and Center Position

### Manual Zoom/Center Bookmarking

```csharp
@page "/bookmarks"
@using Syncfusion.Blazor.Maps
@inject IJSRuntime JS

<div style="margin-bottom: 20px; border: 1px solid #ddd; padding: 15px; border-radius: 4px;">
    <h3>Bookmarks</h3>
    
    <button @onclick="AddBookmark" style="margin-bottom: 10px;">
        Add Bookmark
    </button>

    <div>
        @foreach (var (name, bookmark) in Bookmarks)
        {
            <div style="margin: 8px 0;">
                <button @onclick="() => GoToBookmark(bookmark)" 
                    style="padding: 6px 12px; margin-right: 8px;">
                    @name
                </button>
                <button @onclick="() => RemoveBookmark(name)" 
                    style="padding: 6px 12px; background: #dc3545; color: white;">
                    Delete
                </button>
            </div>
        }
    </div>
</div>

<SfMaps @ref="mapInstance" >
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
    <MapsZoomSettings ZoomFactor="@CurrentZoomLevel"></MapsZoomSettings>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private int CurrentZoomLevel = 4;
    private Dictionary<string, BookmarkData> Bookmarks = new();

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            await LoadBookmarks();
        }
    }

    private async Task AddBookmark()
    {
        string name = await JS.InvokeAsync<string>("prompt", "Enter bookmark name:");
        
        if (!string.IsNullOrEmpty(name))
        {
            var bookmark = new BookmarkData
            {
                Name = name,
                Latitude = mapInstance.CenterPosition.Latitude,
                Longitude = mapInstance.CenterPosition.Longitude,
                ZoomLevel = mapInstance.ZoomLevel,
                CreatedAt = DateTime.Now
            };

            Bookmarks[name] = bookmark;
            await SaveBookmarks();
        }
    }

    private async Task GoToBookmark(BookmarkData bookmark)
    {
        CurrentCenter = new MapsCenterPosition 
        { 
            Latitude = bookmark.Latitude, 
            Longitude = bookmark.Longitude 
        };
        CurrentZoomLevel = bookmark.ZoomLevel;
        
        await mapInstance.PanByDirectionAsync(Syncfusion.Blazor.Maps.PanDirection.Left, CurrentCenter);
    }

    private async Task RemoveBookmark(string name)
    {
        if (Bookmarks.Remove(name))
        {
            await SaveBookmarks();
        }
    }

    private async Task SaveBookmarks()
    {
        string json = System.Text.Json.JsonSerializer.Serialize(Bookmarks);
        await JS.InvokeVoidAsync("localStorage.setItem", "mapBookmarks", json);
        StateHasChanged();
    }

    private async Task LoadBookmarks()
    {
        string json = await JS.InvokeAsync<string>("localStorage.getItem", "mapBookmarks");
        
        if (!string.IsNullOrEmpty(json))
        {
            Bookmarks = System.Text.Json.JsonSerializer.Deserialize<Dictionary<string, BookmarkData>>(json) ?? new();
            StateHasChanged();
        }
    }

    public class BookmarkData
    {
        public string Name { get; set; }
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public int ZoomLevel { get; set; }
        public DateTime CreatedAt { get; set; }
    }
}
```

## Preserving User Interaction State

### Selection State Persistence

```csharp
@page "/persist-selection"
@using Syncfusion.Blazor.Maps
@inject IJSRuntime JS

<button @onclick="ClearSelections">Clear All Selections</button>

<SfMaps @ref="mapInstance" OnMarkerClick="MarkerClickHandler" OnShapeSelected="ShapeSelectHandler">
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
            <MapsMarkerSettings>
                @foreach (var marker in AllMarkers)
                {
                    var isSelected = SelectedMarkers.Contains(marker.Id);
                    <MapsMarker Latitude="@marker.Latitude" Longitude="@marker.Longitude"
                        Fill="@(isSelected ? "red" : "blue")"
                        Shape="MarkerType.Circle" Width="15" Height="15">
                    </MapsMarker>
                }
            </MapsMarkerSettings>
        </MapsLayer>
    </MapsLayers>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private List<MarkerData> AllMarkers = new()
    {
        new MarkerData { Id = 1, Latitude = 37.368, Longitude = -122.095, Name = "SF" },
        new MarkerData { Id = 2, Latitude = 40.7128, Longitude = -74.0060, Name = "NYC" }
    };
    private HashSet<int> SelectedMarkers = new();
    private const string SelectionKey = "mapSelections";

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            await LoadSelections();
        }
    }

    private void MarkerClickHandler(MarkerClickEventArgs args)
    {
        var marker = AllMarkers.FirstOrDefault(m => 
            m.Latitude == args.Latitude && m.Longitude == args.Longitude);
        
        if (marker != null)
        {
            if (SelectedMarkers.Contains(marker.Id))
                SelectedMarkers.Remove(marker.Id);
            else
                SelectedMarkers.Add(marker.Id);

            _ = SaveSelections();
        }
    }

    private void ShapeSelectHandler(ShapeSelectedEventArgs args)
    {
        // Handle shape selection
    }

    private async Task SaveSelections()
    {
        string json = System.Text.Json.JsonSerializer.Serialize(SelectedMarkers.ToList());
        await JS.InvokeVoidAsync("localStorage.setItem", SelectionKey, json);
        StateHasChanged();
    }

    private async Task LoadSelections()
    {
        string json = await JS.InvokeAsync<string>("localStorage.getItem", SelectionKey);
        
        if (!string.IsNullOrEmpty(json))
        {
            var selections = System.Text.Json.JsonSerializer.Deserialize<List<int>>(json);
            SelectedMarkers = new HashSet<int>(selections ?? new());
            StateHasChanged();
        }
    }

    private async Task ClearSelections()
    {
        SelectedMarkers.Clear();
        await JS.InvokeVoidAsync("localStorage.removeItem", SelectionKey);
        StateHasChanged();
    }

    public class MarkerData
    {
        public int Id { get; set; }
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public string Name { get; set; }
    }
}
```

## Session Management

### Session-Based State

```csharp
@page "/session-map"
@using Syncfusion.Blazor.Maps
@inject IJSRuntime JS

<div>
    <p>Session ID: @SessionId</p>
    <button @onclick="ExportSession">Export Session</button>
    <button @onclick="ImportSession">Import Session</button>
</div>

<SfMaps @ref="mapInstance" >
    <MapsLayers>
        <MapsLayer UrlTemplate="https://tile.openstreetmap.org/level/tileX/tileY.png" TValue="string">
        </MapsLayer>
    </MapsLayers>
    <MapsZoomSettings ZoomFactor="@CurrentZoomLevel"></MapsZoomSettings>
</SfMaps>

@code {
    private SfMaps mapInstance;
    private MapsCenterPosition CurrentCenter = new MapsCenterPosition { Latitude = 39.0, Longitude = -98.0 };
    private int CurrentZoomLevel = 4;
    private string SessionId;

    protected override void OnInitialized()
    {
        SessionId = Guid.NewGuid().ToString().Substring(0, 8);
    }

    private async Task ExportSession()
    {
        var session = new SessionData
        {
            SessionId = SessionId,
            MapState = new MapStateData
            {
                Latitude = mapInstance.CenterPosition.Latitude,
                Longitude = mapInstance.CenterPosition.Longitude,
                ZoomLevel = mapInstance.ZoomLevel
            },
            ExportedAt = DateTime.Now
        };

        string json = System.Text.Json.JsonSerializer.Serialize(session);
        var content = System.Text.Encoding.UTF8.GetBytes(json);
        
        await JS.InvokeVoidAsync("downloadFile", "session.json", Convert.ToBase64String(content));
    }

    private async Task ImportSession()
    {
        // File input would be implemented via file upload
    }

    public class SessionData
    {
        public string SessionId { get; set; }
        public MapStateData MapState { get; set; }
        public DateTime ExportedAt { get; set; }
    }

    public class MapStateData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public int ZoomLevel { get; set; }
    }
}
```


