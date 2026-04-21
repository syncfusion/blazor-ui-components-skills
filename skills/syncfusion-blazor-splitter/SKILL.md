---
name: syncfusion-blazor-splitter
description: Implement Blazor Splitter layouts with pane configuration, resizing, and state management. Use when creating split pane layouts, handling user interactions, managing pane states, and applying custom styling to Syncfusion Blazor Splitter components.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Layout Components"
---

# Implementing Syncfusion Blazor Splitter

The Syncfusion Blazor Splitter component is a layout-based container that enables you to split a container area into resizable and collapsible panes. Use this skill when you need to create complex layouts with multiple content areas that users can adjust.

## When to Use This Skill

Use this skill when you need to:

- **Create split layouts** — Divide your UI into multiple panes (2, 3, or more) that users can resize
- **Handle pane configurations** — Control pane sizing, minimum/maximum constraints, visibility, and collapsibility
- **Manage user interactions** — Respond to resize events, collapse/expand actions, and state changes
- **Build complex interfaces** — Create code editor layouts, dashboard panels, or multi-section dashboards
- **Persist pane state** — Save and restore pane sizes and collapse states across page refreshes
- **Apply custom styling** — Customize split bars, resize handles, and overall component appearance

## Component Overview

The Splitter divides container space into independent panes using flexible layout:

- **Default**: Horizontal orientation with vertical separators (left-to-right split)
- **Vertical mode**: Vertical orientation with horizontal separators (top-to-bottom split)
- **Nested splitters**: Support for complex layouts by nesting splitters within panes
- **Resizable**: Users drag separators to adjust pane sizes in real-time
- **Collapsible**: Optional collapse/expand buttons to hide panes and recover space
- **Persistent**: Built-in state persistence to localStorage for user preferences

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup
- WebAssembly vs Blazor Web App configuration
- Namespace imports and service registration
- Stylesheet and script resources
- Basic horizontal and vertical splitter implementation
- First render and initialization

### Split Panes and Layouts
📄 **Read:** [references/split-panes.md](references/split-panes.md)
- Horizontal layout (default orientation)
- Vertical layout (Orientation.Vertical)
- Multiple panes configuration
- Separator customization (SeparatorSize)
- Nested splitter patterns
- Flex-based auto-sizing behavior
- Add and remove panes dynamically (AddPaneAsync, RemovePaneAsync)

### Pane Configuration and Settings
📄 **Read:** [references/pane-configuration.md](references/pane-configuration.md)
- Pane sizing (fixed pixels, percentages, auto)
- Min/Max size constraints
- Visibility control (show/hide panes)
- Collapsible panes with toggle buttons
- Content templates and plain text
- Integrating other Blazor components

### Events and Interactions
📄 **Read:** [references/events-and-interactions.md](references/events-and-interactions.md)
- Created event lifecycle
- OnResizeStart, Resizing, OnResizeStop events
- OnCollapse, OnExpand event handlers
- Collapsed, Expanded completion events
- Event args and callback structures
- Common event handling patterns

### Resizing, Expand, and Collapse
📄 **Read:** [references/resizing-and-expand-collapse.md](references/resizing-and-expand-collapse.md)
- Resizing behavior and gripper element
- Prevent resizing (Resizable property)
- Min/Max size validation during resize
- Enable/disable collapsible panes
- Programmatic expand/collapse methods
- Initial collapsed state configuration
- Customize resize icons and cursors
- Separator template customization (custom HTML templates)

### Styling and Theming
📄 **Read:** [references/styling-and-theming.md](references/styling-and-theming.md)
- Customize split bar appearance
- Style resize handles
- Customize resize arrows
- Hide/show resize handlers
- CSS class selectors for panes
- Theme integration (Bootstrap, Fluent, etc.)

### State Management and Persistence
📄 **Read:** [references/state-management.md](references/state-management.md)
- Enable state persistence (EnablePersistence)
- Two-way data binding (@bind-Collapsed, @bind-Size)
- Save/restore pane sizes and collapsed states
- LocalStorage integration
- Binding pane properties to component state

### Advanced Scenarios and Layouts
📄 **Read:** [references/advanced-scenarios.md](references/advanced-scenarios.md)
- Code editor style layout (nested horizontal + vertical)
- Outlook style layout with TreeView and ListView
- Dashboard and multi-panel layouts
- Performance optimization for complex layouts
- Nested splitters best practices
- Common troubleshooting and edge cases

