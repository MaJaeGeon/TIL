# Component

Blazor 에서는 UI 를 컴포넌트단위로 조립하여 사용하고,   
컴포넌트의 확장자는 `*.razor` 이다.  

아래의 예제처럼 컴포넌트 내에서 컴포넌트를 호출하여 사용할 수 있다.

##### Parents.razor
```cs
<Child></Child>
```
##### Child.razor
```cs
<div>
    <h1>Child Component</h1>
</div>
```

## 자식 컴포넌트에 값을 넘겨주는 방법

##### Parents.razor
```cs
<Child Title="Child Component Title"></Child>
```
##### Child.razor
```cs
<div>
    <h1>@Title</h1>
</div>

@code {
    [Parameter]
    public string Title { get; set; }
}
```

## 부모 컴포넌트에서 자식 컴포넌트의 멤버를 사용하는 방법

##### Parents.razor
```cs
<Child @ref="child"></Child>
<button type="button" @onclick="ChangeTitle">Change Title</button>

@code {
    Child child;

    void ChangeTitle()
    {
        child.Title = "Child Component Title with Property";
        child.ChangeTitle("Child Component Title with Method");
    }
}
```
##### Child.razor
```cs
<div>
    <h1>@Title</h1>
</div>

@code {
    private string title;
    public string Title
    {
        get
        {
            return title;
        }
        set
        {
            title = value;
            StateHasChanged();
        }
    }

    public void ChangeTitle(string title)
    {
        Title = title;
    }
}
```

## 자식 컴포넌트에서 부모 컴포넌트의 메서드를 사용하는 방법 1

##### Parents.razor
```cs
<h1>@Title</h1>
<Child ChangeParentTitle="ChangeTitle"></Child>

@{
    public string Title { get; set; }

    void ChangeTitle()
    {
        Title = "Parent Component";

        StateHasChanged();
    }
}
```
##### Child.razor
```cs
<button type="button" @onclick="ChangeTitle">Change Parent Title</button>

@code {

    [Parameter]
    public Action ChangeParentTitle { get; set; }

    public void ChangeTitle()
    {
        ChangeParentTitle();
    }
}
```

## 자식 컴포넌트에서 부모 컴포넌트의 메서드를 사용하는 방법 2

##### Parents.razor
```cs
<h1>@Title</h1>
<Child ChangeParentTitle="ChangeTitle"></Child>

@{
    public string Title { get; set; }

    void ChangeTitle()
    {
        Title = "Parent Component";
    }
}
```
##### Child.razor
```cs
<button type="button" @onclick="ChangeTitle">Change Parent Title</button>

@code {

    [Parameter]
    public EventCallback ChangeParentTitle { get; set; }

    public void ChangeTitle()
    {
        ChangeParentTitle();
    }
}
```