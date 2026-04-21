# Data Binding in Blazor Sankey Diagram

This guide covers complete data binding concepts, models, and patterns for the Syncfusion Blazor Sankey Diagram component. Learn how to structure, bind, and dynamically update node and link data.

## Table of Contents

- [Overview](#overview)
- [Understanding Data Models](#understanding-data-models)
- [Basic Data Binding Example](#basic-data-binding-example)
- [Simplified Nodes (Without Label Object)](#simplified-nodes-without-label-object)
- [Data Binding from JSON](#data-binding-from-json)
- [Data Binding from REST API](#data-binding-from-rest-api)
- [Dynamic Data Updates](#dynamic-data-updates)
- [Data Binding from Database (Entity Framework)](#data-binding-from-database-entity-framework)
- [Data Validation and Best Practices](#data-validation-and-best-practices)
- [Common Data Binding Patterns](#common-data-binding-patterns)
- [Troubleshooting](#troubleshooting)
- [Complete Working Example](#complete-working-example)

## Overview

The Blazor Sankey Diagram visualizes flows and relationships between categories using:
- **Nodes**: Entities or stages in the flow (e.g., "Solar", "Electricity", "Residential")
- **Links**: Connections between nodes with magnitude values (e.g., Solar → Electricity: 30 units)

Data binding provides the component with structured collections that define what nodes exist and how they connect.

## Understanding Data Models

The Sankey component uses predefined data models from the `Syncfusion.Blazor.Sankey` namespace. You don't need to create custom classes—use the built-in models.

### SankeyDataNode

Represents a category or stage in the flow.

**Properties:**
- **`Id`** (string, required): Unique identifier for the node
  - Must be unique across all nodes
  - Case-sensitive
  - Used by links to reference this node
  
- **`Label`** (SankeyDataLabel, optional): Display label configuration
  - Contains `Text` property for the visible label
  - If not specified, `Id` is displayed

**Example:**
```csharp
new SankeyDataNode() 
{ 
    Id = "Solar",
    Label = new SankeyDataLabel() { Text = "Solar Energy" }
}
```

### SankeyDataLink

Represents a flow connection between two nodes.

**Properties:**
- **`SourceId`** (string, required): ID of the source node
  - Must match an existing node's `Id` exactly
  - Case-sensitive
  
- **`TargetId`** (string, required): ID of the destination node
  - Must match an existing node's `Id` exactly
  - Case-sensitive
  
- **`Value`** (double, required): Magnitude of the flow
  - Must be positive (> 0)
  - Determines the visual width of the link
  - Represents quantity, volume, or any measurable flow

**Example:**
```csharp
new SankeyDataLink() 
{ 
    SourceId = "Solar",
    TargetId = "Electricity",
    Value = 30.5
}
```

### SankeyDataLabel

Used within `SankeyDataNode` to customize label text.

**Properties:**
- **`Text`** (string): The display text for the node label

**Example:**
```csharp
new SankeyDataLabel() { Text = "Solar Energy Production" }
```

---

## Basic Data Binding Example

Here's a complete example showing energy flow from sources to consumption:

```razor
@page "/energy-flow"
@using Syncfusion.Blazor.Sankey

<h3>Energy Distribution Flow</h3>

<SfSankey Nodes="@Nodes" Links="@Links" Width="100%" Height="600px">
</SfSankey>

@code {
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    protected override void OnInitialized()
    {
        // Define nodes (categories/stages)
        Nodes = new List<SankeyDataNode>()
        {
            // Energy sources
            new SankeyDataNode() { Id = "Solar", Label = new SankeyDataLabel() { Text = "Solar" } },
            new SankeyDataNode() { Id = "Wind", Label = new SankeyDataLabel() { Text = "Wind" } },
            new SankeyDataNode() { Id = "Hydro", Label = new SankeyDataLabel() { Text = "Hydro" } },
            new SankeyDataNode() { Id = "Nuclear", Label = new SankeyDataLabel() { Text = "Nuclear" } },
            
            // Energy types
            new SankeyDataNode() { Id = "Electricity", Label = new SankeyDataLabel() { Text = "Electricity" } },
            new SankeyDataNode() { Id = "Heat", Label = new SankeyDataLabel() { Text = "Heat" } },
            
            // End users
            new SankeyDataNode() { Id = "Residential", Label = new SankeyDataLabel() { Text = "Residential" } },
            new SankeyDataNode() { Id = "Commercial", Label = new SankeyDataLabel() { Text = "Commercial" } },
            new SankeyDataNode() { Id = "Industrial", Label = new SankeyDataLabel() { Text = "Industrial" } }
        };

        // Define links (flows with magnitude)
        Links = new List<SankeyDataLink>()
        {
            // Sources to energy types
            new SankeyDataLink() { SourceId = "Solar", TargetId = "Electricity", Value = 30 },
            new SankeyDataLink() { SourceId = "Solar", TargetId = "Heat", Value = 10 },
            new SankeyDataLink() { SourceId = "Wind", TargetId = "Electricity", Value = 45 },
            new SankeyDataLink() { SourceId = "Hydro", TargetId = "Electricity", Value = 25 },
            new SankeyDataLink() { SourceId = "Nuclear", TargetId = "Electricity", Value = 50 },
            
            // Energy types to end users
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Residential", Value = 50 },
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Commercial", Value = 45 },
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Industrial", Value = 55 },
            new SankeyDataLink() { SourceId = "Heat", TargetId = "Residential", Value = 7 },
            new SankeyDataLink() { SourceId = "Heat", TargetId = "Commercial", Value = 3 }
        };
    }
}
```

---

## Simplified Nodes (Without Label Object)

If you omit the `Label` property, the node `Id` is displayed as the label:

```csharp
Nodes = new List<SankeyDataNode>()
{
    new SankeyDataNode() { Id = "Solar" },       // Displays "Solar"
    new SankeyDataNode() { Id = "Electricity" }, // Displays "Electricity"
    new SankeyDataNode() { Id = "Residential" }  // Displays "Residential"
};
```

**When to use:**
- Simple diagrams where ID = display text
- Prototyping or demos
- When node names don't need special formatting

**When to use Label:**
- Different display text than ID (ID: "ELEC", Label: "Electricity")
- Multi-word labels with spacing (ID: "north_america", Label: "North America")
- Special characters or formatting

---

## Data Binding from JSON

### Local JSON Data

Parse JSON strings or files into node and link collections:

```csharp
@code {
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    protected override void OnInitialized()
    {
        string jsonData = @"{
            ""Nodes"": [
                { ""Id"": ""Source A"", ""Label"": { ""Text"": ""Source A"" } },
                { ""Id"": ""Source B"", ""Label"": { ""Text"": ""Source B"" } },
                { ""Id"": ""Target X"", ""Label"": { ""Text"": ""Target X"" } },
                { ""Id"": ""Target Y"", ""Label"": { ""Text"": ""Target Y"" } }
            ],
            ""Links"": [
                { ""SourceId"": ""Source A"", ""TargetId"": ""Target X"", ""Value"": 45 },
                { ""SourceId"": ""Source A"", ""TargetId"": ""Target Y"", ""Value"": 25 },
                { ""SourceId"": ""Source B"", ""TargetId"": ""Target X"", ""Value"": 30 },
                { ""SourceId"": ""Source B"", ""TargetId"": ""Target Y"", ""Value"": 40 }
            ]
        }";

        var data = System.Text.Json.JsonSerializer.Deserialize<SankeyData>(jsonData);
        Nodes = data.Nodes;
        Links = data.Links;
    }

    public class SankeyData
    {
        public List<SankeyDataNode> Nodes { get; set; }
        public List<SankeyDataLink> Links { get; set; }
    }
}
```

### JSON Structure Format

The JSON structure should match this format:

```json
{
    "Nodes": [
        { 
            "Id": "Node1",
            "Label": { "Text": "Node 1 Display Name" }
        },
        { 
            "Id": "Node2",
            "Label": { "Text": "Node 2 Display Name" }
        }
    ],
    "Links": [
        { 
            "SourceId": "Node1",
            "TargetId": "Node2",
            "Value": 100
        }
    ]
}
```

**Simplified JSON (without Label):**
```json
{
    "Nodes": [
        { "Id": "Node1" },
        { "Id": "Node2" }
    ],
    "Links": [
        { "SourceId": "Node1", "TargetId": "Node2", "Value": 100 }
    ]
}
```

---

## Data Binding from REST API

Fetch Sankey data dynamically from a backend API:

```razor
@page "/api-sankey"
@using Syncfusion.Blazor.Sankey
@inject HttpClient Http

<h3>Dynamic Sankey from API</h3>

@if (Nodes.Any())
{
    <SfSankey Nodes="@Nodes" Links="@Links" Width="100%" Height="600px">
    </SfSankey>
}
else
{
    <p>Loading data...</p>
}

@code {
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    protected override async Task OnInitializedAsync()
    {
        try
        {
            // Fetch data from API endpoint
            var response = await Http.GetFromJsonAsync<SankeyDataResponse>("api/sankey/data");
            
            if (response != null)
            {
                Nodes = response.Nodes ?? new List<SankeyDataNode>();
                Links = response.Links ?? new List<SankeyDataLink>();
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error loading Sankey data: {ex.Message}");
        }
    }

    public class SankeyDataResponse
    {
        public List<SankeyDataNode> Nodes { get; set; }
        public List<SankeyDataLink> Links { get; set; }
    }
}
```

### API Controller Example (ASP.NET Core)

```csharp
using Microsoft.AspNetCore.Mvc;
using Syncfusion.Blazor.Sankey;

[ApiController]
[Route("api/[controller]")]
public class SankeyController : ControllerBase
{
    [HttpGet("data")]
    public ActionResult<SankeyDataResponse> GetData()
    {
        var response = new SankeyDataResponse
        {
            Nodes = new List<SankeyDataNode>
            {
                new SankeyDataNode { Id = "Product A", Label = new SankeyDataLabel { Text = "Product A" } },
                new SankeyDataNode { Id = "Product B", Label = new SankeyDataLabel { Text = "Product B" } },
                new SankeyDataNode { Id = "Region 1", Label = new SankeyDataLabel { Text = "Region 1" } },
                new SankeyDataNode { Id = "Region 2", Label = new SankeyDataLabel { Text = "Region 2" } }
            },
            Links = new List<SankeyDataLink>
            {
                new SankeyDataLink { SourceId = "Product A", TargetId = "Region 1", Value = 120 },
                new SankeyDataLink { SourceId = "Product A", TargetId = "Region 2", Value = 80 },
                new SankeyDataLink { SourceId = "Product B", TargetId = "Region 1", Value = 95 },
                new SankeyDataLink { SourceId = "Product B", TargetId = "Region 2", Value = 110 }
            }
        };

        return Ok(response);
    }
}

public class SankeyDataResponse
{
    public List<SankeyDataNode> Nodes { get; set; }
    public List<SankeyDataLink> Links { get; set; }
}
```

---

## Dynamic Data Updates

Update Sankey data dynamically in response to user actions or events:

```razor
@page "/dynamic-sankey"
@using Syncfusion.Blazor.Sankey

<h3>Dynamic Data Updates</h3>

<button @onclick="UpdateData">Update Flow Data</button>
<button @onclick="AddNode">Add New Node</button>

<SfSankey Nodes="@Nodes" Links="@Links" Width="100%" Height="600px">
</SfSankey>

@code {
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();
    private int nodeCounter = 3;

    protected override void OnInitialized()
    {
        LoadInitialData();
    }

    private void LoadInitialData()
    {
        Nodes = new List<SankeyDataNode>()
        {
            new SankeyDataNode() { Id = "A", Label = new SankeyDataLabel() { Text = "Node A" } },
            new SankeyDataNode() { Id = "B", Label = new SankeyDataLabel() { Text = "Node B" } },
            new SankeyDataNode() { Id = "C", Label = new SankeyDataLabel() { Text = "Node C" } }
        };

        Links = new List<SankeyDataLink>()
        {
            new SankeyDataLink() { SourceId = "A", TargetId = "B", Value = 50 },
            new SankeyDataLink() { SourceId = "A", TargetId = "C", Value = 30 }
        };
    }

    private void UpdateData()
    {
        // Modify existing link values
        Links[0].Value += 10;
        Links[1].Value += 5;
        
        // Trigger re-render
        StateHasChanged();
    }

    private void AddNode()
    {
        string newId = $"Node{nodeCounter++}";
        
        // Add new node
        Nodes.Add(new SankeyDataNode() 
        { 
            Id = newId,
            Label = new SankeyDataLabel() { Text = $"New {newId}" }
        });

        // Add link to new node
        Links.Add(new SankeyDataLink() 
        { 
            SourceId = "A",
            TargetId = newId,
            Value = 25
        });

        // Trigger re-render
        StateHasChanged();
    }
}
```

---

## Data Binding from Database (Entity Framework)

Load Sankey data from a database:

```csharp
@page "/db-sankey"
@using Syncfusion.Blazor.Sankey
@inject IDbContextFactory<AppDbContext> DbFactory

<SfSankey Nodes="@Nodes" Links="@Links" Width="100%" Height="600px">
</SfSankey>

@code {
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    protected override async Task OnInitializedAsync()
    {
        using var context = await DbFactory.CreateDbContextAsync();

        // Load nodes from database
        var dbNodes = await context.FlowNodes.ToListAsync();
        Nodes = dbNodes.Select(n => new SankeyDataNode
        {
            Id = n.NodeId,
            Label = new SankeyDataLabel { Text = n.DisplayName }
        }).ToList();

        // Load links from database
        var dbLinks = await context.FlowLinks.ToListAsync();
        Links = dbLinks.Select(l => new SankeyDataLink
        {
            SourceId = l.SourceNodeId,
            TargetId = l.TargetNodeId,
            Value = l.FlowValue
        }).ToList();
    }
}
```

---

## Data Validation and Best Practices

### Required Validations

1. **Unique Node IDs**: Each node must have a unique `Id`
2. **Valid Link References**: All `SourceId` and `TargetId` must reference existing nodes
3. **Positive Values**: Link `Value` must be greater than 0
4. **No Null Collections**: Always initialize `Nodes` and `Links` lists

### Validation Example

```csharp
private bool ValidateSankeyData()
{
    // Check for unique node IDs
    var duplicateIds = Nodes.GroupBy(n => n.Id)
                             .Where(g => g.Count() > 1)
                             .Select(g => g.Key);
    if (duplicateIds.Any())
    {
        Console.WriteLine($"Duplicate node IDs: {string.Join(", ", duplicateIds)}");
        return false;
    }

    // Check for valid link references
    var nodeIds = Nodes.Select(n => n.Id).ToHashSet();
    foreach (var link in Links)
    {
        if (!nodeIds.Contains(link.SourceId))
        {
            Console.WriteLine($"Invalid SourceId: {link.SourceId}");
            return false;
        }
        if (!nodeIds.Contains(link.TargetId))
        {
            Console.WriteLine($"Invalid TargetId: {link.TargetId}");
            return false;
        }
        if (link.Value <= 0)
        {
            Console.WriteLine($"Invalid value: {link.Value} for {link.SourceId} -> {link.TargetId}");
            return false;
        }
    }

    return true;
}
```

### Best Practices

**1. Case-Sensitive IDs**: Node IDs and link references are case-sensitive
```csharp
// ❌ Won't work - case mismatch
new SankeyDataNode() { Id = "Solar" }
new SankeyDataLink() { SourceId = "solar", TargetId = "X", Value = 10 }

// ✅ Works - exact match
new SankeyDataNode() { Id = "Solar" }
new SankeyDataLink() { SourceId = "Solar", TargetId = "X", Value = 10 }
```

**2. Initialize Collections**: Always initialize lists before binding
```csharp
// ✅ Good
public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
public List<SankeyDataLink> Links = new List<SankeyDataLink>();

// ❌ Bad - null references cause errors
public List<SankeyDataNode> Nodes;
public List<SankeyDataLink> Links;
```

**3. Avoid Circular Flows**: While technically possible, avoid creating loops for clarity
```csharp
// ❌ Avoid - creates confusion
new SankeyDataLink() { SourceId = "A", TargetId = "B", Value = 10 }
new SankeyDataLink() { SourceId = "B", TargetId = "A", Value = 5 }
```

**4. Balance Flow Values**: Ensure input and output flows are logical
```csharp
// ✅ Balanced - Node B receives 100, distributes 100
// A → B: 100
// B → C: 60
// B → D: 40
```

**5. Use Meaningful IDs**: Choose descriptive IDs for better maintainability
```csharp
// ✅ Good
new SankeyDataNode() { Id = "solar_energy", Label = new SankeyDataLabel() { Text = "Solar" } }

// ❌ Less clear
new SankeyDataNode() { Id = "node1", Label = new SankeyDataLabel() { Text = "Solar" } }
```

---

## Common Data Binding Patterns

### Multi-Level Flow

```csharp
// Source → Intermediate → Destination
Nodes = new List<SankeyDataNode>()
{
    new SankeyDataNode() { Id = "Sources" },
    new SankeyDataNode() { Id = "Processing" },
    new SankeyDataNode() { Id = "Distribution" },
    new SankeyDataNode() { Id = "EndUsers" }
};

Links = new List<SankeyDataLink>()
{
    new SankeyDataLink() { SourceId = "Sources", TargetId = "Processing", Value = 100 },
    new SankeyDataLink() { SourceId = "Processing", TargetId = "Distribution", Value = 90 }, // 10% loss
    new SankeyDataLink() { SourceId = "Distribution", TargetId = "EndUsers", Value = 85 }   // 5% loss
};
```

### Branching Flow

```csharp
// One source to multiple destinations
Links = new List<SankeyDataLink>()
{
    new SankeyDataLink() { SourceId = "MainSource", TargetId = "Branch1", Value = 40 },
    new SankeyDataLink() { SourceId = "MainSource", TargetId = "Branch2", Value = 35 },
    new SankeyDataLink() { SourceId = "MainSource", TargetId = "Branch3", Value = 25 }
};
```

### Converging Flow

```csharp
// Multiple sources to one destination
Links = new List<SankeyDataLink>()
{
    new SankeyDataLink() { SourceId = "Source1", TargetId = "Destination", Value = 50 },
    new SankeyDataLink() { SourceId = "Source2", TargetId = "Destination", Value = 30 },
    new SankeyDataLink() { SourceId = "Source3", TargetId = "Destination", Value = 20 }
};
```

---

## Troubleshooting

**Links not appearing:**
- Verify `SourceId` and `TargetId` exactly match node `Id` values (case-sensitive)
- Ensure `Value` is greater than 0
- Check that referenced nodes exist in the `Nodes` collection

**Nodes not showing:**
- Confirm nodes have unique `Id` values
- Check that `Nodes` collection is properly bound to the component
- Verify namespaces are imported

**Data not updating:**
- Call `StateHasChanged()` after modifying collections
- Consider creating new list instances instead of modifying existing ones
- Use `@key` directive if rendering multiple Sankeys

---

## Complete Working Example

```razor
@page "/complete-example"
@using Syncfusion.Blazor.Sankey

<h3>Supply Chain Flow</h3>

<SfSankey Nodes="@Nodes" Links="@Links" Width="100%" Height="600px">
    <SankeyNodeSettings Width="20" Padding="25"></SankeyNodeSettings>
    <SankeyLabelSettings Visible="true" FontSize="14px"></SankeyLabelSettings>
</SfSankey>

@code {
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    protected override void OnInitialized()
    {
        Nodes = new List<SankeyDataNode>()
        {
            new SankeyDataNode() { Id = "Suppliers", Label = new SankeyDataLabel() { Text = "Suppliers" } },
            new SankeyDataNode() { Id = "Manufacturing", Label = new SankeyDataLabel() { Text = "Manufacturing" } },
            new SankeyDataNode() { Id = "Warehouse", Label = new SankeyDataLabel() { Text = "Warehouse" } },
            new SankeyDataNode() { Id = "Retail", Label = new SankeyDataLabel() { Text = "Retail" } },
            new SankeyDataNode() { Id = "Online", Label = new SankeyDataLabel() { Text = "Online Sales" } },
            new SankeyDataNode() { Id = "Customers", Label = new SankeyDataLabel() { Text = "Customers" } }
        };

        Links = new List<SankeyDataLink>()
        {
            new SankeyDataLink() { SourceId = "Suppliers", TargetId = "Manufacturing", Value = 500 },
            new SankeyDataLink() { SourceId = "Manufacturing", TargetId = "Warehouse", Value = 450 },
            new SankeyDataLink() { SourceId = "Warehouse", TargetId = "Retail", Value = 300 },
            new SankeyDataLink() { SourceId = "Warehouse", TargetId = "Online", Value = 150 },
            new SankeyDataLink() { SourceId = "Retail", TargetId = "Customers", Value = 280 },
            new SankeyDataLink() { SourceId = "Online", TargetId = "Customers", Value = 145 }
        };
    }
}
```

This example demonstrates a complete supply chain flow from suppliers through manufacturing, warehousing, and distribution channels to end customers, with realistic value diminishment at each stage.
