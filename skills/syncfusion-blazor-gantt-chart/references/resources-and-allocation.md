# Resources and Allocation

## Table of Contents
- [Overview](#overview)
- [When to Read](#when-to-read) - [Key Properties and Features](#key-properties-and-features) - [Basic Resource Model](#basic-resource-model) - [Configure Resources](#configure-resources) - [Resource Units](#resource-units) - [Assign Resources to Tasks](#assign-resources-to-tasks) - [Get Resource Information](#get-resource-information) - [Resource View](#resource-view) - [Over-Allocation Detection](#over-allocation-detection) - [Multi-Resource Tasks](#multi-resource-tasks) - [Display Resources](#display-resources) - [Work Calculation](#work-calculation) - [Best Practices](#best-practices) - [Common Scenarios](#common-scenarios) - [Troubleshooting](#troubleshooting) - [Limitations](#limitations)

---

## Overview

Resources in the Syncfusion Blazor Gantt Chart component represent people, equipment, or materials allocated to tasks. The component supports assigning resources to tasks, displaying resource names in taskbars and columns, managing resource units and allocation percentages, switching to resource view for workload analysis, and detecting over-allocation when resources exceed capacity.

## When to Read
- You need to assign resources (people, equipment) to tasks
- You want to display resource names in taskbars or columns
- You need to manage resource units and allocation percentages
- You require resource view for workload balancing
- You want to detect and visualize over-allocation
- You need to track work distribution across resources

## Key Properties and Features

### Resource Configuration
- `GanttResource` - Resource collection configuration component
- `GanttResource.DataSource` - List of available resources
- `GanttResource.Id` - Resource identifier field mapping
- `GanttResource.Name` - Resource name field mapping
- `GanttResource.MaxUnits` - Maximum capacity field mapping (0-100%)
- `GanttResource.Group` - Resource grouping field mapping

### Assignment Configuration
- `GanttAssignmentFields` - Resource assignment mapping component
- `GanttAssignmentFields.DataSource` - List of task-resource assignments
- `GanttAssignmentFields.PrimaryKey` - Unique assignment ID
- `GanttAssignmentFields.TaskID` - Links assignment to task
- `GanttAssignmentFields.ResourceID` - Links assignment to resource
- `GanttAssignmentFields.Units` - Allocation percentage per assignment

### Resource View
- `ViewType.ResourceView` - Organize tasks by resource instead of hierarchy
- `ShowOverallocation` - Highlight over-allocated resources

### Resource Methods
- `AddResourceAssignmentAsync(assignment)` - Add resource to task
- `DeleteResourceAssignmentAsync(assignment)` - Remove resource from task
- `UpdateResourceAssignmentAsync(assignment)` - Update resource allocation
- `GetResources(taskId)` - Get resources assigned to task
- `GetResourceAssignments(taskId)` - Get assignment details

### Display
- `GanttResourceColumn` - Special column for resource display
- `GanttLabelSettings.RightLabel="Resources"` - Show resources in taskbar labels

## Basic Resource Model

### Resource Data Model

```csharp
public class ResourceData
{
    public int ResourceId { get; set; }
    public string ResourceName { get; set; }
    public double MaxUnit { get; set; }  // Default: 100
    public string Group { get; set; }     // Optional: Team, Department, etc.
}
```

### Task Model with Resources

```csharp
public class TaskData
{
    public int TaskID { get; set; }
    public string TaskName { get; set; }
    public DateTime StartDate { get; set; }
    public DateTime? EndDate { get; set; }
    public string Duration { get; set; }
    public int Progress { get; set; }
    public int? ParentID { get; set; }
    public double? Work { get; set; }
    public string TaskType { get; set; }
}
```

### Assignment Model

```csharp
public class AssignmentModel
{
    public int PrimaryId { get; set; }       // Unique assignment ID
    public int TaskID { get; set; }          // Foreign key to task
    public int ResourceId { get; set; }      // Foreign key to resource
    public double? Unit { get; set; }        // Allocation percentage (0-100)
}
```

## Configure Resources

### Complete Setup Example

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt DataSource="@TaskCollection" 
         Height="450px" 
         Width="850px" 
         TreeColumnIndex="1"
         WorkUnit="WorkUnit.Hour">
    <GanttTaskFields Id="@nameof(TaskData.TaskID)" 
                     Name="@nameof(TaskData.TaskName)" 
                     StartDate="@nameof(TaskData.StartDate)" 
                     EndDate="@nameof(TaskData.EndDate)" 
                     Duration="@nameof(TaskData.Duration)" 
                     Progress="@nameof(TaskData.Progress)"
                     ParentID="@nameof(TaskData.ParentID)" 
                     Work="@nameof(TaskData.Work)" 
                     TaskType="@nameof(TaskData.TaskType)">
    </GanttTaskFields>
    
    <!-- Resource Collection -->
    <GanttResource DataSource="@ResourceCollection" 
                   Id="@nameof(ResourceData.ResourceId)" 
                   Name="@nameof(ResourceData.ResourceName)" 
                   MaxUnits="@nameof(ResourceData.MaxUnit)" 
                   TValue="TaskData" 
                   TResources="ResourceData">
    </GanttResource>
    
    <!-- Assignment Collection -->
    <GanttAssignmentFields DataSource="@AssignmentCollection" 
                           PrimaryKey="@nameof(AssignmentModel.PrimaryId)" 
                           TaskID="@nameof(AssignmentModel.TaskID)" 
                           ResourceID="@nameof(AssignmentModel.ResourceId)" 
                           Units="@nameof(AssignmentModel.Unit)" 
                           TValue="TaskData" 
                           TAssignment="AssignmentModel">
    </GanttAssignmentFields>
    
    <!-- Display Resources in Label -->
    <GanttLabelSettings RightLabel="Resources" TValue="TaskData" />
    
    <GanttEditSettings AllowAdding="true" 
                       AllowEditing="true" 
                       AllowDeleting="true" 
                       AllowTaskbarEditing="true">
    </GanttEditSettings>
    
    <GanttColumns>
        <GanttColumn Field="@nameof(TaskData.TaskID)" HeaderText="ID" />
        <GanttColumn Field="@nameof(TaskData.TaskName)" HeaderText="Task Name" Width="250px" />
        <GanttResourceColumn HeaderText="Resources" Width="300px" />
        <GanttColumn Field="@nameof(TaskData.Work)" HeaderText="Work" />
        <GanttColumn Field="@nameof(TaskData.Duration)" HeaderText="Duration" />
        <GanttColumn Field="@nameof(TaskData.TaskType)" HeaderText="Task Type" />
    </GanttColumns>
</SfGantt>

@code {
    private List<TaskData> TaskCollection { get; set; }
    private List<ResourceData> ResourceCollection { get; set; }
    private List<AssignmentModel> AssignmentCollection { get; set; }

    protected override void OnInitialized()
    {
        TaskCollection = GetTasks();
        ResourceCollection = GetResources();
        AssignmentCollection = GetAssignments();
    }

    public static List<ResourceData> GetResources()
    {
        return new List<ResourceData>()
        {
            new ResourceData() { ResourceId = 1, ResourceName = "Martin Tamer", MaxUnit = 70 },
            new ResourceData() { ResourceId = 2, ResourceName = "Rose Fuller", MaxUnit = 100 },
            new ResourceData() { ResourceId = 3, ResourceName = "Margaret Buchanan", MaxUnit = 100 },
            new ResourceData() { ResourceId = 4, ResourceName = "Fuller King", MaxUnit = 100 },
            new ResourceData() { ResourceId = 5, ResourceName = "Davolio Fuller", MaxUnit = 100 },
            new ResourceData() { ResourceId = 6, ResourceName = "Van Jack", MaxUnit = 100 }
        };
    }

    public static List<AssignmentModel> GetAssignments()
    {
        return new List<AssignmentModel>()
        {
            new AssignmentModel() { PrimaryId = 1, TaskID = 2, ResourceId = 1, Unit = 70 },
            new AssignmentModel() { PrimaryId = 2, TaskID = 2, ResourceId = 6, Unit = 100 },
            new AssignmentModel() { PrimaryId = 3, TaskID = 3, ResourceId = 2, Unit = 100 },
            new AssignmentModel() { PrimaryId = 4, TaskID = 3, ResourceId = 3, Unit = 100 },
            new AssignmentModel() { PrimaryId = 5, TaskID = 4, ResourceId = 4, Unit = 100 }
        };
    }

    public static List<TaskData> GetTasks()
    {
        return new List<TaskData>()
        {
            new TaskData() { TaskID = 1, TaskName = "Project initiation", 
                            StartDate = new DateTime(2021, 03, 28), Duration = "4", 
                            TaskType = "FixedDuration", Work = 128 },
            new TaskData() { TaskID = 2, TaskName = "Identify site location", 
                            StartDate = new DateTime(2021, 03, 29), Duration = "2", 
                            Progress = 30, ParentID = 1, TaskType = "FixedDuration", Work = 16 },
            new TaskData() { TaskID = 3, TaskName = "Perform soil test", 
                            StartDate = new DateTime(2021, 03, 29), Duration = "4", 
                            ParentID = 1, TaskType = "FixedWork", Work = 96 },
            new TaskData() { TaskID = 4, TaskName = "Soil test approval", 
                            StartDate = new DateTime(2021, 03, 29), Duration = "1", 
                            Progress = 30, ParentID = 1, TaskType = "FixedWork", Work = 16 }
        };
    }
}
```

## Resource Units

Resource units indicate allocation percentage or capacity:

### MaxUnits (Resource Capacity)
Maximum available capacity per resource:

```csharp
new ResourceData() { ResourceId = 1, ResourceName = "John", MaxUnit = 70 }  // 70% capacity
new ResourceData() { ResourceId = 2, ResourceName = "Jane", MaxUnit = 100 } // 100% capacity (default)
```

**Use cases:**
- Part-time resources: `MaxUnit = 50` (4 hours/day)
- Full-time resources: `MaxUnit = 100` (8 hours/day)
- Overtime capacity: `MaxUnit = 125` (10 hours/day)

### Units (Task Allocation)
Allocation percentage per task assignment:

```csharp
new AssignmentModel() { TaskID = 2, ResourceId = 1, Unit = 70 }  // 70% of time on this task
new AssignmentModel() { TaskID = 3, ResourceId = 1, Unit = 30 }  // 30% of time on this task
```

**Default**: If not specified, defaults to 100

## Assign Resources to Tasks

### Method 1: Through Cell Editing

Use `GanttResourceColumn` for inline resource editing:

```razor
<GanttColumns>
    <GanttColumn Field="TaskName" HeaderText="Task Name" Width="250px" />
    <GanttResourceColumn HeaderText="Resources" Width="300px" />
</GanttColumns>

<GanttEditSettings AllowEditing="true" Mode="EditMode.Auto" />
```

**User interaction:**
1. Double-click resource cell
2. Select resources from dropdown (multi-select)
3. Press Enter to save

### Method 2: Through Dialog

Edit resources in the edit dialog's Resource tab:

```razor
<GanttEditSettings AllowEditing="true" Mode="EditMode.Dialog" />
```

**Features:**
- Checkbox selection for multiple resources
- Edit unit allocation per resource
- Add/remove resources easily

### Method 3: Programmatically

```csharp
// Add resource assignment
public async Task AssignResource()
{
    var assignment = new AssignmentModel()
    {
        PrimaryId = 15,
        TaskID = 8,
        ResourceId = 4,
        Unit = 75
    };
    
    await GanttInstance.AddResourceAssignmentAsync(assignment);
}

// Update resource allocation
public async Task UpdateResourceUnit()
{
    var assignment = new AssignmentModel()
    {
        PrimaryId = 5,
        TaskID = 3,
        ResourceId = 2,
        Unit = 50  // Changed from 100 to 50
    };
    
    await GanttInstance.UpdateResourceAssignmentAsync(assignment);
}

// Remove resource assignment
public async Task RemoveResource()
{
    var assignment = new AssignmentModel()
    {
        PrimaryId = 3,
        TaskID = 3,
        ResourceId = 2
    };
    
    await GanttInstance.DeleteResourceAssignmentAsync(assignment);
}
```

### Method 4: Add Task with Resources

```csharp
public async Task AddTaskWithResources()
{
    var newTask = new TaskData()
    {
        TaskID = 10,
        TaskName = "New Task",
        StartDate = DateTime.Now,
        Duration = "3",
        Work = 24,
        TaskType = "FixedWork"
    };
    
    // Fourth parameter specifies resource assignments
    var resources = new List<AssignmentModel>()
    {
        new AssignmentModel() { TaskID = 10, ResourceId = 1, Unit = 100 },
        new AssignmentModel() { TaskID = 10, ResourceId = 2, Unit = 50 }
    };
    
    await GanttInstance.AddRecordAsync(newTask, null, RowPosition.Bottom, resources);
}
```

## Get Resource Information

### Get Resources for Task

```csharp
public async Task GetTaskResources(int taskId)
{
    var resources = await GanttInstance.GetResources(taskId);
    
    foreach (var resource in resources)
    {
        Console.WriteLine($"Resource: {resource.ResourceName}, Unit: {resource.MaxUnit}");
    }
}
```

### Get Resource Assignments

```csharp
public async Task GetTaskAssignments(int taskId)
{
    var assignments = await GanttInstance.GetResourceAssignments(taskId);
    
    foreach (var assignment in assignments)
    {
        Console.WriteLine($"Assignment ID: {assignment.PrimaryId}, " +
                         $"Resource: {assignment.ResourceId}, " +
                         $"Unit: {assignment.Unit}");
    }
}
```

## Resource View

Switch to resource-centric view where resources are parent nodes and tasks are children:

```razor
<SfGantt DataSource="@TaskCollection" 
         ViewType="ViewType.ResourceView"
         Height="450px" 
         Width="850px">
    <GanttResource DataSource="@ResourceCollection" 
                   Id="ResourceId" 
                   Name="ResourceName" 
                   MaxUnits="MaxUnit" />
    <GanttAssignmentFields DataSource="@AssignmentCollection" 
                           TaskID="TaskID" 
                           ResourceID="ResourceId" 
                           Units="Unit" />
</SfGantt>
```

**Structure:**
```
└─ Martin Tamer
   ├─ Task 1 (70% allocated)
   └─ Task 2 (30% allocated)
└─ Rose Fuller
   ├─ Task 3 (100% allocated)
   └─ Task 4 (50% allocated)
└─ Unassigned Tasks
   └─ Task 5 (no resources)
```

**When to use:**
- Workload balancing and capacity planning
- Resource utilization analysis
- Identifying over-allocated resources
- Reviewing resource assignments across projects

**Limitations:**
- Parent tasks not supported
- Indent/Outdent not available
- Delete moves task to "Unassigned Tasks" first

## Over-Allocation Detection

Enable over-allocation indicators in Resource View:

```razor
<SfGantt DataSource="@TaskCollection" 
         ViewType="ViewType.ResourceView"
         ShowOverallocation="true">
</SfGantt>
```

**How over-allocation is calculated:**
1. Calculate daily capacity: `DayWorkingHours × MaxUnits / 100`
2. Sum work from all tasks assigned to resource each day
3. If sum exceeds capacity, mark as over-allocated

**Visual indicators:**
- Over-allocated date ranges highlighted with square brackets `[===]`
- Default color: Red/Orange background

**Example:**
- Resource: John (MaxUnit = 100, 8 hours/day capacity)
- Task A: 5 hours on March 1
- Task B: 4 hours on March 1
- **Result**: 9 hours > 8 hours = Over-allocated on March 1

### Resolve Over-Allocation

**Strategy 1: Reduce Units**
```csharp
// Reduce allocation from 100% to 50%
await GanttInstance.UpdateResourceAssignmentAsync(new AssignmentModel()
{
    PrimaryId = 5,
    TaskID = 3,
    ResourceId = 1,
    Unit = 50  // Was 100
});
```

**Strategy 2: Reassign Task**
```csharp
// Move task to different resource
await GanttInstance.DeleteResourceAssignmentAsync(oldAssignment);
await GanttInstance.AddResourceAssignmentAsync(new AssignmentModel()
{
    TaskID = 3,
    ResourceId = 4  // Different resource
});
```

**Strategy 3: Adjust Dates**
```csharp
// Shift task to different date
var updatedTask = await GanttInstance.GetRecordByID(3);
updatedTask.StartDate = updatedTask.StartDate.AddDays(2);
await GanttInstance.UpdateRecordByIDAsync(3, updatedTask);
```

## Multi-Resource Tasks

Assign multiple resources to a single task:

```csharp
var assignments = new List<AssignmentModel>()
{
    new AssignmentModel() { PrimaryId = 10, TaskID = 5, ResourceId = 1, Unit = 50 },  // John 50%
    new AssignmentModel() { PrimaryId = 11, TaskID = 5, ResourceId = 2, Unit = 50 },  // Jane 50%
    new AssignmentModel() { PrimaryId = 12, TaskID = 5, ResourceId = 3, Unit = 25 }   // Bob 25%
};
```

**Display in taskbar:**
```
Task Name [John 50%, Jane 50%, Bob 25%]
```

## Display Resources

### In Taskbar Labels

```razor
<GanttLabelSettings LeftLabel="TaskName" 
                    RightLabel="Resources" 
                    TValue="TaskData" />
```

### In Custom Column

```razor
<GanttColumns>
    <GanttColumn Field="TaskName" HeaderText="Task" />
    <GanttResourceColumn HeaderText="Assigned Resources" Width="300px" />
    <GanttColumn Field="Duration" HeaderText="Duration" />
</GanttColumns>
```

### In Template

```razor
<GanttTemplates TValue="TaskData">
    <TaskbarTemplate>
        @{
            var task = context as TaskData;
            var resources = await GanttInstance.GetResources(task.TaskID);
            <div>
                @task.TaskName
                <br/>
                <small>@string.Join(", ", resources.Select(r => r.ResourceName))</small>
            </div>
        }
    </TaskbarTemplate>
</GanttTemplates>
```

## Work Calculation

When resources are assigned, work is calculated based on:
- Task duration
- Resource units
- Working hours per day

**Formula:**
```
Work = Duration × WorkingHoursPerDay × (Unit / 100)
```

**Example:**
- Duration: 5 days
- Working hours: 8 hours/day
- Unit: 50%
- **Work** = 5 × 8 × 0.5 = **20 hours**

### Task Types

Control how Duration, Work, and Units interact:

#### FixedDuration
```csharp
new TaskData() { TaskType = "FixedDuration", Duration = "5", Work = null }
// Duration fixed, Work calculated from Units
```

#### FixedWork
```csharp
new TaskData() { TaskType = "FixedWork", Duration = null, Work = 40 }
// Work fixed, Duration calculated from Units
```

#### FixedUnit
```csharp
new TaskData() { TaskType = "FixedUnit", Duration = "5", Work = 40 }
// Units fixed, Duration/Work adjusted together
```

## Best Practices

### Resource IDs
- Use stable, unique identifiers
- Avoid changing resource IDs after assignment
- Use integer or GUID types for reliability

### Data Structure
- Keep resource collection separate from task collection
- Use assignment collection for many-to-many relationships
- Normalize resource data (don't duplicate in multiple places)

### Allocation Management
- Default to 100% units unless partial allocation needed
- Sum units across all tasks shouldn't exceed MaxUnits for same time period
- Monitor over-allocation regularly in Resource View

### UI/UX
- Use Resource View for staffing analysis, not as default view
- Show resource column in Project View for quick reference
- Filter Resource View by team/group for large organizations
- Provide clear visual indicators for over-allocation

### Performance
- Limit resource collection size for large projects
- Use filtering in Resource View for better performance
- Consider pagination for projects with many resources

## Common Scenarios

### Scenario 1: Part-Time Resource

```csharp
// Developer works 4 hours/day (50%)
new ResourceData() { ResourceId = 10, ResourceName = "Part-Time Dev", MaxUnit = 50 }

// Assign to task at 50% (full capacity for this resource)
new AssignmentModel() { TaskID = 5, ResourceId = 10, Unit = 50 }
```

### Scenario 2: Shared Resource Across Teams

```csharp
// Database Admin shared 30% to Project A, 70% to Project B
new AssignmentModel() { TaskID = 10, ResourceId = 5, Unit = 30 }  // Project A task
new AssignmentModel() { TaskID = 25, ResourceId = 5, Unit = 70 }  // Project B task
```

### Scenario 3: Team Assignment

```csharp
// Assign entire team to high-priority task
var team = new[] { 1, 2, 3, 4, 5 };  // Resource IDs
foreach (var resourceId in team)
{
    await GanttInstance.AddResourceAssignmentAsync(new AssignmentModel()
    {
        TaskID = 100,
        ResourceId = resourceId,
        Unit = 100
    });
}
```

### Scenario 4: Rebalance Workload

```csharp
// Move half of John's allocation to Jane
public async Task RebalanceWorkload(int taskId, int fromResourceId, int toResourceId)
{
    // Get current assignments
    var assignments = await GanttInstance.GetResourceAssignments(taskId);
    var johnAssignment = assignments.First(a => a.ResourceId == fromResourceId);
    
    // Reduce John's allocation
    johnAssignment.Unit = 50;
    await GanttInstance.UpdateResourceAssignmentAsync(johnAssignment);
    
    // Add Jane with remaining capacity
    await GanttInstance.AddResourceAssignmentAsync(new AssignmentModel()
    {
        TaskID = taskId,
        ResourceId = toResourceId,
        Unit = 50
    });
}
```

## Troubleshooting

**Issue**: Resource names not displaying  
**Solution**: 
- Verify `GanttResource` mappings (Id, Name fields)
- Check `GanttAssignmentFields` TaskID and ResourceID mappings
- Ensure assignment collection links valid TaskID and ResourceId

**Issue**: Resource column shows empty or incorrect data  
**Solution**:
- Use `GanttResourceColumn` component, not regular `GanttColumn`
- Verify foreign key relationships in assignment collection
- Check that resource IDs in assignments exist in resource collection

**Issue**: Over-allocation not showing  
**Solution**:
- Enable `ShowOverallocation="true"`
- Switch to `ViewType="ViewType.ResourceView"`
- Verify `MaxUnits` and working time configuration
- Check that `WorkUnit` is set (e.g., `WorkUnit="WorkUnit.Hour"`)

**Issue**: Resource view feels cluttered  
**Solution**:
- Filter to specific team or role
- Collapse unused resource nodes
- Reduce visible columns
- Use resource grouping for better organization

**Issue**: Can't edit resources in cell  
**Solution**:
- Verify `AllowEditing="true"` in GanttEditSettings
- Use `GanttResourceColumn`, not regular column
- Ensure Mode="EditMode.Auto" or Mode="EditMode.Dialog"

**Issue**: Work not calculating correctly  
**Solution**:
- Set `WorkUnit="WorkUnit.Hour"` or appropriate unit
- Verify `TaskType` (FixedWork, FixedDuration, FixedUnit)
- Check that `Work` field is mapped in GanttTaskFields
- Ensure `DayWorkingTime` is configured correctly

## Limitations

- **Parent tasks**: Cannot assign resources to parent/summary tasks
- **Resource view hierarchy**: No support for Indent/Outdent in Resource View
- **Delete behavior**: In Resource View, first delete moves to "Unassigned Tasks"
- **Narrow screens**: Resource names may truncate with multiple assignments
- **Performance**: Large resource collections (1000+) may impact rendering
- **Real-time updates**: Resource capacity doesn't update dynamically without refresh

