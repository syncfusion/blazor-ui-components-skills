# Getting Started with InputMask (MaskedTextBox)

This guide covers the installation, setup, and basic implementation of the Syncfusion Blazor InputMask (SfMaskedTextBox) component for formatted text input.

## Installation and Setup

### NuGet Package Installation

Install the Syncfusion.Blazor.Inputs NuGet package in your Blazor application:

**Package Manager Console:**
```bash
Install-Package Syncfusion.Blazor.Inputs
```

**dotnet CLI:**
```bash
dotnet add package Syncfusion.Blazor.Inputs
```

**Visual Studio NuGet Package Manager:**
1. Right-click on the project → Manage NuGet Packages
2. Search for "Syncfusion.Blazor.Inputs"
3. Install the package

### Service Registration

Add Syncfusion Blazor services in `Program.cs`:

**Blazor Server / Blazor Web App (.NET 6+):**
```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddSyncfusionBlazor();
// ... rest of your configuration
```

**Blazor WebAssembly:**
```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.Services.AddSyncfusionBlazor();
// ... rest of your configuration
```

### Add Namespace

Add the namespace in `_Imports.razor`:

```razor
@using Syncfusion.Blazor.Inputs
```

### CSS Theme Configuration

Add the CSS reference in your layout file:

**For Blazor Server/Web App - `_Layout.cshtml` or `App.razor`:**
```html
<head>
    <!-- Material 3 Theme (default) -->
    <link href="_content/Syncfusion.Blazor.Themes/material3.css" rel="stylesheet" />
    
    <!-- Or choose another theme: -->
    <!-- <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" /> -->
    <!-- <link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" /> -->
    <!-- <link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" /> -->
</head>
```

**For Blazor WebAssembly - `wwwroot/index.html`:**
```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/material3.css" rel="stylesheet" />
</head>
```

## Basic Implementation

### Minimal Working Example

```razor
@page "/masked-input-demo"
@using Syncfusion.Blazor.Inputs

<div class="masked-input-container">
    <h3>Basic Masked Input</h3>
    
    <label>Phone Number:</label>
    <SfMaskedTextBox Mask="(000) 000-0000" @bind-Value="@phoneNumber"></SfMaskedTextBox>
    
    @if (!string.IsNullOrEmpty(phoneNumber))
    {
        <p>Value: @phoneNumber</p>
    }
</div>

@code {
    private string phoneNumber = "";
}
```

### Common Mask Examples

#### Phone Number (US Format)

```razor
<label>Phone Number:</label>
<SfMaskedTextBox Mask="(000) 000-0000" 
                 Placeholder="Enter phone number"
                 @bind-Value="@phoneNumber">
</SfMaskedTextBox>

@code {
    private string phoneNumber = "";
}
```

**Output Format:** `(555) 123-4567`

#### Social Security Number

```razor
<label>SSN:</label>
<SfMaskedTextBox Mask="000-00-0000" 
                 Placeholder="Enter SSN"
                 @bind-Value="@ssn">
</SfMaskedTextBox>

@code {
    private string ssn = "";
}
```

**Output Format:** `123-45-6789`

#### Date Input

```razor
<label>Date (MM/DD/YYYY):</label>
<SfMaskedTextBox Mask="00/00/0000" 
                 Placeholder="MM/DD/YYYY"
                 @bind-Value="@dateValue">
</SfMaskedTextBox>

@code {
    private string dateValue = "";
}
```

**Output Format:** `12/31/2026`

#### ZIP Code

```razor
<label>ZIP Code:</label>
<SfMaskedTextBox Mask="00000" 
                 Placeholder="Enter ZIP"
                 @bind-Value="@zipCode">
</SfMaskedTextBox>

@code {
    private string zipCode = "";
}
```

**Output Format:** `90210`

#### ZIP+4 Format

```razor
<label>ZIP+4:</label>
<SfMaskedTextBox Mask="00000-0000" 
                 Placeholder="Enter ZIP+4"
                 @bind-Value="@zipPlus4">
</SfMaskedTextBox>

@code {
    private string zipPlus4 = "";
}
```

**Output Format:** `90210-1234`

## Value Binding

### Two-Way Binding

Use `@bind-Value` for automatic two-way data binding:

