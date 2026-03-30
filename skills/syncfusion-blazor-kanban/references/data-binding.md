# Data Binding in Blazor Kanban Component

The Kanban uses `SfDataManager`, which supports both RESTful JSON data services and `IEnumerable` binding.

## Table of Contents

- [Local Data Binding](#local-data-binding)
- [ExpandoObject Binding](#binding-with-expandoobject)
- [DynamicObject Binding](#binding-with-dynamicobject)
- [Observable Collection](#binding-with-observable-collection)
- [Remote Data Binding](#remote-data-binding)
- [Complex Data Binding](#complex-data-binding)

## Local Data Binding

Assign an `IEnumerable` object to the `DataSource` property:

```cshtml
@using Syncfusion.Blazor.Kanban

<SfKanban TValue="TasksModel" KeyField="Status" DataSource="@Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="To Do" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Testing" KeyField="@(new List<string>() {"Testing"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
</SfKanban>

@code {
    public class TasksModel
    {
        public int Id { get; set; }
        public string Status { get; set; }
        public string Summary { get; set; }
    }
    public List<TasksModel> Tasks { get; set; }

    protected override void OnInitialized()
    {
        Tasks = Enumerable.Range(1, 10).Select(x => new TasksModel()
        {
            Id = 1000 + x,
            Status = (new string[] { "Open", "InProgress", "Testing", "Close" })[new Random().Next(4)],
            Summary = (new string[] { "Analyze SQL server connection.", "Fix issues in Safari browser.", "Improve application performance", "Analyze grid control." })[new Random().Next(4)],
        }).ToList();
    }
}
```

> By default, `SfDataManager` uses `BlazorAdaptor` for list data binding.

## Binding with ExpandoObject

Bind data as a list of `ExpandoObject` when the model type is unknown at compile time:

```cshtml
@using Syncfusion.Blazor.Kanban
@using System.Dynamic

<SfKanban KeyField="Status" DataSource="@Tasks">
    <KanbanColumns>
        @foreach (ColumnModel item in columnData)
        {
            <KanbanColumn HeaderText="@item.HeaderText" KeyField="@item.KeyField" AllowAdding="true"></KanbanColumn>
        }
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
</SfKanban>

@code {
    public List<ExpandoObject> Tasks { get; set; } = new List<ExpandoObject>();
    private List<ColumnModel> columnData = new List<ColumnModel>() {
        new ColumnModel(){ HeaderText= "To Do", KeyField= new List<string>() { "Open" } },
        new ColumnModel(){ HeaderText= "In Progress", KeyField= new List<string>() { "In Progress" } },
        new ColumnModel(){ HeaderText= "Testing", KeyField= new List<string>() { "Testing" } },
        new ColumnModel(){ HeaderText= "Done", KeyField=new List<string>() { "Close" } }
    };
    protected override void OnInitialized()
    {
        Tasks = Enumerable.Range(1, 20).Select((x) =>
        {
            dynamic d = new ExpandoObject();
            d.Id = "Task 1000" + x;
            d.Status = (new string[] { "Open", "In Progress", "Testing", "Close" })[new Random().Next(4)];
            d.Summary = (new string[] { "Analyze the new requirements.", "Improve application performance", "Fix the issues reported in the IE browser.", "Validate new requirements" })[new Random().Next(4)];
            d.Assignee = (new string[] { "Nancy Davloio", "Andrew Fuller", "Janet Leverling", "Steven walker" })[new Random().Next(4)];
            return d;
        }).Cast<ExpandoObject>().ToList<ExpandoObject>();
    }
}
```

## Binding with DynamicObject

Override `GetDynamicMemberNames` to perform data operations and editing:

```cshtml
@using Syncfusion.Blazor.Kanban
@using System.Dynamic

<SfKanban KeyField="Status" DataSource="@Tasks">
    <KanbanColumns>
        @foreach (ColumnModel item in columnData)
        {
            <KanbanColumn HeaderText="@item.HeaderText" KeyField="@item.KeyField" AllowAdding="true"></KanbanColumn>
        }
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
</SfKanban>

@code {
    private List<ColumnModel> columnData = new List<ColumnModel>() {
        new ColumnModel(){ HeaderText= "To Do", KeyField= new List<string>() { "Open" } },
        new ColumnModel(){ HeaderText= "In Progress", KeyField= new List<string>() { "In Progress" } },
        new ColumnModel(){ HeaderText= "Testing", KeyField= new List<string>() { "Testing" } },
        new ColumnModel(){ HeaderText= "Done", KeyField=new List<string>() { "Close" } }
    };
    public List<DynamicDictionary> Tasks = new List<DynamicDictionary>() { };
    protected override void OnInitialized()
    {
        Tasks = Enumerable.Range(1, 20).Select((x) =>
        {
            dynamic d = new DynamicDictionary();
            d.Id = "Task 1000" + x;
            d.Status = (new string[] { "Open", "In Progress", "Testing", "Close" })[new Random().Next(4)];
            d.Summary = (new string[] { "Analyze the new requirements.", "Improve application performance", "Fix the issues reported in the IE browser." })[new Random().Next(3)];
            return d;
        }).Cast<DynamicDictionary>().ToList<DynamicDictionary>();
    }
    public class DynamicDictionary : System.Dynamic.DynamicObject
    {
        Dictionary<string, object> dictionary = new Dictionary<string, object>();
        public override bool TryGetMember(GetMemberBinder binder, out object result)
        {
            string name = binder.Name;
            return dictionary.TryGetValue(name, out result);
        }
        public override bool TrySetMember(SetMemberBinder binder, object value)
        {
            dictionary[binder.Name] = value;
            return true;
        }
        public override System.Collections.Generic.IEnumerable<string> GetDynamicMemberNames()
        {
            return this.dictionary?.Keys;
        }
    }
}
```

## Binding with Observable Collection

Use `ObservableCollection` for real-time UI updates when items are added, removed, or moved:

```razor
@using Syncfusion.Blazor.Kanban
@using System.Collections.ObjectModel;
@using System.ComponentModel;

<SfKanban KeyField="Status" DataSource="@ObservableData">
    <KanbanColumns>
        @foreach (ColumnModel item in columnData)
        {
            <KanbanColumn HeaderText="@item.HeaderText" KeyField="@item.KeyField" AllowAdding="true" />
        }
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary" />
</SfKanban>

@code {
    private List<ColumnModel> columnData = new List<ColumnModel>() {
        new ColumnModel(){ HeaderText= "To Do", KeyField= new List<string>() { "Open" } },
        new ColumnModel(){ HeaderText= "In Progress", KeyField= new List<string>() { "In Progress" } },
        new ColumnModel(){ HeaderText= "Testing", KeyField= new List<string>() { "Testing" } },
        new ColumnModel(){ HeaderText= "Done", KeyField=new List<string>() { "Close" } }
    };
    public ObservableCollection<ObservableDatas> ObservableData { get; set; }

    protected override void OnInitialized()
    {
        var tasks = Enumerable.Range(1, 20).Select(x => new ObservableDatas()
        {
            Id = "Task 1000" + x,
            Status = (new string[] { "Open", "In Progress", "Testing", "Close" })[new Random().Next(4)],
            Summary = "Task summary " + x,
            Assignee = (new string[] { "Nancy Davloio", "Andrew Fuller", "Janet Leverling" })[new Random().Next(3)],
        }).ToList();
        ObservableData = new ObservableCollection<ObservableDatas>(tasks);
    }

    public class ObservableDatas : INotifyPropertyChanged
    {
        public string Id { get; set; }
        private string status { get; set; }
        public string Status
        {
            get { return status; }
            set
            {
                this.status = value;
                NotifyPropertyChanged("Status");
            }
        }
        public string Summary { get; set; }
        public string Assignee { get; set; }
        public event PropertyChangedEventHandler PropertyChanged;
        private void NotifyPropertyChanged(string propertyName)
        {
            var handler = PropertyChanged;
            if (handler != null)
                handler(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

## Remote Data Binding

### OData Service

```cshtml
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Kanban

<SfKanban TValue="Order" KeyField="ShipCountry" AllowDragAndDrop="false">
    <SfDataManager Url="https://js.syncfusion.com/ejServices/Wcf/Northwind.svc/Orders" Adaptor="@Syncfusion.Blazor.Adaptors.ODataAdaptor"></SfDataManager>
    <KanbanColumns>
        <KanbanColumn HeaderText="Denmark" KeyField="@(new List<string>() { "Denmark" })"></KanbanColumn>
        <KanbanColumn HeaderText="Brazil" KeyField="@(new List<string>() { "Brazil" })"></KanbanColumn>
        <KanbanColumn HeaderText="Germany" KeyField="@(new List<string>() { "Germany" })"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="OrderID" ContentField="ShipName"></KanbanCardSettings>
    <KanbanEvents TValue="Order" DialogOpen="@((args)=> { args.Cancel = true; })"></KanbanEvents>
</SfKanban>

@code {
    public class Order
    {
        public int? OrderID { get; set; }
        public string ShipName { get; set; }
        public string ShipCountry { get; set; }
    }
}
```

### Web API

```cshtml
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Kanban

<SfKanban TValue="TasksModel" KeyField="Status" AllowDragAndDrop="false">
    <SfDataManager Url="https://blazor.syncfusion.com/services/production/api/Kanban" Adaptor="@Syncfusion.Blazor.Adaptors.WebApiAdaptor"></SfDataManager>
    <KanbanColumns>
        <KanbanColumn HeaderText="To Do" KeyField="@(new List<string>() { "Open" })"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() { "InProgress" })"></KanbanColumn>
        <KanbanColumn HeaderText="Testing" KeyField="@(new List<string>() { "Testing" })"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() { "Close" })"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
    <KanbanEvents TValue="TasksModel" DialogOpen="@((args) => { args.Cancel = true; })"></KanbanEvents>
</SfKanban>

@code {
    public class TasksModel
    {
        public int Id { get; set; }
        public string Status { get; set; }
        public string Assignee { get; set; }
        public string Summary { get; set; }
    }
}
```

### Sending Additional Parameters

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" AllowDragAndDrop="false" Query=@KanbanQuery>
    <SfDataManager Url="https://blazor.syncfusion.com/services/production/api/Kanban" Adaptor="@Syncfusion.Blazor.Adaptors.WebApiAdaptor"></SfDataManager>
    ...
</SfKanban>

@code {
    public Query KanbanQuery { get; set; }

    protected override void OnInitialized()
    {
        KanbanQuery = new Query().AddParams("BlazorKanban", "true");
    }
}
```

## Complex Data Binding

Map nested properties to Kanban fields using dot notation:

```razor
<SfKanban TValue="SwimlaneTasksModel" KeyField="Status.KeyField" DataSource="KanbanSwimlaneTasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})" AllowAdding="true" />
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})" />
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})" />
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id.HeaderId" ContentField="Summary.Content" />
    <KanbanSwimlaneSettings KeyField="AssigneeName.Name" AllowDragAndDrop="true" />
</SfKanban>

@code {
    public class SwimlaneTasksModel
    {
        public HeaderModel Id { get; set; }
        public StatusModel Status { get; set; }
        public ContentModel Summary { get; set; }
        public SwimlaneAssignee AssigneeName { get; set; }
    }
    public class StatusModel { public string KeyField { get; set; } }
    public class HeaderModel { public int HeaderId { get; set; } }
    public class ContentModel { public string Content { get; set; } }
    public class SwimlaneAssignee { public string Name { get; set; } }
}
```
