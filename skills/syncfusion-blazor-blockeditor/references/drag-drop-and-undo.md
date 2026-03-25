# Drag-Drop and Undo-Redo in Syncfusion Blazor Block Editor

## Drag and Drop

Enable intuitive block reordering through drag-and-drop functionality.

### Enable Drag and Drop

By default, drag-and-drop is enabled. Use the `EnableDragAndDrop` property to control this behavior:

```razor
<!-- Enable (default) -->
<SfBlockEditor EnableDragAndDrop="true"></SfBlockEditor>

<!-- Disable -->
<SfBlockEditor EnableDragAndDrop="false"></SfBlockEditor>
```

### Single Block Dragging

Users can drag individual blocks by their drag handle (visible on hover):

```razor
@rendermode InteractiveAuto

<div style="max-width: 800px; margin: 20px auto;">
    <h2>Drag and Drop Blocks</h2>
    <p>Hover over a block and drag its handle to reorder</p>
    
    <SfBlockEditor @bind-Blocks="blockData" 
                   EnableDragAndDrop="true"
                   Height="400px">
    </SfBlockEditor>
</div>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 1 },
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Block 1: Drag Me" } }
        },
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "This is the first block - try dragging it" } }
        },
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 1 },
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Block 2: Also Draggable" } }
        },
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "This is the second block" } }
        },
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 1 },
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Block 3: And This One" } }
        },
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "This is the third block" } }
        }
    };
}
```

### Multiple Block Dragging

Select multiple blocks and drag them together:

```razor
@rendermode InteractiveAuto

<div style="max-width: 800px; margin: 20px auto;">
    <h2>Multiple Block Selection and Dragging</h2>
    <p>Select multiple blocks, then drag together</p>
    
    <SfBlockEditor @bind-Blocks="blockData" 
                   EnableDragAndDrop="true"
                   SelectionChanged="@OnSelectionChanged">
    </SfBlockEditor>
    
    <div style="margin-top: 20px; padding: 15px; background-color: #f5f5f5;">
        <strong>Selected Blocks:</strong> @selectedCount
    </div>
</div>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel { BlockType = BlockType.Paragraph, Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Block 1" } } },
        new BlockModel { BlockType = BlockType.Paragraph, Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Block 2" } } },
        new BlockModel { BlockType = BlockType.Paragraph, Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Block 3" } } },
        new BlockModel { BlockType = BlockType.Paragraph, Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Block 4" } } }
    };

    private int selectedCount = 0;

    private void OnSelectionChanged(SelectionChangedEventArgs args)
    {
        // Track selected block count
        // args contains selection information
        selectedCount = args.SelectedText?.Length ?? 0;
    }
}
```

### Visual Feedback During Drag

While dragging, a visual indicator shows the drop location:

```razor
<!-- The editor automatically shows:
    - Drag handle on hover
    - Drop preview line during drag
    - Drop zone highlight when hovering over valid targets
-->
```

## Undo and Redo

Manage content history with undo/redo functionality.

### Keyboard Shortcuts

| Action | Windows | Mac |
|--------|---------|-----|
| Undo | Ctrl + Z | ⌘ + Z |
| Redo | Ctrl + Y | ⌘ + Y |

### Configure Undo/Redo Stack Depth

Control how many actions can be undone/redone:

```razor
<!-- Keep up to 50 actions in history (default is 30) -->
<SfBlockEditor UndoRedoStack="50"></SfBlockEditor>

<!-- Minimal history (10 actions) -->
<SfBlockEditor UndoRedoStack="10"></SfBlockEditor>

<!-- Large history (100 actions) -->
<SfBlockEditor UndoRedoStack="100"></SfBlockEditor>
```

### Complete Undo-Redo Example

```razor
@page "/undo-redo-demo"
@rendermode InteractiveAuto

<div style="max-width: 900px; margin: 20px auto;">
    <div style="padding: 15px; background-color: #f5f5f5; margin-bottom: 15px; border-radius: 4px;">
        <h2>Undo/Redo Demo</h2>
        <p>Make changes and use Ctrl+Z (Undo) or Ctrl+Y (Redo) to revert/repeat</p>
        
        <div style="display: flex; gap: 10px; flex-wrap: wrap;">
            <button @onclick="@OnUndo" style="padding: 8px 16px; background-color: #007bff; color: white; border: none; border-radius: 4px; cursor: pointer;">
                Undo (Ctrl+Z)
            </button>
            <button @onclick="@OnRedo" style="padding: 8px 16px; background-color: #28a745; color: white; border: none; border-radius: 4px; cursor: pointer;">
                Redo (Ctrl+Y)
            </button>
            <button @onclick="@OnReset" style="padding: 8px 16px; background-color: #6c757d; color: white; border: none; border-radius: 4px; cursor: pointer;">
                Reset Content
            </button>
        </div>

        <div style="margin-top: 15px; padding: 10px; background-color: #e7f3ff; border-radius: 4px;">
            <strong>History Depth:</strong> @historyDepth actions
        </div>
    </div>

    <SfBlockEditor @bind-Blocks="blockData"
                   UndoRedoStack="50"
                   Height="500px"
                   BlockChanged="@OnBlockChanged">
    </SfBlockEditor>
</div>

@code {
    private List<BlockModel> blockData;
    private List<BlockModel> originalData;
    private int historyDepth = 50;

    protected override void OnInitialized()
    {
        originalData = GetInitialContent();
        blockData = GetInitialContent();
    }

    private List<BlockModel> GetInitialContent()
    {
        return new()
        {
            new BlockModel
            {
                BlockType = BlockType.Heading,
                Properties = new HeadingBlockSettings { Level = 1 },
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Undo/Redo History Demo" } }
            },
            new BlockModel
            {
                BlockType = BlockType.Paragraph,
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Start editing: add blocks, modify text, delete content..." } }
            },
            new BlockModel
            {
                BlockType = BlockType.BulletList,
                Content = new()
                {
                    new ContentModel { ContentType = ContentType.Text, Content = "Action 1" },
                    new ContentModel { ContentType = ContentType.Text, Content = "Action 2" },
                    new ContentModel { ContentType = ContentType.Text, Content = "Action 3" }
                }
            },
            new BlockModel { BlockType = BlockType.Paragraph }
        };
    }

    private void OnBlockChanged(BlockChangedEventArgs args)
    {
        // Track that changes have been made
        // Can implement auto-save here
    }

    private async Task OnUndo()
    {
        // The editor will undo the last action when available
        // Keyboard shortcut Ctrl+Z also works
    }

    private async Task OnRedo()
    {
        // The editor will redo the last undone action when available
        // Keyboard shortcut Ctrl+Y also works
    }

    private void OnReset()
    {
        blockData = GetInitialContent();
    }
}
```

