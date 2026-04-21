# Grouping and Organization in Blazor ListView

## Table of Contents
- [Basic Grouping](#basic-grouping)
- [Group Header Styling](#group-header-styling)
- [Grouped Data Structure](#grouped-data-structure)
- [Hierarchical Grouping](#hierarchical-grouping)
- [Advanced Grouping Patterns](#advanced-grouping-patterns)

## Basic Grouping

### Group by Single Field

Use the `GroupBy` property in `ListViewFieldSettings`:

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView DataSource="@DataSource">
    <ListViewFieldSettings TValue="DataModel" 
                          Id="Id" 
                          Text="Text" 
                          GroupBy="Type">
    </ListViewFieldSettings>
</SfListView>

@code {
    List<DataModel> DataSource = new List<DataModel>()
    {
        new DataModel { Id = "1", Text = "1", Type = "Odd" },
        new DataModel { Id = "2", Text = "2", Type = "Even" },
        new DataModel { Id = "3", Text = "3", Type = "Odd" },
        new DataModel { Id = "4", Text = "4", Type = "Even" },
        new DataModel { Id = "5", Text = "5", Type = "Odd" }
    };

    public class DataModel
    {
        public string Id { get; set; }
        public string Text { get; set; }
        public string Type { get; set; }
    }
}
```

### Categories Example

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView DataSource="@Products"
            ShowHeader="true"
            HeaderTitle="Products"
            SortOrder="SortOrder.Ascending">
    <ListViewFieldSettings TValue="Product" 
                          Id="Id" 
                          Text="Name"
                          GroupBy="Category">
    </ListViewFieldSettings>
</SfListView>

@code {
    List<Product> Products = new List<Product>
    {
        new Product { Id = "1", Name = "Laptop", Category = "Electronics" },
        new Product { Id = "2", Name = "Mouse", Category = "Electronics" },
        new Product { Id = "3", Name = "Chair", Category = "Furniture" },
        new Product { Id = "4", Name = "Desk", Category = "Furniture" },
        new Product { Id = "5", Name = "Monitor", Category = "Electronics" }
    };

    public class Product
    {
        public string Id { get; set; }
        public string Name { get; set; }
        public string Category { get; set; }
    }
}
```

## Group Header Styling

### Custom Group Headers

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView DataSource="@DataSource" CssClass="e-list-template">
    <ListViewFieldSettings TValue="Item" 
                          Id="Id" 
                          Text="Text"
                          GroupBy="Category">
    </ListViewFieldSettings>
    <ListViewTemplates TValue="Item">
        <GroupTemplate>
            <div style="padding: 12px; background-color: #e0e0e0; font-weight: bold; font-size: 14px;">
                📁 @context.Text
            </div>
        </GroupTemplate>
    </ListViewTemplates>
</SfListView>

@code {
    List<Item> DataSource = new List<Item>
    {
        new Item { Id = "1", Text = "Item A", Category = "Category 1" },
        new Item { Id = "2", Text = "Item B", Category = "Category 1" },
        new Item { Id = "3", Text = "Item C", Category = "Category 2" },
        new Item { Id = "4", Text = "Item D", Category = "Category 2" }
    };

    public class Item
    {
        public string Id { get; set; }
        public string Text { get; set; }
        public string Category { get; set; }
    }
}
<style>
    /* Remove default padding from group header */
    .e-list-template .e-list-group-item {
        padding: 0 !important;
        margin: 0 !important;
    }

    /* Custom group header styling */
    .group-header {
        padding: 12px;
        background-color: #e0e0e0;
        font-weight: bold;
        font-size: 14px;
        margin: 0;
    }

    /* Align list items properly */
    .e-list-template .e-list-item {
        padding: 12px;
    }
</style>
```

### Styled Groups with Item Counts

```cshtml
<SfListView DataSource="@DataSource" CssClass="custom-grouped-list">
    <ListViewFieldSettings TValue="Item" 
                          Id="Id" 
                          Text="Text"
                          GroupBy="Category">
    </ListViewFieldSettings>
    <ListViewTemplates TValue="Item">
        <GroupTemplate>
            @{
                var categoryName = context.Text;
                var itemsInGroup = context.Items.Count();
            }
            <div class="styled-group-header">
                <span>@categoryName</span>
                <span class="item-badge">@itemsInGroup items</span>
            </div>
        </GroupTemplate>
    </ListViewTemplates>
</SfListView>

@code {
    List<Item> DataSource = new List<Item>
    {
        new Item { Id = "1", Text = "Item A", Category = "Category 1" },
        new Item { Id = "2", Text = "Item B", Category = "Category 1" },
        new Item { Id = "3", Text = "Item C", Category = "Category 2" },
        new Item { Id = "4", Text = "Item D", Category = "Category 2" }
    };

    public class Item
    {
        public string Id { get; set; }
        public string Text { get; set; }
        public string Category { get; set; }
    }
}

<style>
    .custom-grouped-list .e-list-group-item {
        padding: 0 !important;
    }

    .styled-group-header {
        padding: 12px 16px;
        background-color: #1976d2;
        color: white;
        font-weight: bold;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .item-badge {
        background-color: rgba(255,255,255,0.3);
        padding: 4px 12px;
        border-radius: 4px;
        font-size: 12px;
    }
</style>
```

## Grouped Data Structure

### Organize Data by Category

```csharp
public class GroupedViewModel
{
    public string CategoryName { get; set; }
    public List<Item> Items { get; set; }
}

// Data organization
var groupedData = new List<GroupedViewModel>
{
    new GroupedViewModel 
    { 
        CategoryName = "Fruits",
        Items = new List<Item>
        {
            new Item { Id = "1", Text = "Apple" },
            new Item { Id = "2", Text = "Banana" }
        }
    },
    new GroupedViewModel 
    { 
        CategoryName = "Vegetables",
        Items = new List<Item>
        {
            new Item { Id = "3", Text = "Carrot" },
            new Item { Id = "4", Text = "Lettuce" }
        }
    }
};
```

### Display Grouped Data

```cshtml
@foreach (var group in GroupedData)
{
    <div style="margin-bottom: 20px;">
        <h3>@group.CategoryName</h3>
        <SfListView DataSource="@group.Items">
            <ListViewFieldSettings TValue="Item" Id="Id" Text="Text"></ListViewFieldSettings>
        </SfListView>
    </div>
}
```

## Hierarchical Grouping

### Multi-Level Categories

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
        ListData.Add(new DataModel
        {
            Text = "Asia",
            Id = "01",
            Child = new List<DataModel>()
            {
                new DataModel
                {
                    Text = "India",
                    Id = "1",
                    Child = new List<DataModel>()
                    {
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
                        new DataModel { Text = "Shanghai", Id = "2002" }
                    }
                }
            }
        });

        ListData.Add(new DataModel
        {
            Text = "North America",
            Id = "02",
            Child = new List<DataModel>()
            {
                new DataModel
                {
                    Text = "USA",
                    Id = "3",
                    Child = new List<DataModel>()
                    {
                        new DataModel { Text = "California", Id = "3001" },
                        new DataModel { Text = "New York", Id = "3002" }
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

## Advanced Grouping Patterns

### Filter Then Group

```csharp
// Filter products first, then group by category
var filtered = AllProducts.Where(p => p.Price > 50);
var grouped = filtered.GroupBy(p => p.Category)
    .Select(g => new { Category = g.Key, Items = g.ToList() })
    .ToList();
```

### Dynamic Grouping

```cshtml
@using Syncfusion.Blazor.Lists
@using Syncfusion.Blazor.DropDowns

<div style="margin-bottom: 20px;">
    <label>Group by: </label>
    <SfDropDownList TValue="string" 
                    TItem="GroupOption"
                    @bind-Value="@CurrentGroupField"
                    DataSource="@GroupingOptions">
        <DropDownListFieldSettings Text="Label" Value="Field"></DropDownListFieldSettings>
    </SfDropDownList>
</div>

<SfListView DataSource="@AllProducts" @key="@CurrentGroupField">
    <ListViewFieldSettings TValue="Product" 
                          Id="Id" 
                          Text="Name"
                          GroupBy="@CurrentGroupField">
    </ListViewFieldSettings>
</SfListView>

@code {
    private List<Product> AllProducts = new List<Product>();
    private string CurrentGroupField = "Category";
    private List<GroupOption> GroupingOptions = new List<GroupOption>();

    protected override void OnInitialized()
    {
        GroupingOptions = new List<GroupOption>
        {
            new GroupOption { Label = "Category", Field = "Category" },
            new GroupOption { Label = "Brand", Field = "Brand" }
        };

        AllProducts = new List<Product>
        {
            new Product { Id = "1", Name = "Laptop", Category = "Electronics", Brand = "Dell" },
            new Product { Id = "2", Name = "Mouse", Category = "Electronics", Brand = "Logitech" },
            new Product { Id = "3", Name = "Chair", Category = "Furniture", Brand = "Ikea" },
            new Product { Id = "4", Name = "Desk", Category = "Furniture", Brand = "Ikea" }
        };
    }

    public class GroupOption
    {
        public string Label { get; set; } = string.Empty;
        public string Field { get; set; } = string.Empty;
    }

    public class Product
    {
        public string Id { get; set; } = string.Empty;
        public string Name { get; set; } = string.Empty;
        public string Category { get; set; } = string.Empty;
        public string Brand { get; set; } = string.Empty;
    }
}
```

### Group with Summary

```cshtml
<SfListView DataSource="@DataSource" CssClass="summary-list">
    <ListViewFieldSettings TValue="Item" 
                          Id="Id" 
                          Text="Text"
                          GroupBy="Category">
    </ListViewFieldSettings>
    <ListViewTemplates TValue="Item">
        <GroupTemplate>
            @{
                var categoryName = context.Text;
                var categoryItems = context.Items.Cast<Item>();
                var total = categoryItems.Sum(i => i.Quantity);
            }
            <div class="summary-header">
                <strong>@categoryName</strong>
                <span>Total Quantity: @total</span>
            </div>
        </GroupTemplate>
    </ListViewTemplates>
</SfListView>

@code {
    List<Item> DataSource = new List<Item>
    {
        new Item { Id = "1", Text = "Item A", Category = "Electronics", Quantity = 10 },
        new Item { Id = "2", Text = "Item B", Category = "Electronics", Quantity = 15 },
        new Item { Id = "3", Text = "Item C", Category = "Furniture", Quantity = 5 },
        new Item { Id = "4", Text = "Item D", Category = "Furniture", Quantity = 8 }
    };

    public class Item
    {
        public string Id { get; set; }
        public string Text { get; set; }
        public string Category { get; set; }
        public int Quantity { get; set; }
    }
}
```

---

**Related Topics:**
- [Data Binding and Sources](data-binding-and-source.md) - Prepare data for grouping
- [Filtering and Searching](filtering-and-searching.md) - Filter grouped data
- [Advanced Patterns](advanced-patterns.md) - Complex organizational scenarios

