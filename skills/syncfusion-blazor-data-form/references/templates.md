# Templates and Custom Rendering in Blazor DataForm

## Table of Contents

- [Overview](#overview)
- [FormTemplate](#formtemplate)
- [FormItemTemplate](#formitemtemplate)
- [Template Context](#template-context)
- [Custom Editor Rendering](#custom-editor-rendering)
- [Render Fragments](#render-fragments)
- [Advanced Template Patterns](#advanced-template-patterns)

## Overview

Templates allow you to customize the form structure and individual field rendering. Two primary templates:

1. **FormTemplate** - Customize overall form layout
2. **FormItemTemplate** - Customize individual field rendering

## FormTemplate

The `FormTemplate` lets you wrap the form with custom HTML and styling:

```razor
<SfDataForm Model="@employee">
    <FormTemplate>
        <div class="custom-form-wrapper">
            <div class="form-header">
                <h2>Employee Registration</h2>
                <p>Please fill in all required fields</p>
            </div>

            <div class="form-content">
                @context
            </div>

            <div class="form-footer">
                <small>All fields are required</small>
            </div>
        </div>
    </FormTemplate>

    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
</SfDataForm>

<style>
    .custom-form-wrapper {
        border: 1px solid #ddd;
        border-radius: 8px;
        padding: 20px;
        background: #f9f9f9;
    }

    .form-header {
        margin-bottom: 20px;
        border-bottom: 2px solid #007bff;
        padding-bottom: 10px;
    }

    .form-content {
        margin: 20px 0;
    }

    .form-footer {
        margin-top: 20px;
        text-align: center;
        color: #666;
    }
</style>

@code {
    public class Employee
    {
        [Required]
        public string FirstName { get; set; }

        [Required]
        public string LastName { get; set; }
    }

    private Employee employee = new();
}
```

## FormItemTemplate

The `FormItemTemplate` customizes individual field rendering:

```razor
<SfDataForm Model="@user">
    <FormItems>
        <FormItem Field="@nameof(user.FirstName)" LabelText="First Name">
            <FormItemTemplate Context="formItemContext">
                <div class="custom-field">
                    <label class="custom-label">@formItemContext.LabelText</label>
                    <input type="text" 
                           class="form-control custom-input" 
                           @bind="user.FirstName"
                           placeholder="Enter first name" />
                </div>
            </FormItemTemplate>
        </FormItem>

        <FormItem Field="@nameof(user.Email)" LabelText="Email">
            <FormItemTemplate Context="formItemContext">
                <div class="custom-field">
                    <label class="custom-label">@formItemContext.LabelText</label>
                    <input type="email" 
                           class="form-control custom-input" 
                           @bind="user.Email"
                           placeholder="Enter email" />
                </div>
            </FormItemTemplate>
        </FormItem>
    </FormItems>
</SfDataForm>

<style>
    .custom-field {
        margin-bottom: 15px;
        padding: 10px;
        background: #fff;
        border-radius: 4px;
    }

    .custom-label {
        font-weight: bold;
        color: #333;
        margin-bottom: 5px;
        display: block;
    }

    .custom-input {
        border: 2px solid #ddd;
        transition: border-color 0.3s;
    }

    .custom-input:focus {
        border-color: #007bff;
    }
</style>

@code {
    public class User
    {
        [Required]
        public string FirstName { get; set; }

        [EmailAddress]
        public string Email { get; set; }
    }

    private User user = new();
}
```

## Template Context

Access form metadata through template context:

```csharp
public class FormItemTemplateContext
{
    public string FieldName { get; set; }          // Field property name
    public string LabelText { get; set; }          // Display label
    public object Value { get; set; }              // Current value
    public string PlaceHolder { get; set; }        // Placeholder text
    public bool Enabled { get; set; }              // Is field enabled
    public List<string> ErrorMessages { get; set; } // Validation errors
    public string EditorType { get; set; }         // Field editor type
}
```

Usage example:

```razor
<FormItemTemplate Context="itemContext">
    <div class="field-wrapper">
        <label>@itemContext.LabelText</label>

        @if (itemContext.ErrorMessages?.Any() == true)
        {
            <div class="error-messages">
                @foreach (var error in itemContext.ErrorMessages)
                {
                    <span class="error-text">@error</span>
                }
            </div>
        }

        <input type="text" 
               value="@itemContext.Value" 
               disabled="@(!itemContext.Enabled)"
               placeholder="@itemContext.PlaceHolder" />
    </div>
</FormItemTemplate>
```

## Custom Editor Rendering

Render custom editors for specific field types:

```razor
<SfDataForm Model="@product">
    <FormItems>
        <!-- Standard field -->
        <FormItem Field="@nameof(product.Name)" LabelText="Product Name"></FormItem>

        <!-- Custom color picker editor -->
        <FormItem Field="@nameof(product.Color)" LabelText="Color">
            <FormItemTemplate>
                <div class="color-picker-field">
                    <label>Color</label>
                    <input type="color" 
                           @bind="product.Color" 
                           class="form-control" 
                           style="height: 40px;" />
                    <span>Selected: @product.Color</span>
                </div>
            </FormItemTemplate>
        </FormItem>

        <!-- Custom rating editor -->
        <FormItem Field="@nameof(product.Rating)" LabelText="Rating">
            <FormItemTemplate>
                <div class="rating-field">
                    <label>Rating</label>
                    <div class="stars">
                        @for (int i = 1; i <= 5; i++)
                        {
                            var star = i;
                            <span @onclick="() => product.Rating = star"
                                  class="star @(product.Rating >= star ? "filled" : "")">
                                ★
                            </span>
                        }
                    </div>
                </div>
            </FormItemTemplate>
        </FormItem>

        <!-- Custom slider editor -->
        <FormItem Field="@nameof(product.Quantity)" LabelText="Quantity">
            <FormItemTemplate>
                <div class="slider-field">
                    <label>Quantity: @product.Quantity</label>
                    <input type="range" 
                           min="1" 
                           max="100" 
                           @bind="product.Quantity" 
                           class="form-range" />
                </div>
            </FormItemTemplate>
        </FormItem>
    </FormItems>
</SfDataForm>

<style>
    .color-picker-field input {
        cursor: pointer;
    }

    .rating-field .stars {
        font-size: 24px;
        letter-spacing: 10px;
    }

    .star {
        cursor: pointer;
        color: #ddd;
    }

    .star.filled {
        color: #ffc107;
    }

    .slider-field input {
        width: 100%;
    }
</style>

@code {
    public class Product
    {
        public string Name { get; set; }
        public string Color { get; set; } = "#000000";
        public int Rating { get; set; }
        public int Quantity { get; set; }
    }

    private Product product = new();
}
```

## Render Fragments

Use render fragments for reusable template components:

```razor
@page "/form-with-fragments"

<SfDataForm Model="@contact">
    <FormItems>
        <FormItem Field="@nameof(contact.Name)" LabelText="Name">
            @FieldWithHint(contact.Name, "Enter full name")
        </FormItem>

        <FormItem Field="@nameof(contact.Email)" LabelText="Email">
            @FieldWithValidation(contact.Email, "Invalid email format")
        </FormItem>

        <FormItem Field="@nameof(contact.Phone)" LabelText="Phone">
            @FieldWithIcon(contact.Phone, "📱")
        </FormItem>
    </FormItems>
</SfDataForm>

@code {
    public class Contact
    {
        public string Name { get; set; }
        public string Email { get; set; }
        public string Phone { get; set; }
    }

    private Contact contact = new();

    // Reusable template with hint
    private RenderFragment FieldWithHint(string value, string hint) => @<div class="field-with-hint">
        <small class="hint">@hint</small>
    </div>;

    // Reusable template with validation message
    private RenderFragment FieldWithValidation(string value, string validationMsg) => @<div class="field-with-validation">
        @if (string.IsNullOrEmpty(value))
        {
            <span class="validation-error">@validationMsg</span>
        }
    </div>;

    // Reusable template with icon
    private RenderFragment FieldWithIcon(string value, string icon) => @<div class="field-with-icon">
        <span class="icon">@icon</span>
    </div>;
}
```

## Advanced Template Patterns

### Pattern 1: Conditional Field Rendering

```razor
<SfDataForm Model="@user">
    <FormItems>
        <FormItem Field="@nameof(user.UserType)" 
                  LabelText="User Type"
                  EditorType="FormEditorType.DropDownList">
        </FormItem>

        @if (user.UserType == "Admin")
        {
            <FormItem Field="@nameof(user.AdminLevel)" 
                      LabelText="Admin Level"
                      EditorType="FormEditorType.DropDownList">
            </FormItem>
        }
        else if (user.UserType == "Employee")
        {
            <FormItem Field="@nameof(user.Department)" 
                      LabelText="Department"
                      EditorType="FormEditorType.DropDownList">
            </FormItem>
        }
    </FormItems>
</SfDataForm>

@code {
    public class User
    {
        public string UserType { get; set; }
        public string AdminLevel { get; set; }
        public string Department { get; set; }
    }

    private User user = new();
}
```

### Pattern 2: Nested Template with Binding

```razor
<SfDataForm Model="@employee">
    <FormTemplate>
        <div class="advanced-form">
            <div class="section">
                <h3>Basic Information</h3>
                @BasicSection(employee)
            </div>
            <div class="section">
                <h3>Contact Details</h3>
                @ContactSection(employee)
            </div>
        </div>
    </FormTemplate>
</SfDataForm>

@code {
    private RenderFragment BasicSection(Employee emp) => @<div class="form-section">
        <div class="form-group">
            <label>Name</label>
            <input type="text" @bind="emp.Name" class="form-control" />
        </div>
        <div class="form-group">
            <label>Date of Birth</label>
            <input type="date" @bind="emp.DOB" class="form-control" />
        </div>
    </div>;

    private RenderFragment ContactSection(Employee emp) => @<div class="form-section">
        <div class="form-group">
            <label>Email</label>
            <input type="email" @bind="emp.Email" class="form-control" />
        </div>
        <div class="form-group">
            <label>Phone</label>
            <input type="tel" @bind="emp.Phone" class="form-control" />
        </div>
    </div>;

    public class Employee
    {
        public string Name { get; set; }
        public DateTime DOB { get; set; }
        public string Email { get; set; }
        public string Phone { get; set; }
    }

    private Employee employee = new();
}
```

### Pattern 3: Dynamic Field List

```razor
<SfDataForm Model="@survey">
    <FormTemplate>
        <div class="dynamic-form">
            <h2>Survey Questions</h2>
            
            @foreach (var question in survey.Questions)
            {
                @QuestionField(question)
            }

            <button @onclick="AddQuestion" class="btn btn-secondary">
                + Add Question
            </button>
        </div>
    </FormTemplate>
</SfDataForm>

@code {
    private RenderFragment QuestionField(Question q) => @<div class="question-field">
        <input type="text" @bind="q.Text" placeholder="Question text" />
        <button @onclick="() => RemoveQuestion(q)" class="btn btn-sm btn-danger">Remove</button>
    </div>;

    public class Survey
    {
        public List<Question> Questions { get; set; } = new();
    }

    public class Question
    {
        public string Text { get; set; }
    }

    private Survey survey = new();

    private void AddQuestion()
    {
        survey.Questions.Add(new Question());
    }

    private void RemoveQuestion(Question q)
    {
        survey.Questions.Remove(q);
    }
}
```

## Best Practices

✅ **DO:**
- Keep templates clean and readable
- Reuse fragments across multiple fields
- Use proper HTML structure and semantics
- Add appropriate CSS classes for styling
- Bind data properly with two-way binding
- Test template rendering on different screen sizes

❌ **DON'T:**
- Create overly complex nested templates
- Duplicate template code (use fragments)
- Forget to handle null values
- Create templates that break form validation
- Neglect accessibility (labels, ARIA attributes)

## See Also

- [Layout Customization](layout-customization.md) - Form layout customization
- [Form Items](form-items.md) - Field configuration
- [Events](events.md) - Handle template interactions
