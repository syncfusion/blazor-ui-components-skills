# State Management and Persistence

## Table of Contents
- [State Persistence](#state-persistence)
- [Persisted Properties](#persisted-properties)
- [Two-Way Data Binding](#two-way-data-binding)
- [Manual State Saving](#manual-state-saving)

## State Persistence

The Splitter can automatically save pane state to browser localStorage using the `EnablePersistence` property.

### Enable Persistence

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter ID="Splitter" Height="300px" Width="100%" EnablePersistence="true">
    <SplitterPanes>
        <SplitterPane Size="25%" Min="60px" Collapsible="true">
            <ContentTemplate>
                <div style="padding: 20px;">
                    <div style="text-align: center;">
                        <div>Left pane</div>
                    </div>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="50%" Min="60px" Collapsible="true">
            <ContentTemplate>
                <div style="padding: 20px;">
                    <div style="text-align: center;">
                        <div>Middle pane</div>
                    </div>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Min="60px" Collapsible="true">
            <ContentTemplate>
                <div style="padding: 20px;">
                    <div style="text-align: center;">
                        <div>Right pane</div>
                    </div>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    // State automatically saved to localStorage under key: "Splitter"
}
```

**How it works**:
1. Set `EnablePersistence="true"`
2. Set unique `ID` (required for localStorage key)
3. User resizes/collapses panes → state saved to localStorage
4. Page refreshes → state restored from localStorage
5. State persists across sessions

**Important**: The `ID` property is crucial for persistence. Each splitter must have a unique ID.

## Persisted Properties

When persistence is enabled, these pane properties are automatically saved and restored:

| Property | What Gets Saved |
|----------|-----------------|
| `Collapsed` | Whether pane is collapsed (true/false) |
| `Size` | Current pane width/height (px or %) |
| `Min` | Minimum pane size constraint |
| `Max` | Maximum pane size constraint |

### Example: Verify Persistence

```csharp
@using Syncfusion.Blazor.Layouts

<h2>Resize panes, then refresh the page - state is restored</h2>

<SfSplitter ID="PersistentSplitter" Height="300px" Width="100%" EnablePersistence="true">
    <SplitterPanes>
        <SplitterPane Size="30%" Min="100px" Max="50%" Collapsible="true">
            <ContentTemplate>
                <div style="padding: 20px;">
                    <p>Pane 1: Resize me or collapse, then refresh</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="40%" Min="100px" Max="60%" Collapsible="true">
            <ContentTemplate>
                <div style="padding: 20px;">
                    <p>Pane 2: This state will persist</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Min="100px" Collapsible="true">
            <ContentTemplate>
                <div style="padding: 20px;">
                    <p>Pane 3: Browser refresh keeps your layout</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

## Two-Way Data Binding

Bind pane properties to component variables for programmatic control with `@bind-PropertyName`.

### Two-Way Binding Example

```csharp
@using Syncfusion.Blazor.Layouts
@using Syncfusion.Blazor.Buttons

<div>
    <SfButton @onclick="@ToggleSidebar">Toggle Sidebar</SfButton>
    <p>Sidebar Collapsed: @sidebarCollapsed</p>
</div>

<SfSplitter Height="300px" Width="100%">
    <SplitterPanes>
        <SplitterPane Size="25%" Min="60px" Collapsible="true" @bind-Collapsed="@sidebarCollapsed">
            <ContentTemplate>
                <div style="padding: 20px;">
                    <div style="text-align: center;">
                        <div>Sidebar (Two-Way Bound)</div>
                        <p>Collapsed: @sidebarCollapsed</p>
                    </div>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="50%" Min="60px" Collapsible="true">
            <ContentTemplate>
                <div style="padding: 20px;">
                    <div style="text-align: center;">
                        <div>Main Content</div>
                    </div>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Min="60px" Collapsible="true">
            <ContentTemplate>
                <div style="padding: 20px;">
                    <div style="text-align: center;">
                        <div>Right Panel</div>
                    </div>
                </div>
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

**How it works**:
- `@bind-Collapsed="@sidebarCollapsed"` binds pane state to variable
- Collapse in UI → variable updates automatically
- Toggle variable in code → pane updates automatically
- Bidirectional synchronization in real-time

### Bind Multiple Properties

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="300px" Width="100%">
    <SplitterPanes>
        <SplitterPane 
            Size="@pane1Size" 
            @bind-Size="@pane1Size"
            Min="@paneMin"
            Collapsible="true" 
            @bind-Collapsed="@pane1Collapsed">
            <ContentTemplate>
                <div style="padding: 20px;">
                    <p>Size: @pane1Size</p>
                    <p>Collapsed: @pane1Collapsed</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane 
            Size="@pane2Size" 
            @bind-Size="@pane2Size"
            Min="@paneMin"
            Collapsible="true" 
            @bind-Collapsed="@pane2Collapsed">
            <ContentTemplate>
                <div style="padding: 20px;">
                    <p>Size: @pane2Size</p>
                    <p>Collapsed: @pane2Collapsed</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    private string pane1Size = "30%";
    private string pane2Size = "70%";
    private string paneMin = "100px";
    private bool pane1Collapsed = false;
    private bool pane2Collapsed = false;
}
```

## Combined: Persistence + Two-Way Binding

Use both together for powerful state management:

```csharp
@using Syncfusion.Blazor.Layouts

<h3>State persists to localStorage AND syncs with component variables</h3>

<SfSplitter ID="AdvancedSplitter" Height="300px" Width="100%" EnablePersistence="true">
    <SplitterPanes>
        <SplitterPane 
            @bind-Size="@pane1Size" 
            @bind-Collapsed="@pane1Collapsed"
            Min="60px" 
            Collapsible="true">
            <ContentTemplate>
                <div style="padding: 20px;">
                    <h4>Sidebar</h4>
                    <p>Size: @pane1Size</p>
                    <p>Collapsed: @pane1Collapsed</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane 
            @bind-Size="@pane2Size" 
            @bind-Collapsed="@pane2Collapsed"
            Min="100px" 
            Collapsible="true">
            <ContentTemplate>
                <div style="padding: 20px;">
                    <h4>Main Area</h4>
                    <p>Size: @pane2Size</p>
                    <p>Collapsed: @pane2Collapsed</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    private string pane1Size = "25%";
    private string pane2Size = "75%";
    private bool pane1Collapsed = false;
    private bool pane2Collapsed = false;
}
```

## Manual State Saving (No Persistence)

If `EnablePersistence="false"`, manually save state via events:

```csharp
@using Syncfusion.Blazor.Layouts

<button @onclick="@SaveState">Save State</button>
<button @onclick="@RestoreState">Restore State</button>

<SfSplitter ID="ManualSplitter" Height="300px" Width="100%">
    <SplitterEvents OnResizeStop="@OnResize" Collapsed="@OnCollapsed" Expanded="@OnExpanded" />
    <SplitterPanes>
        <SplitterPane Size="30%" Collapsible="true">
            <ContentTemplate><div style="padding: 20px;">Pane 1</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate><div style="padding: 20px;">Pane 2</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    private SplitterState savedState = new();

    private void OnResize(ResizingEventArgs args)
    {
        // Save size changes
    }

    private void OnCollapsed(ExpandedEventArgs args)
    {
        // Save collapsed state
    }

    private void OnExpanded(ExpandedEventArgs args)
    {
        // Save expanded state
    }

    private void SaveState()
    {
        // Manually serialize and save state
        savedState.Pane1Size = "30%";
        savedState.Pane1Collapsed = false;
        Console.WriteLine("State saved");
    }

    private void RestoreState()
    {
        // Manually restore state from saved data
        Console.WriteLine("State restored");
    }

    public class SplitterState
    {
        public string Pane1Size { get; set; }
        public bool Pane1Collapsed { get; set; }
    }
}
```

## Troubleshooting

**Issue**: Persistence not working
- Verify `EnablePersistence="true"` is set
- Verify unique `ID` is provided (not empty)
- Check browser DevTools → Application → LocalStorage for key with splitter ID
- Ensure localStorage is not disabled or full

**Issue**: State not updating with two-way binding
- Verify `@bind-PropertyName` syntax is correct
- Check that property matches the binding (e.g., @bind-Collapsed not @bind-Collapsible)
- Ensure public property is defined in @code block

---

**Next:** Read [Advanced Scenarios and Layouts](advanced-scenarios.md) for complex layout patterns and best practices.
