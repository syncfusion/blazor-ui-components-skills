# Getting Started with Blazor Query Builder

This guide walks through installing and setting up the Syncfusion Blazor Query Builder component in both Blazor WebAssembly and Blazor Server applications.

## Prerequisites

- .NET 6.0 or later
- Visual Studio 2022 or Visual Studio Code
- Basic knowledge of Blazor and C#

## Installation

### Step 1: Create a Blazor Project

#### Via Visual Studio
1. Open Visual Studio 2022
2. Create New Project → Blazor WebAssembly App or Blazor Server App
3. Configure project name and location
4. Select target framework (.NET 8.0 or later)

#### Via Command Line
```bash
# For Blazor WebAssembly
dotnet new blazorwasm -n QueryBuilderApp
cd QueryBuilderApp

# For Blazor Server
dotnet new blazorserver -n QueryBuilderApp
cd QueryBuilderApp
```

### Step 2: Install NuGet Packages

Install the Query Builder and Themes packages:

```bash
dotnet add package Syncfusion.Blazor.QueryBuilder
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

Or via Package Manager Console:
```powershell
Install-Package Syncfusion.Blazor.QueryBuilder
Install-Package Syncfusion.Blazor.Themes
```

### Step 3: Register Syncfusion Services

In `Program.cs`, add the Syncfusion services:

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// For Blazor WebAssembly
builder.Services.AddScoped(sp => new HttpClient { 
    BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) 
});

// For Blazor Server
builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();

// Register Syncfusion services
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();
// ... rest of configuration
```

### Step 4: Add Syncfusion Theme

In your host page (`_Host.cshtml` for Blazor Server or `index.html` for Blazor WebAssembly), add the theme CSS in the `<head>` section:

```html
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Query Builder App</title>
    <base href="~/" />
    
    <!-- Syncfusion Theme -->
    <link rel="stylesheet" href="_content/Syncfusion.Blazor.Themes/bootstrap5.css">
    
    <link rel="stylesheet" href="css/app.css" />
</head>
```

Available themes:
- `bootstrap5.css` - Bootstrap 5 theme
- `material3.css` - Material Design 3 theme
- `fluent2.css` - Microsoft Fluent 2 theme
- `tailwindcss.css` - Tailwind CSS theme

### Step 5: Create Your First Query Builder

Create or edit a component (e.g., `Components/QueryBuilderDemo.razor`):

```razor
@page "/query-builder"
@using Syncfusion.Blazor.QueryBuilder

<h3>Query Builder Demo</h3>

<SfQueryBuilder TValue="Employee" DataSource="@EmployeeData">
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="EmployeeID" Label="Employee ID" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="FirstName" Label="First Name" Type="ColumnType.String"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Department" Label="Department" Type="ColumnType.String"></QueryBuilderColumn>
        <QueryBuilderColumn Field="HireDate" Label="Hire Date" Type="ColumnType.Date" Format="MM/dd/yyyy"></QueryBuilderColumn>
        <QueryBuilderColumn Field="IsActive" Label="Active" Type="ColumnType.Boolean"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

@code {
    private List<Employee> EmployeeData = new();

    protected override void OnInitialized() {
        EmployeeData = new() {
            new() { EmployeeID = 1, FirstName = "John", Department = "Engineering", HireDate = new(2020, 3, 15), IsActive = true },
            new() { EmployeeID = 2, FirstName = "Jane", Department = "Sales", HireDate = new(2021, 5, 20), IsActive = true },
            new() { EmployeeID = 3, FirstName = "Bob", Department = "HR", HireDate = new(2019, 1, 10), IsActive = false }
        };
    }

    public class Employee {
        public int EmployeeID { get; set; }
        public string FirstName { get; set; }
        public string Department { get; set; }
        public DateTime HireDate { get; set; }
        public bool IsActive { get; set; }
    }
}
```

### Step 6: Run the Application

```bash
dotnet run
```

Navigate to your component's route (e.g., `/query-builder`) to see the Query Builder in action.

## Verifying Installation

You should see a Query Builder with:
- Column dropdown for selecting fields
- Operator dropdown for comparison types
- Input field for entering filter values
- Add Rule and Add Group buttons
- Delete buttons for removing rules/groups

If the Query Builder doesn't render:
1. Check browser console (F12) for JavaScript errors
2. Verify CSS link is correct in the host page
3. Ensure `AddSyncfusionBlazor()` is called in `Program.cs`
4. Check that NuGet packages are properly installed

## Next Steps

- [Basic Setup](basic-setup.md) - Configure columns and default rules
- [Filtering & Rules](filtering-and-rules.md) - Add/delete rules programmatically
- [Events & Callbacks](events-and-callbacks.md) - Handle query changes
- [Customization](customization-and-styling.md) - Style and template the UI
