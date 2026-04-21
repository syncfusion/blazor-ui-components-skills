# Split Panes and Layouts

## Table of Contents
- [Horizontal Layout](#horizontal-layout)
- [Vertical Layout](#vertical-layout)
- [Separator Customization](#separator-customization)
- [Multiple Panes](#multiple-panes)
- [Nested Splitters](#nested-splitters)

## Horizontal Layout

**Default behavior**: The Splitter renders in horizontal orientation with vertical separators dividing the container left-to-right.

### Basic Horizontal Example

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="200px" Width="600px">
    <SplitterPanes>
        <SplitterPane Size="200px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Grid</h3>
                    <p>The ASP.NET DataGrid control is a feature-rich control used to display data in a tabular format.</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="200px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Schedule</h3>
                    <p>The ASP.NET Scheduler facilitates almost all calendar features for efficient time management.</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="200px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Chart</h3>
                    <p>ASP.NET charts are used to add beautiful charts in web and mobile applications.</p>
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

**Output**: Three equal-width horizontal panes. Users can drag the vertical separators to resize.

**When to use**: Side-by-side content comparison, sidebar + main content layouts, three-column dashboards.

## Vertical Layout

Use `Orientation="Orientation.Vertical"` to split the container top-to-bottom with horizontal separators.

### Basic Vertical Example

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="305px" Width="600px" Orientation="Orientation.Vertical">
    <SplitterPanes>
        <SplitterPane Size="100px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Grid</h3>
                    <p>The ASP.NET DataGrid control is a feature-rich control used to display data in a tabular format.</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="100px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Schedule</h3>
                    <p>The ASP.NET Scheduler facilitates almost all calendar features for efficient time management.</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="100px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Chart</h3>
                    <p>ASP.NET charts are used to add beautiful charts in web and mobile applications.</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

<style>
    .content {
        padding: 9px;
    }
</style>
```

**Output**: Three stacked vertical panes. Users drag horizontal separators to adjust heights.

**When to use**: Toolbar + content + status bar layouts, code editor (top editors + bottom terminal), top navigation + main + footer.

## Separator Customization

Customize the separator width/height using the `SeparatorSize` property (default: 1px).

### Example: Thicker Separator

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="250px" Width="600px" SeparatorSize="5">
    <SplitterPanes>
        <SplitterPane Size="200px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Grid</h3>
                    <p>Grid component content.</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="200px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Schedule</h3>
                    <p>Schedule component content.</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="200px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Chart</h3>
                    <p>Chart component content.</p>
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

**Effects**:
- **Horizontal splitter**: SeparatorSize sets width (horizontal separator thickness)
- **Vertical splitter**: SeparatorSize sets height (vertical separator thickness)

**Use case**: Make separators more visible/easier to drag by increasing size.

## Multiple Panes

The Splitter supports any number of panes (2, 3, 4, or more). Add additional `SplitterPane` elements for more divisions.

### Four Horizontal Panes

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="200px" Width="800px">
    <SplitterPanes>
        <SplitterPane Size="150px">
            <ContentTemplate><div style="text-align: center; padding: 20px;">Pane 1</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="150px">
            <ContentTemplate><div style="text-align: center; padding: 20px;">Pane 2</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="150px">
            <ContentTemplate><div style="text-align: center; padding: 20px;">Pane 3</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="150px">
            <ContentTemplate><div style="text-align: center; padding: 20px;">Pane 4</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

**Behavior**: Each pane can be resized independently. The last pane is flexible (auto-adjusts when others change).

**Tip**: For more than 3 panes, consider using nested splitters for complex layouts instead of all horizontal/vertical panes.

## Nested Splitters

Create complex layouts by nesting splitters. The inner splitter must have 100% width and height to fit its parent pane.

### Code Editor Layout (Nested Horizontal + Vertical)

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="400px" Width="600px" Orientation="Orientation.Vertical">
    <SplitterPanes>
        <!-- Top Pane: Three-Column Editor -->
        <SplitterPane Size="53%" Min="30%">
            <SfSplitter Height="100%" Width="100%">
                <SplitterPanes>
                    <SplitterPane Size="29%" Min="23%">
                        <ContentTemplate>
                            <div style="padding: 10px;">
                                <h3>HTML</h3>
                                <pre>&lt;!DOCTYPE html&gt;
&lt;html&gt;
  &lt;body&gt;
    &lt;div&gt;Hello&lt;/div&gt;
  &lt;/body&gt;
&lt;/html&gt;</pre>
                            </div>
                        </ContentTemplate>
                    </SplitterPane>
                    <SplitterPane Size="20%" Min="15%">
                        <ContentTemplate>
                            <div style="padding: 10px;">
                                <h3>CSS</h3>
                                <pre>body {
  font-family: Arial;
  margin: 0;
}</pre>
                            </div>
                        </ContentTemplate>
                    </SplitterPane>
                    <SplitterPane Size="35%" Min="35%">
                        <ContentTemplate>
                            <div style="padding: 10px;">
                                <h3>JavaScript</h3>
                                <pre>console.log('Hello');
document.ready();</pre>
                            </div>
                        </ContentTemplate>
                    </SplitterPane>
                </SplitterPanes>
            </SfSplitter>
        </SplitterPane>
        
        <!-- Bottom Pane: Preview -->
        <SplitterPane Size="47%" Min="30%">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Preview</h3>
                    <img src="preview.png" style="width: 100%; height: auto;" />
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

**Layout structure**:
- Outer splitter: Vertical (top = editors, bottom = preview)
- Inner splitter (top pane): Horizontal (left = HTML, center = CSS, right = JS)

**When to use**: Code editors, IDEs, advanced dashboards with sub-sections.

### Nested Splitters Best Practice

```csharp
// Parent splitter (vertical)
<SfSplitter Height="500px" Width="100%">
    <SplitterPanes>
        <!-- Pane 1: Contains nested splitter -->
        <SplitterPane Size="60%">
            <!-- Nested splitter (horizontal) -->
            <SfSplitter Height="100%" Width="100%">
                <!-- Nested panes -->
            </SfSplitter>
        </SplitterPane>
        
        <!-- Pane 2: Simple content -->
        <SplitterPane Size="40%">
            <ContentTemplate>...</ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

**Key points**:
- Inner splitter must have `Height="100%"` and `Width="100%"`
- Parent pane container must have defined size (e.g., Size="60%")
- Supports unlimited nesting levels (but performance degrades with deep nesting)

## Auto-Sizing Panes

When panes don't specify a Size, they auto-adjust based on flex layout.

### Auto-Sizing Example

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="200px" Width="600px">
    <SplitterPanes>
        <SplitterPane>
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Grid</h3>
                    <p>Auto-sized pane</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Schedule</h3>
                    <p>Auto-sized pane</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Chart</h3>
                    <p>Auto-sized pane</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

**Behavior**: All panes divide the container equally. If a pane is hidden or added, remaining panes auto-adjust.

**When to use**: Responsive designs where panes should share space equally, add/remove panes dynamically.

## Add or Remove Panes Dynamically

Panes can be added or removed programmatically using the `AddPaneAsync` and `RemovePaneAsync` methods. This enables dynamic layout management based on user actions.

### Add Pane Dynamically

Add new panes at runtime using `AddPaneAsync` with pane properties and insertion index:

```csharp
@using Syncfusion.Blazor.Layouts
@using Syncfusion.Blazor.Buttons

<SfButton @onclick="@AddPane">Add Pane</SfButton>

<SfSplitter @ref="SplitterObj" Height="200px" Width="600px">
    <SplitterPanes>
        <SplitterPane Size="190px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Pane 1</h3>
                    <p>Initial pane</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="190px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Pane 2</h3>
                    <p>Initial pane</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

<style>
    .content {
        padding: 9px;
    }
</style>

@code {
    private SfSplitter SplitterObj;
    private int paneCount = 2;

    private async Task AddPane()
    {
        paneCount++;
        var newPane = new SplitterPane()
        {
            Size = "190px",
            Content = $"Pane {paneCount}",
            Min = "30px",
            Max = "250px"
        };
        
        // Add pane at index 1 (between pane 1 and pane 2)
        await this.SplitterObj.AddPaneAsync(newPane, 1);
    }
}
```

**Behavior**:
- New pane inserted at specified index
- Existing panes shift right/down
- Separators automatically added
- Layout recalculates automatically

**AddPaneAsync Parameters**:
- `pane`: `SplitterPane` object with properties (Size, Content, Min, Max, etc.)
- `index`: Where to insert (0-based position)

### Remove Pane Dynamically

Remove panes at runtime using `RemovePaneAsync` with pane index:

```csharp
@using Syncfusion.Blazor.Layouts
@using Syncfusion.Blazor.Buttons

<SfButton @onclick="@RemovePane">Remove Pane 2</SfButton>

<SfSplitter @ref="SplitterObj" Height="200px" Width="600px">
    <SplitterPanes>
        <SplitterPane Size="190px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Pane 1</h3>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="190px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Pane 2 (Removable)</h3>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="190px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Pane 3</h3>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

<style>
    .content {
        padding: 9px;
    }
</style>

@code {
    private SfSplitter SplitterObj;

    private async Task RemovePane()
    {
        // Remove pane at index 1
        await this.SplitterObj.RemovePaneAsync(1);
    }
}
```

**Behavior**:
- Pane at index removed
- Remaining panes shift left/up
- Adjacent separator removed
- Layout recalculates automatically

**RemovePaneAsync Parameters**:
- `index`: Pane index to remove (0-based)

### Complete Add/Remove Example

```csharp
@using Syncfusion.Blazor.Layouts
@using Syncfusion.Blazor.Buttons

<div style="margin-bottom: 15px;">
    <SfButton @onclick="@AddPane" CssClass="e-outline">Add Pane</SfButton>
    <SfButton @onclick="@RemovePane" CssClass="e-outline">Remove Selected</SfButton>
    <p>Total Panes: @paneCount</p>
</div>

<SfSplitter @ref="SplitterObj" Height="300px" Width="100%">
    <SplitterPanes>
        <SplitterPane Size="25%">
            <ContentTemplate>
                <div style="padding: 15px; background: #f5f5f5; height: 100%; overflow: auto;">
                    <h3>Pane 1</h3>
                    <button @onclick="@(() => RemovePaneAtIndex(0))" style="padding: 5px 10px; margin-bottom: 10px;">Remove Me</button>
                    <p>Click button to remove this pane</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="25%">
            <ContentTemplate>
                <div style="padding: 15px; background: #f9f9f9; height: 100%; overflow: auto;">
                    <h3>Pane 2</h3>
                    <button @onclick="@(() => RemovePaneAtIndex(1))" style="padding: 5px 10px; margin-bottom: 10px;">Remove Me</button>
                    <p>Click button to remove this pane</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    private SfSplitter SplitterObj;
    private int paneCount = 2;

    private async Task AddPane()
    {
        paneCount++;
        var newPane = new SplitterPane()
        {
            Size = "25%",
            Content = $"Pane {paneCount}",
            Min = "50px"
        };
        
        await this.SplitterObj.AddPaneAsync(newPane, paneCount - 1);
    }

    private async Task RemovePane()
    {
        if (paneCount > 1)
        {
            paneCount--;
            await this.SplitterObj.RemovePaneAsync(paneCount);
        }
    }

    private async Task RemovePaneAtIndex(int index)
    {
        paneCount--;
        await this.SplitterObj.RemovePaneAsync(index);
    }
}
```

### Use Cases

**Add Pane**:
- Expand layout with new content areas
- Multi-step wizards (add panels as user progresses)
- Dynamic form sections
- Tab-like interface with splitter

**Remove Pane**:
- Close optional panels
- Clean up temporary views
- Minimize interface for focused work
- Handle variable content scenarios

### Best Practices

**For Adding Panes**:
- Validate pane properties before adding
- Consider container size when adding multiple panes
- Update pane count tracking
- Provide visual feedback when pane added

**For Removing Panes**:
- Prevent removing all panes (keep at least one)
- Warn if pane contains unsaved data
- Update UI state after removal
- Recalculate dependent layouts

```csharp
// ✅ Good: Prevent removing last pane
private async Task RemovePaneIfValid(int index)
{
    if (paneCount > 1)
    {
        paneCount--;
        await this.SplitterObj.RemovePaneAsync(index);
    }
    else
    {
        Console.WriteLine("Cannot remove last pane");
    }
}

// ✅ Good: Add with validation
private async Task AddPaneIfValid(SplitterPane pane)
{
    if (pane != null && !string.IsNullOrEmpty(pane.Content))
    {
        paneCount++;
        await this.SplitterObj.AddPaneAsync(pane, paneCount - 1);
    }
}
```

---

**Next:** Read [Pane Configuration and Settings](pane-configuration.md) to learn about sizing, constraints, and content options.
