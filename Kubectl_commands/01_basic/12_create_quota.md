# kubectl create quota 

- 지정된 이름, hard limits, 선택적 스콥으로 지정된 리소스 쿼타를 생성한다. 

```py
$ kubectl create quota NAME [--hard=key1=value1,key2=value2] [--scopes=Scope1,Scope2] [--dry-run=server|client|none]
```

- my-quota 로 이름된 quota 리소스를 새로 생성한다. 

```py
kubectl create quota my-quota --hard=cpu=1,memory=1G,pods=2,services=3,replicationcontrollers=2,resourcequotas=1,secrets=5,persistentvolumeclaims=10
```

- best-effort 로 이름된 리소스 쿼타를 새로 생성한다. 

```py
kubectl create quota best-effort --hard=pods=100 --scopes=BestEffort
```

