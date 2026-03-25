# Header Bar and Toolbar Customization

## Table of Contents
- [Overview](#overview)
- [Show or Hide Header Bar](#show-or-hide-header-bar)
- [Toolbar Customization](#toolbar-customization)
- [Built-in Toolbar Items](#built-in-toolbar-items)
- [Custom Toolbar Items](#custom-toolbar-items)
- [Configuring Custom Toolbar](#configuring-custom-toolbar)
- [Adaptive UI](#adaptive-ui)
- [Date Header Customization](#date-header-customization)
- [Date Header with OnRenderCell](#date-header-with-onrendercell)
- [Customize Date Range Text](#customize-date-range-text)
- [TimelineYear Header Customization](#timelineyear-header-customization)
- [Header Indent Cells Customization](#header-indent-cells-customization)

## Overview

The Scheduler header bar provides navigation, date control, and view selection. You can customize the header to include built-in controls, add custom buttons/inputs, or completely hide it. The header supports adaptive UI for responsive layouts and advanced templating for date/range customization.

## Show or Hide Header Bar

Control header bar visibility using the `ShowHeaderBar` property:

```cshtml
@using Syncfusion.Blazor.Schedule

<!-- Show header (default) -->
<SfSchedule TValue="AppointmentData" Height="550px" ShowHeaderBar="true" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<!-- Hide header -->
<SfSchedule TValue="AppointmentData" Height="550px" ShowHeaderBar="false" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
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

**ShowHeaderBar Property:**
- `true` (default) - Displays date navigation and view selector
- `false` - Hides header bar completely

## Toolbar Customization

Customize the Scheduler toolbar using the `ScheduleToolBar` component. This enables comprehensive control over toolbar layout, including built-in controls and custom elements.

### Basic Toolbar with Built-in Items

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Navigations

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleToolBar>
        <ScheduleToolBarPrevious />
        <ScheduleToolBarNext />
        <ScheduleToolBarDateRange />
        <ScheduleToolBarToday />
        <ScheduleToolBarViews />
    </ScheduleToolBar>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
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
            Subject = "Meeting", 
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

## Built-in Toolbar Items

The Scheduler provides these built-in toolbar components:

| Item | Component | Description |
|------|-----------|-------------|
| Previous | `ScheduleToolBarPrevious` | Navigate to previous date range |
| Next | `ScheduleToolBarNext` | Navigate to next date range |
| Date Range | `ScheduleToolBarDateRange` | Display current visible date range |
| Today | `ScheduleToolBarToday` | Jump to today's date |
| Views | `ScheduleToolBarViews` | View-switching buttons for configured views |
| New Event | `ScheduleToolBarNewEvent` | "Add" button for new appointments (mobile/adaptive UI) |

## Custom Toolbar Items

Add custom elements to the toolbar using `ScheduleToolBarCustom` with different item types:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleToolBar>
        <ScheduleToolBarPrevious />
        <ScheduleToolBarNext />
        <ScheduleToolBarDateRange />
        <ScheduleToolBarCustom Type="ItemType.Spacer" />
        <ScheduleToolBarCustom Type="ItemType.Button">
            <SfButton Content="Export" OnClick="OnExport"></SfButton>
        </ScheduleToolBarCustom>
        <ScheduleToolBarCustom Type="ItemType.Separator" />
        <ScheduleToolBarCustom Type="ItemType.Button">
            <SfButton Content="Print" OnClick="OnPrint"></SfButton>
        </ScheduleToolBarCustom>
        <ScheduleToolBarCustom Type="ItemType.Spacer" />
        <ScheduleToolBarToday />
    </ScheduleToolBar>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();

    private void OnExport()
    {
        // Export logic
    }

    private void OnPrint()
    {
        // Print logic
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

**ItemType Options for ScheduleToolBarCustom:**
- `ItemType.Button` - Renders as clickable button (default)
- `ItemType.Input` - Renders input controls (dropdowns, textboxes)
- `ItemType.Spacer` - Adds adaptive space, pushes items to right
- `ItemType.Separator` - Adds vertical divider line between items

## Configuring Custom Toolbar

Create advanced custom toolbar with input controls like dropdowns for filtering:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.DropDowns
@using Syncfusion.Blazor.Data

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings Query="@EventQuery" DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleResources>
        <ScheduleResource TItem="OwnerData" TValue="int" 
            Field="OwnerId" 
            Title="Owner" 
            Name="Owners" 
            AllowMultiple="false"
            DataSource="@OwnerCollections" 
            TextField="OwnerText" 
            IdField="OwnerId" 
            ColorField="Color" 
            Query="@EventQuery">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleToolBar>
        <ScheduleToolBarPrevious />
        <ScheduleToolBarNext />
        <ScheduleToolBarDateRange />
        <ScheduleToolBarCustom Type="ItemType.Spacer" />
        <ScheduleToolBarCustom Type="ItemType.Input">
            <SfDropDownList TValue="int" TItem="OwnerData" 
                @bind-Value="@SelectedOwner" 
                ShowClearButton="false" 
                Width="150px" 
                DataSource="@OwnerCollections"
                Placeholder="Select Owner">
                <DropDownListFieldSettings Text="OwnerText" Value="OwnerId"></DropDownListFieldSettings>
                <DropDownListEvents TValue="int" TItem="OwnerData" ValueChange="OnOwnerChange"></DropDownListEvents>
            </SfDropDownList>
        </ScheduleToolBarCustom>
        <ScheduleToolBarCustom Type="ItemType.Spacer" />
        <ScheduleToolBarToday />
    </ScheduleToolBar>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    private DateTime CurrentDate = new DateTime(2026, 3, 24);
    private Query EventQuery { get; set; }
    private List<OwnerData> OwnerCollections = new List<OwnerData>();
    private List<AppointmentData> DataSource = new List<AppointmentData>();
    private int SelectedOwner = 1;

    protected override void OnInitialized()
    {
        OwnerCollections = new List<OwnerData>
        {
            new OwnerData { OwnerId = 1, OwnerText = "Nancy", Color = "#ea7a57" },
            new OwnerData { OwnerId = 2, OwnerText = "Steven", Color = "#7fa900" },
            new OwnerData { OwnerId = 3, OwnerText = "Michael", Color = "#5978ee" }
        };

        DataSource = new List<AppointmentData>
        {
            new AppointmentData
            {
                Id = 1,
                Subject = "Project Meeting",
                StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
                EndTime = new DateTime(2026, 3, 24, 10, 0, 0),
                OwnerId = 1
            },
            new AppointmentData
            {
                Id = 2,
                Subject = "Design Discussion",
                StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
                EndTime = new DateTime(2026, 3, 24, 11, 0, 0),
                OwnerId = 2
            },
            new AppointmentData
            {
                Id = 3,
                Subject = "Budget Meeting",
                StartTime = new DateTime(2026, 3, 24, 11, 0, 0),
                EndTime = new DateTime(2026, 3, 24, 12, 0, 0),
                OwnerId = 3
            }
        };

        EventQuery = new Query().Where("OwnerId", "equal", SelectedOwner);
    }

    private void OnOwnerChange(ChangeEventArgs<int, OwnerData> args)
    {
        SelectedOwner = args.Value;
        // Update EventQuery to filter appointments by selected owner
        EventQuery = new Query().Where("OwnerId", "equal", SelectedOwner);
    }

    public class OwnerData
    {
        public int OwnerId { get; set; }
        public string OwnerText { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
        public int OwnerId { get; set; }
    }
}
```

## Adaptive UI

Move view options to a popup menu for responsive/mobile layouts using `EnableAdaptiveUI`:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" EnableAdaptiveUI="true" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
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
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
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

**EnableAdaptiveUI Benefits:**
- `true` - Moves view buttons to popup menu, adds "New Event" button
- Mobile-friendly responsive layout
- Consolidates toolbar items for limited screen space

## Date Header Customization

Customize the date header cells using `DateHeaderTemplate`:

```cshtml
@using Syncfusion.Blazor.Schedule
@using System.Globalization

<SfSchedule TValue="AppointmentData" Height="650px" CssClass="date-header-custom" @bind-SelectedDate="@CurrentDate">
    <ScheduleTemplates>
        <DateHeaderTemplate>
            <div class="date-text">@(getDateHeaderText((context as TemplateContext).Date))</div>
            @{
                @switch ((int)(context as TemplateContext).Date.DayOfWeek)
                {
                    case 0: // Sunday
                        <div class="weather-text">25°C</div>
                        break;
                    case 1: // Monday
                        <div class="weather-text">18°C</div>
                        break;
                    case 2: // Tuesday
                        <div class="weather-text">10°C</div>
                        break;
                    case 3: // Wednesday
                        <div class="weather-text">16°C</div>
                        break;
                    case 4: // Thursday
                        <div class="weather-text">8°C</div>
                        break;
                    case 5: // Friday
                        <div class="weather-text">27°C</div>
                        break;
                    case 6: // Saturday
                        <div class="weather-text">17°C</div>
                        break;
                }
            }
        </DateHeaderTemplate>
    </ScheduleTemplates>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    public static string getDateHeaderText(DateTime date)
    {
        return date.ToString("dd ddd", CultureInfo.CurrentCulture);
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

<style>
    .date-header-custom.e-schedule .e-vertical-view .e-header-cells {
        padding: 0;
        text-align: center;
    }

    .date-header-custom.e-schedule .date-text {
        font-size: 14px;
        font-weight: bold;
        padding: 5px;
    }

    .date-header-custom.e-schedule .weather-text {
        font-size: 11px;
        color: #666;
    }
</style>
```

**DateHeaderTemplate Usage:**
- Available for Day, Week, WorkWeek views
- Template context provides `Date` property
- Can add custom content per date cell

## Date Header with OnRenderCell

Customize date headers dynamically using `OnRenderCell` event with `ElementType.DateHeader`:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" OnRenderCell="OnRenderCell"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    .e-schedule .e-vertical-view .e-date-header-wrap table tbody td.e-header-cells {
        background-color: #e8f4f8;
        font-weight: bold;
    }

    .e-schedule .e-date-header-wrap table tbody td.e-header-cells.weekend {
        background-color: #f0e8f8;
    }
</style>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0) 
        }
    };

    public void OnRenderCell(RenderCellEventArgs args)
    {
        // Customize date header cells
        if (args.ElementType == ElementType.DateHeader)
        {
            // Add custom CSS class based on day of week
            if (args.Data.DayOfWeek == DayOfWeek.Saturday || args.Data.DayOfWeek == DayOfWeek.Sunday)
            {
                args.CssClasses = new List<string> { "weekend" };
            }
        }
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

**ElementType Values:**
- `ElementType.DateHeader` - Date header cells in day/week views
- `ElementType.WorkCells` - Work hour cells
- `ElementType.AllDayCells` - All-day cells

## Customize Date Range Text

Customize the date range text displayed in the header using `DateRangeTemplate`:

```cshtml
@using Syncfusion.Blazor.Schedule
@using System.Globalization

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleTemplates>
        <DateRangeTemplate>
            <div class="date-range-text">
                From: @((context as DateRangeTemplateContext).StartDate.ToString("dddd, MMMM dd, yyyy", CultureInfo.CurrentCulture))
                <br />
                To: @((context as DateRangeTemplateContext).EndDate.ToString("dddd, MMMM dd, yyyy", CultureInfo.CurrentCulture))
            </div>
        </DateRangeTemplate>
    </ScheduleTemplates>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}

<style>
    .date-range-text {
        font-size: 12px;
        line-height: 1.5;
        color: #555;
    }
</style>
```

**DateRangeTemplate Context:**
- `StartDate` - First date in current view
- `EndDate` - Last date in current view
- `CurrentView` - Active view type

## TimelineYear Header Customization

Customize day and month headers in TimelineYear view using `DayHeaderTemplate` and `MonthHeaderTemplate`:

```cshtml
@using Syncfusion.Blazor.Schedule
@using System.Globalization

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleTemplates>
        <DayHeaderTemplate>
            <div class="day-header">@(getDayHeaderText((context as TemplateContext).Date))</div>
        </DayHeaderTemplate>
        <MonthHeaderTemplate>
            <div class="month-header">@(getMonthHeaderText((context as TemplateContext).Date))</div>
        </MonthHeaderTemplate>
    </ScheduleTemplates>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineYear" 
            MaxEventsPerRow="2" 
            Orientation="Orientation.Vertical" 
            DisplayName="Vertical Year">
        </ScheduleView>
        <ScheduleView Option="View.TimelineYear" 
            MaxEventsPerRow="2" 
            Orientation="Orientation.Horizontal" 
            DisplayName="Horizontal Year">
        </ScheduleView>
    </ScheduleViews>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    public static string getDayHeaderText(DateTime date)
    {
        return date.ToString("dddd", CultureInfo.InvariantCulture);
    }

    public static string getMonthHeaderText(DateTime date)
    {
        return date.ToString("MMM", CultureInfo.InvariantCulture);
    }

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Conference", 
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0), 
            EndTime = new DateTime(2026, 3, 25, 0, 0, 0) 
        },
        new AppointmentData 
        { 
            Id = 2, 
            Subject = "Seminar", 
            StartTime = new DateTime(2026, 5, 1, 9, 30, 0), 
            EndTime = new DateTime(2026, 5, 1, 12, 0, 0) 
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

**TimelineYear Templates:**
- `DayHeaderTemplate` - Customizes day headers (e.g., "Mon", "Tue")
- `MonthHeaderTemplate` - Customizes month headers (e.g., "Jan", "Feb")
- Works in both Vertical and Horizontal orientations

## Header Indent Cells Customization

Customize header indent cells (left-side header area) using `HeaderIndentTemplate`:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup Resources="@Resources"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int" 
            DataSource="@OwnersData" 
            Field="OwnerId" 
            Title="Owner" 
            Name="Owners" 
            TextField="OwnerText" 
            IdField="Id" 
            ColorField="OwnerColor">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleTemplates>
        <HeaderIndentTemplate>
            <div class="e-resource-text">
                <div class="resource-label">Owners</div>
            </div>
        </HeaderIndentTemplate>
    </ScheduleTemplates>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineWeek"></ScheduleView>
        <ScheduleView Option="View.TimelineMonth"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    .e-schedule .e-timeline-view .e-resource-left-td {
        vertical-align: bottom;
    }

    .e-schedule .e-timeline-view .e-resource-left-td .e-resource-text {
        font-weight: 500;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .e-schedule .e-timeline-view .e-resource-left-td .resource-label {
        border-right: 1px solid rgba(0, 0, 0, 0.12);
        padding: 10px;
        font-size: 14px;
    }
</style>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] Resources { get; set; } = { "Owners" };

    public List<ResourceData> OwnersData { get; set; } = new List<ResourceData>
    {
        new ResourceData { OwnerText = "Nancy", Id = 1, OwnerColor = "#ffaa00" },
        new ResourceData { OwnerText = "Steven", Id = 2, OwnerColor = "#f8a398" },
        new ResourceData { OwnerText = "Michael", Id = 3, OwnerColor = "#7499e1" }
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0), 
            OwnerId = 1 
        }
    };

    public class ResourceData
    {
        public int Id { get; set; }
        public string OwnerText { get; set; }
        public string OwnerColor { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
        public int OwnerId { get; set; }
    }
}
```

**HeaderIndentTemplate Usage:**
- Customizes the left indent header area
- Appears in timeline views and resource-grouped views
- Displays custom text or labels

## Common Patterns

### Header with Custom Styling
Use CSS classes to style header elements (date label, navigation buttons, view selector)

### Resource Filtering via Toolbar
Implement dropdown in custom toolbar to filter appointments by resource

### Responsive Header
Enable adaptive UI for mobile devices to consolidate toolbar items

### Multi-level Headers
Use TimelineYear with DayHeaderTemplate and MonthHeaderTemplate for calendar-like display

### Header Label Customization
Use DateRangeTemplate to override default date range display format
