# CRD

- CR(Custom Resource) 를 말한다. 
- CRD(Custom Resource Definition) 을 말한다.

## Resource 

- Kubernetes에서 Resource는 Pod, Deployment, ReplicaSet 등을 말한다. 
- kubectl을 이용하여 각 리소스를 조회할 수 있다. 

```go
$ kubectl get pods

No resources found in default namespace.
```

- 위와 같이 현재 기동되는 pod 리소스를 확인할 수 있다. 

```go
$ kubectl get pods --all-namespaces

NAMESPACE     NAME                                      READY   STATUS      RESTARTS       AGE
kube-system   helm-install-traefik-crd--1-9qqcm         0/1     Completed   0              132d
kube-system   helm-install-traefik--1-vppx8             0/1     Completed   2              132d
kube-system   svclb-traefik-vxqvw                       2/2     Running     28 (38h ago)   132d
storage       mariadb-0                                 1/1     Running     12 (38h ago)   130d
kube-system   coredns-96cc4f57d-xd9bz                   1/1     Running     14 (38h ago)   132d
kube-system   traefik-56c4b88c4b-28g7w                  1/1     Running     14 (38h ago)   132d
kube-system   local-path-provisioner-84bb864455-8r4gz   1/1     Running     14 (38h ago)   132d
kube-system   metrics-server-ff9dbcb6c-mks6l            0/1     Running     16 (38h ago)   132d
```

- 위와 같이 모든 네임스페이스에서 pod를 조회하면 위와 같은 결과를 확인할 수 있다. 

### 없는 리소스 조회하기

```go
$ kubectl get HelloWorld

error: the server doesn't have a resource type "HelloWorld"
```

- HelloWorld라는 이름의 리소스는 존재하지 않는다. 
- HelloWorld 커스텀 리소스를 생성한다면 우리는 Kubernetes API 서버로 부터 해당 리소스를 조회할 수 있을 것이지만, 현재는 HelloWorld 리소스는 존재하지 않는다는 것을 알 수 있다. 

### Kubernetes에서 등록된 리소스 목록 조회하기 

```go
$ kubectl api-resources  
NAME                              SHORTNAMES   APIVERSION                             NAMESPACED   KIND
bindings                                       v1                                     true         Binding
componentstatuses                 cs           v1                                     false        ComponentStatus
configmaps                        cm           v1                                     true         ConfigMap
endpoints                         ep           v1                                     true         Endpoints
events                            ev           v1                                     true         Event
limitranges                       limits       v1                                     true         LimitRange
namespaces                        ns           v1                                     false        Namespace
nodes                             no           v1                                     false        Node
persistentvolumeclaims            pvc          v1                                     true         PersistentVolumeClaim
persistentvolumes                 pv           v1                                     false        PersistentVolume
pods                              po           v1                                     true         Pod
podtemplates                                   v1                                     true         PodTemplate
replicationcontrollers            rc           v1                                     true         ReplicationController
resourcequotas                    quota        v1                                     true         ResourceQuota
secrets                                        v1                                     true         Secret
serviceaccounts                   sa           v1                                     true         ServiceAccount
services                          svc          v1                                     true         Service
mutatingwebhookconfigurations                  admissionregistration.k8s.io/v1        false        MutatingWebhookConfiguration
validatingwebhookconfigurations                admissionregistration.k8s.io/v1        false        ValidatingWebhookConfiguration
customresourcedefinitions         crd,crds     apiextensions.k8s.io/v1                false        CustomResourceDefinition
apiservices                                    apiregistration.k8s.io/v1              false        APIService
controllerrevisions                            apps/v1                                true         ControllerRevision
daemonsets                        ds           apps/v1                                true         DaemonSet
deployments                       deploy       apps/v1                                true         Deployment
replicasets                       rs           apps/v1                                true         ReplicaSet
statefulsets                      sts          apps/v1                                true         StatefulSet
tokenreviews                                   authentication.k8s.io/v1               false        TokenReview
localsubjectaccessreviews                      authorization.k8s.io/v1                true         LocalSubjectAccessReview
selfsubjectaccessreviews                       authorization.k8s.io/v1                false        SelfSubjectAccessReview
selfsubjectrulesreviews                        authorization.k8s.io/v1                false        SelfSubjectRulesReview
subjectaccessreviews                           authorization.k8s.io/v1                false        SubjectAccessReview
horizontalpodautoscalers          hpa          autoscaling/v1                         true         HorizontalPodAutoscaler
cronjobs                          cj           batch/v1                               true         CronJob
jobs                                           batch/v1                               true         Job
certificatesigningrequests        csr          certificates.k8s.io/v1                 false        CertificateSigningRequest
leases                                         coordination.k8s.io/v1                 true         Lease
endpointslices                                 discovery.k8s.io/v1                    true         EndpointSlice
events                            ev           events.k8s.io/v1                       true         Event
flowschemas                                    flowcontrol.apiserver.k8s.io/v1beta1   false        FlowSchema
prioritylevelconfigurations                    flowcontrol.apiserver.k8s.io/v1beta1   false        PriorityLevelConfiguration
helmchartconfigs                               helm.cattle.io/v1                      true         HelmChartConfig
helmcharts                                     helm.cattle.io/v1                      true         HelmChart
addons                                         k3s.cattle.io/v1                       true         Addon
ingressclasses                                 networking.k8s.io/v1                   false        IngressClass
ingresses                         ing          networking.k8s.io/v1                   true         Ingress
networkpolicies                   netpol       networking.k8s.io/v1                   true         NetworkPolicy
runtimeclasses                                 node.k8s.io/v1                         false        RuntimeClass
poddisruptionbudgets              pdb          policy/v1                              true         PodDisruptionBudget
podsecuritypolicies               psp          policy/v1beta1                         false        PodSecurityPolicy
clusterrolebindings                            rbac.authorization.k8s.io/v1           false        ClusterRoleBinding
clusterroles                                   rbac.authorization.k8s.io/v1           false        ClusterRole
rolebindings                                   rbac.authorization.k8s.io/v1           true         RoleBinding
roles                                          rbac.authorization.k8s.io/v1           true         Role
priorityclasses                   pc           scheduling.k8s.io/v1                   false        PriorityClass
csidrivers                                     storage.k8s.io/v1                      false        CSIDriver
csinodes                                       storage.k8s.io/v1                      false        CSINode
csistoragecapacities                           storage.k8s.io/v1beta1                 true         CSIStorageCapacity
storageclasses                    sc           storage.k8s.io/v1                      false        StorageClass
volumeattachments                              storage.k8s.io/v1                      false        VolumeAttachment
ingressroutes                                  traefik.containo.us/v1alpha1           true         IngressRoute
ingressroutetcps                               traefik.containo.us/v1alpha1           true         IngressRouteTCP
ingressrouteudps                               traefik.containo.us/v1alpha1           true         IngressRouteUDP
middlewares                                    traefik.containo.us/v1alpha1           true         Middleware
middlewaretcps                                 traefik.containo.us/v1alpha1           true         MiddlewareTCP
serverstransports                              traefik.containo.us/v1alpha1           true         ServersTransport
tlsoptions                                     traefik.containo.us/v1alpha1           true         TLSOption
tlsstores                                      traefik.containo.us/v1alpha1           true         TLSStore
traefikservices                                traefik.containo.us/v1alpha1           true         TraefikService
error: unable to retrieve the complete list of server APIs: metrics.k8s.io/v1beta1: the server is currently unable to handle the request
```

