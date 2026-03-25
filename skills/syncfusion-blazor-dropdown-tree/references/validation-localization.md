# Validation and Localization

## Table of Contents
- [Form Validation](#form-validation)
- [Value Validation](#value-validation)
- [Accessibility](#accessibility)
- [Localization](#localization)
- [Placeholder Customization](#placeholder-customization)

## Form Validation

Integrate Dropdown Tree with Blazor's form validation system:

```razor
@using System.ComponentModel.DataAnnotations
@using Syncfusion.Blazor.Navigations

<EditForm Model="@Model" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    
    <div class="form-group">
        <label for="employee">Select Employee:</label>
        <SfDropDownTree TItem="EmployeeData" TValue="string" 
            ID="employee"
            @bind-Value="@Model.SelectedEmployeeId"
            DataSource="@Data"
            Placeholder="Choose an employee">
            <ChildContent>
                <DropDownTreeField TItem="EmployeeData" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
            </ChildContent>
        </SfDropDownTree>
        <ValidationMessage For="@(() => Model.SelectedEmployeeId)" />
    </div>
    
    <button type="submit">Submit</button>
</EditForm>

@code {
    FormModel Model = new FormModel();
    
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", HasChild = true },
        new EmployeeData() { Id = "3", PId = "2", Name = "Andrew Fuller" },
        new EmployeeData() { Id = "4", PId = "1", Name = "Nancy Davolio" }
    };

    private async Task HandleValidSubmit()
    {
        // Handle form submission
        await Task.CompletedTask;
    }

    public class FormModel
    {
        [Required(ErrorMessage = "Employee selection is required")]
        [StringLength(50)]
        public string? SelectedEmployeeId { get; set; }
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

<style>
    .form-group {
        margin-bottom: 20px;
    }

    .form-group label {
        display: block;
        margin-bottom: 8px;
        font-weight: 600;
    }

    button {
        background-color: #667eea;
        color: white;
        padding: 10px 20px;
        border: none;
        border-radius: 4px;
        cursor: pointer;
    }

    button:hover {
        background-color: #5568d3;
    }
</style>
```

## Value Validation

Implement custom validation with the `ValueChanging` event:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    @bind-Value="@SelectedValue"
    OnValueChanging="@OnValueChanging"
    DataSource="@Data"
    Placeholder="Select an employee">
    <ChildContent>
        <DropDownTreeField TItem="EmployeeData" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
    </ChildContent>
</SfDropDownTree>

@if (!string.IsNullOrEmpty(ValidationError))
{
    <div class="error-message">@ValidationError</div>
}

@code {
    string? SelectedValue { get; set; }
    string? ValidationError { get; set; }
    
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", Job = "Manager", HasChild = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", Job = "Lead" },
        new EmployeeData() { Id = "3", PId = "1", Name = "Nancy Davolio", Job = "Developer" }
    };

    private async Task OnValueChanging(DdtChangeEventArgs<string> args)
    {
        ValidationError = null;
        
        // Validate that selected item is not a manager role only
        var selected = Data.FirstOrDefault(x => x.Id == args.Value);
        
        if (selected?.Job == "Manager" && selected.HasChild)
        {
            ValidationError = "Cannot select a manager with subordinates";
            args.Cancel = true;
        }
        
        await Task.CompletedTask;
    }

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public string? Job { get; set; }
        public bool HasChild { get; set; }
        public string? PId { get; set; }
    }
}

<style>
    .error-message {
        color: #d32f2f;
        margin-top: 8px;
        padding: 10px;
        background-color: #ffebee;
        border-radius: 4px;
    }
</style>
```

## Accessibility

Enable keyboard navigation and screen reader support:

```razor
@using Syncfusion.Blazor.Navigations

<div class="container">
    <label for="department-select">Department Selection</label>
    
    <SfDropDownTree TItem="DepartmentData" TValue="string" 
        ID="department-select"
        Placeholder="Choose a department"
        TabIndex="0"
        AllowFiltering="true"
        DataSource="@Data"
        aria-label="Department selection dropdown">
        <ChildContent>
            <DropDownTreeField TItem="DepartmentData" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
        </ChildContent>
    </SfDropDownTree>
    
    <p class="help-text">Use arrow keys to navigate, Space/Enter to select, Escape to close</p>
</div>

@code {
    List<DepartmentData> Data = new List<DepartmentData>
    {
        new DepartmentData() { Id = "1", Name = "Engineering", HasChild = true, Expanded = true },
        new DepartmentData() { Id = "2", PId = "1", Name = "Backend Development" },
        new DepartmentData() { Id = "3", PId = "1", Name = "Frontend Development" },
        new DepartmentData() { Id = "4", Name = "Sales", HasChild = true },
        new DepartmentData() { Id = "5", PId = "4", Name = "Account Management" }
    };

    public class DepartmentData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
        public bool Expanded { get; set; }
        public string? PId { get; set; }
    }
}

<style>
    .container {
        padding: 20px;
    }

    .container label {
        display: block;
        margin-bottom: 8px;
        font-weight: 600;
    }

    .help-text {
        font-size: 12px;
        color: #666;
        margin-top: 8px;
    }
</style>
```

### Keyboard Navigation

| Key | Action |
|-----|--------|
| `Arrow Up/Down` | Navigate between items |
| `Space` | Select/deselect current item (with checkboxes) |
| `Enter` | Select item and close popup |
| `Escape` | Close popup without selection |
| `Home` | Go to first item |
| `End` | Go to last item |
| `Tab` | Move to next control |
| `Ctrl+A` | Select all (multi-select mode) |

## Localization

Support multiple languages by setting the Locale property:

```razor
@using Syncfusion.Blazor.Navigations

<button @onclick="@(() => { CurrentLocale = "en"; })">English</button>
<button @onclick="@(() => { CurrentLocale = "fr"; })">French</button>
<button @onclick="@(() => { CurrentLocale = "de"; })">German</button>

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Locale="@CurrentLocale"
    AllowFiltering="true"
    Placeholder="@GetPlaceholder()"
    DataSource="@Data">
    <ChildContent>
        <DropDownTreeField TItem="EmployeeData" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
    </ChildContent>
</SfDropDownTree>

@code {
    string CurrentLocale = "en";

    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true, Expanded = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan", HasChild = true },
        new EmployeeData() { Id = "3", PId = "2", Name = "Andrew Fuller" }
    };

    private string GetPlaceholder() => CurrentLocale switch
    {
        "en" => "Select an employee",
        "fr" => "Sélectionner un employé",
        "de" => "Mitarbeiter wählen",
        _ => "Select an employee"
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

### Supported Locales

Common locales supported:
- `en`: English
- `fr`: French
- `de`: German
- `es`: Spanish
- `ja`: Japanese
- `zh`: Chinese
- `ar`: Arabic
- `ru`: Russian

## Placeholder Customization

Customize the placeholder text:

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Click here to select an employee from the organization"
    FloatLabelType="FloatLabelType.Auto"
    DataSource="@Data">
    <ChildContent>
        <DropDownTreeField TItem="EmployeeData" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
    </ChildContent>
</SfDropDownTree>

@code {
    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan" }
    };

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
        public string? PId { get; set; }
    }
}
```

### Floating Label

```razor
<SfDropDownTree TItem="EmployeeData" TValue="string" 
    Placeholder="Employee Name"
    FloatLabelType="FloatLabelType.Always"
    DataSource="@Data">
    <ChildContent>
        <DropDownTreeField TItem="EmployeeData" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
    </ChildContent>
