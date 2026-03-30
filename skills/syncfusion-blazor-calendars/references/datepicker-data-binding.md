# Data Binding in Blazor DatePicker

This guide explains how to bind DatePicker values to component state using one-way and two-way binding techniques.

## One-Way Binding

One-way binding updates the DatePicker display when the property changes, but user input doesn't automatically update the property.

### Basic One-Way Binding

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>

<p>Selected: @selectedDate?.ToString("dddd, dd MMMM yyyy")</p>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
}
```

User can select a date in the picker, but `selectedDate` remains unchanged. Use the `ValueChange` event to update programmatically.

### One-Way with Event Handling

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" ValueChange="@OnDateChange"></SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);

    void OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        selectedDate = args.Value;
        StateHasChanged(); // Optional: explicit re-render
    }
}
```

**Result:** User selects a date → `ValueChange` fires → property updates.

## Two-Way Data Binding

Two-way binding (`@bind-Value`) automatically synchronizes the DatePicker value with component state bidirectionally.

### Basic Two-Way Binding

```razor
<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate"></SfDatePicker>

<p>Selected: @selectedDate</p>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
}
```

- User selects date → `selectedDate` updates automatically
- Code changes `selectedDate` → DatePicker display updates

### With Form Data

```razor
<div>
    <label>Birth Date:</label>
    <SfDatePicker TValue="DateTime?" @bind-Value="@formData.BirthDate"></SfDatePicker>
</div>

<button @onclick="SubmitForm">Submit</button>

@code {
    FormModel formData = new();

    void SubmitForm()
    {
        Console.WriteLine($"Birth Date: {formData.BirthDate}");
    }

    class FormModel
    {
        public DateTime? BirthDate { get; set; }
    }
}
```

**Benefit:** Form data automatically reflects DatePicker changes without manual event handling.

### With Nullable DateTime

```razor
<SfDatePicker TValue="DateTime?" @bind-Value="@appointmentDate"></SfDatePicker>

@code {
    DateTime? appointmentDate; // null initially
}
```

`DateTime?` (nullable) allows the DatePicker to represent "no date selected." Useful for optional date fields.

## Dynamic Value Updates

### Programmatic Updates

```razor
<button @onclick="SetToday">Set Today</button>
<button @onclick="ClearDate">Clear</button>

<SfDatePicker @ref="datePicker" TValue="DateTime?" @bind-Value="@selectedDate"></SfDatePicker>

@code {
    SfDatePicker<DateTime?> datePicker;
    DateTime? selectedDate;

    void SetToday()
    {
        selectedDate = DateTime.Now.Date;
    }

    void ClearDate()
    {
        selectedDate = null;
    }
}
```

**Result:** Clicking buttons updates `selectedDate`, and DatePicker display refreshes automatically.

### Conditional Binding

```razor
<SfDatePicker TValue="DateTime?" 
              Value="@(useStartDate ? startDate : endDate)"
              ValueChange="@((ChangedEventArgs<DateTime?> args) => OnDateChange(args, useStartDate))">
</SfDatePicker>

@code {
    DateTime? startDate = new DateTime(2026, 1, 1);
    DateTime? endDate = new DateTime(2026, 12, 31);
    bool useStartDate = true;

    void OnDateChange(ChangedEventArgs<DateTime?> args, bool isStart)
    {
        if (isStart)
            startDate = args.Value;
        else
            endDate = args.Value;
    }
}
```

### Cascading Binding

```razor
<div>
    <label>Start Date:</label>
    <SfDatePicker TValue="DateTime?" @bind-Value="@startDate"></SfDatePicker>
</div>

<div>
    <label>End Date:</label>
    <SfDatePicker TValue="DateTime?" @bind-Value="@endDate" Min="@startDate"></SfDatePicker>
</div>

@code {
    DateTime? startDate = new DateTime(2026, 3, 1);
    DateTime? endDate = new DateTime(2026, 3, 31);
}
```

When `startDate` changes, the `Min` date for `endDate` updates automatically, preventing invalid ranges.

## Null Value Handling

### Handling Null Values

```razor
<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate" AllowNull="true"></SfDatePicker>

<p>
    @if (selectedDate.HasValue)
    {
        <span>Selected: @selectedDate.Value.ToString("dddd, dd MMMM yyyy")</span>
    }
    else
    {
        <span>No date selected</span>
    }
</p>

@code {
    DateTime? selectedDate; // null initially
}
```

**Use `AllowNull`** to allow users to clear the DatePicker back to null.

### Default Value Handling

```razor
<SfDatePicker TValue="DateTime?" 
              Value="@(selectedDate ?? DateTime.Now.Date)">
</SfDatePicker>

@code {
    DateTime? selectedDate; // null
}
```

Display today's date by default, but store null internally.

## DateTime vs DateTime?

### DateTime (Non-Nullable)

```razor
<SfDatePicker TValue="DateTime" Value="@selectedDate"></SfDatePicker>

@code {
    DateTime selectedDate = new DateTime(2026, 3, 18);
}
```

**Disadvantage:** Always has a value; can't represent "no date selected"  
**Use when:** Date is mandatory, always required

### DateTime? (Nullable)

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>

@code {
    DateTime? selectedDate; // null initially
}
```

**Advantage:** Can represent null (no selection)  
**Use when:** Date is optional or can be cleared

## Reactive Updates

### Using StateHasChanged

```razor
<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate"></SfDatePicker>

<button @onclick="async () => await RefreshData()">Refresh Data</button>

@code {
    DateTime? selectedDate;

    async Task RefreshData()
    {
        // Async operation
        await Task.Delay(1000);
        selectedDate = DateTime.Now.Date;
        StateHasChanged(); // Ensure UI updates
    }
}
```

### With Form Validation

```razor
<EditForm Model="@formData" OnValidSubmit="@HandleValidSubmit">
    <SfDatePicker TValue="DateTime?" @bind-Value="@formData.EventDate"></SfDatePicker>
    <button type="submit">Submit</button>
</EditForm>

@code {
    FormModel formData = new();

    void HandleValidSubmit()
    {
        Console.WriteLine($"Event Date: {formData.EventDate}");
    }

    class FormModel
    {
        public DateTime? EventDate { get; set; }
    }
}
```

## Common Patterns

### Pattern 1: Clear and Reset
```razor
<button @onclick="() => selectedDate = null">Clear Date</button>
```

### Pattern 2: Copy Value Between Pickers
```razor
<SfDatePicker TValue="DateTime?" @bind-Value="@date1"></SfDatePicker>
<button @onclick="() => date2 = date1">Copy</button>
<SfDatePicker TValue="DateTime?" @bind-Value="@date2"></SfDatePicker>

@code {
    DateTime? date1;
    DateTime? date2;
}
```

### Pattern 3: Display Formatted Date
```razor
<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate"></SfDatePicker>
<span>@selectedDate?.ToString("MMMM dd, yyyy")</span>

@code {
    DateTime? selectedDate;
}
```

## Troubleshooting

### Issue: Value not updating
**Solution:** Use `@bind-Value` instead of `Value` for two-way binding

### Issue: Null reference exception
**Solution:** Use `DateTime?` instead of `DateTime` for optional dates

### Issue: Display not refreshing after programmatic change
**Solution:** Call `StateHasChanged()` after updating value in non-event code

## Next Steps

- **Formatting:** Configure [display and input formats](date-formats-and-input.md)
- **Events:** Handle [ValueChange and other events](events.md)
- **Constraints:** Set [min/max dates](date-range-and-constraints.md)
