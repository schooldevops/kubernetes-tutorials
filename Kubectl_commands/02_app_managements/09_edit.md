# edit

- 기본 편집기에서 리소스를 편집한다. 

- edit 명령을 사용하면 명령줄 도구를 통해 검색할 수 있는 모든 API 리소스를 직접 편집할 수 있다. 
- KUBE_EDITOR 또는 EDITOR 환경 변수로 정의된 편집기를 열거나 Linux의 경우 vi 또는 window의 경우 notepad로 대체한다. 
- 변경 사항은 한 번에 하나씩 적용되지만 여러 개체를 편집할 수 있다. 

- 명령은 파일 이름과 명령줄 인수를 허용하지만 사용자가 가리키는 파일은 이전에 저장된 리소스 버젼이어야한다. 

- 편집은 리소스를 가져오는 데 사용되는 API 버젼으로 수행된다. 특정 API 버젼을 사용하여 편집하려면 리소스, 버젼 및 그룹을 정규화 하라. 
- 수정을 위해서 특정 API 버젼, fully-qualify 리소스, 버젼, 그룹 등을 지정한다. 

- 기본 포맷은 YAML 이다. 
- JSON 에서 수정을 하기 위해서는 -o json을 이용한다. 

- 다음 플래그 --windows-line-endings 를 이용하여 Windows 줄 끝을 강제 실행할 수 있다. 
- 그렇지 않으면 운영 체제의 기본값이 사용된다. 

- 업데이트 하는 동안 오류가 발생하면 적용되지 않은 변경 사항이 포함된 임시 파일이 디스크에 생성된다. 
- 업데이트를 수행할때 가장 공통적인 에러는 서버에서 리소스의 변경을 수행하는 다른 편집기이다. 
- 이경우 변경사항을 새로운 버젼에 적용하거나 최신 리소스 버젼을 포함하도록 임시 저장된 복사본을 업데이트 해야한다. 

## Usage

```py
$ kubectl edit (RESOURCE/NAME | -f FILENAME)
```

- 서비스 이름 'docker-registry' 인 서비스를 수정한다.

```py
kubectl edit svc/docker-registry
```

- 대체 에디터를 이용한다. 

```py
KUBE_EDITOR="nano" kubectl edit svc/docker-registry
```

- v1 API 포맷을 이용하여 JSON에서 'myjob' 인 잡을 수정한다. 

```py
kubectl edit job.v1.batch/myjob -o json
```

- YAML 에서 배포 'mydeployment'를 편집하고 수정된 구성을 해당 주석에 저장한다. 

```py
kubectl edit deployment/mydeployment -o yaml --save-config
```