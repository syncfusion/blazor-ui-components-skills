# Legend Configuration for Blazor Sankey Diagram

This guide covers how to configure, position, and customize legends in the Syncfusion Blazor Sankey Diagram to enhance data interpretation and user interaction.

## Table of Contents

- [Overview](#overview)
- [Basic Legend Configuration](#basic-legend-configuration)
- [Legend Positioning](#legend-positioning)
- [Legend Sizing and Layout](#legend-sizing-and-layout)
- [Legend Appearance Customization](#legend-appearance-customization)
- [Legend Text Styling](#legend-text-styling)
- [Legend Title Configuration](#legend-title-configuration)
- [Interactive Legend Behavior](#interactive-legend-behavior)
- [Legend Item Ordering](#legend-item-ordering)
- [Complete Example: Comprehensive Legend Configuration](#complete-example-comprehensive-legend-configuration)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Key Considerations](#key-considerations)
- [Summary](#summary)

## Overview

Legends provide a visual key for interpreting nodes or links in a Sankey diagram. They present categories or elements in a data flow, associating items with descriptive labels and colors. Use legends to help users quickly identify what each color or shape represents in your flow visualization.

**When to use legends:**
- When your Sankey diagram has many nodes with different colors
- To provide a quick reference for node categories
- When colors have specific meanings in your data flow
- To enable interactive highlighting of diagram elements

## Basic Legend Configuration

Enable legends using the `SankeyLegendSettings` component nested within `SfSankey`. The `Visible` property controls whether the legend displays.

**Required namespace:**
```csharp
@using Syncfusion.Blazor.Sankey
```

**Basic example with legend enabled:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey Nodes="@Nodes" Links="@Links">
    <SankeyLegendSettings Visible="true"></SankeyLegendSettings>
</SfSankey>

@code {
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    protected override void OnInitialized()
    {
        Nodes = new List<SankeyDataNode>()
        {
            new SankeyDataNode() { Id = "Solar", Label = new SankeyDataLabel() { Text = "Solar" } },
            new SankeyDataNode() { Id = "Wind", Label = new SankeyDataLabel() { Text = "Wind" } },
            new SankeyDataNode() { Id = "Electricity", Label = new SankeyDataLabel() { Text = "Electricity" } },
            new SankeyDataNode() { Id = "Residential", Label = new SankeyDataLabel() { Text = "Residential" } }
        };

        Links = new List<SankeyDataLink>()
        {
            new SankeyDataLink() { SourceId = "Solar", TargetId = "Electricity", Value = 100 },
            new SankeyDataLink() { SourceId = "Wind", TargetId = "Electricity", Value = 120 },
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Residential", Value = 220 }
        };
    }
}
```

This displays a legend with default positioning (auto-positioned) showing all nodes in the diagram.

## Legend Positioning

Control legend position using the `Position` property. Available positions:

- **`SankeyLegendPosition.Auto`** (default) - Component decides optimal position
- **`SankeyLegendPosition.Top`** - Above the diagram
- **`SankeyLegendPosition.Bottom`** - Below the diagram
- **`SankeyLegendPosition.Left`** - Left of the diagram
- **`SankeyLegendPosition.Right`** - Right of the diagram

**Example with bottom positioning:**
```razor
<SfSankey Nodes="@Nodes" Links="@Links">
    <SankeyLegendSettings 
        Visible="true" 
        Position="SankeyLegendPosition.Bottom"
        Height="150px">
    </SankeyLegendSettings>
</SfSankey>

@code {
    // Same nodes and links as above
}
```

**Best practices for positioning:**
- Use `Bottom` or `Top` for wide diagrams with many horizontal flows
- Use `Left` or `Right` for tall diagrams with vertical orientations
- Set `Height` or `Width` explicitly to control legend container size
- Ensure the legend doesn't obscure important diagram elements

## Legend Sizing and Layout

Configure legend dimensions and spacing using these properties:

### Size Properties

- **`Width`**: Legend container width (e.g., "100%", "300px")
- **`Height`**: Legend container height (e.g., "200px", "auto")
- **`Padding`**: Outer padding around the legend collection (default: 10px)
- **`ItemPadding`**: Spacing between individual legend items (default: 10px)

### Shape Properties

- **`ShapeWidth`**: Width of legend shape/symbol (default: 15px)
- **`ShapeHeight`**: Height of legend shape/symbol (default: 15px)
- **`ShapePadding`**: Space between shape and text (default: 5px)

**Example with custom sizing:**
```razor
<SfSankey Nodes="@Nodes" Links="@Links">
    <SankeyLegendSettings 
        Visible="true" 
        Position="SankeyLegendPosition.Bottom"
        Width="100%"
        Height="180px"
        Padding="15"
        ItemPadding="12"
        ShapeWidth="18"
        ShapeHeight="18"
        ShapePadding="8">
    </SankeyLegendSettings>
</SfSankey>

@code {
    // Nodes and links
}
```

## Legend Appearance Customization

Customize the visual appearance of the legend container:

### Background and Border

- **`Background`**: Background color (e.g., "#f0f0f0", "rgba(240,240,240,0.8)")
- **`Opacity`**: Background transparency (0 to 1, default: 1)
- **`BorderColor`**: Border color (e.g., "#cccccc")
- **`BorderWidth`**: Border thickness in pixels (default: 1)

**Example with styled background:**
```razor
<SfSankey Nodes="@Nodes" Links="@Links">
    <SankeyLegendSettings 
        Visible="true" 
        Position="SankeyLegendPosition.Bottom"
        Height="200px"
        Background="#f8f9fa"
        Opacity="0.95"
        BorderColor="#dee2e6"
        BorderWidth="2">
    </SankeyLegendSettings>
</SfSankey>

@code {
    // Nodes and links
}
```

### Margins

Use `SankeyLegendMargin` to control spacing around the legend:

```razor
<SfSankey Nodes="@Nodes" Links="@Links">
    <SankeyLegendSettings 
        Visible="true" 
        Position="SankeyLegendPosition.Bottom">
        <SankeyLegendMargin 
            Left="20" 
            Right="20" 
            Top="20" 
            Bottom="20">
        </SankeyLegendMargin>
    </SankeyLegendSettings>
</SfSankey>

@code {
    // Nodes and links
}
```

## Legend Text Styling

Customize legend text appearance using `SankeyLegendTextStyle`:

**Available properties:**
- **`FontSize`**: Text size (e.g., "12px", "14px")
- **`FontFamily`**: Font family (e.g., "Arial", "Segoe UI")
- **`FontWeight`**: Text weight (e.g., "400", "600", "bold")
- **`FontStyle`**: Text style ("normal", "italic", "oblique")
- **`Color`**: Text color (e.g., "#333333", "rgb(100,100,100)")

**Example with custom text styling:**
```razor
<SfSankey Nodes="@Nodes" Links="@Links">
    <SankeyLegendSettings Visible="true">
        <SankeyLegendTextStyle 
            FontSize="13px" 
            FontFamily="Segoe UI" 
            FontWeight="500" 
            Color="#495057"
            FontStyle="normal">
        </SankeyLegendTextStyle>
    </SankeyLegendSettings>
</SfSankey>

@code {
    // Nodes and links
}
```

## Legend Title Configuration

Add a title to your legend using the `Title` property, and customize its appearance with `SankeyLegendTitleStyle`:

**Example with legend title:**
```razor
<SfSankey Nodes="@Nodes" Links="@Links">
    <SankeyLegendSettings 
        Visible="true" 
        Title="Energy Sources">
        <SankeyLegendTitleStyle 
            FontSize="15px" 
            FontFamily="Segoe UI" 
            FontWeight="600" 
            Color="#212529"
            FontStyle="normal">
        </SankeyLegendTitleStyle>
    </SankeyLegendSettings>
</SfSankey>

@code {
    // Nodes and links
}
```

## Interactive Legend Behavior

### Highlight on Hover

Enable interactive highlighting using `EnableHighlight`. When true, hovering over a legend item highlights the corresponding elements in the Sankey diagram.

**Example with highlight enabled:**
```razor
<SfSankey Nodes="@Nodes" Links="@Links">
    <SankeyLegendSettings 
        Visible="true" 
        EnableHighlight="true">
    </SankeyLegendSettings>
</SfSankey>

@code {
    // Nodes and links
}
```

**Benefits of EnableHighlight:**
- Helps users quickly identify which flows belong to a specific node
- Provides visual feedback for exploration
- Improves understanding of complex multi-node relationships

## Legend Item Ordering

Control the order and arrangement of legend items:

### Reverse Item Order

- **`Reverse`**: Reverses the display order of legend items (default: false)

**Example:**
```razor
<SankeyLegendSettings 
    Visible="true" 
    Reverse="true">
</SankeyLegendSettings>
```

### Inverse Shape-Text Order

- **`IsInversed`**: Swaps the position of shape and text in legend items (default: false)

**Example with inversed items:**
```razor
<SankeyLegendSettings 
    Visible="true" 
    IsInversed="true">
</SankeyLegendSettings>
```

When `IsInversed` is true, text appears before the shape symbol instead of after.

## Complete Example: Comprehensive Legend Configuration

```razor
@using Syncfusion.Blazor.Sankey

<SfSankey Nodes="@Nodes" Links="@Links" Width="100%" Height="600px">
    <SankeyLegendSettings 
        Visible="true" 
        Position="SankeyLegendPosition.Bottom"
        Width="100%"
        Height="200px"
        Background="#f8f9fa"
        Opacity="0.95"
        BorderColor="#dee2e6"
        BorderWidth="2"
        Padding="15"
        ItemPadding="12"
        ShapeWidth="18"
        ShapeHeight="18"
        ShapePadding="8"
        EnableHighlight="true"
        Title="Energy Flow Categories"
        IsInversed="false"
        Reverse="false">
        <SankeyLegendMargin 
            Left="20" 
            Right="20" 
            Top="20" 
            Bottom="20">
        </SankeyLegendMargin>
        <SankeyLegendTextStyle 
            FontSize="13px" 
            FontFamily="Segoe UI" 
            FontWeight="500" 
            Color="#495057"
            FontStyle="normal">
        </SankeyLegendTextStyle>
        <SankeyLegendTitleStyle 
            FontSize="15px" 
            FontFamily="Segoe UI" 
            FontWeight="600" 
            Color="#212529"
            FontStyle="normal">
        </SankeyLegendTitleStyle>
    </SankeyLegendSettings>
</SfSankey>

@code {
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    protected override void OnInitialized()
    {
        Nodes = new List<SankeyDataNode>()
        {
            new SankeyDataNode() { Id = "Solar", Label = new SankeyDataLabel() { Text = "Solar" } },
            new SankeyDataNode() { Id = "Wind", Label = new SankeyDataLabel() { Text = "Wind" } },
            new SankeyDataNode() { Id = "Hydro", Label = new SankeyDataLabel() { Text = "Hydro" } },
            new SankeyDataNode() { Id = "Electricity", Label = new SankeyDataLabel() { Text = "Electricity" } },
            new SankeyDataNode() { Id = "Residential", Label = new SankeyDataLabel() { Text = "Residential" } },
            new SankeyDataNode() { Id = "Commercial", Label = new SankeyDataLabel() { Text = "Commercial" } }
        };

        Links = new List<SankeyDataLink>()
        {
            new SankeyDataLink() { SourceId = "Solar", TargetId = "Electricity", Value = 100 },
            new SankeyDataLink() { SourceId = "Wind", TargetId = "Electricity", Value = 120 },
            new SankeyDataLink() { SourceId = "Hydro", TargetId = "Electricity", Value = 80 },
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Residential", Value = 170 },
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Commercial", Value = 130 }
        };
    }
}
```

## Best Practices

1. **Position Selection**: Choose positions that don't overlap with important diagram content
2. **Size Appropriately**: Ensure legend has enough space for all items without crowding
3. **Consistent Styling**: Match legend fonts and colors with your application theme
4. **Enable Highlight**: Use `EnableHighlight="true"` for better user interaction
5. **Title Usage**: Add descriptive titles when legend items need additional context
6. **Margin Control**: Use margins to create visual separation from the diagram
7. **Test Responsiveness**: Verify legend layout works at different screen sizes

## Troubleshooting

### Legend Not Visible

**Problem**: Legend doesn't appear even with `Visible="true"`

**Solutions**:
- Ensure you have nodes in your data
- Check that `Height` or `Width` isn't set too small
- Verify the legend isn't positioned outside the visible area

### Legend Items Overlapping

**Problem**: Legend items overlap or appear cramped

**Solutions**:
- Increase `Height` property for bottom/top positions
- Increase `Width` property for left/right positions
- Adjust `ItemPadding` to add more space between items
- Reduce `ShapeWidth` and `ShapeHeight` if items are too large

### Legend Text Cut Off

**Problem**: Legend text is truncated

**Solutions**:
- Increase legend container `Width` or `Height`
- Use shorter node labels in your data
- Adjust `FontSize` in `SankeyLegendTextStyle` to make text smaller
- Consider using a different `Position` with more available space

### Highlight Not Working

**Problem**: Hovering over legend items doesn't highlight diagram elements

**Solutions**:
- Verify `EnableHighlight="true"` is set
- Ensure node IDs in your data match the legend items
- Check browser console for JavaScript errors

## Key Considerations

- Keep legend items concise and unambiguous for quick scanning
- Choose positions that avoid overlap with primary diagram content
- Align legend styling with your application theme for consistency
- Use `EnableHighlight` to assist targeted exploration of flows
- Adjust `IsInversed` or `Reverse` to optimize reading order when needed
- Test legend appearance across different screen sizes and orientations

## Summary

Legends enhance Sankey diagram comprehension by providing a visual key for interpreting nodes and flows. Use `SankeyLegendSettings` to control visibility, position, size, and appearance. Enable interactive highlighting with `EnableHighlight` to help users explore complex diagrams. Customize text styles and add titles to match your application's design language and provide clear context for your data flows.
