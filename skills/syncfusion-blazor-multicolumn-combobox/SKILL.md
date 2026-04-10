---
name: syncfusion-blazor-multicolumn-combobox
description: Comprehensive guide for implementing Syncfusion Blazor MultiColumn ComboBox component. Use this when displaying structured data in a dropdown with multiple columns, filtering, selection, paging, virtualization, grouping, and customization options. Perfect for complex data structures and advanced filtering scenarios in Blazor applications. Covers all MultiColumn ComboBox features from Syncfusion.Blazor.MultiColumnComboBox package.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion Blazor MultiColumn ComboBox

The **MultiColumn ComboBox** component is a dropdown control that displays data in multiple columns with built-in features like filtering, selection, paging, virtualization, grouping, and customization. It combines the capabilities of a traditional ComboBox with a data grid layout.

## When to Use This Skill

Use this skill when you need to:
- Display structured data in a dropdown with **multiple columns**
- Implement **filtering and search** capabilities across column data
- Bind to local or **remote data sources** (Web API, OData)
- Support **value selection** with flexible binding options
- Add **paging, virtualization, or grouping** to large datasets
- Customize **appearance, styling, and validation** in forms
- Implement **accessible, keyboard-navigable** dropdowns
- Create **server-side or web app** integrated MultiColumn ComboBox components

## Component Overview

The MultiColumn ComboBox provides:
- **Multi-column layout** for displaying complex data structures
- **Advanced filtering** (local/remote, case-sensitive, multi-column)
- **Data binding** (local arrays, Web API, OData, custom adaptors)
- **Selection modes** with value binding (simple, tuple, complex objects)
- **Performance features** (paging, virtualization, offline mode)
- **Customization** (templates, styling, appearance)
- **Form integration** with validation and floating labels
- **Accessibility** with WCAG compliance and keyboard support

---

## Documentation Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and package setup
- Basic MultiColumn ComboBox implementation
- Template structure
- Creating your first MultiColumn ComboBox

### Columns and Data Display
📄 **Read:** [references/columns-and-templates.md](references/columns-and-templates.md)
- Column definition and configuration
- Column templates and formatting
- Column headers and text alignment
- Grid settings (row height, alternating rows)

### Data Binding Options
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Local data binding (arrays, collections)
- Web API and OData binding
- Action events (ActionBegin, ActionComplete, ActionFailure)
- Observable collections and dynamic binding

### Filtering and Search
📄 **Read:** [references/filtering-and-search.md](references/filtering-and-search.md)
- Filter types and custom filtering logic
- Multi-column filtering techniques
- Case-sensitive filtering
- Remote data filtering

### Selection and Value Binding
📄 **Read:** [references/selection-and-values.md](references/selection-and-values.md)
- Single and multiple value binding
- Value selection patterns
- Custom values and enum binding
- Working with tuples and complex objects

### Advanced Features and Settings
📄 **Read:** [references/features-and-settings.md](references/features-and-settings.md)
- Paging and virtualization for performance
- Grouping data by columns
- Popup settings (size, position, resize)
- Placeholder and floating labels

### Styling and Appearance
📄 **Read:** [references/styling-and-appearance.md](references/styling-and-appearance.md)
- CSS classes and custom styling
- Placeholder and float label customization
- Appearance tweaks (alt rows, grid lines, text wrapping)
- Theme integration

### Accessibility and Localization
📄 **Read:** [references/accessibility-localization.md](references/accessibility-localization.md)
- WCAG compliance and keyboard navigation
- ARIA attributes and screen reader support
- Form validation integration
- Localization for international apps

### Advanced Patterns and Integration
📄 **Read:** [references/advanced-patterns.md](references/advanced-patterns.md)
- Server-side and web app integration
- Custom data adaptors
- Enum and dynamic object binding
- Offline mode and performance optimization

---

## Quick Start Example

Here's a minimal MultiColumn ComboBox implementation:

