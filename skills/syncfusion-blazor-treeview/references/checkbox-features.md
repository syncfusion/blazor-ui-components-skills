# Checkbox Features in TreeView

## Table of Contents
- [Enable Checkboxes](#enable-checkboxes)
- [AutoCheck Behavior](#autocheck-behavior)
- [Checkbox State Management](#checkbox-state-management)
- [Getting Checked Nodes](#getting-checked-nodes)
- [Common Patterns](#common-patterns)

## Enable Checkboxes

### Basic Checkbox Setup

Display checkboxes before each node by setting `ShowCheckBox="true"`:

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="MailItem" ShowCheckBox="true">
    <TreeViewFieldsSettings TValue="MailItem"
        Id="Id"
        Text="FolderName"
        ParentID="ParentId"
        HasChildren="HasSubFolders"
        IsChecked="IsChecked"
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    public class MailItem
    {
        public string? Id { get; set; }
        public string? ParentId { get; set; }
        public string? FolderName { get; set; }
        public bool HasSubFolders { get; set; }
        public bool? IsChecked { get; set; }  // null = indeterminate
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox", HasSubFolders = true },
        new MailItem { Id = "2", ParentId = "1", FolderName = "Important" }
    };
}
```

### Checkbox Display States

- **Checked** - All child nodes are checked (✓)
- **Unchecked** - No child nodes are checked (☐)
- **Indeterminate** - Some child nodes are checked (◐)

## AutoCheck Behavior

The `AutoCheck` property controls whether parent and child checkboxes affect each other.

### Dependent Checkboxes (Default: AutoCheck="true")

When a parent node is checked, all child nodes automatically become checked. When a parent node is unchecked, all children become unchecked:

```csharp
<SfTreeView TValue="MailItem" ShowCheckBox="true" AutoCheck="true">
    <TreeViewFieldsSettings TValue="MailItem" 
        IsChecked="IsChecked"
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    public class MailItem
    {
        public string? Id { get; set; }
        public string? FolderName { get; set; }
        public bool? IsChecked { get; set; }
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Documents", IsChecked = false },
        new MailItem { Id = "1-1", FolderName = "Work", IsChecked = false },
        new MailItem { Id = "1-2", FolderName = "Personal", IsChecked = false }
    };

    // User checks Documents:
    // → Work becomes checked
    // → Personal becomes checked
    // All related nodes update automatically
}
```

**AutoCheck Behavior:**
- ✓ Parent checked → All children checked
- ☐ Parent unchecked → All children unchecked
- ◐ Parent indeterminate → Some children checked, some unchecked
- Parent updates automatically based on child state changes

### Independent Checkboxes (AutoCheck="false")

Child nodes can be checked/unchecked without affecting parents:

```csharp
<SfTreeView TValue="MailItem" ShowCheckBox="true" AutoCheck="false">
    <TreeViewFieldsSettings TValue="MailItem" 
        IsChecked="IsChecked"
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    // Same data model
    // User checks "Work":
    // → "Documents" state NOT affected
    // → "Personal" state NOT affected
    // Only "Work" checkbox changes
}
```

**Use AutoCheck="false" when:**
- Each node represents an independent choice
- Parent-child relationships don't imply selection dependency
- You want fine-grained user control

## Checkbox State Management

### Initialize Checkbox States

Set initial checkbox states via the `IsChecked` property in your data:

```csharp
List<MailItem> MyFolder = new()
{
    new MailItem { Id = "1", FolderName = "Inbox", IsChecked = true },
    new MailItem { Id = "2", FolderName = "Sent", IsChecked = false },
    new MailItem { Id = "3", FolderName = "Drafts", IsChecked = null }  // Indeterminate
};
```

**IsChecked Values:**
- `true` - Node is checked
- `false` - Node is unchecked
- `null` - Node is indeterminate (partial selection)

### Update Checkbox State Programmatically

```csharp
@using Syncfusion.Blazor.Navigations

<button @onclick="CheckAllNodes">Check All</button>
<button @onclick="UncheckAllNodes">Uncheck All</button>

<SfTreeView TValue="MailItem" ShowCheckBox="true">
    <TreeViewFieldsSettings TValue="MailItem"
        IsChecked="IsChecked"
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox", IsChecked = false },
        new MailItem { Id = "2", FolderName = "Sent", IsChecked = false }
    };

    void CheckAllNodes()
    {
        foreach (var item in MyFolder)
        {
            item.IsChecked = true;
        }
        StateHasChanged();
    }

    void UncheckAllNodes()
    {
        foreach (var item in MyFolder)
        {
            item.IsChecked = false;
        }
        StateHasChanged();
    }
}
```

### Handle Checkbox Change Events

```csharp
<SfTreeView TValue="MailItem" ShowCheckBox="true">
    <TreeViewFieldsSettings TValue="MailItem" IsChecked="IsChecked" DataSource="@MyFolder" />
    <TreeViewEvents TValue="MailItem" NodeSelected="OnCheckboxChange"></TreeViewEvents>
