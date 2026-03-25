# Form Items and Field Configuration

## Table of Contents

- [Overview](#overview)
- [FormItem Element Basics](#formitem-element-basics)
- [Label Configuration](#label-configuration)
- [Placeholder Text](#placeholder-text)
- [Editor Type Selection](#editor-type-selection)
- [Disabling Fields](#disabling-fields)
- [Custom Attributes](#custom-attributes)
- [Form Grouping](#form-grouping)
- [Column Organization](#column-organization)

## Overview

FormItem elements allow you to explicitly define individual form fields with custom configuration. Use FormItem when you need precise control over field behavior, appearance, labels, and editor types.

## FormItem Element Basics

The `<FormItem>` element represents a single form field with configurable properties:

```razor
<SfDataForm Model="@employee">
    <FormItems>
        <FormItem Field="@nameof(employee.FirstName)" LabelText="First Name"></FormItem>
        <FormItem Field="@nameof(employee.Email)" LabelText="Email Address"></FormItem>
        <FormItem Field="@nameof(employee.Age)" LabelText="Age"></FormItem>
    </FormItems>
</SfDataForm>

@code {
    public class Employee
    {
        public string FirstName { get; set; }
        public string Email { get; set; }
        public int Age { get; set; }
    }

    private Employee employee = new();
}
```

### Key FormItem Properties

| Property | Type | Purpose |
|----------|------|---------|
| `Field` | string | Model property name to bind to (e.g., `nameof(model.PropertyName)`) |
| `LabelText` | string | Display label for the field |
| `EditorType` | EditorType | Type of editor control (TextBox, DatePicker, etc.) |
| `Placeholder` | string | Placeholder text when field is empty |
| `IsEnabled` | bool | Whether field is enabled (default: true) |

## Label Configuration

### Setting Custom Labels

```razor
<FormItem Field="@nameof(employee.EmployeeId)" 
          LabelText="Employee ID">
</FormItem>

<FormItem Field="@nameof(employee.EmailAddress)" 
          LabelText="Email Address">
</FormItem>
```

### Using Display Attribute

If your model has `[Display]` attribute, the DataForm uses that automatically:

```csharp
public class Employee
{
    [Display(Name = "Full Name")]
    public string Name { get; set; }

    [Display(Name = "Work Email")]
    public string Email { get; set; }
}
```

Then simply use FormItem without LabelText:

```razor
<FormItem Field="@nameof(employee.Name)"></FormItem>
<FormItem Field="@nameof(employee.Email)"></FormItem>
```

### Removing Labels

```razor
<FormItem Field="@nameof(employee.AgreeToTerms)" 
          LabelText="">
</FormItem>
```

## Placeholder Text

Placeholder appears inside the input field when empty:

```razor
<FormItem Field="@nameof(user.Email)" 
          LabelText="Email"
          Placeholder="Enter your email address">
</FormItem>

<FormItem Field="@nameof(user.PhoneNumber)" 
          LabelText="Phone"
          Placeholder="(123) 456-7890">
</FormItem>
```

## Editor Type Selection

The `EditorType` property determines which control renders for a field. Common editor types:

```csharp
public enum FormEditorType
{
    TextBox,           // Single-line text input
    TextArea,          // Multi-line text input
    DatePicker,        // Date selection
    DateTimePicker,    // Date and time selection
    TimePicker,        // Time selection
    DropDownList,      // Dropdown selection
    ComboBox,          // Editable dropdown
    AutoComplete,      // Auto-complete with suggestions
    Checkbox,          // Boolean checkbox (note: lowercase 'b')
    Switch,            // Toggle switch
    Password           // Password input with masking
}
```

### Examples

```razor
<SfDataForm Model="@order">
    <FormItems>
        <!-- Text input -->
        <FormItem Field="@nameof(order.ProductName)" 
                  LabelText="Product Name"
                  EditorType="FormEditorType.TextBox">
        </FormItem>

        <!-- Multi-line text -->
        <FormItem Field="@nameof(order.Description)" 
                  LabelText="Description"
                  EditorType="FormEditorType.TextArea">
        </FormItem>

        <!-- Date picker -->
        <FormItem Field="@nameof(order.OrderDate)" 
                  LabelText="Order Date"
                  EditorType="FormEditorType.DatePicker">
        </FormItem>

        <!-- Number input (uses TextBox with numeric model type) -->
        <FormItem Field="@nameof(order.Quantity)" 
                  LabelText="Quantity"
                  >
        </FormItem>

        <!-- Dropdown -->
        <FormItem Field="@nameof(order.Status)" 
                  LabelText="Status"
                  EditorType="FormEditorType.DropDownList">
        </FormItem>

        <!-- Checkbox (note lowercase 'b') -->
        <FormItem Field="@nameof(order.IsUrgent)" 
                  LabelText="Urgent Order"
                  EditorType="FormEditorType.Checkbox">
        </FormItem>
    </FormItems>
</SfDataForm>

@code {
    public class Order
    {
        public string ProductName { get; set; }
        public string Description { get; set; }
        public DateTime OrderDate { get; set; }
        public int Quantity { get; set; }
        public string Status { get; set; }
        public bool IsUrgent { get; set; }
    }

    private Order order = new();
}
```

## Disabling Fields

### Disable a Field

Use the `IsEnabled` property to disable field input while keeping it visible:

```razor
<FormItem Field="@nameof(employee.EmployeeId)" 
          LabelText="Employee ID"
          IsEnabled="false">
</FormItem>

<!-- Or conditionally -->
<FormItem Field="@nameof(employee.ApprovalStatus)" 
          LabelText="Status"
          IsEnabled="@(employee.Status == 'Draft')">
</FormItem>
```

## Custom Attributes

### Required Fields

Use data annotations to mark fields as required:

```csharp
public class Employee
{
    [Required(ErrorMessage = "Employee Name is required")]
    public string Name { get; set; }

    [Required]
    public string Department { get; set; }
}
```

The DataForm automatically validates these when submitted.

### Pattern Validation

```csharp
public class User
{
    [RegularExpression(@"^\d{3}-\d{3}-\d{4}$", 
        ErrorMessage = "Phone must be in format: 123-456-7890")]
    public string PhoneNumber { get; set; }

    [RegularExpression(@"^[a-zA-Z0-9_]+$", 
        ErrorMessage = "Username can only contain letters, numbers, and underscores")]
    public string Username { get; set; }
}
```

### Range Validation

```csharp
public class Product
{
    [Range(0, 1000, ErrorMessage = "Price must be between 0 and 1000")]
    public decimal Price { get; set; }

    [Range(1, int.MaxValue, ErrorMessage = "Quantity must be at least 1")]
    public int Quantity { get; set; }
}
```

### String Length

```csharp
public class Article
{
    [StringLength(200, MinimumLength = 10, 
        ErrorMessage = "Title must be between 10 and 200 characters")]
    public string Title { get; set; }

    [StringLength(5000, MinimumLength = 100, 
        ErrorMessage = "Content must be between 100 and 5000 characters")]
    public string Content { get; set; }
}
```

## Form Grouping

Organize fields into logical groups using `<FormGroup>`:

```razor
<SfDataForm Model="@employee">
    <FormItems>
        <FormGroup LabelText="Personal Information">
            <FormItem Field="@nameof(employee.FirstName)" LabelText="First Name"></FormItem>
            <FormItem Field="@nameof(employee.LastName)" LabelText="Last Name"></FormItem>
            <FormItem Field="@nameof(employee.DateOfBirth)" LabelText="Date of Birth"></FormItem>
        </FormGroup>

        <FormGroup LabelText="Contact Information">
            <FormItem Field="@nameof(employee.Email)" LabelText="Email"></FormItem>
            <FormItem Field="@nameof(employee.PhoneNumber)" LabelText="Phone"></FormItem>
        </FormGroup>

        <FormGroup LabelText="Employment Details">
            <FormItem Field="@nameof(employee.EmployeeId)" LabelText="Employee ID"></FormItem>
            <FormItem Field="@nameof(employee.Department)" LabelText="Department"></FormItem>
            <FormItem Field="@nameof(employee.Salary)" LabelText="Salary"></FormItem>
        </FormGroup>
    </FormItems>
</SfDataForm>

@code {
    public class Employee
    {
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime DateOfBirth { get; set; }
        public string Email { get; set; }
        public string PhoneNumber { get; set; }
        public string EmployeeId { get; set; }
        public string Department { get; set; }
        public decimal Salary { get; set; }
    }

    private Employee employee = new();
}
```

## Column Organization

### Multi-Column Layout

Set `ColumnCount` to arrange fields across multiple columns:

```razor
<SfDataForm Model="@employee" ColumnCount="2">
    <FormItems>
        <!-- These will arrange in 2 columns by default -->
        <FormItem Field="@nameof(employee.FirstName)" LabelText="First Name"></FormItem>
        <FormItem Field="@nameof(employee.LastName)" LabelText="Last Name"></FormItem>
        <FormItem Field="@nameof(employee.Email)" LabelText="Email"></FormItem>
        <FormItem Field="@nameof(employee.PhoneNumber)" LabelText="Phone"></FormItem>
    </FormItems>
</SfDataForm>
```

### Column Span

Control how many columns a field spans:

```razor
<SfDataForm Model="@employee" ColumnCount="2">
    <FormItems>
        <FormItem Field="@nameof(employee.FirstName)" 
                  LabelText="First Name"
                  ColumnSpan="1">
        </FormItem>
        <FormItem Field="@nameof(employee.LastName)" 
                  LabelText="Last Name"
                  ColumnSpan="1">
        </FormItem>
        
        <!-- Spans both columns -->
        <FormItem Field="@nameof(employee.FullAddress)" 
                  LabelText="Full Address"
                  ColumnSpan="2">
        </FormItem>

        <FormItem Field="@nameof(employee.City)" 
                  LabelText="City"
                  ColumnSpan="1">
        </FormItem>
        <FormItem Field="@nameof(employee.State)" 
                  LabelText="State"
                  ColumnSpan="1">
        </FormItem>
    </FormItems>
</SfDataForm>
```

### Responsive Column Layout

```razor
<!-- Desktop: 3 columns, Tablet: 2 columns, Mobile: 1 column -->
<SfDataForm Model="@employee" ColumnCount="3">
    <FormItems>
        <!-- Form items automatically wrap based on screen size -->
        <FormItem Field="@nameof(employee.FirstName)" LabelText="First Name"></FormItem>
        <FormItem Field="@nameof(employee.LastName)" LabelText="Last Name"></FormItem>
        <FormItem Field="@nameof(employee.Email)" LabelText="Email"></FormItem>
    </FormItems>
</SfDataForm>
```

## See Also

- [Auto-Generation](autogeneration.md) - Automatically generate fields from model
- [Validation](validation.md) - Add validation rules to form items
- [Layout Customization](layout-customization.md) - Customize form appearance
