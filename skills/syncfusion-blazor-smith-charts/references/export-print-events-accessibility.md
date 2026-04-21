# Export, Print, Events, and Accessibility

## Table of Contents
- [Overview](#overview)
- [Export Functionality](#export-functionality)
- [Print Functionality](#print-functionality)
- [Events](#events)
- [Accessibility](#accessibility)
- [Testing and Validation](#testing-and-validation)

## Overview

This guide covers advanced Smith Chart features including:
- **Export** - Save charts as PNG, JPEG, SVG, or PDF
- **Print** - Print charts directly from the browser
- **Events** - Handle chart lifecycle and user interaction events
- **Accessibility** - Ensure WCAG 2.1 AA compliance for all users

## Export Functionality

The Export Functionality provides methods to save Smith Charts in multiple formats using the `ExportAsync` method with the `ExportType` enumeration property, which accepts values: PNG, JPEG, SVG, and PDF. The filename parameter specifies the exported file name.

### Export as PNG

Export Smith Chart as PNG image using `ExportType.PNG` property for standard raster format export:

```razor
@page "/export-png"
@using Syncfusion.Blazor.Charts

<h3>Export Smith Chart as PNG</h3>

<button @onclick="ExportPNG">Export as PNG</button>

<SfSmithChart @ref="smithChart" Height="600px" Width="100%">
    <SmithChartTitle Text="Impedance Analysis"></SmithChartTitle>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@TransmissionData" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    private SfSmithChart smithChart;
    
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };
    
    private async Task ExportPNG()
    {
        await smithChart.ExportAsync(Syncfusion.Blazor.Charts.ExportType.PNG, "SmithChart");
    }
}
```

### Export as JPEG

Export Smith Chart with `ExportType.JPEG` property for compressed raster format export with reduced file sizes:

```razor
@using Syncfusion.Blazor.Charts

<button @onclick="ExportJPEG">Export as JPEG</button>

<SfSmithChart @ref="smithChart">
    <!-- Chart configuration -->
</SfSmithChart>

@code {
    private SfSmithChart smithChart;
    
    private async Task ExportJPEG()
    {
        await smithChart.ExportAsync(Syncfusion.Blazor.Charts.ExportType.JPEG, "SmithChart");
    }
}
```

### Export as SVG

Export Smith Chart with `ExportType.SVG` property as scalable vector graphics for infinitely scalable, editable format suitable for high-quality prints:

```razor
@using Syncfusion.Blazor.Charts

<button @onclick="ExportSVG">Export as SVG</button>

<SfSmithChart @ref="smithChart">
    <!-- Chart configuration -->
</SfSmithChart>

@code {
    private SfSmithChart smithChart;
    
    private async Task ExportSVG()
    {
        await smithChart.ExportAsync(Syncfusion.Blazor.Charts.ExportType.SVG, "SmithChart");
    }
}
```

**SVG Benefits:**
- Infinite scaling without quality loss
- Smaller file sizes for simple charts
- Editable in vector graphics software
- Ideal for technical publications

### Export as PDF

Export Smith Chart with `ExportType.PDF` property directly to portable document format for seamless document integration:

```razor
@using Syncfusion.Blazor.Charts

<button @onclick="ExportPDF">Export as PDF</button>

<SfSmithChart @ref="smithChart">
    <!-- Chart configuration -->
</SfSmithChart>

@code {
    private SfSmithChart smithChart;
    
    private async Task ExportPDF()
    {
        await smithChart.ExportAsync(Syncfusion.Blazor.Charts.ExportType.PDF, "SmithChart");
    }
}
```

### Export with Custom Options

Custom Export Options allows developers to dynamically select export format using the `ExportType` property and customize the filename parameter before triggering the `ExportAsync` method.

```razor
@page "/export-custom"
@using Syncfusion.Blazor.Charts

<h3>Custom Export Options</h3>

<div>
    <label>Format:</label>
    <select @onchange="OnFormatChange">
        <option value="PNG">PNG</option>
        <option value="JPEG">JPEG</option>
        <option value="SVG">SVG</option>
        <option value="PDF">PDF</option>
    </select>
    
    <label>Filename:</label>
    <input type="text" @bind="fileName" />
    
    <button @onclick="ExportChart">Export</button>
</div>

<SfSmithChart @ref="smithChart" Height="600px" Width="100%">
    <SmithChartTitle Text="RF Impedance Measurement"></SmithChartTitle>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@TransmissionData" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    private SfSmithChart smithChart;
    private Syncfusion.Blazor.Charts.ExportType exportFormat = Syncfusion.Blazor.Charts.ExportType.PNG;
    private string fileName = "SmithChart_Export";
    
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };
    
    private void OnFormatChange(Microsoft.AspNetCore.Components.ChangeEventArgs e)
    {
        exportFormat = Enum.Parse<Syncfusion.Blazor.Charts.ExportType>(e.Value.ToString());
    }
    
    private async Task ExportChart()
    {
        await smithChart.ExportAsync(exportFormat, fileName);
    }
}
```

## Print Functionality

Print Functionality enables direct printing of Smith Charts through the browser's print dialog using the `PrintAsync` method, which respects chart dimensions and styling while excluding page UI elements from the print output.

### Basic Print

Print Smith Chart using the `PrintAsync` method to invoke the browser's print dialog for immediate output:

```razor
@page "/print-chart"
@using Syncfusion.Blazor.Charts

<h3>Print Smith Chart</h3>

<button @onclick="PrintChart">Print Chart</button>

<SfSmithChart @ref="smithChart" Height="600px" Width="100%">
    <SmithChartTitle Text="Antenna Impedance Analysis"></SmithChartTitle>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@TransmissionData" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    private SfSmithChart smithChart;
    
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };
    
    private async Task PrintChart()
    {
        await smithChart.PrintAsync();
    }
}
```

**Print Features:**
- Opens browser print dialog
- Prints only the chart (excludes page UI elements)
- Respects chart dimensions and styling
- Compatible with PDF printers for document generation

## Events

Events in Smith Charts are configured through the `SmithChartEvents` component, which provides event handler properties such as `Loaded`, `SeriesRender`, `AxisLabelRendering`, and `LegendRendering` to respond to chart lifecycle and user interaction events.

### Loaded Event

The `Loaded` event fires when Smith Chart completes rendering, triggered via the `SmithChartEvents.Loaded` property with `SmithChartLoadedEventArgs` parameter:

```razor
@using Syncfusion.Blazor.Charts

<SfSmithChart>
    <SmithChartEvents Loaded="OnChartLoaded"></SmithChartEvents>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@TransmissionData" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };

    private void OnChartLoaded(SmithChartLoadedEventArgs args)
    {
        // Chart has finished loading
        Console.WriteLine("Smith Chart loaded successfully");
        // Perform post-load operations
    }
}
```

**Use Cases:**
- Initialize chart-dependent features
- Log chart load times for performance monitoring
- Trigger data refresh or animations
- Enable export/print buttons after load

### SeriesRender Event

The `SeriesRender` event fires after each series completes rendering, triggered via the `SmithChartEvents.SeriesRender` property with `SmithChartSeriesRenderEventArgs` parameter containing series information:

```razor
@using Syncfusion.Blazor.Charts

<SfSmithChart>
    <SmithChartEvents SeriesRender="OnSeriesRender"></SmithChartEvents>
    <SmithChartSeriesCollection>
        <SmithChartSeries Name="Series 1" DataSource="@Data1" 
                          Reactance="Reactance" Resistance="Resistance">
        </SmithChartSeries>
        <SmithChartSeries Name="Series 2" DataSource="@Data2" 
                          Reactance="Reactance" Resistance="Resistance">
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }

    public List<SmithChartData> Data1 = new()
    {
        new SmithChartData { Resistance = 10, Reactance = 20 },
        new SmithChartData { Resistance = 7,  Reactance = 10 },
        new SmithChartData { Resistance = 5,  Reactance = 5 },
        new SmithChartData { Resistance = 3,  Reactance = 2 },
        new SmithChartData { Resistance = 1,  Reactance = 0.5 }
    };

    public List<SmithChartData> Data2 = new()
    {
        new SmithChartData { Resistance = 8,  Reactance = 15 },
        new SmithChartData { Resistance = 6,  Reactance = 8 },
        new SmithChartData { Resistance = 4,  Reactance = 3 },
        new SmithChartData { Resistance = 2,  Reactance = 1 },
        new SmithChartData { Resistance = 1,  Reactance = 0 }
    };

    private void OnSeriesRender(SmithChartSeriesRenderEventArgs args)
    {
        Console.WriteLine($"Series rendered: {args.Text}");
    }
}
```

**Use Cases:**
- Track rendering progress for multiple series
- Apply series-specific customizations
- Validate series data after rendering

### AxisLabelRendering Event

The `AxisLabelRendering` event fires when an axis label is being rendered, triggered via the `SmithChartEvents.AxisLabelRendering` property with `SmithChartAxisLabelRenderEventArgs` parameter for label customization:

```razor
@using Syncfusion.Blazor.Charts

<SfSmithChart>
    <SmithChartEvents AxisLabelRendering="OnAxisLabelRendering"></SmithChartEvents>
    <SmithChartSeriesCollection>
        <SmithChartSeries DataSource="@TransmissionData" 
                          Reactance="Reactance" Resistance="Resistance">
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };

    private void OnAxisLabelRendering(SmithChartAxisLabelRenderEventArgs args)
    {
        // Axis label rendering in progress
        Console.WriteLine("Axis label rendering");
    }
}
```

### LegendRendering Event

The `LegendRendering` event fires before the legend renders, triggered via the `SmithChartEvents.LegendRendering` property with `SmithChartLegendRenderEventArgs` parameter to allow legend customization:

```razor
@using Syncfusion.Blazor.Charts

<SfSmithChart>
    <SmithChartEvents LegendRendering="OnLegendRendering"></SmithChartEvents>
    <SmithChartLegendSettings Visible="true"></SmithChartLegendSettings>
    <SmithChartSeriesCollection>
        <SmithChartSeries Name="Transmission Line" DataSource="@TransmissionData" 
                          Reactance="Reactance" Resistance="Resistance">
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };

    private void OnLegendRendering(SmithChartLegendRenderEventArgs args)
    {
        // Customize legend before rendering
        Console.WriteLine($"Legend rendering for: {args.Text}");
    }
}
```

### Complete Event Example

```razor
@page "/smith-chart-events"
@using Syncfusion.Blazor.Charts

<h3>Smith Chart with Event Handlers</h3>

<div style="margin-bottom: 20px;">
    <h4>Event Log:</h4>
    <ul>
        @foreach (var log in eventLogs)
        {
            <li>@log</li>
        }
    </ul>
</div>

<SfSmithChart>
    <SmithChartEvents 
        Loaded="OnChartLoaded"
        SeriesRender="OnSeriesRender"
        AxisLabelRendering="OnAxisLabelRendering"
        LegendRendering="OnLegendRendering">
    </SmithChartEvents>
    
    <SmithChartLegendSettings Visible="true"></SmithChartLegendSettings>
    
    <SmithChartSeriesCollection>
        <SmithChartSeries Name="Before Matching" DataSource="@BeforeData" 
                          Reactance="Reactance" Resistance="Resistance">
        </SmithChartSeries>
        <SmithChartSeries Name="After Matching" DataSource="@AfterData" 
                          Reactance="Reactance" Resistance="Resistance">
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    private List<string> eventLogs = new();

    private bool loadedCalled;
    private int seriesRenderCount;
    private bool axisLabelCalled;
    private bool legendCalled;
    private bool stateUpdated;

    private const int ExpectedSeriesCount = 2;
    
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> BeforeData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 }
    };
    
    public List<SmithChartData> AfterData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 5, Reactance = 2 },
        new SmithChartData { Resistance = 3, Reactance = 1.2 },
        new SmithChartData { Resistance = 1.5, Reactance = 0.5 }
    };
    
    private void OnChartLoaded(SmithChartLoadedEventArgs args)
    {
        loadedCalled = true;
        eventLogs.Add($"{DateTime.Now:HH:mm:ss} - Chart loaded");
        CheckAndUpdateState();
    }

    private void OnSeriesRender(SmithChartSeriesRenderEventArgs args)
    {
        seriesRenderCount++;
        eventLogs.Add($"{DateTime.Now:HH:mm:ss} - Series rendered: {args.Text}");
        CheckAndUpdateState();
    }

    private void OnAxisLabelRendering(SmithChartAxisLabelRenderEventArgs args)
    {
        axisLabelCalled = true;
        eventLogs.Add($"{DateTime.Now:HH:mm:ss} - Axis label rendering");
        CheckAndUpdateState();
    }

    private void OnLegendRendering(SmithChartLegendRenderEventArgs args)
    {
        legendCalled = true;
        eventLogs.Add($"{DateTime.Now:HH:mm:ss} - Legend rendering: {args.Text}");
        CheckAndUpdateState();
    }

    private void CheckAndUpdateState()
    {
        if (stateUpdated)
            return;

        if (loadedCalled &&
            axisLabelCalled &&
            legendCalled &&
            seriesRenderCount == ExpectedSeriesCount)
        {
            stateUpdated = true;

            _ = InvokeAsync(() =>
            {
                StateHasChanged();
            });
        }
    }
}
```

## Accessibility

Accessibility features ensure Smith Charts are usable by all users through support for keyboard navigation, screen readers, high contrast modes, and color-blind friendly design. The `SmithChartSeriesMarker.Visible` property controls marker visibility, and ARIA attributes can be applied to the chart component.

### WCAG 2.1 AA Compliance

Smith Charts should meet WCAG 2.1 Level AA standards through proper implementation of accessibility properties and features:

**Perceivable:**
- Sufficient color contrast (4.5:1 for text, 3:1 for graphics)
- Alternative text for data visualization
- Visible focus indicators

**Operable:**
- Keyboard navigation support
- No keyboard traps
- Sufficient time for interactions

**Understandable:**
- Clear labels and instructions
- Consistent navigation
- Error identification

**Robust:**
- Semantic HTML structure
- ARIA attributes for screen readers
- Compatible with assistive technologies

### Keyboard Navigation

Keyboard Navigation support is enabled by default with the `tabindex="0"` attribute. Smith Charts support keyboard navigation through Tab, Arrow Keys, Enter/Space, and Escape keys:

| Key | Action |
|-----|--------|
| **Tab** | Move focus to chart |
| **Arrow Keys** | Navigate between data points |
| **Enter/Space** | Activate focused element (legend toggle) |
| **Escape** | Close tooltip or legend menu |

**Implementation using `SmithChartSeriesMarker.Visible` property:**

```razor
@using Syncfusion.Blazor.Charts

<SfSmithChart>
    <SmithChartTitle Text="Antenna Impedance"></SmithChartTitle>
    <SmithChartSeriesCollection>
        <SmithChartSeries Name="Measurement Data" 
                          DataSource="@TransmissionData" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
            <SmithChartSeriesTooltip Visible="true"></SmithChartSeriesTooltip>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };
}
```

### Screen Reader Support

Screen Reader Support is provided through ARIA attributes including `role="img"`, `aria-label`, `aria-describedby`, and `tabindex` properties applied to the `SfSmithChart` component for accessibility:

```razor
@using Syncfusion.Blazor.Charts

<SfSmithChart>
    
    <SmithChartTitle Text="Transmission Line Impedance"></SmithChartTitle>
    
    <SmithChartSeriesCollection>
        <SmithChartSeries Name="Impedance Trace" 
                          DataSource="@TransmissionData" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

<div id="chart-description">
    This Smith Chart shows the impedance matching progression for a transmission line,
    starting at 10 Ohms resistance and 25 Ohms reactance, ending near perfect match at the center.
</div>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };
}
```

**ARIA Attributes:**
- `role="img"` - Identifies chart as an image
- `aria-label` - Brief chart description
- `aria-describedby` - Links to detailed description
- `tabindex="0"` - Makes chart keyboard focusable

### High Contrast Support

High Contrast Support is configured through the `SmithChartSeries.Fill` property, `SmithChartSeriesMarker.Width` property for border thickness, and `SmithChartSeriesMarkerBorder.Color` property to ensure visibility in high contrast themes:

```razor
@using Syncfusion.Blazor.Charts

<SfSmithChart>
    <SmithChartTitle Text="High Contrast Smith Chart"></SmithChartTitle>
    
    <SmithChartSeriesCollection>
        <SmithChartSeries Name="Data Series" 
                          DataSource="@TransmissionData" 
                          Reactance="Reactance" 
                          Resistance="Resistance"
                          Fill="#FFFF00"
                          Width="3">
            <SmithChartSeriesMarker Visible="true"
                                    Shape="@Syncfusion.Blazor.Charts.Shape.Circle"
                                    Height="10"
                                    Width="10"
                                    Fill="#FFFF00">
                <SmithChartSeriesMarkerBorder Color="#000000" Width="2">
                </SmithChartSeriesMarkerBorder>
            </SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };
}
```

**High Contrast Guidelines:**
- Use bold line widths (3-4px)
- Large markers (10-12px)
- Strong borders on markers
- High contrast color combinations
- Avoid relying solely on color to convey information

### Color Blindness Considerations

Color Blindness Considerations are addressed by using accessible color palettes through the `SmithChartSeries.Fill` property and combining colors with different `SmithChartSeriesMarker.Shape` properties (Circle, Diamond, Rectangle) for visual differentiation:

```csharp
// Color-blind friendly palette
private readonly string[] AccessibleColors = new[]
{
    "#0173B2", // Blue
    "#DE8F05", // Orange
    "#029E73", // Green
    "#CC78BC", // Purple
    "#CA9161", // Brown
    "#FBAFE4"  // Pink
};
```

**Best Practices:**
- Combine color with patterns/shapes
- Use different marker shapes for series
- Add data labels or tooltips
- Test with color blindness simulators
- Provide alternative data views

## Testing and Validation

Testing and Validation ensures Smith Charts meet accessibility standards through manual testing of keyboard navigation, screen reader compatibility, and ARIA attributes, along with automated testing using accessibility tools.

### Accessibility Testing Checklist

Testing and validation of accessibility features using keyboard navigation (Tab key), screen reader announcements through ARIA properties, browser zoom support, color contrast verification, and ARIA attribute validation:

**Manual Testing:**
- [ ] Navigate chart using only keyboard (Tab key navigation)
- [ ] Verify screen reader announcements with ARIA attributes
- [ ] Test with browser zoom (200-400%)
- [ ] Check color contrast ratios using `SmithChartSeries.Fill` properties
- [ ] Verify visible focus indicators with `tabindex` property
- [ ] Test with high contrast mode using `SmithChartSeriesMarkerBorder.Color`
- [ ] Validate ARIA attributes (`role`, `aria-label`, `aria-describedby`)

**Automated Testing Tools:**
- **axe DevTools** - Browser extension for accessibility scanning
- **WAVE** - Web accessibility evaluation tool
- **Lighthouse** - Chrome DevTools audit
- **NVDA/JAWS** - Screen reader testing

### Validation Example

Validation Example demonstrates WCAG 2.1 AA compliance using the `SmithChartSeriesMarker.Shape` property for different marker types, `SmithChartSeries.Fill` property for color-blind friendly colors, and ARIA attributes for screen reader support:

```razor
@page "/accessible-smith-chart"
@using Syncfusion.Blazor.Charts

<h1 id="page-title">Accessible Smith Chart Example</h1>

<div role="region" aria-labelledby="page-title">
    <SfSmithChart>
        
        <SmithChartTitle Text="Antenna Impedance vs Frequency"></SmithChartTitle>
        
        <SmithChartLegendSettings Visible="true" Position="@Syncfusion.Blazor.Charts.LegendPosition.Bottom">
        </SmithChartLegendSettings>
        
        <SmithChartSeriesCollection>
            <SmithChartSeries Name="100 MHz" 
                              DataSource="@Freq100" 
                              Reactance="Reactance" 
                              Resistance="Resistance"
                              Fill="#0173B2"
                              Width="3">
                <SmithChartSeriesMarker Visible="true"
                                        Shape="@Syncfusion.Blazor.Charts.Shape.Circle"
                                        Height="10"
                                        Width="10">
                </SmithChartSeriesMarker>
                <SmithChartSeriesTooltip Visible="true"></SmithChartSeriesTooltip>
            </SmithChartSeries>
            
            <SmithChartSeries Name="500 MHz" 
                              DataSource="@Freq500" 
                              Reactance="Reactance" 
                              Resistance="Resistance"
                              Fill="#DE8F05"
                              Width="3">
                <SmithChartSeriesMarker Visible="true"
                                        Shape="@Syncfusion.Blazor.Charts.Shape.Diamond"
                                        Height="10"
                                        Width="10">
                </SmithChartSeriesMarker>
                <SmithChartSeriesTooltip Visible="true"></SmithChartSeriesTooltip>
            </SmithChartSeries>
            
            <SmithChartSeries Name="1 GHz" 
                              DataSource="@Freq1000" 
                              Reactance="Reactance" 
                              Resistance="Resistance"
                              Fill="#029E73"
                              Width="3">
                <SmithChartSeriesMarker Visible="true"
                                        Shape="@Syncfusion.Blazor.Charts.Shape.Rectangle"
                                        Height="10"
                                        Width="10">
                </SmithChartSeriesMarker>
                <SmithChartSeriesTooltip Visible="true"></SmithChartSeriesTooltip>
            </SmithChartSeries>
        </SmithChartSeriesCollection>
    </SfSmithChart>
    
    <div id="chart-details" class="sr-only">
        This Smith Chart displays antenna impedance measurements across three frequency points:
        100 MHz (blue circles), 500 MHz (orange diamonds), and 1 GHz (green rectangles).
        The impedance values progress from higher resistance and reactance at lower frequencies
        toward improved matching at higher frequencies.
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
        white-space: nowrap;
        border-width: 0;
    }
</style>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> Freq100 = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 }
    };
    
    public List<SmithChartData> Freq500 = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 8, Reactance = 15 },
        new SmithChartData { Resistance = 5, Reactance = 3 },
        new SmithChartData { Resistance = 2.5, Reactance = 1.2 }
    };
    
    public List<SmithChartData> Freq1000 = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 6, Reactance = 10 },
        new SmithChartData { Resistance = 4, Reactance = 2 },
        new SmithChartData { Resistance = 2, Reactance = 0.8 }
    };
}
```

## Best Practices

### Export Best Practices

Export Best Practices recommend using the appropriate `ExportType` property based on requirements and performance considerations:

**Format Selection using `ExportType` property:**
- **PNG** - `ExportType.PNG` for general purpose, good quality, larger files
- **JPEG** - `ExportType.JPEG` for smaller files, slight quality loss, no transparency
- **SVG** - `ExportType.SVG` for scalable, editable format, best for publications
- **PDF** - `ExportType.PDF` for direct document integration, print-ready output

**Performance:**
- Export after chart fully loads using `Loaded` event property
- Avoid frequent exports (adds overhead)
- Consider server-side export for batch operations

### Event Handling

Event Handling best practices using `SmithChartEvents` properties for proper initialization and state management:

**Do:**
- Use `SmithChartEvents` properties for initialization and state management
- Log events for debugging during development
- Keep event handlers lightweight
- Use async/await for long-running operations in `ExportAsync` and `PrintAsync` methods

**Don't:**
- Trigger expensive operations in event handler properties
- Modify chart state during `SeriesRender`, `AxisLabelRendering`, or `LegendRendering` events
- Create event handler infinite loops

### Accessibility Compliance

Accessibility Compliance requirements using Smith Chart properties and ARIA attributes:

**Requirements:**
- Add `aria-label` and `aria-describedby` to `SfSmithChart` component
- Provide keyboard navigation support with `tabindex="0"` property
- Test with screen readers for ARIA attribute announcements
- Ensure 4.5:1 color contrast using `SmithChartSeries.Fill` and `SmithChartSeriesMarkerBorder.Color`
- Support high contrast modes with bold `SmithChartSeriesMarker.Width` properties
- Include text alternatives for data using `aria-describedby` and descriptive `role` attributes

