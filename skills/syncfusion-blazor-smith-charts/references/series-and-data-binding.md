# Series and Data Binding

## Table of Contents
- [Data Structure](#data-structure)
    - [Basic Data Model](#basic-data-model)
    - [Example Data](#example-data)
- [Basic Series Configuration](#basic-series-configuration)
    - [Single Series](#single-series)
- [Series Customization](#series-customization)
    - [Fill Color](#fill-color)
    - [Line Width](#line-width)
    - [Opacity](#opacity)
    - [Visibility](#visibility)
- [Multiple Series](#multiple-series)
- [Advanced Data Scenarios](#advanced-data-scenarios)
    - [Custom Property Names](#custom-property-names)
    - [Normalized Impedance](#normalized-impedance)
    - [S-Parameter Conversion](#s-parameter-conversion)
    - [Handling Null Values](#handling-null-values)
- [Dynamic Data Updates](#dynamic-data-updates)
    - [Real-Time Updates](#real-time-updates)
    - [Observable Collections](#observable-collections)
- [Best Practices](#best-practices)
    - [Data Validation](#data-validation)
    - [Performance Optimization](#performance-optimization)
    - [Color Schemes](#color-schemes)
- [Common Patterns](#common-patterns)
    - [Before/After Comparison](#beforeafter-comparison)
    - [Frequency Sweep](#frequency-sweep)

Learn how to configure Smith Chart series, bind impedance data, and customize series appearance for transmission line and RF circuit visualization.

## Data Structure

The **Resistance** property represents the real part of impedance, and the **Reactance** property represents the imaginary part. Smith Chart series require data with **Resistance** and **Reactance** properties representing complex impedance values.

### Basic Data Model

The `Resistance` property stores the real component of impedance, while the `Reactance` property stores the imaginary component. Both are nullable to handle missing or invalid data points.

```csharp
public class SmithChartData
{
    public double? Resistance { get; set; }  // Real part (Ω)
    public double? Reactance { get; set; }   // Imaginary part (Ω)
}
```

**Why nullable `double?`:**
- Allows handling missing or invalid data points gracefully
- Smith Chart skips null values automatically
- Prevents exceptions with incomplete datasets

### Example Data

```csharp
public List<SmithChartData> TransmissionData = new List<SmithChartData>
{
    new SmithChartData { Resistance = 10, Reactance = 25 },    // 10 + j25 Ω
    new SmithChartData { Resistance = 6, Reactance = 4.5 },    // 6 + j4.5 Ω
    new SmithChartData { Resistance = 3.5, Reactance = 1.6 },  // 3.5 + j1.6 Ω
    new SmithChartData { Resistance = 2, Reactance = 1.2 },    // 2 + j1.2 Ω
    new SmithChartData { Resistance = 1, Reactance = 0.8 },    // 1 + j0.8 Ω
    new SmithChartData { Resistance = 0, Reactance = 0.2 }     // 0 + j0.2 Ω (matched)
};
```

## Basic Series Configuration

The `DataSource` property binds the data collection, while `Resistance` and `Reactance` properties map to the corresponding data properties.

### Single Series

The `SmithChartSeries` component uses `DataSource` to bind data, and the `Resistance` and `Reactance` properties specify which data properties contain the real and imaginary impedance components.

```razor
@using Syncfusion.Blazor.Charts

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries Name="Transmission Line" 
                          DataSource="@TransmissionData" 
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
}
```

**Key Properties:**
- `Name` - Series identifier shown in legend
- `DataSource` - Collection of data points
- `Resistance` - Property name for real part
- `Reactance` - Property name for imaginary part

## Series Customization

### Fill Color

The `Fill` property controls the series line color for visual distinction between multiple series.

```razor
<SmithChartSeries Name="Transmission Line" 
                  DataSource="@TransmissionData" 
                  Reactance="Reactance" 
                  Resistance="Resistance"
                  Fill="#FF6B6B">
</SmithChartSeries>
```

**Color Formats:**
- Hex: `#FF6B6B`, `#4ECDC4`
- RGB: `rgb(255, 107, 107)`
- RGBA: `rgba(78, 205, 196, 0.8)`
- Named: `red`, `blue`, `green`

### Line Width

The `Width` property adjusts the series line thickness to control visibility and emphasis.

```razor
<SmithChartSeries Name="Transmission Line" 
                  DataSource="@TransmissionData" 
                  Reactance="Reactance" 
                  Resistance="Resistance"
                  Width="3">
</SmithChartSeries>
```

**Width Guidelines:**
- `1-2` - Thin lines for multiple overlapping series
- `2-3` - Standard visibility (default)
- `3-5` - Emphasized series for key data

### Opacity

The `Opacity` property controls series transparency, allowing background reference series or overlay effects.

```razor
<SmithChartSeries Name="Transmission Line" 
                  DataSource="@TransmissionData" 
                  Reactance="Reactance" 
                  Resistance="Resistance"
                  Opacity="0.75">
</SmithChartSeries>
```

**Opacity Range:** 0.0 (transparent) to 1.0 (opaque)

**Use Cases:**
- `0.4-0.6` - Background reference series
- `0.7-0.9` - Semi-transparent overlays
- `1.0` - Primary series requiring full visibility

### Visibility

The `Visible` property determines whether the series is displayed on the chart.

```razor
@using Syncfusion.Blazor.Charts

<SmithChartSeries Name="Transmission Line" 
                  DataSource="@TransmissionData" 
                  Reactance="Reactance" 
                  Resistance="Resistance"
                  Visible="@showSeries">
</SmithChartSeries>

@code {
    private bool showSeries = true;
    
    private void ToggleSeries()
    {
        showSeries = !showSeries;
    }
}
```

## Multiple Series

Add multiple `SmithChartSeries` components with different `DataSource`, `Fill`, and `Width` properties to display and compare multiple transmission lines or frequency responses.

```razor
@using Syncfusion.Blazor.Charts

<SfSmithChart Height="600px" Width="100%">
    <SmithChartTitle Text="Frequency Response Comparison"></SmithChartTitle>
    <SmithChartLegendSettings Visible="true" Position="@Syncfusion.Blazor.Charts.LegendPosition.Bottom">
    </SmithChartLegendSettings>
    <SmithChartSeriesCollection>
        <!-- 100 MHz -->
        <SmithChartSeries Name="100 MHz" 
                          DataSource="@Freq100Data" 
                          Reactance="Reactance" 
                          Resistance="Resistance"
                          Fill="#FF6B6B"
                          Width="2">
        </SmithChartSeries>
        
        <!-- 500 MHz -->
        <SmithChartSeries Name="500 MHz" 
                          DataSource="@Freq500Data" 
                          Reactance="Reactance" 
                          Resistance="Resistance"
                          Fill="#4ECDC4"
                          Width="2">
        </SmithChartSeries>
        
        <!-- 1 GHz -->
        <SmithChartSeries Name="1 GHz" 
                          DataSource="@Freq1000Data" 
                          Reactance="Reactance" 
                          Resistance="Resistance"
                          Fill="#FFD93D"
                          Width="2">
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> Freq100Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 }
    };
    
    public List<SmithChartData> Freq500Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 8, Reactance = 15 },
        new SmithChartData { Resistance = 5, Reactance = 3 },
        new SmithChartData { Resistance = 2.5, Reactance = 1.2 }
    };
    
    public List<SmithChartData> Freq1000Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 6, Reactance = 10 },
        new SmithChartData { Resistance = 4, Reactance = 2 },
        new SmithChartData { Resistance = 2, Reactance = 0.8 }
    };
}
```

## Advanced Data Scenarios

### Custom Property Names

The `Resistance` and `Reactance` properties in the series can be mapped to any custom property names in your data model to match your specific data structure.

```csharp
public class RFMeasurement
{
    public double? RealPart { get; set; }
    public double? ImaginaryPart { get; set; }
    public string Frequency { get; set; }
}
```

```razor
<SmithChartSeries Name="S11 Measurement" 
                  DataSource="@Measurements" 
                  Reactance="ImaginaryPart" 
                  Resistance="RealPart">
</SmithChartSeries>
```

### Normalized Impedance

Use the `Resistance` and `Reactance` properties to store normalized impedance values (Z/Z₀) by dividing the actual impedance by the characteristic impedance Z₀.

```csharp
public List<SmithChartData> NormalizeImpedance(List<Complex> rawData, double Z0 = 50.0)
{
    return rawData.Select(z => new SmithChartData
    {
        Resistance = z.Real / Z0,
        Reactance = z.Imaginary / Z0
    }).ToList();
}
```

**Common Characteristic Impedances (Z₀):**
- 50Ω - Most RF systems, test equipment
- 75Ω - Cable TV, video transmission
- 300Ω - Twin-lead antenna feedlines

### S-Parameter Conversion

Convert reflection coefficients (S-parameters) to impedance values for the `Resistance` and `Reactance` properties using the formula Z = Z₀ * (1 + S11) / (1 - S11).

```csharp
public SmithChartData SParameterToImpedance(Complex S11, double Z0 = 50.0)
{
    // Convert reflection coefficient to impedance
    Complex Z = Z0 * (1 + S11) / (1 - S11);
    
    return new SmithChartData
    {
        Resistance = Z.Real / Z0,  // Normalized
        Reactance = Z.Imaginary / Z0
    };
}
```

### Handling Null Values

Setting `Resistance` or `Reactance` properties to `null` creates gaps in the series line, allowing for segmented data visualization.

```csharp
public List<SmithChartData> DataWithGaps = new List<SmithChartData>
{
    new SmithChartData { Resistance = 10, Reactance = 25 },
    new SmithChartData { Resistance = 6, Reactance = 4.5 },
    new SmithChartData { Resistance = null, Reactance = null },  // Gap
    new SmithChartData { Resistance = 2, Reactance = 1.2 },
    new SmithChartData { Resistance = 1, Reactance = 0.8 }
};
```

**Use Cases for Gaps:**
- Invalid measurement points
- Data filtering (out-of-range values)
- Segment separation in multi-section analysis

## Dynamic Data Updates

### Observable Collections

Use `ObservableCollection<SmithChartData>` for `DataSource` to enable automatic chart updates when new data points are added to the collection.

```csharp
using System.Collections.ObjectModel;

public ObservableCollection<SmithChartData> LiveData { get; set; } = new();

private void AddDataPoint(double r, double x)
{
    LiveData.Add(new SmithChartData { Resistance = r, Reactance = x });
    StateHasChanged();
}
```

## Best Practices

### Data Validation

Validate `Resistance` and `Reactance` properties before binding to ensure data integrity and prevent visualization errors.

```csharp
private bool IsValidImpedance(SmithChartData data)
{
    if (data.Resistance == null || data.Reactance == null)
        return false;
    
    // Resistance should be non-negative for passive circuits
    if (data.Resistance < 0)
        return false;
    
    // Check for reasonable ranges (normalized)
    if (Math.Abs(data.Resistance.Value) > 1000 || 
        Math.Abs(data.Reactance.Value) > 1000)
        return false;
    
    return true;
}

public List<SmithChartData> FilterValidData(List<SmithChartData> rawData)
{
    return rawData.Where(IsValidImpedance).ToList();
}
```

### Performance Optimization

Optimize `DataSource` performance for large datasets by subsampling data points to maintain chart responsiveness without losing data accuracy.

```csharp
// Subsample data for better performance
public List<SmithChartData> SubsampleData(List<SmithChartData> data, int maxPoints = 500)
{
    if (data.Count <= maxPoints)
        return data;
    
    int step = data.Count / maxPoints;
    return data.Where((item, index) => index % step == 0).ToList();
}
```

### Color Schemes

Apply consistent `Fill` property values across series to create professional color schemes for multi-series comparisons.

```csharp
private readonly string[] FrequencyColors = new[]
{
    "#FF6B6B", // Low frequency - Red
    "#4ECDC4", // Mid frequency - Cyan
    "#FFD93D", // High frequency - Yellow
    "#95E1D3", // Ultra high - Green
    "#F38181"  // Reference - Pink
};
```

## Common Patterns

### Before/After Comparison

Display two series with different `DataSource`, `Fill`, and `Opacity` properties to compare optimization results:

```razor
<SmithChartSeriesCollection>
    <SmithChartSeries Name="Before Optimization" 
                      DataSource="@BeforeData" 
                      Reactance="Reactance" 
                      Resistance="Resistance"
                      Fill="#FF6B6B"
                      Width="2"
                      Opacity="0.7">
    </SmithChartSeries>
    <SmithChartSeries Name="After Optimization" 
                      DataSource="@AfterData" 
                      Reactance="Reactance" 
                      Resistance="Resistance"
                      Fill="#4ECDC4"
                      Width="3">
    </SmithChartSeries>
</SmithChartSeriesCollection>
```

### Frequency Sweep

Generate multiple `SmithChartSeries` each with unique `DataSource` containing `Resistance` and `Reactance` values for different frequency points:

```csharp
public Dictionary<string, List<SmithChartData>> GenerateFrequencySweep()
{
    return new Dictionary<string, List<SmithChartData>>
    {
        ["100 MHz"] = GenerateDataForFrequency(100e6),
        ["500 MHz"] = GenerateDataForFrequency(500e6),
        ["1 GHz"] = GenerateDataForFrequency(1e9),
        ["2 GHz"] = GenerateDataForFrequency(2e9)
    };
}
```
