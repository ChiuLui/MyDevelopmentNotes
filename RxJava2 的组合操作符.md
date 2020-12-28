RxJava 的组合操作符
-------------

# 简介:
> 本篇文章是简单介绍 RxJava 的常用`组合操作符`. 只介绍使用层面, 不涉及原理.

# 目录:
[1.组合操作符的使用场景](#1)

[2.组合操作符有哪些?](#2)

[3.常用组合操作符的简单使用](#3)


# <span id = "1">**1.组合操作符的使用场景**</span>

### 使用场景:

&ensp;&ensp; 简单的说 RxJava 的`组合操作符`就像一个**包裹**, 把多个 `Observable` 打包, 一起发送给`订阅者`.
> 在 RxJava 中的组合操作符用于同时处理多个 Observable 来创建我们所需要的 Observable.


# <span id = "2">**2.组合操作符有哪些?**</span>

### 组合操作符汇总:

|操作符|描述|
|:------:|:------|
| and() , then() , when() | 通过模式(And条件)和计划(Then次序)组合两个或多个Observable发射的数据集 |
| combineLatest() |  	当两个Observables中的任何一个发射了一个数据时，通过一个指定的函数组合每个Observable发射的最新数据（一共两个数据），然后发射这个函数的结果 |
| join() , groupJoin() | 无论何时，如果一个Observable发射了一个数据项，只要在另一个Observable发射的数据项定义的时间窗口内，就将两个Observable发射的数据合并发射 |
| merge() | 将多个Observable合并为一个 |
| startWith() | 在数据序列的开头增加一项数据，在发射原来的Observable的数据序列之前，先发射一个指定的数据序列或数据项 |
| switch() | 将一个发射Observable序列的Observable转换为这样一个Observable：它逐个发射那些Observable最近发射的数据 |
| switchOnNext() | 将一个发射Observables的Observable转换成另一个Observable，后者发射这些Observables最近发射的数据 |
| zip() | 打包，使用一个指定的函数将多个Observable发射的数据组合在一起，然后将这个函数的结果作为单项数据发射 |
| mergeDelayError() | 合并多个Observables，让没有错误的Observable都完成后再发射错误通知 |


# <span id = "3">**3.常用组合操作符的简单使用**</span>



`combineLastest()`

```

    private void combineLastest() {
        Observable<Integer> obs1 =Observable.just(1,2,3);
        Observable<String> obs2 =Observable.just("a","b","c");
        Observable.combineLatest(obs1, obs2, new Func2<Integer, String, String>() {
            @Override
            public String call(Integer integer, String s) {
                return integer+s;
            }
        }).subscribe(new Action1<String>() {
            @Override
            public void call(String s) {
                Log.d(TAG, "combineLastest:"+s);
            }
        });

}


```


`zip()`

```

    private void zip() {
        Observable<Integer> obs1 =Observable.just(1,2,3);
        Observable<String> obs2 =Observable.just("a","b","c");
        Observable.zip(obs1, obs2, new Func2<Integer, String, String>() {

            @Override
            public String call(Integer integer, String s) {
                return integer+s;
            }
        }).subscribe(new Action1<String>() {
            @Override
            public void call(String s) {
                Log.d(TAG, "zip:"+s);
            }
        });
}


```


`concat()`

```

    private void concat() {
        Observable<Integer> obs1 =Observable.just(1,2,3).subscribeOn(Schedulers.io());
        Observable<Integer> obs2 =Observable.just(4,5,6);
        Observable.concat(obs1,obs2).subscribe(new Action1<Integer>() {
            @Override
            public void call(Integer integer) {
                Log.d(TAG, "concat:"+integer);
            }
        });
}


```


`merge()`

```

    private void merge() {
        Observable<Integer> obs1 =Observable.just(1,2,3).subscribeOn(Schedulers.io());
        Observable<Integer> obs2 =Observable.just(4,5,6);
        Observable.merge(obs1,obs2).subscribe(new Action1<Integer>() {
            @Override
            public void call(Integer integer) {
                Log.d(TAG, "merge:"+integer);
            }
        });
}


```


`startWith()`

```

    private void startWith() {
        Observable.just(3, 4, 5).startWith(1, 2)
                .subscribe(new Action1<Integer>() {
                    @Override
                    public void call(Integer integer) {
                        Log.d(TAG, "startWith:"+integer);
                    }
                });
}


```
