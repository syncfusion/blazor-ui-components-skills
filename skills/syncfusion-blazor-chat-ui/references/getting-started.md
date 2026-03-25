# Getting Started with Chat UI

## Installation

### NuGet Package Setup

Install the Syncfusion.Blazor.InteractiveChat NuGet package:

```bash
dotnet add package Syncfusion.Blazor.InteractiveChat
```

### Import Namespaces

Add the required using statements to your `_Imports.razor` file:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.InteractiveChat
```

### Register Service

In your `Program.cs`, add the Syncfusion Blazor service registration:

```csharp
// Program.cs
builder.Services.AddSyncfusionBlazor();
```

### Add Theme

Include the Syncfusion Blazor theme stylesheet in your `App.razor` or layout file:

```html
<!-- Bootstrap 5 theme (or choose another: material, fluent, tailwind) -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Syncfusion Blazor script -->
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

Available themes: `bootstrap5.css`, `material.css`, `fluent.css`, `tailwind.css`, `fabric.css`, `material-dark.css`

## Basic Component Structure

### Minimal Example

Create a simple chat UI with two users:

```csharp
@page "/chat"
@using Syncfusion.Blazor.InteractiveChat

<h2>Chat Application</h2>

<div style="height: 500px; width: 600px;">
    <SfChatUI ID="chatUser" User="CurrentUserModel" Messages="ChatMessages"></SfChatUI>
</div>

@code {
    private UserModel CurrentUserModel = new UserModel 
    { 
        ID = "User1", 
        User = "Albert" 
    };

    private UserModel OtherUserModel = new UserModel 
    { 
        ID = "User2", 
        User = "Michale Suyama" 
    };

    private List<ChatMessage> ChatMessages = new()
    {
        new ChatMessage { 
            Text = "Hi, thinking of painting this weekend.", 
            Author = new UserModel { ID = "User1", User = "Albert" }
        },
        new ChatMessage { 
            Text = "That's fun! What will you paint?", 
            Author = new UserModel { ID = "User2", User = "Michale Suyama" }
        },
        new ChatMessage { 
            Text = "Maybe landscapes.", 
            Author = new UserModel { ID = "User1", User = "Albert" }
        }
    };
}
```

## Component Properties

The SfChatUI component has several key properties for configuration:

- **ID:** Unique component identifier
- **User:** Current logged-in user (UserModel)
- **Messages:** Collection of ChatMessage objects
- **ShowTimestamp:** Display timestamps on messages
- **TypingUsers:** List of users currently typing
- **ShowTimeBreak:** Display date separators between messages

## UserModel Setup

Define users with complete information:

```csharp
private UserModel CreateUser(string id, string name)
{
    return new UserModel 
    { 
        ID = id, 
        User = name,
        AvatarBgColor = "#4a90e2",
        StatusIconCss = "e-icons e-user-online"
    };
}
```

**UserModel Properties:**
- **ID:** Unique user identifier
- **User:** Display name
- **AvatarUrl:** Image URL for avatar
- **AvatarBgColor:** Background color for avatar
- **StatusIconCss:** CSS class for status indicator
- **CssClass:** Custom styling class

## ChatMessage Setup

Create messages with full context:

```csharp
private ChatMessage CreateMessage(string text, UserModel author)
{
    return new ChatMessage 
    { 
        Text = text,
        Author = author,
        Timestamp = DateTime.Now
    };
}
```

**ChatMessage Properties:**
- **Text:** Message content (supports HTML)
- **Author:** UserModel who sent the message
- **Timestamp:** Date/time the message was sent
- **ID:** Unique message identifier (auto-generated)

## Container Styling

Set appropriate dimensions for the component container:

```html
<!-- Fixed height with scroll -->
<div style="height: 500px; width: 100%; overflow-y: auto;">
    <SfChatUI ID="chatUser" User="CurrentUser" Messages="Messages"></SfChatUI>
</div>

<!-- Flex layout for responsive sizing -->
<div style="display: flex; flex-direction: column; height: 100vh;">
    <div style="flex: 1; overflow-y: auto;">
        <SfChatUI ID="chatUser" User="CurrentUser" Messages="Messages"></SfChatUI>
    </div>
</div>
```

## Basic Configuration

A complete minimal setup with common properties:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 500px; width: 600px;">
    <SfChatUI 
        ID="myChat"
        User="CurrentUser" 
        Messages="ChatMessages"
        ShowTimestamp="true"
        TimestampFormat="hh:mm tt">
    </SfChatUI>
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
        User = "Michale",
        AvatarBgColor = "#7cb342"
    };

    private List<ChatMessage> ChatMessages = new()
    {
        new ChatMessage 
        { 
            Text = "Hello! Welcome to the chat.", 
            Author = CurrentUser,
            Timestamp = DateTime.Now.AddHours(-2)
        },
        new ChatMessage 
        { 
            Text = "Thanks! Glad to be here.", 
            Author = OtherUser,
            Timestamp = DateTime.Now.AddHours(-1)
        },
        new ChatMessage 
        { 
            Text = "Let's get started then!", 
            Author = CurrentUser,
            Timestamp = DateTime.Now
        }
    };
}
```

## Managing Messages Dynamically

Add new messages to the conversation:

```csharp
private async Task SendMessage(string text)
{
    var newMessage = new ChatMessage
    {
        Text = text,
        Author = CurrentUser,
        Timestamp = DateTime.Now
    };

    ChatMessages.Add(newMessage);
    StateHasChanged();
    
    // Simulate other user response
    await Task.Delay(1000);
    
    var response = new ChatMessage
    {
        Text = "Thanks for your message!",
        Author = OtherUser,
        Timestamp = DateTime.Now
    };
    
    ChatMessages.Add(response);
    StateHasChanged();
}
```

## Next Steps

- **Message Management:** Configure and manage message content
- **User Profiles:** Set up avatars and user information
- **Typing Indicators:** Show when users are typing
- **Timestamps:** Configure time and date formatting
- **Templates:** Customize message appearance
- **Attachments:** Enable file sharing
- **Events:** Handle user interactions
