# Date Selection & Data Binding

## Table of Contents
- [Overview](#overview)
- [One-Way Binding](#one-way-binding)
- [Two-Way Binding](#two-way-binding)
- [DateOnly Support](#dateonly-support)
- [Dynamic Value Changes](#dynamic-value-changes)
- [Date Range Constraints](#date-range-constraints)
- [Common Patterns](#common-patterns)

---

## Overview

The Calendar component supports multiple data binding approaches:

| Pattern | Syntax | Use Case |
|---------|--------|----------|
| **One-Way** | `Value="@Date"` | Display date, no auto-update |
| **Two-Way** | `@bind-Value="@Date"` | Component ↔ State sync |
| **Dynamic** | `@bind-Value` + events | Reactive updates with side effects |

### Key Properties

- **Value** - Selected date (get/set)
- **Min** - Earliest selectable date (DateTime, inclusive)
- **Max** - Latest selectable date (DateTime, inclusive)
- **TValue** - Type parameter for binding: `DateTime`, `DateTime?`, or `DateOnly` (.NET 6+)

---

## One-Way Binding

### What is One-Way Binding?

Component displays a value passed from parent, but changes don't automatically update the parent.

### Syntax

```razor
<SfCalendar TValue="DateTime?" Value="@SelectedDate"></SfCalendar>
```

### When to Use

- Display calendar with pre-selected date
- User selections don't need immediate parent update
- Parent updates calendar value manually (button click, etc.)

### Example

```razor
@using Syncfusion.Blazor.Calendars

<h3>One-Way Binding Demo</h3>

<p>Selected: @SelectedDate?.ToString("dd/MM/yyyy")</p>

<SfCalendar TValue="DateTime?" Value="@SelectedDate"></SfCalendar>

<button @onclick="UpdateDate">Set to Today</button>

@code {
    public DateTime? SelectedDate { get; set; } = new DateTime(2024, 3, 15);
    
    private void UpdateDate()
    {
        SelectedDate = DateTime.Now;
    }
}
```

**Behavior:**
1. Calendar shows March 15, 2024
2. User clicks a date in calendar (no visible change to display)
3. Button click updates SelectedDate, calendar re-renders with new date

### Important Notes

- Calendar value changes via UI don't update `SelectedDate` automatically
- To sync changes, use two-way binding (`@bind-Value`) or events (`ValueChange`)
- Parent must manually trigger re-renders or update state

---

## Two-Way Binding

### What is Two-Way Binding?

Component and parent stay in perfect sync. User selects date → calendar updates. Parent updates date → calendar re-renders automatically.

### Syntax

```razor
<SfCalendar TValue="DateTime?" @bind-Value="@SelectedDate"></SfCalendar>
```

The `@bind-` prefix establishes two-way binding.

### When to Use

- Calendar selection should immediately update parent state
- Display other UI based on selected date
- Share selected date between multiple components
- Form submission includes selected calendar date

### Example 1: Basic Two-Way Binding

```razor
@using Syncfusion.Blazor.Calendars

<h3>Two-Way Binding Demo</h3>

<p>You selected: @SelectedDate?.ToString("dddd, dd MMMM yyyy")</p>

<SfCalendar TValue="DateTime?" @bind-Value="@SelectedDate"></SfCalendar>

<button @onclick="ResetToToday">Reset</button>

@code {
    public DateTime? SelectedDate { get; set; } = DateTime.Now;
    
    private void ResetToToday()
    {
        SelectedDate = DateTime.Now;
    }
}
```

**Behavior:**
1. Calendar displays current date
2. User clicks a date → text updates immediately
3. Reset button updates SelectedDate → calendar re-renders

### Example 2: Reactive Display Based on Selection

```razor
@using Syncfusion.Blazor.Calendars

<h3>Event Timeline</h3>

<SfCalendar TValue="DateTime?" @bind-Value="@SelectedDate"></SfCalendar>

<div class="event-details">
    <h4>Events for @SelectedDate?.ToString("dd/MM/yyyy"):</h4>
    @if (GetEventsForDate(SelectedDate).Count > 0)
    {
        <ul>
        @foreach (var evt in GetEventsForDate(SelectedDate))
        {
            <li>@evt</li>
        }
        </ul>
    }
    else
    {
        <p>No events scheduled.</p>
    }
</div>

@code {
    public DateTime? SelectedDate { get; set; } = DateTime.Now;
    
    private Dictionary<DateTime, List<string>> Events = new()
    {
        { DateTime.Now, new() { "Team Meeting", "Lunch" } },
        { DateTime.Now.AddDays(1), new() { "Project Deadline" } },
        { DateTime.Now.AddDays(7), new() { "Conference" } }
    };
    
    private List<string> GetEventsForDate(DateTime? date)
    {
        if (date == null) return new();
        var dateOnly = date.Value.Date;
        return Events.ContainsKey(dateOnly) ? Events[dateOnly] : new();
    }
}
```

**Output:** Calendar shows, and selecting a date displays associated events.

### Example 3: Form with Calendar Field

```razor
@using Syncfusion.Blazor.Calendars

<h3>Appointment Booking</h3>

<EditForm Model="@Appointment" OnValidSubmit="@HandleSubmit">
    <div>
        <label>Select Appointment Date:</label>
        <SfCalendar TValue="DateTime?" @bind-Value="@Appointment.Date"></SfCalendar>
    </div>
    
    <p>Date: @Appointment.Date?.ToString("dd/MM/yyyy")</p>
    
    <button type="submit">Book Appointment</button>
</EditForm>

@code {
    public class AppointmentModel
    {
        public DateTime? Date { get; set; }
    }
    
    private AppointmentModel Appointment = new();
    
    private async Task HandleSubmit()
    {
        // Submit appointment with selected date
        Console.WriteLine($"Booking appointment for {Appointment.Date}");
    }
}
```

**Output:** Calendar integrated in a form; selected date included in submission.

---

## DateOnly Support

### What is DateOnly?

`DateOnly` (.NET 6+) represents a date without time component, more efficient than `DateTime?`.

### When to Use DateOnly

- Time component is irrelevant
- Reduces memory footprint
- Cleaner semantics (date ≠ date+time)
- .NET 6+ target framework required

### Syntax

```razor
<SfCalendar TValue="DateOnly" @bind-Value="@SelectedDate"></SfCalendar>

@code {
    public DateOnly SelectedDate { get; set; } = DateOnly.FromDateTime(DateTime.Now);
}
```

### Example 1: Simple DateOnly

```razor
@using Syncfusion.Blazor.Calendars

<h3>Birthday Selector (DateOnly)</h3>

<p>Your birthday: @SelectedDate</p>

<SfCalendar TValue="DateOnly" @bind-Value="@SelectedDate"></SfCalendar>

@code {
    public DateOnly SelectedDate { get; set; } = DateOnly.FromDateTime(DateTime.Now);
}
```

### Example 2: DateOnly with Min/Max

```razor
@using Syncfusion.Blazor.Calendars

<h3>Meeting Room Booking (18-60 days out)</h3>

<SfCalendar TValue="DateOnly" 
    @bind-Value="@BookingDate"
    Min="@MinDate"
    Max="@MaxDate">
</SfCalendar>

<p>Booking: @BookingDate</p>

@code {
    public DateOnly BookingDate { get; set; } = DateOnly.FromDateTime(DateTime.Now);
    
    public DateOnly MinDate { get; set; } = DateOnly.FromDateTime(DateTime.Now.AddDays(18));
    public DateOnly MaxDate { get; set; } = DateOnly.FromDateTime(DateTime.Now.AddDays(60));
}
```

### DateOnly vs DateTime?

| Aspect | DateOnly | DateTime? |
|--------|----------|-----------|
| **Memory** | 4 bytes | 8 bytes |
| **Time** | Not stored | Includes time |
| **Use** | Date-only logic | Date+time needed |
| **Min .NET** | 6.0+ | Any |
| **Semantics** | Clearer intent | Generic |

---

## Dynamic Value Changes

### What is Dynamic Binding?

Update calendar value programmatically and have it reflect in the UI.

### Challenge: Blazor State Management

When you change a property in code, Blazor doesn't automatically re-render unless:
1. Change happens in event handler
2. You call `StateHasChanged()`

### Solution: Use Events

Handle `ValueChange` event to detect selection and update state:

```razor
<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" ValueChange="@OnDateChanged"></CalendarEvents>
</SfCalendar>

@code {
    private void OnDateChanged(ChangedEventArgs<DateTime?> args)
    {
        // args.Value contains new date
        // Update state here
        StateHasChanged(); // Optional - usually implicit
    }
}
```

### Example 1: Programmatic Updates

```razor
@using Syncfusion.Blazor.Calendars

<h3>Calendar with Programmatic Control</h3>

<div>
    <button @onclick="GoToToday">Go to Today</button>
    <button @onclick="GoToNextMonth">Next Month</button>
    <button @onclick="GoToPreviousMonth">Previous Month</button>
</div>

<p>Selected: @SelectedDate?.ToString("dd/MM/yyyy")</p>

<SfCalendar TValue="DateTime?" 
    @bind-Value="@SelectedDate"
    Min="@MinDate"
    Max="@MaxDate">
</SfCalendar>

@code {
    public DateTime? SelectedDate { get; set; } = DateTime.Now;
    public DateTime MinDate { get; set; } = DateTime.Now.AddMonths(-12);
    public DateTime MaxDate { get; set; } = DateTime.Now.AddMonths(12);
    
    private void GoToToday()
    {
        SelectedDate = DateTime.Now;
    }
    
    private void GoToNextMonth()
    {
        if (SelectedDate.HasValue)
        {
            var next = SelectedDate.Value.AddMonths(1);
            if (next <= MaxDate)
                SelectedDate = next;
        }
    }
    
    private void GoToPreviousMonth()
    {
        if (SelectedDate.HasValue)
        {
            var prev = SelectedDate.Value.AddMonths(-1);
            if (prev >= MinDate)
                SelectedDate = prev;
        }
    }
}
```

### Example 2: Cascading Date Selection

```razor
@using Syncfusion.Blazor.Calendars

<h3>Cascading Selection</h3>

<div class="row">
    <div class="col">
        <h5>Start Date:</h5>
        <SfCalendar TValue="DateTime?" 
            @bind-Value="@StartDate"
            Max="@EndDate">
        </SfCalendar>
    </div>
    
    <div class="col">
        <h5>End Date:</h5>
        <SfCalendar TValue="DateTime?" 
            @bind-Value="@EndDate"
            Min="@StartDate">
        </SfCalendar>
    </div>
</div>

<p>Duration: @((EndDate?.Date - StartDate?.Date)?.Days) days</p>

@code {
    public DateTime? StartDate { get; set; } = DateTime.Now;
    public DateTime? EndDate { get; set; } = DateTime.Now.AddDays(7);
}
```

**Behavior:** Changing start date updates end date's minimum; vice versa.

---

## Date Range Constraints

### Purpose

Restrict date selection to valid boundaries using `Min` and `Max`.

### Properties

- **Min** (DateTime) - Earliest selectable date (inclusive, non-null)
- **Max** (DateTime) - Latest selectable date (inclusive, non-null)

### Behavior

- Dates outside [Min, Max] are **disabled** (grayed out)
- User cannot select disabled dates
- If `Value` is set outside range, it's auto-corrected to nearest boundary
- Dates are compared by date only; time components ignored

### Example 1: Business Days Only

```razor
@using Syncfusion.Blazor.Calendars

<h3>Book Meeting (Business Days Only)</h3>

<SfCalendar TValue="DateTime?" 
    @bind-Value="@BookingDate"
    Min="@MonthStart"
    Max="@MonthEnd">
</SfCalendar>

<p>Booking: @BookingDate?.ToString("dd/MM/yyyy")</p>

@code {
    public DateTime? BookingDate { get; set; } = DateTime.Now;
    
    public DateTime MonthStart { get; set; } = new DateTime(DateTime.Now.Year, DateTime.Now.Month, 1);
    public DateTime MonthEnd { get; set; } = new DateTime(DateTime.Now.Year, DateTime.Now.Month, 
        DateTime.DaysInMonth(DateTime.Now.Year, DateTime.Now.Month));
}
```

### Example 2: Past Dates Disabled

```razor
@using Syncfusion.Blazor.Calendars

<h3>No Past Dates (Today onwards)</h3>

<SfCalendar TValue="DateTime?" 
    @bind-Value="@SelectedDate"
    Min="@minBookingDate">
</SfCalendar>

@code {
    public DateTime? SelectedDate { get; set; } = DateTime.Now;
    private DateTime minBookingDate = DateTime.Now;
}
```

### Example 3: Future Dates Only (90 days window)

```razor
@using Syncfusion.Blazor.Calendars

<h3>Plan Trip (Next 90 Days)</h3>

<SfCalendar TValue="DateTime?" 
    @bind-Value="@TripDate"
    Min="@minTripDate"
    Max="@maxTripDate">
</SfCalendar>

<p>Trip Date: @TripDate?.ToString("dd/MM/yyyy")</p>

@code {
    private DateTime minTripDate = DateTime.Now;
    private DateTime maxTripDate = DateTime.Now.AddDays(90);
    public DateTime? TripDate { get; set; } = DateTime.Now.AddDays(14);
}
```

---

## Common Patterns

### Pattern 1: Read-Only Display

```razor
<SfCalendar TValue="DateTime?" Value="@DisplayDate" Enabled="false"></SfCalendar>
```

### Pattern 2: Today Pre-selected

```razor
<SfCalendar TValue="DateTime?" @bind-Value="@SelectedDate"></SfCalendar>

@code {
    public DateTime? SelectedDate { get; set; } = DateTime.Now;
}
```

### Pattern 3: Reset to Default

```razor
<button @onclick="ResetDate">Clear Selection</button>

@code {
    private void ResetDate() => SelectedDate = null;
}
```

### Pattern 4: Future Dates + Range Validation

```razor
<SfCalendar TValue="DateTime?" 
    @bind-Value="@SelectedDate"
    Min="@minAllowedDate"
    Max="@maxAllowedDate">
</SfCalendar>

@code {
    private DateTime minAllowedDate = DateTime.Now;
    private DateTime maxAllowedDate = DateTime.Now.AddDays(365);
}
```

### Pattern 5: Exact Date Match

```razor
@if (SelectedDate == new DateTime(2024, 12, 25))
{
    <p>You selected Christmas!</p>
}
```

---

## Type Mismatch Troubleshooting

### Issue: "Cannot bind to property of type X"

**Common Cause:** `TValue` doesn't match property type.

**Wrong:**
```razor
<SfCalendar TValue="DateTime" @bind-Value="@SelectedDate"></SfCalendar>

@code {
    public DateTime? SelectedDate { get; set; } // Nullable, but TValue is not
}
```

**Fix:**
```razor
<SfCalendar TValue="DateTime?" @bind-Value="@SelectedDate"></SfCalendar>

@code {
    public DateTime? SelectedDate { get; set; } // Matches TValue
}
```

### Issue: DateTime vs DateOnly Mismatch

**Wrong:**
```razor
<SfCalendar TValue="DateOnly" @bind-Value="@SelectedDate"></SfCalendar>

@code {
    public DateTime? SelectedDate { get; set; } // Should be DateOnly
}
```

**Fix:**
```razor
<SfCalendar TValue="DateOnly" @bind-Value="@SelectedDate"></SfCalendar>

@code {
    public DateOnly SelectedDate { get; set; } = DateOnly.FromDateTime(DateTime.Now);
}
```
