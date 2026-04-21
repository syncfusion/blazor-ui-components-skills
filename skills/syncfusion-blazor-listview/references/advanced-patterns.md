# Advanced Patterns in Blazor ListView

## Table of Contents
- [Nested Lists](#nested-lists)
- [Add and Remove Items Dynamically](#add-and-remove-items-dynamically)
- [CRUD Operations](#crud-operations)
- [Complex Data Scenarios](#complex-data-scenarios)
- [Best Practices](#best-practices)

## Nested Lists

### Three-Level Hierarchy

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView DataSource="@ListData" ShowHeader="true" HeaderTitle="Continent">
    <ListViewFieldSettings TValue="DataModel" 
                          Id="Id" 
                          Text="Text" 
                          Child="Child">
    </ListViewFieldSettings>
</SfListView>

@code {
    List<DataModel> ListData = new List<DataModel>();

    protected override void OnInitialized()
    {
        base.OnInitialized();

        // Level 1: Continents
        ListData.Add(new DataModel
        {
            Text = "Asia",
            Id = "01",
            Child = new List<DataModel>()
            {
                // Level 2: Countries
                new DataModel
                {
                    Text = "India",
                    Id = "1",
                    Child = new List<DataModel>()
                    {
                        // Level 3: Cities
                        new DataModel { Text = "Delhi", Id = "1001" },
                        new DataModel { Text = "Mumbai", Id = "1002" },
                        new DataModel { Text = "Bangalore", Id = "1003" }
                    }
                },
                new DataModel
                {
                    Text = "China",
                    Id = "2",
                    Child = new List<DataModel>()
                    {
                        new DataModel { Text = "Beijing", Id = "2001" },
                        new DataModel { Text = "Shanghai", Id = "2002" },
                        new DataModel { Text = "Shenzhen", Id = "2003" }
                    }
                }
            }
        });

        ListData.Add(new DataModel
        {
            Text = "Europe",
            Id = "02",
            Child = new List<DataModel>()
            {
                new DataModel
                {
                    Text = "Germany",
                    Id = "3",
                    Child = new List<DataModel>()
                    {
                        new DataModel { Text = "Berlin", Id = "3001" },
                        new DataModel { Text = "Munich", Id = "3002" }
                    }
                }
            }
        });
    }

    public class DataModel
    {
        public string Id { get; set; }
        public string Text { get; set; }
        public List<DataModel> Child { get; set; }
    }
}
```

### Custom Nested Templates

```cshtml
<SfListView DataSource="@OrganizationData" CssClass="e-list-template">
    <ListViewFieldSettings TValue="OrganizationUnit" 
                          Id="Id" 
                          Text="Name"
                          Child="Departments">
    </ListViewFieldSettings>
    <ListViewTemplates TValue="OrganizationUnit">
        <Template>
            <div class="e-list-wrapper e-list-multi-line">
                <span class="e-list-item-header">@((context as OrganizationUnit).Name)</span>
                @if ((context as OrganizationUnit).Departments?.Count > 0)
                {
                    <span class="e-list-content">
                        @((context as OrganizationUnit).Departments.Count) departments
                    </span>
                }
            </div>
        </Template>
    </ListViewTemplates>
</SfListView>

@code {
    List<OrganizationUnit> OrganizationData = new List<OrganizationUnit>
    {
        new OrganizationUnit
        {
            Id = "1",
            Name = "Engineering",
            Departments = new List<OrganizationUnit>
            {
                new OrganizationUnit { Id = "1-1", Name = "Backend" },
                new OrganizationUnit { Id = "1-2", Name = "Frontend" }
            }
        },
        new OrganizationUnit
        {
            Id = "2",
            Name = "Sales",
            Departments = new List<OrganizationUnit>
            {
                new OrganizationUnit { Id = "2-1", Name = "Enterprise" },
                new OrganizationUnit { Id = "2-2", Name = "SMB" }
            }
        }
    };

    public class OrganizationUnit
    {
        public string Id { get; set; }
        public string Name { get; set; }
        public List<OrganizationUnit> Departments { get; set; }
    }
}
```

## Add and Remove Items Dynamically

### Using ObservableCollection

```cshtml
@using Syncfusion.Blazor.Lists
@using System.Collections.ObjectModel

<div class="flex">
    <div class="margin">
        <SfListView ID="sample-list" DataSource="@DataSource">
            <ListViewFieldSettings TValue="ListDataModel" Id="Id" Text="Text"></ListViewFieldSettings>
            <ListViewTemplates TValue="ListDataModel">
                <Template>
                    @{
                        ListDataModel item = context as ListDataModel;
                        <div class="text-content">
                            @item.Text
                            <span class="delete-icon" @onclick="@(() => { OnDelete(item); })"></span>
                        </div>
                    }
                </Template>
            </ListViewTemplates>
        </SfListView>
    </div>
</div>

<div class="flex">
    <button class="e-btn" @onclick="@AddItem">Add Item</button>
</div>

@code {
    ObservableCollection<ListDataModel> DataSource = new ObservableCollection<ListDataModel>()
    {
        new ListDataModel{ Id = "1", Text = "Artwork"},
        new ListDataModel{ Id = "2", Text = "Abstract"},
        new ListDataModel{ Id = "3", Text = "Modern Painting"},
        new ListDataModel{ Id = "4", Text = "Ceramics"},
        new ListDataModel{ Id = "5", Text = "Animation Art"},
        new ListDataModel{ Id = "6", Text = "Oil Painting"},
    };

    void OnDelete(ListDataModel listDataModel)
    {
        var index = DataSource.ToList().FindIndex(e => e.Id == listDataModel.Id);
        if (index >= 0)
        {
            DataSource.RemoveAt(index);
        }
    }

    void AddItem()
    {
        var random = new Random();
        DataSource.Add(new ListDataModel
        {
            Id = random.Next(100, 300).ToString(),
            Text = "Item " + random.Next(100, 300).ToString(),
        });
    }

    public class ListDataModel
    {
        public string Id { get; set; }
        public string Text { get; set; }
    }
}

<style>
    .flex {
        display: flex;
        justify-content: center;
    }

    .margin {
        margin: 10px;
        width: 300px;
    }

    #sample-list .delete-icon::after {
        content: "\e878";
        float: right;
        cursor: pointer;
        color: red;
    }

    .text-content {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
</style>
```

## CRUD Operations

### Insert Operation

```csharp
void InsertItem(ListDataModel newItem)
{
    DataSource.Add(newItem);
    // ObservableCollection automatically triggers UI update
}

void InsertAtIndex(ListDataModel newItem, int index)
{
    DataSource.Insert(index, newItem);
}
```

### Update Operation

```csharp
void UpdateItem(string id, ListDataModel updatedItem)
{
    var existingItem = DataSource.FirstOrDefault(x => x.Id == id);
    if (existingItem != null)
    {
        var index = DataSource.IndexOf(existingItem);
        DataSource[index] = updatedItem;
    }
}

void UpdateItemProperty(string id, string newText)
{
    var item = DataSource.FirstOrDefault(x => x.Id == id);
    if (item != null)
    {
        item.Text = newText;
        // Update view
        StateHasChanged();
    }
}
```

### Delete Operation

```csharp
void DeleteItem(string id)
{
    var item = DataSource.FirstOrDefault(x => x.Id == id);
    if (item != null)
    {
        DataSource.Remove(item);
    }
}

void DeleteAtIndex(int index)
{
    if (index >= 0 && index < DataSource.Count)
    {
        DataSource.RemoveAt(index);
    }
}

void DeleteAll()
{
    DataSource.Clear();
}
```

### Read/Retrieve Operations

```csharp
ListDataModel GetItemById(string id)
{
    return DataSource.FirstOrDefault(x => x.Id == id);
}

List<ListDataModel> GetItemsByCategory(string category)
{
    return DataSource.Where(x => x.Category == category).ToList();
}

int GetItemCount()
{
    return DataSource.Count;
}
```

## Complex Data Scenarios

### Hierarchical Data with Mixed Types

```csharp
public class TreeNode
{
    public string Id { get; set; }
    public string Name { get; set; }
    public NodeType Type { get; set; }
    public List<TreeNode> Children { get; set; }
    public Dictionary<string, object> Metadata { get; set; }
}

public enum NodeType
{
    Folder,
    File,
    Item
}

// Create complex nested structure
var fileSystem = new TreeNode
{
    Id = "root",
    Name = "Documents",
    Type = NodeType.Folder,
    Children = new List<TreeNode>
    {
        new TreeNode
        {
            Id = "folder1",
            Name = "Projects",
            Type = NodeType.Folder,
            Children = new List<TreeNode>
            {
                new TreeNode { Id = "file1", Name = "project.md", Type = NodeType.File },
                new TreeNode { Id = "file2", Name = "notes.txt", Type = NodeType.File }
            }
        },
        new TreeNode { Id = "file3", Name = "resume.pdf", Type = NodeType.File }
    }
};
```

### Filtered and Grouped Data

```csharp
// Get filtered and grouped data
var groupedItems = AllItems
    .Where(x => x.Active == true)
    .GroupBy(x => x.Category)
    .Select(g => new GroupedData
    {
        Category = g.Key,
        Items = g.ToList(),
        ItemCount = g.Count()
    })
    .ToList();
```

### Asynchronous Data Loading

```cshtml
@using System.Net.Http.Json

<SfListView DataSource="@LoadedItems">
    <ListViewFieldSettings TValue="Item" Id="Id" Text="Name"></ListViewFieldSettings>
</SfListView>

<button @onclick="LoadMoreItems">Load More</button>

@code {
    List<Item> LoadedItems = new List<Item>();
    private int pageNumber = 1;

    async Task LoadMoreItems()
    {
        try
        {
            var newItems = await Http.GetFromJsonAsync<List<Item>>(
                $"/api/items?page={pageNumber}&pageSize=10"
            );
            if (newItems != null)
            {
                LoadedItems.AddRange(newItems);
                pageNumber++;
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error loading items: {ex.Message}");
        }
    }

    public class Item
    {
        public string Id { get; set; }
        public string Name { get; set; }
    }
}
```

## Best Practices

### 1. Use ObservableCollection for Dynamic Lists

```csharp
// ✅ GOOD: Changes automatically update UI
ObservableCollection<Item> Items = new ObservableCollection<Item>();
Items.Add(new Item());

// ❌ AVOID: Manual StateHasChanged required
List<Item> Items = new List<Item>();
Items.Add(new Item());
StateHasChanged();
```

### 2. Key by Unique Identifier

```csharp
// Ensure each item has unique Id
Items.Add(new Item 
{ 
    Id = Guid.NewGuid().ToString(), // Unique
    Name = "New Item"
});
```

### 3. Handle Null References

```csharp
void RemoveItem(Item item)
{
    if (item != null && !string.IsNullOrEmpty(item.Id))
    {
        Items.Remove(item);
    }
}
```

### 4. Optimize Performance with Large Lists

```csharp
// Use virtualization for 500+ items
// Implement pagination
// Lazy-load children

<SfListView DataSource="@Items"
            EnableVirtualization="true"
            Height="500px">
</SfListView>
```

### 5. Proper State Management

```csharp
// Track changes
private List<Item> deletedItems = new List<Item>();
private List<Item> addedItems = new List<Item>();
private List<Item> modifiedItems = new List<Item>();

// Provide undo/redo capability
void Undo()
{
    // Restore to previous state
}
```

### 6. Error Handling in CRUD

```csharp
async Task SafeAddItem(Item newItem)
{
    try
    {
        ValidateItem(newItem);
        Items.Add(newItem);
        await SaveToServer(newItem);
    }
    catch (ValidationException ex)
    {
        // Handle validation errors
        ShowError(ex.Message);
    }
    catch (Exception ex)
    {
        // Handle unexpected errors
        showError("Failed to add item");
    }
}

void ValidateItem(Item item)
{
    if (string.IsNullOrEmpty(item.Name))
        throw new ValidationException("Name is required");
}
```

---

**Related Topics:**
- [Data Binding and Sources](data-binding-and-source.md) - Complex data sources
- [Item Selection and Actions](item-selection-and-actions.md) - CRUD event handling
- [Virtualization and Performance](virtualization-and-performance.md) - Performance optimization

