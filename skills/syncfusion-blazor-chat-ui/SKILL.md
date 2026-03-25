---
name: syncfusion-blazor-chat-ui
description: Implement the Syncfusion Blazor Chat UI component for conversational chat interfaces in Blazor applications. Use this skill when creating chat applications, messaging interfaces, or multi-user conversations. Covers message management, user models, typing indicators, avatars, timestamps, and file attachments with customizable templates.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Syncfusion Blazor Chat UI

Build feature-rich conversational chat interfaces with the Syncfusion Blazor Chat UI component. This skill provides complete guidance for creating multi-user messaging applications, chat panels, and interactive conversation UIs in Blazor Server, WebAssembly, and Web App projects.

## Component Overview

The **SfChatUI** component is a specialized UI control for building conversational chat applications. It manages the complete chat interface including:
- **Message Management:** Collections of chat messages with full message lifecycle
- **User Management:** Multi-user support with profiles, avatars, and status
- **Presence Features:** Typing indicators, online status, user identification
- **Rich Rendering:** Templates for messages, timestamps, typing indicators, suggestions
- **File Support:** File attachment upload and integration
- **Event System:** Lifecycle events and user interaction callbacks
- **Customization:** Complete styling and theming control

## Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup
- Basic component structure and initialization
- User and message configuration
- Minimal working example with conversation

### Messages & Users Management
📄 **Read:** [references/messages-and-users.md](references/messages-and-users.md)
- ChatMessage configuration and properties
- UserModel setup and properties
- Message text content management
- Author identification and user assignment
- Building message collections and conversation history

### User Profiles & Avatars
📄 **Read:** [references/user-profiles-avatars.md](references/user-profiles-avatars.md)
- Avatar URL configuration with images
- Avatar background color customization
- User status indicators (online, offline, busy, away)
- Custom CSS styling for users
- User identification and display names

### Typing Indicators
📄 **Read:** [references/typing-indicators.md](references/typing-indicators.md)
- TypingUsers property configuration
- Showing/hiding typing status indicators
- Managing multiple typing users
- Dynamic typing status updates
- Typing indicator template customization

### Timestamps & Formatting
📄 **Read:** [references/timestamps-and-formatting.md](references/timestamps-and-formatting.md)
- ShowTimestamp property to enable/disable timestamps
- Message timestamp configuration
- TimestampFormat customization
- TimeBreak separators (Today, Yesterday, specific dates)
- TimeBreak template styling

### Templates & Customization
📄 **Read:** [references/templates-and-customization.md](references/templates-and-customization.md)
- EmptyChatTemplate for initial state display
- MessageTemplate for custom message rendering
- TimeBreakTemplate for date separators
- TypingUsersTemplate for typing display
- SuggestionTemplate for quick reply buttons
- CSS styling and theme integration

### Attachments & Events
📄 **Read:** [references/attachments-and-events.md](references/attachments-and-events.md)
- File attachment configuration (Enable)
- SaveUrl and RemoveUrl endpoint setup
- AllowedFileTypes filtering
- MaxFileSize limits and constraints
- SaveFormat options (Blob vs Base64)
- Event handling: Created, MessageSend, UserTyping
- Attachment events: OnAttachmentUploadReady, UploadSuccess, UploadFailed

### Programmatic API & Methods 🆕
📄 **Read:** [references/programmatic-api.md](references/programmatic-api.md)
- ScrollToBottomAsync() for navigating to latest messages
- ScrollToMessageAsync() for jumping to specific messages
- UpdateMessageAsync() for editing existing messages
- FocusAsync() for input field control
- Complete examples with error handling

### Advanced Features 🆕
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- AutoScrollToBottom configuration
- EnableCompactMode for group chats
- Header and Footer customization
- Mention system (@mention) setup
- Quick reply suggestions
- LoadOnDemand for performance
- RTL (Right-to-Left) support
- Custom styling with CssClass

## Quick Start

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 500px; width: 600px;">
    <SfChatUI ID="chatUser" User="CurrentUser" Messages="Messages"></SfChatUI>
</div>

