# Date Range and Constraints in Blazor DatePicker

This guide covers restricting date selection using min/max dates, disabling specific dates, and highlighting special dates.

## Min/Max Date Restrictions

Limit the date range users can select by setting minimum and maximum dates.

### Basic Min/Max

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"
              Min="@minDate"
              Max="@maxDate">
</SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
    DateTime? minDate = new DateTime(2026, 1, 1);
    DateTime? maxDate = new DateTime(2026, 12, 31);
}
```

- User can only select dates between January 1 and December 31, 2026
- Dates outside range appear grayed out/disabled in calendar

### Today or Future Dates Only

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"
              Min="@minBookingDate">
</SfDatePicker>

@code {
    DateTime? selectedDate;
    private DateTime minBookingDate = DateTime.Now.Date;
}
```

Users cannot select past dates. Perfect for appointment booking, event registration.

### Past Dates Only

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"
              Max="@maxHistoricalDate">
</SfDatePicker>

@code {
    DateTime? selectedDate;
    private DateTime maxHistoricalDate = DateTime.Now.Date;
}
```

Users can only select dates in the past. Useful for birth date, historical data entry.

### Age Restriction Example

```razor
<SfDatePicker TValue="DateTime?" @bind-Value="@birthDate"
              Max="@maxBirthDate"
              Placeholder="Select birth date (must be 18+)">
</SfDatePicker>

@code {
    DateTime? birthDate;

    // Users must be at least 18 years old
    DateTime? maxBirthDate => DateTime.Now.AddYears(-18).Date;
}
```

## Disabled Specific Dates

Prevent selection of specific dates while allowing a range.

### Using DisabledDates

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"
              DisabledDates="@disabledDates">
</SfDatePicker>

@code {
    DateTime? selectedDate;
    
    // Disable weekends
    int[] disabledDates = { 6, 0 }; // 6=Saturday, 0=Sunday (day of week)
}
```

### Disable Weekends

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"
              DisabledDates="@disabledDates">
</SfDatePicker>

@code {
    DateTime? selectedDate;
    
    int[] disabledDates = { 0, 6 }; // Sunday=0, Saturday=6
}
```

Users can only pick Monday-Friday (business days).

### Disable Public Holidays

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"
              DisabledDates="@holidays">
</SfDatePicker>

@code {
    DateTime? selectedDate;
    
    int[] holidays = new int[]
    {
        // Example: disable 25th of each month (fixed date)
        25
    };
}
```

## Special Dates - Highlighting

Highlight specific dates without disabling them, drawing attention to special events.

### Using SpecialDates

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"
              SpecialDates="@specialDates">
</SfDatePicker>

@code {
    DateTime? selectedDate;
    
    // Highlight specific dates with custom styling
    public List<SpecialDate> specialDates = new()
    {
        new SpecialDate { Date = new DateTime(2026, 3, 18), CssClass = "birthday" },
        new SpecialDate { Date = new DateTime(2026, 12, 25), CssClass = "holiday" }
    };
}
```

### Highlight Birthdays

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"
              SpecialDates="@birthdayDates">
</SfDatePicker>

<style>
    .birthday {
        background-color: #fff3cd;
        font-weight: bold;
    }
</style>

@code {
    DateTime? selectedDate;
    
    List<SpecialDate> birthdayDates = new()
    {
        new SpecialDate { Date = new DateTime(2026, 1, 15) },
        new SpecialDate { Date = new DateTime(2026, 3, 8) },
        new SpecialDate { Date = new DateTime(2026, 7, 22) }
    };
}
```

### Highlight Available Appointments

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"
              SpecialDates="@availableAppointmentDates">
</SfDatePicker>

<style>
    .available {
        background-color: #d4edda;
        border-radius: 50%;
    }
</style>

@code {
    DateTime? selectedDate;
    
    // Only these dates have available appointments
    List<SpecialDate> availableAppointmentDates = new()
    {
        new SpecialDate { Date = new DateTime(2026, 3, 20), CssClass = "available" },
        new SpecialDate { Date = new DateTime(2026, 3, 22), CssClass = "available" },
        new SpecialDate { Date = new DateTime(2026, 3, 25), CssClass = "available" }
    };
}
```

## Combined Constraints

Apply multiple restrictions together.

### Min/Max + Disabled Weekends

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"
              Min="@minDate"
              Max="@maxDate"
              DisabledDates="@weekends">
</SfDatePicker>

@code {
    DateTime? selectedDate;
    DateTime? minDate = new DateTime(2026, 3, 1);
    DateTime? maxDate = new DateTime(2026, 3, 31);
    int[] weekends = { 0, 6 }; // Sunday, Saturday
}
```

