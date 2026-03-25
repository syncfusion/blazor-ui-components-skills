# Clipboard Operations

## Table of Contents
- [Overview](#overview)
- [Enabling Clipboard Feature](#enabling-clipboard-feature)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Copy, Cut, Paste with Keyboard](#copy-cut-paste-with-keyboard)
- [Context Menu Integration](#context-menu-integration)
- [Programmatic Methods](#programmatic-methods)
- [Modifying Content Before Pasting](#modifying-content-before-pasting)
- [Grid to Scheduler Clipboard Integration](#grid-to-scheduler-clipboard-integration)

## Overview

The Clipboard functionality in Syncfusion Scheduler enables users to cut, copy, and paste appointments efficiently. This feature eliminates repetitive data entry and allows quick schedule adjustments. The feature can be used via keyboard shortcuts or programmatically through public methods like `CopyAsync()`, `CutAsync()`, and `PasteAsync()`.

## Enabling Clipboard Feature

To activate clipboard functionality, set the `AllowClipboard` property to **true**. The `AllowKeyboardInteraction` property must also be enabled for proper keyboard shortcut functionality:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" 
    Height="650px" 
    @bind-SelectedDate="@SelectedDate" 
    AllowClipboard="true" 
    AllowKeyboardInteraction="true" 
    ShowQuickInfo="false">
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
    private DateTime SelectedDate { get; set; } = new DateTime(2026, 3, 24);

    public List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting with clients", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0), 
            IsAllDay = false 
        },
        new AppointmentData 
        { 
            Id = 2, 
            Subject = "Project Review", 
            StartTime = new DateTime(2026, 3, 24, 12, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 14, 0, 0), 
            IsAllDay = false 
        },
        new AppointmentData 
        { 
            Id = 3, 
            Subject = "Team Lunch", 
            StartTime = new DateTime(2026, 3, 25, 12, 0, 0), 
            EndTime = new DateTime(2026, 3, 25, 13, 0, 0), 
            IsAllDay = false 
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public string Location { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public string Description { get; set; }
        public bool IsAllDay { get; set; }
        public string RecurrenceRule { get; set; }
        public string RecurrenceException { get; set; }
        public Nullable<int> RecurrenceID { get; set; }
    }
}
```

## Keyboard Shortcuts

The Syncfusion Scheduler supports standard keyboard shortcuts for clipboard operations:

| Operation | Keyboard Shortcut | Mac Alternative | Description |
|-----------|-------------------|-----------------|-------------|
| Copy | Ctrl+C | Cmd+C | Duplicate selected appointment |
| Cut | Ctrl+X | Cmd+X | Move selected appointment to clipboard |
| Paste | Ctrl+V | Cmd+V | Insert clipboard appointment into selected time slot |

## Copy, Cut, Paste with Keyboard

To use keyboard shortcuts:

1. **Copy an appointment**: Click on the appointment → Press **Ctrl+C**
2. **Cut an appointment**: Click on the appointment → Press **Ctrl+X**
3. **Paste an appointment**: Click on desired time slot → Press **Ctrl+V**

For Mac users, replace **Ctrl** with **Cmd**.

**Example with keyboard interaction enabled:**

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" 
    Height="650px" 
    @bind-SelectedDate="@SelectedDate" 
    AllowClipboard="true" 
    AllowKeyboardInteraction="true" 
    ShowQuickInfo="false">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    private DateTime SelectedDate { get; set; } = new DateTime(2026, 3, 24);

    public List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting with clients", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        },
        new AppointmentData 
        { 
            Id = 2, 
            Subject = "Project Review", 
            StartTime = new DateTime(2026, 3, 24, 12, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 14, 0, 0) 
        },
        new AppointmentData 
        { 
            Id = 3, 
            Subject = "Status Update", 
            StartTime = new DateTime(2026, 3, 25, 15, 0, 0), 
            EndTime = new DateTime(2026, 3, 25, 16, 30, 0) 
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public string Location { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public string Description { get; set; }
        public bool IsAllDay { get; set; }
        public string RecurrenceRule { get; set; }
        public string RecurrenceException { get; set; }
        public Nullable<int> RecurrenceID { get; set; }
    }
}
```

## Context Menu Integration

Use context menus with `CopyAsync()`, `CutAsync()`, and `PasteAsync()` methods to provide clipboard operations programmatically:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Navigations

<div class="col-lg-12 control-section">
    <SfSchedule TValue="AppointmentData" @ref="ScheduleRef" 
        Height="650px" 
        @bind-SelectedDate="@SelectedDate" 
        AllowClipboard="true" 
        ShowQuickInfo="false">
        <ScheduleEventSettings DataSource="@EventDataSource"></ScheduleEventSettings>
        <ScheduleViews>
            <ScheduleView Option="View.Week"></ScheduleView>
            <ScheduleView Option="View.Day"></ScheduleView>
            <ScheduleView Option="View.Month"></ScheduleView>
        </ScheduleViews>
    </SfSchedule>
    
    <SfContextMenu TValue="MenuItem" Target=".e-schedule">
        <MenuItems>
            <MenuItem Text="Copy" Id="Copy" Hidden="@(!isEvent)" IconCss="e-icons e-copy"></MenuItem>
            <MenuItem Text="Cut" Id="Cut" Hidden="@(!isEvent)" IconCss="e-icons e-cut"></MenuItem>
            <MenuItem Text="Paste" Id="Paste" Hidden="@(!isCell)" IconCss="e-icons e-paste"></MenuItem>
        </MenuItems>
        <MenuEvents TValue="MenuItem" OnOpen="OnOpen" ItemSelected="OnItemSelected"></MenuEvents>
    </SfContextMenu>
</div>

@code {
    private DateTime SelectedDate { get; set; } = new DateTime(2026, 3, 24);
    private bool isCell;
    private bool isEvent;
    SfSchedule<AppointmentData> ScheduleRef;
    private AppointmentData EventData { get; set; }
    private CellClickEventArgs CellData { get; set; }
    private ElementInfo<AppointmentData> ElementData { get; set; }

    private List<AppointmentData> EventDataSource = new List<AppointmentData>()
    {   
        new AppointmentData
        {
            Id = 1,
            Subject = "Explosion of Betelgeuse Star",
            Location = "Space Centre USA",
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0),
            CategoryColor = "#1aaa55"
        },
        new AppointmentData
        {
            Id = 2,
            Subject = "Thule Air Crash Report",
            Location = "New York City",
            StartTime = new DateTime(2026, 3, 25, 12, 0, 0),
            EndTime = new DateTime(2026, 3, 25, 14, 0, 0),
            CategoryColor = "#357cd2"
        },
        new AppointmentData
        {
            Id = 3,
            Subject = "Blue Moon Eclipse",
            Location = "Space Centre USA",
            StartTime = new DateTime(2026, 3, 26, 9, 30, 0),
            EndTime = new DateTime(2026, 3, 26, 11, 0, 0),
            CategoryColor = "#7fa900"
        }
    };

    public async Task OnOpen(BeforeOpenCloseMenuEventArgs<MenuItem> args)
    {
        ElementData = await ScheduleRef.GetElementInfoAsync((int)args.Left, (int)args.Top);
        if (args.ParentItem == null && ElementData != null)
        {
            if (ElementData.ElementType == ElementType.Event)
            {
                EventData = ElementData.EventData;
                isCell = false;
                isEvent = true;
            }
            else if (ElementData.ElementType == ElementType.WorkCells || ElementData.ElementType == ElementType.DateHeader || 
                ElementData.ElementType == ElementType.AllDayCells)
            {
                isCell = true;
                isEvent = false;
                CellData = new CellClickEventArgs
                {
                    StartTime = ElementData.StartTime,
                    EndTime = ElementData.EndTime,
                    IsAllDay = ElementData.IsAllDay
                };
            }
            else
            {
                args.Cancel = true;
            }
        }
    }

    public async Task OnItemSelected(MenuEventArgs<MenuItem> args)
    {
        var SelectedMenuItem = args.Item.Id;
        switch (SelectedMenuItem)
        {
            case "Copy":
                await CopyEvent(EventData);
                break;
            case "Cut":
                await CutEvent(EventData);
                break;
            case "Paste":
                if (isCell)
                {
                    await PasteEvent(CellData);
                }
                break;
        }
    }

    private async Task PasteEvent(CellClickEventArgs cellData)
    {
        await ScheduleRef.PasteAsync(cellData);
    }

    private async Task CopyEvent(AppointmentData eventData)
    {
        await ScheduleRef.CopyAsync(eventData);
    }

    private async Task CutEvent(AppointmentData eventData)
    {
        await ScheduleRef.CutAsync(eventData);
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public string Location { get; set; }
        public string Description { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public Nullable<bool> IsAllDay { get; set; }
        public string CategoryColor { get; set; }
        public string RecurrenceRule { get; set; }
        public Nullable<int> RecurrenceID { get; set; }
        public string RecurrenceException { get; set; }
    }
}
```

## Programmatic Methods

Use these public methods to manage clipboard operations programmatically:

| Method | Parameters | Description |
|--------|-----------|-------------|
| `CopyAsync()` | None | Duplicate the selected appointment for reuse |
| `CutAsync()` | None | Remove the selected appointment for moving |
| `PasteAsync(CellClickEventArgs)` | targetElement | Insert clipboard appointment into specified time slot |

## Modifying Content Before Pasting

Use the `Paste` event to intercept and modify appointment details before they are pasted into the scheduler:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Grids

<div class="container">
    <div class="row">
        <div class="col-lg-6">
            <SfSchedule TValue="AppointmentData" @ref="ScheduleRef" 
                Height="550px" 
                Width="100%" 
                @bind-SelectedDate="@CurrentDate" 
                AllowClipboard="true" 
                ShowQuickInfo="false">
                <ScheduleEvents TValue="AppointmentData" Paste="OnBeforePaste"></ScheduleEvents>
                <ScheduleEventSettings DataSource="@ScheduleData"></ScheduleEventSettings>
                <ScheduleViews>
                    <ScheduleView Option="View.Day"></ScheduleView>
                    <ScheduleView Option="View.Week"></ScheduleView>
                    <ScheduleView Option="View.WorkWeek"></ScheduleView>
                    <ScheduleView Option="View.Month"></ScheduleView>
                </ScheduleViews>
            </SfSchedule>
        </div>
        <div class="col-lg-6">
            <SfGrid @ref="GridRef" DataSource="@GridData" AllowSelection="true" Width="100%">
                <GridColumns>
                    <GridColumn Field="OrderID" HeaderText="Order ID" Width="90" TextAlign="TextAlign.Right"></GridColumn>
                    <GridColumn Field="CustomerID" HeaderText="Customer ID" Width="100"></GridColumn>
                    <GridColumn Field="ShipCity" HeaderText="Ship City" Width="100"></GridColumn>
                    <GridColumn Field="ShipName" HeaderText="Ship Name" Width="130"></GridColumn>
                    <GridColumn Field="OrderDate" HeaderText="Order Date" Width="100" Format="d" Type="ColumnType.Date"></GridColumn>
                </GridColumns>
            </SfGrid>
        </div>
    </div>
</div>

@code {
    private DateTime CurrentDate { get; set; } = new DateTime(2026, 3, 24);
    private SfSchedule<AppointmentData> ScheduleRef;
    private SfGrid<OrderData> GridRef;

    private List<AppointmentData> ScheduleData { get; set; } = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 30, 0),
            Location = "Conference Room",
            Description = "Monthly Status Update"
        },
        new AppointmentData 
        { 
            Id = 2, 
            Subject = "Team Lunch", 
            StartTime = new DateTime(2026, 3, 25, 12, 0, 0),
            EndTime = new DateTime(2026, 3, 25, 13, 0, 0),
            Location = "Cafeteria",
            Description = "Team Building"
        }
    };

    private List<OrderData> GridData { get; set; } = new List<OrderData>
    {
        new OrderData 
        { 
            OrderID = 10248, 
            CustomerID = "VINET", 
            ShipName = "Vins et alcools Chevalier", 
            ShipCity = "Reims", 
            ShipAddress = "59 rue de l Abbaye",
            ShipPostalCode = "51100", 
            ShipCountry = "France", 
            OrderDate = new DateTime(2026, 3, 1) 
        },
        new OrderData 
        { 
            OrderID = 10249, 
            CustomerID = "TOMSP", 
            ShipName = "Toms Spezialitäten", 
            ShipCity = "Münster", 
            ShipAddress = "Luisenstr. 48",
            ShipPostalCode = "44087", 
            ShipCountry = "Germany", 
            OrderDate = new DateTime(2026, 3, 2) 
        },
        new OrderData 
        { 
            OrderID = 10250, 
            CustomerID = "HANAR", 
            ShipName = "Hanari Carnes", 
            ShipCity = "Rio de Janeiro", 
            ShipAddress = "Rua do Paço, 67",
            ShipPostalCode = "05454-876", 
            ShipCountry = "Brazil", 
            OrderDate = new DateTime(2026, 3, 3) 
        }
    };

    public async Task OnBeforePaste(PasteEventArgs<AppointmentData> args)
    {
        // Intercept clipboard text and parse grid data
        if (args.ClipboardText is string stringData)
        {
            string[] dataArray = stringData.Split('\t');
            if (dataArray.Length >= 5)
            {
                int id;
                if (int.TryParse(dataArray[0], out id))
                {
                    DateTime orderDate;
                    DateTime.TryParse(dataArray[4], out orderDate);

                    // Transform grid data to appointment
                    args.Data = new List<AppointmentData> {
                        new AppointmentData {
                            Id = id,
                            Subject = $"{dataArray[1]}",
                            StartTime = orderDate,
                            EndTime = orderDate.AddHours(1),
                            Location = dataArray[2],
                            Description = $"Order from {dataArray[3]}"
                        }
                    };
                }
            }
        }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public string Location { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public string Description { get; set; }
        public bool IsAllDay { get; set; }
        public string RecurrenceRule { get; set; }
        public string RecurrenceException { get; set; }
        public int? RecurrenceID { get; set; }
    }

    public class OrderData
    {
        public int OrderID { get; set; }
        public string CustomerID { get; set; }
        public string ShipName { get; set; }
        public string ShipCity { get; set; }
        public string ShipAddress { get; set; }
        public string ShipPostalCode { get; set; }
        public string ShipCountry { get; set; }
        public DateTime OrderDate { get; set; }
    }
}
```

## Grid to Scheduler Clipboard Integration

The example above demonstrates how to copy data from a Syncfusion Grid and paste it as appointments in the Scheduler:

**Steps:**
1. Select a row in the grid (Grid data becomes clipboard text via copy operation)
2. Press **Ctrl+C** to copy the grid row
3. Click on a time slot in the scheduler
4. Press **Ctrl+V** to paste as a new appointment
5. The `Paste` event transforms grid data into appointment format

The `Paste` event receives the clipboard text and the `OnBeforePaste` handler converts tab-separated grid data into appointment objects with appropriate date/time assignments.

## Common Patterns

**When to use keyboard shortcuts:**
- Quick copy-paste operations by end users
- Duplicating nearby appointments
- Simple appointment management

**When to use programmatic methods:**
- Context menu integration with custom UI
- Advanced validation before paste
- Transforming data from external sources
- Batch clipboard operations

**Data transformation in Paste event:**
- Convert grid/table data to appointments
- Adjust dates/times based on target slot
- Add metadata or location information
- Validate before pasting

**Mac vs Windows:**
- Use **Cmd** instead of **Ctrl** for Mac users
- JavaScript interop can detect platform and adapt
