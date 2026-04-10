# CheckBox Advanced Features

## Table of Contents
- [Disabled State Handling](#disabled-state-handling)
- [Accessibility Features](#accessibility-features)
- [Keyboard Navigation](#keyboard-navigation)
- [Form Integration](#form-integration)
- [State Management Patterns](#state-management-patterns)
- [Best Practices](#best-practices)

---

## Disabled State Handling

### Programmatically Disable Checkbox

Disable checkbox based on conditions:

```razor
<SfCheckBox Disabled="@isDisabled" Label="Conditional Disable" TChecked="bool"></SfCheckBox>

<button @onclick="ToggleDisable" class="btn btn-secondary">
    Toggle Disable
</button>

@code {
    private bool isDisabled = false;

    private void ToggleDisable()
    {
        isDisabled = !isDisabled;
    }
}
```

### Disable Multiple Checkboxes

Disable specific checkboxes in a group:

```razor
<div class="checkbox-group">
    @foreach (var item in items)
    {
        <SfCheckBox 
            Disabled="@item.IsDisabled"
            Checked="item.IsSelected"
            Label="@item.Name"
            ValueChange="@((ChangeEventArgs<bool> args) => OnItemChange(item.Id, args))">
        </SfCheckBox>
    }
</div>

@code {
    private List<SelectItem> items = new()
    {
        new SelectItem { Id = 1, Name = "Item 1", IsSelected = false, IsDisabled = false },
        new SelectItem { Id = 2, Name = "Item 2", IsSelected = true, IsDisabled = true },
        new SelectItem { Id = 3, Name = "Item 3", IsSelected = false, IsDisabled = false }
    };

    private void OnItemChange(int id, ChangeEventArgs<bool> args)
    {
        var item = items.FirstOrDefault(i => i.Id == id);
        if (item != null)
        {
            item.IsSelected = args.Checked;
        }
    }
}

public class SelectItem
{
    public int Id { get; set; }
    public string Name { get; set; }
    public bool IsSelected { get; set; }
    public bool IsDisabled { get; set; }
}
```

### Disable Based on Selection

Disable other options when specific checkbox is checked:

```razor
<div class="exclusive-checkboxes">
    <SfCheckBox 
        @bind-Checked="@isExclusive"
        Label="Use Exclusive Option"
        ValueChange="@OnExclusiveChange"
        TChecked= "bool">
    </SfCheckBox>

    <SfCheckBox 
        Disabled="@isExclusive"
        @bind-Checked="@useOther"
        Label="Use Other Option"
        ValueChange="@OnOtherChange"
        TChecked= "bool">
    </SfCheckBox>
</div>

@code {
    private bool isExclusive = false;
    private bool useOther = false;

    private void OnExclusiveChange(ChangeEventArgs<bool> args)
    {
        isExclusive = args.Checked;
        if (isExclusive)
        {
            useOther = false;
        }
    }

    private void OnOtherChange(ChangeEventArgs<bool> args)
    {
        useOther = args.Checked;
    }
}
```

---

## Accessibility Features

### ARIA Attributes

Add accessibility attributes for screen readers:

```razor
<SfCheckBox 
    Label="Accept Privacy Policy"
    aria-label="Accept privacy policy"
    aria-describedby="privacy-desc"
    TChecked = "bool">
</SfCheckBox>

<p id="privacy-desc">
    You must accept our privacy policy to continue registration
</p>

@code {
}
```

### Required Checkbox with ARIA

Mark required checkboxes for accessibility:

```razor
<div class="required-field">
    <SfCheckBox 
        Label="I agree to the terms*"
        aria-required="true"
        aria-label="Agree to terms and conditions"
        TChecked = "bool">
    </SfCheckBox>
</div>

<style>
    .required-field {
        position: relative;
    }

    .required-field label::after {
        content: " (required)";
        color: red;
        font-size: 0.9em;
    }
</style>

@code {
}
```

### Fieldset with Legend

Group related checkboxes with accessible fieldset:

```razor
<fieldset>
    <legend>Notification Preferences</legend>
    
    <SfCheckBox Label="Email Notifications" TChecked="bool"></SfCheckBox>
    <SfCheckBox Label="SMS Notifications" TChecked="bool"></SfCheckBox>
    <SfCheckBox Label="Push Notifications" TChecked="bool"></SfCheckBox>
</fieldset>

@code {
}
```

---

## Keyboard Navigation

### Tab Navigation

Navigate between checkboxes using Tab key:

```razor
<div class="keyboard-nav-demo">
    <p>Use Tab to navigate, Space to toggle</p>
    
    <SfCheckBox Label="First Option" TChecked="bool"></SfCheckBox>
    <SfCheckBox Label="Second Option" TChecked="bool"></SfCheckBox>
    <SfCheckBox Label="Third Option" TChecked="bool"></SfCheckBox>
</div>

@code {
    // Checkboxes are automatically keyboard accessible
    // Tab: Move to next checkbox
    // Shift+Tab: Move to previous checkbox
    // Space: Toggle checkbox state
}
```

### Focus Management

Control focus programmatically:

```razor
@ref="firstCheckbox"

<SfCheckBox 
    @ref="firstCheckbox"
    Label="First CheckBox"
    TChecked="bool">
</SfCheckBox>

<SfCheckBox 
    @ref="secondCheckbox"
    Label="Second CheckBox"
    TChecked="bool">
</SfCheckBox>

<button @onclick="FocusSecond" class="btn btn-primary">
    Focus Second Checkbox
</button>

@code {
    private SfCheckBox<bool> firstCheckbox;
    private SfCheckBox<bool> secondCheckbox;

    private async Task FocusSecond()
    {
        if (secondCheckbox != null)
        {
            // Focus the element (implementation depends on Syncfusion API)
            await secondCheckbox.FocusAsync();
        }
    }
}
```

### Space Bar to Toggle

Standard keyboard interaction pattern:

```razor
<SfCheckBox Label="Press Space to toggle" CssClass="keyboard-checkbox" TChecked="bool"></SfCheckBox>

<style>
    .keyboard-checkbox {
        outline: 2px solid transparent;
        outline-offset: 2px;
    }

    .keyboard-checkbox:focus-within {
        outline-color: #0066cc;
    }
</style>

@code {
    // Space bar toggles the checkbox automatically
    // Syncfusion handles this internally
}
```

---

## Form Integration

### Checkbox in Form

Integrate checkbox with form submission:

```razor
<EditForm Model="@formModel" OnValidSubmit="@HandleSubmit">
    <DataAnnotationsValidator />

    <div class="form-group">
        <label class="form-label">Newsletter Subscription</label>
        <SfCheckBox 
            @bind-Checked="@formModel.SubscribeToNewsletter"
            Label="Subscribe to newsletter">
        </SfCheckBox>
        <ValidationMessage For="@(() => formModel.SubscribeToNewsletter)" />
    </div>

    <div class="form-group">
        <label class="form-label">Notifications</label>
        
        <SfCheckBox 
            @bind-Checked="@formModel.EmailNotifications"
            Label="Email Notifications">
        </SfCheckBox>
        
        <SfCheckBox 
            @bind-Checked="@formModel.SmsNotifications"
            Label="SMS Notifications">
        </SfCheckBox>
    </div>

    <button type="submit" class="btn btn-primary">Submit</button>
</EditForm>

@code {
    private FormModel formModel = new();

    private class FormModel
    {
        public bool SubscribeToNewsletter { get; set; }
        public bool EmailNotifications { get; set; }
        public bool SmsNotifications { get; set; }
    }

    private void HandleSubmit()
    {
        Console.WriteLine($"Newsletter: {formModel.SubscribeToNewsletter}");
        Console.WriteLine($"Email: {formModel.EmailNotifications}");
        Console.WriteLine($"SMS: {formModel.SmsNotifications}");
    }
}
```

### Form Validation with Checkboxes

Validate required checkbox selections:

```razor
<EditForm Model="@model" OnValidSubmit="@Submit">
    <DataAnnotationsValidator />

    <div class="form-group">
        <SfCheckBox 
            @bind-Checked="@model.AcceptTerms"
            Label="I agree to terms and conditions">
        </SfCheckBox>
        <ValidationMessage For="@(() => model.AcceptTerms)" />
    </div>

    <div class="form-group">
        <label>Select at least one category:</label>
        @foreach (var category in categories)
        {
            <SfCheckBox 
                @key="category.Id"
                Checked="model.SelectedCategories.Contains(category.Id)"
                Label="@category.Name"
                ValueChange="@((ChangeEventArgs<bool> args) => ToggleCategory(category.Id, args))">
            </SfCheckBox>
        }
        <ValidationMessage For="@(() => model.SelectedCategories)" />
    </div>

    <button type="submit" class="btn btn-primary">Submit</button>
</EditForm>

@code {
    private FormValidationModel model = new();
    private List<Category> categories = new();

    protected override async Task OnInitializedAsync()
    {
        categories = await LoadCategories();
    }

    private void ToggleCategory(int categoryId, ChangeEventArgs<bool> args)
    {
        if (args.Checked)
        {
            if (!model.SelectedCategories.Contains(categoryId))
            {
                model.SelectedCategories.Add(categoryId);
            }
        }
        else
        {
            model.SelectedCategories.Remove(categoryId);
        }
    }

    private void Submit()
    {
        Console.WriteLine("Form submitted successfully!");
    }

    private Task<List<Category>> LoadCategories()
    {
        return Task.FromResult(new List<Category>
        {
            new Category { Id = 1, Name = "Category A" },
            new Category { Id = 2, Name = "Category B" },
            new Category { Id = 3, Name = "Category C" }
        });
    }
}

public class FormValidationModel
{
    [Range(typeof(bool), "true", "true", ErrorMessage = "You must accept terms")]
    public bool AcceptTerms { get; set; }

    public List<int> SelectedCategories { get; set; } = new();
}

public class Category
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

### Checkbox in Data Table

Integrate checkboxes with data grids:

```razor
<table class="table">
    <thead>
        <tr>
            <th>
                <SfCheckBox 
                    EnableTriState="true"
                    Checked="selectAll"
                    ValueChange="@OnSelectAll">
                </SfCheckBox>
            </th>
            <th>Name</th>
            <th>Email</th>
        </tr>
    </thead>
    <tbody>
        @foreach (var user in users)
        {
            <tr>
                <td>
                    <SfCheckBox 
                        Checked="selectedUsers.Contains(user.Id)"
                        ValueChange="@((ChangeEventArgs<bool> args) => ToggleUser(user.Id, args))">
                    </SfCheckBox>
                </td>
                <td>@user.Name</td>
                <td>@user.Email</td>
            </tr>
        }
    </tbody>
</table>

@code {
    private List<User> users = new();
    private List<int> selectedUsers = new();
    private bool? selectAll = false;

    protected override async Task OnInitializedAsync()
    {
        users = await LoadUsers();
    }

    private void OnSelectAll(ChangeEventArgs<bool?> args)
    {
        if (args.Checked.HasValue)
        {
            selectAll = args.Checked.Value;
            if (args.Checked.Value)
            {
                selectedUsers = users.Select(u => u.Id).ToList();
            }
            else
            {
                selectedUsers.Clear();
            }
        }
    }

    private void ToggleUser(int userId, ChangeEventArgs<bool> args)
    {
        if (args.Checked)
        {
            selectedUsers.Add(userId);
        }
        else
        {
            selectedUsers.Remove(userId);
        }

        // Update select all state
        selectAll = selectedUsers.Count == 0 ? false : selectedUsers.Count == users.Count ? true : null;
    }

    private Task<List<User>> LoadUsers()
    {
        return Task.FromResult(new List<User>
        {
            new User { Id = 1, Name = "John Doe", Email = "john@example.com" },
            new User { Id = 2, Name = "Jane Smith", Email = "jane@example.com" }
        });
    }
}

public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
}
```

---

## State Management Patterns

### Two-Way Binding Pattern

Keep checkbox state synchronized with component state:

```razor
<SfCheckBox @bind-Checked="@userPreference.EnableNotifications" Label="Enable Notifications"></SfCheckBox>

<p>Notifications: @(userPreference.EnableNotifications ? "Enabled" : "Disabled")</p>

@code {
    private UserPreference userPreference = new();

    private class UserPreference
    {
        public bool EnableNotifications { get; set; }
    }
}
```

### State with Change Tracking

Track when checkbox state changes:

```razor
<SfCheckBox 
    @bind-Checked="@isChecked"
    ValueChange="@OnChange"
    Label="Track Changes"
    TChecked="bool">
</SfCheckBox>

<div>
    <p>Current State: @isChecked</p>
    <p>Last Changed: @lastChangeTime</p>
    <p>Changed @changeCount times</p>
</div>

@code {
    private bool isChecked = false;
    private DateTime? lastChangeTime;
    private int changeCount = 0;

    private void OnChange(ChangeEventArgs<bool> args)
    {
        isChecked = args.Checked;
        lastChangeTime = DateTime.Now;
        changeCount++;
    }
}
```

### Local vs Global State

Manage checkbox state at different levels:

```razor
<!-- Child component with local state -->
<CheckboxChild @ref="childComponent"></CheckboxChild>

<button @onclick="GetChildState" class="btn btn-secondary">
    Get Child State
</button>

<p>Child State: @childState</p>

@code {
    private CheckboxChild childComponent;
    private bool childState;

    private void GetChildState()
    {
        childState = childComponent.GetState();
    }
}

<!-- Child component -->
@* 
<SfCheckBox @bind-Checked="@localState" Label="Local State"></SfCheckBox>

@code {
    private bool localState = false;

    public bool GetState()
    {
        return localState;
    }
}
*@
```

### Cascading Parameters

Pass checkbox state down to child components:

```razor
<CheckboxProvider>
    <p>Provider active: @CheckboxContext?.IsActive</p>
</CheckboxProvider>

@code {
    [CascadingParameter]
    public CheckboxProvider CheckboxContext { get; set; }
}

<!-- Provider component -->
<CascadingValue Value="this">
    <SfCheckBox 
        @bind-Checked="@IsActive"
        Label="Active">
    </SfCheckBox>
    @ChildContent
</CascadingValue>

@code {
    public bool IsActive { get; set; } = false;

    [Parameter]
    public RenderFragment ChildContent { get; set; }
}
```

---

## Best Practices

1. **Use @bind-Checked for simple binding** - Simplest way to keep state synchronized
2. **Handle ValueChange for complex logic** - Use when you need to perform actions on change
3. **Provide clear labels** - Always label checkboxes for accessibility
4. **Group related checkboxes** - Use containers to visually organize selections
5. **Implement proper validation** - Validate required checkboxes before form submission
6. **Support keyboard navigation** - Ensure checkboxes are keyboard accessible
7. **Provide visual feedback** - Show state changes clearly
8. **Test accessibility** - Use screen readers to verify checkbox behavior
9. **Handle disabled states** - Provide clear indication when checkboxes are disabled
10. **Document complex patterns** - Comment code that implements custom checkbox logic

