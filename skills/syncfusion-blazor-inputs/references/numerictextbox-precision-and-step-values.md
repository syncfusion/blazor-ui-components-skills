# Precision and Step Values

## Table of Contents
- [Decimals Property](#decimals-property)
- [ValidateDecimalOnType](#validatedecialontype)
- [Step Value Customization](#step-value-customization)
- [Spin Button Behavior](#spin-button-behavior)
- [Edge Cases](#edge-cases)
- [Best Practices](#best-practices)

## Decimals Property

The `Decimals` property restricts the maximum number of decimal places allowed in the input.

### Basic Usage

```razor
<!-- Allow 2 decimal places (e.g., 123.45) -->
<SfNumericTextBox TValue="double?" Value=10 Decimals=2></SfNumericTextBox>

<!-- Allow 3 decimal places (e.g., 123.456) -->
<SfNumericTextBox TValue="double?" Value=10 Decimals=3></SfNumericTextBox>

<!-- Allow 0 decimal places (whole numbers only) -->
<SfNumericTextBox TValue="int?" Value=10 Decimals=0></SfNumericTextBox>
```

### When Decimals Takes Effect

- **During typing:** When `ValidateDecimalOnType=true`
- **On blur:** User cannot save more decimals than allowed
- **Display:** Format property controls how decimals display

### Common Decimal Patterns

| Use Case | Decimals | TValue | Format | Example |
|----------|----------|--------|--------|---------|
| Currency/Price | 2 | decimal? | c2 | $123.45 |
| Tax/Percentage | 2 | double? | p2 | 12.34% |
| Weight/Measurement | 3 | double? | n3 | 123.456 kg |
| Whole numbers | 0 | int? | n0 | 123 |
| Scientific | 4 | double? | n4 | 1.2345e+2 |

## ValidateDecimalOnType

The `ValidateDecimalOnType` property enforces decimal precision **while typing**, providing real-time validation.

### Without ValidateDecimalOnType

```razor
<!-- ValidateDecimalOnType=false (default) -->
<SfNumericTextBox TValue="double?" 
    Value=10 
    Decimals=2
    ValidateDecimalOnType=false>
</SfNumericTextBox>

<!-- User can type: 123.456 -->
<!-- On blur: Becomes 123.46 (rounded to 2 decimals) -->
```

### With ValidateDecimalOnType

```razor
<!-- ValidateDecimalOnType=true -->
<SfNumericTextBox TValue="double?" 
    Value=10 
    Decimals=2
    ValidateDecimalOnType=true>
</SfNumericTextBox>

<!-- User tries to type: 123.456 -->
<!-- Result: 456 is rejected, stays as 123.45 -->
<!-- More restrictive but enforces precision immediately -->
```

### Comparison

| Behavior | ValidateDecimalOnType=false | ValidateDecimalOnType=true |
|----------|------------------------------|---------------------------|
| User enters "123.456" | Accepted, rounded on blur | 3rd decimal rejected |
| User experience | More lenient | Stricter, immediate feedback |
| Use case | General input | Currency, precise measurements |
| Decimal enforcement | Post-entry | Real-time during typing |

### When to Use Each

**Use `ValidateDecimalOnType=false` (default):**
- General numeric input
- When user experience priority is high
- Measurements that can be rounded
- Scientific calculations

**Use `ValidateDecimalOnType=true`:**
- Currency/money values
- Precise measurements (pharmaceutical, chemical)
- Quality control metrics
- Tax/financial calculations

### Best Practice Pattern

```razor
<!-- Currency: Strict validation -->
<SfNumericTextBox TValue="decimal?" 
    Value=0
    Format="c2"
    Decimals=2
    ValidateDecimalOnType=true
    Min=0
    Max="9999.99">
</SfNumericTextBox>

<!-- General measurement: Lenient validation -->
<SfNumericTextBox TValue="double?" 
    Value=0
    Format="n3"
    Decimals=3
    ValidateDecimalOnType=false>
</SfNumericTextBox>
```

## Step Value Customization

The `Step` property controls how much value changes when using spin buttons or arrow keys.

### Basic Step Usage

```razor
<!-- Increment by 1 (default) -->
<SfNumericTextBox TValue="int?" Value=0 Step=1></SfNumericTextBox>

<!-- Increment by 5 -->
<SfNumericTextBox TValue="int?" Value=0 Step=5></SfNumericTextBox>

<!-- Increment by 0.1 (decimal) -->
<SfNumericTextBox TValue="double?" Value=0 Step="0.1"></SfNumericTextBox>

<!-- Increment by 0.01 (currency) -->
<SfNumericTextBox TValue="decimal?" Value=0 Step="0.01"></SfNumericTextBox>
```

### Step with Min/Max Range

```razor
<!-- Quantity: 1-100, step by 1 -->
<SfNumericTextBox TValue="int?" Value=1 Min=1 Max=100 Step=1></SfNumericTextBox>

<!-- Price: $0-$1000, step by $5 -->
<SfNumericTextBox TValue="decimal?" Value=0 Min=0 Max="1000" Step="5" Format="c2"></SfNumericTextBox>

<!-- Percentage: 0-100, step by 10 -->
<SfNumericTextBox TValue="int?" Value=50 Min=0 Max=100 Step=10></SfNumericTextBox>

<!-- Volume: 0-1, step by 0.05 (5%) -->
<SfNumericTextBox TValue="double?" Value=0.5 Min=0 Max=1 Step="0.05"></SfNumericTextBox>
```

### Common Step Patterns

| Use Case | Step | Note |
|----------|------|------|
| Whole numbers | 1 | Default for integers |
| Large quantities | 5, 10, 25, 100 | Faster navigation |
| Currency (USD) | 0.01 | Penny increment |
| Percentage | 1 or 5 | Typical increments |
| Decimal measurements | 0.1 or 0.5 | Context-dependent |
| Volume/Ratio | 0.05 or 0.1 | Fractional steps |

## Spin Button Behavior

Spin buttons (up/down arrows) use the `Step` value to increment/decrement.

### Default Spin Buttons

```razor
<!-- Standard spin buttons -->
<SfNumericTextBox TValue="int?" Value=10></SfNumericTextBox>
<!-- Clicking up arrow: +1 -->
<!-- Clicking down arrow: -1 -->
```

### Spin Button with Custom Step

```razor
<!-- Custom step for faster increment -->
<SfNumericTextBox TValue="int?" Value=0 Step=10></SfNumericTextBox>
<!-- Clicking up arrow: +10 -->
<!-- Clicking down arrow: -10 -->
```

### Disabling Spin Buttons

While NumericTextBox doesn't have a built-in property to hide spin buttons, you can use CSS:

```razor
<style>
    .disable-spin-buttons .e-spin-button {
        display: none;
    }
</style>

<!-- Apply class to hide spin buttons -->
<SfNumericTextBox TValue="int?" Value=10 CssClass="disable-spin-buttons"></SfNumericTextBox>
```

### Keyboard Control

Users can also control value with keyboard:
- **Up arrow:** +Step value
- **Down arrow:** -Step value
- **Hold Shift + Up:** Larger increment (typically 10x)
- **Hold Shift + Down:** Larger decrement (typically 10x)

```razor
<!-- Example: Step=5 -->
<SfNumericTextBox TValue="int?" Value=0 Step=5></SfNumericTextBox>
<!-- Up arrow: +5 -->
<!-- Shift+Up: +50 -->
<!-- Down arrow: -5 -->
<!-- Shift+Down: -50 -->
```

## Edge Cases

### Decimal Rounding

```razor
<!-- Input: 123.456 with Decimals=2 -->
<!-- Result: 123.46 (rounds to nearest) -->
<SfNumericTextBox TValue="double?" Decimals=2></SfNumericTextBox>

<!-- With ValidateDecimalOnType=true: 3rd decimal rejected -->
<SfNumericTextBox TValue="double?" Decimals=2 ValidateDecimalOnType=true></SfNumericTextBox>
```

### Step Exceeds Range

```razor
<!-- Min=0, Max=10, Step=15 -->
<!-- Clicking up from 0: Becomes 15 (exceeds max) -->
<!-- System adjusts to Max value -->
<SfNumericTextBox TValue="int?" Min=0 Max=10 Step=15></SfNumericTextBox>
```

### Very Small Decimals

```razor
<!-- Handling very small numbers -->
<SfNumericTextBox TValue="double?" Value="0.00001" Decimals=5></SfNumericTextBox>
<!-- Works fine with appropriate Decimals setting -->

<!-- With Step too large -->
<SfNumericTextBox TValue="double?" 
    Value="0.00001" 
    Decimals=5 
    Step="0.1">
</SfNumericTextBox>
<!-- Step value dominates precision control -->
```

### Negative Numbers with Step

```razor
<!-- Negative range: -100 to 100, step 5 -->
<SfNumericTextBox TValue="int?" 
    Value=0 
    Min="-100" 
    Max="100" 
    Step=5>
</SfNumericTextBox>
<!-- Down arrow: -5 -->
<!-- Up arrow: +5 -->
```

## Best Practices

### 1. Match Decimals and Format

```razor
<!-- ✅ Correct: Decimals matches Format precision -->
<SfNumericTextBox TValue="decimal?" 
    Format="c2" 
    Decimals=2>
</SfNumericTextBox>

<!-- ❌ Avoid: Mismatch between Decimals and Format -->
<SfNumericTextBox TValue="decimal?" 
    Format="c2" 
    Decimals=3>
</SfNumericTextBox>
```

### 2. Use ValidateDecimalOnType for Currency

```razor
<!-- ✅ Good: Currency with strict validation -->
<SfNumericTextBox TValue="decimal?" 
    Format="c2" 
    Decimals=2 
    ValidateDecimalOnType=true>
</SfNumericTextBox>
```

### 3. Choose Appropriate Step Values

```razor
<!-- ✅ Good: Step matches use case -->
<SfNumericTextBox TValue="decimal?" 
    Value=0 
    Min=0 
    Max="1000" 
    Step="0.01" 
    Format="c2">
</SfNumericTextBox>

<!-- ❌ Avoid: Step mismatches range -->
<SfNumericTextBox TValue="decimal?" 
    Value=0 
    Min=0 
    Max="100" 
    Step="50">
</SfNumericTextBox>
```

### 4. Complete Pattern for Precision-Critical Input

```razor
<!-- Template for financial/precise input -->
<SfNumericTextBox TValue="decimal?" 
    Value=0
    Min=0
    Max="999999.99"
    Step="0.01"
    Format="c2"
    Decimals=2
    ValidateDecimalOnType=true
    Placeholder="Enter amount">
</SfNumericTextBox>
```

### 5. Mobile-Friendly Step Values

```razor
<!-- Larger step for easier mobile interaction -->
<SfNumericTextBox TValue="int?" 
    Value=0 
    Min=0 
    Max=100 
    Step=5
    Placeholder="Mobile-friendly">
</SfNumericTextBox>
```

## Summary

| Property | Purpose | When to Use |
|----------|---------|------------|
| `Decimals` | Limit decimal places | Always for decimal inputs |
| `ValidateDecimalOnType` | Real-time decimal validation | Currency, precise measurements |
| `Step` | Spin button increment | When Min/Max range is set |
| `Format` | Display formatting | Currency, percentage, measurements |

## Next Steps

- See [data-binding-and-events.md](data-binding-and-events.md) for form integration
- See [range-validation-and-formatting.md](range-validation-and-formatting.md) for validation patterns
- See [customization-styling.md](customization-styling.md) for appearance control
