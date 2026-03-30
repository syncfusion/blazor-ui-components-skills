# Events and Callbacks

## Table of Contents
- [Lifecycle Events](#lifecycle-events)
- [Popup Events](#popup-events)
- [Programmatically Controlling Popup](#programmatically-controlling-popup)
- [Selection Events](#selection-events)
- [Filtering Event](#filtering-event)
- [Action Failure Event](#action-failure-event)
- [Event Arguments](#event-arguments)

## Lifecycle Events

### Created Event

Triggered once the Dropdown Tree component has been successfully created and initialized.

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    Created="OnCreated">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", HasChild = true },
    };

    void OnCreated()
    {
        Console.WriteLine("Dropdown Tree has been created successfully");
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

### Destroyed Event

Triggered when the Dropdown Tree component is completely destroyed, allowing cleanup operations.

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    Destroyed="DestroyHandler">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", HasChild = true },
    };

    void DestroyHandler()
    {
        Console.WriteLine("Dropdown Tree has been destroyed");
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

## Popup Events

### OnPopupOpen Event

Triggered after the Dropdown Tree popup is opened and animation is complete.

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.DropDowns

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    OnPopupOpen="OnPopupOpen">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", HasChild = true },
    };

    void OnPopupOpen(PopupEventArgs args)
    {
        Console.WriteLine("Popup has opened");
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

### OnPopupClose Event

Triggered before the Dropdown Tree popup is closed after animation.

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.DropDowns

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    OnPopupClose="OnPopupClose">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", HasChild = true },
    };

    void OnPopupClose(PopupEventArgs args)
    {
        Console.WriteLine("Popup is closing");
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

### Preventing Popup Opening and Closing

Prevent the popup from opening or closing by setting the `Cancel` property to `true` in the event arguments. This is achieved using the `OnPopupOpen` and `OnPopupClose` events with `PopupEventArgs.Cancel`.

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.DropDowns

<SfDropDownTree @ref="sfDropDownTree" TItem="EmployeeData" TValue="string" 
    ID="open" 
    Placeholder="Select an employee" 
    Width="500px" 
    OnPopupOpen="OnPopupOpenHandler">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

<SfDropDownTree @ref="sfDropDownTree2" TItem="EmployeeData" TValue="string" 
    ID="Close" 
    Placeholder="Select an employee" 
    Width="500px" 
    OnPopupClose="OnPopupCloseHandler">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    SfDropDownTree<string, EmployeeData>? sfDropDownTree;
    SfDropDownTree<string, EmployeeData>? sfDropDownTree2;

    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", Job = "General Manager", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", Job = "Product Manager", HasChild = true },
        new EmployeeData() { Id = "3", PId = "2", Name = "Andrew Fuller", Job = "Team Lead", HasChild = true },
        new EmployeeData() { Id = "4", PId = "3", Name = "Anne Dodsworth", Job = "Developer" },
    };

    void OnPopupOpenHandler(PopupEventArgs args)
    {
        args.Cancel = true;
    }

    void OnPopupCloseHandler(PopupEventArgs args)
    {
        args.Cancel = true;
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

## Programmatically Controlling Popup

### ShowPopupAsync Method

You can programmatically open the Dropdown Tree popup by calling the `ShowPopupAsync()` method through a reference to the component instance. This is useful when you need to trigger the popup from external buttons or other events.

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="OpenPopup">Open Popup</SfButton>

<div>
    <SfDropDownTree @ref="sfDropDownTree" 
        TItem="EmployeeData" 
        TValue="string" 
        Placeholder="Select an employee" 
        Width="500px" 
        ID="dropdowntree">
        <DropDownTreeField TItem="EmployeeData" 
            DataSource="Data" 
            ID="Id" 
            Text="Name" 
            HasChildren="HasChild" 
            ParentID="PId">
        </DropDownTreeField>
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
        new EmployeeData() { Id = "9", PId = "1", Name = "Janet Leverling", Job = "HR" }
    };

    async Task OpenPopup()
    {
        if (sfDropDownTree != null)
        {
            await sfDropDownTree.ShowPopupAsync();
        }
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

### HidePopupAsync Method

You can programmatically close the Dropdown Tree popup by calling the `HidePopupAsync()` method. This allows you to close the popup from buttons or after specific actions are completed.

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="ClosePopup">Close Popup</SfButton>

<div>
    <SfDropDownTree @ref="sfDropDownTree" 
        TItem="EmployeeData" 
        TValue="string" 
        Placeholder="Select an employee" 
        Width="500px" 
        ID="dropdowntree">
        <DropDownTreeField TItem="EmployeeData" 
            DataSource="Data" 
            ID="Id" 
            Text="Name" 
            HasChildren="HasChild" 
            ParentID="PId">
        </DropDownTreeField>
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
        new EmployeeData() { Id = "9", PId = "1", Name = "Janet Leverling", Job = "HR" }
    };

    async Task ClosePopup()
    {
        if (sfDropDownTree != null)
        {
            await sfDropDownTree.HidePopupAsync();
        }
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

### Both Open and Close Together

This example demonstrates using both `ShowPopupAsync()` and `HidePopupAsync()` methods with different buttons for complete control over the popup state.

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="OpenPopup">Open Popup</SfButton>
<SfButton OnClick="ClosePopup">Close Popup</SfButton>

<div>
    <SfDropDownTree @ref="sfDropDownTree" 
        TItem="EmployeeData" 
        TValue="string" 
        Placeholder="Select an employee" 
        Width="500px" 
        ID="dropdowntree">
        <DropDownTreeField TItem="EmployeeData" 
            DataSource="Data" 
            ID="Id" 
            Text="Name" 
            HasChildren="HasChild" 
            ParentID="PId">
        </DropDownTreeField>
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
        new EmployeeData() { Id = "9", PId = "1", Name = "Janet Leverling", Job = "HR" }
    };

    async Task OpenPopup()
    {
        if (sfDropDownTree != null)
        {
            await sfDropDownTree.ShowPopupAsync();
        }
    }

    async Task ClosePopup()
    {
        if (sfDropDownTree != null)
        {
            await sfDropDownTree.HidePopupAsync();
        }
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

### Show Popup on Initial Loading

You can automatically show the popup when the component is created by calling `ShowPopupAsync()` in the `Created` event handler.

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree @ref="sfDropDownTree" 
    TItem="EmployeeData" 
    TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    Created="OnCreated">
    <DropDownTreeField TItem="EmployeeData" 
        DataSource="Data" 
        ID="Id" 
        Text="Name" 
        HasChildren="HasChild" 
        ParentID="PId">
    </DropDownTreeField>
</SfDropDownTree>

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
        new EmployeeData() { Id = "9", PId = "1", Name = "Janet Leverling", Job = "HR" }
    };

    async Task OnCreated()
    {
        if (sfDropDownTree != null)
        {
            await sfDropDownTree.ShowPopupAsync();
        }
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

## Selection Events

### ValueChanging Event

Triggered when an item in the popup is selected or when the model value is changed by the user (before the change is applied).

```razor
@using Syncfusion.Blazor.Popups
@using Syncfusion.Blazor.Buttons
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    ValueChanging="ValueChanged">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

<SfDialog Width="335px" IsModal="true" @bind-Visible="Visibility">
    <DialogTemplates>
        <Header>Employee Details</Header>
        <Content>
            <p class="employee">Name - @Name</p>
            <p class="employee">Role - @Role</p>
        </Content>
    </DialogTemplates>
    <DialogButtons>
        <DialogButton Content="OK" IsPrimary="true" OnClick="@DlgButtonClick" />
    </DialogButtons>
</SfDialog>

@code {
    private string Name { get; set; } = string.Empty;
    private string Role { get; set; } = string.Empty;
    private bool Visibility { get; set; }

    private void DlgButtonClick()
    {
        this.Visibility = false;
    }

    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", Job = "General Manager", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", Job = "Product Manager", HasChild = true },
        new EmployeeData() { Id = "3", PId = "2", Name = "Andrew Fuller", Job = "Team Lead", HasChild = true },
        new EmployeeData() { Id = "4", PId = "3", Name = "Anne Dodsworth", Job = "Developer" },
    };

    void ValueChanged(DdtChangeEventArgs<string> args)
    {
        EmployeeData currentData = Data?.Find((item) => item.Id == args.NodeData.Id);
        Name = currentData?.Name;
        Role = currentData?.Job;
        this.Visibility = true;
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

### ValueChanged Event

Triggered after the value changes in the Dropdown Tree component. Receives a list of changed values.

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    ValueChanged="ValueChanged">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", HasChild = true },
    };

    void ValueChanged(List<string> selectedIds)
    {
        Console.WriteLine($"Selected IDs: {string.Join(", ", selectedIds)}");
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

## Filtering Event

### Filtering Event

Triggered when the user types text in the search box. Allows custom filtering logic.

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    AllowFiltering="true" 
    Filtering="OnFiltering">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", HasChild = true },
    };

    void OnFiltering(DdtFilteringEventArgs args)
    {
        // args.Text contains the filter text
        Console.WriteLine($"Filter text: {args.Text}");
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

### Minimum Filter Length

Implement minimum character requirement before filtering:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Select an employee" 
    Width="500px" 
    AllowFiltering="true" 
    Filtering="OnFiltering">
    <DropDownTreeField TItem="EmployeeData" DataSource="Data" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", HasChild = true },
    };

    void OnFiltering(DdtFilteringEventArgs args)
    {
        // Require minimum 2 characters
        if (args.Text.Length < 2)
        {
            args.PreventDefaultAction = true;
        }
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

## Action Failure Event

### OnActionFailure Event

Triggered when any Dropdown Tree action fails (e.g., remote data fetch error). Allows error handling and recovery.

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Data

<SfDropDownTree TValue="int?" TItem="TreeData" 
    Placeholder="Select a name" 
    Width="500px" 
    OnActionFailure="OnActionFailureTemplate">
    <ChildContent>
        <DropDownTreeField TItem="TreeData" Query="@employeeQuery" 
            ID="EmployeeID" Text="FirstName" HasChildren="EmployeeID">
            <SfDataManager Url="url" 
                Adaptor="Syncfusion.Blazor.Adaptors.ODataV4Adaptor" 
                CrossDomain="true">
            </SfDataManager>
        </DropDownTreeField>
    </ChildContent>
    <ActionFailureTemplate>
        <span style="color: red;">A request to fetch data failed. Please try again.</span>
    </ActionFailureTemplate>
</SfDropDownTree>

@code {
    Query? employeeQuery;

    protected override void OnInitialized()
    {
        base.OnInitialized();
        employeeQuery = new Query().From("Employees").Select(new List<string> { "EmployeeID", "FirstName", "Title" }).Take(5);
    }

    void OnActionFailureTemplate()
    {
        Console.WriteLine("Data fetch failed");
    }

    public class TreeData
    {
        public int? EmployeeID { get; set; }
        public string? FirstName { get; set; }
    }
}
```

## Event Arguments

### PopupEventArgs

Arguments for popup events (OnPopupOpen, OnPopupClose):

```csharp
public class PopupEventArgs
{
    public bool Cancel { get; set; }  // Prevent default action
}
```

### DdtChangeEventArgs<TValue>

Arguments for ValueChanging event:

```csharp
public class DdtChangeEventArgs<TValue>
{
    public TValue Value { get; set; }           // Selected value
    public NodeData NodeData { get; set; }      // Current node data
    public bool Cancel { get; set; }            // Prevent selection
}
```

### DdtFilteringEventArgs

Arguments for Filtering event:

```csharp
public class DdtFilteringEventArgs
{
    public string Text { get; set; }              // Filter text
    public bool PreventDefaultAction { get; set; } // Skip default filtering
}
```

## Event Execution Order

1. **Component Initialization**: `Created`
2. **User Interaction**: 
   - Popup opens → `OnPopupOpen`
   - User selects item → `ValueChanging` → `ValueChanged`
   - User searches → `Filtering`
   - Popup closes → `OnPopupClose`
3. **Component Destruction**: `Destroyed`