@code {
    private UserModel CurrentUser = new UserModel 
    { 
        ID = "user1", 
        User = "Albert",
        AvatarBgColor = "#4a90e2"
    };

    private UserModel OtherUser = new UserModel 
    { 
        ID = "user2", 
        User = "Michale Suyama",
        AvatarBgColor = "#7cb342"
    };

    private List<ChatMessage> Messages = new()
    {
        new ChatMessage 
        { 
            Text = "Hi, how are you?", 
            Author = new UserModel { ID = "user1", User = "Albert" }
        },
        new ChatMessage 
        { 
            Text = "I'm doing great! How about you?", 
            Author = new UserModel { ID = "user2", User = "Michale Suyama" }
        }
    };
}
```

## Common Patterns

### Pattern 1: Multi-User Conversation with Status
Display multiple users with online/offline status indicators:

```csharp
<SfChatUI ID="chat" User="CurrentUser" Messages="Messages">
</SfChatUI>

@code {
    private UserModel CurrentUser = new UserModel 
    { 
        ID = "user1", 
        User = "Albert",
        StatusIconCss = "e-icons e-user-online"
    };

    private List<ChatMessage> Messages = new();
}
```

### Pattern 2: Chat with Typing Indicators
Show when users are typing:

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    TypingUsers="TypingUsers">
</SfChatUI>

@code {
    private List<UserModel> TypingUsers = new();
    
    private void UserStartTyping(UserModel user)
    {
        TypingUsers.Add(user);
        StateHasChanged();
    }
}
```

### Pattern 3: Custom Message Templates
Personalize message appearance:

```csharp
<SfChatUI ID="chat" User="CurrentUser" Messages="Messages">
    <MessageTemplate>
        <div class="custom-message">
            <strong>@context.Message.Author.User</strong>
            <p>@((MarkupString)context.Message.Text)</p>
        </div>
    </MessageTemplate>
</SfChatUI>
```

### Pattern 4: Chat with File Attachments
Enable users to share files:

```csharp
<SfChatUI ID="chat" User="CurrentUser" Messages="Messages">
    <ChatUIAttachment 
        Enable 
        SaveUrl="@SaveUrl" 
        RemoveUrl="@RemoveUrl"
        AllowedFileTypes=".pdf,.docx,.jpg,.png">
    </ChatUIAttachment>
</SfChatUI>

@code {
    private string SaveUrl = "api/upload/save";
    private string RemoveUrl = "api/upload/remove";
}
```

### Pattern 5: Auto-Scroll with Programmatic Control
Automatically scroll to bottom and navigate to specific messages:

```csharp
<SfChatUI 
    @ref="chatRef"
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    AutoScrollToBottom="true">
</SfChatUI>

<button @onclick="ScrollToBottom">Go to Latest</button>
<button @onclick="FocusInput">Focus Input</button>

@code {
    private SfChatUI chatRef;
    
    private async Task ScrollToBottom()
    {
        await chatRef.ScrollToBottomAsync();
    }
    
    private async Task FocusInput()
    {
        await chatRef.FocusAsync();
    }
}
```

### Pattern 6: Message Editing with UpdateMessageAsync
Edit existing messages programmatically:

```csharp
<SfChatUI @ref="chatRef" ID="chat" User="CurrentUser" Messages="Messages">
</SfChatUI>

@code {
    private SfChatUI chatRef;
    
    private async Task EditMessage(string messageId, string newText)
    {
        var message = Messages.FirstOrDefault(m => m.ID == messageId);
        if (message != null)
        {
            message.Text = newText;
            await chatRef.UpdateMessageAsync(message, messageId);
        }
    }
}
```

### Pattern 7: Mention System for User Tagging
Enable users to mention others in messages:

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    MentionChar="@"
    MentionUsers="AllUsers"
    ValueSelecting="@OnMentionSelected">
</SfChatUI>

