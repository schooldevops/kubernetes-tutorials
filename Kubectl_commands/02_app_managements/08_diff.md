# diff 

- Diff 설정은 파일이름에 의해서 지정되거나 stdin으로 지정된다. 
- 현재 온라인 설정과 적용되는 경우 구성사이의 차이를 구분한다. 

- 출력은 항상 YAML 이다. 

- KUBECTL_EXTERNAL_DIFF 환경 변수를 사용하여 고유한 diff 명령을 선택할 수 있다. 
- 사용자는 매개변수와 함께 외부 명령도 사용할 수 있다. (KUBECTL_EXTERNAL_DIFF="colordiff -N -u")

- 기본적으로 diff 커맨드는 "-u"를 이용하여 (unified diff) 실행된다. 
- -N 은 옵션이다. 

- 종료 상태: 0이면 차이가 발견되지 않았다. 
- 종료 상태: 1이면 차이가 있다는 것이다. 
- 에러: Kubectl 혹은 diff 가 실패되고 에러가 발생한경우 

## Usage

```py
$ kubectl diff -f FILENAME
```

- pod.json 에서 포함된 리소스 차이를 낸다. 

```py
kubectl diff -f pod.json
```

- stdin에서 파일 읽기의 차이를 본다. 

```py
cat service.yaml | kubectl diff -f -
```