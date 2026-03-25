# Getting Started with Syncfusion Blazor DataForm

## Installation and NuGet Packages

The Syncfusion Blazor DataForm requires two NuGet packages:
- `Syncfusion.Blazor.DataForm` - Core DataForm component
- `Syncfusion.Blazor.Themes` - Theme styles

### Install via Visual Studio

1. Open **Tools → NuGet Package Manager → Manage NuGet Packages for Solution**
2. Search for `Syncfusion.Blazor.DataForm` and install the latest version
3. Search for `Syncfusion.Blazor.Themes` and install
4. Click "Install" to confirm

### Install via Package Manager Console

```powershell
Install-Package Syncfusion.Blazor.DataForm
Install-Package Syncfusion.Blazor.Themes
```

### Install via .NET CLI

```bash
dotnet add package Syncfusion.Blazor.DataForm
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

## Blazor Project Setup

The DataForm component works with all three Blazor hosting models:

### Blazor Server App
- Full server-side rendering with SignalR
- Best for real-time updates and complex business logic

### Blazor WebAssembly App
- Client-side execution in the browser
- Best for offline scenarios and reduced server load

### Blazor Web App (.NET 8+)
- Hybrid model with multiple render modes
- Can mix server and client rendering per component

Regardless of your hosting model, the DataForm setup remains the same.

## Namespace Imports

Add these imports to your `_Imports.razor` file:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.DataForm
@using System.ComponentModel.DataAnnotations
```

## Service Registration

Register the Syncfusion Blazor service in `Program.cs`:

```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

## Theme Setup and Configuration

Add stylesheet and script references to your `index.html` (WebAssembly/Web App) or `_Host.cshtml` (Server):

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Blazor DataForm</title>
    
    <!-- Add Syncfusion theme stylesheet -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
<body>
    <div id="app"></div>
    
    <!-- Add Syncfusion script -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
</body>
```

### Available Themes

Choose one of these theme stylesheets:

```html
<!-- Bootstrap 5 (Default) -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Material Design -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Fluent UI (Microsoft) -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />

<!-- Tailwind CSS -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />

<!-- Material Dark -->
<link href="_content/Syncfusion.Blazor.Themes/material-dark.css" rel="stylesheet" />

<!-- Fabric (Office) -->
<link href="_content/Syncfusion.Blazor.Themes/fabric.css" rel="stylesheet" />
```

## Basic DataForm Creation

Create a simple form with auto-generated fields based on your model:

```razor
@page "/dataform-basic"
@using System.ComponentModel.DataAnnotations

<div class="container mt-5">
    <h2>Employee Registration Form</h2>
    
    <SfDataForm ID="EmployeeForm"
                Model="@employeeData">
        <FormValidator>
            <DataAnnotationsValidator></DataAnnotationsValidator>
        </FormValidator>
        <FormItems>
            <FormAutoGenerateItems></FormAutoGenerateItems>
        </FormItems>
    </SfDataForm>
</div>

@code {
    // Define your data model
    public class Employee
    {
        [Required(ErrorMessage = "First Name is required")]
        [Display(Name = "First Name")]
        public string FirstName { get; set; }

        [Required(ErrorMessage = "Last Name is required")]
        [Display(Name = "Last Name")]
        public string LastName { get; set; }

        [Required(ErrorMessage = "Email is required")]
        [EmailAddress(ErrorMessage = "Invalid email format")]
        [Display(Name = "Email")]
        public string Email { get; set; }

        [Display(Name = "Date of Birth")]
        public DateTime? DateOfBirth { get; set; }

        [Range(0, 150, ErrorMessage = "Please enter valid age")]
        [Display(Name = "Age")]
        public int Age { get; set; }

        [Display(Name = "Active")]
        public bool IsActive { get; set; } = true;
    }

    // Initialize your model
    private Employee employeeData = new Employee
    {
        FirstName = "John",
        LastName = "Doe"
    };
}
```

## Running Your First Form

1. **Build and Run:**
   ```bash
   dotnet run
   ```

2. **Navigate to Component:**
   - Add route to your component page
   - Open browser to the component URL

3. **Interact with Form:**
   - Fill in form fields
   - See real-time validation feedback
   - Submit the form

## Complete Minimal Example

Here's a complete page-level example with submission handling:

```razor
@page "/employee-form"
@using System.ComponentModel.DataAnnotations
@inject NavigationManager NavManager

<div class="form-container">
    <h3>New Employee Registration</h3>
    
    <SfDataForm ID="EmployeeForm"
                Model="@employee"
                OnValidSubmit="HandleValidSubmit">
        
        <FormValidator>
            <DataAnnotationsValidator></DataAnnotationsValidator>
            <ValidationSummary></ValidationSummary>
        </FormValidator>
        
        <FormItems>
            <FormAutoGenerateItems></FormAutoGenerateItems>
        </FormItems>
        
        <FormButtons>
            <button type="submit" class="btn btn-primary">Submit</button>
            <button type="reset" class="btn btn-secondary">Clear</button>
        </FormButtons>
    </SfDataForm>
</div>

@code {
    public class Employee
    {
        [Required]
        [StringLength(50)]
        [Display(Name = "First Name")]
        public string FirstName { get; set; }

        [Required]
        [StringLength(50)]
        [Display(Name = "Last Name")]
        public string LastName { get; set; }

        [Required]
        [EmailAddress]
        [Display(Name = "Email Address")]
        public string Email { get; set; }

        [Display(Name = "Start Date")]
        public DateTime StartDate { get; set; }
    }

    private Employee employee = new();

    private async Task HandleValidSubmit(EditContext context)
    {
        // Handle form submission
        await SaveEmployeeAsync(employee);
        NavManager.NavigateTo("/employees");
    }

    private async Task SaveEmployeeAsync(Employee emp)
    {
        // Save employee to database
        await Task.Delay(500);
        Console.WriteLine($"Saved: {emp.FirstName} {emp.LastName}");
    }
}
```

## Troubleshooting Common Setup Issues

### Issue: Component not rendering
- **Solution:** Verify `AddSyncfusionBlazor()` is called in `Program.cs`
- **Solution:** Check theme stylesheet is loaded in HTML head
- **Solution:** Ensure NuGet packages are installed: `dotnet restore`

### Issue: Styles not applying
- **Solution:** Clear browser cache (Ctrl+Shift+Delete)
- **Solution:** Check theme CSS file path in HTML
- **Solution:** Try different theme to isolate CSS issue

### Issue: Validation not working
- **Solution:** Verify `DataAnnotationsValidator` is included
- **Solution:** Check model properties have `[Required]` or validation attributes
- **Solution:** Ensure `System.ComponentModel.DataAnnotations` is imported

### Issue: Form binding not working
- **Solution:** Check `Model` property is set on `SfDataForm`
- **Solution:** Verify model property names match `Field` attributes
- **Solution:** Ensure model is public class with public properties

## Next Steps

- [Configure form items and fields](form-items.md) - Learn to customize individual form fields
- [Set up validation](validation.md) - Implement comprehensive form validation
- [Handle form events](events.md) - Respond to user interactions
