# State Persistence

## Table of Contents
- [Overview](#overview)
- [Automatic Persistence](#automatic-persistence)
- [Manual State Management](#manual-state-management)
- [Persistence Examples](#persistence-examples)
- [Troubleshooting](#troubleshooting)

## Overview

Dashboard Layout state persistence allows users' custom layout configurations to be saved and automatically restored. This ensures a consistent user experience, with panel positions, sizes, and arrangements remaining intact across browser sessions and page refreshes.

**What Gets Persisted:**
- Panel **Column** positions
- Panel **Row** positions
- Panel widths (**SizeX**)
- Panel heights (**SizeY**)

**Storage Mechanism:**
- Automatically saved to browser **localStorage**
- Persists across browser sessions
- Persists across page refreshes
- Component ID is used as storage key

## Automatic Persistence

### Enabling Persistence

Use the `EnablePersistence` property to automatically save and restore layout state:

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    ID="dashboard" 
    CellSpacing="@(new double[]{10, 10})" 
    Columns="6" 
    EnablePersistence="true">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <HeaderTemplate><div>Panel 0</div></HeaderTemplate>
            <ContentTemplate><div>Panel Content</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeX=2 SizeY=2 Column=1>
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
            <ContentTemplate><div>Panel Content</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeY=2 Column=3>
            <HeaderTemplate><div>Panel 2</div></HeaderTemplate>
            <ContentTemplate><div>Panel Content</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Row=1>
            <HeaderTemplate><div>Panel 3</div></HeaderTemplate>
            <ContentTemplate><div>Panel Content</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    #dashboard .e-panel-header {
        background-color: rgba(0, 0, 0, .1);
        text-align: center;
    }

    #dashboard .e-panel-content {
        text-align: center;
        margin-top: 10px;
    }
</style>
```

**Important Requirements:**
1. **ID attribute is MANDATORY** - The component ID is used as the localStorage key
2. Set `EnablePersistence="true"`
3. localStorage must be available in browser
4. Persistence happens automatically on layout changes

### How Automatic Persistence Works

1. **Initial Load:** Dashboard renders with defined Row/Column/SizeX/SizeY
2. **User Interaction:** User drags, resizes, or rearranges panels
3. **Automatic Save:** Layout automatically saved to localStorage
4. **Page Refresh:** On page reload, Dashboard retrieves saved state and restores it
5. **Next Visit:** Even after closing browser, state persists

### Storage Key Format

The component automatically creates a localStorage key using the component ID:

```
localStorage key: "dashboard"  (uses ID value)
```

If ID is not set, persistence may not work correctly.

## Manual State Management

### Programmatic Methods

The Dashboard Layout provides three methods for explicit state control:

#### GetPersistDataAsync
Retrieves the current layout state as a serialized string:

```csharp
@using Syncfusion.Blazor.Layouts
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="SaveState">Save State</SfButton>

<SfDashboardLayout @ref="dashboardObject" ID="dashboard" CellSpacing="@(new double[]{10, 10})" Columns="6">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <HeaderTemplate><div>Panel 0</div></HeaderTemplate>
            <ContentTemplate><div>Panel Content</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeX=2 SizeY=2 Column=1>
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
            <ContentTemplate><div>Panel Content</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    SfDashboardLayout dashboardObject;
    private string savedState;

    private async Task SaveState()
    {
        if (dashboardObject != null)
        {
            // Retrieve current state as string
            savedState = await dashboardObject.GetPersistDataAsync();
            Console.WriteLine("State saved: " + savedState);
        }
    }
}
```

#### SetPersistDataAsync
Restores a previously saved layout state:

```csharp
@using Syncfusion.Blazor.Layouts
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="RestoreState">Restore State</SfButton>

<SfDashboardLayout 
    @ref="dashboardObject" 
    ID="dashboard" 
    CellSpacing="@(new double[]{10, 10})" 
    Columns="6">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <HeaderTemplate><div>Panel 0</div></HeaderTemplate>
            <ContentTemplate><div>Panel Content</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeX=2 SizeY=2 Column=1>
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
            <ContentTemplate><div>Panel Content</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    SfDashboardLayout dashboardObject;
    private string savedState;

    private async Task RestoreState()
    {
        if (dashboardObject != null && savedState != null)
        {
            // Restore saved state
            await dashboardObject.SetPersistDataAsync(savedState);
            Console.WriteLine("State restored");
        }
    }
}
```

#### ResetPersistDataAsync
Resets the layout to its original configuration and clears localStorage:

```csharp
@using Syncfusion.Blazor.Layouts
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="ResetState">Reset to Default</SfButton>

<SfDashboardLayout 
    @ref="dashboardObject" 
    ID="dashboard" 
    CellSpacing="@(new double[]{10, 10})" 
    Columns="6">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <HeaderTemplate><div>Panel 0</div></HeaderTemplate>
            <ContentTemplate><div>Panel Content</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeX=2 SizeY=2 Column=1>
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
            <ContentTemplate><div>Panel Content</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    SfDashboardLayout dashboardObject;

    private async Task ResetState()
    {
        if (dashboardObject != null)
        {
            // Reset to default and clear localStorage
            await dashboardObject.ResetPersistDataAsync();
            Console.WriteLine("State reset to default");
        }
    }
}
```

## Persistence Examples

### Example 1: Full State Management UI

```csharp
@using Syncfusion.Blazor.Layouts
@using Syncfusion.Blazor.Buttons

