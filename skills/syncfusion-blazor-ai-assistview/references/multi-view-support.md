````markdown
# Multi-View Support

## Table of Contents
- [Overview](#overview)
- [Creating Multiple Views](#creating-multiple-views)
- [Active View Management](#active-view-management)
- [View Configuration](#view-configuration)
- [View Switching](#view-switching)
- [View-Specific Settings](#view-specific-settings)
- [Common Patterns](#common-patterns)

## Overview

The AI AssistView component supports multiple views, allowing you to create specialized AI assistants for different purposes within the same component. Each view can have its own header, icon, banner, templates, and behavior.

**Use Cases:**
- Different AI personalities (e.g., "Code Assistant", "Writing Helper", "Data Analyst")
- Domain-specific assistants (e.g., "Sales", "Support", "Marketing")
- Language-specific assistants
- Task-specific workflows

## Creating Multiple Views

Use the `AssistViews` collection to define multiple assist views:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 500px; width: 700px;">
    <SfAIAssistView 
        PromptRequested="@OnPromptRequested"
        ActiveView="@activeViewIndex"
        ActiveViewChanged="@OnActiveViewChanged">
        <AssistViews>
            <AssistView Header="Code Assistant" IconCss="e-icons e-code">
            </AssistView>
            <AssistView Header="Writing Helper" IconCss="e-icons e-edit">
            </AssistView>
            <AssistView Header="Data Analyst" IconCss="e-icons e-chart">
            </AssistView>
        </AssistViews>
    </SfAIAssistView>
</div>

@code {
    private int activeViewIndex = 0;

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        
        // Respond based on active view
        var response = activeViewIndex switch
        {
            0 => $"<div><strong>[Code Assistant]</strong> {GenerateCodeResponse(args.PromptText)}</div>",
            1 => $"<div><strong>[Writing Helper]</strong> {GenerateWritingResponse(args.PromptText)}</div>",
            2 => $"<div><strong>[Data Analyst]</strong> {GenerateDataResponse(args.PromptText)}</div>",
            _ => "<div>Response</div>"
        };
        
        args.Response = response;
    }

    private void OnActiveViewChanged(int newIndex)
    {
        activeViewIndex = newIndex;
        Console.WriteLine($"Switched to view: {newIndex}");
    }

    private string GenerateCodeResponse(string prompt)
    {
        return "Here's a code solution...";
    }

    private string GenerateWritingResponse(string prompt)
    {
        return "Here's a writing suggestion...";
    }

    private string GenerateDataResponse(string prompt)
    {
        return "Here's a data analysis...";
    }
}
```

## Active View Management

### Getting and Setting Active View

Control which view is currently displayed using the `ActiveView` property:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div class="view-switcher mb-3">
    <button class="btn btn-primary" @onclick="() => SwitchToView(0)">Code Assistant</button>
    <button class="btn btn-primary" @onclick="() => SwitchToView(1)">Writing Helper</button>
    <button class="btn btn-primary" @onclick="() => SwitchToView(2)">Data Analyst</button>
</div>

<div style="height: 500px;">
    <SfAIAssistView 
        @bind-ActiveView="activeViewIndex"
        PromptRequested="@OnPromptRequested">
        <AssistViews>
            <AssistView Header="Code Assistant" IconCss="e-icons e-code"></AssistView>
            <AssistView Header="Writing Helper" IconCss="e-icons e-edit"></AssistView>
            <AssistView Header="Data Analyst" IconCss="e-icons e-chart"></AssistView>
        </AssistViews>
    </SfAIAssistView>
</div>

@code {
    private int activeViewIndex = 0;

    private void SwitchToView(int index)
    {
        activeViewIndex = index;
    }

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = $"<div>Response from view {activeViewIndex}</div>";
    }
}
```

### Listening to View Changes

React to view changes using the `ActiveViewChanged` event:

```csharp
@code {
    private int activeViewIndex = 0;
    private string currentViewName = "Code Assistant";

    private void OnActiveViewChanged(int newIndex)
    {
        activeViewIndex = newIndex;
        
        currentViewName = newIndex switch
        {
            0 => "Code Assistant",
            1 => "Writing Helper",
            2 => "Data Analyst",
            _ => "Unknown"
        };
        
        // Load view-specific data
        LoadViewData(newIndex);
        
        StateHasChanged();
    }

    private void LoadViewData(int viewIndex)
    {
        // Load conversation history for this view
        // Reset prompts or load saved state
        Console.WriteLine($"Loading data for view: {viewIndex}");
    }
}
```

## View Configuration

### Configuring Individual Views

Each `AssistView` can have its own configuration:

```csharp
<SfAIAssistView PromptRequested="@OnPromptRequested">
    <AssistViews>
        <!-- Code Assistant View -->
        <AssistView 
            Header="Code Assistant" 
            IconCss="e-icons e-code"
            ShowClearButton="true">
        </AssistView>
        
        <!-- Writing Helper View -->
        <AssistView 
            Header="Writing Helper" 
            IconCss="e-icons e-edit"
            ShowClearButton="false">
        </AssistView>
        
        <!-- Support Assistant View -->
        <AssistView 
            Header="Customer Support" 
            IconCss="e-icons e-help"
            ShowClearButton="true">
        </AssistView>
    </AssistViews>
</SfAIAssistView>
```

### View Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Header` | `string` | `"AI Assist"` | View name displayed in header/tabs |
| `IconCss` | `string` | `""` | CSS class for view icon |
| `ShowClearButton` | `bool` | `false` | Show clear button in prompt area |
| `BannerTemplate` | `RenderFragment` | `null` | Custom template for initial banner |
| `FooterTemplate` | `RenderFragment` | `null` | Custom template for footer |
| `ViewTemplate` | `RenderFragment` | `null` | Custom template for entire view |
| `PromptItemTemplate` | `RenderFragment` | `null` | Custom template for prompts |
| `ResponseItemTemplate` | `RenderFragment` | `null` | Custom template for responses |

## View Switching

### Programmatic View Switching

Switch views programmatically based on conditions:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 500px;">
    <SfAIAssistView 
        ActiveView="@activeViewIndex"
        ActiveViewChanged="@OnActiveViewChanged"
        PromptRequested="@OnPromptRequested">
        <AssistViews>
            <AssistView Header="General" IconCss="e-icons e-comment"></AssistView>
            <AssistView Header="Code" IconCss="e-icons e-code"></AssistView>
            <AssistView Header="Math" IconCss="e-icons e-calculator"></AssistView>
        </AssistViews>
    </SfAIAssistView>
</div>

@code {
    private int activeViewIndex = 0;

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        // Auto-switch views based on prompt content
        var prompt = args.PromptText.ToLower();
        
        if (prompt.Contains("code") || prompt.Contains("function") || prompt.Contains("class"))
        {
            activeViewIndex = 1; // Switch to Code view
        }
        else if (prompt.Contains("calculate") || prompt.Contains("math") || prompt.Contains("equation"))
        {
            activeViewIndex = 2; // Switch to Math view
        }
        else
        {
            activeViewIndex = 0; // Stay in General view
        }
        
        await Task.Delay(1000);
        args.Response = GenerateViewSpecificResponse(args.PromptText, activeViewIndex);
    }

    private void OnActiveViewChanged(int newIndex)
    {
        activeViewIndex = newIndex;
    }

    private string GenerateViewSpecificResponse(string prompt, int viewIndex)
    {
        return $"<div>Response from view {viewIndex} for: {prompt}</div>";
    }
}
```

### View Navigation UI

Create custom navigation for views:

```csharp
@page "/multi-view-assistant"
@using Syncfusion.Blazor.InteractiveChat

<style>
    .view-tabs {
        display: flex;
        gap: 8px;
        margin-bottom: 16px;
        border-bottom: 2px solid #ddd;
    }
    
    .view-tab {
        padding: 12px 24px;
        border: none;
        background: transparent;
        cursor: pointer;
        font-weight: 500;
        border-bottom: 3px solid transparent;
    }
    
    .view-tab.active {
        color: #0066cc;
        border-bottom-color: #0066cc;
    }
    
    .view-tab:hover {
        background: #f0f0f0;
    }
</style>

<div class="view-tabs">
    @for (int i = 0; i < viewConfigs.Count; i++)
    {
        var index = i;
        var config = viewConfigs[i];
        <button 
            class="view-tab @(activeViewIndex == index ? "active" : "")"
            @onclick="() => SwitchView(index)">
            <i class="@config.IconCss"></i> @config.Name
        </button>
    }
</div>

<div style="height: 500px;">
    <SfAIAssistView 
        ActiveView="@activeViewIndex"
        ActiveViewChanged="@OnActiveViewChanged"
        PromptRequested="@OnPromptRequested"
        ShowHeader="false">
        <AssistViews>
            @foreach (var config in viewConfigs)
            {
                <AssistView Header="@config.Name" IconCss="@config.IconCss"></AssistView>
            }
        </AssistViews>
    </SfAIAssistView>
</div>

@code {
    private int activeViewIndex = 0;

    private List<ViewConfig> viewConfigs = new()
    {
        new ViewConfig { Name = "General", IconCss = "e-icons e-comment" },
        new ViewConfig { Name = "Code", IconCss = "e-icons e-code" },
        new ViewConfig { Name = "Writing", IconCss = "e-icons e-edit" },
        new ViewConfig { Name = "Data", IconCss = "e-icons e-chart" }
    };

    private void SwitchView(int index)
    {
        activeViewIndex = index;
    }

    private void OnActiveViewChanged(int newIndex)
    {
        activeViewIndex = newIndex;
    }

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        var viewName = viewConfigs[activeViewIndex].Name;
        args.Response = $"<div><strong>[{viewName}]</strong> Processing your request...</div>";
    }

    public class ViewConfig
    {
        public string Name { get; set; } = "";
        public string IconCss { get; set; } = "";
    }
}
```

## View-Specific Settings

### Separate Prompts per View

Maintain different conversation histories for each view:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 500px;">
    <SfAIAssistView 
        ActiveView="@activeViewIndex"
        ActiveViewChanged="@OnActiveViewChanged"
        Prompts="@GetPromptsForCurrentView()"
        PromptRequested="@OnPromptRequested">
        <AssistViews>
            <AssistView Header="Support" IconCss="e-icons e-help"></AssistView>
            <AssistView Header="Sales" IconCss="e-icons e-shopping-cart"></AssistView>
            <AssistView Header="Technical" IconCss="e-icons e-settings"></AssistView>
        </AssistViews>
    </SfAIAssistView>
</div>

@code {
    private int activeViewIndex = 0;
    
    private Dictionary<int, List<AssistViewPrompt>> viewPrompts = new()
    {
        { 0, new List<AssistViewPrompt>() }, // Support
        { 1, new List<AssistViewPrompt>() }, // Sales
        { 2, new List<AssistViewPrompt>() }  // Technical
    };

    private List<AssistViewPrompt> GetPromptsForCurrentView()
    {
        return viewPrompts[activeViewIndex];
    }

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        
        var response = GenerateViewSpecificResponse(args.PromptText, activeViewIndex);
        args.Response = response;
        
        // Add to view-specific history
        viewPrompts[activeViewIndex].Add(new AssistViewPrompt
        {
            Prompt = args.PromptText,
            Response = response
        });
    }

    private void OnActiveViewChanged(int newIndex)
    {
        activeViewIndex = newIndex;
        StateHasChanged();
    }

    private string GenerateViewSpecificResponse(string prompt, int viewIndex)
    {
        return viewIndex switch
        {
            0 => $"<div><strong>Support:</strong> How can we help you with {prompt}?</div>",
            1 => $"<div><strong>Sales:</strong> Let me tell you about {prompt}...</div>",
            2 => $"<div><strong>Technical:</strong> Here's a technical analysis of {prompt}...</div>",
            _ => "<div>Response</div>"
        };
    }
}
```

### View-Specific Suggestions

Provide different suggestions for each view:

```csharp
@code {
    private int activeViewIndex = 0;
    
    private Dictionary<int, List<string>> viewSuggestions = new()
    {
        {
            0, new List<string> // Support
            {
                "How do I reset my password?",
                "Where can I find documentation?",
                "How do I contact support?"
            }
        },
        {
            1, new List<string> // Sales
            {
                "What are your pricing plans?",
                "Do you offer enterprise solutions?",
                "What's included in the premium tier?"
            }
        },
        {
            2, new List<string> // Technical
            {
                "How do I integrate the API?",
                "What are the system requirements?",
                "How do I deploy to production?"
            }
        }
    };

    private List<string> GetSuggestionsForCurrentView()
    {
        return viewSuggestions.ContainsKey(activeViewIndex) 
            ? viewSuggestions[activeViewIndex] 
            : new List<string>();
    }
}

<SfAIAssistView 
    ActiveView="@activeViewIndex"
    ActiveViewChanged="@OnActiveViewChanged"
    PromptSuggestions="@GetSuggestionsForCurrentView()"
    PromptRequested="@OnPromptRequested">
    <AssistViews>
        <AssistView Header="Support" IconCss="e-icons e-help"></AssistView>
        <AssistView Header="Sales" IconCss="e-icons e-shopping-cart"></AssistView>
        <AssistView Header="Technical" IconCss="e-icons e-settings"></AssistView>
    </AssistViews>
</SfAIAssistView>
```

## Common Patterns

### Pattern 1: Workspace-Based Views

Create views based on user workspaces or projects:

```csharp
@code {
    private int activeViewIndex = 0;
    private List<Workspace> workspaces = new();

    protected override async Task OnInitializedAsync()
    {
        workspaces = await LoadUserWorkspaces();
    }

    private async Task<List<Workspace>> LoadUserWorkspaces()
    {
        // Load from database
        return new List<Workspace>
        {
            new Workspace { Id = 1, Name = "Project A", IconCss = "e-icons e-folder" },
            new Workspace { Id = 2, Name = "Project B", IconCss = "e-icons e-folder" },
            new Workspace { Id = 3, Name = "Project C", IconCss = "e-icons e-folder" }
        };
    }

    public class Workspace
    {
        public int Id { get; set; }
        public string Name { get; set; } = "";
        public string IconCss { get; set; } = "";
    }
}

<SfAIAssistView 
    ActiveView="@activeViewIndex"
    ActiveViewChanged="@OnActiveViewChanged"
    PromptRequested="@OnPromptRequested">
    <AssistViews>
        @foreach (var workspace in workspaces)
        {
            <AssistView Header="@workspace.Name" IconCss="@workspace.IconCss"></AssistView>
        }
    </AssistViews>
</SfAIAssistView>
```

### Pattern 2: Role-Based Views

Different views for different user roles:

```csharp
@code {
    private int activeViewIndex = 0;
    private string userRole = ""; // "admin", "developer", "user"

    protected override void OnInitialized()
    {
        userRole = GetCurrentUserRole();
        activeViewIndex = GetDefaultViewForRole(userRole);
    }

    private string GetCurrentUserRole()
    {
        // Get from auth system
        return "developer";
    }

    private int GetDefaultViewForRole(string role)
    {
        return role switch
        {
            "admin" => 0,
            "developer" => 1,
            "user" => 2,
            _ => 0
        };
    }
}

<SfAIAssistView 
    ActiveView="@activeViewIndex"
    PromptRequested="@OnPromptRequested">
    <AssistViews>
        <AssistView Header="Admin Console" IconCss="e-icons e-settings"></AssistView>
        <AssistView Header="Developer Tools" IconCss="e-icons e-code"></AssistView>
        <AssistView Header="User Help" IconCss="e-icons e-help"></AssistView>
    </AssistViews>
</SfAIAssistView>
```

### Pattern 3: Language-Based Views

Different views for different languages:

```csharp
@code {
    private int activeViewIndex = 0;
    
    private Dictionary<int, string> viewLanguages = new()
    {
        { 0, "en" },
        { 1, "es" },
        { 2, "fr" },
        { 3, "de" }
    };

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        var language = viewLanguages[activeViewIndex];
        var response = await TranslateAndRespond(args.PromptText, language);
        args.Response = response;
    }

    private async Task<string> TranslateAndRespond(string prompt, string language)
    {
        // Call translation API and generate response
        return $"<div>[{language.ToUpper()}] Response to: {prompt}</div>";
    }
}

<SfAIAssistView 
    ActiveView="@activeViewIndex"
    ActiveViewChanged="@OnActiveViewChanged"
    PromptRequested="@OnPromptRequested">
    <AssistViews>
        <AssistView Header="English" IconCss="e-icons e-comment"></AssistView>
        <AssistView Header="Español" IconCss="e-icons e-comment"></AssistView>
        <AssistView Header="Français" IconCss="e-icons e-comment"></AssistView>
        <AssistView Header="Deutsch" IconCss="e-icons e-comment"></AssistView>
    </AssistViews>
</SfAIAssistView>
```

## Best Practices

1. **Keep view count reasonable** - 3-5 views is optimal for usability
2. **Provide clear view names** - users should understand each view's purpose
3. **Use consistent icons** - help users quickly identify views
4. **Maintain separate state** - each view should have its own conversation history
5. **Save view preference** - remember which view the user was using
6. **Validate view index** - ensure activeViewIndex is within bounds
7. **Load view data lazily** - only load data when view becomes active
8. **Provide view descriptions** - help users understand what each view does

````
