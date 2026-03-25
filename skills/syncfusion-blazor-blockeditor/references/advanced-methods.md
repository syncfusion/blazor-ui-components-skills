# Advanced Methods in Syncfusion Blazor Block Editor

This guide covers advanced programmatic methods for controlling the Block Editor component. These methods enable custom workflows, automation, and fine-grained control over editor behavior.

## Method Overview

| Method | Purpose | Return Type | Async |
|--------|---------|------------|-------|
| `GetSelectedBlocksAsync()` | Get currently selected blocks | `Task<List<BlockModel>>` | ✅ |
| `SelectAllBlocksAsync()` | Select all blocks in editor | `Task` | ✅ |
| `FocusInAsync()` | Set focus to editor | `Task` | ✅ |
| `FocusOutAsync()` | Remove focus from editor | `Task` | ✅ |
| `PrintAsync()` | Print editor content | `Task` | ✅ |
| `EnableToolbarItemsAsync(List<int>)` | Enable toolbar items by index | `Task` | ✅ |
| `DisableToolbarItemsAsync(List<int>)` | Disable toolbar items by index | `Task` | ✅ |

---

## GetSelectedBlocksAsync

Retrieves all currently selected blocks in the editor. This method returns a list of `BlockModel` objects that are currently selected by the user.

### Syntax

```csharp
public async Task<List<BlockModel>> GetSelectedBlocksAsync()
```

### Return Value

Returns a `Task<List<BlockModel>>` containing the selected block models. If no blocks are selected, returns an empty list.

### Example: Get Selected Blocks

```razor
@page "/get-selected"
@rendermode InteractiveAuto
@using Syncfusion.Blazor.BlockEditor

<div style="margin-bottom: 20px;">
    <button class="btn btn-primary" @onclick="GetSelected">Get Selected Blocks</button>
    <button class="btn btn-secondary" @onclick="ClearSelection">Clear Selection</button>
</div>

<div id="result" style="padding: 10px; background-color: #f5f5f5; margin-bottom: 20px; border-radius: 4px;">
    <strong>Selected:</strong> @selectedCount blocks
</div>

<div style="height: 400px; border: 1px solid #ddd; border-radius: 4px;">
    <SfBlockEditor @ref="editorRef" @bind-Blocks="blockData">
        <BlockEditorCommandMenu></BlockEditorCommandMenu>
    </SfBlockEditor>
</div>

@code {
    private SfBlockEditor editorRef;
    private List<BlockModel> blockData = new();
    private int selectedCount = 0;

    protected override void OnInitialized()
    {
        blockData = new()
        {
            new BlockModel { BlockType = BlockType.Heading, Properties = new HeadingBlockSettings { Level = 1 }, 
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Block 1" } } },
            new BlockModel { BlockType = BlockType.Paragraph, 
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Block 2" } } },
            new BlockModel { BlockType = BlockType.Paragraph, 
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Block 3" } } },
            new BlockModel { BlockType = BlockType.Paragraph, 
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Block 4" } } }
        };
    }

    private async Task GetSelected()
    {
        var selected = await editorRef.GetSelectedBlocksAsync();
        selectedCount = selected.Count;
    }

    private async Task ClearSelection()
    {
        selectedCount = 0;
    }
}
```

### Use Cases

1. **Batch Operations**: Get selected blocks to perform bulk actions (delete, duplicate, format)
2. **Selection Analysis**: Determine which blocks are currently selected for UI updates
3. **Content Export**: Export only selected blocks to a different format
4. **Conditional Actions**: Enable/disable features based on selection

---

## SelectAllBlocksAsync

Selects all blocks in the editor programmatically. This is useful for implementing "Select All" functionality or preparing for bulk operations.

### Syntax

```csharp
public async Task SelectAllBlocksAsync()
```

### Example: Select All Blocks

```razor
@page "/select-all"
@rendermode InteractiveAuto
@using Syncfusion.Blazor.BlockEditor

<div style="margin-bottom: 20px;">
    <button class="btn btn-primary" @onclick="SelectAll">Select All Blocks</button>
    <button class="btn btn-warning" @onclick="DeleteSelected">Delete Selected</button>
</div>

<div style="height: 400px; border: 1px solid #ddd; border-radius: 4px;">
    <SfBlockEditor @ref="editorRef" @bind-Blocks="blockData">
        <BlockEditorCommandMenu></BlockEditorCommandMenu>
    </SfBlockEditor>
</div>

@code {
    private SfBlockEditor editorRef;
    private List<BlockModel> blockData = new();

    protected override void OnInitialized()
    {
        blockData = new()
        {
            new BlockModel { BlockType = BlockType.Heading, Properties = new HeadingBlockSettings { Level = 1 },
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Title" } } },
            new BlockModel { BlockType = BlockType.Paragraph,
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Paragraph 1" } } },
            new BlockModel { BlockType = BlockType.Paragraph,
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Paragraph 2" } } }
        };
    }

    private async Task SelectAll()
    {
        await editorRef.SelectAllBlocksAsync();
    }

    private async Task DeleteSelected()
    {
        var selected = await editorRef.GetSelectedBlocksAsync();
        foreach (var block in selected)
        {
            await editorRef.RemoveBlockAsync(block.ID);
        }
    }
}
```

