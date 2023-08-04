---
title: Laravel生命周期
date: 2023-08-03 13:47:43
banner_img:
index_img: https://s3.bmp.ovh/imgs/2023/08/04/83562a7c2b4449b2.jpeg
categories:
- PHP
tags:
- Laravel
- 生命周期
---

# Laravel生命周期

![Laravel生命周期](https://s3.bmp.ovh/imgs/2023/08/03/ada43556c8d0cd07.jpg)

> **大概总结的生命周期如下：**
> 1. 入口文件public/index.php
> 2. 加载composer生成的vendor/autoload.php
> 3. 创建服务容器bootstrap/app.php
> 4. 加载Http核心，由于创建容器时完成了内核绑定，所以此处加载的核心是App\Http\Kernel
> 5. 使用Kernel处理请求，此处将返回响应response
> 6. 响应发送，设置header、响应报文body等
> 7. 请求处理结束后续流程，中间件或事件的分发等

## 入口文件
> *文件路径：public/index.php*
>
> 入口文件概括起来就是执行了3个操作
> 1. 加载项目依赖
> 2. 创建 Laravel 服务容器
> 3. 接收请求并响应

```php
// 记录Laravel的启动时间
define('LARAVEL_START', microtime(true));
// 加载项目依赖
require __DIR__.'/../vendor/autoload.php';
// 创建服务容器
$app = require_once __DIR__.'/../bootstrap/app.php';
// 创建Http核心
$kernel = $app->make(Illuminate\Contracts\Http\Kernel::class);
// 使用Http核心处理请求
$response = $kernel->handle(
    // 创建请求
    $request = Illuminate\Http\Request::capture()
);
// 请求处理完成后响应内容给客户端
$response->send();
// 终止程序
$kernel->terminate($request, $response);
```


## 引导文件
> *文件路径：boostrap/app.php*
> 此文件主要就是创建了服务容器，并将内核以及异常处理器绑定到容器内

```php
// 创建服务容器
$app = new Illuminate\Foundation\Application(
    realpath(__DIR__.'/../')
);
/**
 * 绑定HTTP内核
 * 由于此处将App\Http\Kernel::class
 * 绑定到Illuminate\Contracts\Http\Kernel::class
 * 所以后续使用容器make(Illuminate\Contracts\Http\Kernel::class)的时候
 * 实际上实例化的是App\Http\Kernel::class
 */
$app->singleton(
    Illuminate\Contracts\Http\Kernel::class,
    App\Http\Kernel::class
);
// 绑定Console内核
$app->singleton(
    Illuminate\Contracts\Console\Kernel::class,
    App\Console\Kernel::class
);
// 绑定异常处理器
$app->singleton(
    Illuminate\Contracts\Debug\ExceptionHandler::class,
    App\Exceptions\Handler::class
);

return $app;
```

### 创建服务容器
> 在引导文件内 `new Illuminate\Foundation\Application` 创建容器
> 在构造函数内执行：
> 1. 绑定应用的基础路径
> 2. 注册基础的服务提供者绑定到容器内
> 3. 注册核心容器别名等绑定到容器内

### 内核绑定
> 在引导文件内使用 `$app->singleton()` 绑定内核
> 由于此时只是绑定了自定义的内核类的class
> 所以在使用 `$app->make()` 获取内核的时候才会首次进行实例化


## 请求和响应
> 当前环节的关键代码是
> 获取Http内核
> 获取当前请求对象并使用内核处理
> 内核处理完成后返回了响应对象
> 使用响应对象返回数据给客户端

### 创建内核实例
> 内核构造函数会将容器和路由绑定到当前内核中
> 并且将中间件的分组和别名绑定到路由中
> 中间件分组主要是方便api或web等多组路由绑定基础中间件的操作

### 处理请求
> 在使用内核处理请求前，会使用 `Illuminate\Http\Request::capture()` 获取请求对象
> 创建请求时会将此次请求的query、post、files、cookies等数据绑定到请求对象中
> 然后调用内核的 `handle` 方法进行处理，此方法会执行以下流程：
> 1. 将请求对象注册到容器内，这样后续在控制器内也可以拿到当前的请求对象
> 2. 清除之前请求对象的缓存
> 3. 启动引导，这里主要是将内核中绑定的一些引导服务等执行
> 4. 将请求发送到路由进行匹配以及处理
> 5. 最终会返回响应对象

### 返回响应
> 在入口文件中，内核处理完请求后返回了响应对象
> 调用 `$response->send();` 时将响应的header和报文内容发送到客户端
> 在这一步的时候请求和响应的处理就结束了


## 程序结束
> 在入口文件的最后
> 会调用 `$kernel->terminate($request, $response);` 来执行程序结束前的操作
> 这里主要是处理中间件的后续操作
> 因为中间件使用的是洋葱模型，处理请求时执行的是中间件的前半部分
> 此时返回了响应后，需要执行中间件的后半部分

***完***

