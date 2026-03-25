# User Profiles & Avatars

## Table of Contents
- [Avatar URLs](#avatar-urls)
- [Avatar Background Colors](#avatar-background-colors)
- [User Status Indicators](#user-status-indicators)
- [Custom CSS Styling](#custom-css-styling)
- [User Display Configuration](#user-display-configuration)

## Avatar URLs

The [AvatarUrl](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.UserModel.html#Syncfusion_Blazor_InteractiveChat_UserModel_AvatarUrl) property defines the image URL for a user's avatar. When a URL is provided, the component displays the image. When no URL is provided, it displays fallback initials derived from the user's display name.

### Using Avatar Images

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 600px;">
    <SfChatUI ID="chatUser" User="CurrentUserModel" Messages="ChatUserMessages"></SfChatUI>
</div>

@code {
    private UserModel CurrentUserModel = new UserModel() 
    { 
        ID = "User1", 
        User = "Albert",
        AvatarUrl = "https://example.com/images/albert.jpg"
    };

    private UserModel OtherUserModel = new UserModel() 
    { 
        ID = "User2", 
        User = "Michale Suyama",
        AvatarUrl = "https://example.com/images/michale.jpg"
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
            Author = OtherUserModel 
        }
    };
}
```

### Avatar Fallback to Initials

When no AvatarUrl is provided, the component generates initials from the user's name:

```csharp
private UserModel UserWithoutAvatar = new UserModel() 
{ 
    ID = "user3", 
    User = "John Doe"
    // No AvatarUrl - will display "JD" as initials
};

private UserModel UserWithAvatar = new UserModel() 
{ 
    ID = "user4", 
    User = "Jane Smith",
    AvatarUrl = "https://example.com/jane.jpg"
    // Will display the image
};
```

### Dynamic Avatar URLs

Update avatar URLs based on user data:

```csharp
private async Task LoadUserAvatars()
{
    // Fetch users from database
    var users = await FetchUsersFromDatabase();
    
    // Create UserModels with avatar URLs
    var chatUsers = users.Select(u => new UserModel
    {
        ID = u.Id,
        User = u.DisplayName,
        AvatarUrl = u.AvatarUrl ?? GenerateDefaultAvatar(u.DisplayName)
    }).ToList();
}

private string GenerateDefaultAvatar(string userName)
{
    // Generate URL for default avatar service
    return $"https://api.example.com/avatar?name={Uri.EscapeDataString(userName)}";
}
```

## Avatar Background Colors

The [AvatarBgColor](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.UserModel.html#Syncfusion_Blazor_InteractiveChat_UserModel_AvatarBgColor) property sets a specific background color for a user's avatar using hexadecimal values. This color is used when displaying fallback initials or to customize the avatar appearance.

### Setting Avatar Colors

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 600px;">
    <SfChatUI ID="chatUser" User="CurrentUserModel" Messages="ChatUserMessages"></SfChatUI>
</div>

@code {
    private UserModel CurrentUserModel = new UserModel() 
    { 
        ID = "User1", 
        User = "Albert",
        AvatarBgColor = "#4a90e2"  // Blue
    };

    private UserModel MichaleUserModel = new UserModel() 
    { 
        ID = "User2", 
        User = "Michale Suyama",
        AvatarBgColor = "#7cb342"  // Green
    };

    private UserModel ReenaUserModel = new UserModel() 
    { 
        ID = "User3", 
        User = "Reena",
        AvatarBgColor = "#e84c3d"  // Red
    };

    private List<ChatMessage> ChatUserMessages = new()
    {
        new ChatMessage() { Text = "Hi!", Author = CurrentUserModel },
        new ChatMessage() { Text = "Hello!", Author = MichaleUserModel },
        new ChatMessage() { Text = "Hey everyone!", Author = ReenaUserModel }
    };
}
```

### Color Assignment Strategies

Assign colors consistently across users:

```csharp
private Dictionary<string, string> UserColors = new()
{
    { "user1", "#4a90e2" },  // Blue
    { "user2", "#7cb342" },  // Green
    { "user3", "#e84c3d" },  // Red
    { "user4", "#f5a623" },  // Orange
    { "user5", "#9013fe" }   // Purple
};

private UserModel CreateUserWithColor(string id, string name)
{
    return new UserModel
    {
        ID = id,
        User = name,
        AvatarBgColor = UserColors.ContainsKey(id) 
            ? UserColors[id] 
            : GenerateRandomColor()
    };
}

private string GenerateRandomColor()
{
    var random = new Random();
    var colors = new[] 
    { 
        "#4a90e2", "#7cb342", "#e84c3d", 
        "#f5a623", "#9013fe", "#00bcd4" 
    };
    return colors[random.Next(colors.Length)];
}
```

## User Status Indicators

The [StatusIconCss](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.UserModel.html#Syncfusion_Blazor_InteractiveChat_UserModel_StatusIconCss) property allows you to display user presence status with predefined CSS classes.

### Status Icon Classes

The following predefined status styles are available:

| Status | Icon Class | Meaning |
|--------|-----------|---------|
| Available | `e-icons e-user-online` | User is online |
| Away | `e-icons e-user-away` | User is idle/away |
| Busy | `e-icons e-user-busy` | User is busy |
| Offline | `e-icons e-user-offline` | User is offline |

### Setting User Status

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 600px;">
    <SfChatUI ID="chatUser" User="CurrentUserModel" Messages="ChatUserMessages"></SfChatUI>
</div>

@code {
    private UserModel CurrentUserModel = new UserModel() 
    { 
        ID = "User1", 
        User = "Albert",
        StatusIconCss = "e-icons e-user-online"
    };

    private UserModel MichaleUserModel = new UserModel() 
    { 
        ID = "User2", 
        User = "Michale Suyama",
        StatusIconCss = "e-icons e-user-away"
    };

    private UserModel ReenaUserModel = new UserModel() 
    { 
        ID = "User3", 
        User = "Reena",
        StatusIconCss = "e-icons e-user-busy"
    };

    private List<ChatMessage> ChatUserMessages = new()
    {
        new ChatMessage() { Text = "Hi Michale, are we on track?", Author = CurrentUserModel },
        new ChatMessage() { Text = "Yes, the design phase is complete.", Author = MichaleUserModel },
        new ChatMessage() { Text = "Let me review it.", Author = ReenaUserModel }
    };
}
```

### Dynamic Status Updates

Update user status based on real-time information:

```csharp
private async Task UpdateUserStatus(string userId, string status)
{
    var user = Users.FirstOrDefault(u => u.ID == userId);
    if (user != null)
    {
        user.StatusIconCss = status switch
        {
            "online" => "e-icons e-user-online",
            "away" => "e-icons e-user-away",
            "busy" => "e-icons e-user-busy",
            "offline" => "e-icons e-user-offline",
            _ => "e-icons e-user-offline"
        };
        
        StateHasChanged();
    }
}

// Usage
private async Task UserStatusChangedListener()
{
    // Monitor user status changes
    while (true)
    {
        var statusUpdate = await GetStatusUpdateFromServer();
        await UpdateUserStatus(statusUpdate.UserId, statusUpdate.Status);
        await Task.Delay(5000);
    }
}
```

## Custom CSS Styling

The [CssClass](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.UserModel.html#Syncfusion_Blazor_InteractiveChat_UserModel_CssClass) property allows for custom styling of a user's avatar and message bubble.

### Applying Custom CSS Classes

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 600px;">
    <SfChatUI ID="chatUser" User="CurrentUserModel" Messages="ChatUserMessages"></SfChatUI>
</div>

<style>
    .e-chat-ui .e-message-icon.vip-user {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        border: 2px solid gold;
        box-shadow: 0 0 10px rgba(0,0,0,0.3);
    }

    .e-chat-ui .vip-user .e-message-box {
        background-color: #fff9e6;
        border-left: 4px solid gold;
    }
</style>

@code {
    private UserModel VIPUser = new UserModel() 
    { 
        ID = "vip1", 
        User = "Premium Member",
        CssClass = "vip-user"
    };

    private UserModel RegularUser = new UserModel() 
    { 
        ID = "user1", 
        User = "Regular User"
    };

    private List<ChatMessage> ChatUserMessages = new()
    {
        new ChatMessage() { Text = "I'm a VIP member!", Author = VIPUser },
        new ChatMessage() { Text = "Nice to meet you!", Author = RegularUser }
    };
}
```

### Theme-Specific Styling

Create role-based styling:

```csharp
<style>
    /* Admin users */
    .e-chat-ui .admin-user .e-message-icon {
        background-color: #d32f2f;
        color: white;
        font-weight: bold;
    }

    /* Moderator users */
    .e-chat-ui .moderator-user .e-message-icon {
        background-color: #388e3c;
        color: white;
    }

    /* Support agent */
    .e-chat-ui .support-user .e-message-icon {
        background-color: #1976d2;
        color: white;
    }
</style>

@code {
    private UserModel AdminUser = new UserModel() 
    { 
        ID = "admin1", 
        User = "System Admin",
        CssClass = "admin-user"
    };

    private UserModel ModeratorUser = new UserModel() 
    { 
        ID = "mod1", 
        User = "Moderator",
        CssClass = "moderator-user"
    };

    private UserModel SupportAgent = new UserModel() 
    { 
        ID = "support1", 
        User = "Support Agent",
        CssClass = "support-user"
    };
}
```

## User Display Configuration

### Complete User Profile Setup

Create users with all profile information:

```csharp
private UserModel CreateFullProfile(string id, string name, string email, string role)
{
    return new UserModel
    {
        ID = id,
        User = name,
        AvatarUrl = $"https://api.example.com/avatar/{id}",
        AvatarBgColor = GetColorForRole(role),
        StatusIconCss = GetStatusIcon(role),
        CssClass = GetCssClassForRole(role)
    };
}

private string GetColorForRole(string role)
{
    return role switch
    {
        "admin" => "#d32f2f",
        "moderator" => "#388e3c",
        "support" => "#1976d2",
        _ => "#4a90e2"
    };
}

private string GetStatusIcon(string role)
{
    return role switch
    {
        "admin" => "e-icons e-user-online",
        _ => "e-icons e-user-away"
    };
}

private string GetCssClassForRole(string role)
{
    return role switch
    {
        "admin" => "admin-user",
        "moderator" => "moderator-user",
        "support" => "support-user",
        _ => ""
    };
}
```

## Best Practices

1. **Consistent Avatar Sources** - Use a single CDN or service for avatar images
2. **Color Consistency** - Assign same colors to same user roles
3. **Status Updates** - Update status regularly for real-time presence
4. **Image Optimization** - Use appropriately sized and compressed images
5. **Fallback Initials** - Ensure initials are readable on background colors
6. **Accessibility** - Ensure sufficient color contrast for status indicators
7. **Loading States** - Handle missing or slow-loading avatar images
