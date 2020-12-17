# Startup Class

Startup 클래스는 웹앱의 서비스와 요청 파이프라인을 구성한다.  
ASP.NET Core 에서는 관례상 Startup 이라고 명명된 클래스를 웹앱의  
서비스 및 요청 파이프라인 구성설정에 사용하며, 이 이름은 다르게 지정될 수 있다.

Startup 클래스는 기본적으로 아래의 세 메서드를 가진다.
-  Startup Constructor : Startup 클래스의 생성자로 서비스를 받아올 수 있다. `optional`
-  ConfigureServices Method : 웹앱에서 사용될 services 를 등록한다. `optional`
-  Configure Method : 웹앱의 요청을 처리할 파이프라인을 구성한다. `required`

ConfigureServices 와 Configure 메서드는 웹앱이 시작될 때 ASP.NET Core Runtime 에 의해  
호출되는데 ConfigureServices 메서드를 사용할 경우 Configure 메서드보다 먼저 호출된다.

```cs
public class Startup
{
    public IConfiguration Configuration { get; }
    
    public Startup(IConfiguration configuration)
    {
    	Configuration = configuration;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        services.AddDbContext<MyAppContext>(option =>
        {
            option.UseSqlServer(_config.GetConnectionString("MyAppConnection"));
        });

        //services.AddTransient<DbSeeder>();
        //services.AddScoped<IMemberRepository, MemberRepository>();
    }

    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
        
        app.UseStaticFiles();

        app.UseRouting();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapGet("/", async context =>
            {
                await context.Response.WriteAsync("Hello, World!");
            });
        });
    }
}
```

Startup 클래스는 웹앱의 호스트가 빌드될 때 `Host Builder` 에서  
`WebHostBuilderExtensions.UseStartup<Tstartup>` 메서드를 호출하여 Startup 클래스를 지정한다.  
`Tstartup` 에는 Startup 클래스로 사용할 클래스의 형식이 들어간다.

```cs
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
}
```

<br>

### Startup Constructor

호스트는 Startup 클래스의 생성자에 services 를 넘겨준다.  
사용하고 있는 호스트가 .NET Generic Host 일 경우 아래의 세가지 서비스를 선택적으로 받을 수 있다.
-  IWebHostEnvironment
-  IHostEnvironment
-  IConfiguration

```cs
public class Startup
{
    public IConfiguration Configuration { get; }
    public IHostEnvironment HostEnvironment { get; }
    public IWebHostEnvironment WebHostEnvironment { get; }

    public Startup(IConfiguration configuration,
                IHostEnvironment hostEnvironment,
                    IWebHostEnvironment webHostEnvironment)
    {
        Configuration = configuration;
        HostEnvironment = hostEnvironment;
        WebHostEnvironment = webHostEnvironment;
    }
}
```
추가적인 서비스는 ConfigureServices 메서드를 통해 추가할 수 있으며  
호스트와 앱의 서비스는 Configure 메서드와 웹앱 전체에서 사용될 수 있다.

<br>

### ConfigureServices Method

ConfigureServices 메서드는 Configure 메서드가 호출되기전에 호스트에 의해 호출되어,  
웹앱에서 사용될 서비스들을 서비스 컨테이너에 등록한다.
또한 이곳에서 `appsetting.json` 과 같은 구성옵션이 규칙에 의해 설정되며,  
IServiceCollections 에 있는 `Add{Service}` 확장 메서드를 사용할 수 있다.

```cs
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddDbContext<MyAppContext>(option =>
        {
            option.UseSqlServer(_config.GetConnectionString("MyAppConnection"));
        });

        //services.AddTransient<DbSeeder>();
        //services.AddScoped<IMemberRepository, MemberRepository>();
	}
}
```
서비스 컨테이너에 서비스를 등록하면 웹앱 전체와 Configure 메서드에서 사용할 수 있으며,  
서비스는 Dependency Injection (DI) 또는 ApplicationServices를 통해 사용된다.

<br>

### Configure Method

Configure 메서드에서는 웹앱의 HTTP 요청에 대한 응답 방식을 지정한다.  
요청 파이프라인은 미들웨어를 IApplicationBuilder 의 인스턴스에 추가하여 구성된다.  
IApplicationBuilder 는 Configure 메서드 내에서 사용할 수 있지만 호스트가 IApplicationBuilder 를  
생성하고 DI 로 넘겨주는것이 아닌 Configure 메서드에  
직접적으로 넘겨주기때문에 서비스 컨테이너에는 등록되지 않는다.

```cs
public class Startup
{
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
        
        app.UseStaticFiles();

        app.UseRouting();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapGet("/", async context =>
            {
                await context.Response.WriteAsync("Hello, World!");
            });
        });
    }
}
```
각 Use 확장 메서드는 요청 파이프라인에 하나이상의 미들웨어를 추가한다.  
예를 들어, UseStaticFiles 메서드는 요청 파이프라인에서  
웹앱의 정적 파일을 제공하는 미들웨어를 사용할 수 있도록 한다.