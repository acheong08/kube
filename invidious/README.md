```
kubectl create configmap init-script \
  --namespace=invidious \
  --from-file=config/init-invidious-db.sh \
  --dry-run=client \
  -o yaml > init-script-configmap.yaml
```
```
kubectl create configmap sql-config \
  --namespace=invidious \
  --from-file=config/sql \
  --dry-run=client \
  -o yaml > sql-configmap.yaml
```
```
kubectl create configmap invidious-companion-config \
  --namespace=invidious \
  --from-file=config/companion.yaml
```
