# Advanced Features in Syncfusion Blazor Block Editor

## WebAssembly Integration

The Block Editor works seamlessly with WebAssembly render modes for client-side performance.

### WebAssembly Render Mode

Configure your component to run in the browser:

```razor
@page "/editor"
@rendermode InteractiveWebAssembly

<SfBlockEditor @bind-Blocks="blockData"></SfBlockEditor>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel { BlockType = BlockType.Paragraph }
    };
}
```

### Auto Render Mode (Recommended)

Start on server, switch to WebAssembly:

```razor
@rendermode InteractiveAuto

<SfBlockEditor @bind-Blocks="blockData"></SfBlockEditor>
```

**Benefits:**
- Fast initial page load (server-rendered)
- Rich interactivity (WebAssembly runtime)
- Smooth transition between environments

## Performance Optimization

### 1. Lazy Load Block Content

```razor
@rendermode InteractiveAuto

<SfBlockEditor BlockChanged="@OnBlockChanged"></SfBlockEditor>

@code {
    private List<BlockModel> blockData = new();

    protected override async Task OnInitializedAsync()
    {
        // Load only first 10 blocks initially
        blockData = await LoadInitialBlocks(10);
    }

    private void OnBlockChanged(BlockChangedEventArgs args)
    {
        // Load more blocks as needed
        if (ShouldLoadMore())
        {
            _ = LoadMoreBlocks();
        }
    }

    private async Task<List<BlockModel>> LoadInitialBlocks(int count)
    {
        // Simulate loading from API
        await Task.Delay(100);
        return GetDummyBlocks(count);
    }

    private async Task LoadMoreBlocks()
    {
        await Task.Delay(100);
        // Add more blocks to blockData
    }

    private bool ShouldLoadMore()
    {
        return blockData.Count < 100;
    }

    private List<BlockModel> GetDummyBlocks(int count)
    {
        return Enumerable.Range(0, count)
            .Select(i => new BlockModel 
            { 
                BlockType = BlockType.Paragraph,
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = $"Block {i}" } }
            })
            .ToList();
    }
}
```

### 2. Debounce Change Events

Reduce excessive state updates:

```razor
@rendermode InteractiveAuto

<SfBlockEditor BlockChanged="@OnBlockChanged"></SfBlockEditor>

@code {
    private System.Timers.Timer debounceTimer;
    private const int DebounceDelay = 500; // milliseconds

    protected override void OnInitialized()
    {
        debounceTimer = new System.Timers.Timer(DebounceDelay);
        debounceTimer.Elapsed += async (s, e) => await ProcessChanges();
        debounceTimer.AutoReset = false;
    }

    private void OnBlockChanged(BlockChangedEventArgs args)
    {
        debounceTimer.Stop();
        debounceTimer.Start();
    }

    private async Task ProcessChanges()
    {
        // Perform auto-save or other operations
        await SaveContent();
    }

    private async Task SaveContent()
    {
        // Save to server
        await Task.Delay(100);
        Console.WriteLine("Content saved");
    }

    public void Dispose()
    {
        debounceTimer?.Dispose();
    }
}
```

### 3. Virtual Scrolling for Large Documents

```razor
@rendermode InteractiveAuto

<div style="height: 600px; overflow-y: auto; display: flex; flex-direction: column;">
    <SfBlockEditor @bind-Blocks="visibleBlocks" Height="600px"></SfBlockEditor>
</div>

@code {
    private List<BlockModel> allBlocks = new();
    private List<BlockModel> visibleBlocks = new();
    private const int ItemsPerPage = 20;

    protected override void OnInitialized()
    {
        // Load all blocks, but display only visible ones
        allBlocks = LoadAllBlocks();
        visibleBlocks = allBlocks.Take(ItemsPerPage).ToList();
    }

    private List<BlockModel> LoadAllBlocks()
    {
        return Enumerable.Range(0, 1000)
            .Select(i => new BlockModel 
            { 
                BlockType = BlockType.Paragraph,
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = $"Block {i}" } }
            })
            .ToList();
    }

    private void OnScroll(WheelEventArgs e)
    {
        // Update visible blocks based on scroll position
    }
}
```

