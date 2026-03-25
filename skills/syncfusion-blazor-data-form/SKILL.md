---
name: syncfusion-blazor-dataform
description: Implement Syncfusion Blazor DataForm component for creating dynamic, data-bound forms with validation and field management. Use this when building forms in Blazor with Syncfusion components, handling form validation, binding models, creating editable fields, or managing form events. This skill covers form layout customization, data binding, FormItems configuration, FormAutoGenerateItems setup, templates, events, and data annotation validation.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Blazor Form Components"
---

# Implementing Syncfusion Blazor DataForm Component

A comprehensive skill for implementing the Syncfusion Blazor DataForm component. This skill covers installation, configuration, data binding, validation, event handling, templating, and customization of forms in Blazor applications.

## Component Overview

The Syncfusion Blazor DataForm (SfDataForm) is a powerful form component that streamlines the creation of dynamic, data-driven forms with automatic validation, field binding, and event handling. It supports:

- **Automatic field generation** from model properties
- **Built-in validation** with data annotations and custom rules
- **Multiple binding approaches** (model, EditContext)
- **Customizable layouts** with columns, grouping, and sections
- **Template-based rendering** for custom field designs
- **Event-driven architecture** for form and field-level reactions
- **Localization support** for validation messages and UI text
- **Accessible form rendering** with proper labels and ARIA attributes

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

Start here for installation, NuGet package setup, namespace imports, service registration, theme configuration, and a basic DataForm example.

- Installation & NuGet packages
- Blazor project setup (Server/WebAssembly/Web App)
- Namespace imports & service registration
- Theme setup & configuration
- Basic DataForm creation
- Running your first form

### Form Items and Field Configuration

📄 **Read:** [references/form-items.md](references/form-items.md)

Learn how to define form items, configure labels, placeholders, hints, editor types, and customize individual field behavior.

- FormItem element configuration
- Label, placeholder configurations
- Editor type selection
- Disabling and hiding fields
- Custom attributes (required, pattern, data annotations)
- Form grouping and column organization

### Auto-Generation of Form Fields

📄 **Read:** [references/autogeneration.md](references/autogeneration.md)

Discover how to automatically generate form fields from model properties using FormAutoGenerateItems, including type mapping and combining auto-generated with custom fields.

- FormAutoGenerateItems overview
- Type-to-component mapping (int, string, DateTime, bool, enum, etc.)
- Auto-generating fields for primitive types
- Combining auto-generated and custom fields
- Canceling auto-generation for specific fields
- Configuration options

### Data Binding

📄 **Read:** [references/data-binding.md](references/data-binding.md)

Understand model binding, EditContext binding, two-way data binding, and binding to complex data structures.

- Model binding basics
- EditContext binding approach
- Form-level and field-level binding
- Two-way binding with nested objects
- Binding to complex models
- Data initialization and default values
- Form name and ID configuration

### Form Validation

📄 **Read:** [references/validation.md](references/validation.md)

Master form validation with data annotations, custom validation rules, Fluent Validation integration, and error message display.

- Data annotation validation (Required, Range, EmailAddress, etc.)
- DataAnnotationsValidator setup
- Custom validation attributes and rules
- Fluent Validation framework integration
- Complex model validation scenarios
- Displaying validation messages
- IsValid() method and validation state
- Conditional validation

### Form and Field Events

📄 **Read:** [references/events.md](references/events.md)

Learn about form submission events and field-level events for handling user interactions and form state changes.

- OnSubmit event (fires on every submit attempt)
- OnValidSubmit event (fires only on valid submission)
- OnInvalidSubmit event (fires only on validation failure)
- OnUpdate event (fires on field value changes)
- Event arguments and EditContext access
- Asynchronous event handlers
- Common event patterns

### Layout Customization

📄 **Read:** [references/layout-customization.md](references/layout-customization.md)

Customize form layout with columns, column spans, label positioning, floating labels, and custom button placement.

- Column layout configuration
- Column span and row span
- Columns count and responsiveness
- Label positioning (top, left, floating)
- Floating label styling
- Button alignment and positioning
- Custom submit/reset buttons
- Form grouping with FormGroup

### Templates and Custom Rendering

📄 **Read:** [references/templates.md](references/templates.md)

Create custom form layouts and field renderings using FormTemplate and FormItemTemplate with render fragments.

- FormTemplate for overall form layout customization
- FormItemTemplate for individual field rendering
- Template context and variables
- Custom editor rendering
- Render fragments and composition
- Advanced template patterns
- Template reusability

### Localization

📄 **Read:** [references/localization.md](references/localization.md)

Set up localization for validation messages, form labels, and error messages in multiple languages.

