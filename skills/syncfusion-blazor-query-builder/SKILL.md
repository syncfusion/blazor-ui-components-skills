---
name: syncfusion-blazor-query-builder
description: Implement Syncfusion Blazor Query Builder component for building dynamic, customizable query interfaces with complex filtering logic. Use this when creating advanced search interfaces, implementing business rule engines, or building data filtering workflows with nested condition groups and AND/OR logic. Supports rule management, drag-drop UI, state persistence, and extensive customization options.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Form Components"
---

# Implementing Syncfusion Blazor Query Builder

The Syncfusion Blazor Query Builder component enables you to build dynamic, customizable query interfaces for filtering data with complex conditions, nested groups, and rule-based logic. It's ideal for advanced search UIs, business rule engines, and data filtering workflows.

## Component Overview

The Query Builder organizes filtering logic into:
- **Rules:** Individual conditions (field = value)
- **Groups:** Collections of rules combined with AND/OR logic
- **Operators:** Comparison types (equal, contains, between, etc.)
- **Columns:** Data source fields that can be filtered

Key capabilities include programmatic rule/group management, event handling, drag-drop UI, state persistence, and extensive customization options.

## Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation & setup for Blazor WebAssembly and Blazor Server
- NuGet package configuration
- Initial project setup
- Running your first Query Builder

### Basic Setup & Configuration
📄 **Read:** [references/basic-setup.md](references/basic-setup.md)
- Creating the Query Builder component
- Defining columns and data types
- Configuring button visibility
- Setting default rules and initial state

### Filtering, Rules & Groups (Core Feature)
📄 **Read:** [references/filtering-and-rules.md](references/filtering-and-rules.md)
- Adding and deleting rules programmatically
- Creating and managing groups
- Operators and conditions
- Complex nested group patterns
- User interaction and validation

### Data Binding & Filtered Results
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Binding data sources to Query Builder
- Extracting and working with filtered records
- Dynamic data loading patterns
- Applying queries to datasets

### Events & Callbacks (Core Feature)
📄 **Read:** [references/events-and-callbacks.md](references/events-and-callbacks.md)
- Component lifecycle events (Created, Destroyed)
- Rule change tracking (RuleChanged, Changed)
- Pre-change validation (OnValueChange)
- Event-driven query updates

### Customization & Styling
📄 **Read:** [references/customization-and-styling.md](references/customization-and-styling.md)
- CSS customization and theming
- Template support for custom UI
- Custom operator definitions
- Visual styling of groups and rules

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Import/export queries (JSON, URL parameters)
- State persistence and local storage
- Drag-drop rule and group management
- Lock/clone group rules
- Read-only modes
- Query sorting and filtering

### Localization & Accessibility
📄 **Read:** [references/localization-and-accessibility.md](references/localization-and-accessibility.md)
- Multi-language support (localization)
- Right-to-Left (RTL) support
- Accessibility best practices
- Keyboard navigation
- Screen reader support

### Troubleshooting
📄 **Read:** [references/troubleshooting.md](references/troubleshooting.md)
- Common configuration issues
- Event handling problems
- Data binding troubleshooting
- Performance optimization
- Testing strategies

## Quick Start

Here's a minimal example to get started:

```razor
@using Syncfusion.Blazor.QueryBuilder

<SfQueryBuilder TValue="Order" DataSource="@Orders">
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="CustomerName" Label="Customer" Type="ColumnType.String"></QueryBuilderColumn>
        <QueryBuilderColumn Field="OrderDate" Label="Date" Type="ColumnType.Date"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private List<Order> Orders { get; set; } = new()
    {
        new() { OrderID = 1, CustomerName = "Acme Corp", OrderDate = new(2024, 1, 15) },
        new() { OrderID = 2, CustomerName = "Tech Ltd", OrderDate = new(2024, 2, 20) }
    };

    public class Order {
        public int OrderID { get; set; }
        public string CustomerName { get; set; }
        public DateTime OrderDate { get; set; }
    }
}
```

## Common Patterns