@code {
    private List<UserModel> AllUsers = new()
    {
        new UserModel { ID = "user1", User = "Albert" },
        new UserModel { ID = "user2", User = "Michale" },
        new UserModel { ID = "user3", User = "Reena" }
    };
    
    private void OnMentionSelected(MentionValueSelectingEventArgs<UserModel> args)
    {
        Console.WriteLine($"User mentioned: {args.ItemData.User}");
    }
}
```

### Pattern 8: Compact Mode for Group Chats
Display all messages left-aligned for group chat style:

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    EnableCompactMode="true">
</SfChatUI>

@code {
    // In compact mode, all messages appear on left side
    // Useful for group chat or support ticket interfaces
}
```

## Key Props

| Property | Type | Purpose |
|----------|------|---------|
| `ID` | string | Unique component identifier |
| `User` | UserModel | Current logged-in user |
| `Messages` | List\<ChatMessage\> | Conversation messages collection |
| `ShowTimestamp` | bool | Display message timestamps (default: true) |
| `TimestampFormat` | string | Date/time format (default: "dd/MM/yyyy hh:mm tt") |
| `ShowTimeBreak` | bool | Display date separators between messages |
| `TypingUsers` | List\<UserModel\> | Users currently typing |
| `AutoScrollToBottom` | bool | Auto-scroll to bottom on new messages (default: false) |
| `EnableCompactMode` | bool | Align all messages to left side (default: false) |
| `Height` | string | Component height (default: "100%") |
| `Width` | string | Component width (default: "100%") |
| `HeaderText` | string | Header text display (default: "Chat") |
| `ShowHeader` | bool | Show/hide header (default: true) |
| `ShowFooter` | bool | Show/hide footer (default: true) |
| `Placeholder` | string | Input placeholder text (default: "Type your message…") |
| `Suggestions` | List\<string\> | Quick reply suggestions |
| `MentionChar` | char | Mention trigger character (default: '@') |
| `MentionUsers` | List\<UserModel\> | Users available for mention |
| `LoadOnDemand` | bool | Load messages on demand (default: false) |
| `CssClass` | string | Custom CSS styling |
| `EnableRtl` | bool | Right-to-left direction support (default: false) |
| `EmptyChatTemplate` | RenderFragment | Custom empty state content |
| `MessageTemplate` | RenderFragment\<MessageTemplateContext\> | Custom message rendering |
| `TimeBreakTemplate` | RenderFragment\<TimeBreakTemplateContext\> | Custom date separator |
| `TypingUsersTemplate` | RenderFragment\<TypingUsersTemplateContext\> | Custom typing indicator |
| `SuggestionTemplate` | RenderFragment\<SuggestionTemplateContext\> | Custom suggestion display |
| `FooterTemplate` | RenderFragment | Custom footer area |
| `PreviewTemplate` | RenderFragment\<PreviewTemplateContext\> | Custom attachment preview |

## Programmatic Methods

The Chat UI component provides async methods for programmatic control:

### ScrollToBottomAsync()
Scrolls the chat to the bottom, useful for showing latest messages:

```csharp
@ref="chatRef"

private SfChatUI chatRef;

private async Task ShowLatestMessages()
{
    await chatRef.ScrollToBottomAsync();
}
```

### ScrollToMessageAsync(string messageId)
Navigates to a specific message by ID:

```csharp
private async Task NavigateToMessage(string targetMessageId)
{
    await chatRef.ScrollToMessageAsync(targetMessageId);
}
```

### UpdateMessageAsync(ChatMessage message, string msgId)
Updates an existing message content:

```csharp
private async Task EditMessage(string messageId, string newText)
{
    var message = Messages.FirstOrDefault(m => m.ID == messageId);
    if (message != null)
    {
        message.Text = newText;
        await chatRef.UpdateMessageAsync(message, messageId);
    }
}
```

### FocusAsync()
Sets focus on the chat input field:

```csharp
private async Task FocusChatInput()
{
    await chatRef.FocusAsync();
}
```

## Common Use Cases

1. **Customer Support Chat** - Live support widget with agent presence
2. **Team Messaging** - Internal communication platform
3. **Bot Integration** - AI chatbot with user interface
4. **Community Chat** - Multi-user conversation rooms
5. **Help Desk** - Ticketing and messaging interface
6. **Social Features** - In-app messaging and notifications
7. **Real-time Collaboration** - Live chat during shared activities