- 위와 같이 api-resources 라는 옵션을 이용하면 kubectl 이 api server에 현재 존재하는 모든 리로스를 반환하라고 요청하는 것이다. 
- Name: 리소스 이름이다 .
- SHORTNAMES: 단축 이름이다. 'kubectl get rs' 라고 하면 리플리카 셋의 단축 이름으로 조회한다는 의미가 된다. 
- APIVERSION: 리소스의 현재 버젼이다. 
- NAMESPACED: 리소스의 스코프가 네임스페이스 단위인지 여부를 확인할 수 있다. true인경우 네임스페이스 내부에서 조회되며, false인경우 전체 범위 조회를 할 수 있다. 
- KIND: 리소스의 실제 설명 혹은 내용을 알 수 있다. 

- 마지막에 나온 error은 api-resources를 조회하면서 문제가 생긴 경우 나타난다. APIs: metrics.k8s.io/v1beta1 은 조회가 안된다는 의미로 해석할 수 있다. 

## CRD (Custom Resource Definition)

- Custom Resource는 쿠버네티스에 등록된 새로운 사용자 정의 리소스라고 했다. 
- 이 사용자 정의 리소스를 추가하고자 한다면 CRD를 이용하여 CR을 정의할 수 있다. 

- helloworld-crd.yaml 파일을 생성하고 다음과 같이 기술하자. 
  
```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: helloworlds.schooldevops.com
spec:
  group: schooldevops.com
  names:
    kind: HelloWorld
    plural: helloworlds
    singular: helloworld
    shortNames:
      - hw
  scope: Namespaced
  versions:
    - name: v1alpha1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                morningHeloWorld:
                  type: string
                noonHelloWorld:
                  type: string
                eveningHelloWorld:
                  type: string
                replicas:
                  type: integer
                  minimum: 1
                  maximum: 10
            status:
              type: object
              properties:
                availableReplicas:
                  type: integer
```

### 커스텀 리소스 정의 사용하기

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
```

- apiVersion: api확장을 이용하기 위해서 버젼을 지정했다. 
- kind: CustomResourceDefinition 으로 사용자 정의 리소스임을 알린다. 

### 메타정보 지정하기

```yaml
metadata:
  name: helloworlds.schooldevops.com
