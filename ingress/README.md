```
helm install crowdsec crowdsec/crowdsec -f crowdsec-values.yaml -n crowdsec --create-namespace
helm upgrade traefik traefik/traefik -n kube-system -f traefik-values.yaml
```
