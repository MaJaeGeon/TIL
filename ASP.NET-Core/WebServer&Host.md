# WebServer & Host
<br>

## WebServer

서버는 HTTP 요청을 수신하고, 이를 요청 기능 (Request Features)들의 집합으로 구성한  
HTTPContext 의 형태로 응용프로그램에 전달한다.
ASP.NET Core 에서는 내부적으로 `Kestrel`을 기본 웹 서버로 사용하고있다.

- Kestrel 은 네트워크의 요청을 직접 처리할 수 있다.  
  ![Kestrel-EdgeServer](https://docs.microsoft.com/ko-kr/aspnet/core/fundamentals/servers/kestrel/_static/kestrel-to-internet2.png)
- IIS, Nginx, Apache 와 같은 [역방향 프록시 서버](https://firework-ham.tistory.com/23)를 사용하여 간접적으로 요청을 받아 처리할 수 있다.  
  ![Kestrel-ReverseProxyServer](https://docs.microsoft.com/ko-kr/aspnet/core/fundamentals/servers/kestrel/_static/kestrel-to-internet.png)

<br>

## Host

ASP.NET Core 에서 사용하는 호스트는 두가지로, `.NET Generic Host`, `ASP.NET Core Web Host` 가 있다.  
`ASP.NET Core Web Host` 는 이전 버전과의 하위호환성을 위해 존재하며,  
ASP.NET Core 에서는 `.NET Generic Host` 의 사용을 권장하고 있다.  
`.NET Generic Host` 는 `Host`, `ASP.NET Core Web Host` 는 `WebHost` 라는 클래스명으로 사용된다.  

Host 는 DI, Logging, Configuration, IHostedService 와 같은 웹앱의 리소스들을 캡슐화 하는 객체이다.  
