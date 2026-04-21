# Resizing, Expand, and Collapse

## Table of Contents
- [Resizing Behavior](#resizing-behavior)
- [Size Constraints](#size-constraints)
- [Prevent Resizing](#prevent-resizing)
- [Programmatic Control](#programmatic-control)
- [Customize Resize Icon](#customize-resize-icon)

## Resizing Behavior

By default, resizing is enabled for all panes. Users drag the separator to adjust sizes.

### Basic Resizing

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="200px" Width="600px" SeparatorSize="3">
    <SplitterPanes>
        <SplitterPane Size="200px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Grid</h3>
                    <p>Drag separator to resize</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="200px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Schedule</h3>
                    <p>Drag separator to resize</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="200px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Chart</h3>
                    <p>Drag separator to resize</p>
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
- Horizontal splitter: Drag left-right to resize panes
- Vertical splitter: Drag up-down to resize panes
- Resize gripper element (small icon) appears on separator for visibility
- Adjacent panes auto-adjust when one is resized

## Size Constraints

Use `Min` and `Max` properties to restrict resize range for each pane.

### Min and Max Validation

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="200px" Width="600px" SeparatorSize="4">
    <SplitterPanes>
        <SplitterPane Size="200px" Min="20%" Max="40%">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Grid</h3>
                    <p>Min: 20%, Max: 40%</p>
                    <p>Try to resize - constrained</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="200px" Min="30%" Max="60%">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Schedule</h3>
                    <p>Min: 30%, Max: 60%</p>
                    <p>Try to resize - constrained</p>
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
- First pane: Cannot resize below 20% or above 40%
- Second pane: Cannot resize below 30% or above 60%
- Separator stops when limit reached

**Use case**: Ensure navigation panels stay visible, content areas have minimum space.

### Mixed Units (Pixels and Percentages)

```csharp
<SplitterPane Size="200px" Min="100px" Max="400px">
    <!-- Constrained between 100px and 400px -->
</SplitterPane>

<SplitterPane Size="30%" Min="200px" Max="60%">
    <!-- Constrained between 200px minimum and 60% maximum -->
</SplitterPane>
```

## Prevent Resizing

Disable resizing for specific panes using `Resizable="false"`.

### Disable Resizing on One Pane

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="200px" Width="600px" SeparatorSize="4">
    <SplitterPanes>
        <SplitterPane Size="200px" Min="20%" Max="40%" Resizable="false">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Grid (Fixed)</h3>
                    <p>Cannot resize - Resizable=false</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="200px" Min="30%" Max="60%">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Schedule (Resizable)</h3>
                    <p>Can resize normally</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Chart</h3>
                    <p>Can resize</p>
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

**Important**: Both the pane and its adjacent pane must be resizable for resizing to work. If either has `Resizable="false"`, the separator between them won't resize.

**Use case**: Fixed-width sidebars, locked navigation panels, preventing accidental resize.

## Programmatic Control

Control expand/collapse via public methods on the SfSplitter component.

### Expand Method

```csharp
@using Syncfusion.Blazor.Layouts
@using Syncfusion.Blazor.Buttons

<SfButton @onclick="@Expand">Expand Pane 0</SfButton>
<SfButton @onclick="@Collapse">Collapse Pane 0</SfButton>

<SfSplitter @ref="SplitterObj" Height="200px" Width="600px" SeparatorSize="2">
    <SplitterPanes>
        <SplitterPane Collapsible="true">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>PARIS</h3>
                    <p>Pane 0</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Collapsible="true">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>CAMEMBERT</h3>
                    <p>Pane 1</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Collapsible="true">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>GRENOBLE</h3>
                    <p>Pane 2</p>
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

@code {
    private SfSplitter SplitterObj;

    private async Task Expand()
    {
        // Expand pane at index 0
        await this.SplitterObj.ExpandAsync(0);
    }

    private async Task Collapse()
    {
        // Collapse pane at index 0
        await this.SplitterObj.CollapseAsync(0);
    }
}
```

**Methods**:
- `ExpandAsync(int index)`: Expands pane at given index
- `CollapseAsync(int index)`: Collapses pane at given index

**Use case**: Expand/collapse via button clicks, keyboard shortcuts, or menu items.

### Initial Collapsed State

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="200px" Width="600px" SeparatorSize="2">
    <SplitterPanes>
        <SplitterPane Collapsible="true">
            <ContentTemplate>
                <div style="padding: 10px;">First Pane (Expanded)</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Collapsible="true" Collapsed="true">
            <ContentTemplate>
                <div style="padding: 10px;">Second Pane (Initially Collapsed)</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Collapsible="true">
            <ContentTemplate>
                <div style="padding: 10px;">Third Pane (Expanded)</div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    private bool secondPaneCollapsed { get; set; } = true;
}
```

**Collapsed="true"**: Pane renders in collapsed state on page load.

### Two-Way Binding with Collapsed State

```csharp
@using Syncfusion.Blazor.Layouts
@using Syncfusion.Blazor.Buttons

<SfButton @onclick="@ToggleSidebar">Toggle Sidebar</SfButton>

<SfSplitter Height="300px" Width="100%">
    <SplitterPanes>
        <SplitterPane Size="25%" Collapsible="true" @bind-Collapsed="@sidebarCollapsed">
            <ContentTemplate>
                <div style="padding: 10px;">Sidebar (Collapsed: @sidebarCollapsed)</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate>
                <div style="padding: 10px;">Main Content</div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    private bool sidebarCollapsed = false;

    private void ToggleSidebar()
    {
        sidebarCollapsed = !sidebarCollapsed;
    }
}
```

**@bind-Collapsed**: Two-way binding between pane state and component property. Changes to either side sync automatically.

## Customize Resize Icon

Override default resize gripper and cursor using CSS.

### Customize Resize Handler

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="200px" Width="600px" SeparatorSize="3">
    <SplitterPanes>
        <SplitterPane Size="200px" Min="20%" Max="40%">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Grid</h3>
                    <p>Custom resize icon</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="200px" Min="30%" Max="60%">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Schedule</h3>
                    <p>Custom resize icon</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Chart</h3>
                    <p>Custom resize icon</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

<style>
    .content {
        padding: 10px;
    }
    
    /* Customize resize handler icon */
    .e-splitter .e-split-bar .e-resize-handler:before {
        content: "\e934";
        font-family: 'e-icons';
        font-size: 11px;
        transform: rotate(90deg);
    }
    
    /* Customize cursor for horizontal splitter */
    .e-splitter .e-split-bar.e-split-bar-horizontal.e-resizable-split-bar,
    .e-splitter .e-split-bar.e-split-bar-horizontal.e-resizable-split-bar::after {
        cursor: e-resize;
    }
    
    /* Customize cursor for vertical splitter */
    .e-splitter .e-split-bar.e-split-bar-vertical.e-resizable-split-bar,
    .e-splitter .e-split-bar.e-split-bar-vertical.e-resizable-split-bar::after {
        cursor: n-resize;
    }
</style>
```

### Hide Resize Handler

```csharp
<style>
    /* Hide horizontal resize handler */
    .e-splitter .e-split-bar.e-split-bar-horizontal .e-resize-handler {
        display: none;
    }
    
    /* Hide vertical resize handler */
    .e-splitter .e-split-bar.e-split-bar-vertical .e-resize-handler {
        display: none;
    }
</style>
```

**Use case**: Cleaner UI if resize is obvious, accessibility improvements with custom cursors.

## Separator Template Customization

Customize the separator appearance using `SplitterTemplates` with a custom `Separator` template instead of relying on CSS alone.

### Custom Separator Template

```csharp
@using Syncfusion.Blazor.Layouts

<div>Custom Separator Template</div>
<SfSplitter Height="240px" Width="100%">
    <SplitterTemplates>
        <Separator>
            <div style="height: 10px; background: linear-gradient(to bottom, #ff6b6b, #ee5a6f); border-top: 1px solid #d63031; border-bottom: 1px solid #d63031;"></div>
        </Separator>
    </SplitterTemplates>
    <SplitterPanes>
        <SplitterPane Collapsible="true">
            <ContentTemplate>
                <div style="padding: 20px;">Left Pane</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Collapsible="true">
            <ContentTemplate>
                <div style="padding: 20px;">Middle Pane</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Collapsible="true">
            <ContentTemplate>
                <div style="padding: 20px;">Right Pane</div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

**Behavior**: The `<Separator>` template replaces the default separator rendering completely with your custom HTML/CSS.

**When to use**: Brand-specific separator styling, animated separators, unique UI designs.

### Advanced: Interactive Separator

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="300px" Width="100%">
    <SplitterTemplates>
        <Separator>
            <div style="display: flex; align-items: center; justify-content: center; background: #f0f0f0; cursor: grab; user-select: none;">
                <span style="font-size: 20px; color: #999;">⋮ ⋮</span>
            </div>
        </Separator>
    </SplitterTemplates>
    <SplitterPanes>
        <SplitterPane Size="30%">
            <ContentTemplate>
                <div style="padding: 20px;">
                    <h3>Interactive Separator</h3>
                    <p>Grab the dots above to resize</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate>
                <div style="padding: 20px;">
                    <h3>Content Area</h3>
                    <p>Custom separator improves usability</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

**Output**: Separator shows visual indicator (dots) with grab cursor.

### Template with Blazor Components

```csharp
@using Syncfusion.Blazor.Layouts
@using Syncfusion.Blazor.Buttons

<SfSplitter Height="300px" Width="100%">
    <SplitterTemplates>
        <Separator>
            <div style="background: #e0e0e0; display: flex; align-items: center; justify-content: center; gap: 10px;">
                <SfButton IconCss="e-icon-arrow-left" CssClass="e-small" Disabled="true"></SfButton>
                <div style="width: 2px; height: 30px; background: #999;"></div>
                <SfButton IconCss="e-icon-arrow-right" CssClass="e-small" Disabled="true"></SfButton>
            </div>
        </Separator>
    </SplitterTemplates>
    <SplitterPanes>
        <SplitterPane Size="30%">
            <ContentTemplate><div style="padding: 20px;">Left</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate><div style="padding: 20px;">Right</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

### Key Points about Templates

- **Override HTML**: Template replaces entire separator element
- **Responsive**: Template scales with splitter dimensions
- **Functional**: Resize still works with custom template
- **Styling**: Apply CSS directly in template for visual effects
- **Performance**: Complex templates may impact resize performance

**Best practices**:
- Keep template simple for performance
- Ensure adequate height/width for usability
- Maintain hover/active states visually

## Refresh Content on Resize

Pane content (charts, grids) should refresh when container resizes:

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="400px" Width="100%">
    <SplitterEvents OnResizeStop="@RefreshChart" />
    <SplitterPanes>
        <SplitterPane Size="50%" Min="300px">
            <SfChart @ref="chartRef" Title="Sample Chart" Height="100%">
                <ChartSeriesCollection>
                    <ChartSeries DataSource="@SalesData" XName="Month" YName="Sales" Type="ChartSeriesType.Column"></ChartSeries>
                </ChartSeriesCollection>
            </SfChart>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate>
                <div style="padding: 20px;">Details Panel</div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    private SfChart chartRef;
    private List<DataPoint> SalesData = new();

    protected override void OnInitialized()
    {
        SalesData = new List<DataPoint>
        {
            new DataPoint { Month = "Jan", Sales = 100 },
            new DataPoint { Month = "Feb", Sales = 150 },
            new DataPoint { Month = "Mar", Sales = 120 }
        };
    }

    private async Task RefreshChart(ResizingEventArgs args)
    {
        if (chartRef != null)
            await chartRef.Refresh();
    }

    public class DataPoint
    {
        public string Month { get; set; }
        public double Sales { get; set; }
    }
}
```

---

**Next:** Read [Styling and Theming](styling-and-theming.md) for custom appearance and theme options.
