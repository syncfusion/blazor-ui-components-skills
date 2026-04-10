# TextArea - Events and Data Binding

Comprehensive guide to handling events and implementing data binding patterns with the Syncfusion Blazor TextArea component.

## Data Binding

### Two-Way Binding with @bind-Value

The most common pattern for binding textarea value:

```razor
<SfTextArea @bind-Value="@userInput"
            Placeholder="Type something...">
</SfTextArea>

<p>You typed: @userInput</p>

@code {
    private string userInput = "";
}
```

### One-Way Binding

For read-only scenarios or custom update logic:

```razor
<SfTextArea Value="@content"
            ValueChanged="@OnValueChanged">
</SfTextArea>

@code {
    private string content = "Initial content";
    
    private void OnValueChanged(string newValue)
    {
        content = newValue;
        // Custom logic here
    }
}
```

### ValueExpression for Validation

Required for form validation to work properly:

```razor
<EditForm Model="@model">
    <DataAnnotationsValidator />
    
    <SfTextArea @bind-Value="@model.Comments"
                ValueExpression="@(() => model.Comments)"
                Placeholder="Enter comments...">
    </SfTextArea>
    
    <ValidationMessage For="@(() => model.Comments)" />
</EditForm>

@code {
    private FormModel model = new FormModel();
    
    public class FormModel
    {
        [Required(ErrorMessage = "Comments are required")]
        [StringLength(500, ErrorMessage = "Comments cannot exceed 500 characters")]
        public string Comments { get; set; }
    }
}
```

## Value Change Events

### ValueChange Event

Fires when the textarea value changes (on blur by default):

```razor
<SfTextArea Value="@text"
            ValueChange="@OnValueChange">
</SfTextArea>

<p>Last change: @lastChangeTime</p>

@code {
    private string text = "";
    private DateTime? lastChangeTime;
    
    private void OnValueChange(TextAreaValueChangeEventArgs args)
    {
        text = args.Value;
        lastChangeTime = DateTime.Now;
        
        Console.WriteLine($"Previous: {args.PreviousValue}");
        Console.WriteLine($"Current: {args.Value}");
    }
}
```

**Event Arguments:**
- `Value`: Current textarea value
- `PreviousValue`: Previous textarea value
- `IsInteracted`: Boolean indicating user interaction

### Input Event (Real-Time Tracking)

Fires on every keystroke:

```razor
<SfTextArea Value="@liveText"
            Input="@OnInput"
            RowCount="5">
</SfTextArea>

<div class="live-info">
    <p>Characters: @(liveText?.Length ?? 0)</p>
    <p>Words: @GetWordCount()</p>
</div>

@code {
    private string liveText = "";
    
    private void OnInput(TextAreaInputEventArgs args)
    {
        liveText = args.Value;
        // Real-time processing
    }
    
    private int GetWordCount()
    {
        if (string.IsNullOrWhiteSpace(liveText)) return 0;
        return liveText.Split(new[] { ' ', '\n', '\r' }, 
            StringSplitOptions.RemoveEmptyEntries).Length;
    }
}
```

**Use Cases:**
- Live character/word counting
- Auto-save drafts
- Real-time validation
- Search-as-you-type functionality

### Combining ValueChange and Input

```razor
<SfTextArea @bind-Value="@content"
            Input="@OnInput"
            ValueChange="@OnValueChange"
            RowCount="6">
</SfTextArea>

<div class="status">
    <span>Last keystroke: @lastInput</span>
    <span>Last save: @lastSave</span>
</div>

@code {
    private string content = "";
    private DateTime? lastInput;
    private DateTime? lastSave;
    
    private void OnInput(TextAreaInputEventArgs args)
    {
        lastInput = DateTime.Now;
        // Triggered on every keystroke
    }
    
    private void OnValueChange(TextAreaValueChangeEventArgs args)
    {
        lastSave = DateTime.Now;
        // Triggered on blur or explicit change
    }
}
```

## Focus and Blur Events

### Focus Event

Triggered when textarea receives focus:

```razor
<SfTextArea @bind-Value="@text"
            Focus="@OnFocus"
            Placeholder="Click to focus...">
</SfTextArea>

<p class="@(isFocused ? "text-success" : "")">
    @(isFocused ? "Focused!" : "Not focused")
</p>

@code {
    private string text = "";
    private bool isFocused = false;
    
    private void OnFocus(TextAreaFocusInEventArgs args)
    {
        isFocused = true;
        Console.WriteLine($"Textarea focused at: {DateTime.Now}");
        Console.WriteLine($"Current value: {args.Value}");
    }
}
```

