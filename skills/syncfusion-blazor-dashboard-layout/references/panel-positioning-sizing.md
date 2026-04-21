# Panel Positioning and Sizing

## Table of Contents
- [Panel Properties](#panel-properties)
- [Positioning Panels](#positioning-panels)
- [Sizing Panels](#sizing-panels)
- [Size Constraints](#size-constraints)
- [Panel Identification](#panel-identification)

## Panel Properties

Each `DashboardLayoutPanel` has the following key properties:

| Property | Default | Description |
|----------|---------|-------------|
| **Id** | null | Unique identifier for the panel |
| **Row** | 0 | Vertical position in grid (0-based) |
| **Column** | 0 | Horizontal position in grid (0-based) |
| **SizeX** | 1 | Panel width in cells |
| **SizeY** | 1 | Panel height in cells |
| **MinSizeX** | 1 | Minimum width in cells |
| **MinSizeY** | 1 | Minimum height in cells |
| **MaxSizeX** | null | Maximum width in cells (null = unlimited) |
| **MaxSizeY** | null | Maximum height in cells (null = unlimited) |
| **HeaderTemplate** | null | Template for panel header |
| **ContentTemplate** | null | Template for panel content |
| **CssClass** | null | Custom CSS class for styling |

## Positioning Panels

### Row and Column Properties

Panels are positioned using **Row** and **Column** properties. Both are 0-based indices on the dashboard grid.

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout CellSpacing="@(new double[]{20, 20})" Columns="4">
    <DashboardLayoutPanels>
        <!-- Panel at Row 0, Column 0 (default) -->
        <DashboardLayoutPanel>
            <ContentTemplate><div>1</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Panel at Row 0, Column 1 -->
        <DashboardLayoutPanel Column=1>
            <ContentTemplate><div>2</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Panel at Row 0, Column 2 -->
        <DashboardLayoutPanel Column=2>
            <ContentTemplate><div>3</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Panel at Row 1, Column 0 -->
        <DashboardLayoutPanel Row=1>
            <ContentTemplate><div>4</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Panel at Row 1, Column 1 -->
        <DashboardLayoutPanel Row=1 Column=1>
            <ContentTemplate><div>5</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Panel at Row 1, Column 2 -->
        <DashboardLayoutPanel Row=1 Column=2>
            <ContentTemplate><div>6</div></ContentTemplate>
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

**Key Points:**
- `Row` specifies the row position (0 = first row, 1 = second row, etc.)
- `Column` specifies the column position (0 = first column, 1 = second column, etc.)
- Grid arrangement depends on the Columns property (how many columns fit per row)
- Panels are automatically arranged left-to-right, top-to-bottom when you don't specify positions

## Sizing Panels

### SizeX and SizeY Properties

Panel dimensions are defined in **cells**:
- **SizeX**: Width in cells (default = 1)
- **SizeY**: Height in cells (default = 1)

The actual pixel size depends on your grid configuration (Columns, CellAspectRatio, container width).

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout CellSpacing="@(new double[]{10, 10})" Columns="5">
    <DashboardLayoutPanels>
        <!-- Single cell panel (1x1) -->
        <DashboardLayoutPanel>
            <ContentTemplate><div>0</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- 2 cells wide, 2 cells tall -->
        <DashboardLayoutPanel SizeX=2 SizeY=2 Column=1>
            <ContentTemplate><div>1</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- 1 cell wide, 2 cells tall -->
        <DashboardLayoutPanel SizeY=2 Column=3>
            <ContentTemplate><div>2</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Single cell at next row -->
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

**Sizing Guidelines:**
- Start with SizeX=1 and SizeY=1 (default single cell)
- Use larger values for panels that need more space
- Make sure SizeX + Column ≤ Columns property (avoid overflow)
- Cell aspect ratio affects actual panel height

## Size Constraints

### Minimum and Maximum Sizes

Control what sizes users can resize panels to:

```csharp
<DashboardLayoutPanel 
    SizeX=2 
    SizeY=2
    MinSizeX=1
    MinSizeY=1
    MaxSizeX=4
    MaxSizeY=4>
    <ContentTemplate><div>Resizable Panel</div></ContentTemplate>
</DashboardLayoutPanel>
```

**Properties:**
- **MinSizeX**: Minimum width users can resize to (default = 1)
- **MinSizeY**: Minimum height users can resize to (default = 1)
- **MaxSizeX**: Maximum width users can resize to (null = no limit)
- **MaxSizeY**: Maximum height users can resize to (null = no limit)

### Preventing Extreme Resizing

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    Columns="5" 
    AllowResizing="true">
    <DashboardLayoutPanels>
        <!-- User can resize between 2x2 and 4x4 cells -->
        <DashboardLayoutPanel 
            SizeX=2 
            SizeY=2
            MinSizeX=2
            MinSizeY=2
            MaxSizeX=4
            MaxSizeY=4>
            <HeaderTemplate><div>Constrained Panel</div></HeaderTemplate>
            <ContentTemplate><div>Content</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Standard panel, can be resized to any size -->
        <DashboardLayoutPanel Column=2>
            <HeaderTemplate><div>Flexible Panel</div></HeaderTemplate>
            <ContentTemplate><div>Content</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Panel with minimum size only -->
        <DashboardLayoutPanel 
            SizeX=2 
            Column=3
            MinSizeX=1
            MinSizeY=1>
            <HeaderTemplate><div>Min Constrained</div></HeaderTemplate>
            <ContentTemplate><div>Content</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>
```

**Constraint Behavior:**
- Min constraints prevent panels from becoming too small
- Max constraints prevent panels from becoming too large
- If MaxSizeX is null, no upper limit applies
- Apply constraints to ensure usable minimum content area

## Panel Identification

### Using the Id Property

Each panel should have a unique identifier:

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout>
    <DashboardLayoutPanels>
        <DashboardLayoutPanel Id="panel-metrics">
            <ContentTemplate><div>Metrics Panel</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel Id="panel-chart" Column=1>
            <ContentTemplate><div>Chart Panel</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel Id="panel-table" Column=2>
            <ContentTemplate><div>Table Panel</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>
```

**Important for:**
- State persistence (ID is required to save/restore layout)
- Identifying panels in dynamic scenarios
- Preventing panel overlap when rendering dynamically
- Tracking specific panels in code

### Dynamic Panel Generation

When generating panels dynamically, ensure unique IDs:

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout CellSpacing="@(new double[]{20, 20})" Columns="4">
    <DashboardLayoutPanels>
        @foreach (var panel in PanelItems)
        {
            <DashboardLayoutPanel 
                Id="@panel.Id" 
                Row="@panel.Row" 
                Column="@panel.Column">
                <ContentTemplate>
                    <div class="panel-content">@panel.Content</div>
                </ContentTemplate>
            </DashboardLayoutPanel>
        }
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    public class PanelModel
    {
        public string Id { get; set; }
        public int Row { get; set; } = 0;
        public int Column { get; set; } = 0;
        public string Content { get; set; }
    }

    private List<PanelModel> PanelItems = new List<PanelModel>
    {
        new PanelModel { Id = "panel1", Row = 0, Column = 0, Content = "Panel 1" },
        new PanelModel { Id = "panel2", Row = 0, Column = 1, Content = "Panel 2" },
        new PanelModel { Id = "panel3", Row = 0, Column = 2, Content = "Panel 3" },
        new PanelModel { Id = "panel4", Row = 1, Column = 0, Content = "Panel 4" },
        new PanelModel { Id = "panel5", Row = 1, Column = 1, Content = "Panel 5" },
        new PanelModel { Id = "panel6", Row = 1, Column = 2, Content = "Panel 6" }
    };
}

<style>
    .panel-content {
        text-align: center;
        margin-top: 10px;
        font-size: 18px;
        font-weight: 500;
    }
</style>
```

**Best Practices:**
- Always assign unique IDs when rendering panels dynamically
- Use descriptive ID names (e.g., "panel-metrics", "panel-chart")
- IDs are critical for preventing overlaps in dynamic scenarios
- Include IDs in your data model during dynamic generation
