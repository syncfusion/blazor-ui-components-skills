---
name: syncfusion-blazor-dropdown-tree
description: |
  Implement Syncfusion Blazor Dropdown Tree component for hierarchical data selection with single/multi-selection, checkboxes, filtering, and advanced customization. Use this skill whenever the user needs to display tree-structured data in a dropdown, select single or multiple hierarchical items, implement checkboxes for multi-item selection, filter tree data, customize templates, bind to remote data sources, or handle tree-related events and validation.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Dropdowns"
---

# Implementing Syncfusion Blazor Dropdown Tree Component

The Blazor Dropdown Tree component is a hierarchical data selection control that displays tree-structured data in a dropdown popup. It supports local and remote data binding, single and multi-selection modes, checkboxes, filtering, custom templates, and comprehensive event handling.

## When to Use This Skill

- **Display hierarchical data** in a compact dropdown format
- **Single selection**: Allow users to select one item from a tree structure (default mode)
- **Multi-selection**: Enable selection of multiple tree nodes using CTRL+SHIFT shortcuts
- **Checkbox selection**: Provide checkbox-based multi-selection with dependent state management
- **Filter tree data**: Implement search functionality to filter nodes by text
- **Remote data sources**: Bind to OData, OData V4, Web API, or custom service endpoints
- **Custom templates**: Personalize item display, selected value display, and popup header
- **Handle events**: Respond to lifecycle events, selection changes, popup actions
- **Form validation**: Integrate with form validation frameworks
- **Styling and appearance**: Apply CSS classes, disabled states, and theme customization

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup
- Web and Server app configuration
- Basic component initialization
- Data binding fundamentals
- First working example

### Selection and Modes
📄 **Read:** [references/selection-and-modes.md](references/selection-and-modes.md)
- Single selection (default behavior)
- Multi-selection with AllowMultiSelection property
- Keyboard shortcuts (CTRL, SHIFT)
- Preselected values and dynamic selection
- Getting selected values with @bind-Value
- Clearing selection

### Checkbox Features
📄 **Read:** [references/checkbox-features.md](references/checkbox-features.md)
- ShowCheckBox property for multi-item selection
- AutoUpdateCheckState for dependent parent-child relationships
- ShowSelectAll for selecting/deselecting all nodes
- SelectAllAsync and programmatic selection methods

### Data Binding and Remote Data
📄 **Read:** [references/data-binding-operations.md](references/data-binding-operations.md)
- Local data binding (hierarchical and self-referential)
- ExpandoObject and DynamicObject binding
- Remote data with ODataAdaptor, ODataV4Adaptor, WebApiAdaptor
- Entity Framework integration
- Observable collections with dynamic data updates
- Load on Demand for large datasets
- Adding/removing items dynamically
- GetTreeViewData method for retrieving node information

### Events and Callbacks
📄 **Read:** [references/events-callbacks.md](references/events-callbacks.md)
- Lifecycle events (Created, Destroyed)
- Popup events (OnPopupOpen, OnPopupClose)
- Selection events (ValueChanging, ValueChanged)
- Filtering event with custom filters
- OnActionFailure for error handling
- Event handlers and async support

### Filtering and Search
📄 **Read:** [references/filtering-search.md](references/filtering-search.md)
- AllowFiltering property to enable search
- FilterType options (StartsWith, EndsWith, Contains)
- IgnoreCase for case-insensitive filtering
- FilterBarPlaceholder customization
- Minimum filter length implementation

### Sorting
📄 **Read:** [references/sorting.md](references/sorting.md)
- SortOrder property (None, Ascending, Descending)
- Dynamic sorting updates

### Templates and Customization
📄 **Read:** [references/templates-customization.md](references/templates-customization.md)
- ItemTemplate for custom node rendering
- ValueTemplate for selected value display
- HeaderTemplate for popup header customization
- Advanced styling and layout patterns

### Styling and Appearance
📄 **Read:** [references/styling-appearance.md](references/styling-appearance.md)
- Disabled state configuration
- CssClass property with predefined classes (e-success, e-warning, e-error, e-outline)
- PopupWidth and PopupHeight customization
- ZIndex for layer management
- Theme support and responsive design

