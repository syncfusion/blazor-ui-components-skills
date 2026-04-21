---
name: syncfusion-blazor-heatmap-chart
description: Implements the Syncfusion Blazor HeatMap Chart component for multi-dimensional data visualization using color-coded grids. Use this when working with heatmaps, data matrices, correlation displays, or bubble heatmap variants. This skill covers installation, data binding, axis configuration, color palettes, legends, tooltips, and accessibility features.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Data Visualization"
---

# Implementing Syncfusion Blazor HeatMap Chart

A comprehensive guide for implementing the Syncfusion Blazor HeatMap Chart component to visualize multi-dimensional data using color-coded cells in a matrix layout.

## When to Use This Skill

Use this skill when you need to:
- **Visualize matrix data** with color gradients representing values
- **Display correlation matrices** or comparison data across two dimensions
- **Create thermal or density visualizations** showing data intensity
- **Show tabular data** where cell colors represent magnitudes or categories
- **Implement bubble HeatMaps** combining size and color to represent multiple data dimensions
- **Build data dashboards** requiring intuitive grid-based visualizations
- **Display time-series data** with category or datetime axes
- **Visualize large datasets** with interactive tooltips and legends
- **Create accessible data visualizations** with WCAG-compliant features

## Component Overview

The Syncfusion Blazor HeatMap Chart component (`SfHeatMap`) is a powerful data visualization tool that displays multi-dimensional data in a matrix of cells, where each cell's color represents its value. It supports:

- **Multiple data binding modes**: Array (2D), JSON/Table, and Cell-based
- **Flexible axis types**: Category, Numeric, and DateTime with multi-level labels
- **Two visualization modes**: Rectangle cells and Bubble HeatMaps
- **Rich customization**: Palettes (gradient/fixed), legends, tooltips, titles, and labels
- **Interactive features**: Cell selection, events, and dynamic rendering
- **Accessibility**: WCAG compliance, keyboard navigation, and screen reader support
- **Advanced features**: Content Security Policy support, responsive sizing, performance optimization

## Documentation and Navigation Guide

### Getting Started & Installation
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Prerequisites and system requirements
- Creating Blazor WebAssembly app (Visual Studio, VS Code, .NET CLI)
- Installing Syncfusion.Blazor.HeatMap NuGet package
- Registering Syncfusion Blazor service
- Adding stylesheets and script resources
- Import namespaces
- First HeatMap component implementation
- Basic data source configuration
- Running and testing the application

### Data Binding & Working with Data
📄 **Read:** [references/working-with-data.md](references/working-with-data.md)
- Data source types overview (Array, JSON/Table, Cell)
- Array data binding with 2D and multi-dimensional arrays
- JSON/Table data binding with adaptor configuration
- Cell data binding for individual cell customization
- Field mapping for x-axis and y-axis
- Data transformation patterns
- Handling empty cells and null values
- Performance considerations for large datasets

### Axis Configuration
📄 **Read:** [references/axis.md](references/axis.md)
- Axis types: Category, Numeric, and DateTime
- Category axis with custom labels
- Numeric axis with range settings (min, max, interval)
- DateTime axis with formatting and interval types
- Axis label customization (rotation, alignment, style)
- Axis label templates
- Multi-level labels for hierarchical data
- Axis inversions and opposed positioning
- Axis title configuration

### Appearance & Customization
📄 **Read:** [references/appearance.md](references/appearance.md)
- Cell customizations (borders, highlighting, tile types)
- Margin configuration for spacing
- Title settings (text, alignment, text style)
- Data label configuration (visibility, format, templates)
- Border configuration (width, radius, color)
- Background and container styling
- Text style customization (size, color, font family, weight)
- Template support for data labels
- Responsive design considerations

### Color Palettes
📄 **Read:** [references/palette.md](references/palette.md)
- Palette types: Fixed and Gradient
- Fixed palette with color mapping and value ranges
- Gradient palette with color progression
- Custom color schemes
- Color for empty and null cells
- Built-in palette presets
- Color accessibility considerations
- Color gradient modes (Table, Row, Column)

### Legend Configuration
📄 **Read:** [references/legend.md](references/legend.md)
- Legend visibility and positioning
- Legend types (Gradient, List)
- Legend customization (width, height, alignment)
- Smart legend with toggle cell visibility
- Show percentage in legend
- Legend text style customization
- Legend title configuration
- Segment configuration
- Template support for legend items

### Tooltips
📄 **Read:** [references/tooltip.md](references/tooltip.md)
- Enabling and configuring tooltips
- Tooltip content customization
- Tooltip templates for array data
- Tooltip templates for JSON data
- Tooltip text style customization
- Border and fill customization
- Animation settings
- Conditional tooltip display

