# Node Selection in TreeView

## Table of Contents

1. [Single Node Selection](#single-node-selection)
2. [Multiple Node Selection](#multiple-node-selection)
3. [Selection Events](#selection-events)
4. [Accessing Selected Node Data](#accessing-selected-node-data)
5. [Programmatic Selection](#programmatic-selection)
6. [Clear Selection](#clear-selection)
7. [Selection Properties Reference](#selection-properties-reference)
8. [SelectedNodes vs Expanded Nodes](#selectednodes-vs-expanded-nodes)
9. [Common Selection Patterns](#common-selection-patterns)
10. [Best Practices](#best-practices)

---

## Single Node Selection

By default, the TreeView allows selection of a single node at a time. Click any node to select it.

### Basic Selection Example

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="MailItem">
    <TreeViewFieldsSettings TValue="MailItem" 
        Id="Id" 
        Text="FolderName" 
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
    <TreeViewEvents TValue="MailItem" NodeSelected="OnNodeSelected"></TreeViewEvents>
</SfTreeView>

@code {
    public class MailItem
    {
        public string? Id { get; set; }
        public string? FolderName { get; set; }
    }

    string selectedNodeId;
    string selectedNodeText;

    void OnNodeSelected(NodeSelectEventArgs args)
    {
        selectedNodeId = args.NodeData.Id;
        selectedNodeText = args.NodeData.Text;
        Console.WriteLine($"Selected: {selectedNodeText}");
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox" },
        new MailItem { Id = "2", FolderName = "Sent" }
    };
}
```

## Multiple Node Selection

Enable multiple selection by setting `AllowMultiSelection="true"`:

```csharp
<SfTreeView TValue="MailItem" AllowMultiSelection="true">
    <TreeViewFieldsSettings TValue="MailItem" 
        Id="Id" 
        Text="FolderName" 
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
</SfTreeView>
```

Users can select multiple nodes using:
- **Ctrl+Click** - Toggle individual node selection
- **Shift+Click** - Select range of nodes
- **Ctrl+A** - Select all nodes

### Accessing Multiple Selected Nodes

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="MailItem" 
    AllowMultiSelection="true"
    @bind-SelectedNodes="@SelectedNodeIds">
    <TreeViewFieldsSettings TValue="MailItem" DataSource="@MyFolder" />
    <TreeViewEvents TValue="MailItem" NodeSelected="OnSelectionChanged"></TreeViewEvents>
</SfTreeView>

<div>
    <h3>Selected Nodes:</h3>
    @foreach (var nodeId in SelectedNodeIds)
    {
        <p>@nodeId</p>
    }
</div>

@code {
    string[] SelectedNodeIds = Array.Empty<string>();

    void OnSelectionChanged(NodeSelectEventArgs args)
    {
        // SelectedNodeIds automatically updates via binding
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox" },
        new MailItem { Id = "2", FolderName = "Sent" },
        new MailItem { Id = "3", FolderName = "Drafts" }
    };
}
```

## Selection Events

### NodeSelected Event

Fired when a node is selected (left-click):

```csharp
void OnNodeSelected(NodeSelectEventArgs args)
{
    var nodeId = args.NodeData.Id;
    var nodeText = args.NodeData.Text;
    var nodeLevel = args.NodeData.Level;  // Hierarchy level
    
    // Perform action based on selection
    FetchDetailedData(nodeId);
}
```

### NodeClicked Event

Fired when a node is clicked (regardless of selection):

```csharp
void OnNodeClicked(NodeClickEventArgs args)
{
    var nodeId = args.NodeData.Id;
    Console.WriteLine($"Clicked: {nodeId}");
}

<TreeViewEvents TValue="MailItem" 
    NodeClicked="OnNodeClicked"
    NodeSelected="OnNodeSelected">
</TreeViewEvents>
```

### Prevent Selection

Cancel selection before it happens:

```csharp
void OnNodeSelecting(NodeSelectEventArgs args)
{
    // Set Cancel = true to prevent selection
    if (args.NodeData.Id == "restricted-node")
    {
        args.Cancel = true;  // Prevent selection
        return;
    }
}

// Note: NodeSelecting fires before selection and allows cancellation
```

## Accessing Selected Node Data

### Get Selected Node Properties

```csharp
void OnNodeSelected(NodeSelectEventArgs args)
{
    // Access node data
    var node = args.NodeData;
    
    Console.WriteLine($"ID: {node.Id}");
    Console.WriteLine($"Text: {node.Text}");
    Console.WriteLine($"Level: {node.Level}");
    Console.WriteLine($"Selected: {node.Selected}");
    Console.WriteLine($"Expanded: {node.Expanded}");
    
    // Perform operations with selected node
    LoadNodeDetails(node.Id);
}
```

### NodeData Properties

| Property | Type | Description |
|----------|------|-------------|
| `Id` | string | Unique identifier |
| `Text` | string | Display text |
| `Level` | int | Hierarchy level (0 = root) |
| `Selected` | bool | Is node selected |
| `Expanded` | bool | Is node expanded |
| `Child` | List | Child nodes (hierarchical) |
| `IsChecked` | bool? | Checkbox state |
| `Parent` | TreeViewNodeData | Parent node reference |

## Programmatic Selection

### Set Selected Nodes via Binding

```csharp
@using Syncfusion.Blazor.Navigations

<button @onclick="SelectNodes">Select Inbox & Sent</button>

<SfTreeView TValue="MailItem" 
    AllowMultiSelection="true"
    @bind-SelectedNodes="@SelectedNodeIds">
    <TreeViewFieldsSettings TValue="MailItem" DataSource="@MyFolder" />
</SfTreeView>

@code {
    string[] SelectedNodeIds = Array.Empty<string>();

    void SelectNodes()
    {
        // Programmatically select nodes
        SelectedNodeIds = new string[] { "1", "2" };
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox" },
        new MailItem { Id = "2", FolderName = "Sent" },
        new MailItem { Id = "3", FolderName = "Drafts" }
    };
}
```

### Get Selected Nodes via Reference

```csharp
<button @onclick="GetSelectedNodes">Get Selected</button>

<SfTreeView TValue="MailItem" @ref="treeViewRef">
    <TreeViewFieldsSettings TValue="MailItem" DataSource="@MyFolder" />
</SfTreeView>

@code {
    SfTreeView<MailItem> treeViewRef;

    async Task GetSelectedNodes()
    {
        var selectedIds = treeViewRef.SelectedNodes;
        Console.WriteLine($"Selected: {string.Join(",", selectedIds)}");
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox" },
        new MailItem { Id = "2", FolderName = "Sent" }
    };
}
```

## Clear Selection

### Clear All Selected Nodes

```csharp
<button @onclick="ClearSelection">Clear Selection</button>

@code {
    async Task ClearSelection()
    {
        // Clear selected nodes
        SelectedNodeIds = Array.Empty<string>();
        StateHasChanged();
    }
}
```

### Clear Specific Node Selection

```csharp
void DeselectNode(string nodeId)
{
    SelectedNodeIds = SelectedNodeIds
        .Where(id => id != nodeId)
        .ToArray();
}
```

## Common Patterns

### Pattern 1: Selection with Detail Panel

```csharp
<div style="display: flex; gap: 20px;">
    <div style="flex: 1;">
        <SfTreeView TValue="MailItem" AllowMultiSelection="true">
            <TreeViewFieldsSettings TValue="MailItem" DataSource="@MyFolder" />
            <TreeViewEvents TValue="MailItem" NodeSelected="ShowDetails"></TreeViewEvents>
        </SfTreeView>
    </div>
    
    <div style="flex: 1;">
        @if (SelectedNodeData != null)
        {
            <h3>@SelectedNodeData.FolderName</h3>
            <p>ID: @SelectedNodeData.Id</p>
        }
    </div>
</div>

@code {
    MailItem? SelectedNodeData;

    void ShowDetails(NodeSelectEventArgs args)
    {
        SelectedNodeData = args.NodeData;
    }
}
```

### Pattern 2: Selection with Actions

```csharp
<SfTreeView TValue="MailItem" @ref="treeRef">
    <TreeViewFieldsSettings TValue="MailItem" DataSource="@MyFolder" />
    <TreeViewEvents TValue="MailItem" NodeSelected="HandleSelection"></TreeViewEvents>
</SfTreeView>

<div>
    <button @onclick="DeleteSelected" disabled="@(SelectedNodeIds.Length == 0)">
        Delete (@SelectedNodeIds.Length)
    </button>
    <button @onclick="CopySelected" disabled="@(SelectedNodeIds.Length == 0)">
        Copy (@SelectedNodeIds.Length)
    </button>
</div>

@code {
    SfTreeView<MailItem> treeRef;
    string[] SelectedNodeIds = Array.Empty<string>();

    void HandleSelection(NodeSelectEventArgs args)
    {
        // Update selection
    }

    async Task DeleteSelected()
    {
        foreach (var id in SelectedNodeIds)
        {
            await DeleteNode(id);
        }
        SelectedNodeIds = Array.Empty<string>();
    }

    async Task CopySelected()
    {
        // Implement copy logic
    }
}
```

### Pattern 3: Conditional Selection

```csharp
void OnNodeSelected(NodeSelectEventArgs args)
{
    var level = args.NodeData.Level;
    
    // Allow selection only for leaf nodes
    if (level < 2)
    {
        // This is a parent node - deselect it
        SelectedNodeIds = Array.Empty<string>();
        return;
    }
    
    // This is a child node - allow selection
    SelectedNodeData = args.NodeData;
}
```

## Selection Properties Reference

### SelectedNodes Property

Two-way bindable array of selected node IDs:

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="MailItem" @bind-SelectedNodes="@SelectedNodeIds">
    <TreeViewFieldsSettings TValue="MailItem" 
        Id="Id" 
        Text="FolderName" 
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
    <TreeViewEvents TValue="MailItem" NodeSelected="OnNodeSelected"></TreeViewEvents>
</SfTreeView>

<div>
    <h3>Selected Node IDs:</h3>
    @foreach (var id in SelectedNodeIds)
    {
        <span>@id</span>
    }
</div>

@code {
    string[] SelectedNodeIds = Array.Empty<string>();

    void OnNodeSelected(NodeSelectEventArgs args)
    {
        Console.WriteLine($"Selected: {string.Join(", ", SelectedNodeIds)}");
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox" },
        new MailItem { Id = "2", FolderName = "Sent" }
    };
}
```

| Property | Type | Supports Binding | Purpose |
|----------|------|------------------|---------|
| `SelectedNodes` | string[] | Yes (@bind-SelectedNodes) | Array of selected node IDs |

**Two-Way Binding**:
- Automatically updates when user clicks nodes
- Can be set programmatically to select nodes
- Changes trigger component re-render

---

### AllowMultiSelection Property

```csharp
// Single selection (default)
<SfTreeView TValue="MailItem">
    <TreeViewFieldsSettings TValue="MailItem" DataSource="@MyFolder" />
</SfTreeView>

// Multiple selection
<SfTreeView TValue="MailItem" AllowMultiSelection="true">
    <TreeViewFieldsSettings TValue="MailItem" DataSource="@MyFolder" />
</SfTreeView>
```

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `AllowMultiSelection` | bool | false | Enable/disable multiple node selection |

**When AllowMultiSelection="true"**:
- Users can select multiple nodes using Ctrl+Click
- Users can select range using Shift+Click
- Users can select all with Ctrl+A
- SelectedNodes array contains all selected IDs

---

### SelectedNodesChanged Event

Callback when selected nodes change:

```csharp
<SfTreeView TValue="MailItem" @bind-SelectedNodes="@SelectedNodeIds">
    <TreeViewFieldsSettings TValue="MailItem" DataSource="@MyFolder" />
    <TreeViewEvents TValue="MailItem" SelectedNodesChanged="OnSelectionChanged"></TreeViewEvents>
</SfTreeView>

@code {
    string[] SelectedNodeIds = Array.Empty<string>();

    void OnSelectionChanged(SelectedNodesChangedEventArgs args)
    {
        Console.WriteLine($"Selection changed to: {string.Join(", ", args.NewValue)}");
        
        // args.NewValue = newly selected node IDs
        // args.OldValue = previously selected node IDs
    }
}
```

---

## Selection Patterns

### Pattern 1: Programmatic Selection

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="MailItem" @bind-SelectedNodes="@SelectedNodeIds">
    <TreeViewFieldsSettings TValue="MailItem" DataSource="@MyFolder" />
</SfTreeView>

<button @onclick="SelectFirstNode">Select First</button>
<button @onclick="SelectAllNodes">Select All</button>
<button @onclick="ClearSelection">Clear</button>

@code {
    string[] SelectedNodeIds = Array.Empty<string>();
    List<MailItem> MyFolder = new();

    void SelectFirstNode()
    {
        if (MyFolder.Any())
        {
            SelectedNodeIds = new string[] { MyFolder[0].Id };
        }
    }

    void SelectAllNodes()
    {
        SelectedNodeIds = MyFolder
            .Select(x => x.Id)
            .ToArray();
    }

    void ClearSelection()
    {
        SelectedNodeIds = Array.Empty<string>();
    }
}
```

### Pattern 2: Selection Validation

```csharp
void OnNodeSelecting(NodeSelectEventArgs args)
{
    var node = args.NodeData;
    
    // Allow selection only for leaf nodes
    if (node.HasChildren ?? false)
    {
        args.Cancel = true;  // Prevent parent selection
        return;
    }
    
    // Validate user permission
    if (!CanUserSelectNode(node.Id))
    {
        args.Cancel = true;
        ShowErrorMessage("You don't have permission to select this node");
        return;
    }
    
    // Allow selection
}

bool CanUserSelectNode(string nodeId)
{
    // Check against user permissions
    return true; // or false
}
```

### Pattern 3: Get Selection Data

```csharp
async Task GetSelectedNodeDetails()
{
    var selectedIds = SelectedNodeIds;
    var details = new List<string>();
    
    foreach (var id in selectedIds)
    {
        // Query parent data to find full node info
        var node = FindNodeById(id);
        if (node != null)
        {
            details.Add($"{node.FolderName} (ID: {node.Id})");
        }
    }
    
    Console.WriteLine($"Selected nodes: {string.Join(", ", details)}");
}

MailItem FindNodeById(string nodeId)
{
    // Recursive search through hierarchical data
    return SearchNode(MyFolder, nodeId);
}

MailItem SearchNode(List<MailItem> items, string nodeId)
{
    foreach (var item in items)
    {
        if (item.Id == nodeId)
            return item;
        
        if (item.SubFolders != null)
        {
            var found = SearchNode(item.SubFolders, nodeId);
            if (found != null)
                return found;
        }
    }
    
    return null;
}
```

### Pattern 4: Conditional Selection Limits

```csharp
string[] SelectedNodeIds = Array.Empty<string>();
const int MaxSelectableNodes = 5;

void OnNodeSelecting(NodeSelectEventArgs args)
{
    // Limit maximum selections
    if (AllowMultiSelection && SelectedNodeIds.Length >= MaxSelectableNodes)
    {
        if (!SelectedNodeIds.Contains(args.NodeData.Id))
        {
            args.Cancel = true;
            ShowWarning($"Cannot select more than {MaxSelectableNodes} nodes");
            return;
        }
    }
}

void OnNodeSelected(NodeSelectEventArgs args)
{
    if (SelectedNodeIds.Length > MaxSelectableNodes)
    {
        ShowInfo($"Selected {SelectedNodeIds.Length} nodes");
    }
}
```

### Pattern 5: Select Parent on Child Click

```csharp
void OnNodeSelected(NodeSelectEventArgs args)
{
    var node = args.NodeData;
    var newSelection = new List<string>();
    
    // When child is selected, also select parent
    if (node.Level > 0)
    {
        // Add current node
        newSelection.Add(node.Id);
        
        // Find and add parent
        var parent = FindParentNode(node);
        if (parent != null)
        {
            newSelection.Add(parent.Id);
        }
    }
    else
    {
        newSelection.Add(node.Id);
    }
    
    SelectedNodeIds = newSelection.ToArray();
}

MailItem FindParentNode(MailItem node)
{
    // Logic to find parent based on hierarchy
    return null;
}
```

### Pattern 6: Selection with Validation Feedback

```csharp
string SelectionStatus = "";

void OnNodeSelected(NodeSelectEventArgs args)
{
    var count = SelectedNodeIds.Length;
    
    SelectionStatus = count switch
    {
        0 => "No nodes selected",
        1 => $"1 node selected: {args.NodeData.Text}",
        _ => $"{count} nodes selected"
    };
}

void OnNodeSelecting(NodeSelectEventArgs args)
{
    if (!IsSelectable(args.NodeData))
    {
        args.Cancel = true;
        SelectionStatus = $"Cannot select '{args.NodeData.Text}' - restricted";
    }
}

bool IsSelectable(TreeViewNodeData node)
{
    // Custom logic
    return !node.Id?.StartsWith("restricted") ?? true;
}
```

---

## ExpandedNodes vs SelectedNodes

### Key Differences

| Property | Purpose | Example |
|----------|---------|---------|
| `SelectedNodes` | Currently selected/highlighted node | User clicks node to select it |
| `ExpandedNodes` | Expanded/collapsed state | User clicks expand arrow |

### Using Both Together

```csharp
<SfTreeView TValue="Employee"
    AllowMultiSelection="true"
    @bind-SelectedNodes="@SelectedNodes"
    @bind-ExpandedNodes="@ExpandedNodes">
    <TreeViewFieldsSettings TValue="Employee" DataSource="@Employees" />
</SfTreeView>

@code {
    string[] SelectedNodes = Array.Empty<string>();
    string[] ExpandedNodes = new string[] { "dept-1" };

    // A node can be:
    // - Selected AND Expanded
    // - Selected but NOT Expanded
    // - Expanded but NOT Selected
    // - Neither
}
```

---

## Troubleshooting

**Issue: Selection not updating**
- Ensure `@bind-SelectedNodes` is used for two-way binding
- Call `StateHasChanged()` if updating programmatically
- Verify TreeView reference is valid

**Issue: Multiple selection not working**
- Confirm `AllowMultiSelection="true"` is set
- Check browser console for errors
- Test Ctrl+Click and Shift+Click

**Issue: Selected nodes losing state**
- Don't modify the array reference directly
- Use `StateHasChanged()` after updates
- Consider using reactive patterns for state management

## Next Steps

- Enable [checkbox-features.md](checkbox-features.md) for checkbox-based selection
- Implement [events-handling.md](events-handling.md) for advanced event handling
- Use [navigation-patterns.md](navigation-patterns.md) for parent-child navigation
