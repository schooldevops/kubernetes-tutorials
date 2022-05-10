# patch

- 전략적 병합 패치, JSON 병합 패치 또는 JSON 패치를 사용하여 리소스의 필드를 업데이트한다. 

- JSON 그리고 YAML 포맷들이 수용된다. 

## Usage

```py
$ kubectl patch (-f FILENAME | TYPE NAME) [-p PATCH|--patch-file FILE]
```

- 전략적 병합 패치를 사용하여 노드를 부분적으로 업데이트하고 패치를 JSON으로 지정한다. 

```py
kubectl patch node k8s-node-1 -p '{"spec":{"unschedulable":true}}'
```

- YAML 로 패치를 지정하여 전략적 병합 패치를 사용하여 노드를 부분적으로 업데이트 한다. 

```py
kubectl patch node k8s-node-1 -p $'spec:\n unschedulable: true'
```

- node.json 에서 지정된 타입과 이름으로 식별된 노드를 부분적으로 업데이트 하며 이는 전략적 병합 패치를 이용한다. 

```py
kubectl patch -f node.json -p '{"spec":{"unschedulable":true}}'
```

- 컨테이너의 이미지를 업데이트 하며, spec.containers[*].name 이 요구되며 이는 병합키이므로 필수이다.

```py
kubectl patch pod valid-pod -p '{"spec":{"containers":[{"name":"kubernetes-serve-hostname","image":"new image"}]}}'
```

- 컨테이너 이미지를 JSON 패치를 이용하여 수정하며 이는 위치 배열을 이용한다. 

```py
kubectl patch pod valid-pod --type='json' -p='[{"op": "replace", "path": "/spec/containers/0/image", "value":"new image"}]'
```