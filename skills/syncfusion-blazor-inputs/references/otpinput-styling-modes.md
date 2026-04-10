# OtpInput - Styling Modes

## Table of Contents
- [StylingMode Overview](#stylingmode-overview)
- [Outlined Style](#outlined-style)
- [Underlined Style](#underlined-style)
- [Filled Style](#filled-style)
- [TextTransform Options](#texttransform-options)
- [CssClass for Custom Styling](#cssclass-for-custom-styling)
- [Theme Customization](#theme-customization)
- [Responsive Design Patterns](#responsive-design-patterns)

---

## StylingMode Overview

The `StylingMode` property controls the visual appearance of the OTP input fields. Syncfusion Blazor OtpInput provides three distinct styles to match your application's design language.

### OtpInputStyle Enum Values

| Style | Description | Best For |
|-------|-------------|----------|
| **Outlined** | Border around each input box (default) | Professional, formal applications |
| **Underlined** | Bottom border only | Modern, minimalist designs |
| **Filled** | Solid background with subtle border | Material Design, mobile-first apps |

### Quick Comparison

```razor
@page "/styling-comparison"
@using Syncfusion.Blazor.Inputs

<div class="styling-demo">
    <h3>OTP Styling Modes</h3>
    
    <div class="style-example">
        <label>Outlined (Default)</label>
        <SfOtpInput @bind-Value="@code1"
                    Length="6"
                    StylingMode="OtpInputStyle.Outlined">
        </SfOtpInput>
    </div>
    
    <div class="style-example">
        <label>Underlined</label>
        <SfOtpInput @bind-Value="@code2"
                    Length="6"
                    StylingMode="OtpInputStyle.Underlined">
        </SfOtpInput>
    </div>
    
    <div class="style-example">
        <label>Filled</label>
        <SfOtpInput @bind-Value="@code3"
                    Length="6"
                    StylingMode="OtpInputStyle.Filled">
        </SfOtpInput>
    </div>
</div>

@code {
    private string code1 = "";
    private string code2 = "";
    private string code3 = "";
}

<style>
    .style-example {
        margin: 30px 0;
        padding: 20px;
        background: #f8f9fa;
        border-radius: 8px;
    }
    
    .style-example label {
        display: block;
        margin-bottom: 15px;
        font-weight: 600;
        color: #333;
    }
</style>
```

---

## Outlined Style

The **Outlined** style displays each input with a complete border, providing clear visual boundaries.

### Basic Outlined Style

```razor
<SfOtpInput @bind-Value="@otpValue"
            Length="6"
            StylingMode="OtpInputStyle.Outlined">
</SfOtpInput>

@code {
    private string otpValue = "";
}
```

**Visual Characteristics:**
- Full border around each input box
- Clear separation between fields
- Default style (can omit `StylingMode` property)
- Professional, traditional appearance
- High visual clarity

### Outlined with Separator

```razor
<SfOtpInput @bind-Value="@otpValue"
            Length="6"
            StylingMode="OtpInputStyle.Outlined"
            Separator="-"
            Placeholder="0">
</SfOtpInput>

@code {
    private string otpValue = "";
}
```

### Use Cases for Outlined Style

**Enterprise Applications:**
```razor
@page "/enterprise-login"
@using Syncfusion.Blazor.Inputs

<div class="enterprise-auth">
    <h2>Corporate Authentication</h2>
    <p>Enter your security token</p>
    
    <SfOtpInput @bind-Value="@securityToken"
                Length="8"
                Type="OtpInputType.Text"
                StylingMode="OtpInputStyle.Outlined"
                Separator="-"
                TextTransform="TextTransform.Uppercase">
    </SfOtpInput>
    
    <button class="auth-button">Authenticate</button>
</div>

@code {
    private string securityToken = "";
}

<style>
    .enterprise-auth {
        max-width: 500px;
        margin: 0 auto;
        padding: 40px;
        background: white;
        border: 2px solid #e0e0e0;
        border-radius: 10px;
    }
    
    .auth-button {
        width: 100%;
        padding: 14px;
        margin-top: 25px;
        background: #0066cc;
        color: white;
        border: none;
        border-radius: 5px;
        font-size: 16px;
        font-weight: 600;
        cursor: pointer;
    }
</style>
```

**Banking/Financial Applications:**
```razor
<div class="bank-verification">
    <h3>Transaction Verification</h3>
    <p class="security-notice">🔒 Secure transaction authentication</p>
    
    <SfOtpInput @bind-Value="@transactionOtp"
                Length="6"
                Type="OtpInputType.Number"
                StylingMode="OtpInputStyle.Outlined"
                AutoFocus="true">
    </SfOtpInput>
    
    <p class="info-text">Enter the 6-digit code sent to your registered mobile</p>
</div>

@code {
    private string transactionOtp = "";
}
```

**Best For:**
- Desktop applications
- Professional/corporate environments
- High-security contexts
- When clear field boundaries are important
- Traditional web applications

---

## Underlined Style

The **Underlined** style displays only a bottom border, creating a cleaner, more modern look.

### Basic Underlined Style

```razor
<SfOtpInput @bind-Value="@otpValue"
            Length="6"
            StylingMode="OtpInputStyle.Underlined">
</SfOtpInput>

@code {
    private string otpValue = "";
}
```

**Visual Characteristics:**
- Bottom border only
- Minimalist, clean appearance
- More whitespace around inputs
- Modern, contemporary design
- Focus on content over chrome

### Underlined with Enhanced Focus

```razor
@page "/underlined-focus"
@using Syncfusion.Blazor.Inputs

<div class="modern-verify">
    <h2>Verify Your Account</h2>
    <p class="subtitle">We've sent a code to your email</p>
    
    <SfOtpInput @bind-Value="@verificationCode"
                Length="6"
                StylingMode="OtpInputStyle.Underlined"
                Type="OtpInputType.Number"
                AutoFocus="true"
                CssClass="modern-otp">
    </SfOtpInput>
    
    <button class="verify-btn" disabled="@(verificationCode.Length < 6)">
        Continue
    </button>
</div>

@code {
    private string verificationCode = "";
}

<style>
    .modern-verify {
        max-width: 450px;
        margin: 60px auto;
        text-align: center;
    }
    
    .subtitle {
        color: #666;
        margin: 10px 0 40px;
        font-size: 15px;
    }
    
    .modern-otp {
        margin: 30px 0;
    }
    
    .verify-btn {
        width: 100%;
        padding: 16px;
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        border: none;
        border-radius: 50px;
        font-size: 16px;
        font-weight: 600;
        cursor: pointer;
        transition: transform 0.2s;
    }
    
    .verify-btn:hover:not(:disabled) {
        transform: translateY(-2px);
    }
    
    .verify-btn:disabled {
        background: #ccc;
        cursor: not-allowed;
    }
</style>
```

### Use Cases for Underlined Style

**Mobile-First Applications:**
```razor
<div class="mobile-verify">
    <h3>SMS Verification</h3>
    
    <SfOtpInput @bind-Value="@smsCode"
                Length="4"
                StylingMode="OtpInputStyle.Underlined"
                Type="OtpInputType.Number"
                AutoFocus="true">
    </SfOtpInput>
    
    <div class="resend">
        Didn't receive code? <a href="#" @onclick="ResendCode">Resend</a>
    </div>
</div>

@code {
    private string smsCode = "";
    
    private void ResendCode()
    {
        // Resend logic
    }
}

<style>
    .mobile-verify {
        padding: 20px;
        text-align: center;
    }
    
    .resend {
        margin-top: 20px;
        font-size: 14px;
        color: #666;
    }
    
    .resend a {
        color: #667eea;
        text-decoration: none;
        font-weight: 600;
    }
</style>
```

**Modern Web Apps:**
```razor
<div class="signup-flow">
    <div class="step-indicator">Step 2 of 3</div>
    <h2>Confirm Your Email</h2>
    
    <SfOtpInput @bind-Value="@emailOtp"
                Length="6"
                StylingMode="OtpInputStyle.Underlined"
                Separator=" ">
    </SfOtpInput>
</div>

@code {
    private string emailOtp = "";
}
```

**Best For:**
- Mobile applications
- Modern web designs
- Minimalist interfaces
- Consumer-facing apps
- Progressive web apps (PWAs)

---

## Filled Style

The **Filled** style uses a solid background color with subtle borders, following Material Design principles.

### Basic Filled Style

```razor
<SfOtpInput @bind-Value="@otpValue"
            Length="6"
            StylingMode="OtpInputStyle.Filled">
</SfOtpInput>

@code {
    private string otpValue = "";
}
```

**Visual Characteristics:**
- Solid background color (usually light gray)
- Subtle or no border
- Material Design aesthetic
- Tactile, button-like appearance
- High contrast between filled and empty

### Filled with Dark Mode

```razor
@page "/filled-dark"
@using Syncfusion.Blazor.Inputs

<div class="dark-theme">
    <h2>Security Check</h2>
    <p class="dark-subtitle">Enter your authentication code</p>
    
    <SfOtpInput @bind-Value="@authCode"
                Length="6"
                StylingMode="OtpInputStyle.Filled"
                Type="OtpInputType.Number"
                Placeholder="0"
                CssClass="dark-otp">
    </SfOtpInput>
    
    <button class="dark-button">Verify</button>
</div>

@code {
    private string authCode = "";
}

<style>
    .dark-theme {
        background: #1e1e1e;
        color: white;
        padding: 50px;
        min-height: 400px;
        border-radius: 12px;
    }
    
    .dark-subtitle {
        color: #aaa;
        margin: 10px 0 40px;
    }
    
    .dark-otp {
        margin: 30px 0;
    }
    
    .dark-button {
        width: 100%;
        padding: 15px;
        background: #0d6efd;
        color: white;
        border: none;
        border-radius: 8px;
        font-size: 16px;
        font-weight: 600;
        cursor: pointer;
    }
</style>
```

### Use Cases for Filled Style

**Material Design Apps:**
```razor
<div class="material-verify">
    <h3>Two-Factor Authentication</h3>
    
    <SfOtpInput @bind-Value="@twoFactorCode"
                Length="6"
                StylingMode="OtpInputStyle.Filled"
                Type="OtpInputType.Number"
                AutoFocus="true">
    </SfOtpInput>
    
    <p class="helper-text">
        Enter the code from your authenticator app
    </p>
</div>

@code {
    private string twoFactorCode = "";
}

<style>
    .material-verify {
        padding: 24px;
        background: white;
        border-radius: 12px;
        box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }
    
    .helper-text {
        margin-top: 16px;
        color: #757575;
        font-size: 14px;
    }
</style>
```

**Mobile Apps with Touch Focus:**
```razor
<div class="touch-friendly">
    <SfOtpInput @bind-Value="@pinCode"
                Length="4"
                StylingMode="OtpInputStyle.Filled"
                Type="OtpInputType.Password"
                CssClass="large-touch-otp">
    </SfOtpInput>
</div>

@code {
    private string pinCode = "";
}

<style>
    .large-touch-otp {
        /* Each input field sized for easy touch targets */
    }
</style>
```

**Best For:**
- Material Design applications
- Android-style interfaces
- Touch-heavy mobile apps
- Dark mode interfaces
- Apps with high visual contrast needs

---

## TextTransform Options

The `TextTransform` property converts text input to uppercase, lowercase, or leaves it as-is.

### TextTransform Enum Values

| Value | Description | Use Case |
|-------|-------------|----------|
| **None** | No transformation (default) | Preserve user input as-is |
| **Uppercase** | Convert to uppercase | License keys, codes |
| **Lowercase** | Convert to lowercase | Email-like codes |

### Uppercase Transform

```razor
<SfOtpInput @bind-Value="@licenseKey"
            Length="12"
            Type="OtpInputType.Text"
            TextTransform="TextTransform.Uppercase"
            Separator="-">
</SfOtpInput>

@code {
    private string licenseKey = ""; // Will be: XXXX-XXXX-XXXX
}
```

**Common Use Cases:**
- License keys
- Product activation codes
- Security tokens
- API keys

### Lowercase Transform

```razor
<SfOtpInput @bind-Value="@emailCode"
            Length="8"
            Type="OtpInputType.Text"
            TextTransform="TextTransform.Lowercase">
</SfOtpInput>

@code {
    private string emailCode = ""; // Will be: abcd1234
}
```

### TextTransform Examples

```razor
@page "/text-transform"
@using Syncfusion.Blazor.Inputs

<div class="transform-demo">
    <h3>TextTransform Options</h3>
    
    <div class="transform-example">
        <label>None (As Typed)</label>
        <SfOtpInput @bind-Value="@code1"
                    Length="6"
                    Type="OtpInputType.Text"
                    TextTransform="TextTransform.None">
        </SfOtpInput>
        <p>Value: @code1</p>
    </div>
    
    <div class="transform-example">
        <label>Uppercase</label>
        <SfOtpInput @bind-Value="@code2"
                    Length="6"
                    Type="OtpInputType.Text"
                    TextTransform="TextTransform.Uppercase">
        </SfOtpInput>
        <p>Value: @code2</p>
    </div>
    
    <div class="transform-example">
        <label>Lowercase</label>
        <SfOtpInput @bind-Value="@code3"
                    Length="6"
                    Type="OtpInputType.Text"
                    TextTransform="TextTransform.Lowercase">
        </SfOtpInput>
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
- Only applies to `Type="OtpInputType.Text"`
- Has no effect on numeric or password types
- Transformation happens in UI and value binding
- Useful for consistency in case-sensitive codes

---

## CssClass for Custom Styling

The `CssClass` property applies custom CSS classes to the OtpInput wrapper element.

### Basic Custom Styling

```razor
<SfOtpInput @bind-Value="@otpValue"
            Length="6"
            CssClass="custom-otp-style">
</SfOtpInput>

@code {
    private string otpValue = "";
}

<style>
    .custom-otp-style {
        /* Custom styles applied to wrapper */
    }
</style>
```

### Advanced Custom Styling Examples

**Large Input Fields:**
```razor
<SfOtpInput @bind-Value="@code"
            Length="4"
            StylingMode="OtpInputStyle.Outlined"
            CssClass="large-otp">
</SfOtpInput>

<style>
    .large-otp input {
        width: 60px !important;
        height: 60px !important;
        font-size: 28px !important;
        font-weight: 700;
    }
</style>

@code {
    private string code = "";
}
```

**Colorful Focus States:**
```razor
<SfOtpInput @bind-Value="@code"
            Length="6"
            CssClass="colorful-focus">
</SfOtpInput>

<style>
    .colorful-focus input:focus {
        border-color: #667eea !important;
        box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.25) !important;
        outline: none;
    }
    
    .colorful-focus input {
        transition: all 0.3s ease;
    }
</style>

@code {
    private string code = "";
}
```

**Rounded Inputs:**
```razor
<SfOtpInput @bind-Value="@code"
            Length="6"
            StylingMode="OtpInputStyle.Filled"
            CssClass="rounded-otp">
</SfOtpInput>

<style>
    .rounded-otp input {
        border-radius: 12px !important;
        background: #f0f0f0;
    }
</style>

@code {
    private string code = "";
}
```

**Success/Error States:**
```razor
@page "/otp-states"
@using Syncfusion.Blazor.Inputs

<div class="state-demo">
    <SfOtpInput @bind-Value="@otpValue"
                Length="6"
                CssClass="@otpStateClass">
    </SfOtpInput>
    
    <button @onclick="ValidateOtp">Validate</button>
</div>

@code {
    private string otpValue = "";
    private string otpStateClass = "";
    
    private void ValidateOtp()
    {
        if (otpValue == "123456")
        {
            otpStateClass = "otp-success";
        }
        else
        {
            otpStateClass = "otp-error";
        }
    }
}

<style>
    .otp-success input {
        border-color: #28a745 !important;
        background-color: #d4edda !important;
    }
    
    .otp-error input {
        border-color: #dc3545 !important;
        background-color: #f8d7da !important;
        animation: shake 0.4s;
    }
    
    @keyframes shake {
        0%, 100% { transform: translateX(0); }
        25% { transform: translateX(-10px); }
        75% { transform: translateX(10px); }
    }
</style>
```

---

## Theme Customization

### Using Syncfusion Theme Studio

Syncfusion provides a Theme Studio for customizing component themes:

**Steps:**
1. Visit [Syncfusion Theme Studio](https://blazor.syncfusion.com/themestudio/)
2. Select OtpInput component
3. Customize colors, spacing, borders
4. Download custom theme CSS
5. Replace default theme in your app

### Theme Variables (Example)

```css
/* Custom theme variables for OtpInput */
:root {
    --otp-border-color: #667eea;
    --otp-focus-color: #764ba2;
    --otp-background: #f8f9fa;
    --otp-text-color: #333;
    --otp-placeholder-color: #999;
}
```

### Applying Multiple Themes

```razor
@page "/theme-switcher"
@using Syncfusion.Blazor.Inputs

<div class="theme-demo">
    <div class="theme-selector">
        <button @onclick='() => selectedTheme = "light"'>Light</button>
        <button @onclick='() => selectedTheme = "dark"'>Dark</button>
        <button @onclick='() => selectedTheme = "blue"'>Blue</button>
    </div>
    
    <div class="@($"theme-{selectedTheme}")">
        <SfOtpInput @bind-Value="@otpValue"
                    Length="6"
                    StylingMode="OtpInputStyle.Outlined">
        </SfOtpInput>
    </div>
</div>

@code {
    private string otpValue = "";
    private string selectedTheme = "light";
}

<style>
    .theme-light { background: white; color: black; }
    .theme-dark { background: #1e1e1e; color: white; padding: 20px; }
    .theme-blue { background: #e3f2fd; color: #0d47a1; padding: 20px; }
</style>
```

---

## Responsive Design Patterns

### Mobile-First Approach

```razor
<SfOtpInput @bind-Value="@otpValue"
            Length="6"
            StylingMode="OtpInputStyle.Underlined"
            CssClass="responsive-otp">
</SfOtpInput>

<style>
    /* Mobile (default) */
    .responsive-otp input {
        width: 40px;
        height: 50px;
        font-size: 20px;
    }
    
    /* Tablet */
    @media (min-width: 768px) {
        .responsive-otp input {
            width: 50px;
            height: 60px;
            font-size: 24px;
        }
    }
    
    /* Desktop */
    @media (min-width: 1024px) {
        .responsive-otp input {
            width: 60px;
            height: 70px;
            font-size: 28px;
        }
    }
</style>

@code {
    private string otpValue = "";
}
```

### Adaptive Layout

```razor
@page "/responsive-verify"
@using Syncfusion.Blazor.Inputs

<div class="verify-container">
    <div class="verify-card">
        <h2>Verify Your Account</h2>
        
        <SfOtpInput @bind-Value="@verificationCode"
                    Length="6"
                    StylingMode="OtpInputStyle.Filled"
                    AutoFocus="true"
                    CssClass="adaptive-otp">
        </SfOtpInput>
        
        <button class="verify-button">Verify</button>
    </div>
</div>

@code {
    private string verificationCode = "";
}

<style>
    .verify-container {
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 100vh;
        padding: 20px;
    }
    
    .verify-card {
        width: 100%;
        max-width: 450px;
        padding: 30px;
        background: white;
        border-radius: 12px;
        box-shadow: 0 4px 20px rgba(0,0,0,0.1);
    }
    
    .adaptive-otp {
        margin: 30px 0;
    }
    
    /* Mobile adjustments */
    @media (max-width: 480px) {
        .verify-card {
            padding: 20px;
        }
        
        .adaptive-otp input {
            width: 38px !important;
            height: 48px !important;
            font-size: 18px !important;
        }
    }
    
    .verify-button {
        width: 100%;
        padding: 14px;
        background: #0d6efd;
        color: white;
        border: none;
        border-radius: 8px;
        font-size: 16px;
        font-weight: 600;
        cursor: pointer;
    }
</style>
```

This comprehensive guide covers all styling and customization options for the OtpInput component, enabling you to create beautiful, accessible verification flows that match your application's design system.
