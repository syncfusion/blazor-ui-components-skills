# Dimensions and Appearance

## Table of Contents
- [Overview](#overview)
- [Sizing](#sizing)
  - [Container-Based Sizing](#container-based-sizing)
  - [Explicit Width and Height](#explicit-width-and-height)
  - [Responsive Design](#responsive-design)
- [Padding Configuration](#padding-configuration)
- [Border Customization](#border-customization)
- [Background and Fill](#background-and-fill)
- [Line Width](#line-width)
- [RTL Support](#rtl-support)
- [Container Area Styling](#container-area-styling)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## Overview

Controlling dimensions and appearance is crucial for creating visually appealing and functional Sparkline charts. Syncfusion Blazor Sparkline provides comprehensive options for sizing, styling, and layout customization using properties like `Width`, `Height`, `SparklinePadding`, `SparklineContainerAreaBorder`, `Fill`, `Opacity`, `LineWidth`, and `EnableRtl`.

**Key Customization Areas:**
- Width and Height configuration
- Padding for internal spacing
- Border styling
- Background colors
- Line width for line-based charts
- Right-to-left (RTL) layout support
- Container area appearance

## Sizing

Control sparkline dimensions using `Width` and `Height` properties with pixel, percentage, or container-based sizing options. The `Width` property defines the horizontal span while `Height` property defines the vertical span of the sparkline.

### Container-Based Sizing

The sparkline automatically adapts to its container's dimensions when explicit `Width` and `Height` properties are not specified. When `Width` and `Height` are omitted, the SfSparkline component will inherit the dimensions from its parent container element.

```razor
@using Syncfusion.Blazor.Charts

<div style="width: 650px; height: 350px;">
    <SfSparkline XName="Year" 
                  YName="Population" 
                  TValue="PopulationReport" 
                  DataSource="@PopulationData">
    </SfSparkline>
</div>

@code {
    public class PopulationReport
    {
        public int Year { get; set; }
        public int Population { get; set; }
    }
    
    private List<PopulationReport> PopulationData = new List<PopulationReport> 
    {
        new PopulationReport { Year = 2005, Population = 20090440 },
        new PopulationReport { Year = 2006, Population = 20264080 },
        new PopulationReport { Year = 2007, Population = 20434180 },
        new PopulationReport { Year = 2008, Population = 21007310 },
        new PopulationReport { Year = 2009, Population = 21262640 },
        new PopulationReport { Year = 2010, Population = 21515750 },
        new PopulationReport { Year = 2011, Population = 21766710 },
        new PopulationReport { Year = 2012, Population = 22015580 }
    };
}
```

### Explicit Width and Height

Set specific dimensions using the `Width` and `Height` properties for precise control.

#### Pixel-Based Sizing

Specify exact pixel dimensions using the `Width` property with pixel units (e.g., "350px") and the `Height` property with pixel units (e.g., "150px") for precise control over the sparkline size:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@SalesData" 
              Width="350px" 
              Height="150px"
              Type="SparklineType.Line">
</SfSparkline>

@code {
    public double[] SalesData = new double[] 
    { 
        120, 145, 132, 168, 155, 178, 165 
    };
}
```

#### Percentage-Based Sizing

Size relative to the container using percentage values for the `Width` property (e.g., "80%") and `Height` property (e.g., "70%") to maintain responsiveness:

```razor
@using Syncfusion.Blazor.Charts

<div style="width: 650px; height: 350px;">
    <SfSparkline DataSource="@RevenueData" 
                  Width="80%" 
                  Height="70%"
                  Type="SparklineType.Area"
                  Fill="#E3F2FD">
    </SfSparkline>
</div>

@code {
    public int[] RevenueData = new int[] 
    { 
        250, 320, 290, 380, 350, 420, 395 
    };
}
```

#### Multiple Sizes Example

Demonstrate multiple sparkline sizes using different combinations of the `Width` and `Height` properties to achieve small, medium, and large sparkline layouts:

```razor
@using Syncfusion.Blazor.Charts

<div class="sparkline-sizes">
    <!-- Small -->
    <div class="size-demo">
        <h5>Small (200x60)</h5>
        <SfSparkline DataSource="@Data" 
                      Width="200px" 
                      Height="60px"
                      Type="SparklineType.Line">
        </SfSparkline>
    </div>
    
    <!-- Medium -->
    <div class="size-demo">
        <h5>Medium (350x100)</h5>
        <SfSparkline DataSource="@Data" 
                      Width="350px" 
                      Height="100px"
                      Type="SparklineType.Line">
        </SfSparkline>
    </div>
    
    <!-- Large -->
    <div class="size-demo">
        <h5>Large (500x150)</h5>
        <SfSparkline DataSource="@Data" 
                      Width="500px" 
                      Height="150px"
                      Type="SparklineType.Line">
        </SfSparkline>
    </div>
</div>

@code {
    public int[] Data = new int[] { 3, 6, 4, 8, 5, 9, 7, 11, 8, 10 };
}
```

### Responsive Design

Create responsive sparklines that adapt to screen size using the `Width` property set to "100%" combined with CSS `max-width` constraints and the `SparklineBorder` property for visual definition:

```razor
@using Syncfusion.Blazor.Charts

<div class="responsive-container" style="width: 100%; max-width: 800px;">
    <SfSparkline DataSource="@MetricsData" 
                  Width="100%" 
                  Height="120px"
                  Type="SparklineType.Area"
                  Fill="#FFF3E0"
                  LineWidth="2">
        <SparklineBorder Color="#FF9800" Width="2">
        </SparklineBorder>
    </SfSparkline>
</div>

@code {
    public double[] MetricsData = new double[] 
    { 
        95.5, 98.2, 97.8, 102.3, 100.5, 104.7, 103.2, 101.8 
    };
}
```

#### CSS Grid Layout

Create responsive grid layouts with sparklines using CSS Grid combined with the `Width` property set to "100%" for flexible component sizing within grid cells:

```razor
@using Syncfusion.Blazor.Charts

<style>
    .sparkline-grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
        gap: 20px;
        padding: 20px;
    }
    
    .sparkline-card {
        border: 1px solid #ddd;
        border-radius: 8px;
        padding: 15px;
    }
</style>

<div class="sparkline-grid">
    <div class="sparkline-card">
        <h5>Revenue</h5>
        <SfSparkline DataSource="@Revenue" Width="100%" Height="80px">
        </SfSparkline>
    </div>
    
    <div class="sparkline-card">
        <h5>Orders</h5>
        <SfSparkline DataSource="@Orders" Width="100%" Height="80px">
        </SfSparkline>
    </div>
    
    <div class="sparkline-card">
        <h5>Customers</h5>
        <SfSparkline DataSource="@Customers" Width="100%" Height="80px">
        </SfSparkline>
    </div>
</div>

@code {
    public double[] Revenue = new double[] { 250, 280, 265, 310, 295, 335 };
    public int[] Orders = new int[] { 450, 520, 480, 610, 550, 680 };
    public int[] Customers = new int[] { 1200, 1450, 1380, 1620, 1550, 1780 };
}
```

## Padding Configuration

Control spacing between the sparkline content and its container using the `SparklinePadding` component with its properties: `Top` for top spacing, `Bottom` for bottom spacing, `Left` for left spacing, and `Right` for right spacing.

### Basic Padding

Apply uniform padding using the `SparklinePadding` component with the `Left`, `Right`, `Bottom`, and `Top` properties set to the same value to create equal spacing on all sides:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="new int[]{ 3, 6, 4, 1, 3, 2, 5 }" 
              Type="SparklineType.Area" 
              Height="200px" 
              Width="350px" 
              Fill="#B2CFFF" 
              LineWidth="1">
    <SparklineContainerArea>
        <SparklineContainerAreaBorder Color="#033E96" Width="2">
        </SparklineContainerAreaBorder>
    </SparklineContainerArea>
    <SparklineBorder Color="#033E96" Width="1">
    </SparklineBorder>
    <SparklinePadding Left="20" Right="20" Bottom="20" Top="20">
    </SparklinePadding>
</SfSparkline>
```

### Individual Side Padding

Set different padding values for each side using the `SparklinePadding` component with individual property settings: `Top` property controls top padding, `Bottom` property controls bottom padding, `Left` property controls left padding, and `Right` property controls right padding:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@ChartData" 
              Type="SparklineType.Line" 
              Height="180px" 
              Width="450px"
              LineWidth="2">
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }">
    </SparklineDataLabelSettings>
    <SparklinePadding Top="35" Bottom="10" Left="15" Right="15">
    </SparklinePadding>
</SfSparkline>

@code {
    public int[] ChartData = new int[] { 42, 55, 48, 62, 58, 67, 61 };
}
```

### Padding for Data Labels

When using the `SparklineDataLabelSettings` component with the `Visible` property, add extra top padding using the `SparklinePadding` component with its `Top` property to prevent label clipping:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@LabeledData" 
              Type="SparklineType.Column" 
              Height="180px" 
              Width="450px"
              Fill="#4CAF50">
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }">
        <SparklineFont Size="12" FontWeight="600">
        </SparklineFont>
    </SparklineDataLabelSettings>
    <SparklinePadding Top="40" Bottom="15" Left="10" Right="10">
    </SparklinePadding>
</SfSparkline>

@code {
    public int[] LabeledData = new int[] { 85, 92, 78, 95, 88, 91, 87 };
}
```

### Asymmetric Padding

Create asymmetric padding by setting different values for each side using the `SparklinePadding` component with its `Top`, `Bottom`, `Left`, and `Right` properties configured with distinct values:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@AsymmetricData" 
              Type="SparklineType.Area" 
              Height="160px" 
              Width="400px"
              Fill="#FFE0B2">
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }">
    </SparklineMarkerSettings>
    <SparklinePadding Top="25" Bottom="10" Left="30" Right="10">
    </SparklinePadding>
</SfSparkline>

@code {
    public int[] AsymmetricData = new int[] { 12, 18, 15, 22, 19, 25, 21 };
}
```

## Border Customization

Style the sparkline's container border using the `SparklineContainerAreaBorder` component with the `Color` property for border color and the `Width` property for border thickness, or use the `SparklineBorder` component for styling the data series border.

### Basic Border

Add a border to the sparkline container using the `SparklineContainerArea` component with the `SparklineContainerAreaBorder` child component, configuring the `Color` property for border color and the `Width` property for border thickness:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="new int[]{ 3, 6, 4, 1, 3, 2, 5 }" 
              Type="SparklineType.Area" 
              Height="200px" 
              Width="350px" 
              Fill="#B2CFFF" 
              LineWidth="1">
    <SparklineContainerArea>
        <SparklineContainerAreaBorder Color="#033E96" Width="1">
        </SparklineContainerAreaBorder>
    </SparklineContainerArea>
</SfSparkline>
```

### Thick Border with Padding

Combine the `SparklineContainerAreaBorder` component with a higher `Width` property value and the `SparklinePadding` component with its `Top`, `Bottom`, `Left`, and `Right` properties for enhanced visual separation:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@BorderedData" 
              Type="SparklineType.Column" 
              Height="180px" 
              Width="400px"
              Fill="#FF9800">
    <SparklineContainerArea>
        <SparklineContainerAreaBorder Color="#E65100" Width="3">
        </SparklineContainerAreaBorder>
    </SparklineContainerArea>
    <SparklinePadding Top="15" Bottom="15" Left="15" Right="15">
    </SparklinePadding>
</SfSparkline>

@code {
    public int[] BorderedData = new int[] { 45, 58, 52, 68, 55, 72, 65 };
}
```

### Colored Border Variations

Create different border colors using the `SparklineContainerAreaBorder` component with the `Color` property set to various color values to achieve different visual styles:

```razor
@using Syncfusion.Blazor.Charts

<div class="border-examples">
    <!-- Solid Blue Border -->
    <SfSparkline DataSource="@Data" Height="100px" Width="250px" Fill="#E3F2FD">
        <SparklineContainerArea>
            <SparklineContainerAreaBorder Color="#2196F3" Width="2">
            </SparklineContainerAreaBorder>
        </SparklineContainerArea>
        <SparklinePadding Top="10" Bottom="10" Left="10" Right="10">
        </SparklinePadding>
    </SfSparkline>
    
    <!-- Solid Green Border -->
    <SfSparkline DataSource="@Data" Height="100px" Width="250px" Fill="#E8F5E9">
        <SparklineContainerArea>
            <SparklineContainerAreaBorder Color="#4CAF50" Width="2">
            </SparklineContainerAreaBorder>
        </SparklineContainerArea>
        <SparklinePadding Top="10" Bottom="10" Left="10" Right="10">
        </SparklinePadding>
    </SfSparkline>
    
    <!-- Solid Red Border -->
    <SfSparkline DataSource="@Data" Height="100px" Width="250px" Fill="#FFEBEE">
        <SparklineContainerArea>
            <SparklineContainerAreaBorder Color="#F44336" Width="2">
            </SparklineContainerAreaBorder>
        </SparklineContainerArea>
        <SparklinePadding Top="10" Bottom="10" Left="10" Right="10">
        </SparklinePadding>
    </SfSparkline>
</div>

@code {
    public int[] Data = new int[] { 3, 6, 4, 8, 5, 9, 7 };
}
```

### SparklineBorder vs ContainerAreaBorder

Compare the `SparklineBorder` component (border around data series) with the `SparklineContainerAreaBorder` component (border around entire container), both using the `Color` property for color and the `Width` property for thickness:

```razor
@using Syncfusion.Blazor.Charts

<!-- SparklineBorder: Border around the data series -->
<SfSparkline DataSource="@Data" Height="120px" Width="300px"
              Type="SparklineType.Area" Fill="#E3F2FD">
    <SparklineBorder Color="#2196F3" Width="2">
    </SparklineBorder>
</SfSparkline>

<!-- SparklineContainerAreaBorder: Border around entire container -->
<SfSparkline DataSource="@Data" Height="120px" Width="300px"
              Type="SparklineType.Area" Fill="#E3F2FD">
    <SparklineContainerArea>
        <SparklineContainerAreaBorder Color="#1976D2" Width="2">
        </SparklineContainerAreaBorder>
    </SparklineContainerArea>
</SfSparkline>

<!-- Both Borders -->
<SfSparkline DataSource="@Data" Height="120px" Width="300px"
              Type="SparklineType.Area" Fill="#E3F2FD">
    <SparklineBorder Color="#2196F3" Width="2">
    </SparklineBorder>
    <SparklineContainerArea>
        <SparklineContainerAreaBorder Color="#1976D2" Width="3">
        </SparklineContainerAreaBorder>
    </SparklineContainerArea>
    <SparklinePadding Top="10" Bottom="10" Left="10" Right="10">
    </SparklinePadding>
</SfSparkline>

@code {
    public int[] Data = new int[] { 3, 6, 4, 8, 5, 9, 7 };
}
```

## Background and Fill

Control the colors of the sparkline using the `Fill` property for data series color, the `Opacity` property for transparency level, and the `SparklineContainerArea` component with the `Background` property for container background color.

### Container Background

Set the background color of the sparkline container area using the `SparklineContainerArea` component with the `Background` property to define the container's background color:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="new int[]{ 3, 6, 4, 1, 3, 2, 5 }" 
              Type="SparklineType.Area" 
              Height="200px" 
              Width="350px" 
              Fill="#B2CFFF" 
              LineWidth="1">
    <SparklineContainerArea Background="#EFF1F4">
        <SparklineContainerAreaBorder Color="#033E96" Width="2">
        </SparklineContainerAreaBorder>
    </SparklineContainerArea>
    <SparklineBorder Color="#033E96" Width="1">
    </SparklineBorder>
    <SparklinePadding Left="20" Right="20" Bottom="20" Top="20">
    </SparklinePadding>
</SfSparkline>
```

### Chart Fill Color

The `Fill` property sets the color of the data series, with the optional `Opacity` property controlling the transparency level (0 for fully transparent, 1 for fully opaque).

#### Line Chart Fill

Set the `Fill` property color for line sparklines to control the color of the line in a Line type sparkline:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@LineData" 
              Type="SparklineType.Line" 
              Height="120px" 
              Width="350px"
              Fill="#2196F3"
              LineWidth="2">
</SfSparkline>

@code {
    public int[] LineData = new int[] { 3, 6, 4, 8, 5, 9, 7 };
}
```

#### Area Chart Fill

Set the `Fill` property color and the `Opacity` property for area sparklines to control the area color and transparency:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@AreaData" 
              Type="SparklineType.Area" 
              Height="120px" 
              Width="350px"
              Fill="#4CAF50"
              Opacity="0.6">
</SfSparkline>

@code {
    public int[] AreaData = new int[] { 3, 6, 4, 8, 5, 9, 7 };
}
```

#### Column Chart Fill

Set the `Fill` property for column sparkline bars to control the color of each column in a Column type sparkline:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@ColumnData" 
              Type="SparklineType.Column" 
              Height="120px" 
              Width="350px"
              Fill="#FF9800">
</SfSparkline>

@code {
    public int[] ColumnData = new int[] { 45, 58, 52, 68, 55, 72, 65 };
}
```

### Background Color Combinations

Combine the `Fill` property for series color, the `Opacity` property for transparency, the `LineWidth` property for line thickness, and the `SparklineContainerArea` component with the `Background` property for contrast effects:

```razor
@using Syncfusion.Blazor.Charts

<div class="color-combinations">
    <!-- Light Background -->
    <SfSparkline DataSource="@Data" Height="100px" Width="280px"
                  Type="SparklineType.Area" Fill="#2196F3" Opacity="0.6">
        <SparklineContainerArea Background="#F5F5F5">
        </SparklineContainerArea>
    </SfSparkline>
    
    <!-- Dark Background -->
    <SfSparkline DataSource="@Data" Height="100px" Width="280px"
                  Type="SparklineType.Line" Fill="#FFC107" LineWidth="2">
        <SparklineContainerArea Background="#263238">
        </SparklineContainerArea>
    </SfSparkline>
    
    <!-- Colored Background -->
    <SfSparkline DataSource="@Data" Height="100px" Width="280px"
                  Type="SparklineType.Area" Fill="#FFFFFF" LineWidth="2">
        <SparklineContainerArea Background="#1976D2">
        </SparklineContainerArea>
        <SparklineBorder Color="#FFFFFF" Width="2">
        </SparklineBorder>
    </SfSparkline>
</div>

@code {
    public int[] Data = new int[] { 3, 6, 4, 8, 5, 9, 7 };
}
```

## Line Width

Control the thickness of line-based sparklines using the `LineWidth` property with numeric values (1, 2, 3, etc.) to adjust the thickness of the sparkline line:

### Line Width Variations

Demonstrate different `LineWidth` property values (1, 2, 4) with numeric values to show the visual differences in line thickness for line and area sparklines:

```razor
@using Syncfusion.Blazor.Charts

<div class="line-width-demo">
    <!-- Thin Line -->
    <div>
        <h5>LineWidth: 1</h5>
        <SfSparkline DataSource="@Data" 
                      Type="SparklineType.Line" 
                      Height="80px" 
                      Width="250px"
                      LineWidth="1"
                      Fill="#2196F3">
        </SfSparkline>
    </div>
    
    <!-- Medium Line -->
    <div>
        <h5>LineWidth: 2</h5>
        <SfSparkline DataSource="@Data" 
                      Type="SparklineType.Line" 
                      Height="80px" 
                      Width="250px"
                      LineWidth="2"
                      Fill="#2196F3">
        </SfSparkline>
    </div>
    
    <!-- Thick Line -->
    <div>
        <h5>LineWidth: 4</h5>
        <SfSparkline DataSource="@Data" 
                      Type="SparklineType.Line" 
                      Height="80px" 
                      Width="250px"
                      LineWidth="4"
                      Fill="#2196F3">
        </SfSparkline>
    </div>
</div>

@code {
    public int[] Data = new int[] { 3, 6, 4, 8, 5, 9, 7, 10, 8, 11 };
}
```

### Line Width for Area Charts

Apply the `LineWidth` property to control line thickness and the `SparklineBorder` component with its `Color` and `Width` properties to area sparklines for enhanced visual definition:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@AreaChartData" 
              Type="SparklineType.Area" 
              Height="140px" 
              Width="400px"
              Fill="#E8F5E9"
              LineWidth="3"
              Opacity="0.6">
    <SparklineBorder Color="#4CAF50" Width="3">
    </SparklineBorder>
</SfSparkline>

@code {
    public double[] AreaChartData = new double[] 
    { 
        95.5, 98.2, 97.8, 102.3, 100.5, 104.7, 103.2 
    };
}
```

## RTL Support

Enable right-to-left rendering for internationalization and localization using the `EnableRtl` property set to "true" to support Arabic, Hebrew, and other right-to-left languages.

### Basic RTL Configuration

Configure RTL support using the `EnableRtl` property set to "true" along with optional properties like `Format`, `Height`, and `Width`:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="new double[]{ 300.00, 600.00, 400.21, 100.20, 300.70, 200.04, 500.00 }" 
              Height="200px" 
              Width="350px" 
              Format="c2" 
              EnableRtl="true">
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }" 
                                EdgeLabelMode="EdgeLabelMode.Shift">
    </SparklineDataLabelSettings>
    <SparklinePadding Top="25">
    </SparklinePadding>
</SfSparkline>
```

### RTL with Arabic Text

Support Arabic text and RTL layout using the `EnableRtl` property combined with HTML `dir="rtl"` attribute, and use the `ValueType` property to define the value type as Category:

```razor
@using Syncfusion.Blazor.Charts

<div dir="rtl">
    <h4>مخطط الأداء الشهري</h4>
    <SfSparkline DataSource="@MonthlyData" 
                  TValue="MonthData"
                  XName="MonthName"
                  YName="Value"
                  ValueType="SparklineValueType.Category"
                  Height="150px" 
                  Width="450px"
                  EnableRtl="true"
                  LineWidth="2">
        <SparklineTooltipSettings TValue="MonthData" 
                                  Visible="true" 
                                  Format="${MonthName}: ${Value}">
        </SparklineTooltipSettings>
    </SfSparkline>
</div>

@code {
    public class MonthData
    {
        public string MonthName { get; set; }
        public int Value { get; set; }
    }

    public List<MonthData> MonthlyData = new List<MonthData>
    {
        new MonthData { MonthName = "يناير", Value = 120 },
        new MonthData { MonthName = "فبراير", Value = 145 },
        new MonthData { MonthName = "مارس", Value = 132 },
        new MonthData { MonthName = "أبريل", Value = 168 }
    };
}
```

## Container Area Styling

Comprehensive container styling for professional appearance using the `SparklineContainerArea` component with the `Background` property and the `SparklineContainerAreaBorder` component with `Color` and `Width` properties for complete visual customization.

### Complete Container Customization

Apply complete customization to the container using the `Fill` property for data color, the `LineWidth` property for line thickness, the `Opacity` property for transparency, and the `SparklineContainerArea` component with the `Background` property, combined with the `SparklineContainerAreaBorder` component with `Color` and `Width` properties:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@ProfessionalData" 
              Type="SparklineType.Area" 
              Height="180px" 
              Width="450px"
              Fill="#42A5F5"
              LineWidth="2"
              Opacity="0.7">
    <SparklineContainerArea Background="#FAFAFA">
        <SparklineContainerAreaBorder Color="#1976D2" Width="2">
        </SparklineContainerAreaBorder>
    </SparklineContainerArea>
    <SparklineBorder Color="#1976D2" Width="2">
    </SparklineBorder>
    <SparklinePadding Top="20" Bottom="20" Left="20" Right="20">
    </SparklinePadding>
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }"
                             Size="5"
                             Fill="#FFFFFF">
        <SparklineMarkerBorder Color="#1976D2" Width="2">
        </SparklineMarkerBorder>
    </SparklineMarkerSettings>
</SfSparkline>

@code {
    public double[] ProfessionalData = new double[] 
    { 
        125.5, 148.2, 132.8, 165.3, 155.5, 178.7, 168.2 
    };
}
```

## Complete Examples

### Dashboard Card Design

Create a dashboard card design using the `Type` property set to SparklineType.Area, the `Height` and `Width` properties for sizing, the `Fill` property for color, the `LineWidth` property for line thickness, the `SparklineBorder` component with `Color` and `Width` properties, the `SparklineContainerArea` component with `Background` property, the `SparklinePadding` component with its spacing properties, and the `SparklineMarkerSettings` component with the `Visible` property:

```razor
@using Syncfusion.Blazor.Charts

<style>
    .metric-card {
        background: #ffffff;
        border-radius: 12px;
        box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        padding: 20px;
        margin: 10px;
    }
    
    .metric-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 15px;
    }
    
    .metric-title {
        font-size: 14px;
        color: #666;
        font-weight: 500;
    }
    
    .metric-value {
        font-size: 28px;
        font-weight: bold;
        color: #333;
    }
    
    .metric-change {
        font-size: 14px;
        color: #4CAF50;
    }
</style>

<div class="metric-card">
    <div class="metric-header">
        <div class="metric-title">Monthly Revenue</div>
    </div>
    <div class="metric-value">$458,230</div>
    <div class="metric-change">+12.5% from last month</div>
    <SfSparkline DataSource="@RevenueData" 
                  Type="SparklineType.Area" 
                  Height="80px" 
                  Width="100%"
                  Fill="#E8F5E9"
                  LineWidth="2">
        <SparklineBorder Color="#4CAF50" Width="2">
        </SparklineBorder>
        <SparklineContainerArea Background="transparent">
        </SparklineContainerArea>
        <SparklinePadding Top="10" Bottom="5" Left="0" Right="0">
        </SparklinePadding>
        <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.End }"
                                 Size="6"
                                 Fill="#4CAF50">
        </SparklineMarkerSettings>
    </SfSparkline>
</div>

@code {
    public double[] RevenueData = new double[] 
    { 
        385000, 412000, 398000, 445000, 428000, 458230 
    };
}
```

### Bordered Panel Style

Create a bordered panel using the `Type` property for line charts, the `Height` and `Width` properties for sizing, the `LineWidth` property for thickness, the `Fill` property for color, the `SparklineContainerArea` component with `Background` property, the `SparklineContainerAreaBorder` component with `Color` and `Width` properties, the `SparklinePadding` component for spacing, the `SparklineMarkerSettings` component with `Visible` property, and the `SparklineAxisSettings` component for axis customization:

```razor
@using Syncfusion.Blazor.Charts

<div style="background: #f5f5f5; padding: 30px;">
    <h3 style="margin-bottom: 20px; color: #333;">Performance Metrics</h3>
    <SfSparkline DataSource="@PerformanceData" 
                  Type="SparklineType.Line" 
                  Height="180px" 
                  Width="600px"
                  LineWidth="3"
                  Fill="#FF5722">
        <SparklineContainerArea Background="#FFFFFF">
            <SparklineContainerAreaBorder Color="#E0E0E0" Width="1">
            </SparklineContainerAreaBorder>
        </SparklineContainerArea>
        <SparklinePadding Top="25" Bottom="25" Left="25" Right="25">
        </SparklinePadding>
        <SparklineMarkerSettings Visible="new List<VisibleType> 
                                          { 
                                              VisibleType.High, 
                                              VisibleType.Low 
                                          }"
                                 Size="8">
            <SparklineMarkerBorder Color="#FFFFFF" Width="2">
            </SparklineMarkerBorder>
        </SparklineMarkerSettings>
        <SparklineAxisSettings MinY="0" MaxY="100">
        </SparklineAxisSettings>
    </SfSparkline>
</div>

@code {
    public int[] PerformanceData = new int[] 
    { 
        65, 72, 68, 85, 78, 92, 88, 95, 87, 90 
    };
}
```

### Minimalist Design

Create a minimalist design using the `Type` property for line charts, the `Height` and `Width` properties for compact sizing, the `LineWidth` property set to 1 for subtle lines, the `Fill` property for color, and the `SparklineContainerArea` component with `Background` property set to "transparent":

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@MinimalData" 
              Type="SparklineType.Line" 
              Height="60px" 
              Width="200px"
              LineWidth="1"
              Fill="#9E9E9E">
    <SparklineContainerArea Background="transparent">
    </SparklineContainerArea>
</SfSparkline>

@code {
    public int[] MinimalData = new int[] { 3, 6, 4, 8, 5, 9, 7 };
}
```

## Best Practices

### Sizing Guidelines

**Small Sparklines (Inline):**
- Width: 100-200px
- Height: 40-60px
- Use: Table cells, inline metrics

**Medium Sparklines (Cards):**
- Width: 250-400px
- Height: 80-120px
- Use: Dashboard cards, metric panels

**Large Sparklines (Features):**
- Width: 450-600px
- Height: 150-200px
- Use: Hero sections, detailed charts

### Padding Recommendations

Apply padding recommendations based on component configuration using the `SparklinePadding` component with `Top`, `Bottom`, `Left`, and `Right` properties to accommodate data labels, markers, and minimal designs:

```razor
@using Syncfusion.Blazor.Charts

<!-- With Data Labels -->
<SfSparkline DataSource="@Data" Height="150px" Width="400px">
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }">
    </SparklineDataLabelSettings>
    <SparklinePadding Top="35" Bottom="15" Left="10" Right="10">
    </SparklinePadding>
</SfSparkline>

<!-- With Markers Only -->
<SfSparkline DataSource="@Data" Height="150px" Width="400px">
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }">
    </SparklineMarkerSettings>
    <SparklinePadding Top="15" Bottom="15" Left="10" Right="10">
    </SparklinePadding>
</SfSparkline>

<!-- Minimal -->
<SfSparkline DataSource="@Data" Height="80px" Width="200px">
    <SparklinePadding Top="5" Bottom="5" Left="5" Right="5">
    </SparklinePadding>
</SfSparkline>
```

### Line Width Selection

| Chart Size | Recommended LineWidth | Use Case |
|------------|----------------------|----------|
| Small (< 200px) | 1-2 | Subtle trends |
| Medium (200-400px) | 2-3 | Standard visibility |
| Large (> 400px) | 3-4 | Emphasis |

### Color Contrast

Achieve proper color contrast using the `Fill` property for the sparkline color and the `SparklineContainerArea` component with the `Background` property for the container color, following these guidelines:

**Background vs Fill:**
- Light background → Dark or vibrant fill
- Dark background → Light or bright fill
- Colored background → White or contrasting fill

```razor
@using Syncfusion.Blazor.Charts
<!-- Good Contrast Examples -->

<!-- Light on Dark -->
<SfSparkline DataSource="@Data" Height="100px" Width="300px"
              Fill="#FFFFFF" LineWidth="2">
    <SparklineContainerArea Background="#263238">
    </SparklineContainerArea>
</SfSparkline>

<!-- Dark on Light -->
<SfSparkline DataSource="@Data" Height="100px" Width="300px"
              Fill="#1976D2" LineWidth="2">
    <SparklineContainerArea Background="#FAFAFA">
    </SparklineContainerArea>
</SfSparkline>

<!-- Vibrant on Neutral -->
<SfSparkline DataSource="@Data" Height="100px" Width="300px"
              Fill="#FF5722" LineWidth="2">
    <SparklineContainerArea Background="#F5F5F5">
    </SparklineContainerArea>
</SfSparkline>
```

### Responsive Considerations

Create responsive designs using the `Width` property set to "100%" for fluid sizing and media queries to adjust the `Height` property based on screen size:

```razor
<!-- Desktop -->
@using Syncfusion.Blazor.Charts

<div class="sparkline-wrapper">
    <SfSparkline DataSource="@Data"
                 Width="100%"
                 Height="100%">
    </SfSparkline>
</div>

@code {
    private List<double> Data = new()
    {
        10, 25, 15, 30, 20, 35, 28
    };
}

<style>
@@media (min-width: 768px) {
    .sparkline-wrapper {
        width: 400px;
        height: 120px;
    }
}

@@media (min-width: 480px) and (max-width: 767px) {
    .sparkline-wrapper {
        width: 100%;
        height: 100px;
    }
}

@@media (max-width: 479px) {
    .sparkline-wrapper {
        width: 100%;
        height: 80px;
    }
}
</style>
```

### Accessibility

- Ensure sufficient color contrast using the `Fill` property and `SparklineContainerArea` with `Background` property (WCAG AA: 4.5:1)
- Use the `SparklineContainerAreaBorder` component with `Color` and `Width` properties to improve definition
- Add the `SparklinePadding` component with `Top`, `Bottom`, `Left`, and `Right` properties for better touch targets
- Provide alternative text descriptions through proper semantic HTML structure

**Proper dimensions and appearance styling using the `Width`, `Height`, `SparklinePadding`, `SparklineContainerAreaBorder`, `Fill`, `Opacity`, and `LineWidth` properties create professional, accessible, and visually appealing Sparkline charts that enhance data communication and user experience.**
