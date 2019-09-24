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