## Custom Block Types

Extend the editor with custom blocks:

```razor
@rendermode InteractiveAuto

<SfBlockEditor @bind-Blocks="blockData">
    <BlockEditorCommandMenu Commands="@customCommands" ItemSelect="@OnCommandSelect">
    </BlockEditorCommandMenu>
</SfBlockEditor>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel { BlockType = BlockType.Paragraph }
    };

    private List<CommandItemModel> customCommands = new()
    {
        new CommandItemModel
        {
            ID = "survey-block",
            GroupBy = "Custom",
            Label = "Survey Question",
            IconCss = "e-icons e-comment"
        },
        new CommandItemModel
        {
            ID = "rating-block",
            GroupBy = "Custom",
            Label = "Rating Widget",
            IconCss = "e-icons e-star"
        }
    };

    private void OnCommandSelect(CommandItemSelectEventArgs args)
    {
        if (args.Item.ID == "survey-block")
        {
            InsertSurveyBlock();
        }
        else if (args.Item.ID == "rating-block")
        {
            InsertRatingBlock();
        }
    }

    private void InsertSurveyBlock()
    {
        // Create custom survey block
        var surveyBlock = new BlockModel
        {
            BlockType = BlockType.Paragraph, // Use as container
            Content = new()
            {
                new ContentModel 
                { 
                    ContentType = ContentType.Text, 
                    Content = "Survey Question: [Your question here]"
                }
            }
        };
        blockData.Add(surveyBlock);
    }

    private void InsertRatingBlock()
    {
        // Create custom rating block
        var ratingBlock = new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new()
            {
                new ContentModel 
                { 
                    ContentType = ContentType.Text, 
                    Content = "Rate this: ⭐⭐⭐⭐⭐"
                }
            }
        };
        blockData.Add(ratingBlock);
    }
}
```

## Accessibility Features

### WCAG Compliance

```razor
@rendermode InteractiveAuto

<div>
    <label for="editor-title">Document Title</label>
    <SfBlockEditor @bind-Blocks="blockData" 
                   id="editor-title"
                   role="textbox"
                   aria-label="Rich text editor"
                   aria-describedby="editor-help">
    </SfBlockEditor>
    <div id="editor-help" style="font-size: 12px; color: #666;">
        Use keyboard shortcuts: Ctrl+B for bold, Ctrl+I for italic, / for commands
    </div>
</div>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel { BlockType = BlockType.Paragraph }
    };
}
```

### Keyboard Navigation

```razor
<!-- Editor is fully keyboard accessible by default -->
<!-- Users can navigate without mouse using:
    - Tab: Move between UI elements
    - Arrow Keys: Navigate within blocks
    - Ctrl+Z/Y: Undo/Redo
    - Ctrl+B/I/U: Format text
    - /: Open command menu
-->
```

## Auto-Save Implementation

### Auto-Save with Interval