### Events & Interactivity
📄 **Read:** [references/events.md](references/events.md)
- Component lifecycle events (Load, Loaded)
- CellClick event with event arguments
- CellDoubleClick event
- CellRender event for custom cell rendering
- TooltipRender event for dynamic tooltip content
- LegendRender event for legend customization
- AxisLabelRender event for custom axis labels
- Event handling patterns and best practices
- Performance considerations with events

### Bubble HeatMap Variant
📄 **Read:** [references/bubble-heatmap.md](references/bubble-heatmap.md)
- Bubble HeatMap overview and use cases
- Enabling bubble type (TileType.Bubble)
- Bubble size configuration based on values
- Bubble size types (Absolute, Percentage)
- Bubble color mapping
- Combining bubble size and color for multi-dimensional data
- Data requirements for bubble HeatMaps
- Best practices for bubble visualizations

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Dimensions and sizing (width, height, responsive sizing)
- Cell selection modes (Cell, Column, Row)
- Selection customization
- Content Security Policy (CSP) compliance
- Accessibility features (WCAG, WAI-ARIA, keyboard navigation)
- Screen reader support
- Performance optimization for large datasets
- Virtualization strategies
- Server-side rendering considerations

### API Reference
📄 **Read:** [references/api-reference.md](references/api-reference.md)
- SfHeatMap<TValue> main component
- Core settings classes (Cell, Title, Palette, Legend, Tooltip)
- Axis classes (HeatMapXAxis, HeatMapYAxis)
- Multi-level label configuration
- Bubble configuration classes
- Styling classes (borders, text styles, margins)
- Event argument classes
- Enumerations (CellType, PaletteType, AdaptorType, etc.)
- Complete property and method reference

## Quick Start Example

Here's a minimal example to get started with the HeatMap Chart component:

```razor
@page "/heatmap"
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)"></HeatMapTitleSettings>
    <HeatMapCellSettings ShowLabel="true" TileType="CellType.Rect"></HeatMapCellSettings>
</SfHeatMap>

@code {
    int[,] GetDefaultData()
    {
        int[,] dataSource = new int[,]
        {
            {52, 65, 67, 45, 37, 52},
            {68, 52, 63, 51, 30, 51},
            {7, 16, 47, 47, 88, 6},
            {66, 64, 46, 40, 47, 41},
            {14, 46, 97, 69, 69, 3},
            {54, 46, 61, 46, 40, 39}
        };
        return dataSource;
    }
    
    public object HeatMapData { get; set; }
    
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

**Prerequisites:**
1. Install `Syncfusion.Blazor.HeatMap` NuGet package
2. Register Syncfusion service: `builder.Services.AddSyncfusionBlazor();`
3. Add theme stylesheet and script reference in `index.html`
4. Add namespace: `@using Syncfusion.Blazor.HeatMap`

## Common Patterns

### Pattern 1: JSON Data with Custom Axes
```razor
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@JsonData">
    <HeatMapDataSourceSettings IsJsonData="true" 
                               AdaptorType="AdaptorType.Table" 
                               XDataMapping="ProductName"
                               YDataMapping="Year" 
                               ValueMapping="Value">
    </HeatMapDataSourceSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
</SfHeatMap>

@code {
    private object[] JsonData = new object[]
    {
        new { ProductName = "Laptop", Year = "2022", Value = 150 },
        new { ProductName = "Desktop", Year = "2022", Value = 80 },
        // ... more data
    };
    
    private string[] XAxisLabels = new string[] { "Laptop", "Desktop", "Tablet", "Phone" };
    private string[] YAxisLabels = new string[] { "2022", "2023", "2024" };
}
```

### Pattern 2: Gradient Palette with Custom Colors
```razor
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapPaletteSettings Type="PaletteType.Gradient">
        <HeatMapPalettes>
            <HeatMapPalette Color="#C2E7EC" Value="0"></HeatMapPalette>
            <HeatMapPalette Color="#AEDFE6" Value="25"></HeatMapPalette>
            <HeatMapPalette Color="#7FCDC4" Value="50"></HeatMapPalette>
            <HeatMapPalette Color="#6EB5D0" Value="75"></HeatMapPalette>
            <HeatMapPalette Color="#2D6CA2" Value="100"></HeatMapPalette>
        </HeatMapPalettes>
    </HeatMapPaletteSettings>
</SfHeatMap>
```

### Pattern 3: Bubble HeatMap with Size and Color
```razor
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@BubbleData">
    <HeatMapCellSettings TileType="CellType.Bubble" 
                         BubbleType="BubbleType.SizeAndColor">
        <HeatMapBubbleSize Minimum="10" Maximum="30"></HeatMapBubbleSize>
    </HeatMapCellSettings>
    <HeatMapDataSourceSettings IsJsonData="true" 
                               AdaptorType="AdaptorType.Cell"
                               XDataMapping="XAxis"
                               YDataMapping="YAxis"
                               ValueMapping="Value">
        <HeatMapBubbleDataMapping Size="Size" Color="Value"></HeatMapBubbleDataMapping>
    </HeatMapDataSourceSettings>
