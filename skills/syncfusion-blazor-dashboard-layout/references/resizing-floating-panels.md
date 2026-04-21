# Resizing and Floating Panels

## Table of Contents
- [Panel Resizing](#panel-resizing)
- [Resize Directions](#resize-directions)
- [Floating Panels](#floating-panels)
- [Combining Resizing and Floating](#combining-resizing-and-floating)
- [Practical Examples](#practical-examples)

## Panel Resizing

### Enabling Panel Resizing

Use the `AllowResizing` property to enable users to dynamically adjust panel dimensions:

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    Columns="5" 
    CellAspectRatio="2"
    AllowResizing="true">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <ContentTemplate><div>0</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=2 SizeY=2 Column=1>
            <ContentTemplate><div>1</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeY=2 Column=3>
            <ContentTemplate><div>2</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel Row=1>
            <ContentTemplate><div>3</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .e-panel-content {
        text-align: center;
        margin-top: 10px;
    }
</style>
```

### Default Resize Behavior

When `AllowResizing="true"`:
- Resize handle appears in the **south-east corner** (bottom-right) of each panel
- Users drag the handle to resize the panel
- Resize icon Class: `.e-resize.e-double`
- Only south-east direction is active by default

## Resize Directions

### ResizableHandles Property

Customize which directions panels can be resized:

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    Columns="5" 
    CellAspectRatio="2"
    AllowResizing="true"
    ResizableHandles="e,south-east,west,south">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <ContentTemplate><div>0</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=2 SizeY=2 Column=1>
            <ContentTemplate><div>1</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeY=2 Column=3>
            <ContentTemplate><div>2</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel Row=1>
            <ContentTemplate><div>3</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .e-panel-content {
        text-align: center;
        margin-top: 10px;
    }
</style>
```

### Available Resize Directions

The `ResizableHandles` property accepts comma-separated values:

| Direction | Abbreviation | Handle Position | Use Case |
|-----------|--------------|-----------------|----------|
| East | `e` | Right side | Resize width only |
| West | `w` | Left side | Resize from left |
| South | `s` | Bottom | Resize height only |
| North | `n` | Top | Resize from top |
| South-East | `south-east` | Bottom-right corner | Default, both dimensions |
| South-West | `south-west` | Bottom-left corner | Both dimensions from left |
| North-East | `north-east` | Top-right corner | Both dimensions from top |
| North-West | `north-west` | Top-left corner | Both dimensions from top-left |

### Common Resize Configurations

**Only Horizontal Resizing:**
```csharp
ResizableHandles="e,w"
```

**Only Vertical Resizing:**
```csharp
ResizableHandles="s,n"
```

**All Directions:**
```csharp
ResizableHandles="e,w,s,n,south-east,south-west,north-east,north-west"
```

**Corner Only (Default Behavior):**
```csharp
ResizableHandles="south-east"
```

## Floating Panels

### AllowFloating Property

Floating allows panels to automatically move upward to fill empty cells in preceding rows, optimizing space utilization:

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    Columns="5" 
    CellAspectRatio="2"
    AllowFloating="true">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <ContentTemplate><div>0</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=2 SizeY=2 Column=1>
            <ContentTemplate><div>1</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeY=2 Column=3>
            <ContentTemplate><div>2</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel Row=1>
            <ContentTemplate><div>3</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .e-panel-content {
        text-align: center;
        margin-top: 10px;
    }
</style>
```

### Floating Behavior

When `AllowFloating="true"`:
1. Panels automatically float upward to fill empty cells
2. Empty spaces in preceding rows are occupied first
3. Maintains compact, optimized layout
4. Applies during panel drag operations
5. Applies when panels are resized

### When to Use Floating

**Use floating when:**
- You want maximum space efficiency
- Panels of varying sizes need to coexist
- You want automatic layout optimization
- Space utilization is critical

**Avoid floating when:**
- You want explicit control over panel positions
- Users expect panels to stay in defined rows
- You need predictable layout structure

## Combining Resizing and Floating

### Full Interactive Layout

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    Columns="5" 
    CellAspectRatio="2"
    AllowDragging="true"
    AllowResizing="true"
    AllowFloating="true">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
            <ContentTemplate><div>Draggable, Resizable, Floatable</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=2 SizeY=2 Column=1>
            <HeaderTemplate><div>Panel 2 (2x2)</div></HeaderTemplate>
            <ContentTemplate><div>All interactions enabled</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeY=2 Column=3>
            <HeaderTemplate><div>Panel 3</div></HeaderTemplate>
            <ContentTemplate><div>Full interactivity</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel Row=1>
            <HeaderTemplate><div>Panel 4</div></HeaderTemplate>
            <ContentTemplate><div>Drag, Resize, Float</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .e-panel-header {
        background-color: rgba(0, 0, 0, .1);
        text-align: center;
        padding: 10px;
    }
    .e-panel-content {
        text-align: center;
        margin-top: 10px;
        padding: 10px;
    }
</style>
```

## Practical Examples

### Example 1: Admin Dashboard with Constrained Resizing

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    Columns="6"
    AllowResizing="true"
    ResizableHandles="south-east">
    <DashboardLayoutPanels>
        <!-- Metrics cards - minimal resize -->
        <DashboardLayoutPanel 
            SizeX=2 
            MinSizeX=2 
            MinSizeY=1>
            <HeaderTemplate><div>Sessions</div></HeaderTemplate>
            <ContentTemplate><div class="metric">124,444</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel 
            SizeX=2 
            Column=2 
            MinSizeX=2 
            MinSizeY=1>
            <HeaderTemplate><div>Users</div></HeaderTemplate>
            <ContentTemplate><div class="metric">64,496</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel 
            SizeX=2 
            Column=4 
            MinSizeX=2 
            MinSizeY=1>
            <HeaderTemplate><div>Views</div></HeaderTemplate>
            <ContentTemplate><div class="metric">442,278</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Charts - flexible resizing -->
        <DashboardLayoutPanel 
            SizeX=3 
            SizeY=2 
            Row=1 
            MinSizeX=2 
            MaxSizeX=4>
            <HeaderTemplate><div>Chart 1</div></HeaderTemplate>
            <ContentTemplate><div>Chart visualization</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel 
            SizeX=3 
            SizeY=2 
            Row=1 
            Column=3 
            MinSizeX=2 
            MaxSizeX=4>
            <HeaderTemplate><div>Chart 2</div></HeaderTemplate>
            <ContentTemplate><div>Chart visualization</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .metric {
        font-size: 24px;
        font-weight: bold;
        text-align: center;
        padding: 20px;
        color: #334099;
    }
    .e-panel-header {
        background-color: #f5f5f5;
        padding: 8px;
        font-weight: bold;
    }
    .e-panel-content {
        padding: 10px;
    }
</style>
```

### Example 2: Responsive Workspace with Full Interactivity

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{8, 8})" 
    Columns="12"
    AllowDragging="true"
    AllowResizing="true"
    AllowFloating="true"
    ResizableHandles="e,s,south-east">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel SizeX=6 SizeY=3>
            <HeaderTemplate><div>Main Chart</div></HeaderTemplate>
            <ContentTemplate>
                <div>Large chart area - drag to rearrange, resize from edges</div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=6 SizeY=3 Column=6>
            <HeaderTemplate><div>Data Grid</div></HeaderTemplate>
            <ContentTemplate>
                <div>Data table - fully interactive</div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=3 SizeY=2 Row=3>
            <HeaderTemplate><div>Widget 1</div></HeaderTemplate>
            <ContentTemplate>
                <div>Metric widget</div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=3 SizeY=2 Row=3 Column=3>
            <HeaderTemplate><div>Widget 2</div></HeaderTemplate>
            <ContentTemplate>
                <div>Status widget</div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=6 SizeY=2 Row=3 Column=6>
            <HeaderTemplate><div>Timeline</div></HeaderTemplate>
            <ContentTemplate>
                <div>Timeline visualization</div>
            </ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .e-panel-header {
        background-color: #334099;
        color: white;
        padding: 12px;
        font-weight: bold;
    }
    .e-panel-content {
        padding: 15px;
        background-color: #fafafa;
    }
</style>
```

### Customizing Resize Icon

The resize icon can be customized via CSS:

```css
.e-dashboardlayout.e-control .e-panel .e-panel-container .e-resize.e-double {
    color: #0378d5;
    font-size: 30px;
    height: 20px;
    width: 20px;
}
```
