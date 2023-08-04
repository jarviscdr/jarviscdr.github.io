---
title: Laravel服务提供者和门面
date: 2023-08-04 16:09:43
banner_img:
index_img: https://s3.bmp.ovh/imgs/2023/08/04/83562a7c2b4449b2.jpeg
categories:
- PHP
tags:
- Laravel
- 服务提供者
- 门面
- Facades
---

# 服务提供者

## 创建服务提供者
服务提供者是一种用于注册、维护和注入服务的工具。主要是用来维护应用程序的各种类实例、接口和依赖项，并管理它们的生命周期
简单来说，服务提供者就是为应用程序提供服务的类。
提供的服务可以是任何东西，比如自定义的服务类、数据库连接类、第三方库等等。

所有的服务提供者都会继承 `Illuminate\Support\ServiceProvider` 类。
大多服务提供者都包含一个 `register` 和一个 `boot` 方法。
其中 `register` 方法是用于将服务绑定到服务容器中的，并且不要在 `register` 中注册任何监听器，路由，或者其他任何功能
`boot` 方法则是会在所有服务提供者被注册以后才会被调用

使用 `php artisan make:provider XxxxProvider` 可以快速创建一个服务提供者

## 注册服务提供者
所有服务提供者都是通过配置文件 `config/app.php` 内的 `providers` 数组进行注册。

## 延迟提供者
延迟加载服务，只有在开始使用该服务时再加载，可以提升应用的性能，因为不需要每次请求都加载该服务。
要使用延迟提供者需要实现 `\Illuminate\Contracts\Support\DeferrableProvider` 接口


# 门面(Facades)
**Laravel Facades 充当服务容器中底层类的「静态代理」，主要的作用就是更方便地使用服务容器中的各种类**

Facades的使用就是用 `__callStatic()` 魔术方法动态调用服务类中的各种方法
只要在门面内的 `getFacadeAccessor` 方法内返回在服务容器中绑定的名称即可