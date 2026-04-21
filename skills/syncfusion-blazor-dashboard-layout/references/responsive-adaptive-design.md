# Responsive and Adaptive Design

## Table of Contents
- [Responsive Overview](#responsive-overview)
- [Default Responsive Behavior](#default-responsive-behavior)
- [Custom Breakpoints](#custom-breakpoints)
- [Mobile Optimization](#mobile-optimization)
- [Practical Responsive Examples](#practical-responsive-examples)

## Responsive Overview

The Dashboard Layout component has built-in responsive support that automatically adapts the layout based on the viewport size without requiring manual responsive configuration. The component intelligently transforms between multi-column layouts and simplified stacked layouts depending on screen width.

## Default Responsive Behavior

### Automatic Layout Transformation

By default, layouts automatically stack into a single column when the screen width is **600px or lower**:

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout CellSpacing="@(new double[]{20, 20})" Columns="5">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <ContentTemplate><div>0</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeX=2 SizeY=2 Column=1>
            <ContentTemplate><div>1</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeY=2 Column=3>
            <ContentTemplate><div>2</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Row=1>
            <ContentTemplate><div>3</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .e-panel-content {
        text-align: center;
        margin-top: 10px;
    }
</style>
```

**Default Behavior:**
- **Desktop (>600px):** Full multi-column layout with all panels displayed side-by-side
- **Mobile (≤600px):** Automatic single-column stacking with panels displayed vertically
- **No configuration needed:** Works out-of-the-box

## Custom Breakpoints

### MediaQuery Property

Use the `MediaQuery` property to customize the responsive breakpoint for any desired resolution:

```csharp
@using Syncfusion.Blazor.Layouts

<!-- Stack panels at 700px instead of 600px -->
<SfDashboardLayout 
    CellSpacing="@(new double[]{20, 20})" 
    Columns="5" 
    MediaQuery="max-width:700px">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel>
            <ContentTemplate><div>0</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeX=2 SizeY=2 Column=1>
            <ContentTemplate><div>1</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel SizeY=2 Column=3>
            <ContentTemplate><div>2</div></ContentTemplate>
        </DashboardLayoutPanel>
        <DashboardLayoutPanel Row=1>
            <ContentTemplate><div>3</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .e-panel-content {
        text-align: center;
        margin-top: 10px;
    }
</style>
```

### MediaQuery Syntax

`MediaQuery` accepts standard CSS media query syntax:

```csharp
<!-- Stack below tablet size -->
MediaQuery="max-width:768px"

<!-- Stack on smaller tablets -->
MediaQuery="max-width:600px"

<!-- Stack on very small screens -->
MediaQuery="max-width:480px"

<!-- Stack above desktop size -->
MediaQuery="min-width:1200px"

<!-- Stack in specific device range -->
MediaQuery="(min-width:600px) and (max-width:900px)"
```

### Common Responsive Breakpoints

| Device Type | Breakpoint | MediaQuery |
|-------------|-----------|-----------|
| Large Desktop | >1200px | `min-width:1200px` |
| Desktop | 992px-1199px | None (default) |
| Tablet (landscape) | 768px-991px | `max-width:991px` |
| Tablet (portrait) | 600px-767px | `max-width:767px` |
| Mobile | <600px | `max-width:600px` (default) |

## Mobile Optimization

### Tablet-Friendly Layout

```csharp
@using Syncfusion.Blazor.Layouts

<!-- Stack at tablet size (768px) -->
<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    Columns="5" 
    MediaQuery="max-width:768px">
    <DashboardLayoutPanels>
        <DashboardLayoutPanel SizeX=2>
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
            <ContentTemplate><div>Content</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=2 Column=2>
            <HeaderTemplate><div>Panel 2</div></HeaderTemplate>
            <ContentTemplate><div>Content</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=2 Column=4>
            <HeaderTemplate><div>Panel 3</div></HeaderTemplate>
            <ContentTemplate><div>Content</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .e-panel-header {
        background-color: rgba(0, 0, 0, .1);
        text-align: center;
        padding: 10px;
    }
    .e-panel-content {
        text-align: center;
        margin-top: 10px;
    }
</style>
```

**What Happens at Breakpoint:**
- Layout switches from multi-column to single column
- All panels stack vertically
- Full width utilization on mobile
- Touch-friendly single-column interaction
- Improved readability on small screens

### Mobile-First Approach

```csharp
@using Syncfusion.Blazor.Layouts

<!-- Stack becomes standard for mobile, expands on larger screens -->
<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    Columns="1" 
    MediaQuery="max-width:600px">
    <DashboardLayoutPanels>
        <!-- On mobile (≤600px): Single column (Columns=1) -->
        <!-- On desktop (>600px): Changes to multi-column layout -->
        <DashboardLayoutPanel>
            <HeaderTemplate><div>Panel 1</div></HeaderTemplate>
            <ContentTemplate><div>Full width on mobile</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel>
            <HeaderTemplate><div>Panel 2</div></HeaderTemplate>
            <ContentTemplate><div>Stacks vertically</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel>
            <HeaderTemplate><div>Panel 3</div></HeaderTemplate>
            <ContentTemplate><div>Touch-friendly spacing</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .e-panel-header {
        padding: 12px;
        font-weight: bold;
    }
    .e-panel-content {
        padding: 15px;
    }
</style>
```

## Practical Responsive Examples

### Example 1: News Dashboard with Device-Based Layouts

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{15, 15})" 
    Columns="6"
    MediaQuery="max-width:768px">
    <DashboardLayoutPanels>
        <!-- Featured Story - Full width on mobile -->
        <DashboardLayoutPanel SizeX=6>
            <HeaderTemplate><div>Featured Story</div></HeaderTemplate>
            <ContentTemplate>
                <div>Main article or breaking news</div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Story Grid - 2 columns on desktop, stacks on mobile -->
        <DashboardLayoutPanel SizeX=3 Row=1>
            <HeaderTemplate><div>Story 1</div></HeaderTemplate>
            <ContentTemplate><div>Article preview</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=3 Row=1 Column=3>
            <HeaderTemplate><div>Story 2</div></HeaderTemplate>
            <ContentTemplate><div>Article preview</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Sidebar - Moves below on mobile -->
        <DashboardLayoutPanel SizeX=2 Row=2>
            <HeaderTemplate><div>Categories</div></HeaderTemplate>
            <ContentTemplate><div>Category list</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=2 Row=2 Column=2>
            <HeaderTemplate><div>Popular</div></HeaderTemplate>
            <ContentTemplate><div>Trending articles</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=2 Row=2 Column=4>
            <HeaderTemplate><div>Videos</div></HeaderTemplate>
            <ContentTemplate><div>Video thumbnails</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .e-panel-header {
        background-color: #334099;
        color: white;
        padding: 12px;
        font-weight: bold;
    }
    .e-panel-content {
        padding: 15px;
        min-height: 100px;
    }
</style>
```

### Example 2: Analytics Dashboard with Progressive Disclosure

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{10, 10})" 
    Columns="4"
    MediaQuery="max-width:800px">
    <DashboardLayoutPanels>
        <!-- Key Metrics - Always prominent -->
        <DashboardLayoutPanel SizeX=4>
            <HeaderTemplate><div>Key Metrics</div></HeaderTemplate>
            <ContentTemplate>
                <div style="display:flex; gap:10px;">
                    <div style="flex:1;">Revenue: $125K</div>
                    <div style="flex:1;">Users: 5.2K</div>
                    <div style="flex:1;">Sessions: 12K</div>
                    <div style="flex:1;">Bounce: 35%</div>
                </div>
            </ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Main Chart - Full width on mobile -->
        <DashboardLayoutPanel SizeX=4 Row=1 SizeY=2>
            <HeaderTemplate><div>Traffic Over Time</div></HeaderTemplate>
            <ContentTemplate><div>Chart visualization</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Detailed Analytics - Side by side on desktop, stacks on mobile -->
        <DashboardLayoutPanel SizeX=2 Row=3>
            <HeaderTemplate><div>Top Pages</div></HeaderTemplate>
            <ContentTemplate><div>Page list with metrics</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=2 Row=3 Column=2>
            <HeaderTemplate><div>Referrers</div></HeaderTemplate>
            <ContentTemplate><div>Referrer sources</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Mobile Report - Full width -->
        <DashboardLayoutPanel SizeX=4 Row=4>
            <HeaderTemplate><div>Device Breakdown</div></HeaderTemplate>
            <ContentTemplate><div>Mobile/Desktop/Tablet split</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .e-panel-header {
        background-color: #f5f5f5;
        padding: 10px;
        font-weight: bold;
        border-bottom: 1px solid #ddd;
    }
    .e-panel-content {
        padding: 15px;
    }
</style>
```

### Example 3: Admin Dashboard with Progressive Enhancement

```csharp
@using Syncfusion.Blazor.Layouts

<SfDashboardLayout 
    CellSpacing="@(new double[]{12, 12})" 
    Columns="12"
    MediaQuery="max-width:992px">
    <DashboardLayoutPanels>
        <!-- Header Stats - Always visible, responsive wrapping -->
        <DashboardLayoutPanel SizeX=3>
            <HeaderTemplate><div>Sessions</div></HeaderTemplate>
            <ContentTemplate><div class="stat">124,444</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=3 Column=3>
            <HeaderTemplate><div>Users</div></HeaderTemplate>
            <ContentTemplate><div class="stat">64,496</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=3 Column=6>
            <HeaderTemplate><div>Views</div></HeaderTemplate>
            <ContentTemplate><div class="stat">442,278</div></ContentTemplate>
        </DashboardLayoutPanel>

        <DashboardLayoutPanel SizeX=3 Column=9>
            <HeaderTemplate><div>Conversion</div></HeaderTemplate>
            <ContentTemplate><div class="stat">3.2%</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Main Analytics - Half width on desktop, full on tablet/mobile -->
        <DashboardLayoutPanel SizeX=6 Row=1>
            <HeaderTemplate><div>Daily Active Visitors</div></HeaderTemplate>
            <ContentTemplate><div>Chart area</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Secondary Analytics - Half width on desktop, full on tablet/mobile -->
        <DashboardLayoutPanel SizeX=6 Row=1 Column=6>
            <HeaderTemplate><div>Content Performance</div></HeaderTemplate>
            <ContentTemplate><div>Chart area</div></ContentTemplate>
        </DashboardLayoutPanel>

        <!-- Data Table - Full width -->
        <DashboardLayoutPanel SizeX=12 Row=2>
            <HeaderTemplate><div>Recent Transactions</div></HeaderTemplate>
            <ContentTemplate><div>Table with data</div></ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

<style>
    .stat {
        font-size: 24px;
        font-weight: bold;
        color: #334099;
        text-align: center;
        padding: 20px;
    }
    .e-panel-header {
        background-color: #334099;
        color: white;
        padding: 10px;
        font-weight: bold;
    }
    .e-panel-content {
        padding: 15px;
        min-height: 150px;
    }
</style>
```

### Media Query Best Practices

**Hierarchy of Breakpoints:**
1. Start with reasonable defaults (Columns=5 or 6)
2. Test at common device sizes
3. Set MediaQuery at most critical breakpoint
4. Usually 768px (tablet) or 600px (mobile)
5. Verify readability and usability

**Testing Responsive Layouts:**
- Use browser DevTools (F12) device emulation
- Test landscape and portrait orientations
- Verify touch interactions on mobile
- Check panel stacking and spacing
- Ensure buttons/inputs are clickable size
