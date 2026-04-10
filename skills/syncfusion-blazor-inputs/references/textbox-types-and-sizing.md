# TextBox Types & Sizing

## Table of Contents
- [Sizing Variants](#sizing-variants)
- [Multi-line TextBox](#multi-line-textbox)
- [Read-only TextBox](#read-only-textbox)
- [Disabled TextBox](#disabled-textbox)
- [Rounded Corners](#rounded-corners)
- [Common Patterns](#common-patterns)

---

## Sizing Variants

Control TextBox size using CSS classes applied via the `CssClass` property:

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 300px;">
    <!-- Small TextBox -->
    <SfTextBox Placeholder="Small size" CssClass="e-small"></SfTextBox>

    <!-- Normal size (default) -->
    <SfTextBox Placeholder="Normal size"></SfTextBox>

    <!-- Large TextBox -->
    <SfTextBox Placeholder="Large size" CssClass="e-large"></SfTextBox>
</div>
```

### Available Size Classes

| Class | Height | Padding | Use Case |
|-------|--------|---------|----------|
| `e-small` | ~24px | Compact | Toolbars, inline forms |
| Default | ~32px | Standard | General forms |
| `e-large` | ~40px | Spacious | Mobile, accessibility |

**Example: Form with Consistent Sizing**

```razor
<SfTextBox Placeholder="Username" CssClass="e-large"></SfTextBox>
<SfTextBox Placeholder="Email" CssClass="e-large"></SfTextBox>
<SfTextBox Placeholder="Phone" CssClass="e-large"></SfTextBox>
```

---

## Multi-line TextBox

Enable multi-line mode for larger text input (textarea behavior) using the `Multiline` property:

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 400px;">
    <!-- Multi-line with 4 rows -->
    <SfTextBox Placeholder="Enter your feedback..."
               Multiline="true"
               Rows="4">
    </SfTextBox>

    <!-- Multi-line with 8 rows -->
    <SfTextBox Placeholder="Enter detailed comments..."
               Multiline="true"
               Rows="8">
    </SfTextBox>
</div>
```

### Multi-line Configuration

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `Multiline` | `bool` | `false` | Enable textarea mode |
| `Rows` | `int` | `2` | Number of visible rows |

### Multi-line Behavior

- ✅ Grows vertically when typing
- ✅ Shows scrollbar when text exceeds row height
- ✅ Supports line breaks (Enter key)
- ✅ Text wrapping enabled by default

**Example: Feedback Form**

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 400px;">
    <h3>Feedback Form</h3>
    
    <SfTextBox @bind-Value="@Name"
               Placeholder="Full Name"
               FloatLabelType="@FloatLabelType.Auto">
    </SfTextBox>

    <SfTextBox @bind-Value="@Feedback"
               Placeholder="Your feedback (min 20 chars)"
               Multiline="true"
               Rows="5"
               FloatLabelType="@FloatLabelType.Auto"
               HtmlAttributes="@new Dictionary<string, object> { { "maxlength", "500" } }">
    </SfTextBox>

    <button @onclick="SubmitFeedback">Submit</button>
</div>

@code {
    private string Name = "";
    private string Feedback = "";

    private void SubmitFeedback()
    {
        if (Feedback.Length >= 20)
        {
            Console.WriteLine($"Feedback from {Name}: {Feedback}");
        }
    }
}
```

---

## Read-only TextBox

Prevent user editing by setting `Readonly` to `true`:

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 300px;">
    <!-- Read-only TextBox -->
    <SfTextBox Value="@FixedValue"
               Readonly="true"
               Placeholder="This cannot be edited">
    </SfTextBox>
</div>

@code {
    private string FixedValue = "Original value from database";
}
```

### Read-only Characteristics

| Feature | Behavior |
|---------|----------|
| **User Input** | Cannot type or paste |
| **Selection** | Can select text (copy) |
| **Style** | Gray background by default |
| **Binding** | Still updates programmatically |

**Example: Display User Profile Data**

```razor
<SfTextBox Value="@CurrentUser.Email"
           Readonly="true"
           Placeholder="Email">
</SfTextBox>

<SfTextBox Value="@CurrentUser.JoinDate.ToString()"
           Readonly="true"
           Placeholder="Member since">
</SfTextBox>
```

---

## Disabled TextBox

Disable interaction completely using the `Enabled` property:

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 300px;">
    <!-- Disabled TextBox -->
    <SfTextBox Placeholder="This is disabled"
               Enabled="false"
               Value="Disabled text">
    </SfTextBox>

    <!-- Toggle Enable/Disable -->
    <SfTextBox @ref="@ToggleBox"
               Placeholder="Toggle me"
               Enabled="@IsEnabled">
    </SfTextBox>

    <button @onclick="@(() => IsEnabled = !IsEnabled)">
        @(IsEnabled ? "Disable" : "Enable")
    </button>
</div>

@code {
    private SfTextBox ToggleBox;
    private bool IsEnabled = true;
}
```

### Disabled vs Read-only

| Property | Selection | Copy | Program Update | Visual |
|----------|-----------|------|-----------------|--------|
| **Readonly** | ✅ Yes | ✅ Yes | ✅ Yes | Gray text |
| **Enabled=false** | ❌ No | ❌ No | ❌ No | Completely disabled |

**Use Cases:**

- **Disabled**: Conditional fields (show based on user role)
- **Read-only**: Display-only information (email confirmation)

---

## Rounded Corners

Add rounded corners to TextBox inputs using CSS classes:

```razor
@using Syncfusion.Blazor.Inputs

<style>
    .rounded-small {
        border-radius: 4px;
    }

    .rounded-medium {
        border-radius: 8px;
    }

    .rounded-large {
        border-radius: 16px;
    }

    .rounded-full {
        border-radius: 50px;
    }
</style>

<div style="margin: 50px auto; width: 300px;">
    <SfTextBox Placeholder="Small radius" CssClass="rounded-small"></SfTextBox>
    <SfTextBox Placeholder="Medium radius" CssClass="rounded-medium"></SfTextBox>
    <SfTextBox Placeholder="Large radius" CssClass="rounded-large"></SfTextBox>
    <SfTextBox Placeholder="Full radius" CssClass="rounded-full"></SfTextBox>
</div>
```

### Radius Values

| Class | Radius | Style |
|-------|--------|-------|
| `rounded-small` | 4px | Subtle, professional |
| `rounded-medium` | 8px | Modern, balanced |
| `rounded-large` | 16px | Bold, contemporary |
| `rounded-full` | 50px | Pill-shaped, trendy |

**Example: Modern Form**

```razor
<style>
    .modern-form {
        max-width: 400px;
        margin: 50px auto;
        display: flex;
        flex-direction: column;
        gap: 15px;
    }

    .form-input {
        border-radius: 8px;
    }
</style>

<div class="modern-form">
    <SfTextBox Placeholder="Username" CssClass="form-input"></SfTextBox>
    <SfTextBox Placeholder="Email" CssClass="form-input"></SfTextBox>
    <SfTextBox Placeholder="Password" CssClass="form-input"></SfTextBox>
    <button>Sign In</button>
</div>
```

---

## Common Patterns

### Combining Size and Style

```razor
<SfTextBox Placeholder="Large rounded input"
           CssClass="e-large rounded-medium"
           FloatLabelType="@FloatLabelType.Auto">
</SfTextBox>
```

### Compact Form

```razor
@using Syncfusion.Blazor.Inputs

<div style="width: 250px;">
    <SfTextBox Placeholder="First" CssClass="e-small"></SfTextBox>
    <SfTextBox Placeholder="Last" CssClass="e-small"></SfTextBox>
    <SfTextBox Placeholder="Email" CssClass="e-small"></SfTextBox>
</div>
```

### Expandable Comments Field

```razor
@using Syncfusion.Blazor.Inputs

<div style="width: 400px;">
    <SfTextBox @bind-Value="@Comment"
               Placeholder="Add a comment..."
               Multiline="true"
               Rows="@CommentRows"
               Input="@ExpandCommentBox">
    </SfTextBox>
</div>

@code {
    private string Comment = "";
    private int CommentRows = 2;

    private void ExpandCommentBox(InputEventArgs args)
    {
        if (args.Value.Contains("\n"))
        {
            CommentRows = Math.Min(8, args.Value.Split('\n').Length + 1);
        }
    }
}
```

---

## Next Steps

- ✅ [Input Configuration](../input-configuration.md) - Add floating labels, formatting
- ✅ [Data Binding & Validation](../data-binding-validation.md) - Bind multi-line data, validate
- ✅ [Styling & Appearance](../styling-appearance.md) - Custom colors and themes
- ✅ [Events & Interactions](../events-handling.md) - Handle text input events
