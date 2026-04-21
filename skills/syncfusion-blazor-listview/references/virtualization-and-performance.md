# Virtualization and Performance in Blazor ListView

## Table of Contents
- [Enable Virtualization](#enable-virtualization)
- [Window Scroll vs Container Scroll](#window-scroll-vs-container-scroll)
- [Configuration](#configuration)
- [Large Dataset Handling](#large-dataset-handling)
- [Performance Optimization](#performance-optimization)
- [Limitations](#limitations)

## Enable Virtualization

UI virtualization loads only viewable list items in the viewport, improving ListView performance when handling large datasets.

### Basic Virtual Scrolling

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView DataSource="@ListData" EnableVirtualization="true" Height="400px">
    <ListViewFieldSettings TValue="DataModel" Id="Id" Text="Text"></ListViewFieldSettings>
</SfListView>

@code {
    List<DataModel> ListData = new List<DataModel>();

    protected override void OnInitialized()
    {
        base.OnInitialized();
        
        // Generate large dataset
        for (int i = 0; i < 1000; i++)
        {
            ListData.Add(new DataModel
            {
                Text = $"Item {i + 1}",
                Id = i.ToString()
            });
        }
    }

    public class DataModel
    {
        public string Id { get; set; }
        public string Text { get; set; }
    }
}
```

### Virtual Scrolling with Checkboxes

```cshtml
<SfListView DataSource="@ListData" 
            EnableVirtualization="true" 
            Height="400px"
            ShowCheckBox="true">
    <ListViewFieldSettings TValue="DataModel" Id="Id" Text="Text"></ListViewFieldSettings>
</SfListView>
```

## Window Scroll vs Container Scroll

### Window Scroll (Default)

Scrolls within the browser window:

```cshtml
<SfListView DataSource="@ListData" 
            EnableVirtualization="true">
    <!-- No Height property = window scroll -->
    <ListViewFieldSettings TValue="DataModel" Id="Id" Text="Text"></ListViewFieldSettings>
</SfListView>
```

**Use when:**
- List should take up viewport
- Full-page list experience
- Page-level scrolling

### Container Scroll

Scrolls within defined height container:

```cshtml
<div style="height: 400px; border: 1px solid #ddd;">
    <SfListView DataSource="@ListData" 
                EnableVirtualization="true"
                Height="400px">
        <ListViewFieldSettings TValue="DataModel" Id="Id" Text="Text"></ListViewFieldSettings>
    </SfListView>
</div>
```

**Use when:**
- List is embedded in page
- Need fixed-height list pane
- Sidebar or panel layouts

## Configuration

### Essential Properties for Virtualization

```cshtml
<SfListView DataSource="@ListData"
            EnableVirtualization="true"
            Height="500px"
            ShowCheckBox="true"
            HeaderTitle="Products"
            ShowHeader="true">
    <ListViewFieldSettings TValue="Product" Id="Id" Text="Name"></ListViewFieldSettings>
</SfListView>

@code {
    // Generate large dataset
    List<Product> ListData = new List<Product>();

    protected override void OnInitialized()
    {
        for (int i = 1; i <= 5000; i++)
        {
            ListData.Add(new Product
            {
                Id = i.ToString(),
                Name = $"Product {i}"
            });
        }
    }

    public class Product
    {
        public string Id { get; set; }
        public string Name { get; set; }
    }
}
```

### Virtual Scrolling with Templates

```cshtml
<SfListView DataSource="@ListData" 
            EnableVirtualization="true"
            Height="500px"
            CssClass="e-list-template">
    <ListViewFieldSettings TValue="Item" Id="Id" Text="Name"></ListViewFieldSettings>
    <ListViewTemplates TValue="Item">
        <Template>
            <div class="e-list-wrapper e-list-multi-line">
                <span class="e-list-item-header">@((context as Item).Name)</span>
                <span class="e-list-content">Category: @((context as Item).Category)</span>
            </div>
        </Template>
    </ListViewTemplates>
</SfListView>
```

## Large Dataset Handling

### Generate Sample Data (1000+ Items)

```csharp
protected override void OnInitialized()
{
    base.OnInitialized();

    for (int i = 0; i < 1000; i++)
    {
        int index = new Random().Next(0, 10);
        ListData.Add(new DataModel
        {
            Text = $"Item {i}",
            Id = i.ToString(),
            Category = GetCategoryForIndex(i)
        });
    }
}

string GetCategoryForIndex(int index)
{
    return (index % 5) switch
    {
        0 => "Category A",
        1 => "Category B",
        2 => "Category C",
        3 => "Category D",
        _ => "Category E"
    };
}
```

### Lazy Loading with Virtualization

Load data as user scrolls:

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView @ref="ListView"
            DataSource="@CurrentData"
            EnableVirtualization="true"
            Height="500px">
    <ListViewFieldSettings TValue="Item" Id="Id" Text="Text"></ListViewFieldSettings>
    <ListViewEvents TValue="Item"
                    ItemRender="@OnItemRender">
    </ListViewEvents>
</SfListView>

@code {
    SfListView<Item> ListView;
    List<Item> CurrentData = new List<Item>();
    int pageSize = 100;
    int totalItems = 10000;
    int loadedCount = 0;

    protected override void OnInitialized()
    {
        LoadInitialData();
    }

    void LoadInitialData()
    {
        for (int i = 0; i < pageSize; i++)
        {
            CurrentData.Add(new Item { Id = i.ToString(), Text = $"Item {i}" });
        }
        loadedCount = pageSize;
    }

    void OnItemRender(ItemRenderEventArgs<Item> args)
    {
        // Load more when user reaches near the end
        if (args.Index > loadedCount - 50 && loadedCount < totalItems)
        {
            for (int i = loadedCount; i < loadedCount + pageSize && i < totalItems; i++)
            {
                CurrentData.Add(new Item { Id = i.ToString(), Text = $"Item {i}" });
            }
            loadedCount += pageSize;
        }
    }

    public class Item
    {
        public string Id { get; set; }
        public string Text { get; set; }
    }
}
```

## Performance Optimization

### Data Binding Best Practices

```csharp
// ❌ AVOID: Recreating entire list on updates
ListData = AllData.Where(x => x.Active).ToList();

// ✅ PREFER: Update DataSource reference
ListData.Clear();
ListData.AddRange(filtered);
await ListView.RefreshAsync();
```

### Efficient Item Rendering

```cshtml
<!-- ❌ AVOID: Complex template operations in render loop -->
<Template>
    <span>@((context as Item).ExpensiveCalculation())</span>
</Template>

<!-- ✅ PREFER: Pre-calculate in code-behind -->
<Template>
    <span>@((context as Item).CachedValue)</span>
</Template>
```

### Memory Management

```csharp
public void Dispose()
{
    if (ListView != null)
    {
        // Clear references
        ListData?.Clear();
        ListData = null;
    }
}
```

## Limitations

### Important Constraints

1. **Height in Pixels Required**
   ```csharp
   Height="400px"  // ✅ Required for container scroll
   Height="100%"   // ❌ Percentage values not supported
   ```

2. **Percentage Height Workaround**
   ```cshtml
   <div style="height: 500px;">
       <SfListView DataSource="@Data" 
                   EnableVirtualization="true"
                   Height="500px">
       </SfListView>
   </div>
   ```

3. **Index Retrieval**
   - GetCheckedItemsAsync() returns correct indices only with virtualization
   - ParentId retrieval works for nested virtual lists

4. **Performance Considerations**
   - For <500 items: Virtualization may not be necessary
   - Complex templates: Virtualization impact depends on template complexity
   - Remote data: Combine with server-side pagination for best results

### Device Considerations

```csharp
// Mobile: Smaller viewport, fewer items visible
@if (IsMobile)
{
    <SfListView EnableVirtualization="true" Height="300px"></SfListView>
}
else
{
    <SfListView EnableVirtualization="true" Height="600px"></SfListView>
}
```

---

**Related Topics:**
- [Advanced Patterns](advanced-patterns.md) - Complex scenarios with virtual scrolling
- [Data Binding and Sources](data-binding-and-source.md) - Server-side pagination
- [Item Selection and Actions](item-selection-and-actions.md) - Get indices from virtual lists

