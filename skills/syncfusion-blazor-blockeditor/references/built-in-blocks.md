# Built-In Blocks in Syncfusion Blazor Block Editor

## Overview of Block Types

The Block Editor provides a comprehensive set of block types for creating structured, modular content. Each block type serves a specific purpose and can be combined to build complex documents.

### Available Block Types

| Block Type | Purpose | Properties | Example Use |
|-----------|---------|-----------|-------------|
| Paragraph | Standard text content | None | Regular body text |
| Heading | Section/subsection titles | Level (1-4) | Document structure |
| BulletList | Unordered items | None | Lists, bullet points |
| NumberedList | Ordered items | None | Numbered steps, sequences |
| Checklist | Checkable items | None | Tasks, to-do lists |
| Quote | Citation/emphasis blocks | None | Testimonials, quotes |
| CodeBlock | Formatted code | Language | Code samples |
| Table | Data in rows/columns | Rows, Columns | Data presentation |
| Image | Media content | Source URL, Alt text | Illustrations, photos |
| Divider | Horizontal separator | None | Visual separation |
| Callout | Highlighted information | Type (info, warning, success, error) | Important notices |
| Toggle | Collapsible content | None | Expandable sections |

## Heading Blocks

Create document structure with hierarchical headings.

### Heading Levels

```razor
@rendermode InteractiveAuto

<SfBlockEditor @bind-Blocks="headingData"></SfBlockEditor>

@code {
    private List<BlockModel> headingData = new()
    {
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 1 },
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Heading Level 1" } }
        },
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 2 },
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Heading Level 2" } }
        },
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 3 },
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Heading Level 3" } }
        },
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 4 },
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Heading Level 4" } }
        }
    };
}
```

## Paragraph Blocks

Basic text content blocks for body text and descriptions.

```razor
new BlockModel
{
    BlockType = BlockType.Paragraph,
    Content = new()
    {
        new ContentModel { ContentType = ContentType.Text, Content = "This is a paragraph with basic text content." },
        new ContentModel { ContentType = ContentType.Text, Content = " Additional text in same paragraph." }
    }
}
```

## List Types

### Bullet Lists (Unordered)

```razor
new BlockModel
{
    BlockType = BlockType.BulletList,
    Content = new()
    {
        new ContentModel { ContentType = ContentType.Text, Content = "First bullet point" },
        new ContentModel { ContentType = ContentType.Text, Content = "Second bullet point" },
        new ContentModel { ContentType = ContentType.Text, Content = "Third bullet point" }
    }
}
```

### Numbered Lists (Ordered)

```razor
new BlockModel
{
    BlockType = BlockType.NumberedList,
    Content = new()
    {
        new ContentModel { ContentType = ContentType.Text, Content = "First step" },
        new ContentModel { ContentType = ContentType.Text, Content = "Second step" },
        new ContentModel { ContentType = ContentType.Text, Content = "Third step" }
    }
}
```

### Checklist

Interactive checkable items:

```razor
new BlockModel
{
    BlockType = BlockType.Checklist,
    Content = new()
    {
        new ContentModel { ContentType = ContentType.Text, Content = "Task one" },
        new ContentModel { ContentType = ContentType.Text, Content = "Task two" },
        new ContentModel { ContentType = ContentType.Text, Content = "Task three" }
    }
}
```

## Quote Blocks

Styled blocks for citations and emphasizing important text:

```razor
new BlockModel
{
    BlockType = BlockType.Quote,
    Content = new()
    {
        new ContentModel { ContentType = ContentType.Text, Content = "\"The only way to do great work is to love what you do.\" - Steve Jobs" }
    }
}
```

## Code Blocks

Display formatted code with syntax highlighting:

```razor
new BlockModel
{
    BlockType = BlockType.CodeBlock,
    Properties = new CodeBlockSettings { Language = "csharp" },
    Content = new()
    {
        new ContentModel
        {
            ContentType = ContentType.Text,
            Content = "public class Example {\n  public static void Main() {\n    Console.WriteLine(\"Hello, World!\");\n  }\n}"
        }
    }
}
```

### Supported Languages

- csharp
- javascript
- typescript
- python
- java
- html
- css
- sql
- json
- xml
- bash
- etc.

## Table Blocks

Display tabular data:

