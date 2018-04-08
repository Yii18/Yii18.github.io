---
layout: blog
banana: true
category: php
title:  "laravel-IOC和DI"
date:   2018-01-13 20:06:42
background-image: https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=947933790,1726602414&fm=27&gp=0.jpg
tags:
- mvc
- php
- Laravel Ioc
- laravel
---

## laravel IoC容器
a. 首先解释容器 ： 容器就是存放东西的容器，常见的变量和对象都可以说是容器，一个容器能装什么东西呢，全部取决于你对容器的定义，当然还有这一种容器里面存放的也不是文本和数组，而是对象（接口，类）或者是对象的回调通过这种容器我们可以实现很多的东西就是我们经常说道的懈藕，依赖注入（DI）

b. [laravel学院参考网址](http://laravelacademy.org/post/769.html "laravel学院参考网址")

c. IoC容器是laravel 的核心，根据文档称其为服务容器，顾名思义该容器提供了整个框架中需要的一系列服务，
d. IOC（控制反转）和 DI （依赖注入）通过理解这

## laravel服务提供者
          i. 服务提供者是所有Laravel应用启动的中心，你自己的应用以及所有Laravel的核心服务都是通过服务提供者启动。
          但是，我们所谓的”启动“指的是什么？通常，这意味着注册事物，包括注册服务容器绑定、时间监听器、中间件甚至路由。服务提供者是应用配置的中心。
ii. [参考网址 ： 架构 —— 服务容器](http://laravelacademy.org/post/93.html "参考网址 ： 架构 —— 服务容器")
iii. [参考网址 ：服务器提供者编写 ：](http://laravelacademy.org/post/91.html "参考网址 ：服务器提供者编写 ：")
          iv. 如果你打开Laravel自带的config/app.php文件，将会看到一个providers数组，这里就是应用所要加载的所有服务提供者类，当然，其中很多是延迟加载的，也就是说不是每次请求都会被加载，只有真的用到它们的时候才会加载。
          v. 编写一个服务提供者
              1. 所有的服务提供者继承自Illuminate\Support\ServiceProvider类。继承该抽象类要求至少在服务提供者中定义一个方法：register。在register方法内，你唯一要做的事情就是绑事物到服务容器，不要尝试在其中注册任何时间监听器，路由或者任何其它功能。
			  通过Artisan命令make:provider可以简单生成一个新的提供者：
              2. php artisan make:provider RiakServiceProvider
                  a. register方法绑定事物到服务容器
              3. Laravel编译并保存所有延迟服务提供者提供的服务及服务提供者的类名。然后，只有当你尝试解析其中某个服务时Laravel才会加载其服务提供者。