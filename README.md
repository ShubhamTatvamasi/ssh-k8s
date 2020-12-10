# ssh-k8s

deploy ssh pod
```bash
kubectl run openssh --image=linuxserver/openssh-server \
  --port=2222 --expose --env=USER_NAME=root \
  --env=PUBLIC_KEY="$(cat ~/.ssh/id_ed25519.pub)"

kubectl patch svc openssh \
  --patch='{"spec": {"type": "NodePort"}}'

kubectl patch svc openssh \
  --patch='{"spec": {"ports": [{"nodePort": 32222, "port": 2222}]}}'
```

ssh to pod
```bash
NODE_NAME=$(kubectl get pod openssh -o jsonpath='{.spec.nodeName}')

NODE_IP=$(kubectl get nodes ${NODE_NAME} \
  -o jsonpath='{.status.addresses[?(@.type=="ExternalIP")].address}')

ssh linuxserver.io@${NODE_IP} -p 32222

ssh linuxserver.io@k8s.shubhamtatvamasi.com -p 32222
```

delete deployment
```bash
kubectl delete pod/openssh service/openssh
```

