# Segment Fields and Split Tasks — API Reference

Comprehensive guide for `GanttSegmentFields<TValue, TSegments>` class, which enables task splitting and merging by managing segment collections for segmented taskbars in project timelines.

## Table of Contents
- [Overview](#overview)
- [GanttSegmentFields Class](#ganttsegmentfields-class)
- [Properties](#properties)
- [Configuration Pattern](#configuration-pattern)
- [Split Task Methods](#split-task-methods)
- [Merge Task Methods](#merge-task-methods)
- [UI-Based Operations](#ui-based-operations)
- [Segment Visualization](#segment-visualization)
- [Common Patterns](#common-patterns)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## Overview

The `GanttSegmentFields<TValue, TSegments>` component manages **split tasks** — tasks divided into multiple segments with breaks in between. This represents paused work, interrupted schedules, or tasks with non-continuous execution periods.

**Use Cases:**
- Tasks with planned breaks or interruptions
- Resources temporarily unavailable during task execution
- Tasks paused for approvals or dependencies
- Flexible timeline adjustments for dynamic projects

**Key Features:**
- Load segments at initialization from data source
- Split tasks dynamically via dialog or context menu
- Merge segments programmatically or via UI
- Visual representation with multiple taskbar segments
- Maintains task start/end dates across segments

---

## GanttSegmentFields Class

**Namespace:** `Syncfusion.Blazor.Gantt`

**Type Parameters:**
- **`TValue`** — Type of task collection data source
- **`TSegments`** — Type of segment collection data source

**Inheritance:** `SfOwningComponentBase` → `GanttSegmentFields<TValue, TSegments>`

**Implements:** `IComponent`, `IHandleEvent`, `IHandleAfterRender`, `IDisposable`

---

## Properties

### DataSource
**Type:** `IEnumerable<TSegments>`

The segment collection containing split task data. Each record represents one segment of a split task.

**Required:** Yes

**Example:**
```csharp
<GanttSegmentFields DataSource="@SegmentCollection" ...>
</GanttSegmentFields>

@code {
    private List<SegmentModel> SegmentCollection { get; set; }
}
```

---

### PrimaryKey
**Type:** `string`

The property name that uniquely identifies each segment record in the segment collection.

**Required:** Yes

**Example:**
```csharp
<GanttSegmentFields PrimaryKey="Id" ...>
</GanttSegmentFields>

public class SegmentModel
{
    public int Id { get; set; }  // Unique segment identifier
}
```

---

### ForeignKey
**Type:** `string`

The property name in the segment collection that maps to the task's primary key (from `GanttTaskFields.Id`). Establishes the foreign key relationship between segments and tasks.

**Required:** Yes

**Example:**
```csharp
<GanttSegmentFields ForeignKey="TaskID" ...>
</GanttSegmentFields>

public class SegmentModel
{
    public int Id { get; set; }
    public int TaskID { get; set; }  // Links to task primary key
}
```

---

### StartDate
**Type:** `string`

The property name in the segment collection that specifies the segment's start date.

**Required:** Yes

**Example:**
```csharp
<GanttSegmentFields StartDate="SegmentStartDate" ...>
</GanttSegmentFields>

public class SegmentModel
{
    public DateTime SegmentStartDate { get; set; }
}
```

---

### EndDate
**Type:** `string`

The property name in the segment collection that specifies the segment's end date.

**Required:** Yes (or Duration)

**Example:**
```csharp
<GanttSegmentFields EndDate="SegmentEndDate" ...>
</GanttSegmentFields>

public class SegmentModel
{
    public DateTime SegmentEndDate { get; set; }
}
```

---

### Duration
**Type:** `string`

The property name in the segment collection that specifies the segment's duration. Can be used instead of EndDate.

**Required:** Yes (or EndDate)

**Example:**
```csharp
<GanttSegmentFields Duration="SegmentDuration" ...>
</GanttSegmentFields>

public class SegmentModel
{
    public string SegmentDuration { get; set; }  // e.g., "3 days"
}
```

---

## Configuration Pattern

### Complete Setup Example

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt @ref="GanttInstance"
         DataSource="@TaskCollection" 
         Height="450px" 
         Width="100%" 
         TreeColumnIndex="1"
         EnableContextMenu="true"
         ProjectStartDate="@ProjectStart" 
         ProjectEndDate="@ProjectEnd">
    <GanttTaskFields Id="TaskID" 
                     Name="TaskName" 
                     StartDate="StartDate" 
                     EndDate="EndDate" 
                     Duration="Duration" 
                     ParentID="ParentID">
    </GanttTaskFields>
    
    <GanttSegmentFields PrimaryKey="Id" 
                        ForeignKey="TaskID" 
                        StartDate="SegmentStartDate" 
                        EndDate="SegmentEndDate" 
                        Duration="SegmentDuration"
                        TValue="TaskData" 
                        TSegments="SegmentModel" 
                        DataSource="@SegmentCollection">
    </GanttSegmentFields>
    
    <GanttLabelSettings LeftLabel="TaskName" TValue="TaskData">
    </GanttLabelSettings>
    
    <GanttEditSettings AllowEditing="true" 
                       AllowTaskbarEditing="true">
    </GanttEditSettings>
</SfGantt>

@code {
    private SfGantt<TaskData> GanttInstance;
    private DateTime ProjectStart = new DateTime(2024, 1, 1);
    private DateTime ProjectEnd = new DateTime(2024, 3, 31);
    
    private List<TaskData> TaskCollection { get; set; }
    private List<SegmentModel> SegmentCollection { get; set; }
    
    protected override void OnInitialized()
    {
        TaskCollection = GetTasks();
        SegmentCollection = GetSegments();
    }
    
    // Task model
    public class TaskData
    {
        public int TaskID { get; set; }
        public string TaskName { get; set; }
        public DateTime? StartDate { get; set; }
        public DateTime? EndDate { get; set; }
        public string Duration { get; set; }
        public int? ParentID { get; set; }
    }
    
    // Segment model
    public class SegmentModel
    {
        public int Id { get; set; }                    // Unique segment ID
        public int TaskID { get; set; }                 // Foreign key to task
        public DateTime SegmentStartDate { get; set; }
        public DateTime SegmentEndDate { get; set; }
        public string SegmentDuration { get; set; }
    }
    
    private List<TaskData> GetTasks()
    {
        return new List<TaskData>
        {
            new TaskData { 
                TaskID = 1, 
                TaskName = "Project Initiation", 
                StartDate = new DateTime(2024, 1, 1), 
                Duration = "10"
            },
            new TaskData { 
                TaskID = 2, 
                TaskName = "Design Phase (Split)", 
                StartDate = new DateTime(2024, 1, 15), 
                Duration = "12"  // Total duration including gaps
            },
            new TaskData { 
                TaskID = 3, 
                TaskName = "Development", 
                StartDate = new DateTime(2024, 2, 1), 
                Duration = "20"
            }
        };
    }
    
    private List<SegmentModel> GetSegments()
    {
        return new List<SegmentModel>
        {
            // Task 2 is split into 3 segments with breaks
            new SegmentModel { 
                Id = 1, 
                TaskID = 2, 
                SegmentStartDate = new DateTime(2024, 1, 15),
                SegmentEndDate = new DateTime(2024, 1, 18),
                SegmentDuration = "3"
            },
            new SegmentModel { 
                Id = 2, 
                TaskID = 2, 
                SegmentStartDate = new DateTime(2024, 1, 22),  // Gap from Jan 19-21
                SegmentEndDate = new DateTime(2024, 1, 25),
                SegmentDuration = "3"
            },
            new SegmentModel { 
                Id = 3, 
                TaskID = 2, 
                SegmentStartDate = new DateTime(2024, 1, 29),  // Gap from Jan 26-28
                SegmentEndDate = new DateTime(2024, 2, 2),
                SegmentDuration = "4"
            }
        };
    }
}
```

---

## Split Task Methods

### SplitTaskAsync(int id, List<DateTime> splitDates)

Splits a task by its integer ID at specified dates.

**Parameters:**
- `id` (`int`) — Task ID to split
- `splitDates` (`List<DateTime>`) — List of dates where task should be split

**Returns:** `Task`

**Example:**
```csharp
<button @onclick="SplitTask">Split Task 5</button>

@code {
    private async Task SplitTask()
    {
        await GanttInstance.SplitTaskAsync(5, new List<DateTime>
        {
            new DateTime(2024, 1, 20),  // First split point
            new DateTime(2024, 1, 25)   // Second split point
        });
    }
}
```

**Result:** Task 5 will be divided into 3 segments:
1. Task start → Jan 20
2. Jan 21 → Jan 25
3. Jan 26 → Task end

---

### SplitTaskAsync(string id, List<DateTime> splitDates)

Splits a task by its string ID.

**Parameters:**
- `id` (`string`) — Task ID to split
- `splitDates` (`List<DateTime>`) — List of dates where task should be split

**Example:**
```csharp
await GanttInstance.SplitTaskAsync("TASK-001", new List<DateTime>
{
    new DateTime(2024, 2, 1)
});
```

---

### SplitTaskAsync(Guid id, List<DateTime> splitDates)

Splits a task by its GUID ID.

**Parameters:**
- `id` (`Guid`) — Task ID to split
- `splitDates` (`List<DateTime>`) — List of dates where task should be split

**Example:**
```csharp
await GanttInstance.SplitTaskAsync(
    Guid.Parse("8B23A701-4E27-4A77-A3C7-F1B14D7F7D72"), 
    new List<DateTime> { new DateTime(2024, 1, 15) }
);
```

---

## Merge Task Methods

### MergeTaskAsync(int id, List<(int, int)> segmentIndexes)

Merges specific segments of a task by integer ID.

**Parameters:**
- `id` (`int`) — Task ID containing segments to merge
- `segmentIndexes` (`List<(int, int)>`) — List of segment index pairs to merge (from index, to index)

**Returns:** `Task`

**Example:**
```csharp
<button @onclick="MergeSegments">Merge Segments 0-1</button>

@code {
    private async Task MergeSegments()
    {
        // Merge segments at indexes 0 and 1 of task 3
        var indexes = new List<(int, int)>
        {
            (0, 1)  // Merge first two segments
        };
        
        await GanttInstance.MergeTaskAsync(3, indexes);
    }
}
```

---

### MergeTaskAsync(string id, List<(int, int)> segmentIndexes)

Merges segments by string task ID.

**Example:**
```csharp
await GanttInstance.MergeTaskAsync("TASK-003", new List<(int, int)>
{
    (0, 1),  // Merge first and second segment
    (2, 3)   // Merge third and fourth segment
});
```

---

### MergeTaskAsync(Guid id, List<(int, int)> segmentIndexes)

Merges segments by GUID task ID.

**Example:**
```csharp
await GanttInstance.MergeTaskAsync(
    Guid.Parse("D1B8DCC4-C9F3-4E70-85F0-0FE7243F83C1"),
    new List<(int, int)> { (0, 2) }  // Merge segments 0, 1, and 2
);
```

---

## UI-Based Operations

### Context Menu Split/Merge

**Enable context menu:**
```csharp
<SfGantt EnableContextMenu="true" ...>
</SfGantt>
```

**Built-in items:**
- **"Split Task"** — Opens split dialog for selected task
- **"Merge Task"** — Merges adjacent segments by dragging them together

**Right-click behavior:**
1. Select a task row
2. Right-click to open context menu
3. Click "Split Task" to open split dialog
4. Click "Merge Task" to combine segments

---

### Dialog-Based Splitting

When `EnableContextMenu="true"` and edit mode is Dialog, selecting "Split Task" from context menu opens a dialog:

**Dialog Features:**
- Visual segment timeline
- Add split points by clicking timeline
- Remove split points
- Adjust segment dates
- Preview segment layout

**Requirements:**
- `GanttEditSettings.AllowEditing="true"`
- Valid `GanttTaskFields` mappings
- Task must not be parent or milestone
- Task must have sufficient width

---

### Taskbar Dragging for Merging

**Enable:**
```csharp
<GanttEditSettings AllowTaskbarEditing="true">
</GanttEditSettings>
```

**Usage:**
1. Click and drag a segment taskbar
2. Drop it adjacent to another segment
3. Segments automatically merge if touching

---

## Segment Visualization

### Taskbar Rendering

Segments are rendered as separate taskbars with visual gaps:

```
Task: Design Phase (Split)
┌─────────┐     ┌─────────┐     ┌───────────┐
│ Segment │ GAP │ Segment │ GAP │ Segment   │
│   1     │     │   2     │     │    3      │
└─────────┘     └─────────┘     └───────────┘
Jan 15-18      Jan 22-25       Jan 29-Feb 2
```

### Custom Segment Styling

```razor
<GanttTemplates TValue="TaskData">
    <TaskbarTemplate>
        @{
            var task = context as TaskData;
            <div class="custom-segment-taskbar">
                @task.TaskName
            </div>
        }
    </TaskbarTemplate>
</GanttTemplates>

<style>
    .custom-segment-taskbar {
        background: linear-gradient(to right, #4CAF50, #81C784);
        border: 2px solid #2E7D32;
        border-radius: 4px;
    }
</style>
```

---

## Common Patterns

### Pattern 1: Load Segments at Initialization

```csharp
protected override void OnInitialized()
{
    TaskCollection = GetTasks();
    SegmentCollection = GetInitialSegments();
}

private List<SegmentModel> GetInitialSegments()
{
    return new List<SegmentModel>
    {
        // Pre-defined split for task 5
        new SegmentModel { 
            Id = 1, 
            TaskID = 5, 
            SegmentStartDate = new DateTime(2024, 1, 10),
            SegmentDuration = "2"
        },
        new SegmentModel { 
            Id = 2, 
            TaskID = 5, 
            SegmentStartDate = new DateTime(2024, 1, 15),
            SegmentDuration = "3"
        }
    };
}
```

---

### Pattern 2: Dynamic Splitting Based on Conditions

```csharp
private async Task SplitIfLongTask()
{
    var longTasks = TaskCollection.Where(t => 
        int.Parse(t.Duration) > 10
    ).ToList();
    
    foreach (var task in longTasks)
    {
        // Split long tasks into 2 equal segments
        var midpoint = task.StartDate.Value.AddDays(
            int.Parse(task.Duration) / 2
        );
        
        await GanttInstance.SplitTaskAsync(task.TaskID, new List<DateTime> { midpoint });
    }
}
```

---

### Pattern 3: Merge All Segments of a Task

```csharp
private async Task MergeAllSegments(int taskId)
{
    var segments = SegmentCollection.Where(s => s.TaskID == taskId).ToList();
    
    if (segments.Count > 1)
    {
        // Merge all segments: 0→1, 1→2, 2→3, etc.
        var mergePairs = new List<(int, int)>();
        for (int i = 0; i < segments.Count - 1; i++)
        {
            mergePairs.Add((i, i + 1));
        }
        
        await GanttInstance.MergeTaskAsync(taskId, mergePairs);
    }
}
```

---

### Pattern 4: Split on Weekends

```csharp
private async Task SplitOnWeekends(int taskId)
{
    var task = TaskCollection.First(t => t.TaskID == taskId);
    var splitDates = new List<DateTime>();
    
    var currentDate = task.StartDate.Value;
    var endDate = task.EndDate.Value;
    
    while (currentDate <= endDate)
    {
        // Split before weekends
        if (currentDate.DayOfWeek == DayOfWeek.Friday)
        {
            splitDates.Add(currentDate.AddDays(1)); // Split at Saturday
        }
        currentDate = currentDate.AddDays(1);
    }
    
    if (splitDates.Count > 0)
    {
        await GanttInstance.SplitTaskAsync(taskId, splitDates);
    }
}
```

---

## Best Practices

### 1. Segment Ordering

Ensure segments are chronologically ordered in data source:

```csharp
private List<SegmentModel> GetOrderedSegments()
{
    return SegmentCollection
        .OrderBy(s => s.TaskID)
        .ThenBy(s => s.SegmentStartDate)
        .ToList();
}
```

---

### 2. Validate Segment Boundaries

Segments must fit within task start and end dates:

```csharp
private bool ValidateSegment(SegmentModel segment, TaskData task)
{
    return segment.SegmentStartDate >= task.StartDate &&
           segment.SegmentEndDate <= task.EndDate;
}
```

---

### 3. Avoid Overlapping Segments

```csharp
private bool HasOverlappingSegments(int taskId)
{
    var segments = SegmentCollection
        .Where(s => s.TaskID == taskId)
        .OrderBy(s => s.SegmentStartDate)
        .ToList();
    
    for (int i = 0; i < segments.Count - 1; i++)
    {
        if (segments[i].SegmentEndDate >= segments[i + 1].SegmentStartDate)
        {
            return true;  // Overlapping detected
        }
    }
    return false;
}
```

---

### 4. Timeline Unit Considerations

Tasks must have sufficient width relative to timeline unit:

```csharp
<GanttTimelineSettings TimelineUnitSize="60">
    <GanttTopTierSettings Unit="TimelineViewMode.Week"></GanttTopTierSettings>
    <GanttBottomTierSettings Unit="TimelineViewMode.Day"></GanttBottomTierSettings>
</GanttTimelineSettings>
```

Smaller timeline units (hours) provide better segment visibility.

---

## Troubleshooting

### Issue: Segments Not Displaying

**Possible Causes:**
1. Missing `GanttSegmentFields` component
2. Incorrect foreign key mapping
3. Segments outside task boundaries

**Solution:**
```csharp
// Verify foreign key relationship
<GanttSegmentFields 
    ForeignKey="TaskID"  // Must match GanttTaskFields.Id
    ...>
</GanttSegmentFields>

// Check segment dates
foreach (var segment in SegmentCollection)
{
    var task = TaskCollection.First(t => t.TaskID == segment.TaskID);
    if (segment.SegmentStartDate < task.StartDate || 
        segment.SegmentEndDate > task.EndDate)
    {
        Console.WriteLine($"Segment {segment.Id} outside task {task.TaskID} boundaries");
    }
}
```

---

### Issue: Split Task Option Disabled in Context Menu

**Causes:**
- Task is a parent task
- Task is a milestone
- Task width too narrow
- `EnableContextMenu` not set

**Solution:**
```csharp
<SfGantt EnableContextMenu="true" ...>
    <GanttEditSettings AllowEditing="true" AllowTaskbarEditing="true">
    </GanttEditSettings>
</SfGantt>

// Ensure task is not parent or milestone
var task = TaskCollection.First(t => t.TaskID == 5);
bool canSplit = task.ParentID == null &&  // Not a milestone
                !TaskCollection.Any(t => t.ParentID == task.TaskID);  // Not a parent
```

---

### Issue: Merge Not Working

**Causes:**
- Invalid segment indexes
- Non-adjacent segments
- Incorrect segment index pairs

**Solution:**
```csharp
// Get segment count for task
var segmentCount = SegmentCollection.Count(s => s.TaskID == taskId);

// Valid merge: (0, 1) merges first two segments
// Invalid merge: (0, 2) if segments not adjacent

// Merge adjacent segments only
await GanttInstance.MergeTaskAsync(taskId, new List<(int, int)>
{
    (0, 1)  // ✓ Correct: merge segments 0 and 1
});
```

---

### Issue: Segment Dates Not Updating

**Cause:** Binding issue with data source

**Solution:**
```csharp
// Ensure collection is observable or manually refresh
await GanttInstance.RefreshAsync();
```

---

## Limitations

1. **Cannot split parent tasks** — Only child/leaf tasks can be split
2. **Cannot split milestone tasks** — Milestones have zero duration
3. **Incompatible with multi-taskbar** — Avoid using split tasks with `EnableMultiTaskbar`
4. **Minimum segment width required** — Very short segments may not display properly
5. **Timeline unit dependency** — Hour-level segments require hour timeline unit

---

