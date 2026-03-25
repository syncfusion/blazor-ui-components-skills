# Layout Customization in Blazor DataForm

## Overview

Customize form layout with multi-column designs, column spanning, label positioning, button placement, and visual grouping.

## Column Layout Configuration

### Single Column (Default)

By default, forms render in a single column:

```razor
<SfDataForm Model="@employee">
    <FormItems>
        <FormItem Field="@nameof(employee.FirstName)" LabelText="First Name"></FormItem>
        <FormItem Field="@nameof(employee.LastName)" LabelText="Last Name"></FormItem>
        <FormItem Field="@nameof(employee.Email)" LabelText="Email"></FormItem>
    </FormItems>
</SfDataForm>
```

### Multi-Column Layout

Set `ColumnCount` to arrange fields across multiple columns:

```razor
<!-- 2-column layout -->
<SfDataForm Model="@employee" ColumnCount="2">
    <FormItems>
        <FormItem Field="@nameof(employee.FirstName)" LabelText="First Name"></FormItem>
        <FormItem Field="@nameof(employee.LastName)" LabelText="Last Name"></FormItem>
        <FormItem Field="@nameof(employee.Email)" LabelText="Email"></FormItem>
        <FormItem Field="@nameof(employee.PhoneNumber)" LabelText="Phone"></FormItem>
    </FormItems>
</SfDataForm>

<!-- 3-column layout -->
<SfDataForm Model="@employee" ColumnCount="3">
    <FormItems>
        <FormItem Field="@nameof(employee.FirstName)" LabelText="First Name"></FormItem>
        <FormItem Field="@nameof(employee.LastName)" LabelText="Last Name"></FormItem>
        <FormItem Field="@nameof(employee.MiddleName)" LabelText="Middle Name"></FormItem>
        <FormItem Field="@nameof(employee.Email)" LabelText="Email"></FormItem>
        <FormItem Field="@nameof(employee.PhoneNumber)" LabelText="Phone"></FormItem>
        <FormItem Field="@nameof(employee.FaxNumber)" LabelText="Fax"></FormItem>
    </FormItems>
</SfDataForm>

@code {
    public class Employee
    {
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public string MiddleName { get; set; }
        public string Email { get; set; }
        public string PhoneNumber { get; set; }
        public string FaxNumber { get; set; }
    }

    private Employee employee = new();
}
```

## Column Span

Control how many columns a single field spans using `ColumnSpan`:

```razor
<SfDataForm Model="@employee" ColumnCount="2">
    <FormItems>
        <!-- Single columns -->
        <FormItem Field="@nameof(employee.FirstName)" 
                  LabelText="First Name"
                  ColumnSpan="1">
        </FormItem>
        <FormItem Field="@nameof(employee.LastName)" 
                  LabelText="Last Name"
                  ColumnSpan="1">
        </FormItem>

        <!-- Spans both columns -->
        <FormItem Field="@nameof(employee.FullAddress)" 
                  LabelText="Full Address"
                  ColumnSpan="2"
                  EditorType="FormEditorType.TextArea">
        </FormItem>

        <!-- Back to single columns -->
        <FormItem Field="@nameof(employee.City)" 
                  LabelText="City"
                  ColumnSpan="1">
        </FormItem>
        <FormItem Field="@nameof(employee.State)" 
                  LabelText="State"
                  ColumnSpan="1">
        </FormItem>
    </FormItems>
</SfDataForm>

@code {
    public class Employee
    {
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public string FullAddress { get; set; }
        public string City { get; set; }
        public string State { get; set; }
    }

    private Employee employee = new();
}
```

## Label Positioning

Control where labels appear relative to input fields:

### Top Label (Default)

```razor
<SfDataForm Model="@employee" LabelPosition="FormLabelPosition.Top">
    <FormItems>
        <FormItem Field="@nameof(employee.FirstName)" LabelText="First Name"></FormItem>
        <FormItem Field="@nameof(employee.Email)" LabelText="Email"></FormItem>
    </FormItems>
</SfDataForm>
```

Result:
```
First Name
[________]

Email
[________]
```

### Left Label

```razor
<SfDataForm Model="@employee" LabelPosition="FormLabelPosition.Left">
    <FormItems>
        <FormItem Field="@nameof(employee.FirstName)" LabelText="First Name"></FormItem>
        <FormItem Field="@nameof(employee.Email)" LabelText="Email"></FormItem>
    </FormItems>
</SfDataForm>
```

Result:
```
First Name [________]
Email      [________]
```

## Floating Labels

Floating labels appear inside the input and move up when the field has focus or value:

