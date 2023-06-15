---
sidebar_label: 02-写第一个api
sidebar_position: 1
title: 02-写第一个api
---

### 学习目标
在上一节，我们初始化了一个helloword项目并成功访问 localhost:8000/helloworld/{name} 接口，这章我们来学习下如何自己写一个接口。

代码仓库地址： https://github.com/mouuii/kratos-tutorial commitid:ae3560f5575ddbe7668d72348cafa5a8b5bf26a5

### 定义api

API 与用户的通信协议，通常是 REST API 和 RPC API 作为传输层协议，而 Kratos 主要参考 Google API 指南，实现了对应通信协议支持，并且遵守了 gRPC API 使用 HTTP 映射功能进行 JSON/HTTP 的支持。

也就是通过定义 proto 即可使用 REST API 和 RPC API，通过类似 Google API 的仓库方式进行 API Schema 的管理。

我们打开 api 目录 ，打开 api/agent/v1/agent.proto 文件。

![](https://raw.githubusercontent.com/mouuii/picture/master/%E6%88%AA%E5%B1%8F2023-06-15%20%E4%B8%8B%E5%8D%886.33.53.png)

```proto
	rpc CreateAgent (CreateAgentRequest) returns (CreateAgentReply){
		option (google.api.http) = {
			get: "/helloworld/{name}"
		};
	};

message CreateAgentRequest {
	string name = 1;
}
message CreateAgentReply {
	string result = 1;
}
```    

上面使用了 proto 来定义 api ，如果你没接触过 proto 语法也没事，我来一一讲解。我们定义了一个 CreateAgent 方法，该方法接收 CreateAgentRequest 结构体，返回 CreateAgentReply 结构体。这个 message 关键字完全可以理解为 golang 中的struct。

我们引用了 `google/api/annotations.proto` 这个 grpc 插件，它能够通过写注释的方式来生成 http 相关的代码，比如帮我们把参数解析到结构体上，帮我们注册路由等。