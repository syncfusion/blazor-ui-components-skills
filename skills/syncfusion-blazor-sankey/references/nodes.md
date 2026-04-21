# Nodes in Blazor Sankey Diagram

This guide provides complete information about defining, configuring, and styling nodes in the Syncfusion Blazor Sankey Diagram. Nodes represent entities, categories, or stages in a flow visualization.

## Table of Contents

- [Overview](#overview)
- [Node Structure](#node-structure)
- [Basic Node Configuration](#basic-node-configuration)
- [Complete Working Example](#complete-working-example)
- [Customizing Node Appearance with SankeyNodeSettings](#customizing-node-appearance-with-sankeynodesettings)
- [Complete Styling Example](#complete-styling-example)
- [Node Positioning](#node-positioning)
- [Node Best Practices](#node-best-practices)
- [Dynamic Node Management](#dynamic-node-management)
- [Troubleshooting](#troubleshooting)
- [Summary](#summary)

## Overview

Nodes are the fundamental building blocks of a Sankey diagram. They represent:
- **Entities**: Products, resources, energy sources
- **Categories**: Types, classifications, groups
- **Stages**: Process steps, workflow phases, transformation points

Each node is connected to other nodes via links, and the Sankey component automatically positions nodes based on these connections and flow direction.

## Node Structure

### SankeyDataNode Model

Nodes are defined using the `SankeyDataNode` class from `Syncfusion.Blazor.Sankey`:

```csharp
public class SankeyDataNode
{
    public string Id { get; set; }              // Required: Unique identifier
    public SankeyDataLabel Label { get; set; }  // Optional: Display label
}
```

**Properties:**
- **`Id`** (required): Unique string identifier for the node
  - Must be unique across all nodes
  - Case-sensitive
  - Used by links to reference this node
  
- **`Label`** (optional): Label configuration object
  - Type: `SankeyDataLabel`
  - Contains `Text` property for display text
  - If omitted, `Id` is used as the label

### SankeyDataLabel Model

```csharp
public class SankeyDataLabel
{
    public string Text { get; set; }  // Display text for the node
}
```

---

## Basic Node Configuration

### Simple Node Definition

Define nodes by creating a collection of `SankeyDataNode` objects:

```csharp
@code {
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();

    protected override void OnInitialized()
    {
        Nodes = new List<SankeyDataNode>()
        {
            new SankeyDataNode() { Id = "Solar" },
            new SankeyDataNode() { Id = "Wind" },
            new SankeyDataNode() { Id = "Hydro" },
            new SankeyDataNode() { Id = "Electricity" },
            new SankeyDataNode() { Id = "Residential" },
            new SankeyDataNode() { Id = "Commercial" }
        };
    }
}
```

In this example, each node will display its `Id` as the label.

### Node with Custom Labels

Use the `Label` property to specify different display text:

```csharp
Nodes = new List<SankeyDataNode>()
{
    new SankeyDataNode() 
    { 
        Id = "solar_pv",
        Label = new SankeyDataLabel() { Text = "Solar PV" }
    },
    new SankeyDataNode() 
    { 
        Id = "wind_turbine",
        Label = new SankeyDataLabel() { Text = "Wind Turbines" }
    },
    new SankeyDataNode() 
    { 
        Id = "elec_grid",
        Label = new SankeyDataLabel() { Text = "Electricity Grid" }
    }
};
```

**When to use labels:**
- Display text differs from technical ID
- Multi-word labels with proper spacing
- Special characters or formatting
- User-friendly names vs. database keys

---

## Complete Working Example

Here's a complete example with energy flow visualization:

```razor
@page "/energy-nodes"
@using Syncfusion.Blazor.Sankey

<h3>Energy Flow - Node Configuration</h3>

<SfSankey Nodes="@Nodes" Links="@Links" Width="100%" Height="600px" BackgroundColor="#f5f5f5">
    <SankeyNodeSettings Color="#2e7d32" Width="20" Padding="25"></SankeyNodeSettings>
    <SankeyLinkSettings Color="#1976d2" Opacity="0.4"></SankeyLinkSettings>
    <SankeyLabelSettings Visible="true" Color="#000000" FontSize="14px" FontWeight="500"></SankeyLabelSettings>
</SfSankey>

@code {
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    protected override void OnInitialized()
    {
        // Define nodes with custom labels
        Nodes = new List<SankeyDataNode>()
        {
            // Energy Sources
            new SankeyDataNode() { Id = "Solar", Label = new SankeyDataLabel() { Text = "Solar" } },
            new SankeyDataNode() { Id = "Wind", Label = new SankeyDataLabel() { Text = "Wind" } },
            new SankeyDataNode() { Id = "Hydro", Label = new SankeyDataLabel() { Text = "Hydro" } },
            new SankeyDataNode() { Id = "Nuclear", Label = new SankeyDataLabel() { Text = "Nuclear" } },
            new SankeyDataNode() { Id = "Coal", Label = new SankeyDataLabel() { Text = "Coal" } },
            new SankeyDataNode() { Id = "Natural Gas", Label = new SankeyDataLabel() { Text = "Natural Gas" } },
            new SankeyDataNode() { Id = "Oil", Label = new SankeyDataLabel() { Text = "Oil" } },
            
            // Energy Carriers
            new SankeyDataNode() { Id = "Electricity", Label = new SankeyDataLabel() { Text = "Electricity" } },
            new SankeyDataNode() { Id = "Heat", Label = new SankeyDataLabel() { Text = "Heat" } },
            new SankeyDataNode() { Id = "Fuel", Label = new SankeyDataLabel() { Text = "Fuel" } },
            
            // End Use Sectors
            new SankeyDataNode() { Id = "Residential", Label = new SankeyDataLabel() { Text = "Residential" } },
            new SankeyDataNode() { Id = "Commercial", Label = new SankeyDataLabel() { Text = "Commercial" } },
            new SankeyDataNode() { Id = "Industrial", Label = new SankeyDataLabel() { Text = "Industrial" } },
            new SankeyDataNode() { Id = "Transportation", Label = new SankeyDataLabel() { Text = "Transportation" } },
            
            // Final Outcomes
            new SankeyDataNode() { Id = "Energy Services", Label = new SankeyDataLabel() { Text = "Energy Services" } },
            new SankeyDataNode() { Id = "Losses", Label = new SankeyDataLabel() { Text = "Losses" } }
        };

        // Define links (flows between nodes)
        Links = new List<SankeyDataLink>()
        {
            // Energy Sources to Carriers
            new SankeyDataLink() { SourceId = "Solar", TargetId = "Electricity", Value = 100 },
            new SankeyDataLink() { SourceId = "Wind", TargetId = "Electricity", Value = 120 },
            new SankeyDataLink() { SourceId = "Hydro", TargetId = "Electricity", Value = 80 },
            new SankeyDataLink() { SourceId = "Nuclear", TargetId = "Electricity", Value = 90 },
            new SankeyDataLink() { SourceId = "Coal", TargetId = "Electricity", Value = 200 },
            new SankeyDataLink() { SourceId = "Natural Gas", TargetId = "Electricity", Value = 130 },
            new SankeyDataLink() { SourceId = "Natural Gas", TargetId = "Heat", Value = 80 },
            new SankeyDataLink() { SourceId = "Oil", TargetId = "Fuel", Value = 250 },
            
            // Energy Carriers to Sectors
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Residential", Value = 170 },
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Commercial", Value = 160 },
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Industrial", Value = 210 },
            new SankeyDataLink() { SourceId = "Heat", TargetId = "Residential", Value = 40 },
            new SankeyDataLink() { SourceId = "Heat", TargetId = "Commercial", Value = 20 },
            new SankeyDataLink() { SourceId = "Heat", TargetId = "Industrial", Value = 20 },
            new SankeyDataLink() { SourceId = "Fuel", TargetId = "Transportation", Value = 200 },
            new SankeyDataLink() { SourceId = "Fuel", TargetId = "Industrial", Value = 50 },
            
            // Sectors to Final Outcomes
            new SankeyDataLink() { SourceId = "Residential", TargetId = "Energy Services", Value = 180 },
            new SankeyDataLink() { SourceId = "Commercial", TargetId = "Energy Services", Value = 150 },
            new SankeyDataLink() { SourceId = "Industrial", TargetId = "Energy Services", Value = 230 },
            new SankeyDataLink() { SourceId = "Transportation", TargetId = "Energy Services", Value = 150 },
            new SankeyDataLink() { SourceId = "Residential", TargetId = "Losses", Value = 30 },
            new SankeyDataLink() { SourceId = "Commercial", TargetId = "Losses", Value = 30 },
            new SankeyDataLink() { SourceId = "Industrial", TargetId = "Losses", Value = 50 },
            new SankeyDataLink() { SourceId = "Transportation", TargetId = "Losses", Value = 50 }
        };
    }
}
```

---

## Customizing Node Appearance with SankeyNodeSettings

The `SankeyNodeSettings` component allows you to globally configure how all nodes appear in the diagram.

### Available Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Width` | double | 20 | Width of nodes in pixels |
| `Padding` | double | 10 | Vertical spacing between nodes |
| `Color` | string | null | Fill color for all nodes (hex, rgb, named) |
| `Opacity` | double | 1.0 | Node transparency (0.0 to 1.0) |
| `Alignment` | SankeyNodeAlign | Stretch | Node alignment within layout |
| `Offset` | double | 0 | Additional offset for positioning |

### Basic Node Styling

```razor
<SfSankey Nodes="@Nodes" Links="@Links">
    <SankeyNodeSettings 
        Width="25" 
        Padding="20" 
        Color="#2196F3">
    </SankeyNodeSettings>
</SfSankey>
```

### Node Width

Controls the visual thickness of node rectangles:

```razor
<!-- Thin nodes (default) -->
<SankeyNodeSettings Width="15"></SankeyNodeSettings>

<!-- Medium nodes -->
<SankeyNodeSettings Width="25"></SankeyNodeSettings>

<!-- Wide nodes for emphasis -->
<SankeyNodeSettings Width="40"></SankeyNodeSettings>
```

**Guidelines:**
- **15-20px**: Minimal, clean look
- **20-30px**: Standard, balanced appearance
- **30-50px**: Emphasize nodes, reduce focus on flows

### Node Padding

Controls vertical spacing between nodes in the same column:

```razor
<!-- Tight spacing -->
<SankeyNodeSettings Padding="10"></SankeyNodeSettings>

<!-- Standard spacing -->
<SankeyNodeSettings Padding="20"></SankeyNodeSettings>

<!-- Loose spacing for clarity -->
<SankeyNodeSettings Padding="30"></SankeyNodeSettings>
```

**Use cases:**
- Small padding: Compact diagrams with many nodes
- Large padding: Emphasis on individual nodes, better readability

### Node Color

Set a uniform color for all nodes:

```razor
<!-- Hex color -->
<SankeyNodeSettings Color="#4CAF50"></SankeyNodeSettings>

<!-- RGB color -->
<SankeyNodeSettings Color="rgb(76, 175, 80)"></SankeyNodeSettings>

<!-- Named color -->
<SankeyNodeSettings Color="teal"></SankeyNodeSettings>

<!-- With opacity -->
<SankeyNodeSettings Color="rgba(76, 175, 80, 0.7)"></SankeyNodeSettings>
```

### Node Opacity

Control transparency of all nodes:

```razor
<!-- Fully opaque (default) -->
<SankeyNodeSettings Opacity="1.0"></SankeyNodeSettings>

<!-- Semi-transparent -->
<SankeyNodeSettings Opacity="0.7"></SankeyNodeSettings>

<!-- Very transparent -->
<SankeyNodeSettings Opacity="0.3"></SankeyNodeSettings>
```

### Node Alignment

Control how nodes are aligned within the layout:

```csharp
public enum SankeyNodeAlign
{
    Left,      // Align nodes to the left
    Top,       // Align nodes to the top
    Stretch    // Stretch nodes to fill space (default)
}
```

```razor
<!-- Left-aligned nodes -->
<SankeyNodeSettings Alignment="SankeyNodeAlign.Left"></SankeyNodeSettings>

<!-- Top-aligned nodes -->
<SankeyNodeSettings Alignment="SankeyNodeAlign.Top"></SankeyNodeSettings>

<!-- Stretched nodes (default) -->
<SankeyNodeSettings Alignment="SankeyNodeAlign.Stretch"></SankeyNodeSettings>
```

### Node Offset

Apply additional positioning offset:

```razor
<SankeyNodeSettings 
    Alignment="SankeyNodeAlign.Left" 
    Offset="20">
</SankeyNodeSettings>
```

**Use case**: Fine-tune node placement when automatic positioning needs adjustment.

---

## Complete Styling Example

```razor
<SfSankey Nodes="@Nodes" Links="@Links" BackgroundColor="#f0f0f0">
    <!-- Comprehensive node styling -->
    <SankeyNodeSettings 
        Width="30"
        Padding="25"
        Color="#1976d2"
        Opacity="0.9"
        Alignment="SankeyNodeAlign.Stretch">
    </SankeyNodeSettings>
    
    <!-- Complementary link styling -->
    <SankeyLinkSettings 
        Color="#64b5f6" 
        Opacity="0.4">
    </SankeyLinkSettings>
    
    <!-- Label styling for visibility -->
    <SankeyLabelSettings 
        Visible="true"
        Color="#ffffff"
        FontSize="14px"
        FontWeight="600">
    </SankeyLabelSettings>
</SfSankey>
```

---

## Node Positioning

### Automatic Positioning

Nodes are automatically positioned by the Sankey algorithm based on:
1. **Link structure**: Source and target relationships
2. **Flow direction**: Left-to-right (horizontal) or top-to-bottom (vertical)
3. **Value magnitudes**: Larger flows influence positioning
4. **Node connections**: Number and weight of incoming/outgoing links

**You don't manually set node positions**. The component calculates optimal layout.

### Multi-Level Flows

Nodes are organized into columns/levels based on their position in the flow:

```csharp
// Level 1 (Sources) → Level 2 (Intermediate) → Level 3 (Destinations)
Nodes = new List<SankeyDataNode>()
{
    // Level 1: Sources (leftmost)
    new SankeyDataNode() { Id = "SourceA" },
    new SankeyDataNode() { Id = "SourceB" },
    
    // Level 2: Intermediate (middle)
    new SankeyDataNode() { Id = "ProcessX" },
    new SankeyDataNode() { Id = "ProcessY" },
    
    // Level 3: Destinations (rightmost)
    new SankeyDataNode() { Id = "Output1" },
    new SankeyDataNode() { Id = "Output2" }
};

Links = new List<SankeyDataLink>()
{
    // Level 1 → Level 2
    new SankeyDataLink() { SourceId = "SourceA", TargetId = "ProcessX", Value = 50 },
    new SankeyDataLink() { SourceId = "SourceB", TargetId = "ProcessY", Value = 40 },
    
    // Level 2 → Level 3
    new SankeyDataLink() { SourceId = "ProcessX", TargetId = "Output1", Value = 30 },
    new SankeyDataLink() { SourceId = "ProcessX", TargetId = "Output2", Value = 20 },
    new SankeyDataLink() { SourceId = "ProcessY", TargetId = "Output2", Value = 40 }
};
```

---

## Node Best Practices

### 1. Unique IDs

Ensure all node IDs are unique:

```csharp
// ✅ Good - unique IDs
new SankeyDataNode() { Id = "solar_1" }
new SankeyDataNode() { Id = "solar_2" }

// ❌ Bad - duplicate IDs
new SankeyDataNode() { Id = "solar" }
new SankeyDataNode() { Id = "solar" }  // Error!
```

### 2. Case-Sensitive IDs

Node IDs are case-sensitive:

```csharp
new SankeyDataNode() { Id = "Solar" }    // Different from
new SankeyDataNode() { Id = "solar" }    // this node
```

**Recommendation**: Use consistent casing (e.g., PascalCase or lowercase_with_underscores).

### 3. Meaningful IDs

Use descriptive IDs for maintainability:

```csharp
// ✅ Good - descriptive
new SankeyDataNode() { Id = "solar_energy_production" }
new SankeyDataNode() { Id = "residential_consumption" }

// ❌ Less clear
new SankeyDataNode() { Id = "node1" }
new SankeyDataNode() { Id = "n2" }
```

### 4. Label vs. ID

Use labels when display text differs from ID:

```csharp
// For database compatibility or technical naming
new SankeyDataNode() 
{ 
    Id = "resi_elec_2024",  // Technical ID
    Label = new SankeyDataLabel() { Text = "Residential Electricity" }  // User-friendly
}
```

### 5. Node Organization

Group related nodes logically:

```csharp
Nodes = new List<SankeyDataNode>()
{
    // Group 1: Energy sources
    new SankeyDataNode() { Id = "Solar" },
    new SankeyDataNode() { Id = "Wind" },
    
    // Group 2: Energy types
    new SankeyDataNode() { Id = "Electricity" },
    new SankeyDataNode() { Id = "Heat" },
    
    // Group 3: End users
    new SankeyDataNode() { Id = "Residential" },
    new SankeyDataNode() { Id = "Commercial" }
};
```

---

## Dynamic Node Management

### Adding Nodes Dynamically

```csharp
private void AddNewNode()
{
    string newId = $"Node_{DateTime.Now.Ticks}";
    Nodes.Add(new SankeyDataNode() 
    { 
        Id = newId,
        Label = new SankeyDataLabel() { Text = $"New Node {Nodes.Count + 1}" }
    });
    
    StateHasChanged();  // Trigger re-render
}
```

### Removing Nodes

```csharp
private void RemoveNode(string nodeId)
{
    // Remove the node
    Nodes.RemoveAll(n => n.Id == nodeId);
    
    // Remove associated links
    Links.RemoveAll(l => l.SourceId == nodeId || l.TargetId == nodeId);
    
    StateHasChanged();
}
```

### Updating Node Labels

```csharp
private void UpdateNodeLabel(string nodeId, string newText)
{
    var node = Nodes.FirstOrDefault(n => n.Id == nodeId);
    if (node != null)
    {
        if (node.Label == null)
            node.Label = new SankeyDataLabel();
        
        node.Label.Text = newText;
        StateHasChanged();
    }
}
```

---

## Troubleshooting

### Nodes Not Appearing

**Possible causes:**
1. Node collection not initialized or empty
2. Nodes not bound to component
3. No links connecting to the node (isolated nodes may not render)
4. Duplicate IDs causing conflicts

**Solutions:**
```csharp
// Ensure initialization
public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();

// Verify binding
<SfSankey Nodes="@Nodes" Links="@Links">

// Check for duplicates
var duplicates = Nodes.GroupBy(n => n.Id).Where(g => g.Count() > 1);
```

### Node Labels Not Displaying

**Possible causes:**
1. Label settings visibility is false
2. Label text is null or empty
3. Label color matches background

**Solutions:**
```razor
<SankeyLabelSettings Visible="true" Color="#000000"></SankeyLabelSettings>
```

### Nodes Overlapping

**Possible causes:**
1. Too many nodes in small space
2. Insufficient padding
3. Node width too large

**Solutions:**
```razor
<!-- Increase padding -->
<SankeyNodeSettings Padding="30"></SankeyNodeSettings>

<!-- Reduce node width -->
<SankeyNodeSettings Width="15"></SankeyNodeSettings>

<!-- Increase diagram height -->
<SfSankey Height="800px">
```

---

## Summary

Nodes are the foundation of Sankey diagrams:
- Define nodes with unique `Id` properties
- Use `Label` for custom display text
- Configure global appearance with `SankeyNodeSettings`
- The component automatically positions nodes based on link structure
- Maintain unique, case-sensitive IDs
- Group related nodes logically for maintainability

Effective node configuration creates clear, informative flow diagrams that communicate complex relationships and processes.
