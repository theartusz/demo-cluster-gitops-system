# demo-cluster-system-gitops

## instructions
after deploying the cluster:
- create `acr-auth` dockerconfig secret in flux-system namespace for image automation to authenticate to container registry and read tags from there
- create the same `acr-auth` dockerconfig secret in each app namespace to authenticate to container registry
- create A record in your `dns zone` with nginx public address and ingress subdomains
- in argoCD go to `setting` -> `repositories` and create connection to the repository you want to sync  
