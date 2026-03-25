# Column Validation and Editing

## Table of Contents
- [When to Use](#when-to-use)
- [Enable Editing](#enable-editing)
- [Validation Rules on GanttColumn](#validation-rules-on-ganttcolumn)
- [Data Annotation Validation](#data-annotation-validation)
- [Custom Validation Attribute](#custom-validation-attribute)
- [Custom Validator Component](#custom-validator-component)
- [Per-Column Edit Type](#per-column-edit-type)
- [Key Properties and APIs](#key-properties-and-apis)
- [Common Scenarios and Decisions](#common-scenarios-and-decisions)
- [Troubleshooting](#troubleshooting)

---

## When to Use

Guide the user to this reference when they need to:
- Validate column data before a row is saved (required fields, numeric ranges, string length)
- Display custom validation messages on specific columns
- Use C# data annotation attributes (`[Required]`, `[Range]`, `[StringLength]`) for validation
- Implement custom business-rule validation that goes beyond built-in rules
- Inject a fully custom validator component into the Gantt edit form
- Control which columns can be edited

> Validation is **not** supported for the Resource column.

---

## Enable Editing

When the user wants to allow adding, editing, or deleting tasks, configure `GanttEditSettings`. Mark the primary key column with `IsPrimaryKey="true"`.

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="1400px"
         Toolbar="@(new List<string>() { "Add", "Edit", "Update", "Delete", "Cancel" })">
    <GanttTaskFields Id="TaskID" Name="ActivityName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true">
    </GanttEditSettings>
    <GanttColumns>
        <GanttColumn Field="TaskID" IsPrimaryKey="true" Width="80"></GanttColumn>
        <GanttColumn Field="ActivityName" Width="160"></GanttColumn>
        <GanttColumn Field="StartDate" Width="150"></GanttColumn>
        <GanttColumn Field="Duration" Width="100" AllowEditing="false"></GanttColumn>
        <GanttColumn Field="Progress" Width="100"></GanttColumn>
    </GanttColumns>
</SfGantt>
```

---

## Validation Rules on GanttColumn

When the user wants to enforce constraints (required, min/max value, string length) on a column using inline validation rules, use the `ValidationRules` property on `GanttColumn`. The type is `Syncfusion.Blazor.Grids.ValidationRules`.

**Supported ValidationRules properties:**

| Property | Type | Purpose |
|---|---|---|
| `Required` | `bool` | Field must not be empty |
| `RangeLength` | `object[]` | String length range: `new object[] { minLen, maxLen }` |
| `Range` | `object[]` | Numeric or date range: `new object[] { min, max }` |
| `Number` | `bool` | Value must be numeric |
| `Min` | `double` | Minimum numeric value |
| `Max` | `double` | Maximum numeric value |
| `Messages` | `Dictionary<string, object>` | Custom error messages per rule key |

```razor
<SfGantt @ref="Gantt" TValue="TaskInfoModel" DataSource="@TaskCollection" Height="450px" Width="1400px"
         Toolbar="@(new List<string>() { "Add", "Edit", "Update", "Delete", "Cancel" })" TreeColumnIndex="1">
    <GanttTaskFields Id="TaskID" Name="ActivityName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttEditSettings AllowAdding="true" AllowDeleting="true" AllowEditing="true"></GanttEditSettings>
    <GanttColumns>
        <GanttColumn Field="TaskID" HeaderText="Task ID" IsPrimaryKey="true"
                     Width="80" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right">
        </GanttColumn>
        <GanttColumn Field="ActivityName" HeaderText="Task Name" Width="160"
            ValidationRules="@(new Syncfusion.Blazor.Grids.ValidationRules {
                Required = true,
                RangeLength = new object[] { 5, 10 },
                Messages = new Dictionary<string, object>() {
                    { "required", "Task Name is required." },
                    { "rangelength", "Task Name must be between 5 and 10 characters." }
                }
            })" />
        <GanttColumn Field="StartDate" HeaderText="Start Date" Width="150" Format="d"
                     EditType="Syncfusion.Blazor.Grids.EditType.DateTimePickerEdit"
                     ValidationRules="@(new Syncfusion.Blazor.Grids.ValidationRules {
                         Required = true,
                         Range = new object[] { new DateTime(2020, 1, 1), DateTime.Now },
                         Messages = new Dictionary<string, object>() {
                             { "required", "Start Date is required." }
                         }
                     })">
        </GanttColumn>
        <GanttColumn Field="Progress" HeaderText="Progress" Width="100"
                     EditType="Syncfusion.Blazor.Grids.EditType.NumericEdit"
                     ValidationRules="@(new Syncfusion.Blazor.Grids.ValidationRules {
                         Required = true,
                         Number = true,
                         Min = 5, Max = 50,
                         Messages = new Dictionary<string, object>() {
                             { "required", "Progress is required." },
                             { "min", "Progress must be greater than 5." },
                             { "max", "Progress must be less than 50." }
                         }
                     })">
        </GanttColumn>
    </GanttColumns>
</SfGantt>

@code {
    private SfGantt<TaskInfoModel> Gantt;
    private List<TaskInfoModel> TaskCollection { get; set; }
    protected override void OnInitialized()
    {
        TaskCollection = new List<TaskInfoModel>
        {
            new TaskInfoModel { TaskID = 1, ActivityName = "Product concept", StartDate = new DateTime(2021, 04, 02), Duration = 5, Progress = 30 },
            new TaskInfoModel { TaskID = 2, ActivityName = "Define usage", StartDate = new DateTime(2021, 04, 02), Duration = 3, Progress = 40, ParentID = 1 },
            new TaskInfoModel { TaskID = 3, ActivityName = "Audience research", StartDate = new DateTime(2021, 04, 02), Duration = 3, Progress = 30, ParentID = 1 }
        };
    }

    public class TaskInfoModel
    {
        public int TaskID { get; set; }
        public string ActivityName { get; set; }
        public DateTime? StartDate { get; set; }
        public DateTime? EndDate { get; set; }
        public int? Duration { get; set; }
        public int Progress { get; set; }
        public int? ParentID { get; set; }
    }
}
```

---

## Data Annotation Validation

When the user wants to declare validation rules directly on the C# model class (instead of on `GanttColumn`), use standard `System.ComponentModel.DataAnnotations` attributes. Add `@using System.ComponentModel.DataAnnotations` to the file.

```razor
@using Syncfusion.Blazor.Gantt
@using System.ComponentModel.DataAnnotations

<SfGantt @ref="Gantt" TValue="TaskInfoModel" DataSource="@TaskCollection" Height="450px" Width="1400px"
         Toolbar="@(new List<string>() { "Add", "Edit", "Update", "Delete", "Cancel" })" TreeColumnIndex="1">
    <GanttTaskFields Id="TaskID" Name="ActivityName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttEditSettings AllowAdding="true" AllowDeleting="true" AllowEditing="true"></GanttEditSettings>
    <GanttColumns>
        <GanttColumn Field="TaskID" IsPrimaryKey="true" Width="80"></GanttColumn>
        <GanttColumn Field="ActivityName" Width="160"></GanttColumn>
        <GanttColumn Field="StartDate" Width="150"></GanttColumn>
        <GanttColumn Field="Progress" Width="100" EditType="Syncfusion.Blazor.Grids.EditType.NumericEdit"></GanttColumn>
    </GanttColumns>
</SfGantt>

@code {
    private SfGantt<TaskInfoModel> Gantt;
    private List<TaskInfoModel> TaskCollection { get; set; }
    protected override void OnInitialized()
    {
        TaskCollection = new List<TaskInfoModel>
        {
            new TaskInfoModel { TaskID = 1, ActivityName = "Product concept", StartDate = new DateTime(2021, 04, 02), Duration = 5, Progress = 30 },
            new TaskInfoModel { TaskID = 2, ActivityName = "Define usage", StartDate = new DateTime(2021, 04, 02), Duration = 3, Progress = 40, ParentID = 1 }
        };
    }

    public class TaskInfoModel
    {
        public int TaskID { get; set; }

        [Required(ErrorMessage = "ActivityName is required")]
        [StringLength(50, MinimumLength = 5, ErrorMessage = "ActivityName must be between 5 and 50 characters")]
        public string ActivityName { get; set; }

        public DateTime? StartDate { get; set; }
        public DateTime? EndDate { get; set; }
        public int? Duration { get; set; }

        [Range(0, 100, ErrorMessage = "Progress must be between 0 and 100")]
        public int Progress { get; set; }

        public int? ParentID { get; set; }
    }
}
```

---

## Custom Validation Attribute

When the user needs validation logic beyond what `[Required]`, `[Range]`, and `[StringLength]` offer (e.g., business rules, cross-field checks, regex), create a class that inherits from `ValidationAttribute` and override `IsValid`.

```razor
@using Syncfusion.Blazor.Gantt
@using System.ComponentModel.DataAnnotations

<SfGantt TValue="TaskInfoModel" DataSource="@TaskCollection" Height="450px" Width="1400px"
         Toolbar="@(new List<string>() { "Add", "Edit", "Update", "Delete", "Cancel" })">
    <GanttTaskFields Id="TaskID" Name="ActivityName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttEditSettings AllowAdding="true" AllowDeleting="true" AllowEditing="true"></GanttEditSettings>
    <GanttColumns>
        <GanttColumn Field="TaskID" IsPrimaryKey="true" Width="80"></GanttColumn>
        <GanttColumn Field="ActivityName" Width="160"></GanttColumn>
        <GanttColumn Field="Progress" Width="100" EditType="Syncfusion.Blazor.Grids.EditType.NumericEdit"></GanttColumn>
    </GanttColumns>
</SfGantt>

@code {
    private List<TaskInfoModel> TaskCollection { get; set; }
    protected override void OnInitialized()
    {
        TaskCollection = new List<TaskInfoModel>
        {
            new TaskInfoModel { TaskID = 1, ActivityName = "Product concept", StartDate = new DateTime(2021, 04, 02), Duration = 5, Progress = 30 },
            new TaskInfoModel { TaskID = 2, ActivityName = "Define usage", StartDate = new DateTime(2021, 04, 02), Duration = 3, Progress = 40, ParentID = 1 }
        };
    }

    public class TaskInfoModel
    {
        public int TaskID { get; set; }

        [CustomValidationActivityName]
        public string ActivityName { get; set; }

        public DateTime? StartDate { get; set; }
        public DateTime? EndDate { get; set; }
        public int? Duration { get; set; }

        [CustomValidationProgress]
        public int Progress { get; set; }

        public int? ParentID { get; set; }
    }

    // Custom validation attribute for ActivityName
    public class CustomValidationActivityName : ValidationAttribute
    {
        protected override ValidationResult IsValid(object value, ValidationContext validationContext)
        {
            var str = value as string;
            if (string.IsNullOrWhiteSpace(str))
                return new ValidationResult("Task Name is required.");
            if (str.Length < 5 || str.Length > 10)
                return new ValidationResult("Task Name must be between 5 and 10 characters.");
            return ValidationResult.Success;
        }
    }

    // Custom validation attribute for Progress
    public class CustomValidationProgress : ValidationAttribute
    {
        protected override ValidationResult IsValid(object value, ValidationContext context)
        {
            if (value == null)
                return new ValidationResult("Progress is required.");
            var v = (int)value;
            if (v < 5)
                return new ValidationResult("Progress must be greater than 5.");
            if (v > 50)
                return new ValidationResult("Progress must be less than 50.");
            return ValidationResult.Success;
        }
    }
}
```

---

## Custom Validator Component

When the user needs full control over form-level validation (showing per-field messages in the Gantt tooltip, reacting to field change events, or implementing complex cross-field rules), inject a custom validator component via `GanttEditSettings.Validator`.

The validator component:
- Accepts `ValidatorTemplateContext` as a parameter (from `Syncfusion.Blazor.Grids`)
- Cascades `EditContext` for field-change subscriptions
- Calls `context.ShowValidationMessage(fieldName, isValid, message)` to display/clear messages
- Implements `IDisposable` to clean up event subscriptions

**Index.razor** — inject the custom validator:

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Grids

<SfGantt TValue="TaskInfoModel" DataSource="@TaskCollection" Height="450px" Width="1400px"
         Toolbar="@(new List<string>() { "Add", "Edit", "Update", "Delete", "Cancel" })" TreeColumnIndex="1">
    <GanttTaskFields Id="TaskID" Name="ActivityName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttEditSettings AllowAdding="true" AllowDeleting="true" AllowEditing="true">
        <Validator>
            @{
                ValidatorTemplateContext txt = context as ValidatorTemplateContext;
            }
            <GanttCustomValidator context="@txt"></GanttCustomValidator>
        </Validator>
    </GanttEditSettings>
    <GanttColumns>
        <GanttColumn Field="TaskID" HeaderText="Task ID" IsPrimaryKey="true" Width="80"></GanttColumn>
        <GanttColumn Field="ActivityName" HeaderText="Task Name" Width="160"></GanttColumn>
        <GanttColumn Field="StartDate" HeaderText="Start Date" Width="150"
                     EditType="Syncfusion.Blazor.Grids.EditType.DateTimePickerEdit">
        </GanttColumn>
        <GanttColumn Field="Duration" HeaderText="Duration" Width="100"></GanttColumn>
        <GanttColumn Field="Progress" HeaderText="Progress" Width="100"
                     EditType="Syncfusion.Blazor.Grids.EditType.NumericEdit">
        </GanttColumn>
    </GanttColumns>
</SfGantt>

@code {
    private List<TaskInfoModel> TaskCollection { get; set; }
    protected override void OnInitialized()
    {
        TaskCollection = new List<TaskInfoModel>
        {
            new TaskInfoModel { TaskID = 1, ActivityName = "Product concept", StartDate = new DateTime(2021, 04, 02), Duration = 5, Progress = 30 },
            new TaskInfoModel { TaskID = 2, ActivityName = "Define usage", StartDate = new DateTime(2021, 04, 02), Duration = 3, Progress = 40, ParentID = 1 }
        };
    }

    public class TaskInfoModel
    {
        public int TaskID { get; set; }
        public string ActivityName { get; set; } = string.Empty;
        public DateTime? StartDate { get; set; }
        public DateTime? EndDate { get; set; }
        public int? Duration { get; set; }
        public int Progress { get; set; }
        public int? ParentID { get; set; }
    }
}
```

**GanttCustomValidator.cs** — the validator component:

```csharp
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Forms;
using Syncfusion.Blazor.Grids;

public class GanttCustomValidator : ComponentBase, IDisposable
{
    [Parameter] public ValidatorTemplateContext context { get; set; }
    [CascadingParameter] private EditContext CurrentEditContext { get; set; }
    private ValidationMessageStore _messageStore;

    protected override void OnInitialized()
    {
        if (CurrentEditContext is null)
            throw new InvalidOperationException("GanttCustomValidator requires a cascading EditContext.");

        _messageStore = new ValidationMessageStore(CurrentEditContext);
        CurrentEditContext.OnValidationRequested += ValidateRequested;
        CurrentEditContext.OnFieldChanged += ValidateField;
    }

    private void AddError(FieldIdentifier id, string message)
    {
        _messageStore.Add(id, message);
        context?.ShowValidationMessage(id.FieldName, false, message);
    }

    private void ClearField(FieldIdentifier id)
    {
        _messageStore.Clear(id);
        context?.ShowValidationMessage(id.FieldName, true, null);
    }

    private void HandleValidation(FieldIdentifier identifier)
    {
        if (identifier.Model is TaskInfoModel model)
        {
            _messageStore.Clear(identifier);
            switch (identifier.FieldName)
            {
                case nameof(TaskInfoModel.TaskID):
                    if (model.TaskID <= 0)
                        AddError(identifier, "Task ID is required.");
                    else
                        ClearField(identifier);
                    break;

                case nameof(TaskInfoModel.ActivityName):
                    if (string.IsNullOrWhiteSpace(model.ActivityName))
                        AddError(identifier, "Task Name is required.");
                    else if (model.ActivityName.Length < 5 || model.ActivityName.Length > 10)
                        AddError(identifier, "Task Name must be between 5 and 10 characters.");
                    else
                        ClearField(identifier);
                    break;

                default:
                    ClearField(identifier);
                    break;
            }
        }
        else
        {
            ClearField(identifier);
        }
    }

    private void ValidateField(object sender, FieldChangedEventArgs e)
        => HandleValidation(e.FieldIdentifier);

    private void ValidateRequested(object sender, ValidationRequestedEventArgs e)
    {
        _messageStore.Clear();
        string[] fields = { nameof(TaskInfoModel.TaskID), nameof(TaskInfoModel.ActivityName), nameof(TaskInfoModel.Progress) };
        foreach (var field in fields)
            HandleValidation(CurrentEditContext.Field(field));
    }

    public void Dispose()
    {
        if (CurrentEditContext is not null)
        {
            CurrentEditContext.OnValidationRequested -= ValidateRequested;
            CurrentEditContext.OnFieldChanged -= ValidateField;
        }
    }
}
```

---

## Per-Column Edit Type

When the user wants to control the input editor shown for a column during editing, set `EditType` using `Syncfusion.Blazor.Grids.EditType`.

| EditType | Input shown |
|---|---|
| `DefaultEdit` | Text input (default for strings) |
| `NumericEdit` | Numeric spinner |
| `DatePickerEdit` | Date picker |
| `DateTimePickerEdit` | Date+time picker |
| `BooleanEdit` | Checkbox |
| `DropDownEdit` | Dropdown list |

```razor
<GanttColumn Field="ActivityName" EditType="Syncfusion.Blazor.Grids.EditType.DefaultEdit" Width="200"></GanttColumn>
<GanttColumn Field="Progress" EditType="Syncfusion.Blazor.Grids.EditType.NumericEdit" Width="120"></GanttColumn>
<GanttColumn Field="StartDate" EditType="Syncfusion.Blazor.Grids.EditType.DatePickerEdit" Format="d" Width="150"></GanttColumn>
<GanttColumn Field="IsActive" EditType="Syncfusion.Blazor.Grids.EditType.BooleanEdit" Width="100"></GanttColumn>
```

---

## Key Properties and APIs

| Property / Class | Where Used | Purpose |
|---|---|---|
| `ValidationRules` | `GanttColumn` | Inline validation rules (`Required`, `Min`, `Max`, `RangeLength`, `Range`, `Number`, `Messages`) |
| `Syncfusion.Blazor.Grids.ValidationRules` | Model / column | The correct type for inline `ValidationRules` |
| `[Required]` / `[Range]` / `[StringLength]` | Model class | Data annotation attributes — declared on model properties |
| `ValidationAttribute` | Model class | Base class for custom validation attributes — override `IsValid()` |
| `GanttEditSettings.Validator` | `GanttEditSettings` | `RenderFragment` to inject custom validator component into edit form |
| `ValidatorTemplateContext` | Custom validator | Exposes current row data; call `ShowValidationMessage(field, isValid, msg)` |
| `ShowValidationMessage(field, isValid, msg)` | `ValidatorTemplateContext` | Displays or clears the Gantt validation tooltip for a column |
| `IsPrimaryKey` | `GanttColumn` | Marks the primary key column — required for editing operations |
| `AllowEditing` | `GanttColumn` | `false` makes a column read-only during editing |
| `EditType` | `GanttColumn` | Sets the input editor type for the column |

---

## Common Scenarios and Decisions

**User wants to require a field and set a min/max length**
→ Use `ValidationRules` on `GanttColumn` with `Required = true` and `RangeLength = new object[] { min, max }`.

**User wants to validate numeric range (e.g., Progress 0–100)**
→ Use `ValidationRules` with `Number = true`, `Min = 0`, `Max = 100`. Or use `[Range(0, 100)]` data annotation on the model.

**User wants custom error messages (not the default text)**
→ Add a `Messages = new Dictionary<string, object>() { { "required", "My message" } }` to `ValidationRules`.

**User wants to use `[Required]` and `[StringLength]` attributes on the model**
→ Add `@using System.ComponentModel.DataAnnotations`, annotate the model properties, and no `ValidationRules` on `GanttColumn` is needed.

**User needs custom business-rule validation (e.g., task name cannot start with a digit)**
→ Create a class inheriting `ValidationAttribute`, override `IsValid()`, and apply the attribute to the model property.

**User needs cross-field validation or complex form-level rules**
→ Implement a custom validator component (a `ComponentBase` + `IDisposable`) and inject it via `GanttEditSettings.Validator`.

**User wants certain columns to be read-only**
→ Set `AllowEditing="false"` on the specific `GanttColumn`.

**User gets errors about validation not working on the Resource column**
→ This is a known limitation — validation is not supported for the Resource column.

---

## Troubleshooting

**Validation messages do not appear**
- Confirm `GanttEditSettings` has `AllowAdding="true"` and/or `AllowEditing="true"`
- Confirm `IsPrimaryKey="true"` is set on the key column — editing requires a primary key
- For data annotation attributes, verify `@using System.ComponentModel.DataAnnotations` is included

**`ValidationRules` type errors at compile time**
- Use `Syncfusion.Blazor.Grids.ValidationRules` (not a local or Gantt-specific type) for the `ValidationRules` property value
- Do not use `<GanttColumnValidationRules>` child elements — the correct pattern is the inline property assignment as shown above

**Custom validator component is not called**
- Ensure the component is placed inside `<GanttEditSettings><Validator>...</Validator></GanttEditSettings>`
- Cast `context` to `ValidatorTemplateContext`: `ValidatorTemplateContext txt = context as ValidatorTemplateContext;`
- Confirm `[CascadingParameter] EditContext` is received; without it `OnInitialized` will throw

**`ShowValidationMessage` has no visible effect**
- The field name passed must match exactly the `Field` property on the `GanttColumn`
- Pass `false` and a non-null message to show; pass `true` and `null` to clear
