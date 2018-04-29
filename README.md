# Kubernetes Tutorial 
My kubernetes examples for Developers. A collection of how to work with Kubernetes and Docker.

&nbsp;

## **Contents**

01. **[Prerequisites](#prerequisites)**
02. **[Installation](#minikube)**
	- [Setup Kubernetes on Local Machine](#minikube)
	- [Setup Kubernetes with Vagrant and Virtualbox](https://github.com/sayems/kubernetes.examples/blob/master/vagrant.md#kubernetes-with-vagrant-and-virtualbox)	
	- [Setup Google Kubernetes Engine](https://cloud.google.com/kubernetes-engine/)
	- [Setup Kubernetes on AWS](https://kubernetes.io/docs/getting-started-guides/aws/)
03. **[Dashboard](#kubernetes-dashboard)**
04. **[Useful Commands](#useful-commands)**
05. **[Resources](#resources)**
	- [Courses](#courses)
	- [Books](#books)
	- [Cheat Sheet](https://github.com/sayems/kubernetes.examples/blob/master/cheat-sheet.md#kubernetes-cheat-sheet)

&nbsp;

![](https://github.com/sayems/kubernetes.examples/blob/master/images/kubernetes.png)

&nbsp;

## Prerequisites
- Basic Linux command line skills
- Experience with using Docker
- Familiarity with YAML is also a plus

&nbsp;

## Minikube
The simplest and quickest way to get started with Kubernetes is to install [Minikube](https://kubernetes.io/docs/getting-started-guides/minikube/) on your local machine. Minikube is a tool that sets up a single-node cluster that's great for both testing Kubernetes and developing apps locally.

- [macOS](https://github.com/sayems/kubernetes.examples/blob/master/mac.md#requirements)
- [Linux](https://github.com/sayems/kubernetes.examples/blob/master/linux.md#requirements)
- [Windows](https://github.com/sayems/kubernetes.examples/blob/master/windows.md#requirements)

Since Minikube is running locally instead of on a cloud provider, certain provider-specific features like LoadBalancers and PersistentVolumes will not work out-of-the-box. However, you can use NodePort LoadBalancers and HostPath PersistentVolumes.


**NOTE** Minikube doesn't support ```LoadBalancer``` services, so the service will never get an external IP. But you get the IP and port through which you can access service by running ```minikube service kubia-http```.

&nbsp;

### Kubernetes Dashboard

![](https://github.com/sayems/kubernetes.examples/blob/master/images/dashboard.png)

To open the dashboard in your browser when using Minikube to run your Kubernetes cluster, run the following command:

```
$ minikube dashboard
```
The dashboard will open in your default browser. Unlike with GKE, you won’t need to enter any credentials to access it.

&nbsp;

## Useful Commands

| Command | Description |
| --- | --- |
| `kubectl get pods` | List all pods in ps output format. |
| `kubectl get pods -o wide` | List all pods in ps output format with more information (such as node name) |
| `kubectl get replicationcontroller web` | List a single replication controller with specified NAME in ps output format. |
| `kubectl get -o json pod web-pod-13je7` | List a single pod in JSON output format. |
| `kubectl get -f pod.ya ml -o json` | List a pod identified by type and name specified in “pod.ya ml” in JSON output format. |
| `kubectl get -o template pod/web-pod-13je7 --template=` | Return only the phase value of the specified pod. |
| `kubectl get rc,services` | List all replication controllers and services together in ps output format. |
| `kubectl get rc/web service/frontend pods/web-pod-13je7` | List one or more resources by their type and names. |

&nbsp;

## Resources
There are a lot of helpful Kubernetes resources on the internet. This is a short list of my favorites courses and books!

### Courses

- [Scalable Microservices with Kubernetes](https://www.udacity.com/course/scalable-microservices-with-kubernetes--ud615)


### Books

- [Kubernetes in Action](https://www.manning.com/books/kubernetes-in-action)
