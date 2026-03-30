# Data Binding & Events

Learn how to bind data to the DateTimePicker and handle events for dynamic interactions.

## Data Binding

### One-Way Binding

Set an initial value without automatic updates:

```blazor
@page "/datetimepicker-binding"
@using Syncfusion.Blazor.Calendars

<SfDateTimePicker TValue="DateTime?" Value="@selectedDate"></SfDateTimePicker>

@code {
    private DateTime? selectedDate = new DateTime(2024, 3, 19, 10, 30, 0);
}
```

### Two-Way Binding

Use `@bind-Value` to automatically sync data between component and model:

```blazor
<SfDateTimePicker TValue="DateTime?" @bind-Value="selectedDate"></SfDateTimePicker>

@if (selectedDate.HasValue)
{
    <p>You selected: @selectedDate.Value</p>
}

@code {
    private DateTime? selectedDate;
}
```

### Binding to Model Properties

```blazor
@page "/datetimepicker-model"
@using Syncfusion.Blazor.Calendars

<div>
    <h3>Appointment Form</h3>
    
    <div>
        <label>Title:</label>
        <input @bind="appointment.Title" type="text" />
    </div>
    
    <div>
        <label>Start Date & Time:</label>
        <SfDateTimePicker TValue="DateTime?" @bind-Value="appointment.StartTime"></SfDateTimePicker>
    </div>
    
    <div>
        <label>End Date & Time:</label>
        <SfDateTimePicker TValue="DateTime?" @bind-Value="appointment.EndTime"></SfDateTimePicker>
    </div>
    
    <button @onclick="SaveAppointment">Save</button>
</div>

@code {
    private Appointment appointment = new();
    
    private void SaveAppointment()
    {
        Console.WriteLine($"Saved: {appointment.Title} from {appointment.StartTime} to {appointment.EndTime}");
    }
    
    public class Appointment
    {
        public string Title { get; set; }
        public DateTime? StartTime { get; set; }
        public DateTime? EndTime { get; set; }
    }
}
```

## Event Handling

### ValueChange Event

Triggered when the selected date/time value changes:

```blazor
<SfDateTimePicker TValue="DateTime?" >
    <DateTimePickerEvents TValue="DateTime?" ValueChange="@OnValueChanged"></DateTimePickerEvents>
</SfDateTimePicker>

@code {
    private void OnValueChanged(ChangedEventArgs<DateTime?> args)
    {
        if (args.Value.HasValue)
        {
            Console.WriteLine($"Date changed to: {args.Value.Value}");
        }
        else
        {
            Console.WriteLine("Date cleared");
        }
    }
}
```

### ValueChange with Async Operations

Perform async operations when value changes:

```blazor
<SfDateTimePicker TValue="DateTime?" >
    <DateTimePickerEvents TValue="DateTime?" ValueChange="@OnValueChangedAsync"></DateTimePickerEvents>
</SfDateTimePicker>

<p>@statusMessage</p>

@code {
    private string statusMessage = "";
    
    private async Task OnValueChangedAsync(ChangedEventArgs<DateTime?> args)
    {
        if (args.Value.HasValue)
        {
            statusMessage = "Loading...";
            
            // Simulate async operation (API call)
            await Task.Delay(1000);
            
            statusMessage = $"Loaded data for {args.Value.Value.ToString("d")}";
        }
    }
}
```

### Open & Close Events

Handle when the calendar popup opens or closes:

```blazor
<SfDateTimePicker TValue="DateTime?" >
    <DateTimePickerEvents TValue="DateTime?" 
        OnOpen="@OnOpened"
        OnClose="@OnClosed">
    </DateTimePickerEvents>
</SfDateTimePicker>

@code {
    private void OnOpened(object args)
    {
        Console.WriteLine("Calendar opened");
    }
    
    private void OnClosed(object args)
    {
        Console.WriteLine("Calendar closed");
    }
}
```

### Focus & Blur Events

Track when the component gains or loses focus:

```blazor
<SfDateTimePicker TValue="DateTime?" >
    <DateTimePickerEvents TValue="DateTime?" 
        Focus="@OnFocused"
        Blur="@OnBlurred">
    </DateTimePickerEvents>
</SfDateTimePicker>

@code {
    private void OnFocused(object args)
    {
        Console.WriteLine("Input focused");
    }
    
    private void OnBlurred(Syncfusion.Blazor.Calendars.BlurEventArgs args)
    {
        Console.WriteLine("Input blurred");
    }
}
```

## Common Event Patterns

### Pattern 1: Validation on Value Change

```blazor
@page "/datetimepicker-validation"
@using Syncfusion.Blazor.Calendars

<div>
    <SfDateTimePicker TValue="DateTime?" >
        <DateTimePickerEvents TValue="DateTime?" ValueChange="@ValidateAndUpdate"></DateTimePickerEvents>
    </SfDateTimePicker>
    
    @if (!string.IsNullOrEmpty(validationMessage))
    {
        <div style="color: @(isValid ? "green" : "red");">
            @validationMessage
        </div>
    }
</div>

@code {
    private DateTime? selectedDate;
    private string validationMessage = "";
    private bool isValid = true;
    
    private void ValidateAndUpdate(ChangedEventArgs<DateTime?> args)
    {
        selectedDate = args.Value;
        
        if (!selectedDate.HasValue)
        {
            validationMessage = "Date is required";
            isValid = false;
            return;
        }
        
        if (selectedDate.Value < DateTime.Now)
        {
            validationMessage = "Date cannot be in the past";
            isValid = false;
        }
        else if (selectedDate.Value > DateTime.Now.AddDays(365))
        {
            validationMessage = "Date cannot be more than 1 year in future";
            isValid = false;
        }
        else
        {
            validationMessage = "Date is valid ✓";
            isValid = true;
        }
    }
}
```

