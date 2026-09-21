# Set up dynamic DNS updates for deployments

Initial approach to application deployment is to use the cluster IPs for traefik, as they are set my servicelb. 

## 1. Manually point kube wildcard domains to loadbalancer IPs
```
*.kube.katralis.net.  A  NODE1.IP
*.kube.katralis.net.  A  NODE2.IP
*.kube.katralis.net.  A  NODE3.IP
```
This means that any application deployed in the `.kube` subdomain will be automatically available for testing. Then full deployments can include the correct annotation to update the main DNS server.
## 2. Configure RFC2136/TSIG in Technitium
From the main dashboard of the DNS server, navigate to Settings -> TSIG:
- Click Add
- Give the key a name: `kubernetes-external-dns`
- Leave shared secret empty
- Click Save Settings
Copy the generated shared secret in the .env file, source it and then run:
```bash
kubectl create namespace external-dns

kubectl -n external-dns create secret generic rfc2136-keys \
  --from-literal=rfc2136-tsig-keyname='kubernetes-external-dns' \
  --from-literal=rfc2136-tsig-secret=$TECHNITIUM_SECRET
```
## 3. Add External-DNS helm repo
```
helm repo add external-dns \
  https://kubernetes-sigs.github.io/external-dns/

helm repo update
```

```bash
kubectl -n external-dns get secret rfc2136-keys   -o jsonpath='{.data.rfc2136-tsig-secret}'   | base64 --decode
kubectl -n external-dns logs   deployment/external-dns -f
helm upgrade external-dns   external-dns/external-dns   -n external-dns   -f external-dns-values.yaml
```
