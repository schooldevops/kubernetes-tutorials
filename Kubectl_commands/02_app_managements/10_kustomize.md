# Kustomize

- KRM 리소스의 셋을 구성할때 kubstomization.yaml 파일을 이용한다. 
- DIR 아규먼트는 반드시 kustomization.yaml 이 포함된 디렉토리이어야 하며, git repository URL은 리포지토리 루트와 관련하여 경로 접미사가 동일하게 지정되어야 한다. 
- DIR을 생략하면 '.' 이라고 가정한다. 

## Usage

```py
$ kubectl kustomize DIR
```

- 현재 워킹 디렉토리를 구성한다. 

```py
kubectl kustomize
```

- 몇몇 공유된 설정 디렉토리를 구성한다. 

```py
kubectl kustomize /home/config/production
```

- github 으로 구성한다. 

```py
kubectl kustomize https://github.com/kubernetes-sigs/kustomize.git/examples/helloWorld?ref=v1.0.6
```

