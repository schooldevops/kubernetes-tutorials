# Rollout

- 리소스의 롤아웃을 관리한다. 
- 유효한 리소스 타입은 다음과 같다. 
  - deployments
  - daemonsets
  - statefulsets

## Usage

```py
$ kubectl rollout SUBCOMMAND
```

- 이전 디플로이를 롤백한다. 

```py
kubectl rollout undo deployment/abc
```

- daemonset의 롤아웃 상태를 체크한다. 

```py
kubectl rollout status daemonset/foo
```

# rollout history

- 이전 롤아웃 리비젼과 설정을 확인한다. 

## Usage

```py
$ kubectl rollout history (TYPE NAME | TYPE/NAME) [flags]
```

- 배포의 롤아웃 히스토리를 확인한다. 

```py
kubectl rollout history deployment/abc
```

- daemonset 리비젼 3의 상세를 확인한다. 

```py
kubectl rollout history daemonset/abc --revision=3
```

# puase

- 이전 제공된 리소스를 paused로 마크한다. 
- paused로 마크된 리소스는 컨트롤러에 의해서 체크되지 않는다. 
- 'kubectl rollout resume' 를 이용하여 정지된 리소스를 재실행할 수 있다. 
- 현재 deployment 만 paused가 허용된다. 

## Usage

```py
$ kubectl rollout pause RESOURCE
```

- nginx 배포를 일시 중지됨으로 표시
- 현재 배포 상태는 기능을 계속한다. 배포에 대한 새 업데이트
- 배포가 일시 중지되는 한 영향을 미치지 않는다. 

```py
kubectl rollout pause deployment/nginx
```

# restart

- 리소스를 리스타트 한다. 
- 리소스 롤아웃은 재시작 될 것이다. 

## Usage

```py
$ kubectl rollout restart RESOURCE
```

- deployment를 리스타트 한다. 

```py
kubectl rollout restart deployment/nginx
```

- daemon set을 재시작한다.

```py
kubectl rollout restart daemonset/abc
```

# resume

- 정지된 리소스를 다시 진행한다. 
- 정지된 리소스는 컨틀롤러에 의해서 체크되지 않는다. 
- 리소스 재진행으로 인해서 다시 체크가 시작된다. 
- 현재 오직 deployments는 resumed를 지원한다. 

## Usage

```py
$ kubectl rollout resume RESOURCE
```

- 이미 정지된 디플로이먼트를 재시작한다. 

```py
kubectl rollout resume deployment/nginx
```

# status

- rollout 의 상태를 보여준다. 
- 기본적으로 'rollout status' 이 가장 최근의 롤아웃 상태를 보여주며, 이는 아직 롤아웃이 완료되지 건의 내용이다. 
- 만약 롤아웃이 완료될 때까지 기다리지 않으려면 --watch=false 를 사용할 수 있다. 
- 그 사이 롤아웃이 시작되면 '롤아웃 상태'가 최신 버젼을 계속 보게 된다. 
- 만약 특정 게정판에 고정하고 다른 개정판에서 롤오버 되는 경우 중단하려면 --revision=N을 사용하라. 
- 여기서 N 은 감시해야 하는 개정판이다. 

## Usage

```py
$ kubectl rollout status (TYPE NAME | TYPE/NAME) [flags]
```

- 배포의 롤아웃 상태를 워치 한다. 

```py
kubectl rollout status deployment/nginx
```

# Undo

- 이전 롤아웃을 롤백한다. 

## Usage

```py
$ kubectl rollout undo (TYPE NAME | TYPE/NAME) [flags]
```

- 이전 deployment 에 대해서 롤백을 수행한다. 

```py
kubectl rollout undo deployment/abc
```

- revision 3으로 데몬셋을 롤백한다. 

```py
kubectl rollout undo daemonset/abc --to-revision=3
```

- dry-run 을 통해서 이전 배포를 롤백한다. 

```py
kubectl rollout undo --dry-run=server deployment/abc
```