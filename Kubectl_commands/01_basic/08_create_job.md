# kubectl create job

- 지정된 이름으로 job을 생성한다. 

```py
$ kubectl create job NAME --image=image [--from=cronjob/name] -- [COMMAND] [args...]
```

- job을 생성한다. 

```py
kubectl create job my-job --image=busybox
```

- command와 함께 job을 생성한다. 

```py
kubectl create job my-job --image=busybox -- date
```

- "a-cronjob" 으로 이름된 cron-job 으로 부터 job을 생성한다. 

```py
kubectl create job test-job --from=cronjob/a-cronjob
```