<div style="margin-bottom: 20px;">
    <SfButton OnClick="SaveState" CssClass="btn-primary">Save Layout</SfButton>
    <SfButton OnClick="SetState" CssClass="btn-success">Apply Saved Layout</SfButton>
    <SfButton OnClick="ResetState" CssClass="btn-danger">Reset to Default</SfButton>
    
    @if (!string.IsNullOrEmpty(statusMessage))
    {
        <div class="status-message">@statusMessage</div>
    }
</div>

<SfDashboardLayout 
    @ref="dashboardObject" 
    ID="dashboard" 
    CellSpacing="@(new double[]{10, 10})" 
    Columns="6" 
    EnablePersistence="true">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <HeaderTemplate><div>Panel 0</div></HeaderTemplate>
            <ContentTemplate>
                <div>
                    Drag me around!
                    <p>My position will be saved automatically.</p>
                </div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=2 SizeY=2 Column=1>
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
            <ContentTemplate>
                <div>
                    Resize me!
                    <p>Changes persist across page reloads.</p>
                </div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeY=2 Column=3>
            <HeaderTemplate><div>Panel 2</div></HeaderTemplate>
            <ContentTemplate>
                <div>
                    Save/restore/reset buttons above.
                    <p>Try different combinations.</p>
                </div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel Row=1>
            <HeaderTemplate><div>Panel 3</div></HeaderTemplate>
            <ContentTemplate>
                <div>
                    Your layout is your workspace.
                    <p>Customize to your preference.</p>
                </div>
            </ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    SfDashboardLayout dashboardObject;
    private string savedState;
    private string statusMessage;

    private async Task SaveState()
    {
        if (dashboardObject != null)
        {
            savedState = await dashboardObject.GetPersistDataAsync();
            statusMessage = "✓ Layout saved successfully";
            await Task.Delay(3000);
            statusMessage = "";
            StateHasChanged();
        }
    }

    private async Task SetState()
    {
        if (dashboardObject != null && savedState != null)
        {
            await dashboardObject.SetPersistDataAsync(savedState);
            statusMessage = "✓ Saved layout applied";
            await Task.Delay(3000);
            statusMessage = "";
            StateHasChanged();
        }
        else
        {
            statusMessage = "✗ No saved layout available";
            await Task.Delay(3000);
            statusMessage = "";
            StateHasChanged();
        }
    }

    private async Task ResetState()
    {
        if (dashboardObject != null)
        {
            await dashboardObject.ResetPersistDataAsync();
            statusMessage = "✓ Layout reset to default";
            await Task.Delay(3000);
            statusMessage = "";
            StateHasChanged();
        }
    }
}

<style>
    .btn-primary {
        background-color: #667eea;
        color: white;
        border: none;
        padding: 8px 16px;
        margin-right: 10px;
        cursor: pointer;
        border-radius: 3px;
    }
    .btn-success {
        background-color: #4caf50;
        color: white;
        border: none;
        padding: 8px 16px;
        margin-right: 10px;
        cursor: pointer;
        border-radius: 3px;
    }
    .btn-danger {
        background-color: #f44336;
        color: white;
        border: none;
        padding: 8px 16px;
        margin-right: 10px;
        cursor: pointer;
        border-radius: 3px;
    }
    .status-message {
        display: inline-block;
        margin-left: 20px;
        padding: 8px 12px;
        background-color: #e8f5e9;
        color: #2e7d32;
        border-radius: 3px;
    }
