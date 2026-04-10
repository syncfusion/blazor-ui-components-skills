# OtpInput - Accessibility

## Table of Contents
- [AriaLabels Array for Individual Inputs](#arialabels-array-for-individual-inputs)
- [Keyboard Navigation Support](#keyboard-navigation-support)
- [Screen Reader Compatibility](#screen-reader-compatibility)
- [WCAG 2.1 Compliance](#wcag-21-compliance)
- [Focus Management Best Practices](#focus-management-best-practices)
- [Password Type Accessibility](#password-type-accessibility-considerations)
- [Best Practices for OTP UX](#best-practices-for-otp-ux)
- [Mobile Device Considerations](#mobile-device-considerations)

---

## AriaLabels Array for Individual Inputs

The `AriaLabels` property allows you to set custom ARIA labels for each individual OTP input field, improving screen reader announcements.

### Basic AriaLabels Setup

```razor
<SfOtpInput @bind-Value="@otpValue"
            Length="6"
            AriaLabels="@ariaLabels">
</SfOtpInput>

@code {
    private string otpValue = "";
    
    private string[] ariaLabels = new string[]
    {
        "First digit",
        "Second digit",
        "Third digit",
        "Fourth digit",
        "Fifth digit",
        "Sixth digit"
    };
}
```

**Screen Reader Output:** When user focuses each field, screen reader announces:
- Field 1: "First digit, edit text"
- Field 2: "Second digit, edit text"
- etc.

### Descriptive AriaLabels

```razor
@page "/aria-descriptive"
@using Syncfusion.Blazor.Inputs

<div class="aria-demo">
    <h2 id="otp-title">Email Verification</h2>
    <p id="otp-instructions">Enter the 6-digit code sent to your email address</p>
    
    <SfOtpInput @bind-Value="@verificationCode"
                Length="6"
                AriaLabels="@detailedLabels"
                HtmlAttributes="@accessibilityAttrs">
    </SfOtpInput>
</div>

@code {
    private string verificationCode = "";
    
    private string[] detailedLabels = new string[]
    {
        "Verification code, digit 1 of 6",
        "Verification code, digit 2 of 6",
        "Verification code, digit 3 of 6",
        "Verification code, digit 4 of 6",
        "Verification code, digit 5 of 6",
        "Verification code, digit 6 of 6"
    };
    
    private Dictionary<string, object> accessibilityAttrs = new Dictionary<string, object>
    {
        { "aria-labelledby", "otp-title" },
        { "aria-describedby", "otp-instructions" },
        { "role", "group" }
    };
}
```

**Benefits:**
- Clear context for each input field
- Progress indication (digit X of Y)
- Better understanding for screen reader users

### Context-Specific Labels

**Two-Factor Authentication:**
```razor
private string[] twoFactorLabels = new string[]
{
    "Two-factor authentication code, digit 1",
    "Two-factor authentication code, digit 2",
    "Two-factor authentication code, digit 3",
    "Two-factor authentication code, digit 4",
    "Two-factor authentication code, digit 5",
    "Two-factor authentication code, digit 6"
};
```

**PIN Entry:**
```razor
private string[] pinLabels = new string[]
{
    "PIN digit 1 of 4",
    "PIN digit 2 of 4",
    "PIN digit 3 of 4",
    "PIN digit 4 of 4"
};
```

**SMS Verification:**
```razor
private string[] smsLabels = new string[]
{
    "SMS code, first digit",
    "SMS code, second digit",
    "SMS code, third digit",
    "SMS code, fourth digit"
};
```

---

## Keyboard Navigation Support

The OtpInput component provides comprehensive keyboard navigation following accessibility best practices.

### Supported Keyboard Actions

| Key | Action | Description |
|-----|--------|-------------|
| **0-9** | Input digit | Enter numeric value in current field |
| **A-Z** | Input letter | Enter letter (Text type only) |
| **→ (Right Arrow)** | Move forward | Focus next input field |
| **← (Left Arrow)** | Move backward | Focus previous input field |
| **Backspace** | Delete & move back | Delete current value, focus previous field |
| **Delete** | Clear | Clear current field value |
| **Tab** | Navigate forward | Move to next focusable element |
| **Shift+Tab** | Navigate backward | Move to previous focusable element |
| **Ctrl+V** | Paste | Paste OTP code from clipboard |
| **Home** | Jump to start | Focus first input field |
| **End** | Jump to end | Focus last input field |

### Auto-Advance Behavior

```razor
@page "/auto-advance"
@using Syncfusion.Blazor.Inputs

<div class="auto-advance-demo">
    <h3>Keyboard Navigation Demo</h3>
    <p class="help-text">
        ⌨️ Type digits to auto-advance to next field<br/>
        ⬅️➡️ Use arrow keys to move between fields<br/>
        ⌫ Backspace deletes and moves to previous field
    </p>
    
    <SfOtpInput @bind-Value="@otpValue"
                Length="6"
                Type="OtpInputType.Number"
                AutoFocus="true">
    </SfOtpInput>
    
    <p class="status">Value: @otpValue (@otpValue.Length/6)</p>
</div>

@code {
    private string otpValue = "";
}

<style>
    .help-text {
        background: #f8f9fa;
        padding: 15px;
        border-left: 4px solid #0d6efd;
        margin: 20px 0;
        font-size: 14px;
        line-height: 1.8;
    }
    
    .status {
        margin-top: 20px;
        font-family: monospace;
        color: #666;
    }
</style>
```

### Keyboard Navigation Best Practices

**1. Always Enable AutoFocus for Keyboard Users:**
```razor
<SfOtpInput @bind-Value="@otpValue"
            Length="6"
            AutoFocus="true">
</SfOtpInput>
```

**2. Provide Clear Instructions:**
```razor
<label id="otp-label">Enter 6-digit verification code</label>
<p id="otp-help" class="sr-only">Use arrow keys to navigate between digits. Backspace to delete and move back.</p>

<SfOtpInput @bind-Value="@otpValue"
            Length="6"
            HtmlAttributes="@keyboardAttrs">
</SfOtpInput>

@code {
    private string otpValue = "";
    
    private Dictionary<string, object> keyboardAttrs = new Dictionary<string, object>
    {
        { "aria-labelledby", "otp-label" },
        { "aria-describedby", "otp-help" }
    };
}
```

**3. Test Paste Functionality:**
```razor
@page "/paste-test"
@using Syncfusion.Blazor.Inputs

<div class="paste-demo">
    <h3>Paste Test</h3>
    <p>Try copying: <code>123456</code></p>
    
    <SfOtpInput @bind-Value="@otpValue" Length="6"></SfOtpInput>
    
    <p>Pasted value: @otpValue</p>
</div>

@code {
    private string otpValue = "";
}
```

---

## Screen Reader Compatibility

### Screen Reader Announcements

**On Focus:**
```
"Verification code, digit 1 of 6, edit text"
```

**On Input:**
```
"5" (character entered)
```

**On Field Change (auto-advance):**
```
"Verification code, digit 2 of 6, edit text"
```

**On Complete:**
```
"Code complete. Press Enter to verify."
```

### Enhancing Screen Reader Experience

```razor
@page "/screen-reader"
@using Syncfusion.Blazor.Inputs

<div class="sr-demo">
    <h1 id="page-title">Account Verification</h1>
    
    <div role="region" aria-labelledby="page-title">
        <p id="verification-instructions" class="instructions">
            We've sent a 6-digit verification code to your email. 
            Enter the code below to verify your account.
        </p>
        
        <div class="otp-container">
            <label id="otp-label" for="otp-input">Verification Code</label>
            
            <SfOtpInput @bind-Value="@verificationCode"
                        Length="6"
                        Type="OtpInputType.Number"
                        AriaLabels="@accessibleLabels"
                        ID="otp-input"
                        HtmlAttributes="@screenReaderAttrs"
                        ValueChanged="AnnounceCompletion">
            </SfOtpInput>
            
            <!-- Live region for dynamic announcements -->
            <div aria-live="polite" aria-atomic="true" class="sr-only">
                @liveMessage
            </div>
        </div>
        
        <button @onclick="VerifyCode" 
                disabled="@(verificationCode.Length < 6)"
                aria-label="Verify the entered code">
            Verify Code
        </button>
    </div>
</div>

@code {
    private string verificationCode = "";
    private string liveMessage = "";
    
    private string[] accessibleLabels = new string[]
    {
        "Digit 1 of 6",
        "Digit 2 of 6",
        "Digit 3 of 6",
        "Digit 4 of 6",
        "Digit 5 of 6",
        "Digit 6 of 6"
    };
    
    private Dictionary<string, object> screenReaderAttrs = new Dictionary<string, object>
    {
        { "aria-labelledby", "otp-label" },
        { "aria-describedby", "verification-instructions" },
        { "aria-required", "true" },
        { "role", "group" }
    };
    
    private void AnnounceCompletion(string value)
    {
        verificationCode = value;
        
        if (value.Length == 6)
        {
            liveMessage = "Code complete. 6 digits entered. Press Verify Code button to continue.";
        }
        else
        {
            liveMessage = $"{value.Length} of 6 digits entered";
        }
    }
    
    private void VerifyCode()
    {
        liveMessage = "Verifying code, please wait...";
        // Verification logic
    }
}

<style>
    .sr-only {
        position: absolute;
        left: -10000px;
        width: 1px;
        height: 1px;
        overflow: hidden;
    }
    
    .instructions {
        margin: 20px 0;
        font-size: 16px;
        color: #555;
    }
</style>
```

### ARIA Live Regions for Dynamic Updates

```razor
<!-- Announce verification status -->
<div aria-live="assertive" aria-atomic="true" class="sr-only">
    @statusAnnouncement
</div>

@code {
    private string statusAnnouncement = "";
    
    private async Task VerifyCode()
    {
        statusAnnouncement = "Verifying your code";
        
        await Task.Delay(2000);
        
        if (isValid)
        {
            statusAnnouncement = "Success! Your code has been verified.";
        }
        else
        {
            statusAnnouncement = "Error. The code you entered is invalid. Please try again.";
        }
    }
}
```

---

## WCAG 2.1 Compliance

### Level A Requirements

✅ **1.1.1 Non-text Content:**
- AriaLabels provide text alternatives for input fields
- Clear labeling describes purpose of each input

✅ **1.3.1 Info and Relationships:**
- Semantic HTML structure with proper labels
- Group relationship via `role="group"`

✅ **2.1.1 Keyboard:**
- Full keyboard navigation support
- No mouse-only interactions

✅ **2.4.3 Focus Order:**
- Logical left-to-right focus progression
- Auto-advance maintains natural flow

### Level AA Requirements

✅ **1.4.3 Contrast (Minimum):**
```css
/* Ensure sufficient contrast ratios */
.otp-input input {
    color: #000000;           /* Text: 21:1 contrast on white */
    background: #FFFFFF;
    border-color: #767676;    /* 4.5:1 contrast minimum */
}

.otp-input input:focus {
    border-color: #0056b3;    /* High contrast focus indicator */
    outline: 3px solid #0056b3;
    outline-offset: 2px;
}
```

✅ **2.4.7 Focus Visible:**
```razor
<SfOtpInput @bind-Value="@otpValue"
            Length="6"
            CssClass="wcag-compliant">
</SfOtpInput>

<style>
    .wcag-compliant input:focus {
        outline: 3px solid #0056b3;
        outline-offset: 2px;
        border-color: #0056b3;
        box-shadow: 0 0 0 4px rgba(0, 86, 179, 0.25);
    }
</style>
```

✅ **3.3.2 Labels or Instructions:**
```razor
<fieldset>
    <legend>Two-Factor Authentication</legend>
    <p class="instructions">
        Enter the 6-digit code from your authenticator app. 
        Codes expire after 30 seconds.
    </p>
    
    <SfOtpInput @bind-Value="@code" Length="6" AriaLabels="@labels"></SfOtpInput>
</fieldset>
```

### WCAG-Compliant Complete Example

```razor
@page "/wcag-compliant"
@using Syncfusion.Blazor.Inputs

<main role="main" aria-labelledby="page-heading">
    <h1 id="page-heading">Verify Your Identity</h1>
    
    <form @onsubmit="HandleSubmit" @onsubmit:preventDefault>
        <fieldset class="otp-fieldset">
            <legend id="otp-legend">
                Enter Verification Code
            </legend>
            
            <p id="otp-description" class="help-text">
                We've sent a 6-digit verification code to your registered 
                email address. Enter the code below to continue. 
                The code will expire in 5 minutes.
            </p>
            
            <div class="form-group">
                <label id="otp-label" class="sr-only">
                    6-digit verification code
                </label>
                
                <SfOtpInput @bind-Value="@verificationCode"
                            Length="6"
                            Type="OtpInputType.Number"
                            AutoFocus="true"
                            AriaLabels="@wcagLabels"
                            HtmlAttributes="@wcagAttributes"
                            CssClass="wcag-otp">
                </SfOtpInput>
                
                <!-- Error message -->
                @if (!string.IsNullOrEmpty(errorMessage))
                {
                    <div id="otp-error" 
                         role="alert" 
                         aria-live="assertive" 
                         class="error-message">
                        <span aria-label="Error">⚠️</span> @errorMessage
                    </div>
                }
                
                <!-- Live status updates -->
                <div aria-live="polite" aria-atomic="true" class="sr-only">
                    @statusMessage
                </div>
            </div>
            
            <div class="form-actions">
                <button type="submit" 
                        disabled="@(verificationCode.Length < 6 || isProcessing)"
                        aria-label="@(isProcessing ? "Verifying code, please wait" : "Verify code and continue")">
                    @(isProcessing ? "Verifying..." : "Verify Code")
                </button>
                
                <button type="button" 
                        @onclick="ResendCode"
                        aria-label="Resend verification code to your email">
                    Resend Code
                </button>
            </div>
        </fieldset>
    </form>
</main>

@code {
    private string verificationCode = "";
    private bool isProcessing = false;
    private string errorMessage = "";
    private string statusMessage = "";
    
    private string[] wcagLabels = new string[]
    {
        "Verification code, digit 1 of 6",
        "Verification code, digit 2 of 6",
        "Verification code, digit 3 of 6",
        "Verification code, digit 4 of 6",
        "Verification code, digit 5 of 6",
        "Verification code, digit 6 of 6"
    };
    
    private Dictionary<string, object> wcagAttributes = new Dictionary<string, object>
    {
        { "aria-labelledby", "otp-legend" },
        { "aria-describedby", "otp-description" },
        { "aria-required", "true" },
        { "aria-invalid", "false" },
        { "role", "group" }
    };
    
    private async Task HandleSubmit()
    {
        isProcessing = true;
        errorMessage = "";
        statusMessage = "Verifying your code, please wait";
        
        await Task.Delay(2000);
        
        bool isValid = verificationCode == "123456";
        
        if (isValid)
        {
            statusMessage = "Success! Code verified. Redirecting to your account.";
        }
        else
        {
            errorMessage = "The code you entered is incorrect. Please check and try again.";
            statusMessage = "Verification failed. Please enter a valid code.";
            wcagAttributes["aria-invalid"] = "true";
            wcagAttributes["aria-describedby"] = "otp-description otp-error";
            verificationCode = "";
        }
        
        isProcessing = false;
    }
    
    private async Task ResendCode()
    {
        statusMessage = "Sending new verification code to your email";
        await Task.Delay(1000);
        statusMessage = "New verification code sent successfully";
    }
}

<style>
    .otp-fieldset {
        border: none;
        padding: 0;
        margin: 40px 0;
    }
    
    .help-text {
        color: #555;
        margin: 15px 0 30px;
        font-size: 16px;
        line-height: 1.6;
    }
    
    .wcag-otp input {
        /* High contrast focus */
    }
    
    .wcag-otp input:focus {
        outline: 3px solid #0056b3;
        outline-offset: 2px;
        border-color: #0056b3;
        box-shadow: 0 0 0 4px rgba(0, 86, 179, 0.25);
    }
    
    .error-message {
        margin-top: 15px;
        padding: 12px;
        background: #f8d7da;
        border-left: 4px solid #dc3545;
        color: #721c24;
        border-radius: 4px;
    }
    
    .form-actions {
        margin-top: 25px;
        display: flex;
        gap: 15px;
    }
    
    .form-actions button {
        padding: 12px 24px;
        font-size: 16px;
        font-weight: 600;
        border: none;
        border-radius: 6px;
        cursor: pointer;
    }
    
    .form-actions button[type="submit"] {
        background: #0d6efd;
        color: white;
    }
    
    .form-actions button[type="submit"]:disabled {
        background: #ccc;
        cursor: not-allowed;
    }
    
    .form-actions button[type="button"] {
        background: white;
        color: #0d6efd;
        border: 2px solid #0d6efd;
    }
    
    .sr-only {
        position: absolute;
        left: -10000px;
        width: 1px;
        height: 1px;
        overflow: hidden;
    }
</style>
```

---

## Focus Management Best Practices

### 1. Clear Focus Indicators

```css
/* Always visible, high-contrast focus rings */
.otp-input input:focus {
    outline: 3px solid #0056b3;
    outline-offset: 2px;
    border-color: #0056b3;
    box-shadow: 0 0 0 4px rgba(0, 86, 179, 0.25);
    z-index: 1;
}

/* Never use outline: none without alternative */
```

### 2. Focus Trap for Modal OTP Inputs

```razor
@if (showOtpModal)
{
    <div class="modal-overlay" @onclick="CloseModal">
        <div class="modal-content" @onclick:stopPropagation>
            <h2>Enter Verification Code</h2>
            
            <SfOtpInput @bind-Value="@modalOtp"
                        Length="6"
                        AutoFocus="true">
            </SfOtpInput>
            
            <button @onclick="SubmitModal">Submit</button>
            <button @onclick="CloseModal">Cancel</button>
        </div>
    </div>
}
```

### 3. Restore Focus After Verification

```razor
private ElementReference? previousFocus;

private async Task VerifyAndRestoreFocus()
{
    // Store current focus
    // previousFocus = await JS.InvokeAsync<ElementReference>("document.activeElement");
    
    await VerifyCode();
    
    // Restore focus after verification
    // await previousFocus.FocusAsync();
}
```

---

## Password Type Accessibility Considerations

### Show/Hide Toggle for Password OTP

```razor
@page "/password-toggle"
@using Syncfusion.Blazor.Inputs

<div class="password-otp">
    <label id="pin-label">Enter 4-Digit PIN</label>
    
    <div class="input-group">
        <SfOtpInput @bind-Value="@pinCode"
                    Length="4"
                    Type="@(showPin ? OtpInputType.Number : OtpInputType.Password)"
                    AriaLabels="@pinLabels"
                    HtmlAttributes="@pinAttrs">
        </SfOtpInput>
        
        <button type="button"
                @onclick="ToggleVisibility"
                aria-label="@(showPin ? "Hide PIN" : "Show PIN")"
                aria-pressed="@showPin"
                class="toggle-visibility">
            @(showPin ? "👁️" : "👁️‍🗨️")
        </button>
    </div>
    
    <!-- Announce visibility change -->
    <div aria-live="polite" class="sr-only">
        @visibilityAnnouncement
    </div>
</div>

@code {
    private string pinCode = "";
    private bool showPin = false;
    private string visibilityAnnouncement = "";
    
    private string[] pinLabels = new string[]
    {
        "PIN digit 1 of 4",
        "PIN digit 2 of 4",
        "PIN digit 3 of 4",
        "PIN digit 4 of 4"
    };
    
    private Dictionary<string, object> pinAttrs = new Dictionary<string, object>
    {
        { "aria-labelledby", "pin-label" },
        { "role", "group" }
    };
    
    private void ToggleVisibility()
    {
        showPin = !showPin;
        visibilityAnnouncement = showPin ? "PIN is now visible" : "PIN is now hidden";
    }
}

<style>
    .input-group {
        display: flex;
        align-items: center;
        gap: 15px;
    }
    
    .toggle-visibility {
        padding: 10px;
        background: #f0f0f0;
        border: 2px solid #ccc;
        border-radius: 6px;
        cursor: pointer;
        font-size: 20px;
    }
    
    .toggle-visibility:focus {
        outline: 3px solid #0056b3;
        outline-offset: 2px;
    }
</style>
```

---

## Best Practices for OTP UX

### 1. Clear Instructions

```razor
<div class="otp-instructions">
    <h3>Verification Required</h3>
    <p class="primary-instruction">
        Enter the 6-digit code we sent to your email
    </p>
    <ul class="tips">
        <li>Check your spam folder if you don't see the email</li>
        <li>Code expires in 5 minutes</li>
        <li>Use arrow keys to navigate between fields</li>
    </ul>
</div>
```

### 2. Error Recovery

```razor
@if (attempts >= 3)
{
    <div role="alert" class="error-alert">
        <h4>Too many failed attempts</h4>
        <p>For security, we've locked verification for 15 minutes.</p>
        <button @onclick="ContactSupport" aria-label="Contact customer support for help">
            Contact Support
        </button>
    </div>
}
```

### 3. Success Feedback

```razor
@if (isVerified)
{
    <div role="status" aria-live="polite" class="success-message">
        <span aria-label="Success">✓</span>
        Verification successful! Redirecting...
    </div>
}
```

---

## Mobile Device Considerations

### 1. Large Touch Targets

```css
/* Minimum 44x44px touch targets (iOS/WCAG) */
@media (max-width: 768px) {
    .mobile-otp input {
        width: 48px !important;
        height: 48px !important;
        font-size: 24px !important;
    }
}
```

### 2. Numeric Keyboard on Mobile

```razor
<!-- Type="OtpInputType.Number" automatically triggers numeric keyboard -->
<SfOtpInput @bind-Value="@code"
            Length="6"
            Type="OtpInputType.Number">
</SfOtpInput>
```

### 3. Auto-Fill from SMS

```razor
<!-- Enable SMS OTP auto-fill on iOS/Android -->
<SfOtpInput @bind-Value="@smsCode"
            Length="6"
            HtmlAttributes="@smsAttributes">
</SfOtpInput>

@code {
    private Dictionary<string, object> smsAttributes = new Dictionary<string, object>
    {
        { "autocomplete", "one-time-code" },
        { "inputmode", "numeric" }
    };
}
```

### 4. Viewport and Zoom

```razor
<!-- Ensure no zoom disable -->
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=5.0">
```

This comprehensive accessibility guide ensures your OTP Input implementation is usable by everyone, regardless of ability or assistive technology.