### Use Cases

1. **Select All Feature**: Implement keyboard shortcut (Ctrl+A) handling
2. **Bulk Operations**: Prepare all blocks for formatting or export
3. **Copy All**: Copy entire document content to clipboard
4. **Statistics**: Calculate metrics across all blocks

---

## FocusInAsync

Sets programmatic focus to the Block Editor component. This method ensures the editor receives focus and is ready for user input.

### Syntax

```csharp
public async Task FocusInAsync()
```

### Example: Set Focus to Editor

```razor
@page "/focus-control"
@rendermode InteractiveAuto
@using Syncfusion.Blazor.BlockEditor

<div style="margin-bottom: 20px;">
    <button class="btn btn-info" @onclick="FocusEditor">Focus Editor</button>
    <button class="btn btn-warning" @onclick="BlurEditor">Remove Focus</button>
</div>

<input type="text" placeholder="Click here first" style="margin-bottom: 10px; padding: 8px; width: 100%;" />

<div style="height: 400px; border: 1px solid #ddd; border-radius: 4px;">
    <SfBlockEditor @ref="editorRef" @bind-Blocks="blockData"
                   Focus="@OnFocus" Blur="@OnBlur">
        <BlockEditorCommandMenu></BlockEditorCommandMenu>
    </SfBlockEditor>
</div>

<div style="margin-top: 10px; padding: 10px; background-color: #f0f0f0; border-radius: 4px;">
    <strong>Status:</strong> @focusStatus
</div>

@code {
    private SfBlockEditor editorRef;
    private List<BlockModel> blockData = new();
    private string focusStatus = "Unfocused";

    protected override void OnInitialized()
    {
        blockData = new()
        {
            new BlockModel { BlockType = BlockType.Paragraph,
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Start typing here..." } } }
        };
    }

    private async Task FocusEditor()
    {
        await editorRef.FocusInAsync();
    }

    private async Task BlurEditor()
    {
        await editorRef.FocusOutAsync();
    }

    private void OnFocus(FocusEventArgs args)
    {
        focusStatus = "Focused";
    }

    private void OnBlur(BlurEventArgs args)
    {
        focusStatus = "Unfocused";
    }
}
```

### Use Cases

1. **Auto-Focus**: Focus editor when page loads
2. **Modal Dialog**: Focus editor when modal opens
3. **Dynamic Transitions**: Move focus between editor and other controls
4. **Accessibility**: Ensure focus management for keyboard navigation

---

## FocusOutAsync

Removes focus from the Block Editor component. This method is useful for moving focus to other UI elements or handling focus transitions.

### Syntax

```csharp
public async Task FocusOutAsync()
```

### Example: Remove Focus (see FocusInAsync example above)

### Use Cases

1. **Blur on Save**: Remove focus after saving
2. **Form Navigation**: Move focus to next form field
3. **Dialog Dismissal**: Remove focus when closing overlay
4. **State Management**: Track focus state in application

---

## PrintAsync

Prints the entire Block Editor content. This method opens the browser's print dialog to print the document.

### Syntax

```csharp
public async Task PrintAsync()
```

### Example: Print Editor Content

