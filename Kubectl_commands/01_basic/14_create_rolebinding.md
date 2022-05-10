# kubectl create rolebinding

- 틀정 role 혹은 cluster role 에 대해서 롤 바인딩을 생성한다. 

```py
$ kubectl create rolebinding NAME --clusterrole=NAME|--role=NAME [--user=username] [--group=groupname] [--serviceaccount=namespace:serviceaccountname] [--dry-run=server|client|none]
```

- user1, user2 그리고 gorup1 에 대한 롤 바인딩을 생성하고 admin cluster role을 이용한다.

```py
kubectl create rolebinding admin --clusterrole=admin --user=user1 --user=user2 --group=group1
```