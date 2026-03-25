# Task Dependencies and Validation

## Table of Contents
- [When to Read](#when-to-read)
- [Dependency Types](#dependency-types)
- [Field Mapping](#field-mapping)
- [Dependency String Format](#dependency-string-format)
- [Basic Dependency Setup](#basic-dependency-setup)
- [Restrict Dependency Types](#restrict-dependency-types)
- [Predecessor Validation](#predecessor-validation)
- [Programmatic Dependency API](#programmatic-dependency-api)
- [Dependency Dialog Configuration](#dependency-dialog-configuration)
- [Connector Line Styling](#connector-line-styling)
- [Dependency Events](#dependency-events)
- [Auto-Scheduling Interaction](#auto-scheduling-interaction)
- [Best Practices](#best-practices)
- [Common Scenarios](#common-scenarios)
- [Troubleshooting](#troubleshooting)

---

## When to Read

Use this reference when you need to:
- Define task ordering relationships (Finish-to-Start, Start-to-Start, etc.)
- Map a `Dependency`/`Predecessor` field from your data model
- Enable or disable auto-scheduling based on dependencies
- Add/remove/update predecessors programmatically
- Restrict which dependency types are allowed
- Style dependency connector lines

---

## Dependency Types

| Type | Label | Description |
|------|-------|-------------|
| `DependencyType.FS` | Finish-to-Start | Successor starts after predecessor ends (most common) |
| `DependencyType.SS` | Start-to-Start | Successor starts when predecessor starts |
| `DependencyType.FF` | Finish-to-Finish | Successor ends when predecessor ends |
| `DependencyType.SF` | Start-to-Finish | Successor ends when predecessor starts (rare) |

---

## Field Mapping

Map the data model field that stores predecessor strings using `GanttTaskFields.Dependency`:

```razor
<GanttTaskFields Id="TaskID"
                 Name="TaskName"
                 StartDate="StartDate"
                 Duration="Duration"
                 ParentID="ParentID"
                 Dependency="Predecessor">
</GanttTaskFields>
```

The data model field (`Predecessor` in this example) must be of type `string`.

```csharp
public class TaskData
{
    public int TaskID { get; set; }
    public string TaskName { get; set; }
    public DateTime StartDate { get; set; }
    public string Duration { get; set; }
    public int Progress { get; set; }
    public int? ParentID { get; set; }
    public string Predecessor { get; set; }  // Dependency field
}
```

---

## Dependency String Format

Dependencies are encoded as compact strings stored in the `Predecessor` field:

| Format | Meaning |
|--------|---------|
| `"3"` or `"3FS"` | Task 3 must finish before this task starts |
| `"3SS"` | Task 3 must start before this task starts |
| `"3FF"` | Task 3 must finish before this task finishes |
| `"3SF"` | Task 3 must start before this task finishes |
| `"3FS+2"` | Task 3 finish + 2 days lag before this task starts |
| `"3FS-1"` | Task 3 finish − 1 day lead (start 1 day before predecessor ends) |
| `"2,3FS"` | Multiple predecessors: task 2 (FS) and task 3 (FS) |

The offset unit matches the `DurationUnit` configured on `SfGantt`.

```csharp
// Sample data rows showing dependency strings
new TaskData() { TaskID = 3, TaskName = "Perform soil test", Predecessor = "2" },
new TaskData() { TaskID = 4, TaskName = "Soil test approval", Predecessor = "3FS+1" },
new TaskData() { TaskID = 5, TaskName = "Project estimation", Predecessor = "2,3FS" }
```

---

## Basic Dependency Setup

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt DataSource="@TaskCollection" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     EndDate="EndDate" Duration="Duration" Progress="Progress"
                     ParentID="ParentID" Dependency="Predecessor">
    </GanttTaskFields>
    <GanttEditSettings AllowTaskbarEditing="true" AllowEditing="true" />
</SfGantt>

@code {
    private List<TaskData> TaskCollection { get; set; }

    protected override void OnInitialized()
    {
        TaskCollection = new List<TaskData>()
        {
            new TaskData() { TaskID = 1, TaskName = "Project initiation",
                            StartDate = new DateTime(2022, 04, 05), EndDate = new DateTime(2022, 04, 08) },
            new TaskData() { TaskID = 2, TaskName = "Identify Site location",
                            StartDate = new DateTime(2022, 04, 05), Duration = "0",
                            Progress = 30, ParentID = 1 },
            new TaskData() { TaskID = 3, TaskName = "Perform soil test",
                            StartDate = new DateTime(2022, 04, 05), Duration = "4",
                            Progress = 40, ParentID = 1, Predecessor = "2" },
            new TaskData() { TaskID = 4, TaskName = "Soil test approval",
                            StartDate = new DateTime(2022, 04, 08), Duration = "0",
                            Progress = 30, ParentID = 1, Predecessor = "3FS+1" },
            new TaskData() { TaskID = 5, TaskName = "Project estimation",
                            StartDate = new DateTime(2022, 04, 04), EndDate = new DateTime(2022, 04, 17) },
            new TaskData() { TaskID = 6, TaskName = "Develop floor plan",
                            StartDate = new DateTime(2022, 04, 06), Duration = "3",
                            Progress = 30, ParentID = 5, Predecessor = "4" }
        };
    }

    public class TaskData
    {
        public int TaskID { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public DateTime? EndDate { get; set; }
        public string Duration { get; set; }
        public int Progress { get; set; }
        public int? ParentID { get; set; }
        public string Predecessor { get; set; }
    }
}
```

---

## Restrict Dependency Types

Use `DependencyTypes` to limit which dependency types users can create interactively. By default, all four types (FS, SS, FF, SF) are allowed.

```razor
<SfGantt DataSource="@TaskCollection"
         DependencyTypes="@(new List<DependencyType>() { DependencyType.FS, DependencyType.SS })">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" Dependency="Predecessor">
    </GanttTaskFields>
</SfGantt>
```

When `DependencyTypes` is restricted, the dependency dialog and connector drag will only show the allowed types.

---

## Predecessor Validation

### EnablePredecessorValidation

Controls whether the Gantt auto-validates that task dates align with dependency rules.

```razor
<SfGantt DataSource="@TaskCollection"
         EnablePredecessorValidation="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" Dependency="Predecessor">
    </GanttTaskFields>
</SfGantt>
```

- `true` (default) — Task start/end dates are automatically shifted to satisfy dependency constraints
- `false` — Dates are not adjusted; useful for manual scheduling mode

### AutoUpdatePredecessorOffset

When set to `true` (default), offset values are automatically recalculated when connected tasks are moved.

```razor
<SfGantt DataSource="@TaskCollection"
         EnablePredecessorValidation="true"
         AutoUpdatePredecessorOffset="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" Dependency="Predecessor">
    </GanttTaskFields>
</SfGantt>
```

### Circular Dependency Detection

The Gantt automatically detects circular dependencies and prevents them. When a user tries to create a circular link, the operation is cancelled and the component reverts to its previous state.

---

## Programmatic Dependency API

### Add Predecessor

```csharp
// Add a Finish-to-Start dependency: task 12 depends on task 4
Gantt.AddPredecessor(12, "4FS");

// Add with offset: task 12 depends on task 4 with 2-day lag
Gantt.AddPredecessor(12, "4FS+2");
```

**Overloads:**
```csharp
Gantt.AddPredecessor(int taskId, string predecessorString);
Gantt.AddPredecessor(string taskId, string predecessorString);
Gantt.AddPredecessor(Guid taskId, string predecessorString);
```

### Remove Predecessor

```csharp
// Remove all predecessors from task 2
Gantt.RemovePredecessor(2);
```

### Update Predecessor

```csharp
// Replace predecessor for task 7 with new dependency string
Gantt.UpdatePredecessor(7, "4FS+2");
```

### Complete Example

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Buttons

<div style="margin-bottom:12px;">
    <SfButton @onclick="AddDep">Add Predecessor</SfButton>
    <SfButton @onclick="RemoveDep">Remove Predecessor</SfButton>
    <SfButton @onclick="UpdateDep">Update Predecessor</SfButton>
</div>

<SfGantt @ref="GanttRef" DataSource="@TaskCollection" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" Dependency="Predecessor" ParentID="ParentID" />
    <GanttEditSettings AllowEditing="true" AllowTaskbarEditing="true" />
</SfGantt>

@code {
    private SfGantt<TaskData> GanttRef;

    private void AddDep()    => GanttRef.AddPredecessor(3, "2FS");
    private void RemoveDep() => GanttRef.RemovePredecessor(3);
    private void UpdateDep() => GanttRef.UpdatePredecessor(3, "2SS+1");
}
```

---

## Dependency Dialog Configuration

When `AllowEditing="true"` and `Mode="EditMode.Dialog"`, the edit dialog includes a **Predecessor** tab for managing dependencies visually.

```razor
<SfGantt DataSource="@TaskCollection">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" Dependency="Predecessor" ParentID="ParentID" />
    <GanttEditSettings AllowEditing="true" Mode="EditMode.Dialog" />
    <GanttEditDialogFields>
        <GanttEditDialogField Type="DialogFieldType.General" HeaderText="General" />
        <GanttEditDialogField Type="DialogFieldType.Dependency" HeaderText="Dependencies" />
    </GanttEditDialogFields>
</SfGantt>
```

The `Dependency` dialog tab lets users:
- View all predecessors for the selected task
- Add new predecessors with type and offset
- Delete existing predecessors
- Change dependency type (FS/SS/FF/SF)

---

## Connector Line Styling

Customize the visual appearance of dependency connector lines:

```razor
<SfGantt DataSource="@TaskCollection"
         ConnectorLineBackground="#0078D4"
         ConnectorLineWidth="2">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" Dependency="Predecessor" />
</SfGantt>
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ConnectorLineBackground` | `string` | `"transparent"` | Connector line color (hex or named) |
| `ConnectorLineWidth` | `int` | `1` | Connector line width in pixels |

---

## Dependency Events

### TaskbarEditing / TaskbarEdited

Triggered when a dependency connector is dragged to create/update a link:

```razor
<GanttEvents TaskbarEditing="OnTaskbarEditing"
             TaskbarEdited="OnTaskbarEdited"
             TValue="TaskData" />

@code {
    public void OnTaskbarEditing(TaskbarEditingEventArgs<TaskData> args)
    {
        // args.EditingFields.Predecessor contains the new predecessor string
        Console.WriteLine($"Editing predecessor: {args.EditingFields?.Predecessor}");
    }

    public void OnTaskbarEdited(TaskbarEditedEventArgs<TaskData> args)
    {
        Console.WriteLine($"New predecessor: {args.Data.Predecessor}");
    }
}
```

### OnActionBegin

Cancel dependency changes conditionally:

```razor
<GanttEvents OnActionBegin="ActionBeginHandler" TValue="TaskData" />

@code {
    public void ActionBeginHandler(GanttActionEventArgs<TaskData> args)
    {
        if (args.RequestType == "Predecessor" && args.NewTaskData?.Predecessor == "1FS")
        {
            args.Cancel = true; // Block this specific dependency
        }
    }
}
```

---

## Auto-Scheduling Interaction

When `EnablePredecessorValidation="true"` (default), auto-scheduling applies:
- Successor task start dates shift to comply with dependency constraints
- Offset values (lag/lead) affect the calculated gap
- The entire dependency chain updates when a predecessor task's dates change

When using **manual scheduling** (`TaskMode="ScheduleMode.Manual"`), validation is bypassed — set `EnablePredecessorValidation="false"` to avoid date auto-correction conflicts.

---

## Best Practices

- **Always map `Dependency` field** — Without it, dependency strings are ignored and no connectors are drawn
- **Keep predecessor strings normalized** — Use `3FS` not `3fs`; Gantt is case-sensitive
- **Restrict `DependencyTypes`** — If only FS dependencies are needed, restrict to `DependencyType.FS` to simplify the UI
- **Server-side validation** — Add cycle detection on the server when dependencies are persisted; client-side detection alone isn't sufficient for concurrent edits
- **Be cautious with `AutoUpdatePredecessorOffset = false`** — Offsets can drift when tasks are rescheduled manually

---

## Common Scenarios

### All Tasks FS Chained

```csharp
new TaskData() { TaskID = 1, TaskName = "Phase 1" },
new TaskData() { TaskID = 2, TaskName = "Phase 2", Predecessor = "1" },
new TaskData() { TaskID = 3, TaskName = "Phase 3", Predecessor = "2" },
new TaskData() { TaskID = 4, TaskName = "Phase 4", Predecessor = "3" }
```

### Multiple Predecessors

```csharp
// Task 5 can only start after both task 3 AND task 4 finish
new TaskData() { TaskID = 5, TaskName = "Final Sign-Off", Predecessor = "3FS,4FS" }
```

### Predecessor with Lag and Lead

```csharp
// Task 4 starts 2 days after task 3 ends (lag = +2)
new TaskData() { TaskID = 4, Predecessor = "3FS+2" }

// Task 4 starts 1 day before task 3 ends (lead = -1)
new TaskData() { TaskID = 4, Predecessor = "3FS-1" }
```

---

## Troubleshooting

**Issue**: Connector arrows not showing  
**Solution**: Confirm `Dependency="Predecessor"` (or equivalent field name) is set in `GanttTaskFields`. Check that referenced task IDs actually exist in the data.

**Issue**: Task dates not shifting after dependency change  
**Solution**: Verify `EnablePredecessorValidation="true"` (default). If using manual scheduling, set `TaskMode="ScheduleMode.Manual"` but note that date auto-correction is disabled.

**Issue**: Circular dependency error when there shouldn't be one  
**Solution**: Inspect the predecessor chain — even indirect cycles (A→B→C→A) are detected. Trace the full predecessor chain to find the loop.

**Issue**: Offset not applied correctly  
**Solution**: Check that the offset unit matches `DurationUnit` on `SfGantt`. Offsets in strings like `+2` are interpreted in the component's `DurationUnit` (Day, Hour, or Minute).

**Issue**: `AddPredecessor` not reflected in UI  
**Solution**: Call `StateHasChanged()` or use `await GanttRef.RefreshAsync()` if the UI doesn't update automatically.

---
