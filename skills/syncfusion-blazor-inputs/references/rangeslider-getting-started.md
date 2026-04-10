# RangeSlider - Getting Started

This guide covers the essential steps to implement the Syncfusion Blazor RangeSlider component in your Blazor application. The RangeSlider provides dual handles for selecting a range of values, perfect for price filters, date ranges, or any two-value selection scenario.

## Installation

### NuGet Package

Install the Syncfusion Blazor Inputs package:

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Blazor.Inputs
```

**CLI:**
```bash
dotnet add package Syncfusion.Blazor.Inputs
```

The Inputs package includes the SfSlider component used for both single-value and range sliders.

## Service Registration

### Blazor Server (Program.cs or Startup.cs)

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddSyncfusionBlazor();
// ... other services

var app = builder.Build();
```

### Blazor WebAssembly (Program.cs)

```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.Services.AddSyncfusionBlazor();
// ... other services

await builder.Build().RunAsync();
```

## Namespace Import

Add the namespace in your component file or `_Imports.razor`:

```razor
@using Syncfusion.Blazor.Inputs
```

## CSS Theme Configuration

### Individual Component CSS (Recommended for minimal bundle size)

Add to `wwwroot/index.html` (Blazor WebAssembly) or `Pages/_Host.cshtml` / `Pages/_Layout.cshtml` (Blazor Server):

```html
<head>
    <!-- Syncfusion Blazor Inputs Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
```

### Available Themes
- `bootstrap5.css` - Bootstrap 5
- `material.css` - Material Design
- `fabric.css` - Fluent Design
- `tailwind.css` - Tailwind CSS
- `bootstrap4.css` - Bootstrap 4
- `material-dark.css` - Material Dark
- `bootstrap-dark.css` - Bootstrap Dark
- `tailwind-dark.css` - Tailwind Dark

## Basic RangeSlider Implementation

### Minimal Example

```razor
@page "/rangeslider-demo"
@using Syncfusion.Blazor.Inputs

<div class="control-section">
    <h4>Basic Range Slider</h4>
    <SfSlider @bind-Value="@rangeValues" Type="SliderType.Range" Min="0" Max="100">
    </SfSlider>
    <p>Selected Range: @rangeValues[0] - @rangeValues[1]</p>
</div>

@code {
    private int[] rangeValues = new int[] { 30, 70 };
}
```

**Key Points:**
- `Type="SliderType.Range"` enables dual handles
- `@bind-Value` requires a two-element array (int[] or double[])
- `Min` and `Max` define the selectable range bounds
- Default orientation is horizontal

### Value Binding with Two Handles

The RangeSlider requires a two-element array for the Value property:

```csharp
// Integer range
private int[] priceRange = new int[] { 100, 500 };

// Double range for decimal values
private double[] temperatureRange = new double[] { 18.5, 24.5 };

// Initial values represent starting positions of left and right handles
// rangeValues[0] = left handle (start value)
// rangeValues[1] = right handle (end value)
```

## Complete Working Example

```razor
@page "/price-filter"
@using Syncfusion.Blazor.Inputs

<div class="price-filter-container">
    <h3>Filter Products by Price</h3>
    
    <div class="slider-wrapper">
        <label>Price Range</label>
        <SfSlider @bind-Value="@priceRange"
                  Type="SliderType.Range"
                  Min="0"
                  Max="1000"
                  Step="10">
        </SfSlider>
    </div>
    
    <div class="selected-range">
        <strong>Selected:</strong> $@priceRange[0] - $@priceRange[1]
    </div>
    
    <button class="btn btn-primary" @onclick="ApplyFilter">Apply Filter</button>
</div>

@code {
    private int[] priceRange = new int[] { 200, 800 };
    
    private void ApplyFilter()
    {
        Console.WriteLine($"Filtering products: ${priceRange[0]} - ${priceRange[1]}");
        // Apply filter logic here
    }
}
```

### CSS Styling (Optional)

```css
.price-filter-container {
    max-width: 600px;
    margin: 20px auto;
    padding: 20px;
    border: 1px solid #ddd;
    border-radius: 8px;
}

.slider-wrapper {
    margin: 20px 0;
}

.slider-wrapper label {
    display: block;
    margin-bottom: 10px;
    font-weight: 500;
}

.selected-range {
    text-align: center;
    margin: 15px 0;
    font-size: 16px;
}
```

## Common Patterns for Getting Started

### Pattern 1: Simple Range Selection
```razor
<SfSlider @bind-Value="@values" Type="SliderType.Range" Min="0" Max="100"></SfSlider>

@code {
    private int[] values = new int[] { 20, 80 };
}
```

### Pattern 2: With Step Increment
```razor
<SfSlider @bind-Value="@ageRange" 
          Type="SliderType.Range" 
          Min="18" 
          Max="65" 
          Step="5">
</SfSlider>

@code {
    private int[] ageRange = new int[] { 25, 45 };
}
```

### Pattern 3: Double Values for Decimals
```razor
<SfSlider @bind-Value="@tempRange" 
          Type="SliderType.Range" 
          Min="0.0" 
          Max="50.0" 
          Step="0.5">
</SfSlider>

@code {
    private double[] tempRange = new double[] { 15.5, 28.0 };
}
```

## Troubleshooting

### Value Binding Not Working
**Issue:** Handles don't move or values don't update.

**Solution:** Ensure Value is a two-element array:
```csharp
// ✅ Correct
private int[] values = new int[] { 10, 90 };

// ❌ Wrong - single value
private int value = 50;

// ❌ Wrong - empty array
private int[] values = new int[] { };
```

### Slider Not Visible
**Issue:** Component renders but is not visible.

**Solution:** 
1. Verify CSS theme is referenced in `index.html` or `_Host.cshtml`
2. Check browser console for 404 errors on CSS files
3. Ensure the correct theme path: `_content/Syncfusion.Blazor.Themes/bootstrap5.css`

### Type Property Required
**Issue:** Only one handle appears instead of two.

**Solution:** Always set `Type="SliderType.Range"` for dual handles:
```razor
<!-- ✅ Correct - Range Slider -->
<SfSlider Type="SliderType.Range" @bind-Value="@rangeValues"></SfSlider>

<!-- ❌ Wrong - Default Single Slider -->
<SfSlider @bind-Value="@rangeValues"></SfSlider>
```

### Min/Max Validation
**Issue:** Handles can be set to invalid positions.

**Solution:** Ensure initial values are within Min/Max bounds:
```csharp
// Min=0, Max=100
private int[] values = new int[] { 10, 90 };  // ✅ Valid

// ❌ Invalid - values outside bounds
private int[] values = new int[] { -10, 110 };
```

## Next Steps

After implementing a basic RangeSlider:
1. **Range Configuration** - Learn about Min, Max, Step, and custom values
2. **Ticks and Tooltips** - Add visual markers and value display
3. **Color Ranges** - Implement visual feedback with colored segments
4. **Limits** - Restrict handle movement within specific bounds
5. **Events** - Handle value changes and user interactions

## Related Components

- **Single Slider** - Use `SliderType.Default` for single-value selection
- **NumericTextBox** - Alternative for precise numeric input
- **DateRangePicker** - Specialized component for date range selection
