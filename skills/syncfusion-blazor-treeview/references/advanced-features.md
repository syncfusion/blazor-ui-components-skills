# Advanced Features in TreeView

## Table of Contents
- [Drag and Drop](#drag-and-drop)
- [Virtualization](#virtualization)
- [Search and Filter](#search-and-filter)
- [Sorting](#sorting)
- [Integration](#integration)
- [Performance Optimization](#performance-optimization)

## Drag and Drop

### Enable Drag and Drop

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="EmployeeData" AllowDragAndDrop="true">
    <TreeViewFieldsSettings TValue="EmployeeData"
                            Id="Id"
                            Text="Name"
                            Child="Children"
                            DataSource="@Team">
    </TreeViewFieldsSettings>
    <TreeViewEvents TValue="EmployeeData"
                    OnNodeDragStart="OnDragStart"
                    NodeDropped="OnNodeDropped">
    </TreeViewEvents>
</SfTreeView>

@code {
    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public List<EmployeeData>? Children { get; set; }
    }

    List<EmployeeData> Team = new()
    {
        new EmployeeData
        {
            Id = "team1",
            Name = "Development",
            Children = new()
            {
                new EmployeeData { Id = "dev1", Name = "Alice" },
                new EmployeeData { Id = "dev2", Name = "Bob" }
            }
        }
    };

    void OnDragStart(DragAndDropEventArgs args)
    {
        Console.WriteLine($"Dragging: {args.DraggedNodeData.Name}");
    }

    void OnNodeDropped(DragAndDropEventArgs args)
    {
        var draggedNodeId = args.DraggedNodeData.Id;
        var dropIndex = args.DropIndex;

        Console.WriteLine($"Dropped node {draggedNodeId} at index {dropIndex}");

        // Update hierarchy
        UpdateNodePosition(draggedNodeId, dropIndex);
    }

    void UpdateNodePosition(string draggedNodeId, int dropIndex)
    {
        // Implementation for updating node hierarchy
    }
}
```

### Drag Drop Indicators

The TreeView shows visual indicators during drag-drop:
- **Plus (+) icon**: Drop as child node
- **Minus (-) icon**: Cannot drop at this location
- **Line indicator**: Drop as sibling node

### Restrict Dragging

```csharp
void OnNodeDragStart(DragAndDropEventArgs args)
{
    // Prevent dragging system nodes
    if (args.DraggedNodeData.Id.StartsWith("system_"))
    {
        args.Cancel = true;
        return;
    }
}
```

## Virtualization

### Enable UI Virtualization

Virtualization renders only visible nodes, improving performance with large datasets:

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="TreeData" 
    EnableVirtualization="true" 
    Height="400">
    <TreeViewFieldsSettings TValue="TreeData"
        Id="Id"
        Text="Name"
        ParentID="Pid"
        HasChildren="HasChild"
        DataSource="@TreeDataSource">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    public class TreeData
    {
        public string? Id { get; set; }
        public string? Pid { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
    }

    List<TreeData> TreeDataSource = new();

    protected override void OnInitialized()
    {
        // Generate large dataset
        for (int i = 1; i <= 1000; i++)
        {
            TreeDataSource.Add(new TreeData
            {
                Id = i.ToString(),
                Name = $"Node {i}",
                HasChild = i % 10 == 0
            });
        }
    }
}
```

**Virtualization Requirements:**
- Set `EnableVirtualization="true"`
- Set fixed `Height` (in pixels)
- Use with `LoadOnDemand="true"` for optimal performance

**Benefits:**
- Smooth scrolling with thousands of items
- Reduced memory usage
- Faster initial render

**Limitations:**
- Cannot use expand/collapse animation
- Select All selects only visible items

## Search and Filter

### Implement Search Functionality

```csharp
@using Syncfusion.Blazor.Navigations

<input type="text" 
    @bind="SearchText" 
    @bind:event="oninput"
    placeholder="Search nodes..."
    @onkeyup="HandleSearch" />

<SfTreeView TValue="MailItem">
    <TreeViewFieldsSettings TValue="MailItem"
        Id="Id"
        Text="FolderName"
        ParentID="ParentId"
        HasChildren="HasChildren"
        DataSource="@FilteredFolders">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    string SearchText = "";
    
    List<MailItem> FilteredFolders => GetFilteredFolders();
    
    List<MailItem> GetFilteredFolders()
    {
        if (string.IsNullOrWhiteSpace(SearchText))
            return MyFolder;
        
        var searchLower = SearchText.ToLower();
        var matched = new HashSet<string>();
        
        // Find matching nodes
        foreach (var folder in MyFolder)
        {
            if (folder.FolderName?.ToLower().Contains(searchLower) ?? false)
            {
                matched.Add(folder.Id);
                
                // Include all parents
                var current = folder;
                while (current?.ParentId != null)
                {
                    matched.Add(current.ParentId);
                    current = MyFolder.FirstOrDefault(x => x.Id == current.ParentId);
                }
            }
        }
        
        return MyFolder.Where(x => matched.Contains(x.Id)).ToList();
    }
    
    void HandleSearch()
    {
        // Trigger search on input
        StateHasChanged();
    }

    public class MailItem
    {
        public string? Id { get; set; }
        public string? ParentId { get; set; }
        public string? FolderName { get; set; }
        public bool HasChildren{ get; set; }
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Documents", HasChildren=true },
        new MailItem { Id = "1-1", ParentId = "1", FolderName = "Work" },
        new MailItem { Id = "1-2", ParentId = "1", FolderName = "Personal" }
    };
}
```

### Highlight Search Results

```csharp
@using System.Text.RegularExpressions

<TreeViewTemplates TValue="MailItem">
    <NodeTemplate>
        <span>@Html.Raw(HighlightSearchText((context as MailItem).FolderName))</span>
    </NodeTemplate>
</TreeViewTemplates>

@code {
    string HighlightSearchText(string text)
    {
        if (string.IsNullOrWhiteSpace(SearchText) || string.IsNullOrWhiteSpace(text))
            return text;
        
        var pattern = $"({Regex.Escape(SearchText)})";
        var highlighted = Regex.Replace(
            text,
            pattern,
            "<mark style='background-color: yellow;'>$1</mark>",
            RegexOptions.IgnoreCase
        );
        
        return highlighted;
    }
}
```

## Sorting

### Sort Tree Nodes

```csharp
@using System.Linq

<button @onclick="SortAscending">Sort A-Z</button>
<button @onclick="SortDescending">Sort Z-A</button>

<SfTreeView TValue="FileItem">
    <TreeViewFieldsSettings TValue="FileItem"
        Id="Id"
        Text="FileName"
        ParentID="ParentId"
        DataSource="@SortedFiles">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    public class FileItem
    {
        public string? Id { get; set; }
        public string? ParentId { get; set; }
        public string? FileName { get; set; }
    }

    List<FileItem> AllFiles = new()
    {
        new FileItem { Id = "1", FileName = "Zebra" },
        new FileItem { Id = "2", FileName = "Apple" },
        new FileItem { Id = "3", FileName = "Monkey" }
    };

    List<FileItem> SortedFiles => AllFiles;

    void SortAscending()
    {
        AllFiles.Sort((a, b) => a.FileName?.CompareTo(b.FileName) ?? 0);
        StateHasChanged();
    }

    void SortDescending()
    {
        AllFiles.Sort((a, b) => b.FileName?.CompareTo(a.FileName) ?? 0);
        StateHasChanged();
    }
}
```

### Natural Sort (alphanumeric)

```csharp
void SortNatural()
{
    AllFiles.Sort((a, b) => 
        NaturalSort(a.FileName, b.FileName)
    );
    StateHasChanged();
}

int NaturalSort(string? a, string? b)
{
    if (a == b) return 0;
    if (a == null) return -1;
    if (b == null) return 1;

    var aChars = a.ToCharArray();
    var bChars = b.ToCharArray();

    int aIndex = 0, bIndex = 0;

    while (aIndex < aChars.Length && bIndex < bChars.Length)
    {
        if (char.IsDigit(aChars[aIndex]) && char.IsDigit(bChars[bIndex]))
        {
            // Extract numbers
            long aNum = 0, bNum = 0;
            while (aIndex < aChars.Length && char.IsDigit(aChars[aIndex]))
                aNum = aNum * 10 + (aChars[aIndex++] - '0');
            while (bIndex < bChars.Length && char.IsDigit(bChars[bIndex]))
                bNum = bNum * 10 + (bChars[bIndex++] - '0');

            if (aNum != bNum)
                return aNum.CompareTo(bNum);
        }
        else
        {
            if (aChars[aIndex] != bChars[bIndex])
                return aChars[aIndex].CompareTo(bChars[bIndex]);
            aIndex++;
            bIndex++;
        }
    }

    return aChars.Length.CompareTo(bChars.Length);
}
```

## Integration

### Context Menu Integration

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="EmployeeData" @ref="tree">
    <TreeViewFieldsSettings TValue="EmployeeData" DataSource="@ListData" />
    <TreeViewEvents TValue="EmployeeData" NodeClicked="nodeClicked"></TreeViewEvents>
    
    <SfContextMenu TValue="MenuItem" Target="#treeview" Items="@MenuItems">
        <MenuEvents TValue="MenuItem" ItemSelected="OnMenuSelect"></MenuEvents>
    </SfContextMenu>
</SfTreeView>

@code {
    SfTreeView<EmployeeData> tree;
    string selectedNodeId;

    public List<MenuItem> MenuItems = new()
    {
        new MenuItem { Text = "Copy" },
        new MenuItem { Text = "Rename" },
        new MenuItem { Text = "Delete" }
    };

    void nodeClicked(NodeClickEventArgs args)
    {
        selectedNodeId = args.NodeData.Id;
    }

    void OnMenuSelect(MenuEventArgs args)
    {
        switch (args.Item.Text)
        {
            case "Copy":
                CopyNode(selectedNodeId);
                break;
            case "Rename":
                RenameNode(selectedNodeId);
                break;
            case "Delete":
                DeleteNode(selectedNodeId);
                break;
        }
    }
}
```

### Badge Integration

```csharp
<TreeViewTemplates TValue="TaskItem">
    <NodeTemplate>
        <div style="display: flex; justify-content: space-between; align-items: center;">
            <span>@((context as TaskItem).Name)</span>
            @if ((context as TaskItem).Count > 0)
            {
                <span style="background: #ff6b6b; color: white; border-radius: 12px; padding: 2px 8px; font-size: 12px;">
                    @((context as TaskItem).Count)
                </span>
            }
        </div>
    </NodeTemplate>
</TreeViewTemplates>

@code {
    public class TaskItem
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public int Count { get; set; }
    }
}
```

## Performance Optimization

### Best Practices

1. **Enable virtualization** for large datasets (>500 nodes)
2. **Use LoadOnDemand** to load children only when expanded
3. **Cache node lookups** to avoid repeated searches
4. **Implement pagination** for API responses
5. **Debounce search** to avoid excessive filtering
6. **Use shouldRender patterns** to skip unnecessary renders

### Lazy Loading Pattern

```csharp
void OnNodeExpanded(NodeExpandEventArgs args)
{
    if (args.NodeData.HasChild && args.NodeData.Child == null)
    {
        // Only load if not already loaded
        LoadChildNodes(args.NodeData.Id);
    }
}

async Task LoadChildNodes(string parentId)
{
    var children = await FetchFromApi(parentId);
    // Update data source
}
```

### Avoid Performance Pitfalls

```csharp
// ❌ BAD: Recreating entire list on filter
void BadSearch()
{
    MyFolder = MyFolder  // Creates new list every time
        .Where(x => x.Name.Contains(SearchText))
        .ToList();
}

// ✅ GOOD: Use computed property
List<MailItem> FilteredFolders => MyFolder
    .Where(x => x.FolderName?.Contains(SearchText) ?? false)
    .ToList();
```

## Common Patterns

### Pattern 1: Multi-Select with Toolbar

```csharp
<div>
    <button @onclick="DeleteSelected" disabled="@(SelectedNodeIds.Length == 0)">
        Delete @SelectedNodeIds.Length
    </button>
    <button @onclick="ExportSelected" disabled="@(SelectedNodeIds.Length == 0)">
        Export @SelectedNodeIds.Length
    </button>
</div>

<SfTreeView TValue="Item" AllowMultiSelection="true" @bind-SelectedNodes="@SelectedNodeIds">
    <TreeViewFieldsSettings TValue="Item" DataSource="@Items" />
</SfTreeView>

@code {
    string[] SelectedNodeIds = Array.Empty<string>();
    
    async Task DeleteSelected()
    {
        foreach (var id in SelectedNodeIds)
            await DeleteItem(id);
        SelectedNodeIds = Array.Empty<string>();
    }
}
```

## Methods Reference

### Expand/Collapse Methods

#### ExpandAllAsync()

Expands all nodes in the TreeView:

```csharp
@ref SfTreeView<Employee> TreeViewRef;

<SfTreeView TValue="Employee" @ref="TreeViewRef">
    <TreeViewFieldsSettings TValue="Employee" DataSource="@Employees" />
</SfTreeView>

<button @onclick="ExpandAll">Expand All</button>

@code {
    SfTreeView<Employee> TreeViewRef;
    
    async Task ExpandAll()
    {
        await TreeViewRef.ExpandAllAsync();
    }
}
```

#### CollapseAllAsync()

Collapses all nodes in the TreeView:

```csharp
<button @onclick="CollapseAll">Collapse All</button>

@code {
    async Task CollapseAll()
    {
        await TreeViewRef.CollapseAllAsync();
    }
}
```

---

### Data Retrieval Methods

#### GetTreeData()

Retrieves all data from TreeView:

```csharp
async Task GetAllData()
{
    var allData = TreeViewRef.GetTreeData();
    Console.WriteLine($"Total nodes: {allData.Count}");
    
    foreach (var node in allData)
    {
        Console.WriteLine($"ID: {node.Id}, Text: {node.Text}");
    }
}
```

#### GetNode(nodeId)

Retrieves a specific node:

```csharp
async Task GetSpecificNode()
{
    var node = TreeViewRef.GetNode("employee-5");
    if (node != null)
    {
        Console.WriteLine($"Found: {node.Text}");
    }
}
```

#### GetDisabledNodesAsync()

Gets all disabled nodes:

```csharp
async Task GetDisabled()
{
    var disabledNodes = await TreeViewRef.GetDisabledNodesAsync();
    Console.WriteLine($"Disabled nodes: {disabledNodes.Count}");
}
```

---

### Node Modification Methods

#### AddNodes()

Adds new nodes to TreeView:

```csharp
void AddNewNode()
{
    var newNode = new Employee
    {
        Id = "emp-new",
        Name = "New Employee",
        ParentId = "emp-1"
    };
    
    TreeViewRef.AddNodes(newNode);
}

void AddMultipleNodes()
{
    var newNodes = new List<Employee>
    {
        new Employee { Id = "emp-new-1", Name = "Employee 1", ParentId = "emp-1" },
        new Employee { Id = "emp-new-2", Name = "Employee 2", ParentId = "emp-1" }
    };
    
    TreeViewRef.AddNodes(newNodes);
}
```

#### RemoveNodes()

Removes nodes from TreeView:

```csharp
void RemoveNode()
{
    TreeViewRef.RemoveNodes("emp-5");
}

void RemoveMultipleNodes()
{
    var nodeIds = new string[] { "emp-5", "emp-6", "emp-7" };
    TreeViewRef.RemoveNodes(nodeIds);
}
```

#### RefreshNodeAsync()

Updates a specific node's data:

```csharp
async Task UpdateNodeData()
{
    var node = new Employee
    {
        Id = "emp-5",
        Name = "Updated Name",
        ParentId = "emp-1"
    };
    
    await TreeViewRef.RefreshNodeAsync(node);
}
```

---

### Navigation Methods

#### EnsureVisibleAsync()

Scrolls TreeView to make specified node visible:

```csharp
async Task ScrollToNode()
{
    await TreeViewRef.EnsureVisibleAsync("emp-5");
    
    // Optional: also select the node
    SelectedNodes = new string[] { "emp-5" };
}
```

---

### Enable/Disable Methods

#### EnableNodesAsync()

Enables disabled nodes:

```csharp
async Task EnableNodes()
{
    var nodeIds = new string[] { "emp-5", "emp-6" };
    await TreeViewRef.EnableNodesAsync(nodeIds);
}
```

#### DisableNodesAsync()

Disables nodes (prevents interaction):

```csharp
async Task DisableNodes()
{
    var nodeIds = new string[] { "emp-5", "emp-6" };
    await TreeViewRef.DisableNodesAsync(nodeIds);
}
```

---

### Checkbox Methods

#### CheckAllAsync()

Checks all nodes (if checkboxes enabled):

```csharp
async Task CheckAll()
{
    await TreeViewRef.CheckAllAsync();
}
```

#### UncheckAllAsync()

Unchecks all nodes:

```csharp
async Task UncheckAll()
{
    await TreeViewRef.UncheckAllAsync();
}
```

#### GetAllCheckedNodes()

Gets all checked node IDs:

```csharp
void GetChecked()
{
    var checkedNodeIds = TreeViewRef.GetAllCheckedNodes();
    Console.WriteLine($"Checked nodes: {string.Join(", ", checkedNodeIds)}");
}
```

---

### State Management Methods

#### ClearStateAsync()

Clears all state (selected, expanded, checked nodes):

```csharp
async Task ClearState()
{
    await TreeViewRef.ClearStateAsync();
    
    // After this:
    // - No nodes are selected
    // - No nodes are expanded
    // - No nodes are checked
    // - Scroll position reset
}
```

---

## Advanced Property Reference

### EnableVirtualization

```csharp
<SfTreeView TValue="DataItem"
    EnableVirtualization="true"
    Height="500px"
    LoadOnDemand="true">
    <TreeViewFieldsSettings TValue="DataItem" DataSource="@LargeDataset" />
</SfTreeView>

// Benefits:
// - Renders only visible nodes
// - Handles 100K+ nodes smoothly
// - Reduces memory footprint
// - Requires Height to be set
```

### SortOrder and SortComparer

```csharp
// Ascending sort
<SfTreeView TValue="Employee" SortOrder="SortOrder.Ascending">
    <TreeViewFieldsSettings TValue="Employee" DataSource="@Employees" />
</SfTreeView>

// Custom sort with IComparer
public class EmployeeComparer : IComparer<object>
{
    public int Compare(object x, object y)
    {
        var emp1 = x as Employee;
        var emp2 = y as Employee;
        
        // Sort by Name length first, then alphabetically
        int result = emp1.Name.Length.CompareTo(emp2.Name.Length);
        if (result == 0)
            result = emp1.Name.CompareTo(emp2.Name);
        
        return result;
    }
}

<SfTreeView TValue="Employee" SortComparer="@new EmployeeComparer()">
    <TreeViewFieldsSettings TValue="Employee" DataSource="@Employees" />
</SfTreeView>
```

### DropArea

```csharp
// Allow drag-drop only within tree container
<div class="tree-container">
    <SfTreeView TValue="Employee" 
        AllowDragAndDrop="true" 
        DropArea=".tree-container">
        <TreeViewFieldsSettings TValue="Employee" DataSource="@Employees" />
    </SfTreeView>
</div>

// Or specify by ID
<SfTreeView TValue="Employee" 
    AllowDragAndDrop="true" 
    DropArea="#dropzone">
    <TreeViewFieldsSettings TValue="Employee" DataSource="@Employees" />
</SfTreeView>

<div id="dropzone"></div>
```

### FullRowNavigable & FullRowSelect

```csharp
// Allow selection anywhere on node row (not just text)
<SfTreeView TValue="Employee" FullRowSelect="true">
    <TreeViewFieldsSettings TValue="Employee" DataSource="@Employees" />
</SfTreeView>

// Allow navigation anywhere on node row
<SfTreeView TValue="Employee" FullRowNavigable="true">
    <TreeViewFieldsSettings TValue="Employee" DataSource="@Employees" />
</SfTreeView>
```

---

## Common Patterns with Methods

### Pattern 1: Bulk Operations

```csharp
async Task BulkUpdateNodes()
{
    // Get all checked nodes
    var checkedIds = TreeViewRef.GetAllCheckedNodes();
    
    // Update each node
    foreach (var id in checkedIds)
    {
        var node = TreeViewRef.GetNode(id);
        if (node != null)
        {
            node.Name = $"Updated: {node.Name}";
            await TreeViewRef.RefreshNodeAsync(node);
        }
    }
    
    // Clear selection
    await TreeViewRef.ClearStateAsync();
}
```

### Pattern 2: Export Selected Nodes

```csharp
async Task ExportSelected()
{
    var selectedNodes = SelectedNodes;
    var exportData = new List<EmployeeExport>();
    
    foreach (var id in selectedNodes)
    {
        var node = TreeViewRef.GetNode(id);
        if (node != null)
        {
            exportData.Add(new EmployeeExport
            {
                Id = node.Id,
                Name = node.Text,
                Level = node.Level
            });
        }
    }
    
    // Export to CSV/JSON
    await ExportToFile(exportData);
}
```

### Pattern 3: Dynamic Tree Building

```csharp
async Task BuildDynamicTree()
{
    // Start with root nodes
    TreeViewRef.AddNodes(rootNodes);
    
    // Expand root
    await TreeViewRef.ExpandAllAsync();
    
    // Load child nodes on demand
    var allNodes = TreeViewRef.GetTreeData();
    var parentIds = allNodes
        .Where(n => n.HasChildren)
        .Select(n => n.Id)
        .ToList();
    
    // Fetch and add children for each parent
    foreach (var parentId in parentIds)
    {
        var children = await FetchChildNodes(parentId);
        TreeViewRef.AddNodes(children);
    }
}
```

---

## Troubleshooting

**Issue: Virtualization causes flickering**
- Increase Height value
- Reduce number of items per render
- Check for heavy computations in template

**Issue: Search too slow**
- Implement debouncing
- Limit results displayed
- Use indexed search if possible

**Issue: Drag-drop not working with virtualization**
- Virtualization and drag-drop have limited compatibility
- Consider disabling one or the other
- Test with smaller datasets first

## Next Steps

- Use [events-handling.md](events-handling.md) for all advanced events
- Check [accessibility.md](accessibility.md) for accessible patterns
- Review [performance.md](performance-optimization) for optimization strategies
