# Bubble HeatMap in Blazor HeatMap Chart

This guide explains how to customize the appearance of HeatMap cells as bubbles with different sizes, colors, and sector attributes to visualize multi-dimensional data.

## Table of Contents

- [Overview](#overview)
- [Enabling Bubble Type](#enabling-bubble-type)
- [Bubble Types](#bubble-types)
  - [Bubble Size](#bubble-size)
  - [Bubble Color](#bubble-color)
  - [Bubble Sector](#bubble-sector)
  - [Combining Size and Color](#combining-size-and-color)
- [Data Binding for SizeAndColor Type](#data-binding-for-sizeandcolor-type)
  - [Array Binding](#array-binding)
  - [JSON Binding](#json-binding)
  - [Binding Size and Color from DataSource](#binding-size-and-color-from-datasource)
- [Bubble Configuration Options](#bubble-configuration-options)
- [Use Cases and Best Practices](#use-cases-and-best-practices)

## Overview

By default, HeatMap cells are rendered as rectangles with color fill based on data values using the `TileType` property set to `Rect`. You can change this visualization to display data points as bubbles by setting the `TileType` property to `Bubble` in the `HeatMapCellSettings` component.

Bubble HeatMaps provide additional visual dimensions using the `BubbleType` property:
- **Bubble size**: Represents data magnitude through varying radii (controlled by `BubbleType="BubbleType.Size"`)
- **Bubble color**: Uses color gradients or fixed colors for value differentiation (controlled by `BubbleType="BubbleType.Color"`)
- **Bubble sector**: Uses pie-chart-like sectors within bubbles (controlled by `BubbleType="BubbleType.Sector"`)
- **Bubble size and color**: Combines both dimensions (controlled by `BubbleType="BubbleType.SizeAndColor"`)

This allows you to encode multiple data dimensions in a single visualization, making bubble HeatMaps ideal for complex data analysis.

## Enabling Bubble Type

To render cells as bubbles, set the `TileType` property to `CellType.Bubble` in the `HeatMapCellSettings` component:

```razor
<SfHeatMap DataSource="@HeatMapData">
    <HeatMapCellSettings TileType="CellType.Bubble">
    </HeatMapCellSettings>
</SfHeatMap>
```

Once bubble type is enabled using the `TileType` property, you can further customize the bubble behavior using the `BubbleType` property in the same `HeatMapCellSettings` component.

## Bubble Types

### Bubble Size

In this mode, the bubble radius varies according to data values - larger values produce larger bubbles, smaller values produce smaller bubbles. The `BubbleType` property controls this behavior.

**Configuration:**
- Set `TileType="CellType.Bubble"` in `HeatMapCellSettings`
- Set `BubbleType="Syncfusion.Blazor.HeatMap.BubbleType.Size"` in `HeatMapCellSettings`
- Optionally use `IsInversedBubbleSize="true"` to reverse the size-to-value relationship

**Example:**

```razor
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)">
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
    <HeatMapCellSettings ShowLabel="false" 
                         TileType="CellType.Bubble" 
                         BubbleType="Syncfusion.Blazor.HeatMap.BubbleType.Size">
    </HeatMapCellSettings>
    <HeatMapPaletteSettings Type="PaletteType.Gradient">
        <HeatMapPalettes>
            <HeatMapPalette Color="#C06C84"></HeatMapPalette>
            <HeatMapPalette Color="#6C5B7B"></HeatMapPalette>
            <HeatMapPalette Color="#355C7D"></HeatMapPalette>
        </HeatMapPalettes>
    </HeatMapPaletteSettings>
</SfHeatMap>

@code {
    int[,] GetDefaultData()
    {
        int[,] dataSource = new int[,]
        {
            {73, 39, 26, 39, 94, 0},
            {93, 58, 53, 38, 26, 68},
            {99, 28, 22, 4, 66, 90},
            {14, 26, 97, 69, 69, 3},
            {7, 46, 47, 47, 88, 6},
            {41, 55, 73, 23, 3, 79}
        };
        return dataSource;
    }
    
    string[] XAxisLabels = new string[] { "Nancy", "Andrew", "Janet", "Margaret", "Steven", "Michael" };
    string[] YAxisLabels = new string[] { "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };
    public object HeatMapData { get; set; }
    
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

**Inverse Bubble Size:**

To reverse the size relationship (smaller values = larger bubbles), use the `IsInversedBubbleSize` property set to `true` in the `HeatMapCellSettings` component:

```razor
<HeatMapCellSettings TileType="CellType.Bubble" 
                     BubbleType="Syncfusion.Blazor.HeatMap.BubbleType.Size"
                     IsInversedBubbleSize="true">
</HeatMapCellSettings>
```

### Bubble Color

In this mode, all bubbles have the same size, but their colors vary according to data values controlled by the `BubbleType` property. This is useful when you want to emphasize color differences over size variations.

**Configuration:**
- Set `TileType="CellType.Bubble"` in `HeatMapCellSettings`
- Set `BubbleType="Syncfusion.Blazor.HeatMap.BubbleType.Color"` in `HeatMapCellSettings`

**Example:**

```razor
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)">
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
    <HeatMapCellSettings ShowLabel="true" 
                         TileType="CellType.Bubble" 
                         BubbleType="Syncfusion.Blazor.HeatMap.BubbleType.Color">
    </HeatMapCellSettings>
    <HeatMapPaletteSettings Type="PaletteType.Gradient">
        <HeatMapPalettes>
            <HeatMapPalette Color="#C06C84"></HeatMapPalette>
            <HeatMapPalette Color="#6C5B7B"></HeatMapPalette>
            <HeatMapPalette Color="#355C7D"></HeatMapPalette>
        </HeatMapPalettes>
    </HeatMapPaletteSettings>
</SfHeatMap>

@code {
    int[,] GetDefaultData()
    {
        int[,] dataSource = new int[,]
        {
            {73, 39, 26, 39, 94, 0},
            {93, 58, 53, 38, 26, 68},
            {99, 28, 22, 4, 66, 90},
            {14, 26, 97, 69, 69, 3},
            {7, 46, 47, 47, 88, 6},
            {41, 55, 73, 23, 3, 79}
        };
        return dataSource;
    }
    
    string[] XAxisLabels = new string[] { "Nancy", "Andrew", "Janet", "Margaret", "Steven", "Michael" };
    string[] YAxisLabels = new string[] { "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };
    public object HeatMapData { get; set; }
    
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

### Bubble Sector

In this mode, bubbles are divided into pie-chart-like sectors controlled by the `BubbleType` property, where the sector size represents the data magnitude. Larger sectors indicate higher values.

**Configuration:**
- Set `TileType="CellType.Bubble"` in `HeatMapCellSettings`
- Set `BubbleType="Syncfusion.Blazor.HeatMap.BubbleType.Sector"` in `HeatMapCellSettings`

**Example:**

```razor
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)">
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
    <HeatMapCellSettings ShowLabel="true" 
                         TileType="CellType.Bubble" 
                         BubbleType="Syncfusion.Blazor.HeatMap.BubbleType.Sector">
    </HeatMapCellSettings>
    <HeatMapPaletteSettings Type="PaletteType.Gradient">
        <HeatMapPalettes>
            <HeatMapPalette Color="#C06C84"></HeatMapPalette>
            <HeatMapPalette Color="#6C5B7B"></HeatMapPalette>
            <HeatMapPalette Color="#355C7D"></HeatMapPalette>
        </HeatMapPalettes>
    </HeatMapPaletteSettings>
</SfHeatMap>

@code {
    int[,] GetDefaultData()
    {
        int[,] dataSource = new int[,]
        {
            {73, 39, 26, 39, 94, 0},
            {93, 58, 53, 38, 26, 68},
            {99, 28, 22, 4, 66, 90},
            {14, 26, 97, 69, 69, 3},
            {7, 46, 47, 47, 88, 6},
            {41, 55, 73, 23, 3, 79}
        };
        return dataSource;
    }
    
    string[] XAxisLabels = new string[] { "Nancy", "Andrew", "Janet", "Margaret", "Steven", "Michael" };
    string[] YAxisLabels = new string[] { "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };
    public object HeatMapData { get; set; }
    
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

### Combining Size and Color

The most powerful bubble type combines both size and color variations to represent data values using the `BubbleType` property. This allows you to encode two different data dimensions simultaneously or emphasize a single dimension using both visual attributes.

**Configuration:**
- Set `TileType="CellType.Bubble"` in `HeatMapCellSettings`
- Set `BubbleType="Syncfusion.Blazor.HeatMap.BubbleType.SizeAndColor"` in `HeatMapCellSettings`

This type works with both array and JSON data binding methods. For JSON data with cell binding, use `HeatMapBubbleDataMapping` to control size and color independently.

## Data Binding for SizeAndColor Type

This section demonstrates how to bind data when using `BubbleType="Syncfusion.Blazor.HeatMap.BubbleType.SizeAndColor"` with the `HeatMapDataSourceSettings` component for different data binding approaches.

### Array Binding

#### Array - Table Binding

```razor
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapTitleSettings Text="GDP Growth Rate for Major Economies (in Percentage)">
        <HeatMapTitleTextStyle Size="15px" FontWeight="500" FontStyle="Normal" FontFamily="Segoe UI">
        </HeatMapTitleTextStyle>
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
    <HeatMapCellSettings ShowLabel="true" 
                         TileType="CellType.Bubble" 
                         BubbleType="Syncfusion.Blazor.HeatMap.BubbleType.SizeAndColor">
    </HeatMapCellSettings>
    <HeatMapPaletteSettings Type="PaletteType.Gradient">
        <HeatMapPalettes>
            <HeatMapPalette Color="#F0D6AD" Value=-1></HeatMapPalette>
            <HeatMapPalette Color="#9da49a" Value=0></HeatMapPalette>
            <HeatMapPalette Color="#d7c7a7" Value=3.5></HeatMapPalette>
            <HeatMapPalette Color="#6e888f" Value=6.0></HeatMapPalette>
            <HeatMapPalette Color="#466f86" Value=7.5></HeatMapPalette>
            <HeatMapPalette Color="#19547B" Value=10></HeatMapPalette>
        </HeatMapPalettes>
    </HeatMapPaletteSettings>
    <HeatMapLegendSettings Visible="false"></HeatMapLegendSettings>
</SfHeatMap>

@code {
    double[,] GetDefaultData()
    {
        double[,] dataSource = new double[,]
        {
            {9.5, 2.2, 4.2, 8.2, -0.5, 3.2, 5.4, 7.4, 6.2, 1.4},
            {4.3, 8.9, 10.8, 6.5, 5.1, 6.2, 7.6, 7.5, 6.1, 7.6},
            {3.9, 2.7, 2.5, 3.7, 2.6, 5.1, 5.8, 2.9, 4.5, 5.1},
            {2.4, -3.7, 4.1, 6.0, 5.0, 2.4, 3.3, 4.6, 4.3, 2.7},
            {2.0, 7.0, -4.1, 8.9, 2.7, 5.9, 5.6, 1.9, -1.7, 2.9},
            {5.4, 1.1, 6.9, 4.5, 2.9, 3.4, 1.5, -2.8, -4.6, 1.2},
            {-1.3, 3.9, 3.5, 6.6, 5.2, 7.7, 1.4, -3.6, 6.6, 4.3},
            {-1.6, 2.3, 2.9, -2.5, 1.3, 4.9, 10.1, 3.2, 4.8, 2.0},
            {10.8, -1.6, 4.0, 6.0, 7.7, 2.6, 5.6, -2.5, 3.8, -1.9},
            {6.2, 9.8, -1.5, 2.0, -1.5, 4.3, 6.7, 3.8, -1.2, 2.4},
            {1.2, 10.9, 4.0, -1.4, 2.2, 1.6, -2.6, 2.3, 1.7, 2.4},
            {5.1, -2.4, 8.2, -1.1, 3.5, 6.0, -1.3, 7.2, 9.0, 4.2}
        };
        return dataSource;
    }
    
    string[] XAxisLabels = new string[] { "China", "India", "Australia", "Mexico", "Canada", "Brazil", "USA", "UK", "Germany", "Russia", "France", "Japan" };
    string[] YAxisLabels = new string[] { "2008", "2009", "2010", "2011", "2012", "2013", "2014", "2015", "2016", "2017" };
    public object HeatMapData { get; set; }
    
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

#### Array - Cell Binding

```razor
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapTitleSettings Text="Percentage of Individuals Using Internet by Country">
        <HeatMapTitleTextStyle Size="15px" FontWeight="500" FontStyle="Normal" FontFamily="Segoe UI">
        </HeatMapTitleTextStyle>
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
    <HeatMapLegendSettings Visible="false"></HeatMapLegendSettings>
    <HeatMapCellSettings Format="{value} %" 
                         TileType="CellType.Bubble" 
                         BubbleType="Syncfusion.Blazor.HeatMap.BubbleType.SizeAndColor">
        <HeatMapCellBorder Width="0"></HeatMapCellBorder>
        <HeatMapCellTextStyle Color="White"></HeatMapCellTextStyle>
    </HeatMapCellSettings>
    <HeatMapDataSourceSettings IsJsonData="false" AdaptorType="AdaptorType.Cell">
    </HeatMapDataSourceSettings>
    <HeatMapPaletteSettings Type="PaletteType.Gradient">
        <HeatMapPalettes>
            <HeatMapPalette Color="#3498DB"></HeatMapPalette>
            <HeatMapPalette Color="#2C3E50"></HeatMapPalette>
        </HeatMapPalettes>
    </HeatMapPaletteSettings>
</SfHeatMap>

@code {
    double[,] GetDefaultData()
    {
        double[,] dataSource = new double[,]
        {
            {0, 0, 10.75}, {0, 1, 14.5}, {0, 2, 25.5}, {0, 3, 39.5}, {0, 4, 59.75}, {0, 5, 35.50}, {0, 6, 75.5},
            {1, 0, 20.75}, {1, 1, 35.5}, {1, 2, 29.5}, {1, 3, 75.5}, {1, 4, 80}, {1, 5, 65}, {1, 6, 85},
            {2, 0, 6}, {2, 1, 18.5}, {2, 2, 30.05}, {2, 3, 35.5}, {2, 4, 40.75}, {2, 5, 50.75}, {2, 6, 65},
            {3, 0, 30.5}, {3, 1, 20.5}, {3, 2, 45.30}, {3, 3, 50}, {3, 4, 55}, {3, 5, 85.80}, {3, 6, 87.5},
            {4, 0, 10.5}, {4, 1, 20.75}, {4, 2, 35.5}, {4, 3, 35.5}, {4, 4, 45.5}, {4, 5, 65}, {4, 6, 75.5},
            {5, 0, 45.5}, {5, 1, 20.75}, {5, 2, 45.5}, {5, 3, 50.75}, {5, 4, 79.30}, {5, 5, 84.20}, {5, 6, 87.36},
            {6, 0, 26.82}, {6, 1, 70}, {6, 2, 75}, {6, 3, 79.5}, {6, 4, 88.5}, {6, 5, 89.5}, {6, 6, 91.75},
            {7, 0, 15.75}, {7, 1, 20.75}, {7, 2, 25.5}, {7, 3, 42.35}, {7, 4, 45.15}, {7, 5, 76.5}, {7, 6, 80.5},
            {8, 0, 1.98}, {8, 1, 15.23}, {8, 2, 43}, {8, 3, 49}, {8, 4, 63.80}, {8, 5, 67.97}, {8, 6, 70.52},
            {9, 0, 14.31}, {9, 1, 42.87}, {9, 2, 77.28}, {9, 3, 77.82}, {9, 4, 81.44}, {9, 5, 81.92}, {9, 6, 83.75},
            {10, 0, 25.5}, {10, 1, 35.5}, {10, 2, 40.5}, {10, 3, 45.05}, {10, 4, 50.5}, {10, 5, 75.5}, {10, 6, 90.58}
        };
        return dataSource;
    }
    
    string[] XAxisLabels = new string[] { "China", "Australia", "Mexico", "Canada", "Brazil", "USA", "UK", "Germany", "Russia", "France", "Japan" };
    string[] YAxisLabels = new string[] { "2000", "2005", "2010", "2011", "2012", "2013", "2014" };
    public object HeatMapData { get; set; }
    
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

### JSON Binding

JSON binding allows you to use strongly-typed classes for your data with the `HeatMapDataSourceSettings` component set to `IsJsonData="true"`, making code more maintainable and enabling intellisense support.

#### JSON - Table Binding

```razor
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapTitleSettings Text="Olympic Medal Achievements of Most Successful Countries">
        <HeatMapTitleTextStyle Size="15px" FontWeight="500" FontStyle="Normal" FontFamily="Segoe UI">
        </HeatMapTitleTextStyle>
    </HeatMapTitleSettings>
    <HeatMapDataSourceSettings IsJsonData="true" 
                               AdaptorType="AdaptorType.Table" 
                               XDataMapping="Region">
    </HeatMapDataSourceSettings>
    <HeatMapXAxis Labels="@XLabels" LabelRotation="45" LabelIntersectAction="Syncfusion.Blazor.HeatMap.LabelIntersectAction.None">
    </HeatMapXAxis>
    <HeatMapYAxis Labels="@YLabels">
        <HeatMapYAxisTitle Text="Olympic Year"></HeatMapYAxisTitle>
    </HeatMapYAxis>
    <HeatMapPaletteSettings>
        <HeatMapPalettes>
            <HeatMapPalette Color="#F0C27B"></HeatMapPalette>
            <HeatMapPalette Color="#4B1248"></HeatMapPalette>
        </HeatMapPalettes>
    </HeatMapPaletteSettings>
    <HeatMapCellSettings TileType="CellType.Bubble" BubbleType="Syncfusion.Blazor.HeatMap.BubbleType.SizeAndColor">
        <HeatMapCellBorder Width="1" Radius="4" Color="White"></HeatMapCellBorder>
    </HeatMapCellSettings>
</SfHeatMap>

@code {
    public string[] XLabels = new string[] { "China", "France", "GBR", "Germany", "Italy", "Japan", "KOR", "Russia", "USA" };
    public string[] YLabels = new string[] { "Jan_2000", "Jan_2004", "Jan_2008", "Jan_2012", "Jan_2016" };
    
    public class RegionalData
    {
        public string Region { get; set; }
        public int? Jan_2000 { get; set; }
        public int? Jan_2004 { get; set; }
        public int? Jan_2008 { get; set; }
        public int? Jan_2012 { get; set; }
        public int? Jan_2016 { get; set; }
    }
    
    public RegionalData[] HeatMapData = new RegionalData[]
    {
        new RegionalData { Region = "USA", Jan_2000 = 93, Jan_2004 = 101, Jan_2008 = 112, Jan_2012 = 103, Jan_2016 = 121 },
        new RegionalData { Region = "GBR", Jan_2000 = 28, Jan_2004 = 30, Jan_2008 = 49, Jan_2012 = 65, Jan_2016 = 67 },
        new RegionalData { Region = "China", Jan_2000 = 58, Jan_2004 = 63, Jan_2008 = 100, Jan_2012 = 91, Jan_2016 = 70 },
        new RegionalData { Region = "Russia", Jan_2000 = 89, Jan_2004 = 90, Jan_2008 = 60, Jan_2012 = 69, Jan_2016 = 55 },
        new RegionalData { Region = "Germany", Jan_2000 = 56, Jan_2004 = 49, Jan_2008 = 41, Jan_2012 = 44, Jan_2016 = 42 },
        new RegionalData { Region = "Japan", Jan_2000 = 18, Jan_2004 = 37, Jan_2008 = 25, Jan_2012 = 38, Jan_2016 = 41 },
        new RegionalData { Region = "France", Jan_2000 = 38, Jan_2004 = 33, Jan_2008 = 43, Jan_2012 = 35, Jan_2016 = 42 },
        new RegionalData { Region = "KOR", Jan_2000 = 28, Jan_2004 = 30, Jan_2008 = 32, Jan_2012 = 30, Jan_2016 = 21 },
        new RegionalData { Region = "Italy", Jan_2000 = 34, Jan_2004 = 32, Jan_2008 = 27, Jan_2012 = 28, Jan_2016 = 28 }
    };
}
```

### Binding Size and Color from DataSource

For the `SizeAndColor` bubble type with JSON cell binding, you can bind separate data fields to control bubble size and color independently using the `HeatMapBubbleDataMapping` component with `Size` and `Color` properties.

**Note:** This feature only works with JSON data (`IsJsonData="true"`) and cell adaptor type (`AdaptorType="AdaptorType.Cell"`) in `HeatMapDataSourceSettings`.

```razor
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapEvents TooltipRendering="@TooltipRendering"></HeatMapEvents>
    <HeatMapXAxis Labels="@XLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YLabels"></HeatMapYAxis>
    <HeatMapDataSourceSettings IsJsonData="true" 
                               AdaptorType="AdaptorType.Cell" 
                               XDataMapping="Year" 
                               YDataMapping="Months">
        <HeatMapBubbleDataMapping Size="Accidents" Color="Fatalities"></HeatMapBubbleDataMapping>
    </HeatMapDataSourceSettings>
    <HeatMapCellSettings ShowLabel="false" 
                         TileType="CellType.Bubble" 
                         BubbleType="Syncfusion.Blazor.HeatMap.BubbleType.SizeAndColor">
        <HeatMapCellBorder Width="0"></HeatMapCellBorder>
    </HeatMapCellSettings>
    <HeatMapPaletteSettings Type="PaletteType.Gradient">
        <HeatMapPalettes>
            <HeatMapPalette Color="#C06C84"></HeatMapPalette>
            <HeatMapPalette Color="#355C7D"></HeatMapPalette>
        </HeatMapPalettes>
    </HeatMapPaletteSettings>
    <HeatMapLegendSettings Visible="true"></HeatMapLegendSettings>
    <HeatMapTooltipSettings Enable="true">
        <HeatMapFont Size="12px" FontWeight="500"></HeatMapFont>
    </HeatMapTooltipSettings>
</SfHeatMap>

@code {
    public string[] XLabels = new string[] { "2017", "2016", "2015", "2014", "2013", "2012" };
    public string[] YLabels = new string[] { "Jan-Feb", "Mar-Apr", "May-Jun", "Jul-Aug", "Sep-Oct", "Nov-Dec" };
    
    public void TooltipRendering(Syncfusion.Blazor.HeatMap.TooltipEventArgs args)
    {
        int dataIndex = 0;
        for (int i = 0; i < HeatMapData.Count; i++)
        {
            if (HeatMapData[i].Year == args.XLabel && HeatMapData[i].Months == args.YLabel)
            {
                dataIndex = i;
                break;
            }
        }
        args.Content = new string[] { 
            "Year: " + args.XLabel + "<br/>" + 
            "Months: " + args.YLabel + "<br/>" + 
            "Accidents: " + HeatMapData[dataIndex].Accidents + "<br/>" + 
            "Fatalities: " + HeatMapData[dataIndex].Fatalities 
        };
    }
    
    public List<BubbleDataSource> HeatMapData { get; set; }
    
    public class BubbleDataSource
    {
        public string Year { get; set; }
        public string Months { get; set; }
        public int? Accidents { get; set; }
        public int? Fatalities { get; set; }
        
        public static List<BubbleDataSource> GetData()
        {
            return new List<BubbleDataSource>()
            {
                new BubbleDataSource { Year = "2017", Months = "Jan-Feb", Accidents = 4, Fatalities = 39 },
                new BubbleDataSource { Year = "2017", Months = "Mar-Apr", Accidents = 3, Fatalities = 8 },
                new BubbleDataSource { Year = "2017", Months = "May-Jun", Accidents = 1, Fatalities = 3 },
                new BubbleDataSource { Year = "2016", Months = "Jan-Feb", Accidents = 4, Fatalities = 28 },
                new BubbleDataSource { Year = "2016", Months = "Mar-Apr", Accidents = 5, Fatalities = 92 },
                // ... more data
            };
        }
    }
    
    protected override void OnInitialized()
    {
        this.HeatMapData = BubbleDataSource.GetData();
    }
}
```

In this example:
- `Accidents` property controls bubble **size**
- `Fatalities` property controls bubble **color**
- This allows visualizing two different metrics simultaneously

## Bubble Configuration Options

The following properties in `HeatMapCellSettings` control bubble behavior:

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `TileType` | `CellType` | Sets rendering type using `CellType.Rect` or `CellType.Bubble` | `CellType.Rect` |
| `BubbleType` | `BubbleType` | Defines bubble behavior (Size, Color, Sector, SizeAndColor) | `BubbleType.Size` |
| `IsInversedBubbleSize` | `bool` | Reverses size-to-value relationship when set to `true` | `false` |
| `ShowLabel` | `bool` | Shows/hides cell value labels | `true` |

**BubbleType Enumeration values:**
- `BubbleType.Size` - Varying bubble sizes, constant color
- `BubbleType.Color` - Uniform sizes, varying colors based on values
- `BubbleType.Sector` - Pie-chart sectors within bubbles
- `BubbleType.SizeAndColor` - Combined size and color variations for multi-dimensional data

**HeatMapBubbleDataMapping properties (for SizeAndColor with JSON data):**
- `Size` - Property name from datasource controlling bubble size
- `Color` - Property name from datasource controlling bubble color

## Use Cases and Best Practices

### When to Use Each Bubble Type

**Size** (using `BubbleType="Syncfusion.Blazor.HeatMap.BubbleType.Size"`):
- Single-dimension data where magnitude is the primary concern
- Comparison of relative values across categories
- When color is needed for branding or aesthetic purposes
- Use `IsInversedBubbleSize="true"` when smaller values should have larger bubbles

**Color** (using `BubbleType="Syncfusion.Blazor.HeatMap.BubbleType.Color"`):
- When cell space is limited and bubbles need consistent sizing with `ShowLabel="true"`
- Color-based categorization or thresholds
- Data with distinct value ranges better represented by color gradients

**Sector** (using `BubbleType="Syncfusion.Blazor.HeatMap.BubbleType.Sector"`):
- Percentage or proportion data
- Progress or completion indicators
- When pie-chart-style representation suits your data

**SizeAndColor** (using `BubbleType="Syncfusion.Blazor.HeatMap.BubbleType.SizeAndColor"` with `HeatMapBubbleDataMapping`):
- Multi-dimensional data requiring two metrics with separate `Size` and `Color` properties
- Emphasizing a single metric through both visual channels
- Complex datasets where redundant encoding improves readability

### Best Practices

1. **Choose appropriate bubble types**: Select `BubbleType` based on data characteristics and analysis goals

2. **Consider accessibility**: Color-only encoding (`BubbleType="Color"`) may be problematic for color-blind users; `BubbleType="SizeAndColor"` provides redundancy

3. **Label management**: For size-based bubbles, consider setting `ShowLabel="false"` in `HeatMapCellSettings` to reduce clutter

4. **Palette selection**: Use high-contrast gradient palettes via `HeatMapPaletteSettings` for better bubble color differentiation

5. **Tooltip enhancement**: Enable `HeatMapTooltipSettings` with `Enable="true"` for precise value reading, especially when `ShowLabel="false"`

6. **Data scaling**: Ensure data ranges are appropriate for bubble visualization; use `IsInversedBubbleSize` if needed to prevent extreme outliers from dominating the view

7. **Bubble size limits**: For very small or large datasets, consider using the `AdaptorType` setting in `HeatMapDataSourceSettings` to maintain readability

8. **Combine with legends**: Use `HeatMapLegendSettings` with `Visible="true"` to explain color meanings when using `BubbleType="Color"` or `BubbleType="SizeAndColor"`

9. **Performance**: Bubble rendering with `TileType="CellType.Bubble"` is more complex than rect rendering; test performance with large datasets

10. **Testing**: Always test bubble visualizations with real data using appropriate `BubbleType` and `HeatMapBubbleDataMapping` configurations to ensure clarity and effectiveness
