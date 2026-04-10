# Toast - Advanced Features

## Table of Contents
- [Animation Settings](#animation-settings)
- [Progress Bar](#progress-bar)
- [Action Buttons](#action-buttons)
- [Templates](#templates)
- [Event Callbacks](#event-callbacks)
- [Multiple Toast Management](#multiple-toast-management)
- [Target Container](#target-container)
- [RTL Support](#rtl-support)
- [Custom Dimensions](#custom-dimensions)

---

## Animation Settings

Control how toasts appear and disappear with customizable animations.

### Show and Hide Animations

```razor
<SfToast @ref="ToastObj" Title="Animated Toast" Content="Watch me slide in and fade out!">
    <ToastAnimationSettings>
        <ToastShowAnimationSettings Effect="ToastEffect.SlideLeftIn" Duration="600" Easing="ToastEasing.Ease" />
        <ToastHideAnimationSettings Effect="ToastEffect.FadeOut" Duration="400" Easing="ToastEasing.Linear" />
    </ToastAnimationSettings>
</SfToast>

@code {
    SfToast ToastObj;
}
```

### Available Animation Effects

**Show Effects:**
- `SlideLeftIn` - Slide from left
- `SlideRightIn` - Slide from right
- `SlideTopIn` - Slide from top
- `SlideBottomIn` - Slide from bottom
- `FadeIn` - Fade in
- `FadeZoomIn` - Zoom and fade in
- `FlipLeftDownIn` - Flip animation

**Hide Effects:**
- `SlideLeftOut` - Slide to left
- `SlideRightOut` - Slide to right
- `SlideTopOut` - Slide to top
- `SlideBottomOut` - Slide to bottom
- `FadeOut` - Fade out
- `FadeZoomOut` - Zoom and fade out
- `FlipLeftDownOut` - Flip animation

### Easing Options

- `Linear` - Constant speed
- `Ease` - Slow start and end, faster in middle (default)
- `EaseIn` - Slow start
- `EaseOut` - Slow end
- `EaseInOut` - Slow start and end

### Position-Specific Animations

```razor
@page "/toast-animations"

<SfButton @onclick="ShowTopToast">Top (Slide Down)</SfButton>
<SfButton @onclick="ShowBottomToast">Bottom (Slide Up)</SfButton>
<SfButton @onclick="ShowLeftToast">Left (Slide Right)</SfButton>
<SfButton @onclick="ShowRightToast">Right (Slide Left)</SfButton>

<SfToast @ref="TopToast" Title="Top Toast" Content="Sliding from top">
    <ToastPosition X="Center" Y="Top" />
    <ToastAnimationSettings>
        <ToastShowAnimationSettings Effect="ToastEffect.SlideTopIn" Duration="400" />
        <ToastHideAnimationSettings Effect="ToastEffect.SlideTopOut" Duration="400" />
    </ToastAnimationSettings>
</SfToast>

<SfToast @ref="BottomToast" Title="Bottom Toast" Content="Sliding from bottom">
    <ToastPosition X="Center" Y="Bottom" />
    <ToastAnimationSettings>
        <ToastShowAnimationSettings Effect="ToastEffect.SlideBottomIn" Duration="400" />
        <ToastHideAnimationSettings Effect="ToastEffect.SlideBottomOut" Duration="400" />
    </ToastAnimationSettings>
</SfToast>

<SfToast @ref="LeftToast" Title="Left Toast" Content="Sliding from left">
    <ToastPosition X="Left" Y="Center" />
    <ToastAnimationSettings>
        <ToastShowAnimationSettings Effect="ToastEffect.SlideLeftIn" Duration="400" />
        <ToastHideAnimationSettings Effect="ToastEffect.SlideLeftOut" Duration="400" />
    </ToastAnimationSettings>
</SfToast>

<SfToast @ref="RightToast" Title="Right Toast" Content="Sliding from right">
    <ToastPosition X="Right" Y="Center" />
    <ToastAnimationSettings>
        <ToastShowAnimationSettings Effect="ToastEffect.SlideRightIn" Duration="400" />
        <ToastHideAnimationSettings Effect="ToastEffect.SlideRightOut" Duration="400" />
    </ToastAnimationSettings>
</SfToast>

@code {
    SfToast TopToast, BottomToast, LeftToast, RightToast;

    private async Task ShowTopToast() => await TopToast.ShowAsync();
    private async Task ShowBottomToast() => await BottomToast.ShowAsync();
    private async Task ShowLeftToast() => await LeftToast.ShowAsync();
    private async Task ShowRightToast() => await RightToast.ShowAsync();
}
```

---

## Progress Bar

Display a visual indicator of remaining time before auto-dismiss.

### Basic Progress Bar

```razor
<SfToast @ref="ToastObj" 
         Title="Progress Demo" 
         Content="Watch the progress bar count down"
         ShowProgressBar="true"
         Timeout="5000" />
```

### Progress Bar Direction

```razor
<SfToast @ref="ToastObj" 
         Title="Progress Demo" 
         Content="Progress bar moving right to left"
         ShowProgressBar="true"
         ProgressDirection="ProgressDirection.RTL"
         Timeout="5000" />
```

**Direction options:**
- `ProgressDirection.LTR` - Left to right (default)
- `ProgressDirection.RTL` - Right to left

### Custom Progress Bar Styling

```razor
<SfToast @ref="ToastObj" 
         Title="Styled Progress" 
         Content="Custom progress bar colors"
         ShowProgressBar="true"
         CssClass="custom-progress-toast"
         Timeout="5000" />

<style>
    .custom-progress-toast .e-toast-progress {
        background-color: #4CAF50;
        height: 4px;
    }
</style>
```

---

## Action Buttons

Add interactive buttons to toasts for user actions.

### Single Action Button

```razor
<SfToast @ref="ToastObj" Title="Action Required" Content="Do you want to save changes?">
    <ToastButtons>
        <ToastButton Content="Save" OnClick="@SaveChanges" />
    </ToastButtons>
</SfToast>

@code {
    SfToast ToastObj;

    private async Task SaveChanges()
    {
        // Hide toast after action
        await ToastObj.HideAsync();
    }
}
```

### Multiple Action Buttons

```razor
<SfToast @ref="ToastObj" 
         Title="Confirmation" 
         Content="Unsaved changes detected. What would you like to do?"
         Timeout="0">
    <ToastButtons>
        <ToastButton Content="Save" CssClass="e-success" OnClick="@SaveAndContinue" />
        <ToastButton Content="Discard" CssClass="e-danger" OnClick="@DiscardChanges" />
        <ToastButton Content="Cancel" OnClick="@CancelAction" />
    </ToastButtons>
</SfToast>

@code {
    SfToast ToastObj;

    private async Task SaveAndContinue()
    {
        await ToastObj.HideAsync();
    }

    private async Task DiscardChanges()
    {
        await ToastObj.HideAsync();
    }

    private async Task CancelAction()
    {
        await ToastObj.HideAsync();
    }
}
```

### Styled Action Buttons

```razor
<SfToast @ref="ToastObj" Title="Update Available" Content="A new version is available. Install now?">
    <ToastButtons>
        <ToastButton Content="Install" CssClass="e-primary e-small" OnClick="@InstallUpdate" />
        <ToastButton Content="Later" CssClass="e-flat e-small" OnClick="@RemindLater" />
    </ToastButtons>
</SfToast>

<style>
    .e-toast .e-toast-actions button {
        margin: 0 5px;
        padding: 6px 12px;
        font-size: 13px;
    }
</style>
```

---

## Templates

Customize toast appearance with custom templates.



```razor
<SfToast @ref="ToastObj" Content="Template demo with custom title">
    <ToastTemplates>
        <Template>
            <div class="custom-title">
                <span class="e-icons e-star"></span>
                <strong>Special Notification</strong>
            </div>
        </Template>
    </ToastTemplates>
</SfToast>

<style>
    .custom-title {
        display: flex;
        align-items: center;
        gap: 8px;
        color: #ff9800;
    }
</style>
```

```razor
<SfToast @ref="ToastObj" Title="New Message">
    <ToastTemplates>
        <Template>
            <div class="message-content">
                <img src="images/avatar.png" alt="User" class="avatar" />
                <div class="message-text">
                    <strong>John Doe</strong>
                    <p>Hey! Are you available for a meeting?</p>
                    <small>2 minutes ago</small>
                </div>
            </div>
        </Template>
    </ToastTemplates>
</SfToast>

<style>
    .message-content {
        display: flex;
        gap: 12px;
        align-items: start;
    }
    
    .avatar {
        width: 40px;
        height: 40px;
        border-radius: 50%;
    }
    
    .message-text strong {
        display: block;
        margin-bottom: 4px;
    }
    
    .message-text p {
        margin: 0 0 4px 0;
        font-size: 14px;
    }
    
    .message-text small {
        color: #666;
        font-size: 12px;
    }
</style>
```

### Complete Custom Template

```razor
<SfToast @ref="ToastObj">
    <ToastTemplates>
        <Template>
            <div class="custom-toast-wrapper">
                <div class="toast-header">
                    <span class="e-icons e-check-circle"></span>
                    <h4>Upload Complete</h4>
                    <button @onclick="@(() => ToastObj.HideAsync())" class="close-btn">×</button>
                </div>
                <div class="toast-body">
                    <p><strong>document.pdf</strong> (2.3 MB)</p>
                    <div class="progress-complete">100%</div>
                    <small>Uploaded to My Documents</small>
                </div>
                <div class="toast-actions">
                    <button @onclick="ViewFile" class="action-btn">View</button>
                    <button @onclick="ShareFile" class="action-btn">Share</button>
                </div>
            </div>
        </Template>
    </ToastTemplates>
</SfToast>

@code {
    private async Task ViewFile() 
    { 
        // Open file
        await ToastObj.HideAsync();
    }
    
    private async Task ShareFile() 
    { 
        // Share dialog
        await ToastObj.HideAsync();
    }
}

<style>
    .custom-toast-wrapper {
        padding: 12px;
    }
    
    .toast-header {
        display: flex;
        align-items: center;
        gap: 8px;
        margin-bottom: 10px;
    }
    
    .toast-header h4 {
        flex: 1;
        margin: 0;
        font-size: 16px;
    }
    
    .close-btn {
        background: none;
        border: none;
        font-size: 24px;
        cursor: pointer;
    }
    
    .toast-body {
        margin-bottom: 10px;
    }
    
    .progress-complete {
        background: #4CAF50;
        color: white;
        padding: 4px 8px;
        border-radius: 4px;
        display: inline-block;
        font-size: 12px;
        margin: 5px 0;
    }
    
    .toast-actions {
        display: flex;
        gap: 8px;
    }
    
    .action-btn {
        padding: 6px 16px;
        border: 1px solid #ddd;
        background: white;
        cursor: pointer;
        border-radius: 4px;
    }
</style>
```

---

## Event Callbacks

Handle toast lifecycle events.

### Available Events

```razor
@using Syncfusion.Blazor.Notifications

<SfToast @ref="ToastObj"
         Title="Event Demo"
         Content="Toast with event handlers">

    <ToastEvents 
        Created="OnCreated"
        Destroyed="OnDestroyed"
        OnOpen="OnOpen"
        Opened="Opened"
        Closed="Closed"
        OnClick="OnClick">
    </ToastEvents>

</SfToast>

<button class="e-btn" @onclick="ShowToast">Show Toast</button>

@code {
    private SfToast ToastObj;

    private async Task ShowToast()
    {
        await ToastObj.ShowAsync();
    }

    // Fires after Toast component is created
    public void OnCreated(object args)
    {
        Console.WriteLine("Toast component created");
    }

    // Fires after Toast component is destroyed
    public void OnDestroyed(object args)
    {
        Console.WriteLine("Toast component destroyed");
    }

    // Fires before toast opens (can cancel)
    public void OnOpen(ToastBeforeOpenArgs args)
    {
        Console.WriteLine("Before toast opens");
        // args.Cancel = true;
    }

    // Fires after toast is shown
    public void Opened(ToastOpenArgs args)
    {
        Console.WriteLine("Toast opened");
    }

    // Fires when toast is clicked
    public void OnClick(ToastClickEventArgs args)
    {
        Console.WriteLine("Toast clicked");
    }

    // Fires after toast closes
    public void Closed(ToastCloseArgs args)
    {
        Console.WriteLine("Toast closed");
    }
}

```

### Practical Event Usage

**Track notification history:**
```razor
@code {
    private List<string> notificationHistory = new();

    private void OnOpen(ToastOpenArgs args)
    {
        var message = $"{DateTime.Now:HH:mm:ss} - {ToastObj.Content}";
        notificationHistory.Add(message);
    }
}
```

**Analytics tracking:**
```razor
@code {
    private void OnClick(ToastClickEventArgs args)
    {
        // Track user interaction
        await AnalyticsService.TrackEvent("Toast_Clicked", new 
        {
            Title = ToastObj.Title,
            Timestamp = DateTime.Now
        });
    }
}
```

---

## Multiple Toast Management

Control how multiple toasts are displayed and organized.

### Stack Order

```razor
<SfToast @ref="ToastObj" 
         NewestOnTop="true">
    <ToastPosition X="Right" Y="Bottom"></ToastPosition>
</SfToast>

@code {
    // NewestOnTop = true: Latest toast on top (default)
    // NewestOnTop = false: Latest toast at bottom
}
```

### Sequential Toast Display

```razor
@page "/sequential-toasts"

<SfButton @onclick="ShowSequentialMessages">Show Progress Updates</SfButton>

<SfToast @ref="ToastObj">
    <ToastPosition X="Right" Y="Top"></ToastPosition>
</SfToast>

@code {
    SfToast ToastObj;

    private async Task ShowSequentialMessages()
    {
        ToastObj.Content = "Step 1: Validating data...";
        ToastObj.Timeout = 2000;
        await ToastObj.ShowAsync();
        await Task.Delay(2000);

        ToastObj.Content = "Step 2: Processing records...";
        await ToastObj.ShowAsync();
        await Task.Delay(2000);

        ToastObj.Content = "Step 3: Generating report...";
        await ToastObj.ShowAsync();
        await Task.Delay(2000);

        ToastObj.Content = "Complete! Report generated successfully.";
        ToastObj.CssClass = "e-toast-success";
        ToastObj.Timeout = 5000;
        await ToastObj.ShowAsync();
    }
}
```

### Hide All Toasts

```razor
@code {
    // Hide all visible toasts
    private async Task HideAllToasts()
    {
        await ToastObj.HideAsync("All");
    }
}
```

---

## Target Container

Specify where toasts should be rendered within the page.

### Default (Body)

```razor
<!-- Renders in document body (default) -->
<SfToast @ref="ToastObj" Title="Default" Content="Appears in body" />
```

### Specific Container

```razor
<div id="toast-container" style="position: relative; height: 400px; border: 1px solid #ddd;">
    <p>Toasts will appear within this container</p>
    
    <SfButton @onclick="@(() => ToastObj.ShowAsync())">Show Toast</SfButton>
    
    <SfToast @ref="ToastObj" 
             Title="Contained" 
             Content="Appears in specific container"
             Target="#toast-container">
        <ToastPosition X="Right" Y="Top"></ToastPosition>
    </SfToast>
</div>

@code {
    SfToast ToastObj;
}
```

**Use cases for Target:**
- Modal dialogs with their own notifications
- Specific page sections (dashboard panels)
- Iframe content
- Split-screen layouts

---

## RTL Support

Enable right-to-left support for Arabic, Hebrew, and other RTL languages.

```razor
<SfToast @ref="ToastObj" 
         Title="إشعار" 
         Content="تم حفظ التغييرات بنجاح"
         EnableRtl="true">
    <ToastPosition X="Left" Y="Top"></ToastPosition>
</SfToast>

@code {
    SfToast ToastObj;
}
```

---

## Custom Dimensions

Control toast width and height.

```razor
<SfToast @ref="ToastObj" 
         Title="Custom Size" 
         Content="This toast has custom dimensions"
         Width="400px"
         Height="150px" />
```

**Width options:**
- Pixels: `"400px"`
- Percentage: `"50%"`
- Auto: `"auto"` (fits content)

**Height options:**
- Pixels: `"150px"`
- Auto: `"auto"` (default, fits content)

### Responsive Width

```razor
<SfToast @ref="ToastObj" 
         Title="Responsive" 
         Content="Adapts to screen size"
         Width="90%"
         CssClass="responsive-toast" />

<style>
    @media (min-width: 768px) {
        .responsive-toast {
            width: 400px !important;
        }
    }
</style>
```

---

## Advanced Patterns

### Notification Queue

```razor
@code {
    private Queue<NotificationMessage> notificationQueue = new();
    private bool isShowingToast = false;

    private async Task EnqueueNotification(string title, string content)
    {
        notificationQueue.Enqueue(new NotificationMessage { Title = title, Content = content });
        
        if (!isShowingToast)
        {
            await ProcessNotificationQueue();
        }
    }

    private async Task ProcessNotificationQueue()
    {
        isShowingToast = true;

        while (notificationQueue.Count > 0)
        {
            var notification = notificationQueue.Dequeue();
            ToastObj.Title = notification.Title;
            ToastObj.Content = notification.Content;
            await ToastObj.ShowAsync();
            
            await Task.Delay(ToastObj.Timeout + 500); // Wait for toast to auto-hide + buffer
        }

        isShowingToast = false;
    }

    private class NotificationMessage
    {
        public string Title { get; set; }
        public string Content { get; set; }
    }
}
```

### Extended Timeout on Hover

```razor
<SfToast @ref="ToastObj" 
         Title="Hover Me" 
         Content="Hover to extend display time"
         Timeout="3000"
         ExtendedTimeout="10000" />

@code {
    // ExtendedTimeout: Duration to show when mouse is over toast
    // Timeout: Normal duration
}
```

---

## Next Steps

- Learn about [Message Component](message-implementation.md) for inline alerts
- Explore [Skeleton Component](skeleton-implementation.md) for loading states
- Review [Styling Guide](styling-and-themes.md) for advanced customization
- See [Use Cases](use-cases-and-examples.md) for complete scenarios
