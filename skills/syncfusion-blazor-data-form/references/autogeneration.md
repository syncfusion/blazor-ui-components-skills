# Auto-Generation of Form Fields

Auto-generation automatically creates form fields based on your model properties without manually defining each FormItem. The DataForm maps property types to appropriate editor controls.

## FormAutoGenerateItems Overview

The `<FormAutoGenerateItems>` element automatically generates fields for all public properties in your model:

```razor
<SfDataForm Model="@employee">
    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
</SfDataForm>

@code {
    public class Employee
    {
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public string Email { get; set; }
        public DateTime DateOfBirth { get; set; }
        public int Age { get; set; }
        public bool IsActive { get; set; }
    }

    private Employee employee = new();
}
```

The form automatically generates:
- First Name → TextBox
- Last Name → TextBox
- Email → TextBox
- Date of Birth → DateTimePicker
- Age → NumericTextBox
- Is Active → Checkbox

## Type-to-Component Mapping

The DataForm maps C# types to specific editor components:

| C# Type | Default Editor | Notes |
|---------|---|---|
| `string` | TextBox | Single-line text input |
| `int` | NumericTextBox | Integer with step buttons |
| `long` | NumericTextBox | Long integer |
| `float` | NumericTextBox | Decimal number |
| `decimal` | NumericTextBox | Currency/precision numbers |
| `double` | NumericTextBox | Double precision |
| `bool` | Checkbox | Boolean toggle |
| `DateTime` | DateTimePicker | Date and time selection |
| `DateOnly` | DatePicker | Date selection only |
| `TimeOnly` | TimePicker | Time selection only |
| `enum` | DropDownList | Enumeration values |
| `Guid` | TextBox | Unique identifier |
| `byte[]` | (Not auto-generated) | Use explicit FormItem |

**Important:** Numeric types (int, long, float, decimal, double) use `NumericTextBox` editor with automatic numeric input handling based on the property type. There is no separate `NumericTextBox` editor type.

## Enumeration Type Mapping and Editor Types

The DataForm supports comprehensive enumeration type mapping with the `FormEditorType` enum. Each enumeration type automatically maps to a specific Syncfusion editor control:

### Available FormEditorType Options

| FormEditorType | Description | Use Case |
|---|---|---|
| `TextBox` | Basic single-line text input | General text entry, usernames, URLs |
| `TextArea` | Multi-line text editor | Long descriptions, comments, notes |
| `DropDownList` | Dropdown selection from list | Predefined choices, enumerations |
| `ComboBox` | Combo box with filtering | Searchable selection with typed input |
| `AutoComplete` | Auto-complete input with suggestions | Search with real-time filtering |
| `DatePicker` | Date selection only | Date input without time |
| `DateTimePicker` | Date and time selection | Full date-time input |
| `TimePicker` | Time selection only | Time input without date |
| `Checkbox` | Boolean toggle | Yes/no, true/false values |
| `Switch` | Toggle switch component | Modern boolean input |
| `Password` | Password input with masking | Secure password entry |

### Enumeration Type Auto-Detection

When your model contains enum properties, the DataForm automatically detects them and creates a **DropDownList** editor populated with the enum values:

```csharp
public enum OrderStatus
{
    Pending,
    Processing,
    Shipped,
    Delivered,
    Cancelled
}

public enum Priority
{
    Low,
    Medium,
    High,
    Urgent
}

public enum PaymentMethod
{
    CreditCard,
    DebitCard,
    PayPal,
    BankTransfer,
    Cash
}

public class Order
{
    public string OrderNumber { get; set; }
    public OrderStatus Status { get; set; }
    public Priority Priority { get; set; }
    public PaymentMethod PaymentMethod { get; set; }
    public decimal Amount { get; set; }
}
```

## Auto-Generated Fields Example

```razor
@page "/auto-form"
@using System.ComponentModel.DataAnnotations

<SfDataForm Model="@contact">
    <FormValidator>
        <DataAnnotationsValidator></DataAnnotationsValidator>
    </FormValidator>
    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
</SfDataForm>

@code {
    public class Contact
    {
        public string Name { get; set; }
        public string Email { get; set; }
        public string PhoneNumber { get; set; }
        public DateTime DateOfContact { get; set; }
        public int Priority { get; set; }
        public bool HasFollowUp { get; set; }
    }

    private Contact contact = new();
}
```

