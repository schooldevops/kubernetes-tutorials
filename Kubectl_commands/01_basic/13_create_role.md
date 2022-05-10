# kubectl create role 

- 단일 rule로 롤을 생성한다. 

```py
$ kubectl create role NAME --verb=verb --resource=resource.group/subresource [--resource-name=resourcename] [--dry-run=server|client|none]
```

- pod-reader 로 이름된 롤을 생성한다. 이는 "get", "watch" 그리고 "list" 를 pods에 수행할 수 있도록 한다. 

```py
kubectl create role pod-reader --verb=get --verb=list --verb=watch --resource=pods
```

- pod-reader로 이름된 롤을 만들고 지정된 ResourceName으로 생성한다. 

```py
kubectl create role pod-reader --verb=get --resource=pods --resource-name=readablepod --resource-name=anotherpod
```

- foo 로 이름된 롤을 생성한다. 이때 API Group을 지정된 형태로 생성한다. 

```py
kubectl create role foo --verb=get,list,watch --resource=rs.extensions
```

- SubResource로 지정된 foo로 이름된 롤을 만든다. 

```py
kubectl create role foo --verb=get,list,watch --resource=pods,pods/status
```