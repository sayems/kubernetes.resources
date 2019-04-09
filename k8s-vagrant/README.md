# Multi-node Kubernetes cluster with Vagrant and Ansible

This is a guide for anyone wanting to setup multi-node kubernetes cluster.

&nbsp;


## Table of contents
- [Architecture](#architecture)
- [Prerequisite](#prerequisite)
- [Installation](#installation)
- [Getting Started](#getting-started)
  - [Troubleshooting](#troubleshooting)
- [Deploy to Kubernetes With Helm](#deployment-to-kubernetes-with-helm)
- [SSH Config](#ssh-config)
- [Using SSHFS to Mount Vagrant on Mac](#using-sshfs-to-mount-vagrant-on-mac)
- [Remote Kubernetes Cluster](#remote-kubernetes-cluster)
- [K8s Dashboard](https://github.com/sayems/kubernetes.resources/wiki/Kubernetes-Dashboard)

&nbsp;


Architecture
--

We will setup a 3 nodes Kubernetes cluster on Ubuntu 18.04, which contains the components below:

| IP            | Hostname   | Role   | Version | OS                 |
|---------------|------------|--------|---------|--------------------|
| 192.168.50.10 | k8s-master | Master | v1.14.0 | Ubuntu 18.04.1 LTS |
| 192.168.50.11 | k8s-node-1 | Node1  | v1.14.0 | Ubuntu 18.04.1 LTS |
| 192.168.50.12 | k8s-node-3 | Node3  | v1.14.0 | Ubuntu 18.04.1 LTS |
| 192.168.50.13 | k8s-node-3 | Node3  | v1.14.0 | Ubuntu 18.04.1 LTS |

&nbsp;

[top](#table-of-contents)

&nbsp;


Prerequisite
--

### Hardware

Minimum Recommended Hardware Requirements

| Processor | CPU    | RAM   | Storage | Internet |
|-----------|--------|-------|---------|----------|
| 64-bit    | 8-Core | 16 GB | 20 GB   | 3 Mbps   |

 

### Operating System:
- macOS 10.13+



### Software

- [Kubernetes 1.14+](https://github.com/kubernetes/kubernetes/)
- [Ansible 2.0+](https://github.com/ansible/ansible/)
- [Vagrant 2.2+](https://github.com/hashicorp/vagrant/)
- [Virtualbox 6.0+](https://www.virtualbox.org/wiki/Downloads)
- [Homebrew 2.0.6+](https://brew.sh/)


&nbsp;


[top](#table-of-contents)
&nbsp;


Installation
--

The best way to get started on a macOS is to use [Homebrew](https://brew.sh/). If you already have [Homebrew](https://brew.sh/) installed, you can install ```virtualbox```, ```vagrant```, ```vagrant-manager```,  ```ansible``` and ```kubectl``` by running the following command
```
brew update && brew install virtualbox && brew install vagrant && brew install vagrant-manager && brew install ansible && brew install kubectl
```

If you don't already have [Homebrew](https://brew.sh/) installed, I would strongly recommend that you install it! It's incredibly useful for installing software dependencies like OpenSSL, MySQL, Postgres, Redis, SQLite, and more.

You can install it by running the following command:
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

[top](#table-of-contents)
&nbsp;



Getting Started
--

##### Clone this github repo:
```
git clone https://github.com/sayems/kubernetes.resources.git
```

Navigate to ```k8s-vagrant``` directory
```
$ cd kubernetes.resources/k8s-vagrant/
```

##### Spin up Kubernetes cluster

Navigate to ```k8s/``` directory and then run the following command to start the Vagrant:

```bash
$ vagrant up
```
Now, wait for kubernetes cluster to be ready. This might take a while to complete.


&nbsp;


#### Troubleshooting

If you get an error like the one below, you might need to remove and re-install all plugins.
```
VirtualBox Guest Additions: Starting.
An error occurred during installation of VirtualBox Guest Additions 6.0.4. Some functionality may not work as intended.
In most cases it is OK that the "Window System drivers" installation failed.
```

Run the following command to remove and re-install all plugins:
```
vagrant plugin uninstall vagrant-vbguest
vagrant destroy -f
vagrant up
vagrant plugin install vagrant-vbguest
```

&nbsp;


#### SSH into ```k8s-master```

You can now ```ssh``` into the **```k8s-master```** by running the following command:
```
$ vagrant ssh k8s-master
```

You can verify the cluster by checking the nodes. Run the following command to list all the connected nodes:
```
vagrant@k8s-master:~$ kubectl get nodes -o wide
```
```
NAME         STATUS   ROLES    AGE   VERSION   INTERNAL-IP     EXTERNAL-IP   OS-IMAGE             
k8s-master   Ready    master   21m   v1.14.0   192.168.50.10   <none>        Ubuntu 18.04.1 LTS   
k8s-node-1   Ready    <none>   18m   v1.14.0   192.168.50.11   <none>        Ubuntu 18.04.1 LTS   
k8s-node-2   Ready    <none>   17m   v1.14.0   192.168.50.12   <none>        Ubuntu 18.04.1 LTS 
k8s-node-3   Ready    <none>   15m   v1.14.0   192.168.50.12   <none>        Ubuntu 18.04.1 LTS 
```

&nbsp;


Check everything worked:

```
vagrant@k8s-master:~$ kubectl get pods -n kube-system
NAME                                 READY   STATUS    RESTARTS   AGE
calico-node-jlxkz                    1/1     Running   0          2m57s
calico-node-ml55w                    1/1     Running   0          6m8s
calico-node-shmsr                    1/1     Running   0          4m29s
coredns-86c58d9df4-8lmjw             1/1     Running   0          6m8s
coredns-86c58d9df4-w5l9m             1/1     Running   0          6m8s
etcd-k8s-master                      1/1     Running   0          5m11s
kube-apiserver-k8s-master            1/1     Running   0          5m28s
kube-controller-manager-k8s-master   1/1     Running   0          5m30s
kube-proxy-8lrsv                     1/1     Running   0          6m8s
kube-proxy-bqw9k                     1/1     Running   0          2m57s
kube-proxy-svdf2                     1/1     Running   0          4m29s
kube-scheduler-k8s-master            1/1     Running   0          5m14s
```

&nbsp;


### Deployment to Kubernetes with Helm

Let's deploy **```Selenium Grid```** on Kubernetes cluster using [Helm](https://helm.sh/). [Helm](https://helm.sh/) is a package manager for Kubernetes that allows developers deploy applications on Kubernetes clusters. 


&nbsp;


#### Installing Helm

Installing Helm is actually pretty straightforward. Follow these steps:
```
$ curl https://raw.githubusercontent.com/helm/helm/master/scripts/get > get_helm.sh
$ chmod 700 get_helm.sh
$ ./get_helm.sh
```

&nbsp;


#### Installing Tiller

```
$ kubectl create serviceaccount --namespace kube-system tiller

serviceaccount/tiller created
```
```
$ kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller

clusterrolebinding.rbac.authorization.k8s.io/tiller-cluster-rule created
```
```
$ helm init --service-account tiller --upgrade

$HELM_HOME has been configured at /Users/sidrah/.helm.

Tiller (the Helm server-side component) has been installed into your Kubernetes Cluster.

Please note: by default, Tiller is deployed with an insecure 'allow unauthenticated users' policy.
To prevent this, run `helm init` with the --tiller-tls-verify flag.
For more information on securing your installation see: https://docs.helm.sh/using_helm/#securing-your-helm-installation
Happy Helming!
```

Now, run the following command
```
$ kubectl get pods -n kube-system
```
```
NAME                                                   READY   STATUS    RESTARTS   AGE
tiller-deploy-57c574bfb8-2f7zw                         1/1     Running   0          2m
```
```
$ kubectl get pods -n kube-system | grep tiller --color
```
```
tiller-deploy-57c574bfb8-2f7zw                         1/1     Running   0          3m
```

[top](#table-of-contents)
&nbsp;

##### Selenium Grid installation
```
helm install --name selenium-grid stable/selenium \
--set chromeDebug.enabled=true \
--set firefoxDebug.enabled=true
```
```
NAME                                                        READY   STATUS    RESTARTS   AGE
pod/selenium-grid-selenium-chrome-debug-77c567d586-ln2wr    1/1     Running   0          3m56s
pod/selenium-grid-selenium-firefox-debug-54d5f8c674-qr4fs   1/1     Running   0          3m56s
pod/selenium-grid-selenium-hub-77667c5c7c-5thrs             1/1     Running   0          3m56s

NAME                                 TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/kubernetes                   ClusterIP      10.96.0.1       <none>        443/TCP          30m
service/selenium-grid-selenium-hub   LoadBalancer   10.106.174.57   <pending>     4444:30171/TCP   3m56s

NAME                                                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/selenium-grid-selenium-chrome-debug    1/1     1            1           3m56s
deployment.apps/selenium-grid-selenium-firefox-debug   1/1     1            1           3m56s
deployment.apps/selenium-grid-selenium-hub             1/1     1            1           3m56s

NAME                                                              DESIRED   CURRENT   READY   AGE
replicaset.apps/selenium-grid-selenium-chrome-debug-77c567d586    1         1         1       3m56s
replicaset.apps/selenium-grid-selenium-firefox-debug-54d5f8c674   1         1         1       3m56s
replicaset.apps/selenium-grid-selenium-hub-77667c5c7c             1         1         1       3m56s
```

Selenium hub will automatically start-up using port 4444 by default. You can view the status of the hub by opening a browser window and navigating to: ```http://k8s-master-ip:30171/grid/console```

For more information on how to run tests on Selenium Grid, please visit this [link](https://github.com/sayems/selenium.grid)


[top](#table-of-contents)
&nbsp;


SSH Config
--
If you want to ```ssh``` into your vagrant box without switching into the project directory and typing ```vagrant ssh k8s-master```. Simply copy ```vagrant ssh-config``` to your ```~/.ssh/config```

```
$ cd kubernetes.resources/k8s-vagrant
$ vagrant ssh-config >> ~/.ssh/config
```
This will allow you to use ```ssh k8s-master``` from anywhere.

[top](#table-of-contents)
&nbsp;


Using SSHFS to Mount Vagrant on Mac
--
The first step is to install ```osxfuse``` and ```sshfs``` via Homebrew:
```
brew cask install osxfuse
brew install sshfs
```

Now, create a local directory in your ```~/Desktop```
```
$ mkdir ~/Desktop/k8s
```
Mount your remote directory in ```~/Desktop/k8s```
```
$ sshfs vagrant@k8s-master:/home/vagrant ~/Desktop/k8s
```

[top](#table-of-contents)
&nbsp;


Remote Kubernetes Cluster
--

To connect to a remote Kubernetes cluster from you Mac, you'll need copy the Kubernetes credentials to your home directory as shown below.

```bash
scp -r vagrant@k8s-master:/home/vagrant/.kube ~/.kube/
```

That’s all you have to do. Your local Kubectl should be able to connect with the remote Kubernetes cluster now.


[top](#table-of-contents)
&nbsp;



#### Stop Vagrant

Stop the vagrant machine now.
```
$ vagrant halt
```

&nbsp;


#### Destroy Vagrant

Stops and deletes all traces of the vagrant machine

```
$ vagrant destroy
```

Other useful commands are ```suspend```, ```destroy``` etc.


&nbsp;

[top](#table-of-contents)


