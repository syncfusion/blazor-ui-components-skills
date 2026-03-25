# Data Binding

## Table of Contents
- [Overview](#overview)
- [List Binding](#list-binding)
- [ExpandoObject Binding](#expandobject-binding)
- [DynamicObject Binding](#dynamicobject-binding)
- [ObservableCollection](#observablecollection)
- [Remote Data Binding](#remote-data-binding)
- [OData Services](#odata-services)
- [OData v4 Services](#odata-v4-services)
- [Server-Side Filtering](#server-side-filtering)
- [Web API Adaptor](#web-api-adaptor)
- [Url Adaptor](#url-adaptor)
- [Custom Headers and Authentication](#custom-headers-and-authentication)
- [Error Handling](#error-handling)
- [Load on Demand](#load-on-demand)

## Overview

The Scheduler uses `DataManager` to support both RESTful data service binding and local data source collections. The `DataSource` property can be assigned with an instance of `DataManager` or a list of datasource collection. This provides flexible data binding options for local and remote data sources.

## List Binding

Bind local data to the Scheduler by assigning a list collection to the `DataSource` property within `ScheduleEventSettings`:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Testing", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 30, 0)
        },
        new AppointmentData 
        { 
            Id = 2, 
            Subject = "Conference", 
            StartTime = new DateTime(2026, 3, 24, 12, 30, 0),
            EndTime = new DateTime(2026, 3, 24, 13, 30, 0)
        },
        new AppointmentData 
        { 
            Id = 3, 
            Subject = "Vacation", 
            StartTime = new DateTime(2026, 3, 25, 11, 30, 0),
            EndTime = new DateTime(2026, 3, 25, 13, 0, 0)
        }
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
    }
}
```

> By default, `DataManager` uses `BlazorAdaptor` for binding local data.

## ExpandoObject Binding

Use **ExpandoObject** for dynamic binding when model type is unknown at compile time. ExpandoObject implements `IDictionary<string, object>`, allowing flexible property addition:

```cshtml
@using System.Dynamic
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="ExpandoObject" @bind-SelectedDate="@CurrentDate" Width="100%" Height="550px">
    <ScheduleEventSettings DataSource="@EventsCollection"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
        <ScheduleView Option="View.Agenda"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public List<ExpandoObject> EventsCollection = new List<ExpandoObject>() { };
    
    protected override void OnInitialized()
    {
        DateTime scheduleStart = new DateTime(2026, 3, 24, 10, 0, 0);
        
        EventsCollection = Enumerable.Range(1, 5).Select((x) =>
        {
            dynamic expandoEvent = new ExpandoObject();
            expandoEvent.Id = x;
            expandoEvent.Subject = "Event " + x;
            expandoEvent.StartTime = scheduleStart.AddDays(x - 1);
            expandoEvent.EndTime = scheduleStart.AddDays(x - 1).AddHours(1);
            expandoEvent.IsAllDay = false;
            return (ExpandoObject)expandoEvent;
        }).Cast<ExpandoObject>().ToList<ExpandoObject>();
    }
}
```

## DynamicObject Binding

Use **DynamicObject** for dynamic behavior when working with objects whose properties are unknown at compile time:

```cshtml
@using System.Dynamic
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="DynamicDictionary" @bind-SelectedDate="@CurrentDate" Width="100%" Height="550px">
    <ScheduleEventSettings DataSource="@EventsCollection"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
        <ScheduleView Option="View.Agenda"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public List<DynamicDictionary> EventsCollection = new List<DynamicDictionary>() { };
    
    protected override void OnInitialized()
    {
        DateTime scheduleStart = new DateTime(2026, 3, 24, 10, 0, 0);
        
        EventsCollection = Enumerable.Range(1, 5).Select((x) =>
        {
            var dynamicEvent = new DynamicDictionary();
            dynamicEvent.Add("Id", x);
            dynamicEvent.Add("Subject", "Event " + x);
            dynamicEvent.Add("StartTime", scheduleStart.AddDays(x - 1));
            dynamicEvent.Add("EndTime", scheduleStart.AddDays(x - 1).AddHours(1));
            dynamicEvent.Add("IsAllDay", false);
            return dynamicEvent;
        }).Cast<DynamicDictionary>().ToList<DynamicDictionary>();
    }
    
    public class DynamicDictionary : System.Dynamic.DynamicObject
    {
        Dictionary<string, object> dictionary = new Dictionary<string, object>();
        
        public void Add(string key, object value)
        {
            dictionary.Add(key, value);
        }
        
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
            return dictionary.Keys;
        }
    }
}
```

## ObservableCollection

Use **ObservableCollection** for automatic updates when items are added, removed, or modified. Implements `INotifyCollectionChanged` and `INotifyPropertyChanged`:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons
@using System.Collections.ObjectModel
@using System.ComponentModel

<SfButton @onclick="AddRecord">Add Data</SfButton>
<SfButton @onclick="UpdateRecord" Disabled="ObservableData.Count == 0">Update Data</SfButton>
<SfButton @onclick="DeleteRecord" Disabled="ObservableData.Count == 0">Delete Data</SfButton>

<SfSchedule TValue="AppointmentData" @bind-SelectedDate="@CurrentDate" Width="100%" Height="550px">
    <ScheduleEventSettings DataSource="@ObservableData"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
        <ScheduleView Option="View.Agenda"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public ObservableCollection<AppointmentData> ObservableData { get; set; }
    List<AppointmentData> EventsCollection = new List<AppointmentData>();
    int uniqueid = 1;
    
    protected override void OnInitialized()
    {
        EventsCollection = Enumerable.Range(1, 4).Select(x => new AppointmentData()
        {
            Id = x,
            Subject = "Meeting " + x,
            StartTime = new DateTime(2026, 3, 24 + x - 1, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 24 + x - 1, 11, 0, 0),
            IsAllDay = false
        }).ToList();
        
        ObservableData = new ObservableCollection<AppointmentData>(EventsCollection);
        uniqueid = EventsCollection.Count;
    }
    
    public void AddRecord()
    {
        uniqueid++;
        ObservableData.Add(new AppointmentData() 
        { 
            Id = uniqueid, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 28, 9, 0, 0), 
            EndTime = new DateTime(2026, 3, 28, 11, 0, 0) 
        });
    }
    
    public void DeleteRecord()
    {
        if (ObservableData.Count != 0)
        {
            ObservableData.RemoveAt(0);
        }
    }
    
    public void UpdateRecord()
    {
        if (ObservableData.Count != 0)
        {
            ObservableData[0].Subject = "Updated Event";
        }
    }
    
    public class AppointmentData : INotifyPropertyChanged
    {
        public int Id { get; set; }
        
        private string subject;
        public string Subject
        {
            get => subject;
            set
            {
                if (subject != value)
                {
                    subject = value;
                    OnPropertyChanged(nameof(Subject));
                }
            }
        }
        
        public string Location { get; set; }
        
        private DateTime startTime;
        public DateTime StartTime
        {
            get => startTime;
            set
            {
                if (startTime != value)
                {
                    startTime = value;
                    OnPropertyChanged(nameof(StartTime));
                }
            }
        }
        
        private DateTime endTime;
        public DateTime EndTime
        {
            get => endTime;
            set
            {
                if (endTime != value)
                {
                    endTime = value;
                    OnPropertyChanged(nameof(EndTime));
                }
            }
        }
        
        public string Description { get; set; }
        public bool IsAllDay { get; set; }
        public string RecurrenceRule { get; set; }
        public string RecurrenceException { get; set; }
        public Nullable<int> RecurrenceID { get; set; }
        
        public event PropertyChangedEventHandler PropertyChanged;
        
        private void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

## Remote Data Binding

Bind data from remote services using `SfDataManager` with various adaptors. Provide the service URL to the `Url` option within `ScheduleEventSettings`.

## OData Services

Retrieve data from OData service using `SfDataManager` with OData adaptor:

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Data

<SfSchedule TValue="EventData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings TValue="EventData" Query="@QueryData">
        <SfDataManager Url="https://services.odata.org/V3/Northwind/Northwind.svc" Adaptor="Adaptors.ODataAdaptor"></SfDataManager>
    </ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public Query QueryData = new Query().From("EventDatas");
}
```

## OData v4 Services

Connect with OData v4 service endpoints using `ODataV4Adaptor` for better performance and advanced features:

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Data

<SfSchedule TValue="EventData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings TValue="EventData" Query="@QueryData">
        <SfDataManager Url="https://your-api-endpoint/api/events" Adaptor="Adaptors.ODataV4Adaptor"></SfDataManager>
    </ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public Query QueryData = new Query().From("Events");
}
```

## Server-Side Filtering

Enable server-side filtering by setting `IncludeFiltersInQuery="true"`. This filters appointments based on start date, end date, and recurrence rule, improving performance by reducing data transfer:

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@currentDate">
    <ScheduleEventSettings TValue="AppointmentData" Query="@QueryData" IncludeFiltersInQuery="true">
        <SfDataManager Url="https://your-api-endpoint/api/appointments" Adaptor="Adaptors.ODataV4Adaptor"></SfDataManager>
    </ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime currentDate = new DateTime(2026, 3, 24);
    public Query QueryData = new Query().From("Appointments");
}
```

## Web API Adaptor

Bind data from Web API using the `WebApiAdaptor`:

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@currentDate" Readonly="true">
    <ScheduleEventSettings TValue="AppointmentData">
        <SfDataManager Url="https://localhost:44308/api/appointments" Adaptor="Adaptors.WebApiAdaptor"></SfDataManager>
    </ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime currentDate = new DateTime(2026, 3, 24);
}
```

## Url Adaptor

Use `UrlAdaptor` to map CRUD operations to server-side controller actions:

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@currentDate">
    <ScheduleEventSettings TValue="AppointmentData">
        <SfDataManager 
            Url="https://localhost:44308/api/appointments"
            InsertUrl="https://localhost:44308/api/appointments"
            UpdateUrl="https://localhost:44308/api/appointments"
            RemoveUrl="https://localhost:44308/api/appointments"
            CrudUrl="https://localhost:44308/api/appointments/batch"
            Adaptor="Adaptors.UrlAdaptor">
        </SfDataManager>
    </ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime currentDate = new DateTime(2026, 3, 24);
}
```

**Url Adaptor Properties:**
- `Url` - Initial data fetch endpoint
- `InsertUrl` - Single insert operation endpoint
- `UpdateUrl` - Single update operation endpoint
- `RemoveUrl` - Single delete operation endpoint
- `CrudUrl` - Batch CRUD operations endpoint

## Custom Headers and Authentication

### Setting Custom Headers

Add custom headers to data requests using the `Headers` property:

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@currentDate" Readonly="true">
    <ScheduleEventSettings TValue="AppointmentData">
        <SfDataManager Url="https://your-api-endpoint/api/appointments" Adaptor="Adaptors.ODataV4Adaptor">
            <SfDataManagerHeaders Headers="@HeaderData"></SfDataManagerHeaders>
        </SfDataManager>
    </ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime currentDate = new DateTime(2026, 3, 24);
    
    public Dictionary<string, string> HeaderData = new Dictionary<string, string>()
    {
        { "Authorization", "Bearer YOUR_TOKEN_HERE" },
        { "Custom-Header", "Custom-Value" }
    };
}
```

### Authentication with HttpClient

Configure HttpClient with authentication before calling `AddSyncfusionBlazor()` in **Program.cs**:

```csharp
builder.Services.AddScoped(sp => new HttpClient 
{ 
    BaseAddress = new Uri(builder.HostEnvironment.BaseAddress),
    DefaultRequestHeaders = { Authorization = new AuthenticationHeaderValue("Bearer", accessToken) }
});

builder.Services.AddSyncfusionBlazor();
```

## Error Handling

Handle server-side exceptions using the `OnActionFailure` event:

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Schedule

<span class="error">@ErrorDetails</span>

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@currentDate" Readonly="true">
    <ScheduleEventSettings TValue="AppointmentData">
        <ScheduleEvents TValue="AppointmentData" OnActionFailure="OnActionFailure"></ScheduleEvents>
        <SfDataManager Url="https://your-api-endpoint/api/appointments" Adaptor="Adaptors.ODataV4Adaptor"></SfDataManager>
    </ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    .error {
        color: red;
        margin: 10px 0;
    }
</style>

@code {
    DateTime currentDate = new DateTime(2026, 3, 24);
    public string ErrorDetails = "";
    
    public void OnActionFailure(ActionFailedEventArgs args)
    {
        ErrorDetails = $"Error: {args.Error}";
        Console.WriteLine($"Action failure: {args.Error}");
    }
}
```

## Load on Demand

Implement data loading on demand by filtering appointments server-side based on start and end dates. This minimizes network data transmission:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Data

<SfSchedule TValue="AppointmentData" Width="100%" Height="600px" @bind-SelectedDate="@SelectedDate">
    <ScheduleEventSettings TValue="AppointmentData">
        <SfDataManager AdaptorInstance="@typeof(AppointmentDataAdaptor)" Adaptor="Adaptors.CustomAdaptor"></SfDataManager>
    </ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime SelectedDate { get; set; } = new DateTime(2026, 3, 24);
}
```

With custom adaptor implementing **Read** method to fetch only appointments within the requested date range:

```csharp
public class AppointmentDataAdaptor : DataAdaptor
{
    private AppointmentDataService service = new AppointmentDataService();

    public override async Task<object> Read(DataManagerRequest dm, string key = null)
    {
        var startDate = dm.StartDate ?? DateTime.MinValue;
        var endDate = dm.EndDate ?? DateTime.MaxValue;
        
        var data = await service.GetAppointmentsAsync(startDate, endDate);
        
        return dm.RequiresCounts 
            ? new DataResult() { Result = data, Count = data.Count }
            : data;
    }
}
```

## Common Patterns

**DataSource Options:**
- `List<T>` - Simple local binding
- `ObservableCollection<T>` - Automatic UI updates on collection changes
- `ExpandoObject` - Dynamic properties unknown at compile time
- `DynamicObject` - Custom dynamic behavior control
- `SfDataManager` - Remote data with various adaptors

**Remote Data Adaptors:**
- `ODataAdaptor` - Standard OData protocol
- `ODataV4Adaptor` - OData v4 with advanced features
- `WebApiAdaptor` - ASP.NET Web API
- `UrlAdaptor` - Custom endpoint URLs
- `CustomAdaptor` - Complete custom implementation

**Performance Tips:**
- Use `IncludeFiltersInQuery="true"` for large datasets
- Implement load-on-demand with date-range filtering
- Use appropriate adaptor for your backend API
- Set custom headers for authentication/authorization
