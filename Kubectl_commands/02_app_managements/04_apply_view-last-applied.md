# apply view-last-applied

- 유형/이름 또는 파일별로 최신 마지막으로 적용된 구성 주석을 본다. 

- 기본 출력은 YAML 형식의 stdout 에 인쇄한다. -o 옵션을 사용하여 출력 형식을 변경할 수 있다. 

## Usage

```py
$ kubectl apply view-last-applied (TYPE [NAME | -l label] | TYPE/NAME | -f FILENAME)
```

- YAML 에서 유형/이름별로 마지막으로 적용된 구성 주석 보기 

```py
kubectl apply view-last-applied deployment/nginx
```

- JSON에서 파일별 last-applied-configuration 주석 보기 

```py
kubectl apply view-last-applied -f deploy.yaml -o json
```

