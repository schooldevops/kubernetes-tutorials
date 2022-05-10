# kubectl create cronjob 

- 지정된 이름으로 cronjob을 생성한다. 

```py
$ kubectl create cronjob NAME --image=image --schedule='0/5 * * * ?' -- [COMMAND] [args...]
```

- cron job을 생성한다. 

```py
kubectl create cronjob my-job --image=busybox --schedule="*/1 * * * *"
```

- command 와 함께 cron job을 생성한다. 

```py
kubectl create cronjob my-job --image=busybox --schedule="*/1 * * * *" -- date
```
