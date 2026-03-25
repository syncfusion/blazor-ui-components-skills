# Critical Path and Analysis

## Table of Contents
- [When to Use](#when-to-use)
- [How Critical Path Works](#how-critical-path-works)
- [Enable Critical Path](#enable-critical-path)
- [Configure Slack Value](#configure-slack-value)
- [Customize Critical Taskbar Appearance](#customize-critical-taskbar-appearance)
- [Get Critical Tasks Programmatically](#get-critical-tasks-programmatically)
- [Key Properties and APIs](#key-properties-and-apis)
- [Common Scenarios and Decisions](#common-scenarios-and-decisions)
- [Troubleshooting](#troubleshooting)

---

## When to Use

Guide the user to this reference when they need to:
- Highlight which tasks directly control the project completion date
- Identify tasks that cannot slip without delaying the project
- Visually differentiate critical tasks from non-critical ones
- Widen the critical zone to flag near-critical tasks as well
- Retrieve the list of critical tasks programmatically at runtime
- Customize the appearance of critical taskbars with custom colors or styles

---

## How Critical Path Works

The critical path is the longest sequence of dependent tasks that determines the minimum project duration. The Gantt component uses Critical Path Method (CPM) to automatically calculate and highlight these tasks in red when `EnableCriticalPath` is set to `true`.

**Key calculation rules to keep in mind:**
- A task is critical when its end date falls within `SlackValue` days of the project end date (default `SlackValue` is 0)
- Project end date = `ProjectEndDate` property if set; otherwise the latest task end date in the data source
- Only tasks with **less than 100% progress** can be marked critical — completed tasks are excluded
- Criticality is evaluated per-task based on dependency constraints, not parent-child hierarchy
- The critical path **recalculates automatically** when task dates, durations, dependencies, or progress change
- All dependency types (Finish-to-Start, Start-to-Start, Finish-to-Finish, Start-to-Finish) and offsets are factored in

---

## Enable Critical Path

When the user wants to turn on critical path highlighting, add `EnableCriticalPath="true"` to `SfGantt`. Add `"CriticalPath"` to the `Toolbar` to let the user toggle it interactively at runtime.

**Prerequisites:** Task data must include valid `StartDate`, `EndDate`/`Duration`, and the `Dependency` field must be mapped in `GanttTaskFields`.

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt DataSource="@TaskCollection"
         EnableCriticalPath="true"
         Toolbar="@(new List<string>() { "Add", "Edit", "Delete", "Cancel", "CriticalPath" })"
         Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID"
                     Dependency="Predecessor">
    </GanttTaskFields>
    <GanttEditSettings AllowEditing="true" AllowAdding="true" AllowDeleting="true">
    </GanttEditSettings>
</SfGantt>

@code {
    private List<TaskData> TaskCollection { get; set; }

    protected override void OnInitialized()
    {
        TaskCollection = new List<TaskData>
        {
            new TaskData { TaskID = 1, TaskName = "Project initiation", StartDate = new DateTime(2024, 01, 04), EndDate = new DateTime(2024, 01, 17) },
            new TaskData { TaskID = 2, TaskName = "Identify site location", StartDate = new DateTime(2024, 01, 04), Duration = "0", Progress = 30, ParentID = 1 },
            new TaskData { TaskID = 3, TaskName = "Perform soil test", StartDate = new DateTime(2024, 01, 04), Duration = "4", Progress = 40, ParentID = 1, Predecessor = "2" },
            new TaskData { TaskID = 4, TaskName = "Soil test approval", StartDate = new DateTime(2024, 01, 04), Duration = "0", Progress = 30, ParentID = 1, Predecessor = "3" },
            new TaskData { TaskID = 5, TaskName = "Project estimation", StartDate = new DateTime(2024, 01, 04), EndDate = new DateTime(2024, 01, 17) },
            new TaskData { TaskID = 6, TaskName = "Develop floor plan", StartDate = new DateTime(2024, 01, 06), Duration = "3", Progress = 30, ParentID = 5 },
            new TaskData { TaskID = 7, TaskName = "List materials", StartDate = new DateTime(2024, 01, 06), Duration = "3", Progress = 40, ParentID = 5, Predecessor = "6" },
            new TaskData { TaskID = 8, TaskName = "Estimation approval", StartDate = new DateTime(2024, 01, 06), Duration = "0", Progress = 30, ParentID = 5, Predecessor = "7" }
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

Critical tasks are highlighted in red with emphasized dependency connector lines automatically.

---

## Configure Slack Value

When the user wants to flag near-critical tasks — not just zero-slack tasks — use `GanttCriticalPathSettings` with a `SlackValue` greater than 0. `SlackValue` defines how many days before the project end date a task should be treated as critical.

- `SlackValue = 0` (default) — only tasks with zero slack are critical
- `SlackValue = 2` — tasks ending within 2 days of the project end date are also flagged critical

```razor
<SfGantt DataSource="@TaskCollection" EnableCriticalPath="true" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttCriticalPathSettings SlackValue="2"></GanttCriticalPathSettings>
</SfGantt>
```

**When to increase `SlackValue`:** Use it when the project has tight schedules and the user wants early warnings on tasks approaching the critical zone, not just tasks already on it.

---

## Customize Critical Taskbar Appearance

When the user wants to apply a custom color or style to critical taskbars (instead of the default red), use the `QueryChartRowInfo` event. Check `GanttTaskModel.IsCritical` to target critical tasks and `HasChildRecords` to skip parent summary bars.

```razor
<SfGantt DataSource="@TaskCollection" EnableCriticalPath="true" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttEvents QueryChartRowInfo="OnQueryChartRowInfo" TValue="TaskData"></GanttEvents>
</SfGantt>

@code {
    public void OnQueryChartRowInfo(QueryChartRowInfoEventArgs<TaskData> args)
    {
        // Apply custom CSS only to critical leaf tasks (not parent summary bars)
        if (args.GanttTaskModel.IsCritical && !args.GanttTaskModel.HasChildRecords)
        {
            args.Row.AddClass(new string[] { "critical-taskbar critical-progress" });
        }
    }
}

<style>
    .critical-taskbar .e-gantt-child-taskbar {
        background-color: #06DFF9 !important;
        outline: 1px solid #06DFF9 !important;
    }
    .critical-progress .e-gantt-child-progressbar {
        background-color: #049cae !important;
    }
</style>
```

**Pattern:** Input = `IsCritical` flag → Output = custom CSS class applied to the taskbar row.

---

## Get Critical Tasks Programmatically

When the user needs to retrieve which tasks are currently critical (e.g., for reporting or conditional logic), call `GetCriticalTasksAsync()` on the `SfGantt` reference.

```razor
<SfGantt @ref="GanttRef" DataSource="@TaskCollection" EnableCriticalPath="true"
         Toolbar="@(new List<string>() { "CriticalPath" })" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
</SfGantt>
<button @onclick="ShowCriticalTasks">Get Critical Tasks</button>

@code {
    private SfGantt<TaskData> GanttRef;

    private async Task ShowCriticalTasks()
    {
        var criticalTasks = await GanttRef.GetCriticalTasksAsync();
        // criticalTasks is a list of task records currently on the critical path
        foreach (var task in criticalTasks)
        {
            Console.WriteLine(task.TaskName);
        }
    }
}
```

---

## Key Properties and APIs

| Property / Method | Where Used | Purpose |
|---|---|---|
| `EnableCriticalPath` | `SfGantt` | Enables critical path calculation and red highlighting |
| `GanttCriticalPathSettings` | Child component | Holds critical path configuration |
| `SlackValue` | `GanttCriticalPathSettings` | Days threshold to widen critical zone (default: `0`) |
| `ProjectEndDate` | `SfGantt` | Fixed project end date reference; if not set, uses max task end date |
| `GetCriticalTasksAsync()` | `SfGantt` method | Returns list of currently critical tasks at runtime |
| `QueryChartRowInfo` | `GanttEvents` | Event to customize taskbar appearance per row |
| `GanttTaskModel.IsCritical` | Event arg | `true` when the task is on the critical path |
| `GanttTaskModel.HasChildRecords` | Event arg | `true` for parent summary tasks; use to skip them in customization |
| `"CriticalPath"` toolbar item | `Toolbar` | Built-in toggle button for interactive critical path on/off |

---

## Common Scenarios and Decisions

**User wants to see which tasks will delay the project if they slip**
→ Enable `EnableCriticalPath="true"`. Critical tasks are highlighted in red automatically.

**User wants to flag tasks that are close to critical, not just exactly critical**
→ Add `<GanttCriticalPathSettings SlackValue="N">` where N = number of buffer days.

**User wants a toggle button to turn critical path on/off in the UI**
→ Add `"CriticalPath"` to the `Toolbar` array. No additional code needed.

**User wants to know why a task they expect to be critical is not highlighted**
→ Check: (1) task `Progress` is less than 100%, (2) `Dependency` field is mapped in `GanttTaskFields`, (3) dependency chain is correctly set in the data.

**User wants to apply a brand color to critical tasks instead of the default red**
→ Use `QueryChartRowInfo` event, check `IsCritical && !HasChildRecords`, then call `args.Row.AddClass()` with custom CSS.

**User wants to retrieve critical tasks for a report or log**
→ Call `await GanttRef.GetCriticalTasksAsync()` after the component renders.

**Parent task appears critical but child tasks do not**
→ This is correct behavior. Criticality is evaluated per task based on dependency constraints, not hierarchy. A critical parent does not automatically make children critical.

---

## Troubleshooting

**Critical path is not showing any highlighted tasks**
- Confirm `EnableCriticalPath="true"` is set on `SfGantt`
- Ensure task data has the `Dependency` field mapped via `GanttTaskFields.Dependency`
- Verify task `Progress` values are below 100 — fully completed tasks are excluded
- Check that `ProjectEndDate` (if set) is not earlier than all task end dates

**Unexpected tasks appear critical**
- `SlackValue` may be set too high, widening the critical zone beyond intent — reduce it
- Dependency offsets (e.g., `+2` days) may be creating timing conflicts — review predecessor data

**Custom taskbar color is not applying**
- Confirm the CSS class targets `.e-gantt-child-taskbar` (not `.e-gantt-parent-taskbar`) for leaf tasks
- Ensure `!important` is used on background-color — the default red uses high specificity
- Verify `HasChildRecords` check is used to skip parent rows if needed

**Critical path changes every time a task is edited**
- This is expected behavior. The calculation is live and recalculates after every change to dates, duration, dependencies, or progress.