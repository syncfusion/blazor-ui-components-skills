# Advanced Features and Integration

## Table of Contents
- [Mobile Numeric Keyboard](#mobile-numeric-keyboard)
- [Complex Data Binding Scenarios](#complex-data-binding-scenarios)
- [Real-Time Validation Patterns](#real-time-validation-patterns)
- [Form Validation Integration](#form-validation-integration)
- [Performance Optimization](#performance-optimization)
- [Common Troubleshooting](#common-troubleshooting)
- [Edge Cases and Gotchas](#edge-cases-and-gotchas)

## Mobile Numeric Keyboard

NumericTextBox automatically triggers the native numeric keyboard on mobile devices.

### Mobile Keyboard Behavior

On mobile browsers, NumericTextBox displays the numeric keyboard when focused:
- **iOS:** Shows number pad
- **Android:** Shows numeric keyboard
- **Windows Phone:** Shows number pad

### Optimizing for Mobile

```razor
@page "/mobile-friendly"

<!-- Mobile-optimized numeric input -->
<SfNumericTextBox TValue="int?" 
    Value=1
    Min=1
    Max=100
    Step=5
    Placeholder="Enter quantity"
    FloatLabelType="@FloatLabelType.Auto">
</SfNumericTextBox>

<style>
    /* Larger touch targets on mobile -->
    @media (max-width: 768px) {
        .e-numerictextbox .e-input {
            min-height: 48px;
            font-size: 16px;
            padding: 12px;
        }
        
        .e-numerictextbox .e-spin-button {
            width: 44px;
        }
    }
</style>
```

### Mobile Form Example

```razor
@page "/mobile-order-form"

<h3>Mobile-Friendly Order</h3>

<EditForm Model="@order" OnValidSubmit="@HandleSubmit">
    <div class="mobile-form">
        <div class="form-group">
            <label for="qty">Quantity:</label>
            <SfNumericTextBox 
                TValue="int?"
                ID="qty"
                @bind-Value="order.Quantity"
                Min=1
                Max=999
                Placeholder="Enter qty">
            </SfNumericTextBox>
        </div>
        
        <div class="form-group">
            <label for="price">Price per Item:</label>
            <SfNumericTextBox 
                TValue="decimal?"
                ID="price"
                @bind-Value="order.PricePerItem"
                Format="c2"
                Min=0
                Decimals=2
                Placeholder="Enter price">
            </SfNumericTextBox>
        </div>
        
        <button type="submit" class="btn-submit">Place Order</button>
    </div>
</EditForm>

<style>
    .mobile-form {
        padding: 20px;
    }
    
    .form-group {
        margin-bottom: 20px;
    }
    
    label {
        display: block;
        margin-bottom: 8px;
        font-weight: 600;
    }
    
    .btn-submit {
        width: 100%;
        padding: 12px;
        font-size: 16px;
        touch-action: manipulation;
    }
    
    @media (max-width: 768px) {
        .e-numerictextbox .e-input {
            min-height: 48px;
            font-size: 16px;
        }
    }
</style>

@code {
    private OrderModel order = new();
    
    private async Task HandleSubmit()
    {
        // Process order
    }
    
    public class OrderModel
    {
        public int? Quantity { get; set; }
        public decimal? PricePerItem { get; set; }
    }
}
```

## Complex Data Binding Scenarios

Advanced binding patterns for complex application needs.

### Binding to Nested Properties

```razor
@page "/nested-binding"

<EditForm Model="@company">
    <!-- Bind to nested property -->
    <label>Employee Count:</label>
    <SfNumericTextBox 
        TValue="int?"
        @bind-Value="company.Department.EmployeeCount"
        Min=0
        Max=1000>
    </SfNumericTextBox>
    
    <!-- Bind to another nested property -->
    <label>Budget (USD):</label>
    <SfNumericTextBox 
        TValue="decimal?"
        @bind-Value="company.Department.Budget"
        Format="c2"
        Min=0>
    </SfNumericTextBox>
    
    <button type="submit">Save</button>
</EditForm>

@code {
    private CompanyModel company = new();
    
    public class CompanyModel
    {
        public DepartmentModel Department { get; set; } = new();
    }
    
    public class DepartmentModel
    {
        public int? EmployeeCount { get; set; }
        public decimal? Budget { get; set; }
    }
}
```

### Binding with Calculation

```razor
@page "/calculated-binding"

<!-- Input fields -->
<label>Quantity:</label>
<SfNumericTextBox TValue="int?" 
    @bind-Value="quantity">
</SfNumericTextBox>

<label>Unit Price:</label>
<SfNumericTextBox TValue="decimal?" 
    @bind-Value="unitPrice"
    Format="c2">
</SfNumericTextBox>

<label>Tax Rate %:</label>
<SfNumericTextBox TValue="double?" 
    @bind-Value="taxRate"
    Format="p2">
</SfNumericTextBox>

<!-- Display calculated total (read-only) -->
<label>Total with Tax:</label>
<SfNumericTextBox TValue="decimal?" 
    Value="@CalculateTotal()"
    Format="c2"
    Readonly=true>
</SfNumericTextBox>

@code {
    private int? quantity = 1;
    private decimal? unitPrice = 10;
    private double? taxRate = 0.1;
    
    private decimal CalculateTotal()
    {
        decimal subtotal = (quantity ?? 0) * (unitPrice ?? 0);
        decimal tax = subtotal * (decimal)(taxRate ?? 0);
        return subtotal + tax;
    }
}
```

### Binding to Dictionary

```razor
@page "/dictionary-binding"

<EditForm Model="@settings">
    <label>Max Retries:</label>
    <SfNumericTextBox TValue="int?" 
        Value="@GetSetting("maxRetries")"
        ValueChanged="@((ChangeEventArgs<int?> args) => SetSetting("maxRetries", args.Value))">
    </SfNumericTextBox>
    
    <label>Timeout (seconds):</label>
    <SfNumericTextBox TValue="int?" 
        Value="@GetSetting("timeout")"
        ValueChanged="@((ChangeEventArgs<int?> args) => SetSetting("timeout", args.Value))">
    </SfNumericTextBox>
    
    <button type="submit">Save Settings</button>
</EditForm>

@code {
    private Dictionary<string, int?> settings = new()
    {
        { "maxRetries", 3 },
        { "timeout", 30 }
    };
    
    private int? GetSetting(string key)
    {
        return settings.TryGetValue(key, out var value) ? value : null;
    }
    
    private void SetSetting(string key, int? value)
    {
        if (settings.ContainsKey(key))
            settings[key] = value;
        else
            settings.Add(key, value);
    }
}
```

## Real-Time Validation Patterns

Implement real-time validation with NumericTextBox.

### Real-Time Range Validation

```razor
@page "/real-time-validation"

<label>Age (18-120):</label>
<SfNumericTextBox TValue="int?" 
    Value="@age"
    ValueChanged="@OnAgeChanged"
    Min=18
    Max=120>
</SfNumericTextBox>

@if (!isAgeValid)
{
    <div class="alert alert-danger">
        Age must be between 18 and 120
    </div>
}
else if (age.HasValue)
{
    <div class="alert alert-success">
        ✓ Valid age: @age
    </div>
}

@code {
    private int? age;
    private bool isAgeValid = true;
    
    private void OnAgeChanged(ChangeEventArgs<int?> args)
    {
        age = args.Value;
        isAgeValid = age >= 18 && age <= 120;
    }
}
```

### Real-Time Dependency Validation

```razor
@page "/dependency-validation"

<label>Discount Amount:</label>
<SfNumericTextBox TValue="decimal?" 
    Value="@discountAmount"
    ValueChanged="@OnDiscountChanged"
    Format="c2"
    Min=0>
</SfNumericTextBox>

<label>Max Discount (10% of total):</label>
<SfNumericTextBox TValue="decimal?" 
    Value="@(totalAmount * 0.1m)"
    Readonly=true
    Format="c2">
</SfNumericTextBox>

@if (discountTooHigh)
{
    <div class="alert alert-warning">
        Discount exceeds 10% of total
    </div>
}

@code {
    private decimal? discountAmount = 0;
    private decimal totalAmount = 1000;
    private bool discountTooHigh = false;
    
    private void OnDiscountChanged(ChangeEventArgs<decimal?> args)
    {
        discountAmount = args.Value;
        decimal maxDiscount = totalAmount * 0.1m;
        discountTooHigh = (discountAmount ?? 0) > maxDiscount;
    }
}
```

## Form Validation Integration

Integrate with form validation frameworks.

### Data Annotations Validation

```razor
@page "/validation-form"

<EditForm Model="@product" OnValidSubmit="@HandleSubmit">
    <DataAnnotationsValidator></DataAnnotationsValidator>
    <ValidationSummary></ValidationSummary>
    
    <div class="form-group">
        <label for="stock">Stock Quantity:</label>
        <SfNumericTextBox 
            TValue="int?"
            ID="stock"
            @bind-Value="product.StockQuantity">
        </SfNumericTextBox>
        <ValidationMessage For="@(() => product.StockQuantity)"></ValidationMessage>
    </div>
    
    <div class="form-group">
        <label for="price">Selling Price:</label>
        <SfNumericTextBox 
            TValue="decimal?"
            ID="price"
            @bind-Value="product.SellingPrice"
            Format="c2">
        </SfNumericTextBox>
        <ValidationMessage For="@(() => product.SellingPrice)"></ValidationMessage>
    </div>
    
    <div class="form-group">
        <label for="cost">Cost Price:</label>
        <SfNumericTextBox 
            TValue="decimal?"
            ID="cost"
            @bind-Value="product.CostPrice"
            Format="c2">
        </SfNumericTextBox>
        <ValidationMessage For="@(() => product.CostPrice)"></ValidationMessage>
    </div>
    
    <button type="submit">Save Product</button>
</EditForm>

@code {
    private ProductModel product = new();
    
    private async Task HandleSubmit()
    {
        // Save product
        await Task.Delay(500);
    }
    
    public class ProductModel
    {
        [Range(0, 100000, ErrorMessage = "Stock must be 0-100000")]
        public int? StockQuantity { get; set; }
        
        [Range(0.01, 999999.99, ErrorMessage = "Price must be positive")]
        public decimal? SellingPrice { get; set; }
        
        [Range(0, 999999.99, ErrorMessage = "Cost must be non-negative")]
        public decimal? CostPrice { get; set; }
    }
}
```

### Custom Validation Rules

```razor
@page "/custom-validation"

<EditForm Model="@model" OnValidSubmit="@HandleSubmit">
    <ObjectGraphDataAnnotationsValidator></ObjectGraphDataAnnotationsValidator>
    
    <label>Purchase Price:</label>
    <SfNumericTextBox 
        TValue="decimal?"
        @bind-Value="model.PurchasePrice"
        Format="c2">
    </SfNumericTextBox>
    
    <label>Selling Price:</label>
    <SfNumericTextBox 
        TValue="decimal?"
        @bind-Value="model.SellingPrice"
        Format="c2">
    </SfNumericTextBox>
    
    <ValidationMessage For="@(() => model.SellingPrice)"></ValidationMessage>
    
    <button type="submit">Validate</button>
</EditForm>

@code {
    private PricingModel model = new();
    
    private async Task HandleSubmit()
    {
        // Form is valid, prices meet business rules
    }
    
    public class PricingModel
    {
        public decimal? PurchasePrice { get; set; }
        
        [CustomValidation(typeof(PricingModel), nameof(ValidateSellingPrice))]
        public decimal? SellingPrice { get; set; }
        
        public static ValidationResult? ValidateSellingPrice(decimal? value, ValidationContext context)
        {
            var model = (PricingModel)context.ObjectInstance;
            
            if (value <= model.PurchasePrice)
            {
                return new ValidationResult("Selling price must be higher than purchase price");
            }
            
            return ValidationResult.Success;
        }
    }
}
```

## Performance Optimization

Tips for optimizing NumericTextBox performance.

### Lazy Binding

```razor
@page "/lazy-binding"

<!-- Delay binding updates for performance -->
<SfNumericTextBox TValue="int?" 
    Value="@currentValue"
    @onchange="@OnValueChangedLazy">
</SfNumericTextBox>

<p>Updated value: @displayValue</p>

@code {
    private int? currentValue = 0;
    private int? displayValue = 0;
    private int? deferredValue = 0;
    private System.Timers.Timer? debounceTimer;
    
    private void OnValueChangedLazy(ChangeEventArgs e)
    {
        if (int.TryParse(e.Value?.ToString(), out int value))
        {
            currentValue = value;
            
            // Debounce updates
            debounceTimer?.Stop();
            debounceTimer = new System.Timers.Timer(500);
            debounceTimer.Elapsed += (s, e) =>
            {
                displayValue = currentValue;
                InvokeAsync(StateHasChanged);
            };
            debounceTimer.Start();
        }
    }
}
```

### Virtual Scrolling with Multiple Inputs

```razor
@page "/multiple-inputs"

<!-- For forms with many numeric inputs, virtual scrolling improves performance -->
<div class="form-container" style="height: 600px; overflow-y: auto;">
    @foreach (var item in items)
    {
        <div class="form-row">
            <label>Item @item.Id:</label>
            <SfNumericTextBox TValue="int?" 
                @bind-Value="item.Quantity"
                Min=0>
            </SfNumericTextBox>
        </div>
    }
</div>

@code {
    private List<Item> items = Enumerable.Range(1, 100)
        .Select(i => new Item { Id = i, Quantity = 0 })
        .ToList();
    
    public class Item
    {
        public int Id { get; set; }
        public int? Quantity { get; set; }
    }
}
```

## Common Troubleshooting

### Issue: Component not rendering

**Solution:** Check namespace imports
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Inputs
```

### Issue: Value changes not reflected

**Solution:** Use @bind-Value instead of Value
```razor
<!-- ❌ Wrong -->
<SfNumericTextBox TValue="int?" Value="@myValue"></SfNumericTextBox>

<!-- ✅ Correct -->
<SfNumericTextBox TValue="int?" @bind-Value="@myValue"></SfNumericTextBox>
```

### Issue: Decimal precision lost

**Solution:** Use decimal? instead of double?
```razor
<!-- ❌ Wrong for currency -->
<SfNumericTextBox TValue="double?" Format="c2"></SfNumericTextBox>

<!-- ✅ Correct -->
<SfNumericTextBox TValue="decimal?" Format="c2"></SfNumericTextBox>
```

### Issue: Mobile keyboard not showing

**Solution:** Ensure TValue is numeric
```razor
<!-- Check that TValue is int?, decimal?, or double? -->
<SfNumericTextBox TValue="int?"></SfNumericTextBox>
```

### Issue: Format not applied

**Solution:** Ensure value displayed after blur
```razor
<!-- Format applies on blur, not during editing -->
<!-- Input: 1234.5 -->
<!-- On blur: Display as $1,234.50 with Format="c2" -->
```

## Edge Cases and Gotchas

### Edge Case 1: Very Large Numbers

```razor
<!-- long? supports larger numbers than int? -->
<SfNumericTextBox TValue="long?" 
    Value="9223372036854775807">
</SfNumericTextBox>

<!-- Use Min/Max to prevent overflow -->
<SfNumericTextBox TValue="int?" 
    Min="-2147483648"
    Max="2147483647">
</SfNumericTextBox>
```

### Edge Case 2: Negative Numbers

```razor
<!-- Support negative values with Min -->
<SfNumericTextBox TValue="int?" 
    Min="-100"
    Max="100"
    Value="0">
</SfNumericTextBox>

<!-- Temperature with negative -->
<SfNumericTextBox TValue="int?" 
    Min="-50"
    Max="50"
    Value="20"
    Placeholder="Temperature in Celsius">
</SfNumericTextBox>
```

### Edge Case 3: Zero Values

```razor
<!-- Handle zero carefully in conditions -->
<SfNumericTextBox TValue="int?" 
    Value="@(quantity ?? 0)"
    Min="0">
</SfNumericTextBox>

<!-- Check for HasValue, not value -->
@code {
    private int? quantity;
    
    private bool IsQuantitySet => quantity.HasValue;
    private int SafeQuantity => quantity ?? 0;
}
```

### Edge Case 4: Format and Type Mismatch

```razor
<!-- ❌ Wrong: int with percentage -->
<SfNumericTextBox TValue="int?" Format="p2"></SfNumericTextBox>

<!-- ✅ Correct: double with percentage -->
<SfNumericTextBox TValue="double?" Value="0.5" Format="p2"></SfNumericTextBox>
<!-- Displays: 50.00% -->
```

## Next Steps

- See [data-binding-and-events.md](data-binding-and-events.md) for form integration details
- See [range-validation-and-formatting.md](range-validation-and-formatting.md) for validation options
- See [globalization-accessibility.md](globalization-accessibility.md) for internationalization
