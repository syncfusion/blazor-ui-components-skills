# Tab Item Management

## Table of Contents
- [Conditional Rendering](#conditional-rendering)
- [Dynamic Addition with AddTab()](#dynamic-addition-with-addtab)
- [Dynamic Removal with RemoveTab()](#dynamic-removal-with-removetab)
- [ShowCloseButton Property](#showclosebutton-property)
- [Disabling Tab Items](#disabling-tab-items)
- [Hiding Tab Items](#hiding-tab-items)
- [Tab Index for Navigation](#tab-index-for-navigation)

## Conditional Rendering

Conditionally render tab items using `@foreach` loops. When items are added to or removed from the collection, the DOM automatically updates.

### Basic Conditional Rendering

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton @onclick="AddItem">Add Item</SfButton>
<SfButton @onclick="RemoveItem">Remove Item</SfButton>

<SfTab>
    <TabItems>
        @foreach (var item in TabItems)
        {
            <TabItem>
                <ChildContent>
                    <TabHeader Text="@item.Header"></TabHeader>
                </ChildContent>
                <ContentTemplate>
                    <div>@item.Content</div>
                </ContentTemplate>
            </TabItem>
        }
    </TabItems>
</SfTab>

@code {
    private List<TabData> TabItems = new()
    {
        new TabData { Header = "ASP.NET", Content = "Microsoft ASP.NET is a set of technologies..." },
        new TabData { Header = "ASP.NET MVC", Content = "The Model-View-Controller architectural pattern..." },
        new TabData { Header = "ASP.NET Razor", Content = "Razor is an ASP.NET programming syntax..." }
    };
    
    private void AddItem()
    {
        TabItems.Add(new TabData
        {
            Header = "JavaScript",
            Content = "JavaScript (JS) is an interpreted computer programming language..."
        });
    }
    
    private void RemoveItem()
    {
        if (TabItems.Count > 0)
        {
            TabItems.RemoveAt(0);
        }
    }
    
    public class TabData
    {
        public string Header { get; set; }
        public string Content { get; set; }
    }
}
```

### Features
- Simple and intuitive
- Automatically updates when collection changes
- Good for moderate numbers of items
- Works well with dynamic content

## Dynamic Addition with AddTab()

The `AddTab()` method adds new tab items programmatically at a specific index. Get a component reference with `@ref` to use this method.

### Basic Usage

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="AddTabAtStart">Add Tab at Start</SfButton>
<SfButton OnClick="AddTabAtEnd">Add Tab at End</SfButton>

<SfTab @ref="TabControl" ShowCloseButton="true">
    <TabItems>
        <TabItem Content="New York City comprises 5 boroughs...">
            <ChildContent>
                <TabHeader Text="New York"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Los Angeles is a sprawling Southern California city...">
            <ChildContent>
                <TabHeader Text="Los Angeles"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Chicago is among the largest cities in the U.S...">
            <ChildContent>
                <TabHeader Text="Chicago"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private SfTab TabControl;
    
    private void AddTabAtStart()
    {
        var newTab = new List<TabItem>()
        {
            new TabItem() 
            { 
                Header = new TabHeader() { Text = "Miami" }, 
                Content = "Miami is known for its warm weather and beautiful beaches..." 
            }
        };
        TabControl.AddTab(newTab, 0); // Insert at beginning
    }
    
    private void AddTabAtEnd()
    {
        var newTab = new List<TabItem>()
        {
            new TabItem() 
            { 
                Header = new TabHeader() { Text = "Denver" }, 
                Content = "Denver is the capital of Colorado, known for being one mile above sea level..." 
            }
        };
        TabControl.AddTab(newTab, TabControl.Items.Count); // Append at end
    }
}
```

### AddTab Method Signature
```csharp
public void AddTab(List<TabItem> TabData, int Index)
```

- **TabData**: List of TabItem objects to add
- **Index**: Position where tabs will be inserted (0-based)

### Creating TabItem Programmatically

```razor
@using Syncfusion.Blazor.Navigations

// Create TabItem with header and content
var newTab = new TabItem()
{
    Header = new TabHeader() { Text = "Tab Title" },
    Content = "Tab content text here"
};

// Or with ContentTemplate (RenderFragment)
var tabWithTemplate = new TabItem()
{
    Header = new TabHeader() { Text = "Complex Tab" }
    // ContentTemplate is set separately if needed
};
```

## Dynamic Removal with RemoveTab()

The `RemoveTab()` method removes tab items by index. Use the `ShowCloseButton` property to allow users to close tabs themselves.

### Basic Removal

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="RemoveLastTab">Remove Last Tab</SfButton>
<SfButton OnClick="RemoveFirstTab">Remove First Tab</SfButton>
<SfButton OnClick="RemoveSelected">Remove Selected Tab</SfButton>

<SfTab @ref="TabControl" @bind-SelectedItem="SelectedIndex">
    <TabItems>
        <TabItem Content="Tab 1 content">
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Tab 2 content">
            <ChildContent>
                <TabHeader Text="Tab 2"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Tab 3 content">
            <ChildContent>
                <TabHeader Text="Tab 3"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private SfTab TabControl;
    private int SelectedIndex = 0;
    
    private void RemoveLastTab()
    {
        if (TabControl.Items.Count > 0)
        {
            TabControl.RemoveTab(TabControl.Items.Count - 1);
        }
    }
    
    private void RemoveFirstTab()
    {
        if (TabControl.Items.Count > 0)
        {
            TabControl.RemoveTab(0);
        }
    }
    
    private void RemoveSelected()
    {
        if (SelectedIndex < TabControl.Items.Count)
        {
            TabControl.RemoveTab(SelectedIndex);
        }
    }
}
```

### RemoveTab Method Signature
```csharp
public void RemoveTab(int Index)
```

- **Index**: Index of the tab to remove (0-based)

## ShowCloseButton Property

The `ShowCloseButton` property displays a close button on each tab header. Users can click it to remove the tab.

### Implementation

```razor
@using Syncfusion.Blazor.Navigations

<SfTab ShowCloseButton="true" Width="600px">
    <TabItems>
        <TabItem Content="Content 1">
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Content 2">
            <ChildContent>
                <TabHeader Text="Tab 2"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Content 3">
            <ChildContent>
                <TabHeader Text="Tab 3"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```

### Features
- Adds × (close) button to each tab header
- User-friendly removal without button clicks
- Automatically removes the tab when clicked
- Works seamlessly with dynamic tabs

## Disabling Tab Items

The `Disabled` property on `TabItem` prevents interaction with specific tab items. Disabled tabs appear grayed out and cannot be selected.

### Basic Usage

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton IsPrimary="true" @onclick="ToggleDisabled">Enable/Disable First Item</SfButton>

<SfTab>
    <TabItems>
        <TabItem Disabled="@IsDisabled" Content="Twitter content...">
            <ChildContent>
                <TabHeader Text="Twitter"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Facebook content...">
            <ChildContent>
                <TabHeader Text="Facebook"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="WhatsApp content...">
            <ChildContent>
                <TabHeader Text="WhatsApp"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private bool IsDisabled = false;
    
    private void ToggleDisabled()
    {
        IsDisabled = !IsDisabled;
    }
}
```

### Dynamic Disabling

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        @foreach (var item in TabItems)
        {
            <TabItem Disabled="@item.IsDisabled">
                <ChildContent>
                    <TabHeader Text="@item.Title"></TabHeader>
                </ChildContent>
                <ContentTemplate>
                    <div>@item.Content</div>
                </ContentTemplate>
            </TabItem>
        }
    </TabItems>
</SfTab>

@code {
    private List<TabItemModel> TabItems = new()
    {
        new TabItemModel { Title = "Active", Content = "Active tab", IsDisabled = false },
        new TabItemModel { Title = "Disabled", Content = "This is disabled", IsDisabled = true },
        new TabItemModel { Title = "Another", Content = "Another tab", IsDisabled = false }
    };
    
    public class TabItemModel
    {
        public string Title { get; set; }
        public string Content { get; set; }
        public bool IsDisabled { get; set; }
    }
}
```

## Hiding Tab Items

The `Visible` property controls whether a tab item is displayed. Hidden tabs are completely removed from view.

### Basic Usage

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton @onclick="ToggleVisibility">Show/Hide Item</SfButton>

<SfTab>
    <TabItems>
        <TabItem Visible="@ShowTwitter" Content="Twitter content...">
            <ChildContent>
                <TabHeader Text="Twitter"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Facebook content...">
            <ChildContent>
                <TabHeader Text="Facebook"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="WhatsApp content...">
            <ChildContent>
                <TabHeader Text="WhatsApp"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private bool ShowTwitter = true;
    
    private void ToggleVisibility()
    {
        ShowTwitter = !ShowTwitter;
    }
}
```

### Conditional Visibility

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton @onclick="ToggleAdminTab">Toggle Admin Tab</SfButton>

<SfTab>
    <TabItems>
        <TabItem Content="User content">
            <ChildContent>
                <TabHeader Text="Users"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Visible="@IsAdmin" Content="Admin panel">
            <ChildContent>
                <TabHeader Text="Admin"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Settings content">
            <ChildContent>
                <TabHeader Text="Settings"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private bool IsAdmin = false;
    
    private void ToggleAdminTab()
    {
        IsAdmin = !IsAdmin;
    }
}
```

## Tab Index for Navigation

The `TabIndex` property enables Tab key navigation for tab items. Set to 0 (or positive value) to allow keyboard navigation between tabs.

### Enabling Tab Key Navigation

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        <TabItem TabIndex="0" Content="First tab...">
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem TabIndex="0" Content="Second tab...">
            <ChildContent>
                <TabHeader Text="Tab 2"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem TabIndex="0" Content="Third tab...">
            <ChildContent>
                <TabHeader Text="Tab 3"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```

### TabIndex Behavior
- **TabIndex="0"**: Tab navigates in document order
- **TabIndex="1" or higher**: Tab navigates in TabIndex order (lower values first)
- **Negative TabIndex**: Tab skips the element entirely
- Default behavior: Use arrow keys to navigate; Tab key moves to next focusable element

### Example: Tab Order Control

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        <!-- These will be navigated in reverse order with Tab key -->
        <TabItem TabIndex="3" Content="Navigated last">
            <ChildContent>
                <TabHeader Text="Third"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem TabIndex="2" Content="Navigated second">
            <ChildContent>
                <TabHeader Text="Second"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem TabIndex="1" Content="Navigated first">
            <ChildContent>
                <TabHeader Text="First"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```

## Complete Example: Tab Management Dashboard

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<div>
    <SfButton OnClick="AddTab">Add Tab</SfButton>
    <SfButton OnClick="RemoveTab">Remove Last</SfButton>
    <SfButton OnClick="ToggleSelected">Toggle Selected</SfButton>
    <SfButton OnClick="ToggleVisibility">Toggle Visibility</SfButton>
</div>

<SfTab @ref="TabControl" ShowCloseButton="true" @bind-SelectedItem="SelectedIndex">
    <TabItems>
        @foreach (var item in TabItems)
        {
            <TabItem Disabled="@item.Disabled" Visible="@item.Visible" TabIndex="0">
                <ChildContent>
                    <TabHeader Text="@item.Header"></TabHeader>
                </ChildContent>
                <ContentTemplate>
                    <div>@item.Content</div>
                </ContentTemplate>
            </TabItem>
        }
    </TabItems>
</SfTab>

@code {
    private SfTab TabControl;
    private int SelectedIndex = 0;
    private int Counter = 3;
    
    private List<TabItemData> TabItems = new()
    {
        new TabItemData { Header = "Tab 1", Content = "Content 1", Disabled = false, Visible = true },
        new TabItemData { Header = "Tab 2", Content = "Content 2", Disabled = false, Visible = true },
        new TabItemData { Header = "Tab 3", Content = "Content 3", Disabled = false, Visible = true }
    };
    
    private void AddTab()
    {
        Counter++;
        TabItems.Add(new TabItemData 
        { 
            Header = $"Tab {Counter}", 
            Content = $"Content {Counter}",
            Disabled = false,
            Visible = true
        });
    }
    
    private void RemoveTab()
    {
        if (TabItems.Count > 1)
        {
            TabItems.RemoveAt(TabItems.Count - 1);
        }
    }
    
    private void ToggleSelected()
    {
        if (SelectedIndex < TabItems.Count)
        {
            TabItems[SelectedIndex].Disabled = !TabItems[SelectedIndex].Disabled;
        }
    }
    
    private void ToggleVisibility()
    {
        if (SelectedIndex < TabItems.Count)
        {
            TabItems[SelectedIndex].Visible = !TabItems[SelectedIndex].Visible;
        }
    }
    
    public class TabItemData
    {
        public string Header { get; set; }
        public string Content { get; set; }
        public bool Disabled { get; set; }
        public bool Visible { get; set; }
    }
}
```
