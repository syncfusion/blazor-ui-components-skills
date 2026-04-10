# OtpInput - Getting Started

## Overview

The Syncfusion Blazor OtpInput (One-Time Password Input) component provides a specialized input interface for entering verification codes, authentication codes, PINs, and other multi-digit secure inputs. It automatically manages focus, supports keyboard navigation, and offers multiple styling options for seamless integration into authentication and verification workflows.

**Key Features:**
- Configurable digit length (typically 4-8 digits)
- Multiple input types: Number, Text, Password
- Automatic focus management between inputs
- Keyboard navigation (arrows, backspace, delete)
- Three visual styles: Outlined, Underlined, Filled
- Built-in accessibility support
- Mobile-friendly touch interactions
- Two-way data binding with Value property

**Common Use Cases:**
- Two-factor authentication (2FA)
- Email/SMS verification codes
- PIN entry for secure transactions
- Password reset verification
- Account activation codes
- Device pairing codes

---

## Installation and NuGet Package Setup

### Step 1: Install Syncfusion.Blazor.Inputs Package

**Via NuGet Package Manager:**
```bash
dotnet add package Syncfusion.Blazor.Inputs
```

**Via Package Manager Console:**
```powershell
Install-Package Syncfusion.Blazor.Inputs
```

**Via Visual Studio:**
1. Right-click project → Manage NuGet Packages
2. Search for "Syncfusion.Blazor.Inputs"
3. Click Install

### Step 2: Register Syncfusion Blazor Service

**For Blazor Server / Blazor Web App (.NET 6+):**

In `Program.cs`:
```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// Add Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

// Rest of configuration...
var app = builder.Build();
```

**For Blazor WebAssembly:**

In `Program.cs`:
```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);

// Add Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

---

## CSS Theme Configuration

### Step 3: Import Syncfusion CSS

**Option 1: Via CDN (Recommended for quick start)**

Add to `wwwroot/index.html` (Blazor WebAssembly) or `Pages/_Host.cshtml` / `Pages/_Layout.cshtml` (Blazor Server):

```html
<head>
    <!-- Syncfusion Blazor Theme -->
    <link href="https://cdn.syncfusion.com/blazor/24.1.41/styles/bootstrap5.css" rel="stylesheet" />
</head>
```

**Available Themes:**
- `bootstrap5.css` - Bootstrap 5 theme
- `material.css` - Material Design
- `fabric.css` - Microsoft Fabric
- `tailwind.css` - Tailwind CSS
- `fluent.css` - Microsoft Fluent
- `bootstrap5-dark.css` - Bootstrap 5 Dark
- `material-dark.css` - Material Dark

**Option 2: Via Static Assets (For offline/production)**

Add to the same location:
```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
```

**Option 3: Individual Component CSS**

For reduced bundle size:
```html
<head>
    <link href="_content/Syncfusion.Blazor.Inputs/styles/bootstrap5.css" rel="stylesheet" />
</head>
```

---

## Namespace Imports

### Step 4: Import Required Namespace

**Option A: Per-Page Import**

Add at the top of your `.razor` file:
```razor
@using Syncfusion.Blazor.Inputs
```

**Option B: Global Import (Recommended)**

Add to `_Imports.razor`:
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Inputs
```

---

## Basic SfOtpInput Component Setup

### Minimal Example

The simplest OtpInput with default 4-digit numeric input:

```razor
@page "/otp-basic"
@using Syncfusion.Blazor.Inputs

<h3>Enter Verification Code</h3>

<SfOtpInput @bind-Value="@otpValue"></SfOtpInput>

<p>Entered OTP: @otpValue</p>

@code {
    private string otpValue = "";
}
```

**What this does:**
- Creates 4 input fields (default length)
- Accepts numeric input only (default type)
- Binds value to `otpValue` property
- Displays entered OTP below the component

---

## Length Property for OTP Digit Count

The `Length` property determines how many input fields are displayed.

### Common Length Configurations

**4-Digit OTP (Common for SMS verification):**
```razor
<SfOtpInput @bind-Value="@otpValue" Length="4"></SfOtpInput>
```

**6-Digit OTP (Common for authenticator apps, email verification):**
```razor
<SfOtpInput @bind-Value="@otpValue" Length="6"></SfOtpInput>
```

