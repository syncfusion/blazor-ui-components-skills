# Basic Setup & Configuration

This guide covers creating and configuring the Query Builder component with columns, operators, and initial rules.

## Creating the Component

The simplest Query Builder has a data source and columns:

```razor
@using Syncfusion.Blazor.QueryBuilder

<SfQueryBuilder TValue="Order">
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="CustomerName" Label="Customer" Type="ColumnType.String"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>
```

**Parameters:**
- `TValue`: Type of data model (e.g., `Order`, `Employee`)
- `Field`: Property name in the data model
- `Label`: Display name in the UI
- `Type`: Data type (String, Number, Date, Boolean)

## Column Configuration

### Column Types

```razor
<QueryBuilderColumns>
    <!-- String columns -->
    <QueryBuilderColumn Field="Name" Label="Name" Type="ColumnType.String"></QueryBuilderColumn>
    
    <!-- Number columns -->
    <QueryBuilderColumn Field="Age" Label="Age" Type="ColumnType.Number"></QueryBuilderColumn>
    
    <!-- Date columns -->
    <QueryBuilderColumn Field="BirthDate" Label="Birth Date" Type="ColumnType.Date" Format="MM/dd/yyyy"></QueryBuilderColumn>
    
    <!-- Boolean columns (checkbox) -->
    <QueryBuilderColumn Field="IsActive" Label="Active" Type="ColumnType.Boolean"></QueryBuilderColumn>
</QueryBuilderColumns>
```

### With Data Source

```razor
<SfQueryBuilder TValue="Employee" DataSource="@Employees">
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="EmployeeID" Label="ID" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="FirstName" Label="First Name" Type="ColumnType.String"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Department" Label="Department" Type="ColumnType.String"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Salary" Label="Salary" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="JoinDate" Label="Join Date" Type="ColumnType.Date"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private List<Employee> Employees = new() {
        new() { EmployeeID = 1, FirstName = "John", Department = "IT", Salary = 50000, JoinDate = new(2020, 1, 15) },
        new() { EmployeeID = 2, FirstName = "Jane", Department = "HR", Salary = 45000, JoinDate = new(2019, 6, 20) }
    };

    public class Employee {
        public int EmployeeID { get; set; }
        public string FirstName { get; set; }
        public string Department { get; set; }
        public double Salary { get; set; }
        public DateTime JoinDate { get; set; }
    }
}
```

## Configuring Button Visibility

Control which action buttons appear in the UI:

```razor
<SfQueryBuilder TValue="Order">
    <QueryBuilderShowButtons 
        RuleDelete="true" 
        GroupInsert="true" 
        GroupDelete="true">
    </QueryBuilderShowButtons>
    <QueryBuilderColumns>
        <!-- columns -->
    </QueryBuilderColumns>
</SfQueryBuilder>
```

**Available buttons:**
- `RuleDelete` - Delete individual rules
- `GroupDelete` - Delete groups
- `GroupInsert` - Add new groups

## Setting Default Rules

Initialize the Query Builder with predefined rules:

```razor
<SfQueryBuilder TValue="Order" @ref="QueryBuilder">
    <QueryBuilderRule Condition="and" Rules="@DefaultRules"></QueryBuilderRule>
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Status" Label="Status" Type="ColumnType.String"></QueryBuilderColumn>
        <QueryBuilderColumn Field="OrderDate" Label="Order Date" Type="ColumnType.Date"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private SfQueryBuilder<Order> QueryBuilder;
    
    private List<RuleModel> DefaultRules = new() {
        new RuleModel {
            Label = "Order ID",
            Field = "OrderID",
            Type = "Number",
            Operator = "greater",
            Value = 1000
        },
        new RuleModel {
            Label = "Status",
            Field = "Status",
            Type = "String",
            Operator = "equal",
            Value = "Pending"
        }
    };

    public class Order {
        public int OrderID { get; set; }
        public string Status { get; set; }
        public DateTime OrderDate { get; set; }
    }
}
```

## Nested Groups

Create complex queries with nested groups (AND/OR logic):

```razor
<SfQueryBuilder TValue="Product" @ref="QueryBuilder">
    <QueryBuilderRule Condition="and" Rules="@NestedRules"></QueryBuilderRule>
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="Name" Label="Product Name" Type="ColumnType.String"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Category" Label="Category" Type="ColumnType.String"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Price" Label="Price" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="InStock" Label="In Stock" Type="ColumnType.Boolean"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private SfQueryBuilder<Product> QueryBuilder;
    
    private List<RuleModel> NestedRules = new() {
        new RuleModel {
            Label = "Category",
            Field = "Category",
            Type = "String",
            Operator = "equal",
            Value = "Electronics"
        },
        new RuleModel {
            Condition = "or",  // Nested group with OR condition
            Rules = new List<RuleModel> {
                new RuleModel {
                    Label = "Price",
                    Field = "Price",
                    Type = "Number",
                    Operator = "lessthan",
                    Value = 100
                },
                new RuleModel {
                    Label = "In Stock",
                    Field = "InStock",
                    Type = "Boolean",
                    Operator = "equal",
                    Value = true
                }
            }
        }
    };

    public class Product {
        public string Name { get; set; }
        public string Category { get; set; }
        public decimal Price { get; set; }
        public bool InStock { get; set; }
    }
}
```

## Available Operators by Type

### String Operators
- `equal` - Equals
- `notequal` - Not equals
- `contains` - Contains
- `notcontains` - Does not contain
- `startswith` - Starts with
- `endswith` - Ends with
- `in` - In list
- `notin` - Not in list

### Number Operators
- `equal` - Equals
- `notequal` - Not equals
- `lessthan` - Less than
- `lessthanorequal` - Less than or equal
- `greaterthan` - Greater than
- `greaterthanorequal` - Greater than or equal
- `between` - Between
- `notbetween` - Not between
- `in` - In list
- `notin` - Not in list

### Date Operators
- `equal` - Equals
- `notequal` - Not equals
- `lessthan` - Before
- `lessthanorequal` - Before or on
- `greaterthan` - After
- `greaterthanorequal` - After or on
- `between` - Between dates
- `notbetween` - Not between dates

### Boolean Operators
- `equal` - Is true/false

## Component References

To programmatically access the Query Builder:

```razor
<SfQueryBuilder TValue="Order" @ref="QueryBuilder">
    <!-- columns -->
</SfQueryBuilder>

@code {
    private SfQueryBuilder<Order> QueryBuilder;
    
    private async Task DoSomething() {
        // Access methods on QueryBuilder reference
        var rules = QueryBuilder.GetRules();
    }
}
```

## Next Steps

- [Filtering & Rules](filtering-and-rules.md) - Add/delete rules programmatically
- [Data Binding](data-binding.md) - Bind and filter data
- [Events & Callbacks](events-and-callbacks.md) - Handle query changes
