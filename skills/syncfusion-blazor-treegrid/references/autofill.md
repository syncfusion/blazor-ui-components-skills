# AutoFill in Syncfusion Blazor TreeGrid

## Table of Contents
- [Overview](#overview)
- [Enable AutoFill](#enable-autofill)
- [Requirements](#requirements)
- [Limitations](#limitations)

---

## Overview

AutoFill allows copying selected cell values to adjacent cells by dragging the autofill icon — similar to Excel's fill-handle behavior. After selecting a range of cells, a handle appears at the bottom-right corner; drag it up or down to fill adjacent cells with the same values.

---

## Enable AutoFill

Set `EnableAutoFill="true"` on the grid. **Three conditions must all be met** for the feature to activate:

```razor
@using Syncfusion.Blazor.TreeGrid

<SfTreeGrid DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            TreeColumnIndex="1"
            EnableAutoFill="true"
            AllowPaging="true"
            Toolbar="@(new List<string> { "Add", "Delete", "Update", "Cancel" })">
    
    <TreeGridPageSettings PageSize="5" />

    <!-- Condition 2: Cell selection in Box mode -->
    <TreeGridSelectionSettings
        Type="Syncfusion.Blazor.Grids.SelectionType.Multiple"
        Mode="Syncfusion.Blazor.Grids.SelectionMode.Cell"
        CellSelectionMode="Syncfusion.Blazor.Grids.CellSelectionMode.Box" />

    <!-- Condition 3: Batch editing mode -->
    <TreeGridEditSettings
        AllowAdding="true"
        AllowEditing="true"
        AllowDeleting="true"
        Mode="EditMode.Batch" />

    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"    HeaderText="Task ID"    Width="80"  IsPrimaryKey="true" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName"  HeaderText="Task Name"  Width="155" />
        <TreeGridColumn Field="StartDate" HeaderText="Start Date" Width="100" Format="d"
                        Type="Syncfusion.Blazor.Grids.ColumnType.Date"
                        EditType="Syncfusion.Blazor.Grids.EditType.DatePickerEdit"
                        TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Duration"  HeaderText="Duration"   Width="80"  TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress"  HeaderText="Progress"   Width="80"  TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Priority"  HeaderText="Priority"   Width="80"  />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public List<BusinessObject> TreeData { get; set; }

    protected override void OnInitialized()
    {
        TreeData = BusinessObject.GetSelfDataSource();
    }
}
```

---

## Requirements

All three conditions must be satisfied simultaneously for the autofill handle to appear:

| # | Requirement | Configuration |
|---|---|---|
| 1 | AutoFill enabled | `EnableAutoFill="true"` on `SfTreeGrid` |
| 2 | Cell selection in Box mode | `Mode="Cell"` + `CellSelectionMode="Box"` on `TreeGridSelectionSettings` |
| 3 | Batch editing enabled | `Mode="EditMode.Batch"` on `TreeGridEditSettings` |

> If any one of these is missing, the autofill icon will not appear on cell selection.

---

## Limitations

| Scenario | Behavior |
|---|---|
| String value → Number column | Cell displays as `NaN` — string is not parsed to a number |
| String value → Date column | Cell displays as empty — string is not parsed to a date |
| Sequential/linear series | Not supported — values are copied as-is, no increment (1, 2, 3...) |
| Edit mode other than Batch | AutoFill does not work — Batch editing is mandatory |

---
