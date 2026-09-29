# Homelab Monitoring Stack
## Initial setup
We create a monitoring namespace:
```bash
kubectl create namespace monitoring
```
We add the helm repo:
```bash
helm repo add prometheus-community \
  https://prometheus-community.github.io/helm-charts

helm repo update
```
We can set up an admin username/password for grafana:
```bash
read -s GRAFANA_PASSWORD

kubectl -n monitoring create secret generic grafana-admin \
  --from-literal=admin-user='admin' \
  --from-literal=admin-password="$GRAFANA_PASSWORD"

unset GRAFANA_PASSWORD
```
Then we can install the helm chart using:
```bash
helm upgrade --install monitoring \
  prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --values kube-prometheus-stack/values.yaml
```
