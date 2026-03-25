# Filtering and Grouping

## Table of Contents
- [Enabling Filtering](#enabling-filtering)
- [Filter Configuration](#filter-configuration)
- [Custom Filtering](#custom-filtering)
- [Remote Filtering](#remote-filtering)
- [Data Grouping](#data-grouping)
- [Advanced Grouping](#advanced-grouping)
- [Performance Optimization](#performance-optimization)

## Enabling Filtering

### Basic Search/Filter

```csharp
@page "/filtering-basic"
@using Syncfusion.Blazor.DropDowns

<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item"
               AllowFiltering="true"
               Placeholder="Type to search">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>

@code {
    private List<Item> Items { get; set; } = new();

    protected override void OnInitialized()
    {
        Items = new List<Item>
        {
            new Item { ID = "1", Name = "Apple" },
            new Item { ID = "2", Name = "Apricot" },
            new Item { ID = "3", Name = "Banana" },
            new Item { ID = "4", Name = "Blueberry" }
        };
    }

    public class Item { public string ID { get; set; } public string Name { get; set; } }
}
```

**When to use:** Lists with 10+ items where searching improves UX.

## Filter Configuration

### Placeholder and Debounce

```csharp
<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item"
               AllowFiltering="true"
               FilterBarPlaceholder="Search employees"
               FilterType="FilterType.Contains"
               Placeholder="Select employees">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>

@code {
    private List<Item> Items { get; set; } = new();
}
```

### Filter Types

| FilterType | Behavior | Example |
|-----------|----------|---------|
| `StartsWith` | Match from beginning | "app" matches "Apple" |
| `EndsWith` | Match at end | "ple" matches "Apple" |
| `Contains` | Match anywhere (default) | "pp" matches "Apple" |

```csharp
<SfMultiSelect DataSource="@Items" 
               AllowFiltering="true"
               FilterType="FilterType.StartsWith">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>
```

### Case-Sensitive Filtering

```csharp
<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item"
               AllowFiltering="true"
               IgnoreCase="false">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>
```

## Custom Filtering

### Filter on Multiple Fields

```csharp
@page "/custom-filtering"
@using Syncfusion.Blazor.DropDowns

<SfMultiSelect DataSource="@FilteredItems" 
               TValue="List<string>"
               TItem="Employee"
               AllowFiltering="true"
               Placeholder="Search by name or department">
    <MultiSelectFieldSettings Text="Name" Value="EmployeeID"></MultiSelectFieldSettings>
    <MultiSelectEvents TValue="List<string>" TItem="Employee" 
                       Filtering="OnFiltering">
    </MultiSelectEvents>
</SfMultiSelect>

@code {
    private List<Employee> AllItems { get; set; } = new();
    private List<Employee> FilteredItems { get; set; } = new();

    protected override void OnInitialized()
    {
        AllItems = new List<Employee>
        {
            new Employee { EmployeeID = "1", Name = "Alice", Department = "IT" },
            new Employee { EmployeeID = "2", Name = "Bob", Department = "HR" },
            new Employee { EmployeeID = "3", Name = "Charlie", Department = "IT" }
        };
        FilteredItems = AllItems;
    }

    private void OnFiltering(FilteringEventArgs args)
    {
        args.PreventDefaultAction = true;

        // Filter on Name OR Department
        FilteredItems = AllItems
            .Where(x => x.Name.Contains(args.Text, StringComparison.OrdinalIgnoreCase) ||
                       x.Department.Contains(args.Text, StringComparison.OrdinalIgnoreCase))
            .ToList();
    }

    public class Employee 
    { 
        public string EmployeeID { get; set; } 
        public string Name { get; set; }
        public string Department { get; set; }
    }
}
```

### Custom Filter Logic

```csharp
private void OnFiltering(FilteringEventArgs args)
{
    args.PreventDefaultAction = true;
    
    // Custom logic: Filter by price range if numeric input
    if (int.TryParse(args.Text, out int maxPrice))
    {
        FilteredItems = AllItems
            .Where(x => x.Price <= maxPrice)
            .ToList();
    }
    else
    {
        // Standard text search
        FilteredItems = AllItems
            .Where(x => x.Name.Contains(args.Text, StringComparison.OrdinalIgnoreCase))
            .ToList();
    }
}
```

## Remote Filtering

### Filter from API

```csharp
@page "/remote-filtering"
@using Syncfusion.Blazor.DropDowns
@inject HttpClient Http

<SfMultiSelect DataSource="@RemoteData" 
               TValue="List<string>"
               TItem="ApiItem"
               AllowFiltering="true"
               Placeholder="Search products">
    <MultiSelectFieldSettings Text="ProductName" Value="ProductID"></MultiSelectFieldSettings>
    <MultiSelectEvents TValue="List<string>" TItem="ApiItem" 
                       Filtering="OnRemoteFiltering">
    </MultiSelectEvents>
</SfMultiSelect>

@code {
    private List<ApiItem> RemoteData { get; set; } = new();

    private async Task OnRemoteFiltering(FilteringEventArgs args)
    {
        args.PreventDefaultAction = true;

        try
        {
            // Call API with search term
            string url = $"https://api.example.com/products/search?term={Uri.EscapeDataString(args.Text)}";
            RemoteData = await Http.GetFromJsonAsync<List<ApiItem>>(url);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Filter error: {ex.Message}");
        }
    }

    public class ApiItem
    {
        public string ProductID { get; set; }
        public string ProductName { get; set; }
    }
}
```

### Debounced API Calls

```csharp
@page "/debounced-filtering"
@using Syncfusion.Blazor.DropDowns
@inject HttpClient Http

<SfMultiSelect DataSource="@RemoteData" 
               TValue="List<string>"
               TItem="ApiItem"
               AllowFiltering="true"
               Placeholder="Search (with delay)">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
    <MultiSelectEvents TValue="List<string>" TItem="ApiItem" 
                       Filtering="OnFiltering">
    </MultiSelectEvents>
</SfMultiSelect>

@code {
    private List<ApiItem> RemoteData { get; set; } = new();
    private Timer DebounceTimer { get; set; }

    private void OnFiltering(FilteringEventArgs args)
    {
        args.PreventDefaultAction = true;

        // Clear previous timer
        DebounceTimer?.Dispose();

        // Set new debounce timer (300ms delay)
        DebounceTimer = new Timer(
            async _ => await PerformSearch(args.Text),
            null,
            300,
            Timeout.Infinite
        );
    }

    private async Task PerformSearch(string searchTerm)
    {
        if (string.IsNullOrEmpty(searchTerm))
        {
            RemoteData = new List<ApiItem>();
            return;
        }

        try
        {
            RemoteData = await Http.GetFromJsonAsync<List<ApiItem>>(
                $"https://api.example.com/items?search={Uri.EscapeDataString(searchTerm)}"
            );
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Search failed: {ex.Message}");
        }
    }

    public class ApiItem { public string ID { get; set; } public string Name { get; set; } }
}
```

## Data Grouping

### Enable Grouping

```csharp
@page "/grouping-basic"
@using Syncfusion.Blazor.DropDowns

<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item"
               AllowGrouping="true"
               Placeholder="Select items (grouped)">
    <MultiSelectFieldSettings Text="Name" Value="ID" GroupBy="Category"></MultiSelectFieldSettings>
</SfMultiSelect>

@code {
    private List<Item> Items { get; set; } = new();

    protected override void OnInitialized()
    {
        Items = new List<Item>
        {
            new Item { ID = "1", Name = "Apple", Category = "Fruit" },
            new Item { ID = "2", Name = "Banana", Category = "Fruit" },
            new Item { ID = "3", Name = "Carrot", Category = "Vegetable" },
            new Item { ID = "4", Name = "Lettuce", Category = "Vegetable" }
        };
    }

    public class Item 
    { 
        public string ID { get; set; } 
        public string Name { get; set; }
        public string Category { get; set; }
    }
}
```

**Output:**
```
Fruit
  ☐ Apple
  ☐ Banana
Vegetable
  ☐ Carrot
  ☐ Lettuce
```

### Grouping with Sorting

```csharp
<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item"
               AllowGrouping="true"
               SortOrder="SortOrder.Ascending"
               Placeholder="Sorted groups">
    <MultiSelectFieldSettings Text="Name" Value="ID" GroupBy="Category"></MultiSelectFieldSettings>
</SfMultiSelect>

@code {
    private List<Item> Items { get; set; } = new();

    protected override void OnInitialized()
    {
        Items = new List<Item>
        {
            new Item { ID = "5", Name = "Zebra", Category = "Animal" },
            new Item { ID = "1", Name = "Apple", Category = "Fruit" },
            new Item { ID = "3", Name = "Carrot", Category = "Vegetable" }
        };
        // Groups appear in sorted order: Animal, Fruit, Vegetable
    }

    public class Item { /* ... */ }
}
```

## Advanced Grouping

### Custom Group Headers

```csharp
@page "/custom-group-template"
@using Syncfusion.Blazor.DropDowns

<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item"
               AllowGrouping="true"
               Placeholder="Custom group headers">
    <MultiSelectFieldSettings Text="Name" Value="ID" GroupBy="Category"></MultiSelectFieldSettings>
    <MultiSelectTemplates TItem="Item">
        <GroupTemplate>
            <div>
                <span style="font-weight: bold; color: #2196F3;">
                    @((context as IGroupsData<Item>).Key) 
                    (@((context as IGroupsData<Item>).Items.Count) items)
                </span>
            </div>
        </GroupTemplate>
    </MultiSelectTemplates>
</SfMultiSelect>

@code {
    private List<Item> Items { get; set; } = new();

    public class Item { public string ID { get; set; } public string Name { get; set; } public string Category { get; set; } }
}
```

### Nested/Hierarchical Grouping

For multi-level grouping, group items in code before binding:

```csharp
@code {
    private List<Item> Items { get; set; } = new();

    protected override void OnInitialized()
    {
        var itemList = new List<Item>
        {
            new Item { ID = "1", Name = "Apple", Category = "Fruit", Subcategory = "Red" },
            new Item { ID = "2", Name = "Banana", Category = "Fruit", Subcategory = "Yellow" },
            new Item { ID = "3", Name = "Carrot", Category = "Vegetable", Subcategory = "Root" }
        };

        // Group by category, then by subcategory in code
        Items = itemList
            .GroupBy(x => x.Category)
            .SelectMany(g => g.OrderBy(x => x.Subcategory))
            .ToList();
    }

    public class Item 
    { 
        public string ID { get; set; } 
        public string Name { get; set; }
        public string Category { get; set; }
        public string Subcategory { get; set; }
    }
}
```

## Performance Optimization

### Combine Filtering and Grouping

```csharp
<SfMultiSelect DataSource="@FilteredAndGroupedItems" 
               TValue="List<string>"
               TItem="Item"
               AllowFiltering="true"
               AllowGrouping="true"
               FilterType="FilterType.Contains"
               Placeholder="Filter and grouped">
    <MultiSelectFieldSettings Text="Name" Value="ID" GroupBy="Category"></MultiSelectFieldSettings>
    <MultiSelectEvents TValue="List<string>" TItem="Item" 
                       Filtering="OnFiltering">
    </MultiSelectEvents>
</SfMultiSelect>

@code {
    private List<Item> AllItems { get; set; } = new();
    private List<Item> FilteredAndGroupedItems { get; set; } = new();

    protected override void OnInitialized()
    {
        AllItems = new List<Item>
        {
            new Item { ID = "1", Name = "Apple", Category = "Fruit" },
            new Item { ID = "2", Name = "Apricot", Category = "Fruit" }
        };
        FilteredAndGroupedItems = AllItems;
    }

    private void OnFiltering(FilteringEventArgs args)
    {
        args.PreventDefaultAction = true;

        // Apply filter
        FilteredAndGroupedItems = AllItems
            .Where(x => x.Name.Contains(args.Text, StringComparison.OrdinalIgnoreCase))
            .ToList();

        // Groups still apply on filtered data
    }

    public class Item { public string ID { get; set; } public string Name { get; set; } public string Category { get; set; } }
}
```

### Limit Grouped Results

```csharp
private void OnFiltering(FilteringEventArgs args)
{
    args.PreventDefaultAction = true;

    // Show only top 5 matches
    FilteredAndGroupedItems = AllItems
        .Where(x => x.Name.Contains(args.Text, StringComparison.OrdinalIgnoreCase))
        .Take(5)
        .ToList();
}
```

## Key Takeaways

✅ `AllowFiltering="true"` enables search  
✅ Use `FilterType` to control matching behavior  
✅ Implement `Filtering` event for custom logic  
✅ `AllowGrouping="true"` + `GroupBy` property organizes items  
✅ Debounce remote API calls to reduce server load  
✅ Combine filtering + grouping for powerful UX  

## Next Steps

- **Customize appearance** → See [Customization and Styling](customization-and-styling.md)
- **Handle events** → See [Events and API Reference](events-and-api.md)
- **Implement templates** → See [Accessibility and Templates](accessibility-and-templates.md)
