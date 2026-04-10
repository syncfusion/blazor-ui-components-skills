# TextArea - Customization and Styling

Learn how to customize the appearance and behavior of the Syncfusion Blazor TextArea component with CSS classes, themes, accessibility features, and advanced styling techniques.

## CssClass Property

Apply custom CSS classes to the textarea wrapper:

```razor
<SfTextArea @bind-Value="@text"
            CssClass="custom-textarea bordered-textarea"
            RowCount="5">
</SfTextArea>

<style>
    .custom-textarea textarea {
        border: 2px solid #4CAF50;
        border-radius: 8px;
        font-family: 'Courier New', monospace;
    }
    
    .bordered-textarea textarea:focus {
        border-color: #2196F3;
        box-shadow: 0 0 8px rgba(33, 150, 243, 0.3);
    }
</style>

@code {
    private string text = "";
}
```

## ShowClearButton

Add a clear button for quick text removal:

```razor
<SfTextArea @bind-Value="@content"
            ShowClearButton="true"
            Placeholder="Type and use × to clear..."
            RowCount="4">
</SfTextArea>

@code {
    private string content = "";
}
```

**When to Use:**
- Quick draft clearing
- Form reset functionality
- User convenience for short inputs

## Theme Customization

### Using Built-in Themes

Switch between Syncfusion themes in your app:

```html
<!-- Bootstrap 5 Theme -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Material Theme -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Fluent Theme -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />

<!-- Tailwind Theme -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />
```

### Dark Mode Support

Use dark theme variants:

```html
<!-- Material Dark -->
<link href="_content/Syncfusion.Blazor.Themes/material-dark.css" rel="stylesheet" />

<!-- Bootstrap Dark -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5-dark.css" rel="stylesheet" />

<!-- Fluent Dark -->
<link href="_content/Syncfusion.Blazor.Themes/fluent-dark.css" rel="stylesheet" />
```

### Theme Studio Integration

Create custom themes at: https://blazor.syncfusion.com/themestudio/

1. Select base theme
2. Customize colors, fonts, spacing
3. Export custom CSS
4. Include in your project

## Custom Styling Examples

### Rounded Corners with Shadow

```razor
<SfTextArea @bind-Value="@comment"
            CssClass="rounded-shadow"
            RowCount="5"
            Placeholder="Enter comment...">
</SfTextArea>

<style>
    .rounded-shadow textarea {
        border-radius: 12px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
        padding: 12px;
    }
    
    .rounded-shadow textarea:focus {
        box-shadow: 0 4px 16px rgba(0, 123, 255, 0.2);
        outline: none;
    }
</style>

@code {
    private string comment = "";
}
```

### Monospace Code Editor Style

```razor
<SfTextArea @bind-Value="@code"
            CssClass="code-editor"
            RowCount="10"
            Placeholder="// Enter code here..."
            ResizeMode="Resize.Vertical">
</SfTextArea>

<style>
    .code-editor textarea {
        font-family: 'Consolas', 'Monaco', 'Courier New', monospace;
        font-size: 14px;
        line-height: 1.5;
        background-color: #1e1e1e;
        color: #d4d4d4;
        border: 1px solid #3c3c3c;
        padding: 16px;
    }
    
    .code-editor textarea::placeholder {
        color: #858585;
    }
</style>

@code {
    private string code = "";
}
```

### Success/Error State Styling

```razor
<SfTextArea @bind-Value="@input"
            CssClass="@GetValidationClass()"
            ValueChange="@ValidateInput"
            RowCount="4">
</SfTextArea>

<style>
    .textarea-success textarea {
        border: 2px solid #28a745;
        background-color: #f0fff4;
    }
    
    .textarea-error textarea {
        border: 2px solid #dc3545;
        background-color: #fff5f5;
    }
    
    .textarea-warning textarea {
        border: 2px solid #ffc107;
        background-color: #fffbf0;
    }
</style>

@code {
    private string input = "";
    private string validationState = "";
    
    private void ValidateInput(TextAreaValueChangeEventArgs args)
    {
        input = args.Value;
        
        if (string.IsNullOrEmpty(input))
            validationState = "";
        else if (input.Length < 10)
            validationState = "warning";
        else if (input.Length >= 10 && input.Length <= 100)
            validationState = "success";
        else
            validationState = "error";
    }
    
    private string GetValidationClass()
    {
        return validationState switch
        {
            "success" => "textarea-success",
            "error" => "textarea-error",
            "warning" => "textarea-warning",
            _ => ""
        };
    }
}
```

