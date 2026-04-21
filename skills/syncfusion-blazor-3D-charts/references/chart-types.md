# 3D Chart Types

Complete guide to all supported 3D chart types in Syncfusion Blazor 3D Chart component: Column, Bar, Stacked Column, and Stacked Bar charts with configuration options and examples.

## Table of Contents
- [Overview](#overview)
- [Column Chart](#column-chart)
- [Bar Chart](#bar-chart)
- [Stacked Column Chart](#stacked-column-chart)
- [Stacked Bar Chart](#stacked-bar-chart)
- [Column/Bar Spacing and Width](#columnbar-spacing-and-width)
- [Grouped Charts](#grouped-charts)
- [Cylindrical Charts](#cylindrical-charts)
- [Series Customization](#series-customization)
- [Choosing the Right Chart Type](#choosing-the-right-chart-type)

## Overview

The Syncfusion Blazor 3D Chart component supports four chart types:

| Chart Type | Use Case | Series Type Enum |
|------------|----------|------------------|
| **Column** | Vertical bars for comparing categories | `Chart3DSeriesType.Column` |
| **Bar** | Horizontal bars for better label readability | `Chart3DSeriesType.Bar` |
| **Stacked Column** | Vertical stacked bars showing part-to-whole | `Chart3DSeriesType.StackingColumn` |
| **Stacked Bar** | Horizontal stacked bars for composition | `Chart3DSeriesType.StackingBar` |

All chart types support:
- Multiple series
- Custom colors and opacity
- Data labels, legends, and tooltips
- Cylindrical column facets
- Grouped visualization
- Column spacing and width customization

## Column Chart

Column charts display data as vertical 3D bars, ideal for comparing values across categories. Use the `Type` property set to `Chart3DSeriesType.Column` to create column charts.

### Basic Column Chart

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MedalDetails" 
                       XName="Country" 
                       YName="Gold" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class Chart3DData
    {
        public string Country { get; set; }
        public double Gold { get; set; }
    }

    public List<Chart3DData> MedalDetails = new List<Chart3DData>
    {
        new Chart3DData { Country = "USA", Gold = 61.3 },
        new Chart3DData { Country = "Pakistan", Gold = 20.4 },
        new Chart3DData { Country = "Germany", Gold = 65.1 },
        new Chart3DData { Country = "Australia", Gold = 15.8 },
        new Chart3DData { Country = "Italy", Gold = 29.2 },
        new Chart3DData { Country = "India", Gold = 44.6 },
        new Chart3DData { Country = "Russia", Gold = 40.8 },
        new Chart3DData { Country = "Mexico", Gold = 31.0 },
        new Chart3DData { Country = "Brazil", Gold = 75.9 },
        new Chart3DData { Country = "China", Gold = 51.4 }
    };
}
```

### Multiple Series Column Chart

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Olympic Medal Comparison" 
           WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MedalDetails" 
                       Name="Gold" 
                       XName="Country" 
                       YName="Gold" 
                       Type="Chart3DSeriesType.Column"
                       Fill="#FFD700">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@MedalDetails" 
                       Name="Silver" 
                       XName="Country" 
                       YName="Silver" 
                       Type="Chart3DSeriesType.Column"
                       Fill="#C0C0C0">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@MedalDetails" 
                       Name="Bronze" 
                       XName="Country" 
                       YName="Bronze" 
                       Type="Chart3DSeriesType.Column"
                       Fill="#CD7F32">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class Chart3DData
    {
        public string Country { get; set; }
        public double Gold { get; set; }
        public double Silver { get; set; }
        public double Bronze { get; set; }
    }

    public List<Chart3DData> MedalDetails = new List<Chart3DData>
    {
        new Chart3DData { Country = "USA", Gold = 50, Silver = 70, Bronze = 45 },
        new Chart3DData { Country = "China", Gold = 40, Silver = 60, Bronze = 55 },
        new Chart3DData { Country = "Japan", Gold = 70, Silver = 60, Bronze = 50 },
        new Chart3DData { Country = "Australia", Gold = 60, Silver = 56, Bronze = 40 },
        new Chart3DData { Country = "France", Gold = 50, Silver = 45, Bronze = 35 },
        new Chart3DData { Country = "Germany", Gold = 40, Silver = 30, Bronze = 22 },
        new Chart3DData { Country = "Italy", Gold = 40, Silver = 35, Bronze = 37 },
        new Chart3DData { Country = "Sweden", Gold = 30, Silver = 25, Bronze = 27 }
    };
}
```

## Bar Chart

Bar charts display data as horizontal 3D bars, useful when category labels are long or when comparing horizontal metrics. Use the `Type` property set to `Chart3DSeriesType.Bar` to create bar charts.

### Basic Bar Chart

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Olympic Medals" 
           WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MedalDetails" 
                       XName="Country" 
                       YName="Gold" 
                       Type="Chart3DSeriesType.Bar">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class Chart3DData
    {
        public string Country { get; set; }
        public double Gold { get; set; }
    }

    public List<Chart3DData> MedalDetails = new List<Chart3DData>
    {
        new Chart3DData { Country = "USA", Gold = 50 },
        new Chart3DData { Country = "China", Gold = 40 },
        new Chart3DData { Country = "Japan", Gold = 70 },
        new Chart3DData { Country = "Australia", Gold = 60 },
        new Chart3DData { Country = "France", Gold = 50 },
        new Chart3DData { Country = "Germany", Gold = 40 },
        new Chart3DData { Country = "Italy", Gold = 40 },
        new Chart3DData { Country = "Sweden", Gold = 30 }
    };
}
```

### Multiple Series Bar Chart

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Medal Distribution" 
           WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MedalDetails" 
                       Name="Gold" 
                       XName="Country" 
                       YName="Gold" 
                       Type="Chart3DSeriesType.Bar">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@MedalDetails" 
                       Name="Silver" 
                       XName="Country" 
                       YName="Silver" 
                       Type="Chart3DSeriesType.Bar">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class Chart3DData
    {
        public string Country { get; set; }
        public double Gold { get; set; }
        public double Silver { get; set; }
    }

    public List<Chart3DData> MedalDetails = new List<Chart3DData>
    {
        new Chart3DData { Country = "USA", Gold = 50, Silver = 70 },
        new Chart3DData { Country = "China", Gold = 40, Silver = 60 },
        new Chart3DData { Country = "Japan", Gold = 70, Silver = 60 },
        new Chart3DData { Country = "Australia", Gold = 60, Silver = 56 },
        new Chart3DData { Country = "France", Gold = 50, Silver = 45 },
        new Chart3DData { Country = "Germany", Gold = 40, Silver = 30 },
        new Chart3DData { Country = "Italy", Gold = 40, Silver = 35 },
        new Chart3DData { Country = "Sweden", Gold = 30, Silver = 25 }
    };
}
```

## Stacked Column Chart

Stacked column charts display multiple series stacked vertically, showing both individual values and totals. Use the `Type` property set to `Chart3DSeriesType.StackingColumn` to create stacked column charts. Use the `StackingGroup` property to create separate stacks.

### Basic Stacked Column Chart

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Medal Breakdown" 
           WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MedalDetails" 
                       Name="Gold" 
                       XName="Country" 
                       YName="Gold" 
                       Type="Chart3DSeriesType.StackingColumn">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@MedalDetails" 
                       Name="Silver" 
                       XName="Country" 
                       YName="Silver" 
                       Type="Chart3DSeriesType.StackingColumn">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@MedalDetails" 
                       Name="Bronze" 
                       XName="Country" 
                       YName="Bronze" 
                       Type="Chart3DSeriesType.StackingColumn">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class Chart3DData
    {
        public string Country { get; set; }
        public double Gold { get; set; }
        public double Silver { get; set; }
        public double Bronze { get; set; }
    }

    public List<Chart3DData> MedalDetails = new List<Chart3DData>
    {
        new Chart3DData { Country = "USA", Gold = 46, Silver = 56, Bronze = 26 },
        new Chart3DData { Country = "GBR", Gold = 27, Silver = 17, Bronze = 37 },
        new Chart3DData { Country = "CHN", Gold = 26, Silver = 36, Bronze = 56 },
        new Chart3DData { Country = "RUS", Gold = 56, Silver = 16, Bronze = 36 },
        new Chart3DData { Country = "AUS", Gold = 12, Silver = 46, Bronze = 26 },
        new Chart3DData { Country = "IND", Gold = 26, Silver = 16, Bronze = 76 },
        new Chart3DData { Country = "DEN", Gold = 26, Silver = 12, Bronze = 42 },
        new Chart3DData { Country = "MEX", Gold = 34, Silver = 32, Bronze = 82 }
    };
}
```

### Stacked Column with Groups

Use `StackingGroup` to create separate stacks:

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>

    <Chart3DSeriesCollection>
        <!-- Group 1 -->
        <Chart3DSeries DataSource="@DataSource" 
                       StackingGroup="Team A" 
                       Name="Team A - Gold" 
                       XName="Year" 
                       YName="TeamAGold" 
                       Type="Chart3DSeriesType.StackingColumn">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@DataSource" 
                       StackingGroup="Team A" 
                       Name="Team A - Silver" 
                       XName="Year" 
                       YName="TeamASilver" 
                       Type="Chart3DSeriesType.StackingColumn">
        </Chart3DSeries>
        
        <!-- Group 2 -->
        <Chart3DSeries DataSource="@DataSource" 
                       StackingGroup="Team B" 
                       Name="Team B - Gold" 
                       XName="Year" 
                       YName="TeamBGold" 
                       Type="Chart3DSeriesType.StackingColumn">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@DataSource" 
                       StackingGroup="Team B" 
                       Name="Team B - Silver" 
                       XName="Year" 
                       YName="TeamBSilver" 
                       Type="Chart3DSeriesType.StackingColumn">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class Chart3DData
    {
        public string Year { get; set; }
        public double TeamAGold { get; set; }
        public double TeamASilver { get; set; }
        public double TeamBGold { get; set; }
        public double TeamBSilver { get; set; }
    }

    public List<Chart3DData> DataSource = new List<Chart3DData>
    {
        new Chart3DData { Year = "2012", TeamAGold = 46, TeamASilver = 56, TeamBGold = 29, TeamBSilver = 38 },
        new Chart3DData { Year = "2016", TeamAGold = 46, TeamASilver = 27, TeamBGold = 27, TeamBSilver = 26 },
        new Chart3DData { Year = "2020", TeamAGold = 39, TeamASilver = 22, TeamBGold = 22, TeamBSilver = 38 }
    };
}
```

## Stacked Bar Chart

Stacked bar charts are horizontal versions of stacked column charts, useful for long category labels. Use the `Type` property set to `Chart3DSeriesType.StackingBar` to create stacked bar charts.

### Basic Stacked Bar Chart

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Team Performance" 
           WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MedalDetails" 
                       Name="Gold" 
                       XName="Country" 
                       YName="Gold" 
                       Type="Chart3DSeriesType.StackingBar">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@MedalDetails" 
                       Name="Silver" 
                       XName="Country" 
                       YName="Silver" 
                       Type="Chart3DSeriesType.StackingBar">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@MedalDetails" 
                       Name="Bronze" 
                       XName="Country" 
                       YName="Bronze" 
                       Type="Chart3DSeriesType.StackingBar">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class Chart3DData
    {
        public string Country { get; set; }
        public double Gold { get; set; }
        public double Silver { get; set; }
        public double Bronze { get; set; }
    }

    public List<Chart3DData> MedalDetails = new List<Chart3DData>
    {
        new Chart3DData { Country = "USA", Gold = 46, Silver = 56, Bronze = 26 },
        new Chart3DData { Country = "GBR", Gold = 27, Silver = 17, Bronze = 37 },
        new Chart3DData { Country = "CHN", Gold = 26, Silver = 36, Bronze = 56 },
        new Chart3DData { Country = "RUS", Gold = 56, Silver = 16, Bronze = 36 },
        new Chart3DData { Country = "AUS", Gold = 12, Silver = 46, Bronze = 26 },
        new Chart3DData { Country = "IND", Gold = 26, Silver = 16, Bronze = 76 },
        new Chart3DData { Country = "DEN", Gold = 26, Silver = 12, Bronze = 42 },
        new Chart3DData { Country = "MEX", Gold = 34, Silver = 32, Bronze = 82 }
    };
}
```

## Column/Bar Spacing and Width

Customize the spacing between columns/bars and their width using the `ColumnSpacing` property to control space between columns and the `ColumnWidth` property to control column width.

### Column Spacing Configuration

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>

    <Chart3DSeriesCollection>
        <!-- Default spacing and width -->
        <Chart3DSeries DataSource="@MedalDetails" 
                       Name="Gold" 
                       XName="Country" 
                       YName="Gold" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        
        <!-- Custom spacing (0.5) and width (0.75) -->
        <Chart3DSeries DataSource="@MedalDetails" 
                       Name="Silver" 
                       XName="Country" 
                       YName="Silver" 
                       ColumnSpacing="0.5" 
                       ColumnWidth="0.75" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class Chart3DData
    {
        public string Country { get; set; }
        public double Gold { get; set; }
        public double Silver { get; set; }
    }

    public List<Chart3DData> MedalDetails = new List<Chart3DData>
    {
        new Chart3DData { Country = "USA", Gold = 50, Silver = 70 },
        new Chart3DData { Country = "China", Gold = 40, Silver = 60 },
        new Chart3DData { Country = "Japan", Gold = 70, Silver = 60 },
        new Chart3DData { Country = "Australia", Gold = 60, Silver = 56 },
        new Chart3DData { Country = "France", Gold = 50, Silver = 45 },
        new Chart3DData { Country = "Germany", Gold = 40, Silver = 30 },
        new Chart3DData { Country = "Italy", Gold = 40, Silver = 35 },
        new Chart3DData { Country = "Sweden", Gold = 30, Silver = 25 }
    };
}
```

**Properties:**
- **ColumnSpacing**: Space between columns (0 to 1, default 0)
  - `0` = No space
  - `0.5` = Moderate space
  - `1` = Maximum space
- **ColumnWidth**: Width of columns (0 to 1, default 0.7)
  - `0.5` = Half width
  - `1` = Full width

## Grouped Charts

Group data points by assigning the same `GroupName` property to related series. Works with both column and bar charts. Set the `EnableSideBySidePlacement` property to `false` to enable the grouping effect.

### Grouped Column Chart

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D EnableSideBySidePlacement="false" 
           RotationAngle="20" 
           Depth="300">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>

    <Chart3DSeriesCollection>
        <!-- USA Group -->
        <Chart3DSeries DataSource="@Chart3DPoints" 
                       XName="Year" 
                       YName="USA_Total" 
                       GroupName="USA" 
                       Name="USA Total"
                       ColumnWidth="0.2" 
                       Fill="#CB3587">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@Chart3DPoints" 
                       XName="Year" 
                       YName="USA_Gold" 
                       GroupName="USA" 
                       Name="USA Gold"
                       ColumnWidth="0.2">
        </Chart3DSeries>
        
        <!-- UK Group -->
        <Chart3DSeries DataSource="@Chart3DPoints" 
                       XName="Year" 
                       YName="UK_Total" 
                       GroupName="UK" 
                       Name="UK Total"
                       ColumnWidth="0.2">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@Chart3DPoints" 
                       XName="Year" 
                       YName="UK_Gold" 
                       GroupName="UK" 
                       Name="UK Gold"
                       ColumnWidth="0.2" 
                       Fill="#E7910F">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ColumnData
    {
        public string Year { get; set; }
        public double USA_Total { get; set; }
        public double USA_Gold { get; set; }
        public double UK_Total { get; set; }
        public double UK_Gold { get; set; }
    }

    public List<ColumnData> Chart3DPoints = new List<ColumnData>
    {
        new ColumnData { Year = "2012", USA_Total = 104, USA_Gold = 46, UK_Total = 65, UK_Gold = 29 },
        new ColumnData { Year = "2016", USA_Total = 121, USA_Gold = 46, UK_Total = 67, UK_Gold = 27 },
        new ColumnData { Year = "2020", USA_Total = 113, USA_Gold = 39, UK_Total = 65, UK_Gold = 22 }
    };
}
```

**Key Properties:**
- **GroupName**: Series with same name are grouped together
- **EnableSideBySidePlacement**: Set to `false` for grouping effect
- **ColumnWidth**: Adjust to control column width within groups

### Grouped Bar Chart

Same concept, just change `Type` to `Chart3DSeriesType.Bar`:

```razor
@using Syncfusion.Blazor.Chart3D

<Chart3DSeries DataSource="@Chart3DPoints" 
               XName="Year" 
               YName="USA_Total" 
               GroupName="USA" 
               Type="Chart3DSeriesType.Bar"
               ColumnWidth="0.2">
</Chart3DSeries>
```

## Cylindrical Charts

Create cylindrical column or bar charts by setting the `ColumnFacet` property to `Chart3DShapeType.Cylinder`.

### Cylindrical Column Chart

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D WallColor="transparent" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100" 
           Title="Car Production by Country – 2021">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category" 
                         Interval="1">
    </Chart3DPrimaryXAxis>
    
    <Chart3DPrimaryYAxis Maximum="4" Interval="1">
    </Chart3DPrimaryYAxis>
    
    <Chart3DTooltipSettings Enable="true" 
                            Header="${point.x}" 
                            Format="Production: <b>${point.y}M</b>">
    </Chart3DTooltipSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@Chart3DPoints" 
                       XName="Country" 
                       YName="Production" 
                       ColumnWidth="0.9" 
                       Type="Chart3DSeriesType.Column" 
                       ColumnFacet="Chart3DShapeType.Cylinder">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ProductionData
    {
        public string Country { get; set; }
        public double Production { get; set; }
    }

    public List<ProductionData> Chart3DPoints = new List<ProductionData>
    {
        new ProductionData { Country = "Czechia", Production = 1.11 },
        new ProductionData { Country = "Spain", Production = 1.66 },
        new ProductionData { Country = "USA", Production = 1.56 },
        new ProductionData { Country = "Germany", Production = 3.1 },
        new ProductionData { Country = "Russia", Production = 1.35 },
        new ProductionData { Country = "Slovakia", Production = 1.0 },
        new ProductionData { Country = "South Korea", Production = 3.16 },
        new ProductionData { Country = "France", Production = 0.92 }
    };
}
```

### Cylindrical Stacked Column Chart

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@DataSource" 
                       XName="X" 
                       YName="YValue" 
                       ColumnFacet="Chart3DShapeType.Cylinder" 
                       Type="Chart3DSeriesType.StackingColumn">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@DataSource" 
                       XName="X" 
                       YName="YValue1" 
                       ColumnFacet="Chart3DShapeType.Cylinder" 
                       Type="Chart3DSeriesType.StackingColumn">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@DataSource" 
                       XName="X" 
                       YName="YValue2" 
                       ColumnFacet="Chart3DShapeType.Cylinder" 
                       Type="Chart3DSeriesType.StackingColumn">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ChartData
    {
        public string X { get; set; }
        public double YValue { get; set; }
        public double YValue1 { get; set; }
        public double YValue2 { get; set; }
    }

    public List<ChartData> DataSource = new List<ChartData>
    {
        new ChartData { X = "Q1", YValue = 30, YValue1 = 20, YValue2 = 15 },
        new ChartData { X = "Q2", YValue = 35, YValue1 = 22, YValue2 = 18 },
        new ChartData { X = "Q3", YValue = 28, YValue1 = 25, YValue2 = 20 },
        new ChartData { X = "Q4", YValue = 40, YValue1 = 27, YValue2 = 22 }
    };
}
```

**Cylindrical Options:**
- **Rectangle** (default): Standard rectangular columns
- **Cylinder**: Cylindrical columns for enhanced 3D effect

## Series Customization

Customize series appearance with the `Fill` property for colors and the `Opacity` property for transparency.

### Custom Colors

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MedalDetails" 
                       Name="Gold" 
                       XName="Country" 
                       YName="Gold" 
                       Fill="#FFD700" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@MedalDetails" 
                       Name="Silver" 
                       XName="Country" 
                       YName="Silver" 
                       Fill="#C0C0C0" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@MedalDetails" 
                       Name="Bronze" 
                       XName="Country" 
                       YName="Bronze" 
                       Fill="#CD7F32" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class MedalInfo
    {
        public string Country { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }

    public List<MedalInfo> MedalDetails = new List<MedalInfo>
    {
        new MedalInfo { Country = "USA", Gold = 39, Silver = 41, Bronze = 33 },
        new MedalInfo { Country = "China", Gold = 38, Silver = 32, Bronze = 19 },
        new MedalInfo { Country = "Japan", Gold = 27, Silver = 14, Bronze = 17 },
        new MedalInfo { Country = "UK", Gold = 22, Silver = 21, Bronze = 22 },
        new MedalInfo { Country = "Germany", Gold = 17, Silver = 19, Bronze = 20 },
        new MedalInfo { Country = "India", Gold = 10, Silver = 12, Bronze = 14 }
    };
}
```

### Custom Opacity

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Opacity Column Chart"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="7"
           TiltAngle="10"
           Depth="100">

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@Data"
                       XName="X"
                       YName="Y"
                       Fill="#0364DE"
                       Opacity="0.7"
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>

</SfChart3D>

@code {
    public class ChartPoint
    {
        public string X { get; set; }
        public double Y { get; set; }
    }

    public List<ChartPoint> Data = new List<ChartPoint>
    {
        new ChartPoint { X = "Jan", Y = 40 },
        new ChartPoint { X = "Feb", Y = 55 },
        new ChartPoint { X = "Mar", Y = 48 },
        new ChartPoint { X = "Apr", Y = 62 },
        new ChartPoint { X = "May", Y = 70 },
        new ChartPoint { X = "Jun", Y = 66 },
        new ChartPoint { X = "Jul", Y = 75 }
    };
}
```

**Customization Properties:**
- **Fill**: Hex color code (e.g., `#FF5733`, `#00bdae`)
- **Opacity**: 0 to 1 (0 = transparent, 1 = opaque)

## Choosing the Right Chart Type

### Column Chart
**Use when:**
- Comparing values across categories
- Categories are short (1-2 words)
- Showing trends over time (quarters, years)
- Emphasizing vertical magnitude

**Examples:**
- Monthly sales comparison
- Product ratings
- Survey results

### Bar Chart
**Use when:**
- Category labels are long (3+ words)
- Comparing horizontal metrics
- Showing rankings or ordered lists
- Better readability with many categories

**Examples:**
- Department names
- Product feature comparisons
- Task completion status

### Stacked Column Chart
**Use when:**
- Showing part-to-whole relationships
- Comparing totals across categories
- Visualizing composition changes over time
- Emphasizing cumulative values

**Examples:**
- Budget allocation by department
- Sales by product category
- Population demographics

### Stacked Bar Chart
**Use when:**
- Same as stacked column but with long labels
- Horizontal space is available
- Better readability for composition data

**Examples:**
- Project resource allocation
- Survey response breakdown
- Market share analysis

## Complete Multi-Type Example

```razor
@page "/chart-types-demo"
@using Syncfusion.Blazor.Chart3D

<h3>Chart Type Comparison</h3>

<div style="display: flex; flex-wrap: wrap;">
    <!-- Column Chart -->
    <div style="width: 48%; margin: 1%;">
        <SfChart3D Title="Column Chart" WallColor="transparent" EnableRotation="true" 
                   RotationAngle="7" TiltAngle="10" Depth="100" Height="300px">
            <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
            </Chart3DPrimaryXAxis>
            <Chart3DSeriesCollection>
                <Chart3DSeries DataSource="@SalesDataList" XName="Category" YName="Sales" 
                               Type="Chart3DSeriesType.Column">
                </Chart3DSeries>
            </Chart3DSeriesCollection>
        </SfChart3D>
    </div>
    
    <!-- Bar Chart -->
    <div style="width: 48%; margin: 1%;">
        <SfChart3D Title="Bar Chart" WallColor="transparent" EnableRotation="true" 
                   RotationAngle="7" TiltAngle="10" Depth="100" Height="300px">
            <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
            </Chart3DPrimaryXAxis>
            <Chart3DSeriesCollection>
                <Chart3DSeries DataSource="@SalesDataList" XName="Category" YName="Sales" 
                               Type="Chart3DSeriesType.Bar">
                </Chart3DSeries>
            </Chart3DSeriesCollection>
        </SfChart3D>
    </div>
    
    <!-- Stacked Column Chart -->
    <div style="width: 48%; margin: 1%;">
        <SfChart3D Title="Stacked Column Chart" WallColor="transparent" EnableRotation="true" 
                   RotationAngle="7" TiltAngle="10" Depth="100" Height="300px">
            <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
            </Chart3DPrimaryXAxis>
            <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>
            <Chart3DSeriesCollection>
                <Chart3DSeries DataSource="@SalesDataList" Name="Q1" XName="Category" YName="Q1" 
                               Type="Chart3DSeriesType.StackingColumn">
                </Chart3DSeries>
                <Chart3DSeries DataSource="@SalesDataList" Name="Q2" XName="Category" YName="Q2" 
                               Type="Chart3DSeriesType.StackingColumn">
                </Chart3DSeries>
            </Chart3DSeriesCollection>
        </SfChart3D>
    </div>
    
    <!-- Stacked Bar Chart -->
    <div style="width: 48%; margin: 1%;">
        <SfChart3D Title="Stacked Bar Chart" WallColor="transparent" EnableRotation="true" 
                   RotationAngle="7" TiltAngle="10" Depth="100" Height="300px">
            <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
            </Chart3DPrimaryXAxis>
            <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>
            <Chart3DSeriesCollection>
                <Chart3DSeries DataSource="@SalesDataList" Name="Q1" XName="Category" YName="Q1" 
                               Type="Chart3DSeriesType.StackingBar">
                </Chart3DSeries>
                <Chart3DSeries DataSource="@SalesDataList" Name="Q2" XName="Category" YName="Q2" 
                               Type="Chart3DSeriesType.StackingBar">
                </Chart3DSeries>
            </Chart3DSeriesCollection>
        </SfChart3D>
    </div>
</div>

@code {
    public class SalesData
    {
        public string Category { get; set; }
        public double Sales { get; set; }
        public double Q1 { get; set; }
        public double Q2 { get; set; }
    }

    public List<SalesData> SalesDataList = new List<SalesData>
    {
        new SalesData { Category = "Electronics", Sales = 45, Q1 = 25, Q2 = 20 },
        new SalesData { Category = "Clothing", Sales = 38, Q1 = 18, Q2 = 20 },
        new SalesData { Category = "Furniture", Sales = 52, Q1 = 30, Q2 = 22 },
        new SalesData { Category = "Books", Sales = 28, Q1 = 15, Q2 = 13 },
        new SalesData { Category = "Toys", Sales = 35, Q1 = 18, Q2 = 17 }
    };
}
```

This comprehensive guide covers all 3D chart types with practical examples. Choose the appropriate chart type based on your data structure and visualization goals!
