# Speech-to-Text in Blazor Inline AI Assist

## Table of Contents
1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Enable Speech-to-Text](#enable-speech-to-text)
4. [Configure Speech Recognition Language](#configure-speech-recognition-language)
5. [Enable Interim Results](#enable-interim-results)
6. [Speech-to-Text Events](#speech-to-text-events)
7. [InlineAIAssistSpeechToText Tag Properties](#inlineaiassistspeechtotext-tag-properties)
8. [Inline Toolbar Item for Speech-to-Text](#inline-toolbar-item-for-speech-to-text)
9. [Error Handling](#error-handling)
10. [Browser Compatibility](#browser-compatibility)
11. [Real-World Examples](#real-world-examples)

## Overview

The Blazor Inline AI Assist component provides built-in **Speech-to-Text** support through the browser's [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API). This feature converts spoken words into text using the device's microphone, allowing users to enter prompts without typing. When content is selected, the recognized speech can be submitted as a prompt along with the selected content to provide contextual AI assistance.

The feature is powered by the `SfSpeechToText` component from `Syncfusion.Blazor.Inputs` and is integrated as a child tag (`InlineAIAssistSpeechToText`) of `SfInlineAIAssist`.

## Prerequisites

Before integrating Speech-to-Text, ensure the following:

1. **The Inline AI Assist component is set up** in your Blazor application.
   - [Blazor Getting Started Guide](../getting-started-webapp)

2. **The Inline AI Assist component is integrated with an AI provider** (for end-to-end functionality).
   - [Integration of Azure OpenAI With Blazor Inline AI Assist component](../ai-integrations/openai-integration.md)

3. **The Syncfusion Blazor Inputs package is referenced** (it provides `SfSpeechToText` and the `InlineAIAssistSpeechToText` tag):

```xml
<ItemGroup>
  <PackageReference Include="Syncfusion.Blazor.InteractiveChat" Version="x.x.x" />
  <PackageReference Include="Syncfusion.Blazor.Inputs" Version="x.x.x" />
  <PackageReference Include="Syncfusion.Blazor.Themes" Version="x.x.x" />
</ItemGroup>
```

4. **Required namespaces** at the top of the component:

```razor
@using Syncfusion.Blazor.Inputs
@using Syncfusion.Blazor.InteractiveChat
@using Syncfusion.Blazor.Buttons
```

## Enable Speech-to-Text

To enable the built-in Speech-to-Text functionality, add the `InlineAIAssistSpeechToText` child tag to the `SfInlineAIAssist` component and set the `Enable` property to `true`.

```razor
<SfInlineAIAssist @ref="inlineAssist" RelateTo="#summarizeButton" PromptRequested="OnPromptRequestAsync">
    <InlineAIAssistSpeechToText Enable="true"></InlineAIAssistSpeechToText>
</SfInlineAIAssist>
```

**How it works:**
1. Once enabled, a **microphone icon** appears in the Inline AI Assist popup's built-in editor.
2. When the microphone is selected, the component captures audio from the device's microphone.
3. It converts speech into text using the browser's [SpeechRecognition](https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition) API.
4. The recognized text is inserted into the prompt editor.

## Configure Speech Recognition Language

The `Language` property specifies the language used for speech recognition. Set it to a language code such as `en-US` for American English or `fr-FR` for French. If no language is specified, speech recognition uses the browser's default language setting.

```razor
<InlineAIAssistSpeechToText Enable="true" Language="en-US"></InlineAIAssistSpeechToText>
```

Common language codes:

| Code | Language |
|------|----------|
| `en-US` | English (United States) |
| `en-GB` | English (United Kingdom) |
| `fr-FR` | French (France) |
| `de-DE` | German (Germany) |
| `es-ES` | Spanish (Spain) |
| `ja-JP` | Japanese (Japan) |

> Refer to the [SpeechToText Transcript and Language](https://blazor.syncfusion.com/documentation/speech-to-text/speech-recognition#language-support) documentation for the full list of supported locales.

## Enable Interim Results

The `AllowInterimResults` property controls whether the prompt editor displays partial results while the user is speaking.

| Value | Behavior |
|-------|----------|
| `true` | Show words as they are recognized in real time (interim updates) |
| `false` | Update the prompt editor only after the speech recognition engine produces the final transcript |

```razor
<InlineAIAssistSpeechToText Enable="true" Language="en-US" AllowInterimResults="true"></InlineAIAssistSpeechToText>
```

## Speech-to-Text Events

The `InlineAIAssistSpeechToText` tag exposes the same lifecycle events as `SfSpeechToText`. Use these to track recognition state, react to transcript changes, and surface errors.

| Event | EventArgs Type | When It Fires |
|-------|----------------|---------------|
| `RecognitionStart` | `SpeechRecognitionStartedEventArgs` | When recognition starts |
| `RecognitionStop` | `SpeechRecognitionStoppedEventArgs` | When recognition stops |
| `TranscriptChanged` | `TranscriptChangeEventArgs` | When the transcript is updated |
| `RecognitionError` | `SpeechRecognitionErrorEventArgs` | When an error occurs |

The recognized transcript is inserted into the Inline AI Assist prompt area. When the prompt is submitted, both typed and speech-recognized input are available through the `Prompt` property of the `PromptRequestedEventArgs` argument.

### Event Handler Example

```razor
<SfInlineAIAssist @ref="inlineAssist" RelateTo="#summarizeButton" PromptRequested="OnPromptRequestAsync">
    <InlineAIAssistSpeechToText Enable="true"
                                 Language="en-US"
                                 AllowInterimResults="true"
                                 RecognitionStart="OnRecognitionStart"
                                 RecognitionStop="OnRecognitionStop"
                                 TranscriptChanged="OnTranscriptChanged"
                                 RecognitionError="OnRecognitionError">
    </InlineAIAssistSpeechToText>
    <ResponseActions ItemSelect="OnItemSelectAsync"></ResponseActions>
</SfInlineAIAssist>

@code {
    private void OnRecognitionStart(SpeechRecognitionStartedEventArgs args)
    {
        // Update UI to show "Listening" state
    }

    private void OnRecognitionStop(SpeechRecognitionStoppedEventArgs args)
    {
        // Update UI to show "Stopped" state
    }

    private void OnTranscriptChanged(TranscriptChangeEventArgs args)
    {
        // React to interim/final transcript updates
    }

    private void OnRecognitionError(SpeechRecognitionErrorEventArgs args)
    {
        // Display error message to user
    }
}
```

## InlineAIAssistSpeechToText Tag Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Enable` | bool | `false` | Enables or disables the built-in Speech-to-Text functionality |
| `Language` | string | Browser default | BCP-47 language code for speech recognition (e.g., `en-US`, `fr-FR`) |
| `AllowInterimResults` | bool | `false` | When `true`, shows partial transcript while the user is speaking |
| `RecognitionStart` | EventCallback\<SpeechRecognitionStartedEventArgs\> | - | Fires when recognition starts |
| `RecognitionStop` | EventCallback\<SpeechRecognitionStoppedEventArgs\> | - | Fires when recognition stops |
| `TranscriptChanged` | EventCallback\<TranscriptChangeEventArgs\> | - | Fires when the transcript is updated |
| `RecognitionError` | EventCallback\<SpeechRecognitionErrorEventArgs\> | - | Fires when an error occurs |

## Inline Toolbar Item for Speech-to-Text

If you are using a custom `InlineToolbar` configuration, include the speech-to-text item so users can toggle voice input from the toolbar:

```razor
<InlineToolbar>
    <InlineToolbarItem IconCss="e-icons e-inline-assist-speech-to-text"></InlineToolbarItem>
</InlineToolbar>
```

The built-in speech-to-text button is shown automatically when `InlineAIAssistSpeechToText` is enabled, but you can surface an additional toolbar entry using the `e-inline-assist-speech-to-text` icon class.

## Error Handling

The `SpeechToText` component provides events to handle errors that may occur during speech recognition, including:
- `no-speech` - No speech was detected
- `audio-capture` - Microphone is unavailable
- `not-allowed` - User denied microphone permission
- `network` - Network connectivity error
- `aborted` - Recognition was aborted

For more information, refer to the [Error Handling](https://blazor.syncfusion.com/documentation/speech-to-text/speech-recognition#error-handling) section in the Speech-to-Text documentation.

### Example: Friendly Error Messaging

```razor
@code {
    private string recognitionStatus = "Ready";
    private string recognitionError = string.Empty;

    private void OnRecognitionError(SpeechRecognitionErrorEventArgs args)
    {
        recognitionStatus = "Error";
        recognitionError = args.Error switch
        {
            "no-speech" => "No speech detected. Please try again.",
            "audio-capture" => "No microphone found. Check your device.",
            "not-allowed" => "Microphone access denied. Check browser permissions.",
            "network" => "Network error. Check your connection.",
            _ => "Speech recognition failed."
        };
    }
}
```

## Browser Compatibility

The `SpeechToText` component relies on the [Speech Recognition API](https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition), which has limited browser support.

| Browser | Support |
|---------|---------|
| Chrome (desktop & Android) | Supported |
| Edge | Supported |
| Safari (macOS & iOS) | Supported |
| Opera | Supported |
| Firefox | Not supported (no Web Speech API implementation) |

Refer to the [Browser Compatibility](https://blazor.syncfusion.com/documentation/speech-to-text/speech-recognition#browser-support) section for detailed information.

## Real-World Examples

### Example 1: Content Summarization with Voice Input

Voice-driven content summarization. The user speaks a prompt (e.g., "summarize this in two sentences"), and the Inline AI Assist submits the recognized transcript along with the selected editor content.

```razor
@using Syncfusion.Blazor.Inputs
@using Syncfusion.Blazor.InteractiveChat
@using Syncfusion.Blazor.Buttons

<style>
    #editableText {
        width: 100%;
        min-height: 120px;
        max-height: 300px;
        overflow-y: auto;
        font-size: 16px;
        padding: 12px;
        border-radius: 4px;
        border: 1px solid;
    }
</style>

<div class="container" style="height: 350px; width: 650px;">
    <SfButton id="summarizeButton" IsPrimary="true" Style="margin-bottom: 10px;" @onclick="OnSummarizeClick">
        Content Summarize
    </SfButton>
    <div id="editableText" contenteditable="true">
        @((MarkupString)editableContent)
    </div>
    <SfInlineAIAssist @ref="inlineAssist" RelateTo="#summarizeButton" PromptRequested="OnPromptRequestAsync">
        <InlineAIAssistSpeechToText Enable="true"
                                     Language="en-US"
                                     AllowInterimResults="true"
                                     RecognitionStart="OnRecognitionStart"
                                     RecognitionStop="OnRecognitionStop"
                                     TranscriptChanged="OnTranscriptChanged"
                                     RecognitionError="OnRecognitionError">
        </InlineAIAssistSpeechToText>
        <InlineToolbar>
            <InlineToolbarItem IconCss="e-icons e-inline-assist-speech-to-text"></InlineToolbarItem>
        </InlineToolbar>
        <ResponseActions ItemSelect="OnItemSelectAsync"></ResponseActions>
    </SfInlineAIAssist>
</div>

@code {
    private SfInlineAIAssist inlineAssist = new SfInlineAIAssist();
    private string editableContent = @"<p>Inline AI Assist component provides intelligent text processing capabilities that enhance user productivity. It leverages advanced natural language processing to understand context and deliver precise suggestions. Users can seamlessly integrate AI-powered features into their applications.</p>
        <p>With real-time response streaming and customizable prompts, developers can create interactive experiences. The component supports multiple response modes including inline editing and popup-based interactions.</p>";

    private string recognitionStatus = "Ready";
    private string recognitionError = string.Empty;

    private void OnRecognitionStart(SpeechRecognitionStartedEventArgs args)
    {
        recognitionStatus = "Listening";
        recognitionError = string.Empty;
    }

    private void OnRecognitionStop(SpeechRecognitionStoppedEventArgs args)
    {
        recognitionStatus = "Stopped";
    }

    private void OnTranscriptChanged(TranscriptChangeEventArgs args)
    {
        recognitionStatus = "Transcript updated";
    }

    private void OnRecognitionError(SpeechRecognitionErrorEventArgs args)
    {
        recognitionStatus = "Error";
        recognitionError = "Speech recognition failed.";
    }

    private async Task OnPromptRequestAsync(PromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        string defaultResponse = "For real-time prompt processing, connect the Inline AI Assist component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.";
        await inlineAssist.UpdateResponseAsync(defaultResponse);
    }

    private async Task OnItemSelectAsync(ResponseItemSelectEventArgs args)
    {
        if (args.Item.Label == "Accept")
        {
            var lastPrompt = inlineAssist?.Prompts.LastOrDefault();
            if (lastPrompt != null && !string.IsNullOrEmpty(lastPrompt.Response))
            {
                editableContent = $"<p>{lastPrompt.Response}</p>";
            }
            await inlineAssist!.HidePopupAsync();
        }
        else if (args.Item.Label == "Discard")
        {
            await inlineAssist!.HidePopupAsync();
        }
    }

    private async Task OnSummarizeClick()
    {
        await inlineAssist.ShowPopupAsync();
    }
}
```

### Example 2: Multilingual Voice Prompt

Allow the user to switch the recognition language at runtime:

```razor
<select @bind="selectedLanguage">
    <option value="en-US">English (US)</option>
    <option value="fr-FR">French</option>
    <option value="de-DE">German</option>
    <option value="es-ES">Spanish</option>
</select>

<SfInlineAIAssist @ref="inlineAssist" RelateTo="#aiButton" PromptRequested="OnPromptRequested">
    <InlineAIAssistSpeechToText Enable="true" Language="@selectedLanguage" AllowInterimResults="true">
    </InlineAIAssistSpeechToText>
    <ResponseActions ItemSelect="OnResponseItemSelect"></ResponseActions>
</SfInlineAIAssist>

@code {
    private string selectedLanguage = "en-US";

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        var response = $"You said: {args.Prompt}";
        await inlineAssist.UpdateResponseAsync(response, true);
    }

    private async Task OnResponseItemSelect(ResponseItemSelectEventArgs args)
    {
        await inlineAssist.HidePopupAsync();
    }
}
```

## Best Practices

1. **Set an explicit language** - Always set `Language` to a known BCP-47 code for predictable recognition.
2. **Use interim results for UX** - Set `AllowInterimResults="true"` so users see their words appear live; commit on stop.
3. **Handle microphone errors** - Always wire the `RecognitionError` event to provide user-friendly messages and recovery steps.
4. **Add the toolbar entry** - When using a custom `InlineToolbar`, include the `e-inline-assist-speech-to-text` icon item.
5. **Combine with selection context** - When content is selected, the recognized speech is automatically submitted as a prompt alongside the selected text, providing contextual AI assistance.
6. **Test browser support** - Verify support in your target browsers; Firefox is not supported.
7. **Pair with AI integration** - For end-to-end functionality, ensure the Inline AI Assist is connected to an AI provider so recognized speech is sent as a prompt.

## Related Components

- **SfSpeechToText** - The standalone Syncfusion Blazor Speech-to-Text component from `Syncfusion.Blazor.Inputs`. See the `syncfusion-blazor-speech-to-text` skill for full coverage.
- **SfInlineAIAssist** - The Inline AI Assist component that hosts the speech-to-text capability.
