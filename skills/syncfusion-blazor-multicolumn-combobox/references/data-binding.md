# Data Binding

## Table of Contents
- [Local Data Binding](#local-data-binding)
- [Observable Collections](#observable-collections)
- [Complex Data Types](#complex-data-types)
- [Web API Binding](#web-api-binding)
- [OData Services](#odata-services)
- [Action Events](#action-events)
- [Custom Adaptors](#custom-adaptors)
- [Offline Mode](#offline-mode)
- [Troubleshooting](#troubleshooting)

---

## Local Data Binding

### Simple List Binding

Bind directly to a C# List:

```razor
@page "/local-binding"
@using Syncfusion.Blazor.MultiColumnComboBox

<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       TextField="ProductName"
                       ValueField="ProductID"
                       Placeholder="Select a product">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Name"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private List<ProductData> Products = new();

    protected override void OnInitialized()
    {
        Products = new()
        {
            new ProductData { ProductID = 1, ProductName = "Laptop" },
            new ProductData { ProductID = 2, ProductName = "Mouse" },
            new ProductData { ProductID = 3, ProductName = "Keyboard" }
        };
    }

    public class ProductData
    {
        public int ProductID { get; set; }
        public string ProductName { get; set; }
    }
}
```

### Asynchronous Data Loading

Load data from API or database:

```csharp
protected override async Task OnInitializedAsync()
{
    Products = await FetchProductsFromAPI();
}

private async Task<List<ProductData>> FetchProductsFromAPI()
{
    using var http = new HttpClient();
    return await http.GetFromJsonAsync<List<ProductData>>("https://api.example.com/products");
}
```

---

## Observable Collections

### Dynamic Updates

Use `ObservableCollection` for real-time updates:

```razor
@using System.Collections.ObjectModel

<button @onclick="AddProduct">Add Product</button>

<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       TextField="ProductName"
                       ValueField="ProductID"
                       Placeholder="Select a product">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Name"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private ObservableCollection<ProductData> Products = new();

    protected override void OnInitialized()
    {
        Products = new ObservableCollection<ProductData>
        {
            new ProductData { ProductID = 1, ProductName = "Product A" },
            new ProductData { ProductID = 2, ProductName = "Product B" }
        };
    }

    private void AddProduct()
    {
        Products.Add(new ProductData 
        { 
            ProductID = Products.Count + 1, 
            ProductName = $"Product {(char)('C' + Products.Count)}" 
        });
    }

    public class ProductData
    {
        public int ProductID { get; set; }
        public string ProductName { get; set; }
    }
}
```

**Benefits:**
- ✓ UI updates automatically when collection changes
- ✓ Add/remove items dynamically
- ✓ No need to reassign DataSource

---

## Complex Data Types

### Enum Binding

```razor
<SfMultiColumnComboBox TValue="OrderStatus" TItem="OrderData" 
                       DataSource="@Orders"
                       TextField="Status"
                       ValueField="Status"
                       Placeholder="Select status">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Status" Header="Status"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private List<OrderData> Orders = new();

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
        public OrderStatus Status { get; set; }
    }

    protected override void OnInitialized()
    {
        Orders = new()
        {
            new OrderData { OrderID = 1001, Status = OrderStatus.Pending },
            new OrderData { OrderID = 1002, Status = OrderStatus.Shipped }
        };
    }
}
```

### Value Tuples

```csharp
@code {
    private List<(int Id, string Name, decimal Price)> Products = new();

    protected override void OnInitialized()
    {
        Products = new()
        {
            (1, "Product A", 99.99m),
            (2, "Product B", 149.99m),
            (3, "Product C", 199.99m)
        };
    }
}
```

### Dynamic Objects

```csharp
@code {
    private List<dynamic> Products = new();

    protected override void OnInitialized()
    {
        Products = new()
        {
            new { ProductID = 1, ProductName = "Item A", Category = "Electronics" },
            new { ProductID = 2, ProductName = "Item B", Category = "Books" }
        };
    }
}
```

### ExpandoObject

```csharp
@using System.Dynamic

@code {
    private List<ExpandoObject> Products = new();

    protected override void OnInitialized()
    {
        var product1 = new ExpandoObject();
        product1.ProductID = 1;
        product1.ProductName = "Product A";
        product1.Category = "Electronics";
        Products.Add(product1);
    }
}
```

---

## Web API Binding

### Using Data Adaptor

```razor
@page "/web-api-binding"
@using Syncfusion.Blazor.MultiColumnComboBox
@using Syncfusion.Blazor.Data

<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       Query="@GridQuery"
                       TextField="CustomerName"
                       ValueField="OrderID"
                       Placeholder="Select an order">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="CustomerName" Header="Customer"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="TotalAmount" Header="Amount" Format="C2"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private Query GridQuery = new Query().From("Orders").Select(new List<string> { "OrderID", "CustomerName", "TotalAmount" });

    public class OrderData
    {
        public int OrderID { get; set; }
        public string CustomerName { get; set; }
        public decimal TotalAmount { get; set; }
    }
}
```

### With HttpClient

```csharp
@inject HttpClient Http

@code {
    private List<OrderData> Orders = new();

    protected override async Task OnInitializedAsync()
    {
        try
        {
            Orders = await Http.GetFromJsonAsync<List<OrderData>>("https://api.example.com/api/orders");
        }
        catch (Exception ex)
        {
            // Handle error
            Console.WriteLine($"Error loading orders: {ex.Message}");
        }
    }

    public class OrderData
    {
        public int OrderID { get; set; }
        public string CustomerName { get; set; }
        public decimal TotalAmount { get; set; }
    }
}
```

---

## OData Services

### OData Binding

```razor
@page "/odata-binding"
@using Syncfusion.Blazor.MultiColumnComboBox
@using Syncfusion.Blazor.Data

<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       Query="@ODataQuery"
                       TextField="ProductName"
                       ValueField="ProductID"
                       Placeholder="Select a product">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Name"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private Query ODataQuery = new Query()
        .From("Products")
        .Select(new List<string> { "ProductID", "ProductName", "Category" })
        .Take(20);

    public class ProductData
    {
        public int ProductID { get; set; }
        public string ProductName { get; set; }
        public string Category { get; set; }
    }
}
```


## Action Events

### ActionBegin Event

Fires before data operations:

```razor
<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       OnActionBegin="@OnActionBegin">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private async Task OnActionBegin(ActionBeginEventArgs args)
    {
        // args.RequestType = filter, paging, sorting, etc.
        // Can be used for pre-processing
        Console.WriteLine($"Action: {args.RequestType}");
        await Task.CompletedTask;
    }
}
```

### ActionComplete Event

Fires after an action completes:

```razor
<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       OnActionComplete="@OnActionComplete">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private async Task OnActionComplete(ActionCompleteEventArgs<OrderData> args)
    {
        // args.Result = data returned
        // args.Count = total record count
        Console.WriteLine($"Completed: {args.Count} records");
        await Task.CompletedTask;
    }
}
```

### ActionFailure Event

Fires on error:

```razor
<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       OnActionFailure="@OnActionFailure">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private async Task OnActionFailure(Exception error)
    {
        // error = exception details
        Console.WriteLine($"Error: {error.Message
        Console.WriteLine($"Error: {args.Error}");
        await Task.CompletedTask;
    }
}
```

---

## Custom Adaptors

### Implement Custom Adaptor

```csharp
using Syncfusion.Blazor.Data;

public class CustomAdaptor : DataAdaptor
{
    private List<OrderData> Orders = new();

    public override async Task<object> Read(DataManagerRequest dm, string key = null)
    {
        // Load from database or API
        var allData = await FetchFromDatabase();

        // Apply filtering if needed
        if (dm.Where != null && dm.Where.Count > 0)
        {
            // Filter logic
        }

        // Apply paging
        if (dm.Skip > 0)
            allData = allData.Skip(dm.Skip).ToList();
        if (dm.Take > 0)
            allData = allData.Take(dm.Take).ToList();

        return new DataResult() 
        { 
            Result = allData, 
            Count = allData.Count 
        };
    }

    private async Task<List<OrderData>> FetchFromDatabase()
    {
        // Database call logic
        return new List<OrderData>();
    }

    public class OrderData
   MultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataManager="@DataManagerInstance">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private DataManager DataManagerInstance = new DataManager() 
    { 
        Adaptor = Adaptors.CustomAdaptor, 
        AdaptorInstance = typeof(CustomAdaptor) 
    }
### Use Custom Adaptor

```MultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataManager="@DataManagerInstance">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private DataManager DataManagerInstance = new DataManager() 
    { 
        Adaptor = Adaptors.CustomAdaptor, 
        AdaptorInstance = typeof(CustomAdaptor) 
    };
}
```

---

## Offline Mode

### Cache Data Locally

Enable offline mode for local caching:

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       Query="@GridQuery"
                       TextField="ProductName"
                       ValueField="ProductID"
                       Placeholder="Select product">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private Query GridQuery = new Query().From("Products");
}
```

**Benefits:**
- ✓ Data cached locally
- ✓ Works without internet temporarily
- ✓ Syncs when connection restored

---
ld names match exactly
- ✓ TextField and ValueField are set correctly

**Debug Example:**

```razor
<!-- Verify with debug output -->
@if (Products == null)
{
    <p>Products list is null</p>
}
else if (Products.Count == 0)
{
    <p>Products list is empty</p>
}
else
{
    <SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                           DataSource="@Products"
                           TextField="ProductName"
                           ValueField="ProductID">
        <MultiColumnComboboxColumns>
            <!-- Columns -->
        </MultiColumnComboboxColumns>
    </SfMultiColumnComboBox>
}
```

### Issue: API Call Not Triggering

**Solution:** Ensure API is configured correctly:

```csharp
protected override async Task OnInitializedAsync()
{
    try
    {
        var response = await Http.GetAsync("https://api.example.com/products");
        if (response.IsSuccessStatusCode)
   MultiColumnComboBox TValue="string" TItem="OrderData" 
                       DataSource="@Orders"
                       @bind-Value="@SelectedOrderID">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private int SelectedOrderID { get; set; } // Type mismatch!
}

<!-- ✅ CORRECT -->
<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       @bind-Value="@SelectedOrderID">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private int SelectedOrderID { get; set; }
}
```

**Solution:** Ensure TValue matches the @bind-Value property type.

```

---

## Next Steps

- **Filter data:** See [references/filtering-and-search.md](filtering-and-search.md)
- **Handle selection:** See [references/selection-and-values.md](selection-and-values.md)
- **Optimize performance:** See [references/features-and-settings.md](features-and-settings.md)