**8-Digit OTP (For high-security scenarios):**
```razor
<SfOtpInput @bind-Value="@otpValue" Length="8"></SfOtpInput>
```

**Example with Different Lengths:**
```razor
@page "/otp-lengths"
@using Syncfusion.Blazor.Inputs

<div class="otp-examples">
    <div class="otp-section">
        <label>4-Digit SMS Code:</label>
        <SfOtpInput @bind-Value="@smsCode" Length="4"></SfOtpInput>
    </div>
    
    <div class="otp-section">
        <label>6-Digit Email Verification:</label>
        <SfOtpInput @bind-Value="@emailCode" Length="6"></SfOtpInput>
    </div>
    
    <div class="otp-section">
        <label>8-Digit Security PIN:</label>
        <SfOtpInput @bind-Value="@securityPin" Length="8"></SfOtpInput>
    </div>
</div>

@code {
    private string smsCode = "";
    private string emailCode = "";
    private string securityPin = "";
}

<style>
    .otp-section {
        margin-bottom: 20px;
    }
    
    .otp-section label {
        display: block;
        margin-bottom: 8px;
        font-weight: 500;
    }
</style>
```

---

## Value Binding Basics

### Two-Way Binding with @bind-Value

The OtpInput component supports two-way data binding using the `@bind-Value` directive.

```razor
<SfOtpInput @bind-Value="@otpValue"></SfOtpInput>

@code {
    private string otpValue = "";
}
```

**How it works:**
- User types in input fields → `otpValue` updates automatically
- You can programmatically set `otpValue` → UI updates automatically
- Value is a concatenated string of all digits

### Retrieving the Complete OTP Value

```razor
@page "/otp-submit"
@using Syncfusion.Blazor.Inputs

<h3>Enter 6-Digit Verification Code</h3>

<SfOtpInput @bind-Value="@otpValue" Length="6"></SfOtpInput>

<button class="btn btn-primary" @onclick="SubmitOtp" disabled="@(!IsOtpComplete)">
    Verify Code
</button>

@if (!string.IsNullOrEmpty(message))
{
    <div class="alert @messageClass">@message</div>
}

@code {
    private string otpValue = "";
    private string message = "";
    private string messageClass = "";
    
    private bool IsOtpComplete => otpValue.Length == 6;
    
    private async Task SubmitOtp()
    {
        if (otpValue == "123456") // Replace with actual verification logic
        {
            message = "Verification successful!";
            messageClass = "alert-success";
        }
        else
        {
            message = "Invalid code. Please try again.";
            messageClass = "alert-danger";
            otpValue = ""; // Clear for retry
        }
    }
}
```

### Pre-filling OTP Value

You can set an initial value programmatically:

```razor
<SfOtpInput @bind-Value="@otpValue" Length="6"></SfOtpInput>

<button @onclick="PreFillOtp">Pre-fill Test Code</button>

@code {
    private string otpValue = "";
    
    private void PreFillOtp()
    {
        otpValue = "123456";
    }
}
```

---

## Minimal Working Example

Here's a complete, copy-paste-ready example for email verification:

