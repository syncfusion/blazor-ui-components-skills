# Navigators and Indicators

## Table of Contents
- [Navigators](#navigators)
  - [Show or Hide Previous and Next Buttons](#show-or-hide-previous-and-next-buttons)
  - [Show Buttons on Hover](#show-buttons-on-hover)
  - [Previous and Next Button Templates](#previous-and-next-button-templates)
- [Indicators](#indicators)
  - [Show or Hide Indicators](#show-or-hide-indicators)
  - [Indicators Template](#indicators-template)
  - [Showing Preview in Indicators](#showing-preview-in-indicators)
  - [Indicator Types](#indicator-types)
- [Play Button](#play-button)
  - [Show or Hide Play Button](#show-or-hide-play-button)
  - [Play Button Template](#play-button-template)

## Navigators

Navigators are the previous and next buttons that allow users to manually transition between slides.

### Show or Hide Previous and Next Buttons

Control navigator button visibility using the `ButtonsVisibility` property. The available options are:

- `Visible` – Navigator buttons are always visible
- `Hidden` – Navigator buttons are not visible
- `VisibleOnHover` – Navigator buttons appear only when hovering over the carousel

**Example: Hide navigators completely**

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel ButtonsVisibility="CarouselButtonVisibility.Hidden">
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

**When to use:**
- Hide navigators for mobile-first designs relying on swipe gestures
- Clean minimal interfaces where indicators or thumbnails provide navigation
- Touch-focused applications

### Show Buttons on Hover

Display navigator buttons only when the user hovers their mouse over the carousel:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel ButtonsVisibility="CarouselButtonVisibility.VisibleOnHover">
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

**When to use:**
- Clean design that doesn't want buttons always visible
- Image galleries where buttons might distract from content
- Desktop-focused applications where hover is reliable

### Previous and Next Button Templates

Customize the appearance of navigator buttons using `PreviousButtonTemplate` and `NextButtonTemplate`:

```cshtml
@using Syncfusion.Blazor.Buttons
@using Syncfusion.Blazor.Navigations

<SfCarousel>
    <ChildContent>
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
    </ChildContent>
    <PreviousButtonTemplate>
        <SfButton CssClass="e-flat e-outline nav-btn" title="Previous">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 40 40" width="40" height="40">
                <path d="m13.5 7.01 13 13m-13 13 13-13"></path>
            </svg>
        </SfButton>
    </PreviousButtonTemplate>
    <NextButtonTemplate>
        <SfButton CssClass="e-flat e-outline nav-btn" title="Next">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 40 40" width="40" height="40">
                <path d="m13.5 7.01 13 13m-13 13 13-13"></path>
            </svg>
        </SfButton>
    </NextButtonTemplate>
</SfCarousel>

<style>
    .e-carousel .slide-content {
        align-items: center;
        display: flex;
        font-size: 1.25rem;
        height: 100%;
        justify-content: center;
    }

    .e-carousel .e-carousel-navigators .e-btn:active,
    .e-carousel .e-carousel-navigators .e-btn:hover {
        background-color: transparent !important;
    }

    .e-carousel .e-carousel-navigators .e-btn svg {
        fill: none;
        stroke: currentColor;
        stroke-linecap: square;
        stroke-width: 8px;
        height: 2rem;
        vertical-align: middle;
        width: 2rem;
    }

    .e-carousel .e-carousel-navigators .e-previous .e-btn svg {
        transform: rotate(180deg);
    }
</style>
```

**Template Customization Options:**
- Custom SVG or icon fonts
- Text labels ("Previous"/"Next")
- Images or background graphics
- Brand-specific button styles
- Animated icons

## Indicators

Indicators show the total number of slides and the currently active slide position.

### Show or Hide Indicators

Use the `ShowIndicators` property to control indicator visibility:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel ShowIndicators="false">
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

**When to hide indicators:**
- Single-slide or very few slides where position is obvious
- Minimal designs where screen space is limited
- Custom pagination UI implementations

### Indicators Template

Customize indicator appearance using the `IndicatorsTemplate`:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel ShowIndicators="true">
    <ChildContent>
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
    </ChildContent>
    <IndicatorsTemplate>
        @{
            string slideName = (context.Index + 1).ToString();
            <div class="indicator">
                <div class="fs-6">@slideName</div>
            </div>
        }
    </IndicatorsTemplate>
</SfCarousel>

<style>
    .e-carousel .slide-content {
        align-items: center;
        display: flex;
        font-size: 1.25rem;
        height: 100%;
        justify-content: center;
    }

    .e-carousel .e-carousel-indicators .e-indicator-bars .e-indicator-bar .indicator {
        background-color: #ECECEC;
        border-radius: 0.25rem;
        cursor: pointer;
        display: flex;
        height: 2rem;
        justify-content: center;
        margin: 0.5rem;
        width: 3rem;
    }

    .e-carousel .e-carousel-indicators .e-indicator-bars .e-indicator-bar .indicator .fs-6 {
        margin: auto;
    }

    .e-carousel .e-carousel-indicators .e-indicator-bars .e-indicator-bar.e-active .indicator {
        background-color: #3C78EF;
        color: #fff;
    }
</style>
```

**Template Context:**
- `context.Index` - Zero-based index of the slide
- Access slide properties to display custom information

**Customization Ideas:**
- Numbered indicators (1, 2, 3...)
- Letter indicators (A, B, C...)
- Custom icons or images
- Text labels describing each slide

### Showing Preview in Indicators

Display thumbnail previews of each slide in the indicators:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel CssClass="template-carousel">
    <ChildContent>
        @foreach (Bird bird in birds)
        {
            <CarouselItem>
                <figure class="img-container">
                    <img src="@("https://ej2.syncfusion.com/products/images/carousel/" + bird.ImageName + ".png")" 
                         alt="@bird.ImageName" 
                         style="height:100%;width:100%;" />
                </figure>
            </CarouselItem>
        }
    </ChildContent>
    <IndicatorsTemplate>
        <div class="indicator">
            <img src="@("https://ej2.syncfusion.com/products/images/carousel/" + previewImage[context.Index] + ".png")" 
                 alt="image" 
                 style="height:100%;width:100%;" />
        </div>
    </IndicatorsTemplate>
</SfCarousel>

@code {
    private List<string> previewImage = new List<string>() { 
        "cardinal", "hunei", "costa-rica", "kaohsiung", "bee-eater" 
    };
    
    public List<Bird> birds = new List<Bird> {
        new Bird { ID = 1, Name = "Cardinal", ImageName = "cardinal" },
        new Bird { ID = 2, Name = "Kingfisher", ImageName = "hunei" },
        new Bird { ID = 3, Name = "Keel-billed-toucan", ImageName = "costa-rica" },
        new Bird { ID = 4, Name = "Yellow-warbler", ImageName = "kaohsiung" },
        new Bird { ID = 5, Name = "Bee-eater", ImageName = "bee-eater" }
    };

    public class Bird
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public string ImageName { get; set; }
    }
}

<style>
    .template-carousel .e-carousel-items,
    .template-carousel .e-carousel-navigators {
        height: calc(100% - 3rem);
    }

    .template-carousel .e-carousel-indicators .e-indicator-bars .e-indicator-bar .indicator {
        background-color: #ECECEC;
        border-radius: 0.25rem;
        cursor: pointer;
        height: 2rem;
        margin: 0.6rem;
        width: 3rem;
    }

    .template-carousel .e-carousel-indicators .e-indicator-bars .e-indicator-bar .indicator img {
        padding: 2px;
    }

    .template-carousel .e-carousel-indicators .e-indicator-bars .e-indicator-bar.e-active .indicator {
        background-color: #3C78EF;
    }
</style>
```

**When to use preview indicators:**
- Image galleries where users need visual reference
- Product carousels to show all available items
- Portfolio showcases
- Video or media galleries

### Indicator Types

The carousel supports four built-in indicator types via the `IndicatorsType` property:

#### Default Indicator

Standard dot indicators showing current position:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel IndicatorsType="CarouselIndicatorType.Default">
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
```

**Appearance:** Traditional circular dots, active dot highlighted

#### Dynamic Indicator

Animated indicators that dynamically change to show position:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel IndicatorsType="CarouselIndicatorType.Dynamic">
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
```

**Appearance:** Dots with animation effects showing progression

#### Fraction Indicator

Shows current slide index and total count (e.g., "3 / 5"):

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel IndicatorsType="CarouselIndicatorType.Fraction">
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
```

**Appearance:** Text showing "1 / 5", "2 / 5", etc.

**When to use:** When users need exact position information

#### Progress Indicator

Displays a progress bar showing position through the carousel:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel IndicatorsType="CarouselIndicatorType.Progress">
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
```

**Appearance:** Horizontal bar filling from left to right

**When to use:** Onboarding flows, tutorials, step-by-step processes

## Play Button

The play button allows users to manually control auto-play functionality.

### Show or Hide Play Button

Enable the play/pause button using the `ShowPlayButton` property. This button appears with the navigation controls and allows users to start or stop automatic slide transitions.

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel ShowPlayButton="true">
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

**Important:** The `ShowPlayButton` property depends on `ButtonsVisibility`. If navigators are hidden, the play button won't appear.

**When to show play button:**
- Users need control over auto-play
- Content requires varied reading times
- Accessibility requirement for motion control

### Play Button Template

Customize the play/pause button appearance:

```cshtml
@using Syncfusion.Blazor.Buttons
@using Syncfusion.Blazor.Navigations

<SfCarousel AutoPlay="@IsSlidePlay" ShowPlayButton="true">
    <ChildContent>
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
    </ChildContent>
    <PlayButtonTemplate>
        <SfButton title="@(IsSlidePlay ? "Pause" : "Play")" 
                  IsToggle="true" 
                  @onclick="(()=> IsSlidePlay = !IsSlidePlay)">
            @if (IsSlidePlay)
            {
                <svg version="1.1" xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 32 32">
                    <path d="M16 0c-8.837 0-16 7.163-16 16s7.163 16 16 16 16-7.163 16-16-7.163-16-16-16zM16 29c-7.18 0-13-5.82-13-13s5.82-13 13-13 13 5.82 13 13-5.82 13-13 13zM10 10h4v12h-4zM18 10h4v12h-4z"></path>
                </svg>
            }
            else
            {
                <svg version="1.1" xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 32 32">
                    <path d="M16 0c-8.837 0-16 7.163-16 16s7.163 16 16 16 16-7.163 16-16-7.163-16-16-16zM16 29c-7.18 0-13-5.82-13-13s5.82-13 13-13 13 5.82 13 13-5.82 13-13 13zM12 9l12 7-12 7z"></path>
                </svg>
            }
        </SfButton>
    </PlayButtonTemplate>
</SfCarousel>

@code {
    bool IsSlidePlay = true;
}

<style>
    .e-carousel .slide-content {
        align-items: center;
        display: flex;
        font-size: 1.25rem;
        height: 100%;
        justify-content: center;
    }

    .e-carousel .e-carousel-navigators .e-btn:active,
    .e-carousel .e-carousel-navigators .e-btn:hover {
        background-color: transparent !important;
    }

    .e-carousel .e-carousel-navigators .e-btn svg {
        fill: #fff;
        stroke: transparent;
    }
</style>
```

**Template Features:**
- Custom play/pause icons
- State-based rendering (different icons for play vs pause)
- Tooltips for accessibility
- Brand-specific styling
