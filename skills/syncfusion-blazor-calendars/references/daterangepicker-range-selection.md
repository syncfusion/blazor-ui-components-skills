# Range Selection & Data Binding

## Table of Contents
- [Range Selection Overview](#range-selection-overview)
- [Data Binding Basics](#data-binding-basics)
- [Working with DateTime? Type](#working-with-datetime-type)
- [Working with DateOnly Type](#working-with-dateonly-type)
- [Programmatic Range Updates](#programmatic-range-updates)
- [Clearing Selections](#clearing-selections)
- [Single Date vs Range Selection](#single-date-vs-range-selection)

## Range Selection Overview

The DateRangePicker allows users to select a continuous date range across two calendars. The component stores both start and end dates as a single value. Understanding how the component handles date ranges is essential for implementing effective date selection interfaces.

### How Range Selection Works

When a user selects dates:
1. **First click** - Selects start date, first calendar highlights it
2. **Second click** - Selects end date, range is highlighted between both dates
3. **Value submitted** - Component stores the complete range

The selected range includes all dates from start to end (inclusive).

## Data Binding Basics

### Two-Way Binding with @bind-Value

Use `@bind-Value` to create two-way binding between the component and your data model:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" @bind-Value="SelectedRange"></SfDateRangePicker>

<p>Selected Range: @SelectedRange?.ToString("MMM dd, yyyy")</p>

@code {
    private DateTime? SelectedRange { get; set; }
}
```

**How it works:**
- Component updates `SelectedRange` when user selects dates
- Changing `SelectedRange` updates the component display
- Changes are synchronized automatically

### One-Way Binding with Value Property

For read-only scenarios or when you need more control:

```razor
<SfDateRangePicker TValue="DateTime?" Value="@InitialRange"></SfDateRangePicker>

@code {
    private DateTime? InitialRange { get; set; } = new DateTime(2024, 1, 1);
}
```

### Binding Multiple DateRangePickers

Store multiple range selections in a model:

```razor
@using Syncfusion.Blazor.Calendars

<h4>Q1 Period</h4>
<SfDateRangePicker TValue="DateTime?" @bind-Value="FilterDates.Q1"></SfDateRangePicker>

<h4>Q2 Period</h4>
<SfDateRangePicker TValue="DateTime?" @bind-Value="FilterDates.Q2"></SfDateRangePicker>

@code {
    private class DateFilters
    {
        public DateTime? Q1 { get; set; }
        public DateTime? Q2 { get; set; }
    }
    
    private DateFilters FilterDates = new();
}
```

## Working with DateTime? Type

### DateTime? (Nullable DateTime)

The most common type for DateRangePicker. Allows null values when no date is selected:

```razor
<SfDateRangePicker TValue="DateTime?" 
    @bind-Value="BookingDate"
    Placeholder="Select dates">
</SfDateRangePicker>

@code {
    private DateTime? BookingDate { get; set; }  // Null by default
    
    private void ProcessBooking()
    {
        if (BookingDate.HasValue)
        {
            // User selected a date
            var startDate = BookingDate.Value;
            Console.WriteLine($"Booking from {startDate}");
        }
        else
        {
            // No date selected
            Console.WriteLine("Please select a date");
        }
    }
}
```

### Accessing Start and End Dates

When using DateTime?, the value represents the entire range. Extract components as needed:

```razor
@if (DateRange.HasValue)
{
    <p>Range selected: @DateRange.Value.ToString("MMM dd, yyyy")</p>
    @* Note: DateTime? stores a single DateTime value representing the range *@
}

@code {
    private DateTime? DateRange { get; set; }
}
```

### Range Validation

Validate date ranges before processing:

```razor
<SfDateRangePicker TValue="DateTime?" @bind-Value="TravelDates"></SfDateRangePicker>

@if (IsValidRange())
{
    <p>✓ Valid range selected</p>
}
else if (TravelDates.HasValue)
{
    <p>✗ Invalid range - Please select valid dates</p>
}

@code {
    private DateTime? TravelDates { get; set; }
    
    private bool IsValidRange()
    {
        if (!TravelDates.HasValue)
            return false;
        
        // Add custom validation logic
        return TravelDates.Value > DateTime.Today;
    }
}
```

## Working with DateOnly Type

### DateOnly for .NET 6+

Use `DateOnly` type for .NET 6 and later when you don't need time components:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateOnly?" @bind-Value="TripDates"></SfDateRangePicker>

<p>Trip from: @TripDates?.ToString("MMMM dd, yyyy")</p>

@code {
    private DateOnly? TripDates { get; set; }
}
```

### DateOnly in Models

Use DateOnly in your data models:

```csharp
public class BookingModel
{
    public string BookingId { get; set; }
    public DateOnly CheckInDate { get; set; }
    public DateOnly CheckOutDate { get; set; }
    public int Nights => (CheckOutDate.DayNumber - CheckInDate.DayNumber);
}
```

Bind component to model properties:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateOnly?" @bind-Value="Booking.CheckInDate"></SfDateRangePicker>
<SfDateRangePicker TValue="DateOnly?" @bind-Value="Booking.CheckOutDate"></SfDateRangePicker>

<p>Duration: @Booking.Nights nights</p>

@code {
    private BookingModel Booking { get; set; } = new();
}
```

### Converting Between DateTime and DateOnly

When working with APIs that use DateTime:

```razor
@code {
    private DateOnly? SelectedDateOnly { get; set; }
    
    private DateTime? ConvertToDateTime()
    {
        return SelectedDateOnly?.ToDateTime(TimeOnly.MinValue);
    }
    
    private void ConvertFromDateTime(DateTime? dt)
    {
        SelectedDateOnly = dt.HasValue ? DateOnly.FromDateTime(dt.Value) : null;
    }
}
```

## Programmatic Range Updates

### Setting Date Range in Code

Update the component value from code:

```razor
@using Syncfusion.Blazor.Calendars

<button @onclick="SelectThisMonth">This Month</button>
<button @onclick="SelectLastMonth">Last Month</button>

<SfDateRangePicker TValue="DateTime?" @bind-Value="SelectedRange"></SfDateRangePicker>

@code {
    private DateTime? SelectedRange { get; set; }
    
    private void SelectThisMonth()
    {
        var today = DateTime.Today;
        var firstDay = new DateTime(today.Year, today.Month, 1);
        SelectedRange = firstDay;  // Component recognizes range start
    }
    
    private void SelectLastMonth()
    {
        var today = DateTime.Today;
        var firstDay = new DateTime(today.AddMonths(-1).Year, today.AddMonths(-1).Month, 1);
        SelectedRange = firstDay;
    }
}
```

### Range Templates

Create preset range buttons:

```razor
@using Syncfusion.Blazor.Calendars

<div style="margin-bottom: 20px;">
    <button class="preset-btn" @onclick="() => SelectRange('today')">Today</button>
    <button class="preset-btn" @onclick="() => SelectRange('week')">This Week</button>
    <button class="preset-btn" @onclick="() => SelectRange('month')">This Month</button>
    <button class="preset-btn" @onclick="() => SelectRange('quarter')">This Quarter</button>
</div>

<SfDateRangePicker TValue="DateTime?" @bind-Value="DateRange"></SfDateRangePicker>

@code {
    private DateTime? DateRange { get; set; }
    
    private void SelectRange(string preset)
    {
        var today = DateTime.Today;
        
        DateRange = preset switch
        {
            "today" => today,
            "week" => today.AddDays(-(int)today.DayOfWeek),
            "month" => new DateTime(today.Year, today.Month, 1),
            "quarter" => GetQuarterStart(today),
            _ => DateRange
        };
    }
    
    private DateTime GetQuarterStart(DateTime date)
    {
        int quarter = (date.Month - 1) / 3 + 1;
        int month = (quarter - 1) * 3 + 1;
        return new DateTime(date.Year, month, 1);
    }
}
```

## Clearing Selections

### Manual Clear

Allow users to clear their selection:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" @bind-Value="SelectedRange"></SfDateRangePicker>

@if (SelectedRange.HasValue)
{
    <button @onclick="ClearRange">Clear Selection</button>
}

@code {
    private DateTime? SelectedRange { get; set; }
    
    private void ClearRange()
    {
        SelectedRange = null;
    }
}
```

### Programmatic Clear

Clear ranges based on conditions:

```razor
@code {
    private DateTime? FilterRange { get; set; }
    
    private void ResetFilters()
    {
        FilterRange = null;
        // Reset other filters too
    }
}
```

### Clear Multiple Ranges

Clear all pickers in a form:

```razor
<SfDateRangePicker TValue="DateTime?" @bind-Value="StartRange"></SfDateRangePicker>
<SfDateRangePicker TValue="DateTime?" @bind-Value="EndRange"></SfDateRangePicker>

<button @onclick="ClearAll">Clear All Dates</button>

@code {
    private DateTime? StartRange { get; set; }
    private DateTime? EndRange { get; set; }
    
    private void ClearAll()
    {
        StartRange = null;
        EndRange = null;
    }
}
```

## Single Date vs Range Selection

### Understanding DateTime? Behavior

The DateRangePicker component with `TValue="DateTime?"` technically stores a single DateTime value, but the component's UI and interaction pattern are optimized for range selection:

```razor
@* This renders a dual-calendar interface for range selection *@
<SfDateRangePicker TValue="DateTime?" @bind-Value="DateRange"></SfDateRangePicker>

@code {
    private DateTime? DateRange { get; set; }
}
```

### For True Single Date Selection

Use the **DatePicker** component instead:

```razor
@using Syncfusion.Blazor.Calendars

@* Single date picker - only one calendar *@
<SfDatePicker TValue="DateTime?" @bind-Value="SingleDate"></SfDatePicker>

@code {
    private DateTime? SingleDate { get; set; }
}
```

**Key difference:**
- **DateRangePicker**: Dual calendars, range selection UI, optimized for date ranges
- **DatePicker**: Single calendar, single date UI, optimized for one date

Choose DateRangePicker when users need to select date ranges (bookings, filtering, reports). Choose DatePicker for single dates (birthdays, deadlines, preferences).

## Common Data Binding Patterns

### Form Integration

Bind to form model:

```razor
<EditForm Model="@SearchFilters" OnValidSubmit="@ApplyFilters">
    <DataAnnotationsValidator />
    
    <label>Date Range:</label>
    <SfDateRangePicker TValue="DateTime?" 
        @bind-Value="SearchFilters.DateRange"
        Min="@minSearchDate"
        Max="@maxSearchDate">
    </SfDateRangePicker>
    
    <button type="submit">Search</button>
</EditForm>

@code {
    private DateTime minSearchDate = DateTime.Today.AddMonths(-12);
    private DateTime maxSearchDate = DateTime.Today;
    
    private class SearchModel
    {
        public DateTime? DateRange { get; set; }
    }
    
    private SearchModel SearchFilters = new();
    
    private async Task ApplyFilters()
    {
        if (SearchFilters.DateRange.HasValue)
        {
            // Filter data by date range
        }
    }
}
```

### Dependent Date Ranges

Limit second range based on first selection:

```razor
@using Syncfusion.Blazor.Calendars

<label>Start Period:</label>
<SfDateRangePicker TValue="DateTime?" @bind-Value="StartPeriod"></SfDateRangePicker>

<label>End Period:</label>
<SfDateRangePicker TValue="DateTime?" 
    @bind-Value="EndPeriod"
    Min="@StartPeriod">
</SfDateRangePicker>

@code {
    private DateTime? StartPeriod { get; set; }
    private DateTime? EndPeriod { get; set; }
}
```

## Next Steps

- Read [events-and-interactions.md](events-and-interactions.md) for handling date changes
- Read [customization-and-styling.md](customization-and-styling.md) for appearance customization
- Read [data-and-globalization.md](data-and-globalization.md) for localization
