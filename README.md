# ssh-k8s

deploy ssh pod
```bash
kubectl run openssh --image=linuxserver/openssh-server --restart=Never --port=2222 --expose

kubectl patch svc openssh \
  --patch='{"spec": {"type": "NodePort"}}'

kubectl patch svc openssh \
  --patch='{"spec": {"ports": [{"nodePort": 32222, "port": 2222}]}}'
```
