# Events and Navigation in Blazor ListView

## Table of Contents
- [Event Handling](#event-handling)
- [ListViewItem Events](#listviewitem-events)
- [Navigation Patterns](#navigation-patterns)
- [Hyperlink Integration](#hyperlink-integration)
- [Complete Event Example](#complete-event-example)

## Event Handling

### Available Events

The ListView provides lifecycle and interaction events:

```cshtml
<ListViewEvents TValue="Item"
                OnActionBegin="@OnActionBegin"
                OnActionComplete="@OnActionComplete"
                Clicked="@OnClicked">
</ListViewEvents>

@code {
    void OnActionBegin(ActionBeginEventArgs<Item> args)
    {
        // Triggered when action starts
        // args.EventType, args.Data available
    }

    void OnActionComplete(ActionCompleteEventArgs<Item> args)
    {
        // Triggered when action completes
        // Data binding complete event
    }

    void OnClicked(ClickEventArgs<Item> args)
    {
        // Triggered when item is clicked
        // args.Text, args.Data available
    }
}
```

### Event Flow

```
User Action → OnActionBegin → Processing → OnActionComplete
                                ↓
                          Event Completion
```

## ListViewItem Events

### Click Event

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView DataSource="@Items">
    <ListViewFieldSettings TValue="Item" Id="Id" Text="Text"></ListViewFieldSettings>
    <ListViewEvents TValue="Item"
                    Clicked="@(e => OnItemClicked(e))">
    </ListViewEvents>
</SfListView>

<p>Clicked Item: @SelectedItemText</p>

@code {
    string SelectedItemText = "";

    void OnItemClicked(ClickEventArgs<Item> args)
    {
        SelectedItemText = args.Text;
        Console.WriteLine($"Item clicked: {args.Text}");
        // Access item data
        var itemData = args.Data;
    }

    List<Item> Items = new List<Item>
    {
        new Item { Id = "1", Text = "Option 1" },
        new Item { Id = "2", Text = "Option 2" }
    };

    public class Item
    {
        public string Id { get; set; }
        public string Text { get; set; }
    }
}
```

### ActionBegin Event - Data Binding

```csharp
void OnActionBegin(ActionBeginEventArgs<Item> args)
{
    // Triggered when ListView starts binding data
    if (args.EventType == "initially")
    {
        Console.WriteLine("ListView initializing...");
    }
}
```

### ActionComplete Event - Data Binding Complete

```csharp
void OnActionComplete(ActionCompleteEventArgs<Item> args)
{
    // Triggered when data binding completes
    Console.WriteLine($"Data bound successfully. Total items: {args.Result?.Count}");
}
```

## Navigation Patterns

### Navigate on Item Click

```cshtml
@using Syncfusion.Blazor.Lists
@inject NavigationManager NavigationManager

<SfListView DataSource="@Pages">
    <ListViewFieldSettings TValue="Page" Id="Id" Text="Name"></ListViewFieldSettings>
    <ListViewEvents TValue="Page"
                    Clicked="@(e => Navigate(e.Data.Url))">
    </ListViewEvents>
</SfListView>

@code {
    void Navigate(string url)
    {
        NavigationManager.NavigateTo(url);
    }

    List<Page> Pages = new List<Page>
    {
        new Page { Id = "1", Name = "Home", Url = "/" },
        new Page { Id = "2", Name = "About", Url = "/about" },
        new Page { Id = "3", Name = "Contact", Url = "/contact" }
    };

    public class Page
    {
        public string Id { get; set; }
        public string Name { get; set; }
        public string Url { get; set; }
    }
}
```

### Detail View Navigation

```csharp
void OnItemClicked(ClickEventArgs<Product> args)
{
    var productId = args.Data.Id;
    NavigationManager.NavigateTo($"/product-details/{productId}");
}
```

## Hyperlink Integration

### Using Anchor Tags in Templates

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView DataSource="@DataSource">
    <ListViewFieldSettings TValue="ListDataModel" Id="Id" Text="Name"></ListViewFieldSettings>
    <ListViewTemplates TValue="ListDataModel">
        <Template>
            <a target="_blank" href="@((context as ListDataModel).Url)">
                @((context as ListDataModel).Name)
            </a>
        </Template>
    </ListViewTemplates>
</SfListView>

@code {
    List<ListDataModel> DataSource = new List<ListDataModel>
    {
        new ListDataModel { Id = "1", Name = "Google", Url = "url" },
        new ListDataModel { Id = "2", Name = "Bing", Url = "url" },
        new ListDataModel { Id = "3", Name = "Yahoo", Url = "url" },
        new ListDataModel { Id = "4", Name = "Ask.com", Url = "url" }
    };

    public class ListDataModel
    {
        public string Id { get; set; }
        public string Name { get; set; }
        public string Url { get; set; }
    }
}
```

### Blazor Navigation Links

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView DataSource="@Users" CssClass="e-list-template">
    <ListViewFieldSettings TValue="User" Id="Id" Text="Name"></ListViewFieldSettings>
    <ListViewTemplates TValue="User">
        <Template>
            <NavLink href="@($"/user/{((context as User).Id}")">
                <div class="e-list-wrapper">
                    <span class="e-list-content">@((context as User).Name)</span>
                </div>
            </NavLink>
        </Template>
    </ListViewTemplates>
</SfListView>

@code {
    List<User> Users = new List<User>
    {
        new User { Id = "1", Name = "User A" },
        new User { Id = "2", Name = "User B" }
    };

    public class User
    {
        public string Id { get; set; }
        public string Name { get; set; }
    }
}
```

### Combined Navigation and Event Handling

```csharp
void OnItemClicked(ClickEventArgs<MenuItem> args)
{
    // Custom logic
    LogUserAction(args.Data.Name);
    
    // Navigate
    if (!string.IsNullOrEmpty(args.Data.NavigateUrl))
    {
        NavigationManager.NavigateTo(args.Data.NavigateUrl);
    }
}

void LogUserAction(string itemName)
{
    Console.WriteLine($"User selected: {itemName}");
    // Send to analytics, etc.
}
```

## Complete Event Example

```cshtml
@using Syncfusion.Blazor.Lists
@inject NavigationManager NavigationManager

<div>
    <SfListView @ref="ListView"
                DataSource="@Items"
                ShowHeader="true"
                HeaderTitle="Event Demo">
        <ListViewFieldSettings TValue="MenuItem" Id="Id" Text="Name"></ListViewFieldSettings>
        <ListViewTemplates TValue="MenuItem">
            <Template>
                <div class="e-list-wrapper" @onclick="@((e) => OnItemClicked((context as MenuItem)))">
                    <span class="e-list-content">@((context as MenuItem).Name)</span>
                </div>
            </Template>
        </ListViewTemplates>
        <ListViewEvents TValue="MenuItem"
                        OnActionBegin="@OnActionBegin"
                        OnActionComplete="@OnActionComplete"
                        Clicked="@OnEventClicked">
        </ListViewEvents>
    </SfListView>

    <!-- Event Log -->
    <div style="margin-top: 20px; padding: 12px; background-color: #f5f5f5; border: 1px solid #ddd;">
        <h4>Event Log:</h4>
        <ul style="list-style: none; padding: 0;">
            @foreach (var log in EventLog)
            {
                <li style="padding: 4px 0; font-size: 12px;">
                    <span style="color: #999;">[@log.Timestamp]</span> @log.Message
                </li>
            }
        </ul>
    </div>
</div>

@code {
    SfListView<MenuItem> ListView;
    List<MenuItem> Items = new List<MenuItem>();
    List<EventLogEntry> EventLog = new List<EventLogEntry>();
    private int maxLogs = 10;

    protected override void OnInitialized()
    {
        Items.Add(new MenuItem { Id = "1", Name = "Home", NavigateUrl = "/" });
        Items.Add(new MenuItem { Id = "2", Name = "Products", NavigateUrl = "/products" });
        Items.Add(new MenuItem { Id = "3", Name = "About", NavigateUrl = "/about" });
        Items.Add(new MenuItem { Id = "4", Name = "Contact", NavigateUrl = "/contact" });
    }

    void OnActionBegin(ActionBeginEventArgs<MenuItem> args)
    {
        LogEvent("OnActionBegin triggered");
    }

    void OnActionComplete(ActionCompleteEventArgs<MenuItem> args)
    {
        LogEvent($"OnActionComplete - Items bound: {Items.Count}");
    }

    void OnEventClicked(ClickEventArgs<MenuItem> args)
    {
        LogEvent($"Item clicked: {args.Text}");
    }

    void OnItemClicked(MenuItem item)
    {
        LogEvent($"Custom click: {item.Name}");
        if (!string.IsNullOrEmpty(item.NavigateUrl))
        {
            NavigationManager.NavigateTo(item.NavigateUrl);
        }
    }

    void LogEvent(string message)
    {
        EventLog.Insert(0, new EventLogEntry
        {
            Timestamp = DateTime.Now.ToString("HH:mm:ss"),
            Message = message
        });

        if (EventLog.Count > maxLogs)
        {
            EventLog.RemoveAt(EventLog.Count - 1);
        }
    }

    public class MenuItem
    {
        public string Id { get; set; }
        public string Name { get; set; }
        public string NavigateUrl { get; set; }
    }

    public class EventLogEntry
    {
        public string Timestamp { get; set; }
        public string Message { get; set; }
    }
}
```

---

**Related Topics:**
- [Item Selection and Actions](item-selection-and-actions.md) - Handle selections
- [Advanced Patterns](advanced-patterns.md) - Complex event scenarios
- [Templating and Rendering](templating-and-rendering.md) - Event-aware templates

