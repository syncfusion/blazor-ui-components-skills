# Messages & Users Management

## Table of Contents
- [Message Configuration](#message-configuration)
- [User Model Setup](#user-model-setup)
- [Message Text Content](#message-text-content)
- [Author Identification](#author-identification)
- [Message Collections](#message-collections)

## Message Configuration

The Blazor Chat UI component manages messages through the [Messages](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_Messages) property. Each message is a `ChatMessage` object representing a single message in the conversation.

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 600px;">
    <SfChatUI ID="chatUser" User="CurrentUserModel" Messages="ChatUserMessages"></SfChatUI>
</div>

@code {
    private List<ChatMessage> ChatUserMessages = new()
    {
        new ChatMessage() 
        { 
            Text = "Hi, thinking of painting this weekend.", 
            Author = new UserModel { ID = "User1", User = "Albert" }
        },
        new ChatMessage() 
        { 
            Text = "That's fun! What will you paint?", 
            Author = new UserModel { ID = "User2", User = "Michale Suyama" }
        },
        new ChatMessage() 
        { 
            Text = "Maybe landscapes.", 
            Author = new UserModel { ID = "User1", User = "Albert" }
        }
    };
}
```

## User Model Setup

### Defining the Current User

The `User` property specifies the currently logged-in user. This is essential for identifying which messages belong to the current user (displayed on the right) versus other users (displayed on the left).

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 600px;">
    <SfChatUI ID="chatUser" User="CurrentUserModel" Messages="ChatUserMessages"></SfChatUI>
</div>

@code {
    private UserModel CurrentUserModel = new UserModel() 
    { 
        ID = "User1", 
        User = "Albert" 
    };

    private List<ChatMessage> ChatUserMessages = new()
    {
        new ChatMessage() 
        { 
            Text = "Hi, thinking of painting this weekend.", 
            Author = new UserModel { ID = "User1", User = "Albert" }
        },
        new ChatMessage() 
        { 
            Text = "That's fun! What will you paint?", 
            Author = new UserModel { ID = "User2", User = "Michale Suyama" }
        }
    };
}
```

**Key UserModel Properties:**
- **ID:** Unique identifier for the user (required for matching with message authors)
- **User:** Display name shown in the interface
- **AvatarUrl:** Optional image URL for avatar
- **AvatarBgColor:** Background color for avatar initials
- **StatusIconCss:** CSS class for status indicator
- **CssClass:** Custom CSS styling

### Creating Multiple Users

Define different users for a multi-user conversation:

```csharp
private UserModel AlbertUser = new UserModel() 
{ 
    ID = "user1", 
    User = "Albert",
    AvatarBgColor = "#4a90e2"
};

private UserModel MichaleUser = new UserModel() 
{ 
    ID = "user2", 
    User = "Michale Suyama",
    AvatarBgColor = "#7cb342"
};

private UserModel ReenaUser = new UserModel() 
{ 
    ID = "user3", 
    User = "Reena",
    AvatarBgColor = "#e84c3d"
};
```

## Message Text Content

### Plain Text Messages

Messages can contain plain text content:

```csharp
new ChatMessage() 
{ 
    Text = "This is a plain text message.", 
    Author = CurrentUserModel 
}
```

### HTML-Formatted Messages

Messages support HTML content for rich formatting:

```csharp
new ChatMessage() 
{ 
    Text = "<div><strong>Important:</strong> Please review the document.</div>", 
    Author = CurrentUserModel 
}
```

When rendering HTML messages in templates, use `@((MarkupString)context.Message.Text)` to prevent escaping:

```csharp
<SfChatUI ID="chat" User="CurrentUser" Messages="Messages">
    <MessageTemplate>
        <div>@((MarkupString)context.Message.Text)</div>
    </MessageTemplate>
</SfChatUI>
```

### Message with Special Formatting

Create formatted messages with structure:

```csharp
private ChatMessage CreateFormattedMessage(string title, string content, UserModel author)
{
    var html = $@"
        <div style='border-left: 3px solid #4a90e2; padding: 8px;'>
            <div style='font-weight: bold; color: #333;'>{title}</div>
            <div style='color: #666; margin-top: 4px;'>{content}</div>
        </div>";
    
    return new ChatMessage { Text = html, Author = author };
}
```

## Author Identification

The [Author](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.ChatMessage.html#Syncfusion_Blazor_InteractiveChat_ChatMessage_Author) property identifies the sender of each message. The component compares the author ID with the current user ID to determine message alignment.

### Message Alignment Logic

```csharp
// Messages from the current user appear on the right
if (message.Author.ID == CurrentUser.ID) 
{
    // Right-aligned (current user message)
}
else
{
    // Left-aligned (other user message)
}
```

### Creating Messages with Authors

```csharp
private ChatMessage CreateMessage(string text, UserModel sender)
{
    return new ChatMessage 
    { 
        Text = text,
        Author = sender  // Identifies the sender
    };
}

// Usage
var albertMessage = CreateMessage("Hello!", AlbertUser);
var michaleMessage = CreateMessage("Hi there!", MichaleUser);
```

## Message Collections

### Initializing Message Collection

Load pre-existing messages into the component:

```csharp
private List<ChatMessage> ChatUserMessages = new()
{
    new ChatMessage() 
    { 
        Text = "Hi, thinking of painting this weekend.", 
        Author = new UserModel { ID = "User1", User = "Albert" }
    },
    new ChatMessage() 
    { 
        Text = "That's fun! What will you paint?", 
        Author = new UserModel { ID = "User2", User = "Michale Suyama" }
    },
    new ChatMessage() 
    { 
        Text = "Maybe landscapes.", 
        Author = new UserModel { ID = "User1", User = "Albert" }
    }
};
```

### Adding Messages Dynamically

Add new messages to the collection during conversation:

```csharp
private async Task AddNewMessage(string text, UserModel author)
{
    var newMessage = new ChatMessage 
    { 
        Text = text,
        Author = author,
        Timestamp = DateTime.Now
    };
    
    ChatUserMessages.Add(newMessage);
    StateHasChanged();
}
```

### Clearing Message History

Remove all messages from the conversation:

```csharp
private void ClearConversation()
{
    ChatUserMessages.Clear();
    StateHasChanged();
}
```

### Message History with Pagination

Load messages in batches for performance:

```csharp
private async Task LoadMoreMessages()
{
    // Fetch older messages from database
    var olderMessages = await FetchOlderMessages(lastMessageId: ChatUserMessages.First().ID);
    
    // Prepend to collection
    ChatUserMessages.InsertRange(0, olderMessages);
    StateHasChanged();
}

private async Task<List<ChatMessage>> FetchOlderMessages(string lastMessageId)
{
    // Database query for messages before the first current message
    return await Database.GetMessagesBeforeId(lastMessageId);
}
```

### Message Count and Statistics

Track conversation statistics:

```csharp
private int GetTotalMessageCount()
{
    return ChatUserMessages.Count;
}

private int GetMessagesFromUser(UserModel user)
{
    return ChatUserMessages.Count(m => m.Author.ID == user.ID);
}

private DateTime GetLastMessageTime()
{
    return ChatUserMessages.LastOrDefault()?.Timestamp ?? DateTime.MinValue;
}
```

## Best Practices

1. **Always assign Author** - Every message must have an Author
2. **Match Author ID to Current User** - Ensure author IDs match the User ID for correct alignment
3. **Use Consistent User Objects** - Reuse UserModel instances across messages
4. **HTML Sanitization** - Sanitize user-generated HTML for security
5. **Timestamp Management** - Always set message timestamps
6. **Message Immutability** - Avoid modifying message content after creation
7. **Collection Size** - For large conversations, implement pagination
8. **Error Handling** - Handle null authors and invalid messages gracefully