Generated fields:
- Name → TextBox
- Email → TextBox
- PhoneNumber → TextBox
- DateOfContact → DateTimePicker
- Priority → NumericTextBox
- HasFollowUp → Checkbox

## Using Display Attribute

The `[Display]` attribute customizes field labels and configuration:

```csharp
public class Employee
{
    [Display(Name = "First Name", 
             Description = "Enter employee's first name")]
    public string FirstName { get; set; }

    [Display(Name = "Email Address")]
    public string Email { get; set; }

    [Display(Name = "Date of Birth")]
    public DateTime DOB { get; set; }

    [Display(Name = "Is Active Employee")]
    public bool IsActive { get; set; }
}
```

When auto-generating, the DataForm uses the `[Display(Name = "...")]` value as the field label.

## Combining Auto-Generated and Custom Fields

Mix `FormAutoGenerateItems` with explicit `FormItem` elements. Explicitly defined fields are not auto-generated:

```razor
<SfDataForm Model="@employee">
    <FormItems>
        <!-- Explicit custom field -->
        <FormItem Field="@nameof(employee.FirstName)" 
                  LabelText="First Name (Custom)"
                  Placeholder="Enter first name">
        </FormItem>

        <!-- Auto-generate remaining fields -->
        <FormAutoGenerateItems></FormAutoGenerateItems>

        <!-- Another explicit field -->
        <FormItem Field="@nameof(employee.ConfirmPassword)" 
                  LabelText="Confirm Password"
                  EditorType="FormEditorType.TextBox">
        </FormItem>
    </FormItems>
</SfDataForm>

@code {
    public class Employee
    {
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public string Email { get; set; }
        public string Password { get; set; }
        public string ConfirmPassword { get; set; }
    }

    private Employee employee = new();
}
```

Result:
- FirstName → Custom TextBox with placeholder
- LastName → Auto-generated TextBox
- Email → Auto-generated TextBox
- Password → Auto-generated TextBox
- ConfirmPassword → Custom TextBox

## Excluding Specific Fields from Auto-Generation

Explicitly define fields you want to exclude from auto-generation:

```razor
<SfDataForm Model="@user">
    <FormItems>
        <!-- Define sensitive field explicitly with hidden editor -->
        <FormItem Field="@nameof(user.Password)" 
                  LabelText="Password"
                  EditorType="FormEditorType.TextBox">
        </FormItem>

        <!-- Auto-generate all other fields -->
        <FormAutoGenerateItems></FormAutoGenerateItems>

        <!-- Fields needing special handling -->
        <FormItem Field="@nameof(user.CountryCode)" 
                  LabelText="Country"
                  EditorType="FormEditorType.DropDownList">
        </FormItem>
    </FormItems>
</SfDataForm>

@code {
    public class User
    {
        public string Username { get; set; }
        public string Email { get; set; }
        public string Password { get; set; }
        public string PhoneNumber { get; set; }
        public string CountryCode { get; set; }
        public bool TermsAccepted { get; set; }
    }

    private User user = new();
}
```

Result: Password and CountryCode use custom editors, others auto-generated.

## Disabling Auto-Generation

### Completely Disable Auto-Generation

Omit `FormAutoGenerateItems` to only show explicitly defined fields:

```razor
<SfDataForm Model="@employee">
    <FormItems>
        <!-- Only these fields will render -->
        <FormItem Field="@nameof(employee.FirstName)" LabelText="First Name"></FormItem>
        <FormItem Field="@nameof(employee.Email)" LabelText="Email"></FormItem>
        <FormItem Field="@nameof(employee.Salary)" LabelText="Salary"></FormItem>
        <!-- Other properties are NOT rendered -->
    </FormItems>
</SfDataForm>

@code {
    public class Employee
    {
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public string Email { get; set; }
        public string Password { get; set; }
        public decimal Salary { get; set; }
        public string InternalNotes { get; set; }
    }

    private Employee employee = new();
}
```

Only FirstName, Email, and Salary display. LastName, Password, and InternalNotes are hidden.

