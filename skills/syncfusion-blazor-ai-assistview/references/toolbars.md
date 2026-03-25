````markdown
# Toolbars Customization

## Table of Contents
- [Overview](#overview)
- [Prompt Toolbar](#prompt-toolbar)
- [Response Toolbar](#response-toolbar)
- [Footer Toolbar](#footer-toolbar)
- [Custom Toolbar Items](#custom-toolbar-items)
- [Toolbar Events](#toolbar-events)
- [Common Patterns](#common-patterns)

## Overview

The AI AssistView component supports three types of toolbars to enhance user interaction:

1. **Prompt Toolbar** - Actions available for user prompts (edit, copy, regenerate)
2. **Response Toolbar** - Actions available for AI responses (copy, like, dislike, regenerate)
3. **Footer Toolbar** - Additional actions in the footer area (settings, help, feedback)

Each toolbar can be customized with built-in or custom items.

## Prompt Toolbar

The `PromptToolbar` appears next to each user prompt in the conversation, providing quick actions like copying or editing the prompt.

### Basic Prompt Toolbar

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px;">
    <SfAIAssistView PromptRequested="@OnPromptRequested">
        <AssistViews>
            <AssistView Header="AI Assistant">
                <PromptToolbar>
                    <PromptToolbarItems>
                        <PromptToolbarItem 
                            IconCss="e-icons e-copy" 
                            Tooltip="Copy Prompt"
                            Text="Copy">
                        </PromptToolbarItem>
                        <PromptToolbarItem 
                            IconCss="e-icons e-edit" 
                            Tooltip="Edit Prompt"
                            Text="Edit">
                        </PromptToolbarItem>
                    </PromptToolbarItems>
                </PromptToolbar>
            </AssistView>
        </AssistViews>
    </SfAIAssistView>
</div>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response content</div>";
    }
}
```

### PromptToolbarItem Properties

| Property | Type | Description |
|----------|------|-------------|
| `IconCss` | `string` | CSS class for toolbar item icon |
| `Text` | `string` | Display text for toolbar item |
| `Tooltip` | `string` | Tooltip text shown on hover |
| `Visible` | `bool` | Controls item visibility |
| `Disabled` | `bool` | Controls item enabled state |
| `Template` | `RenderFragment` | Custom template for item rendering |

## Response Toolbar

The `ResponseToolbar` appears next to each AI response, providing actions like copying, rating, or regenerating responses.

### Basic Response Toolbar

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px;">
    <SfAIAssistView PromptRequested="@OnPromptRequested">
        <AssistViews>
            <AssistView Header="AI Assistant">
                <ResponseToolbar>
                    <ResponseToolbarItems>
                        <ResponseToolbarItem 
                            IconCss="e-icons e-copy" 
                            Tooltip="Copy Response"
                            Text="Copy">
                        </ResponseToolbarItem>
                        <ResponseToolbarItem 
                            IconCss="e-icons e-like" 
                            Tooltip="Like Response"
                            Text="Like">
                        </ResponseToolbarItem>
                        <ResponseToolbarItem 
                            IconCss="e-icons e-dislike" 
                            Tooltip="Dislike Response"
                            Text="Dislike">
                        </ResponseToolbarItem>
                        <ResponseToolbarItem 
                            IconCss="e-icons e-refresh" 
                            Tooltip="Regenerate Response"
                            Text="Regenerate">
                        </ResponseToolbarItem>
                    </ResponseToolbarItems>
                </ResponseToolbar>
            </AssistView>
        </AssistViews>
    </SfAIAssistView>
</div>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>AI generated response content</div>";
    }
}
```

### ResponseToolbarItem Properties

| Property | Type | Description |
|----------|------|-------------|
| `IconCss` | `string` | CSS class for toolbar item icon |
| `Text` | `string` | Display text for toolbar item |
| `Tooltip` | `string` | Tooltip text shown on hover |
| `Visible` | `bool` | Controls item visibility |
| `Disabled` | `bool` | Controls item enabled state |
| `Template` | `RenderFragment` | Custom template for item rendering |

## Footer Toolbar

The `AssistViewFooterToolbar` appears at the bottom of the component, providing global actions and settings.

### Basic Footer Toolbar

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px;">
    <SfAIAssistView PromptRequested="@OnPromptRequested">
        <AssistViewFooterToolbar>
            <AssistViewFooterToolbarItems>
                <AssistViewFooterToolbarItem 
                    IconCss="e-icons e-settings" 
                    Tooltip="Settings"
                    Text="Settings"
                    Alignment="Left">
                </AssistViewFooterToolbarItem>
                <AssistViewFooterToolbarItem 
                    IconCss="e-icons e-help" 
                    Tooltip="Help"
                    Text="Help"
                    Alignment="Right">
                </AssistViewFooterToolbarItem>
                <AssistViewFooterToolbarItem 
                    IconCss="e-icons e-feedback" 
                    Tooltip="Feedback"
                    Text="Feedback"
                    Alignment="Right">
                </AssistViewFooterToolbarItem>
            </AssistViewFooterToolbarItems>
        </AssistViewFooterToolbar>
    </SfAIAssistView>
</div>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response content</div>";
    }
}
```

### FooterToolbarItem Properties

| Property | Type | Description |
|----------|------|-------------|
| `IconCss` | `string` | CSS class for toolbar item icon |
| `Text` | `string` | Display text for toolbar item |
| `Tooltip` | `string` | Tooltip text shown on hover |
| `Alignment` | `string` | Alignment ("Left", "Right", "Center") |
| `Visible` | `bool` | Controls item visibility |
| `Disabled` | `bool` | Controls item enabled state |
| `Template` | `RenderFragment` | Custom template for item rendering |

## Custom Toolbar Items

### Using Custom Icons

```csharp
@using Syncfusion.Blazor.InteractiveChat

<!-- Include Font Awesome if using custom icons -->
<link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet" />

<div style="height: 400px;">
    <SfAIAssistView PromptRequested="@OnPromptRequested">
        <AssistViews>
            <AssistView Header="AI Assistant">
                <ResponseToolbar>
                    <ResponseToolbarItems>
                        <!-- Syncfusion icons -->
                        <ResponseToolbarItem 
                            IconCss="e-icons e-copy" 
                            Tooltip="Copy">
                        </ResponseToolbarItem>
                        
                        <!-- Font Awesome icons -->
                        <ResponseToolbarItem 
                            IconCss="fas fa-bookmark" 
                            Tooltip="Save">
                        </ResponseToolbarItem>
                        <ResponseToolbarItem 
                            IconCss="fas fa-share" 
                            Tooltip="Share">
                        </ResponseToolbarItem>
                        
                        <!-- Bootstrap icons -->
                        <ResponseToolbarItem 
                            IconCss="bi bi-download" 
                            Tooltip="Download">
                        </ResponseToolbarItem>
                    </ResponseToolbarItems>
                </ResponseToolbar>
            </AssistView>
        </AssistViews>
    </SfAIAssistView>
</div>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response content</div>";
    }
}
```

### Custom Templates

Create completely custom toolbar items using templates:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px;">
    <SfAIAssistView PromptRequested="@OnPromptRequested">
        <AssistViews>
            <AssistView Header="AI Assistant">
                <ResponseToolbar>
                    <ResponseToolbarItems>
                        <!-- Standard item -->
                        <ResponseToolbarItem 
                            IconCss="e-icons e-copy" 
                            Tooltip="Copy">
                        </ResponseToolbarItem>
                        
                        <!-- Custom template item -->
                        <ResponseToolbarItem>
                            <Template>
                                <button class="btn btn-sm btn-primary" @onclick="HandleCustomAction">
                                    <i class="e-icons e-star"></i> Rate
                                </button>
                            </Template>
                        </ResponseToolbarItem>
                    </ResponseToolbarItems>
                </ResponseToolbar>
            </AssistView>
        </AssistViews>
    </SfAIAssistView>
</div>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response content</div>";
    }

    private void HandleCustomAction()
    {
        Console.WriteLine("Custom action triggered");
    }
}
```

## Toolbar Events

Handle toolbar item clicks to implement custom functionality:

```csharp
@using Syncfusion.Blazor.InteractiveChat
@inject IJSRuntime JS

<div style="height: 400px;">
    <SfAIAssistView 
        PromptRequested="@OnPromptRequested"
        ToolbarItemClicked="@OnToolbarItemClicked">
        <AssistViews>
            <AssistView Header="AI Assistant">
                <ResponseToolbar>
                    <ResponseToolbarItems>
                        <ResponseToolbarItem 
                            Name="copy"
                            IconCss="e-icons e-copy" 
                            Tooltip="Copy Response">
                        </ResponseToolbarItem>
                        <ResponseToolbarItem 
                            Name="like"
                            IconCss="e-icons e-like" 
                            Tooltip="Like Response">
                        </ResponseToolbarItem>
                        <ResponseToolbarItem 
                            Name="save"
                            IconCss="e-icons e-save" 
                            Tooltip="Save Response">
                        </ResponseToolbarItem>
                    </ResponseToolbarItems>
                </ResponseToolbar>
            </AssistView>
        </AssistViews>
    </SfAIAssistView>
</div>

@if (!string.IsNullOrEmpty(statusMessage))
{
    <div class="alert alert-info mt-2">@statusMessage</div>
}

@code {
    private string statusMessage = "";

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>AI generated response content that can be copied or saved.</div>";
    }

    private async Task OnToolbarItemClicked(AssistViewToolbarItemClickedEventArgs args)
    {
        var itemName = args.Name;
        
        switch (itemName)
        {
            case "copy":
                await CopyToClipboard(args);
                statusMessage = "Response copied to clipboard";
                break;
                
            case "like":
                await HandleLike(args);
                statusMessage = "Response liked";
                break;
                
            case "save":
                await HandleSave(args);
                statusMessage = "Response saved";
                break;
                
            default:
                statusMessage = $"Toolbar item clicked: {itemName}";
                break;
        }
        
        StateHasChanged();
    }

    private async Task CopyToClipboard(AssistViewToolbarItemClickedEventArgs args)
    {
        // Get response text and copy to clipboard
        var responseText = "Response text here"; // Extract from args if available
        await JS.InvokeVoidAsync("navigator.clipboard.writeText", responseText);
    }

    private async Task HandleLike(AssistViewToolbarItemClickedEventArgs args)
    {
        // Save like to database
        await Task.Delay(100);
        Console.WriteLine("Response liked");
    }

    private async Task HandleSave(AssistViewToolbarItemClickedEventArgs args)
    {
        // Save response to user's saved items
        await Task.Delay(100);
        Console.WriteLine("Response saved");
    }
}
```

## Common Patterns

### Pattern 1: Dynamic Toolbar Visibility

Show/hide toolbar items based on conditions:

```csharp
@code {
    private bool isResponseLiked = false;
    private bool isResponseSaved = false;

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response content</div>";
    }
}

<SfAIAssistView PromptRequested="@OnPromptRequested">
    <AssistViews>
        <AssistView Header="AI Assistant">
            <ResponseToolbar>
                <ResponseToolbarItems>
                    <ResponseToolbarItem 
                        IconCss="e-icons e-copy" 
                        Tooltip="Copy">
                    </ResponseToolbarItem>
                    
                    <!-- Show 'Unlike' if already liked, otherwise 'Like' -->
                    <ResponseToolbarItem 
                        IconCss="@(isResponseLiked ? "e-icons e-unlike" : "e-icons e-like")" 
                        Tooltip="@(isResponseLiked ? "Unlike" : "Like")">
                    </ResponseToolbarItem>
                    
                    <!-- Only show 'Save' if not already saved -->
                    <ResponseToolbarItem 
                        IconCss="e-icons e-save" 
                        Tooltip="Save"
                        Visible="@(!isResponseSaved)">
                    </ResponseToolbarItem>
                </ResponseToolbarItems>
            </ResponseToolbar>
        </AssistView>
    </AssistViews>
</SfAIAssistView>
```

### Pattern 2: Context-Aware Toolbars

Different toolbar items based on response content:

```csharp
@code {
    private bool responseContainsCode = false;
    private bool responseContainsTable = false;

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        
        var response = "<div><pre><code>function example() { }</code></pre></div>";
        
        responseContainsCode = response.Contains("<code>");
        responseContainsTable = response.Contains("<table>");
        
        args.Response = response;
    }
}

<SfAIAssistView PromptRequested="@OnPromptRequested">
    <AssistViews>
        <AssistView Header="AI Assistant">
            <ResponseToolbar>
                <ResponseToolbarItems>
                    <ResponseToolbarItem 
                        IconCss="e-icons e-copy" 
                        Tooltip="Copy Response">
                    </ResponseToolbarItem>
                    
                    <!-- Show 'Copy Code' only if response contains code -->
                    <ResponseToolbarItem 
                        IconCss="e-icons e-code" 
                        Tooltip="Copy Code"
                        Visible="@responseContainsCode">
                    </ResponseToolbarItem>
                    
                    <!-- Show 'Export Table' only if response contains table -->
                    <ResponseToolbarItem 
                        IconCss="e-icons e-download" 
                        Tooltip="Export Table"
                        Visible="@responseContainsTable">
                    </ResponseToolbarItem>
                </ResponseToolbarItems>
            </ResponseToolbar>
        </AssistView>
    </AssistViews>
</SfAIAssistView>
```

### Pattern 3: Rating System

Implement a rating system using toolbar items:

```csharp
@code {
    private Dictionary<int, int> responseRatings = new(); // promptIndex -> rating (1-5)

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response content</div>";
    }

    private async Task OnToolbarItemClicked(AssistViewToolbarItemClickedEventArgs args)
    {
        if (args.Name.StartsWith("rate"))
        {
            var rating = int.Parse(args.Name.Replace("rate", ""));
            var promptIndex = args.Item.Index; // Assuming index is available
            
            responseRatings[promptIndex] = rating;
            await SaveRating(promptIndex, rating);
            
            StateHasChanged();
        }
    }

    private async Task SaveRating(int promptIndex, int rating)
    {
        // Save to database
        await Task.Delay(100);
        Console.WriteLine($"Saved rating {rating} for prompt {promptIndex}");
    }
}

<SfAIAssistView 
    PromptRequested="@OnPromptRequested"
    ToolbarItemClicked="@OnToolbarItemClicked">
    <AssistViews>
        <AssistView Header="AI Assistant">
            <ResponseToolbar>
                <ResponseToolbarItems>
                    <ResponseToolbarItem 
                        IconCss="e-icons e-copy" 
                        Tooltip="Copy">
                    </ResponseToolbarItem>
                    
                    <!-- Rating stars -->
                    @for (int i = 1; i <= 5; i++)
                    {
                        var rating = i;
                        <ResponseToolbarItem 
                            Name="@($"rate{rating}")"
                            IconCss="e-icons e-star" 
                            Tooltip="@($"Rate {rating} stars")">
                        </ResponseToolbarItem>
                    }
                </ResponseToolbarItems>
            </ResponseToolbar>
        </AssistView>
    </AssistViews>
</SfAIAssistView>
```

### Pattern 4: Footer Toolbar with Settings

Implement settings and preferences in footer toolbar:

```csharp
@code {
    private bool showSettings = false;
    private string selectedTheme = "light";

    private void OnFooterToolbarItemClicked(AssistViewToolbarItemClickedEventArgs args)
    {
        switch (args.Name)
        {
            case "settings":
                showSettings = !showSettings;
                break;
                
            case "clear":
                ClearConversation();
                break;
                
            case "export":
                ExportConversation();
                break;
        }
        
        StateHasChanged();
    }

    private void ClearConversation()
    {
        // Clear all prompts and responses
        Console.WriteLine("Conversation cleared");
    }

    private void ExportConversation()
    {
        // Export conversation to file
        Console.WriteLine("Conversation exported");
    }
}

<div style="height: 450px;">
    <SfAIAssistView 
        PromptRequested="@OnPromptRequested"
        ToolbarItemClicked="@OnFooterToolbarItemClicked">
        <AssistViewFooterToolbar>
            <AssistViewFooterToolbarItems>
                <AssistViewFooterToolbarItem 
                    Name="settings"
                    IconCss="e-icons e-settings" 
                    Tooltip="Settings"
                    Alignment="Left">
                </AssistViewFooterToolbarItem>
                <AssistViewFooterToolbarItem 
                    Name="clear"
                    IconCss="e-icons e-trash" 
                    Tooltip="Clear Conversation"
                    Alignment="Right">
                </AssistViewFooterToolbarItem>
                <AssistViewFooterToolbarItem 
                    Name="export"
                    IconCss="e-icons e-download" 
                    Tooltip="Export Conversation"
                    Alignment="Right">
                </AssistViewFooterToolbarItem>
            </AssistViewFooterToolbarItems>
        </AssistViewFooterToolbar>
    </SfAIAssistView>
</div>

@if (showSettings)
{
    <div class="settings-panel mt-3 p-3 border rounded">
        <h5>Settings</h5>
        <div class="form-group">
            <label>Theme:</label>
            <select class="form-control" @bind="selectedTheme">
                <option value="light">Light</option>
                <option value="dark">Dark</option>
                <option value="auto">Auto</option>
            </select>
        </div>
    </div>
}
```

## Best Practices

1. **Keep toolbars minimal** - Only include essential actions (3-5 items max)
2. **Use clear icons** - Choose icons that clearly represent their action
3. **Provide tooltips** - Always include helpful tooltip text
4. **Handle errors gracefully** - Provide feedback when actions fail
5. **Consider mobile** - Ensure toolbar items are touch-friendly
6. **Use consistent naming** - Use the `Name` property for reliable event handling
7. **Disable when appropriate** - Disable items when actions aren't available
8. **Test accessibility** - Ensure keyboard navigation works properly

````
