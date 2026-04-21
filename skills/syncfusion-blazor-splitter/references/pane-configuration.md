# Pane Configuration and Settings

## Table of Contents
- [Pane Sizing](#pane-sizing)
- [Min/Max Constraints](#minmax-constraints)
- [Pane Visibility](#pane-visibility)
- [Collapsible Panes](#collapsible-panes)
- [Content Types](#content-types)
- [Component Integration](#component-integration)

## Pane Sizing

Control pane width/height using the `Size` property. Supports pixels and percentages.

### Fixed Size (Pixels)

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="200px" Width="600px">
    <SplitterPanes>
        <SplitterPane Size="200px">
            <ContentTemplate>
                <div style="padding: 10px;">Left Pane (200px fixed)</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="200px">
            <ContentTemplate>
                <div style="padding: 10px;">Middle Pane (200px fixed)</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate>
                <div style="padding: 10px;">Right Pane (auto, remaining space)</div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

<style>
    .e-pane {
        text-align: center;
        align-items: center;
        display: grid;
    }
</style>
```

**Behavior**: First pane = 200px, second pane = 200px, third pane = remaining space (auto).

### Percentage Sizing

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="200px" Width="600px">
    <SplitterPanes>
        <SplitterPane Size="30%">
            <ContentTemplate>
                <div style="padding: 10px;">Left Pane (30%)</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="40%">
            <ContentTemplate>
                <div style="padding: 10px;">Middle Pane (40%)</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="30%">
            <ContentTemplate>
                <div style="padding: 10px;">Right Pane (30%)</div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

**Behavior**: Panes always maintain 30%-40%-30% ratio regardless of container width.

### Auto-Sizing

Omit `Size` to let panes auto-adjust:

```csharp
<SfSplitter Height="200px" Width="600px">
    <SplitterPanes>
        <SplitterPane>
            <ContentTemplate><div>Auto-sized</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate><div>Auto-sized</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

**Best for**: Flexible layouts that adapt when panes are added/removed or when container resizes.

**Note**: When all panes specify fixed sizes, the last pane becomes flexible (overrides its Size).

## Min/Max Constraints

Use `Min` and `Max` properties to restrict how much users can resize each pane.

### Size Validation Example

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="200px" Width="600px" SeparatorSize="4">
    <SplitterPanes>
        <SplitterPane Size="200px" Min="20%" Max="40%">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Grid</h3>
                    <p>Min: 20%, Max: 40%</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="200px" Min="30%" Max="60%">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Schedule</h3>
                    <p>Min: 30%, Max: 60%</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Chart</h3>
                    <p>No constraints</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

<style>
    .content {
        padding: 10px;
    }
</style>
```

**Behavior**: 
- First pane: Resizable between 20%-40% of container width
- Second pane: Resizable between 30%-60% of container width
- Third pane: No constraints, fills remaining space

**Use case**: Prevent navigation panels from becoming too small, prevent content areas from taking all space.

## Pane Visibility

Show/hide panes dynamically using the `Visible` property.

### Visibility Control Example

```csharp
@using Syncfusion.Blazor.Layouts

<button class="e-btn" @onclick="@ToggleMiddlePane">Toggle Middle Pane</button>

<SfSplitter Height="240px" Width="700px" SeparatorSize="2">
    <SplitterPanes>
        <SplitterPane Size="30%" Collapsible="true">
            <ContentTemplate>
                <div style="padding: 10px;">Left Pane</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="30%" Collapsible="true" Visible="@this.PaneMiddleVisibility">
            <ContentTemplate>
                <div style="padding: 10px;">Middle Pane (Toggleable)</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Collapsible="true">
            <ContentTemplate>
                <div style="padding: 10px;">Right Pane</div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    public bool PaneMiddleVisibility { get; set; } = false;

    public void ToggleMiddlePane()
    {
        this.PaneMiddleVisibility = !this.PaneMiddleVisibility;
    }
}
```

**Behavior**: 
- Click button to toggle `Visible` property
- Middle pane slides in/out, other panes auto-adjust
- When hidden, pane doesn't take space in layout

**When to use**: Show/hide optional panels, conditional UI sections, space-saving designs.

## Collapsible Panes

Enable built-in collapse/expand buttons using the `Collapsible` property.

### Basic Collapsible Example

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="200px" Width="600px" SeparatorSize="2">
    <SplitterPanes>
        <SplitterPane Collapsible="true">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>PARIS</h3>
                    <p>Paris, the city of lights and love...</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Collapsible="true">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>CAMEMBERT</h3>
                    <p>The village in Normandy where the famous French cheese originates...</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Collapsible="true">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>GRENOBLE</h3>
                    <p>The capital city of the French Alps...</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

<style>
    .content {
        padding: 10px;
    }
</style>
```

**Behavior**: 
- Collapse/expand arrow buttons appear on each pane header
- Click arrows to collapse pane and reclaim space
- Collapsed pane shows only a thin bar

**When to use**: Save screen space, let users focus on important content, optional side panels.

### Initial Collapsed State

Start with specific panes collapsed using `Collapsed` property:

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="200px" Width="600px" SeparatorSize="2">
    <SplitterPanes>
        <SplitterPane Collapsible="true">
            <ContentTemplate><div style="padding: 10px;">First Pane (Expanded)</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane Collapsible="true" Collapsed="true">
            <ContentTemplate><div style="padding: 10px;">Second Pane (Initially Collapsed)</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane Collapsible="true">
            <ContentTemplate><div style="padding: 10px;">Third Pane (Expanded)</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    private bool secondPaneCollapsed { get; set; } = true;
}
```

**Use case**: Load page with collapsible sections collapsed by default, users expand as needed.

## Content Types

### HTML Markup Content

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="200px" Width="600px">
    <SplitterPanes>
        <SplitterPane Size="200px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <div class="content">
                        <h3>Grid</h3>
                        <p>The ASP.NET DataGrid control is used to display data in a tabular format.</p>
                    </div>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="200px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <div class="content">
                        <h3>Schedule</h3>
                        <p>The ASP.NET Scheduler facilitates calendar features for time management.</p>
                    </div>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="200px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <div class="content">
                        <h3>Chart</h3>
                        <p>ASP.NET charts add beautiful visualizations to applications.</p>
                    </div>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

<style>
    .content {
        padding: 10px;
    }
</style>
```

### Plain Text Content

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="200px" Width="600px">
    <SplitterPanes>
        <SplitterPane Size="200px">
            <ContentTemplate>
                <div style="text-align: center; padding: 20px;">
                    Left pane
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="200px">
            <ContentTemplate>
                <div style="text-align: center; padding: 20px;">
                    Middle pane
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="200px">
            <ContentTemplate>
                <div style="text-align: center; padding: 20px;">
                    Right pane
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

<style>
    .e-pane {
        text-align: center;
        align-items: center;
        display: grid;
    }
</style>
```

## Component Integration

### Integrating Blazor Components

Any Blazor component can be rendered inside splitter panes:

```csharp
@using Syncfusion.Blazor.Grids
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="300px" Width="100%" SeparatorSize="4">
    <SplitterPanes>
        <SplitterPane Size="50%" Min="300px">
            <SfGrid DataSource="@Orders">
                <GridColumns>
                    <GridColumn Field=@nameof(Order.OrderID) HeaderText="Order ID" TextAlign="TextAlign.Right" Width="120"></GridColumn>
                    <GridColumn Field=@nameof(Order.CustomerID) HeaderText="Customer Name" Width="150"></GridColumn>
                    <GridColumn Field=@nameof(Order.OrderDate) HeaderText="Order Date" Format="yMd" Type="ColumnType.Date" TextAlign="TextAlign.Right" Width="130"></GridColumn>
                </GridColumns>
            </SfGrid>
        </SplitterPane>
        <SplitterPane Size="50%">
            <ContentTemplate>
                <div style="padding: 20px;">
                    <h3>Details</h3>
                    <p>Select a row from the grid to see details here</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    private List<Order> Orders = new();

    protected override void OnInitialized()
    {
        Orders = new List<Order>
        {
            new Order { OrderID = 10248, CustomerID = "VINET", OrderDate = new DateTime(1996, 7, 4) },
            new Order { OrderID = 10249, CustomerID = "TOMSP", OrderDate = new DateTime(1996, 7, 5) },
            new Order { OrderID = 10250, CustomerID = "HANAR", OrderDate = new DateTime(1996, 7, 8) }
        };
    }

    public class Order
    {
        public int OrderID { get; set; }
        public string CustomerID { get; set; }
        public DateTime OrderDate { get; set; }
    }
}
```

**Best practices**:
- Place large components (Grid, Tree, Chart) in fixed or minimum-sized panes
- Use Size="50%" Min="300px" to prevent pane from becoming too small
- Ensure components handle content resizing properly

### Multiple Components in One Pane

```csharp
<SplitterPane Size="40%">
    <ContentTemplate>
        <div style="overflow: auto; height: 100%;">
            <SfListView TValue="MailItem" DataSource="@Items" ShowCheckBox="true">
                <ListViewFieldSettings TValue="MailItem" Id="Id" Text="Subject"></ListViewFieldSettings>
            </SfListView>
        </div>
    </ContentTemplate>
</SplitterPane>
```

**Key**: Wrap multiple components in a container div with `overflow: auto; height: 100%;` to handle scrolling.

---

**Next:** Read [Events and Interactions](events-and-interactions.md) to learn about handling user events and pane changes.
