# kubectl create poddisruptionbudget 

- 지정된 이름, 셀렉터 그리고 원하는 최소한의 pods를 사용하여 pod중단 예산을 생성한다. 

```py
$ kubectl create poddisruptionbudget NAME --selector=SELECTOR --min-available=N [--dry-run=server|client|none]
```

- my-pdb라는 이름의 포드 중단 예산을 생성하여 app=rails 레이블이 #인 모든 포드를 선택하고 그 중 적어도 하나가 특정 시점에 사용 가능하도록 요구한다. 

```py
kubectl create poddisruptionbudget my-pdb --selector=app=rails --min-available=1
```

- my-pdb 라는 이름의 포드 중단 예산을 생성하여 app=nginx 레이블이 있는 모든 포드를 선택하고 선택된 포드의 최소 절반이 특정 시점에 사용 가능하도록 요구한다. 

```py
kubectl create pdb my-pdb --selector=app=nginx --min-available=50%
```

