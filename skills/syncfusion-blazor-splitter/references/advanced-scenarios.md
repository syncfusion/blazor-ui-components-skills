# Advanced Scenarios and Layouts

## Table of Contents
- [Code Editor Layout](#code-editor-layout)
- [Outlook Style Layout](#outlook-style-layout)
- [Dashboard Layout](#dashboard-layout)
- [Performance Tips](#performance-tips)
- [Nested Splitters Best Practices](#nested-splitters-best-practices)
- [Common Issues](#common-issues)

## Code Editor Layout

Create an IDE-style interface with nested horizontal and vertical splitters for HTML/CSS/JS + preview.

### Complete Code Editor Example

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="600px" Width="100%" Orientation="Orientation.Vertical" CssClass="code-editor-splitter">
    <SplitterPanes>
        <!-- Top Pane: Three-Column Editor -->
        <SplitterPane Size="60%" Min="30%">
            <SfSplitter Height="100%" Width="100%">
                <SplitterPanes>
                    <SplitterPane Size="29%" Min="23%">
                        <ContentTemplate>
                            <div class="code-section">
                                <h3>HTML</h3>
                                <pre>&lt;!DOCTYPE html&gt;
&lt;html&gt;
  &lt;head&gt;
    &lt;title&gt;Sample&lt;/title&gt;
  &lt;/head&gt;
  &lt;body&gt;
    &lt;h1&gt;Hello World&lt;/h1&gt;
  &lt;/body&gt;
&lt;/html&gt;</pre>
                            </div>
                        </ContentTemplate>
                    </SplitterPane>
                    <SplitterPane Size="20%" Min="15%">
                        <ContentTemplate>
                            <div class="code-section">
                                <h3>CSS</h3>
                                <pre>body {
  font-family: Arial;
  background: #f0f0f0;
  margin: 0;
  padding: 20px;
}

h1 {
  color: #333;
  text-align: center;
}</pre>
                            </div>
                        </ContentTemplate>
                    </SplitterPane>
                    <SplitterPane Size="35%" Min="35%">
                        <ContentTemplate>
                            <div class="code-section">
                                <h3>JavaScript</h3>
                                <pre>document.addEventListener('DOMContentLoaded', 
  function() {
    console.log('Page loaded');
    const heading = document.querySelector('h1');
    heading.style.fontSize = '2em';
});

function updateContent() {
  console.log('Content updated');
}</pre>
                            </div>
                        </ContentTemplate>
                    </SplitterPane>
                </SplitterPanes>
            </SfSplitter>
        </SplitterPane>
        
        <!-- Bottom Pane: Preview -->
        <SplitterPane Size="40%" Min="30%">
            <ContentTemplate>
                <div class="preview-section">
                    <h3>Preview</h3>
                    <div class="preview-content">
                        <h1>Hello World</h1>
                        <p>This is the rendered preview of your code</p>
                    </div>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

<style>
    .code-editor-splitter .e-pane {
        overflow: auto;
    }
    
    .code-section {
        padding: 15px;
        background: #f5f5f5;
        height: 100%;
        overflow: auto;
    }
    
    .code-section h3 {
        margin-top: 0;
        background: #333;
        color: white;
        padding: 10px;
        margin: -15px -15px 10px -15px;
    }
    
    .code-section pre {
        background: white;
        border: 1px solid #ddd;
        padding: 10px;
        border-radius: 4px;
        font-family: 'Courier New', monospace;
        font-size: 12px;
        overflow: auto;
    }
    
    .preview-section {
        padding: 20px;
        background: white;
        height: 100%;
        overflow: auto;
    }
    
    .preview-content {
        border: 1px solid #ddd;
        padding: 20px;
        border-radius: 4px;
    }
</style>
```

**Structure**:
- Vertical splitter: top (60%) = editors, bottom (40%) = preview
- Horizontal splitter in top pane: left (29%) = HTML, center (20%) = CSS, right (35%) = JS

## Outlook Style Layout

Create a multi-panel dashboard similar to Outlook with navigation, list, and details.

### Outlook-Style Dashboard

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="600px" Width="100%" CssClass="outlook-layout">
    <SplitterPanes>
        <!-- Left: Navigation Panel -->
        <SplitterPane Size="20%" Min="150px" Collapsible="true">
            <ContentTemplate>
                <div class="panel-section">
                    <h3>Folders</h3>
                    <ul class="folder-list">
                        <li>📧 Inbox</li>
                        <li>📤 Sent</li>
                        <li>📋 Drafts</li>
                        <li>🗑️ Deleted</li>
                        <li>⭐ Starred</li>
                    </ul>
                </div>
            </ContentTemplate>
        </SplitterPane>
        
        <!-- Center: List View -->
        <SplitterPane Size="35%" Min="250px">
            <ContentTemplate>
                <div class="panel-section">
                    <h3>Messages</h3>
                    <div class="message-list">
                        <div class="message-item">
                            <strong>From: John Smith</strong>
                            <p>Subject: Meeting Tomorrow</p>
                            <span class="date">Today 10:30 AM</span>
                        </div>
                        <div class="message-item">
                            <strong>From: Jane Doe</strong>
                            <p>Subject: Project Update</p>
                            <span class="date">Today 9:15 AM</span>
                        </div>
                        <div class="message-item">
                            <strong>From: Bob Wilson</strong>
                            <p>Subject: RE: New Feature</p>
                            <span class="date">Yesterday 4:45 PM</span>
                        </div>
                    </div>
                </div>
            </ContentTemplate>
        </SplitterPane>
        
        <!-- Right: Details View -->
        <SplitterPane Size="45%" Min="300px">
            <ContentTemplate>
                <div class="panel-section">
                    <h3>Message Details</h3>
                    <div class="message-details">
                        <div class="detail-row">
                            <strong>From:</strong> John Smith
                        </div>
                        <div class="detail-row">
                            <strong>To:</strong> me@example.com
                        </div>
                        <div class="detail-row">
                            <strong>Subject:</strong> Meeting Tomorrow
                        </div>
                        <div class="detail-row">
                            <strong>Date:</strong> Today 10:30 AM
                        </div>
                        <hr />
                        <div class="message-body">
                            <p>Hi there,</p>
                            <p>I wanted to confirm our meeting tomorrow at 2 PM.</p>
                            <p>Looking forward to discussing the project.</p>
                            <p>Best regards,<br/>John</p>
                        </div>
                    </div>
                </div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

<style>
    .outlook-layout .panel-section {
        background: white;
        height: 100%;
        overflow: auto;
        border-right: 1px solid #ddd;
    }
    
    .outlook-layout h3 {
        margin: 0;
        padding: 15px;
        background: #f5f5f5;
        border-bottom: 1px solid #ddd;
        font-size: 14px;
    }
    
    .folder-list {
        list-style: none;
        padding: 0;
        margin: 0;
    }
    
    .folder-list li {
        padding: 12px 15px;
        border-bottom: 1px solid #f0f0f0;
        cursor: pointer;
    }
    
    .folder-list li:hover {
        background: #f9f9f9;
    }
    
    .message-list {
        padding: 10px;
    }
    
    .message-item {
        padding: 15px;
        border-bottom: 1px solid #f0f0f0;
        cursor: pointer;
        border-radius: 4px;
        margin-bottom: 5px;
    }
    
    .message-item:hover {
        background: #f9f9f9;
    }
    
    .message-item strong {
        display: block;
        margin-bottom: 5px;
    }
    
    .message-item p {
        margin: 5px 0;
        color: #666;
        font-size: 13px;
    }
    
    .date {
        font-size: 11px;
        color: #999;
    }
    
    .message-details {
        padding: 20px;
    }
    
    .detail-row {
        margin-bottom: 15px;
    }
    
    .message-body {
        margin-top: 20px;
        line-height: 1.6;
    }
</style>
```

## Dashboard Layout

Responsive multi-panel dashboard with resizable sections.

```csharp
@using Syncfusion.Blazor.Layouts

<SfSplitter Height="500px" Width="100%" Orientation="Orientation.Vertical">
    <!-- Top: Metrics -->
    <SplitterPane Size="15%" Min="80px">
        <ContentTemplate>
            <div class="dashboard-section">
                <h3>Key Metrics</h3>
                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px; padding: 10px;">
                    <div class="metric"><strong>1,234</strong><br/>Sales</div>
                    <div class="metric"><strong>567</strong><br/>Users</div>
                    <div class="metric"><strong>89%</strong><br/>Uptime</div>
                    <div class="metric"><strong>$45K</strong><br/>Revenue</div>
                </div>
            </div>
        </ContentTemplate>
    </SplitterPane>
    
    <!-- Middle: Charts and Data -->
    <SplitterPane Size="70%" Min="250px">
        <SfSplitter Height="100%" Width="100%">
            <SplitterPanes>
                <SplitterPane Size="50%" Min="200px">
                    <ContentTemplate>
                        <div class="dashboard-section">
                            <h3>Sales Trend</h3>
                            <div style="height: 100%; display: flex; align-items: center; justify-content: center; background: #f9f9f9;">
                                [Chart Component Would Go Here]
                            </div>
                        </div>
                    </ContentTemplate>
                </SplitterPane>
                <SplitterPane Size="50%" Min="200px">
                    <ContentTemplate>
                        <div class="dashboard-section">
                            <h3>User Distribution</h3>
                            <div style="height: 100%; display: flex; align-items: center; justify-content: center; background: #f9f9f9;">
                                [Chart Component Would Go Here]
                            </div>
                        </div>
                    </ContentTemplate>
                </SplitterPane>
            </SplitterPanes>
        </SfSplitter>
    </SplitterPane>
    
    <!-- Bottom: Logs -->
    <SplitterPane Size="15%" Min="80px">
        <ContentTemplate>
            <div class="dashboard-section">
                <h3>Recent Activity</h3>
                <div style="overflow: auto; height: calc(100% - 45px); padding: 10px;">
                    <div class="log-item">User registered - 2 min ago</div>
                    <div class="log-item">Payment processed - 5 min ago</div>
                    <div class="log-item">Report generated - 8 min ago</div>
                </div>
            </div>
        </ContentTemplate>
    </SplitterPane>
</SfSplitter>

<style>
    .dashboard-section {
        background: white;
        height: 100%;
        border: 1px solid #ddd;
        display: flex;
        flex-direction: column;
    }
    
    .dashboard-section h3 {
        margin: 0;
        padding: 12px;
        background: #007bff;
        color: white;
        border-bottom: 1px solid #ddd;
    }
    
    .metric {
        background: white;
        padding: 15px;
        border: 1px solid #ddd;
        border-radius: 4px;
        text-align: center;
    }
    
    .metric strong {
        font-size: 20px;
        color: #007bff;
    }
    
    .log-item {
        padding: 8px;
        border-bottom: 1px solid #f0f0f0;
        font-size: 12px;
        color: #666;
    }
</style>
```

## Performance Tips

### 1. Avoid Too Many Nested Splitters

```csharp
// ❌ Bad: Deep nesting (3+ levels)
<SfSplitter>
    <SplitterPane>
        <SfSplitter>
            <SplitterPane>
                <SfSplitter>
                    <!-- Deep nesting hurts performance -->
                </SfSplitter>
            </SplitterPane>
        </SfSplitter>
    </SplitterPane>
</SfSplitter>

// ✅ Good: 2 levels maximum
<SfSplitter>
    <SplitterPane>
        <SfSplitter>
            <!-- OK: 2 levels is acceptable -->
        </SfSplitter>
    </SplitterPane>
</SfSplitter>
```

### 2. Lazy Load Content

```csharp
<SfSplitter Height="400px" Width="100%">
    <SplitterPanes>
        <SplitterPane Size="30%" Collapsible="true">
            <ContentTemplate>
                <div>Navigation - Always Loaded</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="70%">
            <ContentTemplate>
                @if (showDetailPane)
                {
                    <DetailComponent /> <!-- Only load when needed -->
                }
                else
                {
                    <div>Click to load details</div>
                }
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

### 3. Virtualization for Large Lists

```csharp
<SplitterPane>
    <ContentTemplate>
        @if (Messages != null && Messages.Count > 0)
        {
            <!-- Use Virtualize for large lists -->
            <Virtualize Items="@Messages">
                <MessageItem Message="@context" />
            </Virtualize>
        }
    </ContentTemplate>
</SplitterPane>
```

### 4. Debounce Resize Events

```csharp
@using System.Timers;

<SfSplitter>
    <SplitterEvents OnResizeStop="@OnResizeStopDebounced" />
    <!-- Panes -->
</SfSplitter>

@code {
    private Timer resizeTimer;
    private bool isResizing = false;

    private void OnResizeStopDebounced(ResizingEventArgs args)
    {
        if (isResizing) return;
        isResizing = true;
        
        resizeTimer = new Timer(500);
        resizeTimer.Elapsed += (s, e) =>
        {
            RefreshContent();
            isResizing = false;
            resizeTimer.Stop();
        };
        resizeTimer.Start();
    }

    private void RefreshContent()
    {
        // Expensive operation
    }
}
```

## Nested Splitters Best Practices

### ✅ DO

- Use clear naming: `OuterSplitter`, `InnerSplitter`, etc.
- Set both `Height="100%"` and `Width="100%"` on nested splitters
- Provide fixed sizes to parent panes
- Use `Min` property to prevent panes from becoming too small
- Test performance with real content

### ❌ DON'T

- Nest more than 2-3 levels deep
- Omit height/width on nested splitters
- Create splitters inside loops
- Mix multiple horizontal/vertical orientations without planning
- Use arbitrary pane sizes

## Common Issues

### Issue 1: Nested Splitter Doesn't Resize Properly

**Cause**: Missing `Height="100%"` or `Width="100%"` on inner splitter

**Fix**:
```csharp
<!-- ❌ Wrong -->
<SplitterPane>
    <SfSplitter>
        <!-- Inner splitter won't take full space -->
    </SfSplitter>
</SplitterPane>

<!-- ✅ Correct -->
<SplitterPane>
    <SfSplitter Height="100%" Width="100%">
        <!-- Takes full pane space -->
    </SfSplitter>
</SplitterPane>
```

### Issue 2: Content Overflows Pane

**Cause**: Pane content doesn't respect container height

**Fix**:
```csharp
<SplitterPane>
    <ContentTemplate>
        <div style="height: 100%; overflow: auto;">
            <!-- Content now scrolls within pane -->
        </div>
    </ContentTemplate>
</SplitterPane>
```

### Issue 3: Collapse Button Disappears

**Cause**: `Collapsible="true"` not set on pane

**Fix**:
```csharp
<!-- ❌ Wrong -->
<SplitterPane>
    <!-- No collapse button -->
</SplitterPane>

<!-- ✅ Correct -->
<SplitterPane Collapsible="true">
    <!-- Has collapse button -->
</SplitterPane>
```

### Issue 4: Resize Performance Issues

**Cause**: Heavy operations on every resize event (especially `Resizing` event)

**Fix**: Use `OnResizeStop` instead of `Resizing`, or debounce resize events

---

**Congratulations!** You now have comprehensive knowledge of Syncfusion Blazor Splitter. For additional help, check the [official documentation](url).