## HTML Attributes Customization

### InputAttributes

Customize the `<textarea>` element:

```razor
<SfTextArea @bind-Value="@text"
            InputAttributes="@inputAttrs"
            RowCount="5">
</SfTextArea>

@code {
    private string text = "";
    private Dictionary<string, object> inputAttrs = new Dictionary<string, object>
    {
        { "autocomplete", "off" },
        { "spellcheck", "true" },
        { "autocorrect", "on" },
        { "autocapitalize", "sentences" },
        { "data-gramm", "false" }, // Disable Grammarly
        { "data-testid", "main-textarea" }
    };
}
```

### HtmlAttributes

Customize the wrapper element:

```razor
<SfTextArea @bind-Value="@content"
            HtmlAttributes="@htmlAttrs"
            RowCount="6">
</SfTextArea>

@code {
    private string content = "";
    private Dictionary<string, object> htmlAttrs = new Dictionary<string, object>
    {
        { "id", "custom-textarea-wrapper" },
        { "data-section", "comments" },
        { "role", "region" },
        { "aria-label", "Comment input area" }
    };
}
```

## Accessibility Features

### ARIA Attributes

Enhance screen reader support:

```razor
<label id="textarea-label">Your Feedback</label>
<SfTextArea @bind-Value="@feedback"
            InputAttributes="@ariaAttrs"
            RowCount="5">
</SfTextArea>

@code {
    private string feedback = "";
    private Dictionary<string, object> ariaAttrs = new Dictionary<string, object>
    {
        { "aria-labelledby", "textarea-label" },
        { "aria-describedby", "textarea-description" },
        { "aria-required", "true" }
    };
}

<small id="textarea-description">Please provide detailed feedback</small>
```

### Keyboard Navigation

TextArea supports standard keyboard interactions:
- **Tab**: Move to next field
- **Shift+Tab**: Move to previous field
- **Ctrl+A**: Select all text
- **Ctrl+C/V**: Copy/paste

### TabIndex Configuration

Control focus order:

```razor
<SfTextArea TabIndex="1" Placeholder="First field"></SfTextArea>
<SfTextArea TabIndex="2" Placeholder="Second field"></SfTextArea>
<button tabindex="3">Submit</button>
```

## RTL (Right-to-Left) Support

Enable RTL for Arabic, Hebrew, etc.:

```razor
<SfTextArea @bind-Value="@arabicText"
            EnableRtl="true"
            Placeholder="أدخل النص هنا..."
            RowCount="5">
</SfTextArea>

@code {
    private string arabicText = "";
}
```

## Responsive Design Patterns

### Mobile-Optimized TextArea

```razor
<SfTextArea @bind-Value="@mobileText"
            CssClass="mobile-textarea"
            RowCount="@(IsMobile() ? 4 : 6)"
            ResizeMode="@(IsMobile() ? Resize.None : Resize.Vertical)"
            Width="100%">
</SfTextArea>

<style>
    .mobile-textarea textarea {
        font-size: 16px; /* Prevents zoom on iOS */
    }
    
    @media (max-width: 768px) {
        .mobile-textarea textarea {
            padding: 12px;
            min-height: 120px;
        }
    }
</style>

@code {
    private string mobileText = "";
    
    private bool IsMobile()
    {
        // Simplified mobile detection
        return false; // Implement based on your app's logic
    }
}
```

### Container-Based Responsive Width

```razor
<div class="container">
    <div class="row">
        <div class="col-12 col-md-8 col-lg-6">
            <SfTextArea @bind-Value="@text"
                        Width="100%"
                        RowCount="5">
            </SfTextArea>
        </div>
    </div>
</div>

@code {
    private string text = "";
}
```

## Advanced Customization Examples

### Character Counter with Progress Bar

