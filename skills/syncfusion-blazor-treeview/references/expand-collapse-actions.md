# Expand/Collapse Actions in TreeView

## Table of Contents
- [Data Binding Expansion](#data-binding-expansion)
- [Method APIs](#method-apis)
- [ExpandedNodes API](#expandednodes-api)
- [Programmatic Expand/Collapse](#programmatic-expand-collapse)
- [Expand on Demand](#expand-on-demand)
- [Events](#events)
- [Common Patterns](#common-patterns)

## Data Binding Expansion

### Expand Nodes via Data Source

Set `Expanded="true"` in your data to expand nodes on initialization:

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="MailItem">
    <TreeViewFieldsSettings TValue="MailItem"
        Id="Id"
        Text="FolderName"
        ParentID="ParentId"
        HasChildren="HasSubFolders"
        Expanded="Expanded"
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
        public bool Expanded { get; set; }  // Controls initial expansion
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox", HasSubFolders = true, Expanded = true },
        new MailItem { Id = "1-1", ParentId = "1", FolderName = "Important" },
        new MailItem { Id = "1-2", ParentId = "1", FolderName = "Drafts" },
        new MailItem { Id = "2", FolderName = "Sent", HasSubFolders = false, Expanded = false }
    };
}
```

**Behavior:**
- Nodes with `Expanded="true"` will be expanded on initial render
- Only affects initial state; user can still collapse them

## Method APIs

### ExpandAllAsync() - Expand All Nodes

Use `ExpandAllAsync()` to programmatically expand all nodes or specific nodes by passing node IDs:

```csharp
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="ExpandAllNodes">Expand All Nodes</SfButton>
<SfButton OnClick="ExpandSpecificNode">Expand MP3 Albums Only</SfButton>

<SfTreeView TValue="MusicAlbum" @ref="treeview" ShowCheckBox="true" AutoCheck="true">
    <TreeViewFieldsSettings TValue="MusicAlbum" 
        Id="Id" 
        DataSource="@Albums" 
        Text="Name" 
        ParentID="ParentId" 
        HasChildren="HasChild" 
        Expanded="Expanded" 
        IsChecked="IsChecked">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    SfTreeView<MusicAlbum>? treeview;

    public class MusicAlbum
    {
        public string? Id { get; set; }
        public string? ParentId { get; set; }
        public string? Name { get; set; }
        public bool Expanded { get; set; }
        public bool? IsChecked { get; set; }
        public bool HasChild { get; set; }
    }

    List<MusicAlbum> Albums = new();

    protected override void OnInitialized()
    {
        Albums.Add(new MusicAlbum { Id = "1", Name = "Discover Music", HasChild = true });
        Albums.Add(new MusicAlbum { Id = "2", ParentId = "1", Name = "Hot Singles" });
        Albums.Add(new MusicAlbum { Id = "3", ParentId = "1", Name = "Rising Artists" });
        Albums.Add(new MusicAlbum { Id = "4", ParentId = "1", Name = "Live Music" });
        Albums.Add(new MusicAlbum { Id = "04", HasChild = true, Name = "MP3 Albums" });
        Albums.Add(new MusicAlbum { Id = "5", ParentId = "04", Name = "Rock", IsChecked = true });
        Albums.Add(new MusicAlbum { Id = "6", ParentId = "04", Name = "Gospel" });
        Albums.Add(new MusicAlbum { Id = "7", ParentId = "04", Name = "Latin Music" });
        Albums.Add(new MusicAlbum { Id = "8", ParentId = "04", Name = "Jazz" });
    }

    // Expand all nodes in the TreeView
    public async Task ExpandAllNodes()
    {
        await treeview.ExpandAllAsync();
    }

    // Expand specific node by ID
    public async Task ExpandSpecificNode()
    {
        await treeview.ExpandAllAsync(new string[] { "04" });
    }
}
```

**Usage:**
- `ExpandAllAsync()` - No parameters: expands all nodes
- `ExpandAllAsync(string[] nodeIds)` - Pass node IDs to expand specific nodes only

### CollapseAllAsync() - Collapse All Nodes

Use `CollapseAllAsync()` to programmatically collapse all nodes or specific nodes:

```csharp
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="CollapseAllNodes">Collapse All Nodes</SfButton>
<SfButton OnClick="CollapseSpecificNode">Collapse MP3 Albums</SfButton>

<SfTreeView TValue="MusicAlbum" @ref="treeview" ShowCheckBox="true" AutoCheck="true">
    <TreeViewFieldsSettings TValue="MusicAlbum" 
        Id="Id" 
        DataSource="@Albums" 
        Text="Name" 
        ParentID="ParentId" 
        HasChildren="HasChild" 
        Expanded="Expanded" 
        IsChecked="IsChecked">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    SfTreeView<MusicAlbum>? treeview;

    public class MusicAlbum
    {
        public string? Id { get; set; }
        public string? ParentId { get; set; }
        public string? Name { get; set; }
        public bool Expanded { get; set; }
        public bool? IsChecked { get; set; }
        public bool HasChild { get; set; }
    }

    List<MusicAlbum> Albums = new();

    protected override void OnInitialized()
    {
        Albums.Add(new MusicAlbum { Id = "1", Name = "Discover Music", HasChild = true });
        Albums.Add(new MusicAlbum { Id = "2", ParentId = "1", Name = "Hot Singles" });
        Albums.Add(new MusicAlbum { Id = "3", ParentId = "1", Name = "Rising Artists" });
        Albums.Add(new MusicAlbum { Id = "4", ParentId = "1", Name = "Live Music" });
        Albums.Add(new MusicAlbum { Id = "04", HasChild = true, Name = "MP3 Albums" });
        Albums.Add(new MusicAlbum { Id = "5", ParentId = "04", Name = "Rock", IsChecked = true });
        Albums.Add(new MusicAlbum { Id = "6", ParentId = "04", Name = "Gospel" });
        Albums.Add(new MusicAlbum { Id = "7", ParentId = "04", Name = "Latin Music" });
        Albums.Add(new MusicAlbum { Id = "8", ParentId = "04", Name = "Jazz" });
    }

    // Collapse all nodes in the TreeView
    public async Task CollapseAllNodes()
    {
        await treeview.CollapseAllAsync();
    }

    // Collapse specific node by ID
    public async Task CollapseSpecificNode()
    {
        await treeview.CollapseAllAsync(new string[] { "04" });
    }
}
```

**Usage:**
- `CollapseAllAsync()` - No parameters: collapses all nodes
- `CollapseAllAsync(string[] nodeIds)` - Pass node IDs to collapse specific nodes only

### Hierarchical Data Expansion

```csharp
<SfTreeView TValue="Folder">
    <TreeViewFieldsSettings TValue="Folder"
        Id="Id"
        Text="FolderName"
        Child="SubFolders"
        Expanded="IsExpanded"
        DataSource="@RootFolders">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    public class Folder
    {
        public string? Id { get; set; }
        public string? FolderName { get; set; }
        public bool IsExpanded { get; set; }
        public List<Folder>? SubFolders { get; set; }
    }

    List<Folder> RootFolders = new()
    {
        new Folder
        {
            Id = "docs",
            FolderName = "Documents",
            IsExpanded = true,
            SubFolders = new()
            {
                new Folder { Id = "work", FolderName = "Work", IsExpanded = true },
                new Folder { Id = "personal", FolderName = "Personal" }
            }
        }
    };
}
```

## ExpandedNodes API

### Two-Way Binding for Expanded State

Use `@bind-ExpandedNodes` to create a two-way binding for programmatic control:

```csharp
@using Syncfusion.Blazor.Navigations

<button @onclick="ExpandAll">Expand All</button>
<button @onclick="CollapseAll">Collapse All</button>
<button @onclick="ToggleNode">Toggle First Node</button>

<SfTreeView TValue="MailItem" @bind-ExpandedNodes="@ExpandedNodeIds">
    <TreeViewFieldsSettings TValue="MailItem"
        Id="Id"
        ParentID="ParentId"
        HasChildren="HasSubFolders"
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
</SfTreeView>

<div>
    <p>Expanded Nodes: @string.Join(", ", ExpandedNodeIds)</p>
</div>

@code {
    string[] ExpandedNodeIds = Array.Empty<string>();

    void ExpandAll()
    {
        // Expand all parent nodes
        ExpandedNodeIds = MyFolder
            .Where(x => x.HasSubFolders)
            .Select(x => x.Id)
            .ToArray();
    }

    void CollapseAll()
    {
        // Collapse all nodes
        ExpandedNodeIds = Array.Empty<string>();
    }

    void ToggleNode()
    {
        var firstParentId = MyFolder.FirstOrDefault(x => x.HasSubFolders)?.Id;
        if (firstParentId != null)
        {
            if (ExpandedNodeIds.Contains(firstParentId))
            {
                ExpandedNodeIds = ExpandedNodeIds
                    .Where(id => id != firstParentId)
                    .ToArray();
            }
            else
            {
                ExpandedNodeIds = ExpandedNodeIds
                    .Append(firstParentId)
                    .ToArray();
            }
        }
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox", HasSubFolders = true },
        new MailItem { Id = "1-1", ParentId = "1", FolderName = "Important" },
        new MailItem { Id = "2", FolderName = "Sent", HasSubFolders = true },
        new MailItem { Id = "2-1", ParentId = "2", FolderName = "Archive" }
    };

    public class MailItem
    {
        public string? Id { get; set; }
        public string? ParentId { get; set; }
        public string? FolderName { get; set; }
        public bool HasSubFolders { get; set; }
    }
}
```

**Benefits:**
- Full programmatic control
- Bidirectional updates (UI changes sync to component)
- Useful for toolbar buttons and keyboard shortcuts

## Programmatic Expand/Collapse

### Expand Specific Nodes

```csharp
@using Syncfusion.Blazor.Navigations

<button @onclick="ExpandInboxAndSubFolders">Expand Inbox</button>

<SfTreeView TValue="MailItem" @bind-ExpandedNodes="@ExpandedNodeIds">
    <TreeViewFieldsSettings TValue="MailItem"
        Id="Id"
        ParentID="ParentId"
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    string[] ExpandedNodeIds = Array.Empty<string>();

    void ExpandInboxAndSubFolders()
    {
        // Expand specific node and its parents
        var nodesToExpand = new[] { "1", "1-1", "1-2" };
        ExpandedNodeIds = nodesToExpand;
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox", ParentId = null },
        new MailItem { Id = "1-1", ParentId = "1", FolderName = "Important" },
        new MailItem { Id = "1-2", ParentId = "1", FolderName = "Drafts" }
    };

    public class MailItem
    {
        public string? Id { get; set; }
        public string? ParentId { get; set; }
        public string? FolderName { get; set; }
    }
}
```

### Collapse Specific Nodes

```csharp
void CollapseNode(string nodeId)
{
    ExpandedNodeIds = ExpandedNodeIds
        .Where(id => id != nodeId)
        .ToArray();
}

// Usage
<button @onclick="() => CollapseNode('1')">Collapse Inbox</button>
```

## Expand on Demand

### Dynamic Loading on Expand

Load child nodes only when parent is expanded:

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="MailItem" LoadOnDemand="true">
    <TreeViewFieldsSettings TValue="MailItem"
        Id="Id"
        ParentID="ParentId"
        HasChildren="HasSubFolders"
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
    <TreeViewEvents TValue="MailItem" NodeExpanded="OnNodeExpanded"></TreeViewEvents>
</SfTreeView>

@code {
    async Task OnNodeExpanded(NodeExpandEventArgs args)
    {
        var parentId = args.NodeData.Id;
        
        // Fetch child nodes from API
        var children = await FetchChildNodes(parentId);
        
        // Add to data source
        var newItems = children
            .Select(x => new MailItem 
            { 
                Id = x.Id, 
                ParentId = parentId,
                FolderName = x.Name 
            })
            .ToList();
        
        MyFolder.AddRange(newItems);
        StateHasChanged();
    }

    async Task<List<dynamic>> FetchChildNodes(string parentId)
    {
        // Simulate API call
        await Task.Delay(500);
        return new() 
        { 
            new { Id = $"{parentId}-1", Name = "Subfolder 1" },
            new { Id = $"{parentId}-2", Name = "Subfolder 2" }
        };
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox", HasSubFolders = true },
        new MailItem { Id = "2", FolderName = "Sent", HasSubFolders = true }
    };

    public class MailItem
    {
        public string? Id { get; set; }
        public string? ParentId { get; set; }
        public string? FolderName { get; set; }
        public bool HasSubFolders { get; set; }
    }
}
```

**LoadOnDemand Benefits:**
- Faster initial render
- Lower bandwidth usage
- Scales well with large hierarchies

## Events

### NodeExpanded Event

Fires when a node is expanded:

```csharp
void OnNodeExpanded(NodeExpandEventArgs args)
{
    var expandedNodeId = args.NodeData.Id;
    var expandedNodeText = args.NodeData.Text;
    
    Console.WriteLine($"Expanded: {expandedNodeText}");
    
    // Load data, track analytics, etc.
    LogNodeExpansion(expandedNodeId);
}

<TreeViewEvents TValue="MailItem" NodeExpanded="OnNodeExpanded"></TreeViewEvents>
```

### NodeCollapsed Event

Fires when a node is collapsed:

```csharp
void OnNodeCollapsed(NodeCollapseEventArgs args)
{
    var collapsedNodeId = args.NodeData.Id;
    Console.WriteLine($"Collapsed: {collapsedNodeId}");
}

<TreeViewEvents TValue="MailItem" NodeCollapsed="OnNodeCollapsed"></TreeViewEvents>
```

### Prevent Expand/Collapse

Cancel expand/collapse operations:

```csharp
void OnNodeExpanding(NodeExpandEventArgs args)
{
    // Set Cancel = true to prevent expansion
    if (args.NodeData.Id == "restricted")
    {
        args.Cancel = true;
        return;
    }
}
```

## Animation on Expand/Collapse

### TreeViewNodeAnimationSettings

The `TreeViewNodeAnimationSettings` component controls the animations that play when nodes expand and collapse. It contains two child components: `TreeViewAnimationExpand` and `TreeViewAnimationCollapse`.

**Configuration Properties:**
- **Duration** - Animation duration in milliseconds
- **Effect** - Type of animation (SlideDown, SlideUp, FadeIn, FadeOut, etc.)
- **Easing** - Animation timing function (Linear, Ease, EaseIn, EaseOut, etc.)

### Basic Animation Setup

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="MailItem">
    <TreeViewFieldsSettings TValue="MailItem"
        Id="Id"
        Text="FolderName"
        ParentID="ParentId"
        HasChildren="HasSubFolders"
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
    
    <!-- Configure animations -->
    <TreeViewNodeAnimationSettings>
        <!-- Animation when expanding nodes -->
        <TreeViewAnimationExpand 
            Duration="500" 
            Effect="@AnimationEffect.SlideDown" 
            Easing="Linear">
        </TreeViewAnimationExpand>
        
        <!-- Animation when collapsing nodes -->
        <TreeViewAnimationCollapse 
            Duration="500" 
            Effect="@AnimationEffect.SlideUp" 
            Easing="Linear">
        </TreeViewAnimationCollapse>
    </TreeViewNodeAnimationSettings>
</SfTreeView>

@code {
    public class MailItem
    {
        public string? Id { get; set; }
        public string? ParentId { get; set; }
        public string? FolderName { get; set; }
        public bool HasSubFolders { get; set; }
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox", HasSubFolders = true },
        new MailItem { Id = "1-1", ParentId = "1", FolderName = "Important" },
        new MailItem { Id = "1-2", ParentId = "1", FolderName = "Drafts" }
    };
}
```

### Disable Animations

Set `Duration="0"` to disable animations:

```csharp
<TreeViewNodeAnimationSettings>
    <TreeViewAnimationExpand Duration="0"></TreeViewAnimationExpand>
    <TreeViewAnimationCollapse Duration="0"></TreeViewAnimationCollapse>
</TreeViewNodeAnimationSettings>
```

### Custom Animation Effects

```csharp
<TreeViewNodeAnimationSettings>
    <!-- Fast expand with fade in -->
    <TreeViewAnimationExpand 
        Duration="300" 
        Effect="@AnimationEffect.FadeIn" 
        Easing="EaseOut">
    </TreeViewAnimationExpand>
    
    <!-- Slow collapse with fade out -->
    <TreeViewAnimationCollapse 
        Duration="800" 
        Effect="@AnimationEffect.FadeOut" 
        Easing="EaseIn">
    </TreeViewAnimationCollapse>
</TreeViewNodeAnimationSettings>
```

**Available Animation Effects:**
- `SlideDown` - Node slides down when expanding
- `SlideUp` - Node slides up when collapsing
- `FadeIn` - Node fades in when expanding
- `FadeOut` - Node fades out when collapsing

### Animation Reference

**TreeViewNodeAnimationSettings Properties:**
```csharp
public class TreeViewNodeAnimationSettings
{
    // Child components for expand/collapse animations
    public TreeViewAnimationExpand Expand { get; set; }
    public TreeViewAnimationCollapse Collapse { get; set; }
}

public class TreeViewAnimationExpand
{
    public int Duration { get; set; }          // Milliseconds (default: 400)
    public AnimationEffect Effect { get; set; } // Animation type
    public string Easing { get; set; }         // Timing function (default: Linear)
}

public class TreeViewAnimationCollapse
{
    public int Duration { get; set; }          // Milliseconds (default: 400)
    public AnimationEffect Effect { get; set; } // Animation type
    public string Easing { get; set; }         // Timing function (default: Linear)
}
```

## Common Patterns

### Pattern 1: Expand to Selected Node

```csharp
@using Syncfusion.Blazor.Navigations

<button @onclick="ExpandToNode">Go to Item 3-2</button>

<SfTreeView TValue="Item" @bind-ExpandedNodes="@ExpandedNodeIds">
    <TreeViewFieldsSettings TValue="Item" 
        Id="Id"
        ParentID="ParentId"
        DataSource="@Items">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    string[] ExpandedNodeIds = Array.Empty<string>();

    void ExpandToNode()
    {
        // Expand all parent nodes to make node "3-2" visible
        ExpandedNodeIds = new[] { "3", "3-2" };  // Expand parent and node
    }

    List<Item> Items = new()
    {
        new Item { Id = "3", ParentId = null, Name = "Category 3" },
        new Item { Id = "3-2", ParentId = "3", Name = "Item 3-2" }
    };

    class Item
    {
        public string? Id { get; set; }
        public string? ParentId { get; set; }
        public string? Name { get; set; }
    }
}
```

### Pattern 2: Expand First Level Only

```csharp
void ExpandFirstLevelOnly()
{
    // Expand only top-level parent nodes
    ExpandedNodeIds = Items
        .Where(x => x.ParentId == null && x.HasChildren)
        .Select(x => x.Id)
        .ToArray();
}
```

### Pattern 3: Remember Expanded State

```csharp
protected override async Task OnInitializedAsync()
{
    // Load expanded state from local storage
    ExpandedNodeIds = await LoadExpandedStateFromStorage();
}

