````markdown
# Methods and Programmatic API

## Table of Contents
- [Overview](#overview)
- [ExecutePromptAsync](#executepromptasync)
- [UpdateResponseAsync](#updateresponseasync)
- [RefreshUIAsync](#refreshuiasync)
- [ScrollToBottomAsync](#scrolltobottomasync)
- [Common Usage Patterns](#common-usage-patterns)
- [Best Practices](#best-practices)

## Overview

The AI AssistView component provides several methods for programmatic control, allowing you to trigger prompts, update responses, refresh the UI, and control scrolling behavior without direct user interaction.

**Available Methods:**
- `ExecutePromptAsync(string)` - Execute a prompt programmatically
- `UpdateResponseAsync(string)` - Update streaming response content
- `RefreshUIAsync()` - Refresh component UI and state
- `ScrollToBottomAsync()` - Scroll to bottom of conversation

## ExecutePromptAsync

Execute prompts programmatically without user input.

### Basic Usage

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px;">
    <SfAIAssistView 
        PromptRequested="@OnPromptRequested">
    </SfAIAssistView>
</div>

<div class="mt-3">
    <button class="btn btn-primary" @onclick="ExecuteWelcomePrompt">
        Show Welcome Message
    </button>
    <button class="btn btn-secondary" @onclick="ExecuteHelpPrompt">
        Show Help
    </button>
</div>

@code {

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        
        args.Response = args.PromptText switch
        {
            "welcome" => "<div><h4>Welcome!</h4><p>This is an AI assistant. How can I help you?</p></div>",
            "help" => "<div><h4>Help</h4><ul><li>Ask questions</li><li>Get assistance</li><li>Learn more</li></ul></div>",
            _ => $"<div>Processing: {args.PromptText}</div>"
        };
    }

    private async Task ExecuteWelcomePrompt()
    {
        if (assistView != null)
        {
            await assistView.ExecutePromptAsync("welcome");
        }
    }

    private async Task ExecuteHelpPrompt()
    {
        if (assistView != null)
        {
            await assistView.ExecutePromptAsync("help");
        }
    }
}
```

### Auto-Execute on Initialize

Execute a prompt when the component loads:

```csharp
@code {
    private SfAIAssistView? assistView;
    private bool hasExecutedWelcome = false;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender && !hasExecutedWelcome && assistView != null)
        {
            hasExecutedWelcome = true;
            await Task.Delay(500); // Small delay for component initialization
            await assistView.ExecutePromptAsync("Welcome! How can I assist you today?");
        }
    }

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Thank you for using the AI Assistant!</div>";
    }
}
```

### Automated Workflows

Execute a series of prompts automatically:

```csharp
@code {
    private SfAIAssistView? assistView;

    private async Task RunAutomatedDemo()
    {
        if (assistView == null) return;

        var demoPrompts = new[]
        {
            "What is Blazor?",
            "How do I create a component?",
            "Show me an example"
        };

        foreach (var prompt in demoPrompts)
        {
            await assistView.ExecutePromptAsync(prompt);
            await Task.Delay(3000); // Wait 3 seconds between prompts
        }
    }

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = $"<div>Demo response for: {args.PromptText}</div>";
    }
}

<button class="btn btn-info" @onclick="RunAutomatedDemo">
    Run Demo
</button>
```

### Context-Based Auto-Prompts

Execute prompts based on application context:

```csharp
@code {
    private SfAIAssistView? assistView;
    private string currentPage = "";

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            currentPage = GetCurrentPage();
            await ExecuteContextualPrompt();
        }
    }

    private async Task ExecuteContextualPrompt()
    {
        if (assistView == null) return;

        var contextPrompt = currentPage switch
        {
            "dashboard" => "Show me key metrics and insights",
            "settings" => "What settings can I configure?",
            "profile" => "How do I update my profile?",
            _ => "How can I help you?"
        };

        await assistView.ExecutePromptAsync(contextPrompt);
    }

    private string GetCurrentPage()
    {
        // Get current page from navigation
        return "dashboard";
    }

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = $"<div>Context-specific response for: {args.PromptText}</div>";
    }
}
```

## UpdateResponseAsync

Update streaming response content progressively (see [streaming.md](streaming.md) for detailed examples).

### Basic Update

```csharp
@code {
    private SfAIAssistView? assistView;

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        args.Response = "<div>Starting response...";
        
        // Update response incrementally
        await Task.Delay(500);
        await assistView!.UpdateResponseAsync(" Adding more content...");
        
        await Task.Delay(500);
        await assistView.UpdateResponseAsync(" Final content.</div>");
    }
}
```

## RefreshUIAsync

Refresh the component UI and footer state manually.

### Basic Refresh

```csharp
@code {
    private SfAIAssistView? assistView;
    private List<AssistViewPrompt> prompts = new();

    private async Task AddPromptAndRefresh()
    {
        // Modify prompts collection
        prompts.Add(new AssistViewPrompt
        {
            Prompt = "New prompt",
            Response = "<div>New response</div>"
        });

        // Refresh UI to show changes
        if (assistView != null)
        {
            await assistView.RefreshUIAsync();
        }
    }

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response content</div>";
    }
}

