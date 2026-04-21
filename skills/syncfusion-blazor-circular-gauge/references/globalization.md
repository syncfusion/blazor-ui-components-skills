# Globalization and Localization

## Overview

The CircularGauge supports internationalization (i18n) through the Syncfusion Blazor locale package. You can format numbers, currencies, and dates according to different cultures and enable right-to-left (RTL) rendering.

## Table of Contents
- [Overview](#overview)
- [Setting Up Globalization](#setting-up-globalization)
    - [Install Required Package](#install-required-package)
    - [Load CLDR Data](#load-cldr-data)
    - [Set Culture Dynamically](#set-culture-dynamically)
- [Number Formatting](#number-formatting)
    - [Standard Format Strings](#standard-format-strings)
    - [Currency Formatting](#currency-formatting)
    - [Percentage Formatting](#percentage-formatting)
- [Culture-Specific Examples](#culture-specific-examples)
    - [German (de-DE)](#german-de-de)
    - [Japanese (ja-JP)](#japanese-ja-jp)
    - [French (fr-FR)](#french-fr-fr)
- [Right-to-Left (RTL) Support](#right-to-left-rtl-support)
    - [Enable RTL](#enable-rtl)
    - [RTL with Arabic Culture](#rtl-with-arabic-culture)
- [Dynamic Culture Switching](#dynamic-culture-switching)
- [Tooltip Localization](#tooltip-localization)
- [Best Practices](#best-practices)
    - [Culture Management](#culture-management)
    - [Format Strings](#format-strings)
    - [RTL Support](#rtl-support)
    - [Performance](#performance)
    - [Testing](#testing)
- [Common Cultures](#common-cultures)
- [Troubleshooting](#troubleshooting)

## Setting Up Globalization

### Install Required Package

```bash
dotnet add package Syncfusion.Blazor.Locale
```

### Load CLDR Data

CLDR (Common Locale Data Repository) provides culture-specific formatting rules.

**Program.cs:**
```csharp
using Syncfusion.Blazor;
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });
builder.Services.AddSyncfusionBlazor();

var host = builder.Build();

// Load CLDR data for cultures
await host.RunAsync();
```

### Set Culture Dynamically

**_Imports.razor:**
```razor
@using System.Globalization
@using Syncfusion.Blazor
```

**Component:**
```cshtml
@using Syncfusion.Blazor.CircularGauge
@using System.Globalization
@inject IJSRuntime JSRuntime

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeAxisLabelStyle Format="c2">
            </CircularGaugeAxisLabelStyle>
            <CircularGaugePointers>
                <CircularGaugePointer Value="65">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

@code {
    protected override void OnInitialized()
    {
        CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE");
        CultureInfo.DefaultThreadCurrentUICulture = new CultureInfo("de-DE");
    }
}
```

## Number Formatting

### Standard Format Strings

Use standard .NET format strings for labels:

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeAxisLabelStyle Format="n2">
            </CircularGaugeAxisLabelStyle>
            <CircularGaugePointers>
                <CircularGaugePointer Value="75.456">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Common Format Strings:**
- `n0` - Number with no decimal places (75)
- `n1` - Number with 1 decimal place (75.5)
- `n2` - Number with 2 decimal places (75.46)
- `c` - Currency with culture-specific symbol
- `c0` - Currency with no decimals
- `p` - Percentage (0.75 → 75%)
- `p1` - Percentage with 1 decimal (0.755 → 75.5%)

### Currency Formatting

Display values as currency:

```cshtml
@using Syncfusion.Blazor.CircularGauge
@using System.Globalization

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="1000">
            <CircularGaugeAxisLabelStyle Format="c0">
            </CircularGaugeAxisLabelStyle>
            <CircularGaugePointers>
                <CircularGaugePointer Value="750">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
@code {
    protected override void OnInitialized()
    {
        // Set culture for Euro
        CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE");
        CultureInfo.DefaultThreadCurrentUICulture = new CultureInfo("de-DE");
    }
}
```

**Output (de-DE):** 750 €  
**Output (en-US):** $750  
**Output (ja-JP):** ¥750

### Percentage Formatting

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="1">
            <CircularGaugeAxisLabelStyle Format="p0">
            </CircularGaugeAxisLabelStyle>
            <CircularGaugePointers>
                <CircularGaugePointer Value="0.75">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Output:** 75% (formatted according to current culture)

## Culture-Specific Examples

### German (de-DE)

```cshtml

@using System.Globalization
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100000">
            <CircularGaugeAxisLabelStyle Format="n0">
            </CircularGaugeAxisLabelStyle>
            <CircularGaugePointers>
                <CircularGaugePointer Value="50000">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

@code {
    protected override void OnInitialized()
    {
        CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE");
        CultureInfo.DefaultThreadCurrentUICulture = new CultureInfo("de-DE");
    }
}

```

**Output:** 50.000 (German uses period as thousands separator)

### Japanese (ja-JP)

```cshtml
@using System.Globalization
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="10000">
            <CircularGaugeAxisLabelStyle Format="c0">
            </CircularGaugeAxisLabelStyle>
            <CircularGaugePointers>
                <CircularGaugePointer Value="5000">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
@code {
    protected override void OnInitialized()
    {
        CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("ja-JP");
        CultureInfo.DefaultThreadCurrentUICulture = new CultureInfo("ja-JP");
    }
}

```

**Output:** ¥5,000

### French (fr-FR)

```cshtml
@using System.Globalization
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeAxisLabelStyle Format="n2">
            </CircularGaugeAxisLabelStyle>
            <CircularGaugePointers>
                <CircularGaugePointer Value="75.5">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
@code {
    protected override void OnInitialized()
    {
        CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("fr-FR");
        CultureInfo.DefaultThreadCurrentUICulture = new CultureInfo("fr-FR");
    }
}

```

**Output:** 75,50 (French uses comma as decimal separator)

## Right-to-Left (RTL) Support

Enable RTL rendering for languages like Arabic and Hebrew.

### Enable RTL

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="60">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Effect:**
- Gauge elements render right-to-left
- Labels positioned for RTL reading
- Axis direction reverses if needed

### RTL with Arabic Culture

```cshtml
@using System.Globalization
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeAxisLabelStyle Format="n0">
            </CircularGaugeAxisLabelStyle>
            <CircularGaugePointers>
                <CircularGaugePointer Value="50">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

@code {
    protected override void OnInitialized()
    {
        CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("ar-SA");
        CultureInfo.DefaultThreadCurrentUICulture = new CultureInfo("ar-SA");
    }
}

```

## Dynamic Culture Switching

Allow users to change culture at runtime:

```cshtml
@using System.Globalization
@using Syncfusion.Blazor.CircularGauge

<div>
    <label>
        Select Culture:
        <select @onchange="ChangeCulture">
            <option value="en-US">English (US)</option>
            <option value="de-DE">German</option>
            <option value="fr-FR">French</option>
            <option value="ja-JP">Japanese</option>
            <option value="ar-SA">Arabic</option>
        </select>
    </label>
</div>

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="1000">
            <CircularGaugeAxisLabelStyle Format="c0">
            </CircularGaugeAxisLabelStyle>
            <CircularGaugePointers>
                <CircularGaugePointer Value="750">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

@code {
    private bool isRtl = false;
    
    void ChangeCulture(Microsoft.AspNetCore.Components.ChangeEventArgs e)
    {
        string culture = e.Value.ToString();
        CultureInfo.DefaultThreadCurrentCulture = new CultureInfo(culture);
        CultureInfo.DefaultThreadCurrentUICulture = new CultureInfo(culture);
        
        // Enable RTL for RTL languages
        isRtl = culture == "ar-SA" || culture == "he-IL";
        
        StateHasChanged();
    }
}
```

## Tooltip Localization

Format tooltip values according to culture:

```cshtml
@using System.Globalization
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeTooltipSettings 
        Enable="true" 
        Format="{value} €">
    </CircularGaugeTooltipSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="75.50">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
@code {
    protected override void OnInitialized()
    {
        CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE");
    }
}

```

**Tooltip displays:** 75,50 € (German format with comma as decimal)

## Best Practices

### Culture Management
- Set culture once in `OnInitialized` or `Program.cs`
- Use `CultureInfo.DefaultThreadCurrentCulture` for consistent formatting
- Store user preference in local storage or user profile

### Format Strings
- Use standard format strings (`n`, `c`, `p`) rather than custom patterns
- Include appropriate decimal places for your data
- Test formats with multiple cultures

### RTL Support
- Enable RTL for Arabic, Hebrew, Persian, Urdu
- Test layout with RTL enabled
- Ensure annotations and titles render correctly

### Performance
- Avoid changing culture frequently
- Set culture before rendering, not during
- Cache formatted strings if possible

### Testing
- Test with at least 3-4 different cultures
- Verify currency symbols display correctly
- Check decimal and thousands separators
- Test RTL layout thoroughly

## Common Cultures

| Culture Code | Language | Currency | Decimal Sep | Thousands Sep |
|--------------|----------|----------|-------------|---------------|
| en-US | English (US) | $ | . | , |
| en-GB | English (UK) | £ | . | , |
| de-DE | German | € | , | . |
| fr-FR | French | € | , | (space) |
| ja-JP | Japanese | ¥ | . | , |
| zh-CN | Chinese | ¥ | . | , |
| es-ES | Spanish | € | , | . |
| ar-SA | Arabic | ﷼ | . | , |
| ru-RU | Russian | ₽ | , | (space) |
| pt-BR | Portuguese (Brazil) | R$ | , | . |

## Troubleshooting

**Numbers not formatting correctly:**
- Verify culture is set before rendering
- Check format string syntax
- Ensure CLDR data is loaded

**Currency symbol wrong:**
- Set correct culture code (en-US vs en-GB)
- Verify culture data is installed
- Check browser locale settings

**RTL not working:**
- Set `EnableRtl="true"` on `SfCircularGauge`
- Verify RTL CSS is not being overridden
- Test with known RTL culture (ar-SA)

**Culture change not reflecting:**
- Call `StateHasChanged()` after changing culture
- Recreate gauge component or force re-render
- Check culture is set on correct thread
