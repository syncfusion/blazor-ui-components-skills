# Syncfusion Blazor Sankey API Reference

Complete API reference for the Syncfusion Blazor Sankey component. Reference: https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Sankey.html

## Table of Contents

- [Namespace: `Syncfusion.Blazor.Sankey`](#namespace-syncfusionblazorsankey)
- [Main Component](#main-component)
- [Data Classes](#data-classes)
- [Settings Components](#settings-components)
- [Enums](#enums)
- [Event Arguments](#event-arguments)
- [Context Classes](#context-classes)
- [Component Hierarchy](#component-hierarchy)
- [Complete Example with All API Properties](#complete-example-with-all-api-properties)
- [Resource Links](#resource-links)

## Namespace: `Syncfusion.Blazor.Sankey`

---

## Main Component

### SfSankey

The main Sankey chart component for creating flow diagrams. Inherits from `SfBaseComponent`.

**Assembly**: Syncfusion.Blazor.dll

#### Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Nodes` | `List<SankeyDataNode>` | null | Collection of nodes (entities/categories) in the diagram |
| `Links` | `List<SankeyDataLink>` | null | Collection of links (connections) between nodes |
| `Width` | `string` | "100%" | Width of the chart (pixels or percentage) |
| `Height` | `string` | "100%" | Height of the chart (pixels or percentage) |
| `Title` | `string` | "" | Main title of the diagram |
| `SubTitle` | `string` | "" | Subtitle of the diagram |
| `Orientation` | `SankeyOrientation` | Auto | Layout orientation (Auto, Horizontal, Vertical) |
| `BackgroundColor` | `string` | Theme default | Background color (hex, rgb, or named color) |
| `BackgroundImage` | `string` | "" | Background image URL |
| `BorderColor` | `string` | null | Border color of the chart |
| `BorderWidth` | `double` | 1 | Width of the border in pixels |
| `LinkCurvature` | `double` | 1 | Curvature of links (0-1, where 0 = straight) |
| `EnableAnimation` | `bool` | true | Enable/disable animations |
| `AnimationDuration` | `double` | 2000 | Duration of animation in milliseconds |
| `AnimationDelay` | `double` | 0 | Delay before animation starts in milliseconds |
| `EnableAutoLayout` | `bool` | true | Enable/disable automatic layout |
| `EnableRTL` | `bool` | false | Enable/disable right-to-left layout |
| `Theme` | `Theme` | Fluent2 | Chart theme (Fluent2, Material, Bootstrap5, etc.) |
| `ID` | `string` | Auto-generated | Unique identifier for the component |
| `CssClass` | `string` | "" | Custom CSS class name |
| `TabIndex` | `double` | 0 | Tab index for keyboard navigation |
| `AccessibilityDescription` | `string` | "" | Accessibility description for screen readers |

#### Events

| Event | Event Args | Description |
|-------|-----------|-------------|
| `Created` | `Action` | Raised when the component is created |
| `NodeClick` | `SankeyNodeEventArgs` | Raised when a node is clicked |
| `NodeEnter` | `SankeyNodeEventArgs` | Raised when mouse enters a node |
| `NodeLeave` | `SankeyNodeEventArgs` | Raised when mouse leaves a node |
| `NodeRendering` | `SankeyNodeRenderEventArgs` | Raised when a node is rendered |
| `LinkClick` | `SankeyLinkEventArgs` | Raised when a link is clicked |
| `LinkEnter` | `SankeyLinkEventArgs` | Raised when mouse enters a link |
| `LinkLeave` | `SankeyLinkEventArgs` | Raised when mouse leaves a link |
| `LinkRendering` | `SankeyLinkRenderEventArgs` | Raised when a link is rendered |
| `LabelRendering` | `SankeyLabelRenderEventArgs` | Raised when a label is rendered |
| `LegendItemRendering` | `SankeyLegendRenderEventArgs` | Raised before each legend item is rendered |
| `LegendItemHover` | `SankeyLegendItemHoverEventArgs` | Raised when mouse hovers over legend item |
| `TooltipRendering` | `SankeyTooltipRenderEventArgs` | Raised when a tooltip is rendered |
| `SizeChanged` | `SankeySizeChangedEventArgs` | Raised when component size changes |
| `PrintCompleted` | `Action` | Raised when print operation completes |
| `ExportCompleted` | `SankeyExportedEventArgs` | Raised when export operation completes |

#### Methods

| Method | Parameters | Returns | Description |
|--------|-----------|---------|-------------|
| `ExportAsync` | `SankeyExportType type, string fileName, PdfPageOrientation? orientation = null, bool allowDownload = true, bool isBase64 = false` | `Task` | Export the chart to PNG, JPEG, SVG, or PDF |
| `PrintAsync` | `ElementReference elementRef = default` | `Task` | Print the chart |
| `RefreshAsync` | None | `Task` | Re-render the component |

---

## Data Classes

### SankeyDataNode

Represents a node in the Sankey diagram.

```csharp
public class SankeyDataNode
{
    public string Id { get; set; }                      // Unique identifier
    public SankeyDataLabel Label { get; set; }          // Node label
    public string Color { get; set; }                   // Node color
    public double? Width { get; set; }                  // Node width
}
```

### SankeyDataLabel

Represents the label of a node.

```csharp
public class SankeyDataLabel
{
    public string Text { get; set; }                    // Label text
}
```

### SankeyDataLink

Represents a link (connection) between two nodes.

```csharp
public class SankeyDataLink
{
    public string SourceId { get; set; }                // Source node ID
    public string TargetId { get; set; }                // Target node ID
    public double Value { get; set; }                   // Flow magnitude
    public string Color { get; set; }                   // Link color
    public double? Opacity { get; set; }                // Link opacity
}
```

---

## Settings Components

### SankeyNodeSettings

Configuration for node appearance and behavior.

**Child of**: `SfSankey`

```razor
<SankeyNodeSettings Width="20" Padding="20" Alignment="SankeyNodeAlign.Left" />
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Width` | `double` | 20 | Width of nodes in pixels |
| `Padding` | `double` | 20 | Padding between nodes in pixels |
| `Alignment` | `SankeyNodeAlign` | None | Node alignment (None, Left, Center, Right) |
| `Offset` | `double` | 0 | Node offset |
| `Color` | `string` | "" | Node fill color |

### SankeyLinkSettings

Configuration for link appearance and behavior.

**Child of**: `SfSankey`

```razor
<SankeyLinkSettings Color="blue" Opacity="0.5" HighlightOpacity="1" />
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Color` | `string` | "" | Link fill color |
| `Opacity` | `double` | 0.5 | Link opacity (0-1) |
| `HighlightOpacity` | `double` | 1 | Opacity when highlighted |
| `InactiveOpacity` | `double` | 0.3 | Opacity when inactive |
| `ColorType` | `SankeyColorType` | Static | Color mapping (Static, Source) |

### SankeyLabelSettings

Configuration for node label appearance.

**Child of**: `SfSankey`

```razor
<SankeyLabelSettings Visible="true" FontSize="12" Color="black" />
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Visible` | `bool` | true | Show/hide labels |
| `FontSize` | `string` | "12px" | Label font size |
| `FontFamily` | `string` | "Arial" | Label font family |
| `FontWeight` | `string` | "400" | Label font weight |
| `Color` | `string` | "" | Label text color |
| `Padding` | `double` | 0 | Padding around label |

### SankeyMargin

Configuration for chart margins.

**Child of**: `SfSankey`

```razor
<SankeyMargin Left="10" Right="10" Top="10" Bottom="10" />
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Left` | `double` | 0 | Left margin in pixels |
| `Right` | `double` | 0 | Right margin in pixels |
| `Top` | `double` | 0 | Top margin in pixels |
| `Bottom` | `double` | 0 | Bottom margin in pixels |

### SankeyTitleStyle

Configuration for chart title styling.

**Child of**: `SfSankey`

```razor
<SankeyTitleStyle FontSize="16px" FontWeight="800" FontFamily="Roboto" />
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `FontSize` | `string` | "16px" | Title font size |
| `FontFamily` | `string` | "Segoe UI" | Title font family |
| `FontWeight` | `string` | "500" | Title font weight |
| `FontStyle` | `string` | "normal" | Title font style |
| `Color` | `string` | "" | Title text color |

### SankeySubtitleStyle

Configuration for chart subtitle styling.

**Child of**: `SfSankey`

```razor
<SankeySubtitleStyle FontSize="12px" FontWeight="400" FontFamily="Roboto" />
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `FontSize` | `string` | "12px" | Subtitle font size |
| `FontFamily` | `string` | "Segoe UI" | Subtitle font family |
| `FontWeight` | `string` | "400" | Subtitle font weight |
| `FontStyle` | `string` | "normal" | Subtitle font style |
| `Color` | `string` | "" | Subtitle text color |

### SankeyLegendSettings

Configuration for legend appearance and behavior.

**Child of**: `SfSankey`

```razor
<SankeyLegendSettings Visible="true" Position="SankeyLegendPosition.Bottom" />
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Visible` | `bool` | false | Show/hide legend |
| `Position` | `SankeyLegendPosition` | Bottom | Legend position (Top, Bottom, Left, Right) |
| `Height` | `double` | 0 | Legend item height |
| `Width` | `double` | 0 | Legend item width |

**Child Elements**:
- `SankeyLegendMargin` - Legend margins
- `SankeyLegendTextStyle` - Legend text styling
- `SankeyLegendTitleStyle` - Legend title styling

### SankeyTooltipSettings

Configuration for tooltip behavior and appearance.

**Child of**: `SfSankey`

```razor
<SankeyTooltipSettings Enable="true" Fill="blue" Opacity="0.8" />
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Enable` | `bool` | false | Show/hide tooltips |
| `Fill` | `string` | "" | Tooltip background color |
| `Opacity` | `double` | 1 | Tooltip opacity |

**Child Templates**:
- `SankeyNodeTemplate` - Custom node tooltip template
- `SankeyLinkTemplate` - Custom link tooltip template

---

## Enums

### SankeyOrientation

Specifies the layout orientation.

```csharp
public enum SankeyOrientation
{
    Auto = 0,           // Auto-detect based on space
    Horizontal = 1,     // Horizontal layout
    Vertical = 2        // Vertical layout
}
```

### SankeyNodeAlign

Specifies node alignment options.

```csharp
public enum SankeyNodeAlign
{
    None = 0,           // No specific alignment
    Left = 1,           // Align to left
    Center = 2,         // Align to center
    Right = 3           // Align to right
}
```

### SankeyColorType

Specifies color mapping for links or nodes.

```csharp
public enum SankeyColorType
{
    Static = 0,         // Use static color
    Source = 1          // Color based on source node
}
```

### SankeyLegendPosition

Specifies legend positioning.

```csharp
public enum SankeyLegendPosition
{
    Top = 0,            // Position at top
    Bottom = 1,         // Position at bottom
    Left = 2,           // Position at left
    Right = 3           // Position at right
}
```

### SankeyExportType

Specifies export format types.

```csharp
public enum SankeyExportType
{
    PNG = 0,            // Export as PNG
    JPEG = 1,           // Export as JPEG
    SVG = 2,            // Export as SVG
    PDF = 3             // Export as PDF
}
```

### Theme

Specifies available themes.

```csharp
public enum Theme
{
    Bootstrap5 = 0,
    Bootstrap5Dark = 1,
    Fluent2 = 2,
    Fluent2Dark = 3,
    Material = 4,
    MaterialDark = 5,
    Tailwind = 6,
    TailwindDark = 7,
    Fabric = 8,
    FabricDark = 9,
    HighContrast = 10
}
```

---

## Event Arguments

### SankeyNodeEventArgs

Arguments for node-related events (Click, Enter, Leave).

```csharp
public class SankeyNodeEventArgs
{
    public SankeyNodeContext Node { get; set; }        // Node context
    public string NodeId { get; set; }                 // Node ID
}
```

### SankeyLinkEventArgs

Arguments for link-related events (Click, Enter, Leave).

```csharp
public class SankeyLinkEventArgs
{
    public SankeyLinkContext Link { get; set; }        // Link context
    public string SourceId { get; set; }               // Source node ID
    public string TargetId { get; set; }               // Target node ID
    public double Value { get; set; }                  // Link value/magnitude
}
```

### SankeyNodeRenderEventArgs

Arguments for node rendering event.

```csharp
public class SankeyNodeRenderEventArgs
{
    public SankeyNodeContext Node { get; set; }        // Node being rendered
    public string Color { get; set; }                  // Node color
    public double Width { get; set; }                  // Node width
}
```

### SankeyLinkRenderEventArgs

Arguments for link rendering event.

```csharp
public class SankeyLinkRenderEventArgs
{
    public SankeyLinkContext Link { get; set; }        // Link being rendered
    public string Color { get; set; }                  // Link color
    public double Opacity { get; set; }                // Link opacity
}
```

### SankeyLabelRenderEventArgs

Arguments for label rendering event.

```csharp
public class SankeyLabelRenderEventArgs
{
    public string Text { get; set; }                   // Label text
    public string Color { get; set; }                  // Label color
}
```

### SankeyTooltipRenderEventArgs

Arguments for tooltip rendering event.

```csharp
public class SankeyTooltipRenderEventArgs
{
    public object Content { get; set; }                // Tooltip content
}
```

### SankeyLegendRenderEventArgs

Arguments for legend item rendering event.

```csharp
public class SankeyLegendRenderEventArgs
{
    public string Text { get; set; }                   // Legend item text
    public string Shape { get; set; }                  // Legend shape
    public string Fill { get; set; }                   // Legend fill color
}
```

### SankeyLegendItemHoverEventArgs

Arguments for legend item hover event.

```csharp
public class SankeyLegendItemHoverEventArgs
{
    public string Text { get; set; }                   // Legend item text
}
```

### SankeySizeChangedEventArgs

Arguments for size changed event.

```csharp
public class SankeySizeChangedEventArgs
{
    public double Width { get; set; }                  // New width
    public double Height { get; set; }                 // New height
}
```

### SankeyExportedEventArgs

Arguments for export completed event.

```csharp
public class SankeyExportedEventArgs
{
    public string FileName { get; set; }               // Exported filename
    public byte[] DataBytes { get; set; }              // Exported data as bytes
}
```

---

## Context Classes

### SankeyNodeContext

Context information for node tooltips.

```csharp
public class SankeyNodeContext
{
    public SankeyDataNode Node { get; set; }           // Node data
    public string NodeId { get; set; }                 // Node ID
}
```

### SankeyLinkContext

Context information for link tooltips.

```csharp
public class SankeyLinkContext
{
    public SankeyDataLink Link { get; set; }           // Link data
    public string SourceId { get; set; }               // Source node ID
    public string TargetId { get; set; }               // Target node ID
    public double Value { get; set; }                  // Link value
}
```

---

## Component Hierarchy

```
SfSankey (Main Component)
├── SankeyNodeSettings
├── SankeyLinkSettings
├── SankeyLabelSettings
├── SankeyMargin
├── SankeyTitleStyle
├── SankeySubtitleStyle
├── SankeyLegendSettings
│   ├── SankeyLegendMargin
│   ├── SankeyLegendTextStyle
│   └── SankeyLegendTitleStyle
└── SankeyTooltipSettings
    ├── SankeyNodeTemplate
    └── SankeyLinkTemplate
```

---

## Complete Example with All API Properties

```razor
@page "/sankey-complete"
@using Syncfusion.Blazor.Sankey

<SfSankey @ref="SankeyRef"
          Width="100%"
          Height="600px"
          Nodes="@Nodes"
          Links="@Links"
          Title="Energy Flow Diagram"
          SubTitle="2024 Analysis"
          Orientation="SankeyOrientation.Horizontal"
          BackgroundColor="#f5f5f5"
          BorderColor="#ddd"
          BorderWidth="1"
          LinkCurvature="0.8"
          EnableAnimation="true"
          AnimationDuration="2000"
          AnimationDelay="0"
          EnableAutoLayout="true"
          EnableRTL="false"
          Theme="Theme.Fluent2"
          CssClass="custom-sankey"
          TabIndex="0"
          NodeClick="OnNodeClick"
          LinkClick="OnLinkClick"
          NodeEnter="OnNodeEnter"
          LinkEnter="OnLinkEnter"
          NodeRendering="OnNodeRendering"
          LinkRendering="OnLinkRendering"
          LabelRendering="OnLabelRendering"
          TooltipRendering="OnTooltipRendering"
          SizeChanged="OnSizeChanged"
          Created="OnCreated">

    <!-- Node Configuration -->
    <SankeyNodeSettings Width="25" Padding="15" Alignment="SankeyNodeAlign.Left" Color="#4472C4" />

    <!-- Link Configuration -->
    <SankeyLinkSettings Color="#70AD47" Opacity="0.6" HighlightOpacity="1" InactiveOpacity="0.2" ColorType="SankeyColorType.Source" />

    <!-- Label Configuration -->
    <SankeyLabelSettings Visible="true" FontSize="12px" FontFamily="Arial" FontWeight="500" Color="#000000" Padding="8" />

    <!-- Margins -->
    <SankeyMargin Left="10" Right="10" Top="10" Bottom="10" />

    <!-- Title Styling -->
    <SankeyTitleStyle FontSize="18px" FontFamily="Roboto" FontWeight="700" Color="#1a1a1a" />

    <!-- Subtitle Styling -->
    <SankeySubtitleStyle FontSize="13px" FontFamily="Roboto" FontWeight="400" Color="#666" />

    <!-- Legend Configuration -->
    <SankeyLegendSettings Visible="true" Position="SankeyLegendPosition.Bottom" Height="15" Width="15">
        <SankeyLegendMargin Left="10" Right="10" Top="10" Bottom="10" />
        <SankeyLegendTextStyle FontSize="12px" FontFamily="Roboto" FontWeight="400" Color="#333" />
        <SankeyLegendTitleStyle FontSize="14px" FontFamily="Roboto" FontWeight="600" Color="#000" />
    </SankeyLegendSettings>

    <!-- Tooltip Configuration -->
    <SankeyTooltipSettings Enable="true" Fill="#333" Opacity="0.9">
        <SankeyNodeTemplate>
            @{
                var data = context as SankeyNodeContext;
                if (data != null)
                {
                    <div style="background-color:#333; color:white; padding:8px; border-radius:4px;">
                        <p><strong>@data.Node.Label.Text</strong></p>
                    </div>
                }
            }
        </SankeyNodeTemplate>
        <SankeyLinkTemplate>
            @{
                var data = context as SankeyLinkContext;
                if (data != null)
                {
                    <div style="background-color:#333; color:white; padding:8px; border-radius:4px;">
                        <p>@data.SourceId → @data.TargetId: @data.Value units</p>
                    </div>
                }
            }
        </SankeyLinkTemplate>
    </SankeyTooltipSettings>

</SfSankey>

@code {
    private SfSankey SankeyRef;
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    protected override void OnInitialized()
    {
        // Initialize nodes
        Nodes = new List<SankeyDataNode>()
        {
            new SankeyDataNode() { Id = "Solar", Label = new SankeyDataLabel() { Text = "Solar" }, Color = "#FFD700" },
            new SankeyDataNode() { Id = "Wind", Label = new SankeyDataLabel() { Text = "Wind" }, Color = "#87CEEB" },
            new SankeyDataNode() { Id = "Hydro", Label = new SankeyDataLabel() { Text = "Hydro" }, Color = "#4169E1" },
            new SankeyDataNode() { Id = "Electricity", Label = new SankeyDataLabel() { Text = "Electricity" } },
            new SankeyDataNode() { Id = "Heat", Label = new SankeyDataLabel() { Text = "Heat" } },
        };

        // Initialize links
        Links = new List<SankeyDataLink>()
        {
            new SankeyDataLink() { SourceId = "Solar", TargetId = "Electricity", Value = 30 },
            new SankeyDataLink() { SourceId = "Wind", TargetId = "Electricity", Value = 45 },
            new SankeyDataLink() { SourceId = "Hydro", TargetId = "Electricity", Value = 25 },
            new SankeyDataLink() { SourceId = "Solar", TargetId = "Heat", Value = 15 },
        };
    }

    private void OnNodeClick(SankeyNodeEventArgs args)
    {
        // Handle node click
        Console.WriteLine($"Node clicked: {args.NodeId}");
    }

    private void OnLinkClick(SankeyLinkEventArgs args)
    {
        // Handle link click
        Console.WriteLine($"Link clicked: {args.SourceId} → {args.TargetId} ({args.Value})");
    }

    private void OnNodeEnter(SankeyNodeEventArgs args)
    {
        // Handle node enter
    }

    private void OnLinkEnter(SankeyLinkEventArgs args)
    {
        // Handle link enter
    }

    private void OnNodeRendering(SankeyNodeRenderEventArgs args)
    {
        // Customize node rendering
    }

    private void OnLinkRendering(SankeyLinkRenderEventArgs args)
    {
        // Customize link rendering
    }

    private void OnLabelRendering(SankeyLabelRenderEventArgs args)
    {
        // Customize label rendering
    }

    private void OnTooltipRendering(SankeyTooltipRenderEventArgs args)
    {
        // Customize tooltip rendering
    }

    private void OnSizeChanged(SankeySizeChangedEventArgs args)
    {
        // Handle size change
        Console.WriteLine($"Size changed: {args.Width}x{args.Height}");
    }

    private void OnCreated()
    {
        // Handle component creation
        Console.WriteLine("Sankey component created");
    }

    public async Task ExportChart()
    {
        await SankeyRef.ExportAsync(SankeyExportType.PNG, "sankey-chart.png");
    }

    public async Task PrintChart()
    {
        await SankeyRef.PrintAsync();
    }

    public async Task RefreshChart()
    {
        await SankeyRef.RefreshAsync();
    }
}
```

---

## Resource Links

- **Official Documentation**: https://blazor.syncfusion.com/documentation/sankey-chart/
- **API Reference**: https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Sankey.html
- **Live Demos**: https://blazor.syncfusion.com/demos/sankey-chart/
- **GitHub Examples**: https://github.com/SyncfusionExamples/

