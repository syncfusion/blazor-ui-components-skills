# Accessibility

## Table of Contents
- [Accessibility Compliance](#accessibility-compliance)
- [WAI-ARIA Attributes](#wai-aria-attributes)
- [Keyboard Interaction](#keyboard-interaction)
- [Disabling Keyboard Interaction](#disabling-keyboard-interaction)

## Accessibility Compliance

The Blazor Carousel component has been designed following [WAI-ARIA](https://www.w3.org/WAI/ARIA/apg/patterns/carousel/) specifications and implements WAI-ARIA roles, states, and properties along with keyboard support. This makes the component usable for people who rely on assistive technologies.

### Standards Supported

The carousel complies with the following accessibility standards:

| Accessibility Criteria | Compliance Level |
|------------------------|------------------|
| [WCAG 2.2](https://www.w3.org/TR/WCAG22/) Support | AA |
| [Section 508](https://www.section508.gov/) Support | Yes |
| Right-To-Left Support | Yes |
| Color Contrast | Yes |
| Mobile Device Support | Yes |
| Keyboard Navigation Support | Yes |
| [Axe-core](https://www.nuget.org/packages/Deque.AxeCore.Playwright) Accessibility Validation | Yes |

The component has been tested with major assistive technologies and meets requirements for:
- **ADA (Americans with Disabilities Act)** compliance
- **Section 508** federal accessibility standards
- **WCAG 2.2 Level AA** guidelines

## WAI-ARIA Attributes

The Blazor Carousel component implements the following ARIA attributes to provide information about elements for assistive technology:

| Attribute | Purpose | Applied To |
|-----------|---------|------------|
| `aria-roledescription` | Describes the element's role | Root carousel element and each slide (value: "carousel" and "slide") |
| `aria-label` | Provides accessible name | Previous button, next button, play/pause button, and all indicator elements |
| `aria-current` | Indicates current item | Active indicator element (set to `true` for the active slide indicator) |
| `aria-hidden` | Hides from assistive tech | All carousel elements except the currently visible item (set to `true` for hidden slides) |
| `aria-live` | Announces dynamic changes | Carousel items element (set to `off` when `AutoPlay` is `true`, set to `polite` when `AutoPlay` is `false`) |
| `aria-role` | Defines element role | Carousel slide item (set to `group`) |

### How ARIA Attributes Work

**aria-roledescription:**
- Tells screen readers this is a "carousel" component
- Each slide is identified as a "slide"

**aria-label:**
- Navigation buttons are labeled "Previous", "Next", "Play", "Pause"
- Indicators are labeled with their position (e.g., "Go to slide 1")

**aria-current:**
- Marks the active indicator dot/element as current
- Helps users understand which slide is displayed

**aria-hidden:**
- Hides inactive slides from screen readers
- Prevents confusion by focusing only on visible content
- Automatically managed as slides change

**aria-live:**
- When auto-play is ON: Set to "off" to avoid interrupting users
- When auto-play is OFF: Set to "polite" to announce slide changes
- Balances information with user experience

**aria-role="group":**
- Groups each slide's content together
- Helps screen readers understand slide boundaries

## Keyboard Interaction

The carousel implements full keyboard navigation support following WAI-ARIA practices. Once the carousel has focus, users can interact using the following keys:

### Keyboard Shortcuts

| Windows | Mac | Action |
|---------|-----|--------|
| <kbd>Alt</kbd> + <kbd>J</kbd> | <kbd>⌥</kbd> + <kbd>J</kbd> | Focus the carousel component (implemented at application level) |
| <kbd>←</kbd> <kbd>→</kbd> | <kbd>←</kbd> <kbd>→</kbd> | Navigate between slides (left/right arrow keys) |
| <kbd>Home</kbd> | <kbd>Home</kbd> | Navigate to the first slide |
| <kbd>End</kbd> | <kbd>End</kbd> | Navigate to the last slide |
| <kbd>Space</kbd> | <kbd>Space</kbd> | Play or pause slide transitions |
| <kbd>Enter</kbd> | <kbd>Enter</kbd> | Perform the action for the focused element (e.g., click an indicator) |

### Default Behavior

Keyboard interaction is **enabled by default** through the `AllowKeyboardInteraction` property (set to `true`). Users can navigate slides immediately after the carousel receives focus.

**Example with default keyboard navigation:**

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
</SfCarousel>
```

Users can immediately use arrow keys to navigate once the carousel has focus.

## Disabling Keyboard Interaction

In some scenarios, you may want to disable keyboard navigation, particularly when:
- The carousel contains input elements (textboxes, buttons, etc.)
- Arrow key presses should not trigger slide changes
- You want to prevent accidental navigation

Set `AllowKeyboardInteraction="false"` to disable default keyboard shortcuts:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfCarousel AllowKeyboardInteraction="false">
    <CarouselItem>
        <div class="slide-content">
            <h3>Contact Form</h3>
            <input type="text" placeholder="Name" />
            <input type="email" placeholder="Email" />
            <textarea placeholder="Message"></textarea>
        </div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">
            <h3>Survey</h3>
            <input type="text" placeholder="Question 1" />
            <input type="text" placeholder="Question 2" />
        </div>
    </CarouselItem>
    <CarouselItem>
        <div class="slide-content">
            <h3>Thank You</h3>
            <p>Your submission has been received.</p>
        </div>
    </CarouselItem>
</SfCarousel>

<style>
    .slide-content {
        padding: 2rem;
    }

    .slide-content input,
    .slide-content textarea {
        display: block;
        margin: 0.5rem 0;
        width: 100%;
        padding: 0.5rem;
    }
</style>
```

**When to disable keyboard interaction:**
- Forms within carousel slides where arrow keys navigate form fields
- Text inputs where left/right arrows move the cursor
- Rich text editors or code editors in slides
- Any scenario where carousel navigation conflicts with content interaction

**Alternative Navigation:**
When keyboard interaction is disabled, users can still navigate using:
- Mouse clicks on navigation buttons
- Touch swipe gestures
- Clicking indicator dots
- Programmatic methods (if you provide custom controls)

## Accessibility Best Practices

### Provide Meaningful Alt Text
Always include descriptive `alt` attributes for images:

```cshtml
<CarouselItem>
    <img src="product1.jpg" alt="Red leather sofa with chrome legs, front view" />
</CarouselItem>
```

### Use Proper Color Contrast
Ensure navigation buttons and indicators have sufficient contrast against the carousel background. The component's default themes meet WCAG AA standards, but verify when using custom styling.

### Don't Rely Solely on Color
Use additional indicators beyond color alone (e.g., shape, text, icons) to convey information.

### Respect User Preferences
If auto-play is enabled, provide a pause button:

```cshtml
<SfCarousel AutoPlay="true" ShowPlayButton="true">
    <!-- carousel items -->
</SfCarousel>
```

This allows users with cognitive disabilities or motion sensitivity to control animation.

### Caption Important Content
When slides contain images that convey information, provide text alternatives:

```cshtml
<CarouselItem>
    <figure>
        <img src="chart.png" alt="Sales growth chart" />
        <figcaption>
            Sales increased 45% year-over-year, reaching $2.3M in Q4 2025.
        </figcaption>
    </figure>
</CarouselItem>
```

### Test with Assistive Technologies
The Blazor Carousel has been validated with:
- **JAWS** (Job Access With Speech)
- **NVDA** (NonVisual Desktop Access)
- **Narrator** (Windows built-in screen reader)
- **VoiceOver** (macOS/iOS screen reader)
- **axe-core** automated accessibility testing

You can test accessibility using the [Syncfusion accessibility demo](https://blazor.syncfusion.com/accessibility/carousel).

## Focus Management

The carousel properly manages focus when:
- Users navigate between slides (focus remains on carousel)
- Modal dialogs or popups open from slides
- Slides contain interactive elements (links, buttons)

If your slides contain focusable elements, the carousel preserves natural tab order:

```cshtml
<SfCarousel>
    <CarouselItem>
        <h2>Slide 1</h2>
        <button>Action 1</button>
        <a href="#">Learn More</a>
    </CarouselItem>
    <CarouselItem>
        <h2>Slide 2</h2>
        <button>Action 2</button>
        <a href="#">Learn More</a>
    </CarouselItem>
</SfCarousel>
```

Users can tab through interactive elements within the active slide, then continue to carousel controls.
