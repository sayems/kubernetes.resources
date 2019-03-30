# Multi-node Kubernetes cluster with Vagrant and Ansible

This is a guide for anyone wanting to setup multi-node kubernetes cluster.

&nbsp;


## Table of contents
- [Prerequisite](#prerequisite)
- [Installation](#installation)
- [Getting Started](#getting-started)
  - [Troubleshooting](#troubleshooting)
- [Deploy to Kubernetes With Helm](#deployment-to-kubernetes-with-helm)


&nbsp;

Prerequisite
--

- [Vagrant](https://github.com/sayems/vagrant.resources)


[top](#table-of-contents)
&nbsp;


Installing ```vagrant``` on Mac
--

The best way to get started on a macOS is to use [Homebrew](https://brew.sh/). If you already have [Homebrew](https://brew.sh/) installed, you can install ```virtualbox```, ```vagrant``` and ```vagrant-manager``` by running the following command
```
brew install virtualbox && brew install vagrant && brew install vagrant-manager
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
vagrant@k8s-master:~$ kubectl get nodes
```
```
NAME         STATUS   ROLES    AGE     VERSION
k8s-master   Ready    master   18m     v1.13.3
node-1       Ready    <none>   12m     v1.13.3
node-2       Ready    <none>   6m22s   v1.13.3
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
NAME:   selenium-grid
LAST DEPLOYED: Sat Mar  2 19:01:06 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                                                   READY  STATUS             RESTARTS  AGE
selenium-grid-selenium-chrome-debug-679d8444c8-nvr4q   0/1    Pending            0         0s
selenium-grid-selenium-firefox-debug-5ddd687df8-fb78t  0/1    Pending            0         0s
selenium-grid-selenium-hub-6c8549fb59-m56sn            0/1    ContainerCreating  0         0s

==> v1/Service
NAME                        TYPE          CLUSTER-IP    EXTERNAL-IP  PORT(S)         AGE
selenium-grid-selenium-hub  LoadBalancer  100.67.61.28  <pending>    4444:32738/TCP  0s

==> v1beta1/Deployment
NAME                                  READY  UP-TO-DATE  AVAILABLE  AGE
selenium-grid-selenium-chrome-debug   0/1    1           0          0s
selenium-grid-selenium-firefox-debug  0/1    1           0          0s
selenium-grid-selenium-hub            0/1    1           0          0s
```

Selenium hub will automatically start-up using port 4444 by default. You can view the status of the hub by opening a browser window and navigating to: ```http://[EC2 PublicIP]:32738/grid/console```

For more information on how to run tests on Selenium Grid, please visit this [link](https://github.com/sayems/selenium.grid)


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


[top](#table-of-contents)
&nbsp;