<button class="btn btn-primary" @onclick="AddPromptAndRefresh">
    Add Prompt & Refresh
</button>
```

### Refresh After External Changes

Refresh UI when data changes outside the component:

```csharp
@using System.Timers
@implements IDisposable

@code {
    private SfAIAssistView? assistView;
    private List<AssistViewPrompt> prompts = new();
    private Timer? refreshTimer;

    protected override void OnInitialized()
    {
        // Set up periodic refresh
        refreshTimer = new Timer(5000); // Every 5 seconds
        refreshTimer.Elapsed += async (sender, e) => await RefreshFromServer();
        refreshTimer.Start();
    }

    private async Task RefreshFromServer()
    {
        // Fetch updated data from server
        var newPrompts = await FetchPromptsFromServer();
        
        if (newPrompts.Count != prompts.Count)
        {
            prompts = newPrompts;
            
            if (assistView != null)
            {
                await assistView.RefreshUIAsync();
                StateHasChanged();
            }
        }
    }

    private async Task<List<AssistViewPrompt>> FetchPromptsFromServer()
    {
        // Simulate server fetch
        await Task.Delay(100);
        return prompts;
    }

    public void Dispose()
    {
        refreshTimer?.Stop();
        refreshTimer?.Dispose();
    }

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response</div>";
    }
}
```

### Refresh After Bulk Operations

Refresh UI after adding multiple items:

```csharp
@code {
    private SfAIAssistView? assistView;
    private List<AssistViewPrompt> prompts = new();

    private async Task ImportConversation()
    {
        // Add multiple prompts
        var importedPrompts = new[]
        {
            new AssistViewPrompt { Prompt = "Q1", Response = "<div>A1</div>" },
            new AssistViewPrompt { Prompt = "Q2", Response = "<div>A2</div>" },
            new AssistViewPrompt { Prompt = "Q3", Response = "<div>A3</div>" }
        };

        prompts.AddRange(importedPrompts);

        // Refresh once after all additions
        if (assistView != null)
        {
            await assistView.RefreshUIAsync();
        }
    }
}
```

## ScrollToBottomAsync

Scroll the conversation to the bottom programmatically.

### Basic Scroll

```csharp
@code {
    private SfAIAssistView? assistView;

    private async Task ScrollToLatestMessage()
    {
        if (assistView != null)
        {
            await assistView.ScrollToBottomAsync();
        }
    }
}

<button class="btn btn-secondary" @onclick="ScrollToLatestMessage">
    Scroll to Bottom
