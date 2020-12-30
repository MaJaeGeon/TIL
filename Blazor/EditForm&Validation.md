# EditForm & Validation

EditForm 은 HTML form 태그에 기능이 추가된것으로  
간편하게 데이터를 전송할 수 있도록 해준다.

Validation 은 EditForm 에서 넘겨지는 데이터의 유효성 검사를 할 수 있다.

## 하위 컴포넌트에 값을 넘겨주는 방법

##### /Models/WeatherForecastModel.cs
```cs
using System.ComponentModel.DataAnnotations;

public class WeatherForecastModel
{
    public DateTime Date { get; set; }

    [Required, Range(-100, 100)]
    public int TemperatureC { get; set; }

    public int TemperatureF => 32 + (int)(TemperatureC / 0.5556);

    [Required, StringLength(10, MinimumLength = 2)]
    public string Summary { get; set; }
}
```

##### /Pages/FetchData.razor
```cs
@inject WeatherForecastService ForecastService

<EditForm Model="_forecast" OnValidSubmit="SaveForecast">
    @* Form Validation 기능 On *@
    <DataAnnotationsValidator />
    
    @* 에러가 있으면 메시지 상세 출력 *@
    <ValidationSummary />

    <label for="TemperatureC">TemperatureC</label>
    <InputNumber class="form-control" placeholder="TemperatureC" @bind-Value="_forecast.TemperatureC" />

    <label for="Summary">Summary</label>
    <InputText class="form-control" placeholder="Summary" @bind-Value="_forecast.Summary" />

    <br />

    <button class="btn btn-primary" type="submit">Save</button>
</EditForm>

@code {
    private List<WeatherForecast> _forecasts;
    WeatherForecast _forecast;

    protected override async Task OnInitializedAsync()
    {
        _forecasts = await ForecastService.GetForecastAsync(DateTime.Now);
    }

    private void SaveForecast()
    {
        _forecast = new WeatherForecast();
        _forecast.Date = DateTime.Now;
        _forecasts.Add(_forecast);
    }
}
```

<br>