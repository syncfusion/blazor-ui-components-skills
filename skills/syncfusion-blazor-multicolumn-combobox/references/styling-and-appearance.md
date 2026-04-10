# Styling and Appearance

## Table of Contents
- [Theme Integration](#theme-integration)
- [CSS Classes](#css-classes)
- [Custom Styling](#custom-styling)
- [Color and Placeholder Styling](#color-and-placeholder-styling)
- [Text Wrapping and Layout](#text-wrapping-and-layout)
- [Component State Styling](#component-state-styling)
- [Examples](#examples)

---

## Theme Integration

### Theme Selection

Syncfusion provides multiple built-in themes:

```html
<!-- Bootstrap 5 (default) -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Material 3 -->
<link href="_content/Syncfusion.Blazor.Themes/material3.css" rel="stylesheet" />

<!-- Fluent 2 -->
<link href="_content/Syncfusion.Blazor.Themes/fluent2.css" rel="stylesheet" />

<!-- Tailwind -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />
```

Place in `_Host.cshtml` or `App.razor` head section.

### Theme-Specific Styling

```razor
<!-- Bootstrap 5 theme -->
<SfMultiColumnComboBox TValue="int" TItem="ProductData" DataSource="@Products">
    <MultiColumnComboboxColumns>
        <!-- Columns with Bootstrap styling applied -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

---

## CSS Classes

### Apply Custom CSS Class

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       CssClass="custom-combobox">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductID" Header="ID"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="ProductName" Header="Product"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>

<style>
    .custom-combobox {
        width: 100%;
        max-width: 400px;
    }

    .custom-combobox .e-input-group {
        border: 2px solid #007bff;
        border-radius: 8px;
    }

    .custom-combobox .e-input-group:focus-within {
        border-color: #0056b3;
        box-shadow: 0 0 0 0.2rem rgba(0, 86, 179, 0.25);
    }
</style>
```

### Default Syncfusion Classes

| Class | Purpose |
|-------|---------|
| `.e-multicolumn-combobox` | Main component wrapper |
| `.e-input-group` | Input field container |
| `.e-input` | Input field |
| `.e-popup` | Dropdown popup |
| `.e-popup-content` | Popup content area |
| `.e-grid` | Grid inside popup |
| `.e-grid-row` | Grid row |
| `.e-grid-cell` | Grid cell |

---

## Custom Styling

### Component Width

```razor
<div style="width: 300px;">
    <SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                           DataSource="@Products"
                           Width="100%">
        <MultiColumnComboboxColumns>
            <!-- Columns -->
        </MultiColumnComboboxColumns>
    </SfMultiColumnComboBox>
</div>
```

### Input Field Styling

```razomulticolumn-combobox .e-input {
        font-size: 16px;
        padding: 10px;
        border-radius: 4px;
    }

    .e-multicolumn-combobox .e-input::placeholder {
        color: #999;
        font-style: italic;
    }
</style>

<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       CssClass="styled-input"
                       Placeholder="Enter product name...">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumn        Placeholder="Enter product name...">
    <!-- Columns -->
</SfComboBox>
```

### Popup Styling

```razor
<style>
    .e-multicolumn-combobox .e-popup {
        border: 1px solid #ddd;
        box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        border-radius: 4px;
    }

    .e-multicolumn-combobox .e-grid-row {
        height: 40px;
    }

    .e-multicolumn-combobox .e-grid-cell {
        padding: 10px 15px;
    }

    .e-multicolumn-combobox .e-grid-row:hover {
        background-color: #f5f5f5;
    }
</style>
```

---

## Color and Placeholder Styling

### Placeholder Color

```razor
<style>
    .custom-placeholder .e-input::placeholder {
        color: #999 !important;
    }

    .custom-placeholder .e-input::-webkit-input-placeholder {
        color: #999 !important;
    }

    .custom-placeholder .e-input::-moz-placeholder {
        color: #999 !important;
        opacity: 1;
    }
</style>

<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       CssClass="custom-placeholder"
                       Placeholder="Select a product...">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

### Float Label Color

```razor
<style>
    .e-multicolumn-combobox .e-float-text {
        color: #007bff;
        font-size: 12px;
    }

    .e-multicolumn-combobox .e-input-group.e-focus .e-float-text {
        color: #0056b3;
    }
</style>

<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       FloatLabelType="FloatLabelType.Always"
                       Placeholder="Product Name">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

### Input Border Color

```razor
<style>
    .bordered-input .e-input-group {
        border: 2px solid #ddd;
    }

    .bordered-input .e-input-group:focus-within {
        border-color: #007bff;
    }

    .bordered-input .e-input-group.e-error {
        border-color: #dc3545;
    }
</sMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       CssClass="bordered-input">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

---

## Text Wrapping and Layout

### Text Wrapping

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       EnableTextWrap="true"
                       TextWrapElement="TextWrapElement.Both">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="Description" Header="Description" Width="300px"></MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

### Column Alignment

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" DataSource="@Products">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="ProductName" 
                                   Header="Product" 
                                   Width="200px" 
                                   TextAlign="TextAlign.Left">
        </MultiColumnComboboxColumn>
        
        <MultiColumnComboboxColumn Field="Price" 
                                   Header="Price" 
                                   Width="100px" 
                                   TextAlign="TextAlign.Right"
                                   Format="C2">
        </MultiColumnComboboxColumn>
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

### Grid Lines

```razor
<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       GridLines="GridLine.Both">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumn        GridLines="GridLine.Both">
    <!-- Columns -->
</SfComboBox>
```
abled State

```razor
<style>
    .e-multicolumn-combobox.e-disabled .e-input {
        background-color: #e9ecef;
        color: #6c757d;
        cursor: not-allowed;
    }
</style>

<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       Disabled="true">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

### ReadOnly State

```razor
<style>
    .e-multicolumn-combobox.e-readonly .e-input {
        background-color: #f8f9fa;
        cursor: default;
    }
</style>

<SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                       DataSource="@Products"
                       ReadOnly="true">
    <MultiColumnComboboxColumns>
        <!-- Columns -->
    </MultiColumnComboboxColumns>
</SfMultiColumnComboBox>
```

### Focus State

```razor
<style>
    .e-multicolumn-combobox .e-input-group.e-focus .e-input {
        border-color: #007bff;
        box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
        outline: none;
    }
</style>
```

---

## Examples

### Complete Styled MultiColumn ComboBox

```razor
@page "/styled-employee-selector"
@using Syncfusion.Blazor.MultiColumnComboBox

<h3>Styled Employee Selector</h3>

<style>
    .employee-selector {
        max-width: 500px;
    }

    .employee-selector .e-input-group {
        border: 2px solid #e0e0e0;
        border-radius: 6px;
        transition: all 0.3s ease;
    }

    .employee-selector .e-input-group:hover {
        border-color: #b0bec5;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    }

    .employee-selector .e-input-group.e-focus {
        border-color: #1976d2;
        box-shadow: 0 0 0 0.2rem rgba(25, 118, 210, 0.25);
    }

    .employee-selector .e-input {
        font-size: 14px;
        padding: 10px 12px;
    }

    .employee-selector .e-input::placeholder {
        color: #9e9e9e;
        font-style: italic;
    }

    .employee-selector .e-popup {
        border-radius: 6px;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    }

    .employee-selector .e-grid-row {
        height: 45px;
    }

    .employee-selector .e-grid-row:hover {
        background-color: #f5f5f5;
    }

    .employee-selector .e-grid-cell {
        padding: 8px 12px;
        border-bottom: 1px solid #f0f0f0;
    }

    .employee-selector .e-grid-header {
        background-color: #f5f5f5;
        font-weight: 600;
        color: #424242;
    }
</style>

<SfMultiColumnComboBox TValue="int" TItem="EmployeeData" 
                       DataSource="@Employees"
                       CssClass="employee-selector"
                       AllowFiltering="true"
                       FloatLabelType="FloatLabelType.Auto"
                       Placeholder="Search employee"
                       EnableAltRow="true"
                       GridLines="GridLine.Horizontal"
                       TextField="FirstName"
                       ValueField="EmployeeID"
                       @bind-Value="@SelectedEmployeeID">
    <MultiColumnComboboxColumns>
        <MultiColumnComboboxColumn Field="EmployeeID" Header="ID" Width="60px" TextAlign="TextAlign.Center"></MultiColumnComboboxColumn>
        <MultiColumnComboboxColumn Field="FirstName" Header="First Name" Width="120px"></MultiColumnComboboxColumn>
        <MultiC<ComboBoxColumn Field="FirstName" Header="First Name" Width="120px"></ComboBoxColumn>
    <ComboBoxColumn Field="LastName" Header="Last Name" Width="120px"></ComboBoxColumn>
    <ComboBoxColumn Field="Department" Header="Department" Width="140px"></ComboBoxColumn>
</SfComboBox>

<div style="margin-top: 20px;">
    @if (SelectedEmployeeID > 0)
    {
        var selected = Employees.FirstOrDefault(e => e.EmployeeID == SelectedEmployeeID);
        @if (selected != null)
        {
            <div class="alert alert-info">
                <strong>Selected:</strong> @selected.FirstName @selected.LastName (@selected.Department)
            </div>
        }
    }
</div>

@code {
    private List<EmployeeData> Employees = new();
    private int SelectedEmployeeID { get; set; }

    protected override void OnInitialized()
    {
        Employees = new()
        {
            new EmployeeData { EmployeeID = 1, FirstName = "John", LastName = "Doe", Department = "Engineering" },
            new EmployeeData { EmployeeID = 2, FirstName = "Jane", LastName = "Smith", Department = "Sales" },
            new EmployeeData { EmployeeID = 3, FirstName = "Bob", LastName = "Johnson", Department = "HR" },
            new EmployeeData { EmployeeID = 4, FirstName = "Alice", LastName = "Brown", Department = "Finance" }
        };
    }

    public class EmployeeData
    {
        public int EmployeeID { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public string Department { get; set; }
    }
}
```multicolumn-combobox .e-input-group {
        background-color: #2d2d2d;
        border-color: #404040;
        color: #ffffff;
   

### Dark Mode Example

```razor
<style>
    .dark-mode .e-multicolumn-combobox .e-input-group {
        background-color: #2d2d2d;
        border-color: #404040;
        color: #ffffff;
    }

    .dark-mode .e-multicolumn-combobox .e-input {
        background-color: #2d2d2d;
        color: #ffffff;
    }

    .dark-mode .e-multicolumn-combobox .e-popup {
        background-color: #2d2d2d;
        color: #ffffff;
    }

    .dark-mode .e-multicolumn-combobox .e-grid-row:hover {
        background-color: #404040;
    }

    .dark-mode .e-multicolumn-combobox .e-grid-cell {
        border-bottom-color: #3a3a3a;
    }
</style>

<div class="dark-mode">
    <SfMultiColumnComboBox TValue="int" TItem="ProductData" 
                           DataSource="@Products">
        <MultiColumnComboboxColumns>
            <!-- Columns -->
        </MultiColumnComboboxColumns>
    </SfMultiColumn

## Next Steps

- **Accessibility considerations:** See [references/accessibility-localization.md](accessibility-localization.md)
- **Form integration:** See [references/features-and-settings.md](features-and-settings.md)
- **Advanced patterns:** See [references/advanced-patterns.md](advanced-patterns.md)
