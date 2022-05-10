# debug

- Debug 클러스터 리소스를 디버깅 컨테이너와 상호작용하도록 이용한다. 

- 'debug' 는 리소스 및 이름으로 식별되는 클러스터 개체애 대한 일반적인 디버깅 작업에 대한 자동화를 제공한다. 
- 리소스를 지정하지 않으면 기본적으로 포트가 사용된다. 

- '디버그'가 수행하는 작업은 지정된 리소스에 따라 다르다. 지원되는 작업은 다음과 같다. 
  - 워크로드: 예를 들어 이미지 태그를 새 버젼으로 변경하는 등 특정 속성이 변경된 기존 포드의 복사본을 생성한다. 
  - 워크로드: 예를 들어 포드를 다시 시작하지 않고 디버깅 유틸리티를 추가하려면 이미 실행중인 포드에 임시 컨테이너를 추가한다. 
  - Node: 노드의 호스트 네임스페이스에서 실행되고 노드의 파일 시스템에 액세스할 수 있는 새 포드를 생성한다. 

```py
$ kubectl debug (POD | TYPE[[.VERSION].GROUP]/NAME) [ -- COMMAND [args...] ]
```

- pod mypod에서 대화형 디버깅 세션을 만들고 즉시 연결한다. (클러스터에서 EphemeralContainers [수명이 짧은 컨테이너] 기능을 활성화 해야한다.)

```py
kubectl debug mypod -it --image=busybox
```

- 사용자 지정 자동화 디버깅 이미지를 사용하여 debugger이라는 디버그 컨테이너를 만든다. (클러스터에 EphemeralContainers 기능을 활성화 해야한다.)

```py
kubectl debug --image=myproj/debug-tools -c debugger mypod
```

- 디버그 컨테이너를 추가하는 mypod의 복사본을 만들고 여기에 첨부한다. 

```py
kubectl debug mypod -it --image=busybox --copy-to=my-debugger
```

- mycontainer 의 명령을 변경하여 mypod 사본 만들기 

```py
kubectl debug mypod -it --copy-to=my-debugger --container=mycontainer -- sh
```

- 모든 컨테이너 이미지를 busybox로 변경하는 mypod 사본을 만든다. 

```py
kubectl debug mypod --copy-to=my-debugger --set-image=*=busybox
```

- 디버그 컨테이너를 추가하고 컨테이너 이미지를 변경하는 mypod 사본 만들기 

```py
kubectl debug mypod -it --copy-to=my-debugger --image=debian --set-image=app=app:debug,sidecar=sidecar:debug
```

- 노드에서 대화형 디버깅 세션을 만들고 즉시 연결한다. 
- 컨테이너는 호스트 네임스페이스에서 실행되고 호스트의 파일 시스템은 /host 에 마운트 된다. 

```py
kubectl debug node/mynode -it --image=busybox
```