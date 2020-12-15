# LINQ (Language INtegrated Query)

<br>

## 1. LINQ 기본

LINQ 는 컬렉션을 편리하게 다룰 수 있게 해주는 Query 언어이다.  
아래는 LINQ 의 표현방법이다.
```cs
from 범위변수 in 데이터소스
where 필터링
orderby 범위변수의 정렬기준
select 범위변수;

```

##### 소스1

```cs
int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9};
var Even = from num in numbers
           where num % 2 == 0
           orderby num
           select num;
           
foreach(var i in Even) Console.Write(i);    // 2468
```

### from

모든 LINQ 쿼리식은 반드시 from 으로 시작해야하며, from 의 데이터소스는  
IEnumerable<T> 인터페이스를 상속받고있는 형식이어야한다.  
배열이나 컬렉션 객체들은 기본적으로 IEnumerable<T> 인터페이스를 상속받고 있다.

소스1에서 아래의 구문은 numbers 배열의 데이터를 num 이라는 범위변수에 넣는다.
```cs
from num in numbers
```

### where

where 에서는 데이터소스로부터 가져올 데이터에 조건을 걸어 필터링을 한다.
소스1에서 아래의 구문은 num 범위변수에서 짝수값만 골라낸다.
```cs
where num % 2 == 0
```

### orderby

orderby 에서는 필터링된 데이터들을 정렬한다.
소스1에서 아래의 구문은 num 을 오름차순으로 정렬한다.
```cs
orderby num
```
orderby 연산자는 기본적으로 오름차순으로 정렬되지만 ascending 키워드를  
명시적으로 입력해도되며 내림차의 경우 descending 키워드를 사용한다.
```cs
orderby num ascending   // 오름차
orderby num descending  // 내림차
```

### select

LINQ 쿼리식에서 최종적인 결과를 추출하며 쿼리식의 마침표와 같다.
```cs
select num;
```
또한 select 에서는 최종적인 결과를 무명형식으로 만들어낼 수도 있다.
```cs
select new {Number = num};
```

<br>

## 2. 여러 데이터소스에 Query 하기