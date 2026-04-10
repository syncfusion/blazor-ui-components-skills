# CheckBox Features

## Table of Contents
- [Installation and Setup](#installation-and-setup)
- [Basic Implementation](#basic-implementation)
- [Checkbox States](#checkbox-states)
- [Label Configuration](#label-configuration)
- [Change Event Handling](#change-event-handling)
- [Value Binding](#value-binding)
- [Multiple Checkboxes](#multiple-checkboxes)
- [Indeterminate State](#indeterminate-state)

---

## Installation and Setup

### NuGet Package

The CheckBox component is part of the **Syncfusion.Blazor.Buttons** package:

```bash
dotnet add package Syncfusion.Blazor.Buttons
dotnet add package Syncfusion.Blazor.Themes
```

### Required Namespaces

Add to `~/_Imports.razor`:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Buttons
```

### Initialization

Register the Syncfusion service in `Program.cs`:

```csharp
using Syncfusion.Blazor;
builder.Services.AddSyncfusionBlazor();
```

Add themes to `index.html` or `App.razor`:

```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

---

## Basic Implementation

### Simple Checkbox

The most basic checkbox usage:

```razor
<SfCheckBox TChecked="bool"></SfCheckBox>

@code {
    // Simple unchecked checkbox
}
```

### Checkbox with Label

Add descriptive text to the checkbox:

```razor
<SfCheckBox Label="Accept Terms and Conditions" TChecked="bool"></SfCheckBox>

@code {
    // Label displays to the right of checkbox
}
```

### Checkbox with Initial State

Set the checkbox as checked by default:

```razor
<SfCheckBox Checked="true" Label="Enable notifications"></SfCheckBox>

@code {
    // Checkbox appears checked on page load
}
```

---

## Checkbox States

### Unchecked State

Default state of the checkbox:

```razor
<SfCheckBox Label="Unchecked" TChecked="bool"></SfCheckBox>

@code {
    // User can click to toggle to checked
}
```

### Checked State

Checkbox is in checked state:

```razor
<SfCheckBox Checked="true" Label="Checked"></SfCheckBox>

@code {
    // Appears with checkmark
}
```

### Disabled Unchecked

User cannot interact with unchecked checkbox:

```razor
<SfCheckBox Disabled="true" Label="Disabled (Unchecked)" TChecked="bool"></SfCheckBox>

@code {
    // Gray appearance, no interaction
}
```

### Disabled Checked

User cannot interact with checked checkbox:

```razor
<SfCheckBox Checked="true" Disabled="true" Label="Disabled (Checked)"></SfCheckBox>

@code {
    // Gray appearance with checkmark, no interaction
}
```

---

## Label Configuration

### Label Positioning

By default, labels appear to the right of the checkbox. Use label text directly:

```razor
<SfCheckBox Label="Right-aligned label" TChecked="bool"></SfCheckBox>

@code {
    // Label is on the right side
}
```

### Label Text Styling

Customize label appearance with CSS:

```razor
<SfCheckBox Label="Bold Label" CssClass="bold-label" TChecked="bool"></SfCheckBox>

<style>
    .bold-label label {
        font-weight: bold;
        color: #333;
    }
</style>

@code {
}
```

### Checkbox Without Label

Create checkbox without any accompanying text:

```razor
<SfCheckBox TChecked="bool"></SfCheckBox>

@code {
    // Standalone checkbox
}
```

---

## Change Event Handling

### OnChange Event

Detect when user toggles the checkbox:

```razor
<SfCheckBox @bind-Checked="isEnabled" ValueChange="@OnCheckboxChange" TChecked="bool"></SfCheckBox>

@code {
    private bool isEnabled = false;

    private void OnCheckboxChange(ChangeEventArgs<bool> args)
    {
        isEnabled = args.Checked;
        Console.WriteLine($"Checkbox changed to: {args.Checked}");
    }
}
```

### Three-State Change Event

For indeterminate state checkboxes:

```razor
<SfCheckBox EnableTriState="true" ValueChange="@OnStateChange" TChecked="bool?"></SfCheckBox>

@code {
    private void OnStateChange(ChangeEventArgs<bool?> args)
    {
        if (args.Checked == null)
        {
            Console.WriteLine("Indeterminate state");
        }
        else if (args.Checked == true)
        {
            Console.WriteLine("Checked");
        }
        else
        {
            Console.WriteLine("Unchecked");
        }
    }
}
```

### Event with UI Update

Update other UI elements when checkbox changes:

```razor
<SfCheckBox @bind-Checked="isEnabled" ValueChange="@OnToggle" Label="Enable Feature" TChecked="bool"></SfCheckBox>

@if (isEnabled)
{
    <div class="feature-content">
        <p>Feature is now enabled</p>
        <button class="btn btn-primary">Configure Feature</button>
    </div>
}

@code {
    private bool isEnabled = false;

    private void OnToggle(ChangeEventArgs<bool> args)
    {
        isEnabled = args.Checked;
        StateHasChanged();
    }
}
```

---

## Value Binding

### Two-Way Binding with @bind-Checked

Automatically sync checkbox state with component property:

```razor
<SfCheckBox @bind-Checked="userConsent" Label="I agree to privacy policy"></SfCheckBox>

<p>Consent Status: @(userConsent ? "Agreed" : "Not Agreed")</p>

@code {
    private bool userConsent = false;
}
```

### Property Binding

Bind checkbox to a model property:

```razor
@inject HttpClient Http

<SfCheckBox @bind-Checked="userSettings.ReceiveEmails" Label="Receive Email Notifications"></SfCheckBox>

@code {
    private UserSettings userSettings = new();

    protected override async Task OnInitializedAsync()
    {
        userSettings = await Http.GetFromJsonAsync<UserSettings>("api/user/settings");
    }
}

public class UserSettings
{
    public bool ReceiveEmails { get; set; }
    public bool ReceiveSms { get; set; }
}
```

### Nullable Boolean Binding

For tri-state checkboxes supporting indeterminate:

```razor
<SfCheckBox @bind-Checked="@selectionState" EnableTriState="true" Label="Select All"></SfCheckBox>

@code {
    private bool? selectionState = null; // null = indeterminate
}
```

---

## Multiple Checkboxes

### Checkbox List

Create multiple independent checkboxes:

```razor
<div class="checkbox-group">
    <h4>Select Your Interests</h4>
    @foreach (var interest in interests)
    {
        <SfCheckBox 
            @key="interest.Id"
            Checked="interest.IsSelected" 
            Label="@interest.Name"
            ValueChange="@((ChangeEventArgs<bool> args) => OnInterestChange(interest.Id, args))">
        </SfCheckBox>
    }
</div>

<p>Selected: @string.Join(", ", interests.Where(i => i.IsSelected).Select(i => i.Name))</p>

@code {
    private List<Interest> interests = new()
    {
        new Interest { Id = 1, Name = "Sports", IsSelected = false },
        new Interest { Id = 2, Name = "Music", IsSelected = false },
        new Interest { Id = 3, Name = "Technology", IsSelected = false },
        new Interest { Id = 4, Name = "Reading", IsSelected = true }
    };

    private void OnInterestChange(int id, ChangeEventArgs<bool> args)
    {
        var interest = interests.FirstOrDefault(i => i.Id == id);
        if (interest != null)
        {
            interest.IsSelected = args.Checked;
        }
    }
}

public class Interest
{
    public int Id { get; set; }
    public string Name { get; set; }
    public bool IsSelected { get; set; }
}
```

### Grouped Checkboxes with Select All

Multiple checkboxes with a master selector:

```razor
<div class="checkbox-group">
    <SfCheckBox 
        @bind-Checked="@selectAll"
        EnableTriState="true"
        Label="Select All"
        ValueChange="@OnSelectAllChange"
        TChecked="bool?">
    </SfCheckBox>
</div>

<div class="checkbox-list">
    @foreach (var item in items)
    {
        <SfCheckBox 
            @key="item.Id"
            Checked="item.Selected"
            Label="@item.Name"
            ValueChange="@((ChangeEventArgs<bool> args) => OnItemChange(item.Id, args))">
        </SfCheckBox>
    }
</div>

@code {
    private List<CheckItem> items = new()
    {
        new CheckItem { Id = 1, Name = "Item 1", Selected = false },
        new CheckItem { Id = 2, Name = "Item 2", Selected = false },
        new CheckItem { Id = 3, Name = "Item 3", Selected = false }
    };

    private bool? selectAll = false;

    private void OnSelectAllChange(ChangeEventArgs<bool?> args)
    {
        if (args.Checked.HasValue)
        {
            foreach (var item in items)
            {
                item.Selected = args.Checked.Value;
            }
            selectAll = args.Checked;
        }
    }

    private void OnItemChange(int id, ChangeEventArgs<bool> args)
    {
        var item = items.FirstOrDefault(i => i.Id == id);
        if (item != null)
        {
            item.Selected = args.Checked;
        }

        // Update select all checkbox state
        int selectedCount = items.Count(i => i.Selected);
        if (selectedCount == 0)
            selectAll = false;
        else if (selectedCount == items.Count)
            selectAll = true;
        else
            selectAll = null; // Indeterminate
    }
}

public class CheckItem
{
    public int Id { get; set; }
    public string Name { get; set; }
    public bool Selected { get; set; }
}
```

---

## Indeterminate State

### Basic Tristate Checkbox

Checkbox with three possible states: checked, unchecked, indeterminate:

```razor
<SfCheckBox 
    EnableTriState="true"
    Checked="null"
    Label="Select All Options"
    TChecked ="bool?">
</SfCheckBox>

@code {
    // null = indeterminate state (dash icon)
    // true = checked state (checkmark)
    // false = unchecked state (empty)
}
```

### Handling Tristate Changes

Work with three-state checkbox values:

```razor
<SfCheckBox 
    EnableTriState="true"
    @bind-Checked="@itemState"
    ValueChange="@OnTristateChange"
    Label="Sub-items Selection"
    TChecked="bool?">
</SfCheckBox>

<div class="sub-items">
    @if (itemState == true)
    {
        <p>✓ All items selected</p>
    }
    else if (itemState == false)
    {
        <p>✗ No items selected</p>
    }
    else
    {
        <p>⊟ Some items selected</p>
    }
</div>

@code {
    private bool? itemState = null;

    private void OnTristateChange(ChangeEventArgs<bool?> args)
    {
        itemState = args.Checked;
    }
}
```

### Master-Detail Tristate Example

When sub-items are partially selected:

```razor
<SfCheckBox 
    EnableTriState="true"
    Checked="@masterState"
    Label="Select All Items"
    ValueChange="@OnMasterChange"
    TChecked="bool?">
</SfCheckBox>

<div class="sub-items">
    @foreach (var item in subItems)
    {
        <SfCheckBox 
            @key="item.Id"
            Checked="item.Selected"
            Label="@item.Name"
            ValueChange="@((ChangeEventArgs<bool> args) => OnSubItemChange(item.Id, args))">
        </SfCheckBox>
    }
</div>

@code {
    private List<SubItem> subItems = new()
    {
        new SubItem { Id = 1, Name = "Option A", Selected = true },
        new SubItem { Id = 2, Name = "Option B", Selected = false },
        new SubItem { Id = 3, Name = "Option C", Selected = false }
    };

    private bool? masterState = null;

    private void OnMasterChange(ChangeEventArgs<bool?> args)
    {
        if (args.Checked.HasValue)
        {
            foreach (var item in subItems)
            {
                item.Selected = args.Checked.Value;
            }
        }
    }

    private void OnSubItemChange(int id, ChangeEventArgs<bool> args)
    {
        var item = subItems.FirstOrDefault(i => i.Id == id);
        if (item != null)
        {
            item.Selected = args.Checked;
        }

        // Update master state
        int selectedCount = subItems.Count(i => i.Selected);
        masterState = selectedCount == 0 ? false : selectedCount == subItems.Count ? true : null;
    }
}

public class SubItem
{
    public int Id { get; set; }
    public string Name { get; set; }
    public bool Selected { get; set; }
}
```

---

## Common Patterns

### Required Checkbox

Enforce checkbox must be checked before form submission:

```razor
<div class="form-group">
    <SfCheckBox 
        @bind-Checked="@acceptTerms"
        Label="I accept the terms and conditions"
        ValueChange="@OnTermsChange"
        TChecked="bool">
    </SfCheckBox>
    @if (!acceptTerms && formSubmitted)
    {
        <span class="error-text">You must accept the terms</span>
    }
</div>

<button disabled="@(!acceptTerms)" class="btn btn-primary">
    Submit
</button>

@code {
    private bool acceptTerms = false;
    private bool formSubmitted = false;

    private void OnTermsChange(ChangeEventArgs<bool> args)
    {
        acceptTerms = args.Checked;
    }
}
```

### Dynamic Checkbox Generation

Generate checkboxes from data source:

```razor
<SfCheckBox Label="Select Categories" EnableTriState="true" @bind-Checked="selectAll" ValueChange="OnSelectAllChanged" TChecked="bool?"></SfCheckBox>

<div class="categories">
    @foreach (var category in categories)
    {
        <SfCheckBox 
            @bind-Checked="@category.Selected"
            Label="@category.Name"
            ValueChange="@OnCategoryChange"
            TChecked="bool">
        </SfCheckBox>
    }
</div>

@code {
    private List<Category> categories = new();
    private bool? selectAll;

    protected override async Task OnInitializedAsync()
    {
        categories = await LoadCategories();
    }

    private void OnCategoryChange(ChangeEventArgs<bool> args)
    {
        // Recalculate select all state
        var selectedCount = categories.Count(c => c.Selected);
        selectAll = selectedCount == 0 ? false : selectedCount == categories.Count ? true : null;
    }

    private void OnSelectAllChanged(ChangeEventArgs<bool?> args)
    {
        if (args.Checked.HasValue)
        {
            foreach (var cat in categories)
            {
                cat.Selected = args.Checked.Value;
            }
        }
    }

    private Task<List<Category>> LoadCategories()
    {
        return Task.FromResult(new List<Category>
        {
            new Category { Id = 1, Name = "Uncategorized", Selected = false },
            new Category { Id = 2, Name = "Urgent", Selected = false },
            new Category { Id = 3, Name = "Important", Selected = false }
        });
    }
    public class Category
    {
        public int Id { get; set; }
        public string Name { get; set; } = string.Empty;
        public bool Selected { get; set; }
    }
}
```

---

## Best Practices

1. **Always provide labels** - Labels improve usability and accessibility
2. **Group related checkboxes** - Use containers to visually group related selections
3. **Handle indeterminate state** - Use tristate for master-detail relationships
4. **Provide visual feedback** - Show current selection count or status
5. **Validate required checkboxes** - Prevent form submission without required selections
6. **Disable when appropriate** - Disable checkboxes that don't apply in current context
7. **Use consistent spacing** - Maintain uniform spacing between multiple checkboxes
8. **Update select-all logic** - Automatically update master checkbox when sub-items change

