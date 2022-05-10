# annotate 

- 하나 혹은 여러개의 어노테이션을 업데이트한다. 
- 모든 kubernetes 객체는 어노테이션이라는 이름으로 추가적인 데이터를 저장하도록 지원한다. 
- 어노테이션들은 key/value 쌍으로 labels보다 클 수 있고 구조화된 JSON과 같은 임의의 문자열 값을 포함할 수 있는 키/값 쌍이다. 
- 툴과 시스템 확장은 자체 데이터를 저장할 수 있다. 

- --overwrite가 설정되어 있지 않으면 이미 존재하는 주석을 설정하려는 시도가 실패한다. 
- --resource-version 이 지정되고 서버의 현재 리소스 버젼과 일치하지 않으면 명령어가 실패한다. 

- 지원되는 리소스의 전체 목록을 보려면 "kubectl api-resources" 를 사용하라. 

## Usage

```py
$ kubectl annotate [--overwrite] (-f FILENAME | TYPE NAME) KEY_1=VAL_1 ... KEY_N=VAL_N [--resource-version=version]
```

- 'description' 주석과 'my frontend' 값으로 'foo'라는 파드를 업데이트한다. 
- 만약 동일한 이름이 여러번 설정되었다면 오직 마지막 값만 지정된다. 

```py
kubectl annotate pods foo description='my frontend'
```

- "pod.json" 에 타입과 이름에 의해서 정의된 파드를 업데이트한다. 

```py
kubectl annotate -f pod.json description='my frontend'
```

- 'description'주석과 'nginx'를 실행하는 my frontend 값으로 pod 'foo' 를 업데이트하여 기존 값을 덮어쓴다. 

```py
kubectl annotate --overwrite pods foo description='my frontend running nginx'
```

- 네임스페이스내 모든 pod를 업데이트한다. 

```py
kubectl annotate pods --all description='my frontend running nginx'
```

- foo 라는 이름의 pod를 업데이트한다. 이는 오직 version 1 로부터 변경이 되지 않은 리소스인 경우에만 확인된다. 

``py
kubectl annotate pods foo description='my frontend running nginx' --resource-version=1
```

- 존재하는 경우 description 이라는 주석을 제거하여 pod 'foo' 를 업데이트 한다. 
- overwrite 플래그가 필요하지 않는다. 

```py
kubectl annotate pods foo description-
```

