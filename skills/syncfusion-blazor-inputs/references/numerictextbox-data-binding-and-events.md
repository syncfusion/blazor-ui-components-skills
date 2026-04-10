# Data Binding and Events

## Table of Contents
- [Two-Way Data Binding](#two-way-data-binding)
- [Value Property](#value-property)
- [Model Binding](#model-binding)
- [Events Overview](#events-overview)
- [ValueChange Event](#valuechange-event)
- [Form Integration](#form-integration)
- [Event Handling Patterns](#event-handling-patterns)
- [Troubleshooting](#troubleshooting)

## Two-Way Data Binding

Two-way binding synchronizes the NumericTextBox value with a C# variable using the `@bind-Value` directive.

### Basic Two-Way Binding

```razor
@page "/binding-example"

<h3>Two-Way Binding Example</h3>

<!-- Input with two-way binding -->
<SfNumericTextBox TValue="int?" @bind-Value="currentValue"></SfNumericTextBox>

<!-- Display current value -->
<p>Current value: @currentValue</p>

@code {
    private int? currentValue = 10;
}
```

**What happens:**
1. User enters value in input field
2. Value automatically updates `currentValue` variable
3. Display updates to show new value
4. Page re-renders automatically

### Benefits of Two-Way Binding

- **Automatic synchronization:** No manual event handling needed
- **Simplified code:** Cleaner than manual event handling
- **Reactive updates:** Other components see changes immediately
- **Form integration:** Works seamlessly with EditForm

## Value Property

The `Value` property directly sets the initial value without binding.

### One-Way Binding (Display Only)

```razor
<!-- Static value, doesn't bind to variable -->
<SfNumericTextBox TValue="int?" Value=42></SfNumericTextBox>

<!-- Bound to variable but one-way (updates variable but not input) -->
@code {
    private int? myValue = 42;
}

<SfNumericTextBox TValue="int?" Value="@myValue"></SfNumericTextBox>
```

### When to Use Value vs @bind-Value

| Use Case | Use | Example |
|----------|-----|---------|
| Display static number | Value | `Value=42` |
| Programmatic updates needed | @bind-Value | `@bind-Value="total"` |
| Form submission | @bind-Value | EditForm integration |
| Read-only display | Value | `Value="@Math.Pi"` |
| Two-way synchronization | @bind-Value | Real-time calculations |

## Model Binding

Model binding integrates NumericTextBox with data models and validation.

### Basic Model Binding

```razor
@page "/model-binding"

<!-- EditForm with data annotations -->
<EditForm Model="@product" OnValidSubmit="@HandleSubmit">
    <DataAnnotationsValidator></DataAnnotationsValidator>
    <ValidationSummary></ValidationSummary>
    
    <!-- Price input with model binding -->
    <div class="form-group">
        <label for="price">Price:</label>
        <SfNumericTextBox TValue="decimal?" 
            @bind-Value="product.Price"
            Format="c2"
            Min=0
            Decimals=2>
        </SfNumericTextBox>
        <ValidationMessage For="@(() => product.Price)"></ValidationMessage>
    </div>
    
    <button type="submit">Save</button>
</EditForm>

@code {
    private ProductModel product = new();
    
    private async Task HandleSubmit()
    {
        // Form submitted with valid data
        await SaveProduct(product);
    }
    
    public class ProductModel
    {
        [Range(0.01, 999999.99)]
        public decimal? Price { get; set; }
    }
}
```

### Data Annotations for Validation

```csharp
public class OrderModel
{
    [Range(1, 1000, ErrorMessage = "Quantity must be 1-1000")]
    public int? Quantity { get; set; }
    
    [Range(0.01, 999999.99, ErrorMessage = "Price must be positive")]
    public decimal? Price { get; set; }
    
    [Range(0, 100, ErrorMessage = "Discount must be 0-100%")]
    public double? Discount { get; set; }
}
```

### Razor Component with Model

```razor
@page "/order-form"

<EditForm Model="@order">
    <DataAnnotationsValidator></DataAnnotationsValidator>
    
    <div>
        <label>Order Quantity:</label>
        <SfNumericTextBox TValue="int?" 
            @bind-Value="order.Quantity"
            Min=1
            Max=1000>
        </SfNumericTextBox>
    </div>
    
    <div>
        <label>Unit Price:</label>
        <SfNumericTextBox TValue="decimal?" 
            @bind-Value="order.UnitPrice"
            Format="c2"
            Min=0
            Decimals=2>
        </SfNumericTextBox>
    </div>
    
    <div>
        <label>Discount %:</label>
        <SfNumericTextBox TValue="double?" 
            @bind-Value="order.DiscountPercent"
            Format="p2"
            Min=0
            Max=100>
        </SfNumericTextBox>
    </div>
    
    <button type="submit">Place Order</button>
</EditForm>

@code {
    private OrderModel order = new();
    
    public class OrderModel
    {
        public int? Quantity { get; set; }
        public decimal? UnitPrice { get; set; }
        public double? DiscountPercent { get; set; }
    }
}
```

## Events Overview

NumericTextBox provides several events for handling user interactions.

### Available Events

NumericTextBox events must be declared inside a `<NumericTextBoxEvents>` nested tag.

| Event | Triggered | Use Case |
|-------|-----------|----------|
| `ValueChange` | Value changes | Real-time calculations, validation |
| `Focus` | Input focused | Trigger animations, show hints |
| `Blur` | Input loses focus | Final validation, format display |
| `Created` | Component initialized | Setup, initialization logic |
| `Destroyed` | Component disposed | Cleanup resources |

## ValueChange Event

The `ValueChange` event fires when the value changes.

### Basic ValueChange

```razor
@page "/value-change"

<SfNumericTextBox TValue="int?" Value=@currentValue>
    <NumericTextBoxEvents TValue="int?" ValueChange="@OnValueChange"></NumericTextBoxEvents>
</SfNumericTextBox>

<p>Current value: @currentValue</p>
<p>Changed count: @changeCount</p>

@code {
    private int? currentValue = 0;
    private int changeCount = 0;
    
    private void OnValueChange(ChangeEventArgs<int?> args)
    {
        currentValue = args.Value;
        changeCount++;
    }
}
```

### Real-Time Calculation with ValueChange

```razor
@page "/price-calculator"

<h3>Price Calculator</h3>

<div>
    <label>Quantity:</label>
    <SfNumericTextBox TValue="int?" 
        Value=@quantity
        Min=1>
        <NumericTextBoxEvents TValue="int?" ValueChange="@OnQuantityChange"></NumericTextBoxEvents>
    </SfNumericTextBox>
</div>

<div>
    <label>Unit Price:</label>
    <SfNumericTextBox TValue="decimal?" 
        Value=@unitPrice
        Format="c2"
        Min=0
        Decimals=2>
        <NumericTextBoxEvents TValue="decimal?" ValueChange="@OnPriceChange"></NumericTextBoxEvents>
    </SfNumericTextBox>
</div>

<div>
    <label>Discount %:</label>
    <SfNumericTextBox TValue="double?" 
        Value=@discount
        Format="p2"
        Min=0
        Max=100>
        <NumericTextBoxEvents TValue="double?" ValueChange="@OnDiscountChange"></NumericTextBoxEvents>
    </SfNumericTextBox>
</div>

<hr />

<h4>Summary</h4>
<p>Quantity: @quantity</p>
<p>Unit Price: @unitPrice?.ToString("C2")</p>
<p>Subtotal: @(quantity * unitPrice)?.ToString("C2")</p>
<p>Discount: @discount?.ToString("P2")</p>
<p><strong>Total: @CalculateTotal().ToString("C2")</strong></p>

@code {
    private int? quantity = 1;
    private decimal? unitPrice = 10m;
    private double? discount = 0;
    
    private void OnQuantityChange(ChangeEventArgs<int?> args)
    {
        quantity = args.Value;
    }
    
    private void OnPriceChange(ChangeEventArgs<decimal?> args)
    {
        unitPrice = args.Value;
    }
    
    private void OnDiscountChange(ChangeEventArgs<double?> args)
    {
        discount = args.Value;
    }
    
    private decimal CalculateTotal()
    {
        decimal subtotal = (quantity ?? 0) * (unitPrice ?? 0);
        decimal discountAmount = subtotal * (decimal)(discount ?? 0) / 100;
        return subtotal - discountAmount;
    }
}
```

## Form Integration

NumericTextBox integrates seamlessly with Blazor EditForm and form validation.

### Complete Form Example

```razor
@page "/order-form"

<h3>Order Form</h3>

<EditForm Model="@order" OnValidSubmit="@HandleSubmit" OnInvalidSubmit="@HandleInvalidSubmit">
    <DataAnnotationsValidator></DataAnnotationsValidator>
    
    <div class="form-group">
        <label for="qty">Quantity:</label>
        <SfNumericTextBox 
            TValue="int?" 
            ID="qty"
            @bind-Value="order.Quantity"
            Min=1
            Max=10000
            Placeholder="Enter quantity">
        </SfNumericTextBox>
        <ValidationMessage For="@(() => order.Quantity)"></ValidationMessage>
    </div>
    
    <div class="form-group">
        <label for="price">Unit Price:</label>
        <SfNumericTextBox 
            TValue="decimal?" 
            ID="price"
            @bind-Value="order.UnitPrice"
            Format="c2"
            Min=0.01m
            Decimals=2
            ValidateDecimalOnType=true
            Placeholder="Enter price">
        </SfNumericTextBox>
        <ValidationMessage For="@(() => order.UnitPrice)"></ValidationMessage>
    </div>
    
    <div class="form-group">
        <label for="discount">Discount %:</label>
        <SfNumericTextBox 
            TValue="double?" 
            ID="discount"
            @bind-Value="order.Discount"
            Format="p2"
            Min=0
            Max=100
            Placeholder="Enter discount">
        </SfNumericTextBox>
        <ValidationMessage For="@(() => order.Discount)"></ValidationMessage>
    </div>
    
    <button type="submit" class="btn btn-primary">Submit Order</button>
</EditForm>

@if (submitMessage != null)
{
    <div class="alert alert-info">@submitMessage</div>
}

@code {
    private OrderForm order = new();
    private string? submitMessage;
    
    private async Task HandleSubmit()
    {
        // Validation passed
        submitMessage = $"Order submitted: {order.Quantity} units @ {order.UnitPrice?.ToString("C2")}";
        await Task.Delay(1000); // Simulate API call
    }
    
    private void HandleInvalidSubmit()
    {
        submitMessage = "Please fix validation errors above.";
    }
    
    public class OrderForm
    {
        [Range(1, 10000, ErrorMessage = "Quantity must be 1-10000")]
        public int? Quantity { get; set; }
        
        [Range(0.01, 999999.99, ErrorMessage = "Price must be between 0.01 and 999999.99")]
        public decimal? UnitPrice { get; set; }
        
        [Range(0, 100, ErrorMessage = "Discount must be 0-100%")]
        public double? Discount { get; set; }
    }
}
```

## Event Handling Patterns

### Pattern 1: Conditional Updates

```razor
<!-- Only update if value is within custom range -->
<SfNumericTextBox TValue="int?">
    <NumericTextBoxEvents TValue="int?" ValueChange="@OnValueChange"></NumericTextBoxEvents>
</SfNumericTextBox>

@code {
    private int? currentValue = 0;
    
    private void OnValueChange(ChangeEventArgs<int?> args)
    {
        if (args.Value >= 0 && args.Value <= 100)
        {
            // Valid range
            currentValue = args.Value;
        }
        else
        {
            // Out of range, revert
            currentValue = 0;
        }
    }
}
```

### Pattern 2: Dependent Fields

```razor
<!-- When quantity changes, recalculate total -->
<SfNumericTextBox TValue="int?" Value=@quantity>
    <NumericTextBoxEvents TValue="int?" ValueChange="@OnQuantityChange"></NumericTextBoxEvents>
</SfNumericTextBox>

@code {
    private int? quantity = 1;
    private decimal? unitPrice = 10;
    private decimal? total;
    
    private void OnQuantityChange(ChangeEventArgs<int?> args)
    {
        quantity = args.Value;
        total = quantity * unitPrice;
    }
}
```

### Pattern 3: Format Conversion

```razor
<!-- Convert input to different format for storage -->
<SfNumericTextBox TValue="double?" Format="p2">
    <NumericTextBoxEvents TValue="double?" ValueChange="@OnPercentageChange"></NumericTextBoxEvents>
</SfNumericTextBox>

@code {
    private void OnPercentageChange(ChangeEventArgs<double?> args)
    {
        // Store as 0-1 range (0.5 = 50%)
        if (args.Value.HasValue)
        {
            double percentageDecimal = args.Value.Value / 100;
            // Save to database
        }
    }
}
```

## Troubleshooting

### Issue: Value not updating after form submission

```razor
<!-- ❌ Problem: Using Value instead of @bind-Value -->
<SfNumericTextBox TValue="int?" Value="@myValue"></SfNumericTextBox>

<!-- ✅ Solution: Use @bind-Value -->
<SfNumericTextBox TValue="int?" @bind-Value="@myValue"></SfNumericTextBox>
```

### Issue: Event fires but variable doesn't update

```razor
<!-- ❌ Problem: Not updating variable in event -->
<SfNumericTextBox TValue="int?">
    <NumericTextBoxEvents TValue="int?" ValueChange="@OnValueChange"></NumericTextBoxEvents>
</SfNumericTextBox>

@code {
    private int? myValue;
    
    private void OnValueChange(ChangeEventArgs<int?> args)
    {
        // Missing: myValue = args.Value;
    }
}

<!-- ✅ Solution: Update variable in event -->
@code {
    private int? myValue;
    
    private void OnValueChange(ChangeEventArgs<int?> args)
    {
        myValue = args.Value;
    }
}
```

### Issue: Mixing @bind-Value with ValueChange event

```razor
<!-- ❌ Problem: Cannot use both @bind-Value and ValueChange event -->
<SfNumericTextBox TValue="int?" 
    @bind-Value="value">
    <NumericTextBoxEvents TValue="int?" ValueChange="@OnValueChange"></NumericTextBoxEvents>
</SfNumericTextBox>

<!-- ✅ Solution: Use @bind-Value only (simplest) -->
<SfNumericTextBox TValue="int?" @bind-Value="value"></SfNumericTextBox>

<!-- OR: Use ValueChange event without @bind-Value -->
<SfNumericTextBox TValue="int?" Value="@value">
    <NumericTextBoxEvents TValue="int?" ValueChange="@OnValueChange"></NumericTextBoxEvents>
</SfNumericTextBox>

@code {
    private int? value;
    
    private void OnValueChange(ChangeEventArgs<int?> args)
    {
        value = args.Value;
    }
}
```

### Issue: Model binding not reflecting changes

```razor
<!-- Make sure model property is public -->
@code {
    public class MyModel
    {
        public int? Value { get; set; }  // ✅ Public
        // private int? Value { get; set; }  // ❌ Won't work
    }
}
```

## Next Steps

- See [range-validation-and-formatting.md](range-validation-and-formatting.md) for validation patterns
- See [customization-styling.md](customization-styling.md) for appearance customization
- See [globalization-accessibility.md](globalization-accessibility.md) for localization
