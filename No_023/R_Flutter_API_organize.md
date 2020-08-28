# How I Organize API Files in my Flutter Project

### Links

[How I Organize API Files in my Flutter Project](https://levelup.gitconnected.com/how-i-organize-api-files-in-my-flutter-project-8f21c17050df)

### Notes

#### 问题

- API层必须给UI层提供合适的抽象
- API层必须能把所有的API的Response转成dart对象，让UI层可以直接使用
- API必须可以自己管理session，让UI可以不用管理

#### 方案

作者给出了一个结构方案。

**Internet---> AuthApiClient ---> ApiClient ---> ApiController ---> UI Layer**

AuthApiClient主要负责注册登录等跟用户权限相关的内容

APIClient负责其他正常的业务

APIController 负责APIClient和UI Layer中的逻辑，具体负责创建请求，切换使用的APIClient。

#### 目录组织

```
├── api(dir)  
│    ├── clients(dir) //继承api_client，封装上层client请求方法。
│    ├── request(dir) //封装request方法，包括获取jsonbody等。
│    ├── response(dir) //封装各种response的model，里面定义字段，及json的序列化
│    ├── api.dart //最终的api的工厂方法
│    ├── api_client.dart   // 包括封装dio，组建请求头，拼接请求连接，实现get，post等方法，将异常抛出。
│    ├── api_error.dart    // api返回的错误封装
│    └── api_response.dart // Base response class                       
└── controllers  
    ├── general_controller.dart //负责发送业务请求，例如数据请求等。
    └── session_controller.dart //负责发送登录注册相关请求，例如登录请求等。
```

放两个图可能更好理解一点

![Image for post](https://miro.medium.com/max/744/1*2-HRwe-42ufu8xZ4AshL0w.png)

![Image for post](https://miro.medium.com/max/744/1*2-HRwe-42ufu8xZ4AshL0w.png)