# Virtualization and Performance

## Table of Contents
- [When to Read](#when-to-read)
- [Row Virtualization](#row-virtualization)
- [Column Virtualization](#column-virtualization)
- [Timeline Virtualization](#timeline-virtualization)
- [On-Demand Child Loading](#on-demand-child-loading)
- [OverscanCount](#overscancount)
- [PageSize](#pagesize)
- [WebAssembly Performance Tips](#webassembly-performance-tips)
- [Blazor Server Performance Tips](#blazor-server-performance-tips)
- [Complete Example](#complete-example)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## When to Read

Use this reference when you need to:
- Render large datasets (1 000+ tasks) without performance degradation
- Enable row and/or column virtualization
- Load child tasks on demand from the server
- Tune pre-rendering buffer sizes (`OverscanCount`, `PageSize`)
- Apply Blazor-specific performance patterns for WebAssembly and Server hosting

---

## Row Virtualization

Row virtualization renders only the visible rows in the DOM, discarding rows that scroll out of view. This is critical for datasets with thousands of tasks.

```razor
<SfGantt DataSource="@TaskCollection"
         Height="450px" Width="900px"
         EnableRowVirtualization="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" ParentID="ParentID" />
</SfGantt>
```

**Requirements:**
- `Height` must be set to a fixed pixel value (not `"auto"` or `"100%"`)
- The component must be able to compute viewport height to determine how many rows to render

---

## Column Virtualization

Column virtualization renders only the visible columns in the grid panel. Useful when the Gantt has many custom columns.

```razor
<SfGantt DataSource="@TaskCollection"
         Height="450px" Width="900px"
         EnableRowVirtualization="true"
         EnableColumnVirtualization="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" ParentID="ParentID" />
</SfGantt>
```

**Note:** Column virtualization only affects the grid (left) panel columns, not the chart area.

---

## Timeline Virtualization

Timeline virtualization renders only the visible date range of the chart panel:

```razor
<SfGantt DataSource="@TaskCollection"
         Height="450px" Width="900px"
         EnableTimelineVirtualization="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" ParentID="ParentID" />
</SfGantt>
```

Particularly useful for projects spanning many months or years at a Day-level timeline, where the total chart width would otherwise be enormous.

---

## On-Demand Child Loading

For very large hierarchical datasets, load child tasks from the server only when the parent is expanded. This avoids loading the entire tree upfront.

### Setup

1. Map a `HasChildMapping` field that signals whether a task has children
2. Enable `LoadChildOnDemand="true"`
3. Use a `SfDataManager` with `UrlAdaptor` pointing to your server

```razor
<SfGantt TValue="TaskData"
         Height="450px" Width="900px"
         LoadChildOnDemand="true">
    <SfDataManager Url="api/gantt" Adaptor="Adaptors.UrlAdaptor" />
    <GanttTaskFields Id="TaskID" Name="TaskName"
                     StartDate="StartDate" Duration="Duration"
                     HasChildMapping="IsParent"
                     ParentID="ParentID" />
</SfGantt>
```

Data model with `HasChildMapping`:

```csharp
public class TaskData
{
    public int TaskID { get; set; }
    public string TaskName { get; set; }
    public DateTime StartDate { get; set; }
    public string Duration { get; set; }
    public int? ParentID { get; set; }
    public bool IsParent { get; set; }  // true = has children, false = leaf node
}
```

### Server-Side Handler

The server receives an expand request with the parent row's ID:

```csharp
[HttpPost]
public IActionResult GetGanttData([FromBody] DataManagerRequest dm)
{
    var data = _service.GetTasksForRequest(dm);
    return Json(new { result = data.Items, count = data.Count });
}
```

---

## OverscanCount

`OverscanCount` controls how many extra rows are pre-rendered beyond the visible viewport. A higher value reduces blank rows during fast scrolling but increases DOM size.

```razor
<SfGantt DataSource="@TaskCollection"
         Height="450px" Width="900px"
         EnableRowVirtualization="true"
         OverscanCount="5">
</SfGantt>
```

- Default: `3`
- Recommended range: `3`–`10`
- Higher values improve scroll smoothness; lower values reduce memory

---

## PageSize

`PageSize` controls how many rows are rendered per virtual page:

```razor
<SfGantt DataSource="@TaskCollection"
         Height="450px" Width="900px"
         EnableRowVirtualization="true"
         PageSize="50">
</SfGantt>
```

- Default: computed from viewport height and `RowHeight`
- Set explicitly when you need predictable page sizes for remote loading

---

## WebAssembly Performance Tips

| Pattern | Guidance |
|---------|----------|
| Lazy loading | Use `LoadChildOnDemand` + UrlAdaptor to avoid loading full dataset into WASM memory |
| Avoid large `@code` bindings | Keep `DataSource` lists small — WASM serializes entire list on each render cycle |
| Use `ShouldRender` override | Prevent unnecessary re-renders in parent components |
| Avoid synchronous HttpClient | Use `await httpClient.GetFromJsonAsync<>()` to not block the WASM render thread |
| AOT compilation | Publish with `<RunAOTCompilation>true</RunAOTCompilation>` for faster execution |
| Enable compression | Ensure the server serves `.wasm` files with Brotli compression |

---

## Blazor Server Performance Tips

| Pattern | Guidance |
|---------|----------|
| Circuit scalability | Each Blazor Server connection holds state; enable row virtualization to reduce re-render payload |
| Minimize SignalR message size | Use `EnableRowVirtualization` + `PageSize` to reduce data sent per scroll event |
| Server-side filtering | Apply filtering at the EF Core level before sending data to the Gantt |
| Async state updates | Use `InvokeAsync(StateHasChanged)` for background-thread UI updates |
| Connection resilience | Configure `CircuitOptions.DisconnectedCircuitMaxRetained` in production |

---

## Complete Example

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Data

<!-- High-performance Gantt: row + column + timeline virtualization, on-demand child loading -->
<SfGantt TValue="TaskData"
         Height="500px"
         Width="1200px"
         EnableRowVirtualization="true"
         EnableColumnVirtualization="true"
         EnableTimelineVirtualization="true"
         LoadChildOnDemand="true"
         OverscanCount="5"
         PageSize="40">

    <SfDataManager Url="api/gantt" Adaptor="Adaptors.UrlAdaptor" />

    <GanttTaskFields Id="TaskID" Name="TaskName"
                     StartDate="StartDate" Duration="Duration"
                     Progress="Progress" ParentID="ParentID"
                     HasChildMapping="IsParent" />

    <GanttColumns>
        <GanttColumn Field="TaskID"   Width="70" />
        <GanttColumn Field="TaskName" HeaderText="Task Name" Width="200" />
        <GanttColumn Field="StartDate" Width="110" />
        <GanttColumn Field="Duration"  Width="90" />
        <GanttColumn Field="Progress"  Width="90" />
    </GanttColumns>

    <GanttTimelineSettings TimelineUnitSize="40">
        <GanttTopTierSettings  Unit="TimelineViewMode.Month" Format="MMMM yyyy" />
        <GanttBottomTierSettings Unit="TimelineViewMode.Day"  Format="d" />
    </GanttTimelineSettings>
</SfGantt>
```

For local dataset virtualization (no server):

```razor
<SfGantt DataSource="@LargeTaskList"
         Height="500px" Width="1000px"
         EnableRowVirtualization="true"
         OverscanCount="4">
    <GanttTaskFields Id="TaskID" Name="TaskName"
                     StartDate="StartDate" Duration="Duration"
                     ParentID="ParentID" />
</SfGantt>

@code {
    private List<TaskData> LargeTaskList { get; set; }

    protected override void OnInitialized()
    {
        LargeTaskList = GenerateTasks(2000);  // 2 000 rows
    }

    private static List<TaskData> GenerateTasks(int count)
    {
        var list = new List<TaskData>();
        for (int i = 1; i <= count; i++)
        {
            list.Add(new TaskData
            {
                TaskID = i,
                TaskName = $"Task {i}",
                StartDate = new DateTime(2024, 01, 01).AddDays(i % 180),
                Duration = $"{(i % 5) + 1}",
                ParentID = i > 1 ? (i % 50 == 0 ? (int?)null : i - 1) : null
            });
        }
        return list;
    }

    public class TaskData
    {
        public int TaskID { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public string Duration { get; set; }
        public int? ParentID { get; set; }
        public bool IsParent { get; set; }
    }
}
```

---

## Best Practices

- **Always set a fixed pixel `Height`** when using `EnableRowVirtualization` — percentage heights prevent viewport calculation
- **Combine row + timeline virtualization** for large projects spanning many months
- **Use `LoadChildOnDemand` with remote data only** — local tree data should be fully loaded; on-demand is for server-side trees
- **Keep `OverscanCount` between 3–8** — higher values help scroll smoothness but increase DOM node count
- **Avoid `StateHasChanged` inside tight loops** — batch updates and call StateHasChanged once
- **Profile before optimizing** — measure render time with browser DevTools before enabling all virtualization flags

---

## Troubleshooting

**Issue**: Rows disappear when scrolling fast  
**Solution**: Increase `OverscanCount` (e.g. from 3 to 8) to pre-render more rows beyond the viewport.

**Issue**: Row virtualization appears disabled (all rows visible)  
**Solution**: Confirm `Height` is set to a fixed pixel value like `"500px"`. Dynamic/percentage heights cause the component to render all rows.

**Issue**: `LoadChildOnDemand` doesn't request children from server  
**Solution**: Ensure `HasChildMapping` is mapped and the data model's flag is `true` for parent rows. Also verify `SfDataManager` uses `UrlAdaptor`.

**Issue**: Timeline feels sluggish on large date ranges  
**Solution**: Enable `EnableTimelineVirtualization="true"` to only render the visible date columns.

**Issue**: Column virtualization hides pinned columns  
**Solution**: Column virtualization does not affect frozen/pinned columns — they are always rendered regardless.

---
