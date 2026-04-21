# Export, Events, and Accessibility

Learn about export functionality, event handling, and accessibility features in the Syncfusion Blazor Range Selector.

## API Reference Summary

**Namespace:** `Syncfusion.Blazor.Charts`

**Main Classes:**
- `SfRangeNavigator` - Primary component for range navigation
- `RangeNavigatorEvents` - Event handler definitions
- `ChangedEventArgs` - Event arguments for range changes
- `RangeLoadedEventArgs` - Event arguments for component loading
- `RangeResizeEventArgs` - Event arguments for resize operations
- `RangeTooltipRenderEventArgs` - Event arguments for tooltip rendering
- `RangeLabelRenderEventArgs` - Event arguments for label rendering

**Key Methods:**
- `Export(ExportType, string, Orientation?)` - Export to PNG, JPEG, SVG, or PDF
- `Print()` - Print the component
- `Refresh()` - Refresh/redraw the component

**Supported Export Types:**
- `ExportType.PNG` - Portable Network Graphics
- `ExportType.JPEG` - JPEG image format
- `ExportType.SVG` - Scalable Vector Graphics
- `ExportType.PDF` - Portable Document Format

## Table of Contents

- [Export](#export)
  - [PNG Export](#png-export)
  - [JPEG Export](#jpeg-export)
  - [SVG Export](#svg-export)
  - [PDF Export](#pdf-export)
- [Events](#events)
  - [Changed Event](#changed-event)
  - [Loaded Event](#loaded-event)
  - [TooltipRender Event](#tooltiprender-event)
  - [LabelRender Event](#labelrender-event)
- [Accessibility](#accessibility)
  - [WCAG Compliance](#wcag-compliance)
  - [Keyboard Navigation](#keyboard-navigation)
  - [Screen Reader Support](#screen-reader-support)
  - [High Contrast Themes](#high-contrast-themes)
  - [Testing Checklist](#testing-checklist)

## Export
Export the Range Selector using the `ExportAsync()` method with `ExportType` parameter (PNG, JPEG, SVG, PDF).

### PNG Export
Export the range selector as a PNG image using `ExportAsync(ExportType.PNG, filename)` method:

```razor
@using Syncfusion.Blazor.Charts

<button @onclick="ExportToPNG" class="btn btn-primary">Export as PNG</button>

<SfRangeNavigator @ref="RangeNavigatorRef"
                  @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close"
                              Type="RangeNavigatorType.Area">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    private SfRangeNavigator RangeNavigatorRef;

    public object Range = new DateTime[]
    {
        DateTime.Now.AddMonths(-6),
        DateTime.Now
    };

    public List<StockInfo> StockData = GetStockData();

    private async Task ExportToPNG()
    {
        await RangeNavigatorRef.ExportAsync(ExportType.PNG, "RangeSelector");
    }

    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }

    private static List<StockInfo> GetStockData()
    {
        var data = new List<StockInfo>();
        var random = new Random();
        var startDate = DateTime.Now.AddMonths(-6);
        double price = 120;

        for (int i = 0; i < 180; i++)
        {
            price += random.NextDouble() * 4 - 2;
            price = Math.Max(price, 50);

            data.Add(new StockInfo
            {
                Date = startDate.AddDays(i),
                Close = Math.Round(price, 2)
            });
        }

        return data;
    }
}
```

### JPEG Export
Export as JPEG using `ExportAsync(ExportType.JPEG, filename)` method:

```razor
@using Syncfusion.Blazor.Charts
<button @onclick="ExportToJPEG">Export as JPEG</button>

@code {
    private async Task ExportToJPEG()
    {
        await RangeNavigatorRef.ExportAsync(ExportType.JPEG, "RangeSelector");
    }
}
```

### SVG Export
Export as scalable vector graphics using `ExportAsync(ExportType.SVG, filename)` method:

```razor
@using Syncfusion.Blazor.Charts
<button @onclick="ExportToSVG">Export as SVG</button>

@code {
    private async Task ExportToSVG()
    {
        await RangeNavigatorRef.ExportAsync(ExportType.SVG, "RangeSelector");
    }
}
```

### PDF Export
Export as PDF document using `ExportAsync(ExportType.PDF, filename, PdfPageOrientation)` method:

```razor
@using Syncfusion.Blazor.Charts
<button @onclick="ExportToPDF">Export as PDF</button>

@code {
    private async Task ExportToPDF()
    {
        await RangeNavigatorRef.ExportAsync(ExportType.PDF, "RangeSelector", PdfPageOrientation.Landscape);
    }
}
```

### Complete Export Example
Implement complete export functionality with multiple formats using `ExportAsync()` method and `ExportType` enum:

```razor
@page "/export-demo"
@using Syncfusion.Blazor.Charts
@using Syncfusion.PdfExport

<div class="export-container">
    <h3>Range Selector Export</h3>
    
    <div class="export-buttons">
        <button @onclick="() => ExportChart(ExportType.PNG)" class="btn btn-primary">
            <span class="icon">📷</span> Export PNG
        </button>
        <button @onclick="() => ExportChart(ExportType.JPEG)" class="btn btn-success">
            <span class="icon">🖼️</span> Export JPEG
        </button>
        <button @onclick="() => ExportChart(ExportType.SVG)" class="btn btn-info">
            <span class="icon">📐</span> Export SVG
        </button>
        <button @onclick="() => ExportChart(ExportType.PDF)" class="btn btn-danger">
            <span class="icon">📄</span> Export PDF
        </button>
    </div>
    
    <div class="range-selector-panel">
        <SfRangeNavigator @ref="RangeNavigatorRef"
                          @bind-Value="@Range" 
                          ValueType="RangeValueType.DateTime"
                          LabelFormat="MMM-yy"
                          Width="100%"
                          Height="150px">
            <RangeNavigatorRangeTooltipSettings Enable="true"></RangeNavigatorRangeTooltipSettings>
            <RangeNavigatorSeriesCollection>
                <RangeNavigatorSeries DataSource="@StockData" 
                                      XName="Date" 
                                      YName="Close"
                                      Type="RangeNavigatorType.Area"
                                      Fill="rgba(33, 150, 243, 0.3)">
                    <RangeNavigatorSeriesBorder Color="#2196F3" Width="2"></RangeNavigatorSeriesBorder>
                </RangeNavigatorSeries>
            </RangeNavigatorSeriesCollection>
        </SfRangeNavigator>
    </div>
    
    @if (!string.IsNullOrEmpty(exportMessage))
    {
        <div class="alert alert-success">@exportMessage</div>
    }
</div>

@code {
    private SfRangeNavigator RangeNavigatorRef;
    public object Range = new DateTime[] { DateTime.Now.AddMonths(-6), DateTime.Now };
    public List<StockInfo> StockData = GenerateStockData();
    private string exportMessage = "";
    
    private async Task ExportChart(ExportType type)
    {
        var fileName = $"RangeSelector_{DateTime.Now:yyyyMMdd_HHmmss}";
        
        if (type == ExportType.PDF)
        {
            await RangeNavigatorRef.ExportAsync(type, fileName, PdfPageOrientation.Landscape);
        }
        else
        {
            await RangeNavigatorRef.ExportAsync(type, fileName);
        }
        
        exportMessage = $"Exported as {type} successfully!";
        StateHasChanged();
        
        await Task.Delay(3000);
        exportMessage = "";
        StateHasChanged();
    }

    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }

    private static List<StockInfo> GenerateStockData()
    {
        var data = new List<StockInfo>();
        var random = new Random();
        var startDate = DateTime.Now.AddMonths(-6);
        double price = 120;

        for (int i = 0; i < 180; i++)
        {
            price += random.NextDouble() * 4 - 2;
            price = Math.Max(price, 50);

            data.Add(new StockInfo
            {
                Date = startDate.AddDays(i),
                Close = Math.Round(price, 2)
            });
        }

        return data;
    }
}

<style>
    .export-container {
        padding: 20px;
        max-width: 1200px;
        margin: 0 auto;
    }
    
    .export-buttons {
        display: flex;
        gap: 10px;
        margin-bottom: 30px;
        flex-wrap: wrap;
    }
    
    .export-buttons button {
        padding: 10px 20px;
        border: none;
        border-radius: 5px;
        color: white;
        cursor: pointer;
        font-size: 14px;
        display: flex;
        align-items: center;
        gap: 8px;
    }
    
    .btn-primary { background: #2196F3; }
    .btn-success { background: #4CAF50; }
    .btn-info { background: #00BCD4; }
    .btn-danger { background: #F44336; }
    
    .range-selector-panel {
        padding: 20px;
        background: white;
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }
    
    .alert {
        margin-top: 20px;
        padding: 15px;
        border-radius: 5px;
        background: #d4edda;
        color: #155724;
        border: 1px solid #c3e6cb;
    }
</style>
```

## Events
Handle component interactions using event handlers like `Changed`, `Loaded`, `TooltipRender`, and `LabelRender` with their respective EventArgs classes.

### Changed Event
Handle range selection changes using `RangeNavigatorEvents` with `Changed` handler and `ChangedEventArgs` parameter:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator Value="@Range" ValueType="RangeValueType.DateTime">
    <RangeNavigatorEvents Changed="SliderChanged"></RangeNavigatorEvents>

    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@Data"
                              XName="Date"
                              YName="Value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<div class="range-info">
    <p>Start: @Range[0].ToShortDateString()</p>
    <p>End: @Range[1].ToShortDateString()</p>
    <p>Duration: @Duration days</p>
</div>

@code {
    public DateTime[] Range =
    {
        DateTime.Now.AddMonths(-3),
        DateTime.Now
    };

    private int Duration => (int)(Range[1] - Range[0]).TotalDays;

    public List<ChartData> Data = GenerateData();

    public class ChartData
    {
        public DateTime Date { get; set; }
        public double Value { get; set; }
    }

    private static List<ChartData> GenerateData()
    {
        var list = new List<ChartData>();
        var start = DateTime.Now.AddMonths(-3);
        var rand = new Random();
        double val = 100;

        for (int i = 0; i < 90; i++)
        {
            val += rand.NextDouble() * 4 - 2;

            list.Add(new ChartData
            {
                Date = start.AddDays(i),
                Value = Math.Round(val, 2)
            });
        }

        return list;
    }

    public void SliderChanged(ChangedEventArgs args)
    {
        Range = new DateTime[]
        {
            (DateTime)args.Start,
            (DateTime)args.End
        };

        StateHasChanged();
    }
}
```

### Loaded Event
Handle component initialization using `RangeNavigatorEvents` with `Loaded` handler and `RangeLoadedEventArgs` parameter:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator>
    <RangeNavigatorEvents Loaded="OnLoaded"></RangeNavigatorEvents>
    ...
</SfRangeNavigator>

@code {
    private void OnLoaded(RangeLoadedEventArgs args)
    {
        Console.WriteLine("Range Selector loaded successfully");
        // Perform post-load operations
    }
}
```

### TooltipRender Event
Customize tooltip content using `RangeNavigatorEvents` with `TooltipRender` handler and `RangeTooltipRenderEventArgs` parameter:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator>
    <RangeNavigatorEvents TooltipRender="OnTooltipRender"></RangeNavigatorEvents>
    <RangeNavigatorRangeTooltipSettings Enable="true"></RangeNavigatorRangeTooltipSettings>
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@Data" XName="Date" YName="Value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    private void OnTooltipRender(RangeTooltipRenderEventArgs args)
    {
        // Customize tooltip text
        if (args.Text != null && DateTime.TryParse(args.Text, out DateTime date))
        {
            args.Text = $"Selected: {date:MMMM dd, yyyy}";
        }
    }
}
```

### LabelRender Event
Customize axis labels using `RangeNavigatorEvents` with `LabelRender` handler and `RangeLabelRenderEventArgs` parameter:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator>
    <RangeNavigatorEvents LabelRender="OnLabelRender"></RangeNavigatorEvents>
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@Data" XName="Date" YName="Value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    private void OnLabelRender(RangeLabelRenderEventArgs args)
    {
        // Customize label text
        if (DateTime.TryParse(args.Text, out DateTime date))
        {
            args.Text = date.ToString("MMM\nyyyy"); // Add line break
        }
    }
}
```

### Complete Event Handling Example
Implement comprehensive event handling using `RangeNavigatorEvents` with multiple event handlers (`Changed`, `Loaded`, `TooltipRender`, `LabelRender`):

```razor
@page "/events-demo"
@using Syncfusion.Blazor.Charts

<div class="events-container">
    <h3>Range Selector Events</h3>
    
    <SfRangeNavigator Height="160px"
                      Value="@Range"
                      ValueType="RangeValueType.DateTime"
                      LabelFormat="MMM-yy">
        <RangeNavigatorEvents Changed="@OnRangeChanged" Loaded="@OnLoaded" TooltipRender="@OnTooltipRender" LabelRender="@OnLabelRender"></RangeNavigatorEvents>
        <RangeNavigatorRangeTooltipSettings Enable="true"></RangeNavigatorRangeTooltipSettings>
        <RangeNavigatorSeriesCollection>
            <RangeNavigatorSeries DataSource="@StockData" 
                                  XName="Date" 
                                  YName="Close"
                                  Type="RangeNavigatorType.Area">
            </RangeNavigatorSeries>
        </RangeNavigatorSeriesCollection>
    </SfRangeNavigator>
    
    <div class="event-log">
        <h4>Event Log</h4>
        <div class="log-entries">
            @foreach (var entry in EventLog.TakeLast(10).Reverse())
            {
                <div class="log-entry">
                    <span class="timestamp">[@entry.Time.ToString("HH:mm:ss")]</span>
                    <span class="event-name">@entry.Event:</span>
                    <span class="event-data">@entry.Data</span>
                </div>
            }
        </div>
    </div>
</div>

@code {
    public class LogEntry
    {
        public DateTime Time { get; set; }
        public string Event { get; set; }
        public string Data { get; set; }
    }
    
    public DateTime[] Range = new DateTime[] { DateTime.Now.AddMonths(-6), DateTime.Now };
    public List<StockInfo> StockData = GenerateStockData();
    private List<LogEntry> EventLog = new List<LogEntry>();

    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }

    private static List<StockInfo> GenerateStockData()
    {
        var data = new List<StockInfo>();
        var random = new Random();
        var startDate = DateTime.Now.AddMonths(-6);
        double price = 120;

        for (int i = 0; i < 180; i++)
        {
            price += random.NextDouble() * 4 - 2;
            price = Math.Max(price, 50);

            data.Add(new StockInfo
            {
                Date = startDate.AddDays(i),
                Close = Math.Round(price, 2)
            });
        }

        return data;
    }
    
    private void OnRangeChanged(ChangedEventArgs args)
    {
        LogEvent("Changed", $"Range: {args.Start:MM/dd/yyyy} - {args.End:MM/dd/yyyy}");
    }
    
    private void OnLoaded(RangeLoadedEventArgs args)
    {
        LogEvent("Loaded", "Range Selector initialized");
    }
    
    private void OnTooltipRender(RangeTooltipRenderEventArgs args)
    {
        if (args.Text != null && DateTime.TryParse(args.Text, out DateTime date))
        {
            args.Text = $"📅 {date:MMM dd, yyyy}";
        }
        LogEvent("TooltipRender", $"Tooltip: {args.Text}");
    }
    
    private void OnLabelRender(RangeLabelRenderEventArgs args)
    {
        LogEvent("LabelRender", $"Label: {args.Text}");
    }
    
    private void LogEvent(string eventName, string data)
    {
        EventLog.Add(new LogEntry
        {
            Time = DateTime.Now,
            Event = eventName,
            Data = data
        });
        StateHasChanged();
    }
}

<style>
    .events-container {
        padding: 20px;
        max-width: 1200px;
        margin: 0 auto;
    }
    
    .event-log {
        margin-top: 30px;
        padding: 20px;
        background: #f8f9fa;
        border-radius: 8px;
    }
    
    .event-log h4 {
        margin-top: 0;
        color: #495057;
    }
    
    .log-entries {
        max-height: 300px;
        overflow-y: auto;
        font-family: 'Courier New', monospace;
        font-size: 13px;
    }
    
    .log-entry {
        padding: 8px;
        border-bottom: 1px solid #dee2e6;
    }
    
    .timestamp {
        color: #6c757d;
        margin-right: 10px;
    }
    
    .event-name {
        font-weight: bold;
        color: #2196F3;
        margin-right: 5px;
    }
    
    .event-data {
        color: #495057;
    }
</style>
```

## Accessibility
Ensure accessibility compliance using properties like `AllowKeyboardNavigation`, `EnableRtl`, `Theme`, color contrast settings, and ARIA attributes.

### WCAG Compliance
The Range Selector is designed to meet WCAG 2.1 Level AA standards with accessibility features:

- ✅ **Perceivable**: Clear visual indicators, sufficient color contrast
- ✅ **Operable**: Full keyboard navigation support
- ✅ **Understandable**: Consistent behavior, clear labels
- ✅ **Robust**: Compatible with assistive technologies

### Keyboard Navigation
Enable full keyboard navigation using the `AllowKeyboardNavigation` property on `SfRangeNavigator`:

| Key | Action |
|-----|--------|
| **Tab** | Move focus to range selector |
| **Left Arrow** | Move left thumb left, decrease start value |
| **Right Arrow** | Move left thumb right, increase start value |
| **Shift + Left Arrow** | Move right thumb left, decrease end value |
| **Shift + Right Arrow** | Move right thumb right, increase end value |
| **Home** | Move thumb to minimum value |
| **End** | Move thumb to maximum value |
| **Page Up** | Increase value by large step |
| **Page Down** | Decrease value by large step |

#### Enable Keyboard Navigation
Set `AllowKeyboardNavigation="true"` on `SfRangeNavigator` to enable keyboard support:

```razor
<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  AllowKeyboardNavigation="true">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@Data" XName="Date" YName="Value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>
```

### Screen Reader Support

The Range Selector includes ARIA attributes for screen reader compatibility:

```razor
<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime">
    <!-- Automatically includes ARIA attributes:
         - aria-label: "Range Navigator"
         - aria-valuenow: Current value
         - aria-valuemin: Minimum value
         - aria-valuemax: Maximum value
         - aria-orientation: "horizontal"
    -->
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@Data" XName="Date" YName="Value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>
```

### High Contrast Themes
Use high contrast themes by setting the `Theme` property to `Theme.HighContrast`:

```html
<!-- In _Layout.cshtml or index.html -->
<link href="_content/Syncfusion.Blazor.Themes/highcontrast.css" rel="stylesheet" />
```

```razor
<SfRangeNavigator Theme="Theme.HighContrast" ...>
```

### Color Contrast
Ensure sufficient color contrast using `SelectedRegionColor` and `UnselectedRegionColor` properties in `RangeNavigatorStyleSettings` (WCAG AA: 4.5:1 minimum):

```razor
<SfRangeNavigator ...>
    <RangeNavigatorStyleSettings SelectedRegionColor="rgba(0, 0, 0, 0.2)" 
                                 UnselectedRegionColor="rgba(0, 0, 0, 0.05)">
    </RangeNavigatorStyleSettings>
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries Fill="#0066CC"  <!-- High contrast color -->
                              XName="Date" 
                              YName="Value">
            <RangeNavigatorSeriesBorder Color="#003366" Width="2"></RangeNavigatorSeriesBorder>
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>
```

### Focus Indicators
Ensure visible focus indicators for keyboard users by styling the component focus state:

```razor
<style>
    .e-range-navigator:focus {
        outline: 2px solid #2196F3;
        outline-offset: 2px;
    }
</style>
```

### Testing Checklist
Ensure compliance using accessibility audit tools and manual testing with keyboard navigation and screen readers:

#### Visual Accessibility
- [ ] Sufficient color contrast (4.5:1 minimum)
- [ ] Text is readable at 200% zoom
- [ ] No information conveyed by color alone
- [ ] Focus indicators are visible
- [ ] High contrast theme works correctly

#### Keyboard Accessibility
- [ ] All functionality available via keyboard
- [ ] Tab order is logical
- [ ] Focus is visible
- [ ] No keyboard traps
- [ ] Keyboard shortcuts documented

#### Screen Reader Accessibility
- [ ] ARIA labels are present and accurate
- [ ] Dynamic content changes are announced
- [ ] Error messages are announced
- [ ] Instructions are clear
- [ ] Test with NVDA, JAWS, or VoiceOver

#### Testing Tools
- [ ] WAVE browser extension
- [ ] axe DevTools
- [ ] Lighthouse accessibility audit
- [ ] Screen reader testing (NVDA/JAWS/VoiceOver)
- [ ] Keyboard-only navigation testing

### Complete Accessibility Example
Implement comprehensive accessibility using `AllowKeyboardNavigation="true"`, `Theme="Theme.HighContrast"`, `RangeNavigatorStyleSettings` with color properties, and ARIA support:

```razor
@page "/accessibility-demo"
@using Syncfusion.Blazor.Charts

<div class="accessible-container">
    <h3 id="range-selector-heading">Stock Price Range Selector</h3>
    <p id="range-selector-description">
        Use arrow keys to adjust the selected date range. 
        Left/Right arrows move the start date, Shift+Left/Right move the end date.
    </p>
    
    <SfRangeNavigator @bind-Value="@Range" 
                      ValueType="RangeValueType.DateTime"
                      Theme="Theme.HighContrast"
                      LabelFormat="MMM-yy"
                      AllowKeyboardNavigation="true"
                      Width="100%"
                      Height="150px">
        <RangeNavigatorRangeTooltipSettings Enable="true" 
                                            Format="MMMM dd, yyyy">
        </RangeNavigatorRangeTooltipSettings>
        <RangeNavigatorStyleSettings SelectedRegionColor="rgba(0, 0, 0, 0.2)" 
                                     UnselectedRegionColor="rgba(0, 0, 0, 0.05)">
        </RangeNavigatorStyleSettings>
        <RangeNavigatorSeriesCollection>
            <RangeNavigatorSeries DataSource="@StockData" 
                                  XName="Date" 
                                  YName="Close"
                                  Type="RangeNavigatorType.Area"
                                  Fill="#0066CC">
                <RangeNavigatorSeriesBorder Color="#003366" Width="2"></RangeNavigatorSeriesBorder>
            </RangeNavigatorSeries>
        </RangeNavigatorSeriesCollection>
    </SfRangeNavigator>
    
    <div role="status" aria-live="polite" class="range-announcement">
        <p>
            <strong>Selected Range:</strong>
            <span>From {Range[0]:MMMM dd, yyyy} to {Range[1]:MMMM dd, yyyy}</span>
        </p>
        <p>
            <strong>Duration:</strong>
            <span>{(Range[1] - Range[0]).TotalDays:N0} days</span>
        </p>
    </div>
    
    <div class="keyboard-help">
        <h4>Keyboard Shortcuts</h4>
        <dl>
            <dt><kbd>←</kbd> Left Arrow</dt>
            <dd>Decrease start date</dd>
            
            <dt><kbd>→</kbd> Right Arrow</dt>
            <dd>Increase start date</dd>
            
            <dt><kbd>Shift</kbd> + <kbd>←</kbd></dt>
            <dd>Decrease end date</dd>
            
            <dt><kbd>Shift</kbd> + <kbd>→</kbd></dt>
            <dd>Increase end date</dd>
            
            <dt><kbd>Home</kbd></dt>
            <dd>Move to earliest date</dd>
            
            <dt><kbd>End</kbd></dt>
            <dd>Move to latest date</dd>
        </dl>
    </div>
</div>

@code {
    public DateTime[] Range = new DateTime[] { DateTime.Now.AddMonths(-6), DateTime.Now };
    public List<StockInfo> StockData = GenerateStockData();
}

<style>
    .accessible-container {
        padding: 20px;
        max-width: 1200px;
        margin: 0 auto;
    }
    
    .range-announcement {
        margin: 20px 0;
        padding: 15px;
        background: #e7f3ff;
        border-radius: 5px;
        border-left: 4px solid #0066CC;
    }
    
    .keyboard-help {
        margin-top: 30px;
        padding: 20px;
        background: #f8f9fa;
        border-radius: 8px;
    }
    
    .keyboard-help h4 {
        margin-top: 0;
        color: #495057;
    }
    
    .keyboard-help dl {
        display: grid;
        grid-template-columns: 200px 1fr;
        gap: 15px;
        margin: 0;
    }
    
    .keyboard-help dt {
        font-weight: bold;
    }
    
    .keyboard-help dd {
        margin: 0;
    }
    
    kbd {
        padding: 2px 6px;
        background: #f4f4f4;
        border: 1px solid #ccc;
        border-radius: 3px;
        font-family: monospace;
        font-size: 0.9em;
    }
    
    /* Focus styles */
    .e-range-navigator:focus-within {
        outline: 3px solid #2196F3;
        outline-offset: 3px;
    }
</style>
```

## Best Practices
Follow these guidelines using accessibility properties like `AllowKeyboardNavigation`, `Theme`, color contrast settings, and event handlers.

### 1. Always Enable Keyboard Navigation
Set `AllowKeyboardNavigation="true"` property on `SfRangeNavigator`:

```razor
<SfRangeNavigator AllowKeyboardNavigation="true" ...>
```

### 2. Provide Clear Instructions
Include descriptive text and use `RangeNavigatorRangeTooltipSettings` to guide users:

```html
<p>Use arrow keys to adjust the date range.</p>
```

### 3. Use High Contrast Colors
Apply high contrast using `Fill` property and `Theme="Theme.HighContrast"`:

```razor
<RangeNavigatorSeries Fill="#0066CC">  <!-- 7:1 contrast ratio -->
```

### 4. Test with Screen Readers
Verify ARIA attributes work correctly with NVDA, JAWS, or VoiceOver.

### 5. Enable Tooltips
Use `RangeNavigatorRangeTooltipSettings` with `Enable="true"` property for context:

```razor
<RangeNavigatorRangeTooltipSettings Enable="true">
```
