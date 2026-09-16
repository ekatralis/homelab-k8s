# Testing Traefik
We can test traefik using the whoami web application, which we can deploy using:
```bash
kubectl apply -f whoami.yaml
```
This will deploy an application at `whoami.katralis.net`, which we can test using:
```bash
curl whoami.katralis.net
```
