# Advanced Features & Patterns

## Table of Contents
- [Show/Hide Other Month Dates](#showhide-other-month-dates)
- [Multiple Date Selection](#multiple-date-selection)
- [Week Number Display](#week-number-display)
- [Custom Cell Rendering](#custom-cell-rendering)
- [Performance Optimization](#performance-optimization)
- [Integration Patterns](#integration-patterns)
- [Complex Use Cases](#complex-use-cases)

---

## Show/Hide Other Month Dates

### What Does This Do?

By default, Calendar shows trailing dates from the previous month and leading dates from the next month to fill the grid:

```
        March 2024
S  M  T  W  T  F  S
25 26 27 28 29  1  2    <- Feb dates (other month)
 3  4  5  6  7  8  9
10 11 12 13 14 15 16
17 18 19 20 21 22 23
24 25 26 27 28 29 30
31  1  2  3  4  5  6    <- Apr dates (other month)
```

### Hiding Other Month Dates

While Syncfusion Calendar doesn't have a built-in toggle, you can hide them with CSS:

```css
/* Hide dates from other months */
.e-calendar .e-other-month {
    visibility: hidden;
}

/* Alternative: Disable them */
.e-calendar .e-other-month {
    opacity: 0.3;
    pointer-events: none;
}
```

### Custom Rendering Approach

Use `OnRenderDayCell` event to customize other month dates:

```razor
@using Syncfusion.Blazor.Calendars

<h3>Calendar - Show/Hide Other Month Dates</h3>

<div>
    <button @onclick="() => ToggleOtherMonthDates()">
        @(ShowOtherMonthDates ? "Hide" : "Show") Other Month Dates
    </button>
</div>

<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" OnRenderDayCell="@CustomizeOtherMonthDates"></CalendarEvents>
</SfCalendar>

<style>
    .hidden-other-month {
        opacity: 0 !important;
        pointer-events: none !important;
    }
</style>

@code {
    public bool ShowOtherMonthDates { get; set; } = true;
    
    private void ToggleOtherMonthDates()
    {
        ShowOtherMonthDates = !ShowOtherMonthDates;
    }
    
    private void CustomizeOtherMonthDates(RenderDayCellEventArgs args)
    {
        if (args.IsOutOfRange && !ShowOtherMonthDates)
        {
            args.ClassList.Add("hidden-other-month");
        }
    }
}
```

---

## Multiple Date Selection

### Use Case

Select multiple dates (vacation period, multiple meetings, etc.)

### Implementation

While Calendar doesn't natively support multiple selection, you can implement it:

```razor
@using Syncfusion.Blazor.Calendars

<h3>Multi-Date Selection (Trip Planning)</h3>

<div class="selection-info">
    <p>Selected Dates: @(SelectedDates.Count > 0 ? string.Join(", ", SelectedDates.Select(d => d.ToString("dd/MM"))) : "None")</p>
    <button @onclick="ClearSelection">Clear All</button>
</div>

<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" 
        ValueChange="@AddToSelection"
        OnRenderDayCell="@HighlightSelected">
    </CalendarEvents>
</SfCalendar>

<style>
    .selected-date {
        background-color: #bbdefb !important;
        border: 2px solid #1976d2 !important;
    }
    
    .start-date {
        background: linear-gradient(to right, #1976d2 50%, #bbdefb 50%) !important;
        color: white !important;
    }
    
    .end-date {
        background: linear-gradient(to left, #1976d2 50%, #bbdefb 50%) !important;
        color: white !important;
    }
</style>

@code {
    public HashSet<DateTime> SelectedDates { get; set; } = new();
    
    private void AddToSelection(ChangedEventArgs<DateTime?> args)
    {
        if (args.Value.HasValue)
        {
            if (SelectedDates.Contains(args.Value.Value.Date))
                SelectedDates.Remove(args.Value.Value.Date);
            else
                SelectedDates.Add(args.Value.Value.Date);
        }
    }
    
    private void HighlightSelected(RenderDayCellEventArgs args)
    {
        if (SelectedDates.Contains(args.Date.Date))
        {
            args.ClassList.Add("selected-date");
            
            var minDate = SelectedDates.Min();
            var maxDate = SelectedDates.Max();
            
            if (args.Date.Date == minDate)
                args.ClassList.Add("start-date");
            
            if (args.Date.Date == maxDate)
                args.ClassList.Add("end-date");
        }
    }
    
    private void ClearSelection()
    {
        SelectedDates.Clear();
    }
}
```

### Range Selection Pattern

```razor
@using Syncfusion.Blazor.Calendars

<h3>Date Range Selection (Vacation)</h3>

<div class="range-inputs">
    <div>
        <label>From:</label>
        <input type="date" @bind="@StartDateString" />
    </div>
    <div>
        <label>To:</label>
        <input type="date" @bind="@EndDateString" />
    </div>
    <p>Duration: @((EndDate?.Date - StartDate?.Date)?.Days) days</p>
</div>

<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" OnRenderDayCell="@HighlightRange"></CalendarEvents>
</SfCalendar>

<style>
    .range-highlight {
        background-color: #e1bee7 !important;
    }
    
    .range-start {
        background-color: #7b1fa2 !important;
        color: white !important;
    }
    
    .range-end {
        background-color: #7b1fa2 !important;
        color: white !important;
    }
</style>

@code {
    public DateTime? StartDate { get; set; }
    public DateTime? EndDate { get; set; }
    
    public string StartDateString 
    { 
        get => StartDate?.ToString("yyyy-MM-dd");
        set => StartDate = DateTime.TryParse(value, out var dt) ? dt : null;
    }
    
    public string EndDateString 
    { 
        get => EndDate?.ToString("yyyy-MM-dd");
        set => EndDate = DateTime.TryParse(value, out var dt) ? dt : null;
    }
    
    private void HighlightRange(RenderDayCellEventArgs args)
    {
        if (StartDate == null || EndDate == null) return;
        
        var min = StartDate.Value.Date < EndDate.Value.Date ? StartDate.Value.Date : EndDate.Value.Date;
        var max = StartDate.Value.Date > EndDate.Value.Date ? StartDate.Value.Date : EndDate.Value.Date;
        
        if (args.Date.Date >= min && args.Date.Date <= max)
        {
            args.ClassList.Add("range-highlight");
            
            if (args.Date.Date == min)
                args.ClassList.Add("range-start");
            
            if (args.Date.Date == max)
                args.ClassList.Add("range-end");
        }
    }
}
```

---

## Week Number Display

### Enabling Week Numbers

```razor
<SfCalendar TValue="DateTime?" ShowWeekNumbers="true"></SfCalendar>
```

**Output:** Shows ISO 8601 week numbers in left column (1-53)

### Week Number Details

- **Standard:** ISO 8601 (International)
- **Week 1:** First week with Thursday of the year
- **Range:** 1-53
- **Use:** Project planning, reporting periods, compliance

### Example: Week-Based Planning

```razor
@using Syncfusion.Blazor.Calendars

<h3>Sprint Planning (By Week Number)</h3>

<SfCalendar TValue="DateTime?" 
    @bind-Value="@SelectedDate"
    ShowWeekNumbers="true">
    <CalendarEvents TValue="DateTime?" 
        ValueChange="@OnDateSelected"
        Navigated="@OnNavigated">
    </CalendarEvents>
</SfCalendar>

<div class="sprint-info">
    <h5>Sprint Details</h5>
    <p>Date: @SelectedDate?.ToString("dddd, dd MMMM yyyy")</p>
    <p>Week Number: @GetWeekNumber(SelectedDate)</p>
    <p>Sprint: @GetSprintName(SelectedDate)</p>
    <p>Work Days Remaining: @GetWorkDaysRemaining(SelectedDate)</p>
</div>

@code {
    public DateTime? SelectedDate { get; set; } = DateTime.Now;
    
    private int GetWeekNumber(DateTime? date)
    {
        if (date == null) return 0;
        var culture = System.Globalization.CultureInfo.GetCultureInfo("en-US");
        return culture.Calendar.GetWeekOfYear(date.Value, 
            System.Globalization.CalendarWeekRule.FirstFourDayWeek,
            DayOfWeek.Monday);
    }
    
    private string GetSprintName(DateTime? date)
    {
        if (date == null) return "";
        int week = GetWeekNumber(date);
        int sprint = (week - 1) / 2 + 1;
        return $"Sprint Q{date.Value.Year}/{sprint}";
    }
    
    private int GetWorkDaysRemaining(DateTime? date)
    {
        if (date == null) return 0;
        var sundayOfWeek = date.Value.AddDays(-(int)date.Value.DayOfWeek);
        var fridayOfWeek = sundayOfWeek.AddDays(5);
        return (fridayOfWeek - date.Value).Days;
    }
    
    private void OnDateSelected(ChangedEventArgs<DateTime?> args)
    {
        // Trigger sprint calculation
        StateHasChanged();
    }
    
    private void OnNavigated(NavigatedEventArgs args)
    {
        // Update sprint info when navigating
    }
}
```

---

## Custom Cell Rendering

### Advanced Rendering Scenarios

#### Scenario 1: Business Hours Indicator

```razor
@using Syncfusion.Blazor.Calendars

<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" OnRenderDayCell="@ShowBusinessHours"></CalendarEvents>
</SfCalendar>

<style>
    .business-hours::after {
        content: "9-5";
        font-size: 10px;
        opacity: 0.6;
    }
    
    .after-hours {
        opacity: 0.5;
    }
</style>

@code {
    private void ShowBusinessHours(RenderDayCellEventArgs args)
    {
        if (args.Date.DayOfWeek != DayOfWeek.Saturday && 
            args.Date.DayOfWeek != DayOfWeek.Sunday)
        {
            args.ClassList.Add("business-hours");
        }
        else
        {
            args.ClassList.Add("after-hours");
        }
    }
}
```

#### Scenario 2: Heat Map (Intensity Visualization)

```razor
@using Syncfusion.Blazor.Calendars

<h3>Activity Heatmap</h3>

<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" OnRenderDayCell="@ColorByIntensity"></CalendarEvents>
</SfCalendar>

<style>
    .intensity-0 { background-color: #ffffff; }
    .intensity-1 { background-color: #eef5e9; }
    .intensity-2 { background-color: #c3e88d; }
    .intensity-3 { background-color: #9ccc65; }
    .intensity-4 { background-color: #7cb342; }
    .intensity-5 { background-color: #558b2f; }
</style>

@code {
    private Dictionary<int, int> ActivityIntensity = new()
    {
        { 1, 2 }, { 2, 3 }, { 3, 1 }, { 5, 4 }, 
        { 10, 5 }, { 15, 3 }, { 20, 2 }, { 25, 4 }
    };
    
    private void ColorByIntensity(RenderDayCellEventArgs args)
    {
        int intensity = ActivityIntensity.ContainsKey(args.Date.Day) 
            ? ActivityIntensity[args.Date.Day] 
            : 0;
        
        args.ClassList.Add($"intensity-{intensity}");
    }
}
```

---

## Performance Optimization

### Large Date Ranges

When working with large datasets:

```csharp
// Lazy-load disabled dates instead of pre-computing all
private async Task<bool> IsDateDisabled(DateTime date)
{
    // Fetch from API only when needed
    var response = await Http.GetAsync($"/api/availability/{date:yyyy-MM-dd}");
    return !response.IsSuccessStatusCode;
}
```

### Memoization for Calculations

```csharp
private Dictionary<int, bool> HolidayCache = new();

private bool IsHoliday(DateTime date)
{
    int dayOfYear = date.DayOfYear;
    
    if (!HolidayCache.ContainsKey(dayOfYear))
    {
        HolidayCache[dayOfYear] = ComputeHolidayStatus(date);
    }
    
    return HolidayCache[dayOfYear];
}
```

### Avoid Heavy Operations in OnRenderDayCell

**Bad:**
```csharp
private void OnRenderDayCell(RenderDayCellEventArgs args)
{
    // Called for EVERY cell on EVERY render!
    var isDisabled = Http.Get($"/api/availability/{args.Date}").IsSuccess; // ❌ Slow!
}
```

**Good:**
```csharp
private Dictionary<DateTime, bool> AvailabilityCache;

private void OnRenderDayCell(RenderDayCellEventArgs args)
{
    // Use cached data
    if (AvailabilityCache.ContainsKey(args.Date.Date))
    {
        args.IsDisabled = !AvailabilityCache[args.Date.Date];
    }
}
```

---

## Integration Patterns

### Pattern 1: Calendar + Data Grid

```razor
@using Syncfusion.Blazor.Calendars
@using Syncfusion.Blazor.Grids

<div class="calendar-grid-layout">
    <div class="calendar-section">
        <h5>Select Date:</h5>
        <SfCalendar TValue="DateTime?" @bind-Value="@SelectedDate"></SfCalendar>
    </div>
    
    <div class="grid-section">
        <h5>Events for @SelectedDate?.ToString("dd/MM/yyyy"):</h5>
        <SfGrid DataSource="@GetEventsForDate(SelectedDate)">
            <GridColumns>
                <GridColumn Field="@nameof(Event.Time)" HeaderText="Time"></GridColumn>
                <GridColumn Field="@nameof(Event.Title)" HeaderText="Event"></GridColumn>
            </GridColumns>
        </SfGrid>
    </div>
</div>

@code {
    public DateTime? SelectedDate { get; set; } = DateTime.Now;
    
    public class Event
    {
        public string Time { get; set; }
        public string Title { get; set; }
    }
    
    private List<Event> GetEventsForDate(DateTime? date)
    {
        if (date == null) return new();
        // Filter events for selected date
        return new() 
        { 
            new() { Time = "09:00", Title = "Stand-up" },
            new() { Time = "14:00", Title = "Review" }
        };
    }
}

<style>
    .calendar-grid-layout {
        display: grid;
        grid-template-columns: 1fr 2fr;
        gap: 20px;
    }
</style>
```

### Pattern 2: Calendar Modal/Dialog

```razor
@using Syncfusion.Blazor.Calendars
@using Syncfusion.Blazor.Popups

<button @onclick="@ShowCalendarDialog">Pick Date</button>

<SfDialog @bind-Visible="@IsDialogOpen" Title="Select Date" Width="400px">
    <DialogTemplates>
        <Content>
            <SfCalendar TValue="DateTime?" @bind-Value="@DialDate"></SfCalendar>
        </Content>
    </DialogTemplates>
    <DialogButtons>
        <DialogButton Content="OK" OnClick="@ConfirmSelection" />
        <DialogButton Content="Cancel" OnClick="@CancelSelection" />
    </DialogButtons>
</SfDialog>

@code {
    public bool IsDialogOpen { get; set; }
    public DateTime? DialDate { get; set; }
    
    private void ShowCalendarDialog()
    {
        IsDialogOpen = true;
    }
    
    private void ConfirmSelection()
    {
        IsDialogOpen = false;
        // Use DialDate
    }
    
    private void CancelSelection()
    {
        IsDialogOpen = false;
    }
}
```

---

## Complex Use Cases

### Use Case 1: Appointment Slots

```razor
@using Syncfusion.Blazor.Calendars

<h3>Schedule Appointment</h3>

<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" 
        ValueChange="@OnDateSelected"
        OnRenderDayCell="@MarkAvailable">
    </CalendarEvents>
</SfCalendar>

@if (AvailableSlots.Count > 0)
{
    <div class="time-slots">
        <h5>Available Times:</h5>
        <div class="slots">
            @foreach (var slot in AvailableSlots)
            {
                <button @onclick="@(() => SelectSlot(slot))" class="slot">
                    @slot.ToString("HH:mm")
                </button>
            }
        </div>
    </div>
}

@code {
    private DateTime? SelectedDate { get; set; }
    private List<DateTime> AvailableSlots { get; set; } = new();
    
    private void OnDateSelected(ChangedEventArgs<DateTime?> args)
    {
        SelectedDate = args.Value;
        LoadAvailableSlots(SelectedDate);
    }
    
    private void LoadAvailableSlots(DateTime? date)
    {
        if (date == null) return;
        
        AvailableSlots = new()
        {
            date.Value.AddHours(9),
            date.Value.AddHours(10),
            date.Value.AddHours(14),
            date.Value.AddHours(15)
        };
    }
    
    private void MarkAvailable(RenderDayCellEventArgs args)
    {
        // Mark dates with available slots
        if (HasAvailableSlots(args.Date))
        {
            args.ClassList.Add("has-slots");
        }
    }
    
    private bool HasAvailableSlots(DateTime date) => true; // Implement logic
    
    private void SelectSlot(DateTime slot)
    {
        Console.WriteLine($"Selected: {slot}");
    }
}

<style>
    .time-slots {
        margin-top: 20px;
    }
    
    .slots {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
        gap: 10px;
    }
    
    .slot {
        padding: 10px;
        border: 1px solid #ddd;
        background: white;
        cursor: pointer;
        border-radius: 4px;
    }
    
    .slot:hover {
        background: #e3f2fd;
    }
</style>
```

### Use Case 2: Recurring Events

```razor
@using Syncfusion.Blazor.Calendars

<h3>Recurring Events Calendar</h3>

<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" OnRenderDayCell="@HighlightRecurring"></CalendarEvents>
</SfCalendar>

@code {
    private class RecurringEvent
    {
        public string Title { get; set; }
        public DayOfWeek DayOfWeek { get; set; }
        public string Color { get; set; }
    }
    
    private List<RecurringEvent> RecurringEvents = new()
    {
        new() { Title = "Team Meeting", DayOfWeek = DayOfWeek.Monday, Color = "blue" },
        new() { Title = "Standup", DayOfWeek = DayOfWeek.Wednesday, Color = "green" },
        new() { Title = "Review", DayOfWeek = DayOfWeek.Friday, Color = "purple" }
    };
    
    private void HighlightRecurring(RenderDayCellEventArgs args)
    {
        var recurringToday = RecurringEvents
            .FirstOrDefault(e => e.DayOfWeek == args.Date.DayOfWeek);
        
        if (recurringToday != null)
        {
            args.ClassList.Add($"recurring-{recurringToday.Color}");
        }
    }
}
```

---

## Summary

Advanced Calendar features enable:
- ✅ Complex date selection patterns
- ✅ Data visualization (heatmaps, intensity)
- ✅ Integration with other components
- ✅ Performance-optimized rendering
- ✅ Enterprise-grade functionality

For production applications, combine these patterns with proper error handling, loading states, and user feedback.
