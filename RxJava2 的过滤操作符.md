RxJava2 的过滤操作符
-------------

# 内容简介:
> 本文章的内容是简单讲解和使用 RxJava2 中的过滤操作符.

# 目录:
[1.过滤操作符的使用场景](#1)

[2.过滤操作符的分类](#2)

[3.简单使用](#3)


# <span id = "1">**1.过滤操作符的使用场景**</span>

### 使用场景:

> &ensp;&ensp; RxJava 的过滤操作符即**根据指定条件**过滤事件, **根据事件数量**过滤事件, **根据时间**过滤事件, **根据指定事件位置**过滤事件.


# <span id = "2">**2.过滤操作符的分类**</span>

|操作符|功能|
|:------:|:------:|
|filter()|过滤数据|
|takeLast()|只发射最后的N项数据|
|last()|只发射最后的一项数据|
|lastOrDefault()|只发射最后的一项数据，如果Observable为空就发射默认值|
|takeLastBuffer()|将最后的N项数据当做单个数据发射|
|skip()|跳过开始的N项数据|
|skipLast()|跳过最后的N项数据|
|take()|只发射开始的N项数据|
|first() , takeFirst()|只发射第一项数据，或者满足某种条件的第一项数据|
|firstOrDefault()|只发射第一项数据，如果Observable为空就发射默认值|
|elementAt()|发射第N项数据|
|elementAtOrDefault()|发射第N项数据，如果Observable数据少于N项就发射默认值|
|sample() , throttleLast()|定期发射Observable最近的数据|
|throttleFirst()|定期发射Observable发射的第一项数据|
|throttleWithTimeout() , debounce()|只有在空闲了一段时间后才发射数据，通俗的说，就是如果一段时间没有操作，就执行一次操作|
|timeout()|如果在一个指定的时间段后还没发射数据，就发射一个异常|
|distinct()|过滤掉重复数据|
|distinctUntilChanged()|过滤掉连续重复的数据|
|ofType()|只发射指定类型的数据|
|ignoreElements()|丢弃所有的正常数据，只发射错误或完成通知|


# <span id = "3">**3.简单使用**</span>


`filter()`

```
Observable.just(1, 2, 3, 4).filter(new Func1<Integer, Boolean>() {
            @Override
            public Boolean call(Integer integer) {
                return integer > 2;
            }
        }).subscribe(new Action1<Integer>() {
            @Override
            public void call(Integer integer) {
                Log.d(TAG, "filter:" + integer);
            }
});


```



`elementAt()`

```


Observable.just(1, 2, 3, 4).elementAt(2).subscribe(new Action1<Integer>() {
            @Override
            public void call(Integer integer) {
                Log.d(TAG, "elementAt:" + integer);
            }
});



```


`distinct()`

```

Observable.just(1, 2, 2, 3, 4, 1).distinct().subscribe(new Action1<Integer>() {
            @Override
            public void call(Integer integer) {
                Log.d(TAG, "distinct:" + integer);
            }
});


```




`skip()`

```

Observable.just(1, 2, 3, 4, 5, 6).skip(2).subscribe(new Action1<Integer>() {
            @Override
            public void call(Integer integer) {
                Log.d(TAG, "skip:" + integer);
            }
});


```


`take()`

```

Observable.just(1, 2, 3, 4, 5, 6).take(2).subscribe(new Action1<Integer>() {
            @Override
            public void call(Integer integer) {
                Log.d(TAG, "take:" + integer);
            }
});


```



`ignoreElements()`

```

Observable.just(1, 2, 3, 4).ignoreElements().subscribe(new Observer<Integer>() {
            @Override
            public void onCompleted() {
                Log.i("wangshu", "onCompleted");
            }

            @Override
            public void onError(Throwable e) {
                Log.i("wangshu", "onError");
            }

            @Override
            public void onNext(Integer integer) {
                Log.i("wangshu", "onNext");
            }
});


```



`throttleFirst()`


```

Observable.create(new Observable.OnSubscribe<Integer>() {
            @Override
            public void call(Subscriber<? super Integer> subscriber) {
                  for(int i=0;i<10;i++){
                      subscriber.onNext(i);
                      try {
                          Thread.sleep(100);
                      } catch (InterruptedException e) {
                          e.printStackTrace();
                      }
                  }
                subscriber.onCompleted();
            }
        }).throttleFirst(200, TimeUnit.MILLISECONDS).subscribe(new Action1<Integer>() {
           @Override
           public void call(Integer integer) {
               Log.d(TAG, "throttleFirst:"+integer);
           }
});


```


`throttleWithTimeOut()`

```

Observable.create(new Observable.OnSubscribe<Integer>() {
            @Override
            public void call(Subscriber<? super Integer> subscriber) {
                for (int i = 0; i < 10; i++) {
                    subscriber.onNext(i);
                    int sleep = 100;
                    if (i % 3 == 0) {
                        sleep = 300;
                    }
                    try {
                        Thread.sleep(sleep);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                subscriber.onCompleted();
            }

    }).throttleWithTimeout(200,TimeUnit.MILLISECONDS).subscribe(new Action1<Integer>() {
            @Override
            public void call(Integer integer) {
                Log.d(TAG, "throttleWithTimeOut:"+integer);
            }
});


```

