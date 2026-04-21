# Templating and Rendering in Blazor ListView

## Table of Contents
- [Item Templates](#item-templates)
- [Header Template](#header-template)
- [Group Header Templates](#group-header-templates)
- [Template CSS Classes](#template-css-classes)
- [Device-Specific Templates](#device-specific-templates)
- [Multi-Line and Avatar Templates](#multi-line-and-avatar-templates)

## Item Templates

Customize how each list item renders with the `Template` property:

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView DataSource="@ListData" CssClass="e-list-template">
    <ListViewFieldSettings TValue="DataModel" Id="Id" Text="Text"></ListViewFieldSettings>
    <ListViewTemplates TValue="DataModel">
        <Template>
            <div class="e-list-wrapper">
                <span class="e-list-content">@((context as DataModel).Text)</span>
            </div>
        </Template>
    </ListViewTemplates>
</SfListView>

@code {
    List<DataModel> ListData = new List<DataModel>
    {
        new DataModel { Text = "Hennessey Venom", Id = "1" },
        new DataModel { Text = "Bugatti Chiron", Id = "2" },
        new DataModel { Text = "Ferrari LaFerrari", Id = "3" }
    };

    public class DataModel
    {
        public string Text { get; set; }
        public string Id { get; set; }
    }
}
```

## Header Template

```cshtml
<SfListView DataSource="@FruitsData" ShowHeader="true">
    <ListViewFieldSettings TValue="DataModel" Id="Id" Text="Text"></ListViewFieldSettings>
    <ListViewTemplates TValue="DataModel">
        <HeaderTemplate>
            <div class="headerContainer" style="display: flex; align-items: center;">
                <span class="fruitHeader" style="font-weight: bold; font-size: 18px;">Fruits</span>
            </div>
        </HeaderTemplate>
    </ListViewTemplates>
</SfListView>

@code {
    List<DataModel> FruitsData = new List<DataModel>
    {
        new DataModel { Text = "Date", Id = "1" },
        new DataModel { Text = "Fig", Id = "2" },
        new DataModel { Text = "Apple", Id = "3" },
        new DataModel { Text = "Apricot", Id = "4" },
        new DataModel { Text = "Grape", Id = "5" }
    };

    public class DataModel
    {
        public string Text { get; set; }
        public string Id { get; set; }
    }
}
```

## Group Header Templates

Customize grouped list headers:

```cshtml
@using Syncfusion.Blazor.Lists

<SfListView DataSource="@GroupedData" CssClass="e-list-template">
    <ListViewFieldSettings TValue="DataModel" 
                          Id="Id" 
                          Text="Text" 
                          GroupBy="Category">
    </ListViewFieldSettings>
    <ListViewTemplates TValue="DataModel">
        <GroupTemplate>
            <div style="padding: 8px; background-color: #e0e0e0; font-weight: bold;">
                Group: @context.Text
            </div>
        </GroupTemplate>
    </ListViewTemplates>
</SfListView>

@code {
    List<DataModel> GroupedData = new List<DataModel>
    {
        new DataModel { Text = "Item 1", Id = "1", Category = "Category A" },
        new DataModel { Text = "Item 2", Id = "2", Category = "Category A" },
        new DataModel { Text = "Item 3", Id = "3", Category = "Category B" },
        new DataModel { Text = "Item 4", Id = "4", Category = "Category B" }
    };

    public class DataModel
    {
        public string Id { get; set; }
        public string Text { get; set; }
        public string Category { get; set; }
    }
}
```

## Template CSS Classes

Used to differentiate and style template rendering:

| CSS Class | Purpose |
|-----------|---------|
| `e-list-template` | Root class for template-enabled ListView |
| `e-list-wrapper` | Template element wrapper (required) |
| `e-list-content` | Content alignment and styling |
| `e-list-avatar` | Avatar/image on left side |
| `e-list-avatar-right` | Avatar/image on right side |
| `e-list-badge` | Badge styling (notifications, status) |
| `e-list-multi-line` | Multi-line item layout |
| `e-list-item-header` | Header text in multi-line items |

### Example: Avatar Template

```cshtml
<SfListView DataSource="@Contacts" CssClass="e-list-template">
    <ListViewFieldSettings TValue="Contact" Id="Id" Text="Name"></ListViewFieldSettings>
    <ListViewTemplates TValue="Contact">
        <Template>
            <div class="e-list-wrapper e-list-multi-line e-list-avatar">
                <span class="e-avatar e-avatar-circle">@((context as Contact).Avatar)</span>
                <span class="e-list-item-header">@((context as Contact).Name)</span>
                <span class="e-list-content">@((context as Contact).Email)</span>
            </div>
        </Template>
    </ListViewTemplates>
</SfListView>

@code {
    List<Contact> Contacts = new List<Contact>
    {
        new Contact { Id = "1", Name = "John Doe", Email = "john@example.com", Avatar = "JD" },
        new Contact { Id = "2", Name = "Jane Smith", Email = "jane@example.com", Avatar = "JS" }
    };

    public class Contact
    {
        public string Id { get; set; }
        public string Name { get; set; }
        public string Email { get; set; }
        public string Avatar { get; set; }
    }
}
```

### Example: Badge Template

```cshtml
<div class="e-list-wrapper e-list-badge">
    <span class="e-list-content">Notification</span>
    <span class="e-badge e-badge-danger">5</span>
</div>
```

## Device-Specific Templates

Render different templates for mobile and desktop:

```cshtml
@using Syncfusion.Blazor.Lists
@using Microsoft.AspNetCore.Http
@inject IHttpContextAccessor HttpContextAccessor

<div class="flex flex__center">
    <SfListView DataSource="@BlogPosts" 
                ShowHeader="true" 
                HeaderTitle="Blog"
                CssClass="e-list-template">
        <ListViewFieldSettings TValue="BlogPost" Id="Id" Text="Title"></ListViewFieldSettings>
        <ListViewTemplates TValue="BlogPost">
            <Template>
                @{
                    BlogPost item = context as BlogPost;
                    <div class="e-list-wrapper e-list-multi-line">
                        <img class="e-avatar" src="@item.Image" alt="Blog" />
                        <div class="flex vertical">
                            <span class="e-list-item-header">@item.Title</span>
                            <span class="e-list-content">@item.Content</span>
                            @if (!IsMobile)
                            {
                                <div>
                                    <span class="timestamp">@item.Date.ToShortDateString()</span>
                                    <span class="comments">@item.CommentsCount comments</span>
                                </div>
                            }
                        </div>
                    </div>
                }
            </Template>
        </ListViewTemplates>
    </SfListView>
</div>

@code {
    bool IsMobile;
    
    List<BlogPost> BlogPosts = new List<BlogPost>
    {
        new BlogPost 
        { 
            Id = "1", 
            Title = "Blazor Tips", 
            Content = "Learn Blazor best practices...",
            Image = "blog1.jpg",
            Date = DateTime.Now,
            CommentsCount = 5
        }
    };

    protected override void OnInitialized()
    {
        var userAgent = HttpContextAccessor.HttpContext.Request.Headers["User-Agent"];
        IsMobile = (userAgent[0] as string)?.ToLower().Contains("mobile") ?? false;
    }

    public class BlogPost
    {
        public string Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        public string Image { get; set; }
        public DateTime Date { get; set; }
        public int CommentsCount { get; set; }
    }
}
```

## Multi-Line and Avatar Templates

### Contact List Example

```cshtml
<SfListView Id="List"
            DataSource="@ContactList"
            HeaderTitle="Contacts"
            ShowHeader="true"
            CssClass="e-list-template"
            Width="350">
    <ListViewFieldSettings TValue="Contact" Id="Id" Text="Name"></ListViewFieldSettings>
    <ListViewTemplates TValue="Contact">
        <Template>
            <div class="e-list-wrapper e-list-multi-line e-list-avatar">
                @if (!string.IsNullOrEmpty((context as Contact).AvatarUrl))
                {
                    <img class="e-avatar e-avatar-circle" src="@((context as Contact).AvatarUrl)" />
                }
                else
                {
                    <span class="e-avatar e-avatar-circle">@((context as Contact).InitialCode)</span>
                }
                <span class="e-list-item-header">@((context as Contact).Name)</span>
                <span class="e-list-content">@((context as Contact).PhoneNumber)</span>
            </div>
        </Template>
    </ListViewTemplates>
</SfListView>

@code {
    List<Contact> ContactList = new List<Contact>
    {
        new Contact { 
            Id = "1", 
            Name = "Jennifer", 
            PhoneNumber = "(206) 555-985774",
            AvatarUrl = "",
            InitialCode = "J"
        },
        new Contact { 
            Id = "2", 
            Name = "Amenda", 
            PhoneNumber = "(206) 555-3412",
            AvatarUrl = "url",
            InitialCode = "A"
        }
    };

    public class Contact
    {
        public string Id { get; set; }
        public string Name { get; set; }
        public string PhoneNumber { get; set; }
        public string AvatarUrl { get; set; }
        public string InitialCode { get; set; }
    }
}
```

---

**Related Topics:**
- [Item Selection and Actions](item-selection-and-actions.md) - Add selection to templates
- [Advanced Layouts](advanced-layouts.md) - Complex template layouts
- [Styling and Customization](styling-and-customization.md) - Template styling

