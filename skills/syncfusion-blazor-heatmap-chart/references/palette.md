# Palette Configuration for HeatMap Chart

## Table of Contents

- [Overview](#overview)
- [Palette Types](#palette-types)
  - [Gradient Palette](#gradient-palette)
  - [Fixed Palette](#fixed-palette)
- [Color Configuration](#color-configuration)
- [Complete Implementation Examples](#complete-implementation-examples)
- [Key Properties and Settings](#key-properties-and-settings)
- [Best Practices](#best-practices)

## Overview

In a HeatMap Chart, each data point is displayed as a cell with a color applied based on the data value. The palette system defines the color range for cells and the gradient type for colors. This reference provides comprehensive guidance on configuring palettes to visualize data effectively using color schemes.

### Purpose of Palette

The palette in a HeatMap Chart serves to:
- Define color ranges for cells based on data values
- Control the transition between colors (gradient or fixed)
- Map data values to visual representations through color
- Provide intuitive data visualization through color-coded cells

### Color Definition

Colors can be defined in the following formats using the `Color` property in the `HeatMapPalette`:
- **RGB format**: `rgb(192, 108, 132)`
- **Hex codes**: `#C06C84`
- **Named colors**: Standard HTML color names

The defined colors are applied to cell backgrounds based on the palette type and cell value.

## Palette Types

The HeatMap Chart supports two primary palette types that determine how colors are applied to cells. The palette type is configured using the `Type` property in the `HeatMapPaletteSettings` component.

### Gradient Palette

The gradient palette provides a smooth transition between defined palette colors based on cell values. The HeatMap automatically calculates all intermediate gradient colors between the start and end colors for all distinct data values.

#### Features of Gradient Palette

- **Smooth Color Transition**: Creates continuous color progression between defined colors
- **Automatic Calculation**: Computes all intermediate colors automatically
- **Default Colors**: Uses default start and end colors if not explicitly defined
- **Value-Based Mapping**: Colors are interpolated based on the data value range

#### Configuration

To enable gradient palette, set the `Type` property to `PaletteType.Gradient`:

```cshtml
<HeatMapPaletteSettings Type="PaletteType.Gradient">
    <HeatMapPalettes>
        <HeatMapPalette Color="#C06C84"></HeatMapPalette>
        <HeatMapPalette Color="#6C5B7B"></HeatMapPalette>
        <HeatMapPalette Color="#355C7D"></HeatMapPalette>
    </HeatMapPalettes>
</HeatMapPaletteSettings>
```

#### Complete Gradient Palette Example

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
            <HeatMapPalette Color="#C06C84"></HeatMapPalette>
            <HeatMapPalette Color="#6C5B7B"></HeatMapPalette>
            <HeatMapPalette Color="#355C7D"></HeatMapPalette>
        </HeatMapPalettes>
    </HeatMapPaletteSettings>
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

**Visual Result**: The HeatMap displays cells with smoothly transitioning colors from #C06C84 (pink) through #6C5B7B (purple) to #355C7D (blue), with each cell's color interpolated based on its data value within the range.

### Fixed Palette

The fixed palette applies solid, distinct colors to HeatMap cells without gradual transitions. Data values are grouped based on the number of colors defined, creating distinct color bands or ranges.

#### Features of Fixed Palette

- **Solid Colors**: Each cell receives a single, solid color without blending
- **Value Grouping**: Data values are categorized into distinct ranges
- **Clear Boundaries**: Sharp distinctions between color ranges
- **Simplified Visualization**: Easier to distinguish between different value ranges

#### Configuration

To enable fixed palette, set the `Type` property to `PaletteType.Fixed`:

```cshtml
<HeatMapPaletteSettings Type="PaletteType.Fixed">
    <HeatMapPalettes>
        <HeatMapPalette Color="#C06C84"></HeatMapPalette>
        <HeatMapPalette Color="#6C5B7B"></HeatMapPalette>
        <HeatMapPalette Color="#355C7D"></HeatMapPalette>
    </HeatMapPalettes>
</HeatMapPaletteSettings>
```

#### Complete Fixed Palette Example

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
            <HeatMapPalette Color="#C06C84"></HeatMapPalette>
            <HeatMapPalette Color="#6C5B7B"></HeatMapPalette>
            <HeatMapPalette Color="#355C7D"></HeatMapPalette>
        </HeatMapPalettes>
    </HeatMapPaletteSettings>
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

**Visual Result**: The HeatMap displays cells with one of three distinct colors (#C06C84, #6C5B7B, or #355C7D), with data values grouped into three ranges. Each range is represented by a single solid color without transitions.

## Color Configuration

### Defining Custom Color Schemes

You can define custom color schemes by specifying multiple `HeatMapPalette` entries within `HeatMapPalettes`. Each palette entry represents a color point in your scheme.

#### Two-Color Scheme

```cshtml
<HeatMapPaletteSettings Type="PaletteType.Gradient">
    <HeatMapPalettes>
        <HeatMapPalette Color="#FFFFFF"></HeatMapPalette>
        <HeatMapPalette Color="#FF0000"></HeatMapPalette>
    </HeatMapPalettes>
</HeatMapPaletteSettings>
```

This creates a gradient from white to red, useful for showing intensity from minimal to maximum values.

#### Multi-Color Scheme

```cshtml
<HeatMapPaletteSettings Type="PaletteType.Gradient">
    <HeatMapPalettes>
        <HeatMapPalette Color="#C06C84"></HeatMapPalette>
        <HeatMapPalette Color="#6C5B7B"></HeatMapPalette>
        <HeatMapPalette Color="#355C7D"></HeatMapPalette>
    </HeatMapPalettes>
</HeatMapPaletteSettings>
```

Multiple colors create more complex gradients with intermediate transition points.

### Color Progression in Gradient Mode

When using gradient palette type, the HeatMap calculates color transitions based on:

1. **Minimum Value**: Maps to the first color in the palette
2. **Maximum Value**: Maps to the last color in the palette
3. **Intermediate Values**: Colors are interpolated proportionally between palette colors

**Example Calculation**:
- Data range: 0 to 100
- Palette: Blue (#0000FF) → Green (#00FF00) → Red (#FF0000)
- Value 0: Blue
- Value 50: Green
- Value 100: Red
- Value 25: Blue-Green blend
- Value 75: Green-Red blend

### Color Mapping in Fixed Mode

For fixed palette type, the value range is divided equally among the defined colors:

**Example with 3 Colors**:
- Data range: 0 to 100
- Colors: Color A, Color B, Color C
- 0-33.3: Color A
- 33.3-66.6: Color B
- 66.6-100: Color C

## Complete Implementation Examples

### Example 1: Sales Performance HeatMap with Gradient Palette

```cshtml
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@SalesData">
    <HeatMapTitleSettings Text="Quarterly Sales Performance by Region">
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@Regions"></HeatMapXAxis>
    <HeatMapYAxis Labels="@Quarters"></HeatMapYAxis>
    <HeatMapCellSettings ShowLabel="true" TileType="CellType.Rect"></HeatMapCellSettings>
    <HeatMapPaletteSettings Type="PaletteType.Gradient">
        <HeatMapPalettes>
            <HeatMapPalette Color="#C06C84"></HeatMapPalette>
            <HeatMapPalette Color="#6C5B7B"></HeatMapPalette>
            <HeatMapPalette Color="#355C7D"></HeatMapPalette>
        </HeatMapPalettes>
    </HeatMapPaletteSettings>
</SfHeatMap>

@code{
    int[,] SalesData = new int[,]
    {
        {73, 39, 26, 39, 94, 0},
        {93, 58, 53, 38, 26, 68},
        {99, 28, 22, 4, 66, 90},
        {14, 26, 97, 69, 69, 3}
    };
    string[] Regions = new string[] {"North", "South", "East", "West", "Central", "Overseas"};
    string[] Quarters = new string[] {"Q1", "Q2", "Q3", "Q4"};
}
```

### Example 2: Temperature Monitoring with Fixed Palette

```cshtml
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@TemperatureData">
    <HeatMapTitleSettings Text="Daily Temperature Readings (°C)">
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@Days"></HeatMapXAxis>
    <HeatMapYAxis Labels="@Hours"></HeatMapYAxis>
    <HeatMapCellSettings ShowLabel="true" TileType="CellType.Rect"></HeatMapCellSettings>
    <HeatMapPaletteSettings Type="PaletteType.Fixed">
        <HeatMapPalettes>
            <HeatMapPalette Color="#C06C84"></HeatMapPalette>
            <HeatMapPalette Color="#6C5B7B"></HeatMapPalette>
            <HeatMapPalette Color="#355C7D"></HeatMapPalette>
        </HeatMapPalettes>
    </HeatMapPaletteSettings>
</SfHeatMap>

@code{
    int[,] TemperatureData = new int[,]
    {
        {73, 39, 26, 39, 94, 0},
        {93, 58, 53, 38, 26, 68},
        {99, 28, 22, 4, 66, 90},
        {14, 26, 97, 69, 69, 3},
        {7, 46, 47, 47, 88, 6},
        {41, 55, 73, 23, 3, 79}
    };
    string[] Days = new string[] {"Mon", "Tue", "Wed", "Thu", "Fri", "Sat"};
    string[] Hours = new string[] {"6AM", "9AM", "12PM", "3PM", "6PM", "9PM"};
}
```

## Key Properties and Settings

### HeatMapPaletteSettings Properties

| Property | Type | Description |
|----------|------|-------------|
| `Type` | `PaletteType` | Defines the palette type. Options: `Gradient` or `Fixed` |

### HeatMapPalette Properties

| Property | Type | Description |
|----------|------|-------------|
| `Color` | `string` | Specifies the color for the palette in RGB or hex code format |

### Palette Type Enumeration

```csharp
public enum PaletteType
{
    Gradient,
    Fixed
}
```

## Best Practices

### Choosing Palette Type

**Use Gradient Palette When**:
- You need to show continuous data progression
- Data has many distinct values
- Smooth transitions better represent data relationships
- Visual continuity is important (e.g., temperature, pressure)

**Use Fixed Palette When**:
- You need to categorize data into distinct groups
- Clear boundaries between ranges are important
- Data naturally falls into categories
- Easier interpretation of discrete ranges is desired

### Color Selection Guidelines

1. **Contrast**: Ensure sufficient contrast between colors for readability
2. **Accessibility**: Consider color-blind friendly palettes
3. **Context**: Choose colors that match the data context (e.g., blue for cold, red for hot)
4. **Number of Colors**: Use 2-5 colors for optimal visualization
   - 2 colors: Simple min-max representation
   - 3 colors: Low-medium-high categorization
   - 4-5 colors: More granular differentiation

### Common Color Schemes

**Temperature Data**:
```cshtml
<HeatMapPalettes>
    <HeatMapPalette Color="#0000FF"></HeatMapPalette> <!-- Blue for cold -->
    <HeatMapPalette Color="#FFFF00"></HeatMapPalette> <!-- Yellow for moderate -->
    <HeatMapPalette Color="#FF0000"></HeatMapPalette> <!-- Red for hot -->
</HeatMapPalettes>
```

**Performance Metrics**:
```cshtml
<HeatMapPalettes>
    <HeatMapPalette Color="#FF0000"></HeatMapPalette> <!-- Red for low -->
    <HeatMapPalette Color="#FFFF00"></HeatMapPalette> <!-- Yellow for medium -->
    <HeatMapPalette Color="#00FF00"></HeatMapPalette> <!-- Green for high -->
</HeatMapPalettes>
```

**Professional/Business**:
```cshtml
<HeatMapPalettes>
    <HeatMapPalette Color="#C06C84"></HeatMapPalette>
    <HeatMapPalette Color="#6C5B7B"></HeatMapPalette>
    <HeatMapPalette Color="#355C7D"></HeatMapPalette>
</HeatMapPalettes>
```

### Performance Considerations

- **Color Count**: More colors in gradient mode require more calculations but provide smoother transitions
- **Data Size**: For large datasets, fixed palette may render faster than gradient
- **Browser Rendering**: Modern browsers handle both types efficiently, but test with your specific data volume

### Integration with Other Features

The palette system works seamlessly with:
- **Legend**: Colors are automatically displayed in the legend
- **Tooltips**: Color values are reflected in tooltip displays
- **Cell Labels**: Ensure text color contrasts with palette colors for readability
- **Themes**: Palette colors override default theme colors

This completes the comprehensive palette configuration reference for the Syncfusion Blazor HeatMap Chart component.