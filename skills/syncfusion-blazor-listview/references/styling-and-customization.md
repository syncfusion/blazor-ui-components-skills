# Styling and Customization in Blazor ListView

## Table of Contents
- [CSS Selectors and Classes](#css-selectors-and-classes)
- [Item Styling](#item-styling)
- [Header and Group Styling](#header-and-group-styling)
- [Hover and Selected States](#hover-and-selected-states)
- [Theme Integration](#theme-integration)
- [HTML Attributes](#html-attributes)

## CSS Selectors and Classes

### ListView Container

```css
.e-listview {
    border: 5px solid rgb(173, 255, 47);
    background-color: #fff;
}
```

### Basic CSS Structure

| Class | Purpose |
|-------|---------|
| `.e-listview` | Main ListView container |
| `.e-list-item` | Individual list item |
| `.e-list-header` | Header container |
| `.e-list-group-item` | Group header item |
| `.e-list-template` | Template-enabled list |
| `.e-list-wrapper` | Item template wrapper |
| `.e-list-content` | Item content area |

## Item Styling

### Basic Item Customization

```css
.e-listview .e-list-item {
    text-align: center;
    color: pink;
    background-color: #2fa1ff;
    padding: 16px;
    border-bottom: 1px solid #e0e0e0;
}
```

### Item Text Styling

```css
.e-listview .e-list-item {
    font-size: 16px;
    font-weight: 500;
    color: #333;
    padding: 12px 16px;
}

.e-listview .e-list-item span {
    display: block;
    margin: 4px 0;
}
```

### Custom Item Backgrounds

```css
/* Alternating row colors */
.e-listview .e-list-item:nth-child(odd) {
    background-color: #f9f9f9;
}

.e-listview .e-list-item:nth-child(even) {
    background-color: #ffffff;
}

/* Category-specific styling */
.e-listview .e-list-item.category-premium {
    background-color: #fff3e0;
    border-left: 4px solid #ff9800;
}

.e-listview .e-list-item.category-popular {
    background-color: #e3f2fd;
    border-left: 4px solid #2196f3;
}
```

## Header and Group Styling

### Header Customization

```css
.e-listview .e-list-header {
    color: #2fa1ff;
    justify-content: center;
    background-color: #f5f5f5;
    font-weight: bold;
    font-size: 18px;
    padding: 16px;
    border-bottom: 2px solid #2fa1ff;
}
```

### Group Header Styling

```css
.e-listview .e-list-group-item {
    color: rgb(173, 255, 47);
    background-color: maroon;
    text-align: end;
    padding: 12px 16px;
    font-weight: bold;
}

/* Modern group header */
.e-listview .e-list-group-item {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding: 12px 16px;
    font-weight: 600;
    display: flex;
    justify-content: space-between;
    align-items: center;
}
```

### Group Item Count Badge

```css
.e-listview .e-list-group-item::after {
    content: attr(data-count);
    background-color: rgba(255, 255, 255, 0.3);
    padding: 4px 12px;
    border-radius: 4px;
    font-size: 12px;
}
```

## Hover and Selected States

### Hover State

```css
.e-listview .e-list-item.e-hover {
    color: red;
    background-color: rgb(173, 255, 47);
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

/* Smooth transition */
.e-listview .e-list-item {
    transition: all 0.2s ease;
}

.e-listview .e-list-item:hover {
    background-color: #f0f0f0;
    transform: translateX(4px);
    cursor: pointer;
}
```

### Selected Item Styling

```css
.e-listview .e-list-item.e-focused {
    color: #2fa1ff;
    background-color: rgb(0, 15, 100);
    border-left: 4px solid #2fa1ff;
}

/* Modern selection */
.e-listview .e-list-item.e-focused {
    background-color: #e3f2fd;
    border-left: 4px solid #2196f3;
    box-shadow: inset 0 0 0 1px #2196f3;
}
```

### Checkbox Selected States

```css
/* Checkbox checked AND hovered */
.e-listview .e-list-item.e-checklist.e-hover.e-active {
    color: rgb(83, 5, 79);
    background-color: rgb(173, 255, 47);
}

/* Checkbox checked AND focused */
.e-listview .e-list-item.e-checklist.e-focused.e-active {
    color: rgb(83, 5, 79);
    background-color: rgb(0, 15, 100);
}

/* Just checked */
.e-listview .e-list-item.e-checklist {
    background-color: #f5f5f5;
}
```

## Theme Integration

### Fluent 2 Theme Customization

```css
/* Fluent 2 color variables */
:root {
    --e-primary: #0078d4;
    --e-border: #e1e1e1;
    --e-surface-light: #ffffff;
    --e-surface-lighter: #f7f7f7;
}

.e-listview {
    background-color: var(--e-surface-light);
    border: 1px solid var(--e-border);
}

.e-listview .e-list-item.e-focused {
    background-color: var(--e-primary);
    color: white;
}
```

### Bootstrap 5 Theme

```css
.e-listview .e-list-item {
    border-bottom: 1px solid #dee2e6;
}

.e-listview .e-list-item:hover {
    background-color: #f8f9fa;
}

.e-listview .e-list-item.e-focused {
    background-color: #0d6efd;
    color: white;
}
```

### Material Design Theme

```css
.e-listview .e-list-item {
    padding: 12px 16px;
    border-bottom: 1px solid rgba(0, 0, 0, 0.12);
}

.e-listview .e-list-item:hover {
    background-color: rgba(0, 0, 0, 0.04);
}

.e-listview .e-list-item.e-focused {
    background-color: #f0f0f0;
}
```

## HTML Attributes

### Support for Custom Attributes

Use the `HtmlAttributes` field to add custom HTML attributes to items:

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView DataSource="@Items">
    <ListViewFieldSettings TValue="Item" 
                          Id="Id" 
                          Text="Text"
                          HtmlAttributes="Attributes">
    </ListViewFieldSettings>
</SfListView>

@code {
    List<Item> Items = new List<Item>
    {
        new Item 
        { 
            Id = "1", 
            Text = "Item 1",
            Attributes = new Dictionary<string, object> 
            { 
                { "data-category", "premium" },
                { "title", "Premium Item" }
            }
        },
        new Item 
        { 
            Id = "2", 
            Text = "Item 2",
            Attributes = new Dictionary<string, object> 
            { 
                { "data-category", "standard" },
                { "title", "Standard Item" }
            }
        }
    };

    public class Item
    {
        public string Id { get; set; }
        public string Text { get; set; }
        public Dictionary<string, object> Attributes { get; set; }
    }
}
```

### Style Items with Data Attributes

```css
/* Premium items */
.e-listview .e-list-item[data-category="premium"] {
    background-color: #fff3e0;
    border-left: 4px solid #ff9800;
}

/* Standard items */
.e-listview .e-list-item[data-category="standard"] {
    background-color: #f5f5f5;
}

/* Apply styles to data-attributes */
.e-listview .e-list-item[data-status="active"] {
    color: #4caf50;
    font-weight: bold;
}

.e-listview .e-list-item[data-status="inactive"] {
    color: #999;
    opacity: 0.7;
}
```

### ARIA and Accessibility Attributes

```csharp
var item = new Item
{
    Id = "1",
    Text = "Action Item",
    Attributes = new Dictionary<string, object>
    {
        { "role", "menuitem" },
        { "aria-label", "Action Item - Click to execute" },
        { "aria-pressed", "false" }
    }
};
```

### Complete Styling Example

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView DataSource="@Items" 
            CssClass="custom-listview"
            ShowHeader="true"
            HeaderTitle="Styled List">
    <ListViewFieldSettings TValue="Item" Id="Id" Text="Text"></ListViewFieldSettings>
</SfListView>

<style>
    .custom-listview {
        background-color: #ffffff;
        border: 1px solid #e0e0e0;
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
        overflow: hidden;
    }

    .custom-listview .e-list-header {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        padding: 16px;
        font-weight: bold;
    }

    .custom-listview .e-list-item {
        padding: 12px 16px;
        border-bottom: 1px solid #f0f0f0;
        transition: all 0.3s ease;
    }

    .custom-listview .e-list-item:hover {
        background-color: #f5f5f5;
        transform: translateX(4px);
    }

    .custom-listview .e-list-item.e-focused {
        background-color: #667eea;
        color: white;
        border-left: 4px solid #764ba2;
    }
</style>

@code {
    List<Item> Items = new List<Item>
    {
        new Item { Id = "1", Text = "Item 1" },
        new Item { Id = "2", Text = "Item 2" },
        new Item { Id = "3", Text = "Item 3" }
    };

    public class Item
    {
        public string Id { get; set; }
        public string Text { get; set; }
    }
}
```

---

**Related Topics:**
- [Templating and Rendering](templating-and-rendering.md) - Template styling
- [Advanced Layouts](advanced-layouts.md) - Layout-specific CSS
- [Item Selection and Actions](item-selection-and-actions.md) - Selection state styling

