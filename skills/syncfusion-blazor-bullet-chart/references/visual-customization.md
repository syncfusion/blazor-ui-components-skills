````markdown
# Visual Customization

This guide covers comprehensive visual customization for the Bullet Chart, including dimensions, themes, titles, borders, colors, and responsive design patterns.

## Table of Contents
- [Chart Dimensions](#chart-dimensions)
- [Theme Selection](#theme-selection)
- [Title and Subtitle](#title-and-subtitle)
- [Border and Background](#border-and-background)
- [Margin and Container](#margin-and-container)
- [Color Palette Customization](#color-palette-customization)
- [Animation](#animation)
- [Responsive Design](#responsive-design)

## Chart Dimensions

### Width and Height

Control chart size using pixel or percentage values:

```razor
@using Syncfusion.Blazor.Charts

<!-- Fixed pixel dimensions -->
<SfBulletChart DataSource="@RevenueData" 
               ValueField="Revenue" 
               TargetField="Target" 
               Width="800px"
               Height="150px"
               Minimum="0" 
               Maximum="300">
    <BulletChartRangeCollection>
        <BulletChartRange End="150"></BulletChartRange>
        <BulletChartRange End="250"></BulletChartRange>
        <BulletChartRange End="300"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

<!-- Percentage-based dimensions -->
<SfBulletChart DataSource="@RevenueData" 
               ValueField="Revenue" 
               TargetField="Target" 
               Width="100%"
               Height="120px"
               Minimum="0" 
               Maximum="300">
    <BulletChartRangeCollection>
        <BulletChartRange End="150"></BulletChartRange>
        <BulletChartRange End="250"></BulletChartRange>
        <BulletChartRange End="300"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class RevenueMetric
    {
        public double Revenue { get; set; }
        public double Target { get; set; }
    }
    
    public List<RevenueMetric> RevenueData = new List<RevenueMetric>
    {
        new RevenueMetric { Revenue = 270, Target = 250 }
    };
}
```

### Dimension Guidelines

| Chart Type | Recommended Width | Recommended Height | Use Case |
|------------|------------------|-------------------|----------|
| Single Horizontal | 600-800px | 100-150px | Dashboard card |
| Multiple Categories | 700-900px | 300-500px | Comparison view |
| Vertical Single | 150-250px | 300-400px | Narrow panel |
| Full-width | 100% | 120-180px | Responsive layout |

### Container-Based Sizing

Use CSS containers for responsive sizing:

```razor
@using Syncfusion.Blazor.Charts

<div class="bullet-chart-container">
    <SfBulletChart DataSource="@SalesData" 
                   ValueField="Sales" 
                   TargetField="Quota" 
                   Width="100%"
                   Height="140px"
                   Title="Sales Performance">
        <BulletChartRangeCollection>
            <BulletChartRange End="200"></BulletChartRange>
            <BulletChartRange End="350"></BulletChartRange>
            <BulletChartRange End="500"></BulletChartRange>
        </BulletChartRangeCollection>
    </SfBulletChart>
</div>

<style>
    .bullet-chart-container {
        width: 100%;
        max-width: 900px;
        margin: 0 auto;
        padding: 20px;
    }
</style>

@code {
    public class SalesMetric
    {
        public double Sales { get; set; }
        public double Quota { get; set; }
    }
    
    public List<SalesMetric> SalesData = new List<SalesMetric>
    {
        new SalesMetric { Sales = 380, Quota = 350 }
    };
}
```

## Theme Selection

### Built-in Themes

Syncfusion provides 18+ professional themes. Use the `Theme` property:

```razor
@using Syncfusion.Blazor.Charts
@using Syncfusion.Blazor

<!-- Material Theme -->
<SfBulletChart DataSource="@PerformanceData" 
               Theme="Theme.Material"
               ValueField="Score" 
               TargetField="Goal" 
               Title="Material Theme">
    <BulletChartRangeCollection>
        <BulletChartRange End="40"></BulletChartRange>
        <BulletChartRange End="70"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

<!-- Bootstrap 5 Theme -->
<SfBulletChart DataSource="@PerformanceData" 
               Theme="Theme.Bootstrap5"
               ValueField="Score" 
               TargetField="Goal" 
               Title="Bootstrap 5 Theme">
    <BulletChartRangeCollection>
        <BulletChartRange End="40"></BulletChartRange>
        <BulletChartRange End="70"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class PerformanceMetric
    {
        public double Score { get; set; }
        public double Goal { get; set; }
    }
    
    public List<PerformanceMetric> PerformanceData = new List<PerformanceMetric>
    {
        new PerformanceMetric { Score = 75, Goal = 70 }
    };
}
```

### Available Themes

**Light Themes:**
- `Theme.Bootstrap5` - Bootstrap 5 styling
- `Theme.Material` - Google Material Design
- `Theme.Fabric` - Microsoft Fabric
- `Theme.Fluent` - Microsoft Fluent Design
- `Theme.Tailwind` - Tailwind CSS
- `Theme.Material3` - Material Design 3
- `Theme.Fluent2` - Fluent 2 Design

**Dark Themes:**
- `Theme.Bootstrap5Dark`
- `Theme.MaterialDark`
- `Theme.FabricDark`
- `Theme.FluentDark`
- `Theme.TailwindDark`
- `Theme.Material3Dark`
- `Theme.Fluent2Dark`

**High Contrast:**
- `Theme.HighContrast`
- `Theme.HighContrastLight`

### Theme Selection Based on Context

```razor
@using Syncfusion.Blazor.Charts
@using Syncfusion.Blazor

<div class="theme-selector">
    <label for="themeSelect">Select Theme:</label>
    <select id="themeSelect" @onchange="OnThemeChange" class="form-select">
        <option value="@Theme.Bootstrap5">Bootstrap 5</option>
        <option value="@Theme.Material">Material</option>
        <option value="@Theme.Fluent">Fluent</option>
        <option value="@Theme.Tailwind">Tailwind</option>
        <option value="@Theme.Bootstrap5Dark">Bootstrap 5 Dark</option>
        <option value="@Theme.MaterialDark">Material Dark</option>
        <option value="@Theme.FluentDark">Fluent Dark</option>
    </select>
</div>

<SfBulletChart DataSource="@RevenueData" 
               Theme="@SelectedTheme"
               ValueField="Revenue" 
               TargetField="Target" 
               Title="Revenue Performance">
    <BulletChartRangeCollection>
        <BulletChartRange End="150"></BulletChartRange>
        <BulletChartRange End="250"></BulletChartRange>
        <BulletChartRange End="300"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public Theme SelectedTheme { get; set; } = Theme.Bootstrap5;
    
    public class RevenueMetric
    {
        public double Revenue { get; set; }
        public double Target { get; set; }
    }
    
    public List<RevenueMetric> RevenueData = new List<RevenueMetric>
    {
        new RevenueMetric { Revenue = 270, Target = 250 }
    };
    
    private void OnThemeChange(Microsoft.AspNetCore.Components.ChangeEventArgs e)
    {
        if (Enum.TryParse<Theme>(e.Value.ToString(), out var theme))
        {
            SelectedTheme = theme;
        }
    }
}
```

### Theme Recommendation by Industry

| Industry/Context | Recommended Theme | Rationale |
|------------------|------------------|-----------|
| Financial Services | Bootstrap 5 | Professional, familiar |
| Healthcare | Material | Clean, accessible |
| Technology Startups | Fluent | Modern, sleek |
| E-commerce | Tailwind | Customizable, trendy |
| Enterprise Apps | Bootstrap 5 | Consistent with corporate sites |
| Dark Mode Apps | MaterialDark or FluentDark | Eye comfort |

## Title and Subtitle

### Basic Title Configuration

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               Title="Q1 2026 Sales Performance"
               Subtitle="North America Region">
    <BulletChartRangeCollection>
        <BulletChartRange End="200"></BulletChartRange>
        <BulletChartRange End="350"></BulletChartRange>
        <BulletChartRange End="500"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class SalesMetric
    {
        public double Sales { get; set; }
        public double Quota { get; set; }
    }
    
    public List<SalesMetric> SalesData = new List<SalesMetric>
    {
        new SalesMetric { Sales = 380, Quota = 350 }
    };
}
```

### Title Style Customization

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               ValueField="Score" 
               TargetField="Goal" 
               Title="Employee Performance Metrics">
    <BulletChartTitleStyle FontFamily="Arial, sans-serif"
                           FontStyle="italic"
                           FontWeight="bold"
                           Color="#1976d2"
                           Size="20px"
                           TextAlignment="Syncfusion.Blazor.Charts.Alignment.Far"
                           TextOverflow="Syncfusion.Blazor.Charts.TextOverflow.Wrap"></BulletChartTitleStyle>
    <BulletChartRangeCollection>
        <BulletChartRange End="40"></BulletChartRange>
        <BulletChartRange End="70"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class PerformanceMetric
    {
        public double Score { get; set; }
        public double Goal { get; set; }
    }
    
    public List<PerformanceMetric> PerformanceData = new List<PerformanceMetric>
    {
        new PerformanceMetric { Score = 85, Goal = 75 }
    };
}
```

**TextAlignment Options:**
- `Near` - Left-aligned
- `Center` - Center-aligned
- `Far` - Right-aligned

### Subtitle Customization

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@RevenueData" 
               ValueField="Revenue" 
               TargetField="Target" 
               Title="Quarterly Revenue"
               Subtitle="in thousands USD">
    <BulletChartTitleStyle Size="18px" Color="#333" FontWeight="600"></BulletChartTitleStyle>
    <BulletChartSubTitleStyle Size="13px" Color="#666" FontStyle="italic"></BulletChartSubTitleStyle>
    <BulletChartRangeCollection>
        <BulletChartRange End="150"></BulletChartRange>
        <BulletChartRange End="250"></BulletChartRange>
        <BulletChartRange End="300"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class RevenueMetric
    {
        public double Revenue { get; set; }
        public double Target { get; set; }
    }
    
    public List<RevenueMetric> RevenueData = new List<RevenueMetric>
    {
        new RevenueMetric { Revenue = 270, Target = 250 }
    };
}
```

## Border and Background

### Chart Border

Add a border around the entire chart:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               Title="Sales Performance">
    <BulletChartBorder Color="#1976d2" Width="3"></BulletChartBorder>
    <BulletChartRangeCollection>
        <BulletChartRange End="200"></BulletChartRange>
        <BulletChartRange End="350"></BulletChartRange>
        <BulletChartRange End="500"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class SalesMetric
    {
        public double Sales { get; set; }
        public double Quota { get; set; }
    }
    
    public List<SalesMetric> SalesData = new List<SalesMetric>
    {
        new SalesMetric { Sales = 380, Quota = 350 }
    };
}
```

### Background Color

Set the chart background color:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               ValueField="Score" 
               TargetField="Goal" 
               Background="#f5f5f5"
               Title="Performance Score">
    <BulletChartBorder Color="#e0e0e0" Width="1"></BulletChartBorder>
    <BulletChartRangeCollection>
        <BulletChartRange End="40"></BulletChartRange>
        <BulletChartRange End="70"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class PerformanceMetric
    {
        public double Score { get; set; }
        public double Goal { get; set; }
    }
    
    public List<PerformanceMetric> PerformanceData = new List<PerformanceMetric>
    {
        new PerformanceMetric { Score = 75, Goal = 70 }
    };
}
```

### Container Customization

```razor
@using Syncfusion.Blazor.Charts

<div class="custom-chart-container">
    <SfBulletChart DataSource="@RevenueData" 
                   ValueField="Revenue" 
                   TargetField="Target" 
                   Background="transparent"
                   Title="Revenue Overview">
        <BulletChartRangeCollection>
            <BulletChartRange End="150"></BulletChartRange>
            <BulletChartRange End="250"></BulletChartRange>
            <BulletChartRange End="300"></BulletChartRange>
        </BulletChartRangeCollection>
    </SfBulletChart>
</div>

<style>
    .custom-chart-container {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        padding: 30px;
        border-radius: 12px;
        box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
    }
</style>

@code {
    public class RevenueMetric
    {
        public double Revenue { get; set; }
        public double Target { get; set; }
    }
    
    public List<RevenueMetric> RevenueData = new List<RevenueMetric>
    {
        new RevenueMetric { Revenue = 270, Target = 250 }
    };
}
```

## Margin and Container

### Margin Configuration

Control spacing around the chart content:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               Title="Sales Performance">
    <BulletChartMargin Left="40" Right="40" Top="30" Bottom="30"></BulletChartMargin>
    <BulletChartRangeCollection>
        <BulletChartRange End="200"></BulletChartRange>
        <BulletChartRange End="350"></BulletChartRange>
        <BulletChartRange End="500"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class SalesMetric
    {
        public double Sales { get; set; }
        public double Quota { get; set; }
    }
    
    public List<SalesMetric> SalesData = new List<SalesMetric>
    {
        new SalesMetric { Sales = 380, Quota = 350 }
    };
}
```

### Vertical Chart with Adjusted Margins

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               Orientation="OrientationType.Vertical"
               Width="25%"
               Height="400"
               ValueField="Score" 
               TargetField="Goal" 
               Title="Performance">
    <BulletChartMargin Left="60" Right="20" Top="40" Bottom="40"></BulletChartMargin>
    <BulletChartRangeCollection>
        <BulletChartRange End="40"></BulletChartRange>
        <BulletChartRange End="70"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class PerformanceMetric
    {
        public double Score { get; set; }
        public double Goal { get; set; }
    }
    
    public List<PerformanceMetric> PerformanceData = new List<PerformanceMetric>
    {
        new PerformanceMetric { Score = 75, Goal = 70 }
    };
}
```

## Color Palette Customization

### Range Colors

Customize individual range colors:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@RevenueData" 
               ValueField="Revenue" 
               TargetField="Target" 
               Title="Revenue with Custom Colors">
    <BulletChartRangeCollection>
        <BulletChartRange End="150" Color="#ffebee" Opacity="0.8"></BulletChartRange>
        <BulletChartRange End="250" Color="#fff3e0" Opacity="0.8"></BulletChartRange>
        <BulletChartRange End="300" Color="#e8f5e9" Opacity="0.8"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class RevenueMetric
    {
        public double Revenue { get; set; }
        public double Target { get; set; }
    }
    
    public List<RevenueMetric> RevenueData = new List<RevenueMetric>
    {
        new RevenueMetric { Revenue = 270, Target = 250 }
    };
}
```

### Color Schemes by Theme

**Professional Business:**
```razor
<BulletChartRangeCollection>
    <BulletChartRange End="40" Color="#c62828"></BulletChartRange>
    <BulletChartRange End="70" Color="#f9a825"></BulletChartRange>
    <BulletChartRange End="100" Color="#2e7d32"></BulletChartRange>
</BulletChartRangeCollection>
```

**Soft Pastels:**
```razor
<BulletChartRangeCollection>
    <BulletChartRange End="40" Color="#ffcdd2" Opacity="0.9"></BulletChartRange>
    <BulletChartRange End="70" Color="#fff9c4" Opacity="0.9"></BulletChartRange>
    <BulletChartRange End="100" Color="#c8e6c9" Opacity="0.9"></BulletChartRange>
</BulletChartRangeCollection>
```

**Monochromatic Blue:**
```razor
<BulletChartRangeCollection>
    <BulletChartRange End="33" Color="#e3f2fd"></BulletChartRange>
    <BulletChartRange End="66" Color="#90caf9"></BulletChartRange>
    <BulletChartRange End="100" Color="#1976d2"></BulletChartRange>
</BulletChartRangeCollection>
```

**High Contrast (Accessibility):**
```razor
<BulletChartRangeCollection>
    <BulletChartRange End="40" Color="#d50000" Opacity="1"></BulletChartRange>
    <BulletChartRange End="70" Color="#ff6f00" Opacity="1"></BulletChartRange>
    <BulletChartRange End="100" Color="#00c853" Opacity="1"></BulletChartRange>
</BulletChartRangeCollection>
```

### Actual and Target Bar Colors

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota">
    <BulletChartValueBorder Color="#1976d2" Width="2"></BulletChartValueBorder>
    <BulletChartTargetBorder Color="#d32f2f" Width="3"></BulletChartTargetBorder>
    <BulletChartRangeCollection>
        <BulletChartRange End="200"></BulletChartRange>
        <BulletChartRange End="350"></BulletChartRange>
        <BulletChartRange End="500"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class SalesMetric
    {
        public double Sales { get; set; }
        public double Quota { get; set; }
    }
    
    public List<SalesMetric> SalesData = new List<SalesMetric>
    {
        new SalesMetric { Sales = 380, Quota = 350 }
    };
}
```

## Animation

### Enable Animation

Add smooth animation when the chart loads:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               ValueField="Score" 
               TargetField="Goal" 
               Title="Animated Chart">
    <BulletChartAnimation Enable="true" Duration="2000" Delay="0"></BulletChartAnimation>
    <BulletChartRangeCollection>
        <BulletChartRange End="40"></BulletChartRange>
        <BulletChartRange End="70"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class PerformanceMetric
    {
        public double Score { get; set; }
        public double Goal { get; set; }
    }
    
    public List<PerformanceMetric> PerformanceData = new List<PerformanceMetric>
    {
        new PerformanceMetric { Score = 75, Goal = 70 }
    };
}
```

**Properties:**
- `Enable` - Turn animation on/off (default: true)
- `Duration` - Animation length in milliseconds (default: 1000)
- `Delay` - Delay before animation starts (default: 0)

### Disable Animation

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData">
    <BulletChartAnimation Enable="false"></BulletChartAnimation>
    <!-- Chart configuration -->
</SfBulletChart>
```

**When to disable:** Real-time dashboards with frequent updates.

## Responsive Design

### Percentage-Based Responsive Layout

```razor
@using Syncfusion.Blazor.Charts

<div class="container-fluid">
    <div class="row">
        <div class="col-12 col-lg-6 mb-4">
            <SfBulletChart DataSource="@SalesData" 
                           ValueField="Sales" 
                           TargetField="Quota" 
                           Width="100%"
                           Height="140px"
                           Title="Sales Performance">
                <BulletChartRangeCollection>
                    <BulletChartRange End="200"></BulletChartRange>
                    <BulletChartRange End="350"></BulletChartRange>
                    <BulletChartRange End="500"></BulletChartRange>
                </BulletChartRangeCollection>
            </SfBulletChart>
        </div>
        <div class="col-12 col-lg-6 mb-4">
            <SfBulletChart DataSource="@RevenueData" 
                           ValueField="Revenue" 
                           TargetField="Target" 
                           Width="100%"
                           Height="140px"
                           Title="Revenue Performance">
                <BulletChartRangeCollection>
                    <BulletChartRange End="150"></BulletChartRange>
                    <BulletChartRange End="250"></BulletChartRange>
                    <BulletChartRange End="300"></BulletChartRange>
                </BulletChartRangeCollection>
            </SfBulletChart>
        </div>
    </div>
</div>
```

### Media Query-Based Sizing

```razor
@using Syncfusion.Blazor.Charts

<div class="responsive-chart-container">
    <SfBulletChart DataSource="@PerformanceData" 
                   ValueField="Score" 
                   TargetField="Goal" 
                   Width="100%"
                   Height="@chartHeight"
                   Title="Performance Score">
        <BulletChartRangeCollection>
            <BulletChartRange End="40"></BulletChartRange>
            <BulletChartRange End="70"></BulletChartRange>
            <BulletChartRange End="100"></BulletChartRange>
        </BulletChartRangeCollection>
    </SfBulletChart>
</div>

<style>
    .responsive-chart-container {
        width: 100%;
    }
    
    @@media (max-width: 768px) {
        .responsive-chart-container {
            padding: 10px;
        }
    }
</style>

@code {
    private string chartHeight = "150px";
    
    public class PerformanceMetric
    {
        public double Score { get; set; }
        public double Goal { get; set; }
    }
    
    public List<PerformanceMetric> PerformanceData = new List<PerformanceMetric>
    {
        new PerformanceMetric { Score = 75, Goal = 70 }
    };
    
    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            // Adjust height based on screen size (example using JS interop)
            // chartHeight = await JSRuntime.InvokeAsync<string>("getResponsiveHeight");
            StateHasChanged();
        }
    }
}
```

### Mobile-First Dashboard

```razor
@using Syncfusion.Blazor.Charts

<div class="mobile-dashboard">
    @foreach (var kpi in KPIData)
    {
        <div class="kpi-card">
            <SfBulletChart DataSource="@(new List<KPIMetric> { kpi })" 
                           ValueField="Value" 
                           TargetField="Target" 
                           Width="100%"
                           Height="120px"
                           Title="@kpi.Name">
                <BulletChartTitleStyle Size="16px"></BulletChartTitleStyle>
                <BulletChartMargin Left="20" Right="20" Top="25" Bottom="25"></BulletChartMargin>
                <BulletChartRangeCollection>
                    <BulletChartRange End="40"></BulletChartRange>
                    <BulletChartRange End="70"></BulletChartRange>
                    <BulletChartRange End="100"></BulletChartRange>
                </BulletChartRangeCollection>
            </SfBulletChart>
        </div>
    }
</div>

<style>
    .mobile-dashboard {
        display: flex;
        flex-direction: column;
        gap: 15px;
        padding: 15px;
    }
    
    .kpi-card {
        background: white;
        border-radius: 8px;
        padding: 15px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    }
    
    @@media (min-width: 768px) {
        .mobile-dashboard {
            flex-direction: row;
            flex-wrap: wrap;
        }
        
        .kpi-card {
            flex: 1 1 calc(50% - 15px);
        }
    }
    
    @@media (min-width: 1200px) {
        .kpi-card {
            flex: 1 1 calc(33.333% - 15px);
        }
    }
</style>

@code {
    public class KPIMetric
    {
        public string Name { get; set; }
        public double Value { get; set; }
        public double Target { get; set; }
    }
    
    public List<KPIMetric> KPIData = new List<KPIMetric>
    {
        new KPIMetric { Name = "Sales", Value = 88, Target = 85 },
        new KPIMetric { Name = "Revenue", Value = 92, Target = 90 },
        new KPIMetric { Name = "Satisfaction", Value = 76, Target = 80 }
    };
}
```

## Best Practices

### 1. Theme Consistency

- Use the same theme across all charts in your application
- Match theme with your application's overall design system
- Test dark themes in actual dark environments

### 2. Color Accessibility

- Ensure sufficient contrast (WCAG AA: 4.5:1 minimum)
- Don't rely solely on color to convey information
- Test with colorblind simulators
- Use patterns or labels in addition to colors

### 3. Dimension Guidelines

**Desktop:**
- Minimum width: 400px
- Ideal width: 600-900px
- Height: 100-180px (single), 300-500px (multiple)

**Mobile:**
- Full width: 100%
- Height: 100-140px
- Adequate touch target size (44x44px minimum)

### 4. Performance

- Disable animation for frequently updating charts
- Use CSS classes instead of inline styles
- Optimize container queries
- Minimize margin/padding calculations

## Troubleshooting

### Issue: Chart not displaying correctly

**Solutions:**
1. Verify Width and Height are set
2. Check parent container has defined dimensions
3. Ensure theme CSS file is loaded
4. Inspect browser console for errors

### Issue: Title/subtitle truncated

**Solutions:**
1. Increase chart width
2. Reduce font size in title style
3. Use `TextOverflow="Wrap"` in title style
4. Shorten title text

### Issue: Colors not appearing as expected

**Solutions:**
1. Check Opacity property (0-1 range)
2. Verify Color property format (#RRGGBB or rgba())
3. Ensure theme doesn't override colors
4. Check browser color rendering settings

### Issue: Responsive layout breaks

**Solutions:**
1. Use Width="100%" instead of fixed pixels
2. Set max-width on container
3. Test at multiple breakpoints
4. Use Bootstrap grid or Flexbox
````