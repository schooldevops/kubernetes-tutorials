# Kubernetes Operators: what are they? Some examples

from: https://www.cncf.io/blog/2022/06/15/kubernetes-operators-what-are-they-some-examples/

https://developer.redis.com/create/kubernetes/kubernetes-operator/

![k8s-operator](imgs/k8s-operator.webp)

- 쿠버네티스는 유연성과 확장성을 보장하기 위해서 제한된 초기 기능을 제공한다. 
- K8s Operators 는 쿠버네티스 API로 활장된 행위를 수행하기 위한 소프트웨어 확장이다. 
- 어떻게 이들이 수행되는지에 대해 알 필요가 있고, 이들이 제공되는 이점이 무엇인지 알 필요가 있다. 

- 쿠버네티스는 더욱 유용하고 강력하며 유연하게 만드는 에이스가 있다. 
- 에이스는 Operator라고 부른다. 
- 쿠버네티스는 처음부터 소프트웨어 핵심에 직접 구축되는 자동화를 위해 설계되었기 때문에 운영자가 존재한다. 
- 사실 쿠버네티스들은 사용자가 워크로드의 자동으로 배포하고, 실행하도록 한다. 

-  이것이 왜 오퍼레이터들이 강력한지이다.
   - 이를 통해 복잡한 클러스터와 시스템이 일련의 패턴과 원칙에 따라 자동으로 작동할 수 있다. 
   - 즉, 오퍼레이터들은 패턴으로 쿠버네티스 클러스터의 변경없이 클러스터의 행위를 확장한다. 
   - API는 커스텀 리소스 컨트롤러로 동작한다. 

- 오퍼레이터의 목적을 이해하기 위해서 반드시 이해해야하는 것이 있다. 
  - 그리고 어떻게 쿠버네티스가 동작하고, 왜 오퍼레이터가 유용한지 알 아야한다.

## 왜 쿠버네티스 오퍼레이터가 필요한가? 

- 이 아티클을 읽는다면 아마도 이미 쿠버네티스에 대해서 알 것이다. 
- 이제 오퍼레이터가 어떻게 로직에 맞아 들어가는지 알아보자. 

- 쿠버네티스는 오픈소스 소프트웨어로 컨테이너 오케스트레이션을 수행한다. 
- 자동화된 배포, 사이징, 컨테이너상의 워크로드의 관리를 수행한다. 
- 이는 2004년 구글에 의해서 만들어 졌고, 워크로드 오케스트레이션 시스템인 Borg에 의해서 착안한 것이다. 
- 쿠버네티스는 Cloud Native Computing Foundation 으로 도네이트 되었다. 
- CNCF 이는 Linux Foundation 의 부분이다. 이제는 마이크로 서비스나 Radhat 등과 같은 메이저 회사에서 함께 관리/개발되고 있다. 

- 쿠버네티스들은 다음 2가지 기본 원칙이 있다. 
  - 단순하고, 유연하게 관리하는 한편, 다른 한편으로는 가능한한 많은 기능을 자동화 할 수 있는 능력이다. 
  - 이는 확장이 가능하고 매우 다양한 컨텍스트와 애플리케이션에서 사용할 수 있도록 하는 주요 강점이다. 

- 그러나 이러한 동일한 원칙은 초기 기능을 API를 통해 노출되는 제한된 명령 및 작업 집합으로 제한한다. 
- 더욱 복잡한 작업을 수행하기 위해서 이 세트를 확장해야하며 개별 어플리케이션 및 특정 작업 영역에 적합한 보다 정교한 자동화를 만들어야한다. 
- 여기에서 오퍼레이터가 등장한다. 

- 쿠버네티스 오퍼레이터는 어플리케이션 로직을 관리하고, 쿠버네티스 컨트롤 플레인의 부분이다. 
- 이들 컨트롤러들은 클러스터의 실제 상태를 주기적으로 체크하고 원하는 상태인지 검사한다. 
- 두 상태가 서로 멀어질 때 Reconcile 하는 역할을 한다. 
- 단순성, 유연성 및 자동화는 운영자 생성을 가능하게 할 뿐만 아니라 이는 쿠버네티스 아키텍처의 초석을 형성하는 기본원칙이기도 한다. 

## What is a Kubernetes Operator?