```razor
@page "/email-verification"
@using Syncfusion.Blazor.Inputs

<div class="verification-container">
    <h2>Email Verification</h2>
    <p class="instruction">We've sent a 6-digit code to your email address.</p>
    
    <div class="otp-wrapper">
        <SfOtpInput @bind-Value="@verificationCode" 
                    Length="6"
                    AutoFocus="true">
        </SfOtpInput>
    </div>
    
    <button class="btn-verify" 
            @onclick="VerifyCode" 
            disabled="@(verificationCode.Length < 6)">
        Verify Email
    </button>
    
    <div class="resend-section">
        <span>Didn't receive the code?</span>
        <button class="btn-link" @onclick="ResendCode">Resend Code</button>
    </div>
    
    @if (!string.IsNullOrEmpty(statusMessage))
    {
        <div class="status-message @statusClass">
            @statusMessage
        </div>
    }
</div>

@code {
    private string verificationCode = "";
    private string statusMessage = "";
    private string statusClass = "";
    
    private async Task VerifyCode()
    {
        statusMessage = "Verifying...";
        statusClass = "info";
        
        // Simulate API call
        await Task.Delay(1000);
        
        // Replace with actual verification logic
        bool isValid = await VerifyWithServer(verificationCode);
        
        if (isValid)
        {
            statusMessage = "✓ Email verified successfully!";
            statusClass = "success";
            // Navigate to next page or update user status
        }
        else
        {
            statusMessage = "✗ Invalid code. Please check and try again.";
            statusClass = "error";
            verificationCode = ""; // Clear for retry
        }
    }
    
    private async Task<bool> VerifyWithServer(string code)
    {
        // Replace with actual API call to your backend
        // Example: return await Http.PostAsJsonAsync("api/verify-email", new { code });
        return code == "123456"; // Mock verification
    }
    
    private async Task ResendCode()
    {
        statusMessage = "Sending new code...";
        statusClass = "info";
        
        await Task.Delay(500);
        
        // Call your API to resend verification code
        statusMessage = "✓ New code sent to your email";
        statusClass = "success";
    }
}

<style>
    .verification-container {
        max-width: 400px;
        margin: 50px auto;
        padding: 30px;
        text-align: center;
        border-radius: 8px;
        box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }
    
    .instruction {
        color: #666;
        margin-bottom: 30px;
    }
    
    .otp-wrapper {
        margin: 30px 0;
    }
    
    .btn-verify {
        width: 100%;
        padding: 12px;
        font-size: 16px;
        font-weight: 600;
        color: white;
        background-color: #0d6efd;
        border: none;
        border-radius: 6px;
        cursor: pointer;
        transition: background-color 0.3s;
    }
    
    .btn-verify:hover:not(:disabled) {
        background-color: #0b5ed7;
    }
    
    .btn-verify:disabled {
        background-color: #ccc;
        cursor: not-allowed;
    }
    
    .resend-section {
        margin-top: 20px;
        font-size: 14px;
        color: #666;
    }
    
    .btn-link {
        background: none;
        border: none;
        color: #0d6efd;
        text-decoration: underline;
        cursor: pointer;
        margin-left: 5px;
    }
    
    .status-message {
        margin-top: 20px;
        padding: 12px;
        border-radius: 6px;
        font-weight: 500;
    }
    
    .status-message.success {
        background-color: #d1e7dd;
        color: #0f5132;
    }
    
    .status-message.error {
        background-color: #f8d7da;
        color: #842029;
    }
    
    .status-message.info {
        background-color: #cff4fc;
        color: #055160;
    }
</style>
```

---

## Next Steps

Once you have the basic OtpInput working:

1. **Configuration** → Read [otpinput-configuration.md](otpinput-configuration.md) to learn about Type (Number, Text, Password), Placeholder, Separator, and other options.

2. **Styling** → Read [otpinput-styling-modes.md](otpinput-styling-modes.md) to customize appearance with StylingMode, TextTransform, and themes.

3. **Events** → Read [otpinput-events-binding.md](otpinput-events-binding.md) to handle ValueChanged, OnInput, OnFocus, and OnBlur events for advanced scenarios.

4. **Accessibility** → Read [otpinput-accessibility.md](otpinput-accessibility.md) to ensure WCAG compliance and optimal UX for all users.

---

## Quick Troubleshooting

**Issue: OtpInput not visible**
- ✓ Ensure CSS theme is imported in `_Host.cshtml` or `index.html`
- ✓ Verify `AddSyncfusionBlazor()` is called in `Program.cs`
- ✓ Check browser console for CSS loading errors

**Issue: Two-way binding not working**
- ✓ Use `@bind-Value` (not just `Value`)
- ✓ Ensure variable is declared in `@code` block
- ✓ Check for typos in variable names

**Issue: Cannot type in input fields**
- ✓ Remove `Disabled="true"` if present
- ✓ Check if `Type` matches expected input (e.g., letters won't work with `Type="OtpInputType.Number"`)
- ✓ Ensure component is not set to `IsReadOnly="true"`

**Issue: Focus not moving between inputs**
- ✓ This is automatic behavior; check if custom JavaScript is interfering
- ✓ Ensure all inputs are enabled
- ✓ Test keyboard navigation (arrows, tab, backspace)
