# Event Buffer in Blazor Scheduler Component

## Table of Contents
- [Overview](#overview)
- [Enable Event Buffer](#enable-event-buffer)
- [Required Event Fields](#required-event-fields)
- [Mapping Buffer Fields](#mapping-buffer-fields)
- [BufferTemplate Usage](#buffertemplate-usage)
- [BufferTemplateContext Members](#buffertemplatecontext-members)
- [Buffer Behavior with AllowOverlap](#buffer-behavior-with-allowoverlap)
- [Supported Views](#supported-views)
- [Buffer Behavior During Drag and Resize](#buffer-behavior-during-drag-and-resize)
- [Notes](#notes)
- [Best Practices](#best-practices)
- [Common Implementation Mistakes](#common-implementation-mistakes)

## Overview

Event Buffer adds configurable pre- and post-procedure segments to each appointment. The buffer blocks scheduling around the event so preparation and recovery time is reserved alongside the actual appointment. The buffer is rendered as a separate element around the appointment and is also considered during overlap detection when `AllowOverlap` is set to `false`.

The buffer feature is enabled through `ScheduleEventSettings.EnableBuffer` and the per-appointment buffer durations are read from `BufferBefore` and `BufferAfter` fields on the appointment data. A `BufferTemplate` can be provided to customize how the buffer element is rendered.

**Key Concepts:**
- `EnableBuffer` turns the buffer feature on for the Scheduler
- `BufferBefore` / `BufferAfter` are integer fields (in minutes) on the appointment model
- Buffer segments are rendered around the appointment in supported views
- With `AllowOverlap="false"`, conflict detection respects both event duration and buffer duration
- During drag and resize, buffer segments re-anchor to the event boundaries

## Enable Event Buffer

Set `EnableBuffer="true"` on `ScheduleEventSettings` to activate the feature:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource" EnableBuffer="true"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Surgery",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 30, 0),
            BufferBefore = 30,
            BufferAfter = 45
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int BufferBefore { get; set; }
        public int BufferAfter { get; set; }
    }
}
```

## Required Event Fields

Each appointment must provide the buffer durations in minutes through the following properties on the data model:

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `BufferBefore` | int | Yes (when buffer is enabled) | Pre-procedure buffer duration in minutes. Rendered as a buffer segment before the appointment start. |
| `BufferAfter` | int | Yes (when buffer is enabled) | Post-procedure buffer duration in minutes. Rendered as a buffer segment after the appointment end. |

Both fields are read from the appointment object directly. If a value is missing, that side of the buffer is not rendered.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource" EnableBuffer="true"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Consultation",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 9, 30, 0),
            BufferBefore = 15,
            BufferAfter = 15
        },
        new AppointmentData
        {
            Id = 2,
            Subject = "Follow-up",
            StartTime = new DateTime(2026, 3, 24, 11, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 30, 0),
            BufferBefore = 0,
            BufferAfter = 30
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int BufferBefore { get; set; }
        public int BufferAfter { get; set; }
    }
}
```

## Mapping Buffer Fields

When the buffer values are stored under different property names on the data model, map them explicitly using `FieldBufferBefore` and `FieldBufferAfter` inside `ScheduleField`:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource" EnableBuffer="true">
        <ScheduleField>
            <FieldBufferBefore Name="PreBuffer"></FieldBufferBefore>
            <FieldBufferAfter Name="PostBuffer"></FieldBufferAfter>
        </ScheduleField>
    </ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Surgery",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 30, 0),
            PreBuffer = 30,
            PostBuffer = 45
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int PreBuffer { get; set; }
        public int PostBuffer { get; set; }
    }
}
```

## BufferTemplate Usage

Provide a `BufferTemplate` inside `ScheduleEventSettings` to control how the buffer element is rendered. The template context is `BufferTemplateContext<TValue>`, which exposes the buffer details for the current segment.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource" EnableBuffer="true">
        <BufferTemplate>
            @{
                var ctx = context as BufferTemplateContext<AppointmentData>;
            }
            @if (ctx != null && ctx.Duration > 0)
            {
                <div class="buffer-cell">
                    <span class="buffer-label">@ctx.Side</span>
                    <span class="buffer-duration">@ctx.Duration min</span>
                </div>
            }
        </BufferTemplate>
    </ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    .buffer-cell {
        display: flex;
        align-items: center;
        gap: 6px;
        font-size: 11px;
        font-weight: 600;
        padding: 2px 4px;
    }

    .buffer-duration {
        white-space: nowrap;
    }
</style>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Surgery",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 30, 0),
            BufferBefore = 30,
            BufferAfter = 45
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int BufferBefore { get; set; }
        public int BufferAfter { get; set; }
    }
}
```

## BufferTemplateContext Members

The `BufferTemplateContext<TValue>` provides access to the buffer segment being rendered:

| Member | Type | Description |
|--------|------|-------------|
| `IsBefore` | bool | `true` when rendering the buffer that appears before the appointment start; `false` for the buffer after the appointment end. |
| `Duration` | int | Buffer duration in minutes for the current segment. |
| `Data` | `TValue` | The underlying appointment data for the parent event. |

These members let you change the buffer appearance depending on which side of the appointment is being rendered, or render different content for short versus long buffers.

## Buffer Behavior with AllowOverlap

When `AllowOverlap="false"` is set on the Scheduler, conflict detection considers the appointment duration and the buffer periods together. A new event is rejected if it would overlap with the appointment itself or any of its associated buffer segments.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" AllowOverlap="false" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource" EnableBuffer="true"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Surgery",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 30, 0),
            BufferBefore = 30,
            BufferAfter = 45
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int BufferBefore { get; set; }
        public int BufferAfter { get; set; }
    }
}
```

With `AllowOverlap="false"`:
- Initial render prioritizes events that fit alongside existing buffers
- User actions (create, drag, resize) are blocked when the resulting time range collides with another event or its buffer
- Recurring events skip conflicting occurrences

## Supported Views

Buffer rendering and interaction are supported in the following Scheduler views:

- `View.Day`
- `View.Week`
- `View.WorkWeek`
- `View.TimelineDay`
- `View.TimelineWeek`
- `View.TimelineWorkWeek`

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource" EnableBuffer="true"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.TimelineDay"></ScheduleView>
        <ScheduleView Option="View.TimelineWeek"></ScheduleView>
        <ScheduleView Option="View.TimelineWorkWeek"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Surgery",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 30, 0),
            BufferBefore = 30,
            BufferAfter = 45
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int BufferBefore { get; set; }
        public int BufferAfter { get; set; }
    }
}
```

## Buffer Behavior During Drag and Resize

When an appointment is dragged or resized, the buffer segments automatically re-anchor to the new event boundaries. The dragged or resized block keeps the same `BufferBefore` and `BufferAfter` durations, so the user gets an accurate preview of the reserved preparation and recovery time at the new position.

## Notes

- `EnableBuffer` is set on `ScheduleEventSettings`, not on the Scheduler itself
- `BufferBefore` and `BufferAfter` are integer durations in minutes
- Buffer segments are not rendered when the corresponding duration is `0`
- Overlap detection with `AllowOverlap="false"` is applied per resource row when resources are configured
- The `BufferTemplate` is optional. When omitted, buffer segments render with their default appearance
- Buffer rendering is not supported in Month, Year, Agenda, and Month-Agenda views
- `BufferTemplateContext<TValue>` is the only context object available inside `BufferTemplate`

## Best Practices

- Always set both `BufferBefore` and `BufferBefore` to `0` explicitly on appointments that do not need buffers to make intent clear
- Pair `EnableBuffer` with `AllowOverlap="false"` when the buffer must block scheduling around the event
- Use the `IsBefore` property of `BufferTemplateContext` to render distinct content for pre- and post-procedure segments
- Map buffer fields through `FieldBufferBefore` / `FieldBufferAfter` when the data model uses different property names
- Limit the buffer template to lightweight markup so the cell height and rendering stay predictable
- Keep buffer durations consistent with the Scheduler `TimeScale` interval for accurate visual alignment

## Common Implementation Mistakes

- Forgetting to add `BufferBefore` and `BufferAfter` properties to the appointment model: the buffer will not render without them
- Setting `EnableBuffer` on the `SfSchedule` tag instead of on `ScheduleEventSettings`
- Using `BufferBefore` / `BufferAfter` as strings instead of integers (durations must be in minutes as `int`)
- Assuming the buffer is honored in Month, Year, or Agenda views; buffer segments are only rendered in the supported views listed above
- Not setting `AllowOverlap="false"` when the buffer must actively block conflicting events
- Mapping buffer fields with a custom name but forgetting to add the corresponding `FieldBufferBefore` / `FieldBufferAfter` entries inside `ScheduleField`
- Referencing the `BufferTemplate` context as `AppointmentData` directly; cast to `BufferTemplateContext<TValue>` to read `IsBefore`, `Duration`, and `Data`
