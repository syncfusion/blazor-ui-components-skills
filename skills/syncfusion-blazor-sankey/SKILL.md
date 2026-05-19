---
name: syncfusion-blazor-sankey
description: Comprehensive guide for implementing Syncfusion Blazor Sankey Diagram (SfSankey) component for flow visualization in Blazor applications. Use this when working with Sankey diagrams, flow visualization, or process flows. This skill covers node and link configuration, data binding, flow magnitude representation, and diagram customization. Ideal for visualizing energy flows, supply chain relationships, user journey maps, traffic analysis, and any scenario involving flows between categories or stages.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Data Visualization"
---

# Implementing Syncfusion Blazor Sankey Diagram

**NuGet:** `Syncfusion.Blazor.Sankey` + `Syncfusion.Blazor.Themes`  
**Namespace:** `Syncfusion.Blazor.Sankey`

A comprehensive skill for implementing the Syncfusion Blazor Sankey Diagram component. The Sankey diagram is a powerful visualization tool for displaying flows and relationships between categories, showing the magnitude and direction of flows using proportionally-sized links.

## When to Use This Skill

Use this skill when you need to:
- **Visualize flows and relationships** between different categories or stages
- **Display energy or resource flows** in systems (electricity, water, materials)
- **Show data movements** between entities (website traffic, user journeys, data pipelines)
- **Illustrate budget or financial flows** (income/expenses, resource allocation)
- **Map supply chain or logistics** flows (product movement, distribution)
- **Create process flow diagrams** with quantifiable magnitudes
- **Analyze network flows** (data transfer, communication patterns)
- **Display conversion funnels** with drop-off visualization
- Implement the `SfSankey` Blazor component
- Work with flow data structures (nodes and links)
- Customize Sankey diagram appearance and behavior
- Handle interactive events for flow exploration
- Export or print flow diagrams

## Component Overview

### What is a Sankey Diagram?

A Sankey diagram is a flow diagram where the width of arrows or connections is proportional to the flow magnitude. It consists of:

- **Nodes**: Entities, categories, or stages in the flow (e.g., energy sources, process steps)
- **Links**: Connections between nodes showing flow direction and magnitude
- **Flow Width**: Visual representation of quantity/magnitude (wider = larger flow)

### Key Features

- **Data-driven visualization** with nodes and links collections
- **Automatic layout** with configurable orientation (horizontal/vertical)
- **Rich customization** for nodes, links, labels, and legends
- **Interactive events** for clicks, hovers, and rendering customization
- **Tooltips** showing flow details on hover
- **Print and export** functionality (PNG, JPEG, SVG, PDF)
- **Accessibility** compliant with WCAG 2.2 standards
- **Responsive design** with dynamic resizing

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)  
**When to read:** First-time setup, installation, project configuration  
**Contains:**
- NuGet package installation (Syncfusion.Blazor.Sankey)
- Service registration and namespace imports
- CSS theme and script references
- Basic Sankey diagram implementation
- Complete minimal working example
- Setup troubleshooting

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)  
**When to read:** Setting up data structure, binding nodes and links, API integration  
**Contains:**
- Understanding SankeyDataNode and SankeyDataLink models
- Creating nodes and links collections
- Binding data to the Sankey component
- REST API integration patterns
- JSON data structure examples
- Dynamic data updates

### Nodes Configuration
📄 **Read:** [references/nodes.md](references/nodes.md)  
**When to read:** Customizing node appearance, styling, properties  
**Contains:**
- Node fundamentals and structure
- Node ID and label configuration
- Node styling with SankeyNodeSettings
- Color, width, and padding properties
- Node positioning and layout
- Complete node examples

### Links Configuration
📄 **Read:** [references/links.md](references/links.md)  
**When to read:** Configuring flow connections, link styling, magnitude representation  
**Contains:**
- Link fundamentals and flow representation
- SourceId, TargetId, and Value properties
- Link styling with SankeyLinkSettings
- Color, opacity, and stroke properties
- Flow direction and magnitude
- Multiple path flow examples

