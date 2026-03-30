# Events and Handlers in Blazor TimePicker

## Table of Contents
- [Event Overview](#event-overview)
- [ValueChange Event](#valuechange-event)
- [Focus Events](#focus-events)
- [Popup Events](#popup-events)
- [Lifecycle Events](#lifecycle-events)
- [Item Render Event](#item-render-event)
- [Event Handler Implementation Patterns](#event-handler-implementation-patterns)
- [Common Event Scenarios](#common-event-scenarios)

## Event Overview

The TimePicker component supports multiple events that fire at different stages of user interaction. All events require specifying the `TValue` generic parameter in the `TimePickerEvents` component.

### Event Types

| Event | Fires When | Return Type |
|-------|-----------|------------|
| `ValueChange` | User selects a time | `ChangeEventArgs<DateTime?>` |
| `OnOpen` | Popup opens | `PopupEventArgs` |
| `OnClose` | Popup closes | `PopupEventArgs` |
| `Focus` | Input gets focus | `FocusEventArgs` |
| `Blur` | Input loses focus | `BlurEventArgs` |
| `OnItemRender` | Each popup item renders | `ItemEventArgs<DateTime?>` |
| `Created` | Component initialized | `object` |
| `Destroyed` | Component destroyed | `object` |

### Basic Event Syntax

```razor
@using Syncfusion.Blazor.Calendars

<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" 
        ValueChange="OnValueChange"
        OnOpen="OnOpen"
        OnClose="OnClose">
    </TimePickerEvents>
</SfTimePicker>

@code {
    private void OnValueChange(Syncfusion.Blazor.Calendars.ChangeEventArgs<DateTime?> args)
    {
        // Handle value change
    }

    private void OnOpen(Syncfusion.Blazor.Calendars.PopupEventArgs args)
    {
        // Handle popup open
    }

    private void OnClose(Syncfusion.Blazor.Calendars.PopupEventArgs args)
    {
        // Handle popup close
    }
}
```

## ValueChange Event

The `ValueChange` event fires when the user selects a time from the popup list. This is the primary event for tracking user selection.

### ValueChange Arguments

```csharp
public class ChangeEventArgs<T>
{
    public T Value { get; set; }          // Selected time
    public DateTime? PreviousValue { get; set; }  // Previously selected time
    public bool IsInteracted { get; set; } // Whether user interacted
}
```

### Basic ValueChange Handler

```razor
@using Syncfusion.Blazor.Calendars

<p>Selected Time: @SelectedTime?.ToString("HH:mm")</p>

<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" ValueChange="OnTimeSelected"></TimePickerEvents>
</SfTimePicker>

@code {
    public DateTime? SelectedTime { get; set; }

    private void OnTimeSelected(Syncfusion.Blazor.Calendars.ChangeEventArgs<DateTime?> args)
    {
        SelectedTime = args.Value;
        Console.WriteLine($"Time selected: {args.Value}");
    }
}
```

### ValueChange with Business Logic

```razor
@using Syncfusion.Blazor.Calendars

<p>Selected Time: @SelectedTime?.ToString("HH:mm")</p>
<p>Status: @ValidationMessage</p>

<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" ValueChange="ValidateAndSave"></TimePickerEvents>
</SfTimePicker>

@code {
    public DateTime? SelectedTime { get; set; }
    public string ValidationMessage { get; set; } = "";

    private void ValidateAndSave(Syncfusion.Blazor.Calendars.ChangeEventArgs<DateTime?> args)
    {
        SelectedTime = args.Value;

        // Business logic
        if (SelectedTime?.Hour < 9 || SelectedTime?.Hour >= 17)
        {
            ValidationMessage = "⚠️ Outside business hours (9 AM - 5 PM)";
        }
        else
        {
            ValidationMessage = "✓ Valid time selected";
            SaveToDatabase(args.Value);
        }
    }

    private void SaveToDatabase(DateTime? time)
    {
        // Save logic here
    }
}
```

## Focus Events

Focus and Blur events track when the input field gains or loses focus.

### Focus Event

Fires when the input field receives focus:

```razor
@using Syncfusion.Blazor.Calendars

<p>Focus Status: @FocusStatus</p>

<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" Focus="OnFocus"></TimePickerEvents>
</SfTimePicker>

@code {
    public string FocusStatus { get; set; } = "Not focused";

    private void OnFocus(Syncfusion.Blazor.Calendars.FocusEventArgs args)
    {
        FocusStatus = "Input has focus";
        Console.WriteLine("User focused on TimePicker input");
    }
}
```

### Blur Event

Fires when the input field loses focus:

```razor
@using Syncfusion.Blazor.Calendars

<p>Input Status: @InputStatus</p>

<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" Blur="OnBlur"></TimePickerEvents>
</SfTimePicker>

@code {
    public string InputStatus { get; set; } = "Not focused";

    private void OnBlur(Syncfusion.Blazor.Calendars.BlurEventArgs args)
    {
        InputStatus = "Input lost focus";
        // Validate or save on blur
        ValidateInput();
    }

    private void ValidateInput()
    {
        // Validation logic
    }
}
```

### Combined Focus/Blur Pattern

```razor
@using Syncfusion.Blazor.Calendars

<div style="border: @BorderStyle; padding: 10px;">
    <SfTimePicker TValue="DateTime?" 
        Placeholder="Click to focus"
        @bind-Value="@SelectedTime">
        <TimePickerEvents TValue="DateTime?" 
            Focus="@(() => IsFocused = true)"
            Blur="@(() => IsFocused = false)">
        </TimePickerEvents>
    </SfTimePicker>
</div>

@code {
    public DateTime? SelectedTime { get; set; }
    public bool IsFocused { get; set; }

    private string BorderStyle => IsFocused ? "2px solid blue" : "1px solid gray";
}
```

## Popup Events

OnOpen and OnClose events track popup visibility changes.

### OnOpen Event

Fires when the time picker popup opens:

```razor
@using Syncfusion.Blazor.Calendars

<p>Popup Status: @PopupStatus</p>

<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" OnOpen="PopupOpened"></TimePickerEvents>
</SfTimePicker>

@code {
    public string PopupStatus { get; set; } = "Closed";

    private void PopupOpened(Syncfusion.Blazor.Calendars.PopupEventArgs args)
    {
        PopupStatus = "Popup is open";
        Console.WriteLine("TimePicker popup opened");
        // Log analytics, track user interaction
    }
}
```

### OnClose Event

Fires when the time picker popup closes:

```razor
@using Syncfusion.Blazor.Calendars

<p>Last Close: @LastClosedAt</p>

<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" OnClose="PopupClosed"></TimePickerEvents>
</SfTimePicker>

@code {
    public string LastClosedAt { get; set; } = "Not closed yet";

    private void PopupClosed(Syncfusion.Blazor.Calendars.PopupEventArgs args)
    {
        LastClosedAt = DateTime.Now.ToString("HH:mm:ss");
        Console.WriteLine("TimePicker popup closed");
    }
}
```

### Popup Event Pattern

```razor
@using Syncfusion.Blazor.Calendars

<div>
    <p>Popup Open: @IsPopupOpen</p>
    
    <SfTimePicker TValue="DateTime?">
        <TimePickerEvents TValue="DateTime?" 
            OnOpen="@(() => IsPopupOpen = true)"
            OnClose="@(() => IsPopupOpen = false)">
        </TimePickerEvents>
    </SfTimePicker>
</div>

@code {
    public bool IsPopupOpen { get; set; }
}
```

## Lifecycle Events

Created and Destroyed events track component lifecycle.

### Created Event

Fires when the component is initialized:

```razor
@using Syncfusion.Blazor.Calendars

<p>Component Status: @ComponentStatus</p>

<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" Created="ComponentCreated"></TimePickerEvents>
</SfTimePicker>

@code {
    public string ComponentStatus { get; set; } = "Not initialized";

    private void ComponentCreated(object args)
    {
        ComponentStatus = "Component initialized";
        Console.WriteLine("TimePicker component created");
        InitializeDefaults();
    }

    private void InitializeDefaults()
    {
        // Set up default values, load data, etc.
    }
}
```

### Destroyed Event

Fires when the component is destroyed:

```razor
@using Syncfusion.Blazor.Calendars

<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" Destroyed="ComponentDestroyed"></TimePickerEvents>
</SfTimePicker>

@code {
    private void ComponentDestroyed(object args)
    {
        Console.WriteLine("TimePicker component destroyed");
        // Clean up resources
    }
}
```

## Item Render Event

The OnItemRender event fires for each item in the popup list as it's being rendered. This allows customization of individual time items.

### Basic OnItemRender

```razor
@using Syncfusion.Blazor.Calendars

<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" OnItemRender="OnItemRenderHandler"></TimePickerEvents>
</SfTimePicker>

@code {
    private void OnItemRenderHandler(Syncfusion.Blazor.Calendars.ItemEventArgs<DateTime?> args)
    {
        // args.ItemData contains the time being rendered
        Console.WriteLine($"Rendering: {args.ItemData}");
    }
}
```

### Custom Item Styling with OnItemRender

```razor
@using Syncfusion.Blazor.Calendars

<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" OnItemRender="HighlightBusinessHours"></TimePickerEvents>
</SfTimePicker>

@code {
    private void HighlightBusinessHours(Syncfusion.Blazor.Calendars.ItemEventArgs<DateTime?> args)
    {
        if (args.ItemData != null)
        {
            int hour = args.ItemData.Value.Hour;
            
            // Highlight business hours (9-17)
            if (hour >= 9 && hour < 17)
            {
                args.HtmlAttributes = new Dictionary<string, object>
                {
                    { "style", "background-color: #d4edda; color: green;" }
                };
            }
        }
    }
}
```

## Event Handler Implementation Patterns

### Pattern 1: Async Event Handler

```razor
@using Syncfusion.Blazor.Calendars

<p>Loading: @IsLoading</p>
<p>Result: @Result</p>

<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" ValueChange="OnTimeSelectedAsync"></TimePickerEvents>
</SfTimePicker>

@code {
    public bool IsLoading { get; set; }
    public string Result { get; set; } = "";

    private async Task OnTimeSelectedAsync(Syncfusion.Blazor.Calendars.ChangeEventArgs<DateTime?> args)
    {
        IsLoading = true;

        try
        {
            // Simulate API call
            await Task.Delay(1000);
            Result = $"Time {args.Value?.ToString("HH:mm")} processed";
        }
        finally
        {
            IsLoading = false;
        }
    }
}
```

### Pattern 2: Multiple Events

```razor
@using Syncfusion.Blazor.Calendars

<p>Selected: @SelectedTime?.ToString("HH:mm")</p>
<p>Events: @EventLog</p>

<SfTimePicker TValue="DateTime?" @bind-Value="@SelectedTime">
    <TimePickerEvents TValue="DateTime?" 
        ValueChange="LogEvent"
        OnOpen="@((PopupEventArgs e) => LogEvent("OnOpen"))"
        OnClose="@((PopupEventArgs e) => LogEvent("OnClose"))"
        Focus="@((FocusEventArgs e) => LogEvent("Focus"))"
        Blur="@((BlurEventArgs e) => LogEvent("Blur"))">
    </TimePickerEvents>
</SfTimePicker>

@code {
    public DateTime? SelectedTime { get; set; }
    public List<string> Events { get; set; } = new();
    public string EventLog => string.Join(" → ", Events.TakeLast(5));

    private void LogEvent(string eventName)
    {
        Events.Add(eventName);
    }

    private void LogEvent(Syncfusion.Blazor.Calendars.ChangeEventArgs<DateTime?> args)
    {
        Events.Add($"ValueChange:{args.Value?.ToString("HH:mm")}");
    }
}
```

### Pattern 3: Event with State Management

```razor
@using Syncfusion.Blazor.Calendars

<p>Selected: @SelectedTime?.ToString("HH:mm")</p>
<p>Validation: @ValidationState</p>

<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" ValueChange="ValidateTimeSelection"></TimePickerEvents>
</SfTimePicker>

@code {
    public DateTime? SelectedTime { get; set; }
    public string ValidationState { get; set; } = "Pending";

    private void ValidateTimeSelection(Syncfusion.Blazor.Calendars.ChangeEventArgs<DateTime?> args)
    {
        SelectedTime = args.Value;

        // Validation state machine
        if (SelectedTime == null)
        {
            ValidationState = "Empty";
        }
        else if (SelectedTime.Value.Hour < 9)
        {
            ValidationState = "⚠️ Too early";
        }
        else if (SelectedTime.Value.Hour >= 17)
        {
            ValidationState = "⚠️ Too late";
        }
        else
        {
            ValidationState = "✓ Valid";
        }

        StateHasChanged();
    }
}
```

## Common Event Scenarios

### Scenario 1: Auto-Save on Selection

```razor
@using Syncfusion.Blazor.Calendars

<p>Auto-saved: @AutoSavedTime?.ToString("HH:mm")</p>

<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" ValueChange="AutoSave"></TimePickerEvents>
</SfTimePicker>

@code {
    public DateTime? AutoSavedTime { get; set; }

    private async Task AutoSave(Syncfusion.Blazor.Calendars.ChangeEventArgs<DateTime?> args)
    {
        // Save to database immediately
        await ApiClient.SavePreferredTime(args.Value);
        AutoSavedTime = args.Value;
    }
}
```

### Scenario 2: Cascading Pickers

```razor
@using Syncfusion.Blazor.Calendars

<p>Start: @StartTime?.ToString("HH:mm")</p>
<p>End: @EndTime?.ToString("HH:mm")</p>
<p>Duration: @GetDuration() minutes</p>

<label>Start Time:</label>
<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" ValueChange="OnStartTimeChanged"></TimePickerEvents>
</SfTimePicker>

<label>End Time:</label>
<SfTimePicker TValue="DateTime?" Min="@StartTime">
    <TimePickerEvents TValue="DateTime?" ValueChange="OnEndTimeChanged"></TimePickerEvents>
</SfTimePicker>

@code {
    public DateTime? StartTime { get; set; }
    public DateTime? EndTime { get; set; }

    private void OnStartTimeChanged(Syncfusion.Blazor.Calendars.ChangeEventArgs<DateTime?> args)
    {
        StartTime = args.Value;
        // Reset end time if before start time
        if (EndTime.HasValue && EndTime < StartTime)
            EndTime = null;
    }

    private void OnEndTimeChanged(Syncfusion.Blazor.Calendars.ChangeEventArgs<DateTime?> args)
    {
        EndTime = args.Value;
    }

    private int GetDuration()
    {
        if (StartTime == null || EndTime == null) return 0;
        return (int)(EndTime.Value - StartTime.Value).TotalMinutes;
    }
}
```

### Scenario 3: Analytics Tracking

```razor
@using Syncfusion.Blazor.Calendars

<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" 
        OnOpen="TrackOpen"
        OnClose="TrackClose"
        ValueChange="TrackSelection">
    </TimePickerEvents>
</SfTimePicker>

@code {
    private void TrackOpen(Syncfusion.Blazor.Calendars.PopupEventArgs args)
    {
        AnalyticsService.Track("timepicker_opened", new { timestamp = DateTime.Now });
    }

    private void TrackClose(Syncfusion.Blazor.Calendars.PopupEventArgs args)
    {
        AnalyticsService.Track("timepicker_closed", new { timestamp = DateTime.Now });
    }

    private void TrackSelection(Syncfusion.Blazor.Calendars.ChangeEventArgs<DateTime?> args)
    {
        AnalyticsService.Track("time_selected", new { value = args.Value });
    }
}
```

## See Also

- [Data Binding →](../data-binding.md)
- [Time Formats →](../time-formats.md)
- [Getting Started →](../getting-started.md)
