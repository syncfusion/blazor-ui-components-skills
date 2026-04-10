# TextArea - Getting Started

Learn how to install, configure, and implement the Syncfusion Blazor TextArea component (SfTextArea) for multi-line text input in your Blazor applications.

## Installation

### Install NuGet Package

Add the Syncfusion.Blazor.Inputs package to your Blazor project:

```bash
dotnet add package Syncfusion.Blazor.Inputs
```

Or via Package Manager Console:

```powershell
Install-Package Syncfusion.Blazor.Inputs
```

### Register Syncfusion Services

In `Program.cs`, register the Syncfusion Blazor services:

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();
```

### Add Namespace Imports

Add the required namespace in `_Imports.razor`:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Inputs
```

## CSS and Script References

### Add Theme Stylesheet

Add the Syncfusion theme CSS in your `App.razor`, `_Layout.cshtml`, or `index.html`:

```html
<head>
    <!-- Syncfusion Blazor Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
```

Available themes:
- `bootstrap5.css` - Bootstrap 5 theme
- `material.css` - Material Design
- `fluent.css` - Fluent UI
- `tailwind.css` - Tailwind CSS
- `fabric.css` - Office Fabric

### Add Script Reference

Add the Syncfusion Blazor script at the end of `<body>`:

```html
<body>
    <!-- Your content -->
    
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
</body>
```

## Basic TextArea Implementation

### Minimal Example

Create a basic multi-line textarea:

```razor
@page "/textarea-demo"
@using Syncfusion.Blazor.Inputs

<h3>Basic TextArea</h3>

<SfTextArea Placeholder="Enter your comments here..."></SfTextArea>
```

### With Two-Way Binding

Bind the textarea value to a variable:

```razor
@page "/textarea-binding"
@using Syncfusion.Blazor.Inputs

<h3>TextArea with Binding</h3>

<SfTextArea @bind-Value="@userInput" 
            Placeholder="Type something...">
</SfTextArea>

<p>You typed: @userInput</p>

@code {
    private string userInput = "";
}
```

### Setting Rows and Columns

Control the initial size with `RowCount` and `ColumnCount`:

```razor
<SfTextArea @bind-Value="@description"
            RowCount="5"
            ColumnCount="50"
            Placeholder="Enter description (5 rows x 50 columns)">
</SfTextArea>

@code {
    private string description = "";
}
```

## Initial Configuration

### Common Properties

```razor
<SfTextArea @bind-Value="@content"
            RowCount="4"
            ColumnCount="40"
            MaxLength="500"
            Placeholder="Enter text (max 500 characters)..."
            Width="100%">
</SfTextArea>

@code {
    private string content = "";
}
```

### With Character Counter

Display remaining characters:

```razor
<div>
    <SfTextArea @bind-Value="@comment"
                MaxLength="200"
                Placeholder="Enter comment (max 200 chars)">
    </SfTextArea>
    <p>Characters remaining: @(200 - (comment?.Length ?? 0))</p>
</div>

@code {
    private string comment = "";
}
```

## Platform-Specific Setup

### Blazor Server

No additional configuration needed. The component works out of the box after service registration.

### Blazor WebAssembly

Ensure the Syncfusion.Blazor.Inputs package is added to the client project.

### Blazor Web App (.NET 8+)

For interactive components, add render mode at the top of the page:

```razor
@page "/textarea"
@rendermode InteractiveServer

<SfTextArea @bind-Value="@text"></SfTextArea>

@code {
    private string text = "";
}
```

Or use `@rendermode InteractiveWebAssembly` or `@rendermode InteractiveAuto` based on your needs.

## Complete Working Example

```razor
@page "/textarea-complete"
@using Syncfusion.Blazor.Inputs

<div class="container">
    <h3>Multi-Line Comment Form</h3>
    
    <div class="form-group">
        <label>Your Feedback:</label>
        <SfTextArea @bind-Value="@feedback"
                    RowCount="6"
                    ColumnCount="60"
                    MaxLength="1000"
                    Placeholder="Share your thoughts..."
                    Width="100%">
        </SfTextArea>
        <small>@(feedback?.Length ?? 0) / 1000 characters</small>
    </div>
    
    <button class="btn btn-primary" @onclick="SubmitFeedback">Submit</button>
    
    @if (!string.IsNullOrEmpty(submittedText))
    {
        <div class="alert alert-success mt-3">
            <strong>Submitted:</strong> @submittedText
        </div>
    }
</div>

@code {
    private string feedback = "";
    private string submittedText = "";
    
    private void SubmitFeedback()
    {
        submittedText = feedback;
        feedback = ""; // Clear after submit
    }
}
```

## Next Steps

- **Configuration**: Learn about resize modes, readonly, disabled states → [textarea-configuration.md](textarea-configuration.md)
- **Events**: Handle value changes, focus, and blur events → [textarea-events-binding.md](textarea-events-binding.md)
- **Customization**: Apply custom styles and themes → [textarea-customization.md](textarea-customization.md)

## Troubleshooting

**TextArea not rendering:**
- Verify `AddSyncfusionBlazor()` is called in Program.cs
- Check namespace imports in _Imports.razor
- Ensure theme CSS and script are referenced

**Value binding not working:**
- Use `@bind-Value` (not `@bind` alone)
- Ensure the bound variable is not null (initialize with empty string)
- Check for two-way binding syntax with `@` symbol

**Styles not applied:**
- Confirm theme CSS is loaded in the `<head>` section
- Check for CSS conflicts with global styles
- Verify the correct theme file name
