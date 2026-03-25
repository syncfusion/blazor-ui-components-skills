# Zooming and View Control

## Table of Contents
- [When to Read](#when-to-read)
- [Zoom Toolbar Buttons](#zoom-toolbar-buttons)
- [Programmatic Zoom API](#programmatic-zoom-api)
- [Custom Zooming Levels](#custom-zooming-levels)
- [ScrollToTimelineAsync](#scrolltotimelineasync)
- [FitToProject](#fittoproject)
- [Complete Example](#complete-example)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## When to Read

Use this reference when you need to:
- Add zoom-in/out/fit toolbar buttons
- Programmatically zoom to a specific level or date range
- Define custom zooming presets
- Scroll the chart viewport to a specific date

---

## Zoom Toolbar Buttons

Add built-in zoom toolbar items using string identifiers:

```razor
<SfGantt DataSource="@TaskCollection"
         Height="450px" Width="900px"
         Toolbar="@(new List<string>() { "ZoomIn", "ZoomOut", "ZoomToFit" })">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" ParentID="ParentID" />
</SfGantt>
```

| Toolbar Item | Description |
|--------------|-------------|
| `"ZoomIn"` | Zoom in — decreases time unit size (e.g. Month → Week → Day) |
| `"ZoomOut"` | Zoom out — increases time unit size (Day → Week → Month → Year) |
| `"ZoomToFit"` | Fits all tasks within the visible chart area |

---

## Programmatic Zoom API

### ZoomInAsync

Zoom in one level (more granular):

```csharp
await GanttRef.ZoomInAsync();
```

### ZoomOutAsync

Zoom out one level (less granular):

```csharp
await GanttRef.ZoomOutAsync();
```

### ZoomToFitAsync

Adjust zoom so all tasks are visible within the chart panel:

```csharp
await GanttRef.ZoomToFitAsync();
```

### ResetZoomAsync

Reset to the default zoom level:

```csharp
await GanttRef.ResetZoomAsync();
```

### Example: Programmatic Zoom Buttons

```razor
<div style="margin-bottom:8px; display:flex; gap:8px;">
    <button @onclick="ZoomIn">🔍 Zoom In</button>
    <button @onclick="ZoomOut">🔎 Zoom Out</button>
    <button @onclick="ZoomFit">Fit All</button>
    <button @onclick="ResetZoom">Reset</button>
</div>

<SfGantt @ref="GanttRef" DataSource="@TaskCollection" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" Duration="Duration" ParentID="ParentID" />
</SfGantt>

@code {
    private SfGantt<TaskData> GanttRef;

    private async Task ZoomIn()  => await GanttRef.ZoomInAsync();
    private async Task ZoomOut() => await GanttRef.ZoomOutAsync();
    private async Task ZoomFit() => await GanttRef.ZoomToFitAsync();
    private async Task ResetZoom() => await GanttRef.ResetZoomAsync();
}
```

---

## Custom Zooming Levels

Replace the default zoom level set with your own presets using `CustomZoomingLevels`:

```razor
<SfGantt DataSource="@TaskCollection"
         Height="450px" Width="900px"
         Toolbar="@(new List<string>() { "ZoomIn", "ZoomOut", "ZoomToFit" })"
         CustomZoomingLevels="@ZoomLevels">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" Duration="Duration" ParentID="ParentID" />
</SfGantt>

@code {
    private GanttZoomTimelineSettings[] ZoomLevels = new GanttZoomTimelineSettings[]
    {
        new GanttZoomTimelineSettings
        {
            Level = 1,
            TimelineUnitSize = 33,
            TopTier    = new GanttTopTierSettings    { Unit = TimelineViewMode.Year,  Format = "yyyy",      Count = 1 },
            BottomTier = new GanttBottomTierSettings { Unit = TimelineViewMode.Month, Format = "MMM",       Count = 1 }
        },
        new GanttZoomTimelineSettings
        {
            Level = 2,
            TimelineUnitSize = 33,
            TopTier    = new GanttTopTierSettings    { Unit = TimelineViewMode.Month, Format = "MMMM yyyy", Count = 1 },
            BottomTier = new GanttBottomTierSettings { Unit = TimelineViewMode.Week,  Format = "'W'w",      Count = 1 }
        },
        new GanttZoomTimelineSettings
        {
            Level = 3,
            TimelineUnitSize = 50,
            TopTier    = new GanttTopTierSettings    { Unit = TimelineViewMode.Week,  Format = "MMM dd",    Count = 1 },
            BottomTier = new GanttBottomTierSettings { Unit = TimelineViewMode.Day,   Format = "EEE d",     Count = 1 }
        },
        new GanttZoomTimelineSettings
        {
            Level = 4,
            TimelineUnitSize = 80,
            TopTier    = new GanttTopTierSettings    { Unit = TimelineViewMode.Day,   Format = "EEE MMM d", Count = 1 },
            BottomTier = new GanttBottomTierSettings { Unit = TimelineViewMode.Hour,  Format = "HH:mm",     Count = 6 }
        }
    };
}
```

### GanttZoomTimelineSettings Properties

| Property | Type | Description |
|----------|------|-------------|
| `Level` | `int` | Zoom level index; `ZoomIn` moves to higher levels, `ZoomOut` moves to lower |
| `TimelineUnitSize` | `double` | Pixel width of each bottom-tier cell at this level |
| `TopTier` | `GanttTopTierSettings` | Top header tier config |
| `BottomTier` | `GanttBottomTierSettings` | Bottom header tier config |

---

## ScrollToTimelineAsync

Programmatically scroll the chart viewport to a specific date without changing zoom level:

```csharp
// Scroll to project start
await GanttRef.ScrollToTimelineAsync(new DateTime(2024, 06, 01));

// Scroll to today
await GanttRef.ScrollToTimelineAsync(DateTime.Today);
```

---

## FitToProject

`ZoomToFitAsync` computes a zoom level where all task bars are visible within the current chart width. Use it after loading a new dataset to auto-adjust the view:

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await GanttRef.ZoomToFitAsync();
    }
}
```

---

## Complete Example

```razor
@using Syncfusion.Blazor.Gantt

<div style="margin-bottom:8px; display:flex; gap:6px; flex-wrap:wrap;">
    <button @onclick="ZoomIn">Zoom In</button>
    <button @onclick="ZoomOut">Zoom Out</button>
    <button @onclick="ZoomFit">Fit All</button>
    <button @onclick="ResetZoom">Reset</button>
    <button @onclick="GoToToday">Scroll to Today</button>
</div>

<SfGantt @ref="GanttRef"
         DataSource="@TaskCollection"
         Height="450px"
         Width="1000px"
         Toolbar="@(new List<string>() { "ZoomIn", "ZoomOut", "ZoomToFit" })"
         CustomZoomingLevels="@ZoomLevels">
    <GanttTaskFields Id="TaskID" Name="TaskName"
                     StartDate="StartDate" Duration="Duration"
                     Progress="Progress" ParentID="ParentID" />
    <GanttTimelineSettings>
        <GanttTopTierSettings Unit="TimelineViewMode.Month" Format="MMMM yyyy" />
        <GanttBottomTierSettings Unit="TimelineViewMode.Day" Format="d" />
    </GanttTimelineSettings>
</SfGantt>

@code {
    private SfGantt<TaskData> GanttRef;

    private async Task ZoomIn()    => await GanttRef.ZoomInAsync();
    private async Task ZoomOut()   => await GanttRef.ZoomOutAsync();
    private async Task ZoomFit()   => await GanttRef.ZoomToFitAsync();
    private async Task ResetZoom() => await GanttRef.ResetZoomAsync();
    private async Task GoToToday() => await GanttRef.ScrollToTimelineAsync(DateTime.Today);

    private List<TaskData> TaskCollection = new()
    {
        new TaskData { TaskID = 1, TaskName = "Project",    StartDate = new DateTime(2024, 04, 01) },
        new TaskData { TaskID = 2, TaskName = "Phase 1",    Duration = "10", Progress = 70, ParentID = 1, StartDate = new DateTime(2024, 04, 01) },
        new TaskData { TaskID = 3, TaskName = "Phase 2",    Duration = "15", Progress = 20, ParentID = 1, StartDate = new DateTime(2024, 04, 15) },
        new TaskData { TaskID = 4, TaskName = "Completion", Duration = "3",  Progress = 0,  ParentID = 1, StartDate = new DateTime(2024, 05, 06) }
    };

    private GanttZoomTimelineSettings[] ZoomLevels = new GanttZoomTimelineSettings[]
    {
        new GanttZoomTimelineSettings
        {
            Level = 1, TimelineUnitSize = 33,
            TopTier    = new GanttTopTierSettings    { Unit = TimelineViewMode.Year,  Format = "yyyy",       Count = 1 },
            BottomTier = new GanttBottomTierSettings { Unit = TimelineViewMode.Month, Format = "MMM",        Count = 1 }
        },
        new GanttZoomTimelineSettings
        {
            Level = 2, TimelineUnitSize = 40,
            TopTier    = new GanttTopTierSettings    { Unit = TimelineViewMode.Month, Format = "MMMM yyyy",  Count = 1 },
            BottomTier = new GanttBottomTierSettings { Unit = TimelineViewMode.Day,   Format = "d",          Count = 1 }
        },
        new GanttZoomTimelineSettings
        {
            Level = 3, TimelineUnitSize = 80,
            TopTier    = new GanttTopTierSettings    { Unit = TimelineViewMode.Day,   Format = "EEE MMM d",  Count = 1 },
            BottomTier = new GanttBottomTierSettings { Unit = TimelineViewMode.Hour,  Format = "HH",         Count = 4 }
        }
    };

    public class TaskData
    {
        public int TaskID { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public string Duration { get; set; }
        public int Progress { get; set; }
        public int? ParentID { get; set; }
    }
}
```

---

## Best Practices

- **Always include `ZoomToFit` toolbar item** — it is the most useful for users on load
- **Order `CustomZoomingLevels` by `Level` ascending** — Gantt iterates levels in order
- **Use `ZoomToFitAsync` in `OnAfterRenderAsync`** — auto-fit after the data loads for a clean first view
- **Test custom levels from Year → Hour** — verify each level has sensible `TimelineUnitSize` for readability
- **Combine `ScrollToTimelineAsync` with a "Today" button** — a standard UX pattern in Gantt applications

---

## Troubleshooting

**Issue**: `ZoomIn`/`ZoomOut` toolbar buttons do nothing  
**Solution**: Ensure `Toolbar` contains the string values `"ZoomIn"` and `"ZoomOut"` (exact casing).

**Issue**: Custom zoom levels appear to be ignored  
**Solution**: `CustomZoomingLevels` must be a non-null `GanttZoomTimelineSettings[]` array. Verify it is assigned before the component renders.

**Issue**: `ZoomToFitAsync` doesn't show all tasks  
**Solution**: Ensure `ViewStartDate`/`ViewEndDate` are not constraining the visible range. ZoomToFit works within the rendered range.

**Issue**: `ScrollToTimelineAsync` has no visible effect  
**Solution**: The scroll target date must be within the rendered date range. If the Gantt has `ViewStartDate`/`ViewEndDate` set, scroll targets outside that window won't move the viewport.

---