### Labels Customization
📄 **Read:** [references/labels.md](references/labels.md)  
**When to read:** Customizing node labels, text formatting  
**Contains:**
- Label configuration and positioning
- Font properties (size, weight, family, color)
- Text formatting and truncation
- Label visibility controls
- Custom label examples

### Legends Configuration
📄 **Read:** [references/legends.md](references/legends.md)  
**When to read:** Adding and customizing diagram legends  
**Contains:**
- Legend configuration with SankeyLegendSettings
- Visibility and positioning options
- Legend item customization
- Shape and color mapping
- Interactive legend behavior
- LegendItemRendering event

### Tooltips
📄 **Read:** [references/tooltips.md](references/tooltips.md)  
**When to read:** Adding interactive tooltips, customizing tooltip content  
**Contains:**
- Tooltip configuration and triggers
- Custom tooltip templates
- Node and link tooltip differentiation
- TooltipRendering event
- Formatting tooltip content
- Styling examples

### API Reference
📄 **Read:** [references/api-reference.md](references/api-reference.md)  
**When to read:** Looking up component properties, events, methods, and enums  
**Contains:**
- Complete SfSankey component API documentation
- All properties with types and defaults
- All 16 events with event arguments
- All public methods (Export, Print, Refresh)
- Data model classes (SankeyDataNode, SankeyDataLink, SankeyDataLabel)
- Settings components (Node, Link, Label, Legend, Tooltip)
- Enums and event arguments
- Full component hierarchy
- Complete working examples

### Customization
📄 **Read:** [references/customization.md](references/customization.md)  
**When to read:** Advanced styling, layout options, themes, RTL support  
**Contains:**
- Background styling (color and image)
- Diagram dimensions and sizing
- Layout orientation (horizontal/vertical)
- RTL (Right-to-Left) support
- Theme integration
- Custom CSS classes
- Responsive design patterns

### Events and Interactivity
📄 **Read:** [references/events.md](references/events.md)  
**When to read:** Handling user interactions, customizing rendering, responding to changes  
**Contains:**
- Complete event reference (16 events)
- Rendering events (NodeRendering, LinkRendering, LabelRendering)
- Interaction events (NodeClick, LinkClick)
- Hover events (NodeEnter/Leave, LinkEnter/Leave)
- Lifecycle events (Created, SizeChanged)
- Export events (PrintCompleted, ExportCompleted)
- Event argument structures and examples

### Print and Export
📄 **Read:** [references/print-export.md](references/print-export.md)  
**When to read:** Exporting diagrams to images/PDF, printing functionality  
**Contains:**
- Printing Sankey diagrams
- Export formats (PNG, JPEG, SVG, PDF)
- Export method configuration
- File naming and quality settings
- Print/export events
- Complete working examples

## Quick Start Example

Here's a minimal example showing energy source flow to energy types and end-use sectors:

