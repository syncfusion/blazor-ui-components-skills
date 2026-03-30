# Customization & Styling

Learn how to customize the appearance and styling of the DateTimePicker component.

## Theme Integration

### Applying Built-in Themes

Syncfusion provides multiple built-in themes. Add the theme CSS to your `App.razor` or layout file:

```html
<!-- Bootstrap 5 Theme (Default) -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Material Design Theme -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Fluent UI Theme -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />

<!-- Tailwind CSS Theme -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />

<!-- Office Fabric Theme -->
<link href="_content/Syncfusion.Blazor.Themes/fabric.css" rel="stylesheet" />
```

### Dark Mode Themes

```html
<!-- Bootstrap 5 Dark -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5-dark.css" rel="stylesheet" />

<!-- Material Dark -->
<link href="_content/Syncfusion.Blazor.Themes/material-dark.css" rel="stylesheet" />

<!-- Fluent Dark -->
<link href="_content/Syncfusion.Blazor.Themes/fluent-dark.css" rel="stylesheet" />

<!-- Tailwind Dark -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind-dark.css" rel="stylesheet" />
```

## CSS Customization

### Component Class Customization

```blazor
@page "/datetimepicker-styling"
@using Syncfusion.Blazor.Calendars

<SfDateTimePicker TValue="DateTime?" 
                  CssClass="custom-datetimepicker">
</SfDateTimePicker>

<style>
    .custom-datetimepicker {
        width: 100%;
        max-width: 300px;
    }
    
    .custom-datetimepicker .e-input-group {
        background-color: #f5f5f5;
    }
    
    .custom-datetimepicker .e-input {
        border: 2px solid #007bff;
        padding: 10px;
        border-radius: 4px;
    }
    
    .custom-datetimepicker .e-input:hover {
        border-color: #0056b3;
    }
</style>
```

### Input Container Styling

```blazor
<div class="datetimepicker-container">
    <SfDateTimePicker TValue="DateTime?" 
                      Placeholder="Select date and time">
    </SfDateTimePicker>
</div>

<style>
    .datetimepicker-container {
        max-width: 400px;
        margin: 20px 0;
    }
    
    .datetimepicker-container .e-input-group {
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    }
    
    .datetimepicker-container .e-input {
        font-size: 14px;
        padding: 12px 15px;
    }
</style>
```

## Popup Calendar Styling

### Style Calendar Popup

```blazor
@page "/datetimepicker-popup-style"
@using Syncfusion.Blazor.Calendars

<SfDateTimePicker TValue="DateTime?" 
                  CssClass="popup-style">
</SfDateTimePicker>

<style>
    /* Calendar popup styles */
    .popup-style .e-datetimepicker .e-popup {
        border: 2px solid #007bff;
        border-radius: 6px;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    }
    
    /* Calendar header */
    .popup-style .e-calendar .e-header {
        background: linear-gradient(135deg, #007bff 0%, #0056b3 100%);
        color: white;
        padding: 12px;
    }
    
    /* Today button */
    .popup-style .e-calendar .e-btn {
        background-color: #28a745;
        color: white;
        border: none;
        border-radius: 4px;
        padding: 8px 16px;
    }
    
    /* Hover effect on dates */
    .popup-style .e-calendar .e-content table td:hover {
        background-color: #e7f3ff;
        cursor: pointer;
    }
</style>
```

## Custom Appearance Options

### Disabled State Styling

```blazor
@page "/datetimepicker-disabled"
@using Syncfusion.Blazor.Calendars

<div>
    <h4>Enabled:</h4>
    <SfDateTimePicker TValue="DateTime?" 
                      Enabled="true">
    </SfDateTimePicker>
    
    <h4 style="margin-top: 20px;">Disabled:</h4>
    <SfDateTimePicker TValue="DateTime?" 
                      Enabled="false"
                      Placeholder="Disabled state">
    </SfDateTimePicker>
</div>

<style>
    .e-datetimepicker:disabled,
    .e-datetimepicker.e-disabled {
        opacity: 0.6;
        cursor: not-allowed;
        background-color: #f5f5f5;
    }
</style>
```