</SfHeatMap>
```

### Pattern 4: Interactive HeatMap with Events
```razor
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapEvents CellClicked="@OnCellClick"
                   TooltipRendering="@OnTooltipRender">
    </HeatMapEvents>
    <HeatMapTooltipSettings>
        <Template>
            @{
                var data = context as TooltipEventArgs;
                <div>@data.XLabel - @data.YLabel: @data.Value</div>
            }
        </Template>
    </HeatMapTooltipSettings>
</SfHeatMap>

@code {
    private void OnCellClick(Syncfusion.Blazor.HeatMap.CellClickEventArgs args)
    {
        Console.WriteLine($"Cell clicked: {args.XLabel}, {args.YLabel}, Value: {args.Value}");
    }
    
    private void OnTooltipRender(Syncfusion.Blazor.HeatMap.TooltipEventArgs args)
    {
        // Customize tooltip content dynamically
    }
}
```

## Key Properties Reference

| Property | Type | Purpose |
|----------|------|---------|
| `DataSource` | object | Sets the data for the HeatMap (array, JSON, or cell data) |
| `Width` | string | Sets the width of the HeatMap (e.g., "100%", "600px") |
| `Height` | string | Sets the height of the HeatMap (e.g., "400px") |
| `HeatMapCellSettings.ShowLabel` | bool | Shows or hides cell labels |
| `HeatMapCellSettings.TileType` | CellType | Sets cell type (Rect or Bubble) |
| `HeatMapDataSourceSettings.IsJsonData` | bool | Indicates if data source is JSON format |
| `HeatMapDataSourceSettings.AdaptorType` | AdaptorType | Sets adaptor type (Table, Cell, None) |
| `HeatMapPaletteSettings.Type` | PaletteType | Sets palette type (Fixed or Gradient) |
| `HeatMapLegendSettings.Visible` | bool | Shows or hides legend |
| `HeatMapTooltipSettings` | object | Configures tooltip appearance and content |
| `HeatMapXAxis.ValueType` | ValueType | Sets x-axis type (Category, Numeric, DateTime) |
| `HeatMapYAxis.ValueType` | ValueType | Sets y-axis type (Category, Numeric, DateTime) |

## Common Use Cases

### Use Case 1: Sales Performance Dashboard
Visualize sales performance across products and time periods using a HeatMap with gradient colors to highlight high and low performers.

**When to use:** Monthly/quarterly sales tracking, product comparison, regional performance analysis.

### Use Case 2: Correlation Matrix
Display correlation coefficients between multiple variables in a matrix format with diverging color scheme.

**When to use:** Statistical analysis, data science projects, feature correlation analysis.

### Use Case 3: Server Resource Monitoring
Monitor server CPU, memory, or network usage across multiple servers over time periods.

**When to use:** DevOps dashboards, infrastructure monitoring, capacity planning.

### Use Case 4: Calendar HeatMap
Show activity levels or event frequencies across days and months in a calendar-style layout.

**When to use:** GitHub-style contribution graphs, attendance tracking, activity logs.

### Use Case 5: Bubble HeatMap for Multi-Dimensional Data
Use bubble size to represent one metric (e.g., sales volume) and color to represent another (e.g., profit margin).

**When to use:** Comparing two related metrics simultaneously, portfolio analysis, competitive positioning.

## Related Skills

- For TreeMap visualization, see [Syncfusion Blazor TreeMap](../syncfusion-blazor-treemap/)
- For other data visualization components, explore the Data Visualization category

## Best Practices

1. **Choose appropriate palette types**: Use gradient palettes for continuous data and fixed palettes for categorical data
2. **Enable tooltips**: Always provide tooltips for better data interpretation
3. **Use meaningful axis labels**: Ensure axis labels clearly describe the data dimensions
4. **Optimize for large datasets**: Consider data sampling or virtualization for datasets with thousands of cells
5. **Test accessibility**: Verify keyboard navigation and screen reader compatibility
6. **Responsive sizing**: Use percentage-based width for responsive layouts
7. **Color accessibility**: Ensure sufficient color contrast and avoid relying solely on color to convey information
8. **Performance**: Minimize complex event handlers and template rendering for better performance

## Troubleshooting Quick Guide

| Issue | Solution |
|-------|----------|
| HeatMap not displaying | Verify NuGet package installation and service registration |
| Data not showing | Check DataSource binding and data format matches adaptor type |
| Colors not appearing | Configure HeatMapPaletteSettings with appropriate palette type |
| Tooltips not working | Ensure HeatMapTooltipSettings is configured with valid content |
| Axis labels missing | Set Labels property on HeatMapXAxis/HeatMapYAxis |
| Performance issues | Reduce cell count, disable animations, or implement virtualization |

For detailed troubleshooting, refer to the specific reference files for each feature area.