### Track Changes Pattern

Implement change tracking with undo/redo:

```razor
@rendermode InteractiveAuto

<div style="max-width: 900px; margin: 20px auto;">
    <div style="padding: 15px; background-color: #f5f5f5; margin-bottom: 15px;">
        <h3>Change Tracking with History</h3>
        <p>Changes: @changeCount | Undo Available: @(hasUndo ? "Yes" : "No")</p>
    </div>

    <SfBlockEditor @bind-Blocks="blockData"
                   UndoRedoStack="30"
                   BlockChanged="@OnBlockChanged">
    </SfBlockEditor>
</div>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel { BlockType = BlockType.Paragraph }
    };

    private int changeCount = 0;
    private bool hasUndo = false;

    private void OnBlockChanged(BlockChangedEventArgs args)
    {
        changeCount++;
        // You can also log changes, trigger auto-save, update UI, etc.
    }
}
```

### Auto-Save with Undo History

Combine undo/redo with auto-save:

```razor
@rendermode InteractiveAuto

<div style="max-width: 900px; margin: 20px auto;">
    <div style="padding: 15px; background-color: #f5f5f5; margin-bottom: 15px; display: flex; justify-content: space-between; align-items: center;">
        <div>
            <strong>Status:</strong> @autoSaveStatus
        </div>
        <div>
            <strong>Last Saved:</strong> @lastSaveTime
        </div>
    </div>

    <SfBlockEditor @bind-Blocks="blockData"
                   UndoRedoStack="50"
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
        // Setup auto-save timer (every 5 seconds)
        autoSaveTimer = new System.Timers.Timer(5000);
        autoSaveTimer.Elapsed += async (s, e) => await OnAutoSave();
        autoSaveTimer.Start();
    }

    private void OnBlockChanged(BlockChangedEventArgs args)
    {
        hasUnsavedChanges = true;
        autoSaveStatus = "Unsaved changes...";
    }

    private async Task OnAutoSave()
    {
        if (hasUnsavedChanges)
        {
            // Simulate saving to database
            await Task.Delay(500);
            hasUnsavedChanges = false;
            autoSaveStatus = "Saved";
            lastSaveTime = DateTime.Now.ToString("HH:mm:ss");
            await InvokeAsync(StateHasChanged);
        }
    }

    public void Dispose()
    {
        autoSaveTimer?.Dispose();
    }
}
```

## Combined Drag-Drop and Undo Example

```razor
@page "/drag-undo-complete"
@rendermode InteractiveAuto

<div style="max-width: 900px; margin: 20px auto;">
    <div style="padding: 15px; background-color: #f0f7ff; margin-bottom: 15px; border-radius: 4px;">
        <h2>Block Editor with Full Interactivity</h2>
        <p>Features: Drag-drop reordering, undo/redo, full editing</p>
    </div>

    <SfBlockEditor @bind-Blocks="blockData"
                   Width="100%"
                   Height="600px"
                   EnableDragAndDrop="true"
                   UndoRedoStack="50"
                   BlockChanged="@OnBlockChanged">
        <BlockEditorCommandMenu></BlockEditorCommandMenu>
        <BlockEditorContextMenu Enable="true"></BlockEditorContextMenu>
        <BlockEditorActionMenu Enable="true"></BlockEditorActionMenu>
    </SfBlockEditor>

    <div style="margin-top: 20px; padding: 15px; background-color: #f5f5f5; border-radius: 4px;">
        <div>Block Count: @blockData.Count</div>
        <div>Last Change: @lastChange</div>
        <div style="font-size: 12px; color: #666; margin-top: 10px;">
            Tip: Use Ctrl+Z to undo, Ctrl+Y to redo, drag handles to reorder
        </div>
    </div>
</div>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 1 },
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Complete Editor Demo" } }
        },
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "This editor has full drag-drop and undo/redo support" } }
        },
        new BlockModel
        {
            BlockType = BlockType.BulletList,
            Content = new()
            {
                new ContentModel { ContentType = ContentType.Text, Content = "Drag blocks by their handle" },
                new ContentModel { ContentType = ContentType.Text, Content = "Undo changes with Ctrl+Z" },
                new ContentModel { ContentType = ContentType.Text, Content = "Redo with Ctrl+Y" }
            }
        },
        new BlockModel { BlockType = BlockType.Paragraph }
    };

    private string lastChange = "None";

    private void OnBlockChanged(BlockChangedEventArgs args)
    {
        lastChange = DateTime.Now.ToString("HH:mm:ss");
    }
}
```
