---
name: syncfusion-blazor-speech-to-text
description: Implement speech-to-text voice input in Blazor applications using Syncfusion SpeechToText component. ALWAYS use this when users need voice input, speech recognition, audio transcription, or implementing the SpeechToText component in Blazor. Trigger for Syncfusion.Blazor.Inputs, microphone input, voice-to-text conversion, language support, transcript binding, listening states, error handling, browser speech API, or any speech recognition requirements.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Syncfusion Blazor Speech To Text

A comprehensive guide for implementing voice input and speech recognition using the Syncfusion Blazor SpeechToText component. This component captures audio from the user's microphone and converts it to text in real time.

## Component Overview

The **SpeechToText** component provides voice input capabilities by leveraging the browser's Speech Recognition API. It automatically handles microphone access, captures audio, transcribes speech, and manages the listening lifecycle.

**Key Features:**
- Real-time speech-to-text conversion
- Multi-language support (en-US, fr-FR, de-DE, and 100+ languages)
- Listening state management (Inactive, Listening, Stopped)
- Interim results for real-time feedback
- Error handling for various microphone/network issues
- Browser compatibility detection
- Customizable tooltips and disabled states
- Event callbacks for lifecycle hooks

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation via NuGet package
- Basic component setup and configuration
- Property binding and two-way data binding
- CSS styling and theming
- Integration with TextArea for display
- Minimal working example

### Transcript and Language
📄 **Read:** [references/transcript-and-language.md](references/transcript-and-language.md)
- Retrieving transcribed text with Transcript property
- Binding transcript to input fields
- Setting language preferences (en-US, fr-FR, etc.)
- Supported language codes and locales
- Real-world multi-language examples
- Language switching patterns

### Button Customization
📄 **Read:** [references/button-customization.md](references/button-customization.md)
- SpeechToTextButtonSettings configuration
- Button text customization (start and stop states)
- Icon CSS classes and icon positioning
- Icon position values (Left, Right, Top, Bottom)
- Primary button styling (IsPrimary property)
- Dynamic button customization patterns
- Icon library integration (Syncfusion, Font Awesome, Bootstrap)
- Multi-language button text examples

### Tooltip Configuration
📄 **Read:** [references/tooltip-configuration.md](references/tooltip-configuration.md)
- SpeechToTextTooltipSettings configuration
- Tooltip text customization for different states
- Tooltip positioning (12 position options available)
- Position reference guide and best practices
- ShowTooltip property control
- Dynamic tooltip enabling/disabling
- Multi-language tooltip support
- Accessibility considerations for tooltips

### Listening States
📄 **Read:** [references/listening-states.md](references/listening-states.md)
- Understanding ListeningState property
- State values: Inactive, Listening, Stopped
- Event handling: SpeechRecognitionStarted, SpeechRecognitionStopped
- State-based UI updates and visual feedback
- Waveform animations during listening
- Status indicators and user guidance

### Interim Results and Configuration
📄 **Read:** [references/interim-results-and-options.md](references/interim-results-and-options.md)
- AllowInterimResults property for real-time updates
- Difference between interim and final results
- ShowTooltip property and tooltip customization
- Disabled state configuration
- HTML attributes for custom styling
- Control panel options and UI customization

### Public Methods
📄 **Read:** [references/methods.md](references/methods.md)
- StartListeningAsync() - Start speech recognition
- StopListeningAsync() - Stop speech recognition
- Method signatures and return types
- Exception handling patterns
- Complete control examples with start/stop buttons
- State management and button disabling

### Error Handling and Troubleshooting
📄 **Read:** [references/error-handling.md](references/error-handling.md)
- Error types: no-speech, aborted, audio-capture, not-allowed, network, etc.
- Error handling patterns and recovery
- User feedback mechanisms for errors
- Network and service availability issues
- Microphone permission troubleshooting
- Browser compatibility checking

### Browser Support and API Limitations
📄 **Read:** [references/browser-support.md](references/browser-support.md)
- Browser compatibility matrix
- Version requirements for Chrome, Edge, Safari, Opera
- Unsupported browsers (Firefox)
- Feature detection and polyfills
- Graceful degradation for unsupported browsers

## Quick Start Example

Here's a minimal example to get started with SpeechToText:

```razor
@using Syncfusion.Blazor.Inputs

<div style="display: flex; flex-direction: column; gap: 20px; align-items: center; padding: 20px;">
    <h3>Voice to Text Converter</h3>
    
    <SfSpeechToText @bind-Transcript="@transcript"></SfSpeechToText>
    
    <SfTextArea 
        RowCount="5" 
        ColumnCount="50" 
        @bind-Value="@transcript" 
        ResizeMode="Resize.None" 
        Placeholder="Transcribed text will appear here...">
    </SfTextArea>
</div>

@code {
    private string transcript = "";
}
```