- 오퍼레이터 개념은 2016년에 CoreOS Linux 개발팀 (나중에 Cotainer Linux가 됨)에 의해 소개 되었다. 
- 쿠버네티스 프로젝트는 Operator 를 단순하게 정의했다. 
- "Operators 들은 소프트웨어 확장으로 커스텀 리소스를 사용하여 어플리케이션을 관리하고, 이들 컴포넌트를 관리한다."
- 다른말로 Operator를 사용하면 기본 요소(예: Pod, 배포, 서비스 또는 ConfigMap) 모음대신 애플리케이션에 적합한 조정만 노출하는 단일 개체로 애플리케이션을 볼 수 있다. 

- 또한 운영자는 실제로 Kubernetes 내에서 실행되는 소프트웨어에 대해 일반적인 1일차 작업(설치, 구성 등) 및 2일차 작업(재구성, 업그레이드, 백업, 장애 조치, 복구 등)의 자동 구현을 허용합니다. Kubernetes 개념 및 API와 기본적으로 통합됩니다.

- 이것이 "네이티브 Kubernetes 애플리케이션" 이라고 불리는 이유이며, 이 정의는 다른 운영 방식으로도 볼 수 있다. 
- K8S 오퍼레이터는 컨트롤러이며 패키징, 관리, 어플리케이션 배포를 쿠버네티스에서 수행하도록 한다. 
- 이 작업을 수행하기 위해서 운영자는 Custom Resource(CR) 을 이용하여 원하는 설정을 정의하고 Custom Resource Definitions(CRD)를 통해서 특정 어플리케이션의 상태를 정의하는데 사용한다. 

- 오퍼레이터의 역학은 어플리케이션의 원하는 상태와 실제 상태를 조정하는역할을 한다. 이는 컨트롤 루프에서 CRD를 이용하여 자동적으로 어플리케이션을 확장, 수정, 재시동을 수행한다.  
- 실제로 쿠버네티스는 운영자가 더 복잡한 작업을 정의하는 데 사용할 수 있는 기본 명령인 기본 명령을 제공한다. 

- 결과적으로 오퍼레이터는 실제 프로그램으로 클러스터에서 동작한다. 그리고 쿠버네티스 API를 통해 통합되어 더 복잡한 기능을 자동화 한다. 
- 이는 쿠버네티스 자체에 의해서 자연적으로 처리된다. 

## K8S Operators: 목적, 기능

- 이것이 시스템 관리를 위한 자동화된 소프트웨어 처럼 들린다면 그것이 맞다. 
- 우리는 오퍼레이터를 전문가 라고 생각할 수 있다. 
- 우리는 우리가 원하는 것과 어떤 도구로 그것을 보여주고 그 목표를 달성하기 위해 끊임없이 노력한다. 

- 이러한 관점에서 오퍼레이터들은 명령형 도구 대신에 선언적으로 정의한다. 
- 왜냐하면 우리의 롤은 원하는 목적과 리소스를 정의하고, 우리가 원하는 상태가 되도록 시스템이 조정되기 때문이다. 

- 오퍼레이터를 통해서 거의 무한한 수의 작업을 자동화 할 수 있지만, 일부 일반적인 작업은 다른 작업보다 더 일반적이다. 
  - 주문형 어플리케이션 배포 기능
  - 애플리케이션의 상태를 백업하거나 지정된 백업에서 애플리케이션을 다시 시작한다. 
  - 새로운 구성 설정 및 필요한 데이터베이스 변경을 포함하여 모든 종속성이 있는 애플리케이션 업데이트 관리
  - Kubernetes API를 지원하지 않는 애플리케이션에 서비스 노출

- 이들은 k8s 오퍼레이터의 가능한 어플리케이션 일부이다. 
- 오퍼레이터가 제한이 있는가? 이는 프로그래머의 능력과 프로젝트의 요구사항에 달려 있다. 

- 이렇게 넣어보자. API(및 kubectl)를 사용하여 애플리케이션을 관리할 수 있는 Kubernetes와 달리 Oprator는 API의 기능을 확장하고 사용자를 위해 애플리케이션의 복잡한 인스턴스를 관리하는 컨트롤러이다. 
- 한편 쿠버네티스의 자체 리소스를 사용하고 다른 한편으로는 해당 특정 애플리케이션 또는 도메인에 특정한 기술을 사용하여 이를 수정한다. 

