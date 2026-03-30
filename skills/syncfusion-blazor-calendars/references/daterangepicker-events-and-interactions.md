# Events & Interactions

## Table of Contents
- [Component Events](#component-events)
- [Event Handler Patterns](#event-handler-patterns)
- [Native HTML Events](#native-html-events)
- [Date Change Handling](#date-change-handling)
- [Focus and Blur Events](#focus-and-blur-events)
- [Common Event Workflows](#common-event-workflows)

## Component Events

### ValueChange Event

The `ValueChange` event fires when the user completes a date range selection:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    Placeholder="Select dates">
    <DateRangePickerEvents TValue="DateTime?" ValueChange="@OnDateRangeChanged"></DateRangePickerEvents>
</SfDateRangePicker>

<p>Last changed: @LastChangeTime</p>

@code {
    private string LastChangeTime { get; set; }
    
    private void OnDateRangeChanged(RangePickerEventArgs<DateTime?> args)
    {
        LastChangeTime = DateTime.Now.ToString("HH:mm:ss");
        Console.WriteLine($"Range changed at {LastChangeTime}");
    }
}
```

### RangePickerEventArgs Properties

Access information about the date change:

```razor
@code {
    private void OnDateRangeChanged(RangePickerEventArgs<DateTime?> args)
    {
        // Event properties:
        var startDate = args.StartDate;     // Start date of range
        var endDate = args.EndDate;         // End date of range
        var isInteracted = args.IsInteracted;             // Interaction
        var text = args.Text;               // Formatted text value
    }
}
```

### OnOpen Event

The `OnOpen` event fires when the calendar popup opens:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    Placeholder="Click to open">
    <DateRangePickerEvents TValue="DateTime?" OnOpen="@OnCalendarOpen"></DateRangePickerEvents>
</SfDateRangePicker>

@code {
    private void OnCalendarOpen(RangePopupEventArgs args)
    {
        Console.WriteLine("Calendar popup opened");
        // Could load data, update hints, etc.
    }
}
```

### OnClose Event

The `OnClose` event fires when the calendar popup closes:

```razor
<SfDateRangePicker TValue="DateTime?" 
    Placeholder="Select dates">
    <DateRangePickerEvents TValue="DateTime?" OnClose="@OnCalendarClose"></DateRangePickerEvents>
</SfDateRangePicker>

@code {
    private void OnCalendarClose(RangePopupEventArgs args)
    {
        Console.WriteLine("Calendar popup closed");
        // Save user interaction, track usage, etc.
    }
}
```

## Event Handler Patterns

### Async Event Handlers

Use async handlers for operations like API calls:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    Placeholder="Select dates">
    <DateRangePickerEvents TValue="DateTime?" ValueChange="@OnDateRangeSelectedAsync"></DateRangePickerEvents>
</SfDateRangePicker>

<p>@StatusMessage</p>

@code {
    private string StatusMessage { get; set; }
    
    private async Task OnDateRangeSelectedAsync(RangePickerEventArgs<DateTime?> args)
    {
        StatusMessage = "Loading...";
        
        // Fetch data for selected range
        var data = await FetchDataForRange(args.StartDate, args.EndDate);
        
        StatusMessage = $"Loaded {data.Count} records";
    }
    
    private async Task<List<Record>> FetchDataForRange(DateTime? startDate, DateTime? endDate)
    {
        // API call
        await Task.Delay(500);  // Simulate API call
        return new List<Record>();
    }
}
```

### Multiple Event Handlers

Handle multiple events in sequence:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    Placeholder="Select dates">
    <DateRangePickerEvents TValue="DateTime?" 
        OnOpen="@OnCalendarOpen"
        OnClose="@OnCalendarClose"
        ValueChange="@OnDateRangeChanged">
    </DateRangePickerEvents>
</SfDateRangePicker>

@code {
    private void OnCalendarOpen(RangePopupEventArgs args)
    {
        Console.WriteLine("1. Calendar opened");
    }
    
    private void OnCalendarClose(RangePopupEventArgs args)
    {
        Console.WriteLine("3. Calendar closed");
    }
    
    private void OnDateRangeChanged(RangePickerEventArgs<DateTime?> args)
    {
        Console.WriteLine("2. Date range changed");
    }
}
```

### Event Propagation Control

Control whether events propagate:

```razor
<SfDateRangePicker TValue="DateTime?" 
    Placeholder="Select dates">
    <DateRangePickerEvents TValue="DateTime?" ValueChange="@((RangePickerEventArgs<DateTime?> args) => HandleChange(args))"></DateRangePickerEvents>
</SfDateRangePicker>

@code {
    private void HandleChange(RangePickerEventArgs<DateTime?> args)
    {
        if (IsValidRange(args.StartDate, args.EndDate))
        {
            // Process event
            ProcessDateRange(args.StartDate, args.EndDate);
        }
        else
        {
            // Reject or warn about invalid selection
            Console.WriteLine("Invalid range selected");
        }
    }
}
```

## Native HTML Events

### Focus and Blur Events

Handle native HTML focus/blur events:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    @onfocus="@OnInputFocus"
    @onblur="@OnInputBlur"
    Placeholder="Select dates">
</SfDateRangePicker>

@code {
    private void OnInputFocus(FocusEventArgs args)
    {
        Console.WriteLine("Input field focused");
    }
    
    private void OnInputBlur(FocusEventArgs args)
    {
        Console.WriteLine("Input field lost focus");
    }
}
```

### Keyboard Events

Handle keyboard interactions:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    @onkeydown="@OnKeyDown"
    @onkeyup="@OnKeyUp"
    Placeholder="Select dates">
</SfDateRangePicker>

@code {
    private void OnKeyDown(KeyboardEventArgs args)
    {
        Console.WriteLine($"Key pressed: {args.Key}");
        
        if (args.Key == "Enter")
        {
            // Handle Enter key
            ProcessSelection();
        }
        else if (args.Key == "Escape")
        {
            // Handle Escape key (typically closes popup)
        }
    }
    
    private void OnKeyUp(KeyboardEventArgs args)
    {
        Console.WriteLine($"Key released: {args.Key}");
    }
    
    private void ProcessSelection()
    {
        Console.WriteLine("Selection submitted");
    }
}
```

### Input Events

Track raw input as user types:

```razor
<SfDateRangePicker TValue="DateTime?" 
    @oninput="@OnInput"
    Placeholder="Select dates">
</SfDateRangePicker>

@code {
    private void OnInput(ChangeEventArgs args)
    {
        var inputValue = args.Value?.ToString();
        Console.WriteLine($"Input: {inputValue}");
    }
}
```

## Date Change Handling

### Detecting Range Changes

Respond to date range selection changes:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    @bind-StartDate="@StartDate"
    @bind-EndDate="@EndDate">
    <DateRangePickerEvents TValue="DateTime?" ValueChange="@DetectRangeChange"></DateRangePickerEvents>
</SfDateRangePicker>

<p>@RangeStatus</p>

@code {
    private DateTime? StartDate { get; set; }
    private DateTime? EndDate { get; set; }
    private string RangeStatus { get; set; }
    
    private void DetectRangeChange(RangePickerEventArgs<DateTime?> args)
    {
        if (args.StartDate != null && args.EndDate != null)
        {
            RangeStatus = $"Range updated: {args.StartDate:d} to {args.EndDate:d}";
        }
    }
}
```

### Filtering Data by Date Range

Update displayed data when range changes:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    @bind-StartDate="@StartDate"
    @bind-EndDate="@EndDate">
    <DateRangePickerEvents TValue="DateTime?" ValueChange="@RefreshData"></DateRangePickerEvents>
</SfDateRangePicker>

<table>
    <tr>
        <th>Date</th>
        <th>Value</th>
    </tr>
    @foreach (var item in FilteredData)
    {
        <tr>
            <td>@item.Date.ToString("d")</td>
            <td>@item.Value</td>
        </tr>
    }
</table>

@code {
    private DateTime? StartDate { get; set; }
    private DateTime? EndDate { get; set; }
    private List<DataItem> AllData { get; set; } = new();
    private List<DataItem> FilteredData { get; set; } = new();
    
    protected override void OnInitialized()
    {
        // Load all data
        AllData = new()
        {
            new() { Date = DateTime.Today.AddDays(-5), Value = 100 },
            new() { Date = DateTime.Today.AddDays(-3), Value = 200 },
            new() { Date = DateTime.Today, Value = 150 }
        };
    }
    
    private void RefreshData(RangePickerEventArgs<DateTime?> args)
    {
        if (args.StartDate.HasValue && args.EndDate.HasValue)
        {
            FilteredData = AllData
                .Where(x => x.Date >= args.StartDate.Value && x.Date <= args.EndDate.Value)
                .ToList();
        }
    }
    
    private class DataItem
    {
        public DateTime Date { get; set; }
        public int Value { get; set; }
    }
}
```

### Cascading Date Ranges

Update second picker based on first selection:

```razor
@using Syncfusion.Blazor.Calendars

<label>First Range:</label>
<SfDateRangePicker TValue="DateTime?" 
    @bind-StartDate="@FirstStartDate"
    @bind-EndDate="@FirstEndDate">
    <DateRangePickerEvents TValue="DateTime?" ValueChange="@OnFirstRangeChanged"></DateRangePickerEvents>
</SfDateRangePicker>

<label>Second Range:</label>
<SfDateRangePicker TValue="DateTime?" 
    @bind-StartDate="@SecondStartDate"
    @bind-EndDate="@SecondEndDate"
    Min="@FirstEndDate">
</SfDateRangePicker>

@code {
    private DateTime? FirstStartDate { get; set; }
    private DateTime? FirstEndDate { get; set; }
    private DateTime? SecondStartDate { get; set; }
    private DateTime? SecondEndDate { get; set; }
    
    private void OnFirstRangeChanged(RangePickerEventArgs<DateTime?> args)
    {
        // Clear second range if it overlaps with new first range
        if (SecondStartDate.HasValue && SecondStartDate < args.EndDate)
        {
            SecondStartDate = null;
            SecondEndDate = null;
        }
    }
}
```

## Focus and Blur Events

### Focus Handling

Respond when component receives focus:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    @ref="DatePickerRef"
    @onfocus="@OnFocus"
    Placeholder="Click to focus">
</SfDateRangePicker>

<p>Focused: @IsFocused</p>

@code {
    private SfDateRangePicker<DateTime?> DatePickerRef;
    private bool IsFocused { get; set; }
    
    private void OnFocus(FocusEventArgs args)
    {
        IsFocused = true;
    }
}
```

### Blur Handling (Validation)

Validate when component loses focus:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    @bind-Value="DateRange"
    @onblur="@OnBlur">
</SfDateRangePicker>

<p style="color: @(ValidationError ? "red" : "green")">@ValidationMessage</p>

@code {
    private DateTime? DateRange { get; set; }
    private bool ValidationError { get; set; }
    private string ValidationMessage { get; set; }
    
    private void OnBlur(FocusEventArgs args)
    {
        ValidateDateRange();
    }
    
    private void ValidateDateRange()
    {
        if (!DateRange.HasValue)
        {
            ValidationError = true;
            ValidationMessage = "Please select a date range";
        }
        else if (DateRange.Value < DateTime.Today)
        {
            ValidationError = true;
            ValidationMessage = "Date cannot be in the past";
        }
        else
        {
            ValidationError = false;
            ValidationMessage = "✓ Valid date range";
        }
    }
}
```

## Common Event Workflows

### Workflow 1: Search with Date Range

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    @bind-StartDate="@SearchStartDate"
    @bind-EndDate="@SearchEndDate">
    <DateRangePickerEvents TValue="DateTime?" ValueChange="@PerformSearch"></DateRangePickerEvents>
</SfDateRangePicker>

<div>@SearchStatus</div>

@code {
    private DateTime? SearchStartDate { get; set; }
    private DateTime? SearchEndDate { get; set; }
    private string SearchStatus { get; set; }
    
    private async Task PerformSearch(RangePickerEventArgs<DateTime?> args)
    {
        if (!args.StartDate.HasValue || !args.EndDate.HasValue)
            return;
        
        SearchStatus = "Searching...";
        
        // Simulate search
        await Task.Delay(1000);
        
        SearchStatus = "Search complete";
    }
}
```

### Workflow 2: Form Submission

```razor
@using Syncfusion.Blazor.Calendars

<EditForm Model="@Report" OnValidSubmit="@SubmitReport">
    <DataAnnotationsValidator />
    
    <label>Report Period:</label>
    <SfDateRangePicker TValue="DateTime?" 
        @bind-StartDate="@Report.StartDate"
        @bind-EndDate="@Report.EndDate">
        <DateRangePickerEvents TValue="DateTime?" ValueChange="@UpdateReportPreview"></DateRangePickerEvents>
    </SfDateRangePicker>
    
    <button type="submit">Generate Report</button>
</EditForm>

@code {
    private class Report
    {
        public DateTime? StartDate { get; set; }
        public DateTime? EndDate { get; set; }
    }
    
    private Report Report = new();
    
    private void UpdateReportPreview(RangePickerEventArgs<DateTime?> args)
    {
        // Preview updates when date changes
    }
    
    private async Task SubmitReport()
    {
        if (Report.StartDate.HasValue && Report.EndDate.HasValue)
        {
            // Submit report
            // await ApiService.SubmitReportAsync(Report);
            Console.WriteLine("Report submitted");
        }
    }
}
```

### Workflow 3: Real-Time Updates

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    @bind-StartDate="@StartDate"
    @bind-EndDate="@EndDate">
    <DateRangePickerEvents TValue="DateTime?" ValueChange="@OnRangeChange"></DateRangePickerEvents>
</SfDateRangePicker>

<div>
    @foreach (var item in RealtimeData)
    {
        <p>@item</p>
    }
</div>

@code {
    private DateTime? StartDate { get; set; }
    private DateTime? EndDate { get; set; }
    private List<string> RealtimeData { get; set; } = new();
    
    private async Task OnRangeChange(RangePickerEventArgs<DateTime?> args)
    {
        RealtimeData.Clear();
        RealtimeData.Add("Loading data...");
        
        // Fetch real-time data
        var data = await FetchRealtimeData(args.StartDate, args.EndDate);
        RealtimeData = data;
    }
    
    private async Task<List<string>> FetchRealtimeData(DateTime? startDate, DateTime? endDate)
    {
        await Task.Delay(500);
        return new() { "Data 1", "Data 2", "Data 3" };
    }
}
```

## Best Practices

1. **Always check HasValue** for nullable date types
2. **Use async handlers** for time-consuming operations
3. **Provide visual feedback** during data operations
4. **Validate input** in OnChange handlers
5. **Handle errors gracefully** in async operations
6. **Update dependent fields** when date changes
7. **Provide clear messages** to users

## Next Steps

- Read [customization-and-styling.md](customization-and-styling.md) for visual customization
- Read [accessibility.md](accessibility.md) for keyboard navigation details
- Read [range-selection.md](range-selection.md) for data binding patterns
