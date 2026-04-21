# Panel Templates and Headers

## Table of Contents
- [Template Overview](#template-overview)
- [HeaderTemplate Usage](#headertemplate-usage)
- [ContentTemplate Usage](#contenttemplate-usage)
- [Advanced Template Patterns](#advanced-template-patterns)
- [Styling Templates](#styling-templates)

## Template Overview

Blazor Dashboard Layout provides two primary templates for rendering panel content:

| Template | Purpose | Position | Usage |
|----------|---------|----------|-------|
| **HeaderTemplate** | Panel title/header display | Top of panel | Optional header/title |
| **ContentTemplate** | Main panel content | Center of panel | Primary content area |
| **CssClass** | Custom styling | Applied to panel | Additional styling |

## HeaderTemplate Usage

### Basic Header Display

The `HeaderTemplate` renders at the top of each panel, typically used for titles or descriptive text:

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout CellSpacing="@(new double[]{10, 10})" Columns="5">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <HeaderTemplate><div>Panel 0</div></HeaderTemplate>
            <ContentTemplate><div>Panel Content</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeX=2 SizeY=2 Column=1>
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
            <ContentTemplate><div>Panel Content</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeY=2 Column=3>
            <HeaderTemplate><div>Panel 2</div></HeaderTemplate>
            <ContentTemplate><div>Panel Content</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Row=1>
            <HeaderTemplate><div>Panel 3</div></HeaderTemplate>
            <ContentTemplate><div>Panel Content</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .e-panel-header {
        background-color: rgba(0, 0, 0, .1);
        text-align: center;
    }
    .e-panel-content {
        text-align: center;
        margin-top: 10px;
    }
</style>
```

### Rich Header Content

Headers can contain complex HTML structures beyond simple text:

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout CellSpacing="@(new double[]{10, 10})" Columns="4">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <HeaderTemplate>
                <div class="header-content">
                    <span class="header-icon">📊</span>
                    <span class="header-title">Sales Dashboard</span>
                    <span class="header-time">Updated 5m ago</span>
                </div>
            </HeaderTemplate>
            <ContentTemplate><div>Dashboard content</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel Column=1>
            <HeaderTemplate>
                <div class="header-content">
                    <span class="header-icon">📈</span>
                    <span class="header-title">Revenue Trend</span>
                    <span class="header-badge">+12%</span>
                </div>
            </HeaderTemplate>
            <ContentTemplate><div>Chart content</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel Column=2>
            <HeaderTemplate>
                <div class="header-content">
                    <span class="header-icon">👥</span>
                    <span class="header-title">Users</span>
                    <span class="header-value">5,234</span>
                </div>
            </HeaderTemplate>
            <ContentTemplate><div>User metrics</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel Column=3>
            <HeaderTemplate>
                <div class="header-content">
                    <span class="header-icon">⚙️</span>
                    <span class="header-title">Settings</span>
                    <button class="header-btn">Configure</button>
                </div>
            </HeaderTemplate>
            <ContentTemplate><div>Configuration options</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .header-content {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 10px;
        background-color: #334099;
        color: white;
    }
    .header-icon {
        font-size: 20px;
        margin-right: 10px;
    }
    .header-title {
        flex: 1;
        font-weight: bold;
    }
    .header-time, .header-badge, .header-value {
        font-size: 12px;
        opacity: 0.8;
    }
    .header-btn {
        background: white;
        color: #334099;
        border: none;
        padding: 4px 8px;
        cursor: pointer;
        border-radius: 3px;
    }
    .e-panel-content {
        padding: 15px;
    }
</style>
```

## ContentTemplate Usage

### Basic Content Display

The `ContentTemplate` holds the main panel content below the header:

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout CellSpacing="@(new double[]{10, 10})" Columns="5">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <HeaderTemplate><div>Panel 0</div></HeaderTemplate>
            <ContentTemplate>
                <div>Simple text content in panel</div>
            </ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Column=1>
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
            <ContentTemplate>
                <div>
                    <p>HTML formatted content</p>
                    <ul>
                        <li>Item 1</li>
                        <li>Item 2</li>
                    </ul>
                </div>
            </ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Column=2>
            <HeaderTemplate><div>Panel 2</div></HeaderTemplate>
            <ContentTemplate>
                <div class="metric-box">
                    <div class="metric-value">$125,320</div>
                    <div class="metric-label">Total Revenue</div>
                </div>
            </ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .metric-box {
        text-align: center;
        padding: 20px;
    }
    .metric-value {
        font-size: 28px;
        font-weight: bold;
        color: #334099;
    }
    .metric-label {
        color: #666;
        margin-top: 5px;
    }
</style>
```

### Hosting Syncfusion Components

Content templates can host complex Syncfusion Blazor components:

```csharp
@using Syncfusion.Blazor.Layouts
@using Syncfusion.Blazor.Charts

<SfDashboardLayout CellSpacing="@(new double[]{10, 10})" Columns="5">
    <DashboardLayoutPanels>
        <!-- Chart Component -->
        <DashboardLayoutPanel SizeX=2 SizeY=2>
            <HeaderTemplate><div>Sales Chart</div></HeaderTemplate>
            <ContentTemplate>
                <SfChart Title="Monthly Sales">
                    <!-- Chart configuration -->
                </SfChart>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Grid Component -->
        <DashboardLayoutPanel SizeX=3 SizeY=2 Column=2>
            <HeaderTemplate><div>Sales Data</div></HeaderTemplate>
            <ContentTemplate>
                <SfGrid DataSource="@GridData">
                    <!-- Grid columns -->
                </SfGrid>
            </ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    // Grid data definition
}
```

## Advanced Template Patterns

### Dynamic Template Content

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout CellSpacing="@(new double[]{10, 10})" Columns="4">
    <DashboardLayoutPanels>
        @foreach (var panel in PanelData)
        {
            <DashboardLayoutPanel Id="@panel.Id" Row="@panel.Row" Column="@panel.Column">
                <HeaderTemplate>
                    <div>@panel.Title</div>
                </HeaderTemplate>
                <ContentTemplate>
                    <div>@panel.Content</div>
                </ContentTemplate>
            </DashboardLayoutPanel>
        }
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    public List<PanelDataItem> PanelData = new List<PanelDataItem>
    {
        new PanelDataItem { Id = "panel1", Title = "Panel 1", Content = "Content 1", Row = 0, Column = 0 },
        new PanelDataItem { Id = "panel2", Title = "Panel 2", Content = "Content 2", Row = 0, Column = 1 },
        new PanelDataItem { Id = "panel3", Title = "Panel 3", Content = "Content 3", Row = 0, Column = 2 }
    };

    public class PanelDataItem
    {
        public string Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        public int Row { get; set; }
        public int Column { get; set; }
    }
}
```

### State-Dependent Content

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout CellSpacing="@(new double[]{10, 10})" Columns="4">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <HeaderTemplate><div>Status Dashboard</div></HeaderTemplate>
            <ContentTemplate>
                @if (IsLoading)
                {
                    <div>Loading...</div>
                }
                else if (HasError)
                {
                    <div class="error">Error loading data</div>
                }
                else
                {
                    <div>
                        <div class="status-item">
                            <span>Status: </span>
                            <span class="status-badge @StatusClass">@StatusText</span>
                        </div>
                        <div class="last-updated">Updated: @LastUpdateTime.ToString("g")</div>
                    </div>
                }
            </ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    private bool IsLoading = false;
    private bool HasError = false;
    private string StatusText = "Online";
    private DateTime LastUpdateTime = DateTime.Now;
    private string StatusClass => StatusText == "Online" ? "online" : "offline";
}

<style>
    .status-badge {
        padding: 4px 8px;
        border-radius: 3px;
        font-weight: bold;
    }
    .status-badge.online {
        background-color: #4caf50;
        color: white;
    }
    .status-badge.offline {
        background-color: #f44336;
        color: white;
    }
    .last-updated {
        font-size: 12px;
        color: #666;
        margin-top: 8px;
    }
</style>
```

## Styling Templates

### CSS Class Property

Use `CssClass` to apply custom styling to individual panels:

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout CellSpacing="@(new double[]{10, 10})" Columns="4">
    <DashboardLayoutPanels>
        <!-- Warning panel -->
        <DashboardLayoutPanel CssClass="panel-warning">
            <HeaderTemplate><div>⚠️ Warning</div></HeaderTemplate>
            <ContentTemplate><div>System maintenance in 2 hours</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Success panel -->
        <DashboardLayoutPanel CssClass="panel-success" Column=1>
            <HeaderTemplate><div>✓ Success</div></HeaderTemplate>
            <ContentTemplate><div>All systems operational</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Info panel -->
        <DashboardLayoutPanel CssClass="panel-info" Column=2>
            <HeaderTemplate><div>ℹ️ Information</div></HeaderTemplate>
            <ContentTemplate><div>New version available</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Error panel -->
        <DashboardLayoutPanel CssClass="panel-error" Column=3>
            <HeaderTemplate><div>✕ Error</div></HeaderTemplate>
            <ContentTemplate><div>Connection timeout occurred</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    /* Warning styling -->
    .panel-warning .e-panel-header {
        background-color: #ff9800;
        color: white;
    }
    .panel-warning .e-panel-content {
        background-color: #fff3e0;
        border-left: 4px solid #ff9800;
    }

    /* Success styling -->
    .panel-success .e-panel-header {
        background-color: #4caf50;
        color: white;
    }
    .panel-success .e-panel-content {
        background-color: #f1f8e9;
        border-left: 4px solid #4caf50;
    }

    /* Info styling -->
    .panel-info .e-panel-header {
        background-color: #2196f3;
        color: white;
    }
    .panel-info .e-panel-content {
        background-color: #e3f2fd;
        border-left: 4px solid #2196f3;
    }

    /* Error styling -->
    .panel-error .e-panel-header {
        background-color: #f44336;
        color: white;
    }
    .panel-error .e-panel-content {
        background-color: #ffebee;
        border-left: 4px solid #f44336;
    }

    /* Common panel styling -->
    .e-panel-header {
        padding: 12px;
        font-weight: bold;
    }
    .e-panel-content {
        padding: 15px;
    }
</style>
```

### Complete Themed Example

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout CellSpacing="@(new double[]{15, 15})" Columns="4">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel CssClass="card-primary">
            <HeaderTemplate>
                <div class="card-header">
                    <span class="card-title">Primary Card</span>
                </div>
            </HeaderTemplate>
            <ContentTemplate>
                <div class="card-body">
                    Primary content goes here
                </div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel CssClass="card-secondary" Column=1>
            <HeaderTemplate>
                <div class="card-header">
                    <span class="card-title">Secondary Card</span>
                </div>
            </HeaderTemplate>
            <ContentTemplate>
                <div class="card-body">
                    Secondary content goes here
                </div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel CssClass="card-accent" Column=2>
            <HeaderTemplate>
                <div class="card-header">
                    <span class="card-title">Accent Card</span>
                </div>
            </HeaderTemplate>
            <ContentTemplate>
                <div class="card-body">
                    Accent content goes here
                </div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel CssClass="card-dark" Column=3>
            <HeaderTemplate>
                <div class="card-header">
                    <span class="card-title">Dark Card</span>
                </div>
            </HeaderTemplate>
            <ContentTemplate>
                <div class="card-body">
                    Dark content goes here
                </div>
            </ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    /* Primary card -->
    .card-primary .e-panel-header {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
    }
    .card-primary .e-panel-content {
        background-color: #f0f4ff;
    }

    /* Secondary card -->
    .card-secondary .e-panel-header {
        background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
        color: white;
    }
    .card-secondary .e-panel-content {
        background-color: #fff0f3;
    }

    /* Accent card -->
    .card-accent .e-panel-header {
        background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
        color: white;
    }
    .card-accent .e-panel-content {
        background-color: #e8f8ff;
    }

    /* Dark card -->
    .card-dark .e-panel-header {
        background: linear-gradient(135deg, #2d3436 0%, #000000 100%);
        color: white;
    }
    .card-dark .e-panel-content {
        background-color: #f5f6fa;
    }

    /* Common card styling */
    .e-panel-header {
        padding: 12px 15px;
        font-weight: bold;
    }
    .card-title {
        display: block;
    }
    .e-panel-content {
        padding: 20px;
    }
    .card-body {
        min-height: 100px;
        display: flex;
        align-items: center;
        justify-content: center;
    }
</style>
```