</SfTreeView>

@code {
    void OnCheckboxChange(NodeSelectEventArgs args)
    {
        var isChecked = args.NodeData.IsChecked;
        var nodeId = args.NodeData.Id;
        
        Console.WriteLine($"Node {nodeId} is now {(isChecked ? "checked" : "unchecked")}");
        
        // Perform actions based on checkbox state
        UpdateParentState(nodeId, isChecked);
    }

    void UpdateParentState(string nodeId, bool? isChecked)
    {
        // Custom logic for parent/child synchronization
    }
}
```

### CheckedNodes Property Reference

The `CheckedNodes` property enables two-way binding for managing which nodes are checked. This property accepts an array of node IDs (as strings) and can be updated programmatically:

```csharp
@using Syncfusion.Blazor.Navigations

<button @onclick="CheckSpecificNodes">Check Work & Development</button>
<button @onclick="ClearAllChecks">Clear All</button>
<p>Checked Nodes: @string.Join(", ", CheckedNodeIds)</p>

<SfTreeView TValue="MailItem" 
    ShowCheckBox="true" 
    AutoCheck="true"
    @bind-CheckedNodes="@CheckedNodeIds">
    <TreeViewFieldsSettings TValue="MailItem"
        Id="Id"
        Text="FolderName"
        ParentID="ParentId"
        HasChildren="HasSubFolders"
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    // Two-way bound property for checked nodes
    string[] CheckedNodeIds = Array.Empty<string>();

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Projects", HasSubFolders = true },
        new MailItem { Id = "2", ParentId = "1", FolderName = "Work" },
        new MailItem { Id = "3", ParentId = "1", FolderName = "Development" },
        new MailItem { Id = "4", FolderName = "Archive", HasSubFolders = false }
    };

    void CheckSpecificNodes()
    {
        // Set specific nodes as checked
        CheckedNodeIds = new[] { "2", "3" };
    }

    void ClearAllChecks()
    {
        // Clear all checked nodes
        CheckedNodeIds = Array.Empty<string>();
    }
}
```

**CheckedNodes Features:**
- **Two-way binding** - Changes reflect immediately in UI
- **Programmatic control** - Set checked nodes from code
- **Array of IDs** - Pass multiple node IDs as strings
- **Dynamic updates** - Modify at any time during component lifecycle

### Bulk Operations with CheckedNodes

```csharp
async Task CheckAllNodesAsync()
{
    // Get all node IDs and check them
    var allNodeIds = GetAllNodeIds(MyFolder);
    CheckedNodeIds = allNodeIds.ToArray();
    await InvokeAsync(StateHasChanged);
}

async Task UncheckAllNodesAsync()
{
    CheckedNodeIds = Array.Empty<string>();
    await InvokeAsync(StateHasChanged);
}

List<string> GetAllNodeIds(List<MailItem> items)
{
    var ids = new List<string>();
    foreach (var item in items)
    {
        ids.Add(item.Id);
    }
    return ids;
}
```

## Getting Checked Nodes

### Access Checked Nodes via Reference

```csharp
@using Syncfusion.Blazor.Navigations

<button @onclick="GetCheckedNodes">Get Checked Nodes</button>

<SfTreeView TValue="MailItem" 
    ShowCheckBox="true"
    @ref="treeRef">
    <TreeViewFieldsSettings TValue="MailItem" 
        IsChecked="IsChecked"
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
</SfTreeView>

<div>
    <h3>Checked Items:</h3>
    @foreach (var item in CheckedItems)
    {
        <p>@item.FolderName</p>
    }
</div>

@code {
    SfTreeView<MailItem> treeRef;
    List<MailItem> CheckedItems = new();

    async Task GetCheckedNodes()
    {
        CheckedItems.Clear();
        
        // Get all checked nodes
        var allNodes = FlattenTree(MyFolder);
        foreach (var node in allNodes)
        {
            if (node.IsChecked == true)
            {
                CheckedItems.Add(node);
            }
        }
    }

    List<MailItem> FlattenTree(List<MailItem> items)
    {
        var result = new List<MailItem>();
        foreach (var item in items)
        {
            result.Add(item);
            // Add recursive flattening if hierarchical
        }
        return result;
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox", IsChecked = true },
        new MailItem { Id = "2", FolderName = "Sent", IsChecked = false },
        new MailItem { Id = "3", FolderName = "Drafts", IsChecked = true }
    };
}
```

### Filter Checked Nodes

```csharp
List<MailItem> GetCheckedNodesLinq()
{
    return FlattenTree(MyFolder)
        .Where(x => x.IsChecked == true)
        .ToList();
}

