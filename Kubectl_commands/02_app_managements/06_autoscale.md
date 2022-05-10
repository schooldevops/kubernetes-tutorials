# autoscale

- autoscaler 를 생성한다. 이는 자동적으로 선택되고 pod의 수를 지정할 수 있다. 
- 디플로이먼트, 리플리카 셋, stateful set 혹은 이름에 의한 replication controller 그리고 autscaler를 생성한다. 
- 자동 확장 처리는 필요에 따라 시스템 내에 배포된 포드 수를 자동으로 늘리거나 줄일 수 있다. 

## Usage

```py
$ kubectl autoscale (-f FILENAME | TYPE NAME | TYPE/NAME) [--min=MINPODS] --max=MAXPODS [--cpu-percent=CPU]
```

- auto scale 를 'foo'  디플로이먼트를 확장한다. 
- 파드 개수를 2 ~ 10개로 지정했다. 
- 대상 cpu 사용율이 지정되지 않았으므로 기본 자동 크기 조정 정책이 사용된다. 

```py
kubectl autoscale deployment foo --min=2 --max=10
```

- replication controller가 'foo' 인 대상을 auto scale 한다. 
- 파드 수를 1 ~ 5로 잡고 CPU 사용율이 80% 으로 잡았다. 

```py
kubectl autoscale rc foo --max=5 --cpu-percent=80
```