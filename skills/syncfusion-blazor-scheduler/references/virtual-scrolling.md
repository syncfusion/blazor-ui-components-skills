# Virtual Scrolling

## Table of Contents
- [Overview](#overview)
- [Virtual Scrolling in Timeline Views](#virtual-scrolling-in-timeline-views)
- [Virtual Scrolling in Agenda View](#virtual-scrolling-in-agenda-view)
- [Virtual Resource Count Configuration](#virtual-resource-count-configuration)
- [Virtual Scrolling with Resources](#virtual-scrolling-with-resources)
- [Virtual Scrolling with Resource Grouping](#virtual-scrolling-with-resource-grouping)
- [Virtual Scrolling with Templates](#virtual-scrolling-with-templates)
- [Lazy Loading Appointments](#lazy-loading-appointments)
- [Lazy Loading with Resources](#lazy-loading-with-resources)
- [Server-Side Lazy Loading Implementation](#server-side-lazy-loading-implementation)
- [Performance Optimization](#performance-optimization)
- [Virtual Scrolling Limitations](#virtual-scrolling-limitations)

## Overview

Virtual scrolling is a performance optimization technique that loads only the visible portion of data into the DOM, dynamically loading/unloading items as you scroll. This enables the Scheduler to handle massive datasets (thousands of resources and events) without performance degradation.

**When to Use Virtual Scrolling:**
- 100+ resources with grouped timeline views
- 5,000+ appointments across resources
- Real-time data with continuous updates
- Mobile/low-memory environments
- Timeline Month/Year views with resource grouping

**Benefits:**
- Dramatically reduced memory footprint
- Smooth scrolling even with 10,000+ events
- Only visible items rendered to DOM
- Automatic scroll handling and caching
- Scales to enterprise-level data volumes

## Virtual Scrolling in Timeline Views

Enable virtual scrolling:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <!-- Virtual scrolling in TimelineMonth -->
        <ScheduleView Option="View.TimelineMonth" AllowVirtualScrolling="true" IsSelected="true"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Project Planning",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0)
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

**Supported Timeline Views with Virtual Scrolling:**
- TimelineDay
- TimelineWeek
- TimelineWorkWeek
- TimelineMonth
- TimelineYear (with limitations)

## Virtual Scrolling in Agenda View

Enable virtual scrolling in Agenda view to handle large upcoming event lists:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <!-- Virtual scrolling in Agenda view -->
        <ScheduleView Option="View.Agenda" AllowVirtualScrolling="true"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData
        {
            Id = 1,
            Subject = "Team Meeting",
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0)
        },
        new AppointmentData
        {
            Id = 2,
            Subject = "Client Call",
            StartTime = new DateTime(2026, 3, 26, 14, 0, 0),
            EndTime = new DateTime(2026, 3, 26, 15, 0, 0)
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

**Use Cases for Agenda Virtual Scrolling:**
- Display 14-30 days of events
- Show thousands of upcoming appointments
- List-based view for mobile devices
- Efficient memory usage for event listings

## Virtual Resource Count Configuration

Control the number of visible resources loaded at once:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup EnableCompactView="false" Resources="@GroupData"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int" DataSource="@ResourceList" Field="ResourceId" Title="Resource" Name="Resources" TextField="Text" IdField="Id" ColorField="Color"></ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <!-- Default: 30 resources visible at once -->
        <ScheduleView Option="View.TimelineMonth" AllowVirtualScrolling="true" IsSelected="true"></ScheduleView>
        
        <!-- Custom: 50 resources visible at once -->
        <ScheduleView Option="View.TimelineMonth" AllowVirtualScrolling="true" VirtualResourceCount="50" DisplayName="50 Resources"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] GroupData { get; set; } = { "Resources" };

    List<ResourceData> ResourceList = new List<ResourceData>();
    List<AppointmentData> DataSource = new List<AppointmentData>();

    protected override void OnInitialized()
    {
        // Generate 300 resources
        for (int i = 1; i <= 300; i++)
        {
            ResourceList.Add(new ResourceData
            {
                Id = i,
                Text = $"Resource {i}",
                Color = GetColor(i)
            });
        }

        // Generate appointments for resources
        for (int i = 1; i <= 300; i++)
        {
            DataSource.Add(new AppointmentData
            {
                Id = i,
                Subject = $"Event for Resource {i}",
                ResourceId = i,
                StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
                EndTime = new DateTime(2026, 3, 24, 10, 0, 0)
            });
        }
    }

    private string GetColor(int index)
    {
        var colors = new string[] { "#FF6B6B", "#4ECDC4", "#45B7D1", "#FFA07A", "#98D8C8" };
        return colors[index % colors.Length];
    }

    public class ResourceData
    {
        public int Id { get; set; }
        public string Text { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public int ResourceId { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

**VirtualResourceCount Guidance:**
- **Default:** 30 resources visible
- **Small screens:** 20-30 resources (mobile, tablets)
- **Standard:** 30-50 resources (desktop, most use cases)
- **Large screens:** 50-100 resources (high-resolution displays)
- **Very large:** 100+ resources (specialized applications)

## Virtual Scrolling with Resources

Use virtual scrolling with resource-based scheduling:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Width="100%" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup EnableCompactView="false" Resources="@GroupData"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int" DataSource="@ResourceList" Field="ResourceId" Title="Resource" Name="Resources" TextField="Text" IdField="Id" ColorField="Color" AllowMultiple="true"></ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineMonth" AllowVirtualScrolling="true" MaxEventsPerRow="4"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] GroupData { get; set; } = { "Resources" };

    List<ResourceData> ResourceList = new List<ResourceData>();
    List<AppointmentData> DataSource = new List<AppointmentData>();

    protected override void OnInitialized()
    {
        // Generate 500 resources for large-scale scenario
        for (int i = 1; i <= 500; i++)
        {
            ResourceList.Add(new ResourceData
            {
                Id = i,
                Text = $"Resource {i}",
                Color = $"#{Random.Shared.Next(0x1000000):X6}"
            });
        }

        // Generate 3600 appointments (multiple per resource)
        var id = 1;
        for (int resource = 1; resource <= 500; resource++)
        {
            for (int day = 0; day < 30; day++)
            {
                if (Random.Shared.Next(10) < 3) // ~30% of resources have events each day
                {
                    DataSource.Add(new AppointmentData
                    {
                        Id = id++,
                        Subject = $"Task for Resource {resource}",
                        ResourceId = resource,
                        StartTime = new DateTime(2026, 3, 24).AddDays(day).AddHours(9),
                        EndTime = new DateTime(2026, 3, 24).AddDays(day).AddHours(10)
                    });
                }
            }
        }
    }

    public class ResourceData
    {
        public int Id { get; set; }
        public string Text { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public int ResourceId { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Virtual Scrolling with Resource Grouping

Combine virtual scrolling with hierarchical resource grouping:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Width="100%" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup EnableCompactView="false" Resources="@GroupData"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int" DataSource="@ResourceList" Field="ResourceId" Title="Resource" Name="Resources" TextField="Text" IdField="Id" GroupIDField="GroupId" ColorField="Color"></ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <!-- Virtual scrolling with multi-level grouping -->
        <ScheduleView Option="View.TimelineMonth" AllowVirtualScrolling="true" MaxEventsPerRow="3"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] GroupData { get; set; } = { "Resources" };

    List<ResourceData> ResourceList = new List<ResourceData>();
    List<AppointmentData> DataSource = new List<AppointmentData>();

    protected override void OnInitialized()
    {
        // Generate grouped resources (e.g., departments with team members)
        var departments = new[] { "Engineering", "Sales", "Support", "Marketing", "Operations" };
        var id = 1;
        
        for (int dept = 0; dept < departments.Length; dept++)
        {
            // 60 resources per department = 300 total
            for (int member = 1; member <= 60; member++)
            {
                ResourceList.Add(new ResourceData
                {
                    Id = id,
                    Text = $"{departments[dept]} - Person {member}",
                    GroupId = dept + 1,
                    Color = GetDepartmentColor(dept)
                });
                id++;
            }
        }

        // Generate appointments
        var appointmentId = 1;
        for (int resourceId = 1; resourceId <= 300; resourceId++)
        {
            for (int day = 0; day < 20; day++)
            {
                DataSource.Add(new AppointmentData
                {
                    Id = appointmentId++,
                    Subject = $"Event for Resource {resourceId}",
                    ResourceId = resourceId,
                    StartTime = new DateTime(2026, 3, 24).AddDays(day).AddHours(9),
                    EndTime = new DateTime(2026, 3, 24).AddDays(day).AddHours(10)
                });
            }
        }
    }

    private string GetDepartmentColor(int deptIndex)
    {
        var colors = new[] { "#FF6B6B", "#4ECDC4", "#45B7D1", "#FFA07A", "#98D8C8" };
        return colors[deptIndex];
    }

    public class ResourceData
    {
        public int Id { get; set; }
        public string Text { get; set; }
        public int GroupId { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public int ResourceId { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Virtual Scrolling with Templates

Apply custom templates while using virtual scrolling:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Width="100%" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleTemplates>
        <ResourceHeaderTemplate>
            <div style="padding: 5px; font-weight: bold;">
                <span>@(((context as TemplateContext).ResourceData as ResourceData).Text)</span>
                <span style="font-size: 0.8em; display: block; color: #666;">
                    @(((context as TemplateContext).ResourceData as ResourceData).Designation)
                </span>
            </div>
        </ResourceHeaderTemplate>
    </ScheduleTemplates>
    
    <ScheduleGroup EnableCompactView="false" Resources="@GroupData"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int" DataSource="@ResourceList" Field="ResourceId" Title="Resource" Name="Resources" TextField="Text" IdField="Id" ColorField="Color"></ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource">
        <Template>
            <div style="padding: 2px; background: white; border-radius: 3px; font-size: 0.9em;">
                <div style="font-weight: bold;">@((context as AppointmentData).Subject)</div>
                <div style="color: #666; font-size: 0.85em;">
                    @((context as AppointmentData).StartTime.ToString("HH:mm")) - @((context as AppointmentData).EndTime.ToString("HH:mm"))
                </div>
            </div>
        </Template>
    </ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineMonth" AllowVirtualScrolling="true" MaxEventsPerRow="2"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] GroupData { get; set; } = { "Resources" };

    List<ResourceData> ResourceList = new List<ResourceData>();
    List<AppointmentData> DataSource = new List<AppointmentData>();

    protected override void OnInitialized()
    {
        for (int i = 1; i <= 100; i++)
        {
            ResourceList.Add(new ResourceData
            {
                Id = i,
                Text = $"Team Member {i}",
                Designation = GetDesignation(i),
                Color = GetColor(i)
            });
        }

        var appointmentId = 1;
        for (int resourceId = 1; resourceId <= 100; resourceId++)
        {
            for (int day = 0; day < 15; day++)
            {
                DataSource.Add(new AppointmentData
                {
                    Id = appointmentId++,
                    Subject = $"Scheduled Task",
                    ResourceId = resourceId,
                    StartTime = new DateTime(2026, 3, 24).AddDays(day).AddHours(10),
                    EndTime = new DateTime(2026, 3, 24).AddDays(day).AddHours(11)
                });
            }
        }
    }

    private string GetDesignation(int index)
    {
        var designations = new[] { "Developer", "Manager", "Designer", "Analyst", "Lead" };
        return designations[index % designations.Length];
    }

    private string GetColor(int index)
    {
        var colors = new string[] { "#FF6B6B", "#4ECDC4", "#45B7D1", "#FFA07A", "#98D8C8" };
        return colors[index % colors.Length];
    }

    public class ResourceData
    {
        public int Id { get; set; }
        public string Text { get; set; }
        public string Designation { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public int ResourceId { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Lazy Loading Appointments

Enable lazy loading to fetch appointments on-demand using `EnableLazyLoading="true"`:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Width="100%" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup EnableCompactView="false" Resources="@GroupData"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int" DataSource="@ResourceList" Field="ResourceId" Title="Resource" Name="Resources" TextField="Text" IdField="Id" ColorField="Color"></ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <!-- Lazy loading fetches events on-demand as resources scroll into view -->
        <ScheduleView Option="View.TimelineMonth" AllowVirtualScrolling="true" EnableLazyLoading="true" IsSelected="true"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] GroupData { get; set; } = { "Resources" };

    List<ResourceData> ResourceList = new List<ResourceData>();
    List<AppointmentData> DataSource = new List<AppointmentData>();

    protected override void OnInitialized()
    {
        // Generate 1000 resources
        for (int i = 1; i <= 1000; i++)
        {
            ResourceList.Add(new ResourceData
            {
                Id = i,
                Text = $"Resource {i}",
                Color = $"#{Random.Shared.Next(0x1000000):X6}"
            });
        }

        // Pre-load only initial visible appointments (for demonstration)
        // In real scenario, backend handles lazy loading per viewport
        var appointmentId = 1;
        for (int resourceId = 1; resourceId <= 50; resourceId++) // Load only first 50
        {
            DataSource.Add(new AppointmentData
            {
                Id = appointmentId++,
                Subject = $"Event for Resource {resourceId}",
                ResourceId = resourceId,
                StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
                EndTime = new DateTime(2026, 3, 24, 10, 0, 0)
            });
        }
    }

    public class ResourceData
    {
        public int Id { get; set; }
        public string Text { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public int ResourceId { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Lazy Loading with Resources

Use lazy loading with Syncfusion DataManager for server-side filtering:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Data

<SfSchedule TValue="AppointmentData" Width="100%" Height="650px" @bind-SelectedDate="@CurrentDate" Readonly="true">
    <ScheduleGroup EnableCompactView="false" Resources="@GroupData"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int" DataSource="@ResourceList" Field="ResourceId" Title="Resource" Name="Resources" TextField="Text" IdField="Id" ColorField="Color"></ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings TValue="AppointmentData">
        <SfDataManager Url="https://blazor.syncfusion.com/services/production/api/VirtualEventData" Adaptor="Adaptors.WebApiAdaptor" CrossDomain="true"></SfDataManager>
    </ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineMonth" AllowVirtualScrolling="true" EnableLazyLoading="true" IsSelected="true"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] GroupData { get; set; } = { "Resources" };

    List<ResourceData> ResourceList = new List<ResourceData>();

    protected override void OnInitialized()
    {
        // Generate 1000 resources
        for (int i = 1; i <= 1000; i++)
        {
            ResourceList.Add(new ResourceData
            {
                Id = i,
                Text = $"Resource {i}",
                Color = $"#{Random.Shared.Next(0x1000000):X6}"
            });
        }
    }

    public class ResourceData
    {
        public int Id { get; set; }
        public string Text { get; set; }
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

## Server-Side Lazy Loading Implementation

Implement server-side controller to handle lazy loading with resource ID filtering:

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.OData.Query;
using System;
using System.Collections.Generic;
using System.Linq;

namespace AppointmentServices.Controllers
{
    public class VirtualEventDataController : Controller
    {
        private readonly AppointmentContext dbContext;

        public VirtualEventDataController(AppointmentContext context)
        {
            dbContext = context;
        }

        // Lazy loading endpoint: returns appointments filtered by ResourceId
        [HttpGet]
        [EnableQuery]
        [Route("api/VirtualEventData")]
        public IActionResult GetData([FromQuery] LazyLoadingParams param)
        {
            IQueryable<AppointmentData> query = dbContext.Appointments;

            // Filter by resource IDs (comma-separated from query string)
            if (!string.IsNullOrEmpty(param.ResourceId))
            {
                var resourceIds = param.ResourceId.Split(',').Select(int.Parse).ToList();
                query = query.Where(a => resourceIds.Contains(a.ResourceId));
            }

            // Filter by date range
            if (param.StartDate.HasValue)
                query = query.Where(a => a.StartTime >= param.StartDate);
            
            if (param.EndDate.HasValue)
                query = query.Where(a => a.EndTime <= param.EndDate);

            return Ok(query.ToList());
        }

        public class LazyLoadingParams
        {
            public DateTime? StartDate { get; set; }
            public DateTime? EndDate { get; set; }
            public string ResourceId { get; set; } // Comma-separated IDs
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
}
```

## Performance Optimization

Combine virtual scrolling with performance best practices:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Width="100%" Height="650px" 
            @bind-SelectedDate="@CurrentDate"
            EnableMaxHeight="true"
            AllowKeyboard="true">
    
    <ScheduleGroup EnableCompactView="false" Resources="@GroupData"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int" DataSource="@ResourceList" Field="ResourceId" Title="Resource" Name="Resources" TextField="Text" IdField="Id" ColorField="Color"></ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.TimelineMonth" 
                     AllowVirtualScrolling="true" 
                     EnableLazyLoading="true"
                     VirtualResourceCount="40"
                     MaxEventsPerRow="3">
        </ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    /* Optimize CSS rendering */
    .e-schedule .e-timeline-view .e-resource-cells {
        min-height: 45px;
    }
    
    /* Reduce reflow during scroll */
    .e-schedule .e-appointment {
        will-change: transform;
    }
</style>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] GroupData { get; set; } = { "Resources" };

    List<ResourceData> ResourceList = new List<ResourceData>();
    List<AppointmentData> DataSource = new List<AppointmentData>();

    protected override void OnInitialized()
    {
        // Generate 500 resources
        for (int i = 1; i <= 500; i++)
        {
            ResourceList.Add(new ResourceData
            {
                Id = i,
                Text = $"Resource {i}",
                Color = GetColor(i)
            });
        }

        // Generate sparse appointments (not all resources have events every day)
        var appointmentId = 1;
        for (int resourceId = 1; resourceId <= 500; resourceId++)
        {
            for (int day = 0; day < 30; day++)
            {
                // ~20% chance of event per resource per day
                if (Random.Shared.Next(100) < 20)
                {
                    DataSource.Add(new AppointmentData
                    {
                        Id = appointmentId++,
                        Subject = "Task",
                        ResourceId = resourceId,
                        StartTime = new DateTime(2026, 3, 24).AddDays(day).AddHours(9),
                        EndTime = new DateTime(2026, 3, 24).AddDays(day).AddHours(10)
                    });
                }
            }
        }
    }

    private string GetColor(int index)
    {
        var colors = new string[] { "#FF6B6B", "#4ECDC4", "#45B7D1", "#FFA07A", "#98D8C8" };
        return colors[index % colors.Length];
    }

    public class ResourceData
    {
        public int Id { get; set; }
        public string Text { get; set; }
        public string Color { get; set; }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public int ResourceId { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

**Performance Optimization Tips:**
- **Enable virtual scrolling** for 100+ resources
- **Use EnableLazyLoading** for 5,000+ appointments
- **Limit VirtualResourceCount** to 30-50 for smooth scrolling
- **Minimize MaxEventsPerRow** to reduce cell complexity
- **Use sparse data** (not every resource has events every day)
- **Combine with server-side filtering** (DataManager with ResourceId query)
- **Avoid heavy templates** (templates add DOM overhead)
- **Disable unused features** (Readonly="true" reduces event handlers)

## Virtual Scrolling Limitations

**Important Constraints:**
- ❌ **Not supported in:**
  - Month view (vertical calendar grid)
  - Agenda view at very large scales (uses different virtualization)
  - MonthAgenda view
  - Year view

- ⚠️ **Supported with limitations:**
  - TimelineYear (Horizontal Orientation only, Vertical has reduced support)
  - Large templates may impact scroll performance
  - Complex nested grouping reduces efficiency

- 📌 **Best practices:**
  - Reserve virtual scrolling for specialized high-volume scenarios
  - Test performance with your actual data size
  - Monitor memory usage with large datasets
  - Enable lazy loading for truly massive datasets (10,000+ events)
  - Keep resource count reasonable (500-1000 max for optimal performance)
