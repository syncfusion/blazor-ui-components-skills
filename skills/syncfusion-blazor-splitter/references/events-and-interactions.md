# Events and Interactions

## Table of Contents
- [Component Lifecycle](#component-lifecycle)
- [Resize Events](#resize-events)
- [Collapse/Expand Events](#collapseexpand-events)
- [Common Patterns](#common-patterns)
- [Event Args Reference Table](#event-args-reference-table)

## Component Lifecycle

### Created Event

Fires after the Splitter component initializes with all panes rendered.

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter @ref="SplitterObj">
    <SplitterEvents Created="@CreatedHandler" />
    <SplitterPanes>
        <SplitterPane Size="200px">
            <ContentTemplate><div>Pane 1</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="200px">
            <ContentTemplate><div>Pane 2</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    private SfSplitter SplitterObj;

    public void CreatedHandler(Object args)
    {
        Console.WriteLine("Splitter created successfully");
        // Initialize pane state, set focus, load data
    }
}
```

**Use case**: Set initial focus, load pane content dynamically, initialize related components.

## Resize Events

Three resize events fire during the pane resizing workflow:

### OnResizeStart

Fires when user begins dragging the separator:

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="300px" Width="100%">
    <SplitterEvents OnResizeStart="@OnResizeStartHandler" />
    <SplitterPanes>
        <SplitterPane Size="50%">
            <ContentTemplate><div>Pane 1</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate><div>Pane 2</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    public void OnResizeStartHandler(ResizeEventArgs args)
    {
        Console.WriteLine($"Resize started on pane: {args.Pane}");
        // Save initial pane state, prepare for changes
    }
}
```

**ResizeEventArgs** (Properties):
- `Cancel`: bool - Cancel the resize action
- `Element`: DOM - Root element of Splitter component
- `Event`: EventArgs - Original event arguments
- `Index`: int[] - Array of indices representing expanded panes order
- `Name`: string - Name of the event ("OnResizeStart")
- `Pane`: List<DOM> - DOM elements of resizing panes
- `Separator`: DOM - Separator element between panes

### Resizing

Fires continuously as separator moves (use throttle if performance issue):

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="300px" Width="100%">
    <SplitterEvents Resizing="@ResizingHandler" />
    <SplitterPanes>
        <SplitterPane Size="50%">
            <ContentTemplate><div>Pane 1</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate><div>Pane 2</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    public void ResizingHandler(ResizingEventArgs args)
    {
        // Called many times during drag, log cautiously
        Console.WriteLine($"Resizing: {args.NextPaneSize}");
    }
}
```

**ResizingEventArgs** (Properties):
- `Element`: DOM - Root element of Splitter component
- `Event`: EventArgs - Original event arguments
- `Index`: int[] - Array of indices representing expanded panes order
- `Name`: string - Name of the event ("Resizing")
- `Pane`: List<DOM> - DOM elements of resizing panes
- `PaneSize`: double[] - Array of current pane sizes during resize
- `Separator`: DOM - Separator element between panes

### OnResizeStop

Fires when user releases separator (resizing complete):

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="300px" Width="100%">
    <SplitterEvents OnResizeStop="@OnResizeStopHandler" />
    <SplitterPanes>
        <SplitterPane Size="50%">
            <ContentTemplate><div>Pane 1</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate><div>Pane 2</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    public void OnResizeStopHandler(ResizingEventArgs args)
    {
        Console.WriteLine($"Resize stopped. New pane sizes: {string.Join(", ", args.PaneSize)}");
        // Refresh content, save state, recalculate layout
    }
}
```

**ResizingEventArgs** (Properties for OnResizeStop):
- `Element`: DOM - Root element of Splitter component
- `Event`: EventArgs - Original event arguments
- `Index`: int[] - Array of indices representing expanded panes order
- `Name`: string - Name of the event ("OnResizeStop")
- `Pane`: List<DOM> - DOM elements of resizing panes
- `PaneSize`: double[] - Array of final pane sizes after resize complete
- `Separator`: DOM - Separator element between panes

## Collapse/Expand Events

### OnCollapse

Fires before a pane collapses (can cancel with `args.Cancel`):

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="300px" Width="100%">
    <SplitterEvents OnCollapse="@OnCollapseHandler" />
    <SplitterPanes>
        <SplitterPane Size="30%" Collapsible="true">
            <ContentTemplate><div>Sidebar (Collapsible)</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate><div>Main Content</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    public void OnCollapseHandler(BeforeExpandEventArgs args)
    {
        Console.WriteLine($"Before collapse pane at index: {string.Join(", ", args.Index)}");
        // Cancel collapse if needed:
        // args.Cancel = true;
    }
}
```

**BeforeExpandEventArgs** (Properties):
- `Cancel`: bool - Set to true to prevent collapse action
- `Element`: DOM - Root element of Splitter component
- `Event`: EventArgs - Original event arguments
- `Index`: int[] - Array of indices representing expanded panes order
- `Name`: string - Name of the event ("OnCollapse")
- `Pane`: List<DOM> - DOM elements of panes
- `Separator`: DOM - Separator element

### OnExpand

Fires before a pane expands:

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="300px" Width="100%">
    <SplitterEvents OnExpand="@OnExpandHandler" />
    <SplitterPanes>
        <SplitterPane Size="30%" Collapsible="true">
            <ContentTemplate><div>Sidebar</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate><div>Main Content</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    public void OnExpandHandler(BeforeExpandEventArgs args)
    {
        Console.WriteLine($"Before expand pane at index: {string.Join(", ", args.Index)}");
        // Can cancel expand if needed: args.Cancel = true;
    }
}
```

**Note**: OnExpand also uses `BeforeExpandEventArgs` with same properties as OnCollapse - fires before expand action begins.

### Collapsed

Fires after pane successfully collapses:

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="300px" Width="100%">
    <SplitterEvents Collapsed="@CollapsedHandler" />
    <SplitterPanes>
        <SplitterPane Size="30%" Collapsible="true">
            <ContentTemplate><div>Sidebar</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate><div>Main Content</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    public void CollapsedHandler(ExpandedEventArgs args)
    {
        Console.WriteLine($"Successfully collapsed pane at index: {string.Join(", ", args.Index)}");
        Console.WriteLine($"New pane sizes: {string.Join(", ", args.PaneSizes)}");
        // Update UI state, save preferences
    }
}
```

**ExpandedEventArgs** (Properties for Collapsed event):
- `Element`: DOM - Root element of Splitter component
- `Event`: EventArgs - Original event arguments
- `Index`: int[] - Array of indices representing expanded panes order
- `Name`: string - Name of the event ("Collapsed")
- `Pane`: List<DOM> - DOM elements of panes
- `PaneSizes`: double[] - Array of pane sizes after collapse
- `Separator`: DOM - Separator element

### Expanded

Fires after pane successfully expands:

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="300px" Width="100%">
    <SplitterEvents Expanded="@ExpandedHandler" />
    <SplitterPanes>
        <SplitterPane Size="30%" Collapsible="true">
            <ContentTemplate><div>Sidebar</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate><div>Main Content</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    public void ExpandedHandler(ExpandedEventArgs args)
    {
        Console.WriteLine($"Successfully expanded pane at index: {string.Join(", ", args.Index)}");
        Console.WriteLine($"New pane sizes: {string.Join(", ", args.PaneSizes)}");
    }
}
```

**ExpandedEventArgs** (Properties for Expanded event):
- `Element`: DOM - Root element of Splitter component
- `Event`: EventArgs - Original event arguments
- `Index`: int[] - Array of indices representing expanded panes order
- `Name`: string - Name of the event ("Expanded")
- `Pane`: List<DOM> - DOM elements of panes
- `PaneSizes`: double[] - Array of pane sizes after expand
- `Separator`: DOM - Separator element

## Common Patterns

### Pattern 1: Sync Splitter State with Component State

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="300px" Width="100%">
    <SplitterEvents Collapsed="@OnPaneCollapsed" Expanded="@OnPaneExpanded" />
    <SplitterPanes>
        <SplitterPane Size="25%" Collapsible="true" Index="0">
            <ContentTemplate><div>Sidebar (Index 0)</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate><div>Content</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

<div>
    <p>Sidebar collapsed: @sidebarCollapsed</p>
</div>

@code {
    private bool sidebarCollapsed = false;

    private void OnPaneCollapsed(ExpandedEventArgs args)
    {
        // Index is an array of indices - check if index 0 (sidebar) is in the array
        if (args.Index != null && args.Index.Length > 0 && args.Index[0] == 0)
            sidebarCollapsed = true;
    }

    private void OnPaneExpanded(ExpandedEventArgs args)
    {
        if (args.Index != null && args.Index.Length > 0 && args.Index[0] == 0)
            sidebarCollapsed = false;
    }
}
```

**Use case**: Update page UI to reflect splitter state (show/hide buttons, change icons, etc.).

### Pattern 2: Refresh Content on Resize

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="400px" Width="100%">
    <SplitterEvents OnResizeStop="@RefreshPaneContent" />
    <SplitterPanes>
        <SplitterPane Size="50%" Min="300px">
            @if (chartComponent != null)
            {
                <SfChart @ref="chartComponent" Title="Chart">
                    <!-- Chart content -->
                </SfChart>
            }
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate>
                <div style="padding: 20px;">Details Panel</div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    private SfChart chartComponent;

    private async Task RefreshPaneContent(ResizingEventArgs args)
    {
        if (chartComponent != null)
        {
            await chartComponent.Refresh();
        }
    }
}
```

**Use case**: Charts, grids, and other components need refresh when container resizes.

### Pattern 3: Prevent Collapse Based on Condition

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="300px" Width="100%">
    <SplitterEvents OnCollapse="@PreventCollapseIfNeeded" />
    <SplitterPanes>
        <SplitterPane Size="25%" Collapsible="true" Index="0">
            <ContentTemplate><div>Important Content (Cannot collapse if dirty)</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate><div>Other Content</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    private bool hasUnsavedChanges = false;

    private void PreventCollapseIfNeeded(BeforeExpandEventArgs args)
    {
        // Check if first pane (index 0) is being collapsed
        if (args.Index != null && args.Index.Length > 0 && args.Index[0] == 0 && hasUnsavedChanges)
        {
            // Prevent collapse if user has unsaved changes
            args.Cancel = true;
            Console.WriteLine("Cannot collapse: Unsaved changes exist");
        }
    }
}
```

**Use case**: Form validation, unsaved changes warning, dependency checks.

### Pattern 4: Complete Event Handling Example

```csharp
@using Syncfusion.Blazor.Layouts

