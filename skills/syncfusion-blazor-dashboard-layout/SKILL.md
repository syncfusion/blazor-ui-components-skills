---
name: syncfusion-blazor-dashboard-layout
description: Guide for implementing Syncfusion Blazor Dashboard Layout component. Use this when building dashboard interfaces with draggable/resizable panels, state persistence, or responsive grid arrangements. Covers panel positioning, sizing constraints, drag-drop rearrangement, and adaptive layouts.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
  category: "Layouts"
---

# Implementing Dashboard Layout with Syncfusion Blazor

The Syncfusion Blazor Dashboard Layout component provides a comprehensive solution for building responsive, interactive dashboards. This skill guides you through creating flexible panel-based layouts with advanced features including drag-and-drop, resizing, state persistence, and adaptive responsive design.

## When to Use This Skill

Use this skill when you need to:
- Create dashboard or grid-based panel layouts
- Enable users to rearrange panels through drag-and-drop
- Allow panel resizing with size constraints
- Build responsive layouts that adapt to different screen sizes
- Save and restore user layout configurations
- Create complex multi-panel displays with headers and dynamic content
- Customize panel styling and appearance
- Prevent panel overlapping in dynamic scenarios

## Component Overview

**SfDashboardLayout** creates a grid-based dashboard where panels represent resizable, draggable containers organized on a configurable grid. Key capabilities:

- **Grid-based positioning** using Row and Column properties
- **Flexible sizing** with width (SizeX) and height (SizeY) in cell units
- **Size constraints** with MinSizeX, MaxSizeX, MinSizeY, MaxSizeY properties
- **Drag-and-drop** with automatic collision handling and panel pushing
- **Resizing** with customizable resize handles in multiple directions
- **Floating panels** that automatically move up to fill empty spaces
- **State persistence** to localStorage with programmatic save/load/reset
- **Templates** for headers and dynamic content
- **Responsive design** with media query breakpoints for mobile adaptation
- **Cell-based grid** with configurable columns, cell aspect ratio, and spacing

## Documentation and Navigation Guide

**Getting Started**
📄 [references/getting-started.md](references/getting-started.md)
- Setting up Dashboard Layout in WebAssembly and Server apps
- NuGet package installation (Syncfusion.Blazor.Layouts, Syncfusion.Blazor.Themes)
- Configuring namespaces and Syncfusion services
- Adding stylesheet and script resources
- Creating first dashboard with basic panels

**Core Layout Configuration**
📄 [references/panel-positioning-sizing.md](references/panel-positioning-sizing.md)
- Panel Row and Column positioning
- Setting panel dimensions with SizeX and SizeY
- Min/Max size constraints (MinSizeX, MaxSizeX, MinSizeY, MaxSizeY)
- Panel ID management and unique identification
- Understanding cell-based grid system

**Interactive Features**
📄 [references/drag-drop-functionality.md](references/drag-drop-functionality.md)
- Enabling drag-and-drop rearrangement
- Automatic collision detection and panel pushing
- Customizing drag handles with DraggableHandle property
- Using CSS selectors for drag-handle targeting
- Placeholder visualization during drag operations

📄 [references/resizing-floating-panels.md](references/resizing-floating-panels.md)
- Enabling panel resizing with AllowResizing property
- Customizing resize directions with ResizableHandles property
- Floating panels with AllowFloating to utilize empty space
- Floating behavior and automatic upward panel movement

**Grid Configuration & Styling**
📄 [references/grid-configuration-styling.md](references/grid-configuration-styling.md)
- Configuring grid with Columns property
- Cell aspect ratio with CellAspectRatio property
- Cell spacing with CellSpacing property (row and column gaps)
- Visualizing grid with ShowGridLines property
- CSS customization of panel headers, content, and backgrounds
- CSS classes: .e-panel-header, .e-panel-content, .e-resize

**Responsive & Adaptive Design**
📄 [references/responsive-adaptive-design.md](references/responsive-adaptive-design.md)
- Built-in responsive support for different screen sizes
- MediaQuery property for custom breakpoints
- Automatic stacking layout at low resolutions (default 600px)
- Mobile-friendly configurations
- Responsive behavior without manual configuration

**Panel Templates & Headers**
📄 [references/panel-templates-headers.md](references/panel-templates-headers.md)
- HeaderTemplate for panel titles and headers
- ContentTemplate for panel main content
- CssClass property for custom styling
- Rendering HTML and components within panels
- Empty space rendering prevention

**State Management & Persistence**
📄 [references/state-persistence.md](references/state-persistence.md)
- Enabling persistence with EnablePersistence property
- localStorage integration for automatic save/restore
- GetPersistDataAsync for retrieving current state as string
- SetPersistDataAsync for restoring saved state
- ResetPersistDataAsync for clearing saved state
- Component ID requirement for persistence

