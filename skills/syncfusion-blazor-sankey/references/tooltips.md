# Tooltip Configuration for Blazor Sankey Diagram

This guide covers how to enable and customize tooltips in the Syncfusion Blazor Sankey Diagram to display contextual information when users hover over nodes and links.

## Table of Contents

- [Overview](#overview)
- [Basic Tooltip Configuration](#basic-tooltip-configuration)
- [Tooltip Appearance Properties](#tooltip-appearance-properties)
- [Tooltip Animation](#tooltip-animation)
- [Tooltip Text Styling](#tooltip-text-styling)
- [Format Strings for Tooltips](#format-strings-for-tooltips)
- [Complete Example: Custom Format Strings](#complete-example-custom-format-strings)
- [Custom Tooltip Templates](#custom-tooltip-templates)
- [Complete Template Example](#complete-template-example)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Key Considerations](#key-considerations)
- [Summary](#summary)

## Overview

Tooltips provide contextual details for diagram elements without cluttering the visualization. They appear when users hover over nodes or links, showing relevant data like values, labels, and custom information.

**When to use tooltips:**
- To display detailed values that aren't shown in node labels
- To show flow information for links (source, target, value)
- To provide additional context without permanent screen space
- To enhance user exploration of complex data flows

## Basic Tooltip Configuration

Enable tooltips using the `SankeyTooltipSettings` component nested within `SfSankey`. Set `Enable="true"` to activate tooltips.

**Required namespace:**
```csharp
@using Syncfusion.Blazor.Sankey
```

**Basic example with tooltips enabled:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey Nodes="@Nodes" Links="@Links">
    <SankeyTooltipSettings Enable="true"></SankeyTooltipSettings>
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

This enables default tooltips that show node names and values when hovering over nodes or links.

## Tooltip Appearance Properties

Customize the visual appearance of tooltips using these properties:

### Background and Transparency

- **`Fill`**: Background color of the tooltip (e.g., "#ffffff", "rgba(50,50,50,0.9)")
- **`Opacity`**: Transparency level (0 to 1, default: 0.75)

**Example:**
```razor
<SankeyTooltipSettings 
    Enable="true" 
    Fill="#2c3e50"
    Opacity="0.9">
</SankeyTooltipSettings>
```

### Border Configuration

- **`Border`**: Configure tooltip border using nested `SankeyTooltipBorder`

**Example with border:**
```razor
<SankeyTooltipSettings Enable="true" Fill="#ffffff">
    <SankeyTooltipBorder Color="#dee2e6" Width="2"></SankeyTooltipBorder>
</SankeyTooltipSettings>
```

## Tooltip Animation

Control tooltip animation behavior:

- **`EnableAnimation`**: Toggles animation (default: true)
- **`Duration`**: Animation duration in milliseconds (default: 300)
- **`FadeOutDuration`**: Fade-out duration in milliseconds (default: 1000)

**Example with custom animation:**
```razor
<SankeyTooltipSettings 
    Enable="true"
    EnableAnimation="true"
    Duration="400"
    FadeOutDuration="800">
</SankeyTooltipSettings>
```

**Animation best practices:**
- Keep `Duration` between 200-500ms for smooth transitions
- Use longer `FadeOutDuration` (800-1500ms) to give users time to read content
- Disable animation (`EnableAnimation="false"`) if performance is critical

## Tooltip Text Styling

Customize tooltip text appearance using `SankeyTooltipTextStyle`:

**Available properties:**
- **`FontSize`**: Text size (e.g., "12px", "14px")
- **`FontFamily`**: Font family (e.g., "Arial", "Segoe UI")
- **`FontWeight`**: Text weight (e.g., "400", "600")
- **`FontStyle`**: Text style ("normal", "italic", "oblique")
- **`Color`**: Text color (e.g., "#ffffff", "#333333")

**Example with styled text:**
```razor
<SankeyTooltipSettings Enable="true" Fill="#2c3e50">
    <SankeyTooltipTextStyle 
        FontSize="13px" 
        FontFamily="Segoe UI" 
        FontWeight="500"
        Color="#ffffff"
        FontStyle="normal">
    </SankeyTooltipTextStyle>
</SankeyTooltipSettings>
```

## Format Strings for Tooltips

Customize tooltip content using format strings. Use placeholders to dynamically insert data:

### Node Format String

Use `NodeFormat` to customize tooltips for nodes.

**Available placeholders:**
- `{name}`: Node name/label
- `{value}`: Node value (sum of all incoming/outgoing flows)

**Example:**
```razor
<SankeyTooltipSettings 
    Enable="true"
    NodeFormat="Node: {name}<br/>Total Flow: {value}">
</SankeyTooltipSettings>
```

### Link Format String

Use `LinkFormat` to customize tooltips for links.

**Available placeholders:**
- `{source}`: Source node name
- `{target}`: Target node name
- `{value}`: Link value
- `{source-value}`: Source node total value
- `{target-value}`: Target node total value

**Example:**
```razor
<SankeyTooltipSettings 
    Enable="true"
    LinkFormat="From: {source} → {target}<br/>Flow: {value}">
</SankeyTooltipSettings>
```

## Complete Example: Custom Format Strings

```razor
@using Syncfusion.Blazor.Sankey

<SfSankey Nodes="@Nodes" Links="@Links" Width="100%" Height="500px">
    <SankeyTooltipSettings 
        Enable="true" 
        Fill="#34495e"
        Opacity="0.9"
        NodeFormat="<b>{name}</b><br/>Total: {value} kWh"
        LinkFormat="<b>{source}</b> → <b>{target}</b><br/>Flow: {value} kWh"
        EnableAnimation="true"
        Duration="400"
        FadeOutDuration="1000">
        <SankeyTooltipTextStyle 
            FontSize="13px" 
            FontFamily="Segoe UI" 
            FontWeight="500"
            Color="#ecf0f1"
            FontStyle="normal">
        </SankeyTooltipTextStyle>
        <SankeyTooltipBorder Color="#7f8c8d" Width="1"></SankeyTooltipBorder>
    </SankeyTooltipSettings>
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

## Custom Tooltip Templates

For advanced scenarios, use custom templates to render arbitrary HTML content in tooltips.

### Node Tooltip Template

Use `SankeyNodeTemplate` to create custom content for node tooltips:

```razor
<SfSankey Nodes="@Nodes" Links="@Links">
    <SankeyTooltipSettings>
        <SankeyNodeTemplate>
            @{
                var data = context as SankeyNodeContext;
                if (data != null)
                {
                    <div class="custom-node-tooltip">
                        <div class="tooltip-header">
                            <strong>@data.Node.Id</strong>
                        </div>
                        <div class="tooltip-body">
                            <p>Label: @data.Node.Label.Text</p>
                            <p>Type: Energy Source</p>
                        </div>
                    </div>
                }
            }
        </SankeyNodeTemplate>
    </SankeyTooltipSettings>
</SfSankey>

<style>
    .custom-node-tooltip {
        background: #ffffff;
        color: #333;
        padding: 10px;
        border-radius: 4px;
        min-width: 150px;
    }
    
    .tooltip-header {
        border-bottom: 1px solid #ddd;
        padding-bottom: 5px;
        margin-bottom: 5px;
    }
    
    .tooltip-body p {
        margin: 3px 0;
        font-size: 12px;
    }
</style>

@code {
    // Nodes and Links
}
```

### Link Tooltip Template

Use `SankeyLinkTemplate` to create custom content for link tooltips:

```razor
<SfSankey Nodes="@Nodes" Links="@Links">
    <SankeyTooltipSettings>
        <SankeyLinkTemplate>
            @{
                var data = context as SankeyLinkContext;
                if (data != null)
                {
                    <div class="custom-link-tooltip">
                        <table>
                            <tr>
                                <th>Source</th>
                                <td>@data.Link.SourceId</td>
                            </tr>
                            <tr>
                                <th>Target</th>
                                <td>@data.Link.TargetId</td>
                            </tr>
                            <tr>
                                <th>Flow</th>
                                <td>@data.Link.Value kWh</td>
                            </tr>
                        </table>
                    </div>
                }
            }
        </SankeyLinkTemplate>
    </SankeyTooltipSettings>
</SfSankey>

<style>
    .custom-link-tooltip {
        background: #ffffff;
        color: #333;
        padding: 10px;
        border-radius: 4px;
    }
    
    .custom-link-tooltip table {
        border-collapse: collapse;
        width: 100%;
    }
    
    .custom-link-tooltip th {
        text-align: left;
        padding: 4px 8px;
        font-weight: 600;
        border-bottom: 1px solid #ddd;
    }
    
    .custom-link-tooltip td {
        padding: 4px 8px;
    }
</style>

@code {
    // Nodes and Links
}
```

## Complete Template Example

```razor
@using Syncfusion.Blazor.Sankey

<SfSankey Nodes="@Nodes" Links="@Links" Width="100%" Height="600px">
    <SankeyTooltipSettings>
        <SankeyNodeTemplate>
            @{
                var data = context as SankeyNodeContext;
                if (data != null)
                {
                    <div class="card">
                        <div class="card-header">
                            <strong>Node: @data.Node.Id</strong>
                        </div>
                        <div class="card-body">
                            <p>Label: @data.Node.Label.Text</p>
                            <p>Category: Energy Flow</p>
                        </div>
                    </div>
                }
            }
        </SankeyNodeTemplate>
        <SankeyLinkTemplate>
            @{
                var data = context as SankeyLinkContext;
                if (data != null)
                {
                    <table class="flow-table">
                        <thead>
                            <tr>
                                <th>From</th>
                                <th>To</th>
                                <th>Value</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>@data.Link.SourceId</td>
                                <td>@data.Link.TargetId</td>
                                <td>@data.Link.Value kWh</td>
                            </tr>
                        </tbody>
                    </table>
                }
            }
        </SankeyLinkTemplate>
    </SankeyTooltipSettings>
</SfSankey>

<style>
    .e-sankeychart-tooltip-template {
        margin: 0px !important;
        padding: 0px !important;
    }
    
    .card {
        min-width: 200px;
        background-color: #2c3e50;
        color: #ecf0f1;
        padding: 12px;
        border-radius: 6px;
        font-family: 'Segoe UI', sans-serif;
    }
    
    .card-header {
        border-bottom: 1px solid #34495e;
        padding-bottom: 6px;
        margin-bottom: 8px;
    }
    
    .card-body p {
        margin: 4px 0;
        font-size: 12px;
    }
    
    .flow-table {
        min-width: 250px;
        background-color: #ffffff;
        color: #333;
        border-collapse: collapse;
        font-family: 'Segoe UI', sans-serif;
    }
    
    .flow-table th {
        background-color: #3498db;
        color: white;
        padding: 8px;
        text-align: left;
        font-weight: 600;
    }
    
    .flow-table td {
        padding: 8px;
        border-bottom: 1px solid #ddd;
    }
</style>

@code {
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    protected override void OnInitialized()
    {
        Nodes = new List<SankeyDataNode>()
        {
            new SankeyDataNode() { Id = "Solar", Label = new SankeyDataLabel() { Text = "100" } },
            new SankeyDataNode() { Id = "Wind", Label = new SankeyDataLabel() { Text = "120" } },
            new SankeyDataNode() { Id = "Electricity", Label = new SankeyDataLabel() { Text = "220" } },
            new SankeyDataNode() { Id = "Residential", Label = new SankeyDataLabel() { Text = "220" } }
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

## Best Practices

1. **Keep It Concise**: Display only essential information in tooltips
2. **Ensure Readability**: Use sufficient contrast between text and background
3. **Use Format Strings First**: Try `NodeFormat` and `LinkFormat` before creating templates
4. **Optimize Animation**: Balance smoothness and performance (300-500ms duration)
5. **Style Consistently**: Match tooltip styling with your application theme
6. **Test Interaction**: Verify tooltips work smoothly across different browsers
7. **Consider Mobile**: Ensure tooltips are readable on smaller screens

## Troubleshooting

### Tooltips Not Appearing

**Problem**: Tooltips don't show when hovering over elements

**Solutions**:
- Verify `Enable="true"` is set in `SankeyTooltipSettings`
- Check that you have valid nodes and links data
- Ensure there are no JavaScript errors in the browser console
- Verify the component has rendered properly

### Tooltip Text Cut Off

**Problem**: Tooltip content is truncated or doesn't fit

**Solutions**:
- Increase tooltip width in custom CSS
- Use shorter format strings
- Break long text into multiple lines using `<br/>` tags
- Adjust `FontSize` to make text smaller

### Template Styling Not Applied

**Problem**: Custom styles in tooltip templates aren't working

**Solutions**:
- Add `!important` to CSS rules if needed
- Target `.e-sankeychart-tooltip-template` class for wrapper styles
- Ensure CSS is in a `<style>` block, not scoped styles
- Check browser DevTools to see which styles are applied

### Animation Performance Issues

**Problem**: Tooltip animation is laggy or slow

**Solutions**:
- Reduce `Duration` value (try 200-300ms)
- Set `EnableAnimation="false"` if performance is critical
- Simplify tooltip templates (less HTML/CSS)
- Check overall page performance and resource usage

### Format Placeholders Not Working

**Problem**: Placeholders like `{name}` or `{value}` show literally

**Solutions**:
- Verify correct property names (`NodeFormat` for nodes, `LinkFormat` for links)
- Check placeholder spelling (case-sensitive)
- Ensure you're not mixing templates with format strings
- Use templates if you need more complex formatting

## Key Considerations

- Keep tooltip text concise and readable
- Ensure sufficient contrast for text and background
- Use templates for richer content or additional fields
- Tune animation durations to balance responsiveness and polish
- Prefer `NodeFormat` and `LinkFormat` for simple customization
- Test tooltip behavior across different browsers and devices
- Consider accessibility for users with different interaction patterns

## Summary

Tooltips enhance Sankey diagram usability by providing contextual information on hover. Use `SankeyTooltipSettings` to enable and configure tooltips with custom styling, animations, and content. For simple scenarios, use format strings (`NodeFormat` and `LinkFormat`) with placeholders. For complex content, use custom templates (`SankeyNodeTemplate` and `SankeyLinkTemplate`) to render rich HTML. Well-designed tooltips help users explore and understand data flows without cluttering the main visualization.