<div>
    <p>Events Log:</p>
    <div style="border: 1px solid #ccc; height: 150px; overflow: auto; padding: 10px;">
        @foreach (var log in eventLogs)
        {
            <div>@log</div>
        }
    </div>
</div>

<SfSplitter Height="300px" Width="100%" @ref="splitter">
    <SplitterEvents 
        Created="@OnCreated"
        OnResizeStart="@OnStart"
        Resizing="@OnResizing"
        OnResizeStop="@OnStop"
        OnCollapse="@OnCollapsing"
        Collapsed="@OnCollapsed"
        OnExpand="@OnExpanding"
        Expanded="@OnExpanded" />
    <SplitterPanes>
        <SplitterPane Size="30%" Collapsible="true">
            <ContentTemplate><div>Pane 1</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate><div>Pane 2</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    private SfSplitter splitter;
    private List<string> eventLogs = new();

    private void LogEvent(string eventName, object args)
    {
        string details = eventName switch
        {
            "OnResizeStart" => $"Indices: {string.Join(",", ((ResizeEventArgs)args).Index ?? Array.Empty<int>())}",
            "Resizing" => $"Sizes: {string.Join(",", ((ResizingEventArgs)args).PaneSize ?? Array.Empty<double>())}",
            "OnResizeStop" => $"Final sizes: {string.Join(",", ((ResizingEventArgs)args).PaneSize ?? Array.Empty<double>())}",
            "OnCollapse" => $"Collapsing indices: {string.Join(",", ((BeforeExpandEventArgs)args).Index ?? Array.Empty<int>())}",
            "Collapsed" => $"Collapsed indices: {string.Join(",", ((ExpandedEventArgs)args).Index ?? Array.Empty<int>())}",
            "OnExpand" => $"Expanding indices: {string.Join(",", ((BeforeExpandEventArgs)args).Index ?? Array.Empty<int>())}",
            "Expanded" => $"Expanded indices: {string.Join(",", ((ExpandedEventArgs)args).Index ?? Array.Empty<int>())}",
            _ => ""
        };
        
        eventLogs.Add($"[{DateTime.Now:HH:mm:ss}] {eventName} - {details}");
        if (eventLogs.Count > 50) eventLogs.RemoveAt(0); // Keep last 50 logs
    }

    private void OnCreated(Object args) => LogEvent("Created", args);
    private void OnStart(ResizeEventArgs args) => LogEvent("OnResizeStart", args);
    private void OnResizing(ResizingEventArgs args) => LogEvent("Resizing", args);
    private void OnStop(ResizingEventArgs args) => LogEvent("OnResizeStop", args);
    private void OnCollapsing(BeforeExpandEventArgs args) => LogEvent("OnCollapse", args);
    private void OnCollapsed(ExpandedEventArgs args) => LogEvent("Collapsed", args);
    private void OnExpanding(BeforeExpandEventArgs args) => LogEvent("OnExpand", args);
    private void OnExpanded(ExpandedEventArgs args) => LogEvent("Expanded", args);
}
```

---

## Event Args Reference Table

| EventArgs Class | Event | Properties | Description |
|-----------------|-------|-----------|-------------|
| **ResizeEventArgs** | OnResizeStart | Cancel, Element, Event, Index[], Name, Pane, Separator | Fires when user starts dragging separator |
| **ResizingEventArgs** | Resizing, OnResizeStop | Element, Event, Index[], Name, Pane, PaneSize[], Separator | Fires during and after drag (use PaneSize[] for current sizes) |
| **BeforeExpandEventArgs** | OnCollapse, OnExpand | Cancel, Element, Event, Index[], Name, Pane, Separator | Fires before collapse/expand (Cancel to prevent) |
| **ExpandedEventArgs** | Collapsed, Expanded | Element, Event, Index[], Name, Pane, PaneSizes[], Separator | Fires after collapse/expand (includes final sizes) |

### Key Differences

| Property | ResizeEventArgs | ResizingEventArgs | BeforeExpandEventArgs | ExpandedEventArgs |
|----------|---|---|---|---|
| `Cancel` | ✅ | ❌ | ✅ | ❌ |
| `Index[]` | ✅ | ✅ | ✅ | ✅ |
| `PaneSize[]` | ❌ | ✅ (current sizes) | ❌ | ❌ |
| `PaneSizes[]` | ❌ | ❌ | ❌ | ✅ (final sizes) |
| `Element` | ✅ | ✅ | ✅ | ✅ |
| `Event` | ✅ | ✅ | ✅ | ✅ |
| `Pane` | ✅ | ✅ | ✅ | ✅ |
| `Separator` | ✅ | ✅ | ✅ | ✅ |

### Common Patterns Using Event Args

**Reading Index Array**:
```csharp
// Index is an int array representing expanded panes
if (args.Index != null && args.Index.Length > 0)
{
    int firstExpandedPane = args.Index[0];
    Console.WriteLine($"Expanded pane indices: {string.Join(",", args.Index)}");
}
```

**Reading Pane Sizes**:
```csharp
// Use PaneSize[] in Resizing/OnResizeStop for current sizes
// Use PaneSizes[] in Collapsed/Expanded for final sizes
if (args.PaneSize != null)
{
    Console.WriteLine($"Current sizes: {string.Join(", ", args.PaneSize)}");
}

if (args.PaneSizes != null)
{
    Console.WriteLine($"Final sizes: {string.Join(", ", args.PaneSizes)}");
}
```

**Accessing DOM Elements**:
```csharp
// Pane is a List<DOM> of pane elements
if (args.Pane != null && args.Pane.Count > 0)
{
    DOM firstPane = args.Pane[0];
    // Can use with JavaScript interop if needed
}

// Separator is the split bar DOM element
DOM separatorElement = args.Separator;
```

---

**Next:** Read [Resizing, Expand, and Collapse](resizing-and-expand-collapse.md) for implementation of resize controls and programmatic methods.