```razor
<SfDataForm Model="@user" LabelPosition="FormLabelPosition.Floating">
    <FormItems>
        <FormItem Field="@nameof(user.FirstName)" LabelText="First Name"></FormItem>
        <FormItem Field="@nameof(user.Email)" LabelText="Email Address"></FormItem>
        <FormItem Field="@nameof(user.PhoneNumber)" LabelText="Phone Number"></FormItem>
    </FormItems>
</SfDataForm>

@code {
    public class User
    {
        public string FirstName { get; set; }
        public string Email { get; set; }
        public string PhoneNumber { get; set; }
    }

    private User user = new();
}
```

Behavior:
- Empty state: Label floats inside input field (placeholder-like)
- Focus/with value: Label moves above input field
- Provides better UX and cleaner appearance

## Form Grouping

Organize related fields into logical groups using `FormGroup`:

```razor
<SfDataForm Model="@employee">
    <FormItems>
        <FormGroup LabelText="Personal Information">
            <FormItem Field="@nameof(employee.FirstName)" LabelText="First Name"></FormItem>
            <FormItem Field="@nameof(employee.LastName)" LabelText="Last Name"></FormItem>
            <FormItem Field="@nameof(employee.DateOfBirth)" LabelText="Date of Birth"></FormItem>
        </FormGroup>

        <FormGroup LabelText="Contact Information">
            <FormItem Field="@nameof(employee.Email)" LabelText="Email"></FormItem>
            <FormItem Field="@nameof(employee.PhoneNumber)" LabelText="Phone"></FormItem>
        </FormGroup>

        <FormGroup LabelText="Employment Details">
            <FormItem Field="@nameof(employee.EmployeeId)" LabelText="Employee ID"></FormItem>
            <FormItem Field="@nameof(employee.Department)" LabelText="Department"></FormItem>
            <FormItem Field="@nameof(employee.Salary)" LabelText="Salary"></FormItem>
        </FormGroup>
    </FormItems>
</SfDataForm>

@code {
    public class Employee
    {
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime DateOfBirth { get; set; }
        public string Email { get; set; }
        public string PhoneNumber { get; set; }
        public string EmployeeId { get; set; }
        public string Department { get; set; }
        public decimal Salary { get; set; }
    }

    private Employee employee = new();
}
```

Visual Result:
```
┌─ Personal Information ─────┐
│ First Name: [_________]   │
│ Last Name:  [_________]   │
│ DOB:        [_________]   │
└───────────────────────────┘

┌─ Contact Information ──────┐
│ Email:      [_________]   │
│ Phone:      [_________]   │
└───────────────────────────┘

┌─ Employment Details ───────┐
│ Employee ID: [_________]  │
│ Department:  [_________]  │
│ Salary:      [_________]  │
└───────────────────────────┘
```

## Collapsible Form Groups

Make form groups collapsible:

```razor
<SfDataForm Model="@employee">
    <FormItems>
        <FormGroup LabelText="Personal Information" IsCollapsed="false">
            <FormItem Field="@nameof(employee.FirstName)" LabelText="First Name"></FormItem>
            <FormItem Field="@nameof(employee.LastName)" LabelText="Last Name"></FormItem>
        </FormGroup>

        <FormGroup LabelText="Advanced Settings" IsCollapsed="true">
            <FormItem Field="@nameof(employee.InternalNotes)" LabelText="Notes"></FormItem>
            <FormItem Field="@nameof(employee.CustomField)" LabelText="Custom Field"></FormItem>
        </FormGroup>
    </FormItems>
</SfDataForm>
```

## Button Customization

### Button Alignment

Control button positioning:

```razor
<SfDataForm Model="@employee" 
            ButtonsAlignment="FormButtonsAlignment.Right">
    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
    <FormButtons>
        <button type="submit" class="btn btn-primary">Save</button>
        <button type="reset" class="btn btn-secondary">Clear</button>
    </FormButtons>
</SfDataForm>
```

Options: `Left`, `Right`, `Center`, `Stretch`

### Custom Buttons

Add custom buttons with custom actions:

```razor
<SfDataForm @ref="dataForm" Model="@employee" OnValidSubmit="HandleSubmit">
    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
    <FormButtons>
        <button type="submit" class="btn btn-primary">Save</button>
        <button type="reset" class="btn btn-secondary">Clear</button>
        <button type="button" @onclick="HandleCancel" class="btn btn-outline-secondary">Cancel</button>
        <button type="button" @onclick="HandlePreview" class="btn btn-info">Preview</button>
    </FormButtons>
</SfDataForm>

@code {
    private SfDataForm dataForm;
    private Employee employee = new();

    private async Task HandleSubmit(EditContext context)
    {
        await SaveEmployeeAsync();
    }

    private async Task HandleCancel()
    {
        await Task.CompletedTask;
        // Navigate back or close
    }

    private async Task HandlePreview()
    {
        // Show preview
        Console.WriteLine($"Preview: {employee.FirstName}");
    }

    private async Task SaveEmployeeAsync()
    {
        // Save logic
    }

    public class Employee
    {
        public string FirstName { get; set; }
        public string LastName { get; set; }
    }
}
```

