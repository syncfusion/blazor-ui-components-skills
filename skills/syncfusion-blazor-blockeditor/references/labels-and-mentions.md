# Labels and Mentions in Syncfusion Blazor Block Editor

Enable dynamic user mentions and label/tag functionality in your Block Editor. This guide covers configuring mention systems, managing user models, and implementing label features for rich content tagging and collaboration.

---

## Overview

| Feature | Component | Trigger | Purpose |
|---------|-----------|---------|---------|
| **Mentions** | BlockEditorLabel | `@` | Reference users in content |
| **Labels/Tags** | BlockEditorLabel | `#` | Tag and categorize content |
| **Users** | UserModel | - | Define available users |

---

## UserModel

Represents a user that can be mentioned in the editor. Users are displayed in mention popups and can be referenced with the `@` mention syntax.

### Properties

```csharp
public class UserModel
{
    /// <summary>
    /// Unique identifier for the user
    /// </summary>
    public string Id { get; set; }

    /// <summary>
    /// Display name of the user (shown in mention popup)
    /// </summary>
    public string Name { get; set; }

    /// <summary>
    /// Optional avatar URL (displayed in mention popup)
    /// </summary>
    public string Avatar { get; set; }
}
```

### Properties Explained

| Property | Type | Required | Example |
|----------|------|----------|---------|
| `Id` | string | ✅ | `"user-001"`, `"john-doe"` |
| `Name` | string | ✅ | `"John Doe"`, `"Jane Smith"` |
| `Avatar` | string | ❌ | `"https://example.com/avatar.jpg"` |

### Example Usage

```csharp
private List<UserModel> users = new()
{
    new UserModel 
    { 
        Id = "user-001", 
        Name = "John Doe", 
        Avatar = "https://example.com/avatars/john.jpg" 
    },
    new UserModel 
    { 
        Id = "user-002", 
        Name = "Jane Smith", 
        Avatar = "https://example.com/avatars/jane.jpg" 
    },
    new UserModel 
    { 
        Id = "user-003", 
        Name = "Bob Johnson", 
        Avatar = "https://example.com/avatars/bob.jpg" 
    }
};
```

---

## LabelItemModel

Represents a label or tag that can be applied to content. Labels are displayed in a label popup and can be inserted with the `#` trigger character.

### Properties

```csharp
public class LabelItemModel
{
    /// <summary>
    /// Unique identifier for the label
    /// </summary>
    public string Id { get; set; }

    /// <summary>
    /// Display name of the label (shown in label popup)
    /// </summary>
    public string Name { get; set; }

    /// <summary>
    /// Color associated with the label (hex format)
    /// </summary>
    public string Color { get; set; }
}
```

### Properties Explained

| Property | Type | Required | Example |
|----------|------|----------|---------|
| `Id` | string | ✅ | `"label-001"`, `"feature"` |
| `Name` | string | ✅ | `"Important"`, `"Feature"`, `"Bug"` |
| `Color` | string | ✅ | `"#FF6B6B"`, `"#4ECDC4"` |

### Example Usage

```csharp
private List<LabelItemModel> labels = new()
{
    new LabelItemModel { Id = "label-001", Name = "Important", Color = "#FF6B6B" },
    new LabelItemModel { Id = "label-002", Name = "Feature", Color = "#4ECDC4" },
    new LabelItemModel { Id = "label-003", Name = "Bug", Color = "#FFD93D" },
    new LabelItemModel { Id = "label-004", Name = "Documentation", Color = "#6BCB77" },
    new LabelItemModel { Id = "label-005", Name = "Review", Color = "#A29BFE" }
};
```

---

## BlockEditorLabel Component

Configure the label/mention popup behavior and content. This component controls how mentions and labels are triggered and displayed.

### Properties

```csharp
public class BlockEditorLabel
{
    /// <summary>
    /// Character that triggers the label/mention popup (default: '#')
    /// </summary>
    public char TriggerChar { get; set; } = '#';

    /// <summary>
    /// List of available labels/items to display in popup
    /// </summary>
    public List<LabelItemModel> Items { get; set; }
}
```

### Properties Explained

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `TriggerChar` | char | `'#'` | Character that activates the popup (use `'@'` for mentions) |
| `Items` | List<LabelItemModel> | null | Available labels to display in popup |

### Basic Configuration

```razor
<SfBlockEditor @bind-Blocks="blockData" Users="@users">
    <!-- Label popup with # trigger -->
    <BlockEditorLabel TriggerChar="#" Items="@labels">
    </BlockEditorLabel>
</SfBlockEditor>
```

