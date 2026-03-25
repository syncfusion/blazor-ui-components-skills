# Navigation Patterns in TreeView

## Table of Contents

1. [Node Navigation Methods](#node-navigation-methods)
2. [Get Node by ID](#get-node-by-id)
3. [Get Parent Node](#get-parent-node)
4. [Get Child Nodes](#get-child-nodes)
5. [Tree Traversal Patterns](#tree-traversal-patterns)
6. [Breadcrumb Navigation](#breadcrumb-navigation)
7. [NavigateUrl Property](#navigateurl-property)
8. [Common Navigation Patterns](#common-navigation-patterns)
9. [Best Practices](#best-practices)
10. [Next Steps](#next-steps)

---

## Node Navigation Methods

### Get Node by ID

```csharp
MailItem GetNodeById(string nodeId)
{
    return FlattenTree(MyFolder)
        .FirstOrDefault(x => x.Id == nodeId);
}

List<MailItem> FlattenTree(List<MailItem> items)
{
    var result = new List<MailItem>();
    foreach (var item in items)
    {
        result.Add(item);
        // Add recursive flattening for hierarchical data
    }
    return result;
}
```

### Get Parent Node

```csharp
MailItem GetParentNode(string nodeId)
{
    var node = GetNodeById(nodeId);
    if (node?.ParentId == null)
        return null;  // Root node has no parent
    
    return GetNodeById(node.ParentId);
}
```

### Get Sibling Nodes

```csharp
List<MailItem> GetSiblingNodes(string nodeId)
{
    var node = GetNodeById(nodeId);
    if (node == null)
        return new();
    
    // Get all nodes with same parent
    return MyFolder
        .Where(x => x.ParentId == node.ParentId && x.Id != nodeId)
        .ToList();
}
```

## Traversing Child Nodes

### Get Direct Children

```csharp
List<MailItem> GetDirectChildren(string parentId)
{
    return MyFolder
        .Where(x => x.ParentId == parentId)
        .ToList();
}

// Usage
<button @onclick="() => ShowChildren('1')">Show Children</button>

@code {
    void ShowChildren(string parentId)
    {
        var children = GetDirectChildren(parentId);
        foreach (var child in children)
        {
            Console.WriteLine($"  - {child.FolderName}");
        }
    }
}
```

### Get All Child Nodes Recursively

```csharp
List<MailItem> GetAllChildren(string parentId)
{
    var result = new List<MailItem>();
    var directChildren = GetDirectChildren(parentId);
    
    foreach (var child in directChildren)
    {
        result.Add(child);
        // Recursively add grandchildren
        result.AddRange(GetAllChildren(child.Id));
    }
    
    return result;
}

// Usage
void ShowAllDescendants(string nodeId)
{
    var allChildren = GetAllChildren(nodeId);
    Console.WriteLine($"Total descendants: {allChildren.Count}");
}
```

### Count Child Nodes

```csharp
int GetChildCount(string parentId, bool recursive = false)
{
    if (!recursive)
    {
        return GetDirectChildren(parentId).Count;
    }
    else
    {
        return GetAllChildren(parentId).Count;
    }
}
```

## Traversal Patterns

### Breadth-First Traversal

```csharp
List<MailItem> BreadthFirstTraversal(string rootId)
{
    var result = new List<MailItem>();
    var queue = new Queue<string>();
    queue.Enqueue(rootId);
    
    while (queue.Count > 0)
    {
        var nodeId = queue.Dequeue();
        var node = GetNodeById(nodeId);
        
        if (node != null)
        {
            result.Add(node);
            
            // Add children to queue
            foreach (var child in GetDirectChildren(nodeId))
            {
                queue.Enqueue(child.Id);
            }
        }
    }
    
    return result;
}
```

### Depth-First Traversal

```csharp
List<MailItem> DepthFirstTraversal(string nodeId, List<MailItem> result = null)
{
    result ??= new List<MailItem>();
    
    var node = GetNodeById(nodeId);
    if (node == null)
        return result;
    
    result.Add(node);
    
    // Recursively traverse children
    foreach (var child in GetDirectChildren(nodeId))
    {
        DepthFirstTraversal(child.Id, result);
    }
    
    return result;
}
```

### Level-Order Traversal

```csharp
Dictionary<int, List<MailItem>> GetNodesByLevel(string rootId)
{
    var result = new Dictionary<int, List<MailItem>>();
    var queue = new Queue<(string Id, int Level)>();
    queue.Enqueue((rootId, 0));
    
    while (queue.Count > 0)
    {
        var (nodeId, level) = queue.Dequeue();
        var node = GetNodeById(nodeId);
        
        if (node != null)
        {
            if (!result.ContainsKey(level))
                result[level] = new();
            
            result[level].Add(node);
            
            foreach (var child in GetDirectChildren(nodeId))
            {
                queue.Enqueue((child.Id, level + 1));
            }
        }
    }
    
    return result;
}

// Usage
var byLevel = GetNodesByLevel("1");
foreach (var level in byLevel)
{
    Console.WriteLine($"Level {level.Key}: {level.Value.Count} nodes");
}
```

## Path Navigation

### Get Node Path

```csharp
List<MailItem> GetNodePath(string targetNodeId)
{
    var path = new List<MailItem>();
    var current = GetNodeById(targetNodeId);
    
    while (current != null)
    {
        path.Insert(0, current);  // Insert at beginning
        
        if (current.ParentId == null)
            break;
        
        current = GetNodeById(current.ParentId);
    }
    
    return path;
}

// Usage
void PrintNodePath(string nodeId)
{
    var path = GetNodePath(nodeId);
    var pathStr = string.Join(" > ", path.Select(x => x.FolderName));
    Console.WriteLine($"Path: {pathStr}");
    
    // Example output: "Root > Documents > Work > Projects"
}
```

### Navigate to Node and Expand

```csharp
void NavigateToNode(string targetNodeId)
{
    var path = GetNodePath(targetNodeId);
    
    // Expand all parent nodes
    ExpandedNodeIds = path
        .Where(x => x.ParentId != null)  // Exclude root
        .Select(x => x.Id)
        .ToArray();
    
    // Select the target node
    SelectedNodeIds = new[] { targetNodeId };
    
    StateHasChanged();
}
```

## Keyboard Navigation

### Implement Arrow Key Navigation

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="MailItem" @ref="treeViewRef">
    <TreeViewFieldsSettings TValue="MailItem" DataSource="@MyFolder" />
</SfTreeView>

@code {
    SfTreeView<MailItem> treeViewRef;
    
    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            // Register keyboard event handler
            await RegisterKeyboardEvents();
        }
    }
    
    async Task RegisterKeyboardEvents()
    {
        // Implementation would involve JS interop
        // to listen for arrow keys
    }
    
    void HandleArrowUp()
    {
        var currentSelected = SelectedNodeIds.FirstOrDefault();
        if (currentSelected == null)
            return;
        
        var allNodes = FlattenTree(MyFolder);
        var currentIndex = allNodes.FindIndex(x => x.Id == currentSelected);
        
        if (currentIndex > 0)
        {
            SelectedNodeIds = new[] { allNodes[currentIndex - 1].Id };
        }
    }
    
    void HandleArrowDown()
    {
        var currentSelected = SelectedNodeIds.FirstOrDefault();
        if (currentSelected == null)
            return;
        
        var allNodes = FlattenTree(MyFolder);
        var currentIndex = allNodes.FindIndex(x => x.Id == currentSelected);
        
        if (currentIndex < allNodes.Count - 1)
        {
            SelectedNodeIds = new[] { allNodes[currentIndex + 1].Id };
        }
    }
    
    void HandleArrowRight()
    {
        // Expand selected node
        var selected = SelectedNodeIds.FirstOrDefault();
        if (selected != null)
        {
            if (!ExpandedNodeIds.Contains(selected))
            {
                ExpandedNodeIds = ExpandedNodeIds
                    .Append(selected)
                    .ToArray();
            }
        }
    }
    
    void HandleArrowLeft()
    {
        // Collapse selected node
        var selected = SelectedNodeIds.FirstOrDefault();
        if (selected != null)
        {
            ExpandedNodeIds = ExpandedNodeIds
                .Where(id => id != selected)
                .ToArray();
        }
    }
}
```

## Search and Navigation

### Find Node by Name

```csharp
MailItem FindNodeByName(string name)
{
    return FlattenTree(MyFolder)
        .FirstOrDefault(x => x.FolderName.Equals(name, StringComparison.OrdinalIgnoreCase));
}

MailItem FindNodeByNameContains(string searchText)
{
    return FlattenTree(MyFolder)
        .FirstOrDefault(x => x.FolderName.Contains(searchText, StringComparison.OrdinalIgnoreCase));
}
```

### Filter and Navigate

```csharp
@using Syncfusion.Blazor.Navigations

<input type="text" @bind="SearchText" placeholder="Search nodes..." />
<button @onclick="SearchAndNavigate">Find</button>

<SfTreeView TValue="MailItem" @bind-SelectedNodes="@SelectedNodeIds">
    <TreeViewFieldsSettings TValue="MailItem" DataSource="@MyFolder" />
</SfTreeView>

@code {
    string SearchText = "";
    string[] SelectedNodeIds = Array.Empty<string>();
    
    void SearchAndNavigate()
    {
        var foundNode = FindNodeByName(SearchText);
        if (foundNode != null)
        {
            NavigateToNode(foundNode.Id);
            Console.WriteLine($"Found: {foundNode.FolderName}");
        }
        else
        {
            Console.WriteLine("Node not found");
        }
    }
}
```

## Common Navigation Patterns

### Pattern 1: Breadcrumb Navigation

```csharp
@using Syncfusion.Blazor.Navigations

<div class="breadcrumb">
    @foreach (var item in CurrentPath)
    {
        <span>@item.FolderName</span>
        <span>/</span>
    }
</div>

<SfTreeView TValue="MailItem">
    <TreeViewFieldsSettings TValue="MailItem" DataSource="@MyFolder" />
    <TreeViewEvents TValue="MailItem" NodeSelected="UpdateBreadcrumb"></TreeViewEvents>
</SfTreeView>

@code {
    List<MailItem> CurrentPath = new();
    
    void UpdateBreadcrumb(NodeSelectEventArgs args)
    {
        CurrentPath = GetNodePath(args.NodeData.Id);
    }
}
```

### Pattern 2: Next/Previous Navigation

```csharp
<button @onclick="GoToPreviousNode">← Previous</button>
<button @onclick="GoToNextNode">Next →</button>

@code {
    void GoToPreviousNode()
    {
        var allNodes = FlattenTree(MyFolder);
        var currentSelected = SelectedNodeIds.FirstOrDefault();
        
        if (currentSelected == null)
            return;
        
        var currentIndex = allNodes.FindIndex(x => x.Id == currentSelected);
        if (currentIndex > 0)
        {
            SelectedNodeIds = new[] { allNodes[currentIndex - 1].Id };
        }
    }
    
    void GoToNextNode()
    {
        var allNodes = FlattenTree(MyFolder);
        var currentSelected = SelectedNodeIds.FirstOrDefault();
        
        if (currentSelected == null)
            return;
        
        var currentIndex = allNodes.FindIndex(x => x.Id == currentSelected);
        if (currentIndex < allNodes.Count - 1)
        {
            SelectedNodeIds = new[] { allNodes[currentIndex + 1].Id };
        }
    }
}
```

### Pattern 3: Go to Root

```csharp
void GoToRoot()
{
    var rootNode = MyFolder.FirstOrDefault(x => x.ParentId == null);
    if (rootNode != null)
    {
        SelectedNodeIds = new[] { rootNode.Id };
        ExpandedNodeIds = Array.Empty<string>();
    }
}

<button @onclick="GoToRoot">Go to Root</button>
```

## NavigateUrl Property

The `NavigateUrl` property enables TreeView nodes to act as navigation links, allowing users to navigate to different pages or URLs when clicking a node.

### Configuration

Map the NavigateUrl field in TreeViewFieldsSettings:

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="NavigationItem">
    <TreeViewFieldsSettings TValue="NavigationItem"
        Id="Id"
        Text="Label"
        NavigateUrl="Url"
        Child="Children"
        DataSource="@NavigationItems">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    public class NavigationItem
    {
        public string? Id { get; set; }
        public string? Label { get; set; }
        public string? Url { get; set; }  // NavigateUrl target
        public List<NavigationItem>? Children { get; set; }
    }

    List<NavigationItem> NavigationItems = new()
    {
        new NavigationItem
        {
            Id = "1",
            Label = "Home",
            Url = "/"
        },
        new NavigationItem
        {
            Id = "2",
            Label = "Products",
            Url = "/products"
        },
        new NavigationItem
        {
            Id = "3",
            Label = "About",
            Url = "/about"
        }
    };
}
```

### NavigateUrl Behavior

- **Default behavior:** Node click navigates to URL in new tab/window (if Url contains http/https)
- **Relative URLs:** Navigate within application (e.g., "/dashboard", "/settings")
- **Absolute URLs:** Navigate to external sites (e.g., "https://example.com")
- **Empty URL:** Click selects node without navigation
- **Hash routes:** Support for SPA routing (e.g., "#/products/list")

### Advanced NavigateUrl Patterns

#### Pattern 1: Open in New Tab

```csharp
public class NavigationItem
{
    public string? Id { get; set; }
    public string? Label { get; set; }
    public string? Url { get; set; }
    public string? Target { get; set; } = "_self";  // "_blank" for new tab
    public List<NavigationItem>? Children { get; set; }
}

<SfTreeView TValue="NavigationItem">
    <TreeViewFieldsSettings TValue="NavigationItem"
        Id="Id"
        Text="Label"
        NavigateUrl="Url"
        DataSource="@NavigationItems">
    </TreeViewFieldsSettings>
</SfTreeView>
```

#### Pattern 2: Conditional Navigation with Events

```csharp
<SfTreeView TValue="NavigationItem" @ref="treeRef">
    <TreeViewFieldsSettings TValue="NavigationItem"
        Id="Id"
        Text="Label"
        Child="Children"
        DataSource="@NavigationItems">
    </TreeViewFieldsSettings>
    <TreeViewEvents TValue="NavigationItem" 
        NodeClicked="OnNodeClick">
    </TreeViewEvents>
</SfTreeView>

@code {
    SfTreeView<NavigationItem> treeRef;
    
    void OnNodeClick(NodeClickEventArgs args)
    {
        var node = args.NodeData as NavigationItem;
        
        // Check permissions before navigation
        if (!UserHasPermission(node.Id))
        {
            Console.WriteLine("Access denied");
            return;  // Prevent navigation
        }
        
        // Allow navigation
        if (!string.IsNullOrEmpty(node.Url))
        {
            // Navigate or trigger custom logic
            Console.WriteLine($"Navigating to: {node.Url}");
        }
    }
    
    bool UserHasPermission(string nodeId)
    {
        // Check user role/permission
        return true;  // Implementation specific
    }
}
```

#### Pattern 3: Programmatic Navigation

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="MenuItem">
    <TreeViewFieldsSettings TValue="MenuItem"
        Id="Id"
        Text="Name"
        NavigateUrl="Route"
        DataSource="@MenuItems">
    </TreeViewFieldsSettings>
    <TreeViewEvents TValue="MenuItem"
        NodeClicked="HandleNavigation">
    </TreeViewEvents>
</SfTreeView>

@code {
    private NavigationManager navManager;
    
    void HandleNavigation(NodeClickEventArgs args)
    {
        var menuItem = args.NodeData as MenuItem;
        
        if (!string.IsNullOrEmpty(menuItem.Route))
        {
            // Use NavigationManager for proper SPA navigation
            navManager.NavigateTo(menuItem.Route);
        }
    }
}
```

#### Pattern 4: Data-Driven Routes

```csharp
<SfTreeView TValue="PageNode">
    <TreeViewFieldsSettings TValue="PageNode"
        Id="Id"
        Text="PageName"
        NavigateUrl="GetRoute"
        DataSource="@PageHierarchy">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    public class PageNode
    {
        public string? Id { get; set; }
        public string? PageName { get; set; }
        public string? PageType { get; set; }
        public string? GetRoute()
        {
            return PageType switch
            {
                "product" => $"/products/{Id}",
                "category" => $"/categories/{Id}",
                "user" => $"/users/{Id}",
                _ => "/"
            };
        }
        public List<PageNode>? Children { get; set; }
    }

    List<PageNode> PageHierarchy = new()
    {
        new PageNode
        {
            Id = "prod1",
            PageName = "Laptop",
            PageType = "product"
        }
    };
}
```

### NavigateUrl Advantages

1. **Built-in navigation** - No need for custom click handlers for simple navigation
2. **SEO friendly** - Proper hyperlink structure
3. **Accessibility** - Links work with screen readers
4. **User expectations** - Familiar navigation behavior (Ctrl+click opens in new tab)
5. **Browser history** - Proper back/forward button support

### NavigateUrl Limitations

1. **Cannot validate** before navigation (use NodeClicked event instead)
2. **Must be URL** - Cannot trigger arbitrary methods
3. **No custom animations** - Direct navigation without custom transitions
4. **Limited control** - Cannot modify query parameters on-the-fly

## Best Practices

1. **Cache node lookups** for performance with large trees
2. **Use appropriate traversal** (BFS vs DFS based on needs)
3. **Implement search** for large hierarchies
4. **Provide breadcrumb** for user context
5. **Support keyboard** navigation shortcuts
6. **Handle deep nesting** gracefully
7. **Validate node existence** before navigation
8. **Use NavigateUrl** for simple navigation patterns
9. **Use NodeClicked events** for conditional navigation
10. **Provide visual feedback** for current location

## Troubleshooting

**Issue: Slow navigation**
- Implement caching for node lookups
- Use indexed collections for O(1) access
- Limit traversal depth if possible

**Issue: Lost user context**
- Save and restore navigation state
- Show breadcrumb path
- Highlight current node

**Issue: Deep nesting lag**
- Use virtualization
- Implement lazy loading
- Paginate large result sets

## Next Steps

- Use [node-selection.md](node-selection.md) for selection handling
- Implement [expand-collapse-actions.md](expand-collapse-actions.md) for expansion control
- Check [accessibility.md](accessibility.md) for keyboard support
