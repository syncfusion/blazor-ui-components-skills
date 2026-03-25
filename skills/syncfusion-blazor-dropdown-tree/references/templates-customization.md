# Templates and Customization

## Table of Contents
- [Item Template](#item-template)
- [Value Template](#value-template)
- [Header Template](#header-template)
- [Advanced Customization](#advanced-customization)

## Item Template

Customize the rendering of each tree node in the popup using the `ItemTemplate`:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px">
    <ChildContent>
        <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
    </ChildContent>
    <ItemTemplate>
        <div>
            <span class="ename">@((context as EmployeeData).Name) - </span>
            <span class="ejob" style="opacity: .60">@((context as EmployeeData).Job)</span>
        </div>
    </ItemTemplate>
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

**Key Points:**
- `context` contains the current node data
- Template is applied to all nodes in the tree
- Supports inline HTML and Blazor components
- Variable `context` is of type `EmployeeData` (cast as needed)

### Item Template with Icons

```razor
<ItemTemplate>
    <div style="display: flex; align-items: center; gap: 10px;">
        @if (((context as EmployeeData).HasChild))
        {
            <span class="e-icons e-folder" style="font-size: 18px;"></span>
        }
        else
        {
            <span class="e-icons e-file" style="font-size: 18px;"></span>
        }
        <div>
            <span style="font-weight: 500;">@((context as EmployeeData).Name)</span>
            <br />
            <span style="font-size: 12px; color: #888;">@((context as EmployeeData).Job)</span>
        </div>
    </div>
</ItemTemplate>
```

## Value Template

Customize the display of the selected value in the input box using the `ValueTemplate`:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px">
    <ChildContent>
        <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId" Expanded="Expanded"></DropDownTreeField>
    </ChildContent>
    <ValueTemplate>
        <span>@((context as EmployeeData).Name) - @((context as EmployeeData).Job)</span>
    </ValueTemplate>
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

**Key Points:**
- `context` contains the selected node data
- Template is applied only to selected value display
- Used to format selected value differently from item display
- Supports multi-selection formatting

### Multi-Selection Value Display

```razor
<ValueTemplate>
    <div style="padding: 5px 0;">
        @if (@context is List<EmployeeData> selectedItems)
        {
            @foreach (var item in selectedItems.Take(2))
            {
                <span style="background: #ddd; padding: 2px 8px; margin-right: 5px; border-radius: 3px;">@item.Name</span>
            }
            @if (selectedItems.Count > 2)
            {
                <span style="background: #ddd; padding: 2px 8px; border-radius: 3px;">+@(selectedItems.Count - 2)</span>
            }
        }
    </div>
</ValueTemplate>
```

## Header Template

Customize the popup header using the `HeaderTemplate`:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    CssClass="custom">
    <ChildContent>
        <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
    </ChildContent>
    <HeaderTemplate>
        <div class="head"> Employee List </div>
    </HeaderTemplate>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", HasChild = true },
        new EmployeeData() { Id = "3", PId = "2", Name = "Andrew Fuller", HasChild = true },
        new EmployeeData() { Id = "4", PId = "3", Name = "Anne Dodsworth" },
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

### Advanced Header with Controls

```razor
<HeaderTemplate>
    <div style="display: flex; justify-content: space-between; align-items: center; padding: 10px 15px; border-bottom: 1px solid #ddd;">
        <span style="font-weight: 600;">Select Employee</span>
        <span style="font-size: 12px; color: #888;">@Data?.Count items</span>
    </div>
</HeaderTemplate>
```

## Advanced Customization

### Complete Custom Template Example

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px"
    AllowFiltering="true">
    <ChildContent>
        <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
    </ChildContent>
    
    <HeaderTemplate>
        <div style="background: #f5f5f5; padding: 12px 15px; border-bottom: 1px solid #ddd;">
            <strong>Organizational Structure</strong>
            <p style="margin: 5px 0; font-size: 12px; color: #666;">Select an employee</p>
        </div>
    </HeaderTemplate>
    
    <ItemTemplate>
        <div style="padding: 8px 5px; display: flex; align-items: center; gap: 10px;">
            <div style="width: 35px; height: 35px; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-weight: bold;">
                @((context as EmployeeData).Name?[0] ?? 'E')
            </div>
            <div style="flex: 1;">
                <div style="font-weight: 500;">@((context as EmployeeData).Name)</div>
                <div style="font-size: 12px; color: #666;">@((context as EmployeeData).Job)</div>
            </div>
            @if (((context as EmployeeData).HasChild))
            {
                <span class="e-icons" style="color: #999;">+</span>
            }
        </div>
    </ItemTemplate>
    
    <ValueTemplate>
        <div style="display: flex; align-items: center; gap: 8px;">
            <div style="width: 25px; height: 25px; background: #667eea; border-radius: 50%; display: flex; align-items: center; justify-content: center; color: white; font-size: 12px; font-weight: bold;">
                @((context as EmployeeData).Name?[0] ?? 'E')
            </div>
            <span>@((context as EmployeeData).Name)</span>
        </div>
    </ValueTemplate>
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

## Template Context Type

When using templates, the `context` variable contains the node data:

```csharp
// Cast to access data properties
var employee = (context as EmployeeData);
var name = employee?.Name;
var job = employee?.Job;
var hasChildren = employee?.HasChild ?? false;
```

## Template Styling

Templates support inline styles and CSS classes:

```razor
<!-- Inline styles -->
<div style="color: red; font-weight: bold;">
    @((context as EmployeeData).Name)
</div>

<!-- CSS classes -->
<div class="employee-item">
    @((context as EmployeeData).Name)
</div>
```

```css
/* In component or global CSS */
.employee-item {
    padding: 8px;
    background-color: #f9f9f9;
    border-left: 3px solid #667eea;
}
```

## Template Best Practices

1. **Performance**: Minimize complex logic in templates
2. **Accessibility**: Include alt text for images and proper ARIA labels
3. **Responsive**: Use flexible layouts for mobile support
4. **Consistency**: Match template styling across item, value, and header
5. **Null Handling**: Always check for null values before accessing properties
6. **Type Safety**: Cast context to specific type for IntelliSense support
