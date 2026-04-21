# Export and Print

This guide explains how to export Stock Charts to various formats (PNG, JPEG, SVG, PDF) and enable print functionality for reports and documentation.

## Table of Contents
- [Overview](#overview)
- [Export Formats](#export-formats)
- [Programmatic Export](#programmatic-export)
- [Export Configuration](#export-configuration)
- [Print Functionality](#print-functionality)
- [Disable Export and Print](#disable-export-and-print)
- [Complete Export Example](#complete-export-example)
- [Common Issues](#common-issues)

## Overview

The Stock Chart component provides built-in export and print capabilities through the `SfStockChart` component, utilizing the `ExportType` property and `ExportAsync()` and `PrintAsync()` methods to allow users to save charts as images or documents, or print them directly.

**Key Features:**
- Export to PNG, JPEG, SVG, and PDF formats
- Export via UI button or programmatically
- Customizable export settings (filename, resolution)
- One-click print functionality
- Option to disable export/print buttons

## Export Formats

The `ExportType` property enables configuration of available export format options in the Stock Chart toolbar. Export is enabled by default and supports PNG, JPEG, SVG, and PDF formats.

### Enable Export Toolbar Button

Export is enabled by default. Use the `ExportType` property to specify the list of available export formats.

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL Stock Price" EnableSelector="false" ExportType="new List<ExportType>() { ExportType.JPEG, ExportType.PDF, ExportType.PNG, ExportType.XLSX }">
    <StockChartEvents ExportCompleted="ExportCompleteHandler"></StockChartEvents>
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

    private void ExportCompleteHandler(ExportEventArgs args)
    {
        Console.WriteLine($"Chart exported successfully");
    }
}
```

The export menu in the toolbar provides options for PNG, JPEG, SVG, PDF, and Print.

## Programmatic Export

Export charts programmatically using the `ExportAsync()` method of the `SfStockChart` component. This method accepts the `ExportType` parameter and an optional filename parameter:

### Export to PNG

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart @ref="StockChartRef" Title="Stock Chart - Export to PNG">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Line" 
                          XName="Date" 
                          YName="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

<button @onclick="ExportToPNG">Export to PNG</button>

@code {
    private SfStockChart StockChartRef;

    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Close = 183.7 },
        new StockInfo { Date = new DateTime(2024, 01, 15), Close = 188.2 }
    };

    private async Task ExportToPNG()
    {
        await StockChartRef.ExportAsync(ExportType.PNG, "StockChart");
    }
}
```

### Export to JPEG

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart @ref="StockChartRef" Title="Stock Chart - Export to JPEG">
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

<button @onclick="ExportToJPEG">Export to JPEG</button>

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

    private async Task ExportToJPEG()
    {
        await StockChartRef.ExportAsync(ExportType.JPEG, "StockChart");
    }
}
```

### Export to SVG

```razor
<button @onclick="ExportToSVG">Export to SVG</button>

@code {
    private SfStockChart StockChartRef;

    private async Task ExportToSVG()
    {
        await StockChartRef.ExportAsync(ExportType.SVG, "StockChart");
    }
}
```

### Export to PDF

```razor
@using Syncfusion.Blazor.Charts

<button @onclick="ExportToPDF">Export to PDF</button>

@code {
    private SfStockChart StockChartRef;

    private async Task ExportToPDF()
    {
        await StockChartRef.ExportAsync(ExportType.PDF, "StockChart");
    }
}
```

## Export Configuration

Customize export settings using the `ExportAsync()` method with additional parameters such as custom filenames and dynamic naming conventions:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart @ref="StockChartRef" Title="Custom Export Settings">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Line" 
                          XName="Date" 
                          YName="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

<div>
    <button @onclick="ExportWithCustomSettings">Export with Custom Settings</button>
</div>

@code {
    private SfStockChart StockChartRef;

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

    private async Task ExportWithCustomSettings()
    {
        // Export to PNG with custom filename including timestamp
        string filename = $"StockChart_{DateTime.Now:yyyyMMdd_HHmmss}";
        await StockChartRef.ExportAsync(ExportType.PNG, filename);
    }
}
```

## Print Functionality

The `PrintAsync()` method enables direct printing of Stock Charts. Print functionality is available both through the toolbar menu and programmatically.

### Enable Print via Toolbar

The print option is available in the export toolbar menu by default. Use the `EnableSelector` property to control toolbar visibility.

### Programmatic Print

Trigger print programmatically using the `PrintAsync()` method of the `SfStockChart` component:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart @ref="StockChartRef" Title="Printable Stock Chart">
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

<button @onclick="PrintChart">Print Chart</button>

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
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7 }
    };

    private async Task PrintChart()
    {
        await StockChartRef.PrintAsync();
    }
}
```

## Disable Export and Print

Set the `ExportType` property to an empty list to disable export options, or use the `EnableSelector` property to hide the entire toolbar including export and print buttons.

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Chart without Export" EnableSelector="false" ExportType="new List<ExportType>() { }">
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
        new StockInfo { Date = new DateTime(2024, 01, 08), Close = 183.7 },
        new StockInfo { Date = new DateTime(2024, 01, 15), Close = 188.2 }
    };
}
```

Set `EnableSelector="false"` to hide the entire toolbar including export/print options.

## Complete Export Example

Comprehensive example demonstrating all export options using the `ExportAsync()` and `PrintAsync()` methods, along with the `StockChartEvents` and `ExportCompleted` event handler:

```razor
@page "/stock-export"
@using Syncfusion.Blazor.Charts

<h3>Stock Chart Export Demo</h3>

<SfStockChart @ref="StockChartRef" 
              Title="AAPL Stock Price - 2024"
              EnableSelector="true">
    <StockChartEvents ExportCompleted="OnExportComplete"></StockChartEvents>
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

<div style="margin-top: 20px;">
    <h4>Export Options:</h4>
    <button class="btn btn-primary" @onclick="() => ExportChart(ExportType.PNG)">PNG</button>
    <button class="btn btn-primary" @onclick="() => ExportChart(ExportType.JPEG)">JPEG</button>
    <button class="btn btn-primary" @onclick="() => ExportChart(ExportType.SVG)">SVG</button>
    <button class="btn btn-primary" @onclick="() => ExportChart(ExportType.PDF)">PDF</button>
    <button class="btn btn-secondary" @onclick="PrintChart">Print</button>
</div>

<div style="margin-top: 10px;">
    <p>@ExportMessage</p>
</div>

@code {
    private SfStockChart StockChartRef;
    private string ExportMessage = "";

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
        new StockInfo { Date = new DateTime(2024, 01, 15), Open = 183.7, High = 190.5, Low = 182.5, Close = 188.2 },
        new StockInfo { Date = new DateTime(2024, 01, 22), Open = 188.2, High = 195.5, Low = 187.5, Close = 193.2 }
    };

    private async Task ExportChart(ExportType type)
    {
        string extension = type.ToString().ToLower();
        string filename = $"AAPL_Stock_{DateTime.Now:yyyyMMdd}.{extension}";
        
        ExportMessage = $"Exporting chart as {type}...";
        await StockChartRef.ExportAsync(type, filename);
    }

    private async Task PrintChart()
    {
        ExportMessage = "Opening print dialog...";
        await StockChartRef.PrintAsync();
    }

    private void OnExportComplete(ExportEventArgs args)
    {
        ExportMessage = $"Chart exported successfully";
        StateHasChanged();
    }
}
```

## Common Issues

### Export Doesn't Download File

**Problem:** `ExportAsync()` method doesn't download the file.

**Solution:** Ensure your browser allows downloads and check browser console for errors. The export feature works in browsers that support the HTML5 download attribute.

### Exported Image is Blank

**Problem:** Exported image/PDF is empty or blank.

**Solution:** Wait for the chart to fully render before exporting:
```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await Task.Delay(1000); // Wait for chart to render
        await StockChartRef.ExportAsync(ExportType.PNG, "chart.png");
    }
}
```

### Print Shows Blank Page

**Problem:** Print preview displays a blank page when using `PrintAsync()`.

**Solution:** Ensure the chart is fully loaded and visible before calling `PrintAsync()`.

### Export Button Not Visible

**Problem:** Export/Print buttons don't appear in toolbar.

**Solution:** Ensure the `EnableSelector` property is not set to `false`:
```razor
<SfStockChart EnableSelector="true">
```

### PDF Export Quality Issues

**Problem:** PDF export has poor quality or resolution.

**Solution:** Export to SVG format for scalable vector graphics that maintain quality at any size, or use PNG with appropriate dimensions.

