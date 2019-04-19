Steps to deploy a k8s Cluster

 * [optional]
 Restore the master.key of the sealedSecret if existing See: [FAQ](https://github.com/bitnami-labs/sealed-secrets#faq) 
 ```
 kubectl replace -f master.key
 ```
 * [optional]
Restore the secret key for flux stored in secret/flux-git-deploy if existing
 ```
kubectl replace -f flux-git-deploy.yaml
 ```
 * Install Tiller
 * Install Flux
 ```
# Tune Flux
--registry-poll-interval=1m
--git-poll-interval=1m
--sync-garbage-collection
# Tune flux-helm-operator
--git-poll-interval=1m-operator
--charts-sync-interval=1m
 ```
 * [optional]
if you didn't apply step 2 get the flux ssh-key
 ```
fluxctl identity --k8s-fwd-ns flux
 ```
And put it in the repo with read/write access
 * [optional]
if you don't already have the sealed-secret key
 ```
kubectl -n kube-system port-forward deployment/sealed-secrets-controller 8080:8080
curl localhost:8080/v1/cert.pem > cert.pem
 ```
use version v0.5.1 of kubeseal cli since later version are broken
