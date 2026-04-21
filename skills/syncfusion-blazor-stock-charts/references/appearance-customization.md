# Appearance Customization

This guide covers appearance customization options for Stock Charts and references relevant properties/components such as `Title`/`StockChartTitleStyle`, `Width`/`Height`, `Background`/`StockChartChartBorder`, `StockChartSeriesBorder`, `Fill`/`BullFillColor`/`BearFillColor`, SVG gradient `Fill` via `url(#id)`, and Syncfusion theme stylesheet links.

## Table of Contents

- [Overview](#overview)
- [Chart Title](#chart-title)
- [Title Customization](#title-customization)
- [Themes](#themes)
- [Available Themes](#available-themes)
- [Chart Dimensions](#chart-dimensions)
- [Responsive Charts](#responsive-charts)
- [Gradient Fills](#gradient-fills)
- [Last Data Label](#last-data-label)
- [Borders and Backgrounds](#borders-and-backgrounds)
- [Custom CSS Styling](#custom-css-styling)
- [Color Customization](#color-customization)
- [Troubleshooting](#troubleshooting)

## Overview

The Stock Chart component provides extensive appearance customization. Key configurable properties and components include `Title`, `StockChartTitleStyle`, `Width`, `Height`, `Background`, `StockChartChartBorder`, `StockChartSeriesBorder`, `Fill`, `BullFillColor`, `BearFillColor`, and theme styles applied via the Syncfusion theme stylesheet.

**Key Customization Areas:**
- Title and subtitle styling
- 15+ built-in themes with dark mode variants
- Flexible dimensions and responsive layouts
- Gradient fills for series
- Border and background customization
- CSS-based styling

## Chart Title

Add a title to provide context for your chart using the `Title` property and `StockChartTitleStyle` for styling:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Apple Inc. Stock Price - 2024">
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

## Title Customization

Customize title appearance using `StockChartTitleStyle` and its properties (`FontFamily`, `Size`, `FontWeight`, `Color`, `TextAlignment`, `FontStyle`, `TextOverflow`):

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="NASDAQ Technology Stocks">
    <StockChartTitleStyle FontFamily="Arial" 
                          Size="24px" 
                          FontWeight="Bold" 
                          Color="#0066cc" 
                          TextAlignment="Alignment.Center">
    </StockChartTitleStyle>
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
}
```

**Title Style Properties:**
- `FontFamily` - Font family name (e.g., "Arial", "Segoe UI")
- `Size` - Font size (e.g., "16px", "1.2em")
- `FontWeight` - Font weight ("Normal", "Bold", "600")
- `Color` - Title color (hex, rgb, or color name)
- `TextAlignment` - Alignment (`Alignment.Near`, `Center`, `Far`)
- `FontStyle` - Style ("Normal", "Italic", "Oblique")
- `TextOverflow` - Overflow behavior ("Wrap", "Trim", "None")

## Themes

Apply a theme by referencing the Syncfusion theme stylesheet (themes are applied via CSS links rather than a chart property):

```html
<!-- In index.html, _Host.cshtml, or App.razor -->
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
```

Themes provide consistent styling across all chart elements including colors, fonts, and spacing.

## Available Themes

### Light Themes

**Bootstrap 5 (Default)**
```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```
Clean, modern design following Bootstrap 5 guidelines.

**Material Design**
```html
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />
```
Google's Material Design with vibrant colors and shadows.

**Material 3**
```html
<link href="_content/Syncfusion.Blazor.Themes/material3.css" rel="stylesheet" />
```
Latest Material Design iteration with enhanced typography.

**Fluent**
```html
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />
```
Microsoft Fluent Design System for modern Windows apps.

**Tailwind CSS**
```html
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />
```
Utility-first design matching Tailwind CSS framework.

**Bootstrap 4**
```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap4.css" rel="stylesheet" />
```
Classic Bootstrap 4 styling.

**Office Fabric**
```html
<link href="_content/Syncfusion.Blazor.Themes/fabric.css" rel="stylesheet" />
```
Microsoft Office design language.

**High Contrast**
```html
<link href="_content/Syncfusion.Blazor.Themes/highcontrast.css" rel="stylesheet" />
```
Maximum contrast for accessibility compliance.

### Dark Themes

Add `-dark` suffix to any theme name:

**Bootstrap 5 Dark**
```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5-dark.css" rel="stylesheet" />
```

**Material Dark**
```html
<link href="_content/Syncfusion.Blazor.Themes/material-dark.css" rel="stylesheet" />
```

**Material 3 Dark**
```html
<link href="_content/Syncfusion.Blazor.Themes/material3-dark.css" rel="stylesheet" />
```

**Fluent Dark**
```html
<link href="_content/Syncfusion.Blazor.Themes/fluent-dark.css" rel="stylesheet" />
```

**Tailwind Dark**
```html
<link href="_content/Syncfusion.Blazor.Themes/tailwind-dark.css" rel="stylesheet" />
```

**Fabric Dark**
```html
<link href="_content/Syncfusion.Blazor.Themes/fabric-dark.css" rel="stylesheet" />
```

### Theme Example

```razor
@page "/stock-chart-dark"
@using Syncfusion.Blazor.Charts

<!-- Material Dark theme applied -->
<SfStockChart Title="Stock Price - Dark Theme">
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

## Chart Dimensions

### Fixed Dimensions

Set explicit `Width` and `Height` on the `SfStockChart` component:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Fixed Size Chart" Width="800px" Height="450px">
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

### Percentage-Based Dimensions

Use `Width='100%'` and `Height='100%'` on `SfStockChart` and container CSS for responsive layouts:

```razor
@using Syncfusion.Blazor.Charts

<div style="width: 100%; height: 600px;">
    <SfStockChart Title="Responsive Chart" Width="100%" Height="100%">
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
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5 }
    };
}
```

## Responsive Charts

Create responsive charts that adapt to container size:

```razor
@using Syncfusion.Blazor.Charts

<div class="chart-container">
    <SfStockChart Title="Responsive Stock Chart" Width="100%" Height="100%">
        <StockChartSeriesCollection>
            <StockChartSeries DataSource="@StockData" 
                              Type="ChartSeriesType.Line" 
                              XName="Date" 
                              YName="Close">
            </StockChartSeries>
        </StockChartSeriesCollection>
    </SfStockChart>
</div>

<style>
    .chart-container {
        width: 100%;
        height: 500px;
        padding: 20px;
    }

    @@media (max-width: 768px) {
        .chart-container {
            height: 350px;
            padding: 10px;
        }
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
        new StockInfo { Date = new DateTime(2024, 02, 01), Close = 183.7 }
    };
}
```

## Gradient Fills

Apply gradient fills to series via the `Fill` property using an SVG gradient reference (e.g., `Fill="url(#gradient1)"`):

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Stock Chart with Gradient">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Area" 
                          XName="Date" 
                          YName="Close"
                          Fill="url(#gradient1)">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

<svg style="height: 0">
    <defs>
        <linearGradient id="gradient1" x1="0" x2="0" y1="0" y2="1">
            <stop offset="0%" style="stop-color:#4CAF50;stop-opacity:0.8" />
            <stop offset="100%" style="stop-color:#4CAF50;stop-opacity:0.1" />
        </linearGradient>
    </defs>
</svg>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 175.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 01, 15), Close = 183.7 },
        new StockInfo { Date = new DateTime(2024, 01, 22), Close = 188.2 }
    };
}
```

### Multiple Gradient Series

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Multi-Series Gradient Chart">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@AAPLData" 
                          Name="AAPL"
                          Type="ChartSeriesType.Area" 
                          XName="Date" 
                          YName="Close"
                          Fill="url(#gradient-aapl)">
        </StockChartSeries>
        <StockChartSeries DataSource="@MSFTData" 
                          Name="MSFT"
                          Type="ChartSeriesType.Area" 
                          XName="Date" 
                          YName="Close"
                          Fill="url(#gradient-msft)">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

<svg style="height: 0">
    <defs>
        <linearGradient id="gradient-aapl" x1="0" x2="0" y1="0" y2="1">
            <stop offset="0%" style="stop-color:#2196F3;stop-opacity:0.7" />
            <stop offset="100%" style="stop-color:#2196F3;stop-opacity:0" />
        </linearGradient>
        <linearGradient id="gradient-msft" x1="0" x2="0" y1="0" y2="1">
            <stop offset="0%" style="stop-color:#FF5722;stop-opacity:0.7" />
            <stop offset="100%" style="stop-color:#FF5722;stop-opacity:0" />
        </linearGradient>
    </defs>
</svg>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> AAPLData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 02, 01), Close = 183.7 }
    };

    public List<StockInfo> MSFTData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 370.2 },
        new StockInfo { Date = new DateTime(2024, 02, 01), Close = 375.8 }
    };
}
```

## Last Data Label

Use `StockChartLastDataLabel` and its properties (`ShowLabel`, `Background`, `LineColor`, `LineWidth`, `DashArray`, `Rx`, `Ry`) along with `StockChartLastDataLabelBorder` and `StockChartLastDataLabelFont` to display the most recent data point value:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL Stock Price">
    <StockChartPrimaryYAxis Interval="10"></StockChartPrimaryYAxis>
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockDetails" Type="ChartSeriesType.Hilo" Width="3" XName="Date" High="High" Low="Low" Open="Open" Close="Close" Volume="Volume">
            <StockChartLastDataLabel ShowLabel="true" Background="#748574" LineColor="blue" LineWidth="2" DashArray="5" Rx="10" Ry="10">
                <StockChartLastDataLabelBorder Color="red" Width="2"></StockChartLastDataLabelBorder>
                <StockChartLastDataLabelFont Color="#F0E68C" FontFamily="Arial" FontStyle="Italic" FontWeight="bold" Size="12px"></StockChartLastDataLabelFont>
            </StockChartLastDataLabel>
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class ChartData
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
        public double High { get; set; }
        public double Volume { get; set; }
    }

    public List<ChartData> StockDetails = new List<ChartData>
    {
        new ChartData { Date = new DateTime(2012, 04, 02), Open = 85.9757, High = 90.6657, Low = 85.7685, Close = 90.5257, Volume = 660187068 },
        new ChartData { Date = new DateTime(2012, 04, 09), Open = 89.4471, High = 92, Low = 86.2157, Close = 86.4614, Volume = 912634864 },
        new ChartData { Date = new DateTime(2012, 04, 16), Open = 87.1514, High = 88.6071, Low = 81.4885, Close = 81.8543, Volume = 1221746066 },
        new ChartData { Date = new DateTime(2012, 04, 23), Open = 81.5157, High = 88.2857, Low = 79.2857, Close = 86.1428, Volume = 965935749 },
        new ChartData { Date = new DateTime(2012, 04, 30), Open = 85.4, High =  85.4857, Low = 80.7385, Close = 80.75, Volume = 615249365 },
        new ChartData { Date = new DateTime(2012, 05, 07), Open = 80.2143, High = 82.2685, Low = 79.8185, Close = 80.9585, Volume = 541742692 },
        new ChartData { Date = new DateTime(2012, 05, 14), Open = 80.3671, High = 81.0728, Low = 74.5971, Close = 75.7685, Volume = 708126233 },
        new ChartData { Date = new DateTime(2012, 05, 21), Open = 76.3571, High = 82.3571, Low = 76.2928, Close = 80.3271, Volume = 682076215 },
        new ChartData { Date = new DateTime(2012, 05, 28), Open = 81.5571, High = 88.0714, Low = 83.0743, Close = 80.1414, Volume = 480059584 }
    };
}
```

## Borders and Backgrounds

Use `Background`, `StockChartChartBorder`, `StockChartMargin`, and `StockChartSeriesBorder` to customize chart borders, backgrounds, and spacing:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Styled Stock Chart" 
              Background="#f8f9fa">
    <StockChartChartBorder Width="2" Color="#0066cc" />
    <StockChartMargin Left="40" Right="40" Top="40" Bottom="40"></StockChartMargin>
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Candle" 
                          XName="Date" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close">
            <StockChartSeriesBorder Width="1"></StockChartSeriesBorder>
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

## Custom CSS Styling

Apply custom CSS by wrapping the chart in a container, using `CssClass` (when available), or targeting Syncfusion classes; chart properties such as `Width`, `Height`, and `Fill` remain applicable.

```razor
@using Syncfusion.Blazor.Charts

<div class="custom-stock-chart">
    <SfStockChart Title="Custom Styled Chart" Width="100%" Height="600px">
        <StockChartSeriesCollection>
            <StockChartSeries DataSource="@StockData" 
                              Type="ChartSeriesType.Line" 
                              XName="Date" 
                              YName="Close"
                              Width="3"
                              Fill="#0066cc">
            </StockChartSeries>
        </StockChartSeriesCollection>
    </SfStockChart>
</div>

<style>
    .custom-stock-chart {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        padding: 20px;
        border-radius: 12px;
        box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
    }

    .custom-stock-chart .e-stockchart {
        border-radius: 8px;
        overflow: hidden;
    }

    .custom-stock-chart .e-chart-title {
        color: white !important;
        font-size: 24px !important;
        text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
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
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 175.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 01, 15), Close = 183.7 }
    };
}
```

## Color Customization

Customize series colors using `BearFillColor` and `BullFillColor` for Candle series or `Fill` for other series types:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Custom Color Scheme">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Candle" 
                          XName="Date" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close"
                          BearFillColor="#FF6B6B"
                          BullFillColor="#4ECDC4">
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

## Troubleshooting

### Theme Not Applied

**Problem:** Chart doesn't reflect the selected theme.

**Solution:** Ensure the theme stylesheet is properly referenced and loaded before the Syncfusion script:
```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

### Chart Not Responsive

**Problem:** Chart doesn't resize with window or container.

**Solution:**
```razor
<!-- Use percentage-based dimensions -->
<div style="width: 100%; height: 500px;">
    <SfStockChart Width="100%" Height="100%">
        <!-- ... -->
    </SfStockChart>
</div>
```

### Gradient Not Visible

**Problem:** Gradient fill doesn't appear on series.

**Solution:** Ensure the SVG gradient definition is in the same component and the ID matches:
```razor
<StockChartSeries Fill="url(#gradient1)">
<!-- ... -->
<svg style="height: 0">
    <defs>
        <linearGradient id="gradient1" x1="0" x2="0" y1="0" y2="1">
            <!-- stops -->
        </linearGradient>
    </defs>
</svg>
```

### Custom Colors Not Working for Candles

**Problem:** BearFillColor and BullFillColor don't apply.

**Solution:** Ensure you're using Candle series type (not HollowCandle):
```razor
<StockChartSeries Type="ChartSeriesType.Candle" 
                  BearFillColor="#FF0000"
                  BullFillColor="#00FF00">
```

### Title Overlaps Chart

**Problem:** Title overlaps with chart content.

**Solution:** Adjust margins:
```razor
<SfStockChart Title="My Chart">
    <StockChartMargin Top="60"></StockChartMargin>
    <!-- ... -->
</SfStockChart>
```

### CSS Styles Not Applying

**Problem:** Custom CSS classes don't affect chart elements.

**Solution:** Use `!important` or more specific selectors:
```css
.custom-stock-chart .e-chart-title {
    color: red !important;
}
```

