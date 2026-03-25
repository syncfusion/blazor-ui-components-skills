# Error Handling and Troubleshooting

## Table of Contents
- [Error Types](#error-types)
- [Error Handling Patterns](#error-handling-patterns)
- [User Feedback Mechanisms](#user-feedback-mechanisms)
- [Recovery Strategies](#recovery-strategies)
- [Debugging](#debugging)

## Error Types

The SpeechToText component can encounter various errors during speech recognition. Understanding these helps you handle them appropriately.

### no-speech

**Cause:** The microphone did not detect any speech input.

**When it occurs:**
- User clicked but didn't speak
- Audio too quiet to detect
- Long silence without vocalization

**Error code:** `"no-speech"`

**Handling:**
```razor
private void OnSpeechError(SpeechRecognitionErrorEventArgs args)
{
    if (args.Error == "no-speech")
    {
        errorMessage = "No speech detected. Please speak clearly into your microphone.";
    }
}
```

### aborted

**Cause:** The speech recognition process was intentionally terminated.

**When it occurs:**
- User clicked the stop button
- Component unmounted during recording
- Another speech recognition started

**Error code:** `"aborted"`

**Handling:**
```razor
private void OnSpeechError(SpeechRecognitionErrorEventArgs args)
{
    if (args.Error == "aborted")
    {
        errorMessage = "Recording was cancelled.";
    }
}
```

### audio-capture

**Cause:** The system was unable to access a microphone device.

**When it occurs:**
- No microphone connected
- Microphone drivers missing
- System audio issues

**Error code:** `"audio-capture"`

**Handling:**
```razor
private void OnSpeechError(SpeechRecognitionErrorEventArgs args)
{
    if (args.Error == "audio-capture")
    {
        errorMessage = "No microphone found. Please check that your microphone is connected.";
    }
}
```

### not-allowed

**Cause:** Access to the microphone was denied by the user or browser settings.

**When it occurs:**
- User clicked "Deny" on permission prompt
- Microphone permission revoked in browser settings
- HTTPS required (HTTP not allowed)

**Error code:** `"not-allowed"`

**Handling:**
```razor
private void OnSpeechError(SpeechRecognitionErrorEventArgs args)
{
    if (args.Error == "not-allowed")
    {
        errorMessage = "Microphone access denied. Please allow microphone access in your browser settings.";
    }
}
```

### service-not-allowed

**Cause:** The current context does not permit the use of the speech recognition service.

**When it occurs:**
- Running on HTTP instead of HTTPS
- Private browsing mode restrictions
- Sandboxed context

**Error code:** `"service-not-allowed"`

**Handling:**
```razor
private void OnSpeechError(SpeechRecognitionErrorEventArgs args)
{
    if (args.Error == "service-not-allowed")
    {
        errorMessage = "Speech recognition service not available in this context. Ensure you're using HTTPS.";
    }
}
```

### network

**Cause:** A network issue is preventing the speech recognition service from functioning.

**When it occurs:**
- No internet connection
- Connection timeout
- Server unreachable
- DNS resolution failed

**Error code:** `"network"`

**Handling:**
```razor
private void OnSpeechError(SpeechRecognitionErrorEventArgs args)
{
    if (args.Error == "network")
    {
        errorMessage = "Network error. Please check your internet connection and try again.";
    }
}
```

### unsupported-browser

**Cause:** The browser being used does not support the SpeechRecognition API.

**When it occurs:**
- Using Firefox (not supported)
- Older browser versions
- IE or very old Edge versions

**Error code:** `"unsupported-browser"`

**Handling:**
```razor
private void OnSpeechError(SpeechRecognitionErrorEventArgs args)
{
    if (args.Error == "unsupported-browser")
    {
        errorMessage = "Your browser doesn't support speech recognition. Please use Chrome, Edge, Safari, or Opera.";
    }
}
```

### default

**Cause:** An unidentified error occurred during the speech recognition process.

**When it occurs:**
- Unknown system error
- Unexpected condition

**Error code:** `"default"`

**Handling:**
```razor
private void OnSpeechError(SpeechRecognitionErrorEventArgs args)
{
    if (args.Error == "default")
    {
        errorMessage = "An unexpected error occurred. Please try again.";
    }
}
```

## Error Handling Patterns

### Pattern 1: Simple Error Message Display

```razor
@using Syncfusion.Blazor.Inputs

<div style="max-width: 500px;">
    <SfSpeechToText 
        SpeechRecognitionError="@OnSpeechError"
        @bind-Transcript="@transcript">
    </SfSpeechToText>
    
    @if (!string.IsNullOrEmpty(errorMessage))
    {
        <div style="background: #ffebee; color: #c62828; padding: 12px; margin-top: 15px; border-radius: 4px; border-left: 4px solid #c62828;">
            <strong>⚠️ Error:</strong> @errorMessage
        </div>
    }
</div>

@code {
    private string transcript = "";
    private string errorMessage = "";
    
    private void OnSpeechError(SpeechRecognitionErrorEventArgs args)
    {
        errorMessage = args.Error switch
        {
            "no-speech" => "No speech detected. Please try again.",
            "audio-capture" => "No microphone found.",
            "not-allowed" => "Microphone access denied.",
            "network" => "Network error occurred.",
            "unsupported-browser" => "Your browser doesn't support voice input.",
            _ => $"Error: {args.Error}"
        };
    }
}
```

### Pattern 2: Comprehensive Error Handling with Details

```razor
@using Syncfusion.Blazor.Inputs

<div style="max-width: 600px;">
    <SfSpeechToText 
        SpeechRecognitionError="@OnSpeechError"
        @bind-Transcript="@transcript">
    </SfSpeechToText>
    
    @if (!string.IsNullOrEmpty(errorMessage))
    {
        <div style="background: #fff3e0; border: 1px solid #ffb74d; border-radius: 4px; padding: 15px; margin-top: 20px;">
            <div style="display: flex; gap: 10px; margin-bottom: 10px;">
                <span style="font-size: 20px;">⚠️</span>
                <div>
                    <strong style="color: #e65100;">@errorTitle</strong>
                    <p style="margin: 5px 0 0 0; color: #666;">@errorMessage</p>
                </div>
            </div>
            
            @if (!string.IsNullOrEmpty(errorSolution))
            {
                <div style="background: #e3f2fd; padding: 10px; border-radius: 4px; margin-top: 10px; font-size: 13px;">
                    <strong>💡 Solution:</strong>
                    <p style="margin: 5px 0 0 0;">@errorSolution</p>
                </div>
            }
            
            <button @onclick="ClearError" style="margin-top: 10px; padding: 8px 12px; background: #ff9800; color: white; border: none; border-radius: 4px; cursor: pointer;">
                Dismiss
            </button>
        </div>
    }
</div>

@code {
    private string transcript = "";
    private string errorMessage = "";
    private string errorTitle = "";
    private string errorSolution = "";
    
    private void OnSpeechError(SpeechRecognitionErrorEventArgs args)
    {
        (errorTitle, errorMessage, errorSolution) = args.Error switch
        {
            "no-speech" => (
                "No Speech Detected",
                "The microphone didn't pick up any speech.",
                "Make sure you're speaking clearly into your microphone and try again."
            ),
            "audio-capture" => (
                "Microphone Not Found",
                "The system couldn't detect a microphone.",
                "Check that your microphone is connected and working properly."
            ),
            "not-allowed" => (
                "Microphone Access Denied",
                "Browser permission for microphone was denied.",
                "Allow microphone access in your browser settings and try again."
            ),
            "network" => (
                "Network Error",
                "Connection issue with speech recognition service.",
                "Check your internet connection and try again."
            ),
            "unsupported-browser" => (
                "Browser Not Supported",
                "Your browser doesn't support speech recognition.",
                "Use Chrome, Edge, Safari, or Opera browser instead."
            ),
            _ => (
                "Unknown Error",
                $"An unexpected error occurred: {args.Error}",
                "Try refreshing the page or using a different browser."
            )
        };
    }
    
    private void ClearError()
    {
        errorMessage = "";
        errorTitle = "";
        errorSolution = "";
    }
}
```

## User Feedback Mechanisms

### Feedback with Retry Options

```razor
@using Syncfusion.Blazor.Inputs

<div style="max-width: 500px;">
    <SfSpeechToText 
        SpeechRecognitionError="@OnSpeechError"
        @bind-Transcript="@transcript">
    </SfSpeechToText>
    
    @if (!string.IsNullOrEmpty(errorMessage))
    {
        <div style="background: #ffebee; padding: 15px; border-radius: 4px; margin-top: 15px;">
            <p style="margin: 0 0 15px 0;">@errorMessage</p>
            
            <div style="display: flex; gap: 10px;">
                <button @onclick="RetryRecognition" style="padding: 8px 16px; background: #4CAF50; color: white; border: none; border-radius: 4px; cursor: pointer;">
                    🔄 Retry
                </button>
                <button @onclick="ClearError" style="padding: 8px 16px; background: #f44336; color: white; border: none; border-radius: 4px; cursor: pointer;">
                    ✕ Cancel
                </button>
            </div>
        </div>
    }
</div>

@code {
    private string transcript = "";
    private string errorMessage = "";
    private int retryCount = 0;
    
    private void OnSpeechError(SpeechRecognitionErrorEventArgs args)
    {
        retryCount++;
        
        errorMessage = args.Error switch
        {
            "no-speech" => "No speech detected. Please speak clearly.",
            "audio-capture" => "Microphone not detected. Check connections.",
            "not-allowed" => "Allow microphone access in browser settings.",
            _ => $"Error occurred: {args.Error}"
        };
        
        if (retryCount > 3)
        {
            errorMessage += " (Multiple attempts failed. Please check your system.)";
        }
    }
    
    private void RetryRecognition()
    {
        ClearError();
        // Component will retry on next click
    }
    
    private void ClearError()
    {
        errorMessage = "";
        retryCount = 0;
    }
}
```

## Recovery Strategies

### Graceful Degradation

```razor
@using Syncfusion.Blazor.Inputs

<div style="max-width: 500px;">
    @if (isBrowserSupported)
    {
        <SfSpeechToText 
            SpeechRecognitionError="@OnSpeechError"
            @bind-Transcript="@transcript">
        </SfSpeechToText>
    }
    else
    {
        <div style="background: #fce4ec; padding: 15px; border-radius: 4px; border-left: 4px solid #c2185b;">
            <p style="margin: 0; color: #880e4f;">
                <strong>Speech recognition not supported.</strong><br/>
                Please type your message instead or use a supported browser (Chrome, Edge, Safari).
            </p>
        </div>
    }
    
    <textarea 
        @bind="@transcript" 
        placeholder="Type or speak your message..."
        rows="4"
        style="width: 100%; padding: 10px; margin-top: 10px; border: 1px solid #ddd; border-radius: 4px;">
    </textarea>
</div>

@code {
    private string transcript = "";
    private bool isBrowserSupported = true;
    
    protected override void OnInitialized()
    {
        // Check browser support
        // In real implementation, use JavaScript interop
    }
    
    private void OnSpeechError(SpeechRecognitionErrorEventArgs args)
    {
        if (args.Error == "unsupported-browser")
        {
            isBrowserSupported = false;
        }
    }
}
```

### Fallback UI

```razor
@using Syncfusion.Blazor.Inputs

<div style="max-width: 500px;">
    <div style="display: flex; gap: 10px; align-items: center;">
        <SfSpeechToText 
            SpeechRecognitionError="@OnSpeechError"
            @bind-Transcript="@transcript">
        </SfSpeechToText>
        
        <span style="color: #999;">OR</span>
        
        <input 
            type="text" 
            @bind="@manualInput" 
            @onkeyup="@OnManualInput"
            placeholder="Type instead..." 
            style="flex: 1; padding: 8px; border: 1px solid #ddd; border-radius: 4px;" />
    </div>
    
    @if (!string.IsNullOrEmpty(errorMessage))
    {
        <p style="color: #f44336; font-size: 12px; margin-top: 10px;">⚠️ @errorMessage</p>
    }
</div>

@code {
    private string transcript = "";
    private string manualInput = "";
    private string errorMessage = "";
    
    private void OnManualInput(KeyboardEventArgs e)
    {
        transcript = manualInput;
    }
    
    private void OnSpeechError(SpeechRecognitionErrorEventArgs args)
    {
        errorMessage = "Voice input unavailable. Use text input instead.";
    }
}
```

## Debugging

### Logging Errors

```razor
@using Syncfusion.Blazor.Inputs

<div>
    <SfSpeechToText 
        SpeechRecognitionError="@OnSpeechError"
        @bind-Transcript="@transcript">
    </SfSpeechToText>
    
    @if (debugMode && errors.Any())
    {
        <div style="background: #263238; color: #aed581; padding: 15px; border-radius: 4px; margin-top: 20px; font-family: monospace; font-size: 12px;">
            <strong>Debug Log:</strong>
            <div>
                @foreach (var error in errors)
                {
                    <div>[@error.Timestamp.ToString("HH:mm:ss")] @error.ErrorCode - @error.Message</div>
                }
            </div>
        </div>
    }
</div>

@code {
    private string transcript = "";
    private bool debugMode = false;
    private List<ErrorLog> errors = new();
    
    public class ErrorLog
    {
        public DateTime Timestamp { get; set; }
        public string ErrorCode { get; set; }
        public string Message { get; set; }
    }
    
    private void OnSpeechError(SpeechRecognitionErrorEventArgs args)
    {
        errors.Add(new ErrorLog
        {
            Timestamp = DateTime.Now,
            ErrorCode = args.Error,
            Message = $"Speech recognition error: {args.Error}"
        });
    }
}
```

## Best Practices

1. **Always handle errors** - Never ignore SpeechRecognitionError events
2. **Provide clear feedback** - Users need to know what went wrong
3. **Offer solutions** - Include tips for fixing common errors
4. **Implement retry logic** - Allow users to try again
5. **Have a fallback** - Provide text input as alternative
6. **Log errors** - Track issues for debugging
7. **Test on target browsers** - Verify behavior in Chrome, Edge, Safari
8. **Check permissions** - Inform users about microphone access requirements
