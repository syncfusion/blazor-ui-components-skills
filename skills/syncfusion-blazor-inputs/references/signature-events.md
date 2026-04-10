# Signature - Event Handling

Comprehensive guide to handling events in the Syncfusion Blazor Signature component for tracking changes, validating signatures, and implementing event-driven workflows.

## Changed Event

Fires when the signature changes (stroke added, cleared, or loaded):

```razor
<SfSignature Changed="@OnSignatureChanged"></SfSignature>

@code {
    private void OnSignatureChanged(SignatureChangeEventArgs args)
    {
        Console.WriteLine($"Action Type: {args.ActionName}");
    }
}
```

### Event Arguments

**SignatureChangeEventArgs Properties:**
- `ActionName`: Type of action ("draw", "clear", "load", "undo", "redo")

### Tracking Signature State

```razor
<SfSignature @ref="signatureRef"
             Changed="@OnSignatureChanged">
</SfSignature>

<div class="signature-status">
    <span class="badge bg-@(hasSignature ? "success" : "secondary")">
        @(hasSignature ? "Signature Present" : "No Signature")
    </span>
    
    <p>Strokes: @strokeCount</p>
</div>

@code {
    private SfSignature signatureRef;
    private bool hasSignature = false;
    private int strokeCount = 0;
    
    private void OnSignatureChanged(SignatureChangeEventArgs args)
    {
        strokeCount++;
        
        Console.WriteLine($"Action: {args.ActionName}");
    }
}
```

### Real-Time Validation

```razor
<SfSignature Changed="@ValidateSignature"></SfSignature>

@if (!isValid)
{
    <div class="alert alert-warning">
        @validationMessage
    </div>
}

@code {
    private bool isValid = true;
    private string validationMessage = "";
    
    private void ValidateSignature(SignatureChangeEventArgs args)
    {
        Console.WriteLine($"Action: {args.ActionName}");
    }
}
```

## OnSave Event

Fires when the signature is saved using the SaveAsync method:

```razor
<SfSignature @ref="signatureRef"
             OnSave="@OnSignatureSave">
</SfSignature>

<button @onclick="SaveSignature">Save</button>

@code {
    private SfSignature signatureRef;
    
    private void OnSignatureSave(SignatureSaveEventArgs args)
    {
        Console.WriteLine($"Saved as: {args.FileName}");
    }
    
    private async Task SaveSignature()
    {
        await signatureRef.SaveAsync();
        // OnSave event will fire after save completes
    }
}
```

### Event Arguments

**SignatureSaveEventArgs Properties:**
- `FileName`: Name of the saved file

### Save with Callback

```razor
<SfSignature @ref="signatureRef"
             OnSave="@HandleSaveComplete">
</SfSignature>

<button @onclick="SaveSignature">Save Signature</button>

@if (saveSuccess)
{
    <div class="alert alert-success">
        Signature saved successfully as @savedFileName
    </div>
}

@code {
    private SfSignature signatureRef;
    private bool saveSuccess = false;
    private string savedFileName = "";
    
    private void HandleSaveComplete(SignatureSaveEventArgs args)
    {
        savedFileName = args.FileName;
        saveSuccess = true;
        
        
        // Auto-hide success message after 3 seconds
        _ = Task.Delay(3000).ContinueWith(_ => 
        {
            saveSuccess = false;
            InvokeAsync(StateHasChanged);
        });
    }
    
    private async Task SaveSignature()
    {
        await signatureRef.SaveAsync();
    }
    
}
```

## Created Event

Fires when the component is fully initialized:

```razor
<SfSignature Created="@OnSignatureCreated"></SfSignature>

@code {
    private void OnSignatureCreated()
    {
        Console.WriteLine("Signature component initialized");
        
        // Load saved signature if exists
        _ = LoadPreviousSignature();
    }
    
    private async Task LoadPreviousSignature()
    {
        // Load from storage
    }
}
```

### Initialization with Pre-loaded Data

