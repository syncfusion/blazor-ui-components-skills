# Legend in Blazor HeatMap Chart Component

## Table of Contents

1. [Overview](#overview)
2. [Legend Types](#legend-types)
   - [Gradient Legend](#gradient-legend)
   - [List (Fixed) Legend](#list-fixed-legend)
3. [Legend Placement](#legend-placement)
4. [Legend Alignment](#legend-alignment)
5. [Legend Dimensions](#legend-dimensions)
6. [Smart Legend](#smart-legend)
   - [Label Display Types](#label-display-types)
7. [Legend Selection](#legend-selection)
8. [Configuration Properties](#configuration-properties)

---

## Overview

The legend provides essential information about the heat map cell values and color ranges. It helps users understand the data visualization by displaying the correlation between colors and data values.

**Key Features:**
- Enable or disable legend visibility using the `Visible` property
- Support for gradient and list-type legends using the `Type` property in `HeatMapPaletteSettings`
- Customizable position, alignment, and dimensions using `Position`, `Alignment`, `Width`, and `Height` properties
- Smart legend for better responsiveness using the `EnableSmartLegend` property
- Interactive legend selection for toggling cell visibility using the `ToggleVisibility` property

**Basic Legend Configuration:**

To enable the legend in your HeatMap Chart, set the `Visible` property to `true` in the `HeatMapLegendSettings` component.

```cshtml
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)">
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
    <HeatMapCellSettings ShowLabel="true" TileType="CellType.Rect"></HeatMapCellSettings>
    <HeatMapPaletteSettings Type="PaletteType.Gradient">
        <HeatMapPalettes>
            <HeatMapPalette Value="0" Color="#C2E7EC"></HeatMapPalette>
            <HeatMapPalette Value="10" Color="#AEDFE6"></HeatMapPalette>
            <HeatMapPalette Value="20" Color="#9AD7E0"></HeatMapPalette>
            <HeatMapPalette Value="30" Color="#72C7D4"></HeatMapPalette>
            <HeatMapPalette Value="40" Color="#5EBFCE"></HeatMapPalette>
            <HeatMapPalette Value="50" Color="#4AB7C8"></HeatMapPalette>
            <HeatMapPalette Value="60" Color="#309DAE"></HeatMapPalette>
            <HeatMapPalette Value="70" Color="#2B8C9B"></HeatMapPalette>
            <HeatMapPalette Value="80" Color="#257A87"></HeatMapPalette>
            <HeatMapPalette Value="90" Color="#15464D"></HeatMapPalette>
            <HeatMapPalette Value="100" Color="#000000"></HeatMapPalette>
        </HeatMapPalettes>
    </HeatMapPaletteSettings>
    <HeatMapLegendSettings Visible="true"></HeatMapLegendSettings>
</SfHeatMap>

@code{
    int[,] GetDefaultData()
    {
        int[,] dataSource = new int[,]
        {
            {73, 39, 26, 39, 94, 0},
            {93, 58, 53, 38, 26, 68},
            {99, 28, 22, 4, 66, 90},
            {14, 26, 97, 69, 69, 3},
            {7, 46, 47, 47, 88, 6},
            {41, 55, 73, 23, 3, 79}
        };
        return dataSource;
    }
    string[] XAxisLabels = new string[] {"Nancy", "Andrew", "Janet", "Margaret", "Steven", "Michael" };
    string[] YAxisLabels = new string[] { "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };
    public object HeatMapData { get; set; }
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

---

## Legend Types

The HeatMap Chart supports two distinct legend types that provide different visual representations of the data range using the `Type` property in `HeatMapPaletteSettings`:

### Gradient Legend

**Description:**
The gradient legend is configured by setting the `Type` property to `PaletteType.Gradient` in `HeatMapPaletteSettings`:
- Displays a continuous color legend with smooth transitions between palette color values
- Ideal for showing continuous data ranges
- Default legend type when using `PaletteType.Gradient`

**Characteristics:**
- Seamless color gradients
- No distinct boundaries between colors
- Best for continuous numerical data

### List (Fixed) Legend

**Description:**
The list legend is configured by setting the `Type` property to `PaletteType.Fixed` in `HeatMapPaletteSettings`:
- Displays a fixed color legend with distinct, separate color items
- Each palette color is shown as an individual list item
- Useful when data has specific ranges or categories

**Configuration:**

Change the legend type by setting the `Type` property in `HeatMapPaletteSettings` to `PaletteType.Fixed`:

```cshtml
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)">
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
    <HeatMapCellSettings ShowLabel="true" TileType="CellType.Rect"></HeatMapCellSettings>
    <HeatMapPaletteSettings Type="PaletteType.Fixed"></HeatMapPaletteSettings>
    <HeatMapLegendSettings ShowLabel="true"></HeatMapLegendSettings>
</SfHeatMap>

@code{
    int[,] GetDefaultData()
    {
        int[,] dataSource = new int[,]
        {
            {73, 39, 26, 39, 94, 0},
            {93, 58, 53, 38, 26, 68},
            {99, 28, 22, 4, 66, 90},
            {14, 26, 97, 69, 69, 3},
            {7, 46, 47, 47, 88, 6},
            {41, 55, 73, 23, 3, 79}
        };
        return dataSource;
    }
    string[] XAxisLabels = new string[] {"Nancy", "Andrew", "Janet", "Margaret", "Steven", "Michael" };
    string[] YAxisLabels = new string[] { "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };
    public object HeatMapData { get; set; }
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

---

## Legend Placement

**Overview:**

Control the position of the legend relative to the HeatMap Chart using the `Position` property in the `HeatMapLegendSettings` component. The legend can be placed at any of the four sides of the chart.

**Available Positions:**
 - `Syncfusion.Blazor.HeatMap.LegendPosition.Left` - Places legend on the left side
 - `Syncfusion.Blazor.HeatMap.LegendPosition.Right` - Places legend on the right side (default)
 - `Syncfusion.Blazor.HeatMap.LegendPosition.Top` - Places legend at the top
 - `Syncfusion.Blazor.HeatMap.LegendPosition.Bottom` - Places legend at the bottom

**Example - Top Position:**

```cshtml
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)">
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
    <HeatMapCellSettings ShowLabel="true" TileType="CellType.Rect"></HeatMapCellSettings>
    <HeatMapLegendSettings ShowLabel="true" Position="Syncfusion.Blazor.HeatMap.LegendPosition.Top"></HeatMapLegendSettings>
</SfHeatMap>

@code{
    int[,] GetDefaultData()
    {
        int[,] dataSource = new int[,]
        {
            {73, 39, 26, 39, 94, 0},
            {93, 58, 53, 38, 26, 68},
            {99, 28, 22, 4, 66, 90},
            {14, 26, 97, 69, 69, 3},
            {7, 46, 47, 47, 88, 6},
            {41, 55, 73, 23, 3, 79}
        };
        return dataSource;
    }
    string[] XAxisLabels = new string[] {"Nancy", "Andrew", "Janet", "Margaret", "Steven", "Michael" };
    string[] YAxisLabels = new string[] { "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };
    public object HeatMapData { get; set; }
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

---

## Legend Alignment

**Overview:**

The `Alignment` property in the `HeatMapLegendSettings` component allows you to align the legend within its allocated space relative to the HeatMap Chart.

**Available Alignment Options:**
- `Syncfusion.Blazor.HeatMap.Alignment.Near` - Aligns legend to the near edge (left/top)
- `Syncfusion.Blazor.HeatMap.Alignment.Center` - Centers the legend (default)
- `Syncfusion.Blazor.HeatMap.Alignment.Far` - Aligns legend to the far edge (right/bottom)

**Example - Center Alignment:**

```cshtml
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)">
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
    <HeatMapCellSettings ShowLabel="true" TileType="CellType.Rect"></HeatMapCellSettings>
    <HeatMapLegendSettings ShowLabel="true" Position="Syncfusion.Blazor.HeatMap.LegendPosition.Right" Height="150px" Alignment="Syncfusion.Blazor.HeatMap.Alignment.Center"></HeatMapLegendSettings>
</SfHeatMap>

@code{
    int[,] GetDefaultData()
    {
        int[,] dataSource = new int[,]
        {
            {73, 39, 26, 39, 94, 0},
            {93, 58, 53, 38, 26, 68},
            {99, 28, 22, 4, 66, 90},
            {14, 26, 97, 69, 69, 3},
            {7, 46, 47, 47, 88, 6},
            {41, 55, 73, 23, 3, 79}
        };
        return dataSource;
    }
    string[] XAxisLabels = new string[] {"Nancy", "Andrew", "Janet", "Margaret", "Steven", "Michael" };
    string[] YAxisLabels = new string[] { "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };
    public object HeatMapData { get; set; }
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

**Usage Tips:**
- Combine alignment with position for precise legend placement
- Center alignment works well with custom dimensions
- Use alignment to optimize space utilization in your layout

---

## Legend Dimensions

**Overview:**

Customize the size of the legend using the `Width` and `Height` properties in the `HeatMapLegendSettings` component. Values can be specified in pixels (e.g., "200px") or percentages (e.g., "50%").

**Properties:**
- `Width` - Sets the legend width
- `Height` - Sets the legend height

**Example - Custom Dimensions:**

```cshtml
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)">
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
    <HeatMapCellSettings ShowLabel="true" TileType="CellType.Rect"></HeatMapCellSettings>
    <HeatMapLegendSettings ShowLabel="true" Position="Syncfusion.Blazor.HeatMap.LegendPosition.Right" Width="200px" Height="150px"></HeatMapLegendSettings>
</SfHeatMap>

@code{
    int[,] GetDefaultData()
    {
        int[,] dataSource = new int[,]
        {
            {73, 39, 26, 39, 94, 0},
            {93, 58, 53, 38, 26, 68},
            {99, 28, 22, 4, 66, 90},
            {14, 26, 97, 69, 69, 3},
            {7, 46, 47, 47, 88, 6},
            {41, 55, 73, 23, 3, 79}
        };
        return dataSource;
    }
    string[] XAxisLabels = new string[] {"Nancy", "Andrew", "Janet", "Margaret", "Steven", "Michael" };
    string[] YAxisLabels = new string[] { "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };
    public object HeatMapData { get; set; }
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

**Best Practices:**
- Use pixel values for fixed-size layouts
- Use percentage values for responsive designs
- Ensure dimensions are appropriate for the number of legend items
- Consider the overall chart size when setting legend dimensions

---

## Smart Legend

**Overview:**

Smart Legend is an advanced feature designed to improve responsiveness and readability when working with palettes that have numerous color items. This feature is controlled by the `EnableSmartLegend` property and is particularly useful for list-type legends with many segments.

**Key Features:**
- Enhanced responsiveness for legends with many items using `EnableSmartLegend` property
- Improved readability through intelligent label management using `LabelDisplayType` property
- Configurable label display modes with `LabelDisplayType` property values (All, Edge, None)
- Only available for Fixed (List) palette type when `Type` is set to `PaletteType.Fixed`

**Enabling Smart Legend:**

Set the `EnableSmartLegend` property to `true` in the `HeatMapLegendSettings` component:

### Label Display Types

The `LabelDisplayType` property in the `HeatMapLegendSettings` component controls how legend labels are displayed in smart legend mode:

**Available Display Types:**

1. **All** - Displays all labels in the legend using `LabelDisplayType.All`
   - Shows every legend label
   - Best for legends with fewer items
   - May cause crowding with many items

2. **Edge** - Displays labels only at extreme ends using `LabelDisplayType.Edge`
   - Shows only the first and last legend labels
   - Ideal for gradient-like ranges
   - Provides clean, minimalist appearance

3. **None** - No labels are displayed using `LabelDisplayType.None`
   - Completely hides legend labels
   - Tooltips appear on hover to show values
   - Maximizes visual space

**Example - Smart Legend Configuration:**

```cshtml
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)">
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
    <HeatMapCellSettings ShowLabel="true" TileType="CellType.Rect"></HeatMapCellSettings>
    <HeatMapPaletteSettings Type="PaletteType.Fixed">
        <HeatMapPalettes>
            <HeatMapPalette Value="0" Color="#C2E7EC"></HeatMapPalette>
            <HeatMapPalette Value="10" Color="#AEDFE6"></HeatMapPalette>
            <HeatMapPalette Value="20" Color="#9AD7E0"></HeatMapPalette>
            <HeatMapPalette Value="30" Color="#72C7D4"></HeatMapPalette>
            <HeatMapPalette Value="40" Color="#5EBFCE"></HeatMapPalette>
            <HeatMapPalette Value="50" Color="#4AB7C8"></HeatMapPalette>
            <HeatMapPalette Value="60" Color="#309DAE"></HeatMapPalette>
            <HeatMapPalette Value="70" Color="#2B8C9B"></HeatMapPalette>
            <HeatMapPalette Value="80" Color="#257A87"></HeatMapPalette>
            <HeatMapPalette Value="90" Color="#15464D"></HeatMapPalette>
            <HeatMapPalette Value="100" Color="#000000"></HeatMapPalette>
        </HeatMapPalettes>
    </HeatMapPaletteSettings>
    <HeatMapLegendSettings ShowLabel="true" Position="Syncfusion.Blazor.HeatMap.LegendPosition.Right" EnableSmartLegend="true" Alignment="Syncfusion.Blazor.HeatMap.Alignment.Center"></HeatMapLegendSettings>
</SfHeatMap>

@code{
    int[,] GetDefaultData()
    {
        int[,] dataSource = new int[,]
        {
            {73, 39, 26, 39, 94, 0},
            {93, 58, 53, 38, 26, 68},
            {99, 28, 22, 4, 66, 90},
            {14, 26, 97, 69, 69, 3},
            {7, 46, 47, 47, 88, 6},
            {41, 55, 73, 23, 3, 79}
        };
        return dataSource;
    }
    string[] XAxisLabels = new string[] {"Nancy", "Andrew", "Janet", "Margaret", "Steven", "Michael" };
    string[] YAxisLabels = new string[] { "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };
    public object HeatMapData { get; set; }
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

**When to Use Smart Legend:**
- Palettes with 10 or more color segments
- Space-constrained layouts
- Responsive applications
- When label readability is a concern

---

## Legend Selection

**Overview:**

Legend Selection is an interactive feature that allows users to toggle the visibility of heat map cells based on their value ranges using the `ToggleVisibility` property. When enabled, clicking on a legend item will hide or show the corresponding cells in the heat map.

**Key Benefits:**
- Interactive data exploration using `ToggleVisibility` property
- Focus on specific value ranges by toggling legend items
- Highlight data of interest through interactive selection
- Improve data analysis capabilities

**Enabling Legend Selection:**

Set the `ToggleVisibility` property to `true` in the `HeatMapLegendSettings` component:

**Example - Interactive Legend:**

```cshtml
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)">
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
    <HeatMapCellSettings ShowLabel="true" TileType="CellType.Rect"></HeatMapCellSettings>
    <HeatMapPaletteSettings Type="PaletteType.Fixed">
        <HeatMapPalettes>
            <HeatMapPalette Value="0" Color="#C2E7EC"></HeatMapPalette>
            <HeatMapPalette Value="10" Color="#AEDFE6"></HeatMapPalette>
            <HeatMapPalette Value="20" Color="#9AD7E0"></HeatMapPalette>
            <HeatMapPalette Value="30" Color="#72C7D4"></HeatMapPalette>
            <HeatMapPalette Value="40" Color="#5EBFCE"></HeatMapPalette>
            <HeatMapPalette Value="50" Color="#4AB7C8"></HeatMapPalette>
            <HeatMapPalette Value="60" Color="#309DAE"></HeatMapPalette>
            <HeatMapPalette Value="70" Color="#2B8C9B"></HeatMapPalette>
            <HeatMapPalette Value="80" Color="#257A87"></HeatMapPalette>
            <HeatMapPalette Value="90" Color="#15464D"></HeatMapPalette>
            <HeatMapPalette Value="100" Color="#000000"></HeatMapPalette>
        </HeatMapPalettes>
    </HeatMapPaletteSettings>
    <HeatMapLegendSettings ShowLabel="true" Position="Syncfusion.Blazor.HeatMap.LegendPosition.Right" EnableSmartLegend="true" ToggleVisibility="true"></HeatMapLegendSettings>
</SfHeatMap>

@code{
    int[,] GetDefaultData()
    {
        int[,] dataSource = new int[,]
        {
            {73, 39, 26, 39, 94, 0},
            {93, 58, 53, 38, 26, 68},
            {99, 28, 22, 4, 66, 90},
            {14, 26, 97, 69, 69, 3},
            {7, 46, 47, 47, 88, 6},
            {41, 55, 73, 23, 3, 79}
        };
        return dataSource;
    }
    string[] XAxisLabels = new string[] {"Nancy", "Andrew", "Janet", "Margaret", "Steven", "Michael"};
    string[] YAxisLabels = new string[] { "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };
    public object HeatMapData { get; set; }
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

**Usage:**
- Click on a legend item to hide cells in that value range
- Click again to show the cells
- Multiple legend items can be toggled independently
- Works best with Fixed (List) palette type

---

## Configuration Properties

**Summary of HeatMapLegendSettings Properties:**

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Visible` | bool | false | Shows or hides the legend |
| `Position` | LegendPosition | Right | Specifies legend position (Left, Right, Top, Bottom) |
| `Alignment` | Syncfusion.Blazor.HeatMap.Alignment | Syncfusion.Blazor.HeatMap.Alignment.Center | Aligns legend within its space (Near, Center, Far) |
| `Width` | string | - | Sets legend width (e.g., "200px" or "50%") |
| `Height` | string | - | Sets legend height (e.g., "150px" or "30%") |
| `ShowLabel` | bool | false | Shows or hides legend labels |
| `EnableSmartLegend` | bool | false | Enables smart legend for Fixed palette type |
| `LabelDisplayType` | LabelDisplayType | All | Controls label display (All, Edge, None) |
| `ToggleVisibility` | bool | false | Enables interactive cell visibility toggling |

**Related Component Properties:**

**HeatMapPaletteSettings:**
- `Type` - Sets palette type (PaletteType.Gradient or PaletteType.Fixed)

**Best Practices:**
- Always enable `Visible` to show the legend
- Use `ShowLabel` to display value labels on legend items
- Combine `EnableSmartLegend` with `ToggleVisibility` for enhanced interactivity
- Set appropriate `Width` and `Height` based on the number of palette items
- Choose `Position` and `Alignment` that complement your overall layout

---

This reference provides comprehensive coverage of all legend-related features in the Syncfusion Blazor HeatMap Chart component. Use these configurations to create informative and interactive heat map visualizations.