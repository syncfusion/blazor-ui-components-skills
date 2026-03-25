# Events in Syncfusion Blazor TreeGrid

> **Required namespace:** Always include `@using Syncfusion.Blazor.Grids;` to access event argument types like `ActionEventArgs`, `RowDataBoundEventArgs`, `BeforeCopyPasteEventArgs`, etc., and the `Action` enum used with `args.RequestType` (e.g. `Action.Sorting`, `Action.Save`). Missing this causes CS0103 "name does not exist" errors.
>
> **TreeGrid-specific namespace:** The following event arg types belong to `Syncfusion.Blazor.TreeGrid` and require `@using Syncfusion.Blazor.TreeGrid;` — **not** `Syncfusion.Blazor.Grids`:
> - `RowExpandingEventArgs<TValue>`, `RowExpandedEventArgs<TValue>`
> - `RowCollapsingEventArgs<TValue>`, `RowCollapsedEventArgs<TValue>`
> - `CheckBoxChangeEventArgs<TValue>`

## Table of Contents
- [TreeGridEvents Component Setup](#treegridevents-component-setup)
- [Lifecycle Events](#lifecycle-events)
- [Data Events](#data-events)
- [Row Events](#row-events)
- [Cell Events](#cell-events)
- [Selection Events](#selection-events)
- [Editing Events](#editing-events)
- [Action Events (Sort, Filter, Page, Search)](#action-events-sort-filter-page-search)
- [UI Interaction Events](#ui-interaction-events)
- [Export Events](#export-events)
- [Cancel Pattern](#cancel-pattern)
- [Complete Events Reference](#complete-events-reference)
- [Indentation Events](#indentation-events)

---

## TreeGridEvents Component Setup

All events are provided via the `<TreeGridEvents>` child component. **`TValue` is required** and must match the grid's `TValue`:

```razor
<SfTreeGrid DataSource="@TreeData"
            TValue="BusinessObject"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            TreeColumnIndex="1">
    
    <TreeGridEvents TValue="BusinessObject"
                    OnActionBegin="ActionBeginHandler"
                    OnActionComplete="ActionCompleteHandler"
                    RowDataBound="RowDataBoundHandler"
                    RowSelected="RowSelectedHandler">
    </TreeGridEvents>
    
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80"  IsPrimaryKey="true" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="200" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public List<BusinessObject> TreeData { get; set; }

    protected override void OnInitialized()
    {
        TreeData = BusinessObject.GetSelfDataSource();
    }

    public void ActionBeginHandler(ActionEventArgs<BusinessObject> args) { }
    public void ActionCompleteHandler(ActionEventArgs<BusinessObject> args) { }
    public void RowDataBoundHandler(RowDataBoundEventArgs<BusinessObject> args) { }
    public void RowSelectedHandler(RowSelectEventArgs<BusinessObject> args) { }
}
```

---

## Lifecycle Events

| Event | When it fires | Args Type |
|---|---|---|
| `OnLoad` | Before initial render | `object` |
| `Created` | After component is created and rendered | `object` |
| `Destroyed` | After component is destroyed | `object` |
| `DataBound` | After data is bound to the grid | `object` |
| `OnDataBound` | Before data binding completes | `BeforeDataBoundArgs<TValue>` |

```razor
<TreeGridEvents TValue="TaskItem"
                Created="OnGridCreated"
                DataBound="OnDataBound">
</TreeGridEvents>

@code {
    public void OnGridCreated(object args)
    {
        // Grid has rendered — safe to call grid methods here
    }

    public void OnDataBound(object args)
    {
        // Data is bound — rows are rendered
    }
}
```

---

## Data Events

| Event | When it fires | Args Type |
|---|---|---|
| `DataBound` | After all data is rendered | `object` |
| `OnDataBound` | Before data bound event | `BeforeDataBoundArgs<TValue>` |

---

## Row Events

| Event | When it fires | Args Type |
|---|---|---|
| `RowDataBound` | As each row is rendered | `RowDataBoundEventArgs<TValue>` |
| `DetailDataBound` | When detail row template renders | `DetailDataBoundEventArgs<TValue>` |
| `RowDropping` | Before row drop (drag-and-drop) | `RowDroppingEventArgs<TValue>` |
| `RowDropped` | After row drop completes | `RowDroppedEventArgs<TValue>` |
| `Expanding` | Before tree row expands | `RowExpandingEventArgs<TValue>` |
| `Expanded` | After tree row expands | `RowExpandedEventArgs<TValue>` |
| `Collapsing` | Before tree row collapses | `RowCollapsingEventArgs<TValue>` |
| `Collapsed` | After tree row collapses | `RowCollapsedEventArgs<TValue>` |

**RowDataBound — customize rows dynamically:**
```csharp
public void RowDataBoundHandler(RowDataBoundEventArgs<BusinessObject> args)
{
    if (args.Data.Priority == "Critical")
    {
        args.Row.AddStyle(new string[] { "background-color: #ff9999" });
    }
}
```

**`RowDataBoundEventArgs<T>` properties:**

| Property | Type | Notes |
|---|---|---|
| `Data` | `T?` | Row data |
| `Row` | `CellDOM?` | DOM row object (add styles/classes) |

**`RowDroppingEventArgs<T>` / `RowDroppedEventArgs<T>` properties:**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel *(RowDroppingEventArgs only)* |
| `Action` | `string?` | Drag action type |
| `Data` | `List<T>?` | Dragged row data |
| `FromIndex` | `int` | Source row index |
| `DropIndex` | `int` | Target row index |
| `Target` | `DOM?` | Target DOM element |
| `TargetDimension` | — | Target element dimensions |
| `ClientX` | `double` | Mouse X coordinate |
| `ClientY` | `double` | Mouse Y coordinate |

**`BeforeDataBoundArgs<T>` properties (OnDataBound):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel data binding |
| `Count` | `int` | Total number of records |
| `Result` | `List<T>?` | Current view data list |

**`RowExpandingEventArgs<T>` properties (Expanding):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel the expand action |
| `Data` | `T?` | Data object of the parent row being expanded |
| `Items` | `List<T>?` | List of child row data objects |
| `ExpandAll` | `bool` | Set `true` to expand all nested child rows of the current row |

**`RowExpandedEventArgs<T>` properties (Expanded):**

| Property | Type | Notes |
|---|---|---|
| `Data` | `T?` | Data object of the parent row that was expanded |
| `Items` | `List<T>?` | List of child row data objects |

**`RowCollapsingEventArgs<T>` properties (Collapsing):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel the collapse action |
| `Data` | `T?` | Data object of the parent row being collapsed |
| `Items` | `List<T>?` | List of child row data objects |
| `CollapseAll` | `bool` | Set `true` to collapse all nested child rows of the current row |

**`RowCollapsedEventArgs<T>` properties (Collapsed):**

| Property | Type | Notes |
|---|---|---|
| `Data` | `T?` | Data object of the parent row that was collapsed |
| `Items` | `List<T>?` | List of child row data objects |

**Expand/Collapse example:**
```csharp
public void ExpandingHandler(RowExpandingEventArgs<BusinessObject> args)
{
    // Cancel expand for rows that have no meaningful children
    if (args.Data.Duration == 0)
    {
        args.Cancel = true;
    }
}

public void CollapsingHandler(RowCollapsingEventArgs<BusinessObject> args)
{
    // Collapse all nested children along with the current row
    args.CollapseAll = true;
}
```

> ⚠️ `RowExpandingEventArgs<T>` and `RowCollapsingEventArgs<T>` are **generic** — always supply `<TValue>`. The `ExpandAll` property on `RowExpandingEventArgs<T>` and `CollapseAll` property on `RowCollapsingEventArgs<T>` are only effective when set in the respective `Expanding`/`Collapsing` event handler.
>
> ⚠️ **Namespace:** `RowExpandingEventArgs<T>`, `RowExpandedEventArgs<T>`, `RowCollapsingEventArgs<T>`, and `RowCollapsedEventArgs<T>` are defined in `Syncfusion.Blazor.TreeGrid` — add `@using Syncfusion.Blazor.TreeGrid;` to resolve them. Do **not** look for them in `Syncfusion.Blazor.Grids`.

---

## Cell Events

| Event | When it fires | Args Type |
|---|---|---|
| `QueryCellInfo` | As each cell is rendered | `QueryCellInfoEventArgs<TValue>` |
| `HeaderCellInfo` | As each header cell renders | `HeaderCellInfoEventArgs` |
| `BeforeCopyPaste` | Before clipboard copy/paste | `BeforeCopyPasteEventArgs` |
| `BeforeCellPaste` | Before cell paste (AutoFill) | `BeforeCellPasteEventArgs<TValue>` |

**QueryCellInfo — customize cells dynamically:**
```csharp
public void QueryCellInfoHandler(QueryCellInfoEventArgs<BusinessObject> args)
{
    if (args.Column.Field == "Progress" && args.Data.Progress < 25)
    {
        args.Cell.AddStyle(new string[] { "color: red; font-weight: bold" });
    }
}
```

**`QueryCellInfoEventArgs<T>` properties:**

| Property | Type | Notes |
|---|---|---|
| `Cell` | `CellDOM?` | DOM cell object (add styles/classes) |
| `Column` | `GridColumn?` | Column object |
| `Data` | `T?` | Row data |
| `ForeignKeyData` | `IDictionary<string, IEnumerable<object>>?` | Foreign key data |

**`HeaderCellInfoEventArgs` properties:**

| Property | Type | Notes |
|---|---|---|
| `Cell` | `CellDOM?` | DOM header cell object |
| `Column` | `GridColumn?` | Column object |

**`BeforeCopyPasteEventArgs` properties:**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel |
| `Action` | `string?` | `"Copy"` or `"Paste"` |
| `ClipboardText` | `string?` | Clipboard text content |

**`BeforeCellPasteEventArgs<T>` properties:**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel |
| `CellValue` | `string?` | Value being pasted (can be modified) |
| `ColumnName` | `string?` | Field name of the target cell |
| `ColumnIndex` | `int` | Column index |
| `Data` | `T?` | Row data of the target cell |
| `RowIndex` | `int` | Row index |

> ⚠️ `BeforeCellPasteEventArgs<T>` is **generic** — always use `BeforeCellPasteEventArgs<TValue>`. There is no non-generic `BeforeCellPasteEventArgs`.

---

## Selection Events

| Event | When it fires | Cancel? | Args Type |
|---|---|---|---|
| `RowSelecting` | Before a row is selected | ✅ | `RowSelectingEventArgs<TValue>` |
| `RowSelected` | After a row is selected | ❌ | `RowSelectEventArgs<TValue>` |
| `RowDeselecting` | Before a row is deselected | ✅ | `RowDeselectEventArgs<TValue>` |
| `RowDeselected` | After a row is deselected | ❌ | `RowDeselectEventArgs<TValue>` |
| `CellSelecting` | Before a cell is selected | ✅ | `CellSelectingEventArgs<TValue>` |
| `CellSelected` | After a cell is selected | ❌ | `CellSelectEventArgs<TValue>` |
| `CellDeselecting` | Before a cell is deselected | ✅ | `CellDeselectEventArgs<TValue>` |
| `CellDeselected` | After a cell is deselected | ❌ | `CellDeselectEventArgs<TValue>` |
| `OnRecordDoubleClick` | On row double-click | ❌ | `RecordDoubleClickEventArgs<TValue>` |
| `CheckboxChange` | When a checkbox in checkbox-selection mode changes state | ❌ | `CheckBoxChangeEventArgs<TValue>` |

**Cancel a row selection:**
```csharp
public void RowSelectingHandler(RowSelectingEventArgs<BusinessObject> args)
{
    if (args.Data.TaskId == 5)
    {
        args.Cancel = true; // Prevent selection of row with TaskId == 5
    }
}
```

> ⚠️ `RowDeselecting` and `RowDeselected` both use the **same** class `RowDeselectEventArgs<T>`. There is no separate `RowDeselectingEventArgs<T>`.
> ⚠️ `CellDeselecting` and `CellDeselected` both use the **same** class `CellDeselectEventArgs<T>`. There is no separate `CellDeselectingEventArgs<T>`.

**`RowSelectingEventArgs<T>` properties (RowSelecting):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel |
| `Event` | `MouseEventArgs?` | Mouse event |
| `Data` | `T?` | Single selected row data |
| `Datas` | `List<T>?` | Multiple selected rows data |
| `RowIndex` | `int` | Row index |
| `RowIndexes` | `List<int>?` | Multiple row indexes |
| `PreviousRowIndex` | `int` | Previous row index |
| `ForeignKeyData` | `IDictionary<string, IEnumerable<object>>?` | Foreign key data |
| `IsCtrlPressed` | `bool` | Ctrl key held |
| `IsShiftPressed` | `bool` | Shift key held |
| `IsInteracted` | `bool` | Triggered by user interaction |
| `IsHeaderCheckboxClicked` | `bool` | Header checkbox clicked |

**`RowSelectEventArgs<T>` properties (RowSelected):**

| Property | Type | Notes |
|---|---|---|
| `Event` | `MouseEventArgs?` | Mouse event |
| `Data` | `T?` | Single selected row data |
| `Datas` | `List<T>?` | Multiple selected rows data |
| `RowIndex` | `int` | Row index |
| `RowIndexes` | `List<int>?` | Multiple row indexes |
| `PreviousRowIndex` | `int` | Previous row index |
| `ForeignKeyData` | `IDictionary<string, IEnumerable<object>>?` | Foreign key data |
| `IsCtrlPressed` | `bool` | Ctrl key held |
| `IsShiftPressed` | `bool` | Shift key held |
| `IsInteracted` | `bool` | Triggered by user interaction |
| `IsHeaderCheckboxClicked` | `bool` | Header checkbox clicked |

**`RowDeselectEventArgs<T>` properties (RowDeselecting / RowDeselected):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel (effective in RowDeselecting) |
| `Event` | `MouseEventArgs?` | Mouse event |
| `Data` | `T?` | Single deselected row data |
| `Datas` | `List<T>?` | Multiple deselected rows data |
| `RowIndex` | `int` | Row index |
| `RowIndexes` | `List<int>?` | Multiple row indexes |
| `ForeignKeyData` | `IDictionary<string, IEnumerable<object>>?` | Foreign key data |
| `IsCtrlPressed` | `bool` | Ctrl key held |
| `IsShiftPressed` | `bool` | Shift key held |
| `IsInteracted` | `bool` | Triggered by user interaction |
| `IsHeaderCheckboxClicked` | `bool` | Header checkbox clicked |

**`CellSelectingEventArgs<T>` properties (CellSelecting):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel |
| `Event` | `MouseEventArgs?` | Mouse event |
| `CellIndex` | `int` | Cell index |
| `RowIndex` | `int` | Row index |
| `Data` | `T?` | Row data |
| `IsCtrlPressed` | `bool` | Ctrl key held |
| `IsShiftPressed` | `bool` | Shift key held |

**`CellSelectEventArgs<T>` properties (CellSelected):**

| Property | Type | Notes |
|---|---|---|
| `Event` | `MouseEventArgs?` | Mouse event |
| `CellIndex` | `int` | Cell index |
| `RowIndex` | `int` | Row index |
| `Data` | `T?` | Row data |
| `CurrentValue` | `object?` | Value of the selected cell |
| `PreviousValue` | `object?` | Value of the previously selected cell |
| `PreviousCellIndex` | `int` | Previously selected cell index |
| `IsCtrlPressed` | `bool` | Ctrl key held |
| `IsShiftPressed` | `bool` | Shift key held |

**`CellDeselectEventArgs<T>` properties (CellDeselecting / CellDeselected):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel (effective in CellDeselecting) |
| `Event` | `MouseEventArgs?` | Mouse event |
| `CellIndex` | `int` | Cell index |
| `RowIndex` | `int` | Row index |
| `Data` | `T?` | Row data |
| `IsCtrlPressed` | `bool` | Ctrl key held |
| `IsShiftPressed` | `bool` | Shift key held |

**`RecordDoubleClickEventArgs<T>` properties (OnRecordDoubleClick):**

| Property | Type | Notes |
|---|---|---|
| `CellIndex` | `int` | Cell index |
| `Column` | `GridColumn?` | Column object |
| `RowData` | `T?` | Row data |
| `RowIndex` | `int` | Row index |
| `CurrentCell` | `CellDOM?` | DOM cell object |
| `CellValue` | `object?` | Cell value |
| `ForeignKeyData` | `IDictionary<string, IEnumerable<object>>?` | Foreign key data |

**`CheckBoxChangeEventArgs<T>` properties (CheckboxChange):**

> ⚠️ **Namespace:** `CheckBoxChangeEventArgs<T>` is defined in `Syncfusion.Blazor.TreeGrid` — add `@using Syncfusion.Blazor.TreeGrid;` to resolve it. Do **not** look for it in `Syncfusion.Blazor.Grids`.

| Property | Type | Notes |
|---|---|---|
| `Checked` | `bool` | `true` if the checkbox is checked; `false` if unchecked |
| `SelectedRowIndex` | `int` | Index of the row whose checkbox changed |
| `SelectedRowIndexes` | `List<int>?` | Indexes of all currently selected rows |

---

## Editing Events

| Event | When it fires | Cancel? | Args Type |
|---|---|---|---|
| `OnBeginEdit` | Before edit mode starts | ✅ | `BeginEditArgs<TValue>` |
| `OnCellEdit` | Before a cell enters edit mode | ✅ | `CellEditArgs<TValue>` |
| `OnCellSave` | Before a cell value is saved | ✅ | `CellSaveArgs<TValue>` |
| `CellSaved` | After a cell value is saved | ❌ | `CellSavedArgs<TValue>` |
| `RowCreating` | Before a new row is added | ✅ | `RowCreatingEventArgs<TValue>` |
| `RowCreated` | After a new row is added | ❌ | `RowCreatedEventArgs<TValue>` |
| `RowUpdating` | Before a row update is saved | ✅ | `RowUpdatingEventArgs<TValue>` |
| `RowUpdated` | After a row update is saved | ❌ | `RowUpdatedEventArgs<TValue>` |
| `RowDeleting` | Before a row is deleted | ✅ | `RowDeletingEventArgs<TValue>` |
| `RowDeleted` | After a row is deleted | ❌ | `RowDeletedEventArgs<TValue>` |
| `RowEditing` | While editing in dialog mode | ✅ | `RowEditingEventArgs<TValue>` |
| `RowEdited` | After edit dialog is saved | ❌ | `RowEditedEventArgs<TValue>` |
| `OnBatchAdd` | Before batch add row | ✅ | `BeforeBatchAddArgs<TValue>` |
| `OnBatchSave` | Before batch save | ✅ | `BeforeBatchSaveArgs<TValue>` |
| `OnBatchDelete` | Before batch delete | ✅ | `BeforeBatchDeleteArgs<TValue>` |
| `OnBatchCancel` | Before batch cancel | ✅ | `BeforeBatchCancelArgs<TValue>` |
| `IndentationChanging` | Before a row is indented or outdented | ✅ | `IndentationChangingEventArgs<TValue>` |
| `IndentationChanged` | After a row is indented or outdented | ❌ | `IndentationChangedEventArgs<TValue>` |

**Validate before save:**
```csharp
public void RowUpdatingHandler(RowUpdatingEventArgs<BusinessObject> args)
{
    if (args.Data.Duration < 0)
    {
        args.Cancel = true; // Prevent saving invalid data
    }
}
```

**⚠️ IMPORTANT - OnBeginEdit Property Usage:**

Use `args.RowData.PropertyName` (not `args.Data`) to access row data:

```csharp
// ✅ CORRECT
private void OnBeginEditHandler(BeginEditArgs<MyItem> args)
{
    var taskName = args.RowData.TaskName;  // Use RowData
}

// ❌ WRONG
private void OnBeginEditHandler(BeginEditArgs<MyItem> args)
{
    // var taskName = args.Data.TaskName;  // Do NOT use Data
}
```

**`BeginEditArgs<T>` properties (OnBeginEdit):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel edit |
| `PrimaryKey` | `string[]?` | Field names with IsPrimaryKey=true |
| `PrimaryKeyValue` | `string[]?` | Primary key values |
| `RowData` | `T?` | Data of the row being edited (use this, not `Data`) |
| `RowIndex` | `int` | Row index |

**`CellEditArgs<T>` properties (OnCellEdit):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel cell edit |
| `ColumnName` | `string?` | Field name of the cell being edited |
| `Column` | `GridColumn?` | Column object |
| `RowData` | `T?` | Original row data |
| `Data` | `T?` | Changed row data |
| `Value` | `string?` | Cell value |
| `ForeignKeyData` | `IDictionary<string, IEnumerable<object>>?` | Foreign key data |
| `IsForeignKey` | `bool` | Whether cell is a foreign key column |
| `PrimaryKey` | `string[]?` | Primary key field names |
| `EditContext` | `EditContext?` | Edit context |
| `ValidationRules` | `ValidationRules?` | Validation rules |

**`CellSaveArgs<T>` / `CellSavedArgs<T>` properties (OnCellSave / CellSaved):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel *(CellSaveArgs only)* |
| `ColumnName` | `string?` | Field name of the edited cell |
| `Column` | `GridColumn?` | Column object |
| `RowData` | `T?` | Original row data |
| `Data` | `T?` | Changed row data |
| `Value` | `object?` | Current cell value |
| `PreviousValue` | `object?` | Previously edited value |
| `IsForeignKey` | `bool` | Whether cell is a foreign key column |
| `CellInfo` | `CellDOM?` | DOM cell object |

**`RowCreatingEventArgs<T>` / `RowCreatedEventArgs<T>` properties:**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel *(RowCreatingEventArgs only)* |
| `Data` | `T?` | Data of the new row |
| `Index` | `int` | Row index |
| `EditContext` | `EditContext?` | Edit context *(internal on RowCreatedEventArgs)* |

**`RowUpdatingEventArgs<T>` / `RowUpdatedEventArgs<T>` properties:**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel *(RowUpdatingEventArgs only)* |
| `IsShiftKeyPressed` | `bool` | Whether Shift was pressed *(RowUpdatingEventArgs only)* |
| `KeyCode` | `string?` | Key pressed to save (`Enter`/`Tab`) *(RowUpdatingEventArgs only)* |
| `Data` | `T?` | Current row data |
| `PreviousData` | `T?` | Previous row data |
| `Index` | `int` | Row index |
| `Action` | `SaveActionType` | `Added` or `Edited` |
| `PrimaryKeys` | `string[]?` | Primary key field names |
| `PrimaryKeyValue` | `object?` | Primary key value |

**⚠️ IMPORTANT - RowDeleting Property Usage:**

Use `args.Datas[0].PropertyName` (not `args.Data`) to access deleted row data:

```csharp
// ✅ CORRECT
private void RowDeletingHandler(RowDeletingEventArgs<MyItem> args)
{
    if (args.Datas[0].HasChildren) 
    {
        args.Cancel = true;  // Prevent delete if row has children
    }
}

// ❌ WRONG
private void RowDeletingHandler(RowDeletingEventArgs<MyItem> args)
{
    // if (args.Data.HasChildren) { }  // Do NOT use Data
}
```

**`RowDeletingEventArgs<T>` / `RowDeletedEventArgs<T>` properties:**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel *(RowDeletingEventArgs only)* |
| `Datas` | `List<T>?` | Collection of rows being deleted (use `Datas[0]`, not `Data`) |
| `PrimaryKeys` | `string[]?` | Primary key field names |

**`RowEditingEventArgs<T>` / `RowEditedEventArgs<T>` properties:**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel *(RowEditingEventArgs only)* |
| `Data` | `T?` | Row data being edited |
| `Index` | `int` | Row index |
| `PrimaryKeys` | `string[]?` | Primary key field names |
| `PrimaryKeyValue` | `object?` | Primary key value |
| `EditContext` | `EditContext?` | Edit context |
| `ForeignKeyData` | `IDictionary<string, IEnumerable<object>>?` | Foreign key data |

**`BeforeBatchAddArgs<T>` properties (OnBatchAdd):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel |
| `DefaultData` | `T?` | Custom default row data |
| `Index` | `int` | Row index |
| `PrimaryKey` | `string[]?` | Primary key field names |
| `EditContext` | `EditContext?` | Edit context |

**`BeforeBatchDeleteArgs<T>` properties (OnBatchDelete):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel |
| `RowData` | `T?` | Data of the row being deleted |
| `RowIndex` | `int` | Row index |
| `PrimaryKey` | `string[]?` | Primary key field names |

**`BeforeBatchSaveArgs<T>` properties (OnBatchSave):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel |
| `BatchChanges` | `BatchChanges<T>?` | All pending add/edit/delete records |

**`BeforeBatchCancelArgs<T>` properties (OnBatchCancel):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel |
| `BatchChanges` | `BatchChanges<T>?` | All pending add/edit/delete records |

> ⚠️ Batch editing event args use the `BeforeBatch*` prefix: `BeforeBatchAddArgs<T>`, `BeforeBatchDeleteArgs<T>`, `BeforeBatchSaveArgs<T>`, `BeforeBatchCancelArgs<T>`. Do **not** use `BatchAddArgs`, `BatchDeleteArgs`, or `BatchSaveArgs` — those do not exist.

**`IndentationChangedEventArgs<T>` properties (IndentationChanging / IndentationChanged base):**

| Property | Type | Notes |
|---|---|---|
| `Data` | `T?` | Data of the row being indented or outdented |
| `Index` | `int` | Row index of the affected row |
| `Level` | `int` | Hierarchical level of the row after the indentation action |
| `IsIndent` | `bool` | `true` if the action is an indent; `false` if it is an outdent |

**`IndentationChangingEventArgs<T>` additional properties (IndentationChanging only):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel the indent or outdent action |

> ⚠️ `IndentationChangingEventArgs<T>` extends `IndentationChangedEventArgs<T>` — it exposes all base properties plus `Cancel`.

**Indentation example:**
```csharp
public void IndentationChangingHandler(IndentationChangingEventArgs<BusinessObject> args)
{
    // Prevent indenting beyond level 2
    if (args.IsIndent && args.Level >= 2)
    {
        args.Cancel = true;
    }
}

public void IndentationChangedHandler(IndentationChangedEventArgs<BusinessObject> args)
{
    Console.WriteLine($"Row {args.Index} moved to level {args.Level}. IsIndent: {args.IsIndent}");
}
```

---

## Action Events (Sort, Filter, Page, Search)

`OnActionBegin` and `OnActionComplete` fire for ALL grid actions. Use `args.RequestType` to identify the action:

```csharp
public void ActionBeginHandler(ActionEventArgs<BusinessObject> args)
{
    switch (args.RequestType)
    {
        case Action.Sorting:
            // About to sort
            break;
        case Action.Filtering:
            // About to filter
            break;
        case Action.Paging:
            // About to change page
            break;
        case Action.Add:
            // About to add a new row
            break;
        case Action.Save:
            // About to save an edit
            break;
        case Action.Delete:
            // About to delete a row
            break;
        case Action.Search:
            // About to search
            break;
    }
}
```

**`ActionEventArgs<T>` properties (OnActionBegin / OnActionComplete):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel the action |
| `RequestType` | `Action` | The type of action being performed (see table below) |
| `Action` | `string?` | Action sub-type (`"Add"`, `"Edit"`, `"Delete"`) |
| `Data` | `T?` | Row data relevant to the action |
| `RowData` | `T?` | Current row data |
| `PreviousData` | `T?` | Previous row data |
| `RowIndex` | `int` | Row index |
| `ColumnName` | `string?` | Column field name (sort/filter/group) |
| `Direction` | `SortDirection` | Sort direction |
| `CurrentFilterObject` | `PredicateModel<object>?` | Current filter predicate |
| `CurrentFilteringColumn` | `string?` | Column field currently being filtered |
| `Columns` | `List<PredicateModel<object>>?` | All filter predicates (Excel/CheckBox) |
| `CurrentPage` | `int` | Current page number |
| `PreviousPage` | `int` | Previous page number |
| `Index` | `int` | Row index for add/edit |
| `PrimaryKeys` | `string[]?` | Primary key field names |
| `PrimaryKeyValue` | `object?` | Primary key value |
| `Code` | `string?` | Key pressed to save (`"Enter"`/`"Tab"`) |
| `IsShiftKeyPressed` | `bool` | Shift+Tab pressed |
| `SearchString` | `string?` | Search string |
| `PreventDataClone` | `bool` | Prevent data object cloning on edit |
| `PreventFilterQuery` | `bool` | Suppress default filter query to server |
| `EditContext` | `EditContext?` | Edit context |
| `CheckboxListData` | `IEnumerable<object>?` | Custom data for checkbox/excel filter |
| `FromColumns` | `List<GridColumn>?` | Columns being reordered |
| `ToColumn` | `GridColumn?` | Destination column for reorder |
| `VisibleColumns` | `List<GridColumn>?` | Visible columns (ColumnState action) |
| `HiddenColumns` | `List<GridColumn>?` | Hidden columns (ColumnState action) |
| `Type` | `string?` | Event type string |
| `SelectedRow` | `int` | Currently selected row index |

**RequestType enum values for `OnActionBegin`:**

> ⚠️ **Namespace:** The `Action` enum (`Action.Sorting`, `Action.Save`, etc.) is defined in `Syncfusion.Blazor.Grids`. Always add `@using Syncfusion.Blazor.Grids;` — or use the fully-qualified form `Syncfusion.Blazor.Grids.Action.Save` — to avoid CS0103 errors.

| RequestType | Trigger |
|---|---|
| `Action.Sorting` | Column sort initiated |
| `Action.Filtering` | Filter applied |
| `Action.Paging` | Page change |
| `Action.Add` | Add row toolbar button |
| `Action.Save` | Save button in edit mode |
| `Action.Delete` | Delete toolbar button |
| `Action.Cancel` | Cancel button in edit mode |
| `Action.Search` | Toolbar search |
| `Action.ColumnState` | Column visibility change |
| `Action.Reorder` | Column reorder |

**Dedicated action events (fire independently):**

| Event | Action | Args Type |
|---|---|---|
| `Sorting` | Before column sort | `SortingEventArgs` |
| `Sorted` | After column sort | `SortedEventArgs` |
| `Filtering` | Before filter applied | `FilteringEventArgs` |
| `Filtered` | After filter applied | `FilteredEventArgs` |
| `FilterDialogOpening` | Before filter dialog opens | `FilterDialogOpeningEventArgs` |
| `FilterDialogOpened` | After filter dialog opens | `FilterDialogOpenedEventArgs` |
| `CheckboxFilterSearching` | In checkbox filter search box | `CheckboxFilterSearchingEventArgs` |
| `Searching` | Before toolbar search | `SearchingEventArgs` |
| `Searched` | After toolbar search | `SearchedEventArgs` |

**`SortingEventArgs` / `SortedEventArgs` properties:**

| Property | Type | Notes |
|---|---|---|
| `ColumnName` | `string?` | Field name of the column being sorted |
| `Direction` | `SortDirection` | `Ascending`, `Descending`, or `None` |
| `Action` | `NotifyCollectionChangedAction` | `Add`, `Remove`, `Replace`, or `Reset` |
| `SortedColumns` | `List<SortColumn>?` | All sorted columns (populated via `SortColumnsAsync`) |
| `Cancel` | `bool` | Set `true` to cancel *(SortingEventArgs only)* |
| `IsCtrlKeyPressed` | `bool` | Whether Ctrl was held for multi-sort *(SortingEventArgs only)* |

> ⚠️ There is **no `Column` property** on `SortingEventArgs` or `SortedEventArgs`. Use `ColumnName` (a `string`) instead.

**⚠️ IMPORTANT - Filtering Events Property Usage:**

Use `args.ColumnName` (not `args.Columns`) to get the column being filtered:

```csharp
// ✅ CORRECT
private void FilteringHandler(FilteringEventArgs args)
{
    var columnName = args.ColumnName;  // Use ColumnName - returns string
    Console.WriteLine($"Filtering column: {columnName}");
}

// ❌ WRONG
private void FilteringHandler(FilteringEventArgs args)
{
    // var field = args.Columns[0].Field;    // Property doesn't exist
    // var value = args.Columns[0].Value;    // Property doesn't exist
}
```

**`FilteringEventArgs` / `FilteredEventArgs` properties:**

| Property | Type | Notes |
|---|---|---|
| `ColumnName` | `string?` | Field name of the column being filtered (use this, not `Columns`) |
| `FilterPredicates` | `List<PredicateModel<object>>?` | Current filter predicates for the column |
| `Cancel` | `bool` | Set `true` to cancel *(FilteringEventArgs only)* |
| `PreventFilterQuery` | `bool` | Suppress default filter query to server *(FilteringEventArgs only)* |

**`SearchingEventArgs` / `SearchedEventArgs` properties:**

| Property | Type | Notes |
|---|---|---|
| `SearchText` | `string` | The search text entered in the toolbar |
| `Cancel` | `bool` | Set `true` to cancel *(SearchingEventArgs only)* |

**⚠️ IMPORTANT - FilterDialog Events Property Usage:**

Use `args.ColumnName` (not `args.Column`) to get the column whose dialog is opening/opened:

```csharp
// ✅ CORRECT
private void FilterDialogOpeningHandler(FilterDialogOpeningEventArgs args)
{
    var columnName = args.ColumnName;  // Use ColumnName - returns string
    Console.WriteLine($"Filter dialog opening for column: {columnName}");
}

// ❌ WRONG
private void FilterDialogOpeningHandler(FilterDialogOpeningEventArgs args)
{
    // var column = args.Column.Field;     // Property doesn't exist
}
```

**`FilterDialogOpeningEventArgs` properties (FilterDialogOpening):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel dialog opening |
| `ColumnName` | `string?` | Column field name (use this, not `Column`) |
| `CheckboxListData` | `IEnumerable<object>?` | Custom data for checkbox/excel filter |
| `FilterChoiceCount` | `int` | Max items in filter popup |
| `FilterOperators` | `List<IFilterOperator>?` | Custom filter operators (Menu filter) |

**`FilterDialogOpenedEventArgs` properties (FilterDialogOpened):**

| Property | Type | Notes |
|---|---|---|
| `ColumnName` | `string?` | Column field name |
| `CheckboxListData` | `IEnumerable<object>?` | Data in the filter popup |
| `FilterChoiceCount` | `int` | Number of items displayed |

**`CheckboxFilterSearchingEventArgs` properties (CheckboxFilterSearching):**

| Property | Type | Notes |
|---|---|---|
| `CheckboxListData` | `IEnumerable<object>?` | Custom data for filter (can be set) |
| `ColumnName` | `string?` | Column field name |
| `SearchText` | `string` | Text typed in checkbox filter search box |

**Sort event example:**

```csharp
private void OnSortingHandler(SortingEventArgs args)
{
    // ✅ Correct: use ColumnName (string)
    Console.WriteLine($"Sorting by: {args.ColumnName}, Direction: {args.Direction}");

    // Cancel sort on a protected column
    if (args.ColumnName == "TaskId")
        args.Cancel = true;
}

private void OnSortedHandler(SortedEventArgs args)
{
    Console.WriteLine($"Sort done. Column: {args.ColumnName}, Action: {args.Action}");
}
```

---

## UI Interaction Events

| Event | When it fires | Args Type |
|---|---|---|
| `OnToolbarClick` | Toolbar button clicked | `ClickEventArgs` |
| `CommandClicked` | Command column button clicked | `CommandClickEventArgs<TValue>` |
| `ContextMenuItemClicked` | Context menu item clicked | `ContextMenuClickEventArgs<TValue>` |
| `ContextMenuOpen` | Before context menu opens | `ContextMenuOpenEventArgs<TValue>` |
| `ColumnMenuItemClicked` | Column menu item clicked | `ColumnMenuClickEventArgs` |
| `OnResizeStart` | Before column resize starts | `ResizeArgs` |
| `ResizeStopped` | After column resize ends | `ResizeArgs` |
| `ColumnReordering` | Before column is reordered | `ColumnReorderingEventArgs` |
| `ColumnReordered` | After column is reordered | `ColumnReorderedEventArgs` |
| `ColumnsStateChanging` | Before column show/hide | `ColumnVisibilityChangingEventArgs` |
| `ColumnsStateChanged` | After column show/hide | `ColumnVisibilityChangedEventArgs` |

**`CommandClickEventArgs<T>` properties (CommandClicked):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel the CUD (Create/Update/Delete) action triggered by the command button |
| `CommandColumn` | `CommandModel?` | The command button definition (see `CommandModel` below) |
| `RowData` | `T?` | Original row data of the row containing the command button |
| `EditedData` | `T?` | Modified row data when saving via command column; `null` when editing, deleting, or cancelling |

> ⚠️ **No `RowIndex` property:** `CommandClickEventArgs<T>` does **not** expose a `RowIndex`. Use `args.RowData` to identify the row. Using `args.RowIndex` will cause a compile error.

**`CommandModel` properties:**

| Property | Type | Notes |
|---|---|---|
| `ID` | `string?` | Command button ID — use `args.CommandColumn?.ID` (**uppercase**, not `.Id`) |
| `Title` | `string?` | Tooltip text for the command button |
| `Type` | `CommandButtonType` | Built-in type: `Edit`, `Delete`, `Save`, `Cancel`, or `None` |
| `ButtonOption` | `CommandButtonOptions?` | Additional button appearance options (icon CSS, content, etc.) |
| `Uid` | `string?` | Internal unique identifier (set internally, read-only) |

**ContextMenuOpen — conditionally hide items:**
```csharp
public void ContextMenuOpenHandler(ContextMenuOpenEventArgs<BusinessObject> args)
{
    // Hide "Delete" for root rows
    if (args.RowInfo?.RowData?.ParentId == null)
    {
        args.HideItems = new List<string> { "Delete" };
    }
}
```

**`ResizeArgs` properties (OnResizeStart / ResizeStopped):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel resize |
| `Column` | `GridColumn?` | Column being resized |

**`ColumnReorderingEventArgs` properties (ColumnReordering):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel |
| `ReorderingColumns` | `List<GridColumn>?` | Columns being moved |
| `ToColumn` | `GridColumn?` | Destination column |

**`ColumnReorderedEventArgs` properties (ColumnReordered):**

| Property | Type | Notes |
|---|---|---|
| `ReorderingColumns` | `List<GridColumn>?` | Columns that were moved |
| `ToColumn` | `GridColumn?` | Destination column |

**`ColumnVisibilityChangingEventArgs` properties (ColumnsStateChanging):**

| Property | Type | Notes |
|---|---|---|
| `Cancel` | `bool` | Set `true` to cancel |
| `VisibleColumns` | `List<GridColumn>?` | Columns becoming visible |
| `HiddenColumns` | `List<GridColumn>?` | Columns becoming hidden |

**`ColumnVisibilityChangedEventArgs` properties (ColumnsStateChanged):**

| Property | Type | Notes |
|---|---|---|
| `VisibleColumns` | `List<GridColumn>?` | Visible columns |
| `HiddenColumns` | `List<GridColumn>?` | Hidden columns |

---

## Export Events

| Event | When it fires | Args Type |
|---|---|---|
| `OnPdfExport` | Before PDF export | `object` |
| `PdfQueryCellInfoEvent` | For each cell during PDF export | `PdfQueryCellInfoEventArgs<TValue>` |
| `OnExcelExport` | Before Excel export | `object` |
| `ExcelQueryCellInfoEvent` | For each cell during Excel export | `ExcelQueryCellInfoEventArgs<TValue>` |

```csharp
// Customize cell styling in exported Excel
public void ExcelQueryCellInfoHandler(ExcelQueryCellInfoEventArgs<BusinessObject> args)
{
    if (args.Column?.Field == "Priority" && args.Value?.ToString() == "Critical")
    {
        args.Style = new ExcelStyle { BackColor = "#FF0000", FontColor = "#FFFFFF" };
    }
}
```

**`PdfQueryCellInfoEventArgs<T>` properties:**

| Property | Type | Notes |
|---|---|---|
| `Data` | `T?` | Row data |
| `Column` | `GridColumn?` | Column object |
| `Value` | `object?` | Cell value |
| `ColSpan` | `int` | Column span |
| `Row` | `PdfGridRow?` | PDF grid row |
| `Cell` | `PdfGridCell?` | PDF grid cell |
| `PdfGridColumn` | — | PDF column |
| `RowIndex` | `int` | Row index |
| `ColumnIndex` | `int` | Column index |
| `Style` | `PdfGridCellStyle?` | Cell style |

**`ExcelQueryCellInfoEventArgs<T>` properties:**

| Property | Type | Notes |
|---|---|---|
| `Data` | `T?` | Row data |
| `Column` | `GridColumn?` | Column object |
| `Value` | `object?` | Cell value |
| `ColSpan` | `int` | Column span |
| `Cell` | `Cell?` | Excel cell object |
| `Style` | `CellStyle?` | Cell style |
| `RowIndex` | `int` | Row index |
| `ColumnIndex` | `int` | Column index |

> ⚠️ `PdfQueryCellInfoEventArgs<T>` and `ExcelQueryCellInfoEventArgs<T>` are **generic** — always use `...<TValue>`. The `OnPdfExport` and `OnExcelExport` events receive `object` args (export properties are passed via method parameters, not event args).

---

## Cancel Pattern

Many `*ing` (before-action) events support cancellation via `args.Cancel = true`:

```csharp
// Cancel pattern — works for RowSelecting, RowCreating, RowUpdating,
// RowDeleting, OnBeginEdit, OnCellEdit, OnCellSave, etc.
public void RowDeletingHandler(RowDeletingEventArgs<BusinessObject> args)
{
    // Prevent deletion of parent tasks that still have children
    if (args.Data.Any(d => d.ParentId == null))
    {
        args.Cancel = true;
    }
}
```

> **Rule:** `*ing` events fire **before** the action (can cancel). `*ed` events fire **after** (cannot cancel).

---

## Complete Events Reference

All 76 events available on `<TreeGridEvents TValue="...">`:

| Event | Category |
|---|---|
| `OnLoad`, `Created`, `Destroyed` | Lifecycle |
| `OnDataBound`, `DataBound` | Data |
| `RowDataBound`, `DetailDataBound` | Row render |
| `HeaderCellInfo`, `QueryCellInfo` | Cell render |
| `OnBeginEdit`, `OnCellEdit`, `OnCellSave`, `CellSaved` | Cell editing |
| `OnBatchAdd`, `OnBatchSave`, `OnBatchDelete`, `OnBatchCancel` | Batch editing |
| `RowEditing`, `RowEdited` | Row editing |
| `RowCreating`, `RowCreated` | Row add |
| `RowUpdating`, `RowUpdated` | Row update |
| `RowDeleting`, `RowDeleted` | Row delete |
| `IndentationChanging`, `IndentationChanged` | Indentation (indent/outdent) |
| `RowSelecting`, `RowSelected`, `RowDeselecting`, `RowDeselected` | Row selection |
| `CellSelecting`, `CellSelected`, `CellDeselecting`, `CellDeselected` | Cell selection |
| `CheckboxChange` | Checkbox selection |
| `OnRecordDoubleClick` | Mouse |
| `Sorting`, `Sorted` | Sort |
| `Filtering`, `Filtered`, `FilterDialogOpening`, `FilterDialogOpened`, `CheckboxFilterSearching` | Filter |
| `Searching`, `Searched` | Search |
| `OnActionBegin`, `OnActionComplete`, `OnActionFailure` | All actions |
| `Expanding`, `Expanded` | Tree expand |
| `Collapsing`, `Collapsed` | Tree collapse |
| `RowDropping`, `RowDropped` | Row drag-drop |
| `BeforeCopyPaste`, `BeforeCellPaste` | Clipboard |
| `OnToolbarClick` | Toolbar |
| `CommandClicked` | Command column |
| `ContextMenuOpen`, `ContextMenuItemClicked` | Context menu |
| `ColumnMenuItemClicked` | Column menu |
| `OnResizeStart`, `ResizeStopped` | Column resize |
| `ColumnReordering`, `ColumnReordered` | Column reorder |
| `ColumnsStateChanging`, `ColumnsStateChanged` | Column state (show/hide) |
| `OnPdfExport`, `PdfQueryCellInfoEvent` | PDF export |
| `OnExcelExport`, `ExcelQueryCellInfoEvent` | Excel export |

---