```razor
@page "/auto-save-editor"
@rendermode InteractiveAuto
@implements IAsyncDisposable

<div style="max-width: 900px; margin: 20px auto;">
    <div style="padding: 15px; background-color: #f5f5f5; margin-bottom: 15px;">
        <h2>Auto-Save Editor</h2>
        <p>Status: <span style="color: @(autoSaveStatus == "Saved" ? "green" : "orange");">@autoSaveStatus</span></p>
        <p>Last Saved: @lastSaveTime</p>
    </div>

    <SfBlockEditor @bind-Blocks="blockData" 
                   Height="500px"
                   BlockChanged="@OnBlockChanged">
    </SfBlockEditor>
</div>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel { BlockType = BlockType.Paragraph }
    };

    private string autoSaveStatus = "Ready";
    private string lastSaveTime = "Never";
    private System.Timers.Timer autoSaveTimer;
    private bool hasUnsavedChanges = false;

    protected override void OnInitialized()
    {
        autoSaveTimer = new System.Timers.Timer(10000); // 10 seconds
        autoSaveTimer.Elapsed += async (s, e) => await PerformAutoSave();
        autoSaveTimer.Start();
    }

    private void OnBlockChanged(BlockChangedEventArgs args)
    {
        hasUnsavedChanges = true;
        autoSaveStatus = "Unsaved changes...";
    }

    private async Task PerformAutoSave()
    {
        if (hasUnsavedChanges)
        {
            autoSaveStatus = "Saving...";
            
            try
            {
                // Save to server
                await SaveToServer();
                
                hasUnsavedChanges = false;
                autoSaveStatus = "Saved";
                lastSaveTime = DateTime.Now.ToString("HH:mm:ss");
            }
            catch (Exception ex)
            {
                autoSaveStatus = "Save failed";
                Console.WriteLine($"Save error: {ex.Message}");
            }

            await InvokeAsync(StateHasChanged);
        }
    }

    private async Task SaveToServer()
    {
        // Simulate API call
        await Task.Delay(500);
    }

    async ValueTask IAsyncDisposable.DisposeAsync()
    {
        autoSaveTimer?.Dispose();
    }
}
```
## Error Handling

### Try-Catch Patterns

```razor
@rendermode InteractiveAuto

<div style="max-width: 900px; margin: 20px auto;">
    @if (!string.IsNullOrEmpty(errorMessage))
    {
        <div style="padding: 15px; background-color: #f8d7da; color: #721c24; border-radius: 4px; margin-bottom: 15px;">
            <strong>Error:</strong> @errorMessage
        </div>
    }

    <SfBlockEditor @bind-Blocks="blockData" BlockChanged="@OnBlockChanged"></SfBlockEditor>
</div>

@code {
    private List<BlockModel> blockData = new();
    private string errorMessage = "";

    protected override async Task OnInitializedAsync()
    {
        try
        {
            blockData = await LoadContent();
        }
        catch (Exception ex)
        {
            errorMessage = $"Failed to load content: {ex.Message}";
        }
    }

    private void OnBlockChanged(BlockChangedEventArgs args)
    {
        try
        {
            ValidateContent();
        }
        catch (Exception ex)
        {
            errorMessage = $"Validation error: {ex.Message}";
        }
    }

    private async Task<List<BlockModel>> LoadContent()
    {
        // Load from API with error handling
        await Task.Delay(100);
        return new() { new BlockModel { BlockType = BlockType.Paragraph } };
    }

    private void ValidateContent()
    {
        if (blockData == null)
            throw new InvalidOperationException("Block data is null");
    }
}
```

## State Persistence

### Save to LocalStorage

```razor
@rendermode InteractiveAuto

<SfBlockEditor @bind-Blocks="blockData" BlockChanged="@OnBlockChanged"></SfBlockEditor>

@code {
    private List<BlockModel> blockData = new();

    protected override async Task OnInitializedAsync()
    {
        // Load from localStorage
        var savedContent = await JS.InvokeAsync<string>("localStorage.getItem", "blockEditorContent");
        if (!string.IsNullOrEmpty(savedContent))
        {
            blockData = System.Text.Json.JsonSerializer.Deserialize<List<BlockModel>>(savedContent);
        }
    }

    private void OnBlockChanged(BlockChangedEventArgs args)
    {
        // Save to localStorage
        var json = System.Text.Json.JsonSerializer.Serialize(blockData);
        _ = JS.InvokeVoidAsync("localStorage.setItem", "blockEditorContent", json);
    }

    [Inject]
    private IJSRuntime JS { get; set; }
}
```

## Common Troubleshooting

| Issue | Solution |
|-------|----------|
| Content not updating | Ensure @bind-Blocks is used correctly |
| Events not firing | Verify render mode is interactive |
| Performance lag | Implement debouncing for BlockChanged |
| Paste not working | Check paste cleanup permissions |
| Shortcuts not working | Verify KeyConfig syntax and render mode |
| WebAssembly issues | Check browser console for JS errors |
