# Build a Minikube

> From scratch

This repository contains resources to create a minikube from scratch.

## Feature targeted

Ability to create a deployment which should result in pods running.

## Pre-requisites

1. Linux machine (centos 7)


## Steps

**1. Setup VM**

`Install Kubelet`

```bash
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
sudo yum install docker -y 
sudo yum install kubelet kubectl -y
```

**2. Run Kubelet**

```bash

$ sudo su -

$ systemctl start docker

$ mkdir manifests

## Run Kubelet pointing to manifests
$ kubelet --pod-manifest-path ./manifests
```

**3. Run Kubernetes Master components**

Copy the following files into the manifests directory

```bash


## API Server
$ cp build-a-minikube/resources/02-api-server.yml ./manifests/

## ETCD
$ cp build-a-minikube/resources/03-etcd.yml ./manifests/

## Controller Manager
$ cp build-a-minikube/resources/05-controller-mngr.yml ./manifests/

## Scheduler
cp build-a-minikube/resources/06-scheduler.yml ./manifests/
```

**4. Restart Kubelet poining to KubeAPI server**

```bash
$ kubelet --pod-manifest-path ./manifests --kubeconfig build-a-minikube/resources/kubeconfig.yml
```

**5. Create Kubernetes Deployment resource**

```bash
## This should return server version
$ kubectl version

$ kubectl apply -f /resources/04-nginx-deployment.yml
```

There we go now we have our own minikube 

### Common Issues you might run into

```
 failed to run Kubelet: misconfiguration: kubelet cgroup driver: "cgroupfs" is different from docker cgroup driver: "systemd"

kubelet --pod-manifest-path ./manifests --cgroup-driver=systemd

kubelet --pod-manifest-path ./manifests --kubeconfig build-a-minikube/resources/kubeconfig.yml --cgroup-driver=systemd 

```
```
Tweak the Path of kubeconfig.yaml for scheduler and controller as per your requirement
```