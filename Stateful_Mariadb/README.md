# Local 에서 MariaDB Stateful 컨테이너 실행하기 

- Local에서 데이터베이스를 Container로 실행하는 경우가 자주 발생한다. 
- DataBase 는 pod가 재실행 되더라도 항상 고정된 볼륨으로 데이터를 저장하고 있어야한다. 이때 주로 사용하는 것이 StatefulSet 이다. 
- 우리는 여기서 StatefulSet을 생성하고, MariaDB를 로컬 환경에서 실행해 볼 것이다. 

## Namespace 생성하기

- 우리는 storage라는 namespace를 생성하고, mariadb를 실행할 것이다. 
- namespace definition 은 다음과 같다. 

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: storage
```

## StorageClass 생성하기

- 저장해야할 스토리지 클래스를 생성한다. 
- 스토리지 클래스의 목적은 동적으로 스토리지를 프로비져닝 하기 위해 사용되는 kubernetes resource 객체이다.
- 일반적으로 시스템 운영자가 Storage Class를 생성하면, 이후 개발자가 pod를 생성할때 PersisentVolumeClaim 을 통해서 볼륨을 필요에 따라 생성할 수 있게 된다. 

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: my-storage
  namespace: storage
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
```

- apiVersion: api는 storage.k8s.io/v1 이다.
- kind: 리소스 종류는 StorageClass라는 것을 알 수 있다. 
- provisioner: 프로비져너는 리소스를 생성한는 주체를 이야기한다. 여기서는 로컬을 이용할 것이므로 프로비저너는 no-provisioner 로 지정했다. 
  - kubernetes.io/gce-pd: GCEPersistentDisk 플러그인을 이용하여 볼륨 생성 
  - kubernetes.io/aws-ebs: AWS ElasticBlockStorage를 이용하여 볼륨 생성 
  - kubernetes.io/azure-disk: Azure File을 이용하여 볼륨 생성 
  - kubernetes.io/azure-disk: Azure Disk를 이용 
- volumeBindingMode: 볼륨 바인딩 모드는 WaitForFirstConsumer로 처음 pod 생성시 볼륨을 바인딩 하게 된다. 모드는 아래와 같다.
  - Immediate Mode: 이 모드는 PVC가 생성되면 StorageClass가 있는 PVC에 대한 PV의 자동 볼륨 바인딩이 포함된다.
  - WaitForFirstConsumer Mode: 이 모드는 사용할 Pod가 생성될 때까지 PV바인딩 및 동적 프로비저닝을 지연한다. 

## PersistentVolume Claim 생성하기

- Persistent Volume Claim은 볼륨을 pod에서 요청할때 볼륨을 요청한 만큼 할당해 주는 역할을 한다. 
- 우리는 참고로 우리는 StatefulSet을 이용하여 mysql을 이용하기 때문에 persistent volume claim 을 바로 생성하지 않고, StatefulSet에서 volumeClaimTemplates 을 이용하여 생성할 것이다. 
- 일반 케이스에서는 직접 생성한다. 

## PersistentVolume 생성하기

- 이제 PersistentVolume 를 생성할 차례이다. 
- 볼륨은 사용하는 네임스페이스등의 다양한 컨테이너용 볼륨을 크게 하나 생성하고  Persistent Volume Claim 으로 필요한 만큼 할당해 나가는 방법을 사용한다. 

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-statefulset-mysql-0
  namespace: storage
spec:
  storageClassName: "my-storage"
  capacity:
    storage: 2Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Delete
  hostPath:
    path: /Users/1111489/Documents/01.STUDY/KUBE/local-storage
    type: DirectoryOrCreate
  nodeAffinity:
    required:
      nodeSelectorTerms:
      - matchExpressions:
        - key: "kubernetes.io/hostname"
          operator: "In"
          values:
          - GLW4XPMQ2X
