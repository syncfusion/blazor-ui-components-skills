# Advanced Features

This guide covers import/export, state persistence, drag-drop, and other advanced Query Builder capabilities.

## Table of Contents

- [Import/Export Queries](#importexport-queries)
- [State Persistence](#state-persistence)
- [Drag & Drop](#drag--drop)
- [Lock & Clone Rules](#lock--clone-rules)
- [Read-Only Mode](#read-only-mode)
- [Sort & Filter Columns](#sort--filter-columns)

## Import/Export Queries

### Export to JSON

Export the current query structure as JSON:

```razor
<SfButton OnClick="ExportQueryAsJson">Export Query</SfButton>

<SfQueryBuilder TValue="Order" @ref="QueryBuilder">
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Status" Label="Status" Type="ColumnType.String"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private SfQueryBuilder<Order> QueryBuilder;
    
    private void ExportQueryAsJson() {
        var rules = QueryBuilder.GetRules();
        var json = JsonConvert.SerializeObject(rules, Formatting.Indented);
        
        Console.WriteLine("Exported Query:");
        Console.WriteLine(json);
        
        // Example output:
        // {
        //   "Condition": "and",
        //   "Rules": [
        //     {
        //       "Field": "OrderID",
        //       "Operator": "greater",
        //       "Value": 1000
        //     }
        //   ]
        // }
    }
}
```

### Import from JSON

Import a previously saved query:

```razor
<SfButton OnClick="ImportQueryFromJson">Load Saved Query</SfButton>

<SfQueryBuilder TValue="Order" @ref="QueryBuilder">
    <QueryBuilderRule Rules="@ImportedRules"></QueryBuilderRule>
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private SfQueryBuilder<Order> QueryBuilder;
    private List<RuleModel> ImportedRules;
    
    private void ImportQueryFromJson() {
        var json = @"{
            ""Condition"": ""and"",
            ""Rules"": [
                {
                    ""Field"": ""OrderID"",
                    ""Type"": ""Number"",
                    ""Operator"": ""greater"",
                    ""Value"": 1000
                }
            ]
        }";
        
        var rules = JsonConvert.DeserializeObject<RuleModel>(json);
        ImportedRules = rules.Rules ?? new List<RuleModel>();
        StateHasChanged();
    }
}
```

### Export/Import via URL

```razor
@code {
    private void ExportToUrl() {
        var rules = QueryBuilder.GetRules();
        var queryString = BuildQueryString(rules);
        var url = $"https://yoursite.com/search?{queryString}";
        
        Console.WriteLine($"Shareable URL: {url}");
    }
    
    private string BuildQueryString(RuleModel rules) {
        var sb = new StringBuilder();
        sb.Append("condition=" + rules.Condition);
        
        foreach (var rule in rules.Rules) {
            sb.Append($"&field={rule.Field}&operator={rule.Operator}&value={rule.Value}");
        }
        
        return sb.ToString();
    }
    
    private void LoadFromUrl(string queryString) {
        var parameters = System.Web.HttpUtility.ParseQueryString(queryString);
        
        var rules = new List<RuleModel>();
        for (int i = 0; i < parameters.Count; i += 3) {
            rules.Add(new RuleModel {
                Field = parameters[$"field{i/3}"],
                Operator = parameters[$"operator{i/3}"],
                Value = parameters[$"value{i/3}"]
            });
        }
        
        ImportedRules = rules;
    }
}
```

## State Persistence

### Save to LocalStorage

```razor
@using System.Text.Json

<SfButton OnClick="SaveQueryToStorage">Save Query</SfButton>
<SfButton OnClick="LoadQueryFromStorage">Load Query</SfButton>

<SfQueryBuilder TValue="Order" @ref="QueryBuilder">
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private SfQueryBuilder<Order> QueryBuilder;
    @inject IJSRuntime JS;
    
    private async Task SaveQueryToStorage() {
        var rules = QueryBuilder.GetRules();
        var json = JsonConvert.SerializeObject(rules);
        
        await JS.InvokeVoidAsync("localStorage.setItem", "savedQuery", json);
        Console.WriteLine("Query saved to localStorage");
    }
    
    private async Task LoadQueryFromStorage() {
        var json = await JS.InvokeAsync<string>("localStorage.getItem", "savedQuery");
        
        if (!string.IsNullOrEmpty(json)) {
            var rules = JsonConvert.DeserializeObject<RuleModel>(json);
            // Apply loaded rules to Query Builder
            Console.WriteLine("Query loaded from localStorage");
        }
    }
}
```

### Save to Database

```razor
@code {
    private async Task SaveQueryToDatabase(string userId, string queryName) {
        var rules = QueryBuilder.GetRules();
        var json = JsonConvert.SerializeObject(rules);
        
        var queryRecord = new SavedQuery {
            UserId = userId,
            QueryName = queryName,
            QueryDefinition = json,
            CreatedDate = DateTime.UtcNow
        };
        
        await _repository.SaveQueryAsync(queryRecord);
        Console.WriteLine($"Query '{queryName}' saved to database");
    }
    
    private async Task LoadQueryFromDatabase(int savedQueryId) {
        var queryRecord = await _repository.GetQueryAsync(savedQueryId);
        
        if (queryRecord != null) {
            var rules = JsonConvert.DeserializeObject<RuleModel>(queryRecord.QueryDefinition);
            // Apply to Query Builder
        }
    }
    
    public class SavedQuery {
        public int Id { get; set; }
        public string UserId { get; set; }
        public string QueryName { get; set; }
        public string QueryDefinition { get; set; }
        public DateTime CreatedDate { get; set; }
    }
}
```

## Drag & Drop

### Enable Drag-Drop Interface

```razor
<SfQueryBuilder TValue="Order" AllowDragAndDrop="true">
    <QueryBuilderShowButtons RuleDelete="true" GroupDelete="true"></QueryBuilderShowButtons>
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Status" Label="Status" Type="ColumnType.String"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>
```

**Benefits:**
- Reorder rules and groups interactively
- Improve query building UX
- Visual feedback during drag operations

### Programmatic Reordering

```razor
@code {
    private async Task ReorderRules() {
        // Get current rules
        var rules = QueryBuilder.GetRules();
        
        // Swap first two rules
        if (rules.Rules.Count >= 2) {
            var temp = rules.Rules[0];
            rules.Rules[0] = rules.Rules[1];
            rules.Rules[1] = temp;
        }
    }
}
```

## Lock & Clone Rules

### Clone Groups and Rules

The Query Builder supports cloning both individual rules and entire groups. Cloning creates an exact duplicate of the selected rule or group adjacent to the original, making it quick to replicate complex query structures.

**Enable Clone Buttons in UI:**

```razor
@using Syncfusion.Blazor.QueryBuilder
@using Syncfusion.Blazor.Buttons

<SfQueryBuilder TValue="EmployeeDetails" @ref="QueryBuilder">
    <QueryBuilderShowButtons RuleDelete="true" GroupDelete="true" CloneGroup="true" CloneRule="true"></QueryBuilderShowButtons>
    <QueryBuilderRule Condition="or" Rules="@Rules"></QueryBuilderRule>
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="EmployeeID" Label="Employee ID" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="FirstName" Label="First Name" Type="ColumnType.String"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Country" Label="Country" Type="ColumnType.String"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    SfQueryBuilder<EmployeeDetails> QueryBuilder;
    
    List<RuleModel> Rules = new List<RuleModel>()
    {
        new RuleModel { Field="Country", Label="Country", Type="String", Operator="equal", Value = "England" },
        new RuleModel { Field="EmployeeID", Label="EmployeeID", Type="Number", Operator="notequal", Value = 1001 }
    };
    
    public class EmployeeDetails
    {
        public int EmployeeID { get; set; }
        public string FirstName { get; set; }
        public string Country { get; set; }
    }
}
```

**Programmatically Clone Group:**

```razor
<SfButton OnClick="CloneGroup">Clone Group</SfButton>

@code {
    private void CloneGroup()
    {
        // CloneGroup(groupId, insertIndex)
        // First parameter: existing group's ID
        // Second parameter: insert index within its parent
        QueryBuilder.CloneGroup("group1", 2);
        Console.WriteLine("Group cloned successfully");
    }
}
```

**Programmatically Clone Rule:**

```razor
<SfButton OnClick="CloneRule">Clone Rule</SfButton>

@code {
    private void CloneRule()
    {
        // CloneRule(ruleId, insertIndex)
        // First parameter: existing rule's ID
        // Second parameter: insert index within its parent group
        QueryBuilder.CloneRule("group0_rule0", 1);
        Console.WriteLine("Rule cloned successfully");
    }
}
```

> **Note:** Ensure that the IDs passed to `CloneGroup` and `CloneRule` refer to existing items in the current model. The cloning buttons can be shown or hidden via `QueryBuilderShowButtons`.

### Lock Groups and Rules

The Query Builder supports locking individual rules or entire groups. When a rule is locked, its field, operator, and value editors are disabled and cannot be changed. Locking a group disables all editors and actions within that group. Locked items still participate in query evaluation; only editing is restricted.

**Enable Lock Buttons in UI:**

```razor
@using Syncfusion.Blazor.QueryBuilder
@using Syncfusion.Blazor.Buttons

<SfQueryBuilder TValue="EmployeeDetails" @ref="QueryBuilder">
    <QueryBuilderShowButtons RuleDelete="true" GroupDelete="true" LockGroup="true" LockRule="true"></QueryBuilderShowButtons>
    <QueryBuilderRule Condition="or" Rules="@Rules"></QueryBuilderRule>
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="EmployeeID" Label="Employee ID" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="FirstName" Label="First Name" Type="ColumnType.String"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Country" Label="Country" Type="ColumnType.String"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    SfQueryBuilder<EmployeeDetails> QueryBuilder;
    
    List<RuleModel> Rules = new List<RuleModel>()
    {
        new RuleModel { Field="Country", Label="Country", Type="String", Operator="equal", Value = "England" },
        new RuleModel { Field="EmployeeID", Label="EmployeeID", Type="Number", Operator="notequal", Value = 1001 }
    };
    
    public class EmployeeDetails
    {
        public int EmployeeID { get; set; }
        public string FirstName { get; set; }
        public string Country { get; set; }
    }
}
```

**Programmatically Lock Group:**

```razor
<SfButton OnClick="LockGroup">Lock Group</SfButton>

@code {
    private void LockGroup()
    {
        // LockGroup(groupId, isLocked)
        // First parameter: group's ID
        // Second parameter: true to lock, false to unlock
        QueryBuilder.LockGroup("group0", true);
        Console.WriteLine("Group locked successfully");
    }
}
```

**Programmatically Lock Rule:**

```razor
<SfButton OnClick="LockRule">Lock Rule</SfButton>

@code {
    private void LockRule()
    {
        // LockRule(ruleId, isLocked)
        // First parameter: rule's ID
        // Second parameter: true to lock, false to unlock
        QueryBuilder.LockRule("group0_rule0", true);
        Console.WriteLine("Rule locked successfully");
    }
}
```

**Complete Example with Lock and Clone:**

```razor
@page "/lock-clone-demo"
@using Syncfusion.Blazor.QueryBuilder
@using Syncfusion.Blazor.Buttons

<SfQueryBuilder TValue="EmployeeDetails" @ref="QueryBuilder">
    <QueryBuilderShowButtons 
        RuleDelete="true" 
        GroupDelete="true" 
        GroupInsert="true"
        CloneGroup="true" 
        CloneRule="true"
        LockGroup="true" 
        LockRule="true">
    </QueryBuilderShowButtons>
    <QueryBuilderRule Condition="or" Rules="@Rules"></QueryBuilderRule>
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="EmployeeID" Label="Employee ID" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="FirstName" Label="First Name" Type="ColumnType.String"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Country" Label="Country" Type="ColumnType.String"></QueryBuilderColumn>
        <QueryBuilderColumn Field="HireDate" Label="Hire Date" Type="ColumnType.Date" Format="MM/dd/yyyy"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

<div class="button-group">
    <SfButton OnClick="CloneGroup" IsPrimary="true">Clone Group</SfButton>
    <SfButton OnClick="CloneRule" IsPrimary="true">Clone Rule</SfButton>
    <SfButton OnClick="LockGroup" IsPrimary="true">Lock Group</SfButton>
    <SfButton OnClick="LockRule" IsPrimary="true">Lock Rule</SfButton>
    <SfButton OnClick="UnlockGroup" IsPrimary="true">Unlock Group</SfButton>
    <SfButton OnClick="UnlockRule" IsPrimary="true">Unlock Rule</SfButton>
</div>

@code {
    SfQueryBuilder<EmployeeDetails> QueryBuilder;
    
    List<RuleModel> Rules = new List<RuleModel>()
    {
        new RuleModel { Field="Country", Label="Country", Type="String", Operator="equal", Value = "England" },
        new RuleModel { Field="EmployeeID", Label="EmployeeID", Type="Number", Operator="notequal", Value = 1001 },
        new RuleModel { Condition = "or", Rules = new List<RuleModel>()
        {
            new RuleModel { Field="FirstName", Label="FirstName", Type="String", Operator="startswith", Value = "John" }
        }}
    };

    private void CloneGroup()
    {
        QueryBuilder.CloneGroup("group1", 2);
    }

    private void CloneRule()
    {
        QueryBuilder.CloneRule("group0_rule0", 1);
    }
    
    private void LockGroup()
    {
        QueryBuilder.LockGroup("group0", true);
    }
    
    private void LockRule()
    {
        QueryBuilder.LockRule("group0_rule0", true);
    }
    
    private void UnlockGroup()
    {
        QueryBuilder.LockGroup("group0", false);
    }
    
    private void UnlockRule()
    {
        QueryBuilder.LockRule("group0_rule0", false);
    }

    public class EmployeeDetails
    {
        public int EmployeeID { get; set; }
        public string FirstName { get; set; }
        public string Country { get; set; }
        public DateTime HireDate { get; set; }
    }
}
```

**Key Features:**
- **Clone Buttons**: Show/hide via `QueryBuilderShowButtons` properties `CloneGroup` and `CloneRule`
- **Lock Buttons**: Show/hide via `QueryBuilderShowButtons` properties `LockGroup` and `LockRule`
- **Methods Available**:
  - `CloneGroup(groupId, insertIndex)` - Clone a group at specified position
  - `CloneRule(ruleId, insertIndex)` - Clone a rule at specified position
  - `LockGroup(groupId, isLocked)` - Lock/unlock a group
  - `LockRule(ruleId, isLocked)` - Lock/unlock a rule

## Read-Only Mode

### Disable Editing

```razor
<SfQueryBuilder TValue="Order" Readonly="true">
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>
```

**Use cases:**
- Display pre-defined queries
- Allow viewing without modification
- Preview mode

### Restrict Specific Operations

```razor
@code {
    private bool CanDeleteRules = false;
    private bool CanAddGroups = true;
    private bool CanModifyValues = true;
    
    private async Task HandleRuleChange(RuleChangeEventArgs args) {
        if (args.Action == "remove" && !CanDeleteRules) {
            Console.WriteLine("Rule deletion not allowed");
            args.Cancel = true;
        }
    }
}
```

## Sort & Filter Columns

### Sort Columns in Dropdown

```razor
<SfQueryBuilder TValue="Order">
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number" SortOrder="1"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Status" Label="Status" Type="ColumnType.String" SortOrder="2"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Amount" Label="Amount" Type="ColumnType.Number" SortOrder="3"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>
```

### Filter Available Columns

```razor
<SfQueryBuilder TValue="Order">
    <QueryBuilderColumns>
        <!-- Show only these columns -->
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number" Visible="true"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Status" Label="Status" Type="ColumnType.String" Visible="true"></QueryBuilderColumn>
        
        <!-- Hide from UI -->
        <QueryBuilderColumn Field="InternalNotes" Label="Internal" Type="ColumnType.String" Visible="false"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>
```

### Dynamic Column Visibility

```razor
<SfButton OnClick="ToggleColumnVisibility">Toggle Columns</SfButton>

@code {
    private bool ShowInternalColumns = false;
    
    private void ToggleColumnVisibility() {
        ShowInternalColumns = !ShowInternalColumns;
        StateHasChanged();
    }
}
```

## Next Steps

- [Localization](localization-and-accessibility.md) - Multi-language and RTL support
- [Troubleshooting](troubleshooting.md) - Performance and advanced issues
- [Customization](customization-and-styling.md) - Styling options