async Task SaveExpandedState()
{
    // Save current expanded state to local storage
    await SaveExpandedStateToStorage(ExpandedNodeIds);
}

<button @onclick="SaveExpandedState">Save State</button>
```

### Pattern 4: Expand Selected Path

```csharp
void ExpandNodePath(string targetNodeId)
{
    var pathToNode = GetNodePath(targetNodeId);
    ExpandedNodeIds = pathToNode.Select(x => x.Id).ToArray();
}

List<Item> GetNodePath(string nodeId)
{
    var path = new List<Item>();
    var current = Items.FirstOrDefault(x => x.Id == nodeId);
    
    while (current != null)
    {
        path.Insert(0, current);
        current = current.ParentId == null 
            ? null 
            : Items.FirstOrDefault(x => x.Id == current.ParentId);
    }
    
    return path;
}
```

## Best Practices

1. **Use Expanded field** for initial state from data
2. **Use ExpandedNodes binding** for programmatic control
3. **Enable LoadOnDemand** for large datasets
4. **Handle NodeExpanded event** for dynamic loading
5. **Provide expand/collapse All buttons** for UX
6. **Save/restore state** for returning users
7. **Use sensible defaults** (expand relevant nodes initially)

## Troubleshooting

**Issue: Expanded state not persisting**
- Use `@bind-ExpandedNodes` for two-way binding
- Call StateHasChanged() after updates
- Verify node IDs are unique

**Issue: Nodes not expanding**
- Ensure HasChildren is true for parent nodes
- Check that ParentID mappings are correct
- Verify Expanded field is included in binding

**Issue: LoadOnDemand not working**
- Confirm LoadOnDemand="true"
- Verify NodeExpanded event is handling data loading
- Check API response format matches expected structure

## Next Steps

- Use [node-selection.md](node-selection.md) to select expanded nodes
- Handle [events-handling.md](events-handling.md) for all expansion events
- Implement [navigation-patterns.md](navigation-patterns.md) for node traversal
