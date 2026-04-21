# Data Binding and Sources in Blazor ListView

## Table of Contents
- [Overview](#overview)
- [Local Data Arrays](#local-data-arrays)
- [Field Mapping](#field-mapping)
- [Remote Data with DataManager](#remote-data-with-datamanager)
- [Entity Framework Integration](#entity-framework-integration)
- [Dynamic Data Updates](#dynamic-data-updates)

## Overview

ListView supports multiple data sources:
- **Local Arrays** - Direct C# collections
- **DataManager** - Remote services (OData, Web API, REST)
- **Entity Framework** - Database-first or code-first
- **Dynamic Collections** - Real-time updates with ObservableCollection

## Local Data Arrays

### Array of JSON Data

Bind simple data with `ListViewFieldSettings`:

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView DataSource="@Data">
    <ListViewFieldSettings TValue="DataModel" Id="Id" Text="Text"></ListViewFieldSettings>
</SfListView>

@code {
    public string HeaderTitle = "Listview";

    List<DataModel> Data = new List<DataModel>();

    protected override void OnInitialized()
    {
        base.OnInitialized();
        Data.Add(new DataModel { Text = "Hennessey Venom", Id = "list-01" });
        Data.Add(new DataModel { Text = "Bugatti Chiron", Id = "list-02" });
        Data.Add(new DataModel { Text = "Bugatti Veyron Super Sport", Id = "list-03" });
        Data.Add(new DataModel { Text = "SSC Ultimate Aero", Id = "list-04" });
        Data.Add(new DataModel { Text = "Koenigsegg CCR", Id = "list-05" });
    }

    public class DataModel
    {
        public string Id { get; set; }
        public string Text { get; set; }
    }
}
```

## Field Mapping

Use `ListViewFieldSettings` to map complex data:

```cshtml
<SfListView DataSource="@Products">
    <ListViewFieldSettings TValue="Product" 
                          Id="ProductId" 
                          Text="ProductName"
                          Tooltip="Description"
                          IconCss="IconClass">
    </ListViewFieldSettings>
</SfListView>

@code {
    public class Product
    {
        public int ProductId { get; set; }
        public string ProductName { get; set; }
        public string Description { get; set; }
        public string IconClass { get; set; }
        public decimal Price { get; set; }
    }

    List<Product> Products = new List<Product>
    {
        new Product 
        { 
            ProductId = 1, 
            ProductName = "Laptop", 
            Description = "High-performance laptop",
            IconClass = "e-icon-device",
            Price = 999.99m
        },
        new Product 
        { 
            ProductId = 2, 
            ProductName = "Mouse", 
            Description = "Wireless mouse",
            IconClass = "e-icon-hardware",
            Price = 29.99m
        }
    };
}
```

### Available Field Settings

| Property | Type | Description |
|----------|------|-------------|
| `Id` | string | Map unique identifier |
| `Text` | string | Displayed text field |
| `Tooltip` | string | Hover tooltip field |
| `IconCss` | string | CSS class for icons |
| `Child` | string | Nested items field |
| `GroupBy` | string | Grouping category field |
| `IsChecked` | string | Checkbox state field |
| `Enabled` | string | Enabled/disabled state |
| `HtmlAttributes` | string | HTML attributes field |

## Remote Data with DataManager

### OData V4 Integration

```cshtml
@using Syncfusion.Blazor.Lists
@using Syncfusion.Blazor.Data

<SfListView HeaderTitle="Products" ShowHeader="true" TValue="Product" Query="@query">
    <ListViewFieldSettings TValue="Product" Id="ProductID" Text="ProductName"></ListViewFieldSettings>
    <SfDataManager Url="url" 
                   Adaptor="Adaptors.ODataV4Adaptor">
    </SfDataManager>
</SfListView>

@code {
    public static List<string> column = new List<string>()
    {
        "ProductID","ProductName"
    };
    Query query = new Query().From("Products").Select(column).Take(6);
    
    public class Product
    {
        public int ProductID { get; set; }
        public string ProductName { get; set; }
    }
}
```

### Web API Integration

```cshtml
@using Syncfusion.Blazor.Lists
@using Syncfusion.Blazor.Data

<SfListView TValue="Employee" Query="@query">
    <ListViewFieldSettings TValue="Employee" Id="EmployeeID" Text="FirstName"></ListViewFieldSettings>
    <SfDataManager Url="url" Adaptor="Adaptors.WebApiAdaptor"></SfDataManager>
</SfListView>

@code {
    Query query = new Query().Take(10);
    
    public class Employee
    {
        public int EmployeeID { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
    }
}
```

## Entity Framework Integration

### Handle CRUD Operations

Create a data access layer for Entity Framework:

```csharp
using YourApp.Shared.Models;
using Microsoft.EntityFrameworkCore;

public class DataAccessLayer
{
    private DataContext db = new DataContext();

    // READ
    public DbSet<Product> GetAllProducts()
    {
        try
        {
            return db.Products;
        }
        catch
        {
            throw;
        }
    }

    // CREATE
    public void AddProduct(Product product)
    {
        try
        {
            db.Products.Add(product);
            db.SaveChanges();
        }
        catch
        {
            throw;
        }
    }

    // DELETE
    public void DeleteProduct(int id)
    {
        try
        {
            var product = db.Products.Find(id);
            if (product != null)
            {
                db.Products.Remove(product);
                db.SaveChanges();
            }
        }
        catch
        {
            throw;
        }
    }

    // UPDATE
    public void UpdateProduct(Product product)
    {
        try
        {
            db.Products.Update(product);
            db.SaveChanges();
        }
        catch
        {
            throw;
        }
    }
}```

### Use Entity Framework Data in ListView

```cshtml
@using YourApp.Shared.Models
@inject DataAccessLayer DataService

<SfListView DataSource="@Products" ShowHeader="true" HeaderTitle="Products">
    <ListViewFieldSettings TValue="Product" Id="ProductId" Text="ProductName"></ListViewFieldSettings>
    <ListViewTemplates TValue="Product">
        <Template>
            <div class="e-list-wrapper e-list-multi-line">
                <span class="e-list-item-header">@((context as Product).ProductName)</span>
                <span class="e-list-content">Price: $@((context as Product).Price)</span>
            </div>
        </Template>
    </ListViewTemplates>
</SfListView>

@code {
    List<Product> Products = new List<Product>();

    protected override async Task OnInitializedAsync()
    {
        var dbProducts = DataService.GetAllProducts();
        Products = dbProducts.ToList();
    }
}
```

### Adding Items from Dialog

```cshtml
@using Syncfusion.Blazor.Popups;
@using Syncfusion.Blazor.Buttons;

<button class="e-btn" @onclick="ShowAddDialog">Add Product</button>

<SfDialog @ref="DialogObj" Target="#container" ShowCloseIcon="true" Header="Add Product" @bind-Visible="@DialogVisible" Width="300px">
    <DialogTemplates>
        <Content>
            <input id="prodName" class="e-input" type="text" placeholder="Product Name" @bind-value="@ProductName" />
            <input id="prodPrice" class="e-input" type="number" placeholder="Price" @bind-value="@ProductPrice" />
        </Content>
    </DialogTemplates>
    <DialogButtons>
        <DialogButton OnClick="@AddProductAsync" Content="Add" IsPrimary="true" CssClass="e-flat"></DialogButton>
    </DialogButtons>
</SfDialog>

@code {
    SfDialog DialogObj;
    bool DialogVisible;
    string ProductName = "";
    decimal ProductPrice = 0;

    async Task ShowAddDialog()
    {
        DialogVisible = true;
        await DialogObj.ShowAsync();
    }

    async Task AddProductAsync()
    {
        var newProduct = new Product { ProductName = ProductName, Price = ProductPrice };
        DataService.AddProduct(newProduct);
        
        ProductName = "";
        ProductPrice = 0;
        DialogVisible = false;
        
        // Refresh list
        var dbProducts = DataService.GetAllProducts();
        Products = dbProducts.ToList();
        StateHasChanged();
    }
}```

### Delete Item

```cshtml
<ListViewTemplates TValue="Product">
    <Template>
        @{
            Product current = (Product)context;
            <div class="flex-container">
                <span>@current.ProductName</span>
                <button class="e-btn e-small" @onclick="@(e => DeleteProduct(current.ProductId))">Delete</button>
            </div>
        }
    </Template>
</ListViewTemplates>

@code {
    async Task DeleteProduct(int id)
    {
        DataService.DeleteProduct(id);
        
        // Refresh list
        var dbProducts = DataService.GetAllProducts();
        Products = dbProducts.ToList();
        StateHasChanged();
    }
}
```

## Dynamic Data Updates

### Using ObservableCollection

For real-time updates, use `ObservableCollection`:

```cshtml
@using System.Collections.ObjectModel
@using Syncfusion.Blazor.Lists

<SfListView DataSource="@Items">
    <ListViewFieldSettings TValue="Item" Id="Id" Text="Name"></ListViewFieldSettings>
</SfListView>

<button class="e-btn" @onclick="AddItem">Add Item</button>

@code {
    ObservableCollection<Item> Items = new ObservableCollection<Item>
    {
        new Item { Id = "1", Name = "Item 1" },
        new Item { Id = "2", Name = "Item 2" }
    };

    void AddItem()
    {
        Items.Add(new Item { Id = DateTime.Now.Ticks.ToString(), Name = "New Item" });
    }

    public class Item
    {
        public string Id { get; set; }
        public string Name { get; set; }
    }
}
```

### Refresh Data

Update the list by reassigning the DataSource:

```csharp
// After modifying data
Products = newProductList;
StateHasChanged();  // Trigger re-render
```

### Append Remote Data

```cshtml
<SfListView @ref="ListView" DataSource="@ListData"></SfListView>

<button @onclick="LoadMoreData">Load More</button>

@code {
    SfListView<Product> ListView;
    List<Product> ListData = new List<Product>();
    int pageSize = 10;
    int pageNumber = 1;

    async Task LoadMoreData()
    {
        var moreData = await FetchDataAsync(pageNumber++, pageSize);
        ListData.AddRange(moreData);
        await ListView.RefreshAsync();
    }

    async Task<List<Product>> FetchDataAsync(int page, int size)
    {
        // Fetch from API
        return await http.GetJsonAsync<List<Product>>($"api/products?page={page}&size={size}");
    }
}
```

---

**Related Topics:**
- [Item Selection and Actions](item-selection-and-actions.md) - Get selected items from data
- [Filtering and Searching](filtering-and-searching.md) - Filter data dynamically
- [Grouping and Organization](grouping-and-organization.md) - Group data by category