### Blur Event

Triggered when textarea loses focus:

```razor
<SfTextArea @bind-Value="@comment"
            Blur="@OnBlur"
            RowCount="4">
</SfTextArea>

<p class="@(hasBlurred ? "text-info" : "")">
    @(hasBlurred ? $"Last edited: {lastBlurTime:hh:mm:ss}" : "Not edited yet")
</p>

@code {
    private string comment = "";
    private bool hasBlurred = false;
    private DateTime lastBlurTime;
    
    private void OnBlur(TextAreaFocusOutEventArgs args)
    {
        hasBlurred = true;
        lastBlurTime = DateTime.Now;
        
        // Save to database, validate, etc.
        Console.WriteLine($"Final value: {args.Value}");
    }
}
```

### Focus Management Pattern

```razor
<SfTextArea @ref="textAreaRef"
            @bind-Value="@content"
            Focus="@OnFocus"
            Blur="@OnBlur"
            RowCount="5">
</SfTextArea>

<button @onclick="FocusTextArea">Focus TextArea</button>

<div class="focus-state">
    Status: @(isFocused ? "Editing" : "Idle")
</div>

@code {
    private SfTextArea textAreaRef;
    private string content = "";
    private bool isFocused = false;
    
    private void OnFocus(TextAreaFocusInEventArgs args)
    {
        isFocused = true;
    }
    
    private void OnBlur(TextAreaFocusOutEventArgs args)
    {
        isFocused = false;
    }
    
    private async Task FocusTextArea()
    {
        await textAreaRef.FocusAsync();
    }
}
```

## Lifecycle Events

### Created Event

Fires when the component is initialized:

```razor
<SfTextArea @bind-Value="@text"
            Created="@OnCreated">
</SfTextArea>

@code {
    private string text = "";
    
    private void OnCreated()
    {
        Console.WriteLine("TextArea component created");
        // Initialize default values, load data, etc.
    }
}
```

### Destroyed Event

Fires when the component is removed from the DOM:

```razor
<SfTextArea @bind-Value="@text"
            Destroyed="@OnDestroyed">
</SfTextArea>

@code {
    private string text = "";
    
    private void OnDestroyed()
    {
        Console.WriteLine("TextArea component destroyed");
        // Cleanup, save state, etc.
    }
}
```

## Form Validation Integration

### With EditForm and DataAnnotations

```razor
<EditForm Model="@feedbackModel" OnValidSubmit="@HandleSubmit">
    <DataAnnotationsValidator />
    
    <div class="form-group">
        <label>Name:</label>
        <InputText @bind-Value="feedbackModel.Name" class="form-control" />
        <ValidationMessage For="@(() => feedbackModel.Name)" />
    </div>
    
    <div class="form-group">
        <label>Feedback:</label>
        <SfTextArea @bind-Value="feedbackModel.Feedback"
                    ValueExpression="@(() => feedbackModel.Feedback)"
                    RowCount="6"
                    MaxLength="1000"
                    Placeholder="Share your thoughts...">
        </SfTextArea>
        <ValidationMessage For="@(() => feedbackModel.Feedback)" />
    </div>
    
    <button type="submit" class="btn btn-primary">Submit</button>
</EditForm>

@code {
    private FeedbackModel feedbackModel = new FeedbackModel();
    
    private void HandleSubmit()
    {
        Console.WriteLine($"Submitted: {feedbackModel.Feedback}");
        // Process the feedback
    }
    
    public class FeedbackModel
    {
        [Required(ErrorMessage = "Name is required")]
        public string Name { get; set; }
        
        [Required(ErrorMessage = "Feedback is required")]
        [StringLength(1000, MinimumLength = 10, 
            ErrorMessage = "Feedback must be between 10 and 1000 characters")]
        public string Feedback { get; set; }
    }
}
```

### Custom Validation

