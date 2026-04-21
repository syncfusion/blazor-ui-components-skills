# Responsive Layout Patterns

## Table of Contents
- [Conditional Rendering](#conditional-rendering)
- [Dynamic Property Binding](#dynamic-property-binding)
- [Layout Adaptation Strategies](#layout-adaptation-strategies)
- [CSS Class Switching](#css-class-switching)
- [Performance Optimization](#performance-optimization)
- [Common Pitfalls](#common-pitfalls)

## Conditional Rendering

Render different content based on the current breakpoint. This is the most common responsive pattern.

### Simple If-Else Pattern

```razor
@using Syncfusion.Blazor

<SfMediaQuery @bind-ActiveBreakPoint="breakpoint"></SfMediaQuery>

@if (breakpoint == "Small")
{
    <MobileView />
}
else if (breakpoint == "Medium")
{
    <TabletView />
}
else
{
    <DesktopView />
}

@code {
    private string breakpoint { get; set; }
}
```

### Show/Hide Components

```razor
<SfMediaQuery @bind-ActiveBreakPoint="bp"></SfMediaQuery>

<header>
    @if (bp == "Small")
    {
        <button @onclick="ToggleSidebar">☰ Menu</button>
    }
    else
    {
        <nav>
            <a href="/">Home</a>
            <a href="/about">About</a>
            <a href="/contact">Contact</a>
        </nav>
    }
</header>

@code {
    private string bp { get; set; }
    
    private void ToggleSidebar() 
    { 
        // Toggle sidebar implementation
    }
}
```

### Grid Layout Adaptation

```razor
<SfMediaQuery @bind-ActiveBreakPoint="bp"></SfMediaQuery>

<div class="@GridClass">
    @for (int i = 0; i < 12; i++)
    {
        <div class="grid-item">Item @(i + 1)</div>
    }
</div>

@code {
    private string bp { get; set; }
    
    private string GridClass => bp switch
    {
        "Small" => "grid-1-column",
        "Medium" => "grid-2-columns",
        _ => "grid-3-columns"
    };
}

<style>
    .grid-1-column { display: grid; grid-template-columns: 1fr; }
    .grid-2-columns { display: grid; grid-template-columns: repeat(2, 1fr); }
    .grid-3-columns { display: grid; grid-template-columns: repeat(3, 1fr); }
</style>
```

## Dynamic Property Binding

Adjust component properties based on the breakpoint to optimize behavior for each device type.

### Data Grid Responsiveness

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Grids

<SfMediaQuery @bind-ActiveBreakPoint="breakpoint"></SfMediaQuery>

<h3>Active Breakpoint: @breakpoint</h3>

@{
    var pageSize = breakpoint == "Small" ? 5 : (breakpoint == "Medium" ? 10 : 25);
    var allowPaging = breakpoint != "Small";
    var enableAdaptiveUI = breakpoint != "Large";
}

<SfGrid DataSource="@orders" 
        EnableAdaptiveUI="@enableAdaptiveUI"
        AllowPaging="@allowPaging">
    <GridPageSettings PageSize="@pageSize"></GridPageSettings>
    <GridColumns>
        <GridColumn Field="@nameof(Order.OrderID)" HeaderText="ID" Width="80"></GridColumn>
        <GridColumn Field="@nameof(Order.CustomerID)" HeaderText="Customer"></GridColumn>
        <GridColumn Field="@nameof(Order.OrderDate)" HeaderText="Date" Format="d"></GridColumn>
        <GridColumn Field="@nameof(Order.Amount)" HeaderText="Amount" Format="C2"></GridColumn>
    </GridColumns>
</SfGrid>

@code {
    private string breakpoint { get; set; }
    private List<Order> orders { get; set; } = new();

    protected override void OnInitialized()
    {
        orders = Enumerable.Range(1, 50).Select(x => new Order
        {
            OrderID = 1000 + x,
            CustomerID = "CUST" + x,
            OrderDate = DateTime.Now.AddDays(-x),
            Amount = 100 + (x * 10)
        }).ToList();
    }

    public class Order
    {
        public int OrderID { get; set; }
        public string CustomerID { get; set; }
        public DateTime OrderDate { get; set; }
        public decimal Amount { get; set; }
    }
}
```

### List View with Variable Items Per Row

```razor
<SfMediaQuery @bind-ActiveBreakPoint="bp"></SfMediaQuery>

<div class="item-list" style="columns: @ColumnCount">
    @foreach (var item in items)
    {
        <div class="item-card">@item</div>
    }
</div>

@code {
    private string bp { get; set; }
    private List<string> items = Enumerable.Range(1, 20)
        .Select(i => $"Item {i}").ToList();
    
    private int ColumnCount => bp switch
    {
        "Small" => 1,
        "Medium" => 2,
        _ => 3
    };
}
```

## Layout Adaptation Strategies

### Strategy 1: Mobile-First Approach

Start with mobile layout, then enhance for larger screens:

```razor
<SfMediaQuery @bind-ActiveBreakPoint="bp"></SfMediaQuery>

<div class="container">
    <aside class="sidebar">
        @if (bp != "Small")
        {
            <SidebarContent />
        }
    </aside>
    <main class="content">
        <MainContent />
    </main>
</div>

<style>
    .container {
        display: flex;
        flex-direction: column;
    }
    
    /* Mobile (Small) - sidebar hidden by default */
    
    /* Tablet and up */
    @media (min-width: 769px) {
        .container {
            flex-direction: row;
        }
        .sidebar {
            width: 250px;
        }
    }
</style>
```

### Strategy 2: Content Reflow

Rearrange content order based on screen size:

```razor
<SfMediaQuery @bind-ActiveBreakPoint="bp"></SfMediaQuery>

<div class="@(bp == "Small" ? "flex-column" : "flex-row")">
    <section class="hero">Hero Section</section>
    <section class="features">Features Section</section>
    <section class="testimonials">Testimonials Section</section>
</div>

<style>
    .flex-column { display: flex; flex-direction: column; }
    .flex-row { display: flex; flex-direction: row; }
</style>
```

### Strategy 3: Conditional Rendering with Fallback

```razor
<SfMediaQuery @bind-ActiveBreakPoint="bp"></SfMediaQuery>

<div class="dashboard">
    @if (bp == "Small")
    {
        <Stack Direction="StackDirection.Vertical" Spacing="10">
            <Widget1 />
            <Widget2 />
            <Widget3 />
        </Stack>
    }
    else if (bp == "Medium")
    {
        <Grid Columns="2">
            <Widget1 />
            <Widget2 />
            <Widget3 />
        </Grid>
    }
    else
    {
        <Grid Columns="3">
            <Widget1 />
            <Widget2 />
            <Widget3 />
        </Grid>
    }
</div>
```

## CSS Class Switching

Combine `ActiveBreakpoint` with CSS classes for styling-based responsive design:

```razor
<SfMediaQuery @bind-ActiveBreakPoint="bp"></SfMediaQuery>

<div class="responsive-container @bp">
    <h1>Responsive Heading</h1>
    <p>This content adapts via CSS classes.</p>
</div>

<style>
    .responsive-container {
        padding: 20px;
        font-size: 14px;
    }
    
    .responsive-container.Medium {
        padding: 30px;
        font-size: 16px;
    }
    
    .responsive-container.Large {
        padding: 50px;
        font-size: 18px;
    }
</style>
```

## Performance Optimization

### Tip 1: Minimize Re-renders
Cache computed breakpoint-dependent values:

```razor
<SfMediaQuery @bind-ActiveBreakPoint="bp"></SfMediaQuery>

@if (ShouldShowSidebar)
{
    <Sidebar />
}

@code {
    private string bp { get; set; }
    
    private bool ShouldShowSidebar => bp != "Small";
}
```

### Tip 2: Debounce Breakpoint Changes
Media Query fires on every pixel change. Implement debouncing if expensive operations occur:

```razor
<SfMediaQuery @bind-ActiveBreakPoint="bp"></SfMediaQuery>

@code {
    private string bp { get; set; }
    private string prevBreakpoint { get; set; }
    
    protected override void OnParametersSet()
    {
        if (bp != prevBreakpoint)
        {
            HandleBreakpointChange(bp);
            prevBreakpoint = bp;
        }
    }
    
    private void HandleBreakpointChange(string newBreakpoint)
    {
        // Expensive operation here (API calls, etc.)
    }
}
```

### Tip 3: Use StateHasChanged Sparingly
Let two-way binding handle updates. Only call `StateHasChanged()` if needed:

```razor
// ❌ Avoid - unnecessary re-render
<SfMediaQuery @bind-ActiveBreakPoint="bp" 
              ActiveBreakPointChanged="OnBreakpointChanged">
</SfMediaQuery>

private void OnBreakpointChanged(string newBreakpoint)
{
    StateHasChanged(); // Usually not needed
}

// ✅ Prefer - let binding handle it
<SfMediaQuery @bind-ActiveBreakPoint="bp"></SfMediaQuery>
```

## Common Pitfalls

### Pitfall 1: Hardcoded Breakpoint Values
```razor
// ❌ Bad - breakpoint name is hardcoded
@if (breakpoint == "Small")

// ✅ Good - use constants
private const string SMALL_BP = "Small";

@if (breakpoint == SMALL_BP)
```

### Pitfall 2: Missing Initialization
```razor
// ❌ Bad - breakpoint might be null initially
@if (breakpoint == "Small") { }

// ✅ Good - provide default value
private string breakpoint = "Large";
```

### Pitfall 3: Too Many Breakpoints
```razor
// ❌ Bad - 6+ breakpoints create maintenance overhead
new List<MediaBreakpoint>() { ... 10 items ... }

// ✅ Good - 3-4 breakpoints cover most scenarios
new List<MediaBreakpoint>() { Mobile, Tablet, Desktop }
```

### Pitfall 4: Blocking Render in Handler
```razor
// ❌ Bad - async operation blocks render
private async Task OnBreakpointChanged(string bp)
{
    await FetchData(); // Blocks UI
}

// ✅ Good - fire-and-forget pattern
private void OnBreakpointChanged(string bp)
{
    _ = Task.Run(() => FetchData()); // Non-blocking
}
```
