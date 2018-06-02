---
title: Retrofit2
toc: true
date: 2016-1-05 14:16:00
categories:
- Android
tags:
- Retrofit2
- 第三方开源库
- 网络请求
description: Retrofit2 是 Square 公司推出的类型安全的 Http 客户端，主要用于 Android 和 Java。Retrofit 可以利用接口、方法和注解参数（parameter annotations）来声明式定义一个请求应该如何被创建，将网络请求变成方法的调用，使用起来非常简洁方便。
---

## Retrofit2 简介
Retrofit2 是 Square 公司推出的类型安全的 Http 客户端，主要用于 Android 和 Java。Retrofit 可以利用接口、方法和注解参数（parameter annotations）来声明式定义一个请求应该如何被创建，将网络请求变成方法的调用，使用起来非常简洁方便。 Retrofit 要求用在 Java 7 or Android 2.3 以上版本上，先简单了解下 Retrofit 的使用步骤：

### 把 HTTP API 变成 Java 的接口
```java
public interface GitHubService {
  @GET("users/{user}/repos")
  Call<List<Repo>> listRepos(@Path("user") String user);
}
```
在 GithubService 接口中有一个方法 listRepos，这个方法用了 @GET 的方式注解，表明这是一个 GET 请求。在后面的括号中的 users/{user}/repos 是请求的路径，其中的 {user} 表示的是这一部分是动态变化的，它的值由方法的参数传递过来，而这个方法的参数`@Path("user") String user`即是用于替换 {user} 。另外这个方法的返回值是 `Call<List<Repo>>`，可以看出 Retrofit 用注解的方式来描述一个 Http 网络请求相关的数据，并且根据这个接口创建请求对象。

### 发出这个网络请求
```java
//使用构建者模式创建 Retrofit 对象
Retrofit retrofit = new Retrofit.Builder()
        .baseUrl("https://api.github.com/")
        .addConverterFactory(GsonConverterFactory.create())
        .build();

GitHubService service = retrofit.create(GitHubService.class);
Call<List<Repo>> repos = service.listRepos("octocat");
repos.enqueue(new Callback<List<Repo>>() {
    @Override
    public void onResponse(Call<List<Repo>> call, Response<List<Repo>> response) {
            
    }
    @Override
    public void onFailure(Call<List<Repo>> call, Throwable t) {

    }
});
```
a.先是构建了一个 Retrofit 对象，其中传入了 baseUrl 参数，baseUrl 和上面的 GET 方法后面的路径组合起来才是一个完整的 url。除了 baseUrl，还有一个 converterFactory，它是用于把返回的 http response 转换成 Java 对象，对应方法的返回值 Call<List<Repo>> 中的 List<Repo>>，其中 Repo 是自定义的类。
b.接着调用它的 create 方法创建了 GitHubService 的实例，然后就可以调用这个实例的方法来请求网络了。调用 listRepo 方法得到一个 Call 对象，然后可以使用 enqueue 或者 execute 来执行发起请求，enqueue 是异步执行，而 execute 是同步执行。

## Retrofit2 引入
在 Module 的 build.gradle 文件中添加 retrofit 相关依赖

```java
dependencies {
    ....
    // 网络访问工具
    compile 'com.squareup.retrofit2:retrofit:2.1.0'
    compile 'com.google.code.gson:gson:2.2.4'
    compile 'com.squareup.retrofit2:converter-gson:2.1.0'
}
```

- `compile 'com.squareup.retrofit2:retrofit:2.1.0'` 添加 Retrofit 依赖
- `compile 'com.google.code.gson:gson:2.2.4'` 添加解析工具的依赖
- `compile 'com.squareup.retrofit2:converter-gson:2.1.0'` 添加Retrofit和解析工具关联的依赖

## Retrofit2 使用

###  **使用步骤**

1)  创建Retorfit.Builder对象，通过Builder指定基本配置信息。 

Retrofit.Builder builder = new Retrofit.Builder();
builder.baseUrl("http://localhost:8080/");
builder.addConverterFactory(GsonConverterFactory.create());

2)  通过Builder构建Retorfit对象

Retrofit retrofit = builder.build();

3)  配置链接和参数

public interface ResponseInfoAPI {
  @GET("HelloDemo/login")
  Call<ResponseInfo> login(@Query("username") String username,@Query("password") String password);
}

- 注：ResponseInfo是服务器回复数据封装成的对象。

测试链接 : [http://localhost:8080/HelloDemo/login?username="XiaoMing"&password="123456](http://localhost:8080/HelloDemo/login?username="XiaoMing"&password="123456)"

4) 完整链接组合

ResponseInfoAPI api = retrofit.create(ResponseInfoAPI.class);

5) 执行联网操作

Call<ResponseInfo> call =api.login(“Hello”,”123456”);
call.enqueue(new Callback<ResponseInfo>() {
​	@Override
​	public void onResponse(Response<ResponseInfo> response, Retrofit retrofit) {
​		// 结果处理
​	}
​	@Override
​	public void onFailure(Throwable throwable) {
​		// 异常处理
​	}
});

请求方法:@GET / @POST 

URL处理 : 测试链接[http://localhost:8080/HelloDemo/login?username="XiaoMing"&password="123456](http://localhost:8080/TakeoutService/login?username="XiaoMing"&password="123456)"

 

### **替换原则** 

1、@Path - 替换参数

@GET("/group/{id}/users")

public Call<List<User>> groupList(@Path("id") int groupId);

2、@Query - 添加查询参数

@GET("/group/{id}/users")

public Call<List<User>> groupList(@Path("id") int groupId, @Query("sort") String sort);

3、@QueryMap - 如果有多个查询参数，把它们放在Map中

@GET("/group/{id}/users")

public Call<List<User>> groupList(@Path("id") int groupId, @QueryMap Map<String, String> options);

## 拓展和参考资料
- [Say hello to retrofit](http://blog.csdn.net/ghost_programmer/article/-details/52372065)
- [retrofit github](https://github.com/square/retrofit) 
- [retrofit 官网](http://square.github.io/retrofit/)
- [Android Retrofit源码解析](https://segmentfault.com/a/1190000006767113)