```razor
@page "/sankey-demo"
@using Syncfusion.Blazor.Sankey

<SfSankey Width="100%" Height="600px" Nodes="@Nodes" Links="@Links">
    <SankeyNodeSettings Width="20" Padding="20"></SankeyNodeSettings>
    <SankeyLinkSettings Opacity="0.4"></SankeyLinkSettings>
</SfSankey>

@code {
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    protected override void OnInitialized()
    {
        // Define nodes
        Nodes = new List<SankeyDataNode>()
        {
            new SankeyDataNode() { Id = "Solar", Label = new SankeyDataLabel() { Text = "Solar" } },
            new SankeyDataNode() { Id = "Wind", Label = new SankeyDataLabel() { Text = "Wind" } },
            new SankeyDataNode() { Id = "Hydro", Label = new SankeyDataLabel() { Text = "Hydro" } },
            new SankeyDataNode() { Id = "Electricity", Label = new SankeyDataLabel() { Text = "Electricity" } },
            new SankeyDataNode() { Id = "Heat", Label = new SankeyDataLabel() { Text = "Heat" } },
            new SankeyDataNode() { Id = "Residential", Label = new SankeyDataLabel() { Text = "Residential" } },
            new SankeyDataNode() { Id = "Commercial", Label = new SankeyDataLabel() { Text = "Commercial" } },
            new SankeyDataNode() { Id = "Industrial", Label = new SankeyDataLabel() { Text = "Industrial" } }
        };

        // Define links (flows)
        Links = new List<SankeyDataLink>()
        {
            new SankeyDataLink() { SourceId = "Solar", TargetId = "Electricity", Value = 30 },
            new SankeyDataLink() { SourceId = "Wind", TargetId = "Electricity", Value = 45 },
            new SankeyDataLink() { SourceId = "Hydro", TargetId = "Electricity", Value = 25 },
            new SankeyDataLink() { SourceId = "Solar", TargetId = "Heat", Value = 15 },
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Residential", Value = 35 },
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Commercial", Value = 30 },
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Industrial", Value = 35 },
            new SankeyDataLink() { SourceId = "Heat", TargetId = "Residential", Value = 10 },
            new SankeyDataLink() { SourceId = "Heat", TargetId = "Commercial", Value = 5 }
        };
    }
}
```

## Common Patterns

### Pattern 1: Simple Flow Diagram
```razor
<SfSankey Nodes="@Nodes" Links="@Links" Width="800px" Height="400px">
</SfSankey>
```

### Pattern 2: Styled Sankey with Custom Colors
```razor
<SfSankey Nodes="@Nodes" Links="@Links" BackgroundColor="#f5f5f5">
    <SankeyNodeSettings Color="#2e7d32" Width="15" Padding="25"></SankeyNodeSettings>
    <SankeyLinkSettings Color="#1976d2" Opacity="0.3"></SankeyLinkSettings>
    <SankeyLabelSettings Color="#000000" FontSize="14px" FontWeight="500"></SankeyLabelSettings>
</SfSankey>
```

### Pattern 3: Interactive Sankey with Events
```razor
<SfSankey Nodes="@Nodes" Links="@Links" NodeClick="OnNodeClick" LinkClick="OnLinkClick">
    <SankeyTooltipSettings Visible="true"></SankeyTooltipSettings>
</SfSankey>

@code {
    private void OnNodeClick(NodeClickEventArgs args)
    {
        Console.WriteLine($"Node clicked: {args.Node.Id}");
    }

    private void OnLinkClick(LinkClickEventArgs args)
    {
        Console.WriteLine($"Link clicked: {args.Link.SourceId} -> {args.Link.TargetId}");
    }
}
```

### Pattern 4: Vertical Orientation
```razor
<SfSankey Nodes="@Nodes" Links="@Links" Orientation="SankeyOrientation.Vertical">
    <SankeyNodeSettings Width="30" Padding="15"></SankeyNodeSettings>
</SfSankey>
```

### Pattern 5: With Legend
```razor
<SfSankey Nodes="@Nodes" Links="@Links">
    <SankeyLegendSettings Visible="true" Position="LegendPosition.Bottom"></SankeyLegendSettings>
</SfSankey>
```

## Complete API Properties Reference

### SfSankey Component Properties

#### Data Properties
| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Nodes` | `List<SankeyDataNode>` | null | Collection of nodes (entities/categories) |
| `Links` | `List<SankeyDataLink>` | null | Collection of links (flows between nodes) |

#### Layout & Sizing Properties
| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Width` | `string` | "100%" | Diagram width (pixels or percentage) |
| `Height` | `string` | "100%" | Diagram height (pixels or percentage) |
| `Orientation` | `SankeyOrientation` | Auto | Layout direction (Auto/Horizontal/Vertical) |
| `EnableAutoLayout` | `bool` | true | Enable/disable automatic layout |

