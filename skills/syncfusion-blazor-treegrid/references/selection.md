# Selection in Syncfusion Blazor TreeGrid

> **Namespace note:** `SelectionMode` and `SelectionType` belong to `Syncfusion.Blazor.Grids`, **not** `Syncfusion.Blazor.TreeGrid`. Always qualify them as `Syncfusion.Blazor.Grids.SelectionMode.Row` and `Syncfusion.Blazor.Grids.SelectionType.Multiple` to avoid CS0104 ambiguity errors.

## Table of Contents
- [Enable and Configure Selection](#enable-and-configure-selection)
- [Selection Mode](#selection-mode)
- [Selection Type](#selection-type)
- [Cell Selection](#cell-selection)
- [Checkbox Selection](#checkbox-selection)
- [Toggle Selection](#toggle-selection)
- [Drag Selection](#drag-selection)
- [Select Row at Initial Rendering](#select-row-at-initial-rendering)
- [Get Selected Row Indexes](#get-selected-row-indexes)
- [Programmatic Selection](#programmatic-selection)
- [Selection Events](#selection-events)
- [Touch Interaction](#touch-interaction)

---

## Enable and Configure Selection

Selection is enabled by default. Disable it with `AllowSelection="false"`:

```razor
<SfTreeGrid DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            AllowSelection="true">
    <TreeGridSelectionSettings Mode="Syncfusion.Blazor.Grids.SelectionMode.Row"
                               Type="Syncfusion.Blazor.Grids.SelectionType.Multiple" />
    ...
</SfTreeGrid>
```

---

## Selection Mode

| Mode | Description |
|---|---|
| `Row` | Select entire rows (default) |
| `Cell` | Select individual cells |
| `Both` | Either rows or cells |

```razor
<TreeGridSelectionSettings Mode="Syncfusion.Blazor.Grids.SelectionMode.Cell" />
<TreeGridSelectionSettings Mode="Syncfusion.Blazor.Grids.SelectionMode.Both" />
```

---

## Selection Type

| Type | Description |
|---|---|
| `Single` | Only one row/cell at a time (default) |
| `Multiple` | Hold Ctrl to select multiple rows; Shift for range |

```razor
<TreeGridSelectionSettings Type="Syncfusion.Blazor.Grids.SelectionType.Multiple" />
```

---

## Cell Selection

Cell selection can be done through simple mouse down or arrow keys (up, down, left, and right).

> **Requirement:** Cell selection requires `Mode` to be `Cell` or `Both`, and `Type` to be `Multiple`.

The `CellSelectionMode` property of `TreeGridSelectionSettings` controls the range selection behaviour:

| Mode | Description |
|---|---|
| `Flow` | Default. Selects cells between start and end index including cells across rows |
| `Box` | Selects cells from start to end column within the row range (rectangular block) |

```razor
<SfTreeGrid DataSource="@TreeGridData" IdMapping="TaskId" ParentIdMapping="ParentId" AllowSelection="true">
    <TreeGridSelectionSettings Type="Syncfusion.Blazor.Grids.SelectionType.Multiple"
                               Mode="Syncfusion.Blazor.Grids.SelectionMode.Cell"
                               CellSelectionMode="Syncfusion.Blazor.Grids.CellSelectionMode.Box" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" />
    </TreeGridColumns>
</SfTreeGrid>
```

---

## Checkbox Selection

Checkbox selection provides an option to select multiple TreeGrid records with the help of a checkbox in each row. To render the checkbox column, add a `TreeGridColumn` with `Type="Syncfusion.Blazor.Grids.ColumnType.CheckBox"`.

> **Note:** No `Field` is needed on the checkbox column. When the checkbox column is first (`index 0`), set `TreeColumnIndex` to point to the actual tree column (e.g., `TreeColumnIndex="2"` if task name is the third column).

```razor
<SfTreeGrid DataSource="@TreeGridData" IdMapping="TaskId" AllowSelection="true"
            ParentIdMapping="ParentId" TreeColumnIndex="2">
    <TreeGridColumns>
        <TreeGridColumn Type="Syncfusion.Blazor.Grids.ColumnType.CheckBox" Width="80" />
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80"  TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Priority" HeaderText="Priority"  Width="80" />
    </TreeGridColumns>
</SfTreeGrid>
```

> - By default, selection is allowed by clicking a row **or** its checkbox. To allow selection **only through the checkbox**, set `CheckboxOnly="true"` on `TreeGridSelectionSettings`.
> - Selection can be persisted across operations using `PersistSelection="true"` on `TreeGridSelectionSettings`. Requires a primary key column (`IsPrimaryKey="true"`).

### Checkbox Selection Mode

In checkbox selection, selection can also be done by clicking on rows. The `CheckboxMode` property of `TreeGridSelectionSettings` controls this behaviour:

| Value | Behavior |
|---|---|
| `Default` | User can select multiple rows by clicking rows one by one |
| `ResetOnRowClick` | Row click resets the previous selection; Ctrl+click or checkbox click adds to selection; Shift+click selects a range |

```razor
<SfTreeGrid DataSource="@TreeGridData" IdMapping="TaskId" AllowSelection="true"
            ParentIdMapping="ParentId" TreeColumnIndex="2">
    <TreeGridSelectionSettings CheckboxMode="CheckboxSelectionType.ResetOnRowClick" />
    <TreeGridColumns>
        <TreeGridColumn Type="Syncfusion.Blazor.Grids.ColumnType.CheckBox" Width="80" />
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80"  TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Priority" HeaderText="Priority"  Width="80" />
    </TreeGridColumns>
</SfTreeGrid>
```

> **Namespace:** `CheckboxSelectionType` belongs to `Syncfusion.Blazor.Grids`. Use full qualification: `Syncfusion.Blazor.Grids.CheckboxSelectionType.ResetOnRowClick` if not using `@using Syncfusion.Blazor.Grids`.

---

## Toggle Selection

Toggle selection allows performing selection and unselection of a particular row or cell. When a selected row or cell is clicked again, it is unselected (and vice versa).

Enable it with `EnableToggle="true"` on `TreeGridSelectionSettings`:

```razor
<SfTreeGrid DataSource="@TreeGridData" IdMapping="TaskId" ParentIdMapping="ParentId" AllowSelection="true">
    <TreeGridSelectionSettings EnableToggle="true"
                               Type="Syncfusion.Blazor.Grids.SelectionType.Multiple" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" />
    </TreeGridColumns>
</SfTreeGrid>
```

> **Note:** If multi-selection is enabled, the first click on any selected row (without Ctrl) clears multi-selection; the second click on the same row deselects it.

---

## Drag Selection

Drag selection allows selecting a range of cells or rows by mouse or touch dragging. Enable it with `AllowDragSelection="true"` on `TreeGridSelectionSettings`. `Type` must be `Multiple`.

- Drag selection works in all `Mode` values (`Row`, `Cell`, `Both`).
- For cell mode, both `Flow` and `Box` `CellSelectionMode` values are supported.

```razor
<SfTreeGrid DataSource="@TreeGridData" IdMapping="TaskId" ParentIdMapping="ParentId" AllowSelection="true">
    <TreeGridSelectionSettings Mode="Syncfusion.Blazor.Grids.SelectionMode.Row"
                               Type="Syncfusion.Blazor.Grids.SelectionType.Multiple"
                               AllowDragSelection="true" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" />
    </TreeGridColumns>
</SfTreeGrid>
```

---

## Select Row at Initial Rendering

To pre-select a row when the TreeGrid first renders, set the `SelectedRowIndex` property (0-based) on `SfTreeGrid`:

```razor
<SfTreeGrid DataSource="@TreeGridData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            AllowSelection="true"
            SelectedRowIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" />
    </TreeGridColumns>
</SfTreeGrid>
```

---

## Get Selected Row Indexes

Retrieve selected row indexes and records programmatically using `GetSelectedRowIndexesAsync` and `GetSelectedRecordsAsync`:

```razor
<SfTreeGrid @ref="TreeGrid" DataSource="@TreeGridData" IdMapping="TaskId" ParentIdMapping="ParentId" AllowSelection="true">
    <TreeGridEvents RowSelected="RowSelectHandler" TValue="TaskItem" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    private SfTreeGrid<TaskItem> TreeGrid;

    private async void RowSelectHandler(RowSelectEventArgs<TaskItem> args)
    {
        // Get indexes of all selected rows
        List<int> indexes = await TreeGrid.GetSelectedRowIndexesAsync();

        // Get the full record objects for all selected rows
        List<TaskItem> records = await TreeGrid.GetSelectedRecordsAsync();
    }
}
```

---

## Programmatic Selection

Select or deselect rows and cells from code:

```razor
<SfTreeGrid @ref="TreeRef" ...>...</SfTreeGrid>
<button @onclick="SelectFirstRow">Select Row 1</button>
<button @onclick="SelectMultiple">Select Rows 0–2</button>
<button @onclick="ClearSelection">Clear</button>

@code {
    private SfTreeGrid<TaskItem> TreeRef;

    // Select by row index (0-based)
    private async Task SelectFirstRow()
    {
        await TreeRef.SelectRowAsync(0);
    }

    // Select multiple rows by index
    private async Task SelectMultiple()
    {
        await TreeRef.SelectRowsAsync(new int[] { 0, 1, 2 });
    }

    // Select a specific cell (row index, column index)
    private async Task SelectCell()
    {
        await TreeRef.SelectCellAsync(new Syncfusion.Blazor.Grids.ValueTuple<int, int>(0, 1));
    }

    // Clear all selections
    private async Task ClearSelection()
    {
        await TreeRef.ClearSelectionAsync();
    }

    // Get selected records
    private async Task GetSelected()
    {
        var selected = await TreeRef.GetSelectedRecordsAsync();
        Console.WriteLine($"Selected: {selected.Count} rows");
    }
}
```

---

## Selection Events

```razor
<SfTreeGrid ...>
    <TreeGridEvents TValue="TaskItem"
                    RowSelected="OnRowSelected"
                    RowDeselected="OnRowDeselected"
                    RowSelecting="OnRowSelecting"
                    CellSelected="OnCellSelected" />
    ...
</SfTreeGrid>

@code {
    private void OnRowSelecting(RowSelectingEventArgs<TaskItem> args)
    {
        // TODO: Implement custom row selection logic here
        // Example patterns:
        /*
        // Cancel selection for certain rows
        if (args.Data.IsLocked)
            args.Cancel = true;
        
        // Enforce single selection on certain rows
        // if (args.Data.IsCritical)
        // {
        //     // Allow only one critical item selected at a time
        // }
        
        // Track selection changes
        // LogSelectionAttempt(args.Data);
        */
    }

    private void OnRowSelected(RowSelectEventArgs<TaskItem> args)
    {
        // TODO: Implement custom logic after row is selected
        // Example patterns:
        /*
        Console.WriteLine($"Selected: {args.Data.TaskName} at index {args.RowIndex}");
        
        // Update detail view or form
        // DisplayRowDetails(args.Data);
        
        // Enable/disable actions based on selection
        // UpdateActionButtons(args.Data);
        
        // Highlight related items
        // HighlightRelatedRows(args.Data);
        */
    }

    private void OnRowDeselected(RowDeselectEventArgs<TaskItem> args)
    {
        // TODO: Implement custom logic after row is deselected
        // Example patterns:
        /*
        Console.WriteLine($"Deselected: {args.Data.TaskName}");
        
        // Clear detail view
        // ClearDetailView();
        
        // Update selection counters
        // UpdateSelectionCount();
        
        // Disable related actions
        // DisableDetailActions();
        */
    }

    private void OnCellSelected(CellSelectEventArgs<TaskItem> args)
    {
        // TODO: Implement custom logic after cell is selected
        // Example patterns:
        /*
        Console.WriteLine($"Cell selected: row {args.RowIndex}, col {args.CellIndex}");
        
        // Enable cell editing
        // EnableCellEditor(args.CellIndex);
        
        // Display cell information
        // ShowCellMetadata(args);
        
        // Highlight column
        // HighlightColumn(args.CellIndex);
        */
    }
}
```
---

## Touch Interaction

On touchscreen devices, tapping a TreeGrid row selects it. A popup appears for multi-row selection:
- Tap the multi-select popup icon, then tap additional rows or cells to add them to the selection.

> **Note:** Multi-selection requires `Type` to be `Multiple`.

---
