# Remote Data Adaptors

## Table of Contents
- [Overview](#overview)
- [Common Adaptors](#common-adaptors) - [UrlAdaptor](#urladaptor) - [OData Adaptors](#odata-adaptors) - [Custom Adaptors](#custom-adaptors) - [Server Responsibilities](#server-responsibilities) - [Controller Result Shape](#controller-result-shape) - [Hierarchy and Dependency Concerns](#hierarchy-and-dependency-concerns) - [Validation Guidance](#validation-guidance) - [Common Issues](#common-issues) - [Summary](#summary)

---

## Overview
`SfDataManager` connects the Gantt Chart to remote services. The adaptor determines how requests are shaped and how responses are interpreted for loading, searching, filtering, sorting, and CRUD operations.

## Common Adaptors
- `UrlAdaptor`
- `ODataAdaptor`
- `ODataV4Adaptor`
- Custom adaptor implementations

## UrlAdaptor
`UrlAdaptor` is the most common choice for custom ASP.NET Core APIs.

```razor
<SfGantt TValue="TaskData" Height="500px">
    <SfDataManager Url="/api/Gantt"
                   InsertUrl="/api/Gantt/Insert"
                   UpdateUrl="/api/Gantt/Update"
                   RemoveUrl="/api/Gantt/Delete"
                   BatchUrl="/api/Gantt/BatchUpdate"
                   Adaptor="Adaptors.UrlAdaptor">
    </SfDataManager>
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" Duration="Duration" ParentID="ParentID"></GanttTaskFields>
</SfGantt>
```

### Best fit
- Custom REST endpoints
- Server-side business validation
- Flexible request and response shaping

## OData Adaptors
OData adaptors fit best when the backend already exposes OData query semantics.

### Best fit
- Existing OData services
- Teams standardizing on OData query options

## Custom Adaptors
Use a custom adaptor when the backend contract cannot align with the default adaptor expectations.

Typical cases:
- Non-standard pagination
- Wrapped response envelopes
- Tenant-specific request shaping
- Custom authentication or request metadata

## Server Responsibilities
Remote APIs often need to handle:
- Search
- Filter
- Sort
- Insert, update, delete
- Batch save

## Controller Result Shape
A common read result looks like this:

```csharp
[HttpPost]
public IActionResult Post([FromBody] DataManagerRequest dm)
{
    var data = new List<TaskData>();
    return Ok(new { result = data, count = data.Count });
}
```

## Hierarchy and Dependency Concerns
The backend should preserve and return:
- `ParentID`
- Dependency field values
- Resource assignments if used
- Canonical ordering after edits

## Validation Guidance
Validate remotely for:
- Required identifiers
- Legal parent-child relationships
- Date consistency
- Dependency integrity

## Common Issues
### Loading works but updates fail
Check whether insert, update, remove, and batch URLs are configured and implemented.

### Hierarchy looks wrong after save
Ensure the server returns normalized parent and ordering values.

### Search or filter does not match expectation
Verify server query semantics align with the selected adaptor.

## Summary
Use `UrlAdaptor` for most application-specific APIs, OData adaptors for OData backends, and custom adaptors only when the request contract truly requires one.
