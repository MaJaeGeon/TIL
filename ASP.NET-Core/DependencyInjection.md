# 종속성 주입 (DI : Dependency Injection)

DI 란 하나의 객체가 다른 객체를 생성하고 종속성을 제공하는 기술로  
객체의 생성과 사용의 관심을 분리하여 가독성과 코드 재사용성을 높여줄 수 있다.  

종속성은 한 객체가 다른 객체에 종속되는것을 말하는것이다.  

```cs
public class MyDependency 
{ 
    public void WriteMessage(string message)
    { 
        Console.WriteLine($"MyDependency.WriteMessage called. Message: {message}"); 
    } 
} 

public class IndexModel : PageModel 
{ 
    private readonly MyDependency _dependency = new MyDependency(); 

    public void OnGet() 
    { 
        _dependency.WriteMessage("IndexModel.OnGet created this message."); 
    } 
}
```

위의 예에서는 MyDependency 클래스의 인스턴스는 IndexModel 클래스에 종속되어있기 때문에  
MyDependency 클래스가 수정되면 IndexModel 에서의 구현도 수정되어야된다.  

DI를 사용하면 위와 같은 문제들을 다음 방법으로 해결할 수 있다.

1.  종속성 구현을 인터페이스나 클래스를 사용하여 추상화한다.
    ```cs
    public interface IMyDependency 
    { 
        void WriteMessage(string message); 
    } 
    
    public class MyDependency : IMyDependency 
    { 
        public ILogger<MyDependency> Logger { get; set; } 
        public MyDependency(ILogger<MyDependency> logger) 
        { 
            Logger = logger; 
        } 

        public void WriteMessage(string message) 
        { 
            Logger.LogInformation($"MyDependency.WriteLine Message : {message}"); 
        } 
    }
    ```

2.  서비스 컨테이너인 IServiceProvider 에 DI를 할 서비스를 등록한다.  
    IServiceProvider는 ASP.NET Core에서 기본적으로 제공하는 서비스 컨테이너이며,  
    서비스는 보통 Startup클래스의 ConfigureServices 메서드에서 등록된다.
    ```cs
    public class Startup
    { 
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddScoped<IMyDependency, MyDependency>();
        }
    }
    ```

3.  DI를 사용할 클래스의 생성자에 원하는 서비스를 주입한다.  
    프레임워크는 종속성의 인스턴스를 생성하고, 더이상 필요하지 않을 때 삭제하는 작업을 담당한다.
    ```cs
    public class HomeController : Controller 
    { 
        private readonly IMyDependency MyDependency; 
        public HomeController(IMyDependency myDependency) 
        { 
            MyDependency = myDependency; 
        } 
        
        public void Index() 
        { 
            MyDependency.WriteMessage("Home Controller : Index"); 
        } 
    }
    ```

DI 패턴을 사용하면 컨트롤러에서는 구현클래스인 MyDependency를 사용하지않고  
IMyDependency 인터페이스만을 사용하기때문에 MyDependency클래스는  
컨트롤러를 수정하지 않고도 쉽게 변경할 수 있다.  
또한 MyDependency클래스의 인스턴스를 생성하지않으며 이 인스턴스는 DI 컨테이너에 의해 생성된다.  

>   DI에서 말하는 서비스란 MyDependency와 같이 서비스를 제공하는 객체를 의미한다.  
>   이 서비스는 웹 서비스를 사용할 수 있지만 웹 서비스와 관련이 있는것은 아니다.  


## Startup클래스에 서비스 주입하기
서비스는 Startup클래스의 생성자와 Configure 메서드에서 주입될 수 있다.  
Generic Host(IHostBuilder)를 사용할 경우 Startup 생성자에는 아래의 서비스 타입만 주입될 수 있다.
-   [IWebHostEnvironment](https://docs.microsoft.com/ko-kr/dotnet/api/microsoft.aspnetcore.hosting.iwebhostenvironment)
-   [IHostEnvironment](https://docs.microsoft.com/ko-kr/dotnet/api/microsoft.extensions.hosting.ihostenvironment)
-   [Configuration](https://docs.microsoft.com/ko-kr/dotnet/api/microsoft.extensions.configuration.iconfiguration)

>   ASP.NET Core에서 사용되는 호스트는 Generic Host인 IHostBuilder 와 Web Host인 IWebHostBuilder 가 있다.  
>   Web Host는 이전버전과의 호환성을 위해 남아있기때문에 MS Docs 에서는 Generic Host의 사용을 권장하고있다.  
>   따라서 Startup 생성자는 위의 세가지 타입만 주입될 수 있다고 보는게 좋다.  

DI 컨테이너에 등록된 모든 서비스들은 Configure 메서드에 주입할 수 있다.
