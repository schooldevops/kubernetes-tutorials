# kubectl create ingress

- 지정된 이름으로 ingress 를 생성한다. 

```py
$ kubectl create ingress NAME --rule=host/path=service:port[,tls[=secret]]
```

- foo.com/bar 에 대한 요청을 tls 기밀 my-cert가 있는 svc # svc1:8080 으로 보내는 simple 이라는 단일 수신을 만든다. 

```py
kubectl create ingress simple --rule="foo.com/bar=svc1:8080,tls=my-cert"
```

- 서비스 svc:port 및 Ingress 클래스를 otheringress로 가리키는 /path의 catch all ingress 를 생성한다. 

```py
kubectl create ingress catch-all --class=otheringress --rule="/path=svc:port"
```

- 2개의 어노테이션 ingress.annotation1, ingress.annotation2 에 추가된 ingress를 생성한다. 

```py
kubectl create ingress annotated --class=default --rule="foo.com/bar=svc:port" \
--annotation ingress.annotation1=foo \
--annotation ingress.annotation2=bla
```

- 동일한 host 그리고 복수개의 paths로 Ingress 를 생성한다. 

```py
kubectl create ingress multipath --class=default \
--rule="foo.com/=svc:port" \
--rule="foo.com/admin/=svcadmin:portadmin"
```

- 복수개의 hosts 와 Prefix로 pathType 로 된 Ingress를 생성한다. 

```py
kubectl create ingress ingress1 --class=default \
--rule="foo.com/path*=svc:8080" \
--rule="bar.com/admin*=svc2:http"
```

- 기본 Ingress 인증과 다른 path타입들을 이용하여 TLS가 가능하도록 Ingress를 생성한다. 

```py
kubectl create ingress ingtls --class=default \
--rule="foo.com/=svc:https,tls" \
--rule="foo.com/path/subpath*=othersvc:8080"
```

- 지정된 secret과 pathType 를 Prefix로 이용하여 TLS가 가능한 Ingress를 생성한다. 

```py
kubectl create ingress ingsecret --class=default \
--rule="foo.com/*=svc:8080,tls=secret1"
```

- 기본 백엔드로 Ingress 를 생성한다. 

```py
kubectl create ingress ingdefault --class=default \
--default-backend=defaultsvc:http \
--rule="foo.com/*=svc:8080,tls=secret1"
```

