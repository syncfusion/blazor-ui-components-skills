---
name: syncfusion-blazor-carousel
description: Guide for implementing Syncfusion Blazor Carousel component for image galleries, product showcases, and content sliders. Use when user mentions carousel, image slider, slideshow, content rotation, product galleries, rotating banners, slide transitions, or needs to display multiple items in a cycling presentation format.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Syncfusion Blazor Carousel

A comprehensive guide for implementing the Syncfusion Blazor Carousel component - an interactive slideshow for cycling through images, text, or custom content with navigation controls, indicators, and animation effects.

## When to Use This Skill

Use this skill when the user needs to:

- Create image galleries or photo carousels
- Build product showcase sliders with multiple items
- Implement content rotation with automatic transitions
- Add slideshow presentations with navigation controls
- Create rotating banners or hero sections
- Display testimonials or reviews in a sliding format
- Build partial-visible slide layouts (showing previous/next items)
- Implement accessible carousels with keyboard navigation
- Customize slide transitions with animations
- Add play/pause controls for slide automation

## Component Overview

The Syncfusion Blazor Carousel is a navigation component that allows users to browse through a collection of items in a cyclic manner. It supports:

- **Multiple Animation Effects:** Slide, Fade, or custom CSS animations
- **Navigation Controls:** Previous/Next buttons with customizable templates
- **Indicators:** Visual slide position markers with multiple types (Default, Dynamic, Fraction, Progress)
- **Accessibility:** Full WCAG 2.2 compliance with keyboard navigation and ARIA attributes
- **Touch Support:** Swipe gestures for mobile devices
- **Partial Views:** Show adjacent slides alongside the active slide
- **Auto Play:** Automatic slide transitions with configurable intervals
- **Theming:** CSS customization for complete visual control

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When the user needs to:
- Install and configure the Carousel package
- Add required namespaces and register services
- Reference theme stylesheets and scripts
- Create a basic carousel with items
- Render initial carousel implementation

### Populating Items and Selection
📄 **Read:** [references/populating-items.md](references/populating-items.md)

When the user needs to:
- Add carousel items using CarouselItem
- Set the initial selected slide
- Navigate programmatically to specific slides
- Implement partial visible slides (showing adjacent items)
- Configure loop behavior with partial slides
- Use SelectedIndex for slide selection
- Call PreviousAsync/NextAsync methods

### Navigators and Indicators
📄 **Read:** [references/navigators-and-indicators.md](references/navigators-and-indicators.md)

When the user needs to:
- Show or hide previous/next navigation buttons
- Display navigators only on hover
- Customize navigator button templates
- Configure indicator visibility and appearance
- Create custom indicator templates
- Show slide previews in indicators
- Use different indicator types (Default, Dynamic, Fraction, Progress)
- Add play/pause button functionality
- Customize play button template

### Accessibility
📄 **Read:** [references/accessibility.md](references/accessibility.md)

When the user needs to:
- Ensure WCAG 2.2 and Section 508 compliance
- Understand WAI-ARIA attributes applied to carousel
- Configure keyboard navigation
- Learn keyboard shortcuts (Arrow keys, Home, End, Space, Enter)
- Enable or disable keyboard interaction
- Support assistive technologies

### Animations and Transitions
📄 **Read:** [references/animations-and-transitions.md](references/animations-and-transitions.md)

When the user needs to:
- Apply built-in animation effects (Slide or Fade)
- Create custom animations with CSS
- Set different intervals for each slide
- Enable or disable auto play
- Configure pause on hover behavior
- Control infinite looping of slides
- Handle slide change events
- Enable or disable touch swipe
- Configure swipe modes (Touch, Mouse, or both)

### Styles and Appearance
📄 **Read:** [references/styles-and-appearance.md](references/styles-and-appearance.md)

When the user needs to:
- Understand CSS class structure
- Customize indicator appearance and spacing
- Modify navigator position and styling
- Adjust partial slide sizing
- Override default carousel styles
- Use Theme Studio for custom themes

## Quick Start Example

Here's a minimal carousel implementation with image slides:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel>
    <CarouselItem>
        <figure>
            <img src="images/bridge.png" alt="Bridge" style="height:100%;width:100%;" />
            <figcaption>Golden Gate Bridge</figcaption>
        </figure>
    </CarouselItem>
    <CarouselItem>
        <figure>
            <img src="images/trees.png" alt="Trees" style="height:100%;width:100%;" />
            <figcaption>Spring Flower Trees</figcaption>
        </figure>
    </CarouselItem>
    <CarouselItem>
        <figure>
            <img src="images/waterfall.png" alt="Waterfall" style="height:100%;width:100%;" />
            <figcaption>Oddadalen Waterfalls</figcaption>
        </figure>
    </CarouselItem>
</SfCarousel>
```

**Prerequisites:** Ensure you have installed `Syncfusion.Blazor.Navigations` package and registered the Syncfusion Blazor service in `Program.cs`. See [Getting Started](references/getting-started.md) for setup details.

## Common Patterns

### Pattern 1: Auto-Playing Carousel with Pause on Hover

```cshtml
<SfCarousel AutoPlay="true" 
            PauseOnHover="true" 
            Interval="3000">
    <CarouselItem>
        <div>Slide 1 Content</div>
    </CarouselItem>
    <CarouselItem>
        <div>Slide 2 Content</div>
    </CarouselItem>
    <CarouselItem>
        <div>Slide 3 Content</div>
    </CarouselItem>
