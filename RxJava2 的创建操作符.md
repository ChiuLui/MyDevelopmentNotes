RxJava2 的创建操作符
-------------

# 内容简介:
> 本文章的内容是简单讲解和使用 RxJava2 中的创建操作符.

# 目录:
[1.创建操作符的使用场景](#1)

[2.创建操作符的分类](#2)

[3.简单使用](#3)


# <span id = "1">**1.创建操作符的使用场景**</span>

### 使用场景:

> &ensp;&ensp; RxJava 的创建操作符即用于创建 Observable 的操作符。


# <span id = "2">**2.创建操作符的分类**</span>

|操作符|功能|
|:------:|:------:|
|just()|将一个或多个对象转换成发射这个或这些对象的一个Observable|
|fromArray()|将一个Iterable, 一个Future, 或者一个数组转换成一个Observable|
|repeat()|创建一个重复发射指定数据或数据序列的Observable|
|repeatWhen()|创建一个重复发射指定数据或数据序列的Observable，它依赖于另一个Observable发射的数据|
|create()|使用一个函数从头创建一个Observable|
|defer()|只有当订阅者订阅才创建Observable；为每个订阅创建一个新的Observable|
|range()|创建一个发射指定范围的整数序列的Observable|
|interval()|创建一个按照给定的时间间隔发射整数序列的Observable|
|timer()|创建一个在给定的延时之后发射单个数据的Observable|
|empty()|创建一个什么都不做直接通知完成的Observable|
|error()| 	创建一个什么都不做直接通知错误的Observable|
|never()|创建一个入不发射任何数据的Observable|


# <span id = "3">**3.简单使用**</span>


`Create()`

```
Observable.create(new ObservableOnSubscribe<Integer>() {
            @Override
            public void subscribe(ObservableEmitter<Integer> emitter) throws Exception {
                try {
                    if (!emitter.isDisposed()) {
                        for (int i = 0; i < 10; i++) {
                            emitter.onNext(i);
                        }
                        emitter.onComplete();
                    }
                } catch (Exception e){
                    emitter.onError(e);
                }
            }
        }).subscribe(new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) throws Exception {
                Log.e(TAG, "onNext:" + integer);
            }
        }, new Consumer<Throwable>() {
            @Override
            public void accept(Throwable throwable) throws Exception {
                Log.e(TAG, "Error:" + throwable.getMessage());
            }
        }, new Action() {
            @Override
            public void run() throws Exception {
                Log.e(TAG, "onComplete");
            }
        });


```



`just()`

```


Observable.just("HelloRxJava")
                .subscribe(new Consumer<String>() {
                    @Override
                    public void accept(String s) throws Exception {
                        Log.e(TAG, s );
                    }
                });


        Observable.just(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
                .subscribe(new Consumer<Integer>() {
                    @Override
                    public void accept(Integer integer) throws Exception {
                        Log.e(TAG, integer + "");
                    }
                }, new Consumer<Throwable>() {
                    @Override
                    public void accept(Throwable throwable) throws Exception {
                        Log.e(TAG, "accept: ");
                    }
                }, new Action() {
                    @Override
                    public void run() throws Exception {
                        Log.e(TAG, "complete" );
                    }
                });



```


`fromArray()`

```

Observable.fromArray("Hello", "from")
                .subscribe(new Consumer<String>() {
                    @Override
                    public void accept(String s) throws Exception {
                        Log.e(TAG,  s);
                    }
                });


```




`fromIterable()`

```

ArrayList<Integer> items = new ArrayList<>();
        for (int i = 0; i < 10; i++){
            items.add(i);
        }
        Observable.fromIterable(items)
                .subscribe(new Consumer<Integer>() {
                    @Override
                    public void accept(Integer integer) throws Exception {

                    }
                }, new Consumer<Throwable>() {
                    @Override
                    public void accept(Throwable throwable) throws Exception {
                        Log.e(TAG, throwable.getMessage());
                    }
                }, new Action() {
                    @Override
                    public void run() throws Exception {
                        Log.e(TAG, "complete." );
                    }
                });


```

