# Advanced Scenarios and Troubleshooting

## Table of Contents
- [Preventing Panel Overlap](#preventing-panel-overlap)
- [All Panels at Same Position](#all-panels-at-same-position)
- [Performance Optimization](#performance-optimization)
- [Complex Layout Patterns](#complex-layout-patterns)
- [SEO Analysis Dashboard](#seo-analysis-dashboard)
- [Troubleshooting Guide](#troubleshooting-guide)

## Preventing Panel Overlap

### The Panel Overlap Problem

Panel overlap occurs **only** when multiple panels are assigned the **same `Id` property**. When you don't explicitly set IDs, the Dashboard Layout component automatically generates unique IDs internally, so there is no overlap issue.

**Overlapping panels occur with duplicate IDs:**
- Panel 1: `Id="panel"` 
- Panel 2: `Id="panel"` ← Same ID causes overlap at Row = 0, Column = 0

```csharp
@using Syncfusion.Blazor.Layouts

<!-- ❌ WRONG - Duplicate IDs will cause overlap -->
<SfDashboardLayout CellSpacing="@(new double[]{20, 20})" Columns="4">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel Id="panel">  <!-- Duplicate ID -->
            <ContentTemplate><div>Panel 1</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Id="panel">  <!-- Same ID causes overlap -->
            <ContentTemplate><div>Panel 2</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>
```

### Solution: Use Unique IDs or Auto-Generated IDs

**Option 1: Assign unique `Id` values explicitly**

```csharp
@using Syncfusion.Blazor.Layouts

<!-- ✅ CORRECT - Each panel has unique ID -->
<SfDashboardLayout CellSpacing="@(new double[]{20, 20})" Columns="4">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel Id="panel-1">
            <ContentTemplate><div>Panel 1</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Id="panel-2" Row="0" Column="1">
            <ContentTemplate><div>Panel 2</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Id="panel-3" Row="0" Column="2">
            <ContentTemplate><div>Panel 3</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>
```

### Dynamic Panel Generation with Unique IDs

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout CellSpacing="@(new double[]{20, 20})" Columns="4">
    <DashboardLayoutPanels>
        @foreach (var panel in PanelItems)
        {
            <DashboardLayoutPanel 
                Id="@panel.Id" 
                Row="@panel.Row" 
                Column="@panel.Column">
                <ContentTemplate>
                    <div class="panel-content">@panel.Content</div>
                </ContentTemplate>
            </DashboardLayoutPanel>
        }
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    public class PanelModel
    {
        public string Id { get; set; }
        public int Row { get; set; } = 0;
        public int Column { get; set; } = 0;
        public string Content { get; set; }
    }

    private List<PanelModel> PanelItems = new List<PanelModel>
    {
        new PanelModel { Id = "panel1", Row = 0, Column = 0, Content = "Panel 1" },
        new PanelModel { Id = "panel2", Row = 0, Column = 1, Content = "Panel 2" },
        new PanelModel { Id = "panel3", Row = 0, Column = 2, Content = "Panel 3" },
        new PanelModel { Id = "panel4", Row = 1, Column = 0, Content = "Panel 4" },
        new PanelModel { Id = "panel5", Row = 1, Column = 1, Content = "Panel 5" },
        new PanelModel { Id = "panel6", Row = 1, Column = 2, Content = "Panel 6" }
    };
}

<style>
    .panel-content {
        text-align: center;
        margin-top: 10px;
        font-size: 18px;
        font-weight: 500;
    }
</style>
```

**Best Practices:**
- Always assign unique IDs to dynamically generated panels
- Use descriptive IDs (e.g., "panel-sales", "panel-metrics")
- Include the ID in your data model
- Never duplicate ID values
- Consider using GUIDs for system-generated panels

## All Panels at Same Position

### Scenario: Dynamic Multi-Panel Display

Sometimes you may want to display all panels starting at the same default position (Row=0, Column=0) and let them automatically arrange:

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{20, 20})" 
    Columns="4"
    AllowFloating="true">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel Id="panel-1">
            <ContentTemplate><div>Panel 1</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Id="panel-2">
            <ContentTemplate><div>Panel 2</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Id="panel-3">
            <ContentTemplate><div>Panel 3</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Id="panel-4">
            <ContentTemplate><div>Panel 4</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .e-panel-content {
        text-align: center;
        padding: 10px;
    }
</style>
```

**With AllowFloating="true":**
- Panels automatically arrange in available space
- No overlap occurs
- Grid flows left-to-right, top-to-bottom
- Optimal space utilization

## Performance Optimization

### Handling Large Panel Counts

For dashboards with many panels, optimize performance:

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    Columns="6"
    AllowFloating="true">
    <DashboardLayoutPanels>
        @foreach (var panel in VisiblePanels)  <!-- Only render visible panels -->
        {
            <DashboardLayoutPanel 
                Id="@panel.Id" 
                Row="@panel.Row" 
                Column="@panel.Column"
                SizeX="@panel.SizeX"
                SizeY="@panel.SizeY">
                <ContentTemplate>
                    <div>@panel.Content</div>
                </ContentTemplate>
            </DashboardLayoutPanel>
        }
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    private List<PanelData> AllPanels = new();
    private List<PanelData> VisiblePanels => AllPanels.Take(20).ToList();  // Limit rendered panels

    public class PanelData
    {
        public string Id { get; set; }
        public int Row { get; set; }
        public int Column { get; set; }
        public int SizeX { get; set; } = 1;
        public int SizeY { get; set; } = 1;
        public string Content { get; set; }
    }
}
```

### Virtual Scrolling Pattern

For extremely large dashboards, implement lazy loading:

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    Columns="6">
    <DashboardLayoutPanels>
        @foreach (var panel in LoadedPanels)
        {
            <DashboardLayoutPanel 
                Id="@panel.Id" 
                Row="@panel.Row" 
                Column="@panel.Column">
                <ContentTemplate>
                    <div>@panel.Content</div>
                </ContentTemplate>
            </DashboardLayoutPanel>
        }
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    private List<PanelData> LoadedPanels = new();
    private int LoadCount = 0;

    protected override async Task OnInitializedAsync()
    {
        // Load initial set of panels
        await LoadMorePanels();
    }

    private async Task LoadMorePanels()
    {
        const int BatchSize = 10;
        for (int i = 0; i < BatchSize; i++)
        {
            LoadedPanels.Add(new PanelData 
            { 
                Id = $"panel-{LoadCount++}",
                Content = $"Panel {LoadCount}"
            });
        }
        await Task.Delay(100);  // Simulate load time
    }

    public class PanelData
    {
        public string Id { get; set; }
        public int Row { get; set; }
        public int Column { get; set; }
        public string Content { get; set; }
    }
}
```

### Performance Tips

**Do:**
- ✅ Limit initial panel count
- ✅ Use OnChange events sparingly
- ✅ Implement lazy loading for large datasets
- ✅ Cache panel data
- ✅ Use trackBy in loops

**Avoid:**
- ❌ Recreating all panels on every render
- ❌ Heavy calculations in ContentTemplate
- ❌ Binding to rapidly changing properties
- ❌ Loading all panels upfront

## Complex Layout Patterns

### Responsive Multi-Section Dashboard

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{15, 15})" 
    Columns="12"
    MediaQuery="max-width:768px"
    AllowDragging="true"
    AllowResizing="true">
    <DashboardLayoutPanels>
        <!-- Header: Full width metrics -->
        <DashboardLayoutPanel SizeX=12 Id="header-metrics">
            <HeaderTemplate><div>Key Performance Indicators</div></HeaderTemplate>
            <ContentTemplate>
                <div style="display:flex; gap:10px;">
                    <div style="flex:1;">Revenue: $1.2M</div>
                    <div style="flex:1;">Profit: $450K</div>
                    <div style="flex:1;">Growth: +25%</div>
                    <div style="flex:1;">Efficiency: 92%</div>
                </div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Main content: 2 columns on desktop, 1 on mobile -->
        <DashboardLayoutPanel SizeX=6 Row=1 Id="main-chart">
            <HeaderTemplate><div>Sales Trend</div></HeaderTemplate>
            <ContentTemplate><div>Chart here</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=6 Row=1 Column=6 Id="revenue-chart">
            <HeaderTemplate><div>Revenue Distribution</div></HeaderTemplate>
            <ContentTemplate><div>Chart here</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Secondary: 3 columns on desktop, full width on mobile -->
        <DashboardLayoutPanel SizeX=4 Row=2 Id="region-data">
            <HeaderTemplate><div>By Region</div></HeaderTemplate>
            <ContentTemplate><div>Regional metrics</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=4 Row=2 Column=4 Id="product-data">
            <HeaderTemplate><div>By Product</div></HeaderTemplate>
            <ContentTemplate><div>Product metrics</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=4 Row=2 Column=8 Id="segment-data">
            <HeaderTemplate><div>By Segment</div></HeaderTemplate>
            <ContentTemplate><div>Segment metrics</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Footer: Full width details -->
        <DashboardLayoutPanel SizeX=12 Row=3 Id="detail-table">
            <HeaderTemplate><div>Detailed Report</div></HeaderTemplate>
            <ContentTemplate><div>Detailed data table</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>
```

## SEO Analysis Dashboard

### Real-World Complex Dashboard Example

This comprehensive example demonstrates how to build a professional SEO (Search Engine Optimization) data analysis dashboard by integrating multiple Syncfusion Blazor components within the Dashboard Layout component. This showcases:

- **Multi-component integration**: Sidebar, AccumulationChart, Chart, and Grid components
- **Advanced templating**: Complex HeaderTemplate and ContentTemplate usage
- **Data visualization**: Multiple chart types (Pie, Doughnut, Column, SplineArea)
- **Responsive design**: Sidebar docking and responsive panel arrangement
- **CSS customization**: Custom styling for cards and visualization
- **Icon integration**: Custom icon implementation from Syncfusion icon library

```cshtml
@using Syncfusion.Blazor.Charts
@using Syncfusion.Blazor.Layouts
@using Syncfusion.Blazor.Navigations

<div class="control-section">
    <div class="col-lg-12 col-sm-12 col-md-12" id="sidebar-section">
        <div id="head">
            <div class="header">
                <div class="menu"><span class="e-icons expand"></span></div>
                <div class="searchContent">
                    <div class="analysis">SEO Analysis Dashboard</div>
                </div>
                <div class="right-content">
                    <div class="information">
                        <span class="e-avatar e-avatar-medium e-avatar-circle image"></span>
                        <div class="text-content">John</div>
                    </div>
                </div>
            </div>
        </div>
        <!-- sidebar element declaration -->
        <SfSidebar ID="dockSidebar" DockSize="60px" EnableDock="true" Type="SidebarType.Over" CloseOnDocumentClick="true" Target="#target">
            <ChildContent>
                <div class="content-area">
                    <div class="dock">
                        <ul>
                            <li class="sidebar-item"><span class="e-icons home"></span></li>
                            <li class="sidebar-item filterHover">
                                <span class="e-icons filter"></span>
                            </li>
                            <li class="sidebar-item">
                                <span class="e-icons analyticsChart"></span>
                            </li>
                            <li class="sidebar-item"><span class="e-icons settings"></span></li>
                            <li class="sidebar-item">
                                <span class="e-icons analytics"></span>
                            </li>
                        </ul>
                    </div>
                </div>
            </ChildContent>
        </SfSidebar>
        <!-- end of sidebar element -->
        <!-- main content declaration -->
        <div id="target">
            <div class="sidebar-content">
                <div class="dashboardParent">
                    <SfDashboardLayout @ref="dashboardObject" CellSpacing="@CellSpacing" Columns="@Columns" CellAspectRatio="@Ratio">
                        <DashboardLayoutPanels>
                            <!-- KPI Cards -->
                            <DashboardLayoutPanel SizeX="2" SizeY="1" Row="0" Column="0">
                                <ContentTemplate>
                                    <div class="card">
                                        <span class="e-icons session"></span>
                                        <div class="card-content text">Session</div>
                                        <div class="card-content number">124,444</div>
                                    </div>
                                </ContentTemplate>
                            </DashboardLayoutPanel>
                            <DashboardLayoutPanel SizeX="2" SizeY="1" Row="0" Column="2">
                                <ContentTemplate>
                                    <div class="card">
                                        <span class="e-icons profile"></span>
                                        <div class="card-content text">Users</div>
                                        <div class="card-content number">64,496</div>
                                    </div>
                                </ContentTemplate>
                            </DashboardLayoutPanel>
                            <DashboardLayoutPanel SizeX="2" SizeY="1" Row="0" Column="4">
                                <ContentTemplate>
                                    <div class="card">
                                        <span class="e-icons views"></span>
                                        <div class="card-content text">Views</div>
                                        <div class="card-content number">442,278</div>
                                    </div>
                                </ContentTemplate>
                            </DashboardLayoutPanel>
                            
                            <!-- Active Visitors Chart -->
                            <DashboardLayoutPanel SizeX="2" SizeY="2" Row="1" Column="0">
                                <HeaderTemplate>
                                    <div>Active Visitors</div>
                                </HeaderTemplate>
                                <ContentTemplate>
                                    <SfAccumulationChart Theme="@Theme" Height="100%" Width="100%" EnableSmartLabels="true" SelectionMode="AccumulationSelectionMode.Point">
                                        <AccumulationChartLegendSettings Visible="true" Position="@Syncfusion.Blazor.Charts.LegendPosition.Bottom"></AccumulationChartLegendSettings>
                                        <AccumulationChartTooltipSettings Enable="true" Header="<b>${point.x}</b>" Format="Composition : <b>${point.y}%</b>"></AccumulationChartTooltipSettings>
                                        <AccumulationChartSeriesCollection>
                                            <AccumulationChartSeries DataSource="@VisitorData" XName="Device" YName="Amount" Radius="100%" InnerRadius="35%" Name="Revenue" Palettes="@(new string[] { "#357cd2", "#00bdae", "#e36593" })">
                                                <AccumulationDataLabelSettings Visible="true" Name="text" Position="AccumulationLabelPosition.Inside">
                                                    <AccumulationChartDataLabelFont FontWeight="600" Color="white" Size="14px"></AccumulationChartDataLabelFont>
                                                </AccumulationDataLabelSettings>
                                            </AccumulationChartSeries>
                                        </AccumulationChartSeriesCollection>
                                    </SfAccumulationChart>
                                </ContentTemplate>
                            </DashboardLayoutPanel>
                            
                            <!-- Visitors by Type Chart -->
                            <DashboardLayoutPanel SizeX="4" SizeY="2" Row="1" Column="2">
                                <HeaderTemplate>
                                    <div>Visitors By Type</div>
                                </HeaderTemplate>
                                <ContentTemplate>
                                    <SfChart Theme="@Theme" Height="100%" Width="100%">
                                        <ChartMargin Top="30"></ChartMargin>
                                        <ChartArea><ChartAreaBorder Width="0"></ChartAreaBorder></ChartArea>
                                        <ChartPrimaryXAxis ValueType="Syncfusion.Blazor.Charts.ValueType.Category" Interval="1">
                                            <ChartAxisMajorGridLines Width="0"></ChartAxisMajorGridLines>
                                        </ChartPrimaryXAxis>
                                        <ChartPrimaryYAxis>
                                            <ChartAxisLineStyle Width="0"></ChartAxisLineStyle>
                                            <ChartAxisMajorTickLines Width="0"></ChartAxisMajorTickLines>
                                        </ChartPrimaryYAxis>
                                        <ChartSeriesCollection>
                                            <ChartSeries DataSource="@DesktopVisitData" Fill="#e36593" XName="Month" YName="Visits" Name="Desktop" Type="ChartSeriesType.Column" Width="2"></ChartSeries>
                                            <ChartSeries DataSource="@MobileVisitData" Fill="#00bdae" XName="Month" YName="Visits" Name="Mobile" Type="ChartSeriesType.Column" Width="2"></ChartSeries>
                                            <ChartSeries DataSource="@TabletVisitData" Fill="#357cd2" XName="Month" YName="Visits" Name="Tablet" Type="ChartSeriesType.Column" Width="2"></ChartSeries>
                                        </ChartSeriesCollection>
                                        <ChartTooltipSettings Enable="true"></ChartTooltipSettings>
                                        <ChartLegendSettings Visible="true"></ChartLegendSettings>
                                    </SfChart>
                                </ContentTemplate>
                            </DashboardLayoutPanel>
                            
                            <!-- Usage Statistics -->
                            <DashboardLayoutPanel SizeX="2" SizeY="2" Row="3" Column="4">
                                <HeaderTemplate>
                                    <div>Usage Statistics</div>
                                </HeaderTemplate>
                                <ContentTemplate>
                                    <SfAccumulationChart Theme="@Theme" Height="100%" Width="100%" EnableSmartLabels="true" SelectionMode="AccumulationSelectionMode.Point">
                                        <AccumulationChartLegendSettings Visible="true" Position="@Syncfusion.Blazor.Charts.LegendPosition.Bottom"></AccumulationChartLegendSettings>
                                        <AccumulationChartSeriesCollection>
                                            <AccumulationChartSeries DataSource="@UsageDataValue" XName="Device" YName="Count" Radius="100%" InnerRadius="0%" Name="Usage" Explode="true" ExplodeIndex="2" ExplodeOffset="10%" Palettes="@(new string[] { "#357cd2", "#00bdae", "#e36593" })">
                                                <AccumulationDataLabelSettings Visible="true" Name="Text" Position="AccumulationLabelPosition.Inside">
                                                    <AccumulationChartDataLabelFont FontWeight="600"></AccumulationChartDataLabelFont>
                                                </AccumulationDataLabelSettings>
                                            </AccumulationChartSeries>
                                        </AccumulationChartSeriesCollection>
                                    </SfAccumulationChart>
                                </ContentTemplate>
                            </DashboardLayoutPanel>
                            
                            <!-- Traffic History Chart -->
                            <DashboardLayoutPanel SizeX="4" SizeY="2" Row="3" Column="0">
                                <HeaderTemplate>
                                    <div>Traffic History</div>
                                </HeaderTemplate>
                                <ContentTemplate>
                                    <SfChart Theme="@Theme" Height="100%" Width="100%">
                                        <ChartArea><ChartAreaBorder Width="0"></ChartAreaBorder></ChartArea>
                                        <ChartPrimaryXAxis ValueType="Syncfusion.Blazor.Charts.ValueType.DateTime" LabelFormat="MMM" IntervalType="IntervalType.Months" EdgeLabelPlacement="EdgeLabelPlacement.Shift">
                                            <ChartAxisMajorGridLines Width="0"></ChartAxisMajorGridLines>
                                        </ChartPrimaryXAxis>
                                        <ChartPrimaryYAxis LabelFormat="{value}" Minimum="0" Maximum="4" Interval="1">
                                            <ChartAxisLineStyle Width="0"></ChartAxisLineStyle>
                                            <ChartAxisMajorTickLines Width="0"></ChartAxisMajorTickLines>
                                        </ChartPrimaryYAxis>
                                        <ChartLegendSettings Visible="true"></ChartLegendSettings>
                                        <ChartSeriesCollection>
                                            <ChartSeries DataSource="@TrafficData" Name="Jan" XName="Period" Opacity="0.5" YName="TrafficRate" Type="ChartSeriesType.SplineArea" Fill="rgb(239, 183, 202)"></ChartSeries>
                                            <ChartSeries DataSource="@TrafficData1" Name="Feb" XName="Period" Opacity="0.5" YName="TrafficRate" Type="ChartSeriesType.SplineArea" Fill="rgb(0, 189, 174)"></ChartSeries>
                                        </ChartSeriesCollection>
                                    </SfChart>
                                </ContentTemplate>
                            </DashboardLayoutPanel>
                        </DashboardLayoutPanels>
                    </SfDashboardLayout>
                </div>
            </div>
        </div>
    </div>
</div>

@code {
    SfDashboardLayout dashboardObject;
    double[] CellSpacing = new double[] { 5, 5 };
    int Columns = 6;
    double Ratio = 100 / 85;
    private Syncfusion.Blazor.Theme Theme { get; set; }

    public List<VisitorsData> VisitorData = new List<VisitorsData>
    {
        new VisitorsData { Amount = 900, Device = "Tablet" },
        new VisitorsData { Amount = 1200, Device = "Desktop" },
        new VisitorsData { Amount = 600, Device = "Mobile" }
    };

    public List<UsageData> UsageDataValue = new List<UsageData>
    {
        new UsageData { Count = 37, Device = "Desktop", Text = "60%" },
        new UsageData { Count = 17, Device = "Mobile", Text = "10%" },
        new UsageData { Count = 19, Device = "Tablet", Text = "20%" }
    };

    public List<VisitsCountData> MobileVisitData = new List<VisitsCountData>
    {
        new VisitsCountData { Visits = 37, Month = "Jan" },
        new VisitsCountData { Visits = 23, Month = "Feb" },
        new VisitsCountData { Visits = 18, Month = "Mar" }
    };

    public List<VisitsCountData> TabletVisitData = new List<VisitsCountData>
    {
        new VisitsCountData { Visits = 38, Month = "Jan" },
        new VisitsCountData { Visits = 17, Month = "Feb" },
        new VisitsCountData { Visits = 26, Month = "Mar" }
    };

    public List<VisitsCountData> DesktopVisitData = new List<VisitsCountData>
    {
        new VisitsCountData { Visits = 46, Month = "Jan" },
        new VisitsCountData { Visits = 27, Month = "Feb" },
        new VisitsCountData { Visits = 26, Month = "Mar" }
    };

    public List<TrafficDataModel> TrafficData = new List<TrafficDataModel>
    {
        new TrafficDataModel { Period = new DateTime(2002, 01, 01), TrafficRate = 2.1 },
        new TrafficDataModel { Period = new DateTime(2003, 01, 01), TrafficRate = 3.5 },
        new TrafficDataModel { Period = new DateTime(2004, 01, 01), TrafficRate = 2.7 },
        new TrafficDataModel { Period = new DateTime(2005, 01, 01), TrafficRate = 1.7 },
        new TrafficDataModel { Period = new DateTime(2006, 01, 01), TrafficRate = 2.2 },
        new TrafficDataModel { Period = new DateTime(2007, 01, 01), TrafficRate = 2.6 },
        new TrafficDataModel { Period = new DateTime(2008, 01, 01), TrafficRate = 2.9 },
        new TrafficDataModel { Period = new DateTime(2009, 01, 01), TrafficRate = 3.7 },
        new TrafficDataModel { Period = new DateTime(2010, 01, 01), TrafficRate = 1.4 },
        new TrafficDataModel { Period = new DateTime(2011, 01, 01), TrafficRate = 3.2 }
    };

    public List<TrafficDataModel> TrafficData1 = new List<TrafficDataModel>
    {
        new TrafficDataModel { Period = new DateTime(2002, 01, 01), TrafficRate = 2 },
        new TrafficDataModel { Period = new DateTime(2003, 01, 01), TrafficRate = 1.7 },
        new TrafficDataModel { Period = new DateTime(2004, 01, 01), TrafficRate = 1.9 },
        new TrafficDataModel { Period = new DateTime(2005, 01, 01), TrafficRate = 2.3 },
        new TrafficDataModel { Period = new DateTime(2006, 01, 01), TrafficRate = 2.3 },
        new TrafficDataModel { Period = new DateTime(2007, 01, 01), TrafficRate = 1.6 },
        new TrafficDataModel { Period = new DateTime(2008, 01, 01), TrafficRate = 1.5 },
        new TrafficDataModel { Period = new DateTime(2009, 01, 01), TrafficRate = 2.7 },
        new TrafficDataModel { Period = new DateTime(2010, 01, 01), TrafficRate = 1.5 },
        new TrafficDataModel { Period = new DateTime(2011, 01, 01), TrafficRate = 2.2 }
    };

    public class TrafficDataModel
    {
        public DateTime Period { get; set; }
        public double TrafficRate { get; set; }
    }

    public class VisitorsData
    {
        public string Device { get; set; }
        public int Amount { get; set; }
    }

    public class VisitsCountData
    {
        public string Month { get; set; }
        public int Visits { get; set; }
    }

    public class UsageData
    {
        public string Device { get; set; }
        public int Count { get; set; }
        public string Text { get; set; }
    }
}
```

### Key Features in This Example

**1. Multi-Component Integration:**
- `SfSidebar` for navigation sidebar with docking
- `SfAccumulationChart` for pie/doughnut charts (Active Visitors, Usage Statistics)
- `SfChart` for column and spline area charts (Visitors by Type, Traffic History)
- `SfDashboardLayout` as the main container

**2. Panel Arrangement:**
- Row 0: Three 1x1 KPI metric cards (Session, Users, Views)
- Row 1: Active Visitors doughnut chart (2x2) + Visitors by Type column chart (4x2)
- Row 3: Revenue pie chart (2x2) + Traffic History spline chart (4x2)

**3. Template Usage:**
```csharp
<HeaderTemplate>
    <div>Panel Title</div>
</HeaderTemplate>
<ContentTemplate>
    <!-- Chart or content goes here -->
</ContentTemplate>
```

**4. CSS Customization:**
- Custom card styling with gradient backgrounds
- Icon sizing and positioning
- Responsive sidebar styling
- Chart container sizing

**5. Data Binding:**
- Multiple data sources (VisitorData, UsageDataValue, MobileVisitData, etc.)
- DateTime series for traffic trends
- Dynamic property binding (XName, YName, DataSource)

### Benefits of This Approach

✅ **Professional UI**: Multiple visualizations in one layout
✅ **Responsive**: Adapts to mobile with sidebar docking
✅ **Flexible**: Easy to add/remove panels or swap chart types
✅ **Performance**: Efficient grid layout with minimal redraws
✅ **Accessible**: Proper heading hierarchy and semantic structure
✅ **Maintainable**: Clean separation of data (C# code block) and template (cshtml)

### Customization Tips

1. **Change Chart Colors**: Modify `Palettes` property
2. **Adjust Panel Sizes**: Change `SizeX` and `SizeY` values
3. **Add More Charts**: Insert additional `DashboardLayoutPanel` components
4. **Update Data**: Modify the data classes and collection initialization
5. **Responsive Breakpoints**: Add `MediaQuery="max-width:768px"` to stack panels on mobile

## Troubleshooting Guide

### Issue: Panels Not Draggable

**Problem:** Dragging panels doesn't work.

**Solutions:**
1. Enable dragging: `AllowDragging="true"`
2. Check if DraggableHandle is too restrictive
3. Ensure JavaScript is enabled
4. Verify component ID is set

### Issue: Resizing Not Working

**Problem:** Resize handles don't appear or resizing fails.

**Solutions:**
1. Enable resizing: `AllowResizing="true"`
2. Check ResizableHandles property
3. Verify min/max size constraints allow resizing
4. Check CSS doesn't hide resize icon

### Issue: Floating Not Working

**Problem:** AllowFloating="true" but panels don't float.

**Solutions:**
1. Verify EnablePersistence is configured correctly
2. Check that panels have unique IDs
3. Ensure no conflicting CSS positioning
4. Test in clean browser session

### Issue: Performance Degradation

**Problem:** Dashboard becomes slow with many panels.

**Solutions:**
1. Reduce panel count initially
2. Implement lazy loading
3. Disable unnecessary features (dragging, resizing)
4. Optimize ContentTemplate complexity
5. Use OnChange events sparingly

### Issue: Layout Not Responsive

**Problem:** MediaQuery doesn't trigger stacking.

**Solutions:**
1. Set MediaQuery property: `MediaQuery="max-width:768px"`
2. Test with actual device or DevTools
3. Check CSS media query syntax
4. Verify browser window size is actually changing
5. Clear cache and reload

### Issue: Saved State Not Restoring

**Problem:** Layout doesn't restore after page reload.

**Solutions:**
1. Ensure ID is set: `ID="dashboard"`
2. Enable persistence: `EnablePersistence="true"`
3. Check localStorage is available (not incognito)
4. Verify no JavaScript errors in console
5. Try ResetPersistDataAsync and re-save

### General Debugging Tips

**Use Browser DevTools:**
```javascript
// Check localStorage
localStorage.getItem('dashboard')

// Clear saved data
localStorage.removeItem('dashboard')

// Check for errors
console.log(error)
```

**Step-by-Step Debugging:**
1. Verify all IDs are unique
2. Check each property individually
3. Test with minimal example first
4. Gradually add complexity
5. Check browser console for errors
6. Verify latest Syncfusion package version

**Common Configuration Errors:**
- ❌ Missing ID attribute for persistence
- ❌ Duplicate panel IDs
- ❌ Invalid Column configuration
- ❌ CSS conflicts with drag/resize
- ❌ EnablePersistence without ID
- ❌ Conflicting negative sizes
