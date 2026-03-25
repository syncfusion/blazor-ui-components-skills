# Styling and Appearance

## Table of Contents
- [CSS Classes](#css-classes)
- [Disabled State](#disabled-state)
- [CssClass Property](#cssclass-property)
- [Popup Sizing](#popup-sizing)
- [Z-Index Control](#z-index-control)
- [Theme Support](#theme-support)

## CSS Classes

The Dropdown Tree component supports built-in CSS classes for styling:

### Success Style

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    CssClass="e-success">
    <ChildContent>
        <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
    </ChildContent>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", HasChild = true }
    };

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
        public string? PId { get; set; }
    }
}
```

### Available CSS Classes

| CSS Class | Style | Use Case |
|-----------|-------|----------|
| `e-success` | Green border and focus color | Success/valid state |
| `e-warning` | Yellow/orange border and focus color | Warning/caution state |
| `e-error` | Red border and focus color | Error/invalid state |
| `e-outline` | Outlined style variant | Alternative appearance |

### Warning Style Example

```razor
<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select with caution" 
    CssClass="e-warning">
    <ChildContent>
        <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
    </ChildContent>
</SfDropDownTree>
```

### Error Style Example

```razor
<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select a valid option" 
    CssClass="e-error">
    <ChildContent>
        <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
    </ChildContent>
</SfDropDownTree>
```

## Disabled State

Disable the component using the `Enabled` property:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Enabled="false">
    <ChildContent>
        <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
    </ChildContent>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan" }
    };

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
        public string? PId { get; set; }
    }
}
```

**Behavior when disabled:**
- Input field becomes read-only
- Dropdown cannot be opened
- Selection cannot be changed
- Appears with reduced opacity/gray styling
- Tab focus not allowed

### Conditional Enabling

```razor
@using Syncfusion.Blazor.Navigations

<button @onclick="@(() => { isEnabled = !isEnabled; })">Toggle Enable</button>

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Enabled="isEnabled">
    <ChildContent>
        <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
    </ChildContent>
</SfDropDownTree>

@code {
    bool isEnabled = true;

    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan" }
    };

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
        public string? PId { get; set; }
    }
}
```

## CssClass Property

Apply custom CSS classes to the component:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    CssClass="my-custom-dropdown e-success">
    <ChildContent>
        <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
    </ChildContent>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan" }
    };

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
        public string? PId { get; set; }
    }
}

<style>
    .my-custom-dropdown {
        border: 2px solid #667eea !important;
        border-radius: 8px !important;
    }

    .my-custom-dropdown.e-focus::before {
        border-color: #667eea !important;
    }

    .my-custom-dropdown .e-input {
        font-size: 16px;
        padding: 12px;
    }
</style>
```

## Popup Sizing

Control popup dimensions using `PopupWidth` and `PopupHeight`:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee"
    PopupWidth="100%"
    PopupHeight="300px">
    <ChildContent>
        <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
    </ChildContent>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", HasChild = true },
        new EmployeeData() { Id = "3", PId = "2", Name = "Andrew Fuller" },
        new EmployeeData() { Id = "4", PId = "1", Name = "Nancy Davolio" },
        new EmployeeData() { Id = "5", Name = "Michael Suyama", HasChild = true },
        new EmployeeData() { Id = "6", PId = "5", Name = "Robert King" }
    };

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
        public bool Expanded { get; set; }
        public string? PId { get; set; }
    }
}
```

### Popup Sizing Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `PopupWidth` | `string` | `100%` | Width of popup container (px, %, auto) |
| `PopupHeight` | `string` | `300px` | Height of popup container (px, auto) |

### Responsive Popup

```razor
<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee"
    PopupWidth="calc(100% - 20px)"
    PopupHeight="auto">
    <ChildContent>
        <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
    </ChildContent>
</SfDropDownTree>
```

## Z-Index Control

Set popup layer depth using PopupSettings:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee">
    <ChildContent>
        <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
    </ChildContent>
    <DropDownTreePopupSettings ZIndex="9999"></DropDownTreePopupSettings>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan" }
    };

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
        public string? PId { get; set; }
    }
}
```

**Z-Index Usage:**
- Default ZIndex: 1000
- Use higher values for overlays over modals
- Use lower values for components that shouldn't overlap other elements

## Theme Support

The Dropdown Tree component supports multiple Syncfusion themes:

### Bootstrap Theme

```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

### Material Theme

```html
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />
```

### Tailwind CSS Theme

```html
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />
```

### Fluent Theme

```html
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />
```

## Complete Styling Example

```razor
@using Syncfusion.Blazor.Navigations

<div class="container">
    <h3>Dropdown Tree Styling</h3>
    
    <div class="form-group">
        <label>Success State:</label>
        <SfDropDownTree TItem="EmployeeData" TValue="string" 
            Placeholder="Valid selection"
            CssClass="e-success"
            PopupWidth="100%"
            PopupHeight="250px">
            <ChildContent>
                <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
            </ChildContent>
        </SfDropDownTree>
    </div>
    
    <div class="form-group">
        <label>Warning State:</label>
        <SfDropDownTree TItem="EmployeeData" TValue="string" 
            Placeholder="Verify before selecting"
            CssClass="e-warning">
            <ChildContent>
                <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
            </ChildContent>
        </SfDropDownTree>
    </div>
    
    <div class="form-group">
        <label>Error State:</label>
        <SfDropDownTree TItem="EmployeeData" TValue="string" 
            Placeholder="Required field"
            CssClass="e-error">
            <ChildContent>
                <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
            </ChildContent>
        </SfDropDownTree>
    </div>
    
    <div class="form-group">
        <label>Disabled State:</label>
        <SfDropDownTree TItem="EmployeeData" TValue="string" 
            Placeholder="Disabled"
            Enabled="false">
            <ChildContent>
                <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
            </ChildContent>
        </SfDropDownTree>
    </div>
</div>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", HasChild = true },
        new EmployeeData() { Id = "3", PId = "2", Name = "Andrew Fuller" },
        new EmployeeData() { Id = "4", PId = "1", Name = "Nancy Davolio" }
    };

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
        public bool Expanded { get; set; }
        public string? PId { get; set; }
    }
}

<style>
    .container {
        max-width: 600px;
        margin: 20px auto;
    }

    .form-group {
        margin-bottom: 20px;
    }

    .form-group label {
        display: block;
        margin-bottom: 8px;
        font-weight: 600;
        color: #333;
    }

    .form-group ::deep .e-ddtree-wrapper {
        width: 100%;
        margin-bottom: 10px;
    }
</style>
```

## Styling Properties Reference

| Property | Type | Purpose |
|----------|------|---------|
| `CssClass` | `string` | Additional CSS classes (e-success, e-warning, e-error, e-outline) |
| `Enabled` | `bool` | Enable/disable the component |
| `PopupWidth` | `string` | Width of popup (px, %, auto) |
| `PopupHeight` | `string` | Height of popup (px, auto) |
| `Width` | `string` | Width of the input element |
| `Height` | `string` | Height of the input element |

## Custom CSS Integration

```css
/* Override Syncfusion styles */
.e-ddtree .e-list-item {
    padding: 12px !important;
}

.e-ddtree .e-list-item:hover {
    background-color: #f0f0f0 !important;
}

.e-ddtree .e-list-item.e-active {
    background-color: #667eea !important;
}

.e-ddtree-wrapper input.e-field {
    border-radius: 4px !important;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1) !important;
}
```
