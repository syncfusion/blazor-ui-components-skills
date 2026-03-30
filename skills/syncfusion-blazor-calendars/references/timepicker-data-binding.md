# Data Binding in Blazor TimePicker

## Table of Contents
- [One-Way Binding](#one-way-binding)
- [Two-Way Binding](#two-way-binding)
- [Dynamic Value Updates](#dynamic-value-updates)
- [Type Handling (DateTime vs DateTime?)](#type-handling-datetime-vs-datetime)
- [Value Change Events](#value-change-events)
- [Common Binding Patterns](#common-binding-patterns)
- [Binding Troubleshooting](#binding-troubleshooting)

## One-Way Binding

One-way binding passes data from the component property to the TimePicker. The TimePicker displays the value but changes in the picker don't update the property.

### Basic One-Way Binding

```razor
@using Syncfusion.Blazor.Calendars

<p>Current Time: @SelectedTime</p>

<SfTimePicker TValue="DateTime?" Value="@SelectedTime"></SfTimePicker>

<button @onclick="UpdateTime">Update Time (Manual)</button>

@code {
    public DateTime? SelectedTime { get; set; } = DateTime.Now;

    public void UpdateTime()
    {
        SelectedTime = DateTime.Now.AddHours(1);
    }
}
```

### When to Use One-Way Binding

- **Display-only scenarios**: Show current time without allowing changes
- **Manual update control**: Update value only through specific button clicks
- **External data sources**: Value comes from API calls or parent component

```razor
@* Display preset times *@
<SfTimePicker TValue="DateTime?" Value="@new DateTime(2026, 3, 19, 14, 0, 0)"></SfTimePicker>

@* Update only via explicit button *@
<SfTimePicker TValue="DateTime?" Value="@AppointmentTime"></SfTimePicker>
<button @onclick="@(() => AppointmentTime = DateTime.Now)">Use Current Time</button>

@code {
    public DateTime? AppointmentTime { get; set; }
}
```

## Two-Way Binding

Two-way binding uses `@bind-Value` to synchronize the TimePicker value with a component property. Changes in the picker immediately update the property.

### Basic Two-Way Binding

```razor
@using Syncfusion.Blazor.Calendars

<p>You selected: @SelectedTime?.ToString("HH:mm")</p>

<SfTimePicker TValue="DateTime?" @bind-Value="@SelectedTime"></SfTimePicker>

@code {
    public DateTime? SelectedTime { get; set; } = DateTime.Now;
}
```

### Real-Time Updates with Two-Way Binding

```razor
@using Syncfusion.Blazor.Calendars

<div class="card">
    <h3>Meeting Scheduler</h3>
    
    <p>Meeting Start Time: <strong>@MeetingStart?.ToString("hh:mm tt")</strong></p>
    <p>Meeting End Time: <strong>@MeetingEnd?.ToString("hh:mm tt")</strong></p>
    <p>Duration: <strong>@GetDuration() minutes</strong></p>

    <label>Start Time:</label>
    <SfTimePicker TValue="DateTime?" @bind-Value="@MeetingStart"></SfTimePicker>

    <label>End Time:</label>
    <SfTimePicker TValue="DateTime?" @bind-Value="@MeetingEnd"></SfTimePicker>
</div>

@code {
    public DateTime? MeetingStart { get; set; } = DateTime.Now;
    public DateTime? MeetingEnd { get; set; } = DateTime.Now.AddMinutes(30);

    public int GetDuration()
    {
        if (MeetingStart == null || MeetingEnd == null)
            return 0;
        return (int)(MeetingEnd.Value - MeetingStart.Value).TotalMinutes;
    }
}
```

### When to Use Two-Way Binding

- **Form input**: User selects time that immediately affects other fields
- **Real-time calculations**: Duration, availability checks based on selected time
- **Cascading updates**: Time selection triggers child component updates
- **Live validation**: Validate time as user selects

## Dynamic Value Updates

The `StateHasChanged()` method forces re-render when updates occur outside Blazor's normal lifecycle (e.g., event handlers, timers).

### Dynamic Update with StateHasChanged

```razor
@using Syncfusion.Blazor.Calendars

<p>Current Selection: @SelectedTime?.ToString("HH:mm")</p>

<SfTimePicker TValue="DateTime?" @bind-Value="@SelectedTime"></SfTimePicker>

<button @onclick="SetCurrentTime">Set to Now</button>
<button @onclick="SetLunchTime">Set to Lunch (12:00)</button>

@code {
    public DateTime? SelectedTime { get; set; }

    private void SetCurrentTime()
    {
        SelectedTime = DateTime.Now;
        StateHasChanged(); // Force UI update
    }

    private void SetLunchTime()
    {
        SelectedTime = new DateTime(DateTime.Now.Year, DateTime.Now.Month, 
                                    DateTime.Now.Day, 12, 0, 0);
        StateHasChanged();
    }
}
```

### Dynamic Updates from ValueChange Event

```razor
@using Syncfusion.Blazor.Calendars

<p>Selected: @SelectedTime?.ToString("HH:mm")</p>
<p>Message: @StatusMessage</p>

<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" ValueChange="OnValueChanged"></TimePickerEvents>
</SfTimePicker>

@code {
    public DateTime? SelectedTime { get; set; }
    public string StatusMessage { get; set; } = "";

    private void OnValueChanged(Syncfusion.Blazor.Calendars.ChangeEventArgs<DateTime?> args)
    {
        SelectedTime = args.Value;
        
        // Update message based on time
        if (SelectedTime?.Hour >= 12 && SelectedTime?.Hour < 17)
            StatusMessage = "Afternoon selected";
        else if (SelectedTime?.Hour >= 9 && SelectedTime?.Hour < 12)
            StatusMessage = "Morning selected";
        else
            StatusMessage = "Evening selected";

        StateHasChanged();
    }
}
```

### Timer-Based Dynamic Updates

```razor
@using Syncfusion.Blazor.Calendars
@implements IAsyncDisposable

<p>Auto-updated Time: @DisplayTime?.ToString("HH:mm:ss")</p>

<SfTimePicker TValue="DateTime?" Value="@DisplayTime"></SfTimePicker>

@code {
    public DateTime? DisplayTime { get; set; } = DateTime.Now;
    private System.Threading.Timer? _timer;

    protected override void OnInitialized()
    {
        _timer = new System.Threading.Timer(UpdateTime, null, 0, 1000); // Update every second
    }

    private void UpdateTime(object? state)
    {
        DisplayTime = DateTime.Now;
        StateHasChanged();
    }

    async ValueTask IAsyncDisposable.DisposeAsync()
    {
        if (_timer is not null)
            await _timer.DisposeAsync();
    }
}
```

## Type Handling (DateTime vs DateTime?)

### DateTime (Non-Nullable)

Use `DateTime` when a time is always required:

```razor
@using Syncfusion.Blazor.Calendars

<SfTimePicker TValue="DateTime" Value="@RequiredTime"></SfTimePicker>

@code {
    public DateTime RequiredTime { get; set; } = DateTime.Now;
}
```

**Pros:** Always has a value, no null checks needed  
**Cons:** Cannot represent "no selection"

### DateTime? (Nullable)

Use `DateTime?` when time selection is optional:

```razor
@using Syncfusion.Blazor.Calendars

<p>Selected Time: @(OptionalTime?.ToString("HH:mm") ?? "No time selected")</p>

<SfTimePicker TValue="DateTime?" @bind-Value="@OptionalTime"></SfTimePicker>

@code {
    public DateTime? OptionalTime { get; set; }
}
```

**Pros:** Can be null, represents optional selection  
**Cons:** Requires null checking

### When to Use Each Type

| Scenario | Type | Example |
|----------|------|---------|
| Optional time selection | `DateTime?` | Remind me at (optional) |
| Always has time | `DateTime` | Shift start time (always set) |
| May start as empty | `DateTime?` | Availability window |
| Form submission optional | `DateTime?` | Call me back time |
| System time tracking | `DateTime` | Last update: (always has time) |

## Value Change Events

The `ValueChange` event fires when the user selects a time in the picker.

### Tracking Value Changes

```razor
@using Syncfusion.Blazor.Calendars

<p>Selected: @SelectedTime?.ToString("HH:mm")</p>
<p>Changes: @ChangeCount</p>

<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" ValueChange="OnChanged"></TimePickerEvents>
</SfTimePicker>

@code {
    public DateTime? SelectedTime { get; set; }
    public int ChangeCount { get; set; }

    private void OnChanged(Syncfusion.Blazor.Calendars.ChangeEventArgs<DateTime?> args)
    {
        SelectedTime = args.Value;
        ChangeCount++;
    }
}
```

### Conditional Logic on Value Change

```razor
@using Syncfusion.Blazor.Calendars

<p>Selected: @SelectedTime?.ToString("HH:mm")</p>
<p>Status: @Status</p>

<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" ValueChange="ValidateTime"></TimePickerEvents>
</SfTimePicker>

@code {
    public DateTime? SelectedTime { get; set; }
    public string Status { get; set; } = "";

    private void ValidateTime(Syncfusion.Blazor.Calendars.ChangeEventArgs<DateTime?> args)
    {
        SelectedTime = args.Value;

        if (SelectedTime == null)
        {
            Status = "No time selected";
        }
        else if (SelectedTime.Value.Hour < 9 || SelectedTime.Value.Hour >= 17)
        {
            Status = "⚠️ Outside business hours";
        }
        else
        {
            Status = "✓ Valid business hours";
        }
    }
}
```

## Common Binding Patterns

### Pattern 1: Form Binding

```razor
@using Syncfusion.Blazor.Calendars

<EditForm Model="@Appointment" OnSubmit="@HandleSubmit">
    <label>Appointment Time:</label>
    <SfTimePicker TValue="DateTime?" @bind-Value="@Appointment.Time"></SfTimePicker>
    
    <button type="submit">Book Appointment</button>
</EditForm>

@code {
    private AppointmentModel Appointment = new();

    private async Task HandleSubmit()
    {
        await ApiClient.SaveAppointment(Appointment);
    }

    public class AppointmentModel
    {
        public DateTime? Time { get; set; }
        public string Location { get; set; }
    }
}
```

### Pattern 2: Parent-Child Component Binding

```razor
@* Parent Component *@
@using Syncfusion.Blazor.Calendars

<TimePicker @bind-Value="@SelectedTime"></TimePicker>
<p>Parent sees: @SelectedTime?.ToString("HH:mm")</p>

@code {
    public DateTime? SelectedTime { get; set; }
}

@* Child Component (TimePicker.razor) *@
@using Syncfusion.Blazor.Calendars

<SfTimePicker TValue="DateTime?" @bind-Value="@Value"></SfTimePicker>

@code {
    [Parameter]
    public DateTime? Value { get; set; }

    [Parameter]
    public EventCallback<DateTime?> ValueChanged { get; set; }
}
```

### Pattern 3: Conditional Binding

```razor
@using Syncfusion.Blazor.Calendars

<label>
    <input type="checkbox" @bind="@IsTimeRequired" />
    Time Required?
</label>

@if (IsTimeRequired)
{
    <SfTimePicker TValue="DateTime?" @bind-Value="@SelectedTime"></SfTimePicker>
}

@code {
    public bool IsTimeRequired { get; set; }
    public DateTime? SelectedTime { get; set; }
}
```

## Binding Troubleshooting

### Issue 1: Value Not Updating

**Problem**: TimePicker shows value but it doesn't update when user selects time.

**Solution**: Use `@bind-Value` instead of `Value`:

```razor
<!-- ❌ WRONG -->
<SfTimePicker TValue="DateTime?" Value="@SelectedTime"></SfTimePicker>

<!-- ✅ CORRECT -->
<SfTimePicker TValue="DateTime?" @bind-Value="@SelectedTime"></SfTimePicker>
```

### Issue 2: Null Reference Exception

**Problem**: Code fails with null reference when checking selected time.

**Solution**: Use null-coalescing operator:

```razor
<!-- ❌ WRONG -->
<p>Hour: @SelectedTime.Value.Hour</p>

<!-- ✅ CORRECT -->
<p>Hour: @(SelectedTime?.Hour ?? 0)</p>
```

### Issue 3: Changes Not Reflecting in UI

**Problem**: Time updates but UI doesn't refresh.

**Solution**: Call `StateHasChanged()` in event handlers:

```razor
private void OnTimeChanged(Syncfusion.Blazor.Calendars.ChangeEventArgs<DateTime?> args)
{
    SelectedTime = args.Value;
    StateHasChanged(); // Force UI update
}
```

## See Also

- [Time Formats →](../time-formats.md)
- [Events and Handlers →](../events-and-handlers.md)
- [Getting Started →](../getting-started.md)
