# Events & Interactions

## Table of Contents
- [Available Events](#available-events)
- [ValueChange Event](#valuechange-event)
- [OnRenderDayCell Event](#onrenderdaycell-event)
- [Lifecycle Events](#lifecycle-events)
- [Navigated Event](#navigated-event)
- [Common Patterns](#common-patterns)
- [Event Examples](#event-examples)

---

## Available Events

The Blazor Calendar fires events during user interaction and component lifecycle:

| Event | Trigger | Purpose |
|-------|---------|---------|
| **ValueChange** | Date selected | Detect date selection change |
| **OnRenderDayCell** | Each day cell renders | Customize cell appearance |
| **Created** | Component initialized | Post-render setup |
| **Destroyed** | Component disposed | Cleanup resources |
| **Navigated** | User navigates views | Track view changes |

### Event Declaration Pattern

All calendar events use `<CalendarEvents>` child element:

```razor
<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?">
        <!-- Event handlers here -->
    </CalendarEvents>
</SfCalendar>
```

---

## ValueChange Event

### Purpose

Triggered when the user selects a new date in the calendar.

### When to Use

- React to date selection (update UI, fetch data)
- Validate selected date
- Update related calendars or UI elements
- Track user interactions for analytics

### Syntax

```razor
<CalendarEvents TValue="DateTime?" ValueChange="@OnDateChanged"></CalendarEvents>
```

### Event Args

```csharp
public class ChangedEventArgs<T>
{
    public T Value { get; set; }  // New selected date
    public T PreviousValue { get; set; }  // Previous selection
}
```

### Example 1: Basic Date Selection Tracking

```razor
@using Syncfusion.Blazor.Calendars

<h3>Date Selection Tracker</h3>

<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" ValueChange="@OnDateChanged"></CalendarEvents>
</SfCalendar>

<p>Last selected: @LastSelected?.ToString("dd/MM/yyyy")</p>

@code {
    public DateTime? LastSelected { get; set; }
    
    private void OnDateChanged(ChangedEventArgs<DateTime?> args)
    {
        LastSelected = args.Value;
    }
}
```

### Example 2: Validate Date Range

```razor
@using Syncfusion.Blazor.Calendars

<h3>Appointment Booking (Weekdays Only)</h3>

<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" ValueChange="@ValidateWeekday"></CalendarEvents>
</SfCalendar>

<p>Status: @StatusMessage</p>

@code {
    public DateTime? SelectedDate { get; set; }
    public string StatusMessage { get; set; } = "Select a weekday";
    
    private void ValidateWeekday(ChangedEventArgs<DateTime?> args)
    {
        if (args.Value == null)
        {
            StatusMessage = "No date selected";
            return;
        }
        
        var dayOfWeek = args.Value.Value.DayOfWeek;
        
        if (dayOfWeek == DayOfWeek.Saturday || dayOfWeek == DayOfWeek.Sunday)
        {
            StatusMessage = "❌ Weekends not available";
            // Could reset value here
        }
        else
        {
            StatusMessage = $"✓ Appointment booked for {args.Value:dddd, dd MMMM}";
            SelectedDate = args.Value;
        }
    }
}
```

### Example 3: Cascade Updates

```razor
@using Syncfusion.Blazor.Calendars

<h3>Trip Planning - Date Changes Cascade</h3>

<div>
    <h5>Start Date:</h5>
    <SfCalendar TValue="DateTime?" @bind-Value="@StartDate">
        <CalendarEvents TValue="DateTime?" ValueChange="@OnStartDateChanged"></CalendarEvents>
    </SfCalendar>
    
    <h5>End Date:</h5>
    <SfCalendar TValue="DateTime?" @bind-Value="@EndDate" Min="@StartDate"></SfCalendar>
</div>

<p>Duration: @CalculateDuration() days</p>

@code {
    public DateTime? StartDate { get; set; } = DateTime.Now;
    public DateTime? EndDate { get; set; } = DateTime.Now.AddDays(7);
    
    private void OnStartDateChanged(ChangedEventArgs<DateTime?> args)
    {
        StartDate = args.Value;
        
        // Auto-adjust end date if before new start
        if (EndDate < StartDate)
        {
            EndDate = StartDate?.AddDays(7);
        }
    }
    
    private int CalculateDuration()
    {
        return StartDate == null || EndDate == null 
            ? 0 
            : (EndDate.Value.Date - StartDate.Value.Date).Days;
    }
}
```

---

## OnRenderDayCell Event

### Purpose

Triggered for each day cell as it renders. Allows customization of cell appearance, content, or state.

### When to Use

- Highlight special dates (holidays, events)
- Disable specific dates (blackout dates)
- Add custom styling or icons to dates
- Show tooltips or additional info
- Mark booked/available dates

### Syntax

```razor
<CalendarEvents TValue="DateTime?" OnRenderDayCell="@CustomizeCell"></CalendarEvents>
```

### Event Args

```csharp
public class RenderDayCellEventArgs
{
    public DateTime Date { get; set; }  // Date of the cell
    public string CellTemplate { get; set; }  // HTML to render
    public bool IsDisabled { get; set; }  // Disable cell
    public bool IsOutOfRange { get; set; }  // Out of Min/Max range
    public List<string> ClassList { get; set; }  // CSS classes to apply
}
```

### Example 1: Highlight Weekends

```razor
@using Syncfusion.Blazor.Calendars

<h3>Calendar with Weekend Highlighting</h3>

<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" OnRenderDayCell="@HighlightWeekends"></CalendarEvents>
</SfCalendar>

<style>
    .weekend { background-color: #ffe6e6; }
</style>

@code {
    private void HighlightWeekends(RenderDayCellEventArgs args)
    {
        if (args.Date.DayOfWeek == DayOfWeek.Saturday || 
            args.Date.DayOfWeek == DayOfWeek.Sunday)
        {
            args.ClassList.Add("weekend");
        }
    }
}
```

### Example 2: Disable Past Dates

```razor
@using Syncfusion.Blazor.Calendars

<h3>No Past Dates (Custom Rendering)</h3>

<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" OnRenderDayCell="@DisablePastDates"></CalendarEvents>
</SfCalendar>

@code {
    private void DisablePastDates(RenderDayCellEventArgs args)
    {
        if (args.Date.Date < DateTime.Now.Date)
        {
            args.IsDisabled = true;
        }
    }
}
```

### Example 3: Mark Special Dates with Icons

```razor
@using Syncfusion.Blazor.Calendars

<h3>Event Calendar</h3>

<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" OnRenderDayCell="@MarkSpecialDates"></CalendarEvents>
</SfCalendar>

<style>
    .has-event::after {
        content: "•";
        color: red;
        font-size: 20px;
        position: absolute;
        bottom: 0;
    }
    
    .birthday::after {
        content: "🎂";
        font-size: 12px;
        position: absolute;
        top: 2px;
        right: 2px;
    }
</style>

@code {
    private HashSet<int> EventDates = new() { 5, 12, 20, 25 };
    private HashSet<int> BirthdayDates = new() { 15, 28 };
    
    private void MarkSpecialDates(RenderDayCellEventArgs args)
    {
        if (EventDates.Contains(args.Date.Day))
        {
            args.ClassList.Add("has-event");
        }
        
        if (BirthdayDates.Contains(args.Date.Day))
        {
            args.ClassList.Add("birthday");
        }
    }
}
```

### Example 4: Disable Booked Slots

```razor
@using Syncfusion.Blazor.Calendars

<h3>Meeting Room Booking</h3>

<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" OnRenderDayCell="@MarkBookedDates"></CalendarEvents>
</SfCalendar>

<style>
    .booked { background-color: #cccccc; opacity: 0.6; }
</style>

@code {
    private HashSet<int> BookedDates = new() { 3, 7, 14, 21, 28 };
    
    private void MarkBookedDates(RenderDayCellEventArgs args)
    {
        if (BookedDates.Contains(args.Date.Day))
        {
            args.IsDisabled = true;
            args.ClassList.Add("booked");
        }
    }
}
```

### Example 5: Today & Selected Date Styling

```razor
@using Syncfusion.Blazor.Calendars

<h3>Date Styling</h3>

<SfCalendar TValue="DateTime?" @bind-Value="@SelectedDate">
    <CalendarEvents TValue="DateTime?" OnRenderDayCell="@StyleDates"></CalendarEvents>
</SfCalendar>

<style>
    .today-marker { border: 3px solid blue; }
    .selected-marker { background-color: #e3f2fd; }
</style>

@code {
    public DateTime? SelectedDate { get; set; }
    
    private void StyleDates(RenderDayCellEventArgs args)
    {
        if (args.Date.Date == DateTime.Now.Date)
        {
            args.ClassList.Add("today-marker");
        }
        
        if (SelectedDate != null && args.Date.Date == SelectedDate.Value.Date)
        {
            args.ClassList.Add("selected-marker");
        }
    }
}
```

---

## Lifecycle Events

### Created Event

Triggered after the Calendar component initializes and renders.

```razor
<CalendarEvents TValue="DateTime?" Created="@OnCreated"></CalendarEvents>

@code {
    private void OnCreated(object args)
    {
        // Component ready
        Console.WriteLine("Calendar initialized");
    }
}
```

### Destroyed Event

Triggered when the Calendar is disposed (component removed from DOM).

```razor
<CalendarEvents TValue="DateTime?" Destroyed="@OnDestroyed"></CalendarEvents>

@code {
    private void OnDestroyed(object args)
    {
        // Cleanup
        Console.WriteLine("Calendar disposed");
    }
}
```

### Example: Track Component Lifecycle

```razor
@using Syncfusion.Blazor.Calendars

<h3>Lifecycle Tracking</h3>

@if (ShowCalendar)
{
    <SfCalendar TValue="DateTime?">
        <CalendarEvents TValue="DateTime?" 
            Created="@OnCreated" 
            Destroyed="@OnDestroyed">
        </CalendarEvents>
    </SfCalendar>
}

<p>Status: @LifecycleStatus</p>
<button @onclick="ToggleCalendar">@(ShowCalendar ? "Hide" : "Show") Calendar</button>

@code {
    public bool ShowCalendar { get; set; } = true;
    public string LifecycleStatus { get; set; } = "Calendar hidden";
    
    private void OnCreated(object args)
    {
        LifecycleStatus = "Calendar created at " + DateTime.Now.ToLongTimeString();
    }
    
    private void OnDestroyed(object args)
    {
        LifecycleStatus = "Calendar destroyed at " + DateTime.Now.ToLongTimeString();
    }
    
    private void ToggleCalendar()
    {
        ShowCalendar = !ShowCalendar;
    }
}
```

---

## Navigated Event

### Purpose

Triggered when user navigates between views (Month ↔ Year ↔ Decade) or when the displayed month/year changes.

### When to Use

- Track user navigation for analytics
- Update dependent UI (breadcrumb, title)
- Control navigation behavior
- Fetch data for displayed period

### Syntax

```razor
<CalendarEvents TValue="DateTime?" Navigated="@OnNavigated"></CalendarEvents>
```

### Event Args

```csharp
public class NavigatedEventArgs
{
    public DateTime Date { get; set; }  // Current focus date
    public CalendarView View { get; set; }  // Current view (Month/Year/Decade)
}
```

### Example

```razor
@using Syncfusion.Blazor.Calendars

<h3>Navigation Tracking</h3>

<p>Current View: @CurrentView</p>
<p>Viewing: @CurrentPeriod</p>

<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" Navigated="@OnNavigated"></CalendarEvents>
</SfCalendar>

@code {
    public string CurrentView { get; set; } = "Month";
    public string CurrentPeriod { get; set; } = DateTime.Now.ToString("MMMM yyyy");
    
    private void OnNavigated(NavigatedEventArgs args)
    {
        CurrentView = args.View.ToString();
        CurrentPeriod = args.View == CalendarView.Month 
            ? args.Date.ToString("MMMM yyyy")
            : args.View == CalendarView.Year
                ? args.Date.ToString("yyyy")
                : $"{args.Date.Year / 10 * 10}-{args.Date.Year / 10 * 10 + 9}";
    }
}
```

---

## Common Patterns

### Pattern 1: Validation + Feedback

```razor
<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" ValueChange="@ValidateAndFeedback"></CalendarEvents>
</SfCalendar>

@code {
    private void ValidateAndFeedback(ChangedEventArgs<DateTime?> args)
    {
        // Validate
        // Show feedback
        // Update state
    }
}
```

### Pattern 2: Multi-Event Handling

```razor
<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" 
        ValueChange="@OnDateChanged"
        OnRenderDayCell="@OnCellRender"
        Navigated="@OnNavigated">
    </CalendarEvents>
</SfCalendar>
```

### Pattern 3: Conditional Cell Customization

```razor
<CalendarEvents TValue="DateTime?" OnRenderDayCell="@CustomizeCell"></CalendarEvents>

@code {
    private void CustomizeCell(RenderDayCellEventArgs args)
    {
        if (IsHoliday(args.Date))
            args.ClassList.Add("holiday");
        
        if (IsWeekend(args.Date))
            args.ClassList.Add("weekend");
        
        if (IsBooked(args.Date))
            args.IsDisabled = true;
    }
    
    private bool IsHoliday(DateTime date) => /* logic */;
    private bool IsWeekend(DateTime date) => /* logic */;
    private bool IsBooked(DateTime date) => /* logic */;
}
```

---

## Event Examples

### Complete Booking Calendar

```razor
@using Syncfusion.Blazor.Calendars

<h3>Restaurant Reservation System</h3>

<div class="booking-form">
    <h5>Select Date:</h5>
    <SfCalendar TValue="DateTime?">
        <CalendarEvents TValue="DateTime?" 
            ValueChange="@OnDateSelected"
            OnRenderDayCell="@HighlightAvailable">
        </CalendarEvents>
    </SfCalendar>
    
    <div class="booking-info">
        <p>Selected: @SelectedDate?.ToString("dddd, dd MMMM yyyy")</p>
        <p>Status: @BookingStatus</p>
        <p>Available Slots: @AvailableSlots</p>
    </div>
</div>

<style>
    .available { background-color: #c8e6c9; }
    .booked { background-color: #ffcccc; }
</style>

@code {
    public DateTime? SelectedDate { get; set; }
    public string BookingStatus { get; set; } = "Select a date";
    public int AvailableSlots { get; set; } = 0;
    
    private Dictionary<int, int> AvailabilityMap = new()
    {
        { 1, 5 }, { 5, 2 }, { 10, 8 }, { 15, 0 }, { 20, 3 }, { 25, 6 }
    };
    
    private void OnDateSelected(ChangedEventArgs<DateTime?> args)
    {
        SelectedDate = args.Value;
        
        if (SelectedDate != null)
        {
            int day = SelectedDate.Value.Day;
            AvailableSlots = AvailabilityMap.ContainsKey(day) 
                ? AvailabilityMap[day] 
                : 7;
            
            BookingStatus = AvailableSlots > 0 
                ? $"✓ {AvailableSlots} slots available" 
                : "❌ Fully booked";
        }
    }
    
    private void HighlightAvailable(RenderDayCellEventArgs args)
    {
        int day = args.Date.Day;
        
        if (AvailabilityMap.ContainsKey(day))
        {
            if (AvailabilityMap[day] == 0)
            {
                args.IsDisabled = true;
                args.ClassList.Add("booked");
            }
            else
            {
                args.ClassList.Add("available");
            }
        }
    }
}
```

---

## Debugging Events

### Enable Console Logging

```csharp
private void OnDateChanged(ChangedEventArgs<DateTime?> args)
{
    Console.WriteLine($"Date changed from {args.PreviousValue} to {args.Value}");
}
```

### Check Event Firing

Add debug output to verify event handlers are called:

```csharp
private void OnRenderDayCell(RenderDayCellEventArgs args)
{
    if (args.Date.Day == 1)
        Console.WriteLine($"Rendering day 1");
}
```
