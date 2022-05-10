# kubectl create configmap

- config map 을 파일, 디렉토리, 특정 리터럴 값을 기반으로 생성한다. 
- 단일 config map 은 하나 이상의 키/값 쌍을 패키징 하 ㄹ수 있다. 
- 파일 기반으로 구성 맵을 생성할 때 키는 파일의 기본 이름으로 설정되고 값은 기본적으로 파일 콘텐츠로 설정된다. 
- 기본 이름이 잘못된 키인 경우 대체 키를 지정할 수 있다. 

- 디렉토리를 기반으로 구성 맵을 생성할 때 기본 이름이 디렉토리의 유효한 키인 각 파일은 구성 맵에 패키징된다. 
- 일반 파일을 제외한 모든 디렉토리 항목은 무시된다. (예: 하위 디렉토리, 심볼릭 링크, 장치, 파이프 등)

```py
$ kubectl create configmap NAME [--from-file=[key=]source] [--from-literal=key1=value1] [--dry-run=server|client|none]
```

- my-config 로 이름된 config map를 새로 생성하며 bar 폴더를 기반으로 생성한다. 

```py
kubectl create configmap my-config --from-file=path/to/bar
```

- my-config 로 이름된 config map를 새로 생성하며 disk의 기본 이름 대신 지정된 키를 사용한다. 

```py
kubectl create configmap my-config --from-file=key1=/path/to/bar/file1.txt --from-file=key2=/path/to/bar/file2.txt
```

- my-config 로 이름된 새로운 config map를 생성하며 key1=config1, key2=config2 로 생성된다. 

```py
kubectl create configmap my-config --from-literal=key1=config1 --from-literal=key2=config2
```

- my-config 로 이름된 새로운 config map를 생성하며 파일에서 key=value 로 생성한다. 

```py
kubectl create configmap my-config --from-file=path/to/bar
```

- my-config 로 이름된 새로운 config map를 생성하며 이는 env 파일로 부터 생성한다. 

```py
kubectl create configmap my-config --from-env-file=path/to/foo.env --from-env-file=path/to/bar.env
```

