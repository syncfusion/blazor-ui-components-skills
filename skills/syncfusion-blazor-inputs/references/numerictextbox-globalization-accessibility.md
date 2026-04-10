# Globalization and Accessibility

## Table of Contents
- [Globalization Overview](#globalization-overview)
- [Number Formatting by Culture](#number-formatting-by-culture)
- [RTL Support](#rtl-support)
- [Accessibility Features](#accessibility-features)
- [ARIA Attributes](#aria-attributes)
- [Keyboard Navigation](#keyboard-navigation)
- [Screen Reader Support](#screen-reader-support)
- [WCAG Compliance](#wcag-compliance)

## Globalization Overview

Globalization enables NumericTextBox to support multiple languages and cultural formats. This includes number formatting, currency symbols, and language-specific input handling.

### Why Globalization Matters

- **User expectations:** Users expect numbers formatted according to their locale
- **Data accuracy:** Different regions use different separators (1,000.00 vs 1.000,00)
- **Currency display:** Symbols vary by region ($, €, £, ¥, etc.)
- **Market reach:** Essential for international applications



## Number Formatting by Culture

Different cultures format numbers differently. The Format property respects CultureInfo.CurrentCulture.

### US Format (en-US)

```razor
<!-- US: 1,234.56 -->
<SfNumericTextBox TValue="decimal?" 
    Value="1234.56" 
    Format="c2">
</SfNumericTextBox>
<!-- Currency: $1,234.56 -->
```

### German Format (de-DE)

```razor
<!-- German: 1.234,56 -->
<SfNumericTextBox TValue="decimal?" 
    Value="1234.56" 
    Format="c2">
</SfNumericTextBox>
<!-- Currency: 1.234,56 € -->
```

### French Format (fr-FR)

```razor
<!-- French: 1 234,56 -->
<SfNumericTextBox TValue="decimal?" 
    Value="1234.56" 
    Format="c2">
</SfNumericTextBox>
<!-- Currency: 1 234,56 € -->
```

### Culture-Aware Component

```razor
@page "/culture-input"
@using System.Globalization

<h3>Culture-Aware NumericTextBox</h3>

<div>
    <label>Select Culture:</label>
    <select @onchange="ChangeCulture">
        <option value="en-US">US (1,234.56)</option>
        <option value="de-DE">German (1.234,56)</option>
        <option value="fr-FR">French (1 234,56)</option>
        <option value="ja-JP">Japanese (1,234.56)</option>
    </select>
</div>

<SfNumericTextBox TValue="decimal?" 
    Value="1234.56"
    Format="c2"
    Placeholder="Culture-aware number">
</SfNumericTextBox>

<p>Current Culture: @CultureInfo.CurrentCulture.DisplayName</p>

@code {
    private void ChangeCulture(ChangeEventArgs e)
    {
        string cultureName = e.Value?.ToString() ?? "en-US";
        CultureInfo.CurrentCulture = new CultureInfo(cultureName);
        CultureInfo.CurrentUICulture = new CultureInfo(cultureName);
    }
}
```

## RTL Support

RTL (Right-to-Left) support is essential for Arabic, Hebrew, Persian, and Urdu languages.

### Enabling RTL

```html
<!-- Add to HTML to enable RTL globally -->
<html dir="rtl" lang="ar">
<head>
    ....
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5-rtl.css" rel="stylesheet" />
</head>
</html>
```

### RTL-Specific Styles

```razor
@page "/rtl-example"

<h3>RTL Example (Arabic)</h3>

<SfNumericTextBox TValue="decimal?" 
    Value="1234.56"
    Format="c2"
    Placeholder="أدخل الرقم">
</SfNumericTextBox>

<style>
    /* RTL adjustments -->
    [dir="rtl"] .e-numerictextbox .e-input {
        text-align: right;
        direction: rtl;
    }
</style>
```

### Component-Level RTL

```razor
<!-- Apply RTL to specific component -->
<div dir="rtl">
    <SfNumericTextBox TValue="int?" 
        Value=10
        Placeholder="أدخل كمية">
    </SfNumericTextBox>
</div>
```

## Accessibility Features

NumericTextBox includes built-in accessibility features for WCAG compliance.

### Semantic HTML

The component uses semantic HTML elements:
- `<input type="number">` for numeric input
- Proper `<label>` associations
- ARIA landmarks and descriptions

```razor
<label for="quantity">Order Quantity:</label>
<SfNumericTextBox TValue="int?" 
    ID="quantity"
    Min=1
    Max=1000
    Placeholder="Enter quantity">
</SfNumericTextBox>
```

### Focus Management

```razor
@page "/focus-example"

<SfNumericTextBox TValue="int?" 
    @ref="numericInput"
    Value=10>
</SfNumericTextBox>

<button @onclick="FocusInput">Focus Input</button>

@code {
    private SfNumericTextBox<int?> numericInput;
    
    private async Task FocusInput()
    {
        // Set focus to input
        await numericInput.FocusAsync();
    }
}
```

### Error Messages and Validation

```razor
@page "/accessible-validation"

<EditForm Model="@model" OnValidSubmit="@HandleSubmit">
    <DataAnnotationsValidator></DataAnnotationsValidator>
    
    <div class="form-group">
        <label for="qty">Quantity:</label>
        <SfNumericTextBox 
            TValue="int?"
            ID="qty"
            @bind-Value="model.Quantity"
            aria-describedby="qty-error">
        </SfNumericTextBox>
        <ValidationMessage 
            For="@(() => model.Quantity)" 
            id="qty-error">
        </ValidationMessage>
    </div>
    
    <button type="submit">Submit</button>
</EditForm>

@code {
    private QuantityModel model = new();
    
    private async Task HandleSubmit()
    {
        // Submit logic
    }
    
    public class QuantityModel
    {
        [Range(1, 1000, ErrorMessage = "Quantity must be between 1 and 1000")]
        public int? Quantity { get; set; }
    }
}
```

## ARIA Attributes

ARIA attributes provide screen reader information about the component.

### Common ARIA Attributes

```razor
<!-- Label association -->
<SfNumericTextBox TValue="int?" 
    ID="quantity"
    aria-label="Order quantity in units">
</SfNumericTextBox>

<!-- Description for complex input -->
<SfNumericTextBox TValue="decimal?" 
    Format="c2"
    aria-describedby="price-help">
</SfNumericTextBox>
<small id="price-help">Enter price in US dollars</small>

<!-- Read-only indication -->
<SfNumericTextBox TValue="int?" 
    Value=42
    Readonly=true
    aria-readonly="true">
</SfNumericTextBox>

<!-- Invalid state indication -->
<SfNumericTextBox TValue="int?" 
    Value=0
    aria-invalid="true"
    aria-errormessage="error-msg">
</SfNumericTextBox>
<span id="error-msg">This field is required</span>
```

### Complete ARIA Example

```razor
@page "/aria-example"

<div class="form-group">
    <label for="price">Product Price (USD):</label>
    <SfNumericTextBox 
        TValue="decimal?"
        ID="price"
        Format="c2"
        Min=0.01m
        Decimals=2
        aria-label="Product price in US dollars"
        aria-describedby="price-help"
        aria-required="true">
    </SfNumericTextBox>
    <small id="price-help">Required field. Enter price in US dollars with up to 2 decimal places.</small>
</div>

<button type="submit" aria-label="Submit price information">Submit</button>
```

## Keyboard Navigation

Users can navigate and control the input using keyboard.

### Default Keyboard Support

| Key | Action |
|-----|--------|
| Tab | Focus input |
| Shift+Tab | Previous field |
| Up Arrow | Increment value by step |
| Down Arrow | Decrement value by step |
| Shift+Up | Increment by 10x step |
| Shift+Down | Decrement by 10x step |
| Home | Go to minimum value |
| End | Go to maximum value |

### Keyboard Example

```razor
@page "/keyboard-navigation"

<h3>Keyboard Controls Demo</h3>
<p>Tab to focus, use arrow keys to adjust value</p>

<div class="form-group">
    <label for="quantity">Quantity (1-100):</label>
    <SfNumericTextBox 
        TValue="int?"
        ID="quantity"
        Value=1
        Min=1
        Max=100
        Step=1
        Placeholder="Use arrow keys">
    </SfNumericTextBox>
</div>

<div class="form-group">
    <label for="price">Price (USD):</label>
    <SfNumericTextBox 
        TValue="decimal?"
        ID="price"
        Value=0
        Min=0
        Max="9999.99"
        Step="0.01"
        Format="c2"
        Placeholder="Use arrow keys">
    </SfNumericTextBox>
</div>

<p>Instructions:</p>
<ul>
    <li>Tab: Move between fields</li>
    <li>Up/Down Arrows: Adjust value</li>
    <li>Shift+Up/Down: Larger adjustments</li>
</ul>
```

## Screen Reader Support

Screen readers announce the label, value, and validation messages.

### Screen Reader Compatible Component

```razor
@page "/screen-reader-example"

<h3>Accessible Form</h3>

<form>
    <!-- Label explicitly associated -->
    <label for="order-qty">Order Quantity:</label>
    <SfNumericTextBox 
        TValue="int?"
        ID="order-qty"
        Min=1
        Max=1000
        aria-required="true"
        aria-describedby="qty-hint">
    </SfNumericTextBox>
    <span id="qty-hint">Required. Enter quantity between 1 and 1000</span>
    
    <hr />
    
    <!-- Currency input -->
    <label for="item-price">Item Price (USD):</label>
    <SfNumericTextBox 
        TValue="decimal?"
        ID="item-price"
        Format="c2"
        Min=0
        Decimals=2
        aria-required="true"
        aria-describedby="price-hint">
    </SfNumericTextBox>
    <span id="price-hint">Enter price in US dollars. Example: $19.99</span>
    
    <hr />
    
    <!-- Percentage -->
    <label for="discount-pct">Discount Percentage:</label>
    <SfNumericTextBox 
        TValue="double?"
        ID="discount-pct"
        Format="p2"
        Min=0
        Max=100
        aria-describedby="discount-hint">
    </SfNumericTextBox>
    <span id="discount-hint">Enter discount as percentage. Example: 15% off</span>
    
    <button type="submit">Submit Order</button>
</form>

<style>
    form div {
        margin-bottom: 20px;
    }
    
    label {
        display: block;
        margin-bottom: 5px;
        font-weight: 600;
    }
    
    span {
        display: block;
        font-size: 0.875rem;
        color: #666;
        margin-top: 5px;
    }
</style>
```

## WCAG Compliance

NumericTextBox meets WCAG 2.1 Level AA standards.

### WCAG Checklist

- ✅ **Perceivable:** Label visible, error messages clear
- ✅ **Operable:** Keyboard accessible, sufficient target size (44px minimum)
- ✅ **Understandable:** Clear labels, helpful error messages
- ✅ **Robust:** Semantic HTML, ARIA support

### Compliant Example

```razor
@page "/wcag-compliant"

<style>
    /* Minimum target size: 44x44px -->
    .e-numerictextbox .e-input {
        min-height: 44px;
        min-width: 44px;
        padding: 10px;
    }
    
    /* Color contrast: 4.5:1 for normal text -->
    .e-numerictextbox .e-input {
        color: #212529;
        background-color: #ffffff;
    }
    
    /* Focus indicator: visible and distinct -->
    .e-numerictextbox .e-input:focus {
        outline: 3px solid #0d6efd;
        outline-offset: 2px;
    }
</style>

<div role="region" aria-labelledby="form-title">
    <h2 id="form-title">Order Form</h2>
    
    <form>
        <div class="form-group">
            <label for="qty" id="qty-label">Quantity:</label>
            <SfNumericTextBox 
                TValue="int?"
                ID="qty"
                Min=1
                aria-labelledby="qty-label"
                aria-required="true">
            </SfNumericTextBox>
        </div>
        
        <button type="submit">Submit Order</button>
    </form>
</div>
```

## Best Practices

1. **Always use labels:** Associate labels with inputs using `<label for>`
2. **Provide help text:** Explain input requirements with aria-describedby
3. **Semantic HTML:** Use proper label and form elements
4. **Keyboard support:** Test with keyboard navigation
5. **Color contrast:** Ensure 4.5:1 contrast ratio for text
6. **Target size:** Minimum 44x44px for touch targets
7. **Error messages:** Clear, specific, linked to inputs
8. **Localization:** Test in multiple languages and regions

## Next Steps

- See [advanced-features.md](advanced-features.md) for complex scenarios
- See [customization-styling.md](customization-styling.md) for appearance options
- See [data-binding-and-events.md](data-binding-and-events.md) for form integration
