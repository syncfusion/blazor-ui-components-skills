# Templates and Customization

## Table of Contents
- [ItemTemplate](#itemtemplate)
- [SeparatorTemplate](#separatortemplate)
- [Template Context](#template-context)
- [Chip Component Integration](#chip-component-integration)
- [Specific Item Customization](#specific-item-customization)

---

## ItemTemplate

Use the `ItemTemplate` parameter to customize how individual breadcrumb items render:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb>
    <BreadcrumbItems>
        <BreadcrumbItem Text="Home"></BreadcrumbItem>
        <BreadcrumbItem Text="Products"></BreadcrumbItem>
        <BreadcrumbItem Text="Current"></BreadcrumbItem>
    </BreadcrumbItems>
    <BreadcrumbTemplates>
        <ItemTemplate>
            <span style="font-weight: bold; color: blue;">@context.Text</span>
        </ItemTemplate>
    </BreadcrumbTemplates>
</SfBreadcrumb>
```

The `context` parameter in `ItemTemplate` contains the current `BreadcrumbItem` object. You can access item properties like `Text`, `Url`, and `IconCss` to customize the rendering.

---

## SeparatorTemplate

Use the `SeparatorTemplate` parameter to customize the separator between breadcrumb items:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb>
    <BreadcrumbItems>
        <BreadcrumbItem Text="Home"></BreadcrumbItem>
        <BreadcrumbItem Text="Products"></BreadcrumbItem>
        <BreadcrumbItem Text="Details"></BreadcrumbItem>
    </BreadcrumbItems>
    <BreadcrumbTemplates>
        <SeparatorTemplate>
            <span class="e-icons e-arrow"></span>
        </SeparatorTemplate>
    </BreadcrumbTemplates>
</SfBreadcrumb>

<style>
    .e-arrow::before {
        content: '\e763';
        font-size: 12px;
    }
</style>
```

The `SeparatorTemplate` renders between each pair of breadcrumb items. Use it to display custom separators like arrows, slashes, or other visual elements.

---

## Template Context

The `context` parameter in templates gives access to the current `BreadcrumbItem` properties:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb>
    <BreadcrumbItems>
        <BreadcrumbItem Text="Home" Url="/" IconCss="e-icons e-home"></BreadcrumbItem>
        <BreadcrumbItem Text="Products" Url="/products"></BreadcrumbItem>
        <BreadcrumbItem Text="Current"></BreadcrumbItem>
    </BreadcrumbItems>
    <BreadcrumbTemplates>
        <ItemTemplate>
            <span>@context.Text (@context.Url)</span>
        </ItemTemplate>
    </BreadcrumbTemplates>
</SfBreadcrumb>
```

Access `context.Text`, `context.Url`, and `context.IconCss` to build dynamic item templates based on item data.

You can also rename the implicit `context` parameter using the `Context` attribute:

```cshtml
<ItemTemplate Context="item">
    <span>@item.Text</span>
</ItemTemplate>
```

---

## Chip Component Integration

Render breadcrumb items as Chip components using `ItemTemplate`:

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfBreadcrumb class="e-breadcrumb-chips">
    <BreadcrumbItems>
        <BreadcrumbItem Text="Cart"></BreadcrumbItem>
        <BreadcrumbItem Text="Billing"></BreadcrumbItem>
        <BreadcrumbItem Text="Shipping"></BreadcrumbItem>
        <BreadcrumbItem Text="Payment"></BreadcrumbItem>
    </BreadcrumbItems>
    <BreadcrumbTemplates>
        <ItemTemplate>
            <SfChip>
                <ChipItems>
                    <ChipItem Text="@context.Text" Enabled="true"></ChipItem>
                </ChipItems>
            </SfChip>
        </ItemTemplate>
    </BreadcrumbTemplates>
</SfBreadcrumb>
```

This renders each breadcrumb item as a Chip component, providing a modern, styled appearance for breadcrumb items.

---

## Specific Item Customization

Customize individual breadcrumb items by adding custom elements as child content:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb EnableNavigation="false" ActiveItem="@ActiveItem">
    <BreadcrumbItems>
        <BreadcrumbItem Text="Home" Url="/" />
        <BreadcrumbItem Text="Products" Url="/products" />
        <BreadcrumbItem Text="Electronics" Url="/electronics" />
        <BreadcrumbItem>
            <span class="e-searchfor-text">
                <span style="margin-right: 5px">Search for:</span>
                <a class="e-breadcrumb-text" href="./details" onclick="return false">Current Item</a>
            </span>
        </BreadcrumbItem>
    </BreadcrumbItems>
</SfBreadcrumb>

<style>
    .e-searchfor-text {
        display: flex;
        align-items: center;
        font-size: 14px;
        font-weight: normal;
    }

    .e-searchfor-text .e-breadcrumb-text {
        padding-left: 0;
    }
</style>

@code {
    private string ActiveItem = "";
}
```

Use child content within a `BreadcrumbItem` to render custom HTML for that specific item. This provides granular control over individual breadcrumb item appearance and behavior.
