---
slug: argo-workflow
title: argo工作流
authors: mouuii
tags: [devops]
---
## argo-workflow介绍
Argo Workflows是一个开源项目，为Kubernetes提供container-native工作流程，其主要通过Kubernetes CRD实现的。

特点如下：
- 工作流的每一步都是一个容器
- 将多步骤工作流建模为一系列任务，或者使用有向无环图（DAG）描述任务之间的依赖关系
- 可以在短时间内轻松运行用于机器学习或数据处理的计算密集型作业
- 在Kubernetes上运行CI/CD Pipeline，无需复杂的软件配置

## 安装

### 命令
要安装 Argo Workflows，请导航至发布页面并找到您希望使用的版本（最好是最新的完整版本）。
以下是安装命令的示例，请确保更新命令以安装正确的版本号,安装好后会在
```shell
kubectl create namespace argo
kubectl apply -n argo -f https://github.com/argoproj/argo-workflows/releases/download/v<<ARGO_WORKFLOWS_VERSION>>/install.yaml
```
安装好后我们可以使用 kubectl 命令查看 argo 命名空间下的服务。
![](https://raw.githubusercontent.com/mouuii/picture/master/%E6%88%AA%E5%B1%8F2023-04-25%20%E4%B8%8B%E5%8D%882.19.36.png)

### 补丁 argo-server 认证
argo-server（以及 UI）默认为客户端身份验证，这需要客户端提供其 Kubernetes 承载令牌才能进行身份验证。有关详细信息，请参阅 Argo 服务器身份验证模式文档。我们将身份验证模式切换为 server ，以便我们现在可以绕过 UI 登录：
```shell
kubectl patch deployment \
  argo-server \
  --namespace argo \
  --type='json' \
  -p='[{"op": "replace", "path": "/spec/template/spec/containers/0/args", "value": [
  "server",
  "--auth-mode=server"
]}]'
```

### ui界面端口转发

将ui端口转发到本地
```shell
kubectl -n argo port-forward deployment/argo-server 2746:2746 & open https://localhost:2746
```
我们可以看到argo-workflow的界面啦 ，默认会显示 argo 命名空间下的工作流任务列表。

![](https://raw.githubusercontent.com/mouuii/picture/master/%E6%88%AA%E5%B1%8F2023-04-25%20%E4%B8%8B%E5%8D%882.23.04.png)

## 提交一个最简单的工作流

### 提交工作流
让我们从创建一个非常简单的工作流模板开始，使用来自 Docker Hub 的 docker/whalesay 容器镜像来回应“hello world”

您可以使用简单的 docker 命令直接从 shell 运行它：

```shell
$ docker run docker/whalesay cowsay "hello world"
 _____________
< hello world >
 -------------
    \
     \
      \
                    ##        .
              ## ## ##       ==
           ## ## ## ##      ===
       /""""""""""""""""___/ ===
  ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
       \______ o          __/
        \    \        __/
          \____\______/


Hello from Docker!
This message shows that your installation appears to be working correctly.

```
我们尝试让argo系统帮助我们运行这个容器，点击argo界面 +SUBMIT NEW WORKFLOW 按钮，将下面的yaml文件，提交到argo系统。
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Workflow                  # 自定义类型
metadata:
  generateName: hello-world-   
spec:
  entrypoint: whalesay          # 入口
  templates:
    - name: whalesay              # 模版名字
      container:
        image: docker/whalesay
        command: [ cowsay ]
        args: [ "hello world" ]
        resources: # limit the resources
          limits:
            memory: 32Mi
            cpu: 100m

```

### 查看工作流
我们可以看到界面上多了个绿色的任务，点击右下角的Logs按钮，我们能查看容器中的日志，打印出了我们要的鲸鱼。

![](https://raw.githubusercontent.com/mouuii/picture/master/%E6%88%AA%E5%B1%8F2023-04-25%20%E4%B8%8B%E5%8D%882.32.45.png)
![](https://raw.githubusercontent.com/mouuii/picture/master/%E6%88%AA%E5%B1%8F2023-04-25%20%E4%B8%8B%E5%8D%882.32.52.png)

### 多步骤工作流
如果我们想执行多个工作流，如何做呢？试试提交下下面的yaml文件吧！
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
  generateName: steps-
spec:
  # 程序入口模版名字
  entrypoint: hello-hello-hello

  # 包含两个模版: hello-hello-hello and whalesay
  templates:
  - name: hello-hello-hello
    # 这个模版有好几个步骤，hello1 先执行
    steps:
    - - name: hello1            # hello1 先执行
        template: whalesay
        arguments:
          parameters:
          - name: message
            value: "hello1"
    - - name: hello2a           # 在hello1后执行
        template: whalesay
        arguments:
          parameters:
          - name: message
            value: "hello2a"
      - name: hello2b           # 与hello02a并发执行
        template: whalesay
        arguments:
          parameters:
          - name: message
            value: "hello2b"

  
  - name: whalesay
    inputs:
      parameters:
      - name: message
    container:
      image: docker/whalesay
      command: [cowsay]
      args: ["{{inputs.parameters.message}}"]

```

看图秒懂！我们创建了3个任务，hello2b和hello2a在hello1任务之后并发执行。同时这三个任务接受的参数不同，打印的结果也不同哦！

![](https://raw.githubusercontent.com/mouuii/picture/master/%E6%88%AA%E5%B1%8F2023-04-25%20%E4%B8%8B%E5%8D%882.42.49.png)


## 六大模版
定义具体的模版有6种类别，如下：

### Container
也许是最常见的模板类型，它会调度一个容器。模板的规范与 Kubernetes 容器规范相同，因此您可以像在 Kubernetes 中的其他任何地方一样在这里定义容器。
```yaml
  - name: whalesay
    container:
      image: docker/whalesay
      command: [cowsay]
      args: ["hello world"]
```
### Script

container 的便利包装器。该规范与容器相同，但添加了 source: 字段，允许您就地定义脚本。该脚本将保存到一个文件中并为您执行。脚本的结果会自动导出到 变量 `{{steps.<NAME>.outputs.result}}` 中，具体取决于它的调用方式。
```yaml
  - name: gen-random-int
    script:
      image: python:alpine3.6
      command: [python]
      source: |
        import random
        i = random.randint(1, 100)
        print(i)
```
### Resource

直接对集群资源进行操作。它可用于获取、创建、应用、删除、替换或修补集群上的资源。此示例在集群上创建一个 ConfigMap 资源
```yaml
  - name: k8s-owner-reference
    resource:
      action: create
      manifest: |
        apiVersion: v1
        kind: ConfigMap
        metadata:
          generateName: owned-eg-
        data:
          some: value

```
### Suspend
暂停模板将暂停执行，持续一段时间或直到手动恢复。可以从 CLI（使用 argo resume ）、API 端点或 UI 恢复挂起模板。
```yaml
  - name: delay
    suspend:
      duration: "20s"
```
### Steps
步骤模板允许您在一系列步骤中定义您的任务。模板的结构是“列表的列表”。外部列表将按顺序运行，内部列表将并行运行。如果要逐一运行内部列表，请使用同步功能。您可以设置各种选项来控制执行，例如 when: 子句以有条件地执行步骤。
在此示例中， step1 首先运行。完成后， step2a 和 step2b 将并行运行：
```yaml
  - name: hello-hello-hello
    steps:
    - - name: step1
        template: prepare-data
    - - name: step2a
        template: run-data-first-half
      - name: step2b
        template: run-data-second-half
```
### Dag
Dag 模板允许您将任务定义为依赖关系图。在 DAG 中，您列出所有任务并设置在特定任务开始之前必须完成哪些其他任务。没有任何依赖关系的任务将立即运行。在此示例中， A 首先运行。一旦完成， B 和 C 将并行运行，一旦它们都完成， D 将运行：
```yaml
  - name: diamond
    dag:
      tasks:
      - name: A
        template: echo
      - name: B
        dependencies: [A]
        template: echo
      - name: C
        dependencies: [A]
        template: echo
      - name: D
        dependencies: [B, C]
        template: echo
```

## 个人感受

概念太多，使用姿势太多，很多东西看的出都是功能的堆叠，并没有一个很好的兼容，上手成本比较高。