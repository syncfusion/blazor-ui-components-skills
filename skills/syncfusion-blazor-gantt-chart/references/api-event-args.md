# API — Event Argument Classes Reference

## Table of Contents
- [Core Action Events](#core-action-events)
- [Row CRUD Events](#row-crud-events)
- [Taskbar Editing Events](#taskbar-editing-events)
- [Dialog Events](#dialog-events)
- [Dependency Events](#dependency-events)
- [Segment Events](#segment-events)
- [Tooltip Events](#tooltip-events)
- [Clipboard Events](#clipboard-events)
- [PDF Export Events](#pdf-export-events)
- [Undo/Redo Events](#undoredo-events)
- [Zoom Events](#zoom-events)
- [Resource Allocation Events](#resource-allocation-events)
- [Context Menu Events](#context-menu-events)
- [Usage Notes](#usage-notes)
- [Related References](#related-references)

---
Comprehensive reference for Gantt event argument types. These classes provide data and control for various Gantt component events. Most support cancellation via a `Cancel` property.

---

## Core Action Events

### GanttActionEventArgs<TValue>

Primary event argument for action lifecycle events (`OnActionBegin`, `OnActionComplete`, `OnActionFailure`).

**Key Properties:**
- **`RequestType`** (`Action`) - Current action: Add, Edit, Delete, Save, Refresh, Sorting, Filtering, Searching, etc.
- **`Data`** (`TValue`) - Record object(s) involved in the action
- **`PreviousData`** (`TValue`) - Previous state before action
- **`Cancel`** (`bool`) - Set true to cancel the action. Default: false
- **`Action`** (`string`) - String representation of current action
- **`RowData`** (`TValue`) - Record object for row operations
- **`RowIndex`** (`int`) - Index of edited/affected row
- **`ModifiedTaskData`** (`List<TValue>`) - Collection of modified tasks
- **`EditContext`** (`EditContext`) - Form edit context
- **`TaskBarEditAction`** (`string`) - Type of taskbar edit (resize, drag, progress)
- **`Direction`** (`SortDirection`) - Sort direction (Ascending/Descending)
- **`Columns`** (`List<PredicateModel<object>>`) - Filter column collection
- **`CurrentFilterObject`** (`PredicateModel<object>`) - Currently filtered object
- **`SearchString`** (`string`) - Search keyword
- **`ValidateMode`** (`ValidateMode`) - Validation mode for predecessors
- **`EnableAutoLinkValidation`** (`bool`) - Enable predecessor validation on draw. Default: true
- **`PredecessorOffSetValidation`** (`bool`) - Restrict taskbar drag beyond predecessor. Default: false
- **`CurrentZoomingLevel`** (`GanttZoomTimelineSettings`) - Current zoom level

**Example:**
```csharp
<SfGantt DataSource="@TaskCollection">
    <GanttEvents OnActionBegin="ActionBeginHandler" 
                 OnActionComplete="ActionCompleteHandler"
                 TValue="TaskData">
    </GanttEvents>
</SfGantt>

@code {
    private void ActionBeginHandler(GanttActionEventArgs<TaskData> args)
    {
        if (args.RequestType == Syncfusion.Blazor.Gantt.Action.BeforeSave)
        {
            // Validate before saving
            if (args.Data.Duration <= 0)
            {
                args.Cancel = true;
                // Show error message
            }
        }
    }
    
    private void ActionCompleteHandler(GanttActionEventArgs<TaskData> args)
    {
        if (args.RequestType == Syncfusion.Blazor.Gantt.Action.Save)
        {
            Console.WriteLine($"Task saved: {args.Data.TaskName}");
        }
    }
}
```

---

## Row CRUD Events

### GanttRowUpdatingEventArgs<T>

Fired before a row is updated.

**Properties:**
- **`Cancel`** (`bool`) - Cancel the update
- **`Data`** (`T`) - Updated data object
- **`PreviousData`** (`T`) - Previous data state
- **`Index`** (`int`) - Row index
- **`Action`** (`string`) - Action type
- **`PrimaryKeyValue`** (`object`) - Primary key value
- **`IsShiftKeyPressed`** (`bool`) - Whether Shift key was pressed
- **`KeyCode`** (`int`) - Key code if keyboard action

### GanttRowUpdatedEventArgs<T>

Fired after a row is updated successfully.

**Properties:** Same as `GanttRowUpdatingEventArgs<T>` (but read-only, no Cancel)

### GanttRowDeletingEventArgs<T> / GanttRowDeletedEventArgs<T>

Similar structure for delete operations (`RowDeleting` fires before, `RowDeleted` fires after).

### GanttRowCreatingEventArgs<T> / GanttRowCreatedEventArgs<T>

Similar structure for row creation operations.

**Example:**
```csharp
<SfGantt DataSource="@TaskCollection">
    <GanttEvents RowUpdating="RowUpdatingHandler" 
                 RowUpdated="RowUpdatedHandler"
                 TValue="TaskData">
    </GanttEvents>
</SfGantt>

@code {
    private void RowUpdatingHandler(GanttRowUpdatingEventArgs<TaskData> args)
    {
        // Validate before update
        if (args.Data.EndDate < args.Data.StartDate)
        {
            args.Cancel = true;
        }
    }
    
    private void RowUpdatedHandler(GanttRowUpdatedEventArgs<TaskData> args)
    {
        Console.WriteLine($"Row {args.Index} updated successfully");
    }
}
```

---

## Taskbar Editing Events

### TaskbarEditingEventArgs<T>

Fired before taskbar editing begins.

**Properties:**
- **`Cancel`** (`bool`) - Cancel the taskbar edit
- **`Data`** (`T`) - Task data
- **`RecordIndex`** (`int`) - Task index
- **`TaskBarEditAction`** (`string`) - Edit action type (LeftResizing, RightResizing, ProgressResizing, ChildDrag, ConnectorPointLeftDrag, etc.)
- **`RoundOffDuration`** (`bool`) - Whether to round off duration

### TaskbarEditedEventArgs<T>

Fired after taskbar editing completes.

**Properties:**
- **`Action`** (`string`) - Action type
- **`Data`** (`T`) - Updated task data
- **`PreviousData`** (`T`) - Previous task state
- **`RecordIndex`** (`int`) - Task index
- **`TaskBarEditAction`** (`string`) - Edit action type
- **`ColumnName`** (`string`) - Edited column name
- **`EditingFields`** (`GanttTaskModel`) - Editable field model
- **`RoundOffDuration`** (`bool`) - Duration rounding flag

**Example:**
```csharp
<SfGantt DataSource="@TaskCollection">
    <GanttEditSettings AllowTaskbarEditing="true"></GanttEditSettings>
    <GanttEvents TaskbarEditing="TaskbarEditingHandler" 
                 TaskbarEdited="TaskbarEditedHandler"
                 TValue="TaskData">
    </GanttEvents>
</SfGantt>

@code {
    private void TaskbarEditingHandler(TaskbarEditingEventArgs<TaskData> args)
    {
        Console.WriteLine($"Editing taskbar: {args.TaskBarEditAction}");
        // Optionally cancel specific actions
        if (args.TaskBarEditAction == "ProgressResizing")
        {
            // args.Cancel = true;
        }
    }
    
    private void TaskbarEditedHandler(TaskbarEditedEventArgs<TaskData> args)
    {
        Console.WriteLine($"Taskbar edited. New duration: {args.EditingFields.Duration}");
    }
}
```

---

## Dialog Events

### GanttDialogOpenEventArgs<TValue>

Fired before a dialog (Add/Edit) opens.

**Properties:**
- **`Cancel`** (`bool`) - Cancel dialog opening
- **`Data`** (`TValue`) - Row data being edited
- **`IsEditAction`** (`bool`) - True if edit dialog, false if add dialog

### GanttDialogOpenedEventArgs<TValue>

Fired after dialog is opened.

**Properties:**
- **`Data`** (`TValue`) - Row data
- **`EditType`** (`DialogType`) - Dialog type (Add/Edit)

### GanttDialogCloseEventArgs<TValue>

Fired before dialog closes.

**Properties:**
- **`Cancel`** (`bool`) - Cancel dialog close
- **`Data`** (`TValue`) - Row data
- **`RequestType`** (`string`) - Close action type (Save/Cancel)

**Example:**
```csharp
<SfGantt DataSource="@TaskCollection">
    <GanttEvents DialogOpening="DialogOpeningHandler" 
                 DialogOpened="DialogOpenedHandler"
                 DialogClosing="DialogClosingHandler"
                 TValue="TaskData">
    </GanttEvents>
</SfGantt>

@code {
    private void DialogOpeningHandler(GanttDialogOpenEventArgs<TaskData> args)
    {
        if (args.IsEditAction)
        {
            Console.WriteLine($"Editing task: {args.Data.TaskName}");
        }
        else
        {
            Console.WriteLine("Adding new task");
        }
    }
    
    private void DialogClosingHandler(GanttDialogCloseEventArgs<TaskData> args)
    {
        if (args.RequestType == "save")
        {
            // Custom validation
            if (string.IsNullOrEmpty(args.Data.TaskName))
            {
                args.Cancel = true;
            }
        }
    }
}
```

---

## Dependency Events

### TaskConnectorChangeEventArgs<T>

Fired before a dependency (predecessor) changes.

**Properties:**
- **`Cancel`** (`bool`) - Cancel the dependency change
- **`Data`** (`T`) - Task data
- **`PredecessorString`** (`string`) - New predecessor string
- **`Type`** (`DependencyType`) - Dependency type (FS, SS, FF, SF)

### TaskConnectorChangedEventArgs<T>

Fired after dependency changes.

**Properties:** Similar to `TaskConnectorChangeEventArgs<T>` (read-only)

---

## Segment Events

### SegmentEventArgs<T>

Used for split task segment operations.

**Properties:**
- **`Cancel`** (`bool`) - Cancel segment operation
- **`Data`** (`T`) - Task data
- **`SegmentData`** (`GanttSegmentData`) - Segment information

---

## Tooltip Events

### BeforeTooltipRenderEventArgs<TValue>

Fired before a tooltip renders, allowing customization.

**Properties:**
- **`Cancel`** (`bool`) - Cancel tooltip display
- **`Content`** (`RenderFragment`) - Custom tooltip content
- **`Data`** (`TValue`) - Task data for the tooltip
- **`Target`** (`string`) - Target element

**Example:**
```csharp
<SfGantt DataSource="@TaskCollection">
    <GanttEvents BeforeTooltipRender="BeforeTooltipRenderHandler" TValue="TaskData">
    </GanttEvents>
</SfGantt>

@code {
    private void BeforeTooltipRenderHandler(BeforeTooltipRenderEventArgs<TaskData> args)
    {
        // Customize or cancel tooltip
        if (args.Data.Progress < 50)
        {
            args.Content = @<div style="color:red;">Behind schedule!</div>;
        }
    }
}
```

---

## Clipboard Events

### BeforeCopyEventArgs

Fired before clipboard copy operation.

**Properties:**
- **`Cancel`** (`bool`) - Cancel copy operation
- **`ClipboardText`** (`string`) - Text being copied to clipboard (can modify)

**Example:**
```csharp
<SfGantt DataSource="@TaskCollection">
    <GanttEvents BeforeCopy="BeforeCopyHandler" TValue="TaskData">
    </GanttEvents>
</SfGantt>

@code {
    private void BeforeCopyHandler(BeforeCopyEventArgs args)
    {
        Console.WriteLine($"Copying: {args.ClipboardText}");
        // Optionally modify clipboard content
        args.ClipboardText = "Custom: " + args.ClipboardText;
    }
}
```

---

## PDF Export Events

### PdfExportEventArgs

Fired before PDF export begins.

**Properties:**
- **`Cancel`** (`bool`) - Cancel export
- **`PdfExportProperties`** (`GanttPdfExportProperties`) - Export configuration
- **`IsMultipleExport`** (`bool`) - Whether exporting multiple grids

### PdfExportedEventArgs

Fired after PDF export completes.

**Properties:**
- **`PdfDocument`** - Exported PDF document object

### PdfQueryCellInfoEventArgs<TValue>

Fired for each cell during PDF export, allowing customization.

**Properties:**
- **`Cell`** (`PdfGanttCell`) - Cell being exported
- **`Data`** (`TValue`) - Row data
- **`Column`** - Column definition

### PdfQueryTaskbarInfoEventArgs<TValue>

Fired for each taskbar during PDF export.

**Properties:**
- **`Taskbar`** (`PdfTaskbar`) - Taskbar being exported
- **`Data`** (`TValue`) - Task data
- **`TaskbarColor`** (`PdfTaskbarColor`) - Taskbar colors

**Example:**
```csharp
<SfGantt DataSource="@TaskCollection" AllowPdfExport="true">
    <GanttEvents PdfExport="PdfExportHandler" 
                 PdfQueryCellInfo="PdfQueryCellHandler"
                 TValue="TaskData">
    </GanttEvents>
</SfGantt>

@code {
    private void PdfExportHandler(PdfExportEventArgs args)
    {
        args.PdfExportProperties.FileName = "ProjectGantt_" + DateTime.Now.ToString("yyyyMMdd");
    }
    
    private void PdfQueryCellHandler(PdfQueryCellInfoEventArgs<TaskData> args)
    {
        if (args.Column.Field == "Duration" && args.Data.Duration > 10)
        {
            args.Cell.Style.BackgroundColor = new PdfColor(255, 200, 200);
        }
    }
}
```

---

## Undo/Redo Events

### GanttUndoRedoEventArgs<TValue>

Fired for undo/redo operations.

**Properties:**
- **`Cancel`** (`bool`) - Cancel undo/redo
- **`Action`** (`GanttUndoRedoAction`) - Action being undone/redone
- **`Data`** (`TValue`) - Affected data
- **`RequestType`** (`string`) - "undo" or "redo"

---

## Zoom Events

### ZoomEventArgs

Fired before zooming action.

**Properties:**
- **`Cancel`** (`bool`) - Cancel zoom
- **`PreviousZoomingLevel`** (`GanttZoomTimelineSettings`) - Previous zoom level
- **`RequestType`** (`ZoomAction`) - Zoom action (ZoomIn/ZoomOut/ZoomToFit)

### ZoomedEventArgs

Fired after zooming completes.

**Properties:**
- **`CurrentZoomingLevel`** (`GanttZoomTimelineSettings`) - Current zoom level
- **`PreviousZoomingLevel`** (`GanttZoomTimelineSettings`) - Previous zoom level

---

## Resource Allocation Events

### ResourceAssignmentChangeEventArgs<TValue>

Fired when resource assignments change.

**Properties:**
- **`Cancel`** (`bool`) - Cancel assignment change
- **`Data`** (`TValue`) - Task data
- **`ResourceData`** - Resource information

---

## Context Menu Events

### ContextMenuClickEventArgs<TValue>

Fired when context menu item is clicked.

**Properties:**
- **`Item`** (`ContextMenuItemModel`) - Clicked menu item
- **`RowData`** (`TValue`) - Row data where menu was invoked

### ContextMenuOpenEventArgs<TValue>

Fired before context menu opens.

**Properties:**
- **`Cancel`** (`bool`) - Cancel menu opening
- **`RowData`** (`TValue`) - Row data
- **`Items`** - Menu items collection (can modify)

---

## Usage Notes

1. **Cancellation Pattern**: Most "...ing" events (before action) support `Cancel = true` to prevent the action.

2. **Strongly-Typed Data**: Use generic `TValue` parameter matching your data model:
   ```csharp
   <GanttEvents TValue="TaskData">
   ```

3. **Event Order**: Typical sequence:
   - `OnActionBegin` - `[Specific event like RowUpdating]` - Action executes - `[Specific event like RowUpdated]` - `OnActionComplete`

4. **EditContext**: Available in `GanttActionEventArgs` for form validation scenarios.

5. **Async Events**: Most event handlers can be async:
   ```csharp
   private async Task ActionBeginHandler(GanttActionEventArgs<TaskData> args)
   {
       await ValidateAsync(args.Data);
   }
   ```