### Read-Only State

```blazor
@page "/datetimepicker-readonly"
@using Syncfusion.Blazor.Calendars

<SfDateTimePicker TValue="DateTime?" 
                  Readonly="true"
                  Value="@DateTime.Now">
</SfDateTimePicker>

<style>
    .e-datetimepicker.e-readonly .e-input {
        cursor: text;
        background-color: #f9f9f9;
    }
</style>
```

## Responsive Design

### Mobile-Friendly DateTimePicker

```blazor
@page "/datetimepicker-responsive"
@using Syncfusion.Blazor.Calendars

<div class="datetimepicker-responsive">
    <SfDateTimePicker TValue="DateTime?" 
                      Placeholder="Select date and time"
                      CssClass="responsive-input">
    </SfDateTimePicker>
</div>

<style>
    .datetimepicker-responsive {
        padding: 10px;
    }
    
    /* Mobile (up to 768px) */
    @media (max-width: 768px)
    {
        .datetimepicker-responsive {
            padding: 5px;
        }
        
        .datetimepicker-responsive .e-input {
            font-size: 16px; /* Prevents zoom on iOS */
            width: 100%;
        }
        
        .datetimepicker-responsive .e-popup {
            width: 100% !important;
            max-height: 400px;
        }
    }
    
    /* Tablet (768px - 1024px) */
    @media (min-width: 768px) and (max-width: 1024px)
    {
        .datetimepicker-responsive {
            max-width: 500px;
        }
    }
    
    /* Desktop (1024px+) */
    @media (min-width: 1024px)
    {
        .datetimepicker-responsive {
            max-width: 400px;
        }
    }
</style>
```

## Advanced Styling Patterns

### Pattern 1: Material Design Style

```blazor
@page "/datetimepicker-material"
@using Syncfusion.Blazor.Calendars

<div class="material-design-container">
    <SfDateTimePicker TValue="DateTime?" 
                      Placeholder="Pick date and time"
                      CssClass="material-input">
    </SfDateTimePicker>
</div>

<style>
    .material-design-container {
        background: white;
        border-radius: 4px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        padding: 20px;
    }
    
    .material-design-container .material-input {
        width: 100%;
    }
    
    .material-design-container .e-input {
        border: none;
        border-bottom: 2px solid #9e9e9e;
        padding: 10px 0;
        font-size: 15px;
        transition: border-color 0.3s ease;
    }
    
    .material-design-container .e-input:focus {
        border-bottom-color: #1976d2;
        outline: none;
    }
    
    .material-design-container .e-input::placeholder {
        color: #9e9e9e;
    }
</style>
```

### Pattern 2: Custom Icons and Buttons

```blazor
@page "/datetimepicker-custom-icon"
@using Syncfusion.Blazor.Calendars

<div class="custom-icon-container">
    <label>Appointment Date:</label>
    <SfDateTimePicker TValue="DateTime?" 
                      Placeholder="Click calendar icon"
                      CssClass="custom-icon-picker">
    </SfDateTimePicker>
</div>

<style>
    .custom-icon-container {
        margin: 20px 0;
    }
    
    .custom-icon-container .e-input-group .e-input-group-icon {
        background-color: #007bff;
        color: white;
        border-radius: 0 4px 4px 0;
        transition: background-color 0.3s;
    }
    
    .custom-icon-container .e-input-group .e-input-group-icon:hover {
        background-color: #0056b3;
    }
    
    .custom-icon-picker .e-input-group-icon::before {
        font-size: 18px;
    }
</style>
```

### Pattern 3: Gradient Borders