```

- 메타정보를 지정한다.
- 리소스이름.그룹이름 형식으로 나타낸다. 
- 여기서는 helloworlds 리소스 이름, schooldevops.com 으로 그룹 이름을 지정했다. 
- 이름은 반드시 복수형이어야한다. helloworlds 로 복수형이어야한다.

### 스펙 지정하기

```yaml
spec:
  group: schooldevops.com
  names:
    kind: HelloWorld
    plural: helloworlds
    singular: helloworld
    shortNames:
      - hw
  scope: Namespaced
  versions:
    - name: v1alpha1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                morningHeloWorld:
                  type: string
                noonHelloWorld:
                  type: string
                eveningHelloWorld:
                  type: string
                replicas:
                  type: integer
                  minimum: 1
                  maximum: 10
            status:
              type: object
              properties:
                availableReplicas:
                  type: integer
```

- group: 리소스의 그룹을 나타낸다. 대부분 생성자의 조직 정보가 들어간다. 
- names: 리소스 이름을 지정한다. 
- names.kind: 리소스의 종류를 지정한다. 여기서는 HelloWorld로 지정했다. 
- names.plurla: 리소스의 복수 형태를 지정한다. 
- names.singular: 리소스의 단수 형태를 지정한다. 
- names.shortNames: 단축 이름이다. 
- scope: 전역인지, Namespace 단위 리소스인지 알려준다. 
- versions: 버젼 정보를 지정한다. 
- versions.name: 버젼의 이름을 지정한다. 
- versions.schema: 버젼의 상세 스키마를 지정한다. 커스텀 리소스가 가지는 정보를 지정하게 된다. 
- versions.schema.openAPIV3Schema: api 스키마 지정
- versions.schema.openAPIV3Schema.properties: 스키마의 프로퍼티를 지정한다. 하위 객체로 각 프로퍼티이름과 속성을 지정할 수 있다. 
- versions.schema.openAPIV3Schema.properties.status: 리소스의 상태를 나타낸다. 

#### CRD 등록하기

```go
$ kubectl apply -f helloworld-crd.yaml

customresourcedefinition.apiextensions.k8s.io/helloworlds.schooldevops.com created
```

- 커스텀 리소스 정의가 생성 되었다. 

```go
$ kubectl api-resources | grep helloworld

helloworlds                       hw           schooldevops.com/v1alpha1              true         HelloWorld
```

- 위와 같이 우리가 등록한 커스텀 리소스 정의가 등록 되었고, api를 통해서 접근할 수 있음을 나타낸다. 

## Custom Resource 생성하기. 

- 이제 CRD를 정의했으니 리소스를 생성해 보자. 
- helloworld-cr.yaml 파일을 만들고 다음과 같이 작성하자.

```yaml
apiVersion: schooldevops.com/v1alpha1
kind: HelloWorld
metadata:
  name: helloworld-test
spec:
  morningHeloWorld: GoodMorning!!!
  noonHelloWorld: HaveALunch~~
  eveningHelloWorld: ComeBackHome!!!
  replicas: 1
```

- 이전에 작성한 CRD의 규칙대로 스펙을 정의했다. 

```go
$ kubectl apply -f helloworld-cr.yaml 

helloworld.schooldevops.com/helloworld-test created
```

- 생성된 리소스 확인하기 

```go
$ kubectl get hw  

NAME              AGE
helloworld-test   58s
```

- 리소스 상세 보기 

```go
$ kubectl describe hw helloworld-test

Name:         helloworld-test
Namespace:    default
Labels:       <none>
Annotations:  <none>
API Version:  schooldevops.com/v1alpha1
Kind:         HelloWorld
Metadata:
  Creation Timestamp:  2022-09-13T12:51:04Z
  Generation:          1
  Managed Fields:
    API Version:  schooldevops.com/v1alpha1
    Fields Type:  FieldsV1
    fieldsV1:
      f:metadata:
        f:annotations:
          .:
          f:kubectl.kubernetes.io/last-applied-configuration:
      f:spec:
        .:
        f:eveningHelloWorld:
        f:morningHeloWorld:
        f:noonHelloWorld:
        f:replicas:
    Manager:         kubectl-client-side-apply
    Operation:       Update
    Time:            2022-09-13T12:51:04Z
  Resource Version:  239692
  UID:               093a927f-4261-4368-a884-1a9df572c47d
Spec:
  Evening Hello World:  ComeBackHome!!!
  Morning Helo World:   GoodMorning!!!
  Noon Hello World:     HaveALunch~~
  Replicas:             1
Events:                 <none>
```

- 생성한 커스텀 리소스의 정보를 확인할 수 있다. 

## WrapUp

- 지금까지 CustomResource가 무엇이고, CRD를 이용하여 CustomResource를 생성할 수 있음을 알 수 있다. 
- 커스텀 리소스의 정의대로 생성을 했고, 리소스를 직접 확인해 보았다. 
- 그러나 우리는 커스텀 리소스를 생성만 했고, 커스텀 리소스가 실제로 하는 역할은 없다. 
- 또한 커스텀 리소스를 변경해도 모두 쿠버네티스는 리소스 내용만 변경될 뿐 어떠한 일도 일어나지 않는다. 
- 이때 커스텀 리소스를 이용하여 동작하게 만드는 것이 컨트롤러 혹은 오퍼레이터가 그 역할을 수행한다. 나중에 컨트롤러나, 오퍼레이터를 확인할 수 있을 것이다. 

