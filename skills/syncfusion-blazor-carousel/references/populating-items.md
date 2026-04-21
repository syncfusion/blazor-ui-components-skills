# Populating Items

## Table of Contents
- [Populating Items Using CarouselItem](#populating-items-using-carouselitem)
- [Selection Using SelectedIndex Property](#selection-using-selectedindex-property)
- [Selection Using Navigation Methods](#selection-using-navigation-methods)
- [Partial Visible Slides](#partial-visible-slides)
- [Partial Visible Without Loop](#partial-visible-without-loop)

## Populating Items Using CarouselItem

Add carousel items using the `CarouselItem` tag. Each item represents a single slide in the carousel. You can add any HTML content or Blazor components within each item.

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel>
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

Each `CarouselItem` can contain:
- Images with captions
- Text content
- Custom HTML markup
- Blazor components
- Mixed content (images, text, buttons, etc.)

## Selection Using SelectedIndex Property

Use the `SelectedIndex` property to set which slide displays initially or to programmatically switch to a specific slide. The index is zero-based.

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel @bind-SelectedIndex="@CurrentIndex">
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

@code {
    int CurrentIndex = 2; // Start at third slide (index 2)
}
```

**Key Points:**
- `SelectedIndex` is zero-based (first slide = 0)
- Two-way binding with `@bind-SelectedIndex` keeps your code property in sync
- Changing the property value programmatically switches the displayed slide
- The carousel will animate to the selected slide

## Selection Using Navigation Methods

Use `PreviousAsync()` and `NextAsync()` methods to navigate between slides programmatically. This is useful when you want to control navigation from external buttons or custom UI elements.

```cshtml
@using Syncfusion.Blazor.Buttons
@using Syncfusion.Blazor.Navigations

<SfCarousel @ref="@CarouselRef" 
            @bind-SelectedIndex="@CurrentIndex" 
            ButtonsVisibility="CarouselButtonVisibility.Hidden">
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

<div class="custom-navigation">
    <SfButton CssClass="w-auto" @onclick="@(()=>OnNavigationClick(CarouselSlideDirection.Previous))">
        Previous
    </SfButton>
    <SfButton CssClass="w-auto" @onclick="@(()=>OnNavigationClick(CarouselSlideDirection.Next))">
        Next
    </SfButton>
</div>

@code {
    SfCarousel CarouselRef;
    int CurrentIndex = 2;

    async Task OnNavigationClick(CarouselSlideDirection slideDirection)
    {
        if (slideDirection == CarouselSlideDirection.Previous)
        {
            await CarouselRef.PreviousAsync();
        }
        else
        {
            await CarouselRef.NextAsync();
        }
    }
}
```

**Methods Available:**
- `PreviousAsync()` - Navigate to the previous slide
- `NextAsync()` - Navigate to the next slide

**Use Cases:**
- Custom navigation buttons positioned outside the carousel
- Thumbnail navigation where clicking an image jumps to that slide
- Integration with external UI controls
- Programmatic slide transitions based on user actions

## Partial Visible Slides

The `PartialVisible` property enables showing the active slide plus partial views of the adjacent (previous and next) slides simultaneously. This creates a preview effect that helps users see what's coming next.

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel PartialVisible="true">
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

**Behavior with Loop Enabled:**
When both `PartialVisible="true"` and `Loop="true"` (default) are set:
- The last slide shows partially on the left at initial rendering
- The first slide shows in the center
- The second slide shows partially on the right
- Users see the continuous loop visually

**Important Note:**
Slide animation is only applicable when `PartialVisible` is enabled. The partial view feature enables smooth sliding transitions between items.

## Partial Visible Without Loop

When `Loop` is disabled with `PartialVisible` enabled, the behavior changes for the first and last slides:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel PartialVisible="true" Loop="false">
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

**Behavior with Loop Disabled:**
- At initial rendering, no previous slide shows on the left (since looping is disabled)
- Only the first slide (center) and second slide (partial right) are visible
- When reaching the last slide, no next slide shows on the right
- Navigation buttons will be disabled at the boundaries

**When to Use:**
- Linear workflows where users shouldn't loop back
- Tutorial slides with a clear start and end
- Step-by-step processes
- Galleries with finite content where looping doesn't make sense
