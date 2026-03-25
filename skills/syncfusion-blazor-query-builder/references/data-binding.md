# Data Binding & Filtered Results

This guide explains how to bind data to the Query Builder, extract filtered results, and apply queries dynamically.

## Basic Data Binding

### Bind Data Source

```razor
@using Syncfusion.Blazor.QueryBuilder

<SfQueryBuilder TValue="Product" DataSource="@Products">
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="ProductID" Label="ID" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Name" Label="Product Name" Type="ColumnType.String"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Price" Label="Price" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Category" Label="Category" Type="ColumnType.String"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private List<Product> Products = new() {
        new() { ProductID = 1, Name = "Laptop", Price = 999.99m, Category = "Electronics" },
        new() { ProductID = 2, Name = "Mouse", Price = 29.99m, Category = "Electronics" },
        new() { ProductID = 3, Name = "Desk", Price = 299.99m, Category = "Furniture" }
    };

    public class Product {
        public int ProductID { get; set; }
        public string Name { get; set; }
        public decimal Price { get; set; }
        public string Category { get; set; }
    }
}
```

### Update Data Dynamically

```razor
@code {
    private async Task LoadProductsFromApi() {
        // Simulate API call
        Products = await FetchProductsFromServer();
        StateHasChanged();  // Notify UI of changes
    }
    
    private async Task<List<Product>> FetchProductsFromServer() {
        // Replace with actual API call
        await Task.Delay(500);
        return new() {
            new() { ProductID = 1, Name = "Laptop", Price = 999.99m, Category = "Electronics" },
            new() { ProductID = 2, Name = "Monitor", Price = 299.99m, Category = "Electronics" }
        };
    }
}
```

## Extracting Filtered Results

### Get Query Rules

Retrieve the current query structure as a rule model:

```razor
<SfButton OnClick="GetFilteredQuery">Get Query</SfButton>

<SfQueryBuilder TValue="Product" @ref="QueryBuilder" DataSource="@Products">
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="Category" Label="Category" Type="ColumnType.String"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Price" Label="Price" Type="ColumnType.Number"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private SfQueryBuilder<Product> QueryBuilder;
    
    private void GetFilteredQuery() {
        // Get the current query rules
        var rules = QueryBuilder.GetRules();
        Console.WriteLine($"Query: {JsonConvert.SerializeObject(rules)}");
    }
}
```

### Filter Data Using Query

Apply the query to filter your data collection:

```razor
<SfButton OnClick="ApplyFilter">Apply Filter</SfButton>
<SfButton OnClick="ShowResults">Show Results</SfButton>

<h4>Filtered Results</h4>
@if (FilteredProducts != null && FilteredProducts.Count > 0) {
    <ul>
        @foreach (var product in FilteredProducts) {
            <li>@product.Name - $@product.Price</li>
        }
    </ul>
}

@code {
    private SfQueryBuilder<Product> QueryBuilder;
    private List<Product> FilteredProducts;
    
    private void ApplyFilter() {
        var rules = QueryBuilder.GetRules();
        FilteredProducts = ApplyQueryToData(Products, rules);
        StateHasChanged();
    }
    
    private List<Product> ApplyQueryToData(List<Product> data, RuleModel rules) {
        // Simple filtering logic (customize based on your query structure)
        var filtered = data;
        
        if (rules?.Rules != null) {
            foreach (var rule in rules.Rules) {
                if (rule.Field == "Category" && rule.Operator == "equal") {
                    filtered = filtered.Where(p => p.Category == rule.Value.ToString()).ToList();
                } else if (rule.Field == "Price" && rule.Operator == "lessthan") {
                    var price = decimal.Parse(rule.Value.ToString());
                    filtered = filtered.Where(p => p.Price < price).ToList();
                }
            }
        }
        
        return filtered;
    }
}
```

## Complex Data Filtering

### Multi-Condition Filtering

