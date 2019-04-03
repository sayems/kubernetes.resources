This example assumes you have a k8s cluster up and running on either Minikube or AWS with kubectl installed. 

&nbsp;

Table of contents
--
- [Quick Start](#quick-start)
- [Demo](demo)
- [Kubia](kubia)
- [Tomcat](tomcat)



&nbsp;


Quick Start
--


Let's run a simple ```Hello World``` app on Kubernetes, execute the following command:

```
kubectl run hello-minikube --image=k8s.gcr.io/echoserver:1.4 --port=8080
```
We can now see that the pod is running:
```
NAME                                  READY   STATUS    RESTARTS   AGE
pod/hello-minikube-5c856cbf98-r8b27   1/1     Running   0          12m

NAME                             READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/hello-minikube   1/1     1            1           12m

```

Next, we're going to create a **Service** to make the deployment accessible from the outside, execute the following command to expose ```hello-minikube``` as a NodePort:

```
kubectl expose deployment hello-minikube --type=NodePort
```

Now, execute the following command to list all the services

```
kubectl get services

NAME             TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
hello-minikube   NodePort    10.233.1.189   <none>        8080:30784/TCP   2m1s
kubernetes       ClusterIP   10.233.0.1     <none>        443/TCP          9m8s
```

Now open a browser and navigate to [http://minikube-ip:30784/](http://172.17.8.101:30784/) to see the result.