```razor
<SfMaskedTextBox Mask="(000) 000-0000" @bind-Value="@phoneNumber"></SfMaskedTextBox>

<p>Current Value: @phoneNumber</p>

@code {
    private string phoneNumber = "5551234567"; // Pre-populate
}
```

### One-Way Binding with Event

Use `Value` and `ValueChanged` for explicit control:

```razor
<SfMaskedTextBox Mask="(000) 000-0000" 
                 Value="@phoneNumber"
                 ValueChanged="@OnPhoneChanged">
</SfMaskedTextBox>

@code {
    private string phoneNumber = "";
    
    private void OnPhoneChanged(string value)
    {
        phoneNumber = value;
        // Perform additional logic
        Console.WriteLine($"Phone number changed: {value}");
    }
}
```

## Initial Value Setup

### Set Initial Value

```razor
<SfMaskedTextBox Mask="000-00-0000" @bind-Value="@ssn"></SfMaskedTextBox>

@code {
    private string ssn = "123456789"; // Will display as: 123-45-6789
}
```

### Clear Value

```razor
<SfMaskedTextBox Mask="(000) 000-0000" @bind-Value="@phoneNumber"></SfMaskedTextBox>
<button @onclick="ClearPhone">Clear</button>

@code {
    private string phoneNumber = "";
    
    private void ClearPhone()
    {
        phoneNumber = "";
    }
}
```

## Complete Getting Started Example

```razor
@page "/masked-input-forms"
@using Syncfusion.Blazor.Inputs

<div class="container">
    <h2>Contact Information Form</h2>
    
    <div class="form-group">
        <label>Phone Number:</label>
        <SfMaskedTextBox Mask="(000) 000-0000" 
                         Placeholder="Enter phone"
                         @bind-Value="@contact.Phone"
                         FloatLabelType="FloatLabelType.Auto">
        </SfMaskedTextBox>
    </div>
    
    <div class="form-group">
        <label>SSN:</label>
        <SfMaskedTextBox Mask="000-00-0000" 
                         Placeholder="Enter SSN"
                         @bind-Value="@contact.SSN"
                         FloatLabelType="FloatLabelType.Auto">
        </SfMaskedTextBox>
    </div>
    
    <div class="form-group">
        <label>ZIP Code:</label>
        <SfMaskedTextBox Mask="00000-0000" 
                         Placeholder="Enter ZIP+4"
                         @bind-Value="@contact.ZipCode"
                         FloatLabelType="FloatLabelType.Auto">
        </SfMaskedTextBox>
    </div>
    
    <button @onclick="SubmitForm" class="btn btn-primary">Submit</button>
    
    @if (submitted)
    {
        <div class="result">
            <h4>Submitted Values:</h4>
            <p>Phone: @contact.Phone</p>
            <p>SSN: @contact.SSN</p>
            <p>ZIP: @contact.ZipCode</p>
        </div>
    }
</div>

<style>
    .container {
        max-width: 500px;
        margin: 20px;
    }
    
    .form-group {
        margin-bottom: 20px;
    }
    
    .form-group label {
        display: block;
        margin-bottom: 5px;
        font-weight: 500;
    }
    
    .result {
        margin-top: 20px;
        padding: 15px;
        background-color: #f0f0f0;
        border-radius: 4px;
    }
</style>

@code {
    private ContactInfo contact = new ContactInfo();
    private bool submitted = false;
    
    private void SubmitForm()
    {
        submitted = true;
        // Process form data
    }
    
    public class ContactInfo
    {
        public string Phone { get; set; } = "";
        public string SSN { get; set; } = "";
        public string ZipCode { get; set; } = "";
    }
}
```

## Next Steps

Now that you have the basic InputMask setup working, explore:

- **[Mask Patterns](inputmask-mask-patterns.md)** - Learn about all mask characters and pattern rules
- **[Custom Characters](inputmask-custom-characters.md)** - Create custom validation rules
- **[Prompt Configuration](inputmask-prompt-configuration.md)** - Customize prompt characters and display
- **[Events and Binding](inputmask-events-binding.md)** - Handle user input and form validation
- **[Styling and Accessibility](inputmask-styling-accessibility.md)** - Theme customization and WCAG compliance
