# Customization & Styling

This guide covers CSS customization, theming, templates, and styling options for the Query Builder component.

## CSS Customization

### Theme Selection

Choose a theme by linking the appropriate CSS file in your host page:

```html
<!-- Bootstrap 5 Theme (Default) -->
<link rel="stylesheet" href="_content/Syncfusion.Blazor.Themes/bootstrap5.css">

<!-- Or use alternative themes -->
<link rel="stylesheet" href="_content/Syncfusion.Blazor.Themes/material3.css">
<link rel="stylesheet" href="_content/Syncfusion.Blazor.Themes/fluent2.css">
<link rel="stylesheet" href="_content/Syncfusion.Blazor.Themes/tailwindcss.css">
```

### Custom CSS Classes

Style Query Builder elements with custom CSS:

```css
/* Custom Query Builder styling */
.e-querybuilder {
    background-color: #f5f5f5;
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 15px;
}

/* Style rules */
.e-rule {
    background-color: white;
    border: 1px solid #e0e0e0;
    border-radius: 4px;
    margin-bottom: 10px;
    padding: 10px;
}

/* Style groups */
.e-group {
    background-color: #fafafa;
    border-left: 4px solid #007bff;
    border-radius: 4px;
    padding: 12px;
    margin-bottom: 10px;
}

/* Style buttons */
.e-querybuilder .e-addrule,
.e-querybuilder .e-addgroup {
    background-color: #007bff;
    color: white;
    border-radius: 4px;
}

/* Operator dropdown */
.e-querybuilder .e-operator-select {
    border-color: #e0e0e0;
}
```

### Inline Styles

Apply inline styles directly to components:

```razor
<SfQueryBuilder TValue="Order" Style="background-color: #fafafa; padding: 20px; border-radius: 8px;">
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>
```

## Component Styling Classes

### Available CSS Classes

| Class | Purpose |
|-------|---------|
| `.e-querybuilder` | Main container |
| `.e-rule` | Individual rule element |
| `.e-group` | Group container |
| `.e-group-header` | Group header (AND/OR selector) |
| `.e-field-select` | Field dropdown |
| `.e-operator-select` | Operator dropdown |
| `.e-value-field` | Value input field |
| `.e-addrule` | Add rule button |
| `.e-addgroup` | Add group button |
| `.e-deletegroup` | Delete group button |
| `.e-deleterule` | Delete rule button |

### Style Specific Elements

```css
/* Custom field dropdown styling */
.e-querybuilder .e-field-select {
    background-color: #e8f4f8;
    border: 2px solid #007bff;
    border-radius: 4px;
    font-weight: 500;
}

/* Custom operator styling */
.e-querybuilder .e-operator-select {
    background-color: white;
    border: 1px solid #ddd;
}

/* Custom value input */
.e-querybuilder .e-value-field input {
    background-color: #fffaf0;
    border: 1px solid #e0a070;
    padding: 8px;
}

/* Button styling */
.e-querybuilder .e-addrule,
.e-querybuilder .e-addgroup {
    background-color: #28a745;
    color: white;
    border-radius: 4px;
    padding: 8px 12px;
    font-weight: 500;
}

.e-querybuilder .e-deletegroup,
.e-querybuilder .e-deleterule {
    background-color: #dc3545;
    color: white;
    border-radius: 4px;
}
```

## Templates

### Custom Field Template

```razor
@using Syncfusion.Blazor.QueryBuilder

<SfQueryBuilder TValue="Order">
    <QueryBuilderTemplates>
        <FieldTemplate>
            <div style="padding: 8px;">
                <strong>@context.Label</strong>
                <p style="font-size: 12px; color: #666;">@context.Field</p>
            </div>
        </FieldTemplate>
    </QueryBuilderTemplates>
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>
```

### Custom Operator Template

```razor
<QueryBuilderTemplates>
    <OperatorTemplate>
        <select style="padding: 5px; border: 1px solid #ddd;">
            <option value="equal">Is Equal To</option>
            <option value="notequal">Is Not Equal To</option>
            <option value="contains">Contains Text</option>
            <option value="lessthan">Less Than</option>
            <option value="greaterthan">Greater Than</option>
        </select>
    </OperatorTemplate>
</QueryBuilderTemplates>
```

### Custom Value Template

```razor
<QueryBuilderTemplates>
    <ValueTemplate>
        @if (context.Field == "Status") {
            <select style="padding: 5px; width: 100%; border: 1px solid #ddd;">
                <option value="Pending">Pending</option>
                <option value="Processing">Processing</option>
                <option value="Completed">Completed</option>
                <option value="Cancelled">Cancelled</option>
            </select>
        } else if (context.Type == "Number") {
            <input type="number" 
                   style="padding: 5px; width: 100%; border: 1px solid #ddd;" 
                   placeholder="Enter value" />
        } else {
            <input type="text" 
                   style="padding: 5px; width: 100%; border: 1px solid #ddd;" 
                   placeholder="Enter value" />
        }
    </ValueTemplate>
</QueryBuilderTemplates>
```

## Dark Mode Support

Implement dark mode styling:

```css
@media (prefers-color-scheme: dark) {
    .e-querybuilder {
        background-color: #1e1e1e;
        color: #e0e0e0;
    }

    .e-rule {
        background-color: #2d2d2d;
        border: 1px solid #444;
    }

    .e-group {
        background-color: #252525;
        border-left-color: #4a9eff;
    }

    .e-querybuilder input,
    .e-querybuilder select {
        background-color: #2d2d2d;
        color: #e0e0e0;
        border-color: #444;
    }
}
```

## Responsive Design

### Mobile-Optimized Styling

```css
/* Tablet devices */
@media (max-width: 768px) {
    .e-querybuilder {
        padding: 10px;
    }

    .e-rule,
    .e-group {
        padding: 8px;
        margin-bottom: 8px;
    }

    .e-querybuilder button {
        font-size: 14px;
        padding: 8px;
    }
}

/* Mobile devices */
@media (max-width: 480px) {
    .e-querybuilder {
        padding: 8px;
    }

    .e-field-select,
    .e-operator-select,
    .e-value-field input {
        width: 100% !important;
        margin-bottom: 8px;
        font-size: 16px;
    }

    .e-rule {
        display: grid;
        grid-template-columns: 1fr;
        gap: 8px;
    }

    .e-querybuilder button {
        width: 100%;
        margin-top: 5px;
    }
}
```

## Custom Operator Definitions

Define custom operators and their formatting:

```razor
@code {
    private List<OperatorModel> CustomOperators = new() {
        new() { Text = "Is Equal To", Value = "equal" },
        new() { Text = "Is Greater Than", Value = "greaterthan" },
        new() { Text = "Is Between", Value = "between" },
        new() { Text = "In List", Value = "in" }
    };
    
    public class OperatorModel {
        public string Text { get; set; }
        public string Value { get; set; }
    }
}
```

## Accessibility Styling

### Focus States

```css
/* Keyboard navigation focus */
.e-querybuilder input:focus,
.e-querybuilder select:focus,
.e-querybuilder button:focus {
    outline: 2px solid #007bff;
    outline-offset: 2px;
}

/* High contrast mode */
@media (prefers-contrast: more) {
    .e-querybuilder {
        border: 2px solid #000;
    }

    .e-querybuilder button {
        border: 2px solid #000;
    }
}
```

## Next Steps

- [Advanced Features](advanced-features.md) - Import/export and state persistence
- [Localization](localization-and-accessibility.md) - Accessibility best practices
- [Troubleshooting](troubleshooting.md) - Styling issues