</SfCarousel>
```

**When to use:** Product showcases, promotional banners where automatic rotation is desired but users need control to pause on hover.

### Pattern 2: Carousel with Hidden Navigators and Progress Indicator

```cshtml
<SfCarousel ButtonsVisibility="CarouselButtonVisibility.Hidden"
            IndicatorsType="CarouselIndicatorType.Progress"
            ShowIndicators="true">
    <CarouselItem>
        <div>Slide 1</div>
    </CarouselItem>
    <CarouselItem>
        <div>Slide 2</div>
    </CarouselItem>
    <CarouselItem>
        <div>Slide 3</div>
    </CarouselItem>
</SfCarousel>
```

**When to use:** Clean minimal design with progress bar showing slide position, suitable for mobile-first designs relying on touch swipe.

### Pattern 3: Partial Visible Slides (Preview Mode)

```cshtml
<SfCarousel PartialVisible="true" 
            Loop="true">
    <CarouselItem>
        <img src="images/image1.png" alt="Image 1" />
    </CarouselItem>
    <CarouselItem>
        <img src="images/image2.png" alt="Image 2" />
    </CarouselItem>
    <CarouselItem>
        <img src="images/image3.png" alt="Image 3" />
    </CarouselItem>
    <CarouselItem>
        <img src="images/image4.png" alt="Image 4" />
    </CarouselItem>
</SfCarousel>
```

**When to use:** Image galleries where showing adjacent slides provides context and encourages navigation.

### Pattern 4: Programmatic Navigation with Buttons

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfCarousel @ref="CarouselRef" 
            ButtonsVisibility="CarouselButtonVisibility.Hidden">
    <CarouselItem><div>Slide 1</div></CarouselItem>
    <CarouselItem><div>Slide 2</div></CarouselItem>
    <CarouselItem><div>Slide 3</div></CarouselItem>
</SfCarousel>

<div class="custom-controls">
    <SfButton @onclick="@(async () => await CarouselRef.PreviousAsync())">Previous</SfButton>
    <SfButton @onclick="@(async () => await CarouselRef.NextAsync())">Next</SfButton>
</div>

@code {
    SfCarousel CarouselRef;
}
```

**When to use:** Custom UI designs requiring external navigation controls separate from the carousel component.

### Pattern 5: Fade Animation with Custom Intervals

```cshtml
<SfCarousel AnimationEffect="CarouselAnimationEffect.Fade">
    <CarouselItem Interval="2000">
        <div>Quick Slide (2s)</div>
    </CarouselItem>
    <CarouselItem Interval="4000">
        <div>Medium Slide (4s)</div>
    </CarouselItem>
    <CarouselItem Interval="6000">
        <div>Long Slide (6s)</div>
    </CarouselItem>
</SfCarousel>
```

**When to use:** Content with varying complexity where some slides need more viewing time than others.

### Pattern 6: Carousel with Selected Index Binding

```cshtml
<SfCarousel @bind-SelectedIndex="@CurrentSlide">
    <CarouselItem><div>Slide 1</div></CarouselItem>
    <CarouselItem><div>Slide 2</div></CarouselItem>
    <CarouselItem><div>Slide 3</div></CarouselItem>
</SfCarousel>

<p>Current Slide: @CurrentSlide</p>

@code {
    private int CurrentSlide = 0;
}
```

**When to use:** Tracking current slide position, synchronizing multiple carousels, or triggering actions based on slide changes.

## Key Properties Reference

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `AutoPlay` | bool | true | Enables automatic slide transitions |
| `AnimationEffect` | CarouselAnimationEffect | Slide | Slide transition animation (Slide, Fade, Custom, None) |
| `ButtonsVisibility` | CarouselButtonVisibility | Visible | Navigator button visibility (Visible, Hidden, VisibleOnHover) |
| `EnableTouchSwipe` | bool | true | Enables swipe gestures on touch devices |
| `Interval` | int | 5000 | Default time (ms) between slide transitions |
| `Loop` | bool | true | Enables infinite looping of slides |
| `PartialVisible` | bool | false | Shows partial view of adjacent slides |
| `PauseOnHover` | bool | true | Pauses auto play when hovering |
| `SelectedIndex` | int | 0 | Index of the currently active slide |
| `ShowIndicators` | bool | true | Shows/hides position indicators |
| `ShowPlayButton` | bool | false | Shows play/pause toggle button |
| `IndicatorsType` | CarouselIndicatorType | Default | Indicator style (Default, Dynamic, Fraction, Progress) |
| `SwipeMode` | CarouselSwipeMode | Touch & Mouse | Swipe input modes |
| `AllowKeyboardInteraction` | bool | true | Enables keyboard navigation |

## Common Use Cases

1. **E-commerce Product Galleries:** Display multiple product images with navigation
2. **Hero Banners:** Rotating promotional content on landing pages
3. **Testimonial Sliders:** Cycling through customer reviews
4. **Before/After Showcases:** Image comparisons with slide transitions
5. **Portfolio Displays:** Presenting project images or case studies
6. **News/Article Highlights:** Featured content rotation
7. **Onboarding Tutorials:** Step-by-step guide slides
8. **Image Lightboxes:** Full-screen image viewing with navigation

## Next Steps

- For setup and installation → Read [Getting Started](references/getting-started.md)
- For item management → Read [Populating Items](references/populating-items.md)
- For UI customization → Read [Navigators and Indicators](references/navigators-and-indicators.md)
- For accessibility compliance → Read [Accessibility](references/accessibility.md)
- For animations → Read [Animations and Transitions](references/animations-and-transitions.md)
- For styling → Read [Styles and Appearance](references/styles-and-appearance.md)
