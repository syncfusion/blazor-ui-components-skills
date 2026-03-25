# Orientation and Header Placement

## Table of Contents
- [HeaderPlacement Property](#headerplacement-property)
- [Horizontal Layouts](#horizontal-layouts)
- [Vertical Layouts](#vertical-layouts)
- [OverflowMode: Scrollable](#overflow-mode-scrollable)
- [OverflowMode: Popup](#overflow-mode-popup)
- [Responsive Behavior](#responsive-behavior)

## HeaderPlacement Property

The `HeaderPlacement` property controls where tab headers are positioned relative to content. The available positions are:

| Position | Description | Use Case |
|----------|-------------|----------|
| **Top** (default) | Headers above content, horizontal arrangement | Standard tabbed interface |
| **Bottom** | Headers below content, horizontal arrangement | Bottom navigation patterns |
| **Left** | Headers left of content, vertical arrangement | Sidebar-like navigation |
| **Right** | Headers right of content, vertical arrangement | Alternative vertical layout |

### Syntax
```razor
<SfTab HeaderPlacement="HeaderPosition.Top">
```

## Horizontal Layouts

### Top Placement (Default)

Headers appear above the content, arranged horizontally.

```razor
@using Syncfusion.Blazor.Navigations

<SfTab HeaderPlacement="HeaderPosition.Top" Width="500px">
    <TabItems>
        <TabItem Content="HTML is a markup language...">
            <ChildContent>
                <TabHeader Text="HTML"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="C# is an object-oriented programming language...">
            <ChildContent>
                <TabHeader Text="C#"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Java is widely used for web applications...">
            <ChildContent>
                <TabHeader Text="Java"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```

**Usage:**
- Navigation tabs at top of page
- Standard web application pattern
- Familiar to most users

### Bottom Placement

Headers appear below the content, arranged horizontally.

```razor
@using Syncfusion.Blazor.Navigations

<SfTab HeaderPlacement="HeaderPosition.Bottom" Width="500px">
    <TabItems>
        <TabItem Content="HTML content...">
            <ChildContent>
                <TabHeader Text="HTML"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="C# content...">
            <ChildContent>
                <TabHeader Text="C#"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Java content...">
            <ChildContent>
                <TabHeader Text="Java"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```

**Usage:**
- Bottom navigation bars
- Alternative design pattern
- Mobile-friendly layout

## Vertical Layouts

### Left Placement

Headers appear left of the content, arranged vertically. Creates a sidebar-style navigation.

```razor
@using Syncfusion.Blazor.Navigations

<SfTab HeaderPlacement="HeaderPosition.Left" Height="250px" Width="600px">
    <TabItems>
        <TabItem Content="HyperText Markup Language, commonly referred to as HTML, is the standard markup language...">
            <ChildContent>
                <TabHeader Text="HTML"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="C# is intended to be a simple, modern, general-purpose, object-oriented programming language...">
            <ChildContent>
                <TabHeader Text="C#"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Java is a set of computer software and specifications developed by Sun Microsystems...">
            <ChildContent>
                <TabHeader Text="Java"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Python is a high-level, general-purpose programming language...">
            <ChildContent>
                <TabHeader Text="Python"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```

**Usage:**
- Dashboard sidebars
- Admin panel navigation
- Better for many tab items
- Wide screens with vertical space

### Right Placement

Headers appear right of the content, arranged vertically.

```razor
@using Syncfusion.Blazor.Navigations

<SfTab HeaderPlacement="HeaderPosition.Right" Height="250px" Width="600px">
    <TabItems>
        <TabItem Content="Content 1...">
            <ChildContent>
                <TabHeader Text="Option 1"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Content 2...">
            <ChildContent>
                <TabHeader Text="Option 2"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Content 3...">
            <ChildContent>
                <TabHeader Text="Option 3"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```

**Usage:**
- Alternative design aesthetic
- Right-to-left support
- Less common but useful for special layouts

## OverflowMode: Scrollable

The `Scrollable` overflow mode (default) displays all tab headers in a single line with horizontal scrolling when tabs exceed available space.

### Features
- Navigation arrows at start and end of header area
- Scroll left/right by clicking arrows or holding them
- Touch and swipe support for mobile devices
- Default behavior for most applications

### Implementation

```razor
@using Syncfusion.Blazor.Navigations

<SfTab OverflowMode="OverflowMode.Scrollable" Width="500px">
    <TabItems>
        <TabItem Content="HTML content">
            <ChildContent>
                <TabHeader Text="HTML"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="C# content">
            <ChildContent>
                <TabHeader Text="C#"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Java content">
            <ChildContent>
                <TabHeader Text="Java"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="JavaScript content">
            <ChildContent>
                <TabHeader Text="JavaScript"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Python content">
            <ChildContent>
                <TabHeader Text="Python"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Ruby content">
            <ChildContent>
                <TabHeader Text="Ruby"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Go content">
            <ChildContent>
                <TabHeader Text="Go"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Rust content">
            <ChildContent>
                <TabHeader Text="Rust"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```

### Scrollable with Vertical Layout

```razor
@using Syncfusion.Blazor.Navigations

<SfTab HeaderPlacement="HeaderPosition.Left" 
        OverflowMode="OverflowMode.Scrollable" 
        Width="600px" Height="400px">
    <!-- Many tab items -->
</SfTab>
```

## OverflowMode: Popup

The `Popup` overflow mode displays a dropdown button to access overflowed tabs. This is useful when space is limited.

### Features
- Single dropdown button to access hidden tabs
- Compact header area
- Good for constrained layouts
- Organized display of overflow items

### Implementation

```razor
@using Syncfusion.Blazor.Navigations

<SfTab OverflowMode="OverflowMode.Popup" Width="500px">
    <TabItems>
        <TabItem Content="HTML is a markup language...">
            <ChildContent>
                <TabHeader Text="HTML"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="C# is a programming language...">
            <ChildContent>
                <TabHeader Text="C#"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Java is widely used...">
            <ChildContent>
                <TabHeader Text="Java"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="VB.Net compiler...">
            <ChildContent>
                <TabHeader Text="VB.Net"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Xamarin is a software company...">
            <ChildContent>
                <TabHeader Text="Xamarin"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="ASP.NET is a framework...">
            <ChildContent>
                <TabHeader Text="ASP.NET"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="ASP.NET MVC...">
            <ChildContent>
                <TabHeader Text="ASP.NET MVC"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="JavaScript is a language...">
            <ChildContent>
                <TabHeader Text="JavaScript"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```

### ReorderActiveTab Property

The `ReorderActiveTab` property controls whether the active tab reorders to the visible area when using Popup mode.

```razor
@using Syncfusion.Blazor.Navigations

<!-- Active tab will NOT move to visible area in popup mode -->
<SfTab OverflowMode="OverflowMode.Popup" ReorderActiveTab="false" Width="500px">
    <!-- Many tab items -->
</SfTab>
```

- **ReorderActiveTab="true"** (default): Selected tab moves to visible area
- **ReorderActiveTab="false"**: Active tab stays in original position, only highlighted in popup

## Responsive Behavior

### Dynamic Orientation Switching

Change HeaderPlacement based on screen size or user preference:

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.DropDowns
@using Syncfusion.Blazor.Buttons

<div class="controls">
    <p>Select Header Position:</p>
    <SfDropDownList TValue="string" TItem="PositionOption" 
                    DataSource="@PositionOptions" 
                    @bind-Value="@SelectedPosition"
                    ValueChange="OnPositionChange">
        <DropDownListFieldSettings Text="Text" Value="Value"></DropDownListFieldSettings>
    </SfDropDownList>
</div>

<SfTab HeaderPlacement="@CurrentHeaderPosition" 
        OverflowMode="@CurrentOverflowMode" 
        Width="500px" Height="250px">
    <TabItems>
        <TabItem Content="HTML content...">
            <ChildContent>
                <TabHeader Text="HTML"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="C# content...">
            <ChildContent>
                <TabHeader Text="C#"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Java content...">
            <ChildContent>
                <TabHeader Text="Java"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="VB.Net content...">
            <ChildContent>
                <TabHeader Text="VB.Net"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Xamarin content...">
            <ChildContent>
                <TabHeader Text="Xamarin"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="ASP.NET content...">
            <ChildContent>
                <TabHeader Text="ASP.NET"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="ASP.NET MVC content...">
            <ChildContent>
                <TabHeader Text="ASP.NET MVC"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="JavaScript content...">
            <ChildContent>
                <TabHeader Text="JavaScript"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private string SelectedPosition = "Top";
    private HeaderPosition CurrentHeaderPosition = HeaderPosition.Top;
    private OverflowMode CurrentOverflowMode = OverflowMode.Scrollable;
    
    private List<PositionOption> PositionOptions = new()
    {
        new PositionOption { Text = "Top", Value = "Top" },
        new PositionOption { Text = "Bottom", Value = "Bottom" },
        new PositionOption { Text = "Left", Value = "Left" },
        new PositionOption { Text = "Right", Value = "Right" }
    };
    
    private void OnPositionChange(Syncfusion.Blazor.DropDowns.ChangeEventArgs<string, PositionOption> args)
    {
        SelectedPosition = args.Value;
        CurrentHeaderPosition = (HeaderPosition)Enum.Parse(typeof(HeaderPosition), args.Value);
        
        // Adjust overflow mode based on position
        CurrentOverflowMode = (args.Value == "Left" || args.Value == "Right") 
            ? OverflowMode.Scrollable 
            : OverflowMode.Scrollable;
    }
    
    public class PositionOption
    {
        public string Text { get; set; }
        public string Value { get; set; }
    }
}
```

### Complete Example: Responsive Tab Configuration

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.DropDowns

<div style="margin-bottom: 20px;">
    <label>Header Placement: </label>
    <SfDropDownList TValue="string" TItem="DropdownOption" 
                    DataSource="@HeaderPlacements"
                    @bind-Value="@HeaderValue"
                    ValueChange="OnHeaderChange">
        <DropDownListFieldSettings Text="Text" Value="Value"></DropDownListFieldSettings>
    </SfDropDownList>
    
    <label style="margin-left: 20px;">Overflow Mode: </label>
    <SfDropDownList TValue="string" TItem="DropdownOption"
                    DataSource="@OverflowModes"
                    @bind-Value="@ModeValue"
                    ValueChange="OnModeChange">
        <DropDownListFieldSettings Text="Text" Value="Value"></DropDownListFieldSettings>
    </SfDropDownList>
</div>

<SfTab HeaderPlacement="@Header" OverflowMode="@Mode" Width="500px" Height="250px" ShowCloseButton="true">
    <TabItems>
        <TabItem Content="HyperText Markup Language...">
            <ChildContent>
                <TabHeader Text="HTML"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="C# is a simple, modern language...">
            <ChildContent>
                <TabHeader Text="C#"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Java is widely used...">
            <ChildContent>
                <TabHeader Text="Java"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="VB.NET compiler...">
            <ChildContent>
                <TabHeader Text="VB.Net"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Xamarin developers...">
            <ChildContent>
                <TabHeader Text="Xamarin"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="ASP.NET framework...">
            <ChildContent>
                <TabHeader Text="ASP.NET"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="ASP.NET MVC pattern...">
            <ChildContent>
                <TabHeader Text="ASP.NET MVC"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="JavaScript language...">
            <ChildContent>
                <TabHeader Text="JavaScript"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private string HeaderValue = "Top";
    private string ModeValue = "Scrollable";
    private HeaderPosition Header = HeaderPosition.Top;
    private OverflowMode Mode = OverflowMode.Scrollable;
    
    private List<DropdownOption> HeaderPlacements = new()
    {
        new DropdownOption { Text = "Top", Value = "Top" },
        new DropdownOption { Text = "Bottom", Value = "Bottom" },
        new DropdownOption { Text = "Left", Value = "Left" },
        new DropdownOption { Text = "Right", Value = "Right" }
    };
    
    private List<DropdownOption> OverflowModes = new()
    {
        new DropdownOption { Text = "Scrollable", Value = "Scrollable" },
        new DropdownOption { Text = "Popup", Value = "Popup" }
    };
    
    private void OnHeaderChange(Syncfusion.Blazor.DropDowns.ChangeEventArgs<string, DropdownOption> args)
    {
        HeaderValue = args.Value;
        Header = (HeaderPosition)Enum.Parse(typeof(HeaderPosition), args.Value);
    }
    
    private void OnModeChange(Syncfusion.Blazor.DropDowns.ChangeEventArgs<string, DropdownOption> args)
    {
        ModeValue = args.Value;
        Mode = (OverflowMode)Enum.Parse(typeof(OverflowMode), args.Value);
    }
    
    public class DropdownOption
    {
        public string Text { get; set; }
        public string Value { get; set; }
    }
}
```

## Best Practices

1. **Choose appropriate placement** for your layout and use case
2. **Use HeaderPlacement="Left"** for sidebars with many items
3. **Use OverflowMode="Popup"** for limited horizontal space
4. **Use OverflowMode="Scrollable"** for standard horizontal tabs
5. **Set Width and Height** appropriately for your design
6. **Test responsiveness** across different screen sizes
7. **Consider mobile** - top or bottom placement works best
8. **Set ReorderActiveTab** based on user experience goals
