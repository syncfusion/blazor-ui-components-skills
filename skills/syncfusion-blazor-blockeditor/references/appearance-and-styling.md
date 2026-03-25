# Appearance and Styling in Syncfusion Blazor Block Editor

## Setting Width and Height

Control the editor's display dimensions using `Width` and `Height` properties. These can be specified in pixels, percentages, viewport units, or relative units.

### Examples of Size Configuration

**Percentage-based (responsive):**
```razor
<SfBlockEditor Width="100%" Height="80vh"></SfBlockEditor>
```

**Pixel-based (fixed):**
```razor
<SfBlockEditor Width="900px" Height="600px"></SfBlockEditor>
```

**Mixed units:**
```razor
<SfBlockEditor Width="100%" Height="400px"></SfBlockEditor>
```

**Container-relative:**
```razor
<div style="display: flex; height: 100vh;">
    <SfBlockEditor Width="100%" Height="100%"></SfBlockEditor>
</div>
```

### Responsive Design Pattern

Create an editor that adapts to screen size:

```razor
@page "/responsive-editor"
@rendermode InteractiveAuto

<div style="display: flex; flex-direction: column; height: 100vh; padding: 20px;">
    <header style="flex-shrink: 0; margin-bottom: 15px;">
        <h1>Responsive Document Editor</h1>
    </header>
    <main style="flex-grow: 1; overflow: hidden;">
        <SfBlockEditor @bind-Blocks="blockData"
                       Width="100%"
                       Height="100%"
                       EnableDragAndDrop="true">
        </SfBlockEditor>
    </main>
</div>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Responsive editor content..." } }
        }
    };
}
```

## Read-Only Mode

Use the `ReadOnly` property to make the editor display-only. This is ideal for document preview, approval workflows, or archived content viewing.

### Enable Read-Only Mode

```razor
<SfBlockEditor @bind-Blocks="blockData" ReadOnly="true"></SfBlockEditor>
```

When `ReadOnly="true"`:
- Users can view and select content
- Editing, pasting, and block manipulation are disabled
- Menus and toolbars are hidden
- Copy, cut operations work; paste operations are blocked

### Toggle Read-Only Dynamically

```razor
@page "/editor-with-toggle"
@rendermode InteractiveAuto

<div style="padding: 20px;">
    <div style="margin-bottom: 15px;">
        <label>
            <input type="checkbox" @onchange="@OnToggleReadOnly" checked="@isReadOnly" />
            Read-Only Mode
        </label>
    </div>

    <SfBlockEditor @bind-Blocks="blockData" ReadOnly="@isReadOnly"></SfBlockEditor>
</div>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 1 },
            Content = new()
            {
                new ContentModel { ContentType = ContentType.Text, Content = "Document Preview" }
            }
        },
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new()
            {
                new ContentModel { ContentType = ContentType.Text, Content = "Toggle read-only mode using the checkbox above." }
            }
        }
    };

    private bool isReadOnly = false;

    private void OnToggleReadOnly(ChangeEventArgs e)
    {
        isReadOnly = (bool)e.Value;
    }
}
```

### Read-Only Use Cases

| Use Case | Configuration |
|----------|---------------|
| Document Preview | `ReadOnly="true"` |
| Published Content | `ReadOnly="true"` |
| Approval Review | Dynamic toggle on approval |
| Draft vs. Final | Toggle based on status |
| Template Display | `ReadOnly="true"` with initial content |

## Custom CSS Classes

Apply custom styling using the `CssClass` property. This allows you to add one or more CSS classes for targeted styling.

### Single CSS Class

```razor
<SfBlockEditor @bind-Blocks="blockData" CssClass="dark-theme"></SfBlockEditor>

<style>
    .dark-theme {
        background-color: #1e1e1e;
        color: #ffffff;
    }

    .dark-theme .e-block {
        border-color: #333333;
    }

    .dark-theme .e-block-content {
        color: #e0e0e0;
    }
</style>
```

### Multiple CSS Classes

```razor
<SfBlockEditor @bind-Blocks="blockData" CssClass="modern-editor compact-view"></SfBlockEditor>

<style>
    .modern-editor {
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    .compact-view .e-block {
        margin-bottom: 8px;
        padding: 10px;
    }

    .compact-view .e-block-content {
        font-size: 14px;
        line-height: 1.5;
    }
</style>
```

## Theme Customization

### Available Built-In Themes

Include one theme CSS file in `App.razor`:

```html
<!-- Bootstrap 5 (Default) -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Material Design -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Material Dark -->
<link href="_content/Syncfusion.Blazor.Themes/material-dark.css" rel="stylesheet" />

<!-- Tailwind CSS -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />

<!-- Fluent Design -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />
```

### Custom Theme with CSS Variables

Override theme variables using CSS:

