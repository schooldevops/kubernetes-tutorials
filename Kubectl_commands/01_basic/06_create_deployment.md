# kubectl create deployment 

- 지정된 이름으로 deployment 를 생성한다. 

```py
$ kubectl create deployment NAME --image=image -- [COMMAND] [args...]
```

- busybox 이미지를 실행하며 my-dep 으로 이름된 deployment 를 생성한다. 

```py
kubectl create deployment my-dep --image=busybox
```

- command 와 함께 deployment 를 생성한다. 

```py
kubectl create deployment my-dep --image=busybox -- date
```

- my-dep 으로 이름된 deployment 를 생성하고 3개의 리플리카로 nginx image를 실행한다. 

```py
kubectl create deployment my-dep --image=nginx --replicas=3
```

- my-dep 으로 이름된 deployment 를 생성하고, busybox 이미지를 실행하고 포트 5701을 노출한다. 
