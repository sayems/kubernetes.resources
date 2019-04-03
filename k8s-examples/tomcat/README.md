Deploy Tomcat on Kubernetes
--

Let's run a ```tomcat-deployment.yml``` app on Kubernetes, execute the following command:

```
cd kubernetes.resources/k8s-examples/tomcat
kubectl apply -f ./tomcat-deployment.yml

```
We can now see that the pod status is ```ContainerCreating```:
```
$ kubectl get all

NAME                                     READY   STATUS              RESTARTS   AGE
pod/tomcat-deployment-69d8476668-cgw7s   0/1     ContainerCreating   0          10s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.233.0.1   <none>        443/TCP   14h

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/tomcat-deployment   0/1     1            0           10s

NAME                                           DESIRED   CURRENT   READY   AGE
replicaset.apps/tomcat-deployment-69d8476668   1         1         0       10s

```

Create a **Service** to make the pod accessible from the outside

```
kubectl expose deployment  tomcat-deployment --type=NodePort 
```


##### Listing services

```
kubectl get services

NAME                TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes          ClusterIP   10.233.0.1     <none>        443/TCP          14h
tomcat-deployment   NodePort    10.233.57.92   <none>        8080:31101/TCP   15s
```

Now open a browser and navigate to [http://minikube-ip:21101/](http://172.17.8.101:31101/) to see the result. 

**NOTE:** You can you ```minikube service tomcat-deployment --URL``` to get the full URL

