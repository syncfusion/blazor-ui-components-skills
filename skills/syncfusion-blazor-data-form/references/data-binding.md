# Data Binding in Blazor DataForm

## Table of Contents

- [Binding Overview](#binding-overview)
- [Model Binding](#model-binding)
- [EditContext Binding](#editcontext-binding)
- [Two-Way Data Binding](#two-way-data-binding)
- [Nested Object Binding](#nested-object-binding)
- [Data Initialization](#data-initialization)
- [Form Name and ID](#form-name-and-id)

## Binding Overview

The DataForm component supports two primary binding approaches:
1. **Model Binding** - Direct model property binding (simplest)
2. **EditContext Binding** - Advanced scenario with custom validation and state management

## Model Binding

The simplest way to bind data to a DataForm is passing a model instance to the `Model` property:

```razor
<SfDataForm Model="@employee">
    <FormItems>
        <FormItem Field="@nameof(employee.FirstName)" LabelText="First Name"></FormItem>
        <FormItem Field="@nameof(employee.Email)" LabelText="Email"></FormItem>
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

### Key Properties

| Property | Type | Purpose |
|----------|------|---------|
| `Model` | object | The data model to bind |
| `Field` | string | Model property name (use `nameof()` for type safety) |
| `T` (Generic) | Type | Model type for compile-time validation |

### Pre-Populated Model

Initialize your model with default values:

```razor
@code {
    private Employee employee = new()
    {
        FirstName = "John",
        LastName = "Doe",
        Email = "john.doe@example.com",
        Age = 30
    };
}
```

The form displays with these initial values.

## EditContext Binding

For advanced scenarios, use Blazor's `EditContext` for custom validation and state management:

```razor
<EditForm EditContext="@editContext" OnSubmit="HandleSubmit">
    <SfDataForm EditContext="@editContext">
        <FormValidator>
            <DataAnnotationsValidator></DataAnnotationsValidator>
        </FormValidator>
        <FormItems>
            <FormAutoGenerateItems></FormAutoGenerateItems>
        </FormItems>
    </SfDataForm>
    <button type="submit" class="btn btn-primary">Submit</button>
</EditForm>

@code {
    public class Employee
    {
        [Required]
        public string Name { get; set; }

        [EmailAddress]
        public string Email { get; set; }
    }

    private Employee employee = new();
    private EditContext editContext;

    protected override void OnInitialized()
    {
        editContext = new EditContext(employee);
    }

    private async Task HandleSubmit()
    {
        if (editContext.Validate())
        {
            await SaveEmployeeAsync();
        }
    }

    private async Task SaveEmployeeAsync()
    {
        // Save to database
        Console.WriteLine($"Saved: {employee.Name}");
    }
}
```

### EditContext Advantages

- **Manual validation control** - Call `editContext.Validate()` explicitly
- **Custom validation rules** - Add custom validators
- **State tracking** - Monitor field changes
- **Nested form support** - Use within EditForm component

## Two-Way Data Binding

Changes in form fields automatically update the model properties and vice versa:

```razor
<div class="container">
    <SfDataForm Model="@person">
        <FormItems>
            <FormItem Field="@nameof(person.FirstName)" LabelText="First Name"></FormItem>
            <FormItem Field="@nameof(person.LastName)" LabelText="Last Name"></FormItem>
            <FormItem Field="@nameof(person.Email)" LabelText="Email"></FormItem>
        </FormItems>
    </SfDataForm>

    <div class="mt-3">
        <h4>Current Values:</h4>
        <p>Name: @person.FirstName @person.LastName</p>
        <p>Email: @person.Email</p>
    </div>
</div>

@code {
    public class Person
    {
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public string Email { get; set; }
    }

    private Person person = new();
}
```

As users type in the form fields, the `person` object updates in real-time, and the display section reflects changes immediately.

### Update Events

Track specific field changes using the `OnUpdate` event:

```razor
<SfDataForm Model="@order" OnUpdate="HandleFieldUpdate">
    <FormItems>
        <FormItem Field="@nameof(order.Quantity)" LabelText="Quantity"></FormItem>
        <FormItem Field="@nameof(order.UnitPrice)" LabelText="Unit Price"></FormItem>
        <FormItem Field="@nameof(order.TotalPrice)" LabelText="Total Price" IsEnabled="false"></FormItem>
    </FormItems>
</SfDataForm>

<p>Last Updated: @lastUpdatedField</p>

@code {
    public class Order
    {
        public int Quantity { get; set; }
        public decimal UnitPrice { get; set; }
        public decimal TotalPrice { get; set; }
    }

    private Order order = new();
    private string lastUpdatedField;

    private Task HandleFieldUpdate(FormUpdateEventArgs args)
    {
        lastUpdatedField = $"{args.FieldName} updated at {DateTime.Now:HH:mm:ss}";

        // Auto-calculate total when quantity or price changes
        if (args.FieldName == nameof(Order.Quantity) || 
            args.FieldName == nameof(Order.UnitPrice))
        {
            order.TotalPrice = order.Quantity * order.UnitPrice;
        }

        return Task.CompletedTask;
    }
}
```

## Nested Object Binding

Bind to properties of nested objects in your model:

```csharp
public class Employee
{
    public string Name { get; set; }
    public Address Address { get; set; }
}

public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public string State { get; set; }
    public string ZipCode { get; set; }
}
```

Use dot notation to bind nested properties:

```razor
<SfDataForm Model="@employee">
    <FormItems>
        <FormGroup LabelText="Personal Information">
            <FormItem Field="@nameof(employee.Name)" LabelText="Name"></FormItem>
        </FormGroup>

        <FormGroup LabelText="Address">
            <!-- Bind to nested Address properties -->
            <FormItem Field="@nameof(employee.Address.Street)" LabelText="Street"></FormItem>
            <FormItem Field="@nameof(employee.Address.City)" LabelText="City"></FormItem>
            <FormItem Field="@nameof(employee.Address.State)" LabelText="State"></FormItem>
            <FormItem Field="@nameof(employee.Address.ZipCode)" LabelText="Zip Code"></FormItem>
        </FormGroup>
    </FormItems>
</SfDataForm>

@code {
    private Employee employee = new()
    {
        Name = "John Doe",
        Address = new Address
        {
            Street = "123 Main St",
            City = "New York",
            State = "NY",
            ZipCode = "10001"
        }
    };
}
```

### Complex Nested Objects

Handle deeply nested structures:

```csharp
public class Company
{
    public string Name { get; set; }
    public Employee CEO { get; set; }
}

public class Employee
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public Contact Contact { get; set; }
}

public class Contact
{
    public string Email { get; set; }
    public string Phone { get; set; }
}
```

Bind at any depth:

```razor
<SfDataForm Model="@company">
    <FormItems>
        <FormItem Field="@nameof(company.Name)" LabelText="Company Name"></FormItem>
        <FormItem Field="@nameof(company.CEO.FirstName)" LabelText="CEO First Name"></FormItem>
        <FormItem Field="@nameof(company.CEO.Contact.Email)" LabelText="CEO Email"></FormItem>
        <FormItem Field="@nameof(company.CEO.Contact.Phone)" LabelText="CEO Phone"></FormItem>
    </FormItems>
</SfDataForm>

@code {
    private Company company = new()
    {
        Name = "Tech Corp",
        CEO = new Employee
        {
            FirstName = "Jane",
            LastName = "Smith",
            Contact = new Contact
            {
                Email = "jane@techcorp.com",
                Phone = "+1-555-0100"
            }
        }
    };
}
```

⚠️ **Important:** Ensure nested objects are initialized before binding.

## Data Initialization

### Initialize with Empty Values

```csharp
private Employee employee = new();
```

### Initialize with Default Values

```csharp
private Employee employee = new()
{
    IsActive = true,
    StartDate = DateTime.Today,
    Department = "Engineering"
};
```

### Load from Database

```razor
@code {
    private Employee employee;

    protected override async Task OnInitializedAsync()
    {
        employee = await LoadEmployeeAsync(employeeId);
    }

    private async Task<Employee> LoadEmployeeAsync(int id)
    {
        // Fetch from API or database
        var response = await httpClient.GetAsync($"/api/employees/{id}");
        return await response.Content.ReadAsAsync<Employee>();
    }
}
```

### Load from API

```razor
@inject HttpClient HttpClient

@code {
    private Product product;
    private string errorMessage;

    protected override async Task OnInitializedAsync()
    {
        try
        {
            product = await HttpClient.GetFromJsonAsync<Product>("/api/products/1");
        }
        catch (Exception ex)
        {
            errorMessage = $"Failed to load product: {ex.Message}";
        }
    }

    public class Product
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public decimal Price { get; set; }
        public string Description { get; set; }
    }
}
```

## Form Name and ID

### Setting Form Identity

```razor
<SfDataForm ID="EmployeeForm" 
            Model="@employee"
            FormName="EmployeeRegistration">
    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
</SfDataForm>

@code {
    private Employee employee = new();
}
```

### Using FormName for Multiple Forms

When you have multiple DataForms on a page, use `FormName` to differentiate them:

```razor
<!-- Employee Form -->
<SfDataForm FormName="EmployeeForm" Model="@employee">
    <FormItems>
        <FormItem Field="@nameof(employee.Name)" LabelText="Employee Name"></FormItem>
        <FormItem Field="@nameof(employee.Email)" LabelText="Email"></FormItem>
    </FormItems>
</SfDataForm>

<!-- Contact Form -->
<SfDataForm FormName="ContactForm" Model="@contact">
    <FormItems>
        <FormItem Field="@nameof(contact.Name)" LabelText="Contact Name"></FormItem>
        <FormItem Field="@nameof(contact.PhoneNumber)" LabelText="Phone"></FormItem>
    </FormItems>
</SfDataForm>

@code {
    private Employee employee = new();
    private Contact contact = new();
}
```

### Accessing Form by ID

Use `@ref` directive to access form methods and properties:

```razor
<SfDataForm @ref="employeeForm" ID="EmployeeForm" Model="@employee">
    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
</SfDataForm>

<button @onclick="ValidateForm">Validate</button>

@code {
    private SfDataForm employeeForm;
    private Employee employee = new();

    private async Task ValidateForm()
    {
        if (await employeeForm.IsValid())
        {
            Console.WriteLine("Form is valid");
        }
        else
        {
            Console.WriteLine("Form has errors");
        }
    }
}
```

## Best Practices

✅ **DO:**
- Use `nameof()` for type-safe field binding
- Initialize nested objects before binding
- Use EditContext for advanced validation scenarios
- Handle `OnUpdate` event for dependent field updates
- Load data in `OnInitializedAsync` lifecycle method

❌ **DON'T:**
- Reference fields as strings like `Field="FirstName"` (use `nameof()`)
- Leave nested objects uninitialized (causes null reference errors)
- Bind sensitive data directly without encryption
- Modify model during rendering (causes infinite loops)

## See Also

- [Form Items](form-items.md) - Explicit field configuration
- [Validation](validation.md) - Add validation rules
- [Events](events.md) - Handle form and field events
