---
name: syncfusion-blazor-blockeditor
description: Implement Syncfusion Blazor Block Editor for modular, block-based rich content creation. ALWAYS use when building structured document editors with customizable blocks like headings, paragraphs, lists, and media. Immediately configure menus, drag-drop, undo/redo, paste cleanup, and events.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Syncfusion Blazor Block Editor

## Component Overview

The Syncfusion Blazor Block Editor is a powerful, modular content creation component that enables users to build rich, structured documents using customizable blocks. Each block represents a specific content type—headings, paragraphs, lists, tables, images, code blocks, and more—providing a clean, organized editing experience.

### Key Features

- **Block-Based Architecture**: Create structured content using distinct block types (heading, paragraph, list, table, image, code, quote, callout, divider, toggle)
- **Intuitive Menus**: Slash command menu (`/`), context menu (right-click), block action menu (hover), and inline toolbar (text selection)
- **Drag-and-Drop Reordering**: Rearrange blocks intuitively by dragging
- **Undo/Redo Support**: Full history management with configurable stack depth
- **Keyboard Shortcuts**: Comprehensive shortcuts for formatting, block creation, and editor operations
- **Paste Cleanup**: Advanced HTML sanitization, style filtering, and tag removal for safe content pasting
- **Event System**: Created, BlockChanged, SelectionChanged, Focus, Blur, and paste lifecycle events
- **Responsive Design**: Adaptive to different screen sizes and viewports
- **Read-Only Mode**: Display-only content without editing capabilities
- **Custom Styling**: CSS class customization and theme integration

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation via NuGet (Visual Studio, VS Code, .NET CLI)
- Blazor Web App project setup
- Service registration and configuration
- Import namespaces and add theme resources
- Render modes (Server, WebAssembly, Auto)
- Basic component initialization
- Block configuration and data binding

### Appearance and Styling
📄 **Read:** [references/appearance-and-styling.md](references/appearance-and-styling.md)
- Set component width and height
- Read-only mode configuration
- Custom CSS class application
- Responsive design patterns
- Theme customization and styling approaches
- Style examples and best practices

### Editor Menus
📄 **Read:** [references/editor-menus.md](references/editor-menus.md)
- Slash command menu (built-in items, customization, events)
- Context menu (right-click actions, customization, events)
- Block action menu (drag handle actions, customization, events)
- Inline toolbar (text formatting options, customization, events)
- Menu event handling and filtering
- Custom command and menu item creation

### Built-In Blocks
📄 **Read:** [references/built-in-blocks.md](references/built-in-blocks.md)
- Heading blocks (levels 1-4)
- Paragraph blocks
- List types (bullet, numbered, checklist)
- Table blocks
- Code blocks
- Image/media blocks
- Quote and callout blocks
- Toggle (collapsible) blocks
- Divider blocks
- Block nesting and hierarchy

### Drag-Drop and Undo-Redo
📄 **Read:** [references/drag-drop-and-undo.md](references/drag-drop-and-undo.md)
- Enable/disable drag-and-drop
- Single and multiple block dragging
- Undo/redo keyboard shortcuts
- Configure undo/redo stack depth
- History management and state tracking
- Drag operation visual feedback

### Events and Interactions
📄 **Read:** [references/events-and-interactions.md](references/events-and-interactions.md)
- Created event (initialization)
- BlockChanged event (structural changes)
- SelectionChanged event (text selection)
- Focus and Blur events (editor state)
- Keyboard shortcuts (content editing, block creation, block management, general operations)
- Custom keyboard shortcut configuration (KeyConfig)
- Event handler patterns and examples

### Content Handling
📄 **Read:** [references/content-handling.md](references/content-handling.md)
- Getting and setting block content
- Content model structure (BlockModel, ContentModel)
- Paste cleanup configuration (AllowedStyles, DeniedTags, KeepFormat, PlainText)
- PasteCleanupStarting and PasteCleanupCompleted events
- Security best practices (XSS prevention)
- Content validation patterns

