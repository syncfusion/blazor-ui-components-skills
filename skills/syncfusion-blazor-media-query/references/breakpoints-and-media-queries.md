# Breakpoints and Media Queries

## Table of Contents
- [Understanding Built-in Breakpoints](#understanding-built-in-breakpoints)
- [ActiveBreakpoint Property](#activebreakpoint-property)
- [Modifying Built-in Breakpoints](#modifying-built-in-breakpoints)
- [Creating Custom Breakpoints](#creating-custom-breakpoints)
- [Common Breakpoint Scenarios](#common-breakpoint-scenarios)

## Understanding Built-in Breakpoints

The Blazor Media Query component includes three built-in breakpoints that match common device sizes:

| Breakpoint | Size Range | Device Type |
|-----------|-----------|-------------|
| **Small** | ≤ 768px | Mobile phones |
| **Medium** | 768px - 1024px | Tablets |
| **Large** | ≥ 1024px | Desktop/laptops |

These breakpoints use CSS media queries under the hood:
- Small: `(max-width: 768px)`
- Medium: `(min-width: 768px) and (max-width: 1024px)`
- Large: `(min-width: 1024px)`

## ActiveBreakpoint Property

The `ActiveBreakpoint` property returns the name of the currently matching breakpoint as a string. Use two-way binding to track breakpoint changes:

```razor
@using Syncfusion.Blazor

<SfMediaQuery @bind-ActiveBreakPoint="currentBreakpoint"></SfMediaQuery>

<p>Active Breakpoint: <strong>@currentBreakpoint</strong></p>

@code {
    private string currentBreakpoint { get; set; }
}
```

The value updates automatically whenever the window resizes and crosses a breakpoint threshold. This enables real-time responsive behavior without page refreshes.

## Modifying Built-in Breakpoints

Customize the default breakpoint values by modifying the media query strings. This is useful when your design requires different breakpoints than the built-in defaults.

```razor
@using Syncfusion.Blazor

<SfMediaQuery @bind-ActiveBreakPoint="activeBreakpoint"></SfMediaQuery>

<h3>The active breakpoint is @activeBreakpoint</h3>

@code {
    private string activeBreakpoint;

    protected override void OnInitialized()
    {
        // Customize breakpoint thresholds
        SfMediaQuery.Small.MediaQuery = "(max-width: 500px)";
        SfMediaQuery.Medium.MediaQuery = "(min-width: 500px) and (max-width: 1200px)";
        SfMediaQuery.Large.MediaQuery = "(min-width: 1200px)";
        
        base.OnInitialized();
    }
}
```

**Common Customizations:**
- Mobile-first: Adjust Small to `(max-width: 480px)`
- Tablet-focused: Set Medium to `(min-width: 600px) and (max-width: 1000px)`
- Large screens: Adjust Large to `(min-width: 1920px)`

## Creating Custom Breakpoints

Define completely custom breakpoints by providing a list of `MediaBreakpoint` objects. This allows you to create breakpoints that match your specific design requirements.

```razor
@using Syncfusion.Blazor

<SfMediaQuery MediaBreakpoints="@customBreakpoints" 
              @bind-ActiveBreakPoint="activeBreakpoint">
</SfMediaQuery>

<h3>The active breakpoint is @activeBreakpoint</h3>

@code {
    private string activeBreakpoint;
    private List<MediaBreakpoint> customBreakpoints = new List<MediaBreakpoint>();

    protected override void OnInitialized()
    {
        customBreakpoints = new List<MediaBreakpoint>()
        {
            new MediaBreakpoint() 
            { 
                Breakpoint = "Mobile", 
                MediaQuery = "(max-width: 600px)" 
            },
            new MediaBreakpoint() 
            { 
                Breakpoint = "Tablet", 
                MediaQuery = "(min-width: 600px) and (max-width: 999px)" 
            },
            new MediaBreakpoint() 
            { 
                Breakpoint = "Laptop", 
                MediaQuery = "(min-width: 1000px) and (max-width: 1199px)" 
            },
            new MediaBreakpoint() 
            { 
                Breakpoint = "Desktop", 
                MediaQuery = "(min-width: 1200px)" 
            }
        };
        base.OnInitialized();
    }
}
```

**MediaBreakpoint Properties:**
- `Breakpoint` (string): The name returned by ActiveBreakpoint
- `MediaQuery` (string): A valid CSS media query string

## Common Breakpoint Scenarios

### Scenario 1: E-Commerce Mobile-First
```csharp
new List<MediaBreakpoint>()
{
    new MediaBreakpoint() { Breakpoint = "Phone", MediaQuery = "(max-width: 480px)" },
    new MediaBreakpoint() { Breakpoint = "Tablet", MediaQuery = "(min-width: 481px) and (max-width: 768px)" },
    new MediaBreakpoint() { Breakpoint = "Desktop", MediaQuery = "(min-width: 769px)" }
}
```

### Scenario 2: Four-Tier Responsive Design
```csharp
new List<MediaBreakpoint>()
{
    new MediaBreakpoint() { Breakpoint = "XS", MediaQuery = "(max-width: 576px)" },
    new MediaBreakpoint() { Breakpoint = "SM", MediaQuery = "(min-width: 577px) and (max-width: 768px)" },
    new MediaBreakpoint() { Breakpoint = "MD", MediaQuery = "(min-width: 769px) and (max-width: 992px)" },
    new MediaBreakpoint() { Breakpoint = "LG", MediaQuery = "(min-width: 993px)" }
}
```

### Scenario 3: High-Resolution Displays
```csharp
new List<MediaBreakpoint>()
{
    new MediaBreakpoint() { Breakpoint = "Mobile", MediaQuery = "(max-width: 768px)" },
    new MediaBreakpoint() { Breakpoint = "Tablet", MediaQuery = "(min-width: 769px) and (max-width: 1024px)" },
    new MediaBreakpoint() { Breakpoint = "Desktop", MediaQuery = "(min-width: 1025px) and (max-width: 1920px)" },
    new MediaBreakpoint() { Breakpoint = "4K", MediaQuery = "(min-width: 1921px)" }
}
```

## Best Practices

**Media Query Syntax:** Use valid CSS media queries. Ensure:
- Proper parentheses: `(max-width: 768px)`
- Logical operators: `and`, `or`, `not`
- No gaps between breakpoints (or use `or` for gaps)

**Breakpoint Naming:** Use clear, descriptive names:
- ✅ "Mobile", "Tablet", "Desktop"
- ✅ "Phone", "Pad", "Monitor"
- ❌ "Small", "Big", "Responsive"

**Testing:** Always test across all defined breakpoints to ensure smooth transitions and proper content visibility.
