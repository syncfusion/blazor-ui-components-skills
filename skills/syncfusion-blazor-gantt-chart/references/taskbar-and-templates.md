# Taskbar Templates and Customization

## Table of Contents
- [When to Read](#when-to-read)
- [Task Rendering Overview](#task-rendering-overview)
- [TaskbarTemplate](#taskbartemplate)
- [ParentTaskbarTemplate](#parenttaskbartemplate)
- [MilestoneTemplate](#milestonetemplate)
- [Taskbar Corner Radius](#taskbar-corner-radius)
- [Baseline Display](#baseline-display)
- [QueryChartRowInfo Event](#querychartrowinfo-event)
- [Conditional Taskbar Styling](#conditional-taskbar-styling)
- [Complete Example](#complete-example)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## When to Read

Use this reference when you need to:
- Render custom HTML/Blazor content inside taskbars
- Apply separate templates for parent and milestone tasks
- Display progress, icons, or labels inside taskbars
- Show baseline bars for schedule comparison
- Conditionally style taskbars using `QueryChartRowInfo`
- Adjust taskbar corner rounding or appearance

---

## Task Rendering Overview

The Gantt chart renders three types of chart elements, each with its own template:

| Element | Template Property | Applicable When |
|---------|-------------------|-----------------|
| Regular task | `TaskbarTemplate` | Non-parent, non-milestone tasks |
| Parent/summary task | `ParentTaskbarTemplate` | Tasks with child rows |
| Milestone | `MilestoneTemplate` | Tasks with zero or null duration |

Templates provide a `context` variable of type `GanttTemplateContext` (the underlying data row).

---

## TaskbarTemplate

Customize the rendering of regular task bars:

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID" />
    <GanttTemplates TValue="TaskData">
        <TaskbarTemplate>
            @{
                var task = context as TaskData;
            }
            <div class="custom-taskbar" style="height:100%; background:#4e73df; border-radius:4px; display:flex; align-items:center; padding:0 8px;">
                <span style="color:white; font-size:11px; overflow:hidden; text-overflow:ellipsis; white-space:nowrap;">
                    @task.TaskName
                </span>
                <span style="color:white; font-size:10px; margin-left:auto;">
                    @task.Progress%
                </span>
            </div>
        </TaskbarTemplate>
    </GanttTemplates>
</SfGantt>
```

### Context Object

Inside `TaskbarTemplate`, `context` is the data row object (your `TValue` model). Cast it:

```razor
@{
    var task = context as TaskData;  // or (TaskData)context
}
```

---

## ParentTaskbarTemplate

Customize the summary/parent task bar appearance:

```razor
<GanttTemplates TValue="TaskData">
    <ParentTaskbarTemplate>
        @{
            var task = context as TaskData;
        }
        <div class="parent-taskbar" style="height:100%; background:#1cc88a; border-radius:2px; display:flex; align-items:center; padding:0 8px;">
            <i class="e-icons e-folder" style="color:white; margin-right:4px;"></i>
            <span style="color:white; font-size:11px; font-weight:bold;">
                @task.TaskName
            </span>
        </div>
    </ParentTaskbarTemplate>
</GanttTemplates>
```

---

## MilestoneTemplate

Customize milestone rendering (tasks with zero/null duration):

```razor
<GanttTemplates TValue="TaskData">
    <MilestoneTemplate>
        @{
            var task = context as TaskData;
        }
        <div style="width:20px; height:20px; background:#e74a3b; transform:rotate(45deg); margin-top:-2px;" title="@task.TaskName">
        </div>
    </MilestoneTemplate>
</GanttTemplates>
```

---

## Taskbar Corner Radius

Control the rounding of taskbar corners using CSS or the built-in `TaskbarCornerRadius` property:

```razor
<SfGantt DataSource="@TaskCollection"
         TaskbarCornerRadius="4">
</SfGantt>
```

---

## Baseline Display

Show baseline bars to compare planned vs. actual dates:

```razor
<SfGantt DataSource="@TaskCollection"
         RenderBaseline="true"
         BaselineColor="#ff7f50"
         Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName"
                     StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress"
                     BaselineStartDate="BaselineStartDate"
                     BaselineEndDate="BaselineEndDate"
                     ParentID="ParentID" />
</SfGantt>
```

**Baseline properties on GanttTaskFields:**

| Property | Description |
|----------|-------------|
| `BaselineStartDate` | Property name for planned start date |
| `BaselineEndDate` | Property name for planned end date |

**Gantt baseline properties:**

| Property | Type | Description |
|----------|------|-------------|
| `RenderBaseline` | `bool` | Show baseline bars |
| `BaselineColor` | `string` | CSS color for baseline bar |

**Data model with baselines:**

```csharp
public class TaskData
{
    public int TaskID { get; set; }
    public string TaskName { get; set; }
    public DateTime StartDate { get; set; }
    public DateTime EndDate { get; set; }
    public string Duration { get; set; }
    public int Progress { get; set; }
    public int? ParentID { get; set; }
    public DateTime BaselineStartDate { get; set; }  // planned start
    public DateTime BaselineEndDate { get; set; }    // planned end
}
```

---

## QueryChartRowInfo Event

Dynamically customize taskbar color/style based on data conditions:

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID" />
    <GanttEvents TValue="TaskData" QueryChartRowInfo="ChartRowInfoHandler" />
</SfGantt>

@code {
    public void ChartRowInfoHandler(QueryChartRowInfoEventArgs<TaskData> args)
    {
        if (args.Data.Progress < 30)
        {
            args.Row.AddClass(new string[] { "low-progress" });
        }
        else if (args.Data.Progress >= 80)
        {
            args.Row.AddClass(new string[] { "high-progress" });
        }
    }
}
```

Add CSS to style the custom classes:

```css
.low-progress  .e-taskbar-main-container { background-color: #e74a3b !important; }
.high-progress .e-taskbar-main-container { background-color: #1cc88a !important; }
```

### QueryChartRowInfoEventArgs Properties

| Property | Type | Description |
|----------|------|-------------|
| `Data` | `TValue` | The data record for this row |
| `Row` | `DOM` | The row DOM element (supports `AddClass`) |

---

## Conditional Taskbar Styling

### Using CSS Class Binding

You can conditionally apply classes directly in the template:

```razor
<GanttTemplates TValue="TaskData">
    <TaskbarTemplate>
        @{
            var task = context as TaskData;
            var color = task.Progress < 30 ? "#e74a3b"
                      : task.Progress < 70 ? "#f6c23e"
                      : "#1cc88a";
        }
        <div style="height:100%; background:@color; border-radius:4px;
                    display:flex; align-items:center; padding:0 6px;">
            <span style="color:white; font-size:11px; white-space:nowrap; overflow:hidden;">
                @task.TaskName (@task.Progress%)
            </span>
        </div>
    </TaskbarTemplate>
</GanttTemplates>
```

---

## Complete Example

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt @ref="GanttRef"
         DataSource="@TaskCollection"
         Height="500px"
         Width="1000px"
         RenderBaseline="true"
         BaselineColor="#ff7f50">
    <GanttTaskFields Id="TaskID" Name="TaskName"
                     StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress"
                     ParentID="ParentID"
                     BaselineStartDate="BaselineStartDate"
                     BaselineEndDate="BaselineEndDate" />
    <GanttEditSettings AllowTaskbarEditing="true" />
    <GanttTemplates TValue="TaskData">
        <TaskbarTemplate>
            @{
                var task = context as TaskData;
                var bg = task.Progress < 50 ? "#f6c23e" : "#4e73df";
            }
            <div style="height:100%; background:@bg; border-radius:4px;
                        display:flex; align-items:center; padding:0 8px; overflow:hidden;">
                <span style="color:white; font-size:11px; white-space:nowrap;">
                    @task.TaskName
                </span>
                <span style="color:white; font-size:10px; margin-left:auto;">
                    @task.Progress%
                </span>
            </div>
        </TaskbarTemplate>
        <ParentTaskbarTemplate>
            @{
                var task = context as TaskData;
            }
            <div style="height:100%; background:#1cc88a; border-radius:3px;
                        display:flex; align-items:center; padding:0 8px;">
                <span style="color:white; font-weight:bold; font-size:11px;">
                    ▶ @task.TaskName
                </span>
            </div>
        </ParentTaskbarTemplate>
        <MilestoneTemplate>
            @{
                var task = context as TaskData;
            }
            <div style="width:18px; height:18px; background:#e74a3b;
                        transform:rotate(45deg);" title="@task.TaskName">
            </div>
        </MilestoneTemplate>
    </GanttTemplates>
</SfGantt>

@code {
    private SfGantt<TaskData> GanttRef;
    private List<TaskData> TaskCollection { get; set; }

    protected override void OnInitialized()
    {
        TaskCollection = new List<TaskData>()
        {
            new TaskData { TaskID = 1, TaskName = "Project Alpha", StartDate = new DateTime(2024, 04, 01) },
            new TaskData { TaskID = 2, TaskName = "Design Phase", StartDate = new DateTime(2024, 04, 01), Duration = "5", Progress = 80, ParentID = 1,
                           BaselineStartDate = new DateTime(2024, 04, 01), BaselineEndDate = new DateTime(2024, 04, 07) },
            new TaskData { TaskID = 3, TaskName = "Development",   StartDate = new DateTime(2024, 04, 08), Duration = "8", Progress = 30, ParentID = 1,
                           BaselineStartDate = new DateTime(2024, 04, 06), BaselineEndDate = new DateTime(2024, 04, 15) },
            new TaskData { TaskID = 4, TaskName = "Milestone",     StartDate = new DateTime(2024, 04, 20), Duration = "0", ParentID = 1 }
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
        public DateTime BaselineStartDate { get; set; }
        public DateTime BaselineEndDate { get; set; }
    }
}
```

---

## Best Practices

- **Keep template content lightweight** — complex Blazor components inside taskbars may slow rendering for large datasets
- **Use `overflow:hidden` on template containers** — prevents content spill outside the taskbar bounds
- **Prefer `QueryChartRowInfo` for color-only changes** — it is more performant than custom templates
- **Test baselines with varied schedules** — baseline bars render behind the actual taskbar; ensure baseline dates are set on all leaf tasks
- **Use `title` attribute on milestone templates** — milestone shapes are small; a tooltip helps identify them

---

## Troubleshooting

**Issue**: Template is not rendering  
**Solution**: Wrap all three template types in a single `<GanttTemplates TValue="...">` parent component.

**Issue**: `context` cast throws NullReferenceException  
**Solution**: Use `context as TaskData` (null-safe) and add a null check: `@if (task != null) { ... }`

**Issue**: Baseline bars not visible  
**Solution**: Confirm `RenderBaseline="true"` is set AND `BaselineStartDate`/`BaselineEndDate` are mapped in `GanttTaskFields` AND the data records have non-default date values.

**Issue**: `QueryChartRowInfo` AddClass not applying styles  
**Solution**: Ensure CSS specificity is high enough. Use `!important` on the target Gantt CSS class or move styles to a component-scoped stylesheet.

---
