# Getting Started with NumericTextBox

## Table of Contents
- [Installation and Setup](#installation-and-setup)
- [WebAssembly App Setup](#webassembly-app-setup)
- [Server App Setup](#server-app-setup)
- [Basic Implementation](#basic-implementation)
- [First Examples](#first-examples)
- [Verify Installation](#verify-installation)

## Installation and Setup

The NumericTextBox component is part of the `Syncfusion.Blazor.Inputs` package. You need to install both the inputs component package and the themes package.

**Required NuGet Packages:**
- `Syncfusion.Blazor.Inputs` - NumericTextBox component
- `Syncfusion.Blazor.Themes` - CSS themes for styling

## WebAssembly App Setup

### Step 1: Install NuGet Packages

Using Package Manager Console:
```csharp
Install-Package Syncfusion.Blazor.Inputs
Install-Package Syncfusion.Blazor.Themes
```

Or using .NET CLI:
```bash
dotnet add package Syncfusion.Blazor.Inputs
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

### Step 2: Add Namespaces to _Imports.razor

Open `~/_Imports.razor` and add:
```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Inputs
```

### Step 3: Register Service in Program.cs

In `~/Program.cs`, add Syncfusion Blazor service:
```csharp
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

### Step 4: Add Stylesheet and Script

In `~/index.html`, add inside `<head>` section:
```html
<head>
    ....
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

## Server App Setup

### Step 1: Install NuGet Packages

Using Package Manager Console:
```csharp
Install-Package Syncfusion.Blazor.Inputs
Install-Package Syncfusion.Blazor.Themes
```

Or using .NET CLI:
```bash
dotnet add package Syncfusion.Blazor.Inputs
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

### Step 2: Add Namespaces to _Imports.razor

Open `~/_Imports.razor` and add:
```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Inputs
```

### Step 3: Register Service in Program.cs

In `~/Program.cs`, add Syncfusion Blazor service:
```csharp
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Web;
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();
// ... rest of configuration
```

### Step 4: Add Stylesheet and Script

In `~/Components/App.razor` or main layout:
```html
<head>
    ....
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
....
<body>
    ....
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</body>
```

### Step 5: Configure Render Mode (Optional)

If using interactive render mode per component:
```razor
@rendermode InteractiveServer
```

## Basic Implementation

### Minimal Example

```razor
<SfNumericTextBox TValue="int?" Value=10></SfNumericTextBox>
```

**Key Points:**
- `TValue="int?"` - Declares the numeric type (nullable int, decimal, double, etc.)
- `Value=10` - Initial value displayed in the input

### Understanding TValue

The `TValue` parameter determines what numeric type the component accepts:

```razor
<!-- Integer values -->
<SfNumericTextBox TValue="int?" Value=10></SfNumericTextBox>

<!-- Decimal values (best for currency) -->
<SfNumericTextBox TValue="decimal?" Value=99.99></SfNumericTextBox>

<!-- Double values -->
<SfNumericTextBox TValue="double?" Value=10.5></SfNumericTextBox>

<!-- Long values -->
<SfNumericTextBox TValue="long?" Value=1000000></SfNumericTextBox>
```

**Choose TValue based on use case:**
- `int?` - Whole numbers only (quantities, ages)
- `decimal?` - Monetary values (prices, totals) - **Most accurate for currency**
- `double?` - Decimal calculations (measurements, ratios)
- `long?` - Large whole numbers (IDs, large quantities)

## First Examples

### Example 1: Simple Counter

```razor
@page "/numerictextbox/simple"

<h3>Simple NumericTextBox</h3>
<SfNumericTextBox TValue="int?" Value=5></SfNumericTextBox>
<p>Enter a number in the field above.</p>
```

### Example 2: Currency Input

```razor
@page "/numerictextbox/currency"

<h3>Price Input</h3>
<SfNumericTextBox TValue="decimal?" Value=99.99 Placeholder="Enter price"></SfNumericTextBox>
```

### Example 3: With Range Validation

```razor
@page "/numerictextbox/range"

<h3>Score Input (0-100)</h3>
<SfNumericTextBox TValue="int?" Value=50 Min=0 Max=100 Placeholder="Enter score"></SfNumericTextBox>
```

### Example 4: Interactive with Display

```razor
@page "/numerictextbox/interactive"

<h3>Interactive Example</h3>
<SfNumericTextBox TValue="int?" @bind-Value="myNumber"></SfNumericTextBox>
<p>You entered: @myNumber</p>

@code {
    private int? myNumber = 10;
}
```

## Verify Installation

After setup, verify everything works correctly:

1. **Create a test page:**
```razor
@page "/test-numerictextbox"

<SfNumericTextBox TValue="int?" Value=42></SfNumericTextBox>
```

2. **Run the application:**
   - WebAssembly: `dotnet watch run`
   - Server: `dotnet watch run`

3. **Check the browser:**
   - Navigate to `/test-numerictextbox`
   - You should see an input field with value 42
   - Click spin buttons to increment/decrement
   - Type numbers directly in the field

**If component doesn't render:**
- ✅ Verify namespaces in `_Imports.razor`
- ✅ Check `Program.cs` has `AddSyncfusionBlazor()`
- ✅ Confirm CSS and JS files loaded in browser (F12 → Network tab)
- ✅ Check browser console for JavaScript errors

**Common Issues:**
- Component appears as plain text → Missing CSS import
- Buttons don't work → Missing JavaScript file
- Compilation error → Missing `@using` statements

## Next Steps

- See [range-validation-and-formatting.md](range-validation-and-formatting.md) to add validation and formatting
- See [data-binding-and-events.md](data-binding-and-events.md) for two-way binding
- See [customization-styling.md](customization-styling.md) for styling options