**What this does:**
1. Renders a microphone button from SpeechToText
2. Binds the transcribed text to the `transcript` variable
3. Displays the transcript in a TextArea for editing
4. Uses two-way binding to keep both in sync

## Common Patterns

### Pattern 1: Real-Time Transcription Display
Display interim results as the user speaks (not just final results):

```razor
<SfSpeechToText 
    AllowInterimResults="true"
    @bind-Transcript="@transcript">
</SfSpeechToText>
<p>@transcript</p>
```

### Pattern 2: Multi-Language Support
Allow users to switch between languages:

```razor
<select @onchange="@((ChangeEventArgs e) => selectedLanguage = e.Value.ToString())">
    <option value="en-US">English</option>
    <option value="fr-FR">French</option>
    <option value="de-DE">German</option>
</select>

<SfSpeechToText 
    Language="@selectedLanguage"
    @bind-Transcript="@transcript">
</SfSpeechToText>

@code {
    private string selectedLanguage = "en-US";
    private string transcript = "";
}
```

### Pattern 3: Listen for State Changes
Provide visual feedback based on listening state:

```razor
<SfSpeechToText 
    ListeningState="@listeningState"
    SpeechRecognitionStarted="@OnListeningStarted"
    SpeechRecognitionStopped="@OnListeningStopped">
</SfSpeechToText>

<div style="margin-top: 15px;">
    @if (listeningState == SpeechToTextState.Listening)
    {
        <span style="color: green;">🎤 Listening...</span>
    }
    else if (listeningState == SpeechToTextState.Stopped)
    {
        <span style="color: orange;">⏸ Stopped</span>
    }
    else
    {
        <span style="color: gray;">○ Ready to listen</span>
    }
</div>

@code {
    private SpeechToTextState listeningState = SpeechToTextState.Inactive;

    private void OnListeningStarted(SpeechRecognitionStartedEventArgs args)
    {
        listeningState = args.State;
    }

    private void OnListeningStopped(SpeechRecognitionStoppedEventArgs args)
    {
        listeningState = args.State;
    }
}
```

### Pattern 4: Error Handling
Gracefully handle errors and inform users:

```razor
<SfSpeechToText 
    SpeechRecognitionError="@OnSpeechError"
    @bind-Transcript="@transcript">
</SfSpeechToText>

@if (!string.IsNullOrEmpty(errorMessage))
{
    <div style="color: red; margin-top: 10px;">
        ⚠️ @errorMessage
    </div>
}

@code {
    private string transcript = "";
    private string errorMessage = "";

    private void OnSpeechError(SpeechRecognitionErrorEventArgs args)
    {
        errorMessage = args.Error switch
        {
            "no-speech" => "No speech detected. Please try again.",
            "audio-capture" => "No microphone found. Check your device.",
            "not-allowed" => "Microphone access denied. Check browser permissions.",
            "network" => "Network error. Check your connection.",
            _ => $"Error: {args.Error}"
        };
    }
}
```

## Key Properties

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `Transcript` | string | "" | Two-way binding for transcribed text |
| `Language` | string | "en-US" | Language code for speech recognition |
| `ListeningState` | SpeechToTextState | Inactive | Current state (Inactive, Listening, Stopped) |
| `AllowInterimResults` | bool | true | Show interim results while speaking |
| `ShowTooltip` | bool | true | Display tooltip on hover |
| `Disabled` | bool | false | Disable the component |
| `HtmlAttributes` | Dictionary | - | Custom HTML attributes for button |

## Common Use Cases

1. **Voice Search:** Add voice input to a search box for hands-free searching
2. **Form Filling:** Populate form fields using voice input instead of typing
3. **Notes Application:** Capture voice notes and convert to text
4. **Accessibility:** Enable voice input for users with typing limitations
5. **Multilingual Chat:** Support speech input in multiple languages for chat applications
6. **Dictation Feature:** Create a dictation tool for long-form text entry
7. **Customer Support:** Record and transcribe customer feedback or support calls
8. **Accessibility Compliance:** Meet WCAG requirements for voice-enabled interfaces

---

## Next Steps

- Start with **Getting Started** reference for installation
- Explore **Transcript and Language** for basic usage
- Check **Browser Support** to ensure compatibility
- Review **Error Handling** for production-ready implementation
