# Finalizer 개념이해하기 

from: https://kubernetes.io/docs/concepts/overview/working-with-objects/finalizers/

- Finalizer 는 삭제 표시된 리소스를 완전히 삭제하기 전까지, 특정 조건이 충족될 때 까지 대기하도록 Kubernetes에 지시하는 네임스페이스 키이다. 
- Finalizer는 객체가 소유한 리소스를 삭제하기 위해서 컨트롤러에게 알림을 보낸다.

<br/>

- 당신이 쿠버네티스에 객체를 삭제하라고 말하면, 지정된 대상에 대해서 Finalizer 를 가진다. Kubernetes API는 .metadata.deletionTimestamp 를 채워 객체를 삭제 대상으로 표시하고 202 상태 코드를 반환한다. (HTTP 수락됨)
- 타겟 객체는 종료상태로 남아 있게 되며, 이는 컨트롤 플레인이나 혹은 다른 커포넌트들이 finalizer에 의해서 정의된 액션을 수행하는 동안 유지된다. 
- 이러한 작업이 완료되면 컨트롤러는 관련 종료자를 대상 객체로 부터 제거하게 된다. 
- metadata.finalizers 필드가 비어 있다면 쿠버네티스는 삭제가 왼료되었고, 객체를 삭제하게 된다. 

<br/>

- Finalizer를 사용하여 리소스의 가비지 컬렉션을 제어 할 수 있다. 
- 예를 들어 finalizer가 관련된 리소스를 정리하도록 정의할 수 있고 혹은 컨트롤러가 대상 리소스를 삭제하기 전에 인프라 스트럭쳐를 정리할 수도 있다. 

<br/>

- 대상 리소스를 삭제하기 전에 특정 클린업 태스크를 수행하도록 컨트롤러에 경고하여 리소스의 가비지 컬렉션을 제어할 수 있다. 

<br/>

- Finalizer는 일반적으로 실행 코드를 지정하지 않는다. 대신에 이들은 일반적으로 주석과 유사한 리소스의 키 목록이다. 쿠버네티스는 일부 finalizer를 자동적으로 지정하지만, 직접 지정할수도 있다. 

## 어떻게 동작하는가? 

- manifest 파일을 이용하여 리소스를 생성할대, metadata.finalizers 필드를 이용하여 finalizer를 지정할 수 있다. 
- 리소스를 삭제하고자 할경우 API 서버가 finalizer 필드에 값을 확인하고, 다음을 수행한다. 
  - 삭제를 시작한 시간과 함께 metadata.deletionTimestamp 필드를 추가하도록 객체를 수정한다. 
  - metadata.finalizers 필드가 비어 있는 동안 객체가 삭제 되는것을 막는다.
  - 202 상태 코드를 반환한다. (HTTP 수락됨)

- Finalizer를 관리하는 컨트롤러는 객체 삭제가 요청되었음을 나타내는 metadata.deletionTimestamp 설정 개체에 대한 업데이트를 확인한다. 
- 그런 다음 컨트롤러는 해당 리소스에 대해 지정된 종료자의 요구 사항을 충족하려고 시도한다. 
- Finalizer 조건이 충족될 때마다 컨트롤러는 리소스의 종료자 필드에서 해당 키를 제거한다. 
- Finalizer 필드가 비어 있으면 삭제 타임스탬프 필드가 설정된 객체가 자동으로 삭제된다. 
- Finalizer를 사용하여 관리되지 않는 리소스의 삭제를 방지할 수도 있다. 

<br/>

- Finalizer의 공통적인 예제는 kubernetes.io/pv-protection 이다. 이는 PersistentVolume 객체의 우발적 삭제를 방지한다. 
- PersistentVolume 객체가 Pod에서 사용중인 경우 Kubernetes는 PV-Protection 종료자를 추가한다. 
- 만약 PersistentVolume 를 삭제하고자 하면, Terminating 상태로 들어간다. 그러나 finalizer 종료가 되지 않았으므로 바로 삭제가 되지 않는다. 
- Pod가 PersistentVolume 사용을 중지하는 경우 Kubernetes 는 pv-protection finalizer를 종료하고, 컨트롤러가 볼륨을 삭제하게 된다. 

## Owner references, labels, finalizers

- 레이블과 마찬가지로 소유자 참조는 kubernetes 에서 객체들 사이에 관계를 설명하지만, 다른 용도로 사용된다. 
- 컨트롤러가 pod와 같은 객체를 관리할 때, 이는 관련 객체의 그룹의 변경을 탐색하기 위해서 레이블을 이용한다. 
- 예를 들어 Job은 하나 혹은 여러개의 Pod를 생성한다. 
- Job 컨트롤러는 레이블을 해당 pods에 추가하고, 동일한 레이블로 클러스테어세 어떤 Pods가 변경되었는지 검사하게 된다. 

<br/>

- Job 컨트롤러는 또한 owner referneces를 Pods에 추가한다. 이는 Pod를 생성한 Job을 가리킨다. 
- 만약 이들 Pod가 실행되는 동안 job을 삭제하면, 쿠버네티스는 owner reference를 이용하여 클러스터에서 어떠한 pod가 정리가 필요한지 결정하게 된다. 

<br/>

- 쿠버네티스는 또한 삭제 대상 리소스에 대한 소유자 참조를 식별할 때 종료자를 처리한다. 

- 몇몇 상태에서 finalizer는 의존객체의 삭제가 블록된다. 이는 대상의 owner 객체가 완전히 삭제되지 않고 예상보다 오래 유지될 수 있다. 
- 이 상황에서 finalizer를 체크하고, 대상의 owner referneces를 확인하고, 원인에 대한 트러블 슈팅을 해야한다. 

<br/>

- 노트:
  - 객체가 삭제 상태에 있는 경우 삭제를 계속할 수 있도록 finalizer를 수동으로 제거하면 안된다. 
  - Finalizer는 일반적으로 목적이 있어서 리소스에 추가된다. 
  - 그러므로 강제로 제거하게 되면 클러스터 상에 이슈를 만들 수 있다. 
  - 이는 Finalizer의 목적을 이해하고 다른 방식으로 수행할 때만 수행해야한다. (예: 일부 종속 객체를 수동으로 정리)

ref: https://www.howtogeek.com/devops/what-are-finalizers-in-kubernetes-how-to-handle-object-deletions/

