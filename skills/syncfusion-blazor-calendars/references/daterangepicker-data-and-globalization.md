# Data & Globalization

## Table of Contents
- [Data Binding Patterns](#data-binding-patterns)
- [DateOnly Type Support](#dateonly-type-support)
- [Globalization Configuration](#globalization-configuration)
- [Culture-Specific Formatting](#culture-specific-formatting)
- [RTL Support](#rtl-support)
- [Localization](#localization)

## Data Binding Patterns

### Model-Based Data Binding

Bind component to a data model:

```razor
@using Syncfusion.Blazor.Calendars

<EditForm Model="@BookingRequest">
    <div class="form-group">
        <label>Check-in and Check-out Dates:</label>
        <SfDateRangePicker TValue="DateTime?" 
            @bind-Value="BookingRequest.StayPeriod"
            Min="@minCheckInDate"
            Placeholder="Select dates">
        </SfDateRangePicker>
    </div>
    
    <button type="submit">Book Now</button>
</EditForm>

@code {
    private DateTime minCheckInDate = DateTime.Today;
    
    public class BookingRequest
    {
        public DateTime? StayPeriod { get; set; }
        public int Guests { get; set; }
        public string RoomType { get; set; }
    }
    
    private BookingRequest BookingRequest = new();
}
```

### Array/List Data Binding

When multiple range pickers are bound to a collection:

```razor
@using Syncfusion.Blazor.Calendars

@foreach (var quarter in Quarters)
{
    <div>
        <h5>@quarter.Name</h5>
        <SfDateRangePicker TValue="DateTime?" 
            @bind-Value="quarter.DateRange"
            Min="@quarter.StartDate"
            Max="@quarter.EndDate">
        </SfDateRangePicker>
    </div>
}

@code {
    private class Quarter
    {
        public string Name { get; set; }
        public DateTime StartDate { get; set; }
        public DateTime EndDate { get; set; }
        public DateTime? DateRange { get; set; }
    }
    
    private List<Quarter> Quarters = new()
    {
        new() { Name = "Q1", StartDate = new(2024, 1, 1), EndDate = new(2024, 3, 31) },
        new() { Name = "Q2", StartDate = new(2024, 4, 1), EndDate = new(2024, 6, 30) }
    };
}
```

### Async Data Loading

Load date constraints from API:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    @bind-Value="SelectedRange"
    Min="@MinAvailableDate"
    Max="@MaxAvailableDate"
    Placeholder="Select available dates">
</SfDateRangePicker>

@code {
    private DateTime? SelectedRange { get; set; }
    private DateTime MinAvailableDate { get; set; }
    private DateTime MaxAvailableDate { get; set; }
    
    protected override async Task OnInitializedAsync()
    {
        // Load availability from API
        var availability = await GetAvailabilityAsync();
        MinAvailableDate = availability.EarliestDate;
        MaxAvailableDate = availability.LatestDate;
    }
    
    private async Task<AvailabilityInfo> GetAvailabilityAsync()
    {
        // Call API
        return new AvailabilityInfo 
        { 
            EarliestDate = DateTime.Today,
            LatestDate = DateTime.Today.AddMonths(6)
        };
    }
    
    private class AvailabilityInfo
    {
        public DateTime EarliestDate { get; set; }
        public DateTime LatestDate { get; set; }
    }
}
```

## DateOnly Type Support

### Using DateOnly (.NET 6+)

Use the `DateOnly` type for date-only values without time components:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateOnly?" @bind-Value="TravelDates"></SfDateRangePicker>

<p>Trip Duration: @(TravelDates?.DayNumber) days</p>

@code {
    private DateOnly? TravelDates { get; set; }
}
```

### DateOnly in Entity Models

Use DateOnly in data models:

```csharp
public class HotelBooking
{
    public int BookingId { get; set; }
    public string GuestName { get; set; }
    public DateOnly CheckInDate { get; set; }
    public DateOnly CheckOutDate { get; set; }
    
    public int GetStayDuration()
    {
        return (CheckOutDate.DayNumber - CheckInDate.DayNumber);
    }
}
```

Bind component to model:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateOnly?" 
    @bind-Value="Booking.CheckInDate"
    Min="@DateOnly.FromDateTime(DateTime.Today)">
</SfDateRangePicker>

@code {
    private HotelBooking Booking = new();
}
```

### Converting Between DateTime and DateOnly

Convert between types when working with different APIs:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateOnly?" @bind-Value="SelectedDateOnly"></SfDateRangePicker>

@code {
    private DateOnly? SelectedDateOnly { get; set; }
    
    // Convert to DateTime for API
    private DateTime? ConvertToDateTime()
    {
        return SelectedDateOnly?.ToDateTime(TimeOnly.MinValue);
    }
    
    // Convert from DateTime (from API)
    private void ConvertFromDateTime(DateTime? dateTime)
    {
        SelectedDateOnly = dateTime.HasValue 
            ? DateOnly.FromDateTime(dateTime.Value) 
            : null;
    }
    
    // Send to API
    private async Task SaveToApi()
    {
        var dateTimeValue = ConvertToDateTime();
        await ApiService.SaveDateAsync(dateTimeValue);
    }
}
```

### DateOnly Validation

Validate DateOnly values:

```razor
@code {
    private DateOnly? CheckInDate { get; set; }
    private DateOnly? CheckOutDate { get; set; }
    
    private bool IsValidStay()
    {
        if (!CheckInDate.HasValue || !CheckOutDate.HasValue)
            return false;
        
        return CheckOutDate.Value > CheckInDate.Value;
    }
}
```

## Culture-Specific Formatting

### Date Format by Culture

Dates are automatically formatted based on culture:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    @bind-Value="DateRange">
</SfDateRangePicker>

<p>Selected: @DateRange?.ToString("D", System.Globalization.CultureInfo.GetCultureInfo(Culture))</p>

@code {
    private DateTime? DateRange { get; set; }
    
    // Output examples:
    // en-US: Monday, January 15, 2024
    // de-DE: Montag, 15. Januar 2024
    // fr-FR: lundi 15 janvier 2024
}
```

### Custom Format Patterns

Apply culture-specific formatting:

```razor
@code {
    private string GetFormattedDate(DateTime? date, string culture)
    {
        if (!date.HasValue)
            return "No date selected";
        
        var cultureInfo = System.Globalization.CultureInfo.GetCultureInfo(culture);
        
        return culture switch
        {
            "en-US" => date.Value.ToString("M/d/yyyy", cultureInfo),  // 1/15/2024
            "de-DE" => date.Value.ToString("dd.MM.yyyy", cultureInfo), // 15.01.2024
            "fr-FR" => date.Value.ToString("dd/MM/yyyy", cultureInfo), // 15/01/2024
            "ja-JP" => date.Value.ToString("yyyy/MM/dd", cultureInfo),  // 2024/01/15
            _ => date.Value.ToString("D", cultureInfo)
        };
    }
}
```

### First Day of Week by Culture

Different cultures have different week start days:

```razor
@code {
    private int GetFirstDayByLocale(string locale)
    {
        return locale switch
        {
            "en-US" or "en-GB" => 0,      // Sunday
            "de-DE" or "fr-FR" => 1,      // Monday
            "ar-AE" or "ar-SA" => 6,      // Saturday
            _ => 0                        // Default: Sunday
        };
    }
}
```

## RTL Support

### Enabling RTL

Enable right-to-left layout for Arabic, Hebrew, and similar languages:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    EnableRtl="true"
    Placeholder="اختر نطاقاً زمنياً">
</SfDateRangePicker>

```

### RTL Styling

Ensure CSS respects RTL layout:

```html
<html dir="rtl">
    <body>
        <!-- RTL content -->
    </body>
</html>
```

Or per component:

```razor
<div dir="rtl">
    <SfDateRangePicker TValue="DateTime?" 
        EnableRtl="true">
    </SfDateRangePicker>
</div>
```

### RTL Language Support

Common RTL languages:

```razor
@code {
    private Dictionary<string, string> RtlLanguages = new()
    {
        { "ar-AE", "العربية (الإمارات)" },      // Arabic
        { "ar-SA", "العربية (المملكة)" },      // Arabic (Saudi Arabia)
        { "he-IL", "עברית (ישראל)" },           // Hebrew
        { "ur-PK", "اردو (پاکستان)" },          // Urdu
        { "fa-IR", "فارسی (ایران)" }            // Persian
    };
}
```

## Localization

### Translation Best Practices

**Labels and Messages:**
```razor
@code {
    private Dictionary<string, Dictionary<string, string>> Translations = new()
    {
        {
            "en", new()
            {
                { "SelectRange", "Select date range" },
                { "CheckIn", "Check-in Date" },
                { "CheckOut", "Check-out Date" }
            }
        },
        {
            "de", new()
            {
                { "SelectRange", "Zeitspanne auswählen" },
                { "CheckIn", "Ankunftsdatum" },
                { "CheckOut", "Abreisedatum" }
            }
        }
    };
    
    private string GetTranslation(string key) 
        => Translations[CurrentLanguage][key];
}
```

## Next Steps

- Read [events-and-interactions.md](events-and-interactions.md) for handling date changes
- Read [accessibility.md](accessibility.md) for keyboard navigation support
- Read [customization-and-styling.md](customization-and-styling.md) for visual customization
