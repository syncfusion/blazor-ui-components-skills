# Markers and Labels

## Table of Contents
- [When to Read](#when-to-read)
- [Event Markers](#event-markers)
- [Event Marker Properties](#event-marker-properties)
- [Label Configuration](#label-configuration)
- [Label Templates](#label-templates)
- [Data Markers](#data-markers)
- [Complete Example](#complete-example)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## When to Read

Use this reference when you need to:
- Add vertical event markers (deadlines, milestones, sprints) to the timeline
- Display left, right, and bottom labels on taskbars
- Render custom HTML/components inside taskbar labels
- Mark individual tasks with data markers

---

## Event Markers

Event markers are vertical lines drawn across the chart panel at specific dates. Use them to highlight dates like project deadlines, sprint boundaries, or holidays.

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt DataSource="@TaskCollection" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" ParentID="ParentID" />
    <GanttEventMarkers>
        <GanttEventMarker Day="@(new DateTime(2024, 04, 10))"
                          Label="Sprint 1 End"
                          CssClass="sprint-marker" />
        <GanttEventMarker Day="@(new DateTime(2024, 04, 25))"
                          Label="Project Deadline"
                          CssClass="deadline-marker" />
        <GanttEventMarker Day="@DateTime.Today"
                          Label="Today" />
    </GanttEventMarkers>
</SfGantt>

<style>
    .sprint-marker   .e-gantt-event-marker { border-left: 2px dashed #4e73df; }
    .deadline-marker .e-gantt-event-marker { border-left: 2px solid #e74a3b; }
</style>
```

---

## Event Marker Properties

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `Day` | `DateTime` | ✅ | The date where the marker line is drawn |
| `Label` | `string` | | Text label shown above the marker line |
| `CssClass` | `string` | | CSS class applied to the marker element for custom styling |

---

## Label Configuration

Gantt supports three label positions relative to each taskbar: left, right, and bottom.

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID" />
    <GanttLabelSettings LeftLabel="TaskID"
                        RightLabel="Progress"
                        TValue="TaskData" />
</SfGantt>
```

**Label binding accepts:**
- A **field name string** from the data model (e.g. `"TaskName"`, `"Progress"`)
- A **template** (see Label Templates below)

### Label Positions

| Position | Property | Renders |
|----------|----------|---------|
| Left | `LeftLabel` | To the left of the taskbar |
| Right | `RightLabel` | To the right of the taskbar |
| Bottom | `BottomLabel` | Below the taskbar |

---

## Label Templates

Use `RenderFragment` templates inside `GanttLabelSettings` for custom label content:

```razor
<GanttLabelSettings TValue="TaskData">
    <LeftLabelTemplate>
        @{
            var task = context as TaskData;
        }
        <div style="font-size:11px; color:#555; white-space:nowrap;">
            #@task.TaskID
        </div>
    </LeftLabelTemplate>

    <RightLabelTemplate>
        @{
            var task = context as TaskData;
        }
        <div style="font-size:11px; color:#4e73df; white-space:nowrap;">
            @task.Progress%
        </div>
    </RightLabelTemplate>

    <BottomLabelTemplate>
        @{
            var task = context as TaskData;
        }
        @if (!string.IsNullOrEmpty(task.AssignedTo))
        {
            <div style="font-size:10px; color:#999;">@task.AssignedTo</div>
        }
    </BottomLabelTemplate>
</GanttLabelSettings>
```

> **Note:** Templates and string binding are mutually exclusive per position. If you use a template, do not set the string property for the same position.

---

## Data Markers

Data markers are note indicators displayed on individual task rows in the chart panel. They are configured per-task through the `Note` field mapping.

```razor
<GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                 Duration="Duration" Notes="Notes" ParentID="ParentID" />
```

The `Notes` field maps to a string property in your data model. When non-empty, a marker icon appears in the chart row. The content is shown in the edit dialog's Notes tab.

```csharp
public class TaskData
{
    public int TaskID { get; set; }
    public string TaskName { get; set; }
    public DateTime StartDate { get; set; }
    public string Duration { get; set; }
    public int? ParentID { get; set; }
    public string Notes { get; set; }  // Data marker note content
}
```

---

## Complete Example

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt DataSource="@TaskCollection" Height="500px" Width="1000px">
    <GanttTaskFields Id="TaskID" Name="TaskName"
                     StartDate="StartDate" Duration="Duration"
                     Progress="Progress" ParentID="ParentID"
                     Notes="Notes" />

    <!-- Event Markers -->
    <GanttEventMarkers>
        <GanttEventMarker Day="@(new DateTime(2024, 04, 15))"
                          Label="Code Freeze"
                          CssClass="freeze-marker" />
        <GanttEventMarker Day="@(new DateTime(2024, 04, 30))"
                          Label="Release"
                          CssClass="release-marker" />
    </GanttEventMarkers>

    <!-- Labels with templates -->
    <GanttLabelSettings TValue="TaskData">
        <RightLabelTemplate>
            @{
                var task = context as TaskData;
            }
            <span style="font-size:11px; color:#4e73df;">
                @task.Progress%
            </span>
        </RightLabelTemplate>
        <BottomLabelTemplate>
            @{
                var task = context as TaskData;
            }
            @if (!string.IsNullOrEmpty(task.Owner))
            {
                <span style="font-size:10px; color:#888;">@task.Owner</span>
            }
        </BottomLabelTemplate>
    </GanttLabelSettings>
</SfGantt>

<style>
    .freeze-marker  .e-gantt-event-marker { border-left: 2px dashed #f6c23e; }
    .release-marker .e-gantt-event-marker { border-left: 3px solid #1cc88a; }
</style>

@code {
    private List<TaskData> TaskCollection = new()
    {
        new TaskData { TaskID = 1, TaskName = "Project", StartDate = new DateTime(2024, 04, 01) },
        new TaskData { TaskID = 2, TaskName = "Design",   Duration = "5",  Progress = 90, ParentID = 1, StartDate = new DateTime(2024, 04, 01), Owner = "Alice" },
        new TaskData { TaskID = 3, TaskName = "Build",    Duration = "10", Progress = 40, ParentID = 1, StartDate = new DateTime(2024, 04, 08), Owner = "Bob", Notes = "API integration in progress" },
        new TaskData { TaskID = 4, TaskName = "Testing",  Duration = "4",  Progress = 10, ParentID = 1, StartDate = new DateTime(2024, 04, 22), Owner = "Carol" }
    };

    public class TaskData
    {
        public int TaskID { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public string Duration { get; set; }
        public int Progress { get; set; }
        public int? ParentID { get; set; }
        public string Owner { get; set; }
        public string Notes { get; set; }
    }
}
```

---

## Best Practices

- **Keep label text short** — long labels overlap adjacent taskbars; prefer `RightLabel` for progress only
- **Use `CssClass` on event markers** — allows per-marker styling without affecting all markers
- **Use `BottomLabel` sparingly** — it increases row height and may affect large datasets
- **Avoid templates for simple field display** — the string `LeftLabel="TaskName"` is more performant
- **Use `Notes` + data markers for task metadata** — avoids cluttering label areas with secondary info

---

## Troubleshooting

**Issue**: Event markers not rendering  
**Solution**: Verify the `Day` date falls within the Gantt's visible date range. Markers outside `ViewStartDate`/`ViewEndDate` won't show.

**Issue**: Label template shows `null` reference  
**Solution**: Cast `context` safely: `var task = context as TaskData;` and add null check `@if (task != null)`.

**Issue**: Both string property and template set for same label position  
**Solution**: Use one or the other. Setting both is undefined behavior — prefer template if custom rendering is needed.

**Issue**: `Notes` field doesn't show marker icon  
**Solution**: Ensure `Notes="Notes"` is mapped in `GanttTaskFields` and the data model property is non-null/non-empty.

---
