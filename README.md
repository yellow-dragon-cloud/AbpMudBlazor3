# MudBlazor Theme in ABP Blazor WebAssembly PART 3

## Introduction

In this part, I will show you how to replace the built-in messaging service to show [MudBlazor](https://www.mudblazor.com/) [message boxes](https://www.mudblazor.com/components/messagebox#api).

## 8. Make Sure You Have Completed PART 1 & Part 2

This project is built on top of [MudBlazor Theme in ABP Blazor WebAssembly PART 1](https://github.com/yellow-dragon-cloud/AbpMudBlazor) and [MudBlazor Theme in ABP Blazor WebAssembly PART 2](https://github.com/yellow-dragon-cloud/AbpMudBlazor2). Before continuing, you need to complete these parts in order, if you haven't already.

## 9. Add [MudBlazor MessageBox](https://www.mudblazor.com/components/messagebox#api) Support to MainLayout

Open `MainLayout.razor` and replace it with the code below:

```razor
@inherits LayoutComponentBase

<MudThemeProvider />
<MudSnackbarProvider />
<MudDialogProvider />

<MudLayout>
    <MudAppBar Elevation="8">
        <MudIconButton Icon="@Icons.Material.Filled.Menu" Color="MudBlazor.Color.Inherit" Edge="Edge.Start" 
                       OnClick="@((e) => DrawerToggle())" />
        <Branding />
        <MudSpacer />
        <NavToolbar />
    </MudAppBar>
    <MudDrawer @bind-Open="_drawerOpen" ClipMode="DrawerClipMode.Always" Elevation="8">
        <NavMenu />
    </MudDrawer>
    <MudMainContent>
        <MudContainer MaxWidth="MaxWidth.False" Class="mt-4">
            <PageAlert />
            @Body
            <UiPageProgress />
        </MudContainer>
    </MudMainContent>
</MudLayout>

@code 
{
    private bool _drawerOpen = true;

    private void DrawerToggle()
    {
        _drawerOpen = !_drawerOpen;
    }
}
```

## 10. Replace [Message Service](https://docs.abp.io/en/abp/latest/UI/Blazor/Message)

In `Volo.Abp.AspNetCore.Components.Web.BasicTheme/Services` folder, create a new `MudBlazorUiMessageService.cs` file with the following code:

```csharp
using System;
using System.Threading.Tasks;
using Localization.Resources.AbpUi;
using Microsoft.Extensions.Localization;
using MudBlazor;
using Volo.Abp.AspNetCore.Components.Messages;
using Volo.Abp.DependencyInjection;

namespace Volo.Abp.AspNetCore.Components.Web.BasicTheme.Services;

[Dependency(ReplaceServices = true)]
public class MudBlazorUiMessageService : IUiMessageService, IScopedDependency
{
    private readonly IDialogService _dialogService;
    private readonly IStringLocalizer<AbpUiResource> _localizer;

    public MudBlazorUiMessageService(IDialogService dialogService, IStringLocalizer<AbpUiResource> localizer)
    {
        _dialogService = dialogService;
        _localizer = localizer;
    }

    public async Task<bool> Confirm(string message, string title = null, Action<UiMessageOptions> options = null)
    {
        var result = await _dialogService.ShowMessageBox(
            title, message, yesText: _localizer["Yes"], noText: _localizer["No"]);
        return result ?? false;
    }

    public Task Error(string message, string title = null, Action<UiMessageOptions> options = null)
    {
        _dialogService.ShowMessageBox(title, message, yesText: _localizer["Ok"]);
        return Task.CompletedTask;
    }

    public Task Info(string message, string title = null, Action<UiMessageOptions> options = null)
    {
        _dialogService.ShowMessageBox(title, message, yesText: _localizer["Ok"]);
        return Task.CompletedTask;
    }

    public Task Success(string message, string title = null, Action<UiMessageOptions> options = null)
    {
        _dialogService.ShowMessageBox(title, message, yesText: _localizer["Ok"]);
        return Task.CompletedTask;
    }

    public Task Warn(string message, string title = null, Action<UiMessageOptions> options = null)
    {
        _dialogService.ShowMessageBox(title, message, yesText: _localizer["Ok"]);
        return Task.CompletedTask;
    }
}
```

This code replaces `IUiMessageService` service. See [Overriding Services](https://docs.abp.io/en/abp/latest/Customizing-Application-Modules-Overriding-Services) for more information.

Finally, edit `Index.razor` to test new message boxes:

```razor
@page "/"
@using Volo.Abp.AspNetCore.Components.Messages
@using Volo.Abp.AspNetCore.Components.Notifications
@inherits BookStoreComponentBase
@inject AuthenticationStateProvider AuthenticationStateProvider
@inject IUiNotificationService NotificationService
@inject IUiMessageService MessageService

<div class="container">
    <div class="p-5 text-center">
        <Button onclick="@(async () => { await NotificationService.Success("Hello, World!"); await NotificationService.Warn("Something went wrong!"); })">
            Show Notifications!
        </Button>
        <hr />
        <Button onclick="@(() => { MessageService.Info("Hello, World!"); })">
            Show Info!
        </Button>
        <br />
        <Button onclick="@(() => { throw new Exception(); })">
            Show Error!
        </Button>
    </div>
</div>
```

## The Result

![image](images/screenshot2.png)

## Next

Complete BookStore sample with MudBlazor components (COMMING SOON!)
