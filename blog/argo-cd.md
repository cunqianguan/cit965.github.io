---
slug: Argo CD
title: Argo CD
authors: mouuii
tags: [devops]
---

### 什么是 Argo CD
ArgoCD 是使用 Kubernetes 持续交付的流行工具。它通过不断协调 repo 的状态与实时工作负载，自动将应用程序部署到 Kubernetes 集群中。

GitOps模型是Argo设计中不可或缺的一部分。它使 repo 成为应用程序所需状态的单一事实来源。应用所需的所有 Kubernetes 清单、Kustomize 模板、Helm charts 和配置文件都应提交到您的存储库。这些资源“声明”应用的成功部署。

Argo 将声明的状态与集群中实际运行的状态进行比较，然后应用正确的更改来解决任何差异。此过程可以配置为自动运行，防止集群偏离存储库。每当出现差异时，Argo 都会重新同步状态，例如在您手动运行 Kubectl 命令之后。

Argo带有CLI和Web UI。它支持多租户和多集群环境，与 SSO 提供程序集成，生成审计跟踪，并可以实施复杂的 rollout 策略，例如 Canary 部署和蓝/绿升级。它还提供集成的回滚功能，因此您可以快速从部署故障中恢复

从历史上看，大多数 CI/CD 实现都依赖于推送驱动行为。这需要您将集群连接到 CI/CD 平台，然后在管道中使用 Kubectl 和 Helm 等工具来应用
Kubernetes 的更改

Argo是一个基于拉动的CI / CD系统。它运行在您的 Kubernetes 集群中，并从您的存储库中提取源代码。然后，Argo 无需手动配置管道即可为您应用更改。

此模型比基于推送的工作流更安全。您不必公开集群的 API 服务器或在 CI/CD 平台中存储 Kubernetes credentials。破坏源存储库只会使攻击者能够访问您的代码，而不是你的集群。

### 为什么选择Argo CD？

应用程序定义、配置和环境应具有声明性和版本控制。应用程序部署和生命周期管理应该是自动化的、可审计的且易于理解的。

### 快速入门 

#### 前置条件
- 安装 kubectl 命令行工具
- 有一个 kubeconfig 文件（默认位置是 ~/.kube/config）

### 安装命令

```sh
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
这将创建一个新的命名空间 argocd ，Argo CD 服务和应用程序资源将驻留在其中。
:::tip
安装清单包括引用 argocd 命名空间的 ClusterRoleBinding 资源。如果要将Argo CD安装到不同的命名空间中，请确保更新命名空间引用
:::

此默认安装将具有自签名证书，如果不进行一些额外的工作，则无法访问。执行以下操作之一：

- 按照说明配置证书（并确保客户端操作系统信任它）
- 将客户端操作系统配置为信任自签名证书
- 在本指南中的所有 Argo CD CLI 操作上使用 --insecure 标志

Kubectl 端口转发也可用于连接到 API 服务器，而无需公开服务

```sh
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

### 实际示例：使用 Argo CD 部署到 Kubernetes

让我们使用 Argo 在 Kubernetes 中运行一个基本的 NGINX Web 服务器实例。我们假设您已经可以访问 Kubernetes 集群，并且您的机器上已经拥有可用的 Kubectl 和 Helm CLI。

#### 创建应用的 GitHub 存储库

首先，前往 GitHub 并为您的应用程序创建一个新的存储库。之后，将您的存储库克隆到您的计算机，准备提交您的 Kubernetes 清单

```sh
$ git clone https://github.com/<username>/<repo>.git
```

复制以下 YAML 并将其另存为 deployment.yaml 在存储库中

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  namespace: argo-demo
  labels:
    app.kubernetes.io/name: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app.kubernetes.io/name: nginx
  template:
    metadata:
      labels:
        app.kubernetes.io/name: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
          - name: http
            containerPort: 80
```   


它定义了一个运行三个 NGINX 副本的基本 Kubernetes 部署对象

接下来，复制第二个 YAML 文件并将其保存到 service.yaml 。它会设置负载均衡器服务以在群集外部公开部署

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx
  namespace: argo-demo
spec:
  type: LoadBalancer
  selector:
    app.kubernetes.io/name: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: http
```      
最后，添加一个将创建应用程序命名空间的清单：

```yaml 
apiVersion: v1
kind: Namespace
metadata:
  name: argo-demo
```

将更改提交到存储库，然后将其推送到 GitHub：