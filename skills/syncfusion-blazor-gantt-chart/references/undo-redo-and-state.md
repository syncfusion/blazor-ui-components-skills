# Undo, Redo, and State Management

## Table of Contents
- [Overview](#overview)
- [When to Read](#when-to-read)
- [Key Properties and Features](#key-properties-and-features)
- [Enable Undo and Redo](#enable-undo-and-redo)
- [Supported Undo/Redo Actions](#supported-undoredo-actions)
- [Configure Undo/Redo Actions](#configure-undoredo-actions)
- [Configure Maximum Undo/Redo Steps](#configure-maximum-undoredo-steps)
- [Programmatic Undo/Redo](#programmatic-undoredo)
- [Undo/Redo Event](#undoredo-event)
- [State Management](#state-management)
- [Server-Side Considerations](#server-side-considerations)
- [Best Practices](#best-practices)
- [Common Scenarios](#common-scenarios)
- [Troubleshooting](#troubleshooting)
- [Limitations](#limitations)

---

## Overview

The Undo and Redo features in the Syncfusion Blazor Gantt Chart component enable users to revert or reapply recent changes, improving editing safety and confidence. State management helps preserve user context such as filters, selection, expansion state, and zoom levels across interactions and data refreshes.

## When to Read
- You need to enable undo/redo functionality for editing operations
- You want to configure which actions are undoable
- You need to control the number of undo/redo steps stored
- You want to trigger undo/redo programmatically
- You need to preserve view state (filters, selection, expansion, zoom)
- You're implementing server-side data with client-side undo/redo

## Key Properties and Features

### Undo/Redo Properties
- `EnableUndoRedo` — Enables undo/redo functionality (default: false)
- `UndoRedoActions` — List of actions to track for undo/redo
- `MaxUndoRedoSteps` — Maximum number of undo/redo steps (default: 20)
- `UndoAsync()` — Method to perform undo programmatically
- `RedoAsync()` — Method to perform redo programmatically

### Events
- `OnUndoRedo` — Triggered after undo or redo operation completes

### Keyboard Shortcuts
- `Ctrl + Z` — Undo last action
- `Ctrl + Y` — Redo last undone action

## Enable Undo and Redo

Enable undo/redo by setting `EnableUndoRedo` to true:

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt DataSource="@TaskCollection" 
         Height="500px" 
         Width="100%" 
         EnableUndoRedo="true"
         UndoRedoActions="@UndoRedoActionsList"
         Toolbar="@ToolbarItems"
         TreeColumnIndex="1" 
         EnableContextMenu="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" 
                     EndDate="EndDate" Duration="Duration" Progress="Progress"
                     Dependency="Predecessor" ParentID="ParentID">
    </GanttTaskFields>
    <GanttEditSettings AllowAdding="true" AllowDeleting="true" 
                       AllowEditing="true" AllowTaskbarEditing="true">
    </GanttEditSettings>
</SfGantt>

@code {
    private List<TaskData> TaskCollection { get; set; }
    
    private List<string> ToolbarItems = new List<string>() 
    { 
        "Add", "Edit", "Delete", "Undo", "Redo", "ZoomIn", "ZoomOut", "ZoomToFit" 
    };
    
    private List<GanttUndoRedoAction> UndoRedoActionsList = new List<GanttUndoRedoAction> 
    {
        GanttUndoRedoAction.Add,
        GanttUndoRedoAction.Edit,
        GanttUndoRedoAction.Delete,
        GanttUndoRedoAction.TaskbarEdit,
        GanttUndoRedoAction.Indent,
        GanttUndoRedoAction.Outdent,
        GanttUndoRedoAction.RowDragAndDrop
    };

    protected override void OnInitialized()
    {
        TaskCollection = GetTaskCollection();
    }

    public class TaskData
    {
        public int TaskID { get; set; }
        public string TaskName { get; set; }
        public DateTime? StartDate { get; set; }
        public DateTime? EndDate { get; set; }
        public string Duration { get; set; }
        public int Progress { get; set; }
        public string Predecessor { get; set; }
        public int? ParentID { get; set; }
    }

    public static List<TaskData> GetTaskCollection()
    {
        return new List<TaskData>
        {
            new TaskData { TaskID = 1, TaskName = "Project initiation", 
                          StartDate = new DateTime(2025, 11, 01), Duration = "2", Progress = 100 },
            new TaskData { TaskID = 2, TaskName = "Identify site location", 
                          StartDate = new DateTime(2025, 11, 01), Duration = "3", Progress = 100, ParentID = 1 },
            new TaskData { TaskID = 3, TaskName = "Site analyze", 
                          StartDate = new DateTime(2025, 11, 02), Duration = "2", Progress = 90, ParentID = 1 }
        };
    }
}
```

> **Important**: Only actions defined in `UndoRedoActions` property will be recorded in the undo/redo history.

## Supported Undo/Redo Actions

The following table lists all built-in actions that can be tracked:

| Action | Description |
|--------|-------------|
| `Edit` | Restores changes made during record edits (cell or dialog) |
| `Delete` | Restores deleted records |
| `Add` | Restores newly added records |
| `ColumnReorder` | Restores column reorder operations |
| `Indent` | Restores indent operations on records |
| `Outdent` | Restores outdent operations on records |
| `ColumnResize` | Restores column width changes |
| `Sort` | Restores column sorting changes |
| `Filter` | Restores applied or cleared filters |
| `Search` | Restores applied or cleared search text |
| `ZoomIn` | Restores zoom-in actions on the timeline |
| `ZoomOut` | Restores zoom-out actions on the timeline |
| `ZoomToFit` | Restores zoom-to-fit actions on the timeline |
| `ColumnState` | Restores show/hide column visibility changes |
| `RowDragAndDrop` | Restores row drag-and-drop reorder operations |
| `TaskbarDragAndDrop` | Restores taskbar drag-and-drop operations |
| `PreviousTimeSpan` | Restores navigation to the previous timespan |
| `NextTimeSpan` | Restores navigation to the next timespan |
| `SplitterResize` | Restores splitter position changes |
| `ColumnFreeze` | Restores column freeze or unfreeze changes |
| `TaskbarEdit` | Restores taskbar edits (move, resize, progress, connectors) |
| `Expand` | Restores expand state changes on records |
| `Collapse` | Restores collapse state changes on records |

## Configure Undo/Redo Actions

Restrict which actions are tracked by customizing the `UndoRedoActions` list:

```csharp
// Track only essential edit operations
private List<GanttUndoRedoAction> UndoRedoActionsList = new List<GanttUndoRedoAction> 
{
    GanttUndoRedoAction.Add,
    GanttUndoRedoAction.Edit,
    GanttUndoRedoAction.Delete,
    GanttUndoRedoAction.TaskbarEdit
};
```

```csharp
// Track comprehensive set of actions
private List<GanttUndoRedoAction> UndoRedoActionsList = new List<GanttUndoRedoAction> 
{
    GanttUndoRedoAction.Sort,
    GanttUndoRedoAction.Add,
    GanttUndoRedoAction.ColumnReorder,
    GanttUndoRedoAction.TaskbarEdit,
    GanttUndoRedoAction.ColumnState,
    GanttUndoRedoAction.Edit,
    GanttUndoRedoAction.Filter,
    GanttUndoRedoAction.NextTimeSpan,
    GanttUndoRedoAction.PreviousTimeSpan,
    GanttUndoRedoAction.Search,
    GanttUndoRedoAction.Delete,
    GanttUndoRedoAction.ZoomIn,
    GanttUndoRedoAction.ZoomOut,
    GanttUndoRedoAction.ZoomToFit,
    GanttUndoRedoAction.Collapse,
    GanttUndoRedoAction.Expand,
    GanttUndoRedoAction.SplitterResize,
    GanttUndoRedoAction.Indent,
    GanttUndoRedoAction.Outdent,
    GanttUndoRedoAction.RowDragAndDrop
};
```

## Configure Maximum Undo/Redo Steps

Limit the number of undo/redo actions stored in history:

```razor
<SfGantt DataSource="@TaskCollection" 
         EnableUndoRedo="true"
         MaxUndoRedoSteps="5"
         UndoRedoActions="@UndoRedoActionsList">
</SfGantt>
```

**Default**: 20 steps

**Behavior**: When the count exceeds `MaxUndoRedoSteps`, the oldest entry is discarded and the newest action is appended.

**Considerations**:
- **Higher values**: More memory usage, longer undo history
- **Lower values**: Less memory usage, shorter undo history
- **Very low values** (1-3): May frustrate users with limited undo capability
- **Recommended**: 10-30 for typical use cases

## Programmatic Undo/Redo

Trigger undo/redo operations via methods instead of toolbar buttons:

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Buttons

<div style="margin-bottom: 16px;">
    <SfButton OnClick="UndoHandler">Undo</SfButton>
    <SfButton OnClick="RedoHandler">Redo</SfButton>
</div>

<SfGantt @ref="GanttInstance" 
         DataSource="@TaskCollection" 
         Height="500px" 
         EnableUndoRedo="true"
         UndoRedoActions="@UndoRedoActionsList">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" 
                     EndDate="EndDate" Duration="Duration" Progress="Progress" />
    <GanttEditSettings AllowAdding="true" AllowDeleting="true" 
                       AllowEditing="true" AllowTaskbarEditing="true" />
</SfGantt>

@code {
    private SfGantt<TaskData> GanttInstance;

    private async Task UndoHandler()
    {
        await GanttInstance.UndoAsync();
    }

    private async Task RedoHandler()
    {
        await GanttInstance.RedoAsync();
    }
}
```

## Undo/Redo Event

Handle the `OnUndoRedo` event to execute custom logic after undo/redo operations:

```razor
<SfGantt DataSource="@TaskCollection" 
         EnableUndoRedo="true">
    <GanttEvents OnUndoRedo="OnUndoRedoHandler" TValue="TaskData" />
</SfGantt>

@code {
    private void OnUndoRedoHandler(UndoRedoEventArgs args)
    {
        if (args.Action == "Undo")
        {
            Console.WriteLine($"Undo performed: {args.Operation}");
            // Log undo operation
            // Update UI indicators
        }
        else if (args.Action == "Redo")
        {
            Console.WriteLine($"Redo performed: {args.Operation}");
            // Log redo operation
        }
    }
}
```

## State Management

Beyond undo/redo history, preserving view state ensures consistent user experience across data refreshes and interactions.

### State Components to Preserve

| State Type | Description | Example Properties |
|------------|-------------|-------------------|
| **Filters** | Active column filters | FilterSettings, FilteredRecords |
| **Selection** | Selected rows/cells | SelectedRowIndexes, SelectedRecords |
| **Expansion** | Expanded/collapsed rows | ExpandedRowIndexes |
| **Sort Order** | Column sorting state | SortedColumns, SortDirection |
| **Zoom Level** | Timeline zoom | ZoomingLevels, CurrentZoomLevel |
| **Splitter Position** | Grid/chart split | SplitterSettings.Position |
| **Column State** | Widths, order, visibility | ColumnState, ColumnOrder |
| **Scroll Position** | Vertical/horizontal scroll | ScrollTop, ScrollLeft |

### Preserve Expanded State

```csharp
private List<int> expandedRowIndexes = new List<int>();

// Store expanded state before refresh
private async Task BeforeRefresh()
{
    expandedRowIndexes = await GanttInstance.GetExpandedRowIndexes();
}

// Restore expanded state after refresh
private async Task AfterRefresh()
{
    await GanttInstance.ExpandByIndexesAsync(expandedRowIndexes);
}
```

### Preserve Selection State

```csharp
private List<int> selectedRowIndexes = new List<int>();

// Store selection before refresh
private async Task StoreSelection()
{
    selectedRowIndexes = await GanttInstance.GetSelectedRowIndexes();
}

// Restore selection after refresh
private async Task RestoreSelection()
{
    await GanttInstance.SelectRowsAsync(selectedRowIndexes);
}
```

### Preserve Filter State

```csharp
private GanttFilterSettings storedFilters;

// Store filter state
private void StoreFilters()
{
    storedFilters = GanttInstance.FilterSettings;
}

// Reapply filters after refresh
private async Task RestoreFilters()
{
    if (storedFilters != null)
    {
        await GanttInstance.FilterByColumnAsync(
            storedFilters.Columns.First().Field,
            storedFilters.Columns.First().Operator,
            storedFilters.Columns.First().Value
        );
    }
}
```

### Preserve Zoom Level

```csharp
private string currentZoomLevel;

// Store zoom level
private void StoreZoom()
{
    currentZoomLevel = GanttInstance.CurrentZoomingLevel.Level;
}

// Restore zoom level
private async Task RestoreZoom()
{
    await GanttInstance.ZoomToLevel(currentZoomLevel);
}
```

### Complete State Preservation Pattern

```csharp
public class GanttViewState
{
    public List<int> ExpandedRows { get; set; }
    public List<int> SelectedRows { get; set; }
    public string ZoomLevel { get; set; }
    public string FilterState { get; set; }
    public string SortState { get; set; }
    public int ScrollTop { get; set; }
    public int ScrollLeft { get; set; }
}

private GanttViewState viewState = new GanttViewState();

// Capture complete state
private async Task CaptureState()
{
    viewState.ExpandedRows = await GanttInstance.GetExpandedRowIndexes();
    viewState.SelectedRows = await GanttInstance.GetSelectedRowIndexes();
    viewState.ZoomLevel = GanttInstance.CurrentZoomingLevel.Level;
    // Store other state properties
}

// Restore complete state
private async Task RestoreState()
{
    await GanttInstance.ExpandByIndexesAsync(viewState.ExpandedRows);
    await GanttInstance.SelectRowsAsync(viewState.SelectedRows);
    await GanttInstance.ZoomToLevel(viewState.ZoomLevel);
    // Restore other state properties
}
```

## Server-Side Considerations

### Client-Side Undo/Redo
When undo/redo is purely client-side:
- Commands are deterministic
- State is maintained in browser
- Works until explicit save/sync

```razor
<SfGantt DataSource="@TaskCollection" 
         EnableUndoRedo="true"
         AutoCommit="false">  <!-- Don't auto-save to server -->
</SfGantt>

<SfButton OnClick="SaveChanges">Save All Changes</SfButton>
```

### Server-Side with Undo/Redo
When changes are persisted immediately:
- Undo must send compensating updates to server
- Server must return normalized records
- Consider using batch operations

```csharp
public async Task OnUndoRedoHandler(UndoRedoEventArgs args)
{
    if (args.Action == "Undo" || args.Action == "Redo")
    {
        // Sync undo/redo action to server
        await SyncToServer(args.ModifiedRecords);
    }
}
```

### Batch Operations
Group related changes for better undo/redo behavior:

```csharp
// Start batch update
await GanttInstance.BatchUpdate();

// Make multiple changes
await GanttInstance.UpdateRecordByIDAsync(taskId1, updatedData1);
await GanttInstance.UpdateRecordByIDAsync(taskId2, updatedData2);

// End batch - this creates single undo step
await GanttInstance.EndBatchUpdate();
```

## Best Practices

### Configuration
- **Include essential actions**: Add, Edit, Delete, TaskbarEdit
- **Consider user workflow**: Track actions users perform frequently
- **Balance history size**: Use `MaxUndoRedoSteps` between 10-30
- **Consistent UI**: Keep undo/redo buttons visible when enabled

### User Experience
- **Provide visual feedback**: Show success messages after undo/redo
- **Keyboard shortcuts**: Ensure Ctrl+Z and Ctrl+Y work consistently
- **Clear indicators**: Show undo/redo availability (enabled/disabled state)
- **Predictable behavior**: Make undo/redo behavior intuitive and expected

### Performance
- **Limit tracked actions**: Don't track every possible action
- **Optimize large datasets**: Consider selective undo for bulk operations
- **Memory management**: Use reasonable `MaxUndoRedoSteps` values
- **Batch operations**: Group related changes into single undo steps

### Development
- **Separate data from UI state**: Keep task data separate from view state
- **Restore user context**: Preserve expansion, selection, filters after refreshes
- **Test edge cases**: Verify undo/redo with complex dependency chains
- **Handle server sync**: Plan undo strategy for server-persisted data

## Common Scenarios

### Undo After Bulk Delete

```csharp
public async Task BulkDeleteWithUndo()
{
    var selectedRecords = await GanttInstance.GetSelectedRecords();
    
    foreach (var record in selectedRecords)
    {
        await GanttInstance.DeleteRecordAsync(record);
    }
    
    // User can undo to restore all deleted tasks
}
```

### Conditional Undo/Redo

```csharp
private bool canUndo = false;
private bool canRedo = false;

public void OnUndoRedoHandler(UndoRedoEventArgs args)
{
    // Update button states based on availability
    canUndo = args.CanUndo;
    canRedo = args.CanRedo;
    StateHasChanged();
}
```

### Undo with Confirmation

```csharp
private async Task UndoWithConfirmation()
{
    bool confirmed = await ShowConfirmDialog("Undo last action?");
    
    if (confirmed)
    {
        await GanttInstance.UndoAsync();
    }
}
```

### Track Undo History

```csharp
private List<string> undoHistory = new List<string>();

public void OnUndoRedoHandler(UndoRedoEventArgs args)
{
    if (args.Action == "Undo")
    {
        undoHistory.Add($"Undid: {args.Operation} at {DateTime.Now}");
    }
    else if (args.Action == "Redo")
    {
        undoHistory.Add($"Redid: {args.Operation} at {DateTime.Now}");
    }
}
```

## Troubleshooting

**Issue**: Undo does not restore previous view  
**Solution**: Data may be restored while UI state (expansion, selection) is not. Implement state preservation logic.

**Issue**: Undo/Redo buttons are disabled  
**Solution**: 
- Verify `EnableUndoRedo="true"` is set
- Check that actions are defined in `UndoRedoActions` list
- Ensure toolbar includes "Undo" and "Redo" items

**Issue**: Remote updates make redo unreliable  
**Solution**: Server may be transforming data. Ensure server returns canonical values after updates.

**Issue**: Undo clears selection  
**Solution**: Store selected row indexes before undo, restore after:
```csharp
var selection = await GanttInstance.GetSelectedRowIndexes();
await GanttInstance.UndoAsync();
await GanttInstance.SelectRowsAsync(selection);
```

**Issue**: Too many undo steps causing performance issues  
**Solution**: Reduce `MaxUndoRedoSteps` to a lower value (e.g., 10)

**Issue**: Undo doesn't work after data refresh  
**Solution**: Undo history is cleared on data refresh. Consider implementing custom undo logic or warning users before refresh.

**Issue**: Keyboard shortcuts (Ctrl+Z) not working  
**Solution**: 
- Verify `AllowKeyboard="true"` is set
- Check that Gantt component has focus
- Ensure no other components are intercepting keyboard events

## Limitations

- **Client-side only**: Undo/redo history is not persisted to server
- **Session-based**: History is cleared on page refresh
- **Memory bound**: Large undo stacks consume memory
- **Complex dependencies**: Undo may not fully restore complex dependency chains
- **Cross-component**: Undo doesn't track changes in other components
- **Real-time collaboration**: May conflict with concurrent edits from other users

---
