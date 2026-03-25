# Task Scheduling and Hierarchy

## Table of Contents
- [When to Read](#when-to-read)
- [Data Structures](#data-structures)
- [Task Types](#task-types)
- [Duration Units](#duration-units)
- [Scheduling Modes](#scheduling-modes)
- [Unscheduled Tasks](#unscheduled-tasks)
- [Indent and Outdent](#indent-and-outdent)
- [Expand and Collapse](#expand-and-collapse)
- [WBS Column](#wbs-column)
- [Key Properties Reference](#key-properties-reference)
- [Best Practices](#best-practices)
- [Common Scenarios](#common-scenarios)
- [Troubleshooting](#troubleshooting)

---

## When to Read

Use this reference when you need to:
- Configure hierarchical (nested) or self-referential (flat + ParentID) task data structures
- Understand how parent, child, and milestone tasks behave
- Set duration units (Day, Hour, Minute)
- Switch between auto-scheduling and manual scheduling
- Allow tasks without full date information (unscheduled tasks)
- Configure indent/outdent, expand/collapse behavior
- Enable WBS (Work Breakdown Structure) numbering

---

## Data Structures

The Gantt supports two hierarchical data patterns:

### 1. Self-Referential (Flat with ParentID)

Each row has a `ParentID` foreign key pointing to its parent row. This is the most common pattern for database-backed data.

```csharp
public class TaskData
{
    public int TaskID { get; set; }
    public string TaskName { get; set; }
    public DateTime StartDate { get; set; }
    public DateTime? EndDate { get; set; }
    public string Duration { get; set; }
    public int Progress { get; set; }
    public int? ParentID { get; set; }      // null = root task
    public string Predecessor { get; set; }
}
```

```razor
<GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                 EndDate="EndDate" Duration="Duration" Progress="Progress"
                 ParentID="ParentID" Dependency="Predecessor">
</GanttTaskFields>
```

### 2. Nested (Child Collection)

Tasks contain a nested `List<TaskData>` property for child tasks. Useful for pre-structured in-memory data.

```csharp
public class TaskData
{
    public int TaskID { get; set; }
    public string TaskName { get; set; }
    public DateTime StartDate { get; set; }
    public string Duration { get; set; }
    public int Progress { get; set; }
    public List<TaskData> SubTasks { get; set; }
}
```

```razor
<GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                 Duration="Duration" Progress="Progress"
                 Child="SubTasks">
</GanttTaskFields>
```

---

## Task Types

### Parent (Summary) Task

A task with child tasks. Dates and progress are rolled up automatically from children.
- `StartDate` = earliest child start
- `EndDate` = latest child end
- `Progress` = weighted average of child progress
- Cannot be edited directly when `AllowTaskbarEditing="true"` (parent taskbars are read-only by default)

### Child (Work Item) Task

A leaf task with no children. Has its own `StartDate`, `Duration`/`EndDate`, and `Progress`.

### Milestone

A zero-duration task rendered as a diamond shape.

```csharp
// Method 1: Zero duration string
new TaskData() { TaskID = 4, TaskName = "Approval", Duration = "0",
                 StartDate = new DateTime(2024, 01, 10) }

// Method 2: Use Milestone field mapping
new TaskData() { TaskID = 4, TaskName = "Approval", IsMilestone = true,
                 StartDate = new DateTime(2024, 01, 10) }
```

```razor
<GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                 Duration="Duration" Milestone="IsMilestone">
</GanttTaskFields>
```

### Unscheduled Task

A task without complete date information (see [Unscheduled Tasks](#unscheduled-tasks)).

---

## Duration Units

Control what unit the `Duration` field values represent using `DurationUnit`:

```razor
<SfGantt DataSource="@TaskCollection" DurationUnit="DurationUnit.Day">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" Duration="Duration" />
</SfGantt>
```

| Enum Value | Description | Example `"4"` means |
|------------|-------------|---------------------|
| `DurationUnit.Day` | Days (default) | 4 working days |
| `DurationUnit.Hour` | Hours | 4 working hours |
| `DurationUnit.Minute` | Minutes | 4 minutes |

> The `Duration` field on the data model can be `string`, `int`, or `double`. String format like `"4days"`, `"4hrs"` is also supported for mixed unit display.

**Mixed Duration Units in Display**

Individual task rows can show units differently in the Duration column using format strings, but the underlying calculation always uses `DurationUnit`.

---

## Scheduling Modes

### Auto Scheduling (Default)

Task dates are calculated automatically based on dependencies, working time, and calendar settings.

```razor
<SfGantt DataSource="@TaskCollection" TaskMode="ScheduleMode.Auto">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" Duration="Duration" />
</SfGantt>
```

### Manual Scheduling

Dates are fully controlled by the user. Dependencies do not auto-shift dates.

```razor
<SfGantt DataSource="@TaskCollection" TaskMode="ScheduleMode.Manual">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" Manual="IsManual">
    </GanttTaskFields>
</SfGantt>
```

```csharp
public class TaskData
{
    public int TaskID { get; set; }
    public string TaskName { get; set; }
    public DateTime StartDate { get; set; }
    public string Duration { get; set; }
    public bool IsManual { get; set; }  // Maps to Manual field
}
```

### Custom Scheduling (Mixed)

Individual tasks can be set to manual or auto via the `Manual` boolean field.

```razor
<SfGantt DataSource="@TaskCollection" TaskMode="ScheduleMode.Custom">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" Manual="IsManual">
    </GanttTaskFields>
</SfGantt>
```

When `IsManual = true` for a task, it is manually scheduled. When `false`, it is auto-scheduled.

---

## Unscheduled Tasks

Allow tasks with missing date or duration information using `AllowUnscheduledTasks="true"`:

```razor
<SfGantt DataSource="@TaskCollection" AllowUnscheduledTasks="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     EndDate="EndDate" Duration="Duration">
    </GanttTaskFields>
</SfGantt>
```

**Valid unscheduled combinations:**

| Has StartDate | Has EndDate/Duration | Display |
|---|---|---|
| ✅ | ❌ | Task starts on date, no end shown |
| ❌ | ✅ | Task has duration, no fixed start |
| ❌ | ❌ | Task shown without timeline bar |

```csharp
// Only start date
new TaskData() { TaskID = 2, TaskName = "Open-ended task",
                 StartDate = new DateTime(2024, 01, 10) }

// Only duration
new TaskData() { TaskID = 3, TaskName = "Floating task",
                 Duration = "5" }

// No date info
new TaskData() { TaskID = 4, TaskName = "Backlog item" }
```

---

## Indent and Outdent

Move tasks up or down in hierarchy using toolbar buttons or programmatic API.

### Enable via Toolbar

```razor
<SfGantt DataSource="@TaskCollection"
         Toolbar="@(new List<string>() { "Indent", "Outdent" })">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" Duration="Duration" ParentID="ParentID" />
    <GanttEditSettings AllowAdding="true" AllowEditing="true" />
</SfGantt>
```

### Programmatic API

```csharp
// Indent selected task (make it a child of the row above)
await GanttRef.IndentAsync();

// Outdent selected task (move it up one level in hierarchy)
await GanttRef.OutdentAsync();
```

**Rules:**
- Indent makes the selected task a child of the task immediately above it
- Outdent moves the task one level up
- The first task in a group cannot be indented (no parent above it)
- Root-level tasks cannot be outdented further

---

## Expand and Collapse

### User Interaction

Click the expand/collapse arrow on parent task rows to show/hide children.

### Programmatic API

```csharp
// Expand all parent tasks
await GanttRef.ExpandAllAsync();

// Collapse all parent tasks
await GanttRef.CollapseAllAsync();

// Expand to a specific level (0 = root only, 1 = first level, etc.)
await GanttRef.ExpandAtLevelAsync(1);

// Collapse to a specific level
await GanttRef.CollapseAtLevelAsync(0);

// Expand/collapse by task key
await GanttRef.ExpandByKeyAsync(5);
await GanttRef.CollapseByKeyAsync(5);
```

### Default Collapsed State

Start with all parents collapsed:

```razor
<SfGantt DataSource="@TaskCollection" CollapseAllParentTasks="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" ParentID="ParentID" />
</SfGantt>
```

### Expand/Collapse Events

```razor
<GanttEvents Expanding="OnExpanding" Collapsing="OnCollapsing" TValue="TaskData" />

@code {
    public void OnExpanding(RowExpandingEventArgs<TaskData> args)
    {
        // Cancel expand for specific rows
        if (args.Data.TaskID == 3)
            args.Cancel = true;
    }

    public void OnCollapsing(RowCollapsingEventArgs<TaskData> args)
    {
        Console.WriteLine($"Collapsing: {args.Data.TaskName}");
    }
}
```

---

## WBS Column

Display Work Breakdown Structure numbering using `ShowWbsColumn`:

```razor
<SfGantt DataSource="@TaskCollection"
         ShowWbsColumn="true"
         AutoGenerateWbs="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" Duration="Duration" ParentID="ParentID" />
</SfGantt>
```

- `ShowWbsColumn="true"` — Adds a WBS column to the grid
- `AutoGenerateWbs="true"` — WBS values are calculated automatically (1, 1.1, 1.1.1, 1.2, etc.)

When `AutoGenerateWbs="false"`, map a data model field containing pre-calculated WBS values via `GanttTaskFields.WBS`.

---

## Key Properties Reference

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `DurationUnit` | `DurationUnit` | `Day` | Unit for task duration values |
| `TaskMode` | `ScheduleMode` | `Auto` | Auto / Manual / Custom scheduling |
| `AllowUnscheduledTasks` | `bool` | `false` | Allow tasks with missing date/duration |
| `CollapseAllParentTasks` | `bool` | `false` | Start with all parents collapsed |
| `ShowWbsColumn` | `bool` | `false` | Show WBS numbering column |
| `AutoGenerateWbs` | `bool` | `false` | Auto-calculate WBS values |
| `TreeColumnIndex` | `int` | `0` | Which column shows expand/collapse arrows |
| `AutoCalculateDateScheduling` | `bool` | `true` | Auto-recalculate dates from dependencies |

---

## Best Practices

- **Use self-referential data** for database-backed sources — easier to persist and query
- **Set `DurationUnit` early** — changing it after data is loaded can cause unexpected recalculations
- **Use `AllowUnscheduledTasks` sparingly** — it complicates date calculations and UI rendering
- **Preserve expand state** after data refresh — store `GetExpandedRowIndexes()` before refresh and restore afterward
- **Use `TreeColumnIndex`** to move hierarchy expand/collapse arrows away from the first column when needed

---

## Common Scenarios

### Project with Phases and Tasks

```csharp
new TaskData() { TaskID = 1, TaskName = "Phase 1: Design" },
new TaskData() { TaskID = 2, TaskName = "Requirements",  Duration = "5", ParentID = 1 },
new TaskData() { TaskID = 3, TaskName = "UI Mockups",    Duration = "3", ParentID = 1, Predecessor = "2" },
new TaskData() { TaskID = 4, TaskName = "Phase 2: Build" },
new TaskData() { TaskID = 5, TaskName = "Frontend",      Duration = "7", ParentID = 4, Predecessor = "3" },
new TaskData() { TaskID = 6, TaskName = "Backend",       Duration = "8", ParentID = 4, Predecessor = "3" }
```

### Hour-Based Scheduling

```razor
<SfGantt DataSource="@TaskCollection" DurationUnit="DurationUnit.Hour">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" Duration="Duration" />
    <GanttDayWorkingTime>
        <GanttDayWorkingTime From="8" To="17" />   <!-- 9-hour working day -->
    </GanttDayWorkingTime>
</SfGantt>
```

---

## Troubleshooting

**Issue**: Parent task dates don't update when child dates change  
**Solution**: Ensure `AutoCalculateDateScheduling="true"` (default). Check that the child's `ParentID` references the correct parent `TaskID`.

**Issue**: Milestones show as a bar, not a diamond  
**Solution**: Verify `Duration = "0"` (string zero) or the `Milestone` field is set to `true` and mapped in `GanttTaskFields.Milestone`.

**Issue**: Indent/Outdent buttons are disabled  
**Solution**: Confirm `AllowEditing="true"` in `GanttEditSettings`. A row must be selected before indent/outdent is available.

**Issue**: `DurationUnit.Hour` tasks show incorrect durations  
**Solution**: Verify `DayWorkingTime` configuration. Hours are calculated within the working day window, not over 24 hours.

**Issue**: Task without StartDate not rendering on chart  
**Solution**: Set `AllowUnscheduledTasks="true"`. Without it, tasks missing required date fields are skipped.

---
