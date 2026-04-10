# Styling & Appearance

## Table of Contents
- [CSS Class Customization](#css-class-customization)
- [Changing Colors](#changing-colors)
- [Floating Label Colors](#floating-label-colors)
- [Background & Text Colors](#background--text-colors)
- [Theme Integration](#theme-integration)
- [Advanced Styling](#advanced-styling)

---

## CSS Class Customization

Apply custom CSS classes via the `CssClass` property:

```razor
@using Syncfusion.Blazor.Inputs

<style>
    .custom-textbox {
        border: 2px solid #4CAF50;
        border-radius: 8px;
        padding: 10px;
    }

    .primary-input {
        border-color: #2196F3;
        background-color: #f0f7ff;
    }

    .accent-input {
        border-color: #FF9800;
    }
</style>

<div style="margin: 50px auto; width: 350px;">
    <SfTextBox Placeholder="Custom styling"
               CssClass="custom-textbox">
    </SfTextBox>

    <SfTextBox Placeholder="Primary theme"
               CssClass="primary-input">
    </SfTextBox>

    <SfTextBox Placeholder="Accent theme"
               CssClass="accent-input">
    </SfTextBox>
</div>
```

### Built-in CSS Classes

| Class | Purpose |
|-------|---------|
| `e-small` | Reduces height and padding |
| `e-large` | Increases height and padding |
| `e-success` | Green validation state |
| `e-warning` | Yellow/orange validation state |
| `e-error` | Red validation state |

**Example: Validation States**

```razor
<SfTextBox Value="@ValidatedInput"
           CssClass="@GetValidationClass()"
           Input="@OnInput">
</SfTextBox>

@code {
    private string ValidatedInput = "";

    private string GetValidationClass()
    {
        return ValidatedInput.Length switch
        {
            0 => "",
            < 3 => "e-error",
            < 6 => "e-warning",
            _ => "e-success"
        };
    }

    private void OnInput(InputEventArgs args)
    {
        ValidatedInput = args.Value;
    }
}
```

---

## Changing Colors

### TextBox Border & Background

```razor
@using Syncfusion.Blazor.Inputs

<style>
    .blue-border {
        border-color: #2196F3 !important;
    }

    .light-background {
        background-color: #f5f5f5;
    }

    .gradient-border {
        border: 2px solid;
        border-color: #FF6B6B;
        background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
    }

    .colored-text {
        color: #1976D2;
    }
</style>

<div style="margin: 50px auto; width: 300px;">
    <SfTextBox Placeholder="Blue border"
               CssClass="blue-border">
    </SfTextBox>

    <SfTextBox Placeholder="Light background"
               CssClass="light-background">
    </SfTextBox>

    <SfTextBox Placeholder="Gradient background"
               CssClass="gradient-border colored-text">
    </SfTextBox>
</div>
```

### Dynamic Color Based on Value

```razor
@using Syncfusion.Blazor.Inputs

<style>
    .input-empty { background-color: #fff3cd; }
    .input-partial { background-color: #fff; }
    .input-complete { background-color: #d4edda; }
</style>

<SfTextBox @bind-Value="@Email"
           Placeholder="Enter email"
           CssClass="@GetEmailColorClass()"
           Input="@ValidateEmail">
</SfTextBox>

<p>Status: @EmailStatus</p>

@code {
    private string Email = "";
    private string EmailStatus = "Empty";

    private void ValidateEmail(InputEventArgs args)
    {
        Email = args.Value;
        EmailStatus = Email.Length == 0 ? "Empty" : 
                     Email.Contains("@") ? "Valid" : "Invalid";
    }

    private string GetEmailColorClass()
    {
        return Email.Length == 0 ? "input-empty" :
               Email.Contains("@") ? "input-complete" : "input-partial";
    }
}
```

---

## Floating Label Colors

Customize floating label appearance:

```razor
@using Syncfusion.Blazor.Inputs

<style>
    /* Focused floating label color */
    .custom-float:focus + label,
    .custom-float:not(:placeholder-shown) + label {
        color: #2196F3;
    }

    /* Float label wrapper styles */
    .float-label-custom .e-float-text {
        color: #666;
        font-size: 12px;
        font-weight: 500;
    }

    .float-label-custom .e-float-input:focus .e-float-text {
        color: #FF6B6B;
        font-size: 11px;
    }
</style>

<div style="margin: 50px auto; width: 350px;">
    <!-- Using inline styles -->
    <SfTextBox Placeholder="Custom label color"
               FloatLabelType="@FloatLabelType.Auto"
               CssClass="float-label-custom">
    </SfTextBox>
</div>
```

### Common Float Label Customizations

```razor
<style>
    /* Blue floating label on focus -->
    .blue-float:focus .e-float-text {
        color: #2196F3;
    }

    /* Animated color transition -->
    .animated-float .e-float-text {
        transition: color 0.3s ease;
    }

    .animated-float:focus .e-float-text {
        color: #FF6B6B;
    }

    /* Uppercase labels -->
    .uppercase-float .e-float-text {
        text-transform: uppercase;
        font-size: 10px;
        font-weight: 600;
    }
</style>

<SfTextBox Placeholder="Name"
           FloatLabelType="@FloatLabelType.Auto"
           CssClass="blue-float animated-float">
</SfTextBox>

<SfTextBox Placeholder="Email"
           FloatLabelType="@FloatLabelType.Auto"
           CssClass="uppercase-float">
</SfTextBox>
```

---

## Background & Text Colors

### Text Color

```razor
<style>
    .primary-text {
        color: #1976D2;
    }

    .muted-text {
        color: #999;
    }

    .accent-text {
        color: #FF6B6B;
    }

    .success-text {
        color: #4CAF50;
    }
</style>

<SfTextBox Placeholder="Primary text"
           Value="Primary color"
           Readonly="true"
           CssClass="primary-text">
</SfTextBox>

<SfTextBox Placeholder="Muted text"
           Value="Muted color"
           Readonly="true"
           CssClass="muted-text">
</SfTextBox>
```

### Background Color

```razor
<style>
    .primary-bg {
        background-color: #e3f2fd;
    }

    .success-bg {
        background-color: #e8f5e9;
    }

    .warning-bg {
        background-color: #fff3e0;
    }

    .error-bg {
        background-color: #ffebee;
    }

    /* Combined: background + text + border -->
    .validated {
        background-color: #e8f5e9;
        color: #2e7d32;
        border-color: #4caf50;
    }
</style>

<div style="margin: 50px auto; width: 300px;">
    <SfTextBox Placeholder="Success field"
               CssClass="success-bg">
    </SfTextBox>

    <SfTextBox Placeholder="Warning field"
               CssClass="warning-bg">
    </SfTextBox>

    <SfTextBox Placeholder="Error field"
               CssClass="error-bg">
    </SfTextBox>
</div>
```

---

## Theme Integration

Syncfusion TextBox components automatically inherit theme styling:

### Available Themes

| Theme | CSS Link |
|-------|----------|
| Bootstrap 5 | `_content/Syncfusion.Blazor.Themes/bootstrap5.css` |
| Bootstrap 5 Dark | `_content/Syncfusion.Blazor.Themes/bootstrap5-dark.css` |
| Fluent 2 | `_content/Syncfusion.Blazor.Themes/fluent2.css` |
| Material 3 | `_content/Syncfusion.Blazor.Themes/material3.css` |
| Tailwind | `_content/Syncfusion.Blazor.Themes/tailwind.css` |

**In `index.html`:**

```html
<!-- Switch themes by changing this line -->
<link href="_content/Syncfusion.Blazor.Themes/material3.css" rel="stylesheet" />
```

### Theme-Specific Colors

```razor
<style>
    /* Material 3 theme overrides -->
    .material-primary {
        background-color: #6750a4; /* Material primary color -->
    }

    .material-secondary {
        background-color: #625b71; /* Material secondary color -->
    }

    /* Bootstrap 5 theme overrides -->
    .bootstrap-primary {
        background-color: #0d6efd;
    }

    .bootstrap-danger {
        background-color: #dc3545;
    }
</style>
```

### Using CSS Variables

```razor
<style>
    :root {
        --primary-color: #2196F3;
        --border-radius: 8px;
        --focus-color: #1976D2;
    }

    .themed-input {
        border-color: var(--primary-color);
        border-radius: var(--border-radius);
    }

    .themed-input:focus {
        box-shadow: 0 0 0 3px rgba(var(--focus-color), 0.1);
    }
</style>

<SfTextBox Placeholder="Theme with CSS variables"
           CssClass="themed-input">
</SfTextBox>
```

---

## Advanced Styling

### Focus Effects

```razor
<style>
    .focus-glow:focus {
        box-shadow: 0 0 8px rgba(33, 150, 243, 0.5);
        outline: none;
    }

    .focus-shadow:focus {
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }

    .focus-border:focus {
        border-bottom-color: #2196F3;
        border-bottom-width: 3px;
    }
</style>

<SfTextBox Placeholder="Glow on focus" CssClass="focus-glow"></SfTextBox>
<SfTextBox Placeholder="Shadow on focus" CssClass="focus-shadow"></SfTextBox>
<SfTextBox Placeholder="Border on focus" CssClass="focus-border"></SfTextBox>
```

### Icon Styling

```razor
<style>
    .icon-styled .e-icons {
        color: #2196F3;
        font-size: 16px;
    }

    .icon-hover:hover .e-icons {
        color: #1976D2;
        transition: color 0.2s ease;
    }
</style>

<SfTextBox @ref="iconBox"
           Placeholder="Styled icon"
           Created="@AddStyledIcon"
           CssClass="icon-styled icon-hover">
</SfTextBox>

@code {
    private SfTextBox iconBox;

    private async void AddStyledIcon()
    {
        if (iconBox != null)
        {
            await iconBox.AddIconAsync("append", "e-icons e-search");
        }
    }
}
```

### Responsive Styling

```razor
<style>
    @media (max-width: 768px) {
        /* Mobile: larger input for touch -->
        .responsive-input {
            height: 44px;
            font-size: 16px;
        }
    }

    @media (min-width: 769px) {
        /* Desktop: compact input -->
        .responsive-input {
            height: 32px;
            font-size: 14px;
        }
    }
</style>

<SfTextBox Placeholder="Responsive sizing" CssClass="responsive-input"></SfTextBox>
```

---

## Next Steps

- ✅ [Events & Interactions](../events-handling.md) - Handle input interactions
- ✅ [Data Binding & Validation](../data-binding-validation.md) - Dynamic styling with state
- ✅ [Advanced Features](../advanced-features.md) - Custom components with styling
