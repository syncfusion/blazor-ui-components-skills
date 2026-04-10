# Radio Button Styling and Customization

## Table of Contents
- [CssClass for Custom Styling](#cssclass-for-custom-styling)
- [HtmlAttributes Dictionary](#htmlattributes-dictionary)
- [EnableRtl for Right-to-Left Layouts](#enablertl-for-right-to-left-layouts)
- [Disabled State](#disabled-state)
- [EnablePersistence for State Storage](#enablepersistence-for-state-storage)
- [Theme Customization](#theme-customization)
- [Responsive Design](#responsive-design)
- [Accessibility Attributes](#accessibility-attributes)
- [Advanced Styling Techniques](#advanced-styling-techniques)
- [Troubleshooting](#troubleshooting)

---

## CssClass for Custom Styling

The `CssClass` property allows you to apply custom CSS classes to the radio button component.

### Basic CssClass Usage

```razor
@using Syncfusion.Blazor.Buttons

<style>
    .custom-radio {
        margin: 15px;
        padding: 10px;
        border: 2px solid #007bff;
        border-radius: 6px;
        background-color: #f8f9fa;
    }
</style>

<SfRadioButton Label="Styled Option 1" 
               Name="styled" 
               Value="1" 
               CssClass="custom-radio">
</SfRadioButton>

<SfRadioButton Label="Styled Option 2" 
               Name="styled" 
               Value="2" 
               CssClass="custom-radio">
</SfRadioButton>
```

### Highlighted Selected Radio Button

```razor
@using Syncfusion.Blazor.Buttons

<style>
    .radio-card {
        padding: 15px;
        margin: 10px 0;
        border: 2px solid #e0e0e0;
        border-radius: 8px;
        transition: all 0.3s ease;
    }
    
    .radio-card:hover {
        border-color: #007bff;
        box-shadow: 0 2px 8px rgba(0, 123, 255, 0.2);
    }
    
    .radio-card.selected {
        border-color: #007bff;
        background-color: #e7f3ff;
    }
</style>

<div class="@GetRadioClass("basic")">
    <SfRadioButton Label="Basic Plan - $9/month" 
                   Name="plan" 
                   Value="basic" 
                   @bind-Checked="@selectedPlan">
    </SfRadioButton>
</div>

<div class="@GetRadioClass("pro")">
    <SfRadioButton Label="Pro Plan - $19/month" 
                   Name="plan" 
                   Value="pro" 
                   @bind-Checked="@selectedPlan">
    </SfRadioButton>
</div>

@code {
    private string selectedPlan = "basic";
    
    private string GetRadioClass(string value)
    {
        return selectedPlan == value ? "radio-card selected" : "radio-card";
    }
}
```

### Multiple CSS Classes

```razor
@using Syncfusion.Blazor.Buttons

<style>
    .radio-base {
        margin: 8px;
    }
    
    .radio-large {
        font-size: 18px;
    }
    
    .radio-bold {
        font-weight: bold;
    }
</style>

<SfRadioButton Label="Large Bold Option" 
               Name="multi" 
               Value="1" 
               CssClass="radio-base radio-large radio-bold">
</SfRadioButton>
```

---

## HtmlAttributes Dictionary

The `HtmlAttributes` property allows you to add custom HTML attributes to the radio button element.

### Adding Data Attributes

```razor
@using Syncfusion.Blazor.Buttons

<SfRadioButton Label="Option 1" 
               Name="test" 
               Value="opt1" 
               HtmlAttributes="@(new Dictionary<string, object> 
               { 
                   { "data-category", "electronics" },
                   { "data-price", "299" }
               })">
</SfRadioButton>

<SfRadioButton Label="Option 2" 
               Name="test" 
               Value="opt2" 
               HtmlAttributes="@(new Dictionary<string, object> 
               { 
                   { "data-category", "clothing" },
                   { "data-price", "49" }
               })">
</SfRadioButton>
```

### ARIA Labels for Accessibility

```razor
@using Syncfusion.Blazor.Buttons

<h4>Select Priority Level</h4>

<SfRadioButton Label="🔴 High" 
               Name="priority" 
               Value="high" 
               HtmlAttributes="@(new Dictionary<string, object> 
               { 
                   { "aria-label", "High priority - requires immediate attention" },
                   { "title", "High Priority" }
               })">
</SfRadioButton>

<SfRadioButton Label="🟡 Medium" 
               Name="priority" 
               Value="medium" 
               HtmlAttributes="@(new Dictionary<string, object> 
               { 
                   { "aria-label", "Medium priority - handle within 24 hours" },
                   { "title", "Medium Priority" }
               })">
</SfRadioButton>

<SfRadioButton Label="🟢 Low" 
               Name="priority" 
               Value="low" 
               HtmlAttributes="@(new Dictionary<string, object> 
               { 
                   { "aria-label", "Low priority - handle when available" },
                   { "title", "Low Priority" }
               })">
</SfRadioButton>
```

### Adding Custom ID

```razor
@using Syncfusion.Blazor.Buttons

<SfRadioButton Label="Custom Element" 
               Name="custom" 
               Value="1" 
               HtmlAttributes="@(new Dictionary<string, object> 
               { 
                   { "id", "my-custom-radio" },
                   { "class", "special-radio" },
                   { "data-testid", "radio-test-1" }
               })">
</SfRadioButton>
```

---

## EnableRtl for Right-to-Left Layouts

The `EnableRtl` property enables right-to-left layout support for languages like Arabic, Hebrew, and Persian.

### Basic RTL Support

```razor
@using Syncfusion.Blazor.Buttons

<h4>RTL Layout (Arabic/Hebrew)</h4>

<SfRadioButton Label="الخيار الأول" 
               Name="rtl" 
               Value="1" 
               EnableRtl="true">
</SfRadioButton>

<SfRadioButton Label="الخيار الثاني" 
               Name="rtl" 
               Value="2" 
               EnableRtl="true">
</SfRadioButton>

<SfRadioButton Label="الخيار الثالث" 
               Name="rtl" 
               Value="3" 
               EnableRtl="true">
</SfRadioButton>
```

### Dynamic RTL Toggle

```razor
@using Syncfusion.Blazor.Buttons

<div>
    <label>
        <input type="checkbox" @bind="isRtl" />
        Enable RTL
    </label>
</div>

<div style="margin-top: 20px;">
    <SfRadioButton Label="Option One" 
                   Name="direction" 
                   Value="1" 
                   EnableRtl="@isRtl"
                   @bind-Checked="@selection">
    </SfRadioButton>
    
    <SfRadioButton Label="Option Two" 
                   Name="direction" 
                   Value="2" 
                   EnableRtl="@isRtl"
                   @bind-Checked="@selection">
    </SfRadioButton>
</div>

@code {
    private bool isRtl = false;
    private string selection = "1";
}
```

---

## Disabled State

The `Disabled` property controls whether users can interact with the radio button.

### Basic Disabled State

```razor
@using Syncfusion.Blazor.Buttons

<h4>Subscription Options</h4>

<SfRadioButton Label="Free Plan (Available)" 
               Name="subscription" 
               Value="free" 
               Disabled="false"
               @bind-Checked="@plan">
</SfRadioButton>

<SfRadioButton Label="Pro Plan (Coming Soon)" 
               Name="subscription" 
               Value="pro" 
               Disabled="true"
               @bind-Checked="@plan">
</SfRadioButton>

<SfRadioButton Label="Enterprise (Available)" 
               Name="subscription" 
               Value="enterprise" 
               Disabled="false"
               @bind-Checked="@plan">
</SfRadioButton>

@code {
    private string plan = "free";
}
```

### Conditional Disabled State

```razor
@using Syncfusion.Blazor.Buttons

<h4>Shipping Method</h4>

<SfRadioButton Label="Standard Shipping (Free)" 
               Name="shipping" 
               Value="standard" 
               Disabled="false"
               @bind-Checked="@shippingMethod">
</SfRadioButton>

<SfRadioButton Label="Express Shipping ($15)" 
               Name="shipping" 
               Value="express" 
               Disabled="@(!canUseExpress)"
               @bind-Checked="@shippingMethod">
</SfRadioButton>

<SfRadioButton Label="Overnight Shipping ($30)" 
               Name="shipping" 
               Value="overnight" 
               Disabled="@(!canUseOvernight)"
               @bind-Checked="@shippingMethod">
</SfRadioButton>

<div style="margin-top: 15px;">
    <p>Cart Total: ${cartTotal:F2}</p>
    <p style="color: orange;">
        @(cartTotal < 50 ? "Add $" + (50 - cartTotal).ToString("F2") + " to unlock Express shipping" : "")
    </p>
</div>

@code {
    private string shippingMethod = "standard";
    private decimal cartTotal = 30.00m;
    
    private bool canUseExpress => cartTotal >= 25.00m;
    private bool canUseOvernight => cartTotal >= 50.00m;
}
```

### Custom Disabled Styling

```razor
@using Syncfusion.Blazor.Buttons

<style>
    .custom-disabled {
        opacity: 0.5;
        cursor: not-allowed;
        background-color: #f5f5f5;
    }
    
    .custom-disabled label {
        text-decoration: line-through;
        color: #999;
    }
</style>

<SfRadioButton Label="Available Option" 
               Name="status" 
               Value="available" 
               @bind-Checked="@status">
</SfRadioButton>

<SfRadioButton Label="Unavailable Option" 
               Name="status" 
               Value="unavailable" 
               Disabled="true"
               CssClass="custom-disabled"
               @bind-Checked="@status">
</SfRadioButton>

@code {
    private string status = "available";
}
```

---

## EnablePersistence for State Storage

The `EnablePersistence` property stores the radio button's state in browser local storage.

### Basic Persistence

```razor
@using Syncfusion.Blazor.Buttons

<h4>Your preference will be remembered</h4>

<SfRadioButton Label="Light Theme" 
               Name="theme" 
               Value="light" 
               EnablePersistence="true"
               @bind-Checked="@theme">
</SfRadioButton>

<SfRadioButton Label="Dark Theme" 
               Name="theme" 
               Value="dark" 
               EnablePersistence="true"
               @bind-Checked="@theme">
</SfRadioButton>

<SfRadioButton Label="Auto" 
               Name="theme" 
               Value="auto" 
               EnablePersistence="true"
               @bind-Checked="@theme">
</SfRadioButton>

<p>Try refreshing the page - your selection persists!</p>

@code {
    private string theme = "auto";
}
```

---

## Theme Customization

Syncfusion provides multiple built-in themes.

### Available Themes

```html
<!-- Bootstrap 5 -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Material Design -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Fluent Design -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />

<!-- Tailwind CSS -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />

<!-- Microsoft Fabric -->
<link href="_content/Syncfusion.Blazor.Themes/fabric.css" rel="stylesheet" />

<!-- Dark Themes -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5-dark.css" rel="stylesheet" />
<link href="_content/Syncfusion.Blazor.Themes/material-dark.css" rel="stylesheet" />
```

### Custom CSS Variables

```razor
@using Syncfusion.Blazor.Buttons

<style>
    :root {
        --radio-primary-color: #6a1b9a;
        --radio-hover-color: #8e24aa;
        --radio-border-width: 2px;
    }
    
    .custom-theme .e-radio:checked + .e-label::before {
        background-color: var(--radio-primary-color);
        border-color: var(--radio-primary-color);
    }
    
    .custom-theme .e-radio:hover + .e-label::before {
        border-color: var(--radio-hover-color);
    }
    
    .custom-theme .e-label {
        color: #333;
        font-weight: 500;
    }
</style>

<div class="custom-theme">
    <SfRadioButton TChecked="string" Label="Custom Styled Option 1" Name="custom" Value="1"></SfRadioButton>
    <SfRadioButton TChecked="string" Label="Custom Styled Option 2" Name="custom" Value="2"></SfRadioButton>
</div>
```

---

## Responsive Design

Create radio buttons that adapt to different screen sizes.

### Responsive Layout

```razor
@using Syncfusion.Blazor.Buttons

<style>
    .radio-grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
        gap: 15px;
        padding: 20px;
    }
    
    .radio-item {
        padding: 15px;
        border: 1px solid #ddd;
        border-radius: 8px;
    }
    
    @media (max-width: 768px) {
        .radio-grid {
            grid-template-columns: 1fr;
        }
    }
</style>

<div class="radio-grid">
    <div class="radio-item">
        <SfRadioButton Label="Mobile Optimized" Name="device" Value="mobile" @bind-Checked="@device"></SfRadioButton>
    </div>
    
    <div class="radio-item">
        <SfRadioButton Label="Tablet Optimized" Name="device" Value="tablet" @bind-Checked="@device"></SfRadioButton>
    </div>
    
    <div class="radio-item">
        <SfRadioButton Label="Desktop Optimized" Name="device" Value="desktop" @bind-Checked="@device"></SfRadioButton>
    </div>
</div>

@code {
    private string device = "mobile";
}
```

### Mobile-First Design

```razor
@using Syncfusion.Blazor.Buttons

<style>
    .mobile-radio-group {
        display: flex;
        flex-direction: column;
        gap: 12px;
    }
    
    .mobile-radio-item {
        padding: 20px;
        background: white;
        border: 2px solid #e0e0e0;
        border-radius: 12px;
        font-size: 16px;
    }
    
    @media (min-width: 768px) {
        .mobile-radio-group {
            flex-direction: row;
            flex-wrap: wrap;
        }
        
        .mobile-radio-item {
            flex: 1 1 calc(50% - 12px);
            min-width: 200px;
        }
    }
</style>

<div class="mobile-radio-group">
    <div class="mobile-radio-item">
        <SfRadioButton Label="Option A" Name="mobile" Value="a" @bind-Checked="@selection"></SfRadioButton>
    </div>
    <div class="mobile-radio-item">
        <SfRadioButton Label="Option B" Name="mobile" Value="b" @bind-Checked="@selection"></SfRadioButton>
    </div>
</div>

@code {
    private string selection = "a";
}
```

---

## Accessibility Attributes

Enhance accessibility for screen readers and assistive technologies.

### ARIA Attributes

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
    <p id="credit-desc" style="font-size: 12px; color: #666;">Visa, MasterCard, Amex accepted</p>
    
    <SfRadioButton Label="PayPal" 
                   Name="payment" 
                   Value="paypal" 
                   HtmlAttributes="@(new Dictionary<string, object> 
                   { 
                       { "aria-describedby", "paypal-desc" }
                   })">
    </SfRadioButton>
    <p id="paypal-desc" style="font-size: 12px; color: #666;">Login to your PayPal account</p>
</div>
```

### Keyboard Navigation Support

Radio buttons automatically support keyboard navigation:
- **Tab**: Move focus between radio groups
- **Arrow Keys**: Navigate within a radio group
- **Space**: Select focused radio button

---

## Advanced Styling Techniques

### Gradient Background

```razor
@using Syncfusion.Blazor.Buttons

<style>
    .gradient-radio {
        margin: 10px;
        padding: 15px 25px;
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        border-radius: 25px;
    }
    
    .gradient-radio label {
        color: white !important;
        font-weight: bold;
    }
</style>

<SfRadioButton Label="Premium Option" 
               Name="premium" 
               Value="1" 
               CssClass="gradient-radio">
</SfRadioButton>
```

### Icon with Radio Button

```razor
@using Syncfusion.Blazor.Buttons

<style>
    .icon-radio {
        display: flex;
        align-items: center;
        gap: 10px;
        padding: 12px;
        border: 2px solid #ddd;
        border-radius: 8px;
        margin: 8px 0;
    }
    
    .icon {
        font-size: 24px;
    }
</style>

<div class="icon-radio">
    <span class="icon">⚡</span>
    <SfRadioButton Label="Fast Delivery" Name="speed" Value="fast" @bind-Checked="@speed"></SfRadioButton>
</div>

<div class="icon-radio">
    <span class="icon">🚚</span>
    <SfRadioButton Label="Standard Delivery" Name="speed" Value="standard" @bind-Checked="@speed"></SfRadioButton>
</div>

@code {
    private string speed = "standard";
}
```

---

## Troubleshooting

### Issue: Custom Styles Not Applying

**Problem:** CssClass styles don't appear.

**Solution:** Ensure styles are defined and classes are correctly specified:

```razor
<style>
    .my-custom-class {
        background: yellow;
    }
</style>

<SfRadioButton Label="Test" CssClass="my-custom-class"></SfRadioButton>
```

### Issue: RTL Not Working

**Problem:** EnableRtl doesn't affect layout.

**Solution:** Set `EnableRtl="true"` (boolean):

```razor
<!-- CORRECT -->
<SfRadioButton Label="Option" EnableRtl="true"></SfRadioButton>
```

### Issue: HtmlAttributes Not Rendering

**Problem:** Custom attributes don't appear in HTML.

**Solution:** Use correct Dictionary syntax:

```razor
<!-- CORRECT -->
<SfRadioButton Label="Test" 
               HtmlAttributes="@(new Dictionary<string, object> { { "data-id", "123" } })">
</SfRadioButton>
```

### Issue: Persistence Not Working

**Problem:** Selection not persisted across page refreshes.

**Solution:** Ensure `EnablePersistence="true"` is set and browser local storage is enabled:

```razor
<SfRadioButton Label="Option" 
               EnablePersistence="true" 
               @bind-Checked="@value">
</SfRadioButton>
```
