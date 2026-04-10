# InputMask Styling and Accessibility

## Table of Contents
- [FloatLabelType Configuration](#floatlabeltype-configuration)
- [ShowClearButton Functionality](#showclearbutton-functionality)
- [CssClass Custom Styling](#cssclass-custom-styling)
- [Width and Responsive Sizing](#width-and-responsive-sizing)
- [Component States](#component-states)
- [Theme Customization](#theme-customization)
- [WCAG 2.1 Compliance](#wcag-21-compliance)
- [Keyboard Navigation](#keyboard-navigation)
- [ARIA Attributes](#aria-attributes)
- [Screen Reader Support](#screen-reader-support)
- [RTL Support](#rtl-support)
- [HtmlAttributes and InputAttributes](#htmlattributes-and-inputattributes)

## FloatLabelType Configuration

The `FloatLabelType` property controls floating label behavior, creating a modern Material Design-style input experience.

### FloatLabelType Options

```csharp
public enum FloatLabelType
{
    Never,    // Label always above input (default)
    Auto,     // Label floats on focus or when value exists
    Always    // Label always floats above input
}
```

### FloatLabelType.Never (Default)

Label remains in fixed position above the input:

```razor
<SfMaskedTextBox Mask="(000) 000-0000" 
                 Placeholder="Enter phone number"
                 FloatLabelType="FloatLabelType.Never"
                 @bind-Value="@phone">
</SfMaskedTextBox>

<!-- Label stays above input field -->
```

### FloatLabelType.Auto

Label floats up when focused or when value exists:

```razor
<SfMaskedTextBox Mask="(000) 000-0000" 
                 Placeholder="Phone Number"
                 FloatLabelType="FloatLabelType.Auto"
                 @bind-Value="@phone">
</SfMaskedTextBox>

<!-- Empty & unfocused: "Phone Number" appears inside input -->
<!-- Focused or has value: "Phone Number" floats above as small label -->
```

### FloatLabelType.Always

Label always floats above, regardless of focus or value:

```razor
<SfMaskedTextBox Mask="000-00-0000" 
                 Placeholder="SSN"
                 FloatLabelType="FloatLabelType.Always"
                 @bind-Value="@ssn">
</SfMaskedTextBox>

<!-- "SSN" always displayed above input as floating label -->
```

### Form with Floating Labels

```razor
<div class="form-container">
    <div class="form-group">
        <SfMaskedTextBox Mask="(000) 000-0000" 
                         Placeholder="Phone Number"
                         FloatLabelType="FloatLabelType.Auto"
                         @bind-Value="@contact.Phone">
        </SfMaskedTextBox>
    </div>
    
    <div class="form-group">
        <SfMaskedTextBox Mask="000-00-0000" 
                         Placeholder="Social Security Number"
                         FloatLabelType="FloatLabelType.Auto"
                         @bind-Value="@contact.SSN">
        </SfMaskedTextBox>
    </div>
    
    <div class="form-group">
        <SfMaskedTextBox Mask="00/00/0000" 
                         Placeholder="Date of Birth"
                         FloatLabelType="FloatLabelType.Auto"
                         @bind-Value="@contact.DOB">
        </SfMaskedTextBox>
    </div>
</div>

<style>
    .form-container {
        max-width: 400px;
        padding: 20px;
    }
    
    .form-group {
        margin-bottom: 30px; /* Extra space for floating label */
    }
</style>

@code {
    private ContactInfo contact = new ContactInfo();
    
    public class ContactInfo
    {
        public string Phone { get; set; } = "";
        public string SSN { get; set; } = "";
        public string DOB { get; set; } = "";
    }
}
```

## ShowClearButton Functionality

The `ShowClearButton` property adds a clear icon to quickly remove the input value.

### Basic Usage

```razor
<SfMaskedTextBox Mask="(000) 000-0000" 
                 ShowClearButton="true"
                 Placeholder="Phone Number"
                 @bind-Value="@phone">
</SfMaskedTextBox>

<!-- Clear 'X' icon appears on the right when field has value -->
```

### With Floating Label

```razor
<SfMaskedTextBox Mask="000-00-0000" 
                 ShowClearButton="true"
                 FloatLabelType="FloatLabelType.Auto"
                 Placeholder="SSN"
                 @bind-Value="@ssn">
</SfMaskedTextBox>

<!-- Clear button appears when SSN is entered -->
```

### Multiple Inputs with Clear Buttons

```razor
<div class="clear-demo">
    <h3>Contact Form</h3>
    
    <div class="input-row">
        <label>Phone:</label>
        <SfMaskedTextBox Mask="(000) 000-0000" 
                         ShowClearButton="true"
                         @bind-Value="@phone">
        </SfMaskedTextBox>
    </div>
    
    <div class="input-row">
        <label>Fax:</label>
        <SfMaskedTextBox Mask="(000) 000-0000" 
                         ShowClearButton="true"
                         @bind-Value="@fax">
        </SfMaskedTextBox>
    </div>
    
    <div class="input-row">
        <label>ZIP:</label>
        <SfMaskedTextBox Mask="00000" 
                         ShowClearButton="true"
                         @bind-Value="@zip">
        </SfMaskedTextBox>
    </div>
</div>

@code {
    private string phone = "";
    private string fax = "";
    private string zip = "";
}
```

## CssClass Custom Styling

Use the `CssClass` property to apply custom CSS classes for styling.

### Basic Custom Styling

```razor
<SfMaskedTextBox Mask="000-00-0000" 
                 CssClass="custom-mask-input"
                 @bind-Value="@ssn">
</SfMaskedTextBox>

<style>
    .custom-mask-input {
        border: 2px solid #4CAF50 !important;
        border-radius: 8px !important;
    }
    
    .custom-mask-input:focus {
        border-color: #2196F3 !important;
        box-shadow: 0 0 8px rgba(33, 150, 243, 0.3) !important;
    }
</style>
```

### Size Variants

```razor
<div class="size-variants">
    <SfMaskedTextBox Mask="000-000" 
                     CssClass="input-small"
                     Placeholder="Small"
                     @bind-Value="@value1">
    </SfMaskedTextBox>
    
    <SfMaskedTextBox Mask="000-000" 
                     CssClass="input-medium"
                     Placeholder="Medium"
                     @bind-Value="@value2">
    </SfMaskedTextBox>
    
    <SfMaskedTextBox Mask="000-000" 
                     CssClass="input-large"
                     Placeholder="Large"
                     @bind-Value="@value3">
    </SfMaskedTextBox>
</div>

<style>
    .input-small .e-input { font-size: 12px; padding: 4px 8px; }
    .input-medium .e-input { font-size: 14px; padding: 8px 12px; }
    .input-large .e-input { font-size: 16px; padding: 12px 16px; }
</style>
```

### Theme-Specific Styling

```razor
<SfMaskedTextBox Mask="(000) 000-0000" 
                 CssClass="dark-theme-input"
                 @bind-Value="@phone">
</SfMaskedTextBox>

<style>
    .dark-theme-input {
        background-color: #2c2c2c !important;
        color: #ffffff !important;
        border-color: #555 !important;
    }
    
    .dark-theme-input .e-input::placeholder {
        color: #aaa !important;
    }
    
    .dark-theme-input .e-mask-prompt-char {
        color: #666 !important;
    }
</style>
```

### Status Indicators

```razor
<SfMaskedTextBox Mask="000-00-0000" 
                 CssClass="@GetValidationClass()"
                 @bind-Value="@ssn"
                 ValueChange="@OnSSNChanged">
</SfMaskedTextBox>

<style>
    .input-valid {
        border-color: #4CAF50 !important;
        background-color: #f1f8f4 !important;
    }
    
    .input-invalid {
        border-color: #f44336 !important;
        background-color: #fef1f0 !important;
    }
    
    .input-warning {
        border-color: #ff9800 !important;
        background-color: #fff8e1 !important;
    }
</style>

@code {
    private string ssn = "";
    private bool isValid = false;
    private bool isIncomplete = false;
    
    private void OnSSNChanged(MaskChangeEventArgs args)
    {
        isIncomplete = args.Value.Contains('_');
        isValid = !isIncomplete && ValidateSSN(args.Value);
    }
    
    private string GetValidationClass()
    {
        if (string.IsNullOrEmpty(ssn)) return "";
        if (isIncomplete) return "input-warning";
        return isValid ? "input-valid" : "input-invalid";
    }
    
    private bool ValidateSSN(string ssn) => true; // Simplified
}
```

## Width and Responsive Sizing

### Fixed Width

```razor
<SfMaskedTextBox Mask="(000) 000-0000" 
                 Width="300px"
                 @bind-Value="@phone">
</SfMaskedTextBox>
```

### Percentage Width

```razor
<SfMaskedTextBox Mask="000-00-0000" 
                 Width="100%"
                 @bind-Value="@ssn">
</SfMaskedTextBox>
```

### Responsive Grid Layout

```razor
<div class="responsive-form">
    <div class="row">
        <div class="col-md-6">
            <label>Phone:</label>
            <SfMaskedTextBox Mask="(000) 000-0000" 
                             Width="100%"
                             @bind-Value="@contact.Phone">
            </SfMaskedTextBox>
        </div>
        
        <div class="col-md-6">
            <label>Fax:</label>
            <SfMaskedTextBox Mask="(000) 000-0000" 
                             Width="100%"
                             @bind-Value="@contact.Fax">
            </SfMaskedTextBox>
        </div>
    </div>
    
    <div class="row">
        <div class="col-md-4">
            <label>SSN:</label>
            <SfMaskedTextBox Mask="000-00-0000" 
                             Width="100%"
                             @bind-Value="@contact.SSN">
            </SfMaskedTextBox>
        </div>
        
        <div class="col-md-4">
            <label>Date of Birth:</label>
            <SfMaskedTextBox Mask="00/00/0000" 
                             Width="100%"
                             @bind-Value="@contact.DOB">
            </SfMaskedTextBox>
        </div>
        
        <div class="col-md-4">
            <label>ZIP:</label>
            <SfMaskedTextBox Mask="00000" 
                             Width="100%"
                             @bind-Value="@contact.ZIP">
            </SfMaskedTextBox>
        </div>
    </div>
</div>

<style>
    .responsive-form {
        padding: 20px;
    }
    
    .row {
        display: flex;
        gap: 15px;
        margin-bottom: 20px;
    }
    
    .col-md-6 {
        flex: 0 0 calc(50% - 7.5px);
    }
    
    .col-md-4 {
        flex: 0 0 calc(33.333% - 10px);
    }
    
    @media (max-width: 768px) {
        .row {
            flex-direction: column;
        }
        
        .col-md-6, .col-md-4 {
            flex: 0 0 100%;
        }
    }
</style>
```

## Component States

### Enabled vs Disabled

```razor
<div class="state-demo">
    <label>Enabled:</label>
    <SfMaskedTextBox Mask="(000) 000-0000" 
                     Enabled="true"
                     @bind-Value="@phoneEnabled">
    </SfMaskedTextBox>
    
    <label>Disabled:</label>
    <SfMaskedTextBox Mask="(000) 000-0000" 
                     Enabled="false"
                     Value="(555) 123-4567">
    </SfMaskedTextBox>
</div>
```

### Readonly State

```razor
<SfMaskedTextBox Mask="000-00-0000" 
                 Readonly="true"
                 Value="123-45-6789"
                 CssClass="readonly-input">
</SfMaskedTextBox>

<style>
    .readonly-input {
        background-color: #f5f5f5 !important;
        cursor: default !important;
    }
</style>
```

### Conditional States

```razor
<div>
    <button @onclick="ToggleEdit">@(isEditable ? "Lock" : "Unlock")</button>
    
    <SfMaskedTextBox Mask="(000) 000-0000" 
                     Readonly="@(!isEditable)"
                     @bind-Value="@phone">
    </SfMaskedTextBox>
</div>

@code {
    private bool isEditable = false;
    private string phone = "";
    
    private void ToggleEdit()
    {
        isEditable = !isEditable;
    }
}
```

## Theme Customization

### Available Themes

```html
<!-- Material 3 (default) -->
<link href="_content/Syncfusion.Blazor.Themes/material3.css" rel="stylesheet" />

<!-- Bootstrap 5 -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Fluent -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />

<!-- Tailwind -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />

<!-- Material (legacy) -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Fabric -->
<link href="_content/Syncfusion.Blazor.Themes/fabric.css" rel="stylesheet" />
```

### Dark Mode Themes

```html
<!-- Material 3 Dark -->
<link href="_content/Syncfusion.Blazor.Themes/material3-dark.css" rel="stylesheet" />

<!-- Bootstrap 5 Dark -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5-dark.css" rel="stylesheet" />

<!-- Fluent Dark -->
<link href="_content/Syncfusion.Blazor.Themes/fluent-dark.css" rel="stylesheet" />

<!-- Tailwind Dark -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind-dark.css" rel="stylesheet" />
```

### Custom Theme with CSS Variables

```razor
<SfMaskedTextBox Mask="(000) 000-0000" 
                 CssClass="custom-theme-input"
                 @bind-Value="@phone">
</SfMaskedTextBox>

<style>
    .custom-theme-input {
        --input-bg: #ffffff;
        --input-border: #cccccc;
        --input-focus-border: #007bff;
        --input-text: #333333;
        --input-placeholder: #999999;
    }
    
    .custom-theme-input .e-input-group {
        background-color: var(--input-bg);
        border-color: var(--input-border);
        color: var(--input-text);
    }
    
    .custom-theme-input .e-input-group.e-input-focus {
        border-color: var(--input-focus-border);
        box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
    }
    
    .custom-theme-input .e-input::placeholder {
        color: var(--input-placeholder);
    }
</style>
```

## WCAG 2.1 Compliance

### Level AA Requirements

The InputMask component meets WCAG 2.1 Level AA standards:

- ✅ **Keyboard accessibility** - Full keyboard navigation
- ✅ **Focus indicators** - Clear visual focus states
- ✅ **Color contrast** - Text meets 4.5:1 ratio
- ✅ **Screen reader support** - ARIA labels and roles
- ✅ **Error identification** - Accessible validation messages

### Accessible Form Pattern

```razor
<div class="accessible-form">
    <div class="form-group" role="group" aria-labelledby="phone-label">
        <label id="phone-label" for="phone-input">
            Phone Number <span class="required" aria-label="required">*</span>
        </label>
        <SfMaskedTextBox ID="phone-input"
                         Mask="(000) 000-0000" 
                         Placeholder="Enter your phone number"
                         @bind-Value="@phone"
                         ValueChange="@OnPhoneChanged">
        </SfMaskedTextBox>
        
        @if (showPhoneError)
        {
            <div class="error-message" role="alert" aria-live="polite">
                @phoneErrorMessage
            </div>
        }
        
        <small class="form-text" id="phone-help">
            Format: (###) ###-####
        </small>
    </div>
</div>

<style>
    .required {
        color: #d32f2f;
        font-weight: bold;
    }
    
    .error-message {
        color: #d32f2f;
        font-size: 14px;
        margin-top: 5px;
    }
    
    .form-text {
        color: #666;
        font-size: 12px;
        display: block;
        margin-top: 5px;
    }
</style>

@code {
    private string phone = "";
    private bool showPhoneError = false;
    private string phoneErrorMessage = "";
    
    private void OnPhoneChanged(MaskChangeEventArgs args)
    {
        if (args.Value.Contains('_'))
        {
            showPhoneError = true;
            phoneErrorMessage = "Please complete the phone number";
        }
        else
        {
            showPhoneError = false;
        }
    }
}
```

## Keyboard Navigation

### Supported Keyboard Shortcuts

| Key | Action |
|-----|--------|
| **Tab** | Move focus to next field |
| **Shift + Tab** | Move focus to previous field |
| **Arrow Left** | Move cursor left |
| **Arrow Right** | Move cursor right |
| **Home** | Move to first position |
| **End** | Move to last position |
| **Backspace** | Delete character before cursor |
| **Delete** | Delete character at cursor |
| **Ctrl + A** | Select all |
| **Ctrl + C** | Copy selected text |
| **Ctrl + V** | Paste text |
| **Ctrl + X** | Cut selected text |
| **Esc** | Clear focus (when ShowClearButton is true) |

### Custom Keyboard Handling

```razor
<SfMaskedTextBox Mask="000-00-0000" 
                 @bind-Value="@ssn"
                 @onkeydown="@HandleKeyDown">
</SfMaskedTextBox>

@code {
    private string ssn = "";
    
    private void HandleKeyDown(KeyboardEventArgs e)
    {
        if (e.Key == "Enter" && !ssn.Contains('_'))
        {
            // Submit form or move to next field
            SubmitForm();
        }
        else if (e.Key == "Escape")
        {
            // Clear field
            ssn = "";
        }
    }
    
    private void SubmitForm()
    {
        Console.WriteLine("Form submitted");
    }
}
```

## ARIA Attributes

### Built-in ARIA Support

The component automatically includes:
- `role="textbox"` - Identifies as text input
- `aria-label` - Accessible name from Placeholder
- `aria-required` - Required state (when in required field)
- `aria-invalid` - Invalid state (when validation fails)
- `aria-describedby` - Associates with help text

### Custom ARIA Labels

```razor
<SfMaskedTextBox Mask="(000) 000-0000" 
                 @bind-Value="@phone"
                 HtmlAttributes="@phoneAttributes">
</SfMaskedTextBox>

@code {
    private string phone = "";
    
    private Dictionary<string, object> phoneAttributes = new Dictionary<string, object>
    {
        { "aria-label", "Phone number input" },
        { "aria-required", "true" },
        { "aria-describedby", "phone-help-text" }
    };
}
```

## Screen Reader Support

### Descriptive Labels

```razor
<div class="form-group">
    <label for="ssn-input" class="visually-hidden">
        Social Security Number (9 digits)
    </label>
    <SfMaskedTextBox ID="ssn-input"
                     Mask="000-00-0000" 
                     Placeholder="SSN"
                     @bind-Value="@ssn">
    </SfMaskedTextBox>
    <span id="ssn-description" class="sr-only">
        Format: three digits, dash, two digits, dash, four digits
    </span>
</div>

<style>
    .visually-hidden, .sr-only {
        position: absolute;
        width: 1px;
        height: 1px;
        padding: 0;
        margin: -1px;
        overflow: hidden;
        clip: rect(0, 0, 0, 0);
        white-space: nowrap;
        border-width: 0;
    }
</style>
```

### Live Regions for Validation

```razor
<SfMaskedTextBox Mask="0000-0000-0000-0000" 
                 @bind-Value="@cardNumber"
                 ValueChange="@OnCardChanged">
</SfMaskedTextBox>

<div role="status" aria-live="polite" aria-atomic="true">
    @if (!string.IsNullOrEmpty(validationMessage))
    {
        <span>@validationMessage</span>
    }
</div>

@code {
    private string cardNumber = "";
    private string validationMessage = "";
    
    private void OnCardChanged(MaskChangeEventArgs args)
    {
        if (args.Value.Contains('_'))
        {
            validationMessage = "Credit card number is incomplete";
        }
        else
        {
            validationMessage = "Credit card number is valid";
        }
    }
}
```

## RTL Support

### Enable Right-to-Left Layout

```razor
<SfMaskedTextBox Mask="000-000-0000" 
                 EnableRtl="true"
                 Placeholder="رقم الهاتف"
                 @bind-Value="@phone">
</SfMaskedTextBox>

<!-- Arabic/Hebrew layout with right-aligned text -->
```

### RTL with Floating Labels

```razor
<SfMaskedTextBox Mask="000-00-0000" 
                 EnableRtl="true"
                 FloatLabelType="FloatLabelType.Auto"
                 Placeholder="رقم الضمان الاجتماعي"
                 @bind-Value="@ssn">
</SfMaskedTextBox>
```

## HtmlAttributes and InputAttributes

### HtmlAttributes (Wrapper Element)

```razor
<SfMaskedTextBox Mask="(000) 000-0000" 
                 @bind-Value="@phone"
                 HtmlAttributes="@wrapperAttributes">
</SfMaskedTextBox>

@code {
    private string phone = "";
    
    private Dictionary<string, object> wrapperAttributes = new Dictionary<string, object>
    {
        { "data-test-id", "phone-input" },
        { "class", "custom-wrapper" },
        { "style", "margin: 10px 0;" }
    };
}
```

### InputAttributes (Input Element)

```razor
<SfMaskedTextBox Mask="000-00-0000" 
                 @bind-Value="@ssn"
                 InputAttributes="@inputAttributes">
</SfMaskedTextBox>

@code {
    private string ssn = "";
    
    private Dictionary<string, object> inputAttributes = new Dictionary<string, object>
    {
        { "autocomplete", "off" },
        { "spellcheck", "false" },
        { "data-lpignore", "true" }, // LastPass ignore
        { "aria-label", "Social Security Number" }
    };
}
```

### Combined Attributes Example

```razor
<SfMaskedTextBox Mask="0000-0000-0000-0000" 
                 @bind-Value="@cardNumber"
                 HtmlAttributes="@htmlAttrs"
                 InputAttributes="@inputAttrs">
</SfMaskedTextBox>

@code {
    private string cardNumber = "";
    
    private Dictionary<string, object> htmlAttrs = new Dictionary<string, object>
    {
        { "class", "payment-field" },
        { "data-field-type", "credit-card" }
    };
    
    private Dictionary<string, object> inputAttrs = new Dictionary<string, object>
    {
        { "autocomplete", "cc-number" },
        { "inputmode", "numeric" },
        { "aria-label", "Credit card number" },
        { "aria-required", "true" }
    };
}
```

## Complete Accessibility Example

```razor
@page "/accessible-mask-form"
@using Syncfusion.Blazor.Inputs

<div class="accessible-form" role="main" aria-labelledby="form-title">
    <h1 id="form-title">Contact Information</h1>
    
    <EditForm Model="@model" OnValidSubmit="@HandleSubmit">
        <DataAnnotationsValidator />
        
        <!-- Phone Number -->
        <div class="form-group" role="group" aria-labelledby="phone-label">
            <label id="phone-label" for="phone-input">
                Phone Number <abbr title="required" aria-label="required">*</abbr>
            </label>
            <SfMaskedTextBox ID="phone-input"
                             Mask="(000) 000-0000" 
                             Placeholder="(555) 123-4567"
                             FloatLabelType="FloatLabelType.Never"
                             ShowClearButton="true"
                             @bind-Value="@model.Phone"
                             ValueExpression="@(() => model.Phone)"
                             InputAttributes="@phoneInputAttrs">
            </SfMaskedTextBox>
            <small id="phone-help" class="form-text">Format: (###) ###-####</small>
            <ValidationMessage For="@(() => model.Phone)" />
        </div>
        
        <!-- SSN -->
        <div class="form-group" role="group" aria-labelledby="ssn-label">
            <label id="ssn-label" for="ssn-input">
                Social Security Number <abbr title="required" aria-label="required">*</abbr>
            </label>
            <SfMaskedTextBox ID="ssn-input"
                             Mask="000-00-0000" 
                             Placeholder="###-##-####"
                             ShowClearButton="true"
                             @bind-Value="@model.SSN"
                             ValueExpression="@(() => model.SSN)"
                             InputAttributes="@ssnInputAttrs">
            </SfMaskedTextBox>
            <small id="ssn-help" class="form-text">Format: ###-##-####</small>
            <ValidationMessage For="@(() => model.SSN)" />
        </div>
        
        <button type="submit" class="btn btn-primary">
            Submit <span class="sr-only">contact information form</span>
        </button>
    </EditForm>
</div>

<style>
    .accessible-form {
        max-width: 600px;
        margin: 20px auto;
        padding: 20px;
    }
    
    .form-group {
        margin-bottom: 25px;
    }
    
    .form-group label {
        display: block;
        margin-bottom: 8px;
        font-weight: 500;
    }
    
    .form-group abbr {
        color: #d32f2f;
        text-decoration: none;
    }
    
    .form-text {
        display: block;
        margin-top: 5px;
        color: #666;
        font-size: 14px;
    }
    
    .validation-message {
        color: #d32f2f;
        font-size: 14px;
        margin-top: 5px;
        display: block;
    }
    
    .sr-only {
        position: absolute;
        width: 1px;
        height: 1px;
        padding: 0;
        margin: -1px;
        overflow: hidden;
        clip: rect(0, 0, 0, 0);
        white-space: nowrap;
        border-width: 0;
    }
    
    .btn {
        padding: 10px 20px;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        font-size: 16px;
    }
    
    .btn-primary {
        background-color: #007bff;
        color: white;
    }
    
    .btn-primary:hover {
        background-color: #0056b3;
    }
    
    .btn-primary:focus {
        outline: 3px solid #80bdff;
        outline-offset: 2px;
    }
</style>

@code {
    private ContactFormModel model = new ContactFormModel();
    
    private Dictionary<string, object> phoneInputAttrs = new Dictionary<string, object>
    {
        { "aria-describedby", "phone-help" },
        { "autocomplete", "tel" },
        { "inputmode", "numeric" }
    };
    
    private Dictionary<string, object> ssnInputAttrs = new Dictionary<string, object>
    {
        { "aria-describedby", "ssn-help" },
        { "autocomplete", "off" },
        { "inputmode", "numeric" }
    };
    
    private void HandleSubmit()
    {
        Console.WriteLine($"Phone: {model.Phone}, SSN: {model.SSN}");
    }
    
    public class ContactFormModel
    {
        [Required(ErrorMessage = "Phone number is required")]
        public string Phone { get; set; }
        
        [Required(ErrorMessage = "SSN is required")]
        public string SSN { get; set; }
    }
}
```

## Best Practices Summary

1. **Use FloatLabelType.Auto** for modern, clean forms
2. **Add ShowClearButton** for better UX on data entry fields
3. **Apply CssClass** for consistent branding
4. **Set Width="100%"** for responsive forms
5. **Provide clear labels** with proper ARIA attributes
6. **Use role and aria-live** for dynamic validation messages
7. **Enable RTL** for right-to-left languages
8. **Test keyboard navigation** thoroughly
9. **Ensure color contrast** meets WCAG AA standards (4.5:1)
10. **Add descriptive help text** for complex mask patterns
