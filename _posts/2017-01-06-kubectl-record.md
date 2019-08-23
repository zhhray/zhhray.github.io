---
layout: post
title: "kubectl 笔记"
subtitle: 'kubectl常用命令'
author: "zhhray"
header-style: text
tags:
  - kubectl
  - 学习笔记

---

#### 集群信息展示  
`kubectl cluster-info`
#### 集群信息的调试、日志信息查看  
`kubectl cluster-info dump`  
#### 查看 服务  
`kubectl get all --namespace=kube-system`  
#### 查看 svc 与 pod  
`kubectl get svc -n kube-system`  
#### 以yaml格式文件内容描述来创建资源  
`kubectl create -f kubernetes-dashboard.yaml`  
#### 查看全部namespace下的pod  
`kubectl get pods --all-namespaces`  
#### 查看pods详细描述   
`kubectl describe pods`  
#### 查看kube-system 命名空间下的pod日志  
`kubectl --namespace=kube-system logs kubernetes-dashboard-xxx`
#### 查看kube-system 命名空间下的kubernetes-dashboard日志   
`kubectl -n kube-system get pods | grep kubernetes-dashboard | awk '{print $1}' | xargs kubectl -n kube-system logs`  
#### 查询dashboard运行的情况  
`kubectl describe --namespace kube-system service kubernetes-dashboard`  
#### 查询dashboard pod信息  
`kubectl describe pod kubernetes-dashboard-6ddbf5b678-wv4tm -n kube-system`  

#### 查命名空间为kube-system的role  
`kubectl get role -n kube-system`  
#### 查命名空间为kube-system的rolebinding  
`kubectl get rolebinding -n kube-system`  
#### 查所有clusterrole  
`kubectl get clusterrole`  
#### 查所有clusterrolebinding  
`kubectl get clusterrolebinding`  
#### 查看某个clusterrole角色yaml描述  
`kubectl get clusterrole/cluster-admin -o yaml`  

#### 查看某namespace 的secrets  
`kubectl get secrets -n kube-public`  
#### 查看某个secrets详细信息  
`kubectl describe secret default-token-wvkvt`  
