## Demo Deployment

Let's learn about Kubernetes by deploying a simple web application on a Kubernetes cluster.


&nbsp;


### Create Kubernetes Objects

###### Create ```~/kubernetes.examples/demo/helloworld.yml```
```
apiVersion: v1
kind: Pod
metadata:
  name: helloworld
  labels:
    app: helloworld
spec:
  containers:
  - name: docker-demo
    image: sayems/docker-demo
    ports:
    - name: nodejs-port
      containerPort: 3000
```

###### Create ```~/kubernetes.examples/demo/helloworld-service.yml```
```
apiVersion: v1
kind: Service
metadata:
  name: helloworld-service
spec:
  ports:
  - port: 80
    targetPort: nodejs-port
    protocol: TCP
  selector:
    app: helloworld
  type: LoadBalancer
```

###### Create ```~/kubernetes.examples/demo/helloworld-nodeport-service.yml```
```
apiVersion: v1
kind: Service
metadata:
  name: helloworld-service
spec:
  ports:
  - port: 31001
    nodePort: 31001
    targetPort: nodejs-port
    protocol: TCP
  selector:
    app: helloworld
  type: NodePort
```



### Create Deployment

###### Navigate to the directory where you store ```helloworld.yml``` file. 
```
cd ~/kubernetes.examples/demo/first-app
``` 

###### Run the following command to deploy app
```
kubectl create -f helloworld.yml
```

Output:
```
pod "helloworld" created
```

&nbsp;

## Accessing Pods and Services from the Outside

#### (1) Port Forward
```
kubectl port-forward helloworld 8081:3000
```

###### Open new terminal tab and run the following command:
```
curl localhost:8081
```

Output:
```
Hello World!
```

&nbsp;

#### (2) Expose
```
kubectl expose pod helloworld --type=NodePort --name helloworld-service
```

###### Run the following command ```kubectl get services``` to get the ```expose``` services.
```
kubectl get services
```

```
NAME                 TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
helloworld-service   NodePort    10.102.191.85   <none>        3000:30944/TCP   2m
kubernetes           ClusterIP   10.96.0.1       <none>        443/TCP          3d
```

##### Describe specific service
```
kubectl describe service helloworld-service
```
```
Name:                     helloworld-service
Namespace:                default
Labels:                   app=helloworld
Annotations:              <none>
Selector:                 app=helloworld
Type:                     NodePort
IP:                       10.102.191.85
Port:                     <unset>  3000/TCP
TargetPort:               3000/TCP
NodePort:                 <unset>  30944/TCP
Endpoints:                10.32.0.2:3000
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```

###### View it on Browser
```
http://<Kube-Master IP adress>:30944
```
```
Hello World!
```

&nbsp;

#### (3) LoadBalancer

###### Create ```~/kubernetes.examples/demo/helloworld-service.yml```
```
apiVersion: v1
kind: Service
metadata:
  name: helloworld-service
spec:
  ports:
  - port: 80
    targetPort: nodejs-port
    protocol: TCP
  selector:
    app: helloworld
  type: LoadBalancer
```



&nbsp;


