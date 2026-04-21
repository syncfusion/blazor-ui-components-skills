# Links in Blazor Sankey Diagram

This guide provides complete information about defining, configuring, and styling links in the Syncfusion Blazor Sankey Diagram. Links represent flow connections between nodes, visually showing the magnitude and direction of relationships.

## Table of Contents

- [Overview](#overview)
- [Link Structure](#link-structure)
- [Basic Link Configuration](#basic-link-configuration)
- [Complete Working Example](#complete-working-example)
- [Customizing Link Appearance with SankeyLinkSettings](#customizing-link-appearance-with-sankeylinksettings)
- [Complete Styling Example](#complete-styling-example)
- [Link Flow Patterns](#link-flow-patterns)
- [Flow Value Considerations](#flow-value-considerations)
- [Link Best Practices](#link-best-practices)
- [Dynamic Link Management](#dynamic-link-management)
- [Troubleshooting](#troubleshooting)
- [Complete Working Example: Energy Distribution](#complete-working-example-energy-distribution)
- [Summary](#summary)

## Overview

Links are the connective elements in a Sankey diagram that represent:
- **Flows**: Movement of resources, energy, data, money
- **Relationships**: Connections between categories or stages
- **Magnitude**: Quantity or volume represented by link width
- **Direction**: Flow from source to target nodes

The visual width of each link is proportional to its `Value` property, making it easy to compare flow magnitudes at a glance.

## Link Structure

### SankeyDataLink Model

Links are defined using the `SankeyDataLink` class from `Syncfusion.Blazor.Sankey`:

```csharp
public class SankeyDataLink
{
    public string SourceId { get; set; }   // Required: Source node ID
    public string TargetId { get; set; }   // Required: Target node ID
    public double Value { get; set; }      // Required: Flow magnitude
}
```

**Properties:**
- **`SourceId`** (required): ID of the source (origin) node
  - Must exactly match an existing node's `Id`
  - Case-sensitive
  
- **`TargetId`** (required): ID of the target (destination) node
  - Must exactly match an existing node's `Id`
  - Case-sensitive
  
- **`Value`** (required): Magnitude or quantity of the flow
  - Must be positive (> 0)
  - Determines the visual width of the link
  - Can be any unit: dollars, kilowatts, liters, items, etc.

---

## Basic Link Configuration

### Simple Link Definition

Define links by creating a collection of `SankeyDataLink` objects:

```csharp
@code {
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    protected override void OnInitialized()
    {
        // Define nodes first
        Nodes = new List<SankeyDataNode>()
        {
            new SankeyDataNode() { Id = "A" },
            new SankeyDataNode() { Id = "B" },
            new SankeyDataNode() { Id = "C" },
            new SankeyDataNode() { Id = "D" }
        };

        // Define links connecting nodes
        Links = new List<SankeyDataLink>()
        {
            new SankeyDataLink() { SourceId = "A", TargetId = "B", Value = 50 },
            new SankeyDataLink() { SourceId = "A", TargetId = "C", Value = 30 },
            new SankeyDataLink() { SourceId = "B", TargetId = "D", Value = 40 },
            new SankeyDataLink() { SourceId = "C", TargetId = "D", Value = 25 }
        };
    }
}
```

**Flow represented:**
- A splits into B (50 units) and C (30 units)
- B flows to D (40 units)
- C flows to D (25 units)
- D receives total of 65 units (40 + 25)

---

## Complete Working Example

Here's a complete example showing supply chain flow:

```razor
@page "/supply-chain-links"
@using Syncfusion.Blazor.Sankey

<h3>Supply Chain Flow - Link Configuration</h3>

<SfSankey Nodes="@Nodes" Links="@Links" Width="100%" Height="600px" BackgroundColor="#ffffff">
    <SankeyNodeSettings Color="#455a64" Width="25" Padding="20"></SankeyNodeSettings>
    <SankeyLinkSettings 
        Color="#90a4ae" 
        Opacity="0.5"
        HighlightOpacity="0.9"
        InactiveOpacity="0.2">
    </SankeyLinkSettings>
    <SankeyLabelSettings Visible="true" Color="#263238" FontSize="13px"></SankeyLabelSettings>
</SfSankey>

@code {
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    protected override void OnInitialized()
    {
        // Define supply chain nodes
        Nodes = new List<SankeyDataNode>()
        {
            // Level 1: Suppliers
            new SankeyDataNode() { Id = "Supplier A", Label = new SankeyDataLabel() { Text = "Supplier A" } },
            new SankeyDataNode() { Id = "Supplier B", Label = new SankeyDataLabel() { Text = "Supplier B" } },
            new SankeyDataNode() { Id = "Supplier C", Label = new SankeyDataLabel() { Text = "Supplier C" } },
            
            // Level 2: Manufacturing
            new SankeyDataNode() { Id = "Factory 1", Label = new SankeyDataLabel() { Text = "Factory 1" } },
            new SankeyDataNode() { Id = "Factory 2", Label = new SankeyDataLabel() { Text = "Factory 2" } },
            
            // Level 3: Distribution
            new SankeyDataNode() { Id = "Warehouse", Label = new SankeyDataLabel() { Text = "Warehouse" } },
            
            // Level 4: Retail
            new SankeyDataNode() { Id = "Retail Stores", Label = new SankeyDataLabel() { Text = "Retail Stores" } },
            new SankeyDataNode() { Id = "Online", Label = new SankeyDataLabel() { Text = "Online Sales" } },
            
            // Level 5: Customers
            new SankeyDataNode() { Id = "Customers", Label = new SankeyDataLabel() { Text = "Customers" } }
        };

        // Define links showing flow with realistic values
        Links = new List<SankeyDataLink>()
        {
            // Suppliers to Manufacturing
            new SankeyDataLink() { SourceId = "Supplier A", TargetId = "Factory 1", Value = 500 },
            new SankeyDataLink() { SourceId = "Supplier A", TargetId = "Factory 2", Value = 200 },
            new SankeyDataLink() { SourceId = "Supplier B", TargetId = "Factory 1", Value = 300 },
            new SankeyDataLink() { SourceId = "Supplier B", TargetId = "Factory 2", Value = 400 },
            new SankeyDataLink() { SourceId = "Supplier C", TargetId = "Factory 2", Value = 250 },
            
            // Manufacturing to Distribution (with loss)
            new SankeyDataLink() { SourceId = "Factory 1", TargetId = "Warehouse", Value = 750 },
            new SankeyDataLink() { SourceId = "Factory 2", TargetId = "Warehouse", Value = 800 },
            
            // Distribution to Retail
            new SankeyDataLink() { SourceId = "Warehouse", TargetId = "Retail Stores", Value = 1000 },
            new SankeyDataLink() { SourceId = "Warehouse", TargetId = "Online", Value = 500 },
            
            // Retail to Customers (with returns/losses)
            new SankeyDataLink() { SourceId = "Retail Stores", TargetId = "Customers", Value = 950 },
            new SankeyDataLink() { SourceId = "Online", TargetId = "Customers", Value = 480 }
        };
    }
}
```

---

## Customizing Link Appearance with SankeyLinkSettings

The `SankeyLinkSettings` component allows you to globally configure how all links appear in the diagram.

### Available Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Color` | string | null | Base color for links (hex, rgb, named) |
| `ColorType` | SankeyColorType | None | Color application strategy |
| `Opacity` | double | 0.8 | Default link transparency (0.0 to 1.0) |
| `HighlightOpacity` | double | 0.8 | Opacity when hovering over link |
| `InactiveOpacity` | double | 0.2 | Opacity for non-hovered links during interaction |

### Basic Link Styling

```razor
<SfSankey Nodes="@Nodes" Links="@Links">
    <SankeyLinkSettings 
        Color="#2196F3" 
        Opacity="0.6">
    </SankeyLinkSettings>
</SfSankey>
```

### Link Color

Set a uniform color for all links:

```razor
<!-- Hex color -->
<SankeyLinkSettings Color="#FF5722"></SankeyLinkSettings>

<!-- RGB color -->
<SankeyLinkSettings Color="rgb(255, 87, 34)"></SankeyLinkSettings>

<!-- Named color -->
<SankeyLinkSettings Color="coral"></SankeyLinkSettings>

<!-- RGBA with transparency -->
<SankeyLinkSettings Color="rgba(255, 87, 34, 0.6)"></SankeyLinkSettings>
```

### Link Color Type

Control how link colors are derived:

```csharp
public enum SankeyColorType
{
    None,    // Use Color property (default)
    Source,  // Match source node color
    Target,  // Match target node color
    Blend    // Gradient from source to target
}
```

**Examples:**

```razor
<!-- Match source node color -->
<SankeyLinkSettings ColorType="SankeyColorType.Source"></SankeyLinkSettings>

<!-- Match target node color -->
<SankeyLinkSettings ColorType="SankeyColorType.Target"></SankeyLinkSettings>

<!-- Gradient blend from source to target -->
<SankeyLinkSettings ColorType="SankeyColorType.Blend"></SankeyLinkSettings>
```

**Use cases:**
- **Source**: Emphasize origin of flows
- **Target**: Emphasize destination of flows
- **Blend**: Show transition between source and target
- **None**: Uniform color for all links

### Link Opacity

Control transparency of links:

```razor
<!-- Standard opacity (default) -->
<SankeyLinkSettings Opacity="0.8"></SankeyLinkSettings>

<!-- More transparent for subtle appearance -->
<SankeyLinkSettings Opacity="0.4"></SankeyLinkSettings>

<!-- More opaque for emphasis -->
<SankeyLinkSettings Opacity="0.95"></SankeyLinkSettings>
```

### Interactive Opacity

Control how links appear during hover interactions:

```razor
<SankeyLinkSettings 
    Opacity="0.5"
    HighlightOpacity="1.0"
    InactiveOpacity="0.1">
</SankeyLinkSettings>
```

**Behavior:**
- **Opacity**: Normal state
- **HighlightOpacity**: Applied when hovering over the link
- **InactiveOpacity**: Applied to OTHER links when one link is hovered

**Example interaction:**
- User hovers over Link A
- Link A opacity changes to `HighlightOpacity` (1.0 - fully visible)
- All other links change to `InactiveOpacity` (0.1 - nearly transparent)
- Creates focus on the hovered link

---

## Complete Styling Example

```razor
<SfSankey Nodes="@Nodes" Links="@Links">
    <!-- Node styling -->
    <SankeyNodeSettings 
        Width="30" 
        Padding="25" 
        Color="#1565c0">
    </SankeyNodeSettings>
    
    <!-- Comprehensive link styling -->
    <SankeyLinkSettings 
        Color="#42a5f5"
        ColorType="SankeyColorType.Blend"
        Opacity="0.6"
        HighlightOpacity="0.95"
        InactiveOpacity="0.15">
    </SankeyLinkSettings>
    
    <!-- Label styling -->
    <SankeyLabelSettings 
        Visible="true" 
        Color="#ffffff" 
        FontSize="14px">
    </SankeyLabelSettings>
</SfSankey>
```

---

## Link Flow Patterns

### One-to-One Flow

Single source to single destination:

```csharp
Links = new List<SankeyDataLink>()
{
    new SankeyDataLink() { SourceId = "A", TargetId = "B", Value = 100 }
};
```

### One-to-Many Flow (Branching)

Single source splitting into multiple destinations:

```csharp
Links = new List<SankeyDataLink>()
{
    new SankeyDataLink() { SourceId = "Source", TargetId = "Dest1", Value = 40 },
    new SankeyDataLink() { SourceId = "Source", TargetId = "Dest2", Value = 35 },
    new SankeyDataLink() { SourceId = "Source", TargetId = "Dest3", Value = 25 }
};
// Source distributes 100 units total across 3 destinations
```

### Many-to-One Flow (Converging)

Multiple sources flowing into single destination:

```csharp
Links = new List<SankeyDataLink>()
{
    new SankeyDataLink() { SourceId = "Source1", TargetId = "Destination", Value = 50 },
    new SankeyDataLink() { SourceId = "Source2", TargetId = "Destination", Value = 30 },
    new SankeyDataLink() { SourceId = "Source3", TargetId = "Destination", Value = 20 }
};
// Destination receives 100 units total from 3 sources
```

### Many-to-Many Flow (Complex Network)

Multiple sources to multiple destinations:

```csharp
Links = new List<SankeyDataLink>()
{
    new SankeyDataLink() { SourceId = "A", TargetId = "X", Value = 30 },
    new SankeyDataLink() { SourceId = "A", TargetId = "Y", Value = 20 },
    new SankeyDataLink() { SourceId = "B", TargetId = "X", Value = 25 },
    new SankeyDataLink() { SourceId = "B", TargetId = "Y", Value = 35 },
    new SankeyDataLink() { SourceId = "C", TargetId = "X", Value = 15 },
    new SankeyDataLink() { SourceId = "C", TargetId = "Z", Value = 30 }
};
```

### Multi-Level Flow

Flow through intermediate stages:

```csharp
// Level 1 → Level 2 → Level 3
Links = new List<SankeyDataLink>()
{
    // Stage 1 to Stage 2
    new SankeyDataLink() { SourceId = "Input1", TargetId = "Process1", Value = 100 },
    new SankeyDataLink() { SourceId = "Input2", TargetId = "Process2", Value = 80 },
    
    // Stage 2 to Stage 3
    new SankeyDataLink() { SourceId = "Process1", TargetId = "Output1", Value = 60 },
    new SankeyDataLink() { SourceId = "Process1", TargetId = "Output2", Value = 35 },  // Loss of 5
    new SankeyDataLink() { SourceId = "Process2", TargetId = "Output2", Value = 75 }   // Loss of 5
};
```

---

## Flow Value Considerations

### Representing Losses

Show process inefficiencies or losses:

```csharp
// Input: 100 units → Output: 90 units (10 lost)
Links = new List<SankeyDataLink>()
{
    new SankeyDataLink() { SourceId = "Input", TargetId = "Process", Value = 100 },
    new SankeyDataLink() { SourceId = "Process", TargetId = "Output", Value = 90 },
    new SankeyDataLink() { SourceId = "Process", TargetId = "Waste", Value = 10 }
};
```

### Balanced vs. Unbalanced Flows

**Balanced** (recommended for clarity):
```csharp
// Input = Output for each node
// Node A: In=0, Out=100
// Node B: In=100, Out=100
// Node C: In=100, Out=0
Links = new List<SankeyDataLink>()
{
    new SankeyDataLink() { SourceId = "A", TargetId = "B", Value = 100 },
    new SankeyDataLink() { SourceId = "B", TargetId = "C", Value = 100 }
};
```

**Unbalanced** (for showing losses/gains):
```csharp
// Node B has loss
// Node B: In=100, Out=85 (15 lost)
Links = new List<SankeyDataLink>()
{
    new SankeyDataLink() { SourceId = "A", TargetId = "B", Value = 100 },
    new SankeyDataLink() { SourceId = "B", TargetId = "C", Value = 60 },
    new SankeyDataLink() { SourceId = "B", TargetId = "D", Value = 25 }
};
```

### Value Scales

Links can use any numeric scale:

```csharp
// Dollars
new SankeyDataLink() { SourceId = "Revenue", TargetId = "Expenses", Value = 250000 }

// Kilowatts
new SankeyDataLink() { SourceId = "Solar", TargetId = "Grid", Value = 45.5 }

// Percentage
new SankeyDataLink() { SourceId = "Traffic", TargetId = "Conversions", Value = 2.8 }

// Count
new SankeyDataLink() { SourceId = "Inventory", TargetId = "Sold", Value = 1250 }
```

**Note**: All values in a diagram should use the same unit for consistency.

---

## Link Best Practices

### 1. Valid Node References

Ensure all SourceId and TargetId match existing node IDs exactly:

```csharp
// ✅ Good - exact match
Nodes.Add(new SankeyDataNode() { Id = "Solar Energy" });
Links.Add(new SankeyDataLink() { SourceId = "Solar Energy", TargetId = "Grid", Value = 50 });

// ❌ Bad - case mismatch
Nodes.Add(new SankeyDataNode() { Id = "Solar Energy" });
Links.Add(new SankeyDataLink() { SourceId = "solar energy", TargetId = "Grid", Value = 50 });
```

### 2. Positive Values Only

Link values must be greater than zero:

```csharp
// ✅ Good
new SankeyDataLink() { SourceId = "A", TargetId = "B", Value = 0.1 }  // Minimum valid
new SankeyDataLink() { SourceId = "A", TargetId = "B", Value = 1000 }

// ❌ Bad
new SankeyDataLink() { SourceId = "A", TargetId = "B", Value = 0 }   // Zero not allowed
new SankeyDataLink() { SourceId = "A", TargetId = "B", Value = -50 } // Negative not allowed
```

### 3. Avoid Circular Flows

While technically possible, circular flows can be confusing:

```csharp
// ❌ Avoid - creates loop
new SankeyDataLink() { SourceId = "A", TargetId = "B", Value = 10 }
new SankeyDataLink() { SourceId = "B", TargetId = "A", Value = 5 }

// ✅ Better - clear direction
new SankeyDataLink() { SourceId = "A", TargetId = "B", Value = 10 }
new SankeyDataLink() { SourceId = "B", TargetId = "C", Value = 10 }
```

### 4. Consistent Units

Use the same unit across all links:

```csharp
// ✅ Good - all in kilowatts
new SankeyDataLink() { SourceId = "Solar", TargetId = "Grid", Value = 50 }     // kW
new SankeyDataLink() { SourceId = "Wind", TargetId = "Grid", Value = 75 }      // kW

// ❌ Bad - mixed units
new SankeyDataLink() { SourceId = "Solar", TargetId = "Grid", Value = 50 }     // kW
new SankeyDataLink() { SourceId = "Wind", TargetId = "Grid", Value = 75000 }   // Watts
```

### 5. Meaningful Values

Use realistic, proportional values:

```csharp
// ✅ Good - clear proportions
new SankeyDataLink() { SourceId = "A", TargetId = "B", Value = 100 }
new SankeyDataLink() { SourceId = "A", TargetId = "C", Value = 50 }
new SankeyDataLink() { SourceId = "A", TargetId = "D", Value = 25 }
// Clearly shows B gets 2x more than C, and 4x more than D
```

---

## Dynamic Link Management

### Adding Links Dynamically

```csharp
private void AddNewLink(string sourceId, string targetId, double value)
{
    // Validate nodes exist
    if (!Nodes.Any(n => n.Id == sourceId) || !Nodes.Any(n => n.Id == targetId))
    {
        Console.WriteLine("Invalid node reference");
        return;
    }

    // Add link
    Links.Add(new SankeyDataLink() 
    { 
        SourceId = sourceId,
        TargetId = targetId,
        Value = value
    });
    
    StateHasChanged();
}
```

### Removing Links

```csharp
private void RemoveLink(string sourceId, string targetId)
{
    Links.RemoveAll(l => l.SourceId == sourceId && l.TargetId == targetId);
    StateHasChanged();
}
```

### Updating Link Values

```csharp
private void UpdateLinkValue(string sourceId, string targetId, double newValue)
{
    var link = Links.FirstOrDefault(l => l.SourceId == sourceId && l.TargetId == targetId);
    if (link != null && newValue > 0)
    {
        link.Value = newValue;
        StateHasChanged();
    }
}
```

---

## Troubleshooting

### Links Not Appearing

**Possible causes:**
1. SourceId or TargetId doesn't match any node ID (case-sensitive)
2. Value is zero or negative
3. Links collection is empty or not bound
4. Referenced nodes don't exist

**Solutions:**
```csharp
// Validate link references
foreach (var link in Links)
{
    if (!Nodes.Any(n => n.Id == link.SourceId))
        Console.WriteLine($"Invalid SourceId: {link.SourceId}");
    
    if (!Nodes.Any(n => n.Id == link.TargetId))
        Console.WriteLine($"Invalid TargetId: {link.TargetId}");
    
    if (link.Value <= 0)
        Console.WriteLine($"Invalid Value: {link.Value}");
}
```

### Links Too Transparent

**Solution:**
```razor
<SankeyLinkSettings Opacity="0.9"></SankeyLinkSettings>
```

### Links Not Interactive

**Solution:**
```razor
<!-- Ensure highlight and inactive opacities are set -->
<SankeyLinkSettings 
    Opacity="0.5"
    HighlightOpacity="1.0"
    InactiveOpacity="0.2">
</SankeyLinkSettings>
```

### Link Colors Not Showing

**Possible causes:**
1. Color string is invalid
2. ColorType is set but nodes don't have colors

**Solutions:**
```razor
<!-- Use valid color -->
<SankeyLinkSettings Color="#2196F3"></SankeyLinkSettings>

<!-- If using ColorType, ensure nodes have colors -->
<SankeyNodeSettings Color="#1565c0"></SankeyNodeSettings>
<SankeyLinkSettings ColorType="SankeyColorType.Source"></SankeyLinkSettings>
```

---

## Complete Working Example: Energy Distribution

```razor
@page "/energy-links"
@using Syncfusion.Blazor.Sankey

<h3>Energy Distribution Network</h3>

<SfSankey Nodes="@Nodes" Links="@Links" Width="100%" Height="700px">
    <SankeyNodeSettings Width="30" Padding="20" Color="#00695c"></SankeyNodeSettings>
    <SankeyLinkSettings 
        ColorType="SankeyColorType.Blend"
        Opacity="0.5"
        HighlightOpacity="0.95"
        InactiveOpacity="0.15">
    </SankeyLinkSettings>
    <SankeyLabelSettings Visible="true" Color="#ffffff" FontSize="13px" FontWeight="600"></SankeyLabelSettings>
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
            new SankeyDataNode() { Id = "Hydro", Label = new SankeyDataLabel() { Text = "Hydro" } },
            new SankeyDataNode() { Id = "Grid", Label = new SankeyDataLabel() { Text = "Power Grid" } },
            new SankeyDataNode() { Id = "Residential", Label = new SankeyDataLabel() { Text = "Residential" } },
            new SankeyDataNode() { Id = "Commercial", Label = new SankeyDataLabel() { Text = "Commercial" } },
            new SankeyDataNode() { Id = "Industrial", Label = new SankeyDataLabel() { Text = "Industrial" } },
            new SankeyDataNode() { Id = "Losses", Label = new SankeyDataLabel() { Text = "Transmission Losses" } }
        };

        Links = new List<SankeyDataLink>()
        {
            // Generation to Grid
            new SankeyDataLink() { SourceId = "Solar", TargetId = "Grid", Value = 250 },
            new SankeyDataLink() { SourceId = "Wind", TargetId = "Grid", Value = 350 },
            new SankeyDataLink() { SourceId = "Hydro", TargetId = "Grid", Value = 200 },
            
            // Grid to Consumers (with losses)
            new SankeyDataLink() { SourceId = "Grid", TargetId = "Residential", Value = 280 },
            new SankeyDataLink() { SourceId = "Grid", TargetId = "Commercial", Value = 240 },
            new SankeyDataLink() { SourceId = "Grid", TargetId = "Industrial", Value = 220 },
            new SankeyDataLink() { SourceId = "Grid", TargetId = "Losses", Value = 60 }
        };
    }
}
```

This example demonstrates realistic energy distribution with transmission losses, showing how links connect generation sources through the grid to end consumers.

---

## Summary

Links define the flows in Sankey diagrams:
- Connect nodes using `SourceId` and `TargetId`
- Represent magnitude with the `Value` property
- Configure global appearance with `SankeyLinkSettings`
- Support multiple color strategies via `ColorType`
- Enable interactive highlighting with opacity settings
- Must reference valid, existing nodes with exact case-sensitive IDs
- Should use positive values with consistent units

Effective link configuration creates clear, informative flow visualizations that accurately represent relationships and magnitudes.
