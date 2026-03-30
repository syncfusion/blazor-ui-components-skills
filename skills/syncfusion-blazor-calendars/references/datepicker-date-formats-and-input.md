# Date Formats and Input in Blazor DatePicker

## Table of Contents
- [Display Format](#display-format)
- [Input Format](#input-format)
- [Format Patterns](#format-patterns)
- [Placeholder Text](#placeholder-text)
- [Format Validation](#format-validation)
- [Custom Examples](#custom-examples)

## Display Format

The `Format` property controls how the selected date appears in the input field.

### Basic Display Format

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" Format="dd/MM/yyyy"></SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
}
```

**Result:** Date displays as "18/03/2026"

### Common Format Patterns

| Format | Example | Description |
|--------|---------|-------------|
| `dd/MM/yyyy` | 18/03/2026 | Day/Month/Year with zero-padding |
| `MM/dd/yyyy` | 03/18/2026 | Month/Day/Year (US format) |
| `yyyy-MM-dd` | 2026-03-18 | ISO 8601 format |
| `dd MMM yyyy` | 18 Mar 2026 | Full month name |
| `dd MMMM yyyy` | 18 March 2026 | Full month name spelled out |
| `dddd, dd MMMM yyyy` | Wednesday, 18 March 2026 | Full day and month names |
| `MM/dd/yy` | 03/18/26 | Two-digit year |
| `d/M/yyyy` | 18/3/2026 | No zero-padding |

### Regional Formats

```razor
<!-- US Format -->
<SfDatePicker TValue="DateTime?" Value="@selectedDate" Format="MM/dd/yyyy"></SfDatePicker>

<!-- European Format -->
<SfDatePicker TValue="DateTime?" Value="@selectedDate" Format="dd/MM/yyyy"></SfDatePicker>

<!-- ISO Format -->
<SfDatePicker TValue="DateTime?" Value="@selectedDate" Format="yyyy-MM-dd"></SfDatePicker>

<!-- Asian Format -->
<SfDatePicker TValue="DateTime?" Value="@selectedDate" Format="yyyy年MM月dd日"></SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
}
```

## Input Format

The `Format` property also controls the **input format**—the pattern users must follow when typing dates directly.

### Strict Input Format

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" Format="dd/MM/yyyy"></SfDatePicker>
```

- User can type: `18/03/2026` → Accepted
- User can type: `3/18/2026` → May not be accepted without zero-padding
- User can type: `March 18, 2026` → Rejected

### With Placeholder

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              Format="dd/MM/yyyy" 
              Placeholder="DD/MM/YYYY">
</SfDatePicker>
```

Placeholder shows the expected input format to guide users.

### Flexible Input Parsing

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" Format="dd/MM/yyyy"></SfDatePicker>

@code {
    DateTime? selectedDate;
}
```

The DatePicker attempts to parse various input formats:
- `18/03/2026` ✓
- `18-03-2026` ✓ (dash separators sometimes work)
- `18/3/2026` ✓ (single-digit month)
- `18/03/26` ✓ (two-digit year)

## Format Patterns

### Standard Date Tokens

| Token | Meaning | Example |
|-------|---------|---------|
| `d` | Day (1-31, no padding) | 5 |
| `dd` | Day (01-31, zero-padded) | 05 |
| `ddd` | Day name (3 letters) | Wed |
| `dddd` | Full day name | Wednesday |
| `M` | Month (1-12, no padding) | 3 |
| `MM` | Month (01-12, zero-padded) | 03 |
| `MMM` | Month name (3 letters) | Mar |
| `MMMM` | Full month name | March |
| `yy` | Year (2 digits) | 26 |
| `yyyy` | Year (4 digits) | 2026 |

### Building Custom Formats

```razor
<!-- Show day of week, full date -->
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              Format="dddd, dd MMMM yyyy">
</SfDatePicker>
<!-- Result: Wednesday, 18 March 2026 -->

<!-- Month name with day and year -->
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              Format="MMMM dd, yyyy">
</SfDatePicker>
<!-- Result: March 18, 2026 -->

<!-- Abbreviated format -->
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              Format="ddd, dd MMM yy">
</SfDatePicker>
<!-- Result: Wed, 18 Mar 26 -->

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
}
```

## Placeholder Text

The `Placeholder` property provides input guidance without changing the display format.

### Basic Placeholder

```razor
<SfDatePicker TValue="DateTime?" Placeholder="Select a date"></SfDatePicker>
```

### Format-Aligned Placeholder

```razor
<SfDatePicker TValue="DateTime?" 
              Format="dd/MM/yyyy" 
              Placeholder="DD/MM/YYYY">
</SfDatePicker>

<!-- Versus -->

<SfDatePicker TValue="DateTime?" 
              Format="MM/dd/yyyy" 
              Placeholder="MM/DD/YYYY">
</SfDatePicker>
```

**Best Practice:** Match placeholder to format to guide users clearly.

### Descriptive Placeholder

```razor
<SfDatePicker TValue="DateTime?" Placeholder="Enter your birth date (DD/MM/YYYY)"></SfDatePicker>
```

## Format Validation

### Handling Invalid Input

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" Format="dd/MM/yyyy"></SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
}
```

**User enters invalid date:** `32/02/2026`
- DatePicker typically **rejects** the input or clears the field
- The `Value` remains unchanged (previous valid date)

### With Error Handling

```razor
<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate" 
              Format="dd/MM/yyyy"
              ValueChange="@OnDateChange">
</SfDatePicker>

<div class="error" style="display:@(hasError ? "block" : "none")">
    Invalid date format. Use DD/MM/YYYY.
</div>

@code {
    DateTime? selectedDate;
    bool hasError = false;

    void OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        if (args.Value.HasValue)
        {
            selectedDate = args.Value;
            hasError = false;
        }
        else
        {
            hasError = true;
        }
    }
}
```

### Using EditForm Validation

```razor
<EditForm Model="@formData">
    <DataAnnotationsValidator />
    
    <SfDatePicker TValue="DateTime?" @bind-Value="@formData.EventDate" Format="dd/MM/yyyy"></SfDatePicker>
    <ValidationMessage For="@(() => formData.EventDate)" />
    
    <button type="submit">Submit</button>
</EditForm>

@code {
    FormModel formData = new();

    class FormModel
    {
        [Required(ErrorMessage = "Event date is required")]
        public DateTime? EventDate { get; set; }
    }
}
```

## Custom Examples

### Time Zone Considerations

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" Format="dd/MM/yyyy HH:mm"></SfDatePicker>

@code {
    // Consider using DateTimeOffset for time zone awareness
    DateTime? selectedDate = new DateTime(2026, 3, 18, 14, 30, 0);
}
```

**Note:** Standard DatePicker shows date only, not time. For date+time, use DateTimePicker.

### Relative Date Display

```razor
<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate" Format="dd/MM/yyyy"></SfDatePicker>

<span>@GetRelativeDate(selectedDate)</span>

@code {
    DateTime? selectedDate;

    string GetRelativeDate(DateTime? date)
    {
        if (!date.HasValue) return "No date selected";
        
        var days = (date.Value.Date - DateTime.Now.Date).Days;
        return days == 0 ? "Today" 
             : days == 1 ? "Tomorrow"
             : days == -1 ? "Yesterday"
             : $"{days} days away";
    }
}
```

### Multiple Format Support

```razor
<div>
    <label>Date Format:</label>
    <select @onchange="@((ChangeEventArgs e) => selectedFormat = e.Value.ToString())">
        <option value="dd/MM/yyyy">DD/MM/YYYY</option>
        <option value="MM/dd/yyyy">MM/DD/YYYY</option>
        <option value="yyyy-MM-dd">YYYY-MM-DD</option>
    </select>
</div>

<SfDatePicker TValue="DateTime?" Value="@selectedDate" Format="@selectedFormat"></SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
    string selectedFormat = "dd/MM/yyyy";
}
```

## Troubleshooting

### Issue: Format not applying
**Solution:** Verify `Format` property is set with correct token case (MM vs mm)

### Issue: User input not matching format
**Solution:** Add `Placeholder` showing expected format, or use mask support

### Issue: Invalid dates accepted
**Solution:** DatePicker may parse ambiguous formats. Use strict validation with EditForm

### Issue: Date displays incorrectly
**Solution:** Check system culture settings and consider explicit CultureInfo setup

## Next Steps

- **Data Binding:** Learn [value updates and state management](data-binding.md)
- **Constraints:** Set [min/max dates and disabled dates](date-range-and-constraints.md)
- **Events:** Handle [format-related events](events.md)