```razor
<div class="textarea-with-counter">
    <SfTextArea @bind-Value="@text"
                Input="@OnInput"
                MaxLength="500"
                RowCount="6"
                CssClass="counted-textarea">
    </SfTextArea>
    
    <div class="counter-bar">
        <div class="progress">
            <div class="progress-bar @GetProgressClass()" 
                 style="width: @GetProgressPercentage()%">
            </div>
        </div>
        <span class="counter-text">@(text?.Length ?? 0) / 500</span>
    </div>
</div>

<style>
    .textarea-with-counter {
        position: relative;
    }
    
    .counter-bar {
        margin-top: 8px;
        display: flex;
        align-items: center;
        gap: 10px;
    }
    
    .progress {
        flex: 1;
        height: 6px;
        background-color: #e9ecef;
        border-radius: 3px;
        overflow: hidden;
    }
    
    .progress-bar {
        height: 100%;
        transition: width 0.3s ease, background-color 0.3s ease;
    }
    
    .bg-success { background-color: #28a745; }
    .bg-warning { background-color: #ffc107; }
    .bg-danger { background-color: #dc3545; }
    
    .counter-text {
        font-size: 0.875rem;
        color: #6c757d;
        white-space: nowrap;
    }
</style>

@code {
    private string text = "";
    
    private void OnInput(TextAreaInputEventArgs args)
    {
        text = args.Value;
    }
    
    private double GetProgressPercentage()
    {
        return ((text?.Length ?? 0) / 500.0) * 100;
    }
    
    private string GetProgressClass()
    {
        var percentage = GetProgressPercentage();
        if (percentage < 70) return "bg-success";
        if (percentage < 90) return "bg-warning";
        return "bg-danger";
    }
}
```

### Toolbar Integration

```razor
<div class="textarea-with-toolbar">
    <div class="toolbar">
        <button @onclick="@(() => ApplyFormat("**", "**"))">Bold</button>
        <button @onclick="@(() => ApplyFormat("*", "*"))">Italic</button>
        <button @onclick="@(() => ApplyFormat("`", "`"))">Code</button>
        <button @onclick="ClearText">Clear</button>
    </div>
    
    <SfTextArea @ref="textAreaRef"
                @bind-Value="@richText"
                RowCount="8"
                CssClass="toolbar-textarea">
    </SfTextArea>
</div>

<style>
    .toolbar {
        background: #f8f9fa;
        padding: 8px;
        border: 1px solid #dee2e6;
        border-bottom: none;
        display: flex;
        gap: 4px;
    }
    
    .toolbar button {
        padding: 4px 12px;
        border: 1px solid #dee2e6;
        background: white;
        cursor: pointer;
        border-radius: 4px;
    }
    
    .toolbar button:hover {
        background: #e9ecef;
    }
</style>

@code {
    private SfTextArea textAreaRef;
    private string richText = "";
    
    private void ApplyFormat(string prefix, string suffix)
    {
        richText += prefix + "text" + suffix;
    }
    
    private void ClearText()
    {
        richText = "";
    }
}
```

## Theme Studio Customization

### Creating Custom Theme

1. Visit https://blazor.syncfusion.com/themestudio/
2. Select base theme (Bootstrap, Material, etc.)
3. Customize:
   - Primary colors
   - Font families
   - Border radius
   - Spacing
4. Preview changes
5. Export CSS file
6. Include in your project

### Applying Custom Theme

```html
<head>
    <!-- Custom Syncfusion theme -->
    <link href="css/custom-syncfusion-theme.css" rel="stylesheet" />
</head>
```

## Best Practices

1. **Consistent Styling**: Use CssClass for reusable styles across components
2. **Accessibility**: Always provide proper ARIA labels and descriptions
3. **Mobile-First**: Set font-size to 16px+ to prevent zoom on mobile
4. **Contrast**: Ensure sufficient color contrast (WCAG 2.1 AA: 4.5:1)
5. **Focus Indicators**: Maintain visible focus states for keyboard navigation
6. **Responsive**: Test on multiple screen sizes and devices
7. **Performance**: Avoid complex CSS selectors that impact rendering
8. **Theme Consistency**: Use Theme Studio for cohesive design across all Syncfusion components

## Troubleshooting

**Custom styles not applying:**
- Ensure CSS is loaded after Syncfusion theme
- Use more specific selectors or `!important` if needed
- Check browser dev tools for CSS conflicts

**Focus state not visible:**
- Don't remove outline without providing alternative focus indicator
- Test with keyboard navigation

**Mobile zoom on focus:**
- Set textarea font-size to at least 16px on mobile devices
