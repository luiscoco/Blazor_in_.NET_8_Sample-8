# Blazor in .NET 8: Sample 8

Let's explore Blazor's capability to create components that respond to changes in browser size and adapt their layout accordingly

**Example: Responsive Dashboard Layout**

Features Demonstrated:

**CSS Media Queries**: Applying different styles based on screen size

**JavaScript Interop for Dynamic Updates**: (Optional, for more advanced scenarios) Detecting browser resize events and triggering component updates from JavaScript

**Component Layout**: Using layout structures (e.g., Grid, Flexbox) within Blazor components

**Setup**

**Project**: Create a new Blazor project or reuse an existing one. Let's call it "ResponsiveBlazer"

**Components**

**Dashboard.razor: (Inside the Pages folder)**:

```cshtml
@page "/dashboard"
@inject IJSRuntime JSRuntime  

<div class="dashboard-container">
    <div class="sidebar @SidebarClass">
        ... Sidebar Content/Navigation ...
    </div>
    <div class="main-content">
        ... Main Dashboard Content ...
    </div>
</div>

@code {
    private string SidebarClass = "sidebar-expanded";

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            // Optional: Setup JavaScript for resize event handling
            await JSRuntime.InvokeVoidAsync("setupResizeListener", DotNetObjectReference.Create(this));
        }
    }

    [JSInvokable] 
    public void ToggleSidebar() 
    {
        SidebarClass = SidebarClass == "sidebar-expanded" ? "sidebar-collapsed" : "sidebar-expanded";
        StateHasChanged();
    }
}
```

**Add CSS (wwwroot/css/site.css for example)**:

```css
.dashboard-container {
    display: grid;
    grid-template-columns: 250px auto; /* Adjust as needed */
}

.sidebar-expanded {
    /* Styles for expanded sidebar */
}

.sidebar-collapsed {
    /* Styles for collapsed sidebar */
}

/* Media query for smaller screens */
@media (max-width: 768px) { 
    .dashboard-container {
        grid-template-columns: 1fr; /* Stack the sidebar and main content */
    }

    .sidebar-collapsed {
        /* Make the sidebar overlay or disappear */
    }
}
```

**Optional JavaScript (in wwwroot/index.html or  _Host.cshtml)**:

```javaScript
window.setupResizeListener = function (dotNetHelper) {
    window.addEventListener('resize', () => {
        // Example: Toggle sidebar based on screen size 
        if (window.innerWidth < 768) {
            dotNetHelper.invokeMethodAsync('ToggleSidebar');
        }
    });
}
```
