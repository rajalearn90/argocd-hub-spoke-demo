# Argo CD Setup

1) ## Install Argo CD

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

2) ## Run Argo CD in HTTP Mode(Insecure)
  (i) kubectl get cm -n argocd

## argocd-cmd-params-cm is the config map we need to do the changes
 (ii) kubectl edit configmap argocd-cmd-params-cm -n argocd

## In this add a entry
data:
  server.insecure: "false"

# Verify
(kubectl decribe deployement/argocd-server -n argocd - this deployment will use the configmap that we are changing above)
verify using: kubectl edit deploy/argocd-server -n argocd (search /insecure)

REF: https://github.com/argoproj/argo-cd/blob/54f1572d46d8d611018f4854cf2f24a24a3ac088/docs/operator-manual/argocd-cmd-params-cm.yaml#L82

3) ## Expose Argo CD Server Service in NodePort Mode

```
kubectl edit svc argocd-server -n argocd
```

and change the type to NodePort from ClusterIP

4) ## Edit the inbound configuration rules for the security group/firewall rules to enable the inbound request for http (80)/https (443) for the specific ports

5) ## get the initial password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

