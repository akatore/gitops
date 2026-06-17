# Exposing app,

argocd app create solar-systemapp-2 \
--repo https://github.com/akatore/gitops \
--path ./solar-system --dest-namespace solar-system \
--dest-server https://kubernetes.default.svc  \
--revision gitops-argocd

argocd app list

argocd app sync argocd/solar-systemapp-2

k get svc -n solar-system

```
abhijeetkatore007@cloudshell:~/cloudshell_open/diveintokubernetes$ k get svc -n solar-system
NAME                   TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
solar-system-service   NodePort   10.109.124.169   <none>        80:31703/TCP   12m
```
kubectl port-forward svc/solar-system-service -n solar-system 8081:80

<img width="948" height="93" alt="image" src="https://github.com/user-attachments/assets/c2867f44-bc8f-4efa-aec7-949b37308f23" />

<img width="432" height="182" alt="image" src="https://github.com/user-attachments/assets/289ef944-7dc6-4460-94aa-aa079e2beeba" />

<img width="790" height="416" alt="image" src="https://github.com/user-attachments/assets/dde9ba89-d334-4ac6-b10e-ed8b35067cc4" />

<img width="951" height="607" alt="image" src="https://github.com/user-attachments/assets/cda605f1-0d28-473a-a240-229e09983367" />