```razor
@using Syncfusion.Blazor.MultiColumnComboBox

<SfMultiColumnComboBox TValue="string" TItem="OrderData" 
                       DataSource="@OrderList" 
                       TextField="CustomerName"
                       ValueField="OrderID"
                       Placeholder="Select an order">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="OrderID" Header="Order ID" Width="100px"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="CustomerName" Header="Customer" Width="150px"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="TotalAmount" Header="Amount" Width="100px" Format="C2"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private List<OrderData> OrderList = new();

    protected override void OnInitialized()
    {
        OrderList = new()
        {
            new OrderData { OrderID = "1001", CustomerName = "Alice Johnson", TotalAmount = 150.50m },
            new OrderData { OrderID = "1002", CustomerName = "Bob Smith", TotalAmount = 280.75m },
            new OrderData { OrderID = "1003", CustomerName = "Carol White", TotalAmount = 420.00m }
        };
    }

    public class OrderData
    {
        public string OrderID { get; set; }
        public string CustomerName { get; set; }
        public decimal TotalAmount { get; set; }
    }
}
```

---

## Common Patterns

### 1. **Filtering User Input**
When users type in the search box, the ComboBox filters items. Use the `AllowFiltering` property and customize with `FilterType`.

```razor
<SfMultiColumnComboBox TValue="string" TItem="ProductData" 
                       DataSource="@Products"
                       AllowFiltering="true"
                       FilterType="FilterType.Contains">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

### 2. **Remote Data Loading**
Bind to Web API or OData services using the `Query` property with data adaptor.

```razor
<SfMultiColumnComboBox TValue="string" TItem="OrderData"
                       Query="@DataQuery">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private Query DataQuery = new Query().From("Orders").Select(new List<string> { "OrderID", "CustomerName" });
}
```

### 3. **Handling Selection Change**
Use the `ValueChange` event to respond when user selects an item.

```razor
<SfMultiColumnComboBox TValue="string" TItem="OrderData"
                       ValueChange="@OnValueChange">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

@code {
    private void OnValueChange(ValueChangeEventArgs<string, OrderData> args)
    {
        // args.Value = selected value
        // args.ItemData = selected item object
    }
}
```

### 4. **Paging Large Datasets**
Enable paging to show limited rows and load more on demand.

```razor
<SfMultiColumnComboBox TValue="string" TItem="OrderData"
                       AllowPaging="true"
                       PageSize="10">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

### 5. **Form Validation**
Integrate with EditForm for validation and required field checks.

```razor
<EditForm Model="@FormModel">
    <SfMultiColumnComboBox TValue="string" TItem="OrderData"
                           @bind-Value="@FormModel.SelectedOrderID"
                           DataSource="@Orders">
        <MultiColumnComboboxColumns>
            <!-- Columns -->
        </MultiColumnComboboxColumns>
    </SfMultiColumnComboBox>
    
    <ValidationMessage For="@(() => FormModel.SelectedOrderID)" />
</EditForm>
```

---

## Key Properties
TextField` | Maps display text field | `TextField="Name"` |
| `ValueField` | Maps value field | `ValueField="ID"` |

| Property | Description | Example |
|----------|-------------|---------|
| `DataSource` | Data collection to bind | `DataSource="@Products"` |
| `TextField` | Maps display text field | `TextField="Name"` |
| `ValueField` | Maps value field | `ValueField="ID"` |
| `Value` / `@bind-Value` | Gets/sets selected value | `@bind-Value="@SelectedID"` |
| `AllowFiltering` | Enables search/filter capability | `AllowFiltering="true"` |
| `FilterType` | Filter matching behavior | `FilterType="FilterType.Contains"` |
| `Placeholder` | Input hint text | `Placeholder="Select..."` |
| `AllowPaging` | Enables paging | `AllowPaging="true"` |
| `PageSize` | Items per page (with paging) | `PageSize="10"` |
| `GroupByField` | Groups items by field | `GroupByField="Category"` |
| `Query` | Data query for remote sources | `Query="@new Query().From('Orders')"` |
| `Disabled` / `ReadOnly` | Control states | `Dis
---

## Common Use Cases

1. **Order Selection Form** - Users pick orders from a grid-like dropdown with order ID, customer, and amount
2. **Employee Lookup** - Search employees by name, email, or ID across multiple columns
3. **Product Catalog** - Display products with category, price, and stock in a multi-column dropdown
4. **Location Selector** - Select cities/regions with country, state, and timezone information
5. **Account Selection** - Pick from accounts showing number, type, and balance in one dropdown

---

## What's Next?

Start with **Getting Started** to install and create your first MultiColumn ComboBox, then explore other guides based on your needs. Each reference provides complete code examples and edge cases.

**Questions?** Check the relevant reference guide for your specific use case.
