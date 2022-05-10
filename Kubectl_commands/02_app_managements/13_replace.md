# Replace

- 파일이름 혹은 표준입력에 따라 리소스를 교체한다. 

- JSON과 YAML  포맷들이 허용된다. 
- 만약 존재하는 리소스가 교체되면 반드시 완료된 리소스 스펙이 제공되어야한다. 

## Usage

```py
$ kubectl replace -f FILENAME
```

- pod.json 의 데이터를 이용하여 pod를 교체한다. 

```py
kubectl replace -f ./pod.json
```

- JSON을 stdin에 전달한 값을 기준으로 pod를 교체한다. 

```py
cat pod.json | kubectl replace -f -
```

- 단일 컨테이너 포드의 이미지 버젼(태그)을 v4로 업데이트한다. 

```py
kubectl get pod mypod -o yaml | sed 's/\(image: myimage\):.*$/\1:v4/' | kubectl replace -f -
```

- 강제로 교체하고 삭제한다. 그리고 다시 리소스를 생성한다. 

```py
kubectl replace --force -f ./pod.json
```