### Add a Rule Programmatically
```razor
<SfButton OnClick="AddFilterRule">Add Rule</SfButton>

@code {
    private SfQueryBuilder<Order> QueryBuilder;
    
    private async Task AddFilterRule() {
        var rule = new RuleModel {
            Label = "Order ID",
            Field = "OrderID",
            Type = "Number",
            Operator = "equal",
            Value = 1000
        };
        await QueryBuilder.AddRule(rule, "group0");
    }
}
```

### Capture Query Changes
```razor
<SfQueryBuilder TValue="Order" DataSource="@Orders">
    <QueryBuilderEvents TValue="Order" RuleChanged="OnRuleChanged"></QueryBuilderEvents>
</SfQueryBuilder>

@code {
    private void OnRuleChanged(RuleChangeEventArgs args) {
        Console.WriteLine($"Query changed: {args.RuleID}");
        // Apply new filter or update results
    }
}
```

### Export Query to JSON
```razor
<SfButton OnClick="ExportQuery">Export</SfButton>

@code {
    private SfQueryBuilder<Order> QueryBuilder;
    
    private async Task ExportQuery() {
        var queryJson = QueryBuilder.GetRules();
        // Save or transmit queryJson
    }
}
```

### Clone a Rule or Group
```razor
<SfQueryBuilder TValue="Order" @ref="QueryBuilder">
    <QueryBuilderShowButtons CloneGroup="true" CloneRule="true"></QueryBuilderShowButtons>
</SfQueryBuilder>

<SfButton OnClick="CloneRule">Clone Rule</SfButton>
<SfButton OnClick="CloneGroup">Clone Group</SfButton>

@code {
    private SfQueryBuilder<Order> QueryBuilder;
    
    private void CloneRule() {
        // Clone the first rule at position 1
        QueryBuilder.CloneRule("group0_rule0", 1);
    }
    
    private void CloneGroup() {
        // Clone group1 at position 2
        QueryBuilder.CloneGroup("group1", 2);
    }
}
```

### Lock a Rule or Group
```razor
<SfQueryBuilder TValue="Order" @ref="QueryBuilder">
    <QueryBuilderShowButtons LockGroup="true" LockRule="true"></QueryBuilderShowButtons>
</SfQueryBuilder>

<SfButton OnClick="LockRule">Lock Rule</SfButton>
<SfButton OnClick="UnlockRule">Unlock Rule</SfButton>

@code {
    private SfQueryBuilder<Order> QueryBuilder;
    
    private void LockRule() {
        // Lock the rule to prevent editing
        QueryBuilder.LockRule("group0_rule0", true);
    }
    
    private void UnlockRule() {
        // Unlock the rule to allow editing
        QueryBuilder.LockRule("group0_rule0", false);
    }
}
```

## Query Export Methods

The Query Builder provides built-in methods to export queries in different formats:

### 1. GetSqlFromRules - Export as Inline SQL
Convert query rules to standard SQL WHERE clause format:

```razor
<SfQueryBuilder TValue="Order" @ref="QueryBuilder">
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="CustomerName" Label="Customer" Type="ColumnType.String"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

<SfButton OnClick="ExportInlineSQL">Get SQL Query</SfButton>
<p>@SqlQuery</p>

@code {
    private SfQueryBuilder<Order> QueryBuilder;
    private string SqlQuery = "";
    
    private void ExportInlineSQL() {
        RuleModel rules = QueryBuilder.GetValidRules();
        SqlQuery = QueryBuilder.GetSqlFromRules(rules);
        // Output: "OrderID = 1000 AND CustomerName LIKE '%Acme%'"
    }
    
    public class Order {
        public int OrderID { get; set; }
        public string CustomerName { get; set; }
    }
}
```

### 2. GetParameterSql - Export as Parameterized SQL
Export SQL with indexed parameters to prevent SQL injection:

