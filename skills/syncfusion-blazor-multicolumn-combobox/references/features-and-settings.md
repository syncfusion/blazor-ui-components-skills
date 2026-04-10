# Features and Settings

## Table of Contents
- [Paging](#paging)
- [Virtualization](#virtualization)
- [Grouping](#grouping)
- [Popup Settings](#popup-settings)
- [Placeholder and Float Label](#placeholder-and-float-label)
- [Grid Settings](#grid-settings)
- [Form Validation](#form-validation)
- [Enabled and Disabled States](#enabled-and-disabled-states)
- [Examples](#examples)

---

## Paging

### Enable Paging

Load data in pages instead of all at once:

```razor
<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       AllowPaging="true"
                       PageSize="10"
                       TextField="CustomerName"
                       ValueField="OrderID"
                       Placeholder="Select an order">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="CustomerName" Header="Customer"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private List<OrderData> Orders = new();
    // 100+ items, shown 10 per page
}
```

### Page Size Settings

```razor
<!-- 15 items per page -->
<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       AllowPaging="true"
                       PageSize="15">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

**Use cases:**
- ✓ Large datasets (1000+ records)
- ✓ Remote data with bandwidth concerns
- ✓ Better UX with gradual loading

---

## Virtualization

### Virtual Scrolling

Render only visible rows for extreme performance:

```razor
<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       EnableVirtualization="true"
                       TextField="CustomerName"
                       ValueField="OrderID"
                       Placeholder="Select an order">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="CustomerName" Header="Customer"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private List<OrderData> Orders = new();
    // Can handle 10,000+ items smoothly
}
```

**Benefits:**
- ✓ Handles 10,000+ items smoothly
- ✓ Minimal memory footprint
- ✓ Fast scrolling performance
- ✓ Better than paging for large lists

### Combined with Paging

```razor
<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       AllowPaging="true"
                       PageSize="50"
                       EnableVirtualization="true">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

---

## Grouping

### Group by Field

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       GroupByField="Category"
                       TextField="ProductName"
                       ValueField="ProductID"
                       Placeholder="Select a product">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Category" Header="Category"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    public class ProductData
    {
        public int ProductID { get; set; }
        public string ProductName { get; set; }
        public string Category { get; set; }
    }

    private List<ProductData> Products = new()
    {
        new ProductData { ProductID = 1, ProductName = "Laptop", Category = "Electronics" },
        new ProductData { ProductID = 2, ProductName = "Mouse", Category = "Electronics" },
        new ProductData { ProductID = 3, ProductName = "Desk", Category = "Furniture" },
        new ProductData { ProductID = 4, ProductName = "Chair", Category = "Furniture" }
    };
}
```

**Note:** `GroupByField` accepts a single string field name for grouping. For complex grouping scenarios, consider using `Query` with remote data.

---

## Popup Settings

### Popup Position and Size

```razor
<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       PopupHeight="400px"
                       PopupWidth="600px">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

### Column Resizing

```razor
<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       TextField="CustomerName"
                       ValueField="OrderID"
                       AllowColumnResizing="true"
                       PopupHeight="300px"
                       PopupWidth="500px">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="CustomerName" Header="Customer"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

**Behavior:** User can drag column borders to resize columns in the popup.

### Popup Events

```razor
<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       PopupOpening="@OnPopupOpening"
                       PopupClosing="@OnPopupClosing">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private async Task OnPopupOpening(PopupOpeningEventArgs args)
    {
        Console.WriteLine("Popup opening");
        await Task.CompletedTask;
    }

    private async Task OnPopupClosing(PopupClosing
}EventArgs args)
    {
        Console.WriteLine("Popup closing");
        await Task.CompletedTask;
    }
```

---

## Placeholder and Float Label
MultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       Placeholder="Choose a product...">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

### Float Label

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       FloatLabelType="FloatLabelType.Auto"
                       Placeholder="Product Name">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumn        DataSource="@Products"
            FloatLabelType="FloatLabelType.Auto"
            Placeholder="Product Name">
    <!-- Columns -->
</SfComboBox>
```

**FloatLabelType Options:**
- `Auto` - Floats on focus or input
- `Always` - Always floats
- `Never` - Never floats

### Custom Placeholder Color
yle>
    .custom-placeholder .e-input::placeholder {
        color: #888;
    }
</style>

<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       CssClass="custom-placeholder"
                       Placeholder="Select product...">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

---

## Grid Settings

### Row Height

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       RowHeight="35">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

### Alternating Row Colors

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       EnableAltRow="true">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

### Grid Lines

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       GridLines="GridLine.Both">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumn        GridLines="GridLine.Both">
    <!-- Columns -->
</SfComboBox>
```

---

## Form Validation

### Bind to EditForm

```razor
<EditForm Model="@OrderModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    
    <div class="form-group">
        <label>Order:</label>
        <SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                               DataSource="@Orders"
                               @bind-Value="@OrderModel.SelectedOrderID">
            <MultiColumnComboboxColumns>
                <MultiColumnComboboxColumn Field="OrderID" Header="Order ID"></MultiColumnComboboxColumn>
                <MultiColumnComboboxColumn Field="CustomerName" Header="Customer"></MultiColumnComboboxColumn>
            </MultiColumnComboboxColumns>
        </SfMultiColumnComboBox>
        <ValidationMessage For="@(() => OrderModel.SelectedOrderID)" />
    </div>
    
    <button type="submit">Submit</button>
</EditForm>

@code {
    private OrderModel OrderModel = new();
    private List<OrderData> Orders = new();

    public class OrderModel
    {
        [Required(ErrorMessage = "Order selection is required")]
        public int SelectedOrderID { get; set; }
    }

    public class OrderData
    {
        public int OrderID { get; set; }
        public string CustomerName { get; set; }
    }

    private async Task HandleValidSubmit()
    {
        Console.WriteLine($"Form submitted with Order: {OrderModel.SelectedOrderID}");
        await Task.CompletedTask;
    }

    protected override void OnInitialized()
    {
        Orders = new()
        {
            new OrderData { OrderID = 1001, CustomerName = "Alice" },
            new OrderData { OrderID = 1002, CustomerName = "Bob" }
        };
    }
}
```

### Custom Validation

```csharp
public class OrderModel
{
    [Required]
    [Range(1000, 9999, ErrorMessage = "Invalid order number")]
    public int SelectedOrderID { get; set; }
}
```

---

## Enabled and Disabled States

### Disable ComboBox

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       Disabled="true"
                       Placeholder="This is disabled">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

### Conditionally Enable/Disable
MultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       Disabled="@IsDisabled"

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       Disabled="@IsDisabled"
                       @bind-Value="@SelectedProductID">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

<button @onclick="@(() => IsDisabled = !IsDisabled)">Toggle Enable</button>

@code {
    private bool IsDisabled = false;
    private int SelectedProductID { get; set; }
}
```

### ReadOnly Mode

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       ReadOnly="true"
                       @bind-Value="@SelectedProductID">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumn

## ExamplesMultiColumnComboBox

<h3>Advanced Order Selection</h3>


### Complete MultiColumn ComboBox Example

```razor
@page "/advanced-combobox"
<div class="form-group mb-3">
    <label>Search Options:</label>
    <div>
        <label>
            <input type="checkbox" @bind="ShowPaging" /> Enable Paging (10 items/page)
        </label>
        <label>
            <input type="checkbox" @bind="ShowVirtualization" /> Enable Virtualization
        </label>
        <label>
            <input type="checkbox" @bind="ShowGrouping" /> Group by Status
        </label>
    </div>
</div>

<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@FilteredOrders"
                       AllowPaging="@ShowPaging"
                       PageSize="10"
                       EnableVirtualization="@ShowVirtualization"
                       GroupByField="@(ShowGrouping ? \"Status\" : null)"
                       AllowFiltering="true"
                       FloatLabelType="FloatLabelType.Auto"
                       Placeholder="Select an order"
                       EnableAltRow="true"
                       GridLines="GridLine.Both"
                       PopupHeight="400px"
                       TextField="CustomerName"
                       ValueField="OrderID"
                       @bind-Value="@SelectedOrderID">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order ID" Width="100px"></MultiColumnComboboxColumn>
        <MultiC<ComboBoxColumn Field="OrderID" Header="Order ID" Width="100px"></ComboBoxColumn>
    <ComboBoxColumn Field="CustomerName" Header="Customer" Width="150px"></ComboBoxColumn>
    <ComboBoxColumn Field="Status" Header="Status" Width="100px"></ComboBoxColumn>
    <ComboBoxColumn Field="TotalAmount" Header="Amount" Width="120px" Format="C2" TextAlign="TextAlign.Right"></ComboBoxColumn>
</SfComboBox>

<p class="mt-3">Selected Order ID: <strong>@SelectedOrderID</strong></p>

@code {
    private List<OrderData> FilteredOrders = new();
    private int SelectedOrderID { get; set; }
    private bool ShowPaging = false;
    private bool ShowVirtualization = false;
    private bool ShowGrouping = false;

    protected override void OnInitialized()
    {
        // Generate sample data
        var orders = new List<OrderData>();
        for (int i = 1; i <= 100; i++)
        {
            orders.Add(new OrderData
            {
                OrderID = 1000 + i,
                CustomerName = $"Customer {i}",
                Status = (OrderStatus)(i % 4),
                TotalAmount = (decimal)(50 + i * 1.5)
            });
        }
        FilteredOrders = orders;
    }

    public enum OrderStatus
    {
        Pending,
        Processing,
        Shipped,
        Delivered
    }

    public class OrderData
    {
        public int OrderID { get; set; }
        public string CustomerName { get; set; }
        public OrderStatus Status { get; set; }
        public decimal TotalAmount { get; set; }
    }
}
```

---

## Next Steps

- **Customize styling:** See [references/styling-and-appearance.md](styling-and-appearance.md)
- **Accessibility:** See [references/accessibility-localization.md](accessibility-localization.md)
- **Advanced integration:** See [references/advanced-patterns.md](advanced-patterns.md)
