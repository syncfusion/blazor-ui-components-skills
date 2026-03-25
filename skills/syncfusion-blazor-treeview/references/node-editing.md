# Node Editing in TreeView

## Table of Contents

1. [Enable Node Editing](#enable-node-editing)
2. [How Node Editing Works](#how-node-editing-works)
3. [Method APIs for Node Operations](#method-apis-for-node-operations)
4. [Edit Settings Configuration](#edit-settings-configuration)
5. [Handling Edit Events](#handling-edit-events)
6. [Rename Operations](#rename-operations)
7. [Validation During Edit](#validation-during-edit)
8. [Cancel Editing](#cancel-editing)
9. [Edit State Management](#edit-state-management)
10. [Common Patterns](#common-patterns)
11. [Reset Edit State](#reset-edit-state)
12. [Best Practices](#best-practices)
13. [Troubleshooting](#troubleshooting)
14. [Next Steps](#next-steps)

---

## Enable Node Editing

Enable editing mode by setting `AllowEditing="true"`:

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="MailItem" AllowEditing="true">
    <TreeViewFieldsSettings TValue="MailItem"
        Id="Id"
        Text="FolderName"
        ParentID="ParentId"
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    public class MailItem
    {
        public string? Id { get; set; }
        public string? ParentId { get; set; }
        public string? FolderName { get; set; }
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox" },
        new MailItem { Id = "2", FolderName = "Sent" }
    };
}
```

## How Node Editing Works

**User Interaction:**
1. Double-click a node to enter edit mode
2. Edit text in the input field
3. Press **Enter** to confirm
4. Press **Escape** to cancel

**Programmatic Approach:**
Use events and APIs to control editing behavior

## Method APIs for Node Operations

### BeginEditAsync() - Start Node Editing Programmatically

Use `BeginEditAsync()` to start edit mode for a specific node by ID:

```csharp
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="EditSelectedNode">Edit Selected Node</SfButton>

<div id="treeview">
    <SfTreeView TValue="EmployeeData" @ref="tree" @bind-SelectedNodes="@selectedNodes" @bind-ExpandedNodes="expandedNodes">
        <TreeViewFieldsSettings Id="Id" ParentID="Pid" DataSource="@ListData" Text="Name" HasChildren="HasChild"></TreeViewFieldsSettings>
        <TreeViewEvents TValue="EmployeeData" NodeSelected="OnSelect" NodeClicked="nodeClicked"></TreeViewEvents>
    </SfTreeView>
</div>

@code
{
    SfTreeView<EmployeeData>? tree;
    string selectedId = string.Empty;
    public string[] selectedNodes = Array.Empty<string>();
    public string[] expandedNodes = new string[] { };

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public string? Pid { get; set; }
        public bool HasChild { get; set; }
    }

    public void OnSelect(NodeSelectEventArgs args)
    {
        selectedId = args.NodeData.Id;
    }

    public void nodeClicked(NodeClickEventArgs args)
    {
        selectedId = args.NodeData.Id;
        selectedNodes = new string[] { args.NodeData.Id };
    }

    // Begin edit mode for the selected node
    public async Task EditSelectedNode()
    {
        if (!string.IsNullOrEmpty(selectedId))
        {
            await tree.BeginEditAsync(selectedId);
        }
    }

    List<EmployeeData> ListData = new()
    {
        new EmployeeData { Id = "1", Name = "Johnson", HasChild = true },
        new EmployeeData { Id = "2", Pid = "1", Name = "Sourav" },
        new EmployeeData { Id = "3", Pid = "1", Name = "Sanjay" },
        new EmployeeData { Id = "4", Pid = "1", Name = "Steve" },
        new EmployeeData { Id = "7", Name = "Laura", HasChild = true },
        new EmployeeData { Id = "8", Pid = "7", Name = "Mic" },
        new EmployeeData { Id = "9", Pid = "7", Name = "Nancy" }
    };
}
```

**Usage:**
- `BeginEditAsync(string nodeId)` - Initiates edit mode for the node with specified ID
- User can then modify the node text in the input field
- Press **Enter** to save or **Escape** to cancel

### GetTreeData() - Retrieve Node Data

Use `GetTreeData()` to retrieve tree data for entire tree or specific nodes:

```csharp
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Inputs
@using Newtonsoft.Json;

<SfMaskedTextBox @ref="mask" Placeholder="Enter Node ID (ex: NA)" FloatLabelType="@FloatLabelType.Always" Width="250"></SfMaskedTextBox>
<button class="e-btn e-info" @onclick="@GetNodeDetails">Get Node Details</button>
<br />
@if (NodeDetails != null)
{
    <p>@NodeDetails</p>
}

<SfTreeView TValue="TreeData" @ref="tree">
    <TreeViewFieldsSettings DataSource="@TreeDataSource" Id="Code" Text="Name" Selected="Selected" Expanded="Expanded" Child="Child"></TreeViewFieldsSettings>
</SfTreeView>

@code
{
    List<TreeData> TreeDataSource = new();
    SfTreeView<TreeData>? tree;
    SfMaskedTextBox? mask;
    string? NodeDetails = null;

    // Retrieve data for specific node by ID
    void GetNodeDetails()
    {
        if (tree != null && mask != null)
        {
            string enteredId = mask.Value?.ToString() ?? string.Empty;
            List<TreeData> nodeData = tree.GetTreeData(enteredId);
            
            if (nodeData.Count > 0)
            {
                NodeDetails = $"Node ID: {nodeData[0].Code} | Name: {nodeData[0].Name}";
            }
            else
            {
                NodeDetails = "Node not found";
            }
            StateHasChanged();
        }
    }

    // Alternative: Get all tree data
    void GetAllTreeData()
    {
        if (tree != null)
        {
            List<TreeData> allData = tree.GetTreeData();
            Console.WriteLine($"Total nodes: {allData.Count}");
        }
    }

    protected override void OnInitialized()
    {
        TreeDataSource.Add(new TreeData
        {
            Code = "NA",
            Name = "North America",
            Child = new List<TreeData>()
            {
                new TreeData { Code = "USA", Name = "United States" },
                new TreeData { Code = "CUB", Name = "Cuba" },
                new TreeData { Code = "MEX", Name = "Mexico" }
            }
        });
        TreeDataSource.Add(new TreeData
        {
            Code = "AF",
            Name = "Africa",
            Child = new List<TreeData>()
            {
                new TreeData { Code = "NGA", Name = "Nigeria" },
                new TreeData { Code = "EGY", Name = "Egypt" },
                new TreeData { Code = "ZAF", Name = "South Africa" }
            }
        });
        TreeDataSource.Add(new TreeData
        {
            Code = "AS",
            Name = "Asia",
            Child = new List<TreeData>()
            {
                new TreeData { Code = "CHN", Name = "China" },
                new TreeData { Code = "IND", Name = "India" },
                new TreeData { Code = "JPN", Name = "Japan" }
            }
        });
    }

    public class TreeData
    {
        public string? Code { get; set; }
        public string? Name { get; set; }
        public bool Expanded { get; set; }
        public bool Selected { get; set; }
        public List<TreeData>? Child { get; set; }
    }
}
```

**Usage:**
- `GetTreeData()` - No parameters: returns entire tree data
- `GetTreeData(string nodeId)` - Returns specific node data by ID
- Useful for retrieving node information programmatically for CRUD operations

## Edit Settings Configuration

### Configure Edit Behavior

```csharp
<SfTreeView TValue="MailItem" AllowEditing="true">
    <TreeViewFieldsSettings TValue="MailItem"
        Id="Id"
        Text="FolderName"
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
</SfTreeView>
```

**Edit Configuration:**
- `AllowEditing` - Enable/disable inline node editing (true/false)
- `DoubleClickAction` - Set to `DoubleClickAction.Edit` to enter edit mode on double-click

## Handling Edit Events

### NodeEditing Event

Fires before node enters edit mode. Use for validation or prevention:

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="MailItem" AllowEditing="true">
    <TreeViewFieldsSettings TValue="MailItem" DataSource="@MyFolder" />
    <TreeViewEvents TValue="MailItem" NodeEditing="OnNodeEditing"></TreeViewEvents>
</SfTreeView>

@code {
    void OnNodeEditing(NodeEditEventArgs args)
    {
        var nodeId = args.NodeData.Id;
        var currentText = args.NodeData.Text;
        
        // Prevent editing of certain nodes
        if (nodeId == "protected")
        {
            args.Cancel = true;  // Cancel edit
            return;
        }
        
        Console.WriteLine($"Editing: {currentText}");
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox" },
        new MailItem { Id = "protected", FolderName = "System Folder" }
    };

    public class MailItem
    {
        public string? Id { get; set; }
        public string? FolderName { get; set; }
    }
}
```

### NodeEdited Event

Fires after node edit is confirmed:

```csharp
void OnNodeEdited(NodeEditEventArgs args)
{
    var nodeId = args.NodeData.Id;
    var oldText = args.OldText;
    var newText = args.NewText;
    
    if (oldText != newText)
    {
        Console.WriteLine($"Renamed: {oldText} → {newText}");
        
        // Update database
        UpdateNodeInDatabase(nodeId, newText);
    }
}

<TreeViewEvents TValue="MailItem" NodeEdited="OnNodeEdited"></TreeViewEvents>
```

## Rename Operations

### Basic Rename

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="MailItem" AllowEditing="true">
    <TreeViewFieldsSettings TValue="MailItem"
        Id="Id"
        Text="FolderName"
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
    <TreeViewEvents TValue="MailItem" 
        NodeEditing="OnNodeEditing"
        NodeEdited="OnNodeEdited">
    </TreeViewEvents>
</SfTreeView>

@code {
    void OnNodeEditing(NodeEditEventArgs args)
    {
        // Clear any previous error
        EditError = "";
    }

    void OnNodeEdited(NodeEditEventArgs args)
    {
        if (string.IsNullOrWhiteSpace(args.NewText))
        {
            EditError = "Name cannot be empty";
            args.Cancel = true;
            return;
        }

        if (args.NewText.Length > 50)
        {
            EditError = "Name too long (max 50 characters)";
            args.Cancel = true;
            return;
        }

        // Success - update data
        var node = MyFolder.FirstOrDefault(x => x.Id == args.NodeData.Id);
        if (node != null)
        {
            node.FolderName = args.NewText;
            Console.WriteLine($"Renamed: {args.OldText} → {args.NewText}");
        }
    }

    string EditError = "";

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox" },
        new MailItem { Id = "2", FolderName = "Sent" }
    };

    public class MailItem
    {
        public string? Id { get; set; }
        public string? FolderName { get; set; }
    }
}
```

### Rename on Double-Click vs Context Menu

```csharp
void OnNodeClicked(NodeClickEventArgs args)
{
    if (args.ClickCount == 2)
    {
        // Double-click entered edit mode automatically
        // Just show feedback
        Console.WriteLine("Edit mode activated");
    }
}

// Or provide explicit rename button
<button @onclick="RenameSelectedNode">Rename Node</button>

@code {
    void RenameSelectedNode()
    {
        // Trigger edit via events or API
        // Note: TreeView doesn't have direct "enter edit" method
        // Use NodeEdited event instead
    }
}
```

## Validation During Edit

### Validate Node Text

```csharp
void OnNodeEdited(NodeEditEventArgs args)
{
    var newText = args.NewText;
    
    // Validation 1: Empty text
    if (string.IsNullOrWhiteSpace(newText))
    {
        args.Cancel = true;
        EditErrorMessage = "Name cannot be empty";
        return;
    }
    
    // Validation 2: Duplicate name
    if (MyFolder.Any(x => x.FolderName == newText && x.Id != args.NodeData.Id))
    {
        args.Cancel = true;
        EditErrorMessage = "Name already exists";
        return;
    }
    
    // Validation 3: Invalid characters
    if (newText.ContainsInvalidChars())
    {
        args.Cancel = true;
        EditErrorMessage = "Invalid characters in name";
        return;
    }
    
    // Validation 4: Length check
    if (newText.Length > 100)
    {
        args.Cancel = true;
        EditErrorMessage = "Name too long";
        return;
    }
    
    // All validations passed
    EditErrorMessage = "";
    var node = MyFolder.FirstOrDefault(x => x.Id == args.NodeData.Id);
    if (node != null)
    {
        node.FolderName = newText;
    }
}
```

## Cancel Editing

### Cancel via Event

```csharp
void OnNodeEdited(NodeEditEventArgs args)
{
    if (ShouldRejectChange(args.NewText))
    {
        args.Cancel = true;  // Revert to original text
        return;
    }
    
    // Proceed with change
}
```

### Cancel via Escape Key

Users can press Escape to cancel edit and keep original text

## Edit State Management

### Track Edit State

```csharp
@using Syncfusion.Blazor.Navigations

<div>
    @if (IsEditing)
    {
        <span style="color: orange;">Editing: @EditingNodeId</span>
    }
</div>

<SfTreeView TValue="MailItem" AllowEditing="true">
    <TreeViewFieldsSettings TValue="MailItem" DataSource="@MyFolder" />
    <TreeViewEvents TValue="MailItem"
        NodeEditing="OnNodeEditing"
        NodeEdited="OnNodeEdited">
    </TreeViewEvents>
</SfTreeView>

@code {
    bool IsEditing = false;
    string EditingNodeId = "";

    void OnNodeEditing(NodeEditEventArgs args)
    {
        IsEditing = true;
        EditingNodeId = args.NodeData.Id;
    }

    void OnNodeEdited(NodeEditEventArgs args)
    {
        IsEditing = false;
        EditingNodeId = "";
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox" },
        new MailItem { Id = "2", FolderName = "Sent" }
    };

    public class MailItem
    {
        public string? Id { get; set; }
        public string? FolderName { get; set; }
    }
}
```

## Common Patterns

### Pattern 1: Edit with Server Update

```csharp
async Task OnNodeEdited(NodeEditEventArgs args)
{
    var nodeId = args.NodeData.Id;
    var newName = args.NewText;
    
    try
    {
        // Update on server
        await HttpClient.PutAsJsonAsync(
            $"/api/folders/{nodeId}",
            new { name = newName }
        );
        
        // Update local data
        var node = MyFolder.FirstOrDefault(x => x.Id == nodeId);
        if (node != null)
            node.FolderName = newName;
    }
    catch (Exception ex)
    {
        args.Cancel = true;
        EditError = $"Failed to update: {ex.Message}";
    }
}
```

### Pattern 2: Conditional Edit Permission

```csharp
void OnNodeEditing(NodeEditEventArgs args)
{
    // Check user permissions
    if (!CurrentUser.CanEditNodes)
    {
        args.Cancel = true;
        return;
    }
    
    // Check node permission
    if (!IsNodeEditable(args.NodeData.Id))
    {
        args.Cancel = true;
        return;
    }
}
```

### Pattern 3: Edit with Confirmation Dialog

```csharp
string EditingText = "";
string EditingNodeId = "";
bool ShowConfirmation = false;

void OnNodeEditing(NodeEditEventArgs args)
{
    EditingNodeId = args.NodeData.Id;
    EditingText = args.NodeData.Text;
}

async Task OnNodeEdited(NodeEditEventArgs args)
{
    if (args.NewText == args.OldText)
        return;
    
    // Show confirmation dialog
    ShowConfirmation = true;
    
    var confirmed = await ShowConfirmationDialog(
        $"Rename '{args.OldText}' to '{args.NewText}'?"
    );
    
    if (!confirmed)
    {
        args.Cancel = true;
    }
}
```

## Reset Edit State

### Reset After Failed Edit

```csharp
void OnNodeEdited(NodeEditEventArgs args)
{
    if (ValidationFailed(args.NewText))
    {
        args.Cancel = true;
        
        // Find and reset the node
        var node = MyFolder.FirstOrDefault(x => x.Id == args.NodeData.Id);
        if (node != null)
        {
            node.FolderName = args.OldText;  // Restore original
        }
    }
}
```

## Best Practices

1. **Validate input** before accepting changes
2. **Provide clear feedback** on validation errors
3. **Show loading state** during server updates
4. **Handle cancellation** gracefully (restore original text)
5. **Prevent editing** of system/protected nodes
6. **Update database** after successful edit
7. **Test with edge cases** (empty, duplicates, special chars)

## Troubleshooting

**Issue: Edit mode not activating**
- Ensure `AllowEditing="true"`
- Verify double-click detection
- Check for JavaScript errors in console

**Issue: Changes not saving**
- Verify NodeEdited event is firing
- Check validation isn't canceling edits
- Ensure data source is updated in event handler

**Issue: Validation errors not showing**
- Display errors via component state
- Use toast notifications
- Show inline error messages

## Next Steps

- Use [events-handling.md](events-handling.md) for all edit events
- Implement [node-selection.md](node-selection.md) for selecting before edit
- Check [authorization-authentication.md](authorization-authentication.md) for edit permissions
