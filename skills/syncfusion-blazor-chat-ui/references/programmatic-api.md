# Programmatic API & Methods

## Table of Contents
- [Component Reference](#component-reference)
- [ScrollToBottomAsync](#scrolltobottomasync)
- [ScrollToMessageAsync](#scrolltomessageasync)
- [UpdateMessageAsync](#updatemessageasync)
- [FocusAsync](#focusasync)

## Component Reference

To use programmatic methods, you need a reference to the SfChatUI component:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<SfChatUI ID="chat" User="CurrentUser" Messages="Messages"></SfChatUI>

@code {
    
    private UserModel CurrentUser = new UserModel 
    { 
        ID = "user1", 
        User = "Albert" 
    };
    
    private List<ChatMessage> Messages = new();
}
```

---

## ScrollToBottomAsync

**Method Signature:** `Task ScrollToBottomAsync()`

Asynchronously scrolls the chat UI to the bottom, showing the latest messages.

### Use Cases
- Auto-scroll when new messages arrive
- "Jump to latest" button functionality
- Reset scroll position after loading history
- Scroll after programmatically adding messages

### Basic Usage

```csharp
<SfChatUI @ref="chatRef" ID="chat" User="CurrentUser" Messages="Messages"></SfChatUI>

<button @onclick="JumpToLatest">Jump to Latest Messages</button>

@code {
    private SfChatUI chatRef;
    
    private async Task JumpToLatest()
    {
        await chatRef.ScrollToBottomAsync();
    }
}
```

### Pattern 1: Auto-Scroll on New Message

```csharp
<SfChatUI 
    @ref="chatRef" 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    MessageSend="@OnMessageSend">
</SfChatUI>

@code {
    private SfChatUI chatRef;
    private List<ChatMessage> Messages = new();
    
    private async Task OnMessageSend(ChatMessageSendEventArgs args)
    {
        // Add message to collection
        Messages.Add(args.Message);
        
        // Auto-scroll to show the new message
        await chatRef.ScrollToBottomAsync();
        StateHasChanged();
    }
}
```

### Pattern 2: Scroll After Loading History

```csharp
<button @onclick="LoadMessages">Load Messages</button>
<SfChatUI @ref="chatRef" ID="chat" User="CurrentUser" Messages="Messages"></SfChatUI>

@code {
    private SfChatUI chatRef;
    private List<ChatMessage> Messages = new();
    
    private async Task LoadMessages()
    {
        // Load messages from database
        Messages = await LoadMessagesFromDatabase();
        StateHasChanged();
        
        // Scroll to bottom to show latest
        await Task.Delay(100); // Allow rendering
        await chatRef.ScrollToBottomAsync();
    }
    
    private async Task<List<ChatMessage>> LoadMessagesFromDatabase()
    {
        // Database query simulation
        await Task.Delay(500);
        return new List<ChatMessage>
        {
            new ChatMessage { Text = "Message 1", Author = CurrentUser },
            new ChatMessage { Text = "Message 2", Author = CurrentUser }
        };
    }
}
```

### Pattern 3: Conditional Auto-Scroll

```csharp
<SfChatUI @ref="chatRef" ID="chat" User="CurrentUser" Messages="Messages"></SfChatUI>

@code {
    private SfChatUI chatRef;
    private bool autoScrollEnabled = true;
    
    private async Task AddNewMessage(ChatMessage message)
    {
        Messages.Add(message);
        StateHasChanged();
        
        // Only auto-scroll if enabled
        if (autoScrollEnabled)
        {
            await chatRef.ScrollToBottomAsync();
        }
    }
}
```

---

## ScrollToMessageAsync

**Method Signature:** `Task ScrollToMessageAsync(string messageId)`

Asynchronously scrolls to a specific message by its ID.

### Use Cases
- Navigate to search results
- Jump to mentioned message
- Highlight specific conversation point
- Navigate to quoted/replied message

### Basic Usage

```csharp
<SfChatUI @ref="chatRef" ID="chat" User="CurrentUser" Messages="Messages"></SfChatUI>

<button @onclick="@(() => GoToMessage("msg-123"))">Go to Message</button>

@code {
    private SfChatUI chatRef;
    
    private async Task GoToMessage(string messageId)
    {
        await chatRef.ScrollToMessageAsync(messageId);
    }
}
```

### Pattern 1: Search and Navigate

```csharp
<input @bind="searchText" placeholder="Search messages" />
<button @onclick="SearchAndNavigate">Search</button>

<SfChatUI @ref="chatRef" ID="chat" User="CurrentUser" Messages="Messages"></SfChatUI>

@code {
    private SfChatUI chatRef;
    private string searchText = "";
    private List<ChatMessage> Messages = new();
    
    private async Task SearchAndNavigate()
    {
        // Find first matching message
        var foundMessage = Messages.FirstOrDefault(m => 
            m.Text.Contains(searchText, StringComparison.OrdinalIgnoreCase));
        
        if (foundMessage != null)
        {
            await chatRef.ScrollToMessageAsync(foundMessage.ID);
        }
        else
        {
            Console.WriteLine("Message not found");
        }
    }
}
```

### Pattern 2: Navigate to Quoted Message

```csharp
<SfChatUI @ref="chatRef" ID="chat" User="CurrentUser" Messages="Messages">
    <MessageTemplate>
        <div>
            @if (!string.IsNullOrEmpty(context.Message.QuotedMessageId))
            {
                <div class="quoted-message" @onclick="@(() => NavigateToQuoted(context.Message.QuotedMessageId))">
                    <small>View quoted message</small>
                </div>
            }
            <div>@context.Message.Text</div>
        </div>
    </MessageTemplate>
</SfChatUI>

@code {
    private SfChatUI chatRef;
    
    private async Task NavigateToQuoted(string quotedMessageId)
    {
        await chatRef.ScrollToMessageAsync(quotedMessageId);
    }
}
```

### Pattern 3: Navigate from Notification

```csharp
<SfChatUI @ref="chatRef" ID="chat" User="CurrentUser" Messages="Messages"></SfChatUI>

@code {
    private SfChatUI chatRef;
    
    protected override async Task OnInitializedAsync()
    {
        // Check if navigating from notification
        var messageId = NavigationManager.QueryString("messageId");
        
        if (!string.IsNullOrEmpty(messageId))
        {
            // Load messages first
            await LoadMessages();
            
            // Then scroll to specific message
            await Task.Delay(200); // Allow rendering
            await chatRef.ScrollToMessageAsync(messageId);
        }
    }
}
```

---

## UpdateMessageAsync

**Method Signature:** `Task UpdateMessageAsync(ChatMessage message, string msgId)`

Updates an existing message in the chat UI.

### Use Cases
- Edit sent messages
- Update message status (read/delivered)
- Correct typos or errors
- Update message content from backend

### Basic Usage

```csharp
<SfChatUI @ref="chatRef" ID="chat" User="CurrentUser" Messages="Messages"></SfChatUI>

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

### Pattern 1: Message Editing UI

```csharp
<SfChatUI @ref="chatRef" ID="chat" User="CurrentUser" Messages="Messages">
    <MessageTemplate>
        <div>
            <p>@context.Message.Text</p>
            @if (context.Message.Author.ID == CurrentUser.ID)
            {
                <button @onclick="@(() => StartEdit(context.Message))">Edit</button>
            }
        </div>
    </MessageTemplate>
</SfChatUI>

@if (editingMessage != null)
{
    <div class="edit-panel">
        <input @bind="editText" />
        <button @onclick="SaveEdit">Save</button>
        <button @onclick="CancelEdit">Cancel</button>
    </div>
}

@code {
    private SfChatUI chatRef;
    private ChatMessage editingMessage;
    private string editText;
    
    private void StartEdit(ChatMessage message)
    {
        editingMessage = message;
        editText = message.Text;
    }
    
    private async Task SaveEdit()
    {
        if (editingMessage != null)
        {
            editingMessage.Text = editText;
            await chatRef.UpdateMessageAsync(editingMessage, editingMessage.ID);
            
            editingMessage = null;
            editText = "";
        }
    }
    
    private void CancelEdit()
    {
        editingMessage = null;
        editText = "";
    }
}
```

### Pattern 2: Real-Time Message Updates

```csharp
<SfChatUI @ref="chatRef" ID="chat" User="CurrentUser" Messages="Messages"></SfChatUI>

@code {
    private SfChatUI chatRef;
    
    protected override async Task OnInitializedAsync()
    {
        // Subscribe to real-time updates
        await hubConnection.On<string, string>("MessageUpdated", async (messageId, newText) =>
        {
            var message = Messages.FirstOrDefault(m => m.ID == messageId);
            
            if (message != null)
            {
                message.Text = newText;
                await chatRef.UpdateMessageAsync(message, messageId);
                StateHasChanged();
            }
        });
    }
}
```

### Pattern 3: Update Message Status

```csharp
<SfChatUI @ref="chatRef" ID="chat" User="CurrentUser" Messages="Messages">
    <MessageTemplate>
        <div>
            <p>@context.Message.Text</p>
            <small>@context.Message.Status</small>
        </div>
    </MessageTemplate>
</SfChatUI>

@code {
    private SfChatUI chatRef;
    
    private async Task MarkAsRead(string messageId)
    {
        var message = Messages.FirstOrDefault(m => m.ID == messageId);
        
        if (message != null)
        {
            message.Status = "Read";
            await chatRef.UpdateMessageAsync(message, messageId);
        }
    }
}
```

### Pattern 4: Batch Message Updates

```csharp
<button @onclick="MarkAllAsRead">Mark All as Read</button>
<SfChatUI @ref="chatRef" ID="chat" User="CurrentUser" Messages="Messages"></SfChatUI>

@code {
    private SfChatUI chatRef;
    
    private async Task MarkAllAsRead()
    {
        foreach (var message in Messages.Where(m => m.Status == "Unread"))
        {
            message.Status = "Read";
            await chatRef.UpdateMessageAsync(message, message.ID);
        }
        
        StateHasChanged();
    }
}
```

---

## FocusAsync

**Method Signature:** `Task FocusAsync()`

Asynchronously sets focus on the chat input text area.

### Use Cases
- Focus input after page load
- Return focus after modal close
- Keyboard navigation support
- Quick reply functionality

### Basic Usage

```csharp
<SfChatUI @ref="chatRef" ID="chat" User="CurrentUser" Messages="Messages"></SfChatUI>

<button @onclick="FocusInput">Focus Chat Input</button>

@code {
    private SfChatUI chatRef;
    
    private async Task FocusInput()
    {
        await chatRef.FocusAsync();
    }
}
```

### Pattern 1: Auto-Focus on Page Load

```csharp
<SfChatUI @ref="chatRef" ID="chat" User="CurrentUser" Messages="Messages"></SfChatUI>

@code {
    private SfChatUI chatRef;
    
    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            // Auto-focus input when page loads
            await Task.Delay(100); // Allow component initialization
            await chatRef.FocusAsync();
        }
    }
}
```

### Pattern 2: Focus After Quick Reply

```csharp
<SfChatUI @ref="chatRef" ID="chat" User="CurrentUser" Messages="Messages"></SfChatUI>

<div class="quick-replies">
    <button @onclick="@(() => SendQuickReply("Yes"))">Yes</button>
    <button @onclick="@(() => SendQuickReply("No"))">No</button>
    <button @onclick="@(() => SendQuickReply("Maybe"))">Maybe</button>
</div>

@code {
    private SfChatUI chatRef;
    
    private async Task SendQuickReply(string reply)
    {
        // Add quick reply message
        Messages.Add(new ChatMessage 
        { 
            Text = reply, 
            Author = CurrentUser 
        });
        
        StateHasChanged();
        
        // Return focus to input for follow-up
        await chatRef.FocusAsync();
    }
}
```

### Pattern 3: Focus After Modal Close

```csharp
<SfChatUI @ref="chatRef" ID="chat" User="CurrentUser" Messages="Messages"></SfChatUI>

@if (showModal)
{
    <div class="modal">
        <h3>Attachment Options</h3>
        <button @onclick="CloseModal">Close</button>
    </div>
}

@code {
    private SfChatUI chatRef;
    private bool showModal = false;
    
    private async Task CloseModal()
    {
        showModal = false;
        StateHasChanged();
        
        // Return focus to chat input
        await chatRef.FocusAsync();
    }
}
```

---

## Complete Example: All Methods Combined

```csharp
@page "/advanced-chat"
@using Syncfusion.Blazor.InteractiveChat

<h3>Advanced Chat with Programmatic Control</h3>

<div class="toolbar">
    <button @onclick="ScrollToBottom">Latest Messages</button>
    <button @onclick="FocusInput">Focus Input</button>
    <button @onclick="EnableEditMode">Edit Last Message</button>
    <input @bind="searchText" placeholder="Search..." />
    <button @onclick="SearchMessages">Search</button>
</div>

<div style="height: 500px; width: 600px;">
    <SfChatUI 
        @ref="chatRef"
        ID="chat" 
        User="CurrentUser" 
        Messages="Messages"
        MessageSend="@OnMessageSend">
    </SfChatUI>
</div>

@if (isEditing)
{
    <div class="edit-panel">
        <input @bind="editText" />
        <button @onclick="SaveEdit">Save Edit</button>
        <button @onclick="CancelEdit">Cancel</button>
    </div>
}

@code {
    private SfChatUI chatRef;
    private string searchText = "";
    private bool isEditing = false;
    private string editText = "";
    private ChatMessage editingMessage;
    
    private UserModel CurrentUser = new UserModel 
    { 
        ID = "user1", 
        User = "Albert",
        AvatarBgColor = "#4a90e2"
    };
    
    private List<ChatMessage> Messages = new()
    {
        new ChatMessage 
        { 
            ID = "msg1",
            Text = "Welcome to advanced chat!", 
            Author = new UserModel { ID = "user1", User = "Albert" }
        }
    };
    
    // ScrollToBottomAsync example
    private async Task ScrollToBottom()
    {
        await chatRef.ScrollToBottomAsync();
    }
    
    // FocusAsync example
    private async Task FocusInput()
    {
        await chatRef.FocusAsync();
    }
    
    // UpdateMessageAsync example
    private void EnableEditMode()
    {
        editingMessage = Messages.LastOrDefault(m => m.Author.ID == CurrentUser.ID);
        if (editingMessage != null)
        {
            editText = editingMessage.Text;
            isEditing = true;
        }
    }
    
    private async Task SaveEdit()
    {
        if (editingMessage != null)
        {
            editingMessage.Text = editText;
            await chatRef.UpdateMessageAsync(editingMessage, editingMessage.ID);
            isEditing = false;
        }
    }
    
    private void CancelEdit()
    {
        isEditing = false;
        editText = "";
    }
    
    // ScrollToMessageAsync example
    private async Task SearchMessages()
    {
        var found = Messages.FirstOrDefault(m => 
            m.Text.Contains(searchText, StringComparison.OrdinalIgnoreCase));
        
        if (found != null)
        {
            await chatRef.ScrollToMessageAsync(found.ID);
        }
    }
    
    private async Task OnMessageSend(ChatMessageSendEventArgs args)
    {
        Messages.Add(args.Message);
        await chatRef.ScrollToBottomAsync();
        StateHasChanged();
    }
}
```

---

## Best Practices

1. **Always await method calls** - All methods are async and return `Task`
2. **Check for null reference** - Ensure `@ref` is initialized before calling methods
3. **Allow rendering time** - Use `Task.Delay()` when calling methods immediately after state changes
4. **Handle errors gracefully** - Wrap calls in try-catch for production code
5. **Update state after modifications** - Call `StateHasChanged()` after updating Messages collection
6. **Use component reference** - Always use `@ref` to get component instance
7. **Combine with events** - Use methods in event handlers for dynamic behavior
8. **Consider timing** - Call methods after `OnAfterRenderAsync` for initialization scenarios

---

## Error Handling

```csharp
private async Task SafeScrollToBottom()
{
    try
    {
        if (chatRef != null)
        {
            await chatRef.ScrollToBottomAsync();
        }
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Scroll error: {ex.Message}");
    }
}
```