- 이 방법으로 오퍼레이터는 관리하는 소프트웨어의 전체 수명 주기를 자동화하는 목적을 수행하며 자동화 없이는 한 명 이상의 작업자가 수행해야 하는 1일차와 2일차에 수행해야 하는 기존 작업을 대신한다. 

## Kubernetes Operator의 이점

- 오퍼레이터는 쿠버네티스 API의 기능을 확장하는 애플리케이션별 컨트롤러이기 때문에 매우 유용하다. 
- 다른말로 Operator는 쿠버네티스에 새로운 기술을 가리킨다. 
- 그러나 이들이 제공하는 구체적인 이점이 무엇인가? 주요 이점을 살펴보자. 

- 1. 오퍼레이터를 통해 상태 비저장 어플리케이션 뿐만 아니라. 상태저장 어플리케이션에도 쿠버네티스 기능을 확장할 수 있다. 
  - 상태 저장 클라우드 애플리케이션과 서비스는 상태 비저장 애플리케이션과 서비스보다 관리하기가 훨씬 더 복잡하기 때문에 이것만으로도 중요한 이점이 있다.
  - 상태 저장 연산자에는 모니터링 솔루션용 Prometheus 연산자와 고가용성 PostgreSQL 데이터베이스 클러스터 관리용 Postgres연산자가 있다. 
- 2. 오퍼레이터는 수동 활동을 표준화 하고, 자동화에 대한 공통적이고 일관된 접근 방식을 만든다. 
- 3. 오퍼레이터는 한 환경에서 다른 환경으로, 한 프로젝트에서 다른 프로젝트로 쉽게 이동할 수 있다. 
  - 이를 통해 사내에서 개발할 필요 없이 다양한 프로젝트에 다운로드, 구성 및 사용할 수 있는 더 많은 일반 오퍼레이터가 있는 생태계의 출현을 가능하게 했다. 
  - 이후 일반 연산자를 찾는 데 가장 인기 있는 허브가 무엇인지 알아볼 것이다. 

- 오퍼레이터를 처음부터 만드는 것이 쉽지 않기 때문에 이 마지막 이점을 과소평가해서는 안된다. 다행이도 몇가지 대안이 있다. 

## 조립식 오퍼레이터와 커스텀 솔루션

- 오퍼레이터를 처음무터 생성하는 것은 매우 복잡하다. 
- 이는 프로그래밍 스킬이 필요하며 (GO가 선호된다 그러나 다른 언어로 클라이언트/서버 커뮤니케이션으로 구현할 수 있다.) 
- 그리고 쿠버네티스 네이티브 컨트롤러의 지식을 통해서 그리고 오퍼레이팅 메커니즘 (reconciliation loops)

- 그러나 다음과 같이 이러한 복잡성을 줄일 수 있는 프레임워크가 있다. 
  - Operator Framework: https://operatorframework.io/
  - Kubebuilder: https://kubebuilder.io/
  - Kubernetes Operators Framework: https://kopf.readthedocs.io/en/stable/

- 운좋게도 우리는 이미 확인했다. 
- 오퍼레이터는 하나의 환경에서 다른 환경으로 변환될 수 있으며, 쉽게 설정할 수 있다. 
- 이러한 사유로 진정한 코티지 경제가 탄생했으며, 이는 오퍼레이터를 통해 Kubernetes에서 애플리케이션의 배포, 관리, 크기조정을 강화하고 단순화하는 것을 목표로한다. 

## Kubernetes Operator: 몇가지 예제

- 현재까지 기존 운영자를 쉽게 검색할 수 있는 두 가지 공식 애플리케이션이 있다. 
  - Artifact HUB: 
    - 오퍼레이터와 helm 차트를 모두 찾을 수 있는곳(CNCF 프로젝트)
    - https://artifacthub.io/packages/search
  - Operator Hub:
    - 오퍼레이터 전용 (Redhat 프로젝트)
    - https://operatorhub.io/
- 오퍼레이터를 통해 매우 다양한 문제에 대한 솔루션을 찾을 수 있다.
- 연산자 목록 대신 해결해야 할 몇 가지 상황부터 시작하여 몇 가지 예를 들어보자. 

