# 람다 (Lambda)

<br>

람다는 익명 메서드를 만들기 위해 사용되는 `Type`이다.
델리게이트를 사용하여 익명 메서드를 만들 수 있지만
람다를 사용하여 익명 메서드를 만들면 코드가 훨씬 더 간결해진다.

람다에는 두가지 형태가 있다.
- ### 람다 식 (Expression Lambda)
```cs
(파라미터) => 코드;
```
- ### 람다 문 (Statement Lambda)
```cs
(파라미터) =>
{
    코드
}
```

<br/>

람다는 `=>` 연산자로만 이루어지며 형식 유추라는 기능을 제공하기때문에
파라미터의 데이터타입을 입력하지않아도 되고, 개수가 한개일 경우 괄호를 생략할 수 있다.

```cs
delegate int Calc(int x, int y);
```
```cs
Clac Plus = (int x, int y) => x + y;
Console.WriteLine(Plus(10, 20));    // 30

Clac Minus = (x, y) => 
{
    return x - y;
}

Console.WriteLine(Plus(10, 20));    // -10
```

<br>

.NET Framework 에서는 Func 와 Action 이라는 델리게이트를 기본적으로 제공해주고
있기때문에 람다를 사용할 때마다 직접 델리게이트를 만들어 줄 필요가없다.

- ### Func Delegate
Func 델리게이트는 형식 파라미터중 가장 마지막 파라미터가 델리게이트의
반환 타입을 나타내며, 파라미터를 0개부터 16개까지 받을 수 있도록 
17개의 Func Delegate 버전이 정의되어있다.
```cs
public delegate TResult Func<out TResult>();
public delegate TResult Func<in T, out TResult>(T arg);
public delegate TResult Func<in T1, in T2, out TResult>(T arg);
...
```

Func 델리게이트는 아래와 같이 사용될 수 있다.

```cs
Func<int, int, int> funcPlus = (x, y) => x + y;
Func<int, int, int> funcMinus = (x, y) =>
{
    return x - y;
};
```
```cs
Console.WriteLine(Plus(10, 20));    // 30
Console.WriteLine(Minus(10, 20));   // -10
```

- ### Action Delegate
Action 델리게이트는 반환형식이 없다는걸 제외하면 Func 델리게이트와 동일하다.
Action 델리게이트는 보통 일반 메서드의 반환형식이 void 일때 사용된다.
```cs
Action<string> actionPrint = message => Console.WriteLine(message);
```
```cs
actionPrint("Hello");   // Hello
```

<br>

다른 메서드에서 파라미터로 람다를 넘겨주려면 아래와 같이 사용할 수 있다.
```cs
public int Count(int[] numbers, Func<int, bool> judge)
{
    int count = 0;

    foreach (var n in numbers)
    {
        if (judge(n) == true)
            count++;
    }

    return count;
}
```
```cs
int[] numbers = new int[] { 1, 2, 3, 4, 5, 6, 7 };
Count(numbers, x => x % 2 == 0);

// 3
```
