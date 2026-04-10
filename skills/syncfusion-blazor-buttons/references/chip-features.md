# Chip Component - Features

## Table of Contents
- [Getting Started](#getting-started)
- [Installation](#installation)
- [Basic Chip Implementation](#basic-chip-implementation)
- [Chip Types](#chip-types)
- [Icon Integration](#avatar-and-icon-integration)
- [Delete Functionality](#delete-functionality)
- [Selection Handling](#selection-handling)
- [Events](#events)
- [Dynamic Chip Collections](#dynamic-chip-collections)
- [Data Binding](#data-binding)

---

## Getting Started

The Blazor Chip component displays compact elements representing attributes, contacts, tags, or actions. It supports multiple types (input, filter, choice, action),icons, and deletion.

### Prerequisites
- .NET SDK 6.0 or later
- Blazor WebAssembly or Blazor Server application

---

## Installation

### Install Packages

```bash
dotnet add package Syncfusion.Blazor.Buttons
dotnet add package Syncfusion.Blazor.Themes
```

### Setup

In `~/_Imports.razor`:
```csharp
@using Syncfusion.Blazor.Buttons
```

In `~/Program.cs`:
```csharp
builder.Services.AddSyncfusionBlazor();
```

In `~/index.html`:
```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

---

## Basic Chip Implementation

```razor
<SfChip>
    <ChipItems>
        <ChipItem Text="Janet Leverling"></ChipItem>
        <ChipItem Text="Margaret Hamilt"></ChipItem>
        <ChipItem Text="Andrew Fuller"></ChipItem>
    </ChipItems>
</SfChip>
```

### Chip with Delete

```razor
<SfChip EnableDelete="true">
    <ChipItems>
        <ChipItem Text="Chip 1"></ChipItem>
        <ChipItem Text="Chip 2"></ChipItem>
        <ChipItem Text="Chip 3"></ChipItem>
    </ChipItems>
</SfChip>
```

---

## Chip Types

### Input Chip

```razor
<SfChip Selection="SelectionType.Single" EnableDelete="true">
    <ChipItems>
        <ChipItem Text="Extra Small"></ChipItem>
        <ChipItem Text="Small"></ChipItem>
        <ChipItem Text="Medium"></ChipItem>
        <ChipItem Text="Large"></ChipItem>
    </ChipItems>
</SfChip>
```

### Filter Chip

```razor
<SfChip Selection="SelectionType.Multiple">
    <ChipItems>
        <ChipItem Text="Extra Small" Enabled="true"></ChipItem>
        <ChipItem Text="Small" Enabled="true"></ChipItem>
        <ChipItem Text="Medium" Enabled="true"></ChipItem>
    </ChipItems>
</SfChip>
```

### Choice Chip

```razor
<SfChip Selection="SelectionType.Single" CssClass="e-outline">
    <ChipItems>
        <ChipItem Text="Small"></ChipItem>
        <ChipItem Text="Medium"></ChipItem>
        <ChipItem Text="Large"></ChipItem>
        <ChipItem Text="Extra Large"></ChipItem>
    </ChipItems>
</SfChip>
```

### Action Chip

```razor
<SfChip>
    <ChipItems>
        <ChipItem Text="Send Message" LeadingIconCss="e-ddb-icons e-message"></ChipItem>
        <ChipItem Text="Make Call" LeadingIconCss="e-ddb-icons e-phone"></ChipItem>
    </ChipItems>
</SfChip>

<style>
    .e-message::before { content: '\e751'; }
    .e-phone::before { content: '\e75f'; }
</style>
```

---

## Icon Integration

### ChipItem Properties

**Text Properties:**
- `Text` (string) - Main chip text
- `LeadingText` (string) - Leading avatar text/initials
- `Value` (string) - Chip value for identification

**Icon Properties:**
- `LeadingIconCss` (string) - CSS class for leading icon
- `LeadingIconUrl` (string) - URL for leading icon image
- `TrailingIconCss` (string) - CSS class for trailing icon (e.g., delete icon)
- `TrailingIconUrl` (string) - URL for trailing icon image

**Appearance Properties:**
- `CssClass` (string) - Custom CSS classes
- `Enabled` (bool) - Enable/disable individual chip
- `HtmlAttributes` (Dictionary<string, object>) - Custom HTML attributes
- `Template` (RenderFragment) - Custom chip template

### With Avatar (Leading Text)

```razor
<SfChip>
    <ChipItems>
        <ChipItem Text="Janet" LeadingText="JL"></ChipItem>
        <ChipItem Text="Margaret" LeadingText="MH"></ChipItem>
        <ChipItem Text="Andrew" LeadingText="AF"></ChipItem>
    </ChipItems>
</SfChip>
```

### With Icon

```razor
<SfChip>
    <ChipItems>
        <ChipItem Text="Home" LeadingIconCss="e-icons e-home"></ChipItem>
        <ChipItem Text="Settings" LeadingIconCss="e-icons e-settings"></ChipItem>
        <ChipItem Text="Profile" LeadingIconCss="e-icons e-user"></ChipItem>
    </ChipItems>
</SfChip>

<style>
    .e-home::before { content: '\e7a2'; }
    .e-settings::before { content: '\e7a7'; }
    .e-user::before { content: '\e7ee'; }
</style>
```

### With Avatar and Delete

```razor
<SfChip EnableDelete="true">
    <ChipItems>
        <ChipItem Text="Andrew" LeadingText="A"></ChipItem>
        <ChipItem Text="Janet" LeadingText="J"></ChipItem>
        <ChipItem Text="Laura" LeadingText="L"></ChipItem>
        <ChipItem Text="Margaret" LeadingText="M"></ChipItem>
    </ChipItems>
</SfChip>
```

---

## Delete Functionality

### Enable Delete

```razor
<SfChip EnableDelete="true">
    <ChipItems>
        <ChipItem Text="Deletable 1"></ChipItem>
        <ChipItem Text="Deletable 2"></ChipItem>
    </ChipItems>
</SfChip>
```

### Delete Event Handling

```razor
<SfChip EnableDelete="true">
    <ChipEvents Deleted="OnChipDeleted"></ChipEvents>
    <ChipItems>
        <ChipItem Text="Chip 1" Value="1"></ChipItem>
        <ChipItem Text="Chip 2" Value="2"></ChipItem>
        <ChipItem Text="Chip 3" Value="3"></ChipItem>
    </ChipItems>
</SfChip>

@code {
    private void OnChipDeleted(ChipDeletedEventArgs args)
    {
        Console.WriteLine($"Deleted: {args.Text}");
    }
}
```

### Conditional Delete

```razor
<SfChip EnableDelete="true">
    <ChipEvents OnDelete="OnBeforeDelete"></ChipEvents>
    <ChipItems>
        <ChipItem Text="Undeletable" Value="1"></ChipItem>
        <ChipItem Text="Deletable" Value="2"></ChipItem>
    </ChipItems>
</SfChip>

@code {
    private void OnBeforeDelete(ChipEventArgs args)
    {
        if (args.Value == "1")
        {
            args.Cancel = true; // Prevent deletion
        }
    }
}
```

---

## Selection Handling

### Single Selection

```razor
<SfChip Selection="SelectionType.Single">
    <ChipEvents OnClick="OnChipClick"></ChipEvents>
    <ChipItems>
        <ChipItem Text="Option 1"></ChipItem>
        <ChipItem Text="Option 2"></ChipItem>
        <ChipItem Text="Option 3"></ChipItem>
    </ChipItems>
</SfChip>

<p>Selected: @selectedChip</p>

@code {
    private string selectedChip = "";

    private void OnChipClick(ChipEventArgs args)
    {
        selectedChip = args.Text;
    }
}
```

### Multiple Selection

```razor
<SfChip Selection="SelectionType.Multiple">
    <ChipEvents OnClick="OnChipClick"></ChipEvents>
    <ChipItems>
        <ChipItem Text="Bold"></ChipItem>
        <ChipItem Text="Italic"></ChipItem>
        <ChipItem Text="Underline"></ChipItem>
    </ChipItems>
</SfChip>

<p>Selected: @string.Join(", ", selectedChips)</p>

@code {
    private List<string> selectedChips = new();

    private void OnChipClick(ChipEventArgs args)
    {
        if (args.Selected)
            selectedChips.Add(args.Text);
        else
            selectedChips.Remove(args.Text);
    }
}
```

---

## Events

### Available Events

**ChipEvents Properties:**
- `OnClick` (EventCallback<ChipEventArgs>) - Triggered when chip is clicked
- `OnBeforeClick` (EventCallback<ChipEventArgs>) - Triggered before chip click (cancelable)
- `OnDelete` (EventCallback<ChipEventArgs>) - Triggered before chip deletion (cancelable)
- `Deleted` (EventCallback<ChipDeletedEventArgs>) - Triggered after chip is deleted
- `SelectionChanged` (EventCallback<SelectionChangedEventArgs>) - Triggered when selection changes
- `Created` (EventCallback<Object>) - Triggered when component is created
- `Destroyed` (EventCallback<Object>) - Triggered when component is destroyed

**ChipEventArgs Properties:**
- `Text` (string) - Chip text
- `Value` (string) - Chip value
- `Index` (int) - Chip index
- `Selected` (bool) - Selection state
- `Cancel` (bool) - Cancel the event (for cancelable events)
- `Element` (ElementReference) - Chip element reference

**ChipDeletedEventArgs Properties:**
- `Text` (string) - Deleted chip text (read-only)
- `Value` (string) - Deleted chip value (read-only)
- `Index` (int) - Deleted chip index (read-only)

### Click Event

```razor
<SfChip>
    <ChipEvents OnClick="OnClick"></ChipEvents>
    <ChipItems>
        <ChipItem Text="Clickable Chip"></ChipItem>
    </ChipItems>
</SfChip>

@code {
    private void OnClick(ChipEventArgs args)
    {
        Console.WriteLine($"Clicked: {args.Text}");
    }
}
```

### Delete Event

```razor
<SfChip EnableDelete="true">
    <ChipEvents OnDelete="OnBeforeDelete"
                Deleted="OnAfterDelete">
    </ChipEvents>
    <ChipItems>
        <ChipItem Text="Chip 1"></ChipItem>
        <ChipItem Text="Chip 2"></ChipItem>
    </ChipItems>
</SfChip>

@code {
    private void OnBeforeDelete(ChipEventArgs args)
    {
        Console.WriteLine($"Before delete: {args.Text}");
        // Can cancel with args.Cancel = true
    }

    private void OnAfterDelete(ChipDeletedEventArgs args)
    {
        Console.WriteLine($"After delete: {args.Text}");
    }
}
```

### Created Event

```razor
<SfChip>
    <ChipEvents Created="OnCreated"></ChipEvents>
    <ChipItems>
        <ChipItem Text="Chip 1"></ChipItem>
    </ChipItems>
</SfChip>

@code {
    private void OnCreated(Object args)
    {
        Console.WriteLine("Chip component created");
    }
}
```

---

## Dynamic Chip Collections

### Render from List

```razor
<SfChip EnableDelete="true">
    <ChipItems>
        @foreach (var tag in tags)
        {
            <ChipItem Text="@tag.Name" Value="@tag.Id"></ChipItem>
        }
    </ChipItems>
</SfChip>

@code {
    private List<Tag> tags = new()
    {
        new Tag { Id = 1, Name = "C#" },
        new Tag { Id = 2, Name = "Blazor" },
        new Tag { Id = 3, Name = "ASP.NET" }
    };

    public class Tag
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }
}
```

### Add/Remove Chips Dynamically

```razor
<SfChip EnableDelete="true">
    <ChipEvents TValue="ChipItem" Deleted="OnChipDeleted"></ChipEvents>
    <ChipItems>
        @foreach (var chip in chipList)
        {
            <ChipItem Text="@chip"></ChipItem>
        }
    </ChipItems>
</SfChip>

<input @bind="newChipText" placeholder="Enter chip text" />
<button @onclick="AddChip">Add Chip</button>

@code {
    private List<string> chipList = new() { "Chip 1", "Chip 2", "Chip 3" };
    private string newChipText = "";

    private void AddChip()
    {
        if (!string.IsNullOrWhiteSpace(newChipText))
        {
            chipList.Add(newChipText);
            newChipText = "";
        }
    }

    private void OnChipDeleted(DeleteEventArgs<ChipItem> args)
    {
        chipList.Remove(args.Text);
    }
}
```

---

## Data Binding

### Bind to Object Collection

```razor
<SfChip EnableDelete="true" Selection="SelectionType.Multiple">
    <ChipEvents OnClick="OnSelection"
                Deleted="OnDeleted">
    </ChipEvents>
    <ChipItems>
        @foreach (var item in items)
        {
            <ChipItem Text="@item.Text" 
                      Value="@item.Id.ToString()"
                      LeadingText="@item.Avatar"
                      Enabled="@item.IsActive">
            </ChipItem>
        }
    </ChipItems>
</SfChip>

@code {
    private List<ChipModel> items = new()
    {
        new ChipModel { Id = 1, Text = "John", Avatar = "J", IsActive = true },
        new ChipModel { Id = 2, Text = "Jane", Avatar = "J", IsActive = true },
        new ChipModel { Id = 3, Text = "Bob", Avatar = "B", IsActive = false }
    };

    private void OnSelection(ChipEventArgs args)
    {
        if (int.TryParse(args.Value, out int id))
        {
            var item = items.FirstOrDefault(i => i.Id == id);
            if (item != null)
                item.IsActive = args.Selected;
        }
    }

    private void OnDeleted(ChipDeletedEventArgs args)
    {
        if (int.TryParse(args.Value, out int id))
        {
            var item = items.FirstOrDefault(i => i.Id == id);
            if (item != null)
                items.Remove(item);
        }
    }

    public class ChipModel
    {
        public int Id { get; set; }
        public string Text { get; set; }
        public string Avatar { get; set; }
        public bool IsActive { get; set; }
    }
}
```

---

## Common Scenarios

### Tag Input

```razor
<div class="tag-input">
    <SfChip EnableDelete="true">
        <ChipEvents TValue="ChipItem" Deleted="RemoveTag"></ChipEvents>
        <ChipItems>
            @foreach (var tag in tags)
            {
                <ChipItem Text="@tag"></ChipItem>
            }
        </ChipItems>
    </SfChip>
    <input @bind="newTag" @onkeydown="OnKeyDown" placeholder="Add tag..." />
</div>

@code {
    private List<string> tags = new() { "blazor", "csharp", "dotnet" };
    private string newTag = "";

    private void OnKeyDown(KeyboardEventArgs e)
    {
        if (e.Key == "Enter" && !string.IsNullOrWhiteSpace(newTag))
        {
            tags.Add(newTag.Trim());
            newTag = "";
        }
    }

    private void RemoveTag(ChipDeletedEventArgs args)
    {
        tags.Remove(args.Text);
    }
}
```

### Contact List

```razor
<SfChip EnableDelete="true">
    <ChipItems>
        @foreach (var contact in contacts)
        {
            <ChipItem Text="@contact.Name" 
                      LeadingText="@contact.Initials"
                      Value="@contact.Id.ToString()">
            </ChipItem>
        }
    </ChipItems>
</SfChip>

@code {
    private List<Contact> contacts = new()
    {
        new Contact { Id = 1, Name = "Janet Leverling", Initials = "JL" },
        new Contact { Id = 2, Name = "Margaret Hamilt", Initials = "MH" },
        new Contact { Id = 3, Name = "Andrew Fuller", Initials = "AF" }
    };

    public class Contact
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Initials { get; set; }
    }
}
```

### Filter Selection

```razor
<SfChip Selection="SelectionType.Multiple" CssClass="e-outline">
    <ChipEvents OnClick="ApplyFilters"></ChipEvents>
    <ChipItems>
        <ChipItem Text="Active" Value="active"></ChipItem>
        <ChipItem Text="Pending" Value="pending"></ChipItem>
        <ChipItem Text="Completed" Value="completed"></ChipItem>
    </ChipItems>
</SfChip>

<p>Active Filters: @string.Join(", ", activeFilters)</p>

@code {
    private List<string> activeFilters = new();

    private void ApplyFilters(ChipEventArgs args)
    {
        var filter = args.Value;
        if (args.Selected)
            activeFilters.Add(filter);
        else
            activeFilters.Remove(filter);
        
        // Apply filters to data
        FilterData();
    }

    private void FilterData()
    {
        // Filter logic
    }
}
```

---

## Best Practices

1. **Use appropriate chip types** - Input for user input, Filter for selections, Choice for options, Action for commands
2. **Provide clear labels** - Make chip text descriptive and concise
3. **Use avatars for people** - Display initials or profile pictures for contact chips
4. **Enable delete when appropriate** - Allow users to remove chips when editable
5. **Handle events properly** - Always handle delete and click events for dynamic behavior
6. **Consider mobile** - Ensure chips are touch-friendly (adequate padding)
7. **Limit chip count** - Use scrolling or grouping for many chips
8. **Provide visual feedback** - Indicate selected state clearly

---

## Next Steps

- Explore [chip-styling.md](chip-styling.md) for appearance customization
- Learn about [chip-advanced-features.md](chip-advanced-features.md) for accessibility