**API Reference**
📄 [references/api-reference.md](references/api-reference.md)
- Complete SfDashboardLayout component API documentation
- All 14 properties with types, defaults, and accepted values
- All 11 methods with parameters, return types, and descriptions
- All 8 events with argument types and usage examples
- DashboardLayoutPanel component properties (17 total)
- PanelModel class properties for programmatic panel data
- Event argument classes with complete examples
- ResizableHandle enumeration with all 8 direction values
- Working code examples for every property and method
- Cross-references to related documentation

**Advanced Scenarios & Troubleshooting**
📄 [references/advanced-scenarios.md](references/advanced-scenarios.md)
- Preventing panel overlap with unique IDs in dynamic rendering
- All panels at same position configuration
- Performance optimization for complex layouts
- Complex multi-panel display patterns
- Troubleshooting common issues and edge cases

## Quick Start Example

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout CellSpacing="@(new double[]{10, 10})" Columns="5">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
            <ContentTemplate><div>Your content here</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeX=2 SizeY=2 Column=1>
            <HeaderTemplate><div>Panel 2</div></HeaderTemplate>
            <ContentTemplate><div>Your content here</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeY=2 Column=3>
            <HeaderTemplate><div>Panel 3</div></HeaderTemplate>
            <ContentTemplate><div>Your content here</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .e-panel-header {
        background-color: rgba(0, 0, 0, .1);
        text-align: center;
    }
    .e-panel-content {
        text-align: center;
        margin-top: 10px;
    }
</style>
```

## Common Patterns

**Basic 3-Panel Dashboard**
```csharp
<SfDashboardLayout CellSpacing="@(new double[]{10, 10})" Columns="6">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel SizeX=2>
            <HeaderTemplate><div>Metrics</div></HeaderTemplate>
            <ContentTemplate><div>250,000</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeX=2 Column=2>
            <HeaderTemplate><div>Users</div></HeaderTemplate>
            <ContentTemplate><div>45,200</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeX=2 Column=4>
            <HeaderTemplate><div>Revenue</div></HeaderTemplate>
            <ContentTemplate><div>$125,000</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>
```

**With Drag-Drop and Resizing**
```csharp
<SfDashboardLayout CellSpacing="@(new double[]{10, 10})" 
    Columns="6" 
    AllowDragging="true" 
    AllowResizing="true">
    <!-- Panel definitions -->
</SfDashboardLayout>
```

**Responsive Mobile Layout**
```csharp
<SfDashboardLayout CellSpacing="@(new double[]{10, 10})" 
    Columns="5" 
    MediaQuery="max-width:600px">
    <!-- Automatically stacks to single column below 600px -->
</SfDashboardLayout>
```

**State Persistence Pattern**
```csharp
<SfDashboardLayout ID="dashboard" EnablePersistence="true" CellSpacing="@(new double[]{10, 10})">
    <!-- Automatically saves to localStorage on changes -->
</SfDashboardLayout>
```

## Key Properties Reference

| Property | Purpose | Common Values |
|----------|---------|----------------|
| **Columns** | Number of grid cells per row | 4, 5, 6, 12 |
| **Row** | Panel vertical position (0-based) | 0, 1, 2, ... |
| **Column** | Panel horizontal position (0-based) | 0, 1, 2, ... |
| **SizeX** | Panel width in cells | 1, 2, 3, ... |
| **SizeY** | Panel height in cells | 1, 2, 3, ... |
| **MinSizeX/MinSizeY** | Minimum size constraints | 1, 2, ... |
| **MaxSizeX/MaxSizeY** | Maximum size constraints | null (unlimited) or specific value |
| **CellSpacing** | Gap between panels (row, column) | [10, 10], [20, 20] |
| **CellAspectRatio** | Height-to-width ratio | 1, 2, 0.5 |
| **AllowDragging** | Enable panel rearrangement | true, false |
| **AllowResizing** | Enable panel resizing | true, false |
| **AllowFloating** | Auto-fill empty spaces | true, false |
| **DraggableHandle** | CSS selector for drag area | ".e-panel-header" |
| **ResizableHandles** | Resize directions | "e" (east), "south-east", etc. |
| **MediaQuery** | Mobile breakpoint | "max-width:600px" |
| **EnablePersistence** | Save state to localStorage | true, false |
| **ID** | Unique component ID (required for persistence) | "dashboard", "layout1" |

## Use Case Examples

**Data Analytics Dashboard**
Use dashboard grid with Charts, Grids, and metrics panels. Enable resizing so users can focus on important metrics.

**Admin Control Panel**
Combine multiple Syncfusion components (grids, charts, forms) in panels. Persist layout so admin preferences are remembered.

**SEO Monitoring Dashboard**
Display analytics metrics and charts using dashboard grid. Use floating panels to automatically optimize space as panels are moved.

**Personal Workspace**
Let users customize their layout completely with drag-drop, resizing, and persistence to create their ideal workspace.

**Responsive Mobile Dashboard**
Design for desktop, automatically adapts to single-column stacked layout on mobile using MediaQuery breakpoints.
