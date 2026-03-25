# State Management in Syncfusion Blazor TreeGrid

## Table of Contents
- [Auto Persistence with EnablePersistence](#auto-persistence-with-enablepersistence)
- [Persisted Properties](#persisted-properties)
- [Manual State Save and Restore](#manual-state-save-and-restore)
- [State Method Summary](#state-method-summary)

---

## Auto Persistence with EnablePersistence

Tree Grid Component retains its state using local storage on browser reload. State persistence is enabled through the `EnablePersistence` property, which is disabled by default. When enabled, the grid's state is preserved even after refreshing the page.

```cshtml
@using Syncfusion.Blazor.TreeGrid;

<SfTreeGrid ID="TreeGrid"
            DataSource="@TreeData"
            Height="312"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            TreeColumnIndex="1"
            EnablePersistence="true"
            AllowPaging="true"
            AllowFiltering="true"
            AllowSorting="true"
            AllowReordering="true"
            AllowResizing="true"
            ShowColumnMenu="true"
            Toolbar="@(new List<string>() { "Search" })">
    <TreeGridPageSettings PageSize="2" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80"  TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public class BusinessObject
    {
        public int TaskId { get; set; }
        public string TaskName { get; set; }
        public int Duration { get; set; }
        public int Progress { get; set; }
        public string Priority { get; set; }
        public int? ParentId { get; set; }
    }

    public List<BusinessObject> TreeData = new List<BusinessObject>();

    protected override void OnInitialized()
    {
        TreeData.Add(new BusinessObject() { TaskId = 1, TaskName = "Parent Task 1", Duration = 10, Progress = 70, ParentId = null, Priority = "High" });
        TreeData.Add(new BusinessObject() { TaskId = 2, TaskName = "Child task 1",  Duration = 4,  Progress = 80, ParentId = 1,    Priority = "Normal" });
        TreeData.Add(new BusinessObject() { TaskId = 3, TaskName = "Child Task 2",  Duration = 5,  Progress = 65, ParentId = 1,    Priority = "Critical" });
        TreeData.Add(new BusinessObject() { TaskId = 4, TaskName = "Parent Task 2", Duration = 6,  Progress = 77, ParentId = null, Priority = "Low" });
        TreeData.Add(new BusinessObject() { TaskId = 5, TaskName = "Child Task 5",  Duration = 9,  Progress = 25, ParentId = 4,    Priority = "Normal" });
        TreeData.Add(new BusinessObject() { TaskId = 6, TaskName = "Child Task 6",  Duration = 9,  Progress = 7,  ParentId = 5,    Priority = "Normal" });
        TreeData.Add(new BusinessObject() { TaskId = 7, TaskName = "Parent Task 3", Duration = 4,  Progress = 45, ParentId = null, Priority = "High" });
        TreeData.Add(new BusinessObject() { TaskId = 8, TaskName = "Child Task 7",  Duration = 3,  Progress = 38, ParentId = 7,    Priority = "Critical" });
        TreeData.Add(new BusinessObject() { TaskId = 9, TaskName = "Child Task 8",  Duration = 7,  Progress = 70, ParentId = 7,    Priority = "Low" });
    }
}
```

> **Important:** The state is persisted based on the **ID** property. Always set a unique `ID` on the TreeGrid — without it, state may not restore correctly.
> You can use `ResetPersistDataAsync` to reset the TreeGrid state to its original values. This clears persisted data in window local storage and re-renders the grid with its original property values.

---

## Persisted Properties

When `EnablePersistence="true"`, the following properties are saved and restored:

| Property |
|---|
| `Columns` |
| `TreeColumnIndex` |
| `TreeGridFilterSettings` |
| `TreeGridSortSettings` |
| `TreeGridPageSettings` |

---

## Manual State Save and Restore

You can handle the TreeGrid's state manually using the built-in state persistence methods: `GetPersistDataAsync`, `SetPersistDataAsync`, and `ResetPersistDataAsync`.

1. `GetPersistDataAsync` — Returns tree grid properties as a string value, suitable for sending over a network and storing in databases.
2. `SetPersistDataAsync` — Loads a previously saved state into the tree grid.
3. `ResetPersistDataAsync` — Clears persisted data in window local storage and renders the tree grid with its original property values.

```cshtml
@using Syncfusion.Blazor.TreeGrid;

<button id="GetPersistence"   @onclick="GetPersistence">Save Current State</button>
<button id="SetPersistence"   @onclick="SetPersistence">Set State</button>
<button id="ClearPersistence" @onclick="ClearPersistence">Reset State</button>

<SfTreeGrid ID="TreeGridOne"
            @ref="TreeGrid"
            DataSource="@TreeData"
            Height="312"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            TreeColumnIndex="1"
            EnablePersistence="true"
            AllowPaging="true"
            AllowFiltering="true"
            AllowSorting="true"
            AllowReordering="true"
            AllowResizing="true"
            ShowColumnMenu="true"
            Toolbar="@(new List<string>() { "Search" })">
    <TreeGridPageSettings PageSize="2" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80"  TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public SfTreeGrid<BusinessObject> TreeGrid;
    public string SavedPersistence { get; set; } = "";

    private async Task GetPersistence()
    {
        SavedPersistence = await TreeGrid.GetPersistDataAsync();
    }

    private async Task SetPersistence()
    {
        await TreeGrid.SetPersistDataAsync(SavedPersistence);
    }

    private async void ClearPersistence()
    {
        await TreeGrid.ResetPersistDataAsync();
    }

    public class BusinessObject
    {
        public int TaskId { get; set; }
        public string TaskName { get; set; }
        public int Duration { get; set; }
        public int Progress { get; set; }
        public string Priority { get; set; }
        public int? ParentId { get; set; }
    }

    public List<BusinessObject> TreeData = new List<BusinessObject>();

    protected override void OnInitialized()
    {
        TreeData.Add(new BusinessObject() { TaskId = 1, TaskName = "Parent Task 1", Duration = 10, Progress = 70, ParentId = null, Priority = "High" });
        TreeData.Add(new BusinessObject() { TaskId = 2, TaskName = "Child task 1",  Duration = 4,  Progress = 80, ParentId = 1,    Priority = "Normal" });
        TreeData.Add(new BusinessObject() { TaskId = 3, TaskName = "Child Task 2",  Duration = 5,  Progress = 65, ParentId = 1,    Priority = "Critical" });
        TreeData.Add(new BusinessObject() { TaskId = 4, TaskName = "Parent Task 2", Duration = 6,  Progress = 77, ParentId = null, Priority = "Low" });
        TreeData.Add(new BusinessObject() { TaskId = 5, TaskName = "Child Task 5",  Duration = 9,  Progress = 25, ParentId = 4,    Priority = "Normal" });
        TreeData.Add(new BusinessObject() { TaskId = 6, TaskName = "Child Task 6",  Duration = 9,  Progress = 7,  ParentId = 5,    Priority = "Normal" });
        TreeData.Add(new BusinessObject() { TaskId = 7, TaskName = "Parent Task 3", Duration = 4,  Progress = 45, ParentId = null, Priority = "High" });
        TreeData.Add(new BusinessObject() { TaskId = 8, TaskName = "Child Task 7",  Duration = 3,  Progress = 38, ParentId = 7,    Priority = "Critical" });
        TreeData.Add(new BusinessObject() { TaskId = 9, TaskName = "Child Task 8",  Duration = 7,  Progress = 70, ParentId = 7,    Priority = "Low" });
    }
}
```

---

## State Method Summary

| Method | Description |
|---|---|
| `GetPersistDataAsync()` | Returns tree grid properties as a string value, suitable for network transfer and database storage |
| `SetPersistDataAsync(string)` | Loads a previously saved state into the tree grid |
| `ResetPersistDataAsync()` | Clears persisted data in window local storage and reverts to original property values |

---
