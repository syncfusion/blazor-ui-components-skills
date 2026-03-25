# Attachments & Events

## Table of Contents
- [File Attachments Configuration](#file-attachments-configuration)
- [Save and Remove URLs](#save-and-remove-urls)
- [File Type Restrictions](#file-type-restrictions)
- [File Size Limits](#file-size-limits)
- [Event Handling](#event-handling)
- [Attachment Events](#attachment-events)
  - [OnAttachmentUploadReady](#onattachmentuploadready)
  - [AttachmentUploadSuccess](#attachmentuploadsuccess)
  - [AttachmentUploadFailed](#attachmentuploadfailed)
  - [AttachmentClick](#attachmentclick)
  - [AttachmentRemoved](#attachmentremoved)
- [Mention Events](#mention-events)

## File Attachments Configuration

The [ChatUIAttachment](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.ChatUIAttachment.html) tag enables file attachment support in the Chat UI component. Set the [Enable](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.ChatUIAttachment.html#Syncfusion_Blazor_InteractiveChat_ChatUIAttachment_Enable) property to `true` to activate file uploads.

### Enable File Attachments

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 600px;">
    <SfChatUI ID="chatUser" User="CurrentUserModel" Messages="ChatUserMessages">
        <ChatUIAttachment Enable="true">
        </ChatUIAttachment>
    </SfChatUI>
</div>

@code {
    private UserModel CurrentUserModel = new UserModel() 
    { 
        ID = "User1", 
        User = "Albert" 
    };

    private List<ChatMessage> ChatUserMessages = new();
}
```

### Disable File Attachments

```csharp
<SfChatUI ID="chat" User="CurrentUser" Messages="Messages">
    <ChatUIAttachment Enable="false">
    </ChatUIAttachment>
</SfChatUI>
```

## Save and Remove URLs

Set the [SaveUrl](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.ChatUIAttachment.html#Syncfusion_Blazor_InteractiveChat_ChatUIAttachment_SaveUrl) and [RemoveUrl](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.ChatUIAttachment.html#Syncfusion_Blazor_InteractiveChat_ChatUIAttachment_RemoveUrl) properties to specify server endpoints for handling file uploads and removals.

### Basic Configuration

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 600px;">
    <SfChatUI ID="chatUser" User="CurrentUserModel" Messages="ChatUserMessages">
        <ChatUIAttachment 
            Enable="true"
            SaveUrl="@SaveUrl" 
            RemoveUrl="@RemoveUrl">
        </ChatUIAttachment>
    </SfChatUI>
</div>

@code {
    private string SaveUrl = "https://api.example.com/upload/save";
    private string RemoveUrl = "https://api.example.com/upload/remove";

    private UserModel CurrentUserModel = new UserModel() 
    { 
        ID = "User1", 
        User = "Albert" 
    };

    private List<ChatMessage> ChatUserMessages = new();
}
```

### Controller Implementation (Server-Side)

```csharp
// API Controller
[ApiController]
[Route("api/[controller]")]
public class UploadController : ControllerBase
{
    [HttpPost("save")]
    public IActionResult Save(IFormFile file)
    {
        try
        {
            if (file != null && file.Length > 0)
            {
                // Save file to server
                var fileName = Guid.NewGuid().ToString() + Path.GetExtension(file.FileName);
                var filePath = Path.Combine("uploads", fileName);
                
                using (var stream = new FileStream(filePath, FileMode.Create))
                {
                    file.CopyTo(stream);
                }
                
                return Ok(new { name = fileName, size = file.Length });
            }
            
            return BadRequest("No file uploaded");
        }
        catch (Exception ex)
        {
            return StatusCode(500, ex.Message);
        }
    }

    [HttpPost("remove")]
    public IActionResult Remove([FromBody] string fileName)
    {
        try
        {
            var filePath = Path.Combine("uploads", fileName);
            
            if (System.IO.File.Exists(filePath))
            {
                System.IO.File.Delete(filePath);
                return Ok();
            }
            
            return NotFound("File not found");
        }
        catch (Exception ex)
        {
            return StatusCode(500, ex.Message);
        }
    }
}
```

## File Type Restrictions

The [AllowedFileTypes](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.ChatUIAttachment.html#Syncfusion_Blazor_InteractiveChat_ChatUIAttachment_AllowedFileTypes) property controls which file types users can upload using file extensions or MIME types.

### Restricting File Types

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 600px;">
    <SfChatUI ID="chatUser" User="CurrentUserModel" Messages="ChatUserMessages">
        <!-- Allow only PDF files -->
        <ChatUIAttachment 
            Enable="true"
            AllowedFileTypes=".pdf" 
            SaveUrl="@SaveUrl" 
            RemoveUrl="@RemoveUrl">
        </ChatUIAttachment>
    </SfChatUI>
</div>

@code {
    private string SaveUrl = "https://api.example.com/upload/save";
    private string RemoveUrl = "https://api.example.com/upload/remove";

    private UserModel CurrentUserModel = new UserModel() 
    { 
        ID = "User1", 
        User = "Albert" 
    };

    private List<ChatMessage> ChatUserMessages = new();
}
```

### Multiple File Types

```csharp
<!-- Allow images and documents -->
<ChatUIAttachment 
    Enable="true"
    AllowedFileTypes=".jpg,.jpeg,.png,.gif,.pdf,.docx,.xlsx" 
    SaveUrl="@SaveUrl" 
    RemoveUrl="@RemoveUrl">
</ChatUIAttachment>
```

### File Type Categories

```csharp
<!-- Documents only -->
<ChatUIAttachment 
    AllowedFileTypes=".pdf,.docx,.doc,.xlsx,.xls,.pptx,.txt"
    SaveUrl="@SaveUrl" 
    RemoveUrl="@RemoveUrl">
</ChatUIAttachment>

<!-- Images only -->
<ChatUIAttachment 
    AllowedFileTypes=".jpg,.jpeg,.png,.gif,.bmp,.svg,.webp"
    SaveUrl="@SaveUrl" 
    RemoveUrl="@RemoveUrl">
</ChatUIAttachment>

<!-- Media files -->
<ChatUIAttachment 
    AllowedFileTypes=".mp3,.mp4,.avi,.mov,.wav,.flv"
    SaveUrl="@SaveUrl" 
    RemoveUrl="@RemoveUrl">
</ChatUIAttachment>
```

## File Size Limits

The [MaxFileSize](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.ChatUIAttachment.html#Syncfusion_Blazor_InteractiveChat_ChatUIAttachment_MaxFileSize) property defines the maximum file size in bytes. Default is `30000000` bytes (≈30 MB).

### Setting File Size Limit

```csharp
<ChatUIAttachment 
    Enable="true"
    MaxFileSize="4000000"  <!-- 4 MB limit -->
    SaveUrl="@SaveUrl" 
    RemoveUrl="@RemoveUrl">
</ChatUIAttachment>
```

### Common Size Configurations

```csharp
<!-- 1 MB limit -->
<ChatUIAttachment MaxFileSize="1000000" Enable SaveUrl="@SaveUrl" RemoveUrl="@RemoveUrl"></ChatUIAttachment>

<!-- 5 MB limit -->
<ChatUIAttachment MaxFileSize="5000000" Enable SaveUrl="@SaveUrl" RemoveUrl="@RemoveUrl"></ChatUIAttachment>

<!-- 10 MB limit -->
<ChatUIAttachment MaxFileSize="10000000" Enable SaveUrl="@SaveUrl" RemoveUrl="@RemoveUrl"></ChatUIAttachment>

<!-- 100 MB limit -->
<ChatUIAttachment MaxFileSize="100000000" Enable SaveUrl="@SaveUrl" RemoveUrl="@RemoveUrl"></ChatUIAttachment>
```

## Event Handling

### Created Event

The [Created](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_Created) event triggers when the component finishes rendering.

```csharp
<SfChatUI ID="chat" User="CurrentUser" Messages="Messages" Created="@OnChatCreated"></SfChatUI>

@code {
    private void OnChatCreated()
    {
        // Component initialization complete
        StateHasChanged();
    }
}
```

### Message Send Event

The [MessageSend](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_MessageSend) event fires when a message is being sent.

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    MessageSend="@OnMessageSend">
</SfChatUI>

@code {
    private void OnMessageSend(ChatMessageSendEventArgs args)
    {
        // Handle message send
        // args.Message contains the message text
    }
}
```

### User Typing Event

The [UserTyping](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_UserTyping) event triggers when user types a message.

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    UserTyping="@OnUserTyping">
</SfChatUI>

@code {
    private void OnUserTyping(ChatUserTypingEventArgs args)
    {
        // Handle user typing
        // Send typing notification to other users
    }
}
```

## Attachment Events

### OnAttachmentUploadReady

The [OnAttachmentUploadReady](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_OnAttachmentUploadReady) event fires before file upload begins.

```csharp
<SfChatUI ID="chat" User="CurrentUser" Messages="Messages" OnAttachmentUploadReady="@OnUploadReady">
    <ChatUIAttachment Enable SaveUrl="@SaveUrl" RemoveUrl="@RemoveUrl"></ChatUIAttachment>
</SfChatUI>

@code {
    private string SaveUrl = "api/upload/save";
    private string RemoveUrl = "api/upload/remove";

    private void OnUploadReady(AttachmentUploadReadyEventArgs args)
    {
        // Validate file before upload
        // Can cancel upload if needed: args.Cancel = true;
    }
}
```

### AttachmentUploadSuccess

The [AttachmentUploadSuccess](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_AttachmentUploadSuccess) event fires when file successfully uploads.

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    AttachmentUploadSuccess="@OnUploadSuccess">
    <ChatUIAttachment Enable SaveUrl="@SaveUrl" RemoveUrl="@RemoveUrl"></ChatUIAttachment>
</SfChatUI>

@code {
    private string SaveUrl = "api/upload/save";
    private string RemoveUrl = "api/upload/remove";

    private void OnUploadSuccess(SuccessEventArgs args)
    {
        // File uploaded successfully
        // args contains file information
    }
}
```

### AttachmentUploadFailed

The [AttachmentUploadFailed](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_AttachmentUploadFailed) event fires when file upload fails.

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    AttachmentUploadFailed="@OnUploadFailed">
    <ChatUIAttachment Enable SaveUrl="@SaveUrl" RemoveUrl="@RemoveUrl"></ChatUIAttachment>
</SfChatUI>

@code {
    private string SaveUrl = "api/upload/save";
    private string RemoveUrl = "api/upload/remove";

    private void OnUploadFailed(FailureEventArgs args)
    {
        // Handle upload failure
        // Show error message to user
    }
}
```

### AttachmentClick

The [AttachmentClick](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_AttachmentClick) event fires when an attachment item is clicked, either before sending or after the attachment is sent.

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    AttachmentClick="@OnAttachmentClick">
    <ChatUIAttachment Enable SaveUrl="@SaveUrl" RemoveUrl="@RemoveUrl"></ChatUIAttachment>
</SfChatUI>

@code {
    private string SaveUrl = "api/upload/save";
    private string RemoveUrl = "api/upload/remove";

    private void OnAttachmentClick(ChatAttachmentClickEventArgs args)
    {
        // Handle attachment click
        // Access file info: args.SelectedFile
        // Can cancel preview: args.Cancel = true;
        
        Console.WriteLine($"Attachment clicked: {args.SelectedFile?.Name}");
    }
}
```

#### Use Case: Custom Attachment Preview

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    AttachmentClick="@OnAttachmentClick">
    <ChatUIAttachment Enable SaveUrl="@SaveUrl" RemoveUrl="@RemoveUrl"></ChatUIAttachment>
</SfChatUI>

@code {
    private string SaveUrl = "api/upload/save";
    private string RemoveUrl = "api/upload/remove";

    private void OnAttachmentClick(ChatAttachmentClickEventArgs args)
    {
        // Cancel default preview
        args.Cancel = true;
        
        // Implement custom preview logic
        if (args.SelectedFile != null)
        {
            var fileExtension = Path.GetExtension(args.SelectedFile.Name);
            
            if (fileExtension == ".pdf")
            {
                // Open PDF in custom viewer
                OpenPdfViewer(args.SelectedFile);
            }
            else if (fileExtension == ".jpg" || fileExtension == ".png")
            {
                // Open image in custom lightbox
                OpenImageLightbox(args.SelectedFile);
            }
            else
            {
                // Download the file
                DownloadFile(args.SelectedFile);
            }
        }
    }
    
    private void OpenPdfViewer(FileInfo file) { /* Custom PDF viewer */ }
    private void OpenImageLightbox(FileInfo file) { /* Custom image lightbox */ }
    private void DownloadFile(FileInfo file) { /* Download logic */ }
}
```

### AttachmentRemoved

The [AttachmentRemoved](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_AttachmentRemoved) event fires when an attachment is being removed. Can be cancelled by setting `args.Cancel = true`.

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    AttachmentRemoved="@OnAttachmentRemoved">
    <ChatUIAttachment Enable SaveUrl="@SaveUrl" RemoveUrl="@RemoveUrl"></ChatUIAttachment>
</SfChatUI>

@code {
    private string SaveUrl = "api/upload/save";
    private string RemoveUrl = "api/upload/remove";

    private void OnAttachmentRemoved(RemovingEventArgs args)
    {
        // Handle attachment removal
        // Can cancel removal: args.Cancel = true;
        
        Console.WriteLine("Attachment removed");
    }
}
```

#### Use Case: Confirm Before Removing

```csharp
@using Syncfusion.Blazor.InteractiveChat
@using Syncfusion.Blazor.Popups

<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    AttachmentRemoved="@OnAttachmentRemoved">
    <ChatUIAttachment Enable SaveUrl="@SaveUrl" RemoveUrl="@RemoveUrl"></ChatUIAttachment>
</SfChatUI>

<SfDialog @ref="confirmDialog" Width="300px" IsModal="true" Visible="false">
    <DialogTemplates>
        <Header>Confirm Removal</Header>
        <Content>Are you sure you want to remove this attachment?</Content>
    </DialogTemplates>
    <DialogButtons>
        <DialogButton Content="Yes" OnClick="@ConfirmRemoval" />
        <DialogButton Content="No" OnClick="@CancelRemoval" />
    </DialogButtons>
</SfDialog>

@code {
    private string SaveUrl = "api/upload/save";
    private string RemoveUrl = "api/upload/remove";
    private UserModel CurrentUser = new() { ID = "user1", User = "Albert" };
    private List<ChatMessage> Messages = new();
    
    private SfDialog confirmDialog;
    private RemovingEventArgs pendingRemovalArgs;

    private async Task OnAttachmentRemoved(RemovingEventArgs args)
    {
        // Cancel the removal temporarily
        args.Cancel = true;
        pendingRemovalArgs = args;
        
        // Show confirmation dialog
        await confirmDialog.ShowAsync();
    }
    
    private async Task ConfirmRemoval()
    {
        // User confirmed, allow removal
        if (pendingRemovalArgs != null)
        {
            pendingRemovalArgs.Cancel = false;
            // Trigger cleanup logic
            await CleanupAttachment();
        }
        await confirmDialog.HideAsync();
    }
    
    private async Task CancelRemoval()
    {
        // Keep the removal cancelled
        await confirmDialog.HideAsync();
    }
    
    private async Task CleanupAttachment()
    {
        // Perform any additional cleanup
        await Task.CompletedTask;
    }
}
```

#### Use Case: Track Attachment Lifecycle

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    AttachmentClick="@OnAttachmentClick"
    OnAttachmentUploadReady="@OnUploadReady"
    AttachmentUploadSuccess="@OnUploadSuccess"
    AttachmentRemoved="@OnAttachmentRemoved">
    <ChatUIAttachment Enable SaveUrl="@SaveUrl" RemoveUrl="@RemoveUrl"></ChatUIAttachment>
</SfChatUI>

@code {
    private string SaveUrl = "api/upload/save";
    private string RemoveUrl = "api/upload/remove";
    private Dictionary<string, DateTime> attachmentLog = new();

    private void OnUploadReady(AttachmentUploadReadyEventArgs args)
    {
        foreach (var file in args.FilesData)
        {
            attachmentLog[file.Name] = DateTime.Now;
            Console.WriteLine($"Upload started: {file.Name}");
        }
    }

    private void OnUploadSuccess(SuccessEventArgs args)
    {
        Console.WriteLine("Upload completed successfully");
    }

    private void OnAttachmentClick(ChatAttachmentClickEventArgs args)
    {
        if (args.SelectedFile != null && attachmentLog.ContainsKey(args.SelectedFile.Name))
        {
            Console.WriteLine($"Attachment accessed: {args.SelectedFile.Name} " +
                            $"(uploaded at {attachmentLog[args.SelectedFile.Name]})");
        }
    }

    private void OnAttachmentRemoved(RemovingEventArgs args)
    {
        Console.WriteLine("Attachment removed from chat");
        // Could log removal for audit purposes
    }
}
```

## Complete Example with All Events

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 500px; width: 600px;">
    <SfChatUI 
        ID="chatUser"
        User="CurrentUser"
        Messages="Messages"
        Created="@OnChatCreated"
        MessageSend="@OnMessageSend"
        UserTyping="@OnUserTyping"
        OnAttachmentUploadReady="@OnUploadReady"
        AttachmentUploadSuccess="@OnUploadSuccess"
        AttachmentUploadFailed="@OnUploadFailed">
        <ChatUIAttachment 
            Enable="true"
            SaveUrl="@SaveUrl"
            RemoveUrl="@RemoveUrl"
            AllowedFileTypes=".pdf,.docx,.jpg,.png"
            MaxFileSize="5000000">
        </ChatUIAttachment>
    </SfChatUI>
</div>

@code {
    private string SaveUrl = "api/upload/save";
    private string RemoveUrl = "api/upload/remove";

    private UserModel CurrentUser = new UserModel 
    { 
        ID = "user1", 
        User = "Albert" 
    };

    private List<ChatMessage> Messages = new();

    private void OnChatCreated()
    {
        Console.WriteLine("Chat UI created");
    }

    private void OnMessageSend(ChatMessageSendEventArgs args)
    {
        Console.WriteLine($"Message sent: {args.Message}");
        // Process message
    }

    private void OnUserTyping(ChatUserTypingEventArgs args)
    {
        Console.WriteLine("User is typing");
        // Send typing notification
    }

    private void OnUploadReady(AttachmentUploadReadyEventArgs args)
    {
        Console.WriteLine("Upload starting");
    }

    private void OnUploadSuccess(SuccessEventArgs args)
    {
        Console.WriteLine("File uploaded successfully");
    }

    private void OnUploadFailed(FailureEventArgs args)
    {
        Console.WriteLine("File upload failed");
    }
}
```

## Mention Events

### ValueSelecting

The [ValueSelecting](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.InteractiveChat.SfChatUI.html#Syncfusion_Blazor_InteractiveChat_SfChatUI_ValueSelecting) event occurs when a user selects a mention from the suggestion popup in the chat UI.

```csharp
@using Syncfusion.Blazor.InteractiveChat

<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    MentionChar="@"
    MentionUsers="@AvailableUsers"
    ValueSelecting="@OnMentionSelecting">
</SfChatUI>

@code {
    private UserModel CurrentUser = new() { ID = "user1", User = "Albert" };
    private List<ChatMessage> Messages = new();
    
    private List<UserModel> AvailableUsers = new()
    {
        new UserModel { ID = "user2", User = "Michale Suyama" },
        new UserModel { ID = "user3", User = "Reena" },
        new UserModel { ID = "user4", User = "Janet" }
    };

    private void OnMentionSelecting(MentionValueSelectingEventArgs<UserModel> args)
    {
        // Handle mention selection
        Console.WriteLine($"User mentioned: {args.ItemData.User}");
    }
}
```

#### Use Case: Track Mentions for Notifications

```csharp
@using Syncfusion.Blazor.InteractiveChat

<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    MentionChar="@"
    MentionUsers="@AvailableUsers"
    ValueSelecting="@OnMentionSelecting"
    MessageSend="@OnMessageSend">
</SfChatUI>

@code {
    private UserModel CurrentUser = new() { ID = "user1", User = "Albert" };
    private List<ChatMessage> Messages = new();
    private List<UserModel> mentionedUsers = new();
    
    private List<UserModel> AvailableUsers = new()
    {
        new UserModel { ID = "user2", User = "Michale Suyama" },
        new UserModel { ID = "user3", User = "Reena" },
        new UserModel { ID = "user4", User = "Janet" }
    };

    private void OnMentionSelecting(MentionValueSelectingEventArgs<UserModel> args)
    {
        // Track mentioned users
        if (!mentionedUsers.Any(u => u.ID == args.ItemData.ID))
        {
            mentionedUsers.Add(args.ItemData);
        }
        
        Console.WriteLine($"Mentioned: @{args.ItemData.User}");
    }
    
    private async Task OnMessageSend(ChatMessageSendEventArgs args)
    {
        // Send notifications to mentioned users
        foreach (var user in mentionedUsers)
        {
            await SendNotification(user, args.Message.Text);
        }
        
        // Clear mentions for next message
        mentionedUsers.Clear();
    }
    
    private async Task SendNotification(UserModel user, string message)
    {
        // Send notification to mentioned user
        Console.WriteLine($"Notifying {user.User}: {message}");
        await Task.CompletedTask;
    }
}
```

#### Use Case: Restrict Mentions Based on User Role

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    MentionChar="@"
    MentionUsers="@GetMentionableUsers()"
    ValueSelecting="@OnMentionSelecting">
</SfChatUI>

@code {
    private UserModel CurrentUser = new() 
    { 
        ID = "user1", 
        User = "Albert",
        // Custom property (extended UserModel)
    };
    
    private List<ChatMessage> Messages = new();
    
    private List<UserModel> AllUsers = new()
    {
        new UserModel { ID = "user2", User = "Michale Suyama" },
        new UserModel { ID = "user3", User = "Reena" },
        new UserModel { ID = "admin1", User = "Admin User" }
    };

    private List<UserModel> GetMentionableUsers()
    {
        // Filter users based on current user's permissions
        // For example, regular users can't mention admins
        return AllUsers.Where(u => !u.User.Contains("Admin")).ToList();
    }

    private void OnMentionSelecting(MentionValueSelectingEventArgs<UserModel> args)
    {
        // Validate the mention
        if (args.ItemData.User.Contains("Admin"))
        {
            // Could cancel if needed
            Console.WriteLine("Admin users cannot be mentioned");
            // args.Cancel = true; // If this property exists
        }
        else
        {
            Console.WriteLine($"Valid mention: @{args.ItemData.User}");
        }
    }
}
```

#### Use Case: Custom Mention Display Format

```csharp
<SfChatUI 
    ID="chat" 
    User="CurrentUser" 
    Messages="Messages"
    MentionChar="@"
    MentionUsers="@TeamMembers"
    ValueSelecting="@OnMentionSelecting">
</SfChatUI>

@code {
    private UserModel CurrentUser = new() { ID = "user1", User = "Albert" };
    private List<ChatMessage> Messages = new();
    
    private List<UserModel> TeamMembers = new()
    {
        new UserModel { ID = "user2", User = "Michale Suyama" },
        new UserModel { ID = "user3", User = "Reena" },
        new UserModel { ID = "user4", User = "Janet" }
    };

    private void OnMentionSelecting(MentionValueSelectingEventArgs<UserModel> args)
    {
        // Log mention with timestamp
        var mention = new
        {
            MentionedUser = args.ItemData.User,
            MentionedBy = CurrentUser.User,
            Timestamp = DateTime.Now
        };
        
        Console.WriteLine($"[{mention.Timestamp:HH:mm:ss}] {mention.MentionedBy} mentioned @{mention.MentionedUser}");
        
        // Could store in database for analytics
        LogMention(mention);
    }
    
    private void LogMention(object mention)
    {
        // Store mention data for analytics
    }
}
```

## Complete Example with All Events

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 500px; width: 600px;">
    <SfChatUI 
        ID="chatUser"
        User="CurrentUser"
        Messages="Messages"
        MentionChar="@"
        MentionUsers="@TeamMembers"
        Created="@OnChatCreated"
        MessageSend="@OnMessageSend"
        UserTyping="@OnUserTyping"
        ValueSelecting="@OnMentionSelecting"
        AttachmentClick="@OnAttachmentClick"
        OnAttachmentUploadReady="@OnUploadReady"
        AttachmentUploadSuccess="@OnUploadSuccess"
        AttachmentUploadFailed="@OnUploadFailed"
        AttachmentRemoved="@OnAttachmentRemoved">
        <ChatUIAttachment 
            Enable="true"
            SaveUrl="@SaveUrl"
            RemoveUrl="@RemoveUrl"
            AllowedFileTypes=".pdf,.docx,.jpg,.png"
            MaxFileSize="5000000">
        </ChatUIAttachment>
    </SfChatUI>
</div>

@code {
    private string SaveUrl = "api/upload/save";
    private string RemoveUrl = "api/upload/remove";

    private UserModel CurrentUser = new UserModel 
    { 
        ID = "user1", 
        User = "Albert" 
    };

    private List<ChatMessage> Messages = new();
    
    private List<UserModel> TeamMembers = new()
    {
        new UserModel { ID = "user2", User = "Michale Suyama" },
        new UserModel { ID = "user3", User = "Reena" }
    };

    // Lifecycle Events
    private void OnChatCreated()
    {
        Console.WriteLine("Chat UI created");
    }

    // Message Events
    private void OnMessageSend(ChatMessageSendEventArgs args)
    {
        Console.WriteLine($"Message sent: {args.Message.Text}");
    }

    private void OnUserTyping(ChatUserTypingEventArgs args)
    {
        Console.WriteLine($"User typing: {args.IsTyping}");
    }

    // Mention Events
    private void OnMentionSelecting(MentionValueSelectingEventArgs<UserModel> args)
    {
        Console.WriteLine($"User mentioned: @{args.ItemData.User}");
    }

    // Attachment Events
    private void OnAttachmentClick(ChatAttachmentClickEventArgs args)
    {
        Console.WriteLine($"Attachment clicked: {args.SelectedFile?.Name}");
    }

    private void OnUploadReady(AttachmentUploadReadyEventArgs args)
    {
        Console.WriteLine("Upload starting");
    }

    private void OnUploadSuccess(SuccessEventArgs args)
    {
        Console.WriteLine("File uploaded successfully");
    }

    private void OnUploadFailed(FailureEventArgs args)
    {
        Console.WriteLine("File upload failed");
    }

    private void OnAttachmentRemoved(RemovingEventArgs args)
    {
        Console.WriteLine("Attachment removed");
    }
}
```

## Event Summary Table

| Event Category | Event Name | Event Args Type | When It Fires |
|----------------|------------|-----------------|---------------|
| **Lifecycle** | `Created` | `object` | Component initialized |
| **Messages** | `MessageSend` | `ChatMessageSendEventArgs` | Message being sent |
| **Messages** | `UserTyping` | `ChatUserTypingEventArgs` | User typing in input |
| **Mentions** | `ValueSelecting` | `MentionValueSelectingEventArgs<UserModel>` | User selects a mention |
| **Attachments** | `OnAttachmentUploadReady` | `AttachmentUploadReadyEventArgs` | Before upload starts |
| **Attachments** | `AttachmentUploadSuccess` | `SuccessEventArgs` | Upload successful |
| **Attachments** | `AttachmentUploadFailed` | `FailureEventArgs` | Upload failed |
| **Attachments** | `AttachmentClick` | `ChatAttachmentClickEventArgs` | Attachment clicked |
| **Attachments** | `AttachmentRemoved` | `RemovingEventArgs` | Attachment removed |

## Best Practices

1. **Always Set URLs** - Configure both SaveUrl and RemoveUrl for attachments
2. **File Validation** - Validate file type and size on both client and server
3. **Security** - Sanitize file names and restrict upload paths
4. **Error Handling** - Provide clear error messages to users for all events
5. **Size Limits** - Set reasonable MaxFileSize values based on your use case
6. **File Organization** - Store files in organized server directories
7. **Logging** - Log file uploads and user actions for audit trail
8. **Cleanup** - Implement cleanup for orphaned/old files
9. **Event Cancellation** - Use `args.Cancel = true` to prevent unwanted actions
10. **Mention Notifications** - Send notifications when users are mentioned
11. **Track User Activity** - Use events to track engagement and analytics
12. **Validate Mentions** - Ensure mentioned users are valid and allowed
