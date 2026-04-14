# Timeline Events

## Table of Contents
- [Overview](#overview)
- [Created Event](#created-event)
- [ItemRendered Event](#itemrendered-event)
- [Event Handling Patterns](#event-handling-patterns)

## Overview

The Timeline component triggers events at specific points in its lifecycle. These events allow you to execute custom code and react to component state changes.

Supported events:
- **Created**: Fires when the Timeline component rendering is completed
- **ItemRendered**: Fires after each Timeline item is rendered

## Created Event

The `Created` event is triggered when the Timeline component finishes rendering. This is useful for initialization tasks that need to happen after the component is ready.

### Basic Usage

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 350px">
    <SfTimeline Created="TimelineCreated">
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem>
                    <Content> @item.Content </Content>
                </TimelineItem>
            }
        </TimelineItems>
    </SfTimeline>
</div>

@code {
    private void TimelineCreated()
    {
        Console.WriteLine("Timeline component has been created successfully.");
        // Perform initialization tasks here
    }

    public class TimelineItemModel
    {
        public string Content { get; set; }
    }

    private List<TimelineItemModel> timelineItems = new List<TimelineItemModel>()
    {
        new TimelineItemModel() { Content = "Planning" },
        new TimelineItemModel() { Content = "Developing" },
        new TimelineItemModel() { Content = "Testing" },
        new TimelineItemModel() { Content = "Launch" }
    };
}
```

### When to Use

- Initialize related components
- Load additional data
- Log analytics events
- Trigger animations or transitions
- Set up event listeners on the DOM
- Update parent component state

### Example with Side Effects

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 350px">
    <p>@statusMessage</p>
    <SfTimeline Created="OnTimelineCreated">
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem>
                    <Content> @item.Content </Content>
                </TimelineItem>
            }
        </TimelineItems>
    </SfTimeline>
</div>

@code {
    private string statusMessage = "Loading...";

    private void OnTimelineCreated()
    {
        statusMessage = "Timeline ready with " + timelineItems.Count + " items";
        Console.WriteLine("Timeline initialized at: " + DateTime.Now);
    }

    public class TimelineItemModel
    {
        public string Content { get; set; }
    }

    private List<TimelineItemModel> timelineItems = new List<TimelineItemModel>()
    {
        new TimelineItemModel() { Content = "Planning" },
        new TimelineItemModel() { Content = "Developing" },
        new TimelineItemModel() { Content = "Testing" },
        new TimelineItemModel() { Content = "Launch" }
    };
}
```

## ItemRendered Event

The `ItemRendered` event is triggered after each individual Timeline item is rendered. This is useful for applying custom styling or logic to specific items.

### Event Arguments

The `ItemRendered` event receives `TimelineRenderedEventArgs` with the following information:
- Event information and context

### Basic Usage

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 350px">
    <SfTimeline ItemRendered="TimelineItemRendered">
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem>
                    <Content> @item.Content </Content>
                </TimelineItem>
            }
        </TimelineItems>
    </SfTimeline>
</div>

@code {
    private void TimelineItemRendered(TimelineRenderedEventArgs args)
    {
        // Custom logic for each rendered item
        Console.WriteLine("Item rendered");
    }

    public class TimelineItemModel
    {
        public string Content { get; set; }
    }

    private List<TimelineItemModel> timelineItems = new List<TimelineItemModel>()
    {
        new TimelineItemModel() { Content = "Planning" },
        new TimelineItemModel() { Content = "Developing" },
        new TimelineItemModel() { Content = "Testing" },
        new TimelineItemModel() { Content = "Launch" }
    };
}
```

### Tracking Rendered Items

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 350px">
    <p>Items rendered: @renderedCount</p>
    <SfTimeline ItemRendered="OnItemRendered">
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem>
                    <Content> @item.Content </Content>
                </TimelineItem>
            }
        </TimelineItems>
    </SfTimeline>
</div>

@code {
    private int renderedCount = 0;

    private void OnItemRendered(TimelineRenderedEventArgs args)
    {
        renderedCount++;
        Console.WriteLine($"Rendered {renderedCount} items");
    }

    public class TimelineItemModel
    {
        public string Content { get; set; }
    }

    private List<TimelineItemModel> timelineItems = new List<TimelineItemModel>()
    {
        new TimelineItemModel() { Content = "Planning" },
        new TimelineItemModel() { Content = "Developing" },
        new TimelineItemModel() { Content = "Testing" },
        new TimelineItemModel() { Content = "Launch" }
    };
}
```

## Event Handling Patterns

### Pattern 1: Simple Event Handler

```razor
<SfTimeline Created="OnCreated">
    <!-- content -->
</SfTimeline>

@code {
    private void OnCreated()
    {
        // Handle event
    }
}
```

### Pattern 2: Event with Arguments

```razor
<SfTimeline ItemRendered="OnItemRendered">
    <!-- content -->
</SfTimeline>

@code {
    private void OnItemRendered(TimelineRenderedEventArgs args)
    {
        // Access event arguments
    }
}
```

### Pattern 3: Async Event Handler

```razor
<SfTimeline Created="OnCreatedAsync">
    <!-- content -->
</SfTimeline>

@code {
    private async Task OnCreatedAsync()
    {
        // Perform async operations
        await Task.Delay(1000);
        Console.WriteLine("Timeline created after delay");
    }
}
```

### Pattern 4: Multiple Events

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 350px">
    <p>@eventLog</p>
    <SfTimeline Created="OnCreated" ItemRendered="OnItemRendered">
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem>
                    <Content> @item.Content </Content>
                </TimelineItem>
            }
        </TimelineItems>
    </SfTimeline>
</div>

@code {
    private string eventLog = "";

    private void OnCreated()
    {
        eventLog += "Created event fired\n";
    }

    private void OnItemRendered(TimelineRenderedEventArgs args)
    {
        eventLog += "Item rendered event fired\n";
    }

    public class TimelineItemModel
    {
        public string Content { get; set; }
    }

    private List<TimelineItemModel> timelineItems = new List<TimelineItemModel>()
    {
        new TimelineItemModel() { Content = "Planning" },
        new TimelineItemModel() { Content = "Developing" },
        new TimelineItemModel() { Content = "Testing" },
        new TimelineItemModel() { Content = "Launch" }
    };
}
```

## Event Lifecycle

Timeline events fire in this order:

1. Component initialization
2. **Created** event fires (once per component lifecycle)
3. Items begin rendering
4. **ItemRendered** event fires (once per item)
5. All items rendered, component fully ready

## Common Use Cases

### Initialize Data After Timeline Loads
```csharp
private async Task OnCreatedAsync()
{
    // Load additional data from service
    var data = await dataService.GetTimelineData();
    // Update component state
}
```

### Track Item Rendering Progress
```csharp
private void OnItemRendered(TimelineRenderedEventArgs args)
{
    renderedCount++;
    if (renderedCount == totalItems)
    {
        Console.WriteLine("All items rendered!");
    }
}
```

### Apply Dynamic Styling
```csharp
private void OnItemRendered(TimelineRenderedEventArgs args)
{
    // Apply dynamic CSS or logic to rendered items
}
```

## Event Reference

| Event | Handler Signature | When It Fires | Use Case |
|-------|---|---|---|
| Created | `Task OnCreated()` or `void OnCreated()` | Once, after component renders | Initialization |
| ItemRendered | `void OnItemRendered(TimelineRenderedEventArgs args)` | Once per item, as each renders | Item-level logic, tracking |
