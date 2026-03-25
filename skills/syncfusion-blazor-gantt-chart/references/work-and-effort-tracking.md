# Work and Effort Tracking

## Table of Contents
- [When to Read](#when-to-read)
- [Concepts Overview](#concepts-overview)
- [TaskType Enum](#tasktype-enum)
- [WorkUnit Property](#workunit-property)
- [Field Mapping](#field-mapping)
- [TaskType Behaviors](#tasktype-behaviors)
- [Work Calculation Formula](#work-calculation-formula)
- [Resource Interaction](#resource-interaction)
- [Complete Example](#complete-example)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## When to Read

Use this reference when you need to:
- Track effort (work hours) in addition to calendar duration
- Configure Fixed Work, Fixed Duration, or Fixed Units task scheduling
- Understand how adding/removing resources affects duration and work
- Set the unit of work measurement (Hours, Days, Minutes)

---

## Concepts Overview

The Gantt work tracking model has three fields per task:

| Field | Description |
|-------|-------------|
| `Duration` | Calendar span of the task (e.g. 5 days) |
| `Work` | Total effort in hours (or days/minutes) |
| `ResourceUnits` | Percentage of each resource's time assigned (0–100%) |

The relationship is:

```
Work = Duration × ResourceUnits / 100
```

When one field changes, the `TaskType` determines which of the other two fields is recalculated.

---

## TaskType Enum

| Value | Description | When Duration changes | When Work changes | When Units change |
|-------|-------------|----------------------|-------------------|-------------------|
| `TaskType.FixedDuration` | Duration is locked | — (fixed) | Units recalculate | Duration stays; Work recalculates |
| `TaskType.FixedWork` | Work hours are locked | Units recalculate | — (fixed) | Duration recalculates |
| `TaskType.FixedUnit` | Resource units are locked | Work recalculates | Duration recalculates | — (fixed) |

Default: `TaskType.FixedUnit`

---

## WorkUnit Property

`WorkUnit` controls the display unit for work values:

| Value | Description |
|-------|-------------|
| `WorkUnit.Hour` | Work displayed as hours (e.g. 40h) — default |
| `WorkUnit.Day` | Work displayed as days (e.g. 5d) |
| `WorkUnit.Minute` | Work displayed as minutes (e.g. 2400min) |

```razor
<SfGantt DataSource="@TaskCollection"
         WorkUnit="WorkUnit.Hour"
         Height="450px" Width="900px">
</SfGantt>
```

---

## Field Mapping

Map the `Work` and `TaskType` fields in `GanttTaskFields`:

```razor
<GanttTaskFields Id="TaskID" Name="TaskName"
                 StartDate="StartDate" Duration="Duration"
                 Progress="Progress" ParentID="ParentID"
                 ResourceInfo="Resources"
                 Work="Work"
                 TaskType="TaskType" />
```

Update the data model to include the mapped properties:

```csharp
public class TaskData
{
    public int TaskID { get; set; }
    public string TaskName { get; set; }
    public DateTime StartDate { get; set; }
    public string Duration { get; set; }
    public int Progress { get; set; }
    public int? ParentID { get; set; }
    public double Work { get; set; }
    public string TaskType { get; set; }         // "FixedDuration", "FixedWork", or "FixedUnit"
    public List<ResourceInfo> Resources { get; set; }
}

public class ResourceInfo
{
    public int ResourceId { get; set; }
    public string ResourceName { get; set; }
    public double Unit { get; set; }  // 0–100 percentage
}
```

> **Note:** `TaskType` in data model is a `string` with values `"FixedDuration"`, `"FixedWork"`, `"FixedUnit"`.

---

## TaskType Behaviors

### FixedDuration

The task spans a fixed calendar period. Work adjusts when resources change.

```csharp
new TaskData
{
    TaskID = 2, TaskName = "Design", Duration = "5",
    TaskType = "FixedDuration",
    Resources = new() { new ResourceInfo { ResourceId = 1, Unit = 100 } }
    // Work = 5 days × 8h × 100% = 40h (auto-calculated)
}
```

**Use for:** Fixed-deadline tasks where calendar time cannot change regardless of resource allocation.

### FixedWork

The total effort is locked. Duration adjusts when resources are added/removed.

```csharp
new TaskData
{
    TaskID = 3, TaskName = "Development", Work = 80,
    TaskType = "FixedWork",
    Resources = new() { new ResourceInfo { ResourceId = 1, Unit = 100 } }
    // Duration = 80h ÷ (8h/day × 100%) = 10 days (auto-calculated)
}
```

**Use for:** Tasks where total effort is estimated and adding resources shortens the timeline.

### FixedUnit

Resource allocation percentage is fixed. Work recalculates when duration changes.

```csharp
new TaskData
{
    TaskID = 4, TaskName = "Testing", Duration = "4",
    TaskType = "FixedUnit",
    Resources = new() { new ResourceInfo { ResourceId = 2, Unit = 50 } }
    // Work = 4 days × 8h × 50% = 16h (auto-calculated)
}
```

**Use for:** Tasks where a resource works at a fixed capacity (e.g. part-time).

---

## Work Calculation Formula

For a single resource:

$$\text{Work} = \text{Duration (days)} \times \text{Hours per day} \times \frac{\text{Unit}}{100}$$

For multiple resources:

$$\text{Work} = \text{Duration} \times \text{Hours per day} \times \sum_{i=1}^{n} \frac{\text{Unit}_i}{100}$$

Where "Hours per day" is derived from the `GanttDayWorkingTime` configuration (default: 8).

---

## Resource Interaction

When resources are added, edited, or removed via the edit dialog or programmatic API, the Gantt recalculates the linked field based on `TaskType`:

| User Action | FixedDuration | FixedWork | FixedUnit |
|-------------|--------------|-----------|-----------|
| Add resource | Work↑ | Duration↓ | Work↑ |
| Remove resource | Work↓ | Duration↑ | Work↓ |
| Change unit% | Work changes | Duration changes | — (units fixed) |
| Change duration | Work changes | Units change | Work changes |

---

## Complete Example

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt DataSource="@TaskCollection"
         Height="500px"
         Width="1000px"
         WorkUnit="WorkUnit.Hour">

    <GanttTaskFields Id="TaskID" Name="TaskName"
                     StartDate="StartDate" Duration="Duration"
                     Progress="Progress" ParentID="ParentID"
                     ResourceInfo="Resources"
                     Work="Work"
                     TaskType="TaskType" />

    <GanttResourceFields Id="ResourceId" Name="ResourceName"
                         Unit="Unit" TResources="ResourceInfo" />

    <GanttResources DataSource="@ResourceCollection" TValue="TaskData" TResources="ResourceInfo" />

    <GanttColumns>
        <GanttColumn Field="TaskID" Width="50" />
        <GanttColumn Field="TaskName" HeaderText="Task" Width="150" />
        <GanttColumn Field="Work" HeaderText="Work (h)" Width="90" />
        <GanttColumn Field="Duration" Width="80" />
        <GanttColumn Field="TaskType" HeaderText="Type" Width="110" />
    </GanttColumns>

    <GanttEditSettings AllowEditing="true" Mode="EditMode.Dialog" />
    <GanttEditDialogFields>
        <GanttEditDialogField Type="DialogFieldType.General" />
        <GanttEditDialogField Type="DialogFieldType.Resources" />
    </GanttEditDialogFields>
</SfGantt>

@code {
    private List<TaskData> TaskCollection = new()
    {
        new TaskData { TaskID = 1, TaskName = "Project", StartDate = new DateTime(2024, 04, 01) },
        new TaskData
        {
            TaskID = 2, TaskName = "Design (Fixed Duration)", Duration = "5",
            Progress = 80, ParentID = 1, StartDate = new DateTime(2024, 04, 01),
            TaskType = "FixedDuration",
            Resources = new() { new ResourceInfo { ResourceId = 1, Unit = 100 } }
        },
        new TaskData
        {
            TaskID = 3, TaskName = "Build (Fixed Work)", Work = 80,
            Progress = 30, ParentID = 1, StartDate = new DateTime(2024, 04, 08),
            TaskType = "FixedWork",
            Resources = new() { new ResourceInfo { ResourceId = 2, Unit = 100 } }
        },
        new TaskData
        {
            TaskID = 4, TaskName = "Testing (Fixed Unit)", Duration = "4",
            Progress = 0, ParentID = 1, StartDate = new DateTime(2024, 04, 22),
            TaskType = "FixedUnit",
            Resources = new() { new ResourceInfo { ResourceId = 1, Unit = 50 } }
        }
    };

    private List<ResourceInfo> ResourceCollection = new()
    {
        new ResourceInfo { ResourceId = 1, ResourceName = "Alice" },
        new ResourceInfo { ResourceId = 2, ResourceName = "Bob" }
    };

    public class TaskData
    {
        public int TaskID { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public string Duration { get; set; }
        public int Progress { get; set; }
        public int? ParentID { get; set; }
        public double Work { get; set; }
        public string TaskType { get; set; }
        public List<ResourceInfo> Resources { get; set; }
    }

    public class ResourceInfo
    {
        public int ResourceId { get; set; }
        public string ResourceName { get; set; }
        public double Unit { get; set; }
    }
}
```

---

## Best Practices

- **Explicitly set `TaskType` per task** — relying on the global default can produce unexpected recalculation when editing specific tasks
- **Use `FixedWork` for effort-based estimation** — allows team leads to add resources to shorten the schedule
- **Use `FixedDuration` for deadline-bound tasks** — prevents resources from changing the end date
- **Show the `Work` column in the grid** — gives users visibility into computed effort, not just duration
- **Map `TaskType` as a string column** with a dropdown editor for user-selectable task type in Dialog mode

---

## Troubleshooting

**Issue**: `Work` column shows 0 for all tasks  
**Solution**: Ensure `Work="Work"` is mapped in `GanttTaskFields` AND the data model's `Work` property has a value (or resources are assigned for auto-calculation).

**Issue**: Adding a resource doesn't change duration (FixedWork task)  
**Solution**: Confirm `TaskType` in the data row is `"FixedWork"` (not the default `"FixedUnit"`).

**Issue**: Duration changes unexpectedly when editing resources  
**Solution**: This is expected behavior for `FixedWork` tasks. Switch to `FixedDuration` if you want calendar time locked.

**Issue**: `TaskType` dropdown not visible in edit dialog  
**Solution**: Add a `GanttEditDialogField Type="DialogFieldType.General"` — the TaskType field appears within the General tab.

---
