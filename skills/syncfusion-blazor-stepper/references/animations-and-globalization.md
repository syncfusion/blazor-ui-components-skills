# Animations and Globalization

## Table of Contents

- [Animation Settings](#animation-settings)
  - [Animation Configuration](#animation-configuration)
  - [Animation Properties](#animation-properties)
  - [Disabling Animations](#disabling-animations)
  - [Custom Animation Timing](#custom-animation-timing)
- [Localization](#localization)
  - [Enable Localization](#enable-localization)
  - [Localized Stepper Example](#localized-stepper-example)
- [Right-to-Left (RTL) Support](#right-to-left-rtl-support)
  - [Basic RTL Implementation](#basic-rtl-implementation)
  - [Dynamic RTL Toggle](#dynamic-rtl-toggle)
  - [RTL with Arabic Content](#rtl-with-arabic-content)
- [Combining Localization and RTL](#combining-localization-and-rtl)
- [Performance Considerations](#performance-considerations)
  - [Animation Performance Tips](#animation-performance-tips)
  - [Accessibility with Animations](#accessibility-with-animations)

## Animation Settings

The Stepper component supports smooth animations when transitioning between steps. Configure animation behavior using the `StepperAnimationSettings` tag directive.

### Animation Configuration

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper>
    <StepperSteps>
        <StepperStep Label="Cart"></StepperStep>
        <StepperStep Label="Shipping"></StepperStep>
        <StepperStep Label="Payment"></StepperStep>
        <StepperStep Label="Review"></StepperStep>
    </StepperSteps>
    <StepperAnimationSettings Enable="true" Duration="2000" Delay="500"></StepperAnimationSettings>
</SfStepper>
```

### Animation Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Enable` | `bool` | `true` | Enable or disable animations |
| `Duration` | `int` | `2000` | Animation duration in milliseconds |
| `Delay` | `int` | `0` | Delay before animation starts in milliseconds |

### Disabling Animations

For performance optimization or accessibility reasons, disable animations:

```csharp
<SfStepper>
    <StepperSteps>
        <StepperStep Label="Step 1"></StepperStep>
        <StepperStep Label="Step 2"></StepperStep>
        <StepperStep Label="Step 3"></StepperStep>
    </StepperSteps>
    <StepperAnimationSettings Enable="false"></StepperAnimationSettings>
</SfStepper>
```

### Custom Animation Timing

```csharp
@using Syncfusion.Blazor.Navigations

<div>
    <div>
        <label>
            Duration (ms): 
            <input type="number" @bind="animationDuration" min="0" max="5000" step="100" />
        </label>
    </div>
    <div>
        <label>
            Delay (ms): 
            <input type="number" @bind="animationDelay" min="0" max="2000" step="50" />
        </label>
    </div>
</div>

<SfStepper>
    <StepperSteps>
        <StepperStep Label="Step 1"></StepperStep>
        <StepperStep Label="Step 2"></StepperStep>
        <StepperStep Label="Step 3"></StepperStep>
    </StepperSteps>
    <StepperAnimationSettings Enable="true" Duration="@animationDuration" Delay="@animationDelay"></StepperAnimationSettings>
</SfStepper>

@code {
    private int animationDuration = 2000;
    private int animationDelay = 0;
}

<style>
    label {
        display: block;
        margin: 10px 0;
    }
    
    input[type="number"] {
        width: 100px;
        padding: 5px;
    }
</style>
```

## Localization

The Stepper component supports localization for displaying text in different languages. Syncfusion provides localized resources for multiple languages.

### Enable Localization

First, set up the culture in your Blazor application:

```csharp
// In Program.cs
using System.Globalization;

var host = builder.Build();
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("fr-FR");
CultureInfo.DefaultThreadCurrentUICulture = new CultureInfo("fr-FR");

await host.RunAsync();
```

### Localized Stepper Example

```csharp
@using Syncfusion.Blazor.Navigations
@using System.Globalization

<div>
    <button @onclick="@(() => ChangeCulture("en-US"))">English</button>
    <button @onclick="@(() => ChangeCulture("fr-FR"))">Français</button>
    <button @onclick="@(() => ChangeCulture("es-ES"))">Español</button>
    <button @onclick="@(() => ChangeCulture("de-DE"))">Deutsch</button>
</div>

<SfStepper>
    <StepperSteps>
        <StepperStep Label="@GetLocalizedLabel("cart")"></StepperStep>
        <StepperStep Label="@GetLocalizedLabel("address")"></StepperStep>
        <StepperStep Label="@GetLocalizedLabel("payment")"></StepperStep>
        <StepperStep Label="@GetLocalizedLabel("confirmation")"></StepperStep>
    </StepperSteps>
</SfStepper>

@code {
    private Dictionary<string, Dictionary<string, string>> localizations = new()
    {
        { "en-US", new()
            {
                { "cart", "Shopping Cart" },
                { "address", "Delivery Address" },
                { "payment", "Payment Method" },
                { "confirmation", "Order Confirmation" }
            }
        },
        { "fr-FR", new()
            {
                { "cart", "Panier" },
                { "address", "Adresse de Livraison" },
                { "payment", "Méthode de Paiement" },
                { "confirmation", "Confirmation de Commande" }
            }
        },
        { "es-ES", new()
            {
                { "cart", "Carrito de Compras" },
                { "address", "Dirección de Envío" },
                { "payment", "Método de Pago" },
                { "confirmation", "Confirmación del Pedido" }
            }
        },
        { "de-DE", new()
            {
                { "cart", "Einkaufswagen" },
                { "address", "Lieferadresse" },
                { "payment", "Zahlungsmethode" },
                { "confirmation", "Bestellbestätigung" }
            }
        }
    };
    
    private string currentCulture = "en-US";
    
    private void ChangeCulture(string culture)
    {
        currentCulture = culture;
        CultureInfo.CurrentCulture = new CultureInfo(culture);
        CultureInfo.CurrentUICulture = new CultureInfo(culture);
    }
    
    private string GetLocalizedLabel(string key)
    {
        if (localizations.TryGetValue(currentCulture, out var labels))
        {
            if (labels.TryGetValue(key, out var label))
            {
                return label;
            }
        }
        return key;
    }
}
```

## Right-to-Left (RTL) Support

Enable RTL layout for languages that read from right to left.

### Basic RTL Implementation

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper EnableRtl="true">
    <StepperSteps>
        <StepperStep Label="الخطوة الأولى"></StepperStep>
        <StepperStep Label="الخطوة الثانية"></StepperStep>
        <StepperStep Label="الخطوة الثالثة"></StepperStep>
        <StepperStep Label="الخطوة الرابعة"></StepperStep>
    </StepperSteps>
</SfStepper>
```

### Dynamic RTL Toggle

```csharp
@using Syncfusion.Blazor.Navigations

<div>
    <label>
        <input type="checkbox" @bind="enableRtl" />
        Enable RTL
    </label>
</div>

<SfStepper EnableRtl="@enableRtl">
    <StepperSteps>
        <StepperStep Label="@(enableRtl ? "المشتريات" : "Shopping")"></StepperStep>
        <StepperStep Label="@(enableRtl ? "العنوان" : "Address")"></StepperStep>
        <StepperStep Label="@(enableRtl ? "الدفع" : "Payment")"></StepperStep>
        <StepperStep Label="@(enableRtl ? "التأكيد" : "Confirmation")"></StepperStep>
    </StepperSteps>
</SfStepper>

@code {
    private bool enableRtl = false;
}

<style>
    label {
        display: flex;
        align-items: center;
        gap: 10px;
        margin-bottom: 20px;
    }
    
    input[type="checkbox"] {
        cursor: pointer;
    }
</style>
```

### RTL with Arabic Content

```csharp
@using Syncfusion.Blazor.Navigations

<!DOCTYPE html>
<html dir="rtl" lang="ar">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Stepper - RTL</title>
    <link href="_content/Syncfusion.Blazor.Themes/material3-rtl.css" rel="stylesheet" />
</head>
<body>
    <div id="app"></div>
    <script src="_framework/blazor.web.js"></script>
</body>
</html>

<!-- In your Blazor component -->
<SfStepper EnableRtl="true">
    <StepperSteps>
        <StepperStep Label="معلومات شخصية" IconCss="sf-icon-user"></StepperStep>
        <StepperStep Label="عنوان التسليم" IconCss="sf-icon-location"></StepperStep>
        <StepperStep Label="طريقة الدفع" IconCss="sf-icon-payment"></StepperStep>
        <StepperStep Label="تأكيد الطلب" IconCss="sf-icon-success"></StepperStep>
    </StepperSteps>
</SfStepper>
```

## Combining Localization and RTL

```csharp
@using Syncfusion.Blazor.Navigations
@using System.Globalization

<div>
    <button @onclick="@(() => SetLanguage("en-US", false))">English</button>
    <button @onclick="@(() => SetLanguage("ar-AE", true))">العربية</button>
    <button @onclick="@(() => SetLanguage("he-IL", true))">עברית</button>
    <button @onclick="@(() => SetLanguage("fa-IR", true))">فارسی</button>
</div>

<SfStepper EnableRtl="@enableRtl">
    <StepperSteps>
        <StepperStep Label="@GetLocalizedLabel("step1")"></StepperStep>
        <StepperStep Label="@GetLocalizedLabel("step2")"></StepperStep>
        <StepperStep Label="@GetLocalizedLabel("step3")"></StepperStep>
        <StepperStep Label="@GetLocalizedLabel("step4")"></StepperStep>
    </StepperSteps>
</SfStepper>

@code {
    private Dictionary<string, Dictionary<string, string>> localizations = new()
    {
        { "en-US", new()
            {
                { "step1", "Step 1" },
                { "step2", "Step 2" },
                { "step3", "Step 3" },
                { "step4", "Step 4" }
            }
        },
        { "ar-AE", new()
            {
                { "step1", "الخطوة الأولى" },
                { "step2", "الخطوة الثانية" },
                { "step3", "الخطوة الثالثة" },
                { "step4", "الخطوة الرابعة" }
            }
        },
        { "he-IL", new()
            {
                { "step1", "שלב 1" },
                { "step2", "שלב 2" },
                { "step3", "שלב 3" },
                { "step4", "שלב 4" }
            }
        },
        { "fa-IR", new()
            {
                { "step1", "مرحله اول" },
                { "step2", "مرحله دوم" },
                { "step3", "مرحله سوم" },
                { "step4", "مرحله چهارم" }
            }
        }
    };
    
    private bool enableRtl = false;
    private string currentCulture = "en-US";
    
    private void SetLanguage(string culture, bool rtl)
    {
        currentCulture = culture;
        enableRtl = rtl;
        CultureInfo.CurrentCulture = new CultureInfo(culture);
        CultureInfo.CurrentUICulture = new CultureInfo(culture);
    }
    
    private string GetLocalizedLabel(string key)
    {
        if (localizations.TryGetValue(currentCulture, out var labels))
        {
            if (labels.TryGetValue(key, out var label))
            {
                return label;
            }
        }
        return key;
    }
}
```

## Performance Considerations

### Animation Performance Tips

1. **Disable animations for low-end devices** or accessibility features
2. **Use shorter durations** for mobile applications
3. **Reduce animation delay** for responsive feel

```csharp
private bool IsLowEndDevice()
{
    // Detect device capability and disable animations if needed
    return false;
}

<StepperAnimationSettings 
    Enable="@!IsLowEndDevice()" 
    Duration="@(IsLowEndDevice() ? 0 : 2000)">
</StepperAnimationSettings>
```

### Accessibility with Animations

Always consider users with motion sensitivity. Provide a way to disable animations:

```csharp
<div>
    <label>
        <input type="checkbox" @bind="prefersReducedMotion" />
        Reduce Motion
    </label>
</div>

<SfStepper>
    <StepperSteps>
        <!-- Steps -->
    </StepperSteps>
    <StepperAnimationSettings Enable="@!prefersReducedMotion"></StepperAnimationSettings>
</SfStepper>

@code {
    private bool prefersReducedMotion = false;
}
```
