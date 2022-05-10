# kubectl create secret

- 지정된 subcommand를 이용하여 secret을 생성한다. 

```py
$ kubectl create secret
```

# kubectl create docker-registry

- Docker registries 를 이용하여 시크릿을 생성한다. 
- Dockercfg 시크릿들은 Docker registries에 대해서 인증하는데 사용된다. 
- Docker 커맨드 라린을 이용할때 이미지를 푸시할때 지정된 레지스트리에 인증할 수 있다. 

```py
'$ docker login DOCKER_REGISTRY_SERVER --username=DOCKER_USER --password=DOCKER_PASSWORD --email=DOCKER_EMAIL'.
```

- 이후 'docker push', 'docker pull' 명령에서 레지스트리를 인증하는 데 사용되는 ~/.dockercfg 파일이 생성된다. 
- 이 메일 주소는 선택사항이다. 

- 애플리케이션을 생성할 때 인증이 필요한 Docker 레지스트리가 있을 수 있다. 
- 노드가 사용자를 대신하여 이미지를 가져오려면 자격 증명이 있어야한다. dockercfg 암호를 만들고 서비스 게정에 연결하여 이 정보를 제공할 수 있다. 

```py
$ kubectl create docker-registry NAME --docker-username=user --docker-password=password --docker-email=email [--docker-server=string] [--from-file=[key=]source] [--dry-run=server|client|none]
```

- .dockercfg 파일이 아직 없으면 다음을 사용하여 dockercfg 암호를 직접 만들 수 있다. 

```py
kubectl create secret docker-registry my-secret --docker-server=DOCKER_REGISTRY_SERVER --docker-username=DOCKER_USER --docker-password=DOCKER_PASSWORD --docker-email=DOCKER_EMAIL
```

- 새로운 시크릿을 생성하며 my-secret 를 ~/.docker/config.json 으로 생성된다. 

```py
kubectl create secret docker-registry my-secret --from-file=.dockerconfigjson=path/to/.docker/config.json
```

# kubectl secret generic 

- 파일에서, 디렉토리에서 혹은 지정된 리터럴 값으로 시크릿을 생성한다. 
- 하나 혹은 여러 키/값 쌍으로 패키지된 단일 시크릿을 생성한다. 
- 파일을 기반으로 비밀을 만들 때 키는 파일의 기본 이름으로 기본 설정되고 값은 기본적으로 파일 콘텐츠로 설정된다. 
- 기본 이름이 유효하지 않은 키이거나 직접 선택하는 경우 대체 키를 지정할 수 있다. 

- 디렉토리를 기반으로 비밀을 만들 때 기본 이름이 디렉터리의 유효한 키인 각 파일은 비밀키로 패키징 된다. 
- 일반 파일을 제외한 모든 디렉토리 항목은 무시된다. (예: 하위 디렉토리, 심볼릭 링크, 장치, 파이프 등)

```py
$ kubectl create generic NAME [--type=string] [--from-file=[key=]source] [--from-literal=key1=value1] [--dry-run=server|client|none]
```

- my-secret 으로 이름된 새로운 시크릿을 생성하고 폴더 바에 있는 각 파일에 대한 키와 함께 생성이 된다. 

```py
kubectl create secret generic my-secret --from-file=path/to/bar
```

- my-secret으로 이름된 새로운 시크릿을 생성하고, 이는 디스크에 있는 이름 대신에 지정된 키로 생성한다. 

```py
kubectl create secret generic my-secret --from-file=ssh-privatekey=path/to/id_rsa --from-file=ssh-publickey=path/to/id_rsa.pub
```

- 새로운 시크릿으로 my-secret 으로 생성한다. key1=supersecret과 key2=topsecret 으로 생성한다. 

```py
kubectl create secret generic my-secret --from-literal=key1=supersecret --from-literal=key2=topsecret
```

- my-secret으로 시크릿을 생성한다. 파일과 리터럴을 이용하여 생성한다 

```py
kubectl create secret generic my-secret --from-file=ssh-privatekey=path/to/id_rsa --from-literal=passphrase=topsecret
```

- my-secret 으로 이름된 새로운 시크릿을 생성한다. 그리고 env파일을 이용하여 생성한다. 

```py
kubectl create secret generic my-secret --from-env-file=path/to/foo.env --from-env-file=path/to/bar.env
```

# secret tls

- 주어진 public/private 키 쌍에서 TLS 시크릿을 생성한다. 

- public/private 키 쌍은 반드시 사전에 존재해야한다. public key 의 인증은 .pem 으로 인코드 되어야하고, private key로 매치된다. 


```py
$ kubectl create tls NAME --cert=path/to/cert/file --key=path/to/key/file [--dry-run=server|client|none]
```

- tls-secret으로 이름된 TLS 시크릿을 주어진 키 쌍으로 생성한다. 

```py
kubectl create secret tls tls-secret --cert=path/to/tls.cert --key=path/to/tls.key
```

