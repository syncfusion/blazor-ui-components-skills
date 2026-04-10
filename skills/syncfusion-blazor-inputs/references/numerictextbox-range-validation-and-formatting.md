# Range Validation and Formatting

## Table of Contents
- [Range Validation](#range-validation)
- [Min and Max Properties](#min-and-max-properties)
- [Number Formatting](#number-formatting)
- [Format Strings](#format-strings)
- [Currency Formatting](#currency-formatting)
- [Percentage Formatting](#percentage-formatting)
- [Custom Format Examples](#custom-format-examples)
- [Best Practices](#best-practices)

## Range Validation

Range validation prevents users from entering values outside specified limits. It's enforced at the UI level, making forms more user-friendly.

### Why Use Range Validation?

- **Prevent invalid data entry** - Stop users from entering out-of-range values
- **Improve data quality** - Ensure values fit business logic
- **Better UX** - Disable invalid actions instead of showing errors later
- **Mobile friendly** - Works with native numeric keyboards

## Min and Max Properties

Use `Min` and `Max` properties to define valid input range.

### Basic Example

```razor
<!-- Quantity between 1 and 100 -->
<SfNumericTextBox TValue="int?" Value=5 Min=1 Max=100></SfNumericTextBox>

<!-- Temperature between -50 and 50 degrees -->
<SfNumericTextBox TValue="int?" Value=0 Min="-50" Max="50"></SfNumericTextBox>

<!-- Price between 0 and 9999.99 -->
<SfNumericTextBox TValue="decimal?" Value=0 Min=0 Max="9999.99"></SfNumericTextBox>
```

### Range with Step

Combine `Min`, `Max`, and `Step` for better control:

```razor
<!-- Price input: $0 to $1000, increment by $5 -->
<SfNumericTextBox TValue="decimal?" Value=0 Min=0 Max="1000" Step="5"></SfNumericTextBox>

<!-- Score 0-100, increment by 10 -->
<SfNumericTextBox TValue="int?" Value=50 Min=0 Max=100 Step="10"></SfNumericTextBox>

<!-- Volume 0-100%, increment by 5 -->
<SfNumericTextBox TValue="int?" Value=50 Min=0 Max=100 Step="5"></SfNumericTextBox>
```

### Common Range Patterns

```razor
<!-- Age: 0-150 -->
<SfNumericTextBox TValue="int?" Min=0 Max=150 Placeholder="Enter age"></SfNumericTextBox>

<!-- Rating: 1-5 -->
<SfNumericTextBox TValue="int?" Min=1 Max=5 Placeholder="Rate 1-5"></SfNumericTextBox>

<!-- Percentage: 0-100 -->
<SfNumericTextBox TValue="int?" Min=0 Max=100 Placeholder="0-100%"></SfNumericTextBox>

<!-- Discount: 0-50 -->
<SfNumericTextBox TValue="decimal?" Min=0 Max=50 Placeholder="Discount amount"></SfNumericTextBox>
```

### Validation Behavior

- **Below Min:** Automatically adjusted to Min value on blur
- **Above Max:** Automatically adjusted to Max value on blur
- **Step enforcement:** Value snaps to nearest valid step

```razor
<!-- Example: If user enters 108 (Max=100), it becomes 100 on blur -->
<SfNumericTextBox TValue="int?" Min=0 Max=100></SfNumericTextBox>
```

## Number Formatting

The `Format` property controls how numbers display when the component loses focus.

### Why Formatting Matters

- **Currency:** $1,234.56 (not 1234.56)
- **Thousands separator:** 1,000,000 (not 1000000)
- **Decimals:** 10.50 (not 10.5)
- **Percentage:** 12.34% (not 0.1234)

### Basic Formatting

```razor
<!-- No special formatting -->
<SfNumericTextBox TValue="int?" Value=1000></SfNumericTextBox>
<!-- Displays: 1000 -->

<!-- Currency format (2 decimals) -->
<SfNumericTextBox TValue="decimal?" Value="1000.50" Format="c2"></SfNumericTextBox>
<!-- Displays: $1,000.50 -->

<!-- Number format (3 decimals) -->
<SfNumericTextBox TValue="double?" Value="1234.567" Format="n3"></SfNumericTextBox>
<!-- Displays: 1,234.567 -->

<!-- Percentage format -->
<SfNumericTextBox TValue="double?" Value="0.1234" Format="p2"></SfNumericTextBox>
<!-- Displays: 12.34% -->
```

## Format Strings

### Standard Format Codes

| Code | Format | Example Input | Display |
|------|--------|---------------|---------|
| `c` or `c2` | Currency | 1234.5 | $1,234.50 |
| `n` or `n2` | Number | 1234.5 | 1,234.50 |
| `p` or `p2` | Percentage | 0.5 | 50.00% |
| `f` or `f2` | Fixed-point | 1234.5 | 1234.50 |

### Decimal Precision

Modify the number after format code for different decimal places:

```razor
<!-- c0: No decimals -->
<SfNumericTextBox TValue="decimal?" Value="100.99" Format="c0"></SfNumericTextBox>
<!-- Displays: $101 -->

<!-- c2: Two decimals (default) -->
<SfNumericTextBox TValue="decimal?" Value="100.5" Format="c2"></SfNumericTextBox>
<!-- Displays: $100.50 -->

<!-- c3: Three decimals -->
<SfNumericTextBox TValue="decimal?" Value="100.999" Format="c3"></SfNumericTextBox>
<!-- Displays: $100.999 -->

<!-- n1: One decimal -->
<SfNumericTextBox TValue="double?" Value="1234.567" Format="n1"></SfNumericTextBox>
<!-- Displays: 1,234.6 -->
```

## Currency Formatting

Currency format is essential for monetary values.

### Currency Example

```razor
@page "/currency-input"

<h3>Product Price Input</h3>

<!-- US Dollar format -->
<SfNumericTextBox TValue="decimal?" 
    Value="99.99" 
    Format="c2" 
    Min="0"
    Max="9999.99"
    Decimals="2"
    ValidateDecimalOnType="true"
    Placeholder="Enter price">
</SfNumericTextBox>

<!-- Display formatted price -->
<p>Price: @price.ToString("C2")</p>

@code {
    private decimal? price = 99.99m;
}
```

### Best Practices for Currency

```razor
<!-- Always use decimal? for currency -->
<SfNumericTextBox TValue="decimal?" Format="c2"></SfNumericTextBox>

<!-- Always set Min=0 for prices -->
<SfNumericTextBox TValue="decimal?" Format="c2" Min="0"></SfNumericTextBox>

<!-- Always set Decimals=2 for currency -->
<SfNumericTextBox TValue="decimal?" Format="c2" Decimals="2"></SfNumericTextBox>

<!-- Use ValidateDecimalOnType to prevent invalid decimals while typing -->
<SfNumericTextBox TValue="decimal?" 
    Format="c2" 
    Decimals="2" 
    ValidateDecimalOnType="true">
</SfNumericTextBox>
```

## Percentage Formatting

Percentage format is useful for rates, ratios, and proportions.

### Percentage Example

```razor
@page "/percentage-input"

<h3>Discount Percentage</h3>

<!-- Percentage format 0-100 -->
<SfNumericTextBox TValue="double?" 
    Value="10" 
    Format="p2" 
    Min="0"
    Max="100"
    Placeholder="Enter discount %">
</SfNumericTextBox>

<!-- Percentage format 0-1 (decimal) -->
<SfNumericTextBox TValue="double?" 
    Value="0.1" 
    Format="p2" 
    Min="0"
    Max="1"
    Placeholder="Enter as decimal">
</SfNumericTextBox>
```

**Important:** Percentage values typically stored as 0-1 (0.5 = 50%), but display as 0-100 using format.

```razor
<!-- Store: 0.25 (25%) -->
<!-- Display with Format="p2": 25.00% -->
<SfNumericTextBox TValue="double?" Value="0.25" Format="p2"></SfNumericTextBox>
```

## Custom Format Examples

### Complex Business Logic

```razor
<!-- Tax rate 0-100% with 1 decimal -->
<SfNumericTextBox TValue="double?" 
    Format="p1" 
    Min="0" 
    Max="100"
    Placeholder="Tax rate %">
</SfNumericTextBox>

<!-- Stock price with 2 decimals -->
<SfNumericTextBox TValue="decimal?" 
    Format="n2" 
    Min="0" 
    Max="100000"
    Decimals="2">
</SfNumericTextBox>

<!-- Weight in kg with 3 decimals -->
<SfNumericTextBox TValue="double?" 
    Format="n3" 
    Min="0" 
    Max="1000"
    Decimals="3"
    Placeholder="Weight (kg)">
</SfNumericTextBox>

<!-- Interest rate 0-30% with 2 decimals -->
<SfNumericTextBox TValue="double?" 
    Format="p2" 
    Min="0" 
    Max="30"
    Placeholder="Interest %">
</SfNumericTextBox>
```

## Best Practices

### 1. Match TValue and Format

```razor
<!-- ✅ Correct: decimal with currency -->
<SfNumericTextBox TValue="decimal?" Format="c2"></SfNumericTextBox>

<!-- ✅ Correct: double with percentage -->
<SfNumericTextBox TValue="double?" Format="p2"></SfNumericTextBox>

<!-- ❌ Avoid: int with currency (may lose precision) -->
<SfNumericTextBox TValue="int?" Format="c2"></SfNumericTextBox>
```

### 2. Set Decimals When Using Decimal Format

```razor
<!-- ✅ Correct: Decimals matches format -->
<SfNumericTextBox TValue="decimal?" 
    Format="c2" 
    Decimals="2"
    ValidateDecimalOnType="true">
</SfNumericTextBox>

<!-- ❌ Avoid: Decimals doesn't match format -->
<SfNumericTextBox TValue="decimal?" 
    Format="c2" 
    Decimals="3">
</SfNumericTextBox>
```

### 3. Use Min/Max with Step

```razor
<!-- ✅ Good: Range with step increment -->
<SfNumericTextBox TValue="int?" 
    Min="0" 
    Max="100" 
    Step="5">
</SfNumericTextBox>

<!-- ✅ Good: Currency with realistic bounds -->
<SfNumericTextBox TValue="decimal?" 
    Format="c2" 
    Min="0" 
    Max="999999.99"
    Decimals="2">
</SfNumericTextBox>
```

### 4. Display Format vs Stored Value

Remember that Format only affects display, not the actual value:

```razor
<!-- Displays: $1,234.50 -->
<!-- Actual value: 1234.5 -->
<SfNumericTextBox TValue="decimal?" Value="1234.5" Format="c2"></SfNumericTextBox>
```

## Next Steps

- See [precision-and-step-values.md](precision-and-step-values.md) for decimal control
- See [data-binding-and-events.md](data-binding-and-events.md) for form integration
- See [customization-styling.md](customization-styling.md) for appearance customization
