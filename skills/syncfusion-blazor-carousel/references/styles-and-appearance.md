# Styles and Appearance

## Table of Contents
- [CSS Structure](#css-structure)
- [Customizing Indicators](#customizing-indicators)
  - [Indicator Spacing](#indicator-spacing)
  - [Indicator Size and Shape](#indicator-size-and-shape)
  - [Indicator Position](#indicator-position)
- [Customizing Navigators](#customizing-navigators)
  - [Navigator Icon Size and Color](#navigator-icon-size-and-color)
  - [Navigator Position](#navigator-position)
  - [External Navigator Buttons](#external-navigator-buttons)
- [Customizing Partial Slides](#customizing-partial-slides)

## CSS Structure

The Blazor Carousel component provides a comprehensive set of CSS classes that you can override to customize its appearance. You can also use [Theme Studio](https://ej2.syncfusion.com/themestudio/?theme=material) to create custom themes.

### CSS Classes Reference

| CSS Class | Purpose |
|-----------|---------|
| `.e-carousel .e-carousel-item` | Customize all carousel items |
| `.e-carousel-item.e-active` | Customize only the active carousel item |
| `.e-carousel .e-carousel-indicators` | Customize the indicators container |
| `.e-carousel .e-carousel-indicators .e-indicator-bars .e-indicator-bar` | Customize individual indicator bars |
| `.e-carousel .e-carousel-indicators .e-indicator-bars .e-indicator-bar .e-indicator` | Customize the indicator element appearance |
| `.e-carousel .e-carousel-navigators` | Customize the navigators container |
| `.e-carousel .e-carousel-navigators .e-previous` | Customize the previous button |
| `.e-carousel .e-carousel-navigators .e-play-pause` | Customize the play and pause button |
| `.e-carousel.e-partial .e-carousel-slide-container` | Customize partial visible slides |

### Visual Structure

```
┌─────────────────────────────────────────────┐
│  .e-carousel                                │
│  ┌───────────────────────────────────────┐ │
│  │ .e-carousel-navigators                │ │
│  │   .e-previous  .e-play-pause  .e-next │ │
│  └───────────────────────────────────────┘ │
│  ┌───────────────────────────────────────┐ │
│  │ .e-carousel-items                     │ │
│  │   .e-carousel-item.e-active           │ │
│  │     (Slide Content)                   │ │
│  └───────────────────────────────────────┘ │
│  ┌───────────────────────────────────────┐ │
│  │ .e-carousel-indicators                │ │
│  │   .e-indicator-bar                    │ │
│  │     .e-indicator (dot/element)        │ │
│  └───────────────────────────────────────┘ │
└─────────────────────────────────────────────┘
```

## Customizing Indicators

### Indicator Spacing

Adjust the space between indicator dots by overriding the `.e-indicator-bar` class:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel CssClass="custom-indicator-spacing">
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
    .custom-indicator-spacing .e-carousel-indicators .e-indicator-bars .e-indicator-bar {
        padding: 8px;  /* Increase spacing (default: 4px) */
    }

    .e-carousel .slide-content {
        align-items: center;
        display: flex;
        font-size: 1.25rem;
        height: 100%;
        justify-content: center;
    }
</style>
```

**Spacing Options:**
- Reduce padding for compact layouts
- Increase padding for touch-friendly mobile interfaces
- Use margin instead of padding for different spacing behavior

### Indicator Size and Shape

Customize indicator dimensions and appearance:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel CssClass="custom-indicators">
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
    .custom-indicators .e-carousel-indicators .e-indicator-bars .e-indicator-bar .e-indicator {
        width: 20px;           /* Custom width (default: 8px) */
        height: 20px;          /* Custom height */
        border-radius: 50%;    /* Circular shape (100% or 50%) */
        background-color: #ccc;
    }

    .custom-indicators .e-carousel-indicators .e-indicator-bars .e-indicator-bar.e-active .e-indicator {
        background-color: #007bff;  /* Active indicator color */
    }

    .e-carousel .slide-content {
        align-items: center;
        display: flex;
        font-size: 1.25rem;
        height: 100%;
        justify-content: center;
    }
</style>
```

**Shape Options:**
- `border-radius: 50%` or `100%` - Circular dots
- `border-radius: 4px` - Rounded rectangles
- `border-radius: 0` - Square indicators
- `border-radius: 50px` - Pill-shaped indicators

### Indicator Position

Move indicators outside the carousel container or reposition them:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel CssClass="indicators-outside">
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
    .indicators-outside .e-carousel-indicators {
        bottom: -40px;  /* Move indicators below carousel */
        position: relative;
    }

    /* Alternative: Position at top */
    .indicators-top .e-carousel-indicators {
        top: 0;
        bottom: auto;
    }

    .e-carousel .slide-content {
        align-items: center;
        display: flex;
        font-size: 1.25rem;
        height: 100%;
        justify-content: center;
    }
</style>
```

**Positioning Options:**
- `bottom: auto` - Remove default bottom positioning
- `top: 0` - Position at top
- `bottom: -40px` - Position below carousel
- `left: 0` or `right: 0` - Align to sides

## Customizing Navigators

### Navigator Icon Size and Color

Customize the previous and next button icons:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel CssClass="custom-navigators">
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
    .custom-navigators .e-carousel-navigators .e-next .e-btn:not(:disabled) .e-btn-icon,
    .custom-navigators .e-carousel-navigators .e-previous .e-btn:not(:disabled) .e-btn-icon {
        color: greenyellow;     /* Custom icon color */
        font-size: 25px;        /* Custom icon size (default: 16px) */
    }

    .e-carousel .slide-content {
        align-items: center;
        display: flex;
        font-size: 1.25rem;
        height: 100%;
        justify-content: center;
    }
</style>
```

**Customization Options:**
- Change icon color to match brand
- Increase size for better touch targets
- Add shadows or borders
- Modify hover/active states

### Navigator Position

Reposition navigator buttons vertically:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel CssClass="navigators-bottom">
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
    .navigators-bottom .e-carousel-navigators {
        top: auto;      /* Remove default top: 50% */
        bottom: 20px;   /* Position 20px from bottom */
    }

    .e-carousel .slide-content {
        align-items: center;
        display: flex;
        font-size: 1.25rem;
        height: 100%;
        justify-content: center;
    }
</style>
```

**Position Variations:**
- `top: 20px` - Position near top
- `bottom: 20px` - Position near bottom
- `top: 50%; transform: translateY(-50%)` - Center vertically (default)

### External Navigator Buttons

Position navigation buttons outside the carousel area:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel CssClass="external-navigators">
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
    .external-navigators .e-carousel-navigators .e-previous,
    .external-navigators .e-carousel-navigators .e-next {
        margin: 0 -60px;       /* Move buttons outside by 60px */
        background: black;      /* Add background */
        border-radius: 50%;     /* Make circular */
        width: 40px;
        height: 40px;
    }

    .e-carousel .slide-content {
        align-items: center;
        display: flex;
        font-size: 1.25rem;
        height: 100%;
        justify-content: center;
    }
</style>
```

**Use Cases:**
- Full-width carousels with external controls
- Image galleries where buttons shouldn't overlay images
- Designs requiring clear separation between content and controls

## Customizing Partial Slides

Adjust the visible area of partial slides:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel PartialVisible="true" CssClass="custom-partial">
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
    .custom-partial.e-carousel.e-partial .e-carousel-slide-container {
        padding: 0 150px;  /* Show more/less of adjacent slides */
    }

    .img-container {
        margin: 0 10px;
        width: 100%;
        height: 100%;
    }

    .img-caption {
        bottom: 4em;
        color: #fff;
        font-size: 12pt;
        height: 2em;
        position: relative;
        padding: 0.3em 1em;
        text-align: center;
        width: 100%;
    }
</style>
```

**Padding Values:**
- `padding: 0 50px` - Show small preview (more of active slide visible)
- `padding: 0 150px` - Show larger preview (less of active slide visible)
- `padding: 0 250px` - Show maximum preview (adjacent slides almost equal to active)

**Responsive Partial Slides:**

```css
/* Adjust partial visibility for different screen sizes */
.custom-partial.e-carousel.e-partial .e-carousel-slide-container {
    padding: 0 150px;
}

@media (max-width: 768px) {
    .custom-partial.e-carousel.e-partial .e-carousel-slide-container {
        padding: 0 80px;  /* Smaller padding for tablets */
    }
}

@media (max-width: 480px) {
    .custom-partial.e-carousel.e-partial .e-carousel-slide-container {
        padding: 0 40px;  /* Even smaller for mobile */
    }
}
```

## Additional Customization Tips

### Custom Active Slide Styling

```css
.e-carousel .e-carousel-item.e-active {
    transform: scale(1.05);
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
    z-index: 1;
}
```

### Smooth Transitions

```css
.e-carousel .e-carousel-item {
    transition: all 0.3s ease-in-out;
}
```

### Background and Borders

```css
.e-carousel {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    border: 2px solid #333;
    border-radius: 10px;
    overflow: hidden;
}
```

### Custom Button Backgrounds

```css
.e-carousel .e-carousel-navigators .e-btn {
    background-color: rgba(0, 0, 0, 0.5);
    border-radius: 50%;
}

.e-carousel .e-carousel-navigators .e-btn:hover {
    background-color: rgba(0, 0, 0, 0.8);
}
```

## Using Theme Studio

For comprehensive theme customization, use [Syncfusion Theme Studio](https://ej2.syncfusion.com/themestudio/?theme=material):

1. Select the base theme (Material, Bootstrap, Fluent, etc.)
2. Customize colors, fonts, and component-specific styles
3. Download the custom theme CSS
4. Reference it in your application

This approach ensures consistent styling across all Syncfusion components in your application.