```razor
@page "/print-document"
@rendermode InteractiveAuto
@using Syncfusion.Blazor.BlockEditor

<div style="margin-bottom: 20px;">
    <button class="btn btn-success" @onclick="PrintContent">Print Document</button>
    <button class="btn btn-info" @onclick="PrintAsHtml">Export as HTML</button>
</div>

<div style="height: 500px; border: 1px solid #ddd; border-radius: 4px;">
    <SfBlockEditor @ref="editorRef" @bind-Blocks="blockData">
        <BlockEditorCommandMenu></BlockEditorCommandMenu>
    </SfBlockEditor>
</div>

@code {
    private SfBlockEditor editorRef;
    private List<BlockModel> blockData = new();

    protected override void OnInitialized()
    {
        blockData = new()
        {
            new BlockModel { BlockType = BlockType.Heading, Properties = new HeadingBlockSettings { Level = 1 },
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Document Title" } } },
            new BlockModel { BlockType = BlockType.Paragraph,
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "This is a sample document that can be printed." } } },
            new BlockModel { BlockType = BlockType.BulletList,
                Content = new() {
                    new ContentModel { ContentType = ContentType.Text, Content = "Point 1" },
                    new ContentModel { ContentType = ContentType.Text, Content = "Point 2" },
                    new ContentModel { ContentType = ContentType.Text, Content = "Point 3" }
                }
            }
        };
    }

    private async Task PrintContent()
    {
        await editorRef.PrintAsync();
    }

    private async Task PrintAsHtml()
    {
        // Export as HTML instead of printing
        var html = await editorRef.GetDataAsHtml(null);
        Console.WriteLine($"HTML Content: {html}");
    }
}
```

### Use Cases

1. **Document Printing**: Print final document to PDF or paper
2. **Print Preview**: Allow users to preview before printing
3. **Export Workflow**: Generate printable version of content
4. **Report Generation**: Print reports created in editor

---

## EnableToolbarItemsAsync

Enables one or more toolbar items by their indices. This allows dynamic control of toolbar availability based on application state.

### Syntax

```csharp
public async Task EnableToolbarItemsAsync(List<int> items)
```

### Parameters

- **items** (`List<int>`): List of toolbar item indices to enable

### Example: Enable/Disable Toolbar Items

```razor
@page "/toolbar-control"
@rendermode InteractiveAuto
@using Syncfusion.Blazor.BlockEditor

<div style="margin-bottom: 20px;">
    <div class="btn-group" role="group">
        <button class="btn btn-outline-primary" @onclick="@(() => ToggleFormatting(true))">
            Enable Formatting
        </button>
        <button class="btn btn-outline-danger" @onclick="@(() => ToggleFormatting(false))">
            Disable Formatting
        </button>
    </div>
</div>

<div style="margin-bottom: 10px; padding: 10px; background-color: #f0f0f0; border-radius: 4px;">
    <strong>Status:</strong> @statusMessage
</div>

<div style="height: 400px; border: 1px solid #ddd; border-radius: 4px;">
    <SfBlockEditor @ref="editorRef" @bind-Blocks="blockData">
        <BlockEditorInlineToolbar Enable="true"></BlockEditorInlineToolbar>
    </SfBlockEditor>
</div>

@code {
    private SfBlockEditor editorRef;
    private List<BlockModel> blockData = new();
    private string statusMessage = "All formatting enabled";

    protected override void OnInitialized()
    {
        blockData = new()
        {
            new BlockModel { BlockType = BlockType.Paragraph,
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Select text to see toolbar options" } } }
        };
    }

    private async Task ToggleFormatting(bool enable)
    {
        // Toolbar item indices: 0=Bold, 1=Italic, 2=Underline, etc.
        var formattingItems = new List<int> { 0, 1, 2, 3, 4, 5 };

        if (enable)
        {
            await editorRef.EnableToolbarItemsAsync(formattingItems);
            statusMessage = "All formatting enabled";
        }
        else
        {
            await editorRef.DisableToolbarItemsAsync(formattingItems);
            statusMessage = "Formatting disabled";
        }
    }
}
```

### Common Toolbar Item Indices

| Index | Action | Description |
|-------|--------|-------------|
| 0 | Bold | Bold text formatting |
| 1 | Italic | Italic text formatting |
| 2 | Underline | Underline text formatting |
| 3 | StrikeThrough | Strikethrough text |
| 4 | TextColor | Change text color |
| 5 | BackgroundColor | Change background color |

### Use Cases

1. **Role-Based Permissions**: Enable/disable features based on user role
2. **Workflow Stages**: Disable editing in finalized documents
3. **Conditional Features**: Enable features only when appropriate
4. **User Level Control**: Restrict advanced formatting for basic users

---

## DisableToolbarItemsAsync

Disables one or more toolbar items by their indices. This prevents users from using specific formatting or editing tools.

### Syntax

```csharp
public async Task DisableToolbarItemsAsync(List<int> items)
```

### Parameters

- **items** (`List<int>`): List of toolbar item indices to disable

### Example: Disable Specific Toolbar Items

