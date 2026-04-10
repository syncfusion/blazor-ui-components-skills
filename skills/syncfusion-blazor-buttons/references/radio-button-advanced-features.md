# Radio Button Advanced Features

## Table of Contents
- [ValueChange Event](#valuechange-event)
- [ChangeArgs Event Data](#changeargs-event-data)
- [CheckedChanged Callback](#checkedchanged-callback)
- [Created Lifecycle Event](#created-lifecycle-event)
- [Combining Multiple Events](#combining-multiple-events)
- [Event Handling Patterns](#event-handling-patterns)
- [Common Event Scenarios](#common-event-scenarios)
- [Accessibility Features](#accessibility-features)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

---

## ValueChange Event

The `ValueChange` event fires when the user selects a different radio button in the group.

### Basic ValueChange Usage

```razor
@using Syncfusion.Blazor.Buttons

<h4>Select Your Plan</h4>

<SfRadioButton Label="Free Plan" 
               Name="plan" 
               Value="free" 
               @bind-Checked="@selectedPlan"
               ValueChange="@OnPlanChanged">
</SfRadioButton>

<SfRadioButton Label="Pro Plan ($9.99/mo)" 
               Name="plan" 
               Value="pro" 
               @bind-Checked="@selectedPlan"
               ValueChange="@OnPlanChanged">
</SfRadioButton>

<SfRadioButton Label="Enterprise Plan ($29.99/mo)" 
               Name="plan" 
               Value="enterprise" 
               @bind-Checked="@selectedPlan"
               ValueChange="@OnPlanChanged">
</SfRadioButton>

<p>@message</p>

@code {
    private string selectedPlan = "free";
    private string message = "";
    
    private void OnPlanChanged(ChangeArgs<string> args)
    {
        message = $"Plan changed to: {args.Value}";
        Console.WriteLine($"ValueChange event fired: {args.Value}");
    }
}
```

### ValueChange with API Call

```razor
@using Syncfusion.Blazor.Buttons

<h4>Language Preference</h4>

<SfRadioButton Label="English" 
               Name="language" 
               Value="en" 
               @bind-Checked="@language"
               ValueChange="@OnLanguageChanged">
</SfRadioButton>

<SfRadioButton Label="Spanish" 
               Name="language" 
               Value="es" 
               @bind-Checked="@language"
               ValueChange="@OnLanguageChanged">
</SfRadioButton>

<SfRadioButton Label="French" 
               Name="language" 
               Value="fr" 
               @bind-Checked="@language"
               ValueChange="@OnLanguageChanged">
</SfRadioButton>

<p>@statusMessage</p>

@code {
    private string language = "en";
    private string statusMessage = "";
    
    private async void OnLanguageChanged(ChangeArgs<string> args)
    {
        statusMessage = "Updating language preference...";
        await SaveLanguagePreference(args.Value);
        statusMessage = $"Language updated to {args.Value}";
        StateHasChanged();
    }
    
    private async Task SaveLanguagePreference(string lang)
    {
        await Task.Delay(500);
        Console.WriteLine($"Saved language: {lang}");
    }
}
```

### ValueChange with Validation

```razor
@using Syncfusion.Blazor.Buttons

<h4>Select Shipping Method</h4>

<SfRadioButton Label="Standard Shipping (Free)" 
               Name="shipping" 
               Value="standard" 
               @bind-Checked="@shippingMethod"
               ValueChange="@OnShippingChanged">
</SfRadioButton>

<SfRadioButton Label="Express Shipping ($15)" 
               Name="shipping" 
               Value="express" 
               @bind-Checked="@shippingMethod"
               ValueChange="@OnShippingChanged">
</SfRadioButton>

<SfRadioButton Label="Overnight Shipping ($30)" 
               Name="shipping" 
               Value="overnight" 
               @bind-Checked="@shippingMethod"
               ValueChange="@OnShippingChanged">
</SfRadioButton>

<p style="color: @messageColor;">@message</p>

@code {
    private string shippingMethod = "standard";
    private string message = "";
    private string messageColor = "black";
    private decimal cartTotal = 45.00m;
    
    private void OnShippingChanged(ChangeArgs<string> args)
    {
        if (args.Value == "overnight" && cartTotal < 50.00m)
        {
            message = "Overnight shipping requires orders over $50.";
            messageColor = "red";
            shippingMethod = "express";
            return;
        }
        
        message = $"Shipping method updated to {args.Value}";
        messageColor = "green";
    }
}
```

---

## ChangeArgs Event Data

The `ChangeArgs<TChecked>` object provides information about the change event.

### ChangeArgs Properties

| Property | Type | Description |
|----------|------|-------------|
| `Value` | TChecked | The new value of the selected radio button |
| `Event` | EventArgs | The underlying browser event (if available) |

### Accessing Event Data

```razor
@using Syncfusion.Blazor.Buttons

<h4>Survey Question</h4>

<SfRadioButton Label="Very Satisfied" 
               Name="satisfaction" 
               Value="5" 
               @bind-Checked="@rating"
               ValueChange="@OnRatingChanged">
</SfRadioButton>

<SfRadioButton Label="Satisfied" 
               Name="satisfaction" 
               Value="4" 
               @bind-Checked="@rating"
               ValueChange="@OnRatingChanged">
</SfRadioButton>

<SfRadioButton Label="Neutral" 
               Name="satisfaction" 
               Value="3" 
               @bind-Checked="@rating"
               ValueChange="@OnRatingChanged">
</SfRadioButton>

<div>
    <p>Selected Rating: @rating</p>
    <p>Event Details: @eventDetails</p>
</div>

@code {
    private string rating = "3";
    private string eventDetails = "";
    
    private void OnRatingChanged(ChangeArgs<string> args)
    {
        eventDetails = $"New value: {args.Value}, Has Event: {args.Event != null}";
        Console.WriteLine($"Rating changed to: {args.Value}");
    }
}
```

### Using Generic Type with ChangeArgs

```razor
@using Syncfusion.Blazor.Buttons

<h4>Enable Feature?</h4>

<SfRadioButton Label="Yes" 
               Name="enable" 
               Value="@Value" 
               @bind-Checked="@isEnabled"
               ValueChange="@OnEnabledChanged">
</SfRadioButton>

<SfRadioButton Label="No" 
               Name="enable" 
               Value="@Value" 
               @bind-Checked="@isEnabled"
               ValueChange="@OnEnabledChanged">
</SfRadioButton>

@code {
    private bool isEnabled = true;
    private bool Value = true;
    private void OnEnabledChanged(ChangeArgs<bool> args)
    {
        Console.WriteLine($"Feature {(args.Value ? "enabled" : "disabled")}");
    }
}
```

---

## CheckedChanged Callback

The `CheckedChanged` event is a simpler callback that provides only the new value.

### Basic CheckedChanged Usage

```razor
@using Syncfusion.Blazor.Buttons

<h4>Newsletter Subscription</h4>

<SfRadioButton Label="Subscribe" 
               Name="newsletter" 
               Value="subscribed" 
               Checked="@subscriptionStatus"
               CheckedChanged="@OnSubscriptionChanged">
</SfRadioButton>

<SfRadioButton Label="Unsubscribe" 
               Name="newsletter" 
               Value="unsubscribed" 
               Checked="@subscriptionStatus"
               CheckedChanged="@OnSubscriptionChanged">
</SfRadioButton>

<p>Status: @subscriptionStatus</p>

@code {
    private string subscriptionStatus = "subscribed";
    
    private void OnSubscriptionChanged(string newValue)
    {
        subscriptionStatus = newValue;
        Console.WriteLine($"Subscription status: {newValue}");
    }
}
```

### CheckedChanged vs ValueChange

```razor
@using Syncfusion.Blazor.Buttons

<div>
    <h4>Using CheckedChanged (Simple)</h4>
    <SfRadioButton Label="Option A" 
                   Name="group1" 
                   Value="a" 
                   Checked="@selection1"
                   CheckedChanged="@((string val) => { selection1 = val; message1 = $"Selected: {val}"; })">
    </SfRadioButton>
    <p>@message1</p>
</div>

<div>
    <h4>Using ValueChange (Detailed)</h4>
    <SfRadioButton Label="Option A" 
                   Name="group2" 
                   Value="a" 
                   @bind-Checked="@selection2"
                   ValueChange="@((ChangeArgs<string> args) => { message2 = $"Selected: {args.Value}"; })">
    </SfRadioButton>
    <p>@message2</p>
</div>

@code {
    private string selection1 = "a";
    private string selection2 = "a";
    private string message1 = "";
    private string message2 = "";
}
```

**Use `CheckedChanged` when:** You only need the new value.  
**Use `ValueChange` when:** You need additional event information.

---

## Created Lifecycle Event

The `Created` event fires when the component is initialized.

### Basic Created Event

```razor
@using Syncfusion.Blazor.Buttons

<SfRadioButton Label="Option 1" 
               Name="test" 
               Value="1" 
               Created="@OnRadioButtonCreated">
</SfRadioButton>

<p>@initMessage</p>

@code {
    private string initMessage = "";
    
    private void OnRadioButtonCreated(Object args)
    {
        initMessage = "Radio buttons initialized";
        Console.WriteLine("Radio button component created");
    }
}
```

### Created Event with Initialization Logic

```razor
@using Syncfusion.Blazor.Buttons

<h4>User Preferences</h4>

<SfRadioButton Label="Light Theme" 
               Name="theme" 
               Value="light" 
               @bind-Checked="@theme"
               Created="@OnThemeRadioCreated">
</SfRadioButton>

<SfRadioButton Label="Dark Theme" 
               Name="theme" 
               Value="dark" 
               @bind-Checked="@theme"
               Created="@OnThemeRadioCreated">
</SfRadioButton>

@code {
    private string theme = "auto";
    private bool isInitialized = false;
    
    private async void OnThemeRadioCreated(Object args)
    {
        if (!isInitialized)
        {
            theme = await LoadThemePreference();
            isInitialized = true;
            StateHasChanged();
        }
    }
    
    private async Task<string> LoadThemePreference()
    {
        await Task.Delay(100);
        return "dark";
    }
}
```

---

## Combining Multiple Events

You can use multiple events together for complex scenarios.

### Example: All Events Combined

```razor
@using Syncfusion.Blazor.Buttons

<h4>Complete Event Handling</h4>

<SfRadioButton Label="Bronze" 
               Name="tier" 
               Value="bronze" 
               @bind-Checked="@membershipTier"
               Created="@OnCreated"
               ValueChange="@OnValueChange"
               CheckedChanged="@OnCheckedChanged">
</SfRadioButton>

<SfRadioButton Label="Silver" 
               Name="tier" 
               Value="silver" 
               @bind-Checked="@membershipTier"
               Created="@OnCreated"
               ValueChange="@OnValueChange"
               CheckedChanged="@OnCheckedChanged">
</SfRadioButton>

<div class="event-log">
    <h5>Event Log:</h5>
    @foreach (var log in eventLogs)
    {
        <p>@log</p>
    }
</div>

@code {
    private string membershipTier = "bronze";
    private List<string> eventLogs = new List<string>();
    
    private void OnCreated(Object args)
    {
        eventLogs.Add($"[{DateTime.Now:HH:mm:ss}] Created event fired");
    }
    
    private void OnValueChange(ChangeArgs<string> args)
    {
        eventLogs.Add($"[{DateTime.Now:HH:mm:ss}] ValueChange: {args.Value}");
    }
    
    private void OnCheckedChanged(string newValue)
    {
        eventLogs.Add($"[{DateTime.Now:HH:mm:ss}] CheckedChanged: {newValue}");
    }
}
```

---

## Event Handling Patterns

### Pattern 1: Conditional Event Handling

```razor
@using Syncfusion.Blazor.Buttons

<h4>Privacy Settings</h4>

<SfRadioButton Label="Public Profile" 
               Name="privacy" 
               Value="public" 
               @bind-Checked="@privacyLevel"
               ValueChange="@OnPrivacyChanged">
</SfRadioButton>

<SfRadioButton Label="Friends Only" 
               Name="privacy" 
               Value="friends" 
               @bind-Checked="@privacyLevel"
               ValueChange="@OnPrivacyChanged">
</SfRadioButton>

<SfRadioButton Label="Private" 
               Name="privacy" 
               Value="private" 
               @bind-Checked="@privacyLevel"
               ValueChange="@OnPrivacyChanged">
</SfRadioButton>

<p>@message</p>

@code {
    private string privacyLevel = "friends";
    private string message = "";
    
    private void OnPrivacyChanged(ChangeArgs<string> args)
    {
        if (args.Value == "public")
        {
            message = "Warning: Profile will be visible to everyone.";
        }
        else
        {
            message = $"Privacy set to: {args.Value}";
        }
    }
}
```

### Pattern 2: Real-Time Calculation

```razor
@using Syncfusion.Blazor.Buttons

<h4>Loan Calculator</h4>

<div>
    <label>Loan Term:</label>
    <SfRadioButton Label="12 months" Name="term" Value="12" @bind-Checked="@loanTerm" ValueChange="@CalculateLoan"></SfRadioButton>
    <SfRadioButton Label="24 months" Name="term" Value="24" @bind-Checked="@loanTerm" ValueChange="@CalculateLoan"></SfRadioButton>
    <SfRadioButton Label="36 months" Name="term" Value="36" @bind-Checked="@loanTerm" ValueChange="@CalculateLoan"></SfRadioButton>
</div>

<div style="margin-top: 20px;">
    <p><strong>Loan Amount:</strong> $10,000</p>
    <p><strong>Term:</strong> @loanTerm months</p>
    <p><strong>Monthly Payment:</strong> ${monthlyPayment:F2}</p>
    <p><strong>Total Interest:</strong> ${totalInterest:F2}</p>
</div>

@code {
    private string loanTerm = "12";
    private decimal monthlyPayment = 0;
    private decimal totalInterest = 0;
    
    protected override void OnInitialized()
    {
        CalculateLoan(new ChangeArgs<string> { Value = loanTerm });
    }
    
    private void CalculateLoan(ChangeArgs<string> args)
    {
        decimal principal = 10000m;
        decimal annualRate = 0.05m;
        int months = int.Parse(args.Value);
        
        decimal monthlyRate = annualRate / 12;
        monthlyPayment = (principal * monthlyRate) / (1 - (decimal)Math.Pow(1 + (double)monthlyRate, -months));
        totalInterest = (monthlyPayment * months) - principal;
    }
}
```

---

## Common Event Scenarios

### Scenario 1: Form Step Navigation

```razor
@using Syncfusion.Blazor.Buttons

<h4>Step @currentStep of 2</h4>

@if (currentStep == 1)
{
    <div>
        <h5>Select Account Type</h5>
        <SfRadioButton Label="Personal" Name="accountType" Value="personal" @bind-Checked="@accountType" ValueChange="@OnStepOneComplete"></SfRadioButton>
        <SfRadioButton Label="Business" Name="accountType" Value="business" @bind-Checked="@accountType" ValueChange="@OnStepOneComplete"></SfRadioButton>
    </div>
}
else
{
    <p>Registration Complete! Account: @accountType</p>
}

@code {
    private int currentStep = 1;
    private string accountType = "";
    
    private void OnStepOneComplete(ChangeArgs<string> args)
    {
        Task.Delay(500).ContinueWith(_ => 
        {
            currentStep = 2;
            InvokeAsync(StateHasChanged);
        });
    }
}
```

---

## Accessibility Features

### ARIA Attributes

Radio buttons support standard ARIA attributes for accessibility:

```razor
@using Syncfusion.Blazor.Buttons

<div role="radiogroup" aria-labelledby="payment-label">
    <h4 id="payment-label">Select Payment Method</h4>
    
    <SfRadioButton Label="Credit Card" 
                   Name="payment" 
                   Value="credit" 
                   HtmlAttributes="@(new Dictionary<string, object> 
                   { 
                       { "aria-describedby", "credit-desc" }
                   })">
    </SfRadioButton>
    <p id="credit-desc">Visa, MasterCard, Amex accepted</p>
</div>
```

### Keyboard Navigation

Radio buttons automatically support:
- **Tab**: Move focus between radio groups
- **Arrow Keys**: Navigate within a radio group
- **Space**: Select focused radio button

---

## Best Practices

### 1. Always Group Related Options

```razor
<!-- GOOD: Same Name property -->
<SfRadioButton TChecked="string" Label="A" Name="group1" Value="a"></SfRadioButton>
<SfRadioButton TChecked="string" Label="B" Name="group1" Value="b"></SfRadioButton>
```

### 2. Use Descriptive Labels

```razor
<!-- GOOD: Clear and descriptive -->
<SfRadioButton TChecked="string" Label="Express Shipping (2-3 days)" Name="shipping" Value="express"></SfRadioButton>

<!-- BAD: Vague -->
<SfRadioButton TChecked="string" Label="Option 2" Name="shipping" Value="express"></SfRadioButton>
```

### 3. Provide Feedback for Events

```razor
@code {
    private void OnValueChange(ChangeArgs<string> args)
    {
        message = $"Selection updated to: {args.Value}";
        StateHasChanged();
    }
}
```

### 4. Handle Async Operations Properly

```razor
@code {
    private async void OnLanguageChanged(ChangeArgs<string> args)
    {
        await SavePreference(args.Value);
        StateHasChanged(); // Refresh UI
    }
}
```

---

## Troubleshooting

### Issue: Event Not Firing

**Problem:** ValueChange or CheckedChanged doesn't trigger.

**Solution:** Ensure correct binding:

```razor
<!-- CORRECT -->
<SfRadioButton TChecked="string" Label="Option" Value="opt" ValueChange="@OnValueChange"></SfRadioButton>

@code {
    private void OnValueChange(ChangeArgs<string> args) { }
}
```

### Issue: Wrong Parameter Type

**Problem:** Compilation error about parameter type.

**Solution:** Match parameter type with event signature:

```razor
<!-- CheckedChanged expects value only -->
<SfRadioButton CheckedChanged="@OnCheckedChanged"></SfRadioButton>

@code {
    private void OnCheckedChanged(string newValue) { }
}

<!-- ValueChange expects ChangeArgs -->
<SfRadioButton ValueChange="@OnValueChange"></SfRadioButton>

@code {
    private void OnValueChange(ChangeArgs<string> args) { }
}
```

### Issue: Async Event Not Updating UI

**Problem:** UI doesn't refresh after async operation.

**Solution:** Call `StateHasChanged()`:

```razor
@code {
    private async void OnValueChange(ChangeArgs<string> args)
    {
        await SomeAsyncOperation();
        StateHasChanged(); // Force UI update
    }
}
```
