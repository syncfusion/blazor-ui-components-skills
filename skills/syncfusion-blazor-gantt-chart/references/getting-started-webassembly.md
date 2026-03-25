# Getting Started with Blazor Gantt Chart - WebAssembly

## Table of Contents
- [Install Gantt NuGet Packages](#install-gantt-nuget-packages)
- [Register Syncfusion Service](#register-syncfusion-service)
- [Add Namespace and Theme](#add-namespace-and-theme)
- [Basic Gantt Component](#basic-gantt-component)
- [GanttTaskFields Mapping](#gantttaskfields-mapping)
- [Common Configuration Options](#common-configuration-options)

---

This guide covers the Syncfusion Blazor Gantt Chart setup steps specific to a **Blazor WebAssembly** application. For general Blazor WebAssembly project creation, refer to the Microsoft Blazor documentation.

---

## Install Gantt NuGet Packages

**Package Manager Console:**
```powershell
Install-Package Syncfusion.Blazor.Gantt
Install-Package Syncfusion.Blazor.Themes
```

**.NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.Gantt
dotnet add package Syncfusion.Blazor.Themes
```

---

## Register Syncfusion Service

In `~/Program.cs`:

```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

---

## Add Namespace and Theme

**`~/_Imports.razor`** — add Gantt namespace:
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Gantt
```

**`~/wwwroot/index.html`** — add theme and script in `<head>`:
```html
<!-- Syncfusion Theme (choose one) -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Syncfusion Scripts -->
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
```

Available themes: `bootstrap5`, `material3`, `fluent2`, `tailwind3`, `highcontrast`.

---

## Basic Gantt Component

Create `~/Pages/Gantt.razor`:

```razor
@page "/gantt"
@using Syncfusion.Blazor.Gantt

<h3>Project Schedule</h3>

<SfGantt DataSource="@TaskCollection" Height="450px" Width="100%">
    <GanttTaskFields Id="TaskId"
                     Name="TaskName"
                     StartDate="StartDate"
                     EndDate="EndDate"
                     Duration="Duration"
                     Progress="Progress"
                     ParentID="ParentId">
    </GanttTaskFields>
</SfGantt>

@code {
    private List<TaskData> TaskCollection { get; set; }

    protected override void OnInitialized()
    {
        TaskCollection = GetTaskCollection();
    }

    public class TaskData
    {
        public int TaskId { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public DateTime? EndDate { get; set; }
        public string Duration { get; set; }
        public int Progress { get; set; }
        public int? ParentId { get; set; }
    }

    private static List<TaskData> GetTaskCollection()
    {
        return new List<TaskData>
        {
            new TaskData { TaskId = 1, TaskName = "Project Initiation", StartDate = new DateTime(2024, 01, 04), EndDate = new DateTime(2024, 01, 07) },
            new TaskData { TaskId = 2, TaskName = "Identify Site Location", StartDate = new DateTime(2024, 01, 04), Duration = "3", Progress = 30, ParentId = 1 },
            new TaskData { TaskId = 3, TaskName = "Perform Soil Test", StartDate = new DateTime(2024, 01, 04), Duration = "4", Progress = 40, ParentId = 1 },
            new TaskData { TaskId = 4, TaskName = "Soil Test Approval", StartDate = new DateTime(2024, 01, 08), Duration = "0", Progress = 30, ParentId = 1 },
            new TaskData { TaskId = 5, TaskName = "Project Estimation", StartDate = new DateTime(2024, 01, 10), EndDate = new DateTime(2024, 01, 15) },
            new TaskData { TaskId = 6, TaskName = "Develop Floor Plan", StartDate = new DateTime(2024, 01, 10), Duration = "3", Progress = 30, ParentId = 5 },
            new TaskData { TaskId = 7, TaskName = "List Materials", StartDate = new DateTime(2024, 01, 10), Duration = "3", Progress = 40, ParentId = 5 },
            new TaskData { TaskId = 8, TaskName = "Estimation Approval", StartDate = new DateTime(2024, 01, 13), Duration = "0", Progress = 30, ParentId = 5 }
        };
    }
}
```

---

## GanttTaskFields Mapping

The `GanttTaskFields` component maps your data model properties to Gantt Chart fields:

| Attribute | Maps To | Required | Description |
|-----------|---------|----------|-------------|
| `Id` | `TaskId` | Yes | Unique task identifier |
| `Name` | `TaskName` | Yes | Task display name in the grid |
| `StartDate` | `StartDate` | Yes | Task start date for scheduling |
| `EndDate` | `EndDate` | No | End date (can be derived from Duration) |
| `Duration` | `Duration` | No | Duration in days e.g. `"3"` (can be derived from dates) |
| `Progress` | `Progress` | No | Progress percentage 0-100 |
| `ParentID` | `ParentId` | No | Parent task Id — makes task a child |

> At least **Id**, **Name**, and **StartDate** must be mapped. Either **EndDate** or **Duration** is needed for the taskbar to render correctly.

For all available `GanttTaskFields` properties.

---

## Common Configuration Options

### Project Date Range

Constrain the visible timeline:
```razor
<SfGantt DataSource="@TaskCollection"
         ProjectStartDate="new DateTime(2024, 01, 01)"
         ProjectEndDate="new DateTime(2024, 12, 31)">
```

### Duration Unit

```razor
<SfGantt DurationUnit="DurationUnit.Day">
```

Options: `DurationUnit.Minute`, `DurationUnit.Hour`, `DurationUnit.Day` (default).

### Enable Editing

```razor
<SfGantt DataSource="@TaskCollection"
         Toolbar="@(new List<string>() { "Add", "Edit", "Delete", "Update", "Cancel" })">
    <GanttTaskFields Id="TaskId" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentId">
    </GanttTaskFields>
    <GanttEditSettings AllowAdding="true"
                       AllowEditing="true"
                       AllowDeleting="true"
                       AllowTaskbarEditing="true"
                       Mode="EditMode.Auto">
    </GanttEditSettings>
</SfGantt>
```

### Column Configuration

```razor
<SfGantt DataSource="@TaskCollection">
    <GanttTaskFields Id="TaskId" Name="TaskName" StartDate="StartDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentId">
    </GanttTaskFields>
    <GanttColumns>
        <GanttColumn Field="TaskId" HeaderText="ID" Width="60"></GanttColumn>
        <GanttColumn Field="TaskName" HeaderText="Task Name" Width="250"></GanttColumn>
        <GanttColumn Field="StartDate" HeaderText="Start" Width="120"></GanttColumn>
        <GanttColumn Field="Duration" HeaderText="Duration" Width="100"></GanttColumn>
        <GanttColumn Field="Progress" HeaderText="Progress" Width="100"></GanttColumn>
    </GanttColumns>
</SfGantt>
```
