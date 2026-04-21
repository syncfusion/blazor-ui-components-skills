# Advanced Layouts in Blazor ListView

## Table of Contents
- [Grid Layouts](#grid-layouts)
- [Dual-List Patterns](#dual-list-patterns)
- [Mobile-Responsive Layouts](#mobile-responsive-layouts)
- [Chat Window UI](#chat-window-ui)
- [Layout-Specific CSS](#layout-specific-css)

## Grid Layouts

Transform ListView into a multi-column grid:

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView DataSource="@Products" CssClass="e-list-template">
    <ListViewFieldSettings TValue="Product" Id="Id" Text="Name"></ListViewFieldSettings>
    <ListViewTemplates TValue="Product">
        <Template>
            <div class="grid-item">
                <img src="@((context as Product).Image)" alt="Product" />
                <h4>@((context as Product).Name)</h4>
                <p>$@((context as Product).Price)</p>
            </div>
        </Template>
    </ListViewTemplates>
</SfListView>

<style>
    .e-listview {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
        gap: 16px;
        padding: 16px;
    }

    .grid-item {
        border: 1px solid #ddd;
        border-radius: 8px;
        padding: 12px;
        text-align: center;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        cursor: pointer;
        transition: transform 0.2s;
    }

    .grid-item:hover {
        transform: translateY(-4px);
        box-shadow: 0 4px 8px rgba(0,0,0,0.15);
    }

    .grid-item img {
        width: 100%;
        height: 120px;
        object-fit: cover;
        border-radius: 4px;
    }
</style>

@code {
    List<Product> Products = new List<Product>
    {
        new Product { Id = "1", Name = "Product 1", Price = 99.99m, Image = "img1.jpg" },
        new Product { Id = "2", Name = "Product 2", Price = 129.99m, Image = "img2.jpg" },
        new Product { Id = "3", Name = "Product 3", Price = 79.99m, Image = "img3.jpg" }
    };

    public class Product
    {
        public string Id { get; set; }
        public string Name { get; set; }
        public decimal Price { get; set; }
        public string Image { get; set; }
    }
}
```

### Grid Layout with Data Manipulation (CRUD)

Add, remove, sort, and filter items in grid layout:

```cshtml
@using Syncfusion.Blazor.Popups
@using Syncfusion.Blazor.Buttons
@using Syncfusion.Blazor.Lists
@using ListviewSort = Syncfusion.Blazor.Lists.SortOrder

<div id="container">
    <div class="sample flex">
        <div class="flex">
            <!-- Header Controls -->
            <div>
                <div class="headerContainer">
                    <div class="e-input-group">
                        <input id="search" class="e-input" placeholder="Search" @bind-value="@SearchValue" @oninput="@(e => Search(e.Value.ToString()))" />
                    </div>
                    <button id="sort" class="e-btn e-small e-round e-primary e-icon-btn" title="Sort" @onclick="@(e => ListSortOrder = (ListSortOrder == ListviewSort.Ascending) ? ListviewSort.Descending : ListviewSort.Ascending)">
                        <span class="e-btn-icon e-icons @(ListSortOrder == ListviewSort.Ascending ? "e-sort-icon-ascending" : "e-sort-icon-descending")\"></span>
                    </button>
                    <button id="add" class="e-btn e-small e-round e-primary e-icon-btn" title="Add" @onclick="@(e => DialogObj.ShowAsync())">
                        <span class="e-btn-icon e-icons e-add-icon\"></span>
                    </button>
                </div>
            </div>
            
            <!-- Add Item Dialog -->
            <SfDialog @ref="DialogObj" Target="#container" ShowCloseIcon="true" Header="Add item" @bind-Visible="@Visible" Width="300px" Height="230px">
                <DialogTemplates>
                    <Content>
                        <div id="listDialog">
                            <div class="input_name">
                                <label for="name">Item text: </label>
                                <input id="name" class="e-input" type="text" placeholder="Enter text" @bind-value="@Value" />
                            </div>
                        </div>
                    </Content>
                </DialogTemplates>
                <DialogButtons>
                    <DialogButton OnClick="@(e => Add())" Content="Add" IsPrimary="true" CssClass="e-flat\"></DialogButton>
                </DialogButtons>
            </SfDialog>
            
            <!-- Grid ListView -->
            <div class="listview-container">
                <SfListView ID="element" DataSource="@DataSource" SortOrder="@ListSortOrder" CssClass="e-list-template grid-view">
                    <ListViewFieldSettings TValue="ListDataModel" Id="Id" Text="Text\"></ListViewFieldSettings>
                    <ListViewTemplates TValue="ListDataModel">
                        <Template>
                            @{
                                ListDataModel currentData = (ListDataModel)context;
                                <div class="grid-item-box">
                                    <div>@currentData.Text</div>
                                    <button class="delete e-control e-btn e-small e-round e-delete-btn e-primary e-icon-btn" @onclick="@(e => Remove(currentData))">
                                        <span class="e-btn-icon e-icons delete-icon">✕</span>
                                    </button>
                                </div>
                            }
                        </Template>
                    </ListViewTemplates>
                </SfListView>
            </div>
        </div>
    </div>
</div>

@code {
    SfDialog DialogObj;
    bool Visible;
    string Value = "";
    string SearchValue = "";
    ListviewSort ListSortOrder = ListviewSort.Ascending;
    List<ListDataModel> DataSource;
    List<ListDataModel> DataSourceOG = new List<ListDataModel>()
    {
        new ListDataModel { Id = "1", Text = "Item 1" },
        new ListDataModel { Id = "2", Text = "Item 2" },
        new ListDataModel { Id = "3", Text = "Item 3" },
        new ListDataModel { Id = "4", Text = "Item 4" },
        new ListDataModel { Id = "5", Text = "Item 5" }
    };

    protected override void OnInitialized()
    {
        DataSource = new List<ListDataModel>(DataSourceOG);
    }

    async void Add()
    {
        await DialogObj.HideAsync();
        DataSourceOG.Add(new ListDataModel { Id = Guid.NewGuid().ToString(), Text = Value });
        DataSource = new List<ListDataModel>(DataSourceOG);
        Value = "";
    }

    void Remove(ListDataModel data)
    {
        var index = DataSourceOG.FindIndex(e => e.Id == data.Id);
        if (index >= 0)
        {
            DataSourceOG.RemoveAt(index);
            DataSource = new List<ListDataModel>(DataSourceOG);
        }
    }

    void Search(string value)
    {
        SearchValue = value;
        if (string.IsNullOrEmpty(value))
        {
            DataSource = new List<ListDataModel>(DataSourceOG);
        }
        else
        {
            DataSource = DataSourceOG.Where(x => x.Text.ToLower().Contains(value.ToLower())).ToList();
        }
    }

    public class ListDataModel
    {
        public string Id { get; set; }
        public string Text { get; set; }
    }
}

<style>
    #container .e-listview.grid-view {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
        background: none;
        border: none;
        padding: 10px;
    }

    #container .e-listview.grid-view .e-list-item {
        height: auto;
        border: 1px solid #ddd;
        padding: 10px;
        text-align: center;
        border-radius: 4px;
    }

    .grid-item-box {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        gap: 8px;
    }

    .delete-icon {
        cursor: pointer;
        color: red;
    }
</style>
```

## Dual-List Patterns

### Dual List with Data Manipulation

Create two ListViews with buttons to move items between them, with filtering and sorting support:

```cshtml
@using Syncfusion.Blazor.Inputs
@using Syncfusion.Blazor.Lists

<div id="container">
    <div class="sample flex">
        <div class="flex">
            <!-- First List -->
            <div class="padding">
                <SfTextBox Placeholder="Filter" Input="@(e => OnInput(e, 1))"></SfTextBox>
                <SfListView DataSource="@FirstData" SortOrder="SortOrder.Ascending">
                    <ListViewFieldSettings TValue="ListDataModel" Id="Id" Text="Text"></ListViewFieldSettings>
                    <ListViewEvents TValue="ListDataModel" Clicked="@(e => OnSelected(e, 1))"></ListViewEvents>
                </SfListView>
            </div>
            
            <!-- Transfer Buttons -->
            <div class="flex vertical vertical__center flex__center padding">
                <div class="padding">
                    <button disabled="@(!FirstListData.Any())" class="e-btn" @onclick="@(e => OnButtonClick(1))">≫</button>
                </div>
                <div class="padding">
                    <button disabled="@(FirstSelected == null)" class="e-btn" @onclick="@(e => OnButtonClick(2))">></button>
                </div>
                <div class="padding">
                    <button disabled="@(SecondSelected == null)" class="e-btn" @onclick="@(e => OnButtonClick(3))"><</button>
                </div>
                <div class="padding">
                    <button disabled="@(!SecondListData.Any())" class="e-btn" @onclick="@(e => OnButtonClick(4))">«</button>
                </div>
            </div>
            
            <!-- Second List -->
            <div class="padding">
                <SfTextBox Placeholder="Filter" Input="@(e => OnInput(e, 2))"></SfTextBox>
                <SfListView DataSource="@SecondData" SortOrder="SortOrder.Ascending">
                    <ListViewFieldSettings Id="Id" Text="Text" TValue="ListDataModel"></ListViewFieldSettings>
                    <ListViewEvents TValue="ListDataModel" Clicked="@(e => OnSelected(e, 2))"></ListViewEvents>
                </SfListView>
            </div>
        </div>
    </div>
</div>

@code {
    List<ListDataModel> FirstData;
    List<ListDataModel> SecondData;
    ListDataModel FirstSelected;
    ListDataModel SecondSelected;

    List<ListDataModel> FirstListData = new List<ListDataModel>()
    {
        new ListDataModel { Text = "Hennessey Venom", Id = "01" },
        new ListDataModel { Text = "Bugatti Chiron", Id = "02" },
        new ListDataModel { Text = "Bugatti Veyron Super Sport", Id = "03" },
        new ListDataModel { Text = "SSC Ultimate Aero", Id = "04" },
        new ListDataModel { Text = "Koenigsegg CCR", Id = "05" }
    };

    List<ListDataModel> SecondListData = new List<ListDataModel>()
    {
        new ListDataModel { Text = "McLaren P1", Id = "09" },
        new ListDataModel { Text = "Ferrari LaFerrari", Id = "10" }
    };

    protected override void OnInitialized()
    {
        FirstData = new List<ListDataModel>(FirstListData);
        SecondData = new List<ListDataModel>(SecondListData);
    }

    void OnButtonClick(int buttonIndex)
    {
        switch (buttonIndex)
        {
            case 1: // Move All
                FirstListData.ForEach(e => SecondListData.Add(e));
                FirstListData.Clear();
                FirstData = new List<ListDataModel>(FirstListData);
                SecondData = new List<ListDataModel>(SecondListData);
                break;
            case 2: // Move Selected Right
                if (FirstSelected != null)
                {
                    SecondListData.Add(FirstSelected);
                    FirstListData.RemoveAt(FirstListData.FindIndex(e => e.Id == FirstSelected.Id));
                    FirstData = new List<ListDataModel>(FirstListData);
                    SecondData = new List<ListDataModel>(SecondListData);
                    FirstSelected = null;
                }
                break;
            case 3: // Move Selected Left
                if (SecondSelected != null)
                {
                    FirstListData.Add(SecondSelected);
                    SecondListData.RemoveAt(SecondListData.FindIndex(e => e.Id == SecondSelected.Id));
                    FirstData = new List<ListDataModel>(FirstListData);
                    SecondData = new List<ListDataModel>(SecondListData);
                    SecondSelected = null;
                }
                break;
            case 4: // Move All Left
                SecondListData.ForEach(e => FirstListData.Add(e));
                SecondListData.Clear();
                FirstData = new List<ListDataModel>(FirstListData);
                SecondData = new List<ListDataModel>(SecondListData);
                break;
        }
    }

    void OnSelected(ClickEventArgs<ListDataModel> eventArgs, int listviewIndex)
    {
        if (listviewIndex == 1)
        {
            FirstSelected = eventArgs.ItemData;
        }
        else
        {
            SecondSelected = eventArgs.ItemData;
        }
    }

    void OnInput(InputEventArgs eventArgs, int listviewIndex)
    {
        if (listviewIndex == 1)
        {
            FirstData = FirstListData.FindAll(e => e.Text.ToLower().Contains(eventArgs.Value.ToLower()));
        }
        else
        {
            SecondData = SecondListData.FindAll(e => e.Text.ToLower().Contains(eventArgs.Value.ToLower()));
        }
    }

    public class ListDataModel
    {
        public string Id { get; set; }
        public string Text { get; set; }
    }
}

<style>
    .flex {
        display: flex;
    }

    .vertical {
        flex-direction: column;
    }

    .vertical__center {
        justify-content: center;
    }

    .flex__center {
        justify-content: center;
    }

    .padding {
        padding: 4px;
    }

    #container .e-listview {
        width: 300px;
        height: 300px;
    }
</style>
```

## Mobile-Responsive Layouts

### Mobile Contact Layout with Avatar

Create a professional contact list with avatar support (both text initials and images):

```cshtml
@using Syncfusion.Blazor.Lists

<div class="flex flex__center">
    <div class="margin">
        <SfListView DataSource="@DataSource"
                     ShowHeader="true"
                     HeaderTitle="Contacts"
                     CssClass="e-list-template"
                     SortOrder="SortOrder.Ascending">
            <ListViewFieldSettings TValue="ListDataModel" Id="Id" Text="Text"></ListViewFieldSettings>
            <ListViewTemplates TValue="ListDataModel">
                <Template>
                    @{
                        ListDataModel item = context as ListDataModel;
                        <div class="e-list-wrapper e-list-multi-line e-list-avatar">
                            @if (!string.IsNullOrEmpty(item.Avatar))
                            {
                                <span class="e-avatar e-avatar-circle">@item.Avatar</span>
                            }
                            else
                            {
                                <span class="@item.Pic e-avatar e-avatar-circle"></span>
                            }
                            <span class="e-list-item-header">@item.Text</span>
                            <span class="e-list-content">@item.Contact</span>
                        </div>
                    }
                </Template>
            </ListViewTemplates>
        </SfListView>
    </div>
</div>

@code {
    List<ListDataModel> DataSource = new List<ListDataModel>()
    {
        new ListDataModel { Text = "Jennifer", Contact = "(206) 555-985774", Id = "1", Avatar = "JE" },
        new ListDataModel { Text = "Amenda", Contact = "(206) 555-3412", Id = "2", Avatar = "AM" },
        new ListDataModel { Text = "Isabella", Contact = "(206) 555-8122", Id = "4", Avatar = "IS" },
        new ListDataModel { Text = "William", Contact = "(206) 555-9482", Id = "5", Avatar = "WI" },
        new ListDataModel { Text = "Jacob", Contact = "(71) 555-4848", Id = "6", Avatar = "JA" },
        new ListDataModel { Text = "Charlotte", Contact = "(206) 555-1189", Id = "9", Avatar = "CH" }
    };

    public class ListDataModel
    {
        public string Id { get; set; }
        public string Text { get; set; }
        public string Avatar { get; set; }
        public string Pic { get; set; }
        public string Contact { get; set; }
    }
}

<style>
    .flex {
        display: flex;
    }

    .flex__center {
        justify-content: center;
    }

    .margin {
        margin: 10px;
        width: 350px;
    }
</style>
```

### Responsive Contact Layout

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView DataSource="@Contacts"
            CssClass="e-list-template contact-list"
            ShowHeader="true"
            HeaderTitle="Contacts">
    <ListViewFieldSettings TValue="Contact" Id="Id" Text="Name"></ListViewFieldSettings>
    <ListViewTemplates TValue="Contact">
        <Template>
            <div class="e-list-wrapper e-list-multi-line contact-item">
                <img class="contact-avatar" src="@((context as Contact).Avatar)" />
                <div class="contact-info">
                    <span class="e-list-item-header">@((context as Contact).Name)</span>
                    <span class="e-list-content phone">@((context as Contact).Phone)</span>
                    <span class="e-list-content email">@((context as Contact).Email)</span>
                </div>
            </div>
        </Template>
    </ListViewTemplates>
</SfListView>

<style>
    .contact-list {
        margin: 0;
        padding: 0;
    }

    .contact-item {
        padding: 12px 16px;
        border-bottom: 1px solid #f0f0f0;
    }

    .contact-avatar {
        width: 48px;
        height: 48px;
        border-radius: 50%;
        object-fit: cover;
        margin-right: 12px;
    }

    .contact-info {
        display: flex;
        flex-direction: column;
        flex: 1;
    }

    .contact-info .phone {
        font-size: 12px;
        color: #666;
    }

    .contact-info .email {
        font-size: 12px;
        color: #999;
        display: none;
    }

    /* Mobile: Show phone, hide email */
    @media (max-width: 600px) {
        .contact-avatar {
            width: 40px;
            height: 40px;
        }

        .contact-info .phone {
            display: block;
        }

        .contact-info .email {
            display: none;
        }
    }

    /* Desktop: Show both phone and email */
    @media (min-width: 601px) {
        .contact-avatar {
            width: 56px;
            height: 56px;
        }

        .contact-info .email {
            display: block;
        }
    }
</style>

@code {
    List<Contact> Contacts = new List<Contact>
    {
        new Contact { Id = "1", Name = "John Doe", Phone = "(555) 123-4567", Email = "john@example.com", Avatar = "avatar1.jpg" },
        new Contact { Id = "2", Name = "Jane Smith", Phone = "(555) 234-5678", Email = "jane@example.com", Avatar = "avatar2.jpg" }
    };

    public class Contact
    {
        public string Id { get; set; }
        public string Name { get; set; }
        public string Phone { get; set; }
        public string Email { get; set; }
        public string Avatar { get; set; }
    }
}
```

## Chat Window UI

### Message Bubble Layout

```cshtml
@using Syncfusion.Blazor.Lists

<div style="display: flex; flex-direction: column; height: 100vh;">
    <!-- Chat view -->
    <SfListView DataSource="@Messages"
                CssClass="chat-list"
                EnableVirtualization="true"
                Height="400px">
        <ListViewFieldSettings TValue="Message" Id="Id" Text="Text"></ListViewFieldSettings>
        <ListViewTemplates TValue="Message">
            <Template>
                @{
                    var msg = context as Message;
                    <div class="message-container" style="text-align: @(msg.IsOwn ? "right" : "left"); margin: 8px 0;">
                        <div class="message-bubble" style="
                            background-color: @(msg.IsOwn ? "#0084ff" : "#e5e5ea");
                            color: @(msg.IsOwn ? "white" : "black");
                            padding: 8px 12px;
                            border-radius: 16px;
                            display: inline-block;
                            max-width: 70%;
                            word-wrap: break-word;
                        ">
                            @msg.Text
                        </div>
                        <div style="font-size: 12px; color: #999; margin-top: 4px;">
                            @msg.Time
                        </div>
                    </div>
                }
            </Template>
        </ListViewTemplates>
    </SfListView>

    <!-- Input -->
    <div style="padding: 12px; border-top: 1px solid #ddd;">
        <div style="display: flex; gap: 8px;">
            <input type="text" placeholder="Type a message..." style="flex: 1; padding: 8px;" @bind="newMessage" />
            <button class="e-btn" @onclick="SendMessage">Send</button>
        </div>
    </div>
</div>

@code {
    List<Message> Messages = new List<Message>();
    string newMessage = "";

    protected override void OnInitialized()
    {
        Messages.Add(new Message { Id = "1", Text = "Hello!", IsOwn = true, Time = "10:30 AM" });
        Messages.Add(new Message { Id = "2", Text = "Hi there!", IsOwn = false, Time = "10:31 AM" });
    }

    void SendMessage()
    {
        if (!string.IsNullOrEmpty(newMessage))
        {
            Messages.Add(new Message
            {
                Id = (Messages.Count + 1).ToString(),
                Text = newMessage,
                IsOwn = true,
                Time = DateTime.Now.ToString("h:mm tt")
            });
            newMessage = "";
        }
    }

    public class Message
    {
        public string Id { get; set; }
        public string Text { get; set; }
        public bool IsOwn { get; set; }
        public string Time { get; set; }
    }
}
```

## Layout-Specific CSS

### Common Layout Classes

```css
/* Flex layout for horizontal arrangement */
.layout-flex {
    display: flex;
    gap: 16px;
}

/* Grid layout for multiple items per row */
.layout-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 16px;
}

/* Two-column layout */
.layout-two-column {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
}

/* Sidebar layout */
.layout-sidebar {
    display: flex;
}

.layout-sidebar-nav {
    width: 250px;
    border-right: 1px solid #ddd;
    overflow-y: auto;
}

.layout-sidebar-content {
    flex: 1;
    overflow-y: auto;
}

/* Responsive adjustments */
@media (max-width: 768px) {
    .layout-grid {
        grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
    }

    .layout-two-column {
        grid-template-columns: 1fr;
    }

    .layout-sidebar {
        flex-direction: column;
    }

    .layout-sidebar-nav {
        width: 100%;
        border-right: none;
        border-bottom: 1px solid #ddd;
    }
}
```

---

**Related Topics:**
- [Templating and Rendering](templating-and-rendering.md) - Create layout templates
- [Item Selection and Actions](item-selection-and-actions.md) - Interaction in layouts
- [Virtualization and Performance](virtualization-and-performance.md) - Performance for large layouts

