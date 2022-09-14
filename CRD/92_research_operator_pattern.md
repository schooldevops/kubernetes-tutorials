# Operator pattern

from: https://kubernetes.io/docs/concepts/extend-kubernetes/operator/

- 오퍼레티너들은 쿠버니테스의 확장 소프트웨어이다. 이를 통해 사용자 지정 리소스를 사용하여 애플리케이션 및 컴포넌트를 관리한다. 
- 오퍼레이터는 쿠버네티스 원칙을 따른다. 

## 모티베이션

- 오퍼레이터 패턴은 사람(운영자)가 서비스나 서비스 셋을 관리하는에 촛점을 두고 이들을 캡쳐한다. 
- 사람(운영자) 는 특정 어플리케이션을 확인하고, 서비스가 잘못수행되는지에 대한 깊은 지식을 가지고 있고, 어떻게 배포하는지 그리고 이러한 문제에 대해서 어떠한 행위를 할지에 대한 지식이 필요하다. 

- 사람들은 쿠버네티스에 워크로드를 수행하고 종종 자동화를 사용하여 반복 가능한 작업을 처리하는 것을 좋아한다. 
- 오퍼레이터 패턴은 자동화된 코드를 어떻게 작성하는지 캡쳐한다.

## 쿠버네티스 오퍼레이터

- 쿠버네티스는 자동화로 디자인 되어 있다. 
- 쿠버네티스 코어로 부터 내장된 많은 자동화를 획득할 수 있다. 
- 쿠버네티스를 이용하여 자동화된 배포, 수행되는 워크로드 그리고 어떻게 쿠버네티스가 수행하는 방식을 자동화 할수있다.

- 쿠버네티스 오퍼레이터 패턴 개념은 쿠버네티스의 코드 변경없이 클러스터의 행위를 확장할 수 있다. 
- 이는 하나 혹은 여러개의 리소스에 대해서 컨트롤러를 연결한다. 
- 오퍼레이터들은 쿠버네티스 API의 클라이언트이며 커스텀 리소스에 대한 컨트롤러로 동작한다. 

## Example 

- 오퍼레이터를 사용하여 자동화할 수 있는 몇가지 사항은 다음과 같다. 
  - 필요에 따라 어플리케이션 배포
  - 어플리케이션의 상태를 백업하고 획득한다. 
  - 어플리케이션의 업그레이드 처리, 관련된 데이터 스키마 혹은 추가 설정 세팅과 같은 작업에 대한 변경사항 반영
  - 어플리케이션에 대한 서비스 퍼블리싱하여 쿠버네티스API가 지원하지 않는 서비스 제공 
  - 클러스터의 전체 또는 일부에서 장애를 시뮬레이션 하여 복원력 테스트
  - 분산 어플리케이션에 대한 리더 선택, 이는 내부적으로 멤버 선택없이 가능 

- 오퍼레이터는 어떻게 생겼을까? 다음은 그 예이다.
  - 1. 클러스터에 구성할 수 있는 SampleDB라는 사용자 지정 리소스이다. 
  - 2. 오퍼레이터 컨트롤러 부분을 포함하는 Pod 가 실행중인지 확인하는 배포 
  - 3. 오퍼레이터 코드의 컨테이너 이미지
  - 4. 컨트롤러 코드는 어떠한 SampleDB 리소스가 설정되어 있는지 찾기위해 컨트롤 플레인에 쿼리를 수행한다. 
  - 5. 오퍼레이터의 코드는 API 서버에 이야기하여 실제 매치되는지 알려주는 코드이다. 
    - 만약 새로운 SampleDB가 추가되면 오퍼레이터는 PersistentVolumeClaims 를 데이터베이스 스토리지 제공, StatefulSet 및 초기 구성을 처리하기 위한 작업을 설정한다. 
    - 만약 삭제되면 오퍼레이터는 스냅샷을 획득하고, StatefulSet 과 볼륨 역시 삭제된다. 
  - 6. 오퍼레이터 또한 일반적인 데이터베이스 백업을 관리한다. 
    - 각 SampleDB 리소스에 대해서 오러페이터는 언제 pod가 생성하는지 결정하고, 데이터베이스에 접근할수 있는지 백업을 할지 수행한다. 
    - 이러한 pod들은 ConfigMap/Secret 에 따라 수행되며, 데이터베이스에 대한 연결 상세와 크레덴셜을 가진다. 
  - 7. 오퍼레이터는 리소스에 대한 강력한 자동화를 제공하는 것을 목적으로 하기 때문에 추가적인 지원 코드가 있다. 
    - 이 예제에서 코드는 데이터베이스가 오래된 버젼으로 수행되고 있는지 체크한다. 실행중인 경우 이를 업그레이드 하는 Job 객체를 생성한다. 

## 오퍼레이터 배포 

- 오퍼레이터를 배포하는 가장 공통적인 방법은 Custom Resource Definition 을 추가하고, 이를 클러스터의 컨트롤러에 연결하는 것이다. 
- 컨트롤러는 보통 컨테이너화된 애플리케이션을 실행하는 것처럼 컨트롤 플레인의 외부에서 수행된다. 
- 예를 들어 Deployment 로 클러스터에 컨트롤러를 실행할 수 있다. 

## 오퍼레이터 이용

- 오퍼레이터가 배포되고 나면, 추가, 수정, 삭제를 리소스에 수행할때 오퍼레이터를 사용한다. 
- 다음은 오퍼레이터를 배포로 설정하는 예이다. 

```go
kubectl get SampleDB  // 설정된 데이터베이스를 찾는다. 

kubectl edit SampleDB/example-database // 수동으로 세팅을 변경한다. 
```

- 이게 다이다. 오퍼레이터는 변경사항을 적용하고 기존 서비스를 양호한 상태로 유지한다. 

## 자신이 소유한 오퍼레이터 작성하기

- 생태계에 원하는 동작을 구현하는 운영자가 없으면 직접 코딩할 수 있다. 
- 또한 쿠버네티스 API의 클라이언트 역할을 할 수 있는 모든 언어/런타임을 사용하여 운영자(즉, 컨트롤러)를 구현한다. 
- 다음은 고유한 클라우드 네이티브 운영자를 작성하는데 사용할 수 있는 몇가지 라이브러리 및 도구이다. 
  - Charmed Operator Framework
  - Java Operator SDK
  - Kopf (Kubernetes Operator Pythonic Framework)
  - kube-rs (Rust)
  - kubebuilder
  - KubeOps (.NET operator SDK)
  - KUDO (Kubernetes Universal Declarative Operator)
  - Metacontroller along with WebHooks that you implement yourself
  - Operator Framework
  - shell-operator
