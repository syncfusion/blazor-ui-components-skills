# Editing — Syncfusion Blazor Pivot Table

Cell editing lets users add, update, or delete raw data directly through the Pivot Table UI. Double-clicking a value cell opens a detail data grid in a popup showing the underlying records. CRUD operations performed there automatically recalculate pivot aggregates.

> **Applies to:** Relational data sources only.

## Table of Contents
- [Enable Editing](#enable-editing)
- [Edit Modes](#edit-modes)
- [Normal Mode](#normal-mode)
- [Dialog Mode](#dialog-mode)
- [Batch Mode](#batch-mode)
- [Command Columns Mode](#command-columns-mode)
- [Inline Editing](#inline-editing)
- [Confirmation Dialogs](#confirmation-dialogs)

---

## Enable Editing

> **⚠️ Correct Tag Name — Frequently Confused:**
>
> The editing configuration tag is **`PivotViewCellEditSettings`** — NOT `PivotViewEditSettings`. The incorrect tag does not exist and will silently fail or cause a compile error.
>
> ```razor
> <!-- ✅ CORRECT -->
> <PivotViewCellEditSettings AllowEditing="true" Mode="EditMode.Normal">
> </PivotViewCellEditSettings>
>
> <!-- ❌ WRONG — this tag does not exist -->
> <PivotViewEditSettings AllowEditing="true">
> </PivotViewEditSettings>
> ```

Add `PivotViewCellEditSettings` inside `SfPivotView`:

```razor
@using Syncfusion.Blazor.PivotView

<SfPivotView TValue="ProductDetails">
    <PivotViewDataSourceSettings DataSource="@data">
        <PivotViewColumns>
            <PivotViewColumn Name="Year"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="Country"></PivotViewRow>
            <PivotViewRow Name="Products"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Sold" Caption="Units Sold"></PivotViewValue>
            <PivotViewValue Name="Amount" Caption="Sold Amount"></PivotViewValue>
        </PivotViewValues>
    </PivotViewDataSourceSettings>
    <PivotViewCellEditSettings
        AllowAdding="true"
        AllowEditing="true"
        AllowDeleting="true"
        AllowEditOnDblClick="true"
        Mode="EditMode.Normal">
    </PivotViewCellEditSettings>
</SfPivotView>

@code {
    public List<ProductDetails> data { get; set; }
    protected override void OnInitialized() => data = ProductDetails.GetProductData().ToList();
}
```

**`PivotViewCellEditSettings` properties:**

| Property | Default | Purpose |
|---|---|---|
| `AllowEditing` | `false` | Enable editing existing records |
| `AllowAdding` | `false` | Enable adding new rows |
| `AllowDeleting` | `false` | Enable deleting rows |
| `AllowCommandColumns` | `false` | Show built-in Edit/Delete command buttons per row |
| `AllowEditOnDblClick` | `true` | Start editing by double-clicking a cell |
| `AllowInlineEditing` | `false` | Edit directly in the cell (not a popup row) |
| `ShowConfirmDialog` | `true` | Confirm before saving changes |
| `ShowDeleteConfirmDialog` | `false` | Confirm before deleting a record |
| `Mode` | `EditMode.Normal` | Edit mode type |

---

## Edit Modes

| Mode | When to use |
|---|---|
| `EditMode.Normal` | Default; one row at a time, selected row becomes editable inline |
| `EditMode.Dialog` | Opens a popup dialog for each edit — cleaner for many fields |
| `EditMode.Batch` | Accumulate multiple edits before saving; best for bulk updates |
| `EditMode.CommandColumn` | Adds Edit/Delete buttons in a dedicated column per row |

---

## Normal Mode

One row edits at a time in place:

```razor
<PivotViewCellEditSettings AllowEditing="true" AllowAdding="true"
    AllowDeleting="true" Mode="EditMode.Normal">
</PivotViewCellEditSettings>
```

---

## Dialog Mode

Opens a dialog per row for editing:

```razor
<PivotViewCellEditSettings AllowEditing="true" AllowAdding="true"
    AllowDeleting="true" Mode="EditMode.Dialog">
</PivotViewCellEditSettings>
```

---

## Batch Mode

Queue multiple edits and commit all at once with a single **Update** click:

```razor
<PivotViewCellEditSettings AllowEditing="true" AllowAdding="true"
    AllowDeleting="true" Mode="EditMode.Batch">
</PivotViewCellEditSettings>
```

> **Use case:** When users need to modify many records at once (e.g., bulk price adjustments) without triggering a pivot recalculation after each individual edit.

---

## Command Columns Mode

Adds dedicated Edit and Delete buttons in each row of the detail grid:

```razor
<PivotViewCellEditSettings AllowEditing="true" AllowAdding="true"
    AllowDeleting="true" AllowCommandColumns="true"
    Mode="EditMode.CommandColumn">
</PivotViewCellEditSettings>
```

---

## Inline Editing

Enable direct cell editing without a popup row transition:

```razor
<PivotViewCellEditSettings AllowEditing="true" AllowInlineEditing="true">
</PivotViewCellEditSettings>
```

---

## Confirmation Dialogs

Show confirmation prompts to prevent accidental changes:

```razor
<PivotViewCellEditSettings
    AllowEditing="true"
    AllowDeleting="true"
    ShowConfirmDialog="true"        <!-- Confirm before saving -->
    ShowDeleteConfirmDialog="true"  <!-- Confirm before deleting -->
    Mode="EditMode.Normal">
</PivotViewCellEditSettings>
```
