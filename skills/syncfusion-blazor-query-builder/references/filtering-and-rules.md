# Filtering, Rules & Groups

This guide covers the core functionality of adding, deleting, and managing rules and groups programmatically in the Query Builder.

## Table of Contents

- [Adding Rules](#adding-rules)
- [Deleting Rules](#deleting-rules)
- [Adding Groups](#adding-groups)
- [Deleting Groups](#deleting-groups)
- [Operators & Conditions](#operators--conditions)
- [Complex Nested Patterns](#complex-nested-patterns)
- [User Interaction](#user-interaction)
- [Validation & Constraints](#validation--constraints)

## Adding Rules

### Basic Rule Addition

Add a rule programmatically using the `AddRule` method:

```razor
<SfButton OnClick="AddRule">Add Rule</SfButton>

<SfQueryBuilder TValue="Order" @ref="QueryBuilder">
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Status" Label="Status" Type="ColumnType.String"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private SfQueryBuilder<Order> QueryBuilder;
    
    private void AddRule() {
        var rule = new RuleModel {
            Label = "Order ID",
            Field = "OrderID",
            Type = "Number",
            Operator = "greater",
            Value = 5000
        };
        QueryBuilder.AddRule(rule, "group0");  // Add to root group
    }

    public class Order {
        public int OrderID { get; set; }
        public string Status { get; set; }
    }
}
```

**Parameters:**
- `rule` - RuleModel with Field, Operator, Value
- `groupId` - Parent group ID (e.g., "group0" for root)

### Multiple Rules

```razor
private void AddMultipleRules() {
    var rule1 = new RuleModel {
        Field = "OrderID",
        Type = "Number",
        Operator = "greater",
        Value = 1000
    };
    
    var rule2 = new RuleModel {
        Field = "Status",
        Type = "String",
        Operator = "equal",
        Value = "Completed"
    };
    
    QueryBuilder.AddRule(rule1, "group0");
    QueryBuilder.AddRule(rule2, "group0");
}
```

## Deleting Rules

Remove a rule by its ID:

```razor
<SfButton OnClick="DeleteRule">Delete First Rule</SfButton>

@code {
    private void DeleteRule() {
        // Delete rule with ID "rule0"
        QueryBuilder.DeleteRule("rule0");
    }
}
```

## Adding Groups

Groups combine multiple rules with AND/OR logic:

```razor
<SfButton OnClick="AddGroup">Add Condition Group</SfButton>

@code {
    private void AddGroup() {
        var group = new RuleModel {
            Condition = "or",  // AND or OR
            Rules = new List<RuleModel> {
                new RuleModel {
                    Field = "Category",
                    Type = "String",
                    Operator = "equal",
                    Value = "Electronics"
                },
                new RuleModel {
                    Field = "Price",
                    Type = "Number",
                    Operator = "lessthan",
                    Value = 500
                }
            }
        };
        
        QueryBuilder.AddGroup(group, "group0");
    }
}
```

## Deleting Groups

Remove an entire group:

```razor
<SfButton OnClick="DeleteGroup">Delete Group</SfButton>

@code {
    private void DeleteGroup() {
        // Delete group with ID "group1"
        QueryBuilder.DeleteGroup("group1");
    }
}
```

## Operators & Conditions

### String Field Filtering

```razor
@code {
    private void StringFiltering() {
        // Contains pattern
        var rule1 = new RuleModel {
            Field = "CustomerName",
            Type = "String",
            Operator = "contains",
            Value = "Corp"
        };
        
        // Starts with
        var rule2 = new RuleModel {
            Field = "Email",
            Type = "String",
            Operator = "startswith",
            Value = "admin"
        };
        
        // In list
        var rule3 = new RuleModel {
            Field = "Status",
            Type = "String",
            Operator = "in",
            Value = new[] { "Active", "Pending" }
        };
        
        QueryBuilder.AddRule(rule1, "group0");
        QueryBuilder.AddRule(rule2, "group0");
        QueryBuilder.AddRule(rule3, "group0");
    }
}
```

### Number Field Filtering

```razor
@code {
    private void NumberFiltering() {
        // Greater than
        var rule1 = new RuleModel {
            Field = "Salary",
            Type = "Number",
            Operator = "greaterthan",
            Value = 50000
        };
        
        // Between range
        var rule2 = new RuleModel {
            Field = "Age",
            Type = "Number",
            Operator = "between",
            Value = new[] { 25, 65 }
        };
        
        // Not equal
        var rule3 = new RuleModel {
            Field = "Quantity",
            Type = "Number",
            Operator = "notequal",
            Value = 0
        };
        
        QueryBuilder.AddRule(rule1, "group0");
        QueryBuilder.AddRule(rule2, "group0");
        QueryBuilder.AddRule(rule3, "group0");
    }
}
```

### Date Field Filtering

```razor
@code {
    private void DateFiltering() {
        // After date
        var rule1 = new RuleModel {
            Field = "OrderDate",
            Type = "Date",
            Operator = "greaterthan",
            Value = new DateTime(2024, 1, 1)
        };
        
        // Between date range
        var rule2 = new RuleModel {
            Field = "HireDate",
            Type = "Date",
            Operator = "between",
            Value = new[] { 
                new DateTime(2020, 1, 1), 
                new DateTime(2024, 12, 31) 
            }
        };
        
        QueryBuilder.AddRule(rule1, "group0");
        QueryBuilder.AddRule(rule2, "group0");
    }
}
```

## Complex Nested Patterns

### Three-Level Nesting

```razor
@code {
    private void ComplexNesting() {
        // Level 1: Root group (AND)
        var level1Group = new RuleModel {
            Condition = "and",
            Rules = new List<RuleModel> {
                // Level 2: Nested group (OR)
                new RuleModel {
                    Condition = "or",
                    Rules = new List<RuleModel> {
                        new RuleModel {
                            Field = "Category",
                            Type = "String",
                            Operator = "equal",
                            Value = "Electronics"
                        },
                        new RuleModel {
                            Field = "Category",
                            Type = "String",
                            Operator = "equal",
                            Value = "Software"
                        }
                    }
                },
                // Level 2: Another rule
                new RuleModel {
                    Field = "Price",
                    Type = "Number",
                    Operator = "lessthan",
                    Value = 1000
                }
            }
        };
        
        QueryBuilder.AddGroup(level1Group, "group0");
    }
}
```

### Business Logic Example

```razor
private void BusinessLogicFilter() {
    // Find orders matching:
    // (Status = "Pending" OR Status = "Processing") 
    // AND (Amount > 500 OR CreatedDate > 7 days ago)
    
    var statusGroup = new RuleModel {
        Condition = "or",
        Rules = new List<RuleModel> {
            new RuleModel {
                Field = "Status",
                Type = "String",
                Operator = "equal",
                Value = "Pending"
            },
            new RuleModel {
                Field = "Status",
                Type = "String",
                Operator = "equal",
                Value = "Processing"
            }
        }
    };
    
    var amountGroup = new RuleModel {
        Condition = "or",
        Rules = new List<RuleModel> {
            new RuleModel {
                Field = "Amount",
                Type = "Number",
                Operator = "greaterthan",
                Value = 500
            },
            new RuleModel {
                Field = "CreatedDate",
                Type = "Date",
                Operator = "greaterthan",
                Value = DateTime.Now.AddDays(-7)
            }
        }
    };
    
    QueryBuilder.AddGroup(statusGroup, "group0");
    QueryBuilder.AddGroup(amountGroup, "group0");
}
```

## User Interaction

### ShowButtons Configuration

Control which action buttons display in the UI:

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
- `RuleDelete` - Delete rule button
- `GroupDelete` - Delete group button
- `GroupInsert` - Add group button

### Capture User Actions

```razor
<SfQueryBuilder TValue="Order" DataSource="@Orders">
    <QueryBuilderEvents TValue="Order" RuleChanged="OnRuleChanged"></QueryBuilderEvents>
    <QueryBuilderColumns>
        <!-- columns -->
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private void OnRuleChanged(RuleChangeEventArgs args) {
        Console.WriteLine($"Rule action: {args.Action}");  // add, remove, update
        Console.WriteLine($"Rule ID: {args.RuleID}");
    }
}
```

## Validation & Constraints

### Prevent Invalid Rules

```razor
@code {
    private void ValidateAndAdd() {
        var rule = new RuleModel {
            Field = "Salary",
            Type = "Number",
            Operator = "equal",
            Value = -1000  // Invalid: negative salary
        };
        
        // Validate before adding
        if (ValidateRule(rule)) {
            QueryBuilder.AddRule(rule, "group0");
        }
    }
    
    private bool ValidateRule(RuleModel rule) {
        // Custom validation logic
        if (rule.Field == "Salary" && rule.Type == "Number") {
            if (rule.Value is int val && val < 0) {
                Console.WriteLine("Invalid: Salary cannot be negative");
                return false;
            }
        }
        return true;
    }
}
```

## Next Steps

- [Data Binding](data-binding.md) - Apply filtered queries to data
- [Events & Callbacks](events-and-callbacks.md) - Handle rule changes
- [Advanced Features](advanced-features.md) - Import/export and state persistence
