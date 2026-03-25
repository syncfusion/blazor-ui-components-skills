# Troubleshooting

This guide helps you diagnose and resolve common Query Builder issues.

## Installation & Setup Issues

### Error: "Query Builder component not rendering"

**Symptoms:** Component appears blank or missing from UI

**Solutions:**
1. Verify CSS theme is linked:
   ```html
   <link rel="stylesheet" href="_content/Syncfusion.Blazor.Themes/bootstrap5.css">
   ```

2. Check Syncfusion services are registered in `Program.cs`:
   ```csharp
   builder.Services.AddSyncfusionBlazor();
   ```

3. Verify NuGet packages are installed:
   ```bash
   dotnet add package Syncfusion.Blazor.QueryBuilder
   ```

4. Check browser console (F12) for JavaScript errors

### Error: "Cannot find Syncfusion namespace"

**Solutions:**
1. Add using directive:
   ```razor
   @using Syncfusion.Blazor.QueryBuilder
   ```

2. Verify project file includes theme package:
   ```xml
   <PackageReference Include="Syncfusion.Blazor.Themes" Version="*" />
   ```

## Data Binding Issues

### Columns not appearing in dropdown

**Cause:** DataSource not bound or columns not configured

**Solution:**
```razor
<!-- Ensure both DataSource and Columns are set -->
<SfQueryBuilder TValue="Employee" DataSource="@Employees">
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="EmployeeID" Label="ID" Type="ColumnType.Number"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private List<Employee> Employees = new();  // Must not be null
    
    protected override void OnInitialized() {
        Employees = GetEmployees();  // Initialize in OnInitialized
    }
}
```

### DataSource updates don't reflect in Query Builder

**Solution:** Call `StateHasChanged()` after updating data:
```razor
@code {
    private async Task LoadData() {
        Employees = await FetchFromApi();
        StateHasChanged();  // Notify UI of changes
    }
}
```

### GetFilteredRecords returns null

**Cause:** Rules not properly extracted

**Solution:**
```razor
@code {
    private async Task GetFiltered() {
        try {
            var rules = await QueryBuilder.GetRules();
            
            if (rules != null) {
                var filtered = ApplyRulesToData(Data, rules);
                Console.WriteLine($"Filtered: {filtered.Count} records");
            } else {
                Console.WriteLine("No rules defined");
            }
        } catch (Exception ex) {
            Console.WriteLine($"Error: {ex.Message}");
        }
    }
}
```

## Event Issues

### Events not firing

**Symptoms:** RuleChanged, Changed events don't trigger

**Solutions:**

1. Verify event handler is defined:
   ```razor
   <QueryBuilderEvents TValue="Order" RuleChanged="OnRuleChanged"></QueryBuilderEvents>
   ```

2. Check handler method signature:
   ```razor
   @code {
       private void OnRuleChanged(RuleChangeEventArgs args) {
           // Correct signature
       }
       
       // ✗ Wrong - missing parameter
       // private void OnRuleChanged() { }
   }
   ```

3. Ensure event is triggered by user action (not programmatic changes)

### OnValueChange validation not preventing changes

**Solution:** Set `Cancel` property on event args:
```razor
@code {
    private void OnValueChanging(ChangeEventArgs args) {
        if (!IsValid(args.Value)) {
            args.Cancel = true;  // Prevent change
            Console.WriteLine("Invalid value, change cancelled");
        }
    }
}
```

## Performance Issues

### Query Builder slow with large datasets

**Symptoms:** Lag when opening dropdown or filtering

**Solutions:**

1. Enable virtual scrolling:
   ```razor
   <SfQueryBuilder TValue="Order" DataSource="@Orders" EnableVirtualization="true">
   </SfQueryBuilder>
   ```

2. Use pagination:
   ```razor
   @code {
       private int PageSize = 100;
       private List<Order> PagedOrders => Orders.Skip(0).Take(PageSize).ToList();
   }
   ```

3. Reduce initial data load:
   ```razor
   @code {
       protected override async Task OnInitializedAsync() {
           // Load only first 1000 records initially
           Orders = await FetchOrdersAsync(0, 1000);
       }
   }
   ```

### Memory leaks in long-running applications

**Solution:** Implement proper cleanup:
```razor
@implements IAsyncDisposable

@code {
    private SfQueryBuilder<Order> QueryBuilder;
    
    async ValueTask IAsyncDisposable.DisposeAsync() {
        if (QueryBuilder != null) {
            await QueryBuilder.DisposeAsync();
        }
    }
}
```

## Styling Issues

### Styles not applied to Query Builder

**Symptoms:** Custom CSS classes not working

**Solutions:**

1. Verify theme CSS is loaded before custom CSS:
   ```html
   <link rel="stylesheet" href="_content/Syncfusion.Blazor.Themes/bootstrap5.css">
   <link rel="stylesheet" href="css/custom.css">  <!-- After Syncfusion theme -->
   ```