### DO YOU WANT TO MONITOR YOUR CLUSTER?

- 쿠버네티스 클러스터 모니터링을 위한 매우 효율적인 엔드투 엔드 솔루션은 kube-prometheus 스택을 기반으로한다. 
- Prometheus Operator를 활용하여 사용자 친화적인 모니터링을 제공하는 문서 및 스크립트와 결합된 Kubernetes 매니페스트, Graphana 대시보드, Prometheus 규칙모음이다. 

### WOULD YOU LIKE TO AUTOMATE TLS CERTIFICATE MANAGEMENT?

- 이경우 클러스터에 구성된 인증 기관 목록에서 발급한 SSL 인증서를 가져올 수 있는 Kubernetes Certmanager Operator 를 사용할 수 있다. 
- 이 솔루션을 사용하면 각 발급자로부터 즉시 새 인증서를 요청하고 서비스 수명 주기에서 자동으로 사용할 수 있다. 

### WOULD YOU LIKE TO AUTOMATE THE MANAGEMENT OF A SERVICE MESH BASED ON ISTIO?

- Istio Operator를 사용하면 Istio 자동으로 설치, 업데이트 및 충돌을 해결을 할 수 있다.
- 이 오퍼레이터는 전제 조건이 거의 없으며 (실제로는 istioctl만 있다.) 모든 API의 설치 및 유효성 검사를 모두 허용한다. 

- DevOps 및 SRE 시나리오 모두에서 사용되는 경우 이 로퍼레이터를 사용하면 관리, 오케스트레이션, 보안, 통신 및 모니터링과 같이 Kubernetes 클러스터에서 사용되는 마이크로서비스의 메시 관리를 처음부터 자동화할 수 있습니다. 
- 그 후, 이 초기 구현은 외상적이지 않은 방식으로 개선될 수 있습니다.

### AUTOMATING THE CREATION OF RESOURCES ON THE PUBLIC CLOUD

- Crossplane은 Crossplane-Provider가 제공하는 CRD 모음을 특정 공급업체에서 제공하는 클라우드 서비스와 매핑하여 Kubernetes 선언적 구문으로 클라우드 리소스를 만들고 관리할 수 있게 해주는 Kubernetes 오퍼레이터이다.
- 코드를 작성할 필요가 없는 애플리케이션 팀에서 사용하도록 설계 되었다. 
- 기본적으로 퍼블릭 클라우드에서 리소스 생성을 자동화하는 것이다. 

- 이 프로젝트는 CNCF의 일부이며 작성자가 설명하는것처럼 클라우드 공급자가 제어 플레인을 사용하여 조직이 자체 클라우드를 생성할 수 있도록 크로스 플레인을 만들었다. 
  
###  HOW TO MANAGE THE ELASTIC CLOUD IN OUR CLUSTER

- Elastic 프로젝트에서 공식적으로 구현한 Elastic Kubernetes Operator를 사용하면 크게 간소화된 배포 구성과 손쉬운 관리를 통해 Kubernetes에서 ElasticSearch 및 Kibana를 자동화 할 수 있다. 
- 오퍼레이터는 더 큰 스토리지를 위한 고정 인덱스와 Kibana Spaces, Canvas및 Elastic Maps와 Kubernetes 인프라의 모니터링 부분을 모두 지원한다. 

## 결론

- 우리가 지금까지 본 연산자는 단지 몇가지 예일 뿐이다. 
- KNative에서 Kubernetes의 Cloud Foundry, Azure Spring Cloud에 이르기까지 다양한 운영자가 있다. 
- 물론 Grafana, Jaeger, ArgoCD, MongoDB, RBAC(역할기반 액세스 제어 모델)와 같이 가장 인기있는 서비스 및 애플리케이션 유형에 대한 오퍼레이터도 있다. 

- 이 모든 경우가 매우 다르지만 항상 동일한 원칙이 적용된다. 
- 오퍼레이터의 장점은 쿠버네티스의 자동화 가능성을 확장하는 것이다. 
- K8s 오퍼레이터의 광범위한 생태계가 있기 때문에 대부분의 용도로 사용할 수 있는 기성품 솔루션을 찾을 수 있다. 

