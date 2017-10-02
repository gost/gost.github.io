---
layout: post
title: "GOST and Kubernetes"
date: 2017-10-02 11:49:00
author: Bert
categories: 
- blog
- Kubernetes
- docker
---
Kubernetes is an open source system for managing containerized applications across multiple hosts, providing basic mechanisms for deployment, maintenance, and scaling of applications.

Kubernetes builds upon a decade and a half of experience at Google running production workloads at scale.

In this blog we are going to run GOST in Kubernetes. We are using a small edition of Kubernetes called Minikube to test things out. Minikube is a tool that makes it easy to run Kubernetes locally. Minikube runs a single-node Kubernetes cluster inside a VM on your laptop.

So step 1 is to install MiniKube, you can find installation instructions for Mac, Linux and Windows at https://kubernetes.io/docs/tasks/tools/install-kubectl/

Step 2 is to start the Minikube cluster:

```
$ minikube start
```

Step 3: Download the GOST Docker-compose.yml file. The Docker-compose file is slightly altered, it has one extra line with respect to the default docker-compose file:

https://github.com/gost/docker-compose/blob/kubernetes/docker-compose.yml#L42

```
kompose.service.type: NodePort
```

This line is needed to be able to access the dashboard from outside the Kubernetes cluster.

To download the modified docker-compose file do:

```
$ wget https://raw.githubusercontent.com/gost/docker-compose/kubernetes/docker-compose.yml
```

Step 4: Install Kompose

Kompose is a tool to convert from the docker-compose world to the Kubernetes world.

```
$ curl -L https://github.com/kubernetes/kompose/releases/download/v1.2.0/kompose-linux-amd64 -o kompose
$ chmod +x kompose
$ sudo mv ./kompose /usr/local/bin/kompose
```

Step 5: Run GOST in Kubernetes

```
$ kompose up
```

Step 6: Get the ip and port of GOST

```
$ minikube service dashboard --url 
http://192.168.99.100:30648
```

Now you should be able to get the GOST dashboard in your browser at http://192.168.99.100:30648



