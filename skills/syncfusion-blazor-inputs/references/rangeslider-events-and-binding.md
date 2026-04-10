# RangeSlider - Events and Data Binding

## Table of Contents
- [Overview](#overview)
- [SliderEvents Component](#sliderevents-component)
- [ValueChange Event](#valuechange-event)
- [OnChange Events](#onchange)
- [Created Event](#created-event)
- [Rendered Event](#rendered-event)
- [Tooltip Events](#tooltip-events)
- [Tick Render Events](#tick-render-events)
- [Form Integration](#form-integration)
- [Validation with EditContext](#validation-with-editcontext)

## Overview

The RangeSlider component provides comprehensive event handling for tracking user interactions, value changes, and component lifecycle. Events enable you to implement custom validation, trigger side effects, update related UI elements, and integrate with Blazor forms. Understanding event timing and usage patterns is essential for building responsive and interactive applications.

## SliderEvents Component

### Basic Event Configuration

Add `SliderEvents` component to handle slider events:

```razor
@using Syncfusion.Blazor.Inputs

<div class="event-demo">
    <h4>Range Slider with Events</h4>
    <SfSlider @bind-Value="@rangeValues"
              Type="SliderType.Range"
              Min="0"
              Max="100">
        <SliderEvents TValue="int[]" 
                      ValueChange="OnValueChange"
                      Created="OnCreated"
                      OnChange="OnChange">
        </SliderEvents>
        <SliderTicks Placement="Placement.After" LargeStep="20"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    
    <div class="event-log">
        <h5>Event Log:</h5>
        <p>@eventMessage</p>
    </div>
</div>

@code {
    private int[] rangeValues = new int[] { 30, 70 };
    private string eventMessage = "No events yet";
    
    private void OnValueChange(SliderChangeEventArgs<int[]> args)
    {
        eventMessage = $"ValueChange: [{args.Value[0]}, {args.Value[1]}]";
    }
    
    private void OnCreated()
    {
        eventMessage = "Slider created";
    }
    
    private void OnChange(SliderChangeEventArgs<int[]> args)
    {
        eventMessage = $"OnChange: [{args.Value[0]}, {args.Value[1]}]";
    }
}
```

**Key Points:**
- `SliderEvents` is a child component of `SfSlider`
- `TValue` must match the slider value type (int[] or double[])
- Multiple events can be configured simultaneously
- Event handlers receive `SliderChangeEventArgs<T>` with current values

## ValueChange Event

### Real-Time Value Updates

`ValueChange` event fires continuously as the user drags handles:

```razor
<div class="value-change-demo">
    <h4>Live Price Filter</h4>
    <SfSlider @bind-Value="@priceRange"
              Type="SliderType.Range"
              Min="0"
              Max="5000"
              Step="50">
        <SliderEvents TValue="int[]" ValueChange="OnPriceChange"></SliderEvents>
        <SliderTicks Placement="Placement.After" LargeStep="1000" Format="C0"></SliderTicks>
        <SliderTooltip IsVisible="true" Format="C0" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    
    <div class="product-count">
        <p>🔍 Filtering products: @priceRange[0].ToString("C0") - @priceRange[1].ToString("C0")</p>
        <p>Found: <strong>@filteredProductCount</strong> products</p>
    </div>
</div>

@code {
    private int[] priceRange = new int[] { 500, 3000 };
    private int filteredProductCount = 0;
    
    private void OnPriceChange(SliderChangeEventArgs<int[]> args)
    {
        // Real-time product filtering
        filteredProductCount = GetProductCountInRange(args.Value[0], args.Value[1]);
        StateHasChanged();
    }
    
    private int GetProductCountInRange(int min, int max)
    {
        // Simulate product filtering logic
        return new Random().Next(50, 500);
    }
}
```

### With IsImmediateValue

Control when `ValueChange` fires:

```razor
<div class="immediate-value-comparison">
    <!-- Fires during drag -->
    <div>
        <h4>Immediate Updates (During Drag)</h4>
        <SfSlider @bind-Value="@range1"
                  Type="SliderType.Range"
                  Min="0"
                  Max="100"
                  IsImmediateValue="true">
            <SliderEvents TValue="int[]" ValueChange="OnImmediate"></SliderEvents>
            <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
        </SfSlider>
        <p>Update count: @immediateCount</p>
    </div>
    
    <!-- Fires on release -->
    <div>
        <h4>On Release Only</h4>
        <SfSlider @bind-Value="@range2"
                  Type="SliderType.Range"
                  Min="0"
                  Max="100"
                  IsImmediateValue="false">
            <SliderEvents TValue="int[]" ValueChange="OnRelease"></SliderEvents>
            <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
        </SfSlider>
        <p>Update count: @releaseCount</p>
    </div>
</div>

@code {
    private int[] range1 = new int[] { 30, 70 };
    private int[] range2 = new int[] { 30, 70 };
    private int immediateCount = 0;
    private int releaseCount = 0;
    
    private void OnImmediate(SliderChangeEventArgs<int[]> args)
    {
        immediateCount++;
    }
    
    private void OnRelease(SliderChangeEventArgs<int[]> args)
    {
        releaseCount++;
    }
}
```

## OnChange
### OnChange Event

Fires **before** the value is updated:

```razor
<div class="onchange-demo">
    <h4>OnChange (Before Update)</h4>
    <SfSlider @bind-Value="@rangeValues"
              Type="SliderType.Range"
              Min="0"
              Max="100">
        <SliderEvents TValue="int[]" OnChange="OnChangeHandler"></SliderEvents>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Previous: @previousMessage</p>
    <p>Current: [@rangeValues[0], @rangeValues[1]]</p>
</div>

@code {
    private int[] rangeValues = new int[] { 30, 70 };
    private string previousMessage = "";
    
    private void OnChangeHandler(SliderChangeEventArgs<int[]> args)
    {
        // Access previous values before update
        previousMessage = $"Before: [{rangeValues[0]}, {rangeValues[1]}] → After: [{args.Value[0]}, {args.Value[1]}]";
    }
}
```

### Event Timing Comparison

```razor
<div class="event-timing-demo">
    <h4>Event Sequence Demonstration</h4>
    <SfSlider @bind-Value="@rangeValues"
              Type="SliderType.Range"
              Min="0"
              Max="100">
        <SliderEvents TValue="int[]"
                      OnChange="LogOnChange"
                      ValueChange="LogValueChange">
        </SliderEvents>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    
    <div class="event-sequence">
        <h5>Event Sequence:</h5>
        <ol>
            @foreach (var log in eventLogs)
            {
                <li>@log</li>
            }
        </ol>
        <button @onclick="ClearLogs">Clear Logs</button>
    </div>
</div>

@code {
    private int[] rangeValues = new int[] { 30, 70 };
    private List<string> eventLogs = new List<string>();
    
    private void LogOnChange(SliderChangeEventArgs<int[]> args)
    {
        eventLogs.Add($"1. OnChange: [{args.Value[0]}, {args.Value[1]}]");
    }
    
    private void LogValueChange(SliderChangeEventArgs<int[]> args)
    {
        eventLogs.Add($"2. ValueChange: [{args.Value[0]}, {args.Value[1]}]");
    }
    
    private void ClearLogs()
    {
        eventLogs.Clear();
    }
}
```

**Event Order:**
1. **OnChange**: Fires first, before value update
2. **ValueChange**: Fires during value update

## Created Event

### Initialization Logic

Execute code when the slider is created:

```razor
<div class="created-demo">
    <h4>Slider with Initialization</h4>
    <SfSlider @bind-Value="@rangeValues"
              Type="SliderType.Range"
              Min="0"
              Max="100">
        <SliderEvents TValue="int[]" Created="OnSliderCreated"></SliderEvents>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Status: @initStatus</p>
</div>

@code {
    private int[] rangeValues = new int[] { 30, 70 };
    private string initStatus = "Not initialized";
    
    private void OnSliderCreated()
    {
        initStatus = $"Slider initialized with range [{rangeValues[0]}, {rangeValues[1]}]";
        
        // Perform initialization logic
        LoadDefaultSettings();
        LogSliderCreation();
    }
    
    private void LoadDefaultSettings()
    {
        // Load user preferences, default values, etc.
    }
    
    private void LogSliderCreation()
    {
        Console.WriteLine($"Range slider created: {DateTime.Now}");
    }
}
```

### Async Initialization

```razor
<div class="async-init-demo">
    <h4>Slider with Async Initialization</h4>
    <SfSlider @bind-Value="@rangeValues"
              Type="SliderType.Range"
              Min="0"
              Max="100">
        <SliderEvents TValue="int[]" Created="OnSliderCreatedAsync"></SliderEvents>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>@loadingMessage</p>
</div>

@code {
    private int[] rangeValues = new int[] { 30, 70 };
    private string loadingMessage = "Loading...";
    
    private async Task OnSliderCreatedAsync()
    {
        loadingMessage = "Loading preferences...";
        
        // Simulate async data loading
        await Task.Delay(1000);
        var savedRange = await LoadSavedRangeFromDatabase();
        
        if (savedRange != null)
        {
            rangeValues = savedRange;
            loadingMessage = "Preferences loaded";
            StateHasChanged();
        }
        else
        {
            loadingMessage = "Using default values";
        }
    }
    
    private async Task<int[]> LoadSavedRangeFromDatabase()
    {
        // Simulate database call
        await Task.Delay(500);
        return new int[] { 20, 80 };
    }
}
```

## Tooltip Events

### OnTooltipChange Event

Customize tooltip content dynamically:

```razor
<div class="tooltip-event-demo">
    <h4>Custom Tooltip Content</h4>
    <SfSlider @bind-Value="@temperatureRange"
              Type="SliderType.Range"
              Min="-20"
              Max="50">
        <SliderEvents TValue="int[]" OnTooltipChange="CustomizeTooltip"></SliderEvents>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
        <SliderTicks Placement="Placement.After" LargeStep="10"></SliderTicks>
    </SfSlider>
</div>

@code {
    private int[] temperatureRange = new int[] { 18, 26 };
    
    private void CustomizeTooltip(SliderTooltipEventArgs<int[]> args)
    {
        // Customize tooltip text based on value
        int value = args.Value[args.HandleIndex ?? 0];
        
        if (value < 0)
            args.Text = $"❄️ {value}°C (Freezing)";
        else if (value >= 0 && value < 15)
            args.Text = $"🥶 {value}°C (Cold)";
        else if (value >= 15 && value < 25)
            args.Text = $"😊 {value}°C (Comfortable)";
        else
            args.Text = $"🔥 {value}°C (Hot)";
    }
}
```

## Tick Render Events

### TicksRender Event

Customize tick labels dynamically:

```razor
<div class="tick-render-demo">
    <h4>Custom Tick Labels</h4>
    <SfSlider @bind-Value="@monthRange"
              Type="SliderType.Range"
              Min="1"
              Max="12">
        <SliderEvents TValue="int[]" TicksRendering="CustomizeTickLabels"></SliderEvents>
        <SliderTicks Placement="Placement.After" LargeStep="1"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Selected months: @GetMonthName(monthRange[0]) - @GetMonthName(monthRange[1])</p>
</div>

@code {
    private int[] monthRange = new int[] { 3, 9 };
    private string[] monthNames = new string[] 
    { 
        "", "Jan", "Feb", "Mar", "Apr", "May", "Jun",
        "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"
    };
    
    private void CustomizeTickLabels(SliderTickEventArgs args)
    {
        // Replace numeric tick labels with month names
        int value = (int)args.Value;
        if (value >= 1 && value <= 12)
        {
            args.Text = monthNames[value];
        }
    }
    
    private string GetMonthName(int month)
    {
        return month >= 1 && month <= 12 ? monthNames[month] : "";
    }
}
```

## Form Integration

### Basic Form Integration

```razor
<EditForm Model="@filterModel" OnValidSubmit="HandleValidSubmit">
    <div class="form-group">
        <label>Price Range</label>
        <SfSlider @bind-Value="@filterModel.PriceRange"
                  Type="SliderType.Range"
                  Min="0"
                  Max="10000"
                  Step="100">
            <SliderEvents TValue="int[]" ValueChange="OnPriceRangeChange"></SliderEvents>
            <SliderTicks Placement="Placement.After" LargeStep="2000" Format="C0"></SliderTicks>
            <SliderTooltip IsVisible="true" Format="C0" ShowOn="TooltipShowOn.Always"></SliderTooltip>
        </SfSlider>
    </div>
    
    <div class="form-group">
        <label>Age Range</label>
        <SfSlider @bind-Value="@filterModel.AgeRange"
                  Type="SliderType.Range"
                  Min="18"
                  Max="100">
            <SliderTicks Placement="Placement.After" LargeStep="20"></SliderTicks>
            <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
        </SfSlider>
    </div>
    
    <button type="submit" class="btn btn-primary">Apply Filters</button>
</EditForm>

<div class="filter-results">
    <p>Filtered: @resultMessage</p>
</div>

@code {
    private FilterModel filterModel = new FilterModel
    {
        PriceRange = new int[] { 1000, 5000 },
        AgeRange = new int[] { 25, 55 }
    };
    
    private string resultMessage = "";
    
    private void OnPriceRangeChange(SliderChangeEventArgs<int[]> args)
    {
        Console.WriteLine($"Price range changed: {args.Value[0]} - {args.Value[1]}");
    }
    
    private void HandleValidSubmit()
    {
        resultMessage = $"Filtering: Price {filterModel.PriceRange[0]:C0}-{filterModel.PriceRange[1]:C0}, " +
                       $"Age {filterModel.AgeRange[0]}-{filterModel.AgeRange[1]}";
    }
    
    public class FilterModel
    {
        public int[] PriceRange { get; set; }
        public int[] AgeRange { get; set; }
    }
}
```

## Validation with EditContext

### Custom Validation

```razor
<EditForm Model="@bookingModel" OnValidSubmit="HandleBookingSubmit">
    <DataAnnotationsValidator />
    
    <div class="form-group">
        <label>Booking Period (Minimum 3 nights)</label>
        <SfSlider @bind-Value="@bookingModel.DateRange"
                  Type="SliderType.Range"
                  Min="1"
                  Max="31">
            <SliderEvents TValue="int[]" ValueChange="ValidateDateRange"></SliderEvents>
            <SliderTicks Placement="Placement.After" LargeStep="5"></SliderTicks>
            <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
        </SfSlider>
        @if (!string.IsNullOrEmpty(validationError))
        {
            <div class="validation-error">@validationError</div>
        }
    </div>
    
    <div class="booking-summary">
        <p>Check-in: Day @bookingModel.DateRange[0]</p>
        <p>Check-out: Day @bookingModel.DateRange[1]</p>
        <p>Duration: @(bookingModel.DateRange[1] - bookingModel.DateRange[0]) nights</p>
    </div>
    
    <button type="submit" class="btn btn-primary" disabled="@(!IsValidRange())">
        Confirm Booking
    </button>
</EditForm>

@code {
    private BookingModel bookingModel = new BookingModel
    {
        DateRange = new int[] { 10, 15 }
    };
    
    private string validationError = "";
    
    private void ValidateDateRange(SliderChangeEventArgs<int[]> args)
    {
        int nights = args.Value[1] - args.Value[0];
        
        if (nights < 3)
        {
            validationError = "⚠️ Minimum stay is 3 nights";
        }
        else if (nights > 14)
        {
            validationError = "⚠️ Maximum stay is 14 nights";
        }
        else
        {
            validationError = "";
        }
    }
    
    private bool IsValidRange()
    {
        int nights = bookingModel.DateRange[1] - bookingModel.DateRange[0];
        return nights >= 3 && nights <= 14;
    }
    
    private void HandleBookingSubmit()
    {
        Console.WriteLine($"Booking confirmed: Day {bookingModel.DateRange[0]} to {bookingModel.DateRange[1]}");
    }
    
    public class BookingModel
    {
        [Required]
        public int[] DateRange { get; set; }
    }
}
```

**CSS for Validation:**
```css
.validation-error {
    color: #d32f2f;
    font-size: 14px;
    margin-top: 5px;
}

button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
}
```

## Best Practices

1. **Choose Right Event**: Use `ValueChange` for real-time
2. **Avoid Heavy Operations**: In `ValueChange` with `IsImmediateValue="true"`
3. **Debounce if Needed**: For expensive operations like API calls
4. **Use Created for Init**: Load saved preferences in `Created` event
5. **Validate in Events**: Implement validation logic in `ValueChange`
6. **Update UI with StateHasChanged**: When modifying component state in events

## Troubleshooting

### Events Not Firing
**Issue:** Event handlers not being called.

**Solution:** Ensure `SliderEvents` component is added with correct `TValue`:
```razor
<SliderEvents TValue="int[]" ValueChange="OnValueChange"></SliderEvents>
```

### Incorrect TValue Type
**Issue:** Runtime error about type mismatch.

**Solution:** Match `TValue` with slider value type:
```razor
<!-- For int[] -->
<SliderEvents TValue="int[]" ...></SliderEvents>

<!-- For double[] -->
<SliderEvents TValue="double[]" ...></SliderEvents>
```

## Next Steps

- **Advanced Validation** - Implement complex business rules
- **Async Operations** - Handle database calls and API requests
- **Performance Optimization** - Debounce and throttle events
- **State Management** - Integrate with Blazor state containers