List<string> GetCheckedNodeIds()
{
    return FlattenTree(MyFolder)
        .Where(x => x.IsChecked == true)
        .Select(x => x.Id)
        .ToList();
}
```

## Common Patterns

### Pattern 1: Permissions Selection (Checkboxes + Hierarchy)

```csharp
@using Syncfusion.Blazor.Navigations

<button @onclick="SavePermissions">Save Permissions</button>

<SfTreeView TValue="Permission" ShowCheckBox="true" AutoCheck="true">
    <TreeViewFieldsSettings TValue="Permission"
        Id="Id"
        Text="Name"
        Child="Children"
        IsChecked="Granted"
        DataSource="@Permissions">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    public class Permission
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public bool? Granted { get; set; }
        public List<Permission>? Children { get; set; }
    }

    List<Permission> Permissions = new()
    {
        new Permission
        {
            Id = "admin",
            Name = "Admin",
            Granted = false,
            Children = new()
            {
                new Permission { Id = "users", Name = "Manage Users", Granted = false },
                new Permission { Id = "settings", Name = "Settings", Granted = false }
            }
        }
    };

    async Task SavePermissions()
    {
        var grantedPerms = GetGrantedPermissions();
        // Save to database
        await SaveToDatabase(grantedPerms);
    }

    List<string> GetGrantedPermissions()
    {
        var result = new List<string>();
        
        void Traverse(List<Permission> items)
        {
            foreach (var item in items)
            {
                if (item.Granted == true)
                    result.Add(item.Id);
                if (item.Children != null)
                    Traverse(item.Children);
            }
        }
        
        Traverse(Permissions);
        return result;
    }
}
```

### Pattern 2: Multi-Select with Item Count

```csharp
<div>
    <strong>Selected: @CheckedCount / @TotalCount</strong>
</div>

<SfTreeView TValue="Item" ShowCheckBox="true">
    <TreeViewFieldsSettings TValue="Item"
        IsChecked="Selected"
        DataSource="@Items">
    </TreeViewFieldsSettings>
    <TreeViewEvents TValue="Item" NodeSelected="OnSelectionChange"></TreeViewEvents>
</SfTreeView>

@code {
    int CheckedCount { get; set; }
    int TotalCount { get; set; }

    void OnSelectionChange(NodeSelectEventArgs args)
    {
        CheckedCount = CountChecked(Items);
        TotalCount = Items.Count;
    }

    int CountChecked(List<Item> items)
    {
        return items.Count(x => x.Selected == true);
    }
}
```

### Pattern 3: Category Selection Filter

```csharp
<button @onclick="ApplyFilter">Filter by Selected</button>

<SfTreeView TValue="Category" ShowCheckBox="true">
    <TreeViewFieldsSettings TValue="Category"
        IsChecked="Selected"
        DataSource="@Categories">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    void ApplyFilter()
    {
        var selectedCategories = Categories
            .Where(x => x.Selected == true)
            .Select(x => x.Id)
            .ToList();
        
        // Filter data source based on selected categories
        FilterDataByCategories(selectedCategories);
    }
}
```

## Best Practices

1. **Always map IsChecked** field in TreeViewFieldsSettings
2. **Use AutoCheck appropriately** - Enable for hierarchical selections, disable for independent choices
3. **Initialize IsChecked** in your data model (true/false/null)
4. **Handle checkbox changes** via events for real-time updates
5. **Use two-way binding** for reactive state updates
6. **Test parent-child relationships** to ensure expected behavior
7. **Provide feedback** when checkbox state changes (toasts, logs)

## Troubleshooting

**Issue: Checkboxes not appearing**
- Ensure `ShowCheckBox="true"` is set
- Verify `IsChecked` property is mapped in TreeViewFieldsSettings
- Check CSS theme is loaded

**Issue: AutoCheck not working**
- Confirm `AutoCheck="true"` (default)
- Verify data has parent-child relationships
- Check that ParentID/Child mappings are correct

**Issue: Checked state not persisting**
- Ensure IsChecked property is nullable (bool?)
- Call StateHasChanged() after updates
- Verify data changes trigger re-render

## Next Steps

- Use [node-selection.md](node-selection.md) for regular node selection
- Implement [expand-collapse-actions.md](expand-collapse-actions.md) for node expansion
- Handle [events-handling.md](events-handling.md) for checkbox and selection events
