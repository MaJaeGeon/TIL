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

from 문을 중첩하여 사용하면 여러데이터소스에 접근할 수 있다.

```cs
class Class
{
    public string Name {get; set;}
    public int[] Scores {get; set;}
}

Class[] classArr = 
{
    new Class(){ Name = "A", Scores = new int[]{11, 23, 34, 47, 55}},
    new Class(){ Name = "B", Scores = new int[]{15, 20, 38, 46, 53}},
    new Class(){ Name = "C", Scores = new int[]{13, 26, 33, 41, 52}},
    new Class(){ Name = "D", Scores = new int[]{10, 25, 32, 45, 59}}
};

var classes = from c in classArr
              from s in c.Scores
              where s < 30
              select new { c.Name, Score = s };
foreach (var i in classes)
    Console.WriteLine($"{i.Name} : {i.Score}");
```
```cs
A : 11
A : 23
B : 15
B : 20
C : 13
C : 26
D : 10
D : 25
```

<br>

## 3. 데이터 분류하기

`group by`를 사용하면 데이터 컬렉션에서 데이터를 특정 기준으로 분류할 수 있다.
```cs
group 범위 변수 by 분류 기준 into 그룹 변수
```

```cs
class Profile
{
    public string Name { get; set; }
    public int Height { get; set; }
}

Profile[] ProfileArr =
{
    new Profile(){ Name = "A", Height = 172},
    new Profile(){ Name = "B", Height = 163},
    new Profile(){ Name = "C", Height = 173},
    new Profile(){ Name = "D", Height = 168}
};

var listProfile = from profile in ProfileArr
                group profile by profile.Height < 170 into g
                select new { GroupKey = g.Key, Profiles = g };

foreach (var Group in listProfile)
{
    Console.WriteLine($"-170 : {Group.GroupKey}");

    foreach (var profile in Group.Profiles) Console.WriteLine($"{profile.Name} : {profile.Height}");
}
```
```cs
-170 : False
A : 172
C : 173
-170 : True
B : 163
D : 168
```

<br>

## 3. 데이터 분류하기
`join`을 사용하여 특정 필드가 일치한 두 데이터소스를 연결할 수 있다.

##### 소스1
```cs
class Post
{
    public string Title {get; set;}
    public string Writer {get; set;}
}

class Profile
{
    public int Id {get; set;}
    public string Name {get; set;}
}

Post[] posts =
{
    new Post(){ Title = "가", Writer = "A"},
    new Post(){ Title = "나", Writer = "B"},
    new Post(){ Title = "다", Writer = "C"},
    new Post(){ Title = "라", Writer = "D"},
    new Post(){ Title = "마", Writer = "Z"}
};

Profile[] profiles =
{
    new Profile(){ Id = 1, Name = "A"},
    new Profile(){ Id = 2, Name = "B"},
    new Profile(){ Id = 3, Name = "C"},
    new Profile(){ Id = 4, Name = "D"},
    new Profile(){ Id = 5, Name = "E"}
};

```

<br>

- 내부 조인 (Inner Join)  
  내부 조인은 교집합과 비슷하다.  
  두개의 데이터소스중 특정 필드가 일치한 데이터만 연결을 시키고 일치하지않은 필드는 제외된다.

  ```cs
  var profileList = from profile in profiles
                    join post in posts on profile.Name equals post.Writer
                    select new { Id = profile.Id, Name = profile.Name, Title = post.Title };

  foreach (var i in profileList)
      Console.WriteLine($"Id : {i.Id} , Name : {i.Name} , Title : {i.Title}");
  ```
  ```cs
  Id : 1 , Name : A , Title : 가
  Id : 2 , Name : B , Title : 나
  Id : 3 , Name : C , Title : 다
  Id : 4 , Name : D , Title : 라
  ```

- 외부 조인 (Outer Join)  
  외부 조인은 합집합과 비슷하며 내부 조인과 달리  
  조인 결과에 기준이되는 데이터들은 모두 포함이된다.
  ```cs
  var profileList = from profile in profiles
                    join post in posts on profile.Name equals post.Writer into WriterInfo
                    from post in WriterInfo.DefaultIfEmpty(new Post() { Title = "없음"})
                    select new { Id = profile.Id, Name = profile.Name, Title = post.Title };

  foreach (var i in profileList)
      Console.WriteLine($"Id : {i.Id} , Name : {i.Name} , Title : {i.Title}");
  ```
  ```cs
  Id : 1 , Name : A , Title : 가
  Id : 2 , Name : B , Title : 나
  Id : 3 , Name : C , Title : 다
  Id : 4 , Name : D , Title : 라
  Id : 5 , Name : E , Title : 없음
  ```