---
name: syncfusion-blazor-treeview
description: |
  Implement Syncfusion Blazor TreeView component for hierarchical data display with single/multi-selection, editing, expand-collapse, checkboxes, drag-drop, and virtualization. Use this skill whenever the user needs to display tree-structured data, enable node selection and editing, implement checkboxes for multi-selection, load data on demand, filter and search nodes, customize node appearance, or handle tree interaction events.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Navigation Components"
---

# Implementing Syncfusion Blazor TreeView Component

The Blazor TreeView component displays hierarchical data in an expandable/collapsible tree structure. It supports local and remote data binding, single and multi-selection, editing, checkboxes, drag-drop reordering, virtualization for large datasets, filtering, and comprehensive event handling.

---

## 📋 Table of Contents

1. [When to Use](#when-to-use)
2. [Installation & Setup](#installation--setup)
3. [Quick Start](#quick-start)
4. [Key Properties](#key-properties)
5. [Key Methods](#key-methods)
6. [Key Events](#key-events)
7. [Common Patterns](#common-patterns)
8. [Complete Reference Navigation](#complete-reference-navigation)

---

## When to Use This Skill

Use the TreeView component when you need to:
- **Display hierarchical data** in a tree structure with expandable/collapsible nodes
- **Single selection**: Allow users to select one node from the tree
- **Multi-selection**: Enable selection of multiple tree nodes using Ctrl+Click and Shift+Click
- **Checkbox selection**: Provide checkbox-based multi-selection with automatic parent-child state management
- **Edit nodes**: Allow inline renaming or editing of node text
- **Drag and drop**: Enable reordering nodes within the hierarchy
- **Filter and search**: Implement search functionality to find nodes
- **Remote data sources**: Bind to Web APIs, OData services, or custom endpoints
- **Handle events**: Respond to expand, collapse, select, edit, and drag-drop actions
- **Virtualization**: Display large datasets (1000+ nodes) with smooth scrolling
- **Custom styling**: Apply icons, colors, and templates for nodes

---

## Installation & Setup

Install Syncfusion NuGet packages and configure your Blazor project:

```csharp
// 1. Install NuGet packages
// Install-Package Syncfusion.Blazor.Navigations -Version 26.1.35
// Install-Package Syncfusion.Blazor.Themes -Version 26.1.35

// 2. Add to _Imports.razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Navigations

// 3. Register service in Program.cs
builder.Services.AddSyncfusionBlazor();

// 4. Add CSS theme to Index.html or _Layout.cshtml
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

---

## Quick Start

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="MailItem">
    <TreeViewFieldsSettings TValue="MailItem"
        Id="Id"
        Text="FolderName"
        Child="Children"
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
    <TreeViewEvents TValue="MailItem" NodeSelected="OnNodeSelected"></TreeViewEvents>
</SfTreeView>

@code {
    public class MailItem
    {
        public string? Id { get; set; }
        public string? FolderName { get; set; }
        public List<MailItem>? Children { get; set; }
    }

    void OnNodeSelected(NodeSelectEventArgs args)
    {
        Console.WriteLine($"Selected: {args.NodeData.Text}");
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox", Children = new() },
        new MailItem { Id = "2", FolderName = "Sent", Children = new() }
    };
}
```

---

## Key Properties

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `AllowDragAndDrop` | bool | false | Enable/disable drag-drop hierarchy reordering |
| `AllowEditing` | bool | false | Allow double-click node renaming |
| `AllowMultiSelection` | bool | false | Enable Ctrl+Click multi-selection |
| `ShowCheckBox` | bool | false | Display checkboxes for each node |
| `AutoCheck` | bool | true | Auto-check/uncheck children when parent checked |
| `EnablePersistence` | bool | false | Persist expanded/selected/checked state to localStorage |
| `EnableVirtualization` | bool | false | Virtual scrolling for 1000+ nodes (requires Height) |
| `ExpandedNodes` | string[] | Empty | Initially expanded node IDs (2-way bindable) |
| `SelectedNodes` | string[] | Empty | Selected node IDs (2-way bindable) |
| `CheckedNodes` | string[] | Empty | Checked node IDs (2-way bindable) |
| `LoadOnDemand` | bool | true | Load children only when node expands |
| `ExpandOn` | ExpandAction | Click | Trigger expand on Click/DoubleClick/None |
| `Height` | string | "auto" | Fixed height (required for virtualization) |

---

## Key Methods

| Method | Purpose |
|--------|---------|
| `ExpandAllAsync()` | Expand all nodes |
| `ExpandAllAsync(string[] nodeIds)` | Expand specific nodes by ID |
| `CollapseAllAsync()` | Collapse all nodes |
| `CollapseAllAsync(string[] nodeIds)` | Collapse specific nodes |
| `BeginEditAsync(string nodeId)` | Enter edit mode for a node |
| `GetTreeData()` | Get all tree data |
| `GetTreeData(string nodeId)` | Get specific node data by ID |
| `EnsureVisibleAsync(string nodeId)` | Scroll to make node visible |
| `CheckAllAsync()` | Check all checkboxes |
| `UncheckAllAsync()` | Uncheck all checkboxes |
| `ClearStateAsync()` | Clear all state (selection, expand, check) |

---

## Key Events

| Event | Fires When | Common Uses |
|-------|-----------|---------|
| `Created` | TreeView initialized | Post-initialization setup, load preferences |
| `DataBound` | Data binding complete | Auto-expand default nodes, validate data |
| `NodeSelected` | Node left-clicked | Load node details, enable actions |
| `NodeClicked` | Node clicked | Distinguish single vs double-click |
| `NodeExpanded` | Node expanded | Load child nodes (load-on-demand) |
| `NodeCollapsed` | Node collapsed | Optional: Unload children from memory |
| `NodeEditing` | Before edit mode | Validate permissions, prevent edits |
| `NodeEdited` | Edit confirmed | Validate new text, save to server |
| `OnNodeDragStart` | Drag begins | Prevent dragging restricted nodes |
| `NodeDropped` | Drop completed | Update hierarchy in server |
| `NodeChecking` | Before checkbox changes | Prevent checking restricted nodes |
| `NodeChecked` | Checkbox changed | Update related data, trigger actions |
| `DataSourceChanged` | Data source updated | Re-apply filters, refresh calculations |
| `OnActionFailure` | Action fails (API error) | Recover from errors, show notifications |
| `OnKeyPress` | Key pressed | Implement keyboard shortcuts (Delete, F2, etc) |

---

## Common Patterns

### Pattern 1: Basic Selection
```csharp
<SfTreeView TValue="Item" @bind-SelectedNodes="@SelectedIds">
    <TreeViewFieldsSettings TValue="Item" DataSource="@Items" />
    <TreeViewEvents TValue="Item" NodeSelected="OnSelect"></TreeViewEvents>
</SfTreeView>

@code {
    string[] SelectedIds = Array.Empty<string>();
    void OnSelect(NodeSelectEventArgs args) => Console.WriteLine(args.NodeData.Text);
}
```

### Pattern 2: Multiple Selection
```csharp
<SfTreeView TValue="Item" AllowMultiSelection="true" @bind-SelectedNodes="@SelectedIds">
    <TreeViewFieldsSettings TValue="Item" DataSource="@Items" />
</SfTreeView>
```

### Pattern 3: Load on Demand
```csharp
void OnNodeExpanded(NodeExpandEventArgs args)
{
    if (args.NodeData.HasChild && args.NodeData.Child == null)
    {
        // Load children from API
        args.NodeData.Child = await FetchChildren(args.NodeData.Id);
    }
}
```

### Pattern 4: Drag and Drop
```csharp
<SfTreeView TValue="Item" AllowDragAndDrop="true">
    <TreeViewFieldsSettings TValue="Item" DataSource="@Items" />
    <TreeViewEvents TValue="Item" NodeDropped="OnDropped"></TreeViewEvents>
</SfTreeView>

void OnDropped(DragAndDropEventArgs args) => UpdateHierarchy(args);
```

### Pattern 5: Node Editing
```csharp
<SfTreeView TValue="Item" AllowEditing="true" DoubleClickAction="DoubleClickAction.Edit">
    <TreeViewFieldsSettings TValue="Item" DataSource="@Items" />
    <TreeViewEvents TValue="Item" NodeEdited="OnEdited"></TreeViewEvents>
</SfTreeView>

void OnEdited(NodeEditEventArgs args) => SaveChanges(args.NodeData);
```

### Pattern 6: Checkboxes
```csharp
<SfTreeView TValue="Item" AllowCheckBoxes="true" ChildChecking="ChildCheckState.Both">
    <TreeViewFieldsSettings TValue="Item" DataSource="@Items" />
</SfTreeView>

var checked = treeRef.GetAllCheckedNodes();
```

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](./references/getting-started.md)
- Installation and NuGet package setup
- Project configuration by type (WebAssembly, Server, Web App, MAUI)
- CSS theme configuration
- Service registration and first TreeView component

### Data Binding and Sources
📄 **Read:** [references/data-binding.md](./references/data-binding.md)
- Local data binding (hierarchical and self-referential structures)
- Remote data with OData, OData V4, and Web API adaptors
- Load on Demand for large datasets
- TreeViewFieldsSettings property mappings
- DataBound event for post-binding operations

### Node Selection
📄 **Read:** [references/node-selection.md](./references/node-selection.md)
- Single node selection (default behavior)
- Multi-selection with AllowMultiSelection property
- Accessing selected node data via NodeSelected event
- Programmatic selection using SelectedNodes binding
- Selection validation and conditional selection

### Expand and Collapse Actions
📄 **Read:** [references/expand-collapse-actions.md](./references/expand-collapse-actions.md)
- Expand/collapse methods (ExpandAllAsync, CollapseAllAsync)
- ExpandedNodes two-way binding for programmatic control
- Initial expand state via data source
- Expand/collapse animations with TreeViewNodeAnimationSettings
- Load-on-demand child node loading via NodeExpanded event

### Node Editing
📄 **Read:** [references/node-editing.md](./references/node-editing.md)
- Enable editing with AllowEditing property
- Double-click to enter edit mode
- BeginEditAsync method for programmatic edit entry
- NodeEditing and NodeEdited events for validation
- Rename operations with conflict detection

### Checkbox Features
📄 **Read:** [references/checkbox-features.md](./references/checkbox-features.md)
- ShowCheckBox property for multi-item selection
- AutoCheck for automatic parent-child synchronization
- CheckedNodes two-way binding for programmatic control
- Getting checked nodes with filtering and iteration
- Permissions and role-based checkbox patterns

### Events and Callbacks
📄 **Read:** [references/events-handling.md](./references/events-handling.md)
- Lifecycle events (Created, DataBound)
- Selection events (NodeSelected, SelectedNodesChanged)
- Expand/collapse events (NodeExpanded, NodeCollapsed)
- Edit events (NodeEditing, NodeEdited)
- Checkbox events (NodeChecking, NodeChecked)
- Drag-drop events (OnNodeDragStart, NodeDropped)
- Keyboard shortcuts (Enter, Delete, F2, Arrow keys)

### Advanced Features
📄 **Read:** [references/advanced-features.md](./references/advanced-features.md)
- Drag-and-drop with hierarchy reordering
- UI virtualization for 1000+ nodes
- Search and filtering functionality
- Sorting (Ascending, Descending, None)
- Performance optimization techniques

### Customization and Styling
📄 **Read:** [references/customization-styling.md](./references/customization-styling.md)
- Icon customization (expand/collapse, node icons)
- Dynamic icons based on data
- Text wrapping and display formatting
- CSS styling with e-icons classes
- Theme support and responsive design

### Authorization and Security
📄 **Read:** [references/authorization-authentication.md](./references/authorization-authentication.md)
- Authentication setup with AuthorizeView
- Role-based authorization and permissions
- Node-level access control
- Claims-based authorization patterns
- Securing edit operations and drag-drop

### Navigation Patterns
📄 **Read:** [references/navigation-patterns.md](./references/navigation-patterns.md)
- Node traversal methods (parent, children, siblings)
- Breadcrumb navigation implementation
- NavigateUrl property for node links
- Parent-child navigation relationships
- Deep-linking to specific nodes

---

## Quick Links and Real-World Examples

**Need help?** Start with:
1. [Quick Start](#quick-start) - Get running in 5 minutes
2. [Common Patterns](#common-patterns) - Copy-paste patterns for your use case
3. [Key Properties](#key-properties) - Find property details
4. [data-binding.md](./references/data-binding.md) - Learn data binding approaches
5. [events-handling.md](./references/events-handling.md) - Understand all events

**Real-world implementations:**
- **File Browser**: Use hierarchical data + expand-collapse + icons + drag-drop
- **Organization Chart**: Use data-binding + templates + multi-level navigation
- **Navigation Menu**: Use hierarchical data + keyboard navigation + load-on-demand
- **Category Filter**: Use self-referential data + checkboxes + filtering
- **Permissions UI**: Use checkboxes + AutoCheck + role-based authorization

---

## Summary

This skill provides comprehensive guidance for implementing the Syncfusion Blazor TreeView component. Use the **Documentation and Navigation Guide** section above to find the specific reference file you need based on your use case.
