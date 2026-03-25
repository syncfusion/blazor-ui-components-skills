# Events and API Reference

## Table of Contents
- [Event Reference](#event-reference)
- [Event Handling](#event-handling)
- [API Methods](#api-methods)
- [Properties](#properties)
- [Reactive Patterns](#reactive-patterns)

## Event Reference

### Complete Event List

| Event | When Fired | Arguments |
|-------|-----------|-----------|
| `Change` | Selection changed | `MultiSelectChangeEventArgs` |
| `Focus` | Component focused | `FocusEventArgs` |
| `Blur` | Component loses focus | `BlurEventArgs` |
| `Open` | Popup opened | `OpenEventArgs` |
| `Close` | Popup closed | `CloseEventArgs` |
| `Filtering` | Filter text entered | `FilteringEventArgs` |
| `CustomValueSelection` | Custom value entered | `CustomValueEventArgs` |
| `OnRemove` | Item removed from selection | `RemoveEventArgs` |

### Change Event

Fired when user selects or deselects items:

```csharp
@page "/events-change"
@using Syncfusion.Blazor.DropDowns

<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item"
               Placeholder="Select items">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
    <MultiSelectEvents TValue="List<string>" TItem="Item"
                       Change="OnSelectionChange">
    </MultiSelectEvents>
</SfMultiSelect>

<p>@EventLog</p>

@code {
    private List<Item> Items { get; set; } = new();
    private string EventLog { get; set; } = "";

    protected override void OnInitialized()
    {
        Items = new List<Item>
        {
            new Item { ID = "1", Name = "Item 1" },
            new Item { ID = "2", Name = "Item 2" }
        };
    }

    private void OnSelectionChange(MultiSelectChangeEventArgs<List<string>> args)
    {
        EventLog = $"Selected: {string.Join(", ", args.Value ?? new List<string>())}";
    }

    public class Item { public string ID { get; set; } public string Name { get; set; } }
}
```

**Arguments:**
```csharp
public class MultiSelectChangeEventArgs<T>
{
    public T Value { get; set; }        // Current selected values
    public T PreviousValue { get; set; } // Previous selected values
    public MultiSelectChangeEventArgs IsInteracted { get; set; }  // User-triggered
}
```

### Focus and Blur Events

```csharp
<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
    <MultiSelectEvents TValue="List<string>" TItem="Item"
                       Focus="OnFocus"
                       Blur="OnBlur">
    </MultiSelectEvents>
</SfMultiSelect>

@code {
    private void OnFocus(FocusEventArgs args)
    {
        Console.WriteLine("Component focused");
    }

    private void OnBlur(BlurEventArgs args)
    {
        Console.WriteLine("Component lost focus");
    }

    private List<Item> Items { get; set; } = new();
}
```

### Open and Close Events

```csharp
<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
    <MultiSelectEvents TValue="List<string>" TItem="Item"
                       Open="OnOpen"
                       Close="OnClose">
    </MultiSelectEvents>
</SfMultiSelect>

@code {
    private bool IsPopupOpen { get; set; } = false;

    private void OnOpen(OpenEventArgs args)
    {
        IsPopupOpen = true;
        Console.WriteLine("Popup opened");
    }

    private void OnClose(CloseEventArgs args)
    {
        IsPopupOpen = false;
        Console.WriteLine("Popup closed");
    }

    private List<Item> Items { get; set; } = new();
}
```

### Filtering Event

```csharp
<SfMultiSelect DataSource="@FilteredItems" 
               TValue="List<string>"
               TItem="Item"
               AllowFiltering="true">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
    <MultiSelectEvents TValue="List<string>" TItem="Item"
                       Filtering="OnFiltering">
    </MultiSelectEvents>
</SfMultiSelect>

@code {
    private List<Item> AllItems { get; set; } = new();
    private List<Item> FilteredItems { get; set; } = new();

    private void OnFiltering(FilteringEventArgs args)
    {
        args.PreventDefaultAction = true;  // Custom filtering

        FilteredItems = AllItems
            .Where(x => x.Name.Contains(args.Text, StringComparison.OrdinalIgnoreCase))
            .ToList();
    }

    private List<Item> Items { get; set; } = new();
}
```

### Custom Value Event

```csharp
<SfMultiSelect DataSource="@Items" 
               AllowCustomValue="true">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
    <MultiSelectEvents TValue="List<string>" TItem="Item"
                       CustomValueSelection="OnCustomValue">
    </MultiSelectEvents>
</SfMultiSelect>

@code {
    private void OnCustomValue(CustomValueEventArgs args)
    {
        // Handle custom value
        Console.WriteLine($"Custom value: {args.NewValue}");
        
        // Optionally add to data source
        Items.Add(new Item 
        { 
            ID = Guid.NewGuid().ToString(), 
            Name = args.NewValue 
        });
    }

    private List<Item> Items { get; set; } = new();
}
```

## Event Handling

### Async Event Handlers

```csharp
<SfMultiSelect DataSource="@Items" 
               TValue="List<string>"
               TItem="Item">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
    <MultiSelectEvents TValue="List<string>" TItem="Item"
                       Change="OnSelectionChangeAsync">
    </MultiSelectEvents>
</SfMultiSelect>

@code {
    private async Task OnSelectionChangeAsync(MultiSelectChangeEventArgs<List<string>> args)
    {
        // Async operations like API calls
        var result = await FetchDataAsync(args.Value);
        Console.WriteLine($"Result: {result}");
    }

    private async Task<string> FetchDataAsync(List<string> values)
    {
        // Simulate API call
        await Task.Delay(1000);
        return $"Processed {values.Count} items";
    }

    private List<Item> Items { get; set; } = new();
}
```

### Preventing Default Behavior

```csharp
private void OnFiltering(FilteringEventArgs args)
{
    args.PreventDefaultAction = true;  // Skip default filter
    // Implement custom filtering logic
}
```

## API Methods

### Open and Close Popup

```csharp
@page "/api-methods"
@using Syncfusion.Blazor.DropDowns

<SfMultiSelect @ref="MultiSelectRef"
               DataSource="@Items" 
               TValue="List<string>"
               TItem="Item"
               Placeholder="See buttons below">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>

<button @onclick="OpenPopup">Open</button>
<button @onclick="ClosePopup">Close</button>
<button @onclick="FocusComponent">Focus</button>
<button @onclick="ClearSelection">Clear</button>

@code {
    private SfMultiSelect<Item, List<string>> MultiSelectRef;
    private List<Item> Items { get; set; } = new();

    protected override void OnInitialized()
    {
        Items = new List<Item>
        {
            new Item { ID = "1", Name = "Item 1" },
            new Item { ID = "2", Name = "Item 2" }
        };
    }

    private async Task OpenPopup()
    {
        if (MultiSelectRef != null)
            await MultiSelectRef.ShowPopupAsync();
    }

    private async Task ClosePopup()
    {
        if (MultiSelectRef != null)
            await MultiSelectRef.HidePopupAsync();
    }

    private async Task FocusComponent()
    {
        if (MultiSelectRef != null)
            await MultiSelectRef.FocusAsync();
    }

    private async Task ClearSelection()
    {
        if (MultiSelectRef != null)
            await MultiSelectRef.ClearAsync();
    }

    public class Item { public string ID { get; set; } public string Name { get; set; } }
}
```

### Refresh Data

```csharp
private async Task RefreshData()
{
    // Update data source
    Items = new List<Item>
    {
        new Item { ID = "1", Name = "Updated Item 1" },
        new Item { ID = "2", Name = "Updated Item 2" }
    };

    // Refresh component
    if (MultiSelectRef != null)
        await MultiSelectRef.Refresh();
}
```

## Properties

### Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `Value` | `List<T>` | Currently selected values |
| `DataSource` | `IEnumerable<T>` | Items to display |
| `Enabled` | `bool` | Enable/disable component |
| `Readonly` | `bool` | Read-only mode |
| `AllowFiltering` | `bool` | Enable filtering |
| `ShowCheckbox` | `bool` | Show checkboxes |
| `Placeholder` | `string` | Placeholder text |
| `PopupHeight` | `string` | Popup height |
| `AllowGrouping` | `bool` | Enable grouping |
| `Mode` | `VisualMode` | Display mode (Box, Delimiter, Default) |

### Accessing Properties

```csharp
@page "/properties"
@using Syncfusion.Blazor.DropDowns

<SfMultiSelect @ref="MultiSelectRef"
               @bind-Value="SelectedValues"
               DataSource="@Items" 
               TValue="List<string>"
               TItem="Item">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>

<button @onclick="GetInfo">Get Info</button>
<p>@Info</p>

@code {
    private SfMultiSelect<Item, List<string>> MultiSelectRef;
    private List<string> SelectedValues { get; set; } = new();
    private List<Item> Items { get; set; } = new();
    private string Info { get; set; } = "";

    protected override void OnInitialized()
    {
        Items = new List<Item>
        {
            new Item { ID = "1", Name = "Item 1" },
            new Item { ID = "2", Name = "Item 2" }
        };
    }

    private void GetInfo()
    {
        Info = $"Selected: {SelectedValues.Count}, Total: {Items.Count}";
    }

    public class Item { public string ID { get; set; } public string Name { get; set; } }
}
```

## Reactive Patterns

### Binding and State Management

```csharp
@page "/reactive-state"
@using Syncfusion.Blazor.DropDowns

<SfMultiSelect DataSource="@Items" 
               @bind-Value="SelectedItems"
               TValue="List<string>"
               TItem="Item"
               Placeholder="Select items">
    <MultiSelectFieldSettings Text="Name" Value="ID"></MultiSelectFieldSettings>
</SfMultiSelect>

<div>
    <h4>Selected Items: @SelectedItems.Count</h4>
    <ul>
        @foreach (var item in SelectedItems)
        {
            <li>@item</li>
        }
    </ul>
</div>

@code {
    private List<string> SelectedItems { get; set; } = new();
    private List<Item> Items { get; set; } = new();

    protected override void OnInitialized()
    {
        Items = new List<Item>
        {
            new Item { ID = "1", Name = "Item 1" },
            new Item { ID = "2", Name = "Item 2" },
            new Item { ID = "3", Name = "Item 3" }
        };
    }

    public class Item { public string ID { get; set; } public string Name { get; set; } }
}
```

### Cascading Updates

```csharp
@page "/cascading"
@using Syncfusion.Blazor.DropDowns

<p>Department: @SelectedDepartment</p>
<SfMultiSelect @bind-Value="SelectedDepartment"
               DataSource="@Departments" 
               TValue="string"
               TItem="string"
               Placeholder="Select department">
</SfMultiSelect>

<p>Employees in @SelectedDepartment:</p>
<SfMultiSelect DataSource="@FilteredEmployees" 
               @bind-Value="SelectedEmployees"
               TValue="List<string>"
               TItem="Employee"
               Placeholder="Select employees">
    <MultiSelectFieldSettings Text="Name" Value="EmployeeID"></MultiSelectFieldSettings>
</SfMultiSelect>

@code {
    private string SelectedDepartment { get; set; } = "IT";
    private List<string> SelectedEmployees { get; set; } = new();
    
    private List<string> Departments { get; set; } = new() { "IT", "HR", "Finance" };
    private List<Employee> AllEmployees { get; set; } = new();
    private List<Employee> FilteredEmployees { get; set; } = new();

    protected override void OnInitialized()
    {
        AllEmployees = new List<Employee>
        {
            new Employee { EmployeeID = "1", Name = "Alice", Department = "IT" },
            new Employee { EmployeeID = "2", Name = "Bob", Department = "HR" },
            new Employee { EmployeeID = "3", Name = "Charlie", Department = "IT" }
        };
        
        UpdateEmployeeList();
    }

    protected override void OnParametersSet()
    {
        UpdateEmployeeList();
    }

    private void UpdateEmployeeList()
    {
        FilteredEmployees = AllEmployees
            .Where(x => x.Department == SelectedDepartment)
            .ToList();
    }

    public class Employee
    {
        public string EmployeeID { get; set; }
        public string Name { get; set; }
        public string Department { get; set; }
    }
}
```

## Key Takeaways

✅ `Change` event fires on selection updates  
✅ Use async handlers for API calls  
✅ `PreventDefaultAction` for custom behavior  
✅ Reference component for API method calls  
✅ Binding provides real-time state sync  
✅ Cascading updates enable dependent selectors  

## Next Steps

- **Advanced usage** → See [Advanced Scenarios](advanced-scenarios.md)
- **Form validation** → See [Advanced Scenarios](advanced-scenarios.md#form-validation)
- **Performance tips** → See [Data Binding](data-binding.md#performance-considerations)
