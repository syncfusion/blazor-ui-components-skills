# Use Cases and Real-World Examples

## Table of Contents
- [Form Submission Feedback](#form-submission-feedback)
- [E-commerce Cart Notifications](#e-commerce-cart-notifications)
- [File Upload Progress](#file-upload-progress)
- [Dashboard Data Loading](#dashboard-data-loading)
- [User Authentication](#user-authentication)
- [API Error Handling](#api-error-handling)
- [Real-time Updates](#real-time-updates)
- [Multi-Step Forms](#multi-step-forms)
- [Notification Center](#notification-center)
- [Complete Application Examples](#complete-application-examples)

---

## Form Submission Feedback

Combine Message for inline validation errors and Toast for submission success/failure.

```razor
@page "/form-example"
@using System.ComponentModel.DataAnnotations

<div class="form-container">
    <h2>User Registration</h2>
    
    @if (!string.IsNullOrEmpty(errorMessage))
    {
        <SfMessage Severity="MessageSeverity.Error" 
                   ShowIcon="true" 
                   ShowCloseIcon="true"
                   Closed="@(() => errorMessage = null)">
            <strong>Error:</strong> @errorMessage
        </SfMessage>
    }
    
    <EditForm Model="@model" OnValidSubmit="HandleSubmit" OnInvalidSubmit="HandleInvalidSubmit">
        <DataAnnotationsValidator />
        
        <div class="form-group">
            <label>Full Name:</label>
            <InputText @bind-Value="model.FullName" class="form-control" />
            <ValidationMessage For="@(() => model.FullName)" />
        </div>
        
        <div class="form-group">
            <label>Email:</label>
            <InputText @bind-Value="model.Email" class="form-control" />
            <ValidationMessage For="@(() => model.Email)" />
        </div>
        
        <div class="form-group">
            <label>Password:</label>
            <InputText @bind-Value="model.Password" type="password" class="form-control" />
            <ValidationMessage For="@(() => model.Password)" />
        </div>
        
        <div class="form-group">
            <label>Confirm Password:</label>
            <InputText @bind-Value="model.ConfirmPassword" type="password" class="form-control" />
            <ValidationMessage For="@(() => model.ConfirmPassword)" />
        </div>
        
        <SfButton Type="ButtonType.Submit" IsPrimary="true" Disabled="@isSubmitting">
            @(isSubmitting ? "Submitting..." : "Register")
        </SfButton>
    </EditForm>
</div>

<SfToast @ref="ToastObj" 
         ShowCloseButton="true" >
    <ToastPosition X="Right" Y="Top"></ToastPosition>
</SfToast>

@code {
    private RegistrationModel model = new();
    private SfToast ToastObj;
    private string errorMessage;
    private bool isSubmitting = false;

    private async Task HandleSubmit()
    {
        if (model.Password != model.ConfirmPassword)
        {
            errorMessage = "Passwords do not match";
            return;
        }

        isSubmitting = true;
        errorMessage = null;

        try
        {
            await Task.Delay(1500); // Simulate API call
            // await UserService.RegisterAsync(model);
            
            ToastObj.Title = "Success!";
            ToastObj.Content = "Your account has been created successfully.";
            ToastObj.Icon = "e-success";
            ToastObj.CssClass = "e-toast-success";
            await ToastObj.ShowAsync();
            
            // Reset form
            model = new();
        }
        catch (Exception ex)
        {
            errorMessage = $"Registration failed: {ex.Message}";
        }
        finally
        {
            isSubmitting = false;
        }
    }

    private void HandleInvalidSubmit()
    {
        errorMessage = "Please correct the errors below.";
    }

    public class RegistrationModel
    {
        [Required(ErrorMessage = "Full name is required")]
        public string FullName { get; set; }

        [Required(ErrorMessage = "Email is required")]
        [EmailAddress(ErrorMessage = "Invalid email format")]
        public string Email { get; set; }

        [Required(ErrorMessage = "Password is required")]
        [MinLength(8, ErrorMessage = "Password must be at least 8 characters")]
        public string Password { get; set; }

        [Required(ErrorMessage = "Please confirm your password")]
        public string ConfirmPassword { get; set; }
    }
}

<style>
    .form-container {
        max-width: 500px;
        margin: 40px auto;
        padding: 30px;
        border: 1px solid #e0e0e0;
        border-radius: 8px;
        background: white;
    }

    .form-group {
        margin-bottom: 20px;
    }

    .form-group label {
        display: block;
        margin-bottom: 5px;
        font-weight: 500;
    }

    .form-control {
        width: 100%;
        padding: 8px 12px;
        border: 1px solid #ccc;
        border-radius: 4px;
    }

    .validation-message {
        color: #f44336;
        font-size: 13px;
        margin-top: 4px;
    }
</style>
```

---

## E-commerce Cart Notifications

Show toast notifications for cart operations with action buttons.

```razor
@page "/cart-demo"

<div class="product-grid">
    @foreach (var product in products)
    {
        <div class="product-card">
            <img src="@product.ImageUrl" alt="@product.Name" />
            <h3>@product.Name</h3>
            <p class="price">$@product.Price</p>
            <SfButton @onclick="@(() => AddToCart(product))">Add to Cart</SfButton>
        </div>
    }
</div>

<SfToast @ref="ToastObj" 
         Timeout="4000">
    <ToastButtons>
        <ToastButton Content="View Cart" OnClick="@ViewCart" CssClass="e-primary" />
        <ToastButton Content="Undo" OnClick="@UndoAddToCart" />
    </ToastButtons>
    <ToastPosition X="Right" Y="Top"></ToastPosition>
</SfToast>

@code {
    private SfToast ToastObj;
    private List<Product> products = new();
    private Product lastAddedProduct;

    protected override void OnInitialized()
    {
        products = new List<Product>
        {
            new Product { Id = 1, Name = "Wireless Mouse", Price = 29.99m, ImageUrl = "/images/mouse.jpg" },
            new Product { Id = 2, Name = "Keyboard", Price = 79.99m, ImageUrl = "/images/keyboard.jpg" },
            new Product { Id = 3, Name = "Monitor", Price = 299.99m, ImageUrl = "/images/monitor.jpg" }
        };
    }

    private async Task AddToCart(Product product)
    {
        lastAddedProduct = product;
        // Add to cart logic
        await CartService.AddAsync(product);

        ToastObj.Title = "Added to Cart";
        ToastObj.Content = $"{product.Name} has been added to your cart.";
        ToastObj.Icon = "e-success";
        ToastObj.CssClass = "e-toast-success";
        await ToastObj.ShowAsync();
    }

    private async Task ViewCart()
    {
        await ToastObj.HideAsync();
        NavigationManager.NavigateTo("/cart");
    }

    private async Task UndoAddToCart()
    {
        if (lastAddedProduct != null)
        {
            await CartService.RemoveAsync(lastAddedProduct.Id);
            
            ToastObj.Title = "Removed";
            ToastObj.Content = $"{lastAddedProduct.Name} has been removed from cart.";
            ToastObj.Icon = "e-info";
            ToastObj.CssClass = "e-toast-info";
            await ToastObj.ShowAsync();
        }
    }

    private class Product
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public decimal Price { get; set; }
        public string ImageUrl { get; set; }
    }
}
```

---

## File Upload Progress

Combine Skeleton for loading state, Message for status, and Toast for completion.

```razor
@page "/file-upload"

<div class="upload-container">
    <h2>File Upload</h2>
    
    @if (uploadStatus == UploadStatus.Error)
    {
        <SfMessage Severity="MessageSeverity.Error" 
                   ShowIcon="true" 
                   ShowCloseIcon="true"
                   Closed="@(() => uploadStatus = UploadStatus.None)">
            @errorMessage
        </SfMessage>
    }
    
    <InputFile OnChange="HandleFileUpload" multiple accept="image/*" />
    
    @if (uploadStatus == UploadStatus.Uploading)
    {
        <div class="upload-progress">
            <SfMessage Severity="MessageSeverity.Info" ShowIcon="true">
                Uploading @fileName... @uploadProgress%
            </SfMessage>
            
            <div class="file-preview-skeleton">
                <SfSkeleton Shape="SkeletonType.Rectangle" Width="100%" Height="200px" />
                <SfSkeleton Shape="SkeletonType.Text" Width="70%" />
            </div>
        </div>
    }
    
    @if (uploadStatus == UploadStatus.Completed && uploadedFiles.Any())
    {
        <div class="uploaded-files">
            <h3>Uploaded Files</h3>
            @foreach (var file in uploadedFiles)
            {
                <div class="file-item">
                    <img src="@file.Url" alt="@file.Name" />
                    <p>@file.Name</p>
                </div>
            }
        </div>
    }
</div>

<SfToast @ref="ToastObj">
    <ToastPosition X="Right" Y="Top"></ToastPosition>
 </SfToast>

@code {
    private SfToast ToastObj;
    private UploadStatus uploadStatus = UploadStatus.None;
    private string fileName;
    private string errorMessage;
    private int uploadProgress;
    private List<UploadedFile> uploadedFiles = new();

    private async Task HandleFileUpload(InputFileChangeEventArgs e)
    {
        uploadStatus = UploadStatus.Uploading;
        errorMessage = null;

        foreach (var file in e.GetMultipleFiles(10))
        {
            fileName = file.Name;
            uploadProgress = 0;

            try
            {
                // Simulate upload with progress
                for (int i = 0; i <= 100; i += 10)
                {
                    uploadProgress = i;
                    StateHasChanged();
                    await Task.Delay(200);
                }

                // Upload file
                using var stream = file.OpenReadStream(maxAllowedSize: 10 * 1024 * 1024);
                var url = await FileService.UploadAsync(stream, file.Name);

                uploadedFiles.Add(new UploadedFile
                {
                    Name = file.Name,
                    Url = url
                });

                uploadStatus = UploadStatus.Completed;

                ToastObj.Title = "Upload Complete";
                ToastObj.Content = $"{file.Name} uploaded successfully!";
                ToastObj.Icon = "e-success";
                ToastObj.CssClass = "e-toast-success";
                await ToastObj.ShowAsync();
            }
            catch (Exception ex)
            {
                uploadStatus = UploadStatus.Error;
                errorMessage = $"Failed to upload {file.Name}: {ex.Message}";
            }
        }
    }

    private enum UploadStatus
    {
        None,
        Uploading,
        Completed,
        Error
    }

    private class UploadedFile
    {
        public string Name { get; set; }
        public string Url { get; set; }
    }
}

<style>
    .upload-container {
        max-width: 800px;
        margin: 40px auto;
        padding: 30px;
    }

    .file-preview-skeleton {
        margin-top: 20px;
        display: flex;
        flex-direction: column;
        gap: 10px;
    }

    .uploaded-files {
        margin-top: 30px;
    }

    .file-item {
        display: inline-block;
        margin: 10px;
        text-align: center;
    }

    .file-item img {
        width: 150px;
        height: 150px;
        object-fit: cover;
        border-radius: 8px;
    }
</style>
```

---

## Dashboard Data Loading

Use Skeleton while loading dashboard data, then show content with Toast for updates.

```razor
@page "/dashboard"

<div class="dashboard">
    <h1>Analytics Dashboard</h1>
    
    @if (isLoading)
    {
        <!-- Dashboard Skeleton -->
        <div class="dashboard-grid">
            @for (int i = 0; i < 4; i++)
            {
                <div class="stat-card-skeleton">
                    <SfSkeleton Shape="SkeletonType.Text" Width="60%" Height="20px" />
                    <SfSkeleton Shape="SkeletonType.Text" Width="40%" Height="32px" />
                </div>
            }
            
            <div class="chart-skeleton">
                <SfSkeleton Shape="SkeletonType.Rectangle" Width="100%" Height="300px" />
            </div>
        </div>
    }
    else
    {
        <!-- Actual Dashboard Content -->
        <div class="dashboard-grid">
            @foreach (var stat in stats)
            {
                <div class="stat-card">
                    <h3>@stat.Label</h3>
                    <p class="value">@stat.Value</p>
                    <span class="change @(stat.ChangePositive ? "positive" : "negative")">
                        @stat.Change%
                    </span>
                </div>
            }
            
            <div class="chart-container">
                <!-- Chart component here -->
                <h3>Revenue Trend</h3>
            </div>
        </div>
    }
    
    @if (showUpdateNotification)
    {
        <SfMessage Severity="MessageSeverity.Info" 
                   ShowIcon="true" 
                   ShowCloseIcon="true"
                   Closed="@(() => showUpdateNotification = false)">
            Dashboard data updated @lastUpdateTime.ToString("HH:mm:ss")
        </SfMessage>
    }
</div>

<SfToast @ref="ToastObj">
    <ToastPosition X="Right" Y="Bottom"></ToastPosition>
 </SfToast>

@code {
    private SfToast ToastObj;
    private bool isLoading = true;
    private bool showUpdateNotification = false;
    private DateTime lastUpdateTime;
    private List<DashboardStat> stats = new();

    protected override async Task OnInitializedAsync()
    {
        await LoadDashboardData();
        
        // Auto-refresh every 30 seconds
        _ = AutoRefreshData();
    }

    private async Task LoadDashboardData()
    {
        isLoading = true;
        await Task.Delay(2000); // Simulate API call

        stats = new List<DashboardStat>
        {
            new DashboardStat { Label = "Total Revenue", Value = "$45,231", Change = 12.5, ChangePositive = true },
            new DashboardStat { Label = "New Users", Value = "1,893", Change = 8.3, ChangePositive = true },
            new DashboardStat { Label = "Orders", Value = "432", Change = -3.2, ChangePositive = false },
            new DashboardStat { Label = "Conversion Rate", Value = "3.24%", Change = 1.8, ChangePositive = true }
        };

        lastUpdateTime = DateTime.Now;
        isLoading = false;
    }

    private async Task AutoRefreshData()
    {
        while (true)
        {
            await Task.Delay(30000); // 30 seconds
            await LoadDashboardData();
            
            showUpdateNotification = true;
            StateHasChanged();
            
            ToastObj.Title = "Data Refreshed";
            ToastObj.Content = "Dashboard data has been updated.";
            ToastObj.Icon = "e-info";
            ToastObj.CssClass = "e-toast-info";
            ToastObj.Timeout = 3000;
            await ToastObj.ShowAsync();
        }
    }

    private class DashboardStat
    {
        public string Label { get; set; }
        public string Value { get; set; }
        public double Change { get; set; }
        public bool ChangePositive { get; set; }
    }
}

<style>
    .dashboard {
        padding: 30px;
    }

    .dashboard-grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
        gap: 20px;
        margin-top: 20px;
    }

    .stat-card, .stat-card-skeleton {
        padding: 20px;
        border: 1px solid #e0e0e0;
        border-radius: 8px;
        background: white;
    }

    .stat-card-skeleton {
        display: flex;
        flex-direction: column;
        gap: 12px;
    }

    .chart-skeleton, .chart-container {
        grid-column: 1 / -1;
        padding: 20px;
        border: 1px solid #e0e0e0;
        border-radius: 8px;
        background: white;
    }

    .value {
        font-size: 32px;
        font-weight: bold;
        margin: 10px 0;
    }

    .change {
        font-size: 14px;
        font-weight: 600;
    }

    .change.positive {
        color: #4CAF50;
    }

    .change.negative {
        color: #F44336;
    }
</style>
```

---

## User Authentication

Show loading skeleton during login, then success/error notifications.

```razor
@page "/login"

<div class="login-container">
    <h2>Login</h2>
    
    @if (!string.IsNullOrEmpty(errorMessage))
    {
        <SfMessage Severity="MessageSeverity.Error" 
                   ShowIcon="true" 
                   ShowCloseIcon="true"
                   Closed="@(() => errorMessage = null)">
            @errorMessage
        </SfMessage>
    }
    
    @if (isLoading)
    {
        <div class="loading-skeleton">
            <SfSkeleton Shape="SkeletonType.Text" Width="100%" Height="40px" />
            <SfSkeleton Shape="SkeletonType.Text" Width="100%" Height="40px" />
            <SfSkeleton Shape="SkeletonType.Rectangle" Width="100%" Height="40px" />
        </div>
    }
    else
    {
        <EditForm Model="@model" OnValidSubmit="HandleLogin">
            <DataAnnotationsValidator />
            
            <div class="form-group">
                <label>Email:</label>
                <InputText @bind-Value="model.Email" class="form-control" />
                <ValidationMessage For="@(() => model.Email)" />
            </div>
            
            <div class="form-group">
                <label>Password:</label>
                <InputText @bind-Value="model.Password" type="password" class="form-control" />
                <ValidationMessage For="@(() => model.Password)" />
            </div>
            
            <SfButton Type="ButtonType.Submit" IsPrimary="true" Disabled="@isAuthenticating">
                @(isAuthenticating ? "Signing in..." : "Sign In")
            </SfButton>
        </EditForm>
    }
</div>

<SfToast @ref="ToastObj">
    <ToastPosition X="Center" Y="Top"></ToastPosition>
 </SfToast>

@code {
    private SfToast ToastObj;
    private LoginModel model = new();
    private bool isLoading = false;
    private bool isAuthenticating = false;
    private string errorMessage;

    private async Task HandleLogin()
    {
        isAuthenticating = true;
        errorMessage = null;

        try
        {
            await Task.Delay(1500); // Simulate auth API call
            // await AuthService.LoginAsync(model.Email, model.Password);
            
            ToastObj.Title = "Welcome Back!";
            ToastObj.Content = "You have been successfully logged in.";
            ToastObj.Icon = "e-success";
            ToastObj.CssClass = "e-toast-success";
            await ToastObj.ShowAsync();
            
            await Task.Delay(1000);
            NavigationManager.NavigateTo("/dashboard");
        }
        catch (Exception ex)
        {
            errorMessage = "Invalid email or password. Please try again.";
        }
        finally
        {
            isAuthenticating = false;
        }
    }

    public class LoginModel
    {
        [Required]
        [EmailAddress]
        public string Email { get; set; }

        [Required]
        public string Password { get; set; }
    }
}
```

---

## API Error Handling

Comprehensive error handling with retry options.

```razor
@page "/api-demo"

<div class="api-demo">
    <h2>Data Management</h2>
    
    @if (!string.IsNullOrEmpty(apiError))
    {
        <SfMessage Severity="MessageSeverity.Error" 
                   ShowIcon="true" 
                   ShowCloseIcon="true"
                   Closed="@(() => apiError = null)">
            <strong>API Error:</strong> @apiError
            <SfButton @onclick="RetryLastOperation" CssClass="retry-btn">Retry</SfButton>
        </SfMessage>
    }
    
    <SfButton @onclick="LoadData">Load Data</SfButton>
    <SfButton @onclick="SaveData">Save Data</SfButton>
</div>

<SfToast @ref="ToastObj">
    <ToastButtons>
        <ToastButton Content="Retry" OnClick="@RetryFromToast" />
        <ToastButton Content="Dismiss" OnClick="@DismissError" />
    </ToastButtons>
    <ToastPosition X="Right" Y="Top"></ToastPosition>
</SfToast>

@code {
    private SfToast ToastObj;
    private string apiError;
    private Action lastFailedOperation;

    private async Task LoadData()
    {
        try
        {
            await ApiService.LoadDataAsync();
            
            ToastObj.Title = "Success";
            ToastObj.Content = "Data loaded successfully.";
            ToastObj.CssClass = "e-toast-success";
            await ToastObj.ShowAsync();
        }
        catch (HttpRequestException ex)
        {
            lastFailedOperation = LoadData;
            apiError = $"Network error: Unable to reach the server.";
            
            ToastObj.Title = "Connection Error";
            ToastObj.Content = "Unable to connect to the server. Please check your internet connection.";
            ToastObj.CssClass = "e-toast-danger";
            ToastObj.Timeout = 0; // Don't auto-dismiss
            await ToastObj.ShowAsync();
        }
        catch (Exception ex)
        {
            lastFailedOperation = LoadData;
            apiError = ex.Message;
        }
    }

    private async Task SaveData()
    {
        // Similar error handling
    }

    private async Task RetryLastOperation()
    {
        apiError = null;
        if (lastFailedOperation != null)
        {
            await lastFailedOperation.Invoke();
        }
    }

    private async Task RetryFromToast()
    {
        await ToastObj.HideAsync();
        await RetryLastOperation();
    }

    private async Task DismissError()
    {
        await ToastObj.HideAsync();
    }
}
```

---

## Real-time Updates

Show toast notifications for real-time events (SignalR, WebSockets).

```razor
@page "/realtime"
@implements IAsyncDisposable

<div class="realtime-container">
    <h2>Real-time Notifications</h2>
    
    <div class="notification-history">
        @foreach (var notification in notifications)
        {
            <div class="notification-item">
                <span class="timestamp">@notification.Time.ToString("HH:mm:ss")</span>
                <span class="message">@notification.Message</span>
            </div>
        }
    </div>
</div>

<SfToast @ref="ToastObj" 
         NewestOnTop="true">
    <ToastPosition X="Right" Y="Bottom"></ToastPosition>
</SfToast>

@code {
    private SfToast ToastObj;
    private List<Notification> notifications = new();
    private HubConnection hubConnection;

    protected override async Task OnInitializedAsync()
    {
        hubConnection = new HubConnectionBuilder()
            .WithUrl(NavigationManager.ToAbsoluteUri("/notificationHub"))
            .Build();

        hubConnection.On<string, string>("ReceiveNotification", async (title, message) =>
        {
            notifications.Add(new Notification
            {
                Time = DateTime.Now,
                Message = message
            });

            ToastObj.Title = title;
            ToastObj.Content = message;
            ToastObj.Icon = "e-info";
            ToastObj.CssClass = "e-toast-info";
            await ToastObj.ShowAsync();
            
            StateHasChanged();
        });

        await hubConnection.StartAsync();
    }

    public async ValueTask DisposeAsync()
    {
        if (hubConnection is not null)
        {
            await hubConnection.DisposeAsync();
        }
    }

    private class Notification
    {
        public DateTime Time { get; set; }
        public string Message { get; set; }
    }
}
```

---

## Best Practices Summary

1. **Use appropriate component** - Toast for temporary, Message for persistent, Skeleton for loading
2. **Combine components** - Use together for comprehensive UX
3. **Handle all states** - Loading, success, error, empty
4. **Provide actions** - Give users options to respond (retry, undo, view)
5. **Clear messaging** - Be specific about what happened and why
6. **Accessibility** - Always include labels and ARIA attributes
7. **Performance** - Remove components from DOM when not needed
8. **Responsive** - Test on all screen sizes
9. **Consistent positioning** - Use same toast position throughout app
10. **Error recovery** - Always provide way to retry or dismiss errors

---

## Next Steps

- Review [Toast Features](toast-features.md) for advanced customization
- Explore [Message Implementation](message-implementation.md) for inline alerts
- Check [Skeleton Patterns](skeleton-implementation.md) for loading states
- Study [Styling Guide](styling-and-themes.md) for theme customization
