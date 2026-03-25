# Data Binding in TreeView

## Table of Contents
- [Overview](#overview)
- [Local Data Binding](#local-data-binding)
- [Remote Data Binding](#remote-data-binding)
- [Load on Demand](#load-on-demand)
- [Property Mappings](#property-mappings)
- [Common Patterns](#common-patterns)

## Overview

The TreeView component supports multiple data binding methods through the `DataSource` property. The component can bind:
- **Local data**: Hierarchical objects or self-referential lists
- **Remote data**: Web APIs, OData services, or HTTP endpoints
- **Mixed approach**: Local + remote (load on demand)

The `TreeViewFieldsSettings` component maps your data properties to TreeView fields:
- `Id` - Unique identifier for each node
- `Text` - Display text for node
- `ParentID` - Parent node reference (self-referential)
- `Child` - Child nodes collection (hierarchical)
- `HasChildren` - Whether node has children
- `Expanded` - Initial expand state
- `IsChecked` - Checkbox state (if enabled)

## Local Data Binding

### Hierarchical Data

Hierarchical data uses nested lists where each parent has a `Child` property containing child nodes:

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="MailItem">
    <TreeViewFieldsSettings TValue="MailItem"
        Id="Id"
        Text="FolderName"
        Child="SubFolders"
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    public class MailItem
    {
        public string? Id { get; set; }
        public string? FolderName { get; set; }
        public List<MailItem>? SubFolders { get; set; }
    }

    List<MailItem> MyFolder = new();

    protected override void OnInitialized()
    {
        var folder1 = new List<MailItem>
        {
            new MailItem { Id = "1-1", FolderName = "Important" },
            new MailItem { Id = "1-2", FolderName = "Drafts" }
        };

        MyFolder.Add(new MailItem
        {
            Id = "1",
            FolderName = "Inbox",
            SubFolders = folder1
        });
    }
}
```

**Hierarchical Data Benefits:**
- Natural nested structure
- Easy to understand parent-child relationships
- Observable collection support for dynamic updates

**Hierarchical Data Limitations:**
- Cannot easily include root-level items alongside parents
- Requires separate handling for flat and nested structures

### Self-Referential Data

Self-referential data uses a flat list where each item references its parent via `ParentID`:

```csharp
@using Syncfusion.Blazor.Navigations

<SfTreeView TValue="MailItem">
    <TreeViewFieldsSettings TValue="MailItem"
        Id="Id"
        Text="FolderName"
        ParentID="ParentId"
        HasChildren="HasSubFolders"
        DataSource="@MyFolder">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    public class MailItem
    {
        public string? Id { get; set; }
        public string? ParentId { get; set; }  // null for root items
        public string? FolderName { get; set; }
        public bool HasSubFolders { get; set; }
    }

    List<MailItem> MyFolder = new()
    {
        // Root level items (ParentId = null)
        new MailItem { Id = "1", FolderName = "Inbox", HasSubFolders = true, ParentId = null },
        new MailItem { Id = "2", FolderName = "Sent", HasSubFolders = false, ParentId = null },

        // Child items (ParentId references parent Id)
        new MailItem { Id = "1-1", FolderName = "Important", ParentId = "1" },
        new MailItem { Id = "1-2", FolderName = "Drafts", ParentId = "1" }
    };
}
```

**Self-Referential Data Benefits:**
- Flexible structure (can have both root and nested items)
- Easier for database mapping
- Simpler dynamic updates
- Better for large datasets

**When to use:** API responses, database queries, complex hierarchies

## Remote Data Binding

The `DataSource` property accepts both `List<T>` and `SfDataManager` instances. When using remote data, assign a configured `SfDataManager` to the `DataSource` property within `TreeViewFieldsSettings`:

```csharp
<SfTreeView TValue="EmployeeData">
    <TreeViewFieldsSettings TValue="EmployeeData"
        DataSource="@DataManager">  <!-- SfDataManager assigned here -->
    </TreeViewFieldsSettings>
</SfTreeView>
```

### Web API with DataManager

```csharp
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Data

<SfTreeView TValue="EmployeeData">
    <TreeViewFieldsSettings TValue="EmployeeData"
        Id="Id"
        Text="Name"
        ParentID="ParentId"
        HasChildren="HasChild"
        DataSource="@WebApiManager">    <!-- SfDataManager instance -->
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? ParentId { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
    }

    SfDataManager WebApiManager;

    protected override void OnInitialized()
    {
        // Configure DataManager with Web API endpoint
        WebApiManager = new SfDataManager
        {
            Url = "https://api.example.com/employees",
            Adaptor = Syncfusion.Blazor.Adaptors.AdaptorType.WebApiAdaptor
        };
    }
}
```

**Key Points:**
- `DataSource` property receives the `SfDataManager` instance (placed inside `TreeViewFieldsSettings`)
- `Adaptor` specifies the data service type (WebApiAdaptor for Web APIs)
- `Url` is the remote endpoint
- Hierarchical data is auto-constructed from ParentID relationships

### OData Service Binding

```csharp
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Data

<SfTreeView TValue="EmployeeData">
    <TreeViewFieldsSettings TValue="EmployeeData"
        Id="EmployeeID"
        Text="FirstName"
        HasChildren="EmployeeID"
        DataSource="@ODataManager"
        Query="@ODataQuery">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    public class EmployeeData
    {
        public string? EmployeeID { get; set; }
        public string? FirstName { get; set; }
    }

    SfDataManager ODataManager;
    Query ODataQuery;

    protected override void OnInitialized()
    {
        ODataManager = new SfDataManager
        {
            Url = "https://services.odata.org/v4/northwind/northwind.svc/Employees",
            Adaptor = Syncfusion.Blazor.Adaptors.AdaptorType.ODataV4Adaptor
        };
        ODataQuery = new Query()
            .Select(new List<string> { "EmployeeID", "FirstName" })
            .Take(5);
    }
}
```

### OData V4 Service

```csharp
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Data

<SfTreeView TValue="EmployeeData">
    <TreeViewFieldsSettings TValue="EmployeeData"
        Id="EmployeeID"
        Text="FirstName"
        HasChildren="EmployeeID"
        DataSource="@ODataV4Manager"
        Query="@ODataV4Query">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    public class EmployeeData
    {
        public string? EmployeeID { get; set; }
        public string? FirstName { get; set; }
    }

    SfDataManager ODataV4Manager;
    Query ODataV4Query;

    protected override void OnInitialized()
    {
        ODataV4Manager = new SfDataManager
        {
            Url = "https://services.odata.org/v4/northwind/northwind.svc/Employees",
            Adaptor = Syncfusion.Blazor.Adaptors.AdaptorType.ODataV4Adaptor
        };
        ODataV4Query = new Query()
            .Select(new List<string> { "EmployeeID", "FirstName" })
            .RequiresCount();
    }
}
```

## Load on Demand

The `LoadOnDemand` property (default: `true`) optimizes performance by loading child nodes only when parent is expanded.

### Enable Load on Demand (Default)

```csharp
// By default, only root nodes load initially
<SfTreeView TValue="EmployeeData" LoadOnDemand="true">
    <TreeViewFieldsSettings TValue="EmployeeData"
        Id="Id"
        Text="Name"
        ParentID="ParentId"
        HasChildren="HasChild"
        DataSource="@Employees">
    </TreeViewFieldsSettings>
</SfTreeView>
```

**Benefits:**
- Faster initial render
- Lower bandwidth for large datasets
- Smooth user experience

### Disable Load on Demand

```csharp
// All nodes load at initialization
<SfTreeView TValue="EmployeeData" LoadOnDemand="false">
    <TreeViewFieldsSettings TValue="EmployeeData"
        Id="Id"
        Text="Name"
        Child="Children"
        DataSource="@Employees">
    </TreeViewFieldsSettings>
</SfTreeView>
```

**Use when:** Small datasets or all data must be available immediately

## Property Mappings

### Complete Field Mapping Example

```csharp
<TreeViewFieldsSettings TValue="EmployeeData"
    Id="EmployeeId"              // Unique identifier
    Text="EmployeeName"          // Display text
    ParentID="ReportsTo"         // Parent reference
    HasChildren="IsManager"      // Has children flag
    Expanded="InitialExpanded"   // Expand state
    IsChecked="InitialChecked"   // Checkbox state
    ImageUrl="ProfileImage"      // Icon/image URL
    Tooltip="EmployeeTitle"      // Hover tooltip
    DataSource="@EmployeeList">
</TreeViewFieldsSettings>

@code {
    public class EmployeeData
    {
        public string? EmployeeId { get; set; }
        public string? EmployeeName { get; set; }
        public string? ReportsTo { get; set; }
        public bool IsManager { get; set; }
        public bool InitialExpanded { get; set; }
        public bool? InitialChecked { get; set; }
        public string? ProfileImage { get; set; }
        public string? EmployeeTitle { get; set; }
    }
}
```

### Hierarchical Data Mapping

```csharp
<TreeViewFieldsSettings TValue="OrganizationNode"
    Id="NodeId"
    Text="NodeName"
    Child="Departments"      // Nested list property
    DataSource="@Organization">
</TreeViewFieldsSettings>

@code {
    public class OrganizationNode
    {
        public string? NodeId { get; set; }
        public string? NodeName { get; set; }
        public List<OrganizationNode>? Departments { get; set; }
    }
}
```

## DataBound Event

The `DataBound` event fires after all data is populated:

```csharp
<SfTreeView TValue="MailItem">
    <TreeViewFieldsSettings TValue="MailItem" DataSource="@MyFolder" />
    <TreeViewEvents TValue="MailItem" DataBound="OnDataBound"></TreeViewEvents>
</SfTreeView>

@code {
    void OnDataBound()
    {
        // Perform post-binding operations
        // - Expand specific nodes
        // - Select default node
        // - Apply conditional formatting
    }
}
```

## Common Patterns

### Pattern 1: Database Query Result

```csharp
protected override async Task OnInitializedAsync()
{
    // Fetch from database via API
    var response = await HttpClient.GetAsync("/api/departments");
    MyFolder = await response.Content.ReadAsAsync<List<MailItem>>();
}
```

### Pattern 2: Dynamic Data Update

```csharp
async Task UpdateTreeData()
{
    // Fetch fresh data from server
    MyFolder = await FetchDataFromServer();
    
    // Trigger re-render
    StateHasChanged();
}

<button @onclick="UpdateTreeData">Refresh Tree</button>
```

### Pattern 3: Mixed Local and Remote

```csharp
protected override async Task OnInitializedAsync()
{
    // Load root from local data
    MyFolder = GetLocalRootNodes();
    
    // Load child data on demand via events
}

void OnNodeExpanded(NodeExpandEventArgs args)
{
    // Load child nodes from API only when expanded
    var children = await FetchChildNodesFromApi(args.NodeData.Id);
    args.NodeData.Children = children;
}
```

### Pattern 4: Filtered Data

```csharp
List<MailItem> FilteredFolder
{
    get => MyFolder.Where(x => x.Visible).ToList();
}

<SfTreeView TValue="MailItem">
    <TreeViewFieldsSettings TValue="MailItem" DataSource="@FilteredFolder" />
</SfTreeView>
```

## Best Practices

1. **Use HasChildren hint** to avoid loading all data at once
2. **Prefer self-referential** for large or complex hierarchies
3. **Enable LoadOnDemand** for performance with large datasets
4. **Cache remote data** to avoid repeated API calls
5. **Use Id and ParentId** consistently across your data
6. **Test with realistic data sizes** to validate performance
7. **Handle DataBound event** for post-binding operations

## Troubleshooting

**Issue: Nodes not displaying**
- Verify DataSource has data
- Check field mappings match class properties
- Ensure Id values are unique

**Issue: Performance degradation**
- Enable LoadOnDemand for large datasets
- Consider virtualization
- Check data size and optimize API responses

**Issue: Remote data not loading**
- Verify API endpoint is accessible
- Check CORS settings on server
- Inspect browser console for errors
- Verify DataManager configuration

## DataSource Property Reference

The `DataSource` property is the core data binding property:

```csharp
// Local data (List)
public List<MailItem> LocalData { get; set; }

<SfTreeView TValue="MailItem">
    <TreeViewFieldsSettings TValue="MailItem"
        Id="Id"
        Text="Name"
        DataSource="@LocalData">
    </TreeViewFieldsSettings>
</SfTreeView>

// Remote data (DataManager)
SfDataManager remoteData = new SfDataManager
{
    Url = "https://api.example.com/items",
    Adaptor = Syncfusion.Blazor.Adaptors.AdaptorType.JsonAdaptor
};

<SfTreeView TValue="Item">
    <TreeViewFieldsSettings TValue="Item"
        Id="Id"
        Text="Name"
        ParentID="ParentId"
        HasChildren="HasChild"
        DataSource="@remoteData">
    </TreeViewFieldsSettings>
</SfTreeView>
```

| Property | Type | Purpose |
|----------|------|---------|
| `DataSource` | IEnumerable / SfDataManager | Collection of data items or remote data configuration |

---

## LoadOnDemand Property Reference

Controls when child nodes are loaded:

```csharp
// Enable (Default) - Load children only when parent expands
<SfTreeView TValue="EmployeeData" LoadOnDemand="true">
    <TreeViewFieldsSettings TValue="EmployeeData"
        Id="Id"
        Text="Name"
        ParentID="ParentId"
        HasChildren="HasChild"
        DataSource="@Employees">
    </TreeViewFieldsSettings>
</SfTreeView>

// Disable - Load all nodes at initialization
<SfTreeView TValue="EmployeeData" LoadOnDemand="false">
    <TreeViewFieldsSettings TValue="EmployeeData"
        Id="Id"
        Text="Name"
        Child="Children"
        DataSource="@AllEmployees">
    </TreeViewFieldsSettings>
</SfTreeView>
```

| Property | Type | Default | Performance Impact |
|----------|------|---------|-----------|
| `LoadOnDemand` | bool | true | Faster initial render, reduces bandwidth with large datasets |

**When to Use Each**:
- `LoadOnDemand="true"`: Large datasets, API-driven data, deep hierarchies, performance critical
- `LoadOnDemand="false"`: Small datasets, all data readily available, needs full tree access

---

## ExpandOn Property Reference

Controls which action triggers node expand/collapse:

```csharp
// Expand on Click (Default)
<SfTreeView TValue="TreeData" ExpandOn="ExpandAction.Click">
    <TreeViewFieldsSettings TValue="TreeData" DataSource="@Data" />
</SfTreeView>

// Expand on Double-Click
<SfTreeView TValue="TreeData" ExpandOn="ExpandAction.DoubleClick">
    <TreeViewFieldsSettings TValue="TreeData" DataSource="@Data" />
</SfTreeView>

// No automatic expand
<SfTreeView TValue="TreeData" ExpandOn="ExpandAction.None">
    <TreeViewFieldsSettings TValue="TreeData" DataSource="@Data" />
</SfTreeView>
```

| Property | Type | Default | Options |
|----------|------|---------|---------|
| `ExpandOn` | ExpandAction | Click | Click, DoubleClick, None |

---

## TreeViewFieldsSettings Complete Reference

Maps data class properties to TreeView fields:

### Required Fields

```csharp
public class Employee
{
    public string? Id { get; set; }           // Maps to Id field
    public string? Name { get; set; }          // Maps to Text field
    public string? ParentId { get; set; }      // Maps to ParentID field
    public List<Employee>? Staff { get; set; } // Maps to Child field
    public bool HasTeam { get; set; }          // Maps to HasChildren field
}

<TreeViewFieldsSettings TValue="Employee"
    Id="Id"
    Text="Name"
    ParentID="ParentId"
    Child="Staff"
    HasChildren="HasTeam"
    DataSource="@Employees">
</TreeViewFieldsSettings>
```

### Optional Fields

```csharp
public class Employee
{
    public string? Id { get; set; }
    public string? Name { get; set; }
    public string? ParentId { get; set; }
    public List<Employee>? Staff { get; set; }
    public bool HasTeam { get; set; }
    
    // Optional fields
    public bool IsExpanded { get; set; }      // Maps to Expanded
    public bool? IsSelected { get; set; }     // Maps to IsChecked
    public string? ProfileImage { get; set; } // Maps to ImageUrl
    public string? JobTitle { get; set; }     // Maps to Tooltip
    public string? CompanyUrl { get; set; }   // Maps to NavigateUrl
}

<TreeViewFieldsSettings TValue="Employee"
    Id="Id"
    Text="Name"
    ParentID="ParentId"
    Child="Staff"
    HasChildren="HasTeam"
    Expanded="IsExpanded"
    IsChecked="IsSelected"
    ImageUrl="ProfileImage"
    Tooltip="JobTitle"
    NavigateUrl="CompanyUrl"
    DataSource="@Employees">
</TreeViewFieldsSettings>
```

### Complete Field Mapping Reference

| Setting | Type | Purpose | Example |
|---------|------|---------|---------|
| `Id` | string | Unique identifier for each node | "EmployeeId" |
| `Text` | string | Display text for node | "EmployeeName" |
| `ParentID` | string | Parent node reference (self-referential) | "ReportsTo" |
| `Child` | string | Child nodes collection (hierarchical) | "Staff" or "Children" |
| `HasChildren` | string | Boolean field indicating if node has children | "HasTeam" or "IsManager" |
| `Expanded` | string | Boolean field for initial expanded state | "InitialExpanded" |
| `IsChecked` | string | Checkbox state field (bool? for nullable) | "Selected" or "IsChecked" |
| `Selected` | string | Boolean field for initial selected state | "InitialSelected" |
| `ImageUrl` | string | Node icon/image URL field | "IconUrl" or "ProfilePic" |
| `Tooltip` | string | Tooltip text field for nodes | "EmployeeTitle" or "Description" |
| `NavigateUrl` | string | URL for node navigation | "CompanyPage" or "ProfileLink" |
| `Query` | Query | OData query for filtering/sorting | Query().Where(...) |

---

## Query Property for Remote Data

Apply filtering and sorting to remote data:

```csharp
@using Syncfusion.Blazor.Data

<SfTreeView TValue="Employee">
    <TreeViewFieldsSettings TValue="Employee"
        Id="Id"
        Text="Name"
        ParentID="ParentId"
        DataSource="@Manager"
        Query="@QueryData">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    SfDataManager Manager = new SfDataManager
    {
        Url = "https://api.example.com/employees",
        Adaptor = Syncfusion.Blazor.Adaptors.AdaptorType.ODataV4Adaptor
    };
    
    Query QueryData = new Query()
        .Where("IsActive", "equal", true)
        .Where("Department", "equal", "Engineering");
}
```

---

## DataBound vs Data Binding Events

### DataBound Event (After Binding Complete)

```csharp
<SfTreeView TValue="Employee">
    <TreeViewFieldsSettings TValue="Employee" DataSource="@Employees" />
    <TreeViewEvents TValue="Employee" DataBound="OnDataBound"></TreeViewEvents>
