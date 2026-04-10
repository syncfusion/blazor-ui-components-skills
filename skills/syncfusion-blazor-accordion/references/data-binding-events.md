# Data Binding and Events in Blazor Accordion Component

## Table of Contents
- [Data Binding](#data-binding)
- [Template-Based Binding](#template-based-binding)
- [Dynamic Item Management](#dynamic-item-management)
- [Accordion Events](#accordion-events)
- [Event Examples](#event-examples)
- [Best Practices](#best-practices)

## Data Binding

The Accordion component supports local data binding using a `foreach` loop to iterate through a data collection and generate accordion items dynamically.

### Basic Data Binding

```cshtml
@using Syncfusion.Blazor.Navigations

<SfAccordion>
    <AccordionItems>
        @foreach (var item in AccordionData)
        {
            <AccordionItem>
                <HeaderTemplate>
                    <div>@item.EmployeeName</div>
                </HeaderTemplate>
                <ContentTemplate>
                    <div>
                        <div><b>Employee ID:</b> @item.EmployeeId</div>
                        <div><b>Designation:</b> @item.Designation</div>
                    </div>
                </ContentTemplate>
            </AccordionItem>
        }
    </AccordionItems>
</SfAccordion>

@code {
    List<AccordionData> AccordionData = new List<AccordionData>() {
        new AccordionData { EmployeeId = 1, EmployeeName = "Laura Callahan", Designation = "Product Manager" },
        new AccordionData { EmployeeId = 3, EmployeeName = "Andrew Fuller", Designation = "Team Lead" },
        new AccordionData { EmployeeId = 4, EmployeeName = "Anne Dodsworth", Designation = "Developer" },
        new AccordionData { EmployeeId = 5, EmployeeName = "Nancy Davolio", Designation = "Product Manager" }
    };

    public class AccordionData {
        public string EmployeeName { get; set; }
        public int EmployeeId { get; set; }
        public string Designation { get; set; }
    }
}
```

**Key Points:**
- Use `foreach` loop inside `<AccordionItems>` to generate items from data
- Each iteration creates an `<AccordionItem>` element
- `HeaderTemplate` and `ContentTemplate` render data properties
- Data source can be List, Array, or any IEnumerable

### When to Use Data Binding

**Use data binding when:**
- Content comes from a database or API
- Number of items varies dynamically
- Items share the same structure/template
- Need to add/remove items programmatically
- Implementing search or filter functionality

## Template-Based Binding

Templates provide full control over content rendering with access to data context.

### HeaderTemplate

Customize the header appearance with HTML and data:

```cshtml
<AccordionItem>
    <HeaderTemplate>
        <div style="display: flex; justify-content: space-between; width: 100%;">
            <span><strong>@item.Title</strong></span>
            <span class="badge">@item.Count</span>
        </div>
    </HeaderTemplate>
    <ContentTemplate>
        <div>@item.Description</div>
    </ContentTemplate>
</AccordionItem>
```

### ContentTemplate

Render complex content structures:

```cshtml
<AccordionItem>
    <HeaderTemplate>
        <div>@employee.Name</div>
    </HeaderTemplate>
    <ContentTemplate>
        <div class="employee-card">
            <img src="@employee.PhotoUrl" alt="@employee.Name" />
            <div class="employee-details">
                <p><b>ID:</b> @employee.Id</p>
                <p><b>Department:</b> @employee.Department</p>
                <p><b>Email:</b> @employee.Email</p>
            </div>
        </div>
    </ContentTemplate>
</AccordionItem>
```

### Conditional Rendering in Templates

Apply conditional logic within templates:

```cshtml
<AccordionItem>
    <HeaderTemplate>
        <div>
            @item.Title
            @if (item.IsNew) {
                <span class="badge-new">New</span>
            }
        </div>
    </HeaderTemplate>
    <ContentTemplate>
        <div>
            @if (item.HasContent) {
                <p>@item.Content</p>
            } else {
                <p>No content available.</p>
            }
        </div>
    </ContentTemplate>
</AccordionItem>
```

## Dynamic Item Management

Add, remove, or modify accordion items at runtime by manipulating the data source.

### Adding Items Dynamically

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton @onclick="AddItem">Add Item</SfButton>

<SfAccordion>
    <AccordionItems>
        @foreach (var item in Items)
        {
            <AccordionItem @bind-Expanded="item.IsExpanded">
                <HeaderTemplate>
                    <div>@item.Header</div>
                </HeaderTemplate>
                <ContentTemplate>
                    <div>@item.Content</div>
                </ContentTemplate>
            </AccordionItem>
        }
    </AccordionItems>
</SfAccordion>

@code {
    List<AccordionItem Data> Items = new List<AccordionItemData>() {
        new AccordionItemData { Header = "Item 1", Content = "Content 1", IsExpanded = true },
        new AccordionItemData { Header = "Item 2", Content = "Content 2", IsExpanded = false }
    };

    void AddItem() {
        Items.Add(new AccordionItemData {
            Header = $"Item {Items.Count + 1}",
            Content = $"Content for item {Items.Count + 1}",
            IsExpanded = false
        });
    }

    public class AccordionItemData {
        public string Header { get; set; }
        public string Content { get; set; }
        public bool IsExpanded { get; set; }
    }
}
```

### Removing Items Dynamically

```cshtml
<SfButton @onclick="RemoveItem">Remove First Item</SfButton>

<SfAccordion>
    <AccordionItems>
        @foreach (var item in Items)
        {
            <AccordionItem>
                <HeaderTemplate>
                    <div>@item.Header</div>
                </HeaderTemplate>
                <ContentTemplate>
                    <div>@item.Content</div>
                </ContentTemplate>
            </AccordionItem>
        }
    </AccordionItems>
</SfAccordion>

@code {
    List<AccordionItemData> Items = new List<AccordionItemData>() {
        new AccordionItemData { Header = "Item 1", Content = "Content 1" },
        new AccordionItemData { Header = "Item 2", Content = "Content 2" },
        new AccordionItemData { Header = "Item 3", Content = "Content 3" }
    };

    void RemoveItem() {
        if (Items.Count > 0) {
            Items.RemoveAt(0);
        }
    }
}
```

### Updating Items

Modify item properties to update accordion content:

```cshtml
<SfButton @onclick="UpdateItem">Update First Item</SfButton>

<SfAccordion>
    <AccordionItems>
        @foreach (var item in Items)
        {
            <AccordionItem>
                <HeaderTemplate>
                    <div>@item.Header</div>
                </HeaderTemplate>
                <ContentTemplate>
                    <div>@item.Content</div>
                </ContentTemplate>
            </AccordionItem>
        }
    </AccordionItems>
</SfAccordion>

@code {
    List<AccordionItemData> Items = new List<AccordionItemData>();

    void UpdateItem() {
        if (Items.Count > 0) {
            Items[0].Header = "Updated Header";
            Items[0].Content = "Updated Content";
            StateHasChanged(); // Trigger UI refresh
        }
    }
}
```

## Accordion Events

The Accordion component provides events for various user interactions and lifecycle stages. All events are configured within a single `<AccordionEvents>` component.

### Available Events

| Event | Description | Event Args Type |
|-------|-------------|-----------------|
| `Created` | Triggered after the accordion is created and rendered | `object` |
| `Destroyed` | Triggered when the accordion is destroyed | `object` |
| `Clicked` | Triggered when clicking anywhere within the accordion | `AccordionClickArgs` |
| `Expanding` | Triggered before an item expands | `ExpandEventArgs` |
| `Expanded` | Triggered after an item has expanded | `ExpandedEventArgs` |
| `Collapsing` | Triggered before an item collapses | `CollapseEventArgs` |
| `Collapsed` | Triggered after an item has collapsed | `CollapsedEventArgs` |

### Event Configuration

All events must be configured within a single `AccordionEvents` component:

```cshtml
<SfAccordion>
    <AccordionEvents 
        Created="OnCreated"
        Expanding="OnExpanding"
        Expanded="OnExpanded"
        Collapsing="OnCollapsing"
        Collapsed="OnCollapsed"
        Clicked="OnClicked"
        Destroyed="OnDestroyed">
    </AccordionEvents>
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

**Important:** Configure all events within a single `<AccordionEvents>` component, not multiple instances.

## Event Examples

### Created Event

Triggered once when the accordion is fully rendered:

```cshtml
<SfAccordion>
    <AccordionEvents Created="OnCreated"></AccordionEvents>
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    void OnCreated(object args) {
        // Perform initialization after accordion is created
        Console.WriteLine("Accordion created successfully");
    }
}
```

**Use cases:**
- Initialize external libraries
- Log component creation
- Perform post-render setup

### Destroyed Event

Triggered when the accordion component is removed:

```cshtml
<SfAccordion>
    <AccordionEvents Destroyed="OnDestroyed"></AccordionEvents>
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    void OnDestroyed(object args) {
        // Cleanup operations
        Console.WriteLine("Accordion destroyed");
    }
}
```

**Use cases:**
- Cleanup external resources
- Unsubscribe from events
- Log component disposal

### Clicked Event

Triggered when clicking anywhere within the accordion:

```cshtml
<SfAccordion>
    <AccordionEvents Clicked="OnClicked"></AccordionEvents>
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
        <AccordionItem Header="Item 2" Content="Content 2"></AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    void OnClicked(AccordionClickArgs args) {
        Console.WriteLine($"Clicked on accordion");
    }
}
```

**Use cases:**
- Track user interactions
- Implement custom analytics
- Handle global accordion click events

### Expanding Event

Triggered before an item expands (can be cancelled):

```cshtml
<SfAccordion>
    <AccordionEvents Expanding="OnExpanding"></AccordionEvents>
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
        <AccordionItem Header="Item 2" Content="Content 2"></AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    void OnExpanding(ExpandEventArgs args) {
        // Perform validation before expansion
        Console.WriteLine($"Item {args.Index} is about to expand");
        
        // Optionally prevent expansion
        // args.Cancel = true;
    }
}
```

**Use cases:**
- Validate before expansion
- Load data before showing content
- Prevent expansion under certain conditions
- Show loading indicators

### Expanded Event

Triggered after an item has fully expanded:

```cshtml
<SfAccordion>
    <AccordionEvents Expanded="OnExpanded"></AccordionEvents>
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
        <AccordionItem Header="Item 2" Content="Content 2"></AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    void OnExpanded(ExpandedEventArgs args) {
        Console.WriteLine($"Item {args.Index} is now expanded");
        // Perform post-expansion actions
    }
}
```

**Use cases:**
- Track which items are viewed
- Load additional data after expansion
- Update UI state
- Trigger animations

### Collapsing Event

Triggered before an item collapses (can be cancelled):

```cshtml
<SfAccordion>
    <AccordionEvents Collapsing="OnCollapsing"></AccordionEvents>
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    void OnCollapsing(CollapseEventArgs args) {
        Console.WriteLine($"Item {args.Index} is about to collapse");
        
        // Optionally prevent collapse
        // args.Cancel = true;
    }
}
```

**Use cases:**
- Validate before collapse
- Warn users about unsaved changes
- Prevent collapse in certain states

### Collapsed Event

Triggered after an item has fully collapsed:

```cshtml
<SfAccordion>
    <AccordionEvents Collapsed="OnCollapsed"></AccordionEvents>
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    void OnCollapsed(CollapsedEventArgs args) {
        Console.WriteLine($"Item {args.Index} is now collapsed");
    }
}
```

**Use cases:**
- Track user behavior
- Update application state
- Clean up temporary data

### Preventing Expansion/Collapse

Use the `Cancel` property in event args to prevent the action:

```cshtml
<SfAccordion>
    <AccordionEvents Expanding="OnExpanding" Collapsing="OnCollapsing"></AccordionEvents>
    <AccordionItems>
        <AccordionItem Header="Item 1 (Can't expand)" Content="Content 1"></AccordionItem>
        <AccordionItem Expanded="true" Header="Item 2 (Can't collapse)" Content="Content 2"></AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    void OnExpanding(ExpandEventArgs args) {
        if (args.Index == 0) {
            args.Cancel = true; // Prevent Item 1 from expanding
        }
    }
    
    void OnCollapsing(CollapseEventArgs args) {
        if (args.Index == 1) {
            args.Cancel = true; // Prevent Item 2 from collapsing
        }
    }
}
```

## Best Practices

### Data Binding

1. **Use strongly-typed models** for data consistency
2. **Keep data models simple** to avoid unnecessary complexity
3. **Initialize data in OnInitialized** lifecycle method
4. **Use async/await** for data fetching
5. **Call StateHasChanged()** after manual data updates

### Event Handling

1. **Don't perform heavy operations** in Expanding/Collapsing events
2. **Use async handlers** for I/O operations
3. **Avoid infinite loops** when modifying state in events
4. **Use Expanding event** for validation and data loading
5. **Use Expanded event** for post-action tasks

### Performance

1. **Use LoadOnDemand="true"** with data binding
2. **Implement virtualization** for large datasets
3. **Avoid re-rendering** entire lists unnecessarily
4. **Cache fetched data** to prevent redundant API calls
5. **Debounce user actions** if updating frequently

### Error Handling

```cshtml
<AccordionEvents Expanding="OnExpanding"></AccordionEvents>

@code {
    async void OnExpanding(ExpandEventArgs args) {
        try {
            var data = await FetchDataAsync(args.Index);
            // Process data
        } catch (Exception ex) {
            Console.WriteLine($"Error: {ex.Message}");
            args.Cancel = true; // Prevent expansion on error
        }
    }
}
```

## Related Topics

- [Getting Started](getting-started.md) - Initial setup and basic implementation
- [Expand Modes](expand-modes.md) - Controlling expansion behavior
- [Content Rendering](content-rendering.md) - LoadOnDemand and performance
- [How-To Scenarios](how-to-scenarios.md) - Practical examples including dynamic items and event handling
