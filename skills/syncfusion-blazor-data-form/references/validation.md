# Form Validation in Blazor DataForm

## Table of Contents

- [Overview](#overview)
- [Data Annotation Validation](#data-annotation-validation)
- [Built-in Validation Attributes](#built-in-validation-attributes)
- [DataAnnotationsValidator Setup](#dataannotationsvalidator-setup)
- [Custom Validation Rules](#custom-validation-rules)
- [Complex Model Validation](#complex-model-validation)
- [Fluent Validation Integration](#fluent-validation-integration)
- [Displaying Validation Messages](#displaying-validation-messages)
- [IsValid Method](#isvalid-method)
- [Conditional Validation](#conditional-validation)

## Overview

Form validation ensures data integrity and provides user feedback. The DataForm supports:
- **Data Annotations** - Built-in validation attributes
- **Custom Validators** - Business logic validation
- **Fluent Validation** - Fluent API for complex rules
- **Server-side Validation** - Backend validation integration

## Data Annotation Validation

Data annotations are declarative validation attributes applied to model properties:

```csharp
using System.ComponentModel.DataAnnotations;

public class Employee
{
    [Required(ErrorMessage = "First Name is required")]
    public string FirstName { get; set; }

    [Required(ErrorMessage = "Last Name is required")]
    public string LastName { get; set; }

    [Required(ErrorMessage = "Email is required")]
    [EmailAddress(ErrorMessage = "Invalid email format")]
    public string Email { get; set; }

    [Range(18, 65, ErrorMessage = "Age must be between 18 and 65")]
    public int Age { get; set; }
}
```

## Built-in Validation Attributes

### Required

Ensures a field has a value:

```csharp
public class User
{
    [Required]
    public string Username { get; set; }

    [Required(ErrorMessage = "Password is mandatory")]
    public string Password { get; set; }

    [Required(AllowEmptyStrings = false)]
    public string Email { get; set; }
}
```

### StringLength

Validates string length:

```csharp
public class Article
{
    [StringLength(200, MinimumLength = 10, 
        ErrorMessage = "Title must be 10-200 characters")]
    public string Title { get; set; }

    [StringLength(5000)]
    public string Content { get; set; }

    [StringLength(100)]
    public string Author { get; set; }
}
```

### Range

Validates numeric values within a range:

```csharp
public class Product
{
    [Range(0.01, 10000, ErrorMessage = "Price must be between 0.01 and 10000")]
    public decimal Price { get; set; }

    [Range(1, int.MaxValue, ErrorMessage = "Quantity must be at least 1")]
    public int Quantity { get; set; }

    [Range(0, 100, ErrorMessage = "Discount must be 0-100%")]
    public decimal DiscountPercent { get; set; }
}
```

### EmailAddress

Validates email format:

```csharp
public class Contact
{
    [EmailAddress(ErrorMessage = "Invalid email format")]
    public string Email { get; set; }

    [EmailAddress]
    public string BackupEmail { get; set; }
}
```

### RegularExpression

Validates against regex pattern:

```csharp
public class User
{
    [RegularExpression(@"^\d{3}-\d{3}-\d{4}$", 
        ErrorMessage = "Phone must be: 123-456-7890")]
    public string PhoneNumber { get; set; }

    [RegularExpression(@"^[a-zA-Z0-9_]{3,20}$", 
        ErrorMessage = "Username: 3-20 chars, letters/numbers/_")]
    public string Username { get; set; }

    [RegularExpression(@"^(?=.*[A-Z])(?=.*\d).{8,}$", 
        ErrorMessage = "Password: 8+ chars, uppercase, numbers")]
    public string Password { get; set; }
}
```

### Compare

Compares two properties:

```csharp
public class PasswordReset
{
    [Required]
    [StringLength(100, MinimumLength = 8)]
    public string NewPassword { get; set; }

    [Compare("NewPassword", ErrorMessage = "Passwords must match")]
    public string ConfirmPassword { get; set; }
}
```

### URL

Validates URL format:

```csharp
public class Website
{
    [Url(ErrorMessage = "Invalid URL format")]
    public string Url { get; set; }

    [Url]
    public string Logo { get; set; }
}
```

### MinLength / MaxLength

Validates collection length:

```csharp
public class Survey
{
    [MinLength(2, ErrorMessage = "Select at least 2 options")]
    public string[] Options { get; set; }

    [MaxLength(5, ErrorMessage = "Maximum 5 files allowed")]
    public string[] Files { get; set; }
}
```

### Custom Display and Error Messages

```csharp
public class Employee
{
    [Required]
    [Display(Name = "First Name", Description = "Enter your first name")]
    public string FirstName { get; set; }

    [Range(18, 65)]
    [Display(Name = "Age", Description = "Your age in years")]
    public int Age { get; set; }

    [EmailAddress(ErrorMessage = "Please enter a valid email address")]
    [Display(Name = "Work Email")]
    public string Email { get; set; }
}
```

## DataAnnotationsValidator Setup

Add validation to your DataForm:

```razor
<SfDataForm ID="EmployeeForm" Model="@employee">
    <!-- Add this FormValidator block -->
    <FormValidator>
        <DataAnnotationsValidator></DataAnnotationsValidator>
    </FormValidator>
    
    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
</SfDataForm>

@code {
    public class Employee
    {
        [Required]
        public string FirstName { get; set; }

        [Required]
        [EmailAddress]
        public string Email { get; set; }

        [Range(18, 65)]
        public int Age { get; set; }
    }

    private Employee employee = new();
}
```

The DataForm automatically validates on submit using the data annotation rules.

## Custom Validation Rules

Create custom validators for business logic:

```csharp
[AttributeUsage(AttributeTargets.Property)]
public class NotBlacklistedEmailAttribute : ValidationAttribute
{
    private static readonly List<string> BlacklistedDomains = new()
    {
        "spam.com",
        "fake.com",
        "test.com"
    };

    protected override ValidationResult IsValid(object value, ValidationContext context)
    {
        if (value is null)
            return ValidationResult.Success;

        var email = value.ToString();
        var domain = email.Split('@')[1];

        if (BlacklistedDomains.Contains(domain))
        {
            return new ValidationResult("Email domain is not allowed");
        }

        return ValidationResult.Success;
    }
}
```

Apply custom validator:

```csharp
public class User
{
    [Required]
    [EmailAddress]
    [NotBlacklistedEmail]
    public string Email { get; set; }
}
```

### Business Logic Validation

```csharp
[AttributeUsage(AttributeTargets.Property)]
public class FutureeDateAttribute : ValidationAttribute
{
    protected override ValidationResult IsValid(object value, ValidationContext context)
    {
        if (value is DateTime dateValue)
        {
            if (dateValue <= DateTime.Now)
            {
                return new ValidationResult("Date must be in the future");
            }
        }

        return ValidationResult.Success;
    }
}
```

Usage:

```csharp
public class Event
{
    [Required]
    public string Name { get; set; }

    [Required]
    [FutureDate(ErrorMessage = "Event date must be in the future")]
    public DateTime EventDate { get; set; }
}
```

## Complex Model Validation

### Nested Object Validation

```csharp
public class Employee
{
    [Required]
    public string Name { get; set; }

    [ValidateComplexType]
    public Address Address { get; set; }
}

public class Address
{
    [Required]
    [StringLength(100)]
    public string Street { get; set; }

    [Required]
    [StringLength(50)]
    public string City { get; set; }

    [RegularExpression(@"^\d{5}(-\d{4})?$", 
        ErrorMessage = "Invalid zip code")]
    public string ZipCode { get; set; }
}
```

### Conditional Validation

```csharp
[AttributeUsage(AttributeTargets.Class)]
public class ConditionalRequiredAttribute : ValidationAttribute
{
    private readonly string _dependentProperty;
    private readonly object _targetValue;

    public ConditionalRequiredAttribute(string dependentProperty, object targetValue)
    {
        _dependentProperty = dependentProperty;
        _targetValue = targetValue;
    }

    protected override ValidationResult IsValid(object value, ValidationContext context)
    {
        var property = context.ObjectType.GetProperty(_dependentProperty);
        var dependentValue = property?.GetValue(context.ObjectInstance);

        if (dependentValue?.Equals(_targetValue) == true && value == null)
        {
            return new ValidationResult($"This field is required when {_dependentProperty} is {_targetValue}");
        }

        return ValidationResult.Success;
    }
}
```

Apply to model:

```csharp
[ConditionalRequired(nameof(Order.Expedite), true, ErrorMessage = "Tracking Number required for expedited orders")]
public class Order
{
    public string OrderNumber { get; set; }

    [Display(Name = "Expedite Shipping")]
    public bool Expedite { get; set; }

    [Display(Name = "Tracking Number")]
    public string TrackingNumber { get; set; }
}
```

## Fluent Validation Integration

For complex validation scenarios, use Fluent Validation library:

### Install NuGet Package

```bash
dotnet add package FluentValidation
```

### Create Validator Class

```csharp
using FluentValidation;

public class EmployeeValidator : AbstractValidator<Employee>
{
    public EmployeeValidator()
    {
        RuleFor(x => x.FirstName)
            .NotEmpty().WithMessage("First Name is required")
            .Length(2, 50).WithMessage("First Name must be 2-50 characters");

        RuleFor(x => x.Email)
            .NotEmpty().WithMessage("Email is required")
            .EmailAddress().WithMessage("Invalid email format")
            .Must(x => !x.Contains("test")).WithMessage("Test emails not allowed");

        RuleFor(x => x.Age)
            .InclusiveBetween(18, 65).WithMessage("Age must be 18-65");

        RuleFor(x => x.Salary)
            .GreaterThan(0).WithMessage("Salary must be greater than 0")
            .LessThan(999999).WithMessage("Salary cannot exceed 999,999");

        RuleFor(x => x.Department)
            .Must(x => new[] { "HR", "IT", "Sales" }.Contains(x))
            .WithMessage("Invalid department selected");
    }
}

public class Employee
{
    public string FirstName { get; set; }
    public string Email { get; set; }
    public int Age { get; set; }
    public decimal Salary { get; set; }
    public string Department { get; set; }
}
```

### Use in DataForm

```razor
@inject IValidator<Employee> EmployeeValidator

<SfDataForm ID="EmployeeForm" 
            Model="@employee" 
            OnValidSubmit="HandleValidSubmit">
    
    <FormValidator>
        <!-- Use custom validator instead of DataAnnotationsValidator -->
        <FluentValidationValidator></FluentValidationValidator>
    </FormValidator>
    
    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
</SfDataForm>

@code {
    private Employee employee = new();

    private async Task HandleValidSubmit(EditContext context)
    {
        var validator = EmployeeValidator;
        var result = await validator.ValidateAsync(employee);

        if (!result.IsValid)
        {
            // Handle validation errors
            return;
        }

        // Save employee
        await SaveEmployeeAsync(employee);
    }

    private async Task SaveEmployeeAsync(Employee emp)
    {
        // Save to database
        await Task.Delay(500);
    }
}
```

## Displaying Validation Messages

### Automatic Message Display

Validation messages display automatically below invalid fields:

```razor
<SfDataForm Model="@user">
    <FormValidator>
        <DataAnnotationsValidator></DataAnnotationsValidator>
    </FormValidator>
    <FormItems>
        <FormItem Field="@nameof(user.Email)" 
                  LabelText="Email"
                  Placeholder="Enter email">
        </FormItem>
    </FormItems>
</SfDataForm>
```

### Validation Summary

Display all validation errors in one place:

```razor
<SfDataForm Model="@employee" OnInvalidSubmit="HandleInvalidSubmit">
    <FormValidator>
        <DataAnnotationsValidator></DataAnnotationsValidator>
        <ValidationSummary></ValidationSummary>
    </FormValidator>
    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
</SfDataForm>

@code {
    private Employee employee = new();

    private Task HandleInvalidSubmit(EditContext context)
    {
        // ValidationSummary automatically displays errors
        return Task.CompletedTask;
    }
}
```

### Custom Error Display

```razor
<SfDataForm Model="@form" OnInvalidSubmit="OnInvalidSubmit">
    <FormValidator>
        <DataAnnotationsValidator></DataAnnotationsValidator>
    </FormValidator>

    @if (validationErrors.Any())
    {
        <div class="alert alert-danger">
            <h4>Please fix the following errors:</h4>
            <ul>
                @foreach (var error in validationErrors)
                {
                    <li>@error</li>
                }
            </ul>
        </div>
    }

    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
</SfDataForm>

@code {
    private List<string> validationErrors = new();
    private Form form = new();

    private Task OnInvalidSubmit(EditContext context)
    {
        validationErrors = context.GetValidationMessages()
            .GroupBy(x => x.Split(": ")[0])
            .Select(g => g.Key + ": " + string.Join(", ", g.Select(x => x.Split(": ")[1])))
            .ToList();

        return Task.CompletedTask;
    }

    public class Form
    {
        [Required]
        public string Name { get; set; }
    }
}
```

## IsValid Method

Check if form is valid without submitting:

```razor
<SfDataForm @ref="dataForm" Model="@employee">
    <FormValidator>
        <DataAnnotationsValidator></DataAnnotationsValidator>
    </FormValidator>
    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
</SfDataForm>

<button @onclick="ValidateForm" class="btn btn-primary">Validate</button>

@code {
    private SfDataForm dataForm;
    private Employee employee = new();

    private async Task ValidateForm()
    {
        if (await dataForm.IsValid())
        {
            Console.WriteLine("Form is valid");
        }
        else
        {
            Console.WriteLine("Form has validation errors");
        }
    }

    public class Employee
    {
        [Required]
        public string Name { get; set; }

        [EmailAddress]
        public string Email { get; set; }
    }
}
```

## Conditional Validation

Validate fields based on other field values:

```razor
<SfDataForm Model="@order" OnUpdate="HandleUpdate">
    <FormValidator>
        <DataAnnotationsValidator></DataAnnotationsValidator>
    </FormValidator>
    
    <FormItems>
        <FormItem Field="@nameof(order.OrderType)" 
                  LabelText="Order Type"
                  EditorType="FormEditorType.DropDownList">
        </FormItem>

        @if (order.OrderType == "Express")
        {
            <FormItem Field="@nameof(order.TrackingNumber)" 
                      LabelText="Tracking Number"
                      Placeholder="Required for express orders">
            </FormItem>
        }

        <FormItem Field="@nameof(order.Notes)" 
                  LabelText="Notes"
                  EditorType="FormEditorType.TextArea">
        </FormItem>
    </FormItems>
</SfDataForm>

@code {
    public class Order
    {
        [Required]
        public string OrderType { get; set; }

        [RequiredIf(nameof(OrderType), "Express", 
            ErrorMessage = "Tracking Number required for express orders")]
        public string TrackingNumber { get; set; }

        public string Notes { get; set; }
    }

    private Order order = new();

    private Task HandleUpdate(FormUpdateEventArgs args)
    {
        StateHasChanged();
        return Task.CompletedTask;
    }
}
```

## Best Practices

✅ **DO:**
- Use `[Required]` for mandatory fields
- Add meaningful error messages
- Combine multiple validation rules
- Validate on both client and server
- Use `[Display]` for user-friendly labels
- Test validation edge cases

❌ **DON'T:**
- Use vague error messages ("Error occurred")
- Skip server-side validation
- Validate sensitive data only on client
- Leave required fields without indicators
- Ignore edge cases

## See Also

- [Form Items](form-items.md) - Field configuration
- [Events](events.md) - Handle validation events
- [Data Binding](data-binding.md) - Model binding setup
