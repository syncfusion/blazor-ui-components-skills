# Events and Interactions in Syncfusion Blazor Block Editor

## Table of Contents
- [Lifecycle Events](#lifecycle-events)
- [Content Change Events](#content-change-events)
- [Selection Events](#selection-events)
- [Focus Events](#focus-events)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Custom Keyboard Configuration](#custom-keyboard-configuration)
- [Event Patterns](#event-patterns)

## Lifecycle Events

### Created Event

Triggers when the Block Editor is successfully initialized and ready for use.

```razor
<SfBlockEditor Created="@OnEditorCreated"></SfBlockEditor>

@code {
    private void OnEditorCreated()
    {
        // Initialize features, load preferences, setup handlers
        Console.WriteLine("Block Editor initialized");
    }
}
```

### Usage Patterns

```razor
@rendermode InteractiveAuto

<div style="padding: 20px;">
    <div style="margin-bottom: 15px; padding: 10px; background-color: #f0f7ff; border-radius: 4px;">
        <strong>Status:</strong> @editorStatus
    </div>

    <SfBlockEditor @bind-Blocks="blockData"
                   Created="@OnEditorCreated"
                   Focus="@OnEditorFocus"
                   Blur="@OnEditorBlur">
    </SfBlockEditor>
</div>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel { BlockType = BlockType.Paragraph }
    };

    private string editorStatus = "Initializing...";

    private void OnEditorCreated()
    {
        editorStatus = "Ready for editing";
    }

    private void OnEditorFocus(FocusEventArgs args)
    {
        editorStatus = "In focus";
    }

    private void OnEditorBlur(BlurEventArgs args)
    {
        editorStatus = "Blurred";
    }
}
```

## Content Change Events

### BlockChanged Event

Triggers whenever editor blocks are modified (added, deleted, or changed).

```razor
<SfBlockEditor BlockChanged="@OnBlockChanged"></SfBlockEditor>

@code {
    private void OnBlockChanged(BlockChangedEventArgs args)
    {
        // args contains information about what changed
        Console.WriteLine($"Blocks changed: {args.Content?.Length ?? 0} items");
    }
}
```

### Track Document Changes

```razor
@rendermode InteractiveAuto

<div style="max-width: 900px; margin: 20px auto;">
    <div style="padding: 15px; background-color: #f5f5f5; margin-bottom: 15px;">
        <h3>Document Change Tracking</h3>
        <div>Total Changes: @changeCount</div>
        <div>Block Count: @blockData.Count</div>
        <div>Last Changed: @lastChangeTime</div>
    </div>

    <SfBlockEditor @bind-Blocks="blockData" 
                   BlockChanged="@OnBlockChanged">
    </SfBlockEditor>
</div>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel { BlockType = BlockType.Paragraph }
    };

    private int changeCount = 0;
    private string lastChangeTime = "Never";

    private void OnBlockChanged(BlockChangedEventArgs args)
    {
        changeCount++;
        lastChangeTime = DateTime.Now.ToString("HH:mm:ss");
        
        // Trigger auto-save
        _ = AutoSaveContent();
    }

    private async Task AutoSaveContent()
    {
        // Simulate API call to save content
        await Task.Delay(1000);
        Console.WriteLine("Content auto-saved");
    }
}
```

## Selection Events

### SelectionChanged Event

Triggers when the user's text selection changes within the editor.

```razor
<SfBlockEditor SelectionChanged="@OnSelectionChanged"></SfBlockEditor>

@code {
    private void OnSelectionChanged(SelectionChangedEventArgs args)
    {
        // args contains selection details
        Console.WriteLine($"Selection changed: {args.SelectedText}");
    }
}
```

### Selection-Based UI Updates

```razor
@rendermode InteractiveAuto

<div style="max-width: 900px; margin: 20px auto;">
    <div style="padding: 15px; background-color: #f5f5f5; margin-bottom: 15px;">
        <h3>Selection Information</h3>
        <div>Selected Text: @selectedText</div>
        <div>Selection Length: @selectionLength</div>
        <div>Has Selection: @(hasSelection ? "Yes" : "No")</div>
    </div>

    <SfBlockEditor @bind-Blocks="blockData" 
                   SelectionChanged="@OnSelectionChanged">
    </SfBlockEditor>
</div>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Select some text to see selection info..." } }
        }
    };

    private string selectedText = "None";
    private int selectionLength = 0;
    private bool hasSelection = false;

    private void OnSelectionChanged(SelectionChangedEventArgs args)
    {
        selectedText = args.SelectedText ?? "None";
        selectionLength = args.SelectedText?.Length ?? 0;
        hasSelection = selectionLength > 0;
    }
}
```

## Focus Events

### Focus Event

Triggers when the editor gains focus.

```razor
<SfBlockEditor Focus="@OnEditorFocus"></SfBlockEditor>

@code {
    private void OnEditorFocus(FocusEventArgs args)
    {
        // Show floating toolbar, highlight border, etc.
        Console.WriteLine("Editor focused");
    }
}
```

### Blur Event

Triggers when the editor loses focus.

```razor
<SfBlockEditor Blur="@OnEditorBlur"></SfBlockEditor>

@code {
    private void OnEditorBlur(BlurEventArgs args)
    {
        // Auto-save, hide toolbars, validate content
        Console.WriteLine("Editor blurred");
    }
}
```

### Complete Focus/Blur Example

```razor
@rendermode InteractiveAuto

<div style="max-width: 900px; margin: 20px auto;">
    <div style="padding: 15px; background-color: #f5f5f5; margin-bottom: 15px;">
        <h3>Focus State: @focusState</h3>
        <p>Focus Count: @focusCount | Blur Count: @blurCount</p>
    </div>

    <div style="border: 3px solid @borderColor; border-radius: 4px; padding: 10px;">
        <SfBlockEditor @bind-Blocks="blockData" 
                       Height="400px"
                       Focus="@OnEditorFocus"
                       Blur="@OnEditorBlur">
        </SfBlockEditor>
    </div>
</div>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Click to focus..." } }
        }
    };

    private string focusState = "Blurred";
    private string borderColor = "#ccc";
    private int focusCount = 0;
    private int blurCount = 0;

    private void OnEditorFocus(FocusEventArgs args)
    {
        focusState = "Focused";
        borderColor = "#007bff";
        focusCount++;
    }

    private void OnEditorBlur(BlurEventArgs args)
    {
        focusState = "Blurred";
        borderColor = "#ccc";
        blurCount++;
    }
}
```

## Keyboard Shortcuts

### Built-In Shortcuts

#### Content Editing and Formatting

| Action | Windows | Mac |
|--------|---------|-----|
| Bold | Ctrl + B | ⌘ + B |
| Italic | Ctrl + I | ⌘ + I |
| Underline | Ctrl + U | ⌘ + U |
| Strikethrough | Ctrl + Shift + X | ⌘ + ⇧ + X |
| Insert Link | Ctrl + K | ⌘ + K |

#### Block Creation and Management

| Action | Windows | Mac |
|--------|---------|-----|
| Create Paragraph | Ctrl + Alt + P | ⌘ + ⌥ + P |
| Create Heading 1 | Ctrl + Alt + 1 | ⌘ + ⌥ + 1 |
| Create Heading 2 | Ctrl + Alt + 2 | ⌘ + ⌥ + 2 |
| Create Bullet List | Ctrl + Shift + 8 | ⌘ + ⇧ + 8 |
| Create Numbered List | Ctrl + Shift + 9 | ⌘ + ⇧ + 9 |
| Duplicate Block | Ctrl + D | ⌘ + D |
| Delete Block | Ctrl + Shift + D | ⌘ + ⇧ + D |
| Move Block Up | Ctrl + Shift + ↑ | ⌘ + ⇧ + ↑ |
| Move Block Down | Ctrl + Shift + ↓ | ⌘ + ⇧ + ↓ |
| Increase Indent | Ctrl + ] or Tab | ⌘ + ] or Tab |
| Decrease Indent | Ctrl + [ or Shift+Tab | ⌘ + [ or ⇧ + Tab |

#### General Operations

| Action | Windows | Mac |
|--------|---------|-----|
| Undo | Ctrl + Z | ⌘ + Z |
| Redo | Ctrl + Y | ⌘ + Y |
| Cut | Ctrl + X | ⌘ + X |
| Copy | Ctrl + C | ⌘ + C |
| Paste | Ctrl + V | ⌘ + V |

## Custom Keyboard Configuration

Override default shortcuts using the `KeyConfig` property:

```razor
<SfBlockEditor KeyConfig="@customKeyConfig"></SfBlockEditor>

@code {
    private Dictionary<string, string> customKeyConfig = new()
    {
        { "Bold", "alt+b" },
        { "Italic", "alt+i" },
        { "Underline", "alt+u" },
        { "Strikethrough", "alt+s" },
        { "Undo", "ctrl+z" },
        { "Redo", "ctrl+y" },
        { "CreateParagraph", "ctrl+alt+p" },
        { "CreateHeading1", "ctrl+alt+1" },
        { "CreateHeading2", "ctrl+alt+2" },
        { "CreateHeading3", "ctrl+alt+3" },
        { "CreateHeading4", "ctrl+alt+4" }
    };
}
```

### Complete Custom Shortcuts Example

```razor
@page "/custom-shortcuts"
@rendermode InteractiveAuto

<div style="max-width: 900px; margin: 20px auto;">
    <div style="padding: 15px; background-color: #f0f7ff; margin-bottom: 15px;">
        <h2>Custom Keyboard Shortcuts</h2>
        <p>Using Alt+B for Bold, Alt+I for Italic, etc.</p>
    </div>

    <SfBlockEditor @bind-Blocks="blockData"
                   Height="500px"
                   KeyConfig="@customShortcuts">
    </SfBlockEditor>

    <div style="margin-top: 20px; padding: 15px; background-color: #f5f5f5;">
        <h3>Custom Shortcuts:</h3>
        <ul style="column-count: 2; gap: 20px;">
            <li>Alt + B = Bold</li>
            <li>Alt + I = Italic</li>
            <li>Alt + U = Underline</li>
            <li>Alt + S = Strikethrough</li>
            <li>Ctrl + Alt + H = Heading 1</li>
            <li>Ctrl + Alt + L = Bullet List</li>
        </ul>
    </div>
</div>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Try custom shortcuts: Alt+B for bold, Alt+I for italic..." } }
        },
        new BlockModel { BlockType = BlockType.Paragraph }
    };

    private Dictionary<string, string> customShortcuts = new()
    {
        { "Bold", "alt+b" },
        { "Italic", "alt+i" },
        { "Underline", "alt+u" },
        { "Strikethrough", "alt+s" },
        { "CreateHeading1", "ctrl+alt+h" },
        { "CreateBulletList", "ctrl+alt+l" },
        { "CreateNumberedList", "ctrl+alt+o" }
    };
}
```

## Event Patterns

### Pattern 1: Complete Editor Workflow

```razor
@rendermode InteractiveAuto

<div style="max-width: 900px; margin: 20px auto;">
    <div style="padding: 15px; background-color: #f5f5f5;">
        <h2>Complete Editor with Events</h2>
        <div>Status: @editorStatus | Changes: @changeCount | Selection: @selectionLength chars</div>
    </div>

    <SfBlockEditor @bind-Blocks="blockData"
                   Created="@OnCreated"
                   Focus="@OnFocus"
                   Blur="@OnBlur"
                   BlockChanged="@OnBlockChanged"
                   SelectionChanged="@OnSelectionChanged">
    </SfBlockEditor>
</div>

@code {
    private List<BlockModel> blockData = new() { new BlockModel { BlockType = BlockType.Paragraph } };
    private string editorStatus = "Initializing...";
    private int changeCount = 0;
    private int selectionLength = 0;

    private void OnCreated() => editorStatus = "Ready";
    private void OnFocus(FocusEventArgs args) => editorStatus = "Focused";
    private void OnBlur(BlurEventArgs args) => editorStatus = "Blurred";
    private void OnBlockChanged(BlockChangedEventArgs args) => changeCount++;
    private void OnSelectionChanged(SelectionChangedEventArgs args) => selectionLength = args.SelectedText?.Length ?? 0;
}
```

### Pattern 2: Auto-Save on Changes

```razor
@rendermode InteractiveAuto

<div style="max-width: 900px; margin: 20px auto;">
    <div style="padding: 15px; background-color: #f5f5f5;">
        <h2>Auto-Save: @autoSaveStatus</h2>
        <p>Last saved: @lastSaveTime</p>
    </div>

    <SfBlockEditor @bind-Blocks="blockData"
                   BlockChanged="@OnBlockChanged">
    </SfBlockEditor>
</div>

@code {
    private List<BlockModel> blockData = new() { new BlockModel { BlockType = BlockType.Paragraph } };
    private string autoSaveStatus = "Idle";
    private string lastSaveTime = "Never";
    private System.Timers.Timer saveTimer;

    protected override void OnInitialized()
    {
        saveTimer = new(3000) { AutoReset = false };
        saveTimer.Elapsed += async (s, e) => await SaveContent();
    }

    private void OnBlockChanged(BlockChangedEventArgs args)
    {
        autoSaveStatus = "Saving...";
        saveTimer.Stop();
        saveTimer.Start();
    }

    private async Task SaveContent()
    {
        await Task.Delay(500);
        lastSaveTime = DateTime.Now.ToString("HH:mm:ss");
        autoSaveStatus = "Saved";
        await InvokeAsync(StateHasChanged);
    }

    public void Dispose() => saveTimer?.Dispose();
}
```

### Pattern 3: Validation on Blur

```razor
<SfBlockEditor Blur="@OnEditorBlur"></SfBlockEditor>

@code {
    private void OnEditorBlur(BlurEventArgs args)
    {
        // Validate content when editor loses focus
        ValidateContent();
    }

    private void ValidateContent()
    {
        // Check content against validation rules
    }
}
```