</SfDropDownTree>
```

### Dynamic Placeholder

```razor
@using Syncfusion.Blazor.Navigations

<SfDropDownTree TItem="EmployeeData" TValue="string" 
    @ref="DropdownRef"
    Placeholder="@DynamicPlaceholder"
    OnPopupOpen="@OnPopupOpen"
    DataSource="@Data">
    <ChildContent>
        <DropDownTreeField TItem="EmployeeData" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
    </ChildContent>
</SfDropDownTree>

@code {
    SfDropDownTree<EmployeeData, string>? DropdownRef { get; set; }
    string DynamicPlaceholder = "Select...";

    List<EmployeeData> Data = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "Steven Buchanan", HasChild = true },
        new EmployeeData() { Id = "2", PId = "1", Name = "Laura Callahan" }
    };

    private async Task OnPopupOpen(PopupEventArgs args)
    {
        DynamicPlaceholder = "Loading employees...";
        await Task.Delay(500);
        DynamicPlaceholder = "Search employees";
    }

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
        public string? PId { get; set; }
    }
}
```

## Complete Validation Example

```razor
@using System.ComponentModel.DataAnnotations
@using Syncfusion.Blazor.Navigations

<EditForm Model="@ValidationModel" OnValidSubmit="@HandleSubmit">
    <DataAnnotationsValidator />
    
    <div class="form-container">
        <h3>Team Member Assignment</h3>
        
        <div class="form-group">
            <label for="department">Department:</label>
            <SfDropDownTree TItem="DepartmentData" TValue="string" 
                ID="department"
                @bind-Value="@ValidationModel.DepartmentId"
                DataSource="@Departments"
                Placeholder="Select department">
                <ChildContent>
                    <DropDownTreeField TItem="DepartmentData" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
                </ChildContent>
            </SfDropDownTree>
            <ValidationMessage For="@(() => ValidationModel.DepartmentId)" />
        </div>
        
        <div class="form-group">
            <label for="team">Team Member:</label>
            <SfDropDownTree TItem="EmployeeData" TValue="string" 
                ID="team"
                @bind-Value="@ValidationModel.EmployeeId"
                DataSource="@Employees"
                AllowFiltering="true"
                Placeholder="Select team member">
                <ChildContent>
                    <DropDownTreeField TItem="EmployeeData" ID="Id" Text="Name" HasChildren="HasChild" ParentID="PId"></DropDownTreeField>
                </ChildContent>
            </SfDropDownTree>
            <ValidationMessage For="@(() => ValidationModel.EmployeeId)" />
        </div>
        
        <button type="submit" class="submit-btn">Assign</button>
    </div>
