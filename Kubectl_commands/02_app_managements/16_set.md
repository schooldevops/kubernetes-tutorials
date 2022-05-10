# set

- 어플리케이션 리소스를 설정한다. 
- 이러한 명령은 기존 애플리케이션 리소스를 변경하는 데 도움이 된다. 

## Usage

```py
$ kubectl set SUBCOMMAND
```

# set env

- pod 템플릿에서 환경 변수를 업데이트한다. 
- 하나 이상의 포드, 포드 템플릿에 환경 변수 정의를 나열한다. 
- Add, update, remove를 수행하며 하나 이상의 포드 템플릿 (복제 컨트롤러 또는 배포 구성 내) 에서 컨테이너 환경 변수 정의에 대해서 수행한다. 
- 지정된 팟(Pod) 또는 팟(Pod) 템플릿의 모든 컨테이너 또는 와일드 카드와 일치하는 컨테이너에 대한 환경 변수 정의를 보거나 수정한다. 

- "--env -" 가 전달되면 표준 env 구문을 사용하여 STDIN 에서 환경 변수를 읽을 수 있다. 

- 가능한 리소스는 다음과 같다. 

- pod (po), replicationcontroller (rc), deployment (deploy), daemonset (ds), statefulset (sts), cronjob (cj), replicaset (rs)

## Usage

```py
$ kubectl set env RESOURCE/NAME KEY_1=VAL_1 ... KEY_N=VAL_N
```

- 새로운 환경 변수로 registry  배포를 업데이트한다. 

```py
kubectl set env deployment/registry STORAGE_DIR=/local
```

- sample-build 배포에 정의된 환경 변수 목록을 가져온다. 

```py
kubectl set env deployment/sample-build --list
```

- 모든 pod에서 정의된 환경 변수 목록 

```py
kubectl set env pods --all --list
```

- 수정된 배포를 YAML 로 출력하고 서버의 개체를 변경하지 않는다. 

```py
kubectl set env deployment/sample-build STORAGE_DIR=/data -o yaml
```

- ENV=prod 가 되도록 프로젝트의 모든 복제 컨트롤러에 있는 모든 컨테이너를 업데이트한다. 

```py
kubectl set env rc --all ENV=prod
```

- secret에서 환경 변수를 임포트한다. 

```py
kubectl set env --from=secret/mysecret deployment/myapp
```

- config map 에서 prefix를 통해 환경 변수를 임포트한다. 

```py
kubectl set env --from=configmap/myconfigmap --prefix=MYSQL_ deployment/myapp
```

- config map으로 부터 지정된 키를 임포트한다. 

```py
kubectl set env --keys=my-example-key --from=configmap/myconfigmap deployment/myapp
```

- 모든 deployment 설정에서 'c1' 컨테이너의 환경 변수 ENV 를 제거한다. 

```py
kubectl set env deployments --all --containers="c1" ENV-
```

- 디스크의 배포 정의에서 환경 변수 ENV 를 제거하고 서버의 배포 구성을 업데이트 한다. 

```py
kubectl set env -f deploy.json ENV-
```

- 일부 로컬 셀 환경을 서버의 배포 구성으로 설정한다. 

```py
env | grep RAILS_ | kubectl set env -e - deployment/registry
```

# set image

- 리소스의 컨테이너 이미지를 업데이트한다. 
- 가능한 리소스는 다음과 같다. 
- pod (po), replicationcontroller (rc), deployment (deploy), daemonset (ds), statefulset (sts), cronjob (cj), replicaset (rs)

## Usage

```py
$ kubectl set image (-f FILENAME | TYPE NAME) CONTAINER_NAME_1=CONTAINER_IMAGE_1 ... CONTAINER_NAME_N=CONTAINER_IMAGE_N
```

- nginx 컨테이너 이미지의 deployments 를 nginx:1.9.1 로 설정한다. 
- 그리고 busybox 컨테이너 이미지는 busybox 이다. 

```py
kubectl set image deployment/nginx busybox=busybox nginx=nginx:1.9.1
```

- 모든 배포의 업데이트 그리고 rc의 nginx 컨테이너의 이미지는 nginx:1.9.1 로 업데이트한다. 

```py
kubectl set image deployments,rc nginx=nginx:1.9.1 --all
```

- daemonset abc 의 모든 컨테이너의 이미지를 nginx:1.9.1 로 업데이트한다. 

```py
kubectl set image daemonset abc *=nginx:1.9.1
```

- 서버에 충돌하지 않고 로컬 파일에서 nginx 컨테이너 이미지를 업데이트한 결과(yaml) 인쇄

```py
kubectl set image -f path/to/file.yaml nginx=nginx:1.9.1 --local -o yaml
```

## set resources 

- 지정된 컴퓨트 리소스가 필요하다. (CPU, memory) 이는 pod template에 정의한다. 
- 만약 pod가 성공적으로 스케줄 된다면 이는 리소스 요청의 양을 보장한다. 그러나 지정된 리밋으로 버스트업 된다. 

- 각 컴퓨터 리소스에 대해서 만약 limit이 지정되었고 request가 누락되었다면 요청의 기본은 limit으로 제한될 것이다. 
- 가능한 리소스는 다음과 같다 (대소문자 구분 안함) : 지원되는 리소스의 전체 목록을 본다면 'kubectl api-resources'를 사용하라. 

## Usage

```py
$ kubectl set resources (-f FILENAME | TYPE NAME) ([--limits=LIMITS & --requests=REQUESTS]
```

- 배포 nginx 컨테이너 CPU 제한을 "200m"으로 설정하고 메모리를 "512Mi"로 설정한다. 

```py
kubectl set resources deployment nginx -c=nginx --limits=cpu=200m,memory=512Mi
```

- 리소스 요청과 limit을 모든 nginx 컨테이너에 설정한다. 

```py
kubectl set resources deployment nginx --limits=cpu=200m,memory=512Mi --requests=cpu=100m,memory=256Mi
```

- nginx 컨테이너에서 리소스에 대해 리소스 요청을 제거한다. 

```py
kubectl set resources deployment nginx --limits=cpu=0,memory=0 --requests=cpu=0,memory=0
```

- 서버에 충돌하지 않고 로컬에서 nginx 컨테이너 제한을 업데이트 한 결과(yaml 형식)를 인쇄한다. 

```py
kubectl set resources -f path/to/file.yaml --limits=cpu=200m,memory=512Mi --local -o yaml
```

https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#set