# Customization and Response Settings

## Table of Contents
1. [Overview](#overview)
2. [Core Component Properties](#core-component-properties)
3. [Response Modes - ResponseMode Property](#responsemode-property)
4. [Component Dimensions](#component-dimensions)
5. [Response Settings](#response-settings)
6. [Response Items Configuration](#response-items-configuration)
7. [Templates](#templates)
8. [Appearance Customization](#appearance-customization)
9. [Real-World Examples](#real-world-examples)

## Overview

Customize the Inline AI Assist component appearance, response handling, and user interactions using properties, response items, templates, and CSS styling.

## Core Component Properties

### Positioning Properties

#### RelateTo Property

Position the popup relative to a specific DOM element.

```razor
<!-- Position popup near button -->
<SfButton id="aiButton">Launch AI</SfButton>
<SfInlineAIAssist RelateTo="#aiButton">
</SfInlineAIAssist>

<!-- Or use element selector -->
<SfInlineAIAssist RelateTo=".target-element">
</SfInlineAIAssist>
```

**Accepted Values:**
- CSS selector string: `"#elementId"`, `".class-name"`
- Element selector: `"#summarizeButton"`
- Class selector: `".input-container"`

**Default:** `null` (centers on screen)

#### Target Property

Specify where the component container should be appended in the DOM.

```razor
<SfInlineAIAssist RelateTo="#button" Target="#app-container">
</SfInlineAIAssist>
```

**Use Cases:**
- Append to specific container: `Target="#main"`
- Append to body: `Target="body"`
- Append to modal: `Target="#modal-content"`

### Placeholder Property

Set the placeholder text for the prompt input area.

```razor
<SfInlineAIAssist Placeholder="Type your prompt here...">
</SfInlineAIAssist>
```

**Example:**
```razor
<SfInlineAIAssist Placeholder="Enter your prompt (e.g., 'Summarize', 'Translate', 'Rewrite')">
</SfInlineAIAssist>
```

**Default:** `"Enter prompt"`

### EnableStreaming Property

Enable real-time response streaming from AI providers.

```razor
<SfInlineAIAssist EnableStreaming="true">
</SfInlineAIAssist>
```

**Use Cases:**
- Live response updates: `EnableStreaming="true"`
- Wait for complete response: `EnableStreaming="false"`
- Stream with buffer: `EnableStreaming="true"`

**Default:** `false`

**Example: Streaming with Different Providers**

```razor
<SfInlineAIAssist @ref="inlineAssist" 
                 RelateTo="#button"
                 EnableStreaming="true"
                 PromptRequested="OnPromptRequestAsync">
</SfInlineAIAssist>

@code {
    private async Task OnPromptRequestAsync(PromptRequestedEventArgs args)
    {
        // Stream response from OpenAI
        var stream = await openAiClient.CreateChatCompletionStreamAsync(new ChatCompletionCreateParamsNonStreaming
        {
            Model = "gpt-4",
            Messages = new[] { new { role = "user", content = args.Prompt } }
        });

        // Update response as chunks arrive
        var response = "";
        await foreach (var chunk in stream)
        {
            response += chunk.Choices[0].Delta.Content;
            await inlineAssist.UpdateResponseAsync(response);
        }
    }
}
```

### CssClass Property

Apply custom CSS classes for styling.

```razor
<SfInlineAIAssist CssClass="custom-inline-assist dark-theme">
</SfInlineAIAssist>
```

**Style Example:**
```css
.custom-inline-assist {
    font-family: 'Segoe UI', sans-serif;
    border-radius: 8px;
}

.custom-inline-assist.dark-theme {
    background-color: #2d2d2d;
    color: #ffffff;
}

.custom-inline-assist .e-inline-editor {
    background-color: #1e1e1e;
    border: 1px solid #404040;
}
```

### ResponseMode Property

The `ResponseMode` property controls how AI responses are displayed. Responses can be shown in two modes: `Inline` (updates content in-place) and `Popup` (shows responses in a floating popup). Toggle this behavior dynamically at runtime.

**Type:** `ResponseDisplayMode` enum  
**Accepted Values:**

| Value | Behavior | Use Case |
|-------|----------|----------|
| `ResponseDisplayMode.Inline` | Updates content in-place within the editor | Direct content editing, inline suggestions |
| `ResponseDisplayMode.Popup` | Shows responses in a floating popup | Review-then-accept workflow, multi-choice responses |

**Default:** `ResponseDisplayMode.Popup`

**Basic Usage:**

```razor
@using Syncfusion.Blazor.InteractiveChat
@using Syncfusion.Blazor.Buttons

<SfButton id="summarizeButton" IsPrimary="true" @onclick="OnSummarizeClick">Content Summarize</SfButton>

<SfInlineAIAssist @ref="inlineAssist" 
                 ResponseMode="@currentResponseMode" 
                 RelateTo="#summarizeButton"
                 PromptRequested="OnPromptRequestAsync">
    <ResponseActions ItemSelect="OnItemSelectAsync"></ResponseActions>
</SfInlineAIAssist>

@code {
    private SfInlineAIAssist inlineAssist = new();
    private ResponseDisplayMode currentResponseMode = ResponseDisplayMode.Popup;
    private string editableContent = "<p>Your editable content here.</p>";

    private async Task OnSummarizeClick()
    {
        await inlineAssist.ShowPopupAsync();
    }

    private async Task OnPromptRequestAsync(PromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        string defaultResponse = "AI-generated response content.";
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
}
```

**Dynamic Switching - Toggle Between Inline and Popup Modes at Runtime:**

```razor
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
    <div style="margin-bottom: 10px;">
        <label for="responseMode">Response Mode:</label>
        <select id="responseMode" @onchange="OnResponseModeChangeAsync">
            <option value="Popup">Popup</option>
            <option value="Inline">Inline</option>
        </select>
    </div>
    <SfButton id="summarizeButton" IsPrimary="true" Style="margin-bottom: 10px;" @onclick="OnSummarizeClick">Content Summarize</SfButton>
    <div id="editableText" contenteditable="true">
        @((MarkupString)editableContent)
    </div>
    <SfInlineAIAssist @ref="inlineAssist" 
                     ResponseMode="@currentResponseMode" 
                     RelateTo="#summarizeButton" 
                     PromptRequested="OnPromptRequestAsync">
        <ResponseActions ItemSelect="OnItemSelectAsync"></ResponseActions>
    </SfInlineAIAssist>
</div>

@code {
    private SfInlineAIAssist inlineAssist = new();
    private ResponseDisplayMode currentResponseMode = ResponseDisplayMode.Popup;
    private string editableContent = @"<p>Inline AI Assist component provides intelligent text processing capabilities that enhance user productivity. It leverages advanced natural language processing to understand context and deliver precise suggestions. Users can seamlessly integrate AI-powered features into their applications.</p>
        <p>With real-time response streaming and customizable prompts, developers can create interactive experiences. The component supports multiple response modes including inline editing and popup-based interactions.</p>";

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

    private async Task OnResponseModeChangeAsync(ChangeEventArgs args)
    {
        if (Enum.TryParse<ResponseDisplayMode>(args.Value?.ToString(), out var mode))
        {
            currentResponseMode = mode;
            await InvokeAsync(StateHasChanged);
            await inlineAssist.ShowPopupAsync();
        }
    }
}
```

> **Note:** Starting from version 33.1x, when a user submits a prompt to the Inline AI Assist, the component automatically scrolls and focuses on the latest prompt and response. This behavior eliminates the need for users to manually scroll down to see the new response. Prior to version 33.1x, previous responses remained visible when new responses were added.

### Toolbar Positioning - ToolbarPosition Property

Configure where the inline toolbar appears in the popup.

```razor
<SfInlineAIAssist>
    <InlineToolbar ToolbarPosition="ToolbarPosition.Bottom">
        <InlineToolbarItems>
            <InlineToolbarItem Text="Send" IconCss="e-icons e-send"></InlineToolbarItem>
        </InlineToolbarItems>
    </InlineToolbar>
</SfInlineAIAssist>
```

**Supported Values:**

| Value | Position | Use Case |
|-------|----------|----------|
| `ToolbarPosition.Inline` | Above the input area | Compact, integrated design |
| `ToolbarPosition.Bottom` | Bottom footer area | Separate dedicated toolbar |

**Default:** `ToolbarPosition.Inline`

**Example: Bottom Toolbar with Custom Items**

```razor
<SfInlineAIAssist @ref="inlineAssist" RelateTo="#button" PromptRequested="OnPromptRequested">
    <InlineToolbar ToolbarPosition="ToolbarPosition.Bottom" ItemClick="OnToolbarItemClickAsync">
        <InlineToolbarItems>
            <InlineToolbarItem Text="Clear" IconCss="e-icons e-close" Tooltip="Clear prompt"></InlineToolbarItem>
            <InlineToolbarItem Text="History" IconCss="e-icons e-history" Tooltip="Show history"></InlineToolbarItem>
            <InlineToolbarItem Text="Settings" IconCss="e-icons e-settings-fill" Tooltip="Show settings"></InlineToolbarItem>
        </InlineToolbarItems>
    </InlineToolbar>
    <ResponseActions ItemSelect="OnItemSelectAsync"></ResponseActions>
</SfInlineAIAssist>

@code {
    private async Task OnToolbarItemClickAsync(ToolbarItemClickEventArgs args)
    {
        switch (args.Item.Text)
        {
            case "Clear":
                // Implement your clear prompt logic here
                Console.WriteLine("Clearing prompt...");
                break;
            case "History":
                // Implement your history logic here
                Console.WriteLine("Showing history...");
                break;
            case "Settings":
                // Implement your settings logic here
                Console.WriteLine("Showing settings...");
                break;
        }
    }
}
```

### Toolbar Item Properties

Each toolbar item can have:

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Text` | string | '' | Display label |
| `IconCss` | string | '' | CSS class for icon |
| `Tooltip` | string | '' | Hover text |
| `Disabled` | boolean | false | Disable item |
| `Visible` | boolean | true | Show/hide item |
| `Id` | string | '' | Unique identifier |
| `CssClass` | string | '' | Custom CSS classes |
| `Type` | string | 'Button' | Item type (Button, Separator, Input) |
| `Template` | string | '' | Custom HTML template |
| `Align` | string | 'Left' | Alignment (Left, Center, Right) |
| `TabIndex` | number | -1 | Keyboard tab order |

## Component Dimensions

### PopupWidth Property

Control the width of the popup window.

```razor
<SfInlineAIAssist PopupWidth="600px">
</SfInlineAIAssist>
```

**Accepted Values:**
- CSS pixel values: `"400px"`, `"500px"`, `"100%"`
- Numeric values (converted to px): `400`, `500`
- Percentage: `"80%"` (relative to viewport)

**Default:** `"400px"`

### PopupHeight Property

Control the height of the popup window.

```razor
<SfInlineAIAssist PopupHeight="300px">
</SfInlineAIAssist>
```

**Accepted Values:**
- CSS pixel values: `"300px"`, `"auto"`, `"100vh"`
- Numeric values (converted to px): `300`, `400`
- `"auto"` - Automatically resize based on content

**Default:** `"auto"`

### ZIndex Property

Control the z-index (stacking order) of the popup.

```razor
<SfInlineAIAssist ZIndex="9999">
</SfInlineAIAssist>
```

**Use Cases:**
- Stack above modals: `ZIndex="2000"`
- Stack below other elements: `ZIndex="100"`
- Default behavior: `ZIndex="1000"`

**Example: Responsive Sizing**

```razor
<SfInlineAIAssist PopupWidth="@(screenWidth < 768 ? "90%" : "600px")"
                 PopupHeight="@(screenHeight < 600 ? "80vh" : "500px")">
</SfInlineAIAssist>

@code {
    private int screenWidth = 1024;
    private int screenHeight = 768;

    protected override async Task OnInitializedAsync()
    {
        // Get screen dimensions from JSInterop
        // screenWidth = await JSRuntime.InvokeAsync<int>("window.innerWidth");
        // screenHeight = await JSRuntime.InvokeAsync<int>("window.innerHeight");
    }
}
```

## Response Settings

### Built-in Response Items

By default, the response popup displays built-in items:

| Item | Action |
|------|--------|
| **Accept** | Accept the AI response |
| **Discard** | Discard the response and start over |

These items are always present and cannot be removed.

### Custom Response Items

Add custom items alongside built-in items using `ResponseSettings`:

```razor
<SfInlineAIAssist PromptRequested="OnPromptRequested">
    <ResponseActions Items="customItems" ItemSelect="OnItemSelect">
    </ResponseActions>
</SfInlineAIAssist>

@code {
    private List<ResponseItem> customItems = new()
    {
        new ResponseItem { Label = "Regenerate", IconCss = "e-icons e-refresh", Tooltip = "Generate new response" },
        new ResponseItem { Label = "Copy", IconCss = "e-icons e-copy", Tooltip = "Copy to clipboard" },
        new ResponseItem { Label = "Edit", IconCss = "e-icons e-edit", Tooltip = "Edit response" }
    };

    private async Task OnItemSelect(ResponseItemSelectEventArgs args)
    {
        switch (args.Item.Label)
        {
            case "Accept":
                await inlineAssist.HidePopupAsync();
                break;
            case "Discard":
                await inlineAssist.HidePopupAsync();
                break;
            case "Regenerate":
                Console.WriteLine("Regenerating...");
                break;
            case "Copy":
                await CopyToClipboardAsync(args.Item);
                break;
            case "Edit":
                await EditResponseAsync(args.Item);
                break;
        }
    }

    private async Task CopyToClipboardAsync(ResponseItem item)
    {
        // Implement copy logic
    }

    private async Task EditResponseAsync(ResponseItem item)
    {
        // Implement edit logic
    }
}
```

### ResponseItem Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Label` | string | '' | Display text |
| `IconCss` | string | '' | CSS class for icon |
| `Disabled` | boolean | false | Disable the item |
| `GroupBy` | string | '' | Group category |
| `Id` | string | '' | Unique identifier |
| `Tooltip` | string | '' | Hover tooltip |

## Response Items Configuration

### Example: Grouped Response Actions

```razor
<SfInlineAIAssist @ref="inlineAssist" PromptRequested="OnPromptRequested">
    <ResponseActions Items="responseItems" ItemSelect="OnItemSelectAsync"></ResponseActions>
</SfInlineAIAssist>

@code {
    private List<ResponseItem> responseItems = new List<ResponseItem>
    {
        // Built-in items (Accept, Discard) are always present and cannot be removed.
        // Custom items are added alongside them:
        new ResponseItem { Label = "Regenerate", IconCss = "e-icons e-refresh", Tooltip = "Regenerate" },
        new ResponseItem { Label = "Copy", IconCss = "e-icons e-copy", Tooltip = "Copy" }
    };

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
}
```

## Templates

### EditorTemplate

Customize the footer area where users enter prompts.

```razor
<SfInlineAIAssist @ref="inlineAssist" PromptRequested="OnPromptRequested">
    <EditorTemplate>
        <div class="custom-editor">
            <textarea @ref="promptRef" 
                     @bind="promptText"
                     placeholder="Enter your prompt..."
                     style="width: 100%; padding: 10px; border: 1px solid #ddd;"></textarea>
            <SfButton @onclick="OnSendPrompt" IsPrimary="true">Send</SfButton>
        </div>
    </EditorTemplate>
</SfInlineAIAssist>

@code {
    private string promptText = "";
    private ElementReference promptRef;

    private async Task OnSendPrompt()
    {
        if (!string.IsNullOrWhiteSpace(promptText))
        {
            await inlineAssist.ExecutePromptAsync(promptText);
            promptText = "";
        }
    }

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        var response = await GetAIResponse(args.Prompt);
        await inlineAssist.UpdateResponseAsync(response, true);
    }
}
```

### ResponseTemplate

Customize how responses are displayed.

```razor
<SfInlineAIAssist PromptRequested="OnPromptRequested">
    <ResponseTemplate>
        <div class="custom-response" style="padding: 15px; background-color: #f9f9f9; border-radius: 8px;">
            <div style="display: flex; align-items: center; margin-bottom: 10px;">
                <span class="e-icons e-ai-chat" style="margin-right: 10px; font-size: 18px;"></span>
                <strong>AI Response</strong>
            </div>
            <div style="line-height: 1.6; color: #333;">
                @((MarkupString)context.Response)
            </div>
        </div>
    </ResponseTemplate>
</SfInlineAIAssist>
```

**Template Context Properties:**
- `context.Response` - The AI response text (HTML)
- `context.ToolbarItems` - Available toolbar items

### Example: Rich Response Template

```razor
<ResponseTemplate>
    <div class="response-container">
        <div class="response-header">
            <span class="response-icon">@((MarkupString)"&#128193;")</span>
            <span class="response-title">AI Generated Content</span>
        </div>
        <div class="response-body">
            @((MarkupString)context.Response)
        </div>
        <div class="response-meta">
            <small>Generated at @DateTime.Now.ToString("HH:mm:ss")</small>
        </div>
    </div>

    <style>
        .response-container {
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            border-radius: 8px;
            padding: 15px;
            margin: 10px 0;
        }
        
        .response-header {
            display: flex;
            align-items: center;
            margin-bottom: 10px;
            font-weight: bold;
        }
        
        .response-icon {
            font-size: 24px;
            margin-right: 10px;
        }
        
        .response-body {
            background: white;
            padding: 10px;
            border-radius: 4px;
            line-height: 1.6;
        }
        
        .response-meta {
            text-align: right;
            margin-top: 10px;
            color: #666;
        }
    </style>
</ResponseTemplate>
```

## Appearance Customization

### CSSClass Property

Apply custom CSS classes to the component.

```razor
<SfInlineAIAssist CSSClass="custom-ai-assist">
</SfInlineAIAssist>

<style>
    .custom-ai-assist {
        /* Your custom styles */
    }
</style>
```

### Example: Dark Theme

```razor
<SfInlineAIAssist CSSClass="dark-theme">
</SfInlineAIAssist>

<style>
    .dark-theme {
        background-color: #1e1e1e;
        color: #e0e0e0;
    }
    
    .dark-theme .response-item {
        background-color: #2d2d2d;
        border-color: #444;
    }
    
    .dark-theme button {
        background-color: #007bff;
        border-color: #0056b3;
    }
    
    .dark-theme button:hover {
        background-color: #0056b3;
    }
</style>
```

### Example: Glassmorphism Style

```razor
<SfInlineAIAssist CSSClass="glassmorphic">
</SfInlineAIAssist>

<style>
    .glassmorphic {
        background: rgba(255, 255, 255, 0.25);
        backdrop-filter: blur(10px);
        -webkit-backdrop-filter: blur(10px);
        border: 1px solid rgba(255, 255, 255, 0.18);
        border-radius: 12px;
        box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
    }
</style>
```

## Real-World Examples

### Example 1: Complete Customization

```razor
@page "/customized"
@using Syncfusion.Blazor.InteractiveChat
@using Syncfusion.Blazor.Buttons

<style>
    .custom-container {
        max-width: 800px;
        margin: 0 auto;
    }
    
    .editor-container {
        background: #f5f5f5;
        border: 1px solid #ddd;
        border-radius: 8px;
        padding: 20px;
        margin-bottom: 20px;
    }
    
    .ai-popup {
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
        border-radius: 8px;
    }
    
    .response-highlight {
        background-color: #fffacd;
        padding: 10px;
        border-left: 4px solid #ffc107;
    }
</style>

<div class="custom-container">
    <h2>Customized AI Assist</h2>
    
    <div class="editor-container">
        <textarea @ref="contentRef" 
                 style="width: 100%; min-height: 200px; padding: 10px; border: 1px solid #ddd; border-radius: 4px;">
Edit your content here...
        </textarea>
    </div>

    <SfButton id="aiButton" IsPrimary="true" @onclick="OnShowAI">
        Ask AI Assistant
    </SfButton>

    <SfInlineAIAssist @ref="inlineAssist"
                     RelateTo="#aiButton"
                     PopupWidth="600px"
                     PopupHeight="400px"
                     CSSClass="ai-popup"
                     PromptRequested="OnPromptRequested">
        <EditorTemplate>
            <div style="padding: 15px;">
                <textarea @bind="customPrompt" 
                         placeholder="Enter your prompt here..."
                         style="width: 100%; min-height: 60px; padding: 10px; border: 1px solid #ddd; border-radius: 4px; resize: none;"></textarea>
                <SfButton @onclick="OnCustomSend" IsPrimary="true" style="margin-top: 10px;">
                    Send
                </SfButton>
            </div>
        </EditorTemplate>

        <ResponseTemplate>
            <div class="response-highlight">
                @((MarkupString)context.Response)
            </div>
        </ResponseTemplate>

        <ResponseActions Items="customResponseItems" ItemSelect="OnResponseAction">
        </ResponseActions>

        <InlineToolbar ToolbarPosition="ToolbarPosition.Bottom">
            <InlineToolbarItems>
                <InlineToolbarItem Text="Copy" IconCss="e-icons e-copy" Tooltip="Copy"></InlineToolbarItem>
                <InlineToolbarItem Type="ToolbarItemType.Separator"></InlineToolbarItem>
                <InlineToolbarItem Text="Insert" IconCss="e-icons e-insert" Tooltip="Insert"></InlineToolbarItem>
            </InlineToolbarItems>
        </InlineToolbar>
    </SfInlineAIAssist>
</div>

@code {
    private SfInlineAIAssist inlineAssist = new();
    private ElementReference contentRef;
    private string customPrompt = "";

    private List<ResponseItem> customResponseItems = new()
    {
        new ResponseItem { Label = "Regenerate", IconCss = "e-icons e-refresh" },
        new ResponseItem { Label = "Copy", IconCss = "e-icons e-copy" }
    };

    private async Task OnShowAI()
    {
        await inlineAssist.ShowPopupAsync();
    }

    private async Task OnCustomSend()
    {
        if (!string.IsNullOrWhiteSpace(customPrompt))
        {
            await inlineAssist.ExecutePromptAsync(customPrompt);
        }
    }

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        var response = await GetAIResponse(args.Prompt);
        await inlineAssist.UpdateResponseAsync(response, true);
    }

    private async Task OnResponseAction(ResponseItemSelectEventArgs args)
    {
        switch (args.Item.Label)
        {
            case "Accept":
            case "Discard":
                await inlineAssist.HidePopupAsync();
                break;
            case "Copy":
                Console.WriteLine("Copying...");
                break;
            case "Insert":
                Console.WriteLine("Inserting...");
                break;
        }
    }

    private async Task<string> GetAIResponse(string prompt)
    {
        return "AI Response: " + prompt;
    }
}
```

### Example 2: Minimal Configuration

```razor
<SfInlineAIAssist @ref="inlineAssist"
                 RelateTo="#triggerButton"
                 PopupWidth="400px"
                 PromptRequested="OnPromptRequested">
    <ResponseActions ItemSelect="OnResponseAction"></ResponseActions>
</SfInlineAIAssist>

@code {
    private SfInlineAIAssist inlineAssist = new();

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        var response = await GetAIResponse(args.Prompt);
        await inlineAssist.UpdateResponseAsync(response, true);
    }

    private async Task OnResponseAction(ResponseItemSelectEventArgs args)
    {
        if (args.Item.Label == "Accept")
        {
            await inlineAssist.HidePopupAsync();
        }
    }

    private async Task<string> GetAIResponse(string prompt)
    {
        return "Response to: " + prompt;
    }
}
```

### Example 3: Streaming with Custom Styling

```razor
<SfInlineAIAssist @ref="inlineAssist"
                 EnableStreaming="true"
                 PopupWidth="700px"
                 CSSClass="streaming-style"
                 PromptRequested="OnPromptRequested">
    <ResponseTemplate>
        <div style="font-size: 14px; line-height: 1.8; color: #333;">
            @((MarkupString)context.Response)
            @if (!isStreamingComplete)
            {
                <span class="streaming-indicator">▌</span>
            }
        </div>
    </ResponseTemplate>
</SfInlineAIAssist>

<style>
    .streaming-style {
        min-height: 300px;
    }
    
    .streaming-indicator {
        animation: blink 0.7s infinite;
    }
    
    @keyframes blink {
        0%, 49% { opacity: 1; }
        50%, 100% { opacity: 0; }
    }
</style>

@code {
    private SfInlineAIAssist inlineAssist = new();
    private bool isStreamingComplete = true;

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        isStreamingComplete = false;
        var response = await GetAIResponse(args.Prompt);
        
        // Stream response
        foreach (var chunk in response.Split(' '))
        {
            await inlineAssist.UpdateResponseAsync(chunk + " ", false);
            await Task.Delay(100);
        }
        
        isStreamingComplete = true;
        await inlineAssist.UpdateResponseAsync("", true);
    }

    private async Task<string> GetAIResponse(string prompt)
    {
        return "This is a streaming response to your prompt: " + prompt;
    }
}
```

## Best Practices

1. **Responsive sizing** - Use percentages for mobile compatibility
2. **Maintain Z-index context** - Ensure popups appear above other elements
3. **Accessible templates** - Include proper semantic HTML in custom templates
4. **Performance** - Avoid heavy CSS calculations in real-time
5. **Consistency** - Use Syncfusion e-icons for UI consistency
6. **Error states** - Show appropriate messages in response template
7. **Keyboard support** - Ensure custom templates support keyboard navigation
8. **Mobile testing** - Test customizations on various screen sizes
9. **Dark mode support** - Provide dark theme CSS classes
10. **Feedback** - Show loading states and streaming indicators
