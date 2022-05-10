# kubectl create priorityclass 

- 지정된 이름, 값, globalDefault 및 설명을 사용하여 우선 순위 클래스를 만든다. 

```py
$ kubectl create priorityclass NAME --value=VALUE --global-default=BOOL [--dry-run=server|client|none]
```

- high-priority 로 이름된 priority class 를 생성한다. 

```py
kubectl create priorityclass high-priority --value=1000 --description="high priority"
```

- default-priority 로 이름된 priority class 를 생성하며 글로벌 기본 priority 로 생성된다. 

```py
kubectl create priorityclass default-priority --value=1000 --global-default=true --description="default priority"
```

- 우선 순위가 낮은 포드를 선점할 수 없는 high-priority라는 우선 순위 클래스를 만든다. 

```py
kubectl create priorityclass high-priority --value=1000 --description="high priority" --preemption-policy="Never"
```