2. Check CSS specificity:
   ```css
   /* Increase specificity if needed */
   .e-querybuilder .e-rule {
       background-color: #fff !important;
   }
   ```

3. Verify correct CSS class names:
   ```css
   /* ✓ Correct -->
   .e-querybuilder .e-rule { }
   
   /* ✗ Wrong -->
   .query-builder-rule { }
   ```

### Components not visible on mobile

**Solution:** Add responsive CSS:
```css
@media (max-width: 768px) {
    .e-querybuilder {
        padding: 10px;
    }
    
    .e-rule {
        display: grid;
        grid-template-columns: 1fr;
    }
}
```

## Configuration Issues

### Column Type not matching operators

**Symptoms:** Operators don't appear or are incorrect

**Solution:** Ensure Column Type matches field data type:
```razor
<!-- ✓ Correct -->
<QueryBuilderColumn Field="Salary" Label="Salary" Type="ColumnType.Number"></QueryBuilderColumn>

<!-- ✗ Wrong - should be Number -->
<QueryBuilderColumn Field="Salary" Label="Salary" Type="ColumnType.String"></QueryBuilderColumn>
```

### Date formats not working

**Solution:** Specify date format explicitly:
```razor
<QueryBuilderColumn 
    Field="OrderDate" 
    Label="Order Date" 
    Type="ColumnType.Date"
    Format="MM/dd/yyyy">
</QueryBuilderColumn>
```

### Boolean columns showing text instead of checkbox

**Solution:** Use correct column type:
```razor
<!-- ✓ Correct -->
<QueryBuilderColumn Field="IsActive" Label="Active" Type="ColumnType.Boolean"></QueryBuilderColumn>

<!-- ✗ Wrong -->
<QueryBuilderColumn Field="IsActive" Label="Active" Type="ColumnType.String"></QueryBuilderColumn>
```

## Validation Issues

### Validation rules not enforced

**Solution:** Implement validation in event handler:
```razor
@code {
    private void OnValueChanging(ChangeEventArgs args) {
        if (args.Field == "Salary" && args.Value is int salary) {
            if (salary < 0 || salary > 1000000) {
                args.Cancel = true;
                Console.WriteLine("Invalid salary range");
            }
        }
    }
}
```

## Export/Import Issues

### Query export returns null

**Symptoms:** GetRules() returns null or empty

**Solution:**
```razor
@code {
    private async Task ExportSafely() {
        var rules = QueryBuilder.GetRules();
        
        if (rules == null) {
            Console.WriteLine("No rules to export");
            return;
        }
        
        if (rules.Rules == null || rules.Rules.Count == 0) {
            Console.WriteLine("Rules collection is empty");
            return;
        }
        
        var json = JsonConvert.SerializeObject(rules);
        Console.WriteLine(json);
    }
}
```

### Import creates invalid rules

**Solution:** Validate before import:
```razor
@code {
    private bool ValidateImportedRules(RuleModel rules) {
        if (rules?.Rules == null || rules.Rules.Count == 0) {
            Console.WriteLine("Invalid rules structure");
            return false;
        }
        
        foreach (var rule in rules.Rules) {
            if (string.IsNullOrEmpty(rule.Field) || string.IsNullOrEmpty(rule.Operator)) {
                Console.WriteLine("Rule missing required fields");
                return false;
            }
        }
        
        return true;
    }
}
```

## Common Workarounds

### Refresh Query Builder after data change
```razor
@code {
    private SfQueryBuilder<Order> QueryBuilder;
    
    private async Task RefreshQueryBuilder() {
        // Force refresh
        await QueryBuilder.Refresh();
    }
}
```

### Clear all rules
```razor
@code {
    private async Task ClearAllRules() {
        var rules = QueryBuilder.GetRules();
        
        foreach (var rule in rules.Rules.ToList()) {
            if (rule.Rules == null) {  // It's a rule, not a group
                await QueryBuilder.DeleteRule(rule.Id);
            }
        }
    }
}
```

### Log component state for debugging
```razor
@code {
    private async Task LogComponentState() {
        var rules = QueryBuilder.GetRules();
        Console.WriteLine("=== Query Builder State ===");
        Console.WriteLine($"Rules: {rules.Rules?.Count ?? 0}");
        Console.WriteLine($"Condition: {rules.Condition}");
        Console.WriteLine(JsonConvert.SerializeObject(rules, Formatting.Indented));
    }
}
```

## Getting Help

1. **Check browser console** (F12) for JavaScript errors
2. **Review documentation** in [getting-started.md](getting-started.md)
3. **Enable debug logging** in your application
4. **Create minimal reproduction** with simplified code
5. **Contact Syncfusion support** with error details and code sample

## Next Steps

- Review [advanced-features.md](advanced-features.md) for optimization
- Check [events-and-callbacks.md](events-and-callbacks.md) for event handling
- See [customization-and-styling.md](customization-and-styling.md) for styling help
