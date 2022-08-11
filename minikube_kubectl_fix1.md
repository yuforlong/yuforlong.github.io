---
title: minikube mac 踩坑实录
date: 2022-08-11 17:00:00
categories:
- k8s
- minikube
tags:
- k8s
- minikube
toc: true
---
在mac使用minikube和kubectl测试，没想到也能踩这么多坑。逐一记录如下：
## 踩坑一：toomanyrequests
<!--more-->

- 过程

1. 执行如下命令
   ```bash
   minikube kubectl -- run ngx --image=nginx:alpine --force
   ```

2. 查看pod status
   ```bash
   minikube kubectl get pod ngx
   ```

   其状态显示为：ErrImagePull 或 ImagePullBackoff

3. 查看详情
   ```bash
   minikube kubectl describe pod ngx
   ```

4. 报错信息
   ```txt
    Failed to pull image "nginx:alpine": rpc error: code = Unknown desc = Error response from daemon: toomanyrequests: You have reached your pull rate limit. You may increase the limit by authenticating and upgrading: https://www.docker.com/increase-rate-limit
   ```

- 修复

1. docker login 方法：[参考](https://zhuanlan.zhihu.com/p/309560768)

   尝试注册docker账号，并通过docker login，无效。

3. 更换镜像方法: [参考](https://www.cnblogs.com/xmai/p/15080572.html)
   
   由于未找到：/etc/docker/daemon.json 而无果。后[参考](https://blog.csdn.net/m0_55613022/article/details/124490832) 在GUI上配置了如下内容得以解决。但是又引入了第二个问题：

   ```json
   {
        "features": {
            "buildkit": true
        },
        "experimental": false,
        "registry-mirrors": [
            "https://docker.mirrors.ustc.edu.cn",
            "https://kfwkfulq.mirror.aliyuncs.com",
            "https://2lqq34jg.mirror.aliyuncs.com",
            "https://pee6w651.mirror.aliyuncs.com",
            "https://registry.docker-cn.com",
            "http://hub-mirror.c.163.com"
        ]
    }
   ```

## 踩坑二：重启

- 过程：
  
1. 在更新docker配置后，尝试接着运行minikube以测试

    ```bash
    minikube kubectl describe pod ngx
    ```

2. 报错：

   ```txt
   The connection to the server 127.0.0.1:52554 was refused - did you specify the right host or port?
   ```

3. 尝试执行其他命令：

   ```bash
   minikube kubectl delete pod ngx

   minikube kubectl get pod ngx
   ```

4. 报错依旧

- 修复

1. [参考](http://www.manongjc.com/detail/17-fxhwupmljriiugb.html) 无效

   系统无systemctl命令

2. 想起查看minikube的状态吧

    ```bash
     minikube status
    ```

3. 通过状态发现未启动
   ```txt
   minikube
   type: Control Plane
   host: Stopped
   kubelet: Stopped
   apiserver: Stopped
   kubeconfig: Stopped
   ```

4. start 一下？
   ```bash
   minikube start --image-mirror-country='cn'
   ```

   ```txt
    😄  Darwin 12.5 (arm64) 上的 minikube v1.22.0
    ✨  根据现有的配置文件使用 docker 驱动程序
    👍  Starting control plane node minikube in cluster minikube
    🚜  Pulling base image ...
    🔄  Restarting existing docker container for "minikube" ...
    ❗  The image 'registry.cn-hangzhou.aliyuncs.com/google_containers/coredns/coredns:v1.8.0' was not found; unable to add it to cache.
    🐳  正在 Docker 20.10.7 中准备 Kubernetes v1.21.2…
    ❌  Unable to load cached images: loading cached images: stat /Users/xxxxxxx/.minikube/cache/images/registry.cn-hangzhou.aliyuncs.com/google_containers/coredns/coredns_v1.8.0: no such file or directory
    🔎  Verifying Kubernetes components...
        ▪ Using image registry.cn-hangzhou.aliyuncs.com/google_containers/k8s-minikube/storage-provisioner:v5 (global image repository)
    🌟  Enabled addons: storage-provisioner, default-storageclass
    🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by defaul
   ```   

5. 再来一次，成功了。看来在重启docker后，minikube需要重新启动。

   ```bash
   minikube kubectl -- run ngx --image=nginx:alpine --force

   minikube kubectl get pod ngx
   ```

   ```txt
   NAME   READY   STATUS    RESTARTS   AGE
   ngx    1/1     Running   0          33s
   ```

## 祝福

祝大家好运！