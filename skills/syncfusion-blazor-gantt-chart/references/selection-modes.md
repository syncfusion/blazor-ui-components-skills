# Selection Modes

## Table of Contents
- [Overview](#overview)
- [When to Read](#when-to-read)
- [Key Properties and Features](#key-properties-and-features)
- [Basic Selection Setup](#basic-selection-setup)
- [Selection Mode](#selection-mode)
- [Selection Type](#selection-type)
- [Cell Selection Mode](#cell-selection-mode)
- [Toggle Selection](#toggle-selection)
- [Drag Selection](#drag-selection)
- [Programmatic Selection](#programmatic-selection)
- [Get Selected Data](#get-selected-data)
- [Selection Events](#selection-events)
- [Custom Selection Styling](#custom-selection-styling)
- [Keyboard Navigation](#keyboard-navigation)
- [Touch Interaction](#touch-interaction)
- [Best Practices](#best-practices)
- [Common Scenarios](#common-scenarios)
- [Integration with Other Features](#integration-with-other-features)
- [Troubleshooting](#troubleshooting)

---

## Overview

The Selection feature in the Syncfusion Blazor Gantt Chart component provides the ability to highlight rows or cells, enabling users to interact with and identify specific tasks. Selection can be performed using mouse clicks, arrow keys, or programmatically via methods.

## When to Read
- You need to enable row or cell selection in your Gantt Chart
- You want to configure single or multiple selection modes
- You need to handle selection events for custom behavior
- You require programmatic selection control
- You want to implement drag selection or toggle selection

## Key Properties and Features

### Selection Properties
- `AllowSelection` — Enables/disables selection (default: true)
- `GanttSelectionSettings.Mode` — Selection mode: Row, Cell, or Both
- `GanttSelectionSettings.Type` — Selection type: Single or Multiple
- `GanttSelectionSettings.EnableToggle` — Enable toggle selection on repeated clicks
- `GanttSelectionSettings.AllowDragSelection` — Enable mouse/touch drag selection
- `GanttSelectionSettings.CellSelectionMode` — Cell selection behavior: Flow, Box, or BoxWithBorder

### Methods
- `SelectRowAsync(index)` — Select specific row by index
- `SelectRowsAsync(indexes)` — Select multiple rows
- `SelectCellAsync(cellIndex)` — Select specific cell
- `SelectCellsAsync(cellIndexes)` — Select multiple cells
- `ClearSelectionAsync()` — Clear all selections
- `GetSelectedRecords()` — Get selected row data
- `GetSelectedRowIndexes()` — Get selected row indices

### Events
- `RowSelecting` — Triggered before row selection (cancellable)
- `RowSelected` — Triggered after row is selected
- `RowDeselecting` — Triggered before row deselection (cancellable)
- `RowDeselected` — Triggered after row is deselected
- `CellSelecting` — Triggered before cell selection (cancellable)
- `CellSelected` — Triggered after cell is selected
- `CellDeselecting` — Triggered before cell deselection (cancellable)
- `CellDeselected` — Triggered after cell is deselected

## Basic Selection Setup

Selection is enabled by default. To explicitly control it:

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt DataSource="@TaskCollection" Height="450px" Width="700px" AllowSelection="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" 
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
</SfGantt>

@code {
    private List<TaskData> TaskCollection { get; set; }

    protected override void OnInitialized()
    {
        TaskCollection = GetTaskCollection();
    }

    public class TaskData
    {
        public int TaskID { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public DateTime? EndDate { get; set; }
        public string Duration { get; set; }
        public int Progress { get; set; }
        public int? ParentID { get; set; }
    }

    public static List<TaskData> GetTaskCollection()
    {
        return new List<TaskData>()
        {
            new TaskData() { TaskID = 1, TaskName = "Project initiation", StartDate = new DateTime(2022, 04, 05), EndDate = new DateTime(2022, 04, 08) },
            new TaskData() { TaskID = 2, TaskName = "Identify Site location", StartDate = new DateTime(2022, 04, 05), Duration = "0", Progress = 30, ParentID = 1 },
            new TaskData() { TaskID = 3, TaskName = "Perform soil test", StartDate = new DateTime(2022, 04, 05), Duration = "4", Progress = 40, ParentID = 1 },
            new TaskData() { TaskID = 4, TaskName = "Soil test approval", StartDate = new DateTime(2022, 04, 05), Duration = "0", Progress = 30, ParentID = 1 }
        };
    }
}
```

## Selection Mode

The `Mode` property controls what can be selected:

| Mode | Description |
|------|-------------|
| `Row` | Select entire rows (default) |
| `Cell` | Select individual cells |
| `Both` | Allow both row and cell selection |

### Row Selection Mode

```razor
<SfGantt DataSource="@TaskCollection">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttSelectionSettings Mode="Syncfusion.Blazor.Grids.SelectionMode.Row" />
</SfGantt>
```

### Cell Selection Mode

```razor
<SfGantt DataSource="@TaskCollection">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttSelectionSettings Mode="Syncfusion.Blazor.Grids.SelectionMode.Cell" />
</SfGantt>
```

### Both Mode

```razor
<SfGantt DataSource="@TaskCollection">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttSelectionSettings Mode="Syncfusion.Blazor.Grids.SelectionMode.Both" />
</SfGantt>
```

### Dynamic Mode Selection

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Grids
@using Syncfusion.Blazor.DropDowns

<div style="margin-bottom:5px">
    <label style="padding: 30px 2px 0 0; font-weight:bold"> Choose selection mode: </label>
    <SfDropDownList TValue="SelectionMode" TItem="DropDownOrder" @bind-Value="@SelectionModeValue" 
                    DataSource="@DropDownData" Width="100px">
        <DropDownListFieldSettings Text="Text" Value="Value" />
    </SfDropDownList>
</div>

<SfGantt DataSource="@TaskCollection" Height="450px" Width="700px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttSelectionSettings Mode="@SelectionModeValue" />
</SfGantt>

@code {
    public SelectionMode SelectionModeValue { get; set; } = SelectionMode.Both;

    public class DropDownOrder
    {
        public string Text { get; set; }
        public SelectionMode Value { get; set; }
    }

    List<DropDownOrder> DropDownData = new List<DropDownOrder>
    {
        new DropDownOrder() { Text = "Both", Value = SelectionMode.Both },
        new DropDownOrder() { Text = "Row", Value = SelectionMode.Row },
        new DropDownOrder() { Text = "Cell", Value = SelectionMode.Cell }
    };
}
```

## Selection Type

The `Type` property controls how many items can be selected:

| Type | Description |
|------|-------------|
| `Single` | Select only one row/cell at a time (default) |
| `Multiple` | Select multiple rows/cells with CTRL/CMD + Click |

### Single Selection

```razor
<SfGantt DataSource="@TaskCollection">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttSelectionSettings Type="Syncfusion.Blazor.Grids.SelectionType.Single" />
</SfGantt>
```

### Multiple Selection

```razor
<SfGantt DataSource="@TaskCollection">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttSelectionSettings Mode="Syncfusion.Blazor.Grids.SelectionMode.Row" 
                            Type="Syncfusion.Blazor.Grids.SelectionType.Multiple" />
</SfGantt>
```

**User Interaction:**
- Hold **CTRL** (Windows/Linux) or **CMD** (macOS) while clicking to add to selection
- Click without modifier key to replace selection

## Cell Selection Mode

When `Mode="Cell"`, the `CellSelectionMode` property controls cell selection behavior:

| Mode | Description |
|------|-------------|
| `Flow` | Continuous cell-by-cell selection |
| `Box` | Rectangular selection region |
| `BoxWithBorder` | Box selection with visual border |

```razor
<SfGantt DataSource="@TaskCollection">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttSelectionSettings Mode="Syncfusion.Blazor.Grids.SelectionMode.Cell" 
                            Type="Syncfusion.Blazor.Grids.SelectionType.Multiple"
                            CellSelectionMode="Syncfusion.Blazor.Grids.CellSelectionMode.Box" />
</SfGantt>
```

## Toggle Selection

Toggle selection allows selecting and deselecting with repeated clicks:

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Buttons

<div style="padding-bottom:20px">
    <SfButton @onclick="ToggleSelection">@buttonText</SfButton>
</div>

<SfGantt @ref="Gantt" DataSource="@TaskCollection" Height="450px" Width="700px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttSelectionSettings Mode="Syncfusion.Blazor.Grids.SelectionMode.Row" 
                            Type="Syncfusion.Blazor.Grids.SelectionType.Multiple" 
                            EnableToggle="@toggle" />
</SfGantt>

@code {
    public SfGantt<TaskData> Gantt;
    private bool toggle = true;
    private string buttonText = "Disable Toggle";

    private void ToggleSelection()
    {
        toggle = !toggle;
        buttonText = toggle ? "Disable Toggle" : "Enable Toggle";
    }
}
```

**Behavior:**
- Click selected row/cell → deselects it
- Click deselected row/cell → selects it
- Works with both single and multiple selection types

## Drag Selection

Enable drag selection to select multiple rows/cells by dragging:

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="700px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttSelectionSettings Mode="Syncfusion.Blazor.Grids.SelectionMode.Row" 
                            Type="Syncfusion.Blazor.Grids.SelectionType.Multiple" 
                            AllowDragSelection="true" />
</SfGantt>
```

**User Interaction:**
- Click and drag across rows/cells to select range
- Works with mouse and touch devices
- Supports all selection modes (Row, Cell, Both)
- Works with Flow and Box cell selection modes

## Programmatic Selection

### Select Row by Index

```csharp
public async Task SelectRow()
{
    await GanttInstance.SelectRowAsync(2); // Select row at index 2
}
```

### Select Multiple Rows

```csharp
public async Task SelectMultipleRows()
{
    await GanttInstance.SelectRowsAsync(new int[] { 1, 2, 3 });
}
```

### Select Cell

```csharp
public async Task SelectCell()
{
    // Select cell at row 1, column 2
    await GanttInstance.SelectCellAsync(new CellIndex { RowIndex = 1, CellIndex = 2 });
}
```

### Select Multiple Cells

```csharp
public async Task SelectMultipleCells()
{
    var cellIndexes = new List<CellIndex>
    {
        new CellIndex { RowIndex = 0, CellIndex = 1 },
        new CellIndex { RowIndex = 1, CellIndex = 2 },
        new CellIndex { RowIndex = 2, CellIndex = 3 }
    };
    await GanttInstance.SelectCellsAsync(cellIndexes);
}
```

### Clear Selection

```razor
@using Syncfusion.Blazor.Buttons

<div style="margin-bottom: 16px;">
    <SfButton @onclick="SelectRows" style="margin-right: 12px;">Select Rows</SfButton>
    <SfButton @onclick="ClearSelection">Clear Selection</SfButton>
</div>

<SfGantt @ref="GanttInstance" DataSource="@TaskCollection" Height="450px" Width="700px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttSelectionSettings Mode="Syncfusion.Blazor.Grids.SelectionMode.Row" 
                            Type="Syncfusion.Blazor.Grids.SelectionType.Multiple" />
</SfGantt>

@code {
    public SfGantt<TaskData> GanttInstance;

    public async Task SelectRows()
    {
        await GanttInstance.SelectRowsAsync(new int[] { 1, 2, 3 });
    }

    public async Task ClearSelection()
    {
        await GanttInstance.ClearSelectionAsync();
    }
}
```

## Get Selected Data

### Get Selected Records

```csharp
public async Task GetSelectedData()
{
    var selectedRecords = await GanttInstance.GetSelectedRecords();
    foreach (var record in selectedRecords)
    {
        Console.WriteLine($"Selected: {record.TaskName}");
    }
}
```

### Get Selected Row Indices

```csharp
public async Task GetSelectedIndices()
{
    var selectedIndices = await GanttInstance.GetSelectedRowIndexes();
    Console.WriteLine($"Selected indices: {string.Join(", ", selectedIndices)}");
}
```

## Selection Events

### Row Selection Events

```csharp
public void RowSelectingHandler(RowSelectingEventArgs<TaskData> args)
{
    // Before row is selected (cancellable)
    if (args.Data.Progress < 50)
    {
        args.Cancel = true; // Cancel selection for tasks with progress < 50
        Console.WriteLine("Selection cancelled: Progress too low");
    }
}

public void RowSelectedHandler(RowSelectEventArgs<TaskData> args)
{
    // After row is selected
    var selectedRow = args.Data;
    Console.WriteLine($"Selected: {selectedRow.TaskName}");
}

public void RowDeselectingHandler(RowDeselectEventArgs<TaskData> args)
{
    // Before row is deselected (cancellable)
}

public void RowDeselectedHandler(RowDeselectEventArgs<TaskData> args)
{
    // After row is deselected
    Console.WriteLine($"Deselected: {args.Data.TaskName}");
}
```

### Cell Selection Events

```csharp
public void CellSelectingHandler(CellSelectingEventArgs<TaskData> args)
{
    // Before cell is selected (cancellable)
    if (args.CellIndex.CellIndex == 0) // Prevent selecting first column
    {
        args.Cancel = true;
    }
}

public void CellSelectedHandler(CellSelectEventArgs<TaskData> args)
{
    // After cell is selected
    Console.WriteLine($"Cell [{args.CellIndex.RowIndex}, {args.CellIndex.CellIndex}] selected");
}
```

### Complete Example with Events

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Grids

<div style="margin-bottom: 12px; color: blue;">
    <strong>@SelectionMessage</strong>
</div>

<SfGantt DataSource="@TaskCollection" Height="450px" Width="700px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttSelectionSettings Mode="SelectionMode.Row" Type="SelectionType.Multiple" />
    <GanttEvents RowSelecting="RowSelectingHandler" 
                 RowSelected="RowSelectedHandler" 
                 RowDeselecting="RowDeselectingHandler" 
                 RowDeselected="RowDeselectedHandler" 
                 TValue="TaskData" />
</SfGantt>

@code {
    private string SelectionMessage { get; set; }

    public void RowSelectingHandler(RowSelectingEventArgs<TaskData> args)
    {
        SelectionMessage = $"Selecting: {args.Data.TaskName}";
    }

    public void RowSelectedHandler(RowSelectEventArgs<TaskData> args)
    {
        SelectionMessage = $"Selected: {args.Data.TaskName}";
    }

    public void RowDeselectingHandler(RowDeselectEventArgs<TaskData> args)
    {
        SelectionMessage = $"Deselecting: {args.Data.TaskName}";
    }

    public void RowDeselectedHandler(RowDeselectEventArgs<TaskData> args)
    {
        SelectionMessage = $"Deselected: {args.Data.TaskName}";
    }
}
```

## Custom Selection Styling

Apply custom CSS to selected rows/cells:

```css
/* Custom selection background */
.e-grid .e-selectionbackground {
    background-color: #bbdefb !important;
}

/* Custom selection border */
.e-grid .e-cellselectionbackground {
    border: 2px solid #2196f3 !important;
}

/* Hover effect for selected rows */
.e-grid tr.e-row.e-selectionbackground:hover {
    background-color: #90caf9 !important;
}
```

## Keyboard Navigation

Enable keyboard shortcuts for selection:

| Key | Action |
|-----|--------|
| **Arrow Keys** | Navigate between rows/cells |
| **CTRL + A** | Select all rows/cells |
| **Shift + Click** | Range select (in multiple mode) |
| **Space** | Toggle selection of focused row |
| **CTRL + Click** | Add/remove from selection |

```razor
<SfGantt DataSource="@TaskCollection" AllowKeyboard="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttSelectionSettings Type="Syncfusion.Blazor.Grids.SelectionType.Multiple" />
</SfGantt>
```

## Touch Interaction

On touch-enabled devices:
- **Single Selection:** Tap row to select
- **Multiple Selection:** Tap row → popup appears → tap popup to enable multi-select mode → tap additional rows

![Multiple selection in Blazor Gantt Chart](images/blazor-gantt-chart-multiple-selection.PNG)

## Best Practices

### Performance Tips
- **Large Datasets**: Limit simultaneous selections to avoid rendering overhead
- **Virtual Scrolling**: Combine with `EnableVirtualization` for better performance
- **Debounce Events**: Debounce selection event handlers if they trigger expensive operations

### User Experience
- Use `Type="Multiple"` for bulk operations (delete, export, etc.)
- Enable `EnableToggle` for intuitive selection/deselection
- Provide visual feedback in selection event handlers
- Clear selection after bulk operations complete

### Development Tips
- Store selected indices in state for persistence across re-renders
- Validate selections in `RowSelecting` event before allowing
- Use `GetSelectedRecords()` for batch processing
- Combine with context menu for selection-based actions

## Common Scenarios

### Select All Rows Programmatically

```csharp
public async Task SelectAllRows()
{
    var totalRows = TaskCollection.Count;
    var allIndices = Enumerable.Range(0, totalRows).ToArray();
    await GanttInstance.SelectRowsAsync(allIndices);
}
```

### Select Rows by Condition

```csharp
public async Task SelectHighPriorityTasks()
{
    var indices = TaskCollection
        .Select((task, index) => new { task, index })
        .Where(x => x.task.Priority == "High")
        .Select(x => x.index)
        .ToArray();
    
    await GanttInstance.SelectRowsAsync(indices);
}
```

### Prevent Selection of Specific Rows

```csharp
public void RowSelectingHandler(RowSelectingEventArgs<TaskData> args)
{
    if (args.Data.IsLocked)
    {
        args.Cancel = true;
        // Show notification: "Cannot select locked tasks"
    }
}
```

### Get Selected Data for Export

```csharp
public async Task ExportSelectedTasks()
{
    var selectedRecords = await GanttInstance.GetSelectedRecords();
    if (selectedRecords.Any())
    {
        // Export logic here
        await ExportService.ExportToCsv(selectedRecords);
    }
}
```

## Integration with Other Features

### Selection + Context Menu

```razor
<SfGantt DataSource="@TaskCollection" EnableContextMenu="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttSelectionSettings Type="SelectionType.Multiple" />
    <GanttContextMenuItems>
        <GanttContextMenuItem Target="Content" Items="@MenuItems" />
    </GanttContextMenuItems>
    <GanttEvents ContextMenuClick="ContextMenuClickHandler" TValue="TaskData" />
</SfGantt>

@code {
    private List<object> MenuItems = new List<object>() { "Delete Selected", "Export Selected" };

    public async Task ContextMenuClickHandler(ContextMenuClickEventArgs<TaskData> args)
    {
        if (args.Item.Text == "Delete Selected")
        {
            var selectedRecords = await GanttInstance.GetSelectedRecords();
            // Delete logic
        }
    }
}
```

### Selection + Row Drag and Drop

```razor
<SfGantt DataSource="@TaskCollection" AllowRowDragAndDrop="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttSelectionSettings Type="SelectionType.Multiple" />
</SfGantt>
```

## Troubleshooting

**Issue**: Selection not working  
**Solution**: Verify `AllowSelection="true"` (it's default, but check if explicitly disabled)

**Issue**: Multiple selection not working  
**Solution**: Ensure `Type="SelectionType.Multiple"` is set in `GanttSelectionSettings`

**Issue**: Cell selection not visible  
**Solution**: Check CSS styles - default styles may be overridden by custom themes

**Issue**: Selection lost after data refresh  
**Solution**: Store selected indices and restore after refresh:
```csharp
var indices = await GanttInstance.GetSelectedRowIndexes();
// After refresh
await GanttInstance.SelectRowsAsync(indices);
```

**Issue**: Drag selection conflicts with row drag  
**Solution**: Disable one feature or use different trigger zones

**Issue**: Performance issues with large selections  
**Solution**: Limit selection count or implement virtual scrolling with `EnableVirtualization`
