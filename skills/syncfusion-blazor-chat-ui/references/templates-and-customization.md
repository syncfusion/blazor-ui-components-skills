# Templates & Customization

## Table of Contents
- [Empty Chat Template](#empty-chat-template)
- [Message Template](#message-template)
- [TimeBreak Template](#timebreak-template)
- [Typing Users Template](#typing-users-template)
- [Suggestion Template](#suggestion-template)
- [CSS Styling](#css-styling)

## Empty Chat Template

The [EmptyChatTemplate](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_EmptyChatTemplate) customizes the chat interface when no messages are displayed. This creates an engaging initial experience for users starting a conversation.

### Basic Empty Chat Template

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 600px;">
    <SfChatUI ID="chatUser" User="CurrentUserModel">
        <EmptyChatTemplate>
            <div class="empty-chat-text">
                <h4><span class="e-icons e-comment-show"></span></h4>
                <h4>No Messages Yet</h4>
                <p>Start a conversation to see your messages here.</p>
            </div>
        </EmptyChatTemplate>
    </SfChatUI>
</div>

<style>
    .empty-chat-text {
        font-size: 15px;
        text-align: center;
        margin-top: 90px;
    }
</style>

@code {
    private UserModel CurrentUserModel = new UserModel() 
    { 
        ID = "User1", 
        User = "Albert" 
    };
}
```

### Custom Welcome Template

```csharp
<SfChatUI ID="chat" User="CurrentUser">
    <EmptyChatTemplate>
        <div class="welcome-template">
            <div class="welcome-header">
                <h2>Welcome! 👋</h2>
                <p>Start chatting with your friends</p>
            </div>
            <div class="welcome-suggestions">
                <div class="suggestion-item">
                    <span class="icon">📝</span>
                    <p>Send your first message</p>
                </div>
                <div class="suggestion-item">
                    <span class="icon">👥</span>
                    <p>Add more participants</p>
                </div>
                <div class="suggestion-item">
                    <span class="icon">📎</span>
                    <p>Share files and media</p>
                </div>
            </div>
        </div>
    </EmptyChatTemplate>
</SfChatUI>

<style>
    .welcome-template {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        height: 100%;
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        padding: 20px;
    }

    .welcome-header h2 {
        margin: 0 0 10px 0;
    }

    .welcome-suggestions {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 20px;
        margin-top: 40px;
        width: 100%;
    }

    .suggestion-item {
        text-align: center;
    }

    .suggestion-item .icon {
        font-size: 32px;
        display: block;
        margin-bottom: 8px;
    }
</style>
```

## Message Template

The [MessageTemplate](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_MessageTemplate) customizes the appearance and styling of each message. The template context includes `Message` and `Index`.

### Basic Custom Message Template

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div class="template-chatui" style="height: 400px; width: 600px;">
    <SfChatUI ID="chatUser" User="CurrentUserModel" Messages="ChatUserMessages">
        <MessageTemplate>
            <div class="message-items e-card">
                <div class="message-text">@((MarkupString)context.Message.Text)</div>
            </div>
        </MessageTemplate>
    </SfChatUI>
</div>

@code {
    private UserModel CurrentUserModel = new UserModel() 
    { 
        ID = "User1", 
        User = "Albert" 
    };

    private UserModel MichaleUserModel = new UserModel() 
    { 
        ID = "User2", 
        User = "Michale Suyama" 
    };

    private List<ChatMessage> ChatUserMessages = new()
    {
        new ChatMessage() 
        { 
            Text = "Hi, thinking of painting this weekend.", 
            Author = CurrentUserModel 
        },
        new ChatMessage() 
        { 
            Text = "That's fun! What will you paint?", 
            Author = MichaleUserModel 
        }
    };
}

<style>
    .template-chatui .e-right .message-items {
        border-radius: 16px 16px 2px 16px;
        background-color: #c5ffbf;
    }

    .template-chatui .e-left .message-items {
        border-radius: 16px 16px 16px 2px;
        background-color: #f5f5f5;
    }

    .template-chatui .message-items {
        padding: 8px 12px;
        max-width: 70%;
    }

    .message-text {
        word-wrap: break-word;
    }
</style>
```

### Advanced Message Template with Metadata

```csharp
<MessageTemplate>
    <div class="custom-message">
        <div class="message-header">
            <strong>@context.Message.Author.User</strong>
            <span class="message-time">@context.Message.Timestamp?.ToString("hh:mm tt")</span>
        </div>
        <div class="message-content">
            @((MarkupString)context.Message.Text)
        </div>
        <div class="message-footer">
            <span class="message-index">#@context.Index</span>
        </div>
    </div>
</MessageTemplate>

<style>
    .custom-message {
        padding: 8px 12px;
        border-radius: 8px;
        background: #f5f5f5;
    }

    .message-header {
        display: flex;
        justify-content: space-between;
        font-size: 12px;
        margin-bottom: 4px;
    }

    .message-time {
        color: #999;
        font-size: 11px;
    }

    .message-content {
        margin: 4px 0;
    }

    .message-footer {
        font-size: 10px;
        color: #ccc;
        margin-top: 4px;
    }
</style>
```

## TimeBreak Template

Customize date separators between message groups using [TimeBreakTemplate](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_TimeBreakTemplate). Template context includes `MessageDate`.

### Basic TimeBreak Template

```csharp
<SfChatUI ID="chat" User="CurrentUser" Messages="Messages" ShowTimeBreak="true">
    <TimeBreakTemplate>
        <div class="timebreak-wrapper">
            @(context.MessageDate.Value.ToString("MMMM dd, yyyy"))
        </div>
    </TimeBreakTemplate>
</SfChatUI>

<style>
    .timebreak-wrapper {
        background-color: #6495ed;
        color: #ffffff;
        border-radius: 5px;
        padding: 4px 8px;
        text-align: center;
        margin: 8px 0;
        font-size: 12px;
        font-weight: 500;
    }
</style>
```

### Advanced TimeBreak with Relative Dates

```csharp
<TimeBreakTemplate>
    <div class="advanced-timebreak">
        <span class="date-label">@GetRelativeDate(context.MessageDate.Value)</span>
    </div>
</TimeBreakTemplate>

<style>
    .advanced-timebreak {
        display: flex;
        align-items: center;
        gap: 12px;
        margin: 16px 0;
    }

    .advanced-timebreak::before,
    .advanced-timebreak::after {
        content: '';
        flex: 1;
        height: 1px;
        background: #ddd;
    }

    .date-label {
        background: white;
        padding: 0 12px;
        color: #666;
        font-size: 12px;
        font-weight: 600;
        white-space: nowrap;
    }
</style>

@code {
    private string GetRelativeDate(DateTime messageDate)
    {
        var today = DateTime.Today;
        if (messageDate.Date == today)
            return "Today";
        else if (messageDate.Date == today.AddDays(-1))
            return "Yesterday";
        else if (messageDate.Date > today.AddDays(-7))
            return messageDate.ToString("dddd");
        else
            return messageDate.ToString("MMMM dd");
    }
}
```

## Typing Users Template

Customize the typing indicator using [TypingUsersTemplate](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_TypingUsersTemplate). Template context includes `Users`.

### Basic Typing Template

```csharp
<SfChatUI ID="chat" User="CurrentUser" Messages="Messages" TypingUsers="TypingUsers">
    <TypingUsersTemplate>
        <div class="typing-wrapper">
            @for (int i = 0; i < context.Users.Count; i++)
            {
                if (i == context.Users.Count - 1 && i > 0)
                {
                    <span>and </span>
                }
                <span class="typing-user">@context.Users[i].User</span>
            }
            <span> @(context.Users.Count == 1 ? "is" : "are") typing...</span>
        </div>
    </TypingUsersTemplate>
</SfChatUI>

<style>
    .typing-wrapper {
        display: flex;
        gap: 4px;
        align-items: center;
        font-family: Arial, sans-serif;
        font-size: 13px;
        color: #666;
        margin: 4px 0;
    }

    .typing-user {
        font-weight: 600;
        color: #0078d4;
    }
</style>
```

### Advanced Typing with Animation

```csharp
<TypingUsersTemplate>
    <div class="advanced-typing">
        <div class="typing-avatars">
            @foreach (var user in context.Users)
            {
                <div class="typing-avatar">@user.User[0]</div>
            }
        </div>
        <div class="typing-indicator">
            <span></span><span></span><span></span>
        </div>
    </div>
</TypingUsersTemplate>

<style>
    .advanced-typing {
        display: flex;
        align-items: center;
        gap: 8px;
        padding: 8px;
    }

    .typing-avatars {
        display: flex;
        margin-left: -8px;
    }

    .typing-avatar {
        width: 28px;
        height: 28px;
        border-radius: 50%;
        background: #4a90e2;
        color: white;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 12px;
        font-weight: bold;
        margin-left: -8px;
        border: 2px solid white;
    }

    .typing-indicator {
        display: flex;
        gap: 3px;
        margin-left: 4px;
    }

    .typing-indicator span {
        width: 6px;
        height: 6px;
        border-radius: 50%;
        background: #999;
        animation: typing 1.4s infinite;
    }

    .typing-indicator span:nth-child(2) {
        animation-delay: 0.2s;
    }

    .typing-indicator span:nth-child(3) {
        animation-delay: 0.4s;
    }

    @keyframes typing {
        0%, 60%, 100% {
            opacity: 0.5;
        }
        30% {
            opacity: 1;
        }
    }
</style>
```

## Suggestion Template

Customize quick reply suggestions using [SuggestionTemplate](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_SuggestionTemplate).

### Basic Suggestion Template

```csharp
<SfChatUI ID="chat" User="CurrentUser" Messages="Messages">
    <SuggestionTemplate>
        <button class="suggestion-btn">
            @context.Suggestion
        </button>
    </SuggestionTemplate>
</SfChatUI>

<style>
    .suggestion-btn {
        background: #f0f0f0;
        border: 1px solid #ddd;
        padding: 8px 12px;
        border-radius: 4px;
        margin: 4px;
        cursor: pointer;
        transition: all 0.3s ease;
    }

    .suggestion-btn:hover {
        background: #e0e0e0;
        border-color: #999;
    }
</style>
```

## CSS Styling

### Component-Wide Styling

```csharp
<style>
    /* Chat container */
    .e-chat-ui {
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        background: #ffffff;
    }

    /* Message area */
    .e-chat-ui .e-chat-message-area {
        background: #f9f9f9;
        padding: 12px;
    }

    /* Left messages (other users) */
    .e-chat-ui .e-left .e-message-box {
        background: #e3e3e3;
        color: #333;
    }

    /* Right messages (current user) */
    .e-chat-ui .e-right .e-message-box {
        background: #4a90e2;
        color: white;
    }

    /* Message text */
    .e-chat-ui .e-message-text {
        word-wrap: break-word;
        max-width: 100%;
    }

    /* Avatar */
    .e-chat-ui .e-message-icon {
        width: 36px;
        height: 36px;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
    }
</style>
```

### Theme Integration

```csharp
<!-- Material theme -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Bootstrap theme -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Fluent theme -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />
```

## Best Practices

1. **Use MarkupString** - Always use `@((MarkupString)...)` for HTML content
2. **Responsive Templates** - Design templates that work on mobile and desktop
3. **Performance** - Keep templates lightweight, avoid complex logic
4. **Accessibility** - Include alt text and labels in templates
5. **Consistent Styling** - Maintain consistent colors and spacing
6. **Theme Support** - Test templates with different themes
7. **Error Handling** - Handle null or missing data gracefully
