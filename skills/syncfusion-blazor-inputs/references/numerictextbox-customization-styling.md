# Customization and Styling

## Table of Contents
- [Placeholder and Labels](#placeholder-and-labels)
- [UI Appearance](#ui-appearance)
- [Spin Button Styling](#spin-button-styling)
- [CSS Class Customization](#css-class-customization)
- [Disabled State Styling](#disabled-state-styling)
- [Mouse Wheel Behavior](#mouse-wheel-behavior)
- [Theme Integration](#theme-integration)
- [Advanced Styling Examples](#advanced-styling-examples)

## Placeholder and Labels

Placeholders provide user hints for empty inputs, while labels provide context.

### Placeholder Property

```razor
<!-- Simple placeholder -->
<SfNumericTextBox TValue="int?" Placeholder="Enter quantity"></SfNumericTextBox>

<!-- Placeholder with format hint -->
<SfNumericTextBox TValue="decimal?" Format="c2" Placeholder="Enter price (USD)"></SfNumericTextBox>

<!-- Placeholder with range hint -->
<SfNumericTextBox TValue="int?" Min=0 Max=100 Placeholder="Enter value 0-100"></SfNumericTextBox>
```

**When placeholder appears:**
- When input is empty and unfocused
- Disappears when user starts typing
- Reappears if input is cleared

### FloatLabelType Property

Float labels animate up when input is focused or contains value.

```razor
<!-- No float label (default) -->
<SfNumericTextBox TValue="int?" 
    Placeholder="Enter quantity">
</SfNumericTextBox>

<!-- Float label on auto -->
<SfNumericTextBox TValue="int?" 
    Placeholder="Enter quantity"
    FloatLabelType="@FloatLabelType.Auto">
</SfNumericTextBox>

<!-- Always float (label always visible) -->
<SfNumericTextBox TValue="int?" 
    Placeholder="Enter quantity"
    FloatLabelType="@FloatLabelType.Always">
</SfNumericTextBox>
```

### Label with Input

```razor
@page "/labeled-input"

<div class="form-group">
    <label for="quantity">Order Quantity:</label>
    <SfNumericTextBox 
        TValue="int?" 
        ID="quantity"
        Min=1
        Max=1000
        Placeholder="Enter quantity"
        FloatLabelType="@FloatLabelType.Auto">
    </SfNumericTextBox>
</div>

<div class="form-group">
    <label for="price">Product Price:</label>
    <SfNumericTextBox 
        TValue="decimal?" 
        ID="price"
        Format="c2"
        Placeholder="Enter price"
        FloatLabelType="@FloatLabelType.Auto">
    </SfNumericTextBox>
</div>

<style>
    .form-group {
        margin-bottom: 15px;
    }
    
    label {
        display: block;
        margin-bottom: 5px;
        font-weight: 600;
    }
</style>
```

## UI Appearance

Control the visual appearance of the component.

### Basic Properties

```razor
<!-- Readonly input -->
<SfNumericTextBox TValue="int?" Value=42 Readonly=true></SfNumericTextBox>

<!-- Disabled input -->
<SfNumericTextBox TValue="int?" Value=42 Enabled=false></SfNumericTextBox>

<!-- With width -->
<SfNumericTextBox TValue="int?" Width="200px"></SfNumericTextBox>

<!-- Full width -->
<SfNumericTextBox TValue="int?" Width="100%"></SfNumericTextBox>
```

### Disabled State

```razor
@page "/disabled-example"

<h3>Disabled States</h3>

<!-- Conditionally disabled -->
<SfNumericTextBox TValue="int?" 
    Value=10
    Enabled="@isEnabled">
</SfNumericTextBox>

<button @onclick="@(() => isEnabled = !isEnabled)">
    Toggle Enabled
</button>

@code {
    private bool isEnabled = true;
}
```

### Readonly vs Disabled

| Property | Readonly | Disabled |
|----------|----------|----------|
| Shows value | ✅ Yes | ✅ Yes |
| User can select text | ✅ Yes | ❌ No |
| Form submission | ✅ Included | ❌ Excluded |
| Tab navigation | ✅ Yes | ❌ No |
| Appears grayed out | ❌ No | ✅ Yes |

```razor
<!-- Use Readonly for display-only values -->
<SfNumericTextBox TValue="decimal?" Value=999.99 Readonly=true Format="c2"></SfNumericTextBox>

<!-- Use Disabled when field should not be submitted -->
<SfNumericTextBox TValue="int?" Value=0 Enabled=false></SfNumericTextBox>
```

## Spin Button Styling

Customize the appearance and behavior of spin buttons (up/down arrows).

### Hide Spin Buttons with CSS

```razor
<style>
    .hide-spin-buttons .e-input-group .e-spin-button {
        display: none;
    }
</style>

<SfNumericTextBox TValue="int?" 
    Value=10
    CssClass="hide-spin-buttons">
</SfNumericTextBox>
```

### Custom Spin Button Styling

```razor
<style>
    /* Style spin buttons */
    .custom-spin .e-input-group .e-spin-button {
        width: 30px;
        background-color: #f0f0f0;
    }
    
    /* Style up button */
    .custom-spin .e-input-group .e-spin-up {
        color: #007bff;
    }
    
    /* Style down button */
    .custom-spin .e-input-group .e-spin-down {
        color: #dc3545;
    }
</style>

<SfNumericTextBox TValue="int?" 
    Value=10
    CssClass="custom-spin">
</SfNumericTextBox>
```

### Spin Button Positioning

```razor
<!-- Default positioning (right side) -->
<SfNumericTextBox TValue="int?" Value=10></SfNumericTextBox>

<!-- Using CSS to position on left -->
<style>
    .spin-left .e-input-group {
        flex-direction: row-reverse;
    }
</style>

<SfNumericTextBox TValue="int?" 
    Value=10
    CssClass="spin-left">
</SfNumericTextBox>
```

## CSS Class Customization

Use the `CssClass` property to apply custom styling.

### Basic CSS Class Usage

```razor
<!-- Apply custom CSS class -->
<SfNumericTextBox TValue="int?" 
    Value=10
    CssClass="my-custom-input">
</SfNumericTextBox>

<style>
    .my-custom-input .e-input {
        font-size: 16px;
        padding: 10px;
        border-radius: 5px;
    }
</style>
```

### Multiple CSS Classes

```razor
<SfNumericTextBox TValue="decimal?" 
    Format="c2"
    CssClass="currency-input large-text primary-color">
</SfNumericTextBox>

<style>
    .currency-input .e-input {
        font-family: 'Courier New', monospace;
    }
    
    .large-text .e-input {
        font-size: 18px;
    }
    
    .primary-color .e-input {
        color: #007bff;
    }
</style>
```

### Context-Based Styling

```razor
@page "/custom-styling"

<div>
    <label>Success State:</label>
    <SfNumericTextBox TValue="int?" Value=50 CssClass="success-state"></SfNumericTextBox>
</div>

<div>
    <label>Warning State:</label>
    <SfNumericTextBox TValue="int?" Value=80 CssClass="warning-state"></SfNumericTextBox>
</div>

<div>
    <label>Error State:</label>
    <SfNumericTextBox TValue="int?" Value=0 CssClass="error-state"></SfNumericTextBox>
</div>

<style>
    .success-state .e-input {
        border: 2px solid #28a745;
        background-color: #f0fff4;
    }
    
    .warning-state .e-input {
        border: 2px solid #ffc107;
        background-color: #fffbf0;
    }
    
    .error-state .e-input {
        border: 2px solid #dc3545;
        background-color: #fff5f5;
    }
</style>
```

## Disabled State Styling

Customize how disabled inputs appear.

### Default Disabled Styling

```razor
<!-- Default disabled appearance -->
<SfNumericTextBox TValue="int?" Value=42 Enabled=false></SfNumericTextBox>
<!-- Appears grayed out automatically -->
```

### Custom Disabled Styling

```razor
<style>
    .custom-disabled:disabled,
    .e-numerictextbox.e-disabled .e-input {
        background-color: #f9f9f9;
        cursor: not-allowed;
        opacity: 0.6;
    }
</style>

<SfNumericTextBox TValue="int?" 
    Value=42 
    Enabled=false
    CssClass="custom-disabled">
</SfNumericTextBox>
```

### Conditional Styling with State

```razor
@page "/conditional-disable"

<SfNumericTextBox TValue="int?" 
    Value="@quantity"
    Enabled="@isAvailable"
    CssClass="@(isAvailable ? 'available' : 'unavailable')">
</SfNumericTextBox>

<button @onclick="@(() => isAvailable = !isAvailable)">
    Toggle Availability
</button>

<style>
    .available .e-input {
        border: 1px solid #28a745;
        background-color: #f0fff4;
    }
    
    .unavailable .e-input {
        border: 1px solid #dee2e6;
        background-color: #f9f9f9;
        opacity: 0.5;
    }
</style>

@code {
    private int? quantity = 10;
    private bool isAvailable = true;
}
```

## Mouse Wheel Behavior

Control whether mouse wheel changes the value.

### Default Behavior

By default, mouse wheel scrolling over the input changes the value. This can be disabled with CSS or JavaScript.

### Disable Mouse Wheel with CSS

```razor
<style>
    /* Prevent mouse wheel value changes -->
    .no-scroll-increment {
        pointer-events: none;
        user-select: none;
    }
    
    .no-scroll-increment .e-input {
        pointer-events: auto;
    }
</style>

<SfNumericTextBox TValue="int?" 
    Value=10
    CssClass="no-scroll-increment">
</SfNumericTextBox>
```

### Disable Mouse Wheel with JavaScript

```razor
@page "/mouse-wheel-control"

<SfNumericTextBox TValue="int?" 
    Value=10
    @ref="textBoxRef">
</SfNumericTextBox>

<button @onclick="DisableMouseWheel">Disable Scroll</button>
<button @onclick="EnableMouseWheel">Enable Scroll</button>

@code {
    private SfNumericTextBox<int?> textBoxRef;
    
    private void DisableMouseWheel()
    {
        // Using JavaScript interop
        // Implementation depends on Syncfusion version
    }
    
    private void EnableMouseWheel()
    {
        // Using JavaScript interop
        // Implementation depends on Syncfusion version
    }
}
```

## Theme Integration

Apply Syncfusion themes and customize with theme variables.

### Supported Themes

```html
<!-- Bootstrap 5 -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Fluent -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />

<!-- Material -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Tailwind -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />
```

### Theme-Specific Styling

```razor
<style>
    /* Bootstrap 5 theme adjustments -->
    .e-numerictextbox.e-bootstrap .e-input {
        border-radius: 0.25rem;
    }
    
    /* Material theme adjustments */
    .e-numerictextbox.e-material .e-input {
        border-radius: 4px;
    }
    
    /* Fluent theme adjustments -->
    .e-numerictextbox.e-fluent .e-input {
        border-radius: 2px;
    }
</style>
```

## Advanced Styling Examples

### Example 1: Currency Input with Icon

```razor
@page "/currency-styled"

<style>
    .currency-input-wrapper {
        position: relative;
        display: inline-block;
        width: 100%;
    }
    
    .currency-icon {
        position: absolute;
        left: 10px;
        top: 50%;
        transform: translateY(-50%);
        color: #28a745;
        font-weight: bold;
    }
    
    .currency-input .e-input {
        padding-left: 30px;
    }
</style>

<div class="currency-input-wrapper">
    <span class="currency-icon">$</span>
    <SfNumericTextBox TValue="decimal?" 
        Format="c2"
        Min=0
        Decimals=2
        Placeholder="0.00"
        CssClass="currency-input">
    </SfNumericTextBox>
</div>
```

### Example 2: Color-Coded Input

```razor
@page "/color-coded"

<SfNumericTextBox TValue="int?" 
    Value="@score"
    Min=0
    Max=100
    @bind-Value="score"
    CssClass="@GetScoreClass()">
</SfNumericTextBox>

<style>
    .score-excellent .e-input {
        border: 2px solid #28a745;
        background-color: #f0fff4;
        color: #155724;
    }
    
    .score-good .e-input {
        border: 2px solid #17a2b8;
        background-color: #f0f8fb;
        color: #0c5460;
    }
    
    .score-fair .e-input {
        border: 2px solid #ffc107;
        background-color: #fffbf0;
        color: #856404;
    }
    
    .score-poor .e-input {
        border: 2px solid #dc3545;
        background-color: #fff5f5;
        color: #721c24;
    }
</style>

@code {
    private int? score = 50;
    
    private string GetScoreClass()
    {
        return score switch
        {
            >= 80 => "score-excellent",
            >= 60 => "score-good",
            >= 40 => "score-fair",
            _ => "score-poor"
        };
    }
}
```

### Example 3: Compact vs Expanded Layout

```razor
@page "/layout-modes"

<h3>Compact Mode</h3>
<SfNumericTextBox TValue="int?" 
    Value=10
    CssClass="compact-input">
</SfNumericTextBox>

<h3>Expanded Mode</h3>
<SfNumericTextBox TValue="int?" 
    Value=10
    CssClass="expanded-input">
</SfNumericTextBox>

<style>
    .compact-input .e-input {
        padding: 4px 8px;
        font-size: 12px;
        height: 28px;
    }
    
    .expanded-input .e-input {
        padding: 12px 16px;
        font-size: 16px;
        height: 48px;
    }
</style>
```

### Example 4: Responsive Input

```razor
@page "/responsive"

<SfNumericTextBox TValue="int?" 
    Value=10
    CssClass="responsive-input">
</SfNumericTextBox>

<style>
    /* Mobile */
    @media (max-width: 576px) {
        .responsive-input .e-input {
            font-size: 16px;
            padding: 10px 12px;
            height: 44px;
        }
    }
    
    /* Tablet */
    @media (min-width: 576px) and (max-width: 1024px) {
        .responsive-input .e-input {
            font-size: 14px;
            padding: 8px 10px;
            height: 36px;
        }
    }
    
    /* Desktop */
    @media (min-width: 1024px) {
        .responsive-input .e-input {
            font-size: 13px;
            padding: 6px 8px;
            height: 32px;
        }
    }
</style>
```

## Best Practices

1. **Use semantic classes:** `success-input`, `error-input` instead of `blue-input`
2. **Keep styling simple:** Avoid deeply nested CSS selectors
3. **Mobile-first:** Start with mobile styles, add complexity for larger screens
4. **Consistent spacing:** Align with design system margins and padding
5. **Accessibility:** Ensure disabled state is visually distinct and semantic

## Next Steps

- See [globalization-accessibility.md](globalization-accessibility.md) for accessibility features
- See [advanced-features.md](advanced-features.md) for complex scenarios
- See [range-validation-and-formatting.md](range-validation-and-formatting.md) for validation styling
