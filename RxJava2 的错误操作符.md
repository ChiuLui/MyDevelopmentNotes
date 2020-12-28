RxJava2 的错误操作符
-------------

# 内容简介:
> 本文章的内容是简单讲解和使用 RxJava2 中的错误处理操作符.

# 目录:
[1.错误处理操作符的使用场景](#1)

[2.错误处理操作符的分类](#2)

[3.简单使用](#3)


# <span id = "1">**1.错误处理操作符的使用场景**</span>

### 使用场景:

> &ensp;&ensp; RxJava 操作符**拦截**原始的 `Observable` 的 `onEror` 通知, 将它替换为其他 数据或数据序列, 让产生的 Observable 能够正常的终止或者根本不终止.

# <span id = "2">**2.错误处理操作符的分类**</span>

### catch(捕获) 操作符:

&ensp;&ensp;catch 操作符拦截原始 Observable 的 onError 通知, 将它替换为其他数据项或数据序列, 让产生的 Observable 能够正常终止或者根本不终止. RxJava 将 catch 实现为以下三个不同的操作符.

- onErrorReturn
> Observable 遇到错误时返回原有 Observable 行为的备用 Observable, 备用 Observable 会忽略原有 Observable 的 onError 调用, 不会将错误传递给观察者. 作为替代, 它会发射一个特殊的项并调用观察者的 onCompleted.

- onErrorResumeNext
> Observable 遇到错误时返回原有 Observable 行为的备用 Observable, 备用 Observable 会忽略原有 Observable 的 onError 调用, 不会将错误传递给观察者. 作为替代, 它会发射备用 Observable 的数据.

- onExceptionResumeNext
> 它和 onErrorResumeNext 类似. 不同的是, 如果 onError 收到的 Throwable 不是一个 Exception, 它会将错误传递给观察者的 onError 方法, 不会使用备用的 Observable.

### retry(重试) 操作符:

- retry
> retry 操作符不会将原始 Observable 的 onError 通知传递给观察者, 它会订阅这个 Observable, 再给它一次机会无错误地完成其数据序列. retry 总是传递 onNext 通知给观察者, 由于重新订阅, 这可能会造成数据项重复. RxJava 中的实现为 retry 和 retryWhen. 这里拿 retry 来举例, 它指定最多重新订阅的次数. 如果次数超过了, 它不会尝试再次订阅, 而会把最新的一个 onError 通知传递给自己的观察者.


# <span id = "3">**3.简单使用**</span>

### catch 简单实现

`onErrorReturn()`
```

Observable.create(new Observable.OnSubscribe<Integer>() {
            @Override
            public void call(Subscriber<? super Integer> subscriber) {
                for (int i = 0; i < 5; i++) {
                    if (i > 2) {
                        subscriber.onError(new Throwable("Throwable"));
                    }
                    subscriber.onNext(i);
                }
                subscriber.onCompleted();
            }
        }).onErrorReturn(new Func1<Throwable, Integer>() {
            @Override
            public Integer call(Throwable throwable) {
                return 6;
            }
        }).subscribe(new Observer<Integer>() {
            @Override
            public void onCompleted() {
                Log.d(TAG, "onCompleted");
            }
            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "onError:" + e.getMessage());
            }
            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "onNext:" + integer);
            }
        });
}


```


### retry 简单实现


`retry()`
```


Observable.create(new Observable.OnSubscribe<Integer>() {
            @Override
            public void call(Subscriber<? super Integer> subscriber) {
                for (int i = 0; i < 5; i++) {
                    if (i==1) {
                        subscriber.onError(new Throwable("Throwable"));
                    }else {
                        subscriber.onNext(i);
                    }
                }
                subscriber.onCompleted();
            }
        }).retry(2).subscribe(new Observer<Integer>() {
            @Override
            public void onCompleted() {
                Log.d(TAG, "onCompleted");
            }

            @Override
            public void onError(Throwable e) {
                Log.d(TAG, "onError:" + e.getMessage());
            }

            @Override
            public void onNext(Integer integer) {
                Log.d(TAG, "onNext:" + integer);
            }
});



```

