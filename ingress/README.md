```
helm install crowdsec crowdsec/crowdsec -f crowdsec-values.yaml -n crowdsec --create-namespace
helm upgrade traefik traefik/traefik -n kube-system -f traefik-values.yaml
```
```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.9/config/manifests/metallb-native.yaml
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.17.0/cert-manager.yaml
```
