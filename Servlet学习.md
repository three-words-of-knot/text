# Servlet简单描述

## Servlet是什么？

- Servlet 在功能上主要是用来接收客户端传来的请求并且生成对应的响应。是一个运行在服务器上的特殊的类。Servlet的主要工作流程如下：
  - 服务器根据 ```web.xml``` 文件中的配置，把请求交给指定的Servlet
  - Servlet 调用其他Java类的方法，完成对于请求信息的逻辑处理
  - Servlet 实现到其他Web组件的跳转
  - Servlet 生成响应

- Servlet 的接口主要包括
  - ```void init(ServletConfig config)``` 服务器在创建好Servlet对象之后，会调用该方法来初始化此对象，传输的参数是服务器向Servlet传递配置信息的具体方式
  - ```void service(ServletRequest req,ServletResponse res)```

## 为什么要使用Servlet？

## 怎么使用Servlet
