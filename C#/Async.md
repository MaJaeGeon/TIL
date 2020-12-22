# Async

<br>

## 1. 스레드를 이용한 비동기 처리

스레드를 사용하면 같은 프로세스 안에서 여러 개의 처리를 병행하여 동작시킬 수 있다.  
```cs
private void DoSomething()
{
    // 시간이 소요되는 처리
}

var th = new Thread(DoSomething);
th. Start();
```

## 2. BackgroundWorker를 이용한 비동기 처리

BackgroundWorker Class 를 사용하면 Event를 사욯한 비동기 처리를 할 수 있다.  

```cs
private void DoSomething(object sender, DoWorkEventArgs e)
{
    // 시간이 소요되는 처리
}

BackgroundWorker worker = new BackgroundWorker();
worker.DoWork += DoSomething;

worker.RunWorkerAsync();
```

## 3. Task를 이용한 비동기 처리

Task Class 는 Thread를 사용한 방식에서 Thread를 생성/삭제할때 드는  
비용문제를 해결하고, 좀더 수준높은 기능을 제공한다. 

```cs
private void DoSomething()
{
    // 시간이 소요되는 처리
}

Task.Run(() => DoSomething());
```

## 4. async/await를 이용한 비동기 처리

C# 5.0 부터 도입되었으며 Task 를 이용한 비동기 처리를  
더욱 간단히 구현할 수 있게 해준다.

```cs
private async Task DoSomethingAsync()
{
    await Task.Run(() => {
        //시간이 소요되는 처리
    });
}

static async void Main()
{
    await DoSomethingAsync();
}
```