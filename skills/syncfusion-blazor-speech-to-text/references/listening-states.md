# Listening States and Status Management

## Table of Contents
- [Understanding Listening States](#understanding-listening-states)
- [State Values](#state-values)
- [Managing State](#managing-state)
- [Event Handling](#event-handling)
- [Visual Feedback](#visual-feedback)
- [State-Based UI Patterns](#state-based-ui-patterns)

## Understanding Listening States

The `ListeningState` property represents the current state of the speech recognition process. This allows you to:
- Provide visual feedback to users
- Disable/enable form fields based on listening status
- Track when recording starts and stops
- Implement conditional logic based on state

## State Values

The `SpeechToTextState` enum has three values:

### 1. Inactive (Default)
The component is idle with no active speech recognition.

**Characteristics:**
- Initial state when the component loads
- No audio is being captured
- User can click to start listening
- Button appears ready/enabled

**Use case:** Show a default icon or "Click to speak" message

```razor
@if (listeningState == SpeechToTextState.Inactive)
{
    <span style="color: gray; font-weight: bold;">○ Ready to speak</span>
}
```

### 2. Listening
The component is actively capturing audio and transcribing speech in real time.

**Characteristics:**
- User clicked the microphone button
- Audio is being captured from the microphone
- Interim results are displayed (if enabled)
- Visual indicator shows active recording
- A stop icon appears to allow cancellation

**Use case:** Show a "Recording..." message or animated icon

```razor
@if (listeningState == SpeechToTextState.Listening)
{
    <span style="color: #4CAF50; font-weight: bold;">🎤 Listening...</span>
}
```

### 3. Stopped
Speech recognition has ended, and no further audio is being processed.

**Characteristics:**
- Speech recognition completed
- Final transcript is ready
- User may have clicked stop or stopped speaking
- Component returns to idle after this state
- May move back to Inactive automatically

**Use case:** Show a "Processing complete" message

```razor
@if (listeningState == SpeechToTextState.Stopped)
{
    <span style="color: #FF9800; font-weight: bold;">⏸ Stopped</span>
}
```

## Managing State

### Binding ListeningState

To track the listening state, bind the `ListeningState` property:

```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText ListeningState="@listeningState" @bind-Transcript="@transcript"></SfSpeechToText>

<p>Current state: @listeningState</p>

@code {
    private SpeechToTextState listeningState = SpeechToTextState.Inactive;
    private string transcript = "";
}
```

### State Transitions

Understanding how states transition helps you predict behavior:

```
Inactive → (user clicks) → Listening → (speech ends or user clicks stop) → Stopped → (automatic) → Inactive
```

## Event Handling

Use event callbacks to react when state changes:

### SpeechRecognitionStarted Event

Fires when listening begins:

```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText 
    SpeechRecognitionStarted="@OnListeningStarted"
    @bind-Transcript="@transcript">
</SfSpeechToText>

@code {
    private string transcript = "";
    
    private void OnListeningStarted(SpeechRecognitionStartedEventArgs args)
    {
        Console.WriteLine($"Listening started. State: {args.State}");
        // Disable form fields, show recording indicator, etc.
    }
}
```

### SpeechRecognitionStopped Event

Fires when listening ends:

```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText 
    SpeechRecognitionStopped="@OnListeningStopped"
    @bind-Transcript="@transcript">
</SfSpeechToText>

@code {
    private string transcript = "";
    
    private void OnListeningStopped(SpeechRecognitionStoppedEventArgs args)
    {
        Console.WriteLine($"Listening stopped. State: {args.State}");
        // Re-enable form fields, hide recording indicator, etc.
    }
}
```

### Complete Event Handling Example

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px; max-width: 500px;">
    <h3>Voice Recording with State Tracking</h3>
    
    <div style="margin-bottom: 20px; padding: 15px; background: #f5f5f5; border-radius: 4px;">
        <strong>State:</strong> @listeningState
        <br />
        <strong>Status:</strong> @GetStatusMessage()
    </div>
    
    <SfSpeechToText 
        ListeningState="@listeningState"
        SpeechRecognitionStarted="@OnListeningStarted"
        SpeechRecognitionStopped="@OnListeningStopped"
        @bind-Transcript="@transcript">
    </SfSpeechToText>
    
    @if (!string.IsNullOrEmpty(transcript))
    {
        <div style="margin-top: 20px; padding: 15px; background: #e3f2fd; border-radius: 4px;">
            <strong>Transcript:</strong>
            <p>@transcript</p>
        </div>
    }
</div>

@code {
    private SpeechToTextState listeningState = SpeechToTextState.Inactive;
    private string transcript = "";
    
    private void OnListeningStarted(SpeechRecognitionStartedEventArgs args)
    {
        listeningState = args.State;
    }
    
    private void OnListeningStopped(SpeechRecognitionStoppedEventArgs args)
    {
        listeningState = args.State;
    }
    
    private string GetStatusMessage()
    {
        return listeningState switch
        {
            SpeechToTextState.Listening => "Recording audio...",
            SpeechToTextState.Stopped => "Processing speech...",
            _ => "Ready to record"
        };
    }
}
```

## Visual Feedback

Provide clear visual indicators of the listening state:

### Status Indicator with Colors

```razor
@using Syncfusion.Blazor.Inputs

<div>
    <div style="margin-bottom: 20px; padding: 15px; border-radius: 4px; @GetStatusStyle()">
        <strong>@GetStatusEmoji() @GetStatusText()</strong>
    </div>
    
    <SfSpeechToText 
        ListeningState="@listeningState"
        SpeechRecognitionStarted="@(args => listeningState = args.State)"
        SpeechRecognitionStopped="@(args => listeningState = args.State)"
        @bind-Transcript="@transcript">
    </SfSpeechToText>
</div>

@code {
    private SpeechToTextState listeningState = SpeechToTextState.Inactive;
    private string transcript = "";
    
    private string GetStatusStyle()
    {
        return listeningState switch
        {
            SpeechToTextState.Listening => "background: #d1e7dd; color: #0f5132;",
            SpeechToTextState.Stopped => "background: #f8d7da; color: #842029;",
            _ => "background: #e2e3e5; color: #6c757d;"
        };
    }
    
    private string GetStatusText()
    {
        return listeningState switch
        {
            SpeechToTextState.Listening => "Recording in progress",
            SpeechToTextState.Stopped => "Speech recognized, processing...",
            _ => "Click microphone to start"
        };
    }
    
    private string GetStatusEmoji()
    {
        return listeningState switch
        {
            SpeechToTextState.Listening => "🔴",
            SpeechToTextState.Stopped => "⏹",
            _ => "⭕"
        };
    }
}
```

### Waveform Animation

Show a visual representation of active listening:

```razor
@using Syncfusion.Blazor.Inputs

<style>
    .waveform {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 40px;
        gap: 5px;
        margin: 20px 0;
    }

    .waveform span {
        display: block;
        width: 6px;
        height: 20px;
        background: #28a745;
        animation: wave-animation 1.2s infinite ease-in-out;
    }

    .waveform span:nth-child(1) { animation-delay: 0s; }
    .waveform span:nth-child(2) { animation-delay: 0.2s; }
    .waveform span:nth-child(3) { animation-delay: 0.4s; }
    .waveform span:nth-child(4) { animation-delay: 0.6s; }
    .waveform span:nth-child(5) { animation-delay: 0.8s; }

    @@keyframes wave-animation {
        0%, 100% {
            height: 10px;
        }
        50% {
            height: 30px;
        }
    }
</style>

<div style="text-align: center;">
    <SfSpeechToText 
        ListeningState="@listeningState"
        SpeechRecognitionStarted="@(args => listeningState = args.State)"
        SpeechRecognitionStopped="@(args => listeningState = args.State)"
        @bind-Transcript="@transcript">
    </SfSpeechToText>

    @if (listeningState == SpeechToTextState.Listening)
    {
        <div class="waveform">
            <span></span>
            <span></span>
            <span></span>
            <span></span>
            <span></span>
        </div>
        <p style="color: #28a745; font-weight: bold;">Listening... Speak now!</p>
    }
    else if (listeningState == SpeechToTextState.Stopped)
    {
        <p style="color: #FF9800;">Processing...</p>
    }
</div>

@code {
    private SpeechToTextState listeningState = SpeechToTextState.Inactive;
    private string transcript = "";
}
```

## State-Based UI Patterns

### Pattern 1: Disable Form During Recording

Prevent user from modifying data while recording:

```razor
@using Syncfusion.Blazor.Inputs

<div style="max-width: 500px;">
    <SfSpeechToText 
        ListeningState="@listeningState"
        SpeechRecognitionStarted="@(args => listeningState = args.State)"
        SpeechRecognitionStopped="@(args => listeningState = args.State)"
        @bind-Transcript="@transcript">
    </SfSpeechToText>
    
    <div style="margin-top: 20px;">
        <input 
            type="text" 
            @bind="@name" 
            placeholder="Name"
            disabled="@(listeningState == SpeechToTextState.Listening)"
            style="width: 100%; padding: 8px; opacity: @(listeningState == SpeechToTextState.Listening ? 0.5 : 1);" />
        
        <textarea 
            @bind="@transcript" 
            placeholder="Transcript"
            disabled="@(listeningState == SpeechToTextState.Listening)"
            rows="4"
            style="width: 100%; padding: 8px; margin-top: 10px; opacity: @(listeningState == SpeechToTextState.Listening ? 0.5 : 1);"></textarea>
    </div>
</div>

@code {
    private SpeechToTextState listeningState = SpeechToTextState.Inactive;
    private string transcript = "";
    private string name = "";
}
```

### Pattern 2: Show Different UI Based on State

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <SfSpeechToText 
        ListeningState="@listeningState"
        SpeechRecognitionStarted="@(args => listeningState = args.State)"
        SpeechRecognitionStopped="@(args => listeningState = args.State)"
        @bind-Transcript="@transcript">
    </SfSpeechToText>

    @if (listeningState == SpeechToTextState.Inactive)
    {
        <p style="color: #666; margin-top: 20px;">Click the microphone to start recording your message.</p>
    }
    else if (listeningState == SpeechToTextState.Listening)
    {
        <div style="background: #d1e7dd; padding: 15px; border-radius: 4px; margin-top: 20px;">
            <p style="margin: 0; color: #0f5132; font-weight: bold;">🎤 Recording... Please speak clearly.</p>
        </div>
    }
    else if (listeningState == SpeechToTextState.Stopped)
    {
        <div style="background: #fff3cd; padding: 15px; border-radius: 4px; margin-top: 20px;">
            <p style="margin: 0; color: #856404; font-weight: bold;">⏹ Processed. Review your message below.</p>
        </div>
        <div style="background: #e3f2fd; padding: 15px; border-radius: 4px; margin-top: 10px;">
            <p>@transcript</p>
        </div>
    }
</div>

@code {
    private SpeechToTextState listeningState = SpeechToTextState.Inactive;
    private string transcript = "";
}
```

## Best Practices

1. **Always initialize state** - Start with `Inactive` to ensure predictable behavior
2. **Provide visual feedback** - Show users what state the component is in
3. **Update UI based on state** - Disable/enable controls appropriately
4. **Handle all state transitions** - Plan for Inactive → Listening → Stopped cycles
5. **Consider accessibility** - Announce state changes for screen reader users