### Labels and Mentions
📄 **Read:** [references/labels-and-mentions.md](references/labels-and-mentions.md)
- UserModel for mentioning users (@mentions)
- LabelItemModel for tagging content (#labels)
- BlockEditorLabel component configuration
- MentionContentSettings and LabelContentSettings
- Implementing collaborative mentions and labels
- Practical workflow examples and patterns

### Advanced Methods
📄 **Read:** [references/advanced-methods.md](references/advanced-methods.md)
- GetSelectedBlocksAsync() - Retrieve selected blocks
- SelectAllBlocksAsync() - Select all blocks programmatically
- FocusInAsync() / FocusOutAsync() - Manage editor focus
- PrintAsync() - Print editor content
- EnableToolbarItemsAsync() / DisableToolbarItemsAsync() - Control toolbar availability
- Advanced workflow patterns and use cases

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- WebAssembly integration and considerations
- Performance optimization strategies
- Custom block type implementation
- Accessibility features (WCAG compliance)
- Auto-save and state persistence patterns
- Common troubleshooting scenarios

## Quick Start Example

```razor
@using Syncfusion.Blazor.BlockEditor

@rendermode InteractiveAuto

<div id="container" style="height: 500px; width: 100%;">
    <SfBlockEditor @bind-Blocks="blockData" EnableDragAndDrop="true">
        <BlockEditorCommandMenu></BlockEditorCommandMenu>
        <BlockEditorContextMenu Enable="true"></BlockEditorContextMenu>
        <BlockEditorActionMenu Enable="true"></BlockEditorActionMenu>
        <BlockEditorInlineToolbar Enable="true"></BlockEditorInlineToolbar>
        <BlockEditorPasteCleanup AllowedStyles="@(new string[] { "font-weight", "font-style", "text-decoration" })"
                                 DeniedTags="@(new string[] { "script", "iframe" })">
        </BlockEditorPasteCleanup>
    </SfBlockEditor>
</div>

@code {
    private List<BlockModel> blockData = new List<BlockModel>
    {
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 1 },
            Content = new List<ContentModel>
            {
                new ContentModel { ContentType = ContentType.Text, Content = "Welcome to Block Editor" }
            }
        },
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new List<ContentModel>
            {
                new ContentModel { ContentType = ContentType.Text, Content = "Start typing or use / to add new blocks..." }
            }
        }
    };
}
```

## Common Patterns

### Pattern 1: Event-Driven Content Tracking
Monitor document changes in real-time for auto-save or logging:

```razor
<SfBlockEditor BlockChanged="@OnBlockChanged" 
               SelectionChanged="@OnSelectionChanged">
</SfBlockEditor>

@code {
    private void OnBlockChanged(BlockChangedEventArgs args)
    {
        // Auto-save, track changes, update UI
    }

    private void OnSelectionChanged(SelectionChangedEventArgs args)
    {
        // Update toolbar state based on selection
    }
}
```

### Pattern 2: Read-Only Preview Mode
Display finalized content without editing:

```razor
<SfBlockEditor @bind-Blocks="blockData" ReadOnly="true">
</SfBlockEditor>
```

### Pattern 3: Custom Keyboard Shortcuts
Override default shortcuts for application-specific commands:

```razor
<SfBlockEditor KeyConfig="@customShortcuts">
</SfBlockEditor>

@code {
    private Dictionary<string, string> customShortcuts = new()
    {
        { "Bold", "alt+b" },
        { "Italic", "alt+i" }
    };
}
```

### Pattern 4: Secure Paste with Content Filtering
Control paste behavior for safety and consistency:

```razor
<BlockEditorPasteCleanup AllowedStyles="@allowedStyles"
                         DeniedTags="@deniedTags"
                         PlainText="false">
</BlockEditorPasteCleanup>

@code {
    private string[] allowedStyles = { "font-weight", "font-style" };
    private string[] deniedTags = { "script", "iframe", "object" };
}
```

## Key Properties

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `Blocks` | `List<BlockModel>` | Empty | The content blocks bound to the editor |
| `Width` | `string` | "100%" | Editor container width |
| `Height` | `string` | "auto" | Editor container height |
| `ReadOnly` | `bool` | false | Enable/disable read-only mode |
| `EnableDragAndDrop` | `bool` | true | Enable/disable block dragging |
| `UndoRedoStack` | `int` | 30 | Maximum undo/redo history depth |
| `CssClass` | `string` | "" | Custom CSS classes for styling |
| `KeyConfig` | `Dictionary<string, string>` | Default shortcuts | Custom keyboard shortcuts |

## Common Use Cases

1. **Blog Platform**: Create a rich blog editor with headings, images, and formatting
2. **Document Management**: Build document creators for articles and reports
3. **Knowledge Base**: Implement structured content editors for documentation
5. **Content Preview**: Display finalized documents in read-only mode
6. **Form Builder**: Use blocks to create dynamic form templates
7. **Email Templates**: Create email templates with customizable blocks
8. **Proposal Builder**: Generate professional proposals with block-based structure
