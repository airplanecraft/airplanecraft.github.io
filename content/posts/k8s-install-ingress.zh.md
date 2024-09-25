+++
Categories = ["Technology"]
Description = ""
Tags = ["k8s","ingress"]
date = "2018-05-03T10:30:37+08:00"
menu = "saa"
title = "k8s使用ingress总结"
toc = true
+++

### 安装准备 
- Centos7.7 vm 或者真实的物理机三台(master一台,node两台)
- 硬件要求2GB ram,最低2CPU，最少32GB 硬盘
- 节点之间最好网络互通，如果不考虑安全可以关闭firewalld
- 可以访问到外部网络,因为需要网络资源，比如yum源和其他k8s需要的yaml文件

### 需要安装和配置
- yum源配置
- 防火墙selinux的关闭与配置
- swap分区配置
- 桥接ipv4流量交给iptables
- 文件句柄数的限制
- Docker的安装
- Kubeadmin,flannel
- 创建deployment,service,pod,ingress,ingress controller,kubernets-dashboard

### 具体安装步骤
- VM 网络选择bridge模式 master:192.168.25.200 node:192.168.25.187/192.168.25.188

- 防火墙
  ```
    $ systemctl stop firewalld
    $ systemctl disable firewalld
  ```
- Selinux
  ```
    $ sed -i "s/^SELINUX=enforcing/SELINUX=disabled/g" /etc/selinux/config
    $ sed -i "s/^SELINUX=permissive/SELINUX=disabled/g" /etc/selinux/config
    $ setenforce 0
  ```
- 关闭swap
  ```
    swapoff -a 
    sed -i 's/.*swap.*/#&/' /etc/fstab
  
  ```
- 网络转发
  
  ```
    cat > /etc/sysctl.d/k8s.conf << EOF
    net.bridge.bridge-nf-call-ip6tables = 1
    net.bridge.bridge-nf-call-iptables = 1
  EOF
  ```
- 文件句柄
  ```
    echo "* soft nofile 65536" >> /etc/security/limits.conf
    echo "* hard nofile 65536" >> /etc/security/limits.conf
    echo "* soft nproc 65536"  >> /etc/security/limits.conf
    echo "* hard nproc 65536"  >> /etc/security/limits.conf
    echo "* soft  memlock  unlimited"  >> /etc/security/limits.conf
    echo "* hard memlock  unlimited"  >> /etc/security/limits.conf
  ```
- yum源

  ```
  wget https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo -O /etc/yum.repos.d/docker-ce.repo


  cat > /etc/yum.repos.d/kubernetes.repo << EOF
        [kubernetes]
        name=Kubernetes
        baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
        enabled=1
        gpgcheck=1
        repo_gpgcheck=1
        gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
        https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg

    EOF

     $ wget https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
    $ rpm --import yum-key.gpg
    $ wget https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
    $ rpm --import rpm-package-key.gpg

  ```

- 安装Docker kubectl kubeadm,kubelet

  ```
    $ yum -y install docker-ce-18.06.1.ce-3.el7
    $ systemctl enable docker && systemctl start docker
    $ yum install -y kubelet-1.13.3 kubeadm-1.13.3 kubectl-1.13.3 kubernetes-cni-0.6.0
    $ systemctl enable kubelet
  ```
- 使用kubeadm来deploy Kubernets master

  ```
    $ kubeadm init \
    --apiserver-advertise-address=192.168.25.249 \
    --image-repository registry.aliyuncs.com/google_containers \
    --kubernetes-version v1.13.3 \
    --service-cidr=10.1.0.0/16 \
    --pod-network-cidr=10.244.0.0/16
  ```
   执行命令后生成的日志保存
   ```
   kubeadm join 192.168.25.200:6443 --token xxx --discovery-token-ca-cert-hash sha256:xxx
   ```

- 配置kubectl

  ```
    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config
  ```
- 安装flannel

  ```
    kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/a70459be0084506e4ec919aa1c114638878db11b/Documentation/kube-flannel.yml

  ```
- Node节点加入

  z在node节点上执行上面保存的日志

  ```
    kubeadm join 192.168.25.200:6443 --token xxx --discovery-token-ca-cert-hash sha256:xxx
  ```

- 安装kubernets-dashboard

  ```
    wget https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml

    kubectl apply -f kubernetes-dashboard.yaml

    wget https://gist.githubusercontent.com/chukaofili/9e94d966e73566eba5abdca7ccb067e6/raw/0f17cd37d2932fb4c3a2e7f4434d08bc64432090/k8s-dashboard-admin-user.yaml
     
   kubectl apply -f k8s-dashboard-admin-user.yaml
    key:
    kubectl describe secrets -n kube-system $(kubectl -n kube-system get secret | awk '/dashboard-admin/{print $1}')

    kubectl create clusterrolebinding test:anonymous --clusterrole=cluster-admin --user=system:anonymous


    
  ```
- Docker 配置
  
  ```
  /etc/docker/daemon.json
  {
    "insecure-registries": [
        "0.0.0.0/0"
    ]
}

  ```
   


  ```
   给k8是生成访问docker的key
  kubectl -n default create secret docker-registry registry-key --docker-server=192.168.25.167:5000 --docker-username=xxx --docker-password=xx --  docker-email= @xx.com
   

  ```

-  deploy and svc
  
  ```
   -deploy-svc.yaml

    apiVersion: v1
    kind: Service
    metadata:
      name:  terminal-svc
      namespace: default
    spec:
      type: NodePort
      selector:
        app:  terminal
      ports:
      - name: robo1
        port: 3012
        targetPort: 3012
        nodePort: 30012
      - name: robo2
        port: 9222
        targetPort: 9222
        nodePort: 30022
    ---
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name:  terminal-deploy
      namespace: default
      labels:
        app:  terminal
    spec:
      replicas: 1
      selector:
        matchLabels:
          app:  terminal
      template:
        metadata:
          labels:
            app:  terminal
        spec:
          imagePullSecrets:
          - name: registry-key
          containers:
          - name:  terminal
            image: 192.168.25.25:5000/node-slim-web-app:latest //docker registry
            ports:
            - name: robo1
              containerPort: 3012
            - name: robo2
              containerPort: 9222
  ```

- ingress

  ```
    wget https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/mandatory.yaml
    vi mandatory.yaml
    kubectl apply -f mandatory.yaml
     kubectl label node --all kubernetes.io/os=linux

  ```
  ```
  ingress.yaml

    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: ingress
      namespace: default
    spec:
      rules:
        - host: xxxx.com
          http:
            paths:
              - path: /api/GetTerminalWS/a1           # urI路径为空，默认为/
                backend:
                  serviceName: terminal-svc
                  servicePort: 3012
  ```




kubectl label node --all kubernetes.io/os=linux

