# demo-cluster-system-gitops

## instructions
after deploying the cluster:
- create `acr-auth` dockerconfig secret in `flux-system` amespace for image automation to authenticate to container registry and read tags from there
```
kubectl create secret docker-registry acr-auth --docker-server=magnifikacr.azurecr.io --docker-username=magnifikacr --docker-password=<provide-password> --docker-email=arturferfecki@outlook.com
```
- create the same `acr-auth` dockerconfig secret in each app namespace to authenticate to container registry
- create A record in your `dns zone` with nginx public address and ingress subdomains
- login to argocd UI with username: `admin` password: 
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```
- in argocd go to `setting` -> `repositories` and create connection to the repository you want to sync. 
For example URL: `https://github.com/theartusz/demo-cluster-gitops-apps.git` and your github token 