---

## MentionContentSettings

Represents a content item that mentions a user. When a user is mentioned using `@`, this settings class stores the mention metadata.

### Properties

```csharp
public class MentionContentSettings : ContentSettings
{
    /// <summary>
    /// ID of the mentioned user
    /// </summary>
    public string UserId { get; set; }
}
```

### Properties Explained

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `UserId` | string | - | References a user ID from UserModel |

### Example Usage

```csharp
new ContentModel
{
    ContentType = ContentType.Mention,
    Content = "@John Doe",
    Properties = new MentionContentSettings
    {
        UserId = "user-001"
    }
}
```

---

## LabelContentSettings

Represents a content item that contains a label or tag. When a label is applied using `#`, this settings class stores the label metadata.

### Properties

```csharp
public class LabelContentSettings : ContentSettings
{
    /// <summary>
    /// ID of the applied label
    /// </summary>
    public string LabelId { get; set; }
}
```

### Properties Explained

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `LabelId` | string | - | References a label ID from LabelItemModel |

### Example Usage

```csharp
new ContentModel
{
    ContentType = ContentType.Label,
    Content = "#Feature",
    Properties = new LabelContentSettings
    {
        LabelId = "label-002"
    }
}
```

---

## Implementing Mentions

Enable user mentions with the `@` character. This allows users to reference other team members in content.

### Setup Mentions

```razor
@page "/mentions"
@rendermode InteractiveAuto
@using Syncfusion.Blazor.BlockEditor

<div style="margin-bottom: 20px; padding: 15px; background-color: #e3f2fd; border-radius: 4px;">
    <strong>💡 Tip:</strong> Type @ followed by a letter to mention a user
</div>

<div style="height: 500px; border: 1px solid #ddd; border-radius: 4px;">
    <SfBlockEditor @bind-Blocks="blockData" Users="@users">
        <!-- Configure @ as mention trigger -->
        <BlockEditorLabel TriggerChar="@" Items="@null">
        </BlockEditorLabel>
    </SfBlockEditor>
</div>

@code {
    private List<BlockModel> blockData = new();
    
    private List<UserModel> users = new()
    {
        new UserModel { Id = "u1", Name = "Alice Johnson", Avatar = "https://example.com/alice.jpg" },
        new UserModel { Id = "u2", Name = "Bob Smith", Avatar = "https://example.com/bob.jpg" },
        new UserModel { Id = "u3", Name = "Charlie Brown", Avatar = "https://example.com/charlie.jpg" },
        new UserModel { Id = "u4", Name = "Diana Prince", Avatar = "https://example.com/diana.jpg" }
    };

    protected override void OnInitialized()
    {
        blockData = new()
        {
            new BlockModel
            {
                BlockType = BlockType.Paragraph,
                Content = new()
                {
                    new ContentModel { ContentType = ContentType.Text, Content = "Hey " },
                    new ContentModel 
                    { 
                        ContentType = ContentType.Mention, 
                        Content = "@Alice", 
                        Properties = new MentionContentSettings { UserId = "u1" } 
                    },
                    new ContentModel { ContentType = ContentType.Text, Content = ", can you review this?" }
                }
            }
        };
    }
}
```

### Mention Features

| Feature | Behavior | Example |
|---------|----------|---------|
| **Auto-complete** | Filter users as you type | Type `@al` → shows Alice |
| **Avatar Display** | Show user profile picture | Avatar appears in popup |
| **Link to Profile** | Mentioned user can be linked | Click mention to view profile |
| **Notifications** | Notify mentioned users | User receives notification |

---

## Implementing Labels/Tags

Enable content labeling with the `#` character. Use labels to organize, categorize, and tag content.

### Setup Labels

