# Syncfusion Blazor Dashboard Layout API Reference

## Overview

The `SfDashboardLayout` component is a grid-structured layout component that creates flexible dashboards with draggable, resizable, and removable panels. This API reference provides comprehensive documentation for all properties, methods, and events of the Dashboard Layout component and its related classes.

---

## Table of Contents

- [SfDashboardLayout Component](#sfdasboardlayout-component)
  - [Properties](#properties)
  - [Methods](#methods)
  - [Events](#events)
- [DashboardLayoutPanel Component](#dashboardlayoutpanel-component)
- [PanelModel Class](#panelmodel-class)
- [Event Arguments](#event-arguments)
- [Enumerations](#enumerations)
- [Usage Examples](#usage-examples)
- [Common Patterns](#common-patterns)

---

## SfDashboardLayout Component

### Properties

| Property | Type | Default | Accepted Values | Description | Reference |
|----------|------|---------|-----------------|-------------|-----------|
| `AllowDragging` | `bool` | `true` | `true`, `false` | Specifies whether panels can be reordered through dragging | [view](drag-drop-functionality.md#drag-and-drop-functionality) |
| `AllowFloating` | `bool` | `true` | `true`, `false` | Specifies whether panels fill available cells while dragging or resizing | [view](resizing-floating-panels.md#floating-panels) |
| `AllowResizing` | `bool` | `false` | `true`, `false` | Specifies whether panels can be resized | [view](resizing-floating-panels.md#panel-resizing) |
| `CellAspectRatio` | `double` | `1` | Any positive number | Defines the cell aspect ratio of the panel | [view](grid-configuration-styling.md#cell-aspect-ratio) |
| `CellSpacing` | `double[]` | `{5, 5}` | Array of doubles | Defines the spacing between panels (horizontal, vertical) | [view](grid-configuration-styling.md#cell-spacing) |
| `Columns` | `int` | `1` | Any positive integer | Determines the number of columns in the layout | [view](grid-configuration-styling.md#configuring-columns) |
| `DraggableHandle` | `string` | `null` | CSS selector string | CSS selector for the draggable handle element | [view](drag-drop-functionality.md#customizing-drag-handles) |
| `EnablePersistence` | `bool` | `false` | `true`, `false` | Persists the component state between page reloads | [view](state-persistence.md#enabling-persistence) |
| `EnableRtl` | `bool` | `false` | `true`, `false` | Enables right-to-left direction for the component | [view](#enablertl-property) |
| `ID` | `string` | `null` | Any string | ID attribute for the Dashboard Layout element | [view](#id-property) |
| `IsAddPanelCalled` | `bool` | `false` | `true`, `false` | Protected property to track if AddPanelAsync was called | [view](#isaddpanelcalled-property) |
| `MediaQuery` | `string` | `max-width:600px` | Valid CSS media query | Media query for responsive stacked layout | [view](responsive-adaptive-design.md#custom-breakpoints) |
| `ResizableHandles` | `ResizableHandle` | `SouthEast` | `East`, `West`, `North`, `South`, `NorthEast`, `NorthWest`, `SouthEast`, `SouthWest` | Specifies the resize handle directions | [view](resizing-floating-panels.md#resizablehandles-property) |
| `ShowGridLines` | `bool` | `false` | `true`, `false` | Shows grid lines for design visualization | [view](grid-configuration-styling.md#visualizing-grid-lines) |

---

### Methods

| Method | Parameters | Return Type | Description | Reference |
|--------|------------|-------------|-------------|-----------|
| `AddPanelAsync()` | `PanelModel panel` | `Task` | Adds a new panel to the Dashboard Layout | [view](#addpanelasync-method) |
| `GetPersistDataAsync()` | None | `Task<string>` | Retrieves persisted state as a string | [view](state-persistence.md#getpersistdataasync) |
| `MovePanelAsync()` | `string idValue, int rowValue, int colValue` | `Task` | Moves a panel to specified row and column | [view](#movepanelasync-method) |
| `RefreshAsync()` | None | `Task` | Updates and refreshes the Dashboard Layout | [view](#refreshasync-method) |
| `RemoveAllAsync()` | None | `Task` | Removes all panels from the Dashboard Layout | [view](#removeallasync-method) |
| `RemovePanelAsync()` | `string idValue` | `Task` | Removes a specific panel by ID | [view](#removepanelasync-method) |
| `ResetPersistDataAsync()` | None | `Task` | Resets the state to original configuration | [view](state-persistence.md#resetpersistdataasync) |
| `ResizePanelAsync()` | `string idValue, int sizeXValue, int sizeYValue` | `Task` | Resizes a panel to specified dimensions | [view](#resizepanelasync-method) |
| `Serialize()` | None | `Task<List<PanelModel>>` | Retrieves all panels as PanelModel collection | [view](#serialize-method) |
| `SetPersistDataAsync()` | `string properties` | `Task` | Loads previously saved state | [view](state-persistence.md#setpersistdataasync) |

---

### Events

| Event | Arguments | Description | Reference |
|-------|-----------|-------------|-----------|
| `Changed` | `ChangeEventArgs` | Raised when panel positions change | [view](#changeeventargs) |
| `Created` | `object` | Raised when the Dashboard Layout is created | [view](#createdeventargs) |
| `Destroyed` | `object` | Raised when the Dashboard Layout is destroyed | [view](#destroyedeventargs) |
| `OnDragStart` | `DragStartArgs` | Raised when a panel starts to drag | [view](#dragstartargs) |
| `OnDragStop` | `DragStopArgs` | Raised when a dragged panel is dropped | [view](#dragstopargs) |
| `OnResizeStart` | `ResizeArgs` | Raised when a panel starts resizing | [view](#resizeargs) |
| `OnResizeStop` | `ResizeArgs` | Raised when panel resizing ends | [view](#resizeargs) |
| `OnWindowResize` | `ResizeArgs` | Raised when the window is resized | [view](#resizeargs) |

---

## DashboardLayoutPanel Component

### Properties

| Property | Type | Default | Accepted Values | Description | Reference |
|----------|------|---------|-----------------|-------------|-----------|
| `AllowDragging` | `bool` | `true` | `true`, `false` | Allow/prevent dragging for this individual panel | [view](#allowdragging-property) |
| `Column` | `int` | `0` | Non-negative integer | Column position of the panel in the grid | [view](panel-positioning-sizing.md#row-and-column-properties) |
| `Content` | `RenderFragment` | `null` | Razor markup | Direct content without template wrapper | [view](#content-property) |
| `ContentTemplate` | `RenderFragment` | `null` | Razor markup | Template for panel content area | [view](panel-templates-headers.md#contenttemplate-usage) |
| `CssClass` | `string` | Empty | CSS class names | Custom CSS classes for panel styling | [view](panel-templates-headers.md#css-class-property) |
| `Enabled` | `bool` | `true` | `true`, `false` | Enable or disable the panel | [view](#enabled-property) |
| `Header` | `RenderFragment` | `null` | Razor markup | Direct header content | [view](panel-templates-headers.md#headertemplate-usage) |
| `HeaderTemplate` | `RenderFragment` | `null` | Razor markup | Template for panel header area | [view](panel-templates-headers.md#headertemplate-usage) |
| `Id` | `string` | Auto-generated | Unique string identifier | Unique identifier for the panel | [view](panel-positioning-sizing.md#panel-identification) |
| `MaxSizeX` | `int?` | `null` | Positive integer or null | Maximum width in cells count | [view](panel-positioning-sizing.md#minimum-and-maximum-sizes) |
| `MaxSizeY` | `int?` | `null` | Positive integer or null | Maximum height in cells count | [view](panel-positioning-sizing.md#minimum-and-maximum-sizes) |
| `MinSizeX` | `int` | `1` | Positive integer | Minimum width in cells count | [view](panel-positioning-sizing.md#minimum-and-maximum-sizes) |
| `MinSizeY` | `int` | `1` | Positive integer | Minimum height in cells count | [view](panel-positioning-sizing.md#minimum-and-maximum-sizes) |
| `Row` | `int` | `0` | Non-negative integer | Row position of the panel in the grid | [view](panel-positioning-sizing.md#row-and-column-properties) |
| `SizeX` | `int` | `1` | Positive integer | Width of the panel in cells count | [view](panel-positioning-sizing.md#sizex-and-sizey-properties) |
| `SizeY` | `int` | `1` | Positive integer | Height of the panel in cells count | [view](panel-positioning-sizing.md#sizex-and-sizey-properties) |
| `ZIndex` | `double` | `1000` | Any positive number | Z-index for panel layering | [view](#zindex-property) |

---

## DashboardLayoutPanel - Additional Property Examples

### Content Property

Direct content assignment alternative to ContentTemplate (simpler use cases):

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout Columns="4" CellSpacing="@(new double[]{10, 10})">
    <DashboardLayoutPanels>
        <!-- Using Content property for simple string content -->
        <DashboardLayoutPanel Id="panel-1">
            <Content>Simple panel content</Content>
        </DashboardLayoutPanel>

        <!-- Using ContentTemplate for complex HTML -->
        <DashboardLayoutPanel Id="panel-2" Column="1">
            <ContentTemplate>
                <div>
                    <p>Complex content with</p>
                    <ul>
                        <li>Multiple elements</li>
                        <li>And formatting</li>
                    </ul>
                </div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Using Header property -->
        <DashboardLayoutPanel Id="panel-3" Column="2">
            <Header>Panel Title</Header>
            <Content>Simple header with content</Content>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>
```

**Usage Pattern:**
- Use `Content` for static text or simple markup
- Use `ContentTemplate` for data binding and complex layouts
- Both cannot be used together in same panel

### AllowDragging Property

Control whether individual panels can be dragged (panel-level override of component setting):

```csharp
@using Syncfusion.Blazor.Layouts
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="ToggleDragging">Toggle Panel 2 Dragging</SfButton>

<SfDashboardLayout Columns="4" CellSpacing="@(new double[]{10, 10})" AllowDragging="true">
    <DashboardLayoutPanels>
        <!-- Draggable panel - can be moved around -->
        <DashboardLayoutPanel Id="panel-1" AllowDragging="true">
            <HeaderTemplate><div>Panel 1 (Draggable)</div></HeaderTemplate>
            <ContentTemplate><div>You can drag this panel</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Panel with dynamic dragging control -->
        <DashboardLayoutPanel Id="panel-2" Column="1" AllowDragging="@allowDragPanel2">
            <HeaderTemplate><div>@(allowDragPanel2 ? "Panel 2 (Draggable)" : "Panel 2 (Locked)")</div></HeaderTemplate>
            <ContentTemplate><div>@(allowDragPanel2 ? "You can drag this panel" : "Dragging is disabled")</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Non-draggable panel - acts as anchor -->
        <DashboardLayoutPanel Id="panel-3" Column="2" AllowDragging="false">
            <HeaderTemplate><div>Panel 3 (Locked)</div></HeaderTemplate>
            <ContentTemplate><div>This panel cannot be dragged</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Panel 4 with conditional dragging -->
        <DashboardLayoutPanel Id="panel-4" Column="3" AllowDragging="@userCanDrag">
            <HeaderTemplate><div>Panel 4 (Permission-based)</div></HeaderTemplate>
            <ContentTemplate><div>@(userCanDrag ? "You have permission to drag" : "Insufficient permissions")</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    private bool allowDragPanel2 = true;
    private bool userCanDrag = true;

    private void ToggleDragging()
    {
        allowDragPanel2 = !allowDragPanel2;
    }
}
```

**Behavior:**
- `AllowDragging="true"`: Panel can be dragged to new positions (respects component AllowDragging setting)
- `AllowDragging="false"`: Panel cannot be dragged, acts as a fixed anchor point
- Overrides component-level AllowDragging property at panel level
- Useful for protecting important panels from accidental movement
- Combines with component-level setting using AND logic (both must be true to allow drag)

### Enabled Property

Control whether a panel is interactive and visible:

```csharp
@using Syncfusion.Blazor.Layouts
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="TogglePanels">Toggle Panel States</SfButton>

<SfDashboardLayout Columns="4" CellSpacing="@(new double[]{10, 10})">
    <DashboardLayoutPanels>
        <!-- Enabled panel - fully interactive -->
        <DashboardLayoutPanel Id="panel-1" Enabled="true">
            <HeaderTemplate><div>Active Panel</div></HeaderTemplate>
            <ContentTemplate><div>This panel is enabled and interactive</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Disabled panel - grayed out, non-interactive -->
        <DashboardLayoutPanel Id="panel-2" Column="1" Enabled="@isPanelEnabled">
            <HeaderTemplate><div>@(isPanelEnabled ? "Enabled" : "Disabled") Panel</div></HeaderTemplate>
            <ContentTemplate><div>@(isPanelEnabled ? "Interact with this panel" : "This panel is disabled")</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Dynamic enable/disable based on condition -->
        <DashboardLayoutPanel Id="panel-3" Column="2" Enabled="@userHasPermission">
            <HeaderTemplate><div>Conditional Panel</div></HeaderTemplate>
            <ContentTemplate><div>Only visible to authorized users</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    private bool isPanelEnabled = false;
    private bool userHasPermission = true;

    private void TogglePanels()
    {
        isPanelEnabled = !isPanelEnabled;
    }
}
```

**Behavior:**
- `Enabled="true"`: Panel appears normally and supports all interactions (dragging, resizing, etc.)
- `Enabled="false"`: Panel appears with reduced opacity, interactions are disabled
- Useful for conditional UI based on permissions or application state

### ZIndex Property

Control the stacking order of panels when they overlap:

```csharp
@using Syncfusion.Blazor.Layouts
@using Syncfusion.Blazor.Buttons

<div>
    <SfButton OnClick="BringToFront">Bring to Front</SfButton>
    <SfButton OnClick="SendToBack">Send to Back</SfButton>
</div>

<SfDashboardLayout AllowFloating="true" Columns="4" CellSpacing="@(new double[]{10, 10})">
    <DashboardLayoutPanels>
        <!-- Default panel (ZIndex = 1000) -->
        <DashboardLayoutPanel Id="panel-1" SizeX="2" SizeY="2">
            <HeaderTemplate><div>Panel 1 (Z: 1000)</div></HeaderTemplate>
            <ContentTemplate><div>Overlapping content</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Higher panel appears on top (ZIndex = 1100) -->
        <DashboardLayoutPanel Id="panel-2" ZIndex="1100" SizeX="2" SizeY="2" Column="1">
            <HeaderTemplate><div>Panel 2 (Z: 1100) - On Top</div></HeaderTemplate>
            <ContentTemplate><div>This panel appears above panel-1</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Lower panel appears below (ZIndex = 900) -->
        <DashboardLayoutPanel Id="panel-3" ZIndex="900" SizeX="2" SizeY="2" Column="2">
            <HeaderTemplate><div>Panel 3 (Z: 900) - Behind</div></HeaderTemplate>
            <ContentTemplate><div>This panel appears below panel-1</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    private SfDashboardLayout dashboard;
    private double panel1ZIndex = 1000;

    private void BringToFront()
    {
        panel1ZIndex = 1200;  // Bring panel-1 to front
    }

    private void SendToBack()
    {
        panel1ZIndex = 800;  // Send panel-1 to back
    }
}
```

**ZIndex Behavior:**
- Default value is 1000
- Higher values appear on top
- Lower values appear below
- Useful when AllowFloating="true" creates overlaps
- Affects stacking order in visual hierarchy

---

## PanelModel Class

### Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `AllowDragging` | `bool` | `true` | Allow/prevent dragging for the panel |
| `Column` | `int` | `0` | Column position of the panel |
| `Content` | `RenderFragment` | `null` | Panel content fragment |
| `CssClass` | `string` | Empty | CSS class names for styling |
| `Enabled` | `bool` | `true` | Panel enabled state |
| `Header` | `RenderFragment` | `null` | Panel header fragment |
| `Id` | `string` | Empty | Unique panel identifier |
| `MaxSizeX` | `int?` | `null` | Maximum width in cells |
| `MaxSizeY` | `int?` | `null` | Maximum height in cells |
| `MinSizeX` | `int` | `1` | Minimum width in cells |
| `MinSizeY` | `int` | `1` | Minimum height in cells |
| `Row` | `int` | `0` | Row position of the panel |
| `SizeX` | `int` | `1` | Width in cells count |
| `SizeY` | `int` | `1` | Height in cells count |
| `ZIndex` | `double` | `1000` | Z-index for layering |

---

## Event Arguments

### ChangeEventArgs

Raised when panel positions or collection changes.

| Property | Type | Description |
|----------|------|-------------|
| `AddedPanels` | `List<PanelModel>` | Panels added to the layout |
| `ChangedPanels` | `List<PanelModel>` | Panels with position changes |
| `RemovedPanels` | `List<PanelModel>` | Panels removed from the layout |
| `IsInteracted` | `bool` | True if triggered by user interaction |

**Usage Example:**
```csharp
<DashboardLayoutEvents Changed="@OnChanged" />

@code {
    private void OnChanged(ChangeEventArgs args)
    {
        if (args.IsInteracted)
        {
            Console.WriteLine($"Changed panels: {args.ChangedPanels.Count}");
            Console.WriteLine($"Added panels: {args.AddedPanels.Count}");
            Console.WriteLine($"Removed panels: {args.RemovedPanels.Count}");
        }
    }
}
```

---

### DragStartArgs

Raised when a panel drag operation begins.

| Property | Type | Description |
|----------|------|-------------|
| `Cancel` | `bool` | Set to true to prevent the drag action |
| `Element` | `ElementReference` | Reference to the DOM element being dragged |
| `Id` | `string` | ID of the panel being dragged |

**Usage Example:**
```csharp
<DashboardLayoutEvents OnDragStart="@OnDragStart" />

@code {
    private void OnDragStart(DragStartArgs args)
    {
        if (args.Id == "protected-panel")
        {
            args.Cancel = true;  // Prevent dragging
        }
    }
}
```

---

### DragStopArgs

Raised when a dragged panel is dropped.

| Property | Type | Description |
|----------|------|-------------|
| `Element` | `ElementReference` | Reference to the DOM element that was dragged |
| `Id` | `string` | ID of the dropped panel |

**Usage Example:**
```csharp
<DashboardLayoutEvents OnDragStop="@OnDragStop" />

@code {
    private void OnDragStop(DragStopArgs args)
    {
        Console.WriteLine($"Panel {args.Id} was dropped");
    }
}
```

---

### ResizeArgs

Raised during resize operations (start and stop).

| Property | Type | Description |
|----------|------|-------------|
| `Element` | `ElementReference` | Reference to the DOM element being resized |
| `Id` | `string` | ID of the resizing panel |
| `IsInteracted` | `bool` | True if triggered by user interaction |
| `Name` | `string` | Event name (OnResizeStart or OnResizeStop) |

**Usage Example:**
```csharp
<DashboardLayoutEvents OnResizeStart="@OnResizeStart" OnResizeStop="@OnResizeStop" />

@code {
    private void OnResizeStart(ResizeArgs args)
    {
        Console.WriteLine($"Resizing panel {args.Id}");
    }

    private void OnResizeStop(ResizeArgs args)
    {
        if (args.IsInteracted)
        {
            Console.WriteLine($"Panel {args.Id} resize completed");
        }
    }
}
```

---

### CreatedEventArgs

Raised when the Dashboard Layout component is fully created and initialized.

**Usage Example:**
```csharp
<DashboardLayoutEvents Created="@OnCreated" />

@code {
    private void OnCreated(object args)
    {
        Console.WriteLine("Dashboard Layout component has been created and initialized");
        // Initialize related components or load dashboard state
    }
}
```

---

### DestroyedEventArgs

Raised when the Dashboard Layout component is destroyed or removed from the DOM.

**Usage Example:**
```csharp
<DashboardLayoutEvents Destroyed="@OnDestroyed" />

@code {
    private void OnDestroyed(object args)
    {
        Console.WriteLine("Dashboard Layout component is being destroyed");
        // Cleanup resources, save state, or unbind event handlers
    }
}
```

---

## Enumerations

### ResizableHandle

Specifies the resize handle directions for panels.

| Value | Description |
|-------|-------------|
| `East` | East (right) direction |
| `West` | West (left) direction |
| `North` | North (top) direction |
| `South` | South (bottom) direction |
| `NorthEast` | North-East (top-right) direction |
| `NorthWest` | North-West (top-left) direction |
| `SouthEast` | South-East (bottom-right) direction |
| `SouthWest` | South-West (bottom-left) direction |

**Usage Example:**
```csharp
<!-- Single direction resize -->
<SfDashboardLayout AllowResizing="true" ResizableHandles="ResizableHandle.SouthEast">
    ...
</SfDashboardLayout>

<!-- Multiple directions (using bitwise OR) -->
<SfDashboardLayout AllowResizing="true" ResizableHandles="ResizableHandle.East | ResizableHandle.South">
    ...
</SfDashboardLayout>
```

---

## Usage Examples

### EnableRtl Property

Enables right-to-left (RTL) text direction for the Dashboard Layout component, useful for languages like Arabic, Hebrew, and Persian.

```csharp
@using Syncfusion.Blazor.Layouts

<!-- Enable RTL language support -->
<SfDashboardLayout ID="dashboard" 
                   Columns="4" 
                   EnableRtl="true"
                   CellSpacing="@(new double[]{10, 10})">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <HeaderTemplate><div>لوحة 1</div></HeaderTemplate>
            <ContentTemplate><div>محتوى اللوحة</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Column="1">
            <HeaderTemplate><div>لوحة 2</div></HeaderTemplate>
            <ContentTemplate><div>محتوى اللوحة</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    /* RTL-specific styling */
    .e-dashboardlayout.e-rtl {
        direction: rtl;
    }
</style>
```

### ID Property

Provides a unique identifier for the Dashboard Layout element, essential for programmatic access and event binding.

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout @ref="dashboardObject" 
                   ID="myDashboard"
                   Columns="4">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel Id="panel-1">
            <HeaderTemplate><div>Statistics</div></HeaderTemplate>
            <ContentTemplate><div>Dashboard Content</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    SfDashboardLayout dashboardObject;

    private async Task RefreshDashboard()
    {
        // Access dashboard using the ID
        if (dashboardObject != null)
        {
            // Perform operations on the dashboard
            await dashboardObject.RefreshAsync();
        }
    }
}
```

### IsAddPanelCalled Property

A protected property that tracks whether `AddPanelAsync` has been called. Useful for determining if dynamic panels have been added programmatically.

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout @ref="dashboardObject" 
                   ID="dashboard"
                   Columns="4">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <HeaderTemplate><div>Initial Panel</div></HeaderTemplate>
            <ContentTemplate><div>Static Content</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<button @onclick="AddDynamicPanel">Add Panel</button>
<p>Panels added dynamically: @panelAdditionStatus</p>

@code {
    SfDashboardLayout dashboardObject;
    private string panelAdditionStatus = "None";

    private async Task AddDynamicPanel()
    {
        var newPanel = new PanelModel
        {
            Id = "dynamic-panel",
            Row = 1,
            Column = 0,
            SizeX = 2,
            SizeY = 1
        };

        // Add panel dynamically
        await dashboardObject.AddPanelAsync(newPanel);

        // Check if AddPanelAsync was called by examining the internal state
        // This helps track dynamic vs. static panel creation
        panelAdditionStatus = "Yes - Dynamic panels added";
    }
}
```

---

## Methods Examples

### AddPanelAsync() Method

Adds a new panel to the Dashboard Layout dynamically at runtime.

```csharp
@using Syncfusion.Blazor.Layouts

<SfButton OnClick="AddNewPanel">Add Panel</SfButton>

<SfDashboardLayout @ref="dashboard" Columns="4">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel Id="panel-1">
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
            <ContentTemplate><div>Content</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    SfDashboardLayout dashboard;
    private int count = 1;

    private async Task AddNewPanel()
    {
        count++;
        var newPanel = new PanelModel { Id = $"panel-{count}", Row = count - 1, Column = 0, SizeX = 2, SizeY = 1 };
        await dashboard.AddPanelAsync(newPanel);
    }
}
```

### MovePanelAsync() Method

Moves a specific panel to a new row and column position.

```csharp
@using Syncfusion.Blazor.Layouts

<SfButton OnClick="MovePanel">Move Panel 1</SfButton>

<SfDashboardLayout @ref="dashboard" Columns="4">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel Id="panel-1" Row="0" Column="0" SizeX="2">
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
            <ContentTemplate><div>Content</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    SfDashboardLayout dashboard;

    private async Task MovePanel()
    {
        await dashboard.MovePanelAsync("panel-1", 1, 1);  // Move to row 1, column 1
    }
}
```

### RefreshAsync() Method

Updates and refreshes the Dashboard Layout component.

```csharp
@using Syncfusion.Blazor.Layouts

<SfButton OnClick="RefreshLayout">Refresh</SfButton>

<SfDashboardLayout @ref="dashboard" Columns="@columnCount">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel Id="panel-1">
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
            <ContentTemplate><div>Content</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    SfDashboardLayout dashboard;
    private int columnCount = 4;

    private async Task RefreshLayout()
    {
        columnCount = 6;
        await Task.Delay(100);
        await dashboard.RefreshAsync();
    }
}
```

### RemoveAllAsync() Method

Removes all panels from the Dashboard Layout.

```csharp
@using Syncfusion.Blazor.Layouts

<SfButton OnClick="ClearAll">Remove All Panels</SfButton>

<SfDashboardLayout @ref="dashboard" Columns="4">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel Id="panel-1">
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
            <ContentTemplate><div>Content</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    SfDashboardLayout dashboard;

    private async Task ClearAll()
    {
        await dashboard.RemoveAllAsync();
    }
}
```

### RemovePanelAsync() Method

Removes a specific panel by its ID.

```csharp
@using Syncfusion.Blazor.Layouts

<SfButton OnClick="RemovePanel">Remove Panel 2</SfButton>

<SfDashboardLayout @ref="dashboard" Columns="4">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel Id="panel-1">
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Id="panel-2" Column="1">
            <HeaderTemplate><div>Panel 2</div></HeaderTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    SfDashboardLayout dashboard;

    private async Task RemovePanel()
    {
        await dashboard.RemovePanelAsync("panel-2");
    }
}
```

### ResizePanelAsync() Method

Resizes a specific panel to new dimensions.

```csharp
@using Syncfusion.Blazor.Layouts

<SfButton OnClick="ResizePanel">Resize Panel 1 to 3x2</SfButton>

<SfDashboardLayout @ref="dashboard" Columns="4" AllowResizing="true">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel Id="panel-1" SizeX="2" SizeY="1" MaxSizeX="4" MaxSizeY="2">
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
            <ContentTemplate><div>Content</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    SfDashboardLayout dashboard;

    private async Task ResizePanel()
    {
        await dashboard.ResizePanelAsync("panel-1", 3, 2);
    }
}
```

### Serialize() Method

Retrieves all panels as a collection of PanelModel objects.

```csharp
@using Syncfusion.Blazor.Layouts

<SfButton OnClick="GetPanels">Get Panel Info</SfButton>

<SfDashboardLayout @ref="dashboard" Columns="4">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel Id="panel-1" SizeX="2">
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    SfDashboardLayout dashboard;

    private async Task GetPanels()
    {
        var panels = (List<PanelModel>)await dashboard.Serialize();
        foreach (var panel in panels)
            Console.WriteLine($"Panel {panel.Id}: {panel.SizeX}x{panel.SizeY}");
    }
}
```

---

> **Note:** For comprehensive examples on `GetPersistDataAsync()`, `SetPersistDataAsync()`, and `ResetPersistDataAsync()` persistence methods, refer to the [State Persistence](state-persistence.md#programmatic-methods) guide, which contains detailed implementation patterns and best practices.

---

## Complete Component Usage Example

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout @ref="dashboardObject"
                   ID="dashboard"
                   Columns="4"
                   CellSpacing="@(new double[]{10, 10})"
                   CellAspectRatio="1.5"
                   AllowDragging="true"
                   AllowResizing="true"
                   AllowFloating="true"
                   ResizableHandles="ResizableHandle.SouthEast"
                   ShowGridLines="false"
                   EnablePersistence="true"
                   MediaQuery="max-width:768px">
    
    <DashboardLayoutEvents Changed="@OnChanged"
                          Created="@OnCreated"
                          OnDragStart="@OnDragStart"
                          OnDragStop="@OnDragStop"
                          OnResizeStart="@OnResizeStart"
                          OnResizeStop="@OnResizeStop" />
    
    <DashboardLayoutPanels>
        <DashboardLayoutPanel Id="panel-1"
                            Row="0"
                            Column="0"
                            SizeX="2"
                            SizeY="1"
                            MinSizeX="1"
                            MaxSizeX="4"
                            AllowDragging="true">
            <HeaderTemplate>
                <span>Panel 1</span>
            </HeaderTemplate>
            <ContentTemplate>
                <div>Panel content goes here</div>
            </ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    SfDashboardLayout dashboardObject;

    private void OnChanged(ChangeEventArgs args)
    {
        Console.WriteLine("Dashboard changed");
    }

    private void OnCreated(object args)
    {
        Console.WriteLine("Dashboard created");
    }

    private void OnDragStart(DragStartArgs args)
    {
        Console.WriteLine($"Dragging panel: {args.Id}");
    }

    private void OnDragStop(DragStopArgs args)
    {
        Console.WriteLine($"Dropped panel: {args.Id}");
    }

    private void OnResizeStart(ResizeArgs args)
    {
        Console.WriteLine($"Resizing panel: {args.Id}");
    }

    private void OnResizeStop(ResizeArgs args)
    {
        Console.WriteLine($"Resize complete: {args.Id}");
    }
}
```

---

## Common Patterns

### Dynamic Panel Operations

```csharp
// Add a panel programmatically
var newPanel = new PanelModel
{
    Id = "new-panel",
    Row = 0,
    Column = 0,
    SizeX = 2,
    SizeY = 1
};
await dashboardObject.AddPanelAsync(newPanel);

// Move a panel to new position
await dashboardObject.MovePanelAsync("panel-1", 1, 2);

// Resize a panel
await dashboardObject.ResizePanelAsync("panel-1", 3, 2);

// Remove a specific panel
await dashboardObject.RemovePanelAsync("panel-1");

// Remove all panels
await dashboardObject.RemoveAllAsync();
```

### State Management

```csharp
// Get current state
var state = await dashboardObject.GetPersistDataAsync();
await localStorage.SetItemAsync("dashboard-state", state);

// Restore state
var savedState = await localStorage.GetItemAsync("dashboard-state");
await dashboardObject.SetPersistDataAsync(savedState);

// Reset to original state
await dashboardObject.ResetPersistDataAsync();

// Get all panels as collection
var panels = await dashboardObject.Serialize();
```

### Conditional Event Handling

```csharp
<DashboardLayoutEvents OnDragStart="@PreventSpecificPanelDrag" />

@code {
    private void PreventSpecificPanelDrag(DragStartArgs args)
    {
        // Allow dragging only for specific panels
        var protectedPanels = new[] { "locked-panel", "header-panel" };
        if (protectedPanels.Contains(args.Id))
        {
            args.Cancel = true;
        }
    }
}
```

---