### Form Validation and Configuration
📄 **Read:** [references/validation-localization.md](references/validation-localization.md)
- Form validation integration
- Value binding in forms
- Localization support
- PopupSettings configuration
- Accessibility and placeholder settings

## Quick Start Example

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px">
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
        new EmployeeData() { Id = "4", PId = "3", Name = "Anne Dodsworth", Job = "Developer" },
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

## Common Patterns

### Pattern 1: Multi-Selection with Checkboxes
Enable checkbox-based selection with automatic parent-child state management:

```razor
<SfDropDownTree TItem="EmployeeData" TValue="string" 
    ShowCheckBox="true" 
    AutoUpdateCheckState="true"
    ShowSelectAll="true">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" 
        ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId">
    </DropDownTreeField>
</SfDropDownTree>
```

### Pattern 2: Preselected Values
Set default selected nodes using @bind-Value with AllowMultiSelection:

```razor
<SfDropDownTree TItem="EmployeeData" TValue="string" 
    AllowMultiSelection="true"
    @bind-Value="@SelectedIds">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" 
        ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId">
    </DropDownTreeField>
</SfDropDownTree>

@code {
    List<string> SelectedIds = new List<string>() { "1", "5" };
}
```

### Pattern 3: Search with Filtering
Enable search functionality with custom filter types:

```razor
<SfDropDownTree TItem="EmployeeData" TValue="string" 
    AllowFiltering="true"
    FilterType="FilterType.Contains"
    FilterBarPlaceholder="Search employees...">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" 
        ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId">
    </DropDownTreeField>
</SfDropDownTree>
```

### Pattern 4: Remote Data Binding (OData)
Bind to remote OData services with DataManager:

```razor
<SfDropDownTree TValue="int?" TItem="TreeData" Placeholder="Select an employee" Width="500px">
    <DropDownTreeField TItem="TreeData" Query="@Query" 
        ID="EmployeeID" Text="FirstName" ParentID="ReportsTo" HasChildren="HasChildren">
        <SfDataManager Url="https://services.odata.org/V4/Northwind/Northwind.svc" 
            Adaptor="Syncfusion.Blazor.Adaptors.ODataV4Adaptor" 
            CrossDomain="true">
        </SfDataManager>
    </DropDownTreeField>
</SfDropDownTree>

@code {
    public Query Query = new Query();
}
```

## Key Properties and APIs

| Property | Type | Description |
|----------|------|-------------|
| `TItem` | Generic | Model class for data items |
| `TValue` | Generic | Type of selected value(s) |
| `Value` | List<TValue> | Two-way bindable selected values |
| `DataSource` | IEnumerable | Local data source (via DropDownTreeField) |
| `AllowMultiSelection` | bool | Enable multi-node selection |
| `ShowCheckBox` | bool | Display checkboxes for selection |
| `AutoUpdateCheckState` | bool | Auto-sync parent-child checkbox states |
| `ShowSelectAll` | bool | Display select/deselect all checkbox |
| `AllowFiltering` | bool | Enable search filtering |
| `FilterType` | FilterType | Filter matching strategy (StartsWith, EndsWith, Contains) |
| `IgnoreCase` | bool | Case-insensitive filtering |
| `LoadOnDemand` | bool | Lazy-load child nodes on expand |
| `Placeholder` | string | Input placeholder text |
| `Width` | string | Component width |
| `PopupWidth` | string | Dropdown popup width |
| `PopupHeight` | string | Dropdown popup height |
| `ZIndex` | int | CSS z-index of popup |
| `CssClass` | string | Custom CSS classes |
| `Disabled` | bool | Disable component interaction |

## Key Events

- **Created**: Component initialization complete
- **Destroyed**: Component destruction
- **OnPopupOpen**: Popup opened after animation
- **OnPopupClose**: Popup closed after animation
- **ValueChanging**: Before value change (DdtChangeEventArgs<TValue>)
- **ValueChanged**: After value change (List<TValue>)
- **Filtering**: Search text entered (DdtFilteringEventArgs)
- **OnActionFailure**: Error during data operations

## Key Methods

- **SelectAllAsync()**: Programmatically select all nodes
- **SelectAllAsync(false)**: Programmatically deselect all nodes
- **ClearAsync()**: Clear all selected values
- **GetTreeViewData(id)**: Get node information by ID