```razor
@page "/labels"
@rendermode InteractiveAuto
@using Syncfusion.Blazor.BlockEditor

<div style="margin-bottom: 20px; padding: 15px; background-color: #f3e5f5; border-radius: 4px;">
    <strong>💡 Tip:</strong> Type # followed by a letter to add a label/tag
</div>

<div style="height: 500px; border: 1px solid #ddd; border-radius: 4px;">
    <SfBlockEditor @bind-Blocks="blockData">
        <!-- Configure # as label trigger -->
        <BlockEditorLabel TriggerChar="#" Items="@labels">
        </BlockEditorLabel>
    </SfBlockEditor>
</div>

@code {
    private List<BlockModel> blockData = new();
    
    private List<LabelItemModel> labels = new()
    {
        new LabelItemModel { Id = "l1", Name = "Important", Color = "#FF6B6B" },
        new LabelItemModel { Id = "l2", Name = "Feature", Color = "#4ECDC4" },
        new LabelItemModel { Id = "l3", Name = "Bug", Color = "#FFD93D" },
        new LabelItemModel { Id = "l4", Name = "Documentation", Color = "#6BCB77" },
        new LabelItemModel { Id = "l5", Name = "Review", Color = "#A29BFE" }
    };

    protected override void OnInitialized()
    {
        blockData = new()
        {
            new BlockModel
            {
                BlockType = BlockType.Paragraph,
                Content = new()
                {
                    new ContentModel { ContentType = ContentType.Text, Content = "Update user profile " },
                    new ContentModel 
                    { 
                        ContentType = ContentType.Label, 
                        Content = "#Feature", 
                        Properties = new LabelContentSettings { LabelId = "l2" } 
                    }
                }
            }
        };
    }
}
```

### Label Features

| Feature | Use Case | Example |
|---------|----------|---------|
| **Color Coding** | Visual categorization | Feature=Blue, Bug=Yellow |
| **Filtering** | Find content by label | Show all #Important items |
| **Organization** | Group related content | All #Documentation blocks |
| **Workflow Tracking** | Status indicators | #InReview, #Approved |

---

## Combined Mentions and Labels Example

```razor
@page "/mentions-labels"
@rendermode InteractiveAuto
@using Syncfusion.Blazor.BlockEditor

<div class="card" style="margin-bottom: 20px;">
    <div class="card-header">
        <h5>Collaborative Editor with Mentions & Labels</h5>
    </div>
    <div class="card-body">
        <div style="margin-bottom: 10px; color: #666;">
            <small>
                💡 Type <strong>@</strong> to mention users<br/>
                💡 Type <strong>#</strong> to add labels
            </small>
        </div>
    </div>
</div>

<div style="height: 500px; border: 1px solid #ddd; border-radius: 4px; overflow: hidden;">
    <SfBlockEditor @bind-Blocks="blockData" Users="@users">
        <!-- Mention configuration -->
        <BlockEditorLabel TriggerChar="@" Items="@null">
        </BlockEditorLabel>
        
        <!-- Note: Only one BlockEditorLabel can be active at a time -->
        <!-- For both mentions and labels, use @ trigger with appropriate handling -->
    </SfBlockEditor>
</div>

<div class="card" style="margin-top: 20px;">
    <div class="card-header">
        <h5>Content Summary</h5>
    </div>
    <div class="card-body">
        <div style="font-size: 14px; line-height: 1.8;">
            <div><strong>Total Blocks:</strong> @blockData.Count</div>
            <div><strong>Mentions:</strong> @GetMentionCount() found</div>
            <div><strong>Labels:</strong> @GetLabelCount() found</div>
        </div>
    </div>
</div>

@code {
    private List<BlockModel> blockData = new();
    
    private List<UserModel> users = new()
    {
        new UserModel { Id = "u1", Name = "Alice Johnson" },
        new UserModel { Id = "u2", Name = "Bob Smith" },
        new UserModel { Id = "u3", Name = "Charlie Brown" },
        new UserModel { Id = "u4", Name = "Diana Prince" }
    };

    private List<LabelItemModel> labels = new()
    {
        new LabelItemModel { Id = "l1", Name = "Important", Color = "#FF6B6B" },
        new LabelItemModel { Id = "l2", Name = "Feature", Color = "#4ECDC4" },
        new LabelItemModel { Id = "l3", Name = "Bug", Color = "#FFD93D" }
    };

    protected override void OnInitialized()
    {
        blockData = new()
        {
            new BlockModel
            {
                BlockType = BlockType.Heading,
                Properties = new HeadingBlockSettings { Level = 1 },
                Content = new()
                {
                    new ContentModel { ContentType = ContentType.Text, Content = "Q1 Goals" }
                }
            },
            new BlockModel
            {
                BlockType = BlockType.Paragraph,
                Content = new()
                {
                    new ContentModel { ContentType = ContentType.Text, Content = "Discussed with " },
                    new ContentModel 
                    { 
                        ContentType = ContentType.Mention, 
                        Content = "@Alice", 
                        Properties = new MentionContentSettings { UserId = "u1" } 
                    },
                    new ContentModel { ContentType = ContentType.Text, Content = " about the new " },
                    new ContentModel 
                    { 
                        ContentType = ContentType.Label, 
                        Content = "#Feature", 
                        Properties = new LabelContentSettings { LabelId = "l2" } 
                    }
                }
            }
        };
    }

    private int GetMentionCount()
    {
        return blockData.SelectMany(b => b.Content).Count(c => c.ContentType == ContentType.Mention);
    }

    private int GetLabelCount()
    {
        return blockData.SelectMany(b => b.Content).Count(c => c.ContentType == ContentType.Label);
    }
}
```