```razor
new BlockModel
{
    BlockType = BlockType.Table,
    Properties = new TableBlockSettings 
    { 
        Rows = 3, 
        Columns = 3,
        HeaderRow = true
    },
    Content = new()
    {
        // Row 1: Headers
        new ContentModel { ContentType = ContentType.Text, Content = "Name" },
        new ContentModel { ContentType = ContentType.Text, Content = "Email" },
        new ContentModel { ContentType = ContentType.Text, Content = "Status" },
        // Row 2: Data
        new ContentModel { ContentType = ContentType.Text, Content = "John Doe" },
        new ContentModel { ContentType = ContentType.Text, Content = "john@example.com" },
        new ContentModel { ContentType = ContentType.Text, Content = "Active" },
        // Row 3: Data
        new ContentModel { ContentType = ContentType.Text, Content = "Jane Smith" },
        new ContentModel { ContentType = ContentType.Text, Content = "jane@example.com" },
        new ContentModel { ContentType = ContentType.Text, Content = "Active" }
    }
}
```

## Image Blocks

Embed media content:

```razor
new BlockModel
{
    BlockType = BlockType.Image,
    Properties = new ImageBlockSettings 
    { 
        Src = "https://example.com/image.jpg",
        Alt = "Example Image",
        Caption = "Image caption text"
    }
}
```

### Image Sizing Configuration

Control image dimensions and resizing behavior with advanced sizing properties:

```razor
new BlockModel
{
    BlockType = BlockType.Image,
    Properties = new ImageBlockSettings 
    { 
        Src = "https://example.com/image.jpg",
        Alt = "Example Image",
        Caption = "Image caption text",
        EnableResize = true,           // Allow users to resize images
        Width = "100%",                // Display width (pixels, percent, auto)
        Height = "auto",               // Display height (pixels, percent, auto)
        MinWidth = "200px",            // Minimum width constraint
        MaxWidth = "800px",            // Maximum width constraint
        MinHeight = "150px",           // Minimum height constraint
        MaxHeight = "600px"            // Maximum height constraint
    }
}
```

#### Image Sizing Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `EnableResize` | bool | false | Enable/disable user image resizing |
| `Width` | string | "auto" | Display width (px, %, auto, inherit) |
| `Height` | string | "auto" | Display height (px, %, auto, inherit) |
| `MinWidth` | string | null | Minimum width when resizing |
| `MaxWidth` | string | null | Maximum width when resizing |
| `MinHeight` | string | null | Minimum height when resizing |
| `MaxHeight` | string | null | Maximum height when resizing |

#### Sizing Examples

**Responsive Full-Width Image:**
```razor
new BlockModel
{
    BlockType = BlockType.Image,
    Properties = new ImageBlockSettings 
    { 
        Src = "https://example.com/banner.jpg",
        Width = "100%",
        Height = "auto",           // Maintains aspect ratio
        MaxWidth = "1200px"        // Limit maximum width
    }
}
```

**Fixed Size Image:**
```razor
new BlockModel
{
    BlockType = BlockType.Image,
    Properties = new ImageBlockSettings 
    { 
        Src = "https://example.com/thumbnail.jpg",
        Width = "300px",
        Height = "300px",
        EnableResize = false       // Prevent resizing
    }
}
```

**Resizable Image with Constraints:**
```razor
new BlockModel
{
    BlockType = BlockType.Image,
    Properties = new ImageBlockSettings 
    { 
        Src = "https://example.com/photo.jpg",
        Width = "500px",
        Height = "auto",
        EnableResize = true,
        MinWidth = "200px",
        MaxWidth = "800px",
        MinHeight = "150px",
        MaxHeight = "600px"
    }
}
```

#### Best Practices for Image Sizing

1. **Maintain Aspect Ratio**: Set height to "auto" to preserve image proportions
2. **Responsive Design**: Use percentage width with maximum width constraints for responsive layouts
3. **Performance**: Limit maximum dimensions to prevent memory issues
4. **User Control**: Use `EnableResize = true` for user-friendly interfaces
5. **Constraints**: Always set both Min and Max constraints when enabling resize
6. **Mobile Optimization**: Use smaller max dimensions for mobile-friendly layouts
7. **Accessibility**: Always provide descriptive `Alt` text for accessibility

#### Complete Image Block Example

```razor
@page "/image-sizing"
@rendermode InteractiveAuto
@using Syncfusion.Blazor.BlockEditor

<SfBlockEditor @bind-Blocks="imageData"></SfBlockEditor>

@code {
    private List<BlockModel> imageData = new()
    {
        // Heading
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 1 },
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Image Gallery" } }
        },

        // Responsive banner
        new BlockModel
        {
            BlockType = BlockType.Image,
            Properties = new ImageBlockSettings 
            { 
                Src = "https://example.com/banner.jpg",
                Alt = "Page Banner",
                Caption = "Responsive banner - scales with screen",
                Width = "100%",
                Height = "auto",
                MaxWidth = "1200px",
                EnableResize = false
            }
        },

        // Resizable thumbnail
        new BlockModel
        {
            BlockType = BlockType.Image,
            Properties = new ImageBlockSettings 
            { 
                Src = "https://example.com/thumbnail.jpg",
                Alt = "Resizable Image",
                Caption = "Drag edges to resize (200-800px)",
                Width = "400px",
                Height = "auto",
                MinWidth = "200px",
                MaxWidth = "800px",
                EnableResize = true
            }
        },

        // Gallery images
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Gallery:" } }
        }
    };
}
```