```razor
<SfTextArea @bind-Value="@email"
            ValueChange="@ValidateEmail"
            RowCount="3">
</SfTextArea>

<div class="@(isValid ? "text-success" : "text-danger")">
    @validationMessage
</div>

@code {
    private string email = "";
    private bool isValid = true;
    private string validationMessage = "";
    
    private void ValidateEmail(TextAreaValueChangeEventArgs args)
    {
        email = args.Value;
        
        if (string.IsNullOrWhiteSpace(email))
        {
            isValid = false;
            validationMessage = "Email list cannot be empty";
            return;
        }
        
        var emails = email.Split('\n', StringSplitOptions.RemoveEmptyEntries);
        var invalidEmails = emails.Where(e => !e.Contains("@")).ToList();
        
        if (invalidEmails.Any())
        {
            isValid = false;
            validationMessage = $"Invalid emails: {string.Join(", ", invalidEmails)}";
        }
        else
        {
            isValid = true;
            validationMessage = $"✓ {emails.Length} valid email(s)";
        }
    }
}
```

## Advanced Event Patterns

### Auto-Save with Debounce

```razor
<SfTextArea @bind-Value="@draft"
            Input="@OnInputDebounced"
            RowCount="8"
            Placeholder="Type to auto-save...">
</SfTextArea>

<p>@saveStatus</p>

@code {
    private string draft = "";
    private string saveStatus = "";
    private System.Threading.Timer debounceTimer;
    
    private void OnInputDebounced(TextAreaInputEventArgs args)
    {
        draft = args.Value;
        
        // Cancel previous timer
        debounceTimer?.Dispose();
        
        // Set new timer (save after 2 seconds of inactivity)
        debounceTimer = new System.Threading.Timer(async _ =>
        {
            await InvokeAsync(async () =>
            {
                await SaveDraft();
            });
        }, null, 2000, Timeout.Infinite);
        
        saveStatus = "Typing...";
    }
    
    private async Task SaveDraft()
    {
        // Simulate save operation
        await Task.Delay(500);
        saveStatus = $"Saved at {DateTime.Now:HH:mm:ss}";
        StateHasChanged();
    }
    
    public void Dispose()
    {
        debounceTimer?.Dispose();
    }
}
```

### Character Limit Warning

```razor
<SfTextArea @bind-Value="@message"
            Input="@OnInput"
            MaxLength="200"
            RowCount="5">
</SfTextArea>

<div class="@GetWarningClass()">
    @GetCharacterMessage()
</div>

@code {
    private string message = "";
    private int maxLength = 200;
    
    private void OnInput(TextAreaInputEventArgs args)
    {
        message = args.Value;
    }
    
    private int RemainingChars => maxLength - (message?.Length ?? 0);
    
    private string GetCharacterMessage()
    {
        return RemainingChars switch
        {
            <= 0 => "Character limit reached!",
            <= 20 => $"Only {RemainingChars} characters remaining",
            _ => $"{message?.Length ?? 0} / {maxLength} characters"
        };
    }
    
    private string GetWarningClass()
    {
        return RemainingChars switch
        {
            <= 0 => "text-danger fw-bold",
            <= 20 => "text-warning",
            _ => "text-muted"
        };
    }
}
```

### Multi-Field Synchronization

```razor
<div class="row">
    <div class="col-md-6">
        <h5>Editor</h5>
        <SfTextArea @bind-Value="@sharedContent"
                    ValueChange="@OnEditorChange"
                    RowCount="8">
        </SfTextArea>
    </div>
    
    <div class="col-md-6">
        <h5>Preview</h5>
        <div class="preview-box">
            @((MarkupString)FormatPreview(sharedContent))
        </div>
    </div>
</div>

@code {
    private string sharedContent = "";
    
    private void OnEditorChange(TextAreaValueChangeEventArgs args)
    {
        sharedContent = args.Value;
        // Trigger preview update
    }
    
    private string FormatPreview(string content)
    {
        if (string.IsNullOrEmpty(content)) return "<em>Nothing to preview</em>";
        
        // Simple markdown-like formatting
        return content
            .Replace("\n", "<br/>")
            .Replace("**", "<strong>")
            .Replace("*", "<em>");
    }
}
```

## Best Practices

1. **Use @bind-Value**: For simple two-way binding scenarios
2. **ValueExpression**: Always include for form validation
3. **Input vs ValueChange**: Use `Input` for real-time, `ValueChange` for save operations
4. **Debouncing**: Implement for auto-save to avoid excessive saves
5. **Focus Management**: Use `Focus`/`Blur` for UX enhancements
6. **Validation**: Combine with EditForm for robust validation
7. **Performance**: Avoid heavy operations in `Input` event (use debouncing)
8. **Cleanup**: Dispose timers and subscriptions properly