</button>
```

### Auto-Scroll After Loading History

Scroll to bottom after loading conversation history:

```csharp
@code {
    private SfAIAssistView? assistView;
    private List<AssistViewPrompt> prompts = new();

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            await LoadConversationHistory();
        }
    }

    private async Task LoadConversationHistory()
    {
        // Load prompts from storage
        prompts = await GetSavedConversation();

        // Wait for UI to render
        await Task.Delay(100);

        // Scroll to bottom to show latest messages
        if (assistView != null)
        {
            await assistView.ScrollToBottomAsync();
        }
    }

    private async Task<List<AssistViewPrompt>> GetSavedConversation()
    {
        // Fetch from storage
        await Task.Delay(500);
        return new List<AssistViewPrompt>
        {
            new AssistViewPrompt { Prompt = "Old Q1", Response = "<div>Old A1</div>" },
            new AssistViewPrompt { Prompt = "Old Q2", Response = "<div>Old A2</div>" }
        };
    }

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response</div>";
    }
}
```

### Scroll on New Message

Automatically scroll when new messages arrive:

```csharp
@code {
    private SfAIAssistView? assistView;

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>New response content</div>";

        // Scroll to show the new response
        await Task.Delay(100); // Wait for render
        if (assistView != null)
        {
            await assistView.ScrollToBottomAsync();
        }
    }
}
```

## Common Usage Patterns

### Pattern 1: Guided Tour

Create an interactive guided tour:

```csharp
@code {
    private SfAIAssistView? assistView;
    private int tourStep = 0;

    private string[] tourPrompts = new[]
    {
        "Welcome to the AI Assistant tour!",
        "You can ask me questions about any topic",
        "Try typing a question in the input box",
        "You can also use the suggested prompts",
        "That's it! Enjoy using the AI Assistant"
    };

    private async Task StartTour()
    {
        for (tourStep = 0; tourStep < tourPrompts.Length; tourStep++)
        {
            if (assistView != null)
            {
                await assistView.ExecutePromptAsync(tourPrompts[tourStep]);
                await Task.Delay(4000); // Wait 4 seconds between steps
            }
        }
    }

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(500);
        args.Response = $"<div>Tour step {tourStep + 1} of {tourPrompts.Length}</div>";
    }
}

<button class="btn btn-success" @onclick="StartTour">
    Start Guided Tour
</button>
```

### Pattern 2: Error Recovery

Automatically retry failed operations:

```csharp
@code {
    private SfAIAssistView? assistView;
    private int retryCount = 0;
    private const int MaxRetries = 3;

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        try
        {
            retryCount = 0;
            var response = await CallAIServiceWithRetry(args.PromptText);
            args.Response = response;
        }
        catch (Exception ex)
        {
            args.Response = $"<div class='alert alert-danger'>Failed after {MaxRetries} retries: {ex.Message}</div>";
        }
    }

    private async Task<string> CallAIServiceWithRetry(string prompt)
    {
        while (retryCount < MaxRetries)
        {
            try
            {
                return await CallAIService(prompt);
            }
            catch (HttpRequestException) when (retryCount < MaxRetries - 1)
            {
                retryCount++;
                await Task.Delay(1000 * retryCount); // Exponential backoff
                
                // Show retry message
                if (assistView != null)
                {
                    await assistView.RefreshUIAsync();
                }
            }
        }

        throw new Exception("Maximum retries exceeded");
    }

    private async Task<string> CallAIService(string prompt)
    {
        // Simulate AI service call
        await Task.Delay(1000);
        return $"<div>Response to: {prompt}</div>";
    }
}
```

### Pattern 3: Batch Processing

Process multiple prompts in batch:

```csharp
@code {
    private SfAIAssistView? assistView;
    private List<string> batchPrompts = new();

    private async Task ProcessBatch()
    {
        batchPrompts = new List<string>
        {
            "Analyze dataset A",
            "Generate report for Q1",
            "Summarize findings"
        };

        foreach (var prompt in batchPrompts)
        {
            if (assistView != null)
            {
                await assistView.ExecutePromptAsync(prompt);
                await Task.Delay(2000); // Wait between prompts
            }
        }

        // Scroll to bottom after batch
        if (assistView != null)
        {
            await assistView.ScrollToBottomAsync();
        }
    }

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        var index = batchPrompts.IndexOf(args.PromptText) + 1;
        args.Response = $"<div>Batch response {index}/{batchPrompts.Count}</div>";
    }
}

