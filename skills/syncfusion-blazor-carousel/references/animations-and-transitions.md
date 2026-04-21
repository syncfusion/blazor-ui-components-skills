# Animations and Transitions

## Table of Contents
- [Animation Effects](#animation-effects)
  - [Fade Animation](#fade-animation)
  - [Custom Animation](#custom-animation)
- [Intervals Between Slides](#intervals-between-slides)
- [Auto Play Slides](#auto-play-slides)
- [Pause on Hover](#pause-on-hover)
- [Looping Slides](#looping-slides)
- [Slide Changing Events](#slide-changing-events)
- [Touch Swipe](#touch-swipe)
- [Swipe Modes](#swipe-modes)

## Animation Effects

The carousel supports multiple built-in animation effects for slide transitions via the `AnimationEffect` property. By default, the `Slide` animation is applied.

### Fade Animation

Apply a fade transition where slides fade out and fade in:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel AnimationEffect="CarouselAnimationEffect.Fade">
    <CarouselItem>
        <figure class="img-container">
            <img src="https://ej2.syncfusion.com/products/images/carousel/cardinal.png" 
                 alt="cardinal" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Cardinal</figcaption>
        </figure>
    </CarouselItem>
    <CarouselItem>
        <figure class="img-container">
            <img src="https://ej2.syncfusion.com/products/images/carousel/hunei.png" 
                 alt="kingfisher" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Kingfisher</figcaption>
        </figure>
    </CarouselItem>
    <CarouselItem>
        <figure class="img-container">
            <img src="https://ej2.syncfusion.com/products/images/carousel/costa-rica.png" 
                 alt="keel-billed-toucan" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Keel-billed-toucan</figcaption>
        </figure>
    </CarouselItem>
    <CarouselItem>
        <figure class="img-container">
            <img src="https://ej2.syncfusion.com/products/images/carousel/kaohsiung.png" 
                 alt="yellow-warbler" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Yellow-warbler</figcaption>
        </figure>
    </CarouselItem>
    <CarouselItem>
        <figure class="img-container">
            <img src="https://ej2.syncfusion.com/products/images/carousel/bee-eater.png" 
                 alt="bee-eater" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Bee-eater</figcaption>
        </figure>
    </CarouselItem>
</SfCarousel>

<style>
    .img-container {
        height: 100%;
        margin: 0;
    }

    .img-caption {
        color: #fff;
        font-size: 1rem;
        position: absolute;
        bottom: 3rem;
        width: 100%;
        text-align: center;
    }
</style>
```

**Available Animation Effects:**
- `Slide` (default) - Slides move horizontally
- `Fade` - Smooth fade transition
- `Custom` - Apply your own CSS animations
- `None` - No animation (instant transition)

**When to use Fade:**
- Image galleries where sliding motion is distracting
- Content that benefits from subtle transitions
- Overlapping or full-screen slides
- Professional presentations

### Custom Animation

Create custom slide transitions using CSS animations. Set `AnimationEffect="Custom"` and apply custom CSS via the `CssClass` property:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel AnimationEffect="CarouselAnimationEffect.Custom" CssClass="parallax">
    <CarouselItem>
        <figure class="img-container">
            <img src="https://ej2.syncfusion.com/products/images/carousel/cardinal.png" 
                 alt="cardinal" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Cardinal</figcaption>
        </figure>
    </CarouselItem>
    <CarouselItem>
        <figure class="img-container">
            <img src="https://ej2.syncfusion.com/products/images/carousel/hunei.png" 
                 alt="kingfisher" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Kingfisher</figcaption>
        </figure>
    </CarouselItem>
    <CarouselItem>
        <figure class="img-container">
            <img src="https://ej2.syncfusion.com/products/images/carousel/costa-rica.png" 
                 alt="keel-billed-toucan" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Keel-billed-toucan</figcaption>
        </figure>
    </CarouselItem>
    <CarouselItem>
        <figure class="img-container">
            <img src="https://ej2.syncfusion.com/products/images/carousel/kaohsiung.png" 
                 alt="yellow-warbler" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Yellow-warbler</figcaption>
        </figure>
    </CarouselItem>
    <CarouselItem>
        <figure class="img-container">
            <img src="https://ej2.syncfusion.com/products/images/carousel/bee-eater.png" 
                 alt="bee-eater" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Bee-eater</figcaption>
        </figure>
    </CarouselItem>
</SfCarousel>

<style>
    .img-container {
        height: 100%;
        margin: 0;
    }

    .img-caption {
        color: #fff;
        font-size: 1rem;
        position: absolute;
        bottom: 3rem;
        width: 100%;
        text-align: center;
    }

    /* Parallax custom animation */
    .parallax .e-carousel-item {
        transition: transform 1s ease-in-out;
    }

    .parallax .e-carousel-item.e-next {
        animation: ParallaxIn 1s ease-in-out;
    }

    .parallax .e-carousel-item.e-prev {
        animation: ParallaxOut 1s ease-in-out;
    }

    @keyframes ParallaxIn {
        from {
            opacity: 0;
            transform: scale(0) translateY(100%);
        }   
        to {
            opacity: 1;
            transform: scale(1) translateY(0);
        }
    }

    @keyframes ParallaxOut {
        from {
            opacity: 1;
            transform: scale(1) translateY(0);
        }
        to {
            opacity: 0;
            transform: scale(0) translateY(-100%);
        }
    }
</style>
```

**Custom Animation Options:**
- **Zoom effects** - Scale slides in/out
- **Rotate transitions** - 3D rotation effects
- **Flip animations** - Card flip style
- **Slide with fade** - Combined effects
- **Bounce/elastic** - Playful animations
- **Parallax** - Layered depth effects

**CSS Animation Classes:**
- `.e-carousel-item` - Target all slides
- `.e-carousel-item.e-next` - Incoming slide
- `.e-carousel-item.e-prev` - Outgoing slide
- `.e-carousel-item.e-active` - Current slide

## Intervals Between Slides

Set different transition intervals for each slide using the `Interval` property on individual `CarouselItem` elements. The interval is specified in milliseconds.

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel>
    <CarouselItem Interval="2000">
        <div class="slide-content">Slide 1 (2 seconds)</div>
    </CarouselItem>
    <CarouselItem Interval="4000">
        <div class="slide-content">Slide 2 (4 seconds)</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 3 (default: 5 seconds)</div>
    </CarouselItem>
    <CarouselItem Interval="6000">
        <div class="slide-content">Slide 4 (6 seconds)</div>
    </CarouselItem>
    <CarouselItem Interval="8000">
        <div class="slide-content">Slide 5 (8 seconds)</div>
    </CarouselItem>
</SfCarousel>

<style>
    .e-carousel .slide-content {
        align-items: center;
        display: flex;
        font-size: 1.25rem;
        height: 100%;
        justify-content: center;
    }
</style>
```

**Default Interval:** 5000ms (5 seconds)

**When to use custom intervals:**
- Text-heavy slides need more reading time
- Quick promotional banners vs detailed content
- Image slides can be shorter than text slides
- Varying content complexity across slides

## Auto Play Slides

Control automatic slide transitions using the `AutoPlay` property. By default, auto-play is enabled (`true`).

**Disable auto-play:**

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel AutoPlay="false">
    <CarouselItem>
        <div class="slide-content">Slide 1</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 2</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 3</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 4</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 5</div>
    </CarouselItem>
</SfCarousel>

<style>
    .e-carousel .slide-content {
        align-items: center;
        display: flex;
        font-size: 1.25rem;
        height: 100%;
        justify-content: center;
    }
</style>
```

**When to disable auto-play:**
- User-controlled content exploration
- Forms or interactive content in slides
- Reading-heavy content where timing varies
- Accessibility concerns (WCAG guideline: allow users to control motion)
- Slides with videos or embedded media

**When to enable auto-play:**
- Hero banners with promotional content
- Image galleries for browsing
- News tickers or announcements
- Testimonial rotations
- Auto-advancing presentations

## Pause on Hover

By default, slide transitions pause when the user hovers their mouse over the carousel (`PauseOnHover="true"`). You can disable this behavior:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel PauseOnHover="false">
    <CarouselItem>
        <div class="slide-content">Slide 1</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 2</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 3</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 4</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 5</div>
    </CarouselItem>
</SfCarousel>

<style>
    .e-carousel .slide-content {
        align-items: center;
        display: flex;
        font-size: 1.25rem;
        height: 100%;
        justify-content: center;
    }
</style>
```

**Default:** `PauseOnHover="true"`

**When to keep pause on hover enabled (default):**
- Users need time to read content
- Slides contain clickable links or buttons
- Complex images requiring inspection
- Best practice for accessibility

**When to disable pause on hover:**
- Kiosk displays where hover isn't meaningful
- Touch-only interfaces
- Continuous looping promotional content

## Looping Slides

Control whether slides loop infinitely using the `Loop` property. By default, looping is enabled (`true`).

**Disable looping:**

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel Loop="false">
    <CarouselItem>
        <div class="slide-content">Slide 1</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 2</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 3</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 4</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 5</div>
    </CarouselItem>
</SfCarousel>

<style>
    .e-carousel .slide-content {
        align-items: center;
        display: flex;
        font-size: 1.25rem;
        height: 100%;
        justify-content: center;
    }
</style>
```

**Behavior when Loop="false":**
- Auto-play stops after reaching the last slide
- Previous button is disabled on the first slide
- Next button is disabled on the last slide
- Users cannot navigate beyond the boundaries

**When to disable looping:**
- Tutorial or onboarding sequences
- Step-by-step processes with a clear end
- Product tours with defined start/finish
- Linear presentations

**When to keep looping enabled (default):**
- Image galleries
- Product showcases
- Testimonial rotations
- News feeds or announcements

## Slide Changing Events

Use the `SelectedIndexChanged` event to execute custom logic when slides change:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel SelectedIndexChanged="OnSelectedIndexChanged">
    <CarouselItem>
        <div class="slide-content">Slide 1</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 2</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 3</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 4</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 5</div>
    </CarouselItem>
</SfCarousel>

<p>Current Slide Index: @currentSlideIndex</p>

@code {
    private int currentSlideIndex = 0;

    void OnSelectedIndexChanged(int index)
    {
        currentSlideIndex = index;
        // Additional custom logic:
        // - Track analytics
        // - Load content dynamically
        // - Update other UI components
        // - Trigger animations
        Console.WriteLine($"Navigated to slide {index}");
    }
}

<style>
    .e-carousel .slide-content {
        align-items: center;
        display: flex;
        font-size: 1.25rem;
        height: 100%;
        justify-content: center;
    }
</style>
```

**Event Parameters:**
- `int index` - Zero-based index of the newly selected slide

**Use Cases:**
- Track user engagement with analytics
- Load slide content dynamically
- Synchronize multiple carousels
- Update external UI based on current slide
- Lazy-load images for performance
- Display slide-specific information

## Touch Swipe

Enable or disable swipe gestures on touch devices using the `EnableTouchSwipe` property. By default, touch swipe is enabled (`true`).

**Disable touch swipe:**

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel EnableTouchSwipe="false">
    <CarouselItem>
        <div class="slide-content">Slide 1</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 2</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 3</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 4</div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">Slide 5</div>
    </CarouselItem>
</SfCarousel>

<style>
    .e-carousel .slide-content {
        align-items: center;
        display: flex;
        font-size: 1.25rem;
        height: 100%;
        justify-content: center;
    }
</style>
```

**When to disable touch swipe:**
- Slides contain swipeable content (nested carousels, maps)
- Image zoom or pan functionality within slides
- Horizontal scrolling content in slides
- Prevent accidental navigation

## Swipe Modes

Control which input methods can trigger slide transitions using the `SwipeMode` property with bitwise operators:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel SwipeMode="CarouselSwipeMode.Mouse & CarouselSwipeMode.Touch">
    <CarouselItem>
        <figure class="img-container">
            <img src="https://ej2.syncfusion.com/products/images/carousel/cardinal.png" 
                 alt="cardinal" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Cardinal</figcaption>
        </figure>
    </CarouselItem>
    <CarouselItem>
        <figure class="img-container">
            <img src="https://ej2.syncfusion.com/products/images/carousel/hunei.png" 
                 alt="kingfisher" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Kingfisher</figcaption>
        </figure>
    </CarouselItem>
    <CarouselItem>
        <figure class="img-container">
            <img src="https://ej2.syncfusion.com/products/images/carousel/costa-rica.png" 
                 alt="keel-billed-toucan" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Keel-billed-toucan</figcaption>
        </figure>
    </CarouselItem>
    <CarouselItem>
        <figure class="img-container">
            <img src="https://ej2.syncfusion.com/products/images/carousel/kaohsiung.png" 
                 alt="yellow-warbler" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Yellow-warbler</figcaption>
        </figure>
    </CarouselItem>
    <CarouselItem>
        <figure class="img-container">
            <img src="https://ej2.syncfusion.com/products/images/carousel/bee-eater.png" 
                 alt="bee-eater" 
                 style="height:100%;width:100%;" />
            <figcaption class="img-caption">Bee-eater</figcaption>
        </figure>
    </CarouselItem>
</SfCarousel>

<style>
    .img-container {
        height: 100%;
        margin: 0;
    }

    .img-caption {
        color: #fff;
        font-size: 1rem;
        position: absolute;
        bottom: 3rem;
        width: 100%;
        text-align: center;
    }
</style>
```

**Available Swipe Modes:**

| Mode | Description |
|------|-------------|
| `CarouselSwipeMode.Touch` | Enable touch swipe gestures only |
| `CarouselSwipeMode.Mouse` | Enable mouse drag gestures only |
| `CarouselSwipeMode.Touch & CarouselSwipeMode.Mouse` | Enable both touch and mouse (default) |
| `~CarouselSwipeMode.Touch & ~CarouselSwipeMode.Mouse` | Disable both touch and mouse |

**Examples:**

**Touch only:**
```cshtml
<SfCarousel SwipeMode="CarouselSwipeMode.Touch">
```

**Mouse only:**
```cshtml
<SfCarousel SwipeMode="CarouselSwipeMode.Mouse">
```

**Both (default):**
```cshtml
<SfCarousel SwipeMode="CarouselSwipeMode.Touch & CarouselSwipeMode.Mouse">
```

**Neither:**
```cshtml
<SfCarousel SwipeMode="~CarouselSwipeMode.Touch & ~CarouselSwipeMode.Mouse">
```

**When to customize swipe modes:**
- Mobile-only apps: Enable touch, disable mouse
- Desktop-only apps: Enable mouse, disable touch
- Kiosk displays: Configure based on input device
- Prevent drag interference with other UI elements
