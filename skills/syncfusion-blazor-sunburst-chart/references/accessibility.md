# Accessibility

The `Blazor Sunburst Chart` component follows accessibility guidelines and standards, including ADA, Section 508, WCAG 2.2, and WAI-ARIA roles.

## Table of Contents
- [Accessibility compliance](#accessibility-compliance)
- [WAI-ARIA attributes](#wai-aria-attributes)
  - [Accessibility-aware properties](#accessibility-aware-properties)
- [Keyboard navigation](#keyboard-navigation)
- [Accessibility-aware behavior](#accessibility-aware-behavior)
- [Ensuring accessibility](#ensuring-accessibility)

## Accessibility compliance

| Accessibility Criteria | Compatibility |
| -- | -- |
| WCAG 2.2 Support | AA |
| Section 508 Support | Full |
| Screen Reader Support | Full |
| Right-To-Left Support | Full |
| Color Contrast | Full |
| Mobile Device Support | Full |
| Keyboard Navigation Support | Full |
| Axe-core Accessibility Validation | Full |

## WAI-ARIA attributes

The chart follows the WAI-ARIA tree view pattern to meet accessibility requirements.

| Element | Default description |
|---|---|
| Segment | Reads the hierarchy path and value of the focused segment. |
| Level | Reads the level index and the category represented by the segments. |
| Breadcrumb | Reads the current drill path from root to the active node. |
| Legend | Reads the category name and is activated to show or hide the corresponding branch. |
| Title | Reads the Sunburst Chart title. |
| Subtitle | Reads the Sunburst Chart subtitle. |
| Tooltip | Reads the hierarchy path and the value of the hovered segment. |

ARIA roles and attributes used in the component:

- Roles: `img`, `button`, `region`
- Attributes: `aria-label`, `aria-hidden`, `aria-pressed`, `aria-roledescription`, `aria-describedby`

### Accessibility-aware properties

The component exposes the following properties, which let you customize how the chart is announced by assistive technologies:

- `AccessibilityDescription` - Provides a text description for the Sunburst Chart root element, enhancing screen reader support.
- `AccessibilityRole` - Specifies the role of the Sunburst Chart, helping assistive technologies to identify the element appropriately.
- `Focusable` - Allows the Sunburst Chart to receive focus so it can participate in keyboard navigation.
- `FocusBorderColor` - Defines the color of the focus indicator that surrounds the focused segment.
- `FocusBorderMargin` - Defines the margin between the focused segment and its focus indicator.
- `FocusBorderWidth` - Defines the width of the focus indicator that surrounds the focused segment.

## Keyboard navigation

The component uses a single roving `tabindex` on the chart root, so only one segment is reachable through `Tab` at a time. Once focus is inside the chart, the following keyboard shortcuts let users explore the hierarchy, drill into a branch, return to the parent, and operate the legend.

| Windows | Mac | Description |
|-----|-----|-----|
| `Tab` | `Tab` | Moves focus to the next focusable element (chart, legend, or breadcrumbs). |
| `Shift` + `Tab` | `Shift` + `Tab` | Moves focus to the previous focusable element. |
| `→` | `→` | Moves focus to the next segment clockwise within the same ring. |
| `←` | `←` | Moves focus to the previous segment counter-clockwise within the same ring. |
| `↑` | `↑` | Moves focus to the segment in the next outer ring. |
| `↓` | `↓` | Moves focus to the segment in the next inner ring. |
| `Home` | `Home` | Moves focus to the first segment in the current ring. |
| `End` | `End` | Moves focus to the last segment in the current ring. |
| `Enter` / `Space` (legend) | `Enter` / `Space` (legend) | Toggles the visibility of the category represented by the focused legend item. |
| `↑` / `↓` / `←` / `→` (legend) | `↑` / `↓` / `←` / `→` (legend) | Moves focus between legend items. |

## Accessibility-aware behavior

Beyond ARIA attributes and keyboard navigation, the chart provides the following behaviors:

- **WCAG 2.2 AA target** — Targets WCAG 2.2 AA criteria including color contrast, non-color state indicators, and focus visibility.
- **Roving tab stop** — A single tab stop for the segment collection; arrow keys move focus between segments, and `Tab` leaves the chart.
- **Focus restoration** — After a drill-down, drill-up, or data refresh, focus is restored to the equivalent logical segment.
- **Accessible segment names** — Every segment exposes a meaningful accessible name combining its hierarchy path and value.
- **Keyboard-accessible tooltip** — Tooltip information is announced to assistive technologies, so users who cannot rely on pointer hover still receive hierarchy path and value.
- **Drill activation** — Drill-down and drill-up are triggered by double-click, double-tap (within 400 ms), breadcrumb click, or `Enter` on a keyboard-focused segment. `Space` and `Escape` are not bound to drill navigation. See [Drill-Down](drill-down.md) for the full drill configuration and the 400 ms double-tap threshold.
- **Reduced motion support** — Animations honor the user's `prefers-reduced-motion` setting.
- **Non-color state indicators** — Selection, highlight, and focus are indicated by more than color alone (opacity, border, or ring emphasis).
- **Forced-colors support** — Focus indicators, selection, and segment borders remain visible in high-contrast or forced-colors mode.
- **Responsive accessibility** — At narrow widths, when inner labels are suppressed, every visible segment remains reachable through tooltip, legend, keyboard navigation, and the accessible summary.

## Ensuring accessibility

The component's accessibility levels are ensured through automated axe-core checks combined with manual keyboard and screen reader testing. The acceptance set verifies WCAG 2.2 AA target behavior, the roving segment tab stop, deterministic arrow, `Home`, `End`, `Enter`, `Space`, and `Escape` keys, focus restoration after drill-down or data updates, accessible path and value names, keyboard-accessible tooltip information, reduced motion, non-color state indicators, and forced-colors support.
