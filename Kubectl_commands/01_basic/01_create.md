# kubectl create

- 파일이나 표준 입력으로 리소스를 생성한다. 

```py
kubectl create -f FILENAME
```

- pod.json 에 있는 데이터를 이용하여 pod를 생성한다.
  
```py
kubectl create -f ./pod.json
```

- stdin에 전달된 JSON을 기반으로 pod 생성 

```py
cat pod.json | kubectl create -f -
```

- JSON에서 docker-redistry.yaml 에서 데이터를 편집한 다음 이를 이용하여 리소스를 생성한다. 

```py
kubectl create -f docker-registry.yaml --edit -o json
```

