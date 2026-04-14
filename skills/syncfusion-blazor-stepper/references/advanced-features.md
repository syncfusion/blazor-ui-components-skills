# Advanced Features

## Table of Contents

- [Step Validation](#step-validation)
  - [Validation States](#validation-states)
  - [Form Validation Example](#form-validation-example)
- [Tooltip Support](#tooltip-support)
  - [ShowTooltip Property](#showtooltip-property)
- [Step Content Templates](#step-content-templates)
- [Step Completion Tracking](#step-completion-tracking)
- [Conditional Step Display](#conditional-step-display)
- [Error Handling and Recovery](#error-handling-and-recovery)

## Step Validation

The Stepper component supports visual validation states for each step using the `IsValid` property. Steps can display success or error indicators based on validation results.

### Validation States

- `IsValid="true"` - Display success/checkmark icon
- `IsValid="false"` - Display error icon
- `IsValid="null"` - No validation indicator (default)

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper>
    <StepperSteps>
        <StepperStep Label="Cart" IsValid="true"></StepperStep>
        <StepperStep Label="Address"></StepperStep>
        <StepperStep Label="Payment" IsValid="false"></StepperStep>
        <StepperStep Label="Confirmation"></StepperStep>
    </StepperSteps>
</SfStepper>
```

### Form Validation Example

```csharp
@using Syncfusion.Blazor.Navigations
@using System.ComponentModel.DataAnnotations

<SfStepper @ref="stepper" StepChanging="@ValidateStep">
    <StepperSteps>
        <StepperStep @ref="step1" Label="Personal Info" IconCss="sf-icon-user"></StepperStep>
        <StepperStep @ref="step2" Label="Address" IconCss="sf-icon-location"></StepperStep>
        <StepperStep @ref="step3" Label="Payment" IconCss="sf-icon-payment"></StepperStep>
    </StepperSteps>
</SfStepper>

<EditForm Model="@formModel" OnSubmit="@HandleSubmit">
    <DataAnnotationsValidator />
    
    <div class="form-container">
        @if (currentStep == 0)
        {
            <div class="form-group">
                <label>Name:</label>
                <InputText @bind-Value="@formModel.Name" />
                <ValidationMessage For="@(() => formModel.Name)" />
            </div>
            <div class="form-group">
                <label>Email:</label>
                <InputText @bind-Value="@formModel.Email" />
                <ValidationMessage For="@(() => formModel.Email)" />
            </div>
        }
        else if (currentStep == 1)
        {
            <div class="form-group">
                <label>Street:</label>
                <InputText @bind-Value="@formModel.Street" />
                <ValidationMessage For="@(() => formModel.Street)" />
            </div>
            <div class="form-group">
                <label>City:</label>
                <InputText @bind-Value="@formModel.City" />
                <ValidationMessage For="@(() => formModel.City)" />
            </div>
        }
        else if (currentStep == 2)
        {
            <div class="form-group">
                <label>Card Number:</label>
                <InputText @bind-Value="@formModel.CardNumber" />
                <ValidationMessage For="@(() => formModel.CardNumber)" />
            </div>
        }
    </div>
    
    <button type="submit" class="btn-submit">
        @(currentStep < 2 ? "Next" : "Submit")
    </button>
</EditForm>

@code {
    private class FormModel
    {
        [Required(ErrorMessage = "Name is required")]
        public string Name { get; set; }
        
        [Required(ErrorMessage = "Email is required")]
        [EmailAddress]
        public string Email { get; set; }
        
        [Required(ErrorMessage = "Street is required")]
        public string Street { get; set; }
        
        [Required(ErrorMessage = "City is required")]
        public string City { get; set; }
        
        [Required(ErrorMessage = "Card number is required")]
        public string CardNumber { get; set; }
    }
    
    private SfStepper stepper;
    private StepperStep step1, step2, step3;
    private FormModel formModel = new FormModel();
    private int currentStep = 0;
    
    private async Task ValidateStep(StepperChangeEventArgs args)
    {
        if (args.ActiveStep > args.PreviousStep)
        {
            var isValid = await ValidateCurrentStep();
            args.Cancel = !isValid;
            
            // Update step validation indicator
            UpdateStepValidation(args.PreviousStep, isValid);
        }
    }
    
    private async Task<bool> ValidateCurrentStep()
    {
        var editContext = new EditContext(formModel);
        var validator = new DataAnnotationsValidator();
        validator.Validate(editContext);
        
        return editContext.GetValidationMessages().Count() == 0;
    }
    
    private void UpdateStepValidation(int stepIndex, bool isValid)
    {
        if (stepIndex == 0) step1.IsValid = isValid;
        else if (stepIndex == 1) step2.IsValid = isValid;
        else if (stepIndex == 2) step3.IsValid = isValid;
    }
    
    private void HandleSubmit()
    {
        Console.WriteLine("Form submitted successfully!");
    }
}

<style>
    .form-container {
        margin: 20px 0;
        padding: 20px;
        border: 1px solid #ddd;
        border-radius: 4px;
        background-color: #fafafa;
    }
    
    .form-group {
        margin-bottom: 15px;
    }
    
    .form-group label {
        display: block;
        font-weight: 600;
        margin-bottom: 5px;
    }
    
    .form-group input {
        width: 100%;
        padding: 8px;
        border: 1px solid #ccc;
        border-radius: 4px;
    }
    
    .btn-submit {
        padding: 10px 20px;
        background-color: #2196F3;
        color: white;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        font-weight: 600;
    }
    
    .btn-submit:hover {
        background-color: #1976D2;
    }
</style>
```

## Tooltip Support

The Stepper component supports tooltips to display additional information when users hover over steps.

### ShowTooltip Property

Use the `ShowTooltip` property to enable tooltips on the Stepper component:

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper ShowTooltip="true">
    <StepperSteps>
        <StepperStep Label="Cart" IconCss="sf-icon-cart"></StepperStep>
        <StepperStep Label="Delivery" IconCss="sf-icon-transport"></StepperStep>
        <StepperStep Label="Payment" IconCss="sf-icon-payment"></StepperStep>
        <StepperStep Label="Confirmation" IconCss="sf-icon-success"></StepperStep>
    </StepperSteps>
</SfStepper>
```

When `ShowTooltip="true"`:
- Tooltips appear on mouse hover over steps
- Tooltip content defaults to the step's Label or Text property
- Works for all step types (Default, Label, Indicator)

## Step Content Templates

Use content templates to create complex step layouts with custom HTML and Blazor components.

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper ActiveStep="@currentStep">
    <ChildContent>
        <StepperSteps>
            <StepperStep Label="Step 1"></StepperStep>
            <StepperStep Label="Step 2"></StepperStep>
            <StepperStep Label="Step 3"></StepperStep>
        </StepperSteps>
    </ChildContent>
    <Template>
        <div class="custom-step-template">
            <div class="step-number">@(context.Step + 1)</div>
            <div class="step-details">
                <strong>@context.Label</strong>
                <p>@GetStepDescription(context.Step)</p>
            </div>
        </div>
    </Template>
</SfStepper>

@code {
    private int currentStep = 0;
    
    private string GetStepDescription(int step)
    {
        return step switch
        {
            0 => "Enter your personal information",
            1 => "Provide your shipping address",
            2 => "Select payment method",
            _ => ""
        };
    }
}

<style>
    .custom-step-template {
        display: flex;
        align-items: center;
        gap: 10px;
        padding: 10px;
    }
    
    .step-number {
        width: 30px;
        height: 30px;
        background-color: #2196F3;
        color: white;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        font-weight: bold;
    }
    
    .step-details strong {
        display: block;
        margin-bottom: 5px;
    }
    
    .step-details p {
        margin: 0;
        font-size: 12px;
        color: #666;
    }
</style>
```

## Step Completion Tracking

Track step completion status and display progress information.

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper @ref="stepper" StepChanged="@OnStepChanged">
    <StepperSteps>
        <StepperStep Label="Step 1" @ref="step1"></StepperStep>
        <StepperStep Label="Step 2" @ref="step2"></StepperStep>
        <StepperStep Label="Step 3" @ref="step3"></StepperStep>
        <StepperStep Label="Step 4" @ref="step4"></StepperStep>
    </StepperSteps>
</SfStepper>

<div class="progress-info">
    <p>Completed Steps: @completedSteps/@totalSteps</p>
    <p>Progress: @((completedSteps * 100) / totalSteps)%</p>
    <div class="progress-bar">
        <div class="progress-fill" style="width: @((completedSteps * 100) / totalSteps)%"></div>
    </div>
</div>

<button @onclick="CompleteCurrentStep">Mark Step Complete</button>

@code {
    private SfStepper stepper;
    private StepperStep step1, step2, step3, step4;
    private int completedSteps = 0;
    private int totalSteps = 4;
    
    private void OnStepChanged(StepperChangedEventArgs args)
    {
        Console.WriteLine($"Current step: {args.ActiveStep}");
    }
    
    private async Task CompleteCurrentStep()
    {
        var currentStep = stepper.ActiveStep;
        completedSteps++;
        
        // Update step status
        var steps = new[] { step1, step2, step3, step4 };
        if (currentStep < steps.Length)
        {
            steps[currentStep].Status = StepperStatus.Completed;
        }
        
        // Move to next step if available
        if (currentStep < totalSteps - 1)
        {
            await stepper.NextStepAsync();
        }
    }
}

<style>
    .progress-info {
        margin: 20px 0;
        padding: 15px;
        background-color: #f5f5f5;
        border-radius: 4px;
    }
    
    .progress-bar {
        height: 8px;
        background-color: #e0e0e0;
        border-radius: 4px;
        overflow: hidden;
        margin-top: 10px;
    }
    
    .progress-fill {
        height: 100%;
        background-color: #4CAF50;
        transition: width 0.3s ease;
    }
</style>
```

## Conditional Step Display

Show or hide steps based on conditions or user selections.

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper>
    <StepperSteps>
        <StepperStep Label="Shipping Method"></StepperStep>
        @if (includeGiftWrap)
        {
            <StepperStep Label="Gift Wrapping" Optional="true"></StepperStep>
        }
        @if (shippingMethod == "Express")
        {
            <StepperStep Label="Express Delivery Confirmation"></StepperStep>
        }
        <StepperStep Label="Payment"></StepperStep>
    </StepperSteps>
</SfStepper>

<div>
    <label>
        <input type="checkbox" @bind="@includeGiftWrap" />
        Include Gift Wrapping
    </label>
</div>

<div>
    <label>
        <select @bind="@shippingMethod">
            <option>Standard</option>
            <option>Express</option>
        </select>
    </label>
</div>

@code {
    private bool includeGiftWrap = false;
    private string shippingMethod = "Standard";
}
```

## Error Handling and Recovery

Handle errors during step transitions and provide recovery options.

```csharp
@using Syncfusion.Blazor.Navigations

<SfStepper StepChanging="@OnStepChanging">
    <StepperSteps>
        <StepperStep Label="Submit Data"></StepperStep>
        <StepperStep Label="Processing"></StepperStep>
        <StepperStep Label="Complete"></StepperStep>
    </StepperSteps>
</SfStepper>

@if (!string.IsNullOrEmpty(errorMessage))
{
    <div class="error-message">
        <strong>Error:</strong> @errorMessage
        <button @onclick="RetryStep">Retry</button>
    </div>
}

@code {
    private string errorMessage = "";
    
    private async Task OnStepChanging(StepperChangeEventArgs args)
    {
        try
        {
            // Simulate step processing
            await ProcessStep(args.ActiveStep);
        }
        catch (Exception ex)
        {
            errorMessage = ex.Message;
            args.Cancel = true;
        }
    }
    
    private async Task ProcessStep(int stepIndex)
    {
        // Simulate API call or processing
        await Task.Delay(500);
        
        if (stepIndex == 1 && new Random().Next(2) == 0)
        {
            throw new Exception("Failed to process data. Please try again.");
        }
    }
    
    private void RetryStep()
    {
        errorMessage = "";
    }
}

<style>
    .error-message {
        margin: 20px 0;
        padding: 15px;
        background-color: #fee;
        border: 1px solid #fcc;
        border-radius: 4px;
        color: #c33;
    }
    
    .error-message button {
        margin-left: 10px;
        padding: 5px 10px;
        background-color: #c33;
        color: white;
        border: none;
        border-radius: 3px;
        cursor: pointer;
    }
</style>
```