</style>
```

### Example 2: Dashboard with Preferences

```csharp
@using Syncfusion.Blazor.Layouts
@using Syncfusion.Blazor.Buttons

<div class="dashboard-toolbar">
    <h2>My Dashboard</h2>
    <div class="toolbar-buttons">
        <SfButton OnClick="SaveAsDefault">Save as Default</SfButton>
        <SfButton OnClick="ResetToDefault">Reset to Default</SfButton>
    </div>
</div>

<SfDashboardLayout 
    @ref="dashboardObject" 
    ID="user-dashboard"
    CellSpacing="@(new double[]{15, 15})" 
    Columns="6"
    AllowDragging="true"
    AllowResizing="true"
    EnablePersistence="true">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <HeaderTemplate><div>📊 Overview</div></HeaderTemplate>
            <ContentTemplate><div>Dashboard overview</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=2 Column=1>
            <HeaderTemplate><div>📈 Sales</div></HeaderTemplate>
            <ContentTemplate><div>Sales data</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=2 Column=3>
            <HeaderTemplate><div>👥 Users</div></HeaderTemplate>
            <ContentTemplate><div>User metrics</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=2 Column=5>
            <HeaderTemplate><div>⚙️ System</div></HeaderTemplate>
            <ContentTemplate><div>System status</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=3 Row=1>
            <HeaderTemplate><div>📋 Reports</div></HeaderTemplate>
            <ContentTemplate><div>Recent reports</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=3 Row=1 Column=3>
            <HeaderTemplate><div>🔔 Alerts</div></HeaderTemplate>
            <ContentTemplate><div>Active alerts</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    SfDashboardLayout dashboardObject;
    private string defaultState;

    protected override void OnAfterRender(bool firstRender)
    {
        if (firstRender)
        {
            // Load default state from localStorage if exists
            LoadDefaultState();
        }
    }

    private async Task SaveAsDefault()
    {
        if (dashboardObject != null)
        {
            defaultState = await dashboardObject.GetPersistDataAsync();
            // Optionally save to server or additional storage
            Console.WriteLine("Default layout saved");
        }
    }

    private async Task ResetToDefault()
    {
        if (dashboardObject != null)
        {
            await dashboardObject.ResetPersistDataAsync();
            Console.WriteLine("Reset to default");
        }
    }

    private void LoadDefaultState()
    {
        // Load default state if needed
    }
}

<style>
    .dashboard-toolbar {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 15px;
        background-color: #f5f5f5;
        margin-bottom: 15px;
        border-radius: 4px;
    }
    .toolbar-buttons button {
        margin-left: 10px;
    }
</style>
```

## Troubleshooting

### Persistence Not Working

**Problem:** Layout changes are not persisting across page reloads.

**Solutions:**
1. **Verify ID is set:** Dashboard Layout must have an ID attribute
   ```csharp
   <SfDashboardLayout ID="dashboard">  <!-- ID is required -->
   ```

2. **Check EnablePersistence:** Ensure it's set to true
   ```csharp
   <SfDashboardLayout EnablePersistence="true">
   ```

3. **Check localStorage availability:** Some browsers/incognito mode disable localStorage
   ```csharp
   // Test in browser console:
   // localStorage.setItem('test', 'value'); 
   // localStorage.getItem('test');
   ```

4. **Clear browser cache:** Sometimes old data interferes
   - DevTools → Application → LocalStorage → Delete entry

### State Mismatch

**Problem:** Saved state doesn't match current panel definitions.

**Solution:** If panel structure changes (panels added/removed), manually reset:
```csharp
await dashboardObject.ResetPersistDataAsync();
```

### localStorage Not Available

**Problem:** browserErrorreading/writing to localStorage (incognito, private mode).

**Solution:** Disable persistence or implement fallback:
```csharp
<SfDashboardLayout EnablePersistence="false">
    <!-- Manual save/restore using GetPersistDataAsync/SetPersistDataAsync -->
</SfDashboardLayout>
```

### State String Format

The state string is JSON-serialized. Example:
```json
{"panels":[{"row":0,"column":0,"sizeX":1,"sizeY":1}]}
```

Can be stored in database, sent to server, or transmitted via API for server-side persistence.
