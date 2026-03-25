# Advanced Features & Properties

## Table of Contents
- [Auto-Scroll Configuration](#auto-scroll-configuration)
- [Compact Mode](#compact-mode)
- [Header & Footer Customization](#header--footer-customization)
- [Component Sizing](#component-sizing)
- [Mention System](#mention-system)
- [Quick Reply Suggestions](#quick-reply-suggestions)
- [Load on Demand](#load-on-demand)
- [Right-to-Left (RTL) Support](#right-to-left-rtl-support)
- [Custom Styling](#custom-styling)

---

## Auto-Scroll Configuration

### AutoScrollToBottom Property

**Type:** `bool`  
**Default:** `false`

Automatically scrolls the chat to the bottom when new messages are received.

### Basic Usage

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    AutoScrollToBottom="true">
</SfChatUI>
```

### Pattern: Conditional Auto-Scroll

```csharp
<label>
    <input type="checkbox" @bind="enableAutoScroll" />
    Auto-scroll enabled
</label>

<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    AutoScrollToBottom="@enableAutoScroll">
</SfChatUI>

@code {
    private bool enableAutoScroll = true;
}
```

### Use Cases
- Real-time chat applications
- Live support conversations
- Streaming message feeds
- Bot conversations with auto-responses

---

## Compact Mode

### EnableCompactMode Property

**Type:** `bool`  
**Default:** `false`

When enabled, displays all messages aligned to the left side, regardless of author.

### Basic Usage

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    EnableCompactMode="true">
</SfChatUI>
```

### Comparison

**Normal Mode (Default):**
- Current user messages: right-aligned
- Other user messages: left-aligned

**Compact Mode:**
- All messages: left-aligned
- User identification via avatar and name

### Pattern: Switchable View Mode

```csharp
<div class="view-toggle">
    <button @onclick="@(() => compactMode = false)">Standard View</button>
    <button @onclick="@(() => compactMode = true)">Compact View</button>
</div>

<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    EnableCompactMode="@compactMode">
</SfChatUI>

@code {
    private bool compactMode = false;
}
```

### Use Cases
- Group chat interfaces
- Support ticket history
- Forum-style conversations
- Multi-user team chats
- Message threads and replies

---

## Header & Footer Customization

### Header Properties

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `ShowHeader` | bool | true | Show/hide header bar |
| `HeaderText` | string | "Chat" | Header title text |
| `HeaderIconCss` | string | Empty | CSS class for header icon |

### Footer Properties

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `ShowFooter` | bool | true | Show/hide footer input area |
| `FooterTemplate` | RenderFragment | null | Custom footer content |

### Pattern 1: Custom Header

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    ShowHeader="true"
    HeaderText="Support Chat"
    HeaderIconCss="e-icons e-comment-show">
</SfChatUI>
```

### Pattern 2: Hidden Header

```csharp
<!-- Embedded chat widget without header -->
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    ShowHeader="false">
</SfChatUI>
```

### Pattern 3: Custom Footer Template

```csharp
<SfChatUI ID="chat" User="CurrentUser" Messages="Messages">
    <FooterTemplate>
        <div class="custom-footer">
            <button @onclick="AttachFile">📎 Attach</button>
            <input type="text" placeholder="Type message..." />
            <button @onclick="SendMessage">Send</button>
        </div>
    </FooterTemplate>
</SfChatUI>

@code {
    private void AttachFile()
    {
        // Custom file attachment logic
    }
    
    private void SendMessage()
    {
        // Custom send logic
    }
}
```

### Pattern 4: Dynamic Header Based on Context

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    HeaderText="@GetHeaderText()"
    HeaderIconCss="@GetHeaderIcon()">
</SfChatUI>

@code {
    private string conversationType = "support"; // "support", "team", "personal"
    
    private string GetHeaderText()
    {
        return conversationType switch
        {
            "support" => "Customer Support",
            "team" => "Team Discussion",
            "personal" => "Direct Message",
            _ => "Chat"
        };
    }
    
    private string GetHeaderIcon()
    {
        return conversationType switch
        {
            "support" => "e-icons e-help",
            "team" => "e-icons e-people",
            "personal" => "e-icons e-comment",
            _ => ""
        };
    }
}
```

---

## Component Sizing

### Height and Width Properties

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `Height` | string | "100%" | Component height (CSS value) |
| `Width` | string | "100%" | Component width (CSS value) |

### Pattern 1: Fixed Dimensions

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    Height="500px"
    Width="400px">
</SfChatUI>
```

### Pattern 2: Responsive Sizing

```csharp
<!-- Full viewport height -->
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    Height="100vh"
    Width="100%">
</SfChatUI>
```

### Pattern 3: Container-Based Sizing

```csharp
<div style="display: flex; height: 600px; width: 800px;">
    <SfChatUI 
        ID="chat" 
        User="CurrentUser" 
        Messages="Messages"
        Height="100%"
        Width="100%">
    </SfChatUI>
</div>
```

### Pattern 4: Mobile-Responsive

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    Height="@chatHeight"
    Width="@chatWidth">
</SfChatUI>

@code {
    private string chatHeight => IsMobile ? "100vh" : "600px";
    private string chatWidth => IsMobile ? "100%" : "400px";
    
    private bool IsMobile => /* detect mobile device */;
}
```

---

## Mention System

### Mention Properties

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `MentionChar` | char | '@' | Character triggering mention popup |
| `MentionUsers` | List\<UserModel\> | Empty | Users available for mention |
| `ValueSelecting` | EventCallback | null | Event when mention selected |

### Basic Mention Setup

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    MentionChar="@"
    MentionUsers="@teamMembers"
    ValueSelecting="@OnMentionSelected">
</SfChatUI>

@code {
    private List<UserModel> teamMembers = new()
    {
        new UserModel { ID = "user1", User = "Albert" },
        new UserModel { ID = "user2", User = "Michale Suyama" },
        new UserModel { ID = "user3", User = "Reena" },
        new UserModel { ID = "user4", User = "Laura Callahan" }
    };
    
    private void OnMentionSelected(MentionValueSelectingEventArgs<UserModel> args)
    {
        Console.WriteLine($"Mentioned user: {args.ItemData.User}");
        // Send notification to mentioned user
    }
}
```

### Pattern 1: Custom Mention Character

```csharp
<!-- Use # for hashtags instead of @ for mentions -->
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    MentionChar="#"
    MentionUsers="@channels">
</SfChatUI>

@code {
    private List<UserModel> channels = new()
    {
        new UserModel { ID = "general", User = "general" },
        new UserModel { ID = "support", User = "support" },
        new UserModel { ID = "dev", User = "dev-team" }
    };
}
```

### Pattern 2: Dynamic Mention List

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    MentionChar="@"
    MentionUsers="@GetAvailableUsers()"
    ValueSelecting="@OnMentionSelected">
</SfChatUI>

@code {
    private List<UserModel> allUsers = new();
    private List<UserModel> onlineUsers = new();
    
    private List<UserModel> GetAvailableUsers()
    {
        // Only show online users in mention list
        return onlineUsers.Where(u => u.ID != CurrentUser.ID).ToList();
    }
    
    private async Task OnMentionSelected(MentionValueSelectingEventArgs<UserModel> args)
    {
        // Send real-time notification
        await NotificationService.NotifyUser(args.ItemData.ID, "You were mentioned");
    }
}
```

### Pattern 3: Mention with Notification

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    MentionChar="@"
    MentionUsers="@teamMembers"
    ValueSelecting="@OnMentionSelected"
    MessageSend="@OnMessageSend">
</SfChatUI>

@code {
    private List<string> mentionedUsers = new();
    
    private void OnMentionSelected(MentionValueSelectingEventArgs<UserModel> args)
    {
        mentionedUsers.Add(args.ItemData.ID);
    }
    
    private async Task OnMessageSend(ChatMessageSendEventArgs args)
    {
        // Send message
        Messages.Add(args.Message);
        
        // Notify all mentioned users
        foreach (var userId in mentionedUsers)
        {
            await SendNotification(userId, args.Message.Text);
        }
        
        mentionedUsers.Clear();
        StateHasChanged();
    }
}
```

---

## Quick Reply Suggestions

### Suggestion Properties

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `Suggestions` | List\<string\> | Empty | Quick reply suggestion texts |
| `SuggestionTemplate` | RenderFragment | null | Custom suggestion rendering |

### Basic Suggestions

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    Suggestions="@quickReplies">
</SfChatUI>

@code {
    private List<string> quickReplies = new()
    {
        "Yes, please",
        "No, thank you",
        "Tell me more",
        "I need help"
    };
}
```

### Pattern 1: Dynamic Suggestions

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    Suggestions="@GetContextualSuggestions()">
</SfChatUI>

@code {
    private string lastBotQuestion = "";
    
    private List<string> GetContextualSuggestions()
    {
        return lastBotQuestion.ToLower() switch
        {
            var q when q.Contains("help") => new() { "Technical Support", "Billing", "Account" },
            var q when q.Contains("payment") => new() { "Credit Card", "PayPal", "Bank Transfer" },
            var q when q.Contains("shipping") => new() { "Standard", "Express", "Overnight" },
            _ => new() { "Yes", "No", "Maybe" }
        };
    }
}
```

### Pattern 2: Custom Suggestion Template

```csharp
<SfChatUI ID="chat" User="CurrentUser" Messages="Messages" Suggestions="@suggestions">
    <SuggestionTemplate>
        <div class="custom-suggestion">
            <span class="suggestion-icon">💡</span>
            <span class="suggestion-text">@context</span>
        </div>
    </SuggestionTemplate>
</SfChatUI>

@code {
    private List<string> suggestions = new() { "Option 1", "Option 2", "Option 3" };
}
```

### Pattern 3: Suggestion with Actions

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    Suggestions="@suggestionTexts"
    MessageSend="@OnMessageSend">
</SfChatUI>

@code {
    private List<string> suggestionTexts = new();
    
    private void ShowSuggestions(List<string> options)
    {
        suggestionTexts = options;
        StateHasChanged();
    }
    
    private async Task OnMessageSend(ChatMessageSendEventArgs args)
    {
        // User selected a suggestion
        if (suggestionTexts.Contains(args.Message.Text))
        {
            // Clear suggestions after selection
            suggestionTexts.Clear();
        }
        
        Messages.Add(args.Message);
        
        // Show new suggestions based on response
        await ProcessUserResponse(args.Message.Text);
    }
}
```

---

## Load on Demand

### LoadOnDemand Property

**Type:** `bool`  
**Default:** `false`

Enables on-demand loading of chat messages for better performance with large message histories.

### Basic Usage

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    LoadOnDemand="true">
</SfChatUI>
```

### Pattern: Lazy Loading Messages

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    LoadOnDemand="true">
</SfChatUI>

<button @onclick="LoadMoreMessages">Load Older Messages</button>

@code {
    private List<ChatMessage> Messages = new();
    private int currentPage = 0;
    private const int pageSize = 50;
    
    protected override async Task OnInitializedAsync()
    {
        await LoadMoreMessages();
    }
    
    private async Task LoadMoreMessages()
    {
        var olderMessages = await FetchMessagesFromDatabase(currentPage, pageSize);
        Messages.InsertRange(0, olderMessages);
        currentPage++;
        StateHasChanged();
    }
    
    private async Task<List<ChatMessage>> FetchMessagesFromDatabase(int page, int size)
    {
        // Database query with pagination
        await Task.Delay(500); // Simulate database call
        return new List<ChatMessage>(); // Return fetched messages
    }
}
```

---

## Right-to-Left (RTL) Support

### EnableRtl Property

**Type:** `bool`  
**Default:** `false`

Enables right-to-left text direction for languages like Arabic, Hebrew, Persian, etc.

### Basic Usage

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    EnableRtl="true">
</SfChatUI>
```

### Pattern: Language-Based RTL

```csharp
<select @bind="selectedLanguage" @bind:after="UpdateRtl">
    <option value="en">English</option>
    <option value="ar">Arabic</option>
    <option value="he">Hebrew</option>
</select>

<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    EnableRtl="@isRtl">
</SfChatUI>

@code {
    private string selectedLanguage = "en";
    private bool isRtl = false;
    
    private void UpdateRtl()
    {
        isRtl = selectedLanguage is "ar" or "he" or "fa" or "ur";
    }
}
```

---

## Custom Styling

### CssClass Property

**Type:** `string`  
**Default:** `String.Empty`

Applies custom CSS classes to the Chat UI component for styling customization.

### Basic Usage

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    CssClass="custom-chat-theme">
</SfChatUI>

<style>
    .custom-chat-theme {
        --chat-primary-color: #4a90e2;
        --chat-bg-color: #f5f5f5;
    }
</style>
```

### Pattern 1: Theme Switching

```csharp
<select @bind="selectedTheme">
    <option value="light-theme">Light</option>
    <option value="dark-theme">Dark</option>
    <option value="blue-theme">Blue</option>
</select>

<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    CssClass="@selectedTheme">
</SfChatUI>

@code {
    private string selectedTheme = "light-theme";
}
```

### Pattern 2: Branded Chat

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    CssClass="company-branded-chat">
</SfChatUI>

<style>
    .company-branded-chat {
        font-family: 'Company Font', sans-serif;
        border: 2px solid #company-color;
        border-radius: 12px;
    }
    
    .company-branded-chat .e-chat-message {
        background-color: #brand-light;
    }
</style>
```

---

## Best Practices

1. **Performance** - Use `LoadOnDemand` for chats with 100+ messages
2. **Accessibility** - Enable RTL for appropriate languages
3. **UX** - Use `AutoScrollToBottom` for real-time conversations
4. **Mobile** - Use responsive `Height` and `Width` values
5. **Mentions** - Provide clear feedback when users are mentioned
6. **Suggestions** - Keep suggestion lists short (3-5 options)
7. **Compact Mode** - Use for multi-user or group conversations
8. **Styling** - Use `CssClass` for consistent branding
