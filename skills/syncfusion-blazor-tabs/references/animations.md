# Animations and Transitions

## Table of Contents
- [Overview](#overview)
- [Animation Configuration](#animation-configuration)
- [Animation Effects](#animation-effects)
- [Default Animations](#default-animations)
- [Disabling Animations](#disabling-animations)
- [Custom Animation Examples](#custom-animation-examples)
- [Performance Considerations](#performance-considerations)

## Overview

The Blazor Tabs component supports smooth animated transitions when switching between tabs. You can customize the animation effects for both navigating to the next tab and returning to the previous tab.

### Animation Structure
- **TabAnimationSettings**: Container for animation configuration
- **TabAnimationPrevious**: Animation when navigating to previous tab
- **TabAnimationNext**: Animation when navigating to next tab

---

## Animation Configuration

### TabAnimationSettings Component

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabAnimationSettings>
        <TabAnimationPrevious Effect="@PreviousEffect" Duration="300" Easing="ease-in-out"></TabAnimationPrevious>
        <TabAnimationNext Effect="@NextEffect" Duration="300" Easing="ease-in-out"></TabAnimationNext>
    </TabAnimationSettings>
    <TabItems>
        <!-- Tab items -->
    </TabItems>
</SfTab>

@code {
    private AnimationEffect PreviousEffect = AnimationEffect.SlideLeftIn;
    private AnimationEffect NextEffect = AnimationEffect.SlideRightIn;
}
```

### Animation Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Effect` | AnimationEffect | SlideLeftIn/SlideRightIn | Animation effect to apply |
| `Duration` | double | 400 | Animation duration in milliseconds |
| `Easing` | string | "ease" | CSS easing function |

---

## Animation Effects

The Tab component supports the following animation effects from the Syncfusion Animation library:

### Available Effects

| Effect | Description | Use Case |
|--------|-------------|----------|
| `AnimationEffect.SlideLeftIn` | Slide from left | Default previous animation |
| `AnimationEffect.SlideRightIn` | Slide from right | Default next animation |
| `AnimationEffect.FadeIn` | Fade in effect | Subtle transitions |
| `AnimationEffect.FadeOut` | Fade out effect | Exiting animation |
| `AnimationEffect.FadeZoomIn` | Fade and zoom in | Modern, emphasis |
| `AnimationEffect.FadeZoomOut` | Fade and zoom out | Exit emphasis |
| `AnimationEffect.ZoomIn` | Zoom in effect | Expanding content |
| `AnimationEffect.ZoomOut` | Zoom out effect | Collapsing content |
| `AnimationEffect.None` | No animation | Performance optimization |

### Effect Selector Implementation

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.DropDowns

<div style="margin: 20px; display: flex; gap: 20px;">
    <div>
        <label>Previous Animation:</label>
        <SfDropDownList TValue="AnimationEffect" DataSource="@AvailableEffects" TItem="AnimationOption" @bind-Value="@PreviousEffect">
            <DropDownListFieldSettings Value="Effect" Text="EffectName"></DropDownListFieldSettings>
        </SfDropDownList>
    </div>
    
    <div>
        <label>Next Animation:</label>
        <SfDropDownList TValue="AnimationEffect" DataSource="@AvailableEffects" TItem="AnimationOption" @bind-Value="@NextEffect">
            <DropDownListFieldSettings Value="Effect" Text="EffectName"></DropDownListFieldSettings>
        </SfDropDownList>
    </div>
</div>

<SfTab Width="600px">
    <TabAnimationSettings>
        <TabAnimationPrevious Effect="@PreviousEffect"></TabAnimationPrevious>
        <TabAnimationNext Effect="@NextEffect"></TabAnimationNext>
    </TabAnimationSettings>
    <TabItems>
        <TabItem Content="Social Media platforms enable communication">
            <ChildContent>
                <TabHeader Text="Social Networks"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Messaging apps provide instant communication">
            <ChildContent>
                <TabHeader Text="Messaging"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Email remains essential for formal communication">
            <ChildContent>
                <TabHeader Text="Email"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private AnimationEffect PreviousEffect = AnimationEffect.SlideLeftIn;
    private AnimationEffect NextEffect = AnimationEffect.SlideRightIn;

    private List<AnimationOption> AvailableEffects = new()
    {
        new() { Effect = AnimationEffect.SlideLeftIn, EffectName = "Slide Left In" },
        new() { Effect = AnimationEffect.SlideRightIn, EffectName = "Slide Right In" },
        new() { Effect = AnimationEffect.FadeIn, EffectName = "Fade In" },
        new() { Effect = AnimationEffect.FadeOut, EffectName = "Fade Out" },
        new() { Effect = AnimationEffect.FadeZoomIn, EffectName = "Fade Zoom In" },
        new() { Effect = AnimationEffect.FadeZoomOut, EffectName = "Fade Zoom Out" },
        new() { Effect = AnimationEffect.ZoomIn, EffectName = "Zoom In" },
        new() { Effect = AnimationEffect.ZoomOut, EffectName = "Zoom Out" },
        new() { Effect = AnimationEffect.None, EffectName = "None (No Animation)" }
    };

    public class AnimationOption
    {
        public AnimationEffect Effect { get; set; }
        public string EffectName { get; set; }
    }
}
```

---

## Default Animations

The Tabs component has sensible defaults built in:

### Default Configuration
- **Previous Tab**: `SlideLeftIn` animation (300ms)
- **Next Tab**: `SlideRightIn` animation (300ms)
- **Easing**: Default CSS easing

### No Configuration Needed

```razor
<!-- Default animations are applied automatically -->
<SfTab>
    <TabItems>
        <TabItem Content="Tab 1 content">
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Tab 2 content">
            <ChildContent>
                <TabHeader Text="Tab 2"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

<!-- Result: Smooth slide animations when switching tabs -->
```

---

## Disabling Animations

### Disable All Animations

For performance optimization or accessibility reasons, you can disable animations:

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabAnimationSettings>
        <TabAnimationPrevious Effect="AnimationEffect.None"></TabAnimationPrevious>
        <TabAnimationNext Effect="AnimationEffect.None"></TabAnimationNext>
    </TabAnimationSettings>
    <TabItems>
        <TabItem Content="Content 1">
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Content 2">
            <ChildContent>
                <TabHeader Text="Tab 2"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```

### Disable Animation Indicator

When using `AnimationEffect.None`, also disable the header indicator animation with CSS:

```css
/* Disable indicator animation when tab effect is None */
.e-tab .e-tab-header:not(.e-vertical) .e-indicator,
.e-tab .e-tab-header.e-vertical .e-indicator {
    transition: none;
}
```

### Conditional Animation Disabling

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton @onclick="@ToggleAnimations">
    @(AnimationsEnabled ? "Disable Animations" : "Enable Animations")
</SfButton>

<SfTab>
    <TabAnimationSettings>
        <TabAnimationPrevious Effect="@(AnimationsEnabled ? AnimationEffect.SlideLeftIn : AnimationEffect.None)"></TabAnimationPrevious>
        <TabAnimationNext Effect="@(AnimationsEnabled ? AnimationEffect.SlideRightIn : AnimationEffect.None)"></TabAnimationNext>
    </TabAnimationSettings>
    <TabItems>
        <TabItem Content="Content 1">
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Content 2">
            <ChildContent>
                <TabHeader Text="Tab 2"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private bool AnimationsEnabled = true;

    private void ToggleAnimations()
    {
        AnimationsEnabled = !AnimationsEnabled;
    }
}
```

---

## Custom Animation Examples

### Example 1: Subtle Fade Animation

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabAnimationSettings>
        <TabAnimationPrevious Effect="AnimationEffect.FadeOut" Duration="250"></TabAnimationPrevious>
        <TabAnimationNext Effect="AnimationEffect.FadeIn" Duration="250"></TabAnimationNext>
    </TabAnimationSettings>
    <TabItems>
        <TabItem>
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div style="padding: 20px;">Subtle fade transition for minimal distraction</div>
            </ContentTemplate>
        </TabItem>
        <TabItem>
            <ChildContent>
                <TabHeader Text="Tab 2"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div style="padding: 20px;">Smooth and gentle transition</div>
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>
```

### Example 2: Emphasis Animation (Zoom)

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabAnimationSettings>
        <TabAnimationPrevious Effect="AnimationEffect.FadeZoomOut" Duration="300"></TabAnimationPrevious>
        <TabAnimationNext Effect="AnimationEffect.FadeZoomIn" Duration="300"></TabAnimationNext>
    </TabAnimationSettings>
    <TabItems>
        <TabItem>
            <ChildContent>
                <TabHeader Text="Features"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div style="padding: 20px;">
                    <h3>Key Features</h3>
                    <ul>
                        <li>Fast performance</li>
                        <li>Easy integration</li>
                        <li>Rich customization</li>
                    </ul>
                </div>
            </ContentTemplate>
        </TabItem>
        <TabItem>
            <ChildContent>
                <TabHeader Text="Benefits"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div style="padding: 20px;">
                    <h3>User Benefits</h3>
                    <ul>
                        <li>Improved UX</li>
                        <li>Better engagement</li>
                        <li>Reduced confusion</li>
                    </ul>
                </div>
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>
```

### Example 3: Fast Transition

```razor
@using Syncfusion.Blazor.Navigations

<!-- Quick transitions for responsive interfaces -->
<SfTab>
    <TabAnimationSettings>
        <TabAnimationPrevious Effect="AnimationEffect.SlideLeftIn" Duration="150"></TabAnimationPrevious>
        <TabAnimationNext Effect="AnimationEffect.SlideRightIn" Duration="150"></TabAnimationNext>
    </TabAnimationSettings>
    <TabItems>
        <TabItem Content="Fast">
            <ChildContent>
                <TabHeader Text="Quick"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Responsive">
            <ChildContent>
                <TabHeader Text="Responsive"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```

### Example 4: Asymmetric Animations

Different animations for previous and next navigation:

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabAnimationSettings>
        <!-- Going back is quick -->
        <TabAnimationPrevious Effect="AnimationEffect.FadeOut" Duration="150"></TabAnimationPrevious>
        <!-- Going forward emphasizes the new content -->
        <TabAnimationNext Effect="AnimationEffect.FadeZoomIn" Duration="400"></TabAnimationNext>
    </TabAnimationSettings>
    <TabItems>
        <TabItem>
            <ChildContent>
                <TabHeader Text="Step 1"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div style="padding: 20px;">First step - gets de-emphasized</div>
            </ContentTemplate>
        </TabItem>
        <TabItem>
            <ChildContent>
                <TabHeader Text="Step 2"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div style="padding: 20px;">Second step - emphasized on entry</div>
            </ContentTemplate>
        </TabItem>
        <TabItem>
            <ChildContent>
                <TabHeader Text="Step 3"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div style="padding: 20px;">Third step - same emphasis</div>
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>
```

### Example 5: Match Orientation

```razor
@using Syncfusion.Blazor.Navigations

<!-- Animation matches tab orientation -->
<SfTab HeaderPlacement="HeaderPosition.Left">
    <TabAnimationSettings>
        <!-- For left orientation, use horizontal animations -->
        <TabAnimationPrevious Effect="AnimationEffect.SlideLeftIn" Duration="300"></TabAnimationPrevious>
        <TabAnimationNext Effect="AnimationEffect.SlideRightIn" Duration="300"></TabAnimationNext>
    </TabAnimationSettings>
    <TabItems>
        <TabItem>
            <ChildContent>
                <TabHeader Text="Navigation 1"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div style="padding: 20px;">Sidebar navigation with horizontal slide</div>
            </ContentTemplate>
        </TabItem>
        <TabItem>
            <ChildContent>
                <TabHeader Text="Navigation 2"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div style="padding: 20px;">Smooth transition matches layout</div>
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>
```

---

## Performance Considerations

### Animation Impact on Performance

- **Duration**: Shorter animations (150-300ms) have less performance impact
- **Effect**: Fade/Zoom effects use CSS transforms (hardware accelerated)
- **Number of Tabs**: More tabs = more transitions = potential performance hit

### Optimization Strategies

#### 1. Reduce Animation Duration for Many Tabs

```razor
<!-- Light animation for performance -->
<SfTab>
    <TabAnimationSettings>
        <TabAnimationPrevious Effect="AnimationEffect.FadeIn" Duration="100"></TabAnimationPrevious>
        <TabAnimationNext Effect="AnimationEffect.FadeOut" Duration="100"></TabAnimationNext>
    </TabAnimationSettings>
    <!-- 20+ tabs -->
</SfTab>
```

#### 2. Disable Animations on Mobile

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabAnimationSettings>
        <TabAnimationPrevious Effect="@(IsMobile ? AnimationEffect.None : AnimationEffect.SlideLeftIn)"></TabAnimationPrevious>
        <TabAnimationNext Effect="@(IsMobile ? AnimationEffect.None : AnimationEffect.SlideRightIn)"></TabAnimationNext>
    </TabAnimationSettings>
    <TabItems>
        <!-- Tab items -->
    </TabItems>
</SfTab>

@code {
    private bool IsMobile = false;

    protected override void OnInitialized()
    {
        // Detect mobile device
        IsMobile = IsUserAgentMobile();
    }

    private bool IsUserAgentMobile()
    {
        // Implementation to detect mobile
        return false;
    }
}
```

#### 3. Use Reduced Motion Preference

Respect user's motion preferences (accessibility):

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabAnimationSettings>
        <TabAnimationPrevious Effect="@GetAnimationEffect()" Duration="300"></TabAnimationPrevious>
        <TabAnimationNext Effect="@GetAnimationEffect()" Duration="300"></TabAnimationNext>
    </TabAnimationSettings>
    <TabItems>
        <!-- Tab items -->
    </TabItems>
</SfTab>

@code {
    private AnimationEffect GetAnimationEffect()
    {
        // Check if user prefers reduced motion
        var prefersReducedMotion = false; // Check via JS if needed
        
        return prefersReducedMotion ? AnimationEffect.None : AnimationEffect.SlideRightIn;
    }
}
```

### Best Practices

✅ **Do:**
- Use animations for visual feedback and guidance
- Keep animations under 400ms for responsiveness
- Test on low-end devices
- Respect user preferences (prefers-reduced-motion)
- Use simple effects for better performance

❌ **Don't:**
- Use overly complex animations
- Animate too many elements simultaneously
- Ignore accessibility concerns (motion sensitivity)
- Add animations without testing performance
- Use long animation durations (> 500ms)

---

## Animation Detection

Detect when animation completes:

```razor
@using Syncfusion.Blazor.Navigations

<SfTab @ref="TabControl">
    <TabEvents Selected="OnTabSelected"></TabEvents>
    <TabAnimationSettings>
        <TabAnimationPrevious Effect="AnimationEffect.SlideLeftIn" Duration="300"></TabAnimationPrevious>
        <TabAnimationNext Effect="AnimationEffect.SlideRightIn" Duration="300"></TabAnimationNext>
    </TabAnimationSettings>
    <TabItems>
        <TabItem>
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div>Content 1</div>
            </ContentTemplate>
        </TabItem>
        <TabItem>
            <ChildContent>
                <TabHeader Text="Tab 2"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div>Content 2</div>
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private SfTab TabControl;

    private async Task OnTabSelected(SelectEventArgs args)
    {
        // Animation completes after ~300ms (duration)
        await Task.Delay(300);
        
        // Perform action after animation
        Console.WriteLine("Tab animation complete, content loaded");
    }
}
```
