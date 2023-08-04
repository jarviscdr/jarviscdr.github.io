---
title: Laravel服务容器
date: 2023-08-04 13:58:43
banner_img:
index_img: https://s3.bmp.ovh/imgs/2023/08/04/83562a7c2b4449b2.jpeg
categories:
- PHP
tags:
- Laravel
- 服务容器
---

**在Laravel架构中，服务容器是用于管理类依赖和依赖注入的工具**
其内部主要实现<控制反转>和<依赖注入>

> *控制反转（IoC）* 是将创建对象的控制权给到第三方，在Laravel中就是将创建的权限给了**服务容器**中
>> 正常程序内，我们创建对象是使用 `new Class()` 方式创建的
>> Laravel使用 **IoC** 的设计模式后，创建对象就使用 `$app->make()` 调用容器进行创建

> *依赖注入（DI）* 是在程序运行时动态为对象创建依赖资源
>> 正常情况下，我们在创建对象时会在 `__construct()` 构造函数内创建依赖资源
>> 使用依赖注入后，Laravel的容器在创建对象时会使用反射 `new ReflectionClass()` 获取需要创建类的反射对象
>> 然后使用反射对象获取类需要的参数`getParameters`，处理完参数后调用`newInstanceArgs`实例化类就可以完成对象的创建和依赖的注入了


**Laravel服务容器使用**

1. 简单绑定
```php
// 第一个参数是绑定的标识，也可以使用字符串自定义
// 第二个参数是解析的闭包函数，可以自定义解析时候的一些自定义操作
$this->app->bind(Transistor::class, function (Application $app) {
    return new Transistor($app->make(PodcastParser::class));
});

// 单例绑定，使用此方法绑定每次获取都是获取同一个对象
$this->app->singleton(Transistor::class, function (Application $app) {
    return new Transistor($app->make(PodcastParser::class));
});

// 绑定实例，$service是一个已经实例化后的对象，所以不需要使用解析闭包
$this->app->instance(Transistor::class, $service);
```

2. 上下文绑定
> 根据当前所处的类进行注入时获取不同的类
> 以下示例表示：
> 在`PhotoController`中注入或者是`make(Filesystem::class)`时返回的对象是`Storage::disk('local')`
> 在`VideoController`和`UploadController`中注入或者是`make`时返回的对象是`Storage::disk('s3')`
> 这种方式能很好地解耦代码
```php
$this->app->when(PhotoController::class)
          ->needs(Filesystem::class)
          ->give(function () {
              return Storage::disk('local');
          });

$this->app->when([VideoController::class, UploadController::class])
          ->needs(Filesystem::class)
          ->give(function () {
              return Storage::disk('s3');
          });
```

3. 解析
```php
// 从服务容器中解析出一个类的实例
$transistor = $this->app->make(Transistor::class);

// 此方法可以将数组的参数传入解析类的构造函数中
$transistor = $this->app->makeWith(Transistor::class, ['id' => 1]);

// 使用门面解析
$transistor = Illuminate\Support\Facades\App::make(Transistor::class);

// 使用帮助函数解析
$transistor = app(Transistor::class);
```