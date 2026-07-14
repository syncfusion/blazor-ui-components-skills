# Commands and Toolbar Configuration

## Table of Contents
1. [Overview](#overview)
2. [Command Configuration](#command-configuration)
3. [CommandMenu Tag (CommandSettings)](#commandmenu-tag-commandsettings)
4. [Command Item Properties](#command-item-properties)
5. [ItemSelect Event](#itemselect-event)
6. [Inline Toolbar Configuration](#inline-toolbar-configuration)
7. [Toolbar Item Types](#toolbar-item-types)
8. [Real-World Examples](#real-world-examples)

## Overview

The Inline AI Assist component supports two customization mechanisms:

1. **Commands** - Quick-action prompts shown in a popup menu
2. **Inline Toolbar** - Action buttons shown below or inline with responses

Commands allow users to execute predefined prompts quickly. The inline toolbar provides action buttons for response management.

## Command Configuration

Commands are predefined prompts that users can select from a popup menu, triggered when they click the component.

### Basic Setup

```razor
@using Syncfusion.Blazor.InteractiveChat
@using Syncfusion.Blazor.Buttons

<SfInlineAIAssist @ref="inlineAssist" PromptRequested="OnPromptRequested">
    <CommandMenu Commands="commandItems"></CommandMenu>
</SfInlineAIAssist>

@code {
    private SfInlineAIAssist inlineAssist = new();
    private List<CommandItem> commandItems = new List<CommandItem>
    {
        new CommandItem { Label = "Summarize", Prompt = "Summarize the content", IconCss = "e-icons e-collapse-2" },
        new CommandItem { Label = "Shorten", Prompt = "Shorten the content", IconCss = "e-icons e-shorten" },
        new CommandItem { Label = "Translate", Prompt = "Translate the content", IconCss = "e-icons e-translate" },
        new CommandItem { Label = "Make professional", Prompt = "Make the content more professional", IconCss = "e-icons e-elaborate" }
    };

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        var response = await GetAIResponse(args.Prompt);
        await inlineAssist.UpdateResponseAsync(response, true);
    }
}
```

## CommandMenu Tag (CommandSettings)

You can render and use the command action popup by using the `Commands` property in the `CommandSettings` tag helper, which renders as the `<CommandMenu>` tag in Razor markup. This property helps to supply commands, control popup dimensions, and customize behavior.

### Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Commands` | `List<CommandItem>` | - | Collection of command items shown in the popup |
| `PopupWidth` | string/number | '400px' | Width of command popup (CSS value or number in px) |
| `PopupHeight` | string/number | 'auto' | Height of command popup (CSS value or number in px). Enables scrollable lists when many commands exist. |
| `ItemSelect` | event | - | Fired when a command item is selected from the popup |

### Example: Custom Popup Size

```razor
<CommandMenu Commands="commandItems" PopupWidth="250px" PopupHeight="200px"></CommandMenu>
```

## Command Item Properties

Each command is configured with these properties:

| Property | Type | Default | Required | Description |
|----------|------|---------|----------|-------------|
| `Label` | string | '' | Yes | Display text shown to user |
| `Prompt` | string | '' | Yes | The prompt sent to AI provider |
| `IconCss` | string | '' | No | CSS class for icon (Syncfusion e-icons) |
| `Disabled` | boolean | false | No | Disable the command |
| `GroupBy` | string | '' | No | Group category for organizing commands |
| `Id` | string | '' | No | Unique identifier |
| `Tooltip` | string | '' | No | Hover tooltip text |

### Example: Complete Command Configuration

```csharp
private List<CommandItem> commandItems = new List<CommandItem>
{
    new CommandItem
    {
        Label = "Summarize",
        Prompt = "Summarize this text into 2-3 key points",
        IconCss = "e-icons e-collapse-2",
        Tooltip = "Summarize the selected text",
        Id = "cmd-summarize",
        GroupBy = "Content Optimization"
    }
};
```

## Command Groups

Organize commands into logical groups using the `GroupBy` property:

```csharp
private List<CommandItem> commandItems = new List<CommandItem>
{
    // Content Generation Group
    new CommandItem { Label = "Generate", Prompt = "Generate new content based on this", GroupBy = "Generate", IconCss = "e-icons e-add" },

    // Content Optimization Group
    new CommandItem { Label = "Summarize", Prompt = "Summarize in 3 sentences", GroupBy = "Optimize", IconCss = "e-icons e-compress" },
    new CommandItem { Label = "Rewrite", Prompt = "Rewrite in professional tone", GroupBy = "Optimize", IconCss = "e-icons e-edit" },

    // Translation Group
    new CommandItem { Label = "Spanish", Prompt = "Translate to Spanish", GroupBy = "Translate", IconCss = "e-icons e-language" },
    new CommandItem { Label = "French", Prompt = "Translate to French", GroupBy = "Translate", IconCss = "e-icons e-language" }
};
```

## ItemSelect Event

The `ItemSelect` event fires when a command is selected. This is typically handled at the `ResponseActions` level, not the command level specifically.

### Event Flow
1. User clicks button to open component
2. Component shows command popup
3. User selects a command
4. Component sends prompt to `PromptRequested` event
5. Response is displayed
6. User clicks response action (Accept/Discard/Custom)
7. `ItemSelect` event fires on response action

### Example

```razor
<SfInlineAIAssist @ref="inlineAssist" PromptRequested="OnPromptRequested">
    <CommandMenu Commands="commandItems" ItemSelect="OnCommandItemSelectAsync"></CommandMenu>
    <ResponseActions ItemSelect="OnResponseAction"></ResponseActions>
</SfInlineAIAssist>

@code {
    private List<CommandItem> commandItems = new List<CommandItem>
    {
        new CommandItem { Label = "Summarize", Prompt = "Summarize this text" }
    };

    private async Task OnCommandItemSelectAsync(CommandItemSelectEventArgs args)
    {
        // Handle command item selection
    }

    private async Task OnResponseAction(ResponseItemSelectEventArgs args)
    {
        switch (args.Item.Label)
        {
            case "Accept":
                // Accept the response
                await inlineAssist.HidePopupAsync();
                break;
            case "Discard":
                // Discard the response
                await inlineAssist.HidePopupAsync();
                break;
            default:
                Console.WriteLine($"Custom action: {args.Item.Label}");
                break;
        }
    }
}
```

## Inline Toolbar Configuration

The inline toolbar appears below or inline with the AI response, providing action buttons.

### Basic Setup

```razor
<SfInlineAIAssist @ref="inlineAssist">
    <InlineToolbar>
        <InlineToolbarItems>
            <InlineToolbarItem Text="Copy" 
                               IconCss="e-icons e-copy" 
                               Tooltip="Copy response"></InlineToolbarItem>
            <InlineToolbarItem Text="Regenerate" 
                               IconCss="e-icons e-refresh" 
                               Tooltip="Generate new response"></InlineToolbarItem>
        </InlineToolbarItems>
    </InlineToolbar>
</SfInlineAIAssist>
```

### ToolbarPosition Property

Control where the toolbar appears:

```razor
<InlineToolbar ToolbarPosition="ToolbarPosition.Bottom">
    <!-- Items -->
</InlineToolbar>
```

**Position Options:**
- `Inline` - Toolbar items appear inline with response (default)
- `Bottom` - Toolbar items appear in dedicated footer area

### Example: Toggle Toolbar Position

```razor
<SfButton @onclick="OnTogglePosition">Toggle Position</SfButton>

<SfInlineAIAssist @ref="inlineAssist">
    <InlineToolbar ToolbarPosition="@currentPosition">
        <!-- Items -->
    </InlineToolbar>
</SfInlineAIAssist>

@code {
    private ToolbarPosition currentPosition = ToolbarPosition.Inline;

    private void OnTogglePosition()
    {
        currentPosition = currentPosition == ToolbarPosition.Inline 
            ? ToolbarPosition.Bottom 
            : ToolbarPosition.Inline;
    }
}
```

## Toolbar Item Types

### Button Item (Default)

```razor
<InlineToolbarItem Text="Copy" 
                   IconCss="e-icons e-copy"
                   Type="ToolbarItemType.Button"
                   Tooltip="Copy text"></InlineToolbarItem>
```

### Separator Item

```razor
<InlineToolbarItem Type="ToolbarItemType.Separator"></InlineToolbarItem>
```

### Input Item

```razor
<InlineToolbarItem Type="ToolbarItemType.Input"
                   Template="<input type='text' placeholder='Search'/>"></InlineToolbarItem>
```

## Toolbar Item Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Text` | string | '' | Display text for button |
| `IconCss` | string | '' | CSS class for icon |
| `Type` | ToolbarItemType | Button | Item type (Button, Separator, Input) |
| `Tooltip` | string | '' | Hover tooltip |
| `Visible` | boolean | true | Show/hide item |
| `Disabled` | boolean | false | Enable/disable |
| `Id` | string | '' | Unique identifier |
| `Template` | string | '' | Custom HTML template |
| `CssClass` | string | '' | Custom CSS classes for styling the toolbar item |
| `Align` | string | 'Left' | Item alignment (Left, Center, Right) |
| `TabIndex` | number | -1 | Tab order for keyboard navigation |

## ItemClick Event

The `ItemClick` event fires when a toolbar item is clicked.

### Example

```razor
<InlineToolbar ItemClick="OnToolbarItemClick">
    <InlineToolbarItems>
        <InlineToolbarItem Text="Copy" IconCss="e-icons e-copy"></InlineToolbarItem>
        <InlineToolbarItem Text="Edit" IconCss="e-icons e-edit"></InlineToolbarItem>
    </InlineToolbarItems>
</InlineToolbar>

@code {
    private void OnToolbarItemClick(ToolbarItemClickEventArgs args)
    {
        switch (args.Item?.Text)
        {
            case "Copy":
                CopyResponseToClipboard();
                break;
            case "Edit":
                EditResponse();
                break;
        }
    }

    private void CopyResponseToClipboard()
    {
        Console.WriteLine("Copying response...");
        // Implement copy logic
    }

    private void EditResponse()
    {
        Console.WriteLine("Editing response...");
        // Implement edit logic
    }
}
```

## Real-World Examples

### Example 1: Content Creation Workflow

```razor
@page "/content-creator"
@using Syncfusion.Blazor.InteractiveChat
@using Syncfusion.Blazor.Buttons

<div style="max-width: 900px;">
    <h2>AI Content Creator</h2>
    
    <SfButton id="creatorButton" IsPrimary="true" @onclick="OnClick">
        Generate Content
    </SfButton>

    <div id="contentArea" style="margin-top: 20px; padding: 20px; border: 1px solid #ddd; min-height: 200px;">
        <p>Your generated content will appear here...</p>
    </div>

    <SfInlineAIAssist @ref="inlineAssist"
                     RelateTo="#creatorButton"
                     PromptRequested="OnPromptRequested">
        <CommandMenu Commands="creatorCommands" PopupWidth="500px"></CommandMenu>

        <InlineToolbar ToolbarPosition="ToolbarPosition.Bottom" ItemClick="OnToolbarClick">
            <InlineToolbarItems>
                <InlineToolbarItem Text="Copy" IconCss="e-icons e-copy" Tooltip="Copy to clipboard"></InlineToolbarItem>
                <InlineToolbarItem Text="Regenerate" IconCss="e-icons e-refresh" Tooltip="Generate new version"></InlineToolbarItem>
                <InlineToolbarItem Type="ToolbarItemType.Separator"></InlineToolbarItem>
                <InlineToolbarItem Text="Insert" IconCss="e-icons e-insert" Tooltip="Insert into document"></InlineToolbarItem>
            </InlineToolbarItems>
        </InlineToolbar>

        <ResponseActions ItemSelect="OnResponseAction"></ResponseActions>
    </SfInlineAIAssist>
</div>

@code {
    private SfInlineAIAssist inlineAssist = new();
    private List<CommandItem> creatorCommands = new List<CommandItem>
    {
        new CommandItem { Label = "Blog Post", Prompt = "Write a 500-word blog post about this topic", GroupBy = "Content Type", IconCss = "e-icons e-document" },
        new CommandItem { Label = "Social Media", Prompt = "Create 5 social media post ideas", GroupBy = "Content Type", IconCss = "e-icons e-share" },
        new CommandItem { Label = "Product Description", Prompt = "Write a compelling product description", GroupBy = "Content Type", IconCss = "e-icons e-box" },

        new CommandItem { Label = "Make It Longer", Prompt = "Expand this content with more details", GroupBy = "Refine", IconCss = "e-icons e-expand" },
        new CommandItem { Label = "Make It Shorter", Prompt = "Condense this to essential points", GroupBy = "Refine", IconCss = "e-icons e-compress" },
        new CommandItem { Label = "Improve Tone", Prompt = "Rewrite in a more professional tone", GroupBy = "Refine", IconCss = "e-icons e-edit" }
    };

    private async Task OnClick()
    {
        await inlineAssist.ShowPopupAsync();
    }

    private async Task OnPromptRequested(PromptRequestedEventArgs args)
    {
        var response = await GetAIResponse(args.Prompt);
        await inlineAssist.UpdateResponseAsync(response, true);
    }

    private void OnToolbarClick(ToolbarItemClickEventArgs args)
    {
        switch (args.Item?.Text)
        {
            case "Copy":
                Console.WriteLine("Copied to clipboard");
                break;
            case "Insert":
                Console.WriteLine("Inserted into document");
                break;
        }
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
        // Call your AI provider
        return "Generated content";
    }
}
```

### Example 2: Email Assistant with Commands

```razor
<SfInlineAIAssist @ref="inlineAssist" PromptRequested="OnPromptRequested">
    <CommandMenu Commands="emailCommands"></CommandMenu>

    <InlineToolbar>
        <InlineToolbarItems>
            <InlineToolbarItem Text="Accept" IconCss="e-icons e-check" Tooltip="Accept changes"></InlineToolbarItem>
            <InlineToolbarItem Text="Reject" IconCss="e-icons e-close" Tooltip="Reject changes"></InlineToolbarItem>
            <InlineToolbarItem Type="ToolbarItemType.Separator"></InlineToolbarItem>
            <InlineToolbarItem Text="Copy" IconCss="e-icons e-copy" Tooltip="Copy to clipboard"></InlineToolbarItem>
        </InlineToolbarItems>
    </InlineToolbar>
</SfInlineAIAssist>

@code {
    private List<CommandItem> emailCommands = new List<CommandItem>
    {
        new CommandItem { Label = "Professional", Prompt = "Rewrite this email in a formal professional tone", IconCss = "e-icons e-align-center" },
        new CommandItem { Label = "Casual", Prompt = "Rewrite this email in a friendly casual tone", IconCss = "e-icons e-align-left" },
        new CommandItem { Label = "Fix Grammar", Prompt = "Check and fix all grammar and spelling errors", IconCss = "e-icons e-check" },
        new CommandItem { Label = "Shorten", Prompt = "Make this email more concise", IconCss = "e-icons e-compress" },
        new CommandItem { Label = "Expand", Prompt = "Add more detail to this email", IconCss = "e-icons e-expand" }
    };
}
```

### Example 3: Code Optimization Commands

```csharp
private List<CommandItem> codeCommands = new List<CommandItem>
{
    new CommandItem { Label = "Format Code", Prompt = "Format this code with proper indentation and style", GroupBy = "Formatting", IconCss = "e-icons e-layout" },
    new CommandItem { Label = "Add Comments", Prompt = "Add meaningful comments to this code", GroupBy = "Documentation", IconCss = "e-icons e-annotation" },
    new CommandItem { Label = "Optimize Performance", Prompt = "Suggest optimizations to improve performance", GroupBy = "Optimization", IconCss = "e-icons e-flash" },
    new CommandItem { Label = "Add Error Handling", Prompt = "Add comprehensive error handling to this code", GroupBy = "Improvement", IconCss = "e-icons e-shield" },
    new CommandItem { Label = "Write Unit Tests", Prompt = "Generate unit tests for this code", GroupBy = "Testing", IconCss = "e-icons e-test" }
};
```

## Best Practices

1. **Use clear labels** - Labels should be action-oriented and descriptive
2. **Organize with GroupBy** - Group related commands for better UX
3. **Provide icons** - Use Syncfusion e-icons for consistency
4. **Test popup size** - Ensure commands are readable at configured size
5. **Implement ItemClick** - Handle toolbar interactions properly
6. **Validate prompts** - Check prompt content before sending to AI
7. **Provide tooltips** - Hover text helps users understand actions
8. **Consider command count** - Too many commands reduce usability (8-12 recommended)
9. **Order commands logically** - Most common commands first
10. **Handle response actions** - Accept/Discard/Custom in ResponseActions
