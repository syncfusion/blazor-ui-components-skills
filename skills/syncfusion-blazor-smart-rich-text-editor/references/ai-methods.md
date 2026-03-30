# AssistViewSettings Methods

All methods are async (`Task`-returning). Obtain a reference via `@ref` on `<AssistViewSettings>`.

## Table of Contents
- [Setup: Getting a Reference](#setup-getting-a-reference)
- [ShowAIPopupAsync](#showaipopupasync)
- [HideAIPopupAsync](#hideaipopupasync)
- [ExecuteAIPromptAsync](#executeaipromptasync)
- [UpdateAIResponseAsync](#updateairesponseasync)
- [GetAIPromptHistoryAsync](#getaiprompthistoryasync)
- [ClearAIPromptHistoryAsync](#clearaiprompthistoryasync)
- [Complete Example](#complete-example)

---

## Setup: Getting a Reference

```razor
<SfSmartRichTextEditor>
    <AssistViewSettings @ref="AssistViewRef" Placeholder="Ask AI..." />
</SfSmartRichTextEditor>

@code {
    private AssistViewSettings AssistViewRef;
}
```

---

## ShowAIPopupAsync

**Signature:** `Task ShowAIPopupAsync()`

Opens the AI Assistant popup and starts a new conversation session.

```razor
private async Task OpenPopup()
{
    await AssistViewRef.ShowAIPopupAsync();
}
```

---

## HideAIPopupAsync

**Signature:** `Task HideAIPopupAsync()`

Closes the AI Assistant popup.

```razor
private async Task ClosePopup()
{
    await AssistViewRef.HideAIPopupAsync();
}
```

---

## ExecuteAIPromptAsync

**Signature:** `Task ExecuteAIPromptAsync(string prompt)`

Sends a prompt to the AI as if the user typed and submitted it. Useful for automating AI workflows from buttons or application logic.

**Parameters:**
- `prompt` — the text to send

```razor
private async Task RunAutoPrompt()
{
    await AssistViewRef.ExecuteAIPromptAsync("Write a professional summary about AI in healthcare");
}
```

---

## UpdateAIResponseAsync

**Signature:** `Task UpdateAIResponseAsync(string outputResponse, bool isFinalUpdate = false)`

Streams or injects text into the current AI response display. Call repeatedly with chunks while streaming; set `isFinalUpdate = true` when streaming is complete (this hides the "Stop" button).

**Parameters:**
- `outputResponse` — a text chunk or the full response string
- `isFinalUpdate` — `true` signals that streaming is finished (default: `false`)

```razor
// Inject a full custom response at once
private async Task InjectCustomResponse()
{
    string response = "This is a custom AI-generated response injected programmatically.";
    await AssistViewRef.UpdateAIResponseAsync(response, isFinalUpdate: true);
}

// Simulate streaming in chunks
private async Task StreamResponse()
{
    string[] chunks = { "This is ", "a streamed ", "response." };
    foreach (var chunk in chunks)
    {
        await AssistViewRef.UpdateAIResponseAsync(chunk);
        await Task.Delay(100); // simulate streaming delay
    }
    await AssistViewRef.UpdateAIResponseAsync("", isFinalUpdate: true);
}
```

---

## GetAIPromptHistoryAsync

**Signature:** `Task<AssistViewPrompt[]> GetAIPromptHistoryAsync()`

Returns all saved prompts and responses from the conversation history in chronological order (oldest first). History is capped at `MaxPromptHistory` (default: 20).

**Returns:** `AssistViewPrompt[]` with fields:
- `Prompt` — `string`: original user prompt
- `Response` — `string`: AI response (Markdown converted to HTML)
- `IsResponseHelpful` — `bool?`: user feedback flag
- `AttachedFiles` — `List<AssistViewAttachment>?`: files attached to the prompt

```razor
private async Task ExportHistory()
{
    AssistViewPrompt[] history = await AssistViewRef.GetAIPromptHistoryAsync();
    foreach (var entry in history)
    {
        Console.WriteLine($"Q: {entry.Prompt}");
        Console.WriteLine($"A: {entry.Response}");
    }
}
```

---

## ClearAIPromptHistoryAsync

**Signature:** `Task ClearAIPromptHistoryAsync()`

Deletes all conversation history and resets the AI popup to a clean state.

```razor
private async Task ClearHistory()
{
    await AssistViewRef.ClearAIPromptHistoryAsync();
}
```

---

## Complete Example

Demonstrates all six methods wired to buttons:

```razor
@using Syncfusion.Blazor.SmartRichTextEditor
@using Syncfusion.Blazor.InteractiveChat
@using Syncfusion.Blazor.Buttons

<div style="display: flex; gap: 8px; padding-bottom: 15px; flex-wrap: wrap;">
    <SfButton @onclick="ShowPopup">Show Popup</SfButton>
    <SfButton @onclick="ExecutePrompt">Execute Prompt</SfButton>
    <SfButton @onclick="InjectResponse">Inject Response</SfButton>
    <SfButton @onclick="GetHistory">Get History</SfButton>
    <SfButton @onclick="ClearHistory">Clear History</SfButton>
    <SfButton @onclick="HidePopup">Hide Popup</SfButton>
</div>

<SfSmartRichTextEditor>
    <AssistViewSettings @ref="AssistViewRef"
                        Placeholder="Ask AI to enhance your content..."
                        MaxPromptHistory="10" />
</SfSmartRichTextEditor>

@code {
    private AssistViewSettings AssistViewRef;

    private async void ShowPopup()    => await AssistViewRef.ShowAIPopupAsync();
    private async void HidePopup()    => await AssistViewRef.HideAIPopupAsync();
    private async void ClearHistory() => await AssistViewRef.ClearAIPromptHistoryAsync();

    private async void ExecutePrompt()
    {
        await AssistViewRef.ExecuteAIPromptAsync("Write a professional summary about AI");
    }

    private async void InjectResponse()
    {
        await AssistViewRef.UpdateAIResponseAsync(
            "This is a custom response injected via UpdateAIResponseAsync().",
            isFinalUpdate: true
        );
    }

    private async void GetHistory()
    {
        AssistViewPrompt[] history = await AssistViewRef.GetAIPromptHistoryAsync();
        foreach (var item in history)
            Console.WriteLine($"[{item.Prompt}] → [{item.Response}]");
    }
}
```
