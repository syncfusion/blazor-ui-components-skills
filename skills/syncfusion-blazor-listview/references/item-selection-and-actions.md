# Item Selection and Actions in Blazor ListView

## Table of Contents
- [Getting Selected Items](#getting-selected-items)
- [GetCheckedItemsAsync Method](#getcheckeditemsasync-method)
- [Selection with Checkboxes](#selection-with-checkboxes)
- [Selection Events](#selection-events)
- [Custom Template Selection](#custom-template-selection)

## Getting Selected Items

### Retrieve Checked Items

Use the `GetCheckedItemsAsync()` method to get selected items:

```cshtml
@using Syncfusion.Blazor.Lists

<div style="display: flex;">
    <div class="margin">
        <SfListView @ref="@SfList"
                    DataSource="@DataSource"
                    ShowCheckBox="true">
            <ListViewFieldSettings TValue="ListDataModel" Id="Id" Text="Text"></ListViewFieldSettings>
        </SfListView>
    </div>
    <div class="margin">
        <div class="padding">
            <button class="e-btn" @onclick="@OnSelect">Get Selected Items</button>
        </div>
        <div class="padding">
            <table>
                <tr>
                    <th>Text</th>
                    <th>Id</th>
                </tr>
                @foreach (var item in SelectedItems)
                {
                    <tr>
                        <td>@item.Text</td>
                        <td>@item.Id</td>
                    </tr>
                }
            </table>
        </div>
    </div>
</div>

@code
{
    SfListView<ListDataModel> SfList;

    List<ListDataModel> SelectedItems = new List<ListDataModel>();

    List<ListDataModel> DataSource = new List<ListDataModel>()
    {
        new ListDataModel{ Id = "1", Text = "Artwork"},
        new ListDataModel{ Id = "2", Text = "Abstract"},
        new ListDataModel{ Id = "3", Text = "Modern Painting"},
        new ListDataModel{ Id = "4", Text = "Ceramics"},
        new ListDataModel{ Id = "5", Text = "Animation Art"},
        new ListDataModel{ Id = "6", Text = "Oil Painting"},
    };

    async void OnSelect()
    {
        var items = await SfList.GetCheckedItemsAsync();
        if (items.Data != null)
        {
            SelectedItems = items.Data;
            this.StateHasChanged();
        }
    }

    public class ListDataModel
    {
        public string Id { get; set; }
        public string Text { get; set; }
    }
}

<style>
    .margin {
        margin: 10px;
        width: 300px;
    }

    .padding {
        padding: 10px 0;
    }

    table {
        width: 100%;
        border-collapse: collapse;
    }

    th, td {
        border: 1px solid #ddd;
        padding: 8px;
        text-align: left;
    }
</style>
```

## GetCheckedItemsAsync Method

### Return Value Details

The method returns an object with these properties:

| Property | Type | Description |
|----------|------|-------------|
| `Data` | List<T> | Array of selected item data objects |
| `Text` | List<string> | Array of selected item text values |
| `Index` | List<int> | Array of selected item indices (virtualization only) |
| `ParentId` | List<string> | Parent IDs of selected items (nested list only) |

### Basic Usage

```csharp
var checkedItems = await ListViewRef.GetCheckedItemsAsync();

// Access selected data
List<MyDataModel> selectedData = checkedItems.Data;

// Access selected text
List<string> selectedText = checkedItems.Text;

// Access indices (for virtual ListView)
List<int> selectedIndices = checkedItems.Index;
```

## Selection with Checkboxes

### Enable Checkbox Selection

```cshtml
<SfListView DataSource="@Items"
            ShowCheckBox="true"
            HeaderTitle="Select Items"
            ShowHeader="true">
    <ListViewFieldSettings TValue="Item" Id="Id" Text="Name"></ListViewFieldSettings>
</SfListView>

@code {
    List<Item> Items = new List<Item>
    {
        new Item { Id = "1", Name = "Item 1" },
        new Item { Id = "2", Name = "Item 2" },
        new Item { Id = "3", Name = "Item 3" }
    };

    public class Item
    {
        public string Id { get; set; }
        public string Name { get; set; }
    }
}
```

### Multiple Selection Modes

**All items selectable:**
```cshtml
<SfListView DataSource="@Items" ShowCheckBox="true"></SfListView>
```

**Pre-select items using IsChecked field:**
```cshtml
@code {
    public class Item
    {
        public string Id { get; set; }
        public string Name { get; set; }
        public bool IsChecked { get; set; }  // Set to true for default selection
    }

    List<Item> Items = new List<Item>
    {
        new Item { Id = "1", Name = "Item 1", IsChecked = true },
        new Item { Id = "2", Name = "Item 2", IsChecked = false }
    };
}
```

Map the field in ListViewFieldSettings:
```cshtml
<ListViewFieldSettings TValue="Item" 
                      Id="Id" 
                      Text="Name"
                      IsChecked="IsChecked">
</ListViewFieldSettings>
```
### Checkbox Position (Left vs Right)

Control checkbox placement using `CheckBoxPosition` property:

```cshtml
@using Syncfusion.Blazor.Lists

<!-- Checkbox on Left (Default) -->
<h3>Checkbox on Left</h3>
<SfListView DataSource="@Data" 
            ShowCheckBox="true"
            CheckBoxPosition="CheckBoxPosition.Left">
    <ListViewFieldSettings TValue="DataModel" Id="Id" Text="Text"></ListViewFieldSettings>
</SfListView>

<!-- Checkbox on Right -->
<h3>Checkbox on Right</h3>
<SfListView DataSource="@Data" 
            ShowCheckBox="true"
            CheckBoxPosition="CheckBoxPosition.Right">
    <ListViewFieldSettings TValue="DataModel" Id="Id" Text="Text"></ListViewFieldSettings>
</SfListView>

@code {
    private DataModel[] Data = {
        new DataModel { Text = "Hennessey Venom", Id = "list-01" },
        new DataModel { Text = "Bugatti Chiron", Id = "list-02" },
        new DataModel { Text = "Bugatti Veyron Super Sport", Id = "list-03" },
        new DataModel { Text = "SSC Ultimate Aero", Id = "list-04" },
        new DataModel { Text = "Koenigsegg CCR", Id = "list-05" }
    };

    public class DataModel
    {
        public string Text { get; set; }
        public string Id { get; set; }
    }
}
```

**CheckBoxPosition Values:**
- `CheckBoxPosition.Left` - Checkbox appears before item text (default)
- `CheckBoxPosition.Right` - Checkbox appears after item text
## Selection Events

### Clicked Event

Respond to item clicks:

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView DataSource="@Items">
    <ListViewFieldSettings TValue="Item" Id="Id" Text="Name"></ListViewFieldSettings>
    <ListViewEvents TValue="Item"
                    Clicked="@(args => OnItemClicked(args))">
    </ListViewEvents>
</SfListView>

<p>Selected: @SelectedItemText</p>

@code {
    string SelectedItemText = "";

    void OnItemClicked(ClickEventArgs<Item> args)
    {
        SelectedItemText = args.Text;
    }

    List<Item> Items = new List<Item>
    {
        new Item { Id = "1", Name = "Option 1" },
        new Item { Id = "2", Name = "Option 2" }
    };

    public class Item
    {
        public string Id { get; set; }
        public string Name { get; set; }
    }
}
```

### OnActionBegin and OnActionComplete Events

```cshtml
<ListViewEvents TValue="Item"
                OnActionBegin="@(args => HandleActionBegin(args))"
                OnActionComplete="@(args => HandleActionComplete(args))">
</ListViewEvents>

@code {
    void HandleActionBegin(ActionBeginEventArgs<Item> args)
    {
        // Triggered at action start
        // args.EventType contains action type
    }

    void HandleActionComplete(ActionCompleteEventArgs<Item> args)
    {
        // Triggered when action completes
    }
}
```

## Custom Template Selection

### Selection in Custom Templates

When using custom templates with checkboxes, ensure proper field mapping:

```cshtml
@using Syncfusion.Blazor.Lists

<div id="container">
    <div class="sample flex vertical-center">
        <div class="padding">
            <SfListView @ref="ListView" 
                       DataSource="@DataSource"
                       ShowCheckBox="true"
                       CssClass="e-list-template">
                <ListViewFieldSettings TValue="ListDataModel" 
                                      Id="Id" 
                                      Text="Name">
                </ListViewFieldSettings>
                <ListViewTemplates TValue="ListDataModel">
                    <Template>
                        @{
                            ListDataModel currentData = (ListDataModel)context;
                            <div class="e-list-wrapper e-list-multi-line e-list-avatar">
                                <img src="@(currentData.Image)" class="e-avatar e-avatar-circle" />
                                <span class="e-list-item-header">@currentData.Name</span>
                                <span class="e-list-content">@currentData.Contact</span>
                            </div>
                        }
                    </Template>
                </ListViewTemplates>
            </SfListView>
        </div>
        <div class="padding">
            <button class="e-btn" @onclick="GetSelected">Get Selected Items</button>
        </div>
    </div>
</div>

@code {
    SfListView<ListDataModel> ListView;

    List<ListDataModel> DataSource = new List<ListDataModel>() {
        new ListDataModel { Name = "Nancy", Contact = "(206) 555-985774", Id = "1", Image = "url" },
        new ListDataModel { Name = "Janet", Contact = "(206) 555-3412", Id = "2", Image = "url" },
        new ListDataModel { Name = "Margaret", Contact = "(206) 555-8122", Id = "4", Image = "url" },
        new ListDataModel { Name = "Andrew", Contact = "(206) 555-9482", Id = "5", Image = "url" }
    };

    async Task GetSelected()
    {
        var selected = await ListView.GetCheckedItemsAsync();
        // Process selected items
    }

    public class ListDataModel
    {
        public string Id { get; set; }
        public string Name { get; set; }
        public string Image { get; set; }
        public string Contact { get; set; }
    }
}

<style>
    #container .e-listview {
        box-shadow: 0 1px 4px #ddd;
        border-bottom: 1px solid #ddd;
    }

    .sample {
        justify-content: center;
        min-height: 280px;
    }

    .vertical-center {
        align-items: center;
    }

    .padding {
        padding: 4px;
    }

    .flex {
        display: flex;
    }
</style>
```

### Click Handler with Custom Data

```cshtml
<SfListView DataSource="@Items" CssClass="e-list-template">
    <ListViewFieldSettings TValue="Item" Id="Id" Text="Name"></ListViewFieldSettings>
    <ListViewTemplates TValue="Item">
        <Template>
            <div class="e-list-wrapper" @onclick="@((e) => SelectItem((Item)context))">
                <span class="e-list-content">@((context as Item).Name)</span>
            </div>
        </Template>
    </ListViewTemplates>
</SfListView>

@code {
    Item SelectedItem;

    void SelectItem(Item item)
    {
        SelectedItem = item;
    }

    List<Item> Items = new List<Item>
    {
        new Item { Id = "1", Name = "Item 1" },
        new Item { Id = "2", Name = "Item 2" }
    };

    public class Item
    {
        public string Id { get; set; }
        public string Name { get; set; }
    }
}
```

---

**Related Topics:**
- [Filtering and Searching](filtering-and-searching.md) - Filter selected results
- [Data Binding and Sources](data-binding-and-source.md) - Manage data dynamically
- [Templating and Rendering](templating-and-rendering.md) - Create selection templates

