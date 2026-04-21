# Component Integration and Global Patterns

## Table of Contents
- [Global Reuse with MainLayout](#global-reuse-with-mainlayout)
- [Cascading Values and Parameters](#cascading-values-and-parameters)
- [Data Grid Integration](#data-grid-integration)
- [Multi-Component Responsive Patterns](#multi-component-responsive-patterns)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Global Reuse with MainLayout

Wrap your entire application in Media Query using `MainLayout.razor` to provide responsive behavior to all pages without duplicating the component.

### Step 1: Update MainLayout.razor

```razor
@inherits LayoutComponentBase

<div class="page">
    <div class="sidebar">
        <NavMenu />
    </div>
    <main>
        <div class="top-row px-4">
            <a href="https://docs.microsoft.com/aspnet/" target="_blank">About</a>
        </div>
        <article class="content px-4">
            <!-- Wrap Body in CascadingValue -->
            <CascadingValue Value="@activeBreakPoint">
                <SfMediaQuery @bind-ActiveBreakPoint="activeBreakPoint"></SfMediaQuery>
                @Body
            </CascadingValue>
        </article>
    </main>
</div>

@code {
    [Parameter]
    public string activeBreakPoint { get; set; }
}
```

**Key Points:**
- `CascadingValue` wraps the `SfMediaQuery` component
- `@Body` is inside the cascading value provider
- `activeBreakPoint` parameter cascades to all child components

### Step 2: Use in Child Pages

```razor
<!-- Pages/Home.razor -->

@page "/"

<h1>Home Page</h1>
<p>Active Breakpoint: @activeBreakPoint</p>

@if (activeBreakPoint == "Small")
{
    <MobileHomeLayout />
}
else
{
    <DesktopHomeLayout />
}

@code {
    [CascadingParameter]
    public string activeBreakPoint { get; set; }
}
```

```razor
<!-- Pages/Counter.razor -->

@page "/counter"

<h1>Counter Page</h1>
<p>Breakpoint: @activeBreakPoint</p>

@code {
    [CascadingParameter]
    public string activeBreakPoint { get; set; }
}
```

**Benefit:** Every page automatically receives the current breakpoint without adding `SfMediaQuery` repeatedly.

## Cascading Values and Parameters

Cascading values enable parent components to pass data to nested child components automatically.

### Basic Pattern

```razor
<!-- Parent Component -->
<CascadingValue Value="@breakpoint">
    <Child />
    <AnotherChild />
</CascadingValue>

@code {
    private string breakpoint = "Large";
}

<!-- Child Component -->
@code {
    [CascadingParameter]
    public string breakpoint { get; set; }
}
```

### Multiple Cascading Values

```razor
<!-- MainLayout.razor -->
<CascadingValue Value="@activeBreakPoint" Name="Breakpoint">
    <CascadingValue Value="@theme" Name="Theme">
        <SfMediaQuery @bind-ActiveBreakPoint="activeBreakPoint"></SfMediaQuery>
        @Body
    </CascadingValue>
</CascadingValue>

@code {
    private string activeBreakPoint { get; set; }
    private string theme = "light";
}

<!-- Child Component -->
@code {
    [CascadingParameter(Name = "Breakpoint")]
    public string Breakpoint { get; set; }
    
    [CascadingParameter(Name = "Theme")]
    public string Theme { get; set; }
}
```

### Optional Cascading Parameters

```razor
@code {
    [CascadingParameter]
    public string Breakpoint { get; set; } = "Large"; // Default value
}
```

## Data Grid Integration

Integrate Media Query with SfGrid for responsive table layouts.

### Responsive Column Visibility

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Grids

<SfMediaQuery @bind-ActiveBreakPoint="breakpoint"></SfMediaQuery>

<h3>Orders - Breakpoint: @breakpoint</h3>

<SfGrid DataSource="@orders" AllowSorting="true" AllowFiltering="true">
    <GridColumns>
        <!-- Always visible -->
        <GridColumn Field="@nameof(Order.OrderID)" HeaderText="Order ID" Width="80"></GridColumn>
        <GridColumn Field="@nameof(Order.CustomerID)" HeaderText="Customer"></GridColumn>
        
        <!-- Hidden on small screens -->
        @if (breakpoint != "Small")
        {
            <GridColumn Field="@nameof(Order.OrderDate)" HeaderText="Date" Format="d"></GridColumn>
            <GridColumn Field="@nameof(Order.ShipCity)" HeaderText="City"></GridColumn>
        }
        
        <!-- Hidden on small/medium screens -->
        @if (breakpoint == "Large")
        {
            <GridColumn Field="@nameof(Order.Amount)" HeaderText="Amount" Format="C2"></GridColumn>
        }
    </GridColumns>
</SfGrid>

@code {
    private string breakpoint { get; set; }
    private List<Order> orders { get; set; }

    protected override void OnInitialized()
    {
        orders = Enumerable.Range(1, 30).Select(x => new Order
        {
            OrderID = 1000 + x,
            CustomerID = "CUST" + x,
            OrderDate = DateTime.Now.AddDays(-x),
            ShipCity = "CityName",
            Amount = 100 + (x * 10)
        }).ToList();
    }

    public class Order
    {
        public int OrderID { get; set; }
        public string CustomerID { get; set; }
        public DateTime OrderDate { get; set; }
        public string ShipCity { get; set; }
        public decimal Amount { get; set; }
    }
}
```

### Adaptive Row Rendering

```razor
@{
    var renderingMode = RowDirection.Horizontal;
    var enableAdaptiveUI = false;

    if (breakpoint == "Small")
    {
        enableAdaptiveUI = true;
        renderingMode = RowDirection.Vertical;
    }
    else if (breakpoint == "Medium")
    {
        enableAdaptiveUI = true;
    }
}

<SfGrid DataSource="@orders" 
        EnableAdaptiveUI="@enableAdaptiveUI"
        RowRenderingMode="@renderingMode"
        AllowPaging="true">
    <GridPageSettings PageSize="@PageSize"></GridPageSettings>
    <!-- Columns definition -->
</SfGrid>

@code {
    private int PageSize => breakpoint == "Small" ? 5 : 10;
}
```

## Multi-Component Responsive Patterns

### Dashboard with Responsive Widget Layout

```razor
@using Syncfusion.Blazor

<SfMediaQuery @bind-ActiveBreakPoint="breakpoint"></SfMediaQuery>

<div class="@DashboardClass">
    <div class="widget widget-large">Widget 1</div>
    @if (breakpoint != "Small")
    {
        <div class="widget @WidgetSizeClass">Widget 2</div>
        <div class="widget @WidgetSizeClass">Widget 3</div>
    }
</div>

@code {
    private string breakpoint { get; set; }
    
    private string DashboardClass => breakpoint switch
    {
        "Small" => "grid-1-column",
        "Medium" => "grid-2-columns",
        _ => "grid-3-columns"
    };
    
    private string WidgetSizeClass => breakpoint == "Medium" ? "widget-medium" : "widget-small";
}

<style>
    .grid-1-column { display: grid; grid-template-columns: 1fr; gap: 20px; }
    .grid-2-columns { display: grid; grid-template-columns: repeat(2, 1fr); gap: 20px; }
    .grid-3-columns { display: grid; grid-template-columns: repeat(3, 1fr); gap: 20px; }
    
    .widget { padding: 20px; border: 1px solid #ddd; }
    .widget-large { grid-column: span 2; }
    .widget-medium { grid-column: span 1; }
    .widget-small { grid-column: span 1; }
</style>
```

### Side-by-Side and Stacked Layouts

```razor
<SfMediaQuery @bind-ActiveBreakPoint="bp"></SfMediaQuery>

<div class="@LayoutClass">
    <aside class="sidebar">
        <SidebarContent />
    </aside>
    <main class="main-content">
        <MainContent />
    </main>
</div>

@code {
    private string bp { get; set; }
    
    private string LayoutClass => bp == "Small" ? "layout-stacked" : "layout-side-by-side";
}

<style>
    .layout-stacked {
        display: flex;
        flex-direction: column;
    }
    
    .layout-side-by-side {
        display: flex;
        flex-direction: row;
    }
    
    .layout-side-by-side .sidebar {
        width: 250px;
        margin-right: 20px;
    }
    
    .layout-side-by-side .main-content {
        flex: 1;
    }
</style>
```

## Best Practices

### Practice 1: Provide Default Breakpoint Values
Always initialize the breakpoint to prevent null reference exceptions:

```razor
private string activeBreakpoint { get; set; } = "Large";
```

### Practice 2: Use Const for Breakpoint Names
Define breakpoint constants to avoid typos:

```razor
@code {
    private const string BP_SMALL = "Small";
    private const string BP_MEDIUM = "Medium";
    private const string BP_LARGE = "Large";
    
    @if (breakpoint == BP_SMALL) { }
}
```

### Practice 3: Separate Layout Components
Create dedicated components for each breakpoint layout:

```razor
<!-- MobileLayout.razor -->
<div class="mobile-layout">Mobile specific UI</div>

<!-- DesktopLayout.razor -->
<div class="desktop-layout">Desktop specific UI</div>

<!-- Main component uses them -->
@if (breakpoint == "Small")
{
    <MobileLayout />
}
else
{
    <DesktopLayout />
}
```

### Practice 4: Test Across Real Devices
Use browser dev tools to simulate different screen sizes, but also test on actual devices for real-world behavior.

## Troubleshooting

### Issue 1: Breakpoint Not Updating
**Problem:** ActiveBreakpoint shows initial value but doesn't change on resize.

**Solution:** Ensure two-way binding is correctly implemented:
```razor
<!-- Correct -->
<SfMediaQuery @bind-ActiveBreakPoint="breakpoint"></SfMediaQuery>

<!-- Wrong -->
<SfMediaQuery ActiveBreakPoint="@breakpoint"></SfMediaQuery>
```

### Issue 2: CascadingParameter Not Received
**Problem:** Child component doesn't receive the cascading value.

**Solution:** Ensure the parameter name matches exactly:
```razor
<!-- Parent -->
<CascadingValue Value="@breakpoint">

<!-- Child - must match parameter name -->
[CascadingParameter]
public string breakpoint { get; set; }
```

### Issue 3: Component Flicker on Resize
**Problem:** UI flickers when crossing breakpoint thresholds.

**Solution:** Use CSS transitions and avoid rapid component swaps:
```razor
<div style="transition: all 0.3s ease;">
    @if (breakpoint == "Small")
    {
        <MobileView />
    }
    else
    {
        <DesktopView />
    }
</div>
```

### Issue 4: Performance Issues with Heavy Components
**Problem:** Rendering multiple heavy components for each breakpoint causes lag.

**Solution:** Use lazy loading or conditional rendering:
```razor
<!-- Bad - renders all layouts -->
<MobileLayout />
<DesktopLayout />

<!-- Good - renders only needed layout -->
@if (breakpoint == "Small")
{
    <MobileLayout />
}
else
{
    <DesktopLayout />
}
```

### Issue 5: SSR Compatibility
**Problem:** Media Query doesn't work on initial server-side render.

**Solution:** Add `@rendermode="InteractiveServer"` to App.razor (Blazor 8+):
```html
<!-- ~/Components/App.razor -->
<body>
    <Routes @rendermode="InteractiveServer" />
</body>
```

This enables client-side interactivity where Media Query can detect viewport size.