```razor
@code {
    private List<Product> FilterWithComplexLogic(List<Product> data, RuleModel rules) {
        return data.Where(product => EvaluateRule(product, rules)).ToList();
    }
    
    private bool EvaluateRule(Product product, RuleModel rule) {
        // Handle nested groups
        if (rule.Rules != null && rule.Rules.Count > 0) {
            if (rule.Condition == "and") {
                return rule.Rules.All(r => EvaluateRule(product, r));
            } else if (rule.Condition == "or") {
                return rule.Rules.Any(r => EvaluateRule(product, r));
            }
        }
        
        // Evaluate single rule
        return EvaluateSingleRule(product, rule);
    }
    
    private bool EvaluateSingleRule(Product product, RuleModel rule) {
        var fieldValue = GetFieldValue(product, rule.Field);
        
        return rule.Operator switch {
            "equal" => fieldValue.Equals(rule.Value),
            "notequal" => !fieldValue.Equals(rule.Value),
            "lessthan" => CompareNumeric(fieldValue, rule.Value) < 0,
            "greaterthan" => CompareNumeric(fieldValue, rule.Value) > 0,
            "contains" => fieldValue.ToString().Contains(rule.Value.ToString()),
            "notcontains" => !fieldValue.ToString().Contains(rule.Value.ToString()),
            "startswith" => fieldValue.ToString().StartsWith(rule.Value.ToString()),
            "endswith" => fieldValue.ToString().EndsWith(rule.Value.ToString()),
            _ => false
        };
    }
    
    private object GetFieldValue(Product product, string field) {
        return field switch {
            "ProductID" => product.ProductID,
            "Name" => product.Name,
            "Price" => product.Price,
            "Category" => product.Category,
            _ => null
        };
    }
    
    private int CompareNumeric(object a, object b) {
        if (decimal.TryParse(a.ToString(), out var numA) && 
            decimal.TryParse(b.ToString(), out var numB)) {
            return numA.CompareTo(numB);
        }
        return 0;
    }
}
```

## Event-Driven Filtering

### Real-Time Filter Updates

```razor
<SfQueryBuilder TValue="Order" @ref="QueryBuilder" DataSource="@Orders">
    <QueryBuilderEvents TValue="Order" RuleChanged="OnQueryChanged"></QueryBuilderEvents>
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Status" Label="Status" Type="ColumnType.String"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Amount" Label="Amount" Type="ColumnType.Number"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

<h4>Results: @FilteredOrders?.Count ?? 0 orders</h4>

@code {
    private SfQueryBuilder<Order> QueryBuilder;
    private List<Order> FilteredOrders;
    
    private void OnQueryChanged(RuleChangeEventArgs args) {
        // Automatically apply filter when query changes
        var rules = QueryBuilder.GetRules();
        FilteredOrders = ApplyQueryToOrders(Orders, rules);
        StateHasChanged();
    }
    
    private List<Order> ApplyQueryToOrders(List<Order> data, RuleModel rules) {
        // Apply filtering logic
        return data;  // Simplified
    }
}
```

## Working with Filtered Results

### Display and Export

```razor
<SfButton OnClick="ExportFiltered">Export Results</SfButton>

@if (FilteredResults != null) {
    <table>
        <thead>
            <tr>
                <th>ID</th>
                <th>Name</th>
                <th>Price</th>
            </tr>
        </thead>
        <tbody>
            @foreach (var item in FilteredResults) {
                <tr>
                    <td>@item.ProductID</td>
                    <td>@item.Name</td>
                    <td>@item.Price</td>
                </tr>
            }
        </tbody>
    </table>
}

@code {
    private List<Product> FilteredResults;
    
    private async Task ExportFiltered() {
        var json = JsonConvert.SerializeObject(FilteredResults);
        var bytes = Encoding.UTF8.GetBytes(json);
        
        // Trigger download
        await JS.InvokeVoidAsync("downloadFile", 
            "filtered-results.json", 
            Convert.ToBase64String(bytes));
    }
    
    @inject IJSRuntime JS;
}
```

### Count & Statistics

```razor
@code {
    private void ShowStatistics(List<Product> filtered) {
        var totalPrice = filtered.Sum(p => p.Price);
        var avgPrice = filtered.Average(p => p.Price);
        var maxPrice = filtered.Max(p => p.Price);
        
        Console.WriteLine($"Total items: {filtered.Count}");
        Console.WriteLine($"Total value: ${totalPrice:F2}");
        Console.WriteLine($"Average price: ${avgPrice:F2}");
        Console.WriteLine($"Highest price: ${maxPrice:F2}");
    }
}
```

## Performance Considerations

### Virtual Scrolling for Large Datasets

```razor
<SfQueryBuilder TValue="Order" DataSource="@Orders" EnableVirtualization="true">
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>
```

### Pagination

```razor
@code {
    private int PageSize = 50;
    private int CurrentPage = 1;
    
    private List<Product> GetPagedResults(List<Product> filtered) {
        return filtered
            .Skip((CurrentPage - 1) * PageSize)
            .Take(PageSize)
            .ToList();
    }
}
```

## Next Steps

- [Events & Callbacks](events-and-callbacks.md) - Handle data binding events
- [Advanced Features](advanced-features.md) - Import/export and persistence
- [Troubleshooting](troubleshooting.md) - Performance and data issues
