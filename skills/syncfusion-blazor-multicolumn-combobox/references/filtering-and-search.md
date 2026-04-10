# Filtering and Search

## Table of Contents
- [Enable Filtering](#enable-filtering)
- [Filter Types](#filter-types)
- [Case-Sensitive Filtering](#case-sensitive-filtering)
- [Custom Filtering](#custom-filtering)
- [Multi-Column Filtering](#multi-column-filtering)
- [Filter Length and Debounce](#filter-length-and-debounce)
- [Remote Filtering](#remote-filtering)
- [Filter Events](#filter-events)
- [Examples](#examples)

---

## Enable Filtering

### Basic Filtering

Enable search/filter on the ComboBox:

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       AllowFiltering="true"
                       TextField="ProductName"
                       ValueField="ProductID"
                       Placeholder="Search products...">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private List<ProductData> Products = new();
    // Data initialization...
}
```

**What happens:**
- Input box appears above dropdown
- User types to filter results
- Dropdown shows matching items only
- Default filter: contains (case-insensitive)

---

## Filter Types

### StartsWith

Match from beginning of text:

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       AllowFiltering="true"
                       FilterType="FilterType.StartsWith"
                       Placeholder="Search...">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

**Example:** Typing "lap" matches "laptop" but not "laptop sleeve"

### Contains

Match anywhere in text (default):

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       AllowFiltering="true"
                       FilterType="FilterType.Contains"
                       Placeholder="Search...">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

**Example:** Typing "top" matches both "laptop" and "desktop"

### EndsWith

Match at end of text:

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       AllowFiltering="true"
                       FilterType="FilterType.EndsWith"
                       TextField="ProductName"
                       ValueField="ProductID"
                       Placeholder="Search...">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

**Example:** Typing "top" matches "laptop" but not "topic"

---

## Case-Sensitive Filtering

### Default: Case-Insensitive

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       AllowFiltering="true"
                       TextField="ProductName"
                       ValueField="ProductID"
                       Placeholder="Search (case-insensitive)...">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

**Behavior:** "LAPTOP", "laptop", "LaPtOp" all match

### Enable Case-Sensitive

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       AllowFiltering="true"
                       IgnoreCase="false"
                       TextField="ProductName"
                       ValueField="ProductID"
                       Placeholder="Search (case-sensitive)...">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

**Behavior:** Only exact case matches. "laptop" ≠ "LAPTOP"

---

## Custom Filtering

### Filter by Specific Field

```razor
<SfMultiColumnComboBox TValue="int" TItem="EmployeeData" 
                       DataSource="@Employees"
                       AllowFiltering="true"
                       FilterType="FilterType.StartsWith"
                       TextField="FullName"
                       ValueField="EmployeeID"
                       Placeholder="Search by name or email">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="EmployeeID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="FullName" Header="Name"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Email" Header="Email"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

### Multi-Column Search

Filter across multiple columns:

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@FilteredProducts"
                       AllowFiltering="true"
                       FilterType="FilterType.Contains"
                       Filtering="@OnFilter"
                       TextField="ProductName"
                       ValueField="ProductID"
                       Placeholder="Search by name or category...">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Category" Header="Category"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private List<ProductData> AllProducts = new();
    private List<ProductData> FilteredProducts = new();

    private async Task OnFilter(FilteringEventArgs args)
    {
        args.PreventDefaultAction = true;
        string searchTerm = args.Text.ToLower();

        // Filter on multiple columns
        FilteredProducts = AllProducts
            .Where(p => 
                p.ProductName.ToLower().Contains(searchTerm) ||
                p.Category.ToLower().Contains(searchTerm) ||
                p.ProductID.ToString().Contains(searchTerm)
            )
            .ToList();

        // Notify component of filtered data
        await Task.CompletedTask;
    }

    protected override void OnInitialized()
    {
        AllProducts = new()
        {
            new ProductData { ProductID = 1, ProductName = "Laptop", Category = "Electronics" },
            new ProductData { ProductID = 2, ProductName = "Mouse Pad", Category = "Accessories" },
            new ProductData { ProductID = 3, ProductName = "Keyboard", Category = "Electronics" }
        };
        FilteredProducts = AllProducts;
    }

    public class ProductData
    {
        public int ProductID { get; set; }
        public string ProductName { get; set; }
        public string Category { get; set; }
    }
}
```

### Global Filtering

Enable search filtering across the component:

```razor
<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       AllowFiltering="true"
                       TextField="CustomerName"
                       ValueField="OrderID">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="CustomerName" Header="Customer"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Amount" Header="Amount"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

**Result:** Component filters across all displayed data.

---

## Filter Length and Debounce (Delay)

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       AllowFiltering="true"
                       DebounceDelay="500"
                       Placeholder="Search products...">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>.
```

**Purpose:** Reduces filtering calls while user is still typing

---

## Remote Filtering

### Filter from Web API

```razor
@page "/remote-filter"
@using Syncfusion.Blazor.DropDowns
@using Syncfusion.Blazor.Data
MultiColumnComboBox
@using Syncfusion.Blazor.Data

<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       QuMultiColumnComboBox
@using Syncfusion.Blazor.Data

<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       Query="@DataQuery"
                       AllowFiltering="true"
                       Filtering="@OnRemoteFilter"
                       TextField="ProductName"
                       ValueField="ProductID"
                       Placeholder="Search products...">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@cod
        // Modify query to filter on server
        DataQuery = new Query()
            .From("products")
            .Where("ProductName", "contains", args.Text);
        
        await Task.CompletedTask;
    }

    public class ProductData
    {
        public int ProductID { get; set; }
        public string ProductName { get; set; }
    }
}
```

---

## Filter Events

### Filtering Event

Fires when user types in filter box:
MultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       AllowFiltering="true"
                       Filtering="@OnFiltering">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumn        Filtering="@OnFiltering">
    <!-- Columns -->

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       AllowFiltering="true"
                       Filtering="@OnFiltering">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumn    
        Console.WriteLine($"User typed: {args.Text}");
        
        if (args.Text.Length < 2)
        {
            args.PreventDefaultAction = true; // Don't show results yet
        }
        
        await Task.CompletedTask;
    }
}
```

---

## Examples

### Complete Filtering Setup

```razor
@page "/filter-example"
@using Syncfusion.Blazor.MultiColumnComboBox

<h3>Order Filter Example</h3>

<div class="form-group">
    <label>Filter Options:</label>
    <select @onchange="@((ChangeEventArgs e) => ChangeFilterType(e.Value.ToString()))" class="form-control">
        <option value="Contains">Contains</option>
        <option value="StartsWith">Starts With</option>
        <option value="EndsWith">Ends With</option>
    </select>
</div>

<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       AllowFiltering="true"
                       FilterType="@CurrentFilterType"
                       TextField="CustomerName"
                       ValueField="OrderID"
                       Placeholder="Search orders by ID or customer name...">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order ID" Width="100px"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="CustomerName" Header="Customer" Width="200px"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="OrderDate" Header="Date" Width="120px" Format="MM/dd/yyyy"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private List<OrderData> Orders = new();
    private FilterType CurrentFilterType = FilterType.Contains;

    protected override void OnInitialized()
    {
        Orders = new()
        {
            new OrderData { OrderID = 1001, CustomerName = "Alice Johnson", OrderDate = new DateTime(2024, 1, 15) },
            new OrderData { OrderID = 1002, CustomerName = "Bob Smith", OrderDate = new DateTime(2024, 1, 20) },
            new OrderData { OrderID = 1003, CustomerName = "Carol White", OrderDate = new DateTime(2024, 1, 25) },
            new OrderData { OrderID = 1004, CustomerName = "David Brown", OrderDate = new DateTime(2024, 2, 5) }
        };
    }

    private void ChangeFilterType(string filterType)
    {
        CurrentFilterType = (FilterType)Enum.Parse(typeof(FilterType), filterType);
    }

    public class OrderData
    {
        public int OrderID { get; set; }
        public string CustomerName { get; set; }
        public DateTime OrderDate { get; set; }
    }
}
```

---

## Next Steps

- **Handle selection:** See [references/selection-and-values.md](selection-and-values.md)
- **Optimize performance:** See [references/features-and-settings.md](features-and-settings.md)
- **Customize styling:** See [references/styling-and-appearance.md](styling-and-appearance.md)
