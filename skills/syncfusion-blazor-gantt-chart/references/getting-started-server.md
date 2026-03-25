# Getting Started with Blazor Gantt Chart - Server App

## Table of Contents
- [Install Gantt NuGet Packages](#install-gantt-nuget-packages)
- [Register Syncfusion Service](#register-syncfusion-service)
- [Add Namespace and Theme](#add-namespace-and-theme)
- [Basic Gantt Component](#basic-gantt-component)
- [Server-Specific Gantt Considerations](#server-specific-gantt-considerations)
- [Gantt Performance Tips for Server](#gantt-performance-tips-for-server)

---

This guide covers the Syncfusion Blazor Gantt Chart setup steps specific to a **Blazor Server** application. For general Blazor Server project creation, refer to the Microsoft Blazor documentation.

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

builder.Services.AddSyncfusionBlazor();
```

> **Note:** `AddSyncfusionBlazor()` must be called after `AddRazorComponents()`.

---

## Add Namespace and Theme

**`~/_Imports.razor`** — add Gantt namespace:
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Gantt
```

**`~/Components/App.razor`** — add theme link and script in `<head>`:
```html
<!-- Syncfusion Theme (choose one) -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Syncfusion Scripts -->
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
```

Available themes: `bootstrap5`, `material3`, `fluent2`, `tailwind3`, `highcontrast`.

---

## Basic Gantt Component

Create `~/Components/Pages/Gantt.razor`:

```razor
@page "/gantt"
@using Syncfusion.Blazor.Gantt
@rendermode InteractiveServer

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

**Key Gantt attributes:**
| Attribute | Required | Description |
|-----------|----------|-------------|
| `DataSource` | Yes | Collection of task data |
| `Height` / `Width` | Recommended | Control dimensions |
| `GanttTaskFields` | Yes | Maps model properties to Gantt fields |
| `@rendermode InteractiveServer` | Yes (Server) | Enables interactivity via SignalR |

---

## Server-Specific Gantt Considerations

### Render Mode

Always set `@rendermode InteractiveServer` on the page or component:
```razor
@rendermode InteractiveServer
```
Without this, the Gantt chart renders as static HTML with no interactivity.

### Prerendering Guard

If prerendering is enabled, the Gantt chart must not render until the Blazor circuit is established — otherwise JS interop calls fail:

```razor
@if (_isLoaded)
{
    <SfGantt DataSource="@TaskCollection" Height="450px" Width="100%">
        <GanttTaskFields Id="TaskId" Name="TaskName" StartDate="StartDate"
                         EndDate="EndDate" Duration="Duration" Progress="Progress"
                         ParentID="ParentId">
        </GanttTaskFields>
    </SfGantt>
}

@code {
    private bool _isLoaded;

    protected override void OnAfterRender(bool firstRender)
    {
        if (firstRender)
        {
            _isLoaded = true;
            StateHasChanged();
        }
    }
}
```

### Large Dataset Tip

For datasets with many tasks, increase the SignalR message size limit to prevent truncation errors:

```csharp
// Program.cs
builder.Services.AddServerSideBlazor()
    .AddHubOptions(options =>
    {
        options.MaximumReceiveMessageSize = 64 * 1024; // 64 KB
    });
```

---

## Gantt Performance Tips for Server

### 1. Row and Timeline Virtualization

```razor
<SfGantt DataSource="@TaskCollection"
         EnableRowVirtualization="true"
         EnableTimelineVirtualization="true">
```

Reduces DOM nodes for large task lists.

### 2. Load Children On Demand

For hierarchical data, load child tasks only when parent is expanded:

```razor
<GanttTaskFields Id="TaskId" Name="TaskName" StartDate="StartDate"
                 EndDate="EndDate" Duration="Duration" Progress="Progress"
                 ParentID="ParentId" HasChildMapping="IsParent">
</GanttTaskFields>
```

Requires `HasChildMapping` field and server-side data adapter.

### 3. Remote Data Instead of In-Memory

Use `SfDataManager` with `UrlAdaptor` to load data from an API endpoint instead of passing all data to the client over SignalR.

```razor
<SfGantt TValue="TaskData" Height="450px" Width="100%">
    <GanttTaskFields Id="TaskId" Name="TaskName" StartDate="StartDate"
                     EndDate="EndDate" Duration="Duration" Progress="Progress"
                     ParentID="ParentId">
    </GanttTaskFields>
    <SfDataManager Url="/api/gantt" Adaptor="Adaptors.UrlAdaptor"></SfDataManager>
</SfGantt>
```

