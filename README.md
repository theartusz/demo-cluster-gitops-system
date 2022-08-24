# demo-cluster-system-gitops

## instructions
after deploying the cluster:
1. create `acr-auth` dockerconfig secret in `flux-system` namespace for image automation to authenticate to container registry and read tags from there
```
kubectl create secret docker-registry container-registry-cred -n flux-system --docker-server=magnifikacr.azurecr.io --docker-username=magnifikacr --docker-password=<provide-password> --docker-email=arturferfecki@outlook.com 
```
or 
```
kubectl create secret docker-registry container-registry-cred \
  --docker-server=${AWS_ACCOUNT}.dkr.ecr.${AWS_REGION}.amazonaws.com \
  --docker-username=AWS \
  --docker-password=$(aws ecr get-login-password) \
  --namespace=health-check
```
2. create secret containing github credentials for ImageUpdateAutomation to make commits to repo
```
k create secret generic github-cred --from-literal=username=$TF_VAR_GITHUB_OWNER --from-literal=password=$TF_VAR_GITHUB_TOKEN -n flux-system
```

3. create A record in your `dns zone` with nginx public address and ingress subdomains
4. login to argocd UI with username: `admin` and password: 
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```
or port-forward: 
```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
5. in argocd go to `setting` -> `repositories` and create connection to the repository you want to sync. 
For example URL: `https://github.com/theartusz/demo-cluster-gitops-apps.git` and your github token 
6. create the same `container-registry-cred` dockerconfig secret in each app namespace to authenticate to container registry
```
 kubectl create secret docker-registry container-registry-cred -n angular --docker-server=magnifikacr.azurecr.io --docker-username=magnifikacr --docker-password=<provide-password> --docker-email=arturferfecki@outlook.com
```
