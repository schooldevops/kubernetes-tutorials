# Scale

- 새로운 크기의 deployment, replica set, replication controller, stateful set 등의 크기를 변경한다. 
- scale은 또한 사용자가 Scale 작업에 대한 하나 이상의 전제 조건을 지정할 수 있다. 
- 만약 --current-replicas 혹은 --resource-version 이 지정된다면 scale 이 시도되기 전에 검토가 이루어진다. 
- 그리고 스케일이 서버로 전송될 때 전제 조건이 참임을 보장한다. 

## Usage

```py
$ kubectl scale [--resource-version=version] [--current-replicas=count] --replicas=COUNT (-f FILENAME | TYPE NAME)
```

- foo 로 설정된 리플리카셋을 3으로 확장한다. 

```py
kubectl scale --replicas=3 rs/foo
```

- foo.yaml 에 타입과 이름으로 식별된 리소스의 스케일을 3으로 변경한다. 

```py
kubectl scale --replicas=3 -f foo.yaml
```

- 만약 현재 mysql의 크기가 2로 배포 되었다면 mysql 을 3으로 설정한다. 

```py
kubectl scale --current-replicas=2 --replicas=3 deployment/mysql
```

- web으로 이름된 복수 리플리케이션 컨트롤러를 스케일 변경한다. 

```py
kubectl scale --replicas=5 rc/foo rc/bar rc/baz
```

- web이라고 이름지어진 stateful set 의 스케일을 3으로 지정한다. 

```py
kubectl scale --replicas=3 statefulset/web
```

