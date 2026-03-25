# Gantt Methods & Properties - Developer Reference

## Table of Contents
- [SfGantt Class](#sfgantt-class)
- [Core Properties](#core-properties) - [Methods Reference](#methods-reference) - [Key Child Classes](#key-child-classes)

---

Comprehensive summary of the `SfGantt<TValue>` class with properties, methods, and practical code examples.

---

## SfGantt<TValue> Class

Main component wrapper for rendering Gantt charts. Inherits from `SfDataBoundComponent`.

**Type Parameter:**
- `TValue`: Data model type (inferred from `DataSource` or specified explicitly with `SfDataManager`)

---

## Core Properties

### Data Binding
- **`DataSource`** (`IEnumerable<TValue>`) - Data collection for Gantt rows
- **`TaskFields`** (`GanttTaskFields`) - Maps model fields to Gantt behavior
- **`Query`** (`Query`) - Data query for filtering/operations

```csharp
<SfGantt DataSource="@TaskCollection">
    <GanttTaskFields Id="TaskId" Name="TaskName" StartDate="StartDate" 
                     EndDate="EndDate" Duration="Duration" Progress="Progress" 
                     ParentID="ParentId" Dependency="Predecessor">
    </GanttTaskFields>
</SfGantt>

@code {
    private List<TaskData> TaskCollection { get; set; }
}
```

### Display & Layout
- **`Height`** (`string`) - Component height ("500px", "auto"). Default: "auto"
- **`Width`** (`string`) - Component width
- **`ID`** (`string`) - DOM element ID
- **`RowHeight`** (`int`) - Grid and chart row height in pixels. Default: 36
- **`TaskbarHeight`** (`int?`) - Taskbar height in pixels
- **`TreeColumnIndex`** (`int`) - Column index for tree structure display. Default: 0

```csharp
<SfGantt DataSource="@TaskCollection" Height="450px" Width="100%" 
         RowHeight="50" TaskbarHeight="40" TreeColumnIndex="1">
</SfGantt>
```

### Columns
- **`Columns`** (`List<GanttColumn>`) - Explicit column definitions
- **`FrozenColumns`** (`int`) - Number of frozen columns. Default: 0
- **`ShowColumnChooser`** (`bool`) - Enable column chooser. Default: false
- **`ShowColumnMenu`** (`bool`) - Enable column menu. Default: false
- **`ColumnMenuItems`** (`string[]`) - Built-in column menu items
- **`ColumnChooserSettings`** (`GanttColumnChooserSettings`) - Column chooser configuration

```csharp
<SfGantt DataSource="@TaskCollection" Columns="@columns" ShowColumnMenu="true" 
         AllowSorting="true" ColumnMenuItems="@columnMenuItems">
</SfGantt>

@code {
    private List<GanttColumn> columns = new List<GanttColumn>()
    {
        new GanttColumn() { Field = "TaskId", HeaderText = "ID", Width = "150" },
        new GanttColumn() { Field = "TaskName", HeaderText = "Task Name", Width = "300" },
        new GanttColumn() { Field = "Duration", HeaderText = "Duration", Width = "200" }
    };
    
    private string[] columnMenuItems = new string[] { "AutoFitAll", "AutoFit", "SortAscending" };
}
```

### Editing
- **`EditSettings`** (`GanttEditSettings`) - CRUD operation configuration
- **`AddDialogFields`** (`List<GanttAddDialogField>`) - Add dialog tab/field layout
- **`EditDialogFields`** (`List<GanttEditDialogField>`) - Edit dialog tab/field layout
- **`AllowUnscheduledTasks`** (`bool`) - Allow tasks without full date info. Default: false

```csharp
<SfGantt DataSource="@TaskCollection" Toolbar="@(new List<string>() { "Add", "Edit", "Delete" })">
    <GanttEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true">
    </GanttEditSettings>
</SfGantt>
```

### Grid Features
- **`AllowSorting`** (`bool`) - Enable sorting. Default: false
- **`AllowMultiSorting`** (`bool`) - Enable multi-column sort. Default: true
- **`AllowFiltering`** (`bool`) - Enable filtering. Default: false
- **`AllowSelection`** (`bool`) - Enable row/cell selection. Default: true
- **`AllowReordering`** (`bool`) - Enable column reordering. Default: false
- **`AllowResizing`** (`bool`) - Enable column resizing. Default: false
- **`AllowRowDragAndDrop`** (`bool`) - Enable row reordering. Default: false
- **`SortSettings`** (`GanttSortSettings`) - Sort behavior configuration
- **`FilterSettings`** (`GanttFilterSettings`) - Filter behavior configuration
- **`SearchSettings`** (`GanttSearchSettings`) - Search configuration
- **`SelectionSettings`** (`GanttSelectionSettings`) - Selection configuration

```csharp
<SfGantt DataSource="@TaskCollection" AllowSorting="true" AllowFiltering="true" 
         SortSettings="@sortSettings" FilterSettings="@filterSettings">
</SfGantt>

@code {
    private GanttSortSettings sortSettings = new GanttSortSettings()
    {
        Columns = new List<GanttSortDescriptor>() {
            new GanttSortDescriptor() { Field="TaskId", Direction=SortDirection.Descending }
        }
    };
    
    private GanttFilterSettings filterSettings = new GanttFilterSettings()
    {
        HierarchyMode = FilterHierarchyMode.None
    };
}
```

### Timeline & Scheduling
- **`ProjectStartDate`** (`DateTime?`) - Project start date. Default: null
- **`ProjectEndDate`** (`DateTime?`) - Project end date. Default: null
- **`DurationUnit`** (`DurationUnit`) - Duration field unit. Default: Day
- **`TaskMode`** (`ScheduleMode`) - Scheduling mode (Auto/Manual/Custom). Default: Auto
- **`TimelineSettings`** (`GanttTimelineSettings`) - Timeline configuration
- **`TimelineTierSettings`** (`GanttTimelineTierSettings`) - Timeline tier settings
- **`CustomZoomingLevels`** (`GanttZoomTimelineSettings[]`) - Custom zoom levels
- **`DayWorkingTime`** (`List<GanttDayWorkingTime>`) - Working hours. Default: 8:00-17:00
- **`IncludeWeekend`** (`bool`) - Include weekends as working days. Default: false
- **`HighlightWeekends`** (`bool`) - Highlight weekend days. Default: false
- **`Holidays`** (`List<GanttHoliday>`) - Holiday definitions
- **`Timezone`** (`string?`) - Time zone for scheduling (e.g., "UTC"). Default: null

```csharp
<SfGantt DataSource="@TaskCollection" ProjectStartDate="@ProjectStart" 
         DurationUnit="DurationUnit.Day" DayWorkingTime="@workingTimes" 
         Holidays="@holidays" HighlightWeekends="true">
</SfGantt>

@code {
    private DateTime? ProjectStart = new DateTime(2022, 01, 01);
    
    private List<GanttDayWorkingTime> workingTimes = new List<GanttDayWorkingTime>()
    {
        new GanttDayWorkingTime() { From = 9, To = 13 },
        new GanttDayWorkingTime() { From = 14, To = 20 }
    };
    
    private List<GanttHoliday> holidays = new List<GanttHoliday>()
    {
        new GanttHoliday() { 
            From = new DateTime(2022, 04, 11), 
            To = new DateTime(2022, 04, 12), 
            Label = "Public holidays"
        }
    };
}
```

### Dependencies & Validation
- **`DependencyTypes`** (`List<DependencyType>`) - Allowed dependency types. Default: FS, SS, FF, SF
- **`EnablePredecessorValidation`** (`bool`) - Validate dependencies. Default: true
- **`AutoUpdatePredecessorOffset`** (`bool`) - Auto-update offsets. Default: true

```csharp
<SfGantt DataSource="@TaskCollection" EnablePredecessorValidation="true"
         DependencyTypes="@(new List<DependencyType>() { DependencyType.FS })">
</SfGantt>
```

### Virtualization & Performance
- **`EnableRowVirtualization`** (`bool`) - Row virtualization. Default: false
- **`EnableColumnVirtualization`** (`bool`) - Column virtualization. Default: false
- **`EnableTimelineVirtualization`** (`bool`) - Timeline virtualization. Default: false
- **`PageSize`** (`int?`) - Items per virtual page
- **`OverscanCount`** (`int`) - Extra items to pre-render. Default: 0
- **`LoadChildOnDemand`** (`bool`) - Load children on expand (remote data). Default: true
- **`AutoCalculateDateScheduling`** (`bool`) - Auto-calculate dates. Default: true

```csharp
<SfGantt DataSource="@TaskCollection" EnableRowVirtualization="true" 
         EnableColumnVirtualization="true" EnableTimelineVirtualization="true"
         PageSize="50" OverscanCount="5">
</SfGantt>
```

### Exports
- **`AllowPdfExport`** (`bool`) - Enable PDF export. Default: false
- **`AllowExcelExport`** (`bool`) - Enable Excel/CSV export. Default: false

```csharp
<SfGantt @ref="Gantt" DataSource="@TaskCollection" AllowPdfExport="true" 
         Toolbar="@toolbarItems">
    <GanttEvents OnToolbarClick="ToolbarClickHandler" TValue="TaskData"></GanttEvents>
</SfGantt>

@code {
    private SfGantt<TaskData> Gantt;
    
    private List<object> toolbarItems = new List<Object>() { 
        new ToolbarItem() { 
            Text = "PDF Export", 
            Id = "PdfExport", 
            PrefixIcon = "e-pdfexport" 
        } 
    };
    
    public async void ToolbarClickHandler(ClickEventArgs args) {
        if (args.Item.Id == "PdfExport") {
            await Gantt.ExportToPdfAsync();
        }
    }
}
```

### UI Features
- **`Toolbar`** (`object`) - Toolbar items (built-in or custom)
- **`ContextMenuItems`** (`object`) - Context menu items
- **`EnableContextMenu`** (`bool`) - Enable all built-in context menu items. Default: false
- **`EventMarkers`** (`List<GanttEventMarker>`) - Event marker definitions
- **`SplitterSettings`** (`GanttSplitterSettings`) - Splitter pane configuration
- **`TooltipSettings`** (`GanttTooltipSettings<TValue>`) - Tooltip configuration
- **`LabelSettings`** (`GanttLabelSettings<TValue>`) - Taskbar label configuration

```csharp
<SfGantt DataSource="@TaskCollection" EnableContextMenu="true" 
         ContextMenuItems="@contextMenuItems" EventMarkers="@eventMarkers">
</SfGantt>

@code {
    private object contextMenuItems = new List<object> { 
        "Add", "TaskInformation", 
        new ContextMenuItemModel { Text = "Refresh", Id = "refresh" } 
    };
    
    private List<GanttEventMarker> eventMarkers = new List<GanttEventMarker>()
    {
        new GanttEventMarker() { 
            Day = new DateTime(2019, 04, 11), 
            Label = "Project approval and kick-off" 
        }
    };
}
```

### Visual Customization
- **`RenderBaseline`** (`bool`) - Show baseline taskbars. Default: true
- **`BaselineColor`** (`string`) - Baseline color (e.g., "Orange", "#FFA500")
- **`ConnectorLineBackground`** (`string`) - Dependency line color. Default: "transparent"
- **`ConnectorLineWidth`** (`int`) - Dependency line width in pixels. Default: 1
- **`GridLines`** (`GridLine`) - Row/cell borders (Both/None/Horizontal/Vertical). Default: Horizontal
- **`EnableRowHover`** (`bool`) - Hover effect on rows. Default: false
- **`TaskbarSettings`** (`GanttTaskbarSettings`) - Taskbar behavior configuration

### Critical Path & Resources
- **`EnableCriticalPath`** (`bool`) - Highlight critical tasks. Default: false
- **`CriticalSettings`** (`GanttCriticalPathSettings`) - Critical path configuration
- **`ShowOverallocation`** (`bool`) - Highlight overallocated resources. Default: false
- **`TaskType`** (`TaskType`) - Task behavior (FixedUnit/FixedDuration/FixedWork/None). Default: FixedUnit

### WBS & State
- **`ShowWbsColumn`** (`bool`) - Display WBS column. Default: false
- **`AutoGenerateWbs`** (`bool`) - Auto-generate WBS values. Default: false
- **`EnableUndoRedo`** (`bool`) - Enable undo/redo. Default: false
- **`UndoRedoActions`** (`List<GanttUndoRedoAction>?`) - Tracked action types
- **`MaxUndoRedoSteps`** (`int`) - Max undo/redo history. Default: 20

### Other Settings
- **`Locale`** (`string`) - Culture for localized content. Default: "en-US"
- **`EnableRtl`** (`bool`) - Right-to-left layout. Default: false
- **`EnablePersistence`** (`bool`) - Persist state in browser storage. Default: false
- **`EnableAdaptiveUI`** (`bool`) - Mobile/tablet adaptive UI. Default: false
- **`CollapseAllParentTasks`** (`bool`) - Initial collapsed state. Default: false
- **`SelectedRowIndex`** (`int`) - Initially selected row index
- **`ScrollToTaskbarOnClick`** (`bool`) - Scroll to taskbar on row click. Default: false
- **`DateFormat`** (`string`) - Date display format (culture-based default)
- **`DisableHtmlEncode`** (`bool`) - Disable HTML encoding. Default: false
- **`AllowFreezeLineMoving`** (`bool`) - Drag freeze line. Default: false

---

## Methods Reference

### CRUD Operations

**`AddRecordAsync(TValue, int?, RowPosition?, object?)`** - Adds new record
```csharp
TaskData data = new TaskData() { TaskId = 30, TaskName = "New Task" };
await Gantt.AddRecordAsync(data, 29, RowPosition.Below);
```

**`UpdateRecordByIDAsync(object)`** - Updates record by ID
```csharp
await Gantt.UpdateRecordByIDAsync(updatedData);
```

**`DeleteRecordAsync(int|string|Guid)`** - Deletes record (3 overloads)
```csharp
await Gantt.DeleteRecordAsync(2);
```

**`RefreshAsync()`** - Refreshes component after external changes
```csharp
await Gantt.RefreshAsync();
```

### Dialog Operations

**`OpenAddDialogAsync()`** - Opens add task dialog
```csharp
await Gantt.OpenAddDialogAsync();
```

**`OpenEditDialogAsync(int|string|Guid)`** - Opens edit dialog (3 overloads)
```csharp
await Gantt.OpenEditDialogAsync(2);
```

**`CancelEdit()`** - Cancels current edit operation
```csharp
Gantt.CancelEdit();
```

### Selection Operations

**`SelectRowAsync(int, bool)`** - Selects row by index
```csharp
await Gantt.SelectRowAsync(2, true);
```

**`SelectRowsAsync(int[])`** - Selects multiple rows
```csharp
await Gantt.SelectRowsAsync(new int[] {2, 5});
```

**`SelectRowsByRangeAsync(int, int)`** - Selects row range
```csharp
await Gantt.SelectRowsByRangeAsync(2, 6);
```

**`SelectCellAsync((int, int), bool?)`** - Selects cell
```csharp
await Gantt.SelectCellAsync((1, 2), true);
```

**`ClearSelectionAsync()`** - Clears all selections
```csharp
await Gantt.ClearSelectionAsync();
```

**`GetSelectedRecordsAsync()`** - Returns selected records
```csharp
var records = await Gantt.GetSelectedRecordsAsync();
```

**`GetSelectedRowIndexesAsync()`** - Returns selected row indexes
```csharp
var indexes = await Gantt.GetSelectedRowIndexesAsync();
```

**`GetSelectedRowCellIndexesAsync()`** - Returns selected cell indexes
```csharp
var cells = await Gantt.GetSelectedRowCellIndexesAsync();
```

### Search & Filter Operations

**`SearchAsync(string)`** - Searches records
```csharp
await Gantt.SearchAsync("Project");
```

**`FilterByColumnAsync(string, string, string, string?, bool?, bool?)`** - Filters by column
```csharp
await Gantt.FilterByColumnAsync("TaskName", "startswith", "Design", "or", true, false);
```

**`ClearFilteringAsync()`** - Clears all filters
```csharp
await Gantt.ClearFilteringAsync();
```

**`GetFilteredRecordsAsync()`** - Returns filtered records
```csharp
var filtered = await Gantt.GetFilteredRecordsAsync();
```

### Sort Operations

**`SortByColumnAsync(string, SortDirection, bool?)`** - Sorts by column
```csharp
await Gantt.SortByColumnAsync("TaskName", SortDirection.Ascending, true);
```

**`ClearSortingAsync()`** - Clears sorting
```csharp
await Gantt.ClearSortingAsync();
```

### Expansion & Collapse

**`ExpandAllAsync()`** - Expands all parent tasks
```csharp
await Gantt.ExpandAllAsync();
```

**`CollapseAllAsync()`** - Collapses all parent tasks
```csharp
await Gantt.CollapseAllAsync();
```

**`ExpandAtLevelAsync(int)`** - Expands to specific level
```csharp
await Gantt.ExpandAtLevelAsync(2);
```

**`CollapseAtLevelAsync(int)`** - Collapses to specific level
```csharp
await Gantt.CollapseAtLevelAsync(0);
```

**`ExpandByKeyAsync(object)`** - Expands record by key
```csharp
await Gantt.ExpandByKeyAsync(12);
```

**`CollapseByKeyAsync(object)`** - Collapses record by key
```csharp
await Gantt.CollapseByKeyAsync(12);
```

### Export Operations

**`ExportToPdfAsync(GanttPdfExportProperties?, bool)`** - Exports to PDF
```csharp
await Gantt.ExportToPdfAsync(exportProps, true);
```

**`ExportToExcelAsync()`** - Exports to Excel
```csharp
await Gantt.ExportToExcelAsync();
```

**`ExportToCsvAsync(ExcelExportProperties?)`** - Exports to CSV
```csharp
await Gantt.ExportToCsvAsync();
```

### Column Operations

**`OpenColumnChooser(double?, double?)`** - Opens column chooser
```csharp
await Gantt.OpenColumnChooser(100, 200);
```

**`HideColumnAsync(string, string)`** - Hides column
```csharp
await Gantt.HideColumnAsync("TaskId", "Field");
```

**`HideColumnsAsync(string[], string)`** - Hides multiple columns
```csharp
await Gantt.HideColumnsAsync(new[] {"TaskId", "Duration"}, "Field");
```

**`ShowColumnAsync(string, string)`** - Shows hidden column
```csharp
await Gantt.ShowColumnAsync("TaskName", "Field");
```

**`ShowColumnsAsync(string[], string)`** - Shows multiple columns
```csharp
await Gantt.ShowColumnsAsync(new[] {"TaskName", "Duration"}, "Field");
```

**`GetColumnsAsync()`** - Returns columns collection
```csharp
var columns = await Gantt.GetColumnsAsync();
```

**`GetColumnIndexByFieldAsync(string)`** - Gets column index
```csharp
int index = await Gantt.GetColumnIndexByFieldAsync("TaskName");
```

**`AutoFitColumnsAsync(string[])`** - Autofits column widths
```csharp
await Gantt.AutoFitColumnsAsync(new[] {"TaskName", "Duration"});
```

**`ReorderColumnsAsync(List<string>, string)`** - Reorders columns
```csharp
await Gantt.ReorderColumnsAsync(new List<string> {"TaskName"}, "Duration");
```

**`RefreshColumnsAsync()`** - Refreshes columns
```csharp
await Gantt.RefreshColumnsAsync();
```

### Row Operations

**`ReorderRowAsync(int, int, string)`** - Reorders rows
```csharp
await Gantt.ReorderRowAsync(2, 5, "above");
```

**`IndentAsync()`** - Indents selected row
```csharp
await Gantt.IndentAsync();
```

**`OutdentAsync()`** - Outdents selected row
```csharp
await Gantt.OutdentAsync();
```

### Dependency Operations

**`AddPredecessor(int|string|Guid, string)`** - Adds predecessor (3 overloads)
```csharp
Gantt.AddPredecessor(12, "4FS");
```

**`RemovePredecessor(int|string|Guid)`** - Removes predecessor (3 overloads)
```csharp
Gantt.RemovePredecessor(2);
```

**`UpdatePredecessor(int|string|Guid, string)`** - Updates predecessor (3 overloads)
```csharp
Gantt.UpdatePredecessor(7, "4FS+2");
```

### Resource Operations

**`AddResourceAssignmentAsync<TAssignment>(TAssignment)`** - Adds resource assignment
```csharp
await Gantt.AddResourceAssignmentAsync(newAssignment);
```

**`DeleteResourceAssignmentAsync<TAssignment>(TAssignment)`** - Deletes resource assignment
```csharp
await Gantt.DeleteResourceAssignmentAsync(assignment);
```

**`UpdateResourceAssignmentAsync<TAssignment>(TAssignment)`** - Updates resource assignment
```csharp
await Gantt.UpdateResourceAssignmentAsync(updatedAssignment);
```

**`GetResourceAssignments<TAssignment>(TValue)`** - Gets task resource assignments
```csharp
var assignments = Gantt.GetResourceAssignments<ResourceAssignment>(taskData);
```

**`GetResources<TResources>(TValue)`** - Gets task resources
```csharp
var resources = Gantt.GetResources<Resource>(taskData);
```

### Task Operations

**`SplitTaskAsync(int|string|Guid, List<DateTime>)`** - Splits task (3 overloads)
```csharp
await Gantt.SplitTaskAsync(7, new List<DateTime> { new DateTime(2023, 3, 2) });
```

**`MergeTaskAsync(int|string|Guid, List<(int,int)>)`** - Merges task segments (3 overloads)
```csharp
await Gantt.MergeTaskAsync(3, new List<(int,int)> {(0, 1)});
```

**`ConvertToMilestone(string)`** - Converts task to milestone
```csharp
Gantt.ConvertToMilestone("1");
```

**`GetTaskByID(string)`** - Gets task by ID
```csharp
var task = Gantt.GetTaskByID("12");
```

**`GetRowTaskModel(TValue)`** - Gets Gantt task model
```csharp
var model = Gantt.GetRowTaskModel(taskData);
```

**`GetCriticalTasksAsync()`** - Gets critical tasks
```csharp
var critical = Gantt.GetCriticalTasksAsync();
```

**`GetCurrentViewRecords()`** - Gets visible records
```csharp
var visible = Gantt.GetCurrentViewRecords();
```

### Scrolling Operations

**`ScrollIntoViewAsync(int, int)`** - Scrolls row/column into view
```csharp
await Gantt.ScrollIntoViewAsync(2, 3);
```

**`ScrollToTaskbarAsync(int|string|Guid)`** - Scrolls to taskbar (3 overloads)
```csharp
await Gantt.ScrollToTaskbarAsync(12);
```

**`ScrollToTimelineAsync(DateTime)`** - Scrolls to date
```csharp
await Gantt.ScrollToTimelineAsync(new DateTime(2023, 12, 25));
```

### Zoom Operations

**`ZoomInAsync()`** - Increases zoom level
```csharp
await Gantt.ZoomInAsync();
```

**`ZoomOutAsync()`** - Decreases zoom level
```csharp
await Gantt.ZoomOutAsync();
```

**`ZoomToFitAsync()`** - Fits all taskbars in view
```csharp
await Gantt.ZoomToFitAsync();
```

**`ResetZoomAsync()`** - Resets to default zoom
```csharp
await Gantt.ResetZoomAsync();
```

### Timeline Operations

**`NextTimeSpan()`** - Extends timeline forward
```csharp
Gantt.NextTimeSpan();
```

**`PreviousTimeSpan()`** - Extends timeline backward
```csharp
Gantt.PreviousTimeSpan();
```

### Undo/Redo Operations

**`UndoAsync()`** - Undoes last action
```csharp
await Gantt.UndoAsync();
```

**`RedoAsync()`** - Redoes last undone action
```csharp
await Gantt.RedoAsync();
```

### Splitter Operations

**`SetSplitterPositionAsync(int|string|SplitterView)`** - Sets splitter position (3 overloads)
```csharp
await Gantt.SetSplitterPositionAsync(2);
await Gantt.SetSplitterPositionAsync("50%");
await Gantt.SetSplitterPositionAsync(SplitterView.Grid);
```

### Persistence Operations

**`GetPersistDataAsync()`** - Gets serialized state
```csharp
string state = await Gantt.GetPersistDataAsync();
```

**`SetPersistDataAsync(string)`** - Loads saved state
```csharp
await Gantt.SetPersistDataAsync(savedState);
```

**`ResetPersistDataAsync()`** - Resets to original state
```csharp
await Gantt.ResetPersistDataAsync();
```

### UI Operations

**`ShowSpinnerAsync()`** - Shows loading spinner
```csharp
await Gantt.ShowSpinnerAsync();
```

**`HideSpinnerAsync()`** - Hides loading spinner
```csharp
await Gantt.HideSpinnerAsync();
```

**`EnableItems(List<int>, bool)`** - Enables/disables toolbar items
```csharp
Gantt.EnableItems(new List<int> {0, 1}, true);
```

**`PreventRender(bool)`** - Prevents component re-render
```csharp
Gantt.PreventRender(true);
```

**`CopyAsync(bool?)`** - Copies selected data to clipboard
```csharp
await Gantt.CopyAsync(true);
```

### State Operations

**`CallStateHasChangedAsync()`** - Triggers async re-render
```csharp
await Gantt.CallStateHasChangedAsync();
```

**`UpdateProjectDates(DateTime?, DateTime?)`** - Updates project dates
```csharp
Gantt.UpdateProjectDates(new DateTime(2023, 1, 1), new DateTime(2023, 12, 31));
```

---

## Key Child Classes

### GanttTaskFields
Maps data model fields to Gantt functionality.

**Properties:** `Id`, `Name`, `StartDate`, `EndDate`, `Duration`, `Progress`, `ParentID`, `Dependency`, `Child`, `Work`, `ResourceInfo`, `Milestone`, `Notes`, `BaselineStartDate`, `BaselineEndDate`, `Manual`, `Segments`

### GanttColumn
Defines grid column properties.

**Properties:** `Field`, `HeaderText`, `Width`, `TextAlign`, `Format`, `Visible`, `Template`, `EditType`, `AllowEditing`, `AllowResizing`, `AllowReordering`, `IsFrozen`

### GanttEditSettings
Configures editing operations.

**Properties:** `AllowAdding`, `AllowEditing`, `AllowDeleting`, `AllowTaskbarEditing`, `Mode` (Dialog/Auto/Taskbar), `ShowDeleteConfirmDialog`, `NewRowPosition`

### GanttSelectionSettings
Controls selection behavior.

**Properties:** `Mode` (Row/Cell/Both), `Type` (Single/Multiple), `CellSelectionMode`, `EnableToggle`

### GanttFilterSettings / GanttSortSettings / GanttSearchSettings
Grid behavior configurations mirroring standard Grid settings.

### GanttTimelineSettings
Timeline display configuration.

**Properties:** `TimelineUnitSize`, `TimelineViewMode` (Hour/Day/Week/Month/Year), `WeekStartDay`, `UpdateTimescaleView`, `ShowTooltip`

### GanttTooltipSettings<TValue>
Tooltip configuration with template support.

**Properties:** `ShowTooltip`, `TaskbarTemplate`, `ConnectorLineTemplate`, `EditingTemplate`

---
