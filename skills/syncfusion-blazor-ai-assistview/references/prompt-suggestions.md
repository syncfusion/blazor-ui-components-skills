# Prompt Suggestions

## Table of Contents
- [Adding Suggestions](#adding-suggestions)
- [Suggestion Headers](#suggestion-headers)
- [Mapping Suggestions to Responses](#mapping-suggestions-to-responses)
- [Dynamic Suggestions](#dynamic-suggestions)
- [User Interaction Patterns](#user-interaction-patterns)

## Adding Suggestions

Use the `PromptSuggestions` property to display a list of suggested prompts that help users refine their queries. These suggestions appear both initially and on-demand:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div class="aiassist-container" style="height: 350px; width: 650px;">
    <SfAIAssistView 
        PromptSuggestions="@suggestions" 
        PromptRequested="@OnPromptRequested">
    </SfAIAssistView>
</div>

@code {
    private List<string> suggestions = new()
    {
        "Best practices for clean, maintainable code?",
        "How to optimize code editor for speed?",
        "What are design patterns in C#?"
    };

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        var response1 = "Use clear naming, break code into small functions, avoid repetition, write tests.";
        var response2 = "Install useful extensions, set up shortcuts, enable linting.";
        var response3 = "Design patterns are reusable solutions to common programming problems.";
        
        var defaultResponse = "For real-time prompt processing, connect the AI AssistView component to your preferred AI service.";
        
        args.Response = args.Prompt switch
        {
            var p when p == suggestions[0] => response1,
            var p when p == suggestions[1] => response2,
            var p when p == suggestions[2] => response3,
            _ => defaultResponse
        };
    }
}
```

**Key Benefits:**
- Guides users on available features
- Reduces typing effort
- Provides example queries
- Improves user engagement
- Shows system capabilities upfront

## Suggestion Headers

Use the `PromptSuggestionsHeader` property to add a descriptive header above the suggestions list:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div class="aiassist-container" style="height: 350px; width: 650px;">
    <SfAIAssistView 
        PromptSuggestions="@suggestions"
        PromptSuggestionsHeader="Suggested Prompts"
        PromptRequested="@OnPromptRequested">
    </SfAIAssistView>
</div>

@code {
    private List<string> suggestions = new()
    {
        "Best practices for clean, maintainable code?",
        "How to optimize code editor for speed?",
        "What are design patterns in C#?"
    };

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "Response to your query";
    }
}
```

**Common Header Examples:**
- `"Suggested Prompts"`
- `"Try asking about:"`
- `"Quick Questions"`
- `"Popular Topics"`
- `"Get Started With:"`

## Mapping Suggestions to Responses

Implement response matching for suggestion-based queries:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<SfAIAssistView 
    PromptSuggestions="@suggestions"
    PromptSuggestionsHeader="What would you like to know?"
    PromptRequested="@OnPromptRequested">
</SfAIAssistView>

@code {
    private List<string> suggestions = new()
    {
        "Best practices for clean, maintainable code?",
        "How to optimize code editor for speed?",
        "What are design patterns in C#?"
    };

    // Map suggestions to pre-defined responses
    private Dictionary<string, string> suggestionResponses = new()
    {
        {
            "Best practices for clean, maintainable code?",
            "<div><strong>Clean Code Principles:</strong><ul>" +
            "<li>Use clear, descriptive variable names</li>" +
            "<li>Break code into small, focused functions</li>" +
            "<li>Avoid code repetition (DRY principle)</li>" +
            "<li>Write comprehensive tests</li>" +
            "<li>Follow established coding standards</li>" +
            "</ul></div>"
        },
        {
            "How to optimize code editor for speed?",
            "<div><strong>Performance Tips:</strong><ul>" +
            "<li>Install only essential extensions</li>" +
            "<li>Use keyboard shortcuts</li>" +
            "<li>Enable code linting and formatting</li>" +
            "<li>Customize theme for your preferences</li>" +
            "<li>Disable auto-save for large files</li>" +
            "</ul></div>"
        },
        {
            "What are design patterns in C#?",
            "<div><strong>Common Design Patterns:</strong><ul>" +
            "<li>Singleton: Single instance across application</li>" +
            "<li>Factory: Creating objects without specifying classes</li>" +
            "<li>Observer: Event-driven architecture</li>" +
            "<li>Strategy: Interchangeable algorithms</li>" +
            "<li>Decorator: Adding behavior to objects dynamically</li>" +
            "</ul></div>"
        }
    };

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        
        // Check if prompt matches a suggestion
        if (suggestionResponses.TryGetValue(args.Prompt, out var response))
        {
            args.Response = response;
        }
        else
        {
            args.Response = "For other queries, connect to your preferred AI service.";
        }
    }
}
```

## Dynamic Suggestions

Update suggestions based on conversation context:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<SfAIAssistView 
    PromptSuggestions="@currentSuggestions"
    PromptSuggestionsHeader="@suggestionsHeader"
    PromptRequested="@OnPromptRequested">
</SfAIAssistView>

@code {
    private List<string> currentSuggestions = new();
    private string suggestionsHeader = "Getting Started";
    private int conversationTurn = 0;

    protected override void OnInitialized()
    {
        // Initial suggestions
        currentSuggestions = new()
        {
            "What is Blazor?",
            "How do I create components?",
            "How do I bind data?"
        };
    }

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        conversationTurn++;
        
        // Generate response
        var response = await GenerateResponse(args.Prompt);
        args.Response = response;

        // Update suggestions based on conversation progress
        if (conversationTurn == 1)
        {
            currentSuggestions = new()
            {
                "Tell me about component lifecycle",
                "How do I handle events?",
                "What is state management?"
            };
            suggestionsHeader = "Common Follow-ups";
        }
        else if (conversationTurn == 2)
        {
            currentSuggestions = new()
            {
                "Show me an example",
                "How do I test components?",
                "Where can I learn more?"
            };
            suggestionsHeader = "Next Steps";
        }

        StateHasChanged();
    }

    private async Task<string> GenerateResponse(string prompt)
    {
        await Task.Delay(1000);
        return $"<div>Response to: {prompt}</div>";
    }
}
```

## User Interaction Patterns

### Pattern 1: Hybrid Suggestions and Free Input

Combine suggestions with ability to type custom prompts:

```csharp
<SfAIAssistView 
    PromptSuggestions="@suggestions"
    PromptSuggestionsHeader="Quick Questions"
    PromptPlaceholder="Or type your own question..."
    PromptRequested="@OnPromptRequested">
</SfAIAssistView>

@code {
    private List<string> suggestions = new()
    {
        "What is Blazor?",
        "How do I create a component?"
    };

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        // Handle both suggested and custom prompts
        await Task.Delay(1000);
        
        var isSuggestion = suggestions.Contains(args.Prompt);
        
        if (isSuggestion)
        {
            // Pre-defined response for suggestion
            args.Response = $"<div>Prepared response for: {args.Prompt}</div>";
        }
        else
        {
            // Call AI service for custom prompt
            args.Response = await CallAIService(args.Prompt);
        }
    }

    private async Task<string> CallAIService(string prompt)
    {
        // Call your AI service
        return $"<div>AI response to: {prompt}</div>";
    }
}
```

### Pattern 2: Category-Based Suggestions

Organize suggestions by category or domain:

```csharp
<SfAIAssistView 
    PromptSuggestions="@GetSuggestionsForCategory(currentCategory)"
    PromptSuggestionsHeader="@currentCategory"
    PromptRequested="@OnPromptRequested">
</SfAIAssistView>

@code {
    private string currentCategory = "Getting Started";
    
    private Dictionary<string, List<string>> suggestionsByCategory = new()
    {
        {
            "Getting Started", new()
            {
                "What is Blazor?",
                "How do I set up a project?",
                "What are the prerequisites?"
            }
        },
        {
            "Components", new()
            {
                "How do I create a component?",
                "What is component lifecycle?",
                "How do I pass parameters?"
            }
        },
        {
            "Data Binding", new()
            {
                "How do I bind data to inputs?",
                "What is two-way binding?",
                "How do I handle form submissions?"
            }
        }
    };

    private List<string> GetSuggestionsForCategory(string category)
    {
        return suggestionsByCategory.TryGetValue(category, out var suggestions)
            ? suggestions
            : new();
    }

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = $"<div>Response to: {args.Prompt}</div>";
    }
}
```

## Best Practices

1. **Limit suggestions to 3-5 items** - too many options overwhelm users
2. **Make suggestions specific** - generic suggestions are less helpful
3. **Update suggestions contextually** - change based on conversation progress
4. **Provide clear headers** - describe what suggestions are about
5. **Always handle custom input** - not all users will use suggestions
6. **Pre-define responses for suggestions** - improves performance and consistency
7. **Track suggestion usage** - understand which suggestions are most helpful
