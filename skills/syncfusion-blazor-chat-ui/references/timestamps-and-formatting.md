# Timestamps & Formatting

## Table of Contents
- [Show/Hide Timestamps](#showhide-timestamps)
- [Timestamp Configuration](#timestamp-configuration)
- [Timestamp Format Customization](#timestamp-format-customization)
- [TimeBreak Separators](#timebreak-separators)
- [TimeBreak Templates](#timebreak-templates)

## Show/Hide Timestamps

The [ShowTimestamp](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_ShowTimestamp) property enables or disables timestamps for all messages. By default, this is set to `true`, displaying the exact date and time when messages were sent.

### Enable Timestamps

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 600px;">
    <SfChatUI 
        ID="chatUser" 
        User="CurrentUserModel" 
        ShowTimestamp="true"
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

    private List<ChatMessage> ChatUserMessages = new()
    {
        new ChatMessage() 
        { 
            Text = "Hi, thinking of painting this weekend.", 
            Author = CurrentUserModel,
            Timestamp = new DateTime(2024, 12, 25, 7, 30, 0)
        },
        new ChatMessage() 
        { 
            Text = "That's fun! What will you paint?", 
            Author = MichaleUserModel,
            Timestamp = new DateTime(2024, 12, 25, 8, 0, 0)
        },
        new ChatMessage() 
        { 
            Text = "Maybe landscapes.", 
            Author = CurrentUserModel,
            Timestamp = new DateTime(2024, 12, 25, 11, 0, 0)
        }
    };
}
```

### Disable Timestamps

Hide timestamps for a cleaner interface:

```csharp
<SfChatUI 
    ID="chatUser" 
    User="CurrentUserModel" 
    ShowTimestamp="false"
    Messages="ChatUserMessages">
</SfChatUI>
```

## Timestamp Configuration

The [Timestamp](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.ChatMessage.html#Syncfusion_Blazor_InteractiveChat_ChatMessage_Timestamp) property on each message specifies the date and time it was sent. By default, it is set to the current date and time when the message is created.

### Setting Message Timestamps

```csharp
// Current time (default)
var message1 = new ChatMessage()
{
    Text = "Just sent",
    Author = CurrentUser
    // Timestamp defaults to DateTime.Now
};

// Specific past time
var message2 = new ChatMessage()
{
    Text = "Sent earlier today",
    Author = OtherUser,
    Timestamp = new DateTime(2024, 12, 25, 7, 30, 0)
};

// Custom timestamp
var message3 = new ChatMessage()
{
    Text = "Old conversation",
    Author = CurrentUser,
    Timestamp = DateTime.Parse("2024-12-20 14:30:00")
};
```

### Managing Timestamps During Message Creation

```csharp
private ChatMessage CreateMessage(string text, UserModel author, DateTime? timestamp = null)
{
    return new ChatMessage
    {
        Text = text,
        Author = author,
        Timestamp = timestamp ?? DateTime.Now
    };
}

// Usage
var messages = new List<ChatMessage>()
{
    CreateMessage("First message", User1),  // Uses current time
    CreateMessage("Second message", User2, new DateTime(2024, 12, 25, 10, 0, 0)),
    CreateMessage("Third message", User1)   // Uses current time
};
```

## Timestamp Format Customization

The [TimestampFormat](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_TimestampFormat) property displays time formats for all messages. The default value is `dd/MM/yyyy hh:mm tt`.

### Common Format Examples

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 600px;">
    <SfChatUI 
        ID="chatUser" 
        User="CurrentUserModel" 
        TimestampFormat="@timestampFormat"
        Messages="ChatUserMessages">
    </SfChatUI>
</div>

@code {
    private string timestampFormat = "MMMM hh:mm tt";  // December 2:30 PM
    
    private UserModel CurrentUserModel = new UserModel() 
    { 
        ID = "User1", 
        User = "Albert" 
    };

    private List<ChatMessage> ChatUserMessages = new();
}
```

### Format String Options

| Format | Output | Example |
|--------|--------|---------|
| `dd/MM/yyyy hh:mm tt` | Full date with time | 25/12/2024 02:30 PM |
| `hh:mm tt` | Time only | 02:30 PM |
| `dd/MM/yyyy HH:mm` | Full date, 24-hour time | 25/12/2024 14:30 |
| `MMMM hh:mm tt` | Month name with time | December 02:30 PM |
| `ddd, MMMM dd` | Day and date | Tue, December 25 |
| `h:mm tt` | Short time | 2:30 PM |
| `yyyy-MM-dd HH:mm:ss` | ISO format | 2024-12-25 14:30:00 |

### Format Examples

```csharp
// Show only time
<SfChatUI 
    ID="chat1" 
    User="CurrentUser" 
    TimestampFormat="hh:mm tt"
    Messages="Messages">
</SfChatUI>

// Show month and time
<SfChatUI 
    ID="chat2" 
    User="CurrentUser" 
    TimestampFormat="MMMM hh:mm tt"
    Messages="Messages">
</SfChatUI>

// Show full date and time
<SfChatUI 
    ID="chat3" 
    User="CurrentUser" 
    TimestampFormat="dddd, MMMM dd, yyyy h:mm tt"
    Messages="Messages">
</SfChatUI>
```

## TimeBreak Separators

The [ShowTimeBreak](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_ShowTimeBreak) property enables date separators between message groups. When enabled, messages are visually grouped by date with separators showing "Today", "Yesterday", or specific dates.

### Enable TimeBreak

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 600px;">
    <SfChatUI 
        ID="chatUser" 
        User="CurrentUserModel" 
        ShowTimeBreak="true"
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

    private List<ChatMessage> ChatUserMessages = new()
    {
        // Yesterday's messages
        new ChatMessage() 
        { 
            Text = "Good day yesterday!", 
            Author = MichaleUserModel,
            Timestamp = DateTime.Now.AddDays(-1).Date
        },
        
        // Today's messages
        new ChatMessage() 
        { 
            Text = "Hi, thinking of painting this weekend.", 
            Author = CurrentUserModel,
            Timestamp = DateTime.Now.Date.AddHours(7).AddMinutes(30)
        },
        new ChatMessage() 
        { 
            Text = "That's fun! What will you paint?", 
            Author = MichaleUserModel,
            Timestamp = DateTime.Now.Date.AddHours(8)
        },
        new ChatMessage() 
        { 
            Text = "Maybe landscapes.", 
            Author = CurrentUserModel,
            Timestamp = DateTime.Now.Date.AddHours(11)
        }
    };
}
```

## TimeBreak Templates

Customize the appearance of date separators using the [TimeBreakTemplate](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_TimeBreakTemplate).

### Basic TimeBreak Template

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div class="template-chatui" style="height: 400px; width: 600px;">
    <SfChatUI 
        ID="chatUser" 
        User="CurrentUserModel" 
        Messages="ChatUserMessages" 
        ShowTimeBreak="true">
        <TimeBreakTemplate>
            <div class="timebreak-wrapper">
                @(context.MessageDate.Value.ToString("MMMM dd, yyyy"))
            </div>
        </TimeBreakTemplate>
    </SfChatUI>
</div>

<style>
    .template-chatui .timebreak-wrapper {
        background-color: #6495ed;
        color: #ffffff;
        border-radius: 5px;
        padding: 2px;
        text-align: center;
        margin: 8px 0;
    }
</style>

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
            Author = CurrentUserModel, 
            Timestamp = new DateTime(2024, 12, 25, 7, 30, 0) 
        },
        new ChatMessage() 
        { 
            Text = "That's fun! What will you paint?", 
            Author = MichaleUserModel, 
            Timestamp = new DateTime(2024, 12, 25, 8, 0, 0) 
        },
        new ChatMessage() 
        { 
            Text = "Maybe landscapes.", 
            Author = CurrentUserModel, 
            Timestamp = new DateTime(2024, 12, 25, 11, 0, 0) 
        }
    };
}
```

### Advanced TimeBreak with Relative Dates

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages" 
    ShowTimeBreak="true">
    <TimeBreakTemplate>
        <div class="advanced-timebreak">
            <span class="date-label">@GetRelativeDate(context.MessageDate.Value)</span>
        </div>
    </TimeBreakTemplate>
</SfChatUI>

<style>
    .advanced-timebreak {
        display: flex;
        align-items: center;
        gap: 8px;
        margin: 12px 0;
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
        padding: 0 8px;
        color: #666;
        font-size: 12px;
        font-weight: 500;
    }
</style>

@code {
    private string GetRelativeDate(DateTime messageDate)
    {
        var today = DateTime.Today;
        var yesterday = today.AddDays(-1);
        var messageDay = messageDate.Date;

        if (messageDay == today)
            return "Today";
        else if (messageDay == yesterday)
            return "Yesterday";
        else if (messageDay > today.AddDays(-7))
            return messageDay.ToString("dddd");
        else
            return messageDay.ToString("MMMM dd");
    }
}
```

## Best Practices

1. **Consistent Format** - Use same timestamp format across the chat
2. **Timezone Handling** - Consider user timezone when displaying timestamps
3. **Relative Dates** - Use "Today", "Yesterday" for better UX
4. **Performance** - Limit TimeBreak calculations for large message lists
5. **Accessibility** - Ensure timestamp text is readable with sufficient contrast
6. **Mobile Optimization** - Consider space constraints on mobile devices
7. **Internationalization** - Format dates based on user's culture/locale
