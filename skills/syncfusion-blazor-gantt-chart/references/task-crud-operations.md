# Task CRUD Operations

## Table of Contents
- [When to Read](#when-to-read)
- [Enable Editing](#enable-editing)
- [Edit Modes](#edit-modes)
- [Adding Tasks](#adding-tasks)
- [Editing Tasks](#editing-tasks)
- [Deleting Tasks](#deleting-tasks)
- [Programmatic CRUD API](#programmatic-crud-api)
- [Dialog Customization](#dialog-customization)
- [Remote CRUD Configuration](#remote-crud-configuration)
- [CRUD Events](#crud-events)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## When to Read

Use this reference when you need to:
- Enable add, edit, and delete operations on Gantt tasks
- Configure edit modes (Auto inline, Dialog, Taskbar-only)
- Open add/edit dialogs programmatically
- Use CRUD APIs: `AddRecordAsync`, `UpdateRecordByIDAsync`, `DeleteRecordAsync`
- Customize add/edit dialog fields and tabs
- Connect to remote CRUD endpoints (InsertUrl, UpdateUrl, RemoveUrl)
- Handle CRUD events for validation and side effects

---

## Enable Editing

Add `GanttEditSettings` as a child component and configure which operations are allowed:

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt DataSource="@TaskCollection"
         Height="450px"
         Width="900px"
         Toolbar="@(new List<string>() { "Add", "Edit", "Delete", "Cancel", "Update" })">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     EndDate="EndDate" Duration="Duration" Progress="Progress"
                     ParentID="ParentID" Dependency="Predecessor">
    </GanttTaskFields>
    <GanttEditSettings AllowAdding="true"
                       AllowEditing="true"
                       AllowDeleting="true"
                       AllowTaskbarEditing="true"
                       ShowDeleteConfirmDialog="true">
    </GanttEditSettings>
</SfGantt>
```

### GanttEditSettings Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `AllowAdding` | `bool` | `false` | Enable adding new tasks |
| `AllowEditing` | `bool` | `false` | Enable editing existing tasks |
| `AllowDeleting` | `bool` | `false` | Enable deleting tasks |
| `AllowTaskbarEditing` | `bool` | `false` | Enable drag/resize on taskbars |
| `Mode` | `EditMode` | `Auto` | Edit mode: Auto, Dialog, or Taskbar |
| `ShowDeleteConfirmDialog` | `bool` | `false` | Show confirmation before deleting |
| `NewRowPosition` | `RowPosition` | `Below` | Where new rows are inserted |

---

## Edit Modes

### Auto Mode (Inline)

Double-click a cell to edit it inline. Press Enter to save, Escape to cancel.

```razor
<GanttEditSettings AllowEditing="true" Mode="EditMode.Auto" />
```

### Dialog Mode

Clicking Edit opens a modal dialog with all task fields organized in tabs.

```razor
<GanttEditSettings AllowEditing="true" Mode="EditMode.Dialog" />
```

### Taskbar Mode

Editing is restricted to taskbar drag/resize operations. No cell or dialog editing.

```razor
<GanttEditSettings AllowTaskbarEditing="true" Mode="EditMode.Taskbar" />
```

---

## Adding Tasks

### Via Toolbar

Add the `"Add"` string to `Toolbar`. Clicking it opens the add dialog (Dialog mode) or creates a new inline row (Auto mode).

```razor
<SfGantt DataSource="@TaskCollection"
         Toolbar="@(new List<string>() { "Add" })">
    <GanttEditSettings AllowAdding="true" />
</SfGantt>
```

### NewRowPosition

Control where new rows are inserted relative to the currently selected row:

```razor
<GanttEditSettings AllowAdding="true"
                   NewRowPosition="RowPosition.Below" />
```

| Value | Description |
|-------|-------------|
| `RowPosition.Above` | Insert above selected row |
| `RowPosition.Below` | Insert below selected row (default) |
| `RowPosition.Child` | Insert as child of selected row |

### Via Context Menu

```razor
<SfGantt DataSource="@TaskCollection" EnableContextMenu="true">
    <GanttEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true" />
</SfGantt>
```

The built-in context menu includes `"Add Child"` and `"Add Next"` options.

---

## Editing Tasks

### Inline (Auto Mode)

Double-click any editable cell to enter inline edit mode.

### Via Dialog

With `Mode="EditMode.Dialog"`, double-clicking a row opens the full edit dialog.

### Taskbar Editing

Set `AllowTaskbarEditing="true"` to allow:
- **Drag** — move the task to a different date
- **Resize left/right** — change start or end date
- **Progress handle** — drag to update progress percentage

---

## Deleting Tasks

### Via Toolbar

```razor
<SfGantt DataSource="@TaskCollection"
         Toolbar="@(new List<string>() { "Delete" })">
    <GanttEditSettings AllowDeleting="true" ShowDeleteConfirmDialog="true" />
</SfGantt>
```

### Delete Confirmation Dialog

`ShowDeleteConfirmDialog="true"` shows a confirmation prompt before deletion.

### Child Task Deletion Behavior

When a parent task is deleted, all its child tasks are deleted as well (cascade delete).

---

## Programmatic CRUD API

### Add Record

```csharp
// Add a new task below row at index 29
TaskData newTask = new TaskData()
{
    TaskID = 30,
    TaskName = "New Development Task",
    StartDate = new DateTime(2024, 05, 01),
    Duration = "3",
    Progress = 0
};
await GanttRef.AddRecordAsync(newTask, 29, RowPosition.Below);
```

**Signature:**
```csharp
AddRecordAsync(TValue data, int? index = null, RowPosition? position = null, object? resourceRecords = null)
```

### Update Record by ID

```csharp
// Update task data — the object must contain the task's ID
var updatedData = new TaskData()
{
    TaskID = 5,
    TaskName = "Updated Task Name",
    StartDate = new DateTime(2024, 05, 10),
    Duration = "4",
    Progress = 60
};
await GanttRef.UpdateRecordByIDAsync(updatedData);
```

### Delete Record

```csharp
// Delete by integer ID
await GanttRef.DeleteRecordAsync(5);

// Delete by string ID
await GanttRef.DeleteRecordAsync("task-5");

// Delete by GUID
await GanttRef.DeleteRecordAsync(taskGuid);
```

### Open Add Dialog Programmatically

```csharp
await GanttRef.OpenAddDialogAsync();
```

### Open Edit Dialog Programmatically

```csharp
// Open edit dialog for task with ID = 2
await GanttRef.OpenEditDialogAsync(2);
```

### Cancel Edit

```csharp
GanttRef.CancelEdit();
```

### Complete Example

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Buttons

<div style="margin-bottom:12px; display:flex; gap:8px;">
    <SfButton @onclick="AddTask">Add Task</SfButton>
    <SfButton @onclick="EditTask">Edit Task 2</SfButton>
    <SfButton @onclick="DeleteTask">Delete Task 3</SfButton>
</div>

<SfGantt @ref="GanttRef" DataSource="@TaskCollection" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID" />
    <GanttEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true" />
</SfGantt>

@code {
    private SfGantt<TaskData> GanttRef;
    private List<TaskData> TaskCollection { get; set; }

    protected override void OnInitialized()
    {
        TaskCollection = new List<TaskData>()
        {
            new TaskData() { TaskID = 1, TaskName = "Project", StartDate = new DateTime(2024, 04, 01) },
            new TaskData() { TaskID = 2, TaskName = "Design",  Duration = "3", ParentID = 1, StartDate = new DateTime(2024, 04, 01) },
            new TaskData() { TaskID = 3, TaskName = "Build",   Duration = "5", ParentID = 1, StartDate = new DateTime(2024, 04, 04), Predecessor = "2" }
        };
    }

    private async Task AddTask()
    {
        var newTask = new TaskData()
        {
            TaskID = 10,
            TaskName = "New Task",
            StartDate = new DateTime(2024, 04, 10),
            Duration = "2",
            ParentID = 1
        };
        await GanttRef.AddRecordAsync(newTask);
    }

    private async Task EditTask()
    {
        await GanttRef.OpenEditDialogAsync(2);
    }

    private async Task DeleteTask()
    {
        await GanttRef.DeleteRecordAsync(3);
    }

    public class TaskData
    {
        public int TaskID { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public string Duration { get; set; }
        public int Progress { get; set; }
        public int? ParentID { get; set; }
        public string Predecessor { get; set; }
    }
}
```

---

## Dialog Customization

Customize which tabs and fields appear in the add/edit dialog:

```razor
<SfGantt DataSource="@TaskCollection">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" Dependency="Predecessor" ParentID="ParentID" />
    <GanttEditSettings AllowEditing="true" Mode="EditMode.Dialog" />

    <!-- Customize Edit dialog tabs -->
    <GanttEditDialogFields>
        <GanttEditDialogField Type="DialogFieldType.General"    HeaderText="General" />
        <GanttEditDialogField Type="DialogFieldType.Dependency" HeaderText="Dependencies" />
        <GanttEditDialogField Type="DialogFieldType.Notes"      HeaderText="Notes" />
    </GanttEditDialogFields>

    <!-- Customize Add dialog tabs -->
    <GanttAddDialogFields>
        <GanttAddDialogField Type="DialogFieldType.General" HeaderText="Task Info" />
    </GanttAddDialogFields>
</SfGantt>
```

**Available `DialogFieldType` values:**
- `General` — Core fields (Name, StartDate, Duration, etc.)
- `Dependency` — Predecessor/dependency configuration
- `Resources` — Resource assignment (requires resource setup)
- `Notes` — Free-text notes field
- `Custom` — Custom columns defined in GanttColumns

---

## Remote CRUD Configuration

For server-persisted data, configure CRUD endpoints using `SfDataManager`:

```razor
<SfGantt TValue="TaskData">
    <SfDataManager Url="api/gantt"
                   InsertUrl="api/gantt/insert"
                   UpdateUrl="api/gantt/update"
                   RemoveUrl="api/gantt/delete"
                   BatchUrl="api/gantt/batch"
                   Adaptor="Adaptors.UrlAdaptor">
    </SfDataManager>
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" ParentID="ParentID" />
    <GanttEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true" />
</SfGantt>
```

Server controller pattern:
```csharp
[HttpPost("insert")]
public IActionResult Insert([FromBody] CRUDModel<TaskData> value)
{
    _service.Insert(value.Value);
    return Json(value.Value);
}

[HttpPost("update")]
public IActionResult Update([FromBody] CRUDModel<TaskData> value)
{
    _service.Update(value.Value);
    return Json(value.Value);
}

[HttpPost("delete")]
public IActionResult Delete([FromBody] CRUDModel<TaskData> value)
{
    _service.Delete(Convert.ToInt32(value.Key));
    return Json(value);
}
```

---

## CRUD Events

### ActionBegin (Before CRUD)

Triggered before any CRUD action. Can cancel the operation:

```razor
<GanttEvents OnActionBegin="ActionBeginHandler" TValue="TaskData" />

@code {
    public void ActionBeginHandler(GanttActionEventArgs<TaskData> args)
    {
        if (args.RequestType == "save" && string.IsNullOrEmpty(args.NewTaskData?.TaskName))
        {
            args.Cancel = true; // Block save if TaskName is empty
        }

        if (args.RequestType == "delete")
        {
            // Log deletion
            Console.WriteLine($"Deleting: {args.Data?.TaskName}");
        }
    }
}
```

**Common `RequestType` values:**
- `"add"` — Before add row is created
- `"save"` — Before save (add or edit)
- `"delete"` — Before delete
- `"edit"` — Before cell/row enters edit mode
- `"cancel"` — Before edit is cancelled

### ActionComplete (After CRUD)

Triggered after a CRUD action completes:

```razor
<GanttEvents OnActionComplete="ActionCompleteHandler" TValue="TaskData" />

@code {
    public void ActionCompleteHandler(GanttActionEventArgs<TaskData> args)
    {
        if (args.RequestType == "save")
        {
            Console.WriteLine($"Saved: {args.ModifiedTaskData?.TaskName}");
        }
    }
}
```

---

## Best Practices

- **Always set `IsPrimaryKey="true"` on the ID column** when using `GanttColumns` — required for proper row identification
- **Use Dialog mode for complex data models** — easier for users to fill in all fields
- **Use `ShowDeleteConfirmDialog="true"`** for any production application to prevent accidental deletions
- **Validate in `ActionBegin`** — prevent saves with invalid data before the component processes them
- **Return the full updated record from server** — the Gantt uses the server response to update its internal state

---

## Troubleshooting

**Issue**: Add/Edit buttons are disabled  
**Solution**: Check `AllowAdding="true"` / `AllowEditing="true"` in `GanttEditSettings`. Also ensure the Toolbar items `"Add"` / `"Edit"` are in the `Toolbar` list.

**Issue**: Programmatic `AddRecordAsync` doesn't show in grid  
**Solution**: The `TaskID` in the new record must be unique. Also call `await GanttRef.RefreshAsync()` if the data source was mutated externally.

**Issue**: Dialog opens but fields are empty  
**Solution**: Verify `GanttTaskFields` field name strings exactly match data model property names (case-sensitive).

**Issue**: Delete removes the wrong row  
**Solution**: Confirm `IsPrimaryKey="true"` is set on the correct column. The primary key identifies which row to delete.

**Issue**: Server CRUD endpoint not called  
**Solution**: Verify `InsertUrl`, `UpdateUrl`, `RemoveUrl` are correct. Check browser Network tab for the request. Ensure `AllowAdding/Editing/Deleting` is enabled.

---