</SfTreeView>

@code {
    void OnDataBound(DataBoundEventArgs args)
    {
        Console.WriteLine("Data binding complete");
        Console.WriteLine($"Total nodes: {args.Data?.Count}");
        
        // Auto-expand root nodes after binding
        ExpandRootNodes();
    }
    
    void ExpandRootNodes()
    {
        var rootNodes = Employees
            .Where(x => x.ParentId == null)
            .Select(x => x.Id)
            .ToArray();
        
        ExpandedNodes = rootNodes;
    }
}
```

### DataSourceChanged Event (When Data Changes)

```csharp
<SfTreeView TValue="Employee">
    <TreeViewFieldsSettings TValue="Employee" DataSource="@Employees" />
    <TreeViewEvents TValue="Employee" DataSourceChanged="OnDataSourceChanged"></TreeViewEvents>
</SfTreeView>

@code {
    void OnDataSourceChanged()
    {
        Console.WriteLine("Data source has changed");
        // Refresh state, validation, etc.
    }
}
```

---

## State Persistence with EnablePersistence

Automatically save and restore data binding state:

```csharp
<SfTreeView TValue="Employee"
    EnablePersistence="true"
    @bind-SelectedNodes="@SelectedNodes"
    @bind-ExpandedNodes="@ExpandedNodes">
    <TreeViewFieldsSettings TValue="Employee" DataSource="@Employees" />
