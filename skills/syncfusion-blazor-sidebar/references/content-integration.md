# Content Integration with ListView, TreeView, and Toolbar

This guide covers integrating other Syncfusion components within the Sidebar for rich navigation experiences.

## Table of Contents
- [ListView Integration](#listview-integration)
- [ListViewFieldSettings](#listviewfieldsettings)
- [TreeView Integration](#treeview-integration)
- [Toolbar Integration](#toolbar-integration)
- [MainLayout Integration](#mainlayout-integration)

## ListView Integration

Any HTML element can be placed in the Sidebar content area. Sidebar supports all types of HTML structures like `TreeView`, `ListView`, etc.

The Sidebar is commonly rendered with ListView component in its content area for menu-based navigation:

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Lists
@using Syncfusion.Blazor.Buttons

<SfSidebar @ref="sidebarObj" ID="sidebar" Type="@Type" Width="100%" @bind-IsOpen="SidebarToggle">
    <ChildContent>
        <div class="title1"> Menu </div>
        <div class="closebtn">
            <SfButton ID="close" @onclick="@Close" CssClass="e-btn close-btn">
                <ChildContent>
                    <span id="innerclose" class="e-icons close-icon"></span>
                </ChildContent>
            </SfButton>
        </div>
        <div id="listcontainer">
            <!-- Listview element declaration -->
            <SfListView DataSource="@Data" ID="list">
                <ListViewFieldSettings TValue="ListViewData" Id="Id" Text="Text"></ListViewFieldSettings>
            </SfListView>
        </div>
        <div class="sub-title">
            ListView component is placed inside the Sidebar content area.
        </div>
    </ChildContent>
</SfSidebar>

<!-- main content declaration -->
<div>
    <div class="title2">Main content</div>
    <div class="sub-title"> Click the button to open/close the Sidebar.</div>
    <div style="padding:20px" class="center-align">
        <SfButton ID="toggle" @onclick="@Toggle" IsToggle="true" CssClass="e-btn e-info">Toggle Sidebar</SfButton>
    </div>
</div>

@code{
    SfSidebar sidebarObj;
    public SidebarType Type = SidebarType.Over;
    
    List<ListViewData> Data = new List<ListViewData>()
    {
        new ListViewData{  Text = "Home", Id = "list-01"},
        new ListViewData{ Text = "Offers", Id = "list-02"},
        new ListViewData{ Text = "Support", Id = "list-03"},
        new ListViewData{ Text = "Logout", Id = "list-04"}
    };

    class ListViewData
    {
        public string Id { get; set; }
        public string Text { get; set; }
    }
    
    public bool SidebarToggle = false;
    
    public void Close()
    {
        SidebarToggle = false;
    }
    
    public void Toggle()
    {
        SidebarToggle = !SidebarToggle;
    }
}

<style>
    /* Listview element styles */
    #listcontainer {
        width: 100%;
    }

    #list {
        margin: 0 auto;
        width: 30%;
    }

    .e-listview .e-list-item {
        text-align: center;
        font-size: 14px;
        padding: 0;
    }
    
    /* Button element styles */
    .e-btn.close-btn :hover {
        box-shadow: none;
        background: transparent;
    }

    .close-btn, .e-listview .e-list-item, #sidebar {
        background-color: rgb(20, 118, 210);
        color: #ffffff;
    }

    .close-icon::before {
        content: '\e7e7';
    }

    .close-btn {
        box-shadow: none;
        border: none;
    }

    .close-btn:hover {
        color: #fafafa;
    }

    .e-icons.close-icon {
        line-height: 2.2;
    }

    .closebtn {
        top: 15px;
        line-height: 36px;
        height: 42px;
        color: black;
        position: absolute;
        right: 10px;
    }
    
    /* Sample level styles */
    .title1 {
        text-align: center;
        font-size: 20px;
        padding: 15px;
    }

    .title2 {
        text-align: center;
        font-size: 20px;
        padding: 15px;
    }

    .sub-title {
        text-align: center;
        font-size: 16px;
        padding: 10px;
    }

    .center-align {
        text-align: center;
        padding: 20px;
    }
</style>
```

## ListViewFieldSettings

Configure ListView within Sidebar using `ListViewFieldSettings`:

- **TValue:** The data model type (e.g., `ListViewData`)
- **Id:** Property name for unique identifier
- **Text:** Property name for display text
- **DataSource:** Collection of items to display

```csharp
<SfListView DataSource="@Data" ID="list">
    <ListViewFieldSettings TValue="ListViewData" 
        Id="Id" 
        Text="Text">
    </ListViewFieldSettings>
</SfListView>

@code {
    List<ListViewData> Data = new List<ListViewData>()
    {
        new ListViewData { Text = "Home", Id = "list-01" },
        new ListViewData { Text = "About", Id = "list-02" },
        new ListViewData { Text = "Services", Id = "list-03" }
    };

    class ListViewData
    {
        public string Id { get; set; }
        public string Text { get; set; }
    }
}
```

## TreeView Integration

For hierarchical navigation, integrate TreeView component in Sidebar:

```razor
@using Syncfusion.Blazor.Navigations

<SfSidebar @bind-IsOpen="SidebarToggle" Width="290px">
    <ChildContent>
        <div class="sidebar-content">
            <SfTreeView CssClass="main-treeview" TValue="TreeData">
                <TreeViewFieldsSettings Id="NodeId" 
                    NavigateUrl="NavigateUrl" 
                    Text="NodeText" 
                    IconCss="IconCss" 
                    DataSource="TreeData" 
                    HasChildren="HasChild" 
                    ParentID="Pid">
                </TreeViewFieldsSettings>                            
            </SfTreeView>
        </div>
    </ChildContent>
</SfSidebar>

@code {
    public bool SidebarToggle = false;

    public class TreeData
    {
        public string NodeId { get; set; }
        public string NodeText { get; set; }
        public string IconCss { get; set; }
        public bool HasChild { get; set; }
        public string Pid { get; set; }
        public string NavigateUrl { get; set; }
    }
    
    private List<TreeData> TreeDataList = new List<TreeData>()
    {
        new TreeData { NodeId = "01", NodeText = "Home", IconCss = "icon-home" },
        new TreeData { NodeId = "02", NodeText = "About", IconCss = "icon-info" },
        new TreeData { NodeId = "03", NodeText = "Services", IconCss = "icon-services", HasChild = true },
        new TreeData { NodeId = "04", NodeText = "Consulting", Pid = "03", IconCss = "icon-consulting" },
        new TreeData { NodeId = "05", NodeText = "Development", Pid = "03", IconCss = "icon-development" }
    };
}
```

## Toolbar Integration

Combine Sidebar with Toolbar for complete navigation solutions:

```razor
@using Syncfusion.Blazor.Navigations

<!-- Toolbar with menu button -->
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem PrefixIcon="e-tbar-menu-icon" TooltipText="Menu" OnClick="Toggle"></ToolbarItem>
        <ToolbarItem>
            <Template>
                <div class="toolbar-title">Application Name</div>
            </Template>
        </ToolbarItem>
        <ToolbarItem PrefixIcon="e-tbar-settings-icon" TooltipText="Settings" Align="@ItemAlign.Right"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>

<!-- Sidebar with Explicit Target -->
<SfSidebar @bind-IsOpen="SidebarToggle" Width="280px" Target=".main-content">
    <ChildContent>
        <nav class="sidebar-nav">
            <!-- Navigation items -->
        </nav>
    </ChildContent>
</SfSidebar>

<!-- Main content -->
<div class="main-content">                                    <!-- Explicit target -->
    <div>                                              <!-- ⚠️ REQUIRED: Inner wrapper for CSS transforms -->
        <!-- Page content -->
    </div>
</div>

@code {
    public bool SidebarToggle = false;

    public void Toggle()
    {
        SidebarToggle = !SidebarToggle;
    }
}
```

## MainLayout Integration

For .NET 8 Blazor Web Apps, integrate Sidebar in MainLayout.razor using **implicit targeting** (recommended):

```razor
@using Syncfusion.Blazor.Navigations
@inherits LayoutComponentBase

<div class="page">
    <!-- Toolbar -->
    <SfToolbar>
        <ToolbarItems>
            <ToolbarItem PrefixIcon="e-tbar-menu-icon tb-icons" TooltipText="Menu" OnClick="@Toggle"></ToolbarItem>
            <ToolbarItem>
                <Template>
                    <div class="app-title">My Application</div>
                </Template>
            </ToolbarItem>
        </ToolbarItems>
    </SfToolbar>

    <!-- Sidebar Navigation (Implicit Targeting - No Target Property) -->
    <SfSidebar Width="290px" 
        MediaQuery="(min-width:600px)" 
        @bind-IsOpen="SidebarToggle">
        <ChildContent>
            <div class="sidebar-content">
                <SfTreeView CssClass="main-treeview" TValue="TreeData">
                    <TreeViewFieldsSettings Id="NodeId" 
                        Text="NodeText" 
                        IconCss="IconCss" 
                        DataSource="TreeDataSource" 
                        HasChildren="HasChild" 
                        ParentID="Pid">
                    </TreeViewFieldsSettings>                            
                </SfTreeView>
            </div>
        </ChildContent>
    </SfSidebar>

    <!-- Main Content (Automatically targets Sidebar by default) -->
    <main class="e-main-content">
        @Body
    </main>
</div>

@code {
    public bool SidebarToggle = false;

    public void Toggle()
    {
        SidebarToggle = !SidebarToggle;
    }

    public class TreeData
    {
        public string NodeId { get; set; }
        public string NodeText { get; set; }
        public string IconCss { get; set; }
        public bool HasChild { get; set; }
        public string Pid { get; set; }
    }

    private List<TreeData> TreeDataSource = new();

    protected override void OnInitialized()
    {
        base.OnInitialized();
        TreeDataSource = new List<TreeData>()
        {
            new TreeData { NodeId = "01", NodeText = "Dashboard", IconCss = "icon-dashboard" },
            new TreeData { NodeId = "02", NodeText = "Settings", IconCss = "icon-settings" },
            new TreeData { NodeId = "03", NodeText = "Help", IconCss = "icon-help" }
        };
    }
}
```

**Integration Benefits:**
- ListView for simple menu lists
- TreeView for hierarchical navigation structures
- Toolbar for consistent header navigation controls
- MainLayout integration for app-wide persistence
- **Implicit targeting** recommended for simplest MainLayout setup
- **Explicit targeting** only when scoping to specific containers
- Responsive behavior with MediaQuery
- Icon support for visual navigation
