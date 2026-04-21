# Filtering and Searching in Blazor ListView

## Table of Contents
- [Real-Time Search Implementation](#real-time-search-implementation)
- [Text Input Filtering](#text-input-filtering)
- [Filter Logic Patterns](#filter-logic-patterns)
- [Case-Insensitive Filtering](#case-insensitive-filtering)
- [Custom Filter Conditions](#custom-filter-conditions)

## Real-Time Search Implementation

### Basic Search with TextBox

```cshtml
@using Syncfusion.Blazor.Inputs
@using Syncfusion.Blazor.Lists

<div id="container">
    <div id="sample">
        <SfTextBox Placeholder="Filter" Input="@OnInput"></SfTextBox>
        <SfListView ID="list" DataSource="@ListData">
            <ListViewFieldSettings TValue="ListDataModel" Id="Id" Text="Text"></ListViewFieldSettings>
        </SfListView>
    </div>
</div>

@code
{
    List<ListDataModel> ListData = new List<ListDataModel>();
    List<ListDataModel> DataSource = new List<ListDataModel>() {
        new ListDataModel { Text = "Hennessey Venom", Id = "list-01" },
        new ListDataModel { Text = "Bugatti Chiron", Id = "list-02" },
        new ListDataModel { Text = "Bugatti Veyron Super Sport", Id = "list-03" },
        new ListDataModel { Text = "SSC Ultimate Aero", Id = "list-04" },
        new ListDataModel { Text = "Koenigsegg CCR", Id = "list-05" },
        new ListDataModel { Text = "McLaren F1", Id = "list-06" }
    };

    protected override void OnInitialized()
    {
        ListData = DataSource;
    }

    void OnInput(InputEventArgs eventArgs)
    {
        ListData = DataSource.FindAll(e => e.Text.ToLower().StartsWith(eventArgs.Value));
    }

    public class ListDataModel
    {
        public string Id { get; set; }
        public string Text { get; set; }
    }
}

<style>
    #list {
        box-shadow: 0 1px 4px #ddd;
        border-bottom: 1px solid #ddd;
    }

    #sample {
        height: 220px;
        margin: 0 auto;
        display: block;
        max-width: 350px;
    }
</style>
```

## Text Input Filtering

### Filter by Multiple Fields

```cshtml
@using Syncfusion.Blazor.Inputs
@using Syncfusion.Blazor.Lists

<div>
    <SfTextBox Placeholder="Search products..." Input="@OnSearch"></SfTextBox>
    <SfListView DataSource="@FilteredProducts" CssClass="e-list-template">
        <ListViewFieldSettings TValue="Product" Id="Id" Text="Name"></ListViewFieldSettings>
        <ListViewTemplates TValue="Product">
            <Template>
                <div class="e-list-wrapper e-list-multi-line">
                    <span class="e-list-item-header">@((context as Product).Name)</span>
                    <span class="e-list-content">Category: @((context as Product).Category)</span>
                    <span class="e-list-content">Price: $@((context as Product).Price)</span>
                </div>
            </Template>
        </ListViewTemplates>
    </SfListView>
</div>

@code {
    List<Product> FilteredProducts = new List<Product>();
    List<Product> AllProducts = new List<Product>
    {
        new Product { Id = "1", Name = "Laptop", Category = "Electronics", Price = 999.99m },
        new Product { Id = "2", Name = "Mouse", Category = "Electronics", Price = 29.99m },
        new Product { Id = "3", Name = "Desk Lamp", Category = "Furniture", Price = 49.99m },
        new Product { Id = "4", Name = "Monitor Stand", Category = "Furniture", Price = 79.99m },
        new Product { Id = "5", Name = "Wireless Keyboard", Category = "Electronics", Price = 89.99m }
    };

    protected override void OnInitialized()
    {
        FilteredProducts = AllProducts;
    }

    void OnSearch(InputEventArgs args)
    {
        string searchTerm = args.Value?.ToLower() ?? "";
        
        FilteredProducts = AllProducts.Where(p =>
            p.Name.ToLower().Contains(searchTerm) ||
            p.Category.ToLower().Contains(searchTerm) ||
            p.Price.ToString().Contains(searchTerm)
        ).ToList();
    }

    public class Product
    {
        public string Id { get; set; }
        public string Name { get; set; }
        public string Category { get; set; }
        public decimal Price { get; set; }
    }
}
```

## Filter Logic Patterns

### StartsWith Filter

Matches from beginning of text:

```csharp
void OnInput(InputEventArgs eventArgs)
{
    ListData = DataSource.FindAll(e => 
        e.Text.ToLower().StartsWith(eventArgs.Value.ToLower())
    );
}
```

### Contains Filter

Matches anywhere in text:

```csharp
void OnInput(InputEventArgs eventArgs)
{
    ListData = DataSource.FindAll(e => 
        e.Text.ToLower().Contains(eventArgs.Value.ToLower())
    );
}
```

### EndsWith Filter

Matches end of text:

```csharp
void OnInput(InputEventArgs eventArgs)
{
    ListData = DataSource.FindAll(e => 
        e.Text.ToLower().EndsWith(eventArgs.Value.ToLower())
    );
}
```

### Exact Match

```csharp
void OnInput(InputEventArgs eventArgs)
{
    ListData = DataSource.FindAll(e => 
        e.Text.Equals(eventArgs.Value, StringComparison.OrdinalIgnoreCase)
    );
}
```

## Case-Insensitive Filtering

Ensure searches work regardless of uppercase/lowercase:

```csharp
void OnSearch(InputEventArgs eventArgs)
{
    string searchTerm = eventArgs.Value?.ToLower() ?? "";
    
    FilteredProducts = AllProducts.Where(p =>
        p.Name.ToLower().Contains(searchTerm) ||
        p.Category.ToLower().Contains(searchTerm)
    ).ToList();
}
```

### Accent-Insensitive Filtering

For international characters:

```csharp
using System.Globalization;
using System.Text;

string NormalizeString(string text)
{
    string normalized = text.Normalize(NormalizationForm.FormD);
    var sb = new StringBuilder();
    foreach (char ch in normalized)
    {
        if (CharUnicodeInfo.GetUnicodeCategory(ch) != UnicodeCategory.NonSpacingMark)
        {
            sb.Append(ch);
        }
    }
    return sb.ToString();
}

void OnSearch(InputEventArgs eventArgs)
{
    string searchTerm = NormalizeString(eventArgs.Value?.ToLower() ?? "");
    
    FilteredProducts = AllProducts.Where(p =>
        NormalizeString(p.Name.ToLower()).Contains(searchTerm)
    ).ToList();
}
```

## Custom Filter Conditions

### Filter with Multiple Conditions

```cshtml
@using Syncfusion.Blazor.Inputs
@using Syncfusion.Blazor.Lists

<div>
    <div>
        <label>Search by Name:</label>
        <SfTextBox Input="@(e => OnNameSearch(e))"></SfTextBox>
    </div>
    <div>
        <label>Filter by Category:</label>
        <SfDropDownList Items="@Categories" ValueChanged="@((string c) => OnCategoryFilter(c))"></SfDropDownList>
    </div>
    <div>
        <label>Price Range:</label>
        <input type="number" placeholder="Min" @onchange="@((ChangeEventArgs e) => minPrice = decimal.Parse(e.Value.ToString()))" />
        <input type="number" placeholder="Max" @onchange="@((ChangeEventArgs e) => maxPrice = decimal.Parse(e.Value.ToString()))" />
        <button @onclick="ApplyFilters">Filter</button>
    </div>

    <SfListView DataSource="@FilteredProducts">
        <ListViewFieldSettings TValue="Product" Id="Id" Text="Name"></ListViewFieldSettings>
    </SfListView>
</div>

@code {
    List<Product> FilteredProducts = new List<Product>();
    List<Product> AllProducts = new List<Product>
    {
        new Product { Id = "1", Name = "Laptop", Category = "Electronics", Price = 999.99m },
        new Product { Id = "2", Name = "Mouse", Category = "Electronics", Price = 29.99m },
        new Product { Id = "3", Name = "Desk", Category = "Furniture", Price = 299.99m }
    };

    List<string> Categories = new List<string> { "Electronics", "Furniture", "All" };
    
    string searchText = "";
    string selectedCategory = "All";
    decimal minPrice = 0;
    decimal maxPrice = 10000;

    protected override void OnInitialized()
    {
        FilteredProducts = AllProducts;
    }

    void OnNameSearch(InputEventArgs args)
    {
        searchText = args.Value?.ToLower() ?? "";
        ApplyFilters();
    }

    void OnCategoryFilter(string category)
    {
        selectedCategory = category;
        ApplyFilters();
    }

    void ApplyFilters()
    {
        FilteredProducts = AllProducts.Where(p =>
            (string.IsNullOrEmpty(searchText) || p.Name.ToLower().Contains(searchText)) &&
            (selectedCategory == "All" || p.Category == selectedCategory) &&
            (p.Price >= minPrice && p.Price <= maxPrice)
        ).ToList();
    }

    public class Product
    {
        public string Id { get; set; }
        public string Name { get; set; }
        public string Category { get; set; }
        public decimal Price { get; set; }
    }
}
```

### Debounced Search (Performance Optimization)

```csharp
private System.Timers.Timer searchTimer;
private string pendingSearch = "";

protected override void OnInitialized()
{
    searchTimer = new System.Timers.Timer(500); // 500ms debounce
    searchTimer.Elapsed += async (s, e) => 
    {
        await InvokeAsync(() => PerformSearch(pendingSearch));
    };
    searchTimer.AutoReset = false;
}

void OnSearch(InputEventArgs eventArgs)
{
    pendingSearch = eventArgs.Value;
    searchTimer.Stop();
    searchTimer.Start();
}

void PerformSearch(string searchTerm)
{
    FilteredProducts = AllProducts.Where(p =>
        p.Name.ToLower().Contains(searchTerm.ToLower())
    ).ToList();
}
```

---

**Related Topics:**
- [Data Binding and Sources](data-binding-and-source.md) - Manage data dynamically
- [Item Selection and Actions](item-selection-and-actions.md) - Work with filtered selections
- [Grouping and Organization](grouping-and-organization.md) - Combine filtering with grouping

