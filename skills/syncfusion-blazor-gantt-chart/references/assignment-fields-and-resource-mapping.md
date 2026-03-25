# Assignment Fields and Resource Mapping — API Reference

Comprehensive guide for `GanttAssignmentFields<TValue, TAssignment>` class, which manages the many-to-many relationship between tasks and resources through resource assignment collections.

## Table of Contents
- [Overview](#overview)
- [GanttAssignmentFields Class](#ganttassignmentfields-class)
- [Properties](#properties)
- [Configuration Pattern](#configuration-pattern)
- [Resource Assignment Methods](#resource-assignment-methods)
- [Foreign Key Relationships](#foreign-key-relationships)
- [CRUD Operations](#crud-operations)
- [Events](#events)
- [Common Patterns](#common-patterns)
- [Troubleshooting](#troubleshooting)

---

## Overview

The `GanttAssignmentFields<TValue, TAssignment>` component establishes the connection between tasks and resources by mapping a resource assignment collection. This collection acts as a junction table that holds the relationships between tasks and resources, enabling multiple resources to be assigned to a single task and tracking allocation units (percentage of time dedicated).

**When to use:**
- Assigning multiple resources to tasks
- Tracking resource allocation percentages (units)
- Implementing resource views grouped by resource
- Managing resource assignment CRUD operations programmatically
- Establishing many-to-many relationships between tasks and resources

---

## GanttAssignmentFields Class

**Namespace:** `Syncfusion.Blazor.Gantt`

**Type Parameters:**
- **`TValue`** — Type of task collection data source
- **`TAssignment`** — Type of resource assignment collection data source

**Inheritance:** `SfOwningComponentBase` → `GanttAssignmentFields<TValue, TAssignment>`

**Implements:** `IComponent`, `IHandleEvent`, `IHandleAfterRender`, `IDisposable`

---

## Properties

### DataSource
**Type:** `IEnumerable<TAssignment>`

The resource assignment collection that contains the many-to-many relationship data between tasks and resources.

**Required:** Yes

**Example:**
```csharp
<GanttAssignmentFields DataSource="@AssignmentCollection" ...>
</GanttAssignmentFields>

@code {
    private List<ResourceAssignment> AssignmentCollection { get; set; }
}
```

---

### PrimaryKey
**Type:** `string`

The primary key property name in the assignment collection that uniquely identifies each assignment record.

**Required:** Yes

**Example:**
```csharp
<GanttAssignmentFields PrimaryKey="PrimaryId" ...>
</GanttAssignmentFields>

public class ResourceAssignment
{
    public int PrimaryId { get; set; }  // Unique identifier
    public int TaskId { get; set; }
    public int ResourceId { get; set; }
}
```

---

### TaskID
**Type:** `string`

The property name in the assignment collection that maps to the task's primary key (from `GanttTaskFields.Id`). This establishes the foreign key relationship to the task collection.

**Required:** Yes

**Example:**
```csharp
<GanttAssignmentFields TaskID="TaskId" ...>
</GanttAssignmentFields>
```

---

### ResourceID
**Type:** `string`

The property name in the assignment collection that maps to the resource's primary key (from `GanttResource.Id`). This establishes the foreign key relationship to the resource collection.

**Required:** Yes

**Example:**
```csharp
<GanttAssignmentFields ResourceID="ResourceId" ...>
</GanttAssignmentFields>
```

---

### Units
**Type:** `string`

The property name that specifies the allocation percentage for a resource on a task. Represents the percentage of time or capacity a resource dedicates to the task.

**Default:** 100% if not specified

**Common Values:**
- `100` — Full-time allocation (100% of resource's capacity)
- `50` — Half-time allocation (50% of resource's capacity)
- `300` — Three full-time equivalent resources working on task

**Example:**
```csharp
<GanttAssignmentFields Units="Unit" ...>
</GanttAssignmentFields>

public class ResourceAssignment
{
    public int PrimaryId { get; set; }
    public int TaskId { get; set; }
    public int ResourceId { get; set; }
    public double Unit { get; set; } = 100;  // Default 100%
}
```

---

### DataManager
**Type:** `DataManager`

Instance of `DataManager` for remote data binding scenarios. Facilitates interactions with remote data sources for resource assignment operations.

**Example:**
```csharp
<GanttAssignmentFields DataManager="@AssignmentDataManager" ...>
</GanttAssignmentFields>

@code {
    private DataManager AssignmentDataManager { get; set; }
}
```

---

### ResourceAssignmentChanging
**Type:** `EventCallback<ResourceAssignmentChangeEventArgs<TAssignment>>`

Event raised when resource assignments are added, updated, or deleted. Provides details about the assignment collections being modified.

**Event Args Properties:**
- `AddedRecords` — List of newly added assignments
- `ChangedRecords` — List of modified assignments
- `DeletedRecords` — List of removed assignments

**Example:**
```csharp
<GanttAssignmentFields ResourceAssignmentChanging="@OnAssignmentChanging" ...>
</GanttAssignmentFields>

@code {
    private void OnAssignmentChanging(ResourceAssignmentChangeEventArgs<ResourceAssignment> args)
    {
        Console.WriteLine($"Added: {args.AddedRecords?.Count ?? 0}");
        Console.WriteLine($"Changed: {args.ChangedRecords?.Count ?? 0}");
        Console.WriteLine($"Deleted: {args.DeletedRecords?.Count ?? 0}");
    }
}
```

---

## Configuration Pattern

### Complete Setup Example

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt @ref="GanttInstance" 
         DataSource="@TaskCollection" 
         Height="450px" 
         Width="100%">
    <GanttTaskFields Id="TaskId" 
                     Name="TaskName" 
                     StartDate="StartDate" 
                     EndDate="EndDate" 
                     Duration="Duration">
    </GanttTaskFields>
    
    <GanttResource DataSource="@ResourceCollection" 
                   Id="ResourceId" 
                   Name="ResourceName" 
                   MaxUnits="MaxUnits" 
                   TValue="TaskData" 
                   TResources="ResourceData">
    </GanttResource>
    
    <GanttAssignmentFields DataSource="@AssignmentCollection"
                           PrimaryKey="PrimaryId"
                           TaskID="TaskId"
                           ResourceID="ResourceId"
                           Units="Unit"
                           TValue="TaskData"
                           TAssignment="ResourceAssignment">
    </GanttAssignmentFields>
    
    <GanttLabelSettings RightLabel="Resources" TValue="TaskData">
    </GanttLabelSettings>
</SfGantt>

@code {
    private SfGantt<TaskData> GanttInstance;
    
    private List<TaskData> TaskCollection { get; set; }
    private List<ResourceData> ResourceCollection { get; set; }
    private List<ResourceAssignment> AssignmentCollection { get; set; }
    
    protected override void OnInitialized()
    {
        TaskCollection = GetTasks();
        ResourceCollection = GetResources();
        AssignmentCollection = GetAssignments();
    }
    
    // Task model
    public class TaskData
    {
        public int TaskId { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public DateTime? EndDate { get; set; }
        public string Duration { get; set; }
    }
    
    // Resource model
    public class ResourceData
    {
        public int ResourceId { get; set; }
        public string ResourceName { get; set; }
        public double MaxUnits { get; set; }
    }
    
    // Resource assignment model (junction table)
    public class ResourceAssignment
    {
        public int PrimaryId { get; set; }      // Unique identifier
        public int TaskId { get; set; }         // Foreign key to TaskData
        public int ResourceId { get; set; }     // Foreign key to ResourceData
        public double Unit { get; set; } = 100; // Allocation percentage
    }
    
    private List<TaskData> GetTasks()
    {
        return new List<TaskData>
        {
            new TaskData { TaskId = 1, TaskName = "Project Planning", StartDate = new DateTime(2024, 1, 1), Duration = "5" },
            new TaskData { TaskId = 2, TaskName = "Design Phase", StartDate = new DateTime(2024, 1, 8), Duration = "3" },
            new TaskData { TaskId = 3, TaskName = "Development", StartDate = new DateTime(2024, 1, 15), Duration = "10" }
        };
    }
    
    private List<ResourceData> GetResources()
    {
        return new List<ResourceData>
        {
            new ResourceData { ResourceId = 1, ResourceName = "John Doe", MaxUnits = 100 },
            new ResourceData { ResourceId = 2, ResourceName = "Jane Smith", MaxUnits = 100 },
            new ResourceData { ResourceId = 3, ResourceName = "Bob Johnson", MaxUnits = 100 }
        };
    }
    
    private List<ResourceAssignment> GetAssignments()
    {
        return new List<ResourceAssignment>
        {
            // Task 1: Assigned to John (100%) and Jane (50%)
            new ResourceAssignment { PrimaryId = 1, TaskId = 1, ResourceId = 1, Unit = 100 },
            new ResourceAssignment { PrimaryId = 2, TaskId = 1, ResourceId = 2, Unit = 50 },
            
            // Task 2: Assigned to Jane (100%)
            new ResourceAssignment { PrimaryId = 3, TaskId = 2, ResourceId = 2, Unit = 100 },
            
            // Task 3: Assigned to John (50%) and Bob (100%)
            new ResourceAssignment { PrimaryId = 4, TaskId = 3, ResourceId = 1, Unit = 50 },
            new ResourceAssignment { PrimaryId = 5, TaskId = 3, ResourceId = 3, Unit = 100 }
        };
    }
}
```

---

## Resource Assignment Methods

### AddResourceAssignmentAsync<TAssignment>(TAssignment)

Adds a new resource assignment to a task programmatically.

**Parameters:**
- `resourceAssignment` (`TAssignment`) — New assignment with TaskID, ResourceID, and Units

**Returns:** `Task`

**Example:**
```csharp
<button @onclick="AddAssignment">Add Resource</button>

<SfGantt @ref="GanttInstance" DataSource="@TaskCollection">
    <GanttAssignmentFields DataSource="@AssignmentCollection"
                           PrimaryKey="PrimaryId"
                           TaskID="TaskId"
                           ResourceID="ResourceId"
                           Units="Unit"
                           TValue="TaskData"
                           TAssignment="ResourceAssignment">
    </GanttAssignmentFields>
</SfGantt>

@code {
    private async Task AddAssignment()
    {
        var newAssignment = new ResourceAssignment
        {
            PrimaryId = AssignmentCollection.Max(a => a.PrimaryId) + 1,
            TaskId = 2,        // Assign to task 2
            ResourceId = 3,    // Assign resource 3 (Bob)
            Unit = 75          // 75% allocation
        };
        
        await GanttInstance.AddResourceAssignmentAsync(newAssignment);
    }
}
```

---

### UpdateResourceAssignmentAsync<TAssignment>(TAssignment)

Updates an existing resource assignment based on its primary key.

**Parameters:**
- `resourceAssignment` (`TAssignment`) — Updated assignment data with PrimaryKey

**Returns:** `Task`

**Example:**
```csharp
private async Task UpdateAssignment()
{
    var existingAssignment = AssignmentCollection.First(a => a.PrimaryId == 3);
    existingAssignment.Unit = 80;  // Change from 100% to 80%
    
    await GanttInstance.UpdateResourceAssignmentAsync(existingAssignment);
}
```

---

### DeleteResourceAssignmentAsync<TAssignment>(TAssignment)

Removes a resource assignment from a task.

**Parameters:**
- `resourceAssignment` (`TAssignment`) — Assignment to remove (must include TaskID and ResourceID)

**Returns:** `Task`

**Example:**
```csharp
private async Task RemoveAssignment()
{
    var assignmentToRemove = new ResourceAssignment
    {
        TaskId = 2,
        ResourceId = 2  // Remove Jane from task 2
    };
    
    await GanttInstance.DeleteResourceAssignmentAsync(assignmentToRemove);
}
```

---

## Foreign Key Relationships

### Relationship Diagram

```
TaskCollection (Tasks)              ResourceCollection (Resources)
    TaskId (PK) ←─────┐         ┌─────→ ResourceId (PK)
    TaskName          │         │       ResourceName
    StartDate         │         │       MaxUnits
                      │         │
                      │         │
        AssignmentCollection (Junction Table)
            PrimaryId (PK)
            TaskId (FK) ──────────┘
            ResourceId (FK) ───────┘
            Unit (allocation %)
```

### Key Rules

1. **PrimaryKey** must be unique in assignment collection
2. **TaskID** must match an existing task's `GanttTaskFields.Id`
3. **ResourceID** must match an existing resource's `GanttResource.Id`
4. **One task can have multiple assignments** (multiple resources)
5. **One resource can be assigned to multiple tasks**

---

## CRUD Operations

### Adding Tasks with Resources

```csharp
private async Task AddTaskWithResources()
{
    // 1. Add the task
    var newTask = new TaskData
    {
        TaskId = 10,
        TaskName = "New Feature",
        StartDate = DateTime.Now,
        Duration = "5"
    };
    await GanttInstance.AddRecordAsync(newTask);
    
    // 2. Add resource assignments
    var assignment1 = new ResourceAssignment
    {
        PrimaryId = GetNextAssignmentId(),
        TaskId = 10,
        ResourceId = 1,
        Unit = 100
    };
    
    var assignment2 = new ResourceAssignment
    {
        PrimaryId = GetNextAssignmentId(),
        TaskId = 10,
        ResourceId = 2,
        Unit = 50
    };
    
    await GanttInstance.AddResourceAssignmentAsync(assignment1);
    await GanttInstance.AddResourceAssignmentAsync(assignment2);
}
```

### Reassigning Resources

```csharp
private async Task ReassignResource()
{
    // Remove old assignment
    var oldAssignment = AssignmentCollection
        .First(a => a.TaskId == 5 && a.ResourceId == 1);
    await GanttInstance.DeleteResourceAssignmentAsync(oldAssignment);
    
    // Add new assignment
    var newAssignment = new ResourceAssignment
    {
        PrimaryId = GetNextAssignmentId(),
        TaskId = 5,
        ResourceId = 3,  // Different resource
        Unit = 100
    };
    await GanttInstance.AddResourceAssignmentAsync(newAssignment);
}
```

---

## Events

### ResourceAssignmentChanging Event

Tracks all changes to resource assignments in a single event.

```csharp
<GanttAssignmentFields ResourceAssignmentChanging="@OnAssignmentChange" ...>
</GanttAssignmentFields>

@code {
    private void OnAssignmentChange(ResourceAssignmentChangeEventArgs<ResourceAssignment> args)
    {
        if (args.AddedRecords?.Count > 0)
        {
            foreach (var added in args.AddedRecords)
            {
                Console.WriteLine($"Added: Task {added.TaskId} → Resource {added.ResourceId} ({added.Unit}%)");
            }
        }
        
        if (args.ChangedRecords?.Count > 0)
        {
            foreach (var changed in args.ChangedRecords)
            {
                Console.WriteLine($"Changed: Assignment {changed.PrimaryId} → Unit {changed.Unit}%");
            }
        }
        
        if (args.DeletedRecords?.Count > 0)
        {
            foreach (var deleted in args.DeletedRecords)
            {
                Console.WriteLine($"Deleted: Task {deleted.TaskId} ← Resource {deleted.ResourceId}");
            }
        }
    }
}
```

---

## Common Patterns

### Pattern 1: Multiple Resources Per Task

```csharp
// Assign 3 resources to a single task with different allocation levels
var assignments = new List<ResourceAssignment>
{
    new ResourceAssignment { PrimaryId = 1, TaskId = 5, ResourceId = 1, Unit = 100 },  // Full-time
    new ResourceAssignment { PrimaryId = 2, TaskId = 5, ResourceId = 2, Unit = 50 },   // Half-time
    new ResourceAssignment { PrimaryId = 3, TaskId = 5, ResourceId = 3, Unit = 25 }    // Quarter-time
};

foreach (var assignment in assignments)
{
    await GanttInstance.AddResourceAssignmentAsync(assignment);
}
```

### Pattern 2: Resource View

Enable resource-centric view to group tasks by assigned resources:

```csharp
<SfGantt ViewType="ViewType.ResourceView" ...>
    <GanttAssignmentFields ...>
    </GanttAssignmentFields>
</SfGantt>
```

### Pattern 3: Checking Existing Assignments

```csharp
public List<ResourceAssignment> GetTaskAssignments(int taskId)
{
    return AssignmentCollection.Where(a => a.TaskId == taskId).ToList();
}

public List<ResourceAssignment> GetResourceAssignments(int resourceId)
{
    return AssignmentCollection.Where(a => a.ResourceId == resourceId).ToList();
}
```

---

## Troubleshooting

### Issue: Resources Not Displaying

**Cause:** Missing or incorrect foreign key mappings

**Solution:**
```csharp
// Verify all mappings are correct
<GanttAssignmentFields 
    DataSource="@AssignmentCollection"
    PrimaryKey="PrimaryId"           // ✓ Unique in assignment collection
    TaskID="TaskId"                  // ✓ Matches GanttTaskFields.Id
    ResourceID="ResourceId"          // ✓ Matches GanttResource.Id
    Units="Unit"
    TValue="TaskData"
    TAssignment="ResourceAssignment">
</GanttAssignmentFields>
```

### Issue: Assignment Methods Not Working

**Cause:** Missing component reference

**Solution:**
```csharp
<SfGantt @ref="GanttInstance" ...>  <!-- Add @ref -->
</SfGantt>

@code {
    private SfGantt<TaskData> GanttInstance;  // Declare reference
}
```

### Issue: Units Not Appearing

**Cause:** Units property not mapped or null

**Solution:**
```csharp
public class ResourceAssignment
{
    public double Unit { get; set; } = 100;  // Default value
}
```