```razor
@page "/restrict-formatting"
@rendermode InteractiveAuto
@using Syncfusion.Blazor.BlockEditor

<div style="margin-bottom: 20px;">
    <button class="btn btn-warning" @onclick="RestrictFormatting">Restrict to Basic Formatting</button>
</div>

<div style="height: 400px; border: 1px solid #ddd; border-radius: 4px;">
    <SfBlockEditor @ref="editorRef" @bind-Blocks="blockData">
        <BlockEditorInlineToolbar Enable="true"></BlockEditorInlineToolbar>
    </SfBlockEditor>
</div>

@code {
    private SfBlockEditor editorRef;
    private List<BlockModel> blockData = new();

    protected override void OnInitialized()
    {
        blockData = new()
        {
            new BlockModel { BlockType = BlockType.Paragraph,
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Content with restricted formatting" } } }
        };
    }

    private async Task RestrictFormatting()
    {
        // Disable advanced formatting items (indices 3-5)
        var advancedItems = new List<int> { 3, 4, 5 }; // StrikeThrough, TextColor, BackgroundColor
        await editorRef.DisableToolbarItemsAsync(advancedItems);
    }
}
```

### Use Cases

1. **Content Templates**: Disable features not needed for template
2. **Compliance**: Remove formatting options for compliance requirements
3. **Simple Editors**: Restrict to basic formatting for casual users
4. **Document Types**: Disable features based on document type

---

## Complete Advanced Workflow Example

```razor
@page "/advanced-workflow"
@rendermode InteractiveAuto
@using Syncfusion.Blazor.BlockEditor

<div class="card" style="margin-bottom: 20px;">
    <div class="card-header">
        <h5>Advanced Editor Control Panel</h5>
    </div>
    <div class="card-body">
        <div class="btn-group mb-2" role="group">
            <button class="btn btn-primary" @onclick="SelectAllBlocks">Select All</button>
            <button class="btn btn-info" @onclick="GetSelection">Get Selection</button>
            <button class="btn btn-secondary" @onclick="FocusEditor">Focus</button>
            <button class="btn btn-success" @onclick="PrintDocument">Print</button>
        </div>
        <div style="padding: 10px; background-color: #f5f5f5; border-radius: 4px;">
            <strong>Info:</strong> @infoMessage
        </div>
    </div>
</div>

<div style="height: 500px; border: 1px solid #ddd; border-radius: 4px;">
    <SfBlockEditor @ref="editorRef" @bind-Blocks="blockData">
        <BlockEditorCommandMenu></BlockEditorCommandMenu>
    </SfBlockEditor>
</div>

@code {
    private SfBlockEditor editorRef;
    private List<BlockModel> blockData = new();
    private string infoMessage = "Ready to use advanced methods";

    protected override void OnInitialized()
    {
        blockData = new()
        {
            new BlockModel { BlockType = BlockType.Heading, Properties = new HeadingBlockSettings { Level = 1 },
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Advanced Methods Demo" } } },
            new BlockModel { BlockType = BlockType.Paragraph,
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Use the buttons above to control the editor programmatically." } } }
        };
    }

    private async Task SelectAllBlocks()
    {
        await editorRef.SelectAllBlocksAsync();
        infoMessage = "All blocks selected";
    }

    private async Task GetSelection()
    {
        var selected = await editorRef.GetSelectedBlocksAsync();
        infoMessage = $"{selected.Count} block(s) selected";
    }

    private async Task FocusEditor()
    {
        await editorRef.FocusInAsync();
        infoMessage = "Editor focused - start typing";
    }

    private async Task PrintDocument()
    {
        await editorRef.PrintAsync();
        infoMessage = "Print dialog opened";
    }
}
```

---

## Best Practices

1. **Async Handling**: Always `await` these methods to ensure completion
2. **State Tracking**: Track editor state when using programmatic methods
3. **User Feedback**: Show loading indicators for operations
4. **Error Handling**: Wrap in try-catch for production code
5. **Performance**: Batch operations when possible to avoid frequent updates

---

## Common Patterns

### Pattern 1: Batch Block Operations
```csharp
// Get selected, modify, and update
var selected = await editor.GetSelectedBlocksAsync();
foreach (var block in selected)
{
    // Modify block properties
    await editor.UpdateBlockAsync(block.ID, block);
}
```

### Pattern 2: Keyboard Shortcut Handling
```csharp
// Intercept Ctrl+A for custom handling
if (keyCode == "CtrlA")
{
    await editor.SelectAllBlocksAsync();
    await editor.FocusInAsync();
}
```

### Pattern 3: Dynamic Toolbar Control
```csharp
// Enable/disable based on user role
if (userRole == "Viewer")
{
    await editor.DisableToolbarItemsAsync(new List<int> { 0, 1, 2, 3, 4, 5 });
}
```

---

**Last Updated:** March 24, 2026  
**Component Version:** 1.0.0+  
**Blazor:** .NET 8.0+
