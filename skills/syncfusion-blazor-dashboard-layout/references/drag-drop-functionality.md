# Drag and Drop Functionality

## Table of Contents
- [Overview](#overview)
- [Enabling Drag and Drop](#enabling-drag-and-drop)
- [Default Drag Behavior](#default-drag-behavior)
- [Collision Handling](#collision-handling)
- [Customizing Drag Handles](#customizing-drag-handles)
- [Drag Handle Examples](#drag-handle-examples)

## Overview

Dashboard Layout provides built-in drag-and-drop functionality allowing users to rearrange panels dynamically. When dragging a panel:

1. A placeholder area appears showing the target position
2. Colliding panels are automatically pushed to available space
3. Panels adjust in real-time based on collision detection
4. Layout optimizes automatically to find suitable placement

## Enabling Drag and Drop

### AllowDragging Property

By default, drag-and-drop is enabled. Explicitly set the `AllowDragging` property when needed:

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    CellAspectRatio="2" 
    Columns="5"
    AllowDragging="true">
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

### Disabling Drag and Drop

```csharp
<SfDashboardLayout AllowDragging="false">
    <!-- Panels cannot be dragged -->
</SfDashboardLayout>
```

## Default Drag Behavior

### Full Panel as Drag Handle

By default, **the entire panel acts as the draggable area**. Users can click anywhere on a panel and drag it to a new location.

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    CellAspectRatio="2" 
    Columns="5">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <ContentTemplate>
                <div>Click anywhere on this panel to drag</div>
            </ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeX=2 SizeY=2 Column=1>
            <ContentTemplate>
                <div>This panel is also fully draggable</div>
            </ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeY=2 Column=3>
            <ContentTemplate>
                <div>Entire panel surface allows dragging</div>
            </ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Row=1>
            <ContentTemplate>
                <div>4</div>
            </ContentTemplate>
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

## Collision Handling

### Automatic Panel Pushing

When panels collide during drag operations, the colliding panels are automatically pushed in the best available direction (left, right, top, or bottom).

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    CellAspectRatio="2" 
    Columns="5">
    <DashboardLayoutPanels>
        <!-- When Panel 0 is dragged over Panel 1, Panel 1 is pushed -->
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

**Collision Behavior:**
1. Placeholder shows where panel will land
2. Colliding panels shift automatically
3. Panels push to available space (adaptive)
4. Layout reorganizes in real-time
5. On release, final positions are confirmed

### Placeholder Visualization

During drag operations, a visual placeholder area is displayed to indicate:
- Current drag position
- Where the panel will land if released
- Helps users determine optimal placement before dropping

## Customizing Drag Handles

### DraggableHandle Property

Restrict dragging to specific elements within panels using CSS selectors:

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    CellAspectRatio="2" 
    Columns="5" 
    DraggableHandle=".e-panel-header">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
            <ContentTemplate>
                <div>Content here is NOT draggable</div>
                <button>Buttons are clickable, not draggable</button>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=2 SizeY=2 Column=1>
            <HeaderTemplate><div>Panel 2</div></HeaderTemplate>
            <ContentTemplate>
                <div>Click header to drag</div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeY=2 Column=3>
            <HeaderTemplate><div>Panel 3</div></HeaderTemplate>
            <ContentTemplate>
                <div>Content is not draggable</div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel Row=1>
            <HeaderTemplate><div>Panel 4</div></HeaderTemplate>
            <ContentTemplate>
                <div>Only drag from header</div>
            </ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .e-panel-header {
        background-color: rgba(0, 0, 0, .1);
        text-align: center;
        cursor: move;
        padding: 10px;
    }
    .e-panel-content {
        text-align: center;
        margin-top: 10px;
    }
</style>
```

### CSS Selector Specificity

The `DraggableHandle` property accepts CSS selectors:

- **Class selector**: `.e-panel-header` (handles with this class)
- **Element selector**: `div` (drag from any div)
- **ID selector**: `#header` (specific element by ID)
- **Attribute selector**: `[data-handle="true"]` (elements with attribute)

### Use Cases for Custom Handles

**Header-Only Dragging:**
```csharp
DraggableHandle=".e-panel-header"
```
Users drag only from the header, allowing content interaction.

**Icon-Based Dragging:**
```csharp
DraggableHandle=".drag-icon"
```
Only a specific icon is draggable.

**Multiple Selectors:**
```csharp
DraggableHandle=".e-panel-header, .drag-zone"
```
Both elements can drag the panel.

## Drag Handle Examples

### Example 1: Header-Only Drag with Buttons

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    Columns="4"
    DraggableHandle=".panel-header">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <HeaderTemplate>
                <div class="panel-header">
                    <span>📊 Dashboard</span>
                </div>
            </HeaderTemplate>
            <ContentTemplate>
                <div>
                    <p>Drag from header only</p>
                    <button>Click me (not draggable)</button>
                </div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel Column=1>
            <HeaderTemplate>
                <div class="panel-header">
                    <span>📈 Analytics</span>
                </div>
            </HeaderTemplate>
            <ContentTemplate>
                <div>
                    <p>Interactive content</p>
                    <input type="text" placeholder="Type here">
                </div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel Column=2>
            <HeaderTemplate>
                <div class="panel-header">
                    <span>📋 Reports</span>
                </div>
            </HeaderTemplate>
            <ContentTemplate>
                <div>
                    <p>Content area</p>
                </div>
            </ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .panel-header {
        background-color: #334099;
        color: white;
        padding: 10px;
        cursor: move;
        font-weight: bold;
    }

    .e-panel-content {
        padding: 15px;
    }

    button {
        padding: 5px 10px;
        cursor: pointer;
    }

    input {
        padding: 5px;
        width: 100%;
    }
</style>
```

### Example 2: Drag Icon Handle

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    Columns="3"
    DraggableHandle=".drag-icon">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <HeaderTemplate>
                <div class="panel-header">
                    <span class="drag-icon">⋮⋮</span>
                    <span>Panel Title</span>
                </div>
            </HeaderTemplate>
            <ContentTemplate>
                <div>Drag from the icon (⋮⋮) only</div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel Column=1>
            <HeaderTemplate>
                <div class="panel-header">
                    <span class="drag-icon">⋮⋮</span>
                    <span>Another Panel</span>
                </div>
            </HeaderTemplate>
            <ContentTemplate>
                <div>Entire content area is interactive</div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel Column=2>
            <HeaderTemplate>
                <div class="panel-header">
                    <span class="drag-icon">⋮⋮</span>
                    <span>Third Panel</span>
                </div>
            </HeaderTemplate>
            <ContentTemplate>
                <div>Click anywhere except the icon to interact</div>
            </ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .panel-header {
        display: flex;
        align-items: center;
        gap: 10px;
        padding: 10px;
        background-color: #f0f0f0;
    }

    .drag-icon {
        cursor: move;
        font-weight: bold;
        color: #666;
    }

    .e-panel-content {
        padding: 15px;
    }
</style>
```
