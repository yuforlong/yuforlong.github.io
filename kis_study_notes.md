---
title: K8s 入门实战笔记
date: 2023-07-06 14:55:00
categories:
- Docker
tags:
- Docker
- K8s
toc: true
---
 看完就忘，记了笔记也忘发。现整理在blog中，以便翻阅。
## 基础知识
<!--more-->

- YAML
      
    ![https://api2.mubu.com/v3/document_image/ad1bef2f-0dbf-4f7b-bc86-31a6b1e33b65-1472901.jpg](https://api2.mubu.com/v3/document_image/ad1bef2f-0dbf-4f7b-bc86-31a6b1e33b65-1472901.jpg)
    
## k8s 架构
- 运行机制
    
    ![https://api2.mubu.com/v3/document_image/0a4be73d-62ac-4447-9f3b-d5efed30e578-1472901.jpg](https://api2.mubu.com/v3/document_image/0a4be73d-62ac-4447-9f3b-d5efed30e578-1472901.jpg)
    
- 思维导图
    
    ![https://api2.mubu.com/v3/document_image/6933b900-1871-4197-a84d-969dee12a7ca-1472901.jpg](https://api2.mubu.com/v3/document_image/6933b900-1871-4197-a84d-969dee12a7ca-1472901.jpg)
    
    ![https://api2.mubu.com/v3/document_image/a453fb5b-62b6-492f-bcf2-7710d6882483-1472901.jpg](https://api2.mubu.com/v3/document_image/a453fb5b-62b6-492f-bcf2-7710d6882483-1472901.jpg)
    
    ![https://api2.mubu.com/v3/document_image/50c3823a-431c-4950-9cb8-a325e3e65f9c-1472901.jpg](https://api2.mubu.com/v3/document_image/50c3823a-431c-4950-9cb8-a325e3e65f9c-1472901.jpg)
    
- 架构说明图
    
    ![https://api2.mubu.com/v3/document_image/2ea25f63-92b9-4a73-90d0-8efe87c3f876-1472901.jpg](https://api2.mubu.com/v3/document_image/2ea25f63-92b9-4a73-90d0-8efe87c3f876-1472901.jpg)
    
## k8s 以Pod 为核心资源
- 一览图
    
    ![https://api2.mubu.com/v3/document_image/e997cd9b-1e4e-49a8-b84e-eff5a9bd87c7-1472901.jpg](https://api2.mubu.com/v3/document_image/e997cd9b-1e4e-49a8-b84e-eff5a9bd87c7-1472901.jpg)
    
- 资源关系图
    
    ![https://api2.mubu.com/v3/document_image/1302b037-c12c-4f90-8cf4-1121ccf88916-1472901.jpg](https://api2.mubu.com/v3/document_image/1302b037-c12c-4f90-8cf4-1121ccf88916-1472901.jpg)
    
## configmap、secret
- 结合 env 使用
    
    ![https://api2.mubu.com/v3/document_image/6102a868-424d-4734-be54-10df932757f8-1472901.jpg](https://api2.mubu.com/v3/document_image/6102a868-424d-4734-be54-10df932757f8-1472901.jpg)
    
- 结合 amount使用
    
    ![https://api2.mubu.com/v3/document_image/51d1cb85-9347-445a-a441-01bafac1bafb-1472901.jpg](https://api2.mubu.com/v3/document_image/51d1cb85-9347-445a-a441-01bafac1bafb-1472901.jpg)
    
## k8s Service工作原理
  
![https://api2.mubu.com/v3/document_image/d663c2cc-72df-4719-89f7-c15d17de2159-1472901.jpg](https://api2.mubu.com/v3/document_image/d663c2cc-72df-4719-89f7-c15d17de2159-1472901.jpg)

- svc配置的关联关系
    
    ![https://api2.mubu.com/v3/document_image/b027d5a6-c7c2-43a9-9b0e-aa6212ed430f-1472901.jpg](https://api2.mubu.com/v3/document_image/b027d5a6-c7c2-43a9-9b0e-aa6212ed430f-1472901.jpg)
    
- NodePort 与 Service、Deployment
    
    ![https://api2.mubu.com/v3/document_image/d59950a0-c763-4e97-808d-3432b9c6e7a1-1472901.jpg](https://api2.mubu.com/v3/document_image/d59950a0-c763-4e97-808d-3432b9c6e7a1-1472901.jpg)
    
## Ingress工作原理(7层代理)
  
![https://api2.mubu.com/v3/document_image/ebc50a6b-b153-46ce-9ef3-6f121ee94688-1472901.jpg](https://api2.mubu.com/v3/document_image/ebc50a6b-b153-46ce-9ef3-6f121ee94688-1472901.jpg)
    
- Ngx官网的Ingress Controller结构图

![https://api2.mubu.com/v3/document_image/34f95fca-95eb-4a42-afb1-93a867fd600a-1472901.jpg](https://api2.mubu.com/v3/document_image/34f95fca-95eb-4a42-afb1-93a867fd600a-1472901.jpg)

> Ingress 也只是一些 HTTP 路由规则的集合，相当于一份静态的描述文件，真正要把这些规则在集群里实施运行，还需要有另外一个东西，这就是 Ingress Controller，它的作用就相当于 Service 的 kube-proxy，能够读取、应用 Ingress 规则，处理、调度流量。
> 按理来说，Kubernetes 应该把 Ingress Controller 内置实现，作为基础设施的一部分，就像 kube-proxy 一样。
> 不过 Ingress Controller 要做的事情太多，与上层业务联系太密切，所以 Kubernetes 把 Ingress Controller 的实现交给了社区，任何人都可以开发 Ingress Controller，只要遵守 Ingress 规则就好。
      
        
        
- IngressClass
    
    ![https://api2.mubu.com/v3/document_image/2b3c3b10-962d-4442-b624-b21385adc84e-1472901.jpg](https://api2.mubu.com/v3/document_image/2b3c3b10-962d-4442-b624-b21385adc84e-1472901.jpg)
        
    - 为什么要有IngressClass随着 
    > Ingress 在实践中的大量应用，很多用户发现这种用法会带来一些问题，比如：> 1. 由于某些原因，项目组需要引入不同的 Ingress Controller，但 Kubernetes 不允许这样做；
    > 2. Ingress 规则太多，都交给一个 Ingress Controller 处理会让它不堪重负；
    > 3. 多个 Ingress 对象没有很好的逻辑分组方式，管理和维护成本很高；
    > 4. 集群里有不同的租户，他们对 Ingress 的需求差异很大甚至有冲突，无法部署在同一个 Ingress Controller 上。

    - ingress 和 Service、Ingress Class 的关系
      
    ![https://api2.mubu.com/v3/document_image/631e0551-418c-41ae-bed6-1578c10c93ba-1472901.jpg](https://api2.mubu.com/v3/document_image/631e0551-418c-41ae-bed6-1578c10c93ba-1472901.jpg)
    
## PV && PVC
  
![https://api2.mubu.com/v3/document_image/c163549b-5acb-41f5-b8b4-2be14259a929-1472901.jpg](https://api2.mubu.com/v3/document_image/c163549b-5acb-41f5-b8b4-2be14259a929-1472901.jpg)

- Pod 与 PV/PVC的关系
    
    ![https://api2.mubu.com/v3/document_image/8e2ded54-5581-4191-bf89-e7096be687c7-1472901.jpg](https://api2.mubu.com/v3/document_image/8e2ded54-5581-4191-bf89-e7096be687c7-1472901.jpg)

## StatefulSet 与 Service 对象的关系
  
    ![https://api2.mubu.com/v3/document_image/eccb38ac-d19e-40b2-9a8d-f0939afe52ee-1472901.jpg](https://api2.mubu.com/v3/document_image/eccb38ac-d19e-40b2-9a8d-f0939afe52ee-1472901.jpg)
    
## 容器探针
- 三种探针
    - Startup，启动探针，用来检查应用是否已经启动成功，适合那些有大量初始化工作要做，启动很慢的应用。
    - Liveness，存活探针，用来检查应用是否正常运行，是否存在死锁、死循环。
    - Readiness，就绪探针，用来检查应用是否可以接收流量，是否能够对外提供服务
- 探针结果的影响
    - 如果 Startup 探针失败，Kubernetes 会认为容器没有正常启动，就会尝试反复重启，当然其后面的 Liveness 探针和 Readiness 探针也不会启动。
    - 如果 Liveness 探针失败，Kubernetes 就会认为容器发生了异常，也会重启容器。
    - 如果 Readiness 探针失败，Kubernetes 会认为容器虽然在运行，但内部有错误，不能正常提供服务，就会把容器从 Service 对象的负载均衡集合中排除，不会给它分配流量。
- 工作流程
    
    ![https://api2.mubu.com/v3/document_image/185525c9-69dd-4312-85b3-f76323b789b2-1472901.jpg](https://api2.mubu.com/v3/document_image/185525c9-69dd-4312-85b3-f76323b789b2-1472901.jpg)
    
- 三种探针配置的关键字段：（startupProbe、livenessProbe、readinessProbe 这三种探针的配置方式都一样）
    - periodSeconds，执行探测动作的时间间隔，默认是 10 秒探测一次。
    - timeoutSeconds，探测动作的超时时间，如果超时就认为探测失败，默认是 1 秒。
    - successThreshold，连续几次探测成功才认为是正常，对于 startupProbe 和livenessProbe 来说它只能是 1。
    - failureThreshold，连续探测失败几次才认为是真正发生了异常，默认是 3 次。
    
## 资源限制：
- 可以在资源中通过request、limit 进行单独Pod资源的限制
- 可以通过namespace 结合 ResourceQuota 对整个NS进行限制
    - NS中的资源可以通过LimitRange，为Pod增加默认限额（否则已经有过限制 NS，是不允许创建未指定resources信息的POD）
    - ResourceQuota示例
        
        ![https://api2.mubu.com/v3/document_image/9d505803-9e30-4fee-b715-185d97ac448b-1472901.jpg](https://api2.mubu.com/v3/document_image/9d505803-9e30-4fee-b715-185d97ac448b-1472901.jpg)
        
        - 说明：
            - 所有 Pod 的需求总量最多是 10 个 CPU 和 10GB 的内存，上限总量是 10 个 CPU 和 20GB的内存。
            - 只能创建 100 个 PVC 对象，使用 100GB 的持久化存储空间。
            - 只能创建 100 个 Pod，100 个 ConfigMap，100 个 Secret，10 个 Service。
            - 只能创建 1 个 Job，1 个 CronJob，1 个 Deployment
    - LimitRange示例
        
        ![https://api2.mubu.com/v3/document_image/9ebfc177-ed7f-4eba-99a7-285d50a61aa8-1472901.jpg](https://api2.mubu.com/v3/document_image/9ebfc177-ed7f-4eba-99a7-285d50a61aa8-1472901.jpg)
        
        - 说明：
            - 它设置了每个容器默认申请 0.2 的 CPU 和 50MB 内存，容器的资源上限是 0.5 的 CPU 和100MB 内存，每个 Pod 的最大使用量是 0.8 的 CPU 和 200MB 内存。
    
## Prometheus
- 架构图
    
    ![https://api2.mubu.com/v3/document_image/479a2e88-c077-41a3-ba24-dfd33aa8e76e-1472901.jpg](https://api2.mubu.com/v3/document_image/479a2e88-c077-41a3-ba24-dfd33aa8e76e-1472901.jpg)

- 脚本备份：
- Ubuntu 安装 kubeadm

    ```bash
    apt-get update && apt-get install -y apt-transport-https curl https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | apt-key add -  
    
    cat <<EOF >/etc/apt/sources.list.d/kubernetes.list 
    
    deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main 
    
    EOF 
    
    apt-get update apt-get install -y kubelet kubeadm kubectl
    ```

    

- Ngx Ingress Controller 还是看官方文档吧

    > [Installation with Manifests | NGINX Ingress Controller](https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-manifests/)
    
## 常用命令：
- kubectl
    
    > [kubectl命令行工具用法详解 - 简书 (jianshu.com)](https://www.jianshu.com/p/8710a3a0aadd)
    
    - kubectl get pod
    - kubectl get pod --v=9
    - kubectl apply -f ngx-pod.yaml
    - kubectl delete -f ngx-pod.yaml
    - kubectl api-resources
    - kubectl explain pod
    
- kubectl pod 相关
    - kubectl run ngx --image=nginx:alpine --dry-run=client -o yaml
    
        > 尝试生成Pod的yaml模板
    
    - kubectl cp wp.conf ngx-pod:/tmp 
    
        > copy 文件至指定name的pod指定目录 中。其中ngx-pod为yaml文件中指定的pod metadata中的name
        >
        > 准确地说，“kubectl cpomkubectl exec” 操作的应该是Pod 里的容器，需要用“-。”参数指定容器名，不过因为大多数 Pod 里只有一个容器，所以就省路了。
    
    - kubectl exec -it ngx-pod -- sh
    
        > 交互式进入ngx-pod内部，并执行sh
    
    - kubectl logs ngx-pod
    
        > Kubernetes 的 Pod 不会在前台运行，只能在后台（相当于默认使用了参数 -d），所以输出信息不能直接看到。我们可以用命令 kubectl logs，它会把 Pod 的标准输出流信息展示给我们看
    
    - kubectl get pod
    
    - kubectl describe pod ngx-pod
    
        > 重点关注返回结果的Events信息
    
    - kubectl delete pod busy-pod
    
- kubectl job 相关
    - kubectl create job echo-job --image=busybox --dry-run=client -o yaml
    
        > 尝试生成job的yaml模板
    
    - 部分spec属性及含义
        - activeDeadlineSeconds，设置 Pod 运行的超时时间。
        - backoffLimit，设置 Pod 的失败重试次数。
        - completions，Job 完成需要运行多少个 Pod，默认是 1 个。
        - parallelism，它与 completions 相关，表示允许并发运行的 Pod 数量，避免过多占用资源。
    
## 其他：
    - k8s中的资源限制CPU单位为毫核，比如：50m 对应的是 0.05 核（50/1000）