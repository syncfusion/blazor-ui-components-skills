# AssistViewSettings Events

## Table of Contents
- [AIPromptRequested](#aipromprequested)
- [AIResponseStopped](#airesponsestopped)
- [AIToolbarItemClicked](#aitoolbaritemclicked)
- [AIPopupOpening](#aipopupopening)
- [AIPopupClosing](#aipopupclosing)

All events are bound directly on the `<AssistViewSettings>` child component.

---

## AIPromptRequested

**Type:** `EventCallback<AssistViewPromptRequestedEventArgs>`  
**Namespace:** `Syncfusion.Blazor.InteractiveChat`

Fires when the user submits a prompt. Use this to intercept, modify, or cancel the prompt before it reaches the AI service.

**EventArgs properties:**

| Property              | Type                          | Description |
|-----------------------|-------------------------------|-------------|
| `Cancel`              | `bool`                        | Set `true` to prevent the prompt from being sent to AI |
| `Prompt`              | `string`                      | The user's prompt text (can be modified) |
| `Response`            | `string`                      | Set a pre-built response to skip the AI call entirely |
| `PromptSuggestions`   | `List<string>`                | Override suggestion chips for the next turn |
| `ResponseToolbarItems`| `List<AssistViewToolbarItem>` | Override toolbar items shown on the response |

```razor
@using Syncfusion.Blazor.SmartRichTextEditor
@using Syncfusion.Blazor.InteractiveChat

<SfSmartRichTextEditor>
    <AssistViewSettings AIPromptRequested="OnAIPrompt" />
</SfSmartRichTextEditor>

@code {
    private async Task OnAIPrompt(AssistViewPromptRequestedEventArgs args)
    {
        // Log or audit the prompt
        Console.WriteLine($"Prompt: {args.Prompt}");

        // To cancel the AI call:
        // args.Cancel = true;

        // To short-circuit with a pre-built response:
        // args.Response = "Here is a predefined answer.";
    }
}
```

---

## AIResponseStopped

**Type:** `EventCallback<ResponseStoppedEventArgs>`  
**Namespace:** `Syncfusion.Blazor.InteractiveChat`

Fires when the user clicks the **"Stop Responding"** button during an active AI stream.

**EventArgs properties:**

| Property    | Type     | Description |
|-------------|----------|-------------|
| `DataIndex` | `int`    | Zero-based index of the active prompt in the conversation. `-1` = not applicable |
| `Prompt`    | `string` | The prompt text that was being responded to |

```razor
@using Syncfusion.Blazor.SmartRichTextEditor
@using Syncfusion.Blazor.InteractiveChat

<SfSmartRichTextEditor>
    <AssistViewSettings AIResponseStopped="OnResponseStopped" />
</SfSmartRichTextEditor>

@code {
    private async Task OnResponseStopped(ResponseStoppedEventArgs args)
    {
        Console.WriteLine($"Stopped at index {args.DataIndex}, prompt: {args.Prompt}");
    }
}
```

---

## AIToolbarItemClicked

**Type:** `EventCallback<AssistViewToolbarItemClickedEventArgs>`  
**Namespace:** `Syncfusion.Blazor.InteractiveChat`

Fires when the user clicks a custom toolbar item inside the AI popup (header, prompt, or response toolbars).

**EventArgs properties:**

| Property    | Type                   | Description |
|-------------|------------------------|-------------|
| `Cancel`    | `bool`                 | Set `true` to cancel the default click action |
| `DataIndex` | `int`                  | Index of the AI response item. `-1` = not applicable |
| `Event`     | `MouseEventArgs`       | The underlying mouse event |
| `Item`      | `AssistViewToolbarItem`| The clicked toolbar item (contains `Text`, `IconCss`, `Tooltip`, etc.) |

```razor
@using Syncfusion.Blazor.SmartRichTextEditor
@using Syncfusion.Blazor.InteractiveChat

<SfSmartRichTextEditor>
    <AssistViewSettings AIToolbarItemClicked="OnToolbarClick" />
</SfSmartRichTextEditor>

@code {
    private async Task OnToolbarClick(AssistViewToolbarItemClickedEventArgs args)
    {
        if (args.Item.Text == "Insert")
        {
            // Handle custom "Insert" button click
            Console.WriteLine("Insert clicked");
        }
    }
}
```

---

## AIPopupOpening

**Type:** `EventCallback<BeforeOpenEventArgs>`  
**Namespace:** `Syncfusion.Blazor.Popups`

Fires before the AI Assistant popup opens. Set `args.Cancel = true` to prevent it from opening (e.g., for validation or access control).

**EventArgs properties:**

| Property    | Type               | Description |
|-------------|--------------------|-------------|
| `Cancel`    | `bool`             | Set `true` to prevent the popup from opening |
| `Element`   | `ElementReference` | Reference to the popup DOM element |

```razor
@using Syncfusion.Blazor.SmartRichTextEditor
@using Syncfusion.Blazor.Popups

<SfSmartRichTextEditor>
    <AssistViewSettings AIPopupOpening="OnPopupOpening" />
</SfSmartRichTextEditor>

@code {
    private async Task OnPopupOpening(BeforeOpenEventArgs args)
    {
        // Prevent opening if user is not authenticated
        if (!UserIsAuthenticated)
        {
            args.Cancel = true;
        }
    }

    private bool UserIsAuthenticated => true; // Replace with real check
}
```

---

## AIPopupClosing

**Type:** `EventCallback<BeforeCloseEventArgs>`  
**Namespace:** `Syncfusion.Blazor.Popups`

Fires before the AI Assistant popup closes. Use to prompt the user to save work or confirm exit.

**EventArgs properties:**

| Property       | Type       | Description |
|----------------|------------|-------------|
| `Cancel`       | `bool`     | Set `true` to prevent the popup from closing |
| `ClosedBy`     | `string`   | Reason for close: `"CloseIcon"`, `"Escape"`, `"OverlayClick"` |
| `Event`        | `EventArgs`| The underlying event that triggered the close |
| `IsInteracted` | `bool`     | `true` if triggered by user interaction; `false` if programmatic |
| `PreventFocus` | `bool`     | When `true`, suppresses focus restoration after close |

```razor
@using Syncfusion.Blazor.SmartRichTextEditor
@using Syncfusion.Blazor.Popups

<SfSmartRichTextEditor>
    <AssistViewSettings AIPopupClosing="OnPopupClosing" />
</SfSmartRichTextEditor>

@code {
    private async Task OnPopupClosing(BeforeCloseEventArgs args)
    {
        // Block accidental Escape key closing
        if (args.ClosedBy == "Escape" && args.IsInteracted)
        {
            args.Cancel = true;
        }
    }
}
```
