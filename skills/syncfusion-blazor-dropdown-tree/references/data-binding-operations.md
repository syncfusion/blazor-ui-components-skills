# Data Binding and Operations

## Table of Contents
- [Local Data Binding](#local-data-binding)
- [Hierarchical Data](#hierarchical-data)
- [Self-Referential Data](#self-referential-data)
- [ExpandoObject Binding](#expandoobject-binding)
- [DynamicObject Binding](#dynamicobject-binding)
- [Remote Data Binding](#remote-data-binding)
- [OData Adaptor](#odata-adaptor)
- [OData V4 Adaptor](#odata-v4-adaptor)
- [Web API Adaptor](#web-api-adaptor)
- [Entity Framework Integration](#entity-framework-integration)
- [Observable Collections](#observable-collections)
- [Load On Demand](#load-on-demand)
- [Dynamic Data Operations](#dynamic-data-operations)
- [GetTreeViewData Method](#gettreeviewdata-method)

## Local Data Binding

Bind to a list of local objects using the `DataSource` property of `DropDownTreeField`:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" Placeholder="Select an employee" Width="500px">
    <DropDownTreeField TItem="EmployeeData" 
        DataSource="Data" 
        ID="Id" 
        Text="Name" 
        HasChildren="HasChild" 
        ParentID="PId">
    </DropDownTreeField>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", Job = "General Manager", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", Job = "Product Manager", HasChild = true },
        new EmployeeData() { Id = "3", PId = "2", Name = "Andrew Fuller", Job = "Team Lead", HasChild = true },
    };

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public string? Job { get; set; }
        public bool HasChild { get; set; }
        public bool Expanded { get; set; }
        public string? PId { get; set; }
    }
}
```

## Hierarchical Data

Bind data with nested child collections:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TValue="string" TItem="MailItem" Placeholder="Select a Folder" Width="500px">
    <DropDownTreeField TItem="MailItem" 
        ID="Id" 
        Text="FolderName" 
        Child="SubFolders" 
        DataSource="@MyFolder" 
        Expanded="Expanded">
    </DropDownTreeField>
</SfDropDownTree>

@code {
    public class MailItem
    {
        public string? Id { get; set; }
        public string? FolderName { get; set; }
        public bool Expanded { get; set; }
        public List<MailItem>? SubFolders { get; set; }
    }

    List<MailItem> MyFolder = new List<MailItem>()
    {
        new MailItem
        {
            Id = "01",
            FolderName = "Inbox",
            SubFolders = new List<MailItem>()
            {
                new MailItem
                {
                    Id = "01-01",
                    FolderName = "Categories",
                    SubFolders = new List<MailItem>()
                    {
                        new MailItem { Id = "01-02", FolderName = "Primary" },
                        new MailItem { Id = "01-03", FolderName = "Social" },
                        new MailItem { Id = "01-04", FolderName = "Promotions" }
                    }
                }
            }
        },
        new MailItem
        {
            Id = "02",
            FolderName = "Others",
            Expanded = true,
            SubFolders = new List<MailItem>()
            {
                new MailItem { Id = "02-01", FolderName = "Sent Items" },
                new MailItem { Id = "02-02", FolderName = "Delete Items" },
                new MailItem { Id = "02-03", FolderName = "Drafts" }
            }
        }
    };
}
```

## Self-Referential Data

Bind data with parent-child relationships using ParentID:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TValue="string" TItem="MailItem" Placeholder="Select a Folder" Width="500px">
    <DropDownTreeField TItem="MailItem" 
        ID="Id" 
        DataSource="@MyFolder" 
        Text="FolderName" 
        ParentID="ParentId" 
        HasChildren="HasSubFolders" 
        Expanded="Expanded">
    </DropDownTreeField>
</SfDropDownTree>

@code {
    List<MailItem> MyFolder = new List<MailItem>
    {
        new MailItem { Id = "1", FolderName = "Inbox", HasSubFolders = true, Expanded = false },
        new MailItem { Id = "2", ParentId = "1", FolderName = "Categories", HasSubFolders = true, Expanded = false },
        new MailItem { Id = "3", ParentId = "2", FolderName = "Primary", HasSubFolders = false },
        new MailItem { Id = "4", ParentId = "2", FolderName = "Social", HasSubFolders = false },
        new MailItem { Id = "5", ParentId = "2", FolderName = "Promotions", HasSubFolders = false },
        new MailItem { Id = "6", FolderName = "Others", HasSubFolders = true, Expanded = true },
        new MailItem { Id = "7", ParentId = "6", FolderName = "Sent Items", HasSubFolders = false },
        new MailItem { Id = "8", ParentId = "6", FolderName = "Delete Items", HasSubFolders = false },
        new MailItem { Id = "9", ParentId = "6", FolderName = "Drafts", HasSubFolders = false },
        new MailItem { Id = "10", ParentId = "6", FolderName = "Archive", HasSubFolders = false }
    };

    public class MailItem
    {
        public string? Id { get; set; }
        public string? ParentId { get; set; }
        public string? FolderName { get; set; }
        public bool Expanded { get; set; }
        public bool HasSubFolders { get; set; }
    }
}
```

## ExpandoObject Binding

Bind to dynamic objects without compile-time type knowledge:

```razor
@using Syncfusion.Blazor.Navigations
@using System.Dynamic

<SfDropDownTree TValue="string" TItem="ExpandoObject" Placeholder="Select a Item">
    <DropDownTreeField TItem="ExpandoObject" 
        ID="ID" 
        DataSource="@TreeData" 
        Text="Name" 
        ParentID="ParentID" 
        HasChildren="HasChildren" 
        Expanded="Expanded">
    </DropDownTreeField>
</SfDropDownTree>

@code {
    public List<ExpandoObject>? TreeData { get; set; }

    protected override void OnInitialized()
    {
        this.TreeData = GetData().ToList();
    }

    public static List<ExpandoObject> Data = new List<ExpandoObject>();
    public static int ParentRecordID { get; set; }
    public static int ChildRecordID { get; set; }

    public static List<ExpandoObject> GetData()
    {
        Data.Clear();
        ParentRecordID = 0;
        ChildRecordID = 0;

        for (var i = 1; i <= 3; i++)
        {
            dynamic ParentRecord = new ExpandoObject();
            ParentRecord.ID = ++ParentRecordID;
            ParentRecord.Name = "Parent " + i;
            ParentRecord.ParentID = null;
            ParentRecord.Expanded = true;
            ParentRecord.HasChildren = true;
            Data.Add(ParentRecord);
            AddChildRecords(ParentRecordID);
        }
        return Data;
    }

    public static void AddChildRecords(int ParentId)
    {
        for (var i = 1; i < 3; i++)
        {
            dynamic ChildRecord = new ExpandoObject();
            ChildRecord.ID = ++ParentRecordID;
            ChildRecord.Name = "Child item" + ++ChildRecordID;
            ChildRecord.ParentID = ParentId;
            Data.Add(ChildRecord);
        }
    }
}
```

## DynamicObject Binding

Bind to DynamicObject implementations:

```razor
@using Syncfusion.Blazor.Navigations
@using System.Dynamic

<SfDropDownTree TValue="string" TItem="DynamicDictionary" Placeholder="Select a Item">
    <DropDownTreeField TItem="DynamicDictionary" 
        ID="ID" 
        DataSource="@TreeData" 
        Text="Name" 
        ParentID="ParentID" 
        HasChildren="HasChildren" 
        Expanded="Expanded">
    </DropDownTreeField>
</SfDropDownTree>

@code {
    public List<DynamicDictionary>? TreeData { get; set; }
    
    protected override void OnInitialized()
    {
        this.TreeData = GetData().ToList();
    }

    public static List<DynamicDictionary> Data = new List<DynamicDictionary>();
    public static int ParentRecordID { get; set; }
    public static int ChildRecordID { get; set; }

    public static List<DynamicDictionary> GetData()
    {
        Data.Clear();
        ParentRecordID = 0;
        ChildRecordID = 0;
        for (var i = 1; i <= 3; i++)
        {
            dynamic ParentRecord = new DynamicDictionary();
            ParentRecord.ID = ++ParentRecordID;
            ParentRecord.Name = "Parent " + i;
            ParentRecord.ParentID = null;
            ParentRecord.Expanded = true;
            ParentRecord.HasChildren = true;
            Data.Add(ParentRecord);
            AddChildRecords(ParentRecordID);
        }
        return Data;
    }

    public static void AddChildRecords(int ParentId)
    {
        for (var i = 1; i < 3; i++)
        {
            dynamic ChildRecord = new DynamicDictionary();
            ChildRecord.ID = ++ParentRecordID;
            ChildRecord.Name = "Child Item " + ++ChildRecordID;
            ChildRecord.ParentID = ParentId;
            Data.Add(ChildRecord);
        }
    }

    public class DynamicDictionary : DynamicObject
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

## Remote Data Binding

Bind to remote services using `DataManager` and `Query` properties. The DataManager component enables interaction with remote data sources.

### DataManager Configuration

**Key properties:**
- `Url`: Service endpoint to fetch data
- `Adaptor`: Data adaptor type (ODataAdaptor, ODataV4Adaptor, WebApiAdaptor, etc.)
- `CrossDomain`: Allow cross-domain requests

## OData Adaptor

Connect to OData services:

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Data

<SfDropDownTree TValue="int?" TItem="TreeData" Placeholder="Select an employee" Width="500px">
    <DropDownTreeField TItem="TreeData" 
        Query="@Query" 
        ID="EmployeeID" 
        Text="FirstName" 
        ParentID="ReportsTo" 
        HasChildren="HasChildren">
        <SfDataManager Url="url" 
            Adaptor="@Syncfusion.Blazor.Adaptors.ODataAdaptor" 
            CrossDomain="true">
        </SfDataManager>
    </DropDownTreeField>
</SfDropDownTree>

@code {
    public Query Query = new Query();
    
    public class TreeData
    {
        public string EmployeeID { get; set; }
        public string ReportsTo { get; set; }
        public string FirstName { get; set; }
        public string Designation { get; set; }
        public string Country { get; set; }
        public bool HasChildren { get; set; }
    }
}
```

## OData V4 Adaptor

Connect to OData V4 services:

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Data

<SfDropDownTree TValue="int?" TItem="TreeData" Placeholder="Select an employee" Width="500px">
    <DropDownTreeField TItem="TreeData" 
        Query="@Query" 
        ID="EmployeeID" 
        Text="FirstName" 
        ParentID="ReportsTo" 
        HasChildren="HasChildren">
        <SfDataManager Url="url" 
            Adaptor="@Syncfusion.Blazor.Adaptors.ODataV4Adaptor" 
            CrossDomain="true">
        </SfDataManager>
    </DropDownTreeField>
</SfDropDownTree>

@code {
    public Query Query = new Query();
    
    public class TreeData
    {
        public string EmployeeID { get; set; }
        public string ReportsTo { get; set; }
        public string FirstName { get; set; }
        public string Designation { get; set; }
        public string Country { get; set; }
        public bool HasChildren { get; set; }
    }
}
```

## Web API Adaptor

Connect to custom Web API endpoints:

### Razor Component

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Data

<SfDropDownTree TValue="int?" TItem="NodeResult" Placeholder="Select an employee" Width="500px">
    <DropDownTreeField TItem="NodeResult" 
        Query="@TreeViewQuery" 
        ID="ProductID" 
        Text="ProductName" 
        ParentID="pid" 
        HasChildren="haschild">
        <SfDataManager Url="api/Nodes" 
            CrossDomain="true" 
            Adaptor="Syncfusion.Blazor.Adaptors.WebApiAdaptor">
        </SfDataManager>
    </DropDownTreeField>
</SfDropDownTree>

@code {
    public Query TreeViewQuery = new Query();

    public class NodeResult
    {
        public int? ProductID { get; set; }
        public string? ProductName { get; set; }
        public int? pid { get; set; }
        public bool haschild { get; set; }
    }
}
```

### Web API Controller

```csharp
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using System.Linq;

namespace DropDownTreeSample.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class NodesController : ControllerBase
    {
        [HttpGet]
        public IEnumerable<NodeResult> Get()
        {
            List<NodeResult> localData = new List<NodeResult>()
            {
                new NodeResult { ProductID = 1, ProductName = "Parent", pid = null, haschild = true },
                new NodeResult { ProductID = 2, ProductName = "Child1", pid = 1, haschild = false },
                new NodeResult { ProductID = 3, ProductName = "Child2", pid = 1, haschild = true },
                new NodeResult { ProductID = 4, ProductName = "Child3", pid = 1, haschild = false },
                new NodeResult { ProductID = 5, ProductName = "SubChild1", pid = 3, haschild = false },
                new NodeResult { ProductID = 6, ProductName = "SubChild2", pid = 3, haschild = false },
            };
            var data = localData.ToList();
            return data;
        }

        public class NodeResult
        {
            public int? ProductID { get; set; }
            public string ProductName { get; set; }
            public int? pid { get; set; }
            public bool haschild { get; set; }
        }
    }
}
```

## Entity Framework Integration

### Step 1: Create DBContext Class

```csharp
using Microsoft.EntityFrameworkCore;

namespace DBTree.Data
{
    public class AppDBContext : DbContext
    {
        public AppDBContext()
        {
        }

        public AppDBContext(DbContextOptions<AppDBContext> options)
            : base(options)
        {
        }

        public DbSet<Employee> Employees { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            if (!optionsBuilder.IsConfigured)
            {
                optionsBuilder.UseSqlServer("Server=(localdb)\\MSSQLLocalDB;Database=DBTree;Integrated Security=True");
            }
        }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Employee>().HasData(
                new Employee() { Id = 1, Name = "Steven Buchanan", Job = "General Manager", HasChild = true, Expanded = true },
                new Employee() { Id = 2, PId = 1, Name = "Laura Callahan", Job = "Product Manager", HasChild = true },
                new Employee() { Id = 3, PId = 2, Name = "Andrew Fuller", Job = "Team Lead", HasChild = true },
                new Employee() { Id = 4, PId = 3, Name = "Anne Dodsworth", Job = "Developer" },
                new Employee() { Id = 5, PId = 1, Name = "Nancy Davolio", Job = "Product Manager", HasChild = true }
            );
        }
    }
}
```

### Step 2: Create Data Access Layer

```csharp
using Microsoft.EntityFrameworkCore;

namespace DBTree.Data
{
    public class EmployeeDataAccessLayer
    {
        AppDBContext db = new();

        public DbSet<Employee> GetAllEmployees()
        {
            try
            {
                return db.Employees;
            }
            catch
            {
                throw;
            }
        }
    }
}
```

### Step 3: Create Web API Controller

```csharp
using DBTree.Data;
using Microsoft.AspNetCore.Mvc;

namespace DBTree.Controller
{
    [Route("api/[controller]")]
    [ApiController]
    public class DefaultController : ControllerBase
    {
        EmployeeDataAccessLayer db = new EmployeeDataAccessLayer();
        
        [HttpGet("{id}")]
        public object Get()
        {
            var data = db.GetAllEmployees().ToList();
            return data;
        }
    }
}
```

### Step 4: Configure Dropdown Tree Component

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Data

<SfDropDownTree TValue="int" TItem="Employee" Placeholder="Select a Employee">
    <DropDownTreeField TItem="Employee" ID="Id" Text="Name" ParentID="PId" HasChildren="HasChild" Expanded="Expanded">
        <SfDataManager Url="api/Default" Adaptor="Adaptors.WebApiAdaptor" CrossDomain="true"></SfDataManager>
    </DropDownTreeField>
</SfDropDownTree>

@code {
    public class Employee
    {
        public int Id { get; set; }
        public int? PId { get; set; }
        public string Name { get; set; }
        public string Job { get; set; }
        public bool HasChild { get; set; }
        public bool Expanded { get; set; }
    }
}
```

## Observable Collections

Support dynamic collections with `INotifyCollectionChanged` notifications:

```razor
@using Syncfusion.Blazor.Navigations
@using System.Collections.ObjectModel

<SfDropDownTree TValue="string" TItem="ObservableDatas" Placeholder="Select a Item">
    <DropDownTreeField TItem="ObservableDatas" 
        DataSource="@ObservableData" 
        ID="@nameof(ObservableDatas.Id)" 
        Child="@nameof(ObservableDatas.Children)" 
        Text="@nameof(ObservableDatas.Name)" 
        HasChildren="@nameof(ObservableDatas.HasChild)" 
        Expanded="@nameof(ObservableDatas.Expanded)">
    </DropDownTreeField>
</SfDropDownTree>

@code {
    public ObservableCollection<ObservableDatas> ObservableData { get; set; }

    protected override void OnInitialized()
    {
        ObservableData = ObservableDatas.GetRecords();
    }

    public class ObservableDatas : INotifyPropertyChanged
    {
        public List<ObservableDatas> Children { get; set; }
        public string Id { get; set; }

        private string _name { get; set; }
        public string Name
        {
            get => _name;
            set
            {
                _name = value;
                NotifyPropertyChanged();
            }
        }

        public bool HasChild { get; set; }

        private bool _expanded = false;
        public bool Expanded
        {
            get => _expanded;
            set
            {
                _expanded = value;
                NotifyPropertyChanged();
            }
        }

        public event PropertyChangedEventHandler PropertyChanged;
        private void NotifyPropertyChanged([CallerMemberName] string propertyName = "")
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }

        public static ObservableCollection<ObservableDatas> GetRecords()
        {
            // Return your observable collection
            return new ObservableCollection<ObservableDatas>();
        }
    }
}
```

## Load On Demand

Lazy-load child nodes only when parent is expanded:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    LoadOnDemand="true">
    <DropDownTreeField TItem="EmployeeData" 
        DataSource="Data" 
        ID="Id" 
        Text="Name" 
        HasChildren="HasChild" 
        ParentID="PId">
    </DropDownTreeField>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", HasChild = true },
        new EmployeeData() { Id = "3", PId = "2", Name = "Andrew Fuller", HasChild = true },
    };

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
        public bool Expanded { get; set; }
        public string? PId { get; set; }
    }
}
```

## Dynamic Data Operations

### Adding New Items

Dynamically add items to the data source:

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="AddData">Add Data</SfButton>
<SfDropDownTree TItem="EmployeeData" TValue="string" Placeholder="Select an employee" Width="500px" LoadOnDemand="true">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", HasChild = true },
    };

    void AddData()
    {
        Data.Add(new EmployeeData() { Id = "3", PId = "2", Name = "Jack", HasChild = false });
    }

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
        public bool Expanded { get; set; }
        public string? PId { get; set; }
    }
}
```

## GetTreeViewData Method

Retrieve complete node details or specific node information by ID:

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="GetData">Get TreeData</SfButton>

<SfDropDownTree @ref="tree" TItem="EmployeeData" TValue="string" Placeholder="Select an employee" Width="500px">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

<span>@EmployeeList</span>

@code {
    SfDropDownTree<string, EmployeeData>? tree;
    
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", Job = "General Manager", HasChild = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", Job = "Product Manager", HasChild = true },
        new EmployeeData() { Id = "6", PId = "2", Name = "Michael Suyama", Job = "Team Lead", HasChild = true },
        new EmployeeData() { Id = "7", PId = "6", Name = "Robert King", Job = "Developer" },
        new EmployeeData() { Id = "11", PId = "6", Name = "Mary", Job = "Developer" },
    };
    
    string EmployeeList { get; set; } = "";

    public void GetData()
    {
        // Get node information for ID "11"
        List<EmployeeData> employees = tree.GetTreeViewData("11");
        
        // Concatenate employee details
        EmployeeList = string.Join(", ", employees.Select(e => $"{e.Name} ({e.Job})"));
    }

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public string? Job { get; set; }
        public bool HasChild { get; set; }
        public bool Expanded { get; set; }
        public string? PId { get; set; }
    }
}
```

**Method Signature:**
```csharp
public List<TItem> GetTreeViewData(string nodeId)
```

Returns a list of node objects matching the given ID. If no ID is provided, returns all tree nodes.
