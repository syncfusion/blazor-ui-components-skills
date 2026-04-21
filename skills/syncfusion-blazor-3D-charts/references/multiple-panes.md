# Multiple Panes for Blazor 3D Charts

This reference provides comprehensive guidance on configuring multiple panes in Syncfusion Blazor 3D Charts using rows and columns for split chart layouts.

## Table of Contents

- [Overview](#overview)
- [Rows Configuration](#rows-configuration)
- [Columns Configuration](#columns-configuration)
- [Combined Rows and Columns](#combined-rows-and-columns)
- [Axis Association](#axis-association)
- [Spanning Panes](#spanning-panes)
- [Best Practices](#best-practices)

## Overview

Multiple panes allow you to split the chart area into rows and columns, enabling side-by-side or stacked comparisons of different data series with independent axes.

## Rows Configuration

Split chart vertically into multiple rows using `Chart3DRows` component. The `Height` property on `Chart3DRow` will be used to define row dimensions, and the `RowIndex` property on `Chart3DAxis` will be used to associate axes with specific rows. The `YAxisName` property on `Chart3DSeries` will be used to bind series to designated row axes.

### Two Rows

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales vs Profit" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DRows>
        <Chart3DRow Height="50%"></Chart3DRow>
        <Chart3DRow Height="50%"></Chart3DRow>
    </Chart3DRows>
    
    <Chart3DAxes>
        <Chart3DAxis RowIndex="1" Name="yAxis1">
        </Chart3DAxis>
    </Chart3DAxes>
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@BusinessData" Name="Sales" XName="Month" 
                       YName="Sales" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@BusinessData" Name="Profit" XName="Month" 
                       YName="Profit" YAxisName="yAxis1" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class BusinessMetrics
    {
        public string Month { get; set; }
        public double Sales { get; set; }
        public double Profit { get; set; }
    }

    public List<BusinessMetrics> BusinessData = new List<BusinessMetrics>
    {
        new BusinessMetrics { Month = "Jan", Sales = 120000, Profit = 12000 },
        new BusinessMetrics { Month = "Feb", Sales = 145000, Profit = 15500 },
        new BusinessMetrics { Month = "Mar", Sales = 165000, Profit = 18000 },
        new BusinessMetrics { Month = "Apr", Sales = 185000, Profit = 20500 }
    };
}
```

### Three Rows

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Regional Performance" Width="900px" Height="700px" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DRows>
        <Chart3DRow Height="33%"></Chart3DRow>
        <Chart3DRow Height="33%"></Chart3DRow>
        <Chart3DRow Height="34%"></Chart3DRow>
    </Chart3DRows>
    
    <Chart3DAxes>
        <Chart3DAxis RowIndex="1" Name="yAxis1">
        </Chart3DAxis>
        <Chart3DAxis RowIndex="2" Name="yAxis2">
        </Chart3DAxis>
    </Chart3DAxes>
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@RegionalData" Name="North" XName="Quarter" 
                       YName="North"
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@RegionalData" Name="South" XName="Quarter" 
                       YName="South" YAxisName="yAxis1" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@RegionalData" Name="East" XName="Quarter" 
                       YName="East" YAxisName="yAxis2" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class RegionalSales
    {
        public string Quarter { get; set; }
        public double North { get; set; }
        public double South { get; set; }
        public double East { get; set; }
    }

    public List<RegionalSales> RegionalData = new List<RegionalSales>
    {
        new RegionalSales { Quarter = "Q1", North = 85, South = 92, East = 78 },
        new RegionalSales { Quarter = "Q2", North = 90, South = 88, East = 82 },
        new RegionalSales { Quarter = "Q3", North = 88, South = 95, East = 85 }
    };
}
```

## Columns Configuration

Split chart horizontally into multiple columns using `Chart3DColumns` component. The `Width` property on `Chart3DColumn` will be used to define column dimensions, and the `ColumnIndex` property on `Chart3DAxis` will be used to associate axes with specific columns. The `XAxisName` property on `Chart3DSeries` will be used to bind series to designated column axes.

### Two Columns

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Product Comparison" Width="1000px" Height="500px" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DColumns>
        <Chart3DColumn Width="50%"></Chart3DColumn>
        <Chart3DColumn Width="50%"></Chart3DColumn>
    </Chart3DColumns>
    
    <Chart3DAxes>
        <Chart3DAxis ColumnIndex="1" Name="xAxis1" 
                     ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
        </Chart3DAxis>
    </Chart3DAxes>
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DPrimaryYAxis>
    </Chart3DPrimaryYAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ProductA" Name="Product A" XName="Month" 
                       YName="Sales" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@ProductB" Name="Product B" XName="Month" 
                       YName="Sales" XAxisName="xAxis1" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ProductSales
    {
        public string Month { get; set; }
        public double Sales { get; set; }
    }

    public List<ProductSales> ProductA = new List<ProductSales>
    {
        new ProductSales { Month = "Jan", Sales = 120 },
        new ProductSales { Month = "Feb", Sales = 145 },
        new ProductSales { Month = "Mar", Sales = 165 }
    };

    public List<ProductSales> ProductB = new List<ProductSales>
    {
        new ProductSales { Month = "Jan", Sales = 95 },
        new ProductSales { Month = "Feb", Sales = 110 },
        new ProductSales { Month = "Mar", Sales = 130 }
    };
}
```

### Three Columns

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Department Analysis" Width="1200px" Height="500px" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DColumns>
        <Chart3DColumn Width="33%"></Chart3DColumn>
        <Chart3DColumn Width="33%"></Chart3DColumn>
        <Chart3DColumn Width="34%"></Chart3DColumn>
    </Chart3DColumns>
    
    <Chart3DAxes>
        <Chart3DAxis ColumnIndex="1" Name="xAxis1" 
                     ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
        </Chart3DAxis>
        <Chart3DAxis ColumnIndex="2" Name="xAxis2" 
                     ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
        </Chart3DAxis>
    </Chart3DAxes>
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DPrimaryYAxis>
    </Chart3DPrimaryYAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData" Name="Sales" XName="Period" 
                       YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@MarketingData" Name="Marketing" XName="Period" 
                       YName="Value" XAxisName="xAxis1" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@SupportData" Name="Support" XName="Period" 
                       YName="Value" XAxisName="xAxis2" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class DepartmentData
    {
        public string Period { get; set; }
        public double Value { get; set; }
    }

    public List<DepartmentData> SalesData = new List<DepartmentData>
    {
        new DepartmentData { Period = "Q1", Value = 85 },
        new DepartmentData { Period = "Q2", Value = 92 }
    };

    public List<DepartmentData> MarketingData = new List<DepartmentData>
    {
        new DepartmentData { Period = "Q1", Value = 72 },
        new DepartmentData { Period = "Q2", Value = 78 }
    };

    public List<DepartmentData> SupportData = new List<DepartmentData>
    {
        new DepartmentData { Period = "Q1", Value = 68 },
        new DepartmentData { Period = "Q2", Value = 75 }
    };
}
```

## Combined Rows and Columns

Create grid layout using both `Chart3DRows` and `Chart3DColumns` components together. The `RowIndex` and `ColumnIndex` properties on `Chart3DAxis` will be used to position axes at specific grid intersections, and the `YAxisName` and `XAxisName` properties on `Chart3DSeries` will be used to associate series with the appropriate grid pane axes.

### 2x2 Grid

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Quarterly Performance Grid"
           Width="1000px"
           Height="700px"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="7"
           TiltAngle="10"
           Depth="100">

    <!-- GRID: 2 Rows × 2 Columns -->
    <Chart3DRows>
        <Chart3DRow Height="50%"></Chart3DRow>
        <Chart3DRow Height="50%"></Chart3DRow>
    </Chart3DRows>

    <Chart3DColumns>
        <Chart3DColumn Width="50%"></Chart3DColumn>
        <Chart3DColumn Width="50%"></Chart3DColumn>
    </Chart3DColumns>

    <!-- Y‑AXES for each pane -->
    <Chart3DAxes>

        <!-- Pane (0,1) -->
        <Chart3DAxis Name="yAxis2" RowIndex="0" ColumnIndex="1"></Chart3DAxis>

        <!-- Pane (1,0) -->
        <Chart3DAxis Name="yAxis3" RowIndex="1" ColumnIndex="0"></Chart3DAxis>

        <!-- Pane (1,1) -->
        <Chart3DAxis Name="yAxis4" RowIndex="1" ColumnIndex="1"></Chart3DAxis>

    </Chart3DAxes>

    <!-- Only ONE X-axis allowed in Chart3D -->
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"></Chart3DPrimaryXAxis>

    <!-- Only ONE primary Y-axis allowed (unused for multipane) -->
    <Chart3DPrimaryYAxis></Chart3DPrimaryYAxis>

    <Chart3DSeriesCollection>

        <!-- Q1 → Pane (0,0) -->
        <Chart3DSeries DataSource="@Q1Data"
                       Name="Q1"
                       XName="Month"
                       YName="Value"
                       Type="Syncfusion.Blazor.Chart3D.Chart3DSeriesType.Column">
        </Chart3DSeries>

        <!-- Q2 → Pane (0,1) -->
        <Chart3DSeries DataSource="@Q2Data"
                       Name="Q2"
                       XName="Month"
                       YName="Value"
                       YAxisName="yAxis2"
                       Type="Syncfusion.Blazor.Chart3D.Chart3DSeriesType.Column">
        </Chart3DSeries>

        <!-- Q3 → Pane (1,0) -->
        <Chart3DSeries DataSource="@Q3Data"
                       Name="Q3"
                       XName="Month"
                       YName="Value"
                       YAxisName="yAxis3"
                       Type="Syncfusion.Blazor.Chart3D.Chart3DSeriesType.Column">
        </Chart3DSeries>

        <!-- Q4 → Pane (1,1) -->
        <Chart3DSeries DataSource="@Q4Data"
                       Name="Q4"
                       XName="Month"
                       YName="Value"
                       YAxisName="yAxis4"
                       Type="Syncfusion.Blazor.Chart3D.Chart3DSeriesType.Column">
        </Chart3DSeries>

    </Chart3DSeriesCollection>

</SfChart3D>

@code {
    public class QuarterData
    {
        public string Month { get; set; }
        public double Value { get; set; }
    }

    public List<QuarterData> Q1Data = new()
    {
        new QuarterData { Month = "Jan", Value = 85 },
        new QuarterData { Month = "Feb", Value = 92 },
        new QuarterData { Month = "Mar", Value = 88 }
    };

    public List<QuarterData> Q2Data = new()
    {
        new QuarterData { Month = "Apr", Value = 90 },
        new QuarterData { Month = "May", Value = 95 },
        new QuarterData { Month = "Jun", Value = 92 }
    };

    public List<QuarterData> Q3Data = new()
    {
        new QuarterData { Month = "Jul", Value = 88 },
        new QuarterData { Month = "Aug", Value = 94 },
        new QuarterData { Month = "Sep", Value = 90 }
    };

    public List<QuarterData> Q4Data = new()
    {
        new QuarterData { Month = "Oct", Value = 96 },
        new QuarterData { Month = "Nov", Value = 100 },
        new QuarterData { Month = "Dec", Value = 105 }
    };
}
```

## Axis Association

Associate series with specific axes in panes using axis properties. The `RowIndex` property on primary and secondary `Chart3DAxis` will be used to position axes in specific rows, and the `YAxisName` property on `Chart3DSeries` will be used to bind series data to these designated axes within their panes.

### Row-Based Axis Association

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Temperature vs Humidity" Width="900px" Height="600px" 
           WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DRows>
        <Chart3DRow Height="50%"></Chart3DRow>
        <Chart3DRow Height="50%"></Chart3DRow>
    </Chart3DRows>
    
    <Chart3DAxes>
        <Chart3DAxis RowIndex="1" Name="yAxis1">
            <Chart3DMajorGridLines Width="0"></Chart3DMajorGridLines>
        </Chart3DAxis>
    </Chart3DAxes>
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DPrimaryYAxis RowIndex="0">
        <Chart3DMajorGridLines Width="0"></Chart3DMajorGridLines>
    </Chart3DPrimaryYAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ClimateData" Name="Temperature (°C)" XName="City" 
                       YName="Temperature" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@ClimateData" Name="Humidity (%)" XName="City" 
                       YName="Humidity" YAxisName="yAxis1" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ClimateInfo
    {
        public string City { get; set; }
        public double Temperature { get; set; }
        public double Humidity { get; set; }
    }

    public List<ClimateInfo> ClimateData = new List<ClimateInfo>
    {
        new ClimateInfo { City = "Tokyo", Temperature = 28, Humidity = 65 },
        new ClimateInfo { City = "Berlin", Temperature = 22, Humidity = 55 },
        new ClimateInfo { City = "London", Temperature = 20, Humidity = 70 },
        new ClimateInfo { City = "Paris", Temperature = 24, Humidity = 60 }
    };
}
```

## Spanning Panes

Span series across multiple panes using the `Span` property on `Chart3DPrimaryYAxis` or `Chart3DAxis`. The `Span` property will be used to extend an axis across multiple rows or columns, allowing series to occupy larger pane areas for emphasis or comparison purposes.

### Row Spanning

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="3D Multi-Row Spanning Example"
           Width="900px" Height="700px"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="7"
           TiltAngle="10"
           Depth="100">

    <!-- Define 3 rows (pane layout) -->
    <Chart3DRows>
        <Chart3DRow Height="25%"></Chart3DRow>
        <Chart3DRow Height="50%"></Chart3DRow>
        <Chart3DRow Height="25%"></Chart3DRow>
    </Chart3DRows>

    <!-- Define Axes for Each Pane -->
    <Chart3DAxes>

        <!-- Middle Pane (Row 1), spanning Rows 1 and 2 -->
        <Chart3DAxis Name="yAxis1"
                     RowIndex="1"
                    >
        </Chart3DAxis>

        <!-- Bottom Pane (Row 2) -->
        <Chart3DAxis Name="yAxis2"
                     RowIndex="2">
        </Chart3DAxis>
    </Chart3DAxes>

    <!-- Default X Axis -->
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>

    <Chart3DPrimaryYAxis Span="2"></Chart3DPrimaryYAxis>

    <!-- Series -->
    <Chart3DSeriesCollection>

        <!-- Series 1 → Top row -->
        <Chart3DSeries DataSource="@SpanData"
                       Name="Series 1"
                       XName="X"
                       YName="Y1"
                       Type="Syncfusion.Blazor.Chart3D.Chart3DSeriesType.Column">
        </Chart3DSeries>

        <!-- Series 2 → Middle pane (row 1 + row 2 because of span) -->
        <Chart3DSeries DataSource="@SpanData"
                       Name="Series 2 (Spanning)"
                       XName="X"
                       YName="Y2"
                       YAxisName="yAxis1"
                       Type="Syncfusion.Blazor.Chart3D.Chart3DSeriesType.Column">
        </Chart3DSeries>

        <!-- Series 3 → Bottom row -->
        <Chart3DSeries DataSource="@SpanData"
                       Name="Series 3"
                       XName="X"
                       YName="Y3"
                       YAxisName="yAxis2"
                       Type="Syncfusion.Blazor.Chart3D.Chart3DSeriesType.Column">
        </Chart3DSeries>

    </Chart3DSeriesCollection>

</SfChart3D>

@code {

    public class SpanningData
    {
        public string X { get; set; }
        public double Y1 { get; set; }
        public double Y2 { get; set; }
        public double Y3 { get; set; }
    }

    public List<SpanningData> SpanData = new()
    {
        new SpanningData { X = "A", Y1 = 50,  Y2 = 120, Y3 = 40 },
        new SpanningData { X = "B", Y1 = 60,  Y2 = 145, Y3 = 45 },
        new SpanningData { X = "C", Y1 = 55,  Y2 = 165, Y3 = 42 }
    };
}
```

## Best Practices

1. **Layout Planning**:
   - Plan pane layout before implementation
   - Consider data relationships between panes
   - Use rows for vertical comparisons (time series)
   - Use columns for side-by-side comparisons
   - Balance pane sizes for optimal viewing

2. **Size Distribution**:
   - Use percentage-based widths/heights for responsiveness
   - Equal sizes work well for comparable data
   - Emphasize important data with larger panes
   - Leave adequate space for labels and legends

3. **Axis Configuration**:
   - Always associate axes with correct panes (RowIndex/ColumnIndex)
   - Use meaningful axis names (yAxis1, yAxis2, etc.)
   - Configure axis ranges independently per pane
   - Hide unnecessary grid lines for clarity

4. **Series Association**:
   - Explicitly set XAxisName and YAxisName for each series
   - Match axis names exactly (case-sensitive)
   - Verify series data renders in intended pane
   - Test with different data ranges

5. **Visual Consistency**:
   - Maintain consistent styling across panes
   - Use coordinated color schemes
   - Align axis labels and titles
   - Consider shared legends when appropriate

6. **Performance**:
   - Limit number of panes (typically ≤4 rows/columns)
   - Monitor rendering performance with multiple panes
   - Consider data density in each pane
   - Test with target data volumes

7. **Responsive Design**:
   - Test multi-pane layouts on various screen sizes
   - Consider single-pane alternative for mobile
   - Adjust chart dimensions for adequate viewing
   - Use percentage-based sizing

8. **User Experience**:
   - Provide clear titles for each pane/series
   - Use tooltips to show detailed information
   - Enable legends for series identification
   - Consider adding pane borders for separation

9. **Spanning**:
   - Use spanning for emphasis
   - Verify span values match layout
   - Test spanning with different data
   - Document spanning logic for maintainability

10. **Testing**:
    - Verify all series render in correct panes
    - Test axis associations thoroughly
    - Validate with edge cases (null values, extremes)
    - Check layout on different devices
    - Test interactive features (rotation, tooltips)
