# Accessibility and Globalization

## Table of Contents
1. [Accessibility Overview](#accessibility-overview)
2. [WCAG 2.2 AA Compliance](#wcag-22-aa-compliance)
3. [Keyboard Navigation](#keyboard-navigation)
4. [ARIA Attributes](#aria-attributes)
5. [Screen Reader Support](#screen-reader-support)
6. [Globalization](#globalization)
7. [Real-World Examples](#real-world-examples)

## Accessibility Overview

The Syncfusion Inline AI Assist component provides built-in accessibility features supporting:
- **WCAG 2.2 AA** compliance
- **Keyboard navigation**
- **Screen reader support** with proper ARIA labels
- **High contrast** mode support
- **Right-to-Left (RTL)** language support
- **Localization** for 78+ languages

These features ensure the component is usable by everyone, including users with disabilities.

## WCAG 2.2 AA Compliance

### Compliance Checklist

| Standard | Implementation | Status |
|----------|-----------------|--------|
| 1.4.3 Contrast (Minimum) | Text contrast ≥ 4.5:1 | ✓ Built-in |
| 1.4.11 Non-text Contrast | UI components ≥ 3:1 | ✓ Built-in |
| 2.1.1 Keyboard | All functionality keyboard accessible | ✓ Built-in |
| 2.1.2 No Keyboard Trap | Tab order logical, trap exits | ✓ Built-in |
| 2.4.3 Focus Order | Logical focus sequence | ✓ Built-in |
| 2.4.7 Focus Visible | Visual focus indicator | ✓ Built-in |
| 4.1.2 Name, Role, Value | ARIA semantics present | ✓ Built-in |
| 4.1.3 Status Messages | Live regions for updates | ✓ Built-in |

### High Contrast Mode

The component automatically adapts to high contrast themes:

```razor
<SfInlineAIAssist CSSClass="high-contrast-mode">
</SfInlineAIAssist>

<style>
    .high-contrast-mode {
        border: 2px solid black;
        color: black;
        background-color: white;
    }
    
    .high-contrast-mode button {
        border: 2px solid black;
        font-weight: bold;
    }
</style>
```

**Testing High Contrast:**
1. Windows: Settings → Ease of Access → High Contrast
2. macOS: System Preferences → Accessibility → Display → Increase Contrast
3. Browser: DevTools → Rendering → Emulate CSS media feature prefers-contrast

## Keyboard Navigation

### Default Keyboard Support

| Key | Action | Context |
|-----|--------|---------|
| **Tab** | Navigate focus forward | Global |
| **Shift+Tab** | Navigate focus backward | Global |
| **Enter** | Activate button/item | Popup, Toolbar |
| **Space** | Activate button/item | Popup, Toolbar |
| **Escape** | Close popup | Popup visible |
| **Arrow Up** | Previous item in menu | Dropdown open |
| **Arrow Down** | Next item in menu | Dropdown open |
| **Home** | First item in menu | Dropdown open |
| **End** | Last item in menu | Dropdown open |
| **Alt+↑** | Open popup | Global (optional) |
| **Alt+↓** | Close popup | Popup visible |

### Example: Custom Keyboard Handler

```razor
@page "/keyboard-support"
@using Syncfusion.Blazor.InteractiveChat
@inject IJSRuntime JSRuntime

<SfInlineAIAssist @ref="inlineAssist"
                 PromptRequested="OnPromptRequested"
                 @onkeydown="OnKeyDown">
</SfInlineAIAssist>

@code {
    private SfInlineAIAssist inlineAssist = new();
    private int promptNumber = 0;

    private async Task OnKeyDown(KeyboardEventArgs args)
    {
        // Ctrl+Shift+A opens AI popup
        if (args.CtrlKey && args.ShiftKey && args.Code == "KeyA")
        {
            args.PreventDefault();
            await inlineAssist.ShowPopupAsync();
        }
        
        // Ctrl+Enter submits prompt
        if (args.CtrlKey && (args.Code == "Enter" || args.Code == "NumpadEnter"))
        {
            args.PreventDefault();
            // Trigger prompt submission
        }
    }

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        promptNumber++;
        var response = $"Response #{promptNumber}: {args.Prompt}";
        await inlineAssist.UpdateResponseAsync(response, true);
    }
}
```

### Accessible Focus Management

```razor
<SfInlineAIAssist @ref="inlineAssist"
                 PromptRequested="OnPromptRequested"
                 Opened="OnPopupOpened"
                 Closed="OnPopupClosed">
    <EditorTemplate>
        <input @ref="promptInput" 
               type="text" 
               placeholder="Enter prompt (Focus automatically here)"
               @onkeydown="OnPromptKeyDown"
               aria-label="Prompt input field" />
    </EditorTemplate>
</SfInlineAIAssist>

@code {
    private SfInlineAIAssist inlineAssist = new();
    private ElementReference promptInput;

    private async Task OnPopupOpened(OpenEventArgs args)
    {
        // Auto-focus on prompt input for keyboard users
        await promptInput.FocusAsync();
    }

    private async Task OnPopupClosed(CloseEventArgs args)
    {
        // Return focus to trigger button
        // Implementation depends on your trigger element
    }

    private async Task OnPromptKeyDown(KeyboardEventArgs args)
    {
        if (args.Code == "Enter" && !args.ShiftKey)
        {
            args.PreventDefault();
            // Submit prompt
        }
    }

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        var response = await GetAIResponse(args.Prompt);
        await inlineAssist.UpdateResponseAsync(response, true);
    }

    private async Task<string> GetAIResponse(string prompt)
    {
        return "Response: " + prompt;
    }
}
```

## ARIA Attributes

### Built-in ARIA Support

The component automatically includes the following WAI-ARIA attributes:

| Attribute | Purpose |
|-----------|---------|
| `role=button` | Indicates that the element is clickable and triggers an action when activated by the user. |
| `role=toolbar` | Specifies that the element is a toolbar. |
| `aria-label` | Defines a string value that labels an interactive element for accessibility. |
| `aria-orientation` | Specifies the orientation of the toolbar. |
| `aria-disabled` | Indicates whether the toolbar or element is currently disabled and not interactive. |
| `aria-multiline` | Indicates that a textbox accepts multiple lines of input or only a single line. |

### Custom ARIA Labels

```razor
<SfInlineAIAssist @ref="inlineAssist"
                 PromptRequested="OnPromptRequested">
    <EditorTemplate>
        <textarea @bind="prompt"
                 aria-label="Enter your prompt for the AI assistant"
                 aria-describedby="promptHelp"
                 placeholder="Describe what you need..."></textarea>
        <div id="promptHelp" style="font-size: 0.85em; color: #666;">
            Provide clear instructions for the AI assistant
        </div>
    </EditorTemplate>
    
    <ResponseTemplate>
        <div role="region" 
             aria-live="assertive" 
             aria-label="AI Assistant Response">
            @((MarkupString)context.Response)
        </div>
    </ResponseTemplate>
    
    <ResponseActions Items="responseItems" ItemSelect="OnItemSelect">
    </ResponseActions>
</SfInlineAIAssist>

@code {
    private string prompt = "";
    private List<ResponseItem> responseItems = new();

    private async Task OnItemSelect(ResponseItemSelectEventArgs args)
    {
        // Handle item selection
    }

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        var response = await GetAIResponse(args.Prompt);
        await inlineAssist.UpdateResponseAsync(response, true);
    }

    private async Task<string> GetAIResponse(string prompt)
    {
        return "Response: " + prompt;
    }
}
```

### ARIA Live Regions

Use live regions for dynamic content updates:

```razor
<SfInlineAIAssist PromptRequested="OnPromptRequested">
    <ResponseTemplate>
        <!-- For polite announcements (default) -->
        <div role="region" aria-live="polite">
            Standard response announcement
        </div>
        
        <!-- For urgent announcements -->
        <div role="region" aria-live="assertive">
            Error message - requires immediate attention
        </div>
        
        <!-- For off-screen announcements -->
        <div role="region" aria-live="polite" class="sr-only">
            Detailed information for screen readers
        </div>
    </ResponseTemplate>
</SfInlineAIAssist>

<style>
    .sr-only {
        position: absolute;
        width: 1px;
        height: 1px;
        padding: 0;
        margin: -1px;
        overflow: hidden;
        clip: rect(0, 0, 0, 0);
        white-space: nowrap;
        border-width: 0;
    }
</style>
```

## Screen Reader Support

### Testing Screen Readers

| Platform | Reader | Download |
|----------|--------|----------|
| Windows | NVDA | https://www.nvaccess.org |
| Windows | JAWS | https://www.freedomscientific.com |
| macOS | VoiceOver | Built-in (Cmd+F5) |
| iOS | VoiceOver | Settings > Accessibility |
| Android | TalkBack | Google Play Store |

### Screen Reader Best Practices

```razor
@page "/screen-reader-friendly"
@using Syncfusion.Blazor.InteractiveChat

<SfInlineAIAssist @ref="inlineAssist"
                 PromptRequested="OnPromptRequested">
    
    <EditorTemplate>
        <!-- 1. Use descriptive labels -->
        <label for="aiPrompt" class="sr-label">Enter your request for AI assistance</label>
        <textarea id="aiPrompt" 
                 @bind="prompt"
                 aria-describedby="promptHint"
                 placeholder="What do you need help with?"></textarea>
        
        <!-- 2. Provide context -->
        <p id="promptHint" class="sr-only">
            Provide specific details about your request. You can ask for text generation, 
            summarization, code review, or other assistance.
        </p>
    </EditorTemplate>
    
    <ResponseTemplate>
        <!-- 3. Clear structure -->
        <div aria-labelledby="responseTitle">
            <h3 id="responseTitle">AI Response</h3>
            <div>@((MarkupString)context.Response)</div>
        </div>
        
        <!-- 4. Action instructions -->
        <div class="sr-only">
            Use Tab key to navigate to action buttons. Press Enter to select.
        </div>
    </ResponseTemplate>
</SfInlineAIAssist>

@code {
    private string prompt = "";
    private SfInlineAIAssist inlineAssist = new();

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        var response = await GetAIResponse(args.Prompt);
        await inlineAssist.UpdateResponseAsync(response, true);
    }

    private async Task<string> GetAIResponse(string prompt)
    {
        return "Processing: " + prompt;
    }
}
```

## Globalization

### Supported Languages

The component supports 78+ languages including:
- English, Español, Français, Deutsch, 中文, 日本語
- العربية (Arabic), עברית (Hebrew), ไทย (Thai), 한국어 (Korean)
- Português, Русский, Italiano, and many more

### RTL Support

Enable Right-to-Left for Arabic, Hebrew, Persian, Urdu:

```razor
@page "/rtl-support"
@using Syncfusion.Blazor.InteractiveChat

<!-- HTML Document Level -->
<!DOCTYPE html>
<html dir="rtl" lang="ar">
<head>
    <meta charset="UTF-8">
    <title>مساعد الذكاء الاصطناعي</title>
</head>
<body>
    <component></component>
</body>
</html>
```

**Component-Level RTL:**

```razor
<SfInlineAIAssist @ref="inlineAssist"
                 EnableRtl="true"
                 Locale="ar"
                 PromptRequested="OnPromptRequested">
</SfInlineAIAssist>

@code {
    private SfInlineAIAssist inlineAssist = new();

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        var response = await GetAIResponse(args.Prompt);
        await inlineAssist.UpdateResponseAsync(response, true);
    }

    private async Task<string> GetAIResponse(string prompt)
    {
        return "الرد: " + prompt;
    }
}
```

### Localization

The Inline AI Assist can be localized to any culture by defining the text of the Inline AI Assist in the corresponding culture. The default locale is `en` (English). The following table represents the default text of the Inline AI Assist in `en` culture.

| KEY | Text |
|-----|------|
| `send` | Send |
| `stopResponseText` | Stop Responding |
| `thinkingIndicator` | Thinking |
| `editingIndicator` | Editing |

Refer to the [Blazor Localization](https://blazor.syncfusion.com/documentation/common/localization) documentation for detailed steps on implementing localization in Blazor components.

Change component text through resource files:

**Step 1: Create Resource File**

Create `Resources/Culture.ar.resx`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="Placeholder" xml:space="preserve">
    <value>اكتب رسالتك هنا</value>
  </data>
  <data name="Accept" xml:space="preserve">
    <value>قبول</value>
  </data>
  <data name="Discard" xml:space="preserve">
    <value>رفض</value>
  </data>
  <data name="Regenerate" xml:space="preserve">
    <value>إعادة إنشاء</value>
  </data>
</root>
```

**Step 2: Configure Culture**

```razor
@page "/localization"
@using System.Globalization
@inject IJSRuntime JSRuntime

@if (isLocalizationReady)
{
    <SfInlineAIAssist @ref="inlineAssist"
                     Locale="@currentLocale"
                     PromptRequested="OnPromptRequested">
        <EditorTemplate>
            <textarea placeholder="@GetLocalizedText("Placeholder")"></textarea>
        </EditorTemplate>
    </SfInlineAIAssist>
}

@code {
    private SfInlineAIAssist inlineAssist = new();
    private string currentLocale = "en";
    private bool isLocalizationReady = false;

    protected override async Task OnInitializedAsync()
    {
        var culture = CultureInfo.CurrentCulture;
        currentLocale = culture.Name; // e.g., "ar", "es", "fr"
        isLocalizationReady = true;
    }

    private string GetLocalizedText(string key)
    {
        return key switch
        {
            "Placeholder" => currentLocale == "ar" ? "اكتب رسالتك هنا" : "Enter your message",
            "Accept" => currentLocale == "ar" ? "قبول" : "Accept",
            "Discard" => currentLocale == "ar" ? "رفض" : "Discard",
            _ => key
        };
    }

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        var response = await GetAIResponse(args.Prompt);
        await inlineAssist.UpdateResponseAsync(response, true);
    }

    private async Task<string> GetAIResponse(string prompt)
    {
        return currentLocale == "ar" ? "الرد: " + prompt : "Response: " + prompt;
    }
}
```

**Step 3: Supported Locales**

| Code | Language |
|------|----------|
| en | English |
| ar | العربية (Arabic) |
| es | Español (Spanish) |
| fr | Français (French) |
| de | Deutsch (German) |
| ja | 日本語 (Japanese) |
| zh | 中文 (Chinese) |
| pt | Português (Portuguese) |
| ru | Русский (Russian) |
| he | עברית (Hebrew) |
| ko | 한국어 (Korean) |
| th | ไทย (Thai) |
| tr | Türkçe (Turkish) |
| hi | हिन्दी (Hindi) |

## Real-World Examples

### Example 1: Fully Accessible Setup

```razor
@page "/accessible-ai"
@using Syncfusion.Blazor.InteractiveChat
@using System.Globalization

<div class="sr-only" role="region" aria-live="polite" aria-label="AI Assistant Status">
    @statusMessage
</div>

<main>
    <h1>Accessible AI Assistant</h1>
    
    <p>Use the button below to open the AI assistant. 
       Keyboard shortcut: <kbd>Alt</kbd> + <kbd>A</kbd></p>
    
    <button id="openAiButton" 
            @onclick="OnOpenAI"
            aria-label="Open AI Assistant (Alt+A)">
        Open AI Assistant
    </button>

    <SfInlineAIAssist @ref="inlineAssist"
                     RelateTo="#openAiButton"
                     EnableRtl="@isRtl"
                     Locale="@currentLocale"
                     PromptRequested="OnPromptRequested"
                     Opened="OnOpened"
                     Closed="OnClosed"
                     @onkeydown="OnGlobalKeyDown">
        
        <EditorTemplate>
            <label for="aiPrompt" class="sr-only">
                AI Assistance Prompt
            </label>
            <textarea @ref="promptInput"
                     id="aiPrompt"
                     @bind="prompt"
                     placeholder="@(isRtl ? "أدخل طلبك" : "Enter your request")"
                     aria-describedby="promptHelp"
                     aria-label="@(isRtl ? "حقل إدخال الطلب" : "Prompt input field")">
            </textarea>
            <p id="promptHelp" class="sr-only">
                @(isRtl ? "اشرح طلبك بوضوح" : "Describe your request clearly")
            </p>
        </EditorTemplate>
        
        <ResponseTemplate>
            <div role="region" 
                 aria-live="polite" 
                 aria-label="@(isRtl ? "رد المساعد الذكي" : "AI Response")">
                <strong>@(isRtl ? "الرد:" : "Response:")</strong>
                <p>@((MarkupString)context.Response)</p>
            </div>
        </ResponseTemplate>
        
        <ResponseActions Items="responseItems" ItemSelect="OnItemSelect">
        </ResponseActions>
    </SfInlineAIAssist>
</main>

<style>
    .sr-only {
        position: absolute;
        width: 1px;
        height: 1px;
        padding: 0;
        margin: -1px;
        overflow: hidden;
        clip: rect(0, 0, 0, 0);
        white-space: nowrap;
        border-width: 0;
    }
    
    button:focus {
        outline: 3px solid #4A90E2;
        outline-offset: 2px;
    }
</style>

@code {
    private SfInlineAIAssist inlineAssist = new();
    private ElementReference promptInput;
    private string prompt = "";
    private string statusMessage = "";
    private string currentLocale = "en";
    private bool isRtl = false;
    private List<ResponseItem> responseItems = new()
    {
        new ResponseItem { Label = "Accept", IconCss = "e-icons e-check" },
        new ResponseItem { Label = "Discard", IconCss = "e-icons e-close" }
    };

    protected override async Task OnInitializedAsync()
    {
        currentLocale = CultureInfo.CurrentCulture.Name;
        isRtl = currentLocale is "ar" or "he" or "fa" or "ur";
    }

    private async Task OnOpenAI()
    {
        statusMessage = isRtl ? "فتح مساعد الذكاء الاصطناعي" : "Opening AI Assistant";
        await inlineAssist.ShowPopupAsync();
    }

    private async Task OnOpened(OpenEventArgs args)
    {
        await promptInput.FocusAsync();
        statusMessage = isRtl ? "تم فتح المساعد" : "Assistant opened";
    }

    private async Task OnClosed(CloseEventArgs args)
    {
        statusMessage = isRtl ? "تم إغلاق المساعد" : "Assistant closed";
    }

    private async Task OnGlobalKeyDown(KeyboardEventArgs args)
    {
        if (args.AltKey && args.Code == "KeyA")
        {
            args.PreventDefault();
            await inlineAssist.ShowPopupAsync();
        }
    }

    private async Task OnItemSelect(ResponseItemSelectEventArgs args)
    {
        if (args.Item.Label == "Accept")
        {
            statusMessage = isRtl ? "تم قبول الرد" : "Response accepted";
            await inlineAssist.HidePopupAsync();
        }
        else if (args.Item.Label == "Discard")
        {
            statusMessage = isRtl ? "تم رفض الرد" : "Response discarded";
            await inlineAssist.HidePopupAsync();
        }
    }

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        statusMessage = isRtl ? "جاري معالجة طلبك..." : "Processing your request...";
        var response = await GetAIResponse(args.Prompt);
        statusMessage = isRtl ? "تم استلام الرد" : "Response received";
        await inlineAssist.UpdateResponseAsync(response, true);
    }

    private async Task<string> GetAIResponse(string prompt)
    {
        return isRtl ? "الرد على طلبك: " + prompt : "Response to your request: " + prompt;
    }
}
```

### Example 2: Mobile Accessibility

```razor
@page "/mobile-accessible"
@using Syncfusion.Blazor.InteractiveChat

<SfInlineAIAssist @ref="inlineAssist"
                 PopupWidth="@(isMobile ? "100%" : "500px")"
                 PopupHeight="@(isMobile ? "80vh" : "400px")"
                 PromptRequested="OnPromptRequested">
    <EditorTemplate>
        <textarea @ref="promptInput"
                 style="font-size: 16px; @(isMobile ? "padding: 12px;" : "")"
                 aria-label="AI prompt input">
        </textarea>
    </EditorTemplate>
</SfInlineAIAssist>

@code {
    private SfInlineAIAssist inlineAssist = new();
    private ElementReference promptInput;
    private bool isMobile = false;

    protected override async Task OnInitializedAsync()
    {
        // Detect mobile device (example)
        isMobile = await JSRuntime.InvokeAsync<bool>("window.navigator.userAgentData?.mobile ?? false");
    }

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        var response = await GetAIResponse(args.Prompt);
        await inlineAssist.UpdateResponseAsync(response, true);
    }

    private async Task<string> GetAIResponse(string prompt)
    {
        return "Response: " + prompt;
    }
}
```

## Compliance Summary

| Aspect | Implementation | Verification |
|--------|-----------------|---------------|
| Keyboard Navigation | All features keyboard accessible | Tab through all controls |
| Screen Reader | ARIA labels and live regions | Test with NVDA/JAWS |
| Color Contrast | ≥ 4.5:1 for text | Use WebAIM contrast checker |
| Focus Visible | Clear focus indicators | Check in different browsers |
| Localization | 78+ languages supported | Change culture setting |
| RTL Support | Arabic, Hebrew, Persian | Set dir="rtl" on container |
| Mobile | Responsive design | Test on iPhone/Android |
| WCAG 2.2 AA | Full compliance | Use WAVE tool for audit |

## Resources

- [W3C WCAG 2.2](https://www.w3.org/WAI/WCAG22/quickref/)
- [ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/)
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
- [Syncfusion Accessibility](https://www.syncfusion.com/accessibility)
- [NVDA Screen Reader](https://www.nvaccess.org/)
