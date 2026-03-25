# Getting Started with Syncfusion Blazor TreeGrid

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Register Service & Namespaces](#register-service--namespaces)
- [Add Stylesheet & Script](#add-stylesheet--script)
- [Minimal TreeGrid Example](#minimal-treegrid-example)
- [Key Concepts](#key-concepts)
- [Server App Setup](#server-app-setup)
- [MAUI Blazor Setup](#maui-blazor-setup)

---

## Prerequisites

- .NET 6 / 7 / 8 / 9 SDK
- Visual Studio 2022 (v17.3+) or Visual Studio Code with C# extension
- Valid Syncfusion license (Community / Trial / Commercial)

---

## Installation

Install both NuGet packages — the component and a theme:

**Package Manager Console:**
```bash
Install-Package Syncfusion.Blazor.TreeGrid
Install-Package Syncfusion.Blazor.Themes
```

**.NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.TreeGrid
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

---

## Register Service & Namespaces

**`_Imports.razor`** — add namespace imports:
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.TreeGrid
@using Syncfusion.Blazor.Grids  @* Required for events, settings, and data types *@
```

**`Program.cs`** — register the Syncfusion Blazor service:
```csharp
using Syncfusion.Blazor;

// Blazor WASM
builder.Services.AddSyncfusionBlazor();
await builder.Build().RunAsync();

// Blazor Server
builder.Services.AddSyncfusionBlazor();
```

---

## Add Stylesheet & Script

**Blazor WASM — `wwwroot/index.html`:**
```html
<head>
    <!-- Syncfusion theme (choose one) -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <!-- Core script (required) -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

**Blazor Server — `Pages/_Host.cshtml` or `App.razor`:**
```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

**Available themes:** `bootstrap5`, `bootstrap5-dark`, `material`, `material-dark`, `material3`, `material3-dark`, `fluent`, `fluent-dark`, `tailwind`, `fabric`, `highcontrast`

---

## Minimal TreeGrid Example

```razor
@using Syncfusion.Blazor.TreeGrid

<SfTreeGrid DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="ID"        Width="80"  IsPrimaryKey="true" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="200" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public class TaskItem
    {
        public int TaskId { get; set; }
        public int? ParentId { get; set; }
        public string TaskName { get; set; }
        public int Duration { get; set; }
        public int Progress { get; set; }
    }

    private List<TaskItem> TreeData = new()
    {
        new() { TaskId = 1, ParentId = null, TaskName = "Planning",     Duration = 10, Progress = 70 },
        new() { TaskId = 2, ParentId = 1,    TaskName = "Research",     Duration = 4,  Progress = 80 },
        new() { TaskId = 3, ParentId = 1,    TaskName = "Design",       Duration = 5,  Progress = 65 },
        new() { TaskId = 4, ParentId = null, TaskName = "Development",  Duration = 20, Progress = 40 },
        new() { TaskId = 5, ParentId = 4,    TaskName = "Backend",      Duration = 10, Progress = 50 },
        new() { TaskId = 6, ParentId = 4,    TaskName = "Frontend",     Duration = 10, Progress = 30 },
    };
}
```

---

## Key Concepts

| Property | Purpose | Notes |
|---|---|---|
| `IdMapping` | Unique identifier property name | Required for self-referential data |
| `ParentIdMapping` | Parent ID property name | Null/0 = root-level row |
| `TreeColumnIndex` | Column that shows expand/collapse icon | 0-based index |
| `IsPrimaryKey` | Marks the primary key column | Required for editing |
| `TValue` | Generic type parameter | Inferred from `DataSource` type |

**Root rows** are records where `ParentId` is `null` or `0`.

**Expand/collapse all programmatically:**
```razor
<SfTreeGrid @ref="TreeRef" ...>...</SfTreeGrid>
<button @onclick="ExpandAll">Expand All</button>
<button @onclick="CollapseAll">Collapse All</button>

@code {
    private SfTreeGrid<TaskItem> TreeRef;

    private async Task ExpandAll()   => await TreeRef.ExpandAllAsync();
    private async Task CollapseAll() => await TreeRef.CollapseAllAsync();
}
```

---

## Server App Setup

Blazor Server uses `Pages/_Host.cshtml` for scripts and requires `AddSyncfusionBlazor()` in `Program.cs`. The rest is identical to WASM.

```csharp
// Program.cs (Blazor Server)
builder.Services.AddRazorPages();
builder.Services.AddServerSideBlazor();
builder.Services.AddSyncfusionBlazor();
```

**Gotcha:** In Server apps, add `@rendermode InteractiveServer` (for .NET 8+ with static SSR) to the component or route.

---

## MAUI Blazor Setup

```csharp
// MauiProgram.cs
builder.Services.AddMauiBlazorWebView();
builder.Services.AddSyncfusionBlazor();
```

In `wwwroot/index.html`:
```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

---
