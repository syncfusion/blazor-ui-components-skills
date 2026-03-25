# Working Time, Holidays, and Timezone

## Table of Contents
- [When to Read](#when-to-read)
- [Working Time Configuration](#working-time-configuration)
- [Work Week Configuration](#work-week-configuration)
- [Holiday Configuration](#holiday-configuration)
- [Timezone Configuration](#timezone-configuration)
- [Impact on Duration Calculation](#impact-on-duration-calculation)
- [Complete Example](#complete-example)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## When to Read

Use this reference when you need to:
- Define custom working hours for the project (e.g. 8am–5pm)
- Specify which days of the week are working days
- Mark specific calendar dates as holidays (non-working days)
- Set a timezone for date rendering and calculation
- Understand how working time settings affect duration arithmetic

---

## Working Time Configuration

Define daily working hours using `GanttDayWorkingTime`:

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt DataSource="@TaskCollection" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" ParentID="ParentID" />
    <GanttDayWorkingTimeCollection>
        <GanttDayWorkingTime From="8" To="12" />
        <GanttDayWorkingTime From="13" To="17" />
    </GanttDayWorkingTimeCollection>
</SfGantt>
```

This defines a split shift: 8:00–12:00 and 13:00–17:00 (8 working hours per day with a 1-hour lunch break).

### GanttDayWorkingTime Properties

| Property | Type | Description |
|----------|------|-------------|
| `From` | `double` | Start time as a fractional hour (e.g. `8` = 8:00 AM, `8.5` = 8:30 AM) |
| `To` | `double` | End time as a fractional hour (e.g. `17` = 5:00 PM) |

### Example — Standard 9–5

```razor
<GanttDayWorkingTimeCollection>
    <GanttDayWorkingTime From="9" To="17" />
</GanttDayWorkingTimeCollection>
```

### Example — Two Shifts

```razor
<GanttDayWorkingTimeCollection>
    <GanttDayWorkingTime From="6"  To="14" />
    <GanttDayWorkingTime From="14" To="22" />
</GanttDayWorkingTimeCollection>
```

---

## Work Week Configuration

Specify which days of the week are working days using `WorkWeek`:

```razor
<SfGantt DataSource="@TaskCollection"
         WorkWeek="@(new string[] { "Monday", "Tuesday", "Wednesday", "Thursday", "Friday" })">
</SfGantt>
```

### WorkWeek Valid Values

Use full English day names (case-sensitive):

```text
"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"
```

### Example — 6-Day Work Week (Monday–Saturday)

```razor
<SfGantt DataSource="@TaskCollection"
         WorkWeek="@(new string[] { "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday" })">
</SfGantt>
```

### Example — 4-Day Work Week (Mon–Thu)

```razor
<SfGantt DataSource="@TaskCollection"
         WorkWeek="@(new string[] { "Monday", "Tuesday", "Wednesday", "Thursday" })">
</SfGantt>
```

Default when not set: Monday through Friday (5-day week).

---

## Holiday Configuration

Mark specific dates as non-working holidays using `GanttHolidays`:

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" ParentID="ParentID" />
    <GanttHolidays>
        <GanttHoliday From="@(new DateTime(2024, 01, 01))" Label="New Year's Day" CssClass="holiday-national" />
        <GanttHoliday From="@(new DateTime(2024, 12, 25))" Label="Christmas Day"  CssClass="holiday-national" />
        <GanttHoliday From="@(new DateTime(2024, 07, 04))" To="@(new DateTime(2024, 07, 08))"
                      Label="Summer Break" CssClass="holiday-closure" />
    </GanttHolidays>
</SfGantt>

<style>
    .holiday-national  .e-gantt-holiday { background-color: rgba(231, 74, 59, 0.15); }
    .holiday-closure   .e-gantt-holiday { background-color: rgba(246, 194, 62, 0.15); }
</style>
```

### GanttHoliday Properties

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `From` | `DateTime` | ✅ | Start date of the holiday |
| `To` | `DateTime` | | End date for multi-day holidays (inclusive). If omitted, single-day holiday. |
| `Label` | `string` | | Text label displayed in the holiday band |
| `CssClass` | `string` | | CSS class for custom holiday styling |

### Multi-Day Holiday

```razor
<GanttHoliday From="@(new DateTime(2024, 12, 23))"
              To="@(new DateTime(2024, 12, 27))"
              Label="Winter Break" />
```

---

## Timezone Configuration

Set a timezone so all date display and calculations use a consistent zone, regardless of the user's local clock:

```razor
<SfGantt DataSource="@TaskCollection"
         Timezone="America/New_York"
         Height="450px" Width="900px">
</SfGantt>
```

### Timezone String Format

Use IANA timezone identifiers (same as JavaScript `Intl.DateTimeFormat`):

| Example | Description |
|---------|-------------|
| `"UTC"` | Coordinated Universal Time |
| `"America/New_York"` | US Eastern (UTC-5/UTC-4 DST) |
| `"America/Chicago"` | US Central |
| `"America/Los_Angeles"` | US Pacific |
| `"Europe/London"` | UK time |
| `"Europe/Berlin"` | Central European time |
| `"Asia/Tokyo"` | Japan Standard Time |
| `"Asia/Kolkata"` | India Standard Time |

### When to Use Timezone

- **Multi-region teams**: Ensure all users see the same dates regardless of local browser timezone
- **Server-stored UTC dates**: Convert stored UTC to project timezone for display
- **Avoid DST anomalies**: Explicit timezone prevents ambiguous dates at DST boundaries

---

## Impact on Duration Calculation

Working time settings directly affect how `Duration` is calculated and displayed:

| Setting | Effect on Duration |
|---------|-------------------|
| Default (Mon–Fri, 8h/day) | 1 day = 8 working hours |
| Custom hours (e.g. 9–17) | 1 day = 8 working hours (same math) |
| Split shift (8–12, 13–17) | 1 day = 8 working hours |
| 6-day week (Mon–Sat) | 1 week = 6 working days |
| Holidays | Holiday dates are skipped; task extends beyond them |

**Key behaviors:**
- A task starting on a holiday automatically shifts its effective start to the next working day
- Holidays within a task's date range are excluded from duration calculation
- Non-working days (weekends/holidays) are visually dimmed in the chart

---

## Complete Example

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt DataSource="@TaskCollection"
         Height="500px"
         Width="1000px"
         Timezone="America/New_York"
         WorkWeek="@(new string[] { "Monday", "Tuesday", "Wednesday", "Thursday", "Friday" })">

    <GanttTaskFields Id="TaskID" Name="TaskName"
                     StartDate="StartDate" Duration="Duration"
                     Progress="Progress" ParentID="ParentID" />

    <GanttDayWorkingTimeCollection>
        <GanttDayWorkingTime From="9"  To="12" />
        <GanttDayWorkingTime From="13" To="18" />
    </GanttDayWorkingTimeCollection>

    <GanttHolidays>
        <GanttHoliday From="@(new DateTime(2024, 04, 08))" Label="Spring Holiday" CssClass="spring-holiday" />
        <GanttHoliday From="@(new DateTime(2024, 04, 19))" To="@(new DateTime(2024, 04, 21))" Label="Long Weekend" />
    </GanttHolidays>

    <GanttTimelineSettings>
        <GanttTopTierSettings Unit="TimelineViewMode.Month" Format="MMMM yyyy" />
        <GanttBottomTierSettings Unit="TimelineViewMode.Day" Format="d" />
    </GanttTimelineSettings>
</SfGantt>

<style>
    .spring-holiday .e-gantt-holiday { background-color: rgba(78, 115, 223, 0.15); }
</style>

@code {
    private List<TaskData> TaskCollection = new()
    {
        new TaskData { TaskID = 1, TaskName = "Phase 1",  StartDate = new DateTime(2024, 04, 01) },
        new TaskData { TaskID = 2, TaskName = "Design",   Duration = "5",  Progress = 80, ParentID = 1, StartDate = new DateTime(2024, 04, 01) },
        new TaskData { TaskID = 3, TaskName = "Build",    Duration = "8",  Progress = 30, ParentID = 1, StartDate = new DateTime(2024, 04, 08) },
        new TaskData { TaskID = 4, TaskName = "QA",       Duration = "4",  Progress = 0,  ParentID = 1, StartDate = new DateTime(2024, 04, 22) }
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

- **Always define `GanttDayWorkingTime` explicitly** — the default is 8:00–17:00 with a 1h break; make it explicit for clarity
- **Use `WorkWeek` to exclude non-working days** — this prevents tasks from spanning weekends in duration counts
- **Test holiday edge cases** — a 1-day task falling on a holiday should shift to the next working day
- **Use consistent `Timezone`** for server-rendered data — store dates in UTC and let the component display them in the project timezone
- **Prefix `CssClass` on holidays** — prevents accidental collision with built-in Gantt CSS class names

---

## Troubleshooting

**Issue**: Weekends still appear as working days  
**Solution**: Set `WorkWeek` explicitly to exclude Saturday/Sunday. This is separate from `ShowWeekend` (which controls visibility only).

**Issue**: Holiday dates are not greyed out  
**Solution**: Verify `GanttHoliday` `From` dates are within the visible timeline range.

**Issue**: Duration calculation doesn't skip holidays  
**Solution**: Holidays only affect duration when properly defined in `GanttHolidays`. Verify the holiday dates match the dates falling within the task span.

**Issue**: All dates appear shifted by several hours  
**Solution**: Set `Timezone` to match your project's reference timezone. Without it, the browser local timezone is used and may misalign UTC-stored dates.

---
