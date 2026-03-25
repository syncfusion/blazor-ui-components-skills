# Editor Menus in Syncfusion Blazor Block Editor

## Table of Contents
- [Slash Command Menu](#slash-command-menu)
- [Context Menu](#context-menu)
- [Block Action Menu](#block-action-menu)
- [Inline Toolbar](#inline-toolbar)
- [Menu Event Patterns](#menu-event-patterns)

## Slash Command Menu

The Slash Command menu (`/`) provides keyboard-driven access to insert or transform blocks without mouse interaction.

### Built-In Commands

Trigger with `/` to see:

| Command | Shortcut | Function |
|---------|----------|----------|
| Paragraph | `/p` | Insert paragraph block |
| Heading 1 | `/h1` | Insert H1 heading |
| Heading 2 | `/h2` | Insert H2 heading |
| Heading 3 | `/h3` | Insert H3 heading |
| Heading 4 | `/h4` | Insert H4 heading |
| Bullet List | `/ul` | Insert bullet list |
| Numbered List | `/ol` | Insert numbered list |
| Checklist | `/cl` | Insert checklist |
| Quote | `/q` | Insert quote block |
| Code Block | `/code` | Insert code block |
| Table | `/table` | Insert table |
| Image | `/img` | Insert image |
| Divider | `/divider` | Insert divider line |
| Callout | `/callout` | Insert callout block |
| Toggle | `/toggle` | Insert toggle/collapsible block |

### Default Slash Command Menu

```razor
<SfBlockEditor>
    <BlockEditorCommandMenu></BlockEditorCommandMenu>
</SfBlockEditor>
```

### Customize Slash Command Menu

Add custom commands or modify behavior:

```razor
@page "/custom-slash-menu"
@rendermode InteractiveAuto

<SfBlockEditor Blocks="@blockData">
    <BlockEditorCommandMenu PopupHeight="400px" 
                            PopupWidth="300px"
                            Commands="@customCommands"
                            ItemSelect="@OnCommandSelect"
                            Filtering="@OnCommandFilter">
    </BlockEditorCommandMenu>
</SfBlockEditor>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel { BlockType = BlockType.Paragraph }
    };

    private List<CommandItemModel> customCommands = new()
    {
        // Keep default block creation commands
        new CommandItemModel
        {
            ID = "para-cmd",
            Type = BlockType.Paragraph,
            GroupBy = "Blocks",
            Label = "Paragraph",
            IconCss = "e-icons e-paragraph"
        },
        new CommandItemModel
        {
            ID = "heading-cmd",
            Type = BlockType.Heading,
            GroupBy = "Blocks",
            Label = "Heading 1",
            IconCss = "e-icons e-heading"
        },
        // Custom application commands
        new CommandItemModel
        {
            ID = "template-cmd",
            GroupBy = "Templates",
            Label = "Insert Template",
            IconCss = "e-icons e-template"
        },
        new CommandItemModel
        {
            ID = "signature-cmd",
            GroupBy = "Special",
            Label = "Add Signature Block",
            IconCss = "e-icons e-signature"
        }
    };

    private void OnCommandSelect(CommandItemSelectEventArgs args)
    {
        // Handle custom command execution
        if (args.Item.ID == "template-cmd")
        {
            // Insert template logic
        }
        else if (args.Item.ID == "signature-cmd")
        {
            // Insert signature block logic
        }
    }

    private void OnCommandFilter(CommandFilteringEventArgs args)
    {
        // Filter commands based on search text
        // Allows smart filtering as user types
    }
}
```

### Slash Menu Events

```razor
<BlockEditorCommandMenu Filtering="@OnFilter" ItemSelect="@OnItemSelect">
</BlockEditorCommandMenu>

@code {
    private void OnFilter(CommandFilteringEventArgs args)
    {
        // args.Text contains the typed filter text
        // Implement fuzzy search or custom filtering logic
    }

    private void OnItemSelect(CommandItemSelectEventArgs args)
    {
        // args.Item contains the selected command
        // args.Name contains the action name
    }
}
```

## Context Menu

Right-click menu for block-level and content-level actions.

### Built-In Context Menu Items

| Item | Function |
|------|----------|
| Undo | Undo last action |
| Redo | Redo last undone action |
| Cut | Cut selected content |
| Copy | Copy selected content |
| Paste | Paste from clipboard |
| Indent | Increase block indent level |
| Outdent | Decrease block indent level |
| Link | Add/edit hyperlink |

### Enable Context Menu

```razor
<SfBlockEditor>
    <BlockEditorContextMenu Enable="true"></BlockEditorContextMenu>
</SfBlockEditor>
```

### Context Menu Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Enable` | bool | true | Enable/disable context menu |
| `ShowItemOnClick` | bool | false | Show submenu on click instead of hover |

### Show Submenu on Click

The `ShowItemOnClick` property controls how submenus are triggered. Set to `true` for better touch device support:

```razor
<!-- Default behavior: Submenu shows on hover -->
<BlockEditorContextMenu Enable="true" ShowItemOnClick="false">
</BlockEditorContextMenu>

<!-- Mobile-friendly: Submenu shows on click -->
<BlockEditorContextMenu Enable="true" ShowItemOnClick="true">
</BlockEditorContextMenu>
```

**Use Cases for ShowItemOnClick:**
- **Touch Devices**: Touch interfaces don't support hover, so set `true` for better UX
- **Desktop with Hover**: Set `false` for immediate submenu display on hover
- **Accessibility**: Touch-friendly approach can improve accessibility

### Customize Context Menu

```razor
@page "/custom-context-menu"
@rendermode InteractiveAuto

<SfBlockEditor Blocks="@blockData">
    <BlockEditorContextMenu Enable="true"
                            ShowItemOnClick="true"
                            Items="@contextMenuItems"
                            ItemSelect="@OnContextMenuSelect"
                            Opening="@OnContextMenuOpening"
                            Closing="@OnContextMenuClosing">
    </BlockEditorContextMenu>
</SfBlockEditor>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel { BlockType = BlockType.Paragraph }
    };

    private List<ContextMenuItemModel> contextMenuItems = new()
    {
        // Standard items
        new ContextMenuItemModel { ID = "cut", Text = "Cut", IconCss = "e-icons e-cut" },
        new ContextMenuItemModel { ID = "copy", Text = "Copy", IconCss = "e-icons e-copy" },
        new ContextMenuItemModel { ID = "paste", Text = "Paste", IconCss = "e-icons e-paste" },
        new ContextMenuItemModel { Separator = true },
        
        // Custom items
        new ContextMenuItemModel
        {
            ID = "format-submenu",
            Text = "Format",
            IconCss = "e-icons e-format-painter",
            Items = new()
            {
                new ContextMenuItemModel { ID = "bold", Text = "Bold", IconCss = "e-icons e-bold" },
                new ContextMenuItemModel { ID = "italic", Text = "Italic", IconCss = "e-icons e-italic" },
                new ContextMenuItemModel { ID = "underline", Text = "Underline", IconCss = "e-icons e-underline" }
            }
        },
        new ContextMenuItemModel { Separator = true },
        new ContextMenuItemModel
        {
            ID = "export",
            Text = "Export",
            IconCss = "e-icons e-export"
        },
        new ContextMenuItemModel
        {
            ID = "delete",
            Text = "Delete",
            IconCss = "e-icons e-delete"
        }
    };

    private void OnContextMenuSelect(ContextMenuItemSelectEventArgs args)
    {
        // Handle custom menu item selection
        if (args.Item.ID == "export")
        {
            // Export block logic
        }
        else if (args.Item.ID == "delete")
        {
            // Delete block logic
        }
    }

    private void OnContextMenuOpening(ContextMenuOpeningEventArgs args)
    {
        // Conditionally show/hide items based on context
    }

    private void OnContextMenuClosing(ContextMenuClosingEventArgs args)
    {
        // Cleanup when menu closes
    }
}
```

### Context Menu Events

```razor
<BlockEditorContextMenu Opening="@OnOpening" 
                        Closing="@OnClosing" 
                        ItemSelect="@OnSelect">
</BlockEditorContextMenu>

@code {
    private void OnOpening(ContextMenuOpeningEventArgs args)
    {
        // args.Target contains clicked element
        // Modify items before display
    }

    private void OnClosing(ContextMenuClosingEventArgs args)
    {
        // args.IsCanceled indicates if closed by user
    }

    private void OnSelect(ContextMenuItemSelectEventArgs args)
    {
        // args.Item contains selected item
        // args.ParentItem contains parent if submenu
    }
}
```

## Block Action Menu

Hover-based menu for block-level operations (drag handle menu).

### Built-In Block Action Items

| Item | Function |
|------|----------|
| Duplicate | Clone block |
| Delete | Remove block |
| Move Up | Move block up |
| Move Down | Move block down |

### Enable Block Action Menu

```razor
<SfBlockEditor>
    <BlockEditorActionMenu Enable="true"></BlockEditorActionMenu>
</SfBlockEditor>
```

### Customize Block Action Menu

```razor
@page "/custom-action-menu"
@rendermode InteractiveAuto

<SfBlockEditor Blocks="@blockData">
    <BlockEditorActionMenu Enable="true"
                           PopupWidth="200px"
                           PopupHeight="150px"
                           EnableTooltip="true"
                           Items="@actionMenuItems"
                           ItemSelect="@OnActionMenuSelect"
                           Opening="@OnActionMenuOpening"
                           Closing="@OnActionMenuClosing">
    </BlockEditorActionMenu>
</SfBlockEditor>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel { BlockType = BlockType.Paragraph, Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Block 1" } } },
        new BlockModel { BlockType = BlockType.Paragraph, Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Block 2" } } },
        new BlockModel { BlockType = BlockType.Paragraph, Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Block 3" } } }
    };

    private List<BlockActionItemModel> actionMenuItems = new()
    {
        new BlockActionItemModel
        {
            ID = "duplicate",
            Label = "Duplicate",
            IconCss = "e-icons e-duplicate",
            Tooltip = "Clone this block"
        },
        new BlockActionItemModel
        {
            ID = "move-up",
            Label = "Move Up",
            IconCss = "e-icons e-arrow-up",
            Tooltip = "Move up in document"
        },
        new BlockActionItemModel
        {
            ID = "move-down",
            Label = "Move Down",
            IconCss = "e-icons e-arrow-down",
            Tooltip = "Move down in document"
        },
        // Custom actions
        new BlockActionItemModel
        {
            ID = "highlight",
            Label = "Highlight",
            IconCss = "e-icons e-highlight",
            Tooltip = "Highlight this block"
        },
        new BlockActionItemModel
        {
            ID = "convert",
            Label = "Convert",
            IconCss = "e-icons e-convert",
            Tooltip = "Convert block type"
        },
        new BlockActionItemModel
        {
            ID = "delete",
            Label = "Delete",
            IconCss = "e-icons e-delete",
            Tooltip = "Remove this block"
        }
    };

    private void OnActionMenuSelect(ActionMenuItemSelectEventArgs args)
    {
        if (args.Item.ID == "highlight")
        {
            // Apply highlighting
        }
        else if (args.Item.ID == "convert")
        {
            // Show block type conversion dialog
        }
    }

    private void OnActionMenuOpening(ActionMenuOpeningEventArgs args)
    {
        // Conditionally show/hide items based on block type
        // args.Block contains current block
    }

    private void OnActionMenuClosing(ActionMenuClosingEventArgs args)
    {
        // Cleanup logic
    }
}
```

### Show/Hide Tooltips

```razor
<BlockEditorActionMenu EnableTooltip="true"></BlockEditorActionMenu>
<!-- or -->
<BlockEditorActionMenu EnableTooltip="false"></BlockEditorActionMenu>
```

## Inline Toolbar

Text selection toolbar for formatting options.

### Built-In Inline Toolbar Items

| Item | Function |
|------|----------|
| Bold | Bold formatting |
| Italic | Italic formatting |
| Underline | Underline text |
| Strikethrough | Strike through text |
| Superscript | Raise text |
| Subscript | Lower text |
| Case Conversion | Change text case |
| Text Color | Change text color |
| Background Color | Change background color |

### Enable Inline Toolbar

```razor
<SfBlockEditor>
    <BlockEditorInlineToolbar Enable="true"></BlockEditorInlineToolbar>
</SfBlockEditor>
```

### Customize Inline Toolbar

```razor
@page "/custom-inline-toolbar"
@rendermode InteractiveAuto

<SfBlockEditor Blocks="@blockData">
    <BlockEditorInlineToolbar Enable="true"
                              PopupWidth="120px"
                              Items="@toolbarItems"
                              ItemClick="@OnToolbarItemClick">
    </BlockEditorInlineToolbar>
</SfBlockEditor>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new()
            {
                new ContentModel { ContentType = ContentType.Text, Content = "Select text to see inline formatting toolbar" }
            }
        }
    };

    private List<InlineToolbarItemModel> toolbarItems = new()
    {
        new InlineToolbarItemModel { Command = CommandName.Bold },
        new InlineToolbarItemModel { Command = CommandName.Italic },
        new InlineToolbarItemModel { Command = CommandName.Underline },
        new InlineToolbarItemModel { Command = CommandName.Strikethrough }
    };

    private void OnToolbarItemClick(InlineToolbarItemClickEventArgs args)
    {
        // args.Item contains clicked toolbar item
        // args.Item.Command contains the command
    }
}
```

## Menu Event Patterns

### Pattern 1: Dynamic Menu Filtering

```razor
<BlockEditorCommandMenu Filtering="@OnFilter">
</BlockEditorCommandMenu>

@code {
    private void OnFilter(CommandFilteringEventArgs args)
    {
        // Show/hide commands based on search
        // Implement smart filtering for better UX
    }
}
```

### Pattern 2: Context-Aware Menu Items

```razor
<BlockEditorContextMenu Opening="@OnOpening">
</BlockEditorContextMenu>

@code {
    private void OnOpening(ContextMenuOpeningEventArgs args)
    {
        // Modify available items based on selected content
        // Enable/disable items based on block type or selection state
    }
}
```

### Pattern 3: Custom Action Execution

```razor
<BlockEditorActionMenu ItemSelect="@OnActionSelect">
</BlockEditorActionMenu>

@code {
    private void OnActionSelect(ActionMenuItemSelectEventArgs args)
    {
        // Execute custom logic based on selected action
        // Update UI, trigger events, modify content
    }
}
```

## Complete Menu Configuration Example

```razor
@page "/complete-menus"
@rendermode InteractiveAuto

<SfBlockEditor @bind-Blocks="blockData" EnableDragAndDrop="true">
    <!-- Slash Command Menu -->
    <BlockEditorCommandMenu PopupHeight="400px" 
                            PopupWidth="320px"
                            ItemSelect="@OnSlashSelect">
    </BlockEditorCommandMenu>

    <!-- Context Menu -->
    <BlockEditorContextMenu Enable="true"
                            ItemSelect="@OnContextSelect"
                            Opening="@OnContextOpening">
    </BlockEditorContextMenu>

    <!-- Block Action Menu -->
    <BlockEditorActionMenu Enable="true"
                           EnableTooltip="true"
                           ItemSelect="@OnActionSelect">
    </BlockEditorActionMenu>

    <!-- Inline Toolbar -->
    <BlockEditorInlineToolbar Enable="true"
                              PopupWidth="150px"
                              ItemClick="@OnToolbarClick">
    </BlockEditorInlineToolbar>
</SfBlockEditor>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel { BlockType = BlockType.Paragraph }
    };

    private void OnSlashSelect(CommandItemSelectEventArgs args) { }
    private void OnContextSelect(ContextMenuItemSelectEventArgs args) { }
    private void OnContextOpening(ContextMenuOpeningEventArgs args) { }
    private void OnActionSelect(ActionMenuItemSelectEventArgs args) { }
    private void OnToolbarClick(InlineToolbarItemClickEventArgs args) { }
}
```
