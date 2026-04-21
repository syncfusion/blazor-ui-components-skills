# Styling and Theming

## Table of Contents
- [Theme Selection](#theme-selection)
- [Customize Split Bar](#customize-split-bar)
- [Customize Resize Handle](#customize-resize-handle)
- [CSS Selectors](#css-selectors)
- [Custom Styling](#custom-styling)

## Theme Selection

Syncfusion provides multiple built-in themes. Choose and include the appropriate CSS file.

### Available Themes

| Theme | CSS File | Best For |
|-------|----------|----------|
| Bootstrap 5 | bootstrap5.css | Bootstrap 5 apps |
| Material 3 | material3.css | Material Design apps |
| Fluent 2 | fluent2.css | Microsoft Fluent Design |
| Tailwind | tailwind.css | Tailwind CSS projects |

### Include Theme in index.html (WebAssembly)

```html
<head>
    <!-- Other head elements -->
    
    <!-- Syncfusion Bootstrap 5 Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    
    <!-- Syncfusion Script -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

### Include Theme in App.razor (Blazor Web App)

```html
<head>
    <!-- Syncfusion Fluent 2 Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/fluent2.css" rel="stylesheet" />
</head>

<body>
    <!-- Other body elements -->
    
    <!-- Syncfusion Script -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</body>
```

### Change Theme at Runtime

```csharp
@page "/themes"
@inject IJSRuntime JS

<button @onclick="@(() => ChangeTheme("bootstrap5"))">Bootstrap 5</button>
<button @onclick="@(() => ChangeTheme("material3"))">Material 3</button>
<button @onclick="@(() => ChangeTheme("fluent2"))">Fluent 2</button>

<SfSplitter Height="300px" Width="100%">
    <SplitterPanes>
        <SplitterPane Size="30%" Collapsible="true">
            <ContentTemplate><div style="padding: 20px;">Left Pane</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate><div style="padding: 20px;">Right Pane</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    private async Task ChangeTheme(string themeName)
    {
        // Remove current theme
        await JS.InvokeVoidAsync("eval", $@"
            document.querySelectorAll('link[href*=syncfusion]').forEach(link => {{
                if (link.href.includes('Themes')) link.remove();
            }});
        ");
        
        // Add new theme
        await JS.InvokeVoidAsync("eval", $@"
            const link = document.createElement('link');
            link.rel = 'stylesheet';
            link.href = '_content/Syncfusion.Blazor.Themes/{themeName}.css';
            document.head.appendChild(link);
        ");
    }
}
```

## Customize Split Bar

Style the separator bar using CSS selectors.

### Horizontal Split Bar Color

```css
/* Default split bar color */
.e-splitter .e-split-bar.e-split-bar-horizontal {
    background: blue;
}

/* Split bar color on hover */
.e-splitter .e-split-bar.e-split-bar-horizontal.e-split-bar-hover {
    background: darkblue;
}

/* Split bar color when active (being dragged) */
.e-splitter .e-split-bar.e-split-bar-horizontal.e-split-bar-active {
    background: lightblue;
}
```

### Vertical Split Bar Color

```css
/* Default split bar color */
.e-splitter .e-split-bar.e-split-bar-vertical {
    background: blue;
}

/* Split bar color on hover */
.e-splitter .e-split-bar.e-split-bar-vertical.e-split-bar-hover {
    background: darkblue;
}

/* Split bar color when active */
.e-splitter .e-split-bar.e-split-bar-vertical.e-split-bar-active {
    background: lightblue;
}
```

### Example: Custom Split Bar

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="300px" Width="100%">
    <SplitterPanes>
        <SplitterPane Size="30%">
            <ContentTemplate><div style="padding: 20px;">Left Pane</div></ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate><div style="padding: 20px;">Right Pane</div></ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

<style>
    .e-splitter .e-split-bar.e-split-bar-horizontal {
        background: linear-gradient(to right, #ff6b6b, #ee5a6f);
    }
    
    .e-splitter .e-split-bar.e-split-bar-horizontal.e-split-bar-hover,
    .e-splitter .e-split-bar.e-split-bar-horizontal.e-split-bar-active {
        background: linear-gradient(to right, #ff5252, #d63031);
    }
</style>
```

## Customize Resize Handle

Style the resize gripper and arrows.

### Resize Handler Color

```css
/* Default resize handler color */
.e-splitter .e-split-bar.e-split-bar-horizontal .e-resize-handler {
    color: rgba(20, 27, 233, 0.54);
}

/* Resize handler color on hover */
.e-splitter .e-split-bar.e-split-bar-horizontal.e-split-bar-hover .e-resize-handler {
    color: green;
}

/* Vertical resize handler */
.e-splitter .e-split-bar.e-split-bar-vertical .e-resize-handler {
    color: rgba(20, 27, 233, 0.54);
}

.e-splitter .e-split-bar.e-split-bar-vertical.e-split-bar-hover .e-resize-handler {
    color: green;
}
```

### Resize Arrows (Horizontal)

```css
/* Horizontal split bar arrows */
.e-splitter .e-split-bar.e-split-bar-horizontal.e-split-bar-hover .e-navigate-arrow.e-arrow-left::before,
.e-splitter .e-split-bar.e-split-bar-horizontal.e-split-bar-active .e-navigate-arrow.e-arrow-left::before,
.e-splitter .e-split-bar.e-split-bar-horizontal.e-split-bar-hover .e-navigate-arrow.e-arrow-right::before,
.e-splitter .e-split-bar.e-split-bar-horizontal.e-split-bar-active .e-navigate-arrow.e-arrow-right::before {
    background-color: green;
}

/* Arrow circular border */
.e-splitter .e-split-bar.e-split-bar-horizontal.e-split-bar-hover .e-navigate-arrow.e-arrow-left,
.e-splitter .e-split-bar.e-split-bar-horizontal.e-split-bar-hover .e-navigate-arrow.e-arrow-right {
    border-color: rgba(33, 227, 22, 0.5);
}
```

### Resize Arrows (Vertical)

```css
/* Vertical split bar arrows */
.e-splitter .e-split-bar.e-split-bar-vertical.e-split-bar-hover .e-navigate-arrow.e-arrow-up::before,
.e-splitter .e-split-bar.e-split-bar-vertical.e-split-bar-active .e-navigate-arrow.e-arrow-up::before,
.e-splitter .e-split-bar.e-split-bar-vertical.e-split-bar-hover .e-navigate-arrow.e-arrow-down::before,
.e-splitter .e-split-bar.e-split-bar-vertical.e-split-bar-active .e-navigate-arrow.e-arrow-down::before {
    background-color: green;
}

/* Arrow circular border */
.e-splitter .e-split-bar.e-split-bar-vertical.e-split-bar-hover .e-navigate-arrow.e-arrow-up,
.e-splitter .e-split-bar.e-split-bar-vertical.e-split-bar-hover .e-navigate-arrow.e-arrow-down {
    border-color: rgba(33, 227, 22, 0.5);
}
```

## Hide Resize Handler

Make the resize handler invisible while keeping resize functionality:

```css
/* Hide horizontal resize handler */
.e-splitter .e-split-bar.e-split-bar-horizontal .e-resize-handler {
    display: none;
}

/* Hide vertical resize handler */
.e-splitter .e-split-bar.e-split-bar-vertical .e-resize-handler {
    display: none;
}
```

## CSS Selectors

Common CSS selectors for Splitter styling:

| Selector | Purpose |
|----------|---------|
| `.e-splitter` | Main splitter container |
| `.e-pane` | Individual pane |
| `.e-split-bar` | Separator bar |
| `.e-split-bar-horizontal` | Horizontal separator |
| `.e-split-bar-vertical` | Vertical separator |
| `.e-split-bar-hover` | Separator on hover |
| `.e-split-bar-active` | Separator being dragged |
| `.e-resize-handler` | Resize gripper icon |
| `.e-navigate-arrow` | Arrow indicators |
| `.e-collapsible-icon` | Collapse/expand button |

## Custom Styling Example

```csharp
@using Syncfusion.Blazor.Layouts

<h2>Custom Styled Splitter</h2>

<SfSplitter Height="400px" Width="100%" CssClass="custom-splitter">
    <SplitterPanes>
        <SplitterPane Size="25%" Collapsible="true">
            <ContentTemplate>
                <div class="pane-content">
                    <h3>Navigation</h3>
                    <ul>
                        <li>Item 1</li>
                        <li>Item 2</li>
                        <li>Item 3</li>
                    </ul>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="60%">
            <ContentTemplate>
                <div class="pane-content">
                    <h3>Main Content</h3>
                    <p>Primary content area</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="15%">
            <ContentTemplate>
                <div class="pane-content">
                    <h3>Details</h3>
                    <p>Secondary panel</p>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

<style>
    /* Custom splitter styling */
    .custom-splitter .e-pane {
        background: #f5f5f5;
        border: 1px solid #ddd;
    }
    
    .custom-splitter .pane-content {
        padding: 20px;
        background: white;
        height: 100%;
        overflow: auto;
    }
    
    .custom-splitter .pane-content h3 {
        margin-top: 0;
        color: #333;
        border-bottom: 2px solid #007bff;
        padding-bottom: 10px;
    }
    
    /* Custom split bar */
    .custom-splitter .e-split-bar.e-split-bar-horizontal {
        background: linear-gradient(to right, #e0e0e0 0%, #f0f0f0 50%, #e0e0e0 100%);
        border-left: 1px solid #999;
        border-right: 1px solid #999;
    }
    
    .custom-splitter .e-split-bar.e-split-bar-horizontal.e-split-bar-hover,
    .custom-splitter .e-split-bar.e-split-bar-horizontal.e-split-bar-active {
        background: linear-gradient(to right, #007bff 0%, #0056b3 50%, #007bff 100%);
    }
    
    /* Custom resize handler */
    .custom-splitter .e-split-bar .e-resize-handler {
        color: #666;
        font-size: 16px;
    }
</style>
```

---

**Next:** Read [State Management and Persistence](state-management.md) to save and restore splitter state.
