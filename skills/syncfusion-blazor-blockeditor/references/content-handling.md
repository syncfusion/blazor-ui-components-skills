# Content Handling in Syncfusion Blazor Block Editor

## Getting and Setting Block Content

### Access Blocks Programmatically

```razor
@rendermode InteractiveAuto

<div style="max-width: 900px; margin: 20px auto;">
    <div style="padding: 15px; background-color: #f5f5f5; margin-bottom: 15px;">
        <button @onclick="@GetContent">Get Content</button>
        <button @onclick="@SetContent">Set New Content</button>
        <button @onclick="@ClearContent">Clear All</button>
        
        <div style="margin-top: 10px;">
            <strong>Content:</strong>
            <pre style="background-color: white; padding: 10px; border-radius: 4px;">@displayContent</pre>
        </div>
    </div>

    <SfBlockEditor @bind-Blocks="blockData"></SfBlockEditor>
</div>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel
        {
            BlockType = BlockType.Heading,
            Properties = new HeadingBlockSettings { Level = 1 },
            Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Sample Content" } }
        }
    };

    private string displayContent = "";

    private void GetContent()
    {
        displayContent = string.Join("\n", blockData.Select((b, i) => $"Block {i}: {b.BlockType}"));
    }

    private void SetContent()
    {
        blockData = new()
        {
            new BlockModel
            {
                BlockType = BlockType.Heading,
                Properties = new HeadingBlockSettings { Level = 1 },
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "New Content Title" } }
            },
            new BlockModel
            {
                BlockType = BlockType.Paragraph,
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "This is new content..." } }
            }
        };
    }

    private void ClearContent()
    {
        blockData = new() { new BlockModel { BlockType = BlockType.Paragraph } };
    }
}
```

### Content Model Structure

```csharp
// Main block structure
public class BlockModel
{
    public BlockType BlockType { get; set; }
    public object Properties { get; set; } // HeadingBlockSettings, etc.
    public List<ContentModel> Content { get; set; }
    public string ID { get; set; }
}

// Content within a block
public class ContentModel
{
    public ContentType ContentType { get; set; } // Text, Link, Image, etc.
    public string Content { get; set; }
    public TextContentSettings Properties { get; set; }
}

// Text content with formatting
public class TextContentSettings
{
    public StyleModel Styles { get; set; } // Bold, Italic, Color, etc.
    public string Link { get; set; }
}
```

## Paste Cleanup Configuration

Control how content is handled when pasted into the editor.

### Configure Allowed Styles

Only specified CSS styles are preserved during paste:

```razor
<SfBlockEditor>
    <BlockEditorPasteCleanup AllowedStyles="@(new string[] 
    { 
        "font-weight", 
        "font-style", 
        "text-decoration" 
    })">
    </BlockEditorPasteCleanup>
</SfBlockEditor>
```

**Default Allowed Styles:**
- font-weight
- font-style
- text-decoration
- text-transform

### Set Denied Tags

Remove specific HTML tags from pasted content:

```razor
<SfBlockEditor>
    <BlockEditorPasteCleanup DeniedTags="@(new string[] 
    { 
        "script", 
        "iframe", 
        "object", 
        "embed" 
    })">
    </BlockEditorPasteCleanup>
</SfBlockEditor>
```

### Keep Format Configuration

Control whether formatting is preserved:

```razor
<!-- Keep formatting (default) -->
<BlockEditorPasteCleanup KeepFormat="true"></BlockEditorPasteCleanup>

<!-- Paste as plain text with no formatting -->
<BlockEditorPasteCleanup KeepFormat="false"></BlockEditorPasteCleanup>
```

### Plain Text Paste

Strip all HTML and styles, paste plain text only:

```razor
<SfBlockEditor>
    <BlockEditorPasteCleanup PlainText="true"></BlockEditorPasteCleanup>
</SfBlockEditor>
```

### Complete Paste Cleanup Example

```razor
@page "/paste-cleanup-demo"
@rendermode InteractiveAuto

<div style="max-width: 900px; margin: 20px auto;">
    <div style="padding: 15px; background-color: #f5f5f5; margin-bottom: 15px;">
        <h2>Paste Cleanup Demo</h2>
        <h3>Test Content:</h3>
        <div contenteditable="true" style="padding: 10px; border: 1px solid #ccc; background-color: white; margin-bottom: 10px;">
            <h2 style="color: red; font-weight: bold;">Formatted Heading</h2>
            <p style="background-color: yellow; font-style: italic;">
                This is a <span style="font-weight: bold;">bold paragraph</span> with 
                <span style="color: blue; font-style: italic;">italic text</span>
            </p>
            <script>console.log('This script will be removed');</script>
        </div>
        <p style="font-size: 12px; color: #666;">Copy from above and paste into the editor below</p>
    </div>

    <h3>Block Editor (with Paste Cleanup):</h3>
    <SfBlockEditor @bind-Blocks="blockData" 
                   Height="400px"
                   PasteCleanupCompleted="@OnPasteComplete">
        <BlockEditorPasteCleanup AllowedStyles="@(new string[] { "text-decoration" })"
                                 DeniedTags="@(new string[] { "script", "iframe" })">
        </BlockEditorPasteCleanup>
    </SfBlockEditor>

    <div style="margin-top: 15px; padding: 10px; background-color: #f0f7ff; border-radius: 4px;">
        <strong>Settings:</strong> Allow only text-decoration style, remove script/iframe tags
    </div>
</div>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel { BlockType = BlockType.Paragraph }
    };

    private void OnPasteComplete(PasteCleanupCompletedEventArgs args)
    {
        Console.WriteLine($"Paste completed. Content length: {args.Content?.Length}");
    }
}
```