```razor
@inject ISignatureService SignatureService

<SfSignature @ref="signatureRef"
             Created="@OnCreated">
</SfSignature>

<p class="text-muted">@initStatus</p>

@code {
    private SfSignature signatureRef;
    private string initStatus = "Initializing...";
    
    private async void OnCreated()
    {
        initStatus = "Loading previous signature...";
        
        // Fetch signature from service
        var existingSignature = await SignatureService.GetUserSignature();
        
        if (!string.IsNullOrEmpty(existingSignature))
        {
            await signatureRef.LoadAsync(existingSignature);
            initStatus = "Previous signature loaded";
        }
        else
        {
            initStatus = "Ready to sign";
        }
        
        StateHasChanged();
    }
}
```

## Event-Driven Workflows

### Form Validation Integration

```razor
<EditForm Model="@model" OnValidSubmit="@HandleSubmit">
    <DataAnnotationsValidator />
    
    <div class="form-group">
        <label>Name:</label>
        <InputText @bind-Value="model.Name" class="form-control" />
        <ValidationMessage For="@(() => model.Name)" />
    </div>
    
    <div class="form-group">
        <label>Signature: <span class="text-danger">*</span></label>
        <SfSignature @ref="signatureRef"
                     Changed="@OnSignatureChanged">
        </SfSignature>
        
        @if (showSignatureError)
        {
            <div class="text-danger small mt-1">Signature is required</div>
        }
    </div>
    
    <button type="submit" class="btn btn-primary">Submit</button>
</EditForm>

@code {
    private SfSignature signatureRef;
    private FormModel model = new FormModel();
    private bool hasSignature = false;
    private bool showSignatureError = false;
    
    private void OnSignatureChanged(SignatureChangeEventArgs args)
    {
        Console.WriteLine($"Action: {args.ActionName}");
        showSignatureError = false;
    }
    
    private async Task HandleSubmit()
    {
        if (!hasSignature)
        {
            showSignatureError = true;
            return;
        }
        
        // Get signature data
        string signatureData = await signatureRef.GetSignatureAsync(SignatureFileType.Png);
        model.SignatureBase64 = signatureData;
        
        // Submit form
        await SubmitForm(model);
    }
    
    private async Task SubmitForm(FormModel formData)
    {
        // Your submission logic
        await Task.CompletedTask;
    }
    
    public class FormModel
    {
        [Required]
        public string Name { get; set; }
        
        public string SignatureBase64 { get; set; }
    }
}
```

### Multi-Signature Tracking

```razor
<div class="multi-signature-form">
    <h4>Document Approval</h4>
    
    <div class="signature-section">
        <label>Manager Signature:</label>
        <SfSignature @ref="managerSigRef"
                     Changed="@((args) => OnSignatureChanged("manager", args))">
        </SfSignature>
        <span class="badge bg-@(signatures["manager"] ? "success" : "warning")">
            @(signatures["manager"] ? "✓ Signed" : "Pending")
        </span>
    </div>
    
    <div class="signature-section">
        <label>Director Signature:</label>
        <SfSignature @ref="directorSigRef"
                     Changed="@((args) => OnSignatureChanged("director", args))">
        </SfSignature>
        <span class="badge bg-@(signatures["director"] ? "success" : "warning")">
            @(signatures["director"] ? "✓ Signed" : "Pending")
        </span>
    </div>
    
    <button class="btn btn-primary" 
            @onclick="SubmitDocument"
            disabled="@(!AllSignaturesComplete())">
        Submit Document
    </button>
    
    <p class="text-muted">@GetCompletionStatus()</p>
</div>

@code {
    private SfSignature managerSigRef;
    private SfSignature directorSigRef;
    
    private Dictionary<string, bool> signatures = new Dictionary<string, bool>
    {
        { "manager", false },
        { "director", false }
    };
    
    private void OnSignatureChanged(string role, SignatureChangeEventArgs args)
    {
       Console.WriteLine($"Action: {args.ActionName}");
    }
    
    private bool AllSignaturesComplete()
    {
        return signatures.All(s => s.Value);
    }
    
    private string GetCompletionStatus()
    {
        int completed = signatures.Count(s => s.Value);
        int total = signatures.Count;
        return $"{completed} of {total} signatures complete";
    }
    
    private async Task SubmitDocument()
    {
        var managerSig = await managerSigRef.GetSignatureAsync(SignatureFileType.Png);
        var directorSig = await directorSigRef.GetSignatureAsync(SignatureFileType.Png);
        
        // Submit to server
        await SubmitApprovedDocument(managerSig, directorSig);
    }
    
    private async Task SubmitApprovedDocument(string managerSig, string directorSig)
    {
        // Your submission logic
        await Task.CompletedTask;
    }
}
```