</SfTreeView>

@code {
    string[] SelectedNodes = Array.Empty<string>();
    string[] ExpandedNodes = Array.Empty<string>();
}
```

**What Gets Persisted**:
- Expanded nodes
- Selected nodes
- Checked nodes (if checkboxes enabled)
- Scroll position

**Storage**: Browser localStorage

---

## Common Data Binding Mistakes to Avoid

1. **Forgetting LoadOnDemand with Large Data**
   ```csharp
   // ❌ BAD - Loads all 100K nodes on init
   <SfTreeView TValue="Data" LoadOnDemand="false">
   
   // ✅ GOOD - Loads only visible nodes
   <SfTreeView TValue="Data" LoadOnDemand="true">
   ```

2. **Inconsistent Field Mappings**
   ```csharp
   // ❌ BAD - Property names don't match class
   <TreeViewFieldsSettings TValue="Employee" Id="employee_id" Text="EmployeeName" />
   
   // ✅ GOOD - Property names match exactly
   <TreeViewFieldsSettings TValue="Employee" Id="Id" Text="Name" />
   ```

3. **Missing HasChildren Indicator**
   ```csharp
   // ❌ BAD - No HasChildren = inefficient queries
   <TreeViewFieldsSettings TValue="Employee"
       Id="Id"
       Text="Name"
       ParentID="ParentId"
       DataSource="@Employees">
   </TreeViewFieldsSettings>
   
   // ✅ GOOD - HasChildren prevents unnecessary API calls
   <TreeViewFieldsSettings TValue="Employee"
       Id="Id"
       Text="Name"
       ParentID="ParentId"
       HasChildren="IsManager"
       DataSource="@Employees">
   </TreeViewFieldsSettings>
   ```

4. **Forgetting to Set Data Types**
   ```csharp
   // ❌ BAD - Data might not load
   List<Employee> Employees; // Null or uninitialized
   
   // ✅ GOOD - Data initialized
   List<Employee> Employees = new();
   
   protected override void OnInitialized()
   {
       LoadEmployeeData();
   }
   ```

---

## Performance Tuning for Data Binding

### For Large Local Datasets (10K+ items)

```csharp
<SfTreeView TValue="LargeDataItem"
    LoadOnDemand="true"
    EnableVirtualization="true"
    Height="500px">
    <TreeViewFieldsSettings TValue="LargeDataItem"
        Id="Id"
        Text="Name"
        ParentID="ParentId"
        HasChildren="HasChild"
        DataSource="@LargeData">
    </TreeViewFieldsSettings>
</SfTreeView>
```

### For Remote API Data

```csharp
<SfTreeView TValue="RemoteItem" LoadOnDemand="true">
    <TreeViewFieldsSettings TValue="RemoteItem"
        Id="Id"
        Text="Name"
        ParentID="ParentId"
        HasChildren="HasChild"
        DataSource="@ApiManager"
        Query="@FilterQuery">
    </TreeViewFieldsSettings>
    <TreeViewEvents TValue="RemoteItem" NodeExpanded="OnNodeExpanded"></TreeViewEvents>
</SfTreeView>

@code {
    // Only load visible nodes on demand
    void OnNodeExpanded(NodeExpandEventArgs args)
    {
        // API loads children only when needed
    }
}
```

---

## Next Steps

- Use [node-selection.md](node-selection.md) to handle user selections
- Enable [checkbox-features.md](checkbox-features.md) for multi-selection
- Implement [expand-collapse-actions.md](expand-collapse-actions.md) for node expansion
