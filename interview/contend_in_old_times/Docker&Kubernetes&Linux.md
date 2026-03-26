- docker常用命令
	docker pull:拉取或者更新指定镜像； docker push:将镜像推送至远程仓库； docker images:列出所有镜像； docker rmi:删除镜像；     
    docker ps:列出所有容器； docker rm:删除容器
- docker使用流程
	a.创建Dockerfile后，docker build创建容器的镜像； b.推送或拉取镜像
- Dockerfile中最常见的指令是什么？Dockerfile中的命令COPY和ADD命令有什么区别？
	FROM:指定基础镜像；     
	运行指令(RUN & CMD & ENTRYPOINT)详见：https://www.cnblogs.com/javazhiyin/p/12955024.html(dockerfile常见面试题)  
	RUN:运行指定的命令；
	CMD:容器启动时要运行的命令   
	WORKDIR:(默认在/根目录)终端登录进去的落脚点。类似于cd,如果无该目录,则自动创建后再cd进去； 
	ENV:设置环境常量，方便下文引用  
	ONBUILD:触发器，当镜像用作另一个镜像构建的基础(例如可使用特定于用户的配置自定义的应用程序构建环境或守护程序)时，ONBUILD指令向镜像添加将在稍后执行的触发指令。  
	VOLUME:自建容器卷  
	EXPOSE:当前容器对外界暴露出的端口  
	LABEL:为镜像指定标签；MAINTAINER:镜像维护者的姓名和邮箱地址
- 查看镜像支持的环境变量
	docker run IMAGE env
