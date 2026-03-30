# Events in Blazor DatePicker

## Table of Contents
- [ValueChange Event](#valuechange-event)
- [OnClose Event](#onclose-event)
- [Focus and Blur Events](#focus-and-blur-events)
- [Created and Destroyed Events](#created-and-destroyed-events)
- [Event Patterns](#event-patterns)
- [Common Workflows](#common-workflows)

## ValueChange Event

Fires when the user selects a date from the calendar or types a valid date.

### Basic ValueChange

```razor
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange"></SfDatePicker>

@code {
    void OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        DateTime? selectedDate = args.Value;
        Console.WriteLine($"Date selected: {selectedDate}");
    }
}
```

### With State Update

```razor
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange"></SfDatePicker>

<p>You selected: @selectedDate</p>

@code {
    DateTime? selectedDate;

    void OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        selectedDate = args.Value;
    }
}
```

### Prevent Invalid Dates

```razor
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange"
              Min="@minDate" Max="@maxDate">
</SfDatePicker>

@code {
    DateTime? minDate = new DateTime(2026, 1, 1);
    DateTime? maxDate = new DateTime(2026, 12, 31);

    void OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        if (args.Value.HasValue && args.Value >= minDate && args.Value <= maxDate)
        {
            Console.WriteLine("Valid date accepted");
        }
    }
}
```

### Trigger API Call on Selection

```razor
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange"></SfDatePicker>

@code {
    async Task OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        if (args.Value.HasValue)
        {
            // Fetch data for selected date
            var data = await FetchDataForDate(args.Value.Value);
            Console.WriteLine($"Data: {data}");
        }
    }

    async Task<string> FetchDataForDate(DateTime date)
    {
        // Simulate API call
        await Task.Delay(500);
        return $"Data for {date:d}";
    }
}
```

## OnClose Event

Fires when the calendar popup closes, regardless of whether a date was selected.

### Basic OnClose

```razor
<SfDatePicker TValue="DateTime?" OnClose="@OnPopupClose"></SfDatePicker>

@code {
    void OnPopupClose(EventArgs args)
    {
        Console.WriteLine("Calendar popup closed");
    }
}
```

### Detect Interaction Without Selection

```razor
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange" OnClose="@OnPopupClose"></SfDatePicker>

<p>Status: @status</p>

@code {
    string status = "";

    void OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        status = $"Date selected: {args.Value}";
    }

    void OnPopupClose(EventArgs args)
    {
        status = "Popup closed without selection";
    }
}
```

### Clean Up Resources

```razor
<SfDatePicker TValue="DateTime?" OnClose="@OnPopupClose"></SfDatePicker>

@code {
    void OnPopupClose(EventArgs args)
    {
        // Clean up temporary resources
        // Close related tooltips, dialogs, etc.
        Console.WriteLine("Cleanup");
    }
}
```

## Focus and Blur Events

Handle focus/blur for validation and state management.

### OnFocus Event

```razor
<SfDatePicker TValue="DateTime?" 
              OnFocus="@OnPickerFocus"
              Placeholder="Click to select date">
</SfDatePicker>

<p>Field focused: @isFocused</p>

@code {
    bool isFocused = false;

    void OnPickerFocus(FocusEventArgs args)
    {
        isFocused = true;
        Console.WriteLine("DatePicker focused");
    }
}
```

### OnBlur Event

```razor
<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate"
              OnBlur="@OnPickerBlur"
              Format="dd/MM/yyyy">
</SfDatePicker>

<span class="error" style="display:@(hasError ? "block" : "none")">
    Invalid date format
</span>

@code {
    DateTime? selectedDate;
    bool hasError = false;

    void OnPickerBlur(FocusEventArgs args)
    {
        // Validate when field loses focus
        hasError = !selectedDate.HasValue;
    }
}
```

### Form Validation on Blur

```razor
<EditForm Model="@formData" OnValidSubmit="@SubmitForm">
    <SfDatePicker TValue="DateTime?" @bind-Value="@formData.EventDate"
                  OnBlur="@OnPickerBlur">
    </SfDatePicker>
    
    <span class="error" style="display:@(hasError ? "block" : "none")">
        Date is required
    </span>
    
    <button type="submit">Submit</button>
</EditForm>

@code {
    FormModel formData = new();
    bool hasError = false;

    void OnPickerBlur(FocusEventArgs args)
    {
        hasError = !formData.EventDate.HasValue;
    }

    void SubmitForm()
    {
        // Submit logic
    }

    class FormModel
    {
        public DateTime? EventDate { get; set; }
    }
}
```

## Created and Destroyed Events

Handle component lifecycle events.

### On Created

Fires after the DatePicker component initializes.

```razor
<SfDatePicker TValue="DateTime?" Created="@OnCreated"></SfDatePicker>

@code {
    void OnCreated(object args)
    {
        Console.WriteLine("DatePicker component created");
        // Initialize default values, fetch data, etc.
    }
}
```

### On Destroyed

Fires before the DatePicker is removed from the DOM.

```razor
<SfDatePicker TValue="DateTime?" Destroyed="@OnDestroyed"></SfDatePicker>

@code {
    void OnDestroyed(object args)
    {
        Console.WriteLine("DatePicker component destroyed");
        // Clean up subscriptions, clear temp data, etc.
    }
}
```

## Event Patterns

### Combine Multiple Events

```razor
<SfDatePicker TValue="DateTime?" 
              ValueChange="@OnDateChange"
              OnClose="@OnPopupClose"
              OnBlur="@OnPickerBlur">
</SfDatePicker>

<div>
    <p>Last action: @lastAction</p>
</div>

@code {
    string lastAction = "";

    void OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        lastAction = $"Selected: {args.Value}";
    }

    void OnPopupClose(EventArgs args)
    {
        lastAction = "Popup closed";
    }

    void OnPickerBlur(FocusEventArgs args)
    {
        lastAction = "Field blurred";
    }
}
```

### Event with Async Operations

```razor
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange"></SfDatePicker>

<p>@message</p>

@code {
    string message = "";

    async Task OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        if (args.Value.HasValue)
        {
            message = "Loading...";
            
            // Async operation
            await Task.Delay(1000);
            
            message = $"Data loaded for {args.Value:d}";
        }
    }
}
```

### Conditional Event Handling

```razor
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange"></SfDatePicker>

@code {
    bool validateOnChange = true;

    void OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        if (validateOnChange && args.Value.HasValue)
        {
            // Validation logic
            ValidateDate(args.Value.Value);
        }
    }

    void ValidateDate(DateTime date)
    {
        // Validation code
    }

    void ToggleValidation()
    {
        validateOnChange = !validateOnChange;
    }
}
```

## Common Workflows

### Workflow 1: Real-Time Date Calculation

```razor
<div>
    <label>Select Start Date:</label>
    <SfDatePicker TValue="DateTime?" ValueChange="@OnStartDateChange"></SfDatePicker>
</div>

<div>
    <p>30 days later: @endDate?.ToString("dd/MM/yyyy")</p>
</div>

@code {
    DateTime? endDate;

    void OnStartDateChange(ChangedEventArgs<DateTime?> args)
    {
        if (args.Value.HasValue)
        {
            endDate = args.Value.Value.AddDays(30);
        }
    }
}
```

### Workflow 2: Cascade Filtering

```razor
<div>
    <label>Select Date:</label>
    <SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange"></SfDatePicker>
</div>

<div>
    <label>Select Event:</label>
    <SfDropDownList TValue="string" DataSource="@events" TItem="EventItem">
        <DropDownListFieldSettings Text="EventName" Value="EventId"></DropDownListFieldSettings>
    </SfDropDownList>
</div>

@code {
    List<EventItem> events = new();

    void OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        if (args.Value.HasValue)
        {
            // Load events for selected date
            events = FetchEventsForDate(args.Value.Value);
        }
    }

    List<EventItem> FetchEventsForDate(DateTime date)
    {
        // Simulate fetching events
        return new List<EventItem>
        {
            new EventItem { EventId = "1", EventName = "Meeting" },
            new EventItem { EventId = "2", EventName = "Conference" }
        };
    }

    class EventItem
    {
        public string EventId { get; set; }
        public string EventName { get; set; }
    }
}
```

### Workflow 3: Multi-Field Dependency

```razor
<div>
    <label>Appointment Date:</label>
    <SfDatePicker TValue="DateTime?" @bind-Value="@appointmentDate" ValueChange="@OnDateChange"></SfDatePicker>
</div>

<div>
    <label>Available Times:</label>
    <SfListBox TValue="string" DataSource="@availableTimes"></SfListBox>
</div>

<p>Selected: @appointmentDate - @selectedTime</p>

@code {
    DateTime? appointmentDate;
    string selectedTime;
    List<string> availableTimes = new();

    void OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        if (args.Value.HasValue)
        {
            // Load available time slots for selected date
            availableTimes = GetAvailableTimesForDate(args.Value.Value);
            selectedTime = null; // Reset selected time
        }
    }

    List<string> GetAvailableTimesForDate(DateTime date)
    {
        // Check availability for that date
        return new List<string> { "09:00", "10:00", "14:00", "15:00" };
    }
}
```

## Troubleshooting

### Issue: ValueChange not firing
**Solution:** Verify event handler syntax matches signature: `(ChangeEventArgs<DateTime?> args)`

### Issue: Event firing multiple times
**Solution:** Check if parent component is re-rendering unnecessarily

### Issue: Async event causing UI freeze
**Solution:** Use `await Task.Run()` for long operations

### Issue: Events not firing in Blazor Server
**Solution:** Ensure component is InteractiveServer render mode

## Next Steps

- **Data Binding:** Learn [ValueChange with data binding](data-binding.md)
- **Advanced:** Handle [multiple pickers and cascading events](advanced-features.md)
- **Styling:** Update [UI based on events](styling-and-appearance.md)
