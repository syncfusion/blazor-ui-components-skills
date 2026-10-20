# Editing — Syncfusion Blazor DataGrid

## Table of Contents

- [Enable Editing](#enable-editing)
- [GridEditSettings Properties](#gridedsettings-properties)
- [EditMode Enum](#editmode-enum)
- [Column EditType](#column-edittype)
- [EditorSettings (Customize Editors)](#editorsettings-customize-editors)
- [Custom EditTemplate per Column](#custom-edittemplate-per-column)
- [Normal (Inline) Edit Mode](#normal-inline-edit-mode)
- [Dialog Edit Mode](#dialog-edit-mode)
- [Cell Edit Mode](#cell-edit-mode)
  - [AllowEditOnKeyPress](#alloweditonkeypress)
  - [EnterKeyDirection](#enterkeydirection)
- [Batch Edit Mode](#batch-edit-mode)
  - [Undo / Redo in Batch Edit Mode](#undo--redo-in-batch-edit-mode)
- [Command Column](#command-column)
- [Validation](#validation)
- [Programmatic Editing Methods](#programmatic-editing-methods)
- [Editing Events](#editing-events)

📄 **Advanced patterns** (cancel conditions, default values, new row position, single-click, delete multiple, inline template, limitations):
→ [editing-patterns.md](editing-patterns.md)

📄 **Validation deep-dive** (data annotations, custom attributes, complex types, custom validator component):
→ [editing-validation.md](editing-validation.md)

---

## Enable Editing

```razor
<SfGrid DataSource="@Orders" Toolbar="@(new List<string>{ "Add","Edit","Delete","Update","Cancel" })">
    <GridEditSettings AllowEditing="true" AllowAdding="true" AllowDeleting="true"
                      Mode="EditMode.Normal" AllowEditOnDblClick="true"
                      ShowDeleteConfirmDialog="true">
    </GridEditSettings>
    <GridColumns>
        <GridColumn Field="OrderID" IsPrimaryKey="true" HeaderText="Order ID" Width="120"></GridColumn>
        <GridColumn Field="CustomerID" HeaderText="Customer" Width="150"></GridColumn>
        <GridColumn Field="Freight" HeaderText="Freight" EditType="EditType.NumericEdit" Width="120"></GridColumn>
        <GridColumn Field="OrderDate" HeaderText="Date" EditType="EditType.DatePickerEdit" Width="130"></GridColumn>
    </GridColumns>
</SfGrid>
```

> **`IsPrimaryKey` is required** — without it, all edits target the first row.

## GridEditSettings Properties

| Property | Type | Description |
|---|---|---|
| `AllowEditing` | bool | Enable row editing |
| `AllowAdding` | bool | Enable adding new rows |
| `AllowDeleting` | bool | Enable row deletion |
| `Mode` | `EditMode` | Editing mode (`Normal`, `Dialog`, `Batch`, `Cell`) |
| `AllowEditOnDblClick` | bool | Enable edit on double-click (default `true`) |
| `AllowEditOnSingleClick` | bool | Enable single-click batch editing — cell enters edit mode on one click; **Batch mode only** |
| `AllowEditOnKeyPress` | bool | Start cell edit when a printable character is typed on a selected cell; **Cell mode only** |
| `EnterKeyDirection` | `EnterKeyDirection` | Where focus moves after saving a cell with the Enter key; **Cell mode only** |
| `ShowDeleteConfirmDialog` | bool | Show confirm dialog before delete |
| `NewRowPosition` | `NewRowPosition` | Position of new row: `Top` (default) or `Bottom` |
| `ShowAddNewRow` | bool | Always display a blank add-new-row form (Normal mode only) |
| `Validator` | `Type` | Custom validator component type for the edit form |
| `EnableUndoRedo` | bool | Enable Undo / Redo action stack for batch edits; **Batch mode only** |
| `UndoRedoLimit` | int | Maximum number of actions retained in the Undo / Redo stack; default `20`. When the limit is reached, the oldest action is removed. **Batch mode only** |

> `ShowAddNewRow` is supported in **Normal** mode only. See [editing-patterns.md](editing-patterns.md) for limitations.

**Column-level editing properties:**

| Property | Description |
|---|---|
| `GridColumn.AllowEditing` | Set to `false` to make a specific column read-only during edit |
| `GridColumn.AllowAdding` | Set to `false` to prevent value entry for a column on new-row add |
| `GridColumn.IsIdentity` | Set to `true` for auto-increment/identity columns — column becomes read-only in both edit and add modes |

## EditMode Enum

| Mode | Description |
|---|---|
| `Normal` | Inline editing within the row (default) |
| `Dialog` | Opens a popup dialog for editing |
| `Batch` | Double-click any cell; save all changes at once |
| `Cell` | Each cell saves independently on Enter or focus move — inline edits without a separate Update action |

## Column EditType

Set `EditType` per column to control the editor widget:

| EditType | Widget |
|---|---|
| `DefaultEdit` | `SfTextBox` |
| `NumericEdit` | `SfNumericTextBox` |
| `DropDownEdit` | `SfDropDownList` |
| `BooleanEdit` | `SfCheckBox` |
| `DatePickerEdit` | `SfDatePicker` |
| `DateTimePickerEdit` | `SfDateTimePicker` |
| `TimePickerEdit` | `SfTimePicker` |

## EditorSettings (Customize Editors)

Pass typed params via `EditorSettings` on `GridColumn`:

```razor
@code {
    public StringEditCellParams StringParams = new StringEditCellParams
    {
        Params = new TextBoxModel { ShowClearButton = true, Multiline = true }
    };

    public NumericEditCellParams NumericParams = new NumericEditCellParams
    {
        Params = new NumericTextBoxModel<object> { Decimals = 2, ShowSpinButton = false, Format = "N2" }
    };

    public DropDownEditCellParams DropDownParams = new DropDownEditCellParams
    {
        Params = new DropDownListModel<object, object> { AllowFiltering = true, ShowClearButton = true }
    };

    public BooleanEditCellParams BooleanParams = new BooleanEditCellParams
    {
        Params = new CheckBoxModel { Label = "Active", LabelPosition = LabelPosition.After }
    };

    public DateEditCellParams DateParams = new DateEditCellParams
    {
        Params = new DatePickerModel { Format = "MM/dd/yyyy", ShowClearButton = true }
    };
}
```

```razor
<GridColumn Field="Freight" EditType="EditType.NumericEdit"
            EditorSettings="@NumericParams" Width="120"></GridColumn>
```

## Custom EditTemplate per Column

Use `EditTemplate` for fully custom per-column editors:

```razor
<GridColumn Field="ShipCity" HeaderText="Ship City" Width="150">
    <EditTemplate>
        @{ var order = context as Order; }
        <SfDropDownList @bind-Value="order.ShipCity" DataSource="@Cities">
            <DropDownListFieldSettings Text="Name" Value="Name"></DropDownListFieldSettings>
        </SfDropDownList>
    </EditTemplate>
</GridColumn>
```

**Nested object** — use `__` (double underscore) instead of `.` in `Field`:

```razor
<GridColumn Field="Address__City" HeaderText="City">
    <EditTemplate>
        <SfTextBox @bind-Value="(context as Order).Address.City"></SfTextBox>
    </EditTemplate>
</GridColumn>
```

**Enum column:**

```razor
<GridColumn Field="Status" HeaderText="Status">
    <EditTemplate>
        @{ var item = context as Order; }
        <SfDropDownList TValue="OrderStatus" TItem="string"
                        DataSource="@(Enum.GetNames(typeof(OrderStatus)).ToList())"
                        @bind-Value="item.Status">
        </SfDropDownList>
    </EditTemplate>
</GridColumn>
```

**Foreign key column:**

```razor
<GridForeignColumn Field="EmployeeID" HeaderText="Employee"
                   ForeignKeyValue="FirstName" ForeignDataSource="@Employees">
    <EditTemplate>
        @{ var order = context as Order; }
        <SfComboBox TValue="int" TItem="Employee" DataSource="@Employees"
                    @bind-Value="order.EmployeeID">
            <ComboBoxFieldSettings Text="FirstName" Value="EmployeeID"></ComboBoxFieldSettings>
        </SfComboBox>
    </EditTemplate>
</GridForeignColumn>
```

> All custom editors must use **`@bind-Value`** for two-way data binding.

## Normal (Inline) Edit Mode

```razor
<GridEditSettings AllowEditing="true" AllowAdding="true" AllowDeleting="true"
                  Mode="EditMode.Normal">
</GridEditSettings>
```

Auto-update a dependent column using `EditTemplate` + `ValueChange`:

```razor
<GridColumn Field="UnitPrice" HeaderText="Unit Price">
    <EditTemplate>
        @{ var item = context as Order; }
        <SfNumericTextBox @ref="PriceBox" @bind-Value="item.UnitPrice"
                          ValueChange="@OnPriceChange">
        </SfNumericTextBox>
    </EditTemplate>
</GridColumn>

@code {
    SfNumericTextBox<double?> PriceBox;

    async Task OnPriceChange(ChangeEventArgs<double?> args)
    {
        // recalculate dependent field, then force re-render
        await Grid.PreventRender(false);
    }
}
```

## Dialog Edit Mode

```razor
<GridEditSettings AllowEditing="true" Mode="EditMode.Dialog"
                  Dialog="@(new DialogSettings { Height = "400px", Width = "600px" })">
    <HeaderTemplate>
        @{ var order = context as Order; }
        <span>@(order?.OrderID == 0 ? "New Record" : $"Edit Order {order.OrderID}")</span>
    </HeaderTemplate>
    <FooterTemplate>
        <SfButton OnClick="@SaveClick" IsPrimary="true">Save</SfButton>
        <SfButton OnClick="@CancelClick">Cancel</SfButton>
    </FooterTemplate>
</GridEditSettings>

@code {
    SfGrid<Order> Grid;
    async Task SaveClick()   => await Grid.EndEditAsync();
    async Task CancelClick() => await Grid.CloseEditAsync();
}
```

> Dialog max height is ~658px on a 1920×1080 screen.

## Full-Row / Dialog Template Editing (GridEditSettings.Template)

Use `GridEditSettings.Template` to replace the **entire row or dialog form** with a custom layout. This differs from per-column `EditTemplate` — it controls the whole edit UI at once.

**Inline (Normal) template** — renders a custom row in-place:

```razor
<SfGrid DataSource="@Orders" Toolbar="@(new string[] { "Add","Edit","Delete","Update","Cancel" })">
    <GridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true"
                      Mode="EditMode.Normal">
        <Template>
            @{
                var order = context as Order;
                <table>
                    <tr>
                        <td>
                            <SfNumericTextBox @bind-Value="order.OrderID"
                                              Enabled="@(order.OrderID == 0)"
                                              TValue="int">
                            </SfNumericTextBox>
                        </td>
                        <td>
                            <SfTextBox @bind-Value="order.CustomerID"></SfTextBox>
                        </td>
                        <td>
                            <SfDatePicker @bind-Value="order.OrderDate"></SfDatePicker>
                        </td>
                    </tr>
                </table>
            }
        </Template>
    </GridEditSettings>
</SfGrid>
```

**Dialog template** — same approach with `Mode="EditMode.Dialog"`:

```razor
<GridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true"
                  Mode="EditMode.Dialog" Dialog="@(new DialogSettings { Width = "450px" })">
    <Template>
        @{
            var order = context as Order;
            <div class="form-row">
                <div class="form-group col-md-6">
                    <SfNumericTextBox @bind-Value="order.OrderID"
                                      Enabled="@(order.OrderID == 0)"
                                      FloatLabelType="FloatLabelType.Always"
                                      Placeholder="Order ID" TValue="int">
                    </SfNumericTextBox>
                </div>
                <div class="form-group col-md-6">
                    <SfTextBox @bind-Value="order.CustomerID"
                               FloatLabelType="FloatLabelType.Always"
                               Placeholder="Customer Name">
                    </SfTextBox>
                </div>
            </div>
        }
    </Template>
</GridEditSettings>
```

> - Always use **`@bind-Value`** on every editor inside the template.
> - Disable the primary key editor with `Enabled="@(order.OrderID == 0)"` to prevent editing on existing rows.
> - For validation inside templates, wrap with `<EditForm>` or use `<DataAnnotationsValidator>`.

## Cell Edit Mode

Cell editing (`Mode="EditMode.Cell"`) lets users update individual cells directly. Each cell commits independently on Enter or when focus moves — no explicit Update action is required.

**Enable cell editing:**

```razor
<SfGrid DataSource="@Orders"
        Toolbar="@(new List<string>() { "Add", "Edit", "Delete", "Update", "Cancel" })"
        Height="315">
    <GridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true"
                      Mode="EditMode.Cell">
    </GridEditSettings>
    <GridColumns>
        <GridColumn Field="OrderID" HeaderText="Order ID" IsPrimaryKey="true"
                    ValidationRules="@(new ValidationRules{ Required=true })"
                    TextAlign="TextAlign.Right" Width="120"></GridColumn>
        <GridColumn Field="CustomerID" HeaderText="Customer Name"
                    ValidationRules="@(new ValidationRules{ Required=true, MinLength=5 })"
                    Width="120"></GridColumn>
        <GridColumn Field="Freight" HeaderText="Freight"
                    ValidationRules="@(new ValidationRules{ Required=true, Min=1, Max=1000 })"
                    Format="C2" TextAlign="TextAlign.Right"
                    EditType="EditType.NumericEdit" Width="120"></GridColumn>
        <GridColumn Field="ShipCountry" HeaderText="Ship Country"
                    EditType="EditType.DropDownEdit" Width="150"></GridColumn>
    </GridColumns>
</SfGrid>
```

> **`IsPrimaryKey="true"` is required** on the unique identifier column so updates map to the correct record. Without it, all edits target the first row.

**Cell edit entry points** — a cell enters edit mode when the user:

- Double-clicks the cell
- Selects the cell and presses **Enter**
- Selects the cell and presses **F2**
- Types a printable character on a selected cell (requires `AllowEditOnKeyPress="true"`)

### AllowEditOnKeyPress

Set `AllowEditOnKeyPress="true"` on `GridEditSettings` so that pressing a printable character (letter, digit, symbol) on a selected cell places that cell directly into edit mode and applies the typed character — ideal for spreadsheet-style data entry such as inventory updates and order management.

```razor
<GridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true"
                  Mode="EditMode.Cell"
                  AllowEditOnKeyPress="true">
</GridEditSettings>
```

> `AllowEditOnKeyPress` applies **only** when `Mode="EditMode.Cell"`.

### EnterKeyDirection

`EnterKeyDirection` controls where focus moves after saving a cell with the Enter key. Default is `EnterKeyDirection.None` (focus stays on the current cell).

| Enum value | Description |
|---|---|
| `EnterKeyDirection.None` | Keeps focus on the current cell after saving |
| `EnterKeyDirection.NextColumn` | Moves focus to the next column in the current row — row-wise entry |
| `EnterKeyDirection.NextRow` | Moves focus to the same column in the next row — column-wise entry across records |

```razor
@using Syncfusion.Blazor.Grids

<GridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true"
                  Mode="EditMode.Cell"
                  EnterKeyDirection="EnterKeyDirection.NextRow">
</GridEditSettings>
```

The full `EnterKeyDirection` enum lives in the `Syncfusion.Blazor.Grids` namespace. Bind it to a UI control for runtime switching:

```razor
@using Syncfusion.Blazor.Grids
@using Syncfusion.Blazor.DropDowns

<label>Enter Key Direction</label>
<SfDropDownList DataSource="@EnterDirectionOptions"
                @bind-Value="SelectedEnterDirection"
                TValue="EnterKeyDirection"
                TItem="DropdownOption<EnterKeyDirection>"
                PopupHeight="200px"
                Width="150px">
    <DropDownListFieldSettings Text="Label" Value="Value"></DropDownListFieldSettings>
</SfDropDownList>

<SfGrid DataSource="@OrderData"
        Toolbar="@(new List<string>() { "Add", "Edit", "Delete", "Update", "Cancel" })"
        Height="315">
    <GridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true"
                      Mode="EditMode.Cell"
                      EnterKeyDirection="@SelectedEnterDirection">
    </GridEditSettings>
    <GridColumns>
        <GridColumn Field="OrderID" HeaderText="Order ID" IsPrimaryKey="true" Width="120"
                    ValidationRules="@(new ValidationRules{ Required=true })"
                    TextAlign="TextAlign.Right"></GridColumn>
        <GridColumn Field="CustomerID" HeaderText="Customer Name" Width="120"
                    ValidationRules="@(new ValidationRules{ Required=true, MinLength=5 })"></GridColumn>
        <GridColumn Field="Freight" HeaderText="Freight" Width="120"
                    ValidationRules="@(new ValidationRules{ Required=true, Min=1, Max=1000 })"
                    Format="C2" TextAlign="TextAlign.Right"
                    EditType="EditType.NumericEdit"></GridColumn>
        <GridColumn Field="ShipCountry" HeaderText="Ship Country" Width="150"
                    EditType="EditType.DropDownEdit"></GridColumn>
    </GridColumns>
</SfGrid>

@code {
    private EnterKeyDirection SelectedEnterDirection { get; set; } = EnterKeyDirection.NextRow;

    private List<DropdownOption<EnterKeyDirection>> EnterDirectionOptions { get; set; } = new List<DropdownOption<EnterKeyDirection>>
    {
        new DropdownOption<EnterKeyDirection> { Label = "None",       Value = EnterKeyDirection.None },
        new DropdownOption<EnterKeyDirection> { Label = "NextRow",    Value = EnterKeyDirection.NextRow },
        new DropdownOption<EnterKeyDirection> { Label = "NextColumn", Value = EnterKeyDirection.NextColumn }
    };

    private class DropdownOption<T>
    {
        public string? Label { get; set; }
        public T? Value { get; set; }
    }
}
```

> Both `EnterKeyDirection` and `AllowEditOnKeyPress` apply **only** when `Mode="EditMode.Cell"`.

### Cell Editing Events

| Event | Args | Cancelable | Description |
|---|---|---|---|
| `OnCellEdit` | `CellEditArgs<T>` | ✅ | Before a cell enters edit mode (double-click, F2, Enter, or `AllowEditOnKeyPress`). Set `args.Cancel = true` to block. |
| `OnCellSave` | `CellSaveArgs<T>` | ✅ | Before the edited cell value is committed. Set `args.Cancel = true` to block the save. |
| `CellSaved` | `CellSavedArgs<T>` | ❌ | After the cell value is saved and the edited value is highlighted in the UI. |
| `EditCanceling` | `EditCancelingEventArgs<T>` | ✅ | Before an edit operation is canceled — use for confirmation prompts or rollback. |
| `EditCanceled` | `EditCanceledEventArgs<T>` | ❌ | After an edit operation is canceled. |

> `OnCellEdit`, `OnCellSave`, and `CellSaved` are the cell-level events documented for Cell mode editing.

```razor
<GridEvents TValue="Order"
            OnCellEdit="OnCellEdit"
            OnCellSave="OnCellSave"
            CellSaved="OnCellSaved"
            EditCanceling="OnEditCanceling"
            EditCanceled="OnEditCanceled">
</GridEvents>

@code {
    void OnCellEdit(CellEditArgs<Order> args)
    {
        // Block editing when order is already shipped
        if (args.Data.Status == "Shipped") args.Cancel = true;
    }

    void OnCellSave(CellSaveArgs<Order> args)
    {
        // Force value to upper-case before saving
        if (args.ColumnName == "CustomerID" && args.Value is string s)
            args.Value = s.ToUpperInvariant();
    }

    void OnCellSaved(CellSavedArgs<Order> args)
    {
        // Auto-update dependent column after a cell save
    }

    void OnEditCanceling(EditCancelingEventArgs<Order> args)
    {
        // Prompt user before discarding changes
    }

    void OnEditCanceled(EditCanceledEventArgs<Order> args)
    {
        // Cleanup after cancel
    }
}
```

### Cell Edit Mode — Troubleshooting

| Issue | Likely cause | Resolution |
|---|---|---|
| Double-clicking a cell does not start editing. | `AllowEditing="false"` or `Mode` is not `EditMode.Cell`. | Set `AllowEditing="true"` and `Mode="EditMode.Cell"` on `GridEditSettings`. |
| Saved changes appear on the wrong record. | The primary key column is missing or `IsPrimaryKey` is not set. | Set `IsPrimaryKey="true"` on the unique identifier column. |
| Typing into a selected cell does not start editing. | `AllowEditOnKeyPress` is `false` (default). | Set `AllowEditOnKeyPress="true"` on `GridEditSettings`. |
| Focus does not move after pressing Enter. | `EnterKeyDirection` is set to `None`. | Set `EnterKeyDirection="EnterKeyDirection.NextRow"` or `"EnterKeyDirection.NextColumn"`. |
| A cell fails to save. | The entered value fails a validation rule on the column. | Adjust the column's `ValidationRules` or correct the entered value. |

## Batch Edit Mode

Double-click any cell to edit; all changes are committed together:

```razor
<SfGrid @ref="Grid" DataSource="@Orders"
        Toolbar="@(new List<string>{ "Add","Delete","Update","Cancel" })">
    <GridEditSettings AllowEditing="true" AllowAdding="true" AllowDeleting="true"
                      Mode="EditMode.Batch">
    </GridEditSettings>
</SfGrid>
```

**Single-click batch editing** — Set `AllowEditOnSingleClick="true"` so cells enter edit mode on a single click instead of a double-click:

```razor
<GridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true"
                  Mode="EditMode.Batch" AllowEditOnSingleClick="true">
</GridEditSettings>
```

> `AllowEditOnSingleClick` applies **only to Batch mode**. Normal and Dialog editing modes are not affected.

Auto-update another column from `CellSaved`:

```razor
<GridEvents TValue="Order" CellSaved="OnCellSaved"></GridEvents>

@code {
    async Task OnCellSaved(CellSavedArgs<Order> args)
    {
        if (args.ColumnName == "UnitPrice" || args.ColumnName == "Quantity")
        {
            var rowIndex = await Grid.GetRowIndexByPrimaryKeyAsync(args.Data.OrderID);
            await Grid.UpdateCellAsync(rowIndex, "Total",
                                       args.Data.UnitPrice * args.Data.Quantity);
        }
    }
}
```

Save all pending batch changes programmatically by passing a `BatchChanges<TValue>` object that contains the added, changed, and deleted records:

```razor
@code {
    async Task SaveBatchChanges()
    {
        var batchChanges = new BatchChanges<Order>
        {
            AddedRecords = new List<Order> { /* newly added rows */ },
            ChangedRecords = new List<Order> { /* modified rows */ },
            DeletedRecords = new List<Order> { /* deleted rows */ }
        };
        await Grid.ApplyBatchChangesAsync(batchChanges);
    }
}
```

### Undo / Redo in Batch Edit Mode

Undo / Redo lets users revert or reapply individual batch edit actions — cell value changes, row additions, and row deletions — without discarding the rest of the pending batch. Undo and Redo are **Batch mode only**.

> The number of stored actions is limited by `UndoRedoLimit` (default `20`). When the limit is reached, the oldest action is removed from the stack.

**Enable with toolbar:**

```razor
<SfGrid DataSource="@Orders"
        Toolbar="@(new List<string>() { "Add", "Edit", "Delete", "Update", "Cancel", "Undo", "Redo" })"
        Height="315">
    <GridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true"
                      EnableUndoRedo="true" Mode="EditMode.Batch">
    </GridEditSettings>
    <GridColumns>
        <GridColumn Field="OrderID" IsPrimaryKey="true" HeaderText="Order ID" Width="120"
                    ValidationRules="@(new ValidationRules{ Required=true })"
                    TextAlign="TextAlign.Right"></GridColumn>
        <GridColumn Field="CustomerID" HeaderText="Customer Name" Width="120"
                    ValidationRules="@(new ValidationRules{ Required=true, MinLength=5 })"></GridColumn>
        <GridColumn Field="Freight" HeaderText="Freight" Width="120"
                    ValidationRules="@(new ValidationRules{ Required=true, Min=1, Max=1000 })"
                    Format="C2" TextAlign="TextAlign.Right"
                    EditType="EditType.NumericEdit"></GridColumn>
        <GridColumn Field="ShipCountry" HeaderText="Ship Country"
                    EditType="EditType.DropDownEdit" Width="150"></GridColumn>
    </GridColumns>
</SfGrid>
```

**Customize the stack size** with `UndoRedoLimit`:

```razor
<GridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true"
                  EnableUndoRedo="true" UndoRedoLimit="25"
                  Mode="EditMode.Batch">
</GridEditSettings>
```

**Keyboard shortcuts:**

| Keys | Action |
|---|---|
| `Ctrl + Z` | Revert the last batch edit action |
| `Ctrl + Y` | Reapply the last reverted batch edit action |
| `Cmd + Z` (macOS) | Revert the last batch edit action |
| `Cmd + Shift + Z` (macOS) | Reapply the last reverted batch edit action |

**API methods:**

| Method | Behavior |
|---|---|
| `UndoAsync()` | Revert the last batch edit action |
| `RedoAsync()` | Reapply the last reverted batch edit action |
| `UndoAllAsync` | Revert all batch edit actions in the undo stack |
| `RedoAllAsync` | Reapply all reverted batch edit actions in the redo stack |
| `ClearUndoRedoAsync` | Clear both the undo stack and the redo stack |

```razor
@code {
    SfGrid<Order> Grid;

    async Task UndoLast() => await Grid.UndoAsync();
    async Task RedoLast() => await Grid.RedoAsync();
    async Task UndoEverything() => await Grid.UndoAllAsync();
    async Task RedoEverything() => await Grid.RedoAllAsync();
    async Task ResetStacks() => await Grid.ClearUndoRedoAsync();
}
```

**Events:**

| Event | Args | Description |
|---|---|---|
| `OnUndo` | `OnUndoEventArgs<T>` | Fires before an undo action is applied |
| `OnUndoCompleted` | `OnUndoCompletedEventArgs<T>` | Fires after an undo action is applied |
| `OnRedo` | `OnRedoEventArgs<T>` | Fires before a redo action is applied |
| `OnRedoCompleted` | `OnRedoCompletedEventArgs<T>` | Fires after a redo action is applied |

```razor
<GridEvents TValue="Order"
            OnUndo="OnUndo"
            OnUndoCompleted="OnUndoCompleted"
            OnRedo="OnRedo"
            OnRedoCompleted="OnRedoCompleted">
</GridEvents>

@code {
    void OnUndo(OnUndoEventArgs<Order> args) { /* before undo */ }
    void OnUndoCompleted(OnUndoCompletedEventArgs<Order> args) { /* after undo */ }
    void OnRedo(OnRedoEventArgs<Order> args) { /* before redo */ }
    void OnRedoCompleted(OnRedoCompletedEventArgs<Order> args) { /* after redo */ }
}
```

> When `ShowDeleteConfirmDialog` is `true`, the confirm dialog opens before the batch operation completes. The undo / redo stack stays active during the dialog and is cleared only after the user confirms **Update** or **Cancel**.

## Command Column

Add inline CRUD buttons directly inside a column:

```razor
<GridColumn HeaderText="Actions" Width="160">
    <GridCommandColumns>
        <GridCommandColumn Type="CommandButtonType.Edit"
            ButtonOption="@(new CommandButtonOptions { IconCss="e-icons e-edit", CssClass="e-flat" })">
        </GridCommandColumn>
        <GridCommandColumn Type="CommandButtonType.Delete"
            ButtonOption="@(new CommandButtonOptions { IconCss="e-icons e-delete", CssClass="e-flat" })">
        </GridCommandColumn>
        <GridCommandColumn Type="CommandButtonType.Save"
            ButtonOption="@(new CommandButtonOptions { IconCss="e-icons e-save", CssClass="e-flat" })">
        </GridCommandColumn>
        <GridCommandColumn Type="CommandButtonType.Cancel"
            ButtonOption="@(new CommandButtonOptions { IconCss="e-icons e-cancel", CssClass="e-flat" })">
        </GridCommandColumn>
    </GridCommandColumns>
</GridColumn>
```

**Custom command button:**

```razor
<GridCommandColumn ButtonOption="@(new CommandButtonOptions { Content = "Details", CssClass = "e-flat" })">
</GridCommandColumn>

<GridEvents TValue="Order" CommandClicked="OnCommandClick"></GridEvents>

@code {
    void OnCommandClick(CommandClickEventArgs<Order> args)
    {
        if (args.CommandColumn.ButtonOption.Content == "Details")
        {
            // handle custom action using args.RowData
        }
    }
}
```

> **Command column does not support Add** — buttons render only after a record exists.

## Validation

Quick reference — for full detail see [editing-validation.md](editing-validation.md).

**Per-column rules:**

```razor
<GridColumn Field="CustomerID"
            ValidationRules="@(new ValidationRules { Required = true, MinLength = 3, MaxLength = 8 })">
</GridColumn>

<GridColumn Field="Freight" EditType="EditType.NumericEdit"
            ValidationRules="@(new ValidationRules { Required = true, Min = 1, Max = 1000 })">
</GridColumn>
```

**Data annotations on model:**

```csharp
public class Order
{
    [Key]                              // primary key
    public int OrderID { get; set; }

    [Required]
    [StringLength(8, MinimumLength = 3)]
    public string CustomerID { get; set; }

    [Range(1, 1000)]
    public double Freight { get; set; }
}
```

> See [editing-validation.md](editing-validation.md) for custom attributes, complex type validation, and custom validator components.

## Programmatic Editing Methods

```razor
@code {
    SfGrid<Order> Grid;

    // Add empty row, or pass data + insert index
    await Grid.AddRecordAsync();
    await Grid.AddRecordAsync(newRecord, index);

    // Edit the currently selected row
    await Grid.StartEditAsync();

    // Save current edit
    await Grid.EndEditAsync();

    // Cancel current edit
    await Grid.CloseEditAsync();

    // Delete selected row(s); or target by field + object
    await Grid.DeleteRecordAsync();
    await Grid.DeleteRecordAsync("OrderID", selectedOrder);

    // Update an entire row by index (persists to datasource)
    await Grid.UpdateRowAsync(rowIndex, updatedOrder);

    // Update a single cell by primary key — UI only, NOT datasource
    await Grid.SetCellValueAsync(primaryKeyValue, "FieldName", newValue);

    // Batch mode: save all pending changes — requires a BatchChanges<TValue> argument
    var batchChanges = new BatchChanges<Order>
    {
        AddedRecords   = new List<Order> { /* newly added rows */ },
        ChangedRecords = new List<Order> { /* modified rows */ },
        DeletedRecords = new List<Order> { /* deleted rows */ }
    };
    await Grid.ApplyBatchChangesAsync(batchChanges);

    // Batch mode: queue a cell change before ApplyBatchChangesAsync
    var idx = await Grid.GetRowIndexByPrimaryKeyAsync(primaryKeyValue);
    await Grid.UpdateCellAsync(idx, "FieldName", newValue);

    // Batch mode: Undo / Redo — require EnableUndoRedo="true" on GridEditSettings
    await Grid.UndoAsync();             // revert the last batch edit action
    await Grid.RedoAsync();             // reapply the last reverted action
    await Grid.UndoAllAsync();          // revert all actions in the undo stack
    await Grid.RedoAllAsync();          // reapply all actions in the redo stack
    await Grid.ClearUndoRedoAsync();    // clear both stacks
}
```

> **`SetCellValueAsync`** updates the rendered cell only — use `UpdateRowAsync` to persist to the datasource.  
> **`UpdateCellAsync`** is Batch mode only — queues the change until `ApplyBatchChangesAsync` is called.  
> **`ApplyBatchChangesAsync`** requires a `BatchChanges<TValue>` argument containing `AddedRecords`, `ChangedRecords`, and `DeletedRecords` lists.  
> **`UndoAsync` / `RedoAsync` / `UndoAllAsync` / `RedoAllAsync` / `ClearUndoRedoAsync`** are Batch mode only and require `EnableUndoRedo="true"`.

## Server-side CRUD Persistence

When the Grid is bound to a **remote data source**, CRUD operations must be persisted to the server via `SfDataManager`. The adaptor handles translating Grid actions into server requests automatically.

| Adaptor | Use Case |
|---|---|
| `UrlAdaptor` | Custom REST API with `InsertUrl`, `UpdateUrl`, `RemoveUrl`, or `CrudUrl`/`BatchUrl` |
| `ODataV4Adaptor` | OData V4 services — uses `POST`/`PATCH`/`DELETE` automatically |
| `WebApiAdaptor` | ASP.NET Core Web API — maps to `GET`/`POST`/`PUT`/`DELETE` |
| `GraphQLAdaptor` | GraphQL servers — flexible query/mutation approach |

```razor
<SfGrid DataSource="@DataManager" Toolbar="@(new List<string>{ "Add","Edit","Delete","Update","Cancel" })">
    <SfDataManager Url="/api/orders" InsertUrl="/api/orders/insert"
                   UpdateUrl="/api/orders/update" RemoveUrl="/api/orders/remove"
                   Adaptor="Adaptors.UrlAdaptor">
    </SfDataManager>
    <GridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true">
    </GridEditSettings>
</SfGrid>
```

> For full server-side adaptor setup (including CRUD endpoints and request/response formats), see [url-adaptor.md](url-adaptor.md), [odatav4-adaptor.md](odatav4-adaptor.md), and [web-api-adaptor.md](web-api-adaptor.md).

## Editing Events

### Event Sequence (Normal / Dialog)

| Operation | Sequence |
|---|---|
| Add | `RowCreating` → `RowCreated` |
| Edit | `OnRowEditStart` → `OnBeginEdit` → `RowEditing` → `RowEdited` |
| Save | `RowUpdating` → `RowUpdated` |
| Delete | `RowDeleting` → `RowDeleted` |
| Cancel | `EditCanceling` → `EditCanceled` |

### Event Sequence (Cell)

| Operation | Sequence |
|---|---|
| Cell enters edit | `OnCellEdit` |
| Cell saves | `OnCellSave` → `CellSaved` |
| Edit canceled | `EditCanceling` → `EditCanceled` |

### Event Sequence (Batch)

| Operation | Sequence |
|---|---|
| Cell enters edit | `OnCellEdit` |
| Cell saves | `OnCellSave` → `CellSaved` |
| Batch save | `OnBatchSave` |
| Batch cancel | `OnBatchCancel` |

### Full Reference

| Event | Args | Cancelable | Description |
|---|---|---|---|
| `RowCreating` | `RowCreatingEventArgs<T>` | ✅ | Before new row added; set `args.Data` for defaults |
| `RowCreated` | `RowCreatedEventArgs<T>` | ❌ | After new row added |
| `OnRowEditStart` | `OnRowEditStartEventArgs<T>` | ❌ | Before row enters edit mode (data cloning setup) |
| `OnBeginEdit` | `BeginEditArgs<T>` | ❌ | Before edit UI renders; set `args.RowData` for custom clone |
| `RowEditing` | `RowEditingEventArgs<T>` | ✅ | Before edit performed; `args.Cancel = true` to block |
| `RowEdited` | `RowEditedEventArgs<T>` | ❌ | After row has entered edit mode |
| `RowUpdating` | `RowUpdatingEventArgs<T>` | ✅ | Before save; `args.Cancel = true` to block |
| `RowUpdated` | `RowUpdatedEventArgs<T>` | ❌ | After row saved to datasource |
| `RowDeleting` | `RowDeletingEventArgs<T>` | ✅ | Before delete; `args.Cancel = true` to block |
| `RowDeleted` | `RowDeletedEventArgs<T>` | ❌ | After row deleted |
| `EditCanceling` | `EditCancelingEventArgs<T>` | ✅ | Before cancel; `args.Cancel = true` to keep edit open |
| `EditCanceled` | `EditCanceledEventArgs<T>` | ❌ | After edit canceled |
| `OnCellEdit` | `CellEditArgs<T>` | ✅ | Cell mode (and Batch mode): before cell enters edit mode |
| `OnCellSave` | `CellSaveArgs<T>` | ✅ | Cell mode: before cell value is committed |
| `CellSaved` | `CellSavedArgs<T>` | ❌ | Cell mode (and Batch mode): after cell value is saved |

**Cancel delete with custom confirmation dialog:**

```razor
<GridEvents TValue="Order" RowDeleting="OnRowDeleting"></GridEvents>

@code {
    bool ShowConfirm = false;
    Order PendingDelete;

    async Task OnRowDeleting(RowDeletingEventArgs<Order> args)
    {
        args.Cancel = true;
        PendingDelete = args.Data;
        ShowConfirm = true;
    }

    async Task ConfirmDelete()
    {
        ShowConfirm = false;
        await Grid.DeleteRecordAsync("OrderID", PendingDelete);
    }
}
```
