# ProgressButton Component - Advanced Features

## Accessibility

### ARIA Support

ProgressButton includes:
- `aria-label` for progress state
- `aria-busy` during progress
- Screen reader announcements

```razor
<SfProgressButton Content="Submit" CssClass="accessible-progress"></SfProgressButton>

<style>
    .accessible-progress:focus-visible {
        outline: 2px solid #0078d4;
        outline-offset: 2px;
    }
</style>
```

## Keyboard Navigation

- **Space/Enter**: Trigger progress
- **Tab**: Move focus

## State Management

```razor
<SfProgressButton @ref="progressBtn" Content="@content" Duration="3000" EnableProgress="@isProcessing">
    <ProgressButtonEvents OnBegin="OnBegin" End="OnEnd" Fail="OnFail"></ProgressButtonEvents>
</SfProgressButton>

<p>Status: @status</p>

@code {
    private SfProgressButton progressBtn;
    private string content = "Process";
    private string status = "Ready";
    private bool isProcessing = false;

    private void OnBegin(ProgressEventArgs args)
    {
        isProcessing = true;
        status = "Processing...";
        content = "Please wait...";
    }

    private void OnEnd(ProgressEventArgs args)
    {
        isProcessing = false;
        status = "Completed";
        content = "Done!";
        
        Task.Delay(1500).ContinueWith(_ =>
        {
            InvokeAsync(() =>
            {
                content = "Process";
                status = "Ready";
                StateHasChanged();
            });
        });
    }

    private void OnFail(ProgressEventArgs args)
    {
        isProcessing = false;
        status = "Failed";
        content = "Retry";
    }
}
```

## Error Handling

```razor
<SfProgressButton Content="Submit" Duration="3000">
    <ProgressButtonEvents OnBegin="OnBegin" Progress="OnProgress" Fail="OnFail"></ProgressButtonEvents>
</SfProgressButton>

@code {
    private async Task OnBegin(ProgressEventArgs args)
    {
        try
        {
            // Simulate API call
            await SubmitDataAsync();
        }
        catch (Exception ex)
        {
            // Trigger fail event
            args.Cancel = true;
        }
    }

    private void OnProgress(ProgressEventArgs args)
    {
        if (args.Percent >= 50)
        {
            // Can cancel at midpoint if needed
            // args.Cancel = true;
        }
    }

    private void OnFail(ProgressEventArgs args)
    {
        // Handle failure
        Console.WriteLine("Operation failed");
    }

    private async Task SubmitDataAsync()
    {
        await Task.Delay(1000);
        // throw new Exception("Simulated error");
    }
}
```

## Async Operations

```razor
<SfProgressButton Content="Upload" Duration="5000">
    <ProgressButtonEvents OnBegin="OnUploadBegin" End="OnUploadEnd"></ProgressButtonEvents>
</SfProgressButton>

@code {
    private async Task OnUploadBegin(ProgressEventArgs args)
    {
        await UploadFileAsync();
    }

    private void OnUploadEnd(ProgressEventArgs args)
    {
        Console.WriteLine("Upload completed");
    }

    private async Task UploadFileAsync()
    {
        // Simulated upload
        await Task.Delay(3000);
    }
}
```

## Performance

Best practices:
1. Set appropriate duration based on operation
2. Provide visual feedback during progress
3. Handle errors gracefully
4. Disable button during progress
5. Reset state after completion

---

See also: [progress-button-features.md](progress-button-features.md) | [progress-button-styling.md](progress-button-styling.md)