</EditForm>

@code {
    TeamAssignmentModel ValidationModel = new TeamAssignmentModel();

    List<DepartmentData> Departments = new List<DepartmentData>
    {
        new DepartmentData() { Id = "1", Name = "Engineering", HasChild = true, Expanded = true },
        new DepartmentData() { Id = "2", PId = "1", Name = "Backend" },
        new DepartmentData() { Id = "3", PId = "1", Name = "Frontend" }
    };

    List<EmployeeData> Employees = new List<EmployeeData>
    {
        new EmployeeData() { Id = "1", Name = "John Smith", HasChild = false },
        new EmployeeData() { Id = "2", Name = "Jane Doe", HasChild = false },
        new EmployeeData() { Id = "3", Name = "Bob Johnson", HasChild = false }
    };

    private async Task HandleSubmit()
    {
        // Process form submission
        await Task.CompletedTask;
    }

    public class TeamAssignmentModel
    {
        [Required(ErrorMessage = "Department is required")]
        public string? DepartmentId { get; set; }

        [Required(ErrorMessage = "Team member is required")]
        [StringLength(50)]
        public string? EmployeeId { get; set; }
    }

    public class DepartmentData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
        public bool Expanded { get; set; }
        public string? PId { get; set; }
    }

    public class EmployeeData
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
        public bool HasChild { get; set; }
        public string? PId { get; set; }
    }
}

<style>
    .form-container {
        max-width: 500px;
        margin: 20px auto;
        padding: 20px;
        border: 1px solid #ddd;
        border-radius: 8px;
    }

    .form-group {
        margin-bottom: 20px;
    }

    .form-group label {
        display: block;
        margin-bottom: 8px;
        font-weight: 600;
        color: #333;
    }

    .submit-btn {
        width: 100%;
        padding: 12px;
        background-color: #667eea;
        color: white;
        border: none;
        border-radius: 4px;
        font-weight: 600;
        cursor: pointer;
    }

    .submit-btn:hover {
        background-color: #5568d3;
    }

    ::deep .validation-message {
        color: #d32f2f;
        font-size: 12px;
        margin-top: 4px;
    }
</style>
```

## Validation Properties Reference

| Property | Type | Purpose |
|----------|------|---------|
| `Placeholder` | `string` | Placeholder text when no value is selected |
| `TabIndex` | `int` | Tab order for keyboard navigation |
| `FloatLabelType` | `FloatLabelType` | Label floating behavior (Always, Auto, Never) |
| `EnableFloatLabel` | `bool` | Enable/disable floating label feature |
| `Required` | `bool` | Mark as required field |

## Best Practices

1. **Always use EditForm for validation** - Enables proper form context and validation
2. **Provide clear error messages** - Users should understand what went wrong
3. **Use FilterBarPlaceholder** - Guide users with search hints
4. **Test keyboard navigation** - Ensure all users can navigate via keyboard
5. **Implement Accessibility standards** - Use aria-label and id attributes
6. **Localize all user-facing text** - Support multiple languages
7. **Display loading states** - Show feedback during data loading
