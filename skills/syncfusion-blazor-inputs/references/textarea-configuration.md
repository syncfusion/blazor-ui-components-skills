# TextArea - Configuration Options

Comprehensive guide to configuring the Syncfusion Blazor TextArea component including sizing, resize behavior, character limits, labels, and state management.

## Table of Contents
- [Sizing Configuration](#sizing-configuration)
- [Resize Modes](#resize-modes)
- [Character Limits](#character-limits)
- [Placeholder and Labels](#placeholder-and-labels)
- [State Management](#state-management)
- [Width and Responsive Design](#width-and-responsive-design)
- [HTML Attributes](#html-attributes)

## Sizing Configuration

### RowCount Property

Set the number of visible text rows:

```razor
<!-- Small textarea (3 rows) -->
<SfTextArea RowCount="3" Placeholder="Brief comment..."></SfTextArea>

<!-- Medium textarea (5 rows) -->
<SfTextArea RowCount="5" Placeholder="Standard description..."></SfTextArea>

<!-- Large textarea (10 rows) -->
<SfTextArea RowCount="10" Placeholder="Detailed explanation..."></SfTextArea>
```

**Default:** `2`  
**When to Use:**
- 3-5 rows: Comments, short descriptions
- 6-8 rows: Messages, feedback forms
- 10+ rows: Articles, detailed content

### ColumnCount Property

Set the approximate width in characters:

```razor
<!-- Narrow (30 columns) -->
<SfTextArea ColumnCount="30" Placeholder="Short text..."></SfTextArea>

<!-- Standard (50 columns) -->
<SfTextArea ColumnCount="50" Placeholder="Normal width..."></SfTextArea>

<!-- Wide (80 columns) -->
<SfTextArea ColumnCount="80" Placeholder="Wide text area..."></SfTextArea>
```

**Default:** `20`  
**Note:** ColumnCount affects initial width but may be overridden by CSS or `Width` property.

### Combined Row and Column Sizing

```razor
<SfTextArea @bind-Value="@articleContent"
            RowCount="12"
            ColumnCount="80"
            Placeholder="Write your article here...">
</SfTextArea>

@code {
    private string articleContent = "";
}
```

## Resize Modes

Control how users can resize the textarea using the `ResizeMode` property.

### Resize.Both (Default)

Allow resizing in both directions:

```razor
<SfTextArea ResizeMode="Resize.Both"
            RowCount="5"
            Placeholder="Resize me in any direction...">
</SfTextArea>
```

User can drag from bottom-right corner to resize both horizontally and vertically.

### Resize.Vertical

Allow only vertical resizing:

```razor
<SfTextArea ResizeMode="Resize.Vertical"
            RowCount="4"
            Width="100%"
            Placeholder="Resize vertically only...">
</SfTextArea>
```

**Use When:** You want fixed width but flexible height (common in forms).

### Resize.Horizontal

Allow only horizontal resizing:

```razor
<SfTextArea ResizeMode="Resize.Horizontal"
            RowCount="3"
            Placeholder="Resize horizontally only...">
</SfTextArea>
```

**Use When:** Fixed height layout with variable width needs.

### Resize.None

Disable user resizing:

```razor
<SfTextArea ResizeMode="Resize.None"
            RowCount="5"
            ColumnCount="60"
            Placeholder="Fixed size - no resizing...">
</SfTextArea>
```

**Use When:**
- Consistent UI layout required
- Mobile-responsive designs
- Controlled form layouts

### Resize Mode Comparison

```razor
<div class="row">
    <div class="col-md-6">
        <h5>Resizable (Both)</h5>
        <SfTextArea ResizeMode="Resize.Both" RowCount="4"></SfTextArea>
    </div>
    
    <div class="col-md-6">
        <h5>Fixed (None)</h5>
        <SfTextArea ResizeMode="Resize.None" RowCount="4"></SfTextArea>
    </div>
</div>
```

## Character Limits

### MaxLength Property

Restrict maximum character input:

```razor
<!-- Tweet-like limit -->
<SfTextArea @bind-Value="@shortText"
            MaxLength="280"
            Placeholder="Max 280 characters...">
</SfTextArea>

<!-- Product description limit -->
<SfTextArea @bind-Value="@description"
            MaxLength="500"
            RowCount="5"
            Placeholder="Product description (max 500 chars)...">
</SfTextArea>

@code {
    private string shortText = "";
    private string description = "";
}
```

**Default:** No limit (null)  
**Behavior:** Prevents typing beyond limit, paste operations are truncated.

### Character Counter Implementation

Display remaining characters to users:

```razor
<div class="character-limit-container">
    <SfTextArea @bind-Value="@userComment"
                MaxLength="200"
                RowCount="4"
                Placeholder="Enter your comment...">
    </SfTextArea>
    
    <div class="character-count">
        <span class="@GetCounterClass()">
            @GetCharacterCount() / 200
        </span>
    </div>
</div>

@code {
    private string userComment = "";
    
    private int GetCharacterCount() => userComment?.Length ?? 0;
    
    private string GetCounterClass()
    {
        var count = GetCharacterCount();
        if (count > 180) return "text-danger";
        if (count > 150) return "text-warning";
        return "text-muted";
    }
}

<style>
    .character-limit-container {
        position: relative;
    }
    
    .character-count {
        text-align: right;
        font-size: 0.875rem;
        margin-top: 4px;
    }
</style>
```

## Placeholder and Labels

### Placeholder Text

Show hint text when textarea is empty:

```razor
<SfTextArea Placeholder="Type your message here..."
            RowCount="4">
</SfTextArea>
```

**Best Practices:**
- Use clear, actionable hints
- Keep it concise
- Indicate expected format if needed

### FloatLabelType

Create animated floating labels:

```razor
<!-- Auto: Label floats on focus or when value exists -->
<SfTextArea @bind-Value="@comment"
            FloatLabelType="FloatLabelType.Auto"
            Placeholder="Comment">
</SfTextArea>

<!-- Always: Label always stays above -->
<SfTextArea @bind-Value="@note"
            FloatLabelType="FloatLabelType.Always"
            Placeholder="Note">
</SfTextArea>

<!-- Never: No floating label (default) -->
<SfTextArea @bind-Value="@text"
            FloatLabelType="FloatLabelType.Never"
            Placeholder="Enter text...">
</SfTextArea>

@code {
    private string comment = "";
    private string note = "";
    private string text = "";
}
```

**FloatLabelType Options:**
- `Auto`: Label floats when focused or has value
- `Always`: Label permanently floated
- `Never`: No floating behavior (default)

## State Management

### ReadOnly State

Prevent editing while allowing selection and copying:

```razor
<SfTextArea @bind-Value="@existingContent"
            ReadOnly="true"
            RowCount="6"
            Placeholder="This content is read-only">
</SfTextArea>

@code {
    private string existingContent = "This is existing content that users can read but not modify.";
}
```

**Use Cases:**
- Display submitted content
- Show historical data
- Preview mode

### Disabled State

Completely disable the textarea:

```razor
<SfTextArea @bind-Value="@lockedContent"
            Disabled="@isLocked"
            RowCount="5">
</SfTextArea>

<button @onclick="@(() => isLocked = !isLocked)">
    @(isLocked ? "Enable" : "Disable") TextArea
</button>

@code {
    private string lockedContent = "This content is locked.";
    private bool isLocked = true;
}
```

**Differences:**
- **ReadOnly**: Focusable, selectable, submittable in forms
- **Disabled**: Not focusable, grayed out, not submitted in forms

### Conditional Configuration

```razor
<SfTextArea @bind-Value="@userInput"
            ReadOnly="@(!isEditing)"
            MaxLength="@maxChars"
            RowCount="@rows">
</SfTextArea>

<button @onclick="ToggleEdit">@(isEditing ? "Lock" : "Edit")</button>

@code {
    private string userInput = "";
    private bool isEditing = false;
    private int maxChars = 500;
    private int rows = 5;
    
    private void ToggleEdit() => isEditing = !isEditing;
}
```

## Width and Responsive Design

### Width Property

Set explicit width:

```razor
<!-- Percentage width -->
<SfTextArea Width="100%" RowCount="4"></SfTextArea>

<!-- Pixel width -->
<SfTextArea Width="500px" RowCount="4"></SfTextArea>

<!-- CSS units -->
<SfTextArea Width="50vw" RowCount="4"></SfTextArea>
```

**Default:** `"100%"`

### Responsive Width Patterns

```razor
<div class="container">
    <!-- Full width on mobile, constrained on desktop -->
    <div class="row">
        <div class="col-12 col-md-8 col-lg-6">
            <SfTextArea Width="100%" RowCount="5"></SfTextArea>
        </div>
    </div>
</div>
```

### Custom Width with ResizeMode

```razor
<!-- Fixed width, vertical resize only -->
<SfTextArea Width="600px"
            ResizeMode="Resize.Vertical"
            RowCount="5">
</SfTextArea>

<!-- Full width, no resize -->
<SfTextArea Width="100%"
            ResizeMode="Resize.None"
            RowCount="4">
</SfTextArea>
```

## HTML Attributes

### InputAttributes

Apply attributes to the underlying `<textarea>` element:

```razor
<SfTextArea @bind-Value="@content"
            InputAttributes="@inputAttrs">
</SfTextArea>

@code {
    private string content = "";
    private Dictionary<string, object> inputAttrs = new Dictionary<string, object>
    {
        { "autocomplete", "off" },
        { "spellcheck", "true" },
        { "data-custom", "value" }
    };
}
```

### HtmlAttributes

Apply attributes to the outer wrapper element:

```razor
<SfTextArea @bind-Value="@text"
            HtmlAttributes="@htmlAttrs">
</SfTextArea>

@code {
    private string text = "";
    private Dictionary<string, object> htmlAttrs = new Dictionary<string, object>
    {
        { "id", "custom-textarea" },
        { "data-testid", "user-input" }
    };
}
```

### TabIndex

Control tab navigation order:

```razor
<SfTextArea TabIndex="1" Placeholder="First field"></SfTextArea>
<SfTextArea TabIndex="2" Placeholder="Second field"></SfTextArea>
<SfTextArea TabIndex="3" Placeholder="Third field"></SfTextArea>
```

## Complete Configuration Example

```razor
@page "/textarea-config"
@using Syncfusion.Blazor.Inputs

<div class="form-container">
    <h3>Advanced TextArea Configuration</h3>
    
    <!-- Full featured textarea -->
    <SfTextArea @bind-Value="@feedback"
                RowCount="8"
                ColumnCount="60"
                MaxLength="1000"
                ResizeMode="Resize.Vertical"
                FloatLabelType="FloatLabelType.Auto"
                Placeholder="Feedback"
                Width="100%"
                ShowClearButton="true"
                CssClass="custom-textarea"
                TabIndex="1">
    </SfTextArea>
    
    <div class="info-bar">
        <span class="char-count">
            @(feedback?.Length ?? 0) / 1000 characters
        </span>
        <span class="word-count">
            @GetWordCount() words
        </span>
    </div>
    
    <!-- Configuration controls -->
    <div class="controls mt-3">
        <label>
            <input type="checkbox" @bind="isReadOnly" />
            Read Only
        </label>
        
        <label>
            <input type="checkbox" @bind="isDisabled" />
            Disabled
        </label>
    </div>
    
    <!-- Preview the bound value -->
    <div class="preview mt-3">
        <strong>Current Value:</strong>
        <pre>@feedback</pre>
    </div>
</div>

@code {
    private string feedback = "";
    private bool isReadOnly = false;
    private bool isDisabled = false;
    
    private int GetWordCount()
    {
        if (string.IsNullOrWhiteSpace(feedback)) return 0;
        return feedback.Split(new[] { ' ', '\n', '\r' }, 
            StringSplitOptions.RemoveEmptyEntries).Length;
    }
}
```

## Best Practices

1. **Sizing**: Start with appropriate `RowCount` for the expected content length
2. **Limits**: Always set `MaxLength` for database-backed fields
3. **Resize**: Use `Resize.Vertical` for form consistency
4. **Width**: Set `Width="100%"` for responsive designs
5. **State**: Use `ReadOnly` for viewable content, `Disabled` for truly unavailable fields
6. **Accessibility**: Provide clear placeholders and labels
7. **Mobile**: Disable resize on mobile (`Resize.None`) for better UX
