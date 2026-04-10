# OtpInput - Configuration Options

## Table of Contents
- [Core Properties Overview](#core-properties-overview)
- [Length Property](#length-property)
- [Type Property](#type-property)
- [Placeholder Configuration](#placeholder-configuration)
- [Separator for Visual Grouping](#separator-for-visual-grouping)
- [AutoFocus for Immediate Input](#autofocus-for-immediate-input)
- [Disabled State Management](#disabled-state-management)
- [ID and HtmlAttributes](#id-and-htmlattributes)
- [Configuration Examples](#configuration-examples)

---

## Core Properties Overview

The SfOtpInput component provides several configuration properties to customize behavior and appearance:

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| Length | int | 4 | Number of OTP input fields |
| Type | OtpInputType | Number | Input type (Number, Text, Password) |
| Placeholder | string | "" | Placeholder text for empty fields |
| Separator | string | "" | Character(s) displayed between input groups |
| AutoFocus | bool | false | Auto-focus first input on component load |
| Disabled | bool | false | Disable all input fields |
| ID | string | auto-generated | Custom ID for the component |
| HtmlAttributes | Dictionary | null | Custom HTML attributes |
| CssClass | string | "" | Custom CSS classes |

---

## Length Property

The `Length` property defines how many individual input fields are displayed.

### Default Length (4 digits)

```razor
<SfOtpInput @bind-Value="@otpValue"></SfOtpInput>

@code {
    private string otpValue = ""; // Can hold up to 4 characters
}
```

### Common Length Configurations

**SMS Verification (4 digits):**
```razor
<SfOtpInput @bind-Value="@smsCode" Length="4"></SfOtpInput>
```

**Email/Authenticator App (6 digits):**
```razor
<SfOtpInput @bind-Value="@emailCode" Length="6"></SfOtpInput>
```

**High-Security PIN (8 digits):**
```razor
<SfOtpInput @bind-Value="@securityPin" Length="8"></SfOtpInput>
```

**Custom Length Example:**
```razor
@page "/otp-length-demo"
@using Syncfusion.Blazor.Inputs

<div class="demo-container">
    <h3>OTP Length Configurations</h3>
    
    <div class="length-selector">
        <label>Select OTP Length:</label>
        <select @bind="selectedLength">
            <option value="4">4 Digits (SMS)</option>
            <option value="6">6 Digits (Email/2FA)</option>
            <option value="8">8 Digits (High Security)</option>
        </select>
    </div>
    
    <div class="otp-display">
        <SfOtpInput @bind-Value="@otpValue" Length="@selectedLength"></SfOtpInput>
    </div>
    
    <p>Current Value: @otpValue (Length: @otpValue.Length/@selectedLength)</p>
</div>

@code {
    private string otpValue = "";
    private int selectedLength = 6;
}
```

**Important Notes:**
- Length can range from 1 to any reasonable number (typically 4-8)
- The `Value` string length will match the number of filled inputs
- Changing Length dynamically will reset the Value

---

## Type Property

The `Type` property determines what kind of input is accepted in each field.

### OtpInputType Enum Values

| Value | Description | Use Case |
|-------|-------------|----------|
| **Number** | Numeric digits only (0-9) | Standard OTP codes, PINs |
| **Text** | Alphanumeric characters | Activation codes, mixed codes |
| **Password** | Masked input (bullets/asterisks) | Secure PIN entry, sensitive codes |

### Number Type (Default)

Best for standard numeric OTP codes:

```razor
<SfOtpInput @bind-Value="@numericOtp"
            Length="6"
            Type="OtpInputType.Number">
</SfOtpInput>

@code {
    private string numericOtp = ""; // Accepts only: 0-9
}
```

**Behavior:**
- Only numeric keys (0-9) are accepted
- Non-numeric input is ignored
- Mobile keyboards show numeric keypad
- Ideal for SMS codes, authenticator apps

### Text Type

For alphanumeric codes:

```razor
<SfOtpInput @bind-Value="@activationCode"
            Length="8"
            Type="OtpInputType.Text">
</SfOtpInput>

@code {
    private string activationCode = ""; // Accepts: A-Z, a-z, 0-9
}
```

**Behavior:**
- Accepts letters and numbers
- Case-sensitive by default
- Use with `TextTransform` for uppercase/lowercase
- Perfect for license keys, activation codes

### Password Type

For secure, masked input:

```razor
<SfOtpInput @bind-Value="@securePin"
            Length="4"
            Type="OtpInputType.Password">
</SfOtpInput>

@code {
    private string securePin = ""; // Input is masked
}
```

**Behavior:**
- Input is hidden (displayed as • or *)
- Value is still accessible in code
- Provides visual security
- Use for PINs, secure transaction codes

### Type Comparison Example

```razor
@page "/otp-types"
@using Syncfusion.Blazor.Inputs

<div class="types-demo">
    <h3>OTP Input Types</h3>
    
    <div class="type-section">
        <h4>Number Type</h4>
        <p class="description">Numeric only (0-9)</p>
        <SfOtpInput @bind-Value="@numericCode"
                    Length="6"
                    Type="OtpInputType.Number">
        </SfOtpInput>
        <p>Value: @numericCode</p>
    </div>
    
    <div class="type-section">
        <h4>Text Type</h4>
        <p class="description">Alphanumeric (A-Z, 0-9)</p>
        <SfOtpInput @bind-Value="@textCode"
                    Length="6"
                    Type="OtpInputType.Text">
        </SfOtpInput>
        <p>Value: @textCode</p>
    </div>
    
    <div class="type-section">
        <h4>Password Type</h4>
        <p class="description">Masked input for security</p>
        <SfOtpInput @bind-Value="@passwordCode"
                    Length="4"
                    Type="OtpInputType.Password">
        </SfOtpInput>
        <p>Value: @passwordCode (hidden in UI)</p>
    </div>
</div>

@code {
    private string numericCode = "";
    private string textCode = "";
    private string passwordCode = "";
}

<style>
    .type-section {
        margin: 30px 0;
        padding: 20px;
        border: 1px solid #ddd;
        border-radius: 8px;
    }
    
    .description {
        color: #666;
        font-size: 14px;
        margin-bottom: 15px;
    }
</style>
```

---

## Placeholder Configuration

The `Placeholder` property sets hint text displayed in empty input fields.

### Basic Placeholder

```razor
<SfOtpInput @bind-Value="@otpValue"
            Length="6"
            Placeholder="0">
</SfOtpInput>

@code {
    private string otpValue = "";
}
```

**Result:** Each empty field shows "0" as a hint.

### Common Placeholder Patterns

**Numeric Hint:**
```razor
<SfOtpInput Placeholder="0"></SfOtpInput>
```

**Dash/Underscore:**
```razor
<SfOtpInput Placeholder="-"></SfOtpInput>
<SfOtpInput Placeholder="_"></SfOtpInput>
```

**Letter Hint (for text type):**
```razor
<SfOtpInput Type="OtpInputType.Text" Placeholder="X"></SfOtpInput>
```

**No Placeholder (empty fields):**
```razor
<SfOtpInput Placeholder=""></SfOtpInput>
```

### Placeholder Example

```razor
@page "/otp-placeholder"
@using Syncfusion.Blazor.Inputs

<div class="placeholder-demo">
    <div class="example">
        <label>No Placeholder</label>
        <SfOtpInput @bind-Value="@code1" Length="4"></SfOtpInput>
    </div>
    
    <div class="example">
        <label>Numeric Placeholder (0)</label>
        <SfOtpInput @bind-Value="@code2" Length="4" Placeholder="0"></SfOtpInput>
    </div>
    
    <div class="example">
        <label>Dash Placeholder (-)</label>
        <SfOtpInput @bind-Value="@code3" Length="4" Placeholder="-"></SfOtpInput>
    </div>
    
    <div class="example">
        <label>Letter Placeholder (X)</label>
        <SfOtpInput @bind-Value="@code4" Length="4" Type="OtpInputType.Text" Placeholder="X"></SfOtpInput>
    </div>
</div>

@code {
    private string code1 = "";
    private string code2 = "";
    private string code3 = "";
    private string code4 = "";
}
```

**Best Practices:**
- Use single character placeholders for clarity
- Match placeholder to expected input (0 for numbers, X for text)
- Consider accessibility - some users rely on placeholders for context

---

## Separator for Visual Grouping

The `Separator` property adds a visual divider between input groups.

### Basic Separator

```razor
<SfOtpInput @bind-Value="@otpValue"
            Length="6"
            Separator="-">
</SfOtpInput>

@code {
    private string otpValue = "";
}
```

**Visual Result:** `[1][2][3] - [4][5][6]`

### Common Separator Patterns

**Hyphen (most common):**
```razor
<SfOtpInput Length="6" Separator="-"></SfOtpInput>
```
**Result:** `123-456`

**Space:**
```razor
<SfOtpInput Length="6" Separator=" "></SfOtpInput>
```
**Result:** `123 456`

**Multiple Characters:**
```razor
<SfOtpInput Length="9" Separator=" - "></SfOtpInput>
```
**Result:** `123 - 456 - 789`

**No Separator:**
```razor
<SfOtpInput Length="6" Separator=""></SfOtpInput>
```
**Result:** `123456`

### Separator Placement

The separator typically appears after every 3rd digit for 6-digit codes:

```razor
@page "/otp-separator"
@using Syncfusion.Blazor.Inputs

<div class="separator-demo">
    <h3>Separator Examples</h3>
    
    <div class="example">
        <label>6-Digit with Hyphen: XXX-XXX</label>
        <SfOtpInput @bind-Value="@code1" Length="6" Separator="-"></SfOtpInput>
        <p>Value: @code1</p>
    </div>
    
    <div class="example">
        <label>8-Digit with Space: XXXX XXXX</label>
        <SfOtpInput @bind-Value="@code2" Length="8" Separator=" "></SfOtpInput>
        <p>Value: @code2</p>
    </div>
    
    <div class="example">
        <label>9-Digit License Key: XXX-XXX-XXX</label>
        <SfOtpInput @bind-Value="@code3" Length="9" Separator="-" Type="OtpInputType.Text"></SfOtpInput>
        <p>Value: @code3</p>
    </div>
</div>

@code {
    private string code1 = "";
    private string code2 = "";
    private string code3 = "";
}
```

**Important Notes:**
- Separator is visual only - not included in `Value` string
- Improves readability for longer codes
- Automatically positioned between input groups
- Works with all Type options

---

## AutoFocus for Immediate Input

The `AutoFocus` property automatically focuses the first input field when the component loads.

### Enable AutoFocus

```razor
<SfOtpInput @bind-Value="@otpValue"
            Length="6"
            AutoFocus="true">
</SfOtpInput>

@code {
    private string otpValue = "";
}
```

**Behavior:**
- First input field receives focus on component render
- User can start typing immediately without clicking
- Improves UX for verification flows

### AutoFocus Use Cases

**Single OTP Input on Page:**
```razor
@page "/verify-email"
@using Syncfusion.Blazor.Inputs

<div class="verification-page">
    <h2>Verify Your Email</h2>
    <p>Enter the 6-digit code we sent to your email</p>
    
    <SfOtpInput @bind-Value="@verificationCode"
                Length="6"
                AutoFocus="true"
                Type="OtpInputType.Number">
    </SfOtpInput>
</div>

@code {
    private string verificationCode = "";
}
```

**Multiple OTP Inputs - Conditional AutoFocus:**
```razor
@page "/multi-step-verify"
@using Syncfusion.Blazor.Inputs

<div class="multi-step">
    @if (currentStep == 1)
    {
        <h3>Step 1: Email Verification</h3>
        <SfOtpInput @bind-Value="@emailCode"
                    Length="6"
                    AutoFocus="true">
        </SfOtpInput>
        <button @onclick="NextStep">Next</button>
    }
    else if (currentStep == 2)
    {
        <h3>Step 2: Phone Verification</h3>
        <SfOtpInput @bind-Value="@phoneCode"
                    Length="4"
                    AutoFocus="true">
        </SfOtpInput>
        <button @onclick="Submit">Submit</button>
    }
</div>

@code {
    private int currentStep = 1;
    private string emailCode = "";
    private string phoneCode = "";
    
    private void NextStep() => currentStep++;
    private void Submit() { /* Verify both codes */ }
}
```

**Best Practices:**
- Enable AutoFocus when OTP is the primary action
- Avoid on pages with multiple focus-critical elements
- Consider mobile keyboards - auto-focus may show keyboard immediately
- Disable if OTP input is below the fold (requires scrolling)

---

## Disabled State Management

The `Disabled` property prevents user interaction with all input fields.

### Enable Disabled State

```razor
<SfOtpInput @bind-Value="@otpValue"
            Length="6"
            Disabled="true">
</SfOtpInput>

@code {
    private string otpValue = "";
}
```

**Behavior:**
- All input fields become non-interactive
- Grayed-out appearance (theme-dependent)
- Focus, keyboard input, and mouse clicks ignored
- Value can still be set programmatically

### Conditional Disabling

**During Verification:**
```razor
@page "/otp-verify-disable"
@using Syncfusion.Blazor.Inputs

<div class="verify-container">
    <label>Enter Verification Code:</label>
    <SfOtpInput @bind-Value="@otpValue"
                Length="6"
                Disabled="@isVerifying">
    </SfOtpInput>
    
    <button @onclick="Verify" disabled="@(otpValue.Length < 6 || isVerifying)">
        @(isVerifying ? "Verifying..." : "Verify Code")
    </button>
    
    @if (isVerified)
    {
        <p class="success">✓ Verification successful!</p>
    }
</div>

@code {
    private string otpValue = "";
    private bool isVerifying = false;
    private bool isVerified = false;
    
    private async Task Verify()
    {
        isVerifying = true;
        StateHasChanged();
        
        await Task.Delay(2000); // Simulate API call
        
        isVerified = true;
        isVerifying = false;
    }
}
```

**After Successful Verification:**
```razor
<SfOtpInput @bind-Value="@verifiedCode"
            Length="6"
            Disabled="@isVerified">
</SfOtpInput>

@if (isVerified)
{
    <p class="success">✓ Code verified and locked</p>
}

@code {
    private string verifiedCode = "123456";
    private bool isVerified = true;
}
```

**Rate Limiting (After Failed Attempts):**
```razor
@page "/otp-rate-limit"
@using Syncfusion.Blazor.Inputs

<div class="rate-limit-demo">
    <SfOtpInput @bind-Value="@otpValue"
                Length="6"
                Disabled="@isLocked">
    </SfOtpInput>
    
    <button @onclick="Verify" disabled="@(otpValue.Length < 6 || isLocked)">
        Verify
    </button>
    
    @if (isLocked)
    {
        <p class="error">Too many failed attempts. Locked for @remainingSeconds seconds.</p>
    }
    
    <p>Failed Attempts: @failedAttempts / 3</p>
</div>

@code {
    private string otpValue = "";
    private int failedAttempts = 0;
    private bool isLocked = false;
    private int remainingSeconds = 0;
    
    private async Task Verify()
    {
        if (otpValue != "123456") // Replace with actual verification
        {
            failedAttempts++;
            otpValue = "";
            
            if (failedAttempts >= 3)
            {
                isLocked = true;
                await LockForDuration(30); // 30 seconds lockout
            }
        }
        else
        {
            // Success
            failedAttempts = 0;
        }
    }
    
    private async Task LockForDuration(int seconds)
    {
        remainingSeconds = seconds;
        
        while (remainingSeconds > 0)
        {
            await Task.Delay(1000);
            remainingSeconds--;
            StateHasChanged();
        }
        
        isLocked = false;
        failedAttempts = 0;
    }
}
```

---

## ID and HtmlAttributes

### Custom ID

Set a custom ID for the OtpInput component:

```razor
<SfOtpInput @bind-Value="@otpValue"
            ID="email-verification-otp"
            Length="6">
</SfOtpInput>

@code {
    private string otpValue = "";
}
```

**Use Cases:**
- CSS targeting for specific styling
- JavaScript interop scenarios
- Automated testing selectors
- Accessibility associations

### HtmlAttributes

Add custom HTML attributes to the wrapper element:

```razor
<SfOtpInput @bind-Value="@otpValue"
            Length="6"
            HtmlAttributes="@customAttributes">
</SfOtpInput>

@code {
    private string otpValue = "";
    
    private Dictionary<string, object> customAttributes = new Dictionary<string, object>
    {
        { "data-testid", "otp-input" },
        { "data-flow", "email-verification" },
        { "aria-describedby", "otp-help-text" }
    };
}
```

**Common Attributes:**
```csharp
// Testing
{ "data-testid", "otp-verification" }

// Analytics
{ "data-track-element", "otp-input" }
{ "data-track-location", "signup-flow" }

// Accessibility
{ "aria-describedby", "otp-instructions" }
{ "aria-label", "Email verification code" }

// Custom data
{ "data-user-id", userId }
{ "data-session", sessionId }
```

---

## Configuration Examples

### Complete Configuration Example

```razor
@page "/otp-complete-config"
@using Syncfusion.Blazor.Inputs

<div class="config-example">
    <h3>Two-Factor Authentication</h3>
    
    <p id="otp-instructions" class="instructions">
        Enter the 6-digit code from your authenticator app
    </p>
    
    <SfOtpInput @bind-Value="@twoFactorCode"
                Length="6"
                Type="OtpInputType.Number"
                Placeholder="0"
                Separator="-"
                AutoFocus="true"
                Disabled="@isProcessing"
                ID="two-factor-otp"
                CssClass="custom-otp"
                HtmlAttributes="@otpAttributes">
    </SfOtpInput>
    
    <button class="btn-primary" 
            @onclick="Authenticate" 
            disabled="@(twoFactorCode.Length < 6 || isProcessing)">
        @(isProcessing ? "Authenticating..." : "Authenticate")
    </button>
    
    @if (!string.IsNullOrEmpty(statusMessage))
    {
        <div class="status @statusClass">@statusMessage</div>
    }
</div>

@code {
    private string twoFactorCode = "";
    private bool isProcessing = false;
    private string statusMessage = "";
    private string statusClass = "";
    
    private Dictionary<string, object> otpAttributes = new Dictionary<string, object>
    {
        { "aria-describedby", "otp-instructions" },
        { "data-testid", "2fa-otp-input" }
    };
    
    private async Task Authenticate()
    {
        isProcessing = true;
        statusMessage = "Verifying code...";
        statusClass = "info";
        StateHasChanged();
        
        await Task.Delay(1500); // Simulate API call
        
        // Replace with actual 2FA verification
        bool isValid = await Verify2FACode(twoFactorCode);
        
        if (isValid)
        {
            statusMessage = "✓ Authentication successful!";
            statusClass = "success";
            // Redirect to dashboard
        }
        else
        {
            statusMessage = "✗ Invalid code. Please try again.";
            statusClass = "error";
            twoFactorCode = "";
        }
        
        isProcessing = false;
    }
    
    private async Task<bool> Verify2FACode(string code)
    {
        // Replace with actual 2FA verification logic
        return code == "123456";
    }
}

<style>
    .config-example {
        max-width: 450px;
        margin: 40px auto;
        padding: 30px;
        border-radius: 10px;
        box-shadow: 0 4px 12px rgba(0,0,0,0.15);
    }
    
    .instructions {
        color: #666;
        margin-bottom: 25px;
        font-size: 14px;
    }
    
    .custom-otp {
        margin: 20px 0;
    }
    
    .btn-primary {
        width: 100%;
        padding: 12px;
        margin-top: 20px;
        font-size: 16px;
        font-weight: 600;
        border: none;
        border-radius: 6px;
        cursor: pointer;
        background-color: #0d6efd;
        color: white;
    }
    
    .btn-primary:disabled {
        background-color: #ccc;
        cursor: not-allowed;
    }
    
    .status {
        margin-top: 15px;
        padding: 12px;
        border-radius: 6px;
        font-weight: 500;
    }
    
    .status.success { background-color: #d1e7dd; color: #0f5132; }
    .status.error { background-color: #f8d7da; color: #842029; }
    .status.info { background-color: #cff4fc; color: #055160; }
</style>
```

This comprehensive configuration example demonstrates all major properties working together in a real-world 2FA scenario.
