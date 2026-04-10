# RangeSlider - Range Configuration

## Table of Contents
- [Overview](#overview)
- [Min, Max, and Step Properties](#min-max-and-step-properties)
- [SliderType Property](#slidertype-property)
- [Value Binding and Data Types](#value-binding-and-data-types)
- [Custom Non-Numeric Values](#custom-non-numeric-values)
- [IsImmediateValue Property](#isimmediatevalue-property)
- [Common Configuration Patterns](#common-configuration-patterns)
- [Validation and Constraints](#validation-and-constraints)

## Overview

Range configuration controls the fundamental behavior of the RangeSlider, including the selectable value range, increment steps, value data types, and real-time update behavior. Proper configuration ensures the slider matches your business requirements and provides optimal user experience.

## Min, Max, and Step Properties

### Basic Range Configuration

The `Min` and `Max` properties define the bounds of selectable values:

```razor
<SfSlider @bind-Value="@rangeValues"
          Type="SliderType.Range"
          Min="0"
          Max="1000">
</SfSlider>

@code {
    private int[] rangeValues = new int[] { 200, 800 };
}
```

**Key Points:**
- `Min` sets the minimum value (default: 0)
- `Max` sets the maximum value (default: 100)
- Initial values must be within Min/Max bounds
- Values are inclusive of Min and Max

### Step Property for Increments

The `Step` property controls value increments when dragging handles:

```razor
<!-- Step by 10 -->
<SfSlider @bind-Value="@priceRange"
          Type="SliderType.Range"
          Min="0"
          Max="1000"
          Step="10">
</SfSlider>

<!-- Step by 0.5 for decimals -->
<SfSlider @bind-Value="@temperatureRange"
          Type="SliderType.Range"
          Min="0.0"
          Max="50.0"
          Step="0.5">
</SfSlider>

@code {
    private int[] priceRange = new int[] { 100, 500 };
    private double[] temperatureRange = new double[] { 18.0, 25.5 };
}
```

**Step Behavior:**
- Default value is 1
- Handles snap to Step intervals when dragged
- Must be a positive number
- Can be decimal (e.g., 0.1, 0.25, 0.5)
- Smaller steps provide more granular control

### Common Range Configurations

```razor
<!-- Age range in years -->
<SfSlider @bind-Value="@ageRange" 
          Type="SliderType.Range"
          Min="18" 
          Max="100" 
          Step="1">
</SfSlider>

<!-- Price range with $10 increments -->
<SfSlider @bind-Value="@budgetRange"
          Type="SliderType.Range"
          Min="0"
          Max="5000"
          Step="10">
</SfSlider>

<!-- Percentage range with 5% steps -->
<SfSlider @bind-Value="@discountRange"
          Type="SliderType.Range"
          Min="0"
          Max="100"
          Step="5">
</SfSlider>

@code {
    private int[] ageRange = new int[] { 25, 45 };
    private int[] budgetRange = new int[] { 500, 2000 };
    private int[] discountRange = new int[] { 10, 50 };
}
```

## SliderType Property

### Range vs Default Type

The `Type` property determines single-value vs range selection:

```razor
<!-- Range Slider - Two handles -->
<SfSlider @bind-Value="@rangeValues" 
          Type="SliderType.Range"
          Min="0" 
          Max="100">
</SfSlider>

<!-- Default Slider - Single handle (for comparison) -->
<SfSlider @bind-Value="@singleValue"
          Type="SliderType.Default"
          Min="0"
          Max="100">
</SfSlider>

@code {
    private int[] rangeValues = new int[] { 30, 70 };
    private int singleValue = 50;
}
```

**SliderType.Range Requirements:**
- Value must be a two-element array
- Provides start and end handles
- Suitable for range selection scenarios
- First array element is the left/start handle
- Second array element is the right/end handle

**When to Use Range Type:**
- Price filtering (min and max price)
- Date ranges (start and end dates)
- Age or demographic ranges
- Time windows (start and end times)
- Any scenario requiring two-value selection

## Value Binding and Data Types

### Integer Arrays

Most common for whole number ranges:

```razor
<SfSlider @bind-Value="@intRange"
          Type="SliderType.Range"
          Min="0"
          Max="100"
          Step="1">
</SfSlider>

@code {
    private int[] intRange = new int[] { 25, 75 };
    
    // Access values
    private void UseValues()
    {
        int startValue = intRange[0];  // 25
        int endValue = intRange[1];    // 75
        int span = endValue - startValue;  // 50
    }
}
```

### Double Arrays for Decimal Values

Use for precise decimal ranges:

```razor
<SfSlider @bind-Value="@doubleRange"
          Type="SliderType.Range"
          Min="0.0"
          Max="100.0"
          Step="0.1">
</SfSlider>

@code {
    private double[] doubleRange = new double[] { 25.5, 75.8 };
    
    // Format for display
    private string DisplayRange => $"{doubleRange[0]:F1} - {doubleRange[1]:F1}";
}
```

### Two-Way Binding with @bind-Value

Automatic synchronization between UI and component:

```razor
<SfSlider @bind-Value="@selectedRange"
          Type="SliderType.Range"
          Min="0"
          Max="1000">
</SfSlider>

<p>Current Range: @selectedRange[0] - @selectedRange[1]</p>

<button @onclick="ResetRange">Reset to Default</button>

@code {
    private int[] selectedRange = new int[] { 200, 800 };
    
    private void ResetRange()
    {
        selectedRange = new int[] { 0, 1000 };
    }
}
```

**Two-Way Binding Features:**
- Automatic UI updates when array values change in code
- Automatic code updates when user drags handles
- No need for manual event handlers for basic scenarios
- Works with both int[] and double[]

## Custom Non-Numeric Values

Use `CustomValues` for non-numeric ranges (sizes, priorities, labels):

```razor
<!-- T-Shirt Size Range -->
<SfSlider @bind-Value="@sizeRange"
          Type="SliderType.Range"
          CustomValues="@sizeLabels">
</SfSlider>

<p>Selected Sizes: @sizeLabels[sizeRange[0]] to @sizeLabels[sizeRange[1]]</p>

@code {
    private string[] sizeLabels = new string[] { "XS", "S", "M", "L", "XL", "XXL" };
    private int[] sizeRange = new int[] { 1, 4 };  // Indices: S to XL
}
```

### Priority Level Range

```razor
<SfSlider @bind-Value="@priorityRange"
          Type="SliderType.Range"
          CustomValues="@priorityLevels">
</SfSlider>

<div>
    <strong>Selected Priority Range:</strong> 
    @priorityLevels[priorityRange[0]] to @priorityLevels[priorityRange[1]]
</div>

@code {
    private string[] priorityLevels = new string[] { "Low", "Medium", "High", "Critical" };
    private int[] priorityRange = new int[] { 0, 2 };  // Low to High
}
```

### Month Range Selection

```razor
<SfSlider @bind-Value="@monthRange"
          Type="SliderType.Range"
          CustomValues="@months">
    <SliderTicks Placement="Placement.After" LargeStep="1" ShowSmallTicks="false"></SliderTicks>
</SfSlider>

<p>Season: @months[monthRange[0]] - @months[monthRange[1]]</p>

@code {
    private string[] months = new string[] 
    { 
        "Jan", "Feb", "Mar", "Apr", "May", "Jun", 
        "Jul", "Aug", "Sep", "Oct", "Nov", "Dec" 
    };
    private int[] monthRange = new int[] { 0, 11 };  // Jan to Dec
}
```

**CustomValues Key Points:**
- Value array contains indices, not the actual custom values
- First index must be >= 0
- Last index must be < CustomValues.Length
- Step property is ignored when CustomValues is used
- Min and Max are automatically set based on array length

## IsImmediateValue Property

Controls when the Value property updates during handle dragging:

### Default Behavior (IsImmediateValue = false)

Value updates only when handle is released:

```razor
<SfSlider @bind-Value="@rangeValues"
          Type="SliderType.Range"
          IsImmediateValue="false"
          Min="0"
          Max="100">
</SfSlider>

<p>Value updates on handle release: @rangeValues[0] - @rangeValues[1]</p>

@code {
    private int[] rangeValues = new int[] { 30, 70 };
    // Updates only after user releases the handle
}
```

### Real-Time Updates (IsImmediateValue = true)

Value updates continuously during dragging:

```razor
<SfSlider @bind-Value="@liveRange"
          Type="SliderType.Range"
          IsImmediateValue="true"
          Min="0"
          Max="100">
</SfSlider>

<p>Live updates: @liveRange[0] - @liveRange[1]</p>
<p>Range span: @(liveRange[1] - liveRange[0])</p>

@code {
    private int[] liveRange = new int[] { 30, 70 };
    // Updates continuously as user drags
}
```

**When to Use IsImmediateValue:**
- **true**: Real-time filtering, live previews, instant visual feedback
- **false**: Reduces render cycles, better for expensive operations triggered on value change

### Performance Consideration

```razor
<!-- Use IsImmediateValue=false for expensive operations -->
<SfSlider @bind-Value="@priceRange"
          Type="SliderType.Range"
          IsImmediateValue="false"
          Min="0"
          Max="10000">
</SfSlider>

@code {
    private int[] priceRange = new int[] { 1000, 5000 };
    
    protected override async Task OnParametersSetAsync()
    {
        // Expensive database query only runs on handle release
        await FilterProductsByPrice(priceRange[0], priceRange[1]);
    }
}
```

## Common Configuration Patterns

### Pattern 1: Price Range Filter
```razor
<SfSlider @bind-Value="@priceRange"
          Type="SliderType.Range"
          Min="0"
          Max="10000"
          Step="100"
          IsImmediateValue="false">
</SfSlider>

@code {
    private int[] priceRange = new int[] { 500, 5000 };
}
```

### Pattern 2: Temperature Range (Celsius)
```razor
<SfSlider @bind-Value="@tempRange"
          Type="SliderType.Range"
          Min="-20.0"
          Max="50.0"
          Step="0.5">
</SfSlider>

@code {
    private double[] tempRange = new double[] { 18.0, 26.0 };
}
```

### Pattern 3: Age Demographic Range
```razor
<SfSlider @bind-Value="@ageRange"
          Type="SliderType.Range"
          Min="0"
          Max="120"
          Step="1">
</SfSlider>

@code {
    private int[] ageRange = new int[] { 18, 65 };
}
```

### Pattern 4: Custom Size Range
```razor
<SfSlider @bind-Value="@sizeRange"
          Type="SliderType.Range"
          CustomValues="@sizes">
</SfSlider>

@code {
    private string[] sizes = new string[] { "XS", "S", "M", "L", "XL", "XXL", "XXXL" };
    private int[] sizeRange = new int[] { 1, 4 };  // S to XL
}
```

## Validation and Constraints

### Ensuring Valid Initial Values

```csharp
@code {
    private int Min = 0;
    private int Max = 1000;
    
    private int[] rangeValues = new int[] { 200, 800 };
    
    protected override void OnInitialized()
    {
        // Validate initial values
        if (rangeValues[0] < Min) rangeValues[0] = Min;
        if (rangeValues[1] > Max) rangeValues[1] = Max;
        if (rangeValues[0] >= rangeValues[1]) 
        {
            // Ensure start < end
            rangeValues[1] = rangeValues[0] + (Max - Min) / 2;
        }
    }
}
```

### Dynamic Min/Max Based on Data

```razor
<SfSlider @bind-Value="@dateRange"
          Type="SliderType.Range"
          Min="@minDate"
          Max="@maxDate"
          Step="1">
</SfSlider>

@code {
    private int minDate = 1;
    private int maxDate = 31;
    private int[] dateRange = new int[] { 5, 25 };
    
    protected override void OnInitialized()
    {
        // Adjust based on selected month
        maxDate = DateTime.DaysInMonth(2024, 2);  // February: 29 days
        
        // Ensure range is valid for new max
        if (dateRange[1] > maxDate) dateRange[1] = maxDate;
    }
}
```

### Preventing Invalid Ranges

```csharp
private int[] _rangeValues = new int[] { 30, 70 };

private int[] rangeValues
{
    get => _rangeValues;
    set
    {
        // Ensure minimum gap of 10
        if (value[1] - value[0] < 10)
        {
            value[1] = value[0] + 10;
        }
        _rangeValues = value;
        StateHasChanged();
    }
}
```

## Troubleshooting

### Invalid Value Array Length
**Issue:** Only one handle appears or component doesn't work.

**Solution:** Ensure array has exactly two elements:
```csharp
// ✅ Correct
private int[] values = new int[] { 10, 90 };

// ❌ Wrong
private int[] values = new int[] { 50 };  // Only one element
private int[] values = new int[] { 10, 50, 90 };  // Three elements
```

### Values Outside Min/Max Range
**Issue:** Handles positioned incorrectly on initialization.

**Solution:** Validate values are within bounds:
```csharp
// Min=0, Max=100
private int[] values = new int[] { 20, 80 };  // ✅ Valid

// ❌ Invalid - clamping required
private int[] values = new int[] { -10, 110 };
```

### CustomValues Index Out of Range
**Issue:** Error when using CustomValues.

**Solution:** Ensure indices are valid:
```csharp
private string[] sizes = new string[] { "S", "M", "L", "XL" };
private int[] sizeRange = new int[] { 0, 3 };  // ✅ Valid (0 to 3)

// ❌ Invalid - index 4 doesn't exist
private int[] sizeRange = new int[] { 0, 4 };
```

## Next Steps

- **Ticks and Tooltip** - Add visual markers and value display
- **Color Ranges** - Implement visual feedback with colored segments
- **Limits** - Restrict handle movement within specific bounds
- **Events** - Handle value changes and validate ranges
