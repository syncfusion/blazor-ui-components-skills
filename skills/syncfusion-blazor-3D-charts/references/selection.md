# Selection for Blazor 3D Charts

This reference provides comprehensive guidance on configuring selection modes in Syncfusion Blazor 3D Charts, including point, series, and cluster selection with multiple selection support.

## Table of Contents

- [Overview](#overview)
- [Selection Modes](#selection-modes)
- [Multiple Selection](#multiple-selection)
- [Initial Selection](#initial-selection)
- [Legend Selection](#legend-selection)
- [Best Practices](#best-practices)

## Overview

Selection allows users to interact with chart data by clicking on data points, series, or clusters. The `SelectionMode` property controls which selection type is enabled (Point, Series, or Cluster). This feature is useful for filtering, highlighting, and detailed data exploration.

## Selection Modes

### Point Selection

Select individual data points using the `SelectionMode` property set to `Point`. This allows users to click on individual data points for detailed analysis.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Point Selection" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100" 
           SelectionMode="Syncfusion.Blazor.Chart3D.SelectionMode.Point">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData" XName="Month" YName="Sales" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class MonthlySales
    {
        public string Month { get; set; }
        public double Sales { get; set; }
    }

    public List<MonthlySales> SalesData = new List<MonthlySales>
    {
        new MonthlySales { Month = "Jan", Sales = 35 },
        new MonthlySales { Month = "Feb", Sales = 28 },
        new MonthlySales { Month = "Mar", Sales = 34 },
        new MonthlySales { Month = "Apr", Sales = 32 },
        new MonthlySales { Month = "May", Sales = 40 },
        new MonthlySales { Month = "Jun", Sales = 32 }
    };
}
```

### Series Selection

Select entire series using the `SelectionMode` property set to `Series`. This enables users to select complete series for comparison across multiple data series.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Series Selection" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100" 
           SelectionMode="Syncfusion.Blazor.Chart3D.SelectionMode.Series">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ComparisonList" Name="Product A" XName="Month" 
                       YName="ValueA" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@ComparisonList" Name="Product B" XName="Month" 
                       YName="ValueB" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@ComparisonList" Name="Product C" XName="Month" 
                       YName="ValueC" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ComparisonData
    {
        public string Month { get; set; }
        public double ValueA { get; set; }
        public double ValueB { get; set; }
        public double ValueC { get; set; }
    }

    public List<ComparisonData> ComparisonList = new List<ComparisonData>
    {
        new ComparisonData { Month = "Jan", ValueA = 35, ValueB = 28, ValueC = 34 },
        new ComparisonData { Month = "Feb", ValueA = 32, ValueB = 27, ValueC = 35 },
        new ComparisonData { Month = "Mar", ValueA = 34, ValueB = 30, ValueC = 38 }
    };
}
```

### Cluster Selection

Select all points in a cluster (category) using the `SelectionMode` property set to `Cluster`. This allows selecting all data points within a specific category across multiple series.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Cluster Selection" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100" 
           SelectionMode="Syncfusion.Blazor.Chart3D.SelectionMode.Cluster">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ClusterPoints" Name="Sales" XName="Region" 
                       YName="Sales" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@ClusterPoints" Name="Marketing" XName="Region" 
                       YName="Marketing" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@ClusterPoints" Name="Support" XName="Region" 
                       YName="Support" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ClusterData
    {
        public string Region { get; set; }
        public double Sales { get; set; }
        public double Marketing { get; set; }
        public double Support { get; set; }
    }

    public List<ClusterData> ClusterPoints = new List<ClusterData>
    {
        new ClusterData { Region = "North", Sales = 120, Marketing = 85, Support = 95 },
        new ClusterData { Region = "South", Sales = 145, Marketing = 92, Support = 105 },
        new ClusterData { Region = "East", Sales = 165, Marketing = 78, Support = 88 },
        new ClusterData { Region = "West", Sales = 135, Marketing = 88, Support = 92 }
    };
}
```

## Multiple Selection

Enable selection of multiple items by setting the `AllowMultiSelection` property to `true`. This allows users to select multiple data points, series, or clusters simultaneously.

### Multiple Point Selection

Select multiple individual data points using `SelectionMode="Point"` combined with `AllowMultiSelection="true"`.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Multiple Point Selection" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100" 
           SelectionMode="Syncfusion.Blazor.Chart3D.SelectionMode.Point" 
           AllowMultiSelection="true">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ProductData" XName="Product" YName="Units" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ProductInfo
    {
        public string Product { get; set; }
        public double Units { get; set; }
    }

    public List<ProductInfo> ProductData = new List<ProductInfo>
    {
        new ProductInfo { Product = "Laptop", Units = 450 },
        new ProductInfo { Product = "Phone", Units = 620 },
        new ProductInfo { Product = "Tablet", Units = 380 },
        new ProductInfo { Product = "Monitor", Units = 290 },
        new ProductInfo { Product = "Keyboard", Units = 510 }
    };
}
```

### Multiple Series Selection

Select multiple series using `SelectionMode="Series"` combined with `AllowMultiSelection="true"`. This allows users to compare and analyze multiple series at once.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Multiple Series Selection" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100" 
           SelectionMode="Syncfusion.Blazor.Chart3D.SelectionMode.Series" 
           AllowMultiSelection="true">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MultiData" Name="Q1" XName="Department" 
                       YName="Q1" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@MultiData" Name="Q2" XName="Department" 
                       YName="Q2" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@MultiData" Name="Q3" XName="Department" 
                       YName="Q3" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@MultiData" Name="Q4" XName="Department" 
                       YName="Q4" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class QuarterlyData
    {
        public string Department { get; set; }
        public double Q1 { get; set; }
        public double Q2 { get; set; }
        public double Q3 { get; set; }
        public double Q4 { get; set; }
    }

    public List<QuarterlyData> MultiData = new List<QuarterlyData>
    {
        new QuarterlyData { Department = "Sales", Q1 = 85, Q2 = 92, Q3 = 88, Q4 = 95 },
        new QuarterlyData { Department = "Marketing", Q1 = 72, Q2 = 78, Q3 = 82, Q4 = 80 }
    };
}
```

## Initial Selection

Pre-select data points using the `Chart3DSelectedDataIndexes` property to specify which points should be initially selected when the chart loads.

### Single Point Pre-selection

Pre-select a single data point by specifying the series index and point index in `Chart3DSelectedDataIndex` with `Series` and `Point` properties.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Initial Point Selected" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100" 
           SelectionMode="Syncfusion.Blazor.Chart3D.SelectionMode.Point">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@InitialData" XName="Category" YName="Value" 
                       Type="Chart3DSeriesType.Column">
            <Chart3DSelectedDataIndexes>
                <Chart3DSelectedDataIndex Series="0" Point="2"></Chart3DSelectedDataIndex>
            </Chart3DSelectedDataIndexes>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class InitialDataPoint
    {
        public string Category { get; set; }
        public double Value { get; set; }
    }

    public List<InitialDataPoint> InitialData = new List<InitialDataPoint>
    {
        new InitialDataPoint { Category = "A", Value = 45 },
        new InitialDataPoint { Category = "B", Value = 62 },
        new InitialDataPoint { Category = "C", Value = 58 },
        new InitialDataPoint { Category = "D", Value = 70 }
    };
}
```

### Multiple Points Pre-selection

Pre-select multiple data points by adding multiple `Chart3DSelectedDataIndex` elements with different `Series` and `Point` property values.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Multiple Points Pre-selected" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100" 
           SelectionMode="Syncfusion.Blazor.Chart3D.SelectionMode.Point" 
           AllowMultiSelection="true">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@PreSelectList" XName="Month" YName="Revenue" 
                       Type="Chart3DSeriesType.Column">
            <Chart3DSelectedDataIndexes>
                <Chart3DSelectedDataIndex Series="0" Point="1"></Chart3DSelectedDataIndex>
                <Chart3DSelectedDataIndex Series="0" Point="3"></Chart3DSelectedDataIndex>
                <Chart3DSelectedDataIndex Series="0" Point="5"></Chart3DSelectedDataIndex>
            </Chart3DSelectedDataIndexes>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class PreSelectData
    {
        public string Month { get; set; }
        public double Revenue { get; set; }
    }

    public List<PreSelectData> PreSelectList = new List<PreSelectData>
    {
        new PreSelectData { Month = "Jan", Revenue = 125 },
        new PreSelectData { Month = "Feb", Revenue = 145 },
        new PreSelectData { Month = "Mar", Revenue = 165 },
        new PreSelectData { Month = "Apr", Revenue = 185 },
        new PreSelectData { Month = "May", Revenue = 195 },
        new PreSelectData { Month = "Jun", Revenue = 175 }
    };
}
```

## Legend Selection

Enable series selection through legend interaction by setting the `Chart3DLegendSettings` properties `ToggleVisibility` to `true` and `EnableHighlight` to `true`. This allows users to toggle series visibility and highlight series from the legend.

```cshtml
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Legend Toggle Selection" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100" 
           SelectionMode="Syncfusion.Blazor.Chart3D.SelectionMode.Series">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DLegendSettings Visible="true" ToggleVisibility="true" EnableHighlight="true">
    </Chart3DLegendSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@LegendData" Name="Desktop" XName="Year" 
                       YName="Desktop" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@LegendData" Name="Mobile" XName="Year" 
                       YName="Mobile" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@LegendData" Name="Tablet" XName="Year" 
                       YName="Tablet" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class DeviceData
    {
        public string Year { get; set; }
        public double Desktop { get; set; }
        public double Mobile { get; set; }
        public double Tablet { get; set; }
    }

    public List<DeviceData> LegendData = new List<DeviceData>
    {
        new DeviceData { Year = "2020", Desktop = 450, Mobile = 620, Tablet = 380 },
        new DeviceData { Year = "2021", Desktop = 480, Mobile = 680, Tablet = 420 },
        new DeviceData { Year = "2022", Desktop = 510, Mobile = 740, Tablet = 450 },
        new DeviceData { Year = "2023", Desktop = 540, Mobile = 800, Tablet = 490 }
    };
}
```

## Best Practices

1. **Selection Mode Choice**:
   - Use **Point** for individual data point analysis
   - Use **Series** for comparing entire series
   - Use **Cluster** for category-based comparisons
   - Match selection mode to user's analytical needs

2. **Multiple Selection**:
   - Enable for comparative analysis
   - Provide clear visual feedback for selected items
   - Consider adding "Clear Selection" button
   - Document Ctrl/Cmd+click behavior for users

3. **Initial Selection**:
   - Pre-select to highlight important data
   - Use sparingly to avoid overwhelming users
   - Ensure pre-selected items are visible
   - Document why items are pre-selected

4. **Legend Selection**:
   - Combine with `EnableHighlight` for better UX
   - Use for series filtering
   - Provide clear legend labels
   - Test with many series (>5)

5. **Visual Feedback**:
   - Ensure selected items are clearly visible
   - Use consistent selection colors
   - Maintain contrast with chart colors
   - Test visibility with different backgrounds

6. **Performance**:
   - Test selection with large datasets
   - Multiple selection may impact performance
   - Consider debouncing rapid selections
   - Monitor re-render performance

7. **User Experience**:
   - Provide clear instructions for selection
   - Show selected item details (tooltip, panel)
   - Allow deselection (click again)
   - Support keyboard navigation where possible

8. **Accessibility**:
   - Provide keyboard alternatives to mouse selection
   - Announce selections to screen readers
   - Maintain focus indicators
   - Ensure sufficient color contrast

9. **Integration**:
   - Wire selection events to update application state
   - Sync selection with other UI components
   - Persist selection across chart updates
   - Handle selection in data binding scenarios

10. **Testing**:
    - Test all selection modes
    - Verify multi-selection behavior
    - Test pre-selection with dynamic data
    - Validate selection with animations
    - Test on touch devices