#### Styling Properties
| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `BackgroundColor` | `string` | Theme default | Background color |
| `BackgroundImage` | `string` | "" | Background image URL |
| `BorderColor` | `string` | null | Border color |
| `BorderWidth` | `double` | 1 | Border width in pixels |
| `Theme` | `Theme` | Fluent2 | Chart theme |
| `CssClass` | `string` | "" | Custom CSS class |

#### Title & Content Properties
| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Title` | `string` | "" | Main title text |
| `SubTitle` | `string` | "" | Subtitle text |
| `LinkCurvature` | `double` | 1 | Link curve intensity (0-1) |

#### Animation Properties
| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `EnableAnimation` | `bool` | true | Enable/disable animations |
| `AnimationDuration` | `double` | 2000 | Animation duration (ms) |
| `AnimationDelay` | `double` | 0 | Animation delay (ms) |

#### Accessibility & Interaction Properties
| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `EnableRTL` | `bool` | false | Enable right-to-left layout |
| `TabIndex` | `double` | 0 | Keyboard tab index |
| `AccessibilityDescription` | `string` | "" | Accessibility description |
| `ID` | `string` | Auto-generated | Component ID |

### Events Reference (16 Total Events)

#### Rendering Events
- **`NodeRendering`** - `SankeyNodeRenderEventArgs` - Fired when a node is rendered
- **`LinkRendering`** - `SankeyLinkRenderEventArgs` - Fired when a link is rendered
- **`LabelRendering`** - `SankeyLabelRenderEventArgs` - Fired when a label is rendered
- **`LegendItemRendering`** - `SankeyLegendRenderEventArgs` - Fired before legend item renders

#### Mouse Interaction Events
- **`NodeClick`** - `SankeyNodeEventArgs` - Fired when node is clicked
- **`LinkClick`** - `SankeyLinkEventArgs` - Fired when link is clicked
- **`NodeEnter`** - `SankeyNodeEventArgs` - Fired when mouse enters node
- **`NodeLeave`** - `SankeyNodeEventArgs` - Fired when mouse leaves node
- **`LinkEnter`** - `SankeyLinkEventArgs` - Fired when mouse enters link
- **`LinkLeave`** - `SankeyLinkEventArgs` - Fired when mouse leaves link

#### UI Events
- **`TooltipRendering`** - `SankeyTooltipRenderEventArgs` - Fired when tooltip renders
- **`LegendItemHover`** - `SankeyLegendItemHoverEventArgs` - Fired on legend hover

#### Lifecycle Events
- **`Created`** - `Action` - Fired when component is created
- **`SizeChanged`** - `SankeySizeChangedEventArgs` - Fired when component size changes

#### Export/Print Events
- **`PrintCompleted`** - `Action` - Fired when print completes
- **`ExportCompleted`** - `SankeyExportedEventArgs` - Fired when export completes

### Public Methods

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `ExportAsync` | `type (SankeyExportType), fileName, orientation?, allowDownload?, isBase64?` | `Task` | Export to PNG/JPEG/SVG/PDF |
| `PrintAsync` | `elementRef?` | `Task` | Print the diagram |
| `RefreshAsync` | None | `Task` | Re-render component |

### Settings Objects

- **SankeyNodeSettings**: Node appearance (Width, Padding, Alignment, Color, Offset)
- **SankeyLinkSettings**: Link appearance (Color, Opacity, HighlightOpacity, InactiveOpacity, ColorType)
- **SankeyLabelSettings**: Label styling (Visible, FontSize, FontFamily, FontWeight, Color, Padding)
- **SankeyMargin**: Chart margins (Left, Right, Top, Bottom)
- **SankeyTitleStyle**: Title styling (FontSize, FontFamily, FontWeight, FontStyle, Color)
- **SankeySubtitleStyle**: Subtitle styling (FontSize, FontFamily, FontWeight, FontStyle, Color)
- **SankeyLegendSettings**: Legend configuration (Visible, Position, Height, Width)
- **SankeyTooltipSettings**: Tooltip configuration (Enable, Fill, Opacity)

### Data Models

| Model | Key Properties | Description |
|-------|---|-------------|
| **SankeyDataNode** | `Id`, `Label` (SankeyDataLabel), `Color`, `Width` | Represents a node |
| **SankeyDataLabel** | `Text` | Node label text |
| **SankeyDataLink** | `SourceId`, `TargetId`, `Value`, `Color`, `Opacity` | Represents a link/connection |

### Enums

| Enum | Values | Description |
|------|--------|-------------|
| **SankeyOrientation** | Auto, Horizontal, Vertical | Layout direction |
| **SankeyNodeAlign** | None, Left, Center, Right | Node alignment |
| **SankeyColorType** | Static, Source | Color mapping type |
| **SankeyLegendPosition** | Top, Bottom, Left, Right | Legend position |
| **SankeyExportType** | PNG, JPEG, SVG, PDF | Export format |
| **Theme** | Bootstrap5, Fluent2, Material, Tailwind, Fabric, etc. | Chart theme |

## Common Use Cases

### Use Case 1: Energy Flow Visualization
Visualize energy sources flowing to energy types and then to end-use sectors (residential, commercial, industrial).

### Use Case 2: Website Traffic Analysis
Show user traffic flow from entry points (search, social, direct) through pages to conversion goals.

### Use Case 3: Budget Allocation
Display budget flows from income sources through categories to final expenses.

### Use Case 4: Supply Chain Flow
Illustrate product movement from suppliers through manufacturing to distribution channels.

### Use Case 5: User Journey Mapping
Map user flow through application states, showing drop-offs and conversions at each stage.

### Use Case 6: Data Pipeline Visualization
Show data flow through processing stages in ETL pipelines or data transformation workflows.

## Data Structure Requirements

### Nodes
Each node must have:
- Unique `Id` (string)
- `Label` with `Text` property

### Links
Each link must have:
- `SourceId` matching a node Id
- `TargetId` matching a node Id
- `Value` representing flow magnitude (numeric)

### Validation Tips
- Ensure all link SourceId and TargetId reference existing node Ids
- Values should be positive numbers
- Avoid circular flows (node A → B → A) for clarity
- Node Ids are case-sensitive

## Troubleshooting Quick Tips

**Diagram not showing:**
- Verify Nodes and Links are properly initialized
- Check that SfSankey has Width and Height set
- Ensure Syncfusion services are registered in Program.cs

**Links not appearing:**
- Confirm SourceId and TargetId match existing node Ids exactly
- Check that Value is greater than 0
- Verify Links collection is bound to component

**Styling not applying:**
- Ensure theme CSS is referenced in App.razor or layout
- Check for conflicting CSS styles
- Verify color format is valid (hex, rgb, named color)

**Events not firing:**
- Confirm event handler method signature matches event args type
- Check that event is properly bound in component markup
- Verify component interactivity mode for .NET 8+ projects

## Next Steps

For detailed implementation of specific features, refer to the reference files listed in the Navigation Guide above. Each reference provides complete, self-contained documentation with examples and best practices.

## Resources

### Documentation & References
- **Official Documentation**: https://blazor.syncfusion.com/documentation/sankey-chart/getting-started
- **Component Demos**: https://blazor.syncfusion.com/demos/sankey-chart/default-functionalities
- **Official API Reference**: https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Sankey.html
- **Local API Reference**: [references/api-reference.md](references/api-reference.md) - Comprehensive local API documentation with all properties, events, methods, and examples

### Community & Support
- **GitHub Examples**: https://github.com/SyncfusionExamples/
- **Support**: https://www.syncfusion.com/support/
- **NuGet Package**: https://www.nuget.org/packages/Syncfusion.Blazor.Sankey/
