````markdown
# Templates

## Table of Contents
- [Overview](#overview)
- [BannerTemplate](#bannertemplate)
- [PromptItemTemplate](#promptitemtemplate)
- [ResponseItemTemplate](#responseitemtemplate)
- [PromptSuggestionItemTemplate](#promptsuggestionitemtemplate)
- [FooterTemplate](#footertemplate)
- [ViewTemplate](#viewtemplate)
- [Template Contexts](#template-contexts)
- [Best Practices](#best-practices)

## Overview

The AI AssistView component provides extensive templating support, allowing you to customize the rendering of various UI elements. Templates give you complete control over how content is displayed while maintaining the component's core functionality.

**Available Templates:**
- `BannerTemplate` - Custom initial/empty state banner
- `PromptItemTemplate` - Custom prompt message rendering
- `ResponseItemTemplate` - Custom response message rendering
- `PromptSuggestionItemTemplate` - Custom suggestion chip rendering
- `FooterTemplate` - Custom footer area
- `ViewTemplate` - Complete view customization

## BannerTemplate

The `BannerTemplate` displays custom content when the conversation is empty or as an initial welcome screen.

### Basic Banner

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px;">
    <SfAIAssistView PromptRequested="@OnPromptRequested">
        <AssistViews>
            <AssistView Header="AI Assistant">
                <BannerTemplate>
                    <div class="banner-content text-center p-4">
                        <h2>👋 Welcome to AI Assistant</h2>
                        <p class="text-muted">Ask me anything to get started!</p>
                    </div>
                </BannerTemplate>
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

### Rich Banner with Features

```csharp
<BannerTemplate>
    <div class="welcome-banner p-5">
        <div class="text-center mb-4">
            <i class="e-icons e-assistview-icon" style="font-size: 48px; color: #0066cc;"></i>
            <h1 class="mt-3">AI Code Assistant</h1>
            <p class="lead text-muted">Get instant help with coding questions, debugging, and best practices</p>
        </div>
        
        <div class="row">
            <div class="col-md-4">
                <div class="feature-card p-3 text-center">
                    <i class="e-icons e-code" style="font-size: 32px;"></i>
                    <h5 class="mt-2">Code Generation</h5>
                    <p class="text-muted small">Generate code snippets</p>
                </div>
            </div>
            <div class="col-md-4">
                <div class="feature-card p-3 text-center">
                    <i class="e-icons e-bug" style="font-size: 32px;"></i>
                    <h5 class="mt-2">Debugging Help</h5>
                    <p class="text-muted small">Find and fix issues</p>
                </div>
            </div>
            <div class="col-md-4">
                <div class="feature-card p-3 text-center">
                    <i class="e-icons e-book" style="font-size: 32px;"></i>
                    <h5 class="mt-2">Best Practices</h5>
                    <p class="text-muted small">Learn coding standards</p>
                </div>
            </div>
        </div>
    </div>
</BannerTemplate>

<style>
    .welcome-banner {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        border-radius: 8px;
    }
    
    .feature-card {
        background: rgba(255, 255, 255, 0.1);
        border-radius: 8px;
        transition: transform 0.2s;
    }
    
    .feature-card:hover {
        transform: translateY(-5px);
    }
</style>
```

## PromptItemTemplate

Customize how user prompts are displayed in the conversation.

### Basic Prompt Template

```csharp
@using Syncfusion.Blazor.InteractiveChat

<SfAIAssistView PromptRequested="@OnPromptRequested">
    <AssistViews>
        <AssistView Header="AI Assistant">
            <PromptItemTemplate Context="promptContext">
                <div class="custom-prompt p-3 mb-2">
                    <div class="d-flex align-items-start">
                        <div class="prompt-avatar me-3">
                            <i class="e-icons e-user"></i>
                        </div>
                        <div class="prompt-content flex-grow-1">
                            <div class="prompt-header d-flex justify-content-between">
                                <strong>You</strong>
                                <small class="text-muted">Just now</small>
                            </div>
                            <div class="prompt-text mt-1">
                                @((MarkupString)promptContext.Prompt)
                            </div>
                        </div>
                    </div>
                </div>
            </PromptItemTemplate>
        </AssistView>
    </AssistViews>
</SfAIAssistView>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response content</div>";
    }
}
```

### Prompt Template with Attachments

```csharp
<PromptItemTemplate Context="promptContext">
    <div class="prompt-item p-3">
        <div class="d-flex">
            <div class="avatar-circle me-3">
                @GetUserInitials()
            </div>
            <div class="flex-grow-1">
                <div class="prompt-header mb-2">
                    <strong>@GetUserName()</strong>
                    <span class="badge bg-secondary ms-2">Prompt #@(promptContext.Index + 1)</span>
                </div>
                
                <div class="prompt-text">
                    @((MarkupString)promptContext.Prompt)
                </div>
                
                @if (promptContext.AttachedFiles?.Count > 0)
                {
                    <div class="attached-files mt-2">
                        <small class="text-muted">
                            <i class="e-icons e-attachment"></i> 
                            @promptContext.AttachedFiles.Count file(s) attached
                        </small>
                        @foreach (var file in promptContext.AttachedFiles)
                        {
                            <div class="file-chip">
                                <i class="e-icons e-file"></i> @file.Name
                            </div>
                        }
                    </div>
                }
            </div>
        </div>
    </div>
</PromptItemTemplate>

@code {
    private string GetUserName() => "John Doe";
    private string GetUserInitials() => "JD";
}
```

## ResponseItemTemplate

Customize how AI responses are displayed.

### Basic Response Template

```csharp
<ResponseItemTemplate Context="responseContext">
    <div class="custom-response p-3 mb-2">
        <div class="d-flex align-items-start">
            <div class="response-avatar me-3">
                <i class="e-assistview-icon"></i>
            </div>
            <div class="response-content flex-grow-1">
                <div class="response-header d-flex justify-content-between">
                    <strong>AI Assistant</strong>
                    <small class="text-muted">@DateTime.Now.ToString("h:mm tt")</small>
                </div>
                <div class="response-text mt-2">
                    @((MarkupString)responseContext.Response)
                </div>
            </div>
        </div>
    </div>
</ResponseItemTemplate>
```

### Response Template with Actions

```csharp
<ResponseItemTemplate Context="responseContext">
    <div class="response-card mb-3">
        <div class="response-header p-2 bg-light">
            <div class="d-flex justify-content-between align-items-center">
                <div>
                    <i class="e-assistview-icon me-2"></i>
                    <strong>AI Response</strong>
                </div>
                <div class="response-actions">
                    <button class="btn btn-sm btn-link" @onclick="() => CopyResponse(responseContext.Response)">
                        <i class="e-icons e-copy"></i>
                    </button>
                    <button class="btn btn-sm btn-link" @onclick="() => LikeResponse(responseContext.Index)">
                        <i class="e-icons e-like"></i>
                    </button>
                    <button class="btn btn-sm btn-link" @onclick="() => RegenerateResponse(responseContext.Prompt)">
                        <i class="e-icons e-refresh"></i>
                    </button>
                </div>
            </div>
        </div>
        
        <div class="response-body p-3">
            @((MarkupString)responseContext.Response)
        </div>
        
        <div class="response-footer p-2 bg-light">
            <small class="text-muted">
                Response #@(responseContext.Index + 1) • @GetResponseLength(responseContext.Response) characters
            </small>
        </div>
    </div>
</ResponseItemTemplate>

@code {
    private void CopyResponse(string response)
    {
        // Implement copy logic
        Console.WriteLine("Copied response");
    }

    private void LikeResponse(int index)
    {
        Console.WriteLine($"Liked response {index}");
    }

    private void RegenerateResponse(string prompt)
    {
        Console.WriteLine($"Regenerating response for: {prompt}");
    }

    private int GetResponseLength(string response)
    {
        return System.Text.RegularExpressions.Regex.Replace(response, "<.*?>", "").Length;
    }
}
```

## PromptSuggestionItemTemplate

Customize the appearance of prompt suggestion chips.

### Basic Suggestion Template

```csharp
<PromptSuggestionItemTemplate Context="suggestionContext">
    <button class="custom-suggestion-chip" @onclick="() => SelectSuggestion(suggestionContext.PromptSuggestion)">
        <i class="e-icons e-lightbulb me-2"></i>
        @suggestionContext.PromptSuggestion
    </button>
</PromptSuggestionItemTemplate>

<style>
    .custom-suggestion-chip {
        border: 2px solid #0066cc;
        background: white;
        color: #0066cc;
        padding: 8px 16px;
        border-radius: 20px;
        margin: 4px;
        cursor: pointer;
        transition: all 0.2s;
    }
    
    .custom-suggestion-chip:hover {
        background: #0066cc;
        color: white;
    }
</style>

@code {
    private void SelectSuggestion(string suggestion)
    {
        Console.WriteLine($"Selected: {suggestion}");
    }
}
```

### Category-Based Suggestion Template

```csharp
<PromptSuggestionItemTemplate Context="suggestionContext">
    <div class="suggestion-card" @onclick="() => SelectSuggestion(suggestionContext.PromptSuggestion)">
        <div class="suggestion-icon">
            @GetSuggestionIcon(suggestionContext.PromptSuggestion)
        </div>
        <div class="suggestion-text">
            @suggestionContext.PromptSuggestion
        </div>
        <div class="suggestion-arrow">
            <i class="e-icons e-chevron-right"></i>
        </div>
    </div>
</PromptSuggestionItemTemplate>

<style>
    .suggestion-card {
        display: flex;
        align-items: center;
        padding: 12px 16px;
        margin: 8px 0;
        border: 1px solid #ddd;
        border-radius: 8px;
        cursor: pointer;
        transition: all 0.2s;
    }
    
    .suggestion-card:hover {
        border-color: #0066cc;
        background: #f0f7ff;
        transform: translateX(4px);
    }
    
    .suggestion-icon {
        margin-right: 12px;
        font-size: 24px;
    }
    
    .suggestion-text {
        flex-grow: 1;
    }
    
    .suggestion-arrow {
        color: #999;
    }
</style>

@code {
    private void SelectSuggestion(string suggestion)
    {
        Console.WriteLine($"Selected: {suggestion}");
    }

    private string GetSuggestionIcon(string suggestion)
    {
        if (suggestion.Contains("code")) return "💻";
        if (suggestion.Contains("write")) return "✍️";
        if (suggestion.Contains("data")) return "📊";
        return "💡";
    }
}
```

## FooterTemplate

Customize the footer area of the component.

### Basic Footer Template

```csharp
<FooterTemplate>
    <div class="custom-footer p-3 border-top">
        <div class="d-flex justify-content-between align-items-center">
            <div>
                <small class="text-muted">
                    Powered by AI • <a href="/privacy">Privacy</a> • <a href="/terms">Terms</a>
                </small>
            </div>
            <div>
                <button class="btn btn-sm btn-link">
                    <i class="e-icons e-settings"></i> Settings
                </button>
                <button class="btn btn-sm btn-link">
                    <i class="e-icons e-help"></i> Help
                </button>
            </div>
        </div>
    </div>
</FooterTemplate>
```

### Footer with Statistics

```csharp
<FooterTemplate>
    <div class="stats-footer p-3 bg-light">
        <div class="row text-center">
            <div class="col">
                <div class="stat-value">@conversationCount</div>
                <div class="stat-label">Conversations</div>
            </div>
            <div class="col">
                <div class="stat-value">@promptCount</div>
                <div class="stat-label">Prompts</div>
            </div>
            <div class="col">
                <div class="stat-value">@GetAverageResponseTime()ms</div>
                <div class="stat-label">Avg Response</div>
            </div>
        </div>
    </div>
</FooterTemplate>

<style>
    .stat-value {
        font-size: 24px;
        font-weight: bold;
        color: #0066cc;
    }
    
    .stat-label {
        font-size: 12px;
        color: #666;
        text-transform: uppercase;
    }
</style>

@code {
    private int conversationCount = 5;
    private int promptCount = 42;

    private int GetAverageResponseTime()
    {
        return 850; // milliseconds
    }
}
```

## ViewTemplate

Complete customization of the view's appearance and layout.

### Custom View Layout

```csharp
<ViewTemplate>
    <div class="custom-view-layout">
        <!-- Custom Header -->
        <div class="custom-header p-3 bg-primary text-white">
            <h4><i class="e-icons e-assistview-icon me-2"></i>Custom AI Assistant</h4>
        </div>
        
        <!-- Custom Content Area -->
        <div class="custom-content p-3">
            <div class="chat-messages">
                <!-- Your custom rendering of messages -->
                @foreach (var prompt in GetPrompts())
                {
                    <div class="message-pair mb-3">
                        <div class="user-message">@prompt.Prompt</div>
                        <div class="ai-message">@prompt.Response</div>
                    </div>
                }
            </div>
        </div>
        
        <!-- Custom Input Area -->
        <div class="custom-input p-3 border-top">
            <div class="input-group">
                <input type="text" class="form-control" placeholder="Type your message..." />
                <button class="btn btn-primary">
                    <i class="e-icons e-send"></i>
                </button>
            </div>
        </div>
    </div>
</ViewTemplate>

@code {
    private List<AssistViewPrompt> GetPrompts()
    {
        return new List<AssistViewPrompt>();
    }
}
```

## Template Contexts

### PromptItemTemplateContext

Available properties in prompt templates:

```csharp
<PromptItemTemplate Context="ctx">
    <div>
        <!-- Index of the prompt (0-based) -->
        <div>Prompt #@(ctx.Index + 1)</div>
        
        <!-- The prompt text -->
        <div>@ctx.Prompt</div>
        
        <!-- Attached files (if any) -->
        @if (ctx.AttachedFiles?.Count > 0)
        {
            <div>Files: @ctx.AttachedFiles.Count</div>
        }
        
        <!-- Toolbar items -->
        @if (ctx.ToolbarItems?.Count > 0)
        {
            <div>Actions: @ctx.ToolbarItems.Count</div>
        }
    </div>
</PromptItemTemplate>
```

### ResponseItemTemplateContext

Available properties in response templates:

```csharp
<ResponseItemTemplate Context="ctx">
    <div>
        <!-- Index of the response -->
        <div>Response #@(ctx.Index + 1)</div>
        
        <!-- The original prompt -->
        <div><strong>Q:</strong> @ctx.Prompt</div>
        
        <!-- The response content -->
        <div><strong>A:</strong> @((MarkupString)ctx.Response)</div>
        
        <!-- Toolbar items -->
        @if (ctx.ToolbarItems?.Count > 0)
        {
            @foreach (var item in ctx.ToolbarItems)
            {
                <button class="btn btn-sm">
                    <i class="@item.IconCss"></i> @item.Text
                </button>
            }
        }
    </div>
</ResponseItemTemplate>
```

### PromptSuggestionItemTemplateContext

Available properties in suggestion templates:

```csharp
<PromptSuggestionItemTemplate Context="ctx">
    <div class="suggestion" data-index="@ctx.Index">
        @ctx.PromptSuggestion
    </div>
</PromptSuggestionItemTemplate>
```

## Best Practices

1. **Keep templates performant** - Avoid heavy computations in templates
2. **Use MarkupString safely** - Sanitize user input to prevent XSS attacks
3. **Maintain accessibility** - Ensure custom templates are keyboard-navigable
4. **Test responsiveness** - Verify templates work on different screen sizes
5. **Provide fallbacks** - Handle null/empty data gracefully
6. **Use consistent styling** - Match your application's design system
7. **Consider dark mode** - Ensure templates work with different themes
8. **Optimize images** - Use appropriate image sizes in templates

### Complete Example

```csharp
@page "/templated-assistant"
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 600px;">
    <SfAIAssistView 
        PromptRequested="@OnPromptRequested"
        Prompts="@conversationHistory">
        <AssistViews>
            <AssistView Header="AI Assistant">
                <BannerTemplate>
                    <div class="welcome-banner text-center p-5">
                        <h2>🤖 AI Code Assistant</h2>
                        <p>Ask me anything about programming!</p>
                    </div>
                </BannerTemplate>
                
                <PromptItemTemplate Context="promptCtx">
                    <div class="prompt-message p-3">
                        <div class="d-flex">
                            <div class="avatar me-3">👤</div>
                            <div>
                                <strong>You:</strong>
                                <div>@promptCtx.Prompt</div>
                            </div>
                        </div>
                    </div>
                </PromptItemTemplate>
                
                <ResponseItemTemplate Context="responseCtx">
                    <div class="response-message p-3 bg-light">
                        <div class="d-flex">
                            <div class="avatar me-3">🤖</div>
                            <div class="flex-grow-1">
                                <strong>AI:</strong>
                                <div>@((MarkupString)responseCtx.Response)</div>
                            </div>
                        </div>
                    </div>
                </ResponseItemTemplate>
                
                <FooterTemplate>
                    <div class="footer-info p-2 text-center small text-muted">
                        @conversationHistory.Count message(s) • Powered by AI
                    </div>
                </FooterTemplate>
            </AssistView>
        </AssistViews>
    </SfAIAssistView>
</div>

@code {
    private List<AssistViewPrompt> conversationHistory = new();

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = $"<div>Response to: {args.PromptText}</div>";
        
        conversationHistory.Add(new AssistViewPrompt
        {
            Prompt = args.PromptText,
            Response = args.Response
        });
    }
}
```

````
