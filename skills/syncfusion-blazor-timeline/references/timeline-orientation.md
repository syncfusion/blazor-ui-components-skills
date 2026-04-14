# Timeline Orientation

## Table of Contents
- [Overview](#overview)
- [Vertical Orientation](#vertical-orientation)
- [Horizontal Orientation](#horizontal-orientation)
- [Combining with Alignment](#combining-with-alignment)
- [Responsive Considerations](#responsive-considerations)

## Overview

The `Orientation` property controls the direction in which Timeline items are displayed. The Timeline component supports both vertical and horizontal layouts to accommodate different design requirements.

Two orientation options:
- **Vertical**: Items displayed sequentially from top to bottom (default)
- **Horizontal**: Items displayed side-by-side from left to right

## Vertical Orientation

Vertical orientation displays items sequentially from top to bottom. This is the default orientation and works well for mobile devices and narrow containers.

```razor
@using Syncfusion.Blazor.Layouts

<div class="container" style="height: 350px">
    <SfTimeline Orientation=TimelineOrientation.Vertical>
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem>
                    <Content>
                        @item.Content
                    </Content>
                    <OppositeContent>
                        @item.OppositeContent
                    </OppositeContent>
                </TimelineItem>
            }
        </TimelineItems>
    </SfTimeline>
</div>

@code {
    public class TimelineItemModel
    {
        public string Content { get; set; }
        public string OppositeContent { get; set; }
    }
    private List<TimelineItemModel> timelineItems = new List<TimelineItemModel>()
    {
        new TimelineItemModel() { Content = "Day 1, 4:00 PM", OppositeContent = "Check-in and campsite visit" },
        new TimelineItemModel() { Content = "Day 1, 7:00 PM", OppositeContent = "Dinner with music" },
        new TimelineItemModel() { Content = "Day 2, 5:30 AM", OppositeContent = "Sunrise between mountains" },
        new TimelineItemModel() { Content = "Day 2, 8:00 AM", OppositeContent = "Breakfast and check-out" }
    };
}
```

**When to use**:
- Mobile and tablet displays
- Long sequences of events
- When space width is limited
- Default and most compatible option

**Alignment behavior in vertical mode**:
- **Before**: Content on left, OppositeContent on right
- **After**: Content on right, OppositeContent on left
- **Alternate**: Content alternates left/right
- **AlternateReverse**: Content alternates right/left

## Horizontal Orientation

Horizontal orientation displays items side-by-side from left to right. This layout works best for shorter sequences and wider screens.

```razor
@using Syncfusion.Blazor.Layouts

<SfTimeline Orientation=TimelineOrientation.Horizontal>
    <TimelineItems>
        @foreach (var item in timelineItems)
        {
            <TimelineItem>
                <Content>
                    @item.Content
                </Content>
                <OppositeContent>
                    @item.OppositeContent
                </OppositeContent>
            </TimelineItem>
        }
    </TimelineItems>
</SfTimeline>

@code {
    public class TimelineItemModel
    {
        public string Content { get; set; }
        public string OppositeContent { get; set; }
    }
    private List<TimelineItemModel> timelineItems = new List<TimelineItemModel>()
    {
        new TimelineItemModel() { Content = "Day 1, 4:00 PM", OppositeContent = "Check-in and campsite visit" },
        new TimelineItemModel() { Content = "Day 1, 7:00 PM", OppositeContent = "Dinner with music" },
        new TimelineItemModel() { Content = "Day 2, 5:30 AM", OppositeContent = "Sunrise between mountains" },
        new TimelineItemModel() { Content = "Day 2, 8:00 AM", OppositeContent = "Breakfast and check-out" }
    };
}
```

**When to use**:
- Desktop and wide-screen displays
- Short sequences (3-6 items recommended)
- Project phases or workflow steps
- When horizontal space is available

**Alignment behavior in horizontal mode**:
- **Before**: Content above, OppositeContent below
- **After**: Content below, OppositeContent above
- **Alternate**: Content alternates top/bottom
- **AlternateReverse**: Content alternates bottom/top

## Combining with Alignment

The orientation and alignment work together to create different layouts. Here's how they interact:

### Vertical + Before Alignment
```razor
<SfTimeline Orientation=TimelineOrientation.Vertical Alignment=TimelineAlignment.Before>
    <!-- Content on left, OppositeContent on right -->
</SfTimeline>
```

### Vertical + Alternate Alignment
```razor
<SfTimeline Orientation=TimelineOrientation.Vertical Alignment=TimelineAlignment.Alternate>
    <!-- Content alternates left/right -->
</SfTimeline>
```

### Horizontal + Before Alignment
```razor
<SfTimeline Orientation=TimelineOrientation.Horizontal Alignment=TimelineAlignment.Before>
    <!-- Content above, OppositeContent below -->
</SfTimeline>
```

### Horizontal + Alternate Alignment
```razor
<SfTimeline Orientation=TimelineOrientation.Horizontal Alignment=TimelineAlignment.Alternate>
    <!-- Content alternates top/bottom -->
</SfTimeline>
```

## Responsive Considerations

For responsive designs, consider switching orientation based on screen width:

```razor
@using Syncfusion.Blazor.Layouts

<div class="responsive-timeline">
    <SfTimeline Orientation="@selectedOrientation">
        <TimelineItems>
            @foreach (var item in timelineItems)
            {
                <TimelineItem>
                    <Content> @item.Content </Content>
                    <OppositeContent> @item.OppositeContent </OppositeContent>
                </TimelineItem>
            }
        </TimelineItems>
    </SfTimeline>
</div>

@code {
    private TimelineOrientation selectedOrientation = TimelineOrientation.Vertical;

    protected override void OnInitialized()
    {
        // Set orientation based on window width
        // In production, use JavaScript interop or media queries
        selectedOrientation = TimelineOrientation.Vertical;
    }

    public class TimelineItemModel
    {
        public string Content { get; set; }
        public string OppositeContent { get; set; }
    }

    private List<TimelineItemModel> timelineItems = new List<TimelineItemModel>()
    {
        new TimelineItemModel() { Content = "Phase 1", OppositeContent = "Q1 2024" },
        new TimelineItemModel() { Content = "Phase 2", OppositeContent = "Q2 2024" },
        new TimelineItemModel() { Content = "Phase 3", OppositeContent = "Q3 2024" }
    };
}
```

**Recommendations**:
- Mobile (< 768px): Use Vertical orientation
- Tablet (768px - 1024px): Use Vertical, allow horizontal for longer sequences
- Desktop (> 1024px): Horizontal can work, choose based on sequence length

## Default Orientation

The default orientation is **Vertical**. If you don't specify the `Orientation` property, items will display vertically from top to bottom.

```razor
<SfTimeline>
    <!-- Defaults to Vertical orientation -->
    <TimelineItems>
        <!-- ... -->
    </TimelineItems>
</SfTimeline>
```

## Layout Comparison

| Feature | Vertical | Horizontal |
|---------|----------|-----------|
| Default | Yes | - |
| Best for | Long sequences, mobile | Short sequences, desktop |
| Max items | Unlimited | 4-6 recommended |
| Space requirement | Height-based | Width-based |
| Mobile friendly | Yes | Limited |
| Desktop friendly | Yes | Yes |

## Key Properties

- `Orientation="TimelineOrientation.Vertical|Horizontal"` - Sets the layout direction
- Works in combination with `Alignment` property
- Affects how content and opposite content are positioned
