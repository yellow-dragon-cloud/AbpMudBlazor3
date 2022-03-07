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

