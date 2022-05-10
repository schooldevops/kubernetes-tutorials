# kubectl create clusterrole 

- 클러스터 롤을 생성한다. 

```py
$ kubectl create clusterrole NAME --verb=verb --resource=resource.group [--resource-name=resourcename] [--dry-run=server|client|none]
```

- pod-reader 이라는 이름으로 클러스터 롤을 생성한다. 
- 이는 사용자에게 "get", "watch", "list" 를 pod에 대해서 수행할 수 있도록 해준다. 

```py
kubectl create clusterrole pod-reader --verb=get,list,watch --resource=pods
```

- pod-reader 로 이름된 클러스터 롤을 생성한다. 
- ResourceName 이 이때 지정된다. 
  
```py
kubectl create clusterrole pod-reader --verb=get --resource=pods --resource-name=readablepod --resource-name=anotherpod
```

- foo로 이름된 클러스터 롤을 생성하여 API Group 을 지정했다.
  
```py
kubectl create clusterrole foo --verb=get,list,watch --resource=rs.extensions
```

- foo 이름으로 클러스터 롤을 생성한다. 여기에 SubResource로 지정된다.

```py
kubectl create clusterrole foo --verb=get,list,watch --resource=pods,pods/status
```

- NonResourceURL 이 지정된 클러스터 롤을 생성하며 이름은 foo이다. 

```py
kubectl create clusterrole "foo" --verb=get --non-resource-url=/logs/*
```

- AggregationRune 이 지정된 "monitoring" 이라는 이름의 클러스터 롤을 생성한다. 

```py
kubectl create clusterrole monitoring --aggregation-rule="rbac.example.com/aggregate-to-monitoring=true"
```
