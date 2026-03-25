# Resources and Grouping

## Table of Contents
- [Overview](#overview)
- [ScheduleResource Component](#scheduleresource-component)
- [Resource Fields Reference](#resource-fields-reference)
- [Resource Data Binding](#resource-data-binding)
- [Binding with ExpandoObject](#binding-with-expandoobject)
- [Binding with DynamicObject](#binding-with-dynamicobject)
- [Binding with ObservableCollection](#binding-with-observablecollection)
- [Multiple Resources Without Grouping](#multiple-resources-without-grouping)
- [Single-Level Resource Grouping](#single-level-resource-grouping)
- [Multi-Level Resource Grouping](#multi-level-resource-grouping)
- [One-to-One Resource Grouping](#one-to-one-resource-grouping)
- [Grouping Resources by Date](#grouping-resources-by-date)
- [Shared Events Across Resources](#shared-events-across-resources)
- [Resource Header Customization](#resource-header-customization)
- [Multiple Column Resource Headers](#multiple-column-resource-headers)
- [Expand and Collapse Resources](#expand-and-collapse-resources)
- [Resource Header Tooltips](#resource-header-tooltips)
- [Resource Color Selection](#resource-color-selection)
- [Custom Working Hours by Resource](#custom-working-hours-by-resource)
- [Custom Working Days by Resource](#custom-working-days-by-resource)

## Overview

Resources enable the Scheduler to display appointments for multiple entities (rooms, employees, equipment, etc.) with individual columns or rows for each resource. The Scheduler supports single and multiple levels of resource grouping, allowing flexible organization of appointments. Resources can be displayed in vertical (calendar views) or timeline views, with extensive customization options for headers, colors, working hours, and days.

**Key Concepts:**
- Resources are entities assigned to appointments (e.g., Conference Room A, Nancy, Equipment X)
- Each resource has properties: ID, Name, Color, Working Hours, Working Days, CSS Class
- Resources can be grouped hierarchically (parent-child relationships)
- Single appointment can be assigned to multiple resources
- Shared events allow editing one instance to update all resource copies
- Resources can have different working schedules

## ScheduleResource Component

The `ScheduleResource` component defines available resources and maps resource data fields. It uses generics for strong typing.

**ScheduleResource Generic Parameters:**
- `TItem` - Resource data model class
- `TValue` - Resource ID type (int, string, Guid, etc.)

**Basic Structure:**

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int"
            DataSource="@ResourceList"
            Field="ResourceId"
            Title="Resource"
            Name="Resources"
            TextField="Name"
            IdField="Id"
            ColorField="Color">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<ResourceData> ResourceList = new List<ResourceData>
    {
        new ResourceData { Id = 1, Name = "Room A", Color = "#FF6B6B" },
        new ResourceData { Id = 2, Name = "Room B", Color = "#4ECDC4" }
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Meeting",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0),
            ResourceId = 1
        }
    };

    public class ResourceData
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int ResourceId { get; set; }
    }
}
```

## Resource Fields Reference

ScheduleResource properties map appointment/resource data fields:

| Property | Type | Description |
|----------|------|-------------|
| **Field** | string | Appointment field name storing resource ID (e.g., "ResourceId") |
| **Title** | string | Display name in event editor (e.g., "Select Room") |
| **Name** | string | Unique resource name for grouping identification (e.g., "Rooms") |
| **TextField** | string | Resource data field for display name (e.g., "Name") |
| **IdField** | string | Resource data field for ID (e.g., "Id") |
| **ColorField** | string | Resource data field for color (e.g., "Color") |
| **AllowMultiple** | bool | Allow appointment assignment to multiple resources (default: false) |
| **DataSource** | object | Resource data collection (List, ExpandoObject, DynamicObject) |
| **Query** | Query | OData query for remote data |
| **ExpandedField** | string | Resource data field indicating initial expand state (bool) |
| **GroupIDField** | string | Parent resource ID for hierarchical grouping (multi-level) |
| **StartHourField** | string | Resource data field for work start hour (e.g., "StartHour") |
| **EndHourField** | string | Resource data field for work end hour (e.g., "EndHour") |
| **WorkDaysField** | string | Resource data field for work days (e.g., "WorkDays") |
| **CssClassField** | string | Resource data field for custom CSS class (e.g., "CssClass") |

## Resource Data Binding

Bind resource data from local List collections:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup Resources="@Resources"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="RoomData" TValue="int"
            DataSource="@RoomsList"
            Field="RoomId"
            Title="Room"
            Name="Rooms"
            TextField="RoomName"
            IdField="Id"
            ColorField="RoomColor">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] Resources { get; set; } = { "Rooms" };

    List<RoomData> RoomsList = new List<RoomData>
    {
        new RoomData { Id = 1, RoomName = "Conference Room A", RoomColor = "#ff9800" },
        new RoomData { Id = 2, RoomName = "Conference Room B", RoomColor = "#2196f3" },
        new RoomData { Id = 3, RoomName = "Meeting Room C", RoomColor = "#4caf50" }
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Team Standup",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 9, 30, 0),
            RoomId = 1
        },
        new AppointmentData
        {
            Id = 2,
            Subject = "Client Meeting",
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0),
            RoomId = 2
        }
    };

    public class RoomData
    {
        public int Id { get; set; }
        public string RoomName { get; set; }
        public string RoomColor { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int RoomId { get; set; }
    }
}
```

## Binding with ExpandoObject

Use ExpandoObject when resource model type is unknown at compile time:

```cshtml
@using Syncfusion.Blazor.Schedule
@using System.Dynamic

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup Resources="@Resources"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ExpandoObject" TValue="int"
            DataSource="@ExpandoResourceList"
            Field="ResourceId"
            Title="Resource"
            Name="Resources"
            TextField="Name"
            IdField="Id"
            ColorField="Color">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] Resources { get; set; } = { "Resources" };

    List<ExpandoObject> ExpandoResourceList = new List<ExpandoObject>
    {
        CreateExpandoResource(1, "Resource A", "#FF6B6B"),
        CreateExpandoResource(2, "Resource B", "#4ECDC4"),
        CreateExpandoResource(3, "Resource C", "#95E1D3")
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Task 1",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0),
            ResourceId = 1
        }
    };

    private ExpandoObject CreateExpandoResource(int id, string name, string color)
    {
        dynamic resource = new ExpandoObject();
        resource.Id = id;
        resource.Name = name;
        resource.Color = color;
        return (ExpandoObject)resource;
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int ResourceId { get; set; }
    }
}
```

## Binding with DynamicObject

Use DynamicObject for dynamic resource properties with custom behavior:

```cshtml
@using Syncfusion.Blazor.Schedule
@using System.Dynamic

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup Resources="@Resources"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="DynamicResourceData" TValue="int"
            DataSource="@DynamicResourceList"
            Field="ResourceId"
            Title="Resource"
            Name="Resources"
            TextField="Name"
            IdField="Id"
            ColorField="Color">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] Resources { get; set; } = { "Resources" };

    List<DynamicResourceData> DynamicResourceList = new List<DynamicResourceData>
    {
        new DynamicResourceData { Id = 1, Name = "Resource A", Color = "#FF6B6B" },
        new DynamicResourceData { Id = 2, Name = "Resource B", Color = "#4ECDC4" }
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Meeting",
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0),
            ResourceId = 1
        }
    };

    public class DynamicResourceData : DynamicObject
    {
        private Dictionary<string, object> properties = new Dictionary<string, object>();

        public int Id
        {
            get { return (int)properties["Id"]; }
            set { properties["Id"] = value; }
        }

        public string Name
        {
            get { return (string)properties["Name"]; }
            set { properties["Name"] = value; }
        }

        public string Color
        {
            get { return (string)properties["Color"]; }
            set { properties["Color"] = value; }
        }

        public override IEnumerable<string> GetDynamicMemberNames()
        {
            return properties.Keys;
        }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int ResourceId { get; set; }
    }
}
```

## Binding with ObservableCollection

Use ObservableCollection for dynamic resource additions/removals with notifications:

```cshtml
@using Syncfusion.Blazor.Schedule
@using System.Collections.ObjectModel
@using System.ComponentModel
@using Syncfusion.Blazor.Buttons

<SfButton @onclick="AddResource">Add Resource</SfButton>
<SfButton @onclick="RemoveResource">Remove Resource</SfButton>

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup Resources="@Resources"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int"
            DataSource="@DynamicResourceList"
            Field="ResourceId"
            Title="Resource"
            Name="Resources"
            TextField="Name"
            IdField="Id"
            ColorField="Color">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] Resources { get; set; } = { "Resources" };
    int resourceCount = 3;

    ObservableCollection<ResourceData> DynamicResourceList = new ObservableCollection<ResourceData>
    {
        new ResourceData { Id = 1, Name = "Resource A", Color = "#FF6B6B" },
        new ResourceData { Id = 2, Name = "Resource B", Color = "#4ECDC4" },
        new ResourceData { Id = 3, Name = "Resource C", Color = "#95E1D3" }
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Meeting",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0),
            ResourceId = 1
        }
    };

    private void AddResource()
    {
        resourceCount++;
        DynamicResourceList.Add(new ResourceData 
        { 
            Id = resourceCount, 
            Name = $"Resource {(char)(64 + (resourceCount % 26))}", 
            Color = "#FFE66D" 
        });
    }

    private void RemoveResource()
    {
        if (DynamicResourceList.Count > 1)
            DynamicResourceList.RemoveAt(DynamicResourceList.Count - 1);
    }

    public class ResourceData : INotifyPropertyChanged
    {
        private int id;
        private string name;
        private string color;

        public int Id
        {
            get { return id; }
            set { id = value; OnPropertyChanged(nameof(Id)); }
        }

        public string Name
        {
            get { return name; }
            set { name = value; OnPropertyChanged(nameof(Name)); }
        }

        public string Color
        {
            get { return color; }
            set { color = value; OnPropertyChanged(nameof(Color)); }
        }

        public event PropertyChangedEventHandler PropertyChanged;

        private void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int ResourceId { get; set; }
    }
}
```

## Multiple Resources Without Grouping

Display multiple resource options without visual grouping:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int"
            DataSource="@ResourceList"
            Field="ResourceId"
            Title="Select Room"
            Name="Rooms"
            TextField="Name"
            IdField="Id"
            ColorField="Color"
            AllowMultiple="true">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<ResourceData> ResourceList = new List<ResourceData>
    {
        new ResourceData { Id = 1, Name = "Room A", Color = "#FF6B6B" },
        new ResourceData { Id = 2, Name = "Room B", Color = "#4ECDC4" },
        new ResourceData { Id = 3, Name = "Room C", Color = "#95E1D3" }
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Team Meeting",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0),
            ResourceId = 1
        }
    };

    public class ResourceData
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int ResourceId { get; set; }
    }
}
```

## Single-Level Resource Grouping

Group appointments by single resource type:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup Resources="@Resources"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int"
            DataSource="@ResourceList"
            Field="ResourceId"
            Title="Room"
            Name="Rooms"
            TextField="Name"
            IdField="Id"
            ColorField="Color">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.TimelineWeek"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] Resources { get; set; } = { "Rooms" };

    List<ResourceData> ResourceList = new List<ResourceData>
    {
        new ResourceData { Id = 1, Name = "Conference Room A", Color = "#FF9800" },
        new ResourceData { Id = 2, Name = "Conference Room B", Color = "#2196F3" },
        new ResourceData { Id = 3, Name = "Board Room", Color = "#4CAF50" }
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Team Standup",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 9, 30, 0),
            ResourceId = 1
        },
        new AppointmentData
        {
            Id = 2,
            Subject = "Management Review",
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0),
            ResourceId = 3
        }
    };

    public class ResourceData
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int ResourceId { get; set; }
    }
}
```

## Multi-Level Resource Grouping

Nest resources hierarchically using GroupIDField:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup Resources="@Resources"></ScheduleGroup>
    <ScheduleResources>
        <!-- Parent level: Buildings -->
        <ScheduleResource TItem="BuildingData" TValue="int"
            DataSource="@BuildingList"
            Field="BuildingId"
            Title="Building"
            Name="Buildings"
            TextField="Name"
            IdField="Id"
            ColorField="Color">
        </ScheduleResource>
        
        <!-- Child level: Rooms within Buildings -->
        <ScheduleResource TItem="RoomData" TValue="int"
            DataSource="@RoomList"
            Field="RoomId"
            Title="Room"
            Name="Rooms"
            TextField="Name"
            IdField="Id"
            GroupIDField="BuildingId"
            ColorField="Color">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.TimelineMonth"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] Resources { get; set; } = { "Buildings", "Rooms" };

    List<BuildingData> BuildingList = new List<BuildingData>
    {
        new BuildingData { Id = 1, Name = "North Building", Color = "#FF9800" },
        new BuildingData { Id = 2, Name = "South Building", Color = "#2196F3" }
    };

    List<RoomData> RoomList = new List<RoomData>
    {
        new RoomData { Id = 1, Name = "Room 101", BuildingId = 1, Color = "#FFB74D" },
        new RoomData { Id = 2, Name = "Room 102", BuildingId = 1, Color = "#FFCC80" },
        new RoomData { Id = 3, Name = "Room 201", BuildingId = 2, Color = "#64B5F6" },
        new RoomData { Id = 4, Name = "Room 202", BuildingId = 2, Color = "#90CAF9" }
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Team Meeting",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0),
            BuildingId = 1,
            RoomId = 1
        }
    };

    public class BuildingData
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Color { get; set; }
    }

    public class RoomData
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public int BuildingId { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int BuildingId { get; set; }
        public int RoomId { get; set; }
    }
}
```

## One-to-One Resource Grouping

Display all child resources for each parent (ByGroupID="false"):

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup ByGroupID="false" Resources="@Resources"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="TeamData" TValue="int"
            DataSource="@TeamList"
            Field="TeamId"
            Title="Team"
            Name="Teams"
            TextField="Name"
            IdField="Id"
            ColorField="Color">
        </ScheduleResource>
        
        <ScheduleResource TItem="PersonData" TValue="int"
            DataSource="@PersonList"
            Field="PersonId"
            Title="Person"
            Name="Persons"
            TextField="Name"
            IdField="Id"
            GroupIDField="TeamId"
            ColorField="Color">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] Resources { get; set; } = { "Teams", "Persons" };

    List<TeamData> TeamList = new List<TeamData>
    {
        new TeamData { Id = 1, Name = "Team A", Color = "#FF6B6B" },
        new TeamData { Id = 2, Name = "Team B", Color = "#4ECDC4" }
    };

    List<PersonData> PersonList = new List<PersonData>
    {
        new PersonData { Id = 1, Name = "Nancy", TeamId = 1, Color = "#FF9999" },
        new PersonData { Id = 2, Name = "Steven", TeamId = 1, Color = "#FFB3B3" },
        new PersonData { Id = 3, Name = "Michael", TeamId = 2, Color = "#7FE5DE" },
        new PersonData { Id = 4, Name = "Angela", TeamId = 2, Color = "#B3F0EB" }
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Meeting",
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0),
            TeamId = 1,
            PersonId = 1
        }
    };

    public class TeamData
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Color { get; set; }
    }

    public class PersonData
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public int TeamId { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int TeamId { get; set; }
        public int PersonId { get; set; }
    }
}
```

## Grouping Resources by Date

Group resources under each date (ByDate="true", calendar views only):

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup ByDate="true" Resources="@Resources"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int"
            DataSource="@ResourceList"
            Field="ResourceId"
            Title="Owner"
            Name="Owners"
            TextField="Name"
            IdField="Id"
            ColorField="Color">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] Resources { get; set; } = { "Owners" };

    List<ResourceData> ResourceList = new List<ResourceData>
    {
        new ResourceData { Id = 1, Name = "Nancy", Color = "#FFAA00" },
        new ResourceData { Id = 2, Name = "Steven", Color = "#7FA900" },
        new ResourceData { Id = 3, Name = "Michael", Color = "#5978EE" }
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Nancy's Task",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0),
            ResourceId = 1
        },
        new AppointmentData
        {
            Id = 2,
            Subject = "Steven's Meeting",
            StartTime = new DateTime(2026, 3, 25, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 25, 11, 0, 0),
            ResourceId = 2
        }
    };

    public class ResourceData
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int ResourceId { get; set; }
    }
}
```

## Shared Events Across Resources

Create events that appear for multiple resources with synchronized editing (AllowGroupEdit="true"):

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup AllowGroupEdit="true" Resources="@Resources"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int[]"
            DataSource="@ResourceList"
            Field="ResourceIds"
            Title="Select Attendees"
            Name="Attendees"
            TextField="Name"
            IdField="Id"
            ColorField="Color"
            AllowMultiple="true">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] Resources { get; set; } = { "Attendees" };

    List<ResourceData> ResourceList = new List<ResourceData>
    {
        new ResourceData { Id = 1, Name = "Nancy", Color = "#FFAA00" },
        new ResourceData { Id = 2, Name = "Steven", Color = "#7FA900" },
        new ResourceData { Id = 3, Name = "Michael", Color = "#5978EE" }
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Team Meeting - All Attendees",
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0),
            ResourceIds = new int[] { 1, 2, 3 }
        },
        new AppointmentData
        {
            Id = 2,
            Subject = "Nancy and Steven Meeting",
            StartTime = new DateTime(2026, 3, 24, 14, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 15, 0, 0),
            ResourceIds = new int[] { 1, 2 }
        }
    };

    public class ResourceData
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int[] ResourceIds { get; set; }
    }
}
```

## Resource Header Customization

Customize resource header display with custom templates:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Width="100%" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleTemplates>
        <ResourceHeaderTemplate>
            @{
                var resourceData = (context as TemplateContext).ResourceData as ResourceData;
                <div class='template-wrap'>
                    <div class="resource-image"><img src="https://ej2.syncfusion.com/demos/src/schedule/images/@(resourceData.Image).png" /></div>
                    <div class="resource-details">
                        <div class="resource-name">@(resourceData.Text)</div>
                        <div class="resource-designation">@(resourceData.Designation)</div>
                    </div>
                </div>
            }
        </ResourceHeaderTemplate>
    </ScheduleTemplates>
    <ScheduleGroup Resources="@Resources"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int" DataSource="@DoctorsData" Field="DoctorId" Title="Doctor Name" Name="Doctors" TextField="Text" IdField="Id" ColorField="Color"></ScheduleResource>
    </ScheduleResources>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
        <ScheduleView Option="View.Agenda"></ScheduleView>
    </ScheduleViews>
</SfSchedule>
@code{
    DateTime CurrentDate = new DateTime(2023, 6, 1);
    public string[] Resources { get; set; } = { "Doctors" };
    public List<ResourceData> DoctorsData { get; set; } = new List<ResourceData>
    {
        new ResourceData{ Text = "Will Smith", Id = 1, Color = "#ea7a57", Designation = "Cardiologist", Image = "will-smith" },
        new ResourceData{ Text = "Alice", Id = 2, Color = "rgb(53, 124, 210)", Designation = "Neurologist", Image = "alice"  },
        new ResourceData{ Text = "Robson", Id = 3, Color = "#7fa900", Designation = "Orthopedic Surgeon", Image = "robson"  }
    };
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public string Location { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public string Description { get; set; }
        public bool IsAllDay { get; set; }
        public string RecurrenceRule { get; set; }
        public string RecurrenceException { get; set; }
        public Nullable<int> RecurrenceID { get; set; }
        public int DoctorId { get; set; }
    }
    public class ResourceData
    {
        public int Id { get; set; }
        public string Text { get; set; }
        public string Designation { get; set; }
        public string Color { get; set; }
        public string Image { get; set; }
    }
}
<style>
    .e-schedule .e-vertical-view .e-resource-cells {
        height: 62px;
    }

    .e-schedule .template-wrap {
        display: flex;
        text-align: left;
    }

    .e-schedule .template-wrap .resource-image img {
        width: 45px;
        height: 45px;
    }

    .e-schedule .template-wrap .resource-details {
        padding-left: 10px;
    }

    .e-schedule .template-wrap .resource-details .resource-name {
        font-size: 16px;
        font-weight: 500;
        margin-top: 5px;
    }

    .e-schedule.e-device .template-wrap .resource-details .resource-name {
        font-size: inherit;
        font-weight: inherit;
    }

    .e-schedule.e-device .e-resource-tree-popup .e-fullrow {
        height: 50px;
    }

    .e-schedule.e-device .template-wrap .resource-details .resource-designation {
        display: none;
    }
</style>
```

## Multiple Column Resource Headers

Display resources with multiple columns (timeline views only):

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup Resources="@Resources"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int"
            DataSource="@ResourceList"
            Field="ResourceId"
            Title="Room"
            Name="Rooms"
            TextField="Name"
            IdField="Id"
            ColorField="Color">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineMonth"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    .e-schedule .e-timeline-month-view .e-resource-left-td {
        width: 200px;
    }

    .e-schedule .e-timeline-month-view .e-resource-left-td .e-resource-text {
        display: grid;
        grid-template-columns: 100px 100px;
        gap: 5px;
        padding: 10px;
    }

    .resource-column {
        border-right: 1px solid #ddd;
        padding: 5px;
        text-align: center;
    }
</style>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] Resources { get; set; } = { "Rooms" };

    List<ResourceData> ResourceList = new List<ResourceData>
    {
        new ResourceData { Id = 1, Name = "Board Room", Capacity = "20", Type = "Executive", Color = "#FF9800" },
        new ResourceData { Id = 2, Name = "Meeting Room A", Capacity = "10", Type = "Standard", Color = "#2196F3" }
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Meeting",
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0),
            ResourceId = 1
        }
    };

    public class ResourceData
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Capacity { get; set; }
        public string Type { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int ResourceId { get; set; }
    }
}
```

## Expand and Collapse Resources

Control initial expand/collapse state using ExpandedField:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup Resources="@Resources"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int"
            DataSource="@ResourceList"
            Field="ResourceId"
            Title="Room"
            Name="Rooms"
            TextField="Name"
            IdField="Id"
            ExpandedField="IsExpanded"
            ColorField="Color">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineMonth"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] Resources { get; set; } = { "Rooms" };

    List<ResourceData> ResourceList = new List<ResourceData>
    {
        new ResourceData { Id = 1, Name = "Room A", IsExpanded = true, Color = "#FF9800" },
        new ResourceData { Id = 2, Name = "Room B", IsExpanded = false, Color = "#2196F3" }
    };

    List<AppointmentData> DataSource = new List<AppointmentData>();

    public class ResourceData
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public bool IsExpanded { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int ResourceId { get; set; }
    }
}
```

## Resource Header Tooltips

Display tooltips on resource headers using HeaderTooltipTemplate:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup Resources="@Resources">
        <HeaderTooltipTemplate>
            @{
                var resourceData = (context as TemplateContext).ResourceData as ResourceData;
                <div class='template-wrap'>
                    <div class="resource-image"><img src="https://ej2.syncfusion.com/demos/src/schedule/images/@(resourceData.Image).png" /></div>
                    <div class="resource-details">
                        <div class="resource-name">@(resourceData.Text)</div>
                    </div>
                </div>
            }
        </HeaderTooltipTemplate>
    </ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TValue="int[]" TItem="ResourceData" DataSource="@ConferenceData" Field="ConferenceId" Title="Attendees" Name="Conferences" TextField="Text" IdField="Id" ColorField="Color" AllowMultiple="true"></ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
        <ScheduleView Option="View.Agenda"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2023, 6, 1);
    public string[] Resources { get; set; } = { "Conferences" };
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
       new AppointmentData { Id = 1, Subject = "Meeting", StartTime = new DateTime(2023, 6, 16, 9, 30, 0) , EndTime = new DateTime(2023, 6, 16, 11, 0, 0),
       ConferenceId = 1 }
    };
    public List<ResourceData> ConferenceData { get; set; } = new List<ResourceData>
    {
        new ResourceData{ Text = "Margaret", Id = 1, Color = "#1aaa55", Image= "margaret" },
        new ResourceData{ Text = "Robert", Id = 2, Color = "#357cd2", Image= "robert" },
        new ResourceData{ Text = "Laura", Id = 3, Color = "#7fa900", Image= "laura" }
    };
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public string Location { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public string Description { get; set; }
        public bool IsAllDay { get; set; }
        public string RecurrenceRule { get; set; }
        public string RecurrenceException { get; set; }
        public Nullable<int> RecurrenceID { get; set; }
        public int ConferenceId { get; set; }
    }
    public class ResourceData
    {
        public int Id { get; set; }
        public string Image { get; set; }
        public string Text { get; set; }
        public string Color { get; set; }
    }
}
```

## Resource Color Selection

Apply specific resource color to events using ResourceColorField:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource" ResourceColorField="ResourceColor"></ScheduleEventSettings>
    <ScheduleGroup Resources="@Resources"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int"
            DataSource="@ResourceList"
            Field="ResourceId"
            Title="Owner"
            Name="Owners"
            TextField="Name"
            IdField="Id"
            ColorField="Color">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] Resources { get; set; } = { "Owners" };

    List<ResourceData> ResourceList = new List<ResourceData>
    {
        new ResourceData { Id = 1, Name = "Nancy", Color = "#FFAA00" },
        new ResourceData { Id = 2, Name = "Steven", Color = "#7FA900" }
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Meeting 1",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0),
            ResourceId = 1,
            ResourceColor = "Owners"
        }
    };

    public class ResourceData
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int ResourceId { get; set; }
        public string ResourceColor { get; set; }
    }
}
```

## Custom Working Hours by Resource

Set different work hours for each resource using StartHourField and EndHourField:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup Resources="@Resources"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int"
            DataSource="@ResourceList"
            Field="ResourceId"
            Title="Staff"
            Name="Staff"
            TextField="Name"
            IdField="Id"
            StartHourField="StartHour"
            EndHourField="EndHour"
            ColorField="Color">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] Resources { get; set; } = { "Staff" };

    List<ResourceData> ResourceList = new List<ResourceData>
    {
        new ResourceData { Id = 1, Name = "Nancy (9-5)", StartHour = 9, EndHour = 17, Color = "#FFAA00" },
        new ResourceData { Id = 2, Name = "Steven (10-6)", StartHour = 10, EndHour = 18, Color = "#7FA900" },
        new ResourceData { Id = 3, Name = "Michael (8-4)", StartHour = 8, EndHour = 16, Color = "#5978EE" }
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Nancy's Meeting",
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0),
            ResourceId = 1
        }
    };

    public class ResourceData
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public int StartHour { get; set; }
        public int EndHour { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int ResourceId { get; set; }
    }
}
```

## Custom Working Days by Resource

Set different working days for each resource using WorkDaysField:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup Resources="@Resources"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int"
            DataSource="@ResourceList"
            Field="ResourceId"
            Title="Staff"
            Name="Staff"
            TextField="Name"
            IdField="Id"
            WorkDaysField="WorkDays"
            ColorField="Color">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] Resources { get; set; } = { "Staff" };

    List<ResourceData> ResourceList = new List<ResourceData>
    {
        // 0=Sunday, 1=Monday, ..., 6=Saturday
        // Nancy works Mon-Fri
        new ResourceData 
        { 
            Id = 1, 
            Name = "Nancy (Mon-Fri)", 
            WorkDays = new int[] { 1, 2, 3, 4, 5 },
            Color = "#FFAA00" 
        },
        // Steven works Tue-Sat
        new ResourceData 
        { 
            Id = 2, 
            Name = "Steven (Tue-Sat)", 
            WorkDays = new int[] { 2, 3, 4, 5, 6 },
            Color = "#7FA900" 
        }
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Monday Meeting",
            StartTime = new DateTime(2026, 3, 23, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 23, 11, 0, 0),
            ResourceId = 1
        }
    };

    public class ResourceData
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public int[] WorkDays { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int ResourceId { get; set; }
    }
}
```
