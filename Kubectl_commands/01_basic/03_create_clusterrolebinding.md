# kubectl create clusterrolebinding 

- 특정 클러스터 롤에 대해 클러스터 롤 바인딩을 생성한다. 

```py
$ kubectl create clusterrolebinding NAME --clusterrole=NAME [--user=username] [--group=groupname] [--serviceaccount=namespace:serviceaccountname] [--dry-run=server|client|none]
```

- cluster-admin 클러스터 역할을 사용하여 user1, user2 및 group1 에 대한 클러스터 롤 바인딩 생성 

```py
kubectl create clusterrolebinding cluster-admin --clusterrole=cluster-admin --user=user1 --user=user2 --group=group1
```