- Built-in localization resources
- Localizing error messages
- Custom localization setup
- Culture and language configuration
- RTL (right-to-left) support

## Quick Start Example

Here's a minimal working example to get started:

```razor
@page "/dataform-example"
@using System.ComponentModel.DataAnnotations
@using Syncfusion.Blazor.DataForm

<SfDataForm ID="MyDataForm"
            Model="@employeeModel">
    <FormValidator>
        <DataAnnotationsValidator></DataAnnotationsValidator>
    </FormValidator>
    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
</SfDataForm>

@code {
    public class EmployeeModel
    {
        [Required(ErrorMessage = "First Name is required")]
        [Display(Name = "First Name")]
        public string FirstName { get; set; }

        [Required(ErrorMessage = "Email is required")]
        [EmailAddress(ErrorMessage = "Invalid email address")]
        [Display(Name = "Email")]
        public string Email { get; set; }

        [Range(18, 65, ErrorMessage = "Age must be between 18 and 65")]
        [Display(Name = "Age")]
        public int Age { get; set; }

        [Display(Name = "Date of Birth")]
        public DateTime? DateOfBirth { get; set; }
    }

    private EmployeeModel employeeModel = new EmployeeModel();
}
```

## Common Patterns

### Pattern 1: Form with Validation and Submission

```razor
<SfDataForm ID="ContactForm" Model="@contact" OnValidSubmit="HandleValidSubmit">
    <FormValidator>
        <DataAnnotationsValidator></DataAnnotationsValidator>
    </FormValidator>
    <FormItems>
        <FormItem Field="@nameof(contact.Name)" LabelText="Full Name"></FormItem>
        <FormItem Field="@nameof(contact.Email)" LabelText="Email Address"></FormItem>
        <FormItem Field="@nameof(contact.Message)" EditorType="FormEditorType.TextArea"></FormItem>
    </FormItems>
</SfDataForm>

@code {
    private Contact contact = new();

    private async Task HandleValidSubmit(EditContext context)
    {
        // Save contact to database
        await SaveContactAsync(contact);
    }
}
```

### Pattern 2: Auto-Generated Form with Custom Fields

```razor
<SfDataForm Model="@product">
    <FormItems>
        <FormItem Field="@nameof(product.Name)" LabelText="Product Name"></FormItem>
        <FormAutoGenerateItems></FormAutoGenerateItems>
        <FormItem Field="@nameof(product.Category)" EditorType="FormEditorType.DropDownList"></FormItem>
    </FormItems>
</SfDataForm>

@code {
    private Product product = new();
}
```

### Pattern 3: Form with Cascading Fields

```razor
<SfDataForm Model="@order" OnUpdate="HandleFieldUpdate">
    <FormItems>
        <FormItem Field="@nameof(order.Country)" LabelText="Country"></FormItem>
        <FormItem Field="@nameof(order.City)" LabelText="City"></FormItem>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
</SfDataForm>

@code {
    private Order order = new();

    private async Task HandleFieldUpdate(FormUpdateEventArgs args)
    {
        if (args.FieldName == nameof(Order.Country))
        {
            // Update cities based on selected country
            order.City = await GetCitiesForCountryAsync(order.Country);
        }
    }
}
```

### Pattern 4: Custom Layout with Columns

```razor
<SfDataForm Model="@employee" ColumnCount="2">
    <FormItems>
        <FormItem Field="@nameof(employee.FirstName)" LabelText="First Name" ColumnSpan="1"></FormItem>
        <FormItem Field="@nameof(employee.LastName)" LabelText="Last Name" ColumnSpan="1"></FormItem>
        <FormItem Field="@nameof(employee.Email)" LabelText="Email" ColumnSpan="2"></FormItem>
        <FormItem Field="@nameof(employee.Department)" LabelText="Department" ColumnSpan="1"></FormItem>
        <FormItem Field="@nameof(employee.Salary)" LabelText="Salary" ColumnSpan="1"></FormItem>
    </FormItems>
</SfDataForm>

@code {
    private Employee employee = new();
}
```

## Key Properties and Methods

### Common Properties

| Property | Type | Purpose |
|----------|------|---------|
| `Model` | object | The data model bound to the form |
| `EditContext` | EditContext | Blazor EditContext for advanced binding scenarios |
| `ColumnCount` | int | Number of columns for form layout |
| `OnValidSubmit` | EventCallback | Fires when form is submitted with valid data |
| `OnInvalidSubmit` | EventCallback | Fires when form is submitted with invalid data |
| `OnSubmit` | EventCallback | Fires on every submit attempt |
| `OnUpdate` | EventCallback | Fires when a field value changes |

