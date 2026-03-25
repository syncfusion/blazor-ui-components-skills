# Clipboard and Accessibility — Developer Reference

Comprehensive guide to clipboard operations, copy/paste functionality, autofill features, and accessibility best practices for the Blazor Gantt Chart component.

---

## Table of Contents

- [Clipboard Operations](#clipboard-operations)
- [Copy Methods](#copy-methods)
- [Copy Hierarchy Modes](#copy-hierarchy-modes)
- [Paste Operations](#paste-operations)
- [Autofill Feature](#autofill-feature)
- [Clipboard Properties](#clipboard-properties)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Accessibility Guidelines](#accessibility-guidelines)
- [ARIA and Screen Readers](#aria-and-screen-readers)
- [Best Practices](#best-practices)

---

## Clipboard Operations

The clipboard feature enables copying selected row or cell data from the Gantt Chart component to the clipboard for use in external applications.

### Basic Clipboard Usage

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="1000px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" 
                     EndDate="EndDate" Duration="Duration" Progress="Progress">
    </GanttTaskFields>
</SfGantt>
```

**Default keyboard shortcuts:**
- **Ctrl + C** — Copy selected rows or cells
- **Ctrl + Shift + H** — Copy with header row

### Selection Prerequisites

Clipboard operations require proper selection configuration:

```razor
<SfGantt DataSource="@TaskCollection">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate">
    </GanttTaskFields>
    <GanttSelectionSettings Mode="SelectionMode.Cell" Type="SelectionType.Multiple">
    </GanttSelectionSettings>
</SfGantt>
```

**Selection modes for clipboard:**
- **Row selection** — Copies entire rows with all column values
- **Cell selection** — Copies individual cell values
- **Multiple selection** — Enables copying multiple rows/cells

---

## Copy Methods

### CopyAsync Method

**Signature:**
```csharp
public Task CopyAsync(bool? withHeader = null)
```

**Parameters:**
- `withHeader` (bool?, optional) — When `true`, includes column headers in copied data. Default: `false`

**Returns:** `Task` — Asynchronous operation

**Description:** Copies selected rows or cells to the clipboard. Can be invoked programmatically through external buttons or custom logic.

### Programmatic Copy Example

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Navigations

<SfGantt @ref="Gantt" DataSource="@TaskCollection" Height="450px" Width="1000px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" 
                     EndDate="EndDate" Duration="Duration">
    </GanttTaskFields>
    <SfToolbar ID="Gantt_Gantt_Toolbar">
        <ToolbarItems>
            <ToolbarItem Id="copyHeader" Text="Copy With Header" 
                         OnClick="ToolbarClick" PrefixIcon="e-copy">
            </ToolbarItem>
            <ToolbarItem Id="copy" Text="Copy" 
                         OnClick="ToolbarClick" PrefixIcon="e-copy">
            </ToolbarItem>
        </ToolbarItems>
    </SfToolbar>
</SfGantt>

@code {
    private SfGantt<TaskData> Gantt;
    private List<TaskData> TaskCollection { get; set; }

    public async void ToolbarClick(ClickEventArgs args)
    {
        var selectedRecords = await Gantt.GetSelectedRecordsAsync();
        if (selectedRecords.Count() > 0)
        {
            bool withHeader = (args.Item.Id == "copyHeader");
            await Gantt.CopyAsync(withHeader);
        }
    }

    public class TaskData
    {
        public int TaskID { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public DateTime? EndDate { get; set; }
        public string Duration { get; set; }
        public int Progress { get; set; }
    }

    protected override void OnInitialized()
    {
        TaskCollection = GetTaskCollection();
    }

    private List<TaskData> GetTaskCollection()
    {
        return new List<TaskData>()
        {
            new TaskData() { 
                TaskID = 1, 
                TaskName = "Project initiation", 
                StartDate = new DateTime(2022, 04, 05), 
                Duration = "3", 
                Progress = 30 
            },
            new TaskData() { 
                TaskID = 2, 
                TaskName = "Identify Site location", 
                StartDate = new DateTime(2022, 04, 05), 
                Duration = "2", 
                Progress = 50 
            }
        };
    }
}
```

---

## Copy Hierarchy Modes

The Gantt Chart supports multiple hierarchical copy modes via the `CopyHierarchyMode` property.

### CopyHierarchyType Enum

```csharp
public enum CopyHierarchyType
{
    Parent,  // Copy selected records with parent records
    Child,   // Copy selected records with child records
    Both,    // Copy selected records with both parents and children
    None     // Copy only selected records
}
```

### Mode Descriptions

| Mode | Behavior | Use Case |
|------|----------|----------|
| **Parent** | Copies selected records + all parent ancestors | Preserve task context when copying subtasks |
| **Child** | Copies selected records + all child descendants | Export parent with complete task breakdown |
| **Both** | Copies selected records + parents + children | Full hierarchy export for external analysis |
| **None** | Copies only selected records (no hierarchy) | Flat data export without relationships |

### Hierarchy Mode Example

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.TreeGrid
@using Syncfusion.Blazor.DropDowns

<SfDropDownList TValue="string" TItem="DropdownData" @bind-Value="@CopyMode" 
                DataSource="@CopyModes" Width="200px">
    <DropDownListEvents TValue="string" TItem="DropdownData" 
                        ValueChange="OnTypeChange">
    </DropDownListEvents>
    <DropDownListFieldSettings Text="Mode" Value="Id">
    </DropDownListFieldSettings>
</SfDropDownList>

<SfGantt @ref="Gantt" DataSource="@TaskCollection" 
         CopyHierarchyMode="@CopyType" Height="450px" Width="1000px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" 
                     Duration="Duration" ParentID="ParentID">
    </GanttTaskFields>
    <SfToolbar ID="Gantt_Gantt_Toolbar">
        <ToolbarItems>
            <ToolbarItem Id="copy" Text="Copy" OnClick="ToolbarClick" 
                         PrefixIcon="e-copy">
            </ToolbarItem>
        </ToolbarItems>
    </SfToolbar>
</SfGantt>

@code {
    private SfGantt<TaskData> Gantt;
    private List<TaskData> TaskCollection { get; set; }
    public string CopyMode { get; set; } = "Parent";
    public CopyHierarchyType CopyType { get; set; } = CopyHierarchyType.Parent;
    public List<DropdownData> CopyModes { get; set; } = new List<DropdownData>();

    public class DropdownData
    {
        public string Id { get; set; }
        public string Mode { get; set; }
    }

    protected override void OnInitialized()
    {
        TaskCollection = GetTaskCollection();
        CopyModes.Add(new DropdownData() { Id = "Parent", Mode = "Parent" });
        CopyModes.Add(new DropdownData() { Id = "Child", Mode = "Child" });
        CopyModes.Add(new DropdownData() { Id = "Both", Mode = "Both" });
        CopyModes.Add(new DropdownData() { Id = "None", Mode = "None" });
    }

    private void OnTypeChange(ChangeEventArgs<string, DropdownData> args)
    {
        CopyType = args.Value switch
        {
            "Parent" => CopyHierarchyType.Parent,
            "Child" => CopyHierarchyType.Child,
            "Both" => CopyHierarchyType.Both,
            "None" => CopyHierarchyType.None,
            _ => CopyHierarchyType.Parent
        };
    }

    public async void ToolbarClick(ClickEventArgs args)
    {
        var selectedRecords = await Gantt.GetSelectedRecordsAsync();
        if (selectedRecords.Count() > 0)
        {
            await Gantt.CopyAsync(false);
        }
    }

    public class TaskData
    {
        public int TaskID { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public string Duration { get; set; }
        public int? ParentID { get; set; }
    }

    private List<TaskData> GetTaskCollection()
    {
        return new List<TaskData>()
        {
            new TaskData() { TaskID = 1, TaskName = "Project initiation", 
                             StartDate = new DateTime(2022, 04, 05), Duration = "5" },
            new TaskData() { TaskID = 2, TaskName = "Identify Site location", 
                             StartDate = new DateTime(2022, 04, 05), Duration = "2", ParentID = 1 },
            new TaskData() { TaskID = 3, TaskName = "Perform soil test", 
                             StartDate = new DateTime(2022, 04, 07), Duration = "3", ParentID = 1 }
        };
    }
}
```

---

## Paste Operations

The Gantt Chart supports custom paste operations for both **row paste** (full record duplication) and **cell paste** (value copying across cells).

### Row Paste

Copy and paste entire rows with hierarchy preservation.

**Keyboard shortcuts:**
- **Ctrl + C** — Copy selected row(s)
- **Ctrl + V** — Paste copied row(s)

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Grids

<SfGantt @ref="Gantt" DataSource="@TaskCollection" @onkeyup="KeyUp" 
         Height="450px" Width="1000px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" 
                     Duration="Duration" ParentID="ParentID">
    </GanttTaskFields>
    <GanttSelectionSettings Type="SelectionType.Multiple">
    </GanttSelectionSettings>
    <GanttEditSettings AllowAdding="true"></GanttEditSettings>
    <GanttEvents BeforeCopy="BeforeCopyHandler" 
                 RowSelected="RowSelect" 
                 RowDeselected="RowDeselect" 
                 TValue="TaskData">
    </GanttEvents>
</SfGantt>

@code {
    private SfGantt<TaskData> Gantt;
    private List<TaskData> TaskCollection { get; set; }
    public List<TaskData> CopiedRecords { get; set; } = new List<TaskData>();
    public double SelectedIndex { get; set; }

    public void RowDeselect(RowDeselectEventArgs<TaskData> args)
    {
        SelectedIndex = -1;
    }

    private void BeforeCopyHandler(BeforeCopyEventArgs args)
    {
        var columns = Gantt.GetColumnsAsync().Result;
        var clipboardText = args.ClipboardText;
        
        if (!string.IsNullOrEmpty(clipboardText))
        {
            var records = clipboardText.Split("\n");
            int index = 0;
            
            foreach (var record in records)
            {
                var columnValues = record.Split("\t");
                int colIndex = 0;
                int newId = TaskCollection.Max(a => a.TaskID) + 1;
                TaskData ganttData = new TaskData();
                
                foreach (var col in columns)
                {
                    if (col.Field == Gantt.TaskFields.Id)
                    {
                        ganttData.GetType().GetProperty(col.Field)
                            .SetValue(ganttData, newId + index);
                    }
                    else if (col.Type == Syncfusion.Blazor.Grids.ColumnType.Date)
                    {
                        ganttData.GetType().GetProperty(col.Field)
                            .SetValue(ganttData, Convert.ToDateTime(columnValues[colIndex]));
                    }
                    else if (col.Type == Syncfusion.Blazor.Grids.ColumnType.String)
                    {
                        ganttData.GetType().GetProperty(col.Field)
                            .SetValue(ganttData, columnValues[colIndex]);
                    }
                    colIndex++;
                }
                index++;
                CopiedRecords.Add(ganttData);
            }
        }
    }

    public void RowSelect(RowSelectEventArgs<TaskData> args)
    {
        SelectedIndex = args.RowIndex;
    }

    private async Task KeyUp(KeyboardEventArgs args)
    {
        if (args.CtrlKey && args.Code == "KeyV" 
            && CopiedRecords.Count > 0 && SelectedIndex > -1)
        {
            var parentID = TaskCollection[(int)SelectedIndex].ParentID;
            
            for (var i = 0; i < CopiedRecords.Count; i++)
            {
                CopiedRecords[i].ParentID = parentID;
                await Gantt.AddRecordAsync(CopiedRecords[i], (int)SelectedIndex, 
                                           RowPosition.Above);
            }
            
            CopiedRecords = new List<TaskData>();
        }
    }

    public class TaskData
    {
        public int TaskID { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public string Duration { get; set; }
        public int Progress { get; set; }
        public int? ParentID { get; set; }
    }

    protected override void OnInitialized()
    {
        TaskCollection = GetTaskCollection();
    }

    private List<TaskData> GetTaskCollection()
    {
        return new List<TaskData>()
        {
            new TaskData() { TaskID = 1, TaskName = "Project initiation", 
                             StartDate = new DateTime(2022, 04, 05), Duration = "3" },
            new TaskData() { TaskID = 2, TaskName = "Identify Site", 
                             StartDate = new DateTime(2022, 04, 05), Duration = "2", ParentID = 1 }
        };
    }
}
```

### Cell Paste

Copy and paste individual cell values across multiple cells.

**Keyboard shortcuts:**
- **Ctrl + C** — Copy selected cell(s)
- **Ctrl + V** — Paste to target cell(s)

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Grids

<SfGantt @ref="Gantt" DataSource="@TaskCollection" 
         @onkeydown="KeyDown" @onkeyup="KeyUp" 
         Height="450px" Width="1000px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" 
                     Duration="Duration" Progress="Progress">
    </GanttTaskFields>
    <GanttSelectionSettings Mode="SelectionMode.Cell" 
                            Type="SelectionType.Multiple">
    </GanttSelectionSettings>
    <GanttEditSettings AllowEditing="true"></GanttEditSettings>
    <GanttEvents CellSelected="CellSelected" 
                 CellDeselected="CellDeselected" 
                 TValue="TaskData">
    </GanttEvents>
</SfGantt>

@code {
    private SfGantt<TaskData> Gantt;
    private List<TaskData> TaskCollection { get; set; }
    public int SelectedIndex { get; set; }
    private List<ValueTuple<int, int>> clonedRecordIndex;

    public void CellDeselected(CellDeselectEventArgs<TaskData> args)
    {
        SelectedIndex = -1;
    }

    public void CellSelected(CellSelectEventArgs<TaskData> args)
    {
        SelectedIndex = Convert.ToInt32(args.RowIndex);
    }

    private async Task KeyDown(KeyboardEventArgs args)
    {
        if (args.CtrlKey && args.Code == "KeyC")
        {
            clonedRecordIndex = await Gantt.GetSelectedRowCellIndexesAsync();
        }
    }

    private async Task KeyUp(KeyboardEventArgs args)
    {
        if (args.CtrlKey && args.Code == "KeyV" 
            && clonedRecordIndex != null && clonedRecordIndex.Count > 0 
            && SelectedIndex > -1)
        {
            var columns = await Gantt.GetColumnsAsync();
            var currentRecords = Gantt.GetCurrentViewRecords();
            
            // Group cell indexes by row
            IDictionary<double, List<double>> clonedRecords = 
                new Dictionary<double, List<double>>();
            
            foreach (var cellIndex in clonedRecordIndex)
            {
                if (!clonedRecords.ContainsKey(cellIndex.Item1))
                {
                    clonedRecords[cellIndex.Item1] = new List<double>();
                }
                clonedRecords[cellIndex.Item1].Add(cellIndex.Item2);
            }
            
            // Paste to target rows
            for (int i = 0; i < clonedRecords.Count; i++)
            {
                double sourceRowIndex = clonedRecords.ElementAt(i).Key;
                List<double> cellIndexes = clonedRecords.ElementAt(i).Value;
                TaskData sourceRecord = currentRecords[Convert.ToInt32(sourceRowIndex)];
                
                if (SelectedIndex + i < currentRecords.Count)
                {
                    TaskData targetRecord = currentRecords[SelectedIndex + i];
                    
                    foreach (var cellIdx in cellIndexes)
                    {
                        GanttColumn col = columns[Convert.ToInt32(cellIdx)];
                        
                        if (!col.IsPrimaryKey)
                        {
                            var value = sourceRecord.GetType().GetProperty(col.Field)
                                .GetValue(sourceRecord);
                            targetRecord.GetType().GetProperty(col.Field)
                                .SetValue(targetRecord, value);
                        }
                    }
                    
                    await Gantt.UpdateRecordByIDAsync(targetRecord);
                }
            }
        }
    }

    public class TaskData
    {
        public int TaskID { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public string Duration { get; set; }
        public int Progress { get; set; }
    }

    protected override void OnInitialized()
    {
        TaskCollection = GetTaskCollection();
    }

    private List<TaskData> GetTaskCollection()
    {
        return new List<TaskData>()
        {
            new TaskData() { TaskID = 1, TaskName = "Task 1", 
                             StartDate = new DateTime(2022, 04, 05), 
                             Duration = "3", Progress = 30 },
            new TaskData() { TaskID = 2, TaskName = "Task 2", 
                             StartDate = new DateTime(2022, 04, 08), 
                             Duration = "4", Progress = 50 }
        };
    }
}
```

---

## Autofill Feature

The autofill feature enables quickly filling multiple cells with the same value through drag selection or keyboard shortcuts.

### Configuration Requirements

To enable autofill, configure the following selection settings:

```razor
<GanttSelectionSettings AllowDragSelection="true"
                        Mode="SelectionMode.Cell"
                        Type="SelectionType.Multiple"
                        CellSelectionMode="CellSelectionMode.Box">
</GanttSelectionSettings>
```

**Property details:**
- `AllowDragSelection` — Enables drag-to-select functionality
- `Mode` — Must be `Cell` for cell-level operations
- `Type` — Must be `Multiple` for multi-cell selection
- `CellSelectionMode` — Use `Box` for rectangular selection

### Autofill Keyboard Shortcut

**Default shortcut:** **Alt + Drag** or **Alt (hold)** during multi-cell selection

### Autofill Implementation

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Grids

<SfGantt @ref="Gantt" DataSource="@TaskCollection" @onkeyup="KeyUp" 
         Height="450px" Width="1000px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" 
                     Duration="Duration" Progress="Progress">
    </GanttTaskFields>
    <GanttSelectionSettings AllowDragSelection="true"
                            Mode="SelectionMode.Cell"
                            Type="SelectionType.Multiple"
                            CellSelectionMode="CellSelectionMode.Box">
    </GanttSelectionSettings>
    <GanttEditSettings AllowEditing="true"></GanttEditSettings>
    <GanttEvents CellSelected="CellSelectedHandler" 
                 CellDeselected="CellDeselectedHandler" 
                 TValue="TaskData">
    </GanttEvents>
</SfGantt>

@code {
    private SfGantt<TaskData> Gantt;
    private List<TaskData> TaskCollection { get; set; }
    private object Value { get; set; }
    private string ColumnField { get; set; }

    private void CellSelectedHandler(CellSelectEventArgs<TaskData> args)
    {
        // Capture first selected cell value
        if (Value == null)
        {
            var columns = Gantt.GetColumnsAsync().Result;
            ColumnField = columns[Convert.ToInt32(args.CellIndex)].Field;
            Value = args.Data.GetType().GetProperty(ColumnField).GetValue(args.Data);
        }
    }

    private void CellDeselectedHandler()
    {
        Value = null;
    }

    private async Task KeyUp(KeyboardEventArgs args)
    {
        // Trigger autofill on Alt key release
        if (args.Code == "AltLeft" || args.Code == "AltRight")
        {
            var selectedCells = await Gantt.GetSelectedRowCellIndexesAsync();
            List<TaskData> autofillData = new List<TaskData>();
            
            // Gather all selected rows
            foreach (var cellIndex in selectedCells)
            {
                TaskData record = Gantt.GetCurrentViewRecords()
                    .ElementAt(Convert.ToInt32(cellIndex.Item1));
                if (!autofillData.Contains(record))
                {
                    autofillData.Add(record);
                }
            }
            
            // Apply captured value to all selected cells
            foreach (var record in autofillData)
            {
                record.GetType().GetProperty(ColumnField).SetValue(record, Value);
                await Gantt.UpdateRecordByIDAsync(record);
            }
        }
    }

    public class TaskData
    {
        public int TaskID { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public string Duration { get; set; }
        public int Progress { get; set; }
    }

    protected override void OnInitialized()
    {
        TaskCollection = GetTaskCollection();
    }

    private List<TaskData> GetTaskCollection()
    {
        return new List<TaskData>()
        {
            new TaskData() { TaskID = 1, TaskName = "Task 1", 
                             StartDate = new DateTime(2022, 04, 05), 
                             Duration = "3", Progress = 30 },
            new TaskData() { TaskID = 2, TaskName = "Task 2", 
                             StartDate = new DateTime(2022, 04, 08), 
                             Duration = "4", Progress = 50 },
            new TaskData() { TaskID = 3, TaskName = "Task 3", 
                             StartDate = new DateTime(2022, 04, 12), 
                             Duration = "2", Progress = 70 }
        };
    }
}
```

### Autofill Workflow

1. **Select first cell** — Click cell with value to autofill
2. **Drag to select range** — Hold Alt and drag to select target cells
3. **Release Alt** — Autofill triggers, copying first value to all selected cells

---

## Clipboard Properties

### CopyHierarchyMode

**Type:** `CopyHierarchyType`  
**Default:** `Parent`

**Description:** Controls hierarchical data inclusion when copying selected records.

```razor
<SfGantt DataSource="@TaskCollection" CopyHierarchyMode="CopyHierarchyType.Both">
</SfGantt>
```

**Available values:**
- `Parent` — Include parent records
- `Child` — Include child records
- `Both` — Include both parents and children
- `None` — Selected records only

### EnableHtmlSanitizer

**Type:** `bool`  
**Default:** `true`

**Description:** Sanitizes HTML content in cells before rendering to prevent XSS attacks. When enabled, HTML tags in copied/pasted data are encoded.

```razor
<SfGantt DataSource="@TaskCollection" EnableHtmlSanitizer="true">
</SfGantt>
```

**Security considerations:**
- **Enabled** — HTML tags encoded: `<script>` → `&lt;script&gt;`
- **Disabled** — Raw HTML rendered (potential security risk)

**When to disable:**
- Trusted data sources only
- Custom HTML rendering required
- Rich text formatting needed

---

## Keyboard Shortcuts

### Built-In Shortcuts

| Shortcut | Action | Context |
|----------|--------|---------|
| **Ctrl + C** | Copy selected rows/cells | Row or cell selection active |
| **Ctrl + Shift + H** | Copy with header | Row or cell selection active |
| **Ctrl + V** | Paste (custom implementation) | Target row/cell selected |
| **Alt + Drag** | Autofill selection | Drag selection enabled |
| **Arrow Keys** | Navigate cells/rows | Grid focused |
| **Tab / Shift + Tab** | Navigate interactive elements | Focus management |
| **Enter / F2** | Edit cell/row | Edit mode enabled |
| **Escape** | Cancel edit/close dialog | Edit or dialog active |

### Custom Keyboard Bindings

Use `@onkeyup` and `@onkeydown` events for custom shortcuts:

```razor
<SfGantt @onkeydown="HandleKeyDown" @onkeyup="HandleKeyUp">
</SfGantt>

@code {
    private async Task HandleKeyDown(KeyboardEventArgs args)
    {
        if (args.CtrlKey && args.Code == "KeyD")
        {
            // Custom: Ctrl + D for duplicate
            await DuplicateSelectedRow();
        }
    }
}
```

---

## Accessibility Guidelines

### Keyboard Navigation

The Gantt Chart must support complete keyboard navigation for accessibility compliance (WCAG 2.1 Level AA).

**Essential keyboard workflows:**
- **Tab navigation** — Move between toolbar, grid, chart, and dialogs
- **Arrow keys** — Navigate rows and cells
- **Enter** — Activate selected item or enter edit mode
- **Escape** — Cancel action or close dialog
- **Space** — Toggle selection (checkboxes, dropdowns)

```razor
<SfGantt DataSource="@TaskCollection" AllowSelection="true">
    <GanttSelectionSettings Mode="SelectionMode.Row" Type="SelectionType.Single">
    </GanttSelectionSettings>
</SfGantt>
```

### Focus Management

**Best practices:**
- Visible focus indicators on all interactive elements
- Logical focus order (left-to-right, top-to-bottom)
- Focus trapped in modal dialogs until dismissed
- Focus returns to trigger element after dialog closes

```css
/* Ensure visible focus outlines */
.e-gantt .e-row:focus,
.e-gantt .e-cell:focus {
    outline: 2px solid #0078d4;
    outline-offset: -2px;
}
```

### Screen Reader Support

**ARIA attributes automatically applied:**
- `role="grid"` — Grid container
- `role="row"` — Row elements
- `role="gridcell"` — Cell elements
- `aria-selected` — Selection state
- `aria-expanded` — Expansion state (parent tasks)

**Custom template accessibility:**

```razor
<GanttColumn Field="TaskName" HeaderText="Task Name">
    <Template>
        @{
            var task = (context as TaskData);
            <div role="gridcell" aria-label="@($"Task: {task.TaskName}")">
                @task.TaskName
            </div>
        }
    </Template>
</GanttColumn>
```

---

## ARIA and Screen Readers

### Built-In ARIA Support

Syncfusion Gantt Chart includes baseline ARIA support for screen readers:

**Automatic ARIA attributes:**
- Grid structure (`role="grid"`, `role="row"`, `role="gridcell"`)
- Selection state (`aria-selected="true|false"`)
- Expansion state (`aria-expanded="true|false"`)
- Sort state (`aria-sort="ascending|descending|none"`)
- Labels (`aria-label`, `aria-labelledby`)

### Custom Template Considerations

When using custom templates, preserve accessibility:

**❌ Poor accessibility:**
```razor
<Template>
    <i class="e-icons e-edit"></i>
</Template>
```

**✅ Good accessibility:**
```razor
<Template>
    <button type="button" aria-label="Edit task" class="e-btn e-icon-btn">
        <i class="e-icons e-edit"></i>
    </button>
</Template>
```

### High Contrast Mode

Support users with visual impairments:

```razor
<SfGantt DataSource="@TaskCollection" CssClass="e-gantt-highcontrast">
</SfGantt>
```

```css
/* High contrast styles */
.e-gantt-highcontrast .e-row {
    border: 1px solid #000;
}

.e-gantt-highcontrast .e-row:focus {
    outline: 3px solid #fff;
    outline-offset: 2px;
}
```

---

## Best Practices

### Clipboard Best Practices

1. **Enable appropriate selection mode**
   - Use `Row` for full-record copying
   - Use `Cell` for granular data copying
   - Use `Multiple` for bulk operations

2. **Validate copied data**
   - Use `BeforeCopy` event to inspect/transform data
   - Block sensitive columns from clipboard
   - Log copy operations for auditing

```razor
<GanttEvents BeforeCopy="ValidateCopy" TValue="TaskData"></GanttEvents>

@code {
    private void ValidateCopy(BeforeCopyEventArgs args)
    {
        if (IsSensitiveDataSelected())
        {
            args.Cancel = true;
            ShowNotification("Cannot copy sensitive data");
        }
    }
}
```

3. **Provide copy feedback**
   - Show toast notification after copy
   - Indicate copied row count
   - Display copy mode (with/without header)

### Accessibility Best Practices

1. **Keyboard support**
   - Test all workflows with keyboard only
   - Provide keyboard alternatives for mouse actions
   - Document keyboard shortcuts in help text

2. **Visual clarity**
   - Maintain 4.5:1 text contrast ratio
   - Use color + text/icons (not color alone)
   - Show visible focus indicators

3. **Template accessibility**
   - Include meaningful text for screen readers
   - Add `aria-label` to icon-only buttons
   - Preserve semantic HTML structure

4. **Testing checklist**
   - ✅ Navigate entire Gantt with Tab key
   - ✅ Select/edit tasks with keyboard
   - ✅ Test with screen reader (NVDA, JAWS, Narrator)
   - ✅ Verify high contrast mode support
   - ✅ Test with 200% browser zoom

### Performance Considerations

**Large datasets with clipboard:**
- Limit copied records (e.g., max 1000 rows)
- Use `CopyHierarchyMode.None` for flat data
- Consider pagination for very large datasets

```razor
<GanttEvents BeforeCopy="LimitCopySize" TValue="TaskData"></GanttEvents>

@code {
    private void LimitCopySize(BeforeCopyEventArgs args)
    {
        var selectedCount = await Gantt.GetSelectedRecordsAsync();
        if (selectedCount.Count() > 1000)
        {
            args.Cancel = true;
            ShowNotification("Cannot copy more than 1000 records");
        }
    }
}
```

---

## Summary

The Blazor Gantt Chart provides comprehensive clipboard and accessibility features:

- **Clipboard operations** — Programmatic and keyboard-driven copy/paste
- **Hierarchy modes** — Parent, Child, Both, None for context-aware copying
- **Autofill** — Rapid cell value duplication with drag selection
- **Paste customization** — Row and cell-level paste implementations
- **Keyboard support** — Complete workflows accessible via keyboard
- **ARIA compliance** — Screen reader friendly with proper semantic markup
- **Security** — HTML sanitization prevents XSS attacks