Users select only business days in March 2026.

### Business Days + Public Holidays

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"
              Min="@minWorkDate"
              DisabledDates="@disabledDates">
</SfDatePicker>

@code {
    DateTime? selectedDate;
    private DateTime minWorkDate = DateTime.Now.Date;
    
    int[] disabledDates = new int[]
    {
        0, 6, // Weekends
        25,   // 25th of month (fixed holiday)
        1     // 1st of month (another fixed holiday)
    };
}
```

## Dynamic Constraints

Update constraints based on user interaction or application state.

### Cascading Date Ranges

```razor
<div>
    <label>Start Date:</label>
    <SfDatePicker TValue="DateTime?" @bind-Value="@startDate" Min="@minStartDate"></SfDatePicker>
</div>

<div>
    <label>End Date:</label>
    <SfDatePicker TValue="DateTime?" @bind-Value="@endDate" Min="@startDate"></SfDatePicker>
</div>

@code {
    DateTime? startDate;
    DateTime? endDate;
    private DateTime minStartDate = DateTime.Now.Date;
}
```

End date must be after or equal to start date. Min constraint updates dynamically.

### Available Dates Based on Availability

```razor
<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate"
              SpecialDates="@availableDates">
</SfDatePicker>

<button @onclick="FetchAvailableDates">Load Available Dates</button>

@code {
    DateTime? selectedDate;
    List<SpecialDate> availableDates = new();

    async Task FetchAvailableDates()
    {
        // Simulate API call to fetch available appointment dates
        var result = await GetAvailableDatesFromAPI();
        availableDates = result.Select(d => new SpecialDate { Date = d }).ToList();
    }

    async Task<List<DateTime>> GetAvailableDatesFromAPI()
    {
        // API call here
        await Task.Delay(500);
        return new List<DateTime>
        {
            DateTime.Now.AddDays(1),
            DateTime.Now.AddDays(2),
            DateTime.Now.AddDays(3)
        };
    }
}
```

## Validation Patterns

### Manual Range Validation

```razor
<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate"
              Min="@minDate" Max="@maxDate">
</SfDatePicker>

<span class="error" style="color:red; display:@(isInvalid ? "block" : "none")">
    Date must be between @minDate?.ToShortDateString() and @maxDate?.ToShortDateString()
</span>

@code {
    DateTime? selectedDate;
    DateTime? minDate = new DateTime(2026, 3, 1);
    DateTime? maxDate = new DateTime(2026, 3, 31);

    bool isInvalid => selectedDate.HasValue && (selectedDate < minDate || selectedDate > maxDate);
}
```

### With EditForm

```razor
<EditForm Model="@bookingData" OnValidSubmit="@SubmitBooking">
    <DataAnnotationsValidator />
    
    <SfDatePicker TValue="DateTime?" @bind-Value="@bookingData.AppointmentDate"
                  Min="@minAppointmentDate">
    </SfDatePicker>
    <ValidationMessage For="@(() => bookingData.AppointmentDate)" />
    
    <button type="submit">Book</button>
</EditForm>

@code {
    BookingModel bookingData = new();
    private DateTime minAppointmentDate = DateTime.Now.Date;

    void SubmitBooking()
    {
        // Process booking
    }

    class BookingModel
    {
        [Required]
        [Range(typeof(DateTime?), "2026-03-01", "2026-12-31", 
            ErrorMessage = "Date must be in 2026")]
        public DateTime? AppointmentDate { get; set; }
    }
}
```

## Troubleshooting

### Issue: Min/Max not restricting dates
**Solution:** Ensure dates are DateTime objects, not strings. Use `DateTime? minDate = new DateTime(...)`

### Issue: Disabled dates not appearing grayed out
**Solution:** Verify DatePicker theme CSS is loaded

### Issue: Special dates CSS not applying
**Solution:** Check CSS class name matches, ensure styles are defined before DatePicker component

### Issue: Dynamic constraints not updating
**Solution:** Call `StateHasChanged()` after updating constraint properties

## Next Steps

- **Events:** Handle [date change and validation events](events.md)
- **Advanced:** Configure [special calendars and globalization](advanced-features.md)
- **Styling:** Customize [appearance and themes](styling-and-appearance.md)
