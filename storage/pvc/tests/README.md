```bash
kubectl exec nfs-test-node1 -- \
  sh -c 'echo "hello from node 1" > /data/node1.txt'

kubectl exec nfs-test-node2 -- cat /data/node1.txt
```
