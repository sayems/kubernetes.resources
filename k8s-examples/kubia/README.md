Dockerfile Instruction
--

To build the docker image, run the following command from ```kube``` directory:
```
docker build -t sayems/kubia .
```


To run docker container, run the following command:
```
docker run -p 8080:8080 -d sayems/kubia
```
The container will be detached from the console (-d flag), which means it will run in the background.

Now open a browser and navigate to [http://localhost:8080/](http://localhost:8080/) to see the result.

&nbsp;


#### Docker Registry

Now, you can push your image to Docker Hub using ```docker push``` command.
```
docker push sayems/kubia
```

&nbsp;



Deploy on Kubernetes
--

To launch a pod using the container image ```sayems/kubia```, execute the following command:

```
kubectl run kubia --image=sayems/kubia --port=8080 --generator=run/v1

```
We can now see that the pod is running:
```
$ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
kubia   1/1     Running   0          22m
```

Create a **Service** to make the pod accessible from the outside

```
kubectl expose replicationcontroller/kubia --type=NodePort --name kubia-http
```


##### Listing services

```
kubectl get services

NAME         TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes   ClusterIP      10.233.0.1     <none>        443/TCP          4m47s
kubia-http   LoadBalancer   10.233.23.96   <pending>     8080:31420/TCP   3m24s
```

Now open a browser and navigate to [http://minikube-ip:31420/](http://localhost:8080/) to see the result.