### Audit Trail with Events

```razor
<SfSignature @ref="signatureRef"
             Changed="@LogChange"
             OnSave="@LogSave"
             Created="@LogCreation">
</SfSignature>

<div class="audit-trail">
    <h5>Activity Log:</h5>
    <ul>
        @foreach (var entry in auditLog)
        {
            <li>@entry</li>
        }
    </ul>
</div>

@code {
    private SfSignature signatureRef;
    private List<string> auditLog = new List<string>();
    
    private void LogCreation()
    {
        AddAuditEntry("Signature component initialized");
    }
    
    private void LogChange(SignatureChangeEventArgs args)
    {
        string action = args.ActionName;
        AddAuditEntry($"Signature {action} at {DateTime.Now:HH:mm:ss}");
    }
    
    private void LogSave(SignatureSaveEventArgs args)
    {
       //logic
    }
    
    private void AddAuditEntry(string message)
    {
        auditLog.Insert(0, $"[{DateTime.Now:yyyy-MM-dd HH:mm:ss}] {message}");
        
        // Keep only last 10 entries
        if (auditLog.Count > 10)
        {
            auditLog = auditLog.Take(10).ToList();
        }
    }
}
```

## Event Combination Patterns

### Enable Submit Only When Signed

```razor
<SfSignature Changed="@OnSignatureChanged"></SfSignature>

<button @onclick="Submit" disabled="@(!canSubmit)">
    Submit
</button>

@code {
    private bool canSubmit = false;
    
    private void OnSignatureChanged(SignatureChangeEventArgs args)
    {
        //Logic
    }
    
    private async Task Submit()
    {
        // Submit logic
    }
}
```

### Show Warning on Exit

```razor
@implements IDisposable

<SfSignature Changed="@OnSignatureChanged"></SfSignature>

@code {
    private bool hasUnsavedChanges = false;
    
    private void OnSignatureChanged(SignatureChangeEventArgs args)
    {
        Console.WriteLine($"Action: {args.ActionName}");
    }
    
    public void Dispose()
    {
        // Cleanup event listeners
    }
}
```

## Best Practices

1. **Changed Event**: Use for real-time validation and UI updates
2. **OnSave Event**: Use for logging, analytics, and success feedback
3. **Created Event**: Use for loading existing signatures or initialization
4. **Debouncing**: Implement debouncing for auto-save to avoid excessive operations
5. **Validation**: Validate signature presence before form submission
6. **State Management**: Track signature state for enabling/disabling buttons
7. **Error Handling**: Always wrap event handlers in try-catch blocks
8. **Performance**: Avoid heavy operations in frequently-fired events (Changed)
9. **Audit Trail**: Log important events for compliance and troubleshooting
10. **User Feedback**: Provide immediate visual feedback in event handlers

## Troubleshooting

**Events not firing:**
- Verify event handler method signature matches expected args
- Check for JavaScript console errors
- Ensure component is fully initialized before interaction

**Changed event fires too frequently:**
- Implement debouncing for auto-save scenarios
- Use OnSave event for less frequent updates
- Consider batching multiple changes

**Cannot access component methods in events:**
- Use @ref to get component reference
- Ensure component is initialized (use Created event)
- Check for null reference before calling methods
