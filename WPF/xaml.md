# XAML (Extensible Application Markup Language)

XAML 은 Extensible Application Markup Language 의 약자로 GUI를 나타내기위한 Microsoft XML의 한 형태이다.  
WPF에서 Window나 Page를 만들면 XAML(*.xaml)과 CodeBehind(*.xaml.cs)파일이 생성된다.  
XAML 파일은 xaml이 가진 모든 요소들을 인터페이스에 묘사한다.  
반면에, CodeBehind 파일은 모든 이벤트들을 처리하고 XAML 컨트롤을 사용하여 UI를 조작할 수 있다.  

XAML은 꺽쇠와 컨트롤의 이름만을 사용하여 구성할 수 있고, 태그들은 끝맺음 태그나 시작태그 앞에 슬래시를 써서 닫아주어야한다.

```xaml
<Button/>
<Button></Button>
```

XAML은 HTML 과 유사하지만 HTML과 달리 대소문자를 구분하고있다.
왜냐하면 컨트롤의 이름이 .NET Framework의 Type과 일치해야하기 때문이다.

컨트롤의 각 속성들은 하위태그에서 설정할 수 있다.
아래의 두 코드는 문법차이를 제외하면 모두 동일하다.

```xaml
<Button FontWeight="Bold" Content="Button1"/>

<Button>
    <Button.FontWeight>Bold</Button.FontWeight>
    <Button.Content>Button1</Button.Content>
</Button>
```
하위태그를 사용하여 속성을 설정하면 더욱 세부적인 설정이 가능하다.

```xaml
<Button>
    <Button.FontWeight>Bold</Button.FontWeight>
    <Button.Content>
        <WrapPanel>
            <TextBlock Foreground="Blue">Multi</TextBlock>
            <TextBlock Foreground="Red">Color</TextBlock>
            <TextBlock>Button</TextBlock>
        </WrapPanel>
    </Button.Content>
</Button>

<Button FontWeight="Bold">
    <WrapPanel>
        <TextBlock Foreground="Blue">Multi</TextBlock>
        <TextBlock Foreground="Red">Color</TextBlock>
        <TextBlock>Button</TextBlock>
    </WrapPanel>
</Button>
```