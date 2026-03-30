# AssistViewSettings Properties

## Table of Contents
- [Commands](#commands)
- [PopupMaxHeight / PopupWidth](#popupmaxheight--popupwidth)
- [Placeholder](#placeholder)
- [Prompts](#prompts)
- [Suggestions](#suggestions)
- [MaxPromptHistory](#maxprompthistory)
- [BannerTemplate](#bannertemplate)
- [HeaderToolbarSettings](#headertoolbarsettings)
- [PromptToolbarSettings](#prompttoolbarsettings)
- [ResponseToolbarSettings](#responsetoolbarsettings)

---

## Commands

**Type:** `List<AICommands>`  
**Default:** empty list

Predefined AI actions displayed in the Smart Action dropdown toolbar. Each `AICommands` entry supports:
- `Text` — display label shown in the UI
- `Prompt` — text sent to the AI when selected (the editor automatically appends " for the selected content" or " for the document content" as context)
- `IconCss` — optional CSS class for an icon
- `Items` — nested `List<AICommands>` for multi-level sub-menus (recursive)

**Important:** The system automatically appends contextual information to your prompt. Do not end your `Prompt` with a period (`.`) if you want clean formatting, as it will result in `". for the selected content"` instead of `" for the selected content"`.

```razor
@using Syncfusion.Blazor.SmartRichTextEditor

<SfSmartRichTextEditor>
    <AssistViewSettings Commands="@MyCommands" />
</SfSmartRichTextEditor>

@code {
    private List<AICommands> MyCommands = new()
    {
        // ✓ Correct: No period at the end - results in "Make this shorter for the selected content"
        new AICommands { Text = "Shorten",  Prompt = "Make this shorter" },
        
        // ✓ Correct: No period - results in "Add more details for the selected content"
        new AICommands { Text = "Expand",   Prompt = "Add more details" },
        
        // ✗ Avoid: Ending with period - results in "Improve clarity. for the selected content"
        // new AICommands { Text = "Improve", Prompt = "Improve clarity." },
        
        new AICommands
        {
            Text = "Translate",
            Items = new List<AICommands>
            {
                new AICommands { Text = "To French",  Prompt = "Translate to French" },
                new AICommands { Text = "To Spanish", Prompt = "Translate to Spanish" }
            }
        }
    };
}
```

> The AI prompt automatically includes editor context (selected text or full document) — you do not need to add `{{selection}}` placeholders. The system appends " for the selected content" or " for the document content" to your prompt automatically.

---

## PopupMaxHeight / PopupWidth

**PopupMaxHeight** — `string` — default: `"400"` (pixels)  
**PopupWidth** — `string` — default: `"600"` (pixels)

Control the size of the AI Assistant popup. Accepts CSS values (`"80vh"`, `"650px"`) or plain numbers treated as pixels.

```razor
<SfSmartRichTextEditor>
    <AssistViewSettings PopupMaxHeight="80vh" PopupWidth="650px" />
</SfSmartRichTextEditor>
```

---

## Placeholder

**Type:** `string`  
**Default:** `"Ask AI to rewrite or generate content."`

Placeholder text shown in the AI prompt textarea when empty.

```razor
<SfSmartRichTextEditor>
    <AssistViewSettings Placeholder="How can I improve this document?" />
</SfSmartRichTextEditor>
```

---

## Prompts

**Type:** `List<AssistViewPrompt>`  
**Default:** empty list

Preloads the AI conversation with predefined prompt/response pairs. Useful for starter workflows or demonstration content.

`AssistViewPrompt` fields:
- `Prompt` — `string`: the user prompt text
- `Response` — `string`: the pre-set AI response (supports Markdown/HTML)

```razor
@using Syncfusion.Blazor.SmartRichTextEditor
@using Syncfusion.Blazor.InteractiveChat

<SfSmartRichTextEditor>
    <AssistViewSettings Prompts="@TemplatePrompts" />
</SfSmartRichTextEditor>

@code {
    private List<AssistViewPrompt> TemplatePrompts = new()
    {
        new AssistViewPrompt
        {
            Prompt   = "Draft a professional email",
            Response = "Subject: Hello Team\n\nDear Team,\n\nI hope this message finds you well.\n\nBest regards"
        },
        new AssistViewPrompt
        {
            Prompt   = "Create API documentation",
            Response = "### GET /users\nRetrieves a list of users.\n\n**Request**\nGET /api/users"
        }
    };
}
```

---

## Suggestions

**Type:** `List<string>`  
**Default:** empty list

Quick-access suggestion chips displayed in the AI popup. Clicking a chip sends it as a prompt instantly.

```razor
@using Syncfusion.Blazor.SmartRichTextEditor

<SfSmartRichTextEditor>
    <AssistViewSettings Suggestions="@QuickSuggestions" />
</SfSmartRichTextEditor>

@code {
    private List<string> QuickSuggestions = new()
    {
        "Make shorter",
        "Improve clarity",
        "Fix grammar",
        "Add examples",
        "More formal",
        "Simplify"
    };
}
```

---

## MaxPromptHistory

**Type:** `int`  
**Default:** `20`

Maximum conversation entries retained in the popup history. When exceeded, oldest entries are removed automatically. History persists across open/close cycles of the popup.

```razor
<SfSmartRichTextEditor>
    <!-- Keep only 5 recent conversations -->
    <AssistViewSettings MaxPromptHistory="5" />
</SfSmartRichTextEditor>
```

---

## BannerTemplate

**Type:** `RenderFragment`  
**Default:** none

Custom template for the banner area at the top of the AI popup. Use for branding, status messages, or usage instructions.

```razor
@using Syncfusion.Blazor.SmartRichTextEditor

<SfSmartRichTextEditor>
    <AssistViewSettings>
        <BannerTemplate>
            <div style="padding: 16px; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); color: white;">
                <h3 style="margin: 0;">Smart AI Assistant</h3>
                <span style="font-size: 12px;">Real-time AI assistance</span>
            </div>
        </BannerTemplate>
    </AssistViewSettings>
</SfSmartRichTextEditor>
```

---

## HeaderToolbarSettings

**Type:** `RenderFragment`  
**Default:** none

Configures toolbar items in the **header section** of the AI popup.

Each `AssistViewToolbarItem` supports:
| Property  | Type           | Default          |
|-----------|----------------|------------------|
| `Text`    | `string`       | `String.Empty`   |
| `IconCss` | `string`       | `String.Empty`   |
| `Tooltip` | `string`       | `""`             |
| `Type`    | `ItemType`     | `ItemType.Button`|
| `CssClass`| `string`       | `""`             |
| `Disabled`| `bool`         | `false`          |
| `Visible` | `bool`         | `true`           |
| `TabIndex`| `int`          | —                |
| `Template`| `RenderFragment` | `null`         |

```razor
@using Syncfusion.Blazor.SmartRichTextEditor
@using Syncfusion.Blazor.InteractiveChat
@using Syncfusion.Blazor.Navigations

<SfSmartRichTextEditor>
    <AssistViewSettings>
        <HeaderToolbarSettings>
            <AssistViewToolbarItem Type="ItemType.Spacer" />
            <AssistViewToolbarItem Text="Close"       IconCss="e-icons e-close" />
            <AssistViewToolbarItem Text="AI Commands" />
        </HeaderToolbarSettings>
    </AssistViewSettings>
</SfSmartRichTextEditor>
```

---

## PromptToolbarSettings

**Type:** `RenderFragment`  
**Default:** none

Configures toolbar items **below the prompt input area**.

```razor
@using Syncfusion.Blazor.SmartRichTextEditor
@using Syncfusion.Blazor.InteractiveChat
@using Syncfusion.Blazor.Navigations

<SfSmartRichTextEditor>
    <AssistViewSettings>
        <PromptToolbarSettings>
            <PromptToolbarItem Text="Edit"  IconCss="e-icons e-assist-edit"  Tooltip="Edit prompt" />
            <PromptToolbarItem Text="Copy"  IconCss="e-icons e-assist-copy"  Tooltip="Copy to clipboard" />
            <PromptToolbarItem Type="ItemType.Separator" />
            <PromptToolbarItem Text="Save"  IconCss="e-icons e-save" />
        </PromptToolbarSettings>
    </AssistViewSettings>
</SfSmartRichTextEditor>
```

---

## ResponseToolbarSettings

**Type:** `RenderFragment`  
**Default:** none

Configures toolbar items in the **AI response viewer section**. Supports `Template` for fully custom buttons.

```razor
@using Syncfusion.Blazor.SmartRichTextEditor
@using Syncfusion.Blazor.InteractiveChat
@using Syncfusion.Blazor.Navigations

<SfSmartRichTextEditor>
    <AssistViewSettings>
        <ResponseToolbarSettings>
            <ResponseToolbarItem Text="Regenerate" IconCss="e-icons e-refresh" />
            <ResponseToolbarItem Text="Copy"       IconCss="e-icons e-copy" />
            <ResponseToolbarItem Type="ItemType.Separator" />
            <ResponseToolbarItem Text="Insert"     IconCss="e-icons e-check" />
            <ResponseToolbarItem>
                <Template>
                    <button onclick="alert('Feedback saved')">👍 Helpful</button>
                </Template>
            </ResponseToolbarItem>
        </ResponseToolbarSettings>
    </AssistViewSettings>
</SfSmartRichTextEditor>
```
