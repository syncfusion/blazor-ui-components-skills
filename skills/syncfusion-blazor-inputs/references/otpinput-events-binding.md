# OtpInput - Events and Data Binding

## Table of Contents
- [Value Property and Two-Way Binding](#value-property-and-two-way-binding)
- [ValueChanged Event Callback](#valuechanged-event-callback)
- [OnInput Event](#oninput-event)
- [OnFocus and OnBlur Events](#onfocus-and-onblur-events)
- [Created Lifecycle Event](#created-lifecycle-event)
- [Event Argument Structures](#event-argument-structures)
- [Form Validation Integration](#form-validation-integration)
- [Real-Time Verification Patterns](#real-time-verification-patterns)
- [Auto-Submit on Completion](#auto-submit-on-completion)

---

## Value Property and Two-Way Binding

The `Value` property stores the concatenated string of all OTP digits entered by the user.

### Basic Two-Way Binding

```razor
<SfOtpInput @bind-Value="@otpValue" Length="6"></SfOtpInput>

<p>Current Value: @otpValue</p>
<p>Length: @otpValue.Length / 6</p>

@code {
    private string otpValue = "";
}
```

**How it works:**
- User types → `otpValue` updates automatically
- You set `otpValue` → UI updates automatically
- Empty fields are not included in the value
- Value is always a string, even for numeric types

### Programmatic Value Setting

```razor
@page "/otp-preset"
@using Syncfusion.Blazor.Inputs

<div class="preset-demo">
    <SfOtpInput @bind-Value="@otpValue" Length="6"></SfOtpInput>
    
    <div class="button-group">
        <button @onclick="SetTestCode">Set Test Code (123456)</button>
        <button @onclick="ClearCode">Clear</button>
        <button @onclick="SetPartialCode">Set Partial (123)</button>
    </div>
    
    <p>Current Value: "@otpValue"</p>
</div>

@code {
    private string otpValue = "";
    
    private void SetTestCode()
    {
        otpValue = "123456";
    }
    
    private void ClearCode()
    {
        otpValue = "";
    }
    
    private void SetPartialCode()
    {
        otpValue = "123"; // Only first 3 fields will be filled
    }
}

<style>
    .button-group {
        margin: 20px 0;
        display: flex;
        gap: 10px;
    }
    
    .button-group button {
        padding: 8px 16px;
        border: 1px solid #0d6efd;
        background: white;
        color: #0d6efd;
        border-radius: 4px;
        cursor: pointer;
    }
    
    .button-group button:hover {
        background: #0d6efd;
        color: white;
    }
</style>
```

### Value Behavior

**Complete OTP:**
```razor
// User fills all 6 fields: 1, 2, 3, 4, 5, 6
// otpValue = "123456"
// otpValue.Length = 6
```

**Partial OTP:**
```razor
// User fills first 3 fields: 1, 2, 3
// otpValue = "123"
// otpValue.Length = 3
```

**Empty OTP:**
```razor
// No fields filled
// otpValue = ""
// otpValue.Length = 0
```

---

## ValueChanged Event Callback

The `ValueChanged` event fires whenever the OTP value changes (on each keystroke, paste, or programmatic update).

### Basic ValueChanged Handler

```razor
<SfOtpInput Value="@otpValue"
            Length="6"
            ValueChanged="OnValueChanged">
</SfOtpInput>

<p>Status: @statusMessage</p>

@code {
    private string otpValue = "";
    private string statusMessage = "Waiting for input...";
    
    private void OnValueChanged(string newValue)
    {
        otpValue = newValue; // Manual update when using ValueChanged
        statusMessage = $"Current: {newValue} (Length: {newValue.Length})";
    }
}
```

**Important:** When using `ValueChanged`, you must manually update the bound value:

```razor
<SfOtpInput Value="@otpValue"
            ValueChanged="OnValueChanged">
</SfOtpInput>

@code {
    private string otpValue = "";
    
    private void OnValueChanged(string newValue)
    {
        otpValue = newValue; // Manual update required
        // Additional logic here
    }
}
```

### Auto-Submit on Completion

```razor
@page "/auto-submit"
@using Syncfusion.Blazor.Inputs

<div class="auto-submit-demo">
    <h3>Email Verification</h3>
    <p>Enter 6-digit code - will auto-verify when complete</p>
    
    <SfOtpInput Value="@verificationCode"
                Length="6"
                ValueChanged="OnCodeChanged"
                Disabled="@isVerifying">
    </SfOtpInput>
    
    @if (!string.IsNullOrEmpty(message))
    {
        <div class="message @messageClass">@message</div>
    }
</div>

@code {
    private string verificationCode = "";
    private bool isVerifying = false;
    private string message = "";
    private string messageClass = "";
    
    private async void OnCodeChanged(string newValue)
    {
        verificationCode = newValue;
        
        // Auto-submit when complete
        if (newValue.Length == 6)
        {
            await VerifyCode(newValue);
        }
        else
        {
            message = "";
        }
        
        StateHasChanged();
    }
    
    private async Task VerifyCode(string code)
    {
        isVerifying = true;
        message = "Verifying...";
        messageClass = "info";
        StateHasChanged();
        
        await Task.Delay(1500); // Simulate API call
        
        // Replace with actual verification
        bool isValid = code == "123456";
        
        if (isValid)
        {
            message = "✓ Verification successful!";
            messageClass = "success";
        }
        else
        {
            message = "✗ Invalid code. Please try again.";
            messageClass = "error";
            verificationCode = ""; // Clear for retry
        }
        
        isVerifying = false;
        StateHasChanged();
    }
}

<style>
    .message {
        margin-top: 20px;
        padding: 12px;
        border-radius: 6px;
        font-weight: 500;
    }
    
    .message.success { background: #d1e7dd; color: #0f5132; }
    .message.error { background: #f8d7da; color: #842029; }
    .message.info { background: #cff4fc; color: #055160; }
</style>
```

### Real-Time Character Counter

```razor
@page "/otp-counter"
@using Syncfusion.Blazor.Inputs

<div class="counter-demo">
    <label>Enter OTP Code</label>
    
    <SfOtpInput Value="@otpValue"
                Length="6"
                ValueChanged="OnValueChanged">
    </SfOtpInput>
    
    <div class="counter">
        <span class="@(otpValue.Length == 6 ? "complete" : "")">
            @otpValue.Length / 6 digits entered
        </span>
    </div>
    
    @if (otpValue.Length == 6)
    {
        <p class="complete-msg">✓ Code complete - ready to verify</p>
    }
</div>

@code {
    private string otpValue = "";
    
    private void OnValueChanged(string newValue)
    {
        otpValue = newValue;
    }
}

<style>
    .counter {
        margin-top: 10px;
        font-size: 14px;
        color: #666;
    }
    
    .counter .complete {
        color: #28a745;
        font-weight: 600;
    }
    
    .complete-msg {
        color: #28a745;
        font-weight: 600;
        margin-top: 15px;
    }
</style>
```

---

## OnInput Event

The `OnInput` event fires on each character input, providing detailed information about the change.

### OnInput Event Structure

```razor
<SfOtpInput @bind-Value="@otpValue"
            Length="6"
            OnInput="HandleInput">
</SfOtpInput>

@code {
    private string otpValue = "";
    
    private void HandleInput(OtpInputEventArgs args)
    {
        Console.WriteLine($"Index: {args.Index}");
        Console.WriteLine($"Value: {args.Value}");
        Console.WriteLine($"Previous: {args.PreviousValue}");
    }
}
```

### OtpInputEventArgs Properties

| Property | Type | Description |
|----------|------|-------------|
| **Index** | int | Index of the input field (0-based) |
| **Value** | string | Complete OTP value after change |
| **PreviousValue** | string | OTP value before change |

### Tracking Input Progress

```razor
@page "/input-tracking"
@using Syncfusion.Blazor.Inputs

<div class="tracking-demo">
    <h3>OTP Input Tracking</h3>
    
    <SfOtpInput @bind-Value="@otpValue"
                Length="6"
                OnInput="TrackInput">
    </SfOtpInput>
    
    <div class="progress-indicator">
        @for (int i = 0; i < 6; i++)
        {
            <div class="progress-dot @(i < otpValue.Length ? "filled" : "")"></div>
        }
    </div>
    
    <div class="log">
        <h4>Input Log:</h4>
        @foreach (var log in inputLogs.TakeLast(5).Reverse())
        {
            <div class="log-entry">@log</div>
        }
    </div>
</div>

@code {
    private string otpValue = "";
    private List<string> inputLogs = new List<string>();
    
    private void TrackInput(OtpInputEventArgs args)
    {
        string action = args.Value.Length > args.PreviousValue.Length ? "Added" : "Removed";
        string log = $"{DateTime.Now:HH:mm:ss} - {action} at index {args.Index}. Value: '{args.Value}'";
        inputLogs.Add(log);
        StateHasChanged();
    }
}

<style>
    .progress-indicator {
        display: flex;
        gap: 8px;
        justify-content: center;
        margin: 20px 0;
    }
    
    .progress-dot {
        width: 12px;
        height: 12px;
        border-radius: 50%;
        background: #ddd;
        transition: background 0.3s;
    }
    
    .progress-dot.filled {
        background: #0d6efd;
    }
    
    .log {
        margin-top: 30px;
        padding: 15px;
        background: #f8f9fa;
        border-radius: 6px;
        font-family: monospace;
        font-size: 12px;
    }
    
    .log-entry {
        padding: 4px 0;
        border-bottom: 1px solid #e0e0e0;
    }
</style>
```

### Validation on Each Input

```razor
@page "/input-validation"
@using Syncfusion.Blazor.Inputs

<div class="validation-demo">
    <SfOtpInput @bind-Value="@otpValue"
                Length="6"
                OnInput="ValidateInput"
                CssClass="@validationClass">
    </SfOtpInput>
    
    @if (!string.IsNullOrEmpty(validationMessage))
    {
        <div class="validation-msg @validationClass">@validationMessage</div>
    }
</div>

@code {
    private string otpValue = "";
    private string validationMessage = "";
    private string validationClass = "";
    
    private void ValidateInput(OtpInputEventArgs args)
    {
        if (args.Value.Length == 6)
        {
            // Check if code matches expected pattern
            if (args.Value.All(char.IsDigit))
            {
                validationMessage = "✓ Valid format";
                validationClass = "valid";
            }
            else
            {
                validationMessage = "✗ Only numbers allowed";
                validationClass = "invalid";
            }
        }
        else
        {
            validationMessage = "";
            validationClass = "";
        }
    }
}

<style>
    .validation-msg {
        margin-top: 10px;
        padding: 8px;
        border-radius: 4px;
        font-size: 14px;
    }
    
    .valid { color: #28a745; background: #d4edda; }
    .invalid { color: #dc3545; background: #f8d7da; }
</style>
```

---

## OnFocus and OnBlur Events

The `OnFocus` and `OnBlur` events track when individual input fields gain or lose focus.

### Focus Event Handlers

```razor
<SfOtpInput @bind-Value="@otpValue"
            Length="6"
            OnFocus="HandleFocus"
            OnBlur="HandleBlur">
</SfOtpInput>

@code {
    private string otpValue = "";
    
    private void HandleFocus(OtpFocusInEventArgs args)
    {
        Console.WriteLine($"Focused on index: {args.Index}");
    }
    
    private void HandleBlur(OtpFocusOutEventArgs args)
    {
        Console.WriteLine($"Blurred from index: {args.Index}");
    }
}
```

### Event Arguments

**OtpFocusInEventArgs:**
- `Index` (int): Index of focused input field

**OtpFocusOutEventArgs:**
- `Index` (int): Index of blurred input field

### Focus Tracking Example

```razor
@page "/focus-tracking"
@using Syncfusion.Blazor.Inputs

<div class="focus-demo">
    <h3>Focus Tracking Demo</h3>
    
    <SfOtpInput @bind-Value="@otpValue"
                Length="6"
                OnFocus="OnFocusIn"
                OnBlur="OnFocusOut">
    </SfOtpInput>
    
    <div class="focus-info">
        <p>Currently focused: Input #{currentFocusIndex + 1}</p>
        <p>Last focused: Input #{lastFocusedIndex + 1}</p>
        <p>Total focus events: @focusCount</p>
    </div>
</div>

@code {
    private string otpValue = "";
    private int currentFocusIndex = -1;
    private int lastFocusedIndex = -1;
    private int focusCount = 0;
    
    private void OnFocusIn(OtpFocusInEventArgs args)
    {
        currentFocusIndex = args.Index;
        lastFocusedIndex = args.Index;
        focusCount++;
    }
    
    private void OnFocusOut(OtpFocusOutEventArgs args)
    {
        currentFocusIndex = -1;
    }
}

<style>
    .focus-info {
        margin-top: 20px;
        padding: 15px;
        background: #f0f0f0;
        border-radius: 6px;
    }
    
    .focus-info p {
        margin: 5px 0;
        font-family: monospace;
    }
</style>
```

### Show Hints on Focus

```razor
@page "/focus-hints"
@using Syncfusion.Blazor.Inputs

<div class="hints-demo">
    <SfOtpInput @bind-Value="@otpValue"
                Length="6"
                OnFocus="ShowHint"
                OnBlur="HideHint">
    </SfOtpInput>
    
    @if (showHint)
    {
        <div class="hint">
            <strong>Hint for digit #{focusedIndex + 1}:</strong>
            @GetHint(focusedIndex)
        </div>
    }
</div>

@code {
    private string otpValue = "";
    private bool showHint = false;
    private int focusedIndex = 0;
    
    private void ShowHint(OtpFocusInEventArgs args)
    {
        focusedIndex = args.Index;
        showHint = true;
    }
    
    private void HideHint(OtpFocusOutEventArgs args)
    {
        showHint = false;
    }
    
    private string GetHint(int index)
    {
        return index switch
        {
            0 => "Enter the first digit from your code",
            5 => "Enter the last digit to complete",
            _ => $"Enter digit number {index + 1}"
        };
    }
}

<style>
    .hint {
        margin-top: 15px;
        padding: 12px;
        background: #fff3cd;
        border-left: 4px solid #ffc107;
        border-radius: 4px;
    }
</style>
```

---

## Created Lifecycle Event

The `Created` event fires once when the component is fully rendered and initialized.

### Basic Created Handler

```razor
<SfOtpInput @bind-Value="@otpValue"
            Length="6"
            Created="OnCreated">
</SfOtpInput>

@code {
    private string otpValue = "";
    
    private void OnCreated(object args)
    {
        Console.WriteLine("OtpInput component created and ready");
        // Initialize logic here
    }
}
```

### Initialization Logic

```razor
@page "/otp-init"
@using Syncfusion.Blazor.Inputs

<div class="init-demo">
    <h3>@pageTitle</h3>
    
    <SfOtpInput @bind-Value="@otpValue"
                Length="6"
                AutoFocus="@autoFocus"
                Created="OnComponentCreated">
    </SfOtpInput>
    
    <p class="status">@statusMessage</p>
</div>

@code {
    private string otpValue = "";
    private string pageTitle = "Loading...";
    private bool autoFocus = false;
    private string statusMessage = "";
    
    private async void OnComponentCreated(object args)
    {
        // Fetch session data or configuration
        await Task.Delay(500);
        
        pageTitle = "Enter Verification Code";
        autoFocus = true;
        statusMessage = "Component initialized. Start typing...";
        
        StateHasChanged();
    }
}
```

---

## Event Argument Structures

### Complete Event Args Reference

```csharp
// OtpInputEventArgs - OnInput event
public class OtpInputEventArgs
{
    public int Index { get; set; }          // Input field index (0-based)
    public string Value { get; set; }       // Current complete OTP value
    public string PreviousValue { get; set; } // Previous OTP value
}

// OtpFocusInEventArgs - OnFocus event
public class OtpFocusInEventArgs
{
    public int Index { get; set; }  // Focused input field index
}

// OtpFocusOutEventArgs - OnBlur event
public class OtpFocusOutEventArgs
{
    public int Index { get; set; }  // Blurred input field index
}
```

---

## Form Validation Integration

### Integration with EditForm

```razor
@page "/otp-form"
@using Syncfusion.Blazor.Inputs
@using System.ComponentModel.DataAnnotations

<EditForm Model="@verificationModel" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    
    <div class="form-group">
        <label>Email Verification Code</label>
        <SfOtpInput @bind-Value="@verificationModel.Code"
                    Length="6"
                    Type="OtpInputType.Number">
        </SfOtpInput>
        <ValidationMessage For="@(() => verificationModel.Code)" />
    </div>
    
    <button type="submit" class="btn-submit">Verify</button>
</EditForm>

@code {
    private VerificationModel verificationModel = new VerificationModel();
    
    private async Task HandleValidSubmit()
    {
        // Process valid OTP
        await Task.Delay(1000);
        // Success logic
    }
    
    public class VerificationModel
    {
        [Required(ErrorMessage = "Verification code is required")]
        [StringLength(6, MinimumLength = 6, ErrorMessage = "Code must be 6 digits")]
        [RegularExpression(@"^\d{6}$", ErrorMessage = "Code must contain only numbers")]
        public string Code { get; set; } = "";
    }
}
```

### Custom Validation

```razor
@page "/custom-validation"
@using Syncfusion.Blazor.Inputs

<EditForm Model="@model" OnValidSubmit="Submit">
    <div class="validation-container">
        <label>Two-Factor Code</label>
        
        <SfOtpInput @bind-Value="@model.TwoFactorCode"
                    Length="6"
                    ValueChanged="ValidateCode">
        </SfOtpInput>
        
        @if (!string.IsNullOrEmpty(customValidationError))
        {
            <div class="validation-error">@customValidationError</div>
        }
    </div>
    
    <button type="submit" disabled="@(!isCodeValid)">
        Continue
    </button>
</EditForm>

@code {
    private AuthModel model = new AuthModel();
    private string customValidationError = "";
    private bool isCodeValid = false;
    
    private void ValidateCode(string value)
    {
        model.TwoFactorCode = value;
        
        if (value.Length < 6)
        {
            customValidationError = "";
            isCodeValid = false;
        }
        else if (!value.All(char.IsDigit))
        {
            customValidationError = "Code must contain only numbers";
            isCodeValid = false;
        }
        else if (value.StartsWith("0"))
        {
            customValidationError = "Invalid code format";
            isCodeValid = false;
        }
        else
        {
            customValidationError = "";
            isCodeValid = true;
        }
    }
    
    private void Submit()
    {
        // Process valid code
    }
    
    public class AuthModel
    {
        public string TwoFactorCode { get; set; } = "";
    }
}

<style>
    .validation-error {
        color: #dc3545;
        font-size: 14px;
        margin-top: 8px;
    }
</style>
```

---

## Real-Time Verification Patterns

### Debounced Verification

```razor
@page "/debounced-verify"
@using Syncfusion.Blazor.Inputs
@using System.Threading

<div class="debounced-demo">
    <SfOtpInput Value="@otpValue"
                Length="6"
                ValueChanged="OnValueChangedDebounced"
                Disabled="@isVerifying">
    </SfOtpInput>
    
    @if (isVerifying)
    {
        <p class="status">Verifying...</p>
    }
    else if (!string.IsNullOrEmpty(result))
    {
        <p class="status @resultClass">@result</p>
    }
</div>

@code {
    private string otpValue = "";
    private bool isVerifying = false;
    private string result = "";
    private string resultClass = "";
    private CancellationTokenSource? cts;
    
    private async void OnValueChangedDebounced(string newValue)
    {
        otpValue = newValue;
        result = "";
        
        if (newValue.Length == 6)
        {
            // Cancel previous verification
            cts?.Cancel();
            cts = new CancellationTokenSource();
            
            try
            {
                // Wait 800ms before verifying
                await Task.Delay(800, cts.Token);
                
                isVerifying = true;
                StateHasChanged();
                
                // Simulate API call
                await Task.Delay(1000, cts.Token);
                
                bool isValid = newValue == "123456";
                
                if (isValid)
                {
                    result = "✓ Code verified!";
                    resultClass = "success";
                }
                else
                {
                    result = "✗ Invalid code";
                    resultClass = "error";
                    otpValue = "";
                }
                
                isVerifying = false;
                StateHasChanged();
            }
            catch (TaskCanceledException)
            {
                // Verification cancelled
            }
        }
    }
}

<style>
    .status.success { color: #28a745; }
    .status.error { color: #dc3545; }
</style>
```

### Progressive Verification (Check as you type)

```razor
@page "/progressive-verify"
@using Syncfusion.Blazor.Inputs

<div class="progressive-demo">
    <SfOtpInput @bind-Value="@otpValue"
                Length="6"
                OnInput="CheckProgressively">
    </SfOtpInput>
    
    <div class="digit-status">
        @for (int i = 0; i < 6; i++)
        {
            <div class="digit-indicator @GetDigitStatus(i)">
                @(i + 1)
            </div>
        }
    </div>
</div>

@code {
    private string otpValue = "";
    private string expectedCode = "123456";
    
    private void CheckProgressively(OtpInputEventArgs args)
    {
        // Check each digit as it's entered
        StateHasChanged();
    }
    
    private string GetDigitStatus(int index)
    {
        if (index >= otpValue.Length)
            return "pending";
        
        return otpValue[index] == expectedCode[index] ? "correct" : "incorrect";
    }
}

<style>
    .digit-status {
        display: flex;
        gap: 10px;
        justify-content: center;
        margin-top: 20px;
    }
    
    .digit-indicator {
        width: 30px;
        height: 30px;
        display: flex;
        align-items: center;
        justify-content: center;
        border-radius: 50%;
        font-weight: 600;
        font-size: 12px;
    }
    
    .digit-indicator.pending { background: #e0e0e0; color: #999; }
    .digit-indicator.correct { background: #d4edda; color: #0f5132; }
    .digit-indicator.incorrect { background: #f8d7da; color: #842029; }
</style>
```

---

## Auto-Submit on Completion

Complete example combining all event patterns for auto-verification:

```razor
@page "/auto-verify-complete"
@using Syncfusion.Blazor.Inputs

<div class="complete-auto-verify">
    <h2>Two-Factor Authentication</h2>
    <p class="instruction">Enter the 6-digit code from your authenticator app</p>
    
    <SfOtpInput Value="@twoFactorCode"
                Length="6"
                Type="OtpInputType.Number"
                AutoFocus="true"
                Disabled="@isProcessing"
                ValueChanged="OnCodeComplete"
                CssClass="@statusClass">
    </SfOtpInput>
    
    @if (!string.IsNullOrEmpty(statusMessage))
    {
        <div class="status-box @statusClass">
            @statusMessage
        </div>
    }
    
    @if (attempts > 0)
    {
        <p class="attempts">Attempts: @attempts / 3</p>
    }
</div>

@code {
    private string twoFactorCode = "";
    private bool isProcessing = false;
    private string statusMessage = "";
    private string statusClass = "";
    private int attempts = 0;
    
    private async void OnCodeComplete(string value)
    {
        twoFactorCode = value;
        
        if (value.Length == 6)
        {
            await VerifyCodeAutomatically(value);
        }
        
        StateHasChanged();
    }
    
    private async Task VerifyCodeAutomatically(string code)
    {
        isProcessing = true;
        statusMessage = "Verifying code...";
        statusClass = "verifying";
        StateHasChanged();
        
        await Task.Delay(1500); // Simulate API call
        
        attempts++;
        
        // Replace with actual verification logic
        bool isValid = code == "123456";
        
        if (isValid)
        {
            statusMessage = "✓ Authentication successful! Redirecting...";
            statusClass = "success";
            
            await Task.Delay(1000);
            // Navigation logic here
        }
        else
        {
            statusMessage = $"✗ Invalid code. Please try again. ({3 - attempts} attempts remaining)";
            statusClass = "error";
            twoFactorCode = "";
            
            if (attempts >= 3)
            {
                statusMessage = "✗ Too many failed attempts. Account locked.";
                // Lock account logic
            }
        }
        
        isProcessing = false;
        StateHasChanged();
    }
}

<style>
    .complete-auto-verify {
        max-width: 500px;
        margin: 50px auto;
        padding: 40px;
        background: white;
        border-radius: 12px;
        box-shadow: 0 4px 20px rgba(0,0,0,0.1);
        text-align: center;
    }
    
    .instruction {
        color: #666;
        margin: 15px 0 40px;
    }
    
    .status-box {
        margin-top: 25px;
        padding: 15px;
        border-radius: 8px;
        font-weight: 500;
        animation: fadeIn 0.3s;
    }
    
    .status-box.verifying {
        background: #cff4fc;
        color: #055160;
    }
    
    .status-box.success {
        background: #d1e7dd;
        color: #0f5132;
    }
    
    .status-box.error {
        background: #f8d7da;
        color: #842029;
        animation: shake 0.4s;
    }
    
    .attempts {
        margin-top: 15px;
        color: #666;
        font-size: 14px;
    }
    
    @keyframes fadeIn {
        from { opacity: 0; transform: translateY(-10px); }
        to { opacity: 1; transform: translateY(0); }
    }
    
    @keyframes shake {
        0%, 100% { transform: translateX(0); }
        25% { transform: translateX(-10px); }
        75% { transform: translateX(10px); }
    }
</style>
```

This comprehensive guide covers all event handling and data binding scenarios for building robust OTP verification flows.
