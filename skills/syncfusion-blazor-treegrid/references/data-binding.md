# Data Binding in Syncfusion Blazor TreeGrid — Skills Reference

This document consolidates data-binding guidance for the Syncfusion Blazor TreeGrid component. It covers local (list) binding, remote binding via `SfDataManager`, lazy loading, observable collections, dynamic object support, and common troubleshooting notes.

## Quick rules and terminology
- IdMapping: property that uniquely identifies a row.
- ParentIdMapping: property that references the parent's Id (use `null` for root unless otherwise configured).
- ChildMapping: use this when your model already contains a nested children collection.
- HasChildMapping: boolean property indicating whether a record has children (used for lazy loading).
- TValue: explicitly set `TValue` on `<SfTreeGrid>` when using remote binding (`SfDataManager`) to avoid type-inference errors.

## Table of Contents
- [Self-Referential (Flat) Data](#self-referential-flat-data)
- [Hierarchical (Nested Children) Data](#hierarchical-nested-children-data)
- [Remote Data Binding](#remote-data-binding)
- [Load on Demand (Lazy Loading)](#load-on-demand-lazy-loading)
- [Observable Collection](#observable-collection)
- [ExpandoObject / DynamicObject Binding](#expandoobject--dynamicobject-binding)
- [Troubleshooting](#troubleshooting)

---

## Self-Referential (Flat) Data

Use a flat list where each item has an Id and a ParentId. This is the most common pattern for TreeGrid data.

Example:

```razor
<SfTreeGrid DataSource="@TreeData" IdMapping="TaskId" ParentIdMapping="ParentId" TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="ID" Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="200" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public class BusinessObject
    {
        public int    TaskId   { get; set; }
        public int?   ParentId { get; set; }   // null = root row
        public string TaskName { get; set; }
        public int    Duration { get; set; }
        public int    Progress { get; set; }
    }

    private List<BusinessObject> TreeData = new()
    {
        new() { TaskId = 1, ParentId = null, TaskName = "Parent Task 1", Duration = 10, Progress = 70 },
        new() { TaskId = 2, ParentId = 1,    TaskName = "Child task 1",  Duration = 4,  Progress = 80 },
        new() { TaskId = 3, ParentId = 1,    TaskName = "Child Task 2",  Duration = 5,  Progress = 65 },
        new() { TaskId = 4, ParentId = null, TaskName = "Parent Task 2", Duration = 6,  Progress = 77 },
        new() { TaskId = 5, ParentId = 4,    TaskName = "Child Task 5",  Duration = 9,  Progress = 25 },
    };
}
```

**Rules:**
- `IdMapping` — the property that uniquely identifies each row
- `ParentIdMapping` — the property pointing to the parent's ID; `null` or `0` = root
- IDs must be **unique** across all records

---

## Hierarchical (Nested Children) Data

If your model already includes nested child collections, use `ChildMapping`. `ChildMapping` and `IdMapping`/`ParentIdMapping` are mutually exclusive.

```razor
<SfTreeGrid DataSource="@TreeData" TValue="BusinessObject" ChildMapping="Children" TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="ID" Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="200" />
        <TreeGridColumn Field="Priority" HeaderText="Priority"  Width="120" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public class BusinessObject
    {
        public int    TaskId   { get; set; }
        public string TaskName { get; set; }
        public string Priority { get; set; }
        public List<BusinessObject> Children { get; set; }  // nested children
    }

    private List<BusinessObject> TreeData = new()
    {
        new()
        {
            TaskId = 1, TaskName = "Project Alpha", Priority = "High",
            Children = new()
            {
                new() { TaskId = 2, TaskName = "Design",      Priority = "Normal", Children = null },
                new() { TaskId = 3, TaskName = "Development", Priority = "High",
                    Children = new()
                    {
                        new() { TaskId = 4, TaskName = "Backend API", Priority = "Critical" }
                    }
                },
            }
        }
    };
}
```

> **Note:** `ChildMapping` and `IdMapping`/`ParentIdMapping` are mutually exclusive — use one or the other.

---

## Remote Data Binding (SfDataManager)

Use `SfDataManager` as a child of `<SfTreeGrid>` to bind to REST, OData, or Web API endpoints. Always set `TValue` on the TreeGrid when binding remotely.

Use `SfDataManager` to connect to REST APIs, OData, or GraphQL services.

```razor
<SfTreeGrid TValue="Employee" IdMapping="EmployeeID" ParentIdMapping="ReportsTo" TreeColumnIndex="1" HasChildMapping="IsParent">
    <SfDataManager Url="https://api.example.com/employees" Adaptor="Adaptors.UrlAdaptor" />
    <TreeGridColumns>
        <TreeGridColumn Field="EmployeeID" HeaderText="ID" Width="80" IsPrimaryKey="true" />
        <TreeGridColumn Field="FirstName" HeaderText="Name" Width="200" />
    </TreeGridColumns>
</SfTreeGrid>
```

> **`HasChildMapping`** — set to the boolean property that tells the TreeGrid whether a row has children (used for lazy loading; skip if loading all data at once).

### OData Adaptor
```razor
<SfDataManager Url="https://api.example.com/odata/Tasks"
               Adaptor="Adaptors.ODataV4Adaptor" />
```

### Web API Adaptor
```razor
<SfDataManager Url="https://api.example.com/api/tasks"
               Adaptor="Adaptors.WebApiAdaptor" />
```

**Server expectations for URL adaptor:**
- `GET` with query parameters (`$top`, `$skip`, `$filter`, `$orderby`)
- Response: `{ result: [...], count: N }`

---

## Load on Demand (Lazy Loading)

Use `LoadChildOnDemand="true"` plus `HasChildMapping` when you want children loaded only when a parent is expanded (useful for large remote datasets).

```razor
<SfTreeGrid TValue="Employee" IdMapping="EmployeeID" ParentIdMapping="ReportsTo" HasChildMapping="HasChildren" LoadChildOnDemand="true" TreeColumnIndex="1">
    <SfDataManager Url="https://api.example.com/employees" Adaptor="Adaptors.UrlAdaptor" />
    <TreeGridColumns>
        <TreeGridColumn Field="EmployeeID"  HeaderText="ID"         Width="80" />
        <TreeGridColumn Field="FirstName"   HeaderText="Name"       Width="200" />
        <TreeGridColumn Field="HasChildren" Visible="false" />
    </TreeGridColumns>
</SfTreeGrid>
```

- `LoadChildOnDemand="true"` — enables lazy loading
- `HasChildMapping` — boolean property indicating whether a row has children
- The server receives a parent ID parameter when a row is expanded

---

## Observable Collection

For real-time updates (add/remove without re-binding), use `ObservableCollection<T>`:

```razor
@using System.Collections.ObjectModel

<SfTreeGrid DataSource="@TreeData" IdMapping="TaskId" ParentIdMapping="ParentId" TreeColumnIndex="0">
    ...
</SfTreeGrid>

@code {
    private ObservableCollection<TaskItem> TreeData = new();

    protected override void OnInitialized()
    {
        TreeData.Add(new TaskItem { TaskId = 1, ParentId = null, TaskName = "Root" });
    }

    // Adding a record will auto-refresh the TreeGrid
    private void AddRecord()
    {
        TreeData.Add(new TaskItem { TaskId = 10, ParentId = 1, TaskName = "New Child" });
    }
}
```

---

## ExpandoObject / DynamicObject Binding

Useful for dynamic schemas where column structure is not known at compile time:

```razor
@using System.Dynamic

<SfTreeGrid DataSource="@TreeData"
            TValue="ExpandoObject"
            IdMapping="TaskID"
            ParentIdMapping="ParentID"
            TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskID"   HeaderText="ID"    Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Name"  Width="200" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    private List<ExpandoObject> TreeData = new();

    protected override void OnInitialized()
    {
        dynamic row1 = new ExpandoObject();
        row1.TaskID = 1; row1.ParentID = null; row1.TaskName = "Parent";
        dynamic row2 = new ExpandoObject();
        row2.TaskID = 2; row2.ParentID = 1;    row2.TaskName = "Child";
        TreeData.Add(row1);
        TreeData.Add(row2);
    }
}
```

DynamicObject notes:
- If you implement `DynamicObject`, override `GetDynamicMemberNames()` to list available properties so the TreeGrid can perform data operations and editing.

Reserved internal keywords (avoid using these property names on your data objects):
- childRecords, hasChildRecords, hasFilteredChildRecords, expanded, parentRecord, index, level, filterLevel, parentIdMapping, uniqueID, parentUniqueID, checkboxState

---

## Troubleshooting (quick)
- Rows not forming hierarchy: check `IdMapping`/`ParentIdMapping` property names and casing.
- All rows appear as root: ensure root rows use `ParentId = null` (or the value expected by your config).
- Remote data not loading: confirm the `Adaptor` matches your API and that `TValue` is set on the grid.
- Lazy load failing: provide `HasChildMapping` and verify the server accepts the parent id parameter.
- Duplicate ID error: ensure every `IdMapping` value is unique.

---

If you want, I can:
- Add short runnable examples (minimal Blazor project) demonstrating each binding mode.
- Generate `SmartObservableCollection<T>` source and a demo page showing `AddRange` performance.

File updated: [d:\\SKill-Update\\data-binding-skills.md](d:\\SKill-Update\\data-binding-skills.md)
