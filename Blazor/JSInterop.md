# JS Interop

Blazor 에서는 자바스크립트를 사용하지않고
자바스크립트가 할 수 있는 기능을 구현할 수 있다.
하지만 외부라이브러리같은경우 자바스크립트로 작성되어있어서
C# 으로는 사용을 하지못하는데 JSRuntime 을 사용하면 사용할 수 있다.

##### /Pages/JsInterop.razor
```cs
@page "/JSInterop"
@inject IJSRuntime JSRuntime

<div>
    <button type="button" class="btn btn-primary" @onclick="ShowAlert">Show Alert</button>
</div>

<br />

<div>
    <button type="button" class="btn btn-primary" @onclick="InputName">Input Name</button>
    <p>@_name</p>
</div>

@code {
    private async void ShowAlert()
    {
        await JSRuntime.InvokeVoidAsync("showAlert");
    }

    string _name = "";

    private async void InputName()
    {
        _name = await JSRuntime.InvokeAsync<string>("inputName", "Write your name");
        StateHasChanged();
    }
}
```

##### /wwwroot/test.js
```js
function showAlert() {
    alert("Hello World");
}

function inputName(text) {
    return prompt(text, 'Input Name');
}
```

##### /Pages/_Host.cshtml
```cs
@page "/"
@namespace BlazorApp.Pages
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@{
    Layout = null;
}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>BlazorApp</title>
    <base href="~/" />
    <link rel="stylesheet" href="css/bootstrap/bootstrap.min.css" />
    <link href="css/site.css" rel="stylesheet" />
</head>
<body>
    <app>
        <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <div id="blazor-error-ui">
        <environment include="Staging,Production">
            An error has occurred. This application may no longer respond until reloaded.
        </environment>
        <environment include="Development">
            An unhandled exception has occurred. See browser dev tools for details.
        </environment>
        <a href="" class="reload">Reload</a>
        <a class="dismiss">🗙</a>
    </div>

    <script src="_framework/blazor.server.js"></script>
    <script src="~/test.js"></script>
</body>
</html>

```