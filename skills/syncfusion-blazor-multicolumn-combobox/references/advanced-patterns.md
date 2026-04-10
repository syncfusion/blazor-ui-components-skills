# Advanced Patterns and Integration

## Table of Contents
- [Server-Side Integration](#server-side-integration)
- [Web App Integration](#web-app-integration)
- [Custom Data Adaptors](#custom-data-adaptors)
- [Dynamic Object Binding](#dynamic-object-binding)
- [Enum Data Binding](#enum-data-binding)
- [Offline Mode](#offline-mode)
- [Performance Optimization](#performance-optimization)
- [Advanced Scenarios](#advanced-scenarios)
- [Examples](#examples)

---

## Server-Side Integration

### Blazor Server-Side

MultiColumn ComboBox automatically works with server-side Blazor:

```razor
@page "/server-multicolumn-combobox"
@using Syncfusion.Blazor.MultiColumnComboBox

<h3>Server-Side MultiColumn ComboBox</h3>

<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
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
    private List<OrderData> Orders = new();

    protected override async Task OnInitializedAsync()
    {
        // Load from server-side database
        Orders = await FetchOrdersFromDatabase();
    }

    private async Task<List<OrderData>> FetchOrdersFromDatabase()
    {
        // Database query via EF Core
        return new List<OrderData>
        {
            new OrderData { OrderID = 1001, CustomerName = "Alice", TotalAmount = 150.00m },
            new OrderData { OrderID = 1002, CustomerName = "Bob", TotalAmount = 250.00m }
        };
    }

    public class OrderData
    {
        public int OrderID { get; set; }
        public string CustomerName { get; set; }
        public decimal TotalAmount { get; set; }
    }
}
```

### Real-Time Updates with SignalR

```csharp
@implements IAsyncDisposable
@using Microsoft.AspNetCore.SignalR.Client
@using Syncfusion.Blazor.MultiColumnComboBox

@page "/realtime-multicolumn-combobox"

<SfMultiColumnComboBox TValue="int" TItem="LiveOrderData" 
                       DataSource="@LiveOrders"
                       TextField="Status"
                       ValueField="OrderID">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Status" Header="Status"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private HubConnection hubConnection;
    private List<LiveOrderData> LiveOrders = new();

    protected override async Task OnInitializedAsync()
    {
        hubConnection = new HubConnectionBuilder()
            .WithUrl(Navigation.ToAbsoluteUri("/orderhub"))
            .WithAutomaticReconnect()
            .Build();

        hubConnection.On<LiveOrderData>("ReceiveOrder", async (order) =>
        {
            LiveOrders.Add(order);
            await InvokeAsync(() => StateHasChanged());
        });

        await hubConnection.StartAsync();
    }

    async ValueTask IAsyncDisposable.DisposeAsync()
    {
        if (hubConnection is not null)
        {
            await hubConnection.DisposeAsync();
        }
    }

    public class LiveOrderData
    {
        public int OrderID { get; set; }
        public string Status { get; set; }
    }
}
```

---

## Web App Integration

### Using with ASP.NET Core Endpoints

```csharp
// Program.cs
builder.Services.AddScoped<OrderService>();

// OrderService.cs
public class OrderService
{
    private readonly HttpClient _http;

    public OrderService(HttpClient http)
    {
        _http = http;
    }

    public async Task<List<OrderData>> GetOrdersAsync()
    {
        return await _http.GetFromJsonAsync<List<OrderData>>("/api/orders");
    }
}

// Page.razor
@inject OrderService OrderService
@using Syncfusion.Blazor.MultiColumnComboBox

<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       TextField="CustomerName"
                       ValueField="OrderID">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="CustomerName" Header="Customer"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private List<OrderData> Orders = new();

    protected override async Task OnInitializedAsync()
    {
        Orders = await OrderService.GetOrdersAsync();
    }
}
```

### API Endpoint

```csharp
[ApiController]
[Route("api/[controller]")]
public class OrdersController : ControllerBase
{
    private readonly IOrderService _orderService;

    [HttpGet]
    public async Task<IActionResult> GetOrders()
    {
        var orders = await _orderService.GetAllOrdersAsync();
        return Ok(orders);
    }

    [HttpGet("search")]
    public async Task<IActionResult> SearchOrders([FromQuery] string searchTerm)
    {
        var orders = await _orderService.SearchOrdersAsync(searchTerm);
        return Ok(orders);
    }
}
```

---

## Custom Data Adaptors

### Implement IDataService

```csharp
public interface ICustomDataAdaptor
{
    Task<List<T>> GetDataAsync<T>(DataManagerRequest dm);
}

public class DatabaseAdaptor : ICustomDataAdaptor
{
    private readonly IDbContextFactory<ApplicationDbContext> _contextFactory;

    public DatabaseAdaptor(IDbContextFactory<ApplicationDbContext> contextFactory)
    {
        _contextFactory = contextFactory;
    }

    public async Task<List<T>> GetDataAsync<T>(DataManagerRequest dm) where T : class
    {
        using var context = _contextFactory.CreateDbContext();
        
        var query = context.Set<T>().AsQueryable();

        // Apply filtering
        if (dm.Where != null && dm.Where.Count > 0)
        {
            foreach (var filter in dm.Where)
            {
                query = ApplyFilter(query, filter);
            }
        }

        // Apply paging
        if (dm.Skip > 0)
            query = query.Skip(dm.Skip);
        if (dm.Take > 0)
            query = query.Take(dm.Take);

        return await query.ToListAsync();
    }

    private IQueryable<T> ApplyFilter<T>(IQueryable<T> query, WhereFilter filter) where T : class
    {
        // Custom filter logic
        return query;
    }
}
```

### Use Custom Adaptor

```razor
@inject ICustomDataAdaptor CustomAdaptor
@using Syncfusion.Blazor.MultiColumnComboBox

<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@FilteredOrders"
                       TextField="CustomerName"
                       ValueField="OrderID"
                       Filtering="@OnFiltering">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="CustomerName" Header="Customer"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private List<OrderData> FilteredOrders = new();

    private async Task OnFiltering(FilteringEventArgs args)
    {
        args.PreventDefaultAction = true;
        
        var dm = new DataManagerRequest { Skip = 0, Take = 20 };
        FilteredOrders = await CustomAdaptor.GetDataAsync<OrderData>(dm);
    }
}
```

---

## Dynamic Object Binding

### Anonymous Types

```razor
@using Syncfusion.Blazor.MultiColumnComboBox

<SfMultiColumnComboBox TValue="int" TItem="dynamic" 
                       DataSource="@DynamicData"
                       TextField="ProductName"
                       ValueField="ProductID"
                       Placeholder="Select item">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private List<dynamic> DynamicData = new();

    protected override void OnInitialized()
    {
        DynamicData = new()
        {
            new { ProductID = 1, ProductName = "Item A" },
            new { ProductID = 2, ProductName = "Item B" }
        };
    }
}
```

### ExpandoObject

```razor
@using System.Dynamic
@using Syncfusion.Blazor.MultiColumnComboBox

<SfMultiColumnComboBox TValue="string" TItem="ExpandoObject" 
                       DataSource="@ExpandoData"
                       TextField="Name"
                       ValueField="Name">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="Name" Header="Name"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Price" Header="Price"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private List<ExpandoObject> ExpandoData = new();

    protected override void OnInitialized()
    {
        dynamic item1 = new ExpandoObject();
        item1.ID = 1;
        item1.Name = "Product A";
        item1.Price = 99.99;

        ExpandoData.Add(item1);
    }
}
```

---

## Enum Data Binding

### Status Enumeration

```razor
@using Syncfusion.Blazor.MultiColumnComboBox

<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       TextField="Status"
                       ValueField="OrderID"
                       Placeholder="Select status">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Status" Header="Status"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    public enum OrderStatus
    {
        [Display(Name = "Pending")]
        Pending = 0,
        
        [Display(Name = "Processing")]
        Processing = 1,
        
        [Display(Name = "Shipped")]
        Shipped = 2,
        
        [Display(Name = "Delivered")]
        Delivered = 3,
        
        [Display(Name = "Cancelled")]
        Cancelled = 4
    }

    public class OrderData
    {
        public int OrderID { get; set; }
        public OrderStatus Status { get; set; }
    }

    private List<OrderData> Orders = new();
}
```

---

## Offline Mode

### Cache Data Locally

```razor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.MultiColumnComboBox

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
    private Query GridQuery = new Query().From("Products").Select(new List<string> { "ProductID", "ProductName" });
}
```

**Behavior:**
- ✓ Data cached on first load
- ✓ Works offline if cached
- ✓ Auto-syncs when online

---

## Performance Optimization

### Virtual Scrolling with Large Datasets

```razor
@using Syncfusion.Blazor.MultiColumnComboBox

<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@Orders"
                       EnableVirtualization="true"
                       AllowFiltering="true"
                       TextField="CustomerName"
                       ValueField="OrderID"
                       Placeholder="Search orders...">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="CustomerName" Header="Customer"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private List<OrderData> Orders = new();

    protected override void OnInitialized()
    {
        // Generate 10,000 items
        Orders = Enumerable.Range(1, 10000)
            .Select(i => new OrderData 
            { 
                OrderID = 1000 + i, 
                CustomerName = $"Customer {i}" 
            })
            .ToList();
    }
}
```

### Lazy Loading

```razor
@using Syncfusion.Blazor.MultiColumnComboBox

<SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                       DataSource="@FilteredOrders"
                       AllowFiltering="true"
                       Filtering="@OnFilter"
                       TextField="CustomerName"
                       ValueField="OrderID"
                       Placeholder="Type to load...">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="CustomerName" Header="Customer"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private List<OrderData> FilteredOrders = new();

    private async Task OnFilter(FilteringEventArgs args)
    {
        args.PreventDefaultAction = true;
        
        if (args.Text.Length < 3)
            return;

        // Load only matching records
        FilteredOrders = await SearchOrdersAsync(args.Text);
    }

    private async Task<List<OrderData>> SearchOrdersAsync(string searchTerm)
    {
        // API call with search term
        var response = await Http.GetFromJsonAsync<List<OrderData>>($"/api/orders/search?q={searchTerm}");
        return response;
    }
}
```

---

## Advanced Scenarios

### Cascading MultiColumn ComboBoxes

```razor
@using Syncfusion.Blazor.MultiColumnComboBox

<div class="form-group">
    <label>Category:</label>
    <SfMultiColumnComboBox TValue="int" TItem="CategoryData" 
                           DataSource="@Categories"
                           TextField="CategoryName"
                           ValueField="CategoryID"
                           @bind-Value="@SelectedCategoryID"
                           ValueChange="@OnCategoryChange">
        <MultiColumnComboboxColumns>
            <MultiColumnComboboxColumn Field="CategoryID" Header="ID"></MultiColumnComboboxColumn>
            <MultiColumnComboboxColumn Field="CategoryName" Header="Category"></MultiColumnComboboxColumn>
        </MultiColumnComboboxColumns>
    </SfMultiColumnComboBox>
</div>

<div class="form-group">
    <label>Product:</label>
    <SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                           DataSource="@FilteredProducts"
                           TextField="ProductName"
                           ValueField="ProductID"
                           Placeholder="Select a product">
        <MultiColumnComboboxColumns>
            <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
            <MultiColumnComboboxColumn Field="ProductName" Header="Product"></MultiColumnComboboxColumn>
        </MultiColumnComboboxColumns>
    </SfMultiColumnComboBox>
    </SfComboBox>
</div>

@code {
    private List<CategoryData> Categories = new();
    private List<ProductData> FilteredProducts = new();
    private int SelectedCategoryID { get; set; }

    private async Task OnCategoryChange(ChangeEventArgs<int, CategoryData> args)
    {
        // Update products based on category
        FilteredProducts = await GetProductsByCategory(args.Value);
    }

    private async Task<List<ProductData>> GetProductsByCategory(int categoryID)
    {
        return await Http.GetFromJsonAsync<List<ProductData>>($"/api/products?categoryId={categoryID}");
    }
}
```

### MultiColumn ComboBox with Custom Templates

```razor
@using Syncfusion.Blazor.MultiColumnComboBox

<SfMultiColumnComboBox TValue="int" TItem="EmployeeData" 
                       DataSource="@Employees"
                       TextField="EmployeeName"
                       ValueField="EmployeeID"
                       @bind-Value="@SelectedEmployeeID">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="EmployeeID" Header="ID" Width="80px"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="EmployeeName" Header="Name" Width="150px">
            <Template>
                @{
                    var employee = context as EmployeeData;
                    <div style="display: flex; align-items: center; gap: 10px;">
                        <img src="@employee.ProfileImage" alt="@employee.EmployeeName" style="width: 30px; height: 30px; border-radius: 50%;">
                        <div>
                            <strong>@employee.EmployeeName</strong>
                            <br />
                            <small>@employee.Department</small>
                        </div>
                    </div>
                }
            </Template>
        </MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="Department" Header="Department" Width="120px"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private int SelectedEmployeeID { get; set; }
    private List<EmployeeData> Employees = new();

    public class EmployeeData
    {
        public int EmployeeID { get; set; }
        public string EmployeeName { get; set; }
        public string Department { get; set; }
        public string ProfileImage { get; set; }
    }
}
```

---

## Examples

### Complete Advanced Integration

```razor
@page "/advanced-integration"
@using Syncfusion.Blazor.MultiColumnComboBox
@using System.Collections.ObjectModel

<h3>Advanced Order Management</h3>

<div class="row">
    <div class="col-md-6">
        <h5>Search with Real-Time Filtering</h5>
        <SfMultiColumnComboBox TValue="int" TItem="OrderData" 
                               DataSource="@FilteredOrders"
                               AllowFiltering="true"
                               Filtering="@OnFilter"
                               EnableVirtualization="true"
                               TextField="CustomerName"
                               ValueField="OrderID"
                               Placeholder="Search orders...">
            <MultiColumnComboboxColumns>
                <MultiColumnComboboxColumn Field="OrderID" Header="Order ID"></MultiColumnComboboxColumn>
                <MultiColumnComboboxColumn Field="CustomerName" Header="Customer"></MultiColumnComboboxColumn>
                <MultiColumnComboboxColumn Field="Status" Header="Status"></MultiColumnComboboxColumn>
            </MultiColumnComboboxColumns>
        </SfMultiColumnComboBox>
    </div>

    <div class="col-md-6">
        <h5>Cascading Selection</h5>
        <SfMultiColumnComboBox TValue="string" TItem="CategoryData" 
                               DataSource="@Categories"
                               TextField="CategoryID"
                               ValueField="CategoryID"
                               @bind-Value="@SelectedCategory"
                               ValueChange="@OnCategoryChanged">
            <MultiColumnComboboxColumns>
                <MultiColumnComboboxColumn Field="CategoryID" Header="Category"></MultiColumnComboboxColumn>
            </MultiColumnComboboxColumns>
        </SfMultiColumnComboBox>

        <SfMultiColumnComboBox TValue="int" TItem="SubcategoryData" 
                               DataSource="@FilteredSubcategories"
                               TextField="SubcategoryName"
                               ValueField="SubcategoryID"
                               Placeholder="Select subcategory">
            <MultiColumnComboboxColumns>
                <MultiColumnComboboxColumn Field="SubcategoryID" Header="Subcategory"></MultiColumnComboboxColumn>
                <MultiColumnComboboxColumn Field="SubcategoryName" Header="Name"></MultiColumnComboboxColumn>
            </MultiColumnComboboxColumns>
        </SfMultiColumnComboBox>
    </div>
</div>

@code {
    private List<OrderData> AllOrders = new();
    private List<OrderData> FilteredOrders = new();
    private List<CategoryData> Categories = new();
    private List<SubcategoryData> FilteredSubcategories = new();
    private string SelectedCategory { get; set; }

    protected override void OnInitialized()
    {
        // Initialize data
        AllOrders = GenerateOrders(1000);
        FilteredOrders = AllOrders;
        Categories = GenerateCategories();
    }

    private async Task OnFilter(FilteringEventArgs args)
    {
        args.PreventDefaultAction = true;
        
        var searchTerm = args.Text.ToLower();
        FilteredOrders = AllOrders
            .Where(o => o.OrderID.ToString().Contains(searchTerm) ||
                       o.CustomerName.ToLower().Contains(searchTerm))
            .ToList();
    }

    private async Task OnCategoryChanged(ChangeEventArgs<string, CategoryData> args)
    {
        FilteredSubcategories = await GetSubcategoriesAsync(args.Value);
    }

    private async Task<List<SubcategoryData>> GetSubcategoriesAsync(string categoryID)
    {
        return new List<SubcategoryData>();
    }

    private List<OrderData> GenerateOrders(int count)
    {
        return Enumerable.Range(1, count)
            .Select(i => new OrderData 
            { 
                OrderID = 1000 + i, 
                CustomerName = $"Customer {i}",
                Status = (OrderStatus)(i % 4)
            })
            .ToList();
    }

    private List<CategoryData> GenerateCategories()
    {
        return new() { new CategoryData { CategoryID = "1", CategoryName = "Electronics" } };
    }

    public class OrderData
    {
        public int OrderID { get; set; }
        public string CustomerName { get; set; }
        public OrderStatus Status { get; set; }
    }

    public class CategoryData
    {
        public string CategoryID { get; set; }
        public string CategoryName { get; set; }
    }

    public class SubcategoryData
    {
        public int SubcategoryID { get; set; }
        public string SubcategoryName { get; set; }
    }

    public enum OrderStatus { Pending, Processing, Shipped, Delivered }
}
```

---

## Next Steps

- **Accessibility:** See [references/accessibility-localization.md](accessibility-localization.md)
- **Styling:** See [references/styling-and-appearance.md](styling-and-appearance.md)
- **Complete examples:** Check parent SKILL.md for guided patterns
