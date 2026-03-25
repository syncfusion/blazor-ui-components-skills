# Events & Callbacks

This guide covers all Query Builder events and how to use them to react to user interactions and component lifecycle changes.

## Table of Contents

- [Component Lifecycle Events](#component-lifecycle-events)
- [Rule Change Events](#rule-change-events)
- [Value Change Events](#value-change-events)
- [Event Patterns](#event-patterns)
- [Async Callbacks](#async-callbacks)

## Component Lifecycle Events

### Created Event

Fires when the Query Builder component has been initialized and is ready:

```razor
@using Syncfusion.Blazor.QueryBuilder

<SfQueryBuilder TValue="Order" DataSource="@Orders">
    <QueryBuilderEvents TValue="Order" Created="OnQueryBuilderCreated"></QueryBuilderEvents>
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private void OnQueryBuilderCreated() {
        Console.WriteLine("Query Builder initialized and ready!");
        // Initialize custom logic, set default values, etc.
    }
}
```

### Destroyed Event

Fires when the Query Builder component is being disposed:

```razor
@code {
    private void OnQueryBuilderDestroyed() {
        Console.WriteLine("Query Builder destroyed, cleaning up resources...");
        // Clean up subscriptions, timers, etc.
    }
}
```

### DataBound Event

Fires after the data source is populated and bound to the Query Builder:

```razor
<SfQueryBuilder TValue="Order" DataSource="@Orders">
    <QueryBuilderEvents TValue="Order" DataBound="OnDataBound"></QueryBuilderEvents>
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private void OnDataBound() {
        Console.WriteLine("Data bound to Query Builder");
        // Apply initial filters, update UI
    }
}
```

## Rule Change Events

### RuleChanged Event

Fires whenever a rule or group is added, removed, or modified:

```razor
<SfQueryBuilder TValue="Order" DataSource="@Orders">
    <QueryBuilderEvents TValue="Order" RuleChanged="OnRuleChanged"></QueryBuilderEvents>
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Status" Label="Status" Type="ColumnType.String"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private void OnRuleChanged(RuleChangeEventArgs args) {
        // Action: "add", "remove", or "update"
        Console.WriteLine($"Action: {args.Action}");
        
        // Rule/Group ID
        Console.WriteLine($"ID: {args.RuleID}");
        
        // Get the modified rule
        if (args.Rule != null) {
            Console.WriteLine($"Field: {args.Rule.Field}");
            Console.WriteLine($"Operator: {args.Rule.Operator}");
            Console.WriteLine($"Value: {args.Rule.Value}");
        }
    }
}
```

**RuleChangeEventArgs properties:**
- `Action` - "add", "remove", or "update"
- `RuleID` - ID of the affected rule
- `Rule` - The RuleModel that was changed

### Changed Event

Fires after a condition (AND/OR), field, operator, or value change is applied:

```razor
<SfQueryBuilder TValue="Order" DataSource="@Orders">
    <QueryBuilderEvents TValue="Order" Changed="OnChanged"></QueryBuilderEvents>
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Status" Label="Status" Type="ColumnType.String"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private void OnChanged(ChangeEventArgs args) {
        Console.WriteLine("Query has changed");
        Console.WriteLine($"Previous value: {args.PreviousValue}");
        Console.WriteLine($"Value: {args.Value}");
        
        // Update filtered results
        ApplyCurrentFilter();
    }
    
    private void ApplyCurrentFilter() {
        // Re-apply filter based on updated query
    }
}
```

## Value Change Events

### OnValueChange Event

Fires BEFORE a condition, field, operator, or value is changed. Use this to validate or react before committing:

```razor
<SfQueryBuilder TValue="Order" DataSource="@Orders">
    <QueryBuilderEvents TValue="Order" OnValueChange="OnValueChanging"></QueryBuilderEvents>
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Amount" Label="Amount" Type="ColumnType.Number"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private void OnValueChanging(ChangeEventArgs args) {
        // Validate the new value
        if (args.Value is int newValue && newValue < 0) {
            Console.WriteLine("Invalid: Cannot use negative values");
            args.Cancel = true;  // Prevent the change
        }
    }
}
```

**Use cases for OnValueChange:**
- Validate input before committing
- Prevent invalid combinations
- Show confirmation dialogs
- Log user actions

## Event Patterns

### Multiple Event Handlers

Handle different events in a single component:

```razor
<SfQueryBuilder TValue="Order" DataSource="@Orders">
    <QueryBuilderEvents TValue="Order" 
        Created="OnCreated"
        DataBound="OnDataBound"
        RuleChanged="OnRuleChanged"
        Changed="OnChanged">
    </QueryBuilderEvents>
    <QueryBuilderColumns>
        <!-- columns -->
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private void OnCreated() {
        Console.WriteLine("1. Component created");
    }
    
    private void OnDataBound() {
        Console.WriteLine("2. Data bound");
    }
    
    private void OnRuleChanged(RuleChangeEventArgs args) {
        Console.WriteLine($"3. Rule {args.Action}: {args.RuleID}");
    }
    
    private void OnChanged(ChangeEventArgs args) {
        Console.WriteLine($"4. Changed: {args.Value}");
    }
}
```

### Event Chaining

```razor
@code {
    private void OnRuleChanged(RuleChangeEventArgs args) {
        // When rule changes, trigger multiple actions
        OnRuleChanged_UpdateUI(args);
        OnRuleChanged_ValidateQuery(args);
        OnRuleChanged_PersistToDatabase(args);
    }
    
    private void OnRuleChanged_UpdateUI(RuleChangeEventArgs args) {
        Console.WriteLine("Updating UI...");
    }
    
    private void OnRuleChanged_ValidateQuery(RuleChangeEventArgs args) {
        Console.WriteLine("Validating query...");
    }
    
    private void OnRuleChanged_PersistToDatabase(RuleChangeEventArgs args) {
        Console.WriteLine("Saving to database...");
    }
}
```

## Async Callbacks

### Async Changed Handler

```razor
<SfQueryBuilder TValue="Order" DataSource="@Orders">
    <QueryBuilderEvents TValue="Order" Changed="OnChangedAsync"></QueryBuilderEvents>
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private async Task OnChangedAsync(ChangeEventArgs args) {
        // Perform async operations (API calls, database queries, etc.)
        var filteredData = await FetchFilteredDataAsync();
        FilteredOrders = filteredData;
        StateHasChanged();
    }
    
    private async Task<List<Order>> FetchFilteredDataAsync() {
        // Simulate API call
        await Task.Delay(500);
        return new List<Order>();
    }
    
    private List<Order> FilteredOrders;
}
```

### Debounced Event Handler

```razor
@code {
    private Timer DebounceTimer;
    
    private void OnChangedDebounced(ChangeEventArgs args) {
        // Cancel previous timer
        DebounceTimer?.Dispose();
        
        // Schedule action after 500ms delay
        DebounceTimer = new Timer(_ => {
            OnChangedDebounced_Execute();
        }, null, 500, Timeout.Infinite);
    }
    
    private void OnChangedDebounced_Execute() {
        Console.WriteLine("Executing debounced action...");
        // Perform expensive operation (API call, filtering, etc.)
    }
    
    public void Dispose() {
        DebounceTimer?.Dispose();
    }
}
```

## Real-World Example: Live Search Filter

```razor
@using Syncfusion.Blazor.QueryBuilder

<SfQueryBuilder TValue="Product" DataSource="@Products">
    <QueryBuilderEvents TValue="Product"
        Created="OnCreated"
        Changed="OnChanged">
    </QueryBuilderEvents>
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="Category" Label="Category" Type="ColumnType.String"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Price" Label="Price" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="InStock" Label="In Stock" Type="ColumnType.Boolean"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

<h4>Results: @FilteredResults?.Count ?? 0 products</h4>
<ul>
    @foreach (var item in FilteredResults ?? new()) {
        <li>@item.Name - $@item.Price</li>
    }
</ul>

@code {
    private SfQueryBuilder<Product> QueryBuilder;
    private List<Product> Products;
    private List<Product> FilteredResults;
    private bool IsLoading = false;
    
    protected override void OnInitialized() {
        Products = GetAllProducts();
        FilteredResults = Products;
    }
    
    private void OnCreated() {
        Console.WriteLine("Query Builder ready");
    }
    
    private async Task OnChanged(ChangeEventArgs args) {
        IsLoading = true;
        StateHasChanged();
        
        // Simulate processing
        await Task.Delay(300);
        
        // Apply filter
        FilteredResults = FilterProducts();
        IsLoading = false;
        StateHasChanged();
    }
    
    private List<Product> FilterProducts() {
        // Implementation of filtering logic
        return Products;
    }
    
    private List<Product> GetAllProducts() {
        return new() {
            new() { Name = "Laptop", Price = 999, InStock = true, Category = "Electronics" },
            new() { Name = "Mouse", Price = 29, InStock = true, Category = "Electronics" },
            new() { Name = "Desk", Price = 299, InStock = false, Category = "Furniture" }
        };
    }
    
    public class Product {
        public string Name { get; set; }
        public decimal Price { get; set; }
        public bool InStock { get; set; }
        public string Category { get; set; }
    }
}
```

## Next Steps

- [Data Binding](data-binding.md) - Apply filtered queries to data
- [Advanced Features](advanced-features.md) - Import/export queries
- [Troubleshooting](troubleshooting.md) - Common event issues