```razor
<style>
    :root {
        --e-block-editor-primary: #007bff;
        --e-block-editor-primary-light: #e7f1ff;
        --e-block-editor-border: #dee2e6;
        --e-block-editor-text: #212529;
        --e-block-editor-bg: #ffffff;
    }

    .custom-theme-editor {
        background-color: var(--e-block-editor-bg);
        color: var(--e-block-editor-text);
        border: 1px solid var(--e-block-editor-border);
    }

    .custom-theme-editor .e-block {
        background-color: var(--e-block-editor-bg);
        border-left: 3px solid var(--e-block-editor-primary);
        padding: 12px;
        margin-bottom: 8px;
    }

    .custom-theme-editor .e-block:hover {
        background-color: var(--e-block-editor-primary-light);
    }
</style>
```

## Complete Styling Example

```razor
@page "/styled-editor"
@rendermode InteractiveAuto

<div class="editor-wrapper">
    <header class="editor-header">
        <h1>Professional Document Editor</h1>
        <p>Create and edit structured content with full styling control</p>
    </header>

    <div class="editor-controls">
        <label>
            <input type="checkbox" @onchange="@OnToggleReadOnly" checked="@isReadOnly" />
            Preview Mode (Read-Only)
        </label>
        <button class="btn" @onclick="@OnChangeTheme">Change Theme</button>
    </div>

    <SfBlockEditor @bind-Blocks="blockData"
                   Width="100%"
                   Height="600px"
                   ReadOnly="@isReadOnly"
                   CssClass="@cssClasses"
                   EnableDragAndDrop="true">
    </SfBlockEditor>

    <footer class="editor-footer">
        <p>Block Count: @blockData.Count | Theme: @currentTheme</p>
    </footer>
</div>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 1 },
            Content = new()
            {
                new ContentModel { ContentType = ContentType.Text, Content = "Document Title" }
            }
        },
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new()
            {
                new ContentModel { ContentType = ContentType.Text, Content = "Professional styled content here..." }
            }
        }
    };

    private bool isReadOnly = false;
    private string currentTheme = "light";
    private string cssClasses = "light-theme professional-styling";

    private void OnToggleReadOnly(ChangeEventArgs e)
    {
        isReadOnly = (bool)e.Value;
    }

    private void OnChangeTheme()
    {
        currentTheme = currentTheme == "light" ? "dark" : "light";
        cssClasses = $"{currentTheme}-theme professional-styling";
    }
}

<style>
    .editor-wrapper {
        display: flex;
        flex-direction: column;
        height: 100vh;
        background-color: #f8f9fa;
    }

    .editor-header {
        padding: 20px;
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        text-align: center;
    }

    .editor-header h1 {
        margin: 0 0 8px 0;
        font-size: 28px;
    }

    .editor-header p {
        margin: 0;
        font-size: 14px;
        opacity: 0.9;
    }

    .editor-controls {
        padding: 15px 20px;
        background-color: white;
        border-bottom: 1px solid #dee2e6;
        display: flex;
        gap: 15px;
        align-items: center;
    }

    .editor-controls label {
        display: flex;
        align-items: center;
        gap: 8px;
        cursor: pointer;
    }

    .btn {
        padding: 8px 16px;
        background-color: #667eea;
        color: white;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        font-size: 14px;
    }

    .btn:hover {
        background-color: #764ba2;
    }

    .editor-footer {
        padding: 12px 20px;
        background-color: white;
        border-top: 1px solid #dee2e6;
        text-align: center;
        font-size: 12px;
        color: #6c757d;
    }

    /* Light Theme */
    .light-theme {
        background-color: #ffffff;
        color: #212529;
    }

    .light-theme .e-block {
        border: 1px solid #dee2e6;
        background-color: #ffffff;
    }

    /* Dark Theme */
    .dark-theme {
        background-color: #1e1e1e;
        color: #e0e0e0;
    }

    .dark-theme .e-block {
        border: 1px solid #333333;
        background-color: #2d2d2d;
    }

    .dark-theme .e-block-content {
        color: #e0e0e0;
    }

    /* Professional Styling */
    .professional-styling .e-block {
        border-radius: 6px;
        margin-bottom: 12px;
        padding: 12px;
        transition: all 0.2s ease;
    }

    .professional-styling .e-block:hover {
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    }

    .professional-styling .e-block-content {
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
        line-height: 1.6;
    }
</style>
```

## Responsive Grid Example

Build an editor that adapts to different screen sizes:

```razor
<style>
    @media (max-width: 768px) {
        .editor-wrapper {
            padding: 10px;
        }

        .editor-header {
            padding: 15px;
        }

        .editor-header h1 {
            font-size: 20px;
        }

        .editor-controls {
            flex-direction: column;
            align-items: flex-start;
        }

        .light-theme .e-block {
            padding: 8px;
            margin-bottom: 8px;
        }

        .light-theme .e-block-content {
            font-size: 14px;
        }
    }
</style>
```
