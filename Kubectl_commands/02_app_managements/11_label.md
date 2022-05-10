# label

- 리소스에서 label을 변경한다. 
  - 레이블 키와 값은 문자나 숫자로 시작해야 하며, 문자, 숫자, 하이픈, 점, 밑줄을 각각 최대 63자까지 포함할 수 있다. 
  - 선택적으로 키는 example.com/my-app 과 같이 DNS 하위 도메인 접두사와 단일 '/'로 시작할 수 있다. 
  - --overwrite 가 true이면 기존 레이블을 덮어쓸 수 있습니다. 그렇지 않으면 레이블을 덮어쓰려고 하면 오류가 발생한다. 
  - --resource-version 이 지정되면 업데이트에서 이 리소스 버젼을 사용하고, 그렇지 않으면 기존 리소스 버젼을 사용한다. 

##  Usage

```py
$ kubectl label [--overwrite] (-f FILENAME | TYPE NAME) KEY_1=VAL_1 ... KEY_N=VAL_N [--resource-version=version]
```

- 'foo' pod를 업데이트하며 레이블 unhealthy 의 값으로 true를 설정한다. 

```py
kubectl label pods foo unhealthy=true
```

- 'foo' pod를 업데이트 하며 레이블 status의 값을 unhealthy 로 설정한다. 기존에 존재하는 값을 오버라이팅 한다. 

```py
kubectl label --overwrite pods foo status=unhealthy
```

- namespace 에서 모든 pod를 업데이트한다. 

```py
kubectl label pods --all status=unhealthy
```

- pod.json 에서 type과 name에 의해서 식별된 pod를 업데이트한다. 

```py
kubectl label -f pod.json status=unhealthy
```

- 리소스가 버젼 1에서 변경되지 않은 경우에만 포드 'foo' 업데이트

```py
kubectl label pods foo status=unhealthy --resource-version=1
```

- 존재하는 경우 'bar' 라는 레이블을 제거하여 'foo' 업데이트한다. 
- --overwrite 플래그가 필요하지 않다. 

```py
kubectl label pods foo bar-
```