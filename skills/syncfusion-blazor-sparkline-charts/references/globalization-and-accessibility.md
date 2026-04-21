# Globalization and Accessibility

## Table of Contents
- [Globalization Overview](#globalization-overview)
- [Number and Currency Formatting](#number-and-currency-formatting)
- [Date and Time Formatting](#date-and-time-formatting)
- [Right-to-Left (RTL) Support](#right-to-left-rtl-support)
- [Accessibility Overview](#accessibility-overview)
- [WCAG Compliance](#wcag-compliance)
- [Keyboard Navigation](#keyboard-navigation)
- [Screen Reader Support](#screen-reader-support)
- [ARIA Attributes](#aria-attributes)
- [Color Contrast](#color-contrast)

## Globalization Overview

Globalization enables sparklines to adapt to different cultures, languages, and regions through localized number, currency, and date formatting.

## Number and Currency Formatting

### Currency Formatting

Display values in currency format with culture-specific symbols:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@RevenueData" 
              Height="60px" 
              Width="250px"
              Format="C">
    <SparklineTooltipSettings TValue="double" Visible="true">
    </SparklineTooltipSettings>
</SfSparkline>

@code {
    public double[] RevenueData = new double[] { 300.00, 600.00, 400.21, 100.20, 300.70, 200.04, 500.00 };
}
```

**Result:** Displays "$300.00", "$600.00", etc. (based on current culture)

### Custom Currency Format

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@SalesData" 
              Height="60px" 
              Width="250px"
              Format="C2">
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.Start, VisibleType.End }">
    </SparklineDataLabelSettings>
    <SparklineTooltipSettings TValue="double" Visible="true">
    </SparklineTooltipSettings>
</SfSparkline>

@code {
    public double[] SalesData = new double[] { 45000.5, 52000.75, 48000.25, 55000.00 };
}
```

**Format codes:**
- `C` or `C2` - Currency with 2 decimal places
- `C0` - Currency without decimals
- `C3` - Currency with 3 decimal places

### Number Formatting with Grouping

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@LargeNumbers" 
              Height="60px" 
              Width="300px"
              Format="N0"
              EnableGroupingSeparator="true">
    <SparklineTooltipSettings TValue="int" Visible="true">
    </SparklineTooltipSettings>
</SfSparkline>

@code {
    public int[] LargeNumbers = new int[] { 30000, 60000, 40000, 10000, 30000, 20000, 50000 };
}
```

**Result:** Displays "30,000", "60,000", etc.

**Format codes:**
- `N` or `N2` - Number with 2 decimal places and grouping
- `N0` - Integer with grouping separator
- `P` - Percentage format
- `P0` - Percentage without decimals

### Percentage Formatting

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@PercentageData" 
              Height="60px" 
              Width="250px"
              Format="P1">
    <SparklineTooltipSettings TValue="double" Visible="true">
    </SparklineTooltipSettings>
</SfSparkline>

@code {
    public double[] PercentageData = new double[] { 0.15, 0.28, 0.22, 0.35, 0.30 };
}
```

**Result:** Displays "15.0%", "28.0%", etc.

## Date and Time Formatting

### DateTime Value Formatting

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@TimeSeriesData" 
              TValue="TimeData"
              XName="Date" 
              YName="Value"
              ValueType="SparklineValueType.DateTime"
              Height="60px" 
              Width="350px">
    <SparklineTooltipSettings TValue="TimeData" 
                              Visible="true"
                              Format="${Date:MMM dd, yyyy}: ${Value}">
    </SparklineTooltipSettings>
</SfSparkline>

@code {
    public class TimeData
    {
        public DateTime Date { get; set; }
        public double Value { get; set; }
    }

    public List<TimeData> TimeSeriesData = new List<TimeData>
    {
        new TimeData { Date = new DateTime(2026, 1, 1), Value = 145 },
        new TimeData { Date = new DateTime(2026, 2, 1), Value = 162 },
        new TimeData { Date = new DateTime(2026, 3, 1), Value = 178 }
    };
}
```

**Date format codes:**
- `MMM dd, yyyy` - "Jan 01, 2026"
- `MM/dd/yyyy` - "01/01/2026"
- `yyyy-MM-dd` - "2026-01-01"
- `dd MMM` - "01 Jan"

### Culture-Specific Formatting

Configure culture in `Program.cs`:

```csharp
using System.Globalization;
using Microsoft.AspNetCore.Localization;

var builder = WebApplication.CreateBuilder(args);

// Configure localization
builder.Services.Configure<RequestLocalizationOptions>(options =>
{
    var supportedCultures = new[]
    {
        new CultureInfo("en-US"),
        new CultureInfo("de-DE"),
        new CultureInfo("fr-FR"),
        new CultureInfo("ja-JP")
    };
    
    options.DefaultRequestCulture = new RequestCulture("en-US");
    options.SupportedCultures = supportedCultures;
    options.SupportedUICultures = supportedCultures;
});

builder.Services.AddSyncfusionBlazor();

var app = builder.Build();
app.UseRequestLocalization();
```

**Culture-specific examples:**
- `de-DE`: "1.234,56 €" (German)
- `en-US`: "$1,234.56" (US)
- `fr-FR`: "1 234,56 €" (French)
- `ja-JP`: "¥1,234" (Japanese)

## Right-to-Left (RTL) Support

### Enable RTL

For right-to-left languages (Arabic, Hebrew, etc.):

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@SalesData" 
              EnableRtl="true"
              Height="60px" 
              Width="300px"
              Format="C2">
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }" 
                                EdgeLabelMode="EdgeLabelMode.Shift">
    </SparklineDataLabelSettings>
    <SparklinePadding Top="25" Left="10" Right="10"></SparklinePadding>
</SfSparkline>

@code {
    public double[] SalesData = new double[] { 300.00, 600.00, 400.21, 100.20, 300.70, 200.04, 500.00 };
}
```

**RTL Effects:**
- Sparkline renders right-to-left
- Data labels positioned for RTL reading
- Tooltip text aligned right

### RTL with Arabic Numerals

```razor
@using System.Globalization
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@Data" 
              EnableRtl="true"
              Height="60px" 
              Width="300px">
    <SparklineTooltipSettings TValue="double" Visible="true">
    </SparklineTooltipSettings>
</SfSparkline>

@code {
    public double[] Data = new double[] { 50, 75, 60, 90, 80 };
    
    protected override void OnInitialized()
    {
        CultureInfo.CurrentCulture = new CultureInfo("ar-SA");
        CultureInfo.CurrentUICulture = new CultureInfo("ar-SA");
    }
}
```

## Accessibility Overview

Syncfusion Blazor Sparkline meets international accessibility standards:

**Compliance:**
- ✅ WCAG 2.2 Level AA
- ✅ Section 508
- ✅ ADA compliance
- ✅ Screen reader support
- ✅ Keyboard navigation
- ✅ Color contrast requirements
- ✅ Mobile device support

## WCAG Compliance

### Color Contrast

Ensure sufficient contrast between sparkline and background:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@Data" 
              Fill="#0066cc"
              LineWidth="2"
              Height="60px" 
              Width="250px">
    <SparklineContainerArea>
        <SparklineContainerAreaBackground Color="#ffffff"></SparklineContainerAreaBackground>
    </SparklineContainerArea>
</SfSparkline>

@code {
    public int[] Data = new int[] { 3, 6, 4, 1, 3, 2, 5 };
}
```

**WCAG AA Requirements:**
- Normal text: 4.5:1 contrast ratio
- Large text: 3:1 contrast ratio
- Non-text elements: 3:1 contrast ratio

**Recommended combinations:**
- Dark blue (#0066cc) on white
- Dark green (#2e7d32) on white
- Black/dark gray (#333) on white
- White on dark blue (#003d82)

### High Contrast Mode

Support users with high contrast preferences:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@Data" 
              Fill="#000000"
              LineWidth="3"
              Height="60px" 
              Width="250px">
    <SparklineContainerArea>
        <SparklineContainerAreaBackground Color="#ffffff"></SparklineContainerAreaBackground>
        <SparklineContainerAreaBorder Color="#000000" Width="2"></SparklineContainerAreaBorder>
    </SparklineContainerArea>
</SfSparkline>

@code {
    public int[] Data = new int[] { 3, 6, 4, 1, 3, 2, 5 };
}
```

## Keyboard Navigation

Sparkline supports full keyboard accessibility:

### Keyboard Shortcuts

| Key | Action |
|-----|--------|
| <kbd>Tab</kbd> | Move focus to sparkline |
| <kbd>Shift + Tab</kbd> | Move focus to previous element |
| <kbd>→</kbd> / <kbd>↓</kbd> | Navigate to next data point |
| <kbd>←</kbd> / <kbd>↑</kbd> | Navigate to previous data point |
| <kbd>Esc</kbd> | Close tooltip |
| <kbd>Ctrl + P</kbd> | Print sparkline (Windows) |
| <kbd>⌘ + P</kbd> | Print sparkline (Mac) |

### Enable Keyboard Navigation

Ensure sparkline is focusable:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline @ref="sparklineRef"
              DataSource="@Data" 
              Height="60px" 
              Width="250px"
              >
    <SparklineTooltipSettings TValue="int" Visible="true">
    </SparklineTooltipSettings>
</SfSparkline>

@code {
    private SfSparkline<int> sparklineRef;
    public int[] Data = new int[] { 3, 6, 4, 1, 3, 2, 5 };
}
```

## Screen Reader Support

### ARIA Labels

Provide meaningful descriptions for screen readers:

```razor
@using Syncfusion.Blazor.Charts

<div role="region" aria-label="Monthly Sales Trend Chart">
    <SfSparkline DataSource="@SalesData" 
                  Height="60px" 
                  Width="300px">
        <SparklineTooltipSettings TValue="int" Visible="true" Format="Sales: ${y}">
        </SparklineTooltipSettings>
    </SfSparkline>
</div>

@code {
    public int[] SalesData = new int[] { 45, 52, 48, 55, 60, 58, 65 };
}
```

### Descriptive Context

Add text alternatives for screen reader users:

```razor
@using Syncfusion.Blazor.Charts

<div class="sparkline-container">
    <h4 id="sparkline-title">Revenue Trend Q1 2026</h4>
    <p class="sr-only">Line chart showing revenue increasing from $45K to $65K over 7 months</p>
    
    <SfSparkline DataSource="@RevenueData" 
                  Height="60px" 
                  Width="300px">
    </SfSparkline>
</div>

<style>
    .sr-only {
        position: absolute;
        width: 1px;
        height: 1px;
        padding: 0;
        margin: -1px;
        overflow: hidden;
        clip: rect(0, 0, 0, 0);
        border: 0;
    }
</style>

@code {
    public int[] RevenueData = new int[] { 45, 52, 48, 55, 60, 58, 65 };
}
```

## ARIA Attributes

Sparkline uses these ARIA attributes automatically:

**Role Attributes:**
- `role="img"` - Identifies sparkline as an image
- `role="region"` - Marks sparkline container as a landmark

**ARIA Properties:**
- `aria-label` - Provides text description
- `aria-hidden` - Hides decorative elements from screen readers
- `aria-pressed` - Indicates toggle state for interactive features

## Color Contrast

### Verify Contrast Ratios

**Tools for checking:**
- WebAIM Contrast Checker
- Chrome DevTools Lighthouse
- axe DevTools browser extension

### Accessible Color Palettes

**Safe combinations:**

```razor
@using Syncfusion.Blazor.Charts

<!-- High contrast: Dark on light -->
<SfSparkline DataSource="@Data" Fill="#003d82" LineWidth="2"></SfSparkline>

<!-- High contrast: Light on dark -->
<SfSparkline DataSource="@Data" Fill="#ffffff" LineWidth="2">
    <SparklineContainerArea Background="#1a1a1a">
    </SparklineContainerArea>
</SfSparkline>

<!-- Accessible brand colors -->
<SfSparkline DataSource="@Data" Fill="#0077b6" LineWidth="2"></SfSparkline>
@code {
    private SfSparkline<int> sparklineRef;
    public int[] Data = new int[] { 3, 6, 4, 1, 3, 2, 5 };
}
```

### Multiple Sparklines with Contrast

```razor
@using Syncfusion.Blazor.Charts

<div class="dashboard" style="background-color: #f5f5f5;">
    <SfSparkline DataSource="@Data1" Fill="#0066cc" LineWidth="2" Height="50px" Width="150px">
    </SfSparkline>
    
    <SfSparkline DataSource="@Data2" Fill="#2e7d32" LineWidth="2" Height="50px" Width="150px">
    </SfSparkline>
    
    <SfSparkline DataSource="@Data3" Fill="#d32f2f" LineWidth="2" Height="50px" Width="150px">
    </SfSparkline>
</div>

@code {
    public int[] Data1 = { 3, 6, 4, 1, 3, 2, 5 };
    public int[] Data2 = { 2, 4, 6, 8, 6, 4, 2 };
    public int[] Data3 = { 5, 3, 4, 6, 2, 1, 3 };
}
```

**All colors meet WCAG AA standards against #f5f5f5 background.**

## Complete Accessible Example

```razor
@using Syncfusion.Blazor.Charts

<div role="region" 
     aria-label="Quarterly Performance Dashboard" 
     class="accessible-sparkline-container">
    
    <div class="metric-card">
        <h3 id="revenue-title">Revenue Trend</h3>
        <p class="sr-only">Line chart showing quarterly revenue from Q1 to Q4 2026, ranging from $250K to $380K</p>
        
        <SfSparkline DataSource="@RevenueData" 
                      TValue="QuarterData"
                      XName="Quarter"
                      YName="Amount"
                      ValueType="SparklineValueType.Category"
                      Type="SparklineType.Area"
                      Height="60px" 
                      Width="250px"
                      Fill="#0066cc"
                      Opacity="0.6"
                      EnableRtl="false"
                      Format="C0"
                     EnableGroupingSeparator="true">
            
            <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }" 
                                     Size="5"
                                     Fill="#ffffff">
                <SparklineMarkerBorder Color="#0066cc" Width="2" />
            </SparklineMarkerSettings>
            <SparklineTooltipSettings TValue="QuarterData" 
                                      Visible="true"
                                      Format="${Quarter}: ${Amount:C0}">
            </SparklineTooltipSettings>
            
            <SparklineContainerArea Background="#ffffff">
                <SparklineContainerAreaBorder Color="#d0d0d0" Width="1"></SparklineContainerAreaBorder>
            </SparklineContainerArea>
            
            <SparklinePadding Top="10" Right="10" Bottom="10" Left="10"></SparklinePadding>
        </SfSparkline>
        
        <p class="metric-summary">
            <strong>$380K</strong> in Q4 2026 
            <span class="change positive" aria-label="15% increase">+15%</span>
        </p>
    </div>
</div>

<style>
    .sr-only {
        position: absolute;
        width: 1px;
        height: 1px;
        padding: 0;
        margin: -1px;
        overflow: hidden;
        clip: rect(0, 0, 0, 0);
        border: 0;
    }
    
    .metric-card {
        border: 1px solid #d0d0d0;
        border-radius: 4px;
        padding: 16px;
        background: #ffffff;
    }
    
    .metric-summary {
        margin-top: 12px;
        font-size: 14px;
    }
    
    .change {
        padding: 2px 6px;
        border-radius: 3px;
        font-size: 12px;
        font-weight: bold;
    }
    
    .change.positive {
        background-color: #e8f5e9;
        color: #2e7d32;
    }
</style>

@code {
    public class QuarterData
    {
        public string Quarter { get; set; }
        public double Amount { get; set; }
    }

    public List<QuarterData> RevenueData = new List<QuarterData>
    {
        new QuarterData { Quarter = "Q1", Amount = 250000 },
        new QuarterData { Quarter = "Q2", Amount = 320000 },
        new QuarterData { Quarter = "Q3", Amount = 290000 },
        new QuarterData { Quarter = "Q4", Amount = 380000 }
    };
}
```

**This example demonstrates:**
- ✅ ARIA labels and roles
- ✅ Screen reader-friendly descriptions
- ✅ High contrast colors (WCAG AA compliant)
- ✅ Keyboard navigation support
- ✅ Semantic HTML structure
- ✅ Currency formatting with grouping separators
- ✅ Visible focus indicators
- ✅ Clear visual hierarchy

## Best Practices

### Accessibility Checklist

- [ ] Add `aria-label` or `aria-labelledby` to sparklines
- [ ] Provide text alternatives for screen readers
- [ ] Ensure color contrast meets WCAG AA (4.5:1)
- [ ] Test keyboard navigation (Tab, Arrow keys, Esc)
- [ ] Enable tooltips for detailed information
- [ ] Use semantic HTML structure
- [ ] Test with screen readers (NVDA, JAWS, VoiceOver)
- [ ] Support high contrast modes
- [ ] Provide context with headings and labels
- [ ] Test on mobile devices

### Globalization Checklist

- [ ] Use `Format` property for number/currency/date formatting
- [ ] Set appropriate culture in `Program.cs`
- [ ] Enable `UseGroupingSeparator` for large numbers
- [ ] Set `EnableRtl="true"` for RTL languages
- [ ] Test with multiple cultures (en-US, de-DE, ar-SA, ja-JP)
- [ ] Verify date formats match culture expectations
- [ ] Check decimal separators and grouping
- [ ] Test currency symbols display correctly

**Accessibility and globalization ensure sparklines work for all users worldwide.**