### Conditionally Display Auto-Generated Fields

```razor
<SfDataForm Model="@employee">
    <FormItems>
        @if (editMode)
        {
            <FormAutoGenerateItems></FormAutoGenerateItems>
        }
        else
        {
            <!-- Show only specific fields in view mode -->
            <FormItem Field="@nameof(employee.FirstName)" 
                      LabelText="First Name"
                      IsEnabled="false">
            </FormItem>
            <FormItem Field="@nameof(employee.Email)" 
                      LabelText="Email"
                      IsEnabled="false">
            </FormItem>
        }
    </FormItems>
</SfDataForm>

@code {
    private bool editMode = true;
    private Employee employee = new();
}
```

## Auto-Generation with Validation

Auto-generated fields respect data annotation validation attributes:

```csharp
public class Registration
{
    [Required(ErrorMessage = "Name is required")]
    [StringLength(100)]
    public string Name { get; set; }

    [Required(ErrorMessage = "Email is required")]
    [EmailAddress(ErrorMessage = "Invalid email address")]
    public string Email { get; set; }

    [Range(18, 100, ErrorMessage = "Age must be 18-100")]
    public int Age { get; set; }

    [Range(0.01, double.MaxValue, ErrorMessage = "Price must be greater than 0")]
    public decimal Price { get; set; }

    [Required]
    public bool TermsAccepted { get; set; }
}
```

When auto-generating these fields, validation rules are automatically enforced:

```razor
<SfDataForm Model="@registration">
    <FormValidator>
        <DataAnnotationsValidator></DataAnnotationsValidator>
    </FormValidator>
    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
</SfDataForm>
```

## Custom Editor Type for Auto-Generated Fields

Override default editor type for specific properties using `[EditorType]` custom attribute or by explicitly defining that field:

```csharp
public class Settings
{
    public string Name { get; set; }
    
    // Override: Use TextArea instead of TextBox for long text
    public string Description { get; set; }
    
    public int Quantity { get; set; }
}
```

Override in markup:

```razor
<SfDataForm Model="@settings">
    <FormItems>
        <FormItem Field="@nameof(settings.Name)"></FormItem>
        
        <!-- Override auto-generated TextBox with TextArea -->
        <FormItem Field="@nameof(settings.Description)" 
                  LabelText="Description"
                  EditorType="FormEditorType.TextArea">
        </FormItem>
        
        <!-- Continue auto-generating others -->
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
</SfDataForm>

@code {
    public class Settings
    {
        public string Name { get; set; }
        public string Description { get; set; }
        public int Quantity { get; set; }
    }

    private Settings settings = new();
}
```

## Auto-Generation with Enums

Auto-generated fields for enum properties create DropDownList with enum values:

```csharp
public enum OrderStatus
{
    Pending,
    Processing,
    Shipped,
    Delivered,
    Cancelled
}

public enum Priority
{
    Low,
    Medium,
    High,
    Urgent
}

public class Order
{
    public string OrderNumber { get; set; }
    public OrderStatus Status { get; set; }
    public Priority Priority { get; set; }
    public DateTime CreatedDate { get; set; }
}
```

Auto-generated result:
- OrderNumber → TextBox
- Status → DropDownList (Pending, Processing, Shipped, Delivered, Cancelled)
- Priority → DropDownList (Low, Medium, High, Urgent)
- CreatedDate → DateTimePicker

```razor
<SfDataForm Model="@order">
    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
</SfDataForm>

@code {
    private Order order = new();
}
```

## Best Practices

✅ **DO:**
- Use auto-generation for straightforward models with standard types
- Combine with explicit FormItems for special fields
- Leverage `[Display]` attributes for better labels
- Use data annotations for validation rules
- Group related fields visually

❌ **DON'T:**
- Auto-generate sensitive fields like passwords (define explicitly)
- Mix auto-generated and custom fields without clear organization
- Rely on auto-generation for complex nested objects
- Forget to add `[Display]` attributes for user-friendly labels
- Auto-generate without validation attributes

## See Also

- [Form Items](form-items.md) - Explicit field configuration
- [Validation](validation.md) - Add validation to auto-generated fields
- [Data Binding](data-binding.md) - Model binding with auto-generation
