# Editing in Syncfusion Blazor TreeGrid

> **Namespace note:** `EditMode` belongs to `Syncfusion.Blazor.TreeGrid`, **not** `Syncfusion.Blazor.Grids`. When both namespaces are imported (which is common), always qualify it as `Syncfusion.Blazor.TreeGrid.EditMode.Row` (or `.Cell`, `.Dialog`, `.Batch`) to avoid CS0104 ambiguity errors.

## Table of Contents
- [Enable Editing](#enable-editing)
- [Edit Modes](#edit-modes)
- [Toolbar with Edit Actions](#toolbar-with-edit-actions)
- [Cell Edit Types](#cell-edit-types)
- [Column Validation](#column-validation)
- [Custom Validation](#custom-validation)
- [Command Column](#command-column)
- [Dialog Editing](#dialog-editing)
- [Batch Editing](#batch-editing)
- [Template Editing](#template-editing)
- [Adding Child & Sibling Rows](#adding-child--sibling-rows)
- [AutoFill (Excel-like)](#autofill-excel-like)
- [Entity Framework Integration](#entity-framework-integration)
- [Edit Events](#edit-events)

---

## Enable Editing

Always set `IsPrimaryKey="true"` on one column — it is required for all edit operations:

```razor
<SfTreeGrid DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            TreeColumnIndex="1"
            Toolbar="@(new List<string> { "Add", "Edit", "Delete", "Update", "Cancel" })">
    <TreeGridEditSettings AllowAdding="true"
                          AllowEditing="true"
                          AllowDeleting="true"
                          Mode="EditMode.Row" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="ID"        IsPrimaryKey="true" Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="200" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" />
        <TreeGridColumn Field="Priority" HeaderText="Priority"  Width="120" AllowEditing="false" />
    </TreeGridColumns>
</SfTreeGrid>
```

> **Tip:** The editing feature can be disabled for a particular column by setting `AllowEditing="false"` on that `TreeGridColumn`.

> **Troubleshooting:** If editing works only for the first row, check that `IsPrimaryKey="true"` is set on a column. Without a primary key, edit or delete operations act on the first row only.

### Default column values on add new

Set default values for columns when adding new records using `TreeGridColumn.DefaultValue`:

```razor
<TreeGridColumn Field="Duration" HeaderText="Duration" Width="100"
                DefaultValue="@DefaultVal"
                TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />

@code {
    public int DefaultVal { get; set; } = 45;
}
```

### Delete confirmation dialog

Show a confirmation dialog before deleting a record by setting `ShowDeleteConfirmDialog="true"` on `TreeGridEditSettings`:

```razor
<TreeGridEditSettings AllowEditing="true"
                      AllowAdding="true"
                      AllowDeleting="true"
                      ShowDeleteConfirmDialog="true" />
```

> `ShowDeleteConfirmDialog` applies to all edit modes.

### Programmatic CRUD operations

Perform CRUD operations using tree grid methods:

- `AddRecordAsync` — adds a new record
- `UpdateRowAsync` — updates an existing record
- `DeleteRecordAsync` — deletes the selected row

```csharp
// Add a new record
await TreeRef.AddRecordAsync(new TaskItem { TaskId = 100, TaskName = "New Task" });

// Update row at index 1
await TreeRef.UpdateRowAsync(1, updatedData);

// Delete the currently selected row
await TreeRef.DeleteRecordAsync();
```

### Cancel CRUD operations based on a condition

Use cancellable events to conditionally prevent CRUD actions. Set `args.Cancel = true` inside the event handler:

```razor
<TreeGridEvents TValue="TaskItem"
                BeforeRowEditing="BeforeRowEditingHandler"
                RowEditing="RowEditingHandler"
                RowCreating="RowCreatingHandler"
                RowDeleting="RowDeletingHandler" />

@code {
    public void BeforeRowEditingHandler(OnRowEditStartEventArgs args)
    {
        if (args.Data.TaskId > 5) args.Cancel = true;
    }

    public void RowEditingHandler(RowEditingEventArgs<TaskItem> args)
    {
        if (args.Data.TaskId > 6) args.Cancel = true;
    }

    public void RowCreatingHandler(RowCreatingEventArgs<TaskItem> args)
    {
        if (args.Data.TaskId > 7) args.Cancel = true;
    }

    public void RowDeletingHandler(RowDeletingEventArgs<TaskItem> args)
    {
        args.Cancel = true; // Prevent all deletions
    }
}
```

### Custom external edit form

Edit data using a form outside the grid. Use the `RowSelected` event to load the selected row's data, then call `UpdateRowAsync` to save changes:

```razor
<SfTreeGrid @ref="TreeRef" DataSource="@TreeData" IdMapping="TaskId" ParentIdMapping="ParentId">
    <TreeGridEvents TValue="TaskItem" RowSelected="RowSelectHandler" />
    ...
</SfTreeGrid>

@code {
    SfTreeGrid<TaskItem> TreeRef;
    public TaskItem SelectedData = new TaskItem();

    public void RowSelectHandler(RowSelectEventArgs<TaskItem> args)
    {
        SelectedData = args.Data;
    }

    public async Task Save()
    {
        await TreeRef.UpdateRowAsync(1, SelectedData);
    }
}
```

---

## Edit Modes

| Mode | Description | Use When |
|---|---|---|
| `Row` (default) | Entire row becomes editable inline | Most common — simple CRUD |
| `Cell` | Single cell edits on double-click | Spreadsheet-like experience |
| `Dialog` | Edit popup dialog | Complex forms, many fields |
| `Batch` | Multiple edits before a single save | Bulk updates |

```razor
<TreeGridEditSettings Mode="EditMode.Cell" />
<TreeGridEditSettings Mode="EditMode.Dialog" />
<TreeGridEditSettings Mode="EditMode.Batch" />
```

---

## Toolbar with Edit Actions

```razor
Toolbar="@(new List<string> { "Add", "Edit", "Delete", "Update", "Cancel" })"
```

Built-in toolbar items for editing:
- `Add` — adds a new row
- `Edit` — edits the selected row
- `Delete` — deletes the selected row
- `Update` — saves pending changes
- `Cancel` — cancels pending changes

---

## Cell Edit Types

Set `EditType` on each column to use the appropriate input control:

| Data Type | Component | EditType value |
|---|---|---|
| string | TextBox | *(default, omit EditType)* |
| int / double / decimal | NumericTextBox | `NumericEdit` |
| List / enum | DropDownList | `DropDownEdit` |
| Date | DatePicker | `DatePickerEdit` |
| DateTime | DateTimePicker | `DateTimePickerEdit` |
| bool | Checkbox | `BooleanEdit` |
| TimeOnly | TimePicker | `TimePickerEdit` |

```razor
<TreeGridColumns>
    <!-- Default: TextBox -->
    <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />

    <!-- Numeric input -->
    <TreeGridColumn Field="Duration"  HeaderText="Duration" Width="100"
                    EditType="Syncfusion.Blazor.Grids.EditType.NumericEdit"
                    EditorSettings="@NumericParams" />

    <!-- Date picker -->
    <TreeGridColumn Field="StartDate" HeaderText="Start Date" Width="150"
                    Type="Syncfusion.Blazor.Grids.ColumnType.Date"
                    Format="d"
                    EditType="Syncfusion.Blazor.Grids.EditType.DatePickerEdit"
                    EditorSettings="@DateParams" />

    <!-- DateTime picker -->
    <TreeGridColumn Field="CreatedAt" HeaderText="Created At" Width="180"
                    Type="Syncfusion.Blazor.Grids.ColumnType.DateTime"
                    EditType="Syncfusion.Blazor.Grids.EditType.DateTimePickerEdit" />

    <!-- Boolean checkbox -->
    <TreeGridColumn Field="Approved" HeaderText="Approved" Width="100"
                    Type="Syncfusion.Blazor.Grids.ColumnType.Boolean"
                    DisplayAsCheckBox="true"
                    EditType="Syncfusion.Blazor.Grids.EditType.BooleanEdit" />

    <!-- DropDownList -->
    <TreeGridColumn Field="Priority" HeaderText="Priority" Width="120"
                    EditType="Syncfusion.Blazor.Grids.EditType.DropDownEdit" />
</TreeGridColumns>

@code {
    private Syncfusion.Blazor.Grids.NumericEditCellParams NumericParams = new()
    {
        Params = new Syncfusion.Blazor.Inputs.NumericTextBoxModel<object> { Format = "N0", Min = 0, Max = 100 }
    };

    private Syncfusion.Blazor.Grids.DateEditCellParams DateParams = new()
    {
        Params = new Syncfusion.Blazor.Calendars.DatePickerModel { Format = "yyyy-MM-dd" }
    };
}
```

> If `EditType` is not defined, the column defaults to **stringedit** (TextBox).

**⚠️ IMPORTANT - Use Correct ColumnType for Numeric Fields:**
- For integer columns, use `Type="Syncfusion.Blazor.Grids.ColumnType.Integer"`
- For decimal/float/double columns, use `Type="Syncfusion.Blazor.Grids.ColumnType.Decimal"`
- ❌ **Do NOT use `ColumnType.Number`** — it doesn't exist in the enum
- Always include `Type` property on numeric columns for correct filtering, sorting, and formatting

```razor
<!-- ✅ CORRECT - Integer for whole numbers -->
<TreeGridColumn Field="Duration" HeaderText="Duration" 
                Type="Syncfusion.Blazor.Grids.ColumnType.Integer"
                EditType="Syncfusion.Blazor.Grids.EditType.NumericEdit" />

<!-- ✅ CORRECT - Decimal for monetary/fractional values -->
<TreeGridColumn Field="Budget" HeaderText="Budget" Format="C2"
                Type="Syncfusion.Blazor.Grids.ColumnType.Decimal"
                EditType="Syncfusion.Blazor.Grids.EditType.NumericEdit" />

<!-- ❌ WRONG - Do NOT use Number -->
<!-- <TreeGridColumn Field="Price" Type="Syncfusion.Blazor.Grids.ColumnType.Number" /> -->
```

### Cell Edit Template

Use `EditTemplate` inside a `TreeGridColumn` to render a fully custom editor component for that column. The `context` is cast to the row model type.

**AutoComplete in EditTemplate:**

```razor
<TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160">
    <EditTemplate>
        <SfAutoComplete ID="TaskName" TItem="TaskItem" TValue="string"
                        @bind-Value="@((context as TaskItem).TaskName)"
                        DataSource="@TreeData">
            <AutoCompleteFieldSettings Value="TaskName" />
        </SfAutoComplete>
    </EditTemplate>
</TreeGridColumn>
```

**ComboBox in EditTemplate:**

```razor
<TreeGridColumn Field="Priority" HeaderText="Priority" Width="120">
    <EditTemplate>
        <SfComboBox ID="Priority" TItem="PriorityItem" TValue="string"
                    @bind-Value="@((context as TaskItem).Priority)"
                    DataSource="@PriorityList">
            <ComboBoxFieldSettings Value="Name" Text="Name" />
        </SfComboBox>
    </EditTemplate>
</TreeGridColumn>
```

**NumericTextBox in EditTemplate:**

```razor
<TreeGridColumn Field="Duration" HeaderText="Duration" Width="100">
    <EditTemplate>
        <SfNumericTextBox TValue="int?" ID="Duration"
                          @bind-Value="@((context as TaskItem).Duration)"
                          ShowClearButton="true" />
    </EditTemplate>
</TreeGridColumn>
```

**TimePicker in EditTemplate:**

```razor
<TreeGridColumn Field="StartDate" HeaderText="Start Date" Width="130"
                Format="hh:mm tt"
                Type="Syncfusion.Blazor.Grids.ColumnType.DateTime">
    <EditTemplate>
        <SfTimePicker TValue="DateTime?" ID="StartDate"
                      @bind-Value="@((context as TaskItem).StartDate)"
                      Format="hh:mm:tt" ShowClearButton="true" />
    </EditTemplate>
</TreeGridColumn>
```

**MultiSelect DropDown in EditTemplate:**

```razor
<TreeGridColumn Field="ChosenItems" HeaderText="Chosen Items" Width="150">
    <EditTemplate>
        <SfMultiSelect ID="ChosenItems"
                       @bind-Value="@((context as TaskItem).ChosenItems)"
                       DataSource="@AvailableChoices"
                       TValue="string" TItem="ChoiceItem">
            <MultiSelectFieldSettings Value="Name" Text="Name" />
        </SfMultiSelect>
    </EditTemplate>
    <Template>
        @{
            var items = (context as TaskItem).ChosenItems;
            <span>@String.Join(", ", items)</span>
        }
    </Template>
</TreeGridColumn>
```

**RichTextEditor in EditTemplate:**

To display the RTE in edit form, disable HTML encoding by setting `DisableHtmlEncode="false"` on the column:

```razor
<TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160"
                DisableHtmlEncode="false">
    <EditTemplate>
        <SfRichTextEditor ID="TaskName"
                          @bind-Value="@((context as TaskItem).TaskName)" />
    </EditTemplate>
</TreeGridColumn>
```

**Enum column with DropDownList in EditTemplate:**

```razor
<TreeGridColumn Field="Priority" HeaderText="Priority" Width="120">
    <EditTemplate>
        <SfDropDownList ID="Priority"
                        DataSource="@DropDownEnumValues"
                        @bind-Value="@((context as TaskItem).Priority)"
                        TValue="PriorityEnum" TItem="string" />
    </EditTemplate>
</TreeGridColumn>

@code {
    public List<string> DropDownEnumValues = Enum.GetNames(typeof(PriorityEnum)).ToList();
}
```

---

## Column Validation

**⚠️ CRITICAL - Use ValidationRules Property (NOT TreeGridValidationRules Tag):**

Use the `ValidationRules` property with `Syncfusion.Blazor.Grids.ValidationRules` object. Do **NOT** use `<TreeGridValidationRules>` tags.

```razor
<!-- ✅ CORRECT - Use ValidationRules property -->
<TreeGridColumn Field="TaskName" 
                ValidationRules="@(new ValidationRules { Required = true, MinLength = 3, MaxLength = 100 })">
</TreeGridColumn>

<!-- ❌ WRONG - Do NOT use TreeGridValidationRules tag -->
<!-- <TreeGridColumn Field="TaskName">
    <TreeGridValidationRules Required="true" MinLength="3" />
</TreeGridColumn> -->
```

**Example: Complete Validation Setup**

```razor
<!-- Text with length validation -->
<TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="200"
                ValidationRules="@(new ValidationRules { Required = true, MinLength = 3, MaxLength = 100 })">
</TreeGridColumn>

<!-- Numeric with range validation -->
<TreeGridColumn Field="Duration" HeaderText="Duration" Width="100"
                Type="Syncfusion.Blazor.Grids.ColumnType.Integer"
                EditType="Syncfusion.Blazor.Grids.EditType.NumericEdit"
                ValidationRules="@(new ValidationRules { Required = true, Number = true, Min = 1, Max = 999 })">
</TreeGridColumn>

<!-- Percentage (0-100) -->
<TreeGridColumn Field="Progress" HeaderText="Progress" Width="100"
                Type="Syncfusion.Blazor.Grids.ColumnType.Integer"
                EditType="Syncfusion.Blazor.Grids.EditType.NumericEdit"
                ValidationRules="@(new ValidationRules { Number = true, Min = 0, Max = 100 })">
</TreeGridColumn>
```

**ValidationRules Properties:**

| Property | Description |
|---|---|
| `Required` | Field must have a value |
| `Number` | Field must be numeric |
| `Min`/`Max` | Min/max numeric range |
| `MinLength`/`MaxLength` | Min/max string length |

---

**Alternative: Data Annotations**

Use `[Required]`, `[Range]`, `[StringLength]` attributes on your model:

```csharp
public class TaskItem
{
    [Required(ErrorMessage = "Task Name is required")]
    [StringLength(100, MinimumLength = 3)]
    public string TaskName { get; set; }

    [Range(1, 999, ErrorMessage = "Duration must be 1–999")]
    public int Duration { get; set; }
}
```

### Custom validator component

Use the `Validator` property of `TreeGridEditSettings` to inject a custom Blazor validator component into the `EditForm`. Inside the validator, access the data via `ValidatorTemplateContext`:

```razor
<TreeGridEditSettings AllowEditing="true" AllowAdding="true" AllowDeleting="true"
                      Mode="Syncfusion.Blazor.TreeGrid.EditMode.Dialog">
    <Validator>
        @{
            ValidatorTemplateContext ctx = context as ValidatorTemplateContext;
        }
        <MyCustomValidator context="@ctx" />
        <ValidationMessage For="@(() => (ctx.EditContext.Model as TaskItem).Duration)" />
    </Validator>
</TreeGridEditSettings>
```

The custom validator component subscribes to `EditContext` events to run validation:

```csharp
public class MyCustomValidator : ComponentBase
{
    [Parameter] public ValidatorTemplateContext context { get; set; }
    [CascadingParameter] private EditContext CurrentEditContext { get; set; }

    private ValidationMessageStore messageStore;

    protected override void OnInitialized()
    {
        messageStore = new ValidationMessageStore(CurrentEditContext);
        CurrentEditContext.OnValidationRequested += ValidateRequested;
        CurrentEditContext.OnFieldChanged += ValidateField;
    }

    protected void HandleValidation(FieldIdentifier identifier)
    {
        if (identifier.FieldName.Equals("Duration"))
        {
            messageStore.Clear(identifier);
            var duration = (context.EditContext.Model as TaskItem).Duration;
            if (duration < 0)
                messageStore.Add(identifier, "Duration must be greater than 0");
            else if (duration > 30)
                messageStore.Add(identifier, "Duration must be less than 30");
        }
    }

    protected void ValidateField(object editContext, FieldChangedEventArgs args)
        => HandleValidation(args.FieldIdentifier);

    private void ValidateRequested(object editContext, ValidationRequestedEventArgs args)
        => HandleValidation(CurrentEditContext.Field("Duration"));
}
```

**Display validation message using the in-built tooltip:**

Use `ValidatorTemplateContext.ShowValidationMessage(fieldName, isValid, message)` to show errors in the tree grid's built-in tooltip instead of a `ValidationMessage` component (preferred for Inline/Batch edit modes):

```csharp
protected void HandleValidation(FieldIdentifier identifier)
{
    if (identifier.FieldName.Equals("Duration"))
    {
        messageStore.Clear(identifier);
        var duration = (context.EditContext.Model as TaskItem).Duration;
        if (duration < 0)
        {
            messageStore.Add(identifier, "Duration must be greater than 0");
            context.ShowValidationMessage("Duration", false, "Duration must be greater than 0");
        }
        else
        {
            messageStore.Clear(identifier);
            context.ShowValidationMessage("Duration", true, null);
        }
    }
}
```

**Disable the in-built validator:**

To use only `DataAnnotationsValidator` (and bypass the tree grid's built-in `ValidationRules` validator), place `DataAnnotationsValidator` inside the `Validator` tag:

```razor
<TreeGridEditSettings Mode="Syncfusion.Blazor.TreeGrid.EditMode.Dialog">
    <Validator>
        <DataAnnotationsValidator />
    </Validator>
</TreeGridEditSettings>
```

**Validation in dialog template for non-column fields:**

When a field used in a dialog `Template` is not defined as a `TreeGridColumn`, its validation summary appears at the top of the dialog form. Use `DataAnnotationsValidator` together with `ValidationMessage`:

```razor
<TreeGridEditSettings Mode="Syncfusion.Blazor.TreeGrid.EditMode.Dialog">
    <Validator>
        <DataAnnotationsValidator />
    </Validator>
    <Template>
        @{
            var row = context as TaskItem;
        }
        <ValidationMessage For="() => row.TaskName" />
        <!-- custom form fields -->
    </Template>
</TreeGridEditSettings>
```

> Validation messages for fields not defined in `TreeGridColumns` appear as a validation summary at the top of the dialog edit form.

---

## Custom Validation

Create custom `ValidationAttribute` classes for business-specific rules:

**Example 1: Email Validation**

```csharp
using System.ComponentModel.DataAnnotations;

public class EmailValidationAttribute : ValidationAttribute
{
    protected override ValidationResult IsValid(object value, ValidationContext validationContext)
    {
        if (value == null || string.IsNullOrEmpty(value.ToString()))
            return ValidationResult.Success; // Optional field
        
        string email = value.ToString();
        try
        {
            var addr = new System.Net.Mail.MailAddress(email);
            return addr.Address == email ? ValidationResult.Success : 
                new ValidationResult("Invalid email format");
        }
        catch
        {
            return new ValidationResult("Invalid email format");
        }
    }
}

public class TaskItem
{
    [EmailValidation]
    public string AssignedTo { get; set; }
}
```

**Example 2: Enumeration/Choice Validation**

```csharp
public class PriorityValidationAttribute : ValidationAttribute
{
    protected override ValidationResult IsValid(object value, ValidationContext validationContext)
    {
        if (value == null || string.IsNullOrEmpty(value.ToString()))
            return new ValidationResult("Priority is required");
        
        string priority = value.ToString().ToLower();
        var validPriorities = new[] { "high", "medium", "low" };
        
        return validPriorities.Contains(priority) ? ValidationResult.Success :
            new ValidationResult("Priority must be High, Medium, or Low");
    }
}

public class TaskItem
{
    [PriorityValidation]
    public string Priority { get; set; }
}
```

**Example 3: Cross-Field Validation (Business Rules)**

```csharp
public class BudgetDurationValidationAttribute : ValidationAttribute
{
    protected override ValidationResult IsValid(object value, ValidationContext validationContext)
    {
        var taskItem = validationContext.ObjectInstance as TaskItem;
        
        if (taskItem != null)
        {
            // Custom rule: High duration (> 50) requires adequate budget (> 3000)
            if (taskItem.Duration > 50 && taskItem.Budget < 3000)
            {
                return new ValidationResult("Tasks with duration > 50 days require budget > $3000");
            }
        }
        
        return ValidationResult.Success;
    }
}

public class TaskItem
{
    public int Duration { get; set; }
    
    [BudgetDurationValidation]
    public decimal Budget { get; set; }
}
```

**Key Points:**
- Custom validation extends `ValidationAttribute`
- Override `IsValid()` method for validation logic
- Return `ValidationResult.Success` if valid
- Return `new ValidationResult("error message")` if invalid
- Access other properties via `validationContext.ObjectInstance` for cross-field validation
- Apply attribute to model properties: `[CustomAttribute]`

---

## Command Column

Add inline Edit / Delete / Save / Cancel buttons per row without a toolbar. Use `TreeGridColumn.Commands` (via `TreeGridCommandColumns`) to define built-in command buttons:

| Command Button | Action |
|---|---|
| `Edit` | Edit the current row |
| `Delete` | Delete the current row |
| `Save` | Save the edited row |
| `Cancel` | Cancel the edited state |

```razor
<TreeGridColumn HeaderText="Manage Records" Width="150">
    <TreeGridCommandColumns>
        <TreeGridCommandColumn Type="CommandButtonType.Edit"
                               ButtonOption="@(new CommandButtonOptions { IconCss = "e-icons e-edit", CssClass = "e-flat" })" />
        <TreeGridCommandColumn Type="CommandButtonType.Delete"
                               ButtonOption="@(new CommandButtonOptions { IconCss = "e-icons e-delete", CssClass = "e-flat" })" />
        <TreeGridCommandColumn Type="CommandButtonType.Save"
                               ButtonOption="@(new CommandButtonOptions { IconCss = "e-icons e-save", CssClass = "e-flat" })" />
        <TreeGridCommandColumn Type="CommandButtonType.Cancel"
                               ButtonOption="@(new CommandButtonOptions { IconCss = "e-icons e-cancel-icon", CssClass = "e-flat" })" />
    </TreeGridCommandColumns>
</TreeGridColumn>
```

### Custom command

Add custom command buttons in a column using `TreeGridCommandColumns` without specifying a `Type`. Handle the button click in the `CommandClicked` event on `TreeGridEvents`:

```razor
<TreeGridEvents TValue="TaskItem" CommandClicked="OnCommandClicked" />

<TreeGridColumn HeaderText="Manage Records" Width="150">
    <TreeGridCommandColumns>
        <TreeGridCommandColumn ButtonOption="@(new CommandButtonOptions { Content = "Details", CssClass = "e-flat" })" />
    </TreeGridCommandColumns>
</TreeGridColumn>

@code {
    public void OnCommandClicked(CommandClickEventArgs<TaskItem> args)
    {
        // Perform required operations here
    }
}
```

---

## Dialog Editing

Opens a form dialog for editing — good for many fields. Set `Mode="EditMode.Dialog"` on `TreeGridEditSettings`:

```razor
<TreeGridEditSettings Mode="EditMode.Dialog" />
```

**Custom dialog template:**

Use the `Template` property inside `TreeGridEditSettings` to render a fully custom edit form in the dialog. Cast `context` to your model type:

```razor
<TreeGridEditSettings Mode="Syncfusion.Blazor.TreeGrid.EditMode.Dialog">
    <Template>
        @{
            var row = context as TaskItem;
        }
        <div>
            <label>Task Name</label>
            <input @bind="row.TaskName" class="e-input" />
            <label>Duration</label>
            <input type="number" @bind="row.Duration" class="e-input" />
        </div>
    </Template>
</TreeGridEditSettings>
```

> Template form editors should have a `name` attribute.

### Customize the header and footer of the edit dialog

Use `HeaderTemplate` and `FooterTemplate` inside `TreeGridEditSettings` to customize the dialog's header text and footer buttons. Use `TreeGrid.CloseEditAsync()` to cancel and `TreeGrid.EndEditAsync()` to save:

```razor
<SfTreeGrid @ref="TreeRef" ...>
    <TreeGridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true"
                          Mode="Syncfusion.Blazor.TreeGrid.EditMode.Dialog">
        <HeaderTemplate>
            @{
                var row = (context as TaskItem);
                <span>@(row.TaskId == null ? "Insert New Task" : "Edit Task " + row.TaskId)</span>
            }
        </HeaderTemplate>
        <FooterTemplate>
            <SfButton OnClick="@Save" IsPrimary="true">@ButtonText</SfButton>
            <SfButton OnClick="@Cancel">Cancel</SfButton>
        </FooterTemplate>
    </TreeGridEditSettings>
    ...
</SfTreeGrid>

@code {
    SfTreeGrid<TaskItem> TreeRef;
    public string ButtonText { get; set; }

    public string GetHeader(TaskItem value)
    {
        if (value.TaskId == null)
        {
            ButtonText = "Insert";
            return "Insert New Task";
        }
        ButtonText = "Save Changes";
        return "Edit Task " + value.TaskId;
    }

    public async Task Cancel() => await TreeRef.CloseEditAsync();
    public async Task Save()   => await TreeRef.EndEditAsync();
}
```

### Disable components in dialog template

Use `BeforeRowEditing` event to disable specific editors when editing (vs. adding) a record. Set the component's `Enabled` property based on a flag toggled in the event handler:

```razor
<TreeGridEvents TValue="TaskItem" BeforeRowEditing="BeforeRowEditingHandler" />

<TreeGridEditSettings Mode="Syncfusion.Blazor.TreeGrid.EditMode.Dialog">
    <Template>
        @{
            var row = context as TaskItem;
        }
        <SfNumericTextBox ID="TaskId" @bind-Value="row.TaskId" Enabled="@IsEnabled" />
        <!-- other fields -->
    </Template>
</TreeGridEditSettings>

@code {
    public bool IsEnabled = true;

    public void BeforeRowEditingHandler(OnRowEditStartEventArgs args)
    {
        // Disable TaskId field when editing an existing record
        IsEnabled = false;
    }
}
```

### Set focus to a specific editor in dialog template

By default, the first input element in the dialog is focused on open. To focus a different field (e.g., when the first is disabled), call `FocusIn()` in the component's `DataBound` or `Created` event:

```razor
<SfAutoComplete @ref="AutoCompleteRef" ID="TaskName" ... >
    <AutoCompleteEvents TValue="string" TItem="TaskItem"
                        DataBound="FocusAutoComplete" />
</SfAutoComplete>

@code {
    SfAutoComplete<string, TaskItem> AutoCompleteRef;

    private async Task FocusAutoComplete()
    {
        await AutoCompleteRef.FocusIn();
    }
}
```

---

## Batch Editing

Batch editing allows making multiple edits across cells and rows before committing changes to the data source. Double-click a cell to enter edit mode; edits are staged on the client and saved together. Set `Mode="EditMode.Batch"` on `TreeGridEditSettings`:

```razor
<SfTreeGrid Toolbar="@(new List<string> { "Add", "Delete", "Update", "Cancel" })">
    <TreeGridEditSettings Mode="EditMode.Batch"
                          AllowAdding="true"
                          AllowEditing="true"
                          AllowDeleting="true"
                          NewRowPosition="RowPosition.Below" />
    ...
</SfTreeGrid>
```

- Click **Update** (toolbar) or call `EndEditAsync()` to commit all pending changes
- Click **Cancel** to discard all staged changes
- Works with AutoFill (requires `EnableAutoFill="true"` and Cell selection mode)

> A primary key column (`IsPrimaryKey="true"`) is required for Batch editing.

### Confirmation dialog in Batch mode

Enable confirmation prompts for batch save and delete operations to prevent unintended changes. Use `ShowConfirmDialog` for save and `ShowDeleteConfirmDialog` for deletes:

```razor
<TreeGridEditSettings Mode="EditMode.Batch"
                      AllowAdding="true"
                      AllowEditing="true"
                      AllowDeleting="true"
                      ShowConfirmDialog="true"
                      ShowDeleteConfirmDialog="true" />
```

> Confirmation dialogs (`ShowConfirmDialog`, `ShowDeleteConfirmDialog`) are available **only** when `Mode` is set to `Batch`. If `ShowConfirmDialog` is `false`, no confirmation dialog is shown on save.

---

## Template Editing

Provide a fully custom edit UI per cell using `EditTemplate` on a `TreeGridColumn`, or provide a full dialog form using the `Template` property of `TreeGridEditSettings` (see [Dialog Editing](#dialog-editing)).

**Per-cell EditTemplate example:**

```razor
<TreeGridColumn Field="Status" HeaderText="Status" Width="140">
    <EditTemplate>
        @{
            var row = context as TaskItem;
        }
        <SfDropDownList TItem="string" TValue="string"
                        DataSource="@Statuses"
                        @bind-Value="row.Status">
        </SfDropDownList>
    </EditTemplate>
</TreeGridColumn>

@code {
    private string[] Statuses = new[] { "Not Started", "In Progress", "Done", "On Hold" };
}
```

> See the [Cell Edit Types](#cell-edit-types) section for all supported `EditTemplate` component examples (AutoComplete, ComboBox, NumericTextBox, TimePicker, MultiSelect, RichTextEditor, enum DropDownList).

---

## Adding Child & Sibling Rows

Control where new rows are inserted using `TreeGridEditSettings.NewRowPosition`:

| Value | Description |
|---|---|
| `Top` | Inserts at the top of the grid (default) |
| `Bottom` | Inserts at the bottom of the grid |
| `Above` | Inserts above the selected row |
| `Below` | Inserts below the selected row |
| `Child` | Inserts as a child of the selected row |

```razor
<TreeGridEditSettings NewRowPosition="RowPosition.Child" />
```

Programmatically add a row using `AddRecordAsync`:

```csharp
// Add as child of currently selected row (no arguments)
await TreeRef.AddRecordAsync();

// Add with specific data at a specific index and position
await TreeRef.AddRecordAsync(new TaskItem { TaskName = "New Task" }, 2, RowPosition.Child);
```

---

## AutoFill (Excel-like)

AutoFill allows copying cell values across rows by dragging the fill handle at the bottom-right corner of the selection. Enable it with `EnableAutoFill="true"` on `SfTreeGrid`:

```razor
<SfTreeGrid EnableAutoFill="true" ...>
    <TreeGridSelectionSettings Type="Syncfusion.Blazor.Grids.SelectionType.Multiple"
                               Mode="Syncfusion.Blazor.Grids.SelectionMode.Cell"
                               CellSelectionMode="Syncfusion.Blazor.Grids.CellSelectionMode.Box" />
    <TreeGridEditSettings Mode="EditMode.Batch" AllowEditing="true" />
    ...
</SfTreeGrid>
```

**Requirements:**
- `EnableAutoFill="true"` on `SfTreeGrid`
- Selection `Mode` must be `Cell`
- `CellSelectionMode` must be `Box`
- Batch editing must be enabled (`Mode="EditMode.Batch"`)

**Limitations:**
- String values are not parsed to number or date types. Dragging string cells over number cells displays **NaN**; dragging over date cells displays an **empty cell**.
- Linear series and sequential data generation are not supported.

---

## Entity Framework Integration

For EF Core integration with a Web API backend, implement CRUD methods in a data access layer, expose them through a Web API controller, and bind the tree grid to the API using `SfDataManager` with `WebApiAdaptor`.

**Data access layer (AddTask, UpdateTask, DeleteTask):**

```csharp
public void AddTask(Task task)
{
    treedb.Tasks.Add(task);
    treedb.SaveChanges();
}

public void UpdateTask(Task task)
{
    treedb.Entry(task).State = EntityState.Modified;
    treedb.SaveChanges();
}

public void DeleteTask(int id)
{
    Task task = treedb.Tasks.Find(id);
    treedb.Tasks.Remove(task);
    treedb.SaveChanges();
}
```

**Web API controller (POST, PUT, DELETE):**

```csharp
[HttpPost]
public object Post([FromBody] Task task)
{
    db.AddTask(task);
    return task;
}

[HttpPut]
public object Put([FromBody] Task task)
{
    db.UpdateTask(task);
    return task;
}

[HttpDelete("{id}")]
public void Delete(int id)
{
    db.DeleteTask(id);
}
```

**Bind the tree grid to the Web API:**

```razor
<SfTreeGrid TValue="Task" IdMapping="TaskID" ParentIdMapping="ParentID"
            HasChildMapping="IsParent" TreeColumnIndex="0"
            Toolbar="@(new List<string> { "Add", "Edit", "Delete", "Update", "Cancel" })">
    <SfDataManager Url="api/TreeGrid" Adaptor="Adaptors.WebApiAdaptor" CrossDomain="true" />
    <TreeGridEditSettings AllowEditing="true"
                          AllowAdding="true"
                          AllowDeleting="true"
                          Mode="Syncfusion.Blazor.TreeGrid.EditMode.Row"
                          NewRowPosition="Syncfusion.Blazor.TreeGrid.RowPosition.Child" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskID" HeaderText="Task ID" IsPrimaryKey="true" Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration" Width="100" />
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="100" />
    </TreeGridColumns>
</SfTreeGrid>
```

---

## Edit Events

**⚠️ IMPORTANT - Namespace Requirement:**
When using `<TreeGridEvents>` tag with event handlers, **always include the namespace**:
```razor
@using Syncfusion.Blazor.Grids
```

**⚠️ IMPORTANT - Event Handler Property Names:**

| Event Handler | Correct Property | Wrong Property | Example |
|---|---|---|---|
| `BeforeRowEditing` | `args.Data.PropertyName` | — | `args.Data.TaskId` |
| `RowDeleting` | `args.Datas[0].PropertyName` | `args.Data.PropertyName` | `args.Datas[0].HasChildren` |

**Dedicated edit events (recommended):**

| Event | When fired | Cancellable | Args type | Common use |
|---|---|---|---|---|
| `BeforeRowEditing` | Before edit mode opens | ✅ Yes | `OnRowEditStartEventArgs` | Pre-fill, restrict editing |
| `RowEditing` | Before save dialog confirms | ✅ Yes | `RowEditingEventArgs<T>` | Dynamic validation |
| `RowEdited` | After dialog save | ❌ No | `RowEditedEventArgs<T>` | Post-save action |
| `OnCellEdit` | Before cell enters edit mode | ✅ Yes | `CellEditArgs<T>` | Customize edit input |
| `OnCellSave` | Before cell value is saved | ✅ Yes | `CellSaveArgs<T>` | Validate cell value |
| `CellSaved` | After a cell value is saved | ❌ No | `CellSavedArgs<T>` | Post-processing |
| `RowCreating` | Before a new row is inserted | ✅ Yes | `RowCreatingEventArgs<T>` | Set default values |
| `RowCreated` | After a new row is added | ❌ No | `RowCreatedEventArgs<T>` | Post-processing |
| `RowUpdating` | Before a row update is saved | ✅ Yes | `RowUpdatingEventArgs<T>` | Validate entire row |
| `RowUpdated` | After a row update is saved | ❌ No | `RowUpdatedEventArgs<T>` | Sync with DB |
| `RowDeleting` | Before a row is deleted | ✅ Yes | `RowDeletingEventArgs<T>` | Confirm, validate |
| `RowDeleted` | After a row is deleted | ❌ No | `RowDeletedEventArgs<T>` | Cleanup |
| `Cancelling` | Before edit cancel | ✅ Yes | `EditCancelingEventArgs<T>` | Confirm discard |
| `Cancelled` | After edit cancel | ❌ No | `EditCanceledEventArgs<T>` | Cleanup |

```razor
@using Syncfusion.Blazor.Grids

<TreeGridEvents TValue="TaskItem"
                BeforeRowEditing="BeforeRowEditingHandler"
                RowEditing="RowEditingHandler"
                RowEdited="RowEditedHandler"
                OnCellSave="OnCellSaveHandler"
                CellSaved="OnCellSavedHandler"
                RowCreating="OnRowCreatingHandler"
                RowCreated="RowCreatedHandler"
                RowUpdating="OnRowUpdatingHandler"
                RowUpdated="RowUpdatedHandler"
                RowDeleting="OnRowDeletingHandler"
                RowDeleted="RowDeletedHandler"
                Cancelling="CancellingHandler"
                Cancelled="CancelledHandler" />

@code {
    /// <summary>
    /// Fires before edit mode opens (Cancellable)
    /// Use args.Data to access row data
    /// </summary>
    private void BeforeRowEditingHandler(OnRowEditStartEventArgs args)
    {
        //custom logic
    }

    /// <summary>
    /// Fires before save dialog confirms (Cancellable)
    /// </summary>
    private void RowEditingHandler(RowEditingEventArgs<TaskItem> args)
    {
        if (args.Data.Duration < 0)
            args.Cancel = true;
    }

    /// <summary>
    /// Fires before a cell value is saved (Cancellable)
    /// </summary>
    private void OnCellSaveHandler(CellSaveArgs<TaskItem> args)
    {
        if (args.ColumnName == "Duration" && (int)args.Value < 0)
            args.Cancel = true;
    }

    /// <summary>
    /// Fires before a new row is inserted (Cancellable)
    /// </summary>
    private void OnRowCreatingHandler(RowCreatingEventArgs<TaskItem> args)
    {
        // Set default values for new row
        args.Data.Progress = 0;
        args.Data.Priority = "Medium";
    }

    /// <summary>
    /// Fires before a row update is saved (Cancellable)
    /// </summary>
    private void OnRowUpdatingHandler(RowUpdatingEventArgs<TaskItem> args)
    {
        if (args.Data.Duration < 0)
            args.Cancel = true;
    }

    /// <summary>
    /// Fires before a row is deleted (Cancellable)
    /// ✅ Use args.Datas[0] — NOT args.Data
    /// </summary>
    private void OnRowDeletingHandler(RowDeletingEventArgs<TaskItem> args)
    {
        // ✅ CORRECT - Use args.Datas[0]
        var taskName = args.Datas[0].TaskName;

        // Prevent deletion of rows that have children
        if (args.Datas[0].HasChildren)
            args.Cancel = true;
    }

    /// <summary>
    /// Fires after a cell value is saved (not cancellable)
    /// </summary>
    private void OnCellSavedHandler(CellSavedArgs<TaskItem> args)
    {
        Console.WriteLine($"Cell saved: {args.ColumnName} = {args.Value}");
    }

    private void RowEditedHandler(RowEditedEventArgs<TaskItem> args) { }
    private void RowCreatedHandler(RowCreatedEventArgs<TaskItem> args) { }
    private void RowUpdatedHandler(RowUpdatedEventArgs<TaskItem> args) { }
    private void RowDeletedHandler(RowDeletedEventArgs<TaskItem> args)
    {
        LogEvent($"✅ RowDeleted: Task '{args.Datas[0].TaskName}' removed");
    }
    private void CancellingHandler(EditCancelingEventArgs<TaskItem> args) { }
    private void CancelledHandler(EditCanceledEventArgs<TaskItem> args) { }
}
```

**Generic way to handle all edit actions:**

```razor
<TreeGridEvents TValue="TaskItem"
                OnActionBegin="OnActionBeginHandler"
                OnActionComplete="OnActionCompleteHandler" />

@code {
    private void OnActionBeginHandler(ActionEventArgs<TaskItem> args)
    {
        switch (args.RequestType)
        {
            case Syncfusion.Blazor.Grids.Action.Save:
                if (args.Action == "Add")
                    Console.WriteLine("Adding new row...");
                else
                    Console.WriteLine("Updating row...");
                break;
            case Syncfusion.Blazor.Grids.Action.Delete:
                Console.WriteLine("Deleting row...");
                break;
        }
    }

    private async Task OnActionCompleteHandler(ActionEventArgs<TaskItem> args)
    {
        if (args.RequestType == Syncfusion.Blazor.Grids.Action.Save)
        {
            // Persist to database
            await SaveToDatabase(args.Data);
        }
        else if (args.RequestType == Syncfusion.Blazor.Grids.Action.Delete)
        {
            // Handle post-delete
        }
    }
}
```

---
