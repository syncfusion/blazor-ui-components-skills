# Working with Data in Blazor HeatMap Chart

This guide explains how to bind different types of data to the Syncfusion Blazor HeatMap Chart component, including array-based data, JSON data, and cell-based data formats.

## Table of Contents

- [Overview](#overview)
- [Data Adaptor Types](#data-adaptor-types)
- [Array Data Binding](#array-data-binding)
  - [Array - Table Binding](#array---table-binding)
  - [Array - Cell Binding](#array---cell-binding)
- [JSON Data Binding](#json-data-binding)
  - [JSON - Table Binding](#json---table-binding)
  - [JSON - Cell Binding](#json---cell-binding)
- [Empty Points and Null Values](#empty-points-and-null-values)
- [Data Transformation Patterns](#data-transformation-patterns)
- [Performance Considerations](#performance-considerations)
- [Troubleshooting Data Binding Issues](#troubleshooting-data-binding-issues)

## Overview

The HeatMap Chart component supports multiple data binding formats to visualize data in a matrix layout. The component can accept:

- **2D Arrays**: Direct arrays of numbers (int, double, decimal)
- **JSON Objects**: Structured data with field mappings
- **Cell Data**: Individual cell-based data with row/column indices

The data binding type is controlled by the `AdaptorType` property in `HeatMapDataSourceSettings`, which can be set to:
- `AdaptorType.Table` - Default for table-based data
- `AdaptorType.Cell` - For cell-specific data with coordinates
- `AdaptorType.None` - No adaptor (for direct array binding)

## Data Adaptor Types

### Table Adaptor
Used when each data entry represents a complete row or contains multiple column values. This is the default and most common adaptor type.

**Use cases:**
- Time-series data (rows = categories, columns = time periods)
- Comparison data (rows = items, columns = metrics)
- Multi-dimensional data matrices

### Cell Adaptor
Used when each data entry represents a single cell with explicit row and column identifiers. Useful for sparse data or when you need precise control over cell positioning.

**Use cases:**
- Sparse matrices with many empty cells
- Dynamic data where cell positions vary
- Custom data structures requiring explicit mapping

## Array Data Binding

### Array - Table Binding

This is the default and simplest data binding method. Use a 2D array where each sub-array represents a row in the HeatMap.

**Data Structure:**
```csharp
// 2D array: [rows, columns]
double[,] dataSource = new double[6, 10];
```

**Complete Example:**

```razor
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapTitleSettings Text="GDP Growth Rate for Major Economies (in Percentage)">
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
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
</SfHeatMap>

@code {
    public double[,] GetDefaultData()
    {
        double[,] dataSource = new double[6, 10]
        {
            {9.5, 2.2, 4.2, 8.2, -0.5, 3.2, 5.4, 7.4, 6.2, 1.4 },
            {4.3, 8.9, 10.8, 6.5, 5.1, 6.2, 7.6, 7.5, 6.1, 7.6},
            {3.9, 2.7, 2.5, 3.7, 2.6, 5.1, 5.8, 2.9, 4.5, 5.1},
            {2.4, -3.7, 4.1, 6.0, 5.0, 2.4, 3.3, 4.6, 4.3, 2.7},
            {2.0, 7.0, -4.1, 8.9, 2.7, 5.9, 5.6, 1.9, -1.7, 2.9},
            {5.4, 1.1, 6.9, 4.5, 2.9, 3.4, 1.5, -2.8, -4.6, 1.2}
        };
        return dataSource;
    }
    
    public string[] XAxisLabels = new string[] { "China", "India", "Australia", "Mexico", "Canada", "Brazil" };
    public string[] YAxisLabels = new string[] { "2008", "2009", "2010", "2011", "2012", "2013", "2014", "2015", "2016", "2017" };
    public object HeatMapData { get; set; }
    
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

**Key Points:**
- No need to specify `AdaptorType` (defaults to Table)
- Array dimensions: `[rows, columns]` where rows correspond to X-axis and columns to Y-axis
- Axis labels are optional but recommended for clarity
- Supports `int[,]`, `double[,]`, `decimal[,]`, and other numeric types

**When to Use:**
- Simple numeric datasets
- Fixed-size matrices
- When you have complete data for all cells
- Performance-critical applications (fastest binding method)

### Array - Cell Binding

Use this when your data consists of individual cell values with explicit row and column indices. Each array entry contains: `[rowIndex, columnIndex, value]`.

**Data Structure:**
```csharp
// Each row: [xIndex, yIndex, value]
double[,] cellData = new double[,]
{
    {0, 0, 10.75}, // Row 0, Column 0, Value 10.75
    {0, 1, 14.5},  // Row 0, Column 1, Value 14.5
    // ... more cells
};
```

**Complete Example:**

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
    <HeatMapCellSettings Format="{value} %">
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
    public double[,] GetDefaultData()
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
    
    public string[] XAxisLabels = new string[] { "China", "Australia", "Mexico", "Canada", "Brazil", "USA", "UK", "Germany", "Russia", "France", "Japan" };
    public string[] YAxisLabels = new string[] { "2000", "2005", "2010", "2011", "2012", "2013", "2014" };
    public object HeatMapData { get; set; }
    
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

**Key Points:**
- Must set `AdaptorType="AdaptorType.Cell"`
- Each entry format: `{rowIndex, colIndex, value}`
- Indices are zero-based (0, 1, 2, ...)
- Missing cells will be treated as empty points
- Axis labels must match the number of unique indices

**When to Use:**
- Sparse data matrices (many empty cells)
- Dynamic datasets where not all cells have values
- Data from databases with cell coordinates
- When you need fine-grained control over cell positioning

## JSON Data Binding

### JSON - Table Binding

In this format, each JSON object represents a complete row, with one property for the row identifier and additional properties for each column value.

**Data Structure:**
```csharp
public class RegionalData
{
    public string Region { get; set; }      // X-axis mapping
    public int? Year_2000 { get; set; }    // Column 1
    public int? Year_2004 { get; set; }    // Column 2
    // ... more year columns
}
```

**Complete Example:**

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
    <HeatMapCellSettings>
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

**Key Configuration:**
- `IsJsonData="true"` - Enables JSON data binding
- `AdaptorType="AdaptorType.Table"` - Table adaptor mode
- `XDataMapping="Region"` - Maps the property used for row headers
- Property names (Jan_2000, etc.) automatically map to Y-axis labels

**When to Use:**
- Data from APIs or databases in JSON format
- When working with entity models or DTOs
- Strongly-typed data scenarios
- Data with meaningful property names

### JSON - Cell Binding

Each JSON object represents a single cell with properties for row identifier, column identifier, and value.

**Data Structure:**
```csharp
public class CellData
{
    public string RowId { get; set; }      // X-axis identifier
    public string ColumnId { get; set; }   // Y-axis identifier
    public string Value { get; set; }      // Cell value
}
```

**Complete Example:**

```razor
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapTitleSettings Text="Most Visited Destinations by International Tourist Arrivals">
        <HeatMapTitleTextStyle Size="15px" FontWeight="500" FontStyle="Normal" FontFamily="Segoe UI">
        </HeatMapTitleTextStyle>
    </HeatMapTitleSettings>
    <HeatMapDataSourceSettings IsJsonData="true" 
                               AdaptorType="AdaptorType.Cell" 
                               XDataMapping="RowId" 
                               YDataMapping="ColumnId" 
                               ValueMapping="Value">
    </HeatMapDataSourceSettings>
    <HeatMapXAxis Labels="@XLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YLabels"></HeatMapYAxis>
    <HeatMapPaletteSettings>
        <HeatMapPalettes>
            <HeatMapPalette Color="#DCD57E"></HeatMapPalette>
            <HeatMapPalette Color="#A6DC7E"></HeatMapPalette>
            <HeatMapPalette Color="#7EDCA2"></HeatMapPalette>
            <HeatMapPalette Color="#6EB5D0"></HeatMapPalette>
        </HeatMapPalettes>
    </HeatMapPaletteSettings>
    <HeatMapCellSettings ShowLabel="true" Format="{value} M">
        <HeatMapCellBorder Width="1" Radius="4" Color="White"></HeatMapCellBorder>
    </HeatMapCellSettings>
</SfHeatMap>

@code {
    public string[] XLabels = new string[] { "Austria", "China", "France", "Germany", "Italy", "Mexico", "Spain", "Thailand", "UK", "USA" };
    public string[] YLabels = new string[] { "2010", "2011", "2012", "2013", "2014", "2015", "2016" };
    
    public class SampleData
    {
        public string RowId { get; set; }
        public string ColumnId { get; set; }
        public string Value { get; set; }
    }
    
    public SampleData[] HeatMapData = new SampleData[]
    {
        new SampleData { RowId = "France", ColumnId = "2010", Value = "77.6" },
        new SampleData { RowId = "France", ColumnId = "2011", Value = "79.4" },
        new SampleData { RowId = "France", ColumnId = "2012", Value = "80.8" },
        new SampleData { RowId = "France", ColumnId = "2013", Value = "86.6" },
        new SampleData { RowId = "USA", ColumnId = "2010", Value = "60.6" },
        new SampleData { RowId = "USA", ColumnId = "2011", Value = "65.4" },
        new SampleData { RowId = "Spain", ColumnId = "2010", Value = "64.9" },
        new SampleData { RowId = "Spain", ColumnId = "2011", Value = "52.6" },
        new SampleData { RowId = "China", ColumnId = "2010", Value = "55.6" },
        new SampleData { RowId = "Italy", ColumnId = "2010", Value = "43.6" },
        // ... more data points
    };
}
```

**Key Configuration:**
- `IsJsonData="true"` - Enables JSON data binding
- `AdaptorType="AdaptorType.Cell"` - Cell adaptor mode
- `XDataMapping="RowId"` - Property name for X-axis values
- `YDataMapping="ColumnId"` - Property name for Y-axis values
- `ValueMapping="Value"` - Property name for cell values

**When to Use:**
- Sparse datasets with many empty cells
- Data from relational databases (cell-level records)
- When each cell is stored as a separate entity
- Dynamically changing grid structures

## Empty Points and Null Values

The HeatMap automatically handles null values in your data, treating them as empty points that are not displayed or are rendered with a default color.

**Example with Null Values:**

```razor
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)">
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
    <HeatMapPaletteSettings EmptyPointColor="#d3d3d3">
    </HeatMapPaletteSettings>
</SfHeatMap>

@code {
    public int?[,] GetDefaultData()
    {
        int?[,] dataSource = new int?[6, 12]
        {
            {8, 5, 2, 6, 8, 2, 9, 3, 7, 8, 7, 6},
            {5, null, 4, 9, 10, null, 11, 7, 3, 7, 8, null},
            {8, 7, 2, null, 5, 3, null, 2, 1, 8, null, 4},
            {10, 2, null, 4, 5, null, 1, 10, 5, 2, 1, null},
            {1, 2, 9, 4, null, 5, 1, null, 12, 1, null, 4},
            {4, null, 3, 5, 2, null, null, null, 5, null, 1, 3},
        };
        return dataSource;
    }
    
    public string[] XAxisLabels = new string[] { "Nancy", "Andrew", "Janet", "Margaret", "Steven", "Michael", "Robert", "Laura", "Anne", "Paul", "Karin", "Mario" };
    public string[] YAxisLabels = new string[] { "2008", "2009", "2010", "2011", "2012", "2013", "2014", "2015", "2016", "2017", "2018", "2019" };
    public object HeatMapData { get; set; }
    
    protected override void OnInitialized()
    {
        HeatMapData = GetDefaultData();
    }
}
```

**Key Points:**
- Use nullable types (`int?`, `double?`, etc.) to allow null values
- Set `EmptyPointColor` property to customize the color of empty cells
- Empty points are excluded from legend and palette calculations
- Default empty point color is light gray

## Data Transformation Patterns

### Converting Database Results to HeatMap Data

```csharp
// Example: Transform EF Core query results to 2D array
public double[,] TransformDatabaseData(List<SalesRecord> records, string[] regions, string[] months)
{
    double[,] data = new double[regions.Length, months.Length];
    
    for (int i = 0; i < regions.Length; i++)
    {
        for (int j = 0; j < months.Length; j++)
        {
            var value = records
                .Where(r => r.Region == regions[i] && r.Month == months[j])
                .Sum(r => r.Amount);
            data[i, j] = value;
        }
    }
    
    return data;
}
```

### Aggregating Data for HeatMap

```csharp
// Example: Aggregate hourly data into daily summaries
public double[,] AggregateToDailyData(List<HourlyMetric> hourlyData, DateTime startDate, int days)
{
    double[,] dailyData = new double[24, days]; // 24 hours x N days
    
    for (int hour = 0; hour < 24; hour++)
    {
        for (int day = 0; day < days; day++)
        {
            var targetDate = startDate.AddDays(day);
            var average = hourlyData
                .Where(m => m.Timestamp.Date == targetDate.Date && m.Timestamp.Hour == hour)
                .Average(m => m.Value);
            dailyData[hour, day] = average;
        }
    }
    
    return dailyData;
}
```

### Handling Missing Data

```csharp
// Fill missing values with interpolation or defaults
public double[,] FillMissingData(double?[,] sparseData, double defaultValue = 0)
{
    int rows = sparseData.GetLength(0);
    int cols = sparseData.GetLength(1);
    double[,] filledData = new double[rows, cols];
    
    for (int i = 0; i < rows; i++)
    {
        for (int j = 0; j < cols; j++)
        {
            filledData[i, j] = sparseData[i, j] ?? defaultValue;
        }
    }
    
    return filledData;
}
```

## Performance Considerations

### Large Dataset Optimization

**Best Practices:**
1. **Use array binding** instead of JSON for better performance with large datasets
2. **Limit visible cells**: Show only necessary data, implement pagination if needed
3. **Avoid complex calculations** in data binding - pre-process data before binding
4. **Use async operations** for data loading:

```csharp
protected override async Task OnInitializedAsync()
{
    HeatMapData = await LoadDataAsync();
}

private async Task<double[,]> LoadDataAsync()
{
    // Simulate async data loading
    await Task.Delay(100);
    return GetDefaultData();
}
```

### Memory Management

For very large matrices (> 100x100 cells):
- Consider data sampling or aggregation
- Implement virtual scrolling if supported
- Load data in chunks if possible
- Use server-side processing for calculations

### Rendering Performance

```csharp
// Efficient data updates
private void UpdateCellValue(int row, int col, double newValue)
{
    if (HeatMapData is double[,] matrix)
    {
        matrix[row, col] = newValue;
        // Trigger re-render only if needed
        StateHasChanged();
    }
}
```

## Troubleshooting Data Binding Issues

### Issue: Data Not Displaying

**Solution Checklist:**
- Verify `DataSource` is properly bound with `@` symbol
- Check data is initialized before component renders
- Ensure array dimensions match axis label counts
- Verify numeric data types (not string for array binding)

### Issue: Axis Labels Don't Match Data

**Solution:**
- Number of axis labels must match data dimensions
- For array binding: `XAxisLabels.Length` should equal first dimension
- For JSON binding: Property names must match field mappings

### Issue: Empty or Missing Cells

**Solution:**
- Use nullable types (`int?`, `double?`) to allow null values
- Set `EmptyPointColor` to visualize empty cells
- Check AdaptorType matches your data structure

### Issue: JSON Data Not Binding

**Solution:**
- Set `IsJsonData="true"` in `HeatMapDataSourceSettings`
- Verify `XDataMapping`, `YDataMapping`, and `ValueMapping` property names
- Ensure property names are case-sensitive matches
- Check AdaptorType (Table vs Cell)

### Issue: Performance Degradation

**Solution:**
- Reduce dataset size through aggregation
- Use array binding instead of JSON for large data
- Disable animations if not needed
- Implement data virtualization or pagination

## Best Practices Summary

1. **Choose the right binding method**:
   - Array-Table: Simple, fixed-size matrices
   - Array-Cell: Sparse data with many empty cells
   - JSON-Table: Structured data from APIs/databases
   - JSON-Cell: Cell-level database records

2. **Handle null values explicitly**: Use nullable types and customize empty point colors

3. **Pre-process data**: Transform and aggregate data before binding for better performance

4. **Match dimensions**: Ensure axis labels match data dimensions

5. **Type safety**: Use strongly-typed classes for JSON binding

6. **Async loading**: Load large datasets asynchronously to avoid UI blocking

7. **Test with real data**: Validate with production-like datasets to catch edge cases