```razor
@using System.Text.Json

<SfButton OnClick="ExportParameterSQL">Get Parameterized SQL</SfButton>
<pre>@ParameterSqlJson</pre>

@code {
    private SfQueryBuilder<Order> QueryBuilder;
    private string ParameterSqlJson = "";
    
    private void ExportParameterSQL() {
        RuleModel rules = QueryBuilder.GetValidRules();
        ParameterSql paramSql = QueryBuilder.GetParameterSql(rules);
        
        ParameterSqlJson = JsonSerializer.Serialize(paramSql, new JsonSerializerOptions { 
            WriteIndented = true,
            Encoder = System.Text.Encodings.Web.JavaScriptEncoder.UnsafeRelaxedJsonEscaping 
        });
        
        // Access individual properties:
        // paramSql.Sql - SQL string with ? placeholders
        // paramSql.Parameters - List of parameter values
    }
}
```

**Output Example:**
```json
{
  "Sql": "OrderID = ? AND CustomerName LIKE ?",
  "Parameters": [1000, "%Acme%"]
}
```

### 3. GetNamedParameterSql - Export as Named Parameter SQL
Export SQL with named parameters (@param0, @param1, etc.):

```razor
@using System.Text.Json

<SfButton OnClick="ExportNamedParameterSQL">Get Named Parameter SQL</SfButton>
<pre>@NamedParameterSqlJson</pre>

@code {
    private SfQueryBuilder<Order> QueryBuilder;
    private string NamedParameterSqlJson = "";
    
    private void ExportNamedParameterSQL() {
        RuleModel rules = QueryBuilder.GetValidRules();
        NamedParameterSql namedParamSql = QueryBuilder.GetNamedParameterSql(rules);
        
        NamedParameterSqlJson = JsonSerializer.Serialize(namedParamSql, new JsonSerializerOptions { 
            WriteIndented = true,
            Encoder = System.Text.Encodings.Web.JavaScriptEncoder.UnsafeRelaxedJsonEscaping 
        });
        
        // Access individual properties:
        // namedParamSql.Sql - SQL string with @param placeholders
        // namedParamSql.Parameters - Dictionary of parameter names/values
    }
}
```

**Output Example:**
```json
{
  "Sql": "OrderID = @param0 AND CustomerName LIKE @param1",
  "Parameters": {
    "@param0": 1000,
    "@param1": "%Acme%"
  }
}
```

### 4. GetMongoQuery - Export as MongoDB Query
Convert query rules to MongoDB query format:

```razor
@using System.Text.Json

<SfButton OnClick="ExportMongoQuery">Get MongoDB Query</SfButton>
<pre>@MongoQueryJson</pre>

@code {
    private SfQueryBuilder<Order> QueryBuilder;
    private string MongoQueryJson = "";
    
    private void ExportMongoQuery() {
        RuleModel rules = QueryBuilder.GetValidRules();
        string mongoQuery = QueryBuilder.GetMongoQuery(rules);
        
        // Pretty print the JSON
        JsonDocument jsonDoc = JsonDocument.Parse(mongoQuery);
        MongoQueryJson = JsonSerializer.Serialize(jsonDoc.RootElement, new JsonSerializerOptions {
            WriteIndented = true
        });
    }
}
```

**Output Example:**
```json
{
  "$and": [
    { "OrderID": { "$eq": 1000 } },
    { "CustomerName": { "$regex": "Acme", "$options": "i" } }
  ]
}
```

### Import Query Methods

You can also import queries back into the Query Builder:

```razor
@code {
    // Import from SQL
    private void ImportSQL(string sqlQuery) {
        QueryBuilder.SetRulesFromSql(sqlQuery);
    }
    
    // Import from Parameterized SQL
    private void ImportParameterSQL(ParameterSql paramSql) {
        QueryBuilder.SetParameterSql(paramSql);
    }
    
    // Import from Named Parameter SQL
    private void ImportNamedParameterSQL(NamedParameterSql namedParamSql) {
        QueryBuilder.SetNamedParameterSql(namedParamSql);
    }
    
    // Import from MongoDB Query
    private void ImportMongoQuery(string mongoQuery) {
        QueryBuilder.SetMongoQuery(mongoQuery);
    }
}
```

### Complete Query Export/Import Example

