# Navigation and Routing

## Table of Contents
- [Relative URL Navigation](#relative-url-navigation)
- [Absolute URL Navigation](#absolute-url-navigation)
- [Disabling Navigation](#disabling-navigation)
- [Active Item Navigation](#active-item-navigation)

---

## Relative URL Navigation

Specify relative URLs in breadcrumb items to navigate within your application:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb>
    <BreadcrumbItems>
        <BreadcrumbItem Text="Home" Url="../"></BreadcrumbItem>
        <BreadcrumbItem Text="Breadcrumb" Url="./breadcrumb/getting-started"></BreadcrumbItem>
        <BreadcrumbItem Text="Default" Url="../"></BreadcrumbItem>
        <BreadcrumbItem Text="Icons" Url="./breadcrumb/icons"></BreadcrumbItem>
        <BreadcrumbItem Text="Navigation" Url="./breadcrumb/navigation"></BreadcrumbItem>
        <BreadcrumbItem Text="Overflow" Url="./breadcrumb/overflow"></BreadcrumbItem>
    </BreadcrumbItems>
</SfBreadcrumb>
```

Relative URLs use:
- `./` for sibling paths
- `../` for parent paths
- `../../` for higher-level paths

These URLs are resolved relative to the current page's location.

---

## Absolute URL Navigation

Specify absolute URLs for external links or fully-qualified internal paths:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb>
    <BreadcrumbItems>
        <BreadcrumbItem Text="Home" Url="https://blazor.syncfusion.com/documentation/breadcrumb/introduction"></BreadcrumbItem>
        <BreadcrumbItem Text="Getting" Url="https://blazor.syncfusion.com/documentation/breadcrumb/getting-started"></BreadcrumbItem>
        <BreadcrumbItem Text="Icons" Url="https://blazor.syncfusion.com/documentation/breadcrumb/icons"></BreadcrumbItem>
        <BreadcrumbItem Text="Navigation" Url="https://blazor.syncfusion.com/documentation/breadcrumb/navigation"></BreadcrumbItem>
        <BreadcrumbItem Text="Overflow" Url="https://blazor.syncfusion.com/documentation/breadcrumb/overflow"></BreadcrumbItem>
    </BreadcrumbItems>
</SfBreadcrumb>
```

Absolute URLs include the full scheme and host (e.g., `https://example.com/path`). When clicked, they navigate to the specified URL directly, regardless of the current application context.

---

## Disabling Navigation

By default, breadcrumb items with a `Url` property navigate when clicked. To prevent navigation, set the `EnableNavigation` property to `false`:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb EnableNavigation="false">
    <BreadcrumbItems>
        <BreadcrumbItem Text="Home" Url="https://blazor.syncfusion.com/documentation/breadcrumb/introduction"></BreadcrumbItem>
        <BreadcrumbItem Text="Getting" Url="https://blazor.syncfusion.com/documentation/breadcrumb/getting-started"></BreadcrumbItem>
        <BreadcrumbItem Text="Icons" Url="https://blazor.syncfusion.com/documentation/breadcrumb/icons"></BreadcrumbItem>
        <BreadcrumbItem Text="Navigation" Url="https://blazor.syncfusion.com/documentation/breadcrumb/navigation"></BreadcrumbItem>
        <BreadcrumbItem Text="Overflow" Url="https://blazor.syncfusion.com/documentation/breadcrumb/overflow"></BreadcrumbItem>
    </BreadcrumbItems>
</SfBreadcrumb>
```

When `EnableNavigation="false"`, breadcrumb items render as static elements without navigation functionality. This is useful for:
- Display-only breadcrumbs
- Custom navigation logic via C# event handlers
- Workflows where navigation should be prevented

---

## Active Item Navigation

By default, the last (active) breadcrumb item does not trigger navigation when clicked. To enable navigation for the active item, set `EnableActiveItemNavigation` to `true`:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb EnableActiveItemNavigation="true">
    <BreadcrumbItems>
        <BreadcrumbItem Text="Home" Url="https://blazor.syncfusion.com/documentation/breadcrumb/introduction"></BreadcrumbItem>
        <BreadcrumbItem Text="Getting" Url="https://blazor.syncfusion.com/documentation/breadcrumb/getting-started"></BreadcrumbItem>
        <BreadcrumbItem Text="Icons" Url="https://blazor.syncfusion.com/documentation/breadcrumb/icons"></BreadcrumbItem>
        <BreadcrumbItem Text="Navigation" Url="https://blazor.syncfusion.com/documentation/breadcrumb/navigation"></BreadcrumbItem>
        <BreadcrumbItem Text="Overflow" Url="https://blazor.syncfusion.com/documentation/breadcrumb/overflow"></BreadcrumbItem>
    </BreadcrumbItems>
</SfBreadcrumb>
```

When `EnableActiveItemNavigation="true"`, the last item becomes clickable and navigates to its specified `Url`. This enables breadcrumb-based refresh or re-navigation scenarios.

**Default behavior:** The last item is typically not clickable because it represents the current page. Set this property to `true` only if your use case requires active item navigation.