## Paste Lifecycle Events

### PasteCleanupStarting Event

Triggers before content is pasted. Can inspect, modify, or cancel:

```razor
<SfBlockEditor PasteCleanupStarting="@OnPasteStarting">
</SfBlockEditor>

@code {
    private void OnPasteStarting(PasteCleanupStartingEventArgs args)
    {
        // args.Content contains pasted content
        // Set args.Cancel = true to prevent paste
        if (ContainsProhibitedContent(args.Content))
        {
            args.Cancel = true;
        }
    }

    private bool ContainsProhibitedContent(string content)
    {
        return content.Contains("<script>") || content.Contains("javascript:");
    }
}
```

### PasteCleanupCompleted Event

Triggers after content is successfully pasted:

```razor
<SfBlockEditor PasteCleanupCompleted="@OnPasteCompleted">
</SfBlockEditor>

@code {
    private void OnPasteCompleted(PasteCleanupCompletedEventArgs args)
    {
        // args.Content contains final pasted content
        // Perform post-paste operations
        LogPasteAction(args.Content);
    }

    private void LogPasteAction(string content)
    {
        Console.WriteLine($"Content pasted: {content.Length} characters");
    }
}
```

## Security Best Practices

### Prevent XSS (Cross-Site Scripting)

```razor
<SfBlockEditor>
    <!-- Block dangerous tags -->
    <BlockEditorPasteCleanup DeniedTags="@(new string[] 
    { 
        "script", 
        "iframe", 
        "object", 
        "embed",
        "style",
        "link"
    })">
    </BlockEditorPasteCleanup>
</SfBlockEditor>
```

### Sanitize HTML Content

```razor
@rendermode InteractiveAuto

<SfBlockEditor PasteCleanupStarting="@OnValidatePaste">
</SfBlockEditor>

@code {
    private void OnValidatePaste(PasteCleanupStartingEventArgs args)
    {
        // Validate and sanitize content before allowing paste
        if (!IsContentSafe(args.Content))
        {
            args.Cancel = true;
            Console.WriteLine("Paste blocked: Content contains potential security risks");
        }
    }

    private bool IsContentSafe(string content)
    {
        // Check for dangerous patterns
        var dangersPatterns = new[] 
        { 
            "javascript:", 
            "onerror=", 
            "onload=", 
            "<script",
            "eval(",
            "expression("
        };

        return !dangersPatterns.Any(p => content.Contains(p, StringComparison.OrdinalIgnoreCase));
    }
}
```

## Content Validation

### Validate on Block Change

```razor
@rendermode InteractiveAuto

<div style="max-width: 900px; margin: 20px auto;">
    <div style="padding: 15px; background-color: #f5f5f5; margin-bottom: 15px;">
        <h3>Validation Status: @validationStatus</h3>
        <div style="color: @(isValid ? "green" : "red");">
            @validationMessage
        </div>
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

    private string validationStatus = "Valid";
    private string validationMessage = "No errors";
    private bool isValid = true;

    private void OnBlockChanged(BlockChangedEventArgs args)
    {
        ValidateContent();
    }

    private void ValidateContent()
    {
        isValid = true;
        validationMessage = "";

        // Check block count
        if (blockData.Count > 100)
        {
            isValid = false;
            validationMessage += "Too many blocks. ";
        }

        // Check total content length
        int totalLength = blockData.Sum(b => b.Content?.Sum(c => c.Content?.Length ?? 0) ?? 0);
        if (totalLength > 50000)
        {
            isValid = false;
            validationMessage += "Content exceeds maximum length. ";
        }

        // Check for empty required content
        var hasContent = blockData.Any(b => b.Content?.Any(c => !string.IsNullOrEmpty(c.Content)) ?? false);
        if (!hasContent)
        {
            isValid = false;
            validationMessage = "Content cannot be empty.";
        }

        validationStatus = isValid ? "Valid ✓" : "Invalid ✗";
        if (isValid)
            validationMessage = "Content is valid and ready to save.";
    }
}
```

## Export Content

### Export as JSON

```razor
@rendermode InteractiveAuto

<button @onclick="@ExportAsJson">Export as JSON</button>

@code {
    private List<BlockModel> blockData = new();

    private void ExportAsJson()
    {
        var json = System.Text.Json.JsonSerializer.Serialize(blockData);
        Console.WriteLine(json);
        // Save to file or send to server
    }
}
```

### Export as HTML

