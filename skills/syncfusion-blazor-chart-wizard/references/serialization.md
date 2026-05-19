# Serialization in Blazor Chart Wizard Component

## Table of Contents

1. [Overview](#overview)
2. [Serialization Methods](#serialization-methods)
   - [SaveChart() Method](#savechart-method)
   - [LoadChartAsync() Method](#loadchartasync-method)
3. [Key Concepts](#key-concepts)
4. [Complete Implementation Example](#complete-implementation-example)
   - [Component Setup](#component-setup)
   - [Data Model](#data-model)
   - [Save and Load Operations](#save-and-load-operations)
   - [Styling](#styling)
5. [Storage Options](#storage-options)
   - [Browser Storage](#browser-storage)
   - [Database Storage](#database-storage)
   - [File Storage](#file-storage)
6. [Best Practices](#best-practices)
7. [Troubleshooting](#troubleshooting)
   - [Common Issues](#common-issues)
   - [Solutions](#solutions)

---

## Overview

The `Chart Wizard` component makes it simple to save and restore your entire chart wizard configuration. This is useful for persisting user settings, sharing chart setups, or restoring previous states.

Serialization captures the complete runtime state of the chart wizard, including:
- Settings and configuration
- Series definitions
- Axes configuration
- Titles and labels
- Styles and themes
- And more

---

## Serialization Methods

### SaveChart() Method

The `SaveChart()` method serializes the current chart state and returns it as a JSON string.

**Signature:**
```csharp
public string SaveChart()
```

**Returns:**
- `string` - A JSON string representing the complete chart wizard configuration

**Usage:**
```csharp
string serializedData = ChartWizard.SaveChart();
```

### LoadChartAsync() Method

The `LoadChartAsync()` method loads a chart configuration from a JSON string (produced by `SaveChart()`) and applies it to the wizard instance.

**Signature:**
```csharp
public Task LoadChartAsync(string data)
```

**Parameters:**
- `data` (string) - The JSON string produced by `SaveChart()`

**Returns:**
- `Task` - An asynchronous task representing the load operation

**Usage:**
```csharp
await ChartWizard.LoadChartAsync(serializedData);
```

---

## Key Concepts

**Important Notes:**

- The serialized JSON captures the full runtime state of the chart wizard. You can store this string in a database, file, or browser storage for later use.
- `LoadChartAsync` resets the chart to its default state before applying the values from the JSON.
- Always use the JSON string produced by `SaveChart()` as input for `LoadChartAsync()`.
- The serialization process is lossless - all configuration details are preserved.
- The JSON format is specific to the Chart Wizard component version.

---

## Complete Implementation Example

### Component Setup

```cshtml
@using Syncfusion.Blazor.ChartWizard

<div class="control-section">
    <div class="toolbar-container">
        <div>
            <button class="btn btn-primary" @onclick="SaveChartAsync">Save</button>
            <button class="btn btn-primary" @onclick="OpenChartAsync">Load</button>
        </div>
    </div>
    <div class="content-wrapper">
        <SfChartWizard @ref="ChartWizard" Width="100%">
            <ChartSettings DataSource="@Top10Cities" 
                          CategoryFields="@categories" 
                          SeriesFields="@chartSeries" 
                          SeriesType="@chartWizardSeriesType">
            </ChartSettings>
        </SfChartWizard>
    </div>
</div>
```

### Data Model

```csharp
public class GlobalCityPopulationItem
{
    public string? City { get; set; }
    public string? Country { get; set; }
    public double? Population { get; set; }
}
```

### Save and Load Operations

```csharp
@code {
    private SfChartWizard? ChartWizard;
    private readonly List<string> chartSeries = new() { "Population" };
    private readonly List<string> categories = new() { "City", "Country" };
    private ChartWizardSeriesType chartWizardSeriesType = ChartWizardSeriesType.Bar;
    public string? serializedString;

    private readonly List<GlobalCityPopulationItem> Top10Cities = new()
    {
        new() { City = "Tokyo", Country = "Japan", Population = 37.4 },
        new() { City = "Delhi", Country = "India", Population = 31.0 },
        new() { City = "Shanghai", Country = "China", Population = 27.1 },
        new() { City = "Sao Paulo", Country = "Brazil", Population = 22.0 },
        new() { City = "Mexico City", Country = "Mexico", Population = 21.9 },
        new() { City = "Cairo", Country = "Egypt", Population = 20.9 },
        new() { City = "Dhaka", Country = "Bangladesh", Population = 20.3 },
        new() { City = "Beijing", Country = "China", Population = 20.0 },
        new() { City = "Mumbai", Country = "India", Population = 20.0 },
        new() { City = "Osaka", Country = "Japan", Population = 19.2 }
    };

    private async Task SaveChartAsync()
    {
        if(ChartWizard != null)
            serializedString = ChartWizard.SaveChart();
    }

    private async Task OpenChartAsync()
    {
        if(ChartWizard != null)
            await ChartWizard.LoadChartAsync(serializedString);
    }
}
```

### Styling

```css
<style>
    .toolbar-container {
        width: 100%;
        height: 10%;
        padding-top: 10px;
        padding-bottom: 10px;
    }
</style>
```

---

## Storage Options

### Browser Storage

Store serialized data in browser storage for client-side persistence:

```csharp
// Using localStorage
@inject IJSRuntime JSRuntime

private async Task SaveToLocalStorage()
{
    if(ChartWizard != null)
    {
        string serializedData = ChartWizard.SaveChart();
        await JSRuntime.InvokeVoidAsync("localStorage.setItem", "chartConfig", serializedData);
    }
}

private async Task LoadFromLocalStorage()
{
    if(ChartWizard != null)
    {
        string? serializedData = await JSRuntime.InvokeAsync<string>("localStorage.getItem", "chartConfig");
        if (!string.IsNullOrEmpty(serializedData))
        {
            await ChartWizard.LoadChartAsync(serializedData);
        }
    }
}
```

### Database Storage

Store serialized data in a database for server-side persistence:

```csharp
// Example with Entity Framework
public class ChartConfiguration
{
    public int Id { get; set; }
    public string UserId { get; set; }
    public string ConfigName { get; set; }
    public string SerializedData { get; set; }
    public DateTime CreatedDate { get; set; }
}

// Save to database
private async Task SaveToDatabase()
{
    if(ChartWizard != null)
    {
        string serializedData = ChartWizard.SaveChart();
        
        var config = new ChartConfiguration
        {
            UserId = currentUserId,
            ConfigName = "My Chart Config",
            SerializedData = serializedData,
            CreatedDate = DateTime.UtcNow
        };
        
        await dbContext.ChartConfigurations.AddAsync(config);
        await dbContext.SaveChangesAsync();
    }
}

// Load from database
private async Task LoadFromDatabase(int configId)
{
    if(ChartWizard != null)
    {
        var config = await dbContext.ChartConfigurations.FindAsync(configId);
        if (config != null)
        {
            await ChartWizard.LoadChartAsync(config.SerializedData);
        }
    }
}
```

### File Storage

Store serialized data as files on the server or client:

```csharp
// Server-side file storage
private async Task SaveToFile()
{
    if(ChartWizard != null)
    {
        string serializedData = ChartWizard.SaveChart();
        string filePath = Path.Combine("ChartConfigs", $"config_{DateTime.Now:yyyyMMddHHmmss}.json");
        
        await File.WriteAllTextAsync(filePath, serializedData);
    }
}

private async Task LoadFromFile(string filePath)
{
    if(ChartWizard != null && File.Exists(filePath))
    {
        string serializedData = await File.ReadAllTextAsync(filePath);
        await ChartWizard.LoadChartAsync(serializedData);
    }
}
```

---

## Best Practices

1. **Always check for null references** before calling serialization methods:
   ```csharp
   if(ChartWizard != null)
   {
       string data = ChartWizard.SaveChart();
   }
   ```

2. **Validate serialized data** before loading:
   ```csharp
   if (!string.IsNullOrEmpty(serializedString))
   {
       await ChartWizard.LoadChartAsync(serializedString);
   }
   ```

3. **Use async/await** properly with LoadChartAsync:
   ```csharp
   private async Task LoadChart()
   {
       await ChartWizard.LoadChartAsync(data);
       // Additional code after load completes
   }
   ```

4. **Store metadata** with serialized data:
   ```csharp
   public class ChartConfigWrapper
   {
       public string Name { get; set; }
       public DateTime SavedDate { get; set; }
       public string Version { get; set; }
       public string SerializedData { get; set; }
   }
   ```

5. **Handle errors gracefully**:
   ```csharp
   try
   {
       await ChartWizard.LoadChartAsync(serializedData);
   }
   catch (Exception ex)
   {
       // Log error and show user-friendly message
       Console.WriteLine($"Failed to load chart: {ex.Message}");
   }
   ```

---

## Troubleshooting

### Common Issues

#### Issue 1: LoadChartAsync Fails Silently

**Symptoms:**
- No error message is shown
- Chart does not update after calling LoadChartAsync

**Causes:**
- Invalid JSON format
- Data from incompatible Chart Wizard version
- Null or empty string passed to LoadChartAsync

**Solutions:**
```csharp
// Validate before loading
private async Task SafeLoadChart(string? data)
{
    if (ChartWizard == null)
    {
        Console.WriteLine("ChartWizard reference is null");
        return;
    }
    
    if (string.IsNullOrWhiteSpace(data))
    {
        Console.WriteLine("Serialized data is empty");
        return;
    }
    
    try
    {
        await ChartWizard.LoadChartAsync(data);
        Console.WriteLine("Chart loaded successfully");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Error loading chart: {ex.Message}");
    }
}
```

#### Issue 2: Chart State Not Preserved

**Symptoms:**
- Serialization completes but some properties are not restored
- Chart appears different after loading

**Causes:**
- Loading chart before data is bound
- Data source changes after serialization

**Solutions:**
```csharp
// Ensure data is loaded before restoring configuration
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender && !string.IsNullOrEmpty(savedConfig))
    {
        // Wait for component to fully initialize
        await Task.Delay(100);
        await ChartWizard.LoadChartAsync(savedConfig);
    }
}
```

#### Issue 3: SaveChart Returns Empty or Invalid JSON

**Symptoms:**
- SaveChart returns empty string or incomplete JSON
- Cannot deserialize saved data

**Causes:**
- Chart not fully initialized
- Called too early in component lifecycle

**Solutions:**
```csharp
// Ensure chart is ready before serializing
private async Task EnsureChartReady()
{
    await Task.Delay(500); // Allow initialization to complete
}

private async Task SaveChartAsync()
{
    if(ChartWizard != null)
    {
        await EnsureChartReady();
        serializedString = ChartWizard.SaveChart();
        
        if (string.IsNullOrEmpty(serializedString))
        {
            Console.WriteLine("Warning: Serialization returned empty data");
        }
    }
}
```

#### Issue 4: Browser Storage Limitations

**Symptoms:**
- Save operation fails for large configurations
- "QuotaExceededError" in browser console

**Causes:**
- localStorage has size limits (typically 5-10MB)
- Large datasets or complex configurations

**Solutions:**
```csharp
// Compress data before storing
private async Task SaveCompressed()
{
    if(ChartWizard != null)
    {
        string serializedData = ChartWizard.SaveChart();
        
        // Check size
        int sizeInBytes = System.Text.Encoding.UTF8.GetByteCount(serializedData);
        Console.WriteLine($"Serialized data size: {sizeInBytes / 1024}KB");
        
        if (sizeInBytes > 4 * 1024 * 1024) // 4MB threshold
        {
            // Use server-side storage instead
            await SaveToDatabase();
        }
        else
        {
            await JSRuntime.InvokeVoidAsync("localStorage.setItem", "chartConfig", serializedData);
        }
    }
}
```

#### Issue 5: Version Compatibility

**Symptoms:**
- Serialized data from older version fails to load
- New features not preserved

**Causes:**
- Schema changes between versions
- Deprecated properties

**Solutions:**
```csharp
// Store version information
public class VersionedChartConfig
{
    public string Version { get; set; } = "1.0";
    public string ComponentVersion { get; set; } = "25.1.35"; // Syncfusion version
    public string SerializedData { get; set; }
}

private async Task SaveWithVersion()
{
    if(ChartWizard != null)
    {
        var config = new VersionedChartConfig
        {
            SerializedData = ChartWizard.SaveChart(),
            ComponentVersion = SfChartWizard.Version // If available
        };
        
        string json = JsonSerializer.Serialize(config);
        await File.WriteAllTextAsync("config.json", json);
    }
}

private async Task LoadWithVersionCheck(string filePath)
{
    string json = await File.ReadAllTextAsync(filePath);
    var config = JsonSerializer.Deserialize<VersionedChartConfig>(json);
    
    if (config != null && ChartWizard != null)
    {
        // Check compatibility
        if (IsCompatibleVersion(config.ComponentVersion))
        {
            await ChartWizard.LoadChartAsync(config.SerializedData);
        }
        else
        {
            Console.WriteLine("Incompatible version detected");
        }
    }
}

private bool IsCompatibleVersion(string savedVersion)
{
    // Implement version compatibility logic
    return true;
}
```
