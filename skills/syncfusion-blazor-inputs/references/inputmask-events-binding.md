# InputMask Events and Data Binding

## Table of Contents
- [Value Property and Two-Way Binding](#value-property-and-two-way-binding)
- [ValueChange Event](#valuechange-event)
- [ValueChanged Event](#valuechanged-event)
- [Focus and Blur Events](#focus-and-blur-events)
- [OnChange vs OnInput vs ValueChange](#onchange-vs-oninput-vs-valuechange)
- [EditForm Integration](#editform-integration)
- [Real-Time Validation Patterns](#real-time-validation-patterns)
- [Event Handler Best Practices](#event-handler-best-practices)

## Value Property and Two-Way Binding

### Basic Two-Way Binding

Use `@bind-Value` for automatic two-way data binding:

```razor
<SfMaskedTextBox Mask="(000) 000-0000" @bind-Value="@phoneNumber"></SfMaskedTextBox>

<p>Current Value: @phoneNumber</p>

@code {
    private string phoneNumber = "";
}
```

### Pre-Populating Values

```razor
<SfMaskedTextBox Mask="000-00-0000" @bind-Value="@ssn"></SfMaskedTextBox>

@code {
    private string ssn = "123456789"; // Will display as: 123-45-6789
    
    protected override void OnInitialized()
    {
        // Or load from service/API
        ssn = UserService.GetSSN();
    }
}
```

### Programmatic Value Updates

```razor
<SfMaskedTextBox Mask="(000) 000-0000" @bind-Value="@phone"></SfMaskedTextBox>

<button @onclick="SetDefaultPhone">Use Default</button>
<button @onclick="ClearPhone">Clear</button>

@code {
    private string phone = "";
    
    private void SetDefaultPhone()
    {
        phone = "5551234567";  // Will display as: (555) 123-4567
    }
    
    private void ClearPhone()
    {
        phone = "";
    }
}
```

## ValueChange Event

The `ValueChange` event fires when the mask value changes. It provides `MaskChangeEventArgs` with detailed information about the change.

### Basic ValueChange Handler

```razor
<SfMaskedTextBox Mask="000-00-0000" 
                 @bind-Value="@ssn"
                 ValueChange="@OnValueChanged">
</SfMaskedTextBox>

<p>Status: @status</p>

@code {
    private string ssn = "";
    private string status = "";
    
    private void OnValueChanged(MaskChangeEventArgs args)
    {
        status = $"'{args.Value}'";
        
        // Access properties
        Console.WriteLine($"Value: {args.Value}");
        Console.WriteLine($"IsInteracted: {args.IsInteracted}");
    }
}
```

### MaskChangeEventArgs Properties

```csharp
public class MaskChangeEventArgs
{
    public string Value { get; set; }           // Current mask value
    public bool IsInteracted { get; set; }      // True if user-initiated
    public string MaskedValue { get; set; }     // Value with mask format
}
```

### Real-Time Validation with ValueChange

```razor
<SfMaskedTextBox Mask="(000) 000-0000" 
                 @bind-Value="@phone"
                 ValueChange="@ValidatePhone">
</SfMaskedTextBox>

@if (!string.IsNullOrEmpty(validationMessage))
{
    <div class="validation-message @(isValid ? "valid" : "invalid")">
        @validationMessage
    </div>
}

@code {
    private string phone = "";
    private string validationMessage = "";
    private bool isValid = false;
    
    private void ValidatePhone(MaskChangeEventArgs args)
    {
        // Check if complete (no prompt characters)
        if (string.IsNullOrEmpty(args.Value) || args.Value.Contains('_'))
        {
            validationMessage = "Please complete the phone number";
            isValid = false;
        }
        else
        {
            // Additional business logic validation
            string digitsOnly = args.Value.Replace("(", "").Replace(")", "")
                                          .Replace(" ", "").Replace("-", "");
            
            if (digitsOnly.StartsWith("000") || digitsOnly.StartsWith("555"))
            {
                validationMessage = "Invalid phone number";
                isValid = false;
            }
            else
            {
                validationMessage = "✓ Valid phone number";
                isValid = true;
            }
        }
    }
}

<style>
    .validation-message {
        margin-top: 5px;
        font-size: 14px;
    }
    
    .validation-message.valid {
        color: green;
    }
    
    .validation-message.invalid {
        color: red;
    }
</style>
```

## ValueChanged Event

The `ValueChanged` event is the standard Blazor two-way binding callback. It fires with just the new value as a string.

### Basic ValueChanged

```razor
<SfMaskedTextBox Mask="000-00-0000" 
                 Value="@ssn"
                 ValueChanged="@((string value) => OnSSNChanged(value))">
</SfMaskedTextBox>

<p>SSN: @ssn</p>
<p>Last 4: @lastFour</p>

@code {
    private string ssn = "";
    private string lastFour = "";
    
    private void OnSSNChanged(string value)
    {
        ssn = value;
        
        // Extract last 4 digits
        if (!string.IsNullOrEmpty(value) && value.Length >= 4)
        {
            lastFour = value.Substring(value.Length - 4);
        }
    }
}
```

### ValueChanged for Async Operations

```razor
<SfMaskedTextBox Mask="00000" 
                 Value="@zipCode"
                 ValueChanged="@OnZipChanged">
</SfMaskedTextBox>

<p>City: @city, State: @state</p>

@code {
    private string zipCode = "";
    private string city = "";
    private string state = "";
    
    private async Task OnZipChanged(string value)
    {
        zipCode = value;
        
        // Check if complete
        if (!string.IsNullOrEmpty(value) && value.Length == 5 && !value.Contains('_'))
        {
            // Lookup city/state from API
            var location = await LocationService.GetByZipCode(value);
            city = location.City;
            state = location.State;
        }
        else
        {
            city = "";
            state = "";
        }
    }
}
```

## Focus and Blur Events

### Focus Event

Fires when the input receives focus:

```razor
<SfMaskedTextBox Mask="(000) 000-0000" 
                 @bind-Value="@phone"
                 Focus="@OnFocus">
</SfMaskedTextBox>

<p>@focusStatus</p>

@code {
    private string phone = "";
    private string focusStatus = "";
    
    private void OnFocus(MaskFocusEventArgs args)
    {
        focusStatus = "Field is focused";
        Console.WriteLine($"Focused with value: {args.Value}");
    }
}
```

### Blur Event

Fires when the input loses focus:

```razor
<SfMaskedTextBox Mask="000-00-0000" 
                 @bind-Value="@ssn"
                 Blur="@OnBlur">
</SfMaskedTextBox>

<p>@blurMessage</p>

@code {
    private string ssn = "";
    private string blurMessage = "";
    
    private void OnBlur(MaskBlurEventArgs args)
    {
        blurMessage = "Field lost focus";
        
        // Validate on blur
        if (string.IsNullOrEmpty(args.Value) || args.Value.Contains('_'))
        {
            blurMessage = "⚠ Please complete the SSN";
        }
        else
        {
            blurMessage = "✓ SSN saved";
        }
    }
}
```

### Combined Focus/Blur for Validation

```razor
<SfMaskedTextBox Mask="0000-0000-0000-0000" 
                 @bind-Value="@cardNumber"
                 Focus="@OnCardFocus"
                 Blur="@OnCardBlur">
</SfMaskedTextBox>

@if (showValidation)
{
    <div class="validation">@validationMsg</div>
}

@code {
    private string cardNumber = "";
    private bool showValidation = false;
    private string validationMsg = "";
    
    private void OnCardFocus(MaskFocusEventArgs args)
    {
        // Hide validation while typing
        showValidation = false;
    }
    
    private void OnCardBlur(MaskBlurEventArgs args)
    {
        // Show validation after leaving field
        showValidation = true;
        
        if (string.IsNullOrEmpty(args.Value) || args.Value.Contains('_'))
        {
            validationMsg = "Please enter complete card number";
        }
        else
        {
            // Perform Luhn algorithm check
            bool isValid = ValidateCreditCard(args.Value);
            validationMsg = isValid ? "✓ Valid card number" : "✗ Invalid card number";
        }
    }
    
    private bool ValidateCreditCard(string cardNumber)
    {
        // Luhn algorithm implementation
        string digitsOnly = cardNumber.Replace("-", "");
        // ... validation logic
        return true; // Simplified
    }
}
```

## OnChange vs OnInput vs ValueChange

Understanding the timing and differences between these events:

### Event Timing Comparison

```razor
<SfMaskedTextBox Mask="000-000" 
                 @bind-Value="@value"
                 OnInput="@OnInputHandler"
                 OnChange="@OnChangeHandler"
                 ValueChange="@OnValueChangeHandler">
</SfMaskedTextBox>

<div class="event-log">
    <h4>Event Log:</h4>
    <ul>
        @foreach (var log in eventLog)
        {
            <li>@log</li>
        }
    </ul>
</div>

@code {
    private string value = "";
    private List<string> eventLog = new List<string>();
    
    private void OnInputHandler(ChangeEventArgs args)
    {
        eventLog.Add($"OnInput: {args.Value} (fires on each keystroke)");
    }
    
    private void OnChangeHandler(ChangeEventArgs args)
    {
        eventLog.Add($"OnChange: {args.Value} (fires on blur if changed)");
    }
    
    private void OnValueChangeHandler(MaskChangeEventArgs args)
    {
        eventLog.Add($"ValueChange: {args.Value} → {args.IsInteracted} (fires on valid change)");
    }
}
```

### When to Use Each Event

| Event | Timing | Use Case |
|-------|--------|----------|
| **OnInput** | Every keystroke | Real-time character counting, live search |
| **ValueChange** | Valid mask change | Mask-specific validation, format checks |
| **OnChange** | Blur (if changed) | Final validation, API calls |
| **Blur** | Loses focus | Save draft, show validation message |
| **Focus** | Gains focus | Hide errors, show hints |

### Pattern: Real-Time Feedback + Final Validation

```razor
<SfMaskedTextBox Mask="(000) 000-0000" 
                 @bind-Value="@phone"
                 ValueChange="@OnPhoneInput"
                 Blur="@OnPhoneBlur">
</SfMaskedTextBox>

<div class="feedback">
    @if (isTyping)
    {
        <span class="info">@liveMessage</span>
    }
    else if (!string.IsNullOrEmpty(finalMessage))
    {
        <span class="@(isValidFinal ? "success" : "error")">@finalMessage</span>
    }
</div>

@code {
    private string phone = "";
    private bool isTyping = false;
    private string liveMessage = "";
    private string finalMessage = "";
    private bool isValidFinal = false;
    
    private void OnPhoneInput(MaskChangeEventArgs args)
    {
        isTyping = true;
        finalMessage = "";
        
        // Live feedback while typing
        int digitCount = args.Value.Replace("(", "").Replace(")", "")
                                   .Replace(" ", "").Replace("-", "")
                                   .Replace("_", "").Length;
        liveMessage = $"{digitCount}/10 digits entered";
    }
    
    private void OnPhoneBlur(MaskBlurEventArgs args)
    {
        isTyping = false;
        liveMessage = "";
        
        // Final validation on blur
        if (string.IsNullOrEmpty(args.Value) || args.Value.Contains('_'))
        {
            finalMessage = "Please complete phone number";
            isValidFinal = false;
        }
        else
        {
            finalMessage = "✓ Phone number saved";
            isValidFinal = true;
        }
    }
}
```

## EditForm Integration

### Basic Form Integration

```razor
<EditForm Model="@contactModel" OnValidSubmit="@HandleSubmit">
    <DataAnnotationsValidator />
    
    <div class="form-group">
        <label>Phone Number:</label>
        <SfMaskedTextBox Mask="(000) 000-0000" 
                         @bind-Value="@contactModel.Phone"
                         ValueExpression="@(() => contactModel.Phone)">
        </SfMaskedTextBox>
        <ValidationMessage For="@(() => contactModel.Phone)" />
    </div>
    
    <div class="form-group">
        <label>SSN:</label>
        <SfMaskedTextBox Mask="000-00-0000" 
                         @bind-Value="@contactModel.SSN"
                         ValueExpression="@(() => contactModel.SSN)">
        </SfMaskedTextBox>
        <ValidationMessage For="@(() => contactModel.SSN)" />
    </div>
    
    <button type="submit" class="btn btn-primary">Submit</button>
</EditForm>

@code {
    private ContactModel contactModel = new ContactModel();
    
    private void HandleSubmit()
    {
        // Form is valid, process data
        Console.WriteLine($"Phone: {contactModel.Phone}");
        Console.WriteLine($"SSN: {contactModel.SSN}");
    }
    
    public class ContactModel
    {
        [Required(ErrorMessage = "Phone number is required")]
        [StringLength(14, MinimumLength = 14, ErrorMessage = "Phone must be complete")]
        public string Phone { get; set; }
        
        [Required(ErrorMessage = "SSN is required")]
        [RegularExpression(@"^\d{3}-\d{2}-\d{4}$", ErrorMessage = "Invalid SSN format")]
        public string SSN { get; set; }
    }
}
```

### Custom Validation Attributes

```csharp
public class ValidPhoneNumberAttribute : ValidationAttribute
{
    protected override ValidationResult IsValid(object value, ValidationContext validationContext)
    {
        if (value == null) return ValidationResult.Success;
        
        string phone = value.ToString();
        
        // Check if complete
        if (phone.Contains('_'))
            return new ValidationResult("Phone number is incomplete");
        
        // Extract digits
        string digitsOnly = new string(phone.Where(char.IsDigit).ToArray());
        
        // Check length
        if (digitsOnly.Length != 10)
            return new ValidationResult("Phone number must be 10 digits");
        
        // Business rules
        if (digitsOnly.StartsWith("000") || digitsOnly.StartsWith("555"))
            return new ValidationResult("Invalid phone number");
        
        return ValidationResult.Success;
    }
}

// Usage:
public class ContactModel
{
    [Required]
    [ValidPhoneNumber]
    public string Phone { get; set; }
}
```

### EditContext for Manual Validation

```razor
<EditForm EditContext="@editContext" OnValidSubmit="@HandleSubmit">
    <div class="form-group">
        <label>Credit Card:</label>
        <SfMaskedTextBox Mask="0000-0000-0000-0000" 
                         @bind-Value="@paymentModel.CardNumber"
                         ValueChange="@OnCardChanged"
                         ValueExpression="@(() => paymentModel.CardNumber)">
        </SfMaskedTextBox>
        <ValidationMessage For="@(() => paymentModel.CardNumber)" />
    </div>
    
    <button type="submit">Submit</button>
</EditForm>

@code {
    private PaymentModel paymentModel = new PaymentModel();
    private EditContext editContext;
    
    protected override void OnInitialized()
    {
        editContext = new EditContext(paymentModel);
    }
    
    private void OnCardChanged(MaskChangeEventArgs args)
    {
        // Manual validation trigger
        var messages = new ValidationMessageStore(editContext);
        
        if (args.Value.Contains('_'))
        {
            messages.Add(() => paymentModel.CardNumber, "Card number is incomplete");
        }
        else if (!ValidateLuhn(args.Value))
        {
            messages.Add(() => paymentModel.CardNumber, "Invalid card number");
        }
        else
        {
            messages.Clear();
        }
        
        editContext.NotifyValidationStateChanged();
    }
    
    private bool ValidateLuhn(string cardNumber)
    {
        // Luhn algorithm
        string digitsOnly = cardNumber.Replace("-", "");
        // ... validation logic
        return true;
    }
    
    public class PaymentModel
    {
        [Required]
        public string CardNumber { get; set; }
    }
}
```

## Real-Time Validation Patterns

### Pattern 1: Character Count Display

```razor
<SfMaskedTextBox Mask="AAAAAAAAAA" 
                 @bind-Value="@codeValue"
                 ValueChange="@OnCodeChanged">
</SfMaskedTextBox>

<div class="char-count">
    <span class="@(charCount == 10 ? "complete" : "incomplete")">
        @charCount / 10 characters
    </span>
</div>

@code {
    private string codeValue = "";
    private int charCount = 0;
    
    private void OnCodeChanged(MaskChangeEventArgs args)
    {
        charCount = args.Value.Replace("_", "").Length;
    }
}
```

### Pattern 2: Format Indicator

```razor
<SfMaskedTextBox Mask="00/00/0000" 
                 @bind-Value="@date"
                 ValueChange="@OnDateChanged">
</SfMaskedTextBox>

<div class="format-indicator">
    <span class="indicator @monthStatus">MM</span> /
    <span class="indicator @dayStatus">DD</span> /
    <span class="indicator @yearStatus">YYYY</span>
</div>

@code {
    private string date = "";
    private string monthStatus = "empty";
    private string dayStatus = "empty";
    private string yearStatus = "empty";
    
    private void OnDateChanged(MaskChangeEventArgs args)
    {
        var parts = args.Value.Split('/');
        
        monthStatus = parts[0].Contains('_') ? "empty" : "filled";
        dayStatus = parts.Length > 1 && !parts[1].Contains('_') ? "filled" : "empty";
        yearStatus = parts.Length > 2 && !parts[2].Contains('_') ? "filled" : "empty";
    }
}

<style>
    .indicator { padding: 2px 5px; border-radius: 3px; }
    .indicator.empty { background: #eee; color: #999; }
    .indicator.filled { background: #4CAF50; color: white; }
</style>
```

### Pattern 3: Async Lookup Validation

```razor
<SfMaskedTextBox Mask="00000" 
                 @bind-Value="@zipCode"
                 ValueChange="@OnZipChanged">
</SfMaskedTextBox>

@if (isValidating)
{
    <span class="loading">Validating...</span>
}
else if (!string.IsNullOrEmpty(lookupResult))
{
    <div class="result @resultClass">@lookupResult</div>
}

@code {
    private string zipCode = "";
    private bool isValidating = false;
    private string lookupResult = "";
    private string resultClass = "";
    
    private CancellationTokenSource cts;
    
    private async Task OnZipChanged(MaskChangeEventArgs args)
    {
        // Cancel previous validation
        cts?.Cancel();
        
        // Clear results
        lookupResult = "";
        isValidating = false;
        
        // Check if complete
        if (args.Value.Length == 5 && !args.Value.Contains('_'))
        {
            isValidating = true;
            cts = new CancellationTokenSource();
            
            try
            {
                // Debounce
                await Task.Delay(500, cts.Token);
                
                // API lookup
                var location = await LocationService.LookupZipCode(args.Value);
                
                if (location != null)
                {
                    lookupResult = $"✓ {location.City}, {location.State}";
                    resultClass = "valid";
                }
                else
                {
                    lookupResult = "✗ Invalid ZIP code";
                    resultClass = "invalid";
                }
            }
            catch (TaskCanceledException)
            {
                // Validation cancelled
            }
            finally
            {
                isValidating = false;
                StateHasChanged();
            }
        }
    }
}
```

## Event Handler Best Practices

### 1. Avoid Heavy Processing in Events

```csharp
// ❌ Bad: Expensive operation on every keystroke
private void OnValueChanged(MaskChangeEventArgs args)
{
    // Don't do this on every change
    var result = ExpensiveAPICall(args.Value);
}

// ✅ Good: Validate completeness first, then debounce
private async Task OnValueChanged(MaskChangeEventArgs args)
{
    if (!args.Value.Contains('_'))
    {
        await Task.Delay(500); // Debounce
        var result = await ExpensiveAPICall(args.Value);
    }
}
```

### 2. Use Async for I/O Operations

```csharp
// ✅ Good: Async for API calls
private async Task OnZipChanged(string value)
{
    if (value.Length == 5)
    {
        var location = await LocationService.GetByZipCode(value);
        // Process result
    }
}
```

### 3. Validate Before Processing

```csharp
private void OnValueChanged(MaskChangeEventArgs args)
{
    // Check if input is complete
    if (string.IsNullOrEmpty(args.Value) || args.Value.Contains('_'))
    {
        return; // Don't process incomplete input
    }
    
    // Now safe to process
    ProcessValue(args.Value);
}
```

### 4. Handle Exceptions

```csharp
private async Task OnPhoneChanged(string value)
{
    try
    {
        if (value.Length == 14)
        {
            await ValidatePhoneNumber(value);
        }
    }
    catch (HttpRequestException ex)
    {
        errorMessage = "Unable to validate. Please try again.";
        Logger.LogError(ex, "Phone validation failed");
    }
}
```

### 5. Clean Up Resources

```csharp
private CancellationTokenSource cts;

private async Task OnValueChanged(MaskChangeEventArgs args)
{
    cts?.Cancel();
    cts = new CancellationTokenSource();
    
    try
    {
        await ValidationService.Validate(args.Value, cts.Token);
    }
    catch (TaskCanceledException)
    {
        // Expected when new input arrives
    }
}

public void Dispose()
{
    cts?.Cancel();
    cts?.Dispose();
}
```

## Complete Example

```razor
@page "/masked-input-events"
@using Syncfusion.Blazor.Inputs
@implements IDisposable

<EditForm EditContext="@editContext" OnValidSubmit="@HandleSubmit">
    <DataAnnotationsValidator />
    
    <div class="form-group">
        <label>Phone Number:</label>
        <SfMaskedTextBox Mask="(000) 000-0000" 
                         @bind-Value="@model.Phone"
                         ValueExpression="@(() => model.Phone)"
                         ValueChange="@OnPhoneChanged"
                         Focus="@OnPhoneFocus"
                         Blur="@OnPhoneBlur">
        </SfMaskedTextBox>
        
        @if (showPhoneValidation)
        {
            <div class="@phoneValidationClass">@phoneValidation</div>
        }
        <ValidationMessage For="@(() => model.Phone)" />
    </div>
    
    <button type="submit" disabled="@(!isFormValid)">Submit</button>
</EditForm>

@code {
    private FormModel model = new FormModel();
    private EditContext editContext;
    private bool isFormValid = false;
    private bool showPhoneValidation = false;
    private string phoneValidation = "";
    private string phoneValidationClass = "";
    
    protected override void OnInitialized()
    {
        editContext = new EditContext(model);
        editContext.OnValidationStateChanged += ValidationStateChanged;
    }
    
    private void ValidationStateChanged(object sender, ValidationStateChangedEventArgs e)
    {
        isFormValid = !editContext.GetValidationMessages().Any();
    }
    
    private void OnPhoneChanged(MaskChangeEventArgs args)
    {
        showPhoneValidation = false;
    }
    
    private void OnPhoneFocus(MaskFocusEventArgs args)
    {
        showPhoneValidation = false;
    }
    
    private void OnPhoneBlur(MaskBlurEventArgs args)
    {
        showPhoneValidation = true;
        
        if (string.IsNullOrEmpty(args.Value) || args.Value.Contains('_'))
        {
            phoneValidation = "Please complete the phone number";
            phoneValidationClass = "invalid";
        }
        else
        {
            phoneValidation = "✓ Valid phone number";
            phoneValidationClass = "valid";
        }
    }
    
    private void HandleSubmit()
    {
        Console.WriteLine($"Submitted: {model.Phone}");
    }
    
    public void Dispose()
    {
        if (editContext != null)
        {
            editContext.OnValidationStateChanged -= ValidationStateChanged;
        }
    }
    
    public class FormModel
    {
        [Required(ErrorMessage = "Phone is required")]
        public string Phone { get; set; }
    }
}
```
