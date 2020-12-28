RxBinding 的使用
-------------

# 简介:
> 本文介绍的是 RxBinding 这个框架. 这个库的基础是在 RxJava 上的, 所以也要同时引入 RxJava.

# 目录:
[1.什么是 RxBinding](#1)

[2.为什么要用 RxBinding](#2)

[3.RxBinding 的简单使用](#3)


# <span id = "1">**1.什么是 RxBinding**</span>

### 介绍:

&ensp;&ensp; [RxBinding 的 GitHub 地址](https://github.com/JakeWharton/RxBinding "RxBinding 的 GitHub 地址")

> RxBinding 能够把 Android 平台的兼容包内的 UI 控件变为 Observaber 对象. 可以把 UI 控件的事件当作 RxJava 中的数据流来使用.


# <span id = "2">**2.为什么要用 RxBinding**</span>

### RxBinding 的优点:

- 它是对 Android View 事件的扩展, 它使得开发者可以对 View 事件使用 RxJava 的各种操作.
- 提供了与 Rxjava 一致的回调, 使得代码简洁明了.
- 几乎支持所有的常用控件及事件, 还对 Kotlin 支持.

### 使用场景
- 可以应用于整个 App 的所有 UI 事件.

# <span id = "3">**3.RxBinding 的简单使用**</span>



`点击事件`

```

RxView.clicks(text1)
                .subscribe(new Consumer<Object>() {
                    @Override
                    public void accept(@NonNull Object o) throws Exception {

                        Toast.makeText(MainActivity.this,"演示点击事件",Toast.LENGTH_SHORT).show();
                    }
});


```



`长点击事件`

```

RxView.longClicks(text2)
                .subscribe(new Consumer<Object>() {
                    @Override
                    public void accept(@NonNull Object o) throws Exception {

                        Toast.makeText(MainActivity.this,"演示长点击事件",Toast.LENGTH_SHORT).show();
                    }
});


```


`防止重复点击`

```

RxView.clicks(text3)
                .compose(RxUtils.useRxViewTransformer(MainActivity.this))
                .subscribe(new Consumer<Object>() {
                    @Override
                    public void accept(@NonNull Object o) throws Exception {

                        Toast.makeText(MainActivity.this,"防止重复点击",Toast.LENGTH_SHORT).show();
                    }
});


```



`表单验证`

```

Observable<CharSequence> ObservablePhone = RxTextView.textChanges(phone);
        Observable<CharSequence> ObservablePassword = RxTextView.textChanges(password);

        Observable.combineLatest(ObservablePhone, ObservablePassword, new BiFunction<CharSequence,CharSequence,ValidationResult>() {

            @Override
            public ValidationResult apply(@NonNull CharSequence o1, @NonNull CharSequence o2) throws Exception {

                if (o1.length()>0 || o2.length()>0) {

                    login.setBackgroundDrawable(getResources().getDrawable(R.drawable.shape_login_pressed));
                } else {

                    login.setBackgroundDrawable(getResources().getDrawable(R.drawable.shape_login_normal));
                }

                ValidationResult result = new ValidationResult();

                if (o1.length()==0) {

                    result.flag = false;
                    result.message = "手机号码不能为空";
                } else if (o1.length()!=11) {

                    result.flag = false;
                    result.message = "手机号码需要11位";
                } else if (o1 !=null && !AppUtils.isPhoneNumber(o1.toString())) {

                    result.flag = false;
                    result.message = "手机号码需要数字";
                } else if(o2.length()==0) {

                    result.flag = false;
                    result.message = "密码不能为空";
                }

                return result;
            }
        }).subscribe(new Consumer<ValidationResult>() {

            @Override
            public void accept(@NonNull ValidationResult r) throws Exception {

                result = r;
            }
        });

        RxView.clicks(login)
                .compose(RxUtils.useRxViewTransformer(TestEditTextActivity.this))
                .subscribeOn(AndroidSchedulers.mainThread())
                .subscribe(new Consumer<Object>() {

                    @Override
                    public void accept(@NonNull Object o) throws Exception {

                        if (result==null) return;

                        if (result.flag) {

                            Toast.makeText(TestEditTextActivity.this,"模拟登录成功",Toast.LENGTH_SHORT).show();
                        } else if (Preconditions.isNotBlank(result.message)) {

                            Toast.makeText(TestEditTextActivity.this,result.message,Toast.LENGTH_SHORT).show();
                        }
                    }
});


```

