# Masking & Input Validation

Master input masking and validation patterns for the DateTimePicker component.

## Table of Contents
- [Input Mask Patterns](#input-mask-patterns)
- [Placeholder Text](#placeholder-text)
- [Strict Mode](#strict-mode)
- [Input Validation](#input-validation)
- [Validation Edge Cases](#validation-edge-cases)
- [Advanced Validation Patterns](#advanced-validation-patterns)

## Input Mask Patterns

### Common Mask Patterns

Input masks control the format users must follow when typing dates and times:

```blazor
@page "/datetimepicker-masks"
@using Syncfusion.Blazor.Calendars

<div>
    <h3>Common Mask Patterns</h3>
    
    <div>
        <label>MM/DD/YYYY HH:MM:</label>
        <SfDateTimePicker TValue="DateTime?" 
                          Mask="00/00/0000 00:00">
        </SfDateTimePicker>
    </div>
    
    <div style="margin-top: 10px;">
        <label>DD-MM-YYYY HH:MM:</label>
        <SfDateTimePicker TValue="DateTime?" 
                          Mask="00-00-0000 00:00">
        </SfDateTimePicker>
    </div>
    
    <div style="margin-top: 10px;">
        <label>YYYY-MM-DD HH:MM:</label>
        <SfDateTimePicker TValue="DateTime?" 
                          Mask="0000-00-00 00:00">
        </SfDateTimePicker>
    </div>
</div>
```

### Mask Symbols

| Symbol | Meaning | Example |
|--------|---------|---------|
| `0` | Digit placeholder (0-9 required) | `00` → "05" |
| `9` | Optional digit (0-9) | `99` → "5" or "05" |
| `#` | Optional digit | Same as `9` |
| `L` | Letter placeholder (a-z, A-Z required) | `LL` → "AM" |
| `?` | Optional letter | Same as optional char |
| `&` | Character placeholder (required) | `&&&` → "abc" |
| `;` | Separator/literal | `;` → stays as-is |

### Mask Examples

**US Date Format with Time**
```blazor
<SfDateTimePicker TValue="DateTime?" 
                  Mask="00/00/0000 00:00 LL">
                  <!-- 03/19/2024 02:30 PM -->
</SfDateTimePicker>
```

**European Date Format**
```blazor
<SfDateTimePicker TValue="DateTime?" 
                  Mask="00.00.0000 00:00">
                  <!-- 19.03.2024 14:30 -->
</SfDateTimePicker>
```

**ISO 8601 Format**
```blazor
<SfDateTimePicker TValue="DateTime?" 
                  Mask="0000-00-00 00:00:00">
                  <!-- 2024-03-19 14:30:00 -->
</SfDateTimePicker>
```

## Placeholder Text

### Default Placeholder

```blazor
<SfDateTimePicker TValue="DateTime?" 
                  Placeholder="Select date and time">
</SfDateTimePicker>
```

### Placeholder with Mask Reference

When using a mask, the placeholder helps guide users on the expected format:

```blazor
<SfDateTimePicker TValue="DateTime?" 
                  Mask="00/00/0000 00:00"
                  Placeholder="MM/DD/YYYY HH:MM">
</SfDateTimePicker>
```

### Custom Placeholder Examples

```blazor
<!-- Contextual placeholder -->
<SfDateTimePicker TValue="DateTime?" 
                  Mask="00/00/0000 00:00"
                  Placeholder="Enter appointment date (MM/DD/YYYY HH:MM)">
</SfDateTimePicker>

<!-- Multiple variants -->
<SfDateTimePicker TValue="DateTime?" 
                  Mask="0000-00-00 00:00"
                  Placeholder="2024-03-19 14:30">
</SfDateTimePicker>
```

## Strict Mode

### Enable Strict Mode

Strict mode enforces validation rules and prevents invalid input:

```blazor
@page "/datetimepicker-strict"
@using Syncfusion.Blazor.Calendars

<SfDateTimePicker TValue="DateTime?" 
                  StrictMode="true"
                  Mask="00/00/0000 00:00">
</SfDateTimePicker>
```

### Strict Mode Behavior

With `StrictMode="true"`:
- ✓ Only valid dates are accepted
- ✓ Invalid values are rejected
- ✓ User cannot submit invalid dates
- ✓ Component prevents form submission with invalid input

### Strict vs Non-Strict Comparison

```blazor
@page "/datetimepicker-strict-compare"
@using Syncfusion.Blazor.Calendars

<div style="display: flex; gap: 20px;">
    <div>
        <h4>Strict Mode (Enforced)</h4>
        <SfDateTimePicker TValue="DateTime?" 
                          StrictMode="true"
                          Mask="00/00/0000 00:00"
                          Placeholder="MM/DD/YYYY HH:MM">
        </SfDateTimePicker>
        <p style="font-size: 12px; color: gray;">Only valid dates accepted</p>
    </div>
    
    <div>
        <h4>Non-Strict Mode (Permissive)</h4>
        <SfDateTimePicker TValue="DateTime?" 
                          StrictMode="false"
                          Mask="00/00/0000 00:00"
                          Placeholder="MM/DD/YYYY HH:MM">
        </SfDateTimePicker>
        <p style="font-size: 12px; color: gray;">Invalid dates can be entered</p>
    </div>
</div>
```

## Input Validation

### Basic Validation on Value Change

```blazor
@page "/datetimepicker-validation"
@using Syncfusion.Blazor.Calendars

<div>
    <SfDateTimePicker TValue="DateTime?" >
        <DateTimePickerEvents TValue="DateTime?" ValueChange="@OnValidateInput"></DateTimePickerEvents>
    </SfDateTimePicker>
    
    @if (!string.IsNullOrEmpty(validationMessage))
    {
        <div style="color: @(isValid ? "green" : "red"); margin-top: 10px;">
            @validationMessage
        </div>
    }
</div>

@code {
    private string validationMessage = "";
    private bool isValid = true;
    
    private void OnValidateInput(ChangedEventArgs<DateTime?> args)
    {
        if (!args.Value.HasValue)
        {
            validationMessage = "Date is required";
            isValid = false;
            return;
        }
        
        var date = args.Value.Value;
        
        // Validate date is not in past
        if (date < DateTime.Now)
        {
            validationMessage = "⚠️ Cannot select past date";
            isValid = false;
            return;
        }
        
        // Validate business hours
        if (date.Hour < 9 || date.Hour > 17)
        {
            validationMessage = "⚠️ Only business hours (9 AM - 5 PM)";
            isValid = false;
            return;
        }
        
        validationMessage = "✓ Valid selection";
        isValid = true;
    }
}
```

### Validation with Min/Max Constraints

```blazor
@page "/datetimepicker-range-validation"
@using Syncfusion.Blazor.Calendars

<div>
    <SfDateTimePicker TValue="DateTime?" 
                      Min="@minDate"
                      Max="@maxDate">
        <DateTimePickerEvents TValue="DateTime?" ValueChange="@OnRangeValidation"></DateTimePickerEvents>
    </SfDateTimePicker>
    
    @if (!string.IsNullOrEmpty(message))
    {
        <p>@message</p>
    }
</div>

@code {
    private DateTime minDate = DateTime.Now;
    private DateTime maxDate = DateTime.Now.AddDays(30);
    private string message = "";
    
    private void OnRangeValidation(ChangedEventArgs<DateTime?> args)
    {
        if (!args.Value.HasValue)
        {
            message = "Date required";
            return;
        }
        
        var date = args.Value.Value;
        
        if (date < minDate || date > maxDate)
        {
            message = $"Date must be between {minDate:d} and {maxDate:d}";
        }
        else
        {
            message = "Valid date selected";
        }
    }
}
```

## Validation Edge Cases

### Case 1: Empty Input

```blazor
@page "/datetimepicker-empty"
@using Syncfusion.Blazor.Calendars

<div>
    <SfDateTimePicker TValue="DateTime?" 
                      AllowEdit="true">
        <DateTimePickerEvents TValue="DateTime?" ValueChange="@HandleEmpty"></DateTimePickerEvents>
    </SfDateTimePicker>
    <p>@statusMessage</p>
</div>

@code {
    private string statusMessage = "";
    private DateTime? selectedDate;
    
    private void HandleEmpty(ChangedEventArgs<DateTime?> args)
    {
        selectedDate = args.Value;
        
        if (selectedDate == null)
        {
            statusMessage = "Date cleared";
        }
        else
        {
            statusMessage = $"Date: {selectedDate.Value:g}";
        }
    }
}
```

### Case 2: Invalid Date String Entry

```blazor
@page "/datetimepicker-invalid-entry"
@using Syncfusion.Blazor.Calendars

<div>
    <SfDateTimePicker TValue="DateTime?" 
                      StrictMode="true"
                      Mask="00/00/0000"
                      Placeholder="MM/DD/YYYY">
    </SfDateTimePicker>
    
    <p style="font-size: 12px; color: gray;">
        Invalid entries (e.g., "13/32/2024") will be rejected in strict mode
    </p>
</div>
```

### Case 3: Ambiguous Dates

```blazor
@page "/datetimepicker-ambiguous"
@using Syncfusion.Blazor.Calendars

<div>
    <div>
        <label>US Format (MM/DD/YYYY):</label>
        <SfDateTimePicker TValue="DateTime?" 
                          Mask="00/00/0000"
                          Placeholder="05/03/2024 = May 3">
        </SfDateTimePicker>
    </div>
    
    <div style="margin-top: 10px;">
        <label>EU Format (DD/MM/YYYY):</label>
        <SfDateTimePicker TValue="DateTime?" 
                          Mask="00/00/0000"
                          Placeholder="03/05/2024 = May 3">
        </SfDateTimePicker>
    </div>
</div>
```

## Advanced Validation Patterns

### Pattern 1: Multi-Field Validation

```blazor
@page "/datetimepicker-multi-field"
@using Syncfusion.Blazor.Calendars

<div style="border: 1px solid #ddd; padding: 20px; border-radius: 4px;">
    <h4>Date Range Selection</h4>
    
    <div>
        <label>Start Date:</label>
        <SfDateTimePicker TValue="DateTime?" 
                          @bind-Value="startDate"
                          Max="@endDate">
            <DateTimePickerEvents TValue="DateTime?" ValueChange="@OnStartDateChanged"></DateTimePickerEvents>
        </SfDateTimePicker>
    </div>
    
    <div style="margin-top: 10px;">
        <label>End Date:</label>
        <SfDateTimePicker TValue="DateTime?" 
                          @bind-Value="endDate"
                          Min="@startDate">
            <DateTimePickerEvents TValue="DateTime?" ValueChange="@OnEndDateChanged"></DateTimePickerEvents>
        </SfDateTimePicker>
    </div>
    
    @if (validationStatus != "")
    {
        <p style="margin-top: 10px; color: @(validationStatus.Contains("✓") ? "green" : "red");">
            @validationStatus
        </p>
    }
</div>

@code {
    private DateTime? startDate;
    private DateTime? endDate;
    private string validationStatus = "";
    
    private void OnStartDateChanged(ChangedEventArgs<DateTime?> args)
    {
        startDate = args.Value;
        ValidateDateRange();
    }
    
    private void OnEndDateChanged(ChangedEventArgs<DateTime?> args)
    {
        endDate = args.Value;
        ValidateDateRange();
    }
    
    private void ValidateDateRange()
    {
        if (!startDate.HasValue || !endDate.HasValue)
        {
            validationStatus = "Both dates required";
            return;
        }
        
        if (startDate > endDate)
        {
            validationStatus = "Start date must be before end date";
            return;
        }
        
        var daysDiff = (endDate - startDate).Value.Days;
        
        if (daysDiff > 365)
        {
            validationStatus = "Range cannot exceed 1 year";
            return;
        }
        
        validationStatus = $"✓ Valid range: {daysDiff} days";
    }
}
```

### Pattern 2: Async Validation

```blazor
@page "/datetimepicker-async-validation"
@using Syncfusion.Blazor.Calendars

<div>
    <SfDateTimePicker TValue="DateTime?" >
        <DateTimePickerEvents TValue="DateTime?" ValueChange="@OnAsyncValidate"></DateTimePickerEvents>
    </SfDateTimePicker>
    
    @if (isValidating)
    {
        <p style="color: blue;">Validating...</p>
    }
    else if (!string.IsNullOrEmpty(validationMessage))
    {
        <p style="color: @(isValid ? "green" : "red");">@validationMessage</p>
    }
</div>

@code {
    private bool isValidating = false;
    private bool isValid = false;
    private string validationMessage = "";
    
    private async Task OnAsyncValidate(ChangedEventArgs<DateTime?> args)
    {
        if (!args.Value.HasValue)
        {
            validationMessage = "";
            return;
        }
        
        isValidating = true;
        
        // Simulate API call to check availability
        await Task.Delay(800);
        
        var date = args.Value.Value;
        
        // Simulate availability check
        isValid = date.Minute % 5 == 0; // Valid if minute is divisible by 5
        
        if (isValid)
        {
            validationMessage = $"✓ Slot available on {date:g}";
        }
        else
        {
            validationMessage = $"✗ Slot unavailable on {date:g}";
        }
        
        isValidating = false;
    }
}
```

### Pattern 3: Custom Validation Rules

```blazor
@page "/datetimepicker-custom-rules"
@using Syncfusion.Blazor.Calendars

<div>
    <SfDateTimePicker TValue="DateTime?" >
        <DateTimePickerEvents TValue="DateTime?" ValueChange="@ValidateCustomRules"></DateTimePickerEvents>
    </SfDateTimePicker>
    
    <div style="margin-top: 20px;">
        @foreach (var rule in validationRules)
        {
            <p style="color: @(rule.Passed ? "green" : "red"); font-size: 14px;">
                @(rule.Passed ? "✓" : "✗") @rule.Description
            </p>
        }
    </div>
</div>

@code {
    private List<ValidationRule> validationRules = new();
    
    private void ValidateCustomRules(ChangedEventArgs<DateTime?> args)
    {
        validationRules.Clear();
        
        if (!args.Value.HasValue)
        {
            return;
        }
        
        var date = args.Value.Value;
        
        // Rule 1: Not in past
        validationRules.Add(new ValidationRule
        {
            Description = "Date is not in the past",
            Passed = date >= DateTime.Now
        });
        
        // Rule 2: Business hours (9 AM - 5 PM)
        validationRules.Add(new ValidationRule
        {
            Description = "During business hours (9 AM - 5 PM)",
            Passed = date.Hour >= 9 && date.Hour < 17
        });
        
        // Rule 3: Not a weekend
        validationRules.Add(new ValidationRule
        {
            Description = "Not on weekend",
            Passed = date.DayOfWeek != DayOfWeek.Saturday && 
                     date.DayOfWeek != DayOfWeek.Sunday
        });
        
        // Rule 4: Within next 30 days
        validationRules.Add(new ValidationRule
        {
            Description = "Within 30 days",
            Passed = (date - DateTime.Now).Days <= 30
        });
    }
    
    private class ValidationRule
    {
        public string Description { get; set; }
        public bool Passed { get; set; }
    }
}
```

---

**Related Topics:**
- [Date Ranges & Special Dates](date-ranges-and-special-dates.md) - Constrain selectable dates
- [Data Binding & Events](data-binding-and-events.md) - Handle validation events
- [Getting Started](getting-started.md) - Basic setup with validation