- [从零开始入门 K8s：详解 K8s 核心概念_云计算_李响_InfoQ精选文章](https://www.infoq.cn/article/knmavdo3jxs3qpkqtzbw)
	- 用户可以通过 UI 或者 CLI 提交一个 Pod 给 Kubernetes 进行部署，这个 Pod 请求首先会通过 CLI 或者 UI 提交给 Kubernetes API Server，下一步 API Server 会把这个信息写入到它的存储系统 etcd，之后 Scheduler 会通过 API Server 的 watch 或者叫做 notification 机制得到这个信息：有一个 Pod 需要被调度。
	- 用户可以通过 UI 或者 CLI 提交一个 Pod 给 Kubernetes 进行部署，这个 Pod 请求首先会通过 CLI 或者 UI 提交给 Kubernetes API Server，下一步 API Server 会把这个信息写入到它的存储系统 etcd，之后 Scheduler 会通过 API Server 的 watch 或者叫做 notification 机制得到这个信息：有一个 Pod 需要被调度。这个时候 Scheduler 会根据它的内存状态进行一次调度决策，在完成这次调度之后，它会向 API Server report 说：“OK！这个 Pod 需要被调度到某一个节点上。”
	- 这个时候 API Server 接收到这次操作之后，会把这次的结果再次写到 etcd 中，然后 API Server 会通知相应的节点进行这次 Pod 真正的执行启动。相应节点的 kubelet 会得到这个通知，kubelet 就会去调 Container runtime 来真正去启动配置这个容器和这个容器的运行环境，去调度 Storage Plugin 来去配置存储，network Plugin 去配置网络。 


- Kubernetes集群中Master组件的作用和执行过程。
1.  创建Kubernetes集群 Kubernetes集群由Master节点和多个Worker节点组成。Master节点是Kubernetes的控制中心，负责管理和调度Worker节点上的容器。可以使用各种工具来创建Kubernetes集群，例如kubeadm、Minikube、Kops、k3s等。

在使用kubeadm创建Kubernetes集群时，kubeadm会执行以下操作：

-   在Master节点上安装和配置kube-apiserver、kube-controller-manager、kube-scheduler、etcd等组件
-   初始化Master节点，包括创建Kubernetes配置文件、生成TLS证书、生成kubeconfig文件等
-   将Worker节点加入到集群中

2.  创建Deployment 在Kubernetes中，Deployment是一种资源对象，用于指定要部署的容器镜像及其相关配置。可以使用kubectl命令或YAML文件来创建Deployment。

在创建Deployment时，用户可以使用kubectl命令或YAML文件将Deployment的定义发送到kube-apiserver。kube-apiserver是Kubernetes的API服务器，它接收用户的请求并将其转换为内部API对象。

kube-controller-manager是Kubernetes的控制器管理器，它会检查Deployment的定义并确保其符合用户的期望。如果Deployment的副本数不足，kube-controller-manager会自动创建新的Pod来满足需求。

3.  创建Service 在Kubernetes中，Service是一种资源对象，用于公开Deployment的访问端点。可以使用kubectl命令或YAML文件来创建Service。

在创建Service时，用户可以使用kubectl命令或YAML文件将Service的定义发送到kube-apiserver。kube-apiserver将Service的定义保存到etcd中。

kube-proxy是Kubernetes的代理程序，它会监视Service的定义并为其创建一个虚拟IP地址和端口号。当Pod对象被创建或删除时，kube-proxy会更新这些端点的映射关系。

4.  创建Ingress 在Kubernetes中，Ingress是一种资源对象，用于公开Service的访问端点。可以使用kubectl命令或YAML文件来创建Ingress。

在创建Ingress时，用户可以使用kubectl命令或YAML文件将Ingress的定义发送到kube-apiserver。kube-apiserver将Ingress的定义保存到etcd中。

ingress-controller是Kubernetes的控制器之一，它会监视Ingress的定义并为其创建一个HTTP(S)负载均衡器。当请求到达负载均衡器时，ingress-controller会将其路由到相应的Service的虚拟IP地址和端口号上。

5.  发送命令 一旦Deployment、Service和Ingress都已创建，用户可以使用kubectl命令来发送命令。例如，可以使用kubectl命令来扩展Deployment的副本数、升级容器镜像版本等。

在发送命令时，用户可以使用kubectl命令将命令发送到kube-apiserver。kube-apiserver会将命令转换为内部API对象并保存到etcd中。

kube-controller-manager会检查API对象的定义并确保其符合用户的期望。如果API对象的状态发生变化，kube-controller-manager会自动更新相关的资源对象。

kube-scheduler是Kubernetes的调度器，它会将Pod分配给Worker节点并确保它们在正确的节点上运行。当有新的Pod需要创建时，kube-scheduler会检查Worker节点的资源情况并选择最合适的节点来运行Pod。

6.  执行命令 一旦用户发送命令，kube-controller-manager和kube-scheduler就会开始执行相关的操作。kube-controller-manager会确保Deployment、Service和Ingress的副本数符合用户的期望，而kube-scheduler会将Pod分配到正确的节点上运行。

kubelet是Kubernetes的节点代理程序，它会监视kube-apiserver和etcd中的资源对象，并执行相应的操作。当kubelet检测到新的Pod需要在节点上运行时，它会从Docker Hub或私有仓库中下载相关的容器镜像，并创建容器来运行Pod。

一旦容器开始运行，kubelet就会监视其状态并确保其始终保持在运行状态。如果容器崩溃或停止运行，kubelet会自动重启容器并将其恢复到运行状态。

总之，Kubernetes从创建到用户发送命令到执行完毕的全过程是一个复杂的过程，涉及多个组件之间的交互和协作。Master节点中的组件起着关键作用，它们负责管理和调度Worker节点上的容器，确保它们始终保持在运行状态。

- 讲讲lvs的几种模式
LVS（Linux Virtual Server）有三种主要的负载均衡模式，它们分别是：NAT模式、DR模式和TUN模式。

1.  NAT模式（Network Address Translation）

在NAT模式下，LVS的负载均衡器会将请求报文的目标IP地址改为某个真实服务器的IP地址，同时也会修改请求报文中的源IP地址。这样，真实服务器就可以直接将响应发送给客户端，而不必经过负载均衡器。

优点：NAT模式比较容易配置，可以实现透明的负载均衡，同时也可以保护真实服务器的IP地址。

缺点：由于负载均衡器需要对请求报文进行地址转换，因此会对性能产生一定的影响。

2.  DR模式（Direct Routing）

在DR模式下，LVS的负载均衡器不对请求报文进行地址转换，而是直接将请求报文转发给真实服务器，真实服务器直接向客户端发送响应。此时，负载均衡器只负责将请求报文转发给真实服务器，不参与响应报文的传输。

优点：DR模式对负载均衡器的性能要求比较低，可以实现较高的性能。

缺点：DR模式需要在网络中配置特殊的路由规则，比较复杂。

3.  TUN模式（IP Tunneling）

在TUN模式下，LVS的负载均衡器会在自己和真实服务器之间建立一个IP隧道，将请求报文封装在隧道中发送给真实服务器。真实服务器收到请求后，将响应报文发送给LVS的负载均衡器，负载均衡器再将响应报文解封装并发送给客户端。

优点：TUN模式不需要对请求报文进行地址转换，对负载均衡器的性能要求比较低，同时也可以隐藏真实服务器的IP地址。

缺点：TUN模式需要在网络中配置特殊的隧道设备，比较复杂。
