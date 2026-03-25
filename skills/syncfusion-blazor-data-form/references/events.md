# Form and Field Events in Blazor DataForm

## Table of Contents

- [Overview](#overview)
- [OnSubmit Event](#onsubmit-event)
- [OnValidSubmit Event](#onvalidsubmit-event)
- [OnInvalidSubmit Event](#oninvalidsubmit-event)
- [OnUpdate Event](#onupdate-event)
- [Event Arguments](#event-arguments)
- [Asynchronous Event Handlers](#asynchronous-event-handlers)
- [Common Event Patterns](#common-event-patterns)

## Overview

The DataForm provides form-level and field-level events for handling user interactions:

| Event | When it fires | Use case |
|-------|---|---|
| `OnSubmit` | Every submit attempt (valid or invalid) | Logging, pre-processing |
| `OnValidSubmit` | Submit with all validation passing | Save data, redirect |
| `OnInvalidSubmit` | Submit with validation failures | Show error summary |
| `OnUpdate` | Any field value changes | Dependent field updates, calculations |

## OnSubmit Event

The `OnSubmit` event fires every time the form is submitted, regardless of validation results. Use this for actions that must occur on every submit attempt:

```razor
<SfDataForm Model="@contact" OnSubmit="HandleSubmit">
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
        [Required]
        public string Name { get; set; }

        [EmailAddress]
        public string Email { get; set; }
    }

    private Contact contact = new();

    private async Task HandleSubmit(EditContext context)
    {
        // Log submission attempt
        Console.WriteLine($"Form submitted at {DateTime.Now}");

        // Pre-processing: trim whitespace
        contact.Name = contact.Name?.Trim();
        contact.Email = contact.Email?.Trim();

        // Continue to validation
        await Task.CompletedTask;
    }
}
```

### OnSubmit with Model Binding

```csharp
private async Task HandleSubmit(EditContext context)
{
    var model = (Contact)context.Model;
    Console.WriteLine($"Submitted: {model.Name} ({model.Email})");
}
```

## OnValidSubmit Event

The `OnValidSubmit` event fires only when the form is submitted AND all validation rules pass. Use this to save data or navigate:

```razor
<SfDataForm Model="@employee" OnValidSubmit="HandleValidSubmit">
    <FormValidator>
        <DataAnnotationsValidator></DataAnnotationsValidator>
    </FormValidator>
    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
    <FormButtons>
        <button type="submit" class="btn btn-success">Save Employee</button>
    </FormButtons>
</SfDataForm>

@code {
    public class Employee
    {
        [Required]
        [StringLength(100)]
        public string Name { get; set; }

        [Required]
        [EmailAddress]
        public string Email { get; set; }

        [Range(1, 1000000)]
        public decimal Salary { get; set; }
    }

    private Employee employee = new();
    private string message;

    private async Task HandleValidSubmit(EditContext context)
    {
        // Only executes if all validation passes
        try
        {
            await SaveEmployeeAsync(employee);
            message = "Employee saved successfully!";
        }
        catch (Exception ex)
        {
            message = $"Error saving employee: {ex.Message}";
        }
    }

    private async Task SaveEmployeeAsync(Employee emp)
    {
        // Simulate API call
        await Task.Delay(1000);
        Console.WriteLine($"Saved: {emp.Name}");
    }
}
```

### OnValidSubmit with Navigation

```csharp
@inject NavigationManager Navigation

private async Task HandleValidSubmit(EditContext context)
{
    await SaveEmployeeAsync(employee);
    
    // Navigate to success page
    Navigation.NavigateTo("/employees/success");
}
```

## OnInvalidSubmit Event

The `OnInvalidSubmit` event fires when the form is submitted but validation fails. Use this to handle errors or show messages:

```razor
<SfDataForm Model="@form" OnInvalidSubmit="HandleInvalidSubmit">
    <FormValidator>
        <DataAnnotationsValidator></DataAnnotationsValidator>
    </FormValidator>

    @if (!string.IsNullOrEmpty(errorMessage))
    {
        <div class="alert alert-danger" role="alert">
            <strong>Validation Error!</strong> @errorMessage
        </div>
    }

    <FormItems>
        <FormItem Field="@nameof(form.Email)" 
                  LabelText="Email"></FormItem>
        <FormItem Field="@nameof(form.Age)" 
                  LabelText="Age"></FormItem>
    </FormItems>
</SfDataForm>

@code {
    public class Form
    {
        [Required]
        [EmailAddress(ErrorMessage = "Please enter a valid email")]
        public string Email { get; set; }

        [Range(18, 100, ErrorMessage = "Age must be 18-100")]
        public int Age { get; set; }
    }

    private Form form = new();
    private string errorMessage;

    private Task HandleInvalidSubmit(EditContext context)
    {
        // Get first validation error
        var firstError = context.GetValidationMessages().FirstOrDefault();
        errorMessage = firstError ?? "Please fix the errors above";
        
        return Task.CompletedTask;
    }
}
```

## OnUpdate Event

The `OnUpdate` event fires whenever a field value changes. This is useful for:
- Dependent field updates (cascading dropdowns)
- Real-time calculations
- Dynamic form behavior

```razor
<SfDataForm Model="@order" OnUpdate="HandleFieldUpdate">
    <FormValidator>
        <DataAnnotationsValidator></DataAnnotationsValidator>
    </FormValidator>
    <FormItems>
        <FormItem Field="@nameof(order.Quantity)" 
                  LabelText="Quantity"
                  >
        </FormItem>
        <FormItem Field="@nameof(order.UnitPrice)" 
                  LabelText="Unit Price"
                  >
        </FormItem>
        <FormItem Field="@nameof(order.TotalPrice)" 
                  LabelText="Total Price"
                  IsEnabled="false">
        </FormItem>
    </FormItems>
</SfDataForm>

@code {
    public class Order
    {
        public int Quantity { get; set; }
        public decimal UnitPrice { get; set; }
        public decimal TotalPrice { get; set; }
    }

    private Order order = new();

    private Task HandleFieldUpdate(FormUpdateEventArgs args)
    {
        // Calculate total whenever quantity or price changes
        if (args.FieldName == nameof(Order.Quantity) || 
            args.FieldName == nameof(Order.UnitPrice))
        {
            order.TotalPrice = order.Quantity * order.UnitPrice;
        }

        return Task.CompletedTask;
    }
}
```

### Cascading Dropdowns Example

```razor
<SfDataForm Model="@location" OnUpdate="HandleLocationUpdate">
    <FormItems>
        <FormItem Field="@nameof(location.Country)" 
                  LabelText="Country"
                  EditorType="FormEditorType.DropDownList">
        </FormItem>
        <FormItem Field="@nameof(location.State)" 
                  LabelText="State"
                  EditorType="FormEditorType.DropDownList">
        </FormItem>
        <FormItem Field="@nameof(location.City)" 
                  LabelText="City"
                  EditorType="FormEditorType.DropDownList">
        </FormItem>
    </FormItems>
</SfDataForm>

@code {
    public class Location
    {
        public string Country { get; set; }
        public string State { get; set; }
        public string City { get; set; }
    }

    private Location location = new();

    private async Task HandleLocationUpdate(FormUpdateEventArgs args)
    {
        // Reset dependent fields when parent changes
        if (args.FieldName == nameof(Location.Country))
        {
            location.State = null;
            location.City = null;
            // Load states for selected country
            var states = await GetStatesForCountryAsync(location.Country);
        }
        else if (args.FieldName == nameof(Location.State))
        {
            location.City = null;
            // Load cities for selected state
            var cities = await GetCitiesForStateAsync(location.State);
        }
    }

    private async Task<List<string>> GetStatesForCountryAsync(string country)
    {
        // Load from API or database
        return new List<string> { "State 1", "State 2" };
    }

    private async Task<List<string>> GetCitiesForStateAsync(string state)
    {
        // Load from API or database
        return new List<string> { "City 1", "City 2" };
    }
}
```

## Event Arguments

### FormUpdateEventArgs

Passed to `OnUpdate` event handler:

```csharp
public class FormUpdateEventArgs
{
    public string FieldName { get; set; }  // Name of updated field
    public object Value { get; set; }      // New value
    public object OldValue { get; set; }   // Previous value
}
```

Usage:

```csharp
private Task HandleUpdate(FormUpdateEventArgs args)
{
    Console.WriteLine($"Field '{args.FieldName}' changed from {args.OldValue} to {args.Value}");
    return Task.CompletedTask;
}
```

### EditContext

Passed to form-level event handlers. Access validation and model:

```csharp
private async Task HandleSubmit(EditContext context)
{
    // Get the model
    var model = (MyModel)context.Model;

    // Check if valid
    bool isValid = !context.GetValidationMessages().Any();

    // Get validation messages
    var messages = context.GetValidationMessages();

    // Mark field as modified
    context.MarkAsUnmodified(context.FieldIdentifiers.First());
}
```

## Asynchronous Event Handlers

Event handlers support async operations for API calls:

```razor
<SfDataForm Model="@user" OnValidSubmit="HandleValidSubmit">
    <FormValidator>
        <DataAnnotationsValidator></DataAnnotationsValidator>
    </FormValidator>
    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
</SfDataForm>

@if (!string.IsNullOrEmpty(statusMessage))
{
    <div class="alert alert-info">@statusMessage</div>
}

@code {
    public class User
    {
        [Required]
        public string Name { get; set; }

        [Required]
        [EmailAddress]
        public string Email { get; set; }
    }

    private User user = new();
    private string statusMessage;

    private async Task HandleValidSubmit(EditContext context)
    {
        statusMessage = "Saving user...";

        try
        {
            // Make API call
            var response = await httpClient.PostAsJsonAsync("/api/users", user);

            if (response.IsSuccessStatusCode)
            {
                statusMessage = "User saved successfully!";
            }
            else
            {
                statusMessage = "Failed to save user";
            }
        }
        catch (Exception ex)
        {
            statusMessage = $"Error: {ex.Message}";
        }
    }
}
```

### Async Field Update

```csharp
private async Task HandleFieldUpdate(FormUpdateEventArgs args)
{
    if (args.FieldName == nameof(user.Email))
    {
        // Check if email is available
        var isAvailable = await CheckEmailAvailabilityAsync(user.Email);
        
        if (!isAvailable)
        {
            // Handle unavailable email
        }
    }
}

private async Task<bool> CheckEmailAvailabilityAsync(string email)
{
    var response = await httpClient.GetAsync($"/api/users/check-email?email={email}");
    return response.IsSuccessStatusCode;
}
```

## Common Event Patterns

### Pattern 1: Validation and Save

```csharp
private async Task HandleValidSubmit(EditContext context)
{
    try
    {
        // Validation already passed, save data
        await SaveDataAsync();
        await ShowSuccessMessage("Saved successfully");
        await NavigateAsync("/success");
    }
    catch (Exception ex)
    {
        await ShowErrorMessage($"Save failed: {ex.Message}");
    }
}
```

### Pattern 2: Dependent Field Updates

```csharp
private Task HandleFieldUpdate(FormUpdateEventArgs args)
{
    switch (args.FieldName)
    {
        case nameof(Order.Country):
            ResetCountryDependentFields();
            break;

        case nameof(Order.ProductType):
            UpdateAvailableDiscounts();
            break;

        case nameof(Order.Quantity):
            RecalculateTotal();
            break;
    }

    StateHasChanged();
    return Task.CompletedTask;
}
```

### Pattern 3: Real-Time Validation

```csharp
private async Task HandleFieldUpdate(FormUpdateEventArgs args)
{
    if (args.FieldName == nameof(User.Username))
    {
        var isUnique = await CheckUsernameAvailabilityAsync(user.Username);
        
        if (!isUnique)
        {
            // Show warning to user
            usernameError = "Username already taken";
        }
        else
        {
            usernameError = null;
        }
    }
}
```

### Pattern 4: Conditional Field Display

```csharp
private Task HandleFieldUpdate(FormUpdateEventArgs args)
{
    if (args.FieldName == nameof(Order.HasInsurance))
    {
        // Toggle visibility of insurance-related fields
        showInsuranceFields = (bool)args.Value;
    }

    StateHasChanged();
    return Task.CompletedTask;
}
```

## See Also

- [Validation](validation.md) - Set up form validation rules
- [Form Items](form-items.md) - Configure individual fields
- [Data Binding](data-binding.md) - Bind data to forms
