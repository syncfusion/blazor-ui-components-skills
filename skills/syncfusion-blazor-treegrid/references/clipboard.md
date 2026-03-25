# Clipboard in Syncfusion Blazor TreeGrid

> **Namespace note:** `CopyHierarchyType` and `BeforeCopyPasteEventArgs` belong to `Syncfusion.Blazor.TreeGrid`. Always qualify them as `Syncfusion.Blazor.TreeGrid.CopyHierarchyType.Both` and `Syncfusion.Blazor.TreeGrid.BeforeCopyPasteEventArgs` to avoid CS0104 ambiguity errors when both Grid and TreeGrid namespaces are in scope.

## Table of Contents
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Copy via External Button](#copy-via-external-button)
- [Copy Hierarchy Modes](#copy-hierarchy-modes)
- [Paste](#paste)
- [Clipboard Events](#clipboard-events)

---

## Keyboard Shortcuts

The TreeGrid supports built-in clipboard operations with no additional configuration:

| Shortcut | Action |
|---|---|
| `Ctrl + C` | Copy selected rows or cells to clipboard |
| `Ctrl + Shift + H` | Copy selected rows or cells **with column headers** |
| `Ctrl + V` | Paste copied cell values into selected cells |

---

## Copy via External Button

Use `CopyAsync` to trigger copy programmatically. Pass `true` to include column headers:

```razor
@using Syncfusion.Blazor.TreeGrid
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="Copy">Copy</SfButton>
<SfButton OnClick="CopyWithHeader">Copy With Header</SfButton>

<SfTreeGrid @ref="TreeGrid"
            DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"    HeaderText="Task ID"     Width="80"  IsPrimaryKey="true" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName"  HeaderText="Task Name"   Width="155" />
        <TreeGridColumn Field="StartDate" HeaderText="Start Date"  Width="100" Format="d" Type="Syncfusion.Blazor.Grids.ColumnType.Date" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Duration"  HeaderText="Duration"    Width="80"  TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress"  HeaderText="Progress"    Width="80"  TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    SfTreeGrid<BusinessObject> TreeGrid;
    public List<BusinessObject> TreeData { get; set; }

    protected override void OnInitialized()
    {
        TreeData = BusinessObject.GetSelfDataSource();
    }

    public async Task Copy()
    {
        await TreeGrid.CopyAsync();
    }

    public async Task CopyWithHeader()
    {
        await TreeGrid.CopyAsync(withHeader: true);
    }
}
```

---

## Copy Hierarchy Modes

`CopyHierarchyMode` controls which related rows are included in the clipboard output when a record is copied:

```razor
<SfTreeGrid DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            TreeColumnIndex="1"
            CopyHierarchyMode="CopyHierarchyType.Both">
    ...
</SfTreeGrid>
```

| Value | Behavior |
|---|---|
| `Parent` (default) | Selected records + their parent records |
| `Child` | Selected records + their child records |
| `Both` | Selected records + both parent and child records |
| `None` | Only the selected records — no related rows |

**Dynamic change via dropdown:**
```razor
@using Syncfusion.Blazor.TreeGrid;
@using Syncfusion.Blazor.DropDowns;

<SfDropDownList TValue="string" TItem="CopyModeOption"
                DataSource="@CopyModes"
                @bind-Value="@CopyMode">
    <DropDownListEvents TValue="string" TItem="CopyModeOption"
                        ValueChange="OnTypeChange" />
    <DropDownListFieldSettings Text="Text" Value="Value" />
</SfDropDownList>

<SfTreeGrid @ref="TreeGrid" CopyHierarchyMode="@CopyType" DataSource="@TreeData" IdMapping="TaskId" ParentIdMapping="ParentId" TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" Width="60" TextAlign="TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="80"></TreeGridColumn>
        <TreeGridColumn Field="StartDate" HeaderText="Start Date" Format="d" Type=ColumnType.Date Width="90" TextAlign="TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="Duration" HeaderText="Duration" Width="80" TextAlign="TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="80"></TreeGridColumn>
    </TreeGridColumns>
</SfTreeGrid>

@code {
    SfTreeGrid<BusinessObject> TreeGrid;

    public string CopyMode { get; set; } = "Parent";

    public CopyHierarchyType CopyType { get; set; } = CopyHierarchyType.Parent;

    public List<BusinessObject> TreeData { get; set; }

    public List<CopyModeOption> CopyModes { get; set; } = new List<CopyModeOption>();

    public class CopyModeOption
    {
        public string Value { get; set; }

        public string Text { get; set; }
    }

    protected override void OnInitialized()
    {
        this.TreeData = BusinessObject.GetSelfDataSource().ToList();

        this.CopyModes.Add(new CopyModeOption() { Value = "Parent", Text = "Parent" });
        this.CopyModes.Add(new CopyModeOption() { Value = "Child", Text = "Child" });
        this.CopyModes.Add(new CopyModeOption() { Value = "Both", Text = "Both" });
        this.CopyModes.Add(new CopyModeOption() { Value = "None", Text = "None" });
    }

    private void OnTypeChange(Syncfusion.Blazor.DropDowns.ChangeEventArgs<string, CopyModeOption> args)
    {
        if (args.Value == "Parent")
        {
            CopyType = CopyHierarchyType.Parent;
        }
        else if (args.Value == "Child")
        {
            CopyType = CopyHierarchyType.Child;
        }
        else if (args.Value == "Both")
        {
            CopyType = CopyHierarchyType.Both;
        }
        else if (args.Value == "None")
        {
            CopyType = CopyHierarchyType.None;
        }
    }
    
    public class BusinessObject
    {
        public int TaskId { get; set; }
        public string TaskName { get; set; }
        public DateTime? StartDate { get; set; }
        public int? Duration { get; set; }
        public int? Progress { get; set; }
        public string Priority { get; set; }
        public int? ParentId { get; set; }

        public static List<BusinessObject> GetSelfDataSource()
        {
            List<BusinessObject> BusinessObjectCollection = new List<BusinessObject>();
            BusinessObjectCollection.Add(new BusinessObject() { TaskId = 1, TaskName = "Parent Task 1", StartDate = new DateTime(2017, 10, 23), Duration = 10, Progress = 70, Priority = "Critical", ParentId = null });
            BusinessObjectCollection.Add(new BusinessObject() { TaskId = 2, TaskName = "Child task 1", StartDate = new DateTime(2017, 10, 23), Duration = 12, Progress = 80, Priority = "Low", ParentId = 1 });
            BusinessObjectCollection.Add(new BusinessObject() { TaskId = 3, TaskName = "Child Task 2", StartDate = new DateTime(2017, 10, 24), Duration = 5, Progress = 65, Priority = "Critical", ParentId = 2 });
            BusinessObjectCollection.Add(new BusinessObject() { TaskId = 4, TaskName = "Child task 3", StartDate = new DateTime(2017, 10, 25), Duration = 6, Priority = "High", Progress = 77, ParentId = 3 });
            BusinessObjectCollection.Add(new BusinessObject() { TaskId = 5, TaskName = "Child Task 5", StartDate = new DateTime(2017, 10, 26), Duration = 9, Progress = 25, ParentId = 4, Priority = "Normal" });
            BusinessObjectCollection.Add(new BusinessObject() { TaskId = 6, TaskName = "Child Task 6", StartDate = new DateTime(2017, 10, 27), Duration = 9, Progress = 7, ParentId = 5, Priority = "Normal" });
            BusinessObjectCollection.Add(new BusinessObject() { TaskId = 7, TaskName = "Parent Task 3", StartDate = new DateTime(2017, 10, 28), Duration = 4, Progress = 45, ParentId = null, Priority = "High" });
            BusinessObjectCollection.Add(new BusinessObject() { TaskId = 8, TaskName = "Child Task 7", StartDate = new DateTime(2017, 10, 29), Duration = 3, Progress = 38, ParentId = 7, Priority = "Critical" });
            BusinessObjectCollection.Add(new BusinessObject() { TaskId = 9, TaskName = "Child Task 8", StartDate = new DateTime(2017, 10, 30), Duration = 7, Progress = 70, ParentId = 7, Priority = "Low" });
            return BusinessObjectCollection;
        }
    }
}
```

---

## Paste

Copy a group of cells with `Ctrl + C`, then select a target cell and press `Ctrl + V` to paste.

**Requirements for paste to work:**
- Selection `Mode="Cell"` and `CellSelectionMode="Box"`
- `EditMode.Batch` editing enabled

```razor
<SfTreeGrid DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            TreeColumnIndex="1"
            AllowPaging="true"
            Toolbar="@(new List<string> { "Add", "Delete", "Update", "Cancel" })">
    <TreeGridPageSettings PageSize="5" />
    <TreeGridSelectionSettings
        Type="Syncfusion.Blazor.Grids.SelectionType.Multiple"
        Mode="Syncfusion.Blazor.Grids.SelectionMode.Cell"
        CellSelectionMode="Syncfusion.Blazor.Grids.CellSelectionMode.Box" />
    <TreeGridEditSettings
        AllowAdding="true"
        AllowEditing="true"
        AllowDeleting="true"
        Mode="EditMode.Batch" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"    HeaderText="Task ID"    Width="80"  IsPrimaryKey="true" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName"  HeaderText="Task Name"  Width="155" />
        <TreeGridColumn Field="StartDate" HeaderText="Start Date" Width="100" Format="d" Type="Syncfusion.Blazor.Grids.ColumnType.Date" EditType="Syncfusion.Blazor.Grids.EditType.DatePickerEdit" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Duration"  HeaderText="Duration"   Width="80"  TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress"  HeaderText="Progress"   Width="80"  TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Priority"  HeaderText="Priority"   Width="80"  />
    </TreeGridColumns>
</SfTreeGrid>
```

**Paste limitations:**
| Scenario | Result |
|---|---|
| String → Number column | Displays as `NaN` |
| String → Date column | Displays as empty cell |

---

## Clipboard Events

Use `TreeGridEvents` to intercept clipboard operations:

| Event | Trigger | Can Cancel? |
|---|---|---|
| `BeforeCopyPaste` | Before any copy or paste action | ✅ — cancels entire copy/paste |
| `BeforeCellPaste` | Before pasting into each individual cell | ✅ — can cancel or change the value per cell |

```razor
@using Syncfusion.Blazor.TreeGrid

<TreeGridEvents TValue="BusinessObject"
                BeforeCopyPaste="OnBeforeCopyPaste"
                BeforeCellPaste="OnBeforeCellPaste">
</TreeGridEvents>

@code {
    public void OnBeforeCopyPaste(Syncfusion.Blazor.TreeGrid.BeforeCopyPasteEventArgs args)
    {
        // args.Cancel = true; // Prevent the entire copy/paste
    }

    public void OnBeforeCellPaste(Syncfusion.Blazor.TreeGrid.BeforeCellPasteEventArgs<BusinessObject> args)
    {
        // args.Cancel = true; // Skip paste for this specific cell
        // args.Value = ...; // Or override the value being pasted
    }
}
```

---
