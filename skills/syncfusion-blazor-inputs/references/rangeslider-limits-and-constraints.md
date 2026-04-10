# RangeSlider - Limits and Constraints

## Table of Contents
- [Overview](#overview)
- [SliderLimits Configuration](#sliderlimits-configuration)
- [MinStart and MaxStart Properties](#minstart-and-maxstart-properties)
- [MinEnd and MaxEnd Properties](#minend-and-maxend-properties)
- [Fixed Handle Positions](#fixed-handle-positions)
- [Common Constraint Patterns](#common-constraint-patterns)
- [Validation with Limits](#validation-with-limits)
- [Use Cases and Scenarios](#use-cases-and-scenarios)

## Overview

Slider limits allow you to restrict handle movement within specific bounds, creating constraints on the selectable range. This feature is essential for scenarios like booking systems (minimum stay requirements), budget planning (enforcing budget ranges), scheduling (minimum/maximum time windows), and any situation where you need to control the allowable range selection.

## SliderLimits Configuration

### Basic Limits Setup

Add `SliderLimits` component to restrict handle movement:

```razor
@using Syncfusion.Blazor.Inputs

<SfSlider @bind-Value="@dateRange"
          Type="SliderType.Range"
          Min="1"
          Max="31">
    <SliderLimits Enabled="true"
                  MinStart="5"
                  MinEnd="15"
                  MaxStart="15"
                  MaxEnd="25">
    </SliderLimits>
    <SliderTicks Placement="Placement.After" LargeStep="5"></SliderTicks>
    <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
</SfSlider>

<p>Available dates: Day @dateRange[0] to Day @dateRange[1]</p>

@code {
    private int[] dateRange = new int[] { 10, 20 };
    // Start handle can't go below day 5
    // End handle can't go beyond day 25
}
```

**Key Points:**
- `SliderLimits` is a child component of `SfSlider`
- `Enabled="true"` activates limit constraints
- Limits restrict how far handles can be moved
- Handles cannot cross the defined boundaries

## MinStart and MaxStart Properties

### MinStart - Minimum Value for Start Handle

Prevents the start (left) handle from going below a certain value:

```razor
<div class="booking-slider">
    <h4>Hotel Booking Dates</h4>
    <SfSlider @bind-Value="@bookingRange"
              Type="SliderType.Range"
              Min="1"
              Max="30">
        
        <SliderLimits Enabled="true"
                      MinStart="5"
                      MinEnd="29"
                      MaxStart="6"
                      MaxEnd="30">
        </SliderLimits>
        <SliderTicks Placement="Placement.After" LargeStep="5"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Booking Period: Day @bookingRange[0] - @bookingRange[1]</p>
    <p class="info">⚠️ Check-in available from Day 5 onwards</p>
</div>

@code {
    private int[] bookingRange = new int[] { 10, 20 };
    // User cannot select check-in before day 5
}
```

### MaxStart - Maximum Value for Start Handle

Prevents the start handle from exceeding a certain value:

```razor
<div class="early-bird-slider">
    <h4>Early Registration Period</h4>
    <SfSlider @bind-Value="@registrationRange"
              Type="SliderType.Range"
              Min="1"
              Max="60">
        <SliderLimits Enabled="true" 
                      MinStart="1"
                      MaxStart="14"
                      MinEnd="2"
                      MaxEnd="60">
            <!-- Start handle cannot exceed day 14 (early bird deadline) -->
        </SliderLimits>
        <SliderTicks Placement="Placement.After" LargeStep="7"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Registration Window: Day @registrationRange[0] - @registrationRange[1]</p>
    <p class="info">🎯 Early bird discount applies only if start date is before Day 14</p>
</div>

@code {
    private int[] registrationRange = new int[] { 5, 30 };
    // Start date limited to first 14 days for early bird pricing
}
```

### Combining MinStart and MaxStart

Create a specific range where the start handle can move:

```razor
<div class="project-timeline">
    <h4>Project Phase Timeline (Weeks)</h4>
    <SfSlider @bind-Value="@phaseRange"
              Type="SliderType.Range"
              Min="1"
              Max="52">
        <SliderLimits Enabled="true" 
                      MinStart="4"
                      MaxStart="12"
                      MinEnd="5"
                      MaxEnd="52">
            <!-- Start handle restricted to weeks 4-12 -->
        </SliderLimits>
        <SliderTicks Placement="Placement.After" LargeStep="4"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Phase Duration: Week @phaseRange[0] to Week @phaseRange[1]</p>
    <p class="info">📅 Project phase must start between weeks 4-12</p>
</div>

@code {
    private int[] phaseRange = new int[] { 8, 20 };
    // Start handle can only be positioned between weeks 4 and 12
}
```

## MinEnd and MaxEnd Properties

### MinEnd - Minimum Value for End Handle

Prevents the end (right) handle from going below a certain value:

```razor
<div class="minimum-stay-slider">
    <h4>Hotel Stay Period</h4>
    <SfSlider @bind-Value="@stayRange"
              Type="SliderType.Range"
                      MinStart="1"
                      MaxStart="23"
                      MinEnd="7"
                      MaxEnd="30">
        <SliderLimits Enabled="true" MinEnd="7">
            <!-- End handle cannot go below day 7 (minimum 7-day stay) -->
        </SliderLimits>
        <SliderTicks Placement="Placement.After" LargeStep="5"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Stay: Day @stayRange[0] to Day @stayRange[1] (@(stayRange[1] - stayRange[0]) days)</p>
    <p class="info">⏱️ Minimum stay: 7 days</p>
</div>

@code {
    private int[] stayRange = new int[] { 5, 15 };
    // Check-out must be at least on day 7
}
```

### MaxEnd - Maximum Value for End Handle

Prevents the end handle from exceeding a certain value:

```razor
<div class="blackout-dates-slider">
    <h4>Rental Period (Holiday Blackout After Day 20)</h4>
    <SfSlider @bind-Value="@rentalRange"
              Type="SliderType.Range"
              Min="1"
              Max="31">
        <SliderLimits Enabled="true" MaxEnd="20">
            <!-- End handle cannot exceed day 20 (blackout period starts) -->
        </SliderLimits>
        <SliderTicks Placement="Placement.After" LargeStep="5"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Rental: Day @rentalRange[0] - @rentalRange[1]</p>
    <p class="warning">🚫 No rentals available after Day 20 (holiday period)</p>
</div>

@code {
    private int[] rentalRange = new int[] { 5, 15 };
    // Rental must end before day 20
}
```

### Combining MinEnd and MaxEnd

Create a specific range where the end handle can move:

```razor
<div class="delivery-window">
    <h4>Delivery Time Window</h4>
    <SfSlider @bind-Value="@deliveryRange"
              Type="SliderType.Range"
              Min="1"
              Max="30">
        <SliderLimits Enabled="true" 
                      MinStart="1"
                      MaxStart="20"
                      MinEnd="7"
                      MaxEnd="21">
            <!-- End handle restricted to days 7-21 -->
        </SliderLimits>
        <SliderTicks Placement="Placement.After" LargeStep="7"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Delivery Window: Day @deliveryRange[0] to Day @deliveryRange[1]</p>
    <p class="info">📦 Delivery completion must be between days 7-21</p>
</div>

@code {
    private int[] deliveryRange = new int[] { 3, 14 };
    // End handle can only be positioned between days 7 and 21
}
```

## Fixed Handle Positions

### StartHandleFixed - Lock Start Handle

Prevent movement of the start handle while allowing end handle to move:

```razor
<div class="project-start-fixed">
    <h4>Project Duration (Start Date Fixed)</h4>
    <SfSlider @bind-Value="@projectRange"
              Type="SliderType.Range"
              Min="1"
              Max="52">
        <SliderLimits Enabled="true" StartHandleFixed="true">
            <!-- Start handle is locked at initial position -->
        </SliderLimits>
        <SliderTicks Placement="Placement.After" LargeStep="4"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Project: Week @projectRange[0] (fixed) to Week @projectRange[1]</p>
    <p class="info">🔒 Project starts Week @projectRange[0] - only adjust end date</p>
</div>

@code {
    private int[] projectRange = new int[] { 10, 25 };
    // User can only adjust the end date
}
```

### EndHandleFixed - Lock End Handle

Prevent movement of the end handle while allowing start handle to move:

```razor
<div class="deadline-fixed">
    <h4>Task Duration (Deadline Fixed)</h4>
    <SfSlider @bind-Value="@taskRange"
              Type="SliderType.Range"
              Min="1"
              Max="30">
        <SliderLimits Enabled="true" EndHandleFixed="true">
            <!-- End handle is locked at initial position -->
        </SliderLimits>
        <SliderTicks Placement="Placement.After" LargeStep="5"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Task Window: Day @taskRange[0] to Day @taskRange[1] (deadline)</p>
    <p class="info">⏰ Deadline is Day @taskRange[1] - only adjust start date</p>
</div>

@code {
    private int[] taskRange = new int[] { 10, 30 };
    // User can only adjust the start date
}
```

### Both Handles Fixed (Read-Only Range Display)

Lock both handles to display a non-editable range:

```razor
<div class="read-only-range">
    <h4>Approved Vacation Period (Read-Only)</h4>
    <SfSlider @bind-Value="@vacationRange"
              Type="SliderType.Range"
              Min="1"
              Max="365">
        <SliderLimits Enabled="true" 
                      StartHandleFixed="true" 
                      EndHandleFixed="true">
        </SliderLimits>
        <SliderTicks Placement="Placement.After" LargeStep="30"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>✓ Approved: Day @vacationRange[0] to Day @vacationRange[1]</p>
</div>

@code {
    private int[] vacationRange = new int[] { 150, 165 };
    // Both handles locked - display only
}
```

**Note:** For purely read-only display, consider using `ReadOnly="true"` on `SfSlider` instead.

## Common Constraint Patterns

### Pattern 1: Minimum Range Span

Ensure a minimum gap between start and end handles:

```razor
<div class="minimum-span-slider">
    <h4>Booking Period (Minimum 3 Days)</h4>
    <SfSlider @bind-Value="@bookingRange"
              Type="SliderType.Range"
              Min="1"
              Max="30">
        <SliderLimits Enabled="true">
            <!-- Enforce minimum span via code -->
        </SliderLimits>
        <SliderTicks Placement="Placement.After" LargeStep="5"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Booking: Day @bookingRange[0] - @bookingRange[1] (@(bookingRange[1] - bookingRange[0]) days)</p>
</div>

@code {
    private int[] _bookingRange = new int[] { 5, 10 };
    
    private int[] bookingRange
    {
        get => _bookingRange;
        set
        {
            // Enforce minimum 3-day span
            if (value[1] - value[0] < 3)
            {
                value[1] = value[0] + 3;
            }
            _bookingRange = value;
        }
    }
}
```

### Pattern 2: Maximum Range Span

Limit the maximum gap between handles:

```razor
<div class="maximum-span-slider">
    <h4>Vacation Request (Max 14 Days)</h4>
    <SfSlider @bind-Value="@vacationRange"
              Type="SliderType.Range"
              Min="1"
              Max="365">
        <SliderLimits Enabled="true">
            <!-- Enforce maximum span via code -->
        </SliderLimits>
        <SliderTicks Placement="Placement.After" LargeStep="30"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Request: Day @vacationRange[0] - @vacationRange[1] (@(vacationRange[1] - vacationRange[0]) days)</p>
    @if (vacationRange[1] - vacationRange[0] > 14)
    {
        <p class="error">❌ Maximum vacation period is 14 days</p>
    }
</div>

@code {
    private int[] _vacationRange = new int[] { 100, 110 };
    
    private int[] vacationRange
    {
        get => _vacationRange;
        set
        {
            // Enforce maximum 14-day span
            if (value[1] - value[0] > 14)
            {
                value[1] = value[0] + 14;
            }
            _vacationRange = value;
        }
    }
}
```

### Pattern 3: Specific Allowed Window

Combine multiple limit properties for complex constraints:

```razor
<div class="allowed-window">
    <h4>Early Bird Registration Window</h4>
    <SfSlider @bind-Value="@registrationWindow"
              Type="SliderType.Range"
              Min="1"
              Max="90">
        <SliderLimits Enabled="true"
                      MinStart="7"
                      MaxStart="30"
                      MinEnd="14"
                      MaxEnd="60">
            <!-- Start: days 7-30, End: days 14-60 -->
        </SliderLimits>
        <SliderTicks Placement="Placement.After" LargeStep="10"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Registration: Day @registrationWindow[0] to Day @registrationWindow[1]</p>
</div>

@code {
    private int[] registrationWindow = new int[] { 15, 45 };
    // Start must be between days 7-30
    // End must be between days 14-60
}
```

### Pattern 4: Dynamic Limits Based on Business Logic

```razor
<div class="dynamic-limits">
    <h4>Season Booking</h4>
    
    <label>
        <input type="checkbox" @bind="isPeakSeason" @bind:event="onchange" />
        Peak Season
    </label>
    
    <SfSlider @bind-Value="@bookingRange"
              Type="SliderType.Range"
              Min="1"
              Max="30">
        <SliderLimits Enabled="true"
                      MinStart="1"
                      MaxStart="@(30 - minimumStay)"
                      MinEnd="@(bookingRange[0] + minimumStay)"
                      MaxEnd="30">
            <!-- Minimum stay changes based on season -->
        </SliderLimits>
        <SliderTicks Placement="Placement.After" LargeStep="5"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Stay: Day @bookingRange[0] - @bookingRange[1] (@(bookingRange[1] - bookingRange[0]) days)</p>
    <p class="info">Minimum stay: @minimumStay days</p>
</div>

@code {
    private int[] bookingRange = new int[] { 5, 12 };
    private bool isPeakSeason = false;
    
    private int minimumStay => isPeakSeason ? 7 : 3;
}
```

## Validation with Limits

### Form Validation with Limits

```razor
<EditForm Model="@reservationModel" OnValidSubmit="HandleSubmit">
    <DataAnnotationsValidator />
    
    <div class="form-group">
        <label>Reservation Period (Days @reservationModel.MinDate - @reservationModel.MaxDate)</label>
        <SfSlider @bind-Value="@reservationModel.DateRange"
                  Type="SliderType.Range"
                  Min="1"
                  Max="31">
            <SliderLimits Enabled="true"
                          MinStart="@reservationModel.MinDate"
                          MaxStart="@(reservationModel.MaxDate - 1)"
                          MaxEnd="@reservationModel.MaxDate"
                          MinEnd="@reservationModel.MinStayEnd">
            </SliderLimits>
            <SliderTicks Placement="Placement.After" LargeStep="5"></SliderTicks>
            <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
        </SfSlider>
        <ValidationMessage For="@(() => reservationModel.DateRange)" />
    </div>
    
    <button type="submit" class="btn btn-primary">Submit Reservation</button>
</EditForm>

@code {
    private ReservationModel reservationModel = new ReservationModel
    {
        MinDate = 5,
        MaxDate = 25,
        MinStayEnd = 8,  // Minimum 3-day stay (5 + 3)
        DateRange = new int[] { 10, 15 }
    };
    
    private void HandleSubmit()
    {
        Console.WriteLine($"Reservation: Day {reservationModel.DateRange[0]} - {reservationModel.DateRange[1]}");
    }
    
    public class ReservationModel
    {
        public int MinDate { get; set; }
        public int MaxDate { get; set; }
        public int MinStayEnd { get; set; }
        
        [Required]
        [MinLength(2, ErrorMessage = "Date range must have start and end dates")]
        public int[] DateRange { get; set; }
    }
}
```

### Custom Validation Logic

```csharp
private bool ValidateRange()
{
    int span = bookingRange[1] - bookingRange[0];
    
    // Minimum span validation
    if (span < 3)
    {
        errorMessage = "Minimum booking period is 3 days";
        return false;
    }
    
    // Maximum span validation
    if (span > 14)
    {
        errorMessage = "Maximum booking period is 14 days";
        return false;
    }
    
    // Business rule: no bookings ending on day 31
    if (bookingRange[1] == 31)
    {
        errorMessage = "Check-out not available on day 31";
        return false;
    }
    
    errorMessage = string.Empty;
    return true;
}
```

## Use Cases and Scenarios

### Use Case 1: Hotel Booking with Minimum Stay

```razor
<div class="hotel-booking">
    <h4>Hotel Reservation</h4>
    <SfSlider @bind-Value="@hotelStay"
              Type="SliderType.Range"
              Min="1"
              Max="31">
        <SliderLimits Enabled="true"
                      MinStart="@earliestCheckIn"
                      MaxStart="@(latestCheckOut - minimumNights)"
                      MaxEnd="@latestCheckOut"
                      MinEnd="@(hotelStay[0] + minimumNights)">
        </SliderLimits>
        <SliderTicks Placement="Placement.After" LargeStep="5"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Check-in: Day @hotelStay[0] | Check-out: Day @hotelStay[1]</p>
    <p>Duration: @(hotelStay[1] - hotelStay[0]) nights (Minimum: @minimumNights)</p>
</div>

@code {
    private int[] hotelStay = new int[] { 10, 14 };
    private int earliestCheckIn = 5;
    private int latestCheckOut = 28;
    private int minimumNights = 3;
}
```

### Use Case 2: Budget Allocation with Constraints

```razor
<div class="budget-allocation">
    <h4>Department Budget Allocation</h4>
    <SfSlider @bind-Value="@budgetRange"
              Type="SliderType.Range"
              Min="0"
              Max="1000000"
              Step="10000">
        <SliderLimits Enabled="true"
                      MinStart="@minimumBudget"
                      MaxStart="@(maximumBudget - step)"
                      MinEnd="@(minimumBudget + step)"
                      MaxEnd="@maximumBudget">
        </SliderLimits>
        <SliderTicks Placement="Placement.After" LargeStep="200000" Format="C0"></SliderTicks>
        <SliderTooltip IsVisible="true" Format="C0" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Allocated: @budgetRange[0].ToString("C0") - @budgetRange[1].ToString("C0")</p>
</div>

@code {
    private int[] budgetRange = new int[] { 150000, 600000 };
    private int minimumBudget = 100000;    // Minimum required
    private int maximumBudget = 750000;    // Maximum available
}
```

### Use Case 3: Production Schedule with Maintenance Windows

```razor
<div class="production-schedule">
    <h4>Production Schedule (Days 15-20: Maintenance)</h4>
    <SfSlider @bind-Value="@productionPeriod"
              Type="SliderType.Range"
              Min="1"
              Max="31">
        <SliderLimits Enabled="true"
                      MinStart="1"
                      MaxStart="14"
                      MinEnd="21"
                      MaxEnd="31">
            <!-- Production cannot overlap maintenance (days 15-20) -->
        </SliderLimits>
        <SliderTicks Placement="Placement.After" LargeStep="5"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always"></SliderTooltip>
    </SfSlider>
    <p>Production: Day @productionPeriod[0] - @productionPeriod[1]</p>
    <p class="warning">🔧 Maintenance: Days 15-20 (No Production)</p>
</div>

@code {
    private int[] productionPeriod = new int[] { 5, 25 };
    // Must start before day 15 and end after day 20
}
```

## Troubleshooting

### Limits Not Working
**Issue:** Handles move beyond defined limits.

**Solution:** Ensure `Enabled="true"` is set:
```razor
<SliderLimits Enabled="true" MinStart="10" MaxEnd="80"></SliderLimits>
```

### Invalid Initial Values
**Issue:** Initial values violate limit constraints, causing unexpected behavior.

**Solution:** Validate initial values against limits:
```csharp
protected override void OnInitialized()
{
    // Ensure initial values respect limits
    if (rangeValues[0] < minStart) rangeValues[0] = minStart;
    if (rangeValues[1] > maxEnd) rangeValues[1] = maxEnd;
}
```

### Fixed Handle Still Moves
**Issue:** Handle marked as fixed still responds to drag.

**Solution:** Verify both `Enabled="true"` and fixed property are set:
```razor
<SliderLimits Enabled="true" StartHandleFixed="true"></SliderLimits>
```

## Next Steps

- **Orientation** - Apply limits to vertical sliders
- **Events** - Validate range on value change
- **Customization** - Style slider to highlight restricted areas
- **Form Integration** - Combine limits with validation