### Hide Default Buttons

```razor
<SfDataForm Model="@employee" ShowSubmitButton="false" ShowResetButton="false">
    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
    <FormButtons>
        <!-- Your custom buttons only -->
        <button type="button" @onclick="SaveAndContinue" class="btn btn-success">
            Save & Continue
        </button>
    </FormButtons>
</SfDataForm>
```

## Responsive Layout

Automatically adapt to screen size:

```razor
<SfDataForm Model="@employee" 
            ColumnCount="3"
            ResponsiveLayoutSettings="@ResponsiveSettings">
    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
</SfDataForm>

@code {
    private ResponsiveLayoutSettings ResponsiveSettings = new()
    {
        DesktopColumns = 3,
        TabletColumns = 2,
        MobileColumns = 1
    };
}
```

Behavior:
- Desktop (1200px+): 3 columns
- Tablet (768px-1199px): 2 columns
- Mobile (<768px): 1 column

## Advanced Layout Example

Complete form with custom layout, groups, and responsive design:

```razor
<SfDataForm Model="@employee" 
            ColumnCount="2"
            LabelPosition="FormLabelPosition.Top"
            ButtonsAlignment="FormButtonsAlignment.Right"
            OnValidSubmit="HandleSubmit">
    
    <FormValidator>
        <DataAnnotationsValidator></DataAnnotationsValidator>
    </FormValidator>

    <FormItems>
        <FormGroup LabelText="Personal Information">
            <FormItem Field="@nameof(employee.FirstName)" 
                      LabelText="First Name"
                      ColumnSpan="1">
            </FormItem>
            <FormItem Field="@nameof(employee.LastName)" 
                      LabelText="Last Name"
                      ColumnSpan="1">
            </FormItem>
            <FormItem Field="@nameof(employee.DateOfBirth)" 
                      LabelText="Date of Birth"
                      ColumnSpan="2">
            </FormItem>
        </FormGroup>

        <FormGroup LabelText="Contact Information">
            <FormItem Field="@nameof(employee.Email)" 
                      LabelText="Email"
                      ColumnSpan="2">
            </FormItem>
            <FormItem Field="@nameof(employee.Phone)" 
                      LabelText="Phone"
                      ColumnSpan="1">
            </FormItem>
            <FormItem Field="@nameof(employee.Mobile)" 
                      LabelText="Mobile"
                      ColumnSpan="1">
            </FormItem>
        </FormGroup>

        <FormGroup LabelText="Employment">
            <FormItem Field="@nameof(employee.EmployeeId)" 
                      LabelText="Employee ID"
                      ColumnSpan="1">
            </FormItem>
            <FormItem Field="@nameof(employee.Department)" 
                      LabelText="Department"
                      ColumnSpan="1">
            </FormItem>
            <FormItem Field="@nameof(employee.Designation)" 
                      LabelText="Designation"
                      ColumnSpan="2">
            </FormItem>
        </FormGroup>
    </FormItems>

    <FormButtons>
        <button type="submit" class="btn btn-primary">Save Employee</button>
        <button type="reset" class="btn btn-outline-secondary">Clear</button>
    </FormButtons>
</SfDataForm>

@code {
    public class Employee
    {
        [Required]
        public string FirstName { get; set; }

        [Required]
        public string LastName { get; set; }

        public DateTime DateOfBirth { get; set; }

        [EmailAddress]
        public string Email { get; set; }

        public string Phone { get; set; }
        public string Mobile { get; set; }
        public string EmployeeId { get; set; }
        public string Department { get; set; }
        public string Designation { get; set; }
    }

    private Employee employee = new();

    private async Task HandleSubmit(EditContext context)
    {
        await SaveEmployeeAsync();
    }

    private async Task SaveEmployeeAsync()
    {
        Console.WriteLine($"Saved: {employee.FirstName} {employee.LastName}");
    }
}
```

## Best Practices

✅ **DO:**
- Use column layouts to reduce form height
- Group logically related fields
- Use floating labels for modern appearance
- Make forms responsive for mobile
- Provide clear visual hierarchy
- Align buttons consistently

❌ **DON'T:**
- Create overly wide columns (harder to read)
- Mix too many label positions
- Overcrowd form groups
- Ignore mobile responsiveness
- Hide buttons or make them unclear

## See Also

- [Form Items](form-items.md) - Configure individual fields
- [Templates](templates.md) - Custom form rendering