---

## Practical Workflow Examples

### Example 1: Task Assignment with Mentions

```razor
private void CreateTaskAssignment(string taskDescription, string assigneeId)
{
    blockData.Add(new BlockModel
    {
        BlockType = BlockType.Paragraph,
        Content = new()
        {
            new ContentModel { ContentType = ContentType.Text, Content = "Task: " },
            new ContentModel { ContentType = ContentType.Text, Content = taskDescription },
            new ContentModel { ContentType = ContentType.Text, Content = " - Assigned to " },
            new ContentModel 
            { 
                ContentType = ContentType.Mention, 
                Content = $"@{users.First(u => u.Id == assigneeId).Name}",
                Properties = new MentionContentSettings { UserId = assigneeId }
            }
        }
    });
}
```

### Example 2: Issue Tracking with Labels

```razor
private void CreateIssue(string title, string issueType, string labelId)
{
    var label = labels.First(l => l.Id == labelId);
    
    blockData.Add(new BlockModel
    {
        BlockType = BlockType.Paragraph,
        Content = new()
        {
            new ContentModel { ContentType = ContentType.Text, Content = $"{title} " },
            new ContentModel 
            { 
                ContentType = ContentType.Label, 
                Content = $"#{label.Name}",
                Properties = new LabelContentSettings { LabelId = labelId }
            }
        }
    });
}
```

### Example 3: Comment Thread

```razor
private void AddComment(string comment, string userId)
{
    var user = users.First(u => u.Id == userId);
    
    blockData.Add(new BlockModel
    {
        BlockType = BlockType.Paragraph,
        Content = new()
        {
            new ContentModel { ContentType = ContentType.Text, Content = $"{user.Name}: " },
            new ContentModel { ContentType = ContentType.Text, Content = comment }
        }
    });
}
```

---

## Best Practices

### Mentions
1. **Notify Users**: Send notifications when mentioned
2. **Limit List**: Show most recent/relevant users first
3. **Validation**: Verify user IDs exist before saving
4. **Permission Check**: Only mention users in same workspace/team

### Labels
1. **Consistent Colors**: Use consistent color scheme across labels
2. **Limit Quantity**: Keep label list manageable (5-15 labels)
3. **Clear Names**: Use descriptive, short label names
4. **Documentation**: Document label meanings for team
5. **Cleanup**: Archive unused labels periodically

### General
1. **Save Content**: Persist mentions and labels to database
2. **Search Integration**: Index mentions/labels for search
3. **Export**: Include mention/label data when exporting
4. **Validation**: Validate user IDs and label IDs on save
5. **Performance**: Limit user/label list size for performance

---

## Common Patterns

### Pattern 1: Populate User List from API
```csharp
protected override async Task OnInitializedAsync()
{
    users = await HttpClient.GetFromJsonAsync<List<UserModel>>("/api/users");
}
```

### Pattern 2: Filter Labels by Category
```csharp
private List<LabelItemModel> GetLabelsByCategory(string category)
{
    return labels.Where(l => l.Category == category).ToList();
}
```

### Pattern 3: Extract Mentions for Processing
```csharp
private List<string> ExtractMentionedUserIds()
{
    return blockData
        .SelectMany(b => b.Content)
        .Where(c => c.ContentType == ContentType.Mention)
        .Select(c => (c.Properties as MentionContentSettings)?.UserId)
        .Where(id => id != null)
        .ToList();
}
```

---

## Troubleshooting

| Issue | Cause | Solution |
|-------|-------|----------|
| Mentions not showing | Users list empty | Populate Users property with UserModel items |
| Labels not showing | Items list empty or null | Set Items property with LabelItemModel items |
| Popup not appearing | Wrong trigger character | Verify TriggerChar is set correctly |
| Mentions not saving | ContentType mismatch | Ensure ContentType = ContentType.Mention |

---

**Last Updated:** March 24, 2026  
**Component Version:** 1.0.0+  
**Blazor:** .NET 8.0+
