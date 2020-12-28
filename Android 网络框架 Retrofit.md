Android 网络框架 Retrofit 基本使用
------------
# 简介
> 本篇文章介绍的是 Android 网络框架 Retrofit 的基本使用.

# 目录
[1.Retrofit 介绍](#1)
[2.Retrofit 的基本使用](#2)
[3.Retrofit 的注解分类](#3)
[4.Retrofit 注解的使用](#4)
[5.Retrofit 多种解析器](#5)

# <span id = "1">**1.Retrofit 介绍**</span>
这个库已经火了很久了, 不过到现在都没实际运用过. 实在是感觉自己太 out 了. 这个库是 square 公司出的 一个 Android 网络请求框架. 底层也是用 okHttp 实现的. **Retrofit 最大的特点就是使用注解的方式提供功能** (square 公司真是强无敌, okHttp 也是他们做的) [Retrofit GitHub地址](https://github.com/square/retrofit)
> 推荐博客 [https://blog.csdn.net/carson_ho/article/details/73732076](https://blog.csdn.net/carson_ho/article/details/73732076 "这是一份很详细的 Retrofit 2.0 使用教程（含实例讲解）")

### 为什么要用 Retrofit:
我认为的优点:
- 简洁易用
- 使用注解的方式来动态的配置网络请求
- 支持同步异步请求
- 可以选择不同的解析器来解析数据
- 提供对 RXJava 的支持

# <span id = "2">2.Retrofit 的基本使用</span>
### 第一步:添加依赖
`build.gradle`
```
	//retrofit 的依赖
	compile 'com.squareup.retrofit2:retrofit:2.1.0'
	//支持 Gson 的解析器的依赖
    compile 'com.squareup.retrofit2:converter-gson:2.1.0'
```

### 第二步:添加网络权限
`AndroidManifest.xml`
```
<uses-permission android:name="android.permission.INTERNET"/>
```

### 第三步:创建用于存放数据的实体类
`DataBean.class`
```
public class DataBean {

	...

	}
```

### 第四步:创建 Retrofit 用于请求网络的接口
`IpService`
```
public interface IpService {
	//使用 Get 请求方式
    @GET("getIpInfo.php?ip=59.108.54.37")
    Call<DataBean> getIpMsg();
}
```
> @Get 注解代表请求方法, 这里为使用 @Get 方法来请求网络, 里面的字符串代表 完整 Url 的后半段.
> getIpMsg() 用来返回一个 Call<T> 类型的参数. 我们定义的我们定义的这个方法一般用请求参数注解来动态的拼接 Url 这里没有使用到动态的方法.

### 第五步:创建 Retrofit 请求网络

```

	/**
     * 普通GET请求
     */
    private void getIpInformation() {
        String url = "http://ip.taobao.com/service/";
		//创建 Retrofit 的 Builder 对象
        Retrofit retrofit = new Retrofit.Builder()
                .baseUrl(url)//设置 Url 的前部分
                .addConverterFactory(GsonConverterFactory.create())//设置 Gson 解析器
                .build();//创建对象
		//使用 Retrofit 动态代理获取到之前定义的接口
        IpService ipService = retrofit.create(IpService.class);
		//调用定义的接口里面的方法
        Call<DataBean> call = ipService.getIpMsg();
		//设置回调
        call.enqueue(new Callback<DataBean>() {
            @Override
            public void onResponse(Call<DataBean> call, Response<DataBean> response) {
                String country = response.body().getData().getCountry();
                Toast.makeText(getApplicationContext(), country, Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onFailure(Call<IpModel> call, Throwable t) {

            }
        });

```
上面的注释写的很清楚了, 不赘述.
# <span id = "3">3.Retrofit 的注解分类</span>
![Retrofit 注解的分类](http://upload-images.jianshu.io/upload_images/944365-ee747d1e331ed5a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

图片来自:[这是一份很详细的 Retrofit 2.0 使用教程（含实例讲解）](图片来自:https://blog.csdn.net/carson_ho/article/details/73732076)

>这些注解都是用在我们为 Retrofit 定义的接口里面, 一般的方式为(不是所有都是这个形式):

```
public interface IpService {
	@标记类
    @请求方式注解("getIpInfo.php?ip=59.108.54.37")
    Call<DataBean> getIpMsg(@网络请求参数("参数名") String path);
}
```


>可以看到图片分为了 3 类, 接下来一个一个介绍.

# <span id = "4">4.Retrofit 注解的使用</span>
### 请求方法注解:

在 Retrofit 中的 Url 是由三部分组成的, 一部分是在 创建对象的时候传进来的 `baseUrl`
的 Url 加上请求网络接口的 例如: @GET("getIpInfo.php?ip=59.108.54.37")的部分, 再加上 Call 方法传进来的 参数注解才是一个完整的 Url.

Http 的请求方法注解有 8 种 :

- @GET
- @POST
- @PUT
- @DELETE
- @HEAD
- @PATCH
- @OPTIONS
- @HTTP

前 七 个我们的使用:
>```
>public interface IpService {
>
>    @请求方法注解("getIpInfo.php?ip=59.108.54.37")
>    Call<DataBean> getIpMsg();
>}
>```


第 八 种 Http 让人摸不着头脑, 其实 Http 就是替换上面的其中的一个通过属性method、path、hasBody进行设置
> ```
public interface GetRequest_Interface { /**
     * method：网络请求的方法（区分大小写）
     * path：网络请求地址路径
     * hasBody：是否有请求体
     */ 
@HTTP(method = "GET", path = "blog/{id}", hasBody = false) 
Call<ResponseBody> getCall(@Path("id") int id); 
// {id} 表示是一个变量 
// method 的值 retrofit 不会做处理，所以要自行保证准确 }
> ```

### 请求参数注解
- @Header

动态的添加消息头

```
public interface IpServiceForPath {
// @Header
@GET("user")
Call<DataBean> getUser(@Header("Authorization") String authorization)
}
```

- @Headers

静态的添加消息头

```
public interface IpServiceForPath {
// @Headers
@Headers("Authorization: authorization")
@GET("user")
Call<DataBean> getUser()
}
```

- @Body

用 POST 方式将自定义的对象转成 JSON 字符串发送到服务器

```
public interface IpServiceForPath {
    @GET("getIpInfo.php")
    Call<DataBean> getIpMsg(@Body Ip ip);
}
```
```
public class Ip {
    private String ip;
    public Ip(String ip) {
        this.ip = ip;
    }
}
```

- @Path

在请求方法注解中包含了 {替换名}, 它对应着 @Path 注解中的 "替换名", 用传入的 String path 参数来替换注解方法地址中的 {替换名} 

```
public interface IpServiceForPath {
    @GET("{path}/getIpInfo.php?ip=59.108.54.37")
    Call<DataBean> getIpMsg(@Path("path") String path);
}
```

- @Field

在 POST 中请求数据类型为键值对

```
public interface IpServiceForPost {
    @FormUrlEncoded
    @POST("getIpInfo.php")
    Call<DataBean> getIpMsg(@Field("ip") String first);
}
```

- @FieldMap

在 POST 中请求数据类型为键值对以 Map 类型传入

```
public interface IpServiceForPost {
    @FormUrlEncoded
    @POST("getIpInfo.php")
    Call<DataBean> getIpMsg(@FieldMap Map<String, String>);
}
```

- @Part

单个文件上传

```
public interface IpServiceForPost {
    @Multipart
    @POST("getIpInfo.php")
    Call<DataBean> getIpMsg(@Part MultipartBody.Part photo, @Part("description") RequestBody description);
}
```

- @PartMap

多个文件上传

```
public interface IpServiceForPost {
    @Multipart
    @POST("getIpInfo.php")
    Call<IpModel> getIpMsg(@PartMap Map<MultipartBody.Part, RequestBody>);
}
```

- @Query

在 GET 请求方式中动态的指定查询条件

```
public interface IpServiceForQuery{
    @GET("getIpInfo.php")
    Call<DataBean> getIpMsg(@Query("ip")String ip);
}
```

- @QueryMap

在 GET 动态的指定查询的条件组.就是以 Map 的方式传进来

```
public interface IpServiceForQuery{
    @GET("getIpInfo.php")
    Call<DataBean> getIpMsg(@Query("ip")String ip);
}
```

### 标记类注解:
- @FormUrlEncoded
> 表示请求体是一个 Form 表单.

- @Multipart
> 表示请求体是一个支持文件上传的表单.

- Streaming
> 表示返回的数据全部用流的形式返回, 如果不使用它, 默认会把全部数据加载到内存, 所以下载大文件时需要加上这个注解.

# <span id = "5">5.Retrofit 多种解析器</span>
还记得基本使用中的我们添加的解析器依赖吗? Retrofit 提供了多中解析器给我们选择.
- Gson 	`com.squareup.retrofit2:converter-gson:2.0.2`
- Jackson 	`com.squareup.retrofit2:converter-jackson:2.0.2`
- Simple XML 	`com.squareup.retrofit2:converter-simplexml:2.0.2`
- Protobuf 	`com.squareup.retrofit2:converter-protobuf:2.0.2`
- Moshi 	`com.squareup.retrofit2:converter-moshi:2.0.2`
- Wire 	`com.squareup.retrofit2:converter-wire:2.0.2`
- Scalars 	`com.squareup.retrofit2:converter-scalars:2.0.2`

> 然后只要在 Build 的时候 `.addConverterFactory(GsonConverterFactory.create())` 就可以啦