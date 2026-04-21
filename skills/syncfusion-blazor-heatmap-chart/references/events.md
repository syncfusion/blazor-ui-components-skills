# Events in Blazor HeatMap Chart Component

## Table of Contents
- [Overview](#overview)
- [CellClicked Event](#cellclicked-event)
- [CellRendering Event](#cellrendering-event)
- [CellSelected Event](#cellselected-event)
- [Created Event](#created-event)
- [OnLoad Event](#onload-event)
- [Loaded Event](#loaded-event)
- [Resized Event](#resized-event)
- [TooltipRendering Event](#tooltiprendering-event)
- [CellDoubleClicked Event](#celldoubleclicked-event)
- [LegendRendering Event](#legendrendering-event)

## Overview

This section describes the events that will be triggered for appropriate actions in HeatMap. The events should be declared in the HeatMap component using the [HeatMapEvents](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.HeatMapEvents.html) class. Events provide powerful mechanisms to respond to user interactions and lifecycle changes in the HeatMap component.

## CellClicked Event

When you click on a HeatMap cell, the [CellClicked](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.HeatMapEvents.html#Syncfusion_Blazor_HeatMap_HeatMapEvents_CellClicked) event is triggered. The [CellClickEventArgs](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.CellClickEventArgs.html) contains properties such as **HasRightClicked**, **X**, **Y**, **XLabel**, **YLabel**, and **Value** to access cell information. 

When you right-click on a HeatMap cell, the `CellClicked` event will be triggered, and the [HasRightClicked](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.CellClickEventArgs.html#Syncfusion_Blazor_HeatMap_CellClickEventArgs_HasRightClicked) property in the event argument will be set to **true**.

### Example

The following example demonstrates how to use the `CellClicked` event. In this example, content will be displayed when you click on a HeatMap cell. Additionally, a dialog box showing the cell value, x-axis label, and y-axis label of the current cell will appear only when you right-click on the HeatMap cell.

```cshtml
@using Syncfusion.Blazor.HeatMap
@using Syncfusion.Blazor.Popups

@if (IsCellClicked)
{
    <div>@CellClicked</div>
}

<SfHeatMap DataSource="@dataSource">
    <HeatMapEvents CellClicked="CellClick"></HeatMapEvents>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)">
    </HeatMapTitleSettings>
    <HeatMapCellSettings ShowLabel="true" TileType="CellType.Rect">
        <HeatMapCellBorder Width="1" Radius="4" Color="White"></HeatMapCellBorder>
    </HeatMapCellSettings>
    <HeatMapLegendSettings ShowLabel="true"></HeatMapLegendSettings>
</SfHeatMap>

@if (this.ShowButton)
{
    <SfDialog ResizeHandles="@DialogResizeDirections" AllowDragging="true" Height="200px" Width="300px" EnableResize="true" ShowCloseIcon="true" @bind-Visible="Visibility">
        <DialogPositionData X="@Xvalue" Y="@Yvalue"></DialogPositionData>
        <DialogEvents Closed="@DialogClose"></DialogEvents>
        <DialogTemplates>
            <Header>HeatMap Cell Values</Header>
            <Content>
                <table class="styled-table">
                    <thead>
                        <tr>
                            <th>X-axis Label</th>
                            <th>Y-axis Label</th>
                            <th>Current Cell Value</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td>@xLabel</td>
                            <td>@yLabel</td>
                            <td>@cellValue</td>
                        </tr>
                    </tbody>
                </table>
            </Content>
        </DialogTemplates>
    </SfDialog>
}

<style>
    .styled-table {
        width: 100%;
        border-collapse: collapse;
    }

        .styled-table th, .styled-table td {
            border: 1px solid black;
            padding: 10px;
            text-align: left;
        }

        .styled-table thead {
            background-color: #f2f2f2;
        }
</style>

@code {
    public bool IsCellClicked = false;
    public string CellClicked;
    public bool ShowButton { get; set; } = false;
    public bool Visibility { get; set; } = false;
    public ResizeDirection[] DialogResizeDirections { get; set; } = new ResizeDirection[] { ResizeDirection.All };
    string[] XAxisLabels = new string[] { "Nancy", "Andrew", "Janet", "Margaret", "Steven", "Michael" };
    string[] YAxisLabels = new string[] { "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };
    public string xLabel { get; set; }
    public string yLabel { get; set; }
    public double cellValue { get; set; }
    public string Xvalue { get; set; }
    public string Yvalue { get; set; }

    int[,] dataSource = new int[,]
    {
        {73, 39, 26, 39, 94, 0},
        {93, 58, 53, 38, 26, 68},
        {99, 28, 22, 4, 66, 90},
        {14, 26, 97, 69, 69, 3},
        {7, 46, 47, 47, 88, 6},
        {41, 55, 73, 23, 3, 79},
    };

    public void CellClick(Syncfusion.Blazor.HeatMap.CellClickEventArgs args)
    {
        if (args.HasRightClicked)
        {
            Xvalue = args.X;
            Yvalue = args.Y;
            ShowButton = true;
            Visibility = true;
            xLabel = args.XLabel;
            yLabel = args.YLabel;
            cellValue = args.Value;
        } else {
            IsCellClicked = true;
            CellClicked = "The cell clicked event has been triggered!!";
        }
    }

    private void DialogClose(Object args)
    {
        ShowButton = false;
    }
}
```

## CellRendering Event

The [CellRendering](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.HeatMapEvents.html#Syncfusion_Blazor_HeatMap_HeatMapEvents_CellRendering) event will be triggered before each HeatMap cell is rendered. The [HeatMapCellRenderEventArgs](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.HeatMapCellRenderEventArgs.html) provides properties such as **CellValue**, **CellColor**, ***FontSize**, **Height**, **Width**, **XLabel**, **YLabel** and **BorderColor** to customize the cell appearance.

This event allows you to modify properties like **CellValue** to change displayed text, **CellColor** to alter the cell background color, and **BorderColor** to customize cell borders before they are displayed.

### Example

The following example demonstrates how to use the `CellRendering` event to customize the cell value, color, and border color of the cell.

```cshtml
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@dataSource">
    <HeatMapEvents CellRendering="@CellRender"/>
    <HeatMapTitleSettings Text="GDP Growth Rate for Major Economies (in Percentage)"/>
    <HeatMapXAxis Labels="@xAxisLabels"/>
    <HeatMapYAxis Labels="@yAxisLabels"/>
</SfHeatMap>

@code{
    private void CellRender(HeatMapCellRenderEventArgs args)
    {
        if (args.CellValue == "2.2")
        {
            args.CellValue = "UPFRONT TEXT";
            args.CellColor = "#c7afcf";
            args.BorderColor = "Red";
        }
    }
    public double[,] dataSource = new double[2, 2]
    {
            {9.5, 2.2 },
            {4.3, 8.9 }
    };
    public string[] xAxisLabels = new string[] { "China", "India" };
    public string[] yAxisLabels = new string[] { "2008", "2009" };
}
```

## CellSelected Event

When single or multiple cells in the HeatMap are selected, the [CellSelected](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.HeatMapEvents.html#Syncfusion_Blazor_HeatMap_HeatMapEvents_CellSelected) event is triggered. The [SelectedEventArgs](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.SelectedEventArgs.html) provides a **Data** property containing a collection of selected cells and **Cancel** property to cancel the event.

This event is useful for implementing custom logic when users select one or more cells in the HeatMap by accessing the **Data** property to retrieve information about all selected cells.

### Example

The following example demonstrates how to use the `CellSelected` event to obtain the count and details of the selected cells.

```cshtml
@using Syncfusion.Blazor.HeatMap

@if (IsVisible)
{
    <div> Total Selected Cells : <b> @SelectedCellCount </b> </div>
}
<SfHeatMap DataSource="@dataSource" AllowSelection="true">
    <HeatMapEvents CellSelected="@CellSelected" />
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)" />
    <HeatMapXAxis Labels="@xAxisLabels" />
    <HeatMapYAxis Labels="@yAxisLabels" />
</SfHeatMap>

@code {
    public bool IsVisible = false;
    public int SelectedCellCount { get; set; }
    private void CellSelected(Syncfusion.Blazor.HeatMap.SelectedEventArgs args)
    {
        IsVisible = true;
        SelectedCellCount = args.Data.Count;
    }
    public double[,] dataSource = new double[,]
    {
            { 73, 39, 26, 39, 94, 0 },
            { 93, 58, 53, 38, 26, 68 },
            { 99, 28, 22, 4, 66, 90 },
            { 14, 26, 97, 69, 69, 3 },
            { 7, 46, 47, 47, 88, 6 },
            { 41, 55, 73, 23, 3, 79 },
            { 56, 69, 21, 86, 3, 33 },
            { 45, 7, 53, 81, 95, 79 },
            { 60, 77, 74, 68, 88, 51 },
            { 25, 25, 10, 12, 78, 14 },
            { 25, 56, 55, 58, 12, 82 },
            { 74, 33, 88, 23, 86, 59 }
    };
    public string[] xAxisLabels = new string[] { "Nancy", "Andrew", "Janet", "Margaret", "Steven",
                 "Michael", "Robert", "Laura", "Anne", "Paul", "Karin", "Mario" };
    public string[] yAxisLabels = new string[] { "Mon", "Tues", "Wed", "Thurs", "Fri", "Sat" };
}
```

## Created Event

The [Created](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.HeatMapEvents.html#Syncfusion_Blazor_HeatMap_HeatMapEvents_Created) event is triggered during the initial rendering process, that is, immediately after the HeatMap component is initialized. This event will be executed only once without event arguments.

This event is useful for performing initialization tasks or setting up configurations that need to occur immediately after the component is created.

### Example

The following example demonstrates how to use the `Created` event.

```cshtml
@using Syncfusion.Blazor.HeatMap

@if (IsVisible)
{
    <div> <b>@EventText</b> </div>
}
<SfHeatMap DataSource="@dataSource">
    <HeatMapEvents Created="@Created" />
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)" />
    <HeatMapXAxis Labels="@xAxisLabels" />
    <HeatMapYAxis Labels="@yAxisLabels" />
</SfHeatMap>

@code {
    public bool IsVisible = false;
    public string EventText { get; set; }
    private void Created()
    {
        IsVisible = true;
        EventText = "The created event has been triggered!!!";
    }
    public double[,] dataSource = new double[,]
    {
            { 73, 39, 26, 39, 94, 0 },
            { 93, 58, 53, 38, 26, 68 },
            { 99, 28, 22, 4, 66, 90 },
            { 14, 26, 97, 69, 69, 3 },
            { 7, 46, 47, 47, 88, 6 },
            { 41, 55, 73, 23, 3, 79 },
            { 56, 69, 21, 86, 3, 33 },
            { 45, 7, 53, 81, 95, 79 },
            { 60, 77, 74, 68, 88, 51 },
            { 25, 25, 10, 12, 78, 14 },
            { 25, 56, 55, 58, 12, 82 },
            { 74, 33, 88, 23, 86, 59 }
    };
    public string[] xAxisLabels = new string[] { "Nancy", "Andrew", "Janet", "Margaret", "Steven",
                 "Michael", "Robert", "Laura", "Anne", "Paul", "Karin", "Mario" };
    public string[] yAxisLabels = new string[] { "Mon", "Tues", "Wed", "Thurs", "Fri", "Sat" };
}
```

## OnLoad Event

The [OnLoad](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.HeatMapEvents.html#Syncfusion_Blazor_HeatMap_HeatMapEvents_OnLoad) event is triggered before the HeatMap is rendered. The [LoadedEventArgs](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.LoadedEventArgs.html) is passed to this event.

This event provides an opportunity to modify HeatMap properties and configuration before the component is rendered to the user.

### Example

The following example demonstrates how to use the `OnLoad` event.

```cshtml
@using Syncfusion.Blazor.HeatMap

@if (IsVisible)
{
    <div> <b>@EventText</b> </div>
}
<SfHeatMap DataSource="@dataSource">
    <HeatMapEvents OnLoad="@Load" />
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)" />
    <HeatMapXAxis Labels="@xAxisLabels" />
    <HeatMapYAxis Labels="@yAxisLabels" />
</SfHeatMap>

@code {
    public bool IsVisible = false;
    public string EventText { get; set; }
    private void Load(Syncfusion.Blazor.HeatMap.LoadedEventArgs args)
    {
        IsVisible = true;
        EventText = "The load event has been triggered!!!";
    }
    public double[,] dataSource = new double[,]
    {
            { 73, 39, 26, 39, 94, 0 },
            { 93, 58, 53, 38, 26, 68 },
            { 99, 28, 22, 4, 66, 90 },
            { 14, 26, 97, 69, 69, 3 },
            { 7, 46, 47, 47, 88, 6 },
            { 41, 55, 73, 23, 3, 79 },
            { 56, 69, 21, 86, 3, 33 },
            { 45, 7, 53, 81, 95, 79 },
            { 60, 77, 74, 68, 88, 51 },
            { 25, 25, 10, 12, 78, 14 },
            { 25, 56, 55, 58, 12, 82 },
            { 74, 33, 88, 23, 86, 59 }
    };
    public string[] xAxisLabels = new string[] { "Nancy", "Andrew", "Janet", "Margaret", "Steven",
                 "Michael", "Robert", "Laura", "Anne", "Paul", "Karin", "Mario" };
    public string[] yAxisLabels = new string[] { "Mon", "Tues", "Wed", "Thurs", "Fri", "Sat" };
}
```

## Loaded Event

The [Loaded](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.HeatMapEvents.html#Syncfusion_Blazor_HeatMap_HeatMapEvents_Loaded) event is triggered when the HeatMap component is re-rendered during a browser window resize. The [LoadedEventArgs](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.LoadedEventArgs.html) is passed to this event.

This event is useful for responding to the completion of the rendering process after the component has been fully loaded or reloaded on the page.

### Example

The following example demonstrates how to use the `Loaded` event.

```cshtml
@using Syncfusion.Blazor.HeatMap

@if (IsVisible)
{
    <div> <b>@EventText</b> </div>
}
<SfHeatMap DataSource="@dataSource">
    <HeatMapEvents Loaded="@Loaded" />
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)" />
    <HeatMapXAxis Labels="@xAxisLabels" />
    <HeatMapYAxis Labels="@yAxisLabels" />
</SfHeatMap>

@code {
    public bool IsVisible = false;
    public string EventText { get; set; }
    private void Loaded(Syncfusion.Blazor.HeatMap.LoadedEventArgs args)
    {
        IsVisible = true;
        EventText = "The loaded event has been triggered!!!";
    }
    public double[,] dataSource = new double[,]
    {
            { 73, 39, 26, 39, 94, 0 },
            { 93, 58, 53, 38, 26, 68 },
            { 99, 28, 22, 4, 66, 90 },
            { 14, 26, 97, 69, 69, 3 },
            { 7, 46, 47, 47, 88, 6 },
            { 41, 55, 73, 23, 3, 79 },
            { 56, 69, 21, 86, 3, 33 },
            { 45, 7, 53, 81, 95, 79 },
            { 60, 77, 74, 68, 88, 51 },
            { 25, 25, 10, 12, 78, 14 },
            { 25, 56, 55, 58, 12, 82 },
            { 74, 33, 88, 23, 86, 59 }
    };
    public string[] xAxisLabels = new string[] { "Nancy", "Andrew", "Janet", "Margaret", "Steven",
                 "Michael", "Robert", "Laura", "Anne", "Paul", "Karin", "Mario" };
    public string[] yAxisLabels = new string[] { "Mon", "Tues", "Wed", "Thurs", "Fri", "Sat" };
}
```

## Resized Event

When the browser window is resized, the [Resized](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.HeatMapEvents.html#Syncfusion_Blazor_HeatMap_HeatMapEvents_Resized) event is triggered. The [ResizeEventArgs](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.ResizeEventArgs.html) is passed to this event.

This event allows you to respond to changes in the viewport size by adjusting the HeatMap display or performing custom resize logic using the `ResizeEventArgs` properties `CurrentSize` and `PreviousSize`.

### Example

The following example demonstrates how to use the `Resized` event.

```cshtml
@using Syncfusion.Blazor.HeatMap

@if (IsVisible)
{
    <div> <b>@EventText</b> </div>
}
<SfHeatMap DataSource="@dataSource">
    <HeatMapEvents Resized="@Resized" />
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)" />
    <HeatMapXAxis Labels="@xAxisLabels" />
    <HeatMapYAxis Labels="@yAxisLabels" />
</SfHeatMap>

@code {
    public bool IsVisible = false;
    public string EventText { get; set; }
    private void Resized(Syncfusion.Blazor.HeatMap.ResizeEventArgs args)
    {
        IsVisible = true;
        EventText = "The resized event has been triggered!!!";
    }
    public double[,] dataSource = new double[,]
    {
            { 73, 39, 26, 39, 94, 0 },
            { 93, 58, 53, 38, 26, 68 },
            { 99, 28, 22, 4, 66, 90 },
            { 14, 26, 97, 69, 69, 3 },
            { 7, 46, 47, 47, 88, 6 },
            { 41, 55, 73, 23, 3, 79 },
            { 56, 69, 21, 86, 3, 33 },
            { 45, 7, 53, 81, 95, 79 },
            { 60, 77, 74, 68, 88, 51 },
            { 25, 25, 10, 12, 78, 14 },
            { 25, 56, 55, 58, 12, 82 },
            { 74, 33, 88, 23, 86, 59 }
    };
    public string[] xAxisLabels = new string[] { "Nancy", "Andrew", "Janet", "Margaret", "Steven",
                 "Michael", "Robert", "Laura", "Anne", "Paul", "Karin", "Mario" };
    public string[] yAxisLabels = new string[] { "Mon", "Tues", "Wed", "Thurs", "Fri", "Sat" };
}
```

## TooltipRendering Event

The [TooltipRendering](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.HeatMapEvents.html#Syncfusion_Blazor_HeatMap_HeatMapEvents_TooltipRendering) event is triggered before the tooltip is rendered on the HeatMap cell. The [TooltipEventArgs](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.TooltipEventArgs.html) provides properties such as **Content**, **XLabel**, **YLabel**, **XValue**, **YValue**, and **Value** to customize tooltip information.

This event provides the ability to customize tooltip content dynamically by modifying the **Content** property based on cell data accessed through **XLabel**, **YLabel**, and **Value** properties.

### Example

The following example demonstrates how to use the `TooltipRendering` event to customize tooltips for specific cells when hovering over them.

```cshtml
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@dataSource">
    <HeatMapEvents TooltipRendering="@TooltipRendering" />
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)" />
    <HeatMapXAxis Labels="@xAxisLabels" />
    <HeatMapYAxis Labels="@yAxisLabels" />
</SfHeatMap>

@code {
    private void TooltipRendering(Syncfusion.Blazor.HeatMap.TooltipEventArgs args)
    {
        args.Content = new string[] { $"On {args.YLabel}, {args.XLabel} contributed {args.Value * 1000:C0} in sales revenue." };
    }
    public double[,] dataSource = new double[,]
    {
            { 73, 39, 26, 39, 94, 0 },
            { 93, 58, 53, 38, 26, 68 },
            { 99, 28, 22, 4, 66, 90 },
            { 14, 26, 97, 69, 69, 3 },
            { 7, 46, 47, 47, 88, 6 },
            { 41, 55, 73, 23, 3, 79 },
            { 56, 69, 21, 86, 3, 33 },
            { 45, 7, 53, 81, 95, 79 },
            { 60, 77, 74, 68, 88, 51 },
            { 25, 25, 10, 12, 78, 14 },
            { 25, 56, 55, 58, 12, 82 },
            { 74, 33, 88, 23, 86, 59 }
    };
    public string[] xAxisLabels = new string[] { "Nancy", "Andrew", "Janet", "Margaret", "Steven",
                 "Michael", "Robert", "Laura", "Anne", "Paul", "Karin", "Mario" };
    public string[] yAxisLabels = new string[] { "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday" };
}
```

## CellDoubleClicked Event

When you double-click on a HeatMap cell, the [CellDoubleClicked](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.HeatMapEvents.html#Syncfusion_Blazor_HeatMap_HeatMapEvents_CellDoubleClicked) event is triggered. The [CellDoubleClickEventArgs](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.CellDoubleClickEventArgs.html) provides properties such as **CellElement**, **X**, **Y**, **XLabel**, **YLabel**, and **Value** to retrieve cell information.

This event is useful for implementing drill-down functionality or displaying detailed information about a cell by accessing properties like **XLabel**, **YLabel**, and **Value** when users double-click on it.

### Example

The following example demonstrates how to use the `CellDoubleClicked` event to retrieve the value of a cell, as well as its x-axis and y-axis labels, by performing a double-click action.

```cshtml
@using Syncfusion.Blazor.HeatMap

@if(IsVisible) {
    <div>
        <span> X-Label : <b> @XLabel </b> </span> <br />
        <span> Y-Label : <b> @YLabel </b> </span> <br />
        <span> CellValue : <b> @CellValue </b> </span>
    </div>
}

<SfHeatMap DataSource="@dataSource">
    <HeatMapEvents CellDoubleClicked="@CellDoubleClicked" />
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)" />
    <HeatMapXAxis Labels="@xAxisLabels" />
    <HeatMapYAxis Labels="@yAxisLabels" />
</SfHeatMap>

@code {
    public bool IsVisible = false;
    public string XLabel { get; set; }
    public string YLabel { get; set; }
    public double CellValue { get; set; }
    private void CellDoubleClicked(CellDoubleClickEventArgs args)
    {
        IsVisible = true;
        XLabel = args.XLabel;
        YLabel = args.YLabel;
        CellValue = args.Value;
    }
    public double[,] dataSource = new double[,]
    {
            { 73, 39, 26, 39, 94, 0 },
            { 93, 58, 53, 38, 26, 68 },
            { 99, 28, 22, 4, 66, 90 },
            { 14, 26, 97, 69, 69, 3 },
            { 7, 46, 47, 47, 88, 6 },
            { 41, 55, 73, 23, 3, 79 },
            { 56, 69, 21, 86, 3, 33 },
            { 45, 7, 53, 81, 95, 79 },
            { 60, 77, 74, 68, 88, 51 },
            { 25, 25, 10, 12, 78, 14 },
            { 25, 56, 55, 58, 12, 82 },
            { 74, 33, 88, 23, 86, 59 }
    };
    public string[] xAxisLabels = new string[] { "Nancy", "Andrew", "Janet", "Margaret", "Steven",
                 "Michael", "Robert", "Laura", "Anne", "Paul", "Karin", "Mario" };
    public string[] yAxisLabels = new string[] { "Mon", "Tues", "Wed", "Thurs", "Fri", "Sat" };
}
```

## LegendRendering Event

The [LegendRendering](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.HeatMapEvents.html#Syncfusion_Blazor_HeatMap_HeatMapEvents_LegendRendering) event is triggered before each legend item is rendered. The [LegendRenderEventArgs](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.HeatMap.LegendRenderEventArgs.html) provides a **Text** property to customize legend item text.

This event allows you to customize legend items by modifying the **Text** property or adjusting colors and hiding specific legend items before they are rendered.

### Example

The following example demonstrates how to use the `LegendRendering` event to customize the value of the text in the legend.

```cshtml
@using Syncfusion.Blazor.HeatMap

<SfHeatMap DataSource="@HeatMapData">
    <HeatMapEvents LegendRendering="@LegendRender" />
    <HeatMapTitleSettings Text="Sales Revenue per Employee (in 1000 US$)">
    </HeatMapTitleSettings>
    <HeatMapXAxis Labels="@XAxisLabels"></HeatMapXAxis>
    <HeatMapYAxis Labels="@YAxisLabels"></HeatMapYAxis>
    <HeatMapCellSettings ShowLabel="true" TileType="CellType.Rect"></HeatMapCellSettings>
    <HeatMapPaletteSettings Type="PaletteType.Gradient">
        <HeatMapPalettes>
            <HeatMapPalette Value="0" Color="#C2E7EC"></HeatMapPalette>
            <HeatMapPalette Value="10" Color="#AEDFE6"></HeatMapPalette>
            <HeatMapPalette Value="20" Color="#9AD7E0"></HeatMapPalette>
            <HeatMapPalette Value="30" Color="#72C7D4"></HeatMapPalette>
            <HeatMapPalette Value="40" Color="#5EBFCE"></HeatMapPalette>
            <HeatMapPalette Value="50" Color="#4AB7C8"></HeatMapPalette>
            <HeatMapPalette Value="60" Color="#309DAE"></HeatMapPalette>
            <HeatMapPalette Value="70" Color="#2B8C9B"></HeatMapPalette>
            <HeatMapPalette Value="80" Color="#257A87"></HeatMapPalette>
            <HeatMapPalette Value="90" Color="#15464D"></HeatMapPalette>
            <HeatMapPalette Value="100" Color="#000000"></HeatMapPalette>
        </HeatMapPalettes>
    </HeatMapPaletteSettings>
    <HeatMapLegendSettings Visible="true"></HeatMapLegendSettings>
</SfHeatMap>

@code {
    public int[,] HeatMapData = new int[,]
    {
        {73, 39, 26, 39, 94, 0},
        {93, 58, 53, 38, 26, 68},
        {99, 28, 22, 4, 66, 90},
        {14, 26, 97, 69, 69, 3},
        {7, 46, 47, 47, 88, 6},
        {41, 55, 73, 23, 3, 79}
    };
    public string[] XAxisLabels = new string[] { "Nancy", "Andrew", "Janet", "Margaret", "Steven", "Michael" };
    public string[] YAxisLabels = new string[] { "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };
    private void LegendRender(Syncfusion.Blazor.HeatMap.LegendRenderEventArgs args)
    {
        if (args.Text != "0")
        {
            args.Text = "";
        }
    }
}
```
