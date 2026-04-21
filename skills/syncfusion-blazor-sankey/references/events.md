# Event Handling for Blazor Sankey Diagram

This guide covers the comprehensive set of events provided by the Syncfusion Blazor Sankey Diagram for handling user interactions, lifecycle moments, and customization opportunities.

## Table of Contents

- [Event Overview](#event-overview)
- [Rendering Events](#rendering-events)
- [Interaction Events](#interaction-events)
- [Hover Events](#hover-events)
- [Lifecycle Events](#lifecycle-events)
- [Export and Print Events](#export-and-print-events)
- [Complete Event Examples](#complete-event-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Event Overview

The Blazor Sankey Diagram provides 16 events that enable responsive behaviors, dynamic customization, and coordinated application updates. Events are categorized by their purpose and trigger conditions.

**Event categories:**
- **Rendering Events**: Customize appearance before elements render
- **Interaction Events**: Respond to user clicks on nodes and links
- **Hover Events**: Handle mouse enter/leave for nodes and links
- **Lifecycle Events**: React to diagram creation and size changes
- **Export/Print Events**: Respond to completion of print and export operations

**Required namespace:**
```csharp
@using Syncfusion.Blazor.Sankey
```

## Rendering Events

Rendering events fire before diagram elements are drawn, allowing dynamic customization based on data or application state.

### NodeRendering

Fires before each node is rendered, enabling node-specific styling.

**Event arguments**: `SankeyNodeRenderEventArgs`
- `Node`: Reference to the node being rendered
- `Fill`: Node fill color (modifiable)
- `Cancel`: Set to true to skip rendering this node

**Example:**
```razor
<SfSankey Nodes="@Nodes" Links="@Links" NodeRendering="OnNodeRendering">
</SfSankey>

@code {
    private void OnNodeRendering(SankeyNodeRenderEventArgs args)
    {
        // Color nodes based on category
        if (args.Node.Id.StartsWith("Solar"))
        {
            args.Fill = "#ffc107"; // Amber for solar
        }
        else if (args.Node.Id.StartsWith("Wind"))
        {
            args.Fill = "#2196f3"; // Blue for wind
        }
        else if (args.Node.Id.StartsWith("Coal"))
        {
            args.Fill = "#795548"; // Brown for coal
        }
    }

    // Nodes and links initialization
}
```

**Use cases:**
- Apply conditional styling based on node properties
- Highlight nodes meeting specific criteria
- Implement custom color schemes
- Skip rendering for filtered nodes

### LinkRendering

Fires before each link is rendered, enabling link-specific styling.

**Event arguments**: `SankeyLinkRenderEventArgs`
- `Link`: Reference to the link being rendered
- `Fill`: Link fill color (modifiable)
- `Opacity`: Link opacity (modifiable)
- `Cancel`: Set to true to skip rendering this link

**Example:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey Nodes="@Nodes" Links="@Links" LinkRendering="OnLinkRendering">
</SfSankey>

@code {
    private void OnLinkRendering(SankeyLinkRenderEventArgs args)
    {
        // Color links by value threshold
        if (args.Link.Value > 150)
        {
            args.Fill = "#4caf50"; // Green for high flow
        }
        else if (args.Link.Value > 75)
        {
            args.Fill = "#ff9800"; // Orange for medium flow
        }
        else
        {
            args.Fill = "#f44336"; // Red for low flow
        }
        
        args.Opacity = 0.6;
    }

    // Nodes and links initialization
}
```

**Use cases:**
- Color links by value ranges
- Adjust opacity based on importance
- Highlight critical flows
- Filter links dynamically

### LabelRendering

Fires before labels are rendered, allowing text and format customization.

**Event arguments**: `SankeyLabelRenderEventArgs`
- `Node`: Reference to the node whose label is rendering
- `Text`: Label text (modifiable)
- `Fill`: Label color (modifiable)
- `FontSize`: Font size (modifiable)
- `Cancel`: Set to true to hide this label

**Example:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey Nodes="@Nodes" Links="@Links" LabelRendering="OnLabelRendering">
    <SankeyLabelSettings Visible="true"></SankeyLabelSettings>
</SfSankey>

@code {
    private void OnLabelRendering(SankeyLabelRenderEventArgs args)
    {
        // Format label with units
        args.Text = $"{args.Node.Label.Text} (kWh)";
        
        // Make high-value nodes bold/larger
        if (GetNodeTotalValue(args.Node) > 200)
        {
            args.FontSize = "16px";
            args.Fill = "#d32f2f";
        }
    }

    private double GetNodeTotalValue(SankeyDataNode node)
    {
        // Calculate total flow through node
        return Links.Where(l => l.SourceId == node.Id || l.TargetId == node.Id)
                    .Sum(l => l.Value);
    }

    // Nodes and links initialization
}
```

**Use cases:**
- Add units or prefixes to labels
- Format numbers with custom patterns
- Emphasize important nodes
- Hide labels conditionally

### LegendItemRendering

Fires before legend items are rendered, enabling customization.

**Event arguments**: `SankeyLegendRenderEventArgs`
- `Text`: Legend item text (modifiable)
- `Fill`: Legend shape color (modifiable)
- `Cancel`: Set to true to hide this legend item

**Example:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey Nodes="@Nodes" Links="@Links" LegendItemRendering="OnLegendItemRendering">
    <SankeyLegendSettings Visible="true"></SankeyLegendSettings>
</SfSankey>

@code {
    private void OnLegendItemRendering(SankeyLegendRenderEventArgs args)
    {
        // Add custom prefix to legend items
        args.Text = $"Category: {args.Text}";
        
        // Custom color mapping
        if (args.Text.Contains("Energy"))
        {
            args.Fill = "#1976d2";
        }
    }

    // Nodes and links initialization
}
```

**Use cases:**
- Customize legend text formatting
- Apply custom color schemes
- Filter legend items
- Add contextual information

## Interaction Events

Interaction events respond to user clicks on diagram elements.

### NodeClick

Fires when a user clicks on a node.

**Event arguments**: `SankeyNodeEventArgs`
- `Node`: Reference to the clicked node
- `Target`: DOM element that was clicked

**Example:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey Nodes="@Nodes" Links="@Links" NodeClick="OnNodeClick">
</SfSankey>

<div class="info-panel">
    <h4>Selected Node</h4>
    <p>@SelectedNodeInfo</p>
</div>

@code {
    private string SelectedNodeInfo = "Click a node to see details";

    private void OnNodeClick(SankeyNodeEventArgs args)
    {
        var incomingFlow = Links.Where(l => l.TargetId == args.Node.Id).Sum(l => l.Value);
        var outgoingFlow = Links.Where(l => l.SourceId == args.Node.Id).Sum(l => l.Value);
        
        SelectedNodeInfo = $"Node: {args.Node.Id}\n" +
                          $"Incoming: {incomingFlow}\n" +
                          $"Outgoing: {outgoingFlow}";
        StateHasChanged();
    }

    // Nodes and links initialization
}
```

**Use cases:**
- Show detailed information panels
- Navigate to detail views
- Trigger related UI updates
- Record analytics events
- Filter or drill down data

### LinkClick

Fires when a user clicks on a link.

**Event arguments**: `SankeyLinkEventArgs`
- `Link`: Reference to the clicked link
- `Target`: DOM element that was clicked

**Example:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey Nodes="@Nodes" Links="@Links" LinkClick="OnLinkClick">
</SfSankey>

<div class="info-panel">
    <h4>Selected Flow</h4>
    <p>@SelectedLinkInfo</p>
</div>

@code {
    private string SelectedLinkInfo = "Click a link to see flow details";

    private void OnLinkClick(SankeyLinkEventArgs args)
    {
        var percentage = (args.Link.Value / GetTotalFlow()) * 100;
        
        SelectedLinkInfo = $"Flow: {args.Link.SourceId} → {args.Link.TargetId}\n" +
                          $"Value: {args.Link.Value}\n" +
                          $"Percentage: {percentage:F1}%";
        StateHasChanged();
    }

    private double GetTotalFlow()
    {
        return Links.Sum(l => l.Value);
    }

    // Nodes and links initialization
}
```

**Use cases:**
- Display flow details
- Show breakdown or sub-flows
- Navigate to transaction lists
- Trigger drill-down views

## Hover Events

Hover events fire when the mouse pointer enters or leaves diagram elements.

### NodeEnter

Fires when the pointer enters a node region.

**Event arguments**: `SankeyNodeEventArgs`
- `Node`: Reference to the node being entered

**Example:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey Nodes="@Nodes" Links="@Links" NodeEnter="OnNodeEnter">
</SfSankey>

@code {
    private void OnNodeEnter(SankeyNodeEventArgs args)
    {
        Console.WriteLine($"Hovering over node: {args.Node.Id}");
        // Could highlight related elements, show preview, etc.
    }

    // Nodes and links
}
```

### NodeLeave

Fires when the pointer leaves a node region.

**Event arguments**: `SankeyNodeEventArgs`
- `Node`: Reference to the node being left

**Example:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey Nodes="@Nodes" Links="@Links" NodeLeave="OnNodeLeave">
</SfSankey>

@code {
    private void OnNodeLeave(SankeyNodeEventArgs args)
    {
        Console.WriteLine($"Left node: {args.Node.Id}");
        // Could remove highlights, hide preview, etc.
    }

    // Nodes and links
}
```

### LinkEnter

Fires when the pointer enters a link region.

**Event arguments**: `SankeyLinkEventArgs`
- `Link`: Reference to the link being entered

**Example:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey Nodes="@Nodes" Links="@Links" LinkEnter="OnLinkEnter">
</SfSankey>

<div class="hover-info">@HoverInfo</div>

@code {
    private string HoverInfo = "";

    private void OnLinkEnter(SankeyLinkEventArgs args)
    {
        HoverInfo = $"Flow: {args.Link.SourceId} → {args.Link.TargetId} ({args.Link.Value})";
        StateHasChanged();
    }

    // Nodes and links
}
```

### LinkLeave

Fires when the pointer leaves a link region.

**Event arguments**: `SankeyLinkEventArgs`
- `Link`: Reference to the link being left

**Example:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey Nodes="@Nodes" Links="@Links" LinkLeave="OnLinkLeave">
</SfSankey>

@code {
    private void OnLinkLeave(SankeyLinkEventArgs args)
    {
        HoverInfo = "";
        StateHasChanged();
    }

    // Nodes and links
}
```

### LegendItemHover

Fires when hovering over a legend item.

**Event arguments**: `SankeyLegendItemHoverEventArgs`
- `Text`: Legend item text
- `Fill`: Legend item color

**Example:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey Nodes="@Nodes" Links="@Links" LegendItemHover="OnLegendItemHover">
    <SankeyLegendSettings Visible="true" EnableHighlight="true"></SankeyLegendSettings>
</SfSankey>

@code {
    private void OnLegendItemHover(SankeyLegendItemHoverEventArgs args)
    {
        Console.WriteLine($"Hovering over legend: {args.Text}");
    }

    // Nodes and links
}
```

## Lifecycle Events

Lifecycle events fire during diagram initialization and size changes.

### Created

Fires after the diagram is fully initialized and rendered.

**Event arguments**: None

**Example:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey Nodes="@Nodes" Links="@Links" Created="OnCreated">
</SfSankey>

@code {
    private void OnCreated()
    {
        Console.WriteLine("Sankey diagram created and ready");
        // Perform one-time setup, load additional data, etc.
    }

    // Nodes and links
}
```

**Use cases:**
- Initialize related components
- Load additional data
- Set up external integrations
- Record initialization metrics

### SizeChanged

Fires when the component size changes.

**Event arguments**: `SankeySizeChangedEventArgs`
- `PreviousSize`: Previous dimensions
- `CurrentSize`: New dimensions

**Example:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey Nodes="@Nodes" Links="@Links" SizeChanged="OnSizeChanged">
</SfSankey>

@code {
    private void OnSizeChanged(SankeySizeChangedEventArgs args)
    {
        Console.WriteLine($"Size changed from {args.PreviousSize.Width}x{args.PreviousSize.Height} " +
                         $"to {args.CurrentSize.Width}x{args.CurrentSize.Height}");
        
        // Adjust layout, reload data, etc.
    }

    // Nodes and links
}
```

**Use cases:**
- Adjust layout for new dimensions
- Reload or filter data for available space
- Update related UI elements
- Trigger responsive behavior

### TooltipRendering

Fires before tooltips render, allowing content and style customization.

**Event arguments**: `SankeyTooltipRenderEventArgs`
- `Node`: Node reference (if tooltip is for a node)
- `Link`: Link reference (if tooltip is for a link)
- `Text`: Tooltip text (modifiable)
- `Fill`: Tooltip background (modifiable)
- `Cancel`: Set to true to prevent tooltip

**Example:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey Nodes="@Nodes" Links="@Links" TooltipRendering="OnTooltipRendering">
    <SankeyTooltipSettings Enable="true"></SankeyTooltipSettings>
</SfSankey>

@code {
    private void OnTooltipRendering(SankeyTooltipRenderEventArgs args)
    {
        if (args.Node != null)
        {
            var total = GetNodeTotalValue(args.Node);
            args.Text = $"<b>{args.Node.Id}</b><br/>Total Flow: {total} kWh";
            args.Fill = "#2c3e50";
        }
        else if (args.Link != null)
        {
            var percentage = (args.Link.Value / GetTotalFlow()) * 100;
            args.Text = $"<b>{args.Link.SourceId} → {args.Link.TargetId}</b><br/>" +
                       $"Value: {args.Link.Value} kWh ({percentage:F1}%)";
            args.Fill = "#34495e";
        }
    }

    // Helper methods and data initialization
}
```

**Use cases:**
- Add custom content to tooltips
- Format tooltip data
- Apply conditional styling
- Hide tooltips for specific elements

## Export and Print Events

Events that fire after export or print operations complete.

### PrintCompleted

Fires after printing completes.

**Event arguments**: None

**Example:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey @ref="SankeyRef" Nodes="@Nodes" Links="@Links" PrintCompleted="OnPrintCompleted">
</SfSankey>

<button @onclick="PrintDiagram">Print</button>

@code {
    private SfSankey SankeyRef;

    private async Task PrintDiagram()
    {
        await SankeyRef.PrintAsync();
    }

    private void OnPrintCompleted()
    {
        Console.WriteLine("Print operation completed");
        // Show notification, log event, etc.
    }

    // Nodes and links
}
```

**Use cases:**
- Show success notifications
- Log print operations
- Update UI state
- Trigger follow-up actions

### ExportCompleted

Fires after export completes.

**Event arguments**: `SankeyExportedEventArgs`
- `Base64`: Base64-encoded image data
- `DataUrl`: Data URL of exported image

**Example:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey @ref="SankeyRef" Nodes="@Nodes" Links="@Links" ExportCompleted="OnExportCompleted">
</SfSankey>

<button @onclick="ExportDiagram">Export as PNG</button>

@code {
    private SfSankey SankeyRef;

    private async Task ExportDiagram()
    {
        await SankeyRef.ExportAsync(SankeyExportType.PNG, "sankey-export", null, true, false);
    }

    private void OnExportCompleted(SankeyExportedEventArgs args)
    {
        Console.WriteLine("Export completed");
        Console.WriteLine($"Base64 length: {args.Base64?.Length ?? 0}");
        
        // Could upload to server, show preview, etc.
    }

    // Nodes and links
}
```

**Use cases:**
- Upload exported images to server
- Show export success notification
- Process exported data
- Trigger analytics events

## Complete Event Examples

### Example 1: Interactive Dashboard with Multiple Events

```razor
@using Syncfusion.Blazor.Sankey

<SfSankey @ref="SankeyRef"
          Width="100%" 
          Height="600px"
          Nodes="@Nodes" 
          Links="@Links"
          NodeClick="OnNodeClick"
          LinkClick="OnLinkClick"
          NodeEnter="OnNodeEnter"
          NodeLeave="OnNodeLeave"
          NodeRendering="OnNodeRendering"
          LinkRendering="OnLinkRendering"
          Created="OnCreated"
          SizeChanged="OnSizeChanged">
    <SankeyTooltipSettings Enable="true"></SankeyTooltipSettings>
</SfSankey>

<div class="event-log">
    <h4>Event Log</h4>
    <div>@EventMessage</div>
</div>

@code {
    private SfSankey SankeyRef;
    private string EventMessage = "Diagram ready";
    
    private void OnCreated()
    {
        EventMessage = "Diagram initialized successfully";
        StateHasChanged();
    }
    
    private void OnNodeClick(SankeyNodeEventArgs args)
    {
        EventMessage = $"Clicked node: {args.Node.Id}";
        StateHasChanged();
    }
    
    private void OnLinkClick(SankeyLinkEventArgs args)
    {
        EventMessage = $"Clicked link: {args.Link.SourceId} → {args.Link.TargetId} ({args.Link.Value})";
        StateHasChanged();
    }
    
    private void OnNodeEnter(SankeyNodeEventArgs args)
    {
        EventMessage = $"Hovering: {args.Node.Id}";
        StateHasChanged();
    }
    
    private void OnNodeLeave(SankeyNodeEventArgs args)
    {
        EventMessage = "Hover ended";
        StateHasChanged();
    }
    
    private void OnNodeRendering(SankeyNodeRenderEventArgs args)
    {
        // Apply category-based colors
        if (args.Node.Id.Contains("Energy"))
            args.Fill = "#4caf50";
        else if (args.Node.Id.Contains("Loss"))
            args.Fill = "#f44336";
    }
    
    private void OnLinkRendering(SankeyLinkRenderEventArgs args)
    {
        // Color by value
        args.Fill = args.Link.Value > 100 ? "#2196f3" : "#ff9800";
        args.Opacity = 0.5;
    }
    
    private void OnSizeChanged(SankeySizeChangedEventArgs args)
    {
        EventMessage = $"Resized to {args.CurrentSize.Width}x{args.CurrentSize.Height}";
        StateHasChanged();
    }
    
    // Nodes and links initialization
}
```

### Example 2: Advanced Tooltip Customization

```razor
@using Syncfusion.Blazor.Sankey

<SfSankey Nodes="@Nodes" 
          Links="@Links"
          TooltipRendering="OnTooltipRendering">
    <SankeyTooltipSettings Enable="true"></SankeyTooltipSettings>
</SfSankey>

@code {
    private void OnTooltipRendering(SankeyTooltipRenderEventArgs args)
    {
        if (args.Node != null)
        {
            var incoming = Links.Where(l => l.TargetId == args.Node.Id).Sum(l => l.Value);
            var outgoing = Links.Where(l => l.SourceId == args.Node.Id).Sum(l => l.Value);
            var efficiency = incoming > 0 ? (outgoing / incoming) * 100 : 100;
            
            args.Text = $"<div style='padding: 10px;'>" +
                       $"<h4 style='margin: 0 0 8px 0;'>{args.Node.Id}</h4>" +
                       $"<p style='margin: 4px 0;'>Incoming: {incoming:F1} kWh</p>" +
                       $"<p style='margin: 4px 0;'>Outgoing: {outgoing:F1} kWh</p>" +
                       $"<p style='margin: 4px 0;'>Efficiency: {efficiency:F1}%</p>" +
                       $"</div>";
            args.Fill = "#ffffff";
        }
        else if (args.Link != null)
        {
            var totalFlow = Links.Sum(l => l.Value);
            var percentage = (args.Link.Value / totalFlow) * 100;
            
            args.Text = $"<div style='padding: 10px;'>" +
                       $"<h4 style='margin: 0 0 8px 0;'>Flow Details</h4>" +
                       $"<p style='margin: 4px 0;'>{args.Link.SourceId} → {args.Link.TargetId}</p>" +
                       $"<p style='margin: 4px 0;'>Value: {args.Link.Value:F1} kWh</p>" +
                       $"<p style='margin: 4px 0;'>Share: {percentage:F1}%</p>" +
                       $"</div>";
            args.Fill = "#f8f9fa";
        }
    }
    
    // Nodes and links initialization
}
```

## Best Practices

1. **State Management**: Call `StateHasChanged()` when updating component state in event handlers
2. **Performance**: Avoid heavy processing in high-frequency events (NodeEnter, NodeLeave)
3. **Error Handling**: Wrap event handlers in try-catch for graceful error handling
4. **Null Checks**: Always check for null references in event arguments
5. **Event Combination**: Combine multiple events for rich interactions
6. **Conditional Logic**: Use rendering events for dynamic styling
7. **User Feedback**: Provide visual feedback for interaction events
8. **Cleanup**: Remove highlights or overlays in "Leave" events
9. **Async Operations**: Use async/await for operations that require it
10. **Testing**: Test event handlers across different browsers and scenarios

## Troubleshooting

### Events Not Firing

**Problem**: Event handlers aren't being called

**Solutions**:
- Verify event name spelling (case-sensitive)
- Check that method signature matches event delegate
- Ensure component is fully rendered before expecting events
- Check browser console for JavaScript errors
- Verify data (nodes/links) is properly initialized

### StateHasChanged Issues

**Problem**: UI doesn't update after event handler runs

**Solutions**:
- Call `StateHasChanged()` at end of event handler
- Ensure you're modifying component properties, not local variables
- Check for async operations that need await
- Verify component isn't disposed or unmounted

### Performance Problems with Hover Events

**Problem**: Diagram is laggy during mouse movement

**Solutions**:
- Minimize processing in NodeEnter/NodeLeave/LinkEnter/LinkLeave
- Debounce rapid-fire operations
- Avoid calling `StateHasChanged()` on every hover event
- Cache calculated values instead of recalculating
- Consider disabling hover events if not needed

### Rendering Event Changes Not Applied

**Problem**: Changes in rendering events don't appear

**Solutions**:
- Ensure you're modifying the correct property (Fill, Opacity, etc.)
- Check that values are valid (colors, numbers)
- Verify rendering event is attached correctly
- Clear browser cache if changes seem cached

### Tooltip Rendering Issues

**Problem**: Custom tooltip content doesn't display properly

**Solutions**:
- Verify HTML in `args.Text` is valid
- Check for CSS conflicts with custom styles
- Ensure special characters are properly escaped
- Test with simple text first, then add complexity
- Verify TooltipSettings Enable="true"

## Key Considerations

- Use events to build interactive, responsive diagrams
- Combine events to orchestrate complex user experiences
- Call `StateHasChanged()` when updating UI state
- Consider performance for high-frequency events
- Use `Created` event for initialization tasks
- Leverage rendering events for dynamic customization
- Provide user feedback for interaction events
- Test across browsers and devices

## Summary

The Blazor Sankey Diagram provides 16 comprehensive events covering rendering, interaction, hover, lifecycle, and export scenarios. Use rendering events (NodeRendering, LinkRendering, LabelRendering, LegendItemRendering) for dynamic customization. Handle user interaction with click events (NodeClick, LinkClick). Respond to hover behavior with enter/leave events. Monitor lifecycle with Created and SizeChanged events. Track operations with PrintCompleted and ExportCompleted. Combine events to create rich, interactive visualizations that respond to user input and application state.