## Divider Blocks

Add horizontal separators for visual organization:

```razor
new BlockModel
{
    BlockType = BlockType.Divider
}
```

## Callout Blocks

Highlighted information blocks with different types:

```razor
new BlockModel
{
    BlockType = BlockType.Callout,
    Properties = new CalloutBlockSettings 
    { 
        CalloutType = "info" // info, warning, success, error
    },
    Content = new()
    {
        new ContentModel { ContentType = ContentType.Text, Content = "This is an important notification." }
    }
}
```

### Callout Types

- `info`: Blue informational callout
- `warning`: Yellow warning callout
- `success`: Green success callout
- `error`: Red error callout

## Toggle (Collapsible) Blocks

Create expandable content sections:

```razor
new BlockModel
{
    BlockType = BlockType.Toggle,
    Properties = new ToggleBlockSettings 
    { 
        Title = "Click to expand"
    },
    Content = new()
    {
        new ContentModel { ContentType = ContentType.Text, Content = "Hidden content that expands on click" }
    }
}
```

## Block Nesting and Hierarchy

Blocks can be nested to create hierarchical structures:

```razor
@rendermode InteractiveAuto

<SfBlockEditor @bind-Blocks="nestedData"></SfBlockEditor>

@code {
    private List<BlockModel> nestedData = new()
    {
        // Parent heading
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 1 },
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Main Section" } }
        },
        // Nested content under heading
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Introduction paragraph" } }
        },
        new BlockModel
        {
            BlockType = BlockType.BulletList,
            Content = new()
            {
                new ContentModel { ContentType = ContentType.Text, Content = "Point 1" },
                new ContentModel { ContentType = ContentType.Text, Content = "Point 2" }
            }
        },
        // Subsection
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 2 },
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Subsection" } }
        },
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Subsection content" } }
        }
    };
}
```

## Complete Document Structure Example

```razor
@page "/document-structure"
@rendermode InteractiveAuto

<SfBlockEditor @bind-Blocks="documentBlocks" EnableDragAndDrop="true"></SfBlockEditor>

@code {
    private List<BlockModel> documentBlocks = new()
    {
        // Title
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 1 },
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Document Title" } }
        },
        
        // Introduction
        new BlockModel
        {
            BlockType = BlockType.Paragraph,
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "This document demonstrates all block types." } }
        },
        
        // Info callout
        new BlockModel
        {
            BlockType = BlockType.Callout,
            Properties = new CalloutBlockSettings { CalloutType = "info" },
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Key information for readers" } }
        },
        
        // Section with list
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 2 },
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Features" } }
        },
        new BlockModel
        {
            BlockType = BlockType.BulletList,
            Content = new()
            {
                new ContentModel { ContentType = ContentType.Text, Content = "Feature one" },
                new ContentModel { ContentType = ContentType.Text, Content = "Feature two" }
            }
        },
        
        // Steps section
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 2 },
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Steps" } }
        },
        new BlockModel
        {
            BlockType = BlockType.NumberedList,
            Content = new()
            {
                new ContentModel { ContentType = ContentType.Text, Content = "First step" },
                new ContentModel { ContentType = ContentType.Text, Content = "Second step" }
            }
        },
        
        // Divider
        new BlockModel { BlockType = BlockType.Divider },
        
        // Quote
        new BlockModel
        {
            BlockType = BlockType.Quote,
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "\"Success is the sum of small efforts repeated day in and day out.\"" } }
        },
        
        // Toggle for additional info
        new BlockModel
        {
            BlockType = BlockType.Toggle,
            Properties = new ToggleBlockSettings { Title = "Advanced Options" },
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Additional detailed information here" } }
        },
        
        // Empty paragraph for user input
        new BlockModel { BlockType = BlockType.Paragraph }
    };
}
```

## Custom Block Properties

Extend blocks with additional properties:

```razor
// Block with custom formatting
new BlockModel
{
    BlockType = BlockType.Paragraph,
    Properties = new ParagraphBlockSettings 
    { 
        Alignment = "center",
        BackgroundColor = "#f0f0f0"
    },
    Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Centered paragraph with background" } }
}
```