### Complete API Reference
📄 **Read:** [references/api-reference.md](references/api-reference.md)
- All SfSplitter properties, methods, and events
- SplitterPane property details
- Event arguments and handlers
- Complete code examples for each API
- Content property vs ContentTemplate comparison
- RTL, persistence, and advanced property usage

## Quick Start Example

### Horizontal Splitter with Two Panes

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="240px" Width="100%">
    <SplitterPanes>
        <SplitterPane Size="300px">
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Left Pane</h3>
                    <p>Content here</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate>
                <div style="padding: 10px;">
                    <h3>Right Pane</h3>
                    <p>Content here</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

### Vertical Splitter with Collapse Buttons

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="300px" Width="100%" Orientation="Orientation.Vertical">
    <SplitterPanes>
        <SplitterPane Size="100px" Collapsible="true">
            <ContentTemplate>
                <div style="padding: 10px;">Top Pane</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Collapsible="true">
            <ContentTemplate>
                <div style="padding: 10px;">Bottom Pane</div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

## Common Patterns

### Pattern 1: Responsive Three-Pane Layout

```csharp
<SfSplitter Height="400px" Width="100%">
    <SplitterPanes>
        <SplitterPane Size="20%" Min="150px" Collapsible="true">
            <ContentTemplate><div>Sidebar</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="60%" Min="300px">
            <ContentTemplate><div>Main Content</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="20%" Min="150px" Collapsible="true">
            <ContentTemplate><div>Details Panel</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

**When to use**: Dashboard layouts, IDE-style interfaces, content management systems.

### Pattern 2: Nested Splitters (Code Editor Layout)

```csharp
<SfSplitter Height="400px" Orientation="Orientation.Vertical">
    <SplitterPanes>
        <SplitterPane Size="60%">
            <SfSplitter Height="100%">
                <SplitterPanes>
                    <SplitterPane Size="30%"><ContentTemplate>HTML</ContentTemplate></SplitterPane>
                    <SplitterPane Size="30%"><ContentTemplate>CSS</ContentTemplate></SplitterPane>
                    <SplitterPane Size="40%"><ContentTemplate>JS</ContentTemplate></SplitterPane>
                </SplitterPanes>
            </SfSplitter>
        </SplitterPane>
        <SplitterPane Size="40%"><ContentTemplate>Preview</ContentTemplate></SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

**When to use**: Code editors, preview panes, complex dashboard layouts.

### Pattern 3: Collapsible Sidebar with State Binding

```csharp
<SfSplitter Height="300px" Width="100%">
    <SplitterPanes>
        <SplitterPane Size="250px" Min="50px" Collapsible="true" @bind-Collapsed="@sidebarCollapsed">
            <ContentTemplate><div>Navigation</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate><div>Main Area</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    private bool sidebarCollapsed = false;
}
```

**When to use**: Applications needing dynamic sidebar toggles synchronized with UI state.

## Key Props and Configuration

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Height` | string | Container height | Required |
| `Width` | string | Container width | 100% |
| `Orientation` | Orientation | Horizontal or Vertical | Horizontal |
| `SeparatorSize` | int | Separator thickness in pixels | 1 |
| `EnablePersistence` | bool | Save state to localStorage | false |
| `Size` | string | Pane width/height (px or %) | Auto |
| `Min` | string | Minimum pane size | 0 |
| `Max` | string | Maximum pane size | Unlimited |
| `Resizable` | bool | Allow pane resizing | true |
| `Collapsible` | bool | Show collapse/expand buttons | false |
| `Visible` | bool | Show/hide pane | true |
| `Collapsed` | bool | Initial collapsed state | false |

## Common Use Cases

1. **Code Editor Interface** — Top panes for code tabs, bottom pane for output. Use nested horizontal splitters in top pane.

2. **Admin Dashboard** — Left sidebar for navigation, main area for content. Add right panel for details or settings.

3. **Mail Client** — Left pane for folder list, center for email list, right for email content.

4. **Project Management Tool** — Multiple collapsible panels for timeline, tasks, and gantt chart.

5. **Data Analysis Dashboard** — Resizable panels for charts, tables, and filters with state persistence.

---

**Next Steps:**
- Start with [Getting Started](references/getting-started.md) for setup instructions
- Read [Split Panes](references/split-panes.md) to understand layout options
- Explore [Pane Configuration](references/pane-configuration.md) for customization
- Check [Advanced Scenarios](references/advanced-scenarios.md) for complex layouts
