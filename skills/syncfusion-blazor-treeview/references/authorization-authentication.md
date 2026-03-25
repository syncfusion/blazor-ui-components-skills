# Authorization and Authentication in TreeView

## Table of Contents

1. [Authentication Setup](#authentication-setup)
2. [Basic AuthorizeView Integration](#basic-authorizeview-integration)
3. [Role-Based Authorization](#role-based-authorization)
4. [Permission Filtering](#permission-filtering)
5. [Node-Level Security](#node-level-security)
6. [Claims-Based Authorization](#claims-based-authorization)
7. [Securing Events and Methods](#securing-events-and-methods)
8. [Best Practices for Security](#best-practices-for-security)
9. [Troubleshooting](#troubleshooting)
10. [Next Steps](#next-steps)

---

## Authentication Setup

### Basic AuthorizeView Integration

Protect the entire TreeView component from unauthorized access:

```csharp
@using Syncfusion.Blazor.Navigations
@using Microsoft.AspNetCore.Authorization

<AuthorizeView>
    <Authorized>
        <div>
            <p>Welcome, @context.User.Identity?.Name!</p>
            
            <SfTreeView TValue="MailItem">
                <TreeViewFieldsSettings TValue="MailItem"
                    Id="Id"
                    Text="FolderName"
                    ParentID="ParentId"
                    DataSource="@MyFolder">
                </TreeViewFieldsSettings>
            </SfTreeView>
            
            <form method="post" action="Identity/Account/LogOut">
                <button type="submit">Log out</button>
            </form>
        </div>
    </Authorized>
    
    <NotAuthorized>
        <p>You are not authorized to view this content.</p>
    </NotAuthorized>
</AuthorizeView>

@code {
    public class MailItem
    {
        public string? Id { get; set; }
        public string? ParentId { get; set; }
        public string? FolderName { get; set; }
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox" },
        new MailItem { Id = "2", FolderName = "Sent" }
    };
}
```

## Role-Based Authorization

### Restrict by User Role

```csharp
@using Syncfusion.Blazor.Navigations
@using Microsoft.AspNetCore.Authorization

<AuthorizeView Roles="Admin,Manager">
    <Authorized>
        <SfTreeView TValue="EmployeeItem">
            <TreeViewFieldsSettings TValue="EmployeeItem" DataSource="@Employees" />
        </SfTreeView>
    </Authorized>
    
    <NotAuthorized>
        <p>You don't have permission to view employee data.</p>
    </NotAuthorized>
</AuthorizeView>

@code {
    public class EmployeeItem
    {
        public string? Id { get; set; }
        public string? Name { get; set; }
    }

    List<EmployeeItem> Employees = new()
    {
        new EmployeeItem { Id = "1", Name = "Alice" },
        new EmployeeItem { Id = "2", Name = "Bob" }
    };
}
```

### Multiple Role Check

```csharp
<AuthorizeView Policy="CanViewReports">
    <Authorized>
        <!-- Show TreeView -->
    </Authorized>
    <NotAuthorized>
        <p>You don't have permission.</p>
    </NotAuthorized>
</AuthorizeView>
```

## Permission-Based Node Filtering

### Filter Nodes by User Permissions

```csharp
@using Syncfusion.Blazor.Navigations
@inject AuthenticationStateProvider AuthenticationStateProvider

<SfTreeView TValue="FolderItem">
    <TreeViewFieldsSettings TValue="FolderItem"
        Id="Id"
        Text="FolderName"
        ParentID="ParentId"
        DataSource="@FilteredFolders">
    </TreeViewFieldsSettings>
</SfTreeView>

@code {
    [CascadingParameter]
    private Task<AuthenticationState>? authenticationStateTask { get; set; }
    
    string CurrentUserId = "";
    List<FolderItem> FilteredFolders => GetUserFolders();

    public class FolderItem
    {
        public string? Id { get; set; }
        public string? ParentId { get; set; }
        public string? FolderName { get; set; }
        public string? OwnerId { get; set; }
        public bool IsPublic { get; set; }
    }

    List<FolderItem> AllFolders = new()
    {
        new FolderItem { Id = "1", FolderName = "Public", IsPublic = true },
        new FolderItem { Id = "2", FolderName = "Private", OwnerId = "user1" },
        new FolderItem { Id = "3", FolderName = "Shared", OwnerId = "user2" }
    };

    protected override async Task OnInitializedAsync()
    {
        if (authenticationStateTask != null)
        {
            var state = await authenticationStateTask;
            CurrentUserId = state.User.FindFirst("sub")?.Value ?? "";
        }
    }

    List<FolderItem> GetUserFolders()
    {
        return AllFolders.Where(f => 
            f.IsPublic || 
            f.OwnerId == CurrentUserId
        ).ToList();
    }
}
```

## Node-Level Authorization

### Control Node Visibility

```csharp
@using Syncfusion.Blazor.Navigations
@using System.Security.Claims
@inject AuthenticationStateProvider AuthenticationStateProvider

<SfTreeView TValue="DocumentItem">
    <TreeViewFieldsSettings TValue="DocumentItem"
        Id="Id"
        Text="FileName"
        ParentID="ParentId"
        DataSource="@VisibleDocuments">
    </TreeViewFieldsSettings>
    <TreeViewEvents TValue="DocumentItem"
        NodeSelected="OnNodeSelected"
        NodeEditing="OnNodeEditing">
    </TreeViewEvents>
</SfTreeView>

@code {
    [CascadingParameter]
    private Task<AuthenticationState>? authenticationStateTask { get; set; }
    
    ClaimsPrincipal? currentUser;

    public class DocumentItem
    {
        public string? Id { get; set; }
        public string? ParentId { get; set; }
        public string? FileName { get; set; }
        public string? AccessLevel { get; set; }  // "public", "restricted", "private"
    }

    List<DocumentItem> AllDocuments = new()
    {
        new DocumentItem { Id = "1", FileName = "Public Report", AccessLevel = "public" },
        new DocumentItem { Id = "2", FileName = "Confidential", AccessLevel = "restricted" },
        new DocumentItem { Id = "3", FileName = "Personal", AccessLevel = "private" }
    };

    List<DocumentItem> VisibleDocuments => GetVisibleDocuments();

    protected override async Task OnInitializedAsync()
    {
        if (authenticationStateTask != null)
        {
            var state = await authenticationStateTask;
            currentUser = state.User;
        }
    }

    List<DocumentItem> GetVisibleDocuments()
    {
        return AllDocuments.Where(d =>
            CanUserAccessDocument(d)
        ).ToList();
    }

    bool CanUserAccessDocument(DocumentItem doc)
    {
        return doc.AccessLevel switch
        {
            "public" => true,
            "restricted" => currentUser?.IsInRole("Manager") ?? false,
            "private" => currentUser?.Identity?.Name == "admin",
            _ => false
        };
    }

    void OnNodeSelected(NodeSelectEventArgs args)
    {
        var doc = (DocumentItem)args.NodeData;
        if (!CanUserAccessDocument(doc))
        {
            args.Cancel = true;
            Console.WriteLine("Access denied");
        }
    }

    void OnNodeEditing(NodeEditEventArgs args)
    {
        var doc = (DocumentItem)args.NodeData;
        
        // Only admin can edit
        if (currentUser?.Identity?.Name != "admin")
        {
            args.Cancel = true;
        }
    }
}
```

## Action-Level Authorization

### Control Operations by Permission

```csharp
@using Syncfusion.Blazor.Navigations
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService

<button @onclick="DeleteNode" 
    disabled="@(!CanDeleteNodes)">
    Delete
</button>

<SfTreeView TValue="FileItem">
    <TreeViewFieldsSettings TValue="FileItem" DataSource="@MyFiles" />
    <TreeViewEvents TValue="FileItem"
        OnNodeDragStart="OnBeforeDrag"
        NodeEditing="OnNodeEditing">
    </TreeViewEvents>
</SfTreeView>

@code {
    [CascadingParameter]
    private Task<AuthenticationState>? authenticationStateTask { get; set; }
    
    ClaimsPrincipal? currentUser;
    bool CanDeleteNodes => currentUser?.IsInRole("Admin") ?? false;
    bool CanEditNodes => currentUser?.IsInRole("Editor") ?? false;
    bool CanDragNodes => currentUser?.IsInRole("Organizer") ?? false;

    public class FileItem
    {
        public string? Id { get; set; }
        public string? FileName { get; set; }
    }

    List<FileItem> MyFiles = new()
    {
        new FileItem { Id = "1", FileName = "Document.pdf" }
    };

    protected override async Task OnInitializedAsync()
    {
        if (authenticationStateTask != null)
        {
            var state = await authenticationStateTask;
            currentUser = state.User;
        }
    }

    async Task DeleteNode()
    {
        if (!CanDeleteNodes)
        {
            Console.WriteLine("Delete permission denied");
            return;
        }
        
        // Proceed with deletion
    }

    void OnBeforeDrag(NodeDragEventArgs args)
    {
        if (!CanDragNodes)
        {
            args.Cancel = true;
            Console.WriteLine("Drag permission denied");
        }
    }

    void OnNodeEditing(NodeEditEventArgs args)
    {
        if (!CanEditNodes)
        {
            args.Cancel = true;
            Console.WriteLine("Edit permission denied");
        }
    }
}
```

## Setup Authentication in Program.cs

### Configure Authentication Service

```csharp
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Authentication.OpenIdConnect;

var builder = WebAssemblyHostBuilder.CreateDefault(args);

// Add authentication
builder.Services.AddOidcAuthentication(options =>
{
    builder.Configuration.Bind("Auth0", options.ProviderOptions);
});

// Add authorization
builder.Services.AddAuthorizationCore();

var app = builder.Build();
await app.RunAsync();
```

### Configure Authorization Policies

```csharp
// In Blazor Server Program.cs
builder.Services.AddAuthorization(options =>
{
    options.AddPolicy("CanViewReports", policy =>
        policy.RequireRole("Admin", "Manager"));
    
    options.AddPolicy("CanEditNodes", policy =>
        policy.RequireRole("Editor", "Admin"));
    
    options.AddPolicy("CanDeleteNodes", policy =>
        policy.RequireRole("Admin"));
});
```

## Logout and Session Management

### User Logout

```csharp
@using Syncfusion.Blazor.Navigations

<AuthorizeView>
    <Authorized>
        <p>Logged in as: @context.User.Identity?.Name</p>
        
        <SfTreeView TValue="MailItem">
            <TreeViewFieldsSettings TValue="MailItem" DataSource="@MyFolder" />
        </SfTreeView>
        
        <form method="post" action="Identity/Account/LogOut">
            <button type="submit" class="btn btn-danger">Log out</button>
        </form>
    </Authorized>
    
    <NotAuthorized>
        <p>Not logged in. <a href="Identity/Account/Login">Log in</a></p>
    </NotAuthorized>
</AuthorizeView>

@code {
    public class MailItem
    {
        public string? Id { get; set; }
        public string? FolderName { get; set; }
    }

    List<MailItem> MyFolder = new()
    {
        new MailItem { Id = "1", FolderName = "Inbox" }
    };
}
```

## Common Patterns

### Pattern 1: Conditional Features

```csharp
@if (currentUser?.IsInRole("Admin") ?? false)
{
    <div>
        <button @onclick="BulkDelete">Bulk Delete</button>
        <button @onclick="ExportAll">Export All</button>
    </div>
}

<SfTreeView TValue="Item" 
    AllowEdit="@(currentUser?.IsInRole("Editor") ?? false)"
    AllowDragAndDrop="@(currentUser?.IsInRole("Organizer") ?? false)">
    <TreeViewFieldsSettings TValue="Item" DataSource="@VisibleItems" />
</SfTreeView>
```

### Pattern 2: Audit Logging

```csharp
async Task OnNodeModified(NodeEditEventArgs args)
{
    // Log the action
    await AuditLog(new()
    {
        Action = "NodeEdited",
        NodeId = args.NodeData.Id,
        UserId = CurrentUserId,
        OldValue = args.OldText,
        NewValue = args.NewText,
        Timestamp = DateTime.UtcNow
    });
}
```

## Best Practices

1. **Always check authorization** before sensitive operations
2. **Filter data server-side**, not just client-side
3. **Use role-based access** for broad permissions
4. **Use claims-based access** for fine-grained control
5. **Log security events** for audit trails
6. **Test with multiple roles** to ensure proper filtering
7. **Don't rely on UI disabling** alone for security

## Troubleshooting

**Issue: AuthorizeView not working**
- Ensure CascadingAuthenticationState is set up
- Check authentication is configured in Program.cs
- Verify AuthenticationState is provided

**Issue: Nodes still visible after filtering**
- Ensure filtering happens in GetVisibleDocuments
- Check that CanUserAccessDocument logic is correct
- Verify CurrentUserId is set properly

**Issue: Permission checks not firing**
- Ensure event handlers are attached
- Verify Cancel property is set to true
- Check authorization logic

## Next Steps

- Use [node-selection.md](node-selection.md) for selective access
- Implement [events-handling.md](events-handling.md) for operation logging
- Check [advanced-features.md](advanced-features.md) for role-based features
