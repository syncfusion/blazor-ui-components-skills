# Taskbar Editing

## Table of Contents
- [When to Read](#when-to-read)
- [Enable Taskbar Editing](#enable-taskbar-editing)
- [Drag to Move](#drag-to-move)
- [Resize to Change Duration](#resize-to-change-duration)
- [Progress Handle](#progress-handle)
- [Connector Line Drawing](#connector-line-drawing)
- [TaskbarEditing Event (Before)](#taskbarediting-event-before)
- [TaskbarEdited Event (After)](#taskbaredited-event-after)
- [OnActionBegin Cancel Pattern](#onactionbegin-cancel-pattern)
- [Restrict Editing by Condition](#restrict-editing-by-condition)
- [Complete Example](#complete-example)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## When to Read

Use this reference when you need to:
- Enable drag, resize, and progress editing on taskbars
- Handle `TaskbarEditing` / `TaskbarEdited` events with full argument details
- Cancel taskbar edits based on conditions (e.g. locked tasks)
- Understand connector-line drag behavior for dependency creation
- Prevent taskbar editing for specific tasks

---

## Enable Taskbar Editing

Add `AllowTaskbarEditing="true"` to `GanttEditSettings`:

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID" />
    <GanttEditSettings AllowTaskbarEditing="true" />
</SfGantt>
```

This enables three taskbar interactions:
1. **Drag** — move the entire task bar to a new date
2. **Resize** — drag left/right edge to change start/end date
3. **Progress handle** — drag the progress dot to update `Progress`

---

## Drag to Move

Click and drag a taskbar horizontally to change the task start and end dates while preserving duration.

- The `StartDate` and `EndDate` update on drop
- Dependent tasks may auto-adjust if `EnablePredecessorValidation="true"`

---

## Resize to Change Duration

Drag either the left or right edge of a taskbar:

- **Right edge drag** — changes `EndDate` / `Duration`
- **Left edge drag** — changes `StartDate`, preserves end date

---

## Progress Handle

A circular handle appears at the progress boundary within the taskbar:

- Drag right to increase `Progress` (0–100)
- Drag left to decrease `Progress`
- Fires `TaskbarEditing` and `TaskbarEdited` with `TaskbarEditAction.ProgressResizing`

---

## Connector Line Drawing

When `AllowTaskbarEditing="true"`, connector hotspots appear at each taskbar end:

- Hover over a taskbar to reveal left/right connector circles
- Drag from one taskbar's endpoint to another to create a dependency
- This fires `TaskbarEditing` with action `DrawConnectorLine`

To enable dependency editing via connector drag, also map `Dependency`:

```razor
<GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                 Duration="Duration" Dependency="Predecessor" ParentID="ParentID" />
```

---

## TaskbarEditing Event (Before)

Fires **before** a taskbar edit action completes. Can cancel the operation.

```razor
<GanttEvents TValue="TaskData" TaskbarEditing="TaskbarEditingHandler" />

@code {
    public void TaskbarEditingHandler(TaskbarEditingEventArgs<TaskData> args)
    {
        // args.Data — the task being edited
        // args.EditingFields — updated field values (StartDate, EndDate, Duration, Progress)
        // args.Action — the type of taskbar action
        // args.Cancel — set true to prevent the edit

        Console.WriteLine($"Editing: {args.Data.TaskName}, Action: {args.Action}");

        // Cancel editing for tasks with TaskID = 1
        if (args.Data.TaskID == 1)
        {
            args.Cancel = true;
        }
    }
}
```

### TaskbarEditingEventArgs Properties

| Property | Type | Description |
|----------|------|-------------|
| `Data` | `TValue` | The task record being edited |
| `EditingFields` | `GanttEditingFields` | New values for StartDate, EndDate, Duration, Progress |
| `Action` | `TaskbarEditAction` | The current edit operation type |
| `Cancel` | `bool` | Set `true` to cancel the operation |
| `RoundOff` | `bool` | Whether dates are rounded to nearest unit |

### TaskbarEditAction Enum

| Value | Description |
|-------|-------------|
| `ChildDrag` | Dragging a child task |
| `ParentDrag` | Dragging a parent task |
| `LeftResizing` | Resizing the left edge |
| `RightResizing` | Resizing the right edge |
| `ProgressResizing` | Dragging the progress handle |
| `DrawConnectorLine` | Drawing a connector between tasks |
| `ConnectorPointLeftDrag` | Dragging from the left connector point |
| `ConnectorPointRightDrag` | Dragging from the right connector point |

---

## TaskbarEdited Event (After)

Fires **after** a taskbar edit completes successfully:

```razor
<GanttEvents TValue="TaskData" TaskbarEdited="TaskbarEditedHandler" />

@code {
    public void TaskbarEditedHandler(TaskbarEditedEventArgs<TaskData> args)
    {
        // args.Data — the updated task record with new dates/progress
        // args.EditingFields — the field values that changed
        // args.Action — what was performed

        Console.WriteLine($"Edited: {args.Data.TaskName}");
        Console.WriteLine($"New StartDate: {args.Data.StartDate:d}");
        Console.WriteLine($"New Progress: {args.Data.Progress}");

        // Persist to server
        _ = SaveToServer(args.Data);
    }

    private async Task SaveToServer(TaskData task)
    {
        // await httpClient.PostAsJsonAsync("api/tasks/update", task);
    }
}
```

### TaskbarEditedEventArgs Properties

| Property | Type | Description |
|----------|------|-------------|
| `Data` | `TValue` | The updated task record |
| `EditingFields` | `GanttEditingFields` | New values for changed fields |
| `Action` | `TaskbarEditAction` | The edit action that completed |
| `PreviousData` | `TValue` | Task state before the edit |

---

## OnActionBegin Cancel Pattern

`OnActionBegin` fires for all Gantt actions, including taskbar edits:

```razor
<GanttEvents TValue="TaskData" OnActionBegin="ActionBeginHandler" />

@code {
    public void ActionBeginHandler(GanttActionEventArgs<TaskData> args)
    {
        // Intercept taskbar-edit saves
        if (args.RequestType == "taskbarediting")
        {
            // Block editing on locked tasks
            if (args.Data?.IsLocked == true)
            {
                args.Cancel = true;
            }
        }
    }
}
```

---

## Restrict Editing by Condition

### Prevent Edit for Specific Tasks via TaskbarEditing

```razor
<GanttEvents TValue="TaskData" TaskbarEditing="PreventLockedEditing" />

@code {
    public void PreventLockedEditing(TaskbarEditingEventArgs<TaskData> args)
    {
        if (args.Data.IsLocked)
        {
            args.Cancel = true;
        }
    }
}
```

### Disable Taskbar Editing Globally and Enable Programmatically

```razor
<GanttEditSettings AllowTaskbarEditing="false" />

@code {
    // Toggle editing dynamically
    private bool editEnabled = false;

    private void ToggleEditing()
    {
        editEnabled = !editEnabled;
    }
}
```

---

## Complete Example

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt @ref="GanttRef"
         DataSource="@TaskCollection"
         Height="450px"
         Width="1000px">
    <GanttTaskFields Id="TaskID" Name="TaskName"
                     StartDate="StartDate" Duration="Duration"
                     Progress="Progress" ParentID="ParentID"
                     Dependency="Predecessor" />
    <GanttEditSettings AllowTaskbarEditing="true" />
    <GanttEvents TValue="TaskData"
                 TaskbarEditing="OnTaskbarEditing"
                 TaskbarEdited="OnTaskbarEdited" />
</SfGantt>

<div>@_statusMessage</div>

@code {
    private SfGantt<TaskData> GanttRef;
    private string _statusMessage = "Ready.";

    private List<TaskData> TaskCollection = new()
    {
        new TaskData { TaskID = 1, TaskName = "Project", StartDate = new DateTime(2024, 04, 01) },
        new TaskData { TaskID = 2, TaskName = "Design",  Duration = "3", Progress = 60, ParentID = 1, StartDate = new DateTime(2024, 04, 01) },
        new TaskData { TaskID = 3, TaskName = "Build",   Duration = "5", Progress = 30, ParentID = 1, StartDate = new DateTime(2024, 04, 04), Predecessor = "2" }
    };

    public void OnTaskbarEditing(TaskbarEditingEventArgs<TaskData> args)
    {
        _statusMessage = $"Editing '{args.Data.TaskName}' — action: {args.Action}";

        // Prevent editing task with ID 1 (root/summary)
        if (args.Data.TaskID == 1)
        {
            args.Cancel = true;
            _statusMessage = "Cannot edit root task.";
        }
    }

    public void OnTaskbarEdited(TaskbarEditedEventArgs<TaskData> args)
    {
        _statusMessage = $"Saved '{args.Data.TaskName}' — Start: {args.Data.StartDate:d}, Progress: {args.Data.Progress}%";
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
        public bool IsLocked { get; set; }
    }
}
```

---

## Best Practices

- **Use `TaskbarEditing` (before) for validation** — cancel bad edits before they apply
- **Use `TaskbarEdited` (after) for persistence** — commit changes to your data store after a successful edit
- **Map `Dependency` field** if you want connector-line drawing to create/update predecessors
- **Test `ProgressResizing`** separately — progress drag fires the same event pair but with different action enum
- **Enable `EnablePredecessorValidation="true"`** if dependent tasks should auto-shift after a drag

---

## Troubleshooting

**Issue**: Taskbar drag has no effect  
**Solution**: Ensure `AllowTaskbarEditing="true"` is set in `GanttEditSettings`.

**Issue**: `TaskbarEditing` fires but `Cancel = true` doesn't stop the edit  
**Solution**: Cancellation only works in the `TaskbarEditing` (before) event, not `TaskbarEdited` (after).

**Issue**: Connector lines don't appear when hovering taskbars  
**Solution**: Confirm `AllowTaskbarEditing="true"` and that `Dependency` is mapped in `GanttTaskFields`.

**Issue**: `args.Data` is null inside event handler  
**Solution**: Ensure `TValue` generic parameter on `GanttEvents` matches the Gantt's `TValue`.

---
