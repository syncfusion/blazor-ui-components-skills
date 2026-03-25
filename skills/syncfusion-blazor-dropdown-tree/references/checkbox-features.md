# Checkbox Features

## Table of Contents
- [ShowCheckBox Property](#showcheckbox-property)
- [Auto Update Check State](#auto-update-check-state)
- [Select/Unselect All](#selectunselect-all)
- [Programmatic Selection](#programmatic-selection)

## ShowCheckBox Property

Enable checkbox-based selection by setting the `ShowCheckBox` property to `true`. When enabled, a checkbox appears before each item text in the popup, allowing users to select multiple nodes.

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    ShowCheckBox="true">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", Job = "General Manager", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", Job = "Product Manager", HasChild = true },
        new EmployeeData() { Id = "3", PId = "2", Name = "Andrew Fuller", Job = "Team Lead", HasChild = true },
        new EmployeeData() { Id = "4", PId = "3", Name = "Anne Dodsworth", Job = "Developer" },
        new EmployeeData() { Id = "10", PId = "3", Name = "Lilly", Job = "Developer" },
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

## Auto Update Check State

Enable automatic parent-child checkbox state synchronization using the `AutoUpdateCheckState` property. When enabled:
- If one or more child items are unchecked, the parent checkbox shows intermediate state
- If all child items are checked, the parent checkbox is also checked
- If a parent checkbox is checked, all child items are automatically checked

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    ShowCheckBox="true" 
    AutoUpdateCheckState="true">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", Job = "General Manager", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", Job = "Product Manager", HasChild = true },
        new EmployeeData() { Id = "3", PId = "2", Name = "Andrew Fuller", Job = "Team Lead", HasChild = true },
        new EmployeeData() { Id = "4", PId = "3", Name = "Anne Dodsworth", Job = "Developer" },
        new EmployeeData() { Id = "10", PId = "3", Name = "Lilly", Job = "Developer" },
        new EmployeeData() { Id = "5", PId = "1", Name = "Nancy Davolio", Job = "Product Manager", HasChild = true },
        new EmployeeData() { Id = "6", PId = "5", Name = "Michael Suyama", Job = "Team Lead", HasChild = true },
        new EmployeeData() { Id = "7", PId = "6", Name = "Robert King", Job = "Developer" },
        new EmployeeData() { Id = "11", PId = "6", Name = "Mary", Job = "Developer" },
        new EmployeeData() { Id = "9", PId = "1", Name = "Janet Leverling", Job = "HR"}
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

## Select/Unselect All

Provide a checkbox in the popup header to select or deselect all tree items using the `ShowSelectAll` property:

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="SelectAllNode">Select all node</SfButton>
<SfButton OnClick="UnSelectAllNode">UnSelect all node</SfButton>

<div style="padding-top:20px">
    <SfDropDownTree @ref="sfDropDownTree" TItem="EmployeeData" TValue="string" 
        Placeholder="Select an employee" 
        Width="500px"  
        ShowCheckBox="true"
        ShowSelectAll="true">
        <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
    </SfDropDownTree>
</div>

@code {
    SfDropDownTree<string, EmployeeData>? sfDropDownTree;
    
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", Job = "General Manager", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", Job = "Product Manager", HasChild = true },
        new EmployeeData() { Id = "3", PId = "2", Name = "Andrew Fuller", Job = "Team Lead", HasChild = true },
        new EmployeeData() { Id = "4", PId = "3", Name = "Anne Dodsworth", Job = "Developer" },
        new EmployeeData() { Id = "10", PId = "3", Name = "Lilly", Job = "Developer" },
        new EmployeeData() { Id = "5", PId = "1", Name = "Nancy Davolio", Job = "Product Manager", HasChild = true },
        new EmployeeData() { Id = "6", PId = "5", Name = "Michael Suyama", Job = "Team Lead", HasChild = true },
        new EmployeeData() { Id = "7", PId = "6", Name = "Robert King", Job = "Developer" },
        new EmployeeData() { Id = "11", PId = "6", Name = "Mary", Job = "Developer" },
        new EmployeeData() { Id = "9", PId = "1", Name = "Janet Leverling", Job = "HR"}
    };

    async Task SelectAllNode()
    {
        await sfDropDownTree.SelectAllAsync();
    }

    async Task UnSelectAllNode()
    {
        await sfDropDownTree.SelectAllAsync(false);
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

## Programmatic Selection

### SelectAllAsync() Method

Select all tree nodes programmatically:

```csharp
await sfDropDownTree.SelectAllAsync();
```

**Parameters:** None (defaults to true)

**Returns:** Task

### SelectAllAsync(bool) Method

Select or deselect all tree nodes programmatically by passing a boolean parameter:

```csharp
// Select all
await sfDropDownTree.SelectAllAsync(true);

// Deselect all
await sfDropDownTree.SelectAllAsync(false);
```

**Parameters:**
- `value` (bool): `true` to select all, `false` to deselect all

**Returns:** Task

### Complete Example with Programmatic Control

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<div style="margin-bottom: 20px">
    <SfButton OnClick="SelectAll">Select All</SfButton>
    <SfButton OnClick="DeselectAll">Deselect All</SfButton>
    <SfButton OnClick="GetSelectedCount">Get Selected Count</SfButton>
</div>

<SfDropDownTree @ref="tree" TItem="EmployeeData" TValue="string" 
    Placeholder="Select employees" 
    Width="500px"
    ShowCheckBox="true"
    AutoUpdateCheckState="true"
    @bind-Value="@SelectedValues">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" 
        ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId">
    </DropDownTreeField>
</SfDropDownTree>

<div style="margin-top: 20px">
    <p>Selected Count: @(SelectedValues?.Count ?? 0)</p>
    <p>Selected IDs: @string.Join(", ", SelectedValues ?? new List<string>())</p>
</div>

@code {
    SfDropDownTree<string, EmployeeData>? tree;
    List<string> SelectedValues = new List<string>();

    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", HasChild = true },
        new EmployeeData() { Id = "3", PId = "2", Name = "Andrew Fuller", HasChild = true },
        new EmployeeData() { Id = "4", PId = "3", Name = "Anne Dodsworth" },
        new EmployeeData() { Id = "5", PId = "1", Name = "Nancy Davolio", HasChild = true },
        new EmployeeData() { Id = "6", PId = "5", Name = "Michael Suyama", HasChild = true },
        new EmployeeData() { Id = "7", PId = "6", Name = "Robert King" },
    };

    async Task SelectAll()
    {
        await tree.SelectAllAsync(true);
    }

    async Task DeselectAll()
    {
        await tree.SelectAllAsync(false);
    }

    void GetSelectedCount()
    {
        // SelectedValues is automatically updated through @bind-Value
        Console.WriteLine($"Total selected: {SelectedValues.Count}");
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

## CheckBox State Behavior

### Without AutoUpdateCheckState (default)

- Parent and child checkbox states are independent
- Selecting a child does not affect parent
- Selecting a parent does not affect children

### With AutoUpdateCheckState

| Condition | Behavior |
|-----------|----------|
| All children checked | Parent is checked |
| Some children unchecked | Parent shows intermediate state |
| All children unchecked | Parent is unchecked |
| Parent checked | All children automatically checked |
| Parent unchecked | All children automatically unchecked |

## API Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ShowCheckBox` | bool | false | Displays checkboxes for all nodes |
| `AutoUpdateCheckState` | bool | false | Auto-sync parent-child checkbox states |
| `ShowSelectAll` | bool | false | Display select/deselect all checkbox in header |
| `@bind-Value` | List<TValue> | null | Two-way bind to selected node IDs |

## Related Methods

- `SelectAllAsync()` - Select all nodes
- `SelectAllAsync(bool)` - Select or deselect all nodes
- `ClearAsync()` - Clear all selections
