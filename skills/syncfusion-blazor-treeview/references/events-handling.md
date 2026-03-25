# Events Handling in TreeView

## Table of Contents
- [Event Structure](#event-structure)
- [Lifecycle Events](#lifecycle-events)
- [Node Interaction Events](#node-interaction-events)
- [Expand/Collapse Events](#expand-collapse-events)
- [Edit Events](#edit-events)
- [Drag-Drop Events](#drag-drop-events)
- [Common Patterns](#common-patterns)

## Event Structure

All TreeView events are attached via the `TreeViewEvents` component:

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="MailItem">
    <TreeViewFieldsSettings TValue="MailItem" DataSource="@MyFolder" />
    <TreeViewEvents TValue="MailItem"
        Created="OnCreated"
        DataBound="OnDataBound"
        NodeSelected="OnNodeSelected"
        NodeClicked="OnNodeClicked"
        NodeExpanded="OnNodeExpanded"
        NodeCollapsed="OnNodeCollapsed"
        NodeEditing="OnNodeEditing"
        NodeEdited="OnNodeEdited"
        OnNodeDragStart="OnDragStart"
        NodeDropped="OnNodeDropped">
    </TreeViewEvents>
</SfTreeView>

@code {
    // Event handlers below
}
```

## Lifecycle Events

### Created Event

Fires when TreeView is fully initialized and rendered:

```csharp
void OnCreated(ActionEventArgs args)
{
    Console.WriteLine("TreeView created and ready");
    
    // Perform post-initialization actions:
    // - Set initial expanded nodes
    // - Load user preferences
    // - Initialize filters
    InitializeTreeAfterCreation();
}

void InitializeTreeAfterCreation()
{
    // Load user's last view state
    SelectedNodeIds = new[] { "1" };
    ExpandedNodeIds = new[] { "1", "2" };
}
```

### DataBound Event

Fires when data is populated in TreeView:

```csharp
void OnDataBound(DataBoundEventArgs args)
{
    Console.WriteLine("Data binding complete");
    Console.WriteLine($"Total nodes: {args.Data?.Count}");
    
    // Perform post-binding operations:
    // - Select default node
    // - Expand specific nodes
    // - Validate data
    ExpandDefaultNodes();
}

void ExpandDefaultNodes()
{
    // Auto-expand root nodes on first load
    var rootNodes = MyFolder
        .Where(x => x.ParentId == null)
        .Select(x => x.Id)
        .ToArray();
    
    ExpandedNodeIds = rootNodes;
}
```

## Node Interaction Events

### NodeSelected Event

Fires when a node is selected (left-click):

```csharp
void OnNodeSelected(NodeSelectEventArgs args)
{
    var node = args.NodeData;
    
    Console.WriteLine($"Selected: {node.Id} - {node.Text}");
    Console.WriteLine($"Level: {node.Level}");
    Console.WriteLine($"Selected: {node.Selected}");
    
    // Access node properties
    SelectedNodeId = node.Id;
    SelectedNodeText = node.Text;
    
    // Load related data
    LoadNodeDetails(node.Id);
}
```

### NodeClicked Event

Fires when a node is clicked (regardless of selection):

```csharp
void OnNodeClicked(NodeClickEventArgs args)
{
    var clickCount = args.ClickCount;
    var node = args.NodeData;
    
    if (clickCount == 1)
    {
        // Single click
        Console.WriteLine($"Single click: {node.Text}");
    }
    else if (clickCount == 2)
    {
        // Double click
        Console.WriteLine($"Double click: {node.Text}");
        // Usually triggers edit mode
    }
}
```

### Prevent Node Selection

```csharp
void OnNodeSelecting(NodeSelectEventArgs args)
{
    // Set Cancel = true to prevent selection
    if (args.NodeData.Id == "restricted")
    {
        args.Cancel = true;
        return;
    }
    
    if (args.NodeData.Level > 2)
    {
        args.Cancel = true;  // Prevent selecting deep nodes
        return;
    }
}
```

## Expand/Collapse Events

### NodeExpanded Event

Fires when a node is expanded:

```csharp
void OnNodeExpanded(NodeExpandEventArgs args)
{
    var nodeId = args.NodeData.Id;
    var nodeText = args.NodeData.Text;
    
    Console.WriteLine($"Expanded: {nodeText}");
    
    // Load child nodes if implementing load-on-demand
    LoadChildNodesIfNeeded(nodeId);
}

async Task LoadChildNodesIfNeeded(string parentId)
{
    // Check if children already loaded
    if (HasChildren(parentId))
        return;
    
    // Fetch from API
    var children = await FetchChildNodes(parentId);
    
    // Add to data source
    foreach (var child in children)
    {
        MyFolder.Add(new MailItem
        {
            Id = child.Id,
            ParentId = parentId,
            FolderName = child.Name
        });
    }
    
    StateHasChanged();
}
```

### NodeCollapsed Event

Fires when a node is collapsed:

```csharp
void OnNodeCollapsed(NodeCollapseEventArgs args)
{
    var nodeId = args.NodeData.Id;
    Console.WriteLine($"Collapsed: {nodeId}");
    
    // Optional: Unload children to save memory
    UnloadChildrenIfLarge(nodeId);
}

void UnloadChildrenIfLarge(string parentId)
{
    var childCount = MyFolder
        .Count(x => x.ParentId == parentId);
    
    if (childCount > 100)
    {
        // Remove children from data source
        var toRemove = MyFolder
            .Where(x => x.ParentId == parentId)
            .ToList();
        
        foreach (var item in toRemove)
            MyFolder.Remove(item);
    }
}
```

## Edit Events

### NodeEditing Event

Fires before entering edit mode:

```csharp
void OnNodeEditing(NodeEditEventArgs args)
{
    var nodeId = args.NodeData.Id;
    var currentText = args.NodeData.Text;
    
    Console.WriteLine($"Before edit: {currentText}");
    
    // Prevent editing certain nodes
    if (nodeId.StartsWith("system_"))
    {
        args.Cancel = true;
        return;
    }
    
    // Check permissions
    if (!UserHasEditPermission(nodeId))
    {
        args.Cancel = true;
        return;
    }
}

bool UserHasEditPermission(string nodeId)
{
    // Check user role or node ownership
    return CurrentUser.Role == "Admin" || CurrentUser.CanEdit;
}
```

### NodeEdited Event

Fires after edit is confirmed:

```csharp
void OnNodeEdited(NodeEditEventArgs args)
{
    var nodeId = args.NodeData.Id;
    var oldText = args.OldText;
    var newText = args.NewText;
    
    Console.WriteLine($"After edit: {oldText} → {newText}");
    
    // Validate new text
    if (!ValidateNodeName(newText))
    {
        args.Cancel = true;
        EditError = "Invalid name";
        return;
    }
    
    // Update data source
    var node = MyFolder.FirstOrDefault(x => x.Id == nodeId);
    if (node != null)
    {
        node.FolderName = newText;
    }
    
    // Update server
    SaveNodeChange(nodeId, newText);
}

bool ValidateNodeName(string name)
{
    // Check for empty
    if (string.IsNullOrWhiteSpace(name))
        return false;
    
    // Check length
    if (name.Length > 100)
        return false;
    
    // Check special characters
    var invalidChars = new[] { '<', '>', '"', '|' };
    if (name.Any(c => invalidChars.Contains(c)))
        return false;
    
    return true;
}
```

## Checkbox Events

### NodeChecking Event

Fires *before* a node's checkbox state changes:

```csharp
void OnNodeChecking(NodeCheckingEventArgs args)
{
    var nodeId = args.NodeData.Id;
    var isChecking = args.IsChecked;
    
    Console.WriteLine($"Before checkbox change: Node {nodeId}, IsChecked: {isChecking}");
    
    // Prevent checking certain nodes
    if (nodeId.StartsWith("readonly_"))
    {
        args.Cancel = true;  // Prevent the check action
        return;
    }
    
    // Validate permissions
    if (!UserHasCheckPermission(nodeId))
    {
        args.Cancel = true;
        return;
    }
    
    // Business logic validation
    if (isChecking && !ValidateCheckPrerequisites(nodeId))
    {
        args.Cancel = true;
        return;
    }
}

bool UserHasCheckPermission(string nodeId)
{
    return CurrentUser.Role == "Admin" || CurrentUser.CanSelectItems;
}

bool ValidateCheckPrerequisites(string nodeId)
{
    // Example: Can't check a node if parent isn't checked
    var node = MyFolder.FirstOrDefault(x => x.Id == nodeId);
    if (node?.ParentId != null)
    {
        var parent = MyFolder.FirstOrDefault(x => x.Id == node.ParentId);
        return parent?.IsChecked == true;
    }
    return true;
}
```

### NodeChecked Event

Fires *after* a node's checkbox state is successfully changed:

```csharp
void OnNodeChecked(NodeCheckedEventArgs args)
{
    var nodeId = args.NodeData.Id;
    var isChecked = args.IsChecked;
    
    Console.WriteLine($"After checkbox change: Node {nodeId}, IsChecked: {isChecked}");
    
    // Update related data
    UpdateNodeMetadata(nodeId, isChecked);
    
    // Trigger dependent actions
    if (isChecked)
    {
        OnNodeCheckActionAsync(nodeId);
    }
    else
    {
        OnNodeUncheckActionAsync(nodeId);
    }
    
    // Log the change
    LogCheckboxChange(nodeId, isChecked);
}

void UpdateNodeMetadata(string nodeId, bool isChecked)
{
    var node = MyFolder.FirstOrDefault(x => x.Id == nodeId);
    if (node != null)
    {
        node.IsChecked = isChecked;
        // Save to database or state management
    }
}

async Task OnNodeCheckActionAsync(string nodeId)
{
    // Perform actions when node is checked
    // Example: Load related data, enable features, etc.
    await InvokeAsync(StateHasChanged);
}

async Task OnNodeUncheckActionAsync(string nodeId)
{
    // Perform actions when node is unchecked
    // Example: Clear selection, disable features, etc.
    await InvokeAsync(StateHasChanged);
}

void LogCheckboxChange(string nodeId, bool isChecked)
{
    Console.WriteLine($"[AUDIT] Checkbox changed: NodeId={nodeId}, IsChecked={isChecked}, Timestamp={DateTime.Now}");
}
```

## Drag-Drop Events

### OnNodeDragStart Event

Fires when drag operation begins:

```csharp
void OnNodeDragStart(DragAndDropEventArgs args)
{
    var draggedNodeId = args.DraggedNodeData.Id;
    
    Console.WriteLine($"Drag started: {draggedNodeId}");
    
    // Prevent dragging certain nodes
    if (draggedNodeId.StartsWith("locked_"))
    {
        args.Cancel = true;
        return;
    }
    
    // Optional: Show visual feedback
    IsDragging = true;
}
```

### OnNodeDragged Event

Fires repeatedly while dragging:

```csharp
void OnNodeDragged(DragAndDropEventArgs args)
{
    var draggedNodeId = args.DraggedNodeData.Id;
    
    // Update visual indicator
    // Note: Cannot cancel at this stage
    Console.WriteLine($"Dragging node: {draggedNodeId}");
}
```

### NodeDropped Event

Fires when node is dropped:

```csharp
void OnNodeDropped(DragAndDropEventArgs args)
{
    var draggedNodeId = args.DraggedNodeData.Id;
    var dropIndex = args.DropIndex;
    
    Console.WriteLine($"Dropped node {draggedNodeId} at index {dropIndex}");
    
    // Update data model
    UpdateNodeHierarchy(draggedNodeId, dropIndex);
}

void UpdateNodeHierarchy(string draggedNodeId, int dropIndex)
{
    var draggedNode = MyFolder.FirstOrDefault(x => x.Id == draggedNodeId);
    
    if (draggedNode == null)
        return;
    
    // Update node position based on dropIndex
    // Remove from current position and insert at new position
    MyFolder.Remove(draggedNode);
    MyFolder.Insert(dropIndex, draggedNode);
    
    // Save to database
    SaveHierarchyChange(draggedNodeId, dropIndex);
}
```

## Event Arguments Reference

### NodeSelectEventArgs
```csharp
public class NodeSelectEventArgs
{
    public TreeViewNodeData NodeData { get; set; }  // Selected node
    public bool Cancel { get; set; }                // Cancel selection
}
```

### NodeClickEventArgs
```csharp
public class NodeClickEventArgs
{
    public TreeViewNodeData NodeData { get; set; }
    public int ClickCount { get; set; }            // 1 or 2
}
```

### NodeExpandEventArgs / NodeCollapseEventArgs
```csharp
public class NodeExpandEventArgs
{
    public TreeViewNodeData NodeData { get; set; }
    public bool Cancel { get; set; }
}
```

### NodeEditEventArgs
```csharp
public class NodeEditEventArgs
{
    public TreeViewNodeData NodeData { get; set; }
    public string OldText { get; set; }
    public string NewText { get; set; }
    public bool Cancel { get; set; }
}
```

### DragAndDropEventArgs
```csharp
public class DragAndDropEventArgs
{
    public TreeViewNodeData DraggedNodeData { get; set; }
    public int DropIndex { get; set; }
    public bool Cancel { get; set; }
}
```

## Common Patterns

### Pattern 1: Multi-Step Edit with Validation

```csharp
string EditingNodeId = "";
string EditError = "";

void OnNodeEditing(NodeEditEventArgs args)
{
    EditingNodeId = args.NodeData.Id;
    EditError = "";
}

void OnNodeEdited(NodeEditEventArgs args)
{
    // Step 1: Validate locally
    if (!ValidateNodeName(args.NewText))
    {
        EditError = "Invalid name format";
        args.Cancel = true;
        return;
    }
    
    // Step 2: Check for duplicates
    if (CheckDuplicate(args.NewText, args.NodeData.Id))
    {
        EditError = "Name already exists";
        args.Cancel = true;
        return;
    }
    
    // Step 3: Update
    var node = MyFolder.FirstOrDefault(x => x.Id == args.NodeData.Id);
    if (node != null)
        node.FolderName = args.NewText;
    
    EditingNodeId = "";
    EditError = "";
}
```

### Pattern 2: Event Logging

```csharp
List<EventLog> EventLogs = new();

void LogEvent(string eventType, string nodeId, string message)
{
    EventLogs.Add(new EventLog
    {
        Timestamp = DateTime.Now,
        EventType = eventType,
        NodeId = nodeId,
        Message = message
    });
}

void OnNodeSelected(NodeSelectEventArgs args)
{
    LogEvent("NodeSelected", args.NodeData.Id, $"Selected: {args.NodeData.Text}");
}

void OnNodeExpanded(NodeExpandEventArgs args)
{
    LogEvent("NodeExpanded", args.NodeData.Id, "Node expanded");
}
```

### Pattern 3: Cascade Events

```csharp
void OnNodeExpanded(NodeExpandEventArgs args)
{
    // When user expands, auto-select first child
    var firstChild = MyFolder.FirstOrDefault(x => x.ParentId == args.NodeData.Id);
    if (firstChild != null)
    {
        SelectedNodeIds = new[] { firstChild.Id };
        StateHasChanged();
    }
}
```

### Pattern 4: Real-time Sync

```csharp
async Task OnNodeEdited(NodeEditEventArgs args)
{
    var nodeId = args.NodeData.Id;
    var newText = args.NewText;
    
    try
    {
        // Update server immediately
        await HttpClient.PutAsJsonAsync(
            $"/api/folders/{nodeId}",
            new { name = newText }
        );
        
        // Show success feedback
        ShowSuccessNotification("Node updated");
    }
    catch (Exception ex)
    {
        // Revert on error
        args.Cancel = true;
        ShowErrorNotification($"Failed to update: {ex.Message}");
    }
}
```

## Additional Events

### DataSourceChanged Event

Fires when the data source is changed or updated:

```csharp
<SfTreeView TValue="Employee">
    <TreeViewFieldsSettings TValue="Employee" DataSource="@Employees" />
    <TreeViewEvents TValue="Employee" DataSourceChanged="OnDataSourceChanged"></TreeViewEvents>
</SfTreeView>

@code {
    void OnDataSourceChanged()
    {
        Console.WriteLine("Data source has been changed");
        
        // Perform actions like:
        // - Refresh calculations
        // - Update derived data
        // - Re-apply filters
        RecalculateNodeStats();
        ApplyCurrentFilters();
    }
    
    void RecalculateNodeStats()
    {
        // Recalculate any statistics based on new data
    }
}
```

### OnActionFailure Event

Fires when a TreeView action fails (e.g., API error during load on demand):

```csharp
<SfTreeView TValue="Employee">
    <TreeViewFieldsSettings TValue="Employee" DataSource="@Employees" />
    <TreeViewEvents TValue="Employee" OnActionFailure="OnActionFailed"></TreeViewEvents>
</SfTreeView>

@code {
    void OnActionFailed(FailureEventArgs args)
    {
        Console.WriteLine($"Action failed: {args.Error}");
        
        // Handle error gracefully
        if (args.Error != null)
        {
            // Log error
            LogError(args.Error);
            
            // Show user notification
            ShowErrorNotification("Failed to load data. Please try again.");
            
            // Attempt recovery
            AttemptRecovery();
        }
    }
    
    void LogError(Exception error)
    {
        // Send to error tracking service
        Console.WriteLine($"Error: {error.Message}");
    }
    
    void AttemptRecovery()
    {
        // Reload data or reset state
    }
}
```

### OnKeyPress Event

Fires when user presses a key while TreeView has focus:

```csharp
<SfTreeView TValue="Employee">
    <TreeViewFieldsSettings TValue="Employee" DataSource="@Employees" />
    <TreeViewEvents TValue="Employee" OnKeyPress="OnKeyPressed"></TreeViewEvents>
</SfTreeView>

@code {
    void OnKeyPressed(KeyboardEventArgs args)
    {
        Console.WriteLine($"Key pressed: {args.Key}");
        
        switch (args.Key)
        {
            case "Enter":
                HandleEnterKey();
                break;
            case "Delete":
                HandleDeleteKey();
                break;
            case "F2":
                HandleEditKey();
                break;
            case "ArrowUp":
            case "ArrowDown":
            case "ArrowLeft":
            case "ArrowRight":
                // Navigation handled automatically
                break;
        }
    }
    
    void HandleEnterKey()
    {
        // Expand/Collapse or Select action
        Console.WriteLine("Enter key pressed");
    }
    
    void HandleDeleteKey()
    {
        // Delete current node
        Console.WriteLine("Delete key pressed");
    }
    
    void HandleEditKey()
    {
        // Enter edit mode
        Console.WriteLine("F2 (Edit) key pressed");
    }
}
```

**Keyboard Shortcuts Reference**:
- **Enter**: Expand/Collapse or Select node
- **Delete**: Delete selected node
- **F2**: Enter edit mode
- **Arrow Keys**: Navigate between nodes
- **Ctrl+A**: Select all nodes (if multi-selection enabled)
- **Ctrl+C/X/V**: Copy/Cut/Paste (if editing enabled)
- **Escape**: Exit edit mode

### OnNodeRender Event

Fires for each node as it's rendered, allowing custom rendering logic:

```csharp
<SfTreeView TValue="Employee">
    <TreeViewFieldsSettings TValue="Employee" DataSource="@Employees" />
    <TreeViewEvents TValue="Employee" OnNodeRender="OnNodeRender"></TreeViewEvents>
</SfTreeView>

@code {
    void OnNodeRender(NodeRenderEventArgs args)
    {
        var nodeData = args.Node;
        
        Console.WriteLine($"Rendering node: {nodeData.Text}");
        
        // Apply conditional formatting
        if (nodeData.Level > 3)
        {
            // Hide deeply nested nodes
            args.Node.Hidden = true;
        }
        
        // Add custom classes
        if (IsHighPriority(nodeData))
        {
            args.Node.CssClass = "priority-high";
        }
        else if (IsLowPriority(nodeData))
        {
            args.Node.CssClass = "priority-low";
        }
        
        // Modify node appearance
        if (IsDisabled(nodeData))
        {
            args.Node.Disabled = true;
        }
    }
    
    bool IsHighPriority(TreeViewNodeData node)
    {
        // Custom logic to determine priority
        return node.Text?.Contains("URGENT") ?? false;
    }
    
    bool IsLowPriority(TreeViewNodeData node)
    {
        return node.Level > 2;
    }
    
    bool IsDisabled(TreeViewNodeData node)
    {
        return node.Id?.StartsWith("disabled_") ?? false;
    }
}

<style>
    .priority-high {
        color: red;
        font-weight: bold;
    }
    
    .priority-low {
        color: gray;
        opacity: 0.7;
    }
</style>
```

---

## Complete Event Handler Attachment

Attach multiple events in one place:

```csharp
<SfTreeView TValue="Employee">
    <TreeViewFieldsSettings TValue="Employee" DataSource="@Employees" />
    <TreeViewEvents TValue="Employee"
        Created="OnCreated"
        DataBound="OnDataBound"
        DataSourceChanged="OnDataSourceChanged"
        NodeSelected="OnNodeSelected"
        NodeClicked="OnNodeClicked"
        NodeExpanding="OnNodeExpanding"
        NodeExpanded="OnNodeExpanded"
        NodeCollapsing="OnNodeCollapsing"
        NodeCollapsed="OnNodeCollapsed"
        NodeEditing="OnNodeEditing"
        NodeEdited="OnNodeEdited"
        NodeChecking="OnNodeChecking"
        NodeChecked="OnNodeChecked"
        OnNodeDragStart="OnDragStart"
        OnNodeDragged="OnDragged"
        OnNodeDragStop="OnDragStop"
        NodeDropped="OnNodeDropped"
        OnKeyPress="OnKeyPressed"
        OnNodeRender="OnNodeRender"
        OnActionFailure="OnActionFailed">
    </TreeViewEvents>
</SfTreeView>

@code {
    // Define all handlers here
}
```

---

## Best Practices

1. **Handle errors gracefully** in event handlers
2. **Cancel events** when validation fails
3. **Provide user feedback** via toasts or status messages
4. **Log important events** for debugging
5. **Avoid heavy operations** in frequently-fired events (OnNodeDragged)
6. **Use StateHasChanged()** after manual updates
7. **Cache event results** to avoid repeated calculations
8. **Handle keyboard events** for accessibility
9. **Use OnNodeRender** for conditional formatting
10. **Catch OnActionFailure** for error recovery

## Troubleshooting

**Issue: Events not firing**
- Ensure TreeViewEvents component is added
- Check event handler names match property names
- Verify no exceptions in event handler

**Issue: Cancel not working**
- Some events don't support Cancel (OnNodeDragged)
- Verify Cancel property exists on event args
- Check cancel logic is correct

**Issue: Performance degradation**
- Move heavy logic from OnNodeDragged to NodeDropped
- Cache data instead of recalculating
- Use throttling for rapid-fire events

## Next Steps

- Use [expand-collapse-actions.md](expand-collapse-actions.md) for expand events
- Implement [node-editing.md](node-editing.md) for edit events
- Check [advanced-features.md](advanced-features.md) for drag-drop details
