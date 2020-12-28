# Templated Component
RenderFragment 또는 RenderFragment<T> 형식의 파라미터를 통해  
HTML 태그들을 파라미터로 넘겨받을 수 있다.

##### Parents.razor
```cs
<TableTemplate Items="forecasts">
    <Header>
        <th>Date</th>
        <th>Temp. (C)</th>
        <th>Temp. (F)</th>
        <th>Summary</th>
    </Header>
    <Row Context="forecast">
        <td>@forecast.Date.ToShortDateString()</td>
        <td>@forecast.TemperatureC</td>
        <td>@forecast.TemperatureF</td>
        <td>@forecast.Summary</td>
    </Row>
</TableTemplate>
```

##### Child.razor
```cs
@using BlazorApp.Data
@typeparam TItem

<h3>TableTemplate</h3>

<table class="table">
    <thead>
        <tr>
            @Header
        </tr>
    </thead>
    <tbody>
        @foreach (var item in Items)
        {
            <tr>
                @Row(item)
            </tr>
        }
    </tbody>
</table>

@code {
    [Parameter]
    public RenderFragment Header { get; set; }

    [Parameter]
    public RenderFragment<TItem> Row { get; set; }

    [Parameter]
    public IReadOnlyList<TItem> Items { get; set; }
}
```