# Recurring Events in Blazor Scheduler Component

## Table of Contents
- [Recurrence Options and Rules](#recurrence-options-and-rules)
- [Recurrence Properties](#recurrence-properties)
- [Creating a Recurring Event](#creating-a-recurring-event)
- [Adding Exceptions](#adding-exceptions)
- [Editing an Occurrence from a Series](#editing-an-occurrence-from-a-series)
- [Edit/Delete Following Recurrence Events](#editdelete-following-recurrence-events)
- [Daily Frequency](#daily-frequency)
- [Weekly Frequency](#weekly-frequency)
- [Monthly Frequency](#monthly-frequency)
- [Yearly Frequency](#yearly-frequency)
- [Recurrence Validation](#recurrence-validation)

It represents an appointment that is created for a certain time interval and occurring repeatedly on a daily, weekly, monthly or yearly basis at the same time interval based on the provided recurrence rule. Usually, the recurring events are indicated by a repeat marker added at the bottom-right position.

**Note:** Set `RecurrenceRule` property to create recurring events.

## Recurrence Options and Rules

Events can be repeated on a daily, weekly, monthly or yearly basis based on the recurrence rule which accepts the string value. The following details should be assigned to the `RecurrenceRule` property to generate the recurring instances.

* Repeat type - daily/weekly/monthly/yearly.
* How many times it needs to be repeated?
* The interval duration.
* The time period to render the appointment, etc.

There are four repeat types available namely:

* **Daily** - Creates the recurring instances on daily basis.
* **Weekly** - Creates the recurring instances on weekly basis for the selected days.
* **Monthly** - Creates the recurring instances on monthly basis for the selected months and other provided recurrence criteria.
* **Yearly** - Creates the recurring instances on yearly basis.

## Recurrence Properties

Recurrence properties define how appointments repeat. Refer to [iCalendar](https://datatracker.ietf.org/doc/html/rfc5545#section-3.3.10) specifications for valid recurrence rule strings.

| Property | Purpose | Example |
|-------|---------| --------- |
| FREQ | Maintains the repeat type (Daily, Weekly, Monthly, Yearly) value of the appointment. | FREQ=DAILY;INTERVAL=1|
| INTERVAL | Maintains the interval value of the appointments. When the daily appointment is created at an interval of 2, the appointments are rendered on the days Monday, Wednesday and Friday (Creates an appointment on all days by leaving the interval of one day gap). | FREQ=DAILY;INTERVAL=2|
| COUNT | It holds the appointment's count value. When the COUNT value is 10, then 10 instances of appointments are created in the recurrence series. | FREQ=DAILY;INTERVAL=1;COUNT=10|
| UNTIL | This property holds the end date value (in ISO format) denoting when the recurrence actually ends. | FREQ=DAILY;INTERVAL=1;UNTIL=20200530T041343Z;|
| BYDAY | It holds the day value(s), representing on which the appointments actually renders. Create the weekly appointment, and select the day(s) from the day options (Monday/Tuesday/Wednesday/Thursday/Friday/Saturday/Sunday). When Monday is selected, the first two letters of the selected day "MO" is saved in the BYDAY property. When multiple days are selected, the values are separated by commas. | FREQ=WEEKLY;INTERVAL=1;BYDAY=MO,WE;COUNT=10|
| BYMONTHDAY | This property is used to store the date value of the Month, while creating the Month recurrence appointment. When a Monthly recurrence appointment is created for every 3rd day of the month, then BYMONTHDAY holds the value 3 and creates the appointment on 3rd day of every month. | FREQ=MONTHLY;BYMONTHDAY=3;INTERVAL=1;COUNT=10|
| BYMONTH | This property is used to store the index value of the selected Month while creating the yearly appointments. When the yearly appointment is created on June month, the index value of June month 6 will get stored in the BYMONTH field. The appointment is created on every 6th month of a year. | FREQ=YEARLY;BYMONTHDAY=16;BYMONTH=6;INTERVAL=1;COUNT=10|
| BYSETPOS | This property is used to store the index value of the week. When the monthly appointment is created in second week of a month, the index value of the second week (2) is stored in BYSETPOS. | FREQ=MONTHLY;BYDAY=MO;BYSETPOS=2;COUNT=10|

**Note:** The default recurrence related validation has been included for recurrence appointments similar to the one available in Outlook. The validation usually occurs during the recurrence appointment creation, editing, drag and drop or resizing of the recurrence appointments and also if any single occurrence changes.

## Creating a Recurring Event

The following example depicts how to create a recurring event on Scheduler with the specific recurrence rule. In the following example, an event is made to repeat on daily mode and ends after 5 occurrences.

```cshtml
@code{
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2020, 1, 6, 9, 30, 0), 
            EndTime = new DateTime(2020, 1, 6, 11, 0, 0),
            RecurrenceRule = "FREQ=DAILY;INTERVAL=1;COUNT=5"  // Repeats daily, 5 times
        }
    };
}
```

## Adding Exceptions

A few instance of the recurrence series can be excluded on specific dates, by adding those exceptional dates to the `RecurrenceException` field. These date values should be given in the ISO date time format with no hyphens(-) separating the date elements.

For example, 7th January 2020 can be represented as 20200107. Also, the time part being represented in UTC format needs to add "Z" after the time portion with no space. "09:30 AM" is therefore represented as "040000Z".

```cshtml
@code{
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2020, 1, 6, 9, 30, 0), 
            EndTime = new DateTime(2020, 1, 6, 11, 0, 0),
            RecurrenceRule = "FREQ=DAILY;INTERVAL=1;COUNT=5", 
            RecurrenceException = "20200107T040000Z,20200109T040000Z"  // Excludes Jan 7 and Jan 9
        }
    };
}
```

## Editing an Occurrence from a Series

To edit a single occurrence, add it as a new event with `RecurrenceID` pointing to the parent event's ID. Add the occurrence date to the parent's `RecurrenceException` field to exclude it from the series.

```cshtml
@code{
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { 
            Id = 1, 
            Subject = "Scrum Meeting", 
            StartTime = new DateTime(2020, 1, 28, 9, 30, 0), 
            EndTime = new DateTime(2020, 1, 28, 11, 0, 0),
            RecurrenceRule = "FREQ=DAILY;INTERVAL=1;COUNT=5", 
            RecurrenceException = "20200130T040000Z"  // Excludes Jan 30 from series
        },
        new AppointmentData { 
            Id = 2, 
            Subject = "Scrum Meeting Rescheduled", 
            StartTime = new DateTime(2020, 1, 30, 10, 30, 0), 
            EndTime = new DateTime(2020, 1, 30, 12, 0, 0), 
            RecurrenceID = 1  // Links to parent event (Id=1)
        }
    };
}
```

## Edit/Delete Following Recurrence Events

Enable editing or deleting of following recurrence events by setting `AllowEditFollowingEvents="true"`. When a recurrence event is edited or deleted with the "following events" option, those events become a separate series and changes don't reflect to the parent series.

**Note:** Set `AllowEditFollowingEvents="true"` in `ScheduleEventSettings` to enable this feature.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData">
    <ScheduleEventSettings DataSource="@DataSource" AllowEditFollowingEvents="true"></ScheduleEventSettings>
</SfSchedule>

@code{
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2020, 3, 9, 9, 30, 0), 
            EndTime = new DateTime(2020, 3, 9, 11, 0, 0), 
            RecurrenceRule = "FREQ=DAILY;INTERVAL=1;COUNT=5" 
        }
    };
}
```

## Daily Frequency

| Description | Example |
|-------------|---------|
| Daily recurring event that never ends | FREQ=DAILY; INTERVAL=1 |
| Daily recurring event that ends after 5 occurrences | FREQ=DAILY; INTERVAL=1; COUNT=5 |
| Daily recurring event that ends exactly on 12/12/2020 | FREQ=DAILY; INTERVAL=1; UNTIL=20201212T041343Z |
| Daily event that recurs on alternative days and repeats for 10 occurrences | FREQ=DAILY; INTERVAL=2; COUNT=10 |

## Weekly Frequency

| Description | Example |
|-------------|---------|
| Weekly recurring event that repeats on every Monday, Wednesday and Friday and never ends | FREQ=WEEKLY; INTERVAL=1; BYDAY=MO,WE,FR |
| Repeats every week Thursday and ends after 10 occurrences | FREQ=WEEKLY; INTERVAL=1; BYDAY=TH; COUNT=10 |
| Repeats every week Monday and ends on 12/12/2020 | FREQ=WEEKLY; INTERVAL=1; BYDAY=MO; UNTIL=20201212T041343Z |
| Repeats on Monday, Wednesday and Friday of alternative weeks and ends after 10 occurrences | FREQ=WEEKLY; INTERVAL=2; BYDAY=MO, WE, FR; COUNT=10 |

## Monthly Frequency

| Description | Example |
|-------------|---------|
| Monthly recurring event that repeats on every 15th day of a month and never ends | FREQ=MONTHLY; BYMONTHDAY=15; INTERVAL=1 |
| Monthly recurring event that repeats on every 16th day of a month and ends after 10 occurrences | FREQ=MONTHLY; BYMONTHDAY=16; INTERVAL=1; COUNT=10 |
| Repeats every 17th day of a month and ends on 12/12/2020 | FREQ=MONTHLY; BYMONTHDAY=17; INTERVAL=1; UNTIL=20201212T041343Z |
| Repeats every 2nd Friday of a month and never ends | FREQ=MONTHLY; BYDAY=FR; BYSETPOS=2; INTERVAL=1 |
| Repeats every 4th Wednesday of a month and ends after 10 occurrences | FREQ=MONTHLY; BYDAY=WE; BYSETPOS=4; INTERVAL=1; COUNT=10 |
| Repeats every 4th Friday of a month and ends on 12/12/2020 | FREQ=MONTHLY; BYDAY=FR; BYSETPOS=4; INTERVAL=1; UNTIL=20201212T041343Z; |

## Yearly Frequency

| Description | Example |
|-------------|---------|
| Yearly event that repeats on every 15th day of December month and never ends | FREQ=YEARLY; BYMONTHDAY=15; BYMONTH=12; INTERVAL=1 |
| Event that repeats on every 10th day of December month and ends after 10 occurrences | FREQ=YEARLY; BYMONTHDAY=10; BYMONTH=12; INTERVAL=1; COUNT=10 |
| Repeats on every 12th day of December month and ends on 12/12/2025 | FREQ=YEARLY; BYMONTHDAY=12; BYMONTH=12; INTERVAL=1; UNTIL=20251212T041343Z |
| Repeats on every 3rd Friday of December month and never ends | FREQ=YEARLY; BYDAY=FR; BYMONTH=12; BYSETPOS=3; INTERVAL=1 |
| Repeats on every 3rd Tuesday of December month and ends after 10 occurrences | FREQ=YEARLY; BYDAY=TU; BYMONTH=12; BYSETPOS=3; INTERVAL=1; COUNT=10 |
| Repeats on every 4th Wednesday of December month and ends on 12/12/2028 | FREQ=YEARLY; BYDAY=WE; BYMONTH=12; BYSETPOS=4; INTERVAL=1; UNTIL=20281212T041343Z |

## Recurrence Validation

The built-in validation support has been added by default for recurring appointments during its creation, edit, drag and drop or resize action. The following are the possible validation alerts that displays on Scheduler while creating or editing the recurring events.

| Validation messages | Description |
|-------|---------|
| The recurrence pattern is not valid. | This alert will raise, when the selected recurrence rule value is not a valid one. For example, when you try to select the end date value (using `Until` option) for a recurring event, which occurs before the start date, an alert will popup out saying that the chosen pattern is invalid. |
| The changes made to specific instances of this series will be canceled and those events will match the series again. | This alert will raise, when you try to edit the whole series, whose occurrence might have been already edited. For example, If there are five occurrences and one of the occurrence is already edited. Now, when you try to edit the entire series, you will get this validation alert. |
| The duration of the event must be shorter than how frequently it occurs. Shorten the duration, or change the recurrence pattern in the recurrence event editor. | This validation will occur, if the event duration is longer than the selected frequency. For example, if you create a recurring appointment with two days duration in `Daily` frequency with no intervals set to it, you may get this alert. |
| Some months have fewer than the selected date. For these months, the occurrence will fall on the last date of the month. | When you try to create a recurring appointment on 31st of every month, where few months won't have 31 days and in this scenario, you will get this alert. |
| Two occurrences of the same event cannot occur on the same day. | This validation will occur, when you try to edit or move any single occurrence to some other date, where another occurrence of the same event is already present. |