```razor
@page "/query-export-demo"
@using Syncfusion.Blazor.QueryBuilder
@using Syncfusion.Blazor.Buttons
@using System.Text.Json

<SfQueryBuilder TValue="Order" @ref="QueryBuilder">
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="CustomerName" Label="Customer" Type="ColumnType.String"></QueryBuilderColumn>
        <QueryBuilderColumn Field="OrderDate" Label="Order Date" Type="ColumnType.Date"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Amount" Label="Amount" Type="ColumnType.Number"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

<div class="button-group">
    <SfButton OnClick="@(() => ExportQuery("SQL"))">Export SQL</SfButton>
    <SfButton OnClick="@(() => ExportQuery("ParameterSQL"))">Export Parameter SQL</SfButton>
    <SfButton OnClick="@(() => ExportQuery("NamedSQL"))">Export Named SQL</SfButton>
    <SfButton OnClick="@(() => ExportQuery("MongoDB"))">Export MongoDB</SfButton>
</div>

<div class="output">
    <h3>@OutputTitle</h3>
    <pre>@OutputQuery</pre>
</div>

@code {
    private SfQueryBuilder<Order> QueryBuilder;
    private string OutputQuery = "";
    private string OutputTitle = "";
    
    private void ExportQuery(string format) {
        RuleModel rules = QueryBuilder.GetValidRules();
        
        switch(format) {
            case "SQL":
                OutputTitle = "Inline SQL Query";
                OutputQuery = QueryBuilder.GetSqlFromRules(rules);
                break;
                
            case "ParameterSQL":
                OutputTitle = "Parameterized SQL Query";
                ParameterSql paramSql = QueryBuilder.GetParameterSql(rules);
                OutputQuery = JsonSerializer.Serialize(paramSql, new JsonSerializerOptions { 
                    WriteIndented = true,
                    Encoder = System.Text.Encodings.Web.JavaScriptEncoder.UnsafeRelaxedJsonEscaping 
                });
                break;
                
            case "NamedSQL":
                OutputTitle = "Named Parameter SQL Query";
                NamedParameterSql namedSql = QueryBuilder.GetNamedParameterSql(rules);
                OutputQuery = JsonSerializer.Serialize(namedSql, new JsonSerializerOptions { 
                    WriteIndented = true,
                    Encoder = System.Text.Encodings.Web.JavaScriptEncoder.UnsafeRelaxedJsonEscaping 
                });
                break;
                
            case "MongoDB":
                OutputTitle = "MongoDB Query";
                string mongoQuery = QueryBuilder.GetMongoQuery(rules);
                JsonDocument jsonDoc = JsonDocument.Parse(mongoQuery);
                OutputQuery = JsonSerializer.Serialize(jsonDoc.RootElement, new JsonSerializerOptions {
                    WriteIndented = true
                });
                break;
        }
    }
    
    public class Order {
        public int OrderID { get; set; }
        public string CustomerName { get; set; }
        public DateTime OrderDate { get; set; }
        public decimal Amount { get; set; }
    }
}
```

## Key Features at a Glance

| Feature | Use Case | Reference |
|---------|----------|-----------|
| **Drag-Drop Rules** | Reorder conditions interactively | [advanced-features.md](references/advanced-features.md) |
| **Clone Groups/Rules** | Duplicate complex query structures | [advanced-features.md](references/advanced-features.md) |
| **Lock Groups/Rules** | Prevent editing of specific conditions | [advanced-features.md](references/advanced-features.md) |
| **Nested Groups** | Complex multi-condition logic | [filtering-and-rules.md](references/filtering-and-rules.md) |
| **Event Tracking** | React to query changes | [events-and-callbacks.md](references/events-and-callbacks.md) |
| **State Persistence** | Save/restore queries | [advanced-features.md](references/advanced-features.md) |
| **Localization** | Multi-language support | [localization-and-accessibility.md](references/localization-and-accessibility.md) |
| **Custom Templates** | Branded UI | [customization-and-styling.md](references/customization-and-styling.md) |

## Next Steps

1. Start with [references/getting-started.md](references/getting-started.md) to set up your environment
2. Follow [references/basic-setup.md](references/basic-setup.md) for component configuration
3. Explore [references/filtering-and-rules.md](references/filtering-and-rules.md) for core filtering logic
4. Handle queries with [references/events-and-callbacks.md](references/events-and-callbacks.md)
5. Refer to other guides as needed for advanced scenarios
