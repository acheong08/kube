```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm upgrade --install -f values.yaml kube-prometheus-stack prometheus-community/kube-prometheus-stack -n monitoring
```
