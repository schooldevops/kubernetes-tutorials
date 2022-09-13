# Custom Resource Definition

- Custom Resource Definition 은 Kubernetes API를 확장한다. 
- Resources는 Kubernetes API에서 엔드포인트가 되며, API 객체들의 컬렉션을 저장한다. 
  - 예를 들어 내장 Deployment 리소스가 있다. 이는 어플리케이션의 배포에 사용된다.
  - yaml 파일에는 객체를 설명하고, Deployment 리소스 타입을 이용한다. 
- kubectl 을 이용하여 클러스터에서 객체를 생성한다. 
- Custom Resource 는 cluster에 추가하고자 하는 리소스이며, 이는 모든 클러스터에 가능하지는 않다. 
- Kubernetes API의 확장이다. 
- Custom Resource들은 yaml 파일에 정의된다.
- 관리자와 같이 동적으로 CRD를 추가할 수 있으며, 클러스터에 확장 기능을 추가할 수 있다. 
- Operator는 이러한 CRD를 이용하여 Kubernetes API를 확장한다. 

## Resource란

- 리소스는 Kubernetes API내 엔드포인트이며, API 객체들의 컬렉션을 저장한다. 
- apps/v1 은 API Endpoint group 중 하나이다. 
- Deployment는 리소스이다. 

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

## Custom Resource

- 커스텀 리소스는 Kubernetes API의 확장이다. 
- 리소스들은 기본적으로 활성화 되어 있지 않다. 
- 커스텀 리소스는 한번 생성되고 나면 kubectl 을 이용하여 접근할 수 있다. 
- 선언적 api를 제공한다. 

### Definition

- 선언적 명령어로 API Server에 명령을 보낼 수 있다. 
- 이는 YAML 파일로 작성한다. 

### CRD 동작방식

1. 클라이언트 요청수행 kube-apiserver가 받음
2. kube-aggregator 가 받아서 리소스가 어떠한 그룹에 속하는지 그룹 찾기 (ETCD조회)
3. kube resources (pod, services...등 검색) (ETCD 조회) - 빌트인 리소스 검색
4. apiextension-apiserver 검색 (CRD검색) (ETCD조회)
5. 위 과정에서 존재하는 경우 각 리소스를 반환
6. 존재하지 않는경우 404 반환

## Custom Resource Definition (CRD)

- Custom Resource Definition API 리소스는 커스텀 리소스를 정의할 수 있도록 한다. 
  - Custom Resource Definition 은 YAML을 이용하여 정의한다. 
  - yaml을 이용하여 커스텀 리소스를 생성한다. 
- CRD가 생성되면, Custom Controller는 create/update/delete 이벤트를 처리한다 


ref: https://refactorfirst.com/create-kubernetes-custom-resource-definition-crd
