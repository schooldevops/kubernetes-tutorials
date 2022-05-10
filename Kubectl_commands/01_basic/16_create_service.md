# kubectl create service

- 지정된 하위 커맨드를 이용하여 서비스를 생성한다. 

```py
$ kubectl create service
```

# kubectl create service clusterip

- ClusterIP 서비스를 생성하며 지정된 이름으로 생성한다. 

```py
$ kubectl create clusterip NAME [--tcp=<port>:<targetPort>] [--dry-run=server|client|none]
```

- my-cs 로 이름지어진 ClusterIP 를 생성한다. 

```py
kubectl create service clusterip my-cs --tcp=5678:8080
```

- my-cs 로 이름지어진 ClusterIP 서비스를 생성한다. (headerless 모드로 생성한다.)

```py
kubectl create service clusterip my-cs --clusterip="None"
```

# kubectl create service externalname

- 지정된 이름으로 ExternalName 서비스를 생성한다. 

- ExternalName 서비스는 외부 DNS 주소를 참조한다. 이는 pod에 매핑되지 않는다. 
- 애플리케이션 작성자가 플랫폼 외부, 다른 클러스터 또는 로컬에 존재하는 서비스로 참조할 수 있다. 

```py
$ kubectl create externalname NAME --external-name external.name [--dry-run=server|client|none]
```

- ExternalName 서비스를 새성한다. 이름은 my-ns이다. 

```py
kubectl create service externalname my-ns --external-name bar.com
```

# kubectl create service loadbalancer 

- 지정된 이름의 LoadBalancer 서비스를 생성한다. 

```py
$ kubectl create loadbalancer NAME [--tcp=port:targetPort] [--dry-run=server|client|none]
```

- my-alb 이름으로 LoadBalancer 서비스를 생성한다. 

```py
kubectl create service loadbalancer my-lbs --tcp=5678:8080
```

# kubectl create service nodeport

- 지정된 이름으로 NodePort 서비스를 생성한다. 

```py
$ kubectl create nodeport NAME [--tcp=port:targetPort] [--dry-run=server|client|none]
```

- my-ns 로 이름지어진 NodePort 서비스를 생성한다. 

```py
kubectl create service nodeport my-ns --tcp=5678:8080
```

