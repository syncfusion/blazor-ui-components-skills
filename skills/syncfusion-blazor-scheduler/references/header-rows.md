# Timeline Header Rows Customization

## Table of Contents
- [Overview](#overview)
- [ScheduleHeaderRow Component](#scheduleheaderrow-component)
- [HeaderRowType Options](#headerrowtype-options)
- [Basic Header Row Setup](#basic-header-row-setup)
- [Year and Month Header Rows](#year-and-month-header-rows)
- [Week Number Header Rows](#week-number-header-rows)
- [Displaying Complete Year Timeline](#displaying-complete-year-timeline)
- [Day and Hour Header Rows](#day-and-hour-header-rows)
- [Customizing Header Rows with Templates](#customizing-header-rows-with-templates)
- [Header Row Styling and CSS](#header-row-styling-and-css)
- [Header Rows with Resource Groups](#header-rows-with-resource-groups)
- [Combining Header Rows](#combining-header-rows)
- [Localization in Header Rows](#localization-in-header-rows)
- [Common Patterns](#common-patterns)

## Overview

Timeline views in the Scheduler can display multiple hierarchical header rows to provide context at different time scales. The `ScheduleHeaderRows` component allows you to configure Year, Month, Week, Date, and Hour rows in any combination, making it easy to display multi-level time hierarchies for better date navigation and appointment visualization.

**Header Row Hierarchy (largest to smallest):**
- Year (YYYY format)
- Month (MMM or MMMM format)
- Week (Week number or Wk XX)
- Date (DD or DDD format)
- Hour (HH:mm format)

## ScheduleHeaderRow Component

The `ScheduleHeaderRow` component defines individual header rows in the timeline view. It has properties for configuring row type and custom templates.

**ScheduleHeaderRow Properties:**
- `Option` (required) - HeaderRowType enum value (Year, Month, Week, Date, Hour)
- `Template` (optional) - Custom template for rendering header cell content
- Applies only to timeline views (TimelineDay, TimelineWeek, TimelineMonth, TimelineYear)

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleHeaderRows>
        <!-- Year header row -->
        <ScheduleHeaderRow Option="HeaderRowType.Year"></ScheduleHeaderRow>
        
        <!-- Month header row -->
        <ScheduleHeaderRow Option="HeaderRowType.Month"></ScheduleHeaderRow>
        
        <!-- Week header row -->
        <ScheduleHeaderRow Option="HeaderRowType.Week"></ScheduleHeaderRow>
        
        <!-- Date header row -->
        <ScheduleHeaderRow Option="HeaderRowType.Date"></ScheduleHeaderRow>
    </ScheduleHeaderRows>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineMonth"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Project Meeting", 
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
        public bool IsAllDay { get; set; }
    }
}
```

## HeaderRowType Options

The `HeaderRowType` enum provides these header row types:

| HeaderRowType | Timeline Views | Format | Description |
|---------------|----------------|--------|-------------|
| **Year** | All timeline views | YYYY | Displays year numbers (e.g., 2026) |
| **Month** | All timeline views | MMM or MMMM | Displays month names (e.g., March, Mar) |
| **Week** | TimelineDay, TimelineWeek, TimelineMonth | Wk XX | Displays ISO week numbers with "Wk" prefix |
| **Date** | All timeline views | DD or DDD | Displays day numbers (e.g., 24, Mon 24) |
| **Hour** | TimelineDay, TimelineWeek (NOT TimelineMonth) | HH:mm | Displays hour and minute slots (e.g., 09:00) |

**Important Notes:**
- Hour row only works with day/week views, NOT with month or year views
- Order matters: place header rows from largest to smallest time unit
- Multiple rows create a hierarchical view structure
- Each row type has built-in localization support

## Basic Header Row Setup

Start with a simple multi-level header configuration:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleHeaderRows>
        <ScheduleHeaderRow Option="HeaderRowType.Month"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Week"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Date"></ScheduleHeaderRow>
    </ScheduleHeaderRows>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineMonth" MaxEventsPerRow="3"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Team Standup", 
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 10, 30, 0) 
        },
        new AppointmentData 
        { 
            Id = 2, 
            Subject = "Sprint Planning", 
            StartTime = new DateTime(2026, 3, 25, 14, 0, 0), 
            EndTime = new DateTime(2026, 3, 25, 15, 30, 0) 
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**Result:** Three-level header showing Month → Week → Date hierarchy

## Year and Month Header Rows

Display long-range timelines with year and month grouping:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleHeaderRows>
        <ScheduleHeaderRow Option="HeaderRowType.Year"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Month"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Date"></ScheduleHeaderRow>
    </ScheduleHeaderRows>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineMonth" MaxEventsPerRow="4"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Q1 Review", 
            StartTime = new DateTime(2026, 3, 30, 9, 0, 0), 
            EndTime = new DateTime(2026, 3, 30, 11, 0, 0) 
        },
        new AppointmentData 
        { 
            Id = 2, 
            Subject = "Q2 Planning", 
            StartTime = new DateTime(2026, 4, 1, 10, 0, 0), 
            EndTime = new DateTime(2026, 4, 1, 12, 0, 0) 
        },
        new AppointmentData 
        { 
            Id = 3, 
            Subject = "Mid-year Check", 
            StartTime = new DateTime(2026, 6, 15, 14, 0, 0), 
            EndTime = new DateTime(2026, 6, 15, 15, 0, 0) 
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**Result:** Year 2026 → Months (March, April, etc.) → Individual dates

## Week Number Header Rows

Show ISO week numbers for better weekly organization:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleHeaderRows>
        <ScheduleHeaderRow Option="HeaderRowType.Month"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Week"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Date"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Hour"></ScheduleHeaderRow>
    </ScheduleHeaderRows>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineWeek" MaxEventsPerRow="6"></ScheduleView>
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
            StartTime = new DateTime(2026, 3, 23, 9, 0, 0), 
            EndTime = new DateTime(2026, 3, 23, 10, 0, 0) 
        },
        new AppointmentData 
        { 
            Id = 2, 
            Subject = "Wednesday Workshop", 
            StartTime = new DateTime(2026, 3, 25, 14, 0, 0), 
            EndTime = new DateTime(2026, 3, 25, 16, 0, 0) 
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**Result:** Month → Week (Wk 12) → Date → Hour hierarchy for detailed weekly view

## Displaying Complete Year Timeline

Show entire year in a single timeline with Interval=12:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleHeaderRows>
        <ScheduleHeaderRow Option="HeaderRowType.Year"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Month"></ScheduleHeaderRow>
    </ScheduleHeaderRows>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <!-- Interval=12 displays 12 months in single view -->
        <ScheduleView Option="View.TimelineMonth" MaxEventsPerRow="2" Interval="12"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Annual Conference", 
            StartTime = new DateTime(2026, 5, 15, 9, 0, 0), 
            EndTime = new DateTime(2026, 5, 17, 17, 0, 0) 
        },
        new AppointmentData 
        { 
            Id = 2, 
            Subject = "Year-end Review", 
            StartTime = new DateTime(2026, 12, 28, 10, 0, 0), 
            EndTime = new DateTime(2026, 12, 28, 12, 0, 0) 
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**Key Property:**
- `Interval="12"` in TimelineMonth view shows 12-month calendar

## Day and Hour Header Rows

Configure headers for day-level scheduling with hour granularity:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleHeaderRows>
        <ScheduleHeaderRow Option="HeaderRowType.Date"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Hour"></ScheduleHeaderRow>
    </ScheduleHeaderRows>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineDay" MaxEventsPerRow="8" Interval="1"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "9am Standup", 
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 9, 30, 0) 
        },
        new AppointmentData 
        { 
            Id = 2, 
            Subject = "11am Design Review", 
            StartTime = new DateTime(2026, 3, 24, 11, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 12, 0, 0) 
        },
        new AppointmentData 
        { 
            Id = 3, 
            Subject = "2pm Development", 
            StartTime = new DateTime(2026, 3, 24, 14, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 17, 0, 0) 
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**Result:** Date (Tuesday, March 24) → Hour rows (09:00, 10:00, 11:00, etc.)

## Customizing Header Rows with Templates

Use the Template property to customize header row display with custom text, formatting, or additional data:

```cshtml
@using Syncfusion.Blazor.Schedule
@using System.Globalization

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleHeaderRows>
        <ScheduleHeaderRow Option="HeaderRowType.Year">
            <Template>
                <div class="year-header">
                    <strong>Year: @(getYearText((context as TemplateContext).Date))</strong>
                </div>
            </Template>
        </ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Month">
            <Template>
                <div class="month-header">
                    @(getMonthText((context as TemplateContext).Date))
                </div>
            </Template>
        </ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Week">
            <Template>
                <div class="week-header">
                    Week @(getWeekNumber((context as TemplateContext).Date))
                </div>
            </Template>
        </ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Date">
            <Template>
                <div class="date-header">
                    <span>@(getDateText((context as TemplateContext).Date))</span>
                </div>
            </Template>
        </ScheduleHeaderRow>
    </ScheduleHeaderRows>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineMonth" MaxEventsPerRow="3"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    .year-header {
        padding: 8px 4px;
        font-size: 14px;
        background-color: #e3f2fd;
        font-weight: bold;
    }

    .month-header {
        padding: 6px 4px;
        font-size: 12px;
        background-color: #f3e5f5;
        font-weight: 500;
    }

    .week-header {
        padding: 4px 4px;
        font-size: 11px;
        background-color: #fff3e0;
        text-align: center;
    }

    .date-header {
        padding: 4px 4px;
        font-size: 10px;
        background-color: #f5f5f5;
    }
</style>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();

    private static string getYearText(DateTime date)
    {
        return date.ToString("yyyy", CultureInfo.InvariantCulture);
    }

    private static string getMonthText(DateTime date)
    {
        return date.ToString("MMMM", CultureInfo.InvariantCulture);
    }

    private static int getWeekNumber(DateTime date)
    {
        return CultureInfo.InvariantCulture.Calendar.GetWeekOfYear(
            date, 
            CalendarWeekRule.FirstFourDayWeek, 
            DayOfWeek.Monday);
    }

    private static string getDateText(DateTime date)
    {
        return date.ToString("dd ddd", CultureInfo.InvariantCulture);
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**Template Context:**
- Access via `(context as TemplateContext).Date`
- Provides the date for each header cell
- Use for formatting, calculations, or adding custom content

## Header Row Styling and CSS

Apply custom styles to header rows using CSS classes:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" CssClass="custom-header-rows" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleHeaderRows>
        <ScheduleHeaderRow Option="HeaderRowType.Month"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Week"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Date"></ScheduleHeaderRow>
    </ScheduleHeaderRows>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineMonth" MaxEventsPerRow="4"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    /* Overall header row styling */
    .custom-header-rows.e-schedule .e-header-cells {
        padding: 8px 4px;
        font-weight: 500;
    }

    /* Year header row */
    .custom-header-rows.e-schedule .e-header-row:nth-child(1) .e-header-cells {
        background-color: #1976d2;
        color: white;
        font-size: 14px;
        font-weight: bold;
    }

    /* Month header row */
    .custom-header-rows.e-schedule .e-header-row:nth-child(2) .e-header-cells {
        background-color: #0097a7;
        color: white;
        font-size: 12px;
    }

    /* Week header row */
    .custom-header-rows.e-schedule .e-header-row:nth-child(3) .e-header-cells {
        background-color: #388e3c;
        color: white;
        font-size: 11px;
    }

    /* Date header row */
    .custom-header-rows.e-schedule .e-header-row:nth-child(4) .e-header-cells {
        background-color: #f5f5f5;
        color: #333;
        border-top: 1px solid #ddd;
    }

    /* Hover effect */
    .custom-header-rows.e-schedule .e-header-cells:hover {
        background-color: rgba(0, 0, 0, 0.04);
    }
</style>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**CSS Targets:**
- `.e-header-cells` - Header row cells
- `.e-header-row:nth-child(n)` - Specific row layers
- Customize background, text color, font size, borders

## Header Rows with Resource Groups

Display header rows alongside grouped resources:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup Resources="@Resources"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int"
            DataSource="@ResourceDataList"
            Field="ResourceId"
            Title="Room"
            Name="Rooms"
            TextField="Name"
            IdField="Id"
            ColorField="Color">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleHeaderRows>
        <ScheduleHeaderRow Option="HeaderRowType.Month"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Week"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Date"></ScheduleHeaderRow>
    </ScheduleHeaderRows>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineMonth" MaxEventsPerRow="3"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] Resources = { "Rooms" };

    List<ResourceData> ResourceDataList = new List<ResourceData>
    {
        new ResourceData { Id = 1, Name = "Conference Room A", Color = "#ff9800" },
        new ResourceData { Id = 2, Name = "Conference Room B", Color = "#2196f3" },
        new ResourceData { Id = 3, Name = "Meeting Room C", Color = "#4caf50" }
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Team Meeting",
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0),
            ResourceId = 1
        },
        new AppointmentData
        {
            Id = 2,
            Subject = "Client Discussion",
            StartTime = new DateTime(2026, 3, 24, 14, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 15, 0, 0),
            ResourceId = 2
        },
        new AppointmentData
        {
            Id = 3,
            Subject = "Project Kickoff",
            StartTime = new DateTime(2026, 3, 25, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 25, 10, 30, 0),
            ResourceId = 3
        }
    };

    public class ResourceData
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
        public int ResourceId { get; set; }
    }
}
```

**Result:** Month → Week → Date headers display for each resource group

## Combining Header Rows

Create complex hierarchies by combining multiple header row types:

```cshtml
@using Syncfusion.Blazor.Schedule

<!-- Four-level hierarchy: Year → Month → Week → Date -->
<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleHeaderRows>
        <ScheduleHeaderRow Option="HeaderRowType.Year"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Month"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Week"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Date"></ScheduleHeaderRow>
    </ScheduleHeaderRows>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineMonth" MaxEventsPerRow="2"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<!-- Five-level hierarchy: Year → Month → Week → Date → Hour (for weeks) -->
<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleHeaderRows>
        <ScheduleHeaderRow Option="HeaderRowType.Month"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Week"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Date"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Hour"></ScheduleHeaderRow>
    </ScheduleHeaderRows>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineWeek" MaxEventsPerRow="4"></ScheduleView>
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
        public bool IsAllDay { get; set; }
    }
}
```

**Order Guidelines:**
- Always place largest time unit first
- Never reverse the hierarchy order
- Maximum 5 header rows recommended for clarity
- Excessive rows may reduce legibility

## Localization in Header Rows

Header rows automatically support localization based on current culture:

```cshtml
@using Syncfusion.Blazor.Schedule
@using System.Globalization

<div>
    Current Culture: @CultureInfo.CurrentCulture.Name
</div>

<SfSchedule TValue="AppointmentData" Height="650px" Locale="fr-FR" @bind-SelectedDate="@CurrentDate">
    <ScheduleHeaderRows>
        <ScheduleHeaderRow Option="HeaderRowType.Month"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Week"></ScheduleHeaderRow>
        <ScheduleHeaderRow Option="HeaderRowType.Date"></ScheduleHeaderRow>
    </ScheduleHeaderRows>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineMonth" MaxEventsPerRow="3"></ScheduleView>
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
        public bool IsAllDay { get; set; }
    }
}
```

**Localized Elements:**
- Month names adapt to locale (March, Marzo, Mars, etc.)
- Week day names localize (Monday, Lundi, Lunes, etc.)
- Number formats follow culture settings
- Set via `Locale` property on SfSchedule

## Common Patterns

### Pattern 1: Quarterly Planning View
Display year-long timeline with month grouping for quarterly reviews:

```cshtml
<ScheduleHeaderRows>
    <ScheduleHeaderRow Option="HeaderRowType.Year"></ScheduleHeaderRow>
    <ScheduleHeaderRow Option="HeaderRowType.Month"></ScheduleHeaderRow>
</ScheduleHeaderRows>
<ScheduleView Option="View.TimelineMonth" MaxEventsPerRow="2" Interval="12"></ScheduleView>
```

### Pattern 2: Weekly Project Timeline
Show week structure with daily dates and hours for task scheduling:

```cshtml
<ScheduleHeaderRows>
    <ScheduleHeaderRow Option="HeaderRowType.Month"></ScheduleHeaderRow>
    <ScheduleHeaderRow Option="HeaderRowType.Week"></ScheduleHeaderRow>
    <ScheduleHeaderRow Option="HeaderRowType.Date"></ScheduleHeaderRow>
    <ScheduleHeaderRow Option="HeaderRowType.Hour"></ScheduleHeaderRow>
</ScheduleHeaderRows>
<ScheduleView Option="View.TimelineWeek" MaxEventsPerRow="4"></ScheduleView>
```

### Pattern 3: Daily Hour-level Scheduling
Detailed day view with hourly slots for appointment booking:

```cshtml
<ScheduleHeaderRows>
    <ScheduleHeaderRow Option="HeaderRowType.Date"></ScheduleHeaderRow>
    <ScheduleHeaderRow Option="HeaderRowType.Hour"></ScheduleHeaderRow>
</ScheduleHeaderRows>
<ScheduleView Option="View.TimelineDay" MaxEventsPerRow="8"></ScheduleView>
```

### Pattern 4: Year Overview with Resources
Multi-resource Gantt-style view showing allocations across year:

```cshtml
<ScheduleGroup Resources="@Resources"></ScheduleGroup>
<ScheduleHeaderRows>
    <ScheduleHeaderRow Option="HeaderRowType.Year"></ScheduleHeaderRow>
    <ScheduleHeaderRow Option="HeaderRowType.Month"></ScheduleHeaderRow>
</ScheduleHeaderRows>
<ScheduleView Option="View.TimelineMonth" MaxEventsPerRow="2" Interval="12"></ScheduleView>
```

### Pattern 5: Custom-templated Headers with Event Counts
Use templates to display custom information in header cells:

```cshtml
<ScheduleHeaderRow Option="HeaderRowType.Date">
    <Template>
        <div>
            @((context as TemplateContext).Date.ToString("dd"))
            <span class="event-count" style="font-size: 9px;">events: N</span>
        </div>
    </Template>
</ScheduleHeaderRow>
```
