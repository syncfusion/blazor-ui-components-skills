# Accessibility and Internationalization in Blazor Linear Gauge

## Table of Contents
- [Overview](#overview)
- [Accessibility Features](#accessibility-features)
  - [WCAG Compliance](#wcag-compliance)
  - [Screen Reader Support](#screen-reader-support)
  - [ARIA Attributes](#aria-attributes)
  - [High Contrast Mode](#high-contrast-mode)
- [Internationalization](#internationalization)
  - [Number Formatting](#number-formatting)
  - [Locale-Based Formatting](#locale-based-formatting)
  - [Custom Format Strings](#custom-format-strings)
- [Best Practices](#best-practices)
- [Complete Examples](#complete-examples)

## Overview

The Blazor Linear Gauge provides comprehensive accessibility and internationalization features:

**Accessibility:**
- WCAG 2.1 compliance
- Screen reader compatibility
- ARIA attributes
- High contrast mode support

**Internationalization:**
- Number formatting for different locales
- Custom format strings
- Culture-specific rendering

These features ensure your gauges are usable by everyone, regardless of ability or language.

## Accessibility Features

### WCAG Compliance

The Linear Gauge follows Web Content Accessibility Guidelines (WCAG) 2.1 standards.

**Built-in compliance features:**
- **Perceivable:** Visual elements have sufficient contrast
- **Operable:** Fully keyboard accessible
- **Understandable:** Clear labels and structure
- **Robust:** Compatible with assistive technologies

**Example of accessible gauge:**

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Title="Temperature Monitor" Width="250px" Height="400px">
    <LinearGaugeTitleStyle Color="#212529" Size="18px" FontWeight="bold">
    </LinearGaugeTitleStyle>
    
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="50">
            <LinearGaugeAxisLabelStyle Format="{value}°C">
                <LinearGaugeAxisLabelFont Color="#212529" Size="12px">
                </LinearGaugeAxisLabelFont>
            </LinearGaugeAxisLabelStyle>
            
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="22" 
                                    Color="#007bff"
                                    EnableDrag="true">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Accessibility features:**
- Clear title describes gauge purpose
- High-contrast colors (text: #212529, pointer: #007bff)
- Sufficient text size (18px title, 12px labels)
- Unit labels provide context (°C)

### Screen Reader Support

Linear Gauge provides semantic information to screen readers.

**Screen reader features:**
- Gauge announces its purpose via title
- Pointer values are announced when changed
- Tooltips are readable by screen readers
- Status changes are communicated

**Example with screen reader optimization:**

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Title="Battery Level">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeAxisLabelStyle Format="{value}%">
            </LinearGaugeAxisLabelStyle>
            
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="@batteryLevel" 
                                    Type="Syncfusion.Blazor.LinearGauge.Point.Bar"
                                    Width="30"
                                    Color="@batteryColor">
                </LinearGaugePointer>
            </LinearGaugePointers>
            
            <LinearGaugeRanges>
                <LinearGaugeRange Start="0" End="20" 
                                  Color="#dc3545">
                </LinearGaugeRange>
                <LinearGaugeRange Start="20" End="50" 
                                  Color="#ffc107">
                </LinearGaugeRange>
                <LinearGaugeRange Start="50" End="100" 
                                  Color="#28a745">
                </LinearGaugeRange>
            </LinearGaugeRanges>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

<div role="status" aria-live="polite">
    Battery level: @batteryLevel%. Status: @batteryStatus
</div>

@code {
    private double batteryLevel = 75;
    private string batteryColor = "#28a745";
    private string batteryStatus = "Good";
    
    protected override void OnInitialized()
    {
        UpdateBatteryStatus();
    }
    
    private void UpdateBatteryStatus()
    {
        if (batteryLevel < 20)
        {
            batteryStatus = "Critical - Charge soon";
            batteryColor = "#dc3545";
        }
        else if (batteryLevel < 50)
        {
            batteryStatus = "Low";
            batteryColor = "#ffc107";
        }
        else
        {
            batteryStatus = "Good";
            batteryColor = "#28a745";
        }
    }
}
```

**Screen reader announces:**
- "Battery Level Indicator"
- "Battery level: 75%. Status: Good"
- Range context when navigating

### ARIA Attributes

Use ARIA (Accessible Rich Internet Applications) attributes to enhance accessibility.

**Supported ARIA attributes:**

```razor
@using Syncfusion.Blazor.LinearGauge

<div role="group" aria-labelledby="gauge-title">
    <h3 id="gauge-title">System Performance</h3>
    
    <SfLinearGauge>
        <LinearGaugeAxes>
            <LinearGaugeAxis Minimum="0" Maximum="100">
                <LinearGaugeAxisLabelStyle Format="{value}%">
                </LinearGaugeAxisLabelStyle>
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="@cpuUsage" 
                                        EnableDrag="true">
                    </LinearGaugePointer>
                </LinearGaugePointers>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </SfLinearGauge>
</div>

@code {
    private double cpuUsage = 68;
}
```

**ARIA attributes used:**
- `role="group"` - Groups related elements
- `aria-labelledby` - Associates label with gauge
- `aria-label` - Provides accessible name
- `aria-valuemin` - Minimum value
- `aria-valuemax` - Maximum value
- `aria-valuenow` - Current value
- `aria-valuetext` - Human-readable value description

### High Contrast Mode

Design gauges that work well in high contrast mode for users with visual impairments.

**High contrast principles:**
- Use distinct colors with high contrast ratios
- Avoid relying solely on color to convey information
- Provide text labels alongside visual indicators
- Ensure borders are visible

**Example:**

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Title="Temperature" Width="250px" Height="400px">
    <LinearGaugeTitleStyle Color="#000000" 
                          Size="18px" 
                          FontWeight="bold">
    </LinearGaugeTitleStyle>
    
    <LinearGaugeBorder Color="#000000" Width="2">
    </LinearGaugeBorder>
    
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="50">
            <LinearGaugeLine Color="#000000" Width="2">
            </LinearGaugeLine>
            
            <LinearGaugeAxisLabelStyle Format="{value}°C">
                <LinearGaugeAxisLabelFont Color="#000000" 
                                          Size="12px" 
                                          FontWeight="600">
                </LinearGaugeAxisLabelFont>
            </LinearGaugeAxisLabelStyle>
            
            <LinearGaugeMajorTicks Height="10" 
                                   Width="2" 
                                   Color="#000000">
            </LinearGaugeMajorTicks>
            
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="22" 
                                    Color="#0000FF"
                                    Width="20"
                                    Height="20">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**High contrast features:**
- Black text and borders
- Bold fonts for better visibility
- Larger pointer size
- Strong color contrast (black on white, blue pointer)
- Thicker lines and borders

## Internationalization

### Number Formatting

Format numbers according to different locales and cultures.

**Basic number formatting:**

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Format="n2">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100000">
            <LinearGaugeAxisLabelStyle>
            </LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="45000">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Format specifiers:**
- `N2` - Number with 2 decimal places (45000.00)
- `C` - Currency format ($45,000.00)
- `P` - Percentage format (45%)
- `{value}` - Raw value

### Locale-Based Formatting

Apply culture-specific formatting using .NET globalization.

**Set culture in Program.cs:**

```csharp
using System.Globalization;

var builder = WebApplication.CreateBuilder(args);

// Set default culture
var culture = new CultureInfo("fr-FR");
CultureInfo.DefaultThreadCurrentCulture = culture;
CultureInfo.DefaultThreadCurrentUICulture = culture;

builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();
// ... rest of configuration
```

**French locale example:**

```razor
@using Syncfusion.Blazor.LinearGauge
@using System.Globalization

<div>
    <p>Locale: @CultureInfo.CurrentCulture.Name</p>
</div>

<SfLinearGauge Title="Température">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="50">
            <LinearGaugeAxisLabelStyle Format="{value}°C">
            </LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="22.5">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Numbers display with French formatting (22,5 instead of 22.5).

### Custom Format Strings

Create custom formats for specific requirements.

#### Currency with Custom Symbol

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Title="Revenue">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="10000">
            <LinearGaugeAxisLabelStyle Format="€{value}">
            </LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="7500">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Labels show "€0", "€2500", "€5000", etc.

#### Time Format

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Title="Time Elapsed">
    <LinearGaugeEvents AxisLabelRendering="OnAxisLabelRendering">
    </LinearGaugeEvents>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="120">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="45">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private void OnAxisLabelRendering(Syncfusion.Blazor.LinearGauge.AxisLabelRenderEventArgs args)
    {
        int minutes = (int)args.AxisValue;
        int hours = minutes / 60;
        int mins = minutes % 60;
        args.Text = $"{hours:D2}:{mins:D2}";
    }
}
```

**Result:** Labels show "00:00", "00:30", "01:00", "01:30", "02:00".

#### Metric Units

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Title="Distance">
    <LinearGaugeEvents AxisLabelRendering="OnAxisLabelRendering">
    </LinearGaugeEvents>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="10000">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="2500">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private void OnAxisLabelRendering(Syncfusion.Blazor.LinearGauge.AxisLabelRenderEventArgs args)
    {
        double meters = args.AxisValue;
        
        if (meters >= 1000)
        {
            args.Text = $"{meters / 1000:F1} km";
        }
        else
        {
            args.Text = $"{meters:F0} m";
        }
    }
}
```

**Result:** Labels show "0 m", "2.5 km", "5.0 km", "7.5 km", "10.0 km".

#### Abbreviated Numbers

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge Title="Population">
    <LinearGaugeEvents AxisLabelRendering="OnAxisLabelRendering">
    </LinearGaugeEvents>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="1000000">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="350000">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private void OnAxisLabelRendering(Syncfusion.Blazor.LinearGauge.AxisLabelRenderEventArgs args)
    {
        double value = args.AxisValue;
        
        if (value >= 1000000)
        {
            args.Text = $"{value / 1000000:F1}M";
        }
        else if (value >= 1000)
        {
            args.Text = $"{value / 1000:F0}K";
        }
        else
        {
            args.Text = $"{value:F0}";
        }
    }
}
```

**Result:** Labels show "0", "250K", "500K", "750K", "1.0M".

## Best Practices

### Accessibility Best Practices

1. **Always provide a title** - Describes the gauge's purpose
2. **Use ARIA attributes** - Enhance screen reader support
3. **High contrast colors** - Ensure visibility for all users
4. **Text alternatives** - Provide text values alongside visual indicators
5. **Sufficient size** - Make interactive elements large enough (minimum 44×44px for touch)
6. **Focus indicators** - Ensure focused elements are clearly visible
7. **Avoid color-only communication** - Use patterns, labels, or text

### Internationalization Best Practices

1. **Set culture early** - Configure in Program.cs or App.razor
2. **Use format specifiers** - Leverage .NET formatting (N0, C, P)
3. **Test with multiple locales** - Verify appearance in target cultures
4. **Localize all text** - Titles, tooltips, annotations
5. **Consider number systems** - Some cultures use different digit symbols
6. **Date/time awareness** - Format time-based values appropriately
7. **Unit localization** - Use appropriate units (Celsius vs Fahrenheit, km vs miles)

## Complete Examples

### Fully Accessible Dashboard Gauge

```razor
@using Syncfusion.Blazor.LinearGauge

<div class="accessible-gauge" role="region" aria-labelledby="gauge-title">
    <h3 id="gauge-title">Server CPU Usage</h3>
    <p id="gauge-description">
        Monitor real-time CPU usage. Use arrow keys to simulate CPU load.
    </p>
    
    <SfLinearGauge>
        <LinearGaugeEvents ValueChange="OnValueChange">
        </LinearGaugeEvents>
        
        <LinearGaugeTooltipSettings Enable="true" 
                                    Format="{value}% CPU">
            <LinearGaugeTooltipTextStyle Size="14px" FontWeight="bold">
            </LinearGaugeTooltipTextStyle>
        </LinearGaugeTooltipSettings>
        
        <LinearGaugeAxes>
            <LinearGaugeAxis Minimum="0" Maximum="100">
                <LinearGaugeAxisLabelStyle Format="{value}%">
                    <LinearGaugeAxisLabelFont Color="#212529" 
                                              Size="12px" 
                                              FontWeight="600">
                    </LinearGaugeAxisLabelFont>
                </LinearGaugeAxisLabelStyle>
                
                <LinearGaugeMajorTicks Height="10" Color="#495057">
                </LinearGaugeMajorTicks>
                
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="@cpuUsage" 
                                        EnableDrag="true"
                                        Color="@pointerColor"
                                        Width="15"
                                        Height="15">
                    </LinearGaugePointer>
                </LinearGaugePointers>
                
                <LinearGaugeRanges>
                    <LinearGaugeRange Start="0" End="50" 
                                      Color="#28a745">
                    </LinearGaugeRange>
                    <LinearGaugeRange Start="50" End="75" 
                                      Color="#ffc107">
                    </LinearGaugeRange>
                    <LinearGaugeRange Start="75" End="100" 
                                      Color="#dc3545">
                    </LinearGaugeRange>
                </LinearGaugeRanges>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </SfLinearGauge>
    
    <div role="status" aria-live="polite" aria-atomic="true" class="status-message">
        CPU Usage: @cpuUsage%. Status: @GetStatus()
    </div>
    
    <div class="legend" role="list">
        <div role="listitem">
            <span class="legend-color" style="background: #28a745;"></span>
            <span>Normal (0-50%)</span>
        </div>
        <div role="listitem">
            <span class="legend-color" style="background: #ffc107;"></span>
            <span>Warning (50-75%)</span>
        </div>
        <div role="listitem">
            <span class="legend-color" style="background: #dc3545;"></span>
            <span>Critical (75-100%)</span>
        </div>
    </div>
</div>

<style>
    .accessible-gauge {
        max-width: 400px;
        padding: 20px;
        background: white;
        border: 2px solid #dee2e6;
        border-radius: 8px;
    }
    .accessible-gauge h3 {
        margin-top: 0;
        color: #212529;
    }
    .status-message {
        margin-top: 15px;
        padding: 10px;
        background: #e9ecef;
        border-radius: 4px;
        font-weight: 600;
    }
    .legend {
        margin-top: 15px;
        display: flex;
        flex-direction: column;
        gap: 8px;
    }
    .legend > div {
        display: flex;
        align-items: center;
        gap: 10px;
    }
    .legend-color {
        width: 20px;
        height: 20px;
        border-radius: 3px;
        border: 1px solid #dee2e6;
    }
</style>

@code {
    private double cpuUsage = 42;
    private string pointerColor = "#28a745";
    
    private void OnValueChange(ValueChangeEventArgs args)
    {
        cpuUsage = Math.Round(args.Value, 0);
        UpdatePointerColor();
    }
    
    private void UpdatePointerColor()
    {
        if (cpuUsage < 50)
            pointerColor = "#28a745";
        else if (cpuUsage < 75)
            pointerColor = "#ffc107";
        else
            pointerColor = "#dc3545";
    }
    
    private string GetStatus()
    {
        if (cpuUsage < 50) return "Normal";
        if (cpuUsage < 75) return "Warning";
        return "Critical";
    }
}
```

### Multi-Locale Temperature Gauge

```razor
@using Syncfusion.Blazor.LinearGauge
@using System.Globalization

<div class="locale-selector">
    <label for="locale-select">Select Locale:</label>
    <select id="locale-select" @onchange="ChangeLocale">
        <option value="en-US">English (US)</option>
        <option value="fr-FR">Français (France)</option>
        <option value="de-DE">Deutsch (Deutschland)</option>
        <option value="ja-JP">日本語 (Japan)</option>
        <option value="ar-SA">العربية (Saudi Arabia)</option>
    </select>
</div>

<div>
    <SfLinearGauge Title="@GetLocalizedTitle()" 
                   Width="250px" 
                   Height="400px">
        <LinearGaugeTitleStyle FontWeight="bold" Size="18px">
        </LinearGaugeTitleStyle>
        
        <LinearGaugeAxes>
            <LinearGaugeAxis Minimum="0" Maximum="50">
                <LinearGaugeAxisLabelStyle Format="@labelFormat">
                </LinearGaugeAxisLabelStyle>
                
                <LinearGaugeMajorTicks Interval="10"></LinearGaugeMajorTicks>
                <LinearGaugeMinorTicks Interval="5"></LinearGaugeMinorTicks>
                
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="@temperature">
                    </LinearGaugePointer>
                </LinearGaugePointers>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </SfLinearGauge>
    
    <div class="temperature-display">
        @GetFormattedTemperature()
    </div>
</div>

<style>
    .locale-selector {
        margin-bottom: 20px;
    }
    .locale-selector label {
        margin-right: 10px;
        font-weight: 600;
    }
    .locale-selector select {
        padding: 5px 10px;
        border: 1px solid #ced4da;
        border-radius: 4px;
    }
    .temperature-display {
        margin-top: 15px;
        text-align: center;
        font-size: 20px;
        font-weight: bold;
        color: #007bff;
    }
</style>

@code {
    private double temperature = 22.5;
    private string currentLocale = "en-US";
    private string labelFormat = "{value}°C";
    
    private void ChangeLocale(Microsoft.AspNetCore.Components.ChangeEventArgs e)
    {
        currentLocale = e.Value.ToString();
        var culture = new CultureInfo(currentLocale);
        CultureInfo.CurrentCulture = culture;
        CultureInfo.CurrentUICulture = culture;
        
        StateHasChanged();
    }
    
    private string GetLocalizedTitle()
    {
        return currentLocale switch
        {
            "en-US" => "Temperature",
            "fr-FR" => "Température",
            "de-DE" => "Temperatur",
            "ja-JP" => "温度",
            "ar-SA" => "درجة الحرارة",
            _ => "Temperature"
        };
    }
    
    private string GetFormattedTemperature()
    {
        var formatted = temperature.ToString("N1", CultureInfo.CurrentCulture);
        
        return currentLocale switch
        {
            "en-US" => $"Current: {formatted}°C",
            "fr-FR" => $"Actuel: {formatted}°C",
            "de-DE" => $"Aktuell: {formatted}°C",
            "ja-JP" => $"現在: {formatted}°C",
            "ar-SA" => $"الحالي: {formatted}°م",
            _ => $"Current: {formatted}°C"
        };
    }
}
```

## Key Takeaways

**Accessibility:**
1. **WCAG Compliance** - Follow Web Content Accessibility Guidelines
2. **Screen Readers** - Use semantic HTML and ARIA attributes
3. **High Contrast** - Use distinct colors with sufficient contrast
4. **Focus Indicators** - Make focused elements clearly visible
5. **Text Alternatives** - Provide context beyond color

**Internationalization:**
6. **Set Culture** - Configure globalization in Program.cs
7. **Format Specifiers** - Use N0, N2, C, P for locale-aware formatting
8. **Custom Formats** - Create formats with AxisLabelRendering event
9. **Localize Text** - Translate titles, tooltips, and labels
10. **Test Locales** - Verify appearance in target cultures

Making gauges accessible and internationalized ensures they work for all users, regardless of ability or language, expanding your application's reach and usability.
