# Grid Configuration and Styling

## Table of Contents
- [Grid Structure](#grid-structure)
- [Configuring Columns](#configuring-columns)
- [Cell Aspect Ratio](#cell-aspect-ratio)
- [Cell Spacing](#cell-spacing)
- [Visualizing Grid Lines](#visualizing-grid-lines)
- [CSS Customization](#css-customization)

## Grid Structure

The Dashboard Layout is built on a grid system divided into equally-sized cells. All positioning and sizing is based on these grid cells.

**Key Properties:**
- **Columns**: Number of cells per row
- **CellAspectRatio**: Height-to-width ratio of cells
- **CellSpacing**: Gap between cells
- **ShowGridLines**: Make grid visible for design

## Configuring Columns

### Columns Property

The `Columns` property specifies how many equal-width cells fit in each row:

```csharp
@using Syncfusion.Blazor.Layouts

<!-- 5 columns per row -->
<SfDashboardLayout Columns="5">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <ContentTemplate><div>Panel 1 (1 cell wide)</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Column=1>
            <ContentTemplate><div>Panel 2 (1 cell wide)</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Column=2>
            <ContentTemplate><div>Panel 3 (1 cell wide)</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Column=3>
            <ContentTemplate><div>Panel 4 (1 cell wide)</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Column=4>
            <ContentTemplate><div>Panel 5 (1 cell wide)</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .e-panel-content {
        text-align: center;
        padding: 10px;
    }
</style>
```

### Common Column Configurations

| Value | Layout | Use Case |
|-------|--------|----------|
| 1 | Single column (full width) | Mobile or stacked layouts |
| 2 | Two equal columns | 50/50 split |
| 3 | Three equal columns | Trisection layout |
| 4 | Four columns | 25% grid, 1/2/3 panel layouts |
| 5 | Five columns | Mixed 1/2/1.5 sizing |
| 6 | Six columns | 50%, 33%, 16.67% combinations |
| 12 | Twelve columns | Bootstrap-like system |

### Multi-Row Layouts

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout CellSpacing="@(new double[]{10, 10})" Columns="6">
    <DashboardLayoutPanels>
        <!-- Row 1: 3 panels of 2 cells each -->
        <DashboardLayoutPanel SizeX=2>
            <ContentTemplate><div>Panel 1 (2x1)</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeX=2 Column=2>
            <ContentTemplate><div>Panel 2 (2x1)</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeX=2 Column=4>
            <ContentTemplate><div>Panel 3 (2x1)</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Row 2: 2 panels -->
        <DashboardLayoutPanel SizeX=3 Row=1>
            <ContentTemplate><div>Panel 4 (3x1)</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeX=3 Row=1 Column=3>
            <ContentTemplate><div>Panel 5 (3x1)</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .e-panel-content {
        text-align: center;
        padding: 10px;
    }
</style>
```

## Cell Aspect Ratio

### CellAspectRatio Property

Controls the height-to-width ratio of grid cells. The aspect ratio is calculated as `width / height`.

```csharp
@using Syncfusion.Blazor.Layouts

<!-- CellAspectRatio = 2 means width is 2x the height -->
<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    CellAspectRatio="2" 
    Columns="5">
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

### AspectRatio Calculations

| Ratio | Formula | Result |
|-------|---------|--------|
| 1 | 100px width / 100px height | Square cells |
| 2 | 100px width / 50px height | Wide, short cells |
| 0.5 | 100px width / 200px height | Narrow, tall cells |
| 3 | 100px width / 33.33px height | Very wide, short |
| 0.75 | 100px width / 133.33px height | Slightly tall |

**Example: For 100px cell width:**
- AspectRatio 2 → 50px height
- AspectRatio 1 → 100px height
- AspectRatio 0.5 → 200px height

### Use Cases

**Wide Cells (AspectRatio > 1):** Charts, graphs, timeline visualizations
```csharp
<SfDashboardLayout CellAspectRatio="2.5" Columns="4">
    <!-- Cells are approximately 2.5x wider than tall -->
</SfDashboardLayout>
```

**Square Cells (AspectRatio = 1):** Even distribution, balanced panels
```csharp
<SfDashboardLayout CellAspectRatio="1" Columns="5">
    <!-- Square cells -->
</SfDashboardLayout>
```

**Tall Cells (AspectRatio < 1):** Text-heavy content, forms
```csharp
<SfDashboardLayout CellAspectRatio="0.5" Columns="3">
    <!-- Cells are taller than wide -->
</SfDashboardLayout>
```

## Cell Spacing

### CellSpacing Property

Defines the gap between panels in both horizontal and vertical directions.

```csharp
@using Syncfusion.Blazor.Layouts

<!-- [row spacing, column spacing] -->
<SfDashboardLayout 
    CellSpacing="@(new double[]{20, 20})" 
    Columns="5">
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

### Spacing Format

`CellSpacing` is a double array: `[verticalGap, horizontalGap]`

```csharp
// 10px spacing between all panels
CellSpacing="@(new double[]{10, 10})"

// 15px vertical, 5px horizontal spacing
CellSpacing="@(new double[]{15, 5})"

// Dense layout
CellSpacing="@(new double[]{5, 5})"

// Loose layout
CellSpacing="@(new double[]{25, 25})"
```

### Impact on Layout

| Spacing | Effect | Use Case |
|---------|--------|----------|
| [5, 5] | Compact, minimal gaps | Dense dashboards |
| [10, 10] | Standard, clean gaps | Most dashboards |
| [15, 15] | Spacious, readable | Data-heavy displays |
| [20, 20] | Very spacious, clear | Minimal content per panel |

## Visualizing Grid Lines

### ShowGridLines Property

Makes the underlying grid structure visible during design and layout development:

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    Columns="5" 
    ShowGridLines="true">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel Column=1>
            <ContentTemplate><div>0</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeX=1 SizeY=2 Column=2>
            <ContentTemplate><div>1</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeY=1 Column=3>
            <ContentTemplate><div>2</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Row=1 Column=3>
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

**Benefits of ShowGridLines:**
- Visualize grid cell divisions
- Debug panel positioning
- Verify Row/Column assignments
- Design layout more accurately
- Identify size misalignments

**Development Workflow:**
1. Enable ShowGridLines during design
2. Position and size panels visually
3. Verify Row/Column and SizeX/SizeY values
4. Disable ShowGridLines for production
5. Deploy final layout

## CSS Customization

### Customizing Panel Headers

```css
.e-dashboardlayout.e-control .e-panel .e-panel-container .e-panel-header {
    color: #754131;
    background-color: #c9e2f7;
    text-align: center;
}
```

### Customizing Panel Content

```css
.e-dashboardlayout.e-control .e-panel .e-panel-container .e-panel-content {
    background-color: #c9e2f7;
    padding: 50px;
}
```

### Customizing Resize Icon

```css
.e-dashboardlayout.e-control .e-panel .e-panel-container .e-resize.e-double {
    color: #0378d5;
    font-size: 30px;
    height: 20px;
    width: 20px;
}
```

### Customizing Background

```css
.e-dashboardlayout.e-control.e-responsive {
    background: #b3d3ed;
}
```

### Complete Styling Example

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    Columns="5">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <HeaderTemplate><div>Styled Panel</div></HeaderTemplate>
            <ContentTemplate><div>Content with custom styling</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    /* Dashboard background -->
    .e-dashboardlayout.e-control.e-responsive {
        background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
    }

    /* Panel headers -->
    .e-dashboardlayout.e-control .e-panel .e-panel-container .e-panel-header {
        background: linear-gradient(to right, #667eea 0%, #764ba2 100%);
        color: white;
        padding: 12px;
        font-weight: bold;
        border-radius: 4px 4px 0 0;
    }

    /* Panel content -->
    .e-dashboardlayout.e-control .e-panel .e-panel-container .e-panel-content {
        background-color: white;
        padding: 15px;
        border-radius: 0 0 4px 4px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }

    /* Resize icon -->
    .e-dashboardlayout.e-control .e-panel .e-panel-container .e-resize.e-double {
        color: #667eea;
        opacity: 0.6;
        transition: opacity 0.3s;
    }

    .e-dashboardlayout.e-control .e-panel:hover .e-resize.e-double {
        opacity: 1;
    }
</style>
```
