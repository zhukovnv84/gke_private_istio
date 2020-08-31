# gke_private_istio
istio with private LB on gke cluster



install gke in private mode 
use currect Master_cidr_ranges
don't use istio as addon. Disable it.


Manual installation Istio with internal LB

```bash

curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.7.0 sh -

export PATH=$PATH:istio-1.7.0/bin/


```


connect to gke cluster...


```bash

istioctl install --set profile=default

kubectl label namespace default istio-injection=enabled

istioctl install -f internal.yaml


```

cat internal.yaml

 ```bash

apiVersion: install.istio.io/v1alpha1
kind: IstioOperator
spec:
  components:
    ingressGateways:
      - namespace: istio-system
        name: ilb-gateway
        enabled: true
        k8s:
          resources:
            requests:
              cpu: 200m
          serviceAnnotations:
            cloud.google.com/load-balancer-type: "internal"
          service:
            type: LoadBalancer
            ports:
            - port: 80
              name: http
              targetPort: 8080
```
