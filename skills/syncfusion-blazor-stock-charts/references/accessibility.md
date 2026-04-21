# Accessibility

This guide explains how to implement accessible Stock Charts that comply with WCAG 2.2 standards and support keyboard navigation, screen readers, and assistive technologies.

## Table of Contents

- [Overview](#overview)
- [WCAG 2.2 Compliance](#wcag-22-compliance)
- [WAI-ARIA Attributes](#wai-aria-attributes)
- [Keyboard Navigation](#keyboard-navigation)
- [Keyboard Shortcuts Reference](#keyboard-shortcuts-reference)
- [Screen Reader Support](#screen-reader-support)
- [High Contrast Themes](#high-contrast-themes)
- [Focus Management](#focus-management)
- [Best Practices](#best-practices)
- [Testing and Validation](#testing-and-validation)

## Overview

The Syncfusion Blazor Stock Chart component is built with accessibility in mind, providing comprehensive support for users with disabilities through keyboard navigation, screen reader compatibility, and WCAG 2.2 compliance.

**Key Accessibility Features:**
- Full keyboard navigation support
- WAI-ARIA attributes for screen readers
- High contrast themes for visual impairments
- Focus indicators for keyboard users
- Semantic HTML structure
- WCAG 2.2 Level AA compliance

## WCAG 2.2 Compliance

The Stock Chart component adheres to WCAG 2.2 Level AA standards:

### Perceivable

**Text Alternatives:** All chart elements have text equivalents accessible to assistive technologies.

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Apple Inc. Stock Price - Accessible Chart">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Candle" 
                          XName="Date" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double High { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7 }
    };
}
```

**Color Contrast:** High contrast themes ensure minimum contrast ratios of 4.5:1 for text and 3:1 for UI components.

**Adaptable:** Charts are responsive and adapt to different viewport sizes and orientations.

### Operable

**Keyboard Accessible:** All functionality is available via keyboard without requiring specific timings.

**Focus Visible:** Clear focus indicators show which element has keyboard focus.

### Understandable

**Readable:** Text content is understandable with proper labels and descriptions.

**Predictable:** Navigation and functionality operate in predictable ways.

### Robust

**Compatible:** Works with current and future assistive technologies including screen readers.

## WAI-ARIA Attributes

The Stock Chart automatically includes appropriate ARIA attributes:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Stock Chart with ARIA Support">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Name="AAPL Stock Price"
                          Type="ChartSeriesType.Line" 
                          XName="Date" 
                          YName="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Close = 183.7 }
    };
}
```

**ARIA Attributes Applied:**
- `role="img"` - Identifies chart as an image
- `aria-label` - Provides accessible name derived from title
- `aria-describedby` - Links to detailed description
- `aria-hidden="false"` - Ensures visibility to assistive tech
- `tabindex="0"` - Makes chart keyboard focusable

## Keyboard Navigation

The Stock Chart is fully navigable using keyboard shortcuts, allowing users to interact without a mouse.

### Enable Keyboard Navigation

Keyboard navigation is enabled by default.:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Keyboard Navigable Chart">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Candle" 
                          XName="Date" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double High { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7 },
        new StockInfo { Date = new DateTime(2024, 01, 15), Open = 183.7, High = 190.5, Low = 182.5, Close = 188.2 }
    };
}
```

### Navigation Instructions

Provide instructions for keyboard users:

```razor
@using Syncfusion.Blazor.Charts

<div class="keyboard-help">
    <h4>Keyboard Navigation</h4>
    <p>Press <kbd>Tab</kbd> to focus on the chart. Use arrow keys to navigate between data points.</p>
    <ul>
        <li><kbd>Tab</kbd> - Move focus to chart</li>
        <li><kbd>Arrow Keys</kbd> - Navigate data points</li>
        <li><kbd>Enter</kbd> - Select data point</li>
        <li><kbd>Ctrl + P</kbd> - Print chart</li>
    </ul>
</div>

<SfStockChart Title="AAPL Stock Price">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Line" 
                          XName="Date" 
                          YName="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

<style>
    .keyboard-help {
        background-color: #f0f0f0;
        padding: 15px;
        border-radius: 5px;
        margin-bottom: 20px;
    }
    .keyboard-help kbd {
        background-color: #333;
        color: white;
        padding: 2px 6px;
        border-radius: 3px;
        font-family: monospace;
    }
</style>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Close = 183.7 }
    };
}
```

## Keyboard Shortcuts Reference

Complete list of keyboard shortcuts for Stock Chart navigation:

| Shortcut | Action | Description |
|----------|--------|-------------|
| <kbd>Tab</kbd> | Focus Chart | Move keyboard focus to the chart |
| <kbd>Shift + Tab</kbd> | Focus Previous | Move focus to previous element |
| <kbd>→</kbd> (Right Arrow) | Next Point | Navigate to next data point |
| <kbd>←</kbd> (Left Arrow) | Previous Point | Navigate to previous data point |
| <kbd>↑</kbd> (Up Arrow) | Next Series | Move to next series (multi-series) |
| <kbd>↓</kbd> (Down Arrow) | Previous Series | Move to previous series (multi-series) |
| <kbd>Enter</kbd> / <kbd>Space</kbd> | Select | Select/activate focused data point |
| <kbd>Ctrl + P</kbd> | Print | Open print dialog |
| <kbd>Escape</kbd> | Close | Close tooltip or exit selection mode |
| <kbd>Home</kbd> | First Point | Jump to first data point |
| <kbd>End</kbd> | Last Point | Jump to last data point |
| <kbd>Page Up</kbd> | Previous Period | Navigate to previous time period |
| <kbd>Page Down</kbd> | Next Period | Navigate to next time period |
| <kbd>+</kbd> / <kbd>=</kbd> | Zoom In | Zoom into chart |
| <kbd>-</kbd> | Zoom Out | Zoom out of chart |
| <kbd>Ctrl + 0</kbd> | Reset Zoom | Reset zoom to default |

### Custom Keyboard Instructions Component

```razor
@using Syncfusion.Blazor.Charts

<div class="accessibility-panel">
    <button @onclick="ToggleInstructions" class="help-button">
        Keyboard Help
    </button>

    @if (ShowInstructions)
    {
        <div class="instructions-modal" role="dialog" aria-labelledby="instructions-title">
            <div class="instructions-content">
                <h3 id="instructions-title">Keyboard Navigation Guide</h3>
                <table class="shortcuts-table">
                    <thead>
                        <tr>
                            <th>Key</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr><td><kbd>Tab</kbd></td><td>Focus chart</td></tr>
                        <tr><td><kbd>Arrow Keys</kbd></td><td>Navigate data points</td></tr>
                        <tr><td><kbd>Enter</kbd></td><td>Select point</td></tr>
                        <tr><td><kbd>Ctrl + P</kbd></td><td>Print chart</td></tr>
                        <tr><td><kbd>Escape</kbd></td><td>Close dialogs</td></tr>
                    </tbody>
                </table>
                <button @onclick="ToggleInstructions" class="close-button">Close</button>
            </div>
        </div>
    }
</div>

<SfStockChart Title="Accessible Stock Chart">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Candle" 
                          XName="Date" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

<style>
    .instructions-modal {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.5);
        display: flex;
        align-items: center;
        justify-content: center;
        z-index: 1000;
    }
    .instructions-content {
        background: white;
        padding: 30px;
        border-radius: 8px;
        max-width: 600px;
        max-height: 80vh;
        overflow-y: auto;
    }
    .shortcuts-table {
        width: 100%;
        border-collapse: collapse;
        margin: 20px 0;
    }
    .shortcuts-table th,
    .shortcuts-table td {
        padding: 10px;
        text-align: left;
        border-bottom: 1px solid #ddd;
    }
    kbd {
        background-color: #333;
        color: white;
        padding: 3px 8px;
        border-radius: 3px;
        font-family: monospace;
        font-size: 0.9em;
    }
</style>

@code {
    private bool ShowInstructions = false;

    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double High { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5 }
    };

    private void ToggleInstructions()
    {
        ShowInstructions = !ShowInstructions;
    }
}
```

## Screen Reader Support

The Stock Chart provides comprehensive screen reader support:

### Screen Reader Announcements

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL Stock Analysis">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Name="Apple Inc. Stock"
                          Type="ChartSeriesType.Candle" 
                          XName="Date" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double High { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 02, 01), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7 },
        new StockInfo { Date = new DateTime(2024, 03, 01), Open = 183.7, High = 190.5, Low = 182.5, Close = 188.2 }
    };
}
```

Screen readers announce:
- Chart title
- Series names
- Data point values when focused
- Range changes and zoom levels
- Button labels and states

### Accessible Data Table Alternative

Provide a data table as an accessible alternative:

```razor
@using Syncfusion.Blazor.Charts

<div class="chart-section">
    <SfStockChart Title="Stock Price Chart">
        <StockChartSeriesCollection>
            <StockChartSeries DataSource="@StockData" 
                              Type="ChartSeriesType.Candle" 
                              XName="Date" 
                              High="High" 
                              Low="Low" 
                              Open="Open" 
                              Close="Close">
            </StockChartSeries>
        </StockChartSeriesCollection>
    </SfStockChart>
</div>

<details class="data-table-section">
    <summary>View Data as Table</summary>
    <table class="accessible-data-table">
        <caption>Stock Price Data</caption>
        <thead>
            <tr>
                <th scope="col">Date</th>
                <th scope="col">Open</th>
                <th scope="col">High</th>
                <th scope="col">Low</th>
                <th scope="col">Close</th>
            </tr>
        </thead>
        <tbody>
            @foreach (var item in StockData)
            {
                <tr>
                    <td>@item.Date.ToString("MMM dd, yyyy")</td>
                    <td>$@item.Open.ToString("F2")</td>
                    <td>$@item.High.ToString("F2")</td>
                    <td>$@item.Low.ToString("F2")</td>
                    <td>$@item.Close.ToString("F2")</td>
                </tr>
            }
        </tbody>
    </table>
</details>

<style>
    .accessible-data-table {
        width: 100%;
        border-collapse: collapse;
        margin-top: 15px;
    }
    .accessible-data-table th,
    .accessible-data-table td {
        padding: 10px;
        text-align: left;
        border: 1px solid #ddd;
    }
    .accessible-data-table th {
        background-color: #f0f0f0;
        font-weight: bold;
    }
</style>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double High { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7 }
    };
}
```

## High Contrast Themes

Enable high contrast themes for users with visual impairments:

```html
<!-- In index.html, _Host.cshtml, or App.razor -->
<head>
    <link href="_content/Syncfusion.Blazor.Themes/highcontrast.css" rel="stylesheet" />
</head>
```

### Implement Theme Switcher

```razor
@using Syncfusion.Blazor.Charts

<div class="theme-selector">
    <label for="theme-select">Select Theme:</label>
    <select id="theme-select" @onchange="ChangeTheme">
        <option value="bootstrap5">Bootstrap 5</option>
        <option value="material">Material</option>
        <option value="highcontrast">High Contrast</option>
    </select>
</div>

<SfStockChart Title="Theme-Aware Stock Chart">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Line" 
                          XName="Date" 
                          YName="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Close = 183.7 }
    };

    private void ChangeTheme(ChangeEventArgs e)
    {
        string theme = e.Value.ToString();
        // Implement theme switching logic
        Console.WriteLine($"Switching to theme: {theme}");
    }
}
```

## Focus Management

Manage keyboard focus for optimal user experience:

```razor
@using Syncfusion.Blazor.Charts
@using Microsoft.JSInterop

<button @onclick="FocusChart">Focus on Chart</button>

<SfStockChart @ref="StockChartRef" 
              Title="Focus Management Demo">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Candle" 
                          XName="Date" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    private SfStockChart StockChartRef;

    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double High { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5 }
    };

    private async Task FocusChart()
    {
        // Focus management implementation
        await Task.CompletedTask;
    }
}
```

## Best Practices

### 1. Always Provide Meaningful Titles

```razor
<SfStockChart Title="Apple Inc. Stock Price Performance - January 2024">
```

### 2. Use Descriptive Series Names

```razor
<StockChartSeries Name="Apple Inc. (AAPL) Closing Price" ...>
```

### 3. Provide Alternative Text Content

Include data table or textual summary for screen reader users.

### 4. Ensure Sufficient Color Contrast

Use themes with minimum contrast ratios of 4.5:1.

### 5. Test with Multiple Screen Readers

- JAWS (Windows)
- NVDA (Windows)
- VoiceOver (macOS/iOS)
- TalkBack (Android)

### 6. Support Keyboard-Only Navigation

Ensure all interactive elements are reachable via keyboard.

### 7. Provide Skip Links

```razor
<a href="#main-content" class="skip-link">Skip to main content</a>
<div id="main-content">
    <SfStockChart ...>
    </SfStockChart>
</div>
```

## Testing and Validation

### Automated Testing Tools

- **axe DevTools** - Browser extension for accessibility testing
- **WAVE** - Web accessibility evaluation tool
- **Lighthouse** - Chrome DevTools accessibility audit

### Manual Testing Checklist

- [ ] Navigate entire chart using only keyboard
- [ ] Test with screen reader (NVDA, JAWS, VoiceOver)
- [ ] Verify high contrast theme readability
- [ ] Check color contrast ratios
- [ ] Test focus indicators visibility
- [ ] Validate ARIA attributes
- [ ] Test with browser zoom at 200%
- [ ] Verify responsive behavior
- [ ] Test with Windows High Contrast mode
- [ ] Validate with keyboard shortcuts

### Example Test Scenario

```razor
@page "/accessibility-test"
@using Syncfusion.Blazor.Charts

<h1>Accessibility Test Page</h1>

<div class="test-instructions">
    <h2>Test Instructions:</h2>
    <ol>
        <li>Press Tab to focus on the chart</li>
        <li>Use Arrow keys to navigate data points</li>
        <li>Press Enter to select a point</li>
        <li>Verify screen reader announces values</li>
        <li>Test with high contrast theme</li>
    </ol>
</div>

<SfStockChart Title="Accessibility Test Chart">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Name="Test Stock Data"
                          Type="ChartSeriesType.Candle" 
                          XName="Date" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double High { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7 }
    };
}
```
