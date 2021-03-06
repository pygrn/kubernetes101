## Create the namespace
```bash
$ kubectl create namespace pythongirona
namespace/pythongirona created
```


## Create the deployment

Create flasktest `deployment` with:
```bash
$ kubectl apply -f demo/deployment.yaml 
deployment.extensions/flasktest created
```

![Deployment screenshoot](./screenshots/0-deployment.png)

### POD detail
![POD detail](./screenshots/0-pod-0.png)
![POD detail](./screenshots/0-pod-1.png)


## Create the service

Create flasktest `service` with:
```bash
$ kubectl apply -f demo/service.yaml 
service/flasktest created
```
![Deployment screenshoot](./screenshots/1-service.png)

Now our `deployment` is "grouped" and accessible over this new `service`, and the `service` uses the **local** IP `10.43.152.90`.

So, from our cluster it can reached it with:
```bash
rolf@k8s-2:~$ curl 10.43.152.90
Hello World!
```


## Create the ingress

Create flasktest `ingress` with:
```
$ kubectl apply -f demo/ingress.yaml 
ingress.extensions/flasktest-ingress created
```

![Deployment screenshoot](./screenshots/2-ingress.png)

Now our `service` is exposed using `hello.env.pythongirona.cat`, so from internet it's reachable:
```bash
$ curl hello.env.pythongirona.cat
Hello World! 
```

## Increase replicas

Now we're going to increase the replicas definition of our `deployment` to `3`.

```bash
$ kubectl get pods --namespace="pythongirona"
NAME                        READY   STATUS    RESTARTS   AGE
flasktest-f6bd9b855-tgrq8   1/1     Running   0          55m

$ sed -i 's/replicas: 1/replicas: 3/g' demo/deployment.yaml

$ kubectl apply -f demo/deployment.yaml 
deployment.extensions/flasktest configured

$ kubectl get pods --namespace="pythongirona"
NAME                        READY   STATUS    RESTARTS   AGE
flasktest-f6bd9b855-2zjx5   1/1     Running   0          10s
flasktest-f6bd9b855-tgrq8   1/1     Running   0          55m
flasktest-f6bd9b855-tkljx   1/1     Running   0          10s

$ kubectl describe service flasktest --namespace="pythongirona"
Name:              flasktest
Namespace:         pythongirona
Labels:            <none>
Annotations:       kubectl.kubernetes.io/last-applied-configuration:
                     {"apiVersion":"v1","kind":"Service","metadata":{"annotations":{},"name":"flasktest","namespace":"pythongirona"},"spec":{"ports":[{"name":"...
Selector:          name=flasktestbackend
Type:              ClusterIP
IP:                10.43.152.90
Port:              http  80/TCP
TargetPort:        5000/TCP
Endpoints:         10.42.0.20:5000,10.42.1.49:5000,10.42.2.25:5000
Session Affinity:  None
Events:            <none>

```