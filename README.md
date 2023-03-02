# Agree-kubeadm v1.17.0

[![GoDoc Widget]][GoDoc] [![CII Best Practices](https://bestpractices.coreinfrastructure.org/projects/569/badge)](https://bestpractices.coreinfrastructure.org/projects/569)

<img src="https://github.com/kubernetes/kubernetes/raw/master/logo/logo.png" width="100">

----

Agree-kubeadm is an open source tools for install kubernetes and ACaaS addon eg. 


----

## To start using Agree-kubeadm
初始化集群
```
cat kubeadm.conf
apiVersion: kubeadm.k8s.io/v1beta2
bootstrapTokens:
- groups:
  - system:bootstrappers:kubeadm:default-node-token
  token: abcdef.0123456789abcdef
  ttl: 24h0m0s
  usages:
  - signing
  - authentication
kind: InitConfiguration
localAPIEndpoint:
  advertiseAddress: 6.6.6.88
  bindPort: 6443
---
apiServer:
  timeoutForControlPlane: 4m0s
apiVersion: kubeadm.k8s.io/v1beta2
certificatesDir: /etc/kubernetes/pki
clusterName: kubernetes
controllerManager:
  extraArgs:
    experimental-cluster-signing-duration: "876000h0m0s"
    feature-gates: "RotateKubeletServerCertificate=true"
dandelion:
  networkType: ipip
  imageTag: v0.12.0
dns:
  type: CoreDNS
etcd:
  local:
    dataDir: /var/lib/etcd
imageRepository: acaas-registry.agree:9980/k8s.gcr.io
kind: ClusterConfiguration
kubernetesVersion: v1.17.0
networking:
  dnsDomain: cluster.local
  podSubnet: 10.244.0.0/16
  serviceSubnet: 10.96.0.10/24
scheduler: {}
---
apiVersion: kubeproxy.config.k8s.io/v1alpha1
kind: KubeProxyConfiguration
mode: iptables

akubeadm init --cofig=kubeadm.conf --v=3
```
指定组件的安装
```
akubeadm init phase addon dandelion --config=kubeadm.conf --v=3
```

## To start developing AKubeadm

If you want to build AKubeadm right away there are two options:

##### You have a working [Go environment].

```
require go < 1.17.0

mkdir -p $WORKDIR
cd $WORKDIR/agree-kubeadm
git clone https://github.com/zhutong196/agree-kubeadm.git
cd agree-kubeadm
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o akubeadm kubeadm.go

```

##### You have a working [Docker environment].

```
require go < 1.17.0

git clone ghttps://github.com/zhutong196/agree-kubeadm.git
cd agree-kubeadm
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o akubeadm kubeadm.go

```


## Support

If you need support, start with the [troubleshooting guide],
and work your way through the process that we've outlined.

