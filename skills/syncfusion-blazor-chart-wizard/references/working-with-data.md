# Working with Data in Blazor Chart Wizard Component

## Table of Contents

- [Overview](#overview)
- [ChartSettings Properties](#chartsettings-properties)
  - [DataSource Property](#datasource-property)
  - [CategoryFields Property](#categoryfields-property)
  - [SeriesFields Property](#seriesfields-property)
  - [SeriesType Property](#seriestype-property)
- [Configuring Fields](#configuring-fields)
  - [Single-Category, Single-Series Chart](#single-category-single-series-chart)
  - [Multi-Series Chart](#multi-series-chart)
  - [Field Configuration Best Practices](#field-configuration-best-practices)
- [List Binding](#list-binding)
  - [Using List<T> as DataSource](#using-listt-as-datasource)
  - [Complete List Binding Example](#complete-list-binding-example)
- [ObservableCollection Binding](#observablecollection-binding)
  - [Dynamic Data Collections](#dynamic-data-collections)
  - [Complete ObservableCollection Example](#complete-observablecollection-example)
- [Data Model Design](#data-model-design)
  - [Creating Data Models](#creating-data-models)
  - [Model Properties](#model-properties)
- [Working Examples](#working-examples)

---

## Overview

The primary configuration for the chart wizard is provided via the `ChartSettings` component. This component allows you to bind data sources and configure how data is displayed in the chart through various properties.

## ChartSettings Properties

### DataSource Property

**Type:** `IEnumerable<Object>`

The `DataSource` property supplies the collection of data objects for the chart. Each object in the collection should have fields that are referenced by `CategoryFields` and `SeriesFields` properties. Any `IEnumerable<Object>` type can be assigned to this property, including `List<T>`, `ObservableCollection<T>`, and other collection types.

### CategoryFields Property

**Type:** `IEnumerable<string>`

The `CategoryFields` property specifies one or more field names from your data objects to use as category (x-axis) values. This property accepts a collection of field names that exist in your data model.

**Examples:**
- Single field: `new List<string>{ "Country" }` or `new[] { "Month" }`
- Multiple fields: `new[] { "Region", "Country" }` for nested or grouped categories

### SeriesFields Property

**Type:** `IEnumerable<string>`

The `SeriesFields` property lists one or more numeric field names to render as chart series. Use multiple field names to create multi-series charts that display multiple data values for each category.

**Examples:**
- Single series: `new[] { "Sales" }`
- Multiple series: `new[] { "Gold", "Silver", "Bronze" }`

**Important:** The order of `SeriesFields` determines the default series ordering in the chart.

### SeriesType Property

**Type:** `ChartWizardSeriesType`

The `SeriesType` property selects the chart type for rendering series. Common values include:
- `ChartWizardSeriesType.Bar` - Horizontal bar charts
- `ChartWizardSeriesType.Column` - Vertical column charts
- `ChartWizardSeriesType.Line` - Line charts
- `ChartWizardSeriesType.Area` - Area charts
- `ChartWizardSeriesType.Pie` - Pie charts

## Configuring Fields

### Single-Category, Single-Series Chart

For charts with one category field and one series field, use the following configuration:

```razor
<ChartSettings DataSource="@SalesData"
               CategoryFields="@(new[]{ "Month" })"
               SeriesFields="@(new[]{ "Sales" })"
               SeriesType="ChartWizardSeriesType.Column" />
```

This configuration creates a simple chart where:
- `Month` field provides the x-axis categories
- `Sales` field provides the y-axis values
- Data is displayed as columns

### Multi-Series Chart

For charts displaying multiple data series for each category:

```razor
<ChartSettings DataSource="@OlympicsData"
               CategoryFields="@(new[]{ "Country" })"
               SeriesFields="@(new[]{ "Gold", "Silver", "Bronze" })"
               SeriesType="ChartWizardSeriesType.Bar" />
```

This configuration creates a multi-series chart where:
- `Country` field provides the x-axis categories
- Three series (`Gold`, `Silver`, `Bronze`) are displayed for each country
- Data is displayed as horizontal bars

### Field Configuration Best Practices

**Important Considerations:**

- The order of `SeriesFields` determines the default series ordering in the chart visualization
- `CategoryFields` can include multiple fields for nested or grouped categories; the wizard will combine them as specified
- Ensure that series fields contain numeric data types for proper chart rendering
- Category fields can be strings, dates, or other comparable types

## List Binding

### Using List<T> as DataSource

Any `IEnumerable` object can be assigned to the `DataSource` property of the `ChartSettings`. The most common approach is using `List<T>` where `T` is your data model class.

**Key Benefits:**
- Simple and straightforward implementation
- Type-safe data binding
- IntelliSense support for data model properties
- No additional dependencies required

### Complete List Binding Example

```razor
@using Syncfusion.Blazor.ChartWizard

<div class="control-section">
    <SfChartWizard Width="90%" Theme="Theme.Fluent2" PropertyPanelExpanded="true">
        <ChartSettings DataSource="@OlympicsDataSource"
                        CategoryFields="@(new[] { "Country" })"
                        SeriesFields="@(new[] { "Gold", "Silver", "Bronze" })"
                        SeriesType="ChartWizardSeriesType.Bar"
                        EnablePropertyPanel="true"
                        AllowExport="true">
        </ChartSettings>
    </SfChartWizard>
</div>

@code {
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country" };

    private readonly List<OlympicsData> OlympicsDataSource = new()
    {
        new OlympicsData { Country = "USA", CountryCode = "USA", Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsData { Country = "China", CountryCode = "CHN", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsData { Country = "Great Britain", CountryCode = "GBR", Gold = 14, Silver = 22, Bronze = 29 },
        new OlympicsData { Country = "France", CountryCode = "FRA", Gold = 16, Silver = 26, Bronze = 22 },
        new OlympicsData { Country = "Australia", CountryCode = "AUS", Gold = 18, Silver = 19, Bronze = 16 },
        new OlympicsData { Country = "Japan", CountryCode = "JPN", Gold = 20, Silver = 12, Bronze = 13 },
        new OlympicsData { Country = "Italy", CountryCode = "ITA", Gold = 12, Silver = 13, Bronze = 15 },
        new OlympicsData { Country = "Netherlands", CountryCode = "NLD", Gold = 15, Silver = 7,  Bronze = 12 },
        new OlympicsData { Country = "Germany", CountryCode = "DEU", Gold = 12, Silver = 13, Bronze = 8  },
        new OlympicsData { Country = "South Korea", CountryCode = "KOR", Gold = 13, Silver = 9,  Bronze = 10 }
    };

    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }
}
```

This example demonstrates:
- Creating a `List<OlympicsData>` as the data source
- Initializing the list with inline data
- Binding the list to `ChartSettings`
- Configuring multiple series (Gold, Silver, Bronze) for visualization
- Using a custom data model class with multiple properties

## ObservableCollection Binding

### Dynamic Data Collections

The `ObservableCollection<T>` provides a dynamic data collection that sends notifications when items are added, removed, or moved. This collection implements the `INotifyCollectionChanged` interface, which provides notification when the collection changes through adding, removing, moving, or clearing operations.

**Key Benefits:**
- Automatic UI updates when collection changes
- Built-in change notification mechanism
- Ideal for real-time data scenarios
- Supports dynamic data operations

**Use Cases:**
- Real-time data updates
- Interactive charts with data manipulation
- Scenarios requiring automatic chart refresh
- Applications with frequently changing data

### Complete ObservableCollection Example

```razor
@using Syncfusion.Blazor.ChartWizard
@using System.Collections.ObjectModel

<div class="control-section">
    <SfChartWizard>
        <ChartSettings DataSource="@OlympicsDataSource"
                    CategoryFields="@categories"
                    SeriesFields="@chartSeries"
                    SeriesType="ChartWizardSeriesType.Bar" />
    </SfChartWizard>
</div>

@code {
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "CountryCode" };
    private ObservableCollection<OlympicsData>? OlympicsDataSource;

    public class OlympicsData
    {
        public string? CountryCode { get; set; }
        public double Gold { get; set; }
        public double Silver { get; set; }
        public double Bronze { get; set; }
        
        public static ObservableCollection<OlympicsData> GetData()
        {
            return new ObservableCollection<OlympicsData>
            {
                new OlympicsData { CountryCode = "GBR", Gold = 27, Silver = 23, Bronze = 17 },
                new OlympicsData { CountryCode = "CHN", Gold = 26, Silver = 18, Bronze = 26 },
                new OlympicsData { CountryCode = "AUS", Gold = 8, Silver = 11, Bronze = 10 },
                new OlympicsData { CountryCode = "RUS", Gold = 19, Silver = 18, Bronze = 19 }
            };
        }
    }

    protected override void OnInitialized()
    {
        OlympicsDataSource = OlympicsData.GetData();
    }
}
```

This example demonstrates:
- Using `ObservableCollection<T>` for dynamic data binding
- Creating a static factory method (`GetData()`) for data initialization
- Initializing the collection in the `OnInitialized` lifecycle method
- Proper nullable reference type handling with `?` operator
- Using `double` data type for numeric values

## Data Model Design

### Creating Data Models

When designing data models for the Chart Wizard, create classes with properties that represent your data structure. Each property should have appropriate data types and access modifiers.

**Best Practices:**
- Use meaningful property names that describe the data
- Include nullable reference types (`?`) for optional string properties
- Use numeric types (`int`, `double`, `decimal`) for series data
- Consider using `string?` for category fields that might be null

### Model Properties

**Category Field Properties:**
- Can be `string`, `string?`, `DateTime`, or other comparable types
- Used for x-axis values or grouping
- Should have public getters and setters

**Series Field Properties:**
- Should be numeric types: `int`, `double`, `decimal`, `float`
- Used for y-axis values or data points
- Must be public properties with getters and setters

**Example Data Model:**

```csharp
public class OlympicsData
{
    public string? Country { get; set; }      // Category field (nullable)
    public string? CountryCode { get; set; }  // Alternative category field
    public int Gold { get; set; }             // Series field (numeric)
    public int Silver { get; set; }           // Series field (numeric)
    public int Bronze { get; set; }           // Series field (numeric)
}
```

## Working Examples

### Example 1: Sales Data with List<T>

```csharp
public class SalesData
{
    public string? Month { get; set; }
    public decimal Sales { get; set; }
    public decimal Expenses { get; set; }
}

private readonly List<SalesData> SalesDataSource = new()
{
    new SalesData { Month = "January", Sales = 35000, Expenses = 28000 },
    new SalesData { Month = "February", Sales = 42000, Expenses = 31000 },
    new SalesData { Month = "March", Sales = 48000, Expenses = 35000 }
};
```

Configuration:
```razor
<ChartSettings DataSource="@SalesDataSource"
               CategoryFields="@(new[]{ "Month" })"
               SeriesFields="@(new[]{ "Sales", "Expenses" })"
               SeriesType="ChartWizardSeriesType.Column" />
```

### Example 2: Real-Time Data with ObservableCollection

```csharp
private ObservableCollection<LiveData>? LiveDataSource;

public class LiveData
{
    public DateTime Timestamp { get; set; }
    public double Value { get; set; }
}

protected override void OnInitialized()
{
    LiveDataSource = new ObservableCollection<LiveData>();
    // Add initial data
    LiveDataSource.Add(new LiveData { Timestamp = DateTime.Now, Value = 100 });
}

// Later: Add new data points dynamically
// LiveDataSource.Add(new LiveData { Timestamp = DateTime.Now, Value = 150 });
```

Configuration:
```razor
<ChartSettings DataSource="@LiveDataSource"
               CategoryFields="@(new[]{ "Timestamp" })"
               SeriesFields="@(new[]{ "Value" })"
               SeriesType="ChartWizardSeriesType.Line" />
```

### Example 3: Nested Categories

```csharp
public class RegionalSalesData
{
    public string? Region { get; set; }
    public string? Country { get; set; }
    public int Sales { get; set; }
}
```

Configuration:
```razor
<ChartSettings DataSource="@RegionalSalesDataSource"
               CategoryFields="@(new[]{ "Region", "Country" })"
               SeriesFields="@(new[]{ "Sales" })"
               SeriesType="ChartWizardSeriesType.Bar" />
```
