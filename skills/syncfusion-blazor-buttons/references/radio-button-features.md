# Radio Button Features

## Table of Contents
- [Getting Started](#getting-started)
- [Basic Implementation](#basic-implementation)
- [Radio Button Grouping](#radio-button-grouping)
- [Label Configuration](#label-configuration)
- [State and Value Management](#state-and-value-management)
- [Two-Way Binding](#two-way-binding)
- [Pre-Selecting Values](#pre-selecting-values)
- [Form Integration](#form-integration)
- [Common Patterns](#common-patterns)
- [Key Properties](#key-properties)
- [Troubleshooting](#troubleshooting)

---

## Getting Started

### Package Installation

Install the Syncfusion Blazor Buttons package:

```bash
dotnet add package Syncfusion.Blazor.Buttons
```

### Register Services

Add Syncfusion service in `Program.cs`:

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddSyncfusionBlazor();
var app = builder.Build();
app.Run();
```

### Add Namespace

Add to `_Imports.razor`:

```razor
@using Syncfusion.Blazor.Buttons
```

### Add Theme

Include theme CSS in `index.html` or `_Host.cshtml`:

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
```

**Available Themes:** bootstrap5.css, material.css, fabric.css, fluent.css, tailwind.css

---

## Basic Implementation

### Minimal Example

```razor
@using Syncfusion.Blazor.Buttons

<SfRadioButton TChecked="string" Label="Option 1" Name="basic" Value="option1"></SfRadioButton>
```

### Simple Radio Group

```razor
@using Syncfusion.Blazor.Buttons

<h4>Choose Your Favorite Color</h4>

<SfRadioButton Label="Red" Name="color" Value="red" @bind-Checked="@selectedColor"></SfRadioButton>
<SfRadioButton Label="Blue" Name="color" Value="blue" @bind-Checked="@selectedColor"></SfRadioButton>
<SfRadioButton Label="Green" Name="color" Value="green" @bind-Checked="@selectedColor"></SfRadioButton>

<p>Selected Color: <strong>@selectedColor</strong></p>

@code {
    private string selectedColor = "red";
}
```

**Key Points:**
- All radio buttons share the same `Name` property
- Each has a unique `Value` property
- `@bind-Checked` enables two-way binding
- Only one radio button can be checked at a time

---

## Radio Button Grouping

Radio buttons are grouped using the `Name` property. Buttons with the same name are mutually exclusive.

### Single Group

```razor
@using Syncfusion.Blazor.Buttons

<h4>Payment Method</h4>

<SfRadioButton Label="Credit Card" Name="payment" Value="credit" @bind-Checked="@paymentMethod"></SfRadioButton>
<SfRadioButton Label="PayPal" Name="payment" Value="paypal" @bind-Checked="@paymentMethod"></SfRadioButton>
<SfRadioButton Label="Bank Transfer" Name="payment" Value="bank" @bind-Checked="@paymentMethod"></SfRadioButton>

@code {
    private string paymentMethod = "credit";
}
```

### Multiple Independent Groups

```razor
@using Syncfusion.Blazor.Buttons

<h4>Pizza Order</h4>

<div>
    <h5>Size</h5>
    <SfRadioButton Label="Small" Name="size" Value="small" @bind-Checked="@pizzaSize"></SfRadioButton>
    <SfRadioButton Label="Medium" Name="size" Value="medium" @bind-Checked="@pizzaSize"></SfRadioButton>
    <SfRadioButton Label="Large" Name="size" Value="large" @bind-Checked="@pizzaSize"></SfRadioButton>
</div>

<div>
    <h5>Crust</h5>
    <SfRadioButton Label="Thin" Name="crust" Value="thin" @bind-Checked="@crustType"></SfRadioButton>
    <SfRadioButton Label="Regular" Name="crust" Value="regular" @bind-Checked="@crustType"></SfRadioButton>
    <SfRadioButton Label="Thick" Name="crust" Value="thick" @bind-Checked="@crustType"></SfRadioButton>
</div>

<p>Order: @pizzaSize @crustType crust pizza</p>

@code {
    private string pizzaSize = "medium";
    private string crustType = "regular";
}
```

---

## Label Configuration

### Basic Label

The `Label` property displays text next to the radio button:

```razor
@using Syncfusion.Blazor.Buttons

<SfRadioButton TChecked="string" Label="Enable notifications" Name="notify" Value="enabled"></SfRadioButton>
```

### Label Positioning

Use `LabelPosition` to control label placement:

```razor
@using Syncfusion.Blazor.Buttons

<!-- Label after radio button (default) -->
<SfRadioButton Label="Accept" 
               Name="consent" 
               Value="yes" 
               LabelPosition="LabelPosition.After">
</SfRadioButton>

<!-- Label before radio button -->
<SfRadioButton Label="Accept" 
               Name="consent2" 
               Value="yes" 
               LabelPosition="LabelPosition.Before">
</SfRadioButton>
```

**LabelPosition Options:**
- `LabelPosition.After` - Label appears after (right of) the radio button (default)
- `LabelPosition.Before` - Label appears before (left of) the radio button

### Custom Label Content

For HTML content, use `ChildContent`:

```razor
@using Syncfusion.Blazor.Buttons

<SfRadioButton Name="terms" Value="accept" @bind-Checked="@accepted">
    <ChildContent>
        I accept the <a href="/terms">Terms of Service</a> and 
        <a href="/privacy">Privacy Policy</a>
    </ChildContent>
</SfRadioButton>

@code {
    private string accepted = "";
}
```

### Label with Icons

```razor
@using Syncfusion.Blazor.Buttons

<style>
    .icon-label {
        display: flex;
        align-items: center;
        gap: 8px;
    }
</style>

<SfRadioButton Name="priority" Value="high" @bind-Checked="@priority">
    <ChildContent>
        <span class="icon-label">
            <span>⚠️</span>
            <span>High Priority</span>
        </span>
    </ChildContent>
</SfRadioButton>

<SfRadioButton Name="priority" Value="low" @bind-Checked="@priority">
    <ChildContent>
        <span class="icon-label">
            <span>✓</span>
            <span>Low Priority</span>
        </span>
    </ChildContent>
</SfRadioButton>

@code {
    private string priority = "high";
}
```

---

## State and Value Management

### Checked Property

The `Checked` property represents the current state (one-way binding):

```razor
@using Syncfusion.Blazor.Buttons

<SfRadioButton Label="Option A" 
               Name="group" 
               Value="a" 
               Checked="@isChecked">
</SfRadioButton>

@code {
    private bool isChecked = true;
}
```

### Value Property

The `Value` property uniquely identifies each radio button:

```razor
@using Syncfusion.Blazor.Buttons

<SfRadioButton Label="Small" Name="size" Value="S" @bind-Checked="@selectedSize"></SfRadioButton>
<SfRadioButton Label="Medium" Name="size" Value="M" @bind-Checked="@selectedSize"></SfRadioButton>
<SfRadioButton Label="Large" Name="size" Value="L" @bind-Checked="@selectedSize"></SfRadioButton>

<p>Selected: @selectedSize</p>

@code {
    private string selectedSize = "M";
}
```

### String Values

```razor
@using Syncfusion.Blazor.Buttons

<h4>Rate this product (1-5)</h4>

<SfRadioButton Label="1 Star" Name="rating" Value="1" @bind-Checked="@rating"></SfRadioButton>
<SfRadioButton Label="2 Stars" Name="rating" Value="2" @bind-Checked="@rating"></SfRadioButton>
<SfRadioButton Label="3 Stars" Name="rating" Value="3" @bind-Checked="@rating"></SfRadioButton>
<SfRadioButton Label="4 Stars" Name="rating" Value="4" @bind-Checked="@rating"></SfRadioButton>
<SfRadioButton Label="5 Stars" Name="rating" Value="5" @bind-Checked="@rating"></SfRadioButton>

@code {
    private string rating = "3";
}
```

### Boolean Values

```razor
@using Syncfusion.Blazor.Buttons

<SfRadioButton Label="Enable Feature" 
               Name="feature" 
               Value="@Value" 
               @bind-Checked="@isEnabled">
</SfRadioButton>

<SfRadioButton Label="Disable Feature" 
               Name="feature" 
               Value="@false" 
               @bind-Checked="@isEnabled">
</SfRadioButton>

<p>Feature is: @(isEnabled ? "Enabled" : "Disabled")</p>

@code {
    private bool isEnabled = true;
    private bool Value = true;

}
```

---

## Two-Way Binding

Use `@bind-Checked` for automatic synchronization between UI and code:

```razor
@using Syncfusion.Blazor.Buttons

<SfRadioButton Label="Morning" Name="time" Value="morning" @bind-Checked="@preferredTime"></SfRadioButton>
<SfRadioButton Label="Afternoon" Name="time" Value="afternoon" @bind-Checked="@preferredTime"></SfRadioButton>
<SfRadioButton Label="Evening" Name="time" Value="evening" @bind-Checked="@preferredTime"></SfRadioButton>

<p>You prefer: @preferredTime</p>

<button @onclick="@(() => preferredTime = "evening")">Set to Evening</button>

@code {
    private string preferredTime = "morning";
}
```

**How it works:**
- User clicks radio button → variable updates automatically
- Code updates variable → radio button selection updates automatically

### Computed Properties

```razor
@using Syncfusion.Blazor.Buttons

<h4>Subscription Duration</h4>

<SfRadioButton Label="Monthly ($10/mo)" Name="duration" Value="monthly" @bind-Checked="@duration"></SfRadioButton>
<SfRadioButton Label="Yearly ($100/yr)" Name="duration" Value="yearly" @bind-Checked="@duration"></SfRadioButton>

<div style="margin-top: 20px;">
    <p>Price: <strong>${price}</strong></p>
    <p>Savings: <strong>${savings}</strong></p>
</div>

@code {
    private string duration = "monthly";
    
    private int price => duration == "monthly" ? 10 : 100;
    private int savings => duration == "yearly" ? 20 : 0;
}
```

---

## Pre-Selecting Values

Set the initial value to pre-select a radio button:

```razor
@using Syncfusion.Blazor.Buttons

<h4>Newsletter Frequency</h4>

<SfRadioButton Label="Daily" Name="newsletter" Value="daily" @bind-Checked="@frequency"></SfRadioButton>
<SfRadioButton Label="Weekly" Name="newsletter" Value="weekly" @bind-Checked="@frequency"></SfRadioButton>
<SfRadioButton Label="Monthly" Name="newsletter" Value="monthly" @bind-Checked="@frequency"></SfRadioButton>

@code {
    private string frequency = "weekly"; // Pre-selected
}
```

### Load from User Data

```razor
@using Syncfusion.Blazor.Buttons

<h4>Notification Preference</h4>

<SfRadioButton Label="Email" Name="notify" Value="email" @bind-Checked="@notificationMethod"></SfRadioButton>
<SfRadioButton Label="SMS" Name="notify" Value="sms" @bind-Checked="@notificationMethod"></SfRadioButton>
<SfRadioButton Label="Push" Name="notify" Value="push" @bind-Checked="@notificationMethod"></SfRadioButton>

@code {
    private string notificationMethod = "";
    
    protected override async Task OnInitializedAsync()
    {
        notificationMethod = await GetUserPreference();
    }
    
    private async Task<string> GetUserPreference()
    {
        await Task.Delay(100);
        return "email"; // User's saved preference
    }
}
```

---

## Form Integration

Integrate with Blazor's EditForm for validation:

```razor
@using Syncfusion.Blazor.Buttons
@using System.ComponentModel.DataAnnotations

<EditForm Model="@registrationForm" OnValidSubmit="@HandleSubmit">
    <DataAnnotationsValidator />
    
    <div class="form-group">
        <label>Gender: *</label>
        <div>
            <SfRadioButton Label="Male" 
                           Name="gender" 
                           Value="male" 
                           @bind-Checked="@registrationForm.Gender">
            </SfRadioButton>
            
            <SfRadioButton Label="Female" 
                           Name="gender" 
                           Value="female" 
                           @bind-Checked="@registrationForm.Gender">
            </SfRadioButton>
            
            <SfRadioButton Label="Other" 
                           Name="gender" 
                           Value="other" 
                           @bind-Checked="@registrationForm.Gender">
            </SfRadioButton>
        </div>
        <ValidationMessage For="@(() => registrationForm.Gender)" />
    </div>
    
    <button type="submit">Register</button>
</EditForm>

@code {
    private RegistrationForm registrationForm = new RegistrationForm();
    
    public class RegistrationForm
    {
        [Required(ErrorMessage = "Please select a gender")]
        public string Gender { get; set; }
    }
    
    private void HandleSubmit()
    {
        Console.WriteLine($"Form submitted with gender: {registrationForm.Gender}");
    }
}
```

---

## Common Patterns

### Pattern 1: Radio Group with Event Handling

```razor
@using Syncfusion.Blazor.Buttons

<SfRadioButton Label="Option 1" 
               Name="options" 
               Value="option1" 
               @bind-Checked="@selectedOption"
               ValueChange="@OnValueChange">
</SfRadioButton>

<SfRadioButton Label="Option 2" 
               Name="options" 
               Value="option2" 
               @bind-Checked="@selectedOption"
               ValueChange="@OnValueChange">
</SfRadioButton>

<p>You selected: @selectedOption</p>

@code {
    private string selectedOption = "option1";
    
    private void OnValueChange(ChangeArgs<string> args)
    {
        Console.WriteLine($"Value changed to {args.Value}");
    }
}
```

### Pattern 2: Conditional Radio Buttons

```razor
@using Syncfusion.Blazor.Buttons

<SfRadioButton Label="Standard Shipping (Free)" 
               Name="shipping" 
               Value="standard" 
               @bind-Checked="@shippingMethod">
</SfRadioButton>

<SfRadioButton Label="Express Shipping ($10)" 
               Name="shipping" 
               Value="express" 
               Disabled="@(!canUseExpress)"
               @bind-Checked="@shippingMethod">
</SfRadioButton>

<SfRadioButton Label="Overnight Shipping ($25)" 
               Name="shipping" 
               Value="overnight" 
               Disabled="@(!canUseOvernight)"
               @bind-Checked="@shippingMethod">
</SfRadioButton>

@code {
    private string shippingMethod = "standard";
    private bool canUseExpress = true;
    private bool canUseOvernight = false;
}
```

### Pattern 3: Custom Styled Radio Buttons

```razor
@using Syncfusion.Blazor.Buttons

<style>
    .custom-radio {
        margin: 10px;
        padding: 10px;
        border: 1px solid #ddd;
        border-radius: 4px;
    }
    
    .premium-option {
        background-color: #fff3cd;
        border-color: #ffc107;
    }
</style>

<SfRadioButton Label="Basic" 
               Name="tier" 
               Value="basic" 
               CssClass="custom-radio"
               @bind-Checked="@tier">
</SfRadioButton>

<SfRadioButton Label="Premium" 
               Name="tier" 
               Value="premium" 
               CssClass="custom-radio premium-option"
               @bind-Checked="@tier">
</SfRadioButton>

@code {
    private string tier = "basic";
}
```

---

## Key Properties

### Core Properties

| Property | Type | Description | When to Use |
|----------|------|-------------|-------------|
| `Label` | string | Text displayed next to the radio button | Always provide a descriptive label |
| `Value` | string | Unique identifier for the radio button | Distinguish between options in a group |
| `Name` | string | Groups radio buttons together | Same name for mutually exclusive group |
| `Checked` | TChecked | Current checked state (two-way bindable) | Use @bind-Checked for synchronization |

### Event Properties

| Property | Type | Description | When to Use |
|----------|------|-------------|-------------|
| `ValueChange` | EventCallback<ChangeArgs<TChecked>> | Fired when selection changes | Respond to user selections |
| `CheckedChanged` | EventCallback<TChecked> | Fired when checked state changes | Simpler state change notifications |
| `Created` | EventCallback<Object> | Fired when component is created | Initialization logic |

### Appearance Properties

| Property | Type | Description | When to Use |
|----------|------|-------------|-------------|
| `LabelPosition` | LabelPosition | Position of label (Before/After) | Control label placement |
| `CssClass` | string | Custom CSS classes | Apply custom styling |
| `HtmlAttributes` | Dictionary<string, object> | Additional HTML attributes | Data attributes, ARIA labels |
| `Disabled` | bool | Disables the radio button | Prevent user interaction |

### Behavior Properties

| Property | Type | Description | When to Use |
|----------|------|-------------|-------------|
| `EnableRtl` | bool | Enables right-to-left layout | RTL languages (Arabic, Hebrew) |
| `EnablePersistence` | bool | Persists state across sessions | Survive page refreshes |
| `IsOnChangeEvent` | bool | Triggers event on change vs click | Control event firing behavior |

---

## Troubleshooting

### Issue: Radio Buttons Not Mutually Exclusive

**Problem:** Multiple radio buttons can be selected.

**Solution:** Ensure all radio buttons have the **same** `Name` property:

```razor
<!-- CORRECT: Same Name -->
<SfRadioButton Label="A" Name="group1" Value="a" @bind-Checked="@selection"></SfRadioButton>
<SfRadioButton Label="B" Name="group1" Value="b" @bind-Checked="@selection"></SfRadioButton>

<!-- INCORRECT: Different Names -->
<SfRadioButton Label="A" Name="groupA" Value="a" @bind-Checked="@selection"></SfRadioButton>
<SfRadioButton Label="B" Name="groupB" Value="b" @bind-Checked="@selection"></SfRadioButton>
```

### Issue: Two-Way Binding Not Working

**Problem:** Variable doesn't update when clicked.

**Solution:** Use `@bind-Checked`, not `Checked`:

```razor
<!-- CORRECT -->
<SfRadioButton Label="Option" Value="opt" @bind-Checked="@value"></SfRadioButton>

<!-- INCORRECT -->
<SfRadioButton Label="Option" Value="opt" Checked="@value"></SfRadioButton>
```

### Issue: Type Mismatch Error

**Problem:** Compilation error about type mismatch.

**Solution:** Ensure `Value` type matches the bound variable type:

```razor
@code {
    private string selection = "option1"; // String type
}

<!-- CORRECT: String value -->
<SfRadioButton Label="Option" Value="option1" @bind-Checked="@selection"></SfRadioButton>

<!-- INCORRECT: Int value with string variable -->
<SfRadioButton Label="Option" Value="1" @bind-Checked="@selection"></SfRadioButton>
```

### Issue: Pre-Selection Not Working

**Problem:** Radio button not initially selected.

**Solution:** Ensure initial value **exactly matches** a `Value` property:

```razor
@code {
    private string choice = "option2"; // Must match exactly
}

<SfRadioButton Label="A" Name="test" Value="option1" @bind-Checked="@choice"></SfRadioButton>
<SfRadioButton Label="B" Name="test" Value="option2" @bind-Checked="@choice"></SfRadioButton>
```
