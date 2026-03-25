# Timeline Configuration

## Table of Contents
- [When to Read](#when-to-read)
- [Basic Timeline Setup](#basic-timeline-setup)
- [TimelineViewMode](#timelineviewmode)
- [Top and Bottom Tier Settings](#top-and-bottom-tier-settings)
- [Tier Format Strings](#tier-format-strings)
- [WeekStartDay and ShowWeekend](#weekstartday-and-showweekend)
- [TimelineUnitSize](#timelineunitsize)
- [ViewStartDate and ViewEndDate](#viewstartdate-and-viewenddate)
- [Programmatic Timeline Navigation](#programmatic-timeline-navigation)
- [Custom Zooming Levels](#custom-zooming-levels)
- [Timeline Tooltip](#timeline-tooltip)
- [Complete Example](#complete-example)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## When to Read

Use this reference when you need to:
- Configure the Gantt timeline header (top and bottom tiers)
- Switch between Hour, Day, Week, Month, Year views
- Set custom date formats for timeline cells
- Programmatically navigate (next/previous time span, scroll to date)
- Control visible date range with `ViewStartDate` / `ViewEndDate`
- Define custom zooming level sets

---

## Basic Timeline Setup

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt DataSource="@TaskCollection" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" ParentID="ParentID" />
    <GanttTimelineSettings TimelineUnitSize="60">
        <GanttTopTierSettings Unit="TimelineViewMode.Week" Format="MMM dd, yyyy" />
        <GanttBottomTierSettings Unit="TimelineViewMode.Day" Format="d" Count="1" />
    </GanttTimelineSettings>
</SfGantt>
```

---

## TimelineViewMode

Controls the granularity of each timeline header tier:

| Enum Value | Description |
|------------|-------------|
| `TimelineViewMode.Hour` | Each cell = 1 hour |
| `TimelineViewMode.Day` | Each cell = 1 day |
| `TimelineViewMode.Week` | Each cell = 1 week |
| `TimelineViewMode.Month` | Each cell = 1 month |
| `TimelineViewMode.Year` | Each cell = 1 year |

Example — Month/Week view:

```razor
<GanttTimelineSettings>
    <GanttTopTierSettings Unit="TimelineViewMode.Month" Format="MMMM yyyy" />
    <GanttBottomTierSettings Unit="TimelineViewMode.Week" Format="dd" />
</GanttTimelineSettings>
```

---

## Top and Bottom Tier Settings

The timeline header has two tiers. Use `GanttTopTierSettings` and `GanttBottomTierSettings`:

```razor
<GanttTimelineSettings>
    <GanttTopTierSettings
        Unit="TimelineViewMode.Month"
        Format="MMMM yyyy"
        Count="1" />
    <GanttBottomTierSettings
        Unit="TimelineViewMode.Day"
        Format="EEE d"
        Count="1" />
</GanttTimelineSettings>
```

### Tier Setting Properties

| Property | Type | Description |
|----------|------|-------------|
| `Unit` | `TimelineViewMode` | Time unit for this tier |
| `Format` | `string` | Date format string (Intl.DateTimeFormat tokens) |
| `Count` | `int` | Number of units per cell (e.g. `Count=2` = 2 days per cell) |

---

## Tier Format Strings

Format strings follow .NET / JavaScript date token conventions:

| Token | Example Output | Meaning |
|-------|---------------|---------|
| `d` | 4 | Day of month, no padding |
| `dd` | 04 | Day of month, zero-padded |
| `ddd` | Mon | Short weekday name |
| `dddd` | Monday | Full weekday name |
| `M` | 4 | Month number, no padding |
| `MM` | 04 | Month number, zero-padded |
| `MMM` | Apr | Short month name |
| `MMMM` | April | Full month name |
| `yy` | 24 | 2-digit year |
| `yyyy` | 2024 | 4-digit year |
| `h` | 9 | Hour (12h), no padding |
| `hh` | 09 | Hour (12h), zero-padded |
| `HH` | 14 | Hour (24h), zero-padded |

Example format combinations:

```text
"MMM dd, yyyy"   →  Apr 01, 2024
"EEE, MMM d"     →  Mon, Apr 1
"W w"            →  W 14   (week number)
"HH:mm"          →  09:30
```

---

## WeekStartDay and ShowWeekend

### WeekStartDay

Set which day starts the week (0 = Sunday, 1 = Monday, …, 6 = Saturday):

```razor
<GanttTimelineSettings WeekStartDay="1">
    <!-- Monday starts week -->
</GanttTimelineSettings>
```

### ShowWeekend

Hide or show Saturday/Sunday columns:

```razor
<SfGantt DataSource="@TaskCollection" ShowWeekend="false">
</SfGantt>
```

Weekends are shown by default. Set `ShowWeekend="false"` to collapse them and reduce chart width.

---

## TimelineUnitSize

Controls the pixel width of each bottom-tier cell:

```razor
<GanttTimelineSettings TimelineUnitSize="75">
    <!-- Each day column is 75px wide -->
</GanttTimelineSettings>
```

- Default is `33`
- Larger values increase chart width and readability
- Smaller values compress the timeline

---

## ViewStartDate and ViewEndDate

Restrict the visible timeline range:

```razor
<SfGantt DataSource="@TaskCollection"
         ViewStartDate="@(new DateTime(2024, 01, 01))"
         ViewEndDate="@(new DateTime(2024, 12, 31))">
</SfGantt>
```

- Only the date range between `ViewStartDate` and `ViewEndDate` is rendered
- Tasks outside this range are not visible in the chart panel
- The grid panel still shows all tasks

---

## Programmatic Timeline Navigation

### Scroll to a Specific Date

```csharp
await GanttRef.ScrollToTimelineAsync(new DateTime(2024, 06, 01));
```

### Navigate to Next/Previous Time Span

```csharp
// Move timeline forward by one view period
await GanttRef.NextTimeSpan();

// Move timeline backward by one view period
await GanttRef.PreviousTimeSpan();
```

### Update Timescale View

Programmatically change the current zoom level:

```csharp
await GanttRef.UpdateTimescaleViewAsync("Week");
```

Valid string values: `"Hour"`, `"Day"`, `"Week"`, `"Month"`, `"Year"`

### Example: Navigation Buttons

```razor
<div style="margin-bottom:8px; display:flex; gap:8px;">
    <button @onclick="PrevSpan">← Previous</button>
    <button @onclick="NextSpan">Next →</button>
    <button @onclick="GoToToday">Today</button>
</div>

<SfGantt @ref="GanttRef" DataSource="@TaskCollection" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" Duration="Duration" ParentID="ParentID" />
    <GanttTimelineSettings>
        <GanttTopTierSettings Unit="TimelineViewMode.Month" Format="MMMM yyyy" />
        <GanttBottomTierSettings Unit="TimelineViewMode.Day" Format="d" />
    </GanttTimelineSettings>
</SfGantt>

@code {
    private SfGantt<TaskData> GanttRef;

    private async Task PrevSpan() => await GanttRef.PreviousTimeSpan();
    private async Task NextSpan() => await GanttRef.NextTimeSpan();
    private async Task GoToToday() => await GanttRef.ScrollToTimelineAsync(DateTime.Today);
}
```

---

## Custom Zooming Levels

Define a custom set of zoom presets using `GanttZoomTimelineSettings`:

```razor
<SfGantt DataSource="@TaskCollection"
         Toolbar="@(new List<string>() { "ZoomIn", "ZoomOut", "ZoomToFit" })"
         CustomZoomingLevels="@ZoomingLevels">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" Duration="Duration" ParentID="ParentID" />
</SfGantt>

@code {
    private GanttZoomTimelineSettings[] ZoomingLevels = new GanttZoomTimelineSettings[]
    {
        new GanttZoomTimelineSettings
        {
            TopTier = new GanttTopTierSettings { Unit = TimelineViewMode.Year,  Format = "yyyy",       Count = 1 },
            BottomTier = new GanttBottomTierSettings { Unit = TimelineViewMode.Month, Format = "MMM", Count = 1 },
            TimelineUnitSize = 33, Level = 1
        },
        new GanttZoomTimelineSettings
        {
            TopTier = new GanttTopTierSettings { Unit = TimelineViewMode.Month, Format = "MMMM yyyy",  Count = 1 },
            BottomTier = new GanttBottomTierSettings { Unit = TimelineViewMode.Week,  Format = "dd",   Count = 1 },
            TimelineUnitSize = 33, Level = 2
        },
        new GanttZoomTimelineSettings
        {
            TopTier = new GanttTopTierSettings { Unit = TimelineViewMode.Week,  Format = "'W'w yyyy",  Count = 1 },
            BottomTier = new GanttBottomTierSettings { Unit = TimelineViewMode.Day,   Format = "EEE d", Count = 1 },
            TimelineUnitSize = 50, Level = 3
        }
    };
}
```

### GanttZoomTimelineSettings Properties

| Property | Type | Description |
|----------|------|-------------|
| `TopTier` | `GanttTopTierSettings` | Top tier configuration for this zoom level |
| `BottomTier` | `GanttBottomTierSettings` | Bottom tier configuration for this zoom level |
| `TimelineUnitSize` | `double` | Cell pixel width at this zoom level |
| `Level` | `int` | Zoom level index (lower = more zoomed out) |

---

## Timeline Tooltip

Show a tooltip when hovering over timeline header cells:

```razor
<GanttTimelineSettings ShowTooltip="true">
    <GanttTopTierSettings Unit="TimelineViewMode.Month" Format="MMMM yyyy" />
    <GanttBottomTierSettings Unit="TimelineViewMode.Day" Format="d" />
</GanttTimelineSettings>
```

---

## Complete Example

```razor
@using Syncfusion.Blazor.Gantt

<div style="margin-bottom:8px; display:flex; gap:8px; flex-wrap:wrap;">
    <button @onclick="PrevSpan">← Prev</button>
    <button @onclick="NextSpan">Next →</button>
    <button @onclick="GoToToday">Today</button>
    <button @onclick='() => SwitchView("Day")'>Day</button>
    <button @onclick='() => SwitchView("Week")'>Week</button>
    <button @onclick='() => SwitchView("Month")'>Month</button>
</div>

<SfGantt @ref="GanttRef"
         DataSource="@TaskCollection"
         Height="450px"
         Width="1000px"
         ShowWeekend="false">
    <GanttTaskFields Id="TaskID" Name="TaskName"
                     StartDate="StartDate" Duration="Duration"
                     Progress="Progress" ParentID="ParentID" />
    <GanttTimelineSettings TimelineUnitSize="60" WeekStartDay="1">
        <GanttTopTierSettings Unit="TimelineViewMode.Month" Format="MMMM yyyy" Count="1" />
        <GanttBottomTierSettings Unit="TimelineViewMode.Day" Format="EEE d" Count="1" />
    </GanttTimelineSettings>
</SfGantt>

@code {
    private SfGantt<TaskData> GanttRef;

    private List<TaskData> TaskCollection = new()
    {
        new TaskData { TaskID = 1, TaskName = "Phase 1", StartDate = new DateTime(2024, 04, 01) },
        new TaskData { TaskID = 2, TaskName = "Design",  StartDate = new DateTime(2024, 04, 01), Duration = "5",  Progress = 80, ParentID = 1 },
        new TaskData { TaskID = 3, TaskName = "Build",   StartDate = new DateTime(2024, 04, 08), Duration = "10", Progress = 30, ParentID = 1 }
    };

    private async Task PrevSpan()  => await GanttRef.PreviousTimeSpan();
    private async Task NextSpan()  => await GanttRef.NextTimeSpan();
    private async Task GoToToday() => await GanttRef.ScrollToTimelineAsync(DateTime.Today);

    private async Task SwitchView(string view)
    {
        await GanttRef.UpdateTimescaleViewAsync(view);
    }

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

- **Use `WeekStartDay="1"` for European locales** — Monday-start week is standard in many countries
- **Set `ShowWeekend="false"` for work-only projects** — reduces chart width and focuses on working days
- **Use `TimelineUnitSize` ≥ 50 for Day view** — narrower cells become unreadable at scale
- **Provide ZoomIn/ZoomOut toolbar items** when using custom zooming levels — users expect a familiar control
- **Pair `ViewStartDate`/`ViewEndDate` with project bounds** — prevents rendering an unbounded timeline

---

## Troubleshooting

**Issue**: Timeline shows wrong week start  
**Solution**: Set `WeekStartDay` on `GanttTimelineSettings`. Default is 0 (Sunday).

**Issue**: `ScrollToTimelineAsync` has no visible effect  
**Solution**: Ensure the target date is within the rendered date range. If `ViewStartDate`/`ViewEndDate` are set, the scroll target must be within those bounds.

**Issue**: Custom zooming levels are ignored  
**Solution**: Confirm `CustomZoomingLevels` is assigned a non-null array and levels are ordered by `Level` value ascending.

**Issue**: Timeline tooltip not showing  
**Solution**: Set `ShowTooltip="true"` on `GanttTimelineSettings` (not on the root `SfGantt` component).

---
