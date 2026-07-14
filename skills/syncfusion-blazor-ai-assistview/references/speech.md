# Speech Recognition and Synthesis

## Table of Contents
- [Overview](#overview)
- [Text-to-Speech](#text-to-speech)
- [Configuring Speech Settings](#configuring-speech-settings)
- [Speech-to-Text](#speech-to-text)
- [Speech-to-Text Configuration Options](#speech-to-text-configuration-options)
- [Error Handling](#error-handling)
- [Browser Compatibility](#browser-compatibility)
- [See Also](#see-also)

## Overview

The Syncfusion Blazor AI AssistView component provides comprehensive speech capabilities through built-in support for both **Text-to-Speech (TTS)** and **Speech-to-Text (STT)** using the browser's Web Speech APIs. This enables accessible voice interactions, allowing users to both listen to AI responses and provide input via voice.

## Text-to-Speech

The Syncfusion Blazor AI AssistView component provides built-in `Text-to-Speech` (TTS) support using the browser's Web Speech API, specifically the [SpeechSynthesisUtterance](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesisUtterance) interface. This allows AI-generated responses to be converted into spoken audio, enhancing accessibility and user interaction.

### Configure Text-to-Speech

To enable the built-in Text-to-Speech functionality, add the `e-assist-audio` icon to the `ResponseToolbarItem` tag directive within the `ResponseToolbar`. When clicked, it fetches the text from the generated AI response and uses the browser's SpeechSynthesis API to read it aloud.

```csharp
@using Syncfusion.Blazor.InteractiveChat
@using AssistView_OpenAI.Components.Services
@using Syncfusion.Blazor.Navigations
@inject AzureOpenAIService OpenAIService

<div class="integration-texttospeech-section">
    <SfAIAssistView @ref="assistView" PromptRequested="@PromptRequest">
        <AssistViews>
            <AssistView>
                <BannerTemplate>
                    <div class="banner-content">
                        <div class="e-icons e-assist-audio"></div>
                        <i>Ready to assist voice enabled !</i>
                    </div>
                </BannerTemplate>
            </AssistView>
        </AssistViews>
        <AssistViewToolbar ItemClicked="ToolbarItemClicked">
            <AssistViewToolbarItem Type="ItemType.Spacer"></AssistViewToolbarItem>
            <AssistViewToolbarItem IconCss="e-icons e-refresh"></AssistViewToolbarItem>
        </AssistViewToolbar>
        <ResponseToolbar>
            <ResponseToolbarItem IconCss="e-icons e-assist-copy" Tooltip="Copy"></ResponseToolbarItem>
            <ResponseToolbarItem IconCss="e-icons e-assist-audio" Tooltip="Read Aloud"></ResponseToolbarItem>
            <ResponseToolbarItem IconCss="e-icons e-assist-like" Tooltip="Like"></ResponseToolbarItem>
            <ResponseToolbarItem IconCss="e-icons e-assist-dislike" Tooltip="Need Improvement"></ResponseToolbarItem>
        </ResponseToolbar>
    </SfAIAssistView>
</div>

@code {
    private SfAIAssistView assistView;
    private string finalResponse { get; set; }
    private async Task PromptRequest(AssistViewPromptRequestedEventArgs args)
    {
        var lastIdx = assistView.Prompts.Count - 1;
        assistView.Prompts[lastIdx].Response = string.Empty;
        finalResponse = string.Empty;
        try
        {
            await foreach (var chunk in OpenAIService.GetChatResponseStreamAsync(args.Prompt))
            {
                await UpdateResponse(args, chunk);
            }

            args.Response = finalResponse;
        }
        catch (Exception ex)
        {
            args.Response = $"Error: {ex.Message}";
        }
    }

    private async Task UpdateResponse(AssistViewPromptRequestedEventArgs args, string response)
    {
        var lastIdx = assistView.Prompts.Count - 1;
        await Task.Delay(30); // Small delay for UI updates
        assistView.Prompts[lastIdx].Response += response.Replace("\n", "<br>");
        finalResponse = assistView.Prompts[lastIdx].Response;
        StateHasChanged();
    }

    private void ToolbarItemClicked(AssistViewToolbarItemClickedEventArgs args)
    {
        if (args.Item.IconCss == "e-icons e-refresh")
        {
            assistView.Prompts.Clear();
        }
    }
}
```

**CSS Styling:**

```css
.integration-texttospeech-section {
    height: 350px;
    width: 650px;
    margin: 0 auto;
}

.integration-texttospeech-section .banner-content .e-assist-audio:before {
    font-size: 25px;
}

.integration-texttospeech-section .e-view-container {
    margin: auto;
}

.integration-texttospeech-section .banner-content {
    display: flex;
    flex-direction: column;
    gap: 10px;
    text-align: center;
}

@media only screen and (max-width: 750px) {
    .integration-texttospeech-section {
        width: 100%;
    }
}
```

## Configuring Speech Settings

You can use the `TextToSpeechSettings` tag to customize the speech synthesis behavior using the following available properties:

| Property | Type | Description |
|----------|------|-------------|
| `Language` | string | Language code for speech synthesis (e.g., "en-US") |
| `SpeechPitch` | float | Pitch level for speech (0.1 to 2.0, default: 1) |
| `SpeechRate` | float | Rate of speech (0.1 to 2.0, default: 1) |
| `Volume` | float | Volume level for speech (0 to 1, default: 1) |
| `Voice` | string | Voice selection for speech synthesis |

### Example with Custom Speech Settings

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div class="integration-texttospeech-section">
    <SfAIAssistView Prompts="@promptsData" PromptRequested="@PromptRequest">
        <ResponseToolbar>
            <ResponseToolbarItem IconCss="e-icons e-assist-copy" Tooltip="Copy"></ResponseToolbarItem>
            <ResponseToolbarItem IconCss="e-icons e-assist-audio" Tooltip="Read Aloud"></ResponseToolbarItem>
            <ResponseToolbarItem IconCss="e-icons e-assist-like" Tooltip="Like"></ResponseToolbarItem>
            <ResponseToolbarItem IconCss="e-icons e-assist-dislike" Tooltip="Need Improvement"></ResponseToolbarItem>
        </ResponseToolbar>
        <AIAssistViewTextToSpeechSettings Language="en-US" SpeechPitch="1" SpeechRate="1" Volume="1">
        </AIAssistViewTextToSpeechSettings>
    </SfAIAssistView>
</div>

@code {
    private List<AssistViewPrompt> promptsData = new List<AssistViewPrompt>()
    {
        new AssistViewPrompt() { Prompt = "What is AI?", Response = "AI stands for Artificial Intelligence, enabling machines to mimic human intelligence for tasks such as learning, problem-solving, and decision-making." }
    };

    private async Task PromptRequest(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "For real-time prompt processing, connect the AI AssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.";
    }
}
```

**CSS Styling:**

```css
.integration-texttospeech-section {
    height: 350px;
    width: 650px;
    margin: 0 auto;
}

@media only screen and (max-width: 750px) {
    .integration-texttospeech-section {
        width: 100%;
    }
}
```

## Speech-to-Text

The Blazor AI AssistView component integrates `Speech-to-Text` functionality through the browser's [Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API). This enables the conversion of spoken words into text using the device's microphone, allowing users to interact with the AI AssistView through voice input.

### Configure Speech-to-Text

To enable Speech-to-Text functionality in the Blazor AI AssistView component, the `SpeechToText` component listens to audio input from the device's microphone, transcribes spoken words into text, and updates the AI AssistView's editable footer using the `FooterTemplate` tag directive to display the transcribed text. The transcribed text is then sent as a prompt to the Azure OpenAI service via the AI AssistView component.

```csharp
@using Syncfusion.Blazor.InteractiveChat
@using AssistView_OpenAI.Components.Services
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Inputs
@using Syncfusion.Blazor.Buttons
@inject AzureOpenAIService OpenAIService
@inject IJSRuntime JSRuntime

<div class="integration-speechtotext-section">
    <SfAIAssistView @ref="assistView" PromptRequested="@PromptRequest">
        <AssistViews>
            <AssistView>
                <FooterTemplate>
                    <div class="e-footer-wrapper">
                        <div id="assistview-footer" class="content-editor" contenteditable="true" placeholder="Click to speak or start typing..." @oninput="@UpdateContent" @onkeydown="@OnKeyDown" @ref="@EditableDiv">@AssistViewFooterValue</div>
                        <div class="option-container">
                            <SfSpeechToText ID="speechToText" TranscriptChanging="@OnTranscriptChange" SpeechRecognitionStopped="@HandleStopRecognition"
                            CssClass="@($"e-flat {SpeechToTextCssClass}")"></SfSpeechToText>
                            <SfButton ID="assistview-sendButton" IconCss="e-assist-send e-icons" CssClass="@ButtonCssClass" @onclick="SendButtonClicked"></SfButton>
                        </div>
                    </div>
                </FooterTemplate>
                <BannerTemplate>
                    <div class="banner-content">
                        <div class="e-icons e-listen-icon"></div>
                        <i>Click the below mic-button to convert your voice to text.</i>
                    </div>
                </BannerTemplate>
            </AssistView>
        </AssistViews>
        <AssistViewToolbar ItemClicked="ToolbarItemClicked">
            <AssistViewToolbarItem Type="ItemType.Spacer"></AssistViewToolbarItem>
            <AssistViewToolbarItem IconCss="e-icons e-refresh"></AssistViewToolbarItem>
        </AssistViewToolbar>
        <PromptToolbar ItemClicked="PromptToolbarItemClicked"></PromptToolbar>
    </SfAIAssistView>
</div>

@code {
    private SfAIAssistView assistView;
    private string finalResponse { get; set; }
    private string AssistViewFooterValue = String.Empty;
    private ElementReference EditableDiv;
    private string FooterContent = String.Empty;
    private string SpeechToTextCssClass = "visible";
    private string ButtonCssClass = String.Empty;

    private async void OnTranscriptChange(TranscriptChangeEventArgs args)
    {
        AssistViewFooterValue = args.Transcript;
        await JSRuntime.InvokeVoidAsync("updateContentEditableDiv", EditableDiv, AssistViewFooterValue);
        await InvokeAsync(StateHasChanged);
    }
    private async Task UpdateContent()
    {
        FooterContent = await JSRuntime.InvokeAsync<String>("isFooterContainsValue", EditableDiv);
        ToggleVisibility();
    }
    private async Task HandleStopRecognition()
    {
        FooterContent = AssistViewFooterValue;
        ToggleVisibility();
        await InvokeAsync(StateHasChanged);
    }
    private void ToggleVisibility()
    {
        ButtonCssClass = string.IsNullOrWhiteSpace(FooterContent) ? "" : "visible";
        SpeechToTextCssClass = string.IsNullOrWhiteSpace(FooterContent) ? "visible" : "";
    }
    private async Task PromptRequest(AssistViewPromptRequestedEventArgs args)
    {
        AssistViewFooterValue = String.Empty;
        await JSRuntime.InvokeVoidAsync("emptyFooterValue", EditableDiv);
        await UpdateContent();
        var lastIdx = assistView.Prompts.Count - 1;
        assistView.Prompts[lastIdx].Response = string.Empty;
        finalResponse = string.Empty;
        try
        {
            await foreach (var chunk in OpenAIService.GetChatResponseStreamAsync(args.Prompt))
            {
                await UpdateResponse(args, chunk);
            }

            args.Response = finalResponse;
        }
        catch (Exception ex)
        {
            args.Response = $"Error: {ex.Message}";
        }
        ToggleVisibility();
    }

    private async Task UpdateResponse(AssistViewPromptRequestedEventArgs args, string response)
    {
        var lastIdx = assistView.Prompts.Count - 1;
        await Task.Delay(30); // Small delay for UI updates
        assistView.Prompts[lastIdx].Response += response.Replace("\n", "<br>");
        finalResponse = assistView.Prompts[lastIdx].Response;
        StateHasChanged();
    }

    private async Task SendButtonClicked()
    {
        await assistView.ExecutePromptAsync(FooterContent);
    }
    private void ToolbarItemClicked(AssistViewToolbarItemClickedEventArgs args)
    {
        if (args.Item.IconCss == "e-icons e-refresh")
        {
            assistView.Prompts.Clear();
        }
    }
    private async Task OnKeyDown(KeyboardEventArgs e)
    {
        if (e.Key == "Enter" && !e.ShiftKey)
        {
            await SendButtonClicked();
        }
    }
    private async void PromptToolbarItemClicked(AssistViewToolbarItemClickedEventArgs args)
    {
        if (args.Item.IconCss == "e-icons e-assist-edit") {
            AssistViewFooterValue = assistView.Prompts[args.DataIndex].Prompt;
            await JSRuntime.InvokeVoidAsync("updateContentEditableDiv", EditableDiv, AssistViewFooterValue);
            await UpdateContent();
        }
    }
}
```

**JavaScript Helper Functions (speechtotext.js):**

```javascript
// Checks if the content editable element contains meaningful text and cleans up.
function isFooterContainsValue(elementref) {
    if (!elementref.innerText.trim() !== '') {
        if ((elementref.innerHTML === '<br>' || elementref.innerHTML.trim() === '')) {
            elementref.innerHTML = '';
        }
    }
    return elementref.innerText || "";
}

// Clears the text content of a content editable element.
function emptyFooterValue(elementref) {
    if (elementref) {
        elementref.innerText = "";
    }
}

// Updates the text content of a content editable element with a specified value.
function updateContentEditableDiv(element, value) {
    if (element) {
        element.innerText = value;
    }
}
```

**CSS Styling (speechtotext.css):**

```css
.integration-speechtotext-section {
    height: 350px;
    width: 650px;
    margin: 0 auto;
}

.integration-speechtotext-section .banner-content .e-listen-icon:before {
    font-size: 25px;
}

.integration-speechtotext-section .e-view-container {
    margin: auto;
}

.integration-speechtotext-section .banner-content {
    display: flex;
    flex-direction: column;
    gap: 10px;
    text-align: center;
}

.integration-speechtotext-section #assistview-sendButton {
    width: 40px;
    height: 40px;
    font-size: 20px;
    border: none;
    background: none;
    cursor: pointer;
}

.integration-speechtotext-section #speechToText.visible,
.integration-speechtotext-section #assistview-sendButton.visible {
    display: inline-block;
}

.integration-speechtotext-section #speechToText,
.integration-speechtotext-section #assistview-sendButton {
    display: none;
}

@media only screen and (max-width: 750px) {
    .integration-speechtotext-section {
        width: 100%;
    }
}

.integration-speechtotext-section .e-footer-wrapper {
    display: flex;
    border: 1px solid #c1c1c1;
    padding: 5px 5px 5px 10px;
    margin: 5px 5px 0 5px;
    border-radius: 30px;
}

.integration-speechtotext-section .content-editor {
    width: 100%;
    overflow-y: auto;
    font-size: 14px;
    min-height: 25px;
    max-height: 200px;
    padding: 10px;
}

.integration-speechtotext-section .content-editor[contentEditable=true]:empty:before {
    content: attr(placeholder);
    color: #6b7280;
}

.integration-speechtotext-section .option-container {
    align-self: flex-end;
}
```

## Speech-to-Text Configuration Options

* **`Language`**: Specifies the language for speech recognition. For example:
    * `en-US` for American English
    * `fr-FR` for French

* **`AllowInterimResults`**: Set to `true` to receive real-time (interim) recognition results, or `false` to receive only final results.

The `speechtotext.js` file handles operations related to the content of the editable footer, such as checking for meaningful input, clearing existing text, and updating the content with the transcribed value. Meanwhile, the `speechtotext.css` file styles the AI AssistView layout and ensures the component remains responsive across different screen sizes and devices.

## Error Handling

The `SpeechToText` component provides events to handle errors that may occur during speech recognition. For more information, refer to the [Error Handling](https://blazor.syncfusion.com/documentation/speech-to-text/speech-recognition#error-handling) section in the documentation.

## Browser Compatibility

The `SpeechToText` component relies on the [Speech Recognition API](https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition), which has limited browser support. Refer to the [Browser Compatibility](https://blazor.syncfusion.com/documentation/speech-to-text/speech-recognition#browser-support) section for detailed information.

