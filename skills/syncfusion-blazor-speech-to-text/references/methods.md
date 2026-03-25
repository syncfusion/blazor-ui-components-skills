# Public Methods

## Overview

The SpeechToText component provides two public methods to control speech recognition functionality.

---

## 📋 Public Methods

### 1. StartListeningAsync()

**Signature:**
```csharp
public Task StartListeningAsync()
```

**Description:** Starts the speech recognition process. Once called, the component begins listening for audio input from the user's microphone.

**Returns:** `Task` - A task that completes when the listening has started

**Usage:**
```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText @ref="speechComponent"></SfSpeechToText>

<button @onclick="StartListening">Start Listening</button>

@code {
    private SfSpeechToText speechComponent;
    
    private async Task StartListening()
    {
        await speechComponent.StartListeningAsync();
    }
}
```

**Example with Button Integration:**
```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText @ref="speechComponent" @bind-Transcript="@transcript"></SfSpeechToText>

<button @onclick="async () => await speechComponent.StartListeningAsync()" 
        style="padding: 10px 20px; background: #007bff; color: white; border: none; border-radius: 4px; cursor: pointer;">
    🎤 Start Listening
</button>

<p>@transcript</p>

@code {
    private SfSpeechToText speechComponent;
    private string transcript = "";
}
```

**Exception Handling:**
```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText @ref="speechComponent"></SfSpeechToText>

<button @onclick="SafeStartListening">Start with Error Handling</button>

@code {
    private SfSpeechToText speechComponent;
    private string errorMessage = "";
    
    private async Task SafeStartListening()
    {
        try
        {
            await speechComponent.StartListeningAsync();
            errorMessage = "";
        }
        catch (Exception ex)
        {
            errorMessage = $"Failed to start listening: {ex.Message}";
        }
    }
}
```

---

### 2. StopListeningAsync()

**Signature:**
```csharp
public Task StopListeningAsync()
```

**Description:** Stops the speech recognition process. The component will stop listening for audio input and finalize the transcription.

**Returns:** `Task` - A task that completes when listening has stopped

**Usage:**
```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText @ref="speechComponent"></SfSpeechToText>

<button @onclick="StopListening">Stop Listening</button>

@code {
    private SfSpeechToText speechComponent;
    
    private async Task StopListening()
    {
        await speechComponent.StopListeningAsync();
    }
}
```

**Example with Button Integration:**
```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText @ref="speechComponent" @bind-Transcript="@transcript"></SfSpeechToText>

<button @onclick="async () => await speechComponent.StopListeningAsync()" 
        style="padding: 10px 20px; background: #dc3545; color: white; border: none; border-radius: 4px; cursor: pointer;">
    ⏹️ Stop Listening
</button>

<p>Final Transcript: @transcript</p>

@code {
    private SfSpeechToText speechComponent;
    private string transcript = "";
}
```

**Exception Handling:**
```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText @ref="speechComponent"></SfSpeechToText>

<button @onclick="SafeStopListening">Stop with Error Handling</button>

@code {
    private SfSpeechToText speechComponent;
    private string errorMessage = "";
    
    private async Task SafeStopListening()
    {
        try
        {
            await speechComponent.StopListeningAsync();
            errorMessage = "";
        }
        catch (Exception ex)
        {
            errorMessage = $"Failed to stop listening: {ex.Message}";
        }
    }
}
```

---

## 🎯 Complete Example: Start and Stop

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Speech Recognition Control</h3>
    
    <SfSpeechToText 
        @ref="speechComponent"
        @bind-Transcript="@transcript"
        ListeningState="@listeningState"
        SpeechRecognitionStarted="@OnStarted"
        SpeechRecognitionStopped="@OnStopped"
        SpeechRecognitionError="@OnError">
    </SfSpeechToText>
    
    <div style="margin-top: 20px;">
        <button @onclick="HandleStart" 
                disabled="@(listeningState == SpeechToTextState.Listening)"
                style="padding: 10px 20px; margin-right: 10px; background: #28a745; color: white; border: none; border-radius: 4px; cursor: pointer;">
            🎤 Start Listening
        </button>
        
        <button @onclick="HandleStop" 
                disabled="@(listeningState != SpeechToTextState.Listening)"
                style="padding: 10px 20px; background: #dc3545; color: white; border: none; border-radius: 4px; cursor: pointer;">
            ⏹️ Stop Listening
        </button>
    </div>
    
    <div style="margin-top: 20px;">
        <strong>Status:</strong> @GetStatusText()
    </div>
    
    <div style="margin-top: 20px; padding: 15px; background: #f5f5f5; border-radius: 4px;">
        <strong>Transcript:</strong>
        <p>@transcript</p>
    </div>
    
    @if (!string.IsNullOrEmpty(errorMessage))
    {
        <div style="margin-top: 20px; padding: 15px; background: #f8d7da; border: 1px solid #f5c6cb; border-radius: 4px; color: #721c24;">
            <strong>Error:</strong> @errorMessage
        </div>
    }
</div>

@code {
    private SfSpeechToText speechComponent;
    private string transcript = "";
    private SpeechToTextState listeningState = SpeechToTextState.Stopped;
    private string errorMessage = "";
    
    private async Task HandleStart()
    {
        try
        {
            errorMessage = "";
            await speechComponent.StartListeningAsync();
        }
        catch (Exception ex)
        {
            errorMessage = $"Failed to start: {ex.Message}";
        }
    }
    
    private async Task HandleStop()
    {
        try
        {
            errorMessage = "";
            await speechComponent.StopListeningAsync();
        }
        catch (Exception ex)
        {
            errorMessage = $"Failed to stop: {ex.Message}";
        }
    }
    
    private void OnStarted(SpeechRecognitionStartedEventArgs args)
    {
        listeningState = SpeechToTextState.Listening;
        Console.WriteLine("Speech recognition started");
    }
    
    private void OnStopped(SpeechRecognitionStoppedEventArgs args)
    {
        listeningState = SpeechToTextState.Stopped;
        Console.WriteLine("Speech recognition stopped");
    }
    
    private void OnError(SpeechRecognitionErrorEventArgs args)
    {
        errorMessage = args.ErrorMessage;
        listeningState = SpeechToTextState.Stopped;
    }
    
    private string GetStatusText() => listeningState switch
    {
        SpeechToTextState.Listening => "🎤 Listening...",
        SpeechToTextState.Stopped => "⏹️ Stopped",
        _ => "Unknown"
    };
}
```

---

## ⚠️ Best Practices

### ✅ DO
- Call `StartListeningAsync()` when ready to capture speech
- Call `StopListeningAsync()` to finalize transcription
- Await the async methods properly
- Handle potential exceptions
- Disable buttons appropriately based on listening state

### ❌ DON'T
- Call both methods simultaneously
- Forget to check listening state before calling methods
- Ignore error handling
- Call `StartListeningAsync()` twice without stopping first
- Assume the microphone is always available

---

## 🔗 Related Topics
- [Getting Started](getting-started.md)
- [Listening States](listening-states.md)
- [Error Handling](error-handling.md)
- [Events](../SYNCFUSION_BLAZOR_SPEECHTOTEXT_COMPLETE_API_REFERENCE.md#events)

---

**Last Updated:** March 24, 2026  
**Component:** Syncfusion Blazor SpeechToText  
**Namespace:** Syncfusion.Blazor.Inputs
