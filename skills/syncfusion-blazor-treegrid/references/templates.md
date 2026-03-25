# Templates in Syncfusion Blazor TreeGrid

## Table of Contents
- [Critical Rules](#critical-rules)
- [Column Template](#column-template)
- [Header Template](#header-template)
- [Row Template](#row-template)
- [Detail Template](#detail-template)
- [Edit Template](#edit-template)
- [Toolbar Template](#toolbar-template)
- [Empty Record Template](#empty-record-template)

---

## Critical Rules

⚠️ **MANDATORY RULES TO AVOID ERRORS:**

1. **RowTemplate, DetailTemplate, and LoadingIndicatorTemplate MUST be inside `<TreeGridTemplates>` block** — not directly under `<SfTreeGrid>`
2. **Use `<RowTemplateTreeColumn>` wrapper** in the first `<td>` of row templates to preserve expand/collapse icon functionality
3. **Use async methods only:**
   - `await TreeGridRef.RefreshAsync()` ✅ (NOT `Refresh()`)
   - `await TreeGridRef.StartEditAsync()` ✅ (NOT `StartEdit()`)
   - `await TreeGridRef.DeleteRecordAsync()` ✅ (NOT `DeleteRecord()`)
4. **EditType values (NOT "Default"):**
   - `EditType.DefaultEdit` ✅
   - `EditType.DatePickerEdit`, `EditType.NumericEdit`, `EditType.DropDownEdit`
5. **Always cast `context`** to your model type: `var row = context as TaskItem;`
6. **Validate dates** — DateTime(year, month, day) must be valid (e.g., March has 31 days, not 32)
7. **Templates require proper scope** — declare template block inside `<TreeGridTemplates>`

---

## Column Template

Render custom Blazor content inside data cells. Use `context` (cast to your model type):

```razor
<TreeGridColumn Field="Progress" HeaderText="Progress" Width="180">
    <Template>
        @{
            var row = context as TaskItem;
        }
        <div style="display:flex; align-items:center; gap:8px">
            <div style="flex:1; background:#eee; border-radius:4px; height:8px">
                <div style="width:@(row.Progress)%; background:steelblue; height:8px; border-radius:4px;"></div>
            </div>
            <span style="font-size:12px">@row.Progress%</span>
        </div>
    </Template>
</TreeGridColumn>
```

**Nested component in template:**
```razor
<TreeGridColumn Field="Status" HeaderText="Status" Width="140">
    <Template>
        @{
            var row = context as TaskItem;
            var color = row.Status switch
            {
                "Done"        => "green",
                "In Progress" => "orange",
                "Blocked"     => "red",
                _             => "gray"
            };
        }
        <span style="color:@color; font-weight:bold">● @row.Status</span>
    </Template>
</TreeGridColumn>
```

---

## Header Template

Custom content in the column header cell:

```razor
<TreeGridColumn Field="Priority" Width="120">
    <HeaderTemplate>
        <span>
            <i class="e-icons e-flag" style="margin-right:4px"></i>
            Priority
        </span>
    </HeaderTemplate>
</TreeGridColumn>
```

---

## Row Template

Replace the entire row rendering with custom markup. **CRITICAL:** Use `<RowTemplateTreeColumn>` wrapper in the first `<td>` to preserve tree expand/collapse functionality, and place template inside `<TreeGridTemplates>`:

```razor
<SfTreeGrid DataSource="@Employees"
            TValue="Employee"
            IdMapping="EmployeeID"
            ParentIdMapping="ParentId"
            RowHeight="70"
            TreeColumnIndex="0">
    
    <TreeGridColumns>
        <TreeGridColumn Field="Name"       HeaderText="Employee"   Width="260" />
        <TreeGridColumn Field="Department" HeaderText="Department" Width="160" />
        <TreeGridColumn Field="HireDate"   HeaderText="Hire Date"  Width="140" />
    </TreeGridColumns>

    <!-- ✅ CORRECT: Templates inside TreeGridTemplates -->
    <TreeGridTemplates>
        <RowTemplate>
            @{
                var emp = context as Employee;
            }
            <!-- ✅ CRITICAL: Use RowTemplateTreeColumn for tree column -->
            <td style="padding: 8px">
                <RowTemplateTreeColumn>
                    <div style="display:flex; align-items:center; gap:10px">
                        <img src="@emp.ImageUrl" style="width:48px; height:48px; border-radius:50%; object-fit:cover" />
                        <div>
                            <div style="font-weight:bold">@emp.Name</div>
                            <div style="color:gray; font-size:12px">@emp.Title</div>
                        </div>
                    </div>
                </RowTemplateTreeColumn>
            </td>
            <td style="padding:8px">@emp.Department</td>
            <td style="padding:8px">@emp.HireDate.ToString("MMM d, yyyy")</td>
        </RowTemplate>
    </TreeGridTemplates>
</SfTreeGrid>
```

> ⚠️ **Common Errors:**
> - ❌ RowTemplate outside `<TreeGridTemplates>` → context undefined error
> - ❌ Missing `<RowTemplateTreeColumn>` → expand/collapse icon won't work
> - ❌ Wrong `<td>` count → layout breaks
> - ✅ Test editing and selection thoroughly when combining with CRUD features

---

## Detail Template

Expandable panel that appears below a row when the detail icon is clicked. **CRITICAL:** Place inside `<TreeGridTemplates>`:

```razor
<SfTreeGrid DataSource="@TreeData" IdMapping="TaskId" ParentIdMapping="ParentId" TreeColumnIndex="1">
    
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="ID"        Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="220" />
        <TreeGridColumn Field="Priority" HeaderText="Priority"  Width="120" />
    </TreeGridColumns>

    <!-- ✅ CORRECT: DetailTemplate inside TreeGridTemplates -->
    <TreeGridTemplates>
        <DetailTemplate>
            @{
                var task = context as TaskItem;
            }
            <div style="padding-left: 10px; display: inline-block; width: 90%; font-size:13px; font-family:'Segoe UI';">
                <div class="e-description" style="word-wrap: break-word;">
                    <b>@(task?.TaskName)</b> is assigned to @(task?.AssignedTo ?? "Unassigned"), and is currently @(task?.Status?.ToLower() ?? "pending").
                    This task focuses on: <b>@(task?.Description ?? "No description")</b>
                </div>
                <div class="e-description" style="word-wrap: break-word; margin-top:5px;">
                    <b style="margin-right:10px;">Timeline:</b> @(task?.StartDate?.ToString("MMM d, yyyy") ?? "Not started") to @(task?.DueDate?.ToString("MMM d, yyyy") ?? "No due date")
                </div>
                <div class="e-description" style="word-wrap: break-word; margin-top:5px;">
                    <b style="margin-right:10px;">Progress:</b> @(task?.Progress)% | <b style="margin-left:10px; margin-right:10px;">Priority:</b> @(task?.Priority)
                </div>
            </div>
        </DetailTemplate>
    </TreeGridTemplates>
</SfTreeGrid>
```

> ⚠️ **Common Errors:**
> - ❌ DetailTemplate outside `<TreeGridTemplates>` → context undefined
> - ❌ Invalid DateTime values like `new DateTime(2026, 3, 32)` → ArgumentOutOfRangeException
> - ✅ Use narrative text format for better readability
> - ✅ Always validate date ranges (Jan-31, Feb-28/29, Mar-31, etc.)

---

## Edit Template

Provide a fully custom edit control for a specific column. Use `EditType.DefaultEdit` (**NOT** `EditType.Default`):

```razor
<!-- ✅ CORRECT EditType value -->
<TreeGridColumn Field="AssignedTo" HeaderText="Assigned To" Width="180" EditType="EditType.DefaultEdit">
    <EditTemplate>
        @{
            var row = context as TaskItem;
        }
        <select class="e-field" @bind="row.AssignedTo" style="width:100%; padding:8px; border:1px solid #ccc; border-radius:4px">
            <option value="">-- Select Team Member --</option>
            @foreach (var member in TeamMembers)
            {
                <option value="@member">@member</option>
            }
        </select>
    </EditTemplate>
    <Template>
        @{
            var row = context as TaskItem;
        }
        <span>@(row?.AssignedTo ?? "Unassigned")</span>
    </Template>
</TreeGridColumn>

<!-- Alternative: Syncfusion AutoComplete component -->
<TreeGridColumn Field="Priority" HeaderText="Priority" Width="140" EditType="EditType.DefaultEdit">
    <EditTemplate>
        @{
            var row = context as TaskItem;
        }
        <SfAutoComplete TItem="string" TValue="string"
                        DataSource="@PriorityOptions"
                        @bind-Value="row.Priority"
                        Placeholder="Select priority...">
        </SfAutoComplete>
    </EditTemplate>
</TreeGridColumn>

@code {
    private List<string> TeamMembers = new() { "Alice", "Bob", "Charlie", "Diana", "Eve" };
    private List<string> PriorityOptions = new() { "Low", "Medium", "High", "Critical" };
}
```

> ⚠️ **Common Errors:**
> - ❌ `EditType="EditType.Default"` → Does NOT exist
> - ✅ `EditType="EditType.DefaultEdit"` ✅ Correct value
> - ✅ Include both `<EditTemplate>` AND `<Template>` for complete column definition

---

## Toolbar Template

Customize toolbar actions with proper async methods. **CRITICAL:** Use `RefreshAsync()` and `StartEditAsync()`:

```razor
<SfTreeGrid @ref="TreeGridRef"
            DataSource="@TreeData"
            TValue="TaskItem"
            Toolbar="@ToolbarItems">
    
    <TreeGridEvents TValue="TaskItem" OnToolbarClick="OnToolbarClick" />
    
    <TreeGridColumns>
        <!-- column definitions -->
    </TreeGridColumns>
</SfTreeGrid>

@code {
    private SfTreeGrid<TaskItem> TreeGridRef;

    private List<Syncfusion.Blazor.Navigations.ItemModel> ToolbarItems = new()
    {
        new() { Id = "grid_add", TooltipText = "Add New", Text = "➕ Add" },
        new() { Id = "grid_edit", TooltipText = "Edit Selected", Text = "✏️ Edit" },
        new() { Id = "grid_delete", TooltipText = "Delete Selected", Text = "🗑️ Delete" },
        new() { Id = "grid_refresh", TooltipText = "Refresh Data", Text = "🔄 Refresh" }
    };

    // ✅ CORRECT: Use async methods
    private async Task OnToolbarClick(Syncfusion.Blazor.Navigations.ClickEventArgs args)
    {
        if (args.Item.Id == "grid_add")
        {
            await TreeGridRef.StartEditAsync();
        }
        else if (args.Item.Id == "grid_edit")
        {
            var selectedRecord = await TreeGridRef.GetSelectedRecordsAsync();
            if (selectedRecord.Count > 0)
            {
                await TreeGridRef.StartEditAsync();
            }
        }
        else if (args.Item.Id == "grid_delete")
        {
            var selectedRecord = await TreeGridRef.GetSelectedRecordsAsync();
            if (selectedRecord.Count > 0)
            {
                await TreeGridRef.DeleteRecordAsync();
            }
        }
        else if (args.Item.Id == "grid_refresh")
        {
            await TreeGridRef.RefreshAsync();
        }
    }
}
```

> ⚠️ **Common Errors:**
> - ❌ `await TreeGridRef.StartEdit()` → Missing Async suffix
> - ❌ `await TreeGridRef.Refresh()` → Wrong method name
> - ✅ `await TreeGridRef.StartEditAsync()` ✅
> - ✅ `await TreeGridRef.RefreshAsync()` ✅
> - ✅ `await TreeGridRef.DeleteRecordAsync()` ✅

---

## Empty Record Template

Show custom content when there are no rows to display:

```razor
<SfTreeGrid ...>
    <TreeGridTemplates>
        <EmptyRecordTemplate>
            <div style="text-align:center; padding:40px; color:gray">
                <span class="e-icons e-empty" style="font-size:48px"></span>
                <p>No tasks found. <a href="#" @onclick="AddFirstTask">Add your first task</a></p>
            </div>
        </EmptyRecordTemplate>
    </TreeGridTemplates>
    ...
</SfTreeGrid>
```

## Complete Working Example

Here's a full component combining all template types correctly:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.TreeGrid
@using Syncfusion.Blazor.Grids

<SfTreeGrid @ref="TreeGridRef"
            DataSource="@TreeData"
            TValue="TaskItem"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            TreeColumnIndex="1"
            AllowSorting="true"
            AllowFiltering="true"
            AllowPaging="true"
            Toolbar="@ToolbarItems">

    <TreeGridPageSettings PageSize="5" />
    <TreeGridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true" Mode="EditMode.Dialog" />
    <TreeGridEvents TValue="TaskItem" OnToolbarClick="OnToolbarClick" RowDataBound="OnRowDataBound" OnActionComplete="OnActionComplete" />

    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="ID" Width="80" IsPrimaryKey="true" Visible="false" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="220" />
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="180">
            <Template>
                @{
                    var row = context as TaskItem;
                }
                <div style="display:flex; align-items:center; gap:8px">
                    <div style="flex:1; background:#eee; border-radius:4px; height:8px">
                        <div style="width:@(row?.Progress)%; background:steelblue; height:8px; border-radius:4px;"></div>
                    </div>
                    <span style="font-size:12px">@(row?.Progress)%</span>
                </div>
            </Template>
        </TreeGridColumn>
        <TreeGridColumn Field="Status" HeaderText="Status" Width="140">
            <Template>
                @{
                    var row = context as TaskItem;
                    var color = row?.Status switch
                    {
                        "Done" => "green",
                        "In Progress" => "orange",
                        "Blocked" => "red",
                        _ => "gray"
                    };
                }
                <span style="color:@color; font-weight:bold">● @row?.Status</span>
            </Template>
        </TreeGridColumn>
        <TreeGridColumn Field="AssignedTo" HeaderText="Assigned To" Width="160" EditType="EditType.DefaultEdit">
            <EditTemplate>
                @{
                    var row = context as TaskItem;
                }
                <select class="e-field" @bind="row.AssignedTo" style="width:100%; padding:8px">
                    <option value="">-- Select --</option>
                    @foreach (var member in TeamMembers)
                    {
                        <option value="@member">@member</option>
                    }
                </select>
            </EditTemplate>
            <Template>
                @{
                    var row = context as TaskItem;
                }
                <span>@(row?.AssignedTo ?? "Unassigned")</span>
            </Template>
        </TreeGridColumn>
    </TreeGridColumns>

    <!-- ✅ ALL TEMPLATES INSIDE TreeGridTemplates -->
    <TreeGridTemplates>
        <RowTemplate>
            @{
                var task = context as TaskItem;
            }
            <td style="padding:8px">
                <RowTemplateTreeColumn>
                    <div style="display:flex; align-items:center; gap:10px">
                        <div style="width:48px; height:48px; background:#e0e0e0; border-radius:50%; display:flex; align-items:center; justify-content:center; font-weight:bold">
                            @task?.TaskId
                        </div>
                        <div>
                            <div style="font-weight:bold">@task?.TaskName</div>
                            <div style="color:gray; font-size:12px">@task?.Priority</div>
                        </div>
                    </div>
                </RowTemplateTreeColumn>
            </td>
            <td style="padding:8px; text-align:center">@task?.Progress%</td>
            <td style="padding:8px; text-align:center">@task?.Status</td>
            <td style="padding:8px">@(task?.AssignedTo ?? "—")</td>
        </RowTemplate>

        <DetailTemplate>
            @{
                var task = context as TaskItem;
            }
            <div style="padding-left:10px; width:90%">
                <b>@task?.TaskName</b> is assigned to @(task?.AssignedTo ?? "Unassigned").
                <br /><b>Description:</b> @(task?.Description ?? "N/A")
            </div>
        </DetailTemplate>
    </TreeGridTemplates>

</SfTreeGrid>

@code {
    private SfTreeGrid<TaskItem> TreeGridRef;
    private List<TaskItem> TreeData = new();

    private List<Syncfusion.Blazor.Navigations.ItemModel> ToolbarItems = new()
    {
        new() { Id = "grid_add", Text = "Add" },
        new() { Id = "grid_edit", Text = "Edit" },
        new() { Id = "grid_delete", Text = "Delete" },
        new() { Id = "grid_refresh", Text = "Refresh" }
    };

    private List<string> TeamMembers = new() { "Alice", "Bob", "Charlie" };

    public class TaskItem
    {
        public int TaskId { get; set; }
        public int? ParentId { get; set; }
        public string TaskName { get; set; }
        public int Progress { get; set; }
        public string Status { get; set; }
        public string AssignedTo { get; set; }
        public string Priority { get; set; }
        public string Description { get; set; }
    }

    protected override void OnInitialized()
    {
        // Valid dates: Month 1-12, Day must be valid for the month
        TreeData = new()
        {
            new TaskItem { TaskId = 1, ParentId = null, TaskName = "Project A", Progress = 65, Status = "In Progress", Priority = "High", Description = "Main project" },
            new TaskItem { TaskId = 2, ParentId = 1, TaskName = "Task 1", Progress = 100, Status = "Done", Priority = "High", Description = "Subtask 1" },
            new TaskItem { TaskId = 3, ParentId = 1, TaskName = "Task 2", Progress = 45, Status = "In Progress", Priority = "Medium", Description = "Subtask 2" }
        };
    }

    private async Task OnToolbarClick(Syncfusion.Blazor.Navigations.ClickEventArgs args)
    {
        if (args.Item.Id == "grid_refresh")
            await TreeGridRef.RefreshAsync();
        else if (args.Item.Id == "grid_add")
            await TreeGridRef.StartEditAsync();
    }

    private void OnRowDataBound(Syncfusion.Blazor.Grids.RowDataBoundEventArgs<TaskItem> args)
    {
       //custom logic
    }

    private async Task OnActionComplete(Syncfusion.Blazor.Grids.ActionEventArgs<TaskItem> args)
    {
        if (args.RequestType == Syncfusion.Blazor.Grids.Action.Save || args.RequestType == Syncfusion.Blazor.Grids.Action.Delete)
            await TreeGridRef.RefreshAsync();
    }
}
```

---

## Error Prevention Checklist

Before implementing templates, verify:

- [ ] All templates are inside `<TreeGridTemplates>` block
- [ ] Row template uses `<RowTemplateTreeColumn>` wrapper in first `<td>`
- [ ] Edit template uses `EditType.DefaultEdit` (not `Default`)
- [ ] All async methods use `Async` suffix: `RefreshAsync()`, `StartEditAsync()`, `DeleteRecordAsync()`
- [ ] DateTime values are valid (March has 31 days, not 32)
- [ ] Context is always cast to model type: `var item = context as ModelType;`
- [ ] `<td>` count matches column count in row templates
- [ ] Both `<Template>` and `<EditTemplate>` are defined for editable columns

---
