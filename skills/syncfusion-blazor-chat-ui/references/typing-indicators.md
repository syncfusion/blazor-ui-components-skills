# Typing Indicators

## Table of Contents
- [Show/Hide Typing Indicators](#showhide-typing-indicators)
- [Multiple Users Typing](#multiple-users-typing)
- [Dynamic Typing Status](#dynamic-typing-status)
- [Typing User Management](#typing-user-management)
- [Typing Indicator Templates](#typing-indicator-templates)

## Show/Hide Typing Indicators

The [TypingUsers](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_TypingUsers) property displays the current users who are typing to indicate active participants. When the property is empty, typing indicators are removed.

### Basic Typing Indicator

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 600px;">
    <SfChatUI 
        ID="chatUser" 
        User="CurrentUserModel" 
        TypingUsers="TypingUsers" 
        Messages="ChatUserMessages">
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

    // Users currently typing
    private List<UserModel> TypingUsers = new() 
    { 
        MichaleUserModel 
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
```

### Toggle Typing Indicator

Show/hide typing indicator on demand:

```csharp
private async Task StartTyping(UserModel user)
{
    if (!TypingUsers.Contains(user))
    {
        TypingUsers.Add(user);
        StateHasChanged();
    }
}

private async Task StopTyping(UserModel user)
{
    TypingUsers.Remove(user);
    StateHasChanged();
}

// Usage
private async Task UserStartsTyping()
{
    await StartTyping(MichaleUserModel);
    
    // Simulate typing duration
    await Task.Delay(3000);
    
    // Send message and stop typing
    var message = new ChatMessage
    {
        Text = "Great! Let me know when you start.",
        Author = MichaleUserModel
    };
    
    ChatUserMessages.Add(message);
    await StopTyping(MichaleUserModel);
    StateHasChanged();
}
```

## Multiple Users Typing

Show when multiple users are typing simultaneously:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 600px;">
    <SfChatUI 
        ID="groupChat" 
        User="CurrentUser" 
        TypingUsers="TypingUsers" 
        Messages="Messages">
    </SfChatUI>
</div>

@code {
    private UserModel CurrentUser = new UserModel() 
    { 
        ID = "user1", 
        User = "Albert" 
    };

    private UserModel MichaleUser = new UserModel() 
    { 
        ID = "user2", 
        User = "Michale Suyama" 
    };

    private UserModel ReenaUser = new UserModel() 
    { 
        ID = "user3", 
        User = "Reena" 
    };

    private List<UserModel> TypingUsers = new();
    private List<ChatMessage> Messages = new();

    // Simulate multiple users typing
    private async Task SimulateGroupTyping()
    {
        // Michale starts typing
        TypingUsers.Add(MichaleUser);
        StateHasChanged();
        
        await Task.Delay(1000);
        
        // Reena also starts typing
        TypingUsers.Add(ReenaUser);
        StateHasChanged();
        
        await Task.Delay(2000);
        
        // Michale sends message
        Messages.Add(new ChatMessage
        {
            Text = "I have a suggestion",
            Author = MichaleUser
        });
        TypingUsers.Remove(MichaleUser);
        StateHasChanged();
        
        await Task.Delay(1000);
        
        // Reena sends message
        Messages.Add(new ChatMessage
        {
            Text = "I was thinking the same thing",
            Author = ReenaUser
        });
        TypingUsers.Remove(ReenaUser);
        StateHasChanged();
    }
}
```

## Dynamic Typing Status

### Update Typing Status in Real-time

Manage typing status dynamically based on user input:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 600px;">
    <SfChatUI 
        ID="chat" 
        User="CurrentUser" 
        TypingUsers="TypingUsers" 
        Messages="Messages"
        UserTyping="@OnUserTyping">
    </SfChatUI>
</div>

@code {
    private UserModel CurrentUser = new UserModel() 
    { 
        ID = "user1", 
        User = "You" 
    };

    private List<UserModel> TypingUsers = new();
    private List<ChatMessage> Messages = new();
    private bool isUserTyping = false;

    private async Task OnUserTyping(ChatUserTypingEventArgs args)
    {
        // User is typing
        if (!isUserTyping)
        {
            isUserTyping = true;
            
            // Notify other users (would send to server/SignalR)
            await NotifyOthersUserTyping();
        }
        
        // Reset typing timer
        await ResetTypingTimer();
    }

    private async Task ResetTypingTimer()
    {
        // After 3 seconds of no input, assume user stopped typing
        await Task.Delay(3000);
        isUserTyping = false;
        
        // Notify others user stopped typing
        await NotifyOthersUserStoppedTyping();
    }

    private async Task NotifyOthersUserTyping()
    {
        // Would communicate via SignalR or HTTP to other clients
        // Others would add CurrentUser to their TypingUsers list
    }

    private async Task NotifyOthersUserStoppedTyping()
    {
        // Would communicate via SignalR or HTTP
        // Others would remove CurrentUser from their TypingUsers list
    }
}
```

## Typing User Management

### Add/Remove Typing Users

Programmatically manage the typing users list:

```csharp
private void AddTypingUser(UserModel user)
{
    if (!TypingUsers.Any(u => u.ID == user.ID))
    {
        TypingUsers.Add(user);
        StateHasChanged();
    }
}

private void RemoveTypingUser(UserModel user)
{
    TypingUsers.RemoveAll(u => u.ID == user.ID);
    StateHasChanged();
}

private void ClearTypingUsers()
{
    TypingUsers.Clear();
    StateHasChanged();
}

private bool IsUserTyping(string userId)
{
    return TypingUsers.Any(u => u.ID == userId);
}
```

### Typing Timeout Management

Automatically clear typing status after inactivity:

```csharp
private Dictionary<string, System.Timers.Timer> TypingTimers = new();

private void StartTypingTimeout(UserModel user)
{
    AddTypingUser(user);
    
    // Cancel existing timer if any
    if (TypingTimers.ContainsKey(user.ID))
    {
        TypingTimers[user.ID].Stop();
        TypingTimers[user.ID].Dispose();
    }
    
    // Create new timeout timer (3 seconds)
    var timer = new System.Timers.Timer(3000);
    timer.Elapsed += (s, e) =>
    {
        RemoveTypingUser(user);
        timer.Stop();
        timer.Dispose();
        TypingTimers.Remove(user.ID);
    };
    
    TypingTimers[user.ID] = timer;
    timer.Start();
}
```

## Typing Indicator Templates

Customize the typing indicator appearance using templates:

### Basic Typing Template

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 600px;">
    <SfChatUI 
        ID="chat" 
        User="CurrentUser" 
        TypingUsers="TypingUsers" 
        Messages="Messages">
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
</div>

<style>
    .typing-wrapper {
        display: flex;
        gap: 4px;
        align-items: center;
        font-family: Arial, sans-serif;
        font-size: 14px;
        color: #555;
        margin: 5px 0;
    }

    .typing-user {
        font-weight: bold;
        color: #0078d4;
    }
</style>

@code {
    private UserModel CurrentUser = new UserModel() 
    { 
        ID = "user1", 
        User = "Albert" 
    };

    private UserModel MichaleUser = new UserModel() 
    { 
        ID = "user2", 
        User = "Michale" 
    };

    private UserModel ReenaUser = new UserModel() 
    { 
        ID = "user3", 
        User = "Reena" 
    };

    private List<UserModel> TypingUsers = new() 
    { 
        MichaleUser, 
        ReenaUser 
    };

    private List<ChatMessage> Messages = new();
}
```

### Advanced Typing Template with Indicators

```csharp
<SfChatUI ID="chat" User="CurrentUser" TypingUsers="TypingUsers" Messages="Messages">
    <TypingUsersTemplate>
        <div class="custom-typing">
            <div class="typing-avatars">
                @foreach (var user in context.Users)
                {
                    <div class="typing-avatar" title="@user.User">
                        @user.User.Substring(0, 1)
                    </div>
                }
            </div>
            <div class="typing-indicator">
                <span></span><span></span><span></span>
            </div>
        </div>
    </TypingUsersTemplate>
</SfChatUI>

<style>
    .custom-typing {
        display: flex;
        align-items: center;
        gap: 8px;
        padding: 8px;
    }

    .typing-avatars {
        display: flex;
        gap: -4px;
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
        margin-left: -4px;
    }

    .typing-indicator {
        display: flex;
        gap: 3px;
    }

    .typing-indicator span {
        width: 8px;
        height: 8px;
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
            transform: translateY(0);
        }
        30% {
            opacity: 1;
            transform: translateY(-8px);
        }
    }
</style>
```

## Best Practices

1. **Set Typing Timeout** - Clear typing status after inactivity
2. **Avoid Duplicate Users** - Check if user already in TypingUsers before adding
3. **Real-time Sync** - Use SignalR or WebSockets to sync typing status
4. **Performance** - Limit number of typing indicators shown
5. **User Feedback** - Provide clear visual indication of who is typing
6. **Stop on Send** - Remove user from typing list when message is sent
7. **Accessibility** - Include text descriptions in typing templates