### Pattern 2: Update Related Field

When one date changes, update constraints on another:

```blazor
@page "/datetimepicker-related"
@using Syncfusion.Blazor.Calendars

<div>
    <div>
        <label>Start Date:</label>
        <SfDateTimePicker TValue="DateTime?" 
                          @bind-Value="startDate">
            <DateTimePickerEvents TValue="DateTime?" ValueChange="@OnStartDateChanged"></DateTimePickerEvents>
        </SfDateTimePicker>
    </div>
    
    <div>
        <label>End Date:</label>
        <SfDateTimePicker TValue="DateTime?" 
                          @bind-Value="endDate"
                          Min="@startDate"
                          Placeholder="Must be after start date">
        </SfDateTimePicker>
    </div>
</div>

@code {
    private DateTime? startDate;
    private DateTime? endDate;
    
    private void OnStartDateChanged(ChangedEventArgs<DateTime?> args)
    {
        startDate = args.Value;
        // Clear end date if it's now before start date
        if (endDate.HasValue && endDate < startDate)
        {
            endDate = null;
        }
    }
}
```

### Pattern 3: Conditional Display Based on Selection

```blazor
@page "/datetimepicker-conditional"
@using Syncfusion.Blazor.Calendars

<div>
    <SfDateTimePicker TValue="DateTime?" 
                      @bind-Value="selectedDate">
        <DateTimePickerEvents TValue="DateTime?" ValueChange="@OnDateSelected"></DateTimePickerEvents>
    </SfDateTimePicker>
    
    @if (selectedDate.HasValue)
    {
        @if (selectedDate.Value.DayOfWeek == DayOfWeek.Friday)
        {
            <p style="color: blue;">You selected a Friday! 🎉</p>
        }
        
        @if (selectedDate.Value.Hour >= 17)
        {
            <p style="color: orange;">Evening slot selected</p>
        }
    }
</div>

@code {
    private DateTime? selectedDate;
    
    private void OnDateSelected(ChangedEventArgs<DateTime?> args)
    {
        selectedDate = args.Value;
    }
}
```

### Pattern 4: Trigger External Action

```blazor
@page "/datetimepicker-external"
@using Syncfusion.Blazor.Calendars

<div>
    <SfDateTimePicker TValue="DateTime?" >
        <DateTimePickerEvents TValue="DateTime?" ValueChange="@OnDateSelectedAsync"></DateTimePickerEvents>
    </SfDateTimePicker>
    
    @if (!string.IsNullOrEmpty(loadedData))
    {
        <div style="margin-top: 20px; padding: 10px; background-color: #f0f0f0;">
            @loadedData
        </div>
    }
</div>

@code {
    private string loadedData = "";
    
    private async Task OnDateSelectedAsync(ChangedEventArgs<DateTime?> args)
    {
        if (args.Value.HasValue)
        {
            loadedData = "Loading data...";
            
            // Simulate API call
            await Task.Delay(500);
            
            loadedData = $"Data loaded for {args.Value.Value.ToString("d")}";
        }
    }
}
```

## Event Reference

| Event | Trigger | Args |
|-------|---------|------|
| `ValueChange` | When date/time value changes | `ChangedEventArgs<DateTime?>` |
| `OnOpen` | When calendar popup opens | `object` |
| `OnClose` | When calendar popup closes | `object` |
| `Focus` | When input gains focus | `object` |
| `Blur` | When input loses focus | `Syncfusion.Blazor.Calendars.BlurEventArgs` |
| `Created` | When component is initialized | `object` |
| `Destroyed` | When component is destroyed | `object` |
| `Navigated` | When navigating calendar views | `NavigatedEventArgs` |
| `OnRenderDayCell` | When each day cell renders | `RenderDayCellEventArgs` |

## State Management

### Local Component State

```blazor
@page "/datetimepicker-state"
@using Syncfusion.Blazor.Calendars

<div>
    <SfDateTimePicker TValue="DateTime?" @bind-Value="selectedDate"></SfDateTimePicker>
    
    <button @onclick="ResetDate">Reset</button>
    <button @onclick="SetToNow">Set to Now</button>
</div>

@code {
    private DateTime? selectedDate;
    
    private void ResetDate()
    {
        selectedDate = null;
    }
    
    private void SetToNow()
    {
        selectedDate = DateTime.Now;
    }
}
```

### Cascading Parameters for Parent-Child Communication

```blazor
<!-- Parent Component -->
@page "/parent"
@using Syncfusion.Blazor.Calendars

<DateTimeSelectorChild SelectedDateChanged="@OnChildDateChanged"></DateTimeSelectorChild>

@code {
    private void OnChildDateChanged(DateTime? date)
    {
        Console.WriteLine($"Parent received date: {date}");
    }
}

<!-- Child Component: DateTimeSelectorChild.razor -->
@using Syncfusion.Blazor.Calendars

<SfDateTimePicker TValue="DateTime?" >
    <DateTimePickerEvents TValue="DateTime?" ValueChange="@NotifyParent"></DateTimePickerEvents>
</SfDateTimePicker>

@code {
    [Parameter]
    public EventCallback<DateTime?> SelectedDateChanged { get; set; }
    
    private async Task NotifyParent(ChangedEventArgs<DateTime?> args)
    {
        await SelectedDateChanged.InvokeAsync(args.Value);
    }
}
```

---

**Related Topics:**
- [Getting Started](getting-started.md) - Basic component setup
- [Date & Time Formatting](date-time-formatting.md) - Format selected dates
- [Masking & Input Validation](masking-and-input-validation.md) - Validate user input
