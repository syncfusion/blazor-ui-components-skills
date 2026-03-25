# Data Binding in Blazor Diagram

## Table of Contents
- [Overview](#overview)
- [Local Data Binding](#local-data-binding)
- [Hierarchical Data Binding](#hierarchical-data-binding)
- [Accessing Custom Data in NodeCreating](#accessing-custom-data-in-nodecreating)
- [Remote Data Binding](#remote-data-binding)
- [Common Gotchas](#common-gotchas)

---

## Overview

Instead of manually defining every node and connector, the diagram can auto-generate them from a data source. Define a model class with `ID` and `ParentID` fields, pass it to `DataSourceSettings`, and let the layout engine handle placement.

**When to use data binding:**
- Loading org chart data from a database
- Rendering hierarchical trees from API data
- Building diagrams from any collection of objects with parent-child relationships

---

## Local Data Binding

Bind a flat list to automatically generate a hierarchical diagram:

```razor
@using Syncfusion.Blazor.Diagram

<SfDiagramComponent Height="600px"
                    NodeCreating="OnNodeCreating"
                    ConnectorCreating="OnConnectorCreating">
    <DataSourceSettings ID="Id" ParentID="ParentId" DataSource="@data" />
    <Layout Type="LayoutType.HierarchicalTree"
            HorizontalSpacing="40" VerticalSpacing="40" />
</SfDiagramComponent>

@code {
    public class OrgData
    {
        public string Id { get; set; }
        public string Name { get; set; }
        public string Role { get; set; }
        public string ParentId { get; set; }   // empty = root
    }

    List<OrgData> data = new()
    {
        new OrgData { Id = "1", Name = "CEO",        Role = "Director",  ParentId = "" },
        new OrgData { Id = "2", Name = "CTO",        Role = "Manager",   ParentId = "1" },
        new OrgData { Id = "3", Name = "CFO",        Role = "Manager",   ParentId = "1" },
        new OrgData { Id = "4", Name = "Dev Lead",   Role = "Lead",      ParentId = "2" },
        new OrgData { Id = "5", Name = "Designer",   Role = "Creative",  ParentId = "3" },
    };

    private void OnNodeCreating(IDiagramObject obj)
    {
        var node = obj as Node;
        node.Width = 120; node.Height = 50;
        node.Style = new ShapeStyle { Fill = "#6BA5D7", StrokeColor = "white" };
    }

    private void OnConnectorCreating(IDiagramObject obj)
    {
        (obj as Connector).Type = ConnectorSegmentType.Orthogonal;
    }
}
```

**Key `DataSourceSettings` properties:**

| Property | Description |
|----------|-------------|
| `ID` | Name of the unique identifier field in your data class |
| `ParentID` | Name of the parent reference field |
| `DataSource` | The collection of data objects |
| `Root` | Optional: specify the ID of the root node (when multiple root candidates exist) |

---

## Hierarchical Data Binding

For multi-root or complex hierarchies:

```razor
<DataSourceSettings ID="Id" ParentID="ParentId" DataSource="@data" Root="root1" />
```

Set `Root` to the ID of the node you want as the top-level root when the layout has multiple nodes with empty `ParentId`.

---

## Accessing Custom Data in NodeCreating

The `NodeCreating` callback receives the raw data object — cast it to access your model's fields:

```csharp
private void OnNodeCreating(IDiagramObject obj)
{
    var node = obj as Node;
    node.Width = 120;
    node.Height = 50;

    // Access your data model
    if (node.Data is OrgData data)
    {
        // Apply styling based on data
        node.Style = new ShapeStyle
        {
            Fill = data.Role == "Director" ? "#E74C3C" :
                   data.Role == "Manager"  ? "#3498DB" : "#2ECC71",
            StrokeColor = "white"
        };

        // The annotation content is usually auto-set from data,
        // but you can customize it:
        if (node.Annotations.Count > 0)
            node.Annotations[0].Content = $"{data.Name}\n({data.Role})";
    }
}
```

---

## Remote Data Binding

Fetch data from an API and bind after loading:

```razor
<SfDiagramComponent @ref="_diagram" Height="600px"
                    NodeCreating="OnNodeCreating"
                    ConnectorCreating="OnConnectorCreating">
    <DataSourceSettings ID="Id" ParentID="ParentId" DataSource="@data" />
    <Layout Type="LayoutType.OrganizationalChart"
            HorizontalSpacing="40" VerticalSpacing="40" />
</SfDiagramComponent>

@code {
    List<OrgData> data = new();

    protected override async Task OnInitializedAsync()
    {
        // Fetch from API
        data = await Http.GetFromJsonAsync<List<OrgData>>("api/org-chart");
    }

    private void OnNodeCreating(IDiagramObject obj)
    {
        var node = obj as Node;
        node.Width = 120; node.Height = 50;
        node.Style = new ShapeStyle { Fill = "#6BA5D7", StrokeColor = "white" };
    }

    private void OnConnectorCreating(IDiagramObject obj)
    {
        (obj as Connector).Type = ConnectorSegmentType.Orthogonal;
    }
}
```

---

## Common Gotchas

- **Root node must have an empty `ParentId`** (empty string `""`, not `null`) — otherwise it won't be recognized as root
- **`ID` and `ParentID` in `DataSourceSettings` are field name strings** — they must exactly match your model's property names (case-sensitive)
- **`NodeCreating` is required** — without it, the auto-generated nodes have no size and won't be visible
- **`DataSource` + `Layout` go together** — data binding without a layout type results in all nodes stacked at (0, 0)
- **`node.Data`** in `NodeCreating` holds your original data object — cast it to your model type to access custom fields
- **Do NOT set `Nodes`/`Connectors` collections when using data binding** — the diagram generates them automatically from the data source