```blazor
@page "/datetimepicker-gradient"
@using Syncfusion.Blazor.Calendars

<SfDateTimePicker TValue="DateTime?" 
                  Placeholder="Gradient style"
                  CssClass="gradient-border">
</SfDateTimePicker>

<style>
    .gradient-border .e-input {
        background: linear-gradient(white, white) padding-box,
                    linear-gradient(135deg, #667eea 0%, #764ba2 100%) border-box;
        border: 2px solid transparent;
        border-radius: 4px;
        padding: 10px 12px;
    }
    
    .gradient-border .e-input:focus {
        background: linear-gradient(white, white) padding-box,
                    linear-gradient(135deg, #f093fb 0%, #f5576c 100%) border-box;
    }
</style>
```

## CSS Variables

### Custom Property Styling

```blazor
@page "/datetimepicker-css-vars"
@using Syncfusion.Blazor.Calendars

<div class="themed-container">
    <SfDateTimePicker TValue="DateTime?" 
                      Placeholder="Custom theme"
                      CssClass="css-var-input">
    </SfDateTimePicker>
</div>

<style>
    :root {
        --primary-color: #007bff;
        --primary-dark: #0056b3;
        --input-padding: 12px;
        --border-radius: 6px;
        --shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    }
    
    .themed-container .css-var-input {
        width: 100%;
        max-width: 400px;
    }
    
    .themed-container .e-input {
        border: 2px solid var(--primary-color);
        border-radius: var(--border-radius);
        padding: var(--input-padding);
        box-shadow: var(--shadow);
    }
    
    .themed-container .e-input:focus {
        border-color: var(--primary-dark);
        box-shadow: 0 4px 12px rgba(0, 86, 179, 0.15);
    }
</style>
```

## Complete Styling Example

Combining multiple styling approaches:

```blazor
@page "/datetimepicker-complete-style"
@using Syncfusion.Blazor.Calendars

<div class="appointment-form">
    <h3>Book Your Appointment</h3>
    
    <div class="form-group">
        <label for="appointment-date">Select Date & Time:</label>
        <SfDateTimePicker TValue="DateTime?" 
                          ID="appointment-date"
                          Placeholder="MM/DD/YYYY HH:MM"
                          Mask="00/00/0000 00:00"
                          Format="MM/dd/yyyy HH:mm"
                          CssClass="appointment-input">
        </SfDateTimePicker>
    </div>
    
    <button class="submit-btn">Book Appointment</button>
</div>

<style>
    .appointment-form {
        max-width: 500px;
        margin: 30px auto;
        padding: 30px;
        background: white;
        border-radius: 8px;
        box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
    }
    
    .appointment-form h3 {
        color: #333;
        margin-bottom: 25px;
        text-align: center;
    }
    
    .form-group {
        margin-bottom: 20px;
    }
    
    .form-group label {
        display: block;
        margin-bottom: 8px;
        color: #555;
        font-weight: 500;
    }
    
    .appointment-input {
        width: 100%;
    }
    
    .appointment-input .e-input {
        border: 2px solid #e0e0e0;
        border-radius: 4px;
        padding: 12px 15px;
        font-size: 14px;
        transition: all 0.3s ease;
    }
    
    .appointment-input .e-input:focus {
        border-color: #007bff;
        box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.1);
        outline: none;
    }
    
    .submit-btn {
        width: 100%;
        padding: 12px;
        background: linear-gradient(135deg, #007bff 0%, #0056b3 100%);
        color: white;
        border: none;
        border-radius: 4px;
        font-size: 16px;
        font-weight: 600;
        cursor: pointer;
        transition: transform 0.2s, box-shadow 0.2s;
    }
    
    .submit-btn:hover {
        transform: translateY(-2px);
        box-shadow: 0 4px 12px rgba(0, 123, 255, 0.3);
    }
    
    .submit-btn:active {
        transform: translateY(0);
    }
</style>
```

---

**Related Topics:**
- [Getting Started](getting-started.md) - Basic component setup
- [Date & Time Formatting](date-time-formatting.md) - Format display
- [Data Binding & Events](data-binding-and-events.md) - Interactive styling
