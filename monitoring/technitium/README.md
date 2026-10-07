# Deploying Technitium DNS monitoring dashboard
We create a secret with read-only access in Technitium:
```bash
kubectl -n monitoring create secret generic technitium-exporter \
  --from-literal=token="$TECHNITIUM_TOKEN"
```
We can then deploy the Technitium-Exporter for Prometheus using:
```bash
kubectl apply -f technitium-exporter.yaml
```
We can test using:
```bash
# Check exporter
kubectl -n monitoring port-forward \
  svc/technitium-exporter 9105:9105
curl http://localhost:9105/metrics
curl -s http://localhost:9105/metrics | grep technitium_up
# Check ServiceMonitor status
kubectl get servicemonitor -n monitoring technitium-exporter
kubectl -n monitoring port-forward \
  svc/monitoring-kube-prometheus-prometheus 9090:9090
# Check query in http://localhost:9090
```
Once all checks pass, we can import the Grafana dashboard:
```bash
24555
```