```

- apiVersion: PersistentVolume의 api 버젼은 v1이다.
- metadata.name: StatefulSet에서는 볼륨을 직접 바인딩해주기 위해서 특정 이름 규칙을 이용하여 바인딩한다. 그러므로 위 이름 규칙은 다음과 같은 형태로 잡았다. 
  - <pv>.<statefulset>.<pod-name> 의 형태에 따라 볼륨 이름을 지정해 주었다. 
- spec.storageClassName: 이전에 생성한 스토리지 클래스 이름을 지정했다. 
- spec.storageClassName.capacity.storage: 스토리지 용량을 2Gi로 설정했다. 
- spec.volumeMode: Filesystem으로 지정했다. 이는 pod의 디렉토리에 마운트 한다.
  - Filesystem: 기본모드이다. 파드의 디렉터리에 마운트 된다. 
  - Block: 파일 시스템이 없는 블록 장치로 파드에 제공할때 유용하다. 
- spec.accessModes: 파일에 접근하기 위한 모드를 지정한다. 
  - ReadWriteOnce(RWO): 하나의 노드에서 해당 볼륨이 읽기/쓰기로 마운트 될 수 있다. 파드가 동일 노드에서 구동되는 경우 복수의 파드에서 볼륨에 접근가능하다. 
  - ReadOnlyMany(ROM): 볼륨이 다수의 노드에서 읽기 전용으로 마운트 될 수 있다. 
  - ReadWriteMany(RWM): 볼륨이 다수의 노드에서 읽기/쓰기로 마운트 될 수 있다. 
  - ReadWriteOncePod(RWOP): 볼륨이 단일 파드에서 읽기/쓰기로 마운트 될 수 있다. 전체 클러스터에서 단 하나의 파드만 해당 PVC를 읽거나 쓸 수 있어야하는 경우 ReadWriteOncePod 접근 모드를 사용한다. 
- spec.persistentVolumeReclaimPolicy: PV와 바인딩된 PVC를 삭제하는경우 파일을 어떻게 처리할지 지정한다.
  - Retain: PVC가 삭제되어 Released 상태가 되어도 실제 내부의 파일은 삭제하지 않는 정책이다. 
  - Delete: PVC가 삭제되어 Released 상태가 되면 PV와 연결된 디스크 내부 자체를 삭제한다. 즉 데이터가 삭제된다. 
  - Recycle: NFS, HostPath 를 사용하는 PV에 적용할 수 있는 방식으로, 볼륨 내부의 파일 전체를 삭제한다. 프로비저닝된 스토리지 자체를 삭제하지 않는다는 점에서 Delete와 다르다. (이 방식은 Deprecated 됬다.)
- spec.hostPath.path: 호스트의 경로를 지정한다. 로컬의 어떤 위치로 저장할지 지정할 수 있다. 
- spec.hostPath.type: DirectoryOrCreate 디렉토리가 없다면 생성한다.
- nodeAffinity: 볼륨을 생성할 노드를 지정한다. 
- nodeAffinity.required: 어피니티 매치가 필요하다.
- nodeAffinity.required.nodeSelectorTerms: 노드 선택할 방식을 지정한다.
- nodeAffinity.required.nodeSelectorTerms.matchExpressions: 노드 선택시 사용할 표현식
- nodeAffinity.required.nodeSelectorTerms.matchExpressions.key: kubernetes.io/hostname 으로 호스트를 지정한다.
- nodeAffinity.required.nodeSelectorTerms.matchExpressions.operator: In, NotIn, Exists 및 DoesNotExist을 지정한다.
- nodeAffinity.required.nodeSelectorTerms.matchExpressions.values: 표현식의 값을 지정한다.

## StatefulSet 지정하기.

- 이제 mariadb를 위한 pod를 생성할 차례이다. 
- 데이터베이스등과 같이 파일이 유지 되어야하는 경우, statefulset을 통해서 pod를 생성한다.
- pod를 재 배포하는 경우라도 동일한 볼륨에 생성이 된다. 


```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
  namespace: storage
spec:
  replicas: 1
  serviceName: mysql
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      terminationGracePeriodSeconds: 10
      containers:
      - name: mysql
        image: mariadb:latest
        ports:
          - name: tpc
            protocol: TCP
            containerPort: 3306
        env:
          - name: MYSQL_ROOT_PASSWORD
            value: password
        volumeMounts:
          - name: mysql-data
            mountPath: /var/lib/mysql
  volumeClaimTemplates:
    - metadata:
        name: mysql-data
        namespace: storage
      spec:
        accessModes:
          - ReadWriteOnce
        resources:
          requests:
            storage: 500Mi
```

- 생성하는 내용은 일반 deployment와 동일하다.
- 한가지 차이점은 volumeClaimTemplates을 이용하여 볼륨 클레임을 생성한다는 것이다. 
- volumeClaimTemplates.metadata: 볼름 클레임의 메타 정보를 지정한다. 여기서는 이름과 네임스페이스를 지정했다. 
- volumeClaimTemplates.spec.accessModes: 일기 쓰기 모드를 지정했다. 
- volumeClaimTemplates.spec.resources.requests.storage: 500Mi로 약 500메가로 잡았다. 

## 실행하기

- 이제 statefulset을 실행해 보자. 

```py
$ kubectl apply -f 00_namespace.yaml

$ kubectl apply -f 20_mysql-menifest.yaml

$ kubectl get all -n storage
NAME          READY   STATUS    RESTARTS   AGE
pod/mysql-0   1/1     Running   0          113m

NAME                     TYPE       CLUSTER-IP    EXTERNAL-IP   PORT(S)          AGE
service/mysql-nodeport   NodePort   10.43.72.47   <none>        3306:32306/TCP   106m

NAME                     READY   AGE
statefulset.apps/mysql   1/1     113m
```

- 정상적으로 수행되고 있음을 파악할 수 있다. 

```py
kubectl get pv,pvc -n storage

NAME                                                        CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM                        STORAGECLASS   REASON   AGE
persistentvolume/pv-statefulset-mysql-0                     2Gi        RWO            Delete           Available                                my-storage              115m
persistentvolume/pvc-193cc960-22c7-4931-94f1-08ed4800ce1b   500Mi      RWO            Delete           Bound       storage/mysql-data-mysql-0   local-path              114m

NAME                                       STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
persistentvolumeclaim/mysql-data-mysql-0   Bound    pvc-193cc960-22c7-4931-94f1-08ed4800ce1b   500Mi      RWO            local-path     115m
```

## WrapUp

- StatefulSet을 이용하여 로컬 환경에 볼륨을 생성하여 pod를 실행해 보았다. 
- StatefulSet의 특징은 PersistentVolumeClaim을 volumeClaimTemplates로 직접 지정한다는 점에서 일반 Deployment와 다르다는 것도 알았다. 
- 서비스는 32306으로 노드 포트를 이용하여 mariaDB에 접근할 수 있다. 



