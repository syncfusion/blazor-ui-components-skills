# Accessibility and Localization

## Table of Contents
- [WCAG Compliance](#wcag-compliance)
- [Keyboard Navigation](#keyboard-navigation)
- [ARIA Attributes](#aria-attributes)
- [Screen Reader Support](#screen-reader-support)
- [Focus Management](#focus-management)
- [Color Contrast](#color-contrast)
- [Localization](#localization)
- [Form Validation Accessibility](#form-validation-accessibility)
- [Examples](#examples)

---

## WCAG Compliance

### WCAG 2.1 Level AA

The Syncfusion MultiColumn ComboBox meets WCAG 2.1 AA standards:

- ✓ Perceivable: Visible labels, sufficient color contrast
- ✓ Operable: Keyboard accessible, focus indicators
- ✓ Understandable: Clear labels, predictable behavior
- ✓ Robust: Compatible with assistive technologies

### Ensure Accessibility

```razor
<!-- ✅ ACCESSIBLE -->
<label for="product-selector">Select Product:</label>
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       ID="product-selector"
                       DataSource="@Products"
                       Placeholder="Choose a product">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

<!-- ❌ NOT ACCESSIBLE -->
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products">
    <MultiColumnComboboxColumns>
        <!-- No label, no ID -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

---

## Keyboard Navigation

### Default Keyboard Support

| Key | Action |
|-----|--------|
| `Tab` | Move to ComboBox |
| `Space` / `Enter` | Open dropdown |
| `↓ Arrow` | Navigate down |
| `↑ Arrow` | Navigate up |
| `Home` | First item |
| `End` | Last item |
| `Enter` | Select item |
| `Escape` | Close dropdown |
| `Backspace` | Clear input |

### Keyboard-Only Navigation

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       AllowFiltering="true">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

**User Journey (Keyboard Only):**
1. Press `Tab` to focus input
2. Type to filter: "lap" for laptop
3. Press `↓` to navigate results
4. Press `Enter` to select
5. Continue with `Tab` to next field

---

## ARIA Attributes

ARIA attributes enhance accessibility for screen readers:

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       Placeholder="Select a product">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

### ARIA Label Best Practices

```razor
<!-- ✅ GOOD: Clear, descriptive label -->
<label for="customer-combo">Select Customer:</label>
<SfMultiColumnComboBox TValue="int" TItem="CustomerData"
                       ID="customer-combo"
                       DataSource="@Customers"
                       Placeholder="Select customer for order">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

<!-- ❌ BAD: No label, no ID -->
<SfMultiColumnComboBox TValue="int" TItem="CustomerData"
                       DataSource="@Customers">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="CustomerID" Header="ID"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

---

## Screen Reader Support

### Component Announcement

When dropdown opens, screen readers announce:
- Component name and type
- Number of items
- Current selection (if any)
- Instructions for interaction

### Enhance with Description

```razor
<fieldset>
    <legend>Order Selection</legend>
    <div>
        <label for="order-selector">Select Order:</label>
        <small id="order-help">Choose an order from the list</small>
        <SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                               ID="order-selector"
                               DataSource="@Orders"
                               TextField="CustomerName"
                               ValueField="OrderID"
                               InputAttributes="@(new Dictionary<string, object> { {"aria-describedby", "order-help"} })"
                               Placeholder="Choose an order from the list">
            <MultiColumnComboboxColumns>
                <MultiColumnComboboxColumn Field="OrderID" Header="Order ID"></MultiColumnComboboxColumn>
                <MultiColumnComboboxColumn Field="CustomerName" Header="Customer"></MultiColumnComboboxColumn>
            </MultiColumnComboboxColumns>
        </SfMultiColumnComboBox>
    </div>
</fieldset>
```

---
s Management

### Custom Focus Styling

```razor
<style>
    .e-
## Focumulticolumn-combobox .e-input:focus {
        outline: 2px solid #0066cc;
        outline-offset: 2px;
    }

    .e-multicolumn-combobox .e-input-group.e-focus {
        box-shadow: 0 0 0 3px rgba(0, 102, 204, 0.25);
    }
</style>
```

### Visible Focus Outline

```razor
<!-- ✅ ACCESSIBLE: Focus outline clearly visible -->
<SfMultiColumnComboBox TValue="int" TItem="ProductData" DataSource="@Products">
    <MultiColumnComboboxColumns>
        <!-- Default focus styling works -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

<!-- ❌ INACCESSIBLE: Focus outline hidden -->
<style>
    .e-multicolumn-combobox .e-input:focus {
        outline: none; /* Don't remove! */
    }
</style>
```

### Tab Order Management

```razor
<!-- Controls tab order using HtmlAttributes -->
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       TextField="ProductName"
                       ValueField="ProductID"
                       HtmlAttributes="@(new Dictionary<string, object> { {"tabindex", "1"} })">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

<button tabindex="2">Submit</button>
```

---

## Color Contrast

### Sufficient Contrast Ratios

All text must meet WCAG AA standards (4.5:1 for normal text, 3:1 for large text):

```razor
<style>
    /* ✅ GOOD: 4.5:1 contrast (black on white) */
    .e-multicolumn-combobox .e-input {
        color: #000000;
        background-color: #ffffff;
    }

    /* ❌ BAD: Insufficient contrast (gray on white) */
    .e-multicolumn-combobox .e-input {
        color: #cccccc;
        background-color: #ffffff;
    }
</style>
```

## Form Validation Accessibility

### Accessible Validation Messages

```razor
<EditForm Model="@OrderModel" OnValidSubmit="@HandleSubmit">
    <DataAnnotationsValidator />
    
    <div class="form-group">
        <label for="order-select">Order Selection:</label>
        
        <SfMultiColumnComboBox TValue="int" TItem="OrderData"
                               ID="order-select"
                               DataSource="@Orders"
                               TextField="CustomerName"
                               ValueField="OrderID"
                               @bind-Value="@OrderModel.SelectedOrderID"
                               InputAttributes="@GetInputAttributes()">
            <MultiColumnComboboxColumns>
                <MultiColumnComboboxColumn Field="OrderID" Header="Order ID"></MultiColumnComboboxColumn>
                <MultiColumnComboboxColumn Field="CustomerName" Header="Customer"></MultiColumnComboboxColumn>
            </MultiColumnComboboxColumns>
        </SfMultiColumnComboBox>
        
        <div id="order-error" class="invalid-feedback">
            <ValidationMessage For="@(() => OrderModel.SelectedOrderID)" />
        </div>
    </div>
    
    <button type="submit" class="btn btn-primary">Submit</button>
</EditForm>

@code {
    private OrderModel OrderModel = new();
    private List<OrderData> Orders = new();

    public class OrderModel
    {
        [Required(ErrorMessage = "Please select an order")]
        public int SelectedOrderID { get; set; }
    }
}
```

### Error Announcement

```razor
<style>
    .invalid-feedback {
        display: block;
        color: #dc3545;
        font-size: 14px;
        margin-top: 5px;
    }

    .form-group .e-input-group.e-error {
        border-color: #dc3545;
    }

    .e-multicolumn-combobox.e-error .e-input-group {
        border-color: #dc3545;
    }
</style>

<!-- Screen reader announces errors -->
<div role="alert" class="invalid-feedback">
    Order selection is required
</div>
```

---

## Examples

### Fully Accessible Form

```razor
@page "/accessible-form"
@using Syncfusion.Blazor.DropDowns

<h2>Order Form MultiColumnComboBox TValue="int" TItem="CustomerData"
                                   ID="customer-select"
                                   DataSource="@Customers"
                                   AllowFiltering="true"
                                   Placeholder="Search customer..."
                                   @bind-Value="@OrderForm.CustomerID">
                <MultiColumnComboboxColumns>
                    <MultiColumnComboboxColumn Field="CustomerID" Header="ID" Width="60px"></MultiColumnComboboxColumn>
                    <MultiColumnComboboxColumn Field="CustomerName" Header="Name" Width="180px"></MultiColumnComboboxColumn>
                    <MultiColumnComboboxColumn Field="Email" Header="Email" Width="200px"></MultiColumnComboboxColumn>
                </MultiColumnComboboxColumns>
            </SfMultiColumnComboBox>
            
            <ValidationMessage For="@(() => OrderForm.CustomerID)" />
        </div>
        
        <div class="form-group">
            <label for="product-select">
                <span class="required">*</span> Product
            </label>
            
            <SfMultiColumnComboBox TValue="int" TItem="ProductData"
                                   ID="product-select"
                                   DataSource="@Products"
                                   AllowFiltering="true"
                                   Placeholder="Search product..."
                                   @bind-Value="@OrderForm.ProductID">
                <MultiColumnComboboxColumns>
                    <MultiColumnComboboxColumn Field="ProductID" Header="ID" Width="60px"></MultiColumnComboboxColumn>
                    <MultiColumnComboboxColumn Field="ProductName" Header="Product" Width="180px"></MultiColumnComboboxColumn>
                    <MultiColumnComboboxColumn Field="Price" Header="Price" Width="100px" Format="C2"></MultiColumnComboboxColumn>
                </MultiColumnComboboxColumns>
            </SfMultiColumnel for="product-select">
                <span class="required">*</span> Product
            </label>
            
            <SfMultiColumnComboBox TValue="int" TItem="ProductData"
                                   ID="product-select"
                                   DataSource="@Products"
                                   AllowFiltering="true"
                                   TextField="ProductName"
                                   ValueField="ProductID"
                                   Placeholder="Search product..."
                                   InputAttributes="@(new Dictionary<string, object> { {"aria-label", "Select a product"} })"
                                   @bind-Value="@OrderForm.ProductID">
                <MultiColumnComboboxColumns>
                    <MultiColumnComboboxColumn Field="ProductID" Header="ID" Width="60px"></MultiColumnComboboxColumn>
                    <MultiColumnComboboxColumn Field="ProductName" Header="Product" Width="180px"></MultiColumnComboboxColumn>
                    <MultiColumnComboboxColumn Field="Price" Header="Price" Width="100px" Format="C2"></MultiColumnComboboxColumn>
                </MultiColumnComboboxColumns>
            </SfMultiColumnComboBox>
            
            <ValidationMessage For="@(() => OrderForm.ProductID)" />
        </div>
    </fieldset>
    
    <button type="submit" class="btn btn-primary">Submit Order</button>
</EditForm>

<style>
    .required {
        color: #dc3545;
        margin-right: 5px;
    }

    .form-group {
        margin-bottom: 20px;
    }

    .form-group label {
        display: block;
        margin-bottom: 5px;
        font-weight: 600;
        color: #333;
    }

    .form-group small {
        display: block;
        margin-bottom: 8px;
        color: #666;
        font-size: 13px;
    }
</style>

@code {
    private OrderFormModel OrderForm = new();
    private List<CustomerData> Customers = new();
    private List<ProductData> Products = new();

    protected override void OnInitialized()
    {
        Customers = new()
        {
            new CustomerData { CustomerID = 1, CustomerName = "John Doe", Email = "john@example.com" },
            new CustomerData { CustomerID = 2, CustomerName = "Jane Smith", Email = "jane@example.com" }
        };

        Products = new()
        {
            new ProductData { ProductID = 1, ProductName = "Laptop", Price = 999.99m },
            new ProductData { ProductID = 2, ProductName = "Mouse", Price = 29.99m }
        };
    }

    private async Task HandleSubmit()
    {
        Console.WriteLine("Form submitted");
        await Task.CompletedTask;
    }

    public class OrderFormModel
    {
        [Required(ErrorMessage = "Customer selection is required")]
        public int CustomerID { get; set; }

        [Required(ErrorMessage = "Product selection is required")]
        public int ProductID { get; set; }
    }

    public class CustomerData
    {
        public int CustomerID { get; set; }
        public string CustomerName { get; set; }
        public string Email { get; set; }
    }

    public class ProductData
    {
        public int ProductID { get; set; }
        public string ProductName { get; set; }
        public decimal Price { get; set; }
    }
}
```

---

## Next Steps

- **Advanced styling:** See [references/styling-and-appearance.md](styling-and-appearance.md)
- **Advanced patterns:** See [references/advanced-patterns.md](advanced-patterns.md)
- **Performance optimization:** See [references/features-and-settings.md](features-and-settings.md)
