# Advanced Scenarios

## Table of Contents
- [Form Integration and Validation](#form-integration-and-validation)
- [Localization and Globalization](#localization-and-globalization)
- [Custom Value Creation](#custom-value-creation)
- [Performance Optimization](#performance-optimization)
- [Multiple Dropdown Instances](#multiple-dropdown-instances)
- [Server vs Client Filtering](#server-vs-client-filtering)
- [Real-World Use Cases](#real-world-use-cases)
- [Edge Cases and Troubleshooting](#edge-cases-and-troubleshooting)

## Form Integration and Validation

### Validation in EditForm

```csharp
@page "/form-validation"
@using Syncfusion.Blazor.DropDowns
@using System.ComponentModel.DataAnnotations

<EditForm Model="FormData" OnValidSubmit="HandleSubmit">
    <DataAnnotationsValidator />
    
    <div class="form-group">
        <label>Select at least 2 employees:</label>
        <SfMultiSelect DataSource="@Employees" 
                       @bind-Value="FormData.SelectedEmployees"
                       TValue="List<string>"
                       TItem="Employee"
                       Placeholder="Select employees">
            <MultiSelectFieldSettings Text="Name" Value="EmployeeID"></MultiSelectFieldSettings>
        </SfMultiSelect>
        <ValidationMessage For="@(() => FormData.SelectedEmployees)" />
    </div>
    
    <button type="submit" class="btn btn-primary">Submit</button>
</EditForm>

@code {
    private List<Employee> Employees { get; set; } = new();
    private FormModel FormData { get; set; } = new();

    protected override void OnInitialized()
    {
        Employees = new List<Employee>
        {
            new Employee { EmployeeID = "1", Name = "Alice" },
            new Employee { EmployeeID = "2", Name = "Bob" },
            new Employee { EmployeeID = "3", Name = "Charlie" }
        };
    }

    private async Task HandleSubmit()
    {
        // Process form submission
        await Task.Delay(500);
        Console.WriteLine($"Submitted: {string.Join(", ", FormData.SelectedEmployees)}");
    }

    public class FormModel
    {
        [Required(ErrorMessage = "Please select employees")]
        [MinLength(2, ErrorMessage = "Select at least 2 employees")]
        public List<string> SelectedEmployees { get; set; } = new();
    }

    public class Employee
    {
        public string EmployeeID { get; set; }
        public string Name { get; set; }
    }
}
```

### Custom Validation

```csharp
public class TeamSelectionValidator
{
    [Required(ErrorMessage = "At least one person required")]
    [MinLength(1)]
    [MaxLength(10, ErrorMessage = "Cannot exceed 10 people")]
    public List<string> SelectedMembers { get; set; } = new();

    [CustomValidation(typeof(TeamSelectionValidator), nameof(ValidateTeamDiversity))]
    public string TeamDiversity { get; set; }

    public static ValidationResult ValidateTeamDiversity(string value, ValidationContext context)
    {
        var instance = context.ObjectInstance as TeamSelectionValidator;
        if (instance?.SelectedMembers?.Count > 5)
        {
            return new ValidationResult("Large teams require manager approval");
        }
        return ValidationResult.Success;
    }
}
```

## Localization and Globalization

### Multi-Language Support

```csharp
@page "/localization"
@using Syncfusion.Blazor.DropDowns
@inject IJSRuntime JS

<div>
    <button @onclick="() => SetLanguage('en')">English</button>
    <button @onclick="() => SetLanguage('es')">Español</button>
    <button @onclick="() => SetLanguage('fr')">Français</button>
</div>

<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item"
               Placeholder="@GetPlaceholder(CurrentLanguage)">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>

@code {
    private List<Item> Items { get; set; } = new();
    private string CurrentLanguage { get; set; } = "en";

    private Dictionary<string, string> Placeholders = new()
    {
        { "en", "Select items" },
        { "es", "Seleccionar elementos" },
        { "fr", "Sélectionner les éléments" }
    };

    protected override void OnInitialized()
    {
        Items = new List<Item>
        {
            new Item { ID = "1", Name = "Item 1" },
            new Item { ID = "2", Name = "Item 2" }
        };
    }

    private void SetLanguage(string lang)
    {
        CurrentLanguage = lang;
        // Update items for language
    }

    private string GetPlaceholder(string lang)
    {
        return Placeholders.ContainsKey(lang) ? Placeholders[lang] : Placeholders["en"];
    }

    public class Item { public string ID { get; set; } public string Name { get; set; } }
}
```

### RTL (Right-to-Left) Languages

```csharp
@page "/rtl-arabic"
@using Syncfusion.Blazor.DropDowns

<div dir="rtl" style="text-align: right;">
    <h3>اختر الموظفين</h3>
    
    <SfMultiSelect DataSource="@Employees" 
                   TValue="List<string>"
                   TItem="Employee"
                   EnableRtl="true"
                   Placeholder="اختر موظف">
        <MultiSelectFieldSettings Text="Name" Value="EmployeeID"></MultiSelectFieldSettings>
    </SfMultiSelect>
</div>

@code {
    private List<Employee> Employees { get; set; } = new();

    protected override void OnInitialized()
    {
        Employees = new List<Employee>
        {
            new Employee { EmployeeID = "1", Name = "أحمد محمد" },
            new Employee { EmployeeID = "2", Name = "فاطمة علي" },
            new Employee { EmployeeID = "3", Name = "محمود حسن" }
        };
    }

    public class Employee { public string EmployeeID { get; set; } public string Name { get; set; } }
}
```

## Custom Value Creation

### Tag Input with Dynamic Values

```csharp
@page "/custom-tags"
@using Syncfusion.Blazor.DropDowns

<SfMultiSelect @bind-Value="SelectedTags"
               DataSource="@TagList" 
               TValue="List<string>"
               TItem="Tag"
               AllowCustomValue="true"
               Placeholder="Type or select tags">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
    <MultiSelectEvents TValue="List<string>" TItem="Tag"
                       CustomValueSelection="OnCustomTag">
    </MultiSelectEvents>
</SfMultiSelect>

<div>
    <h4>Tags:</h4>
    @foreach (var tag in SelectedTags)
    {
        <span class="badge badge-primary">@tag</span>
    }
</div>

@code {
    private List<string> SelectedTags { get; set; } = new();
    private List<Tag> TagList { get; set; } = new();

    protected override void OnInitialized()
    {
        TagList = new List<Tag>
        {
            new Tag { ID = "1", Name = "Important" },
            new Tag { ID = "2", Name = "Urgent" },
            new Tag { ID = "3", Name = "Follow-up" }
        };
    }

    private void OnCustomTag(CustomValueEventArgs args)
    {
        // Automatically add custom tag
        if (!SelectedTags.Contains(args.NewValue))
        {
            SelectedTags.Add(args.NewValue);
        }
    }

    public class Tag { public string ID { get; set; } public string Name { get; set; } }
}
```

## Performance Optimization

### Lazy Loading Large Datasets

```csharp
@page "/lazy-loading"
@using Syncfusion.Blazor.DropDowns

<SfMultiSelect DataSource="@VisibleItems" 
               TValue="List<string>"
               TItem="Item"
               EnableVirtualization="true"
               ItemsCount="50"
               Placeholder="Scroll to load more">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
    <MultiSelectEvents TValue="List<string>" TItem="Item"
                       Filtering="OnFilter">
    </MultiSelectEvents>
</SfMultiSelect>

@code {
    private List<Item> AllItems { get; set; } = new();
    private List<Item> VisibleItems { get; set; } = new();
    private int PageSize = 100;
    private int CurrentPage = 1;

    protected override void OnInitialized()
    {
        // Generate large dataset (10,000 items)
        AllItems = Enumerable.Range(1, 10000)
            .Select(i => new Item { ID = i.ToString(), Name = $"Item {i}" })
            .ToList();

        LoadPage(1);
    }

    private void OnFilter(FilteringEventArgs args)
    {
        args.PreventDefaultAction = true;
        
        VisibleItems = AllItems
            .Where(x => x.Name.Contains(args.Text, StringComparison.OrdinalIgnoreCase))
            .Take(100)
            .ToList();
    }

    private void LoadPage(int page)
    {
        var skip = (page - 1) * PageSize;
        VisibleItems = AllItems.Skip(skip).Take(PageSize).ToList();
    }

    public class Item { public string ID { get; set; } public string Name { get; set; } }
}
```

### Memoization for Filter Results

```csharp
private Dictionary<string, List<Item>> FilterCache { get; set; } = new();

private void OnFilter(FilteringEventArgs args)
{
    if (FilterCache.ContainsKey(args.Text))
    {
        VisibleItems = FilterCache[args.Text];
    }
    else
    {
        VisibleItems = AllItems
            .Where(x => x.Name.Contains(args.Text, StringComparison.OrdinalIgnoreCase))
            .ToList();
        
        FilterCache[args.Text] = VisibleItems;
    }
}
```

## Multiple Dropdown Instances

### Master-Detail Pattern

```csharp
@page "/master-detail"
@using Syncfusion.Blazor.DropDowns

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">
    <!-- Master Dropdown -->
    <div>
        <h4>Categories</h4>
        <SfMultiSelect @bind-Value="SelectedCategories"
                       DataSource="@Categories" 
                       TValue="List<string>"
                       TItem="Category"
                       Change="OnCategoryChange"
                       Placeholder="Select categories">
            <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
        </SfMultiSelect>
    </div>

    <!-- Detail Dropdown -->
    <div>
        <h4>Products in selected categories</h4>
        <SfMultiSelect @bind-Value="SelectedProducts"
                       DataSource="@FilteredProducts" 
                       TValue="List<string>"
                       TItem="Product"
                       Placeholder="Select products">
            <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
        </SfMultiSelect>
    </div>
</div>

@code {
    private List<string> SelectedCategories { get; set; } = new();
    private List<string> SelectedProducts { get; set; } = new();
    private List<Category> Categories { get; set; } = new();
    private List<Product> AllProducts { get; set; } = new();
    private List<Product> FilteredProducts { get; set; } = new();

    protected override void OnInitialized()
    {
        Categories = new List<Category>
        {
            new Category { ID = "1", Name = "Electronics" },
            new Category { ID = "2", Name = "Clothing" }
        };

        AllProducts = new List<Product>
        {
            new Product { ID = "1", Name = "Laptop", CategoryID = "1" },
            new Product { ID = "2", Name = "Mouse", CategoryID = "1" },
            new Product { ID = "3", Name = "Shirt", CategoryID = "2" }
        };

        FilteredProducts = AllProducts;
    }

    private void OnCategoryChange(MultiSelectChangeEventArgs<List<string>> args)
    {
        if (args.Value?.Count > 0)
        {
            FilteredProducts = AllProducts
                .Where(p => args.Value.Contains(p.CategoryID))
                .ToList();
        }
        else
        {
            FilteredProducts = AllProducts;
        }
    }

    public class Category { public string ID { get; set; } public string Name { get; set; } }
    public class Product 
    { 
        public string ID { get; set; } 
        public string Name { get; set; }
        public string CategoryID { get; set; }
    }
}
```

## Server vs Client Filtering

### Client-Side Filtering (Recommended for small data)

```csharp
<SfMultiSelect DataSource="@LocalData" 
               AllowFiltering="true"
               Placeholder="Client-side filter">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>

@code {
    private List<Item> LocalData { get; set; } = new();
}
```

**Pros:** Fast, no server calls  
**Cons:** Loads all data upfront

### Server-Side Filtering (For large datasets)

```csharp
<SfMultiSelect DataSource="@ServerData" 
               AllowFiltering="true">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
    <MultiSelectEvents TValue="List<string>" TItem="Item"
                       Filtering="OnServerFilter">
    </MultiSelectEvents>
</SfMultiSelect>

@code {
    private List<Item> ServerData { get; set; } = new();

    private async Task OnServerFilter(FilteringEventArgs args)
    {
        args.PreventDefaultAction = true;
        
        // Call API with filter term
        ServerData = await Http.GetFromJsonAsync<List<Item>>(
            $"https://api.example.com/items?filter={Uri.EscapeDataString(args.Text)}"
        );
    }

    @inject HttpClient Http
}
```

**Pros:** Handles large datasets  
**Cons:** Network latency

## Real-World Use Cases

### Employee Selection for Payroll

```csharp
private async Task ProcessPayroll(List<string> selectedEmployeeIds)
{
    var employees = AllEmployees.Where(e => selectedEmployeeIds.Contains(e.ID)).ToList();
    
    foreach (var emp in employees)
    {
        var salary = CalculateSalary(emp);
        await UpdatePayrollRecord(emp.ID, salary);
    }
}
```

### Permission/Role Assignment

```csharp
<SfMultiSelect DataSource="@AvailableRoles" 
               @bind-Value="UserRoles"
               TValue="List<string>"
               TItem="Role"
               ShowCheckbox="true">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>
```

### Bulk Actions on Selected Items

```csharp
<button @onclick="() => BulkDelete(SelectedItems)" 
        class="btn btn-danger" 
        disabled="@(SelectedItems.Count == 0)">
    Delete Selected (@SelectedItems.Count)
</button>

@code {
    private async Task BulkDelete(List<string> itemIds)
    {
        if (!await ConfirmDelete()) return;
        
        foreach (var id in itemIds)
        {
            await DeleteItemAsync(id);
        }
        
        SelectedItems.Clear();
    }
}
```

## Edge Cases and Troubleshooting

### Handling Null Values

```csharp
private void SafeHandleSelection(MultiSelectChangeEventArgs<List<string>> args)
{
    var selected = args.Value ?? new List<string>();
    Console.WriteLine($"Selected {selected.Count} items");
}
```

### Large Selection Count Performance

```csharp
// Limit selections programmatically
private void OnSelectionChange(MultiSelectChangeEventArgs<List<string>> args)
{
    const int MaxSelections = 50;
    
    if (args.Value?.Count > MaxSelections)
    {
        args.Value.RemoveAt(args.Value.Count - 1);  // Remove latest
        ErrorMessage = $"Maximum {MaxSelections} items allowed";
    }
}
```

### Preventing Duplicate Selection

```csharp
private void ValidateNoDuplicates(List<Item> dataSource, List<string> selected)
{
    var uniqueSelected = selected.Distinct().ToList();
    if (uniqueSelected.Count != selected.Count)
    {
        // Handle duplicate detection
    }
}
```

### Handling Rapid API Calls

```csharp
private Timer DebounceTimer { get; set; }

private void OnFiltering(FilteringEventArgs args)
{
    DebounceTimer?.Dispose();
    DebounceTimer = new Timer(
        _ => FetchFilteredData(args.Text),
        null,
        500,  // 500ms delay
        Timeout.Infinite
    );
}
```

## Key Takeaways

✅ Use `EditForm` with validation attributes  
✅ Enable RTL with `EnableRtl="true"`  
✅ `AllowCustomValue` for dynamic tag creation  
✅ Virtual scrolling for 1000+ items  
✅ Server filtering for API-driven data  
✅ Debounce remote API calls  
✅ Cache filter results to reduce server load  
✅ Handle null/empty selections gracefully  

## Next Steps

- **Basic setup** → See [Getting Started](getting-started.md)
- **Events handling** → See [Events and API Reference](events-and-api.md)
- **Styling options** → See [Customization and Styling](customization-and-styling.md)