## Block Content Model

Each block contains a list of `ContentModel` items:

```csharp
public class ContentModel
{
    public ContentType ContentType { get; set; } // Text, Link, Image, etc.
    public string Content { get; set; }
    public TextContentSettings Properties { get; set; } // Bold, Italic, Color, etc.
}
```

### Text Content with Styling

```razor
new ContentModel
{
    ContentType = ContentType.Text,
    Content = "Bold and italic text",
    Properties = new TextContentSettings
    {
        Styles = new StyleModel
        {
            Bold = true,
            Italic = true,
            TextColor = "#ff0000"
        }
    }
}
```

## Best Practices

1. **Hierarchy**: Use appropriate heading levels (H1 → H2 → H3)
2. **Lists**: Use bullet/numbered lists for grouped items
3. **Tables**: Use tables for structured data presentation
4. **Visual Separation**: Use dividers between major sections
5. **Callouts**: Reserve for important information
6. **Toggle Blocks**: Use for optional/advanced content
7. **Empty Blocks**: Include at least one empty block for user input

---

## Enums Reference

### BlockType Enum

Represents the type of block in the editor. Each block type defines specific rendering and interaction behavior.

```csharp
public enum BlockType
{
    /// <summary>Standard text content block</summary>
    Paragraph,

    /// <summary>Section/subsection heading (use with HeadingBlockSettings Level)</summary>
    Heading,

    /// <summary>Unordered list items</summary>
    BulletList,

    /// <summary>Ordered/numbered list items</summary>
    NumberedList,

    /// <summary>Checklist with checkable items</summary>
    Checklist,

    /// <summary>Citation or quoted text block</summary>
    Quote,

    /// <summary>Code with syntax highlighting support</summary>
    CodeBlock,

    /// <summary>Tabular data with rows and columns</summary>
    Table,

    /// <summary>Image or media content</summary>
    Image,

    /// <summary>Horizontal visual separator</summary>
    Divider,

    /// <summary>Highlighted information box (info, warning, success, error)</summary>
    Callout,

    /// <summary>Expandable/collapsible content section</summary>
    Toggle
}
```

### ContentType Enum

Represents the type of content within a block.

```csharp
public enum ContentType
{
    /// <summary>Plain text content</summary>
    Text,

    /// <summary>Hyperlink content</summary>
    Link,

    /// <summary>User mention content (e.g., @user)</summary>
    Mention,

    /// <summary>Label or tag content (e.g., #label)</summary>
    Label
}
```

### SaveFormat Enum

Represents the format for saving image content.

```csharp
public enum SaveFormat
{
    /// <summary>Save as Blob reference (default)</summary>
    Blob,

    /// <summary>Save as Base64-encoded string</summary>
    Base64
}
```

### BlockAction Enum

Represents actions available for block operations.

```csharp
public enum BlockAction
{
    /// <summary>Clone/duplicate a block</summary>
    Duplicate,

    /// <summary>Remove/delete a block</summary>
    Delete,

    /// <summary>Move a block up one position</summary>
    MoveUp,

    /// <summary>Move a block down one position</summary>
    MoveDown
}
```

### TableColumnType Enum

Represents the type of column in a table block.

```csharp
public enum TableColumnType
{
    /// <summary>Text column - accepts any text input</summary>
    Text,

    /// <summary>Number column - numeric input only</summary>
    Number,

    /// <summary>Checkbox column - boolean values</summary>
    Checkbox,

    /// <summary>Dropdown column - predefined value selection</summary>
    Dropdown
}
```

#### Enum Usage Examples

**Using BlockType:**
```csharp
new BlockModel
{
    BlockType = BlockType.Heading,  // Use enum value
    Properties = new HeadingBlockSettings { Level = 1 }
}

// Check block type
if (block.BlockType == BlockType.CodeBlock)
{
    // Handle code block
}
```

**Using ContentType:**
```csharp
new ContentModel
{
    ContentType = ContentType.Text,  // Plain text
    Content = "Hello world"
}

new ContentModel
{
    ContentType = ContentType.Link,  // Hyperlink
    Content = "Click here",
    Properties = new LinkContentSettings { Url = "https://example.com" }
}
```

**Using TableColumnType:**
```csharp
// In table configuration
var tableSettings = new TableBlockSettings
{
    Rows = 3,
    Columns = 3,
    ColumnTypes = new() 
    { 
        TableColumnType.Text,      // First column: text
        TableColumnType.Number,    // Second column: numbers
        TableColumnType.Checkbox   // Third column: checkboxes
    }
};
```

---

**Last Updated:** March 24, 2026  
**Component Version:** 1.0.0+