### Available FormEditorType Values

**Important**: In most cases, you don't need to specify `EditorType` as the DataForm automatically selects the appropriate editor based on your model property type. Use these values only when you need to override the default behavior.

| FormEditorType | Use Case | When to Use |
|----------------|----------|-------------|
| `TextBox` | Single-line text input | Override for string fields (default), not needed for numeric types |
| `TextArea` | Multi-line text input | When you need multi-line text entry for string properties |
| `DatePicker` | Date selection only | Override for DateTime (default for DateTime) |
| `DateTimePicker` | Date and time selection | When you need both date and time for DateTime properties |
| `TimePicker` | Time selection only | When you need only time selection for TimeSpan or DateTime |
| `DropDownList` | Dropdown selection from list | For enum or custom list selection |
| `ComboBox` | Editable dropdown with filtering | When you need searchable dropdown |
| `AutoComplete` | Auto-complete with suggestions | When you need auto-suggest functionality |
| `Checkbox` | Boolean toggle (note: lowercase 'b') | Override for bool (default), NOT `CheckBox` |
| `Switch` | Toggle switch for boolean | Alternative to checkbox for bool properties |
| `Password` | Password input with masking | For password string fields |

**Critical Notes:**
- ⚠️ **Automatic Type Mapping**: The DataForm intelligently maps C# types to appropriate editors automatically:
  - `int`, `long`, `decimal`, `double`, `float` → Numeric textbox (no EditorType needed)
  - `string` → Textbox (no EditorType needed)
  - `bool` → Checkbox (no EditorType needed)
  - `DateTime` → DatePicker (no EditorType needed)
  - `enum` → DropDownList (no EditorType needed)
- ⚠️ Use `Checkbox` (lowercase 'b'), NOT `CheckBox` (capital B)
- ⚠️ There is NO `NumericTextBox` editor type - numeric behavior is automatic based on property type
- ⚠️ Only specify `EditorType` when you want to override the default behavior

### Common Methods

| Method | Purpose |
|--------|---------|
| `Refresh()` | Refresh form and re-render fields |
| `Submit()` | Programmatically submit the form |
| `Reset()` | Clear form and reset to initial values |
| `IsValid()` | Check if form is valid |
| `Validate()` | Trigger validation without submitting |

## Important EditorType Guidelines

When configuring FormItem elements with `EditorType`, keep these critical points in mind:

1. **Automatic Editor Selection**: The DataForm automatically selects the appropriate editor based on your model property type. You typically don't need to specify `EditorType` unless you want to override the default behavior.
   - **Numeric properties** (int, long, float, decimal, double) → Automatically render as numeric textbox
   - **String properties** → Automatically render as textbox
   - **Boolean properties** → Automatically render as checkbox
   - **DateTime properties** → Automatically render as date picker
   - **Enum properties** → Automatically render as dropdown

2. **When to Specify EditorType**: Only specify `EditorType` when you want to override the default:
   - Use `FormEditorType.TextArea` for multi-line string input
   - Use `FormEditorType.Switch` instead of checkbox for boolean
   - Use `FormEditorType.DateTimePicker` instead of `DatePicker` for DateTime with time
   - Use `FormEditorType.Password` for password strings
   - Use `FormEditorType.DropDownList` or `ComboBox` for custom lists

3. **Checkbox Casing**: When explicitly specifying checkbox, use `FormEditorType.Checkbox` with lowercase 'b', not `FormEditorType.CheckBox`.

4. **No NumericTextBox Type**: There is no `FormEditorType.NumericTextBox`. Numeric behavior is automatic based on property type.

**Example:**
```razor
<!-- Numeric field - EditorType not needed, automatically renders numeric textbox -->
<FormItem Field="@nameof(model.Quantity)" 
          LabelText="Quantity">
</FormItem>

<!-- Boolean field - EditorType not needed, automatically renders checkbox -->
<FormItem Field="@nameof(model.IsActive)" 
          LabelText="Is Active">
</FormItem>

<!-- String field with TextArea override -->
<FormItem Field="@nameof(model.Description)" 
          LabelText="Description"
          EditorType="FormEditorType.TextArea">
</FormItem>

<!-- Boolean field with Switch override -->
<FormItem Field="@nameof(model.IsEnabled)" 
          LabelText="Enabled"
          EditorType="FormEditorType.Switch">
</FormItem>
```

## Related Skills

- [Syncfusion Blazor Components](../../../SKILL.md) - Main library skill for all Blazor components
- [AutoComplete](../../../dropdowns/implementing-autocomplete/) - For dropdown field editors
- [File Upload](../../../file-upload/implementing-file-upload/) - For file input fields