<button class="btn btn-primary" @onclick="ProcessBatch">
    Process Batch
</button>
```

### Pattern 4: Real-Time Updates

Update conversation based on external events:

```csharp
@using Microsoft.AspNetCore.SignalR.Client
@implements IAsyncDisposable

@code {
    private SfAIAssistView? assistView;
    private HubConnection? hubConnection;

    protected override async Task OnInitializedAsync()
    {
        hubConnection = new HubConnectionBuilder()
            .WithUrl("https://localhost:5001/chathub")
            .Build();

        hubConnection.On<string>("ReceiveMessage", async (message) =>
        {
            if (assistView != null)
            {
                await assistView.ExecutePromptAsync(message);
            }
        });

        await hubConnection.StartAsync();
    }

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = $"<div>Real-time response: {args.PromptText}</div>";
    }

    public async ValueTask DisposeAsync()
    {
        if (hubConnection != null)
        {
            await hubConnection.DisposeAsync();
        }
    }
}
```

## Best Practices

### 1. Always Check for Null

```csharp
@code {
    private async Task SafeExecutePrompt(string prompt)
    {
        if (assistView != null)
        {
            await assistView.ExecutePromptAsync(prompt);
        }
    }
}
```

### 2. Handle Component Lifecycle

```csharp
@code {
    private SfAIAssistView? assistView;
    private bool isComponentReady = false;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            isComponentReady = true;
            await ExecuteInitialPrompts();
        }
    }

    private async Task ExecuteInitialPrompts()
    {
        if (isComponentReady && assistView != null)
        {
            await assistView.ExecutePromptAsync("Welcome!");
        }
    }
}
```

### 3. Use Try-Catch for Error Handling

```csharp
@code {
    private async Task SafeMethodCall()
    {
        try
        {
            if (assistView != null)
            {
                await assistView.ExecutePromptAsync("test");
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error: {ex.Message}");
            // Show error to user
        }
    }
}
```

### 4. Debounce Rapid Calls

```csharp
@code {
    private DateTime lastRefreshTime = DateTime.MinValue;
    private readonly TimeSpan refreshDebounceInterval = TimeSpan.FromMilliseconds(500);

    private async Task DebouncedRefresh()
    {
        var now = DateTime.Now;
        if (now - lastRefreshTime > refreshDebounceInterval)
        {
            lastRefreshTime = now;
            if (assistView != null)
            {
                await assistView.RefreshUIAsync();
            }
        }
    }
}
```

### 5. Provide User Feedback

```csharp
@code {
    private string statusMessage = "";
    private bool isProcessing = false;

    private async Task ExecuteWithFeedback(string prompt)
    {
        isProcessing = true;
        statusMessage = "Processing...";
        StateHasChanged();

        try
        {
            if (assistView != null)
            {
                await assistView.ExecutePromptAsync(prompt);
                statusMessage = "Complete!";
            }
        }
        catch (Exception ex)
        {
            statusMessage = $"Error: {ex.Message}";
        }
        finally
        {
            isProcessing = false;
            StateHasChanged();
        }
    }
}
```

## Summary

The programmatic API provides powerful control over the AI AssistView component:

- **ExecutePromptAsync** - Automate prompts and create workflows
- **UpdateResponseAsync** - Implement streaming responses
- **RefreshUIAsync** - Sync UI with external data changes
- **ScrollToBottomAsync** - Ensure latest content is visible

Use these methods to create sophisticated AI-powered experiences that respond to user actions, external events, and application state changes.

````
