# Scheduler Views

## Table of Contents
- [Overview](#overview)
- [Available View Modes](#available-view-modes)
- [Setting Current View](#setting-current-view)
- [View Configuration Properties](#view-configuration-properties)
- [Day View](#day-view)
- [Day View with Multiple Days](#day-view-with-multiple-days)
- [Week View](#week-view)
- [Week View with Custom First Day](#week-view-with-custom-first-day)
- [Work Week View](#work-week-view)
- [Custom Working Days](#custom-working-days)
- [Month View](#month-view)
- [Month View with Multiple Events](#month-view-with-multiple-events)
- [Agenda View](#agenda-view)
- [Agenda View with Custom Day Count](#agenda-view-with-custom-day-count)
- [Month Agenda View](#month-agenda-view)
- [Timeline Day View](#timeline-day-view)
- [Timeline Week View](#timeline-week-view)
- [Timeline Work Week View](#timeline-work-week-view)
- [Timeline Month View](#timeline-month-view)
- [Timeline Year View](#timeline-year-view)
- [Year View](#year-view)
- [View-Specific Configuration](#view-specific-configuration)
- [View Templates](#view-templates)
- [Start and End Hours](#start-and-end-hours)
- [Hide Weekends](#hide-weekends)
- [Extending View Intervals](#extending-view-intervals)
- [Display Name for Custom Views](#display-name-for-custom-views)
- [Week Numbers in Views](#week-numbers-in-views)
- [Readonly Views](#readonly-views)

## Overview

The Scheduler supports 12 different view modes to display appointments from various perspectives: daily, weekly, monthly, yearly, and timeline-based layouts. Each view can be individually configured with custom properties, templates, and styling. Views are added to the Scheduler using the `ScheduleViews` collection, and you can switch between views programmatically or via the header navigation.

**View Categories:**
- **Vertical Calendar Views:** Day, Week, WorkWeek, Month, Agenda, MonthAgenda, Year
- **Horizontal Timeline Views:** TimelineDay, TimelineWeek, TimelineWorkWeek, TimelineMonth, TimelineYear

## Available View Modes

The Scheduler supports these view modes (set via `View` enum in `ScheduleView Option`):

| View | Description | Use Case |
|------|-------------|----------|
| **Day** | Single day with hourly time slots | Detailed daily schedule |
| **Week** | 7-day week (Sunday-Saturday) | Weekly overview |
| **WorkWeek** | Working days only (Mon-Fri by default) | Business week focus |
| **Month** | Full month calendar | Monthly planning |
| **Agenda** | Upcoming events list (7 days default) | Quick event review |
| **MonthAgenda** | Month calendar + agenda below | Month with details |
| **TimelineDay** | Horizontal single day | Horizontal daily view |
| **TimelineWeek** | Horizontal 7-day week | Horizontal weekly view |
| **TimelineWorkWeek** | Horizontal working days | Horizontal work week |
| **TimelineMonth** | Horizontal month | Horizontal month view |
| **TimelineYear** | Horizontal full year | Horizontal yearly view |
| **Year** | Year mini-calendar | Year overview |

## Setting Current View

Set the active view using `@bind-CurrentView` property:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-CurrentView="@CurrentView" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
        <ScheduleView Option="View.Agenda"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    View CurrentView = View.Week; // Default view

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Team Standup",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 9, 30, 0)
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## View Configuration Properties

ScheduleView component provides these configuration properties:

| Property | Type | Applicable Views | Description |
|----------|------|------------------|-------------|
| **Option** | View | All | View mode (Day, Week, Month, etc.) |
| **IsSelected** | bool | All | Set as active view on load |
| **DateFormat** | string | All | Date display format per view |
| **Readonly** | bool | All | Disable CRUD operations |
| **ShowWeekend** | bool | All except Agenda | Hide Saturday/Sunday |
| **DisplayName** | string | All except Agenda/MonthAgenda | Custom view label in toolbar |
| **Interval** | int | All except Agenda/MonthAgenda | Number of units (days/weeks/months) |
| **StartHour** | string | Day, Week, WorkWeek, Timeline views | Work start hour (e.g., "09:00") |
| **EndHour** | string | Day, Week, WorkWeek, Timeline views | Work end hour (e.g., "18:00") |
| **WorkDays** | int[] | All except Agenda/Timeline | Custom working days (0=Sun, 1=Mon, etc.) |
| **ShowWeekNumber** | bool | Day, Week, WorkWeek, Month | Display ISO week numbers |
| **MaxEventsPerRow** | int | Month, Agenda, Timeline views | Events displayed per cell |
| **AllowVirtualScrolling** | bool | Agenda, Timeline views | Enable virtual scrolling |
| **CellTemplate** | RenderFragment | All except Agenda | Custom cell content |
| **EventTemplate** | RenderFragment | All | Custom event rendering |
| **DateHeaderTemplate** | RenderFragment | All | Custom date header |
| **ResourceHeaderTemplate** | RenderFragment | All | Custom resource header |
| **TimeScale** | ScheduleTimeScale | Day, Week, WorkWeek, Timeline | Timescale configuration |
| **HeaderRows** | ScheduleHeaderRows | Timeline views | Multi-level headers |
| **Group** | ScheduleGroup | All | Resource grouping config |
| **Orientation** | Orientation | TimelineYear, Year | Horizontal/Vertical layout |

## Day View

Display a single day with hourly time slots:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Morning Standup",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 9, 30, 0)
        },
        new AppointmentData
        {
            Id = 2,
            Subject = "Development Task",
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 12, 0, 0)
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Day View with Multiple Days

Extend Day view to show multiple consecutive days:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <!-- Show 3 consecutive days -->
        <ScheduleView Option="View.Day" Interval="3" DisplayName="3 Days"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Day 1 Event",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0)
        },
        new AppointmentData
        {
            Id = 2,
            Subject = "Day 2 Event",
            StartTime = new DateTime(2026, 3, 25, 14, 0, 0),
            EndTime = new DateTime(2026, 3, 25, 15, 0, 0)
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Week View

Display 7-day week from Sunday to Saturday:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Monday Meeting",
            StartTime = new DateTime(2026, 3, 23, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 23, 11, 0, 0)
        },
        new AppointmentData
        {
            Id = 2,
            Subject = "Friday Review",
            StartTime = new DateTime(2026, 3, 27, 16, 0, 0),
            EndTime = new DateTime(2026, 3, 27, 17, 0, 0)
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Week View with Custom First Day

Set the first day of the week:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" FirstDayOfWeek="1" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <!-- Week starting from Monday (1) instead of Sunday (0) -->
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Work Week View

Display only working days (Monday-Friday by default):

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Weekday Event",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0)
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Custom Working Days

Configure custom working days per view:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <!-- Display only Mon(1), Wed(3), Fri(5) -->
        <ScheduleView Option="View.WorkWeek" WorkDays="@CustomDays"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public int[] CustomDays = { 1, 3, 5 }; // Mon, Wed, Fri

    List<AppointmentData> DataSource = new List<AppointmentData>();

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Month View

Display full month calendar with appointments:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Team Meeting",
            StartTime = new DateTime(2026, 3, 10, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 10, 11, 0, 0)
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Month View with Multiple Events

Control events displayed per cell using `MaxEventsPerRow`:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <!-- Show 3 events per cell, rest shown with "+more" -->
        <ScheduleView Option="View.Month" MaxEventsPerRow="3"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Event 1",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0)
        },
        new AppointmentData
        {
            Id = 2,
            Subject = "Event 2",
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0)
        },
        new AppointmentData
        {
            Id = 3,
            Subject = "Event 3",
            StartTime = new DateTime(2026, 3, 24, 11, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 12, 0, 0)
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Agenda View

Display upcoming appointments in list format (7 days by default):

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Agenda"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Task 1",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0)
        },
        new AppointmentData
        {
            Id = 2,
            Subject = "Task 2",
            StartTime = new DateTime(2026, 3, 26, 14, 0, 0),
            EndTime = new DateTime(2026, 3, 26, 15, 0, 0)
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Agenda View with Custom Day Count

Set custom number of days displayed in Agenda using `AgendaDaysCount`:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" AgendaDaysCount="14" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <!-- Show 14 days instead of default 7 -->
        <ScheduleView Option="View.Agenda"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Month Agenda View

Combine month calendar with agenda list below:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.MonthAgenda"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Meeting",
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0)
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Timeline Day View

Display single day in horizontal layout with hourly columns:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineDay" MaxEventsPerRow="4"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Morning Task",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0)
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Timeline Week View

Display 7-day week in horizontal layout:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineWeek" MaxEventsPerRow="2"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Timeline Work Week View

Display working days (Mon-Fri) in horizontal layout:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineWorkWeek" MaxEventsPerRow="3"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Timeline Month View

Display entire month in horizontal layout:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineMonth" MaxEventsPerRow="2"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Timeline Year View

Display entire year in horizontal layout:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <!-- Default: Horizontal orientation -->
        <ScheduleView Option="View.TimelineYear" DisplayName="Horizontal Year"></ScheduleView>
        <!-- Vertical orientation -->
        <ScheduleView Option="View.TimelineYear" Orientation="Orientation.Vertical" DisplayName="Vertical Year"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Year View

Display full year mini-calendar with click-to-navigate:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Year"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## View-Specific Configuration

Configure different properties for different views:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <!-- Day view: show all-day row -->
        <ScheduleView Option="View.Day" StartHour="08:00" EndHour="18:00"></ScheduleView>
        
        <!-- Week view: hide weekends, show week numbers -->
        <ScheduleView Option="View.Week" ShowWeekend="false" ShowWeekNumber="true"></ScheduleView>
        
        <!-- Month view: limit events, make readonly -->
        <ScheduleView Option="View.Month" MaxEventsPerRow="2" Readonly="true"></ScheduleView>
        
        <!-- Agenda view: custom day count -->
        <ScheduleView Option="View.Agenda" AllowVirtualScrolling="true"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## View Templates

Customize event rendering per view:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Month">
            <EventTemplate>
                <div style="background-color: @((context as AppointmentData).Color); color: white; padding: 5px;">
                    @((context as AppointmentData).Subject)
                </div>
            </EventTemplate>
        </ScheduleView>
        
        <ScheduleView Option="View.Week">
            <DateHeaderTemplate>
                <div style="text-align: center; font-weight: bold;">
                    @(((context as TemplateContext).Date).ToString("ddd"))
                </div>
            </DateHeaderTemplate>
        </ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Meeting",
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0),
            Color = "#FF6B6B"
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public string Color { get; set; }
    }
}
```

## Start and End Hours

Limit visible time range using `StartHour` and `EndHour`:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <!-- Show business hours only: 8 AM to 6 PM -->
        <ScheduleView Option="View.Day" StartHour="08:00" EndHour="18:00"></ScheduleView>
        <ScheduleView Option="View.Week" StartHour="08:00" EndHour="18:00"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Hide Weekends

Remove Saturday and Sunday from views using `ShowWeekend`:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week" ShowWeekend="false"></ScheduleView>
        <ScheduleView Option="View.Month" ShowWeekend="false"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Extending View Intervals

Display multiple periods in a single view using `Interval`:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <!-- Show 3 days instead of 1 -->
        <ScheduleView Option="View.Day" Interval="3" DisplayName="3 Days"></ScheduleView>
        
        <!-- Show 2 weeks instead of 1 -->
        <ScheduleView Option="View.Week" Interval="2" DisplayName="2 Weeks"></ScheduleView>
        
        <!-- Show 3 months instead of 1 -->
        <ScheduleView Option="View.Month" Interval="3" DisplayName="Quarterly"></ScheduleView>
        
        <!-- Show 12 months (entire year) in month view -->
        <ScheduleView Option="View.TimelineMonth" Interval="12" DisplayName="Year Timeline"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Display Name for Custom Views

Add custom labels for extended views using `DisplayName`:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day" Interval="1" DisplayName="Daily"></ScheduleView>
        <ScheduleView Option="View.Day" Interval="3" DisplayName="3-Day View"></ScheduleView>
        <ScheduleView Option="View.Week" Interval="1" DisplayName="Weekly"></ScheduleView>
        <ScheduleView Option="View.Week" Interval="2" DisplayName="Bi-Weekly"></ScheduleView>
        <ScheduleView Option="View.Month" Interval="1" DisplayName="Monthly"></ScheduleView>
        <ScheduleView Option="View.Month" Interval="3" DisplayName="Quarterly"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Week Numbers in Views

Display ISO week numbers using `ShowWeekNumber`:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week" ShowWeekNumber="true"></ScheduleView>
        <ScheduleView Option="View.Month" ShowWeekNumber="true"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Readonly Views

Prevent CRUD operations on specific views using `Readonly`:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <!-- Month view is editable -->
        <ScheduleView Option="View.Month" Readonly="false"></ScheduleView>
        
        <!-- Week view is read-only (no create, edit, delete) -->
        <ScheduleView Option="View.Week" Readonly="true"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```
