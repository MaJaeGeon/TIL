# 델리게이트 (Delegate)

<br>

델리게이트는 메서드를 참조하는 `Type`으로, 델리게이트의 객체를 만들어서 사용하게된다.
델리게이트를 호출하게되면 델리게이트가 참조하고있는 메서드가 호출된다.

##### 델리게이트 형식
```cs
private delegate 반환형식 델리게이트명(파라미터들);
```

델리게이트에 참조하려고하는 메서드는 델리게이트와 형식이 같아야한다.
예를 들어 반환타입이 `bool`이고, 파라미터가 `int`형 변수 2개라면
델리게이트도 반환타입이 `bool` 이고, 파라미터도 `int`형 변수 2개를 가지고 있어야 한다.

```cs
private delegate bool judgement(int x);
public bool Two(int value)
{ 
    return value % 2 == 0; 
} 
```
```cs
judgement judge = Two;
if(judge(2))
    Console.WriteLine("짝수입니다.");   // 짝수입니다.
```

델리게이트는 메서드에서 값이 아닌 코드 즉, 메서드를 파라미터로 넘겨주고싶을때 사용한다.
아래의 예제처럼 델리게이트를 사용하면 메서드를 오버라이딩 하지않고도 여러 조건을 판단할 수 있다.

```cs
public delegate bool judgement(int x);

public bool Two(int value)
{
    return value % 2 == 0;
}

public bool Three(int value)
{
    return value % 3 == 0;
}

public void OddOrEven(int value, judgement judge)
{
    if (judge(value)) Console.WriteLine("짝수입니다.");
    else Console.WriteLine("홀수입니다.");
}
```
```cs
OddOrEven(10, Two);     // 짝수입니다.
OddOrEven(10, Three);   // 홀수입니다.
```

### 델리게이트 체인
하나의 델리게이트는 여러 메서드를 동시에 참조할 수 있다.
```cs
public delegate void Print(string Message);

public void print1(string message)
{
    Console.WriteLine("print1 Message : " + message);
}

public void print2(string message)
{
    Console.WriteLine("print2 Message : " + message);
}

public void print3(string message)
{
    Console.WriteLine("print3 Message : " + message);
}
```
```cs
Print printer_1 = print1;
printer_1 += print2;
printer_1 += print3;

printer_1("Print Delegate 1");

Print printer_2 = (Print)Delegate.Combine(
    new Print(print1),
    new Print(print2),
    new Print(print3)
);

/*
print1 Message : Print Delegate 1
print2 Message : Print Delegate 1
print3 Message : Print Delegate 1
print1 Message : Print Delegate 2
print2 Message : Print Delegate 2
print3 Message : Print Delegate 2
*/
```

### 익명 메서드
메서드는 한정자, 반환타입, 파라미터가 없어도 메서드명은 반드시 있어야한다.
하지만 델리게이트를 사용하면 이름이 없는 메서드를 만들 수 있다.
```cs
public delegate int Clac(int x, int y);

Clac Plus = delegate (int x, int y) { return x + y; };
Clac Minus = delegate (int x, int y) { return x - y; };
```
```cs
Console.WriteLine(Plus(10, 20));    // 30
Console.WriteLine(Minus(10, 20));   // -10
```