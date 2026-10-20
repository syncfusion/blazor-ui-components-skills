# Editing Patterns — Syncfusion Blazor DataGrid

Advanced editing behaviors, row-level controls, and UI patterns.

## Table of Contents

- [Cancel Edit Based on Condition](#cancel-edit-based-on-condition)
- [Disable Editing for a Specific Row](#disable-editing-for-a-specific-row)
- [Provide New/Edited Item via Events](#provide-newedited-item-via-events)
- [Default Column Values](#default-column-values)
- [New Row Position](#new-row-position)
- [Always Show Add-New-Row Form](#always-show-add-new-row-form)
- [Delete Multiple Rows](#delete-multiple-rows)
- [Single-Click Editing](#single-click-editing)
- [Saving a New Row at a Specific Index](#saving-a-new-row-at-a-specific-index)
- [Inline Template Editing](#inline-template-editing)
- [Batch Undo / Redo Patterns](#batch-undo--redo-patterns)
- [Limitations](#limitations)

## Cancel Edit Based on Condition

Set `args.Cancel = true` inside edit/add/delete events to conditionally block CRUD:

```razor
<GridEvents TValue="Order"
            RowEditing="OnRowEditing"
            RowCreating="OnRowCreating"
            RowDeleting="OnRowDeleting">
</GridEvents>

@code {
    void OnRowEditing(RowEditingEventArgs<Order> args)
    {
        if (args.Data.Role == "Admin") args.Cancel = true;   // block edit
    }
    void OnRowCreating(RowCreatingEventArgs<Order> args)
    {
        if (!IsAddAllowed) args.Cancel = true;               // block add
    }
    void OnRowDeleting(RowDeletingEventArgs<Order> args)
    {
        if (args.Datas[0].Role == "Admin") args.Cancel = true; // block delete
    }
}
```

> `RowDeleting` uses `args.Datas` (array), not `args.Data`.

**Batch mode equivalents:**

| Batch Event | Cancelable | Purpose |
|---|---|---|
| `OnCellEdit` | ✅ | Block editing a specific cell |
| `OnBatchAdd` | ✅ | Block adding a new row |
| `OnBatchDelete` | ✅ | Block deleting a row |

## Disable Editing for a Specific Row

```razor
<GridEvents TValue="Order" RowEditing="OnRowEditing"></GridEvents>

@code {
    void OnRowEditing(RowEditingEventArgs<Order> args)
    {
        if (args.Data.ShipCountry == "France")
            args.Cancel = true;
    }
}
```

## Provide New/Edited Item via Events

Use when the model has no parameterless constructor, or when custom initialization is needed.  
Grid uses `Activator.CreateInstance<TValue>()` by default — if this fails, supply instances manually:

```razor
<GridEvents TValue="Order"
            RowCreating="OnRowCreating"
            OnBeginEdit="OnBeginEdit">
</GridEvents>

@code {
    void OnRowCreating(RowCreatingEventArgs<Order> args)
    {
        // Set default values on the new row object
        args.Data.CustomerID = "HANAR";
        args.Data.Freight = 5.0;
        args.Data.ShipCountry = "Brazil";
    }

    void OnBeginEdit(BeginEditArgs<Order> args)
    {
        // Provide a manual deep-clone for the edited row
        args.RowData = new Order(args.RowData.OrderID, args.RowData.CustomerID,
                                 args.RowData.Freight, args.RowData.ShipCountry);
    }
}
```

> `Grid.AddRecordAsync(newRecord, index)` can also inject a pre-built record at a specific index.

## Default Column Values

Pre-fill specific columns when a new row is added using `GridColumn.DefaultValue`:

```razor
<GridColumn Field="CustomerID" DefaultValue="@("HANAR")" Width="120"></GridColumn>
<GridColumn Field="Freight" EditType="EditType.NumericEdit" DefaultValue="@(1.0)" Width="120"></GridColumn>
<GridColumn Field="ShipCountry" EditType="EditType.DropDownEdit" DefaultValue="@("France")" Width="150"></GridColumn>
```

> `DefaultValue` is typed as `object`. Pass the value as the correct runtime type.

## New Row Position

```razor
<GridEditSettings AllowAdding="true" Mode="EditMode.Normal"
                  NewRowPosition="NewRowPosition.Bottom">
</GridEditSettings>
```

| Value | Description |
|---|---|
| `NewRowPosition.Top` | Insert new row at top (default) |
| `NewRowPosition.Bottom` | Insert new row at bottom |

> Supported in **Normal** and **Batch** modes only.

## Always Show Add-New-Row Form

Keeps a persistent blank form visible for continuous data entry:

```razor
<GridEditSettings AllowAdding="true" Mode="EditMode.Normal"
                  ShowAddNewRow="true"
                  NewRowPosition="NewRowPosition.Top">
</GridEditSettings>
```

Press **Enter** or click **Update** in the toolbar to commit the new record.

**Limitations:**
- Normal mode only — not supported in Dialog or Batch
- With Virtual/Infinite Scrolling, new row always appears at **top** regardless of `NewRowPosition`
- Not compatible with Column Virtualization

## Delete Multiple Rows

```razor
<SfGrid DataSource="@Orders" Toolbar="@(new List<string>{ "Delete" })">
    <GridEditSettings AllowDeleting="true"></GridEditSettings>
    <GridSelectionSettings Type="SelectionType.Multiple"></GridSelectionSettings>
    ...
</SfGrid>
```

Select rows (checkbox or click), then click **Delete** toolbar item — or call programmatically:

```razor
await Grid.DeleteRecordAsync(); // deletes all currently selected rows
```

> Enable `ShowDeleteConfirmDialog="true"` on `GridEditSettings` to prevent accidental deletions.

## Single-Click Editing

### Normal / Dialog Mode — via `OnRecordClick`

Trigger edit mode on a single row click using `OnRecordClick`:

```razor
<SfGrid @ref="Grid" DataSource="@Orders">
    <GridEditSettings AllowEditing="true" Mode="EditMode.Normal"></GridEditSettings>
    <GridEvents TValue="Order" OnRecordClick="RecordClickHandler"></GridEvents>
    ...
</SfGrid>

@code {
    SfGrid<Order> Grid;
    int? CurrentRowIndex;

    async Task RecordClickHandler(RecordClickEventArgs<Order> args)
    {
        if (Grid.IsEdit && CurrentRowIndex != args.RowIndex)
            await Grid.EndEditAsync();      // save/close previous row

        CurrentRowIndex = args.RowIndex;
        await Grid.SelectRowAsync(args.RowIndex);
        await Grid.StartEditAsync();
    }
}
```

### Batch Mode — via `AllowEditOnSingleClick`

For **Batch editing**, use the built-in `AllowEditOnSingleClick` property on `GridEditSettings`. Clicking a cell once immediately activates edit mode — no `OnRecordClick` handler needed:

```razor
<SfGrid DataSource="@Orders" Toolbar="@(new List<string>{ "Add", "Delete", "Update", "Cancel" })">
    <GridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true"
                      Mode="EditMode.Batch" AllowEditOnSingleClick="true">
    </GridEditSettings>
    ...
</SfGrid>
```

> `AllowEditOnSingleClick` applies **only to Batch mode**. For Normal and Dialog modes use the `OnRecordClick` approach above.

## Saving a New Row at a Specific Index

Use `OnActionBegin` to override where a newly added row is saved in the datasource:

```razor
<GridEvents TValue="Order" OnActionBegin="OnActionBegin"></GridEvents>

@code {
    void OnActionBegin(ActionEventArgs<Order> args)
    {
        if (args.RequestType == Action.Save && args.Action == "Add")
        {
            // Save at end of current page
            args.Index = (Grid.PageSettings.CurrentPage * Grid.PageSettings.PageSize) - 1;
        }
    }
}
```

## Inline Template Editing

Replace the entire edit row with a fully custom layout using `GridEditSettings.Template`:

```razor
<GridEditSettings AllowEditing="true" AllowAdding="true" Mode="EditMode.Normal">
    <Template>
        @{
            var order = context as Order;
        }
        <table>
            <tr>
                <td><SfNumericTextBox @bind-Value="order.OrderID" Enabled="false"></SfNumericTextBox></td>
                <td><SfTextBox @bind-Value="order.CustomerID"></SfTextBox></td>
                <td><SfNumericTextBox @bind-Value="order.Freight"></SfNumericTextBox></td>
                <td><SfDropDownList @bind-Value="order.ShipCountry" DataSource="@Countries">
                        <DropDownListFieldSettings Text="Name" Value="Name"></DropDownListFieldSettings>
                    </SfDropDownList>
                </td>
            </tr>
        </table>
    </Template>
</GridEditSettings>
```

> All custom editors must use **`@bind-Value`** for two-way data binding.

## Batch Undo / Redo Patterns

The Data Grid tracks every batch edit action — cell value change, row add, row delete — in an Undo / Redo stack. Patterns below use this stack to build safe, observable, and resumable batch workflows.

📄 **Core API reference:** [editing.md → Undo / Redo in Batch Edit Mode](editing.md#undo--redo-in-batch-edit-mode)  
📄 **Events:** [events.md → Editing Events](events.md#editing-events)

### Prerequisites

```razor
<GridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true"
                  Mode="EditMode.Batch"
                  EnableUndoRedo="true"
                  UndoRedoLimit="50">
</GridEditSettings>
```

> Undo / Redo applies **only** when `Mode="EditMode.Batch"`. Other modes silently ignore the stack.

### Bound External Buttons

Drive Undo / Redo from external buttons outside the toolbar:

```razor
@using Syncfusion.Blazor.Buttons
@using Syncfusion.Blazor.Grids

<SfButton OnClick="() => Grid.UndoAsync()">Undo</SfButton>
<SfButton OnClick="() => Grid.RedoAsync()">Redo</SfButton>
<SfButton OnClick="() => Grid.UndoAllAsync()">Undo All</SfButton>
<SfButton OnClick="() => Grid.RedoAllAsync()">Redo All</SfButton>
<SfButton OnClick="() => Grid.ClearUndoRedoAsync()">Reset Stack</SfButton>

<SfGrid @ref="Grid" DataSource="@Orders"
        Toolbar="@(new List<string>() { "Add", "Edit", "Delete", "Update", "Cancel", "Undo", "Redo" })">
    <GridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true"
                      Mode="EditMode.Batch" EnableUndoRedo="true">
    </GridEditSettings>
    <GridColumns>
        <GridColumn Field="OrderID" IsPrimaryKey="true" HeaderText="Order ID" Width="120"></GridColumn>
        <GridColumn Field="CustomerID" HeaderText="Customer" Width="150"></GridColumn>
        <GridColumn Field="Freight" HeaderText="Freight" Format="C2" Width="120"></GridColumn>
    </GridColumns>
</SfGrid>

@code {
    SfGrid<Order> Grid;
}
```

### Keyboard Shortcuts

The grid listens for these shortcuts whenever focus is inside the grid and a batch action is in scope:

| Key | Action |
|---|---|
| `Ctrl+Z` | Undo last batch action |
| `Ctrl+Y` | Redo last reverted action |
| `Cmd+Z` (macOS) | Undo last batch action |
| `Cmd+Shift+Z` (macOS) | Redo last reverted action |

### Conditional Undo / Redo

Block an undo or redo step from `OnUndo` / `OnRedo` when business rules forbid rolling back the action:

```razor
<GridEvents TValue="Order"
            OnUndo="OnBeforeUndo"
            OnRedo="OnBeforeRedo">
</GridEvents>

@code {
    void OnBeforeUndo(OnUndoEventArgs<Order> args)
    {
        // Block undo if user does not have permission to revert the action
        if (!HasRevertPermission) args.Cancel = true;
    }

    void OnBeforeRedo(OnRedoEventArgs<Order> args)
    {
        // Block redo when the redo would violate a downstream rule
        if (ViolatesRule(args.Data)) args.Cancel = true;
    }
}
```

### Auditing Undo / Redo

Run logging or audit-trail logic after an undo / redo is applied:

```razor
<GridEvents TValue="Order"
            OnUndoCompleted="OnUndoDone"
            OnRedoCompleted="OnRedoDone">
</GridEvents>

@code {
    void OnUndoDone(OnUndoCompletedEventArgs<Order> args)
        => Audit.Log("Undo", args.Action, args.Data);

    void OnRedoDone(OnRedoCompletedEventArgs<Order> args)
        => Audit.Log("Redo", args.Action, args.Data);
}
```

### Reset Stack After Save or Cancel

Use `ClearUndoRedoAsync` after a successful save, or after `Cancel` clears pending changes, to start the next batch with a clean stack:

```razor
<SfButton OnClick="SaveAndReset">Save</SfButton>
<SfButton OnClick="CancelAndReset">Cancel</SfButton>

@code {
    SfGrid<Order> Grid;

    async Task SaveAndReset()
    {
        await Grid.EndEditAsync();
        await Grid.ClearUndoRedoAsync();
    }

    async Task CancelAndReset()
    {
        await Grid.CloseEditAsync();
        await Grid.ClearUndoRedoAsync();
    }
}
```

### Stack Size Tuning

`UndoRedoLimit` caps the stack. Choose a value based on the typical edit session:

| Scenario | Suggested `UndoRedoLimit` |
|---|---|
| Quick data correction (few cells) | `10`–`20` (default `20`) |
| Bulk import / cleanup | `100`–`500` |
| Memory-constrained clients | `5`–`10` |

> When the limit is reached, the **oldest** action is dropped — recent edits always remain undoable.

### Interaction with Confirm Dialogs

When `ShowDeleteConfirmDialog="true"`, the confirmation dialog appears before the delete is committed. The Undo / Redo stack remains active while the dialog is open and is cleared only after the user confirms **Update** or **Cancel**.

### Limitations

| Limitation | Detail |
|---|---|
| **Mode** | Undo / Redo applies to Batch mode only — Normal, Dialog, and Cell modes ignore `EnableUndoRedo` |
| **Toolbar dependency** | The `Undo` and `Redo` toolbar items only render when `EnableUndoRedo="true"` and `Mode="EditMode.Batch"` |
| **Stack lifetime** | The stack is in-memory only — it is cleared after a successful save, after cancel, or after `ClearUndoRedoAsync`. It does **not** survive page reload |
| **Custom edits** | Actions performed through `UpdateCellAsync` / `UpdateRowAsync` outside the standard batch flow are tracked by the stack as long as Batch mode is active |
| **Remote adaptors** | `EnableUndoRedo` does not affect server requests — Undo / Redo only manipulate the pending batch before it is sent |

## Limitations

| Limitation | Detail |
|---|---|
| **Command column — no Add** | Command buttons render only after a record exists; Add is not supported via command column |
| **`ShowAddNewRow`** | Normal mode only; always at top with Virtual/Infinite Scrolling; incompatible with Column Virtualization |
| **`NewRowPosition`** | Supported in Normal and Batch modes only |
| **`SetCellValueAsync`** | Updates Grid UI only — does **not** persist to the underlying datasource |
| **`UpdateCellAsync`** | Batch mode only — queues the cell change until `ApplyBatchChangesAsync` is called |
| **`AllowEditOnSingleClick`** | Batch mode only — single click activates cell edit; Normal and Dialog modes are unaffected |
| **`AddRecordAsync` / `StartEditAsync`** | Normal and Dialog modes only; not applicable in Batch mode |
| **Parameterless constructor** | Grid uses `Activator.CreateInstance<TValue>()` — model must have a parameterless constructor, or supply instances via `RowCreating`/`OnBeginEdit` |
| **`IsIdentity` columns** | Treated as read-only during both add and edit operations |
| **Validation timing** | Messages re-trigger only on form submit or field blur (Blazor `EditForm` behavior) |
| **Complex type validation** | Requires `[ValidateComplexType]` + `Microsoft.AspNetCore.Components.DataAnnotations.Validation` NuGet |
| **Dialog max height** | Capped at ~658px on 1920×1080 screens |
| **`AllowEditing=false` on column** | Disables editing only; column still appears in add form unless `AllowAdding=false` is also set |
