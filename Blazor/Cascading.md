# Cascading

상위 컴포넌트에서 하위 컴포넌트로 값을 넘겨주기위해 
컴포넌트 파라미터로 값을 넘겨줄 수 있지만 하위 컴포넌트가 여러개라면
아래와 같은 상황이 일어나서 불편할 수 있다.

```cs
<component1 Name="name">
    <component2 Name="name">
        <component3 Name="name">
        </component3>
        <component3 Name="name">
        </component3>
    </component2>
</component1>
```

Cascading Value 와 Cascading Parameter 를 사용하면
편리한 방법으로 하위 컴포넌트에게 데이터를 이동시킬 수 있다.

##### Parents.razor
```cs
<CascadingValue Name="ThemeColor" Value="Red">
    <Child></Child>
</CascadingValue>
```
##### Child.razor
```cs
<div>
    <h1 style="color:@Color">I am Child Component</h1>
</div>

@code {
    [CascadingParameter(Name = "ThemeColor")]
    private string Color { get; set; }
}
```