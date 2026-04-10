# Design User Interface with Blazor AppBar Component

## Table of Contents
- [Overview](#overview)
- [Spacer](#spacer)
- [Separator](#separator)
- [Media Query](#media-query)
- [Designing AppBar with Menu](#designing-appbar-with-menu)
- [Designing AppBar with Buttons](#designing-appbar-with-buttons)
- [Designing AppBar with SideBar](#designing-appbar-with-sidebar)

## Overview

The Blazor AppBar provides several components and patterns for creating sophisticated user interfaces. This guide covers layout components (Spacer, Separator), responsive design patterns (Media Query), and integration with other Syncfusion components (Menu, Buttons, Sidebar).

## Spacer

`AppBarSpacer` provides spacing between AppBar contents, creating flexible layouts where elements can be pushed to different sides of the AppBar.

**When to use:**
- Separating left-aligned and right-aligned content
- Creating flexible responsive layouts
- Pushing action buttons to the right side
- Grouping related elements with space between groups

**How it works:**
- Acts as a flexible spacer that expands to fill available space
- Content before the spacer aligns to the left
- Content after the spacer aligns to the right
- Multiple spacers divide space equally

**Basic example:**

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfAppBar ColorMode="AppBarColor.Primary">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-home"></SfButton>
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-cut"></SfButton>
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-pan"></SfButton>
</SfAppBar>
```

This example creates three sections:
- Home button on the left
- Cut button in the center
- Pan button on the right

**Common pattern - Logo and actions:**

```cshtml
<SfAppBar ColorMode="AppBarColor.Primary">
    <!-- Left: Logo and navigation -->
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
    <span class="app-title">My Application</span>
    
    <!-- Spacer pushes everything after it to the right -->
    <AppBarSpacer></AppBarSpacer>
    
    <!-- Right: User actions -->
    <SfButton CssClass="e-inherit" IconCss="e-icons e-settings"></SfButton>
    <SfButton CssClass="e-inherit" Content="Login"></SfButton>
</SfAppBar>
```

## Separator

`AppBarSeparator` displays a vertical line to visually group or separate AppBar contents.

**When to use:**
- Visually grouping related buttons
- Separating different functional areas
- Creating visual hierarchy in the AppBar
- Improving scannability of multiple actions

**How it works:**
- Displays as a vertical line between elements
- Inherits color from the AppBar's color mode
- Provides visual separation without affecting layout spacing

**Example with button groups:**

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfAppBar ColorMode="AppBarColor.Primary">
    <!-- Clipboard group -->
    <SfButton CssClass="e-inherit" IconCss="e-icons e-cut"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-copy"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-paste"></SfButton>
    
    <AppBarSeparator></AppBarSeparator>
    
    <!-- Text formatting group -->
    <SfButton CssClass="e-inherit" IconCss="e-icons e-bold"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-underline"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-italic"></SfButton>
    
    <AppBarSeparator></AppBarSeparator>
    
    <!-- Alignment group -->
    <SfButton CssClass="e-inherit" IconCss="e-icons e-align-left"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-align-right"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-align-center"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-justify"></SfButton>
</SfAppBar>

<style>
    .e-btn.e-inherit {
        margin: 0 3px;
    }
</style>
```

This example creates three distinct button groups separated by visual dividers.

## Media Query

Media queries allow the AppBar to adapt to different screen sizes, creating responsive layouts that work across devices.

**When to use:**
- Building responsive applications
- Adapting navigation for mobile and desktop
- Adjusting layout based on screen width
- Optimizing content display for different devices

**Responsive pattern:**

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfAppBar ColorMode="AppBarColor.Primary">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
    <SfButton CssClass="e-inherit" Content="Home"></SfButton>
    <SfButton CssClass="e-inherit" Content="About"></SfButton>
    <SfButton CssClass="e-inherit" Content="Products"></SfButton>
    <SfButton CssClass="e-inherit" Content="Contacts"></SfButton>
    <AppBarSpacer></AppBarSpacer>
    <AppBarSeparator></AppBarSeparator>
    <SfButton CssClass="e-inherit" Content="Login"></SfButton>
</SfAppBar>

<style>
    .e-btn.e-inherit {
        margin: 0 3px;
    }
    
    /* Tablet and smaller */
    @media screen and (max-width: 1024px) {
        .e-appbar {
            flex-flow: row wrap;
            height: auto;
            gap: 8px;
        }
    }
    
    /* Mobile */
    @media screen and (max-width: 480px) {
        .e-appbar {
            gap: 4px;
        }
        .e-btn.e-inherit {
            margin: 0 2px;
        }
    }
</style>
```

**Key responsive properties:**
- `flex-flow: row wrap` - Allows items to wrap to multiple lines
- `height: auto` - Adjusts height for wrapped content
- `gap` - Adds spacing between wrapped items

## Designing AppBar with Menu

The `SfMenu` component can be integrated into the AppBar for hierarchical navigation. Use the `e-inherit` CSS class to match the AppBar's styling.

**When to use:**
- Hierarchical navigation structures
- Dropdown menus for categories
- Multi-level navigation
- Complex navigation patterns

**Basic menu integration:**

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfAppBar ColorMode="AppBarColor.Primary">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
    
    <SfMenu CssClass="e-inherit" TValue="MenuItem">
        <MenuItems>
            <MenuItem Text="Company">
                <MenuItems>
                    <MenuItem Text="About Us"></MenuItem>
                    <MenuItem Text="Customers"></MenuItem>
                    <MenuItem Text="Blog"></MenuItem>
                    <MenuItem Text="Careers"></MenuItem>
                </MenuItems>
            </MenuItem>
        </MenuItems>
    </SfMenu>
    
    <SfMenu CssClass="e-inherit" TValue="MenuItem">
        <MenuItems>
            <MenuItem Text="Products">
                <MenuItems>
                    <MenuItem Text="Developer"></MenuItem>
                    <MenuItem Text="Analytics"></MenuItem>
                    <MenuItem Text="Reporting"></MenuItem>
                    <MenuItem Text="Help Desk"></MenuItem>
                </MenuItems>
            </MenuItem>
        </MenuItems>
    </SfMenu>
    
    <SfMenu CssClass="e-inherit" TValue="MenuItem">
        <MenuItems>
            <MenuItem Text="About Us"></MenuItem>
        </MenuItems>
    </SfMenu>
    
    <SfMenu CssClass="e-inherit" TValue="MenuItem">
        <MenuItems>
            <MenuItem Text="Carrers"></MenuItem>
        </MenuItems>
    </SfMenu>
    
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" Content="Login"></SfButton>
</SfAppBar>

<style>
    .e-btn.e-inherit {
        margin: 0 3px;
    }
</style>
```

**Important:**
- Always use `CssClass="e-inherit"` on SfMenu to inherit AppBar colors
- Specify `TValue="MenuItem"` for proper type binding
- Each menu can have nested MenuItems for dropdown functionality

## Designing AppBar with Buttons

The AppBar supports both `SfButton` and `SfDropDownButton` components for actions. Use the `e-inherit` CSS class to match the AppBar's color scheme.

**When to use:**
- Action buttons for user interactions
- Dropdown buttons for contextual actions
- Icon buttons for compact layouts
- Text buttons for navigation

**Example with dropdown button:**

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons
@using Syncfusion.Blazor.SplitButtons

<SfAppBar ColorMode="AppBarColor.Primary">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
    
    <SfDropDownButton CssClass="e-inherit" Content="Products">
        <DropDownMenuItems>
            <DropDownMenuItem Text="Developer"></DropDownMenuItem>
            <DropDownMenuItem Text="Analytics"></DropDownMenuItem>
            <DropDownMenuItem Text="Reporting"></DropDownMenuItem>
            <DropDownMenuItem Text="E-Signature"></DropDownMenuItem>
            <DropDownMenuItem Text="Help Desk"></DropDownMenuItem>
        </DropDownMenuItems>
    </SfDropDownButton>
    
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" Content="Login"></SfButton>
</SfAppBar>

<style>
    .e-btn.e-inherit {
        margin: 0 3px;
    }
</style>
```

**Button styling tips:**
- Use `CssClass="e-inherit"` for consistent styling with the AppBar
- Use `IconCss` for icon-only buttons
- Use `Content` for text or combined icon-text buttons
- Add margin with CSS for spacing between buttons

**Icon button pattern:**

```cshtml
<SfAppBar ColorMode="AppBarColor.Primary">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-home"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-search"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-notifications"></SfButton>
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-settings"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-user"></SfButton>
</SfAppBar>
```

## Designing AppBar with SideBar

The AppBar can control the expand/collapse state of a `SfSidebar` component, creating a classic navigation pattern with a toggleable sidebar.

**When to use:**
- Applications with extensive navigation
- Dashboard layouts
- Admin panels
- Applications needing collapsible side navigation

**Complete integration example:**

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons
@using Syncfusion.Blazor.Inputs

<div>
    <SfAppBar>
        <SfButton CssClass="e-inherit" IconCss="e-icons e-menu" OnClick="@Toggle"></SfButton>
        <div class="e-folder">
            <div class="e-folder-name">Navigation Pane</div>
        </div>
    </SfAppBar>
</div>

<SfSidebar HtmlAttributes="@HtmlAttribute" Width="210px" Target=".main-content" MediaQuery="(min-width:600px)" @bind-IsOpen="SidebarToggle">
    <ChildContent>
        <div class="main-menu">
            <div class="table-content">
                <SfTextBox Placeholder="Search..."></SfTextBox>
                <p class="main-menu-header">TABLE OF CONTENTS</p>
            </div>
            <div>
                <SfTreeView CssClass="main-treeview" ExpandOn="@Expand" TValue="TreeData">
                    <TreeViewFieldsSettings Id="NodeId" Text="NodeText" DataSource="Treedata" HasChildren="HasChild" ParentID="Pid">
                    </TreeViewFieldsSettings>
                </SfTreeView>
            </div>
        </div>
    </ChildContent>
</SfSidebar>

<div class="main-content" id="main-text">
    <div class="sidebar-content">
        <div class="sidebar-heading">Responsive Sidebar with Treeview</div>
        <p class="paragraph-content">
            This is a graphical aid for visualizing and categorizing the site, in the style of an expandable and collapsable treeview component.
            It auto-expands to display the node(s), if any, corresponding to the currently viewed title, highlighting that node(s)
            and its ancestors. Load-on-demand when expanding nodes is available where supported (most graphical browsers),
            falling back to a full-page reload. MediaWiki-supported caching, aside from squid, has been considered so that
            unnecessary re-downloads of content are avoided where possible. The complete expanded/collapsed state of
            the treeview persists across page views in most situations.
        </p>
    </div>
</div>

@code {
    public ExpandAction Expand = ExpandAction.Click;
    public bool SidebarToggle = false;
    Dictionary<string, object> HtmlAttribute = new Dictionary<string, object>()
    {
        {"class", "sidebar-treeview" }
    };
    
    public void Toggle(MouseEventArgs args)
    {
        SidebarToggle = !SidebarToggle;
    }
    
    public class TreeData {
        public string NodeId { get; set; }
        public string NodeText { get; set; }
        public bool HasChild { get; set; }
        public string Pid { get; set; }
    }
    
    private List<TreeData> Treedata = new List<TreeData>();
    
    protected override void OnInitialized() {
        base.OnInitialized();
        Treedata.Add(new TreeData { NodeId = "01", NodeText = "Installation" });
        Treedata.Add(new TreeData { NodeId = "02", NodeText = "Deployment" });
        Treedata.Add(new TreeData { NodeId = "03", NodeText = "Quick Start" });
        Treedata.Add(new TreeData { NodeId = "04", NodeText = "Components", HasChild=true });
        Treedata.Add(new TreeData { NodeId = "04-01", NodeText = "Calendar", Pid="04" });
        Treedata.Add(new TreeData { NodeId = "04-02", NodeText = "DatePicker", Pid="04" });
        Treedata.Add(new TreeData { NodeId = "04-03", NodeText = "DateTimePicker", Pid="04" });
        Treedata.Add(new TreeData { NodeId = "04-04", NodeText = "DateRangePicker", Pid="04" });
        Treedata.Add(new TreeData { NodeId = "04-05", NodeText = "TimePicker", Pid="04" });
        Treedata.Add(new TreeData { NodeId = "04-06", NodeText = "SideBar", Pid="04" });
    }
}

<style>
    .e-appbar .e-folder {
        margin: 0 5px;
    }
    
    .sidebar-treeview .e-treeview .e-icon-collapsible,
    .sidebar-treeview .e-treeview .e-icon-expandable {
        float: right;
        margin: 3px;
    }
    
    .sidebar-treeview .e-treeview,
    .sidebar-treeview .e-treeview .e-ul {
        padding: 0;
        margin: 0;
    }
    
    .sidebar-treeview {
        z-index: 20 !important;
        border-right: 1px solid #dee2e6 !important;
    }
    
    .sidebar-treeview .main-menu .main-menu-header {
        color: #656a70;
        padding: 15px 15px 15px 0;
        font-size: 14px;
        width: 13em;
        margin: 0;
    }
    
    #main-text .sidebar-heading {
        font-size: 16px;
    }
    
    #main-text .sidebar-content {
        padding: 15px;
        font-size: 14px;
    }
    
    #main-text .paragraph-content {
        padding: 15px 0;
        font-weight: normal;
        font-size: 14px;
    }
    
    .e-folder {
        text-align: center;
        font-weight: 500;
        font-size: 16px;
    }
    
    .sidebar-treeview .e-treeview .e-text-content {
        padding-left: 18px;
    }
    
    .main-content {
        height: 380px;
    }
    
    .e-folder-name {
        margin-top: -2px;
    }
</style>
```

**Key integration points:**

1. **AppBar button with click handler:**
```cshtml
<SfButton CssClass="e-inherit" IconCss="e-icons e-menu" OnClick="@Toggle"></SfButton>
```

2. **Sidebar with two-way binding:**
```cshtml
<SfSidebar @bind-IsOpen="SidebarToggle">
```

3. **Toggle method in code:**
```csharp
public bool SidebarToggle = false;

public void Toggle(MouseEventArgs args)
{
    SidebarToggle = !SidebarToggle;
}
```

**Pattern benefits:**
- Clean separation of navigation (sidebar) and control (AppBar)
- Responsive behavior with MediaQuery support
- Two-way binding for state management
- TreeView integration for hierarchical navigation

## Best Practices

**Layout Components:**
- Use `AppBarSpacer` to create flexible layouts that adapt to content
- Use `AppBarSeparator` to group related actions visually
- Combine Spacer and Separator for complex layouts

**Responsive Design:**
- Test AppBar at different screen widths
- Use media queries to adjust layout for mobile and tablet
- Consider hiding text labels on small screens and showing only icons
- Use `flex-flow: row wrap` for wrapping on smaller screens

**Component Integration:**
- Always use `CssClass="e-inherit"` on child components (buttons, menus)
- This ensures consistent styling with the AppBar's color mode
- Test integration with different AppBar color modes

**Sidebar Integration:**
- Use two-way binding (`@bind-IsOpen`) for state management
- Set appropriate `Target` for sidebar to control positioning
- Use `MediaQuery` for responsive sidebar behavior
- Consider z-index for proper layering
