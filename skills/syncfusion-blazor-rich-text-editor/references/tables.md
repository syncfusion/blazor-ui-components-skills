# Tables

## Table of Contents
- [Inserting a Table](#inserting-a-table)
- [Table Settings Properties](#table-settings-properties)
- [Table Quick Toolbar](#table-quick-toolbar)
- [TableToolbarCommand Enum Reference](#tabletoolbarcommand-enum-reference)
- [Row and Column Selection](#row-and-column-selection)
- [Merge and Split Cells](#merge-and-split-cells)
- [Custom Table Styles](#custom-table-styles)

---

## Inserting a Table

Add `ToolbarCommand.CreateTable` to the toolbar. Users then click/drag on a grid to select rows and columns (or type numbers in the popup dialog on touch devices):

```razor
<SfRichTextEditor>
    <RichTextEditorToolbarSettings Items="@Tools" />
</SfRichTextEditor>

@code {
    private List<ToolbarItemModel> Tools = new()
    {
        new() { Command = ToolbarCommand.Bold },
        new() { Command = ToolbarCommand.Italic },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.CreateTable },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Undo },
        new() { Command = ToolbarCommand.Redo }
    };
}
```

---

## Table Settings Properties

Use `RichTextEditorTableSettings` to control default table behaviour:

| Property | Type | Default | Description |
|---|---|---|---|
| `Width` | `string` | `"100%"` | Default table width on insert |
| `MinWidth` | `double` | `0` | Minimum table width |
| `MaxWidth` | `double?` | `null` | Maximum table width |
| `EnableResize` | `bool` | `true` | Allow resizing columns by dragging |
| `Styles` | `List<DropDownItemModel>` | Built-in | Border/style presets shown in quick toolbar |

```razor
<SfRichTextEditor>
    <RichTextEditorTableSettings Width="80%" MinWidth="200" EnableResize="true" />
</SfRichTextEditor>
```

---

## Table Quick Toolbar

Configure which actions appear when a table is selected. Include `TableToolbarCommand.TableCell` to expose merge/split:

```razor
<SfRichTextEditor>
    <RichTextEditorToolbarSettings Items="@Tools" />
    <RichTextEditorQuickToolbarSettings Table="@TableTools" />
</SfRichTextEditor>

@code {
    private List<ToolbarItemModel> Tools = new()
    {
        new() { Command = ToolbarCommand.CreateTable }
    };

    private List<TableToolbarItemModel> TableTools = new()
    {
        new() { Command = TableToolbarCommand.TableHeader },
        new() { Command = TableToolbarCommand.TableRemove },
        new() { Command = TableToolbarCommand.Separator },
        new() { Command = TableToolbarCommand.TableRows },
        new() { Command = TableToolbarCommand.TableColumns },
        new() { Command = TableToolbarCommand.TableCell },        // enables merge/split
        new() { Command = TableToolbarCommand.Separator },
        new() { Command = TableToolbarCommand.Styles },
        new() { Command = TableToolbarCommand.BackgroundColor },
        new() { Command = TableToolbarCommand.Alignments },
        new() { Command = TableToolbarCommand.TableCellVerticalAlign }
    };
}
```

---

## TableToolbarCommand Enum Reference

Complete list of all valid `TableToolbarCommand` values:

| Value | Enum Name | Description |
|---|---|---|
| `0` | `HorizontalSeparator` | Legacy separator — organizes toolbar into segments |
| `1` | `TableHeader` | Toggle the header row of the table |
| `2` | `TableRows` | Add or remove rows (above / below) |
| `3` | `TableColumns` | Add or remove columns (left / right) |
| `4` | `BackgroundColor` | Set background color of selected cell(s) |
| `5` | `TableRemove` | Delete the entire table from the editor |
| `6` | `Alignments` | Align table cell content (left / center / right) |
| `7` | `TableCellVerticalAlign` | Adjust vertical alignment of cell content (top / middle / bottom) |
| `8` | `Styles` | Apply predefined border/style presets to the table |
| `9` | `TableEditProperties` | Open dialog to edit table attributes (dimensions, padding, etc.) |
| `10` | `TableCell` | Manage cell merge / split operations |
| `11` | `Separator` | Visual separator to group toolbar items into logical sections |

## Row and Column Selection

**Mouse:** Click and drag across cells to select multiple rows or columns.

**Keyboard:**
- `Shift + Arrow keys` — extend selection across cells.
- `Tab` — move to next cell; auto-inserts a row when in the last cell.
- `Shift + Tab` — move to previous cell.

**Select whole table:**
- Press `Backspace` immediately after the table.
- Press `Delete` immediately before the table.

---

## Merge and Split Cells

Merging and splitting requires `TableToolbarCommand.TableCell` to be in the quick toolbar.

1. Select two or more adjacent cells by clicking and dragging (or `Shift + Arrow`).
2. Click **Merge Cells** from the quick toolbar dropdown that appears.

To split a previously merged cell:
1. Click the merged cell.
2. Choose **Split Cell** from the quick toolbar.

---

## Custom Table Styles

Supply a `Styles` list on `RichTextEditorTableSettings` to add style options to the quick toolbar dropdown. Each item's `Value` should be a CSS class name you define in your stylesheet:

```razor
<SfRichTextEditor>
    <RichTextEditorTableSettings Styles="@TableStyles" />
</SfRichTextEditor>

@code {
    private List<DropDownItemModel> TableStyles = new()
    {
        new() { Text = "Dashed",        Value = "e-dashed-borders" },
        new() { Text = "Alternate",     Value = "e-alternate-rows" }
    };
}
```

```css
/* In your app's CSS */
.e-richtexteditor .e-rte-table.e-dashed-borders td,
.e-richtexteditor .e-rte-table.e-dashed-borders th {
    border-style: dashed;
}
.e-richtexteditor .e-rte-table.e-alternate-rows tbody tr:nth-child(2n) {
    background-color: #f4f4f4;
}
```

> Table styles are applied as CSS classes on the `<table>` element and are preserved in the saved HTML output.

---

## Cell Background Color

Use `TableToolbarCommand.BackgroundColor` in the table quick toolbar to set per-cell background colour from a colour picker. Combine with `Alignments`, `TableCellVerticalAlign`, and `TableEditProperties` for full cell-level control:

```razor
<SfRichTextEditor>
    <RichTextEditorQuickToolbarSettings Table="@TableTools" />
</SfRichTextEditor>

@code {
    private List<TableToolbarItemModel> TableTools = new()
    {
        new() { Command = TableToolbarCommand.BackgroundColor },
        new() { Command = TableToolbarCommand.Alignments },
        new() { Command = TableToolbarCommand.TableCellVerticalAlign },
        new() { Command = TableToolbarCommand.Separator },
        new() { Command = TableToolbarCommand.TableEditProperties },
        new() { Command = TableToolbarCommand.TableRemove }
    };
}
```

---

## Nesting Tables

Place the cursor inside a table cell and click **Create Table** again to insert a nested table. Nesting is useful for complex layouts such as structured forms or hierarchical data:

```razor
<!-- Pre-set nested table in Value -->
<SfRichTextEditor Value="@NestedHtml">
    <RichTextEditorToolbarSettings Items="@Tools" />
</SfRichTextEditor>

@code {
    private string NestedHtml = @"
<table border='1' style='width:100%;border-collapse:collapse;'>
  <tr><th>Department</th><th>Details</th></tr>
  <tr>
    <td>Sales</td>
    <td>
      <table border='1' style='width:100%;border-collapse:collapse;'>
        <tr><th>Employee</th><th>Target</th></tr>
        <tr><td>John Doe</td><td>$50,000</td></tr>
      </table>
    </td>
  </tr>
</table>";
}
```

---

## Quick Insert (Hover Icons)

- **Add column:** Hover over a cell in the **first row** → a dot appears at the top edge → hover the dot → click **+** to insert a column to the left.
- **Add row:** Hover over a cell in the **first column** → a dot appears at the left edge → hover the dot → click **+** to insert a row above.

---

## Copy / Cut / Paste Table Content

Select cells by dragging or `Shift + Arrow`, then use standard shortcuts:

| Action | Windows | Mac |
|---|---|---|
| Copy | `Ctrl + C` | `⌘ + C` |
| Cut | `Ctrl + X` | `⌘ + X` |
| Paste | `Ctrl + V` | `⌘ + V` |

Paste behaviour:
- Preserves table structure, formatting and cell properties
- Supports cross-table operations
- Compatible with content pasted from Excel and Word
- Handles partial table pastes as new tables or into existing cells
