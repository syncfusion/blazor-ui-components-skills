# Data Binding and Field Mapping in Blazor Gantt Chart

## Overview

Data binding connects the Blazor Gantt Chart to data sources, enabling dynamic visualization and management of project information. The component supports both local and remote data through the `DataSource` property, which accepts either a list of business objects or a `SfDataManager` instance.

## Table of Contents
- [Data Structure Types](#data-structure-types)
- [Task Field Mapping](#task-field-mapping)
- [Dynamic Binding Options](#dynamic-binding-options)
- [Observable Collections](#observable-collections)
- [Best Practices](#best-practices)

## Data Structure Types

### Hierarchical Data Binding

Hierarchical data organizes complex parent-child relationships through nested object structures using the `Child` field mapping.

**Configuration**
```csharp
<SfGantt DataSource="@TaskCollection">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" 
                     EndDate="EndDate" Duration="Duration" Progress="Progress" 
                     Child="SubTasks">
    </GanttTaskFields>
</SfGantt>
```

**Data Model**
```csharp
public class TaskData
{
    public int TaskID { get; set; }
    public string TaskName { get; set; }
    public DateTime StartDate { get; set; }
    public DateTime? EndDate { get; set; }
    public string Duration { get; set; }
    public int Progress { get; set; }
    public List<TaskData> SubTasks { get; set; }  // Child tasks
}
```

**Sample Data Structure**
```csharp
List<TaskData> Tasks = new List<TaskData>()
{
    new TaskData() 
    { 
        TaskID = 1, 
        TaskName = "Project initiation", 
        StartDate = new DateTime(2022, 04, 05), 
        SubTasks = new List<TaskData>()
        {
            new TaskData() { TaskID = 2, TaskName = "Identify Site", StartDate = new DateTime(2022, 04, 05), Duration = "3" },
            new TaskData() { TaskID = 3, TaskName = "Perform soil test", StartDate = new DateTime(2022, 04, 05), Duration = "4" }
        }
    }
};
```

**Limitations**
- Indent/Outdent not supported
- ExpandCollapse state maintenance not supported
- Row drag-and-drop not supported

### Self-Referential Data Binding

Self-referential data uses flat structures where tasks reference relationships through ID fields. This is ideal for database-driven applications.

**Configuration**
```csharp
<SfGantt DataSource="@TaskCollection">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" 
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
</SfGantt>
```

**Data Model**
```csharp
public class TaskData
{
    public int TaskID { get; set; }
    public string TaskName { get; set; }
    public DateTime StartDate { get; set; }
    public string Duration { get; set; }
    public int Progress { get; set; }
    public int? ParentID { get; set; }  // References parent task
}
```

**Sample Data**
```csharp
List<TaskData> Tasks = new List<TaskData>()
{
    new TaskData() { TaskID = 1, TaskName = "Project initiation", StartDate = new DateTime(2021, 04, 05) },
    new TaskData() { TaskID = 2, TaskName = "Identify Site", StartDate = new DateTime(2021, 04, 05), Duration = "4", ParentID = 1 },
    new TaskData() { TaskID = 3, TaskName = "Perform soil test", StartDate = new DateTime(2021, 04, 05), Duration = "4", ParentID = 1 }
};
```

**Advantages**
- Full support for Indent/Outdent
- Drag-and-drop reordering supported
- Ideal for Entity Framework and Dapper
- Natural fit for database foreign key relationships

## Task Field Mapping

The `GanttTaskFields` component maps data properties to Gantt Chart features.

### Required Fields

| Field | Property | Description | Type |
|-------|----------|-------------|------|
| ID | `Id` | Unique task identifier | int/string |
| Name | `Name` | Task display name | string |
| Start Date | `StartDate` | Task start date/time | DateTime |

### Optional Fields

| Field | Property | Description | Type |
|-------|----------|-------------|------|
| End Date | `EndDate` | Task end date/time | DateTime |
| Duration | `Duration` | Task length (e.g., "5 days") | string |
| Progress | `Progress` | Completion percentage (0-100) | int |
| Parent ID | `ParentID` | Parent task reference | int/string |
| Child | `Child` | Nested child tasks | List<T> |
| Dependency | `Dependency` | Task dependencies (e.g., "2FS") | string |
| Notes | `Notes` | Task description/notes | string |
| BaselineStartDate | `BaselineStartDate` | Planned start date | DateTime |
| BaselineEndDate | `BaselineEndDate` | Planned end date | DateTime |
| Resource Info | `ResourceInfo` | Assigned resources | List<Resource> |
| Work | `Work` | Effort in hours | double |
| CSS Class | `CssClass` | Custom CSS for row | string |

**Complete Mapping Example**
```csharp
<GanttTaskFields 
    Id="TaskID" 
    Name="TaskName" 
    StartDate="StartDate" 
    EndDate="EndDate"
    Duration="Duration" 
    Progress="Progress" 
    ParentID="ParentID"
    Dependency="Predecessor"
    Notes="Description"
    BaselineStartDate="PlannedStart"
    BaselineEndDate="PlannedEnd"
    ResourceInfo="Resources"
    Work="EstimatedHours"
    CssClass="RowStyle">
</GanttTaskFields>
```

## Dynamic Binding Options

### DynamicObject Binding

Handles scenarios where the data model is unknown at compile time.

**Required Namespace**
```csharp
using System.Dynamic;
```

**Data Model**
```csharp
public class DynamicDictionary : DynamicObject
{
    Dictionary<string, object> dictionary = new Dictionary<string, object>();
    
    public override bool TryGetMember(GetMemberBinder binder, out object result)
    {
        return dictionary.TryGetValue(binder.Name, out result);
    }
    
    public override bool TrySetMember(SetMemberBinder binder, object value)
    {
        dictionary[binder.Name] = value;
        return true;
    }
    
    public override IEnumerable<string> GetDynamicMemberNames()
    {
        return dictionary?.Keys;
    }
}
```

**Usage**
```csharp
<SfGantt DataSource="@DynamicData" Height="450px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" 
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
</SfGantt>

@code {
    private List<DynamicDictionary> DynamicData { get; set; }
    
    protected override void OnInitialized()
    {
        DynamicData = new List<DynamicDictionary>();
        dynamic task = new DynamicDictionary();
        task.TaskID = 1;
        task.TaskName = "Dynamic Task";
        task.StartDate = DateTime.Now;
        task.Duration = "5";
        task.Progress = 50;
        DynamicData.Add(task);
    }
}
```

**Requirements**
- Override `GetDynamicMemberNames()` method
- Return all property names needed for rendering and operations

### ExpandoObject Binding

Alternative for runtime-defined models.

**Required Namespace**
```csharp
using System.Dynamic;
```

**Usage**
```csharp
<SfGantt TValue="ExpandoObject" DataSource="@ExpandoData">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" 
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
</SfGantt>

@code {
    private List<ExpandoObject> ExpandoData { get; set; }
    
    protected override void OnInitialized()
    {
        ExpandoData = new List<ExpandoObject>();
        dynamic task = new ExpandoObject();
        task.TaskID = 1;
        task.TaskName = "Expando Task";
        task.StartDate = DateTime.Now;
        task.Duration = "5";
        task.Progress = 50;
        ExpandoData.Add(task);
    }
}
```

## Observable Collections

### ObservableCollection Support

Automatically updates UI when data changes using `INotifyCollectionChanged`.

**Required Namespaces**
```csharp
using System.Collections.ObjectModel;
using System.Collections.Specialized;
```

**Configuration**
```csharp
@using System.Collections.ObjectModel
@using System.Collections.Specialized

<SfGantt DataSource="@ObservableData">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" 
                     Duration="Duration" ParentID="ParentID">
    </GanttTaskFields>
    <GanttEditSettings AllowAdding="true" AllowDeleting="true"></GanttEditSettings>
</SfGantt>

@code {
    public ObservableCollection<TaskData> ObservableData { get; set; }
    
    protected override void OnInitialized()
    {
        ObservableData = new ObservableCollection<TaskData>()
        {
            new TaskData() { TaskID = 1, TaskName = "Task 1", StartDate = DateTime.Now }
        };
        ObservableData.CollectionChanged += OnCollectionChanged;
    }
    
    public void OnCollectionChanged(object sender, NotifyCollectionChangedEventArgs e)
    {
        if (e.Action == NotifyCollectionChangedAction.Add)
        {
            Console.WriteLine($"Task added: {((TaskData)e.NewItems[0]).TaskName}");
        }
    }
}
```

**Supported Operations**
- Add: Automatically renders new tasks
- Remove: Automatically removes tasks from view
- Move: Reorders tasks
- Clear: Removes all tasks

### INotifyPropertyChanged Support

Updates UI when individual property values change.

**Required Namespace**
```csharp
using System.ComponentModel;
```

**Data Model**
```csharp
public class TaskData : INotifyPropertyChanged
{
    public int TaskID { get; set; }
    
    private string taskName;
    public string TaskName
    {
        get => taskName;
        set
        {
            if (taskName != value)
            {
                taskName = value;
                NotifyPropertyChanged(nameof(TaskName));
            }
        }
    }
    
    public DateTime StartDate { get; set; }
    public string Duration { get; set; }
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    private void NotifyPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

**Usage**
```csharp
public void UpdateTask()
{
    if (ObservableData.Count > 0)
    {
        var task = ObservableData.First();
        task.TaskName = "Updated Task Name";  // Automatically updates UI
    }
}
```

## Best Practices

### Choosing Data Structure

| Scenario | Recommended Structure | Reason |
|----------|----------------------|---------|
| Database with foreign keys | Self-referential | Natural fit for EF/Dapper |
| JSON/API with nested objects | Hierarchical | Matches data format |
| Need Indent/Outdent | Self-referential | Feature supported |
| Simple static data | Either | Choose based on preference |

### Performance Tips

**Large Datasets**
- Use self-referential structure for better performance
- Enable virtualization with `EnableVirtualization="true"`
- Load child tasks on-demand for deep hierarchies

**Data Updates**
- Use ObservableCollection for frequent additions/removals
- Implement INotifyPropertyChanged for property updates
- Batch updates when possible

**Field Mapping**
- Map only required fields
- Use nullable types (`DateTime?`, `int?`) for optional fields
- Avoid complex object references in mapped fields

### Common Pitfalls

**Incorrect Parent-Child References**
```csharp
// ❌ Wrong - ParentID references non-existent task
new TaskData() { TaskID = 2, TaskName = "Child", ParentID = 999 }

// ✅ Correct - ParentID references valid parent
new TaskData() { TaskID = 1, TaskName = "Parent" },
new TaskData() { TaskID = 2, TaskName = "Child", ParentID = 1 }
```

**Missing Required Fields**
```csharp
// ❌ Wrong - Missing StartDate
<GanttTaskFields Id="TaskID" Name="TaskName" Duration="Duration">

// ✅ Correct - All required fields present
<GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate">
```

**Type Mismatches**
```csharp
// ❌ Wrong - Duration should be string
public int Duration { get; set; }

// ✅ Correct - Duration as string
public string Duration { get; set; }  // e.g., "5 days"
```

## Summary

- **Data Structures**: Choose hierarchical for nested data, self-referential for database integration
- **Required Fields**: Always map Id, Name, and StartDate
- **Dynamic Binding**: Use DynamicObject/ExpandoObject when model is unknown at compile time
- **Reactive Updates**: Use ObservableCollection and INotifyPropertyChanged for automatic UI updates
- **Performance**: Self-referential structure performs better with large datasets and virtualization