```razor
@code {
    private string ExportAsHtml()
    {
        var html = new StringBuilder();
        html.AppendLine("<html><body>");

        foreach (var block in blockData)
        {
            html.Append(ConvertBlockToHtml(block));
        }

        html.AppendLine("</body></html>");
        return html.ToString();
    }

    private string ConvertBlockToHtml(BlockModel block)
    {
        return block.BlockType switch
        {
            BlockType.Heading => $"<h{GetHeadingLevel(block)}>{GetBlockText(block)}</h{GetHeadingLevel(block)}>",
            BlockType.Paragraph => $"<p>{GetBlockText(block)}</p>",
            BlockType.BulletList => ConvertListToHtml(block, "ul"),
            BlockType.NumberedList => ConvertListToHtml(block, "ol"),
            _ => GetBlockText(block)
        };
    }

    private string GetBlockText(BlockModel block)
    {
        return string.Concat(block.Content?.Select(c => c.Content) ?? Array.Empty<string>());
    }

    private int GetHeadingLevel(BlockModel block)
    {
        if (block.Properties is HeadingBlockSettings heading)
            return heading.Level;
        return 1;
    }

    private string ConvertListToHtml(BlockModel block, string listType)
    {
        var items = block.Content?.Select(c => $"<li>{c.Content}</li>") ?? Array.Empty<string>();
        return $"<{listType}>{string.Concat(items)}</{listType}>";
    }
}
```

## Import Content

### Load from JSON

```razor
@code {
    private async Task LoadFromJson(string json)
    {
        try
        {
            blockData = System.Text.Json.JsonSerializer.Deserialize<List<BlockModel>>(json);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error loading JSON: {ex.Message}");
        }
    }
}
```

### Load from HTML

```razor
@code {
    private List<BlockModel> ConvertHtmlToBlocks(string html)
    {
        var blocks = new List<BlockModel>();
        
        // Parse HTML and convert to blocks
        // This is simplified; real implementation would use HTML parser
        
        if (html.Contains("<h1>"))
        {
            blocks.Add(new BlockModel
            {
                BlockType = BlockType.Heading,
                Properties = new HeadingBlockSettings { Level = 1 },
                Content = new() { new ContentModel { ContentType = ContentType.Text, Content = "Heading" } }
            });
        }

        return blocks;
    }
}
```

## Complete Content Handling Example

```razor
@page "/content-handling-complete"
@rendermode InteractiveAuto

<div style="max-width: 1000px; margin: 20px auto;">
    <div style="display: flex; gap: 20px;">
        <!-- Left panel: Content management -->
        <div style="flex: 0 0 200px; padding: 15px; background-color: #f5f5f5; border-radius: 4px;">
            <h3>Content Management</h3>
            <button @onclick="@GetBlockData" style="display: block; width: 100%; padding: 8px; margin-bottom: 8px;">Get Data</button>
            <button @onclick="@ExportJson" style="display: block; width: 100%; padding: 8px; margin-bottom: 8px;">Export JSON</button>
            <button @onclick="@ClearAll" style="display: block; width: 100%; padding: 8px;">Clear</button>

            <div style="margin-top: 15px;">
                <strong>Status:</strong> @blockData.Count blocks
            </div>
        </div>

        <!-- Right panel: Editor -->
        <div style="flex: 1;">
            <SfBlockEditor @bind-Blocks="blockData" 
                           Height="500px"
                           BlockChanged="@OnBlockChanged"
                           PasteCleanupStarting="@OnPasteStarting">
                <BlockEditorPasteCleanup AllowedStyles="@(new string[] { "font-weight", "font-style" })"
                                         DeniedTags="@(new string[] { "script", "iframe" })">
                </BlockEditorPasteCleanup>
            </SfBlockEditor>
        </div>
    </div>

    <!-- Output display -->
    <div style="margin-top: 20px; padding: 15px; background-color: #f0f7ff; border-radius: 4px;">
        <strong>Last Action:</strong> @lastAction
    </div>
</div>

@code {
    private List<BlockModel> blockData = new()
    {
        new BlockModel { BlockType = BlockType.Paragraph }
    };

    private string lastAction = "Ready";

    private void GetBlockData()
    {
        lastAction = $"Retrieved {blockData.Count} blocks";
    }

    private void ExportJson()
    {
        var json = System.Text.Json.JsonSerializer.Serialize(blockData, new System.Text.Json.JsonSerializerOptions { WriteIndented = true });
        lastAction = "Exported as JSON";
        Console.WriteLine(json);
    }

    private void ClearAll()
    {
        blockData = new() { new BlockModel { BlockType = BlockType.Paragraph } };
        lastAction = "Content cleared";
    }

    private void OnBlockChanged(BlockChangedEventArgs args)
    {
        lastAction = $"Content updated at {DateTime.Now:HH:mm:ss}";
    }

    private void OnPasteStarting(PasteCleanupStartingEventArgs args)
    {
        if (args.Content?.Contains("<script>") ?? false)
        {
            args.Cancel = true;
            lastAction = "Paste blocked: Script tags detected";
        }
    }
}
```
